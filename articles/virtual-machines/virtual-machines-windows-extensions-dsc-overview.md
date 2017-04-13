<properties
   pageTitle="Κατάσταση ρύθμισης παραμέτρων για Azure Επισκόπηση επιθυμητοί | Microsoft Azure"
   description="Επισκόπηση για τη χρήση του προγράμματος χειρισμού επέκταση Microsoft Azure για ρύθμιση παραμέτρων κατάσταση επιθυμητοί PowerShell. Όπως τις προϋποθέσεις, αρχιτεκτονική, cmdlet για..."
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

# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Εισαγωγή στο πρόγραμμα χειρισμού επέκταση ρύθμισης παραμέτρων κατάσταση επιθυμητοί Azure #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Η παράγοντας Εικονική Azure και τις σχετικές επεκτάσεις αποτελούν μέρος των υπηρεσιών Microsoft Azure υποδομής. Εικονική επεκτάσεις είναι στοιχεία λογισμικού που επεκτείνουν τη λειτουργικότητα Εικονική και να απλοποιήσετε διάφορες λειτουργίες διαχείρισης Εικονική. Για παράδειγμα, την επέκταση VMAccess μπορεί να χρησιμοποιηθεί για επαναφορά κωδικού πρόσβασης ενός διαχειριστή ή την επέκταση προσαρμοσμένης δέσμης ενεργειών μπορεί να χρησιμοποιηθεί για να εκτελέσετε μια δέσμη ενεργειών σε η Εικονική.

Σε αυτό το άρθρο παρουσιάζει την επέκταση ρύθμισης παραμέτρων κατάσταση επιθυμητοί PowerShell (DSC) για ΣΠΣ Azure ως μέρος του Azure PowerShell SDK. Μπορείτε να χρησιμοποιήσετε τη νέα cmdlet για να στείλετε και να εφαρμόσετε μια ρύθμιση παραμέτρων PowerShell DSC σε μια Εικονική Azure με δυνατότητα με την επέκταση PowerShell DSC. Το PowerShell DSC επέκταση κλήσεις σε PowerShell DSC για να ενεργοποιηθεί τις παραμέτρους του ληφθέντος DSC σε η Εικονική. Αυτή η λειτουργία είναι επίσης διαθέσιμες μέσω της πύλης Azure.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία ##
**Τοπικό υπολογιστή** Για να αλληλεπιδράσετε με την επέκταση Εικονική Azure, πρέπει να χρησιμοποιήσετε την πύλη του Azure ή το Azure SDK PowerShell. 

**Παράγοντας επισκέπτη** Η Εικονική Azure που θα δημιουργηθεί από τη ρύθμιση των παραμέτρων DSC πρέπει να είναι ένα λειτουργικό σύστημα που υποστηρίζει είτε Framework διαχείρισης των Windows (WMF) 4.0 ή 5.0. Μπορείτε να βρείτε την πλήρη λίστα των υποστηριζόμενων OS εκδόσεων στο [Ιστορικό εκδόσεων επέκταση DSC](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Όροι και έννοιες ##
Αυτός ο οδηγός προϋποθέτει εξοικείωση με τις έννοιες των εξής:

Ρύθμιση παραμέτρων - ένα έγγραφο ρύθμισης παραμέτρων DSC. 

Κόμβος - Ο στόχος για μια ρύθμιση παραμέτρων DSC. Σε αυτό το έγγραφο, "κόμβο" αναφέρεται πάντα σε μια Εικονική Azure.

Ρύθμιση παραμέτρων - μια .psd1 του αρχείου δεδομένων που περιέχουν δεδομένα περιβάλλοντος για μια ρύθμιση παραμέτρων

## <a name="architectural-overview"></a>Αρχιτεκτονική Επισκόπηση ##

Την επέκταση Azure DSC χρησιμοποιεί πλαίσιο Azure Εικονική παράγοντας για παράδοση και ενεργοποιηθεί αναφορά για ρυθμίσεις παραμέτρων DSC εκτελείται σε ΣΠΣ Azure. Η επέκταση DSC αναμένει ένα αρχείο .zip που περιέχει τουλάχιστον ένα έγγραφο ρύθμισης παραμέτρων και ένα σύνολο παραμέτρων που παρέχεται μέσω του PowerShell SDK Azure είτε μέσω της πύλης Azure.

Όταν η επέκταση καλείται για πρώτη φορά, εκτελείται μια διαδικασία εγκατάστασης. Η διαδικασία αυτή εγκαθιστά μια έκδοση από το Windows διαχείρισης Framework (WMF) με χρήση της λογικής παρακάτω:

1. Εάν το λειτουργικό σύστημα Εικονική Azure είναι Windows Server 2016, καμία ενέργεια. Windows Server 2016 έχει ήδη την πιο πρόσφατη έκδοση του PowerShell εγκατεστημένο.
2. Εάν το `wmfVersion` έχει καθοριστεί ιδιότητα, αυτό το WMF εγκαταστήσει, εκτός εάν δεν είναι συμβατή με την εικονική Μηχανή OS.
3. Εάν δεν υπάρχει `wmfVersion` ιδιότητα έχει καθοριστεί, η πιο πρόσφατη ισχύει έκδοση από το WMF είναι εγκατεστημένο.

Εγκατάσταση από το WMF απαιτεί επανεκκίνηση. Μετά την επανεκκίνηση, την επέκταση λήψεις που καθορίζεται στο αρχείο .zip το `modulesUrl` την ιδιότητα. Εάν αυτήν τη θέση είναι στο χώρο αποθήκευσης αντικειμένων blob του Azure, ενός διακριτικού συσχετισμών Ασφαλείας μπορεί να έχει καθοριστεί σε το `sasToken` ιδιοτήτων για να αποκτήσετε πρόσβαση στο αρχείο. Μετά την .zip έχουν ληφθεί και έγινε η αποσυσκευασία, η συνάρτηση ρύθμισης παραμέτρων ορίζεται στο `configurationFunction` εκτέλεση για να δημιουργήσετε το αρχείο MOF. Στη συνέχεια, εκτελεί την επέκταση `Start-DscConfiguration -Force` στο αρχείο που έχει δημιουργηθεί MOF. Η επέκταση καταγράφει εξόδου και εγγράφει την ξανά στο κανάλι κατάστασης Azure. Από αυτό το σημείο, η συνάρτηση LCM DSC χειρίζεται παρακολούθηση και διόρθωση κανονικά. 

## <a name="powershell-cmdlets"></a>Cmdlet του PowerShell ##

Cmdlet του PowerShell μπορεί να χρησιμοποιηθεί με ARM ή ASM σε συσκευάσετε, δημοσίευση και την παρακολούθηση DSC επέκταση αναπτύξεις. Τα ακόλουθα cmdlet που παρατίθενται είναι οι λειτουργικές μονάδες ASM, αλλά μπορεί να αντικατασταθεί "Azure" με "AzureRm" για να χρησιμοποιήσετε το μοντέλο ARM. Για παράδειγμα, `Publish-AzureVMDscConfiguration` χρησιμοποιεί ASM, όπου `Publish-AzureRmVMDscConfiguration` χρησιμοποιεί ARM. 

`Publish-AzureVMDscConfiguration`σας μεταφέρει σε ένα αρχείο ρύθμισης παραμέτρων, σαρώνει για τους πόρους εξαρτώμενα DSC και δημιουργεί ένα αρχείο .zip που περιέχει τη ρύθμιση παραμέτρων και DSC τους πόρους που απαιτούνται για να ενεργοποιηθεί αυτή η ρύθμιση παραμέτρων. Το επίσης να δημιουργήσετε το πακέτο τοπικά χρησιμοποιώντας το `-ConfigurationArchivePath` παραμέτρου. Διαφορετικά, δημοσιεύει το αρχείο .zip με το χώρο αποθήκευσης αντικειμένων blob του Azure και διασφαλίσει με ενός διακριτικού συσχετισμών Ασφαλείας.

Το αρχείο .zip που δημιουργήθηκε από αυτό το cmdlet περιλαμβάνει τη δέσμη ενεργειών ρύθμισης παραμέτρων .ps1 στον ριζικό κατάλογο του φακέλου αρχειοθέτησης. Πόροι έχουν το φάκελο λειτουργική μονάδα που τοποθετείται στο φάκελο αρχειοθέτησης. 

`Set-AzureVMDscExtension`Εισάγει τις ρυθμίσεις που απαιτούνται από την επέκταση PowerShell DSC σε μια Εικονική αντικειμένου ρύθμισης παραμέτρων, το οποίο, στη συνέχεια, μπορούν να εφαρμοστούν σε μια Εικονική Azure με `Update-AzureVM`.

`Get-AzureVMDscExtension`ανακτά την κατάσταση επέκταση DSC από μια συγκεκριμένη Εικονική. 

`Get-AzureVMDscExtensionStatus`ανακτά την κατάσταση της ρύθμισης παραμέτρων DSC λάβει απόφαση από το πρόγραμμα χειρισμού επέκταση DSC. Αυτή η ενέργεια μπορεί να εκτελεστεί σε μία μόνο Εικονική ή ομάδα ΣΠΣ.

`Remove-AzureVMDscExtension`Καταργεί το πρόγραμμα χειρισμού επέκταση από μια δεδομένη εικονική μηχανή. Αυτό το cmdlet **καταργήσετε τη ρύθμιση παραμέτρων,** κατάργηση της εγκατάστασης του WMF ή να αλλάξετε τις ρυθμίσεις που έχουν εφαρμοστεί σε η εικονική μηχανή. Καταργεί μόνο το πρόγραμμα χειρισμού επέκτασης. 

**Βασικές διαφορές cmdlet ASM και ARM**

- Cmdlet για ARM είναι σύγχρονη. Cmdlet του ASM είναι ασύγχρονης.
- ResourceGroupName, VMName, ArchiveStorageAccountName, έκδοση και την τοποθεσία είναι όλες τις νέες απαιτούμενες παραμέτρους.
- ArchiveResourceGroupName είναι μια νέα προαιρετική παράμετρος για ARM. Μπορείτε να ορίσετε αυτήν την παράμετρο, όταν το λογαριασμό χώρου αποθήκευσης που ανήκει σε μια ομάδα πόρων διαφορετικό από αυτό όπου έχει δημιουργηθεί η εικονική μηχανή.
- ConfigurationArchive ονομάζεται ArchiveBlobName στο ARM
- ContainerName ονομάζεται ArchiveContainerName στο ARM
- StorageEndpointSuffix ονομάζεται ArchiveStorageEndpointSuffix στο ARM
- Ο διακόπτης AutoUpdate έχει προστεθεί ARM, για να ενεργοποιήσετε την αυτόματη ενημέρωση από το πρόγραμμα χειρισμού επέκταση στην πιο πρόσφατη έκδοση ως και όταν είναι διαθέσιμη. Σημειώστε αυτή η παράμετρος έχει τη δυνατότητα να προκαλέσει την επανεκκίνηση σε η Εικονική όταν κυκλοφορήσει μια νέα έκδοση του το WMF. 


## <a name="azure-portal-functionality"></a>Λειτουργίες Azure πύλης ##
Αναζητήστε μια Εικονική κλασική. Στην περιοχή Ρυθμίσεις -> Γενικά, κάντε κλικ στην επιλογή "Επεκτάσεις." Δημιουργείται ένα νέο παράθυρο. Κάντε κλικ στην επιλογή "Προσθήκη" και επιλέξτε PowerShell DSC.

Η πύλη πρέπει να εισόδου.
**Ρύθμιση παραμέτρων λειτουργικές μονάδες ή δέσμη ενεργειών**: αυτό το πεδίο είναι υποχρεωτικό. Απαιτεί ένα αρχείο .ps1 που περιέχει μια δέσμη ενεργειών ρύθμισης παραμέτρων ή σε ένα αρχείο .zip με μια δέσμη ενεργειών ρύθμισης παραμέτρων .ps1 από το ριζικό κατάλογο και όλους τους πόρους εξαρτώμενα στους φακέλους λειτουργική μονάδα εντός του .zip. Αυτό μπορεί να δημιουργηθεί με το `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` cmdlet περιλαμβάνονται στο Azure PowerShell SDK. Το αρχείο .zip αποστέλλεται στο χρήστη αποθήκευσης αντικειμένων blob του ασφαλές μέσω ενός διακριτικού συσχετισμών Ασφαλείας. 

**Αρχείο PSD1 δεδομένων ρύθμισης παραμέτρων**: αυτό το πεδίο είναι προαιρετικό. Εάν η ρύθμιση των παραμέτρων σας απαιτεί ένα αρχείο δεδομένων ρύθμισης παραμέτρων σε .psd1, χρησιμοποιήστε αυτό το πεδίο για να επιλέξετε και στείλτε το στο χώρο αποθήκευσης αντικειμένων blob του χρήστη, όπου είναι ασφαλή μέσω ενός διακριτικού συσχετισμών Ασφαλείας. 
 
**Όνομα Module-Qualified της ρύθμισης παραμέτρων**: .ps1 αρχεία μπορούν να έχουν πολλές συναρτήσεις ρύθμισης παραμέτρων. Πληκτρολογήστε το όνομα της ρύθμισης παραμέτρων .ps1 δέσμης ενεργειών ακολουθούμενο από ένα '\' και το όνομα της συνάρτησης ρύθμισης παραμέτρων. Για παράδειγμα, εάν η δέσμη ενεργειών .ps1 περιλαμβάνει το όνομα "configuration.ps1" και η ρύθμιση παραμέτρων είναι "IisInstall", μπορείτε να εισαγάγετε:`configuration.ps1\IisInstall`

**Ρύθμιση παραμέτρων ορίσματα**: Εάν η συνάρτηση ρύθμισης παραμέτρων λαμβάνει ορίσματα, εισαγάγετε σε αυτό το σημείο στη μορφή `argumentName1=value1,argumentName2=value2`. Σημείωση Αυτή η μορφή είναι μια διαφορετική μορφή από τον τρόπο ρύθμισης παραμέτρων ορίσματα γίνονται δεκτές μέσω των cmdlet του PowerShell ή πρότυπα διαχείρισης πόρων. 

## <a name="getting-started"></a>Γρήγορα αποτελέσματα ##

Η επέκταση Azure DSC λαμβάνει έγγραφα ρύθμισης παραμέτρων DSC και θεσπίζει τους σε VM Azure. Ακολουθεί ένα απλό παράδειγμα μιας διαμόρφωσης. Να το αποθηκεύσετε τοπικά ως "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Ακολουθήστε τα παρακάτω βήματα τοποθετήσετε τη δέσμη ενεργειών IisInstall.ps1 στην την καθορισμένη εικονική Μηχανή, εκτελεί τη ρύθμιση παραμέτρων και πάλι αναφορά της κατάστασης.
 
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "demo.ps1.zip" -StorageContext $storageContext -ConfigurationName "runScript" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```

## <a name="logging"></a>Καταγραφή ##

Αρχεία καταγραφής τοποθετούνται σε:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[αριθμό έκδοσης]

## <a name="next-steps"></a>Επόμενα βήματα ##

Για περισσότερες πληροφορίες σχετικά με το PowerShell DSC, [επισκεφθείτε το Κέντρο τεκμηρίωση του PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Εξετάστε το [πρότυπο διαχείρισης πόρων Azure για την επέκταση DSC](virtual-machines-windows-extensions-dsc-template.md
). 

Για να βρείτε πρόσθετες λειτουργίες για να διαχειριστείτε με DSC PowerShell, [περιηγηθείτε στη συλλογή του PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) για περισσότερους πόρους DSC.

Για λεπτομέρειες σχετικά με μεταβίβαση παραμέτρων διάκριση πεζών-κεφαλαίων σε ρυθμίσεις παραμέτρων, ανατρέξτε στο θέμα [Διαχείριση διαπιστευτηρίων με ασφάλεια με το πρόγραμμα χειρισμού επέκταση DSC](virtual-machines-windows-extensions-dsc-credentials.md).