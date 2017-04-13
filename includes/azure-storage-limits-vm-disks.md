Μια εικονική μηχανή Azure υποστηρίζει την επισύναψη ενός αριθμού δίσκων δεδομένων. Για βέλτιστη απόδοση, θα θέλετε να περιορίσετε τον αριθμό των ιδιαίτερα εκμεταλλευθεί δίσκων που έχουν επισυναφθεί σε η εικονική μηχανή για να αποφύγετε πιθανές περιορισμού. Εάν όλων των δίσκων δεν είναι ιδιαίτερα εφαρμόζονται την ίδια στιγμή, το λογαριασμό χώρου αποθήκευσης μπορούν να υποστηρίξουν ένα μεγαλύτερο αριθμό δίσκων.

- **Για λογαριασμούς τυπική χώρου αποθήκευσης:** Μια τυπική χώρου αποθήκευσης λογαριασμός έχει μέγιστο αίτημα συνολικό ποσοστό 20.000 IOP Προέλευσης. Το συνολικό IOP Προέλευσης κατά μήκος όλων των δίσκων σας εικονική μηχανή σε λογαριασμό τυπική χώρου αποθήκευσης δεν πρέπει να υπερβαίνει αυτό το όριο.

    Μπορείτε να υπολογίσετε υπερβαίνει τον αριθμό των ιδιαίτερα εκμεταλλευθεί δίσκων που υποστηρίζονται από ένα λογαριασμό χώρου αποθήκευσης ενός προτύπου με βάση το όριο επιτόκιο αίτηση. Για παράδειγμα, για ένα βασικό Εικονική επίπεδο, ο μέγιστος αριθμός των ιδιαίτερα εκμεταλλευθεί δίσκων είναι σχετικά με 66 (20.000/300 IOP Προέλευσης ανά δίσκο), καθώς και για μια τυπική Εικονική επίπεδο, είναι περίπου 40 (20.000/500 IOP Προέλευσης ανά δίσκο), όπως φαίνεται στον παρακάτω πίνακα. 
 
- **Για λογαριασμούς του χώρου αποθήκευσης premium:** Ένα άρτιο αποθήκευσης έχει μέγιστη ταχύτητα μετάδοσης συνολικό ποσοστό 50 Gbps. Ο συνολικός αριθμός μεταξύ όλων των δίσκων σας Εικονική δεν πρέπει να υπερβαίνει αυτό το όριο.