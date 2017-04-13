<properties
   pageTitle="Προβολή λειτουργίες ανάπτυξης με REST API | Microsoft Azure"
   description="Περιγράφει τον τρόπο χρήσης του Azure διαχείριση πόρων REST API για τον εντοπισμό θέματα από διαχειριστή πόρων ανάπτυξης."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/13/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Λειτουργίες ανάπτυξης προβολή με Azure διαχείριση πόρων REST API

> [AZURE.SELECTOR]
- [Πύλη](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API](resource-manager-troubleshoot-deployments-rest.md)

Εάν έχετε λάβει ένα μήνυμα σφάλματος κατά την ανάπτυξη πόρων σε Azure, που μπορεί να θέλετε να δείτε περισσότερες λεπτομέρειες σχετικά με τις λειτουργίες ανάπτυξης που εκτελέστηκαν. Το REST API παρέχει λειτουργίες που σας επιτρέπουν να βρείτε τα σφάλματα και να προσδιορίσετε πιθανές διορθωτικές ενέργειες.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Μπορείτε να αποφύγετε σφάλματα κατά την επικύρωση πρότυπο και την υποδομή πριν από την ανάπτυξη. Μπορείτε επίσης να συνδεθείτε επιπλέον αίτησης και απόκρισης πληροφορίες κατά τη διάρκεια της ανάπτυξης που μπορεί να είναι χρήσιμες αργότερα για την αντιμετώπιση προβλημάτων. Για να μάθετε περισσότερα σχετικά με την επικύρωση και καταγραφή αίτησης και απόκρισης πληροφορίες, ανατρέξτε στο θέμα [ανάπτυξη μιας ομάδας πόρων με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy-rest.md).

## <a name="troubleshoot-with-rest-api"></a>Αντιμετώπιση προβλημάτων με REST API

1. Αναπτύξτε τους πόρους σας με τη λειτουργία [δημιουργίας μιας ανάπτυξης του προτύπου](https://msdn.microsoft.com/library/azure/dn790564.aspx) . Για να διατηρήσετε τις πληροφορίες που μπορεί να είναι χρήσιμες για τον εντοπισμό σφαλμάτων, ορίστε την ιδιότητα **debugSetting** στην πρόσκληση σε JSON με **requestContent** ή/και **responseContent**. 

        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
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
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }

    Από προεπιλογή, η τιμή **debugSetting** έχει οριστεί σε **κανένα**. Κατά τον καθορισμό της τιμής **debugSetting** , εξετάστε προσεκτικά τον τύπο των πληροφοριών που που περνά μέσα σε κατά την ανάπτυξη. Με πληροφορίες σύνδεσης για την αίτηση ή την απάντηση, θα μπορούσε να ενδεχομένως να εκθέτουν ευαίσθητα δεδομένα τα οποία γίνεται ανάκτηση μέσω τις λειτουργίες ανάπτυξης. 

2. Λήψη πληροφοριών σχετικά με μια ανάπτυξη του με τη λειτουργία [λήψη πληροφοριών σχετικά με μια ανάπτυξη του προτύπου](https://msdn.microsoft.com/library/azure/dn790565.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}

    Στην απάντηση, σημειώστε ιδίως τα στοιχεία **provisioningState** , **correlationId** και **σφαλμάτων** . Το **correlationId** χρησιμοποιείται για την παρακολούθηση συμβάντων για τις σχετικές και μπορεί να είναι χρήσιμο όταν εργάζεστε με την τεχνική υποστήριξη για την αντιμετώπιση προβλημάτων του ζητήματος.
    
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }

3. Λήψη πληροφοριών σχετικά με τις λειτουργίες ανάπτυξης με τη λειτουργία [λίστα όλες οι λειτουργίες ανάπτυξη προτύπου](https://msdn.microsoft.com/library/azure/dn790518.aspx) . 

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}

    Η απόκριση θα περιλαμβάνει αίτηση ή/και απάντηση πληροφορίες με βάση τι που καθορίζεται στην ιδιότητα **debugSetting** κατά την ανάπτυξη.
    
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }

4. Λήψη συμβάντα από τα αρχεία καταγραφής ελέγχου για την ανάπτυξη με τη λειτουργία [λίστα συμβάντα διαχείρισης της συνδρομής](https://msdn.microsoft.com/library/azure/dn931934.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}


## <a name="next-steps"></a>Επόμενα βήματα

- Για βοήθεια σχετικά με την επίλυση σφαλμάτων συγκεκριμένη ανάπτυξη, ανατρέξτε στο θέμα [Επίλυση συνηθισμένων σφαλμάτων κατά την ανάπτυξη πόρων για Azure με τη διαχείριση πόρων Azure](resource-manager-common-deployment-errors.md).
- Για να μάθετε σχετικά με τη χρήση των αρχείων καταγραφής ελέγχου για την παρακολούθηση της άλλους τύπους ενεργειών, ανατρέξτε στο θέμα [ελέγχου λειτουργιών με τη διαχείριση πόρων](resource-group-audit.md).
- Για να επικυρώσετε την ανάπτυξη πριν από την εκτέλεση του, ανατρέξτε στο θέμα [ανάπτυξη μιας ομάδας πόρων με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy.md).
