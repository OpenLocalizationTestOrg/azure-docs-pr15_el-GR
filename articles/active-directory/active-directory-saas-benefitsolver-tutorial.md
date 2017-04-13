<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Benefitsolver | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Benefitsolver με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Benefitsolver

Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και Benefitsolver.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Μια Benefitsolver καθολικής σύνδεσης με δυνατότητα συνδρομή

Μετά την ολοκλήρωση αυτού του προγράμματος εκμάθησης, θα μπορείτε να καθολικής σύνδεσης στην εφαρμογή με την [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md)Azure AD χρηστών που έχουν εκχωρηθεί σε Benefitsolver.

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα εξής δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για Benefitsolver
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Σενάριο")
##<a name="enabling-the-application-integration-for-benefitsolver"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για Benefitsolver

Είναι ο στόχος αυτής της ενότητας για να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για το Benefitsolver.

###<a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για Benefitsolver, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **Benefitsolver**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Συλλογή εφαρμογών")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **Benefitsolver**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Benefitssolver] (./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης

Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα Benefitsolver με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.  
Η εφαρμογή σας Benefitsolver αναμένει το διεκδικήσεων SAML με μια συγκεκριμένη μορφή που απαιτεί να προσθέσετε προσαρμοσμένες αντιστοιχίσεις χαρακτηριστικών της ρύθμισης παραμέτρων του **saml διακριτικού χαρακτηριστικά** .  
Το παρακάτω στιγμιότυπο οθόνης εμφανίζει ένα παράδειγμα για αυτό.

![Χαρακτηριστικά] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Χαρακτηριστικά")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Benefitsolver** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Benefitsolver** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων των ρυθμίσεων της εφαρμογής** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων εφαρμογής] (./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Ρύθμιση παραμέτρων εφαρμογής")

    1.  Στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε **http://azure.benefitsolver.com**.
    2.  Στο πλαίσιο κειμένου **Διεύθυνση URL απάντηση** , πληκτρολογήστε **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  


    3.  Κάντε κλικ στο κουμπί **Επόμενο**.

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Benefitsolver** , για να κάνετε λήψη του μετα-δεδομένων, κάντε κλικ στην επιλογή **Λήψη μετα-δεδομένων**και, στη συνέχεια, αποθηκεύστε το αρχείο μετα-δεδομένων τοπικά στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Στείλτε το αρχείο λήψης μετα-δεδομένα, στην ομάδα υποστήριξής σας Benefitsolver.

    >[AZURE.NOTE] Η ομάδα υποστήριξης Benefitsolver έχει για να κάνετε την πραγματική ρύθμιση των παραμέτρων SSO.
Θα λάβετε μια ειδοποίηση όταν έχει ενεργοποιηθεί η SSO για τη συνδρομή σας.

6.  Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

7.  Στο μενού στην επάνω, κάντε κλικ στην επιλογή **Ιδιότητες** για να ανοίξετε το παράθυρο διαλόγου **SAML διακριτικού χαρακτηριστικά** .

    ![Χαρακτηριστικά] (./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Χαρακτηριστικά")

8.  Για να προσθέσετε αντιστοιχίσεις απαιτούμενο χαρακτηριστικό, ακολουθήστε τα παρακάτω βήματα:

    ![Χαρακτηριστικά] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Χαρακτηριστικά")

  	|Όνομα χαρακτηριστικού|Τιμή του χαρακτηριστικού|
  	|---|---|
  	|ClientID|Πρέπει να λάβετε αυτήν την τιμή από την ομάδα υποστήριξης Benefitsolver.|
  	|ClientKey|Πρέπει να λάβετε αυτήν την τιμή από την ομάδα υποστήριξης Benefitsolver.|
  	|LogoutURL|Πρέπει να λάβετε αυτήν την τιμή από την ομάδα υποστήριξης Benefitsolver.|
  	|Το EmployeeID|Πρέπει να λάβετε αυτήν την τιμή από την ομάδα υποστήριξης Benefitsolver.|

    1.  Για κάθε γραμμή δεδομένων στον παραπάνω πίνακα, κάντε κλικ στην επιλογή **Προσθήκη χρήστη χαρακτηριστικό**.
    2.  Στο πλαίσιο κειμένου **Όνομα χαρακτηριστικού** , πληκτρολογήστε το όνομα χαρακτηριστικού εμφανίζονται για τη συγκεκριμένη γραμμή.
    3.  Στο πλαίσιο κειμένου **Τιμή του χαρακτηριστικού** , επιλέξτε την τιμή του χαρακτηριστικού εμφανίζονται για τη συγκεκριμένη γραμμή.
    4.  Κάντε κλικ στην επιλογή **Ολοκλήρωση**.

9.  Κάντε κλικ στην επιλογή **Εφαρμογή αλλαγών**.

##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη

Για να επιτρέψετε στους χρήστες Azure AD για να συνδεθείτε στο Benefitsolver, πρέπει να παρασχεθεί στο Benefitsolver.  
Στην περίπτωση Benefitsolver, δεδομένα υπαλλήλων είναι στην εφαρμογή σας συμπληρωμένο μέσω ενός αρχείου απογραφή από το σύστημα HRIS (συνήθως nightly).  

>[AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε οποιοδήποτε άλλο Benefitsolver χρήστη λογαριασμού εργαλεία δημιουργίας ή APIs που παρέχεται από Benefitsolver σε λογαριασμούς χρηστών AAD διάταξη.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών

Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες να Benefitsolver, εκτελέστε τα ακόλουθα βήματα:

1.  Στην κλασική Azure πύλη, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη σελίδα **Benefitsolver **ενοποίησης της εφαρμογής, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ναι")

Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).