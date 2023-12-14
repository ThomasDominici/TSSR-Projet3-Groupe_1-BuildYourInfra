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
	-  C'est normal. Cela signifie juste qu'il n'y a plus assez d'espace dans le disque de la machine hôte pour effectuer le RAID1, fais de la place dans le disque de ta machine hôte ou son stocké les disques virtuelles et retente l'opération !


# Restriction horaire d'utilisation des machines

## Etablissement des horaires 

Nous pouvons définir les horaires d'accès aux machines des utilisateurs.
Nous pouvons le faire de deux manières. Manuellement, on peut clic droit sur un utilisateur : 
- Propriétés
- Compte
- Horaires d'accès

On arrive sur un tableau d'horaires que l'on peut modifier.

![img](https://github.com/ThomasDominici/TSSR-Projet3-Groupe_1-BuildYourInfra/blob/Ressources_Images/horairesacces.JPG?raw=true)

Mais cette méthode serait trop longue pour l'appliquer à chaque utilisateur. Nous avons donc utilisé le script suivant : 
```PowerShell
Function Set-LogonHours{
   [CmdletBinding()]
   Param(
   [Parameter(Mandatory=$True)][ValidateRange(0,23)]$TimeIn24Format,
   [Parameter(Mandatory=$True,ValueFromPipeline=$True,ValueFromPipelineByPropertyName=$True,Position=0)]$Identity,
   [parameter(mandatory=$False)][ValidateSet("WorkingDays", "NonWorkingDays")]$NonSelectedDaysare="NonWorkingDays",
   [parameter(mandatory=$false)][switch]$Sunday,
   [parameter(mandatory=$false)][switch]$Monday,
   [parameter(mandatory=$false)][switch]$Tuesday,
   [parameter(mandatory=$false)][switch]$Wednesday,
   [parameter(mandatory=$false)][switch]$Thursday,
   [parameter(mandatory=$false)][switch]$Friday,
   [parameter(mandatory=$false)][switch]$Saturday
)
Process{
   $FullByte=New-Object "byte[]" 21
   $FullDay=[ordered]@{}
   0..23 | foreach{$FullDay.Add($_,"0")}
   $TimeIn24Format.ForEach({$FullDay[$_]=1})
   $Working= -join ($FullDay.values)
   Switch ($PSBoundParameters["NonSelectedDaysare"])
   {
    'NonWorkingDays' {$SundayValue=$MondayValue=$TuesdayValue=$WednesdayValue=$ThursdayValue=$FridayValue=$SaturdayValue="000000000000000000000000"}
    'WorkingDays' {$SundayValue=$MondayValue=$TuesdayValue=$WednesdayValue=$ThursdayValue=$FridayValue=$SaturdayValue="111111111111111111111111"}
   }
   Switch ($PSBoundParameters.Keys)
   {
    'Sunday' {$SundayValue=$Working}
    'Monday' {$MondayValue=$Working}
    'Tuesday' {$TuesdayValue=$Working}
    'Wednesday' {$WednesdayValue=$Working}
    'Thursday' {$ThursdayValue=$Working}
    'Friday' {$FridayValue=$Working}
    'Saturday' {$SaturdayValue=$Working}
   }

   $AllTheWeek="{0}{1}{2}{3}{4}{5}{6}" -f $SundayValue,$MondayValue,$TuesdayValue,$WednesdayValue,$ThursdayValue,$FridayValue,$SaturdayValue

   # Timezone Check
   if ((Get-TimeZone).baseutcoffset.hours -lt 0){
    $TimeZoneOffset = $AllTheWeek.Substring(0,168+ ((Get-TimeZone).baseutcoffset.hours))
    $TimeZoneOffset1 = $AllTheWeek.SubString(168 + ((Get-TimeZone).baseutcoffset.hours))
    $FixedTimeZoneOffSet="$TimeZoneOffset1$TimeZoneOffset"
   }
   if ((Get-TimeZone).baseutcoffset.hours -gt 0){
    $TimeZoneOffset = $AllTheWeek.Substring(0,((Get-TimeZone).baseutcoffset.hours))
    $TimeZoneOffset1 = $AllTheWeek.SubString(((Get-TimeZone).baseutcoffset.hours))
    $FixedTimeZoneOffSet="$TimeZoneOffset1$TimeZoneOffset"
   }
   if ((Get-TimeZone).baseutcoffset.hours -eq 0){
    $FixedTimeZoneOffSet=$AllTheWeek
   }

   $i=0
   $BinaryResult=$FixedTimeZoneOffSet -split '(\d{8})' | Where {$_ -match '(\d{8})'}

   Foreach($singleByte in $BinaryResult){
    $Tempvar=$singleByte.tochararray()
    [array]::Reverse($Tempvar)
    $Tempvar= -join $Tempvar
    $Byte = [Convert]::ToByte($Tempvar, 2)
    $FullByte[$i]=$Byte
    $i++
   }
   Set-ADUser -Identity $Identity -Replace @{logonhours = $FullByte} 
}
end{
   Write-Output "All Done :)"
}
}
Get-ADUser -SearchBase "OU=01Utilisateurs,DC=ekoloclast,DC=lan" -Filter * | Set-LogonHours -TimeIn24Format @(8,9,10,11,12,13,14,15,16,17) `
                    -Monday -Tuesday -Wednesday -Thursday -Friday -NonSelectedDaysare NonWorkingDays
```

Ce script permet d'établir les mêmes horaires à tous les employés de l'entreprise, c'est-à-dire de 8h à 18h du lundi au vendredi.


## Mise en place de la GPO associée

En plus de l'établissement des horaires, nous allons mettre en place une GPO qui déconnecte les employés de leur session après les heures indiquées.
Il s'agit d'une GPO s'appliquant aux ordinateurs.
Le chemin d'édition de cette GPO est le suivant : 
- Configuration ordinateur
- Windows Settings
- Paramètres de sécurité
- Stratégies locales
- Options de sécurité
- Serveur réseau Microsoft : Déconnecter les clients à l'ouverture de session heures expirent
- Enabled


La GPO a bien été montée et s'applique à l'ensemble des ordinateurs du domaine.
