# Guide d'installation : 

## Serveur Passbolt : 

Pour installer un serveur Passbolt, nous allons utiliser un serveur Debian qui servira à ce rôle à l'avenir.  Nous effectuerons ces étapes en root.  

Nous allons d'abord récupérer les différents éléments suivant :   
- En premier lieu, il nous faut récupérer l'installer de passbolt :
```Bash 
wget "https://download.passbolt.com/ce/installer/passbolt-repo-setup.ce.sh"
```

- Puis, nous allons télécharger SHA512SUM pour le script d'installation. SHA512SUM permet de calculer et vérifier les empreintes numériques des fichiers chiffrés.
```Bash
wget https://github.com/passbolt/passbolt-dep-scripts/releases/latest/download/passbolt-ce-SHA512SUM.txt
```

Nous allons ensuite exécuter le script suivant : 
```Bash
sha512sum -c passbolt-ce-SHA512SUM.txt && bash ./passbolt-repo-setup.ce.sh  || echo \"Bad checksum. Aborting\" && rm -f passbolt-repo-setup.ce.sh
```

Il est temps d'installer Passbolt: 
```Bash
sudo apt install passbolt-ce-server
```

Cela va lancer une interface d'installation graphique. Nous allons maintenant en suivre les étapes.


