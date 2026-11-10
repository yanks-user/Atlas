# Installation et mises à jour

[← Retour à la page Atlas](../README.md#télécharger)

## Installateur ou portable ?

Choisissez l'une ou l'autre version : elles ne se mélangent pas.

- **Installateur** : lancez-le. Sa première page propose le dossier d'installation (`C:\Users\<votre nom>\AppData\Local\Programs\Atlas` par défaut, bouton *Parcourir* pour en choisir un autre, hors `C:\Program Files`).
- **Version portable** : décompressez `Atlas_APP.zip`, puis lancez le .exe.

Où sont vos réglages (adresse du serveur, touches, audio, thème) :

- **Avec l'installateur** : dans `%APPDATA%\Atlas`. Ils survivent aux mises à jour, qui gardent aussi le dossier choisi.
- **Version portable** : à côté du .exe.

Changer de version :

- **Déplacer Atlas** : relancez l'installateur et choisissez un autre dossier. L'ancien emplacement est retiré, vos réglages sont conservés.
- **En venant de la version portable** : ressaisissez l'adresse du serveur et vos touches une fois. Le mot de passe du serveur, vos pseudos réservés et vos sessions sauvegardées sont repris.
- **Désinstaller** : *Paramètres Windows > Applications > Atlas*.

## Mises à jour

Atlas regarde au démarrage (puis toutes les 6 heures) si une version plus récente est publiée sur la page des [Releases](https://github.com/yanks-user/Atlas/releases/latest), et l'indique par un bandeau.

- **Version portable** : *Télécharger* ouvre la page de la version, il suffit ensuite de remplacer l'ancien .exe par le nouveau.
- **Avec l'installateur** : le bouton devient *Mettre à jour*. Atlas télécharge la version, puis *Redémarrer et installer* la met en place et rouvre Atlas.
- **Votre accord** : rien n'est téléchargé ni installé sans votre clic. Seule la version de l'application est transmise (GitHub voit votre adresse IP, comme tout site que vous visitez).
- **Désactiver** : *Paramètres > À propos*.
- **Serveur** : il fait de même dans son journal (`UPDATE_CHECK=off` pour le couper). Si le serveur d'un groupe n'est pas à jour, les joueurs le voient et préviennent celui qui l'héberge.
- **Version 1.0.0** : elle n'a pas cette fonction, elle se remplace une fois à la main.
