<properties
    pageTitle="Χρήση των εφαρμογών του App-V με Azure RemoteApp | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε εφαρμογές App-V στο Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="using-app-v-apps-in-azure-remoteapp"></a>Χρήση των εφαρμογών του App-V στο Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Μπορείτε να χρησιμοποιήσετε τις εφαρμογές App-V σε μια συλλογή υβριδική Azure RemoteApp, που απαιτεί συμμετοχή σε τομέα.

Πριν ξεκινήσετε, βεβαιωθείτε ότι έχετε εγκαταστήσει το πρόγραμμα-πελάτη App-V 5.1 με τις πιο πρόσφατες ενημερώσεις. Θα πρέπει να δημιουργήσετε μια [προσαρμοσμένη εικόνα](remoteapp-create-custom-image.md) που περιλαμβάνει το πρόγραμμα-πελάτη App-V.  

Είναι εύκολο να χρησιμοποιήσετε την υπάρχουσα υποδομή App-V με Azure RemoteApp. Δεδομένου ότι μια συλλογή υβριδική έχει αναπτυχθεί σε μια VNET Azure που έχει πρόσβαση σε ελεγκτή του τομέα σας και του ΣΠΣ έχουν συνδεθεί τομέα, μπορείτε να αξιοποιήσετε σας υπάρχουσα εφαρμογή-v υποδομής και ανάπτυξη μεθόδους για να easyily εφαρμογή App-V κεντρικού υπολογιστή στο Azure RemoteApp. Εδώ θα βρείτε ορισμένα ζητήματα που θα πρέπει να γνωρίζετε με βάση τον τύπο του App-V ανάπτυξης που έχετε τη συγκεκριμένη στιγμή:

| Επιλογές ρύθμισης παραμέτρων |                       | Θετικό                                                               | Αρνητική                                                                                              |
|-----------------------|-----------------------|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Μέθοδος παράδοσης       | Η ροή (σε ζήτηση) | Η εφαρμογή είναι πάντα τις πιο πρόσφατες ενημερώσεις και γλυκών                                     | Ο λανθάνων χρόνος πρώτη φορά                                                                                    |
|                       | Ενεργοποιηθεί               | Πιο ασφαλής; εφαρμογή υπάρχει ήδη στην την εικονική Μηχανή                              | Διόγκωση - παίρνει χώρου εικόνας (το όριο 127 GB)                                                           |
| Εφαρμογή θέση αποθήκευσης  | Κοινόχρηστο περιεχόμενο        | Εφαρμογή εκτελείται στη μνήμη Azure RemoteApp παρουσίας                         | Τρώει μνήμη και καλή σύνδεση ροής (αρχείο) διακομιστή όπου βρίσκεται η εφαρμογή                      |
|                       | Δίσκο (Cached)         | Γρήγορη εκτέλεση. Εφαρμογή δεν εξαρτάται από το διαθεσιμότητα προέλευση περιεχομένου | Διόγκωση - παίρνει χώρου εικόνας (το όριο 127 GB)                                                           |
| Στόχευση             | Χρήστη                  | Απαιτεί πλήρη μεμονωμένη υποδομή App-V                          |                                                                                                       |
|                       | Καθολικό πρότυπο (υπολογιστή)      |  Προ-δημοσίευση ή στόχου χρησιμοποιώντας δημοσίευσης διακομιστή                         |  Πρέπει να ενημερώσετε την εικόνα σας Azure, εάν θέλετε να ενημερώσετε την εφαρμογή (πολύ μεγάλο). Καταλαμβάνει χώρο στην εικόνα. |

 Μετά τη δημιουργία προσαρμοσμένου την εικόνα σας και σας συλλογή υβριδική, δημοσιεύστε την εφαρμογή σας, εκχωρήσετε στους χρήστες και απολαύστε τις υπάρχουσες εφαρμογές App-V σε Azure RemoteApp παραδόθηκε σε οποιαδήποτε συσκευή σε οποιοδήποτε σημείο.