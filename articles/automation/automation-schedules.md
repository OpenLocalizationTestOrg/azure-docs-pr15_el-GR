<properties 
   pageTitle="Χρονοδιαγράμματα στο Azure Automation | Microsoft Azure"
   description="Αυτοματοποίηση χρονοδιαγράμματα που χρησιμοποιούνται για να προγραμματίσετε runbooks στο Azure Automation ώστε να ξεκινά αυτόματα. Περιγράφει τον τρόπο δημιουργίας και διαχείρισης χρονοδιαγράμματος με έτσι ώστε να μπορείτε να ξεκινήσετε μια runbook αυτόματα σε μια συγκεκριμένη χρονική στιγμή ή σε ένα επαναλαμβανόμενο χρονοδιάγραμμα."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="mgoedtel" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>Προγραμματισμός μιας runbook στο Azure Automation

Για να προγραμματίσετε μια runbook στο Azure αυτοματισμού για Έναρξη από ένα καθορισμένο χρονικό διάστημα, συνδέετε με ένα ή περισσότερα προγράμματα. Ένα χρονοδιάγραμμα μπορεί να ρυθμιστεί για εκτέλεση μία φορά ή σε μια Επανεμφάνιση ωριαία είτε καθημερινή χρονοδιαγράμματος για runbooks στην πύλη του Azure κλασική και για runbooks στην πύλη του Azure, μπορείτε να επιπλέον προγραμματίσετε τους για εβδομαδιαία, μηνιαία, συγκεκριμένες ημέρες της εβδομάδας ή ημέρες του μήνα ή μια συγκεκριμένη ημέρα του μήνα.  Μια runbook μπορούν να συνδεθούν με πολλαπλά χρονοδιαγράμματα και ένα χρονοδιάγραμμα μπορεί να έχει πολλές runbooks συνδεδεμένα με αυτό.

>[AZURE.NOTE]  Χρονοδιαγράμματα δεν υποστηρίζουν αυτήν τη στιγμή DSC αυτοματισμού Azure ρυθμίσεις παραμέτρων.

## <a name="windows-powershell-cmdlets"></a>Cmdlet του Windows PowerShell

Τα cmdlet στον παρακάτω πίνακα χρησιμοποιούνται για τη δημιουργία και διαχείριση χρονοδιαγραμμάτων με το Windows PowerShell στο Azure αυτοματισμού. Στείλει ως μέρος του [λειτουργική μονάδα Azure PowerShell](../powershell-install-configure.md).

|Cmdlet για|Περιγραφή|
|:---|:---|
|**Cmdlet διαχείρισης πόρων του Azure**||
|[Get-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603733.aspx)|Ανακτά ένα χρονοδιάγραμμα.|
|[Νέα AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx)|Δημιουργεί ένα νέο χρονοδιάγραμμα.|
|[Κατάργηση AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603691.aspx)|Καταργεί ένα χρονοδιάγραμμα.|
|[Ορισμός AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx)|Ορίζει τις ιδιότητες για ένα υπάρχον χρονοδιάγραμμα.|
|[Get-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt619406.aspx)|Ανακτά προγραμματισμένη runbooks.|
|[REGISTER-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx)|Συσχετίζει ένα runbook με ένα χρονοδιάγραμμα.|
|[Κατάργηση της καταχώρησης AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603844.aspx)|Dissociates μια runbook από ένα χρονοδιάγραμμα.|
|**Azure cmdlet διαχείρισης υπηρεσίας**||
|[Get-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690274.aspx)|Ανακτά ένα χρονοδιάγραμμα.|
|[Νέα AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690271.aspx)|Δημιουργεί ένα νέο χρονοδιάγραμμα.|
|[Κατάργηση AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690279.aspx)|Καταργεί ένα χρονοδιάγραμμα.|
|[Ορισμός AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690270.aspx)|Ορίζει τις ιδιότητες για ένα υπάρχον χρονοδιάγραμμα.|
|[Get-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn913778.aspx)|Ανακτά προγραμματισμένη runbooks.|
|[REGISTER-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690265.aspx)|Συσχετίζει ένα runbook με ένα χρονοδιάγραμμα.|
|[Κατάργηση της καταχώρησης AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690273.aspx)|Dissociates μια runbook από ένα χρονοδιάγραμμα.|

## <a name="creating-a-schedule"></a>Δημιουργία ενός χρονοδιαγράμματος

Μπορείτε να δημιουργήσετε ένα νέο χρονοδιάγραμμα για runbooks στην πύλη Azure, στην κλασική πύλη ή με το Windows PowerShell. Έχετε επίσης την επιλογή να δημιουργήσετε ένα νέο χρονοδιάγραμμα όταν συνδέετε μια runbook σε ένα χρονοδιάγραμμα με το Azure πύλη κλασική ή Azure.

>[AZURE.NOTE] Όταν μπορείτε να συσχετίσετε ένα χρονοδιάγραμμα με μια runbook, αυτοματισμού αποθηκεύει τις τρέχουσες εκδόσεις των λειτουργικών μονάδων στο λογαριασμό σας και τις συνδέσεις που χρονοδιάγραμμα.  Αυτό σημαίνει ότι, εάν είχατε λειτουργικής μονάδας με την έκδοση 1.0 στο λογαριασμό σας όταν δημιουργήσατε ένα χρονοδιάγραμμα και, στη συνέχεια, να ενημερώσει τη λειτουργική μονάδα στην έκδοση 2.0, το χρονοδιάγραμμα θα συνεχίσει να χρησιμοποιεί 1.0.  Για να χρησιμοποιήσετε τη λειτουργική μονάδα ενημερωμένη έκδοση, πρέπει να δημιουργήσετε ένα νέο χρονοδιάγραμμα. 

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Για να δημιουργήσετε ένα νέο χρονοδιάγραμμα στην πύλη του Azure

1. Στην πύλη του Azure, από το λογαριασμό σας αυτοματισμού, κάντε κλικ στο πλακίδιο **περιουσιακών στοιχείων** για να ανοίξετε το blade **περιουσιακών στοιχείων** .
2. Κάντε κλικ στο πλακίδιο **χρονοδιαγράμματα** για να ανοίξετε το blade **χρονοδιαγράμματα** .
3. Κάντε κλικ στην επιλογή **Προσθήκη ένα χρονοδιάγραμμα** στο επάνω μέρος του blade.
4. Στη blade το **νέο χρονοδιάγραμμα** , πληκτρολογήστε ένα **όνομα** και, προαιρετικά, μια **Περιγραφή** για το νέο χρονοδιάγραμμα.
5. Επιλέξτε αν το χρονοδιάγραμμα θα εκτελεστεί μία φορά, ή σε μια επαναλαμβανόμενη εργασία χρονοδιάγραμμα, επιλέγοντας **μία φορά** ή **Περιοδικότητα**.  Εάν επιλέξετε **μία φορά** καθορίστε **ώρα έναρξης** και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.  Εάν επιλέξετε **Περιοδικότητα**, καθορίστε μια **ώρα έναρξης** και τη συχνότητα για πόσο συχνά θέλετε runbook για να επαναλάβετε - με **ώρα**, **ημέρα**, **εβδομάδα**ή ανά **μήνα**.  Εάν επιλέξετε **εβδομάδα** ή **μήνα** από την αναπτυσσόμενη λίστα, η **επιλογή "Περιοδικότητα"** θα εμφανίζεται το blade και κατά την επιλογή, θα εμφανιστεί η **επιλογή "Περιοδικότητα"** blade και μπορείτε να επιλέξετε την ημέρα της εβδομάδας, εάν έχετε επιλέξει **την εβδομάδα**.  Εάν έχετε επιλέξει **μήνα**, μπορείτε να επιλέξετε από τις **ημέρες της εβδομάδας** ή συγκεκριμένες ημέρες του μήνα στο ημερολόγιο και τέλος, κάντε που θέλετε να εκτελέσετε την τελευταία ημέρα του μήνα ή όχι και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.   

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Για να δημιουργήσετε ένα νέο χρονοδιάγραμμα στην πύλη του Azure κλασική

1. Στην κλασική Azure πύλη, επιλέξτε αυτοματοποίηση και, στη συνέχεια, στη συνέχεια, επιλέξτε το όνομα του λογαριασμού αυτοματισμού.
1. Επιλέξτε την καρτέλα **στοιχεία** .
1. Στο κάτω μέρος του παραθύρου, κάντε κλικ στην επιλογή **Προσθήκη ρύθμιση**.
1. Κάντε κλικ στην επιλογή **Προσθήκη χρονοδιάγραμμα**.
1. Πληκτρολογήστε ένα **όνομα** και προαιρετικά μια **Περιγραφή** για το νέο χρονοδιάγραμμα schedule.your θα εκτελεστεί **Μία φορά**, **ωριαία**, **ημερήσια**, **Εβδομαδιαία**ή **Μηνιαία**.
1. Καθορίστε **Ώρα έναρξης** και άλλες επιλογές ανάλογα με τον τύπο του χρονοδιαγράμματος που έχετε επιλέξει.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Για να δημιουργήσετε ένα νέο χρονοδιάγραμμα με το Windows PowerShell

Μπορείτε να χρησιμοποιήσετε το cmdlet [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) για να δημιουργήσετε ένα νέο χρονοδιάγραμμα στο Azure αυτοματισμού για κλασική runbooks ή τα cmdlet [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) για runbooks στην πύλη του Azure. Πρέπει να καθορίσετε την ώρα έναρξης για το χρονοδιάγραμμα και τη συχνότητα που πρέπει να εκτελέσει.

Τα ακόλουθα δείγματα εντολών δείχνει πώς μπορείτε να δημιουργήσετε ένα χρονοδιάγραμμα για το 15 και 30η κάθε μήνα, χρησιμοποιώντας μια cmdlet διαχείρισης πόρων Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

Τα ακόλουθα δείγματα εντολών δείχνουν τον τρόπο για να δημιουργήσετε ένα νέο χρονοδιάγραμμα που θα εκτελείται κάθε μέρα στο 3:30 μμ ξεκινώντας μια cmdlet διαχείρισης υπηρεσίας Azure σε 20 Ιανουαρίου 2015.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Σύνδεση ένα χρονοδιάγραμμα με μια runbook

Μια runbook μπορούν να συνδεθούν με πολλαπλά χρονοδιαγράμματα και ένα χρονοδιάγραμμα μπορεί να έχει πολλές runbooks συνδεδεμένα με αυτό. Εάν μια runbook έχει παραμέτρους, στη συνέχεια, μπορείτε να παράσχετε τιμές για αυτές. Πρέπει να δώσετε τιμές για οποιαδήποτε υποχρεωτικές παράμετροι και μπορεί να παρέχουν τιμές για οποιαδήποτε προαιρετικές παραμέτρους.  Αυτές οι τιμές θα χρησιμοποιηθεί κάθε φορά που ξεκινά με αυτό το χρονοδιάγραμμα runbook.  Μπορείτε να επισυνάψετε το ίδιο runbook άλλο χρονοδιάγραμμα και να καθορίσετε διαφορετικές τιμές παραμέτρου.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Για να συνδέσετε ένα χρονοδιάγραμμα με ένα runbook με την πύλη του Azure

1. Στην πύλη του Azure, από το λογαριασμό σας αυτοματισμού, κάντε κλικ στο πλακίδιο **Runbooks** για να ανοίξετε το blade **Runbooks** .
2. Κάντε κλικ στο όνομα του runbook για να προγραμματίσετε.
3. Εάν τη συγκεκριμένη στιγμή, runbook δεν είναι συνδεδεμένο σε ένα χρονοδιάγραμμα, στη συνέχεια, θα έχετε την επιλογή για να δημιουργήσετε ένα νέο χρονοδιάγραμμα ή να συνδέσετε ένα υπάρχον χρονοδιάγραμμα.  
4. Εάν runbook έχει παραμέτρους, μπορείτε να επιλέξετε την επιλογή **Modify εκτέλεση ρυθμίσεις (προεπιλογή: Azure)** και το blade **παράμετροι** παρουσιάζονται όπου μπορείτε να εισαγάγετε αντίστοιχα τις πληροφορίες.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Για να συνδέσετε ένα χρονοδιάγραμμα με ένα runbook με την πύλη κλασική του Azure

1. Στην κλασική Azure πύλη, επιλέξτε **Αυτοματοποίηση** και, στη συνέχεια, στη συνέχεια, κάντε κλικ στο όνομα ενός λογαριασμού αυτοματισμού.
2. Επιλέξτε την καρτέλα **Runbooks** .
3. Κάντε κλικ στο όνομα του runbook για να προγραμματίσετε.
4. Κάντε κλικ στην καρτέλα **Χρονοδιάγραμμα** .
5. Εάν τη συγκεκριμένη στιγμή, runbook δεν είναι συνδεδεμένο σε ένα χρονοδιάγραμμα, στη συνέχεια, θα έχετε την επιλογή για **σύνδεση σε ένα νέο χρονοδιάγραμμα** ή να **συνδέσετε ένα υπάρχον χρονοδιάγραμμα**.  Εάν runbook αυτήν τη στιγμή είναι συνδεδεμένο σε ένα χρονοδιάγραμμα, κάντε κλικ στην επιλογή **σύνδεση** στο κάτω μέρος του παραθύρου για να αποκτήσετε πρόσβαση σε αυτές τις επιλογές.
6. Εάν runbook έχει παραμέτρους, θα σας ζητηθεί για τις τιμές τους.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Για να συνδέσετε ένα χρονοδιάγραμμα με ένα runbook με το Windows PowerShell

Μπορείτε να χρησιμοποιήσετε τη [Συνάρτηση Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) για να συνδέσετε ένα χρονοδιάγραμμα σε κλασική runbook ή [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet για runbooks στην πύλη του Azure.  Μπορείτε να καθορίσετε τιμές για τις παραμέτρους του runbook με την παράμετρο παραμέτρους. Για περισσότερες πληροφορίες σχετικά με τον καθορισμό τιμές παραμέτρων, ανατρέξτε στο θέμα [ξεκινώντας μια Runbook στο Azure αυτοματισμού](automation-starting-a-runbook.md) .

Τα ακόλουθα δείγματα εντολών δείχνουν πώς μπορείτε να συνδέσετε ένα χρονοδιάγραμμα με μια runbook χρησιμοποιώντας ένα cmdlet διαχείρισης πόρων Azure με παραμέτρους.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
Τα ακόλουθα δείγματα εντολών δείχνουν πώς μπορείτε να συνδέσετε ένα χρονοδιάγραμμα χρησιμοποιώντας ένα cmdlet διαχείρισης υπηρεσίας Azure με παραμέτρους.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Απενεργοποίηση χρονοδιαγράμματος

Όταν απενεργοποιείτε ένα χρονοδιάγραμμα, οποιαδήποτε runbooks συνδεδεμένα με αυτό δεν είναι πλέον θα εκτελείται σε αυτό το χρονοδιάγραμμα. Με μη αυτόματο τρόπο, μπορείτε να απενεργοποιήσετε ένα χρονοδιάγραμμα ή να ορίσετε μια ώρα λήξης για τα χρονοδιαγράμματα με συχνότητα κατά τη δημιουργία τους. Όταν η ώρα λήξης, το χρονοδιάγραμμα θα απενεργοποιηθεί.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Για να απενεργοποιήσετε ένα χρονοδιάγραμμα από την πύλη του Azure

1. Στην πύλη του Azure, από το λογαριασμό σας αυτοματισμού, κάντε κλικ στο πλακίδιο **περιουσιακών στοιχείων** για να ανοίξετε το blade **περιουσιακών στοιχείων** .
2. Κάντε κλικ στο πλακίδιο **χρονοδιαγράμματα** για να ανοίξετε το blade **χρονοδιαγράμματα** .
2. Κάντε κλικ στο όνομα του χρονοδιαγράμματος για να ανοίξετε το blade λεπτομέρειες.
3. Αλλάξτε την επιλογή **ενεργοποιημένη** σε **όχι**.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Για να απενεργοποιήσετε ένα χρονοδιάγραμμα από την πύλη κλασική του Azure

Μπορείτε να απενεργοποιήσετε ένα χρονοδιάγραμμα στην πύλη του Azure κλασική από τη σελίδα λεπτομερειών χρονοδιαγράμματος για το χρονοδιάγραμμα.

1. Στην κλασική Azure πύλη, επιλέξτε αυτοματοποίηση και, στη συνέχεια, στη συνέχεια, κάντε κλικ στο όνομα ενός λογαριασμού αυτοματισμού.
1. Επιλέξτε την καρτέλα στοιχεία.
1. Κάντε κλικ στο όνομα του χρονοδιαγράμματος για να ανοίξετε τη σελίδα λεπτομερειών.
2. Αλλάξτε την επιλογή **ενεργοποιημένη** σε **όχι**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Για να απενεργοποιήσετε ένα χρονοδιάγραμμα με το Windows PowerShell

Μπορείτε να χρησιμοποιήσετε το cmdlet [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) για να αλλάξετε τις ιδιότητες ενός υπάρχοντος χρονοδιαγράμματος για μια κλασική runbook ή το cmdlet [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) για runbooks στην πύλη του Azure. Για να απενεργοποιήσετε το χρονοδιάγραμμα, καθορίστε **false** για την παράμετρο **IsEnabled** .

Τα ακόλουθα δείγματα εντολών δείχνουν πώς μπορείτε να απενεργοποιήσετε ένα χρονοδιάγραμμα για μια runbook χρησιμοποιώντας ένα cmdlet διαχείρισης πόρων Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

Τα ακόλουθα δείγματα εντολών δείχνουν πώς μπορείτε να απενεργοποιήσετε ένα χρονοδιάγραμμα χρησιμοποιώντας το cmdlet Azure υπηρεσία διαχείρισης.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Επόμενα βήματα

- Για να ξεκινήσετε με runbooks στο Azure αυτοματισμού, ανατρέξτε στο θέμα [ξεκινώντας μια Runbook στο Azure Automation](automation-starting-a-runbook.md) 