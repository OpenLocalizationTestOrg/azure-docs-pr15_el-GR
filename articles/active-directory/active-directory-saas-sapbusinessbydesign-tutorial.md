<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με SAP επιχειρήσεις ByDesign | Microsoft Azure"
    description="Μάθετε τον τρόπο ρύθμισης παραμέτρων Καθολικής σύνδεσης μεταξύ Azure Active Directory και ByDesign επιχειρήσεις SAP."
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
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με ByDesign επιχειρήσεις SAP

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς μπορείτε να ενοποιήσετε ByDesign επιχειρήσεις SAP με Azure Active Directory (Azure AD).

Ενοποίηση ByDesign επιχειρήσεις SAP με Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα:

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση σε ByDesign επιχειρήσεις SAP
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να συνδεθεί στην ByDesign επιχειρήσεις SAP (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ρυθμίσετε τις παραμέτρους ενοποίησης Azure AD με ByDesign επιχειρήσεις SAP, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Μια ByDesign επιχειρήσεις SAP καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Σενάριο περιγραφή
Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής.

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύριο μπλοκ δόμησης:

1. Προσθήκη ByDesign επιχειρήσεις SAP από τη συλλογή
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-sap-business-bydesign-from-the-gallery"></a>Προσθήκη ByDesign επιχειρήσεις SAP από τη συλλογή
Για να ρυθμίσετε την ενσωμάτωση των ByDesign επιχειρήσεις SAP στην Azure AD, πρέπει να προσθέσετε SAP επιχειρήσεις ByDesign από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε SAP επιχειρήσεις ByDesign από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.


3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **ByDesign επιχειρήσεις SAP**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_01.png)

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **ByDesign επιχειρήσεις SAP**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Υπηρεσία καταλόγου Active Directory](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Σε αυτήν την ενότητα, μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με το SAP ByDesign επιχειρήσεις που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε ποιος είναι ο χρήστης αντίστοιχο στο ByDesign επιχειρήσεις SAP σε ένα χρήστη στο Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στο SAP επιχειρήσεις ByDesign πρέπει να καθοριστούν.

Αυτή η σχέση σύνδεση είναι εγκατεστημένος κατά την αντιστοίχιση της τιμής του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** στο ByDesign επιχειρήσεις SAP.

Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με ByDesign επιχειρήσεις SAP, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
3. **[Δημιουργία μιας ByDesign επιχειρήσεις SAP δοκιμή χρήστη](#creating-an-sap-business-bydesign-test-user)** - έχουν αντίστοιχο του Britta Simon σε ByDesign επιχειρήσεις SAP που συνδέεται με το Azure AD αναπαριστάται με εκείνη.
4. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Σε αυτήν την ενότητα, ενεργοποιείτε Azure AD καθολικής σύνδεσης στην πύλη του κλασική και ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή σας ByDesign επιχειρήσεις SAP.

Εφαρμογή ByDesign επιχειρήσεις SAP αναμένει το διεκδικήσεων SAML σε συγκεκριμένη μορφή. Ρυθμίστε τις ακόλουθες απαιτήσεις για αυτήν την εφαρμογή. Μπορείτε να διαχειριστείτε τις τιμές από αυτά τα χαρακτηριστικά από την καρτέλα **"Atrribute"** της εφαρμογής. Το παρακάτω στιγμιότυπο οθόνης εμφανίζει ένα παράδειγμα για αυτό. 


**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με ByDesign επιχειρήσεις SAP, εκτελέστε τα παρακάτω βήματα:**


1. Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **ByDesign επιχειρήσεις SAP** , στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χαρακτηριστικά**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_80.png) 


2. Στη λίστα διακριτικού χαρακτηριστικά SAML χαρακτηριστικά, επιλέξτε το χαρακτηριστικό nameidentifier και, στη συνέχεια, κάντε κλικ στην επιλογή **Επεξεργασία**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_84.png) 

3. Στο παράθυρο διαλόγου Επεξεργασία χαρακτηριστικό χρήστη, εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_85.png) 

    μια. Από την τιμή του χαρακτηριστικού λίστα, επιλέξτε το fuction **ExtractMailPrefix()**

    β. Από τη λίστα αλληλογραφίας, επιλέξτε το χαρακτηριστικό χρήστη που θέλετε να χρησιμοποιήσετε για την εφαρμογή. 
    Για παράδειγμα, εάν θέλετε να χρησιμοποιήσετε το EmployeeID ως χρήστης μοναδικό αναγνωριστικό και που έχετε αποθηκεύσει την τιμή του χαρακτηριστικού στο το ExtensionAttribute2, στη συνέχεια, επιλέξτε **user.extensionattribute2**. 

    c. Κάντε κλικ στην επιλογή **Ολοκλήρωση**. 
    

4. Στην κλασική πύλη, στη σελίδα ενοποίησης εφαρμογής **ByDesign επιχειρήσεις SAP** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .
     
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6] 

5. Στη σελίδα **Πώς θα θέλατε χρήστες να πραγματοποιούν είσοδο ByDesign επιχειρήσεις SAP** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_03.png) 

6. Στη σελίδα του παραθύρου διαλόγου **Διαμόρφωση παραμέτρων ρυθμίσεων εφαρμογής** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_04.png) 

    μια. Στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από τους χρήστες σας να καθολικής σύνδεσης στην εφαρμογή σας SAP επιχειρήσεις ByDesign χρησιμοποιώντας το ακόλουθο μοτίβο:`https://<servername>.sapbydesign.com`
    
    β. Κάντε κλικ στο κουμπί **Επόμενο**
 
7. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο ByDesign επιχειρήσεις SAP** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_05.png)

    μια. Κάντε κλικ στην επιλογή **Λήψη μετα-δεδομένων**και, στη συνέχεια, αποθηκεύστε το αρχείο στον υπολογιστή σας.

    β. Κάντε κλικ στο κουμπί **Επόμενο**.


8. Για να λάβετε SSO έχει ρυθμιστεί για την εφαρμογή σας, ακολουθήστε τα παρακάτω βήματα:

    μια. Πραγματοποιήστε είσοδο πύλη SAP επιχειρήσεις ByDesign με δικαιώματα διαχειριστή.

    β. Μεταβείτε στην **εφαρμογή και κοινών εργασιών διαχείρισης χρηστών** και κάντε κλικ στην καρτέλα **Υπηρεσίες παροχής ταυτότητας** .

    c. Κάντε κλικ στην επιλογή **Νέα υπηρεσία παροχής ταυτότητας** και επιλέξτε το αρχείο XML μετα-δεδομένων που έχετε λάβει από την πύλη του Azure κλασική. Με την εισαγωγή των μετα-δεδομένων, το σύστημα αποστέλλει αυτόματα το πιστοποιητικό απαιτείται υπογραφή και κρυπτογράφηση πιστοποιητικού.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

    d. Για να συμπεριλάβετε τη **Διεύθυνση URL της υπηρεσίας καταναλωτή διεκδίκηση** στην αίτηση SAML, επιλέξτε **Περιλαμβάνουν διεκδίκηση καταναλωτή διεύθυνση URL της υπηρεσίας**.

    ε. Κάντε κλικ στην επιλογή **Ενεργοποίηση καθολικής σύνδεσης**.

    f. Αποθηκεύστε τις αλλαγές σας.

    γ. Κάντε κλικ στην καρτέλα **My συστήματος** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

    h. Αντιγράψτε τη **Διεύθυνση URL SSO** και επικολλήστε τον στο πλαίσιο κειμένου **Azure AD εισόδου στη διεύθυνση URL** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

    να. Καθορίστε εάν τον εργαζόμενο με μη αυτόματο τρόπο να επιλέξετε ανάμεσα σε σύνδεση με το Αναγνωριστικό χρήστη και τον κωδικό πρόσβασης ή SSO, επιλέγοντας **Επιλογή υπηρεσίας παροχής ταυτότητας μη αυτόματα**.

    j. Στην ενότητα **SSO διεύθυνση URL** , καθορίστε τη διεύθυνση URL που πρέπει να χρησιμοποιείται από τον υπάλληλο για να συνδεθεί στο σύστημα. 
    Στο τη διεύθυνση URL αποστέλλονται υπαλλήλου αναπτυσσόμενης λίστας, μπορείτε να επιλέξετε μεταξύ τις ακόλουθες επιλογές:
    
    **Μη SSO διεύθυνσης URL**
 
    Το σύστημα στέλνει μόνο σύστημα κανονικής διεύθυνσης URL για τον εργαζόμενο. Ο εργαζόμενος δεν είναι δυνατό να συνδεθείτε χρησιμοποιώντας SSO, και πρέπει να χρησιμοποιήσετε τον κωδικό πρόσβασης ή να αντί για αυτό το πιστοποιητικό.

    **ΔΙΕΎΘΥΝΣΗ URL SSO** 

    Το σύστημα στέλνει μόνο τη διεύθυνση URL SSO για τον εργαζόμενο. Ο εργαζόμενος μπορεί να συνδεθεί με SSO. Αίτηση ελέγχου ταυτότητας ανακατευθύνεται μέσω του IdP.

    **Αυτόματη επιλογή**
 
    Εάν SSO δεν είναι ενεργό, το σύστημα στέλνει τη διεύθυνση URL κανονική συστήματος για τον εργαζόμενο. Εάν το SSO είναι ενεργό, το σύστημα ελέγχει αν ο εργαζόμενος έχει έναν κωδικό πρόσβασης. Εάν ένας κωδικός πρόσβασης είναι διαθέσιμη, αποστέλλονται SSO διεύθυνση URL και μη SSO διεύθυνση URL για τον εργαζόμενο. Ωστόσο, εάν ο εργαζόμενος δεν έχει κωδικό πρόσβασης, θα σταλεί μόνο τη διεύθυνση URL SSO για τον εργαζόμενο.

    k. Αποθηκεύστε τις αλλαγές σας.

9. Στην κλασική πύλη, επιλέξτε την καθολική σύνδεση ρύθμιση των παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
    
    ![Azure AD καθολικής σύνδεσης][10]

10. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  
 
    ![Azure AD καθολικής σύνδεσης][11]


### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη δοκιμής στην κλασική πύλη που ονομάζεται Britta Simon.


![Δημιουργία Azure AD χρήστη][20]

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_09.png) 

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png) 

4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png) 

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα:
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_05.png) 

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα:
    
    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_06.png) 

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_07.png) 

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_08.png) 

    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   



### <a name="creating-an-sap-business-bydesign-test-user"></a>Δημιουργία ενός χρήστη δοκιμής ByDesign επιχειρήσεις SAP

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο ByDesign επιχειρήσεις SAP. Εργαστείτε επικοινωνήστε με την ομάδα υποστήριξης ByDesign επιχειρήσεις SAP για να προσθέσετε τους χρήστες στην πλατφόρμα ByDesign επιχειρήσεις SAP. 

> [AZURE.NOTE] Βεβαιωθείτε ότι η τιμή NameID πρέπει να συμφωνεί με με το πεδίο όνομα χρήστη της πλατφόρμας ByDesign επιχειρήσεις SAP.

### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Σε αυτήν την ενότητα, μπορείτε να ενεργοποιήσετε Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης σε ByDesign επιχειρήσεις SAP.

![Εκχώρηση χρήστη][200] 

**Για να αντιστοιχίσετε Britta Simon ByDesign επιχειρήσεις SAP, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εκχώρηση χρήστη][201] 

2. Στη λίστα εφαρμογών, επιλέξτε **ByDesign επιχειρήσεις SAP**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_50.png) 

3. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Εκχώρηση χρήστη][203]

4. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

5. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Εκχώρηση χρήστη][205]


### <a name="testing-single-sign-on"></a>Δοκιμές καθολικής σύνδεσης

Σε αυτήν την ενότητα, μπορείτε να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.

Όταν κάνετε κλικ στο πλακίδιο ByDesign επιχειρήσεις SAP στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε στην εφαρμογή σας ByDesign επιχειρήσεις SAP.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_205.png
