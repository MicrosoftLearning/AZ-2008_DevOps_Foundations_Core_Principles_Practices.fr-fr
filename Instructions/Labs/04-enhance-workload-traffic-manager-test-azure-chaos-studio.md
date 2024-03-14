---
lab:
  title: Améliorer la résilience des charges de travail à l’aide de Traffic Manager et tester la résilience à l’aide d’Azure Chaos Studio
  module: Operate with DevOps
---

# Lab 04 - Améliorer la résilience des charges de travail à l’aide de Traffic Manager et tester la résilience à l’aide d’Azure Chaos Studio

## Durée estimée : 35 minutes

## Scénario

Souvenez-vous du scénario de ce module dans lequel vous travaillez pour une entreprise de développement de logiciels dans le secteur de la vente au détail qui rencontre des temps d’arrêt et des problèmes de performances fréquents avec leur nouvelle application de magasin en ligne. Puisque vous avez décidé d’améliorer la résilience des charges de travail et des tests à l’aide de, respectivement, Traffic Manager et Azure Chaos Studio, ce laboratoire vous donne la possibilité d’implémenter un profil Traffic Manager et de valider la fonctionnalité Traffic Manager, de configurer l’environnement Azure Chaos Studio et d’implémenter et de valider une expérience.

## Objectifs

Dans ce labo, vous allez :

- Préparer l’abonnement Azure pour le labo
- Améliorer la résilience des charges de travail à l’aide de Traffic Manager
- Tester la résilience des charges de travail à l’aide d’Azure Chaos Studio
- Supprimer les ressources utilisées dans les labos

> **Remarque :** Dans le labo précédent, vous avez déployé deux instances d’une application web .NET dans deux régions Azure. Dans ce labo, vous allez d’abord créer une configuration résiliente qui implémente la fonctionnalité d’équilibrage de charge d’Azure Traffic Manager entre les deux instances d’application web. Ensuite, vous allez utiliser Azure Chaos Studio pour déclencher une défaillance de l’une des applications pour tester la résilience de l’implémentation d’équilibrage de charge.

> **Remarque :** Pour ce labo, utilisez le même compte GitHub que celui que vous avez utilisé dans tous les laboratoires précédents.

## Prérequis

- Terminé [Lab 01 - Planification et gestion agiles à l’aide de GitHub](01-agile-planning-management-using-github.md)
- Terminé [Lab 02 - Implémenter le flux de travail avec GitHub](02-implement-manage-repositories-using-github.md)
- Terminé [Lab 03 - Implémenter CI/CD avec GitHub Actions et IaC avec Bicep](03-implement-ci-cd-with-github-actions-and-iac-with-bicep.md)
- Un abonnement Azure auquel vous disposez de l’accès au niveau du propriétaire

## Exercice 0 : Préparer l’abonnement Azure pour le labo

> **Remarque :** Si vous utilisez un nouvel abonnement Azure, certains fournisseurs de ressources Azure peuvent ne pas être inscrits. Comme le processus d'inscription peut prendre quelques minutes, pour minimiser le temps d'attente, vous les inscrirez au début de ce laboratoire.

> **Remarque :** Dans ce laboratoire, vous allez utiliser Azure Chaos Studio. Si c’est la première fois que vous implémentez Azure Chaos Studio dans votre abonnement, vous devez d’abord inscrire le fournisseur de ressources correspondant.

1. Démarrez un navigateur web et accédez au portail Azure à `https://portal.azure.com`.
1. Si vous y êtes invité, connectez-vous à l’aide de votre compte Microsoft Entra ID avec l’accès propriétaire à l’abonnement Azure que vous avez utilisé dans le labo précédent.
1. Dans l’onglet navigateur web affichant le portail Azure, dans la zone de texte de recherche en haut de la page, entrez **Abonnements** et, dans la liste des résultats, sélectionnez **Abonnements**.
1. Dans la page abonnements, dans le menu vertical situé à gauche, sélectionnez **fournisseurs de ressources**.
1. Dans la liste des fournisseurs de ressources, recherchez et sélectionnez **Microsoft.Chaos**.
1. Une fois le fournisseur de ressources **Microsoft.Chaos** sélectionné, dans la barre d’outils, sélectionnez **Inscrire**.

   > **Remarque :** N’attendez pas que l’inscription soit terminée, mais passez directement à l’exercice suivant. L’inscription peut prendre quelques minutes.

## Exercice 1 : Améliorer la résilience des charges de travail à l’aide de Traffic Manager

Dans cet exercice, vous allez implémenter une configuration résiliente qui distribue les requêtes entre les deux instances d’application web .NET dans deux régions Azure différentes à l’aide d’Azure Traffic Manager.

> **Remarque :** Dans le cadre de notre laboratoire, nous allons envisager le déploiement de . Application web basée sur NET eShopOnWeb dans la région USA Est en tant qu’instance principale. Bien que dans ce cas, cette considération soit purement arbitraire (et utilisée uniquement à des fins de démonstration), il peut y avoir des scénarios où il peut être utile de hiérarchiser l’un des points de terminaison.

L’exercice se compose des tâches suivantes :

- Tâche 1 : Implémenter un profil Traffic Manager
- Tâche 2 : Valider la fonctionnalité Traffic Manager

### Tâche 1 : Implémenter un profil Traffic Manager

1. Dans l’onglet navigateur web affichant le portail Azure, dans la zone de texte de recherche en haut de la page, entrez **profils Traffic Manager** et, dans la liste des résultats, sélectionnez **profils Traffic Manager**.
1. Sur la page **Load balancing\| Traffic Manager**, sélectionnez **+ Create**.
1. Dans la page **Créer un profil Traffic Manager**, effectuez les actions suivantes :

   - Dans la zone de texte **Nom**, entrez **devopsfoundationstmprofile**.

       > **Remarque :** Le nom du profil Traffic Manager doit être globalement unique. Si vous recevez un message d’erreur indiquant que le nom est déjà utilisé, essayez un autre nom et veillez à l’enregistrer. Vous en aurez besoin tout au long de ce labo.

   - Dans la **méthode de routage** liste déroulante, sélectionnez **Priorité **.

   > **Remarque :** Nous choisissons la méthode de routage prioritaire pour refléter l’hypothèse quelque peu arbitraire selon laquelle toutes les requêtes doivent être gérées par l’application web Azure App Service dans la région USA Est.

   - Vérifiez que votre abonnement Azure apparaît dans la liste déroulante **Abonnement**
   - Sélectionnez le lien **Créer un nouveau** sous la liste déroulante **groupe de ressources**, dans la zone de texte **Nom**, entrez **devops-foundations-rg**, puis sélectionnez **OK**.
   - Dans la liste déroulante **emplacement du groupe de ressources**, sélectionnez la même région Azure que celle que vous avez choisie dans les laboratoires précédents de ce cours.

1. Sélectionnez **Créer** pour démarrer le processus d’approvisionnement.

   > **Remarque** : Attendez la fin du déploiement. Cela doit se terminer dans un délai d’une minute.

1. Sur la page **Load balancing\| Traffic Manager**, si nécessaire, sélectionnez **Refresh** puis **devopsfoundationstmprofile**.
1. Dans la page **devopsfoundationstmprofile**, dans la section **Essentials**, copiez la valeur du paramètre **nom DNS** et enregistrez-le. Vous en aurez besoin tout au long de ce labo.
1. Dans la **page devopsfoundationstmprofile** , dans le menu de navigation de gauche, dans la **section Paramètres** , sélectionnez **Configuration**.
1. Examinez le contenu de la page de **configuration\| de devopsfoundationstmprofile**. Notez que, par défaut, **durée de vie DNS (TTL)** est définie sur **60** secondes. Remplacez la valeur par **5** secondes.

   > **Remarque :** L’abaissement de la valeur de la durée de vie accélère le temps de basculement. 

   > **Remarque :** Outre la valeur de la durée de vie, de l’intervalle de détection, du délai d’expiration de la sonde et du nombre toléré d’échecs, cela affecte la durée de détection d’une défaillance du point de terminaison.

1. Dans la page **devopsfoundationstmprofile \| Configuration**, dans la liste déroulante **Protocole**, sélectionnez **HTTPS** et, dans la zone de texte **Port**, entrez **443**.

   > **Remarque :** Les deux applications web Azure App Service qui seront configurées en tant que points de terminaison sont accessibles via HTTPS sur le port TCP par défaut 443.

1. Dans la page **devopsfoundationstmprofile \| Configuration**, sélectionnez **Enregistrer**.
1. Dans la page **devopsfoundationstmprofile**, dans le menu de navigation de gauche, dans la section **Paramètres**, sélectionnez **Points de terminaison**.
1. Dans la page **devopsfoundationstmprofile \| Point de terminaison***, sélectionnez **+ Ajouter**.

   > **Remarque :** Vous allez d’abord ajouter un point de terminaison représentant l’application web Azure App Service.

1. Dans le volet **Ajouter un point de terminaison**, effectuez les actions suivantes :

   - Vérifiez que **point de terminaison Azure** apparaît dans la liste déroulante **Type**.
   - Dans la zone de texte **Nom**, entrez **application web DevOps Foundations - USA Est**.
   - Vérifiez que la case **Activer le point de terminaison** est cochée.
   - Dans la liste déroulante **type de ressource cible**, sélectionnez **App Service**.
   - Dans la liste déroulante **ressource cible**, dans la section **rg-eshoponweb-eastus**, sélectionnez le nom de l’application web App Service dans la région Azure USA Est.
   - Dans la zone de texte **Priorité**, entrez **1**.

   > **Remarque :** Une valeur de priorité inférieure désigne une priorité plus élevée. Dans ce cas, toutes les demandes seront envoyées à l’instance d’application web dans la région USA Est, tant que cette instance reste disponible.

   - Vérifiez que les **contrôles d’intégrité** sont activés.

1. Sélectionnez **Ajouter**.

1. De retour sur la page **devopsfoundationstmprofile \| Point de terminaison***, sélectionnez **+ Ajouter**.

   > **Remarque :** Ensuite, vous allez ajouter un point de terminaison représentant l’autre application web Azure App Service déployée dans la région Azure Europe Ouest.

1. Dans le volet **Ajouter un point de terminaison**, effectuez les actions suivantes :

   - Vérifiez que **point de terminaison Azure** apparaît dans la liste déroulante **Type**.
   - Dans la zone de texte **Nom**, entrez **application web DevOps Foundations - Europe Ouest**.
   - Vérifiez que la case **Activer le point de terminaison** est cochée.
   - Dans la liste déroulante **type de ressource cible**, sélectionnez **App Service**.
   - Dans la liste déroulante **ressource cible**, dans la section **rg-eshoponweb-westeurope**, sélectionnez le nom de l’application web App Service dans la région Azure Europe Ouest.
   - Dans la zone de texte **Priorité**, entrez **2**.
   - Vérifiez que les **contrôles d’intégrité** sont activés.

1. Sélectionnez **Ajouter**.
1. De retour sur la page **devopsfoundationstmprofile \| Point de terminaison*****, vérifiez que les deux points de terminaison sont répertoriés avec l’entrée **En ligne** dans la colonne **Moniteur**.

   > **Remarque :** Vous devrez peut-être attendre quelques minutes et sélectionner **Actualiser** avant que l’entrée d’état soit à jour.

### Tâche 2 : Valider la fonctionnalité Traffic Manager

1. Démarrez une fenêtre de navigateur web et accédez au nom DNS associé au profil Traffic Manager.
1. Vérifiez que le navigateur web affiche la page familière du site web hébergé par l’application web Azure App Service que vous avez déployée dans le labo précédent.
1. Fermez la fenêtre de navigateur web nouvellement ouverte.
1. Basculez vers la fenêtre du navigateur web affichant le portail Azure.
1. Dans le portail Azure, sélectionnez l’icône **Azure Cloud Shell** à droite de la zone de texte de recherche.
1. Si nécessaire, sélectionnez **Bash** dans le menu déroulant dans le coin supérieur gauche du volet Cloud Shell.
1. Dans la session Bash dans le volet Cloud Shell, exécutez la commande suivante pour résoudre le nom DNS du profil Traffic Manager que vous avez créé dans la tâche précédente sur l’un des deux points de terminaison correspondants (remplacez l’espace réservé `<tm_profile>` par le nom DNS du profil Traffic Manager, par exemple, `devopsfoundationstmprofile.trafficmanager.net`), mais veillez à supprimer le `https:\\` du début :

   ```cli
   nslookup <tm_profile>
   ```

1. Réexécutez la même commande quelques fois, en attendant un peu plus de 5 secondes entre chaque appel (pour éliminer la possibilité de mise en cache DNS) et notez que la sortie dans chaque cas fait référence au nom DNS de l’application web dans la région Azure USA Est.

## Exercice 2 : Tester la résilience des charges de travail à l’aide d’Azure Chaos Studio

Dans cet exercice, vous allez tester la résilience des charges de travail à l’aide d’Azure Chaos Studio.

> **Remarque :** Cet exercice illustre l’utilisation d’Azure Chaos Studio. L’objectif d’Azure Chaos Studio est de faciliter la mesure, la compréhension et la création de la résilience des applications et des services. Cela s’effectue en perturbant intentionnellement les charges de travail afin d’identifier les lacunes de résilience et d’implémenter l’atténuation correspondante de manière proactive plutôt que réactive.

L’exercice se compose des tâches suivantes :

- Tâche 1 : Configurer l’environnement Azure Chaos Studio
- Tâche 2 : Implémenter une expérience
- Tâche 3 : Valider l’expérience

### Tâche 1 : Configurer l’environnement Azure Chaos Studio

1. Dans l’onglet navigateur web affichant le portail Azure, dans la zone de texte de recherche en haut de la page, entrez **Chaos Studio** et, dans la liste des résultats, sélectionnez **Chaos Studio**.
1. Dans la page **Chaos Studio**, sélectionnez **Cibles**.
1. Dans la page **Chaos Studio\| Targets**, sélectionnez l’instance d’application web Azure App Service dans le groupe de ressources **rg-eshoponweb-eastus** dans la région USA Est que vous avez déployée dans le labo précédent.
1. Dans la barre d’outils, sélectionnez le **Activer les cibles** en-tête de liste déroulante et, dans la liste déroulante, sélectionnez **Activer les cibles directes de service (toutes les ressources)**.
1. Dans la page **Activer les cibles directes de service**, vérifiez que l’instance d’application web App Service appropriée est répertoriée, sélectionnez **Vérifier + Activer**, puis sélectionnez **Activer**.

   > **Remarque :** Attendez que la cible soit activée. L’opération doit prendre moins d’une minute.

### Tâche 2 : Implémenter une expérience

1. Dans le portail Azure, dans la zone de texte de recherche en haut de la page, entrez **Chaos Studio** et, dans la liste des résultats, sélectionnez **Chaos Studio**.
1. Dans la page **Chaos Studio**, sélectionnez **Expériences**.
1. Dans la page **expériences**, sélectionnez **+ Créer**, puis, dans la liste déroulante, sélectionnez **Nouvelle expérience**.
1. Sous l’onglet **Informations de base** de la page **Créer une expérience**, effectuez les actions suivantes :

   - Vérifiez que votre abonnement Azure apparaît dans la liste déroulante **abonnement**.
   - Sélectionnez le lien **Créer un nouveau** sous la liste déroulante **groupe de ressources**, dans la zone de texte **Nom**, entrez **devops-foundations-rg**, puis sélectionnez **OK**.
   - Dans la section **Détails de l’expérience**, dans la zone de texte **Nom**, entrez **DevOps_Foundations_Labs_Experiment_01**.
   - Dans la liste déroulante **Région**, sélectionnez la région **Europe Ouest** Région Azure.

   > **Remarque :** Vous pouvez éventuellement choisir n’importe quelle région Azure, mais compte tenu du fait que vous testez des échecs dans une ressource dans la région Europe Ouest, n’importe quelle région autre que USA Est semble plus appropriée.

1. Sous l’onglet **Informations de base** de la page **Créer une expérience**, sélectionnez **Suivant : Autorisations >**.
1. Sous l’onglet **Autorisations**, acceptez l’option par défaut **identité affectée par le système**, puis sélectionnez **Suivant : Concepteur d’expériences >**.
1. Sous l’onglet **Concepteur d’expériences**, effectuez les actions suivantes :

   - Dans la zone de texte **étape**, entrez **étape 1 : Basculement d’une application web App Service**.
   - Dans la zone de texte **branche**, entrez **Branche 1 : Émuler un échec App Service**.
   - Sélectionnez **+ Ajouter une action** et, dans la liste déroulante, sélectionnez **Ajouter une erreur**.

1. Sous l’onglet **Détails** de l’erreur du **volet Ajouter une erreur** , dans la **liste déroulante Erreurs** , sélectionnez **Arrêter App Service** et définissez la valeur Durée **(minutes)** sur 10.
1. Sous l’onglet **Détails de l’erreur** du volet **Ajouter une erreur**, sélectionnez **Suivant : Ressources cibles >**.
1. Sous l’onglet **Ressources cibles** du volet **Ajouter une erreur**, vérifiez que l’option **Sélectionner manuellement dans une liste** est sélectionnée, sélectionnez l’application web App Service que vous avez ajoutée en tant que cible à l’environnement Chaos Studio, puis sélectionnez **Ajouter**.
1. De retour sous l’onglet **Concepteur d’expériences** de la page **Créer une expérience** , sélectionnez **Vérifier + créer**.
1. Dans l’onglet **Vérifier + créer** de la page **Créer une expérience**, sélectionnez **Créer**.

   > **Remarque :** Attendez la création de l’expérience. L’opération doit prendre moins d’une minute.

   > **Remarque :** Pour que l’expérience réussisse, vous devez également accorder les autorisations de compte de service managé nouvellement créés suffisantes pour arrêter l’application Web Azure App Service. Nous allons tirer parti à cet effet du rôle contributeur Azure intégré, mais vous pouvez créer un rôle personnalisé si vous souhaitez suivre le principe du privilège minimum.

1. Dans le portail Azure, dans la zone de texte de recherche en haut de la page, entrez **App Services** et sélectionnez **App Services** dans la liste des résultats.
1. Dans la page ** App Services**, sélectionnez l’application Web Azure App Service dans la région USA Est que vous avez déployée dans le labo précédent.
1. Dans la page de l’application web, dans le menu vertical situé à gauche, sélectionnez **contrôle d’accès (IAM)**.
1. Dans la page **contrôle d’accès (IAM)** de l’application web, sélectionnez **+ Ajouter** et, dans la liste déroulante, sélectionnez **Ajouter une attribution de rôle**.
1. Sous l’onglet **Rôle** de la page **Ajouter une attribution de rôle**, sélectionnez **rôles d’administrateur privilégiés**, dans la liste des rôles, sélectionnez **Contributeur**, puis sélectionnez **Suivant**.
1. Sous l’onglet **Membres** de la page **Ajouter une attribution de rôle**, définissez l’option **Accès affecté à** sur **Identité managée**, puis sélectionnez **+ Sélectionner des membres**
1. Dans le volet **Sélectionner des identités managées**, dans la liste déroulante **Identités managées**, sélectionnez **Chaos Experiment**, dans la liste des identités managées, sélectionnez celui que vous avez créé précédemment dans cette tâche, puis sélectionnez **Sélectionner**.
1. De retour sous l’onglet **Membres** de la page **Ajouter une attribution de rôle**, sélectionnez **Vérifier + affecter**, puis **Vérifier + affecter**.

### Tâche 3 : Valider l’expérience

1. Dans le portail Azure, revenez à la page **Chaos Studio** et, dans le menu vertical situé à gauche, sélectionnez **Expériences**.
1. Dans la liste des expériences, sélectionnez **DevOps_Foundations_Labs_Experiment_01**.
1. Dans la page **DevOps_Foundations_Labs_Experiment_01**, sélectionnez **Démarrer** et, lorsque vous y êtes invité à confirmer, sélectionnez **OK**.
1. Attendez que l’état de l’expérience change pour **Exécution**.
1. Dans la page **DevOps_Foundations_Labs_Experiment_01**, dans la section **Historique**, sélectionnez le lien **Détails** dans l’entrée représentant l’exécution actuelle de l’expérience.
1. Dans la page des détails de l’expérience **DevOps_Foundations_Labs_Experiment_01**, passez en revue la liste contenant l’étape, la branche et l’action associées à l’expérience.

   > **Remarque :** Vous devrez peut-être sélectionner entrée de barre d’outils **Actualiser** pour mettre à jour l’état de la liste.

1. Sélectionnez l’entrée **Action** pour afficher les **détails de l’erreur** volet.
1. Dans la section **Cibles en cours d’exécution**, sélectionnez l’entrée représentant l’application web Azure App Service.
1. Dans la page de l’application web, dans la section **Essential**, notez que l’état de l’application web est répertorié comme **Arrêtée**.

   > **Remarque :** Vous devrez peut-être sélectionner l’entrée de barre d’outils **Actualiser** pour mettre à jour l’état de l’application web.

   > **Remarque :** Nous allons maintenant vérifier si Traffic Manager redirige correctement les demandes ciblant son profil vers le point de terminaison représentant l’application web App Service dans la région Europe Ouest.

1. Démarrez un navigateur web et accédez au nom DNS associé au profil Traffic Manager.
1. Vérifiez que le navigateur web affiche la page familière du site web hébergé par l’application web Azure App Service que vous avez déployée dans le labo précédent.
1. Basculez vers la fenêtre du navigateur web affichant le portail Azure.
1. Dans le portail Azure, sélectionnez l’icône **Azure Cloud Shell** à droite de la zone de texte de recherche.
1. Dans la session Bash dans le volet Cloud Shell, exécutez la commande suivante pour résoudre le nom DNS du profil Traffic Manager que vous avez créé dans l’exercice précédent sur l’un des deux points de terminaison correspondants (remplacez l’espace réservé `<tm_profile>` par le nom DNS du profil Traffic Manager, mais veillez à supprimer le début`https:\\`) :

   ```cli
   nslookup <tm_profile>
   ```

1. Réexécutez la même commande quelques fois, en attendant un peu plus de 5 secondes entre chaque appel (pour éliminer la possibilité de mise en cache DNS) et vérifiez que la sortie dans chaque cas fait référence au nom DNS de l’application web dans la région Azure Europe Ouest.

> **Remarque :** Bien que cet exemple semble un peu simpliste, l’objectif principal de l’exercice était d’illustrer le processus d’implémentation de tests basés sur Azure Chaos Studio et ses composants principaux. Vous utiliseriez l’approche équivalente si une application web Azure App Service faisait partie d’un environnement plus complexe, où l’impact de son échec peut être beaucoup plus difficile à prédire.

### Exercice 3 : Supprimer les ressources utilisées dans les labos

Dans cet exercice, vous allez supprimer les ressources utilisées dans les labos.

1. Dans l’onglet navigateur web affichant le portail Azure, dans la zone de texte de recherche en haut de la page, entrez **groupes de ressources** et, dans la liste des résultats, sélectionnez **groupes de ressources**.
1. Dans la page **Groupes de ressources**, dans la liste des groupes de ressources, sélectionnez le groupe de ressources que vous avez créé dans le labo actuel.
1. Sur la page Groupe de ressources, sélectionnez **Supprimer un groupe de ressources**.
1. Dans la zone de texte **Entrez le nom du groupe de ressources pour confirmer la suppression**, entrez le nom du groupe de ressources que vous êtes sur le point de supprimer, puis sélectionnez **Supprimer**.

   > **Remarque :** Attendez que le groupe de ressources soit supprimé. L’opération doit prendre moins d’une minute.

1. Répétez l’étape précédente pour le groupe de ressources que vous avez créé dans les laboratoires précédents de ce cours.

    > **Remarque :** La suppression du groupe de ressources supprime toutes les ressources associées, y compris le profil Traffic Manager et l’environnement Azure Chaos Studio.

    > **Remarque :** Il n’est pas nécessaire de supprimer le compte GitHub et les dépôts que vous avez créés dans les laboratoires précédents de ce cours. Vous pouvez continuer à les utiliser en tant que portefeuille de votre travail.
