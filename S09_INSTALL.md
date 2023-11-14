# Guide d'installation :

Nous allons ici voir l'installation d'un Active Directory sur un serveur Windows Server 2022 avec la configuration des outils utiles pour ce projet.


## Installation d'Active Directory : 

- Dans le Server Manager, cliquer sur **manage**
- On sélectionne **add rôles and features**
- On rajoute les fonctionnalités et on lance l'installation

 
### Création d'un domaine Active Directory : 

Une fois l'installation terminée :   
- Cocher **Promouvoir ce serveur en contrôleur de domaine**
- Ajouter une nouvelle forêt
- La nommer **Ekoloclast.lan**
- Mettre un mot de passe
- Installer
- On redémarre ensuite l'ordinateur.

### Création d'une Unité Organisationnelle (OU) : 

-   On sélectionne **Tools**
-   Active Directory Users and Computers
-   Sur Ekoloclast.lan ==> clic droit ==> new ==> Organizationnal Unit
-   Nommer "Paris" 
-   Décocher **Protect container from accidental deletion**
-   Créer l'OU.


### Création d'un groupe : 


-   On sélectionne **Tools**
-   Active Directory Users and Computers
-   Sur Ekoloclast.lan ==> clic droit ==> new ==> Group
-   Nommer "Utilisateurs"
-   Sélectionner le scope **Domain Local** si l'on veut le cantonner à ce domaine ou **Global** si l'on veut pouvoir l'utiliser plus tard dans un domaine approuvé.
-   Sélectionner le type de groupe **Security**.
-   Créer le groupe.



