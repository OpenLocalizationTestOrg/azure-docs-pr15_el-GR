<properties
   pageTitle="Δημιουργήστε ένα σύνολο εγγραφών και εγγραφών για μια ζώνη DNS χρησιμοποιώντας το PowerShell | Microsoft Azure"
   description="Πώς να δημιουργήσετε εγγραφές κεντρικού υπολογιστή για το Azure DNS. Ρύθμιση εγγραφής σύνολα και εγγραφών με χρήση του PowerShell"
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



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>Δημιουργήστε σύνολα εγγραφών DNS και εγγραφές με χρήση του PowerShell


> [AZURE.SELECTOR]
- [Πύλη του Azure](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure CLI](dns-getstarted-create-recordset-cli.md)

Σε αυτό το άρθρο σάς καθοδηγεί στη διαδικασία δημιουργίας εγγραφών και τα σύνολα εγγραφών με χρήση του Windows PowerShell. Μετά τη δημιουργία της ζώνης DNS, μπορείτε να προσθέσετε τις εγγραφές DNS για τον τομέα σας. Για να κάνετε αυτό, πρέπει πρώτα να κατανοήσετε τις εγγραφές DNS και σύνολα εγγραφών.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Βεβαιωθείτε ότι έχετε εγκαταστήσει την πιο πρόσφατη έκδοση του PowerShell

Βεβαιωθείτε ότι έχετε εγκαταστήσει την πιο πρόσφατη έκδοση του τα cmdlet του PowerShell για τη διαχείριση πόρων Azure. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .

## <a name="create-a-record-set-and-record"></a>Δημιουργήστε ένα σύνολο εγγραφών και εγγραφής

Αυτή η ενότητα περιγράφει πώς μπορείτε να δημιουργήσετε ένα σύνολο εγγραφών και εγγραφή.


### <a name="1-connect-to-your-subscription"></a>1. συνδεθείτε με τη συνδρομή σας

Ανοίξτε την κονσόλα του PowerShell και συνδεθείτε στο λογαριασμό σας. Χρησιμοποιήστε το παρακάτω παράδειγμα, για να σας βοηθήσει να συνδεθείτε:

    Login-AzureRmAccount

Ελέγξτε τις συνδρομές για το λογαριασμό.

    Get-AzureRmSubscription

Καθορίστε τη συνδρομή στην οποία θέλετε να χρησιμοποιήσετε.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

Για περισσότερες πληροφορίες σχετικά με την εργασία με το PowerShell, ανατρέξτε στο θέμα [Χρήση του Windows PowerShell με το διαχειριστή πόρων](../powershell-azure-resource-manager.md).


### <a name="2-create-a-record-set"></a>2. Δημιουργήστε ένα σύνολο εγγραφών

Μπορείτε να δημιουργήσετε σύνολα εγγραφών με χρήση του `New-AzureRmDnsRecordSet` cmdlet. Όταν δημιουργείτε ένα σύνολο εγγραφών, πρέπει να καθορίσετε την εγγραφή Ορισμός ονόματος, της ζώνης, ο χρόνος ζωής (TTL) και τον τύπο εγγραφής.

Για να δημιουργήσετε μια εγγραφή ρύθμιση στην κορυφή της ζώνης (σε αυτήν την περίπτωση, "contoso.com"), χρησιμοποιήστε το όνομα της καρτέλας "@", συμπεριλαμβανομένων των εισαγωγικών. Αυτή είναι μια συνήθη σύμβαση DNS.

Το παρακάτω παράδειγμα δημιουργεί μια εγγραφή σύνολο με το σχετικό όνομα "www" στη ζώνη DNS "contoso.com". Το πλήρως προσδιορισμένο όνομα του συνόλου εγγραφών είναι "www.contoso.com". Ο τύπος της εγγραφής είναι "Α", και την τιμή TTL είναι 60 δευτερόλεπτα. Αφού ολοκληρώσετε αυτό το βήμα, θα έχετε ένα σύνολο καρτελών κενή "www" στην οποία έχει ανατεθεί η μεταβλητή *$rs*.

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>Αν μια εγγραφή που ήδη υπάρχει

Εάν μια εγγραφή που ήδη υπάρχει, η εντολή αποτυγχάνει, εκτός εάν το *-Αντικατάσταση* διακόπτης χρησιμοποιείται. Το *-Αντικατάσταση* επιλογής ενεργοποιεί μια ερώτηση επιβεβαίωσης, το οποίο μπορούν να αποκρύπτονται, χρησιμοποιώντας το *-ισχύ* εναλλαγή.


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


Σε αυτό το παράδειγμα, μπορείτε να καθορίσετε τη ζώνη χρησιμοποιώντας το όνομα της ζώνης και το όνομα της ομάδας πόρων. Εναλλακτικά, μπορείτε να καθορίσετε ένα αντικείμενο ζώνη, όπως επιστρέφονται από `Get-AzureRmDnsZone` ή `New-AzureRmDnsZone`.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`Επιστρέφει ένα τοπικό αντικείμενο που αντιπροσωπεύει το σύνολο εγγραφών που δημιουργήθηκε σε Azure DNS.

### <a name="3-add-a-record"></a>3. Προσθήκη μιας εγγραφής

Για να χρησιμοποιήσετε το σύνολο εγγραφών που έχουν δημιουργηθεί πρόσφατα "www", πρέπει να προσθέσετε εγγραφές σε αυτήν. Μπορείτε να προσθέσετε IPv4 ορίστε *A* εγγραφές σε μια εγγραφή "www", χρησιμοποιώντας το παρακάτω παράδειγμα. Αυτό το παράδειγμα βασίζεται σε μεταβλητό *$rs* που έχετε ορίσει στο προηγούμενο βήμα.

Προσθήκη εγγραφών σε μια εγγραφή ρυθμίσετε χρησιμοποιώντας `Add-AzureRmDnsRecordConfig` είναι μια λειτουργία χωρίς σύνδεση. Μόνο την τοπική μεταβλητή *$rs* είναι ενημερωθεί.


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4. Αποθηκεύστε τις αλλαγές

Αποθηκεύστε τις αλλαγές στο σύνολο εγγραφών. Χρήση `Set-AzureRmDnsRecordSet` για να αποστείλετε τις αλλαγές στην εγγραφή που έχει οριστεί σε Azure DNS.

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5. ανάκτηση συνόλου εγγραφών

Μπορείτε να ανακτήσετε του συνόλου εγγραφών από το Azure DNS με χρήση `Get-AzureRmDnsRecordSet` όπως φαίνεται στο παρακάτω παράδειγμα.


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


Μπορείτε επίσης να χρησιμοποιήσετε το εργαλείο nslookup ή άλλα εργαλεία DNS ερώτημα για το νέο σύνολο εγγραφών.

Εάν δεν έχετε αναθέσει ακόμη τον τομέα για να τους διακομιστές ονομάτων Azure DNS, πρέπει να καθορίσετε το όνομα του διακομιστή και διεύθυνση για τη ζώνη.


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Δημιουργία ενός συνόλου εγγραφών κάθε τύπο με μία μόνο εγγραφή


Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να δημιουργήσετε ένα σύνολο εγγραφών από κάθε τύπο εγγραφής. Κάθε σύνολο εγγραφών περιέχει μία εγγραφή.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>Επόμενα βήματα

[Πώς μπορείτε να διαχειριστείτε ζώνες DNS με χρήση του PowerShell](dns-operations-dnszones.md)

[Διαχειριστείτε τις εγγραφές DNS και σύνολα εγγραφών με χρήση του PowerShell](dns-operations-recordsets.md)

[Αυτοματοποίηση Azure λειτουργιών με .NET SDK](dns-sdk.md)
