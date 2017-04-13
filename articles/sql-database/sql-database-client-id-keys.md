<properties
   pageTitle="Λάβετε τις απαιτούμενες τιμές για τον έλεγχο ταυτότητας μια εφαρμογή για να αποκτήσετε πρόσβαση σε βάση δεδομένων SQL από τον κώδικα | Microsoft Azure"
   description="Δημιουργήστε ένα κεφάλαιο υπηρεσία για την πρόσβαση σε βάση δεδομένων SQL από τον κώδικα."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Λάβετε τις απαιτούμενες τιμές για τον έλεγχο ταυτότητας μια εφαρμογή για να αποκτήσετε πρόσβαση σε βάση δεδομένων SQL από τον κώδικα

Για να δημιουργήσετε και να διαχειριστείτε βάσης δεδομένων SQL από τον κώδικα πρέπει να καταχωρήσετε την εφαρμογή σας στον τομέα Azure Active Directory (AAD) στην συνδρομής όπου έχουν δημιουργηθεί Azure τους πόρους σας.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Δημιουργία μιας υπηρεσίας κεφαλαίου για πρόσβαση σε πόρους από μια εφαρμογή

Πρέπει να έχετε την πιο πρόσφατη του [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) εγκατασταθεί και εκτελείται. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

Η ακόλουθη δέσμη ενεργειών PowerShell δημιουργεί εφαρμογή του Active Directory (AD) και του αρχικού υπηρεσιών που θα πρέπει να ελέγχουν την ταυτότητα μας εφαρμογή C#. Η δέσμη ενεργειών εξόδους τιμών χρειαζόμαστε για το προηγούμενο παράδειγμα C#. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure για να δημιουργήσετε μια υπηρεσία κεφαλαίου για να αποκτήσετε πρόσβαση σε πόρους](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Δείτε επίσης

- [Δημιουργήστε μια βάση δεδομένων SQL με C#](sql-database-get-started-csharp.md)
- [Σύνδεση με βάση δεδομένων SQL με χρήση του ελέγχου ταυτότητας καταλόγου Azure Active Directory](sql-database-aad-authentication.md)


