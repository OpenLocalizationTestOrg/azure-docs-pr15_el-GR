<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με οικογένεια BPM Questetra | Microsoft Aure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καθολικής σύνδεσης μεταξύ Azure Active Directory και οικογένεια BPM Questetra."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με οικογένεια BPM Questetra

Στόχος αυτού του προγράμματος εκμάθησης είναι ώστε να σας δείξουν πώς μπορείτε να ενοποιήσετε οικογένεια BPM Questetra με Azure Active Directory (Azure AD).  
Ενοποίηση Questetra BPM οικογένεια με Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα: 

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση στην οικογένεια προγραμμάτων BPM Questetra 
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα λήψη υπογραφή στην οικογένεια προγραμμάτων BPM Questetra (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία 

Για να ρυθμίσετε τις παραμέτρους ενοποίησης Azure AD με οικογένεια BPM Questetra, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Μια [Οικογένεια προγραμμάτων BPM Questetra](https://senbon-imadegawa-988.questetra.net/) καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Σενάριο περιγραφή
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής.  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τρεις κύριο μπλοκ δόμησης:

1. Προσθήκη οικογένεια BPM Questetra από τη συλλογή 
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Προσθήκη οικογένεια BPM Questetra από τη συλλογή
Για να ρυθμίσετε την ενσωμάτωση της οικογένειας προγραμμάτων BPM Questetra στην Azure AD, πρέπει να προσθέσετε οικογένεια BPM Questetra από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε οικογένεια BPM Questetra από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**. 

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **Οικογένεια BPM Questetra**.

    ![Εφαρμογές][5]

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **Οικογένεια BPM Questetra**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Ο στόχος αυτής της ενότητας είναι να σας δείξουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με οικογένεια BPM Questetra που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε ποιος είναι ο χρήστης αντίστοιχο στην οικογένεια προγραμμάτων BPM Questetra σε έναν χρήστη στο Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στην οικογένεια προγραμμάτων BPM Questetra πρέπει να καθοριστούν.  
Αυτή η σχέση σύνδεση είναι εγκατεστημένος κατά την αντιστοίχιση της τιμής του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** στην οικογένεια προγραμμάτων BPM Questetra.
 
Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με την οικογένεια προγραμμάτων BPM Questetra, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
4. **[Δημιουργία μιας οικογένειας προγραμμάτων BPM Questetra δοκιμή χρήστη](#creating-a-questetra-bpm-suite-test-user)** - έχουν αντίστοιχο του Britta Simon στην οικογένεια BPM Questetra το οποίο είναι συνδεδεμένο με το Azure AD αναπαράσταση εκείνη.
5. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Στόχος αυτής της ενότητας είναι για να ενεργοποιήσετε Azure AD καθολικής σύνδεσης στην πύλη του Azure κλασική και για τη ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή σας οικογένεια BPM Questetra.

**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με την οικογένεια προγραμμάτων BPM Questetra, εκτελέστε τα παρακάτω βήματα:**

1. Στην κλασική πύλη Azure, στη σελίδα ενοποίησης **Questetra BPM οικογένεια** εφαρμογών, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][8]

2. Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο οικογένεια προγραμμάτων BPM Questetra** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Azure AD καθολικής σύνδεσης][9]


3. Σε ένα παράθυρο προγράμματος περιήγησης web διαφορετικά, συνδεθείτε στην τοποθεσία της εταιρείας σας **Questetra BPM οικογένεια** ως διαχειριστής.

4. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **Ρυθμίσεις του συστήματος**. 

    ![Azure AD καθολικής σύνδεσης][10]

5. Για να ανοίξετε τη σελίδα **SingleSignOnSAML** , κάντε κλικ στην επιλογή **SSO (SAML)**. 

    ![Azure AD καθολικής σύνδεσης][11]


6. Στην κλασική Azure πύλη, στη σελίδα παράθυρο διαλόγου **Ρύθμιση παραμέτρων των ρυθμίσεων της εφαρμογής** , εκτελέστε τα ακόλουθα βήματα: 

    ![Ρύθμιση παραμέτρων εφαρμογής][13]
 
    μια. Στο που **Questetra BPM οικογένεια** εταιρική τοποθεσία, στην ενότητα πληροφορίες SP, αντιγράψτε τη **Διεύθυνση URL ACS**και στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** .

    β. Και στο που **Questetra BPM οικογένεια** εταιρική τοποθεσία, στην ενότητα πληροφορίες SP, αντιγράψτε το **Αναγνωριστικό οντότητας**και, στη συνέχεια, επικολλήστε την στο πλαίσιο κειμένου **Διεύθυνση URL εκδότη** .

    c. Και στο που **Questetra BPM οικογένεια** εταιρική τοποθεσία, στην ενότητα πληροφορίες SP, αντιγράψτε τη **Διεύθυνση URL ACS**και στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **Διεύθυνση URL απάντηση** .

    d. Κάντε κλικ στο κουμπί **Επόμενο**.

 
7. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στην οικογένεια προγραμμάτων BPM Questetra** , κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο πιστοποιητικού τοπικά στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][14]


8. Σε εσάς **Οικογένεια BPM Questetra** εταιρική τοποθεσία, ακολουθήστε τα παρακάτω βήματα: 

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][15]

    μια. Επιλέξτε **Ενεργοποίηση της καθολικής σύνδεσης**.
     
    β. Στην κλασική Azure πύλη, αντιγράψτε την τιμή **Διεύθυνσης URL εκδότη** και, στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **Το Αναγνωριστικό οντότητας** .

    c. Στην κλασική Azure πύλη, αντιγράψτε την τιμή **Μία μόνο είσοδο στη διεύθυνση URL της υπηρεσίας** και, στη συνέχεια, επικολλήστε το στο πλαίσιο κειμένου **εισόδου διεύθυνση URL της σελίδας** .

    d. Στην κλασική Azure πύλη, αντιγράψτε την τιμή **Μία διεύθυνση URL της υπηρεσίας Sign-Out** και, στη συνέχεια, επικολλήστε τη **διεύθυνση URL της σελίδας Sign-out** πλαίσιο κειμένου.

    ε. Στο πλαίσιο κειμένου **NameID μορφή** , πληκτρολογήστε **urn: oasis: ονόματα: tc: SAML:1.1:nameid-μορφή: emailAddress**.


    f. Δημιουργία βάσης 64 κωδικοποιημένο αρχείου από το πιστοποιητικό που έχετε λάβει. 

    >[AZURE.TIP] Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [πώς μπορείτε να μετατρέψετε ένα πιστοποιητικό δυαδικών δεδομένων σε ένα αρχείο κειμένου](http://youtu.be/PlgrzUZ-Y1o).

    γ. Άνοιγμα βάσης 64 κωδικοποιημένο πιστοποιητικού στο Σημειωματάριο, αντιγράψτε το περιεχόμενο της στο Πρόχειρο και, στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **επικύρωσης πιστοποιητικού** . 

    h. Κάντε κλικ στην επιλογή **Αποθήκευση**.


9. Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. 

    ![Τι είναι το Azure AD Connect][17]


10. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  

    ![Τι είναι το Azure AD Connect][18]




### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Στόχος αυτής της ενότητας είναι να δημιουργήσετε έναν χρήστη δοκιμής στην πύλη Azure κλασική που ονομάζεται Britta Simon.

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία Azure AD δοκιμής χρήστη][100] 

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Δημιουργία Azure AD δοκιμής χρήστη][101] 

4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**. 

    ![Δημιουργία Azure AD δοκιμής χρήστη][102] 

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία Azure AD δοκιμής χρήστη][103]
 
    μια. Ως **Τύπο του χρήστη**, επιλέξτε **νέο χρήστη στην εταιρεία σας**.
  
    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί Επόμενο.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα: 

    ![Δημιουργία Azure AD δοκιμής χρήστη][104] 
  
    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**. 
 
    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία Azure AD δοκιμής χρήστη][105]  

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία Azure AD δοκιμής χρήστη][106]   
  
    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.
  
    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>Δημιουργία μια οικογένεια προγραμμάτων BPM Questetra δοκιμής χρήστη

Στόχος αυτής της ενότητας είναι να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στην οικογένεια προγραμμάτων BPM Questetra.

**Για να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στην οικογένεια προγραμμάτων BPM Questetra, ακολουθήστε τα παρακάτω βήματα:**

1.  Σύνδεση στην τοποθεσία της εταιρείας σας οικογένεια BPM Questetra ως διαχειριστής.
2.  Μεταβείτε στις επιλογές **ρυθμίσεις του συστήματος > λίστα χρηστών > Νέος χρήστης**. 
3.  Στο παράθυρο διαλόγου νέο χρήστη, εκτελέστε τα ακόλουθα βήματα: 

    ![Δημιουργία δοκιμής χρήστη][300] 

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε το όνομα χρήστη του Britta στο Azure AD.

    β. Στο πλαίσιο κειμένου **ηλεκτρονικού ταχυδρομείου** , πληκτρολογήστε το όνομα χρήστη του Britta στο Azure AD.

    c. Στο πλαίσιο κειμένου **κωδικός πρόσβασης** , πληκτρολογήστε έναν κωδικό πρόσβασης.

4.  Κάντε κλικ στην επιλογή **Προσθήκη νέου χρήστη**.



### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Στόχος αυτής της ενότητας είναι η ενεργοποίηση Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης στην οικογένεια προγραμμάτων BPM Questetra.

![Τι είναι το Azure AD Connect][200]

**Για να αντιστοιχίσετε Britta Simon Questetra BPM οικογένεια, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική Azure πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Τι είναι το Azure AD Connect][201]

2. Στη λίστα εφαρμογών, επιλέξτε **Οικογένεια BPM Questetra**.

    ![Τι είναι το Azure AD Connect][205]

1. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Τι είναι το Azure AD Connect][202]

1. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

    ![Τι είναι το Azure AD Connect][203]

2. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Τι είναι το Azure AD Connect][204]



### <a name="testing-single-sign-on"></a>Δοκιμή καθολικής σύνδεσης

Είναι ο στόχος αυτής της ενότητας για να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.  
Όταν κάνετε κλικ στο πλακίδιο οικογένεια BPM Questetra στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε στην εφαρμογή σας οικογένεια BPM Questetra.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 