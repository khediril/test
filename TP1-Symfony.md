Excellent ! C'est une excellente idée de lier l'apprentissage de Symfony à la collaboration avec Git dès le début, et de se baser sur la documentation officielle. Cela habituera les étudiants aux meilleures pratiques.

Voici un atelier guidé et détaillé pour démarrer avec Symfony et Git/GitHub, conçu pour deux étudiants et basé exclusivement sur la documentation officielle de Symfony.

---

# Atelier Guidé Symfony & Git/GitHub : Votre Première Application Collaborative

## Contexte du Cours : Développement Web avec Symfony

Bienvenue dans votre premier atelier pratique ! Ce module se concentre sur le développement web avec le framework côté serveur Symfony. L'objectif est de vous faire manipuler le code et les outils de manière concrète. Pour ce faire, nous allons simuler un projet réel où deux étudiants collaborent sur la création d'une application web simple.

**Application :** Nous allons créer une petite application Symfony qui affichera un message d'accueil et permettra de naviguer vers quelques pages simples (accueil, à propos).

## Objectifs de cet Atelier

À la fin de cet atelier, vous serez capables de :
*   Configurer votre environnement de développement Symfony.
*   Créer un nouveau projet Symfony à partir de zéro.
*   Comprendre la structure de base d'une application Symfony (contrôleurs, routes, vues).
*   Utiliser Git pour initialiser un dépôt local et le lier à un dépôt distant sur GitHub.
*   Collaborer sur un même projet en utilisant des branches, des commits et des Pull Requests (PR).
*   Gérer et résoudre des conflits de fusion (merge conflicts).
*   Maintenir votre copie locale du code à jour avec le dépôt partagé.

## Prérequis

*   **Deux ordinateurs** (ou deux sessions / utilisateurs si vous travaillez sur la même machine, mais idéalement deux machines séparées pour une simulation plus réaliste de la collaboration).
*   **Git installé** sur chaque machine. Pour vérifier, ouvrez un terminal et tapez `git --version`. Si ce n'est pas le cas, suivez les instructions sur [git-scm.com/downloads](https://git-scm.com/downloads).
*   **Deux comptes GitHub distincts.** Si vous n'en avez pas, créez-en un sur [github.com](https://github.com/).
*   **PHP 8.1 ou supérieur** installé sur chaque machine (avec les extensions requises : `php-cli`, `php-mbstring`, `php-xml`, `php-intl`, `php-sqlite3` ou `php-pdo_mysql` selon votre base de données future, etc.). Vérifiez avec `php -v` et `php -m`.
*   **Composer** (le gestionnaire de paquets PHP) installé sur chaque machine. Vérifiez avec `composer --version`. Si ce n'est pas le cas, suivez les instructions sur [getcomposer.org/download/](https://getcomposer.org/download/).
*   **L'outil Symfony CLI** installé sur chaque machine. Ce n'est pas obligatoire mais fortement recommandé pour faciliter le développement.
*   Un éditeur de texte de votre choix (VS Code est très populaire et performant).

## Références Documentation Officielle Symfony

Ce TP s'appuie directement sur les sections suivantes de la documentation officielle de Symfony (version actuelle) :
*   [Installation de Symfony](https://symfony.com/doc/current/setup.html)
*   [Installation de PHP](https://symfony.com/doc/current/setup/requirements.html)
*   [Créer une application Symfony](https://symfony.com/doc/current/setup.html#create-application)
*   [Lancer le serveur web local](https://symfony.com/doc/current/setup.html#running-the-symfony-application)
*   [Introduction aux contrôleurs](https://symfony.com/doc/current/controller.html)
*   [Introduction aux routes](https://symfony.com/doc/current/routing.html)
*   [Introduction aux templates Twig](https://symfony.com/doc/current/templates.html)
*   [Créer un template de base et l'étendre](https://symfony.com/doc/current/templates.html#template-inheritance)

---

## Partie 0 : Préparation de l'Environnement (Chaque Étudiant)

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

### Étape 0.2 : Vérification des Prérequis PHP et Composer

1.  Vérifiez votre version de PHP (doit être 8.1 ou supérieur) :
    ```bash
    php -v
    ```
    Si PHP n'est pas installé ou est une version trop ancienne, suivez les instructions sur [symfony.com/doc/current/setup/requirements.html](https://symfony.com/doc/current/setup/requirements.html) pour votre OS.
2.  Vérifiez les extensions PHP :
    ```bash
    php -m
    ```
    Assurez-vous que les extensions mentionnées dans les prérequis (mbstring, xml, intl, pdo_sqlite ou pdo_mysql) sont activées. Si ce n'est pas le cas, référez-vous à la documentation PHP ou à des tutoriels spécifiques à votre OS.
3.  Vérifiez votre installation de Composer :
    ```bash
    composer --version
    ```
    Si Composer n'est pas installé, suivez les instructions sur [getcomposer.org/download/](https://getcomposer.org/download/).

### Étape 0.3 : Installation de l'Outil Symfony CLI

1.  Suivez les instructions sur [symfony.com/doc/current/setup.html#install-symfony-cli](https://symfony.com/doc/current/setup.html#install-symfony-cli) pour installer l'outil Symfony CLI sur votre système.
2.  Vérifiez l'installation :
    ```bash
    symfony --version
    ```

---

## Partie 1 : Création du Projet Symfony & Initialisation Git (Réalisée par l'**Étudiant A**)

L'Étudiant A va créer le projet Symfony et le dépôt GitHub initial.

### Étape 1.1 : Créer une Nouvelle Application Symfony

1.  Dans votre terminal (machine de l'**Étudiant A**), naviguez vers le répertoire où vous souhaitez créer votre projet (par exemple, `~/Projets`).
    ```bash
    cd ~/Projets # Exemple
    ```
2.  Créez une nouvelle application Symfony. Nous utiliserons le profil "webapp" qui inclut des paquets couramment utilisés pour les applications web (comme Twig pour les templates).
    **Référence :** [symfony.com/doc/current/setup.html#create-application](https://symfony.com/doc/current/setup.html#create-application)
    ```bash
    symfony new mon-app-symfony --webapp
    ```
    *Cette commande va créer un nouveau répertoire `mon-app-symfony`, télécharger Symfony et ses dépendances, et installer les paquets essentiels.*
3.  Naviguez dans le répertoire du projet :
    ```bash
    cd mon-app-symfony
    ```

### Étape 1.2 : Lancer le Serveur de Développement

1.  Lancez le serveur web local de Symfony pour vérifier que l'application fonctionne.
    **Référence :** [symfony.com/doc/current/setup.html#running-the-symfony-application](https://symfony.com/doc/current/setup.html#running-the-symfony-application)
    ```bash
    symfony serve
    ```
    *Le terminal affichera une URL (généralement `https://127.0.0.1:8000`). Ouvrez cette URL dans votre navigateur.*
    Vous devriez voir la page d'accueil par défaut de Symfony, félicitations !
2.  Laissez le terminal avec le serveur Symfony ouvert. Ouvrez un **nouveau terminal** pour la suite des opérations Git.

### Étape 1.3 : Initialisation Git et Premier Commit

1.  Dans le nouveau terminal, assurez-vous d'être dans le répertoire `mon-app-symfony`.
2.  Vérifiez le statut Git (Symfony a déjà initialisé un dépôt Git et ignoré les fichiers non essentiels) :
    ```bash
    git status
    ```
    *Vous devriez voir un message indiquant qu'il n'y a rien à commiter ou que vous êtes sur la branche `main` avec des commits initiaux déjà faits par Symfony CLI.*
3.  Si Symfony CLI a déjà fait le commit initial, passez à l'étape suivante. Sinon, si vous avez utilisé `git init` manuellement et que vous voyez des fichiers en vert, faites :
    ```bash
    git add .
    git commit -m "Initial commit: Projet Symfony créé"
    ```

### Étape 1.4 : Créer le Dépôt Distant sur GitHub

1.  Ouvrez votre navigateur et allez sur [github.com](https://github.com/).
2.  Connectez-vous avec le compte de l'**Étudiant A**.
3.  Cliquez sur le bouton vert **"New"** ou sur l'icône **"+"** en haut à droite, puis **"New repository"**.
4.  **Nommez le dépôt :** `mon-app-symfony` (utilisez le même nom que votre projet local).
5.  **Description :** (Optionnel) `Notre première application Symfony collaborative.`
6.  **Public ou Private :** Choisissez **"Public"** pour faciliter la collaboration.
7.  **NE cochez PAS** "Add a README file", "Add .gitignore", ou "Choose a license" (Symfony les a déjà gérés localement).
8.  Cliquez sur le bouton vert **"Create repository"**.
9.  Sur la page suivante, GitHub vous donnera des instructions. Utilisez celles sous la section "…or push an existing repository from the command line".

### Étape 1.5 : Lier le Dépôt Local à GitHub et Pousser le Code

1.  Dans votre terminal (toujours sur la machine de l'**Étudiant A**), copiez-collez les commandes fournies par GitHub (remplacez `YOUR_USERNAME` par le nom d'utilisateur GitHub de l'Étudiant A) :
    ```bash
    git remote add origin https://github.com/YOUR_USERNAME/mon-app-symfony.git
    git branch -M main
    git push -u origin main
    ```
    *   `git remote add origin ...` : Lie votre dépôt local au dépôt distant sur GitHub, en lui donnant le nom `origin`.
    *   `git branch -M main` : Assure que votre branche principale s'appelle `main`.
    *   `git push -u origin main` : Envoie (pousse) les commits de votre branche `main` locale vers la branche `main` du dépôt `origin` sur GitHub. `-u` établit une liaison de suivi.

2.  Si nécessaire, une fenêtre peut s'ouvrir pour vous demander de vous connecter à GitHub ou d'entrer votre nom d'utilisateur et un Personal Access Token (PAT). Si c'est le cas, suivez les instructions pour générer un PAT sur GitHub ([settings > Developer settings > Personal access tokens > Tokens (classic)](https://github.com/settings/tokens)) avec l'accès `repo`.

3.  Actualisez la page de votre dépôt sur GitHub. Vous devriez maintenant voir tous les fichiers de votre projet Symfony.

### Étape 1.6 : Ajouter l'Étudiant B comme Collaborateur (Étudiant A)

1.  Sur la page du dépôt GitHub (`mon-app-symfony`), cliquez sur l'onglet **"Settings"**.
2.  Dans le menu latéral gauche, cliquez sur **"Collaborators and teams"**.
3.  Cliquez sur le bouton vert **"Add collaborator"**.
4.  Recherchez le nom d'utilisateur GitHub de l'**Étudiant B** et ajoutez-le. L'Étudiant B recevra une invitation par e-mail qu'il devra accepter.

---

## Partie 2 : Rejoindre le Projet (Réalisée par l'**Étudiant B**)

L'Étudiant B va maintenant obtenir une copie du projet Symfony et configurer son environnement.

### Étape 2.1 : Cloner le Dépôt

1.  Assurez-vous d'avoir accepté l'invitation de collaboration envoyée par l'Étudiant A sur GitHub.
2.  Dans votre terminal (sur la machine de l'**Étudiant B**), naviguez vers le répertoire où vous souhaitez stocker le projet (par exemple, `~/Projets`).
    ```bash
    cd ~/Projets # Exemple
    ```
3.  Clonez le dépôt. Pour obtenir l'URL de clonage, allez sur la page GitHub du dépôt de l'Étudiant A, cliquez sur le bouton vert **"Code"**, puis copiez l'URL HTTPS.
    ```bash
    git clone https://github.com/USERNAME_ETUDIANT_A/mon-app-symfony.git
    ```
    *Exemple : `git clone https://github.com/AliceDupont/mon-app-symfony.git`*
4.  Naviguez dans le dossier du projet cloné :
    ```bash
    cd mon-app-symfony
    ```

### Étape 2.2 : Installer les Dépendances du Projet

Lorsque vous clonez un projet Symfony, le dossier `vendor/` (contenant toutes les dépendances PHP) n'est pas inclus dans Git (il est listé dans `.gitignore`). Vous devez les installer localement.

1.  Dans le terminal (machine de l'**Étudiant B**), dans le répertoire `mon-app-symfony`, exécutez :
    ```bash
    composer install
    ```
    *Cela téléchargera et installera toutes les dépendances PHP nécessaires au projet.*

### Étape 2.3 : Lancer le Serveur de Développement

1.  Lancez le serveur web local de Symfony :
    **Référence :** [symfony.com/doc/current/setup.html#running-the-symfony-application](https://symfony.com/doc/current/setup.html#running-the-symfony-application)
    ```bash
    symfony serve
    ```
    *Ouvrez l'URL fournie dans votre navigateur. Vous devriez voir la même page d'accueil Symfony que l'Étudiant A.*
2.  Laissez le terminal avec le serveur Symfony ouvert. Ouvrez un **nouveau terminal** pour la suite des opérations Git.

---

## Partie 3 : Ajout de Fonctionnalités Indépendantes (Sans Conflits)

Nous allons maintenant ajouter des pages spécifiques à l'application. Chaque étudiant ajoutera une page différente.

### Étape 3.1 : Création de la Page "Accueil" par l'**Étudiant A**

L'Étudiant A va créer une route et un contrôleur pour une page d'accueil personnalisée.

1.  Dans votre terminal (machine de l'**Étudiant A**), assurez-vous que votre branche `main` est à jour :
    ```bash
    git checkout main
    git pull origin main
    ```
2.  **Créez une nouvelle branche** pour cette fonctionnalité :
    ```bash
    git checkout -b feature-homepage
    ```
3.  **Créez un nouveau contrôleur pour l'accueil :**
    **Référence :** [symfony.com/doc/current/controller.html](https://symfony.com/doc/current/controller.html)
    Utilisez la console Symfony pour générer un contrôleur.
    ```bash
    php bin/console make:controller HomeController
    ```
    *Suivez les invites. Laissez la suggestion de route par défaut (`/home`) pour l'instant.*
4.  **Modifiez le contrôleur `src/Controller/HomeController.php` :**
    **Référence :** [symfony.com/doc/current/controller.html](https://symfony.com/doc/current/controller.html)
    Ouvrez ce fichier. Remplacez le contenu de la méthode `index` par :
    ```php
    <?php

    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\HttpFoundation\Response;
    use Symfony\Component\Routing\Annotation\Route;

    class HomeController extends AbstractController
    {
        #[Route('/', name: 'app_home')] // Modifiez la route pour être la page d'accueil
        public function index(): Response
        {
            return $this->render('home/index.html.twig', [
                'controller_name' => 'HomeController',
                'message' => 'Bienvenue sur notre application Symfony collaborative !'
            ]);
        }
    }
    ```
    *Nous avons changé la route de `/home` à `/` pour que ce soit la page d'accueil par défaut, et ajouté un `message` à passer au template.*
5.  **Modifiez le template `templates/home/index.html.twig` :**
    **Référence :** [symfony.com/doc/current/templates.html](https://symfony.com/doc/current/templates.html)
    Ouvrez ce fichier. Le contenu généré par `make:controller` peut être simple. Modifiez-le pour afficher le message :
    ```twig
    {% extends 'base.html.twig' %}

    {% block title %}Accueil - Mon App Symfony{% endblock %}

    {% block body %}
    <style>
        .example-wrapper { margin: 1em auto; max-width: 800px; width: 95%; font: 18px/1.5 sans-serif; }
        .example-wrapper code { background: #F5F5F5; padding: 2px 6px; }
    </style>

    <div class="example-wrapper">
        <h1>Hello {{ controller_name }}! ✅</h1>
        <p>{{ message }}</p>

        <p>Ceci est la page d'accueil personnalisée par l'Étudiant A.</p>
        <ul>
            <li>Your controller at <code>{{ 'src/Controller/HomeController.php'|file_link(0) }}</code></li>
            <li>Your template at <code>{{ 'templates/home/index.html.twig'|file_link(0) }}</code></li>
        </ul>
    </div>
    {% endblock %}
    ```
6.  **Vérifiez la page dans votre navigateur :** Allez sur `https://127.0.0.1:8000/`. Vous devriez voir votre message personnalisé.
7.  **Vérifiez le statut Git, ajoutez et "committez" vos changements :**
    ```bash
    git status
    git add .
    git commit -m "feat: Création de la page d'accueil personnalisée"
    ```
8.  **Poussez votre branche vers GitHub :**
    ```bash
    git push origin feature-homepage
    ```

### Étape 3.2 : Création de la Page "À Propos" par l'**Étudiant B**

L'Étudiant B va créer une route et un contrôleur pour une page "À Propos".

1.  Dans votre terminal (machine de l'**Étudiant B**), assurez-vous que votre branche `main` est à jour :
    ```bash
    git checkout main
    git pull origin main
    ```
    *À ce stade, votre `main` ne contient PAS les changements de l'Étudiant A, car ils sont sur sa branche `feature-homepage`.*
2.  **Créez une nouvelle branche** pour cette fonctionnalité :
    ```bash
    git checkout -b feature-about
    ```
3.  **Créez un nouveau contrôleur pour la page "À Propos" :**
    **Référence :** [symfony.com/doc/current/controller.html](https://symfony.com/doc/current/controller.html)
    ```bash
    php bin/console make:controller AboutController
    ```
    *Laissez la suggestion de route par défaut (`/about`).*
4.  **Modifiez le contrôleur `src/Controller/AboutController.php` :**
    Ouvrez ce fichier. Le contrôleur sera similaire à celui généré. Modifiez la méthode `index` :
    ```php
    <?php

    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\HttpFoundation\Response;
    use Symfony\Component\Routing\Annotation\Route;

    class AboutController extends AbstractController
    {
        #[Route('/about', name: 'app_about')]
        public function index(): Response
        {
            return $this->render('about/index.html.twig', [
                'controller_name' => 'AboutController',
                'description' => 'Cette application est développée dans le cadre d\'un TP Symfony.'
            ]);
        }
    }
    ```
5.  **Modifiez le template `templates/about/index.html.twig` :**
    Ouvrez ce fichier. Modifiez-le pour afficher la description :
    ```twig
    {% extends 'base.html.twig' %}

    {% block title %}À Propos - Mon App Symfony{% endblock %}

    {% block body %}
    <style>
        .example-wrapper { margin: 1em auto; max-width: 800px; width: 95%; font: 18px/1.5 sans-serif; }
        .example-wrapper code { background: #F5F5F5; padding: 2px 6px; }
    </style>

    <div class="example-wrapper">
        <h1>À Propos de Notre Application</h1>
        <p>{{ description }}</p>

        <p>Cette page a été créée par l'Étudiant B.</p>
        <ul>
            <li>Your controller at <code>{{ 'src/Controller/AboutController.php'|file_link(0) }}</code></li>
            <li>Your template at <code>{{ 'templates/about/index.html.twig'|file_link(0) }}</code></li>
        </ul>
    </div>
    {% endblock %}
    ```
6.  **Vérifiez la page dans votre navigateur :** Allez sur `https://127.0.0.1:8000/about`. Vous devriez voir votre page "À Propos".
7.  **Vérifiez le statut Git, ajoutez et "committez" vos changements :**
    ```bash
    git status
    git add .
    git commit -m "feat: Création de la page À Propos"
    ```
8.  **Poussez votre branche vers GitHub :**
    ```bash
    git push origin feature-about
    ```

### Étape 3.3 : Créer et Fusionner la Pull Request de l'**Étudiant A**

L'Étudiant A va proposer ses changements pour la page d'accueil à la branche principale.

1.  Allez sur la page du dépôt GitHub (`mon-app-symfony`) dans votre navigateur.
2.  Vous devriez voir une bannière "feature-homepage had recent pushes..." avec un bouton **"Compare & pull request"**. Cliquez dessus.
    *Si non, cliquez sur l'onglet **"Pull requests"**, puis sur le bouton vert **"New pull request"**. Sélectionnez `base: main` et `compare: feature-homepage`.*
3.  Donnez un titre significatif à votre PR (ex: "feat: Page d'accueil personnalisée") et une description courte.
4.  Cliquez sur **"Create pull request"**.
5.  La page de la PR s'ouvre. GitHub vérifiera si les branches peuvent être fusionnées automatiquement. Ici, cela devrait être le cas car l'Étudiant B a travaillé sur des fichiers différents.
6.  **Fusionnez la PR :** Cliquez sur le bouton vert **"Merge pull request"**, puis **"Confirm merge"**.
7.  **(Optionnel) Supprimez la branche :** Après la fusion, vous pouvez cliquer sur le bouton **"Delete branch"** pour nettoyer votre dépôt distant.

### Étape 3.4 : Synchroniser la Branche `main` de l'**Étudiant B**

L'Étudiant B doit récupérer les changements de l'Étudiant A qui viennent d'être fusionnés sur `main`.

1.  Sur sa machine (machine de l'**Étudiant B**), assurez-vous d'être sur la branche `main` :
    ```bash
    git checkout main
    ```
2.  Puis, récupérez les derniers changements du dépôt distant :
    ```bash
    git pull origin main
    ```
    *Vous devriez voir que les fichiers `src/Controller/HomeController.php` et `templates/home/index.html.twig` ont été ajoutés/modifiés. Ouvrez-les pour vérifier.*

### Étape 3.5 : Créer et Fusionner la Pull Request de l'**Étudiant B**

Maintenant, l'Étudiant B propose ses changements pour la page "À Propos".

1.  Allez sur la page du dépôt GitHub (`mon-app-symfony`).
2.  Créez une Pull Request pour la branche `feature-about` vers `main` (comme l'Étudiant A l'a fait pour `feature-homepage`).
    *Sélectionnez `base: main` et `compare: feature-about`.*
3.  Donnez un titre (ex: "feat: Page À Propos") et une description.
4.  Cliquez sur **"Create pull request"**.
5.  GitHub indiquera que la fusion est possible.
6.  **Fusionnez la PR :** Cliquez sur **"Merge pull request"**, puis **"Confirm merge"**.
7.  **(Optionnel) Supprimez la branche :** Cliquez sur **"Delete branch"**.

### Étape 3.6 : Synchroniser la Branche `main` de l'**Étudiant A**

L'Étudiant A doit récupérer les changements de l'Étudiant B.

1.  Sur sa machine (machine de l'**Étudiant A**), assurez-vous d'être sur la branche `main` :
    ```bash
    git checkout main
    ```
2.  Puis, récupérez les derniers changements du dépôt distant :
    ```bash
    git pull origin main
    ```
    *Vous devriez voir que les fichiers `src/Controller/AboutController.php` et `templates/about/index.html.twig` ont été ajoutés/modifiés. Ouvrez-les pour vérifier.*
3.  Vérifiez les deux pages dans votre navigateur (`/` et `/about`).

---

## Partie 4 : Gestion et Résolution de Conflits de Fusion

Cette partie simule une situation courante en collaboration où deux développeurs modifient la même partie d'un même fichier.

### Étape 4.1 : Modification Conflicteuse par l'**Étudiant A**

L'Étudiant A va modifier le titre du template de base (`templates/base.html.twig`).

1.  **Assurez-vous que votre branche `main` est à jour :**
    ```bash
    git checkout main
    git pull origin main
    ```
2.  **Créez une nouvelle branche** pour votre tâche :
    ```bash
    git checkout -b refactor-base-template
    ```
3.  **Modifiez le fichier `templates/base.html.twig` :**
    **Référence :** [symfony.com/doc/current/templates.html#template-inheritance](https://symfony.com/doc/current/templates.html#template-inheritance)
    Ouvrez ce fichier. Remplacez la ligne `<title>{% block title %}Welcome!{% endblock %}</title>` par :
    ```twig
    <title>{% block title %}Symfony App Collaborative - A{% endblock %}</title>
    ```
    *Remplacez "A" par une indication que c'est la version de l'Étudiant A.*
4.  **Committer les changements :**
    ```bash
    git add templates/base.html.twig
    git commit -m "refactor: Modification du titre de base par Etudiant A"
    ```
5.  **Poussez votre branche :**
    ```bash
    git push origin refactor-base-template
    ```

### Étape 4.2 : Modification Conflicteuse par l'**Étudiant B**

L'Étudiant B va modifier la *même ligne* du titre du template de base (`templates/base.html.twig`).

1.  **Très important : Assurez-vous que votre branche `main` est à jour** AVANT de créer une nouvelle branche pour cette tâche.
    ```bash
    git checkout main
    git pull origin main
    ```
    *À ce stade, votre `main` ne contient PAS les changements de `refactor-base-template` de l'Étudiant A, car ils sont sur une autre branche.*
2.  **Créez une nouvelle branche** pour votre tâche :
    ```bash
    git checkout -b update-base-title
    ```
3.  **Modifiez le fichier `templates/base.html.twig` :**
    Ouvrez ce fichier. Remplacez la ligne `<title>{% block title %}Welcome!{% endblock %}</title>` par :
    ```twig
    <title>{% block title %}Projet Symfony B - Titre Mis à Jour{% endblock %}</title>
    ```
    *Remplacez "B" par une indication que c'est la version de l'Étudiant B.*
4.  **Committer les changements :**
    ```bash
    git add templates/base.html.twig
    git commit -m "style: Mise à jour du titre de base par Etudiant B"
    ```
5.  **Poussez votre branche :**
    ```bash
    git push origin update-base-title
    ```

### Étape 4.3 : La Première Pull Request (Étudiant A)

L'Étudiant A va créer sa Pull Request.

1.  Allez sur la page du dépôt GitHub (`mon-app-symfony`).
2.  Créez une PR pour `refactor-base-template` vers `main`.
3.  GitHub devrait indiquer que la fusion est possible.
4.  **Fusionnez la PR** et supprimez la branche.

### Étape 4.4 : La Deuxième Pull Request et Résolution de Conflits (Étudiant B)

Maintenant, l'Étudiant B va créer sa PR.

1.  Allez sur la page du dépôt GitHub (`mon-app-symfony`).
2.  Créez une PR pour `update-base-title` vers `main`.
3.  **Observe le conflit :** GitHub devrait indiquer "**This branch has conflicts that must be resolved**". C'est normal !
4.  **Résolution Locale du Conflit par l'**Étudiant B** :**

    a.  **Mettez à jour votre branche `main` locale :**
        ```bash
        git checkout main
        git pull origin main
        ```
        *Votre `main` contient maintenant les changements de l'Étudiant A provenant de la branche `refactor-base-template`.*

    b.  **Basculer sur votre branche de fonctionnalité (`update-base-title`) :**
        ```bash
        git checkout update-base-title
        ```

    c.  **Fusionnez `main` dans votre branche de fonctionnalité :**
        ```bash
        git merge main
        ```
        *C'est ici que le conflit se produit. Git vous affichera un message d'erreur et des marqueurs de conflit dans le fichier `templates/base.html.twig`.*
        ```
        Auto-merging templates/base.html.twig
        CONFLICT (content): Merge conflict in templates/base.html.twig
        Automatic merge failed; fix conflicts and then commit the result.
        ```

    d.  **Ouvrez `templates/base.html.twig` dans votre éditeur de texte.** Vous verrez des marqueurs de conflit :
        ```twig
        <title>{% block title %}
        <<<<<<< HEAD
        Projet Symfony B - Titre Mis à Jour
        =======
        Symfony App Collaborative - A
        >>>>>>> main
        {% endblock %}</title>
        ```
        *   `<<<<<<< HEAD` : C'est votre version (`update-base-title`).
        *   `=======` : La séparation entre les deux versions.
        *   `>>>>>>> main` : C'est la version qui vient de `main` (celle de l'Étudiant A).

    e.  **Modifiez le fichier pour résoudre le conflit :** Choisissez quelle version garder, ou combinez les deux. Supprimez les marqueurs (`<<<<<<<`, `=======`, `>>>>>>>`).
        *Exemple de résolution (une combinaison) :*
        ```twig
        <title>{% block title %}Notre Super App Symfony Collaborative{% endblock %}</title>
        ```

    f.  **Ajoutez le fichier résolu à la zone de staging :**
        ```bash
        git add templates/base.html.twig
        ```

    g.  **Committer la résolution du conflit :**
        ```bash
    git commit -m "fix: Résolution des conflits sur le titre de base"
        ```
        *Git peut pré-remplir un message de commit. Vous pouvez le laisser ou le modifier.*

    h.  **Poussez votre branche (`update-base-title`) vers GitHub :**
        ```bash
        git push origin update-base-title
        ```
        *Maintenant, la branche `update-base-title` sur GitHub est à jour et sans conflit.*

5.  **Retournez sur GitHub** à votre Pull Request. GitHub devrait maintenant indiquer "This branch has no conflicts with the base branch".
6.  **Fusionnez la PR.**

### Étape 4.5 : Synchroniser la Branche `main` de l'**Étudiant A**

1.  Sur sa machine (machine de l'**Étudiant A**), assurez-vous d'être sur la branche `main` :
    ```bash
    git checkout main
    ```
2.  Puis, récupérez les derniers changements du dépôt distant :
    ```bash
    git pull origin main
    ```
    *L'Étudiant A a maintenant la version fusionnée et résolue du fichier `templates/base.html.twig`. Ouvrez le fichier pour vérifier.*
3.  Vérifiez les pages dans votre navigateur, le titre de l'onglet devrait être la version résolue.

---

## Conclusion et Bonnes Pratiques

Félicitations ! Vous avez non seulement démarré un projet Symfony, mais vous avez aussi expérimenté un cycle complet de collaboration avec Git et GitHub, incluant la gestion des branches, des Pull Requests et la résolution de conflits.

Voici quelques bonnes pratiques à retenir, spécifiques à votre contexte de développement Symfony collaboratif :

*   **`git pull origin main` systématiquement :** Avant de commencer une nouvelle tâche ou de créer une nouvelle branche, assurez-vous toujours que votre branche `main` locale est à jour avec le dépôt distant. C'est la première chose à faire chaque matin !
*   **Branches pour chaque fonctionnalité/fix :** Ne travaillez jamais directement sur la branche `main`. Créez une branche dédiée (`feature-*, bugfix-*, refactor-*`) pour chaque tâche.
*   **Commits petits, fréquents et descriptifs :** Chaque commit devrait représenter une étape logique et atomique. Le message de commit doit être clair et expliquer ce que le commit fait et pourquoi.
    *   *Exemple :* `feat: Ajout du contrôleur et template de la page Contact`
    *   *Exemple :* `fix: Correction du bug d'affichage sur la page d'accueil`
*   **Pull Requests (PR) pour le code review :** Utilisez les PR pour proposer vos changements à la branche `main`. C'est l'occasion pour votre coéquipier de revoir votre code, de poser des questions et de suggérer des améliorations avant la fusion. C'est aussi à ce moment que les conflits potentiels sont détectés.
*   **Fusionnez `main` dans votre branche de fonctionnalité avant de créer une PR (ou en cas de conflit) :** Si votre branche de fonctionnalité est en retard par rapport à `main`, fusionnez `main` DANS votre branche avant de faire la PR. Cela vous permettra de résoudre les conflits sur votre propre branche, plus facilement, plutôt que d'avoir des conflits lors de la fusion de la PR.
*   **Communiquez :** En cas de doute, de blocage, ou si vous anticipez un conflit (par exemple, vous savez que vous travaillez sur la même partie qu'un collègue), parlez-en ! La communication est la clé d'une collaboration réussie.
*   **Ne poussez jamais directement sur `main` :** La branche `main` doit rester propre et stable. Tous les changements doivent y arriver via des Pull Requests validées.
*   **Utilisez `php bin/console make:controller` et `php bin/console make:entity` :** Ces commandes simplifient grandement la création des blocs de code Symfony et vous assurent de suivre les bonnes conventions.

Continuez à pratiquer ! La maîtrise de Git et de Symfony vient avec l'expérience et la répétition. N'hésitez pas à explorer davantage la documentation officielle de Symfony pour chaque nouveau concept.