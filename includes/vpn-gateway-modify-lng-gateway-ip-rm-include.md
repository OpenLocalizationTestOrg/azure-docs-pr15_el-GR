Για να τροποποιήσετε τη διεύθυνση IP πύλης, χρησιμοποιήστε το `New-AzureRmVirtualNetworkGatewayConnection` cmdlet. Με την προϋπόθεση ότι διατηρείτε ακριβώς το ίδιο με το υπάρχον όνομα το όνομα της πύλης τοπικό δίκτυο, θα αντικαταστήσει τις ρυθμίσεις. Προς το παρόν, το cmdlet "Ορισμός" δεν υποστηρίζει τροποποίηση διεύθυνση IP της πύλης.

### <a name="gwipnoconnection"></a>Πώς μπορείτε να τροποποιήσετε τη διεύθυνση IP πύλης - δεν υπάρχει σύνδεση πύλης

Για να ενημερώσετε τη διεύθυνση IP πύλης για την πύλη τοπικό δίκτυο που δεν έχει ακόμη μια σύνδεση, χρησιμοποιήστε το παρακάτω παράδειγμα. Μπορείτε επίσης να ενημερώσετε τα προθέματα διευθύνσεων την ίδια στιγμή. Οι ρυθμίσεις που καθορίζετε θα αντικαταστήσει τις υπάρχουσες ρυθμίσεις. Φροντίστε να χρησιμοποιήσετε το υπάρχον όνομα της πύλης του τοπικού δικτύου. Εάν δεν το χρησιμοποιείτε, θα δημιουργήσετε μια νέα πύλη τοπικό δίκτυο, δεν αντικατάσταση της υπάρχουσας.

Χρησιμοποιήστε το παρακάτω παράδειγμα, αντικαθιστώντας τις τιμές για τη δική σας.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Πώς μπορείτε να τροποποιήσετε τη διεύθυνση IP πύλης - υπάρχουσα σύνδεση πύλης

Εάν υπάρχει ήδη μια σύνδεση πύλης, θα πρέπει πρώτα να καταργήσετε τη σύνδεση. Στη συνέχεια, μπορείτε να τροποποιήσετε τη διεύθυνση IP πύλης και δημιουργήστε ξανά μια νέα σύνδεση. Αυτό θα έχει ως αποτέλεσμα ορισμένες χρόνου εκτός λειτουργίας για τη σύνδεση VPN.


>[AZURE.IMPORTANT] Μην διαγράφετε την πύλη VPN. Εάν το κάνετε αυτό, θα πρέπει να επιστρέψετε τα βήματα για να δημιουργήσετε ξανά, καθώς και να ρυθμίσει ξανά τις παραμέτρους του δρομολογητή εσωτερικής εγκατάστασης με τη διεύθυνση IP που εκχωρούνται για την πύλη που έχουν δημιουργηθεί πρόσφατα.
 

1. Για να καταργήσετε τη σύνδεση. Μπορείτε να βρείτε το όνομα της σύνδεσής σας με τη χρήση του `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. Τροποποιήστε την τιμή GatewayIpAddress. Μπορείτε, επίσης, να τροποποιήσετε το προθέματα διεύθυνση αυτήν τη στιγμή, εάν είναι απαραίτητο. Σημειώστε ότι αυτή η ενέργεια θα αντικαταστήσει τις υπάρχουσες ρυθμίσεις τοπικού δικτύου πύλης. Χρησιμοποιήστε το υπάρχον όνομα της πύλης σας τοπικό δίκτυο όταν τροποποιείτε, έτσι ώστε οι ρυθμίσεις θα αντικαταστήσει. Εάν δεν το χρησιμοποιείτε, θα δημιουργήσετε μια νέα πύλη τοπικό δίκτυο, μην τροποποιείτε ένα υπάρχον.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. Δημιουργία της σύνδεσης. Σε αυτό το παράδειγμα, θα σας θα ρυθμίσετε τις παραμέτρους μιας τύπος σύνδεσης ασφαλείας IP. Όταν αναδημιουργήσετε τη σύνδεσή σας, χρησιμοποιήστε τον τύπο σύνδεσης που καθορίζεται για τη ρύθμιση παραμέτρων. Για πρόσθετη σύνδεση τύπους, ανατρέξτε στη σελίδα [cmdlet του PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) .  Για να αποκτήσετε το όνομα VirtualNetworkGateway, μπορείτε να εκτελέσετε το `Get-AzureRmVirtualNetworkGateway` cmdlet.

    Ορίστε τις μεταβλητές:

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    Δημιουργία της σύνδεσης:
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

