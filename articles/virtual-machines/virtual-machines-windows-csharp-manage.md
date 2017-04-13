<properties
    pageTitle="Διαχείριση ΣΠΣ χρήση της διαχείρισης πόρων Azure και C# | Microsoft Azure"
    description="Διαχείριση εικονικές μηχανές χρήση της διαχείρισης πόρων Azure και C#."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-azure-resource-manager-and-c"></a>Διαχείριση εικονικές μηχανές Windows Azure χρησιμοποιώντας τη διαχείριση πόρων Azure και C#  

Οι εργασίες σε αυτό το άρθρο σας δείξουν πώς μπορείτε να διαχειριστείτε εικονικές μηχανές, όπως η έναρξη, διακοπή και ενημέρωση. Πρέπει να υπάρχει μια εικονική μηχανή σε μια ομάδα πόρων για να ολοκληρώσετε τις εργασίες σε αυτό το άρθρο.

Για να ολοκληρώσετε τις εργασίες σε αυτό το άρθρο, χρειάζεστε:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Ένα διακριτικό ελέγχου ταυτότητας](../resource-group-authenticate-service-principal.md)

## <a name="create-a-visual-studio-project-and-install-packages"></a>Δημιουργήστε ένα έργο Visual Studio και εγκατάσταση πακέτων

Πακέτα NuGet είναι ο ευκολότερος τρόπος για να εγκαταστήσετε τις βιβλιοθήκες που χρειάζεστε για να ολοκληρώσετε τις εργασίες σε αυτό το άρθρο. Τις βιβλιοθήκες που εγκαθιστάτε για αυτό το άρθρο είναι η βιβλιοθήκη ελέγχου ταυτότητας Active Directory Azure και τη βιβλιοθήκη υπηρεσίας παροχής τον υπολογισμό πόρων. Ολοκληρώσετε αυτά τα βήματα για να λάβετε τις βιβλιοθήκες στο Visual Studio:

1. Κάντε κλικ στο **αρχείο** > **νέα** > **έργου**.

2. Στα **πρότυπα** > **Visual C#**, επιλέξτε **Εφαρμογή Console**, πληκτρολογήστε το όνομα και τη θέση του έργου και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

3. Κάντε δεξί κλικ στο όνομα του έργου στην Εξερεύνηση λύσεων και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.

4. Τύπος *Υπηρεσίας καταλόγου Active Directory* στο πλαίσιο αναζήτησης, κάντε κλικ στην επιλογή **εγκατάσταση** του πακέτου βιβλιοθήκη ελέγχου ταυτότητας Active Directory και, στη συνέχεια, ακολουθήστε τις οδηγίες για να εγκαταστήσετε το πακέτο.

5. Στο επάνω μέρος της σελίδας, επιλέξτε **Περιλαμβάνουν προέκδοση**. Τύπος *Microsoft.Azure.Management.Compute* στο πλαίσιο αναζήτησης, κάντε κλικ στην επιλογή **εγκατάσταση** για τις βιβλιοθήκες .NET τον υπολογισμό και, στη συνέχεια, ακολουθήστε τις οδηγίες για να εγκαταστήσετε το πακέτο.

Τώρα είστε έτοιμοι να ξεκινήσετε να χρησιμοποιείτε τις βιβλιοθήκες για να διαχειριστείτε τις εικονικές μηχανές.

## <a name="set-up-the-project"></a>Ρύθμιση του έργου

Τώρα που η εφαρμογή έχει δημιουργηθεί και έχετε εγκαταστήσει και τις βιβλιοθήκες, μπορείτε να δημιουργήσετε ένα διακριτικό χρησιμοποιώντας τις πληροφορίες της εφαρμογής. Αυτό το διακριτικό χρησιμοποιείται για τον έλεγχο ταυτότητας αιτήσεις για τη διαχείριση πόρων Azure.

1. Ανοίξτε το αρχείο Program.cs για το έργο που δημιουργήσατε και, στη συνέχεια, προσθέστε αυτές τις χρήση προτάσεων στο επάνω μέρος του αρχείου:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
        
2. Προσθέστε μεταβλητές τη μέθοδο κύριες της κλάσης προγράμματος για να καθορίσετε το όνομα της ομάδας πόρων, καθώς και το όνομα του η εικονική μηχανή και το αναγνωριστικό συνδρομής:

        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var subscriptionId = "subsciption id";

    Μπορείτε να βρείτε το αναγνωριστικό συνδρομής, εκτελώντας Get-AzureRmSubscription.
    
3. Για να λάβετε το διακριτικό που απαιτείται για να δημιουργήσετε τα διαπιστευτήρια, προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

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
    
4. Για να δημιουργήσετε τα διαπιστευτήρια, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main στο Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

5. Αποθηκεύστε το αρχείο Program.cs.

## <a name="display-information-about-a-virtual-machine"></a>Εμφάνιση πληροφοριών σχετικά με μια εικονική μηχανή

1. Προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος του έργου που δημιουργήσατε προηγουμένως:

        public static async void GetVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");

          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
            groupName, 
            vmName, 
            InstanceViewTypes.InstanceView);

          Console.WriteLine("hardwareProfile");
          Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);

          Console.WriteLine("\nstorageProfile");
          Console.WriteLine("  imageReference");
          Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
          Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
          Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
          Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
          Console.WriteLine("  osDisk");
          Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
          Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
          Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
          Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);

          Console.WriteLine("\nosProfile");
          Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
          Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
          Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
          Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);

          Console.WriteLine("\nnetworkProfile");
          foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
          {
            Console.WriteLine("  networkInterface id: " + nic.Id);
          }

          Console.WriteLine("\nvmAgent");
          Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
          Console.WriteLine("    statuses");
          foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
          {
            Console.WriteLine("    code: " + stat.Code);
            Console.WriteLine("    level: " + stat.Level);
            Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
            Console.WriteLine("    message: " + stat.Message);
            Console.WriteLine("    time: " + stat.Time);
          }

          Console.WriteLine("\ndisks");
          foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
          {
            Console.WriteLine("  name: " + idisk.Name);
            Console.WriteLine("  statuses");
            foreach (InstanceViewStatus istat in idisk.Statuses)
            {
              Console.WriteLine("    code: " + istat.Code);
              Console.WriteLine("    level: " + istat.Level);
              Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
              Console.WriteLine("    time: " + istat.Time);
            }
          }

          Console.WriteLine("\nVM general status");
          Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
          Console.WriteLine("  id: " + vmResult.Id);
          Console.WriteLine("  name: " + vmResult.Name);
          Console.WriteLine("  type: " + vmResult.Type);
          Console.WriteLine("  location: " + vmResult.Location);
          Console.WriteLine("\nVM instance status");
          foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
          {
            Console.WriteLine("\n  code: " + istat.Code);
            Console.WriteLine("  level: " + istat.Level);
            Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
          }
          
        }

2. Για να καλέσετε τη μέθοδο που μόλις προσθέσατε, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        GetVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
    
3. Αποθηκεύστε το αρχείο Program.cs.

4. Κάντε κλικ στην επιλογή **Έναρξη** στο Visual Studio και, στη συνέχεια, πραγματοποιήστε είσοδο στο Azure AD χρησιμοποιώντας το ίδιο όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείτε με τη συνδρομή σας.

    Όταν εκτελείτε αυτήν τη μέθοδο, θα πρέπει να δείτε κάτι παρόμοιο με αυτό το παράδειγμα:
    
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0

        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite

          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True

          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1

          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM

          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM

          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus

          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## <a name="stop-a-virtual-machine"></a>Διακοπή μια εικονική μηχανή

Μπορείτε να διακόψετε μια εικονική μηχανή με δύο τρόπους. Μπορείτε να διακόψετε μια εικονική μηχανή και διατήρηση όλες τις ρυθμίσεις, αλλά συνεχίσετε να χρεωθεί για αυτό, ή μπορείτε να διακόψετε μια εικονική μηχανή και να καταργήσετε την εκχώρηση του. Όταν μια εικονική μηχανή κατάργηση εκχώρησης, όλους τους πόρους που σχετίζονται με αυτό είναι επίσης τελειώνει κατάργηση εκχώρησης και χρέωσης για αυτήν.

1. Σχόλιο οποιαδήποτε κωδικού που προσθέσατε προηγουμένως τη μέθοδο κύριες, εκτός από τον κωδικό για να λάβει τα διαπιστευτήρια.

2. Προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async void StopVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
        }

    Εάν θέλετε να καταργήσετε την εκχώρηση η εικονική μηχανή, αλλάξτε την κλήση σβησίματος σε αυτόν τον κώδικα:

        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);

3. Για να καλέσετε τη μέθοδο που μόλις προσθέσατε, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        StopVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Αποθηκεύστε το αρχείο Program.cs.

5. Κάντε κλικ στην επιλογή **Έναρξη** στο Visual Studio και, στη συνέχεια, πραγματοποιήστε είσοδο στο Azure AD χρησιμοποιώντας το ίδιο όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείτε με τη συνδρομή σας.

    Θα πρέπει να μπορείτε να δείτε την κατάσταση της αλλαγής εικονική μηχανή για διακοπή. Εάν εκτελέσατε τη μέθοδο που έχει διακοπεί κλήσης Deallocate, την κατάσταση (Κατάργηση εκχώρησης).

## <a name="start-a-virtual-machine"></a>Ξεκινήστε μια εικονική μηχανή

1. Σχόλιο οποιαδήποτε κωδικού που προσθέσατε προηγουμένως τη μέθοδο κύριες, εκτός από τον κωδικό για να λάβει τα διαπιστευτήρια.

2. Προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async void StartVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
        }

3. Για να καλέσετε τη μέθοδο που μόλις προσθέσατε, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        StartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Αποθηκεύστε το αρχείο Program.cs.

5. Κάντε κλικ στην επιλογή **Έναρξη** στο Visual Studio και, στη συνέχεια, πραγματοποιήστε είσοδο στο Azure AD χρησιμοποιώντας το ίδιο όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείτε με τη συνδρομή σας.

    Θα πρέπει να βλέπετε την κατάσταση του η εικονική μηχανή αλλαγή σε λειτουργία.

## <a name="restart-a-running-virtual-machine"></a>Επανεκκινήστε μια εικονική μηχανή που εκτελείται

1. Σχόλιο οποιαδήποτε κωδικού που προσθέσατε προηγουμένως τη μέθοδο κύριες, εκτός από τον κωδικό για να λάβει τα διαπιστευτήρια.

2. Προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async void RestartVirtualMachineAsync(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.RestartAsync(groupName, vmName);
        }

3. Για να καλέσετε τη μέθοδο που μόλις προσθέσατε, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        RestartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Αποθηκεύστε το αρχείο Program.cs.

5. Κάντε κλικ στην επιλογή **Έναρξη** στο Visual Studio και, στη συνέχεια, πραγματοποιήστε είσοδο στο Azure AD χρησιμοποιώντας το ίδιο όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείτε με τη συνδρομή σας.

## <a name="resize-a-virtual-machine"></a>Αλλαγή μεγέθους μια εικονική μηχανή

Αυτό το παράδειγμα δείχνει πώς μπορείτε να αλλάξετε το μέγεθος του μια εικονική μηχανή εκτελείται.

1. Σχόλια οποιαδήποτε κωδικού που προσθέσατε προηγουμένως τη μέθοδο κύριες, εκτός από τον κωδικό για να λάβει τα διαπιστευτήρια.

2. Προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async void UpdateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Για να καλέσετε τη μέθοδο που μόλις προσθέσατε, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        UpdateVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Αποθηκεύστε το αρχείο Program.cs.

5. Κάντε κλικ στην επιλογή **Έναρξη** στο Visual Studio και, στη συνέχεια, πραγματοποιήστε είσοδο στο Azure AD χρησιμοποιώντας το ίδιο όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείτε με τη συνδρομή σας.

    Θα πρέπει να βλέπετε το μέγεθος του την αλλαγή εικονική μηχανή Standard_A1.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Προσθέσετε ένα δίσκο δεδομένων σε μια εικονική μηχανή

Αυτό το παράδειγμα δείχνει πώς μπορείτε να προσθέσετε ένα δίσκο δεδομένων σε μια εικονική μηχανή εκτελείται.

1. Σχόλιο οποιαδήποτε κωδικού που προσθέσατε προηγουμένως τη μέθοδο κύριες, εκτός από τον κωδικό για να λάβει τα διαπιστευτήρια.

2. Προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async void AddDataDiskAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Adding the disk to the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.StorageProfile.DataDisks.Add(
            new DataDisk
              {
                Lun = 0,
                Name = "mydatadisk1",
                Vhd = new VirtualHardDisk
                  {
                    Uri = "https://mystorage1.blob.core.windows.net/vhds/mydatadisk1.vhd"
                  },
                CreateOption = DiskCreateOptionTypes.Empty,
                DiskSizeGB = 2,
                Caching = CachingTypes.ReadWrite
              });
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Για να καλέσετε τη μέθοδο που μόλις προσθέσατε, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        AddDataDiskAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Αποθηκεύστε το αρχείο Program.cs.

5. Κάντε κλικ στην επιλογή **Έναρξη** στο Visual Studio και, στη συνέχεια, πραγματοποιήστε είσοδο στο Azure AD χρησιμοποιώντας το ίδιο όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείτε με τη συνδρομή σας.

## <a name="delete-a-virtual-machine"></a>Διαγράψτε μια εικονική μηχανή

1. Σχόλιο οποιαδήποτε κωδικού που προσθέσατε προηγουμένως τη μέθοδο κύριες, εκτός από τον κωδικό για να λάβει τα διαπιστευτήρια.

2. Προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος:

        public static async void DeleteVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.DeleteAsync(groupName, vmName);
        }

3. Για να καλέσετε τη μέθοδο που μόλις προσθέσατε, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        DeleteVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Αποθηκεύστε το αρχείο Program.cs.

5. Κάντε κλικ στην επιλογή **Έναρξη** στο Visual Studio και, στη συνέχεια, πραγματοποιήστε είσοδο στο Azure AD χρησιμοποιώντας το ίδιο όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείτε με τη συνδρομή σας.

## <a name="next-steps"></a>Επόμενα βήματα

Εάν υπάρχουν ζητήματα με μια ανάπτυξη, ενδέχεται να κοιτάξετε [αναπτύξεις ομάδα πόρων αντιμετώπιση προβλημάτων με την πύλη Azure](../resource-manager-troubleshoot-deployments-portal.md)
