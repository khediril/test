Absolument ! Excellent choix de thème, la "gestion des dons de sang" offre de nombreuses opportunités pour explorer les concepts de Symfony de manière concrète et progressive.

Voici la suite de l'atelier, centrée sur la mise en place d'une base de données et la création de la première entité pour nos donateurs, toujours avec un focus sur la collaboration Git/GitHub et les références à la documentation officielle.

---

# Atelier Guidé Symfony & Git/GitHub : Gestion des Donateurs (Partie 1)

## Contexte du Cours : Développement Web avec Symfony

Nous continuons notre parcours dans le développement web avec Symfony. Dans cet atelier, nous allons poser les bases de notre application de gestion des dons de sang en introduisant le concept de persistance des données. Nous allons créer notre première entité (le Donateur) et apprendre à interagir avec une base de données.

**Application :** Notre application Symfony `mon-app-symfony` va désormais gérer des informations sur les donateurs. Nous allons commencer par définir ce qu'est un donateur et comment stocker ses informations.

## Objectifs de cet Atelier

À la fin de cet atelier, vous serez capables de :
*   Configurer Doctrine ORM (Object-Relational Mapper) pour interagir avec une base de données.
*   Créer une entité Symfony/Doctrine pour modéliser une table de base de données (`Donateur`).
*   Générer et exécuter des migrations de base de données pour mettre à jour le schéma.
*   Travailler de manière collaborative sur la structure de l'application via Git/GitHub.
*   Anticiper et gérer des conflits potentiels liés aux changements de structure.

## Prérequis

*   Avoir terminé l'atelier précédent "Votre Première Application Collaborative".
*   Votre projet `mon-app-symfony` doit être cloné sur les deux machines et les dépendances installées (`composer install`).
*   Le serveur de développement Symfony doit être fonctionnel sur chaque machine (`symfony serve`).
*   Vous êtes familiers avec le flux de travail Git (`checkout`, `pull`, `branch`, `add`, `commit`, `push`, `Pull Request`, `merge`).

## Références Documentation Officielle Symfony

Cet atelier s'appuie directement sur les sections suivantes de la documentation officielle de Symfony :
*   [Doctrine ORM](https://symfony.com/doc/current/doctrine.html)
*   [Configuration de la base de données](https://symfony.com/doc/current/doctrine/database.html)
*   [Création d'entités](https://symfony.com/doc/current/doctrine/entities.html)
*   [Migrations de base de données](https://symfony.com/doc/current/doctrine/migrations.html)
*   [Associations d'entités (pour plus tard)](https://symfony.com/doc/current/doctrine/associations.html)

---

## Partie 0 : Synchronisation Initiale (Chaque Étudiant)

**Chaque étudiant doit réaliser cette section sur sa propre machine.**

1.  **Assurez-vous que votre copie locale du projet est à jour :**
    ```bash
    cd mon-app-symfony
    git checkout main
    git pull origin main
    ```
    *Ceci garantit que les deux étudiants partent de la même base de code suite aux fusions de l'atelier précédent.*
2.  **Lancez votre serveur Symfony** dans un terminal séparé ou en arrière-plan :
    ```bash
    symfony serve
    ```
    *Laissez ce terminal ouvert. Utilisez un nouveau terminal pour les commandes Git et Symfony CLI.*

---

## Partie 1 : Configuration de la Base de Données (Réalisée par l'**Étudiant A**)

L'Étudiant A sera le responsable de la configuration initiale de la base de données et de la première entité.

### Étape 1.1 : Comprendre Doctrine ORM et la Base de Données

Symfony utilise **Doctrine ORM** pour interagir avec les bases de données. Un ORM permet de manipuler des objets PHP (appelés "entités") comme s'ils étaient des lignes dans une base de données, sans écrire de SQL directement.

Par défaut, Symfony utilise SQLite comme base de données légère pour le développement, ce qui est parfait pour cet atelier.

**Référence :** [symfony.com/doc/current/doctrine.html](https://symfony.com/doc/current/doctrine.html) et [symfony.com/doc/current/doctrine/database.html](https://symfony.com/doc/current/doctrine/database.html)

### Étape 1.2 : Configuration de la Connexion à la Base de Données

Le fichier de configuration principal pour Doctrine est `config/packages/doctrine.yaml`. Cependant, la connexion à la base de données est gérée par la variable d'environnement `DATABASE_URL` dans le fichier `.env`.

1.  **Ouvrez le fichier `.env`** dans votre éditeur de code.
2.  Localisez la ligne `DATABASE_URL`. Elle devrait ressembler à ceci :
    ```dotenv
    # DATABASE_URL="mysql://app:!ChangeMe!@127.0.0.1:3306/app?serverVersion=8.0.32&charset=utf8mb4"
    # DATABASE_URL="postgresql://app:!ChangeMe!@127.0.0.1:5432/app?serverVersion=16&charset=utf8"
    DATABASE_URL="sqlite:///%kernel.project_dir%/var/data.db"
    ```
    *Assurez-vous que la ligne commençant par `sqlite` est décommentée (sans `#` au début) et que les autres sont commentées si vous n'utilisez pas MySQL ou PostgreSQL.*
    Cette configuration indique à Symfony d'utiliser une base de données SQLite stockée dans le fichier `var/data.db` à la racine de votre projet. Ce fichier sera créé automatiquement.

3.  **Vérifiez le statut Git** pour voir si `.env` a été modifié :
    ```bash
    git status
    ```
    *Normalement, `.env` est ignoré par Git via le fichier `.gitignore` pour éviter de pousser des informations sensibles (comme les mots de passe de base de données) sur GitHub. C'est une bonne pratique ! Vous ne devriez donc pas voir de changement ici.*

### Étape 1.3 : Créer la Première Entité : Donateur

Nous allons créer une entité `Donateur` avec quelques propriétés de base.

1.  **Créez une nouvelle branche Git** pour cette fonctionnalité :
    ```bash
    git checkout -b feature-donateur-entity
    ```
2.  **Utilisez la console Symfony pour générer l'entité `Donateur` :**
    **Référence :** [symfony.com/doc/current/doctrine/entities.html](https://symfony.com/doc/current/doctrine/entities.html)
    ```bash
    php bin/console make:entity Donateur
    ```
    *La console vous posera des questions sur les propriétés de votre entité. Pour cet atelier, ajoutez les suivantes :*
    *   `nom` (string, longueur 255, nullable: no)
    *   `prenom` (string, longueur 255, nullable: no)
    *   `email` (string, longueur 255, nullable: yes)
    *   `dateNaissance` (datetime_immutable, nullable: yes)
    *   `groupeSanguin` (string, longueur 5, nullable: yes)
    *   `telephone` (string, longueur 20, nullable: yes)

    *Exemple d'interaction dans le terminal :*
    ```
    php bin/console make:entity Donateur

     Your new entity will be stored in src/Entity/Donateur.php.
     You can add fields now, or add them later by running
     `php bin/console make:entity Donateur` again.

     New property name (id field will be created automatically):
     > nom
     Field type (string, text, boolean, integer, float, array, json, datetime, datetimemutable, date, time, decimal, simple_array, json_array, object, binary, blob, guid, tsquery):
     > string
     Field length [255]:
     > 255
     Can this field be null in the database (yes/no) [no]:
     > no

     Add another property? (yes/no) [yes]:
     > prenom
     Field type [string]:
     > string
     Field length [255]:
     > 255
     Can this field be null in the database (yes/no) [no]:
     > no

     Add another property? (yes/no) [yes]:
     > email
     Field type [string]:
     > string
     Field length [255]:
     > 255
     Can this field be null in the database (yes/no) [no]:
     > yes

     Add another property? (yes/no) [yes]:
     > dateNaissance
     Field type [datetime_immutable]:
     > datetime_immutable
     Can this field be null in the database (yes/no) [no]:
     > yes

     Add another property? (yes/no) [yes]:
     > groupeSanguin
     Field type [string]:
     > string
     Field length [5]:
     > 5
     Can this field be null in the database (yes/no) [no]:
     > yes

     Add another property? (yes/no) [yes]:
     > telephone
     Field type [string]:
     > string
     Field length [20]:
     > 20
     Can this field be null in the database (yes/no) [no]:
     > yes

     Add another property? (yes/no) [no]:
     > no

     created: src/Entity/Donateur.php
     created: src/Repository/DonateurRepository.php

    ```
    *Cette commande a créé deux fichiers : `src/Entity/Donateur.php` (qui représente notre donateur et ses propriétés) et `src/Repository/DonateurRepository.php` (qui sera utilisé pour interroger la base de données).*

### Étape 1.4 : Générer et Exécuter une Migration de Base de Données

Les entités définissent la structure de vos données en PHP. Pour que cette structure soit reflétée dans votre base de données (ici `var/data.db`), vous devez créer et exécuter une migration.

1.  **Générez une nouvelle migration :**
    **Référence :** [symfony.com/doc/current/doctrine/migrations.html](https://symfony.com/doc/current/doctrine/migrations.html)
    ```bash
    php bin/console make:migration
    ```
    *Cette commande compare l'état actuel de vos entités PHP avec le schéma de votre base de données et génère un fichier PHP dans `migrations/` qui contient les instructions SQL nécessaires pour mettre à jour la base de données. Vous devriez voir un message comme `Created new migration file to "<date_time>_<migration_name>.php"`.*
2.  **Exécutez la migration :**
    ```bash
    php bin/console doctrine:migrations:migrate
    ```
    *Cette commande exécute le fichier de migration généré. Vous devrez confirmer avec `yes`.*
    *Vous devriez voir `Executing migration <date_time>_Version...` et `Successfully migrated...`*
    *Un fichier `var/data.db` a été créé à la racine de votre projet avec la table `donateur` à l'intérieur !*

3.  **Vérifiez le statut Git, ajoutez et "committez" vos changements :**
    ```bash
    git status
    git add .
    git commit -m "feat: Ajout de l'entité Donateur et de sa migration"
    ```
    *Le commit inclura `src/Entity/Donateur.php`, `src/Repository/DonateurRepository.php` et le fichier de migration dans `migrations/`.*

4.  **Poussez votre branche vers GitHub :**
    ```bash
    git push origin feature-donateur-entity
    ```

### Étape 1.5 : Créer la Pull Request (Étudiant A)

1.  Allez sur la page du dépôt GitHub (`mon-app-symfony`) dans votre navigateur.
2.  Vous devriez voir une bannière "feature-donateur-entity had recent pushes..." avec un bouton **"Compare & pull request"**. Cliquez dessus.
    *Si non, cliquez sur l'onglet **"Pull requests"**, puis sur le bouton vert **"New pull request"**. Sélectionnez `base: main` et `compare: feature-donateur-entity`.*
3.  Donnez un titre significatif à votre PR (ex: "feat: Ajout de l'entité Donateur") et une description courte.
4.  Cliquez sur **"Create pull request"**.
5.  GitHub devrait indiquer que la fusion est possible.
6.  **Ne fusionnez PAS encore !** Laissez la PR ouverte. L'Étudiant B va maintenant la revoir et y intégrer ses propres changements, ce qui pourrait potentiellement créer des conflits que nous résoudrons dans l'atelier suivant si nous décidons d'y introduire de nouvelles entités ou de modifier le fichier de configuration. Pour l'instant, on maintient les branches séparées.

---

## Partie 2 : Préparation pour la Collaboration : Ajout d'une autre Entité (Réalisée par l'**Étudiant B**)

L'Étudiant B va créer une entité simple "Centre de Collecte" pour se familiariser avec le processus, avant qu'ils ne fusionnent tous leurs changements.

### Étape 2.1 : Synchroniser la Branche `main` et créer une nouvelle Branche

1.  Sur votre machine (machine de l'**Étudiant B**), assurez-vous d'être sur la branche `main` et qu'elle est à jour :
    ```bash
    git checkout main
    git pull origin main
    ```
2.  **Créez une nouvelle branche Git** pour cette fonctionnalité :
    ```bash
    git checkout -b feature-centre-collecte-entity
    ```

### Étape 2.2 : Créer l'Entité : CentreCollecte

1.  **Utilisez la console Symfony pour générer l'entité `CentreCollecte` :**
    ```bash
    php bin/console make:entity CentreCollecte
    ```
    *Ajoutez les propriétés suivantes :*
    *   `nom` (string, longueur 255, nullable: no)
    *   `adresse` (string, longueur 255, nullable: yes)
    *   `ville` (string, longueur 255, nullable: yes)
    *   `telephone` (string, longueur 20, nullable: yes)

2.  **Générez une nouvelle migration pour cette entité :**
    ```bash
    php bin/console make:migration
    ```
    *Un nouveau fichier de migration sera créé.*

3.  **Exécutez la migration pour créer la table `centre_collecte` dans votre base de données locale :**
    ```bash
    php bin/console doctrine:migrations:migrate
    ```
    *Confirmez avec `yes`.*

4.  **Vérifiez le statut Git, ajoutez et "committez" vos changements :**
    ```bash
    git status
    git add .
    git commit -m "feat: Ajout de l'entité CentreCollecte et de sa migration"
    ```

5.  **Poussez votre branche vers GitHub :**
    ```bash
    git push origin feature-centre-collecte-entity
    ```

### Étape 2.3 : Créer la Pull Request (Étudiant B)

1.  Allez sur la page du dépôt GitHub (`mon-app-symfony`) dans votre navigateur.
2.  Créez une PR pour `feature-centre-collecte-entity` vers `main`.
3.  Donnez un titre significatif à votre PR (ex: "feat: Ajout de l'entité Centre de Collecte") et une description courte.
4.  Cliquez sur **"Create pull request"**.
5.  **Observe le conflit :** GitHub devrait indiquer "**This branch has conflicts that must be resolved**" ou au minimum qu'il y a des changements qui se chevauchent. C'est normal ! Les deux branches ont ajouté des fichiers de migration différents et pourraient potentiellement entrer en conflit si les migrations affectaient les mêmes ressources ou si nous avions modifié un même fichier de configuration Doctrine. Dans ce cas, les migrations sont distinctes, mais Git voit que `main` a évolué depuis que vous avez créé votre branche.

---

## Partie 3 : Résolution Collaborative et Fusion des Entités

Maintenant, nous allons résoudre la situation où l'Étudiant B a une PR en attente tandis que l'Étudiant A a déjà ajouté une entité.

### Étape 3.1 : Mettre à Jour la Branche de l'Étudiant B avec `main` (Étudiant B)

Pour résoudre les "conflits" (ou plutôt les divergences car les fichiers sont distincts mais les branches `main` ont divergé), l'Étudiant B doit fusionner la branche `main` (qui contient déjà le travail de l'Étudiant A) dans sa propre branche.

1.  Sur votre machine (machine de l'**Étudiant B**), assurez-vous d'être sur votre branche de fonctionnalité :
    ```bash
    git checkout feature-centre-collecte-entity
    ```
2.  **Récupérez les derniers changements de la branche `main` distante :**
    ```bash
    git pull origin main
    ```
    *À ce stade, comme votre branche `feature-centre-collecte-entity` a divergé de `main` (qui contient maintenant les commits de l'Étudiant A), Git va tenter une fusion automatique. Normalement, il n'y aura pas de conflits de texte car l'Étudiant A a créé des fichiers différents (Donateur.php, sa migration) que ceux créés par l'Étudiant B (CentreCollecte.php, sa migration).*
    *Vous devriez voir un message indiquant que de nouveaux fichiers ont été ajoutés, comme `creating 100644 src/Entity/Donateur.php`, etc.*
3.  **Si Git vous demande d'entrer un message de commit** pour la fusion (ce qui est probable), vous pouvez laisser le message par défaut ou le modifier, puis enregistrer et quitter l'éditeur.
4.  **Poussez votre branche mise à jour vers GitHub :**
    ```bash
    git push origin feature-centre-collecte-entity
    ```
    *Ceci met à jour votre Pull Request sur GitHub, indiquant qu'elle peut maintenant être fusionnée sans conflit.*

### Étape 3.2 : Fusionner la Pull Request de l'Étudiant B (Étudiant A ou B)

Maintenant que la branche de l'Étudiant B est à jour, sa PR est prête à être fusionnée.

1.  Allez sur la page du dépôt GitHub (`mon-app-symfony`).
2.  Accédez à la Pull Request de l'**Étudiant B** (`feature-centre-collecte-entity`).
3.  GitHub devrait maintenant indiquer "This branch has no conflicts with the base branch".
4.  **Fusionnez la PR :** Cliquez sur le bouton vert **"Merge pull request"**, puis **"Confirm merge"**.
5.  **(Optionnel) Supprimez la branche :** Après la fusion, vous pouvez cliquer sur le bouton **"Delete branch"** pour nettoyer votre dépôt distant.

### Étape 3.3 : Nettoyage et Synchronisation Finale (Chaque Étudiant)

1.  **Chaque étudiant doit synchroniser sa branche `main` locale :**
    ```bash
    git checkout main
    git pull origin main
    ```
    *Vos répertoires de projet contiennent maintenant à la fois l'entité `Donateur` et l'entité `CentreCollecte`, ainsi que leurs migrations respectives.*
2.  **Vérifiez le contenu de votre base de données locale :**
    Vous pouvez utiliser un outil comme [DB Browser for SQLite](https://sqlitebrowser.org/) pour ouvrir le fichier `var/data.db` et confirmer que les tables `donateur` et `centre_collecte` existent.

---

## Conclusion et Prochaines Étapes

Félicitations ! Vous avez franchi une étape majeure en configurant votre base de données et en créant vos premières entités Symfony/Doctrine de manière collaborative. Vous comprenez maintenant comment :
*   Utiliser `make:entity` pour générer du code PHP pour vos modèles de données.
*   Utiliser `make:migration` et `doctrine:migrations:migrate` pour synchroniser vos entités avec votre base de données.
*   Gérer un flux de travail de Pull Request où des changements structuraux sont introduits par différentes personnes.

**Prochaines Étapes :**
Dans le prochain atelier, nous allons nous concentrer sur la **manipulation de ces entités**. Nous allons :
1.  Créer des formulaires pour ajouter de nouveaux donateurs.
2.  Développer des pages pour lister tous les donateurs.
3.  Voir comment modifier et supprimer des donateurs.

Continuez à pratiquer les commandes Git et à vous référer à la documentation Symfony. C'est en manipulant et en explorant que l'apprentissage se solidifie !