## <a name="traffic-manager-profile"></a>Κίνηση Διαχείριση προφίλ

Διαχείριση κυκλοφορία και τη πόρων τελικού σημείου θυγατρικό Ενεργοποίηση δρομολόγηση DNS σε τελικά σημεία στο Azure και εκτός της Azure. Όπως διανομής κίνηση διέπεται από μεθόδων δρομολόγησης πολιτικής. Διαχείριση κίνηση επιτρέπει επίσης τελικού σημείου εύρυθμης λειτουργίας να παρακολουθούνται και κίνηση εκτρέπονται σωστά με βάση την εύρυθμη λειτουργία του τελικού σημείου. 

| Ιδιότητα | Περιγραφή |
|---|---|
|**trafficRoutingMethod**| πιθανές τιμές είναι οι *επιδόσεις*, *Weighted*και *προτεραιότητα* | 
| **dnsConfig** | FQDN για το προφίλ | 
| **Πρωτόκολλο** | παρακολούθηση πρωτόκολλο, πιθανές τιμές είναι *HTTP* και *HTTPS*|
| **Θύρα** | παρακολούθηση θύρα |  
| **Διαδρομή** | παρακολούθηση διαδρομής |
| **Τα τελικά σημεία** |  κοντέινερ για τους πόρους τελικού σημείου | 

### <a name="endpoint"></a>Τελικό σημείο 

Ένα τελικό σημείο είναι ένας πόρος θυγατρικό προφίλ Manager κίνηση. Αντιπροσωπεύει μια υπηρεσία ή web τελικού σημείου σε ποια χρήστη κατανέμεται κυκλοφορία με βάση την πολιτική έχει ρυθμιστεί στο προφίλ της διαχείρισης κίνηση πόρου. 

| Ιδιότητα | Περιγραφή | 
|---|---| 
| **Τύπος** |  ο τύπος του τελικού σημείου, πιθανές τιμές είναι *Azure τελικό σημείο* *Εξωτερικών τελικού σημείου*και *Ένθετων συναρτήσεων τελικού σημείου* | 
| **targetResourceId** |  δημόσια διεύθυνση IP του ένα τελικό σημείο υπηρεσίας ή web. Αυτό μπορεί να είναι Azure ή εξωτερική τελικού σημείου. | 
| **Βάρος** | το πάχος τελικού σημείου χρησιμοποιούνται στη διαχείριση της κυκλοφορίας. | 
| **Προτεραιότητα** | προτεραιότητα του τελικού σημείου, χρησιμοποιείται για να ορίσετε μια ενέργεια ανακατεύθυνσης |

Δείγμα της διαχείρισης κίνηση σε μορφή Json: 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>Πρόσθετοι πόροι

Διαβάστε [REST API τεκμηρίωση για τη Διαχείριση κίνηση](https://msdn.microsoft.com/library/azure/mt163664.aspx) για περισσότερες πληροφορίες.
