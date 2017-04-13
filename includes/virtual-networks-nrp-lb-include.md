## <a name="load-balancer"></a>Εξισορρόπηση φόρτου
Μια μονάδα εξισορρόπησης φόρτου χρησιμοποιείται όταν θέλετε να περιορίσετε το μέγεθος των εφαρμογών σας. Σενάρια τυπικές ανάπτυξης αφορούν εφαρμογές που εκτελούνται σε πολλές παρουσίες Εικονική. Οι εμφανίσεις Εικονική είναι fronted από μια μονάδα εξισορρόπησης φόρτου που σας βοηθά να διανείμετε κίνηση του δικτύου στις διάφορες παρουσίες. 

![NIC του σε μια μεμονωμένη Εικονική](./media/resource-groups-networking/figure8.png)

| Ιδιότητα | Περιγραφή |
|---|---|
| *frontendIPConfigurations* | μια μονάδα εξισορρόπησης φόρτου μπορούν να περιλαμβάνουν μία ή περισσότερες προσκηνίου διευθύνσεις IP, διαφορετικά γνωστό ως μια εικονική διευθύνσεις IP (VIPs). Αυτές οι διευθύνσεις IP χρησιμοποιηθεί ως εισόδου για την κυκλοφορία και μπορεί να είναι IP δημόσια ή ιδιωτική IP |
|*backendAddressPools* | αυτές είναι διευθύνσεις IP που σχετίζεται με το NIC Εικονική στην οποία θα διανεμηθεί φόρτωσης |
|*loadBalancingRules* | μια ιδιότητα κανόνας αντιστοιχίζει ένα δεδομένο προσκηνίου IP και συνδυασμό θύρα σε ένα σύνολο παρασκηνίου διευθύνσεις IP και θύρα συνδυασμό. Με έναν μεμονωμένο ορισμό ενός πόρου εξισορρόπησης φόρτου, μπορείτε να ορίσετε πολλές φόρτωσης εξισορρόπηση κανόνες, κάθε κανόνα που αντικατοπτρίζουν ένας συνδυασμός από ένα πρώτο πλάνο Τερματισμός IP και τη θύρα και πίσω τέλος IP και τη θύρα που σχετίζεται με εικονικές μηχανές. Ο κανόνας είναι μια θύρα στο χώρο συγκέντρωσης προσκηνίου να πολλές εικονικές μηχανές στο χώρο συγκέντρωσης παρασκηνίου |  
| *Καθετήρες* | καθετήρες σας επιτρέπουν να παρακολουθείτε την εύρυθμη λειτουργία των παρουσιών Εικονική. Εάν αποτύχει μια δοκιμή του εύρυθμης λειτουργίας, την παρουσία εικονική μηχανή θα ληφθούν από περιστροφής αυτόματα |
| *inboundNatRules* | Κανόνες NAT που καθορίζει την εισερχόμενη κυκλοφορία ροή σε πρώτο πλάνο Τερματισμός IP και διανέμονται για το IP παρασκηνίου σε μια συγκεκριμένη εικονική μηχανή παρουσία. Κανόνας NAT είναι μία θύρα στο χώρο συγκέντρωσης προσκηνίου σε μια εικονική μηχανή στο χώρο συγκέντρωσης παρασκηνίου | 

Παράδειγμα του προτύπου εξισορρόπησης φόρτου σε μορφή Json:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "dnsNameforLBIP": {
          "type": "string",
          "metadata": {
            "description": "Unique DNS name"
          }
        },
        "location": {
          "type": "string",
          "allowedValues": [
            "East US",
            "West US",
            "West Europe",
            "East Asia",
            "Southeast Asia"
          ],
          "metadata": {
            "description": "Location to deploy"
          }
        },
        "addressPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/16",
          "metadata": {
            "description": "Address Prefix"
          }
        },
        "subnetPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/24",
          "metadata": {
            "description": "Subnet Prefix"
          }
        },
        "publicIPAddressType": {
          "type": "string",
          "defaultValue": "Dynamic",
          "allowedValues": [
            "Dynamic",
            "Static"
          ],
          "metadata": {
            "description": "Public IP type"
          }
        }
      },
      "variables": {
        "virtualNetworkName": "virtualNetwork1",
        "publicIPAddressName": "publicIp1",
        "subnetName": "subnet1",
        "loadBalancerName": "loadBalancer1",
        "nicName": "networkInterface1",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "nicId": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "backEndIPConfigID": "[concat(variables('nicId'),'/ipConfigurations/ipconfig1')]"
      },
      "resources": [
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[parameters('location')]",
      "properties": {
        "publicIPAllocationMethod": "[parameters('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsNameforLBIP')]"
        }
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[parameters('location')]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[parameters('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[parameters('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "subnet": {
                "id": "[variables('subnetRef')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(variables('lbID'), '/backendAddressPools/LoadBalancerBackend')]"
                }
              ],
              "loadBalancerInboundNatRules": [
                {
                  "id": "[concat(variables('lbID'),'/inboundNatRules/RDP')]"
                }
              ]
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[variables('publicIPAddressID')]"
              }
            }
          }
        ],
        "backendAddressPools": [
          {
            "name": "loadBalancerBackEnd"
          }
        ],
        "inboundNatRules": [
          {
            "name": "RDP",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPort": 3389,
              "backendPort": 3389,
              "enableFloatingIP": false
            }
          }
        ]
      }
    }
      ]
    }

### <a name="additional-resources"></a>Πρόσθετοι πόροι

Διαβάστε [Φόρτωση εξισορρόπησης REST API](https://msdn.microsoft.com/library/azure/mt163651.aspx) για περισσότερες πληροφορίες.