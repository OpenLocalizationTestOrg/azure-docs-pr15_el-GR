<properties
   pageTitle="Δημιουργία VNet διεισδύουν με τη χρήση προτύπων από διαχειριστή πόρων | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε μια διεισδύουν εικονικού δικτύου, χρησιμοποιώντας τα πρότυπα στη Διαχείριση πόρων."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-vnet-peering-using-resource-manager-templates"></a>Δημιουργία VNet διεισδύουν με τη χρήση προτύπων από διαχειριστή πόρων

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Για να δημιουργήσετε ένα VNet διεισδύουν με τη χρήση προτύπων για τη διαχείριση πόρων, ακολουθήστε τα παρακάτω βήματα:

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../powershell-install-configure.md) και ακολουθήστε τις οδηγίες προς το τέλος για να συνδεθείτε στο Azure και επιλέξτε τη συνδρομή σας.

    > [AZURE.NOTE] Το cmdlet του PowerShell για τη Διαχείριση VNet διεισδύουν αποστέλλεται με [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Το κείμενο που ακολουθεί εμφανίζει τον ορισμό μιας σύνδεσης peering VNet για VNet1 να VNet2, με βάση το παραπάνω σενάριο. Αντιγράψτε το παρακάτω περιεχόμενο και αποθηκεύστε το σε ένα αρχείο με το όνομα VNetPeeringVNet1.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. Στην παρακάτω ενότητα δείχνει τον ορισμό μιας σύνδεσης peering VNet για VNet2 να VNet1, με βάση το παραπάνω σενάριο.  Αντιγράψτε το παρακάτω περιεχόμενο και αποθηκεύστε το σε ένα αρχείο με το όνομα VNetPeeringVNet2.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    Όπως φαίνεται στο παραπάνω πρότυπο, υπάρχουν μερικές ιδιότητες με δυνατότητα ρύθμισης παραμέτρων για διεισδύουν VNet:

  	|Επιλογή|Περιγραφή|Προεπιλεγμένη|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Εάν ή όχι το χώρο διευθύνσεων από ένα ομότιμης σύνδεσης VNet περιλαμβάνεται ως τμήμα της ετικέτας virtual_network.|Ναι|
  	|AllowForwardedTraffic|Εάν η κίνηση δεν που προέρχονται από μια peered VNet αποδοχή ή απορρίφθηκαν.|Όχι|
  	|AllowGatewayTransit|Επιτρέπει την ομότιμης σύνδεσης VNet για να χρησιμοποιήσετε την πύλη VNet.|Όχι|
  	|UseRemoteGateways|Χρήση πύλης VNet σας ομότιμης σύνδεσης. Το ομότιμης σύνδεσης VNet πρέπει να έχετε μια πύλη έχει ρυθμιστεί και AllowGatewayTransit επιλεγμένο. Δεν μπορείτε να χρησιμοποιήσετε αυτήν την επιλογή, εάν έχετε μια πύλη έχει ρυθμιστεί.|Όχι|

    Κάθε σύνδεση στο VNet διεισδύουν περιλαμβάνει το σύνολο των ιδιοτήτων παραπάνω. Για παράδειγμα, μπορείτε να ορίσετε AllowVirtualNetworkAccess σε True για σύνδεση διεισδύουν VNet VNet1 για να VNet2 και να τη ρυθμίσετε σε False για τη σύνδεση peering VNet προς την άλλη κατεύθυνση.


4. Για να αναπτύξετε το αρχείο προτύπου, μπορείτε να εκτελέσετε το cmdlet New-AzureRmResourceGroupDeployment για να δημιουργήσετε ή να ενημερώσετε την ανάπτυξη. Για περισσότερες πληροφορίες σχετικά με τη χρήση προτύπων για τη διαχείριση πόρων, ανατρέξτε σε αυτό το [άρθρο](../resource-group-template-deploy.md).

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Αντικαταστήστε την ομάδα όνομα και πρότυπο αρχείο πόρου ανάλογα με την περίπτωση.

    Κάτω από ένα παράδειγμα βασίζεται σε το παραπάνω σενάριο:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Εμφανίζει το αποτέλεσμα:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Εμφανίζει το αποτέλεσμα:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Αφού ολοκληρωθεί η ανάπτυξη, μπορείτε να εκτελέσετε τα παρακάτω για να προβάλετε την κατάσταση peering cmdlet:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Εμφανίζει το αποτέλεσμα:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Αφού δημιουργηθεί διεισδύουν σε αυτό το σενάριο, θα πρέπει να μπορείτε να ξεκινήσετε τις συνδέσεις από οποιαδήποτε εικονική μηχανή με οποιοδήποτε εικονική μηχανή σε δύο VNets. Από προεπιλογή, AllowVirtualNetworkAccess είναι True και VNet διεισδύουν θα προμήθεια το πρώτο γράμμα κάθε λέξης ACL για να επιτρέψετε την επικοινωνία μεταξύ του VNets. Εξακολουθείτε να μπορείτε να εφαρμόσετε κανόνες ομάδας (NSG) ασφαλείας δικτύου να αποκλείσετε τη σύνδεση μεταξύ συγκεκριμένων δευτερεύοντα δίκτυα ή εικονικές μηχανές να αποκτήσει λεπτομερές ως έλεγχο πρόσβασης μεταξύ δύο εικονικού δικτύων.  Για τη δημιουργία κανόνων NSG περισσότερες πληροφορίες, ανατρέξτε σε αυτό το [άρθρο](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Για να δημιουργήσετε ένα VNet διεισδύουν στις συνδρομές, ακολουθήστε τα παρακάτω βήματα:

1. Πραγματοποιήστε είσοδο στο Azure με δικαιώματα χρήστη-A του λογαριασμού σε μια συνδρομή και εκτελέστε το ακόλουθο cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    Δεν είναι η απαίτηση, διεισδύουν μπορεί να πραγματοποιηθεί ακόμα και εάν οι χρήστες μεμονωμένα αυξήσετε διεισδύουν αιτήσεις τους αντίστοιχους Vnets με την προϋπόθεση ότι ταιριάζουν με τις αιτήσεις. Προσθήκη ενός χρήστη με δικαιώματα από τα άλλα VNet ως χρήστες στο το τοπικό VNet διευκολύνει να κάνετε την εγκατάσταση.

2. Πραγματοποιήστε είσοδο στο Azure με δικαιώματα χρήστη-B του λογαριασμού για τη συνδρομή-B και εκτελέστε το ακόλουθο cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. Σε ένα χρήστη του χρήστη μια περίοδο λειτουργίας σύνδεσης, εκτελέσετε αυτό το cmdlet:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    Παρακάτω θα δείτε πώς ορίζεται στο αρχείο JSON.  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. Σε μια περίοδο λειτουργίας σύνδεσης χρήστη B, εκτελέστε το ακόλουθο cmdlet:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    Δείτε τώρα πώς ορίζεται στο αρχείο JSON:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Αφού δημιουργηθεί διεισδύουν σε αυτό το σενάριο, θα πρέπει να μπορείτε να ξεκινήσετε τις συνδέσεις από οποιαδήποτε εικονική μηχανή με οποιοδήποτε εικονική μηχανή από δύο VNets στις διάφορες συνδρομές.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Σε αυτό το σενάριο, μπορείτε να αναπτύξετε το παρακάτω για να καθορίσετε το διεισδύουν VNet δείγμα προτύπου.  Θα χρειαστεί να ορίσετε την ιδιότητα AllowForwardedTraffic στην τιμή True, που σας επιτρέπουν να συσκευής εικονικού δικτύου του peered VNet για αποστολή και λήψη κίνηση.

    Εδώ είναι το πρότυπο για τη δημιουργία ενός VNet διεισδύουν από HubVNet σε VNet1. Σημειώστε ότι AllowForwardedTraffic έχει οριστεί σε false.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Εδώ είναι το πρότυπο για τη δημιουργία ενός VNet διεισδύουν από VNet1 σε HubVnet. Σημειώστε ότι AllowForwardedTraffic έχει οριστεί στην τιμή true.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Αφού δημιουργηθεί διεισδύουν, μπορείτε να ανατρέξετε σε αυτό το [άρθρο](virtual-network-create-udr-arm-ps.md) για να ορίσετε routes(UDR) που ορίζονται από το χρήστη για να ανακατευθύνετε την κίνηση VNet1 μέσω μια εικονική συσκευή για να χρησιμοποιήσετε τις δυνατότητές του. Όταν ορίζετε την επόμενη διεύθυνση μεταπήδησης στο θέμα δρομολόγηση, μπορείτε να ορίσετε τη διεύθυνση IP του εικονικού συσκευής στο το ομότιμης σύνδεσης VNet HubVNet.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Για να δημιουργήσετε μια διεισδύουν μεταξύ εικονικού δικτύων από διαφορετική ανάπτυξη μοντέλα, ακολουθήστε τα παρακάτω βήματα:
1. Το κείμενο που ακολουθεί παρουσιάζει τον ορισμό μιας σύνδεσης peering VNet για VNET1 να VNET2 σε αυτό το σενάριο. Μόνο μία σύνδεση απαιτείται για ομότιμη κλασική εικονικό δίκτυο σε δίκτυο εικονικού manager Azure πόρων.

    Φροντίστε να τοποθετήσετε το Αναγνωριστικό συνδρομής για το σημείο όπου βρίσκεται ο κλασική εικονικού δικτύου ή VNET2 και αλλάξτε MyResouceGroup στο όνομα της ομάδας κατάλληλο πόρων.

    {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "Παράμετροι": {}, "Μεταβλητές": {}, "Πόροι": [{"apiVersion": "2016-06-01", "Τύπος": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings", "όνομα": "VNET1 LinkToVNET2", "θέση": "[ομάδα πόρων () .location]", "Ιδιότητες": {"allowVirtualNetworkAccess": τιμή true, "allowForwardedTraffic": false, "allowGatewayTransit": false, "useRemoteGateways": false, "remoteVirtualNetwork": {"αναγνωριστικό": "[resourceId (' Microsoft.ClassicNetwork/virtualNetworks', 'VNET2')]"}}}]}

2. Για να αναπτύξετε το αρχείο προτύπου, εκτελέστε το ακόλουθο cmdlet για να δημιουργήσετε ή να ενημερώσετε την ανάπτυξη.

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Όταν ολοκληρωθεί με επιτυχία την ανάπτυξη, μπορείτε να εκτελέσετε το ακόλουθο cmdlet για να προβάλετε την κατάσταση peering:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Αφού δημιουργηθεί διεισδύουν μεταξύ ενός κλασική VNet και διαχείριση πόρων VNet, θα πρέπει να μπορείτε να ξεκινήσετε συνδέσεις από οποιαδήποτε εικονική μηχανή στο VNET1 με οποιαδήποτε εικονική μηχανή στο VNET2 και το αντίστροφο.
