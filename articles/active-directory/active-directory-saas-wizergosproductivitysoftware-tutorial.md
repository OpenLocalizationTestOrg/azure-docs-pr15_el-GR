<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με λογισμικό παραγωγικότητας Wizergos | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καθολικής σύνδεσης μεταξύ Azure Active Directory και λογισμικό παραγωγικότητας Wizergos."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με λογισμικό παραγωγικότητας Wizergos 

Ο στόχος αυτού του προγράμματος εκμάθησης είναι να σας δείξουν πώς μπορείτε να ενοποιήσετε το λογισμικό παραγωγικότητας Wizergos με Azure Active Directory (Azure AD).

Ενοποίηση λογισμικό παραγωγικότητας Wizergos με Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα:

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση σε λογισμικό παραγωγικότητας Wizergos
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να συνδεθεί στην λογισμικό παραγωγικότητας Wizergos (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ρυθμίσετε τις παραμέτρους ενοποίησης Azure AD με λογισμικό παραγωγικότητας Wizergos, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Ένα λογισμικό παραγωγικότητας Wizergos καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Σενάριο περιγραφή
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής.

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύριο μπλοκ δόμησης:

1. Προσθήκη λογισμικό παραγωγικότητας Wizergos από τη συλλογή
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Προσθήκη λογισμικό παραγωγικότητας Wizergos από τη συλλογή
Για να ρυθμίσετε την ενσωμάτωση του λογισμικού παραγωγικότητα Wizergos στην Azure AD, πρέπει να προσθέσετε λογισμικό παραγωγικότητας Wizergos από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε λογισμικό παραγωγικότητας Wizergos από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**. 

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.
    
    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.
    
    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **Λογισμικό παραγωγικότητας Wizergos**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε το **Λογισμικό παραγωγικότητας Wizergos**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Επιλογή της εφαρμογής στη συλλογή](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Ο στόχος αυτής της ενότητας είναι να σας δείξουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με το λογισμικό παραγωγικότητας Wizergos που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε ποιος είναι ο χρήστης αντίστοιχο στο λογισμικό παραγωγικότητας Wizergos σε έναν χρήστη στο Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στο λογισμικό παραγωγικότητας Wizergos πρέπει να καθοριστούν.

Αυτή η σχέση σύνδεση είναι εγκατεστημένος κατά την αντιστοίχιση της τιμής του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** στο λογισμικό παραγωγικότητας Wizergos.

Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με Softwareder παραγωγικότητας BynWizergos, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
3. **[Δημιουργία ένα λογισμικό παραγωγικότητας Wizergos δοκιμή χρήστη](#creating-a-wizergos-productivity-software-test-user)** - έχουν αντίστοιχο του Britta Simon στο λογισμικό παραγωγικότητας Wizergos που είναι συνδεδεμένο με το Azure AD αναπαράσταση εκείνη.
4. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Σε αυτήν την ενότητα, ενεργοποιείτε Azure AD καθολικής σύνδεσης στην πύλη του κλασική και ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή σας λογισμικό παραγωγικότητας Wizergos.

**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με το λογισμικό παραγωγικότητας Wizergos, εκτελέστε τα ακόλουθα βήματα:**

1. Στην κλασική πύλη, στη σελίδα **Λογισμικό παραγωγικότητας Wizergos** ενοποίησης της εφαρμογής, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .
     
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6] 

2. Στη σελίδα **Πώς θα θέλατε χρήστες να πραγματοποιούν είσοδο λογισμικό παραγωγικότητας Wizergos** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στην επιλογή **Επόμενο**:
    
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)

3. Στη σελίδα **Ρύθμιση παραμέτρων των ρυθμίσεων της εφαρμογής** παραθύρου διαλόγου, κάντε κλικ στην επιλογή **Επόμενο**:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)

4. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο λογισμικό παραγωγικότητας Wizergos** , κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο στον υπολογιστή σας:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)

5. Σε ένα παράθυρο προγράμματος περιήγησης web διαφορετικά, καθολικής σύνδεσης στο μισθωτή του λογισμικού παραγωγικότητα Wizergos ως διαχειριστής.

6. Από το μενού hamburger, επιλέξτε **Διαχείριση**.

    ![Ρύθμιση παραμέτρων πλευρά καθολική σύνδεση στην εφαρμογή](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)

7. Στη σελίδα διαχείρισης στο αριστερό μενού επιλέξτε **Έλεγχος ΤΑΥΤΌΤΗΤΑΣ** και κάντε κλικ στο **Azure AD**.

    ![Ρύθμιση παραμέτρων πλευρά καθολική σύνδεση στην εφαρμογή](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)

8. Εκτελέστε τα ακόλουθα βήματα στην ενότητα **Έλεγχος ΤΑΥΤΌΤΗΤΑΣ** .

    ![Ρύθμιση παραμέτρων πλευρά καθολική σύνδεση στην εφαρμογή](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)

    μια. Κάντε κλικ στο κουμπί **ΑΠΟΣΤΟΛΉ** για να αποστείλετε το πιστοποιητικό που έχετε λάβει από το Azure AD. 

    β. Στο **URL εκδότη** πλαίσιο κειμένου, τοποθετήστε την τιμή της **Διεύθυνσης URL εκδότη** από τον Οδηγό ρύθμισης παραμέτρων εφαρμογής Azure AD.

    c. Στο **Μεμονωμένο URL καθολικής σύνδεσης** πλαισίου κειμένου, τοποθετήστε την τιμή του **Μία μόνο είσοδο στη διεύθυνση URL της υπηρεσίας** από τον Οδηγό ρύθμισης παραμέτρων εφαρμογής Azure AD.

    d. Σε **Μία διεύθυνση URL Sign-Out** πλαίσιο κειμένου, τοποθετήστε την τιμή του **Μία διεύθυνση URL της υπηρεσίας Sign-out** από τον Οδηγό ρύθμισης παραμέτρων εφαρμογής Azure AD.

    ε. Κάντε κλικ στο κουμπί **Αποθήκευση** .

9. Στην κλασική πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
    
    ![Azure AD καθολικής σύνδεσης][10]

10. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  
    
    ![Azure AD καθολικής σύνδεσης][11]



### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Στόχος αυτής της ενότητας είναι να δημιουργήσετε έναν χρήστη δοκιμής στην κλασική πύλη που ονομάζεται Britta Simon.

![Δημιουργία Azure AD χρήστη][20]

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)

4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png)

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα:
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)

    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   



### <a name="creating-a-wizergos-productivity-software-test-user"></a>Δημιουργία ενός χρήστη δοκιμής λογισμικό παραγωγικότητας Wizergos

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο λογισμικό παραγωγικότητας Wizergos. Εργασία επικοινωνήστε με την ομάδα υποστήριξης λογισμικό παραγωγικότητας Wizergos μέσω [support@wizergos.com](emailTo:support@wizergos.com) για να προσθέσετε τους χρήστες στην πλατφόρμα λογισμικό παραγωγικότητας Wizergos.


### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Στόχος αυτής της ενότητας είναι η ενεργοποίηση Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης σε λογισμικό παραγωγικότητας Wizergos.
    
   ![Εκχώρηση χρήστη][200]

**Για να αντιστοιχίσετε Britta Simon λογισμικό παραγωγικότητας Wizergos, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.
    
    ![Εκχώρηση χρήστη][201]

2. Στη λίστα εφαρμογών, επιλέξτε το **Λογισμικό Wizergos την παραγωγικότητα**.
    
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)

1. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.
    
    ![Εκχώρηση χρήστη][203]

1. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

2. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.
    
    ![Εκχώρηση χρήστη][205]



### <a name="testing-single-sign-on"></a>Δοκιμές καθολικής σύνδεσης

Είναι ο στόχος αυτής της ενότητας για να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.
 
Όταν κάνετε κλικ στο πλακίδιο λογισμικό παραγωγικότητας Wizergos στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε στην εφαρμογή σας λογισμικό παραγωγικότητας Wizergos.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
