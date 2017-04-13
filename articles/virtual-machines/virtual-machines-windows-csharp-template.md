<properties
    pageTitle="Ανάπτυξη μια Εικονική χρησιμοποιώντας C# και ένα πρότυπο από διαχειριστή πόρων | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε C# και ένα πρότυπο από διαχειριστή πόρων για να αναπτύξετε μια Εικονική Azure."
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
    ms.date="10/10/2016"
    ms.author="davidmu"/>

# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Ανάπτυξη μια εικονική μηχανή Azure χρησιμοποιώντας C# και ένα πρότυπο από διαχειριστή πόρων

Χρησιμοποιώντας τις ομάδες πόρων και τα πρότυπα, είστε μπορούν να διαχειρίζονται όλους τους πόρους μαζί που υποστηρίζουν την εφαρμογή σας. Αυτό το άρθρο σας δείχνει πώς μπορείτε να χρησιμοποιήσετε Visual Studio και C# για να ρυθμίσετε τον έλεγχο ταυτότητας, δημιουργήστε ένα πρότυπο και, στη συνέχεια, ανάπτυξη Azure πόρους χρησιμοποιώντας το πρότυπο που δημιουργήσατε.

Πρέπει πρώτα να βεβαιωθείτε ότι έχετε ήδη κάνει αυτά τα βήματα για τη ρύθμιση:

- Εγκατάσταση του [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Επαληθεύστε την εγκατάσταση του [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) ή του [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Λήψη ενός [διακριτικού ελέγχου ταυτότητας](../resource-group-authenticate-service-principal.md)
- Δημιουργήστε μια ομάδα πόρων με τη χρήση [Του PowerShell Azure](../resource-group-template-deploy.md), [Azure CLI](../resource-group-template-deploy-cli.md)ή [Azure πύλη](../resource-group-template-deploy-portal.md).

Χρειάζονται περίπου 30 λεπτά για να κάνετε τα εξής βήματα.
    
## <a name="step-1-create-the-visual-studio-project-the-template-file-and-the-parameters-file"></a>Βήμα 1: Δημιουργήστε το έργο Visual Studio, το αρχείο προτύπου και στο αρχείο παραμέτρων

### <a name="create-the-template-file"></a>Δημιουργήστε το αρχείο προτύπου

Ένα πρότυπο από διαχειριστή πόρων Azure δίνει τη δυνατότητα για να αναπτύξετε και να διαχειριστείτε πόρους Azure μαζί. Το πρότυπο είναι μια περιγραφή JSON για τους πόρους και τις παραμέτρους συσχετισμένη ανάπτυξης.

Στο Visual Studio, κάντε τα εξής βήματα:

1. Κάντε κλικ στο **αρχείο** > **νέα** > **έργου**.

2. Στα **πρότυπα** > **Visual C#**, επιλέξτε **Εφαρμογή Console**, πληκτρολογήστε το όνομα και τη θέση του έργου και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

3. Κάντε δεξί κλικ στο όνομα του έργου στο Εξερεύνηση λύσεων, κάντε κλικ στην επιλογή **Προσθήκη** > **Νέο στοιχείο**.

4. Κάντε κλικ στην επιλογή Web, επιλέξτε το αρχείο JSON, εισαγάγετε *VirtualMachineTemplate.json* για το όνομα και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

5. Στη αριστερή και δεξιά αγκύλη του αρχείου VirtualMachineTemplate.json, προσθέστε το στοιχείο απαιτείται σχήματος και το στοιχείο απαιτείται contentVersion:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Παράμετροι](../resource-group-authoring-templates.md#parameters) δεν είναι πάντα απαραίτητα, αλλά παρέχουν ένας τρόπος για να εισαγάγετε τιμές, όταν έχει αναπτυχθεί το πρότυπο. Προσθέστε το στοιχείο παράμετροι και τα θυγατρικά στοιχεία μετά το στοιχείο contentVersion:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

7. [Μεταβλητές](../resource-group-authoring-templates.md#variables) μπορούν να χρησιμοποιηθούν σε ένα πρότυπο για να καθορίσετε τιμές που μπορεί να αλλάζουν συχνά ή που πρέπει να δημιουργηθεί από ένα συνδυασμό των τιμών της παραμέτρου. Προσθέστε το στοιχείο μεταβλητές μετά την ενότητα παράμετροι:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }

8. [Πόροι](../resource-group-authoring-templates.md#resources) όπως η εικονική μηχανή, το εικονικό δίκτυο και το λογαριασμό χώρου αποθήκευσης ορίζονται δίπλα στο πρότυπο. Προσθέστε την ενότητα "Πόροι" μετά από την ενότητα μεταβλητές:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvnet1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
      
9. Αποθηκεύστε το αρχείο προτύπου που δημιουργήσατε.

### <a name="create-the-parameters-file"></a>Δημιουργία του αρχείου παραμέτρων

Για να καθορίσετε τιμές για τις παραμέτρους του πόρου που έχουν οριστεί στο πρότυπο, μπορείτε να δημιουργήσετε ένα αρχείο παραμέτρων που περιέχει τις τιμές που χρησιμοποιούνται όταν έχει αναπτυχθεί το πρότυπο. Στο Visual Studio, κάντε τα εξής βήματα:

1. Κάντε δεξί κλικ στο όνομα του έργου στο Εξερεύνηση λύσεων, κάντε κλικ στην επιλογή **Προσθήκη** > **Νέο στοιχείο**.

2. Κάντε κλικ στην επιλογή Web, επιλέξτε το αρχείο JSON, εισαγάγετε *Parameters.json* για το όνομα και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

3. Ανοίξτε το αρχείο parameters.json και, στη συνέχεια, προσθέστε αυτό το περιεχόμενο JSON:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Σε αυτό το άρθρο δημιουργεί μια εικονική μηχανή εκτελεί μια έκδοση του λειτουργικού συστήματος των Windows Server. Για να μάθετε περισσότερα σχετικά με την επιλογή άλλες εικόνες, ανατρέξτε στο θέμα [Περιήγηση και επιλογή Azure εικονική μηχανή εικόνων με το Windows PowerShell και το Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

4. Αποθηκεύστε το αρχείο παραμέτρων που δημιουργήσατε.

## <a name="step-2-install-the-libraries"></a>Βήμα 2: Εγκαταστήστε τις βιβλιοθήκες

Τα πακέτα NuGet είναι ο ευκολότερος τρόπος για να εγκαταστήσετε τις βιβλιοθήκες που χρειάζεστε για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης. Χρειάζεστε τη βιβλιοθήκη Azure πόρων διαχείρισης και τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory Azure για να δημιουργήσετε τους πόρους. Για να λάβετε αυτές τις βιβλιοθήκες στο Visual Studio, κάντε τα εξής βήματα:

1. Κάντε δεξί κλικ στο όνομα του έργου στην Εξερεύνηση λύσεων, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**και, στη συνέχεια, κάντε κλικ στο κουμπί Αναζήτηση.

2. Τύπος *Υπηρεσίας καταλόγου Active Directory* στο πλαίσιο αναζήτησης, κάντε κλικ στην επιλογή **εγκατάσταση** του πακέτου βιβλιοθήκη ελέγχου ταυτότητας Active Directory και, στη συνέχεια, ακολουθήστε τις οδηγίες για να εγκαταστήσετε το πακέτο.

4. Στο επάνω μέρος της σελίδας, επιλέξτε **Περιλαμβάνουν προέκδοση**. Τύπος *Microsoft.Azure.Management.ResourceManager* στο πλαίσιο αναζήτησης, κάντε κλικ στην επιλογή **εγκατάσταση** για τις βιβλιοθήκες Microsoft Azure πόρων διαχείρισης και, στη συνέχεια, ακολουθήστε τις οδηγίες για να εγκαταστήσετε το πακέτο.

Τώρα είστε έτοιμοι να ξεκινήσετε να χρησιμοποιείτε τις βιβλιοθήκες για να δημιουργήσετε την εφαρμογή σας.

## <a name="step-3-create-the-credentials-that-are-used-to-authenticate-requests"></a>Βήμα 3: Δημιουργία τα διαπιστευτήρια που χρησιμοποιούνται για τον έλεγχο ταυτότητας αιτήσεις

Η εφαρμογή Azure Active Directory έχει δημιουργηθεί και έχει εγκατασταθεί στη βιβλιοθήκη ελέγχου ταυτότητας. Τώρα μπορείτε να μορφοποιήσετε τις πληροφορίες της εφαρμογής σε διαπιστευτηρίων που χρησιμοποιούνται για τον έλεγχο ταυτότητας αιτήσεων για τη διαχείριση πόρων Azure.

1. Ανοίξτε το αρχείο Program.cs για το έργο που δημιουργήσατε και, στη συνέχεια, προσθέστε αυτές τις χρήση προτάσεων στο επάνω μέρος του αρχείου:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;

2.  Προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος για να λάβετε το διακριτικό που είναι απαραίτητα για να δημιουργήσετε τα διαπιστευτήρια:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    Αντικατάσταση {αναγνωριστικό υπολογιστή-πελάτη} με το αναγνωριστικό του Azure Active Directory εφαρμογή, {προγράμματος-πελάτη-μυστικό} με το πλήκτρο πρόσβασης της εφαρμογής AD και {μισθωτή αναγνωριστικού}, με το αναγνωριστικό μισθωτή για τη συνδρομή σας. Μπορείτε να βρείτε το αναγνωριστικό μισθωτή, εκτελώντας Get-AzureRmSubscription. Μπορείτε να βρείτε τον αριθμό-κλειδί πρόσβασης χρησιμοποιώντας την πύλη του Azure.

3. Για να δημιουργήσετε τα διαπιστευτήρια, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main στο αρχείο Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Αποθηκεύστε το αρχείο Program.cs.

## <a name="step-4-deploy-the-template"></a>Βήμα 4: Ανάπτυξη του προτύπου

Σε αυτό το βήμα, μπορείτε να χρησιμοποιήσετε την ομάδα πόρων που δημιουργήσατε προηγουμένως, αλλά μπορείτε επίσης να δημιουργήσετε μια ομάδα πόρων με την [ομάδα πόρων](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) και οι κλάσεις [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) .

1. Προσθέστε μεταβλητές τη μέθοδο κύριες της κλάσης προγράμματος για να καθορίσετε τα ονόματα των πόρων που δημιουργήσατε προηγουμένως, το όνομα της ανάπτυξης και το αναγνωριστικό συνδρομής:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";

    Αντικαταστήστε την τιμή όνομα ομάδας με το όνομα της ομάδας πόρων. Αντικαταστήστε την τιμή του deploymentName με το όνομα που θέλετε να χρησιμοποιήσετε για την ανάπτυξη. Μπορείτε να βρείτε το αναγνωριστικό συνδρομής, εκτελώντας Get-AzureRmSubscription.

2. Προσθέστε αυτήν τη μέθοδο για την τάξη προγράμματος για να αναπτύξετε τους πόρους στην ομάδα πόρων χρησιμοποιώντας το πρότυπο που έχετε ορίσει:

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

    Εάν θέλετε να αναπτύξετε το πρότυπο από ένα λογαριασμό του χώρου αποθήκευσης, μπορείτε να αντικαταστήσετε την ιδιότητα προτύπου με την ιδιότητα TemplateLink.

3. Για να καλέσετε τη μέθοδο που μόλις προσθέσατε, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5-delete-the-resources"></a>Βήμα 5: Διαγραφή των πόρων

Επειδή που θα χρεωθείτε για τους πόρους που χρησιμοποιούνται στο Azure, είναι πάντα καλό να διαγράψετε τους πόρους που δεν χρειάζονται πλέον. Δεν χρειάζεται να διαγράψετε κάθε πόρο ξεχωριστά από μια ομάδα πόρων. Διαγράψτε την ομάδα των πόρων και όλους τους πόρους διαγράφονται αυτόματα.

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

2.  Για να καλέσετε τη μέθοδο που μόλις προσθέσατε, προσθέστε αυτόν τον κωδικό για τη μέθοδο Main:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##<a name="step-6-run-the-console-application"></a>Βήμα 6: Εκτέλεση της εφαρμογής κονσόλας

1.  Για να εκτελέσετε την εφαρμογή console, κάντε κλικ στην επιλογή **Έναρξη** στο Visual Studio και, στη συνέχεια, πραγματοποιήστε είσοδο στο Azure AD χρησιμοποιώντας τα ίδια διαπιστευτήρια που χρησιμοποιείτε με τη συνδρομή σας.

2.  Πατήστε το πλήκτρο **Enter** , αφού εμφανιστεί η κατάσταση αποδεκτοί.

    Θα πρέπει να μπορεί να χρειαστούν περίπου πέντε λεπτά για αυτήν την εφαρμογή κονσόλας για να εκτελέσετε εντελώς από την αρχή για να ολοκληρώσετε. Προτού πατήσετε το πλήκτρο Enter για να ξεκινήσετε τη διαγραφή πόρων, μπορεί να χρειαστούν μερικά λεπτά για να επιβεβαιώσετε τη δημιουργία των πόρων στην πύλη του Azure πριν από τη διαγραφή τους.

3. Για να δείτε την κατάσταση των πόρων, μεταβείτε στα αρχεία καταγραφής ελέγχου στην πύλη του Azure:

    ![Αναζήτηση αρχείων καταγραφής ελέγχου στην πύλη του Azure](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Επόμενα βήματα

- Εάν υπάρχουν προβλήματα με την ανάπτυξη, θα ήταν μια επόμενο βήμα για να δείτε τις [αναπτύξεις ομάδα πόρων αντιμετώπιση προβλημάτων με την πύλη Azure](../resource-manager-troubleshoot-deployments-portal.md).
- Μάθετε πώς μπορείτε να διαχειριστείτε την εικονική μηχανή που δημιουργήσατε ανατρέχοντας στο θέμα [Διαχείριση εικονικές μηχανές χρήση της διαχείρισης πόρων Azure και PowerShell](virtual-machines-windows-csharp-manage.md).
