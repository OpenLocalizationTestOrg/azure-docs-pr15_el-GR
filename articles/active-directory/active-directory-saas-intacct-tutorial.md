<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Intacct | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Intacct με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
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

#<a name="tutorial-azure-active-directory-integration-with-intacct"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Intacct
  
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και Intacct.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Ένας μισθωτής Intacct
  
Μετά την ολοκλήρωση αυτού του προγράμματος εκμάθησης, θα μπορείτε να καθολικής σύνδεσης σε μια εφαρμογή στην τοποθεσία της εταιρείας σας Intacct (υπηρεσία παροχής που ξεκινούν από σύμβολο στην) ή με την [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md)Azure AD χρηστών που έχουν εκχωρηθεί σε Intacct.
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα εξής δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για Intacct
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-intacct-tutorial/IC790030.png "Σενάριο")
##<a name="enabling-the-application-integration-for-intacct"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για Intacct
  
Είναι ο στόχος αυτής της ενότητας για να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για το Intacct.

###<a name="to-enable-the-application-integration-for-intacct-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για Intacct, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-intacct-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-intacct-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-intacct-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-intacct-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **Intacct**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-intacct-tutorial/IC790031.png "Συλλογή εφαρμογών")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **Intacct**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Intacct] (./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα Intacct με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.  
Ως μέρος αυτής της διαδικασίας, που απαιτούνται για να δημιουργήσετε ένα αρχείο βάσης 64 κωδικοποιημένο πιστοποιητικό.  
Εάν δεν είστε εξοικειωμένοι με αυτήν τη διαδικασία, δείτε [πώς μπορείτε να μετατρέψετε ένα πιστοποιητικό δυαδικών δεδομένων σε ένα αρχείο κειμένου](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Intacct** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-intacct-tutorial/IC790033.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Intacct** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-intacct-tutorial/IC790034.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , στο πλαίσιο κειμένου **Intacct εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL σας χρησιμοποιώντας το παρακάτω μοτίβο "*https://Intacct.com/company*" και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-intacct-tutorial/IC790035.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Intacct** , κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο πιστοποιητικού στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-intacct-tutorial/IC790036.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Σε ένα παράθυρο προγράμματος περιήγησης web διαφορετικά, συνδεθείτε στην τοποθεσία της εταιρείας σας Intacct ως διαχειριστής.

6.  Κάντε κλικ στην καρτέλα **εταιρείας** και, στη συνέχεια, κάντε κλικ στην επιλογή **Στοιχεία εταιρείας**.

    ![Εταιρεία] (./media/active-directory-saas-intacct-tutorial/IC790037.png "Εταιρεία")

7.  Κάντε κλικ στην καρτέλα **ασφάλεια** και, στη συνέχεια, κάντε κλικ στην επιλογή **Επεξεργασία**.

    ![Ασφάλεια] (./media/active-directory-saas-intacct-tutorial/IC790038.png "Ασφάλεια")

8.  Στην ενότητα **καθολικής σύνδεσης (SSO)** , ακολουθήστε τα παρακάτω βήματα:

    ![Καθολική σύνδεση] (./media/active-directory-saas-intacct-tutorial/IC790039.png "Καθολική σύνδεση")

    1.  Επιλέξτε **Ενεργοποίηση της καθολικής σύνδεσης σε**.
    2.  Ως **Τύπος υπηρεσίας παροχής ταυτότητας**, επιλέξτε **SAML 2.0**.
    3.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Intacct** , αντιγράψτε την τιμή **Διεύθυνσης URL εκδότη** και στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **Διεύθυνση URL εκδότη** .
    4.  Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Intacct** , αντιγράψτε την τιμή **Διεύθυνση URL απομακρυσμένης σύνδεσης** και, στη συνέχεια, επικολλήστε τη **Διεύθυνση URL σύνδεσης** πλαισίου κειμένου.
    5.  Δημιουργία αρχείου **βάσης 64 κωδικοποιημένο** από το πιστοποιητικό που έχετε λάβει.
        
        >[AZURE.TIP]Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [πώς μπορείτε να μετατρέψετε ένα πιστοποιητικό δυαδικών δεδομένων σε ένα αρχείο κειμένου](http://youtu.be/PlgrzUZ-Y1o)

    6.  Ανοίξτε το βάσης 64 κωδικοποιημένο πιστοποιητικό στο Σημειωματάριο, αντιγράψτε το περιεχόμενο της στο Πρόχειρο και στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **πιστοποιητικού**
    7.  Κάντε κλικ στην επιλογή **Αποθήκευση**.

9.  Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-intacct-tutorial/IC790040.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")
##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη
  
Για να επιτρέψετε στους χρήστες Azure AD για να συνδεθείτε στο Intacct, πρέπει να παρασχεθεί στο Intacct.  
Στην περίπτωση Intacct, προμήθεια είναι μια μη αυτόματη εργασία.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Η παροχή μιας λογαριασμούς χρήστη, εκτελέστε τα ακόλουθα βήματα:

1.  Συνδεθείτε στο μισθωτή σας **Intacct** .

2.  Κάντε κλικ στην καρτέλα **εταιρείας** και, στη συνέχεια, κάντε κλικ στην επιλογή **χρήστες**.

    ![Οι χρήστες] (./media/active-directory-saas-intacct-tutorial/IC790041.png "Οι χρήστες")

3.  Κάντε κλικ στην καρτέλα **Προσθήκη**

    ![Προσθήκη] (./media/active-directory-saas-intacct-tutorial/IC790042.png "Προσθήκη")

4.  Στην ενότητα **Πληροφοριών χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Πληροφορίες χρήστη] (./media/active-directory-saas-intacct-tutorial/IC790043.png "Πληροφορίες χρήστη")

    1.  Πληκτρολογήστε το **Αναγνωριστικό χρήστη**, το **Επώνυμο**, **όνομα**, τη **διεύθυνση ηλεκτρονικού ταχυδρομείου**, τον **τίτλο** και το **Τηλέφωνο** του Azure AD λογαριασμού που θέλετε για την παροχή των σε τα σχετικά πλαίσια κειμένου.
    2.  Επιλέξτε τα **δικαιώματα διαχειριστή** του Azure AD λογαριασμού που θέλετε για την παροχή των.
    3.  Κάντε κλικ στην επιλογή **Αποθήκευση**.
        
        >[AZURE.NOTE] Ο κάτοχος λογαριασμού AAD θα λάβετε ένα μήνυμα ηλεκτρονικού ταχυδρομείου και να ακολουθήσετε μια σύνδεση για να επιβεβαιώσετε το λογαριασμό, προκειμένου να καταστεί ενεργή.

>[AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε οποιοδήποτε άλλο Intacct χρήστη λογαριασμού εργαλεία δημιουργίας ή APIs που παρέχεται από Intacct σε λογαριασμούς χρηστών AAD διάταξη.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών
  
Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-intacct-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες να Intacct, εκτελέστε τα ακόλουθα βήματα:

1.  Στην κλασική Azure πύλη, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη σελίδα **Intacct **ενοποίησης της εφαρμογής, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-intacct-tutorial/IC790044.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-intacct-tutorial/IC767830.png "Ναι")
  
Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).