#<a name="how-to-create-a-custom-virtual-machine"></a>Πώς μπορείτε να δημιουργήσετε μια προσαρμοσμένη εικονική μηχανή

Μια *προσαρμοσμένη* εικονική μηχανή αναφέρεται σε μια εικονική μηχανή που δημιουργείτε χρησιμοποιώντας τη μέθοδο **Από τη συλλογή** , επειδή το σάς επιτρέπει να ρυθμίσετε τις παραμέτρους περισσότερες επιλογές από τη **Γρήγορη δημιουργία** μέθοδο. Περιλαμβάνουν τις εξής επιλογές:

- Περισσότερες επιλογές για την εικόνα που θα χρησιμοποιήσετε για να δημιουργήσετε την εικονική μηχανή (Εικονική)
- Σύνδεση την εικονική Μηχανή με ένα εικονικό δίκτυο
- Προσθήκη η Εικονική σε μια υπάρχουσα υπηρεσία cloud
- Προσθήκη η Εικονική σε ένα σύνολο διαθεσιμότητα

> [AZURE.IMPORTANT] Εάν θέλετε την εικονική μηχανή σας για να χρησιμοποιήσετε ένα εικονικό δίκτυο, ώστε να μπορείτε να συνδεθείτε σε αυτό απευθείας από όνομα κεντρικού υπολογιστή ή ρύθμιση του συνδέσεις ανεξαρτήτως εσωτερικής εγκατάστασης, βεβαιωθείτε ότι μπορείτε να καθορίσετε το εικονικό δίκτυο όταν δημιουργείτε την εικονική μηχανή. Μια εικονική μηχανή μπορεί να ρυθμιστεί για να συμμετάσχετε σε ένα εικονικό δίκτυο μόνο όταν δημιουργείτε την εικονική μηχανή. Για περισσότερες πληροφορίες σχετικά με τα δίκτυα εικονικού, ανατρέξτε στο θέμα [Επισκόπηση εικονικού δικτύου Azure](http://go.microsoft.com/fwlink/p/?LinkID=294063).

1. Είσοδος στην [πύλη του Azure](http://manage.windowsazure.com).

2. Στη γραμμή εντολών, κάντε κλικ στην επιλογή **Δημιουργία**.

3. Κάντε κλικ στην επιλογή **τον υπολογισμό**, κάντε κλικ στην επιλογή **εικονική μηχανή**και, στη συνέχεια, κάντε κλικ στην επιλογή **Από τη συλλογή**.

4. Επιλέξτε την εικόνα που θέλετε να χρησιμοποιήσετε και, στη συνέχεια, κάντε κλικ στο βέλος για να συνεχίσετε.

5. Εάν πολλές εκδόσεις του η εικόνα είναι διαθέσιμες, ημερομηνία **κυκλοφορίας έκδοση**, επιλέξτε την έκδοση που θέλετε να χρησιμοποιήσετε.

6. **Εικονική μηχανή όνομα**, πληκτρολογήστε το όνομα που θέλετε να χρησιμοποιήσετε για την εικονική μηχανή.

7. Χρήση **επιπέδων** και το **μέγεθος** για να επιλέξετε το κατάλληλο μέγεθος για την εικονική μηχανή. Το μέγεθος που επιλέγετε επηρεάζει το μέγιστο ρύθμιση παραμέτρων για την εικονική μηχανή, καθώς και την τιμολόγηση. Για λεπτομέρειες ρύθμισης παραμέτρων, ανατρέξτε στο θέμα [εικονική μηχανή και τα μεγέθη υπηρεσία Cloud για Azure](http://go.microsoft.com/fwlink/p/?LinkID=389844).

8. Στο **Νέο όνομα χρήστη**, πληκτρολογήστε ένα όνομα για το λογαριασμό διαχείρισης που θέλετε να χρησιμοποιήσετε για να διαχειριστείτε το διακομιστή.

9. Στο **Νέο κωδικό πρόσβασης**, πληκτρολογήστε έναν ισχυρό κωδικό πρόσβασης για το λογαριασμό διαχειριστή. Στο πλαίσιο **Επιβεβαίωση κωδικού πρόσβασης**, πληκτρολογήστε ξανά τον ίδιο κωδικό πρόσβασης.

10. Κάντε κλικ στο βέλος για να συνεχίσετε.

11. Στην **Υπηρεσία Cloud**, κάντε ένα από τα εξής:

    - Εάν αυτή είναι η εικονική μηχανή πρώτο ή μόνο στην υπηρεσία cloud, επιλέξτε **Δημιουργία μια νέα υπηρεσία στο Cloud**. Στη συνέχεια, στο πλαίσιο **Όνομα DNS υπηρεσία Cloud**, πληκτρολογήστε ένα όνομα που χρησιμοποιεί μεταξύ 3 και 24 πεζά γράμματα και αριθμούς. Αυτό το όνομα γίνεται μέρος του URI που χρησιμοποιείται για να επικοινωνήσετε με την εικονική μηχανή μέσω της υπηρεσίας cloud.
    - Εάν αυτή η εικονική μηχανή που προστίθεται σε μια υπηρεσία cloud, επιλέξτε την από τη λίστα.

    > [AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με την τοποθέτηση εικονικές μηχανές στην ίδια υπηρεσία cloud, ανατρέξτε στο θέμα [Πώς να συνδεθείτε εικονικές μηχανές σε μια υπηρεσία στο cloud](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/).

12. Στην **Περιοχή/συνάφειας/εικονική ομάδα δικτύου**, επιλέξτε περιοχή, ομάδα συσχέτισης ή εικονικού δικτύου που θέλετε να χρησιμοποιήσετε για την εικονική μηχανή. Για περισσότερες πληροφορίες σχετικά με τις ομάδες συσχέτισης, ανατρέξτε στο θέμα [σχετικά με τις ομάδες συσχέτισης για εικονικού δικτύου](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

13. Στο **Χώρο αποθήκευσης λογαριασμού**, επιλέξτε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης για το αρχείο VHD ή χρησιμοποιήστε ένα λογαριασμό που δημιουργούνται αυτόματα χώρου αποθήκευσης. Μόνο ένα λογαριασμό χώρου αποθήκευσης ανά περιοχή δημιουργείται αυτόματα. Όλα άλλες εικονικές μηχανές που δημιουργείτε με αυτήν τη ρύθμιση βρίσκονται σε αυτόν το λογαριασμό χώρου αποθήκευσης. Περιορίζονται σε 20 λογαριασμούς χώρου αποθήκευσης.

14. Εάν θέλετε η εικονική μηχανή στην οποία θα ανήκει σε ένα σύνολο διαθεσιμότητα, σε **Ορισμός διαθεσιμότητας**, επιλέξτε **Ορισμός διαθεσιμότητας δημιουργία** ή προσθέσετε σε ένα υπάρχον σύνολο διαθεσιμότητα.

    **Σημείωση**: εικονικές μηχανές σε ένα σύνολο διαθεσιμότητα έχουν αναπτυχθεί σε διαφορετική σφαλμάτων τομείς. Διάθεση πολλών εικονικές μηχανές στο διαθεσιμότητα σύνολο βοηθά να βεβαιωθείτε ότι η εφαρμογή σας είναι διαθέσιμο κατά τη διάρκεια αποτυχίες δικτύου, αποτυχίες υλικού στον τοπικό δίσκο και τις προγραμματισμένες χρόνου εκτός λειτουργίας.

15.  Στην περιοχή **τα τελικά σημεία**, αναθεωρήστε τα νέα τελικά σημεία που θα δημιουργηθεί, για να επιτρέπουν συνδέσεις προς την εικονική μηχανή, όπως μέσω απομακρυσμένης επιφάνειας εργασίας ή ένα πρόγραμμα-πελάτη ασφαλούς κελύφους (SSH). Μπορείτε επίσης να προσθέσετε τα τελικά σημεία τώρα, ή δημιουργήσετε αργότερα. Για οδηγίες σχετικά με τη δημιουργία αργότερα, δείτε [πώς μπορείτε να ορίσετε επάνω τελικά σημεία σε μια εικονική μηχανή](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

16.  Στην περιοχή **Εικονική παράγοντας**, αποφασίστε εάν θέλετε να εγκαταστήσετε τον παράγοντα Εικονική. Αυτός ο παράγοντας παρέχει το περιβάλλον για να εγκαταστήσετε επεκτάσεις που μπορούν να σας βοηθήσουν να αλληλεπιδράσετε με την εικονική μηχανή. Για λεπτομέρειες, ανατρέξτε στο θέμα [Διαχείριση επεκτάσεις](http://go.microsoft.com/FWLink/p/?LinkID=390493).

17. Κάντε κλικ στο βέλος για να δημιουργήσετε την εικονική μηχανή.

    ![Επιτυχής Δημιουργία προσαρμοσμένου εικονική μηχανή](./media/howto-custom-create-vm/VMSuccessWindows.png)

##<a name="next-steps"></a>Επόμενα βήματα##
Αφού δημιουργηθεί η εικονική μηχανή, έχει ξεκινήσει αυτόματα. Κατά την πύλη Εμφανίζει την κατάσταση ως εκτελείται, μπορείτε να συνδεθείτε η εικονική μηχανή. Για οδηγίες, ανατρέξτε σε ένα από τα ακόλουθα άρθρα:

- [Πώς μπορείτε να συνδεθείτε στο μια εικονική μηχανή εκτελείται Linux](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)
- [Πώς μπορείτε να συνδεθείτε στο μια εικονική μηχανή εκτελούν τον Windows Server](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md)

