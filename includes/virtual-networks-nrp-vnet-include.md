## <a name="virtual-network"></a>Εικονικό δίκτυο
Εικονικοί πόροι δίκτυα (VNET) και δευτερευόντων δικτύων βοηθούν ορίζουν ένα όριο ασφαλείας για φόρτους εργασίας που εκτελείται στο Azure. Μια VNet χαρακτηρίζεται από μια συλλογή από τους χώρους διευθύνσεων ορίζεται ως μπλοκ CIDR. 

>[AZURE.NOTE] Οι διαχειριστές δικτύου είναι εξοικειωμένοι με CIDR σημειογραφία. Εάν δεν είστε εξοικειωμένοι με CIDR, [Μάθετε περισσότερα σχετικά με αυτό](http://whatismyipaddress.com/cidr).

![VNet με πολλά δευτερεύοντα δίκτυα](./media/resource-groups-networking/Figure4.png)

VNets περιέχει τις ακόλουθες ιδιότητες.

|Ιδιότητα|Περιγραφή|Δείγματα τιμών|
|---|---|---|
|**addressSpace**|Συλλογή προθέματα διευθύνσεων που απαρτίζουν το VNet στο CIDR σημειογραφία|192.168.0.0/16|
|**δευτερεύοντα δίκτυα**|Συλλογή των δευτερευόντων δικτύων που απαρτίζουν το VNet|Δείτε παρακάτω [δευτερεύοντα δίκτυα](#Subnets) .|
|**διεύθυνση IP**|Η διεύθυνση IP που έχουν εκχωρηθεί σε αντικείμενο. Αυτή είναι μια ιδιότητα μόνο για ανάγνωση.|104.42.233.77|

### <a name="subnets"></a>Δευτερεύοντα δίκτυα
Ένα υποδίκτυο είναι ένας πόρος θυγατρικό από μια VNet, και σας βοηθά ορίσετε τμήματα των κενών διαστημάτων διεύθυνση μέσα σε ένα μπλοκ CIDR, χρησιμοποιώντας προθέματα διευθύνσεων IP. NIC μπορεί να προστεθεί σε δευτερεύοντα δίκτυα, και συνδεδεμένοι ΣΠΣ, παρέχοντας συνδεσιμότητας για διάφορες φόρτους εργασίας.

Δευτερεύοντα δίκτυα περιέχουν τις ακόλουθες ιδιότητες. 

|Ιδιότητα|Περιγραφή|Δείγματα τιμών|
|---|---|---|
|**addressPrefix**|Πρόθεμα μία διεύθυνση που απαρτίζουν το υποδίκτυο στο CIDR σημειογραφία|192.168.1.0/24|
|**networkSecurityGroup**|NSG εφαρμόζονται στο υποδίκτυο|ανατρέξτε στο θέμα [NSGs](#Network-Security-Group)|
|**routeTable**|Δρομολόγηση πίνακα εφαρμόζεται στο υποδίκτυο|ανατρέξτε στο θέμα [UDR](#Route-table)|
|**ipConfigurations**|Συλλογή των αντικειμένων παραμέτρων IP που χρησιμοποιούνται από το NIC συνδεδεμένο με το υποδίκτυο|ανατρέξτε στο θέμα [UDR](#Route-table)|


Δείγμα VNet σε μορφή JSON:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Πρόσθετοι πόροι

- Λάβετε περισσότερες πληροφορίες σχετικά με το [VNet](../articles/virtual-network/virtual-networks-overview.md).
- Διαβάστε την [τεκμηρίωση αναφοράς REST API](https://msdn.microsoft.com/library/azure/mt163650.aspx) για VNets.
- Διαβάστε την [τεκμηρίωση αναφοράς REST API](https://msdn.microsoft.com/library/azure/mt163618.aspx) για δευτερεύοντα δίκτυα.