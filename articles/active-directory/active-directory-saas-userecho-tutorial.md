<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με UserEcho | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καθολικής σύνδεσης μεταξύ Azure Active Directory και UserEcho."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με UserEcho

Ο στόχος αυτού του προγράμματος εκμάθησης είναι να σας δείξουν πώς μπορείτε να ενοποιήσετε το UserEcho με το Azure Active Directory (Azure AD).  
Ενοποίηση UserEcho με Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα: 

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση σε UserEcho 
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να συνδεθεί στην UserEcho (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία 

Για να ρυθμίσετε τις παραμέτρους ενοποίησης Azure AD με UserEcho, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Μια UserEcho καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Σενάριο περιγραφή
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύριο μπλοκ δόμησης:

1. Προσθήκη UserEcho από τη συλλογή 
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-userecho-from-the-gallery"></a>Προσθήκη UserEcho από τη συλλογή
Για να ρυθμίσετε την ενσωμάτωση των UserEcho στην Azure AD, πρέπει να προσθέσετε UserEcho από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε UserEcho από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**. 

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **UserEcho**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_01.png)

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **UserEcho**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Ο στόχος αυτής της ενότητας είναι να σας δείξουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με UserEcho που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε ποιος είναι ο χρήστης αντίστοιχο στο UserEcho σε έναν χρήστη στο Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στο UserEcho πρέπει να καθοριστούν.  
Αυτή η σχέση σύνδεση είναι εγκατεστημένος κατά την αντιστοίχιση της τιμής του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** στο UserEcho.
 
Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με UserEcho, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
4. **[Δημιουργία μιας UserEcho δοκιμή χρήστη](#creating-a-userecho-test-user)** - έχουν αντίστοιχο του Britta Simon στο UserEcho που είναι συνδεδεμένο με το Azure AD αναπαριστάται με εκείνη.
5. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Στόχος αυτής της ενότητας είναι για να ενεργοποιήσετε Azure AD καθολικής σύνδεσης στην πύλη του Azure κλασική και για τη ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή σας UserEcho. 




**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με UserEcho, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **UserEcho** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6] 

2. Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο UserEcho** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_03.png) 

3. Στη σελίδα του παραθύρου διαλόγου **Διαμόρφωση παραμέτρων ρυθμίσεων εφαρμογής** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_04.png) 

    μια. Στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από τους χρήστες σας για να συνδεθείτε εφαρμογή σας UserEcho (π.χ.: *https://fabrikam.UserEcho.com/*).

    β. Κάντε κλικ στο κουμπί **Επόμενο**.
 
 
4. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο UserEcho** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_05.png) 

    μια. Κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο στον υπολογιστή σας.

    β. Κάντε κλικ στο κουμπί **Επόμενο**.


1. Σε ένα άλλο παράθυρο προγράμματος περιήγησης, πραγματοποιήστε είσοδο τοποθεσία της εταιρείας σας UserEcho ως διαχειριστής.

1. Στη γραμμή εργαλείων στο επάνω μέρος, επιλέξτε το όνομα χρήστη για να αναπτύξετε το μενού και, στη συνέχεια, κάντε κλικ στην επιλογή **εγκατάσταση**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

1. Κάντε κλικ στην επιλογή **ενοποιήσεις**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 


1. Κάντε κλικ στην επιλογή **τοποθεσία Web**και, στη συνέχεια, κάντε κλικ στην επιλογή **καθολικής σύνδεσης (SAML2)**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 


1. Στη σελίδα **καθολικής σύνδεσης (SAML)** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png) 

    μια. Ως **με δυνατότητα SAML**, επιλέξτε **Ναι**. 

    β. Στο το Azure κλασική πύλη, τη ρύθμιση παραμέτρων Καθολικής σύνδεσης στη σελίδα του παραθύρου διαλόγου UserEcho, αντιγράψτε την τιμή **Μία μόνο είσοδο στη διεύθυνση URL της υπηρεσίας** και, στη συνέχεια, επικολλήστε την στο πλαίσιο κειμένου **Διεύθυνση URL SSO SAML** .

    c. Στο το Azure κλασική πύλη, τη ρύθμιση παραμέτρων Καθολικής σύνδεσης στη σελίδα του παραθύρου διαλόγου UserEcho, αντιγράψτε την τιμή **Διεύθυνση URL απομακρυσμένης αποσυνδεθείτε** και, στη συνέχεια, επικολλήστε τη **διεύθυνση URL απομακρυσμένης logoout** πλαίσιο κειμένου. 

    d. Άνοιγμα ληφθέντων πιστοποιητικού στο Σημειωματάριο, αντιγράψτε το περιεχόμενο και, στη συνέχεια, επικολλήστε την στο πλαίσιο κειμένου **Πιστοποιητικό X.509** .    

    ε. Κάντε κλικ στην επιλογή **Αποθήκευση**.


6. Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. 

    ![Azure AD καθολικής σύνδεσης][10]

7. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  

    ![Azure AD καθολικής σύνδεσης][11]




### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Στόχος αυτής της ενότητας είναι να δημιουργήσετε έναν χρήστη δοκιμής στην πύλη Azure κλασική που ονομάζεται Britta Simon.

![Δημιουργία Azure AD χρήστη][20]

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_09.png)  

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 
 
4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**. 

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα: 

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_05.png)  

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα: 

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_06.png) 
 
    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.
    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_07.png) 
 
8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_08.png) 
  
    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   

  
 
### <a name="creating-a-userecho-test-user"></a>Δημιουργία μιας UserEcho δοκιμής χρήστη

Στόχος αυτής της ενότητας είναι να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο UserEcho.

**Για να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο UserEcho, ακολουθήστε τα παρακάτω βήματα:**

1. Σύνδεση στην τοποθεσία της εταιρείας σας UserEcho ως διαχειριστής.

1. Στη γραμμή εργαλείων στο επάνω μέρος, επιλέξτε το όνομα χρήστη για να αναπτύξετε το μενού και, στη συνέχεια, κάντε κλικ στην επιλογή **εγκατάσταση**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

1. Κάντε κλικ στην επιλογή **χρήστες**, για να αναπτύξετε την ενότητα **χρήστες** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png) 

1. Κάντε κλικ στην επιλογή **χρήστες**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png) 

1. Κάντε κλικ στην επιλογή **πρόσκληση έναν νέο χρήστη**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)


1. Στο παράθυρο διαλόγου **πρόσκληση έναν νέο χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png) 

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta Simon**.

    β. Στο πλαίσιο κειμένου **ηλεκτρονικού ταχυδρομείου** , πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου του Britta στην πύλη του Azure κλασική.

    c. Κάντε κλικ στην **πρόσκληση**.

Αποστέλλεται μια πρόσκληση Britta, που σας επιτρέπει εκείνη για να ξεκινήσετε να χρησιμοποιείτε το UserEcho. 



### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Στόχος αυτής της ενότητας είναι η ενεργοποίηση Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης σε UserEcho.

![Εκχώρηση χρήστη][200] 

**Για να αντιστοιχίσετε Britta Simon UserEcho, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική Azure πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εκχώρηση χρήστη][201] 

2. Στη λίστα εφαρμογών, επιλέξτε **UserEcho**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_50.png) 

1. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Εκχώρηση χρήστη][203] 

1. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

2. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Εκχώρηση χρήστη][205]



### <a name="testing-single-sign-on"></a>Δοκιμή καθολικής σύνδεσης

Είναι ο στόχος αυτής της ενότητας για να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.  
Όταν κάνετε κλικ στο πλακίδιο UserEcho στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε στην εφαρμογή σας UserEcho.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_205.png






