<properties
    pageTitle="Δημιουργήστε έναν υπολογιστή Windows εικονικού παρακολούθηση και διαγνωστικά χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure | Microsoft Azure"
    description="Χρησιμοποιήστε ένα πρότυπο manager Azure πόρων για να δημιουργήσετε μια νέα εικονική μηχανή Windows με επέκταση Azure Διαγνωστικά."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# <a name="create-a-windows-virtual-machine-with-monitoring-and-diagnostics-using-azure-resource-manager-template"></a>Δημιουργήστε έναν υπολογιστή Windows εικονικού παρακολούθηση και διαγνωστικά χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure

Η επέκταση Διαγνωστικά Azure παρέχει την παρακολούθηση και δυνατότητες Διαγνωστικά του Windows Azure εικονική μηχανή με βάση. Μπορείτε να ενεργοποιήσετε αυτές τις δυνατότητες στην εικονική μηχανή της, συμπεριλαμβάνοντας την επέκταση ως μέρος του προτύπου manager azure πόρων. Για περισσότερες πληροφορίες σχετικά με τη συμπερίληψη κάθε επέκταση ως μέρος ενός προτύπου εικονική μηχανή, ανατρέξτε στο θέμα [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure με Εικονική επεκτάσεις](virtual-machines-windows-extensions-authoring-templates.md) . Σε αυτό το άρθρο περιγράφει πώς μπορείτε να προσθέσετε την επέκταση Διαγνωστικά Azure σε ένα πρότυπο εικονική μηχανή των windows.  
  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>Προσθήκη της επέκτασης Διαγνωστικά Azure τον ορισμό Εικονική πόρων 

Για να ενεργοποιήσετε την επέκταση Διαγνωστικά σε ένα Windows εικονικό μηχάνημα πρέπει να προσθέσετε την επέκταση ως πόρο Εικονική στο πρότυπο της διαχείρισης πόρων.

Για μια απλή διαχείριση πόρων με βάση εικονική μηχανή προσθέστε τη ρύθμιση παραμέτρων επέκταση στον πίνακα, *τους πόρους* για την εικονική μηχανή: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Μια άλλη συνήθη σύμβαση είναι να προσθέσετε τη ρύθμιση παραμέτρων επέκταση στο ριζικό κόμβο πόροι του προτύπου αντί για τον ορισμό του κάτω από την εικονική μηχανή πόρους κόμβο. Με αυτήν την προσέγγιση πρέπει να καθορίσετε μια ιεραρχική σχέση μεταξύ την επέκταση και η εικονική μηχανή ρητά με το *όνομα* και *Τύπος* τιμές. Για παράδειγμα: 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

Η επέκταση πάντα είναι συσχετισμένη με την εικονική μηχανή, μπορείτε να είτε απευθείας Ορισμός απευθείας στην περιοχή η εικονική μηχανή πόρων κόμβου ή ορίστε το επίπεδο βάσης και χρησιμοποιήστε τους ιεραρχική κανόνες ονοματοθεσίας για να συσχετίσετε με την εικονική μηχανή.

Για τα σύνολα κλίμακα εικονική μηχανή τη ρύθμιση παραμέτρων των επεκτάσεων καθορίζεται στην ιδιότητα *extensionProfile* το *VirtualMachineProfile*.
   
Η ιδιότητα *publisher* με την τιμή του **Microsoft.Azure.Diagnostics** και η ιδιότητα *τύπου* με την τιμή του **IaaSDiagnostics** προσδιορίζει την επέκταση Διαγνωστικά Azure.

Η τιμή της ιδιότητας *όνομα* μπορεί να χρησιμοποιηθεί για να ανατρέξετε στις την επέκταση στην ομάδα πόρων. Ρύθμιση συγκεκριμένα **Microsoft.Insights.VMDiagnosticsSettings** παρέχουν τη δυνατότητα να εύκολα προσδιορίζεται από το Azure κλασική πύλης πύλης εξασφαλίζει ότι τα γραφήματα παρακολούθησης εμφανίζονται σωστά στην πύλη του Azure κλασική.

Η *typeHandlerVersion* Καθορίζει την έκδοση της επέκτασης που θέλετε να χρησιμοποιήσετε. Ρύθμιση *autoUpgradeMinorVersion* δευτερεύουσα έκδοση στην **τιμή true** εξασφαλίζει ότι θα λάβετε την πιο πρόσφατη δευτερεύουσα έκδοση της επέκτασης που είναι διαθέσιμη. Συνιστάται ιδιαίτερα να ορίσετε πάντα *autoUpgradeMinorVersion* για να είναι **αληθή** πάντα, έτσι ώστε να έχετε πάντα να χρησιμοποιείτε την πιο πρόσφατη διαθέσιμη Διαγνωστικά επέκταση με όλες τις νέες δυνατότητες και διορθώσεις σφαλμάτων. 

Το στοιχείο *Ρυθμίσεις* περιέχει ρυθμίσεις παραμέτρων ιδιοτήτων για την επέκταση που μπορούν να ορίσετε και να διαβάζετε πίσω από την επέκταση (μερικές φορές αναφέρεται ως δημόσια ρύθμισης παραμέτρων). Η ιδιότητα *xmlcfg* περιέχει xml βάσει ρύθμισης παραμέτρων για τα αρχεία καταγραφής διαγνωστικών, μετρητές επιδόσεων κ.λπ που συλλέγονται από τον παράγοντα Διαγνωστικά. Για περισσότερες πληροφορίες σχετικά με το σχήμα xml ίδια, ανατρέξτε στο θέμα [Διαγνωστικά ρύθμισης παραμέτρων σχήματος](https://msdn.microsoft.com/library/azure/dn782207.aspx) . Μια συνηθισμένη πρακτική είναι για την αποθήκευση στη ρύθμιση παραμέτρων πραγματική xml ως μεταβλητή στο πρότυπο της διαχείρισης πόρων Azure και, στη συνέχεια, concatenate και base64 κωδικοποιήσετε τους για να ορίσετε την τιμή για το *xmlcfg*. Ανατρέξτε στην ενότητα σχετικά με [μεταβλητές Διαγνωστικά ρύθμισης παραμέτρων](#diagnostics-configuration-variables) για να μάθετε περισσότερα σχετικά με τον τρόπο για να αποθηκεύσετε το αρχείο xml σε μεταβλητές. Η ιδιότητα *storageAccount* Καθορίζει το όνομα του λογαριασμού χώρου αποθήκευσης στο οποίο θα μεταφερθούν Διαγνωστικά δεδομένων. 
 
Τις ιδιότητες σε *protectedSettings* (μερικές φορές αναφέρεται ως προσωπική ρύθμιση παραμέτρων) μπορεί να οριστεί, αλλά δεν μπορεί να διαβαστεί ξανά μετά τη ρύθμιση. Ο χαρακτήρας μόνο για εγγραφή των *protectedSettings* καθιστά χρήσιμες για την αποθήκευση απόρρητο όπως το κλειδί λογαριασμού χώρου αποθήκευσης όπου θα εγγραφούν τα διαγνωστικά δεδομένα.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Καθορισμός λογαριασμού χώρου αποθήκευσης Διαγνωστικά ως παραμέτρους 

Το Διαγνωστικά επέκταση json τμήμα κώδικα παραπάνω προϋποθέτει δύο παραμέτρους *existingdiagnosticsStorageAccountName* και *existingdiagnosticsStorageResourceGroup* για να καθορίσετε το λογαριασμό χώρου αποθήκευσης Διαγνωστικά όπου θα αποθηκευτούν τα Διαγνωστικά δεδομένων. Που καθορίζει το λογαριασμό χώρου αποθήκευσης Διαγνωστικά ως παράμετρο καθιστά εύκολη για να αλλάξετε το λογαριασμό χώρου αποθήκευσης Διαγνωστικά σε διαφορετικά περιβάλλοντα π.χ. ενδέχεται να θέλετε να χρησιμοποιήσετε ένα λογαριασμό διαφορετικό Διαγνωστικά χώρου αποθήκευσης για τον έλεγχο και ένα διαφορετικό για την ανάπτυξη παραγωγής.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

Είναι βέλτιστη πρακτική για να καθορίσετε ένα λογαριασμό Διαγνωστικά του χώρου αποθήκευσης σε μια ομάδα πόρων διαφορετική από την ομάδα των πόρων για την εικονική μηχανή. Μια ομάδα πόρων μπορεί να θεωρείται μια μονάδα ανάπτυξη με το δικό της ζωής του, ένα εικονικό μηχάνημα μπορεί να αναπτυχθεί και να επαναληφθεί η ανάπτυξη καθώς πραγματοποιούνται από νέες ενημερώσεις ρυθμίσεις παραμέτρων σε αυτήν, αλλά μπορεί να θέλετε να συνεχίσετε την αποθήκευση των δεδομένων Διαγνωστικά στον ίδιο λογαριασμό χώρου αποθήκευσης σε αυτές τις αναπτύξεις εικονική μηχανή. Αντιμετωπίζετε το λογαριασμό χώρου αποθήκευσης σε έναν άλλο πόρο ενεργοποιεί το λογαριασμό χώρου αποθήκευσης, ώστε να αποδέχεται δεδομένα από διάφορες εικονική μηχανή αναπτύξεις, διευκολύνοντας την αντιμετώπιση προβλημάτων σε όλες τις διάφορες εκδόσεις.

>[AZURE.NOTE] Εάν δημιουργείτε ένα πρότυπο εικονική μηχανή των windows από το Visual Studio τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης ενδέχεται να έχει ρυθμιστεί για να χρησιμοποιήσετε τον ίδιο λογαριασμό χώρου αποθήκευσης όπου έχει αποσταλεί η εικονική μηχανή VHD. Πρόκειται για να απλοποιήσετε αρχική ρύθμιση του η Εικονική. Που θα πρέπει να συντελεστής εκ νέου το πρότυπο για να χρησιμοποιήσετε ένα λογαριασμό διαφορετικό χώρο αποθήκευσης που μπορούν να περάσουν στο ως παράμετρο. 

## <a name="diagnostics-configuration-variables"></a>Διαγνωστικά μεταβλητές ρύθμισης παραμέτρων
 
Το Διαγνωστικά επέκταση json τμήμα κώδικα παραπάνω ορίζει έναν μεταβλητή *accountid* για να απλοποιήσετε γρήγορα το κλειδί λογαριασμού χώρου αποθήκευσης για την αποθήκευση διαγνωστικών:   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


Η ιδιότητα *xmlcfg* για την επέκταση Διαγνωστικά έχει οριστεί με πολλές μεταβλητές που ενοποιούνται μαζί. Οι τιμές αυτών των μεταβλητών είναι σε xml, ώστε να πρέπει να είναι διαφυγής σωστά κατά τη ρύθμιση των μεταβλητών json.

Παρακάτω περιγράφονται Διαγνωστικά ρύθμισης παραμέτρων xml που συλλέγει μετρητές επιπέδου επιδόσεων τυπική συστήματος μαζί με ορισμένα αρχεία καταγραφής συμβάντων των windows και αρχεία καταγραφής διαγνωστικών υποδομής. Έχει διαφυγής και μορφοποιηθεί σωστά, έτσι ώστε η ρύθμιση παραμέτρων μπορεί να επικολληθεί απευθείας στην ενότητα μεταβλητές του προτύπου σας. Ανατρέξτε στο θέμα το [Σχήμα της ομάδας παραμέτρων Διαγνωστικά](https://msdn.microsoft.com/library/azure/dn782207.aspx) για μια πιο human αναγνώσιμο παράδειγμα του xml ρύθμισης παραμέτρων.
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Ο κόμβος μετρικά ορισμού xml στο τις παραπάνω παραμέτρους είναι ένα στοιχείο σημαντικές ρύθμισης παραμέτρων όπως ορίζει πώς θα συναθροιστεί και θα αποθηκευτούν μετρητών επιδόσεων που ορίζονται από προηγούμενη έκδοση στην xml στον κόμβο *PerformanceCounter* . 

> [AZURE.IMPORTANT] Αυτές οι μετρήσεις μονάδα δίσκου τα γραφήματα παρακολούθησης και τις ειδοποιήσεις στην πύλη του Azure.  Ο κόμβος **μετρικά** με την *resourceID* και **MetricAggregation** πρέπει να περιλαμβάνονται στη ρύθμιση παραμέτρων διαγνωστικών για Εικονική σας Εάν θέλετε να δείτε τα δεδομένα παρακολούθησης Εικονική στην πύλη του Azure. 

Ακολουθεί ένα παράδειγμα του xml για ορισμούς μετρικά: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Το χαρακτηριστικό *resourceID* προσδιορίζει με μοναδικό τρόπο η εικονική μηχανή στη συνδρομή σας. Φροντίστε να χρησιμοποιήσετε τις συναρτήσεις subscription() και resourceGroup(), έτσι ώστε το πρότυπο ενημερώνει αυτόματα τις τιμές με βάση τη συνδρομή και αναπτύσσετε σε ομάδα πόρων.

Εάν θέλετε να δημιουργήσετε πολλές εικονικές μηχανές σε επανάληψη, στη συνέχεια, θα πρέπει να συμπλήρωση της τιμής *resourceID* με μια συνάρτηση copyIndex() για να διαφοροποιήσετε σωστά κάθε Εικονική μεμονωμένα. Η τιμή *xmlCfg* μπορούν να ενημερωθούν ώστε να υποστηρίζει αυτή ως εξής:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

Η τιμή MetricAggregation *PT1H* και *PT1M* εκφράζει την έγκρισή μια συγκέντρωση πάνω από ένα λεπτό και μια συγκέντρωση πάνω από κάθε ώρα.

## <a name="wadmetrics-tables-in-storage"></a>WADMetrics πίνακες στο χώρο αποθήκευσης

Η ρύθμιση παραμέτρων μετρικά παραπάνω θα δημιουργήσει πίνακες στο λογαριασμό σας Διαγνωστικά του χώρου αποθήκευσης με τις ακόλουθες συνθήκες ονοματοθεσίας:

- **WADMetrics** : τυπικό πρόθεμα για όλους τους πίνακες WADMetrics
- **PT1H** ή **PT1M** : υποδεικνύει ότι ο πίνακας περιέχει συγκέντρωση δεδομένων πάνω από 1 ώρα ή 1 λεπτό
- **P10D** : υποδεικνύει τον πίνακα θα περιέχει τα δεδομένα για 10 ημερών από τον πίνακα ξεκίνησαν συλλογής δεδομένων
- **V2S** : σταθερά συμβολοσειράς
- **ΕΕΕΕΜΜΗΗ** : την ημερομηνία κατά την οποία ο πίνακας αποτελέσματα συλλογής δεδομένων

Παράδειγμα: *WADMetricsPT1HP10DV2S20151108* θα περιέχει τα δεδομένα μετρικά συναθροιστεί περισσότερο από μία ώρα για 10 ημερών ξεκινούν 11-Νοεμβρίου-2015    

Κάθε πίνακας WADMetrics θα περιέχει τις ακόλουθες στήλες:

- **PartitionKey**: το partitionkey έχει συνταχθεί με βάση την τιμή *resourceID* για να προσδιορίσει μοναδικά τον πόρο Εικονική. για π.χ.: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : ακολουθεί τη μορφή <Descending time tick>:<Performance Counter Name>. Φθίνουσα υπολογισμού υποδιαίρεσης ώρας είναι υποδιαιρέσεις μέγιστο χρόνο μείον την ώρα από την αρχή της περιόδου συγκέντρωσης. Π.χ. Αν η περίοδος δείγματος αποτελέσματα σε 10-Νοεμβρίου-2015 και θα ήταν 00:00Hrs UTC, στη συνέχεια, τον υπολογισμό: DateTime.MaxValue.Ticks - (νέα DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Υποδιαιρέσεις). Για τη μνήμη διαθέσιμα byte επιδόσεων μετρητή τον αριθμό-κλειδί γραμμή θα είναι όπως: 2519551871999999999__:005CMemory:005CAvailable:0020 byte που
- **Το Όνομα_μετρητή** : είναι το όνομα του μετρητή επιδόσεων. Αυτή η αναζήτηση εντοπίζει *counterSpecifier* ορίζεται στο αρχείο config xml.
- **Μέγιστο** : τη μέγιστη τιμή του μετρητή απόδοσης κατά την περίοδο συγκέντρωσης.
- **Ελάχιστο** : Η ελάχιστη τιμή του μετρητή απόδοσης κατά την περίοδο συγκέντρωσης.
- **Σύνολο** : αναφέρεται το άθροισμα όλων των τιμών του μετρητή απόδοσης κατά την περίοδο συγκέντρωσης.
- **Καταμέτρηση** : Ο συνολικός αριθμός των τιμών που αναφέρθηκε για το μετρητή επιδόσεων.
- **Μέσος όρος** : Η τιμή μέσο όρο (Συνολική καταμέτρηση) του μετρητή απόδοσης κατά την περίοδο συγκέντρωσης.


## <a name="next-steps"></a>Επόμενα βήματα

- Για ένα πρότυπο ολοκληρωμένο δείγμα των Windows εικονικό μηχάνημα με επέκταση Διαγνωστικά δείτε [201-εικονική-παρακολούθηση-Διαγνωστικά-επέκταση](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- Αναπτύξτε το πρότυπο διαχείρισης πόρων με τη χρήση [Του PowerShell Azure](virtual-machines-windows-ps-manage.md) ή [γραμμή εντολών Azure](virtual-machines-linux-cli-deploy-templates.md)
- Μάθετε περισσότερα σχετικά με τα [πρότυπα σύνταξης από διαχειριστή πόρων Azure](../resource-group-authoring-templates.md)







