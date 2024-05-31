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

## Technique d'injection et explication

**Technique de base avec l'absence de sécurité**
Les deux techniques suivantent fonctionne quand aucune sécurité a été mise en place.

```
- url/image?filename=/etc/passwd
- url/image?filename=../../../../../../etc/passwd
```

**Néttoyage fait par l'application**
Certaines applications nettoient les requêtes des utilisateurs en retirant automatiquement les ../ ou modifier l'écriture

Pour contourner cette restriction, un attaquant peut inclure un doublon dans le chemin.

```
url/image?filename=....//etc/passwd
url/image?filename=....\/etc/passwd
url/image?filename=..;/..;/etc/passwd
url/image?filename=....//....//....//etc/passwd
```

**Serveur qui enlève les séquances**
Dans certaines requêtes web, comme les chemins d'URL ou les paramètres de fichiers des requêtes multipart/form-data, les serveurs web peuvent automatiquement enlever les séquences qui permettent de naviguer vers des répertoires supérieurs

```
url/image?filename=%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd
```

**Fichier commence par le répertoire de base**
Certaines applications web imposent que les noms de fichiers fournis par les utilisateurs commencent par un répertoire de base spécifique. Cela a pour but d'empêcher les utilisateurs d'accéder à des fichiers en dehors de ce répertoire

Pour contourner cette restriction, un attaquant peut inclure le répertoire de base attendu dans le nom de fichier, suivi de séquences de traversée de répertoire pour remonter dans l'arborescence des répertoires et accéder aux fichiers souhaités

```
url/image?filename=var/www/images/../../../etc/passwd
```

**Nul byte**
Octet nul : En informatique, un octet nul est un caractère de contrôle dont la valeur est zéro. En codage URL, il est représenté par %00. Dans de nombreux langages de programmation et systèmes de fichiers, un octet nul est interprété comme une fin de chaîne de caractères.

Certaines applications web imposent que les noms de fichiers fournis par les utilisateurs se terminent par une extension de fichier spécifique, par exemple .png. Pour contourner cette restriction, les attaquants peuvent utiliser un octet nul pour tronquer le chemin de fichier avant l'extension requise.

exemple :

```
url/image?filename=../../../etc/passwd%00.png
```

## Comment prévenir une attaque de traversée de chemin

La méthode la plus efficace pour prévenir les vulnérabilités de traversée de chemin est d'éviter de passer des entrées fournies par les utilisateurs aux API du système de fichiers. De nombreuses fonctions d'application qui font cela peuvent être réécrites pour offrir le même comportement de manière plus sécurisée.

Si vous ne pouvez pas éviter de passer des entrées utilisateur aux API du système de fichiers, nous recommandons d'utiliser deux couches de défense pour prévenir les attaques :

1. **Valider l'entrée utilisateur avant de la traiter :**
   - Idéalement, comparez l'entrée utilisateur avec une liste blanche de valeurs autorisées. Cela permet de s'assurer que seules des valeurs approuvées peuvent être utilisées.
   - Si cela n'est pas possible, vérifiez que l'entrée contient uniquement des contenus autorisés, tels que des caractères alphanumériques uniquement. Cela limite les possibilités d'injection de séquences malveillantes.

2. **Utiliser des API de système de fichiers pour canoniser le chemin :**
    - Après avoir validé l'entrée fournie, ajoutez l'entrée au répertoire de base et utilisez une API de système de fichiers de la plateforme pour canoniser le chemin. La canonisation résout les segments du chemin comme . et .., produisant un chemin absolu standardisé.
    - Vérifiez que le chemin canonisé commence par le répertoire de base attendu. Cela garantit que le chemin reste dans les limites des répertoires autorisés et n'accède pas à des fichiers non souhaités en dehors de ces répertoires.

### Exemple de validation

- **Validation de l'entrée :**
  - Supposons que l'application autorise uniquement des noms de fichiers alphanumériques. Une entrée comme ../../etc/passwd serait rejetée car elle contient des caractères non autorisés (../).
- **Canonisation du chemin :**
  - Si l'utilisateur entre image.jpg, le chemin complet pourrait être /var/www/images/image.jpg.
  - Utilisez une API pour canoniser ce chemin. La vérification garantirait que le chemin commence par /var/www/images et non par un autre répertoire, empêchant ainsi une traversée de répertoire.

### Conclusion

En validant strictement les entrées utilisateur et en utilisant des API de système de fichiers pour canoniser et vérifier les chemins, les applications peuvent réduire significativement le risque de vulnérabilités de traversée de chemin. Ces mesures de défense en profondeur offrent une sécurité renforcée contre les tentatives d'accès non autorisé aux fichiers système.

## Référence web

- [OWASP Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal)
- [PortSwigger - Directory Traversal](https://portswigger.net/web-security/file-path-traversal)
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Directory%20Traversal/README.md)
- [Wordlists](https://github.com/carlospolop/Auto_Wordlists/blob/main/wordlists/file_inclusion_linux.txt)
