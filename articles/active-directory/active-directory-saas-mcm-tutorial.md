<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με με | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε με με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!" 
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
    ms.date="08/30/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mcm"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με με
  
Στόχος αυτού του προγράμματος εκμάθησης είναι ώστε να σας δείξουν πώς μπορείτε να ενοποιήσετε με με Azure Active Directory (Azure AD).

Ενοποίηση με με Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα:

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση σε με
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να συνδεθεί στην με (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ρυθμίσετε τις παραμέτρους ενοποίησης Azure AD με με, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια έγκυρη συνδρομή Azure
- Μια με καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Σενάριο περιγραφή
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής.

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύριο μπλοκ δόμησης:

1. Προσθήκη με από τη συλλογή
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης

## <a name="adding-mcm-from-the-gallery"></a>Προσθήκη με από τη συλλογή
Για να ρυθμίσετε την ενσωμάτωση των με στην Azure AD, πρέπει να προσθέσετε με από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε με από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "Προσθήκη εφαρμογής")

5.  Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "Προσθήκη εφαρμογής από gallerry")

6.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **με**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "Συλλογή εφαρμογών")

7.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **με**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Με] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "Με")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Ο στόχος αυτής της ενότητας είναι να σας δείξουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με με βάση μια δοκιμής χρήστη που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, πρέπει να γνωρίζετε τι είναι το αντίστοιχο χρήστη στο με ένα χρήστη στο Azure AD της Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στο με πρέπει να καθοριστούν.

Αυτή η σχέση σύνδεση είναι εγκατεστημένος κατά την αντιστοίχιση της τιμής του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** στο με.

Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με με, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
3. **[Τη δημιουργία μιας με δοκιμή χρήστη](#creating-a-mcm-test-user)** - έχουν αντίστοιχο του Britta Simon στο με που συνδέεται με το Azure AD αναπαράσταση εκείνη.
4. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης
  
Σε αυτήν την ενότητα, ενεργοποιείτε Azure AD καθολικής σύνδεσης στην πύλη του κλασική και ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή σας με.

**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με με, εκτελέστε τα ακόλουθα βήματα:**

1.  Στην κλασική πύλη Azure, στη σελίδα ενοποίηση **με** εφαρμογή, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο με** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Microsoft Azure AD καθολικής σύνδεσης] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure AD καθολικής σύνδεσης")

3.  Στη σελίδα του παραθύρου διαλόγου διαμόρφωση παραμέτρων ρυθμίσεων εφαρμογής, εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

    μια. Στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε: `https://myaba.co.uk/client-access/<company name>/saml.php`.
    
    β. Κάντε κλικ στο κουμπί **Επόμενο**

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο με** , κάντε κλικ στην επιλογή **Λήψη μετα-δεδομένων**και, στη συνέχεια, αποθηκεύστε το αρχείο πιστοποιητικού στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5. Για να λάβετε SSO έχει ρυθμιστεί για την εφαρμογή σας, επικοινωνήστε με την ομάδα υποστήριξης με. Επισυνάψτε το αρχείο που έχετε λάβει μετα-δεδομένων και να το μοιραστείτε με με την ομάδα για να ρυθμίσετε SSO στην πλευρά τους.

6.  Στην κλασική πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

7. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")


### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD

Στόχος αυτής της ενότητας είναι να δημιουργήσετε έναν χρήστη δοκιμής στην κλασική πύλη που ονομάζεται Britta Simon.

![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)

4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα:
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)

    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   

###<a name="creating-a-mcm-test-user"></a>Δημιουργία ενός χρήστη με έλεγχο
  
Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο με. Να εργαστείτε επικοινωνήστε με την ομάδα υποστήριξης για να προσθέσετε τους χρήστες στην πλατφόρμα με.

>[AZURE.NOTE]Μπορείτε να χρησιμοποιήσετε οποιοδήποτε άλλο με χρήστη λογαριασμού εργαλεία δημιουργίας ή APIs που παρέχεται από με λογαριασμούς χρηστών AAD διάταξη.


###<a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη
  
Στόχος αυτής της ενότητας είναι η ενεργοποίηση Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης σε με.
    
![Αντιστοίχιση χρηστών] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "Αντιστοίχιση χρηστών")

**Για να αντιστοιχίσετε Britta Simon με, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.
    
    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "Αντιστοίχιση χρηστών")

2. Στη λίστα εφαρμογών, επιλέξτε **με**.
    
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)

1. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.
    
    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "Αντιστοίχιση χρηστών")

1. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

2. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.
    
    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "Αντιστοίχιση χρηστών")


### <a name="testing-single-sign-on"></a>Δοκιμές καθολικής σύνδεσης

Είναι ο στόχος αυτής της ενότητας για να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.
 
Όταν κάνετε κλικ στο πλακίδιο με στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε στην εφαρμογή σας με.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)