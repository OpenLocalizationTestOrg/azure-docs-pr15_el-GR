<properties
   pageTitle="Γρήγορα αποτελέσματα με το DNS Azure | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε ζώνες DNS για το Azure DNS. Αυτό είναι ένα βήμα προς βήμα για να λάβετε την πρώτη ζώνη DNS δημιουργήθηκε για να ξεκινήσετε φιλοξενεί τον DNS τομέα σας με χρήση του PowerShell."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-a-dns-zone-using-powershell"></a>Δημιουργήστε μια ζώνη DNS με χρήση του Powershell

> [AZURE.SELECTOR]
- [Πύλη του Azure](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)

Σε αυτό το άρθρο θα σας καθοδηγήσει τα βήματα για να δημιουργήσετε μια ζώνη DNS με χρήση του PowerShell. Μπορείτε επίσης να δημιουργήσετε μια ζώνη DNS χρησιμοποιώντας CLI ή την πύλη του Azure.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>Σχετικά με Etags και ετικέτες

### <a name="etags"></a>Etags

Ας υποθέσουμε ότι δύο άτομα ή δύο διεργασίες, δοκιμάστε να τροποποιήσετε μια εγγραφή DNS την ίδια στιγμή. Ποιο κερδίζει; Και ο νικητής γνωρίζει ότι αυτά απλώς έχετε αντικαταστήσει αλλαγές που έχουν δημιουργηθεί από κάποιον άλλο;

Azure DNS χρησιμοποιεί Etags για να χειριστείτε τις ταυτόχρονες αλλαγές με τον ίδιο πόρο με ασφάλεια. Κάθε πόρο DNS (zone ή σύνολο εγγραφών) έχει μια Etag που σχετίζονται με αυτό. Κάθε φορά που γίνεται ανάκτηση έναν πόρο, το Etag επίσης ανάκτηση. Κατά την ενημέρωση ενός πόρου, έχετε την επιλογή για να επιστρέψει το Etag, ώστε να Azure DNS να επαληθεύσετε ότι το Etag στην τις συμφωνίες διακομιστή. Επειδή κάθε ενημέρωση σε έναν πόρο έχει ως αποτέλεσμα το Etag που δημιουργείται ξανά, μια ασυμφωνία Etag υποδεικνύει μια αλλαγή ταυτόχρονες Παρουσιάστηκε. Etags χρησιμοποιούνται επίσης κατά τη δημιουργία ενός νέου πόρου για να βεβαιωθείτε ότι ο πόρος δεν υπάρχει ήδη.

Από προεπιλογή, το Azure DNS PowerShell χρησιμοποιεί Etags να αποκλείσετε ταυτόχρονες αλλαγές σε ζώνες και να καταγράψετε σύνολα. Προαιρετικό *-Αντικατάσταση* διακόπτης μπορεί να χρησιμοποιηθεί για να κάνετε απόκρυψη Etag ελέγχων, οπότε ταυτόχρονες αλλαγές που έχουν προκύψει θα αντικατασταθεί.

Στο επίπεδο από το Azure DNS REST API, Etags καθορίζονται χρησιμοποιώντας κεφαλίδες HTTP.  Συμπεριφορά τους δίνεται στον παρακάτω πίνακα:

|Κεφαλίδα|Συμπεριφορά|
|------|--------|
|Κανένας|ΤΟΠΟΘΈΤΗΣΗ πάντα με επιτυχία (χωρίς ελέγχων Etag)|
|IF-match<etag>|ΤΟΠΟΘΈΤΗΣΗ επιτυγχάνει μόνο εάν υπάρχει πόρων και ταιριάζει με Etag|
|IF-match *     | ΤΟΠΟΘΈΤΗΣΗ επιτυγχάνει μόνο εάν υπάρχει πόρων|
|Εάν κανένα-Ταίριασμα * |  ΤΟΠΟΘΈΤΗΣΗ επιτυγχάνει μόνο αν δεν υπάρχει πόρων|

### <a name="tags"></a>Ετικέτες

Ετικέτες είναι διαφορετικό από Etags. Ετικέτες είναι μια λίστα με ζεύγη ονόματος-τιμής και χρησιμοποιούνται από τη διαχείριση πόρων Azure με ετικέτα πόρους για σκοπούς χρέωσης ή ομαδοποίησης. Για περισσότερες πληροφορίες σχετικά με τις ετικέτες, ανατρέξτε στο θέμα [Χρήση ετικετών για να οργανώσετε το Azure πόρους](../resource-group-using-tags.md).

Azure DNS PowerShell υποστηρίζει ετικέτες σε ζώνες και σύνολα εγγραφών που καθορίζονται χρησιμοποιώντας τις επιλογές `-Tag` παραμέτρου.


## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Βεβαιωθείτε ότι έχετε τα ακόλουθα στοιχεία πριν από την έναρξη της ρύθμισης παραμέτρων.

- Μια συνδρομή του Azure. Εάν δεν έχετε ήδη μια συνδρομή του Azure, μπορείτε να ενεργοποιήσετε το [MSDN συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ή το σύμβολο του για έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/).

- Θα πρέπει να εγκαταστήσετε την πιο πρόσφατη έκδοση του τα cmdlet του PowerShell για τη διαχείριση πόρων Azure (1,0 ή νεότερη έκδοση). Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .

## <a name="step-1---sign-in"></a>Βήμα 1 - Είσοδος

Ανοίξτε την κονσόλα του PowerShell και συνδεθείτε στο λογαριασμό σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση του Windows PowerShell με το διαχειριστή πόρων](../powershell-azure-resource-manager.md).

Χρησιμοποιήστε το παρακάτω παράδειγμα, για να σας βοηθήσει να συνδεθείτε:

    Login-AzureRmAccount

Ελέγξτε τις συνδρομές για το λογαριασμό.

    Get-AzureRmSubscription

Καθορίστε τη συνδρομή στην οποία θέλετε να χρησιμοποιήσετε.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>Βήμα 2 - Δημιουργία μιας ομάδας πόρων

Azure διαχείριση πόρων απαιτεί όλων των ομάδων πόρων καθορίσετε μια θέση. Χρησιμοποιείται ως την προεπιλεγμένη θέση για τους πόρους σε αυτήν την ομάδα πόρων. Ωστόσο, επειδή οι όλους τους πόρους DNS είναι καθολικού, δεν τοπικές, η επιλογή της θέσης ομάδα πόρων δεν έχει καμία επίδραση στην Azure DNS.

Μπορείτε να παραλείψετε αυτό το βήμα εάν χρησιμοποιείτε μια υπάρχουσα ομάδα πόρων.

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>Βήμα 3 - Register

Η υπηρεσία Azure DNS γίνεται από την υπηρεσία παροχής Microsoft.Network πόρων. Azure τη συνδρομή σας πρέπει να έχει εγγραφεί για να χρησιμοποιήσετε αυτήν την υπηρεσία παροχής πόρων να μπορέσετε να χρησιμοποιήσετε Azure DNS. Αυτή είναι μια λειτουργία φορά για κάθε εγγραφή.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>Βήμα 4 - Δημιουργήστε μια ζώνη DNS

Δημιουργείται μια ζώνη DNS, χρησιμοποιώντας το `New-AzureRmDnsZone` cmdlet. Υπάρχουν παραδείγματα κάτω από το στοιχείο για να δημιουργήσετε μια ζώνη DNS με ή χωρίς ετικέτες. Για περισσότερες πληροφορίες σχετικά με τις ετικέτες, ανατρέξτε στην ενότητα σχετικά με τα [ετικέτες](#tags) σε αυτό το άρθρο.

>[AZURE.NOTE] Στο Azure DNS, θα πρέπει να έχει καθοριστεί ζώνη ονόματα χωρίς να τερματίζονται μια **'.'**. Για παράδειγμα, ως '**contoso.com**' Όχι '**contoso.com.**'.

### <a name="to-create-a-dns-zone"></a>Για να δημιουργήσετε μια ζώνη DNS

Το παρακάτω παράδειγμα δημιουργεί μια ζώνη DNS που ονομάζεται *contoso.com* στην ομάδα πόρων που ονομάζεται *MyResourceGroup*. Χρησιμοποιήστε το παράδειγμα, για να δημιουργήσετε μια ζώνη DNS, αντικαθιστώντας τις τιμές για τη δική σας.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>Για να δημιουργήσετε μια ζώνη DNS με ετικέτες

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια ζώνη DNS με δύο ετικέτες, *έργου = επίδειξη* και *φάκελος = δοκιμής*. Χρησιμοποιήστε το παράδειγμα, για να δημιουργήσετε μια ζώνη DNS, αντικαθιστώντας τις τιμές για τη δική σας.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>Προβολή εγγραφών

Δημιουργία μιας ζώνης DNS δημιουργεί επίσης οι ακόλουθες εγγραφές DNS:

- Η εγγραφή *Έναρξη της αρχής* (SOA). Αυτό είναι παρουσίαση στη ρίζα της κάθε ζώνης DNS.

- Το έγκυρων εγγραφές διακομιστή ονομάτων (NS). Αυτές εμφανίζουν ποιες όνομα διακομιστές που φιλοξενούν της ζώνης. Azure DNS χρησιμοποιεί ένα χώρο συγκέντρωσης των διακομιστών ονομάτων και επομένως διαφορετικούς διακομιστές ονομάτων μπορεί να εκχωρηθεί σε διαφορετικές ζώνες στο Azure DNS. Ανατρέξτε στο θέμα [πληρεξουσίου τομέα στο Azure DNS](dns-domain-delegation.md) για περισσότερες πληροφορίες.

Για να δείτε αυτές τις εγγραφές, χρησιμοποιήστε `Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


Εγγραφή σύνολα από το ριζικό κατάλογο (ή *κορυφή*) χρήση DNS Zone **@** κατά την εγγραφή Ορισμός ονόματος.


## <a name="test"></a>Έλεγχος

Μπορείτε να ελέγξετε τη ζώνη DNS χρησιμοποιώντας DNS εργαλεία όπως το nslookup, Σκάψιμο ή το [cmdlet του PowerShell επίλυση DnsName](https://technet.microsoft.com/library/jj590781.aspx).

Εάν έχετε ακόμη δεν έχετε αναθέσει τον τομέα σας για να χρησιμοποιήσετε τη νέα ζώνη στο Azure DNS, θα πρέπει να κατευθύνει το ερώτημα DNS απευθείας σε έναν από τους διακομιστές ονομάτων για τη ζώνη. Τους διακομιστές ονομάτων για τη ζώνη παρέχονται στις εγγραφές NS, όπως αναφέρονται από `Get-AzureRmDnsRecordSet` παραπάνω. Βεβαιωθείτε ότι η substitute τις σωστές τιμές για τη ζώνη σε την παρακάτω εντολή.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>Επόμενα βήματα

Αφού δημιουργήσετε μια ζώνη DNS, δημιουργήστε [σύνολα εγγραφής και εγγραφές](dns-getstarted-create-recordset.md) για να ξεκινήσετε επίλυση ονομάτων για τον τομέα σας στο Internet.

