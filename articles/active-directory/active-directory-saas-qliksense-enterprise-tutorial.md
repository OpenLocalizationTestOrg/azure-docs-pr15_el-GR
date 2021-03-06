<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Qlik νόημα για μεγάλες επιχειρήσεις | Microsoft Azure"
    description="Μάθετε τον τρόπο ρύθμισης παραμέτρων Καθολικής σύνδεσης μεταξύ Azure Active Directory και Qlik νόημα για μεγάλες επιχειρήσεις."
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
    ms.date="08/31/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Qlik νόημα για μεγάλες επιχειρήσεις

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς μπορείτε να ενοποιήσετε Qlik νόημα για μεγάλες επιχειρήσεις με το Azure Active Directory (Azure AD).

Ενοποίηση Qlik νόημα για μεγάλες επιχειρήσεις με το Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα:

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση σε Qlik νόημα για μεγάλες επιχειρήσεις
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να συνδεθεί στην Qlik νόημα για μεγάλες επιχειρήσεις (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση - κλασική πύλη του Azure

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ρυθμίσετε τις παραμέτρους ενοποίησης Azure AD με Qlik νόημα για μεγάλες επιχειρήσεις, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Ένα εταιρικό νόημα Qlik καθολικής σύνδεσης με δυνατότητα συνδρομής


> [AZURE.NOTE] Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, δεν συνιστάται να χρησιμοποιείτε ένα περιβάλλον παραγωγής.


Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Σενάριο περιγραφή
Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να ελέγξετε Azure AD καθολικής σύνδεσης σε περιβάλλον δοκιμής.

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από δύο κύριο μπλοκ δόμησης:

1. Προσθήκη εταιρικού νόημα Qlik από τη συλλογή
2. Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης


## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Προσθήκη εταιρικού νόημα Qlik από τη συλλογή
Για να ρυθμίσετε την ενσωμάτωση του Qlik για μεγάλες επιχειρήσεις νόημα στην Azure AD, πρέπει να προσθέσετε Qlik νόημα για μεγάλες επιχειρήσεις από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε Qlik νόημα εταιρείας από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory][1]
2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **Qlik νόημα για μεγάλες επιχειρήσεις**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **Qlik νόημα για μεγάλες επιχειρήσεις**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων και έλεγχος Azure AD μονό καθολικής σύνδεσης
Σε αυτήν την ενότητα, μπορείτε να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με Qlik νόημα για μεγάλες επιχειρήσεις που βασίζεται σε ένα χρήστη δοκιμής που ονομάζεται "Britta Simon".

Για καθολικής σύνδεσης για να εργαστείτε, Azure AD πρέπει να γνωρίζετε ποιος είναι ο χρήστης αντίστοιχο στο Qlik νόημα για μεγάλες επιχειρήσεις σε ένα χρήστη στο Azure AD. Με άλλα λόγια, μια σχέση σύνδεση μεταξύ ενός χρήστη Azure AD και το σχετικό χρήστη στο εταιρικό νόημα Qlik πρέπει να καθοριστούν.

Αυτή η σχέση σύνδεση είναι εγκατεστημένος κατά την αντιστοίχιση της τιμής του **ονόματος χρήστη** στο Azure AD ως τιμή του το **όνομα χρήστη** στο Qlik νόημα για μεγάλες επιχειρήσεις.

Για να ρυθμίσετε τις παραμέτρους και να ελέγξετε Azure AD καθολικής σύνδεσης με νόημα Qlik για μεγάλες επιχειρήσεις, πρέπει να ολοκληρώσετε τα παρακάτω μπλοκ δόμησης:

1. **[Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης](#configuring-azure-ad-single-sign-on)** - για να ενεργοποιήσετε τους χρήστες σας για να χρησιμοποιήσετε αυτήν τη δυνατότητα.
2. **[Δημιουργία μιας Azure AD δοκιμή χρήστη](#creating-an-azure-ad-test-user)** - για να ελέγξετε Azure AD καθολικής σύνδεσης με Britta Simon.
3. **[Δημιουργία ενός εταιρικού νόημα Qlik δοκιμή χρήστη](#creating-a-qliksense-enterprise-test-user)** - έχουν αντίστοιχο του Britta Simon στο Qlik νόημα για μεγάλες επιχειρήσεις που συνδέεται με το Azure AD αναπαράσταση εκείνη.
4. **[Εκχώρηση του Azure AD δοκιμή χρήστη](#assigning-the-azure-ad-test-user)** - για να ενεργοποιήσετε την Britta Simon για να χρησιμοποιήσετε Azure AD καθολικής σύνδεσης.
5. **[Δοκιμές καθολικής σύνδεσης](#testing-single-sign-on)** - για να επιβεβαιώσετε αν λειτουργεί η ρύθμιση παραμέτρων.

### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Σε αυτήν την ενότητα, ενεργοποιείτε Azure AD καθολικής σύνδεσης στην πύλη του κλασική και ρύθμιση παραμέτρων Καθολικής σύνδεσης στην εφαρμογή σας Qlik νόημα για μεγάλες επιχειρήσεις.


**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με νόημα Qlik για μεγάλες επιχειρήσεις, εκτελέστε τα ακόλουθα βήματα:**

1. Στην κλασική πύλη, στη σελίδα ενοποίησης εφαρμογής **Qlik νόημα για μεγάλες επιχειρήσεις** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .
     
    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης][6] 

2. Στη σελίδα **Πώς θα θέλατε χρήστες να πραγματοποιούν είσοδο Qlik νόημα για μεγάλες επιχειρήσεις** , επιλέξτε **Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png) 

3. Στη σελίδα του παραθύρου διαλόγου **Διαμόρφωση παραμέτρων ρυθμίσεων εφαρμογής** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png) 

    μια. Στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL που χρησιμοποιούνται από τους χρήστες σας να καθολικής σύνδεσης στην εφαρμογή σας Qlik νόημα για μεγάλες επιχειρήσεις, χρησιμοποιώντας το ακόλουθο μοτίβο: **https://\<Qlik νόημα πλήρως Qualifed Hostname\>: 443 / < εικονικού διακομιστή μεσολάβησης πρόθεμα\>/samlauthn/**.
    
    > [AZURE.NOTE] Προσέξτε την κάθετο στο τέλος της αυτό URI.  Απαιτείται.

    β. Κάντε κλικ στο κουμπί **Επόμενο**
 
4. Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Qlik νόημα για μεγάλες επιχειρήσεις** , εκτελέστε τα ακόλουθα βήματα:

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)

    μια. Κάντε κλικ στην επιλογή **Λήψη μετα-δεδομένων**και, στη συνέχεια, αποθηκεύστε το αρχείο στον υπολογιστή σας.  Προετοιμαστείτε για να επεξεργαστείτε αυτό το αρχείο μετα-δεδομένων πριν από την αποστολή στο διακομιστή Qlik νόημα.

    β. Κάντε κλικ στο κουμπί **Επόμενο**.

5. Προετοιμασία του αρχείου Ομοσπονδία μετα-δεδομένα XML, ώστε να μπορείτε να αποστείλετε που νόημα Qlik διακομιστή.

    > [AZURE.NOTE] Πριν από την αποστολή τα μετα-δεδομένα IdP στο διακομιστή Qlik νόημα, το αρχείο πρέπει να είναι δυνατή η επεξεργασία για να καταργήσετε πληροφορίες για να εξασφαλίσετε τη σωστή λειτουργία μεταξύ Azure AD και νόημα Qlik server.

    ![QlikSense][qs24]

    μια. Ανοίξτε το αρχείο FederationMetaData.xml που έχουν ληφθεί από το Azure σε έναν επεξεργαστή κειμένου.

    β. Αναζήτηση για την τιμή **RoleDescriptor**.  Θα υπάρχουν τέσσερις καταχωρήσεις (δύο ζεύγη στοιχείο tag ανοίγματος και κλεισίματος).

    c. Διαγραφή στο μεταξύ των ετικετών RoleDescriptor και όλες τις πληροφορίες από το αρχείο.

    d. Αποθηκεύστε το αρχείο και να διατηρήσετε τα οποία για χρήση αργότερα σε αυτό το έγγραφο.

6. Περιήγηση για να το Qlik νόημα Qlik Management Console (QMC) ως χρήστης στον οποίο μπορούν να δημιουργήσετε ρυθμίσεις παραμέτρων εικονικού διακομιστή μεσολάβησης.

7. Στο QMC το, κάντε κλικ στο στοιχείο μενού εικονικού διακομιστή μεσολάβησης.

    ![QlikSense][qs6] 

8. Στο κάτω μέρος της οθόνης, κάντε κλικ στο κουμπί Δημιουργία νέου.

    ![QlikSense][qs7]

9. Εμφανίζεται η οθόνη εικονικού διακομιστή μεσολάβησης επεξεργασία.  Στη δεξιά πλευρά της οθόνης είναι ένα μενού για να κάνετε ορατό επιλογές ρύθμισης παραμέτρων.

    ![QlikSense][qs9]

10. Με την επιλογή μενού αναγνώρισης επιλεγμένο, εισαγάγετε τις πληροφορίες αναγνώρισης για τη ρύθμιση παραμέτρων Azure εικονικού διακομιστή μεσολάβησης.

    ![QlikSense][qs8]  
    
    μια. Το πεδίο Περιγραφή είναι ένα φιλικό όνομα για τη ρύθμιση παραμέτρων εικονικού διακομιστή μεσολάβησης.  Πληκτρολογήστε μια τιμή για μια περιγραφή.
    
    β. Το πεδίο πρόθεμα προσδιορίζει το τελικό σημείο εικονικού διακομιστή μεσολάβησης για τη σύνδεση με νόημα Qlik με Azure AD καθολικής σύνδεσης.  Πληκτρολογήστε ένα μοναδικό πρόθεμα όνομα για αυτό εικονικού διακομιστή μεσολάβησης.

    c. Χρονικό όριο αδράνειας περιόδου λειτουργίας (λεπτά) είναι το χρονικό όριο για τις συνδέσεις μέσω αυτό εικονικού διακομιστή μεσολάβησης.

    d. Το όνομα της κεφαλίδας cookie περιόδου λειτουργίας είναι το όνομα cookie την αποθήκευση του αναγνωριστικού περιόδου λειτουργίας για την περίοδο λειτουργίας Qlik νόημα ένας χρήστης λαμβάνει μετά την επιτυχή έλεγχο ταυτότητας.  Αυτό το όνομα πρέπει να είναι μοναδικό.

11. Κάντε κλικ στην επιλογή μενού ελέγχου ταυτότητας για να κάνετε ορατό.  Εμφανίζεται η οθόνη ελέγχου ταυτότητας.

    ![QlikSense][qs10]

    μια. Στην αναπτυσσόμενη λίστα **λειτουργία ανώνυμης πρόσβασης** κατά Καθορίζει αν ανώνυμοι χρήστες ενδέχεται να πρόσβαση νόημα Qlik μέσω του εικονικού διακομιστή μεσολάβησης.  Η προεπιλογή είναι χωρίς ανώνυμος χρήστης.

    β. Στην αναπτυσσόμενη λίστα **μεθόδου ελέγχου ταυτότητας** κατά καθορίζει θα χρησιμοποιήσει το σχήμα ελέγχου ταυτότητας του εικονικού διακομιστή μεσολάβησης.  Επιλέξτε SAML από την αναπτυσσόμενη λίστα.  Περισσότερες επιλογές θα εμφανίζονται ως αποτέλεσμα.

    c. Στο **πεδίο URI host SAML**, εισαγωγής θα καταχωρούν οι χρήστες όνομα κεντρικού υπολογιστή για να αποκτήσετε πρόσβαση νόημα Qlik μέσω αυτό εικονικού διακομιστή μεσολάβησης SAML.  Το όνομα κεντρικού υπολογιστή είναι το uri του διακομιστή Qlik νόημα.

    d. Στο πεδίο **Αναγνωριστικό οντότητας SAML**, εισαγάγετε την ίδια τιμή που πληκτρολογήσατε στο πεδίο SAML host URI.

    ε. Τα **μετα-δεδομένα SAML IdP** είναι το αρχείο επεξεργασία νωρίτερα στην ενότητα **Επεξεργασία Ομοσπονδία μετα-δεδομένα από το Azure AD ρύθμισης παραμέτρων** .  **Πριν από την αποστολή τα μετα-δεδομένα IdP, το αρχείο πρέπει να είναι δυνατή η επεξεργασία** για να καταργήσετε πληροφορίες για να εξασφαλίσετε τη σωστή λειτουργία μεταξύ Azure AD και νόημα Qlik server.  **Ανατρέξτε στις οδηγίες παραπάνω εάν το αρχείο δεν έχει ακόμη για να είναι δυνατή η επεξεργασία.**  Εάν το αρχείο έχει υποστεί επεξεργασία, κάντε κλικ στο κουμπί Αναζήτηση και επιλέξτε το αρχείο δυνατή η επεξεργασία μετα-δεδομένων για αποστολή στη ρύθμιση παραμέτρων εικονικού διακομιστή μεσολάβησης.

    f. Πληκτρολογήστε το όνομα χαρακτηριστικού ή αναφορά σχήματος για το χαρακτηριστικό SAML που αντιπροσωπεύει το **αναγνωριστικό χρήστη** Azure AD θα σας στείλει στο διακομιστή Qlik νόημα.  Σχήμα αναφοράς πληροφορίες είναι διαθέσιμες στη ρύθμιση παραμέτρων Azure εφαρμογή οθόνες δημοσίευση.  Για να χρησιμοποιήσετε το χαρακτηριστικό όνομα, **πληκτρολογήστε http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.

    γ. Πληκτρολογήστε την τιμή για τον **κατάλογο χρήστη** που θα επισυναφθούν χρηστών όταν εκτελούν έλεγχο ταυτότητας διακομιστή νόημα Qlik μέσω Azure AD.  Οποίος καθορίζεται τιμές πρέπει να περικλείεται σε **αγκύλες []**.  Για να χρησιμοποιήσετε ένα χαρακτηριστικό που αποστέλλεται σε το Azure AD SAML διεκδίκηση, πληκτρολογήστε το όνομα του χαρακτηριστικού σε αυτό κείμενο πλαισίου **χωρίς** αγκύλες.

    h. Ο **Αλγόριθμος υπογραφής SAML** ορίζει το πιστοποιητικό παροχής (σε αυτήν την περίπτωση νόημα Qlik διακομιστή) υπηρεσίας είσοδο για τη ρύθμιση παραμέτρων εικονικού διακομιστή μεσολάβησης.  Εάν Qlik νόημα server χρησιμοποιεί ένα αξιόπιστο πιστοποιητικό που δημιουργούνται με χρήση του Microsoft RSA βελτιωμένη και AES Cryptographic Provider, αλλάξτε τον αλγόριθμο υπογραφής SAML σε **SHA-256**.

    να. Στην ενότητα SAML χαρακτηριστικό αντιστοίχισης επιτρέπει για πρόσθετες ιδιότητες όπως ομάδες να αποστέλλονται Qlik νόημα για χρήση σε κανόνες ασφαλείας.

12. Κάντε κλικ στο το επιλογή μενού για να κάνετε ορατό εξισορρόπησης φόρτου.  Εμφανίζεται η οθόνη εξισορρόπηση φόρτου.

    ![QlikSense][qs11]

13. Κάντε κλικ στο νέο διακομιστή κόμβο κουμπί Προσθήκη, επιλέξτε μηχανισμός κόμβο ή κόμβους νόημα Qlik θα στείλετε περιόδων λειτουργίας για σκοπούς εξισορρόπηση φόρτου και κάντε κλικ στο κουμπί Προσθήκη.

    ![QlikSense][qs12]

14. Κάντε κλικ στην επιλογή "για προχωρημένους" για να κάνετε ορατό. Εμφανίζεται η οθόνη για προχωρημένους.

    ![QlikSense][qs13]

    μια. Στη λίστα λευκό Host προσδιορίζει ονόματα που γίνονται αποδεκτά κατά τη σύνδεση με το διακομιστή Qlik νόημα.  **Πληκτρολογήστε το όνομα κεντρικού υπολογιστή που θα καθορίσετε τους χρήστες κατά τη σύνδεση στο διακομιστή Qlik νόημα.** Το όνομα κεντρικού υπολογιστή έχει την ίδια τιμή με το uri SAML κεντρικού υπολογιστή χωρίς το https://.

15. Κάντε κλικ στο κουμπί εφαρμογή.

    ![QlikSense][qs14]

16. Κάντε κλικ στο κουμπί OK για να αποδεχτείτε το μήνυμα προειδοποίησης που δηλώνει θα γίνει επανεκκίνηση διακομιστές μεσολάβησης που συνδέονται με το εικονικού διακομιστή μεσολάβησης.

    ![QlikSense][qs15]

17. Στη δεξιά πλευρά της οθόνης, εμφανίζεται το μενού στοιχεία συνδεδεμένων.  Κάντε κλικ στην επιλογή μενού διακομιστές μεσολάβησης.

    ![QlikSense][qs16]

18. Εμφανίζεται η οθόνη διακομιστή μεσολάβησης.  Κάντε κλικ στο κουμπί σύνδεση στο κάτω μέρος για να συνδέσετε ένα διακομιστή μεσολάβησης στο εικονικού διακομιστή μεσολάβησης.

    ![QlikSense][qs17]

19. Επιλέξτε τον κόμβο διακομιστή μεσολάβησης που θα υποστηρίζουν αυτήν τη σύνδεση εικονικού διακομιστή μεσολάβησης και κάντε κλικ στο κουμπί σύνδεση.  Μετά τη σύνδεση, ο διακομιστής μεσολάβησης θα εμφανίζεται στην περιοχή συσχετισμένη διακομιστές μεσολάβησης.

    ![QlikSense][qs18]
    ![QlikSense][qs19]

20. Μετά από πέντε με δέκα δευτερόλεπτα περίπου, θα εμφανιστεί το μήνυμα QMC ανανέωση.  Κάντε κλικ στο κουμπί Ανανέωση QMC.

    ![QlikSense][qs20]

21. Κατά την ανανέωση του QMC, κάντε κλικ στο στοιχείο μενού διακομιστές μεσολάβησης εικονική. Η νέα καταχώρηση εικονικού διακομιστή μεσολάβησης SAML εμφανίζεται στον πίνακα στην οθόνη.  Μόνο κλικ στην εγγραφή εικονικού διακομιστή μεσολάβησης.

    ![QlikSense][qs51]

22. Στο κάτω μέρος της οθόνης, θα ενεργοποιήσει το κουμπί Λήψη SP μετα-δεδομένων.  Κάντε κλικ στο κουμπί Λήψη SP μετα-δεδομένων για να αποθηκεύσετε τα μετα-δεδομένα σε ένα αρχείο.

    ![QlikSense][qs52]

23. Ανοίξτε το αρχείο sp μετα-δεδομένων.  Παρατηρήστε το **αναγνωριστικό οντότητας** και τη **AssertionConsumerService** εγγραφή.  Αυτές οι τιμές είναι ισοδύναμο με το **αναγνωριστικό** και την **είσοδο στη διεύθυνση URL** στη ρύθμιση παραμέτρων εφαρμογής Azure AD. Εάν δεν ταιριάζουν, στη συνέχεια, θα πρέπει να αντικαταστήσετε τους στον Οδηγό ρύθμισης παραμέτρων Azure AD εφαρμογής.

    ![QlikSense][qs53]

24. Στην κλασική πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
    
    ![Azure AD καθολικής σύνδεσης][10]

25. Στη σελίδα **επιβεβαίωσης καθολική σύνδεση** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**.  
 
    ![Azure AD καθολικής σύνδεσης][11]


### <a name="creating-an-azure-ad-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Azure AD
Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη δοκιμής στην κλασική πύλη που ονομάζεται Britta Simon.


![Δημιουργία Azure AD χρήστη][20]

**Για να δημιουργήσετε ένα χρήστη δοκιμής Azure AD, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png) 

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να εμφανίσετε τη λίστα των χρηστών, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png) 

4. Για να ανοίξετε το παράθυρο διαλόγου **Προσθήκη χρήστη** , στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png) 

5. Στη σελίδα του παραθύρου διαλόγου **πείτε μας σχετικά με αυτόν το χρήστη** , εκτελέστε τα παρακάτω βήματα:  ![τη δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png) 

    μια. Ως τύπο του χρήστη, επιλέξτε νέο χρήστη στην εταιρεία σας.

    β. Στο πλαίσιο όνομα χρήστη **πλαίσιο κειμένου**, πληκτρολογήστε **BrittaSimon**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.

6.  Στη σελίδα του παραθύρου διαλόγου **Προφίλ χρήστη** , εκτελέστε τα ακόλουθα βήματα: ![τη δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png) 

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε **Britta**.  

    β. Στο πλαίσιο **Όνομα επώνυμο** κειμένου, τύπος, **Simon**.

    c. Στο πλαίσιο κειμένου **Εμφανιζόμενο όνομα** , πληκτρολογήστε **Britta Simon**.

    d. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

    ε. Κάντε κλικ στο κουμπί **Επόμενο**.

7. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png) 

8. Στη σελίδα του παραθύρου διαλόγου **λήψη προσωρινό κωδικό πρόσβασης** , εκτελέστε τα ακόλουθα βήματα:

    ![Δημιουργία ενός χρήστη δοκιμής Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png) 

    μια. Σημειώστε την τιμή της το **Νέο κωδικό πρόσβασης**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.   


### <a name="creating-an-qlik-sense-enterprise-test-user"></a>Δημιουργία ενός χρήστη δοκιμής Qlik νόημα για μεγάλες επιχειρήσεις

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα χρήστη που ονομάζεται Britta Simon στο Qlik νόημα για μεγάλες επιχειρήσεις. Εργαστείτε επικοινωνήστε με την ομάδα υποστήριξης Qlik νόημα για μεγάλες επιχειρήσεις για να προσθέσετε τους χρήστες στην πλατφόρμα Qlik νόημα για μεγάλες επιχειρήσεις.


### <a name="assigning-the-azure-ad-test-user"></a>Εκχώρηση Azure AD δοκιμής χρήστη

Σε αυτήν την ενότητα, μπορείτε να ενεργοποιήσετε Britta Simon χρήση Azure καθολικής σύνδεσης, εκχώρηση της πρόσβασης σε Qlik νόημα για μεγάλες επιχειρήσεις.

![Εκχώρηση χρήστη][200] 

**Για να αντιστοιχίσετε Britta Simon Qlik νόημα για μεγάλες επιχειρήσεις, ακολουθήστε τα παρακάτω βήματα:**

1. Στην κλασική πύλη, για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εκχώρηση χρήστη][201] 

2. Στη λίστα εφαρμογών, επιλέξτε **Qlik νόημα για μεγάλες επιχειρήσεις**.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png) 

3. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **χρήστες**.

    ![Εκχώρηση χρήστη][203]

4. Στη λίστα χρηστών, επιλέξτε **Britta Simon**.

5. Στη γραμμή εργαλείων στο κάτω μέρος, κάντε κλικ στο κουμπί **Αντιστοίχιση**.

    ![Εκχώρηση χρήστη][205]


## <a name="testing-single-sign-on"></a>Δοκιμές καθολικής σύνδεσης

Σε αυτήν την ενότητα, μπορείτε να ελέγξετε Azure AD καθολική σύνδεση ρύθμιση των παραμέτρων σας χρησιμοποιώντας τον πίνακα της Access.

Όταν κάνετε κλικ στο πλακίδιο για μεγάλες επιχειρήσεις νόημα Qlik στον πίνακα της Access, που θα πρέπει να λάβετε αυτόματα πραγματοποιήσει-σε στην εφαρμογή σας Qlik νόημα για μεγάλες επιχειρήσεις.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png