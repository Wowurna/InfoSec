# XSS
Le Cross-Site Scripting (XSS) est une vulnérabilité de sécurité web courante qui permet à un attaquant d'injecter du code malveillant, généralement sous forme de scripts, dans les pages web vues par d'autres utilisateurs. Ces scripts peuvent être utilisés pour détourner des sessions, voler des cookies, rediriger les utilisateurs vers des sites malveillants, ou effectuer des actions en leur nom sans leur consentement. Le XSS se produit principalement en raison d'une validation et d'un assainissement inadéquats des entrées utilisateur dans les applications web. Cette vulnérabilité peut avoir des conséquences graves sur la confidentialité et la sécurité des utilisateurs, ainsi que sur l'intégrité de l'application.

## Comment fonctionne le xss
Le cross-site scripting fonctionne en manipulant un site web vulnérable afin qu’il renvoie du JavaScript malveillant aux utilisateurs. Lorsque le code malveillant s’exécute dans le navigateur d’une victime, l’attaquant peut compromettre entièrement son interaction avec l’application.

## Trois type d'attaques XSS
- **XSS réfléchi**, où le script malveillant provient de la requête HTTP actuelle.
- **XSS stocké**, où le script malveillant provient de la base de données du site Web.
- **XSS basé sur le DOM,** où la vulnérabilité existe dans le code côté client plutôt que dans le code côté serveur.

## Aide mémoire 
- [PortSwigger](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
