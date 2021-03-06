<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με λογισμικό Rally | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Rally λογισμικό με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Rally λογισμικού
  
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση Azure και Rally λογισμικού.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Ένας μισθωτής Rally λογισμικού
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα εξής δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για το λογισμικό Rally
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-rally-software-tutorial/IC769525.png "Σενάριο")
##<a name="enabling-the-application-integration-for-rally-software"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για το λογισμικό Rally
  
Είναι ο στόχος αυτής της ενότητας για να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για το λογισμικό Rally.

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για το λογισμικό Rally, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-rally-software-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-rally-software-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-rally-software-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-rally-software-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **rally**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-rally-software-tutorial/IC769526.png "Συλλογή εφαρμογών")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε το **Λογισμικό Rally**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![RALLY λογισμικού] (./media/active-directory-saas-rally-software-tutorial/IC769527.png "RALLY λογισμικού")
##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα Rally λογισμικό με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.  
Ως μέρος αυτής της διαδικασίας, σας ζητείται να αποστείλετε ένα πιστοποιητικό για να Rally λογισμικού.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Rally λογισμικού **, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-rally-software-tutorial/IC749323.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Rally** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Microsoft Azure AD καθολικής σύνδεσης] (./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , στο πλαίσιο κειμένου **URL Rally μισθωτή του λογισμικού** , πληκτρολογήστε τη διεύθυνση URL σας χρησιμοποιώντας το παρακάτω μοτίβο "*https://\<μισθωτή όνομα\>. rally.com*", και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-rally-software-tutorial/IC769529.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Rally** , κάντε κλικ στην επιλογή λήψη μετα-δεδομένων και, στη συνέχεια, αποθηκεύστε το στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-rally-software-tutorial/IC769530.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Συνδεθείτε στο μισθωτή σας **Rally λογισμικού** .

6.  Στη γραμμή εργαλείων στο επάνω μέρος, κάντε κλικ στην επιλογή **Ρύθμιση**και, στη συνέχεια, επιλέξτε **τη συνδρομή**.

    ![Συνδρομή] (./media/active-directory-saas-rally-software-tutorial/IC769531.png "Συνδρομή")

7.  Κάντε κλικ στο κουμπί **ενέργειας** στη γραμμή εργαλείων επάνω στη δεξιά πλευρά και, στη συνέχεια, επιλέξτε **Επεξεργασία συνδρομή**.

8.  Στη σελίδα του παραθύρου διαλόγου **συνδρομής** , ακολουθήστε τα παρακάτω βήματα και, στη συνέχεια, κάντε κλικ στο κουμπί **Αποθήκευση & Κλείσιμο**:

    ![Έλεγχος ταυτότητας] (./media/active-directory-saas-rally-software-tutorial/IC769542.png "Έλεγχος ταυτότητας")

    1.  Επιλέξτε **τον έλεγχο ταυτότητας Rally ή SSO** από αναπτυσσόμενη λίστα ελέγχου ταυτότητας
    2.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο λογισμικό Rally** , αντιγράψτε την τιμή **Αναγνωριστικό υπηρεσίας παροχής ταυτότητας** και, στη συνέχεια, επικολλήστε την στο πλαίσιο κειμένου **Διεύθυνση URL της υπηρεσίας παροχής ταυτότητας**
    3.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο λογισμικό Rally** , αντιγράψτε την τιμή **Διεύθυνση URL απομακρυσμένης αποσυνδεθείτε** .

9.  Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-rally-software-tutorial/IC769547.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")
##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη
  
Για AAD τους χρήστες να μπορούν να πραγματοποιήσουν είσοδο, πρέπει να αποδοθεί στην εφαρμογή λογισμικού Rally χρησιμοποιώντας τα ονόματά χρήστης Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Για να ρυθμίσετε την προμήθεια του χρήστη, εκτελέστε τα ακόλουθα βήματα:

1.  Συνδεθείτε στο μισθωτή σας Rally λογισμικού.

2.  Μεταβείτε στις επιλογές **ρύθμισης \> ΧΡΉΣΤΕΣ**, και, στη συνέχεια, κάντε κλικ στην επιλογή **+ Add New**.

    ![Οι χρήστες] (./media/active-directory-saas-rally-software-tutorial/IC781039.png "Οι χρήστες")

3.  Πληκτρολογήστε το όνομα στο πλαίσιο κειμένου νέο χρήστη και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη με λεπτομέρειες**.

4.  Στην ενότητα **Δημιουργία χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία χρήστη] (./media/active-directory-saas-rally-software-tutorial/IC781040.png "Δημιουργία χρήστη")

    1.  Στο πλαίσιο κειμένου **Όνομα χρήστη** , πληκτρολογήστε το όνομα του χρήστη Azure AD που θέλετε για την παροχή των.
    2.  Στο πλαίσιο κειμένου **Διεύθυνση ηλεκτρονικού ταχυδρομείου** , πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου του χρήστη Azure AD που θέλετε για την παροχή των.
    3.  Κάντε κλικ στο κουμπί **Αποθήκευση & Κλείσιμο**.

>[AZURE.NOTE]Μπορείτε να χρησιμοποιήσετε οποιοδήποτε άλλο Rally λογισμικό χρήστη λογαριασμού εργαλεία δημιουργίας ή APIs που παρέχεται από λογισμικό Rally σε λογαριασμούς χρηστών AAD διάταξη.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών
  
Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες Rally λογισμικού, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική Azure πύλη, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη σελίδα ενοποίησης εφαρμογής **Rally λογισμικού** , κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-rally-software-tutorial/IC769548.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-rally-software-tutorial/IC767830.png "Ναι")
  
Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).




