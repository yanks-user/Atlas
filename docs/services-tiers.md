# Les services tiers

[← Retour à la page Atlas](../README.md#les-services-tiers)

Atlas s'appuie sur quelques services extérieurs.

![Schéma des services tiers d'Atlas : Render. Upstash et UptimeRobot sont facultatifs.

| Service | À quoi il sert | Obligatoire ? | Ce qu'il voit | Coût |
|---|---|---|---|---|
| **Render** | Héberge le serveur Atlas, qui met les joueurs en relation. | Non : Docker ou Node.js conviennent aussi. | Les échanges avec le serveur : Render déchiffre la connexion, il voit donc ce qui s'y échange (pseudos, sessions, et le mot de passe du serveur à la connexion). Jamais la voix. | Plan gratuit : le disque est effacé à chaque redémarrage |
| **Upstash** | Garde les pseudos, les codes de session et les sessions quand le disque de Render est effacé. | Non, facultatif : [voir comment](upstash.md). | Les pseudos, les noms de session et des empreintes de jetons. Jamais le mot de passe du serveur ni la voix. | Plan gratuit |
| **UptimeRobot** | Appelle le serveur Render toutes les 10 minutes : il ne s'endort plus après 15 minutes sans joueur, et répond tout de suite. | Non, facultatif : [voir comment](uptimerobot.md). | L'adresse du serveur, et s'il répond. Jamais les pseudos, les sessions, le mot de passe ni la voix. | Plan gratuit |
| **STUN de Google** | Aide les PC à se trouver pour que la voix passe en direct. | Par défaut. L'adresse se change dans le fichier de configuration. | Votre adresse IP et votre port, quand vous rejoignez une session. Jamais la voix. | Gratuit |
| **Votre propre serveur** | Remplace Render, avec un disque durable : Docker (NAS, mini-serveur, hébergeur) ou Node.js sous Windows. | Non, facultatif. | Rien d'extérieur : c'est vous qui l'hébergez. | À votre charge |

La voix ne passe par aucun de ces services : elle circule directement entre les joueurs. Un relais TURN peut s'ajouter en option pour les réseaux qui bloquent les connexions directes (voir le guide).
