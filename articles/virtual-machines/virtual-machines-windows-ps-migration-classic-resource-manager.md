<properties
    pageTitle="Μετεγκατάσταση για τη διαχείριση πόρων με το PowerShell | Microsoft Azure"
    description="Σε αυτό το άρθρο παρουσιάζει τη μετεγκατάσταση που υποστηρίζονται πλατφόρμα IaaS πόρων από κλασική για τη διαχείριση πόρων Azure, χρησιμοποιώντας τις εντολές του Azure PowerShell"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Μετεγκατάσταση IaaS πόρους από κλασική διαχείριση πόρων για να Azure με χρήση του Azure PowerShell

Αυτά τα βήματα σας δείχνουν πώς μπορείτε να χρησιμοποιήσετε τις εντολές του Azure PowerShell για τη μετεγκατάσταση υποδομής ως πόροι υπηρεσίας (IaaS) από το μοντέλο κλασική ανάπτυξης στο μοντέλο ανάπτυξης διαχείρισης πόρων Azure. 

Εάν θέλετε, μπορείτε επίσης να μετεγκαταστήσετε πόρους, χρησιμοποιώντας το [περιβάλλον γραμμής εντολών Azure (Azure CLI)](virtual-machines-linux-cli-migration-classic-resource-manager.md).

- Για σενάρια υποστηριζόμενες μετεγκατάστασης, ανατρέξτε στο θέμα [υποστηριζόμενες πλατφόρμες μετεγκατάσταση IaaS πόρων από κλασική διαχείριση πόρων για να Azure](virtual-machines-windows-migration-classic-resource-manager.md). 
- Για λεπτομερείς οδηγίες και μετεγκατάστασης αναλυτικές οδηγίες, ανατρέξτε στο θέμα [τεχνική βαθύ κατάρρευση στη μετεγκατάσταση πλατφόρμα που υποστηρίζονται από κλασική διαχείριση πόρων για να Azure](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).

## <a name="step-1-plan-for-migration"></a>Βήμα 1: Σχεδιασμός για τη μετεγκατάσταση

Ακολουθούν ορισμένες βέλτιστες πρακτικές που συνιστάται κατά την αξιολόγηση μετεγκατάσταση IaaS πόρους από κλασική διαχείριση πόρων για να:

- Διαβάστε τις [υποστηριζόμενες και μη υποστηριζόμενες δυνατότητες και τις ρυθμίσεις παραμέτρων](virtual-machines-windows-migration-classic-resource-manager.md). Εάν έχετε εικονικές μηχανές που χρησιμοποιούν ρυθμίσεις δεν υποστηρίζονται ή δυνατότητες, συνιστάται να περιμένετε την υποστήριξη/δυνατότητα ρύθμισης παραμέτρων για να ανακοινώσει. Εναλλακτικά, εάν να ανταποκρίνεται στις ανάγκες σας, καταργήστε αυτήν τη δυνατότητα ή μετακινήστε από ότι η ρύθμιση παραμέτρων για να ενεργοποιήσετε την μετεγκατάσταση.
-   Εάν έχετε αυτοματοποιημένη δεσμών ενεργειών που σήμερα ανάπτυξη υποδομή και τις εφαρμογές σας, δοκιμάστε να δημιουργήσετε μια παρόμοια ρύθμιση δοκιμής χρησιμοποιώντας αυτές τις δέσμες ενεργειών για τη μετεγκατάσταση. Εναλλακτικά, μπορείτε να ρυθμίσετε δείγμα περιβάλλοντα, χρησιμοποιώντας την πύλη του Azure.

>[AZURE.IMPORTANT]Πύλες εικονικού δικτύου (--τοποθεσίας, Azure ExpressRoute, πύλη εφαρμογής, σημείο σε τοποθεσία) δεν υποστηρίζονται αυτήν τη στιγμή για τη μετεγκατάσταση από κλασική για τη διαχείριση πόρων. Για τη μετεγκατάσταση ενός κλασική εικονικού δικτύου με μια πύλη, καταργήστε την πύλη πριν από την εκτέλεση μιας λειτουργίας ολοκλήρωσης για να μετακινήσετε το δίκτυο (μπορείτε να εκτελέσετε το βήμα προετοιμασία χωρίς να διαγράψετε την πύλη). Μετά την ολοκλήρωση της μετεγκατάστασης, συνδεθείτε ξανά την πύλη στη Διαχείριση Azure πόρων.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Βήμα 2: Εγκαταστήστε την πιο πρόσφατη έκδοση του Azure PowerShell

Υπάρχουν δύο επιλογές κύριο για να εγκαταστήσετε το Azure PowerShell: [Συλλογή PowerShell](https://www.powershellgallery.com/profiles/azure-sdk/) ή [Web πλατφόρμα Installer (WebPI)](http://aka.ms/webpi-azps). WebPI λαμβάνει μηνιαίες ενημερώσεις. Συλλογή PowerShell λαμβάνει ενημερώσεις σε συνεχή βάση. Σε αυτό το άρθρο βασίζεται σε Azure PowerShell έκδοση 2.1.0.

Για οδηγίες εγκατάστασης, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

<br>
## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>Βήμα 3: Ορίστε τη συνδρομή σας και να εγγραφούν για μετεγκατάσταση

Πρώτα, ξεκινήστε μια γραμμή εντολών του PowerShell. Για τη μετεγκατάσταση, πρέπει να ρυθμίσετε το περιβάλλον σας για δύο κλασική και διαχείριση πόρων.

Πραγματοποιήστε είσοδο στο λογαριασμό σας για το μοντέλο από διαχειριστή πόρων.

```powershell
    Login-AzureRmAccount
```

Λάβετε τις διαθέσιμες συνδρομές χρησιμοποιώντας την ακόλουθη εντολή:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Ορισμός Azure τη συνδρομή σας για την τρέχουσα περίοδο λειτουργίας. Αυτό το παράδειγμα ορίζει το προεπιλεγμένο όνομα της συνδρομής **Μου Azure συνδρομής**. Αντικαταστήστε το όνομα της συνδρομής παράδειγμα με το δικό σας. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

>[AZURE.NOTE] Δήλωση είναι ένα μοναδικό βήμα, αλλά πρέπει να το κάνετε αυτό μία φορά πριν να επιχειρήσετε μετεγκατάστασης. Χωρίς την εγγραφή σας, μπορείτε να δείτε το ακόλουθο μήνυμα σφάλματος: 

>   *BadRequest: Συνδρομή δεν έχει καταχωρηθεί για μετεγκατάσταση.* 

Καταχώρηση με την υπηρεσία παροχής πόρων μετεγκατάστασης, χρησιμοποιώντας την ακόλουθη εντολή:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Περιμένετε πέντε λεπτά για την καταχώρηση για να ολοκληρώσετε. Μπορείτε να ελέγξετε την κατάσταση έγκρισης, χρησιμοποιώντας την ακόλουθη εντολή:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Βεβαιωθείτε ότι είναι RegistrationState `Registered` πριν να συνεχίσετε. 

Πραγματοποιήστε είσοδο λογαριασμό σας για το μοντέλο κλασική.

```powershell
    Add-AzureAccount
```

Λάβετε τις διαθέσιμες συνδρομές χρησιμοποιώντας την ακόλουθη εντολή:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Ορισμός Azure τη συνδρομή σας για την τρέχουσα περίοδο λειτουργίας. Αυτό το παράδειγμα ορίζει την προεπιλεγμένη συνδρομή σε **Συνδρομή μου Azure**. Αντικαταστήστε το όνομα της συνδρομής παράδειγμα με το δικό σας. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Βήμα 4: Βεβαιωθείτε ότι έχετε αρκετό πυρήνων Azure διαχείριση πόρων εικονική μηχανή στην περιοχή Azure της τρέχουσας ανάπτυξης ή VNET

Μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή PowerShell για να ελέγξετε τον αριθμό της τρέχουσας πυρήνων που έχετε στη Διαχείριση Azure πόρων. Για να μάθετε περισσότερα σχετικά με τα όρια πυρήνα, ανατρέξτε στο θέμα [όρια και τη διαχείριση πόρων Azure](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager). 

Αυτό το παράδειγμα ελέγχει τη διαθεσιμότητα στην περιοχή **Δυτική ΗΠΑ** . Αντικαταστήστε το όνομα της περιοχής παράδειγμα με το δικό σας. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Βήμα 5: Εκτέλεση εντολές για τη μετεγκατάσταση τους πόρους σας IaaS

>[AZURE.NOTE] Όλες οι λειτουργίες που περιγράφονται εδώ είναι idempotent. Εάν έχετε κάποιο πρόβλημα εκτός μιας μη υποστηριζόμενης δυνατότητας ή ένα σφάλμα παραμέτρων, συνιστάται να επαναλάβετε την προετοιμασία, ματαίωση, ή ολοκλήρωση της λειτουργίας. Η πλατφόρμα προσπαθεί, στη συνέχεια, την ενέργεια ξανά.

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Μετεγκατάσταση εικονικές μηχανές σε μια υπηρεσία cloud (όχι σε ένα εικονικό δίκτυο)

Λήψη της λίστας των υπηρεσιών cloud, χρησιμοποιώντας την ακόλουθη εντολή και, στη συνέχεια, επιλέξτε την υπηρεσία cloud που θέλετε να μετεγκαταστήσετε. Εάν το ΣΠΣ στην υπηρεσία cloud είναι ένα εικονικό δίκτυο ή αν έχει web ή εργαζόμενου ρόλους, η εντολή επιστρέφει ένα μήνυμα σφάλματος.

```powershell
    Get-AzureService | ft Servicename
```

Λήψη του ονόματος ανάπτυξης για την υπηρεσία cloud. Σε αυτό το παράδειγμα, το όνομα της υπηρεσίας είναι **Υπηρεσία μου**. Αντικαταστήστε το όνομα της υπηρεσίας παράδειγμα με το δικό σας όνομα υπηρεσίας. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Προετοιμάστε τις εικονικές μηχανές στην υπηρεσία cloud για μετεγκατάσταση. Έχετε δύο επιλογές για να επιλέξετε.

* **Επιλογή 1. Μετεγκατάσταση του ΣΠΣ σε μια πλατφόρμα δημιουργήθηκε εικονικού δικτύου**

    Πρώτα, επικύρωση εάν μπορείτε να μετεγκαταστήσετε την υπηρεσία cloud, χρησιμοποιώντας τις παρακάτω εντολές:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Η προηγούμενη εντολή εμφανίζει τις προειδοποιήσεις και τα σφάλματα που αποτρέπουν μετεγκατάστασης. Εάν η επικύρωση είναι επιτυχής, στη συνέχεια, μπορείτε να μετακινήσετε σε στο βήμα **Προετοιμασία** :

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```

* **Επιλογή 2. Μετεγκατάσταση σε ένα υπάρχον δίκτυο εικονικού στο μοντέλο ανάπτυξης για τη διαχείριση πόρων**

    Αυτό το παράδειγμα ορίζει το όνομα της ομάδας πόρων σε **myResourceGroup**, το όνομα εικονικού δικτύου για να **myVirtualNetwork** και το όνομα του δευτερεύοντος δικτύου για να **mySubNet**. Αντικαταστήστε τα ονόματα στο παράδειγμα με τα ονόματα των πόρων τη δική σας.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Πρώτα, επικύρωση εάν μπορείτε να μετεγκαταστήσετε την υπηρεσία cloud χρησιμοποιώντας την ακόλουθη εντολή:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Η προηγούμενη εντολή εμφανίζει τις προειδοποιήσεις και τα σφάλματα που αποτρέπουν μετεγκατάστασης. Εάν η επικύρωση είναι επιτυχής, μπορείτε να προχωρήσετε με το ακόλουθο βήμα προετοιμασία:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```
    

Όταν ολοκληρωθεί με επιτυχία η λειτουργία προετοιμασία με οποιαδήποτε από τις παραπάνω επιλογές, ερωτήματος της κατάστασης μετεγκατάστασης του ΣΠΣ. Βεβαιωθείτε ότι είναι σε το `Prepared` κατάσταση.

Αυτό το παράδειγμα ορίζει το όνομα Εικονική **myVM**. Αντικαταστήστε το όνομα παράδειγμα με το δικό σας όνομα Εικονική.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Ελέγξτε τις παραμέτρους για τους πόρους προετοιμασμένο με τη χρήση του PowerShell ή το Azure πύλη. Εάν δεν είστε έτοιμοι για μετεγκατάσταση και θέλετε να επιστρέψετε στην παλιά κατάσταση, χρησιμοποιήστε την ακόλουθη εντολή:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Εάν σας αρέσει το προετοιμασμένο ρύθμισης παραμέτρων, μπορείτε να μετακινηθείτε προς τα εμπρός και ολοκλήρωση τους πόρους, χρησιμοποιώντας την ακόλουθη εντολή:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>Μετεγκατάσταση εικονικές μηχανές σε ένα εικονικό δίκτυο

Για τη μετεγκατάσταση εικονικές μηχανές σε ένα εικονικό δίκτυο, μπορείτε να μετεγκαταστήσετε το δίκτυο. Μετεγκατάσταση αυτόματα τις εικονικές μηχανές με το δίκτυο. Επιλέξτε το εικονικό δίκτυο που θέλετε να μετεγκαταστήσετε. 

Αυτό το παράδειγμα ορίζει το όνομα εικονικού δικτύου **myVnet**. Αντικαταστήστε το όνομα εικονικού δικτύου παράδειγμα με το δικό σας. 

```powershell
    $vnetName = "myVnet"
```

>[AZURE.NOTE] Εάν το εικονικό δίκτυο περιέχει web ή ρόλους εργαζόμενου ή ΣΠΣ με ρυθμίσεις δεν υποστηρίζονται, λαμβάνετε ένα μήνυμα σφάλματος επικύρωσης.

Πρώτα, επικυρώσετε εάν μπορείτε να κάνετε μετεγκατάσταση του εικονικού δικτύου, χρησιμοποιώντας την ακόλουθη εντολή:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Η προηγούμενη εντολή εμφανίζει τις προειδοποιήσεις και τα σφάλματα που αποτρέπουν μετεγκατάστασης. Εάν η επικύρωση είναι επιτυχής, μπορείτε να προχωρήσετε με το ακόλουθο βήμα προετοιμασία:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Ελέγξτε τις παραμέτρους για τις προετοιμασμένο εικονικές μηχανές με τη χρήση του PowerShell Azure ή την πύλη του Azure. Εάν δεν είστε έτοιμοι για μετεγκατάσταση και θέλετε να επιστρέψετε στην παλιά κατάσταση, χρησιμοποιήστε την ακόλουθη εντολή:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Εάν σας αρέσει το προετοιμασμένο ρύθμισης παραμέτρων, μπορείτε να μετακινηθείτε προς τα εμπρός και ολοκλήρωση τους πόρους, χρησιμοποιώντας την ακόλουθη εντολή:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>Μετεγκατάσταση ενός λογαριασμού χώρου αποθήκευσης

Όταν ολοκληρώσετε τη μετεγκατάσταση του εικονικές μηχανές, συνιστάται να κάνετε μετεγκατάσταση τους λογαριασμούς χώρου αποθήκευσης.

Προετοιμασία κάθε λογαριασμό χώρου αποθήκευσης για τη μετεγκατάσταση, χρησιμοποιώντας την ακόλουθη εντολή. Σε αυτό το παράδειγμα, το όνομα λογαριασμού του χώρου αποθήκευσης είναι **myStorageAccount**. Αντικαταστήστε το όνομα παράδειγμα με το όνομα του δικού σας λογαριασμού χώρου αποθήκευσης. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Ελέγξτε τις παραμέτρους για το λογαριασμό προετοιμασμένο χώρου αποθήκευσης με τη χρήση του PowerShell Azure ή την πύλη του Azure. Εάν δεν είστε έτοιμοι για μετεγκατάσταση και θέλετε να επιστρέψετε στην παλιά κατάσταση, χρησιμοποιήστε την ακόλουθη εντολή:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Εάν σας αρέσει το προετοιμασμένο ρύθμισης παραμέτρων, μπορείτε να μετακινηθείτε προς τα εμπρός και ολοκλήρωση τους πόρους, χρησιμοποιώντας την ακόλουθη εντολή:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Επόμενα βήματα

- Για περισσότερες πληροφορίες σχετικά με τη μετεγκατάσταση, ανατρέξτε στο θέμα [υποστηριζόμενες πλατφόρμες μετεγκατάσταση IaaS πόρων από κλασική διαχείριση πόρων για να Azure](virtual-machines-windows-migration-classic-resource-manager.md).
- Για τη μετεγκατάσταση δικτύου επιπλέον πόρους για διαχείριση πόρων με χρήση του PowerShell Azure, χρησιμοποιήστε παρόμοια βήματα με [Μετακίνηση AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx), [Μετακίνηση AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx)και [Μετακίνηση AzureRouteTable](https://msdn.microsoft.com/library/mt786718.aspx).
- Για άνοιγμα προέλευσης δέσμες ενεργειών που μπορείτε να χρησιμοποιήσετε για τη μετεγκατάσταση Azure πόρους από κλασική για τη διαχείριση πόρων, ανατρέξτε στο θέμα [Εργαλεία Κοινότητας για μετεγκατάσταση στο διαχειριστή πόρων Azure μετεγκατάστασης](virtual-machines-windows-migration-scripts.md)
