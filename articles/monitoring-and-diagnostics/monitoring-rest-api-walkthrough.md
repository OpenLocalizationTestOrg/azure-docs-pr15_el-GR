<properties
    pageTitle="Παρακολούθηση REST API οδηγίες Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ελέγχουν την ταυτότητα αιτήσεων σε και να χρησιμοποιήσετε το Azure παρακολούθηση REST API."
    authors="mcollier, rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="mcollier"/>

# <a name="azure-monitoring-rest-api-walkthrough"></a>Παρακολούθηση REST API οδηγίες Azure
Αυτό το άρθρο σας δείχνει πώς μπορείτε να εκτελέσετε τον έλεγχο ταυτότητας, ώστε να σας κώδικα να χρησιμοποιήσετε το [Microsoft Azure οθόνη REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

Το API οθόνη Azure δίνει τη δυνατότητα να ανακτήσετε μέσω προγραμματισμού τα διαθέσιμα προεπιλεγμένων μετρικό ορισμών (ο τύπος μέτρησης όπως χρόνος CPU, αιτήσεις, κ.λπ.), υποδιαίρεση και μετρικό τιμές. Μόλις ανακτηθούν, τα δεδομένα μπορούν να αποθηκευτούν σε ένα χώρο αποθήκευσης χωριστά δεδομένα όπως βάση δεδομένων SQL Azure, DocumentDB ή Azure λίμνης δεδομένων. Από εκεί επιπλέον ανάλυση μπορεί να πραγματοποιηθεί σύμφωνα με τις ανάγκες.

Εκτός από την εργασία με διάφορα σημεία μετρικό δεδομένων, όπως σε αυτό το άρθρο παρουσιάζει, το API οθόνη καθιστά δυνατή την λίστας ειδοποίησης κανόνες, προβολή αρχείων καταγραφής δραστηριότητας και πολλά άλλα. Για μια πλήρη λίστα των λειτουργιών διαθέσιμη, ανατρέξτε στο άρθρο [Microsoft Azure οθόνη REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Πραγματοποίηση ελέγχου ταυτότητας αιτήσεις Azure οθόνη

Το πρώτο βήμα είναι να ελέγχουν την ταυτότητα της αίτησης.

Όλες τις εργασίες που έχουν εκτελεστεί σε το API οθόνη Azure χρήση του μοντέλου από διαχειριστή πόρων Azure ελέγχου ταυτότητας. Επομένως, όλες οι αιτήσεις πρέπει να υποβληθούν σε έλεγχο ταυτότητας με το Azure Active Directory (Azure AD). Μία προσέγγιση για τον έλεγχο ταυτότητας την εφαρμογή-πελάτη είναι να δημιουργήσετε μια Azure AD κεφάλαιο υπηρεσίας και να ανακτήσετε το διακριτικό ελέγχου ταυτότητας (JWT). Το παρακάτω δείγμα δέσμης ενεργειών δείχνει τη δημιουργία μιας υπηρεσίας Azure AD κεφαλαίου μέσω του PowerShell. Για μια πιο λεπτομερή Γνωρίστε, ανατρέξτε στην τεκμηρίωση σχετικά [με τη χρήση του PowerShell Azure για να δημιουργήσετε μια υπηρεσία κεφαλαίου για να αποκτήσετε πρόσβαση σε πόρους](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password—powershell). Επίσης, είναι δυνατό να [δημιουργήσετε μια αρχή υπηρεσίας μέσω της πύλης Azure](../resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Ερώτημα για το API οθόνη Azure, την εφαρμογή-πελάτη πρέπει να χρησιμοποιήσετε το κεφάλαιο που δημιουργήσατε προηγουμένως service για τον έλεγχο ταυτότητας. Το παρακάτω παράδειγμα δέσμη ενεργειών του PowerShell εμφανίζει μία προσέγγιση, χρησιμοποιώντας τη [Βιβλιοθήκη ελέγχου ταυτότητας Active Directory](../active-directory/active-directory-authentication-libraries.md) (ADAL) για να σας βοηθήσουν ο έλεγχος ταυτότητας JWT διακριτικού. Το διακριτικό JWT μεταβιβάζεται ως μέρος μιας παραμέτρου εξουσιοδότησης HTTP στις προσκλήσεις σε το Azure οθόνη REST API.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.windows.net/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)
 
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values 
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Όταν ολοκληρωθεί το βήμα ρύθμισης ελέγχου ταυτότητας, ερωτημάτων, στη συνέχεια, μπορεί να εκτελεστεί σε σχέση με το Azure οθόνη REST API. Υπάρχουν δύο ερωτήματα χρήσιμα:

1. Λίστα των ορισμών μετρικό για έναν πόρο

2. Ανάκτηση των μετρικό τιμών


## <a name="retrieve-metric-definitions"></a>Ανάκτηση μετρικό ορισμών
>[AZURE.NOTE] Για να ανακτήσετε μετρικό ορισμών χρησιμοποιώντας το Azure οθόνη REST API, χρησιμοποιήστε "2016-03-01" με την έκδοση API.

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Για μια εφαρμογή της λογικής Azure, των ορισμών μετρικό θα είναι παρόμοιο με το παρακάτω στιγμιότυπο οθόνης:

![ALT "JSON προβολή μετρικό ορισμού απάντηση".](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Για περισσότερες πληροφορίες, ανατρέξτε στην τεκμηρίωση [λίστας των ορισμών μετρικό για έναν πόρο στο Azure οθόνη REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) .

## <a name="retrieve-metric-values"></a>Ανάκτηση μετρικό τιμών
Μόλις οι διαθέσιμες μετρικό ορισμοί είναι γνωστό, στη συνέχεια, είναι δυνατό να ανακτήσει τις σχετικές τιμές μετρικό. Χρησιμοποιήστε τη μέτρηση όνομα "τιμή" (μην το ' localizedValue') για τυχόν φιλτραρίσματος αιτήσεις (για παράδειγμα, ανακτήσετε τα σημεία δεδομένων μετρικό 'CpuTime' και 'Αιτήσεις'). Εάν δεν εμφανίζονται φίλτρα έχουν καθοριστεί, επιστρέφεται η μέτρηση προεπιλεγμένης.

>[AZURE.NOTE] Για να ανακτήσετε μετρικό τιμές χρησιμοποιώντας το Azure οθόνη REST API, χρησιμοποιήστε "2016-06-01" με την έκδοση API.

**Μέθοδος**: ΛΉΨΗ

**Αίτηση URI**: https://management.azure.com/subscriptions/_{αναγνωριστικό συνδρομής}_/resourceGroups/_{--όνομα της ομάδας πόρων}_/providers/_{πόρων-παροχής-χώρο ονομάτων}_/_{Τύπος πόρου}_/έκδοση api & /providers/microsoft.insights/metrics?$filter=_{όνομα πόρου}__{φίλτρο}_=_{apiVersion}_

Για παράδειγμα, για να ανακτήσετε τα σημεία δεδομένων μετρικό RunsSucceeded για την περιοχή δεδομένη χρονική στιγμή και για μια ώρα ως 1 ώρα, η αίτηση θα είναι ως εξής:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Το αποτέλεσμα θα είναι παρόμοιο με αυτό το παράδειγμα που ακολουθεί στιγμιότυπο οθόνης:

![ALT "Που εμφανίζει την τιμή μέτρησης μέσος χρόνος απόκρισης JSON απάντηση"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Για να ανακτήσετε πολλά σημεία δεδομένων ή συνάθροιση, προσθήκη των ονομάτων μετρικό ορισμού και τύπους συνάθροισης στο φίλτρο, όπως φαίνεται στο ακόλουθο παράδειγμα:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>Χρήση ARMClient
Εναλλακτική λύση για τη χρήση του PowerShell (όπως φαίνεται παραπάνω), είναι να χρησιμοποιήσετε [ARMClient](https://github.com/projectkudu/ARMClient) στον υπολογιστή σας με Windows. ARMClient χειρίζεται αυτόματα τον έλεγχο ταυτότητας Azure AD (και που προκύπτει το διακριτικό JWT). Ακολουθήστε τα παρακάτω βήματα περιγράφουν χρήσης των ARMClient για την ανάκτηση μετρικό δεδομένων:

1. Εγκαταστήστε το [Chocolatey](https://chocolatey.org/) και [ARMClient](https://github.com/projectkudu/ARMClient).

2. Στο παράθυρο της terminal, πληκτρολογήστε _armclient.exe login_. Αυτό σας ζητά να συνδεθείτε στο Azure.

3. Τύπος _armclient ΛΉΨΗ [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01_

4. Τύπος _armclient ΛΉΨΗ [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01_


![ALT "Χρήση ARMClient ώστε να λειτουργεί με το Azure παρακολούθησης REST API"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)


## <a name="retrieve-the-resource-id"></a>Ανάκτηση το Αναγνωριστικό πόρου
Χρήση του REST API πραγματικά μπορεί να βοηθήσει να κατανοήσετε το διαθέσιμο μετρικό ορισμών υποδιαίρεση και σχετικές τιμές. Αυτά τα στοιχεία είναι χρήσιμο κατά τη χρήση της [Βιβλιοθήκης διαχείρισης Azure](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Για τον παραπάνω κώδικα, το Αναγνωριστικό πόρου για να χρησιμοποιήσετε είναι η πλήρης διαδρομή για την επιθυμητή Azure πόρων. Για παράδειγμα, για να υποβάλετε ερώτημα σε σχέση με μια εφαρμογή Web της Azure, θα είναι το Αναγνωριστικό πόρου:

*/Subscriptions/{Subscription-ID}/resourceGroups/{Resource-Group-Name}/providers/Microsoft.Web/Sites/{site-name}/*

Η παρακάτω λίστα περιέχει μερικά παραδείγματα μορφές Αναγνωριστικό πόρου για διάφορους Azure πόρους:

* **Ο διανομέας IoT** - /subscriptions/_{αναγνωριστικό συνδρομής}_/resourceGroups/_{--όνομα της ομάδας πόρων}_/providers/Microsoft.Devices/IotHubs/_{iot-διανομέα-name}_

* **Χώρος συγκέντρωσης ελαστικότητας SQL** - /subscriptions/_{αναγνωριστικό συνδρομής}_/resourceGroups/_{--όνομα της ομάδας πόρων}_/providers/Microsoft.Sql/servers/_{χώρου συγκέντρωσης-db}_/elasticpools/_{sql-χώρου συγκέντρωσης-name}_

* **Βάση δεδομένων SQL (v12)** - /subscriptions/_{αναγνωριστικό συνδρομής}_/resourceGroups/_{--όνομα της ομάδας πόρων}_/providers/Microsoft.Sql/servers/_{όνομα διακομιστή}_/databases/_{όνομα βάσης δεδομένων}_

* **Υπηρεσία Bus** - /subscriptions/_{αναγνωριστικό συνδρομής}_/resourceGroups/_{--όνομα της ομάδας πόρων}_/providers/Microsoft.ServiceBus/_{χώρο ονομάτων}_/_{servicebus-όνομα}_

* **Εικονική κλίμακα σύνολα** - /subscriptions/_{αναγνωριστικό συνδρομής}_/resourceGroups/_{--όνομα της ομάδας πόρων}_/providers/Microsoft.Compute/virtualMachineScaleSets/_{εικονική-όνομα}_

* **ΣΠΣ** - /subscriptions/_{αναγνωριστικό συνδρομής}_/resourceGroups/_{--όνομα της ομάδας πόρων}_/providers/Microsoft.Compute/virtualMachines/_{εικονική-όνομα}_

* **Συμβάν διανομείς** - /subscriptions/_{αναγνωριστικό συνδρομής}_/resourceGroups/_{--όνομα της ομάδας πόρων}_/providers/Microsoft.EventHub/namespaces/_{eventhub-χώρο ονομάτων}_


Υπάρχουν εναλλακτικές προσεγγίσεις για την ανάκτηση το Αναγνωριστικό πόρου, όπως με την Εξερεύνηση των πόρων Azure, προβολή τον επιθυμητό πόρο στην πύλη του Azure και μέσω του PowerShell ή το Azure CLI.

### <a name="azure-resource-explorer"></a>Εξερεύνηση Azure πόρων
Για να βρείτε το Αναγνωριστικό πόρου για έναν πόρο που θέλετε, ένα χρήσιμο προσέγγιση είναι να χρησιμοποιήσετε το εργαλείο [Azure Explorer πόρων](https://resources.azure.com) . Μετάβαση στον επιθυμητό πόρο και, στη συνέχεια, εξετάστε το Αναγνωριστικό που εμφανίζεται, όπως το παρακάτω στιγμιότυπο οθόνης:

![ALT "Εξερεύνηση Azure πόρων"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Πύλη του Azure
Επίσης μπορείτε να αποκτήσετε το Αναγνωριστικό πόρου από την πύλη του Azure. Για να το κάνετε, Μετάβαση στον επιθυμητό πόρο και, στη συνέχεια, επιλέξτε την εντολή Ιδιότητες. Το Αναγνωριστικό πόρου εμφανίζεται με το blade ιδιότητες, όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης:

![ALT "Αναγνωριστικό πόρου εμφανίζεται στο το blade ιδιότητες στην πύλη του Azure"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
Το Αναγνωριστικό πόρου μπορεί να ανακτηθεί χρησιμοποιώντας καθώς και των cmdlet του Azure PowerShell. Για παράδειγμα, για να αποκτήσετε το Αναγνωριστικό πόρου για μια εφαρμογή Web Azure, εκτελέστε το cmdlet Get-AzureRmWebApp, όπως το παρακάτω στιγμιότυπο οθόνης:

![ALT "Αναγνωριστικό πόρου που λαμβάνονται μέσω του PowerShell"](./media\monitoring-rest-api-walkthrough\resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
Για να ανακτήσετε το Αναγνωριστικό πόρου χρησιμοποιώντας το Azure CLI, εκτέλεση της εντολής "Εμφάνιση azure webapp", που καθορίζει το '--json' επιλογή, όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης:

![ALT "Αναγνωριστικό πόρου που λαμβάνονται μέσω του PowerShell"](./media\monitoring-rest-api-walkthrough\resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Ανάκτηση δεδομένων καταγραφής δραστηριότητας
Εκτός από την εργασία με μετρικό ορισμούς και τις σχετικές τιμές, επίσης, είναι δυνατή η ανάκτηση επιπλέον ενδιαφέρον ιδέες σχετικά με τους πόρους Azure. Ως παράδειγμα, είναι δυνατό να δεδομένα του [αρχείου καταγραφής δραστηριότητας](https://msdn.microsoft.com/library/azure/dn931934.aspx) ερωτήματος. Το παρακάτω παράδειγμα παρουσιάζει τη χρήση του Azure οθόνη REST API για δεδομένα του αρχείου καταγραφής δραστηριότητας ερωτήματος μέσα σε μια συγκεκριμένη περιοχή ημερομηνιών για μια συνδρομή στο Azure:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Επόμενα βήματα
* Διαβάστε την [Επισκόπηση της παρακολούθησης](monitoring-overview.md).
* Προβάλετε τα [υποστηριζόμενα μετρικά με οθόνη Azure](monitoring-supported-metrics.md).
* Αναθεωρήστε το [Microsoft Azure οθόνη REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Ελέγξτε τη [βιβλιοθήκη Azure διαχείρισης](https://msdn.microsoft.com/library/azure/mt417623.aspx).
