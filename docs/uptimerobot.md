# Garder le serveur éveillé avec UptimeRobot (facultatif)

[← Retour à la page Atlas](../README.md#render)

Sur le plan gratuit de Render, le serveur s'endort après 15 minutes sans joueur, et la connexion suivante attend environ une minute qu'il se réveille. Sans cette option, rien ne change. Avec elle, [UptimeRobot](https://uptimerobot.com), un service en ligne gratuit, appelle le serveur toutes les 5 minutes : il ne s'endort plus, et les joueurs se connectent tout de suite.

1. Sur [uptimerobot.com](https://uptimerobot.com), créez un compte gratuit.
2. Ajoutez un moniteur (**New monitor**) de type **HTTP(s)**.
3. Saisissez l'adresse de votre service Render, en `https` : `https://nom-du-service.onrender.com`, et un intervalle de **5 minutes**.
4. Enregistrez.

## À savoir

- UptimeRobot peut afficher le serveur comme « Down » (erreur 426) : c'est normal, le serveur ne répond qu'à l'application. L'appel suffit à le garder éveillé. Coupez les alertes de ce moniteur pour ne pas recevoir d'e-mails.
- Render offre 750 heures gratuites par mois d'après Render : de quoi garder **un** serveur éveillé tout le mois, pas deux.
- Éveillé, le serveur garde ses données plus longtemps, mais son disque reste effacé à chaque redémarrage ou mise à jour : [Upstash](upstash.md) les garde.
- UptimeRobot est un service tiers : il ne voit que l'adresse du serveur et s'il répond. Jamais les pseudos, les sessions, le mot de passe ni la voix.
