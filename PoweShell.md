# Aide mémoire PowerShell

**Vérifier un hash d'un fichier**
``` PowerShell
Get-FileHash <nomDuFichier>
```
Validé un hash par comparaison
```
$fileHash = Get-FileHash <nomDuFichier>
$referenceHash = "<Le Hash de référance>"

if ($fileHash.Hash -eq $referenceHash) {
    Write-Output "Le hash du fichier correspond à la valeur de référence."
} else {
    Write-Output "Le hash du fichier ne correspond pas à la valeur de référence."
}
```
Pour changer l'algoritm il suffit d'ajouté à Get-FileHash l'argument suivant : -Algorithm MD5 ou autre algoritme

**Liste des commandes**
```
Get-Command
```

**Connaitre la version de PowerShell**
```
$PSVersionTable.PSVersion
```

**Copier un fichier**
```
Copy-Item -Path "C:\chemin\vers\fichier.txt" -Destination "C:\nouveau\chemin\vers\fichier.txt"
```

**Déplacer un fichier**
```
Move-Item -Path "C:\chemin\vers\fichier.txt" -Destination "C:\nouveau\chemin\vers\fichier.txt"
```

**Renommer un fichier**
```
Rename-Item -Path "C:\chemin\vers\ancienNom.txt" -NewName "nouveauNom.txt"
```

**Supprimer un fichier**
```
Remove-Item -Path "C:\chemin\vers\fichier.txt"
```

**Créer un nouveau dossier**
```
New-Item -Path "C:\chemin\vers\nouveauDossier" -ItemType Directory
```

**Lister les processus en cours d'exécution**
```
Get-Process
```

**Arrêter un processus**
```
Stop-Process -Name "nomDuProcessus"
```

**Démarrer un nouveau processus**
```
Start-Process "notepad.exe"
```

**Lister les services**
```
Get-Service
```

**Démarrer un service**
```
Start-Service -Name "nomDuService"
```

**Arrêter un service**
```
Stop-Service -Name "nomDuService"
```

**Redémarrer un service**
```
Restart-Service -Name "nomDuService"
```

**Lister les clés de registre**
```
Get-ChildItem -Path "HKLM:\Software"
```

**Créer une clé de registre**
```
New-Item -Path "HKLM:\Software\NouvelleClé"
```

**Supprimer une clé de registre**
```
Remove-Item -Path "HKLM:\Software\NouvelleClé"
```

**Créer un nouvel utilisateur**
```
New-LocalUser -Name "NouvelUtilisateur" -Password (ConvertTo-SecureString "MotDePasse" -AsPlainText -Force)
```

**Ajouter un utilisateur à un groupe**
```
Add-LocalGroupMember -Group "NomDuGroupe" -Member "NomUtilisateur"
```

**Créer un nouveau groupe**
```
New-LocalGroup -Name <groupName> -Description "Description du groupe"
```

**Supprimer un groupe**
```
Remove-LocalGroup -Name <groupName>
```

**Afficher les groupe**
```
Get-LocalGroup
```

**Définir une variable**
```
$maVariable = "Valeur"
```

**Afficher la valeur d'une variable**
```
$maVariable
```

**Effacer une variable**
```
Remove-Variable -Name "maVariable"
```

**Obtenir l'adresse IP de la machine**
```
Get-NetIPAddress
```

**Tester la connectivité réseau (Ping)**
```
Test-Connection -ComputerName "google.com"
```
