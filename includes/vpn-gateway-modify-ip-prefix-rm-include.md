### <a name="noconnection"></a>Πώς μπορείτε να προσθέσετε ή να καταργήσετε προθέματα - δεν υπάρχει σύνδεση πύλης

- **Για να προσθέσετε** επιπλέον διεύθυνση προθέματα σε τοπική δικτύου πύλης που δημιουργήσατε, αλλά που δεν διαθέτει ακόμη μια σύνδεση πύλης, χρησιμοποιήστε το παρακάτω παράδειγμα. Φροντίστε να αλλάξετε τις τιμές στο δικό σας.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- **Για να καταργήσετε** ένα πρόθεμα διεύθυνσης από μια πύλη τοπικό δίκτυο που δεν έχει μια σύνδεση VPN, χρησιμοποιήστε το παρακάτω παράδειγμα. Αφήστε ανάληψη τα προθέματα που δεν χρειάζεστε πλέον. Σε αυτό το παράδειγμα, θα σας δεν είναι πλέον πρέπει πρόθεμα 20.0.0.0/24 (από το προηγούμενο παράδειγμα), έτσι θα ενημερώσουμε στον τοπικό δίκτυο πύλης και να αποκλείσετε αυτό το πρόθεμα.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Πώς μπορείτε να προσθέσετε ή να καταργήσετε προθέματα - υπάρχουσα σύνδεση πύλης

Εάν έχετε δημιουργήσει τη σύνδεση πύλης και θέλετε να προσθέσετε ή να καταργήσετε τα προθέματα διεύθυνση IP που περιέχονται στην πύλη του τοπικού δικτύου σας, θα χρειαστεί να κάνετε τα παρακάτω βήματα με τη σειρά. Αυτό θα έχει ως αποτέλεσμα ορισμένες χρόνου εκτός λειτουργίας για τη σύνδεση VPN. Κατά την ενημέρωση του προθέματα, θα πρώτα να καταργήσετε τη σύνδεση, τροποποιήστε τα προθέματα, και, στη συνέχεια, δημιουργήστε μια νέα σύνδεση. Στα παρακάτω παραδείγματα, θα πρέπει να αλλάξετε τις τιμές στο δικό σας.

>[AZURE.IMPORTANT] Μην διαγράφετε την πύλη VPN. Εάν κάνετε αυτό, θα πρέπει να επιστρέψετε τα βήματα για να δημιουργήσετε ξανά, καθώς και να ρυθμίσει ξανά τις παραμέτρους του δρομολογητή εσωτερικής εγκατάστασης με τις νέες ρυθμίσεις.
 
1. Για να καταργήσετε τη σύνδεση.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Τροποποιήστε τα προθέματα διεύθυνση για την πύλη τοπικού δικτύου.

    Ορίστε τη μεταβλητή για το LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Τροποποιήστε τα προθέματα.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Δημιουργία της σύνδεσης. Σε αυτό το παράδειγμα, θα σας θα ρυθμίσετε τις παραμέτρους μιας τύπος σύνδεσης ασφαλείας IP. Όταν αναδημιουργήσετε τη σύνδεσή σας, χρησιμοποιήστε τον τύπο σύνδεσης που καθορίζεται για τη ρύθμιση παραμέτρων. Για πρόσθετη σύνδεση τύπους, ανατρέξτε στη σελίδα [cmdlet του PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) .

    Ορίστε τη μεταβλητή για το VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Δημιουργία της σύνδεσης. Σημειώστε ότι αυτό το δείγμα χρησιμοποιεί τη μεταβλητή $local που έχετε ορίσει στο προηγούμενο βήμα.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
