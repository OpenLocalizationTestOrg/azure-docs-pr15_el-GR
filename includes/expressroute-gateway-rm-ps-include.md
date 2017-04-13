Τα βήματα για αυτήν την εργασία, χρησιμοποιήστε μια VNet με βάση τις παρακάτω τιμές. Πρόσθετες ρυθμίσεις και τα ονόματα είναι επίσης με διάρθρωση σε αυτήν τη λίστα. Δεν χρησιμοποιούμε αυτήν τη λίστα απευθείας σε οποιοδήποτε από τα βήματα, παρόλο που προσθέσουμε μεταβλητές με βάση τις τιμές σε αυτήν τη λίστα. Μπορείτε να αντιγράψετε τη λίστα για να χρησιμοποιήσετε ως αναφορά, αντικαθιστώντας τις τιμές με το δικό σας.

Λίστα αναφοράς ρύθμισης παραμέτρων:
    
- Όνομα εικονικού δικτύου = "TestVNet"
- Εικονικό χώρο διευθύνσεων δικτύου = 192.168.0.0/16
- Ομάδα πόρων = "TestRG"
- Όνομα Subnet1 = "FrontEnd" 
- Χώρο διευθύνσεων Subnet1 = "192.168.0.0/16"
- Όνομα πύλης υποδικτύου: "GatewaySubnet" πάντα πρέπει να ονομάσετε ένα υποδίκτυο πύλης *GatewaySubnet*.
- Χώρο διευθύνσεων υποδικτύου πύλης = "192.168.200.0/26"
- Περιοχή = "Ανατολικής η.π.α."
- Όνομα πύλης = "GW"
- Όνομα πύλης IP = "GWIP"
- Ρύθμιση παραμέτρων IP όνομα πύλης = "gwipconf"
-  Τύπος = "ExpressRoute" Αυτός ο τύπος είναι απαραίτητη για ρύθμιση παραμέτρων ExpressRoute.
- Δημόσια IP όνομα πύλης = "gwpip"


## <a name="add-a-gateway"></a>Προσθήκη μιας πύλης

1. Σύνδεση με τη συνδρομή σας στο Azure. 

        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

2. Δηλώστε την μεταβλητές για αυτήν την άσκηση. Αυτό το παράδειγμα θα χρησιμοποιήσει τη χρήση των μεταβλητών σε το παρακάτω δείγμα. Φροντίστε να επεξεργαστείτε έτσι ώστε να αντικατοπτρίζει τις ρυθμίσεις που θέλετε να χρησιμοποιήσετε. 
        
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"

3. Αποθηκεύστε το αντικείμενο εικονικού δικτύου ως μεταβλητή.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG

4. Προσθέστε ένα δευτερεύον πύλης σας εικονικού δικτύου. Το υποδίκτυο πύλης πρέπει να ονομάζεται "GatewaySubnet". Που θα θέλετε να δημιουργήσετε μια πύλη που είναι /27 ή μεγαλύτερο (/ 26, / 25, κ.λπ.).
            
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26

5. Ρύθμιση των παραμέτρων.

            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

6. Αποθηκεύστε το υποδίκτυο πύλης ως μεταβλητή.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet

7. Αίτηση για μια δημόσια διεύθυνση IP. Η διεύθυνση IP ζητείται πριν από τη δημιουργία της πύλης. Δεν μπορείτε να καθορίσετε τη διεύθυνση IP που θέλετε να χρησιμοποιήσετε. δυναμικά έχει εκχωρηθεί. Θα χρησιμοποιήσετε αυτήν τη διεύθυνση IP στην επόμενη ενότητα ρύθμισης παραμέτρων. Το AllocationMethod πρέπει να είναι δυναμική.

        $pip = New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic

8. Δημιουργήστε τη ρύθμιση παραμέτρων για την πύλη. Η ρύθμιση παραμέτρων της πύλης ορίζει το υποδίκτυο και στη δημόσια διεύθυνση IP για να χρησιμοποιήσετε. Σε αυτό το βήμα, καθορίζετε τη ρύθμιση παραμέτρων που θα χρησιμοποιηθεί κατά τη δημιουργία της πύλης. Αυτό το βήμα δεν δημιουργεί το αντικείμενο πύλης. Χρησιμοποιήστε το παρακάτω δείγμα για τη δημιουργία της ρύθμισης παραμέτρων της πύλης. 

        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

9. Δημιουργία της πύλης. Σε αυτό το βήμα, το **-GatewayType** είναι ιδιαίτερα σημαντικό. Πρέπει να χρησιμοποιήσετε την τιμή **ExpressRoute**. Σημειώστε ότι, μετά την εκτέλεση αυτών των cmdlet, της πύλης μπορεί να διαρκέσει 20 λεπτά ή περισσότερο για να δημιουργήσετε.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>Επαλήθευση της πύλης που δημιουργήθηκε

Χρησιμοποιήστε την παρακάτω εντολή για να επαληθεύσετε ότι η πύλη έχει δημιουργηθεί.

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>Αλλαγή του μεγέθους μιας πύλης

Υπάρχουν αρκετά της [Πύλης SKU](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή για να αλλάξετε την πύλη SKU οποιαδήποτε στιγμή.

>[AZURE.IMPORTANT] Αυτή η εντολή δεν λειτουργεί για UltraPerformance πύλη. Για να αλλάξετε την πύλη για μια πύλη UltraPerformance, πρώτα να καταργήσετε την υπάρχουσα πύλη ExpressRoute και, στη συνέχεια, δημιουργήστε μια νέα πύλη UltraPerformance. Για να υποβιβάσετε της πύλης από μια πύλη UltraPerformance, πρώτα να καταργήσετε την πύλη UltraPerformance και, στη συνέχεια, δημιουργήστε μια νέα πύλη.

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>Κατάργηση μιας πύλης

Χρησιμοποιήστε την παρακάτω εντολή για να καταργήσετε μια πύλη

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  
