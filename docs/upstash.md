# Garder les pseudos et les sessions avec Upstash (facultatif)

[← Retour à la page TACNET](../README.md#render)

Sur le plan gratuit de Render, le disque est effacé à chaque redémarrage : les pseudos réservés et les sessions gardées disparaissent. Sans cette option, rien ne change. Avec elle, le serveur copie ses pseudos réservés, ses codes de session et ses sessions gardées chez [Upstash](https://upstash.com), un petit service en ligne gratuit (environ 256 Mo et 500 000 commandes par mois d'après Upstash), et les y retrouve après chaque redémarrage, mise à jour ou mise en veille.

1. Sur [upstash.com](https://upstash.com), créez un compte gratuit, puis une base **Redis**. Choisissez une région d'Europe, par exemple Frankfurt, proche de votre service Render.
2. Sur la page de la base, dans la partie **REST API**, copiez `UPSTASH_REDIS_REST_URL` et `UPSTASH_REDIS_REST_TOKEN`.
3. Dans Render, onglet **Environment** de votre service, ajoutez ces deux variables, avec les mêmes noms, puis enregistrez. Render relance le serveur.
4. Vérifiez : l'onglet **Logs** affiche « Stockage durable Upstash : actif ».

## À savoir

- Le serveur doit être en version 1.3.0 ou plus récente (*Manual Deploy*, puis *Deploy latest commit*).
- Le jeton (`UPSTASH_REDIS_REST_TOKEN`) est un mot de passe : ne le partagez pas.
- Prévoyez une base par serveur, ou un `UPSTASH_KEY_PREFIX` différent pour chacun.
- Seuls les pseudos réservés, les codes de session et les sessions gardées sont conservés. Après un redémarrage, les joueurs connectés se reconnectent.
- Upstash est un service tiers : il reçoit les pseudos, les noms de session et des empreintes de jetons, jamais le mot de passe du serveur ni la voix.
