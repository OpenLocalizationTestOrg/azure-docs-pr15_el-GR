<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Panopto | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Panopto με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panopto"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Panopto
  
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και Panopto.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Ένας μισθωτής Panopto
  
Μετά την ολοκλήρωση αυτού του προγράμματος εκμάθησης, θα μπορείτε να καθολικής σύνδεσης σε μια εφαρμογή στην τοποθεσία της εταιρείας σας Panopto (υπηρεσία παροχής που ξεκινούν από σύμβολο στην) ή με την [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md)Azure AD χρηστών που έχουν εκχωρηθεί σε Panopto.
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα ακόλουθα δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για Panopto
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-panopto-tutorial/IC777665.png "Σενάριο")
##<a name="enabling-the-application-integration-for-panopto"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για Panopto
  
Είναι ο στόχος αυτής της ενότητας για να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για το Panopto.

###<a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για Panopto, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-panopto-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-panopto-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-panopto-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-panopto-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **Panopto**.

    ![Συλλογή Appkication] (./media/active-directory-saas-panopto-tutorial/IC777666.png "Συλλογή Appkication")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **Panopto**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Panopto] (./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα Panopto με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.  
Ως μέρος αυτής της διαδικασίας, που απαιτούνται για να δημιουργήσετε ένα αρχείο βάσης 64 κωδικοποιημένο πιστοποιητικό.  
Εάν δεν είστε εξοικειωμένοι με αυτήν τη διαδικασία, δείτε [πώς μπορείτε να μετατρέψετε ένα πιστοποιητικό δυαδικών δεδομένων σε ένα αρχείο κειμένου](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Panopto** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-panopto-tutorial/IC777667.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Panopto** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-panopto-tutorial/IC777668.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , στο πλαίσιο κειμένου **Panopto εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL σας χρησιμοποιώντας το παρακάτω μοτίβο "*https://\<μισθωτή όνομα\>. Panopto.com*", και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-panopto-tutorial/IC777528.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Panopto** , για να κάνετε λήψη του πιστοποιητικού, κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο πιστοποιητικού στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-panopto-tutorial/IC777669.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Σε ένα παράθυρο προγράμματος περιήγησης web διαφορετικά, συνδεθείτε στην τοποθεσία της εταιρείας σας Panopto ως διαχειριστής.

6.  Στη γραμμή εργαλείων στα αριστερά, κάντε κλικ στην επιλογή **σύστημα**και, στη συνέχεια, κάντε κλικ στην επιλογή **Υπηρεσίες παροχής ταυτότητας**.

    ![Σύστημα] (./media/active-directory-saas-panopto-tutorial/IC777670.png "Σύστημα")

7.  Κάντε κλικ στην επιλογή **Προσθήκη υπηρεσίας παροχής**.

    ![Υπηρεσίες παροχής ταυτότητας] (./media/active-directory-saas-panopto-tutorial/IC777671.png "Υπηρεσίες παροχής ταυτότητας")

8.  Στην ενότητα SAML υπηρεσία παροχής που χρησιμοποιείτε, ακολουθήστε τα παρακάτω βήματα:

    ![Ρύθμιση παραμέτρων ΑΠΑ] (./media/active-directory-saas-panopto-tutorial/IC777672.png "Ρύθμιση παραμέτρων ΑΠΑ")

    1.  Από τη λίστα **Τύπος υπηρεσίας παροχής** , επιλέξτε **SAML20**
    2.  Στο πλαίσιο κειμένου **Όνομα της παρουσίας** , πληκτρολογήστε ένα όνομα για την παρουσία.
    3.  Στο πλαίσιο κειμένου **Φιλική περιγραφή** , πληκτρολογήστε μια φιλική περιγραφή.
    4.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Panopto** , αντιγράψτε την τιμή **Διεύθυνσης URL εκδότη** και στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **εκδότη** .
    5.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Panopto** , αντιγράψτε την τιμή **Διεύθυνσης URL SSO SAML** και, στη συνέχεια, επικολλήστε το στο πλαίσιο κειμένου **Αναπήδηση διεύθυνση Url της σελίδας** .
    6.  Δημιουργία αρχείου **βάσης 64 κωδικοποιημένο** από το πιστοποιητικό που έχετε λάβει.  

        >[AZURE.TIP] Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [πώς μπορείτε να μετατρέψετε ένα πιστοποιητικό δυαδικών δεδομένων σε ένα αρχείο κειμένου](http://youtu.be/PlgrzUZ-Y1o)

    7.  Ανοίξτε το βάσης 64 κωδικοποιημένο πιστοποιητικό στο Σημειωματάριο, αντιγράψτε το περιεχόμενο της στο Πρόχειρο και στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **Δημόσιο κλειδί**
    8.  Κάντε κλικ στην επιλογή **Αποθήκευση**.
        ![Αποθήκευση] (./media/active-directory-saas-panopto-tutorial/IC777673.png "Αποθήκευση")

9.  Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-panopto-tutorial/IC777674.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")
##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη
  
Δεν υπάρχει κανένα στοιχείο ενέργεια για να ρυθμίσετε τις παραμέτρους χρήστη προμήθεια να Panopto.  
Όταν ένα χρήστη που έχει αντιστοιχιστεί προσπαθεί να συνδεθείτε στο Panopto χρησιμοποιώντας τον πίνακα της access, Panopto ελέγχει εάν υπάρχει ο χρήστης.  
Εάν υπάρχει δεν υπάρχει λογαριασμός χρήστη διαθέσιμη ακόμα, δημιουργείται αυτόματα από Panopto.

>[AZURE.NOTE]Μπορείτε να χρησιμοποιήσετε οποιοδήποτε άλλο Panopto χρήστη λογαριασμού εργαλεία δημιουργίας ή APIs που παρέχεται από Panopto να παρέχετε Azure AD λογαριασμών χρηστών.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών
  
Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες να Panopto, εκτελέστε τα ακόλουθα βήματα:

1.  Στην κλασική Azure πύλη, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη σελίδα **Panopto **ενοποίησης της εφαρμογής, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-panopto-tutorial/IC777675.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-panopto-tutorial/IC767830.png "Ναι")
  
Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).