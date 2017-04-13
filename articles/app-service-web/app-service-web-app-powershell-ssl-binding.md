<properties
    pageTitle="Χρήση του PowerShell σύνδεση πιστοποιητικά SSL"
    description="Μάθετε πώς μπορείτε να συνδέσετε πιστοποιητικά SSL σε εφαρμογή web με χρήση του PowerShell."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure εφαρμογής υπηρεσίας SSL πιστοποιητικό σύνδεση χρήση του PowerShell #

Με την έκδοση του Microsoft Azure PowerShell έκδοση 1.1.0 μια νέα cmdlet έχει προστεθεί που θα σας δώσει στο χρήστη τη δυνατότητα να δεσμεύσετε υπάρχουσες ή νέες πιστοποιητικά SSL σε μια υπάρχουσα εφαρμογή Web.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Για να μάθετε σχετικά με τη χρήση Azure από διαχειριστή πόρων βάσει Azure PowerShell cmdlet για τη Διαχείριση εφαρμογών Web της έλεγχος [Azure από διαχειριστή πόρων με βάση τις εντολές του PowerShell για Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Αποστολή και σύνδεση νέου πιστοποιητικού SSL ##

Σενάριο: Ο χρήστης θα θέλατε να δεσμεύσετε ένα πιστοποιητικό SSL σε μία από τις εφαρμογές του web.

Εάν γνωρίζετε το όνομα της ομάδας πόρων που περιέχει την εφαρμογή web, το όνομα της εφαρμογής web, τη διαδρομή του αρχείου .pfx πιστοποιητικό στον υπολογιστή του χρήστη, τον κωδικό πρόσβασης για το πιστοποιητικό και το προσαρμοσμένο όνομα κεντρικού υπολογιστή, θα σας να χρησιμοποιήσετε την παρακάτω εντολή PowerShell για τη δημιουργία συγκεκριμένη σύνδεση SSL:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Σημειώστε ότι πριν προσθέσετε μια σύνδεση SSL σε μια εφαρμογή web, πρέπει να έχετε ένα όνομα κεντρικού υπολογιστή (προσαρμοσμένο τομέα) ήδη ρυθμίσει τις παραμέτρους. Εάν δεν έχει ρυθμιστεί το όνομα κεντρικού υπολογιστή και, στη συνέχεια, θα λάβετε ένα μήνυμα σφάλματος "όνομα κεντρικού υπολογιστή" δεν υπάρχει ενώ εκτελείται δημιουργία AzureRmWebAppSSLBinding. Μπορείτε να προσθέσετε ένα όνομα κεντρικού υπολογιστή απευθείας από την πύλη ή τη χρήση του Azure PowerShell. Το παρακάτω τμήμα κώδικα PowerShell μπορεί να είναι για να ρυθμίσετε το όνομα κεντρικού υπολογιστή πριν από την εκτέλεση δημιουργία AzureRmWebAppSSLBinding.   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
Είναι σημαντικό να κατανοήσετε ότι το cmdlet Set-AzureRmWebApp αντικαθιστά τα ονόματα για την εφαρμογή web. Ως εκ τούτου το παραπάνω τμήμα κώδικα PowerShell προσάρτηση στην υπάρχουσα λίστα των ονομάτων κεντρικού υπολογιστή για την εφαρμογή web.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Αποστολή και σύνδεση ένα υπάρχον πιστοποιητικό SSL ##

Σενάριο: Ο χρήστης θα θέλατε να δεσμεύσετε ένα πιστοποιητικό SSL που έχουν αποσταλεί προηγουμένως σε μία από τις εφαρμογές του web.

Θα μπορώ να αποκτήσω τη λίστα των πιστοποιητικών ήδη αποστείλει σε μια ομάδα συγκεκριμένο πόρο, χρησιμοποιώντας την ακόλουθη εντολή

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Σημειώστε ότι τα πιστοποιητικά είναι τοπικά σε μια συγκεκριμένη θέση και ομάδα πόρων, ο χρήστης πρέπει να αποστείλετε ξανά το πιστοποιητικό, εάν η εφαρμογή έχει ρυθμιστεί web είναι σε διαφορετική θέση και πόρων ομαδοποίηση άλλες που απαιτείται το πιστοποιητικό 

Εάν γνωρίζετε το όνομα της ομάδας πόρων που περιέχει την εφαρμογή web, το όνομα της εφαρμογής web, την αποτύπωση πιστοποιητικού και το προσαρμοσμένο όνομα κεντρικού υπολογιστή, θα σας να χρησιμοποιήσετε την παρακάτω εντολή PowerShell για τη δημιουργία συγκεκριμένη σύνδεση SSL:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Διαγραφή μιας υπάρχουσας σύνδεσης SSL  ##

Σενάριο: Ο χρήστης θα θέλατε να διαγράψετε μια υπάρχουσα σύνδεση SSL.

Εάν γνωρίζετε το όνομα της ομάδας πόρων που περιέχει την εφαρμογή web, το όνομα της εφαρμογής web και το προσαρμοσμένο όνομα κεντρικού υπολογιστή, θα σας να χρησιμοποιήσετε την παρακάτω εντολή PowerShell για να καταργήσετε αυτήν τη σύνδεση SSL:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Λάβετε υπόψη ότι εάν η σύνδεση καταργημένα SSL δεν ήταν η τελευταία σύνδεση χρησιμοποιώντας αυτό το πιστοποιητικό σε που θέση, από προεπιλογή, το πιστοποιητικό θα διαγραφεί, εάν ο χρήστης που θέλετε να διατηρήσετε το πιστοποιητικό που μπορεί να χρησιμοποιήσει την επιλογή DeleteCertificate για να διατηρήσετε το πιστοποιητικό

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Αναφορές ###
- [Azure διαχείριση πόρων με βάση τις εντολές του PowerShell για Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Εισαγωγή στο περιβάλλον εφαρμογής υπηρεσίας](app-service-app-service-environment-intro.md)
- [Χρήση του Azure PowerShell με τη Διαχείριση Azure πόρων](../powershell-azure-resource-manager.md)
