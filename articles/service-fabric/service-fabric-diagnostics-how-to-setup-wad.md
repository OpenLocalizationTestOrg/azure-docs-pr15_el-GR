<properties
   pageTitle="Συλλογή αρχείων καταγραφής, χρησιμοποιώντας τα Διαγνωστικά Azure | Microsoft Azure"
   description="Σε αυτό το άρθρο περιγράφει πώς μπορείτε να ρυθμίσετε Διαγνωστικά του Azure για τη συλλογή αρχείων καταγραφής από ένα σύμπλεγμα ύφασμα υπηρεσία εκτελείται στο Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="ms-toddabel"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="toddabel"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Συλλογή αρχείων καταγραφής, χρησιμοποιώντας τα Διαγνωστικά Azure

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Όταν χρησιμοποιείτε ένα σύμπλεγμα Azure Service ύφασμα, είναι καλή ιδέα να συγκεντρώσετε τα αρχεία καταγραφής από όλους τους κόμβους σε μια κεντρική θέση. Αντιμετωπίζετε τα αρχεία καταγραφής σε μια κεντρική θέση σάς βοηθά να ανάλυση και αντιμετώπιση προβλημάτων στο το σύμπλεγμά σας ή θέματα στο τις εφαρμογές και υπηρεσίες που εκτελούνται σε αυτό το σύμπλεγμα.

Ένας τρόπος για την αποστολή και συλλογή αρχείων καταγραφής είναι να χρησιμοποιήσετε την επέκταση Διαγνωστικά Azure, η οποία κάνει αποστολή των αρχείων καταγραφής στην αποθήκευσης Azure. Τα αρχεία καταγραφής δεν είναι αυτό χρήσιμες απευθείας στο χώρο αποθήκευσης. Αλλά μπορείτε να χρησιμοποιήσετε μια εξωτερική διαδικασία για να διαβάσετε τα συμβάντα από χώρο αποθήκευσης και να τα τοποθετήσετε στο προϊόν όπως [Ανάλυση καταγραφής](../log-analytics/log-analytics-service-fabric.md), [Ελαστικά αναζήτησης](service-fabric-diagnostic-how-to-use-elasticsearch.md)ή μια άλλη λύση ανάλυση καταγραφής.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Θα χρησιμοποιήσετε αυτά τα εργαλεία για να εκτελέσετε ορισμένες από τις εργασίες σε αυτό το έγγραφο:

* [Διαγνωστικά του Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) (που σχετίζονται με τις υπηρεσίες Cloud Azure αλλά έχει καλές πληροφορίες και παραδείγματα)
* [Azure από διαχειριστή πόρων](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](../powershell-install-configure.md)
* [Azure προγράμματος-πελάτη για τη διαχείριση πόρων](https://github.com/projectkudu/ARMClient)
* [Azure προτύπου για τη διαχείριση πόρων](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)


## <a name="log-sources-that-you-might-want-to-collect"></a>Αρχείο καταγραφής προελεύσεις που μπορεί να θέλετε να συλλέξετε
- **Αρχεία καταγραφής από την υπηρεσία ύφασμα**: που εκπέμπει από την πλατφόρμα τυπική συμβάντων ανίχνευσης για Windows (ETW) και Προέλευση_συμβάντος κανάλι. Αρχεία καταγραφής μπορεί να είναι ένας από τους τύπους:
  - Λειτουργικές συμβάντα: αρχεία καταγραφής για τις εργασίες που εκτελεί την πλατφόρμα ύφασμα υπηρεσίας. Παραδείγματα η δημιουργία εφαρμογές και υπηρεσίες, αλλαγές της κατάστασης κόμβο και πληροφορίες αναβάθμισης.
  - [Αξιόπιστη παραγόντων προγραμματισμού συμβάντων μοντέλο](service-fabric-reliable-actors-diagnostics.md)
  - [Αξιόπιστη υπηρεσίες προγραμματισμού συμβάντων μοντέλο](service-fabric-reliable-services-diagnostics.md)
- **Εφαρμογή συμβάντα**: συμβάντα που εκπέμπει από την υπηρεσία κώδικα και η εγγραφή, χρησιμοποιώντας την κλάση βοηθητικού προγράμματος Προέλευση_συμβάντος που παρέχεται στην τα πρότυπα του Visual Studio. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να γράψετε αρχεία καταγραφής από την εφαρμογή σας, ανατρέξτε στο θέμα [οθόνη και διάγνωση υπηρεσίες σε μια ρύθμιση ανάπτυξης τοπικό υπολογιστή](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).


## <a name="deploy-the-diagnostics-extension"></a>Αναπτύξτε την επέκταση Διαγνωστικά
Είναι το πρώτο βήμα για τη συλλογή αρχείων καταγραφής για να αναπτύξετε την επέκταση Διαγνωστικά σε κάθε του ΣΠΣ στο σύμπλεγμα ύφασμα υπηρεσίας. Η επέκταση Διαγνωστικά συλλέγει αρχεία καταγραφής σε κάθε Εικονική και αποστέλλει τους με το λογαριασμό χώρου αποθήκευσης που καθορίζετε. Τα βήματα διαφέρουν ανάλογα με λίγο Εάν χρησιμοποιείτε το Azure πύλη ή διαχείριση πόρων Azure. Τα βήματα που περιγράφονται επίσης ποικίλλουν ανάλογα με αν ανάπτυξης είναι μέρος της δημιουργίας σύμπλεγμα ή είναι για ένα σύμπλεγμα που υπάρχει ήδη. Ας εξετάσουμε τα βήματα για κάθε σενάριο.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>Αναπτύξτε την επέκταση Διαγνωστικά ως μέρος της δημιουργίας σύμπλεγμα μέσω της πύλης
Για να αναπτύξετε την επέκταση Διαγνωστικά του ΣΠΣ στο σύμπλεγμα ως μέρος της δημιουργίας σύμπλεγμα, μπορείτε να χρησιμοποιήσετε τον πίνακα ρυθμίσεις Διαγνωστικά φαίνεται στην παρακάτω εικόνα. Για να ενεργοποιήσετε την αξιόπιστη ηθοποιών ή αξιόπιστων υπηρεσιών συλλογής συμβάντων, βεβαιωθείτε ότι τα Διαγνωστικά έχει οριστεί **σε** (η προεπιλεγμένη ρύθμιση). Αφού δημιουργήσετε το σύμπλεγμα, δεν μπορείτε να αλλάξετε αυτές τις ρυθμίσεις χρησιμοποιώντας την πύλη.

![Azure ρυθμίσεων των διαγνωστικών του στην πύλη για τη δημιουργία συμπλέγματος](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Η υποστήριξη Azure ομάδας *απαιτεί* υποστήριξη τα αρχεία καταγραφής για να επιλύσετε οποιαδήποτε αιτήσεις υποστήριξης που δημιουργείτε. Αυτά τα αρχεία καταγραφής είναι που έχουν συλλεχθεί σε πραγματικό χρόνο και είναι αποθηκευμένα σε έναν από τους λογαριασμούς χώρου αποθήκευσης που έχουν δημιουργηθεί με την ομάδα των πόρων. Οι ρυθμίσεις Διαγνωστικά ρύθμιση παραμέτρων επιπέδου εφαρμογής συμβάντων. Αυτά τα συμβάντα περιλαμβάνουν [Αξιόπιστη ηθοποιών](service-fabric-reliable-actors-diagnostics.md) συμβάντα, συμβάντα [Αξιόπιστων υπηρεσιών](service-fabric-reliable-services-diagnostics.md) και ορισμένα συμβάντα ύφασμα υπηρεσίας επιπέδου συστήματος να είναι αποθηκευμένο στο χώρο αποθήκευσης Azure.

Προϊόντα, όπως [Ελαστικά αναζήτησης](service-fabric-diagnostic-how-to-use-elasticsearch.md) ή από τη δική σας διεργασία να λάβετε τα συμβάντα από το λογαριασμό χώρου αποθήκευσης. Δεν υπάρχει αυτήν τη στιγμή τρόπος για να φιλτράρετε ή να groom τα συμβάντα που αποστέλλονται στον πίνακα. Εάν δεν μπορείτε να υλοποιήσετε μια διαδικασία για να καταργήσετε τα συμβάντα από τον πίνακα, ο πίνακας θα συνεχίσει να αυξάνεται.

Όταν θέλετε να δημιουργήσετε ένα σύμπλεγμα, χρησιμοποιώντας την πύλη, συνιστάται να κάνετε λήψη του προτύπου *πριν κάνετε κλικ στο κουμπί * *OK*** για να δημιουργήσετε το σύμπλεγμα. Για λεπτομέρειες, ανατρέξτε για να [ρυθμίσετε ένα σύμπλεγμα ύφασμα υπηρεσία, χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure](service-fabric-cluster-creation-via-arm.md). Θα χρειαστεί το πρότυπο για να κάνετε αλλαγές αργότερα, επειδή δεν μπορείτε να κάνετε ορισμένες αλλαγές, χρησιμοποιώντας την πύλη.

Μπορείτε να εξαγάγετε τα πρότυπα από την πύλη, χρησιμοποιώντας τα ακόλουθα βήματα. Ωστόσο, αυτά τα πρότυπα μπορεί να είναι πιο δύσκολο να χρησιμοποιήσετε, επειδή μπορεί να έχουν τιμές null που λείπουν από τις απαιτούμενες πληροφορίες.

1. Ανοίξτε την ομάδα πόρων.
2. Επιλέξτε **Ρυθμίσεις** για να εμφανίσετε τον πίνακα "Ρυθμίσεις".
3. Επιλέξτε **αναπτύξεις** για να εμφανιστεί ο πίνακας ιστορικού ανάπτυξης.
4. Επιλέξτε μια ανάπτυξη του για να εμφανίσετε τις λεπτομέρειες της ανάπτυξης.
5. Επιλέξτε **Εξαγωγή προτύπου** για να εμφανίσετε το πρότυπο πίνακα.
6. Επιλέξτε **Αποθήκευση στο αρχείο** για να εξαγάγετε ένα αρχείο που περιέχει το πρότυπο, η παράμετρος και αρχεία του PowerShell.

Μετά την εξαγωγή των αρχείων, πρέπει να κάνετε μια τροποποίηση. Επεξεργαστείτε το αρχείο parameters.json και καταργήστε το στοιχείο **adminPassword** . Αυτό θα προκαλέσει προτροπή για τον κωδικό πρόσβασης όταν εκτελείται η δέσμη ενεργειών ανάπτυξης. Όταν χρησιμοποιείτε τη δέσμη ενεργειών ανάπτυξης, ίσως χρειαστεί να διορθώσετε τιμές null των παραμέτρων.

Για να χρησιμοποιήσετε το πρότυπο που έχετε λάβει για να ενημερώσετε μια ρύθμιση παραμέτρων:

1. Εξαγάγετε τα περιεχόμενα σε ένα φάκελο στον τοπικό σας υπολογιστή.
2. Τροποποίηση των περιεχομένων για να απεικονίσει τη νέα ρύθμιση παραμέτρων.
3. Ξεκινήστε το PowerShell και μεταβείτε στο φάκελο όπου έχετε κάνει εξαγωγή του περιεχομένου.
4. Εκτέλεση **deploy.ps1** και συμπληρώστε το Αναγνωριστικό συνδρομής, το όνομα της ομάδας πόρων (Χρησιμοποιήστε το ίδιο όνομα για να ενημερώσετε τις ρυθμίσεις παραμέτρων) και ένα όνομα μοναδικό ανάπτυξης.


### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Ανάπτυξη την επέκταση Διαγνωστικά ως μέρος της δημιουργίας σύμπλεγμα χρησιμοποιώντας τη διαχείριση πόρων Azure
Για να δημιουργήσετε ένα σύμπλεγμα χρησιμοποιώντας τη διαχείριση πόρων, πρέπει να προσθέσετε τη ρύθμιση παραμέτρων Διαγνωστικά JSON στο πρότυπο διαχείρισης πόρων πλήρες σύμπλεγμα πριν να δημιουργήσετε το σύμπλεγμα. Παρέχουμε ένα πρότυπο δείγμα πέντε Εικονική σύμπλεγμα διαχείριση πόρων με τη ρύθμιση παραμέτρων Διαγνωστικά προστεθεί ως μέρος των μας δειγμάτων προτύπου για τη διαχείριση πόρων. Μπορείτε να το δείτε σε αυτήν τη θέση στη συλλογή Azure δείγματα: [σύμπλεγμα πέντε κόμβο με το δείγμα προτύπου για τη διαχείριση πόρων Διαγνωστικά](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Για να δείτε τη ρύθμιση Διαγνωστικά του προτύπου για τη διαχείριση πόρων, ανοίξτε το αρχείο azuredeploy.json και αναζητήστε **IaaSDiagnostics**. Για να δημιουργήσετε ένα σύμπλεγμα χρησιμοποιώντας αυτό το πρότυπο, επιλέξτε το κουμπί **Ανάπτυξη να Azure** διαθέσιμο στην προηγούμενη σύνδεση.

Εναλλακτικά, μπορείτε να κάντε λήψη του δείγματος διαχείριση πόρων, κάνετε αλλαγές σε αυτό και δημιουργήστε ένα σύμπλεγμα με το τροποποιημένο πρότυπο, χρησιμοποιώντας το `New-AzureRmResourceGroupDeployment` εντολής σε ένα παράθυρο του Azure PowerShell. Δείτε τον παρακάτω κώδικα για τις παραμέτρους που έχετε στείλει σε την εντολή. Για λεπτομερείς πληροφορίες σχετικά με τον τρόπο για να αναπτύξετε μια ομάδα πόρων με χρήση του PowerShell, ανατρέξτε στο άρθρο [ανάπτυξη μιας ομάδας πόρων με το πρότυπο διαχείρισης πόρων Azure](../resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>Ανάπτυξη την επέκταση Διαγνωστικά σε ένα υπάρχον σύμπλεγμα
Εάν έχετε ένα υπάρχον σύμπλεγμα που δεν έχει αναπτυχθεί Διαγνωστικά ή εάν θέλετε να τροποποιήσετε μια υπάρχουσα ρύθμιση παραμέτρων, μπορείτε να προσθέσετε ή να ενημερώσετε. Τροποποίηση του προτύπου για τη διαχείριση πόρων που χρησιμοποιείται για να δημιουργήσετε το υπάρχον σύμπλεγμα ή να κάνετε λήψη του προτύπου από την πύλη, όπως περιγράφεται παραπάνω. Τροποποιήστε το αρχείο template.json, ακολουθώντας τις παρακάτω εργασίες.

Προσθέστε ένα νέο πόρο χώρου αποθήκευσης στο πρότυπο, προσθέτοντας στην ενότητα πόρους.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Στη συνέχεια, προσθέσετε στην ενότητα παράμετροι απλώς μετά των ορισμών λογαριασμού χώρου αποθήκευσης, μεταξύ `supportLogStorageAccountName` και `vmNodeType0Name`. Αντικατάσταση κειμένου κράτησης θέσης *αποθήκευσης ονόματος λογαριασμού* με το όνομα του λογαριασμού χώρου αποθήκευσης.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Στη συνέχεια, να ενημερώσετε το `VirtualMachineProfile` ενότητα του αρχείου template.json, προσθέτοντας τον παρακάτω κώδικα μέσα σε έναν πίνακα επεκτάσεις. Φροντίστε να προσθέσετε ένα ερωτηματικό στην αρχή ή Τέλος, ανάλογα με το σημείο όπου έχει εισαχθεί.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Μετά την τροποποίηση του αρχείου template.json όπως περιγράφεται, δημοσιεύστε ξανά το πρότυπο διαχείρισης πόρων. Εάν το πρότυπο έχει εξαχθεί, εκτελώντας το αρχείο deploy.ps1 αναδημοσιεύσει το πρότυπο. Μετά την ανάπτυξη, βεβαιωθείτε ότι **ProvisioningState** είναι **ολοκληρώθηκε με επιτυχία**.


## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Ενημέρωση Διαγνωστικά για να συλλέξετε και να αποστείλετε αρχεία καταγραφής από νέα κανάλια Προέλευση_συμβάντος
Για να ενημερώσετε τα Διαγνωστικά για τη συλλογή αρχείων καταγραφής από νέα κανάλια Προέλευση_συμβάντος που αντιπροσωπεύει μια νέα εφαρμογή που πρόκειται να αναπτύξετε, ακολουθήστε τα ίδια βήματα όπως στην [προηγούμενη ενότητα](#deploywadarm) για τη ρύθμιση των διαγνωστικών του για ένα υπάρχον σύμπλεγμα.

Ενημέρωση του `EtwEventSourceProviderConfiguration` ενότητα στο αρχείο template.json για να προσθέσετε καταχωρήσεις για τα νέα κανάλια Προέλευση_συμβάντος πριν να εφαρμόσετε τη ρύθμιση παραμέτρων ενημέρωσης, χρησιμοποιώντας το `New-AzureRmResourceGroupDeployment` εντολή PowerShell. Το όνομα του αρχείου προέλευσης συμβάντων ορίζεται ως τμήμα του κώδικα στο αρχείο που δημιουργούνται από το Visual Studio ServiceEventSource.cs.

Για παράδειγμα, εάν το αρχείο προέλευσης συμβάντος ονομάζεται μου Προέλευση_συμβάντος, προσθέστε τον ακόλουθο κώδικα για να τοποθετήσετε τα συμβάντα από μου Προέλευση_συμβάντος σε έναν πίνακα με το όνομα MyDestinationTableName.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Για να συλλέξετε μετρητές επιδόσεων ή αρχεία καταγραφής συμβάντων, τροποποιήστε το πρότυπο διαχείρισης πόρων, χρησιμοποιώντας τα παραδείγματα που παρέχονται στο θέμα [Δημιουργία μια εικονική μηχανή Windows παρακολούθηση και διαγνωστικά με τη χρήση ενός προτύπου για τη διαχείριση πόρων Azure](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md). Στη συνέχεια, μπορείτε να δημοσιεύσετε ξανά το πρότυπο διαχείρισης πόρων.

## <a name="next-steps"></a>Επόμενα βήματα
Για να κατανοήσετε με περισσότερες λεπτομέρειες ποια συμβάντα που θα πρέπει να αναζητήσετε κατά την αντιμετώπιση προβλημάτων, ανατρέξτε στο θέμα τα συμβάντα διαγνωστικών που εκπέμπει για [Αξιόπιστη ηθοποιών](service-fabric-reliable-actors-diagnostics.md) και [Αξιόπιστη υπηρεσίες](service-fabric-reliable-services-diagnostics.md).


## <a name="related-articles"></a>Σχετικά άρθρα
* [Μάθετε πώς μπορείτε να συλλέξετε μετρητές επιδόσεων ή αρχεία καταγραφής χρησιμοποιώντας την επέκταση Διαγνωστικά](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)
* [Λύση ύφασμα υπηρεσίας στο αρχείο καταγραφής αναλυτικών στοιχείων](../log-analytics/log-analytics-service-fabric.md)
