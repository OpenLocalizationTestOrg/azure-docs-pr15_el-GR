<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Picturepark | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Picturepark με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Picturepark
  
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και Picturepark.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Ένας μισθωτής Picturepark
  
Μετά την ολοκλήρωση αυτού του προγράμματος εκμάθησης, θα μπορείτε να καθολικής σύνδεσης σε μια εφαρμογή στην τοποθεσία της εταιρείας σας Picturepark (υπηρεσία παροχής που ξεκινούν από σύμβολο στην) ή με την [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md)Azure AD χρηστών που έχουν εκχωρηθεί σε Picturepark.
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα εξής δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για Picturepark
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-picturepark-tutorial/IC795055.png "Σενάριο")

##<a name="enabling-the-application-integration-for-picturepark"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για Picturepark
  
Είναι ο στόχος αυτής της ενότητας για να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για το Picturepark.

###<a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για Picturepark, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-picturepark-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-picturepark-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-picturepark-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-picturepark-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **Picturepark**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-picturepark-tutorial/IC795056.png "Συλλογή εφαρμογών")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **Picturepark**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Picturepark] (./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα Picturepark με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.  
Ρύθμιση παραμέτρων Καθολικής σύνδεσης για Picturepark απαιτεί να ανακτήσετε μια τιμή αποτύπωση από ένα πιστοποιητικό.  
Εάν δεν είστε εξοικειωμένοι με αυτήν τη διαδικασία, δείτε [πώς μπορείτε να ανακτήσετε ένα πιστοποιητικό αποτύπωση τιμή](http://youtu.be/YKQF266SAxI)...

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Picturepark** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-picturepark-tutorial/IC795058.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Picturepark** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-picturepark-tutorial/IC795059.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , στο πλαίσιο κειμένου **Picturepark εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL σας χρησιμοποιώντας το παρακάτω μοτίβο "*http://company.picturepark.com*" και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-picturepark-tutorial/IC795060.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Picturepark** , για να κάνετε λήψη του πιστοποιητικού, κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο πιστοποιητικού τοπικά στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-picturepark-tutorial/IC795061.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Σε ένα παράθυρο προγράμματος περιήγησης web διαφορετικά, συνδεθείτε στην τοποθεσία της εταιρείας σας Picturepark ως διαχειριστής.

6.  Στη γραμμή εργαλείων στο επάνω μέρος, κάντε κλικ στην επιλογή **Εργαλεία διαχείρισης**και, στη συνέχεια, κάντε κλικ στην επιλογή **Κονσόλα διαχείρισης**.

    ![Κονσόλα διαχείρισης] (./media/active-directory-saas-picturepark-tutorial/IC795062.png "Κονσόλα διαχείρισης")

7.  Κάντε κλικ στην επιλογή **Έλεγχος ταυτότητας**και, στη συνέχεια, κάντε κλικ στην επιλογή **υπηρεσίες παροχής ταυτότητας**.

    ![Έλεγχος ταυτότητας] (./media/active-directory-saas-picturepark-tutorial/IC795063.png "Έλεγχος ταυτότητας")

8.  Στην ενότητα **Ρύθμιση παραμέτρων της υπηρεσίας παροχής ταυτότητας** , ακολουθήστε τα παρακάτω βήματα:

    ![Ρύθμιση παραμέτρων της υπηρεσίας παροχής ταυτότητας] (./media/active-directory-saas-picturepark-tutorial/IC795064.png "Ρύθμιση παραμέτρων της υπηρεσίας παροχής ταυτότητας")

    1.  Κάντε κλικ στην επιλογή **Προσθήκη**.
    2.  Πληκτρολογήστε ένα όνομα για τη ρύθμιση παραμέτρων.
    3.  Επιλέξτε **Ορισμός ως προεπιλογής**.
    4.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Picturepark** , αντιγράψτε την τιμή **Διεύθυνσης URL SSO SAML** και στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **URI εκδότη** .
    5.  Αντιγράψτε την τιμή **αποτύπωση** από το πιστοποιητικό που έχει εξαχθεί και, στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **Εκτύπωση πρακτικός αξιόπιστο εκδότη** .  

        >[AZURE.TIP]Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [πώς μπορείτε να ανακτήσετε ένα πιστοποιητικό αποτύπωση τιμή](http://youtu.be/YKQF266SAxI)

    6.  Κάντε κλικ στην επιλογή **JoinDefaultUsersGroup**.
    7.  Για να ορίσετε το χαρακτηριστικό **Emailaddress** στο πλαίσιο κειμένου **διεκδίκηση** , πληκτρολογήστε **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
        ![Ρύθμιση παραμέτρων] (./media/active-directory-saas-picturepark-tutorial/IC795065.png "Ρύθμιση παραμέτρων")
    8.  Κάντε κλικ στην επιλογή **Αποθήκευση**.

9.  Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-picturepark-tutorial/IC795066.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη
  
Για να επιτρέψετε στους χρήστες Azure AD για να συνδεθείτε στο Picturepark, πρέπει να παρασχεθεί στο Picturepark.  
Στην περίπτωση Picturepark, προμήθεια είναι μια μη αυτόματη εργασία.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Η παροχή μιας λογαριασμούς χρήστη, εκτελέστε τα ακόλουθα βήματα:

1.  Συνδεθείτε στο μισθωτή σας **Picturepark** .

2.  Στη γραμμή εργαλείων στο επάνω μέρος, κάντε κλικ στην επιλογή **Εργαλεία διαχείρισης**και, στη συνέχεια, κάντε κλικ στην επιλογή **χρήστες**.

    ![Οι χρήστες] (./media/active-directory-saas-picturepark-tutorial/IC795067.png "Οι χρήστες")

3.  Στην καρτέλα **Επισκόπηση των χρηστών** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Διαχείριση χρηστών] (./media/active-directory-saas-picturepark-tutorial/IC795068.png "Διαχείριση χρηστών")

4.  Στο παράθυρο διαλόγου **Δημιουργία χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία χρήστη] (./media/active-directory-saas-picturepark-tutorial/IC795069.png "Δημιουργία χρήστη")

    1.  Πληκτρολογήστε το: **διεύθυνση ηλεκτρονικού ταχυδρομείου**, **τον κωδικό πρόσβασης**, **Επιβεβαιώστε τον κωδικό πρόσβασης**, **όνομα**, **Επώνυμο**, **εταιρεία**, **χώρα**, **ZIP**, **Πόλη** μια έγκυρη Azure Active Directory User θέλετε επιφύλαξη παροχή σε τα σχετικά πλαίσια κειμένου.
    2.  Επιλέξτε μια **γλώσσα**.
    3.  Κάντε κλικ στην επιλογή **Δημιουργία**.

>[AZURE.NOTE]Μπορείτε να χρησιμοποιήσετε οποιοδήποτε άλλο Picturepark χρήστη λογαριασμού εργαλεία δημιουργίας ή APIs που παρέχεται από Picturepark σε λογαριασμούς χρηστών AAD διάταξη.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών
  
Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες να Picturepark, εκτελέστε τα ακόλουθα βήματα:

1.  Στην κλασική Azure πύλη, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη σελίδα **Picturepark **ενοποίησης της εφαρμογής, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-picturepark-tutorial/IC795070.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-picturepark-tutorial/IC767830.png "Ναι")
  
Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).