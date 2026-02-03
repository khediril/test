Compris ! Nous allons adapter le TP pour utiliser MySQL comme système de gestion de base de données, tout en conservant le fil conducteur de la "gestion des dons de sang" et l'apprentissage guidé basé sur la documentation officielle.

Voici la version révisée de l'atelier, avec un focus sur MySQL.

---

# Atelier Guidé Symfony & Git/GitHub : Gestion des Donateurs (Partie 1 - Avec MySQL)

## Contexte du Cours : Développement Web avec Symfony

Nous continuons notre parcours dans le développement web avec Symfony. Dans cet atelier, nous allons poser les bases de notre application de gestion des dons de sang en introduisant le concept de persistance des données. Nous allons créer notre première entité (le Donateur) et apprendre à interagir avec une base de données **MySQL**.

**Application :** Notre application Symfony `mon-app-symfony` va désormais gérer des informations sur les donateurs, stockées dans une base de données MySQL.

## Objectifs de cet Atelier

À la fin de cet atelier, vous serez capables de :
*   **Installer et configurer un serveur MySQL local.**
*   Configurer Doctrine ORM (Object-Relational Mapper) pour interagir avec une base de données **MySQL**.
*   Créer une entité Symfony/Doctrine pour modéliser une table de base de données (`Donateur`).
*   Générer et exécuter des migrations de base de données pour mettre à jour le schéma de votre base MySQL.
*   Travailler de manière collaborative sur la structure de l'application via Git/GitHub.

## Prérequis

*   Avoir terminé l'atelier précédent "Votre Première Application Collaborative".
*   Votre projet `mon-app-symfony` doit être cloné sur les deux machines et les dépendances installées (`composer install`).
*   Le serveur de développement Symfony doit être fonctionnel sur chaque machine (`symfony serve`).
*   Vous êtes familiers avec le flux de travail Git (`checkout`, `pull`, `branch`, `add`, `commit`, `push`, `Pull Request`, `merge`).
*   **MySQL Server installé et fonctionnel** sur chaque machine. Vous pouvez l'installer via :
    *   **XAMPP / MAMP / WAMP** (paquets tout-en-un pour Windows, macOS, Linux).
    *   **Docker** (recommandé pour une configuration isolée et reproductible).
    *   Installation directe via les paquets de votre OS (ex: `sudo apt install mysql-server` sur Debian/Ubuntu, `brew install mysql` sur macOS avec Homebrew).
*   Un client MySQL (par exemple, phpMyAdmin inclus dans XAMPP/MAMP/WAMP, MySQL Workbench, DBeaver) pour créer et vérifier les bases de données.

## Références Documentation Officielle Symfony

Cet atelier s'appuie directement sur les sections suivantes de la documentation officielle de Symfony :
*   [Doctrine ORM](https://symfony.com/doc/current/doctrine.html)
*   [Configuration de la base de données](https://symfony.com/doc/current/doctrine/database.html)
*   [Création d'entités](https://symfony.com/doc/current/doctrine/entities.html)
*   [Migrations de base de données](https://symfony.com/doc/current/doctrine/migrations.html)
*   [Associations d'entités (pour plus tard)](https://symfony.com/doc/current/doctrine/associations.html)

---

## Partie 0 : Préparation de l'Environnement et Base de Données MySQL (Chaque Étudiant)

**Chaque étudiant doit réaliser cette section sur sa propre machine.**

### Étape 0.1 : Vérification et Configuration de Git

1.  Ouvrez votre terminal (ou Git Bash sur Windows).
2.  Vérifiez votre installation de Git :
    ```bash
    git --version
    ```
3.  Configurez votre nom d'utilisateur Git :
    ```bash
    git config --global user.name "Votre Nom Complet"
    ```
    *Exemple : `git config --global user.name "Alice Dupont"`*
4.  Configurez votre adresse e-mail (celle associée à votre compte GitHub) :
    ```bash
    git config --global user.email "votre.email@example.com"
    ```
    *Exemple : `git config --global user.email "alice.dupont@etu.fr"`*
5.  Vérifiez votre configuration :
    ```bash
    git config --list
    ```
    Confirmez que `user.name` et `user.email` sont corrects.

### Étape 0.2 : Vérification des Prérequis PHP, Composer et MySQL

1.  Vérifiez votre version de PHP (doit être 8.1 ou supérieur) :
    ```bash
    php -v
    ```
2.  Vérifiez que l'extension PHP pour MySQL est activée :
    ```bash
    php -m | grep pdo_mysql
    ```
    *Vous devriez voir `pdo_mysql` dans la liste.* Si ce n'est pas le cas, vous devrez l'activer dans votre configuration PHP (souvent `php.ini` ou via votre gestionnaire de paquets/XAMPP/MAMP).
3.  Vérifiez votre installation de Composer :
    ```bash
    composer --version
    ```
4.  **Assurez-vous que votre serveur MySQL est démarré et fonctionnel.**
    *Si vous utilisez XAMPP/MAMP, démarrez les services Apache et MySQL.*
    *Si vous utilisez Docker, assurez-vous que votre conteneur MySQL est lancé.*

### Étape 0.3 : Créer la Base de Données MySQL Locale

Chaque étudiant aura sa propre base de données MySQL locale pour le développement.

1.  Ouvrez un client MySQL (phpMyAdmin, MySQL Workbench, DBeaver, ou la console MySQL).
2.  Connectez-vous à votre serveur MySQL (généralement `root` avec ou sans mot de passe par défaut, ou l'utilisateur configuré pour XAMPP/MAMP).
3.  Exécutez la commande SQL pour créer votre base de données. Utilisez un nom clair, par exemple `symfony_dons_de_sang_votre_nom`.
    ```sql
    CREATE DATABASE symfony_dons_de_sang_votre_nom CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ```
    *Exemple pour l'Étudiant A : `CREATE DATABASE symfony_dons_de_sang_alice;`*
    *Exemple pour l'Étudiant B : `CREATE DATABASE symfony_dons_de_sang_bob;`*

### Étape 0.4 : Synchronisation Initiale du Code

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

## Partie 1 : Configuration de la Base de Données & Première Entité (Réalisée par l'**Étudiant A**)

L'Étudiant A sera le responsable de la configuration initiale de la base de données MySQL dans le projet Symfony et de la première entité.

### Étape 1.1 : Comprendre Doctrine ORM et la Base de Données MySQL

Symfony utilise **Doctrine ORM** pour interagir avec MySQL. Votre application Symfony sera configurée pour se connecter à votre instance MySQL locale.

**Référence :** [symfony.com/doc/current/doctrine.html](https://symfony.com/doc/current/doctrine.html) et [symfony.com/doc/current/doctrine/database.html](https://symfony.com/doc/current/doctrine/database.html)

### Étape 1.2 : Configuration de la Connexion à la Base de Données MySQL

La connexion à la base de données est gérée par la variable d'environnement `DATABASE_URL` dans le fichier `.env`.

1.  **Ouvrez le fichier `.env`** dans votre éditeur de code.
2.  **Localisez la ligne `DATABASE_URL` et modifiez-la pour MySQL.**
    *Commentez la ligne `sqlite` si elle est décommentée.*
    *Décommentez et ajustez la ligne `mysql` pour correspondre à votre configuration MySQL locale et au nom de la base de données que vous avez créée.*

    *Exemple (pour l'Étudiant A, avec un utilisateur `root` et sans mot de passe, et une base de données `symfony_dons_de_sang_alice`):*
    ```dotenv
    # DATABASE_URL="sqlite:///%kernel.project_dir%/var/data.db"
    DATABASE_URL="mysql://root:@127.0.0.1:3306/symfony_dons_de_sang_alice?serverVersion=8.0.32&charset=utf8mb4"
    ```
    *   **`root`** : Remplacez par votre nom d'utilisateur MySQL.
    *   **`@`** : Suivi du mot de passe MySQL si vous en avez un (ex: `root:monmotdepasse@`). Si pas de mot de passe, laissez `root:@`.
    *   **`127.0.0.1:3306`** : L'adresse et le port de votre serveur MySQL local.
    *   **`/symfony_dons_de_sang_alice`** : Remplacez par le nom EXACT de la base de données que vous avez créée à l'Étape 0.3.
    *   `serverVersion` : La version de votre serveur MySQL (ajustez si nécessaire, ex: `5.7` ou `8.0`).

3.  **Vérifiez le statut Git** : `git status`. Normalement, `.env` est ignoré par Git via le fichier `.gitignore` (c'est une bonne pratique). Vous ne devriez donc pas voir de changement ici, ce qui signifie que vos informations sensibles ne seront pas poussées sur GitHub.

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
    *Ajoutez les propriétés suivantes :*
    *   `nom` (string, longueur 255, nullable: no)
    *   `prenom` (string, longueur 255, nullable: no)
    *   `email` (string, longueur 255, nullable: yes, unique: yes)
    *   `dateNaissance` (datetime_immutable, nullable: yes)
    *   `groupeSanguin` (string, longueur 5, nullable: yes)
    *   `telephone` (string, longueur 20, nullable: yes)

    *Exemple d'interaction dans le terminal :*
    ```
    php bin/console make:entity Donateur
    # ... suivez les invites comme dans l'atelier précédent ...
    ```
    *Cette commande a créé deux fichiers : `src/Entity/Donateur.php` (qui représente notre donateur et ses propriétés) et `src/Repository/DonateurRepository.php` (qui sera utilisé pour interroger la base de données).*

### Étape 1.4 : Générer et Exécuter une Migration de Base de Données MySQL

Les entités définissent la structure de vos données en PHP. Pour que cette structure soit reflétée dans votre base de données MySQL, vous devez créer et exécuter une migration.

1.  **Générez une nouvelle migration :**
    **Référence :** [symfony.com/doc/current/doctrine/migrations.html](https://symfony.com/doc/current/doctrine/migrations.html)
    ```bash
    php bin/console make:migration
    ```
    *Cette commande compare l'état actuel de vos entités PHP avec le schéma de votre base de données MySQL configurée dans `.env` et génère un fichier PHP dans `migrations/` qui contient les instructions SQL nécessaires pour créer la table `donateur`.*
    *Vous devriez voir un message comme `Created new migration file to "<date_time>_<migration_name>.php"`.*
2.  **Exécutez la migration :**
    ```bash
    php bin/console doctrine:migrations:migrate
    ```
    *Cette commande exécute le fichier de migration généré. Vous devrez confirmer avec `yes`.*
    *Vous devriez voir `Executing migration <date_time>_Version...` et `Successfully migrated...`*
    *Vérifiez dans votre client MySQL (phpMyAdmin, MySQL Workbench) que la table `donateur` a bien été créée dans votre base de données `symfony_dons_de_sang_alice`.*

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
5.  **Ne fusionnez PAS encore !** Laissez la PR ouverte. L'Étudiant B va maintenant travailler de son côté, et nous gérerons la fusion après.

---

## Partie 2 : Préparation pour la Collaboration : Ajout d'une autre Entité (Réalisée par l'**Étudiant B**)

L'Étudiant B va créer une entité simple "Centre de Collecte" pour se familiariser avec le processus dans un environnement MySQL.

### Étape 2.1 : Synchroniser la Branche `main`, Configurer MySQL et créer une nouvelle Branche

1.  Sur votre machine (machine de l'**Étudiant B**), assurez-vous d'être sur la branche `main` et qu'elle est à jour :
    ```bash
    git checkout main
    git pull origin main
    ```
2.  **Configurez votre fichier `.env` pour MySQL.**
    *Ouvrez le fichier `.env` dans votre éditeur.*
    *Commentez la ligne `sqlite` si elle est décommentée.*
    *Décommentez et ajustez la ligne `mysql` pour correspondre à votre configuration MySQL locale et au nom de la base de données que vous avez créée à l'Étape 0.3 (`symfony_dons_de_sang_bob`).*

    *Exemple (pour l'Étudiant B) :*
    ```dotenv
    # DATABASE_URL="sqlite:///%kernel.project_dir%/var/data.db"
    DATABASE_URL="mysql://root:@127.0.0.1:3306/symfony_dons_de_sang_bob?serverVersion=8.0.32&charset=utf8mb4"
    ```
    *Enregistrez le fichier. Encore une fois, ce changement n'est pas versionné par Git.*

3.  **Créez une nouvelle branche Git** pour cette fonctionnalité :
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
    *Un nouveau fichier de migration sera créé dans `migrations/`.*

3.  **Exécutez la migration pour créer la table `centre_collecte` dans votre base de données locale MySQL :**
    ```bash
    php bin/console doctrine:migrations:migrate
    ```
    *Confirmez avec `yes`.*
    *Vérifiez dans votre client MySQL que la table `centre_collecte` a bien été créée dans votre base de données `symfony_dons_de_sang_bob`.*

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
5.  **Observe le conflit :** GitHub devrait indiquer "**This branch has conflicts that must be resolved**" ou au minimum qu'il y a des changements qui se chevauchent. C'est normal ! Les deux branches ont ajouté des fichiers d'entité et de migration différents, et la branche `main` distante a été mise à jour par l'Étudiant A entre-temps.

---

## Partie 3 : Résolution Collaborative et Fusion des Entités

Nous allons maintenant fusionner les deux Pull Requests qui ajoutent des entités distinctes.

### Étape 3.1 : Mettre à Jour la Branche de l'Étudiant B avec `main` (Étudiant B)

Pour résoudre les "conflits" (qui seront ici des fusions automatiques sans conflit de texte, car les fichiers modifiés sont distincts), l'Étudiant B doit fusionner la branche `main` (qui contient déjà le travail de l'Étudiant A) dans sa propre branche.

1.  Sur votre machine (machine de l'**Étudiant B**), assurez-vous d'être sur votre branche de fonctionnalité :
    ```bash
    git checkout feature-centre-collecte-entity
    ```
2.  **Récupérez les derniers changements de la branche `main` distante :**
    ```bash
    git pull origin main
    ```
    *Git va tenter une fusion automatique. Normalement, il n'y aura pas de conflits de texte car l'Étudiant A a créé des fichiers différents (Donateur.php, sa migration) que ceux créés par l'Étudiant B (CentreCollecte.php, sa migration).*
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
2.  **Chaque étudiant doit exécuter les migrations manquantes sur sa base de données locale :**
    Étant donné que l'Étudiant A a déjà appliqué sa migration `Donateur` et que l'Étudiant B a déjà appliqué sa migration `CentreCollecte`, il faut s'assurer que chaque base de données locale contient bien *les deux tables*.
    Pour l'Étudiant A :
    ```bash
    php bin/console doctrine:migrations:migrate
    ```
    *L'Étudiant A appliquera la migration `CentreCollecte` qui lui manquait.*
    Pour l'Étudiant B :
    ```bash
    php bin/console doctrine:migrations:migrate
    ```
    *L'Étudiant B appliquera la migration `Donateur` qui lui manquait.*
    *Dans les deux cas, confirmez avec `yes`.*
3.  **Vérifiez le contenu de votre base de données locale :**
    Utilisez votre client MySQL (phpMyAdmin, MySQL Workbench) pour ouvrir votre base de données (`symfony_dons_de_sang_alice` ou `symfony_dons_de_sang_bob`) et confirmer que les tables `donateur` et `centre_collecte` existent.

---

## Conclusion et Prochaines Étapes

Félicitations ! Vous avez configuré votre application Symfony pour utiliser MySQL, créé vos premières entités, géré des migrations et collaboré avec succès via Git et GitHub. Vous avez également vu comment gérer la divergence des branches et l'application des migrations dans un contexte collaboratif.

**Prochaines Étapes :**
Dans le prochain atelier, nous allons nous concentrer sur la **manipulation de ces entités**. Nous allons :
1.  Créer des formulaires Symfony pour ajouter de nouveaux donateurs.
2.  Développer des pages pour lister tous les donateurs.
3.  Voir comment modifier et supprimer des donateurs.

Continuez à pratiquer les commandes Git et à vous référer à la documentation Symfony. C'est en manipulant et en explorant que l'apprentissage se solidifie !