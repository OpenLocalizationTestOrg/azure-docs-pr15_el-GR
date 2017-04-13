Ακολουθούν οι περιορισμοί χρήσης και άλλοι περιορισμοί υπηρεσίας για την υπηρεσία καταλόγου Active Directory του Azure.

| Κατηγορία | Όρια |
|---|---|
| Σε καταλόγους | Ένα μεμονωμένο χρήστη μόνο μπορούν να συσχετιστούν με έως 20 σε καταλόγους Azure Active Directory.<br />Παραδείγματα των πιθανών συνδυασμών: <ul> <li>Ένα μεμονωμένο χρήστη να δημιουργεί 20 καταλόγων.</li><li>Ένα μεμονωμένο χρήστη να προστίθεται σε καταλόγους 20 ως μέλος.</li><li>Ένα μεμονωμένο χρήστη να δημιουργεί 10 καταλόγους και αργότερα προστίθεται από άλλους 10 διαφορετικούς καταλόγους.</li></ul> |  
| Αντικείμενα | <ul><li>Έως 500.000 αντικείμενα μπορεί να χρησιμοποιηθεί σε έναν κατάλογο μόνο από τους χρήστες της δωρεάν έκδοσης του Azure Active Directory.</li><li>Ένας χρήστης δεν είναι διαχειριστές να δημιουργήσετε λιγότερα από 250 αντικείμενα.</li></ul> |
| Επεκτάσεις σχήματος | <ul><li>Επεκτάσεις τύπου συμβολοσειράς μπορεί να έχει έως 256 χαρακτήρες. </li><li>Δυαδικό τύπο επεκτάσεις περιορίζονται σε 256 byte.</li><li>100 τιμές επέκταση (σε ΌΛΟΥΣ τους τύπους και ΌΛΕΣ τις εφαρμογές) μπορούν να εγγραφούν σε οποιοδήποτε αντικείμενο.</li><li>Μόνο "χρήστης", "Ομάδα", "TenantDetail", "Διάταξη", "Εφαρμογή" και "ServicePrincipal" οντοτήτων μπορεί να επεκταθεί με τύπο "Συμβολοσειράς" ή "Δυαδικό" Τύπος μίας τιμής χαρακτηριστικά.</li><li>Επεκτάσεις σχήματος είναι διαθέσιμες μόνο στην έκδοση API Graph 1.21-προεπισκόπηση. Η εφαρμογή πρέπει να σας εκχωρηθεί πρόσβαση εγγραφής για να καταχωρήσετε μια επέκταση.</li></ul> |
| Εφαρμογές | Έως 10 χρήστες μπορούν να είναι κάτοχοι μια μεμονωμένη εφαρμογή. |
| Ομάδες | <ul><li>Έως 10 χρήστες μπορεί να είναι κάτοχοι μιας ομάδας μία.</li><li>Οποιονδήποτε αριθμό αντικειμένων μπορεί να είναι μέλη από μια ομάδα στην υπηρεσία καταλόγου Azure Active Directory.</li><li>Ο αριθμός των μελών σε μια ομάδα, μπορείτε να πραγματοποιήσετε συγχρονισμό από το καταλόγου Active Directory εσωτερικής εγκατάστασης Azure Active Directory περιορίζεται στα μέλη 15K, χρήσης του συγχρονισμού Active Directory καταλόγου Azure (DirSync).</li><li>Ο αριθμός των μελών σε μια ομάδα, μπορείτε να πραγματοποιήσετε συγχρονισμό από το καταλόγου Active Directory εσωτερικής εγκατάστασης Azure Active Directory με χρήση Azure AD Connect περιορίζεται στα μέλη 50K.</li></ul> |
| Πίνακας της Access | <ul><li>Υπάρχει όριο στον αριθμό των εφαρμογών που μπορούν να προβληθούν στο πλαίσιο πρόσβασης ανά τελικού χρήστη, για τους χρήστες που έχουν εκχωρηθεί άδειες χρήσης για το Azure AD Premium ή την οικογένεια φορητότητας για μεγάλες επιχειρήσεις.</li><li>Έως 10 πλακίδια εφαρμογών (παραδείγματα: πλαίσιο, Salesforce ή το Dropbox) μπορούν να προβληθούν στο πλαίσιο πρόσβασης για κάθε τελικού χρήστη για τους χρήστες που έχουν εκχωρηθεί άδειες χρήσης δωρεάν ή Azure AD βασικές εκδόσεις του Azure Active Directory. Αυτό το όριο δεν ισχύει για τους λογαριασμούς διαχειριστή.</li></ul> |
| Αναφορές | Έως 1.000 γραμμών μπορεί να προβάλει ή να ληφθεί σε όλες τις αναφορές. Οποιαδήποτε πρόσθετα δεδομένα δεκαδικά ψηφία περικόπτονται. |