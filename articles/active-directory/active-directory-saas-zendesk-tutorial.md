<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Zendesk | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Zendesk με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!." 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Zendesk
  
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και Zendesk.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Ένας μισθωτής Zendesk
  
Μετά την ολοκλήρωση αυτού του προγράμματος εκμάθησης, θα μπορείτε να καθολικής σύνδεσης σε μια εφαρμογή στην τοποθεσία της εταιρείας σας Zendesk (υπηρεσία παροχής που ξεκινούν από σύμβολο στην) ή με την [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md)Azure AD χρηστών που έχουν εκχωρηθεί σε Zendesk.
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα ακόλουθα δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για Zendesk
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-zendesk-tutorial/IC773083.png "Σενάριο")

##<a name="enabling-the-application-integration-for-zendesk"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για Zendesk
  
Είναι ο στόχος αυτής της ενότητας για να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για το Zendesk.

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για Zendesk, ακολουθήστε τα παρακάτω βήματα:

1.  Στην πύλη διαχείρισης του Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-zendesk-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-zendesk-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-zendesk-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-zendesk-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **Zendesk**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-zendesk-tutorial/IC773084.png "Συλλογή εφαρμογών")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **Zendesk**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Zendesk] (./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα Zendesk με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.  
Ρύθμιση παραμέτρων Καθολικής σύνδεσης για Zendesk απαιτεί να ανακτήσετε μια τιμή αποτύπωση από ένα πιστοποιητικό.  
Εάν δεν είστε εξοικειωμένοι με αυτήν τη διαδικασία, ανατρέξτε στο θέμα [πώς μπορείτε να ανακτήσετε ένα πιστοποιητικό αποτύπωση τιμή](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην πύλη του Azure AD, στη σελίδα ενοποίησης εφαρμογής **Zendesk** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Καθολικής σύνδεσης] (./media/active-directory-saas-zendesk-tutorial/IC773086.png "Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Zendesk** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-zendesk-tutorial/IC773087.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-zendesk-tutorial/IC773088.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")
  
    μια. Στο πλαίσιο κειμένου **Zendesk εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL σας χρησιμοποιώντας το παρακάτω μοτίβο:`https://<tenant-name>.zendesk.com`

    β. Κάντε κλικ στο κουμπί **Επόμενο**.



4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Zendesk** , κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο πιστοποιητικού τοπικά στο compiter σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-zendesk-tutorial/IC777534.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Σε ένα παράθυρο προγράμματος περιήγησης web διαφορετικά, συνδεθείτε στην τοποθεσία της εταιρείας σας Zendesk ως διαχειριστής.

6.  Κάντε κλικ στην επιλογή **Διαχείριση**.

7.  Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Ρυθμίσεις**και, στη συνέχεια, κάντε κλικ στην επιλογή **ασφάλεια**.

    ![Ασφάλεια] (./media/active-directory-saas-zendesk-tutorial/IC773089.png "Ασφάλεια")

8.  Στη σελίδα για την **ασφάλεια** , κάντε κλικ στην καρτέλα **διαχείρισης & παραγόντων** .

9.  Επιλέξτε **μονό καθολικής σύνδεσης (SSO) και SAML**, και, στη συνέχεια, επιλέξτε **SAML**.

10. Στην πύλη του Azure AD, στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Zendesk** , αντιγράψτε την τιμή **Διεύθυνσης URL SSO SAML** και, στη συνέχεια, επικολλήστε την στο πλαίσιο κειμένου **Διεύθυνση URL SSO SAML** .

11. Στην πύλη του Azure AD, στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Zendesk** , αντιγράψτε την τιμή **Διεύθυνση URL απομακρυσμένης αποσυνδεθείτε** και, στη συνέχεια, επικολλήστε την στο πλαίσιο κειμένου **Διεύθυνση URL απομακρυσμένης αποσυνδεθείτε** .

    ![Καθολικής σύνδεσης] (./media/active-directory-saas-zendesk-tutorial/IC773090.png "Καθολικής σύνδεσης")

12. Αντιγράψτε την τιμή **αποτύπωση** από το πιστοποιητικό που έχει εξαχθεί και, στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **Αποτυπώματος πιστοποιητικού** .

    >[AZURE.TIP] Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [πώς μπορείτε να ανακτήσετε ένα πιστοποιητικό αποτύπωση τιμή](http://youtu.be/YKQF266SAxI)

13. Κάντε κλικ στην επιλογή **Αποθήκευση**.

14. Στην πύλη Azure AD, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-zendesk-tutorial/IC773093.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη
  
Για να επιτρέψετε στους χρήστες Azure AD για να συνδεθείτε στο **Zendesk**, πρέπει να παρασχεθεί στο **Zendesk**.  
Στην περίπτωση **Zendesk**, προμήθεια είναι μια μη αυτόματη εργασία.

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Για να προμηθεύσουν ένα λογαριασμό χρήστη για να Zendesk, εκτελέστε τα ακόλουθα βήματα:

1.  Συνδεθείτε στο μισθωτή σας **Zendesk** .

2.  Επιλέξτε την καρτέλα **Λίστα πελατών** .

3.  Επιλέξτε την καρτέλα **χρήστη** και κάντε κλικ στην επιλογή **Προσθήκη**.

    ![Προσθήκη χρήστη] (./media/active-directory-saas-zendesk-tutorial/IC773632.png "Προσθήκη χρήστη")

4.  Πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου από έναν υπάρχοντα λογαριασμό Azure AD που θέλετε για την παροχή των και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Νέο χρήστη] (./media/active-directory-saas-zendesk-tutorial/IC773633.png "Νέο χρήστη")

>[AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε οποιοδήποτε άλλο Zendesk χρήστη λογαριασμού εργαλεία δημιουργίας ή APIs που παρέχεται από Zendesk σε λογαριασμούς χρηστών AAD διάταξη.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών
  
Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες να Zendesk, εκτελέστε τα ακόλουθα βήματα:

1.  Στην πύλη του Azure AD, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη σελίδα ενοποίησης εφαρμογής **Zendesk **, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-zendesk-tutorial/IC773094.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-zendesk-tutorial/IC767830.png "Ναι")
  
Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).
