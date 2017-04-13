<properties
   pageTitle="Προβολή λειτουργίες ανάπτυξη με το PowerShell | Microsoft Azure"
   description="Περιγράφει τον τρόπο χρήσης του PowerShell Azure για τον εντοπισμό θέματα από διαχειριστή πόρων ανάπτυξης."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-powershell"></a>Προβολή λειτουργίες ανάπτυξη με το Azure PowerShell

> [AZURE.SELECTOR]
- [Πύλη](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API](resource-manager-troubleshoot-deployments-rest.md)

Μπορείτε να προβάλετε τις εργασίες για μια ανάπτυξη μέσω του PowerShell Azure. Ενδέχεται να πιο χρήσιμο να προβάλετε τις λειτουργίες όταν έχετε λάβει ένα μήνυμα σφάλματος κατά την ανάπτυξη, ώστε να σε αυτό το άρθρο εστιάζει στην προβολή λειτουργίες που έχουν αποτύχει. PowerShell παρέχει cmdlet που σας επιτρέπουν να βρείτε τα σφάλματα και να προσδιορίσετε πιθανά επιδιορθώσεις εύκολα.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Μπορείτε να αποφύγετε σφάλματα κατά την επικύρωση πρότυπο και την υποδομή πριν από την ανάπτυξη. Μπορείτε επίσης να συνδεθείτε επιπλέον αίτησης και απόκρισης πληροφορίες κατά τη διάρκεια της ανάπτυξης που μπορεί να είναι χρήσιμες αργότερα για την αντιμετώπιση προβλημάτων. Για να μάθετε περισσότερα σχετικά με την επικύρωση και καταγραφή αίτησης και απόκρισης πληροφορίες, ανατρέξτε στο θέμα [ανάπτυξη μιας ομάδας πόρων με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy.md).

## <a name="use-deployment-operations-to-troubleshoot"></a>Χρήση λειτουργιών ανάπτυξης για την αντιμετώπιση προβλημάτων

1. Για να λάβετε τη γενική κατάσταση μιας ανάπτυξης, χρησιμοποιήστε την εντολή **Get-AzureRmResourceGroupDeployment** . Μπορείτε να φιλτράρετε τα αποτελέσματα για μόνο αυτές τις αναπτύξεις που έχουν αποτύχει.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Που επιστρέφει το αποτυχίας αναπτύξεις με την εξής μορφή:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Κάθε ανάπτυξης είναι συνήθως αποτελείται από πολλά λειτουργίες, με κάθε λειτουργίας που αναπαριστά ένα βήμα της διαδικασίας ανάπτυξης. Για να ανακαλύψετε τι δεν πήγε καλά με μια ανάπτυξη, συνήθως πρέπει να δείτε λεπτομέρειες σχετικά με τις λειτουργίες ανάπτυξης. Μπορείτε να δείτε την κατάσταση των εργασιών με **Get-AzureRmResourceGroupDeploymentOperation**.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Που επιστρέφει πολλές εργασίες με κάθε μία με την εξής μορφή:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. Για να δείτε περισσότερες λεπτομέρειες σχετικά με τις λειτουργίες αποτυχίας, η ανάκτηση των ιδιοτήτων για λειτουργίες με κατάσταση **απέτυχε** .

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Που επιστρέφει όλες τις λειτουργίες αποτυχίας με κάθε μία με την εξής μορφή:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Σημειώστε το Αναγνωριστικό παρακολούθησης για τη λειτουργία. Θα χρησιμοποιήσετε που στο επόμενο βήμα για εστίαση σε μια συγκεκριμένη λειτουργία.

4. Για να λάβετε το μήνυμα κατάστασης μιας συγκεκριμένης αποτυχίας λειτουργίας, χρησιμοποιήστε την ακόλουθη εντολή:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Που επιστρέφει:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## <a name="use-audit-logs-to-troubleshoot"></a>Χρήση των αρχείων καταγραφής ελέγχου για την αντιμετώπιση προβλημάτων

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Για να δείτε τα σφάλματα για μια ανάπτυξη, χρησιμοποιήστε τα παρακάτω βήματα:

1. Για να ανακτήσετε εγγραφές καταγραφής, εκτελέστε την εντολή **Get-AzureRmLog** . Μπορείτε να χρησιμοποιήσετε τις παραμέτρους **ομάδα πόρων** και την **κατάσταση** για να επιστρέψετε μόνο τα συμβάντα που απέτυχε για μια ομάδα πόρων μόνο. Εάν δεν καθορίσετε μια ώρα έναρξης και λήξης, επιστρέφονται καταχωρήσεις για την τελευταία ώρα.
Για παράδειγμα, για να ανακτήσετε εκτελέστε τις λειτουργίες αποτυχίας για την τελευταία ώρα:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    Μπορείτε να καθορίσετε ένα συγκεκριμένο χρονικό διάστημα. Στο επόμενο παράδειγμα, θα δούμε για αποτυχίας ενέργειες για την τελευταία ημέρα. 

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Εναλλακτικά, μπορείτε να ορίσετε ένα ακριβές έναρξης και ώρα λήξης για αποτυχίας ενέργειες:

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Εάν αυτή η εντολή επιστρέφει πάρα πολλές εγγραφές και τις ιδιότητες, μπορείτε να εστιάσετε τις προσπάθειες ελέγχου από την ανάκτηση της ιδιότητας **Ιδιότητες** . Θα, επίσης, να περιλαμβάνουν την παράμετρο **DetailedOutput** για να δείτε τα μηνύματα σφάλματος.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Που επιστρέφει ιδιότητες των εγγραφών του αρχείου με την εξής μορφή:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Ας που βασίζονται σε αυτά τα αποτελέσματα, η εστίαση στο δεύτερο στοιχείο. Μπορείτε να περιορίσετε τα αποτελέσματα, εξετάζοντας το μήνυμα κατάστασης για αυτή την καταχώρηση.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Που επιστρέφει:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## <a name="next-steps"></a>Επόμενα βήματα

- Για βοήθεια σχετικά με την επίλυση σφαλμάτων συγκεκριμένη ανάπτυξη, ανατρέξτε στο θέμα [Επίλυση συνηθισμένων σφαλμάτων κατά την ανάπτυξη πόρων για Azure με τη διαχείριση πόρων Azure](resource-manager-common-deployment-errors.md).
- Για να μάθετε σχετικά με τη χρήση των αρχείων καταγραφής ελέγχου για την παρακολούθηση της άλλους τύπους ενεργειών, ανατρέξτε στο θέμα [ελέγχου λειτουργιών με τη διαχείριση πόρων](resource-group-audit.md).
- Για να επικυρώσετε την ανάπτυξη πριν από την εκτέλεση του, ανατρέξτε στο θέμα [ανάπτυξη μιας ομάδας πόρων με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy.md).

