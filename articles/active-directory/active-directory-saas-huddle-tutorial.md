<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Huddle | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Huddle με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Huddle
  
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και Huddle.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Μια Huddle καθολικής σύνδεσης με δυνατότητα συνδρομή
  
Μετά την ολοκλήρωση αυτού του προγράμματος εκμάθησης, θα μπορείτε να καθολικής σύνδεσης σε μια εφαρμογή στην τοποθεσία της εταιρείας σας Huddle (υπηρεσία παροχής που ξεκινούν από σύμβολο στην) ή με την [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md)Azure AD χρηστών που έχουν εκχωρηθεί σε Huddle.
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα εξής δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για Huddle
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-huddle-tutorial/IC787830.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")
##<a name="enabling-the-application-integration-for-huddle"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για Huddle
  
Είναι ο στόχος αυτής της ενότητας για να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για το Huddle.

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για Huddle, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-huddle-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-huddle-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-huddle-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-huddle-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **Huddle**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-huddle-tutorial/IC787831.png "Συλλογή εφαρμογών")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **Huddle**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Huddle] (./media/active-directory-saas-huddle-tutorial/IC787832.png "Huddle")
##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα Huddle με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη **Huddle** ενοποίησης σελίδα εφαρμογής, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-huddle-tutorial/IC787833.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Huddle** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-huddle-tutorial/IC787834.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , στο πλαίσιο κειμένου **Huddle εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL του μισθωτή σας Huddle χρησιμοποιώντας το ακόλουθο μοτίβο "*http://company.huddle.com*" και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-huddle-tutorial/IC787835.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Huddle** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-huddle-tutorial/IC787836.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

    1.  Κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο πιστοποιητικού στον υπολογιστή σας.
    2.  Αντιγράψτε την τιμή **Διεύθυνσης URL εκδότη** , τη **Διεύθυνση URL SSO SAML** τιμή και το πιστοποιητικό που έχετε λάβει και, στη συνέχεια, στείλτε τους για την ομάδα υποστήριξης του Huddle.

    >[AZURE.NOTE] Καθολικής σύνδεσης πρέπει να είναι ενεργοποιημένες από την ομάδα υποστήριξης Huddle.
Θα λάβετε μια ειδοποίηση όταν έχει ολοκληρωθεί η ρύθμιση παραμέτρων.

5.  Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-huddle-tutorial/IC787837.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")
##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη
  
Για να επιτρέψετε στους χρήστες Azure AD για να συνδεθείτε στο Huddle, πρέπει να παρασχεθεί στο Huddle.  
Στην περίπτωση Huddle, προμήθεια είναι μια μη αυτόματη εργασία.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Για να ρυθμίσετε την προμήθεια του χρήστη, εκτελέστε τα ακόλουθα βήματα:

1.  Συνδεθείτε στο **Huddle** τοποθεσία της εταιρείας σας ως διαχειριστής.

2.  Κάντε κλικ στο **χώρο εργασίας**.

3.  Κάντε κλικ στην επιλογή **άτομα \> Πρόσκληση ατόμων**.

    ![Άτομα] (./media/active-directory-saas-huddle-tutorial/IC787838.png "Άτομα")

4.  Στην ενότητα **Δημιουργία μια νέα πρόσκληση** , ακολουθήστε τα παρακάτω βήματα:

    ![Νέα πρόσκληση] (./media/active-directory-saas-huddle-tutorial/IC787839.png "Νέα πρόσκληση")

    1.  Στη λίστα **Επιλέξτε μια ομάδα για να προσκαλέσετε άτομα για να συμμετάσχετε** , επιλέξτε **την ομάδα**.
    2.  Πληκτρολογήστε τη **Διεύθυνση ηλεκτρονικού ταχυδρομείου** από έναν έγκυρο λογαριασμό AAD που θέλετε για την παροχή των στο σχετικό πλαίσιο κειμένου.
    3.  Κάντε κλικ στην **πρόσκληση**.

    >[AZURE.NOTE] Ο κάτοχος λογαριασμός Azure AD θα λάβουν ένα μήνυμα ηλεκτρονικού ταχυδρομείου, όπως μια σύνδεση για να επιβεβαιώσετε το λογαριασμό, προκειμένου να καταστεί ενεργή.

>[AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε οποιοδήποτε άλλο Huddle χρήστη λογαριασμού εργαλεία δημιουργίας ή APIs που παρέχεται από Huddle για την παροχή των AAD λογαριασμών χρηστών.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών
  
Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες να Huddle, εκτελέστε τα ακόλουθα βήματα:

1.  Στην κλασική Azure πύλη, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη **Huddle **ενοποίησης σελίδα εφαρμογής, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-huddle-tutorial/IC787840.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-huddle-tutorial/IC767830.png "Ναι")
  
Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).