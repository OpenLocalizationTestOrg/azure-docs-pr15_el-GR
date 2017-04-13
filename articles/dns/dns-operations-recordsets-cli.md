<properties
   pageTitle="Διαχείριση σύνολα εγγραφών DNS και εγγραφές στην DNS Azure χρησιμοποιώντας Azure CLI | Microsoft Azure"
   description="Διαχείριση σύνολα εγγραφών DNS και εγγραφές σε Azure DNS όταν φιλοξενεί τον τομέα σας στο Azure DNS. Όλες οι εντολές CLI για λειτουργίες σε σύνολα εγγραφών και εγγραφών."
   services="dns"
   documentationCenter="na"
   authors="jtuliani"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>Διαχειριστείτε τις εγγραφές DNS και σύνολα εγγραφών με τη χρήση CLI


> [AZURE.SELECTOR]
- [Πύλη του Azure](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)


Αυτό το άρθρο σας δείχνει πώς μπορείτε να διαχειριστείτε τα σύνολα εγγραφών και τις εγγραφές για τη ζώνη DNS χρησιμοποιώντας πλατφόρμες Azure περιβάλλον γραμμής εντολών του (CLI).

Είναι σημαντικό να κατανοήσετε τη διαφορά μεταξύ σύνολα εγγραφών DNS και μεμονωμένες εγγραφές DNS. Ένα σύνολο εγγραφών είναι μια συλλογή εγγραφών σε μια ζώνη που έχουν το ίδιο όνομα και είναι του ίδιου τύπου. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Κατανόηση των σύνολα εγγραφών και εγγραφών](dns-getstarted-create-recordset-cli.md).


## <a name="configure-the-cross-platform-azure-cli"></a>Ρύθμιση παραμέτρων του CLI Azure πλατφόρμες

Azure DNS είναι μια Azure υπηρεσία μόνο από διαχειριστή πόρων. Να μην έχει API Azure υπηρεσία διαχείρισης. Βεβαιωθείτε ότι το CLI Azure έχει ρυθμιστεί ώστε να χρησιμοποιήσετε τη λειτουργία διαχείρισης πόρων, με τη χρήση του `azure config mode arm` εντολή.

Εάν βλέπετε **σφάλματος: 'dns' δεν είναι μια εντολή azure**, το πιθανότερο είναι επειδή χρησιμοποιείτε Azure CLI στη Διαχείριση υπηρεσίας Azure λειτουργία, δεν βρίσκεστε σε λειτουργία διαχείρισης πόρων.

## <a name="create-a-new-record-set-and-record"></a>Δημιουργήστε ένα νέο σύνολο εγγραφών και εγγραφής

Για να δημιουργήσετε μια νέα εγγραφή, ορίστε στην πύλη του Azure, ανατρέξτε στο θέμα [Δημιουργία ενός συνόλου εγγραφών και εγγραφών](dns-getstarted-create-recordset-cli.md).


## <a name="retrieve-a-record-set"></a>Ανακτήστε ένα σύνολο εγγραφών

Για να ανακτήσετε ένα υπάρχον σύνολο εγγραφών, χρησιμοποιήστε `azure network dns record-set show`. Καθορίστε την ομάδα πόρων, όνομα ζώνης, εγγραφή Ορισμός σχετικό όνομα και τύπο εγγραφής. Χρησιμοποιήστε το παρακάτω παράδειγμα, αντικαθιστώντας τις τιμές με το δικό σας.

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>Λίστα σύνολα εγγραφών

Μπορείτε να εμφανίσετε όλες τις εγγραφές σε μια ζώνη DNS, χρησιμοποιώντας το `azure network dns record-set list` εντολή. Πρέπει να καθορίσετε το όνομα της ομάδας πόρων και όνομα ζώνης.

### <a name="to-list-all-record-sets"></a>Για να εμφανίσετε όλα τα σύνολα εγγραφών

Αυτό το παράδειγμα επιστρέφει όλα τα σύνολα εγγραφών, ανεξάρτητα από το όνομα ή τύπος εγγραφής:

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>Για να σύνολα εγγραφών λίστα ενός συγκεκριμένου τύπου

Αυτό το παράδειγμα επιστρέφει όλα τα σύνολα εγγραφών που ταιριάζουν με τον τύπο εγγραφής που δίνεται (σε αυτήν την περίπτωση, "Ένα" εγγραφές):

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>Προσθήκη εγγραφής σε ένα σύνολο εγγραφών

Προσθήκη εγγραφών για σύνολα εγγραφών με χρήση του `azure network dns record-set add-record`εντολή. Οι παράμετροι για την προσθήκη εγγραφών σε ένα σύνολο εγγραφών διαφέρουν ανάλογα με τον τύπο της εγγραφής που ορίζεται. Για παράδειγμα, όταν χρησιμοποιείτε ένα σύνολο εγγραφών του τύπου "Α", μπορείτε να καθορίσετε μόνο εγγραφές με την παράμετρο `-a <IPv4 address>`.

Για να δημιουργήσετε ένα σύνολο εγγραφών, χρησιμοποιήστε το `azure network dns record-set create`εντολή. Καθορίστε την ομάδα πόρων, όνομα ζώνης, εγγραφή ορίστε σχετικό όνομα, τύπο εγγραφής και ώρα για να live (TTL). Εάν το `--ttl` παράμετρος δεν έχει οριστεί, η προεπιλεγμένη τιμή (σε δευτερόλεπτα) είναι τέσσερις.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Αφού δημιουργήσετε το "Ένα" σύνολο εγγραφών, προσθέστε τη διεύθυνση IPv4 χρησιμοποιώντας το `azure network dns record-set add-record`εντολή.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να δημιουργήσετε ένα σύνολο εγγραφών από κάθε τύπο εγγραφής. Κάθε σύνολο εγγραφών περιέχει μία εγγραφή.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>Ενημέρωση μιας εγγραφής σε ένα σύνολο εγγραφών

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>Για να προσθέσετε μια άλλη διεύθυνση IP (1.2.3.4) σε μια υπάρχουσα εγγραφή "A" Ορισμός ("www"):

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>Για να καταργήσετε μια υπάρχουσα τιμή από ένα σύνολο εγγραφών
Χρήση `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>Κατάργηση εγγραφής από ένα σύνολο εγγραφών

Εγγραφές μπορούν να καταργηθούν από μια εγγραφή ρυθμίσετε χρησιμοποιώντας `azure network dns record-set delete-record`. Η εγγραφή που καταργείται πρέπει να είναι ακριβή αντιστοιχία με μια υπάρχουσα εγγραφή σε όλες τις παραμέτρους του.

Κατάργηση της τελευταίας εγγραφής από ένα σύνολο εγγραφών δεν διαγράφει το σύνολο εγγραφών. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα αυτού του άρθρου που ονομάζεται [Διαγραφή ενός συνόλου εγγραφών](#delete).

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Κατάργηση εγγραφής AAAA από ένα σύνολο εγγραφών

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>Κατάργηση εγγραφής CNAME από ένα σύνολο εγγραφών

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>Κατάργηση εγγραφής MX από ένα σύνολο εγγραφών

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>Κατάργηση εγγραφής NS από σύνολο εγγραφών

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>Κατάργηση εγγραφής PTR από ένα σύνολο εγγραφών
Σε αυτήν την περίπτωση ' μου-arpa-τοποθεσία zone.com' αντιπροσωπεύει τη ζώνη ARPA που αντιπροσωπεύει την περιοχή διευθύνσεων IP.  Κάθε εγγραφή PTR ρύθμιση σε αυτήν τη ζώνη αντιστοιχεί σε μια διεύθυνση IP μέσα σε αυτήν την περιοχή διευθύνσεων IP.

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>Καταργήστε μια εγγραφή SRV από ένα σύνολο εγγραφών

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>Κατάργηση εγγραφής TXT από ένα σύνολο εγγραφών

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>Διαγράψτε ένα σύνολο εγγραφών

Μπορείτε να διαγράψετε το σύνολα εγγραφών με τη χρήση του `Remove-AzureRmDnsRecordSet` cmdlet. Δεν μπορείτε να διαγράψετε το SOA και εγγραφή NS ορίζει στην κορυφή ζώνη (όνομα = "@") που δημιουργήθηκαν αυτόματα κατά τη δημιουργία της ζώνης. Θα διαγραφούν αυτόματα αν διαγραφεί της ζώνης.

Στο παρακάτω παράδειγμα, το "Α" σύνολο "δοκιμή-a" εγγραφών θα καταργηθεί από τη ζώνη DNS "contoso.com":

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

Ο διακόπτης προαιρετικά *-q* μπορεί να χρησιμοποιηθεί για να κάνετε απόκρυψη ερώτηση επιβεβαίωσης.


## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με το Azure DNS, ανατρέξτε στο θέμα [Επισκόπηση Azure DNS](dns-overview.md). Για πληροφορίες σχετικά με την αυτοματοποίηση DNS, ανατρέξτε στο θέμα [Δημιουργία DNS ζώνες και εγγραφή συνόλων χρησιμοποιώντας το .NET SDK](dns-sdk.md).

Εάν θέλετε να εργαστείτε με αντίστροφη τις εγγραφές DNS, ανατρέξτε στο θέμα [πώς μπορείτε να διαχειριστείτε αντίστροφη εγγραφές DNS για τις υπηρεσίες σας χρησιμοποιώντας το CLI Azure](dns-reverse-dns-record-operations-cli.md).
