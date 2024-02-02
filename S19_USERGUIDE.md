# Guide d'utilisation : 

## Nmap : 

Nmap nous permet de "sniffer" les différents ports du réseau et de sortir un ensemnble d'informations utiles pour des attaques ultérieures en profondeur.   
L'outil existe en local sur Kali Linux. Nous pouvons le retrouver dans le menu en haut à gauche.

Une fois sur la console, voici quelques lignes de commande possibles pour sniffer un réseau : 
```
nmap 192.168.10.0/24
# Scan complet du réseau 192.168.10.0/24
nmap -sS 192.168.10.0/24
# Scan en mode discret du réseau, plus dur à détecter
nmap -sp 192.168.10.0/24
# Balayage ping du réseau pour voir les connexions possibles
```

Il en existe beaucoup d'autres avec plus de possibilités.   
Après un scan de quelques secondes, nmap affiche le résultat sur le CLI.  


## Hydra : 

Hydra nous permet de faire des attaques par force brute sur un réseau, afin de récupérer des mots de passe.  
C'est aussi un outil aussi natif de Kali Linux.  

On peut se créer un dossier de listes de mots : 
```
cd /home/kali 
mkdir passwords-and-usernames
git clone https://github.com/danielmiessler/SecLists
cp -r SecLists/Usernames/*.txt  /home/kali/passwords-and-usernames/
cp -r SecLists/Passwords/*.txt  /home/kali/passwords-and-usernames/
```

On peut utiliser un ensemble d'options avec notre attaque :   
    -s = port  
    -l = utilisateur simple    
    -L = utilisateurs extraient d’une liste   
    -P = mots de passe extraient d’une liste  
    -T = nombre d’essai par seconde (4, étant une valeur recommandé, pour outrepasser certains IPS vieillissant et non mis à jours)  
    -V = Afficher tout les essais dans la fenêtre du terminal   


Pour l'utilisateur, on utilise **l** si on le connaît (et on le renseigne), sinon on utilise **L** et on renseigne une liste de mots pour le trouver.   
De même pour le mot de passe, **p** si on le connaît et **P** si on ne le connaît pas et on renseigne une liste de mots pour l'attaque.  
Pour plus d'infos : 
```
hydra --help
```

La syntaxe générale est  :
```
hydra -l < nom d'utilisateur > -P < mot de passe > < protocole > : // < ip >
```

Exemple dans notre réseau : 
```
hydra -l wilder -P /usr/share/wordlists/rockyou.txt 192.169.10.91 ssh
# Attaque par brute force de notre serveur GLPI avec SSH installé et configuré sur l'utilisateur   
 connu wilder en utilisant la liste de mots rockyou.txt sur le protocole SSH.  
```

Hydra nous affiche le résultat après l'attaque.  


