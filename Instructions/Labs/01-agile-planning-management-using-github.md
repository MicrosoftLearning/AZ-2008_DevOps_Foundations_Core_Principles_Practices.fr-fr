---
lab:
  title: Planification et gestion agiles avec GitHub
  module: Plan with DevOps
---

# Labo 01 : planification Agile et gestion en tirant parti de GitHub

## Durée estimée : 40 minutes

## Scénario

Souvenez-vous du scénario de ce module, dans lequel vous travaillez pour une entreprise de développement de logiciels dans le secteur de la vente au détail qui envisage de migrer son magasin en ligne vers une nouvelle application, mais qui rencontre des difficultés lors de la planification du projet en raison du manque de collaboration et de communication entre les équipes de développement et d’exploitation. Comme vous avez décidé d’utiliser GitHub pour la planification et la gestion agiles, ce labo vous offre l’opportunité de créer un dépôt GitHub, des jalons et des problèmes associés, un projet et un tableau de projet. En outre, vous pourrez ajouter un brouillon d’élément au tableau de projet et un élément basé sur un problème, et passer en revue les paramètres d’automatisation.

## Objectifs

Dans ce labo, vous allez :

- Créer un dépôt GitHub, un projet et un tableau de projet
- Créer et gérer des éléments de tableau de projet

> **Remarque :** Pour ce labo et les suivants, vous aurez besoin d’un compte GitHub. Nous vous recommandons vivement de créer un compte distinct pour suivre les labos. Pour créer un compte GitHub, suivez les instructions de l’article [Créer un compte sur GitHub](https://docs.github.com/get-started/quickstart/creating-an-account-on-github).

## Exercice 1 : Créer un dépôt GitHub, un projet et un tableau de projet

Dans cet exercice, vous allez créer un référentiel GitHub, un projet et un tableau de projet.

> **Remarque :** Selon la documentation GitHub, Projects est un *outil flexible et adaptable qui permet de planifier et de suivre le travail sur GitHub*. Les projets permettent d’accéder aux *tableaux*, qui servent le rôle de tableaux *kanban*. Kanban est une méthode bien établie et largement adoptée dans un contexte agile pour visualiser l’avancement du travail dans un projet.

L’exercice se compose des tâches suivantes :

- Tâche 1 : Créer un référentiel GitHub
- Tâche 2 : Créer des jalons et des problèmes GitHub
- Tâche 3 : Créer un projet GitHub
- Tâche 4 : Créer un tableau de projet GitHub

### Tâche 1 : Créer un référentiel GitHub

1. Démarrez un navigateur web et accédez à la page d’accueil de [GitHub](https://github.com).
1. Lorsque vous êtes invité à vous authentifier, connectez-vous avec votre compte utilisateur GitHub.
1. Dans la page principale de GitHub, sélectionnez l’onglet **Référentiels**, puis choisissez **Nouveau**.
1. Dans la page **Créer un référentiel**, effectuez les actions suivantes :

   - Dans la liste déroulante **Propriétaire**, sélectionnez le nom d’utilisateur de votre compte GitHub.
   - Dans la zone **Nom du référentiel**, entrez **`DevOpsCoreIntroRepo`**.
   - Passez la visibilité du référentiel sur **Privé**.
   - Activez la case à cocher **Ajouter un fichier README**.
   - Dans la liste déroulante **Ajouter .gitignore**, sélectionnez **Visual Studio**.

   > **Remarque :** Pour en savoir plus sur .gitignore, reportez-vous à [Ignorer les fichiers](https://docs.github.com/get-started/getting-started-with-git/ignoring-files).

   - Dans la liste déroulante **Choisir une licence**, sélectionnez **MIT**.

   > **Remarque :** Pour en savoir plus sur les sélections de licences, reportez-vous à [Gestion des licences d’un référentiel](https://docs.github.com/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository).

1. Cliquez sur **Create repository** (Créer le dépôt).

### Tâche 2 : Créer des jalons et des problèmes GitHub

1. Dans la page **DevOpsCoreIntroRepo**, sélectionnez l’onglet **Problèmes**.

   > **Remarque :** Vous allez créer un jalon et un problème associés au référentiel nouvellement créé. Vous les utiliserez ultérieurement en travaillant sur le tableau de projet.

1. Dans la page **Problèmes**, à gauche du bouton **Nouveau problème**, sélectionnez **Jalons**.
1. Dans la page **Jalons**, sélectionnez **Nouveau jalon**.
1. Dans la page **Nouveau jalon**, effectuez les actions suivantes :

   - Dans la zone de texte **Titre**, entrez **`alpha release`**.
   - Dans la zone de texte **Date d’échéance (facultatif)**, indiquez la date correspondant à une semaine avant la date actuelle.
   - Dans la zone de texte **Description**, entrez **`Completion of the alpha release`**.

1. Sélectionnez **Créer un jalon**.
1. Répétez les trois dernières étapes pour créer un jalon **version bêta** avec la date d’échéance correspondant à deux semaines avant la date actuelle. Dans la zone de texte **Description**, entrez **`Completion of the beta release`**.
1. Revenez à la page **Problèmes**, puis sélectionnez **Nouveau problème**.
1. Dans la zone de texte **Ajouter un titre**, entrez **`Repo README page is empty`**.
1. Dans la zone de texte **Ajouter une description**, entrez **`Brevity might be a virtue, but this README page can really use some text`**.
1. Sélectionnez l’icône d’engrenage à côté de l’entrée **Jalon**, puis dans la liste déroulante, choisissez **version alpha**.
1. Sélectionnez l’icône d’engrenage à côté de l’entrée **Étiquettes**, puis dans la liste déroulante, choisissez **bogue**.
1. Sélectionnez **Soumettre un nouveau problème**. Vous remarquerez que le problème a été automatiquement étiqueté **#1**.

### Tâche 3 : Créer un projet GitHub

1. Dans la page principale de GitHub, sélectionnez l’avatar à droite de la barre d’outils, puis choisissez **Vos projets** dans le menu déroulant.
1. Sélectionnez **Nouveau projet**.
1. Dans le volet **Sélectionner un modèle**, choisissez **Planification d’équipe**, puis **Créer un projet**.  

   > **Remarque :** Vous avez également la possibilité de démarrer de zéro et de visualiser le projet sous forme de tableau, de liste ou de feuille de route.

1. Dans la page du nouveau projet, sélectionnez le nom du projet généré automatiquement. Le panneau **Créer une image** s’affiche automatiquement.
1. Dans la zone de texte **Nom du projet**, entrez **`DevOps Core Intro Project`**.
1. Dans la zone de texte **Description courte**, entrez **`Introduction to GitHub Projects`**, puis sélectionnez **Enregistrer**.
1. Dans la section **README**, copiez le texte suivant.

   > **Remarque :** La section **README** comprend un éditeur Markdown simplifié, qui vous aide à créer une page README visuellement attrayante pour le projet. Vous pouvez utiliser les icônes de la barre d’outils pour mettre en forme le texte, puis accéder à l’onglet **Aperçu** pour examiner les modifications. Copiez et collez le texte suivant dans la section README de l’éditeur de texte :

   ```text
   ### Welcome to DevOps Core Intro Project ###

   **Projects are a customizable, flexible tool for planning and tracking your work.**

   To find out more, refer to GitHub documentation [about Projects](https://docs.github.com/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects).
   ```

1. Sélectionnez **Aperçu** pour examiner la page correspondante.
1. Sélectionnez **Enregistrer**.
1. Faites défiler la page jusqu’à **Zone de danger ** et **repérez** l’option vous permettant de basculer la visibilité du projet entre **Privé** et **Public**. Ensuite, fermez le projet et supprimez-le.

   > **Remarque :** À ce stade, ne fermez pas le projet et ne le supprimez pas. Notez uniquement les options disponibles.

1. Faites défiler la page jusqu’en haut et remarquez que les options de **gestion des accès** au projet et de création des champs personnalisés dans l’interface du projet sont désormais affichées.
1. Sélectionnez la flèche gauche dans l’étiquette **<- Paramètres** pour quitter la page **Paramètres** .

### Tâche 4 : Créer un tableau de projet GitHub

1. Dans la page **Projet de présentation de DevOps Core**, sélectionnez la flèche vers le bas à côté de l’onglet **Backlog** et, dans la section **Mise en page**, sélectionnez **Tableau**.

   > **Remarque :** Vous pouvez utiliser cette option pour basculer facilement entre les vues table, tableau et feuille de route.

1. Examinez la page résultante qui se compose de trois colonnes prédéfinies étiquetées comme suit :

   - **À faire** – éléments en attente de traitement
   - **En cours** –éléments en cours de traitement
   - **Terminé** – éléments déjà traités

   > **Remarque :** Cette mise en page représente un tableau kanban très simple. Dans chaque colonne, vous pouvez ajouter des éléments individuels. Vous pouvez également ajouter des colonnes supplémentaires.

1. Pour ajouter une colonne supplémentaire, sélectionnez l’icône **+** à droite de la colonne **Terminé**, puis cliquez sur **+ Nouvelle colonne**.
1. Dans la fenêtre **Nouvelle option**, dans la zone de texte **Texte d’étiquette**, entrez **`Review In Progress`**, puis choisissez une couleur à attribuer à la colonne. Dans la zone de texte **Description**, entrez **`This item is being reviewed`**, puis sélectionnez **Enregistrer**.
1. Sélectionnez le petit cercle à côté de l’étiquette **Vérification en cours** de la colonne nouvellement ajoutée pour la faire glisser entre les colonnes **En cours** et **Terminé**.

## Exercice 2 : Créer et gérer des éléments de tableau de projet

Dans cet exercice, vous allez créer et gérer des éléments dans le tableau de projet.

> **Remarque :** Il existe deux façons principales d’ajouter des éléments à un tableau de projet. Vous pouvez créer un brouillon d’élément ou ajouter un élément représentant un problème existant dans un référentiel GitHub.

L’exercice se compose des tâches suivantes :

- Tâche 1 : Ajouter un brouillon
- Tâche 2 : Ajouter un élément en fonction d’un problème
- Tâche 3 : Examiner les paramètres d’automatisation

### Tâche 1 : Ajouter un brouillon

1. Dans la colonne **À faire** de la page **Projet de présentation de DevOps Core**, sélectionnez **+ Ajouter un élément**.

   > **Remarque :** Dans la zone de texte qui s’affiche automatiquement, vous pouvez commencer à saisir du texte pour créer un brouillon ou taper **#** pour référencer un problème existant dans l’un de vos référentiels GitHub. Nous allons commencer par aborder la première de ces deux techniques.

1. Dans la zone de texte, saisissez **`Missing Wiki`**, puis appuyez sur la touche **Entrée** du clavier. Un nouveau brouillon est ajouté à la colonne **À faire**.
1. Dans le nouveau brouillon, sélectionnez les points de suspension et, dans le menu déroulant, choisissez **Convertir en problème**.
1. Dans la liste déroulante **Sélectionner un élément**, choisissez **DevOpsCoreIntroRepo** pour ajouter l’élément au référentiel que vous avez créé dans l’exercice précédent. Vous remarquerez que le problème a été automatiquement étiqueté **#2**.
1. Sélectionnez le problème **Wiki manquant**.
1. Dans le volet **Wiki manquant #2**, vous remarquerez que des paramètres de configuration supplémentaires sont désormais disponibles, y compris les étiquettes et les jalons.
1. Sélectionnez **Ajouter des étiquettes** et, dans la liste déroulante **Sélectionner des éléments**, choisissez **amélioration**.
1. Sélectionnez **Ajouter un jalon** et, dans la liste déroulante **Sélectionner un élément**, choisissez **version alpha**.
1. Fermez le volet **Wiki manquant #2**.

   > **Remarque :** À présent, vous allez ajouter un autre brouillon et le convertir en problème.

1. Dans la colonne **À faire** de la page **Projet de présentation de DevOps Core**, sélectionnez **+ Ajouter un élément**.
1. Dans la zone de texte, saisissez **`Additional collaborators needed`**, puis appuyez sur la touche **Entrée** du clavier. Un nouveau brouillon est ajouté à la colonne **À faire**.
1. Dans le nouveau brouillon, sélectionnez les points de suspension et, dans le menu déroulant, choisissez **Convertir en problème**, puis sélectionnez **DevOpsCoreIntroRepo** pour ajouter l’élément au référentiel.

### Tâche 2 : Ajouter un élément en fonction d’un problème

1. Dans la colonne **À faire** de la page **Projet de présentation de DevOps Core**, sélectionnez **+ Ajouter un élément**.
1. Dans la zone de texte, saisissez **#** pour afficher une liste des référentiels existants. Dans la liste, sélectionnez **DevOpsCoreIntroRepo**.
1. Ensuite, dans la liste des problèmes existants, sélectionnez **La page README du référentiel est vide #1**. Ce problème est automatiquement ajouté à la colonne **À faire**.
1. Sélectionnez **La page README du référentiel est vide** pour afficher le volet correspondant.
1. Dans le volet **La page README du référentiel est vide**, dans la section Personnes responsables, sélectionnez **Ajouter des personnes responsables...**. Ensuite, dans la liste déroulante **Sélectionner des éléments**, sélectionnez votre nom d’utilisateur GitHub, puis fermez le volet **La page README du référentiel est vide**.
1. Faites glisser l’élément **La page README du référentiel est vide** de la colonne **À faire** à la colonne **En cours**.

### Tâche 3 : Examiner les paramètres d’automatisation

1. Dans la page **Projet de présentation de DevOps Core**, sélectionnez les points de suspension situés juste sous l’avatar.
1. Dans la liste déroulante, sélectionnez **Workflows**.
1. Dans la page **Workflows**, dans le menu de gauche, examinez la liste des workflows par défaut. '

   > **Remarque :** Par défaut, les workflows **Élément fermé** et **Demande de tirage (pull request) fusionnée** sont activés.

1. Sélectionnez **Élément fermé** et passez en revue le workflow. Le workflow définit automatiquement l’état d’un problème sur **Terminé**, dès lors que cet élément est fermé.
1. Sélectionnez l’avatar en haut à droite de la page et, dans le menu déroulant, choisissez **Vos référentiels**.
1. Dans la liste des référentiels, sélectionnez **DevOpsCoreIntroRepo**.
1. Dans la section **README**, sélectionnez l’icône du crayon pour ouvrir le fichier en mode d’édition.
1. Remplacez le contenu existant du fichier par le texte suivant :

   ```text
   ### Welcome to DevOps Core Intro Project Repository ###

   **Projects are a customizable, flexible tool for planning and tracking your work.**

   To find out more, refer to GitHub documentation [about Projects](https://docs.github.com/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects).
   ```

1. Sélectionnez **Valider les modifications**.
1. Dans la fenêtre **Valider les modifications**, acceptez les paramètres par défaut et **validez les modifications** à nouveau.
1. Sélectionnez l’onglet **Problèmes**.
1. Sélectionnez **La page README du référentiel est vide**.
1. Dans la page **La page README du référentiel est vide #1**, sélectionnez **Fermer le problème**.
1. Remarquez que la fermeture de l’élément a entraîné les actions suivantes :

   - L’état de l’élément a été automatiquement modifié en **Terminé**, comme mentionné par un commentaire supplémentaire indiquant que le bot **github-project-automation** a déplacé l’élément de **En cours** vers **Terminé** dans **Projet de présentation de DevOps Core**.
   - Le jalon **version alpha** a été marqué comme terminé à 50 %, comme indiqué par la barre horizontale verte qui s’affiche dans la section **Jalon** de la page.

   > **Remarque :** Si vous ne voyez pas les modifications, actualisez la page.

1. Pour vérifier cela, retournez à la vue tableau du **Projet de présentation de DevOps Core**. Vous remarquerez que l’élément **La page README du référentiel est vide** s’affiche désormais dans la colonne **Terminé**.
