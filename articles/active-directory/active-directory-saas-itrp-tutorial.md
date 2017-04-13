<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με ITRP | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε ITRP με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
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
    ms.date="09/07/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-itrp"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με ITRP
  
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και ITRP.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Ένας μισθωτής ITRP
  
Μετά την ολοκλήρωση αυτού του προγράμματος εκμάθησης, θα μπορείτε να καθολικής σύνδεσης σε μια εφαρμογή στην τοποθεσία της εταιρείας σας ITRP (υπηρεσία παροχής που ξεκινούν από σύμβολο στην) ή με την [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md)Azure AD χρηστών που έχουν εκχωρηθεί σε ITRP.
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα εξής δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για ITRP
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-itrp-tutorial/IC775551.png "Σενάριο")
##<a name="enabling-the-application-integration-for-itrp"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για ITRP
  
Είναι ο στόχος αυτής της ενότητας για να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για το ITRP.

###<a name="to-enable-the-application-integration-for-itrp-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για ITRP, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-itrp-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-itrp-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-itrp-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-itrp-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **ITRP**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-itrp-tutorial/IC775565.png "Συλλογή εφαρμογών")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **ITRP**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα ITRP με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.  
Ρύθμιση παραμέτρων Καθολικής σύνδεσης για ITRP απαιτεί να ανακτήσετε μια τιμή αποτύπωση από ένα πιστοποιητικό.  
Εάν δεν είστε εξοικειωμένοι με αυτήν τη διαδικασία, ανατρέξτε στο θέμα [πώς μπορείτε να ανακτήσετε ένα πιστοποιητικό αποτύπωση τιμή](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **ITRP** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-itrp-tutorial/IC771709.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο ITRP** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-itrp-tutorial/IC775567.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , στο πλαίσιο κειμένου **ITRP εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL σας χρησιμοποιώντας το παρακάτω μοτίβο "*https://\<μισθωτή όνομα\>. ITRP.com*", και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-itrp-tutorial/IC775568.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο ITRP** , για να κάνετε λήψη του πιστοποιητικού, κάντε κλικ στην επιλογή **Λήψη πιστοποιητικού**και, στη συνέχεια, αποθηκεύστε το αρχείο πιστοποιητικού τοπικά ως **c:\\ITRP.cer**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-itrp-tutorial/IC775569.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Σε ένα παράθυρο προγράμματος περιήγησης web διαφορετικά, συνδεθείτε στην τοποθεσία της εταιρείας σας ITRP ως διαχειριστής.

6.  Στη γραμμή εργαλείων στο επάνω μέρος, κάντε κλικ στην επιλογή **Ρυθμίσεις**.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  Στο αριστερό παράθυρο περιήγησης, επιλέξτε **Καθολικής σύνδεσης**.

    ![Καθολικής σύνδεσης] (./media/active-directory-saas-itrp-tutorial/IC775571.png "Καθολικής σύνδεσης")

8.  Στην ενότητα Ρύθμιση παραμέτρων Καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

    ![Καθολικής σύνδεσης] (./media/active-directory-saas-itrp-tutorial/IC775572.png "Καθολικής σύνδεσης")

    ![Καθολικής σύνδεσης] (./media/active-directory-saas-itrp-tutorial/IC775573.png "Καθολικής σύνδεσης")

    1.  Κάντε κλικ στην επιλογή **Ενεργοποίηση**.
    2.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο ITRP** , αντιγράψτε την τιμή **Διεύθυνση URL απομακρυσμένης αποσυνδεθείτε** και, στη συνέχεια, επικολλήστε το στο πλαίσιο κειμένου **Διεύθυνση URL απομακρυσμένης αποσύνδεσης** .
    3.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο ITRP** , αντιγράψτε την τιμή **Διεύθυνσης URL SSO SAML** και στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **Διεύθυνση URL SSO SAML** .
    4.  Αντιγράψτε την τιμή **αποτύπωση** από το πιστοποιητικό που έχει εξαχθεί και, στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **Αποτυπώματος πιστοποιητικού** .
        
        >[AZURE.TIP]Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [πώς μπορείτε να ανακτήσετε ένα πιστοποιητικό αποτύπωση τιμή](http://youtu.be/YKQF266SAxI)

    5.  Κάντε κλικ στην επιλογή **Αποθήκευση**.

9.  Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-itrp-tutorial/IC775574.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")
##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη
  
Για να επιτρέψετε στους χρήστες Azure AD για να συνδεθείτε στο ITRP, πρέπει να παρασχεθεί στο ITRP.  
Στην περίπτωση ITRP, προμήθεια είναι μια μη αυτόματη εργασία.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Η παροχή μιας λογαριασμούς χρήστη, εκτελέστε τα ακόλουθα βήματα:

1.  Συνδεθείτε στο μισθωτή σας **ITRP** .

2.  Στη γραμμή εργαλείων στο επάνω μέρος, κάντε κλικ στην επιλογή **εγγραφές**.

    ![Διαχειριστής] (./media/active-directory-saas-itrp-tutorial/IC775575.png "Διαχειριστής")

3.  Από το αναδυόμενο μενού, επιλέξτε **άτομα**.

    ![Άτομα] (./media/active-directory-saas-itrp-tutorial/IC775587.png "Άτομα")

4.  Κάντε κλικ στην επιλογή **Προσθήκη νέου προσώπου** ("+").

    ![Διαχειριστής] (./media/active-directory-saas-itrp-tutorial/IC775576.png "Διαχειριστής")

5.  Στο παράθυρο διαλόγου Προσθήκη νέου προσώπου, ακολουθήστε τα παρακάτω βήματα:

    ![Χρήστη] (./media/active-directory-saas-itrp-tutorial/IC775577.png "Χρήστη")

    1.  Πληκτρολογήστε το **όνομα**, **ηλεκτρονικού ταχυδρομείου** από έναν έγκυρο λογαριασμό AAD που θέλετε για την παροχή των.
    2.  Κάντε κλικ στην επιλογή **Αποθήκευση**.

>[AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε οποιοδήποτε άλλο ITRP χρήστη λογαριασμού εργαλεία δημιουργίας ή APIs που παρέχεται από ITRP σε λογαριασμούς χρηστών AAD διάταξη.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών
  
Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-itrp-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες να ITRP, εκτελέστε τα ακόλουθα βήματα:

1.  Στην πύλη του Azure AD, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη σελίδα **ITRP **ενοποίησης της εφαρμογής, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-itrp-tutorial/IC775588.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-itrp-tutorial/IC767830.png "Ναι")
  
Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).