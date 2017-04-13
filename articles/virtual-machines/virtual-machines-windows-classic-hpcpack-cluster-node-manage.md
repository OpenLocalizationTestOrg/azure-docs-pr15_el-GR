<properties
 pageTitle="Διαχείριση κόμβους υπολογιστικών σύμπλεγμα HPC πακέτο | Microsoft Azure"
 description="Μάθετε περισσότερα σχετικά με τα εργαλεία δέσμης ενεργειών του PowerShell για προσθήκη, κατάργηση, Έναρξη και διακοπή κόμβους υπολογιστικών σύμπλεγμα HPC πακέτο στο Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Διαχειριστείτε τον αριθμό και διαθεσιμότητα κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure

Εάν έχετε δημιουργήσει ένα σύμπλεγμα HPC πακέτο σε ΣΠΣ Azure, ίσως θέλετε τρόποι εύκολα Προσθήκη, κατάργηση, ξεκινήστε (προετοιμασία) ή διακοπή (διαχειριστείτε) έναν αριθμό υπολογισμού κόμβο ΣΠΣ στο σύμπλεγμα. Για να κάνετε αυτές τις εργασίες, εκτελέστε δεσμών ενεργειών του Azure PowerShell που είναι εγκατεστημένα στον κόμβο κεφαλής Εικονική. Αυτές οι δέσμες ενεργειών σάς βοηθούν να ελέγχετε τον αριθμό και τη διαθεσιμότητα των πόρων σας σύμπλεγμα HPC πακέτο, ώστε να μπορείτε να ελέγχετε κόστους.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* **Σύμπλεγμα HPC πακέτο στο ΣΠΣ Azure** - Δημιουργήστε ένα σύμπλεγμα HPC πακέτο στο μοντέλο κλασική ανάπτυξης χρησιμοποιώντας τουλάχιστον HPC πακέτο 2012 R2 ενημέρωση 1. Για παράδειγμα, μπορείτε να αυτοματοποιήσετε την ανάπτυξη, χρησιμοποιώντας την τρέχουσα εικόνα Εικονική πακέτο HPC στο το Azure Marketplace και μια δέσμη ενεργειών του Azure PowerShell. Για πληροφορίες και τις προϋποθέσεις, ανατρέξτε στο θέμα [Δημιουργία ένα σύμπλεγμα HPC με τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

    Μετά την ανάπτυξη, βρείτε τον κόμβο δέσμες ενεργειών διαχείρισης στο Παράθυρο %\_ΚΕΝΤΡΙΚΉ φάκελο bin % στον κόμβο κεφαλίδας. Πρέπει να εκτελέσετε κάθε μία από τις δέσμες ενεργειών ως διαχειριστής.

* **Πιστοποιητικό αρχείου ή Διαχείριση ρυθμίσεων δημοσίευση azure** - πρέπει να κάνετε ένα από τα εξής στον κόμβο κεφαλίδας:

    * **Αρχείο ρυθμίσεων δημοσίευση εισαγωγής το Azure**. Για να το κάνετε αυτό, εκτελέστε τα ακόλουθα cmdlet του PowerShell Azure στον κόμβο κεφαλίδας:

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **Ρύθμιση παραμέτρων του πιστοποιητικού Azure διαχείρισης στον κόμβο κεφαλίδας**. Εάν έχετε εγκαταστήσει το αρχείο .cer, εισαγάγετέ το στο χώρο αποθήκευσης πιστοποιητικών CurrentUser\My και, στη συνέχεια, εκτελέστε το ακόλουθο cmdlet του PowerShell Azure για το περιβάλλον Azure (AzureCloud ή AzureChinaCloud):

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Προσθήκη κόμβος ΣΠΣ

Προσθήκη κόμβους υπολογιστικών με τη δέσμη ενεργειών **Προσθήκη HpcIaaSNode.ps1** .

### <a name="syntax"></a>Σύνταξη
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Παράμετροι

* **Όνομα_υπηρεσίας** - όνομα του στο cloud υπηρεσίας που νέα κόμβος ΣΠΣ θα προστεθούν στο.

* **Όνομα εικόνας** - Azure Εικονική εικόνα όνομα, το οποίο μπορεί να ληφθεί από το Azure κλασική πύλη ή Azure PowerShell cmdlet **Get-AzureVMImage**. Η εικόνα πρέπει να ικανοποιεί τις ακόλουθες απαιτήσεις:

    1. Πρέπει να έχει εγκατασταθεί λειτουργικό σύστημα Windows.

    2. Πακέτο HPC πρέπει να έχει εγκατασταθεί στο ρόλο κόμβο υπολογισμού.

    3. Η εικόνα πρέπει να είναι μια ιδιωτική εικόνα στην κατηγορία χρήστη, όχι μια δημόσια Azure Εικονική εικόνα.

* **Ποσότητα** - αριθμός κόμβος ΣΠΣ θα προστεθούν.

* **InstanceSize** - μέγεθος του κόμβου υπολογισμού ΣΠΣ.

* **DomainUserName** - όνομα χρήστη τομέα που θα χρησιμοποιηθεί για τη σύνδεση του νέου ΣΠΣ με τον τομέα.

* **DomainUserPassword** - τον κωδικό πρόσβασης του χρήστη για τον τομέα σας.

* **NodeNameSeries** (προαιρετικά) - ονομασία μοτίβο για τους κόμβους υπολογισμού. Πρέπει να είναι η μορφή &lt; *ριζικό\_όνομα*&gt;&lt;*Έναρξη\_αριθμό*&gt;%. Για παράδειγμα, MyCN % 10% σημαίνει μια σειρά των ονομάτων κόμβο υπολογισμού ξεκινώντας από MyCN11. Εάν δεν καθορίζονται, η δέσμη ενεργειών χρησιμοποιεί τον κόμβο ρυθμισμένο ονομασία σειρά στο σύμπλεγμα HPC.

### <a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα προσθέτει 20 μέγεθος μεγάλο κόμβος ΣΠΣ στη το cloud υπηρεσίας *hpcservice1*, με βάση την εικόνα Εικονική *hpccnimage1*.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Κατάργηση κόμβος ΣΠΣ

Κατάργηση κόμβους υπολογιστικών με τη δέσμη ενεργειών **HpcIaaSNode.ps1 κατάργηση** .

### <a name="syntax"></a>Σύνταξη

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Παράμετροι

* **Όνομα** - ονόματα των κόμβοι συμπλέγματος να καταργηθούν. Υποστηρίζονται χαρακτήρες μπαλαντέρ. Το όνομα του συνόλου παραμέτρων είναι όνομα. Δεν μπορείτε να καθορίσετε το **όνομα** και τη **κόμβο** παράμετροι.

* **Κόμβος** - HpcNode το αντικείμενο για τους κόμβους να καταργηθούν, που μπορεί να ληφθεί έως το cmdlet του HPC PowerShell [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Το όνομα του συνόλου παραμέτρων είναι κόμβος. Δεν μπορείτε να καθορίσετε το **όνομα** και τη **κόμβο** παράμετροι.

* **DeleteVHD** (προαιρετικά) - τη ρύθμιση για να διαγράψετε το σχετικό δίσκων για το ΣΠΣ που έχουν καταργηθεί.

* **Δύναμη** (προαιρετικά) - τη ρύθμιση για να επιβάλετε κόμβους HPC χωρίς σύνδεση πριν να καταργήσετε τους.

* **Επιβεβαίωση** (προαιρετικό) - ερώτηση επιβεβαίωσης πριν από την εκτέλεση της εντολής.

* **WhatIf** - τη ρύθμιση για να περιγράψετε τι θα συμβεί εάν εκτελέσατε την εντολή χωρίς στην πραγματικότητα εκτέλεση της εντολής.

### <a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα επιβάλει κόμβους χωρίς σύνδεση με τα ονόματα αρχίζουν *HPCNode-CN -* και τους καταργεί τους κόμβους και σχετικές δίσκων τους.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Έναρξη κόμβος ΣΠΣ

Ξεκινήστε κόμβους υπολογιστικών με τη δέσμη ενεργειών **HpcIaaSNode.ps1 Έναρξη** .

### <a name="syntax"></a>Σύνταξη

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Παράμετροι

* **Όνομα** - ονόματα από τους κόμβους συμπλέγματος για να ξεκινήσετε. Υποστηρίζονται χαρακτήρες μπαλαντέρ. Το όνομα του συνόλου παραμέτρων είναι όνομα. Δεν μπορείτε να καθορίσετε το **όνομα** και τη **κόμβο** παράμετροι.

* **Κόμβος**- HpcNode το αντικείμενο για τους κόμβους για να ξεκινήσετε, που μπορεί να ληφθεί έως το cmdlet του HPC PowerShell [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Το όνομα του συνόλου παραμέτρων είναι κόμβος. Δεν μπορείτε να καθορίσετε το **όνομα** και τη **κόμβο** παράμετροι.

### <a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα ξεκινά κόμβους με ονόματα αρχίζουν *HPCNode-CN -*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Διακοπή κόμβος ΣΠΣ

Διακοπή κόμβους υπολογιστικών με τη δέσμη ενεργειών **HpcIaaSNode.ps1 διακοπή** .

### <a name="syntax"></a>Σύνταξη

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Παράμετροι


* **Όνομα**- ονόματα από τους κόμβους συμπλέγματος διακοπή. Υποστηρίζονται χαρακτήρες μπαλαντέρ. Το όνομα του συνόλου παραμέτρων είναι όνομα. Δεν μπορείτε να καθορίσετε το **όνομα** και τη **κόμβο** παράμετροι.

* **Κόμβος** - HpcNode το αντικείμενο για τους κόμβους για να διακοπεί, που μπορεί να ληφθεί έως το cmdlet του HPC PowerShell [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Το όνομα του συνόλου παραμέτρων είναι κόμβος. Δεν μπορείτε να καθορίσετε το **όνομα** και τη **κόμβο** παράμετροι.

* **Δύναμη** (προαιρετικά) - τη ρύθμιση για να επιβάλετε κόμβους HPC χωρίς σύνδεση πριν από τη διακοπή τους.

### <a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα επιβάλει κόμβους χωρίς σύνδεση με τα ονόματα αρχίζουν *HPCNode-CN -* και, στη συνέχεια, σταματά τους κόμβους.

Διακοπή-HPCIaaSNode.ps1 – HPCNodeCN όνομα-*-δύναμη

## <a name="next-steps"></a>Επόμενα βήματα

* Εάν θέλετε ένας τρόπος για την αυτόματη μεγέθυνση ή σμίκρυνση τους κόμβους συμπλέγματος σύμφωνα με το τρέχον φόρτο εργασίας των εργασιών και των εργασιών στο σύμπλεγμα, ανατρέξτε στο θέμα [Αυτόματη μεγέθυνση και σμίκρυνση τους πόρους σύμπλεγμα HPC πακέτο στο Azure σύμφωνα με το φόρτο εργασίας σύμπλεγμα](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).
