<properties
    pageTitle="Επισκόπηση των γραμμών σύνδεσης εφαρμογές λογικής | Microsoft Azure"
    description="Επισκόπηση των γραμμών σύνδεσης που μπορούν να χρησιμοποιηθούν σε μια εφαρμογή για λογική"
    services=""
    documentationCenter="" 
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>Χρήση γραμμών σύνδεσης σε μια εφαρμογή λογική

Γραμμές σύνδεσης παρέχει γρήγορη πρόσβαση σε συμβάντα, δεδομένων και τις ενέργειες σε υπηρεσίες, τα πρωτόκολλα και πλατφόρμες.  Την πλήρη λίστα των γραμμών σύνδεσης που υποστηρίζει λογική εφαρμογών μπορεί να [είναι εδώ](apis-list.md).  Γραμμές σύνδεσης μπορεί να χρησιμοποιηθεί ως ένα έναυσμα ή μια ενέργεια σε μια εφαρμογή λογικής και μπορεί να απαιτεί ρύθμιση παραμέτρων *σύνδεσης* για να χρησιμοποιήσετε (για παράδειγμα: εξουσιοδότησης ένα λογαριασμό Twitter για την πρόσβαση ή να δημοσιεύσετε εκ μέρους σας).

## <a name="basics"></a>Βασικά στοιχεία

Φιλοξενούμενη υπηρεσιών που μπορείτε να έχετε πρόσβαση ως μέρος μιας εφαρμογής λογικής σε ενοποίηση με άλλες υπηρεσίες όπως το Dynamics, Azure, Salesforce, [και περισσότερες](apis-list.md)είναι οι γραμμές σύνδεσης.  Είναι αναπτύσσεται και γίνεται από τη Microsoft, ώστε να μπορείτε να δημιουργήσετε ενοποίησης ροών εργασίας με κλίμακα, μετάδοσης και την ασφάλεια που λαμβάνονται φροντίσει.  Μπορείτε να προσθέσετε μια γραμμή σύνδεσης σε μια εφαρμογή λογικής, αναζήτηση και επιλέγοντας μια ενέργεια γραμμής σύνδεσης ή ένα έναυσμα στην περιοχή **Εμφάνιση Microsoft διαχειριζόμενων APIs**.

![Μενού "ενέργεια" για την επιλογή έναυσμα][1]

Κάθε γραμμή σύνδεσης ενέργειας ή έναυσμα θα έχει το σύνολο ιδιοτήτων για να ρυθμίσετε τις παραμέτρους της.  Μπορείτε να κάντε κλικ στα κουμπιά πληροφορίες για να μάθετε περισσότερα σχετικά με την ενέργεια ή αναφορά την τεκμηρίωση [για να μάθετε περισσότερα](apis-list.md).

Εάν θέλετε να ενσωματώσετε με μια υπηρεσία ή API που δεν είναι ακόμη μια γραμμή σύνδεσης, μπορείτε να επεκτείνετε εφαρμογές λογικής μέσω μια [προσαρμοσμένη γραμμή σύνδεσης](../app-service-logic/app-service-logic-create-api-app.md) ή απλώς κλήσεων απευθείας με την υπηρεσία πάνω από ένα πρωτόκολλο όπως HTTP.

## <a name="triggers"></a>Εναυσμάτων

Ορισμένες γραμμές σύνδεσης έχει ένα έναυσμα, γεγονός που σημαίνει ότι ένα συμβάν από αυτήν τη γραμμή σύνδεσης θα fire μια εφαρμογή λογικής και στο στοιχείο οποιαδήποτε δεδομένα ως μέρος του εναύσματος.  Ένα έναυσμα είναι πάντα το πρώτο βήμα σε μια εφαρμογή για λογική.  Δημοφιλείς εναύσματα περιλαμβάνουν λειτουργίες όπως:
 
 * Περιοδικότητα - εκτελέστε κάθε ώρα
 * Όταν λάβει μια αίτηση HTTP
 * Όταν προστίθεται ένα στοιχείο σε μια ουρά
 * Όταν λαμβάνεται μήνυμα ηλεκτρονικού ταχυδρομείου
 
Κάποια από τα εναύσματα θα εφαρμόζεται άμεσων συμβάντος συμβαίνει μέσω μια ειδοποίηση στην εφαρμογή λογική και άλλους θα χρειαστείτε ένα χρονικό διάστημα περιοδικότητας που ρυθμίσατε στο πόσο συχνά την εφαρμογή λογικής θα ελέγχει την υπηρεσία για ένα συμβάν (έως και κάθε 15 δευτερόλεπτα).  

Μετά τη λήψη ενός συμβάντος, θα εφαρμόζεται η εφαρμογή λογικής εκτέλεση και τις ενέργειες στη ροή εργασίας θα ξεκινήσει.  Επίσης θα μπορείτε να αποκτήσετε πρόσβαση σε δεδομένα από το έναυσμα σε ολόκληρη τη ροή εργασίας (για παράδειγμα το έναυσμα 'σε ένα νέο tweet' θα μεταβίβαση το tweet σε εκτέλεση του).

## <a name="actions"></a>Ενέργειες

Οι περισσότερες γραμμές σύνδεσης έχει μία ή πολλές ενέργειες που μπορεί να εκτελεστεί ως μέρος της ροής εργασίας.  Οι ενέργειες είναι όλα τα βήματα που πραγματοποιούνται μετά την εκτέλεση του έχει ενεργοποιηθεί από ένα έναυσμα.  Για να προσθέσετε μια ενέργεια, κάντε κλικ το κουμπί **Νέο βήμα** και αναζήτησης για τη γραμμή σύνδεσης που θέλετε να χρησιμοποιήσετε.  Μόλις επιλεγεί (και μετά τη ρύθμιση των παραμέτρων οι [συνδέσεις](#connections) που ενδεχομένως να απαιτούνται) θα δείτε την κάρτα ενεργειών που μπορείτε να ρυθμίσετε τις παραμέτρους.  Μπορείτε να επιλέξετε δεδομένα από τα προηγούμενα βήματα, κάνοντας κλικ σε οποιοδήποτε από τα διακριτικά για εξόδους, ή εισαγάγετε σε οποιαδήποτε ρύθμιση παραμέτρων, όπως απαιτείται.

![Ρύθμιση παραμέτρων μιας ενέργειας γραμμής σύνδεσης][2]

## <a name="connections"></a>Συνδέσεις

Οι περισσότερες γραμμές σύνδεσης απαιτεί να ρυθμίσετε μια *σύνδεση* να μπορέσετε να χρησιμοποιήσετε τη γραμμή σύνδεσης.  Μια *σύνδεση* είναι οποιαδήποτε διαμόρφωση σύνδεσης ή σύνδεσης που απαιτείται για να αποκτήσετε πρόσβαση στη γραμμή σύνδεσης.  Για γραμμές σύνδεσης που χρησιμοποιούν διακριτικό, να δημιουργήσετε μια σύνδεση σημαίνει είσοδο στην υπηρεσία (όπως το Office 365, Salesforce ή GitHub) όπου το διακριτικό πρόσβασης μπορεί να είναι με κρυπτογράφηση και με ασφάλεια είναι αποθηκευμένα σε ένα Azure μυστικού κατάστημα.  Άλλες γραμμές σύνδεσης (όπως FTP και SQL) απαιτούν μια σύνδεση που περιέχει ρύθμισης παραμέτρων όπως διεύθυνση διακομιστή, όνομα χρήστη και τον κωδικό πρόσβασης.  Αυτές οι λεπτομέρειες ρύθμισης παραμέτρων σύνδεσης επίσης κρυπτογραφούνται και αποθηκεύονται με ασφάλεια.  Συνδέσεις θα μπορούν να έχουν πρόσβαση στην υπηρεσία για όσο διάστημα επιτρέπει την υπηρεσία.  Για τις συνδέσεις του Azure Active Directory διακριτικό (όπως το Office 365 και Dynamics) μπορεί να συνεχίσουμε να ανανεώσετε το διακριτικό πρόσβασης απεριόριστο χρονικό διάστημα.  Άλλες υπηρεσίες ενδέχεται να θέσετε όρια σε χρονικό διάστημα μπορούμε να χρησιμοποιήσουμε ενός διακριτικού χωρίς αυτό γίνεται η ανανέωση.  Σε γενικές γραμμές συγκεκριμένες ενέργειες όπως η αλλαγή κωδικού πρόσβασης θα ακυρώσει όλων των διακριτικών πρόσβασης.  

Συνδέσεις είναι δυνατό να προβάλετε και να διαχειρίζεται στο Azure κάνοντας κλικ στην επιλογή **Αναζήτηση** και επιλέγοντας **API συνδέσεις**.  Από τον πόρο συνδέσεις API μπορείτε να δείτε, επεξεργασία, ενημέρωση, ή να εγκρίνετε ξανά τις συνδέσεις που έχετε δημιουργήσει.

## <a name="next-steps"></a>Επόμενα βήματα

- [Δημιουργήστε την πρώτη εφαρμογή λογικής](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Μάθετε κοινές χρήσεις και δείτε παραδείγματα των εφαρμογών που είναι λογική](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Γρήγορα αποτελέσματα με εναύσματα ενοποίησης για μεγάλες επιχειρήσεις και ενέργειες](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png