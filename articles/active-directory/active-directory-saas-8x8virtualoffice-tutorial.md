<properties
    pageTitle="Πρόγραμμα εκμάθησης: Azure Active Directory ενοποίηση με το Office εικονικές 8 x 8 | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καθολικής σύνδεσης μεταξύ Azure Active καταλόγου και 8 x 8 εικονικού του Office."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Πρόγραμμα εκμάθησης: Azure Active Directory ενοποίηση με το Office εικονικές 8 x 8

Ο στόχος αυτού του προγράμματος εκμάθησης είναι να σας δείξουν πώς μπορείτε να ενοποιήσετε το Office εικονικές 8 x 8 με το Azure Active Directory (Azure AD).

Ενοποίηση 8 x 8 εικονικό Office με το Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα:

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση στο Office εικονικές 8 x 8
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να συνδεθεί στην Office εικονικές 8 x 8 (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ρυθμίσετε τις παραμέτρους Azure AD ενοποίηση με το Office εικονικές 8 x 8, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Μια 8 x 8 εικονικού Office καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Περιγραφή σενάριο
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να ελέγξετε Microsoft Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής.

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύρια δομικά στοιχεία:

1. Προσθήκη 8 x 8 εικονικές Office από τη συλλογή
2. Ρύθμιση παραμέτρων και έλεγχος Microsoft Azure AD καθολικής σύνδεσης


## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Προσθήκη 8 x 8 εικονικές Office από τη συλλογή
Για να ρυθμίσετε την ενοποίηση του Office εικονικές 8 x 8 σε Azure AD, πρέπει να προσθέσετε 8 x 8 εικονικές Office από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε 8 x 8 εικονικές Office από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.
    
    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **8 x 8 εικονικές Office**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_01.png)
7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **Office εικονικές 8 x 8**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Επιλογή της εφαρμογής στη συλλογή](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_0001.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Microsoft Azure AD καθολικής σύνδεσης
Ο στόχος αυτής της ενότητας είναι να σας δείξουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Microsoft Azure AD καθολικής σύνδεσης με 8 x 8 εικονικού Office που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε τι το αντίστοιχο χρήστη στο εικονικού το Office σε έναν χρήστη στο Azure AD είναι 8 x 8. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στο 8 x 8 εικονικού Office πρέπει να δημιουργηθεί.

Αυτή η σχέση σύνδεση είναι εγκατεστημένος κατά την αντιστοίχιση της τιμής του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** στο Office εικονικές 8 x 8.

Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Microsoft Azure AD καθολικής σύνδεσης με το Office εικονικές 8 x 8, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Microsoft Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Microsoft Azure AD καθολικής σύνδεσης με Britta Simon.
3. **[Δημιουργία χρήστη δοκιμής εικονικού Office 8 x 8](#creating-a-8x8-virtual-office-test-user)** - να υπάρχει ένα αντίστοιχο των Britta Simon στο Office εικονικές 8 x 8 που είναι συνδεδεμένο με το Azure AD αναπαράσταση εκείνη.
4. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε το Microsoft Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Microsoft Azure AD καθολικής σύνδεσης

Σε αυτήν την ενότητα, ενεργοποιείτε Microsoft Azure AD καθολικής σύνδεσης στην πύλη του κλασική και ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή Office εικονικό 8 x 8.

**Για να ρυθμίσετε τις παραμέτρους της Microsoft Azure AD καθολικής σύνδεσης με το Office εικονικές 8 x 8, εκτελέστε τα ακόλουθα βήματα:**

1. Στην κλασική πύλη, στη σελίδα ενοποίησης εφαρμογής **Office εικονικές 8 x 8** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .
     
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6] 

2. Στη σελίδα **Πώς θέλετε οι χρήστες για να συνδεθείτε στο Office εικονικές 8 x 8** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_03.png) 

3. Στη σελίδα **Ρύθμιση παραμέτρων των ρυθμίσεων της εφαρμογής** παραθύρου διαλόγου, ακολουθήστε τα παρακάτω βήματα και κάντε κλικ στην επιλογή **Επόμενο**:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_04.png)

    μια. Στο πλαίσιο κειμένου **Διεύθυνση URL απάντηση** , πληκτρολογήστε:`https://sso.8x8.com/saml2`

    β. Κάντε κλικ στο κουμπί **Επόμενο**

4. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Office εικονικές 8 x 8** , εκτελέστε τα ακόλουθα βήματα και κάντε κλικ στην επιλογή **Επόμενο**:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_05.png)

    μια. Κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο στον υπολογιστή σας.

    β. Κάντε κλικ στο κουμπί **Επόμενο**.

5. Καθολικής σύνδεσης στο μισθωτή του Office εικονικού 8 x 8 ως διαχειριστής.
6. Επιλέξτε **εικονικό Office Mgr λογαριασμού** στην εφαρμογή του πίνακα.

    ![Ρύθμιση παραμέτρων στην πλευρά εφαρμογής](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

7. Επιλέξτε λογαριασμό για **επιχειρήσεις** για τη διαχείριση και κάντε κλικ στο κουμπί **Είσοδος** .

    ![Ρύθμιση παραμέτρων στην πλευρά εφαρμογής](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

8. Κάντε κλικ στην καρτέλα **Λογαριασμοί** στη λίστα μενού.

    ![Ρύθμιση παραμέτρων στην πλευρά εφαρμογής](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

9. Κάντε κλικ στην επιλογή **Καθολικής σύνδεσης** στη λίστα των λογαριασμών.

    ![Ρύθμιση παραμέτρων στην πλευρά εφαρμογής](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

10. Επιλέξτε **Signle εισόδου στην** στην περιοχή μεθόδου ελέγχου ταυτότητας και κάντε κλικ στην επιλογή **SAML**.

    ![Ρύθμιση παραμέτρων στην πλευρά εφαρμογής](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

11. Αντιγράψτε **SAML SSO διεύθυνση URL**, **μίας κάλαντα ανάληψη διεύθυνση URL της υπηρεσίας** και τη **διεύθυνση URL εκδότη** από Azure AD για να **συνδεθείτε στη διεύθυνση URL**, **Έξοδος διεύθυνση URL** και **εκδότη διεύθυνση URL** στο Office εικονικού 8 x 8. 

    ![Ρύθμιση παραμέτρων στην πλευρά εφαρμογής](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    ![Ρύθμιση παραμέτρων στην πλευρά εφαρμογής](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_007.png)

12. Κάντε κλικ στο κουμπί **περιήγησης** για να αποστείλετε το πιστοποιητικό το οποίο έχετε κάνει λήψη του από το Azure AD.

13. Κάντε κλικ στο κουμπί **Αποθήκευση** .

14. Στην κλασική πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Azure AD καθολικής σύνδεσης][10]

15. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  

    ![Azure AD καθολικής σύνδεσης][11]



### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Στόχος αυτής της ενότητας είναι να δημιουργήσετε έναν χρήστη δοκιμής στην κλασική πύλη που ονομάζεται Britta Simon.
    
![Δημιουργία Azure AD χρήστη][20]

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_09.png)

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png)

4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png)

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_05.png)

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα:
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_06.png)

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα παράθυρο διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_07.png)

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_08.png)

    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   



### <a name="creating-a-8x8-virtual-office-test-user"></a>Δημιουργία χρήστη 8 x 8 εικονικού Office δοκιμής

Στόχος αυτής της ενότητας είναι να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο Office εικονικές 8 x 8. 8 x 8 εικονικό Office υποστηρίζει προμήθεια απλώς στιγμή, που είναι από προεπιλογή ενεργοποιημένη.

Δεν υπάρχει κανένα στοιχείο ενέργειας για εσάς σε αυτήν την ενότητα. Θα δημιουργηθεί ένα νέο χρήστη κατά την προσπάθεια πρόσβασης Office εικονικές 8 x 8, εάν δεν υπάρχει ακόμα. 

> [AZURE.NOTE] Εάν χρειάζεστε για να δημιουργήσετε έναν χρήστη με μη αυτόματο τρόπο, πρέπει να επικοινωνήσετε με την ομάδα υποστήριξης του Office εικονικού 8 x 8.


### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Στόχος αυτής της ενότητας είναι η ενεργοποίηση Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης στο Office εικονικές 8 x 8.
    
![Εκχώρηση χρήστη][200]

**Για να εκχωρήσετε Britta Simon Office εικονικές 8 x 8, εκτελέστε τα ακόλουθα βήματα:**

1. Στην κλασική πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εκχώρηση χρήστη][201]

2. Στη λίστα εφαρμογών, επιλέξτε **Office εικονικές 8 x 8**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_50.png)

3. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.
    
    ![Εκχώρηση χρήστη][203]

4. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

5. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Εκχώρηση χρήστη][205]



### <a name="testing-single-sign-on"></a>Δοκιμές καθολικής σύνδεσης

Είναι ο στόχος αυτής της ενότητας για να ελέγξετε το Microsoft Azure AD καθολικής σύνδεσης ρύθμισης παραμέτρων χρησιμοποιώντας τον πίνακα της Access.

Όταν κάνετε κλικ στο πλακίδιο του Office εικονικού 8 x 8 στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε στην εφαρμογή του Office εικονικού 8 x 8.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_205.png
