<properties
    pageTitle="Χρήση του PowerShell για να ενεργοποιήσετε την Διαγνωστικά Azure σε έναν εικονικό υπολογιστή με Windows | Microsoft Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    description="Μάθετε τον τρόπο χρήσης του PowerShell για να ενεργοποιήσετε την Διαγνωστικά Azure σε έναν εικονικό υπολογιστή με Windows"
    authors="sbtron"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>


# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Χρήση του PowerShell για να ενεργοποιήσετε την Διαγνωστικά Azure σε έναν εικονικό υπολογιστή με Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Διαγνωστικά του Azure είναι η δυνατότητα μέσα σε Azure που επιτρέπει τη συλλογή διαγνωστικών δεδομένων σε μια εφαρμογή ανεπτυγμένος. Μπορείτε να χρησιμοποιήσετε την επέκταση Διαγνωστικά για τη συλλογή διαγνωστικών δεδομένων όπως αρχεία καταγραφής εφαρμογών ή μετρητές επιδόσεων από μια Azure εικονική μηχανή (Εικονική) που εκτελεί Windows. Σε αυτό το άρθρο περιγράφει τον τρόπο χρήσης του Windows PowerShell για να ενεργοποιήσετε την επέκταση Διαγνωστικά για μια Εικονική. Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για τις προϋποθέσεις που χρειάζονται για αυτό το άρθρο.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>Ενεργοποιήστε την επέκταση Διαγνωστικά Εάν χρησιμοποιείτε το μοντέλο ανάπτυξης για τη διαχείριση πόρων

Μπορείτε να ενεργοποιήσετε την επέκταση Διαγνωστικά, ενώ μπορείτε να δημιουργήσετε μια Εικονική των Windows μέσω του μοντέλου ανάπτυξης διαχείρισης πόρων Azure, προσθέτοντας τις παραμέτρους επέκταση στο πρότυπο διαχείρισης πόρων. Ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή Windows παρακολούθηση και διαγνωστικά χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure](virtual-machines-windows-extensions-diagnostics-template.md).

Για να ενεργοποιήσετε την επέκταση Διαγνωστικά σε μια υπάρχουσα Εικονική που δημιουργήθηκε μέσω του μοντέλου ανάπτυξης για τη διαχείριση πόρων, μπορείτε να χρησιμοποιήσετε το cmdlet [Set-AzureRMVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt603499.aspx) PowerShell, όπως φαίνεται παρακάτω.


    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* είναι η διαδρομή προς το αρχείο που περιέχει τη ρύθμιση παραμέτρων Διαγνωστικά σε XML, όπως περιγράφεται στο [δείγμα](#sample-diagnostics-configuration) παρακάτω.  

Εάν το αρχείο ρύθμισης παραμέτρων Διαγνωστικά καθορίζει ένα στοιχείο **StorageAccount** με ένα όνομα λογαριασμού χώρου αποθήκευσης, η δέσμη ενεργειών *AzureRMVMDiagnosticsExtension σύνολο* θα ρυθμίσει αυτόματα την επέκταση διαγνωστικών για την αποστολή διαγνωστικών δεδομένων σε αυτόν το λογαριασμό χώρου αποθήκευσης. Για να γίνει αυτό, το λογαριασμό χώρου αποθήκευσης πρέπει να είναι στην ίδια συνδρομή του ως την εικονική Μηχανή.

Εάν δεν υπάρχει **StorageAccount** έχει καθοριστεί στη ρύθμιση παραμέτρων Διαγνωστικά, στη συνέχεια, πρέπει να μεταβιβάζουν στην παράμετρο *StorageAccountName* για το cmdlet. Εάν έχει καθοριστεί η παράμετρος *StorageAccountName* , στη συνέχεια, το cmdlet θα χρησιμοποιείτε πάντα το λογαριασμό χώρου αποθήκευσης που καθορίζεται στην παράμετρο και όχι τον που καθορίζεται στο αρχείο παραμέτρων Διαγνωστικά.

Εάν τα Διαγνωστικά του χώρου αποθήκευσης είναι σε μια διαφορετική συνδρομή από την εικονική Μηχανή, στη συνέχεια, πρέπει να μεταβιβάζουν ρητά στις παραμέτρους *StorageAccountName* και *StorageAccountKey* το cmdlet. Η παράμετρος *StorageAccountKey* δεν είναι απαραίτητος κατά το λογαριασμό χώρου αποθήκευσης Διαγνωστικά είναι στην ίδια συνδρομή του, όπως το cmdlet αυτόματα να ερωτήματος και να ορίσετε την τιμή του κλειδιού κατά την ενεργοποίηση του την επέκταση Διαγνωστικά. Ωστόσο, εάν το λογαριασμό χώρου αποθήκευσης Διαγνωστικά βρίσκεται σε μια διαφορετική συνδρομή, στη συνέχεια, το cmdlet ίσως να μην μπορείτε να πραγματοποιήσετε αυτόματη λήψη τον αριθμό-κλειδί και πρέπει να καθορίσετε τον αριθμό-κλειδί μέσω της παραμέτρου *StorageAccountKey* ρητά.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Όταν είναι ενεργοποιημένη η επέκταση Διαγνωστικά σε μια Εικονική, μπορείτε να λάβετε τις τρέχουσες ρυθμίσεις, χρησιμοποιώντας το cmdlet [Get-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603678.aspx) .

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Το cmdlet επιστρέφει *PublicSettings*, που περιέχει τη ρύθμιση παραμέτρων XML σε μορφή κωδικοποίηση Base64. Για να διαβάσετε το XML, πρέπει να αποκωδικοποιείτε το.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

Το cmdlet [Κατάργηση AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603782.aspx) μπορεί να χρησιμοποιηθεί για να καταργήσετε την επέκταση Διαγνωστικά από την εικονική Μηχανή.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>Ενεργοποιήστε την επέκταση Διαγνωστικά Εάν χρησιμοποιείτε το μοντέλο κλασική ανάπτυξης

Μπορείτε να χρησιμοποιήσετε το cmdlet [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) για να ενεργοποιήσετε μια επέκταση Διαγνωστικά σε μια Εικονική που δημιουργείτε μέσω του μοντέλου κλασική ανάπτυξης. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια νέα Εικονική μέσω του μοντέλου κλασική ανάπτυξης με την επέκταση Διαγνωστικά με δυνατότητα.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Για να ενεργοποιήσετε την επέκταση Διαγνωστικά σε μια υπάρχουσα Εικονική που δημιουργήθηκε μέσω του μοντέλου κλασική ανάπτυξης, χρησιμοποιήστε πρώτα το cmdlet [Get-AzureVM](https://msdn.microsoft.com/library/mt589152.aspx) για να λάβετε τη ρύθμιση παραμέτρων Εικονική. Στη συνέχεια, ενημερώστε τις ρυθμίσεις παραμέτρων Εικονική για να συμπεριλάβετε την επέκταση Διαγνωστικά χρησιμοποιώντας το cmdlet [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) . Τέλος, εφαρμογή τις ενημερωμένες παραμέτρους την εικονική Μηχανή χρησιμοποιώντας [AzureVM ενημερωμένη έκδοση](https://msdn.microsoft.com/library/mt589121.aspx).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Δείγμα Διαγνωστικά ρύθμισης παραμέτρων

Το παρακάτω δείγμα XML μπορεί να χρησιμοποιηθεί για τη ρύθμιση παραμέτρων Διαγνωστικά δημόσια με τα παραπάνω δεσμών ενεργειών. Αυτή η ρύθμιση παραμέτρων δείγμα θα μεταφορά διάφορες μετρητές επιδόσεων για το λογαριασμό χώρου αποθήκευσης Διαγνωστικά, μαζί με σφάλματα από την εφαρμογή, ασφάλεια και κανάλια συστήματος στα αρχεία καταγραφής συμβάντων των Windows και τυχόν σφάλματα από τα αρχεία καταγραφής διαγνωστικών υποδομής.

Η ρύθμιση παραμέτρων πρέπει να ενημερωθούν ώστε να περιλαμβάνουν τα εξής:

- Το χαρακτηριστικό *resourceID* του στοιχείου **μετρικά** πρέπει να ενημερωθεί με το Αναγνωριστικό πόρου για την εικονική Μηχανή.
    - Το Αναγνωριστικό πόρου είναι δυνατό να δημιουργηθεί με τη χρήση του παρακάτω μοτίβο: "/resourceGroups//συνδρομές / {*Αναγνωριστικό συνδρομής για τη συνδρομή με την εικονική Μηχανή*} {*το όνομα ομάδα πόρων για την εικονική Μηχανή*} / {*το όνομα Εικονική*} providers/Microsoft.Compute/virtualMachines/".
    - Για παράδειγμα, εάν το Αναγνωριστικό συνδρομής για τη συνδρομή όπου εκτελείται η Εικονική είναι **11111111-1111-1111-1111-111111111111**, το όνομα της ομάδας πόρων για την ομάδα πόρων είναι **MyResourceGroup**και το όνομα Εικονική είναι **MyWindowsVM**, στη συνέχεια, η τιμή για *resourceID* θα είναι:

        ```
        <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
        ```

    - Για περισσότερες πληροφορίες σχετικά με τον τρόπο μετρικά δημιουργούνται με βάση τη ρύθμιση παραμέτρων μετρητές και μετρήσεις επιδόσεων, ανατρέξτε στο θέμα [Διαγνωστικά Azure μετρικά πίνακα στο χώρο αποθήκευσης](virtual-machines-windows-extensions-diagnostics-template.md#wadmetrics-tables-in-storage).

- Το στοιχείο **StorageAccount** πρέπει να ενημερωθεί με το όνομα του λογαριασμού Διαγνωστικά του χώρου αποθήκευσης.

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Επόμενα βήματα
- Για πρόσθετες οδηγίες σχετικά με τη χρήση της δυνατότητας Διαγνωστικά Azure και άλλες τεχνικές για την αντιμετώπιση προβλημάτων, ανατρέξτε στο θέμα [Ενεργοποίηση Διαγνωστικά στις υπηρεσίες Cloud Azure και εικονικές μηχανές](../cloud-services/cloud-services-dotnet-diagnostics.md).
- [Σχήμα ρυθμίσεις παραμέτρων Διαγνωστικά](https://msdn.microsoft.com/library/azure/mt634524.aspx) εξηγεί τις διάφορες επιλογές ρυθμίσεις παραμέτρων XML για την επέκταση Διαγνωστικά.
