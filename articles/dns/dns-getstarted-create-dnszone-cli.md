<properties
   pageTitle="Δημιουργήστε μια ζώνη DNS χρησιμοποιώντας CLI | Microsoft Azure"
   description="Μάθετε πώς να δημιουργείτε βήμα προς βήμα για να ξεκινήσετε φιλοξενεί τον τομέα DNS χρησιμοποιώντας CLI ζώνες DNS για το Azure DNS"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>Δημιουργήστε μια ζώνη Azure DNS χρησιμοποιώντας CLI


> [AZURE.SELECTOR]
- [Πύλη του Azure](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)


Σε αυτό το άρθρο θα σας καθοδηγήσει τα βήματα για να δημιουργήσετε μια ζώνη DNS χρησιμοποιώντας CLI. Μπορείτε επίσης να δημιουργήσετε μια ζώνη DNS με τη χρήση του PowerShell ή το Azure πύλη.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Χρησιμοποιήστε το Microsoft Azure CLI αυτές τις οδηγίες. Φροντίστε να ενημερώνετε για την πιο πρόσφατη CLI Azure (0.9.8 ή νεότερη έκδοση) για να χρησιμοποιήσετε τις εντολές Azure DNS. Τύπος `azure -v` για να ελέγξετε ποια έκδοση του Azure CLI είναι εγκατεστημένο στον υπολογιστή σας.

## <a name="step-1---set-up-azure-cli"></a>Βήμα 1 - ρύθμιση Azure CLI

### <a name="1-install-azure-cli"></a>1. εγκατάσταση Azure CLI

Μπορείτε να εγκαταστήσετε το Azure CLI για Windows, Linux ή MAC. Ακολουθήστε τα παρακάτω βήματα πρέπει να ολοκληρωθούν πριν να διαχειρίζεστε το DNS Azure χρησιμοποιώντας Azure CLI. Περισσότερες πληροφορίες είναι διαθέσιμη από την [εγκατάσταση του Azure CLI](../xplat-cli-install.md). Οι εντολές DNS απαιτείται Azure CLI έκδοση 0.9.8 ή νεότερη έκδοση.

Όλες οι εντολές δίκτυο παροχής στο CLI μπορεί να βρεθεί χρησιμοποιώντας την ακόλουθη εντολή:

    azure network

### <a name="2-switch-cli-mode"></a>2. λειτουργία CLI διακόπτη

Azure DNS χρησιμοποιεί διαχείριση πόρων Azure. Βεβαιωθείτε ότι αλλάζετε CLI λειτουργία για να χρησιμοποιήσετε τις εντολές ARM.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3. συνδεθείτε στο λογαριασμό σας Azure

Θα σας ζητηθεί για τον έλεγχο ταυτότητας με τα διαπιστευτήριά σας. Λάβετε υπόψη ότι μπορείτε να χρησιμοποιήσετε μόνο τους λογαριασμούς ORGID.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4. Επιλέξτε τη συνδρομή

Επιλέξτε από το Azure όλες τις συνδρομές σας για να χρησιμοποιήσετε.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5. Δημιουργήστε μια ομάδα πόρων

Azure διαχείριση πόρων απαιτεί όλων των ομάδων πόρων καθορίσετε μια θέση. Χρησιμοποιείται ως την προεπιλεγμένη θέση για τους πόρους σε αυτήν την ομάδα πόρων. Ωστόσο, επειδή οι όλους τους πόρους DNS είναι καθολικού, δεν τοπικές, η επιλογή της θέσης ομάδα πόρων δεν έχει καμία επίδραση στην Azure DNS.

Μπορείτε να παραλείψετε αυτό το βήμα εάν χρησιμοποιείτε μια υπάρχουσα ομάδα πόρων.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. register

Η υπηρεσία Azure DNS γίνεται από την υπηρεσία παροχής Microsoft.Network πόρων. Azure τη συνδρομή σας πρέπει να έχει εγγραφεί για να χρησιμοποιήσετε αυτήν την υπηρεσία παροχής πόρων να μπορέσετε να χρησιμοποιήσετε Azure DNS. Αυτή είναι μια λειτουργία φορά για κάθε εγγραφή.

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>Βήμα 2 - Δημιουργία μιας ζώνης DNS

Μια ζώνη DNS δημιουργείται χρησιμοποιώντας το `azure network dns zone create` εντολή. Προαιρετικά, μπορείτε να δημιουργήσετε μια ζώνη DNS μαζί με ετικέτες. Ετικέτες είναι μια λίστα με ζεύγη ονόματος-τιμής και χρησιμοποιούνται από τη διαχείριση πόρων Azure με ετικέτα πόρους για σκοπούς χρέωσης ή ομαδοποίησης. Για περισσότερες πληροφορίες σχετικά με τις ετικέτες, ανατρέξτε στο θέμα [Χρήση ετικετών για να οργανώσετε το Azure πόρους](../resource-group-using-tags.md).

Στο Azure DNS, θα πρέπει να έχει καθοριστεί ζώνη ονόματα χωρίς να τερματίζονται μια **'.'**. Για παράδειγμα, ως '**contoso.com**' Όχι '**contoso.com.**'.


### <a name="to-create-a-dns-zone"></a>Για να δημιουργήσετε μια ζώνη DNS

Το παρακάτω παράδειγμα δημιουργεί μια ζώνη DNS που ονομάζεται *contoso.com* στην ομάδα πόρων που ονομάζεται *MyResourceGroup*.

Χρησιμοποιήστε το παράδειγμα για τη δημιουργία της ζώνης DNS, αντικαθιστώντας τις τιμές για τη δική σας.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>Για να δημιουργήσετε μια ζώνη DNS και ετικέτες.

Azure DNS CLI υποστηρίζει ετικέτες της ζώνες DNS που καθορίζονται, χρησιμοποιώντας την προαιρετική *-ετικέτα* παραμέτρου. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια ζώνη DNS με δύο ετικέτες, το project = επίδειξη και φάκελος = δοκιμής.

Χρησιμοποιήστε το παρακάτω παράδειγμα, για να δημιουργήσετε μια ζώνη DNS και ετικέτες, αντικαθιστώντας τις τιμές για τη δική σας.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Προβολή εγγραφών

Δημιουργία μιας ζώνης DNS δημιουργεί επίσης οι ακόλουθες εγγραφές DNS:

- Η καρτέλα 'Έναρξη της αρχής' (SOA). Αυτό είναι παρουσίαση στη ρίζα της κάθε ζώνης DNS.

- Το έγκυρων εγγραφές διακομιστή ονομάτων (NS). Αυτές εμφανίζουν ποιες όνομα διακομιστές που φιλοξενούν της ζώνης. Azure DNS χρησιμοποιεί ένα χώρο συγκέντρωσης των διακομιστών ονομάτων και επομένως διαφορετικούς διακομιστές ονομάτων μπορεί να αντιστοιχιστεί σε διαφορετικές ζώνες στο Azure DNS. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πληρεξουσίου τομέα στο Azure DNS](dns-domain-delegation.md) .

Για να δείτε αυτές τις εγγραφές, χρησιμοποιήστε `azure network dns-record-set show`.<BR>
*Χρήση: δικτύου dns σύνολο καρτελών εμφάνιση <-ομάδα πόρων >< dns-ζώνη-name > <name><type>*


Στο παρακάτω παράδειγμα, εάν εκτελείτε την εντολή με ομάδα πόρων *myresourcegroup*, εγγραφή όνομα συνόλου *"@"* (για μια εγγραφή ρίζα), και πληκτρολογήστε *SOA*, αυτό θα yield το εξής αποτέλεσμα:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
Για να προβάλετε τις εγγραφές NS που δημιουργήθηκαν με τη ζώνη, χρησιμοποιήστε την ακόλουθη εντολή:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Εγγραφή σύνολα από το ριζικό κατάλογο (ή *κορυφή*) χρήση DNS Zone **@** κατά την εγγραφή Ορισμός ονόματος.

## <a name="test"></a>Έλεγχος

Μπορείτε να ελέγξετε τη ζώνη DNS χρησιμοποιώντας DNS εργαλεία όπως το nslookup, ΣΚΆΨΙΜΟ, ή το `Resolve-DnsName` cmdlet του PowerShell.

Εάν έχετε ακόμη δεν έχετε αναθέσει τον τομέα σας για να χρησιμοποιήσετε τη νέα ζώνη στο Azure DNS, πρέπει να κατευθύνει το ερώτημα DNS απευθείας σε έναν από τους διακομιστές ονομάτων για τη ζώνη. Τους διακομιστές ονομάτων για τη ζώνη δίδονται στο τις εγγραφές NS, όπως αναφέρονται από "Εμφάνιση συνόλου εγγραφών dns azure δικτύου" παραπάνω. Βεβαιωθείτε ότι η substitute τις σωστές τιμές για τη ζώνη στην παρακάτω εντολή.

Το παρακάτω παράδειγμα χρησιμοποιεί το ΣΚΆΨΙΜΟ ερώτημα για τον τομέα contoso.com χρησιμοποιώντας τους διακομιστές ονομάτων που έχουν εκχωρηθεί για τη ζώνη DNS. Το ερώτημα έχει ώστε να οδηγούν σε ένα διακομιστή ονομάτων για τον οποίο χρησιμοποιήσαμε * @ * και όνομα ζώνης χρησιμοποιώντας το ΣΚΆΨΙΜΟ.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Επόμενα βήματα

Αφού δημιουργήσετε μια ζώνη DNS, δημιουργήστε [σύνολα εγγραφής και εγγραφές](dns-getstarted-create-recordset-cli.md) για να ξεκινήσετε επίλυση ονομάτων για τον τομέα σας στο Internet.
