# Ping Castle

## 1. Pré-requis

1 Domaine Active Directory
Windows 10 Entreprise Edition
Vous allez devoir installer PingCastle sur une machine cliente de votre domaine, connecter avec un compte utilisateur de ce domaine.

## 2. Installation

Dans un premier temps, rendez-vous sur la page officiel de [PingCastle](https://www.pingcastle.com/download/), dans l'onglet téléchargement, télécharger la dernière version.

Une fois le téléchargement terminée, extraire l'archive, et lancer "**PingCastle.exe**".

Voila, votre PingCastle est désormais installé et, vous avez le premier audit de votre domaine.

## Purple Knight

## 1. Pré-requis

1 Domain Active Directory
Windows 10 Entreprise Edition
Vous allez devoir installer Purple Knight sur une machine cliente de votre domaine, connecter avec un compte utilisateur de ce domaine.

## 2. Installation 
Dans un premier temps, rendez-vous sur la page officiel de [Purple Knight](https://www.purple-knight.com/request-form/), remplissez le formulaire, et attendre environ 10-15 min pour recevoir par le mail que vous avez renseignés, l'archive d'installation de Purple Knight.

Une fois l'archive téléchargée, on va enlever le "Block File", pour ce faire, ouvrez un terminal PowerShell et exécutez la commande suivante :
```PowerShell
dir -Path <emplacement de votre archive depuis la racine exemple "C:\"> -Recurse | Unblock-File
```

Vérifiez votre Politique d'exécution de mots de passes, la passez en "**RemoteSigned**" sur l'utilisateur courant au besoin.

Une fois ses deux étapes résolues, extraire l'archive, et lancer "**PurpleKnight.exe**".

Voila, votre Purple Knight est désormais installé et, vous avez le premier audit de votre domaine.

# BloodHound

1. Pré-requis
1 Domain Active Directory
Windows 10 Entreprise Edition
Vous allez devoir installer BloodHound sur une machine cliente de votre domaine, connecter avec un compte utilisateur de ce domaine.
