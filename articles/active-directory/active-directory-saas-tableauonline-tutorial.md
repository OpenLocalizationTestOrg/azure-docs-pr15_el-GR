<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Tableau Online | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καθολικής σύνδεσης μεταξύ Azure Active Directory και Tableau Online."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Tableau Online

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς μπορείτε να ενοποιήσετε Tableau Online με το Azure Active Directory (Azure AD).

Ενοποίηση Tableau Online με το Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα:

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση σε Tableau Online
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να συνδεθεί στην Online Tableau (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ρυθμίσετε τις παραμέτρους Azure AD ενοποίηση με το Tableau Online, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Μια **Ηλεκτρονική Tableau** καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Σενάριο περιγραφή
Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής. Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύριο μπλοκ δόμησης:

1. Προσθήκη Tableau Online από τη συλλογή
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-tableau-online-from-the-gallery"></a>Προσθήκη Tableau Online από τη συλλογή
Για να ρυθμίσετε την ενσωμάτωση του Tableau Online στην Azure AD, πρέπει να προσθέσετε Tableau Online από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε Tableau Online από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**. 

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **Tableau Online**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_01.png)

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **Tableau Online**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Σε αυτήν την ενότητα, μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με το Online Tableau που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε ποιος είναι ο χρήστης αντίστοιχο στο Tableau Online σε ένα χρήστη στο Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στο Tableau Online πρέπει να καθοριστούν.
Αυτή η σχέση σύνδεση είναι εγκατεστημένος κατά την αντιστοίχιση της τιμής του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** στο Tableau Online.

Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με Tableau Online, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
4. **[Τη δημιουργία μιας Online Tableau δοκιμή χρήστη](#creating-a-Tableau-Online-test-user)** - έχουν αντίστοιχο του Britta Simon στο Online Tableau που είναι συνδεδεμένο με το Azure AD αναπαράσταση εκείνη.
5. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Στόχος αυτής της ενότητας είναι για να ενεργοποιήσετε Azure AD καθολικής σύνδεσης στην πύλη του Azure κλασική και για τη ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή σας Tableau Online.

**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με το Tableau Online, εκτελέστε τα ακόλουθα βήματα:**

1. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **Γρήγορης εκκίνησης**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6]
2. Στην κλασική πύλη, στη σελίδα του **Tableau Online** ενοποίηση εφαρμογής, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][7] 

3. Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο ηλεκτρονική Tableau** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
    
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_06.png)

4. Στη σελίδα του παραθύρου διαλόγου **Διαμόρφωση παραμέτρων ρυθμίσεων εφαρμογής** , εκτελέστε τα ακόλουθα βήματα: 

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_07.png)


    μια. Στο πλαίσιο κειμένου εισόδου στη διεύθυνση URL, πληκτρολογήστε μια διεύθυνση URL με το ακόλουθο μοτίβο:`https://sso.online.tableau.com`

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

5. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Tableau Online** , κάντε κλικ στην επιλογή **Λήψη μετα-δεδομένων**, και, στη συνέχεια, αποθηκεύστε το αρχείο στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_08.png)

6. Επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
    
    ![Azure AD καθολικής σύνδεσης][10]

7. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  
    
    ![Azure AD καθολικής σύνδεσης][11]
8. Σε ένα παράθυρο προγράμματος περιήγησης διαφορετικά, καθολικής σύνδεσης στην εφαρμογή σας Tableau Online. Μεταβείτε στις **Ρυθμίσεις** και, στη συνέχεια, **τον έλεγχο ταυτότητας**

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)

9. Στην ενότητα **Τύποι ελέγχου ταυτότητας** . Επιλέξτε το πλαίσιο **καθολικής σύνδεσης με SAML** ελέγχου για να ενεργοποιήσετε την SAML.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

10. Κάντε κύλιση μέχρι την ενότητα **Εισαγωγή αρχείου μετα-δεδομένων σε Tableau Online** .  Κάντε κλικ στο κουμπί Αναζήτηση και εισαγάγετε το αρχείο μετα-δεδομένων που έχετε λάβει από το Azure AD. Στη συνέχεια, κάντε κλικ στο κουμπί **εφαρμογή**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

11. Στην ενότητα **διεκδικήσεων Match** , εισαγάγετε το αντίστοιχο όνομα διεκδίκηση υπηρεσία παροχής ταυτότητας για τη διεύθυνση ηλεκτρονικού ταχυδρομείου, όνομα και επώνυμο. Για να λάβετε αυτές τις πληροφορίες από το Azure AD:

    μια. Επιστρέψτε στο Azure AD. Στην κλασική πύλη Azure, σε **Tableau Online** τη σελίδα ενοποίησης της εφαρμογής, στο μενού στην κορυφή, κάντε κλικ στην επιλογή **χαρακτηριστικά**. Αντιγράψτε το όνομα για τις τιμές: userprincipalname, givenname και "Επώνυμο".
     
    ![Azure AD καθολικής σύνδεσης](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)

    β. Μεταβείτε στην εφαρμογή Tableau Online και, στη συνέχεια, ορίστε την ενότητα **Χαρακτηριστικά Online Tableau** ως εξής:
    
    -  Διεύθυνση ηλεκτρονικού ταχυδρομείου: **Αλληλογραφία** ή **userprincipalname**
    -  Όνομα: **givenname**
    -  Επώνυμο: **"Επώνυμο"**

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη δοκιμής στην κλασική πύλη που ονομάζεται Britta Simon.

![Δημιουργία Azure AD χρήστη][20]

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_09.png) 

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα:
 
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_05.png) 

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_06.png) 

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_07.png) 

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_08.png) 

    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   



### <a name="creating-a-tableau-online-test-user"></a>Δημιουργία Tableau Online δοκιμής χρήστη

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο Tableau Online.

1. Στο **Tableau Online**, κάντε κλικ στην επιλογή **Ρυθμίσεις** και, στη συνέχεια, την ενότητα **ελέγχου ταυτότητας** . Κάντε κύλιση προς τα κάτω ενότητα **Επιλογή χρηστών** . Κάντε κλικ στην επιλογή **Προσθήκη χρηστών** και, στη συνέχεια, να **εισαγάγετε διευθύνσεις ηλεκτρονικού ταχυδρομείου**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Επιλέξτε **Προσθήκη χρηστών για καθολική σύνδεση (SSO) έλεγχο ταυτότητας**. Στο πλαίσιο κειμένου **Πληκτρολογήστε τις διευθύνσεις ηλεκτρονικού ταχυδρομείου** Προσθήκηbritta.simon@contoso.com

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)

3.  Κάντε κλικ στην επιλογή **Δημιουργία**.


### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Σε αυτήν την ενότητα, μπορείτε να ενεργοποιήσετε Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης σε Tableau Online.

![Εκχώρηση χρήστη][200] 

**Για να αντιστοιχίσετε Britta Simon Tableau Online, εκτελέστε τα ακόλουθα βήματα:**

1. Στην κλασική πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εκχώρηση χρήστη][201] 

3. Στη λίστα εφαρμογών, επιλέξτε **Tableau Online**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_50.png) 

4. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Εκχώρηση χρήστη][203] 

5. Στη λίστα όλοι οι χρήστες, επιλέξτε **Britta Simon**.

6. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Εκχώρηση χρήστη][205]


### <a name="testing-single-sign-on"></a>Δοκιμές καθολικής σύνδεσης

Είναι ο στόχος αυτής της ενότητας για να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.

Όταν κάνετε κλικ στο πλακίδιο Tableau Online στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα υπογραφή στην Tableau Online εφαρμογή σας.

## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_205.png
