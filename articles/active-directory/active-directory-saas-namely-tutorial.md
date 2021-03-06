<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με συγκεκριμένα | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καθολικής σύνδεσης μεταξύ Azure Active Directory και συγκεκριμένα."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="prasannas"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με συγκεκριμένα

Στόχος αυτού του προγράμματος εκμάθησης είναι ώστε να σας δείξουν πώς μπορείτε να ενοποιήσετε δηλαδή με Azure Active Directory (Azure AD).

Ενοποίηση συγκεκριμένα με Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα: 

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση σε συγκεκριμένα 
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας για να λάβετε αυτόματη υπογραφή στην συγκεκριμένα (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία 

Για να ρυθμίσετε τις παραμέτρους Azure AD ενοποίηση με συγκεκριμένα, μπορείτε πρέπει τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Μια δηλαδή καθολικής σύνδεσης enabled συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Σενάριο περιγραφή
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής. 

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύριο μπλοκ δόμησης:

1. Προσθήκη συγκεκριμένα από τη συλλογή 
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-namely-from-the-gallery"></a>Προσθήκη συγκεκριμένα από τη συλλογή
Για να ρυθμίσετε την ενσωμάτωση των δηλαδή στην Azure AD, πρέπει να προσθέσετε συγκεκριμένα από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε συγκεκριμένα από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**. 

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **συγκεκριμένα**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_01.png)

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **συγκεκριμένα**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Ο στόχος αυτής της ενότητας είναι να σας δείξουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με συγκεκριμένα, με βάση μια δοκιμής χρήστη που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε ποιος είναι ο χρήστης αντίστοιχο στο συγκεκριμένα σε έναν χρήστη στο Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στο δηλαδή πρέπει να δημιουργηθεί.

Αυτή η σχέση σύνδεση είναι εγκατεστημένος αντιστοιχίζοντας την τιμή του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** σε συγκεκριμένα.
 
Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με δηλαδή, που πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
4. **[Τη δημιουργία μιας δηλαδή δοκιμή χρήστη](#creating-a-namely-test-user)** - για να έχουν αντίστοιχο του Britta Simon σε συγκεκριμένα που είναι συνδεδεμένο με το Azure AD αναπαράσταση εκείνη.
5. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Στόχος αυτής της ενότητας είναι για να ενεργοποιήσετε Azure AD καθολικής σύνδεσης στην πύλη του Azure κλασική και για τη ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή σας συγκεκριμένα. 




**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με συγκεκριμένα, εκτελέστε τα παρακάτω βήματα:**

1. Στην κλασική πύλη Azure, στη **δηλαδή** ενοποίησης σελίδα εφαρμογής, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6] 

2. Στη σελίδα **Πώς θα θέλατε χρήστες να πραγματοποιούν είσοδο στην δηλαδή** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
 
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-namely-tutorial/tutorial_namely_03.png) 

3. Στη σελίδα **Ρύθμιση παραμέτρων των ρυθμίσεων της εφαρμογής** παραθύρου διαλόγου, ακολουθήστε τα παρακάτω βήματα:.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-namely-tutorial/tutorial_namely_04.png) 

    μια. Στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από τους χρήστες σας για να συνδεθείτε εφαρμογή σας συγκεκριμένα (π.χ.: *https://fabrikam.Namely.com/*).

    β. Κάντε κλικ στο κουμπί **Επόμενο**.
 
 
4. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο δηλαδή** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-namely-tutorial/tutorial_namely_05.png) 

    μια. Κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο στον υπολογιστή σας.

    β. Κάντε κλικ στο κουμπί **Επόμενο**.


1. Σε ένα άλλο παράθυρο του προγράμματος περιήγησης, πραγματοποιήστε είσοδο για να σας δηλαδή εταιρική τοποθεσία ως διαχειριστής.

1. Στη γραμμή εργαλείων στο επάνω μέρος, κάντε κλικ στην επιλογή **εταιρείας**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

1. Κάντε κλικ στην καρτέλα **Ρυθμίσεις** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 


1. Κάντε κλικ στην επιλογή **SAML**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 


1. Στη σελίδα **Ρυθμίσεις SAML** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png) 

    μια. Κάντε κλικ στην επιλογή **Ενεργοποίηση SAML**. 

    β. Στην κλασική πύλη Azure, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο δηλαδή** , αντιγράψτε την τιμή **Μία μόνο είσοδο στη διεύθυνση URL της υπηρεσίας** και, στη συνέχεια, επικολλήστε το στο πλαίσιο κειμένου **διεύθυνση url DDO υπηρεσία παροχής ταυτότητας** . 

    c. Άνοιγμα ληφθέντων πιστοποιητικού στο Σημειωματάριο, αντιγράψτε το περιεχόμενο και, στη συνέχεια, επικολλήστε την στο πλαίσιο κειμένου **πιστοποιητικό υπηρεσία παροχής ταυτότητας** .    

    d. Κάντε κλικ στην επιλογή **Αποθήκευση**.


6. Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. 

    ![Azure AD καθολικής σύνδεσης][10]

7. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  

    ![Azure AD καθολικής σύνδεσης][11]




### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Στόχος αυτής της ενότητας είναι να δημιουργήσετε έναν χρήστη δοκιμής στην πύλη Azure κλασική που ονομάζεται Britta Simon.

![Δημιουργία Azure AD χρήστη][20]


**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_09.png)  

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 
 
4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**. 

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα: 

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_05.png)  

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα: 

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_06.png) 
 
    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.
    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_07.png) 
 
8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_08.png) 
  
    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   

  
 
### <a name="creating-a-namely-test-user"></a>Δημιουργία μιας δηλαδή δοκιμή χρήστη

Στόχος αυτής της ενότητας είναι να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon συγκεκριμένα.

**Για να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon συγκεκριμένα, ακολουθήστε τα παρακάτω βήματα:**

1. Σύνδεση στην σας δηλαδή εταιρική τοποθεσία ως διαχειριστής.

1. Στη γραμμή εργαλείων στο επάνω μέρος, κάντε κλικ στην επιλογή **άτομα**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

1. Κάντε κλικ στην καρτέλα **καταλόγου** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

1. Κάντε κλικ στην επιλογή **Προσθήκη νέου προσώπου**.



1. Στο παράθυρο διαλόγου **Προσθήκη νέου προσώπου** , ακολουθήστε τα παρακάτω βήματα:

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.

    β. Στο πλαίσιο κειμένου **όνομα επώνυμο** , πληκτρολογήστε **Simon**.

    c. Στο πλαίσιο κειμένου **ηλεκτρονικού ταχυδρομείου** , πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου του Britta στην πύλη του Azure κλασική.

    d. Κάντε κλικ στην επιλογή **Αποθήκευση**.





### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Στόχος αυτής της ενότητας είναι η ενεργοποίηση Britta Simon χρήση Azure καθολικής σύνδεσης, δηλαδή εκχώρηση της πρόσβασης.

![Εκχώρηση χρήστη][200] 

**Για να αντιστοιχίσετε Britta Simon σε συγκεκριμένα, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική Azure πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εκχώρηση χρήστη][201] 

2. Στη λίστα εφαρμογών, επιλέξτε **συγκεκριμένα**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-namely-tutorial/tutorial_namely_50.png) 

1. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Εκχώρηση χρήστη][203] 

1. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

2. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Εκχώρηση χρήστη][205]



### <a name="testing-single-sign-on"></a>Δοκιμή καθολικής σύνδεσης

Είναι ο στόχος αυτής της ενότητας για να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.

Όταν κάνετε κλικ το συγκεκριμένα πλακιδίου στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα συνδεθεί στην σας συγκεκριμένα εφαρμογής.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-namely-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-namely-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-namely-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-namely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-namely-tutorial/tutorial_general_205.png






