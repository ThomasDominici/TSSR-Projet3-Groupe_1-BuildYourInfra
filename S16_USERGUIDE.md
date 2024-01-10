# Guide d'utilisation : 

## Lier les machines du domaine au serveur WSUS : 

### Modifier la méthode d'affectation des ordinateurs : 

Sur la console WSUS : 
- "Options" à gauche
- "Ordinateurs"
- Cocher l'option "Utiliser les paramètres de stratégie de groupe ou de registre sur les ordinateurs"
- Valider


### Créer des groupes d'ordinateurs dans WSUS : 

- Sur "Tous les ordinateurs", clic droit
- Ajouter un groupe d'ordinateurs
- Le nommer (nous avons créé chez nous "Clients","Serveurs","DC")
- On obtient une arborescence représentant les ordinateurs de notre réseau


### Lier les PC et les serveurs à WSUS par GPO : 

Nous allons créer des GPO, une pour les paramètres en commun de nos ordianteurs et d'autres pour les paramètres spécifiques de nos Clients, DC et Serveurs, soit au total 4 GPO.
Il s'agira de GPO Ordinateurs filtrées avec le groupe GrpOrdinateurs rassemblant toutes les machines et reliées aux OU correspondantes.


#### GPO WSUS pour les paramètres communs :

Editer la GPO : 
- Configuration ordinateur
- Stratégies
- Modèles d'administration
- Composants Windows
- Windows Update  

Nous allons d'abord défninir l'adresse de notre serveur WSUS avec "Spécifier l'emplacement intranet du service de mise à jour Microsoft"
- http://srvWSUS.ekoloclast.lan:8530 (8530 = port par défaut de WSUS)
- Le mettre deux fois dans les deux premières cases

Le second paramètre se nomme "Configuration du service Mises à jour automatiques":
- Garder l'option 4 - Téléchargmeent automatique et planification des installations
- Cocher les semaines 3 et 4 car Windows publie ses MAJ courant de la seconde semaine du mois en général  

Le troisième paramètre est "Ne pas se connecter à des emplacements Internet Windows Update"
-Activer

#### GPO WSUS pour les paramètres des clients : 

Editer la GPO : 
- Configuration ordinateur
- Stratégies
- Modèles d'administration
- Composants Windows
- Windows Update

Premier paramètre : "Autoriser le ciblage côté client" : 
- Activer
- Nommer le groupe en fonction du nom donné au groupe sur le serveur WSUS (Clients puis plus tard nous ferons de même avec Serveurs et DC sur les GPO suivantes)

Second paramètre : "Désactiver le redémarrage automatique pour les mises à jour pendant les heures d'activité" :
- Activer
- Définir les heures d'activité  



#### GPO WSUS pour les paramètres des serveurs : 

Editer la GPO : 
- Configuration ordinateur
- Stratégies
- Modèles d'administration
- Composants Windows
- Windows Update

Premier paramètre : "Autoriser le ciblage côté client" : 
- Activer
- Nommer le groupe en fonction du nom donné au groupe sur le serveur WSUS (Serveurs)

Second paramètre : "Désactiver le redémarrage automatique pour les mises à jour pendant les heures d'activité" :
- Activer
- Définir les heures d'activité  



#### GPO WSUS pour les paramètres des DC : 

Editer la GPO : 
- Configuration ordinateur
- Stratégies
- Modèles d'administration
- Composants Windows
- Windows Update

Premier paramètre : "Autoriser le ciblage côté client" : 
- Activer
- Nommer le groupe en fonction du nom donné au groupe sur le serveur WSUS (DC)

Second paramètre : "Désactiver le redémarrage automatique pour les mises à jour pendant les heures d'activité" :
- Activer
- Définir les heures d'activité


### Tester la mise en place des GPO : 

Sur un client taper dans cmd : 
```
gpupdate /force
```

Une fois le message de mise à jour approuvée affiché, redémarrer.
Une fois redémarré, rafraîchir la console WSUS, le client est maintenant visible.


