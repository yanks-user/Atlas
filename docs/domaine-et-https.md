# Domaine et HTTPS pour votre serveur Atlas

[← Retour à la page Atlas](../README.md#il-faut-un-serveur)

Ce guide est pour celles et ceux qui **hébergent leur propre serveur** Atlas, avec Docker ou avec Node.js sous Windows. Sur Render, le nom de domaine et le HTTPS sont déjà fournis : il n'y a rien à faire.

## À quoi ça sert

Sans domaine, les joueurs saisissent l'adresse de votre machine, un numéro de port et une empreinte à recopier. **Avec un nom de domaine**, ils saisissent une adresse simple, `wss://atlas.example.com`, et **plus d'empreinte** : la connexion est chiffrée avec un vrai certificat.

Pour cela, un petit logiciel, le **reverse proxy**, reçoit leurs connexions et les transmet à Atlas. Ce guide utilise **Caddy**, qui obtient et renouvelle tout seul le certificat HTTPS.

> **Caddy n'est pas le seul choix.** Il est choisi ici parce qu'il est le plus simple, mais Atlas fonctionne avec **n'importe quel reverse proxy** : Nginx, Nginx Proxy Manager, Traefik, HAProxy, Apache, celui de votre NAS ou de votre box… Si vous en avez déjà un, ou si vous préférez un autre, passez directement à [Un autre reverse proxy](#un-autre-reverse-proxy-nginx-haproxy-traefik).

```
Application  →  wss:// chiffré  →  Caddy  →  ws:// local  →  Serveur Atlas
```

## Avant de commencer

Trois choses à avoir en place. Dans tout ce guide, remplacez `atlas.example.com` par votre domaine.

1. **Un nom de domaine**, ou un sous-domaine gratuit (DuckDNS, par exemple).
2. **Ce nom pointe vers votre machine.** Chez le service où vous l'avez obtenu, créez un enregistrement `A` avec l'adresse IP publique de votre box. Attendez quelques minutes.
3. **Les ports 80 et 443 (TCP) arrivent sur la machine** : ouvrez-les, ou redirigez-les depuis votre box. Le 80 sert à obtenir le certificat, le 443 aux joueurs.

## Avec Docker

On ajoute Caddy au fichier du serveur : tout reste dans le même dossier, et une seule commande démarre l'ensemble. Il faut que le serveur Docker de la [page Atlas](../README.md#docker) soit déjà en place.

**1. Placez-vous dans le dossier du serveur**, celui qui contient `docker-compose.yml` :

```
cd atlas-serveur
```

**2. Créez le fichier de Caddy.** Cette commande crée `Caddyfile` et l'ouvre dans l'éditeur nano :

```
nano Caddyfile
```

Collez ces trois lignes, avec votre domaine, puis enregistrez (**Ctrl+O**, **Entrée**) et quittez (**Ctrl+X**) :

```
atlas.example.com {
    reverse_proxy atlas:8080
}
```

`atlas` est le nom du service dans `docker-compose.yml` : ne le changez pas.

**3. Remplacez le fichier du serveur.** Ouvrez-le, effacez son contenu (dans nano, **Ctrl+K** plusieurs fois), collez celui-ci, remettez votre mot de passe (8 caractères minimum), puis enregistrez et quittez comme avant :

```
nano docker-compose.yml
```

```yaml
# Serveur Atlas + Caddy (domaine et HTTPS) : le seul fichier à changer
#
# Pour mettre à jour le serveur proprement (deux commandes, dans le dossier de ce fichier) :
#   docker compose build --no-cache
#   docker compose up -d
# (Pour mettre à jour Caddy aussi, ajoutez « docker compose pull » avant la seconde.)
services:
  atlas:
    # Construit depuis le dépôt public d'Atlas, comme Render
    build: https://github.com/yanks-user/Atlas-SERVER-RENDER.git
    restart: unless-stopped
    init: true
    stop_grace_period: 15s
    # Pas de "ports" : seul Caddy joint Atlas, à l'intérieur de Docker
    expose:
      - "8080"
    environment:
      # Mot de passe demandé aux joueurs, 8 caractères minimum
      SERVER_PASSWORD: "changez-moi-svp"
      # Caddy chiffre et transmet : Atlas n'a plus besoin de son certificat
      TLS: "off"
      TRUST_PROXY: "true"
    volumes:
      - atlas-data:/data
    read_only: true
    tmpfs:
      - /tmp
    cap_drop:
      - ALL
    security_opt:
      - no-new-privileges:true

  caddy:
    image: caddy:2
    restart: unless-stopped
    depends_on:
      - atlas
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile:ro
      # Certificats et réglages de Caddy : à garder entre deux redémarrages
      - caddy-data:/data
      - caddy-config:/config

volumes:
  atlas-data:
  caddy-data:
  caddy-config:
```

Ce qui change : Atlas n'ouvre plus de port vers l'extérieur (seul Caddy est joignable), `TLS` et `TRUST_PROXY` sont activés, et un second service, `caddy`, apparaît.

**4. Démarrez :**

```
docker compose up -d --build
```

Caddy demande son certificat au premier démarrage, ce qui prend quelques secondes.

**5. Vérifiez :**

```
docker compose logs caddy
```

La ligne `certificate obtained successfully` confirme que le certificat est obtenu. Puis, depuis n'importe quel ordinateur :

```
curl -i https://atlas.example.com/
```

La réponse attendue est un code **426** avec `WebSocket uniquement.` : c'est bon signe.

**6. Prévenez les joueurs.** Dans Atlas, **Paramètres > Serveur** : adresse `wss://atlas.example.com`, sans numéro de port, et **empreinte vide**.

> **Docker Desktop (Windows, Mac).** Tous les joueurs peuvent y arriver avec la même adresse : une seule personne qui se trompe 5 fois de mot de passe bloque alors tout le monde pendant une minute. Sur Linux, un NAS ou un hébergeur, ce cas ne se présente pas.

## Avec Node.js sous Windows

Caddy est un seul fichier à lancer à côté du serveur, sans installation.

**1. Téléchargez Caddy.** Sur [caddyserver.com/download](https://caddyserver.com/download), choisissez **Windows** et **amd64**, puis enregistrez le fichier sous le nom `caddy.exe` dans un nouveau dossier, par exemple `C:\caddy`.

**2. Réglez le serveur Atlas.** Ajoutez ces trois lignes dans `config.env` (à côté de `demarrer-serveur.cmd`), puis fermez la fenêtre du serveur et relancez `demarrer-serveur.cmd` :

```
TLS=off
TRUST_PROXY=true
HOST=127.0.0.1
```

`HOST=127.0.0.1` rend le serveur joignable depuis cette machine seulement : Caddy, installé dessus, est le seul à l'atteindre.

**3. Créez le fichier de Caddy.** Dans `C:\caddy`, créez avec le Bloc-notes un fichier nommé exactement `Caddyfile`, sans extension (**Enregistrer sous**, type **Tous les fichiers**), avec votre domaine :

```
atlas.example.com {
    reverse_proxy 127.0.0.1:8080
}
```

**4. Lancez Caddy**, dans PowerShell :

```
cd C:\caddy
.\caddy.exe run
```

Autorisez Caddy dans le pare-feu Windows s'il le demande. **Laissez la fenêtre ouverte pendant les parties**, comme celle du serveur : la fermer arrête Caddy. La ligne `certificate obtained successfully` confirme que le certificat est obtenu.

**5. Vérifiez et prévenez les joueurs.** Ouvrez `https://atlas.example.com` dans un navigateur : la page affiche `WebSocket uniquement.`, c'est bon signe. Les joueurs saisissent dans **Paramètres > Serveur** l'adresse `wss://atlas.example.com`, sans numéro de port, et **l'empreinte vide**.

## Si ça ne marche pas

| Ce que vous voyez | Que faire |
|---|---|
| Le journal de Caddy parle de `timeout` ou d'un certificat impossible à obtenir | Les ports 80 et 443 ne sont pas joignables depuis Internet : vérifiez la redirection de votre box et le pare-feu. Certains fournisseurs bloquent le port 80. |
| Le navigateur ne trouve pas `atlas.example.com` | Le nom ne pointe pas encore vers votre box : vérifiez l'enregistrement `A` et patientez quelques minutes. Si l'adresse IP de votre box change, utilisez un service d'adresse dynamique (DuckDNS, par exemple). |
| `curl` : code 502 ou 503 | Caddy ne joint pas Atlas. Docker : `docker compose ps` doit montrer `atlas` et `caddy` en `Up`, et le Caddyfile doit contenir `atlas:8080`. Windows : la fenêtre du serveur doit être ouverte, et `config.env` contenir `TLS=off`. |
| « Certificat refusé » dans l'application | L'adresse doit commencer par `wss://`, et l'empreinte doit être vide. |

## Un autre reverse proxy (Nginx, HAProxy, Traefik…)

**Caddy est un choix, pas une obligation.** Si vous avez déjà un reverse proxy (Nginx, Nginx Proxy Manager, Traefik, HAProxy, Apache, celui de votre NAS…), ou si vous en préférez un autre, gardez-le : il suffit de régler deux choses.

**1. Le serveur Atlas** doit savoir qu'un proxy chiffre à sa place, et ne doit être joignable que par lui :

| | Réglage |
|---|---|
| Docker | Dans `docker-compose.yml` : `TLS: "off"` et `TRUST_PROXY: "true"`, avec `"127.0.0.1:8080:8080"` à la place de `"8080:8080"` si le proxy est sur la même machine (ou aucun `ports` si le proxy est un conteneur du même fichier, comme Caddy plus haut). |
| Node.js | Dans `config.env` : `TLS=off`, `TRUST_PROXY=true` et `HOST=127.0.0.1`. |

**2. Le proxy** doit faire ces quatre choses :

1. **Chiffrer** côté joueurs, avec un certificat valide pour votre domaine.
2. **Laisser passer le WebSocket** vers le serveur (en-têtes `Upgrade` et `Connection`).
3. **Ajouter l'adresse du joueur** à la fin de l'en-tête `X-Forwarded-For`.
4. **Garder les connexions longues ouvertes** : une session peut durer des heures.

Exemple pour **Nginx**, dans le bloc `server` qui écoute en HTTPS sur votre domaine :

```
location / {
    proxy_pass http://127.0.0.1:8080;

    # Passage de HTTP à WebSocket
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";

    # Vraie adresse du joueur, lue par TRUST_PROXY
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

    # Une session peut durer des heures
    proxy_read_timeout 1h;
    proxy_send_timeout 1h;
}
```

Avec **Nginx Proxy Manager**, créez un *Proxy Host* vers `127.0.0.1` (ou l'adresse de la machine) port `8080`, cochez **Websockets Support** et demandez un certificat **Let's Encrypt** dans l'onglet SSL.

Dans tous les cas, les joueurs saisissent `wss://atlas.example.com` et laissent l'empreinte vide. Le guide du dossier `Atlas_SERV.zip` (page des [Releases](https://github.com/yanks-user/Atlas/releases/latest)) donne des exemples complets pour Nginx et HAProxy, avec le certificat Let's Encrypt.
