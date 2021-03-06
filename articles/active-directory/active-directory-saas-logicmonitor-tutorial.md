<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με LogicMonitor | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε LogicMonitor με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
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

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με LogicMonitor
  
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και LogicMonitor.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Ένας μισθωτής LogicMonitor
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα ακόλουθα δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για LogicMonitor
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Σενάριο")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για LogicMonitor
  
Είναι ο στόχος αυτής της ενότητας για να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για το LogicMonitor.

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για LogicMonitor, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **logicmonitor**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Συλλογή εφαρμογών")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **LogicMonitor**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![LogicMonitor] (./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα LogicMonitor με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **LogicMonitor **, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο LogicMonitor** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από τους χρήστες σας για να συνδεθείτε LogicMonitor \(e, g,: "*http://company.logicmonitor.com*"\), και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο LogicMonitor** , κάντε κλικ στην επιλογή **Λήψη μετα-δεδομένων**και, στη συνέχεια, αποθηκεύστε το στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Συνδεθείτε στην τοποθεσία της εταιρείας σας **LogicMonitor** ως διαχειριστής.

6.  Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **Ρυθμίσεις**.

    ![Ρυθμίσεις] (./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Ρυθμίσεις")

7.  Στο τη γραμμή περιήγησης στην αριστερή πλευρά, κάντε κλικ στην επιλογή **Καθολικής σύνδεσης**

    ![Καθολικής σύνδεσης] (./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Καθολικής σύνδεσης")

8.  Στην ενότητα **Ρυθμίσεις Καθολικής σύνδεσης (SSO)** , ακολουθήστε τα παρακάτω βήματα:

    ![Ρυθμίσεις Καθολικής σύνδεσης] (./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Ρυθμίσεις Καθολικής σύνδεσης")

    1.  Επιλέξτε **Ενεργοποίηση της καθολικής σύνδεσης**.
    2.  Ως **Προεπιλεγμένη εκχώρηση ρόλων**, επιλέξτε **μόνο για ανάγνωση**.
    3.  Ανοίξτε το αρχείο λήψης μετα-δεδομένων στο Σημειωματάριο και, στη συνέχεια, επικολλήστε περιεχόμενο του αρχείου στο πλαίσιο κειμένου **Μετα-δεδομένων υπηρεσίας παροχής ταυτότητας** .
    4.  Κάντε κλικ στην επιλογή **Αποθήκευση αλλαγών**.

9.  Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")
##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη
  
Για AAD τους χρήστες να μπορούν να πραγματοποιήσουν είσοδο, πρέπει να αποδοθεί στην εφαρμογή LogicMonitor χρησιμοποιώντας τα ονόματά χρήστης Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους προμήθεια του χρήστη, εκτελέστε τα ακόλουθα βήματα:

1.  Συνδεθείτε στην τοποθεσία της εταιρείας σας LogicMonitor ως διαχειριστής.

2.  Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **Ρυθμίσεις**και, στη συνέχεια, κάντε κλικ στην επιλογή **ρόλων και των χρηστών**.

    ![Ρόλοι και τους χρήστες] (./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Ρόλοι και τους χρήστες")

3.  Κάντε κλικ στην επιλογή **Προσθήκη**.

4.  Στην ενότητα **Προσθήκη λογαριασμού** , εκτελέστε τα ακόλουθα βήματα:

    ![Προσθήκη λογαριασμού] (./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Προσθήκη λογαριασμού")

    1.  Πληκτρολογήστε τις τιμές **όνομα_χρήστη**, **ηλεκτρονικού ταχυδρομείου**, **τον κωδικό πρόσβασης** και **Πληκτρολογήστε ξανά τον κωδικό πρόσβασης** του χρήστη Azure Active Directory που θέλετε για την παροχή των σε τα σχετικά πλαίσια κειμένου.
    2.  Επιλέξτε **τους ρόλους**, **Προβολή δικαιωμάτων** και την **κατάσταση**.
    3.  Κάντε κλικ στην επιλογή **Υποβολή**.

>[AZURE.NOTE]Μπορείτε να χρησιμοποιήσετε οποιοδήποτε άλλο LogicMonitor χρήστη λογαριασμού εργαλεία δημιουργίας ή APIs που παρέχεται από LogicMonitor για την παροχή των Azure Active Directory τους λογαριασμούς χρηστών.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών
  
Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες να LogicMonitor, εκτελέστε τα ακόλουθα βήματα:

1.  Στην κλασική Azure πύλη, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη σελίδα **LogicMonitor** ενοποίησης της εφαρμογής, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Ναι")
  
Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).




