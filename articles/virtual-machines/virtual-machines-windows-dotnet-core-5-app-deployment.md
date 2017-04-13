<properties
   pageTitle="Αυτοματοποίηση ανάπτυξη εφαρμογών με επεκτάσεις εικονική μηχανή | Microsoft Azure"
   description="Πρόγραμμα εκμάθησης πυρήνα DotNet Azure εικονική μηχανή"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Ανάπτυξη εφαρμογών με τα πρότυπα για τη διαχείριση πόρων Azure

Όταν όλες οι Azure απαιτήσεις υποδομής έχουν που έχουν προσδιοριστεί και μετατραπεί σε ένα πρότυπο ανάπτυξης, την ανάπτυξη εφαρμογών πραγματική πρέπει να εκτελούνται. Ανάπτυξη εφαρμογών εδώ αναφέρεται σε κατά την εγκατάσταση του δυαδικά αρχεία πραγματική εφαρμογών σε Azure πόρους. Για το δείγμα κατάστημα μουσικής, .net πυρήνα και των υπηρεσιών IIS πρέπει να είναι εγκατεστημένη και ρυθμισμένη σε κάθε εικονική μηχανή. Τα δυαδικά αρχεία μουσικής Store πρέπει να εγκατασταθούν στο μηχάνημα εικονικές και τη βάση δεδομένων αποθήκευσης μουσικής δημιουργηθεί εκ των προτέρων.

Πώς να αυτοματοποιήσετε εικονική μηχανή επεκτάσεις εφαρμογής ανάπτυξης και ρύθμισης παραμέτρων για να Azure εικονικές μηχανές λεπτομέρειες για αυτό το έγγραφο. Επισημαίνονται όλων των εξαρτήσεων και ρυθμίσεις παραμέτρων μοναδικών τιμών. Για βέλτιστη εμπειρία, η προ-ανάπτυξη μια παρουσία της λύσης για να σας Azure συνδρομής και λειτουργούν μαζί με το πρότυπο διαχείρισης πόρων Azure. Το πρότυπο ολοκλήρωσης μπορείτε να βρείτε εδώ – [Μουσικής ανάπτυξης Store στα Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-Windows).

## <a name="configuration-script"></a>Δέσμη ενεργειών ρύθμισης παραμέτρων

Εικονική μηχανή επεκτάσεις είναι εξειδικευμένες τα προγράμματα που εκτελούνται σε εικονικές μηχανές για την παροχή αυτοματισμού ρύθμισης παραμέτρων. Επεκτάσεις είναι διαθέσιμες για πολλές συγκεκριμένες σκοπούς όπως προστασίας από ιούς, ρύθμιση παραμέτρων καταγραφής και ρύθμισης παραμέτρων Docker. Η επέκταση προσαρμοσμένης δέσμης ενεργειών μπορεί να χρησιμοποιηθεί για να εκτελέσετε τις δέσμες ενεργειών σε σχέση με μια εικονική μηχανή. Με το δείγμα κατάστημα μουσικής, είναι έως και την επέκταση προσαρμοσμένη δέσμη ενεργειών για να ρυθμίσετε τις παραμέτρους του εικονικές μηχανές Windows και εγκαταστήστε την εφαρμογή κατάστημα μουσικής.

Πριν από την που περιγράφει με λεπτομέρεια πώς επεκτάσεις εικονική μηχανή δηλώνονται σε ένα πρότυπο από διαχειριστή πόρων Azure, εξετάστε τη δέσμη ενεργειών που εκτελείται. Αυτή η δέσμη ενεργειών ρυθμίζει τις παραμέτρους του Windows εικονική μηχανή για τη φιλοξενία της εφαρμογής αποθήκευσης μουσικής. Κατά την εκτέλεση, η δέσμη ενεργειών εγκαθιστά όλα είναι απαραίτητο λογισμικό, εγκαταστήστε την εφαρμογή store μουσική από το στοιχείο ελέγχου προέλευσης και προετοιμασία της βάσης δεδομένων. 

> Αυτό το δείγμα είναι για σκοπούς επίδειξης.

```none
Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# install iis
Install-WindowsFeature web-server -IncludeManagementTools

# install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# download / config music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceserver>", $sqlserver } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceuser>", $user } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replacepass>", $password } | Set-Content C:\music\config.json

# workaround for db creation bug
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>Επέκταση Εικονική δέσμης ενεργειών

Εικονική επεκτάσεις μπορεί να εκτελεστεί σε σχέση με μια εικονική μηχανή κατά το χρόνο δημιουργίας, συμπεριλαμβάνοντας τον πόρο επέκταση στο πρότυπο της διαχείρισης πόρων Azure. Η επέκταση μπορούν να προστεθούν με τον οδηγό του Visual Studio Προσθήκη πόρων ή με την εισαγωγή έγκυρη JSON στο πρότυπο. Ο πόρος επέκταση δέσμης ενεργειών είναι ένθετο μέσα σε τον πόρο εικονική μηχανή. Αυτό μπορούν να προβληθούν στο παρακάτω παράδειγμα.

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Εικονική επέκταση δέσμης ενεργειών](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339). 

Παρατηρήστε στο το παρακάτω JSON ότι η δέσμη ενεργειών είναι αποθηκευμένη στο GitHub. Αυτή η δέσμη ενεργειών επίσης μπορεί να είναι αποθηκευμένα στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Επίσης, τα πρότυπα διαχείρισης πόρων Azure επιτρέπουν τη συμβολοσειρά εκτέλεσης δέσμης ενεργειών προς κατασκευή τέτοια ώστε τιμές παραμέτρων προτύπου μπορούν να χρησιμοποιηθούν ως παράμετροι για εκτέλεση δέσμης ενεργειών. Σε αυτήν την περίπτωση δεδομένα παρέχονται κατά την ανάπτυξη των προτύπων και, στη συνέχεια, μπορεί να χρησιμοποιηθεί αυτές τις τιμές κατά την εκτέλεση της δέσμης ενεργειών.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Για περισσότερες πληροφορίες σχετικά με τη χρήση της επέκτασης προσαρμοσμένη δέσμη ενεργειών, ανατρέξτε στο θέμα [Προσαρμογή των επεκτάσεων δέσμης ενεργειών με τα πρότυπα για τη διαχείριση πόρων](./virtual-machines-windows-extensions-customscript.md).

## <a name="next-step"></a>Επόμενο βήμα

<hr>

[Εξερευνήστε Azure περισσότερα πρότυπα διαχείρισης πόρων](https://github.com/Azure/azure-quickstart-templates)
