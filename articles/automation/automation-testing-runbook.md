<properties 
    pageTitle="Δοκιμή μια runbook στο Azure αυτοματισμού | Microsoft Azure"
    description="Πριν να δημοσιεύσετε ένα runbook στο Azure αυτοματισμού, μπορείτε να ελέγξετε για να βεβαιωθείτε ότι που λειτουργεί όπως αναμένεται.  Σε αυτό το άρθρο περιγράφει τον τρόπο για να δοκιμάσετε μια runbook και να προβάλετε τα αποτελέσματά."
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

# <a name="testing-a-runbook-in-azure-automation"></a>Δοκιμή μια runbook στο αυτοματισμού Azure
Κατά τη δοκιμή μια runbook, εκτελείται η [πρόχειρη έκδοση](automation-creating-importing-runbook.md#publishing-a-runbook) και τυχόν ενέργειες που εκτελεί έχουν ολοκληρωθεί. Δημιουργείται χωρίς ιστορικού εργασίας, αλλά εμφανίζονται ροών [εξόδου](automation-runbook-output-and-messages.md#output-stream) και [προειδοποιήσεις και σφάλματα](automation-runbook-output-and-messages.md#message-streams) κατά τη δοκιμή εξόδου παραθύρου. Μηνύματα στη [ροή λεπτομερής](automation-runbook-output-and-messages.md#message-streams) εμφανίζονται στο παράθυρο εξόδου μόνο εάν έχει οριστεί η [μεταβλητή $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) Continue.

Παρόλο που η πρόχειρη έκδοση εκτελείται, runbook εκτελεί τη ροή εργασίας κανονικά εξακολουθεί να και εκτελεί τις ενέργειες σε σχέση με τους πόρους στο περιβάλλον. Για αυτόν το λόγο, θα πρέπει να δοκιμάσετε μόνο runbooks στο μη παραγωγής πόρους.

Η διαδικασία για να ελέγξετε κάθε [τύπο του runbook](automation-runbook-types.md) είναι η ίδια και δεν υπάρχει διαφορά κατά τη δοκιμή μεταξύ του προγράμματος επεξεργασίας κειμένου και το πρόγραμμα επεξεργασίας γραφικών στην πύλη του Azure.  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>Για να δοκιμάσετε μια runbook στην πύλη του Azure

Μπορείτε να εργαστείτε με οποιονδήποτε [τύπο runbook](automation-runbook-types.md) στην πύλη του Azure.

1. Ανοίξτε την πρόχειρη έκδοση του runbook στο [πρόγραμμα επεξεργασίας κειμένου](automation-editing-a-runbook.md#Portal) ή [γραφικών επεξεργασίας](automation-graphical-authoring-intro.md).
2. Κάντε κλικ στο κουμπί **Έλεγχος** για να ανοίξετε το blade δοκιμής.
3. Εάν runbook έχει παραμέτρους, θα παρατεθούν στο αριστερό παράθυρο όπου μπορείτε να παράσχετε τιμές που θα χρησιμοποιηθεί για τον έλεγχο.
4. Εάν θέλετε να εκτελέσετε τον έλεγχο σε [Υβριδική Runbook εργασίας](automation-hybrid-runbook-worker.md), στη συνέχεια, αλλαγή **Ρυθμίσεων εκτέλεση** για να **Υβριδική εργασίας** και επιλέξτε το όνομα της ομάδας προορισμού.  Διαφορετικά, διατηρήστε την προεπιλογή **Azure** για να εκτελέσετε τον έλεγχο στο cloud.
5. Κάντε κλικ στο κουμπί **Έναρξη** , για να ξεκινήσετε τον έλεγχο.
6. Εάν runbook είναι [Ροή εργασίας του PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) ή [Graphical](automation-runbook-types.md#graphical-runbooks), στη συνέχεια, μπορείτε να διακόψετε ή την αναστολή ενώ ελέγχεται με τα κουμπιά κάτω από το παράθυρο εξόδου. Κατά την αναστολή runbook, ολοκληρώνει την τρέχουσα δραστηριότητά πριν από την αναστολή. Όταν τίθεται σε αναστολή runbook, μπορείτε να διακόψετε ή να επανεκκινήστε το.
7. Έλεγχος την έξοδο από το runbook στο παράθυρο εξόδου.


## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε πώς μπορείτε να δημιουργήσετε ή να εισαγάγετε μια runbook, ανατρέξτε στο θέμα [Δημιουργία ή εισαγωγή ενός runbook στο Azure Automation](automation-creating-importing-runbook.md)
- Για να μάθετε περισσότερα σχετικά με τη σύνταξη γραφικών, ανατρέξτε στο θέμα [Graphical σύνταξη από κοινού με αυτοματισμού Azure](automation-graphical-authoring-intro.md)
- Για να ξεκινήσετε με το PowerShell runbooks ροής εργασίας, ανατρέξτε στο θέμα [μου πρώτη runbook PowerShell ροής εργασίας](automation-first-runbook-textual.md)
- Για να μάθετε περισσότερα σχετικά με τη ρύθμιση των παραμέτρων runboks για να λάβετε μηνύματα κατάστασης και των σφαλμάτων, συμπεριλαμβανομένων των προτεινόμενων πρακτικές, ανατρέξτε στο θέμα [Runbook εξόδου και μηνύματα σε αυτοματισμού Azure](automation-runbook-output-and-messages.md)