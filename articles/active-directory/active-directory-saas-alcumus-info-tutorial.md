<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με το Exchange πληροφορίες Alcumus | Microsoft Azure"
    description="Μάθετε τον τρόπο ρύθμισης παραμέτρων Καθολικής σύνδεσης μεταξύ Azure Active Directory και το Exchange πληροφορίες Alcumus."
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


# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με το Exchange πληροφορίες Alcumus

Στόχος αυτού του προγράμματος εκμάθησης είναι ώστε να σας δείξουν πώς μπορείτε να ενοποιήσετε Alcumus πληροφορίες Exchange με το Azure Active Directory (Azure AD).  
Ενοποίηση Alcumus πληροφορίες Exchange με το Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα: 

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση στο Exchange πληροφορίες Alcumus 
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να συνδεθεί στην Alcumus πληροφορίες Exchange (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία 

Για να ρυθμίσετε τις παραμέτρους Azure AD ενοποίηση με το Exchange πληροφορίες Alcumus, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του [Azure AD](https://azure.microsoft.com/)
- Μια [Alcumus πληροφορίες Exchange](http://www.alcumusgroup.com/) καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Σενάριο περιγραφή
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τρεις κύριο μπλοκ δόμησης:

1. Προσθήκη Alcumus Exchange πληροφορίες από τη συλλογή 
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Προσθήκη Alcumus Exchange πληροφορίες από τη συλλογή
Για να ρυθμίσετε την ενοποίηση του Exchange πληροφορίες Alcumus σε Azure AD, πρέπει να προσθέσετε Alcumus πληροφορίες Exchange από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε Alcumus Exchange πληροφορίες από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**. 

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **Alcumus πληροφορίες Exchange**.

    ![Εφαρμογές][5]

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **Alcumus πληροφορίες Exchange**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Εφαρμογές][400]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Ο στόχος αυτής της ενότητας είναι να σας δείξουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με το Exchange πληροφορίες Alcumus που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε ποιος είναι ο χρήστης αντίστοιχο στο Exchange Alcumus πληροφορίες σε έναν χρήστη στο Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στο Exchange πληροφορίες Alcumus πρέπει να καθοριστούν.  
Αυτή η σχέση σύνδεση είναι εγκατεστημένος κατά την αντιστοίχιση της τιμής του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** στο Exchange πληροφορίες Alcumus.
 
Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με το Exchange πληροφορίες Alcumus, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
4. **[Τη δημιουργία μιας ανταλλαγής πληροφοριών Alcumus δοκιμή χρήστη](#creating-a-alcumus-info-exchange-test-user)** - έχουν αντίστοιχο του Britta Simon στο Exchange πληροφορίες Alcumus που είναι συνδεδεμένο με το Azure AD αναπαράσταση εκείνη.
5. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Στόχος αυτής της ενότητας είναι για να ενεργοποιήσετε Azure AD καθολικής σύνδεσης στην πύλη του Azure κλασική και για τη ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή σας Exchange πληροφορίες Alcumus.

**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με το Exchange πληροφορίες Alcumus, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Alcumus πληροφορίες Exchange** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6]

2. Στη σελίδα **Πώς θα θέλατε χρήστες να πραγματοποιούν είσοδο Alcumus πληροφορίες Exchange** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Azure AD καθολικής σύνδεσης][7]

3. Στη σελίδα του παραθύρου διαλόγου **Διαμόρφωση παραμέτρων ρυθμίσεων εφαρμογής** , εκτελέστε τα ακόλουθα βήματα: 

    ![Azure AD καθολικής σύνδεσης][8]
 
    μια. στο πλαίσιο κειμένου **Διεύθυνση URL απάντηση** , πληκτρολογήστε τη διεύθυνση URL καταναλωτή που ήταν το πρόγραμμα εγκατάστασης για εσάς από την ομάδα υποστήριξης Alcumus πληροφορίες Exchange.

    > [AZURE.NOTE] Εάν δεν γνωρίζετε τι είναι η σωστή τιμή, επικοινωνήστε με την ομάδα υποστήριξης του Exchange πληροφορίες Alcumus μέσω [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).

    β. Κάντε κλικ στο κουμπί **Επόμενο**.
 
4. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Exchange πληροφορίες Alcumus** , κάντε κλικ στην επιλογή **Λήψη μετα-δεδομένων**και, στη συνέχεια, αποθηκεύστε το αρχείο μετα-δεδομένων τοπικά στον υπολογιστή σας.

    ![Τι είναι το Azure AD Connect][9]

5. Επικοινωνήστε με την ομάδα υποστήριξης του Exchange πληροφορίες Alcumus μέσω [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com), δώστε τους με το αρχείο μετα-δεδομένων και τους να το ενημερώσει ότι θα πρέπει να επιτρέπουν SSO για εσάς.


6. Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. 

    ![Τι είναι το Azure AD Connect][10]

7. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  

    ![Τι είναι το Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Στόχος αυτής της ενότητας είναι να δημιουργήσετε έναν χρήστη δοκιμής στην πύλη Azure κλασική που ονομάζεται Britta Simon.  

![Δημιουργία Azure AD χρήστη][20]

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
 
4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**. 

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα: 

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.
  
    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.
  
    c. Κάντε κλικ στο κουμπί Επόμενο.



6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα: 

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 
  

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  
  
    β. Στο το **Επώνυμο** txtbox, τύπος, **Simon**.
  
    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.
  
    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.
  
    ε. Κάντε κλικ στο κουμπί **Επόμενο**.


7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
 

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 

    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.
  
    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   

  
 
### <a name="creating-a-alcumus-info-exchange-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Alcumus πληροφορίες Exchange

Στόχος αυτής της ενότητας είναι να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο Exchange πληροφορίες Alcumus.

**Για να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο Exchange πληροφορίες Alcumus, ακολουθήστε τα παρακάτω βήματα:**

1. Επικοινωνήστε με την ομάδα υποστήριξης του Exchange πληροφορίες Alcumus μέσω [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com),


### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Στόχος αυτής της ενότητας είναι η ενεργοποίηση Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης στο Exchange πληροφορίες Alcumus.

![Εκχώρηση χρήστη][200]

**Για να αντιστοιχίσετε Britta Simon Alcumus πληροφορίες Exchange, ακολουθήστε τα παρακάτω βήματα:**

1. Στην πύλη του Azure, για να ανοίξετε την προβολή εφαρμογές, στην προβολή καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εκχώρηση χρήστη][201]

2. Στη λίστα εφαρμογών, επιλέξτε **Alcumus πληροφορίες Exchange**.

    ![Εκχώρηση χρήστη][202]

1. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Εκχώρηση χρήστη][203]

1. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

2. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Εκχώρηση χρήστη][205]



### <a name="testing-single-sign-on"></a>Δοκιμές καθολικής σύνδεσης

Είναι ο στόχος αυτής της ενότητας για να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.  
Όταν κάνετε κλικ στο πλακίδιο Alcumus πληροφορίες Exchange στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε στην εφαρμογή σας Exchange πληροφορίες Alcumus.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png