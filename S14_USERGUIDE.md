# Guide d'utilisateur : 

Semaine 14  
Version 1.0  

# Ajout de la fonction LOG dans les scripts d'ajout Users, groupes, OU et PC :

Dans chacun des scripts créés précédemment pour la création et la gestion de notre domaine AD, nous avons ajouté une fonction de journalisation afin de suivre l'ensemble des actions effectués au cours du temps.
La fonction est la suivante :   

```Powershell
Function Log
{
    param([string]$FilePath,[string]$Content)

    # Vérifie si le fichier existe, sinon le crée
    If (-not (Test-Path -Path $FilePath))
    {
        New-Item -ItemType File -Path $FilePath | Out-Null
    }

    # Construit la ligne de journal
    $Date = Get-Date -Format "dd/MM/yyyy-HH:mm:ss"
    $User = [System.Security.Principal.WindowsIdentity]::GetCurrent().Name
    $logLine = "$Date;$User;$Content"

    # Ajoute la ligne de journal au fichier
    Add-Content -Path $FilePath -Value $logLine
}
```

On définit ensuite une variable désignant le lieu où sera enregistré notre journal : 

```Powershell
#Pour le script d'ajout des PC
$LogFile="C:\Windows\Logs\Scripts\logPC"
# Pour le script d'ajout des groupes et OU
$LogFile="C:\Windows\Logs\Scripts\logOUGRP"
# Pour le script d'ajout des utilisateurs
$LogFile="C:\Windows\Logs\Scripts\logUsers"
```

A partir de là, dès que notre script effectue une action que nous estimons importante à enregistrer, nous rajoutons une ligne de journalisation après cette action. 
Cette ligne de journalisation peut prendre la forme suivante :  

```Powershell
New-ADOrganizationalUnit -Name 01Utilisateurs -path "DC=ekoloclast,DC=lan" -ProtectedFromAccidentalDeletion:$False
Log -FilePath $LogFile -Content " Création de l'OU 01Utilisateurs "
```

Dans cet exemple, nous enregistrons l'action dans le journal grâce à la ligne commençant par **Log**. **Log** est une fonction que nous avons définit avec deux paramètres. Le premier est le paramètre **Filepath** qui va déterminer où l'on enregistre l'action (ici c'est $logFile que nous avons défini plus haut). Le second est le paramètre **content** que l'on rempli manuellement pour renseigner la nature de l'action.
Dans cet exemple, nous enregistrons donc la création de l'OU 01Utilisateurs dans le fichier **C:\Windows\Logs\Scripts\logOUGRP**.


## Utilisation du logiciel de supervision PRTG : 
