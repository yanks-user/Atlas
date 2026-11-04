<p align="center"><img src="docs/logo.png" alt="Logo de TACNET" width="96"></p>

# TACNET

**Communication vocale par sessions : parlez en groupe, canal par canal, comme avec une radio.**

TACNET est une application Windows sans installation. Une personne crée une session et partage un code de 4 caractères ; les autres la rejoignent et choisissent leur canal. Le serveur ne fait que mettre les joueurs en relation : la voix circule directement entre eux.

![Vue d'un joueur dans TACNET](docs/vue-joueur.png)

## Ce que permet TACNET

- **Sessions et canaux** : un code à 4 caractères, des canaux créés par le gérant, des règles d'écoute (« ce canal entend celui-là »).
- **Parler comme à la radio** : Push To Talk vers son canal (Int), vers les canaux reliés (Ext), vers toute la session (Général) ou vers un seul canal. Les touches se règlent et fonctionnent même quand une autre fenêtre, un jeu par exemple, est au premier plan.
- **Rôles** : gérant, adjoint, joueur, auditeur, et des rôles personnalisés réglés option par option.
- **Mode Half-Duplex** : un seul émetteur à la fois vers un canal.
- **Canaux masqués** : chaque joueur ne voit que son propre canal, idéal pour garder le secret des équipes.
- **Léger** : Windows 64 bits, un seul fichier .exe, sans installation.

## Télécharger

Tout se trouve dans la page [**Releases**](https://github.com/yanks-user/TACNET/releases/latest). Version actuelle : **1.0.0**.

| Fichier | Pour qui | Contenu |
|---|---|---|
| `TACNET_APP.zip` | **Tout le monde** | L'application (`TACNET-1.0.0-portable.exe`), la licence et un guide en français. |
| `TACNET_SERV.zip` | Les plus avancés | Un serveur maison (Docker) avec son guide : domaine, HTTPS, gestion des pseudos. |

Décompressez le zip puis lancez le .exe. L'application n'est pas signée : si Windows affiche « Windows a protégé votre PC », cliquez sur *Informations complémentaires*, puis *Exécuter quand même*.

## Il faut un serveur

TACNET a besoin d'un serveur pour mettre les joueurs en relation. **La méthode conseillée est de le créer gratuitement sur [Render](https://render.com)**, sans machine à laisser allumée et sans compte GitHub :

1. Sur Render : **New +**, **Web Service**, **Public Git Repository**, puis collez `https://github.com/yanks-user/TACNET-SERVER-RENDER`.
2. Réglez *Language* sur **Docker**, laissez *Root Directory* et *Dockerfile Path* vides, et choisissez le plan **Free**.
3. Ajoutez la variable d'environnement `SERVER_PASSWORD` (le mot de passe de votre groupe, 8 caractères minimum), puis lancez le déploiement.
4. Donnez aux joueurs l'adresse `wss://nom-du-service.onrender.com` et le mot de passe.

Le pas-à-pas détaillé est dans le guide (`README.html`) de `TACNET_APP.zip`. Le dépôt [TACNET-SERVER-RENDER](https://github.com/yanks-user/TACNET-SERVER-RENDER) contient ce que Render déploie.

> **Plan gratuit de Render :** le serveur s'endort après 15 minutes sans activité (le réveil prend environ une minute) et ses données, pseudos réservés et sessions gardées, sont effacées à chaque redémarrage. Pour un serveur permanent, choisissez un plan payant de Render, ou le serveur maison de `TACNET_SERV.zip`.

## Captures

| Canaux masqués : ce que voit un joueur | Paramètres de la session : ce que règle le gérant |
|---|---|
| ![Vue limitée à son canal](docs/canaux-masques.png) | ![Paramètres de la session](docs/parametres-session.png) |

## Licence

Tous droits réservés, sauf ce que permet [LICENCE.txt](LICENCE.txt) : utilisation gratuite, partage de copies non modifiées avec leur licence, mais ni vente ni modification. Les composants tiers gardent leurs propres licences, réunies dans le dossier `licenses` des zips.
