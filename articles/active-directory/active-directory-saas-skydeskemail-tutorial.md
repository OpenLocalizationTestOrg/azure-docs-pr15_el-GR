<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με το ηλεκτρονικό ταχυδρομείο Skydesk | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καθολικής σύνδεσης μεταξύ Azure Active Directory και το ηλεκτρονικό ταχυδρομείο Skydesk."
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


# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Skydesk ηλεκτρονικού ταχυδρομείου

Στόχος αυτού του προγράμματος εκμάθησης είναι ώστε να σας δείξουν πώς μπορείτε να ενοποιήσετε Skydesk ηλεκτρονικού ταχυδρομείου με το Azure Active Directory (Azure AD).

Ενοποίηση ηλεκτρονικού ταχυδρομείου Skydesk με Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα:

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση στο ηλεκτρονικό ταχυδρομείο Skydesk
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να συνδεθεί στην Skydesk ηλεκτρονικού ταχυδρομείου (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure Active Directory

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ρυθμίσετε τις παραμέτρους Azure AD ενοποίηση με το ηλεκτρονικό ταχυδρομείο Skydesk, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Ένα μήνυμα ηλεκτρονικού ταχυδρομείου Skydesk καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Σενάριο περιγραφή
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής. 

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύριο μπλοκ δόμησης:

1. Προσθήκη Skydesk ηλεκτρονικού ταχυδρομείου από τη συλλογή
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-skydesk-email-from-the-gallery"></a>Προσθήκη Skydesk ηλεκτρονικού ταχυδρομείου από τη συλλογή
Για να ρυθμίσετε την ενσωμάτωση του μηνύματος ηλεκτρονικού ταχυδρομείου Skydesk στην Azure AD, πρέπει να προσθέσετε Skydesk ηλεκτρονικού ταχυδρομείου από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε Skydesk ηλεκτρονικού ταχυδρομείου από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**. 

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **Skydesk ηλεκτρονικού ταχυδρομείου**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_01.png)

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **Skydesk μήνυμα ηλεκτρονικού ταχυδρομείου**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Στόχος αυτής της ενότητας είναι να σας δείξουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με το ηλεκτρονικό ταχυδρομείο Skydesk που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε ποιος είναι ο χρήστης αντίστοιχο Skydesk μέσω ηλεκτρονικού ταχυδρομείου σε έναν χρήστη στο Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη σε μήνυμα ηλεκτρονικού ταχυδρομείου Skydesk πρέπει να καθοριστούν.

Αυτή η σχέση σύνδεση είναι εγκατεστημένος κατά την αντιστοίχιση της τιμής του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** σε μήνυμα ηλεκτρονικού ταχυδρομείου Skydesk.

Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με το ηλεκτρονικό ταχυδρομείο Skydesk, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
3. **[Δημιουργία μια διεύθυνση ηλεκτρονικού ταχυδρομείου Skydesk δοκιμή χρήστη](#creating-a-Skydesk-Email-test-user)** - έχουν αντίστοιχο του Britta Simon σε μήνυμα ηλεκτρονικού ταχυδρομείου Skydesk που είναι συνδεδεμένο με το Azure AD αναπαράσταση εκείνη.
4. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Στόχος αυτής της ενότητας είναι για να ενεργοποιήσετε Azure AD καθολικής σύνδεσης στην πύλη του Azure κλασική και για τη ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή ηλεκτρονικού ταχυδρομείου Skydesk.



**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με το ηλεκτρονικό ταχυδρομείο Skydesk, εκτελέστε τα ακόλουθα βήματα:**

1. Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Skydesk ηλεκτρονικού ταχυδρομείου** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6] 

2. Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Skydesk ηλεκτρονικού ταχυδρομείου** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_03.png) 


3. Στη σελίδα του παραθύρου διαλόγου **Διαμόρφωση παραμέτρων ρυθμίσεων εφαρμογής** , εκτελέστε τα ακόλουθα βήματα:
 
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_04.png) 


    μια. Στο πλαίσιο κειμένου εισόδου στη διεύθυνση URL, πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από τους χρήστες σας να καθολικής σύνδεσης για την εφαρμογή ηλεκτρονικού ταχυδρομείου Skydesk σας χρησιμοποιώντας το παρακάτω μοτίβο: **"https://mail.skydesk.jp/portal/\<το όνομα της εταιρείας\>"**.

    β. Κάντε κλικ στο κουμπί **Επόμενο**.


4. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Skydesk ηλεκτρονικού ταχυδρομείου** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_05.png) 

    μια. Κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο στον υπολογιστή σας.

    β. Κάντε κλικ στο κουμπί **Επόμενο**.


5. Για να ενεργοποιήσετε SSO **Skydesk**μέσω ηλεκτρονικού ταχυδρομείου, ακολουθήστε τα παρακάτω βήματα:
 
    μια. Καθολικής σύνδεσης στο λογαριασμό σας ηλεκτρονικού ταχυδρομείου Skydesk ως διαχειριστής.

    β. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή διαμόρφωση και επιλέξτε Org. 

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)

    c. Κάντε κλικ στο τομέων από το αριστερό τμήμα του παραθύρου.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Κάντε κλικ στο στοιχείο Προσθήκη τομέα.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    ε. Πληκτρολογήστε το όνομα του τομέα και, στη συνέχεια, επαληθεύστε τον τομέα.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Κάντε κλικ στην επιλογή ελέγχου ταυτότητας **SAML** από το αριστερό τμήμα του παραθύρου

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

6. Στη σελίδα του παραθύρου διαλόγου **Έλεγχος ταυτότητας SAML** , εκτελέστε τα παρακάτω βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [AZURE.NOTE] Για να χρησιμοποιήσετε τον έλεγχο ταυτότητας βάσει SAML, θα πρέπει να έχετε είτε σε **επαλήθευση του τομέα** ή **πύλη διεύθυνση URL** εγκατάστασης. Μπορείτε να ορίσετε την πύλη διεύθυνση URL με το μοναδικό όνομα.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)


    μια. Στην κλασική πύλη Azure AD, αντιγράψτε την τιμή **Διεύθυνσης URL SSO SAML** και, στη συνέχεια, επικολλήστε τη **Διεύθυνση URL σύνδεσης** πλαισίου κειμένου.

    β. Στην κλασική πύλη Azure AD, αντιγράψτε την τιμή **Μία διεύθυνση URL της υπηρεσίας Sign-Out** και, στη συνέχεια, επικολλήστε την στο πλαίσιο κειμένου διεύθυνση URL **αποσυνδεθείτε** .

    c. **Αλλαγή κωδικού πρόσβασης διεύθυνσης URL** είναι προαιρετικό επομένως, αφήστε την κενή.

    d. Κάντε κλικ στο **Λήψη αριθμού-κλειδιού από το αρχείο** για να επιλέξετε το πιστοποιητικό Skydesk ηλεκτρονικού ταχυδρομείου έχετε λάβει και, στη συνέχεια, κάντε κλικ στην επιλογή **Άνοιγμα** για να αποστείλετε το πιστοποιητικό.

    ε. Ως **αλγόριθμο**, επιλέξτε **RSA**.

    f. Κάντε κλικ στο κουμπί **Ok** για να αποθηκεύσετε τις αλλαγές.


7. Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Azure AD καθολικής σύνδεσης][10]

8. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  
  
    ![Azure AD καθολικής σύνδεσης][11]




### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Στόχος αυτής της ενότητας είναι να δημιουργήσετε έναν χρήστη δοκιμής στην πύλη Azure κλασική που ονομάζεται Britta Simon.

![Δημιουργία Azure AD χρήστη][20]

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_09.png) 

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_05.png) 

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_06.png) 

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_07.png) 

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:
 
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_08.png) 

    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   



### <a name="creating-a-skydesk-email-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Skydesk ηλεκτρονικού ταχυδρομείου

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon Skydesk μέσω ηλεκτρονικού ταχυδρομείου.

μια. Κάντε κλικ στο **Χρήστη πρόσβαση** από το αριστερό τμήμα του παραθύρου Skydesk μέσω ηλεκτρονικού ταχυδρομείου και, στη συνέχεια, εισαγάγετε το όνομα χρήστη. 

![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)


[AZURE.NOTE] Εάν χρειάζεστε για τη δημιουργία χρηστών μαζικά, πρέπει να επικοινωνήσετε με την ομάδα υποστήριξης του ηλεκτρονικού ταχυδρομείου Skydesk.


### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Στόχος αυτής της ενότητας είναι η ενεργοποίηση Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης στο ηλεκτρονικό ταχυδρομείο Skydesk.

![Εκχώρηση χρήστη][200] 

**Για να αντιστοιχίσετε Britta Simon Skydesk ηλεκτρονικού ταχυδρομείου, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική Azure πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.
 
    ![Εκχώρηση χρήστη][201] 

2. Στη λίστα εφαρμογών, επιλέξτε **Skydesk μήνυμα ηλεκτρονικού ταχυδρομείου**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_50.png) 

1. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Εκχώρηση χρήστη][203] 

1. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

2. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Εκχώρηση χρήστη][205]



### <a name="testing-single-sign-on"></a>Δοκιμή καθολικής σύνδεσης

Είναι ο στόχος αυτής της ενότητας για να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.

Όταν κάνετε κλικ στο πλακίδιο Skydesk ηλεκτρονικού ταχυδρομείου στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε στην εφαρμογή Skydesk ηλεκτρονικού ταχυδρομείου σας.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_205.png