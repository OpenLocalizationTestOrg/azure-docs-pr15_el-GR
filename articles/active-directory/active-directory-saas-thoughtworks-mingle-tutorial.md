<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Thoughtworks Mingle | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Thoughtworks Mingle με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Thoughtworks Mingle
  
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και Thoughtworks Mingle.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Ένας μισθωτής Thoughtworks Mingle
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα εξής δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για Thoughtworks Mingle
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
4.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Σενάριο")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για Thoughtworks Mingle
  
Στόχος αυτής της ενότητας είναι να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για Thoughtworks Mingle.

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για Thoughtworks Mingle, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **thoughtworks mingle**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Συλλογή εφαρμογών")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **Thoughtworks Mingle**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Thoughtworks Mingle] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks Mingle")

##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα Thoughtworks Mingle με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.  
Ως μέρος αυτής της διαδικασίας, σας ζητείται να αποστείλετε ένα πιστοποιητικό για να Thoughtworks Mingle.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη **Thoughtworks Mingle **ενοποίησης σελίδα εφαρμογής, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Thoughtworks Mingle** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , στο πλαίσιο **Διεύθυνση URL μισθωτή Mingle Thoughtworks** κειμένου, πληκτρολογήστε τη διεύθυνση URL σας χρησιμοποιώντας το παρακάτω μοτίβο "*http://company.mingle.thoughtworks.com*" και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Thoughtworks Mingle** , κάντε κλικ στην επιλογή λήψη μετα-δεδομένων και, στη συνέχεια, αποθηκεύστε το στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Συνδεθείτε στο **Thoughtworks Mingle** τοποθεσία της εταιρείας σας ως διαχειριστής.

6.  Κάντε κλικ στην καρτέλα **Διαχείριση** και, στη συνέχεια, κάντε κλικ στην επιλογή **SSO Config**.

    ![SSO ρύθμισης παραμέτρων] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "SSO ρύθμισης παραμέτρων")

7.  Στην ενότητα **SSO Config** , ακολουθήστε τα παρακάτω βήματα:

    ![SSO ρύθμισης παραμέτρων] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "SSO ρύθμισης παραμέτρων")

    1.  Για να αποστείλετε το αρχείο μετα-δεδομένων, κάντε κλικ στην επιλογή " **Επιλογή αρχείου**".
    2.  Κάντε κλικ στην επιλογή **Αποθήκευση αλλαγών**.

8.  Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη
  
Για AAD τους χρήστες να μπορούν να πραγματοποιήσουν είσοδο, πρέπει να αποδοθεί στην εφαρμογή Thoughtworks Mingle χρησιμοποιώντας τα ονόματά χρήστης Azure Active Directory.  
Στην περίπτωση Thoughtworks Mingle, προμήθεια είναι μια μη αυτόματη εργασία.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Για να ρυθμίσετε την προμήθεια του χρήστη, εκτελέστε τα ακόλουθα βήματα:

1.  Συνδεθείτε στο Thoughtworks Mingle τοποθεσία της εταιρείας σας ως διαχειριστής.

2.  Κάντε κλικ στην επιλογή **προφίλ**.

    ![Το πρώτο σας έργο] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Το πρώτο σας έργο")

3.  Κάντε κλικ στην καρτέλα **Διαχείριση** και, στη συνέχεια, κάντε κλικ στην επιλογή **χρήστες**.

    ![Οι χρήστες] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Οι χρήστες")

4.  Κάντε κλικ στην επιλογή **νέο χρήστη**.

    ![Νέο χρήστη] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Νέο χρήστη")

5.  Στη σελίδα του παραθύρου διαλόγου **Νέο χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Νέο χρήστη] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Νέο χρήστη")

    1.  Πληκτρολογήστε το **όνομα εισόδου**, **εμφανιζόμενο όνομα**, **επιλογή τον κωδικό πρόσβασης**, **Επιβεβαιώστε τον κωδικό πρόσβασης** από έναν έγκυρο λογαριασμό AAD που θέλετε για την παροχή των σε τα σχετικά πλαίσια κειμένου.
    2.  Ως **τύπο χρήστη**, επιλέξτε **Πλήρης χρήστη**.
    3.  Κάντε κλικ στην επιλογή **Δημιουργία αυτό το προφίλ**.

>[AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε οποιαδήποτε άλλα Thoughtworks Mingle χρήστη λογαριασμού εργαλεία δημιουργίας ή APIs που παρέχεται από Thoughtworks Mingle σε λογαριασμούς χρηστών AAD διάταξη.

##<a name="assigning-users"></a>Αντιστοίχιση χρηστών
  
Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες να Thoughtworks Mingle, εκτελέστε τα ακόλουθα βήματα:

1.  Στην κλασική Azure πύλη, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη **Thoughtworks Mingle** ενοποίησης σελίδα εφαρμογής, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Ναι")
  
Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](active-directory-saas-access-panel-introduction.md).
