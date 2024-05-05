# _(<abbr title="Développeur Web et Web Mobile">DWWM</abbr> 2018)_ <abbr title="Compétence Professionnelle">CP</abbr> 6 - Développer les composants d'accès aux données
> [REAC (03/05/2018), pages 23 et 24](https://www.banque.di.afpa.fr/EspaceEmployeursCandidatsActeurs/EGPResultat.aspx?ct=01280m03&type=t)

## 🚀 Contexte

Maintenant que tu connais la structure de ta base de données et qu’elle est créée, il va falloir expliquer comment ton application pourra accéder aux données stockées.

En PHP, tu connais certainement PDO, mais tu as peut-être également utilisé un ORM comme Eloquent ou encore Doctrine.  
Pour NodeJS, tu as peut-être utilisé les pilotes [mysql](https://www.npmjs.com/package/mysql), [pg](https://www.npmjs.com/package/pg) ou [sqlite](https://www.npmjs.com/package/sqlite) pour accéder à ta base de données ou encore un ORM comme [Prisma](https://www.npmjs.com/package/prisma) ou [Sequelize](https://www.npmjs.com/package/sequelize).

L’idée ici est d’expliquer comment le back va se connecter à la base de données **ET** comment le back est structuré pour accéder aux données.

Tu vas donc avoir très certainement avoir besoin de parler des services et des modèles qui permettent d’accéder aux données et de les altérer.

Comme cette <abbr title="Compétence Professionnelle">CP</abbr> _(et les suivantes)_ parlent de sécurité,
c’est l’occasion de parler de ton fichier .env et du .gitignore afin de ne pas avoir de fichiers sensibles dans le repo de ton projet,
dont les informations de connexion à la base de données.

!!! warning "Utilisation d'ORM/query builder"
    Si tu utilises un ORM ou un query builder, il est important de bien expliquer comment tu l'as configuré et comment tu l'utilises.  
    En plus d'expliquer comment cet outil a été paramétré, tu dois être en mesure d'expliquer la construction SQL générée par cet outil.

    Par exemple avec Prisma :
    ```javascript
    const user = await prisma.user.findUnique({
      select: {
        id: true,
        name: true,
        email: true,
        posts: {
          select: {
            title: true,
          },
        },
      },
      include: {
        posts: true,
      },
      where: {
        id: 1,
      },
    });
    ```

    Ce qui donnera :
    ```sql
    SELECT u.id, u.name, u.email, p.title -- Sélection des colonnes
    FROM "User" u -- Table principale
    INNER JOIN "Post" p ON p.userId = u.id -- Jointure avec la table "Post"
    WHERE u.id = 1; -- Condition de sélection
    ```

    _(Oui, c'est un exemple fictif et pas entièrement correspondant, mais tu as compris l'idée !)_

On est d'accord : cette <abbr title="Compétence Professionnelle">CP</abbr> est un gros morceau, mais c'est un passage obligé pour bien comprendre comment ton application va interagir avec la base de données !

## 📝 Critères d'évaluation
!!! abstract "Critères d'évaluation"
    - Les traitements relatifs aux manipulations des données répondent aux fonctionnalités décrites dans le dossier de conception technique
    - Un test unitaire est associé à chaque composant, avec une double approche fonctionnelle et sécurité
    - Les composants d’accès à la base de données suivent les règles de sécurisation reconnues
    - La démarche de recherche permet de résoudre un problème technique ou de mettre en œuvre une nouvelle fonctionnalité
    - La veille sur les vulnérabilités connues permet d’identifier des failles potentielles
    - La documentation technique liée aux technologies associées, rédigée en langue anglaise, est comprise (sans contre-sens, ...)

## 🤯 Aller plus loin _(hors référentiel)_

### 🛡 Sécurisation des données

J'imagine que dans ton application tu as des données sensibles, comme des informations personnelles ou encore des mots de passe.  
En ce qui concerne les mots de passe, il est essentiel que ces derniers soient stockés de manière sécurisée, c'est-à-dire **hashés**.  
Pour cela, tu peux utiliser des fonctions de hachage comme [bcrypt](https://www.npmjs.com/package/bcrypt) avec NodeJS ou encore [password_hash](https://www.php.net/manual/fr/function.password-hash.php) en PHP.

Pour les informations personnelles, tu as également la possibilité de les sécuriser en les chiffrant !  
Tu peux très bien te baser sur un algorithme maison ou encore utiliser des bibliothèques comme [crypto-js](https://www.npmjs.com/package/crypto-js) en NodeJS ou encore [openssl](https://www.php.net/manual/en/book.openssl.php) en PHP.

### 🗑️ Suppression des données

!!! quote "Un peu de ménage ne fait jamais de mal !"
    On est d'accord ! Cependant, la suppression de données est une opération délicate.  
    Se dire "il suffit de faire un `DELETE FROM table WHERE id = 1`" peut potentiellement poser des problèmes.

En effet, il est important de bien réfléchir à la suppression de données, notamment pour les données liées à d'autres données _(les fameux `ON DELETE CASCADE` par exemple)_.

Puisque l'erreur est humaine, permettre la suppression aussi simplement que via un bouton "Supprimer" peut être risqué, même si ce dernier est protégé par un message de confirmation.

On va préférer d'une suppression logique, c'est-à-dire qu'on va "désactiver" la donnée plutôt que de la supprimer définitivement.  
C'est ce qu'on appelle plus communément le **"soft delete"**, là où la suppression définitive est appelée **"hard delete"**.

Il est également important de maintenir un certain état des données stockées, notamment pour des raisons légales ou encore pour des raisons de traçabilité.  
Bien entendu, lorsque l'utilisateur souhaite supprimer définitivement ses données _(<abbr title="Règlement Général sur la Protection des Données">RGPD</abbr>)_, il est important de bien l'informer des conséquences de cette action et des délais de suppression dans la politique de confidentialité de ton application.

---

## 📚 Documentations
- [Éditions ENI - Merise - Guide pratique (3e édition), par Jean-Luc Baptiste](https://www.editions-eni.fr/livre/merise-guide-pratique-3e-edition-modelisation-des-donnees-et-des-traitements-manipulations-avec-le-langage-sql-9782409015342)

## 🛠 Outils
- [Looping](https://www.looping.fr/)

---

[⬅️ <abbr title="Compétence Professionnelle">CP</abbr> 5 - Créer une base de données](cp-5-creer-une-base-de-donnees.md)  
[➡️ <abbr title="Compétence Professionnelle">CP</abbr> 7 - Développer la partie back-end d'une application web ou web mobile](cp-7-developper-la-partie-back-end-d-une-application-web-ou-web-mobile.md)  
[🏠 Retour à l'accueil du millésime 2018](index.md)