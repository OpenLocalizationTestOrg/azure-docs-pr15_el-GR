<properties
   pageTitle="Ανάπτυξη των πόρων με REST API και το πρότυπο | Microsoft Azure"
   description="Διαχείριση πόρων Azure χρήση και διαχείριση πόρων REST API για να αναπτύξετε ένα πόρους σε Azure. Τους πόρους που ορίζονται σε ένα πρότυπο από διαχειριστή πόρων."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Ανάπτυξη των πόρων με πρότυπα διαχείρισης πόρων και διαχείριση πόρων REST API

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Πύλη](resource-group-template-deploy-portal.md)
- [REST API](resource-group-template-deploy-rest.md)

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να χρησιμοποιήσετε το REST API διαχείρισης πόρων με τα πρότυπα για τη διαχείριση πόρων για να αναπτύξετε το παράθυρο των πόρων σε Azure.  

> [AZURE.TIP] Για βοήθεια σχετικά με τον εντοπισμό σφαλμάτων σε ένα μήνυμα σφάλματος κατά την ανάπτυξη, ανατρέξτε στο θέμα:
>
> - [Λειτουργίες ανάπτυξης προβολή με REST API](resource-manager-troubleshoot-deployments-rest.md) για να μάθετε περισσότερα σχετικά με τη λήψη πληροφοριών που σας βοηθά να αντιμετώπιση προβλημάτων με το σφάλμα
> - [Αντιμετώπιση προβλημάτων συνηθισμένων σφαλμάτων κατά την ανάπτυξη πόρων για Azure με τη Διαχείριση Azure πόρων](resource-manager-common-deployment-errors.md) για να μάθετε τον τρόπο επίλυσης κοινών σφαλμάτων ανάπτυξης

Το πρότυπο μπορεί να είναι είτε ένα τοπικό αρχείο ή ένα εξωτερικό αρχείο που είναι διαθέσιμα μέσω ενός URI. Όταν το πρότυπό σας βρίσκεται σε ένα λογαριασμό του χώρου αποθήκευσης, μπορείτε να περιορίσετε την πρόσβαση στο πρότυπο και να παρέχουν ένα διακριτικό υπογραφής (συσχετισμών Ασφαλείας) κοινόχρηστο πρόσβασης κατά την ανάπτυξη.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Ανάπτυξη με το REST API
1. Ορισμός [κοινών παραμέτρους και κεφαλίδες](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), συμπεριλαμβανομένων κωδικοί ελέγχου ταυτότητας.
2. Εάν δεν έχετε μια υπάρχουσα ομάδα πόρων, δημιουργήστε μια ομάδα πόρων. Δώστε το αναγνωριστικό συνδρομής, το όνομα της νέας ομάδας πόρων και θέση που χρειάζεστε για τη λύση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία μιας ομάδας πόρων](https://msdn.microsoft.com/library/azure/dn790525.aspx).

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. Επικυρώστε την ανάπτυξη πριν από την εκτέλεση του με την εκτέλεση της λειτουργίας [επικύρωση μια ανάπτυξη του προτύπου](https://msdn.microsoft.com/library/azure/dn790547.aspx) . Κατά τη δοκιμή ανάπτυξης, δώστε τις παραμέτρους ακριβώς όπως θα κάνατε κατά την εκτέλεση της ανάπτυξης (απεικονίζεται στο επόμενο βήμα).

3. Δημιουργήστε μια ανάπτυξη. Δώστε το αναγνωριστικό συνδρομής, το όνομα της ομάδας πόρων για την ανάπτυξη, το όνομα της ανάπτυξης, καθώς και μια σύνδεση στο πρότυπό σας. Για πληροφορίες σχετικά με το αρχείο προτύπου, ανατρέξτε [στο αρχείο παραμέτρων](#parameter-file). Για περισσότερες πληροφορίες σχετικά με το REST API για να δημιουργήσετε μια ομάδα πόρων, ανατρέξτε στο θέμα [Δημιουργία μιας ανάπτυξης του προτύπου](https://msdn.microsoft.com/library/azure/dn790564.aspx). Παρατηρήστε ότι η **λειτουργία** έχει οριστεί σε **προσαύξηση**. Για να εκτελέσετε μια πλήρη ανάπτυξη, ορίστε **λειτουργία** **ολοκληρώθηκε**. Να είστε προσεκτικοί όταν χρησιμοποιείτε την πλήρη λειτουργία, όπως κατά λάθος, μπορείτε να διαγράψετε τους πόρους που δεν βρίσκονται στο πρότυπό σας.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",
              }
            }
          }
   
      Εάν θέλετε να καταγράψετε περιεχόμενο απόκρισης, αίτηση περιεχομένου ή και τα δύο, συμπεριλάβετε **debugSetting** στην πρόσκληση σε.

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      Μπορείτε να ρυθμίσετε το λογαριασμό χώρου αποθήκευσης για να χρησιμοποιήσετε μια κοινόχρηστη πρόσβαση διακριτικό υπογραφής (συσχετισμών Ασφαλείας). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ανάθεση πρόσβαση με υπογραφή πρόσβασης σε κοινή χρήση](https://msdn.microsoft.com/library/ee395415.aspx).

4. Λάβετε την κατάσταση της ανάπτυξης πρότυπο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [λήψη πληροφοριών σχετικά με μια ανάπτυξη του προτύπου](https://msdn.microsoft.com/library/azure/dn790565.aspx).

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Επόμενα βήματα
- Για ένα παράδειγμα ανάπτυξης πόρους μέσω της βιβλιοθήκης του προγράμματος-πελάτη .NET, ανατρέξτε στο θέμα [Χρήση πόρους ανάπτυξης των βιβλιοθηκών .NET και ένα πρότυπο](virtual-machines/virtual-machines-windows-csharp-template.md).
- Για να ορίσετε τις παραμέτρους στο πρότυπο, ανατρέξτε στο θέμα [πρότυπα σύνταξη από κοινού](resource-group-authoring-templates.md#parameters).
- Για οδηγίες σχετικά με την ανάπτυξη της λύσης σε διαφορετικά περιβάλλοντα, ανατρέξτε στο θέμα [ανάπτυξη και περιβάλλοντα δοκιμής στο Microsoft Azure](solution-dev-test-environments.md).
- Για λεπτομέρειες σχετικά με τη χρήση μιας αναφοράς KeyVault για τη μεταβίβαση ασφαλούς τιμών, ανατρέξτε στο θέμα [μεταβιβάζουν ασφαλούς τιμές κατά την ανάπτυξη](resource-manager-keyvault-parameter.md).
