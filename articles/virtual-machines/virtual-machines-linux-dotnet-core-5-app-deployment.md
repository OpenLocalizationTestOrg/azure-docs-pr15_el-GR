<properties
   pageTitle="Αυτοματοποίηση ανάπτυξη εφαρμογών με επεκτάσεις εικονική μηχανή | Microsoft Azure"
   description="Πρόγραμμα εκμάθησης πυρήνα DotNet Azure εικονική μηχανή"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Ανάπτυξη εφαρμογών με τα πρότυπα για τη διαχείριση πόρων Azure

Όταν όλες οι Azure απαιτήσεις υποδομής έχουν που έχουν προσδιοριστεί και μετατραπεί σε ένα πρότυπο ανάπτυξης, την ανάπτυξη εφαρμογών πραγματική πρέπει να εκτελούνται. Ανάπτυξη εφαρμογών εδώ αναφέρεται σε κατά την εγκατάσταση του δυαδικά αρχεία πραγματική εφαρμογών σε Azure πόρους. Για το δείγμα κατάστημα μουσικής, .net πυρήνα, NGINX και επόπτη πρέπει να είναι εγκατεστημένη και ρυθμισμένη σε κάθε εικονική μηχανή. Τα δυαδικά αρχεία μουσικής Store πρέπει να εγκατασταθούν στο μηχάνημα εικονικές και τη βάση δεδομένων αποθήκευσης μουσικής δημιουργηθεί εκ των προτέρων.

Πώς να αυτοματοποιήσετε εικονική μηχανή επεκτάσεις εφαρμογής ανάπτυξης και ρύθμισης παραμέτρων για να Azure εικονικές μηχανές λεπτομέρειες για αυτό το έγγραφο. Επισημαίνονται όλων των εξαρτήσεων και ρυθμίσεις παραμέτρων μοναδικών τιμών. Για βέλτιστη εμπειρία, η προ-ανάπτυξη μια παρουσία της λύσης για να σας Azure συνδρομής και λειτουργούν μαζί με το πρότυπο διαχείρισης πόρων Azure. Το πρότυπο ολοκλήρωσης μπορείτε να βρείτε εδώ – [Ανάπτυξης Store μουσικής στο Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Δέσμη ενεργειών ρύθμισης παραμέτρων

Εικονική μηχανή επεκτάσεις είναι εξειδικευμένες τα προγράμματα που εκτελούνται σε εικονικές μηχανές για την παροχή αυτοματισμού ρύθμισης παραμέτρων. Επεκτάσεις είναι διαθέσιμες για πολλές συγκεκριμένες σκοπούς όπως προστασίας από ιούς, ρύθμιση παραμέτρων καταγραφής και ρύθμισης παραμέτρων Docker. Επέκταση προσαρμοσμένων δεσμών ενεργειών μπορεί να χρησιμοποιηθεί για την εκτέλεση οποιαδήποτε δέσμη ενεργειών σε μια εικονική μηχανή. Με το δείγμα κατάστημα μουσικής, είναι έως και την επέκταση προσαρμοσμένη δέσμη ενεργειών για να ρυθμίσετε τις παραμέτρους του Ubuntu εικονικές μηχανές και εγκαταστήστε την εφαρμογή κατάστημα μουσικής.

Πριν από την που περιγράφει με λεπτομέρεια πώς επεκτάσεις εικονική μηχανή δηλώνονται σε ένα πρότυπο από διαχειριστή πόρων Azure, εξετάστε τη δέσμη ενεργειών που εκτελείται. Αυτή η δέσμη ενεργειών ρυθμίζει τις παραμέτρους η εικονική μηχανή Ubuntu για τη φιλοξενία της εφαρμογής μουσικής αποθήκευσης. Κατά την εκτέλεση, η δέσμη ενεργειών εγκαθιστά όλα είναι απαραίτητο λογισμικό, εγκαταστήστε την εφαρμογή store μουσική από το στοιχείο ελέγχου προέλευσης και προετοιμασία της βάσης δεδομένων. 

Για να μάθετε περισσότερα σχετικά με τη φιλοξενία ένα .net βασική εφαρμογή Linux, ανατρέξτε στο θέμα [δημοσίευση σε ένα περιβάλλον παραγωγής Linux](https://docs.asp.net/en/latest/publishing/linuxproduction.html). 

> Αυτό το δείγμα είναι για σκοπούς επίδειξης.

```none
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>Επέκταση Εικονική δέσμης ενεργειών

Εικονική επεκτάσεις μπορεί να εκτελεστεί σε σχέση με μια εικονική μηχανή κατά το χρόνο δημιουργίας, συμπεριλαμβάνοντας τον πόρο επέκταση στο πρότυπο της διαχείρισης πόρων Azure. Η επέκταση μπορούν να προστεθούν με τον οδηγό του Visual Studio Προσθήκη πόρων ή με την εισαγωγή έγκυρη JSON στο πρότυπο. Ο πόρος επέκταση δέσμης ενεργειών είναι ένθετο μέσα σε τον πόρο εικονική μηχανή. Αυτό μπορούν να προβληθούν στο παρακάτω παράδειγμα.

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Εικονική επέκταση δέσμης ενεργειών](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359). 

Παρατηρήστε στο το παρακάτω JSON ότι η δέσμη ενεργειών είναι αποθηκευμένη στο GitHub. Αυτή η δέσμη ενεργειών επίσης μπορεί να είναι αποθηκευμένα στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Επίσης, τα πρότυπα διαχείρισης πόρων Azure επιτρέπουν τη συμβολοσειρά εκτέλεσης δέσμης ενεργειών για να συνταχθεί έτσι ώστε οι τιμές παραμέτρων προτύπου μπορούν να χρησιμοποιηθούν ως παράμετροι για εκτέλεση δέσμης ενεργειών. Σε αυτήν την περίπτωση δεδομένα παρέχονται κατά την ανάπτυξη των προτύπων και, στη συνέχεια, μπορεί να χρησιμοποιηθεί αυτές τις τιμές κατά την εκτέλεση της δέσμης ενεργειών.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Για περισσότερες πληροφορίες σχετικά με τη χρήση της επέκτασης προσαρμοσμένη δέσμη ενεργειών, ανατρέξτε στο θέμα [Προσαρμογή των επεκτάσεων δέσμης ενεργειών με τα πρότυπα για τη διαχείριση πόρων](./virtual-machines-linux-extensions-customscript.md).

## <a name="next-step"></a>Επόμενο βήμα

<hr>

[Εξερευνήστε Azure περισσότερα πρότυπα διαχείρισης πόρων](https://github.com/Azure/azure-quickstart-templates)
