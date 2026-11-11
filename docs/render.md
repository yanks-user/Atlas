# Héberger le serveur sur Render

[← Retour à la page Atlas](../README.md#render)

[Render](https://render.com) construit et lance le serveur Atlas à partir du dépôt public, sans machine à laisser allumée et sans compte GitHub. Render fournit l'adresse et le certificat : il n'y a pas d'empreinte à donner aux joueurs.

## Créer le serveur

1. Sur [render.com](https://render.com), créez un compte gratuit.
2. Cliquez sur **New +**, puis **Web Service**, onglet **Public Git Repository**, et collez :
   ```
   https://github.com/yanks-user/Atlas-Server-Render
   ```
3. Remplissez la page de création :

   | Réglage | Valeur |
   |---|---|
   | **Name** | Le nom de votre choix : il donne l'adresse du serveur (`nom.onrender.com`) |
   | **Language** | **Docker** |
   | **Branch** | `main` |
   | **Region** | Une région proche des joueurs, par exemple Frankfurt |
   | **Instance Type** | **Free** |
   | **Root Directory, Dockerfile Path** | Laissez vides |

4. Dans **Environment Variables**, ajoutez `SERVER_PASSWORD` avec le mot de passe des joueurs (8 caractères minimum). C'est la seule variable à saisir.
5. Cliquez sur **Deploy Web Service**. La construction prend quelques minutes.

## Vérifier

- L'onglet **Logs** affiche « Serveur de sessions en écoute ».
- L'adresse du service, en haut de sa page (`https://nom.onrender.com`), ouverte dans un navigateur, affiche « WebSocket uniquement. » : c'est normal, le serveur ne répond qu'à l'application.

## Donner l'accès aux joueurs

À l'écran de connexion d'Atlas : l'adresse `wss://nom.onrender.com`, **sans numéro de port**, le champ *Empreinte* **vide**, puis le mot de passe choisi à l'étape 4.

## Mettre à jour le serveur

- L'onglet **Logs** affiche « Nouvelle version d'Atlas » quand il en existe une. Les joueurs, eux, sont prévenus dans l'application.
- Cliquez sur **Manual Deploy**, puis **Deploy latest commit**.
- Le redéploiement coupe les joueurs connectés : choisissez un moment entre deux parties.

## Changer le mot de passe

Onglet **Environment** du service : modifiez `SERVER_PASSWORD`, puis enregistrez. Render relance le serveur, et les joueurs saisissent le nouveau mot de passe.

## Le plan gratuit

- Son disque est **effacé à chaque redémarrage** : les pseudos réservés, les codes et les sessions gardées disparaissent. [Upstash](upstash.md) les garde gratuitement.
- Il **s'endort après 15 minutes** sans joueur : la connexion suivante attend environ une minute. [UptimeRobot](uptimerobot.md) peut le garder éveillé.
