Λόγω βρίσκεται σε εξέλιξη ανάπτυξη, την έκδοση Android SDK εγκατεστημένη στο Android Studio μπορεί να δεν ταιριάζουν με την έκδοση στον κώδικα. Στο Android SDK που αναφέρονται σε αυτό το πρόγραμμα εκμάθησης είναι η έκδοση 23, το αργότερο τη στιγμή της εγγραφής. Ο αριθμός έκδοσης ενδέχεται να αυξηθούν ενώ εμφανίζονται νέες εκδόσεις του SDK και, συνιστάται να χρησιμοποιείτε την πιο πρόσφατη διαθέσιμη έκδοση.

Δύο συμπτώματα της ασυμφωνία εκδόσεων είναι οι εξής:

1. Όταν δημιουργείτε ή εκ νέου δημιουργία του έργου, ενδέχεται να λάβετε μηνύματα σφάλματος Gradle όπως "**απέτυχε η εύρεση προορισμού Google Inc.:Google APIs:n**".

2. Τυπική Android αντικείμενα στον κώδικα που πρέπει να επιλύσετε με βάση `import` δηλώσεις μπορεί να δημιουργό μηνύματα σφάλματος.

Εάν εμφανίζεται ένα από τα παραπάνω, την έκδοση του Android SDK εγκατεστημένο στο Android Studio μπορεί να συμφωνεί με τον προορισμό SDK του έργου που έχετε λάβει.  Για να επαληθεύσετε την έκδοση, κάντε τις ακόλουθες αλλαγές:


1. Στο Android Studio, κάντε κλικ στην επιλογή **Εργαλεία** => **Android** => **SDK Manager**. Εάν δεν έχετε εγκαταστήσει την πιο πρόσφατη έκδοση της πλατφόρμας SDK, στη συνέχεια, κάντε κλικ για να την εγκαταστήσετε. Σημειώστε τον αριθμό έκδοσης.

2. Στην καρτέλα "Εξερεύνηση έργου", στην περιοχή **Gradle δέσμες ενεργειών**, ανοίξτε το αρχείο **build.gradle (modeule: εφαρμογή)**. Βεβαιωθείτε ότι η **compileSdkVersion** και **buildToolsVersion** έχουν οριστεί στην πιο πρόσφατη έκδοση SDK εγκατεστημένο. Οι ετικέτες μπορεί να μοιάζει κάπως έτσι:
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. Στην Εξερεύνηση έργου του Android Studio κάντε δεξί κλικ στον κόμβο του έργου, επιλέξτε **Ιδιότητες**και στην αριστερή στήλη, επιλέξτε **Android**. Βεβαιωθείτε ότι έχει οριστεί **Προορισμού Δημιουργία έργου** για την ίδια έκδοση SDK με το **targetSdkVersion**.

4. Στο Android Studio, αρχείο δήλωσης δεν χρησιμοποιείται πλέον για να καθορίσετε το προορισμού SDK και ελάχιστη έκδοση SDK, σε αντίθεση με την περίπτωση με έκλειψη.
