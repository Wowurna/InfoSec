# Path Traversal

## Qu'est-ce que le Path Traversal ?

Le Path Traversal (ou traversée de chemin) est une vulnérabilité de sécurité qui permet à un attaquant de lire des fichiers arbitraires sur le serveur en manipulant les entrées pour naviguer en dehors du répertoire racine de l'application web. Cela peut inclure des fichiers système, des fichiers de configuration, ou des fichiers contenant des données sensibles.

## Impact potentiel

- Exposition des fichiers système critiques : Accès à des fichiers comme /etc/passwd qui peuvent contenir des informations sur les utilisateurs du système.
- Accès aux fichiers de configuration sensibles : Par exemple, des fichiers de configuration contenant des informations sur les bases de données, les clés API, etc.
- Révélation des données sensibles : Informations confidentielles des utilisateurs, données internes de l'entreprise.
- Possibilité d'attaque ultérieure : Informations obtenues peuvent être utilisées pour d'autres attaques plus sophistiquées.

## Niveau de Risque

Le niveau de risque peut être évalué en fonction de l'impact et de la probabilité d'exploitation :

**Faible**

- Scénario : L'accès est limité à des fichiers non sensibles et l'application est bien isolée.
- Impact : Faible impact sur la sécurité et l'intégrité des données.
- Probabilité : Faible, en raison de bonnes pratiques de sécurité en place.

**Moyen**

- Scénario : L'application permet l'accès à certains fichiers sensibles, mais les données critiques ne sont pas compromises directement.
- Impact : Informations utiles pour d'autres attaques, mais pas de compromission majeure directe.
- Probabilité : Moyenne, si des points d'entrée vulnérables existent.

**Élevé**

- Scénario : Accès à des fichiers critiques contenant des données sensibles ou des configurations de sécurité.
- Impact : Compromission potentielle du système, accès à des mots de passe, clés API, ou informations utilisateur.
- Probabilité : Élevée, surtout si les mesures de sécurité sont insuffisantes

## Exemple de rédaction dans un rapport

### Vulnérabilité : Path Traversal

**Description :**
Une vulnérabilité de Path Traversal a été découverte permettant aux attaquants d'accéder à des fichiers en dehors du répertoire prévu en manipulant les entrées utilisateur.

**Impact Potentiel :**
- Exposition de fichiers système critiques (par ex. `/etc/passwd`).
- Accès à des fichiers de configuration sensibles.
- Révélation de données utilisateur confidentielles.
- Utilisation des informations obtenues pour des attaques ultérieures.

**Probabilité d'Exploitation :**
- Facilité d'exploitation : Moyenne à Élevée
- Prévalence : Courante dans les applications mal sécurisées.

**Niveau de Risque : Élevé**
L'accès à des fichiers critiques et des informations sensibles, combiné à la facilité d'exploitation de cette vulnérabilité, constitue un risque élevé pour la sécurité de l'application et du système sous-jacent.

## Référence web

- [OWASP Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal)
- [PortSwigger - Directory Traversal](https://portswigger.net/web-security/file-path-traversal)
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Directory%20Traversal/README.md)
