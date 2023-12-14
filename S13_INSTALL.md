# RAID1 sur Windows Server

Pour cela vous allez avoir besoin de rajouter un disque dans votre VM, sa capacité doit au moins être équivalente à celle du disque qui sera dupliqué grâce au RAID1.

## Disk Management 
Une fois votre disque branché, il faut le configurer dans le "**Disk Management**".   
Pour y accéder, fais un clic droit sur le logo Windows de ta "**Barre Démarrer**".  
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubRAID1/Capture%20d'%C3%A9cran%202023-12-14%20101117.png?raw=true)

Une fois votre "**Disk Management**" ouvert, le "**Disk Management**" détecte votre nouveau disque et vous propose de choisir entre un format MBR ou GPT, il est important de choisir le même format que le disque cible ! Dans notre cas, on utilisera le GPT.  

Une fois votre disque au bon format, vous pouvez constater qu'il est basic, donc nous allons le convertir en disque dynamique.  

Fais un clic droit sur le disque que vous venez d'ajouter et sélectionnez "**Convert to Dynamic Disk**".  
![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubRAID1/Capture%20d'%C3%A9cran%202023-12-14%20101208.png?raw=true)

Un petit Wizard Windows va se lancer, suivez les étapes en laissant tout par défaut et terminer la conversion.

Enfin, choisissez le volume que vous voulez dupliqué, faite un clic droit et sélectionnez  "**Add Mirror**" (ce qui correspond à faire un RAID1).  

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/GitHubRAID1/Capture%20d'%C3%A9cran%202023-12-14%20101257.png?raw=true)

Dans le Wizard Windows, sélectionnez le disque que vous venez d'implémenter dans votre VM, et terminez la configuration.  

## FAQ
1. A la fin de la configuration de mon miroir, un message d'erreur me dit qu'il n'y a pas assez d'espace sur mon disque alors que celui ci est vide !  
	-  C'est normal Billy ! Cela signifie juste qu'il n'y a plus assez d'espace dans le disque de la machine hôte pour effectuer le RAID1, fais de la place dans le disque de ta machine hôte ou son stocké les disques virtuelles et retente l'opération !
