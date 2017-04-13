<properties
    pageTitle="Azure διαχείριση πόρων υποστήριξης για τη Διαχείριση κίνηση | Microsoft Azure "
    description="Χρήση του PowerShell για τη Διαχείριση κίνηση με τη Διαχείριση Azure πόρων"
    services="traffic-manager"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="azure-resource-manager-support-for-azure-traffic-manager"></a>Azure διαχείριση πόρων υποστήριξης για τη Διαχείριση κίνηση Azure

Azure διαχείριση πόρων είναι το περιβάλλον εργασίας προτιμώμενη διαχείρισης για τις υπηρεσίες στο Azure. Azure προφίλ διαχείρισης κίνηση μπορείτε να διαχειριστείτε με χρήση API διαχείρισης πόρων Azure βάσει και εργαλεία.

## <a name="resource-model"></a>Μοντέλο πόρων

Azure κίνηση Manager έχει ρυθμιστεί ώστε να χρησιμοποιώντας μια συλλογή ρυθμίσεων που ονομάζεται ένα προφίλ διαχείρισης κίνηση. Αυτό το προφίλ περιέχει ρυθμίσεις DNS, ρυθμίσεις δρομολόγησης κυκλοφορίας, τελικού σημείου ρυθμίσεις παρακολούθησης και μια λίστα με τα τελικά σημεία υπηρεσίας στο οποίο η κυκλοφορία δρομολογείται.

Κάθε προφίλ διαχείρισης κίνηση αντιπροσωπεύεται από έναν πόρο τύπου 'TrafficManagerProfiles'. Στο επίπεδο REST API, το URI για κάθε προφίλ είναι ως εξής:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="comparison-with-the-azure-traffic-manager-classic-api"></a>Σύγκριση με το API κλασική Διαχείριση κίνηση Azure

Υποστήριξης της διαχείρισης πόρων Azure για τη Διαχείριση κίνηση χρησιμοποιεί διαφορετική ορολογία σε σχέση με το μοντέλο κλασική ανάπτυξης. Ο παρακάτω πίνακας εμφανίζει τις διαφορές μεταξύ τους όρους της διαχείρισης πόρων και κλασική:

| Διαχείριση πόρων όρων | Κλασική όρων |
|-----------------------|--------------|
| Μέθοδος δρομολόγησης κίνηση | Μέθοδος εξισορρόπησης φόρτου |
| Μέθοδος προτεραιότητα | Μέθοδος ανακατεύθυνσης |
| Μέθοδος σταθμισμένου | Μέθοδος ROUND robin |
| Μέθοδος επιδόσεων | Μέθοδος επιδόσεων |

Θα σας που βασίζεται σε σχόλια των πελατών, αλλάξει την ορολογία ώστε να βελτιώσετε σαφήνεια και μείωση κοινά παρανοήσεις. Δεν υπάρχει διαφορά στη λειτουργία.

## <a name="limitations"></a>Περιορισμοί

Κατά την αναφορά τελικού σημείου τύπου 'AzureEndpoints' για μια εφαρμογή Web, τα τελικά σημεία κίνηση Manager μπορεί να αναφέρεται μόνο η προεπιλογή (παραγωγή) [υποδιαίρεση Web App](../app-service-web/web-sites-staged-publishing.md). Προσαρμοσμένη υποδοχές δεν υποστηρίζονται. Ως λύση, προσαρμοσμένες υποδοχές μπορούν να ρυθμιστούν με τη χρήση του τύπου 'ExternalEndpoints'.

## <a name="setting-up-azure-powershell"></a>Για τη ρύθμιση του Azure PowerShell

Χρησιμοποιήστε το Microsoft Azure PowerShell αυτές τις οδηγίες. Το ακόλουθο άρθρο εξηγεί πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure.

- [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md)

Τα παραδείγματα σε αυτό το άρθρο προϋποθέτουν ότι έχετε μια υπάρχουσα ομάδα πόρων. Μπορείτε να δημιουργήσετε μια ομάδα πόρων χρησιμοποιώντας την ακόλουθη εντολή:

```powershell
    New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

>[AZURE.NOTE] Azure διαχείριση πόρων απαιτούν όλων των ομάδων πόρων μια θέση. Αυτή η θέση χρησιμοποιείται ως προεπιλογή για τους πόρους που έχουν δημιουργηθεί με αυτήν την ομάδα πόρων. Ωστόσο, επειδή το κίνηση Διαχείριση προφίλ πόροι είναι καθολικού, δεν τοπικές ρυθμίσεις, η επιλογή της θέσης ομάδα πόρων έχει καμία επίδραση στη Διαχείριση Azure κίνηση.

## <a name="create-a-traffic-manager-profile"></a>Δημιουργήστε ένα προφίλ διαχείρισης κίνηση

Για να δημιουργήσετε ένα προφίλ διαχείρισης κίνηση, χρησιμοποιήστε το cmdlet New-AzureRmTrafficManagerProfile:

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

Ο παρακάτω πίνακας περιγράφει τις παραμέτρους:

| Παράμετρος | Περιγραφή |
|-----------|-------------|
| Όνομα | Το όνομα του πόρου για τον πόρο προφίλ Manager κίνηση. Προφίλ στην ίδια ομάδα πόρων πρέπει να έχουν μοναδικά ονόματα. Αυτό το όνομα είναι ξεχωριστό από το όνομα DNS που χρησιμοποιείται για ερωτήματα DNS.|
| ResourceGroupName | Το όνομα της ομάδας πόρων που περιέχει τον πόρο προφίλ.|
| TrafficRoutingMethod | Καθορίζει τη μέθοδο δρομολόγησης κίνηση που χρησιμοποιείται για τον προσδιορισμό των τελικού σημείου επιστρέφεται απάντηση ενός ερωτήματος DNS. Πιθανές τιμές είναι 'Επιδόσεις', 'Weighted' ή 'Προτεραιότητα'.|
| RelativeDnsName | Καθορίζει το όνομα κεντρικού υπολογιστή τμήμα του ονόματος DNS που παρέχεται από αυτό το προφίλ Manager κίνηση. Αυτή η τιμή συνδυάζεται με το όνομα τομέα DNS που χρησιμοποιείται από τη Διαχείριση Azure κίνηση για να σχηματίσουν το πλήρως προσδιορισμένο όνομα τομέα (FQDN) του προφίλ. Για παράδειγμα, η ρύθμιση της τιμής "contoso" γίνεται "contoso.trafficmanager.net."|
| TTL | Καθορίζει το DNS Time-to-Live (TTL), σε δευτερόλεπτα. Σε αυτό το TTL πληροφορεί το επίλυσης DNS τοπικό και προγράμματα-πελάτες DNS χρονικό διάστημα στο cache DNS απαντήσεων για αυτό το προφίλ Manager κίνηση.|
| MonitorProtocol | Καθορίζει το πρωτόκολλο που θα χρησιμοποιηθεί για την παρακολούθηση της εύρυθμης λειτουργίας του τελικού σημείου. Πιθανές τιμές είναι 'HTTP' και 'HTTPS'.|
| MonitorPort | Καθορίζει τη θύρα TCP που χρησιμοποιείται για την παρακολούθηση της εύρυθμης λειτουργίας του τελικού σημείου.|
| MonitorPath | Καθορίζει τη διαδρομή σε σχέση με το τελικό σημείο όνομα τομέα που χρησιμοποιείται για τη διερεύνηση για το τελικό σημείο εύρυθμης λειτουργίας.|

Το cmdlet δημιουργεί ένα προφίλ διαχείρισης κίνηση στο Azure και επιστρέφει μια αντίστοιχη αντικείμενο προφίλ με το PowerShell. Σε αυτό το σημείο του προφίλ δεν περιέχει οποιαδήποτε τελικά σημεία. Για περισσότερες πληροφορίες σχετικά με την προσθήκη τελικά σημεία ενός προφίλ διαχείρισης κίνηση, ανατρέξτε στο θέμα [Προσθήκη τα τελικά σημεία Manager κίνηση](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Λάβετε ένα προφίλ διαχείρισης κίνηση

Για να ανακτήσετε ένα υπάρχον αντικείμενο προφίλ Manager κίνηση, χρησιμοποιήστε το cmdlet Get-AzureRmTrafficManagerProfle:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Αυτό το cmdlet επιστρέφει ένα αντικείμενο προφίλ Manager κίνηση.

## <a name="update-a-traffic-manager-profile"></a>Ενημερώστε ένα προφίλ διαχείρισης κίνηση

Τροποποίηση προφίλ διαχείρισης κίνηση ακολουθεί μια διαδικασία βήμα 3:

1. Ανάκτηση του προφίλ, χρησιμοποιώντας Get-AzureRmTrafficManagerProfile ή χρησιμοποιήστε το προφίλ που επιστρέφονται από νέα AzureRmTrafficManagerProfile.
2. Τροποποιήστε το προφίλ. Μπορείτε να προσθέσετε και να καταργήσετε τα τελικά σημεία ή αλλαγή των παραμέτρων του τελικού σημείου ή το προφίλ σας. Αυτές οι αλλαγές είναι λειτουργίες χωρίς σύνδεση. Πρόκειται να αλλάξετε μόνο το τοπικό αντικείμενο στη μνήμη που αντιπροσωπεύει το προφίλ.
3. Ολοκλήρωση χρησιμοποιώντας το cmdlet Set-AzureRmTrafficManagerProfile τις αλλαγές σας.

Όλες οι ιδιότητες προφίλ μπορούν να αλλάξουν εκτός από το προφίλ RelativeDnsName. Για να αλλάξετε το RelativeDnsName, πρέπει να διαγράψετε το προφίλ και ένα νέο προφίλ με νέο όνομα.

Το παρακάτω παράδειγμα δείχνει τον τρόπο για να αλλάξετε το προφίλ TTL:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Υπάρχουν τρεις τύποι Manager κίνηση τελικά σημεία:

1. **Azure τελικά σημεία** είναι υπηρεσίες που φιλοξενούνται στο Azure
2. **Εξωτερική τελικά σημεία** είναι υπηρεσίες που φιλοξενείται εκτός του Azure
3. **Ένθετες τελικά σημεία** που χρησιμοποιούνται για την κατασκευή ένθετη ιεραρχίες κίνηση Διαχείριση προφίλ του. Ένθετες τελικά σημεία ενεργοποίηση για προχωρημένους διαμορφώσεις δρομολόγηση κίνηση για σύνθετη εφαρμογές.

Σε όλες τις τρεις περιπτώσεις, μπορείτε να προσθέσετε τα τελικά σημεία με δύο τρόπους:

1. Χρήση μιας διαδικασίας 3-βήμα που περιγράφονται παραπάνω. Το πλεονέκτημα αυτής της μεθόδου είναι ότι μπορούν να γίνουν πολλές αλλαγές τελικού σημείου σε μια μεμονωμένη ενημερωμένη έκδοση.
2. Χρησιμοποιώντας το cmdlet New-AzureRmTrafficManagerEndpoint. Αυτό το cmdlet προσθέτει ένα τελικό σημείο σε υπάρχον προφίλ Manager κίνηση σε μια μεμονωμένη λειτουργία.

## <a name="adding-azure-endpoints"></a>Προσθήκη Azure τελικά σημεία

Azure τελικά σημεία αναφοράς υπηρεσιών που φιλοξενούνται στο Azure. Υποστηρίζονται τρεις τύποι Azure τελικά σημεία:

1. Εφαρμογές Azure Web
2. 'Κλασική' στο cloud services (το οποίο μπορεί να περιέχει μια υπηρεσία PaaS ή IaaS εικονικές μηχανές)
3. Azure PublicIpAddress πόροι (το οποίο μπορεί να συνδεθεί σε μια μονάδα εξισορρόπησης φόρτου ή μια εικονική μηχανή NIC). Το PublicIpAddress πρέπει να έχετε ένα όνομα DNS στους οποίους έχουν ανατεθεί θα χρησιμοποιηθεί στη Διαχείριση κίνηση.

Σε κάθε περίπτωση:

- Η υπηρεσία έχει καθοριστεί χρησιμοποιώντας την παράμετρο 'targetResourceId' Προσθήκη AzureRmTrafficManagerEndpointConfig ή δημιουργία AzureRmTrafficManagerEndpoint.
- Η 'Target' και 'EndpointLocation' είναι υποδηλώνεται από το TargetResourceId.
- Καθορισμός το 'βάρος' είναι προαιρετικό. Βάρους χρησιμοποιούνται μόνο εάν το προφίλ έχει ρυθμιστεί ώστε να χρησιμοποιήσετε τη μέθοδο δρομολόγησης κίνηση 'Weighted'. Διαφορετικά, αγνοούνται. Εάν καθοριστεί, η τιμή πρέπει να είναι ένας αριθμός μεταξύ 1 και 1000. Η προεπιλεγμένη τιμή είναι "1".
- Καθορισμός την "προτεραιότητα" είναι προαιρετικό. Προτεραιότητες χρησιμοποιούνται μόνο εάν το προφίλ έχει ρυθμιστεί ώστε να χρησιμοποιήσετε τη μέθοδο δρομολόγησης κίνηση 'Προτεραιότητα'. Διαφορετικά, αγνοούνται. Έγκυρες τιμές είναι από 1 έως 1000 με χαμηλότερες τιμές που υποδεικνύει την υψηλότερη προτεραιότητα. Εάν καθοριστεί για ένα τελικό σημείο, πρέπει να καθοριστεί για όλα τα τελικά σημεία. Εάν παραλειφθεί το όρισμα, προεπιλεγμένες τιμές, ξεκινώντας από το "1" εφαρμόζονται με τη σειρά που παρατίθενται τα τελικά σημεία.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Παράδειγμα 1: Προσθήκη με προσθήκη AzureRmTrafficManagerEndpointConfig τα τελικά σημεία Web App

Σε αυτό το παράδειγμα, δημιουργήστε ένα προφίλ διαχείρισης κίνηση και προσθέστε δύο τελικά σημεία Web App χρησιμοποιώντας το cmdlet Προσθήκη AzureRmTrafficManagerEndpointConfig.

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-a-classic-cloud-service-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Παράδειγμα 2: Προσθήκη ενός τελικού σημείου υπηρεσίας cloud 'κλασική' με νέα AzureRmTrafficManagerEndpoint

Σε αυτό το παράδειγμα, μια "κλασική" τελικού σημείου υπηρεσίας Cloud προστίθεται ένα προφίλ διαχείρισης κίνηση. Σε αυτό το παράδειγμα, θα σας που καθορίζεται στο προφίλ χρησιμοποιώντας τα ονόματα ομάδων προφίλ και πόρων, αντί να που περνά μέσα σε ένα αντικείμενο προφίλ. Υποστηρίζονται και τις δύο μεθόδους.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### <a name="example-3-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Παράδειγμα 3: Προσθήκη ένα τελικό σημείο publicIpAddress με νέα AzureRmTrafficManagerEndpoint

Σε αυτό το παράδειγμα, προστίθεται πόρου δημόσια διεύθυνση IP για το προφίλ της διαχείρισης κίνηση. Πρέπει να έχετε ένα όνομα DNS έχει ρυθμιστεί στη δημόσια διεύθυνση IP και μπορεί να συνδεθεί στο NIC από μια Εικονική ή σε μια μονάδα εξισορρόπησης φόρτου.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## <a name="adding-external-endpoints"></a>Προσθήκη εξωτερικών τελικά σημεία

Διαχείριση κίνηση χρησιμοποιεί εξωτερικό τελικά σημεία για να κατευθύνουν κυκλοφορία με τις υπηρεσίες που φιλοξενείται εκτός του Azure. Όπως με Azure τελικά σημεία, μπορούν να προστεθούν εξωτερικών τελικά σημεία είτε με προσθήκη-AzureRmTrafficManagerEndpointConfig ακολουθούμενο από σύνολο AzureRmTrafficManagerProfile ή δημιουργία AzureRMTrafficManagerEndpoint.

Κατά τον καθορισμό εξωτερικών τελικά σημεία:

- Το όνομα τομέα τελικού σημείου πρέπει να έχει καθοριστεί χρησιμοποιώντας την παράμετρο 'Target'
- Εάν χρησιμοποιείται η μέθοδος δρομολόγησης κίνηση 'Επιδόσεις', 'EndpointLocation' είναι απαραίτητο. Διαφορετικά είναι προαιρετικό. Η τιμή πρέπει να είναι ένα [όνομα έγκυρη Azure περιοχής](https://azure.microsoft.com/regions/).
- Η 'Βάρος' και 'Προτεραιότητα' είναι προαιρετικό.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Παράδειγμα 1: Προσθήκη εξωτερικών τελικά σημεία με προσθήκη AzureRmTrafficManagerEndpointConfig και σύνολο AzureRmTrafficManagerProfile

Σε αυτό το παράδειγμα, θα σας δημιουργία κίνηση Διαχείριση προφίλ, προσθέστε δύο εξωτερικών τελικά σημεία και αποθηκεύστε τις αλλαγές.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Παράδειγμα 2: Προσθήκη εξωτερικών τελικά σημεία με νέα AzureRmTrafficManagerEndpoint

Σε αυτό το παράδειγμα, προσθέσουμε εξωτερικών τελικού σημείου σε υπάρχον προφίλ. Στο προφίλ που έχει καθοριστεί χρησιμοποιώντας τα ονόματα ομάδων προφίλ και πόρων.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## <a name="adding-nested-endpoints"></a>Προσθήκη τελικά σημεία 'Ένθετες'

Κάθε προφίλ διαχείρισης κίνηση καθορίζει μία μέθοδο δρομολόγησης κίνηση. Ωστόσο, υπάρχουν σενάρια που απαιτούν πιο εξελιγμένη δρομολόγηση κίνηση από αυτήν τη δρομολόγηση που παρέχεται από ένα μεμονωμένο προφίλ διαχείρισης κίνηση. Μπορείτε να κάνετε ένθεση προφίλ διαχείρισης κίνηση για να συνδυάσετε τα οφέλη από περισσότερες από μία μέθοδο δρομολόγησης κίνηση. Ένθετες προφίλ σάς επιτρέπουν να παρακάμψετε την προεπιλεγμένη συμπεριφορά Manager κίνηση για υποστήριξη μεγαλύτερων και πιο σύνθετες αναπτύξεις εφαρμογής. Για πιο λεπτομερείς παραδείγματα, ανατρέξτε στο θέμα [προφίλ ένθετες διαχείρισης κίνηση](traffic-manager-nested-profiles.md).

Ένθετες τελικά σημεία έχουν ρυθμιστεί το προφίλ του γονικού, χρησιμοποιώντας έναν τύπο συγκεκριμένες τελικού σημείου, 'NestedEndpoints'. Κατά τον καθορισμό ένθετη τελικά σημεία:

- Το τελικό σημείο πρέπει να έχει καθοριστεί χρησιμοποιώντας την παράμετρο 'targetResourceId'
- Εάν χρησιμοποιείται η μέθοδος δρομολόγησης κίνηση 'Επιδόσεις', 'EndpointLocation' είναι απαραίτητο. Διαφορετικά είναι προαιρετικό. Η τιμή πρέπει να είναι ένα [όνομα έγκυρη Azure περιοχής](http://azure.microsoft.com/regions/).
- Η 'Βάρος' και 'Προτεραιότητα' είναι προαιρετική, όπως για Azure τελικά σημεία.
- Η παράμετρος 'MinChildEndpoints' είναι προαιρετικό. Η προεπιλεγμένη τιμή είναι "1". Εάν ο αριθμός των διαθέσιμη τελικά σημεία κάτω από αυτό το όριο, το γονικό στοιχείο προφίλ θεωρεί το προφίλ του παιδιού 'υποβαθμισμένο' και εκτρέπει κίνηση για τα άλλα τελικά σημεία στο γονικό προφίλ.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Παράδειγμα 1: Προσθήκη ένθετων τελικά σημεία με προσθήκη AzureRmTrafficManagerEndpointConfig και σύνολο AzureRmTrafficManagerProfile

Σε αυτό το παράδειγμα, θα σας δημιουργία νέων Manager κίνηση θυγατρικό και γονικό προφίλ, προσθέστε το παιδί ως μια ένθετη τελικό σημείο για το γονικό στοιχείο και αποθηκεύστε τις αλλαγές.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

Για συντομία σε αυτό το παράδειγμα, θα σας δεν προσθέσατε άλλα τελικά σημεία των θυγατρικών ή γονικό προφίλ.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Παράδειγμα 2: Προσθήκη ένθετων τελικά σημεία με νέα AzureRmTrafficManagerEndpoint

Σε αυτό το παράδειγμα, προσθέσουμε ένα υπάρχον προφίλ θυγατρικό ως ένα τελικό σημείο ένθετη σε υπάρχον προφίλ γονικό στοιχείο. Στο προφίλ που έχει καθοριστεί χρησιμοποιώντας τα ονόματα ομάδων προφίλ και πόρων.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2

## <a name="update-a-traffic-manager-endpoint"></a>Ενημερώστε ένα τελικό σημείο Manager κίνηση

Υπάρχουν δύο τρόποι για να ενημερώσετε ένα υπάρχον τελικό σημείο Manager κίνηση:

1. Λάβετε το προφίλ της διαχείρισης κίνηση χρησιμοποιώντας Get-AzureRmTrafficManagerProfile, ενημερώστε τις ιδιότητες τελικό σημείο μέσα στο προφίλ, και εφαρμόστε τις αλλαγές με χρήση AzureRmTrafficManagerProfile σύνολο. Αυτή η μέθοδος έχει το πλεονέκτημα της τη δυνατότητα να ενημερώσετε περισσότερα από ένα τελικό σημείο με μία λειτουργία.
2. Λάβετε το τελικό σημείο Manager κίνηση χρησιμοποιώντας Get-AzureRmTrafficManagerEndpoint, ενημερώστε τις ιδιότητες τελικού σημείου, και εφαρμόστε τις αλλαγές με χρήση AzureRmTrafficManagerEndpoint σύνολο. Αυτή η μέθοδος είναι απλούστερο, επειδή δεν απαιτεί τη δημιουργία ευρετηρίου σε έναν πίνακα τελικά σημεία στο προφίλ.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Παράδειγμα 1: Ενημέρωση χρησιμοποιώντας Get-AzureRmTrafficManagerProfile και σύνολο AzureRmTrafficManagerProfile τα τελικά σημεία

Σε αυτό το παράδειγμα, θα σας να τροποποιήσετε την προτεραιότητα σε δύο τελικά σημεία μέσα σε ένα υπάρχον προφίλ.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Παράδειγμα 2: Ενημέρωση ενός ορίου χρήσης Get-AzureRmTrafficManagerEndpoint και AzureRmTrafficManagerEndpoint σύνολο

Σε αυτό το παράδειγμα, θα σας να τροποποιήσετε το πάχος από ένα μεμονωμένο τελικό σημείο σε ένα υπάρχον προφίλ.

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Ενεργοποίηση και απενεργοποίηση τελικά σημεία και προφίλ

Κίνηση Manager επιτρέπει μεμονωμένα τελικά σημεία για να ενεργοποιηθεί και να απενεργοποιηθεί, καθώς και παρέχει ενεργοποίηση και απενεργοποίηση των ολόκληρο προφίλ.
Αυτές οι αλλαγές μπορούν να γίνουν από λήψη/ενημέρωση/ρύθμιση τους πόρους τελικού σημείου ή το προφίλ σας. Για να οργανώσετε αυτές τις κοινές λειτουργίες, επίσης υποστηρίζονται μέσω των cmdlet αποκλειστικό.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Παράδειγμα 1: Ενεργοποίηση και απενεργοποίηση ένα προφίλ διαχείρισης κίνηση

Για να ενεργοποιήσετε ένα προφίλ διαχείρισης κίνηση, χρησιμοποιήστε Ενεργοποίηση AzureRmTrafficManagerProfile. Στο προφίλ που μπορεί να καθοριστεί χρησιμοποιώντας ένα αντικείμενο προφίλ. Το αντικείμενο προφίλ μπορούν να περάσουν μέσω της διοχέτευσης ή χρησιμοποιώντας το '-TrafficManagerProfile' παραμέτρου. Σε αυτό το παράδειγμα, θα σας να καθορίσετε το προφίλ με το όνομα της ομάδας προφίλ και πόρων.

```powershell
    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Για να απενεργοποιήσετε ένα προφίλ διαχείρισης κίνηση:

```powershell
    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Το cmdlet απενεργοποίηση AzureRmTrafficManagerProfile μηνύματα για επιβεβαίωση. Αυτή η ερώτηση μπορούν να αποκρύπτονται χρησιμοποιώντας το '-ισχύ ' παραμέτρου.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Παράδειγμα 2: Ενεργοποίηση και απενεργοποίηση ένα τελικό σημείο Manager κίνηση

Για να ενεργοποιήσετε ένα τελικό σημείο Manager κίνηση, χρησιμοποιήστε Ενεργοποίηση AzureRmTrafficManagerEndpoint. Υπάρχουν δύο τρόποι για να καθορίσετε το τελικό σημείο

1. Χρησιμοποιώντας ένα αντικείμενο TrafficManagerEndpoint διέλευσή του μέσω της διοχέτευσης ή χρησιμοποιώντας το '-TrafficManagerEndpoint' παραμέτρου
2. Χρησιμοποιώντας το όνομα τελικού σημείου, τον τύπο σημείου τέλους, όνομα προφίλ και όνομα ομάδας πόρων:

```powershell
    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Ομοίως, για να απενεργοποιήσετε ένα τελικό σημείο Manager κίνηση:

```powershell
     Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Όπως με απενεργοποίηση-AzureRmTrafficManagerProfile, το cmdlet απενεργοποίηση AzureRmTrafficManagerEndpoint μηνύματα για επιβεβαίωση. Αυτή η ερώτηση μπορούν να αποκρύπτονται χρησιμοποιώντας το '-ισχύ ' παραμέτρου.

## <a name="delete-a-traffic-manager-endpoint"></a>Διαγραφή ενός ορίου Manager κίνηση

Για να καταργήσετε μεμονωμένα τελικά σημεία, χρησιμοποιήστε το cmdlet κατάργηση AzureRmTrafficManagerEndpoint:

```powershell
    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Αυτό το cmdlet μηνύματα για επιβεβαίωση. Αυτή η ερώτηση μπορούν να αποκρύπτονται χρησιμοποιώντας το '-ισχύ ' παραμέτρου.

## <a name="delete-a-traffic-manager-profile"></a>Διαγραφή προφίλ Manager κίνηση

Για να διαγράψετε ένα προφίλ διαχείρισης κίνηση, χρησιμοποιήστε το cmdlet κατάργηση AzureRmTrafficManagerProfile, καθορίζοντας τα ονόματα ομάδων προφίλ και πόρων:

```powershell
    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Αυτό το cmdlet μηνύματα για επιβεβαίωση. Αυτή η ερώτηση μπορούν να αποκρύπτονται χρησιμοποιώντας το '-ισχύ ' παραμέτρου.

Στο προφίλ που θα διαγραφεί μπορείτε επίσης να καθορίσετε χρησιμοποιώντας ένα αντικείμενο προφίλ:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Επίσης μπορούν να διοχετευθούν αυτήν τη σειρά:

```powershell
    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Επόμενα βήματα

[Παρακολούθηση Manager κίνηση](traffic-manager-monitoring.md)

[Θέματα επιδόσεων για τη Διαχείριση κίνηση](traffic-manager-performance-considerations.md)
