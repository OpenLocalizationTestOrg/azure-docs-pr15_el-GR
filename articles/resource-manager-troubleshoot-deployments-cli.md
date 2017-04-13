<properties
   pageTitle="Προβολή λειτουργίες ανάπτυξης με Azure CLI | Microsoft Azure"
   description="Περιγράφει τον τρόπο χρήσης του Azure CLI για τον εντοπισμό θέματα από διαχειριστή πόρων ανάπτυξης."
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
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-cli"></a>Λειτουργίες ανάπτυξης προβολή με Azure CLI

> [AZURE.SELECTOR]
- [Πύλη](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API](resource-manager-troubleshoot-deployments-rest.md)

Εάν έχετε λάβει ένα μήνυμα σφάλματος κατά την ανάπτυξη πόρων σε Azure, που μπορεί να θέλετε να δείτε περισσότερες λεπτομέρειες σχετικά με τις λειτουργίες ανάπτυξης που εκτελέστηκαν. Azure CLI παρέχει εντολές που σας επιτρέπουν να βρείτε τα σφάλματα και να προσδιορίσετε πιθανές διορθωτικές ενέργειες.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Μπορείτε να αποφύγετε σφάλματα κατά την επικύρωση πρότυπο και την υποδομή πριν από την ανάπτυξη. Μπορείτε επίσης να συνδεθείτε επιπλέον αίτησης και απόκρισης πληροφορίες κατά τη διάρκεια της ανάπτυξης που μπορεί να είναι χρήσιμες αργότερα για την αντιμετώπιση προβλημάτων. Για να μάθετε περισσότερα σχετικά με την επικύρωση και καταγραφή αίτησης και απόκρισης πληροφορίες, ανατρέξτε στο θέμα [ανάπτυξη μιας ομάδας πόρων με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy-cli.md).

## <a name="use-audit-logs-to-troubleshoot"></a>Χρήση των αρχείων καταγραφής ελέγχου για την αντιμετώπιση προβλημάτων

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Για να δείτε τα σφάλματα για μια ανάπτυξη, χρησιμοποιήστε τα παρακάτω βήματα:

1. Για να δείτε τα αρχεία καταγραφής ελέγχου, εκτελέστε την εντολή **Εμφάνιση καταγραφής azure ομάδας** . Μπορείτε να συμπεριλάβετε το **--τελευταία ανάπτυξης** την επιλογή για να ανακτήσετε μόνο το αρχείο καταγραφής για το πιο πρόσφατο ανάπτυξη.

        azure group log show ExampleGroup --last-deployment

2. Η εντολή **Εμφάνιση αρχείο καταγραφής της ομάδας azure** επιστρέφει πολλές πληροφορίες. Για την αντιμετώπιση προβλημάτων, συνήθως θέλετε στην εστίαση σε λειτουργίες που απέτυχε. Η ακόλουθη δέσμη ενεργειών χρησιμοποιεί την επιλογή **--json** και το βοηθητικό πρόγραμμα JSON [jq](https://stedolan.github.io/jq/) για την αναζήτηση του αρχείου καταγραφής για ανάπτυξη αποτυχίες.

        azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'
        
        {
        "claims": {
          "aud": "https://management.core.windows.net/",
          "iss": "https://sts.windows.net/<guid>/",
          "iat": "1442510510",
          "nbf": "1442510510",
          "exp": "1442514410",
          "ver": "1.0",
          "http://schemas.microsoft.com/identity/claims/tenantid": "{guid}",
          "http://schemas.microsoft.com/identity/claims/objectidentifier": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
          "puid": "XXXXXXXXXXXXXXXX",
          "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",
          "altsecid": "1:example.com:XXXXXXXXXXX",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string="">",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
          "name": "Tom FitzMacken",
          "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
          "groups": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
          "wids": "{guid}",
          "appid": "{guid}",
          "appidacr": "0",
          "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
          "http://schemas.microsoft.com/claims/authnclassreference": "1",
          "ipaddr": "000.000.000.000"
        },
        "properties": {
          "statusCode": "Conflict",
          "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
            mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
            \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
        },
        ...

    Μπορείτε να δείτε **Ιδιότητες** περιλαμβάνει πληροφορίες στο json σχετικά με τη λειτουργία απέτυχε.

    Μπορείτε να χρησιμοποιήσετε το **--λεπτομερής** και **- vv** επιλογές για να δείτε περισσότερες πληροφορίες από τα αρχεία καταγραφής.  Χρησιμοποιήστε το **--λεπτομερής** την επιλογή για να εμφανίσετε τα βήματα που περιγράφονται οι λειτουργίες μετάβαση μέσω `stdout`. Για μια πλήρη αίτηση ιστορικό, χρησιμοποιήστε την επιλογή **- vv** . Τα μηνύματα παρέχουν συχνά σημαντική ενδείξεις σχετικά με την αιτία οποιαδήποτε αποτυχιών.

3. Για να εστιάσετε σε το μήνυμα κατάστασης για τις καταχωρήσεις αποτυχίας, χρησιμοποιήστε την ακόλουθη εντολή:

        azure group log show ExampleGroup --json | jq -r ".[] | select(.status.value == \"Failed\") | .properties.statusMessage"


## <a name="use-deployment-operations-to-troubleshoot"></a>Χρήση λειτουργιών ανάπτυξης για την αντιμετώπιση προβλημάτων

1. Λάβετε τη συνολική κατάσταση εγγραφής με την εντολή **Εμφάνιση azure ομάδα ανάπτυξης** . Στο παρακάτω παράδειγμα έχει αποτύχει η ανάπτυξη.

        azure group deployment show --resource-group ExampleGroup --name ExampleDeployment
        
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : ExampleDeployment
        data:    ResourceGroupName  : ExampleGroup
        data:    ProvisioningState  : Failed
        data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
        data:    Mode               : Incremental
        data:    Name             Type    Value
        data:    ---------------  ------  ------------
        data:    siteName         String  ExampleSite
        data:    hostingPlanName  String  ExamplePlan
        data:    siteLocation     String  West US
        data:    sku              String  Free
        data:    workerSize       String  0
        info:    group deployment show command OK

2. Για να δείτε το μήνυμα για αποτυχίας λειτουργίες για μια ανάπτυξη, χρησιμοποιήστε:

        azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json  | jq ".[] | select(.properties.provisioningState == \"Failed\") | .properties.statusMessage.Message"


## <a name="next-steps"></a>Επόμενα βήματα

- Για βοήθεια σχετικά με την επίλυση σφαλμάτων συγκεκριμένη ανάπτυξη, ανατρέξτε στο θέμα [Επίλυση συνηθισμένων σφαλμάτων κατά την ανάπτυξη πόρων για Azure με τη διαχείριση πόρων Azure](resource-manager-common-deployment-errors.md).
- Για να μάθετε σχετικά με τη χρήση των αρχείων καταγραφής ελέγχου για την παρακολούθηση της άλλους τύπους ενεργειών, ανατρέξτε στο θέμα [ελέγχου λειτουργιών με τη διαχείριση πόρων](resource-group-audit.md).
- Για να επικυρώσετε την ανάπτυξη πριν από την εκτέλεση του, ανατρέξτε στο θέμα [ανάπτυξη μιας ομάδας πόρων με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy.md).
