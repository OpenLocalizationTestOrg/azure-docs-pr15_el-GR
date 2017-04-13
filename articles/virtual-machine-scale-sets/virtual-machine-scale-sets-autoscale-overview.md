<properties
    pageTitle="Αυτόματη κλιμάκωση και εικονική μηχανή κλιμάκωση σύνολα | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε εργαλεία διαγνωστικών και autoscale πόρους για να κλιμακωθεί αυτόματα εικονικές μηχανές σε ένα σύνολο κλίμακα."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="automatic-scaling-and-virtual-machine-scale-sets"></a>Αυτόματη κλιμάκωση και εικονική μηχανή κλιμάκωση σύνολα

Αυτόματη κλιμάκωση της εικονικές μηχανές σε ένα σύνολο κλίμακα είναι τη δημιουργία ή διαγραφή μηχανές του συνόλου σύμφωνα με τις ανάγκες ώστε να ταιριάζει με απαιτήσεις επιδόσεων. Καθώς εξελίσσεται του όγκου εργασίας, μια εφαρμογή μπορεί να απαιτούν πρόσθετους πόρους για να το ενεργοποιήσετε για την αποτελεσματική εκτέλεση εργασιών.

Αυτόματη κλιμάκωση είναι μια αυτοματοποιημένη διαδικασία που βοηθά στην ελάφρυνση επιβάρυνσης διαχείρισης. Με τη μείωση της επιβάρυνσης, δεν χρειάζεται να συνεχή παρακολούθηση των επιδόσεων του συστήματος ή την Αποφασίστε πώς μπορείτε να διαχειριστείτε τους πόρους. Κλιμάκωση είναι μια ελαστική διαδικασία. Περισσότεροι πόροι μπορούν να προστεθούν ως η φόρτωση αυξάνεται, αλλά ως ζήτηση πόρους μειώνει μπορούν να καταργηθούν Ελαχιστοποίηση κόστους και συντήρησης επιδόσεων.

Ρύθμιση αυτόματης κλίμακας σε κλίμακα ορίστε, χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure, Azure PowerShell, Azure CLI ή την πύλη του Azure.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Ρύθμιση κλιμάκωσης με τη χρήση προτύπων από διαχειριστή πόρων

Αντί για την ανάπτυξη και τη διαχείριση κάθε πόρο της εφαρμογής σας ξεχωριστά, χρησιμοποιήστε ένα πρότυπο που αναπτύσσει όλους τους πόρους σε μια ενιαία, συντονισμένη λειτουργία. Στο πρότυπο, ορίζονται πόρους εφαρμογής και ανάπτυξη παράμετροι έχουν καθοριστεί για διαφορετικά περιβάλλοντα. Το πρότυπο αποτελείται από JSON και παραστάσεις που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε τιμές για την ανάπτυξη. Για περισσότερες πληροφορίες, δείτε [πρότυπα διαχείρισης πόρων σύνταξης Azure](../resource-group-authoring-templates.md).

Στο πρότυπο, μπορείτε να καθορίσετε το στοιχείο δυνατότητα:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Δυνατότητα προσδιορίζει τον αριθμό των εικονικές μηχανές του συνόλου. Με μη αυτόματο τρόπο, μπορείτε να αλλάξετε την ικανότητα, την ανάπτυξη ενός προτύπου με μια διαφορετική τιμή. Εάν αναπτύσσετε ένα πρότυπο για να αλλάξετε μόνο η δυναμικότητα, μπορείτε να συμπεριλάβετε μόνο το στοιχείο SKU με το ενημερωμένο χωρητικότητα.

Αυτόματη αλλαγή η δυναμικότητα των σας κλίμακα ρυθμίσετε χρησιμοποιώντας το συνδυασμό του πόρου autoscaleSettings και την επέκταση Διαγνωστικά.

### <a name="configure-the-azure-diagnostics-extension"></a>Ρύθμιση παραμέτρων την επέκταση Διαγνωστικά του Azure

Αυτόματης κλίμακας μπορούν να γίνουν μόνο εάν είναι επιτυχής σε κάθε εικονική μηχανή του συνόλου κλίμακα μετρικά συλλογής. Η επέκταση Διαγνωστικά Azure παρέχει τις δυνατότητες παρακολούθησης και τα Διαγνωστικά που ανταποκρίνεται στις ανάγκες συλλογής μετρικά του πόρου autoscale. Μπορείτε να εγκαταστήσετε την επέκταση ως μέρος του προτύπου για τη διαχείριση πόρων.

Αυτό το παράδειγμα εμφανίζει τις μεταβλητές που χρησιμοποιούνται με το πρότυπο για να ρυθμίσετε την επέκταση διαγνωστικών:

    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Παράμετροι παρέχονται όταν έχει αναπτυχθεί το πρότυπο. Σε αυτό το παράδειγμα, το όνομα του λογαριασμού χώρου αποθήκευσης όπου αποθηκεύονται τα δεδομένα και το όνομα του συνόλου κλίμακα από το οποίο συλλέγονται δεδομένα παρέχονται. Επίσης σε αυτό το παράδειγμα Windows Server, μόνο ο μετρητής επιδόσεων νήματος Count συλλέγονται. Όλους τους διαθέσιμους μετρητές επιδόσεων στα Windows ή Linux μπορεί να χρησιμοποιηθεί για τη συλλογή πληροφοριών διαγνωστικών και μπορούν να συμπεριληφθούν στη ρύθμιση παραμέτρων επέκτασης.

Αυτό το παράδειγμα εμφανίζει τον ορισμό της επέκτασης με το πρότυπο:

    "extensionProfile": {
      "extensions": [
        {
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
              "storageAccount": "[variables('diagnosticsStorageAccountName')]"
            },
            "protectedSettings": {
              "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
              "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
              "storageAccountEndPoint": "https://core.windows.net"
            }
          }
        }
      ]
    }

Όταν εκτελείται την επέκταση Διαγνωστικά, τα δεδομένα που συλλέγονται σε έναν πίνακα που βρίσκεται στο χώρο αποθήκευσης στο λογαριασμό που έχετε καθορίσει. Στον πίνακα WADPerformanceCounters, μπορείτε να βρείτε τα δεδομένα που έχουν συλλεχθεί:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>Ρύθμιση παραμέτρων του πόρου autoScaleSettings

Ο πόρος autoscaleSettings χρησιμοποιεί τις πληροφορίες από την επέκταση Διαγνωστικά να αποφασίσετε εάν θα αυξήσετε ή να μειώσετε τον αριθμό των εικονικές μηχανές του συνόλου κλίμακα.

Αυτό το παράδειγμα εμφανίζει τη ρύθμιση παραμέτρων του πόρου στο πρότυπο:

    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 650
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 550
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
      }
    }

Στο παραπάνω παράδειγμα, δημιουργούνται δύο κανόνες για τον καθορισμό του αυτόματων ενεργειών κλίμακας. Ο πρώτος κανόνας καθορίζει την ενέργεια ανάληψη κλίμακα και τον δεύτερο κανόνα ορίζει την ενέργεια σε κλίμακα. Αυτές οι τιμές παρέχονται οι κανόνες:

- **metricName** - αυτή η τιμή είναι το ίδιο με του μετρητή απόδοσης που έχετε ορίσει στη μεταβλητή wadperfcounter για την επέκταση Διαγνωστικά. Στο παραπάνω παράδειγμα, χρησιμοποιείται ο μετρητής νήματος Count.  
- **metricResourceUri** - αυτή η τιμή είναι το αναγνωριστικό πόρου του συνόλου κλίμακα εικονική μηχανή. Αυτό το αναγνωριστικό περιέχει το όνομα της ομάδας πόρων, το όνομα της υπηρεσίας παροχής πόρων και το όνομα της κλίμακας ρύθμιση για να κλιμακωθεί.
- **timeGrain** – αυτή η τιμή είναι το επίπεδο λεπτομερειών της τα μετρικά που έχουν συλλεχθεί. Στο προηγούμενο παράδειγμα, συλλέγονται δεδομένα σε ένα χρονικό διάστημα από ένα λεπτό. Αυτή η τιμή χρησιμοποιείται με timeWindow.
- **στατιστική τιμή** – αυτή η τιμή καθορίζει τον τρόπο τα μετρικά συνδυάζονται για να χωρέσει την αυτόματη ενέργεια κλίμακας. Οι πιθανές τιμές είναι: Μέσος όρος, ελάχιστο, μέγιστο.
- **timeWindow** – αυτή η τιμή είναι η περιοχή του χρόνου στο οποίο συλλέγονται δεδομένα της παρουσίας. Πρέπει να είναι μεταξύ 5 λεπτά και 12 ώρες.
- **timeAggregation** – αυτή η τιμή που καθορίζει τον τρόπο θα πρέπει να συνδυάσετε τα δεδομένα που συλλέγονται μέσα στο χρόνο. Η προεπιλεγμένη τιμή είναι μέσο όρο. Οι πιθανές τιμές είναι: Μέσος όρος, ελάχιστο, μέγιστο, τελευταία, σύνολο, πλήθος.
- **τελεστής** – αυτή η τιμή είναι ο τελεστής που χρησιμοποιείται για να συγκρίνετε τα δεδομένα μετρικό και το όριο. Οι πιθανές τιμές είναι: ισούται με, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
- **όριο** – αυτή η τιμή είναι η τιμή που ενεργοποιεί την ενέργεια κλίμακα. Φροντίστε να παρέχουν επαρκή διαφορά μεταξύ του ορίου για την ενέργεια ανάληψη κλίμακα και το όριο για την ενέργεια σε κλίμακα. Εάν ορίσετε τις τιμές που θα είναι το ίδιο, το σύστημα προβλέπει σταθερό αλλαγή, το οποίο αποτρέπει την εφαρμογή μιας ενέργειας κλίμακας. Για παράδειγμα, τη ρύθμιση και τα δύο σε 600 νήματα στο προηγούμενο παράδειγμα δεν λειτουργεί.
- **κατεύθυνση** – αυτή η τιμή καθορίζει την ενέργεια που πραγματοποιείται όταν το όριο είναι δυνατό. Οι πιθανές τιμές είναι αύξηση ή μείωση.
- **Τύπος** – αυτή η τιμή είναι τον τύπο της ενέργειας που πρέπει να πραγματοποιείται και πρέπει να έχει οριστεί σε ChangeCount.
- **τιμή** – αυτή η τιμή είναι ο αριθμός των εικονικές μηχανές που έχουν προστεθεί ή καταργηθεί από το σύνολο κλίμακα. Αυτή η τιμή πρέπει να είναι 1 ή μεγαλύτερη.
- **cooldown** – αυτή η τιμή είναι το χρονικό διάστημα για να περιμένετε από την τελευταία ενέργεια κλίμακας πριν από την επόμενη ενέργεια. Αυτή η τιμή πρέπει να είναι μεταξύ ένα λεπτό και μία εβδομάδα.

Ανάλογα με το μετρητή επιδόσεων που χρησιμοποιείτε, ορισμένα από τα στοιχεία στη ρύθμιση παραμέτρων προτύπου χρησιμοποιούνται διαφορετικά. Στο προηγούμενο παράδειγμα, ο μετρητής επιδόσεων είναι νήματος καταμέτρηση, το όριο είναι 650 για μια ενέργεια κλιμάκωσης και το όριο είναι 550 για την ενέργεια κλίμακα του. Εάν χρησιμοποιείτε ένα μετρητή όπως % χρόνος επεξεργασίας, το όριο έχει οριστεί σε το ποσοστό της CPU που καθορίζει μια ενέργεια κλίμακας.

Όταν δημιουργείται μια φόρτωση σε τις εικονικές μηχανές που ενεργοποιεί μια ενέργεια κλίμακας, η δυναμικότητα του συνόλου αυξάνεται με βάση την τιμή στο πρότυπο. Για παράδειγμα, σε μια κλίμακα σύνολο όπου έχει οριστεί η δυναμικότητα με 3 και την τιμή ενέργεια κλίμακα έχει οριστεί σε 1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Όταν η φόρτωση δημιουργείται που προκαλεί το πλήθος average νήματος για να μεταβείτε υπερβαίνει το όριο του 650:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

Μια κλίμακα ανάληψης ενέργειά που προκαλεί τη δυνατότητα του συνόλου αυξάνεται κατά μία:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

Και μια εικονική μηχανή προστίθεται στο σύνολο κλίμακα:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Μετά από μια περίοδο cooldown πέντε λεπτά, εάν ο μέσος όρος των νήματα σε υπολογιστές παραμένει πάνω από 600, άλλο υπολογιστή που προστίθεται στο σύνολο. Εάν το πλήθος average νήματος παραμένει κάτω από το στοιχείο 550, η δυναμικότητα του συνόλου κλίμακα μειώνεται με μία και ένα μηχάνημα καταργείται από το σύνολο.

## <a name="set-up-scaling-using-azure-powershell"></a>Ρύθμιση κλιμάκωσης με χρήση του PowerShell Azure

Για να δείτε παραδείγματα χρήσης του PowerShell για να ρυθμίσετε autoscaling, εξετάστε το [PowerShell οθόνη Azure γρήγορης έναρξης δείγματα](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Ρύθμιση κλιμάκωσης χρησιμοποιώντας Azure CLI

Για να δείτε παραδείγματα χρήσης Azure CLI για να ρυθμίσετε autoscaling, εξετάστε το [Azure οθόνη πλατφόρμες CLI γρήγορης έναρξης δείγματα](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Ρύθμιση κλιμάκωσης με την πύλη Azure

Για να δείτε ένα παράδειγμα της χρήσης του Azure πύλη για να ρυθμίσετε autoscaling, δείτε [Δημιουργία συνόλου κλίμακα εικονική μηχανή με την πύλη Azure](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Διερεύνηση κλίμακας ενέργειες

- [Πύλη του azure]() - αυτήν τη στιγμή, μπορείτε να λάβετε ένα περιορισμένο χρονικό πληροφοριών με την πύλη.
- [Εξερεύνηση των πόρων azure]() - αυτό το εργαλείο είναι η καλύτερη για την Εξερεύνηση την τρέχουσα κατάσταση του συνόλου κλίμακα. Ακολουθήστε αυτήν τη διαδρομή και θα πρέπει να δείτε την προβολή παρουσία του συνόλου κλίμακας που δημιουργήσατε: συνδρομές > {τη συνδρομή σας} > resourceGroups > {σας ομάδα πόρων} > υπηρεσίες παροχής > Microsoft.Compute > virtualMachineScaleSets > {το σύνολο κλίμακα} > virtualMachines
- Azure PowerShell - Χρησιμοποιήστε αυτή την εντολή για να λάβετε ορισμένες πληροφορίες:

        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput

- Σύνδεση με τον υπολογιστή εικονικές jumpbox ακριβώς όπως θα κάνατε με οποιοδήποτε άλλο υπολογιστή και, στη συνέχεια, μπορείτε να αποκτήσετε απομακρυσμένη πρόσβαση τις εικονικές μηχανές της κλίμακας ρύθμιση για την παρακολούθηση μεμονωμένων διεργασιών.

## <a name="next-steps"></a>Επόμενα βήματα

- Δείτε [αυτόματης κλιμάκωσης μηχανές σε ένα σύνολο κλίμακα εικονική μηχανή](virtual-machine-scale-sets-windows-autoscale.md) για να δείτε ένα παράδειγμα του τρόπου για να δημιουργήσετε μια κλίμακα οριστεί η ρύθμιση αυτόματης κλίμακας έχει ρυθμιστεί.
- Βρείτε παραδείγματα της οθόνης Azure παρακολούθησης στο [Azure οθόνη PowerShell γρήγορης έναρξης δείγματα](../monitoring-and-diagnostics/insights-powershell-samples.md)
- Μάθετε σχετικά με τις δυνατότητες ειδοποίηση χρησιμοποιήσετε [autoscale ενέργειες για να στείλετε μηνύματα ηλεκτρονικού ταχυδρομείου και webhook οι ειδοποιήσεις στην οθόνη Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).
- Μάθετε περισσότερα σχετικά με τον τρόπο [χρήσης ελέγχου καταγράφει για να στείλετε μηνύματα ηλεκτρονικού ταχυδρομείου και webhook οι ειδοποιήσεις στην οθόνη Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
- Μάθετε σχετικά με τα [σενάρια autoscale για προχωρημένους](./virtual-machine-scale-sets-advanced-autoscale.md).
