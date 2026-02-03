Absolument ! Voici de nouveau le TP détaillé et guidé pour l'utilisation de Git et GitHub, conçu spécifiquement pour deux étudiants collaborant sur le développement d'une application.

---

# TP Git & GitHub pour Débutants : Collaboration à Deux

## Scénario : Développement d'une Application Simple

**Contexte :** Vous êtes deux étudiants, "Étudiant A" et "Étudiant B", travaillant ensemble sur un projet d'application web très simple (par exemple, une application Python/Flask ou une page HTML/CSS/JS basique). L'objectif est de simuler un flux de travail collaboratif en utilisant Git et GitHub.

**Application :** Nous allons créer une application web simple qui affichera un message d'accueil et quelques fonctionnalités ajoutées par chaque étudiant. Pour simplifier, nous utiliserons un fichier Python (`app.py`) et un fichier HTML (`index.html`).

## Objectifs de ce TP

À la fin de ce TP, vous serez capables de :
*   Configurer Git sur vos machines.
*   Créer et configurer un dépôt GitHub partagé.
*   Cloner un dépôt existant.
*   Effectuer des modifications, les "committer" et les "pusher" vers le dépôt distant.
*   Créer et gérer des branches pour développer des fonctionnalités séparément.
*   Proposer des changements via des Pull Requests (PR) sur GitHub.
*   Fusionner des branches et résoudre des conflits de fusion.
*   Maintenir votre copie locale du code à jour avec le dépôt distant.

## Prérequis

*   **Deux ordinateurs** (ou deux sessions / utilisateurs si vous travaillez sur la même machine, mais idéalement deux machines séparées).
*   **Git installé** sur chaque machine. Pour vérifier, ouvrez un terminal et tapez `git --version`. Si ce n'est pas le cas, suivez les instructions sur [git-scm.com/downloads](https://git-scm.com/downloads).
*   **Deux comptes GitHub distincts.** Si vous n'en avez pas, créez-en un sur [github.com](https://github.com/).
*   Connaissance de base des commandes du terminal (cd, mkdir, ls/dir).
*   Un éditeur de texte de votre choix (VS Code, Sublime Text, Notepad++, Gedit, etc.).

---

## Configuration Initiale (Chaque Étudiant)

**Chaque étudiant doit réaliser cette section sur sa propre machine.**

### Étape 0 : Ouvrir le Terminal et Configurer Git

1.  Ouvrez votre terminal (ou Git Bash sur Windows, ou l'émulateur de terminal de votre OS).
2.  Configurez votre nom d'utilisateur Git. Ce nom sera visible dans l'historique des commits.
    ```bash
    git config --global user.name "Votre Nom Complet"
    ```
    *Exemple : `git config --global user.name "Alice Dupont"`*
3.  Configurez votre adresse e-mail. Utilisez l'e-mail associé à votre compte GitHub.
    ```bash
    git config --global user.email "votre.email@example.com"
    ```
    *Exemple : `git config --global user.email "alice.dupont@etu.fr"`*
4.  Vérifiez votre configuration pour vous assurer que tout est correct :
    ```bash
    git config --list
    ```
    Vous devriez voir `user.name` et `user.email` dans la liste.

---

## Partie 1 : Création du Dépôt Principal (Réalisée par l'**Étudiant A**)

L'Étudiant A sera le responsable de la création initiale du projet et du dépôt GitHub.

### Étape 1.1 : Créer le Dépôt Local

1.  Créez un dossier pour votre projet et naviguez-y :
    ```bash
    mkdir mon-app-collaborative
    cd mon-app-collaborative
    ```
2.  Initialisez un nouveau dépôt Git local :
    ```bash
    git init
    ```
    *Vous devriez voir un message comme : `Initialized empty Git repository in /chemin/vers/mon-app-collaborative/.git/`*
3.  Créez le fichier `README.md` (fichier de description du projet) et un premier fichier d'application, `app.py`.
    ```bash
    echo "# Mon App Collaborative" > README.md
    echo "print('Bienvenue dans notre application collaborative !')" > app.py
    echo "<h1>Bienvenue !</h1><p>Cette application est un projet collaboratif.</p>" > index.html
    ```
4.  Vérifiez le statut de votre dépôt :
    ```bash
    git status
    ```
    *Vous devriez voir `README.md`, `app.py` et `index.html` en rouge, indiquant qu'ils ne sont pas encore suivis.*
5.  Ajoutez ces fichiers à la zone de staging (index Git) :
    ```bash
    git add README.md app.py index.html
    # Ou pour ajouter tous les nouveaux fichiers et modifications :
    # git add .
    ```
6.  Vérifiez de nouveau le statut. Les fichiers devraient être en vert.
    ```bash
    git status
    ```
7.  "Committer" les changements. Un commit est un enregistrement des modifications à un moment précis.
    ```bash
    git commit -m "Initial commit: Ajout du README, app.py et index.html"
    ```
    *Vous verrez un message confirmant le commit.*

### Étape 1.2 : Créer le Dépôt Distant sur GitHub

1.  Ouvrez votre navigateur et allez sur [github.com](https://github.com/).
2.  Connectez-vous avec le compte de l'**Étudiant A**.
3.  Cliquez sur le bouton vert **"New"** ou sur l'icône **"+"** en haut à droite, puis **"New repository"**.
4.  **Nommez le dépôt :** `mon-app-collaborative` (ou un nom similaire).
5.  **Description :** (Optionnel) `Notre première application collaborative.`
6.  **Public ou Private :** Choisissez **"Public"** pour faciliter la collaboration dans le cadre de ce TP.
7.  **NE cochez PAS** "Add a README file", "Add .gitignore", ou "Choose a license" (nous les avons déjà créés localement).
8.  Cliquez sur le bouton vert **"Create repository"**.
9.  Sur la page suivante, GitHub vous donnera des instructions. Vous devez utiliser celles sous la section "…or push an existing repository from the command line".

### Étape 1.3 : Lier le Dépôt Local à GitHub et Pousser le Code

1.  Dans votre terminal (toujours sur la machine de l'**Étudiant A**), copiez-collez les commandes fournies par GitHub (remplacez `YOUR_USERNAME` par le nom d'utilisateur GitHub de l'Étudiant A) :
    ```bash
    git remote add origin https://github.com/YOUR_USERNAME/mon-app-collaborative.git
    git branch -M main
    git push -u origin main
    ```
    *   `git remote add origin ...` : Lie votre dépôt local au dépôt distant sur GitHub, en lui donnant le nom `origin`.
    *   `git branch -M main` : Renomme votre branche principale en `main` (si elle n'est pas déjà appelée ainsi).
    *   `git push -u origin main` : Envoie (pousse) les commits de votre branche `main` locale vers la branche `main` du dépôt `origin` sur GitHub. `-u` établit une liaison de suivi.

2.  Une fenêtre peut s'ouvrir pour vous demander de vous connecter à GitHub ou d'entrer votre nom d'utilisateur et un Personal Access Token (PAT). Si c'est le cas, générez un PAT sur GitHub ([settings > Developer settings > Personal access tokens > Tokens (classic)](https://github.com/settings/tokens)) avec l'accès `repo`.

3.  Actualisez la page de votre dépôt sur GitHub. Vous devriez maintenant voir vos fichiers `README.md`, `app.py` et `index.html`.

### Étape 1.4 : Ajouter l'Étudiant B comme Collaborateur (Étudiant A)

Pour que l'Étudiant B puisse pousser directement vers le dépôt (sans avoir à forker), l'Étudiant A doit l'ajouter comme collaborateur.

1.  Sur la page du dépôt GitHub (`mon-app-collaborative`), cliquez sur l'onglet **"Settings"**.
2.  Dans le menu latéral gauche, cliquez sur **"Collaborators and teams"**.
3.  Cliquez sur le bouton vert **"Add collaborator"**.
4.  Recherchez le nom d'utilisateur GitHub de l'**Étudiant B** et ajoutez-le. L'Étudiant B recevra une invitation par e-mail qu'il devra accepter.

---

## Partie 2 : Rejoindre le Projet (Réalisée par l'**Étudiant B**)

L'Étudiant B va maintenant obtenir une copie du projet de l'Étudiant A.

### Étape 2.1 : Cloner le Dépôt

1.  Assurez-vous d'avoir accepté l'invitation de collaboration envoyée par l'Étudiant A sur GitHub.
2.  Dans votre terminal (sur la machine de l'**Étudiant B**), naviguez vers le répertoire où vous souhaitez stocker le projet (par exemple, votre dossier Documents ou Projets).
    ```bash
    cd ~/Documents/Projets # Exemple
    ```
3.  Clonez le dépôt. Pour obtenir l'URL de clonage, allez sur la page GitHub du dépôt de l'Étudiant A, cliquez sur le bouton vert **"Code"**, puis copiez l'URL HTTPS.
    ```bash
    git clone https://github.com/USERNAME_ETUDIANT_A/mon-app-collaborative.git
    ```
    *Exemple : `git clone https://github.com/AliceDupont/mon-app-collaborative.git`*
4.  Naviguez dans le dossier du projet cloné :
    ```bash
    cd mon-app-collaborative
    ```
5.  Vérifiez le contenu :
    ```bash
    ls # ou dir sur Windows
    ```
    Vous devriez voir `README.md`, `app.py` et `index.html`.

---

## Partie 3 : Collaboration : Ajout de Fonctionnalités (Sans Conflits)

Nous allons maintenant simuler le développement de deux fonctionnalités différentes par chaque étudiant.

### Étape 3.1 : Ajout d'une Fonctionnalité par l'**Étudiant A**

L'Étudiant A va travailler sur une fonctionnalité de "login".

1.  **Vérifiez que votre branche `main` est à jour** (très important avant de commencer une nouvelle tâche !) :
    ```bash
    git checkout main
    git pull origin main
    ```
    *Même si vous êtes l'initiateur, c'est une bonne habitude. Pour l'instant, il n'y aura pas de nouvelles mises à jour.*
2.  **Créez une nouvelle branche** pour votre fonctionnalité :
    ```bash
    git checkout -b feature-login
    ```
    *Ceci crée et bascule immédiatement sur la nouvelle branche `feature-login`.*
3.  **Modifiez le fichier `app.py` et `index.html` :**
    Ouvrez `app.py` dans votre éditeur et ajoutez une ligne, par exemple :
    ```python
    print('Bienvenue dans notre application collaborative !')
    print('Fonctionnalité de login ajoutée par Etudiant A.')
    ```
    Ouvrez `index.html` et ajoutez une section de login :
    ```html
    <h1>Bienvenue !</h1><p>Cette application est un projet collaboratif.</p>
    <h2>Connexion</h2>
    <form>
        <label for="username">Nom d'utilisateur:</label><br>
        <input type="text" id="username" name="username"><br>
        <label for="password">Mot de passe:</label><br>
        <input type="password" id="password" name="password"><br><br>
        <input type="submit" value="Se connecter">
    </form>
    ```
4.  **Vérifiez le statut :**
    ```bash
    git status
    ```
    *Vous verrez `app.py` et `index.html` modifiés.*
5.  **Ajoutez et "committez" vos changements :**
    ```bash
    git add .
    git commit -m "feat: Ajout de la fonctionnalité de connexion"
    ```
6.  **Poussez votre branche vers GitHub :**
    ```bash
    git push origin feature-login
    ```
    *La première fois, Git vous dira peut-être de faire `git push --set-upstream origin feature-login` ou `git push -u origin feature-login`. Faites-le si demandé.*

### Étape 3.2 : Ajout d'une Fonctionnalité par l'**Étudiant B**

L'Étudiant B va travailler sur une fonctionnalité de "dashboard".

1.  **Vérifiez que votre branche `main` est à jour :**
    ```bash
    git checkout main
    git pull origin main
    ```
    *Pour l'instant, `main` n'a pas les changements de l'Étudiant A, car ils sont sur sa branche `feature-login`.*
2.  **Créez une nouvelle branche** pour votre fonctionnalité :
    ```bash
    git checkout -b feature-dashboard
    ```
3.  **Modifiez le fichier `app.py` et `index.html` :**
    Ouvrez `app.py` dans votre éditeur et ajoutez une ligne (différente de celle de l'Étudiant A) :
    ```python
    print('Bienvenue dans notre application collaborative !')
    print('Fonctionnalité de tableau de bord ajoutée par Etudiant B.')
    ```
    Ouvrez `index.html` et ajoutez une section de tableau de bord :
    ```html
    <h1>Bienvenue !</h1><p>Cette application est un projet collaboratif.</p>
    <h2>Dashboard</h2>
    <ul>
        <li>Statistique 1</li>
        <li>Statistique 2</li>
    </ul>
    ```
4.  **Vérifiez le statut :**
    ```bash
    git status
    ```
5.  **Ajoutez et "committez" vos changements :**
    ```bash
    git add .
    git commit -m "feat: Implémentation du tableau de bord"
    ```
6.  **Poussez votre branche vers GitHub :**
    ```bash
    git push origin feature-dashboard
    ```

### Étape 3.3 : Créer et Fusionner une Pull Request (PR) (Étudiant A)

L'Étudiant A va proposer ses changements à la branche principale via une Pull Request.

1.  Allez sur la page du dépôt GitHub de l'Étudiant A.
2.  Vous devriez voir une bannière "feature-login had recent pushes..." avec un bouton **"Compare & pull request"**. Cliquez dessus.
    *Si non, cliquez sur l'onglet **"Pull requests"**, puis sur le bouton vert **"New pull request"**. Sélectionnez `base: main` et `compare: feature-login`.*
3.  Donnez un titre significatif à votre PR (ex: "feat: Ajout de la fonctionnalité de connexion") et une description.
4.  Cliquez sur **"Create pull request"**.
5.  La page de la PR s'ouvre. GitHub vérifiera si les branches peuvent être fusionnées automatiquement. Ici, cela devrait être le cas.
6.  **Fusionnez la PR :** Cliquez sur le bouton vert **"Merge pull request"**, puis **"Confirm merge"**.
7.  **(Optionnel) Supprimez la branche :** Après la fusion, vous pouvez cliquer sur le bouton **"Delete branch"** pour nettoyer votre dépôt distant.

### Étape 3.4 : Synchroniser la Branche Principale (Étudiant B)

L'Étudiant B doit récupérer les changements de l'Étudiant A.

1.  Sur sa machine, l'Étudiant B doit s'assurer d'être sur la branche `main` :
    ```bash
    git checkout main
    ```
2.  Puis, récupérer les derniers changements du dépôt distant :
    ```bash
    git pull origin main
    ```
    *Vous devriez voir que les changements de `app.py` et `index.html` de l'Étudiant A ont été récupérés. Ouvrez les fichiers pour vérifier.*

### Étape 3.5 : Créer et Fusionner une Pull Request (PR) (Étudiant B)

Maintenant, l'Étudiant B propose ses changements.

1.  Allez sur la page du dépôt GitHub de l'Étudiant A.
2.  Créez une Pull Request pour la branche `feature-dashboard` (comme l'Étudiant A l'a fait pour `feature-login`).
    *Sélectionnez `base: main` et `compare: feature-dashboard`.*
3.  Donnez un titre (ex: "feat: Implémentation du tableau de bord") et une description.
4.  Cliquez sur **"Create pull request"**.
5.  GitHub indiquera que la fusion est possible.
6.  **Fusionnez la PR :** Cliquez sur **"Merge pull request"**, puis **"Confirm merge"**.
7.  **(Optionnel) Supprimez la branche :** Cliquez sur **"Delete branch"**.

### Étape 3.6 : Synchroniser la Branche Principale (Étudiant A)

L'Étudiant A doit récupérer les changements de l'Étudiant B.

1.  Sur sa machine, l'Étudiant A doit s'assurer d'être sur la branche `main` :
    ```bash
    git checkout main
    ```
2.  Puis, récupérer les derniers changements du dépôt distant :
    ```bash
    git pull origin main
    ```
    *Vous devriez voir que les changements de `app.py` et `index.html` de l'Étudiant B ont été récupérés. Ouvrez les fichiers pour vérifier.*

---

## Partie 4 : Collaboration : Résolution de Conflits de Fusion

Cette partie est cruciale pour comprendre comment Git gère les situations où deux personnes modifient la même partie du même fichier.

### Étape 4.1 : Création d'une Modification Conflicteuse (Étudiant A)

L'Étudiant A va modifier la ligne d'accueil de `app.py` et `index.html`.

1.  **Assurez-vous que votre branche `main` est à jour :**
    ```bash
    git checkout main
    git pull origin main
    ```
2.  **Créez une nouvelle branche** pour votre tâche :
    ```bash
    git checkout -b refactor-welcome-msg
    ```
3.  **Modifiez la première ligne de `app.py` et le titre `<h1>` de `index.html` :**
    *   `app.py` (remplacez la ligne existante) :
        ```python
        print('Hello, world! Bienvenue à tous les collaborateurs !')
        print('Fonctionnalité de login ajoutée par Etudiant A.')
        print('Fonctionnalité de tableau de bord ajoutée par Etudiant B.')
        ```
    *   `index.html` (modifiez le `<h1>`) :
        ```html
        <h1>Notre super App Collaborative !</h1><p>Cette application est un projet collaboratif.</p>
        <h2>Connexion</h2>
        <!-- ... reste du code ... -->
        ```
4.  **Committer les changements :**
    ```bash
    git add .
    git commit -m "refactor: Message de bienvenue mis à jour et titre principal"
    ```
5.  **Poussez votre branche :**
    ```bash
    git push origin refactor-welcome-msg
    ```

### Étape 4.2 : Création d'une Autre Modification Conflicteuse (Étudiant B)

L'Étudiant B va modifier la *même* ligne d'accueil de `app.py` et le *même* titre `<h1>` de `index.html`.

1.  **Très important : Assurez-vous que votre branche `main` est à jour** AVANT de créer une nouvelle branche pour cette tâche.
    ```bash
    git checkout main
    git pull origin main
    ```
    *À ce stade, votre `main` ne contient PAS les changements de `refactor-welcome-msg` de l'Étudiant A, car ils sont sur une autre branche.*
2.  **Créez une nouvelle branche** pour votre tâche :
    ```bash
    git checkout -b update-greetings
    ```
3.  **Modifiez la première ligne de `app.py` et le titre `<h1>` de `index.html` :**
    *   `app.py` (remplacez la ligne existante) :
        ```python
        print('Bienvenue sur notre application collective ! Bon travail à l\'équipe !')
        print('Fonctionnalité de login ajoutée par Etudiant A.')
        print('Fonctionnalité de tableau de bord ajoutée par Etudiant B.')
        ```
    *   `index.html` (modifiez le `<h1>`) :
        ```html
        <h1>Bienvenue sur AppCo ! Une application faite pour vous.</h1><p>Cette application est un projet collaboratif.</p>
        <h2>Connexion</h2>
        <!-- ... reste du code ... -->
        ```
4.  **Committer les changements :**
    ```bash
    git add .
    git commit -m "style: Mise à jour du message d'accueil et du titre"
    ```
5.  **Poussez votre branche :**
    ```bash
    git push origin update-greetings
    ```

### Étape 4.3 : La Première Pull Request (Étudiant A)

L'Étudiant A va créer sa Pull Request.

1.  Allez sur GitHub et créez une PR pour `refactor-welcome-msg` vers `main`.
2.  GitHub devrait indiquer que la fusion est possible (car aucune autre PR n'a été fusionnée sur `main` qui entre en conflit).
3.  **Fusionnez la PR** et supprimez la branche.

### Étape 4.4 : La Deuxième Pull Request et Résolution de Conflits (Étudiant B)

Maintenant, l'Étudiant B va créer sa PR.

1.  Allez sur GitHub et créez une PR pour `update-greetings` vers `main`.
2.  **Observe le conflit :** GitHub devrait indiquer "**This branch has conflicts that must be resolved**". C'est normal !
3.  **Options de résolution :**
    *   **Résolution sur GitHub :** Pour de petits conflits, GitHub offre un éditeur de conflits. Vous pouvez cliquer sur "Resolve conflicts". C'est simple mais limité.
    *   **Résolution locale (méthode recommandée) :** C'est la méthode la plus courante et la plus puissante. C'est ce que nous allons faire.

#### Résolution Locale du Conflit par l'**Étudiant B**

1.  **Mettez à jour votre branche `main` locale :**
    ```bash
    git checkout main
    git pull origin main
    ```
    *Votre `main` contient maintenant les changements de l'Étudiant A de la branche `refactor-welcome-msg`.*
2.  **Basculer sur votre branche de fonctionnalité (`update-greetings`) :**
    ```bash
    git checkout update-greetings
    ```
3.  **Fusionnez `main` dans votre branche de fonctionnalité :**
    ```bash
    git merge main
    ```
    *C'est ici que le conflit se produit. Git vous affichera un message d'erreur et des marqueurs de conflit dans les fichiers.*
    ```
    Auto-merging app.py
    CONFLICT (content): Merge conflict in app.py
    Auto-merging index.html
    CONFLICT (content): Merge conflict in index.html
    Automatic merge failed; fix conflicts and then commit the result.
    ```
4.  **Ouvrez `app.py` et `index.html` dans votre éditeur de texte.** Vous verrez des marqueurs de conflit :
    ```
    <<<<<<< HEAD
    print('Bienvenue sur notre application collective ! Bon travail à l'équipe !')
    =======
    print('Hello, world! Bienvenue à tous les collaborateurs !')
    >>>>>>> main
    print('Fonctionnalité de login ajoutée par Etudiant A.')
    print('Fonctionnalité de tableau de bord ajoutée par Etudiant B.')
    ```
    *   `<<<<<<< HEAD` : C'est votre version (`update-greetings`).
    *   `=======` : La séparation entre les deux versions.
    *   `>>>>>>> main` : C'est la version qui vient de `main` (celle de l'Étudiant A).

5.  **Modifiez les fichiers pour résoudre le conflit :** Choisissez quelle version garder, ou combinez les deux. Supprimez les marqueurs (`<<<<<<<`, `=======`, `>>>>>>>`).
    *Exemple de résolution pour `app.py` (une combinaison) :*
    ```python
    print('Bienvenue sur notre App Collaborative ! Hello World de l\'équipe !')
    print('Fonctionnalité de login ajoutée par Etudiant A.')
    print('Fonctionnalité de tableau de bord ajoutée par Etudiant B.')
    ```
    *Exemple de résolution pour `index.html` :*
    ```html
    <h1>Notre App Collaborative : Bienvenue à Tous !</h1><p>Cette application est un projet collaboratif.</p>
    <h2>Connexion</h2>
    <!-- ... reste du code ... -->
    ```
6.  **Ajoutez les fichiers résolus à la zone de staging :**
    ```bash
    git add app.py index.html
    ```
7.  **Committer la résolution du conflit :**
    ```bash
    git commit -m "fix: Résolution des conflits sur le message de bienvenue"
    ```
    *Git peut pré-remplir un message de commit. Vous pouvez le laisser ou le modifier.*
8.  **Poussez votre branche (`update-greetings`) vers GitHub :**
    ```bash
    git push origin update-greetings
    ```
    *Maintenant, la branche `update-greetings` sur GitHub est à jour et sans conflit.*
9.  **Retournez sur GitHub** à votre Pull Request. GitHub devrait maintenant indiquer "This branch has no conflicts with the base branch".
10. **Fusionnez la PR.**

### Étape 4.5 : Synchroniser la Branche Principale (Étudiant A)

1.  Sur sa machine, l'Étudiant A doit s'assurer d'être sur la branche `main` :
    ```bash
    git checkout main
    ```
2.  Puis, récupérer les derniers changements du dépôt distant :
    ```bash
    git pull origin main
    ```
    *L'Étudiant A a maintenant la version fusionnée et résolue des fichiers. Ouvrez `app.py` et `index.html` pour vérifier.*

---

## Conclusion et Bonnes Pratiques

Félicitations ! Vous avez réalisé un TP complet sur la collaboration Git/GitHub, incluant la gestion des branches, des Pull Requests et la résolution de conflits.

Voici quelques bonnes pratiques à retenir :

*   **`git pull origin main` (ou `master`) souvent :** Avant de commencer une nouvelle tâche ou de créer une nouvelle branche, assurez-vous toujours que votre branche principale est à jour.
*   **Utilisez des branches de fonctionnalité :** Travaillez sur une nouvelle branche pour chaque fonctionnalité ou correctif. Cela isole vos changements et simplifie la collaboration.
*   **Commits fréquents et descriptifs :** Faites des commits souvent et utilisez des messages clairs qui expliquent CE QUI a été fait et POURQUOI.
*   **Pull Requests (PR) :** Utilisez les PR pour proposer vos changements à la branche principale. C'est l'occasion pour vos coéquipiers de revoir votre code avant la fusion.
*   **Communiquez :** En cas de doute ou de conflit potentiel, parlez à vos coéquipiers ! Git est un outil, mais la communication humaine est essentielle.
*   **Ne poussez jamais directement sur `main` (sauf pour des raisons très spécifiques et coordonnées) :** Toujours passer par une branche de fonctionnalité et une PR.

Continuez à pratiquer ! La maîtrise de Git vient avec l'expérience.