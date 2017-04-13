<properties
   pageTitle="Διαχείριση ζώνες DNS χρησιμοποιώντας το PowerShell | Microsoft Azure"
   description="Μπορείτε να διαχειριστείτε ζώνες DNS με χρήση του Powershell Azure. Πώς μπορείτε να ενημερώσετε, να διαγράψετε και να δημιουργήσετε ζώνες DNS σε Azure DNS"
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

# <a name="how-to-manage-dns-zones-using-powershell"></a>Πώς μπορείτε να διαχειριστείτε ζώνες DNS με χρήση του PowerShell

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-dnszones-cli.md)
- [PowerShell](dns-operations-dnszones.md)



Σε αυτό το άρθρο θα σας δείξουν πώς μπορείτε να διαχειριστείτε τη ζώνη DNS με χρήση του PowerShell. Για να χρησιμοποιήσετε αυτά τα βήματα, θα πρέπει να εγκαταστήσετε την πιο πρόσφατη έκδοση του τα cmdlet του PowerShell για τη διαχείριση πόρων Azure (1,0 ή νεότερη έκδοση). Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .


## <a name="create-a-new-dns-zone"></a>Δημιουργήστε μια νέα ζώνη DNS

Για να δημιουργήσετε μια ζώνη DNS, ανατρέξτε στο θέμα [Δημιουργία μιας ζώνης DNS με χρήση του PowerShell](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>Λάβετε μια ζώνη DNS

Για να ανακτήσετε μια ζώνη DNS, χρησιμοποιήστε το `Get-AzureRmDnsZone` cmdlet. Αυτή η λειτουργία επιστρέφει ένα αντικείμενο ζώνης DNS που αντιστοιχεί σε μια υπάρχουσα ζώνη στο Azure DNS. Το αντικείμενο περιέχει δεδομένα σχετικά με τη ζώνη (όπως ο αριθμός των σύνολα εγγραφών), αλλά δεν περιέχει τα σύνολα εγγραφή τον εαυτό τους.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>Λίστα ζώνες DNS

Παραλείποντας το όνομα της ζώνης από `Get-AzureRmDnsZone`, μπορείτε να απαριθμήσετε όλες τις ζώνες σε μια ομάδα πόρων. Αυτή η λειτουργία επιστρέφει έναν πίνακα ζώνη αντικειμένων.

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>Ενημέρωση μιας ζώνης DNS

Αλλαγές σε έναν πόρο ζώνης DNS μπορούν να γίνουν με τη χρήση `Set-AzureRmDnsZone`. Αυτό δεν ενημερώνουν κανένα από τα σύνολα εγγραφών DNS στο εσωτερικό της ζώνης (ανατρέξτε στο θέμα [πώς μπορείτε να τις εγγραφές DNS Διαχείριση](dns-operations-recordsets.md)). Χρησιμοποιείται μόνο για την ενημέρωση ιδιοτήτων του πόρου ζώνη ίδια. Αυτό περιορίζεται αυτήν τη στιγμή για τη διαχείριση πόρων Azure 'ετικέτες' για τον πόρο ζώνη. Για περισσότερες πληροφορίες, ανατρέξτε [Etags και ετικέτες](dns-getstarted-create-dnszone.md#Etags-and-tags) .

Χρησιμοποιήστε ένα από τα εξής δύο τρόπους για την ενημέρωση ζώνης DNS:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Καθορίστε τη ζώνη με την ομάδα όνομα και πόρων ζώνης

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>Καθορίστε τη ζώνη χρησιμοποιώντας ένα αντικείμενο $zone

Καθορίστε τη ζώνη χρησιμοποιώντας ένα αντικείμενο $zone από `Get-AzureRmDnsZone`. Κατά τη χρήση `Set-AzureRmDnsZone` με ένα αντικείμενο $zone, θα χρησιμοποιηθεί Etag ελέγχων για να βεβαιωθείτε ότι δεν αντικαθίστανται ταυτόχρονες αλλαγές. Μπορείτε να χρησιμοποιήσετε την προαιρετική *-Αντικατάσταση* διακόπτη για να κάνετε απόκρυψη αυτών των ελέγχων. Για περισσότερες πληροφορίες, ανατρέξτε [Etags και ετικέτες](dns-getstarted-create-dnszone.md#Etags-and-tags) .


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>Διαγραφή μιας ζώνης DNS

Ζώνες DNS μπορούν να διαγραφούν χρησιμοποιώντας το cmdlet κατάργηση AzureRmDnsZone.

Πριν από τη διαγραφή μιας ζώνης DNS σε Azure DNS, θα πρέπει να διαγράψετε όλα τα σύνολα εγγραφών, με εξαίρεση τις εγγραφές NS και SOA στη ρίζα της ζώνης που δημιουργήθηκαν αυτόματα κατά τη δημιουργία της ζώνης.

Χρησιμοποιήστε έναν από τους εξής δύο τρόπους για να καταργήσετε μια ζώνη DNS:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Καθορίστε τη ζώνη χρησιμοποιώντας το όνομα ζώνης και το όνομα της ομάδας πόρων

Αυτή η λειτουργία έχει μια προαιρετική *-ισχύ* διακόπτη που αποκρύπτει σας ζητηθεί να επιβεβαιώσετε ότι θέλετε να καταργήσετε τη ζώνη DNS.

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>Καθορίστε τη ζώνη χρησιμοποιώντας ένα αντικείμενο $zone

Καθορίστε τη ζώνη χρησιμοποιώντας ένα αντικείμενο $zone από `Get-AzureRmDnsZone`. Αυτή η λειτουργία έχει μια προαιρετική *-ισχύ* διακόπτη που αποκρύπτει σας ζητηθεί να επιβεβαιώσετε ότι θέλετε να καταργήσετε τη ζώνη DNS. Όπως και με `Set-AzureRmDnsZone`, που καθορίζει τη ζώνη χρησιμοποιώντας ένα αντικείμενο $zone επιτρέπει Etag ελέγχων για να βεβαιωθείτε ότι δεν διαγράφονται ταυτόχρονες αλλαγές. <BR>
Προαιρετικό *-Αντικατάσταση* σημαία αποκρύπτει αυτών των ελέγχων. Για περισσότερες πληροφορίες, ανατρέξτε [Etags και ετικέτες](dns-getstarted-create-dnszone.md#Etags-and-tags) .

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



Το αντικείμενο ζώνη μπορούν επίσης να διοχετευθούν αντί για που διαβιβάζεται ως παράμετρο:

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>Επόμενα βήματα

Αφού δημιουργήσετε μια ζώνη DNS, δημιουργήστε [σύνολα εγγραφής και εγγραφές](dns-getstarted-create-recordset.md) για να ξεκινήσετε επίλυση ονομάτων για τον τομέα σας στο Internet.