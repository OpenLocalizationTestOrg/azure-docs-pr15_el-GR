## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>Πώς μπορείτε να δημιουργήσετε ένα VNet χρησιμοποιώντας το CLI Azure

Μπορείτε να χρησιμοποιήσετε το CLI Azure για να διαχειριστείτε τους πόρους σας Azure από τη γραμμή εντολών από οποιονδήποτε υπολογιστή που εκτελεί Windows, Linux ή OSX. Για να δημιουργήσετε ένα VNet χρησιμοποιώντας το Azure CLI, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ το Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../articles/xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.
2. Εκτελέστε την εντολή **λειτουργία azure ρύθμισης παραμέτρων** για να μεταβείτε σε λειτουργία διαχείρισης πόρων, όπως φαίνεται παρακάτω.

        azure config mode arm

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        info:    New mode is arm

3. Εάν είναι απαραίτητο, εκτελέστε το **azure ομάδα δημιουργία** για να δημιουργήσετε μια νέα ομάδα πόρων, όπως φαίνεται παρακάτω. Παρατηρήστε ότι το αποτέλεσμα της εντολής. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται. Για περισσότερες πληροφορίες σχετικά με τις ομάδες πόρων, επισκεφθείτε την [Επισκόπηση της διαχείρισης πόρων Azure](../articles/virtual-network/resource-group-overview.md#resource-groups).

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

4. Εκτελέστε την εντολή **Δημιουργία vnet azure δικτύου** για να δημιουργήσετε ένα VNet και ένα υποδίκτυο, όπως φαίνεται παρακάτω. 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g (ή--ομάδα πόρων)**. Το όνομα της ομάδας πόρων όπου θα δημιουργηθεί η VNet. Για το σενάριο, *TestRG*.
    - **-n (ή--όνομα)**. Όνομα το VNet θα δημιουργηθεί. Για το σενάριο, *TestVNet*
    - **-(ή--προθέματα διεύθυνση)**. Λίστα μπλοκ CIDR που χρησιμοποιούνται για το χώρο διευθύνσεων VNet. Για το σενάριο, *192.168.0.0/16*
    - **-l (ή--θέση)**. Azure περιοχής όπου θα δημιουργηθεί η VNet. Για το σενάριο, *centralus*.

5. Εκτελέστε την εντολή **Δημιουργία υποδίκτυο vnet azure δικτύου** για να δημιουργήσετε ένα υποδίκτυο, όπως φαίνεται παρακάτω. Παρατηρήστε ότι το αποτέλεσμα της εντολής. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται.

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (ή το όνομα--vnet**. Όνομα το VNet όπου θα δημιουργηθεί το υποδίκτυο. Για το σενάριο, *TestVNet*.
    - **-n (ή--όνομα)**. Το όνομα του νέου υποδικτύου. Για το σενάριο, *FrontEnd*.
    - **-(ή--πρόθεμα διεύθυνσης)**. Μπλοκ CIDR υποδικτύου. Τέσσερις μας σενάριο, *192.168.1.0/24*.

6. Επαναλάβετε το βήμα 5 παραπάνω για να δημιουργήσετε άλλα δευτερεύοντα δίκτυα, εάν είναι απαραίτητο. Για το σενάριο, εκτελέστε την παρακάτω εντολή για να δημιουργήσετε το υποδίκτυο *παρασκηνίου* .

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. Εκτελέστε την εντολή **Εμφάνιση vnet azure δικτύου** για να προβάλετε τις ιδιότητες του νέου vnet, όπως φαίνεται παρακάτω.

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
