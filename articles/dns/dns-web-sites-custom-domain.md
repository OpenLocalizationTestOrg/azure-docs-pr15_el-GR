<properties
   pageTitle="Δημιουργία προσαρμοσμένων εγγραφών DNS για μια εφαρμογή web | Microsoft Azure  "
   description="Τρόπος δημιουργίας προσαρμοσμένου τομέα εγγραφές DNS για το web app χρησιμοποιώντας Azure DNS."
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

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Δημιουργία εγγραφών DNS για μια εφαρμογή web σε έναν προσαρμοσμένο τομέα

Μπορείτε να χρησιμοποιήσετε Azure DNS για τη φιλοξενία του προσαρμοσμένου τομέα για τις εφαρμογές web. Για παράδειγμα, δημιουργείτε μια εφαρμογή Azure web και θέλετε οι χρήστες σας για να έχετε πρόσβαση σε αυτά, είτε με τη χρήση contoso.com ή www.contoso.com ως ένα FQDN.

Για να το κάνετε αυτό, πρέπει να δημιουργήσετε δύο εγγραφές:

- Μια εγγραφή ρίζας "Α" που δείχνει σε contoso.com
- Μια εγγραφή "CNAME" για το όνομα www που οδηγεί σε μια εγγραφή A

Λάβετε υπόψη ότι εάν δημιουργήσετε μια εγγραφή A για μια εφαρμογή web στο Azure, της εγγραφής A πρέπει να είναι με μη αυτόματο τρόπο ενημερώνονται εάν η υποκείμενη διεύθυνση IP για τις αλλαγές της εφαρμογής web.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Προτού ξεκινήσετε, πρέπει πρώτα να δημιουργήσετε μια ζώνη DNS σε Azure DNS και πληρεξουσίου τη ζώνη στο μητρώο καταχώρησης σε Azure DNS.

1. Για να δημιουργήσετε μια ζώνη DNS, ακολουθήστε τα βήματα στο θέμα [Δημιουργία μιας ζώνης DNS](dns-getstarted-create-dnszone.md).
2. Για να αναθέσετε DNS σε Azure DNS, ακολουθήστε τα βήματα στην [ανάθεση τομέα DNS](dns-domain-delegation.md).

Αφού δημιουργήσετε μια ζώνη και την ανάθεση σε Azure DNS, μπορείτε να δημιουργήσετε τις εγγραφές, στη συνέχεια, για τον προσαρμοσμένο τομέα.


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Δημιουργήστε μια εγγραφή A για τον προσαρμοσμένο τομέα σας

Μια εγγραφή A χρησιμοποιείται για να αντιστοιχίσετε ένα όνομα για τη διεύθυνση IP. Στο παρακάτω παράδειγμα θα σας θα εκχωρήσετε @ ως μια εγγραφή A σε μια διεύθυνση IPv4:

### <a name="step-1"></a>Βήμα 1

Δημιουργήστε μια εγγραφή A και να εκχωρήσετε σε μια μεταβλητή $rs

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>Βήμα 2

Προσθέστε την τιμή IPv4 του συνόλου εγγραφών που δημιουργήσατε προηγουμένως "@" χρησιμοποιώντας τη μεταβλητή $rs στους οποίους έχουν ανατεθεί. Η τιμή IPv4 στους οποίους έχουν ανατεθεί θα είναι η διεύθυνση IP για την εφαρμογή web.

Για να βρείτε τη διεύθυνση IP για μια εφαρμογή web, ακολουθήστε τα βήματα στο θέμα [Ρύθμιση παραμέτρων ενός προσαρμοσμένου ονόματος τομέα στο Azure εφαρμογής υπηρεσίας](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address).

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>Βήμα 3

Αποθηκεύστε τις αλλαγές στο σύνολο εγγραφών. Χρήση `Set-AzureRMDnsRecordSet` για να αποστείλετε τις αλλαγές στην εγγραφή που έχει οριστεί σε Azure DNS:

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Δημιουργήστε μια εγγραφή CNAME για τον προσαρμοσμένο τομέα σας

Εάν τον τομέα σας γίνεται ήδη από Azure DNS ( [ανάθεση τομέα DNS](dns-domain-delegation.md), ανατρέξτε στο θέμα που μπορείτε να χρησιμοποιήσετε το ακόλουθο παράδειγμα για να δημιουργήσετε μια εγγραφή CNAME για contoso.azurewebsites.net.

### <a name="step-1"></a>Βήμα 1

Ανοίξτε PowerShell και δημιουργήστε ένα νέο σύνολο εγγραφών CNAME και εκχωρήστε σε μεταβλητή $rs. Αυτό το παράδειγμα θα δημιουργήσετε έναν τύπο σύνολο εγγραφών CNAME με ένα "διάρκεια ζωής" 600 δευτερολέπτων στη ζώνη DNS με το όνομα "contoso.com".

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Βήμα 2

Αφού δημιουργηθεί το σύνολο εγγραφών CNAME, πρέπει να δημιουργήσετε ένα ψευδώνυμο τιμή η οποία θα οδηγεί στην εφαρμογή web.

Χρήση της μεταβλητής που έχουν εκχωρηθεί προηγουμένως "$rs" μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή PowerShell για να δημιουργήσετε το ψευδώνυμο για το contoso.azurewebsites.net εφαρμογής web.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Βήμα 3

Αποθηκεύστε τις αλλαγές με χρήση του `Set-AzureRMDnsRecordSet` cmdlet:

    Set-AzureRMDnsRecordSet -RecordSet $rs

Μπορείτε να επικυρώσετε την εγγραφή που δημιουργήθηκε σωστά κατά την υποβολή ερωτημάτων το "www.contoso.com" Χρήση της εντολής nslookup, όπως φαίνεται παρακάτω:

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>Δημιουργία εγγραφής "awverify" για τις εφαρμογές web


Εάν αποφασίσετε να χρησιμοποιήσετε μια εγγραφή A για την εφαρμογή web σας, πρέπει να ακολουθήσετε μια διαδικασία επαλήθευσης για να βεβαιωθείτε ότι είστε κάτοχος του προσαρμοσμένου τομέα. Αυτό το βήμα επαλήθευσης γίνεται, δημιουργώντας μια ειδική εγγραφή CNAME με το όνομα "awverify". Αυτή η ενότητα ισχύει για ένα μόνο εγγραφών.


### <a name="step-1"></a>Βήμα 1

Δημιουργήστε την εγγραφή "awverify". Στο παρακάτω παράδειγμα, θα δημιουργήσουμε την εγγραφή "aweverify" για contoso.com για επαλήθευση της κατοχής του προσαρμοσμένου τομέα.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Βήμα 2

Αφού δημιουργηθεί το σύνολο εγγραφών "awverify", αντιστοιχίστε την εγγραφή CNAME Ορισμός ψευδώνυμο. Στο παρακάτω παράδειγμα, η Microsoft θα εκχωρήσει την εγγραφή CNAMe οριστεί ψευδώνυμο awverify.contoso.azurewebsites.net.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Βήμα 3

Αποθηκεύστε τις αλλαγές με χρήση του `Set-AzureRMDnsRecordSet cmdlet`, όπως φαίνεται στην παρακάτω εντολή.

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>Επόμενα βήματα

Ακολουθήστε τα βήματα στο θέμα [ρύθμιση των παραμέτρων ενός προσαρμοσμένου ονόματος τομέα για την υπηρεσία εφαρμογής](../app-service-web/web-sites-custom-domain-name.md) για να ρυθμίσετε τις παραμέτρους του web app για να χρησιμοποιήσετε έναν προσαρμοσμένο τομέα.








