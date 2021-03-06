<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με κονσόλα διαχείρισης Mimecast | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Κονσόλα διαχείρισης Mimecast με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με κονσόλα διαχείρισης Mimecast
  
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και Mimecast Κονσόλα διαχείρισης.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Μια κονσόλα διαχείρισης Mimecast καθολικής σύνδεσης με δυνατότητα συνδρομή
  
Μετά την ολοκλήρωση αυτού του προγράμματος εκμάθησης, θα μπορούν να καθολικής σύνδεσης σε μια εφαρμογή στην τοποθεσία της εταιρείας σας κονσόλα διαχείρισης Mimecast (υπηρεσία παροχής που ξεκινούν από σύμβολο στην) ή με την [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md)Azure AD χρηστών που έχουν εκχωρηθεί σε Mimecast Κονσόλα διαχείρισης.
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα εξής δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για Κονσόλα διαχείρισης Mimecast
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Σενάριο")
##<a name="enabling-the-application-integration-for-mimecast-admin-console"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για Κονσόλα διαχείρισης Mimecast
  
Είναι ο στόχος αυτής της ενότητας για να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για Mimecast Κονσόλα διαχείρισης.

###<a name="to-enable-the-application-integration-for-mimecast-admin-console-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για Κονσόλα διαχείρισης Mimecast, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **Mimecast Κονσόλα διαχείρισης**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Συλλογή εφαρμογών")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **Κονσόλα διαχείρισης Mimecast**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Κονσόλα διαχείρισης Mimecast] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Κονσόλα διαχείρισης Mimecast")
##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα Κονσόλα διαχείρισης Mimecast με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.  
Ως μέρος αυτής της διαδικασίας, που απαιτούνται για να δημιουργήσετε ένα αρχείο βάσης 64 κωδικοποιημένο πιστοποιητικό.  
Εάν δεν είστε εξοικειωμένοι με αυτήν τη διαδικασία, δείτε [πώς μπορείτε να μετατρέψετε ένα πιστοποιητικό δυαδικών δεδομένων σε ένα αρχείο κειμένου](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Mimecast Κονσόλα διαχείρισης** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Κονσόλα διαχείρισης Mimecast** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , στο πλαίσιο κειμένου **Mimecast διαχείρισης κονσόλας εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από τους χρήστες σας για να συνδεθείτε εφαρμογή σας κονσόλα διαχείρισης Mimecast (π.χ.: "https://webmail-uk.mimecast.com" ή "https://webmail-us.mimecast.com"), και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    >[AZURE.NOTE] Είσοδος στη διεύθυνση URL είναι συγκεκριμένη περιοχή.

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στην κονσόλα διαχείρισης Mimecast** , για να κάνετε λήψη του πιστοποιητικού, κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο πιστοποιητικού τοπικά στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Σε ένα παράθυρο προγράμματος περιήγησης web διαφορετικά, συνδεθείτε με την Κονσόλα διαχείρισης Mimecast ως διαχειριστής.

6.  Μεταβείτε στις επιλογές **υπηρεσίες \> εφαρμογής**.

    ![Υπηρεσίες] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Υπηρεσίες")

7.  Κάντε κλικ στην επιλογή **Έλεγχος ταυτότητας προφίλ**.

    ![Έλεγχος ταυτότητας προφίλ] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Έλεγχος ταυτότητας προφίλ")

8.  Κάντε κλικ στην επιλογή **νέο προφίλ του ελέγχου ταυτότητας**.

    ![Νέο προφίλ του ελέγχου ταυτότητας] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "Νέο προφίλ του ελέγχου ταυτότητας")

9.  Στην ενότητα **Έλεγχος ταυτότητας προφίλ** , ακολουθήστε τα παρακάτω βήματα:

    ![Έλεγχος ταυτότητας προφίλ] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Έλεγχος ταυτότητας προφίλ")

    1.  Στο πλαίσιο κειμένου **Περιγραφή** , πληκτρολογήστε ένα όνομα για τη ρύθμιση παραμέτρων.
    2.  Επιλέξτε **έλεγχο ταυτότητας SAML για Κονσόλα διαχείρισης Mimecast**.
    3.  Ως **υπηρεσία παροχής που χρησιμοποιείτε**, επιλέξτε **Azure Active Directory**.
    4.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στην κονσόλα διαχείρισης Mimecast** , αντιγράψτε την τιμή **Διεύθυνσης URL εκδότη** και στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **Διεύθυνση URL εκδότη** .
    5.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στην κονσόλα διαχείρισης Mimecast** , αντιγράψτε την τιμή **Διεύθυνση URL απομακρυσμένης σύνδεσης** και, στη συνέχεια, επικολλήστε τη **Διεύθυνση URL σύνδεσης** πλαισίου κειμένου.
    6.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στην κονσόλα διαχείρισης Mimecast** , αντιγράψτε την τιμή **Διεύθυνση URL απομακρυσμένης σύνδεσης** και κατόπιν επικολλήστε το στο πλαίσιο κειμένου **Διεύθυνση URL αποσυνδεθείτε** .  

        >[AZURE.NOTE]Η διεύθυνση URL σύνδεσης και της τιμής αποσύνδεσης διεύθυνση URL είναι για την Κονσόλα διαχείρισης Mimecast τα ίδια.

    7.  Δημιουργία αρχείου **βάσης 64 κωδικοποιημένο** από το πιστοποιητικό που έχετε λάβει.  

        >[AZURE.TIP]Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [πώς μπορείτε να μετατρέψετε ένα πιστοποιητικό δυαδικών δεδομένων σε ένα αρχείο κειμένου](http://youtu.be/PlgrzUZ-Y1o).

    8.  Άνοιγμα βάσης 64 κωδικοποιημένο πιστοποιητικού στο Σημειωματάριο, καταργήστε την πρώτη γραμμή ("*--*") και την τελευταία γραμμή ("*--*"), αντιγράψτε το υπόλοιπο περιεχόμενο του στο Πρόχειρο και στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **Πιστοποιητικό υπηρεσία παροχής ταυτότητας (μετα-δεδομένα)** .
    9.  Επιλέξτε **να επιτρέπεται η καθολική σύνδεση στο**.
    10. Κάντε κλικ στην επιλογή **Αποθήκευση**.

10. Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")
##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη
  
Για να επιτρέψετε στους χρήστες Azure AD για να συνδεθείτε στην κονσόλα διαχείρισης Mimecast, πρέπει να αποδοθεί στην κονσόλα διαχείρισης Mimecast.  
Στην περίπτωση Mimecast Κονσόλα διαχείρισης, προμήθεια είναι μια μη αυτόματη εργασία.
  
Πρέπει να καταχωρήσετε έναν τομέα, για να δημιουργήσετε χρήστες.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Για να ρυθμίσετε την προμήθεια του χρήστη, εκτελέστε τα ακόλουθα βήματα:

1.  Πραγματοποιήστε είσοδο σας **Κονσόλα διαχείρισης Mimecast** ως διαχειριστής.

2.  Μεταβείτε στις επιλογές **σε καταλόγους \> εσωτερικό**.

    ![Σε καταλόγους] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Σε καταλόγους")

3.  Κάντε κλικ στην **καταχώρηση του νέου τομέα**.

    ![Καταχώρηση του τομέα] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Καταχώρηση του τομέα")

4.  Αφού δημιουργηθεί το νέο τομέα, κάντε κλικ στην επιλογή **Νέα διεύθυνση**.

    ![Νέα διεύθυνση] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "Νέα διεύθυνση")

5.  Στο παράθυρο διαλόγου νέα διεύθυνση, ακολουθήστε τα παρακάτω βήματα:

    ![Αποθήκευση] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Αποθήκευση")

    1.  Πληκτρολογήστε τη **Διεύθυνση ηλεκτρονικού ταχυδρομείου**, **Καθολικό όνομα**, **τον κωδικό πρόσβασης** και **Επιβεβαίωση κωδικού πρόσβασης** χαρακτηριστικά έναν έγκυρο λογαριασμό AAD που θέλετε για την παροχή των σε τα σχετικά πλαίσια κειμένου.
    2.  Κάντε κλικ στην επιλογή **Αποθήκευση**.

>[AZURE.NOTE]Μπορείτε να χρησιμοποιήσετε οποιαδήποτε άλλα εργαλεία δημιουργίας λογαριασμού χρήστη Κονσόλα διαχείρισης Mimecast ή API που παρέχεται από την Κονσόλα διαχείρισης Mimecast σε λογαριασμούς χρηστών AAD διάταξη.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών
  
Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-mimecast-admin-console-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες Κονσόλα διαχείρισης Mimecast, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική Azure πύλη, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη σελίδα ενοποίησης εφαρμογής **Mimecast Κονσόλα διαχείρισης **, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Ναι")
  
Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).