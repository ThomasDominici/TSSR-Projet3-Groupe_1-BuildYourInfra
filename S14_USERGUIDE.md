# Guide d'utilisateur : 

Semaine 14  
Version 1.0  

## Ajout de la fonction LOG dans les scripts d'ajout Users, groupes, OU et PC :

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

## Désactivation des Utilisateurs

```Powershell
Foreach ($row in $CSVData)
{
    $row.Prénom = RemplacementCaractereSpeciaux -String $row.Prénom
    $row.Nom = RemplacementCaractereSpeciaux -String $row.Nom   
}
$ADUsers = Get-ADUser -Filter * -Properties SamAccountName

Foreach ($ADUser in $ADUsers) 
{
    $ADUserNom =  $ADUser.Surname
    $ADUserPrenom = $ADUser.GivenName
    $ADUserLogin = $ADuser.SamAccountName
    #$ADUserSpe = $ADUser.UserPrincipalName
    
    $CSVUser = $CSVData | Where-Object { $_.Prénom -eq $ADUserPrenom -and $_.Nom -eq $ADUserNom }
    If ($ADUser.ObjectGUID -eq "5897a287-f5e0-422b-8fe9-c0c91035a363" -or $ADUser.ObjectGUID -eq "a50119a6-73b4-4ed9-a05e-badeed3236e9" -or $ADUser.ObjectGUID -eq "514ec1a1-ec9b-4fde-ac7e-6788874f511d" -or $ADUser.ObjectGUID -eq "9d0ab936-e8f4-40b4-8d0d-b4e4848187d8" )
        {
        Write-Host "Utilisateur Essentiel à ne pas toucher"
        }
    Else
        {
            If (!$CSVUser)
            {
                Write-Host "L'utilisateur $ADUserLogin n'existe pas dans le CSV. Désactivation et déplacement dans les archives."
                Set-ADUser -Identity $ADUser -Enabled:$false
                $ADUser | Move-ADObject -TargetPath "OU=Utilisateurs,OU=03Archive,DC=Ekoloclast,DC=lan"
                Log -FilePath $LogFile -Content "L'utilisateur $ADUserLogin n'existe pas dans le CSV. Désactivation et déplacement dans les archives."
            }
        }
}
```

Voici la partie du script qui nous permet la désactivation des utilisateurs de l'AD en fonction de notre CSV.    

Dans la première boucle, nous remplaçons tous les caractères spéciaux des Noms et Prénoms de notre CSV.    

Dans la deuxième boucle, on commence par exclure tous les Utilisateurs essentiels au bon fonctionnement de notre AD, dans l'ordre "Administrator, Guest, KRBTGT, et glpi-ldap"    

Ensuite nous recherchons des objets dans la variable CSVData qui correspondent aux Prénoms et aux Noms des utilisateurs de notre AD.    
Comme nous utilisons l'inversion de résultat avec le "**!**", seules les personnes qui sont présentes dans l'AD et non dans le CSV sont sorties grâce à la condition, il nous suffit juste de les désactiver puis de les déplacer dans une OU Archive, et de journaliser le tout.    

## Utilisation du logiciel de supervision PRTG : 
