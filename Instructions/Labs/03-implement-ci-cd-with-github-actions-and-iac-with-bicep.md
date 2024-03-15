---
lab:
  title: Implémenter l’intégration continue et la livraison continue (CI/CD) avec GitHub Actions et IaC avec Bicep
  module: Deliver with DevOps
---

# Labo 03 – Implémenter CI/CD avec GitHub Actions et IaC avec Bicep

## Durée estimée : 40 minutes

## Scénario

Souvenez-vous du scénario de ce module dans lequel vous travaillez pour une entreprise de développement de logiciels dans le secteur de la vente au détail qui souhaite s’assurer que le processus de publication d’une nouvelle version de son application de magasin en ligne est efficace et fiable tout en réduisant le risque d’erreurs. Étant donné que vous avez décidé d’utiliser GitHub pour faciliter la gestion du cycle de vie des applications, ce laboratoire vous donne la possibilité de dupliquer (fork) et de passer en revue le référentiel GitHub contenant le code source d’une application web, d’un workflow GitHub Actions et d’un modèle Bicep. En outre, vous pourrez configurer l’environnement cible et valider la fonctionnalité Infrastructure as Code (IaC) et l’intégration continue et livraison continue (CI/CD).

## Objectifs

Dans ce labo, vous allez :

- Préparer l’abonnement Azure pour le labo
- Implémenter l’infrastructure en tant que code (IaC) ainsi que l’intégration continue et la livraison continue (CI/CD) à l’aide de GitHub Actions et un modèle Bicep

> **Remarque :** Dans le labo précédent, vous avez configuré GitHub Pages, ce qui signifie que vous avez déjà implémenté le déploiement continu (peut-être même sans vous en rendre compte). GitHub Pages utilise GitHub Actions (en arrière-plan) pour effectuer des déploiements automatisés après tout commit réalisé sur la branche primaire. Vous pouvez vérifier cela en accédant à l’onglet **Actions** de la page principale du dépôt dupliqué (forked) **Spoon-Knife**. À présent, vous allez améliorer cette fonctionnalité. En particulier, vous allez utiliser un workflow GitHub Actions développé sur mesure avec un modèle Bicep pour approvisionner deux applications web Azure App Service dans différentes régions Azure et déployer une application web .NET personnalisée dans les deux.

> **Remarque :** Pour ce labo et les suivants, utilisez le même compte GitHub que vous avez créé pour le premier labo.

## Prérequis

- [Labo 01 – Planification et gestion agiles avec GitHub](01-agile-planning-management-using-github.md) terminé
- [Labo 02 – Implémenter le flux de travail avec GitHub](02-implement-manage-repositories-using-github.md) terminé
- Un abonnement Azure pour lequel vous disposez au moins de l’accès au niveau contributeur. Si vous n’avez pas d’abonnement Azure, vous pouvez vous inscrire à un [essai gratuit](https://azure.microsoft.com/free).

## Exercice 0 : Préparer l’abonnement Azure pour le labo

> **Remarque :** Si vous utilisez un nouvel abonnement Azure, certains fournisseurs de ressources Azure peuvent ne pas être inscrits. Comme le processus d’inscription peut prendre quelques minutes, pour minimiser le temps d’attente, vous les inscrirez au début de ce labo.

> **Remarque :** Dans ce labo, vous allez utiliser Azure Cloud Shell. Si c’est la première fois que vous utilisez Azure Cloud Shell dans votre abonnement, vous devez inscrire le fournisseur de ressources correspondant. 

1. Démarrez un navigateur web et accédez au portail Azure à l’adresse `https://portal.azure.com`.
1. Si vous y êtes invité, connectez-vous à l’aide de votre compte Microsoft Entra ID avec l’accès Propriétaire à l’abonnement Azure disponible.
1. Sous l’onglet de navigateur web affichant le portail Azure, dans la zone de texte de recherche située en haut de la page, entrez **Abonnements**, puis, dans la liste des résultats, sélectionnez **Abonnements**.
1. Dans la page **Abonnements**, sélectionnez l’abonnement que vous souhaitez utiliser dans ce labo.
1. Dans la page des abonnements, dans le menu vertical situé à gauche, sélectionnez **Fournisseurs de ressources**.
1. Dans la liste des fournisseurs de ressources, recherchez et sélectionnez **Microsoft.CloudShell**.
1. Une fois le fournisseur de ressources **Microsoft.CloudShell** sélectionné, dans la barre d’outils, sélectionnez **Inscrire**.

   > **Remarque :** Passez directement à l’exercice suivant sans attendre la fin de la procédure d’inscription. L’inscription peut prendre quelques minutes.

## Exercice 1 : Implémenter l’infrastructure en tant que code (IaC) et l’intégration continue et la livraison continue (CI/CD) avec GitHub Actions et un modèle Bicep

Dans cet exercice, vous allez implémenter IaC avec CI/CD dans des applications web Azure App Service avec GitHub Actions et un modèle Bicep.

> **Remarque :** Pour simplifier la configuration initiale, vous allez utiliser une duplication (fork) d’un dépôt GitHub existant qui contient le code source de l’application .NET, le workflow GitHub Actions et le modèle Bicep.

L’exercice se compose des tâches suivantes :

- Tâche 1 : Dupliquer et passer en revue le dépôt GitHub contenant le code source d’une application web, un workflow GitHub Actions et un modèle Bicep
- Tâche 2 : Configurer l’environnement cible
- Tâche 3 : Valider IaC et CI/CD sur le plan fonctionnel

### Tâche 1 : Dupliquer et passer en revue le dépôt GitHub contenant le code source d’une application web, un workflow GitHub Actions et un modèle Bicep

1. Passez à la fenêtre de navigateur affichant votre compte GitHub et vérifiez que vous êtes toujours authentifié (si ce n’est pas le cas, connectez-vous à l’aide de votre compte d’utilisateur GitHub).
1. Ouvrez un autre onglet dans la même fenêtre de navigateur et accédez au dépôt [eShopOnWeb](https://github.com/MicrosoftLearning/eShopOnWeb) qui héberge la version .NET d’un exemple de site web, un workflow GitHub Actions et un modèle Bicep.
1. Dans la page du dépôt **eShopOnWeb**, sélectionnez **Fork** (Dupliquer).
1. Dans la page **Create a new fork** (Créer une fourche), vérifiez que l’entrée **Owner** (Propriétaire) dans la liste déroulante affiche votre nom d’utilisateur GitHub. Acceptez l’entrée par défaut **eShopOnWeb** dans la zone de texte **Repository name** (Nom du dépôt), confirmez que la case **Copy the main branch only** (Copier la branche primaire uniquement) est cochée, puis sélectionnez **Create fork** (Créer une fourche).

   > **Remarque :** Votre session de navigateur est automatiquement redirigée vers le dépôt qui vient d’être dupliqué.

1. Dans la page **eShopOnWeb**, vérifiez que la liste déroulante affiche la branche actuelle **main**.
1. Passez en revue la structure du dépôt et notez qu’elle inclut les composants suivants :

   - Répertoire **.github/workflows** contenant plusieurs workflows YAML, dont un nommé **eshoponweb-cicd.yml**
   - Répertoire **infra** contenant le modèle Bicep nommé **webapp.bicep**
   - Répertoire **src/Web** contenant le code .NET de l’application web source

1. Ouvrez le répertoire **.github/workflows**, puis sélectionnez le fichier **eshoponweb-cicd.yml** pour afficher son contenu. Notez que ce workflow GitHub Actions contient les travaux **buildandtest** et **deploy**, qui comprennent les étapes suivantes :

   - buildandtest
      - effectuer un basculement sur une branche du dépôt
      - préparer l’exécuteur pour la version souhaitée du Kit de développement logiciel (SDK) .NET
      - générer, tester et publier le projet .NET
      - charger les artefacts de code de site web publiés
      - charger le modèle bicep en tant qu’artefact pour le travail suivant
   - deploy
      - télécharger les artefacts publiés créés dans le travail précédent
      - télécharger le modèle bicep chargé dans le travail précédent
      - se connecter à l’abonnement Azure cible à l’aide d’un principal de service
      - déployer une application web Azure App Service à l’aide du modèle bicep
      - publier les artefacts de site web dans l’application web Azure App Service

   > **Remarque :** Ne vous attardez pas sur les détails de chaque travail. Il est beaucoup plus important à ce stade de comprendre simplement l’objectif du workflow.

1. Notez que le processus d’approvisionnement des ressources Azure cible un seul groupe de ressources qui est défini au début du workflow en tant que variable d’environnement `RESOURCE-GROUP`.

   > **Remarque :** Vous devez créer le groupe de ressources avant d’exécuter le workflow.

1. Notez que le workflow s’appuie sur un secret pour s’authentifier auprès de l’abonnement Azure cible pendant l’étape Configurer Azure CLI, comme le montre l’instruction `creds: ${{ secrets.AZURE_CREDENTIALS }}`.

   > **Remarque :** Vous devez configurer ce secret avant d’exécuter le workflow.

1. Notez également que vous pouvez configurer ce workflow pour être déclenché manuellement, comme le montre la syntaxe `on: workflow_dispatch:`.

### Tâche 2 : Configurer l’environnement cible

> **Remarque :** Vous allez commencer par créer les groupes de ressources. Vous exécuterez le workflow à deux reprises pour déployer deux instances du site web dans deux régions Azure différentes.

1. Passez à l’onglet de navigateur web affichant le portail Azure à l’adresse `https://portal.azure.com`.
1. Dans le portail Azure, dans la zone de texte de recherche située en haut de la page, entrez **Groupes de ressources**, puis sélectionnez **Groupes de ressources** dans la liste des résultats.
1. Dans la page **Groupes de ressources**, sélectionnez **+ Créer**.
1. Dans la zone de texte **Groupes de ressources**, entrez **rg-eshoponweb-westeurope**.
1. Dans la liste déroulante **Région**, sélectionnez **(Europe) Europe Ouest**.
1. Sélectionnez **Vérifier + créer**, puis, dans **Vérifier + créer**, sélectionnez **Créer**.
1. Dans la page **Groupes de ressources**, sélectionnez **+ Créer**.
1. Dans la zone de texte **Groupes de ressources**, entrez **rg-eshoponweb-eastus**.
1. Dans la liste déroulante **Région**, sélectionnez **(US) USA Est**.
1. Sélectionnez **Vérifier + créer**, puis, dans **Vérifier + créer**, sélectionnez **Créer**.

    > **Remarque :** Vous allez ensuite créer un principal de service qui sera utilisé pour l’authentification à partir du workflow GitHub Actions auprès de l’abonnement Azure cible et lui attribuer le rôle de Contributeur dans l’abonnement.

1. Dans le portail Azure, sélectionnez l’icône **Cloud Shell** à droite de la zone de texte de recherche.
1. Si nécessaire, sélectionnez **Bash** dans le menu déroulant dans le coin supérieur gauche du volet Cloud Shell.
1. Dans la session Bash du volet Cloud Shell, exécutez la commande suivante pour stocker la valeur de votre ID d’abonnement Azure dans une variable :

   ```cli
   SUBCRIPTION_ID=$(az account show --query id --output tsv) 
   echo $SUBCRIPTION_ID
   ```

1. Copiez la valeur de l’ID d’abonnement retourné par la deuxième commande et enregistrez-la. Vous en aurez besoin plus loin dans cet exercice.
1. Dans la session Bash du volet Cloud Shell, exécutez la commande suivante pour créer un principal de service Microsoft Entra ID et lui attribuer le rôle de Contributeur dans l’étendue de l’abonnement :

   ```cli
   az ad sp create-for-rbac --name "devopsfoundationslabsp" --role contributor --scopes /subscriptions/$SUBCRIPTION_ID --json-auth
   ```

1. Copiez l’intégralité de la sortie au format JSON de la commande et enregistrez-la. Vous en aurez besoin rapidement. Le format de la sortie doit ressembler au texte suivant :

   ```json
   {
     "clientId": "cccccccc-cccc-cccc-cccc-cccccccccccc",
     "clientSecret": "xxxxx~xxxxxxxxxxxx-xxxxxxxxxxxxx_xxxxxxx",
     "subscriptionId": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
     "tenantId": "zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

1. Passez à la fenêtre de navigateur web affichant la page du dépôt GitHub **eShopOnWeb** dupliqué, puis, dans la barre d’outils, sélectionnez **Settings** (Paramètres).
1. Dans le menu vertical de gauche, dans la section **Security** (Sécurité), sélectionnez **Secrets and variables** (Secrets et variables), puis, dans la liste déroulante, sélectionnez **Actions**.
1. Dans le volet **Actions secrets and variables** (Secrets et variables d’actions), sélectionnez **New repository secret** (Nouveau secret de dépôt).
1. Dans le volet **Actions secrets / New secret** (Secrets d’actions / Nouveau secret), dans la zone de texte **Name** (Nom), entrez **AZURE_CREDENTIALS**.
1. Dans la zone de texte **Secret**, collez le texte au format JSON que vous avez enregistré précédemment dans cette tâche et sélectionnez **Add secret** (Ajouter le secret).
1. Revenez au navigateur web affichant la session Bash dans le volet Cloud Shell, puis exécutez la commande suivante pour générer le nom de la première application web App Service que vous allez déployer :

   ```cli
   echo devops-webapp-westeurope-$RANDOM$RANDOM
   ```

1. Copiez la valeur retournée par la commande et enregistrez-la. Vous en aurez besoin plus loin dans cet exercice.
1. Dans la session Bash du volet Cloud Shell, exécutez la commande suivante pour générer le nom de la deuxième application web App Service que vous allez déployer :

   ```cli
   echo devops-webapp-eastus-$RANDOM$RANDOM
   ```

1. Copiez la valeur retournée par la commande et enregistrez-la. Vous en aurez besoin plus loin dans cet exercice.

### Tâche 3 : Valider IaC et CI/CD sur le plan fonctionnel

1. Passez à la fenêtre de navigateur web affichant la page du dépôt GitHub **eShopOnWeb** dupliqué. Si nécessaire, dans la page **eShopOnWeb**, cliquez sur l’onglet **Code**, puis, dans la liste déroulante, confirmez que la branche actuelle est **main**.
1. Dans la fenêtre de navigateur web affichant la branche **main** de la page du dépôt GitHub **eShopOnWeb** dupliqué, accédez au dossier **.github/workflows** et sélectionnez **eshoponweb-cicd.yml**.
1. Dans le volet **.github/workflows/eshoponweb-cicd.yml**, sélectionnez l’icône en forme de crayon pour modifier le workflow.
1. Dans le volet **Edit** (Modifier), remplacez la ligne 4 par le texte suivant :

   ```yaml
   on: workflow_dispatch
   ```

    >**Remarque** : Veillez à utiliser une mise en retrait appropriée.

1. Dans le volet **Edit** (Modifier), remplacez la ligne 8 par le texte suivant :

   ```yaml
    RESOURCE-GROUP: rg-eshoponweb-westeurope
   ```

1. Dans le volet **Edit** (Modifier), remplacez l’espace réservé `YOUR-SUBS-ID` à la ligne 11 par la valeur de l’ID d’abonnement Azure que vous avez enregistrée précédemment dans cet exercice :
1. Dans le volet **Edit** (Modifier), remplacez l’espace réservé `eshoponweb-webapp-NAME` à la ligne 11 par le nom de la **première** application web Azure App Service que vous avez générée précédemment dans cet exercice.
1. Dans le volet **.github/workflows/eshoponweb-cicd.yml**, sélectionnez **Commit changes** (Commiter les modifications), puis sélectionnez à nouveau **Commit changes**.
1. Dans la fenêtre de navigateur web affichant la branche **main** de la page du dépôt GitHub **eShopOnWeb** dupliqué, accédez au dossier **infra** et sélectionnez **webapp.bicep**.
1. Dans le volet **infra/webapp.bicep**, sélectionnez l’icône en forme de crayon pour modifier le workflow.
1. Dans le volet **Edit** (Modifier), remplacez la ligne 2 par le texte suivant :

   ```yaml
   param sku string = 'S1' // The SKU of App Service Plan
   ```

1. Dans le volet **infra/webapp.bicep**, sélectionnez **Commit changes** (Commiter les modifications), puis sélectionnez à nouveau **Commit changes**.
1. Dans la fenêtre de navigateur web affichant la page du dépôt GitHub **eShopOnWeb** dupliqué, sélectionnez **Actions**.
1. Si vous êtes invité à activer GitHub Actions, sélectionnez **I understand my workflows, go ahead and enable them** (Je comprends mes workflows et je souhaite les activer).
    >**Remarque** : Ce comportement est attendu, car par défaut, GitHub désactive les workflows dans un dépôt dupliqué pour votre propre sécurité.
1. Dans la section **All workflows** (Tous les workflows) côté gauche, sélectionnez **eShopOnWeb Build and Test**.
1. Dans le volet **eShopOnWeb Build and Test**, sélectionnez **Run workflow** (Exécuter le workflow). Dans la liste déroulante, confirmez que la branche main (**Branch: main**) est sélectionnée, puis sélectionnez **Run workflow** une nouvelle fois.

    >**Remarque** : Cette opération doit déclencher une exécution de workflow.

1. Sélectionnez l’exécution de workflow **eShopOnWeb Build and Test**.

    >**Remarque** : Si nécessaire, actualisez la page pour voir la dernière exécution de workflow.

1. Dans le volet **eShopOnWeb Build and Test #1**, sélectionnez **buildandtest**.
1. Surveillez la progression du workflow et vérifiez que toutes les étapes du travail **buildandtest** aboutissent.
1. Une fois ce travail terminé, dans la section **Summary** (Résumé), sélectionnez **deploy**.
1. Surveillez la progression du workflow et vérifiez que toutes les étapes du travail **deploy** aboutissent.

    >**Remarque** : Si l’une des étapes échoue, dans le coin supérieur droit de la page qui affiche la progression du workflow, sélectionnez **Re-run all jobs** (Réexécuter tous les travaux), puis, dans le volet **Re-run all jobs**, sélectionnez **Re-run jobs** (Réexécuter les travaux).

1. Passez à la fenêtre de navigateur web affichant la page du dépôt GitHub **eShopOnWeb** dupliqué. Si nécessaire, dans la page **eShopOnWeb**, dans la liste déroulante, confirmez que la branche actuelle est **main**.
1. Dans la fenêtre de navigateur web affichant la branche **main** de la page du dépôt GitHub **eShopOnWeb** dupliqué, accédez au dossier **.github/workflows** et sélectionnez **eshoponweb-cicd.yml**.
1. Dans le volet **.github/workflows/eshoponweb-cicd.yml**, sélectionnez l’icône en forme de crayon pour modifier le workflow.
1. Dans le volet **Edit** (Modifier), remplacez la ligne 8 par le texte suivant :

   ```yaml
    RESOURCE-GROUP: rg-eshoponweb-eastus
   ```

1. Dans le volet **Edit** (Modifier), remplacez l’espace réservé `eshoponweb-webapp-NAME` à la ligne 11 par le nom de la **deuxième** application web Azure App Service que vous avez générée précédemment dans cet exercice.
1. Dans le volet **.github/workflows/eshoponweb-cicd.yml**, sélectionnez **Commit changes** (Commiter les modifications), puis sélectionnez à nouveau **Commit changes**.
1. Dans la fenêtre de navigateur web affichant la page du dépôt GitHub **eShopOnWeb** dupliqué, sélectionnez **Actions**.
1. Dans la section **All workflows** (Tous les workflows) côté gauche, sélectionnez **eShopOnWeb Build and Test**.
1. Dans le volet **eShopOnWeb Build and Test**, sélectionnez **Run workflow** (Exécuter le workflow). Dans la liste déroulante, confirmez que la branche main (**Branch: main**) est sélectionnée, puis sélectionnez **Run workflow** une nouvelle fois.

    >**Remarque** : Cette opération doit déclencher une exécution de workflow.

1. Sélectionnez l’exécution de workflow **eShopOnWeb Build and Test**.
1. Dans le volet **eShopOnWeb Build and Test #2**, sélectionnez **buildandtest**.
1. Surveillez la progression du workflow et vérifiez que toutes les étapes du travail **buildandtest** aboutissent.
1. Une fois ce travail terminé, dans la section **Summary** (Résumé), sélectionnez **deploy**.
1. Surveillez la progression du workflow et vérifiez que toutes les étapes du travail **deploy** aboutissent.

    >**Remarque** : Si l’une des étapes échoue, dans le coin supérieur droit de la page qui affiche la progression du workflow, sélectionnez **Re-run all jobs** (Réexécuter tous les travaux), puis, dans le volet **Re-run all jobs**, sélectionnez **Re-run jobs** (Réexécuter les travaux).

1. Dans la fenêtre de navigateur web affichant le portail Azure, dans la zone de texte de recherche située en haut de la page, entrez **App Services**, puis sélectionnez **App Services** dans la liste des résultats.
1. Dans la page **App Services**, dans la liste des services d’application, sélectionnez le service d’application **devops-webapp-westeurope-** créé précédemment dans cet exercice.
1. Dans la page **devops-webapp-westeurope-**, dans la section **Essentials** (Informations de base), vérifiez que la valeur **Default domain** (Domaine par défaut) est affichée et sélectionnez-la pour ouvrir l’application web dans un nouvel onglet de navigateur.
1. Sous le nouvel onglet de navigateur, vérifiez que l’application web est affichée et qu’elle est fonctionnelle. Vous pouvez également vérifier la deuxième application web dans la région **USA Est** de la même façon.
    >**Remarque** : Laissez les ressources Azure déployées en cours d’exécution. Vous en aurez besoin dans le prochain labo.
