# Guide d'installation :

## Serveur WSUS : 

### Installation de WSUS : 

Sur une serveur Windows 2022 : 
- Ajouter des rôles et fonctionnalités
- Sélectionner "Windows Server Update Services"
- Inclure tous les outils de gestion
- Sélectionner des services de rôle "WID Connectivity" et "WSUS Services"
- Renseigner l'emplacement de stockage des mises à jour
- Installer

- Une fois l'installation terminée, cliqué en haut à gauche sur "Lancer les tâches de post-installation".


### Configuration de WSUS : 

Lancer la console "Services WSUS" : 
- "Next" pour commencer
- Ne pas cocher l'option du programme d'amélioration
- Sélectionner "Synchroniser depuis Microsoft Update"
- Ne pas cocher l'utilisation d'un proxy
- Start connecting
- Choisir les langues Français et Anglais
- Cocher "Windows Server 2019"
- Sélectionner les versions de Windows qui nous intéressent (pour nous, Windows 10, version 1903 et après, ainsi que Windows 11)
- Cocher les cases " Mise à jour critique", "Mise à jour de la sécurité", Mise à jour" et "Mise à jour de définitions"
- Cocher "Synchroniser automatiquement" et renseigner l'heure de synchronisation souhaitée
- Cocher "Commencer la synchronisation initiale"
- Terminer
- Le serveur de synchronise et la configuration de base est terminée.
