<properties
   pageTitle="Προώθηση διαπιστευτήρια στον Azure χρησιμοποιώντας DSC | Microsoft Azure"
   description="Επισκόπηση σε ασφαλή προώθηση διαπιστευτήρια στον Azure εικονικές μηχανές χρήση της ρύθμισης παραμέτρων κατάσταση επιθυμητοί PowerShell"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Διέρχεται τα διαπιστευτήρια για το πρόγραμμα χειρισμού επέκταση Azure DSC #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Σε αυτό το άρθρο καλύπτει την επέκταση επιθυμητοί ρύθμισης παραμέτρων κατάστασης για Azure. Μπορείτε να βρείτε μια επισκόπηση του το πρόγραμμα χειρισμού επέκταση DSC στο [Εισαγωγή στο πρόγραμμα χειρισμού επέκταση ρύθμισης παραμέτρων κατάσταση επιθυμητοί Azure](virtual-machines-windows-extensions-dsc-overview.md). 


## <a name="passing-in-credentials"></a>Η διαβίβαση διαπιστευτήρια
Ως μέρος της διαδικασίας ρύθμισης παραμέτρων, μπορεί να χρειάζεται να ρυθμίσετε τους λογαριασμούς χρηστών, πρόσβαση στις υπηρεσίες ή εγκατάσταση του προγράμματος στο περιβάλλον εργασίας χρήστη. Για να εκτελέσετε τις παρακάτω ενέργειες, πρέπει να παρέχει διαπιστευτήρια. 

DSC επιτρέπει με παραμέτρους ρυθμίσεις παραμέτρων στην οποία διαπιστευτήρια που εισήχθησαν σε τη ρύθμιση παραμέτρων και με ασφάλεια έχουν αποθηκευτεί σε αρχεία MOF. Το πρόγραμμα χειρισμού επέκταση Azure απλοποιεί τη Διαχείριση διαπιστευτηρίων, παρέχοντας αυτόματη διαχείριση πιστοποιητικών. 

Λάβετε υπόψη τα παρακάτω DSC δέσμη ενεργειών ρύθμισης παραμέτρων που δημιουργεί ένα τοπικό λογαριασμό χρήστη με τον καθορισμένο κωδικό πρόσβασης:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Είναι σημαντικό να συμπεριλάβετε *τοπικού κεντρικού υπολογιστή κόμβου* ως μέρος της ρύθμισης παραμέτρων. Εάν λείπει αυτήν τη δήλωση, ακολουθήστε τα παρακάτω βήματα δεν λειτουργούν όπως το πρόγραμμα χειρισμού επέκταση συγκεκριμένα αναζητά τη δήλωση κόμβο τοπικού κεντρικού υπολογιστή. Επίσης, είναι σημαντικό να συμπεριλάβετε typecast *[PsCredential]*, όπως τον συγκεκριμένο τύπο ενεργοποιεί την επέκταση για την κρυπτογράφηση των διαπιστευτηρίων. 

Δημοσίευση αυτήν τη δέσμη ενεργειών στο χώρο αποθήκευσης αντικειμένων blob:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Ορίστε την επέκταση Azure DSC και δώστε τα διαπιστευτήρια:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"
 
$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments
 
$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Πώς είναι ασφαλείς διαπιστευτήρια
Εκτέλεση του κώδικα ζητά μια πιστοποίηση. Όταν παρέχεται, αποθηκεύονται στη μνήμη σύντομα. Όταν δημοσιευτεί με `Set-AzureVmDscExtension` cmdlet, θα μεταδίδονται μέσω HTTPS για να την εικονική Μηχανή, όπου Azure αποθηκεύει κρυπτογραφημένα στο δίσκο, χρησιμοποιώντας το τοπικό πιστοποιητικό Εικονική. Στη συνέχεια, είναι λίγο αποκρυπτογράφηση στη μνήμη και εκ νέου κρυπτογραφημένα για τη μεταβίβαση DSC.

Αυτή η συμπεριφορά είναι διαφορετική από τη [Χρήση ασφαλούς ρυθμίσεις παραμέτρων χωρίς το πρόγραμμα χειρισμού επέκτασης](https://msdn.microsoft.com/powershell/dsc/securemof). Το περιβάλλον Azure παρέχει έναν τρόπο για τη μετάδοση δεδομένων ρύθμισης παραμέτρων με ασφάλεια μέσω πιστοποιητικά. Όταν χρησιμοποιείτε το πρόγραμμα χειρισμού DSC επέκταση, δεν χρειάζεται για την παροχή $CertificatePath ή ένα $CertificateID / $Thumbprint καταχώρησης στο ConfigurationData.


## <a name="next-steps"></a>Επόμενα βήματα ##

Για περισσότερες πληροφορίες σχετικά με το πρόγραμμα χειρισμού επέκταση Azure DSC, ανατρέξτε στο θέμα [Εισαγωγή στο πρόγραμμα χειρισμού επέκταση ρύθμισης παραμέτρων κατάσταση επιθυμητοί Azure](virtual-machines-windows-extensions-dsc-overview.md). 

Εξετάστε το [πρότυπο διαχείρισης πόρων Azure για την επέκταση DSC](virtual-machines-windows-extensions-dsc-template.md).

Για περισσότερες πληροφορίες σχετικά με το PowerShell DSC, [επισκεφθείτε το Κέντρο τεκμηρίωση του PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Για να βρείτε πρόσθετες λειτουργίες για να διαχειριστείτε με DSC PowerShell, [περιηγηθείτε στη συλλογή του PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) για περισσότερους πόρους DSC.
