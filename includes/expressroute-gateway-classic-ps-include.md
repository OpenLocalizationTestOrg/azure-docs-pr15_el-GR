Πρέπει πρώτα να δημιουργήσετε ένα VNet και ένα υποδίκτυο πύλης, πριν από την εργασία για τις ακόλουθες εργασίες. Ανατρέξτε στο άρθρο [Ρύθμιση εικονικού δικτύου με την πύλη κλασική](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) για περισσότερες πληροφορίες.   

## <a name="add-a-gateway"></a>Προσθήκη μιας πύλης

Χρησιμοποιήστε την παρακάτω εντολή για να δημιουργήσετε μια πύλη. Φροντίστε να αντικαταστήσετε τις τιμές για τη δική σας.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Επαλήθευση της πύλης που δημιουργήθηκε

Χρησιμοποιήστε την παρακάτω εντολή για να επαληθεύσετε ότι η πύλη έχει δημιουργηθεί. Αυτή η εντολή ανακτά επίσης το Αναγνωριστικό πύλη, το οποίο που χρειάζεστε για τις άλλες λειτουργίες.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Αλλαγή του μεγέθους μιας πύλης

Υπάρχουν αρκετά της [Πύλης SKU](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή για να αλλάξετε την πύλη SKU οποιαδήποτε στιγμή.

>[AZURE.IMPORTANT] Αυτή η εντολή δεν λειτουργεί για UltraPerformance πύλη. Για να αλλάξετε την πύλη για μια πύλη UltraPerformance, πρώτα να καταργήσετε την υπάρχουσα πύλη ExpressRoute και, στη συνέχεια, δημιουργήστε μια νέα πύλη UltraPerformance. Για να υποβιβάσετε της πύλης από μια πύλη UltraPerformance, πρώτα να καταργήσετε την πύλη UltraPerformance και, στη συνέχεια, δημιουργήστε μια νέα πύλη. 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Κατάργηση μιας πύλης

Χρησιμοποιήστε την παρακάτω εντολή για να καταργήσετε μια πύλη

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>