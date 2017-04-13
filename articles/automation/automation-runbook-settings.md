<properties 
   pageTitle="Ρυθμίσεις Runbook"
   description="Περιγράφει τις ρυθμίσεις παραμέτρων για μια runbook στο αυτοματισμού Azure και πώς μπορείτε να αλλάξετε τους χρησιμοποιώντας την πύλη διαχείρισης του Azure και το Windows PowerShell."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />

# <a name="runbook-settings"></a>Ρυθμίσεις Runbook

Κάθε runbook στο Azure αυτοματισμού έχει πολλές ρυθμίσεις που σας βοηθούν να γίνει αναγνώριση των στοιχείων και για να αλλάξετε τη συμπεριφορά της καταγραφής. Κάθε μία από αυτές τις ρυθμίσεις περιγράφεται παρακάτω ακολουθούμενο από διαδικασίες σχετικά με τον τρόπο για να τους τροποποιήσετε.

## <a name="settings"></a>Ρυθμίσεις

### <a name="name-and-description"></a>Όνομα και περιγραφή

Δεν μπορείτε να αλλάξετε το όνομα του runbook μια αφού έχει δημιουργηθεί. Η περιγραφή είναι προαιρετικό και μπορεί να περιέχει έως 512 χαρακτήρες.

### <a name="tags"></a>Ετικέτες

Οι ετικέτες σάς επιτρέπουν να αντιστοιχίσετε ξεχωριστές λέξεις και φράσεις που θα σας βοηθήσουν να εντοπίσετε μια runbook. Για παράδειγμα, κατά την υποβολή μιας runbook στη [Συλλογή Runbook](https://msdn.microsoft.com/library/dn781422.aspx), μπορείτε να καθορίσετε συγκεκριμένες ετικέτες για να προσδιορίσετε ποιες κατηγορίες runbook πρέπει να εμφανίζεται στο. Μπορείτε να καθορίσετε πολλές ετικέτες για μια runbook διαχωρίζοντάς τις με ερωτηματικά.

### <a name="logging"></a>Καταγραφή

Από προεπιλογή, λεπτομερής και την πρόοδό εγγραφές δεν εγγράφονται ιστορικού εργασίας. Μπορείτε να αλλάξετε τις ρυθμίσεις για μια συγκεκριμένη runbook για να συνδεθείτε αυτών των εγγραφών. Για περισσότερες πληροφορίες σχετικά με αυτές τις εγγραφές, ανατρέξτε στο θέμα [Runbook εξόδου και τα μηνύματα](https://msdn.microsoft.com/library/dn879148.aspx).

## <a name="changing-runbook-settings"></a>Αλλαγή των ρυθμίσεων runbook

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>Αλλαγή των ρυθμίσεων runbook με την πύλη διαχείρισης του Azure

Μπορείτε να αλλάξετε τις ρυθμίσεις για μια runbook στην πύλη διαχείρισης του Azure από τη σελίδα **Ρύθμιση παραμέτρων** για runbook.

1. Στην πύλη διαχείρισης του Azure, επιλέξτε **Αυτοματοποίηση** και, στη συνέχεια, στη συνέχεια, κάντε κλικ στο όνομα ενός λογαριασμού αυτοματισμού.
1. Επιλέξτε την καρτέλα **Runbooks** .
1. Κάντε κλικ στο όνομα ενός runbook.
1. Επιλέξτε την καρτέλα **Ρύθμιση παραμέτρων** .

### <a name="changing-runbook-settings-with-windows-powershell"></a>Αλλαγή των ρυθμίσεων runbook με το Windows PowerShell

Μπορείτε να χρησιμοποιήσετε το cmdlet [Set-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) για να αλλάξετε τις ρυθμίσεις για μια runbook. Εάν θέλετε να καθορίσετε πολλές ετικέτες, μπορείτε είτε να παράσχετε έναν πίνακα ή μία συμβολοσειρά με οριοθετημένο με κόμματα τιμές για την παράμετρο ετικέτες. Μπορείτε να λάβετε την τρέχουσα ετικέτες με το [Get-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx).

Τα ακόλουθα δείγματα εντολών δείχνουν πώς μπορείτε να ορίσετε τις ιδιότητες για ένα runbook. Αυτό το δείγμα προσθέτει τρεις ετικετών για τις υπάρχουσες ετικέτες και καθορίζει ότι θα καταγράφονται λεπτομερές εγγραφές.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>Σχετικά άρθρα
- [Αποτέλεσμα Runbook και μηνύματα](../automation-runbook-output-and-messages) 
- [Δημιουργία ή εισαγωγή ενός Runbook](https://msdn.microsoft.com/library/dn643637.aspx) 