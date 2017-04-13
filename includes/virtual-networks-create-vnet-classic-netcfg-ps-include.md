## <a name="how-to-create-a-vnet-using-a-network-config-file-from-powershell"></a>Πώς μπορείτε να δημιουργήσετε ένα VNet χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων δικτύου από το PowerShell

Azure χρησιμοποιεί ένα αρχείο xml για να ορίσετε όλα VNets διαθέσιμη σε μια συνδρομή. Μπορείτε να κάνετε λήψη αυτού του αρχείου, και να επεξεργαστείτε για να τροποποιήσετε ή να διαγράψετε υπάρχοντα VNets και να δημιουργείτε νέα. Σε αυτό το έγγραφο, θα μάθετε πώς μπορείτε να κάνετε λήψη αυτού του αρχείου, γνωστή ως αρχείο ρύθμισης παραμέτρων (ή netcgf) δικτύου, και να επεξεργαστείτε για να δημιουργήσετε μια νέα VNet. Επιλέξτε το [σχήμα της ομάδας παραμέτρων Azure εικονικού δικτύου](https://msdn.microsoft.com/library/azure/jj157100.aspx) για να μάθετε περισσότερα σχετικά με το αρχείο ρύθμισης παραμέτρων δικτύου.

Για να δημιουργήσετε ένα VNet χρησιμοποιώντας ένα αρχείο netcfg χρησιμοποιώντας το PowerShell, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../articles/powershell-install-configure.md) και ακολουθήστε τις οδηγίες προς το τέλος για να συνδεθείτε στο Azure και επιλέξτε τη συνδρομή σας.
2. Από την κονσόλα Azure PowerShell, χρησιμοποιήστε το cmdlet **Get-AzureVnetConfig** για να κάνετε λήψη του αρχείου ρύθμισης παραμέτρων δικτύου, εκτελέστε την παρακάτω εντολή. 

        Get-AzureVNetConfig -ExportToFile c:\NetworkConfig.xml

    Αναμενόμενο αποτέλεσμα:

        XMLConfiguration                                                                                                     
        ----------------                                                                                                     
        <?xml version="1.0" encoding="utf-8"?>...  

3. Ανοίξτε το αρχείο που αποθηκεύσατε στο βήμα 2 παραπάνω χρησιμοποιώντας οποιαδήποτε εφαρμογή επεξεργασίας XML ή κείμενο και αναζητήστε το **<VirtualNetworkSites>** στοιχείο. Εάν έχετε οποιοδήποτε δίκτυα που έχετε ήδη δημιουργήσει, κάθε δίκτυο θα εμφανίζεται ως ξεχωριστή **<VirtualNetworkSite>** στοιχείο.
4. Για να δημιουργήσετε το εικονικό δίκτυο που περιγράφονται σε αυτό το σενάριο, προσθέστε το παρακάτω δείγμα XML κάτω από το **<VirtualNetworkSites>** στοιχείο:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

9.  Αποθηκεύστε το αρχείο ρύθμισης παραμέτρων δικτύου.
10. Από την κονσόλα Azure PowerShell, χρησιμοποιήστε το cmdlet **Set-AzureVnetConfig** για να αποστείλετε το αρχείο ρύθμισης παραμέτρων δικτύου, εκτελέστε την παρακάτω εντολή. Παρατηρήστε το αποτέλεσμα του κάτω από την εντολή, θα πρέπει να βλέπετε **ολοκληρώθηκε με** στην περιοχή **OperationStatus**. Εάν αυτό σημαίνει ότι δεν ισχύει, ελέγξτε το αρχείο xml για σφάλματα.

        Set-AzureVNetConfig -ConfigurationPath c:\NetworkConfig.xml

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        OperationDescription OperationId                          OperationStatus
        -------------------- -----------                          ---------------
        Set-AzureVNetConfig  49579cb9-3f49-07c3-ada2-7abd0e28c4e4 Succeeded 
    
11. Από την κονσόλα Azure PowerShell, χρησιμοποιήστε το cmdlet **Get-AzureVnetSite** για να βεβαιωθείτε ότι το νέο δίκτυο προστέθηκε, εκτελέστε την παρακάτω εντολή. 

        Get-AzureVNetSite -VNetName TestVNet

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        AddressSpacePrefixes : {192.168.0.0/16}
        Location             : Central US
        AffinityGroup        : 
        DnsServers           : {}
        GatewayProfile       : 
        GatewaySites         : 
        Id                   : b953f47b-fad9-4075-8cfe-73ff9c98278f
        InUse                : False
        Label                : 
        Name                 : TestVNet
        State                : Created
        Subnets              : {FrontEnd, BackEnd}
        OperationDescription : Get-AzureVNetSite
        OperationId          : 3f35d533-1f38-09c0-b286-3d07cd0904d8
        OperationStatus      : Succeeded