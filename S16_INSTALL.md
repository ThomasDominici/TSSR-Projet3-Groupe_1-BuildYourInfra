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


# Pré-requis
Il vous faudra :
- une VM ou un CT debian 12 de préférence mise à jour (avec apt update/upgrade) et une connexion a internet pour récupérer le software d'installation
 - le fichier /etc/hostname doit contenir le nom de votre machine en Full Qualified Domain Name (FQDN), pour le bon déroulement de l'installation, il devra etre différent de votre domaine de messagerie, exemple dans notre cas : 
```bash
#Saisir le nom de votre machine en FQDN
mail.iRedMailGH.com 
```
- le fichier /etc/hosts doit contenir le nom de votre machine en Full Qualified Domain Name (FQDN) ainsi que l'adresse Lo de votre machine, pour le bon déroulement de l'installation, il devra etre différent de votre domaine de messagerie, exemple dans notre cas : 
```bash
127.0.0.1 mail.iRedMailGH.com localhost
```

- Comme vous pouvez le constater, il faut absolument que les deux champs renseignés sois identique.

# Installation de iRedMail

Une fois votre VM/CT préparée on va récupérer l'installer via un wget :
Pensez à vérifier sur le site d'iRedMail la version actuelle.
```bash
wget https://github.com/iredmail/iRedMail/archive/refs/tags/1.6.8.tar.gz
```

Une fois l'archive récupérée, on va l'extraire avec la commande suivante : 

```bash
tar -zxf 1.6.8.tar.gz
```

l'option "**-xf**" permet d'extraire tout les fichiers présents dans l'archive
l'option "**-z**" est obligatoire du a l'extension "**gz**"

Une fois l'archive extraite on va se rendre dans le dossier iRedMail-1.6.8 et on va lancer le script "**iRedMail.sh**"

```bash
cd iRedMail-1.6.8
bash iRedMail.sh
```

Si vous avez bien saisie les bons pré requis le script ce lance et un installation graphique apparaitra : 
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20152954.png?raw=true)

# Configuration

La configuration commence par un message vous signifiant que vous pouvez la suspendre a tout moment avec la commande magique "**Ctrl+C**" et vous donne un lien vers le forum d'iredmail en cas de pépin.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20152954.png?raw=true)

Ensuite vous pouvez décidé de changer l'emplacement de stockage des mails, nous prendrons le chemin par défaut : /var/vmail.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153004.png?raw=true)

On continue pour le choix du serveur web qui permettra d'utilisé l'interface web d'iRedMail, nous choisirons d'utilisé Nginx
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153011.png?raw=true)

Le choix de la base de donnée, comme préciser dans l'encart, choisissez la database avec laquelle vous etes le plus habitué. Nous choisirons MariaDB
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153018.png?raw=true)

Ici, on vous demande de renseigner le suffixe LDAP, pour nous cela sera : "**dc=ekoloclast,dc=lan**"
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153131.png?raw=true)

Spécifiez le mot de passe que vous souhaitez utiliser.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153145.png?raw=true)

Ici, on va renseigner le domaine mail. ekoloclast.lan
(il est recommandé, si vous souhaitez lié votre domaine mail et AD d'utilisé exactement le même nom de domaine, ce démarche ne sera pas expliqué dans ce tutoriel)
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153206.png?raw=true)

Choisissez le mot de passe pour l'administration de votre compte pour accéder à l'interface administrateur de votre interface web 
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153218.png?raw=true)

Enfin, des composants optionnel. A votre bon cœur Messieurs Dame.
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/iRedMail/Capture%20d'%C3%A9cran%202024-01-10%20153225.png?raw=true)

Suite à cela votre serveur va effectuez l'installation, pensez à redémarrez votre serveur pour que votre serveur sois complétement fonctionnel.
