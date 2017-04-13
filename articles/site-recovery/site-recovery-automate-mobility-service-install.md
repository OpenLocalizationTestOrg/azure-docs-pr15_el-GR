<properties
    pageTitle="Αναπαραγωγή VMware εικονικές μηχανές σε Azure χρησιμοποιώντας Επαναφορά τοποθεσίας με DSC αυτοματισμού Azure | Microsoft Azure"
    description="Περιγράφει τον τρόπο χρήσης DSC αυτοματισμού Azure για να αναπτύξετε αυτόματα την υπηρεσία Azure τοποθεσίας αποκατάστασης φορητότητα και Azure παράγοντας για μηχανές εικονικού/φυσικά σε Azure."
    services="site-recovery"
    documentationCenter=""
    authors="krnese"
    manager="lorenr"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="krnese"/>

# <a name="replicate-vmware-virtual-machines-to-azure-by-using-site-recovery-with-azure-automation-dsc"></a>Αναπαραγωγή VMware εικονικές μηχανές σε Azure χρησιμοποιώντας Επαναφορά τοποθεσίας με DSC Αυτοματισμός Azure

Λειτουργίες διαχείρισης οικογένειας προγραμμάτων, σας παρέχουμε μια ολοκληρωμένη δημιουργία αντιγράφων ασφαλείας και λύση αποκατάστασης από καταστροφή που μπορείτε να χρησιμοποιήσετε ως μέρος του σχεδίου συνέχειας επιχειρήσεις.

Θα σας αποτελέσματα αυτό ταξίδι μαζί με το Hyper-V χρησιμοποιώντας το Hyper-V αντιγράφου. Αλλά έχουμε διευρύνει για την υποστήριξη μια ρύθμιση ετερογενή, επειδή οι πελάτες έχουν πολλές υπερεπόπτες και πλατφόρμες στο τους σύννεφων.

Εάν εκτελείτε VMware φόρτους εργασίας ή/και φυσική διακομιστές σήμερα, ένα διακομιστή διαχείρισης όλα τα στοιχεία Επαναφορά τοποθεσίας Azure εκτελείται στο περιβάλλον σας για το χειρισμό της αναπαραγωγής επικοινωνίας και τα δεδομένα με το Azure, όταν ο προορισμός είναι Azure.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>Ανάπτυξη της υπηρεσίας φορητότητα αποκατάστασης τοποθεσία χρησιμοποιώντας DSC αυτοματισμού
Ας ξεκινήσουμε, κάνοντας μια γρήγορη ανάλυση του τι σημαίνει αυτό διακομιστή διαχείρισης.

Ο διακομιστής διαχείρισης λειτουργεί διάφορες τους ρόλους διακομιστή. Ένα από αυτούς τους ρόλους είναι *ρύθμισης παραμέτρων*, οι οποίες συντεταγμένων επικοινωνίας και να διαχειρίζεται διεργασίες αναπαραγωγής και ανάκτησης δεδομένων.

Επιπλέον, ο ρόλος *διαδικασία* λειτουργεί ως μια πύλη αναπαραγωγής. Αυτός ο ρόλος λαμβάνει αναπαραγωγή δεδομένων από το προστατευμένο προέλευσης μηχανές βελτιστοποιεί με σε cache, συμπίεση και κρυπτογράφηση και, στη συνέχεια, αποστέλλει ένα λογαριασμό Azure χώρου αποθήκευσης. Μία από τις συναρτήσεις του ρόλου διαδικασία είναι επίσης να προωθήσετε της εγκατάστασης της υπηρεσίας φορητότητα σε προστατευμένο μηχανές και εκτελέστε Αυτόματος εντοπισμός του ΣΠΣ VMware.

Εάν υπάρχει μια αποκατάσταση μετά από από το Azure, ο ρόλος *κύριας προορισμού* θα χειρίζεται τα δεδομένα αναπαραγωγής ως τμήμα αυτής της λειτουργίας.

Για τα προστατευμένα μηχανήματα, βασίζονται στην *υπηρεσία φορητότητα*. Αυτό το στοιχείο έχει αναπτυχθεί σε κάθε υπολογιστή (VMware Εικονική ή φυσική διακομιστή) που θέλετε να αναπαραγάγετε σε Azure. Το καταγράφει εγγραφής δεδομένων στον υπολογιστή και τα προωθεί στο διακομιστή διαχείρισης (διαδικασία ρόλο).

Όταν χειρισμός αδιάκοπη παροχή υπηρεσιών, είναι σημαντικό να κατανοήσετε το φόρτο εργασίας, την υποδομή σας και τα στοιχεία που περιλαμβάνονται. Μπορείτε, στη συνέχεια, μπορεί να πληροί τις απαιτήσεις για το στόχο χρόνου αποκατάστασης (RTO) και στόχος σημείου αποκατάστασης (RPO). Στο πλαίσιο αυτό, η υπηρεσία φορητότητα είναι το κλειδί για τη διασφάλιση ότι το φόρτο εργασίας δεν είναι προστατευμένο, όπως θα περιμένατε.

Πώς μπορούν να σας, με βελτιστοποιημένη τρόπο, βεβαιωθείτε ότι έχετε μια αξιόπιστη ρύθμιση προστατευμένου με βοήθεια από ορισμένα στοιχεία οικογένεια προγραμμάτων διαχείρισης λειτουργίες;

Σε αυτό το άρθρο παρέχει ένα παράδειγμα του πώς μπορείτε να χρησιμοποιήσετε Azure αυτοματισμού επιθυμητοί κατάσταση ρύθμισης παραμέτρων (DSC), μαζί με την Επαναφορά τοποθεσίας, για να βεβαιωθείτε ότι:

- Η υπηρεσία φορητότητα και παράγοντας Εικονική Azure αναπτύσσονται στα Windows μηχανήματα που θέλετε να προστατεύσετε.
- Η υπηρεσία φορητότητα και παράγοντας Εικονική Azure εκτελούνται πάντα όταν Azure είναι ο στόχος αναπαραγωγής.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- Ένα αποθετήριο δεδομένων για να αποθηκεύσετε την απαιτούμενη ρύθμιση
- Ένα αποθετήριο δεδομένων για να αποθηκεύσετε τη φράση πρόσβασης που απαιτείται για την καταχώρηση με το διακομιστή διαχείρισης

 > [AZURE.NOTE] Δημιουργείται ένα μοναδικό φράση πρόσβασης για κάθε διακομιστή διαχείρισης. Εάν πρόκειται να αναπτύξετε πολλές διακομιστές διαχείρισης, πρέπει να βεβαιωθείτε ότι η σωστή φράση πρόσβασης είναι αποθηκευμένο στο αρχείο passphrase.txt.

- Πλαίσιο διαχείρισης των Windows (WMF) 5.0 εγκατεστημένο στους υπολογιστές που θέλετε να ενεργοποιήσετε για προστασία (προϋπόθεση για αυτοματισμού DSC)

 > [AZURE.NOTE] Εάν θέλετε να χρησιμοποιήσετε μηχανές DSC για Windows που έχουν εγκατασταθεί 4.0 WMF, ανατρέξτε στην ενότητα [Χρήση DSC στα περιβάλλοντα αποσύνδεσης](#Use DSC in disconnected environments).

Η υπηρεσία φορητότητα μπορεί να εγκατασταθεί μέσω της γραμμής εντολών και αποδέχεται πολλά ορίσματα. Λόγο πρέπει να έχετε τα δυαδικά δεδομένα (μετά την εξαγωγή τους από τις ρυθμίσεις σας) και να αποθηκεύσετε σε ένα σημείο όπου μπορείτε να τις ανακτήσετε, χρησιμοποιώντας μια ρύθμιση παραμέτρων DSC.

## <a name="step-1-extract-binaries"></a>Βήμα 1: Δυαδικά στοιχεία αποσπάσματος

1. Για να εξαγάγετε τα αρχεία που χρειάζεστε για αυτήν την εγκατάσταση, μεταβείτε στον ακόλουθο κατάλογο στο διακομιστή διαχείρισης:

    **\Microsoft Recovery\home\svsystems\pushinstallsvc\repository azure τοποθεσίας**

    Σε αυτόν το φάκελο, θα πρέπει να βλέπετε ένα αρχείο MSI με το όνομα:

    **Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**

    Χρησιμοποιήστε την ακόλουθη εντολή για να εξαγάγετε το πρόγραμμα εγκατάστασης:

    **/X:C:\Users\Administrator\Desktop\Mobility_Service\Extract/q.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe**

2. Επιλέξτε όλα τα αρχεία και να τις στείλετε σε συμπιεσμένο φάκελο (μορφή zip).

Τώρα έχετε τα δυαδικά δεδομένα που χρειάζεστε για να αυτοματοποιήσετε τη διαμόρφωση της υπηρεσίας φορητότητα χρησιμοποιώντας DSC αυτοματισμού.

### <a name="passphrase"></a>Η φράση πρόσβασης

Στη συνέχεια, πρέπει να προσδιορίσετε το σημείο όπου θέλετε να τοποθετήσετε αυτόν το φάκελο σε μορφή zip. Μπορείτε να χρησιμοποιήσετε ένα λογαριασμό Azure χώρου αποθήκευσης, όπως φαίνεται αργότερα, για να αποθηκεύσετε τη φράση πρόσβασης που χρειάζεστε για τη ρύθμιση. Ο παράγοντας θα, στη συνέχεια, καταχωρήστε το διακομιστή διαχείρισης ως μέρος της διαδικασίας.

Η φράση πρόσβασης που λάβατε όταν αναπτυχθεί το διακομιστή διαχείρισης μπορούν να αποθηκευτούν σε ένα αρχείο κειμένου ως passphrase.txt.

Τοποθετήστε το φάκελο σε μορφή zip και τη φράση πρόσβασης σε ένα αποκλειστικό κοντέινερ στο λογαριασμό Azure χώρου αποθήκευσης.

![Θέση του φακέλου](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Εάν προτιμάτε να διατηρήσετε αυτά τα αρχεία σε ένα κοινόχρηστο στοιχείο στο δίκτυό σας, μπορείτε να το κάνετε. Πρέπει να εξασφαλίσετε ότι ο πόρος DSC που θα χρησιμοποιήσετε αργότερα έχει πρόσβαση και να λάβετε την εγκατάσταση και τη φράση πρόσβασης.

## <a name="step-2-create-the-dsc-configuration"></a>Βήμα 2: Δημιουργία της ρύθμισης παραμέτρων DSC

Το πρόγραμμα εγκατάστασης εξαρτάται από WMF 5.0. Για τον υπολογιστή για να εφαρμόσετε με επιτυχία τις ρυθμίσεις μέσω DSC αυτοματισμού, WMF 5.0 πρέπει να είναι παρουσίαση.

Το περιβάλλον χρησιμοποιεί τις ακόλουθες ρυθμίσεις παραμέτρων DSC παράδειγμα:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
Η ρύθμιση παραμέτρων θα κάντε τα εξής:

- Τις μεταβλητές θα σας ενημερώσει τη ρύθμιση παραμέτρων πού μπορείτε να βρείτε τα δυαδικά δεδομένα για την υπηρεσία φορητότητα και τον παράγοντα Εικονική Azure, πού μπορείτε να βρείτε τη φράση πρόσβασης και τη θέση για να αποθηκεύσετε το αποτέλεσμα.
- Η ρύθμιση παραμέτρων θα εισαγάγει τον πόρο xPSDesiredStateConfiguration DSC, ώστε να μπορείτε να χρησιμοποιήσετε `xRemoteFile` για να κάνετε λήψη των αρχείων από το χώρο αποθήκευσης.
- Η ρύθμιση παραμέτρων θα δημιουργήσει έναν κατάλογο όπου θέλετε να αποθηκεύσετε τα δυαδικά δεδομένα.
- Ο πόρος αρχειοθέτησης θα εξαγάγετε τα αρχεία από το φάκελο σε μορφή zip.
- Το πακέτο του πόρου εγκατάστασης θα εγκαταστήσετε την υπηρεσία φορητότητα από το UNIFIEDAGENT. Πρόγραμμα εγκατάστασης EXE με τα συγκεκριμένα ορίσματα. (Τις μεταβλητές που θα δημιουργήσετε τα ορίσματα πρέπει να αλλάξουν ώστε να αντικατοπτρίζει το περιβάλλον σας.)
- Το πακέτο AzureAgent πόρων θα εγκαταστήσει τον παράγοντα Εικονική Azure, η οποία προτείνεται σε κάθε Εικονική που εκτελείται στο Azure. Ο παράγοντας Εικονική Azure επίσης καθιστά δυνατή για να προσθέσετε επεκτάσεις η Εικονική μετά την ανακατεύθυνση.
- Η υπηρεσία πόρο ή τους πόρους εξασφαλίζει ότι οι σχετικές υπηρεσίες φορητότητα και των υπηρεσιών Azure πάντα εκτελούνται.

Αποθήκευση της ρύθμισης παραμέτρων ως **ASRMobilityService**.

>[AZURE.NOTE] Να θυμάστε ότι για να αντικαταστήσετε το CSIP στη ρύθμιση παραμέτρων σας ώστε να αντικατοπτρίζει το πραγματικό διαχείρισης διακομιστή, ώστε ο παράγοντας θα συνδεθεί σωστά και θα χρησιμοποιήσει τη σωστή φράση πρόσβασης.

## <a name="step-3-upload-to-automation-dsc"></a>Βήμα 3: Αποστολή αυτοματισμού DSC

Επειδή η ρύθμιση παραμέτρων DSC που κάνατε θα εισαγάγει απαιτείται DSC πόρων λειτουργικής μονάδας (xPSDesiredStateConfiguration), πρέπει να εισαγάγετε αυτήν τη λειτουργική μονάδα στο αυτοματισμού, πριν από την αποστολή της ρύθμισης παραμέτρων DSC.

Πραγματοποιήστε είσοδο λογαριασμό σας Αυτοματισμός, μεταβείτε **παγίων** > **λειτουργικές μονάδες**, και κάντε κλικ στην επιλογή **Αναζήτηση συλλογή**.

Εδώ μπορείτε να κάνετε αναζήτηση για τη λειτουργική μονάδα και να τα εισαγάγετε στο λογαριασμό σας.

![Λειτουργική μονάδα εισαγωγής](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Όταν ολοκληρώσετε, μεταβείτε στον υπολογιστή σας όπου έχετε τις ενότητες διαχείριση πόρων Azure εγκατασταθεί και να συνεχίσετε να εισαγάγουν τις παραμέτρους DSC που έχουν δημιουργηθεί πρόσφατα.

### <a name="import-cmdlets"></a>Cmdlet για εισαγωγή

Στο PowerShell, πραγματοποιήστε είσοδο στο Azure τη συνδρομή σας. Τροποποιήστε τα cmdlet για να αντικατοπτρίζει το περιβάλλον και καταγραφή πληροφοριών του λογαριασμού σας αυτοματισμού σε μεταβλητή:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Αποστολή της ρύθμισης παραμέτρων DSC αυτοματισμού, χρησιμοποιώντας το ακόλουθο cmdlet:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>Μεταγλώττιση τη ρύθμιση παραμέτρων σε DSC αυτοματισμού

Στη συνέχεια, πρέπει να μεταγλωττίσετε τη ρύθμιση παραμέτρων σε DSC αυτοματισμού, έτσι ώστε να μπορείτε να ξεκινήσετε να καταχωρήσετε κόμβους σε αυτήν. Για να επιτύχετε που, εκτελώντας το ακόλουθο cmdlet:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Αυτό μπορεί να χρειαστούν μερικά λεπτά, επειδή στην ουσία την ανάπτυξη για τη ρύθμιση παραμέτρων για την υπηρεσία ελκυστική DSC.

Μετά τη μεταγλώττιση, τη ρύθμιση παραμέτρων, μπορείτε να ανακτήσετε πληροφορίες για την εργασία με χρήση του PowerShell (Get-AzureRmAutomationDscCompilationJob) ή με τη χρήση του [Azure πύλη](https://portal.azure.com/).

![Ανακτήσετε την εργασία](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Έχετε τώρα με επιτυχία δημοσιευτεί και αποστέλλεται της ρύθμισης παραμέτρων DSC DSC αυτοματισμού.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Βήμα 4: Ενσωματωμένη μηχανές να Αυτοματισμός DSC
>[AZURE.NOTE] Μία από τις προϋποθέσεις για την ολοκλήρωση αυτό το σενάριο είναι ότι οι υπολογιστές σας Windows έχουν ενημερωθεί με την πιο πρόσφατη έκδοση του WMF. Μπορείτε να κάνετε λήψη και να εγκαταστήσετε τη σωστή έκδοση για την πλατφόρμα σας από το [Κέντρο λήψης](https://www.microsoft.com/download/details.aspx?id=50395).

Τώρα θα δημιουργήσετε μια metaconfig για DSC που θέλετε να συσχετίσετε με κόμβους σας. Για να ολοκληρωθεί με επιτυχία με αυτό, πρέπει να ανακτήσετε τη διεύθυνση URL τελικού σημείου και του πρωτεύοντος κλειδιού για το λογαριασμό σας αυτοματισμού επιλεγμένο στο Azure. Μπορείτε να βρείτε αυτές τις τιμές στην περιοχή **αριθμών-κλειδιών** σε το blade **όλες τις ρυθμίσεις** για το λογαριασμό αυτοματισμού.

![Βασικές τιμές](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

Σε αυτό το παράδειγμα, έχετε ένα διακομιστή φυσικής Windows Server 2012 R2 που θέλετε να προστατεύσετε χρησιμοποιώντας Επαναφορά τοποθεσίας.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Έλεγχος για οποιεσδήποτε εκκρεμείς λειτουργίες μετονομασίας αρχείων από το μητρώο

Πριν να ξεκινήσετε να συσχετίσετε το διακομιστή με το τελικό σημείο DSC Αυτοματισμός, συνιστάται να ελέγξετε για οποιαδήποτε σε εκκρεμότητα λειτουργίες μετονομασίας αρχείων στο μητρώο. Αυτά ενδέχεται να εμποδίζουν τη διαμόρφωση της τελικής επεξεργασίας λόγω επανεκκίνηση σε εκκρεμότητα.

Εκτελέστε το ακόλουθο cmdlet για να επαληθεύσετε ότι δεν υπάρχει καμία σε εκκρεμότητα επανεκκινήστε τον υπολογιστή στο διακομιστή:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Εάν η επιλογή αυτή εμφανίζει κενή, είναι OK για να συνεχίσετε. Εάν όχι, θα πρέπει να αντιμετωπίσετε αυτό με την επανεκκίνηση του διακομιστή κατά τη διάρκεια μιας συντήρησης.

Για να εφαρμόσετε τη ρύθμιση παραμέτρων του διακομιστή, ξεκινήστε το περιβάλλον ενσωματωμένη δέσμες ενεργειών στο PowerShell (ISE) και εκτελέστε την ακόλουθη δέσμη ενεργειών. Αυτό είναι ουσιαστικά DSC τοπική ρύθμιση παραμέτρων που θα ζητήσετε από το μηχανισμό τοπική Configuration Manager για να εγγραφείτε στην υπηρεσία DSC αυτοματισμού και να ανακτήσετε τη συγκεκριμένη ρύθμιση παραμέτρων (ASRMobilityService.localhost).

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Αυτή η ρύθμιση παραμέτρων θα προκαλέσει το μηχανισμό τοπική Configuration Manager να καταχωρηθεί με DSC αυτοματισμού. Αυτό θα καθορίσει επίσης πώς θα πρέπει να λειτουργούν το μηχανισμό, τι θα πρέπει να κάνει εάν υπάρχει μια μετατόπιση ρύθμισης παραμέτρων (ApplyAndAutoCorrect) και τον τρόπο αυτό θα πρέπει να συνεχίσετε με τη ρύθμιση παραμέτρων εάν απαιτείται επανεκκίνηση.

Αφού εκτελέσετε αυτήν τη δέσμη ενεργειών, πρέπει να ξεκινά ο κόμβος για την καταχώρηση με DSC αυτοματισμού.

![Καταχώρηση κόμβο σε εξέλιξη](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Εάν επιστρέψετε στην πύλη του Azure, μπορείτε να δείτε ότι ο κόμβος που μόλις καταχωρημένες εμφανίστηκε τώρα στην πύλη.

![Καταχωρημένες κόμβου στην πύλη](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Στο διακομιστή, μπορείτε να εκτελέσετε το ακόλουθο cmdlet του PowerShell για να επαληθεύσετε ότι ο κόμβος έχει καταχωρηθεί σωστά:

```powershell
Get-DscLocalConfigurationManager
```

Μετά τη ρύθμιση παραμέτρων έχει τα μηνύματα μεταφέρονται και εφαρμόζεται στο διακομιστή, μπορείτε να το επαληθεύσετε, εκτελώντας το ακόλουθο cmdlet:

```powershell
Get-DscConfigurationStatus
```

Το αποτέλεσμα εμφανίζει ότι ο διακομιστής έχει με επιτυχία τα μηνύματα μεταφέρονται της ρύθμισης παραμέτρων:

![Εξόδου](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Επιπλέον, τη ρύθμιση υπηρεσία φορητότητα διαθέτει δικό του αρχείου καταγραφής που μπορείτε να βρείτε στο *SystemDrive*\ProgramData\ASRSetupLogs.

Που είναι. Έχετε τώρα με επιτυχία αναπτύσσεται και καταχωρηθεί η υπηρεσία φορητότητα στον υπολογιστή που θέλετε να προστατεύσετε χρησιμοποιώντας Επαναφορά τοποθεσίας. DSC θα βεβαιωθείτε ότι οι υπηρεσίες απαιτείται εκτελούνται πάντα.

![Επιτυχής ανάπτυξης](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Μετά το διακομιστή διαχείρισης εντοπίσει την επιτυχή ανάπτυξη, μπορείτε να ρυθμίσετε τις παραμέτρους προστασίας και να ενεργοποιήσετε την αναπαραγωγή στον υπολογιστή με τη χρήση Επαναφορά τοποθεσίας.

## <a name="use-dsc-in-disconnected-environments"></a>Χρήση DSC στα περιβάλλοντα αποσύνδεσης

Εάν σας μηχανές δεν είστε συνδεδεμένοι στο Internet, θα εξακολουθούν να μπορούν να βασίζεστε στη DSC για να αναπτύξετε και να ρυθμίσετε την υπηρεσία φορητότητα στον το φόρτο εργασίας που θέλετε να προστατεύσετε.

Που μπορεί να εμφανίσει το δικό σας διακομιστή ελκυστική DSC στο περιβάλλον σας για την παροχή ουσιαστικά τις ίδιες δυνατότητες που μπορείτε να πάρετε από το DSC αυτοματισμού. Αυτό σημαίνει ότι οι υπολογιστές-πελάτες θα τραβήξτε τη ρύθμιση παραμέτρων (αφού έχει καταχωρηθεί) για το τελικό σημείο DSC. Ωστόσο, μια άλλη επιλογή είναι να push με μη αυτόματο τρόπο τις παραμέτρους DSC σε υπολογιστές σας, τοπικά ή απομακρυσμένα.

Σημειώστε ότι σε αυτό το παράδειγμα, υπάρχει μια πρόσθετη παράμετρο για το όνομα του υπολογιστή. Τα απομακρυσμένα αρχεία που βρίσκονται τώρα σε έναν απομακρυσμένο κοινόχρηστο που θα πρέπει να είναι δυνατή η πρόσβαση από τα μηχανήματα που θέλετε να προστατεύσετε. Τέλος της δέσμης ενεργειών θεσπίζει τη ρύθμιση παραμέτρων και, στη συνέχεια, ξεκινά για να εφαρμόσετε τις ρυθμίσεις παραμέτρων DSC στον υπολογιστή προορισμού.

### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Βεβαιωθείτε ότι έχει εγκατασταθεί της λειτουργικής μονάδας PowerShell xPSDesiredStateConfiguration. Για μηχανές Windows όπου είναι εγκατεστημένο το WMF 5.0, μπορείτε να εγκαταστήσετε τη λειτουργική μονάδα xPSDesiredStateConfiguration, εκτελώντας το ακόλουθο cmdlet του στους υπολογιστές προορισμού:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Επίσης, μπορείτε να κάνετε λήψη και να αποθηκεύσετε τη λειτουργική μονάδα, σε περίπτωση που πρέπει να διανείμετε σε υπολογιστές των Windows που έχουν WMF 4.0. Εκτελέσετε αυτό το cmdlet σε έναν υπολογιστή όπου υπάρχει PowerShellGet (WMF 5.0):

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Επίσης για WMF 4.0, βεβαιωθείτε ότι έχει εγκατασταθεί το [Windows 8.1 ενημέρωση KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) σε υπολογιστές.

Μηχανές Windows που έχουν WMF 5.0 και WMF 4.0 μπορεί να προωθηθεί τις ακόλουθες ρυθμίσεις παραμέτρων:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Εάν θέλετε να ξεκινήσει το δικό σας διακομιστή ελκυστική DSC στο εταιρικό σας δίκτυο για να μιμηθείτε τις δυνατότητες που μπορείτε να λάβετε από DSC αυτοματισμού, ανατρέξτε στο θέμα [για τη ρύθμιση του διακομιστή web ελκυστική DSC](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Προαιρετικό: Ανάπτυξη μια ρύθμιση παραμέτρων DSC με τη χρήση ενός προτύπου για τη διαχείριση πόρων Azure

Σε αυτό το άρθρο περιλαμβάνει εστιασμένη σε πώς μπορείτε να δημιουργήσετε το δικό σας DSC ρύθμισης παραμέτρων για την αυτόματη ανάπτυξη της υπηρεσίας φορητότητα και τον παράγοντα Εικονική Azure--και βεβαιωθείτε ότι που εκτελούνται σε υπολογιστές που θέλετε να προστατεύσετε. Έχουμε επίσης ένα πρότυπο από διαχειριστή πόρων Azure που θα αναπτύξετε αυτήν τη ρύθμιση παραμέτρων DSC σε ένα νέο ή υπάρχοντα λογαριασμό Azure αυτοματισμού. Το πρότυπο θα χρησιμοποιήσει παραμέτρους εισόδου για να δημιουργήσετε Αυτοματισμός περιουσιακών στοιχείων που θα περιέχει τις μεταβλητές για το περιβάλλον σας.

Μετά την ανάπτυξη του προτύπου, μπορείτε να ανατρέξετε στο βήμα 4 σε αυτόν τον οδηγό για να ενσωματωμένη απλώς υπολογιστές σας.

Το πρότυπο θα κάντε τα εξής:

1. Χρήση ενός υπάρχοντος λογαριασμού αυτοματισμού ή δημιουργήστε ένα νέο
2. Λήψη παραμέτρων εισαγωγής για:
    - ASRRemoteFile--στη θέση όπου έχετε αποθηκεύσει τη ρύθμιση υπηρεσία φορητότητα
    - ASRPassphrase--στη θέση όπου έχετε αποθηκεύσει το αρχείο passphrase.txt
    - ASRCSEndpoint--τη διεύθυνση IP του διακομιστή διαχείρισης
3. Εισαγωγή της λειτουργικής μονάδας PowerShell xPSDesiredStateConfiguration
4. Δημιουργία και τη ρύθμιση παραμέτρων DSC μεταγλώττιση

Όλα τα προηγούμενα βήματα θα συμβεί στη σωστή σειρά, ώστε να μπορείτε να ξεκινήσετε προσθήκης λογαριασμών υπολογιστές σας για την προστασία.

Το πρότυπο, με οδηγίες για την ανάπτυξη, βρίσκεται στο [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Ανάπτυξη του προτύπου με χρήση του PowerShell:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Επόμενα βήματα

Μετά την ανάπτυξη των υπαλλήλων υπηρεσίας φορητότητας, μπορείτε να [ενεργοποιήσετε την αναπαραγωγή](site-recovery-vmware-to-azure.md#step-6-replicate-applications) για τις εικονικές μηχανές.
