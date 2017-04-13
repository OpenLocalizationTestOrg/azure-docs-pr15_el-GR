## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Πώς μπορείτε να δημιουργήσετε μια κλασική VNet χρησιμοποιώντας Azure CLI

Μπορείτε να χρησιμοποιήσετε το CLI Azure για να διαχειριστείτε τους πόρους σας Azure από τη γραμμή εντολών από οποιονδήποτε υπολογιστή που εκτελεί Windows, Linux ή OSX. Για να δημιουργήσετε ένα VNet χρησιμοποιώντας το Azure CLI, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../articles/xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.
2. Εκτελέστε την εντολή **Δημιουργία vnet azure δικτύου** για να δημιουργήσετε ένα VNet και ένα υποδίκτυο, όπως φαίνεται παρακάτω. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται.

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    Αναμενόμενο αποτέλεσμα:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **--vnet**. Όνομα το VNet θα δημιουργηθεί. Για το σενάριο, *TestVNet*
    - **-e (ή--χώρου διευθύνσεων)**. VNet χώρου διευθύνσεων. Για το σενάριο, *192.168.0.0*
    - **-i (ή - cidr)**. Μάσκα δικτύου σε μορφή CIDR. Για το σενάριο, *16*.
    - **- n (ή--υποδικτύου όνομα**). Το όνομα του πρώτου υποδικτύου. Για το σενάριο, *FrontEnd*.
    - **-p (ή--υποδικτύου-Έναρξη-ip)**. Αρχική διεύθυνση IP για το υποδίκτυο ή το χώρο υποδικτύου διευθύνσεων. Για το σενάριο, *192.168.1.0*.
    - **-r (ή--υποδικτύου cidr)**. Μάσκα δικτύου σε μορφή CIDR για υποδίκτυο. Για το σενάριο, *24*.
    - **-l (ή--θέση)**. Azure περιοχής όπου θα δημιουργηθεί η VNet. Για το σενάριο, *Κεντρικές ΗΠΑ*.

3. Εκτελέστε την εντολή **Δημιουργία υποδίκτυο vnet azure δικτύου** για να δημιουργήσετε ένα υποδίκτυο, όπως φαίνεται παρακάτω. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται.

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (ή το όνομα--vnet**. Όνομα το VNet όπου θα δημιουργηθεί το υποδίκτυο. Για το σενάριο, *TestVNet*.
    - **-n (ή--όνομα)**. Το όνομα του νέου υποδικτύου. Για το σενάριο, *παρασκηνίου*.
    - **-(ή--πρόθεμα διεύθυνσης)**. Μπλοκ CIDR υποδικτύου. Τέσσερις μας σενάριο, *192.168.2.0/24*.

4. Εκτελέστε την εντολή **Εμφάνιση vnet azure δικτύου** για να προβάλετε τις ιδιότητες του νέου vnet, όπως φαίνεται παρακάτω.

            azure network vnet show

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
