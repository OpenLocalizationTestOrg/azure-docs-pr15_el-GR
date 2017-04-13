<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Ceridian Dayforce HCM | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καθολικής σύνδεσης μεταξύ Azure Active Directory και Ceridian Dayforce HCM."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Ceridian Dayforce HCM

Στόχος αυτού του προγράμματος εκμάθησης είναι ώστε να σας δείξουν πώς μπορείτε να ενοποιήσετε Ceridian Dayforce HCM με Azure Active Directory (Azure AD).  
Ενοποίηση Ceridian Dayforce HCM με Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα:

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση σε Ceridian Dayforce HCM
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να συνδεθεί στην HCM Dayforce Ceridian (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure


Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ρυθμίσετε τις παραμέτρους ενοποίησης Azure AD με Ceridian Dayforce HCM, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Μια Ceridian Dayforce HCM καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Σενάριο περιγραφή
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύριο μπλοκ δόμησης:

1. Προσθήκη Ceridian Dayforce HCM από τη συλλογή
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Προσθήκη Ceridian Dayforce HCM από τη συλλογή
Για να ρυθμίσετε την ενσωμάτωση των Ceridian Dayforce HCM στην Azure AD, πρέπει να προσθέσετε Ceridian Dayforce HCM από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε Ceridian Dayforce HCM από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**. 

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **Ceridian Dayforce HCM**.

    ![Εφαρμογές](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **Ceridian Dayforce HCM**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Εφαρμογές](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Ο στόχος αυτής της ενότητας είναι να σας δείξουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με Ceridian Dayforce HCM που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε ποιος είναι ο χρήστης αντίστοιχο στο Ceridian Dayforce HCM σε έναν χρήστη στο Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στο Ceridian Dayforce HCM πρέπει να καθοριστούν.

Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με Ceridian Dayforce HCM, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
4. **[Δημιουργία μιας HCM Dayforce Ceridian δοκιμή χρήστη](#creating-a-ceridian-dayforce-hcm-test-user)** - έχουν αντίστοιχο του Britta Simon στο HCM Dayforce Ceridian που είναι συνδεδεμένο με το Azure AD αναπαριστάται με εκείνη.
5. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Στόχος αυτής της ενότητας είναι για να ενεργοποιήσετε Azure AD καθολικής σύνδεσης στην πύλη του Azure κλασική και για τη ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή σας Ceridian Dayforce HCM.

Η εφαρμογή σας Ceridian Dayforce HCM αναμένει το διεκδικήσεων SAML σε συγκεκριμένη μορφή. Λειτουργούν επικοινωνήστε με την ομάδα Dayforce HCM πρώτα, για να προσδιορίσετε το αναγνωριστικό χρήστη σωστά. Η Microsoft συνιστά χρησιμοποιώντας το χαρακτηριστικό **"όνομα"** ως αναγνωριστικό χρήστη. Μπορείτε να διαχειριστείτε την τιμή του χαρακτηριστικού αυτού στο παράθυρο διαλόγου **"Atrribute"** . Το παρακάτω στιγμιότυπο οθόνης εμφανίζει ένα παράδειγμα για αυτό. 

![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png) 

**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με Ceridian Dayforce HCM, ακολουθήστε τα παρακάτω βήματα:**




1. Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Ceridian Dayforce HCM** , στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χαρακτηριστικά**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png) 


1. Στη λίστα **διακριτικού χαρακτηριστικά saml** χαρακτηριστικά, επιλέξτε το χαρακτηριστικό όνομα και, στη συνέχεια, κάντε κλικ στην επιλογή **Επεξεργασία**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png) 


1. Στο παράθυρο διαλόγου **Επεξεργασία χαρακτηριστικό χρήστη** , εκτελέστε τα ακόλουθα βήματα:
 
    μια. Από την **Τιμή του χαρακτηριστικού** λίστα, επιλέξτε το χαρακτηριστικό χρήστη που θέλετε να χρησιμοποιήσετε για την εφαρμογή.  
    Για παράδειγμα, εάν θέλετε να χρησιμοποιήσετε το EmployeeID ως χρήστης μοναδικό αναγνωριστικό και που έχετε αποθηκεύσει την τιμή του χαρακτηριστικού στο το ExtensionAttribute2, στη συνέχεια, επιλέξτε **user.extensionattribute2**. 

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.  
    



1. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **Γρήγορης εκκίνησης**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  





1. Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Ceridian Dayforce HCM** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6] 

2. Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Ceridian Dayforce HCM** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png) 

3. Στη σελίδα **Ρύθμιση παραμέτρων των ρυθμίσεων της εφαρμογής** παραθύρου διαλόγου, ακολουθήστε τα παρακάτω βήματα:.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png) 


    μια. Στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από τους χρήστες σας να καθολικής σύνδεσης στην εφαρμογή σας Ceridian Dayforce HCM. Για περιβάλλοντα παραγωγής, χρησιμοποιήστε την παρακάτω μορφή διεύθυνσης URL:`https://sso.dayforcehcm.com/DayforcehcmNamespace`

    Για σαφήνεια, αντικαταστήστε DayforcehcmNamespace με το χώρο ονομάτων του περιβάλλοντός σας ή το Αναγνωριστικό εταιρείας; Για παράδειγμα:`https://sso.dayforcehcm.com/contoso`
    
    Για περιβάλλοντα δοκιμής, χρησιμοποιήστε την παρακάτω μορφή διεύθυνσης URL:`https://ssotest.dayforcehcm.com/DayforcehcmNamespace` 

    β. Στο πλαίσιο κειμένου **Διεύθυνση URL απάντηση** , πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από Azure AD για να δημοσιεύσετε την απάντηση.  
    Για περιβάλλοντα παραγωγής, χρησιμοποιήστε:`https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2`  
    Για περιβάλλοντα δοκιμής, χρησιμοποιήστε:`https://fs-test.dayforcehcm.com/sp/ACS.saml2`  
   

4. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Ceridian Dayforce HCM** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png) 

    μια. Κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο στον υπολογιστή σας.

    β. Κάντε κλικ στο κουμπί **Επόμενο**.


5. Για να λάβετε SSO έχει ρυθμιστεί για την εφαρμογή σας, επικοινωνήστε με την ομάδα υποστήριξης Ceridian Dayforce HCM μέσω ηλεκτρονικού ταχυδρομείου και δώστε τους με τα εξής:

    - Αρχείο πιστοποιητικού που έχουν ληφθεί
    - Η **διεύθυνση URL εκδότη**
    - Η **διεύθυνση URL SAML SSO** 
    - **Μία έξοδος διεύθυνση URL της υπηρεσίας** 


6. Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Azure AD καθολικής σύνδεσης][10]

7. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  

    ![Azure AD καθολικής σύνδεσης][11]



### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Στόχος αυτής της ενότητας είναι να δημιουργήσετε έναν χρήστη δοκιμής στην πύλη Azure κλασική που ονομάζεται Britta Simon.

![Δημιουργία Azure AD χρήστη][20]

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png) 

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png) 

4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png) 

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png) 

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png) 

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png) 

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png) 

    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   



### <a name="creating-a-ceridian-dayforce-hcm-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Ceridian Dayforce HCM

Στόχος αυτής της ενότητας είναι να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο Ceridian Dayforce HCM. 

Εργασία επικοινωνήστε με την ομάδα υποστήριξης του Ceridian Dayforce HCM για να προστεθεί σε χρήστες σας στην εφαρμογή Ceridian Dayforce HCM. 





### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Στόχος αυτής της ενότητας είναι η ενεργοποίηση Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης σε Ceridian Dayforce HCM.

![Εκχώρηση χρήστη][200] 

**Για να αντιστοιχίσετε Britta Simon Ceridian Dayforce HCM, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική Azure πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εκχώρηση χρήστη][201] 

2. Στη λίστα εφαρμογών, επιλέξτε **Ceridian Dayforce HCM**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png) 

1. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Εκχώρηση χρήστη][203] 

1. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

2. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Εκχώρηση χρήστη][205]



### <a name="testing-single-sign-on"></a>Δοκιμές καθολικής σύνδεσης

Είναι ο στόχος αυτής της ενότητας για να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.  
Όταν κάνετε κλικ στο πλακίδιο Ceridian Dayforce HCM στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε στην εφαρμογή σας Ceridian Dayforce HCM.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png
