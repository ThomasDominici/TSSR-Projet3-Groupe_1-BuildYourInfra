# Guide d'installation des deux serveurs ADCORE : 

Pour l'installation de ces serveurs, on utilise le script suivant : 

```PowerShell
###################################################################  Initialisation Variable  ##########################################################################



$arg1 = read-host "Donner un nom au serveur CORE" 

$arg2 = read-host "Donner une adresse IP" 

$arg3 = read-host "Donner l'adresse IP de la Gateway"

$arg4 = read-host "Renseigner le nom de Domaine"



#############################################################################  MAIN  ###################################################################################



Rename-Computer -NewName $arg1 -Force

New-NetIPAddress -IPAddress "$arg2" -PrefixLength "24" -InterfaceIndex (Get-NetAdapter).ifIndex -DefaultGateway "$arg3"

Set-DnsClientServerAddress -InterfaceIndex (Get-NetAdapter).ifIndex -ServerAddresses ("$arg4")

Install-WindowsFeature -Name RSAT-AD-Tools -IncludeManagementTools -IncludeAllSubFeature

Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools -IncludeAllSubFeature

install-WindowsFeature -Name DNS -IncludeManagementTools -IncludeAllSubFeature

Install-ADDSDomainController -DomainName "$arg4" -Credential (Get-Credential)

Get-ADDomainController -Identity $arg1

Restart-Computer

```

On passe ensuite sur S17_USERGUIDE.md pour la répartition des rôles FSMO.
