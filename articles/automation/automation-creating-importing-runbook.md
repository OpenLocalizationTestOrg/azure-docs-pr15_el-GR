<properties
    pageTitle="Δημιουργία ή εισαγωγή ενός runbook στο Azure Automation"
    description="Σε αυτό το άρθρο περιγράφει τον τρόπο για να δημιουργήσετε μια νέα runbook στο Azure Automation ή εισαγάγετε έναν από ένα αρχείο."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Δημιουργία ή εισαγωγή ενός runbook στο Azure Automation

Μπορείτε να προσθέσετε μια runbook αυτοματισμού Azure είτε [τη δημιουργία μιας νέας](#creating-a-new-runbook) ή με την εισαγωγή ενός υπάρχοντος runbook από ένα αρχείο ή από τη [Συλλογή Runbook](automation-runbook-gallery.md). Σε αυτό το άρθρο παρέχει πληροφορίες για τη δημιουργία και εισαγωγή runbooks από αρχείο.  Μπορείτε να λάβετε όλες τις λεπτομέρειες σχετικά με την πρόσβαση runbooks Κοινότητας και λειτουργικών μονάδων σε [συλλογές Runbook και λειτουργική μονάδα για την αυτοματοποίηση Azure](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Δημιουργία μιας νέας runbook

Μπορείτε να δημιουργήσετε μια νέα runbook στην αυτοματοποίηση Azure χρησιμοποιώντας μία από τις Azure πύλες ή του Windows PowerShell. Μόλις δημιουργηθεί runbook, μπορείτε να επεξεργαστείτε χρησιμοποιώντας τις πληροφορίες σε [Ροή εργασίας του PowerShell εκμάθησης](automation-powershell-workflow.md) και [γραφικών σύνταξη από κοινού με Azure αυτοματισμού](automation-graphical-authoring-intro.md).

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Για να δημιουργήσετε μια νέα runbook Azure αυτοματισμού με την πύλη κλασική Azure

Μπορείτε να εργαστείτε μόνο με [ροή εργασίας του PowerShell runbooks](automation-runbook-types.md#powershell-workflow-runbooks) στην πύλη του Azure.

1. Στην κλασική Azure πύλη, κάντε κλικ στην επιλογή, **Δημιουργία**, **εφαρμογή υπηρεσιών**, **αυτοματισμού**, **Runbook**, **Γρήγορης δημιουργίας**.
2. Καταχωρήστε τις απαιτούμενες πληροφορίες και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**. Το όνομα του runbook πρέπει να ξεκινούν με γράμμα και μπορούν να έχουν γράμματα, αριθμούς, χαρακτήρες υπογράμμισης και παύλες.
3. Εάν θέλετε να επεξεργαστείτε runbook τώρα και, στη συνέχεια, κάντε κλικ στην επιλογή **Επεξεργασία Runbook**. Διαφορετικά, κάντε κλικ στο **κουμπί OK**.
4. Το νέο runbook θα εμφανίζονται στην καρτέλα **Runbooks** .


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Για να δημιουργήσετε μια νέα runbook Azure αυτοματισμού με την πύλη του Azure

1. Στην πύλη του Azure, ανοίξτε το λογαριασμό σας αυτοματισμού.
2. Κάντε κλικ στο πλακίδιο **Runbooks** για να ανοίξετε τη λίστα των runbooks.
3. Κάντε κλικ στο κουμπί **Προσθήκη ενός runbook** και, στη συνέχεια, **Δημιουργία μιας νέας runbook**.
2. Πληκτρολογήστε ένα **όνομα** για το runbook και επιλέξτε τον [τύπο](automation-runbook-types.md). Το όνομα του runbook πρέπει να ξεκινούν με γράμμα και μπορούν να έχουν γράμματα, αριθμούς, χαρακτήρες υπογράμμισης και παύλες.
3. Κάντε κλικ στην επιλογή **Δημιουργία** για να δημιουργήσετε runbook και ανοίξτε το πρόγραμμα επεξεργασίας.


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Για να δημιουργήσετε μια νέα runbook Azure αυτοματισμού με το Windows PowerShell

Μπορείτε να χρησιμοποιήσετε το cmdlet [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) για να δημιουργήσετε μια κενή [runbook PowerShell ροής εργασίας](automation-runbook-types.md#powershell-workflow-runbooks). Μπορείτε είτε να καθορίσετε το **όνομα** παραμέτρου για να δημιουργήσετε μια κενή runbook που μπορείτε να επεξεργαστείτε αργότερα ή μπορείτε να καθορίσετε την παράμετρο **διαδρομή** για να εισαγάγετε ένα αρχείο runbook. Η παράμετρος **τύπου** πρέπει να περιλαμβάνονται επίσης για να καθορίσετε έναν από τους τέσσερις τύπους runbook.

Τα ακόλουθα δείγματα εντολών δείχνουν πώς μπορείτε να δημιουργήσετε μια νέα κενή runbook.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Εισαγωγή ενός runbook από ένα αρχείο σε αυτοματισμού Azure

Μπορείτε να δημιουργήσετε μια νέα runbook στο Azure αυτοματισμού, εισάγοντας μια δέσμη ενεργειών PowerShell ή PowerShell ροής εργασίας (επέκταση .ps1) ή ένα εξαγόμενο γραφικών runbook (.graphrunbook).  Πρέπει να καθορίσετε τον [τύπο του runbook](automation-runbook-types.md) που θα δημιουργηθεί από την εισαγωγή λαμβάνοντας υπόψη τα παρακάτω θέματα.

- Ένα αρχείο .graphrunbook μπορεί να εισαχθούν μόνο σε ένα νέο [γραφικό runbook](automation-runbook-types.md#graphical-runbooks)και γραφικών runbooks μπορούν να δημιουργηθούν μόνο από ένα αρχείο .graphrunbook.
- Ένα αρχείο .ps1 που περιέχει μια ροή εργασίας PowerShell μπορούν να εισαχθούν μόνο σε μια [ροή εργασίας PowerShell runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Εάν το αρχείο περιέχει πολλές ροές εργασίας PowerShell, στη συνέχεια, η εισαγωγή θα αποτύχει. Πρέπει να αποθηκεύσετε κάθε ροή εργασίας στο δικό του αρχείου και εισαγάγετε κάθε ξεχωριστά.
- Ένα αρχείο .ps1 που δεν περιέχει μια ροή εργασίας μπορεί να εισαχθεί σε ένα [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) ή μια [ροή εργασίας PowerShell runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Εάν εισάγεται σε μια ροή εργασίας του PowerShell runbook, στη συνέχεια, θα μετατραπεί σε μια ροή εργασίας και των σχολίων θα συμπεριληφθούν στην runbook που καθορίζει τις αλλαγές που έγιναν.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Για να εισαγάγετε μια runbook από ένα αρχείο με την πύλη κλασική Azure
Μπορείτε να χρησιμοποιήσετε την παρακάτω διαδικασία για να εισαγάγετε ένα αρχείο δέσμης ενεργειών στο Azure αυτοματισμού.  Σημειώστε ότι μπορείτε να εισαγάγετε μόνο ένα αρχείο .ps1 σε μια ροή εργασίας του PowerShell runbook χρησιμοποιώντας αυτήν την πύλη.  Πρέπει να χρησιμοποιήσετε το Azure πύλη για τους άλλους τύπους.

1. Στην πύλη του Azure διαχείρισης, επιλέξτε **Αυτοματοποίηση** και, στη συνέχεια, επιλέξτε ένα λογαριασμό αυτοματισμού.
2. Κάντε κλικ στην επιλογή **Εισαγωγή**.
3. Κάντε κλικ στο κουμπί **Αναζήτηση για το αρχείο** και εντοπίστε το αρχείο δέσμης ενεργειών για την εισαγωγή.
4. Εάν θέλετε να επεξεργαστείτε runbook τώρα και, στη συνέχεια, κάντε κλικ στην επιλογή **Επεξεργασία Runbook**. Διαφορετικά, κάντε κλικ στο κουμπί OK.
5. Νέα runbook θα εμφανίζονται στην καρτέλα **Runbooks** για το λογαριασμό αυτοματισμού.
6. Πρέπει να [δημοσιεύσετε runbook](#publishing-a-runbook) μπορείτε να το εκτελέσετε.


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Για να εισαγάγετε μια runbook από ένα αρχείο με την πύλη του Azure
Μπορείτε να χρησιμοποιήσετε την παρακάτω διαδικασία για να εισαγάγετε ένα αρχείο δέσμης ενεργειών στο Azure αυτοματισμού.  

>[AZURE.NOTE] Σημειώστε ότι μπορείτε να εισαγάγετε μόνο ένα αρχείο .ps1 σε μια ροή εργασίας του PowerShell runbook με την πύλη.

1. Στην πύλη του Azure, ανοίξτε το λογαριασμό σας αυτοματισμού.
2. Κάντε κλικ στο πλακίδιο **Runbooks** για να ανοίξετε τη λίστα των runbooks.
3. Κάντε κλικ στο κουμπί **Προσθήκη ενός runbook** και, στη συνέχεια, **Εισαγωγή**.
4. Κάντε κλικ στο **αρχείο Runbook** για να επιλέξετε το αρχείο προς εισαγωγή
2. Εάν το πεδίο **όνομα** είναι ενεργοποιημένη, στη συνέχεια, έχετε την επιλογή για να την αλλάξετε.  Το όνομα του runbook πρέπει να ξεκινούν με γράμμα και μπορούν να έχουν γράμματα, αριθμούς, χαρακτήρες υπογράμμισης και παύλες.
3. Ο [Τύπος runbook](automation-runbook-types.md) θα επιλεγεί αυτόματα, αλλά μπορείτε να αλλάξετε τον τύπο μετά τη λήψη τους περιορισμούς που εφαρμόζονται στο λογαριασμό. 
3. Νέα runbook θα εμφανίζονται στη λίστα των runbooks για το λογαριασμό αυτοματισμού.
4. Πρέπει να [δημοσιεύσετε runbook](#publishing-a-runbook) μπορείτε να το εκτελέσετε.

>[AZURE.NOTE] Μετά την εισαγωγή ενός γραφικού runbook ή ένα γραφικό runbook ροής εργασίας του PowerShell, έχετε την επιλογή για να μετατρέψετε τον άλλο τύπο Εάν θέλατε να επισημανθεί. Δεν μπορείτε να μετατρέψετε σε μορφή κειμένου.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Για να εισαγάγετε μια runbook από ένα αρχείο δέσμης ενεργειών με το Windows PowerShell

Μπορείτε να χρησιμοποιήσετε το cmdlet [AzureRMAutomationRunbook εισαγωγή](https://msdn.microsoft.com/library/mt603735.aspx) για να εισαγάγετε ένα αρχείο δέσμης ενεργειών ως Πρόχειρο runbook PowerShell ροής εργασίας. Εάν υπάρχει ήδη runbook, η εισαγωγή θα αποτύχει, εκτός εάν χρησιμοποιείτε το *-ισχύ* παραμέτρου. 

Τα ακόλουθα δείγματα εντολών δείχνουν πώς μπορείτε να εισαγάγετε ένα αρχείο δέσμης ενεργειών σε μια runbook.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Δημοσίευση μιας runbook

Όταν δημιουργείτε ή εισαγάγετε μια νέα runbook, πρέπει να δημοσιεύσετε το πριν να εκτελέσετε.  Κάθε runbook στο αυτοματισμού έχει ένα Πρόχειρο και μια δημοσιευμένη έκδοση. Μόνο η δημοσιευμένη έκδοση είναι διαθέσιμη για να εκτελείται και είναι δυνατή η επεξεργασία μόνο την πρόχειρη έκδοση. Η δημοσιευμένη έκδοση δεν επηρεάζεται από τις αλλαγές στην έκδοση του Πρόχειρου. Όταν η πρόχειρη έκδοση πρέπει να είναι διαθέσιμα, στη συνέχεια, δημοσιεύετε το οποίο αντικαθιστά την έκδοση Published με την πρόχειρη έκδοση.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Για να δημοσιεύσετε μια runbook με την πύλη κλασική Azure

1. Άνοιγμα runbook στην πύλη του Azure κλασική.
1. Στο επάνω μέρος της οθόνης, κάντε κλικ στην επιλογή **συντάκτη**.
1. Στο κάτω μέρος της οθόνης, κάντε κλικ στην επιλογή **Δημοσίευση** και, στη συνέχεια, **Ναι** στο μήνυμα επιβεβαίωσης.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Για να δημοσιεύσετε μια runbook με την πύλη Azure

1. Άνοιγμα runbook στην πύλη του Azure.
1. Κάντε κλικ στο κουμπί **Επεξεργασία** .
1. Κάντε κλικ στο κουμπί **Δημοσίευση** και, στη συνέχεια, **Ναι** στο μήνυμα επιβεβαίωσης.


## <a name="to-publish-a-runbook-using-windows-powershell"></a>Για να δημοσιεύσετε μια runbook χρησιμοποιώντας το Windows PowerShell

Μπορείτε να χρησιμοποιήσετε το cmdlet [AzureRmAutomationRunbook δημοσίευση](https://msdn.microsoft.com/library/mt603705.aspx) για να δημοσιεύσετε μια runbook με το Windows PowerShell. Τα ακόλουθα δείγματα εντολών δείχνουν πώς μπορείτε να δημοσιεύσετε ένα δείγμα runbook.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Επόμενα βήματα
- Για να μάθετε πώς μπορείτε να επωφεληθείτε από την Runbook και συλλογή λειτουργική μονάδα PowerShell, ανατρέξτε στην ενότητα [συλλογές Runbook και λειτουργική μονάδα για την αυτοματοποίηση Azure](automation-runbook-gallery.md)
- Για να μάθετε περισσότερα σχετικά με την επεξεργασία του PowerShell και τη ροή εργασίας του PowerShell runbooks με ένα πρόγραμμα επεξεργασίας κειμένου, ανατρέξτε στο θέμα [Επεξεργασία κειμένου runbooks στο Azure Automation](automation-edit-textual-runbook.md)
- Για να μάθετε περισσότερα σχετικά με τη σύνταξη από κοινού runbook γραφικών, ανατρέξτε στο θέμα [Graphical σύνταξη από κοινού με αυτοματισμού Azure](automation-graphical-authoring-intro.md)
