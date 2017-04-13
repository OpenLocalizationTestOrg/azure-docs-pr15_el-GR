<properties
   pageTitle="Προσαρμοσμένη δέσμη ενεργειών επέκταση σε μια Εικονική Windows | Microsoft Azure"
   description="Αυτοματοποίηση εργασιών ρύθμισης παραμέτρων Εικονική Azure χρησιμοποιώντας την επέκταση προσαρμοσμένη δέσμη ενεργειών για να εκτελούν δέσμες ενεργειών PowerShell σε έναν απομακρυσμένο Εικονική των Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# <a name="custom-script-extension-for-windows-virtual-machines"></a>Προσαρμοσμένη δέσμη ενεργειών επέκταση για εικονικές μηχανές Windows

Σε αυτό το άρθρο παρέχει μια επισκόπηση του τρόπου χρήσης την επέκταση προσαρμοσμένης δέσμης ενεργειών στο Windows ΣΠΣ με τη χρήση των cmdlet του Azure PowerShell με API διαχείρισης υπηρεσίας Azure.

Επεκτάσεις εικονική μηχανή (Εικονική) έχουν δημιουργηθεί από τη Microsoft και Αξιόπιστοι εκδότες τρίτων κατασκευαστών για να επεκτείνετε τη λειτουργικότητα του Εικονική. Για μια επισκόπηση των επεκτάσεων Εικονική, ανατρέξτε στο θέμα [επεκτάσεις Εικονική Azure και δυνατότητες](virtual-machines-windows-extensions-features.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα με τη χρήση του μοντέλου από διαχειριστή πόρων](virtual-machines-windows-extensions-customscript.md).

## <a name="custom-script-extension-overview"></a>Επισκόπηση επέκταση προσαρμοσμένη δέσμη ενεργειών

Με την επέκταση προσαρμοσμένης δέσμης ενεργειών των Windows, μπορείτε να εκτελέσετε δεσμών ενεργειών του PowerShell σε έναν απομακρυσμένο Εικονική χωρίς να εισέλθετε σε αυτό. Μπορείτε να εκτελέσετε τις δέσμες ενεργειών μετά την προμήθεια του Εικονική ή ανά πάσα στιγμή κατά τη διάρκεια του κύκλου ζωής της η Εικονική χωρίς να ανοίξω τυχόν πρόσθετες θύρες. Τα πιο συνηθισμένα περιπτώσεις χρήσης για την εκτέλεση προσαρμοσμένης δέσμης ενεργειών επέκταση περιλαμβάνουν εκτέλεση, την εγκατάσταση και ρύθμιση παραμέτρων επιπλέον λογισμικό σε η Εικονική μετά την παροχή της υπηρεσίας.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Προϋποθέσεις για την εκτέλεση της επέκτασης προσαρμοσμένης δέσμης ενεργειών

1. Εγκατάσταση των <a href="http://azure.microsoft.com/downloads" target="_blank">cmdlet του Azure PowerShell</a> έκδοση 0.8.0 ή νεότερη έκδοση.
2. Εάν θέλετε οι δέσμες ενεργειών για να εκτελέσετε σε μια υπάρχουσα Εικονική, βεβαιωθείτε ότι είναι ενεργοποιημένο Εικονική παράγοντας στην η Εικονική. Εάν δεν είναι εγκατεστημένο, ακολουθήστε τα παρακάτω [βήματα](virtual-machines-windows-classic-agents-and-extensions.md). Εάν η Εικονική δημιουργείται από την πύλη του Azure, παράγοντας Εικονική είναι εγκατεστημένο από προεπιλογή.
3. Κάντε αποστολή των δεσμών ενεργειών που θέλετε να εκτελέσετε στο την εικονική Μηχανή με το χώρο αποθήκευσης Azure. Οι δέσμες ενεργειών μπορεί να προέρχονται από ένα μεμονωμένο κοντέινερ ή πολλούς χώρους αποθήκευσης.
4. Θα πρέπει να είναι η σύνταξη της δέσμης ενεργειών, ώστε να ξεκινά η δέσμη ενεργειών καταχώρησης, ο οποίος ξεκινά από την επέκταση, άλλες δέσμες ενεργειών.

## <a name="custom-script-extension-scenarios"></a>Προσαρμοσμένη δέσμη ενεργειών επέκταση σενάρια

### <a name="upload-files-to-the-default-container"></a>Αποστολή αρχείων στο προεπιλεγμένο κοντέινερ

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να εκτελέσετε τις δέσμες ενεργειών σε η Εικονική εάν βρίσκονται στο κοντέινερ χώρου αποθήκευσης του προεπιλεγμένου λογαριασμού της συνδρομής σας. Μπορείτε να αποστείλετε τις δέσμες ενεργειών σε ContainerName. Μπορείτε να επαληθεύσετε τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης, χρησιμοποιώντας το **Get-AzureSubscription – προεπιλεγμένη** εντολή.

Το ακόλουθο παράδειγμα δημιουργεί μια Εικονική, αλλά μπορείτε επίσης να εκτελέσετε το ίδιο σενάριο σε μια υπάρχουσα Εικονική.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Αποστολή αρχείων σε ένα κοντέινερ χώρου αποθήκευσης μη προεπιλεγμένες

Αυτό το σενάριο δείχνει πώς μπορείτε να χρησιμοποιήσετε ένα κοντέινερ χώρου αποθήκευσης μη προεπιλεγμένες μέσα την ίδια συνδρομή ή σε μια διαφορετική συνδρομή για την αποστολή αρχείων και δεσμών ενεργειών. Αυτό το παράδειγμα εμφανίζει μια υπάρχουσα Εικονική, αλλά οι ίδιες ενέργειες μπορεί να γίνει όταν θέλετε να δημιουργήσετε μια Εικονική.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Αποστολή δέσμες ενεργειών σε πολλές κοντέινερ σε διαφορετικό χώρο αποθήκευσης λογαριασμούς

  Εάν τα αρχεία δέσμης ενεργειών είναι αποθηκευμένα σε πολλές κοντέινερ, πρέπει να παρέχετε την πλήρη πρόσβαση κοινόχρηστο υπογραφές (συσχετισμών Ασφαλείας) διεύθυνση URL για τα αρχεία για να εκτελέσετε τις δέσμες ενεργειών.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>Προσθέστε την επέκταση προσαρμοσμένης δέσμης ενεργειών από την πύλη του Azure

Μεταβείτε στο η Εικονική στην <a href="https://portal.azure.com/ " target="_blank">πύλη του Azure</a> και προσθέστε την επέκταση, καθορίζοντας το αρχείο δέσμης ενεργειών για να εκτελέσετε.

  ![Καθορίστε το αρχείο δέσμης ενεργειών][5]


### <a name="uninstall-the-custom-script-extension"></a>Κατάργηση της εγκατάστασης της επέκτασης προσαρμοσμένης δέσμης ενεργειών

Μπορείτε να καταργήσετε την εγκατάσταση την επέκταση προσαρμοσμένης δέσμης ενεργειών από την εικονική Μηχανή χρησιμοποιώντας την ακόλουθη εντολή.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Χρησιμοποιήστε την επέκταση προσαρμοσμένης δέσμης ενεργειών με τα πρότυπα

Για να μάθετε πώς μπορείτε να χρησιμοποιήσετε την επέκταση προσαρμοσμένης δέσμης ενεργειών με τα πρότυπα για τη διαχείριση πόρων Azure, ανατρέξτε στο θέμα [Χρήση επέκταση της προσαρμοσμένης δέσμης ενεργειών για Windows ΣΠΣ με τα πρότυπα για τη διαχείριση πόρων Azure](virtual-machines-windows-extensions-customscript.md).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png
