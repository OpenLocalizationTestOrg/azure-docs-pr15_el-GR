
1. Στον υπολογιστή εσωτερικής εγκατάστασης, συνδεθείτε στην [Πύλη διαχείρισης του Azure](http://manager.windowsazure.com) (αυτή είναι η παλιά πύλη).

2. Στο κάτω μέρος του παραθύρου περιήγησης, επιλέξτε **+ ΝΈΟ** > **Εφαρμογή υπηρεσιών** > **Υπηρεσία BizTalk** > **Δημιουργία προσαρμοσμένης**.

3. Δώστε ένα **Όνομα υπηρεσίας BizTalk** και επιλέξτε μια **Edition**. 

    Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί **mobile1**. Θα πρέπει να πληκτρολογήσετε ένα μοναδικό όνομα για το νέο υπηρεσία BizTalk.

4. Μόλις δημιουργηθεί η υπηρεσία BizTalk, επιλέξτε την καρτέλα **Υβριδική συνδέσεις** και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

    ![Προσθήκη σύνδεσης υβριδική](./media/hybrid-connections-create-new/3.png)

    Αυτό δημιουργεί μια νέα σύνδεση υβριδική.

5. Δώστε ένα **όνομα** και **Όνομα κεντρικού υπολογιστή** για τη σύνδεση της υβριδικής και ρύθμιση **θύρας** για `1433`. 
  
    ![Ρύθμιση παραμέτρων υβριδικού σύνδεσης](./media/hybrid-connections-create-new/4.png)

    Το όνομα κεντρικού υπολογιστή είναι το όνομα του διακομιστή εσωτερικής εγκατάστασης. Αυτό ρυθμίζει τις παραμέτρους υβριδική σύνδεσης για να αποκτήσετε πρόσβαση σε SQL Server που εκτελείται στη θύρα 1433. Εάν χρησιμοποιείτε μια καθορισμένη παρουσία του SQL Server, χρησιμοποιήστε αντί για αυτό το στατική θύρα που ορίσατε προηγουμένως.

6. Αφού δημιουργήσετε τη νέα σύνδεση, την κατάσταση του της νέας σύνδεσης εμφανίζει **εσωτερικής εγκατάστασης δεν ολοκληρώθηκε**.

7. Επιστρέψετε σε μια κινητή υπηρεσία σας, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**, κάντε κύλιση προς τα κάτω σε **υβριδική συνδέσεις** και κάντε κλικ στην επιλογή **Προσθήκη σύνδεσης υβριδική**, στη συνέχεια, επιλέξτε τη σύνδεση υβριδική που μόλις δημιουργήσατε και επιλέξτε **OK**.

    Αυτή η δυνατότητα επιτρέπει την υπηρεσία κινητών τηλεφώνων για να χρησιμοποιήσετε τη νέα σύνδεση υβριδική.

Στη συνέχεια, θα πρέπει να εγκαταστήσετε το αρχείο διαχείρισης σύνδεσης υβριδική στον υπολογιστή εσωτερικής εγκατάστασης.