<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με μαυροπίνακα μάθετε - Shibboleth | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καθολικής σύνδεσης μεταξύ Azure Active Directory και μάθετε Μαυροπίνακας - Shibboleth."
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
    ms.date="09/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn---shibboleth"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με μαυροπίνακα μάθετε - Shibboleth

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς μπορείτε να ενοποιήσετε μαυροπίνακα μάθετε - Shibboleth με το Azure Active Directory (Azure AD).

Ενοποίηση μάθετε Μαυροπίνακας - Shibboleth με Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα:

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση για να μάθετε Μαυροπίνακας - Shibboleth
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να πραγματοποιήσει-σε για να μάθετε Μαυροπίνακας - Shibboleth (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ρυθμίσετε τις παραμέτρους ενοποίησης Azure AD με μαυροπίνακα μάθετε - Shibboleth, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Μια μάθετε Μαυροπίνακας - Shibboleth καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Σενάριο περιγραφή
Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής.

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύριο μπλοκ δόμησης:

1. Προσθήκη μάθετε Μαυροπίνακας - Shibboleth από τη συλλογή
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-blackboard-learn---shibboleth-from-the-gallery"></a>Προσθήκη μάθετε Μαυροπίνακας - Shibboleth από τη συλλογή
Για να ρυθμίσετε την ενοποίηση του μαυροπίνακα μάθετε - Shibboleth σε Azure AD, πρέπει να προσθέσετε μαυροπίνακα μάθετε - Shibboleth από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογές.

**Για να προσθέσετε μαυροπίνακα μάθετε - Shibboleth από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory][1]
2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **Μαυροπίνακα μάθετε - Shibboleth**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_01.png)

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **Μάθετε Μαυροπίνακας - Shibboleth**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Σε αυτήν την ενότητα, μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με μαυροπίνακα μάθετε - Shibboleth που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε τι το αντίστοιχο χρήστη στο μάθετε Μαυροπίνακας - Shibboleth είναι σε ένα χρήστη στο Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στο μαυροπίνακα μάθετε - Shibboleth πρέπει να δημιουργηθεί.

Αυτή η σχέση σύνδεση είναι εγκατεστημένος κατά την αντιστοίχιση της τιμής του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** στο μαυροπίνακα μάθετε - Shibboleth.

Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με μαυροπίνακα μάθετε - Shibboleth, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
3. **[Δημιουργία μιας μαυροπίνακα μάθετε - Shibboleth δοκιμής χρήστη](#creating-a-blackboard-learn-shibboleth-test-user)** - να υπάρχει ένα αντίστοιχο των Britta Simon στο μαυροπίνακα μάθετε - Shibboleth που είναι συνδεδεμένο με το Azure AD αναπαράσταση εκείνη.
4. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Σε αυτήν την ενότητα, ενεργοποιείτε Azure AD καθολικής σύνδεσης στην πύλη του κλασική και ρύθμιση παραμέτρων Καθολικής σύνδεσης σε σας μαυροπίνακα μάθετε - Shibboleth εφαρμογής.


**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με μαυροπίνακα μάθετε - Shibboleth, εκτελέστε τα ακόλουθα βήματα:**

1. Στην κλασική πύλη, στη **Μαυροπίνακα μάθετε - Shibboleth** ενοποίησης σελίδα εφαρμογής, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .
     
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6] 

2. Στη σελίδα **Πώς θα θέλατε χρήστες να πραγματοποιούν είσοδο για να μάθετε Μαυροπίνακας - Shibboleth** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_03.png) 

3. Στη σελίδα του παραθύρου διαλόγου **Διαμόρφωση παραμέτρων ρυθμίσεων εφαρμογής** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_04.png) 

    μια. Στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από τους χρήστες σας να καθολικής σύνδεσης για να σας μαυροπίνακα μάθετε - εφαρμογής Shibboleth χρησιμοποιώντας το ακόλουθο μοτίβο: **https://\<yourblackoardlearnserver\>.blackboardlearn.com/Shibboleth.sso/Login**
    
    β. Στο πλαίσιο κειμένου " **αναγνωριστικό** ", πληκτρολογήστε τη διεύθυνση URL με το ακόλουθο μοτίβο: **https://\<yourblackoardlearnserver\>.blackboardlearn.com/shibboleth-sp**

    c. Στο πλαίσιο κειμένου **Διεύθυνση URL απάντηση** , πληκτρολογήστε τη διεύθυνση URL με το ακόλουθο μοτίβο: **https://\<yourblackoardlearnserver\>.blackboardlearn.com/Shibboleth.sso/SAML2/POST**

    > [AZURE.NOTE] Θα μπορείτε να βρείτε όλες αυτές τις τιμές σε το doucument Ομοσπονδία μετα-δεδομένων που παρέχονται από το συνεργάτη μαυροπίνακα μάθετε.

    d. Κάντε κλικ στο κουμπί **Επόμενο**
 
4. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο μαυροπίνακα μάθετε - Shibboleth** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_05.png)

    μια. Κάντε κλικ στην επιλογή **Λήψη μετα-δεδομένων**και, στη συνέχεια, αποθηκεύστε το αρχείο στον υπολογιστή σας.

    β. Κάντε κλικ στο κουμπί **Επόμενο**.


5. Για να λάβετε SSO έχει ρυθμιστεί για την εφαρμογή σας, επικοινωνήστε με το μαυροπίνακα μάθετε - συνεργάτη Shibboleth και δώστε τους τα εξής:

    • Τα έχετε λάβει **μετα-δεδομένων**

    • Τη **διεύθυνση URL εκδότη**

    • Το **SAML SSO διεύθυνσης URL**

    • Τη **διεύθυνση URL της υπηρεσίας καθολικής Sign-out**

6. Στην κλασική πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
    
    ![Azure AD καθολικής σύνδεσης][10]

7. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  
 
    ![Azure AD καθολικής σύνδεσης][11]


### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη δοκιμής στην κλασική πύλη που ονομάζεται Britta Simon.


![Δημιουργία Azure AD χρήστη][20]

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_09.png) 

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_03.png) 

4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_04.png) 

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα παρακάτω βήματα:  ![τη δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_05.png) 

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα: ![τη δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_06.png) 

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_07.png) 

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_08.png) 

    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   


### <a name="creating-an-blackboard-learn---shibboleth-test-user"></a>Δημιουργία μιας μαυροπίνακα μάθετε - Shibboleth δοκιμής χρήστη

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο μαυροπίνακα μάθετε - Shibboleth. Εργαστείτε επικοινωνήστε με το συνεργάτη σας μάθετε μαυροπίνακα για να προσθέσετε τους χρήστες στο μαυροπίνακα μάθετε - την πλατφόρμα Shibboleth.


### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Σε αυτήν την ενότητα, μπορείτε να ενεργοποιήσετε Britta Simon για να χρησιμοποιήσετε Azure καθολικής σύνδεσης εκχωρώντας της access για να μάθετε Μαυροπίνακας - Shibboleth.

![Εκχώρηση χρήστη][200] 

**Για να αντιστοιχίσετε Britta Simon για να μάθετε Μαυροπίνακας - Shibboleth, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εκχώρηση χρήστη][201] 

2. Στη λίστα εφαρμογών, επιλέξτε **Μάθετε Μαυροπίνακας - Shibboleth**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearnshibboleth_50.png) 

3. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Εκχώρηση χρήστη][203]

4. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

5. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Εκχώρηση χρήστη][205]


### <a name="testing-single-sign-on"></a>Δοκιμή καθολικής σύνδεσης

Σε αυτήν την ενότητα, μπορείτε να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.

Όταν κάνετε κλικ το μαυροπίνακα μάθετε - Shibboleth πλακίδιο στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε για να σας μαυροπίνακα μάθετε - Shibboleth εφαρμογής.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_205.png