<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Brightspace με Desire2Learn | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Brightspace με Desire2Learn με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
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

#<a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Brightspace με Desire2Learn

Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και Brightspace από Desire2Learn.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Μια Brightspace από Desire2Learn καθολικής σύνδεσης με δυνατότητα συνδρομή

Αφού ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, θα μπορούν να καθολικής σύνδεσης στην εφαρμογή στο σας Brightspace Desire2Learn εταιρική τοποθεσία (υπηρεσία παροχής που ξεκινούν από σύμβολο στην) ή χρησιμοποιώντας την [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md)Azure AD χρηστών που έχουν εκχωρηθεί σε Brightspace με Desire2Learn.

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα εξής δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για Brightspace με Desire2Learn
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Σενάριο")
##<a name="enabling-the-application-integration-for-brightspace-by-desire2learn"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για Brightspace με Desire2Learn

Στόχος αυτής της ενότητας είναι να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για Brightspace με Desire2Learn.

###<a name="to-enable-the-application-integration-for-brightspace-by-desire2learn-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για Brightspace κατά Desire2Learn, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **Brightspace από Desire2Learn**.

    ![Συλλογή Apllication] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Συλλογή Apllication")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **Brightspace από Desire2Learn**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Brightspace με Desire2Learn] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace με Desire2Learn")
##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης

Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα Brightspace με Desire2Learn με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Brightspace από Desire2Learn** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Brightspace από Desire2Learn** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

    1.  Στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από τους χρήστες σας για να συνδεθείτε στο σας **Brightspace από Desire2Learn** (π.χ.: *https://partnershowcase.desire2learn.com/Shibboleth.sso/Login?entityID=https://sts.windows-ppe.net/5caf9349-fd93-4a74-b064-0070f65bfb49/&target=https%3A%2F%2Fpartnershowcase.desire2learn.com%2Fd2l%2FshibbolethSSO%2Faspinfo.asp*).
    2.  Κάντε κλικ στο κουμπί **Επόμενο**

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Brightspace κατά Desire2Learn** , για να κάνετε λήψη μετα-δεδομένων του, κάντε κλικ στην επιλογή **Λήψη μετα-δεδομένων**και, στη συνέχεια, αποθηκεύστε τα μετα-δεδομένα στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Στείλτε το αρχείο λήψης μετα-δεδομένων για να σας Brightspace από την ομάδα υποστήριξης Desire2Learn.

    >[AZURE.NOTE] Σας Brightspace από την ομάδα υποστήριξης Desire2Learn έχει για να κάνετε την πραγματική ρύθμιση των παραμέτρων SSO.
Θα λάβετε μια ειδοποίηση όταν έχει ενεργοποιηθεί η SSO για τη συνδρομή σας.

6.  Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")
##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη

Για να ενεργοποιήσετε τους χρήστες Azure AD για να συνδεθείτε στο Brightspace κατά Desire2Learn, τους πρέπει να γίνει προμήθεια σε Brightspace με Desire2Learn.  
Στην περίπτωση Brightspace κατά Desire2Learn, τους λογαριασμούς χρήστη πρέπει να δημιουργηθούν με Brightspace σας από την ομάδα υποστήριξης Desire2Learn.

>[AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε οποιαδήποτε άλλα Brightspace, εργαλεία δημιουργίας λογαριασμού χρήστη Desire2Learn ή APIs που παρέχεται από Brightspace με Desire2Learn για την παροχή των Azure Active Directory τους λογαριασμούς χρηστών.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών

Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-brightspace-by-desire2learn-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες να Brightspace από Desire2Learn, εκτελέστε τα ακόλουθα βήματα:

1.  Στην κλασική Azure πύλη, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη σελίδα **Brightspace από Desire2Learn **ενοποίησης της εφαρμογής, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Ναι")

Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).
