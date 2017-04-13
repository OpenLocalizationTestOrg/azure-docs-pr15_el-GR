## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Ανάπτυξη του προτύπου ARM, χρησιμοποιώντας το CLI Azure

Για να αναπτύξετε το πρότυπο ARM που λάβατε με τη χρήση Azure CLI, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../articles/xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.
2. Εκτελέστε το **`azure config mode`** εντολή για να μεταβείτε σε λειτουργία διαχείρισης πόρων, όπως φαίνεται παρακάτω.

        azure config mode arm

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        info:    New mode is arm

3. Εάν είναι απαραίτητο, εκτελέστε το **`azure group create`** για να δημιουργήσετε μια νέα ομάδα πόρων, όπως φαίνεται παρακάτω. Παρατηρήστε ότι το αποτέλεσμα της εντολής. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται. Για περισσότερες πληροφορίες σχετικά με τις ομάδες πόρων, επισκεφθείτε την [Επισκόπηση της διαχείρισης πόρων Azure](../articles/resource-group-overview.md).

        azure group create -n TestRG -l centralus

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (ή--όνομα)**. Όνομα για τη νέα ομάδα πόρων. Για το σενάριο, *TestRG*.
    - **-l (ή--θέση)**. Azure περιοχής όπου θα δημιουργηθεί η νέα ομάδα πόρων. Για το σενάριο, *centralus*.

4. Εκτελέστε το **`azure group deployment create`** cmdlet για να αναπτύξετε το νέο VNet χρησιμοποιώντας το πρότυπο και η παράμετρος αρχεία που έχουν ληφθεί και τροποποίησης παραπάνω. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g (ή--ομάδα πόρων)**. Όνομα της ομάδας πόρων τη νέα VNet θα δημιουργηθεί στο.
    - **-f (ή--αρχείο προτύπου)**. Διαδρομή προς το αρχείο προτύπου ARM.
    - **-e (ή--παράμετροι-αρχείο)**. Διαδρομή προς το αρχείο σας ARM παράμετροι.

5. Εκτελέστε το **`azure network vnet show`** εντολή για να προβάλετε τις ιδιότητες του νέου vnet, όπως φαίνεται παρακάτω.

        azure network vnet show -g TestRG -n TestVNet

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
