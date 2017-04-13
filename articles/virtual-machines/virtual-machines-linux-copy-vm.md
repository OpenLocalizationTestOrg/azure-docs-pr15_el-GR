<properties
    pageTitle="Δημιουργήστε ένα αντίγραφο του σας Εικονική Linux Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα αντίγραφο του σας Azure Linux εικονική μηχανή στο μοντέλο ανάπτυξης για τη διαχείριση πόρων"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>

# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure"></a>Δημιουργήστε ένα αντίγραφο της μια εικονική μηχανή Linux εκτελείται σε Azure


Αυτό το άρθρο σας δείχνει πώς μπορείτε να δημιουργήσετε ένα αντίγραφο του Azure εικονικές τον υπολογιστή σας (Εικονική) με χρήση του μοντέλου ανάπτυξης διαχείρισης πόρων Linux. Πρώτα αντιγραφή πάνω από το λειτουργικό σύστημα και δίσκων δεδομένων σε ένα νέο κοντέινερ, στη συνέχεια, ρύθμιση πόρους δικτύου και δημιουργήστε τη νέα εικονική μηχανή.

Μπορείτε επίσης να [αποστείλετε και να δημιουργήσετε μια Εικονική από εικόνα προσαρμοσμένης δίσκου](virtual-machines-linux-upload-vhd.md).


## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Βεβαιωθείτε ότι πληροίτε τις ακόλουθες προϋποθέσεις, πριν να ξεκινήσετε τα βήματα:

- Έχετε [Azure CLI] (... / xplat-cli-install.md) λήψη και εγκατάσταση στον υπολογιστή σας. 

- Χρειάζεστε επίσης ορισμένες πληροφορίες σχετικά με τις υπάρχουσες Εικονική Linux Azure:

| Πληροφορίες εικονική Μηχανή προέλευσης | Πού να το λάβετε |
|------------|-----------------|
| Εικονική όνομα | `azure vm list` |
| Όνομα ομάδας πόρων | `azure vm list` |
| Θέση | `azure vm list` |
| Όνομα λογαριασμού χώρου αποθήκευσης | `azure storage account list -g <resourceGroup>` |
| Το όνομα του κοντέινερ | `azure storage container list -a <sourcestorageaccountname>` |
| Όνομα του αρχείου VHD εικονική Μηχανή προέλευσης | `azure storage blob list --container <containerName>` |



- Θα πρέπει να κάνετε ορισμένες επιλογές σχετικά με το νέο Εικονική:   <br> -Όνομα κοντέινερ   <br> Εικονική - όνομα   <br> Εικονική - μέγεθος   <br> όνομα - vNet   <br> -Όνομα υποδίκτυο   <br> Όνομα - IP   <br> -NIC όνομα
    

## <a name="login-and-set-your-subscription"></a>Σύνδεση και να ορίσετε τη συνδρομή σας

1. Συνδεθείτε στο το CLI.
        
        azure login

2. Βεβαιωθείτε ότι βρίσκεστε σε λειτουργία διαχείρισης πόρων.
    
        azure config mode arm

3. Ορίστε τη σωστή συνδρομή. Μπορείτε να χρησιμοποιήσετε 'λογαριασμός azure λίστα' για να δείτε όλες τις συνδρομές σας.

        azure account set <SubscriptionId>



## <a name="stop-the-vm"></a>Διακοπή την εικονική Μηχανή 

Διακοπή και εκχώρηση την προέλευση Εικονική. Μπορείτε να χρησιμοποιήσετε 'λίστα azure εικονική' για να λάβετε μια λίστα όλων των του ΣΠΣ στη συνδρομή σας και τους πόρων ονόματα ομάδων.
    
        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## <a name="copy-the-vhd"></a>Αντιγράψτε τον VHD


Μπορείτε να αντιγράψετε τον VHD από το χώρο αποθήκευσης προέλευσης στο προορισμού χρησιμοποιώντας το `azure storage blob copy start`. Σε αυτό το παράδειγμα, πρόκειται να αντιγράψετε τον VHD τον ίδιο λογαριασμό χώρου αποθήκευσης, αλλά διαφορετικό κοντέινερ.

Για να αντιγράψετε τον VHD στο άλλο κοντέινερ στον ίδιο λογαριασμό χώρου αποθήκευσης, πληκτρολογήστε:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>
        

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Ρύθμιση του εικονικού δικτύου για το νέο Εικονική

Δημιουργία εικονικού δικτύου και NIC για το νέο Εικονική. 

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## <a name="create-the-new-vm"></a>Δημιουργία του νέου Εικονική 

Μπορείτε πλέον να δημιουργήσετε μια Εικονική από σας [χρησιμοποιώντας ένα πρότυπο διαχείρισης πόρων](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) έχουν αποσταλεί εικονικού δίσκου ή μέσω του CLI, καθορίζοντας το URI το αντιγραμμένο δίσκο, πληκτρολογώντας:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure CLI για να διαχειριστείτε την νέα εικονική μηχανή σας, ανατρέξτε στο θέμα [Azure CLI εντολές για τη διαχείριση πόρων Azure](azure-cli-arm-commands.md).
