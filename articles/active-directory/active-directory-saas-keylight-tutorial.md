<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Keylight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καθολικής σύνδεσης μεταξύ Azure Active Directory και Keylight."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-keylight"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Keylight

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς μπορείτε να ενοποιήσετε Keylight με Azure Active Directory (Azure AD).

Ενοποίηση Keylight με Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα:

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση σε Keylight
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να συνδεθεί στην Keylight (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ρυθμίσετε τις παραμέτρους ενοποίησης Azure AD με Keylight, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure
- Μια Keylight καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Σενάριο περιγραφή
Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής. 

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύριο μπλοκ δόμησης:

1. Προσθήκη Keylight από τη συλλογή
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-keylight-from-the-gallery"></a>Προσθήκη Keylight από τη συλλογή
Για να ρυθμίσετε την ενσωμάτωση των Keylight στην Azure AD, πρέπει να προσθέσετε Keylight από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε Keylight από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**. 

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **Keylight**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_01.png)

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **Keylight**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Σε αυτήν την ενότητα, μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με Keylight που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με Keylight, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
4. **[Δημιουργία μιας Keylight δοκιμή χρήστη](#creating-a-keylight-test-user)** - έχουν αντίστοιχο του Britta Simon στο Keylight που είναι συνδεδεμένο με το Azure AD αναπαριστάται με εκείνη.
5. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Σε αυτήν την ενότητα, ενεργοποιείτε Azure AD καθολικής σύνδεσης στην πύλη του Azure κλασική και ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή σας Keylight.


**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με Keylight, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Keylight** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6] 


2. Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Keylight** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_03.png) 

3. Στη σελίδα του παραθύρου διαλόγου **Διαμόρφωση παραμέτρων ρυθμίσεων εφαρμογής** , εκτελέστε τα ακόλουθα βήματα:
 
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_04.png) 


    μια. Στο πλαίσιο κειμένου εισόδου στη διεύθυνση URL, πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από τους χρήστες σας να καθολικής σύνδεσης στην εφαρμογή σας Keylight χρησιμοποιώντας το ακόλουθο μοτίβο: **"https://\<το όνομα της εταιρείας\>.keylightgrc.com/Login.aspx?saml=1"**.


4. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Keylight** , εκτελέστε τα ακόλουθα βήματα:
 
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_05.png) 

    μια. Κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο στον υπολογιστή σας.

    β. Κάντε κλικ στο κουμπί **Επόμενο**.


5. Για να ενεργοποιήσετε SSO στο Keylight, ακολουθήστε τα παρακάτω βήματα:
 
    μια. Καθολικής σύνδεσης στο λογαριασμό σας Keylight ως διαχειριστής.

    β. Στο μενού στο επάνω μέρος, κάντε κλικ στο **άτομο**και επιλέξτε **Το πρόγραμμα εγκατάστασης Keylight**.
       
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-keylight-tutorial/401.png) 

    c. Στο treeview στα αριστερά, κάντε κλικ στην επιλογή **SAML**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. Στο παράθυρο διαλόγου **Ρυθμίσεις SAML** , κάντε κλικ στην επιλογή **Επεξεργασία**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-keylight-tutorial/404.png) 
  

5. Στη σελίδα του παραθύρου διαλόγου **Επεξεργασία ρυθμίσεων SAML** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-keylight-tutorial/405.png) 

    μια. Ορίστε **τον έλεγχο ταυτότητας SAML** σε **ενεργό**.


    β. Στην κλασική πύλη Azure AD, αντιγράψτε την τιμή **Διεύθυνσης URL SSO SAML** και, στη συνέχεια, επικολλήστε την στο πλαίσιο κειμένου **Διεύθυνση URL σύνδεσης υπηρεσίας παροχής ταυτότητας** .

    c. Στην κλασική πύλη Azure AD, αντιγράψτε την τιμή **Μία διεύθυνση URL της υπηρεσίας Sign-Out** και, στη συνέχεια, επικολλήστε την στο πλαίσιο κειμένου **Διεύθυνση URL αποσυνδεθείτε υπηρεσία παροχής ταυτότητας** .

    d. Κάντε κλικ στην επιλογή **Επιλογή αρχείου** για να επιλέξετε το πιστοποιητικό Keylight που έχουν ληφθεί και, στη συνέχεια, κάντε κλικ στην επιλογή **Άνοιγμα** για να αποστείλετε το πιστοποιητικό.


    ε. Ορίστε την **τοποθεσία αναγνωριστικό χρήστη SAML** **NameIdentifier**στοιχείο της πρότασης θέμα.
   
    f. Παροχή του **Keylight υπηρεσία παροχής χρησιμοποιώντας το ακόλουθο μοτίβο: **https://&lt;το όνομα της εταιρείας&gt;. keylightgrc.com**.

    γ. Ορισμός **αυτόματης προμήθεια στους χρήστες** σε **ενεργό**.

    h. Ορίστε τον **τύπο λογαριασμού αυτόματης διάταξη** **Πλήρους**χρήστη.

    να. Με **ρόλο ασφαλείας Αυτόματης παροχής**, επιλέξτε **Τυπικό χρήστη με SAML**.
   
    j. Ως **παροχής αυτόματης ρύθμισης παραμέτρων ασφαλείας**, επιλέξτε **Τυπική ρύθμιση παραμέτρων του χρήστη**.
   
    k. Στο πλαίσιο κειμένου χαρακτηριστικό ηλεκτρονικού ταχυδρομείου, πληκτρολογήστε **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    l. Στο πλαίσιο κειμένου **όνομα χαρακτηριστικού** , πληκτρολογήστε **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    m. Στο πλαίσιο κειμένου **τελευταία χαρακτηριστικό όνομα** , πληκτρολογήστε **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    n. Κάντε κλικ στην επιλογή **Αποθήκευση**.
   
  
   
  
6. Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Azure AD καθολικής σύνδεσης][10]

7. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  

    ![Azure AD καθολικής σύνδεσης][11]




### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη δοκιμής στην κλασική Azure πύλη που ονομάζεται Britta Simon.

Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

![Δημιουργία Azure AD χρήστη][20]



**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_09.png) 


2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 


4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_05.png) 

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_06.png) 

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_07.png) 

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_08.png) 

    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   



### <a name="creating-a-keylight-test-user"></a>Δημιουργία μιας Keylight δοκιμής χρήστη

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο Keylight. Keylight υποστηρίζει μόνο σε δεδομένη χρονική προμήθεια, που είναι ενεργοποιημένη από προεπιλογή.

Δεν υπάρχει κανένα στοιχείο ενέργειας για εσάς σε αυτήν την ενότητα. Κατά την πρόσβαση σε Keylight εάν ο χρήστης δεν υπάρχει ακόμα, δημιουργείται ένας νέος χρήστης. 

> [AZURE.NOTE] Εάν πρέπει να δημιουργήσετε έναν νέο χρήστη με μη αυτόματο τρόπο, πρέπει να επικοινωνήσετε με την ομάδα υποστήριξης του Keylight.


### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Σε αυτήν την ενότητα, μπορείτε να ενεργοποιήσετε Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης σε Keylight.

![Εκχώρηση χρήστη][200] 

**Για να αντιστοιχίσετε Britta Simon Keylight, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική Azure πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εκχώρηση χρήστη][201] 

2. Στη λίστα εφαρμογών, επιλέξτε **Keylight**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_50.png) 

1. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Εκχώρηση χρήστη][203] 

1. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

2. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Εκχώρηση χρήστη][205]



### <a name="testing-single-sign-on"></a>Δοκιμές καθολικής σύνδεσης

Σε αυτήν την ενότητα, μπορείτε να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.

Όταν κάνετε κλικ στο πλακίδιο Keylight στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε στην εφαρμογή σας Keylight.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_205.png
