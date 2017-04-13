<properties
   pageTitle="Διαχείριση σύνολα εγγραφών DNS και εγγραφές, χρησιμοποιώντας την πύλη του Azure | Microsoft Azure"
   description="Διαχείριση σύνολα εγγραφών DNS και εγγραφές σε Azure DNS όταν φιλοξενεί τον τομέα σας στο Azure DNS. Όλες οι εντολές του PowerShell για λειτουργίες σε σύνολα εγγραφών και εγγραφών."
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>Διαχειριστείτε τις εγγραφές DNS και σύνολα εγγραφών με χρήση του PowerShell



> [AZURE.SELECTOR]
- [Πύλη του Azure](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)



Αυτό το άρθρο σας δείχνει πώς μπορείτε να διαχειριστείτε σύνολα εγγραφών και τις εγγραφές για τη ζώνη DNS με χρήση του Windows PowerShell.

Είναι σημαντικό να κατανοήσετε τη διαφορά ανάμεσα σε σύνολα εγγραφών DNS και μεμονωμένες εγγραφές DNS. Ένα σύνολο εγγραφών είναι η συλλογή των εγγραφών σε μια ζώνη που έχουν το ίδιο όνομα και είναι του ίδιου τύπου. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία DNS σύνολα εγγραφών και εγγραφών χρησιμοποιώντας την πύλη του Azure](dns-getstarted-create-recordset-portal.md).

Για να διαχειριστείτε τις σύνολα εγγραφών και τις εγγραφές, χρειάζεστε την πιο πρόσφατη έκδοση του τα cmdlet του PowerShell για τη διαχείριση πόρων Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md). Για περισσότερες πληροφορίες σχετικά με την εργασία με το PowerShell, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure](../powershell-azure-resource-manager.md).



## <a name="create-a-new-record-set-and-a-record"></a>Δημιουργήστε ένα νέο σύνολο εγγραφών και μια εγγραφή

Για να δημιουργήσετε μια εγγραφή Ορισμός με χρήση του PowerShell, ανατρέξτε στο θέμα [Δημιουργία DNS σύνολα εγγραφών και εγγραφών με χρήση του PowerShell](dns-getstarted-create-recordset.md).

## <a name="get-a-record-set"></a>Λάβετε ένα σύνολο εγγραφών

Για να ανακτήσετε ένα υπάρχον σύνολο εγγραφών, χρησιμοποιήστε `Get-AzureRmDnsRecordSet`. Καθορίστε την εγγραφή Ορισμός σχετικό όνομα, τον τύπο εγγραφής και τη ζώνη.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Όπως και με `New-AzureRmDnsRecordSet`, το όνομα της καρτέλας πρέπει να είναι ένα σχετικό όνομα, γεγονός που σημαίνει ότι το πρέπει να συμπεριλάβετε το όνομα της ζώνης.

Μπορείτε να καθορίσετε τη ζώνη χρησιμοποιώντας το όνομα της ζώνης και το όνομα της ομάδας πόρων ή χρησιμοποιώντας ένα αντικείμενο ζώνης:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`Επιστρέφει ένα τοπικό αντικείμενο που αντιπροσωπεύει το σύνολο εγγραφών που δημιουργήθηκε σε Azure DNS.

## <a name="list-record-sets"></a>Λίστα σύνολα εγγραφών

Μπορείτε επίσης να χρησιμοποιήσετε`Get-AzureRmDnsRecordSet` σε λίστα εγγραφών σύνολα Εάν παραλείψετε το *– όνομα* ή/και *– RecordType* παράμετροι.

### <a name="to-list-all-record-sets"></a>Για να εμφανίσετε όλα τα σύνολα εγγραφών

Αυτό το παράδειγμα επιστρέφει όλα τα σύνολα εγγραφών, ανεξάρτητα από το όνομα ή τύπος εγγραφής:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>Για να λίστα σύνολα εγγραφών από έναν συγκεκριμένο τύπο εγγραφής

Αυτό το παράδειγμα επιστρέφει όλα τα σύνολα εγγραφών που ταιριάζουν με τον τύπο εγγραφής που δίνεται. Σε αυτήν την περίπτωση, η εγγραφή που σημαίνει ότι επιστρέφονται είναι "Ένα" εγγραφές:

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Στη ζώνη μπορούν να καθοριστούν, χρησιμοποιώντας είτε την *– ZoneName* *– ResourceGroupName* παράμετροι και (όπως φαίνεται) ή καθορίζοντας ένα αντικείμενο ζώνης:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>Προσθήκη εγγραφής σε ένα σύνολο εγγραφών

Προσθήκη εγγραφών για σύνολα εγγραφών με χρήση του `Add-AzureRmDnsRecordConfig` cmdlet. Αυτή είναι μια λειτουργία χωρίς σύνδεση. Έχει αλλάξει μόνο το τοπικό αντικείμενο που αντιπροσωπεύει το σύνολο εγγραφών.

Οι παράμετροι για την προσθήκη εγγραφών σε ένα σύνολο εγγραφών διαφέρουν ανάλογα με τον τύπο του συνόλου εγγραφών. Για παράδειγμα, κατά τη χρήση ενός συνόλου εγγραφών του τύπου "Α", μπορείτε να καθορίσετε μόνο εγγραφές με την παράμετρο *-IPv4Address*.

Πρόσθετες εγγραφές μπορούν να προστεθούν σε κάθε συνόλου εγγραφών με επιπλέον κλήσεων στο `Add-AzureRmDnsRecordConfig`. Μπορείτε να προσθέσετε έως και 20 εγγραφές σε κάθε σύνολο εγγραφών. Ωστόσο, σύνολα εγγραφών του τύπου "CNAME" μπορεί να περιέχει πολύ μία εγγραφή και ένα σύνολο εγγραφών δεν μπορεί να περιέχει δύο πανομοιότυπες εγγραφές. Κενή σύνολα εγγραφών (με μηδέν records) μπορεί να δημιουργηθεί, αλλά δεν εμφανίζονται στο τους διακομιστές ονομάτων του Azure DNS.

Αφού το σύνολο εγγραφών περιέχει στην επιθυμητή συλλογή των εγγραφών, πρέπει να δεσμεύσετε χρησιμοποιώντας το `Set-AzureRmDnsRecordSet` cmdlet. Μετά από ένα σύνολο εγγραφών έχει ολοκληρωθεί, αντικαθιστά την υπάρχουσα εγγραφή ρύθμιση στο Azure DNS.

### <a name="to-create-an-a-record-set-with-a-single-record"></a>Για να δημιουργήσετε μια εγγραφή A που έχουν οριστεί με μία μόνο εγγραφή

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Η σειρά των εργασιών για να δημιουργήσετε μια εγγραφή μπορεί επίσης να *διοχετεύεται*, γεγονός που σημαίνει ότι περάσετε το αντικείμενο συνόλου εγγραφών με χρήση της διοχέτευσης και όχι που περνά μέσα ως παράμετρο. Για παράδειγμα:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>Παραδείγματα επιπλέον τύπο εγγραφής

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>Τροποποίηση υπάρχοντος σύνολα εγγραφών

Τα βήματα για την τροποποίηση ένα υπάρχον σύνολο εγγραφών είναι παρόμοια με τα βήματα που κάνετε κατά τη δημιουργία εγγραφών. Η σειρά των εργασιών είναι ως εξής:

1.  Ανάκτηση στην υπάρχουσα εγγραφή ρυθμίσετε χρησιμοποιώντας `Get-AzureRmDnsRecordSet`.

2.  Τροποποίηση του συνόλου εγγραφών με την προσθήκη εγγραφών, κατάργηση εγγραφών, αλλάζοντας τις παραμέτρους εγγραφή ή αλλαγή της εγγραφής ορίστε time to live (TTL). Αυτή είναι μια λειτουργία χωρίς σύνδεση. Έχει αλλάξει μόνο το τοπικό αντικείμενο που αντιπροσωπεύει το σύνολο εγγραφών.

3.  Ολοκλήρωση τις αλλαγές σας, χρησιμοποιώντας το `Set-AzureRmDnsRecordSet` cmdlet. Αντικαθιστά την υπάρχουσα εγγραφή ρύθμιση στο Azure DNS.


### <a name="to-update-a-record-in-an-existing-record-set"></a>Για να ενημερώσετε μια εγγραφή σε ένα υπάρχον σύνολο εγγραφών

Σε αυτό το παράδειγμα, αλλάξουμε τη διεύθυνση IP του μια υπάρχουσα εγγραφή "A":

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Το `Set-AzureRmDnsRecordSet` cmdlet χρησιμοποιεί etag ελέγχων για να βεβαιωθείτε ότι δεν αντικαθίστανται ταυτόχρονες αλλαγές. Χρησιμοποιήστε το *-Αντικατάσταση* σημαία για να κάνετε απόκρυψη αυτών των ελέγχων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [σχετικά με etags και ετικέτες](dns-getstarted-create-dnszone.md#tagetag).

### <a name="to-modify-an-soa-record"></a>Για να τροποποιήσετε μια εγγραφή SOA

Δεν είναι δυνατό να προσθέσετε ή καταργήσετε εγγραφές από την εγγραφή SOA δημιουργείται αυτόματα ρύθμιση στην κορυφή ζώνη (όνομα = "@"). Ωστόσο, μπορείτε να τροποποιήσετε οποιαδήποτε από τις παραμέτρους εντός της εγγραφής SOA (εκτός από τα "κεντρικός υπολογιστής") και την εγγραφή ρύθμιση TTL.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να αλλάξετε την ιδιότητα *ηλεκτρονικού ταχυδρομείου* της εγγραφής SOA:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>Για να τροποποιήσετε τις εγγραφές NS στην κορυφή ζώνης

Δεν μπορείτε να προσθέσετε, να καταργήσετε ή να τροποποιήσετε τις εγγραφές στο δημιουργείται αυτόματα NS συνόλου εγγραφών στην κορυφή ζώνη (όνομα = "@"). Είναι η μόνη αλλαγή που επιτρέπεται να τροποποιήσετε το σύνολο εγγραφών TTL.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να αλλάξετε την ιδιότητα TTL του συνόλου εγγραφών NS:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>Για να προσθέσετε εγγραφές σε ένα υπάρχον σύνολο εγγραφών

Σε αυτό το παράδειγμα, προσθέσουμε δύο πρόσθετες εγγραφές MX στο υπάρχον σύνολο εγγραφών:

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>Κατάργηση εγγραφής από ένα υπάρχον σύνολο εγγραφών

Εγγραφές μπορούν να καταργηθούν από μια εγγραφή ρυθμίσετε χρησιμοποιώντας `Remove-AzureRmDnsRecordConfig`. Η εγγραφή που καταργείται πρέπει να είναι ακριβή αντιστοιχία με μια υπάρχουσα εγγραφή σε όλες τις παραμέτρους του. Αλλαγές που πρέπει να πραγματοποιηθούν με τη χρήση `Set-AzureRmDnsRecordSet`.

Κατάργηση της τελευταίας εγγραφής από ένα σύνολο εγγραφών δεν διαγράφει το σύνολο εγγραφών. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαγραφή ενός συνόλου εγγραφών](#delete-a-record-set) κάτω από το στοιχείο.


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Η σειρά των εργασιών για να καταργήσετε μια εγγραφή από ένα σύνολο εγγραφών μπορούν επίσης να διοχετευθούν, γεγονός που σημαίνει ότι περάσετε το αντικείμενο συνόλου εγγραφών με χρήση της διοχέτευσης και όχι που περνά μέσα ως παράμετρο. Για παράδειγμα:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Κατάργηση εγγραφής AAAA από ένα σύνολο εγγραφών

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>Κατάργηση εγγραφής CNAME από ένα σύνολο εγγραφών

Ένα σύνολο εγγραφών CNAME μπορεί να περιέχει πολύ μία εγγραφή, καταργώντας αυτήν την εγγραφή αφήνει ένα κενό σύνολο εγγραφών.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>Κατάργηση εγγραφής MX από ένα σύνολο εγγραφών

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>Κατάργηση εγγραφής NS από σύνολο εγγραφών

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>Καταργήστε μια εγγραφή SRV από ένα σύνολο εγγραφών

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>Κατάργηση εγγραφής TXT από ένα σύνολο εγγραφών

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>Διαγράψτε ένα σύνολο εγγραφών

Μπορείτε να διαγράψετε το σύνολα εγγραφών με τη χρήση του `Remove-AzureRmDnsRecordSet` cmdlet. Δεν μπορείτε να διαγράψετε το SOA και εγγραφή NS ορίζει στην κορυφή ζώνη (όνομα = "@") που δημιουργήθηκαν αυτόματα κατά τη δημιουργία της ζώνης. Θα διαγραφούν αυτόματα αν διαγραφεί της ζώνης.

Χρησιμοποιήστε μία από τις ακόλουθες τρεις μεθόδους για να καταργήσετε ένα σύνολο εγγραφών:

### <a name="specify-all-the-parameters-by-name"></a>Καθορίστε όλες τις παραμέτρους κατά όνομα

Προαιρετικό *-ισχύ* διακόπτης μπορεί να χρησιμοποιηθεί για να κάνετε απόκρυψη ερώτηση επιβεβαίωσης.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>Καθορίστε το σύνολο εγγραφών με όνομα και τύπος και καθορίστε τη ζώνη από αντικείμενο

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>Καθορισμός του συνόλου εγγραφών με αντικειμένου

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Κατά τον καθορισμό του συνόλου εγγραφών με χρήση ενός αντικειμένου, επιτρέπει etag ελέγχων για να βεβαιωθείτε ότι δεν διαγράφονται ταυτόχρονες αλλαγές. Προαιρετικό *-Αντικατάσταση* σημαία αποκρύπτει αυτών των ελέγχων. Για περισσότερες πληροφορίες, ανατρέξτε [Etags και ετικέτες](dns-getstarted-create-dnszone.md#tagetag) .

Το αντικείμενο σύνολο εγγραφών μπορούν επίσης να διοχετευθούν αντί για που διαβιβάζεται ως παράμετρο:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με το Azure DNS, ανατρέξτε στο θέμα [Επισκόπηση Azure DNS](dns-overview.md). Για πληροφορίες σχετικά με την αυτοματοποίηση DNS, ανατρέξτε στο θέμα [Δημιουργία DNS ζώνες και εγγραφή συνόλων χρησιμοποιώντας το .NET SDK](dns-sdk.md).

Για περισσότερες πληροφορίες σχετικά με αντίστροφη εγγραφές DNS, δείτε [πώς μπορείτε να διαχειριστείτε αντίστροφη εγγραφές DNS για τις υπηρεσίες σας με χρήση του PowerShell](dns-reverse-dns-record-operations-ps.md).
