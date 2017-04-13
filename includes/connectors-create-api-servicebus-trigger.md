Παρακάτω θα δείτε πώς μπορείτε να χρησιμοποιήσετε το έναυσμα **Υπηρεσίας Bus - όταν λαμβάνετε ένα μήνυμα σε μια ουρά** για να ξεκινήσετε μια ροή εργασίας εφαρμογή λογικής κατά την αποστολή του νέου στοιχείου σε μια υπηρεσία Bus ουρά.  

>[AZURE.NOTE]Θα σας ζητηθεί να συνδεθείτε με την υπηρεσία Bus συμβολοσειρά σύνδεσης, εάν δεν έχετε ήδη δημιουργήσει μια σύνδεση σε υπηρεσία Bus.  

1. Στο πλαίσιο αναζήτησης στη σχεδίαση εφαρμογών λογικής, εισαγάγετε **bus υπηρεσίας**. Στη συνέχεια, επιλέξτε το έναυσμα **Υπηρεσίας Bus - όταν λαμβάνετε ένα μήνυμα σε μια ουρά** .  
![Εικόνα έναυσμα Bus υπηρεσίας 1](./media/connectors-create-api-servicebus/trigger-1.png)   
- Εμφανίζεται το παράθυρο διαλόγου **Όταν λαμβάνετε ένα μήνυμα σε μια ουρά** .  
![Υπηρεσία Bus έναυσμα εικόνα 2](./media/connectors-create-api-servicebus/trigger-2.png)   
- Πληκτρολογήστε το όνομα της υπηρεσίας Bus ουράς θέλετε το έναυσμα για την παρακολούθηση.   
![Εικόνα έναυσμα Bus υπηρεσίας 3](./media/connectors-create-api-servicebus/trigger-3.png)   

Σε αυτό το σημείο, την εφαρμογή λογικής σας έχει ρυθμιστεί με ένα έναυσμα. Κατά τη λήψη ενός νέου στοιχείου στην ουρά που επιλέξατε, το έναυσμα θα ξεκινήσει μια εκτέλεση των άλλων εναύσματα και ενέργειες της ροής εργασίας.    