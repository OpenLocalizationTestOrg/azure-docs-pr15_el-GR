<properties
    pageTitle="Ανάπτυξη μηχανικής εκμάθησης χώρου εργασίας με χρήση προτύπου για τη διαχείριση πόρων Azure | Microsoft Azure"
    description="Πώς μπορείτε να αναπτύξετε ένα χώρο εργασίας για Azure μηχανικής εκμάθησης χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure"
    services="machine-learning"
    documentationCenter=""
    authors="ahgyger"
    manager="haining"
    editor="garye"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="ahgyger"/>
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Ανάπτυξη μηχανικής εκμάθησης χώρου εργασίας με χρήση του Azure Manager πόρων

## <a name="introduction"></a>Εισαγωγή
Χρησιμοποιώντας ένα διαχειριστή πόρων Azure πρότυπο ανάπτυξης εξοικονομεί χρόνο που, παρέχοντάς σας με τρόπο για να αναπτύξετε διασυνδεδεμένων στοιχεία με μια επικύρωση και προσπαθήστε ξανά να μηχανισμό. Για να ρυθμίσετε το Azure μηχανικής εκμάθησης τους χώρους εργασίας, για παράδειγμα, πρέπει να πρώτα να ρυθμίσετε ένα λογαριασμό Azure χώρου αποθήκευσης και, στη συνέχεια, αναπτύξτε το χώρο εργασίας. Φανταστείτε αυτή η ενέργεια με μη αυτόματο τρόπο για εκατοντάδες χώρους εργασίας. Πιο εύκολη εναλλακτική λύση είναι να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure για να αναπτύξετε ένα χώρο εργασίας Azure μηχανικής εκμάθησης και όλες τις εξαρτήσεις του. Σε αυτό το άρθρο σάς καθοδηγεί αυτήν τη διαδικασία βήμα προς βήμα. Για μια εξαιρετική Επισκόπηση της διαχείρισης πόρων Azure, ανατρέξτε στο θέμα [Επισκόπηση της διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Βήμα προς βήμα: δημιουργία χώρου εργασίας μηχανικής εκμάθησης
Θα θα Δημιουργία ομάδας του Azure πόρων και κατόπιν να αναπτύξετε ένα νέο λογαριασμό Azure χώρου αποθήκευσης και νέου Azure μηχανικής εκμάθησης χώρου εργασίας χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων. Μόλις ολοκληρωθεί η ανάπτυξη, θα σας θα εκτυπώσετε σημαντικές πληροφορίες σχετικά με τους χώρους εργασίας που έχουν δημιουργηθεί (το πρωτεύον κλειδί, το workspaceID και τη διεύθυνση URL για το χώρο εργασίας).

### <a name="create-an-azure-resource-manager-template"></a>Δημιουργία προτύπου από διαχειριστή πόρων Azure
Ένα χώρο εργασίας μηχανικής εκμάθησης απαιτεί ένα λογαριασμό Azure χώρου αποθήκευσης για την αποθήκευση του συνόλου δεδομένων που είναι συνδεδεμένα με αυτό.
Το ακόλουθο πρότυπο χρησιμοποιεί το όνομα της ομάδας πόρων για να δημιουργήσετε το όνομα λογαριασμού του χώρου αποθήκευσης και το όνομα του χώρου εργασίας.  Επίσης, χρησιμοποιεί το όνομα του λογαριασμού χώρου αποθήκευσης ως ιδιότητα κατά τη δημιουργία του χώρου εργασίας.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Αποθήκευση αυτού του προτύπου ως αρχείο mlworkspace.json στην περιοχή c:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Αναπτύξτε την ομάδα πόρων, με βάση το πρότυπο
* Άνοιγμα του PowerShell
* Εγκατάσταση λειτουργικές μονάδες για διαχείριση πόρων Azure και διαχείριση της υπηρεσίας Azure  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Αυτά τα βήματα, λήψη και εγκατάσταση των λειτουργικών μονάδων είναι απαραίτητο για να ολοκληρώσετε τα υπόλοιπα βήματα. Αυτό πρέπει να γίνει μία φορά στο περιβάλλον όπου εκτελείτε τις εντολές του PowerShell.   

* Ο έλεγχος ταυτότητας Azure  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Αυτό το βήμα πρέπει να επαναληφθεί για κάθε περίοδο λειτουργίας. Αφού ελεγχθεί η ταυτότητά, πρέπει να εμφανίζεται πληροφορίες τη συνδρομή σας.

![Λογαριασμός Azure][1]

Τώρα που έχουμε πρόσβαση Azure, μπορούμε να δημιουργήσουμε την ομάδα των πόρων.

* Δημιουργήστε μια ομάδα πόρων

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Βεβαιωθείτε ότι η ομάδα πόρων παρέχεται σωστά. **ProvisioningState** πρέπει να είναι "ολοκληρώθηκε με επιτυχία."
Το όνομα της ομάδας πόρων χρησιμοποιείται από το πρότυπο για να δημιουργήσετε το όνομα λογαριασμού του χώρου αποθήκευσης. Το όνομα του λογαριασμού χώρου αποθήκευσης πρέπει να είναι μεταξύ 3 και 24 χαρακτήρες και χρησιμοποιήστε αριθμούς και πεζά γράμματα μόνο.

![Ομάδα πόρων][2]

* Χρησιμοποιώντας την ανάπτυξη ομάδα πόρων, να αναπτύξετε ένα νέο χώρο εργασίας μηχανικής εκμάθησης.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Μόλις ολοκληρωθεί η ανάπτυξη, είναι άμεση πρόσβαση ιδιότητες του χώρου εργασίας που αναπτύσσεται. Για παράδειγμα, μπορείτε να αποκτήσετε πρόσβαση το πρωτεύον κλειδί διακριτικού.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Ένας άλλος τρόπος για να ανακτήσετε τα διακριτικά από υπάρχοντα χώρο εργασίας είναι να χρησιμοποιήσετε την εντολή Ενεργοποίηση AzureRmResourceAction. Για παράδειγμα, μπορείτε να παραθέσετε τα διακριτικά κύριας και δευτερεύουσας από όλους τους χώρους εργασίας.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Μετά την παροχή της υπηρεσίας στο χώρο εργασίας, μπορείτε επίσης να αυτοματοποιήσετε πολλές εργασίες Azure μηχανικής εκμάθησης Studio χρήση του [PowerShell λειτουργική μονάδα για Azure μηχανικής εκμάθησης](http://aka.ms/amlps).

## <a name="next-steps"></a>Επόμενα βήματα 
* Μάθετε περισσότερα σχετικά με τα [Πρότυπα για τη διαχείριση πόρων σύνταξης Azure](../resource-group-authoring-templates.md). 
* Έχετε μια ματιά στο [Αποθετήριο δεδομένων πρότυπα γρήγορη έναρξη Azure](https://github.com/Azure/azure-quickstart-templates). 
* Παρακολουθήστε αυτό το βίντεο σχετικά με τη [Διαχείριση πόρων Azure](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 
 
<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
