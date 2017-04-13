Μπορείτε να επαληθεύσετε ότι τη σύνδεση που ολοκληρώθηκε με επιτυχία, χρησιμοποιώντας το `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet, με ή χωρίς `-Debug`. 

1. Χρησιμοποιήστε το cmdlet παράδειγμα που ακολουθεί, τη ρύθμιση των παραμέτρων των τιμών ανάλογα με τη δική σας. Εάν σας ζητηθεί, επιλέξτε 'A' προκειμένου να εκτελέσετε 'All'. Στο παράδειγμα, `-Name` αναφέρεται στο όνομα της σύνδεσης που δημιουργήσατε και θέλετε να ελέγξετε.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Όταν ολοκληρωθεί το cmdlet, προβάλετε τις τιμές. Στο παρακάτω παράδειγμα, εμφανίζει την κατάσταση σύνδεσης ως 'Σύνδεση' και μπορείτε να δείτε εισόδου και εξόδου byte.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }