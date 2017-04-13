<properties
    pageTitle="Ανάπτυξη Azure πόρους χρησιμοποιώντας C# | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε C# και διαχείριση πόρων Azure για να δημιουργήσετε Microsoft Azure πόρους."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="deploy-azure-resources-using-c"></a>Ανάπτυξη πόρους Azure χρησιμοποιώντας C# 

Αυτό το άρθρο σας δείχνει πώς μπορείτε να δημιουργήσετε Azure πόρους χρησιμοποιώντας C#.

Πρέπει πρώτα να βεβαιωθείτε ότι έχετε ολοκληρώσει αυτές τις εργασίες:

- Εγκατάσταση του [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Επαληθεύστε την εγκατάσταση του [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) ή του [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Λήψη ενός [διακριτικού ελέγχου ταυτότητας](../resource-group-authenticate-service-principal.md)

Χρειάζονται περίπου 30 λεπτά για να κάνετε τα εξής βήματα.

## <a name="step-1-create-a-visual-studio-project-and-install-the-libraries"></a>Βήμα 1: Δημιουργήστε ένα έργο Visual Studio και εγκαταστήστε τις βιβλιοθήκες

Τα πακέτα NuGet είναι ο ευκολότερος τρόπος για να εγκαταστήσετε τις βιβλιοθήκες που χρειάζεστε για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης. Για να δείτε τις βιβλιοθήκες που χρειάζεστε στο Visual Studio, κάντε τα εξής βήματα:

1. Κάντε κλικ στο **αρχείο** > **νέα** > **έργου**.

2. Στα **πρότυπα** > **Visual C#**, επιλέξτε **Εφαρμογή Console**, πληκτρολογήστε το όνομα και τη θέση του έργου και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

3. Κάντε δεξί κλικ στο όνομα του έργου στην Εξερεύνηση λύσεων και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.

4. Τύπος *Υπηρεσίας καταλόγου Active Directory* στο πλαίσιο αναζήτησης, κάντε κλικ στην επιλογή **εγκατάσταση** του πακέτου βιβλιοθήκη ελέγχου ταυτότητας Active Directory και, στη συνέχεια, ακολουθήστε τις οδηγίες για να εγκαταστήσετε το πακέτο.

5. Στο επάνω μέρος της σελίδας, επιλέξτε **Περιλαμβάνουν προέκδοση**. Τύπος *Microsoft.Azure.Management.Compute* στο πλαίσιο αναζήτησης, κάντε κλικ στην επιλογή **εγκατάσταση** για τις βιβλιοθήκες .NET τον υπολογισμό και, στη συνέχεια, ακολουθήστε τις οδηγίες για να εγκαταστήσετε το πακέτο.

6. Τύπος *Microsoft.Azure.Management.Network* στο πλαίσιο αναζήτησης, κάντε κλικ στην επιλογή **εγκατάσταση** για τις βιβλιοθήκες .NET δικτύου και, στη συνέχεια, ακολουθήστε τις οδηγίες για να εγκαταστήσετε το πακέτο.

7. Τύπος *Microsoft.Azure.Management.Storage* στο πλαίσιο αναζήτησης, κάντε κλικ στην επιλογή **εγκατάσταση** για τις βιβλιοθήκες .NET χώρου αποθήκευσης και, στη συνέχεια, ακολουθήστε τις οδηγίες για να εγκαταστήσετε το πακέτο.

8. Πληκτρολογήστε *Microsoft.Azure.Management.ResourceManager* στο πλαίσιο αναζήτησης, κάντε κλικ στην επιλογή **εγκατάσταση** για τις βιβλιοθήκες διαχείρισης πόρων.

Τώρα είστε έτοιμοι να ξεκινήσετε να χρησιμοποιείτε τις βιβλιοθήκες για να δημιουργήσετε την εφαρμογή σας.

## <a name="step-2-create-the-credentials-that-are-used-to-authenticate-requests"></a>Βήμα 2: Δημιουργία τα διαπιστευτήρια που χρησιμοποιούνται για τον έλεγχο ταυτότητας αιτήσεις

Τώρα μπορείτε να μορφοποιήσετε τις πληροφορίες της εφαρμογής που δημιουργήσατε προηγουμένως σε διαπιστευτηρίων που χρησιμοποιούνται για τον έλεγχο ταυτότητας αιτήσεις για τη διαχείριση πόρων Azure.

1. Ανοίξτε το αρχείο Program.cs για το έργο που δημιουργήσατε και, στη συνέχεια, προσθέστε αυτές τις χρήση προτάσεων στο επάνω μέρος του αρχείου:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. Για να δημιουργήσετε το διακριτικό που είναι απαραίτητο, προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

    Αντικατάσταση {αναγνωριστικό υπολογιστή-πελάτη} με το αναγνωριστικό του Azure Active Directory εφαρμογή, {προγράμματος-πελάτη-μυστικό} με το πλήκτρο πρόσβασης της εφαρμογής AD και {μισθωτή αναγνωριστικού}, με το αναγνωριστικό μισθωτή για τη συνδρομή σας. Μπορείτε να βρείτε το αναγνωριστικό μισθωτή, εκτελώντας Get-AzureRmSubscription. Μπορείτε να βρείτε τον αριθμό-κλειδί πρόσβασης χρησιμοποιώντας την πύλη του Azure.

3. Για να καλέσετε τη μέθοδο που έχετε προσθέσει προηγουμένως, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main στο αρχείο Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Αποθηκεύστε το αρχείο Program.cs.

## <a name="step-3-register-the-resource-providers-and-create-the-resources"></a>Βήμα 3: Καταχώρηση τις υπηρεσίες παροχής πόρων και δημιουργήστε τους πόρους

### <a name="register-the-providers-and-create-a-resource-group"></a>Καταχώρηση τις υπηρεσίες παροχής και να δημιουργήσετε μια ομάδα πόρων

Όλοι οι πόροι πρέπει να περιέχονται σε μια ομάδα πόρων. Για να προσθέσετε πόρους σε μια ομάδα, πρέπει να καταχωρούνται τη συνδρομή σας με τις υπηρεσίες παροχής πόρων.

1. Προσθέστε μεταβλητές τη μέθοδο κύριες της κλάσης προγράμματος για να καθορίσετε τα ονόματα που θέλετε να χρησιμοποιήσετε για τους πόρους:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    Αντικαταστήστε όλες τις τιμές μεταβλητών με τα ονόματα και το αναγνωριστικό που θέλετε να χρησιμοποιήσετε. Μπορείτε να βρείτε το αναγνωριστικό συνδρομής, εκτελώντας Get-AzureRmSubscription.

2. Για να δημιουργήσετε την ομάδα των πόρων και να καταχωρήσετε τις υπηρεσίες παροχής, προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
            
          Console.WriteLine("Registering the providers...");
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Για να καλέσετε τη μέθοδο που έχετε προσθέσει προηγουμένως, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### <a name="create-a-storage-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης

Ένα [λογαριασμό χώρου αποθήκευσης](../storage/storage-create-storage-account.md) είναι απαραίτητο να αποθηκεύσετε το αρχείο εικονικού σκληρού δίσκου που δημιουργείται για την εικονική μηχανή.

1. Για να δημιουργήσετε το λογαριασμό χώρου αποθήκευσης, προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Για να καλέσετε τη μέθοδο που έχετε προσθέσει προηγουμένως, προσθέστε αυτόν τον κωδικό για τη μέθοδο κύριες της κλάσης προγράμματος:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-public-ip-address"></a>Δημιουργία στη δημόσια διεύθυνση IP

Απαιτείται μια δημόσια διεύθυνση IP για την επικοινωνία με την εικονική μηχανή.

1. Για να δημιουργήσετε στη δημόσια διεύθυνση IP του υπολογιστή εικονικές, προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Για να καλέσετε τη μέθοδο που έχετε προσθέσει προηγουμένως, προσθέστε αυτόν τον κωδικό για τη μέθοδο κύριες της κλάσης προγράμματος:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-virtual-network"></a>Δημιουργήστε το εικονικό δίκτυο

Μια εικονική μηχανή που δημιουργείται με το μοντέλο ανάπτυξης διαχείρισης πόρων πρέπει να είναι ένα εικονικό δίκτυο.

1. Για να δημιουργήσετε μια εικονική δικτύου και ένα υποδίκτυο, προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Για να καλέσετε τη μέθοδο που έχετε προσθέσει προηγουμένως, προσθέστε αυτόν τον κωδικό για τη μέθοδο κύριες της κλάσης προγράμματος:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### <a name="create-the-network-interface"></a>Δημιουργία του περιβάλλοντος εργασίας δικτύου

Μια εικονική μηχανή πρέπει ένα περιβάλλον εργασίας δικτύου να επικοινωνήσει με το εικονικό δίκτυο.

1. Για να δημιουργήσετε ένα περιβάλλον εργασίας δικτύου, προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Για να καλέσετε τη μέθοδο που έχετε προσθέσει προηγουμένως, προσθέστε αυτόν τον κωδικό για τη μέθοδο κύριες της κλάσης προγράμματος:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-an-availability-set"></a>Δημιουργήστε ένα σύνολο διαθεσιμότητα

Διαθεσιμότητα σύνολα διευκολύνουν για να διαχειριστείτε τη διατήρηση της τις εικονικές μηχανές που χρησιμοποιούνται από την εφαρμογή σας.

1. Για να δημιουργήσετε το σύνολο διαθεσιμότητα, προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Για να καλέσετε τη μέθοδο που έχετε προσθέσει προηγουμένως, προσθέστε αυτόν τον κωδικό για τη μέθοδο κύριες της κλάσης προγράμματος:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### <a name="create-a-virtual-machine"></a>Δημιουργήστε μια εικονική μηχανή

Τώρα που έχετε δημιουργήσει όλους τους πόρους υποστήριξης, μπορείτε να δημιουργήσετε μια εικονική μηχανή.

1. Για να δημιουργήσετε την εικονική μηχανή, προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
        }

    >[AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης δημιουργεί μια εικονική μηχανή εκτελεί μια έκδοση του λειτουργικού συστήματος των Windows Server. Για να μάθετε περισσότερα σχετικά με την επιλογή άλλες εικόνες, ανατρέξτε στο θέμα [Περιήγηση και επιλογή Azure εικονική μηχανή εικόνων με το Windows PowerShell και το Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

2. Για να καλέσετε τη μέθοδο που έχετε προσθέσει προηγουμένως, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avSetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##<a name="step-4-delete-the-resources"></a>Βήμα 4: Διαγραφή των πόρων

Επειδή που θα χρεωθείτε για τους πόρους που χρησιμοποιούνται στο Azure, είναι πάντα καλό να διαγράψετε τους πόρους που δεν χρειάζονται πλέον. Εάν θέλετε να διαγράψετε τις εικονικές μηχανές και όλους τους πόρους υποστήριξης, μόνο που έχετε να κάνετε είναι να διαγράψετε την ομάδα των πόρων.

1.  Για να διαγράψετε την ομάδα των πόρων, προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Για να καλέσετε τη μέθοδο που έχετε προσθέσει προηγουμένως, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## <a name="step-5-run-the-console-application"></a>Βήμα 5: Εκτέλεση της εφαρμογής κονσόλας

1. Για να εκτελέσετε την εφαρμογή console, κάντε κλικ στην επιλογή **Έναρξη** στο Visual Studio και, στη συνέχεια, πραγματοποιήστε είσοδο στο Azure AD χρησιμοποιώντας το ίδιο όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείτε με τη συνδρομή σας.

2. Πατήστε το πλήκτρο **Enter** μετά από κάθε κωδικός κατάστασης επιστρέφεται για να δημιουργήσετε κάθε πόρο. Αφού δημιουργηθεί η εικονική μηχανή, κάντε το επόμενο βήμα πριν πατώντας το πλήκτρο Enter για να διαγράψετε όλους τους πόρους.

    Αυτό πρέπει να διαρκέσει περίπου πέντε λεπτά για αυτήν την εφαρμογή κονσόλας για να εκτελέσετε εντελώς από την αρχή μέχρι να ολοκληρωθεί. Προτού πατήσετε το πλήκτρο Enter για να ξεκινήσετε τη διαγραφή πόρων, μπορεί να χρειαστούν μερικά λεπτά για να επιβεβαιώσετε τη δημιουργία των πόρων στην πύλη του Azure πριν από τη διαγραφή τους.

3. Για να δείτε την κατάσταση των πόρων, μεταβείτε στα αρχεία καταγραφής ελέγχου στην πύλη του Azure:

    ![Αναζήτηση αρχείων καταγραφής ελέγχου στην πύλη του Azure](./media/virtual-machines-windows-csharp/crpportal.png)
    
## <a name="next-steps"></a>Επόμενα βήματα

- Εκμεταλλευτείτε χρησιμοποιώντας ένα πρότυπο για να δημιουργήσετε μια εικονική μηχανή χρησιμοποιώντας τις πληροφορίες χρησιμοποιώντας την [Ανάπτυξη μια εικονική μηχανή Azure C# και ενός προτύπου για τη διαχείριση πόρων](virtual-machines-windows-csharp-template.md).
- Μάθετε πώς μπορείτε να διαχειριστείτε την εικονική μηχανή που δημιουργήσατε ανατρέχοντας στο θέμα [Διαχείριση εικονικές μηχανές χρήση της διαχείρισης πόρων Azure και PowerShell](virtual-machines-windows-csharp-manage.md).
