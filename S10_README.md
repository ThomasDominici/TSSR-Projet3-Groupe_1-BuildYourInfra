# TSSR-Projet3-Groupe_1-BuildYourInfra
Semaine 10
Version 1.0


## Présentation du projet :
L'objectif du projet est de réaliser un réseau dans une entreprise qui fonctionne actuellement sur une box FAI.  
Il faut d'ores et déjà prendre en compte l'évolution prochaine des effectifs suite à la fututre fusion/acquisition.

## Objectif de la semaine S10  
Cette semaine nous avions pour objectif de : 
  - Installer et configurer un serveur debian avec PassBolt "Logiciel de Gestionnaire de Mot de Passe"
  - Installer et configurer un deuxieme serveur AD DC pour assurer la redondance des données ainsi
  - Automatisation de la création d'OU, groupe, et Gestion des Utilisateurs parti de l'entreprise

## Les membres du groupe :

|Nom|Rôle|Travaux effectués|
| :---: | :---: | --- |
|Thomas | PO | Installation et configuration du serveur PassBolt |
|Vincent | Agent actif | Script |
|Fabrice | SM |  |


### Tâches Communes : 
Réflexion sur la conception des scripts
Finalisation des configuration PassBolt et AD 
Configuration des IP des differente machine en concordance avec le réseaux Proxmox : 
| Machine | IP |
|  :---: | :---: |
| Serveur AD DC | 192.168.10.5|
| Serveur AD2  | 192.168.10.10 |
| Serveur PassBolt | 192.168.10.15 |
| Client Ubuntu  | 192.168.10.20 |

## Les Difficultés :
.
