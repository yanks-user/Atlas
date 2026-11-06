# Installation et mises à jour

[← Retour à la page TACNET](../README.md#télécharger)

## Installateur ou portable ?

Choisissez l'une ou l'autre version : elles ne se mélangent pas.

- **Installateur** : lancez-le. Sa première page propose le dossier d'installation (`C:\Users\<votre nom>\AppData\Local\Programs\TACNET` par défaut, bouton *Parcourir* pour en choisir un autre, hors `C:\Program Files`).
- **Version portable** : décompressez `TACNET_APP.zip`, puis lancez le .exe.

Où sont vos réglages (adresse du serveur, touches, audio, thème) :

- **Avec l'installateur** : dans `%APPDATA%\TACNET`. Ils survivent aux mises à jour, qui gardent aussi le dossier choisi.
- **Version portable** : à côté du .exe.

Changer de version :

- **Déplacer TACNET** : relancez l'installateur et choisissez un autre dossier. L'ancien emplacement est retiré, vos réglages sont conservés.
- **En venant de la version portable** : ressaisissez l'adresse du serveur et vos touches une fois. Le mot de passe du serveur, vos pseudos réservés et vos sessions sauvegardées sont repris.
- **Désinstaller** : *Paramètres Windows > Applications > TACNET*.

## Mises à jour

TACNET regarde au démarrage (puis toutes les 6 heures) si une version plus récente est publiée sur la page des [Releases](https://github.com/yanks-user/TACNET/releases/latest), et l'indique par un bandeau.

- **Version portable** : *Télécharger* ouvre la page de la version, il suffit ensuite de remplacer l'ancien .exe par le nouveau.
- **Avec l'installateur** : le bouton devient *Mettre à jour*. TACNET télécharge la version, puis *Redémarrer et installer* la met en place et rouvre TACNET.
- **Votre accord** : rien n'est téléchargé ni installé sans votre clic. Seule la version de l'application est transmise (GitHub voit votre adresse IP, comme tout site que vous visitez).
- **Désactiver** : *Paramètres > À propos*.
- **Serveur** : il fait de même dans son journal (`UPDATE_CHECK=off` pour le couper). Si le serveur d'un groupe n'est pas à jour, les joueurs le voient et préviennent celui qui l'héberge.
- **Version 1.0.0** : elle n'a pas cette fonction, elle se remplace une fois à la main.
