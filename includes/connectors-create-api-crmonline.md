#### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
- Azure λογαριασμού; Μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/free)
- Ένα λογαριασμό [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) 

Πριν να χρησιμοποιήσετε το λογαριασμό σας Dynamics σε μια εφαρμογή λογικής, εγκρίνετε την εφαρμογή λογικής για να συνδεθείτε στο λογαριασμό σας CRM Online. Μπορείτε να το κάνετε εύκολα μέσα στην πύλη του Azure την εφαρμογή σας λογική. 

Επιτρέπουν την εφαρμογή της λογικής για να συνδεθείτε στο λογαριασμό σας CRM Online με τα παρακάτω βήματα:

1. Δημιουργία εφαρμογής λογικής. Στο εργαλείο σχεδίασης λογικής εφαρμογών, επιλέξτε **Εμφάνιση Microsoft διαχειριζόμενων APIs** από την αναπτυσσόμενη λίστα και, στη συνέχεια, πληκτρολογήστε "dynamics" στο πλαίσιο αναζήτησης. Επιλέξτε μία από τις εναύσματα ή τις ενέργειες:  
  ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Εάν δεν έχετε δημιουργήσει προηγουμένως τις συνδέσεις στο Dynamics, θα σας ζητηθεί να συνδεθείτε χρησιμοποιώντας τα διαπιστευτήριά Dynamics:  
  ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Επιλέξτε **Είσοδος**και πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης. Επιλέξτε **Είσοδος**. 

    Αυτά τα διαπιστευτήρια που χρησιμοποιούνται για να εξουσιοδοτήσετε την εφαρμογή σας λογικής για να συνδεθείτε και να αποκτήσετε πρόσβαση στα δεδομένα στο λογαριασμό σας στο Dynamics. 
4. Παρατηρήστε ότι έχει δημιουργηθεί η σύνδεση. Τώρα, συνεχίστε με τα άλλα βήματα στην εφαρμογή λογικής:  
  ![](./media/connectors-create-api-crmonline/dynamics-properties.png)
