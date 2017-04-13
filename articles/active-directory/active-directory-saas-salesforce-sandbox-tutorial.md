<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με φίλτρου Salesforce | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Salesforce φίλτρου με Azure Active Directory για την ενεργοποίηση της καθολικής σύνδεσης, αυτοματοποιημένη προμήθεια και άλλα!." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/28/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με Salesforce φίλτρου
>[AZURE.TIP]Για τα σχόλια, κάντε κλικ [εδώ](http://go.microsoft.com/fwlink/?LinkId=521878).
  
Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να εμφανίσετε την ενοποίηση του Azure και φίλτρου Salesforce.  
Άμμου σας δίνουν τη δυνατότητα να δημιουργήσετε πολλά αντίγραφα της εταιρείας σας σε ξεχωριστό περιβάλλοντα για διάφορους λόγους, όπως η ανάπτυξη, δοκιμή και εκπαίδευση, χωρίς επιπτώσεις τα δεδομένα και τις εφαρμογές στην εταιρεία σας Salesforce παραγωγής.  
Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Επισκόπηση φίλτρου](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure
-   Φίλτρου στο Salesforce.com
  
Εάν δεν έχετε ακόμη μια έγκυρη φίλτρου στο Salesforce.com, πρέπει να επικοινωνήσετε με Salesforce.
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα εξής δομικά στοιχεία:

1.  Ενεργοποίηση την ενοποίηση εφαρμογής για Salesforce φίλτρου
2.  Ρύθμιση παραμέτρων Καθολικής σύνδεσης
3.  Ενεργοποίηση του τομέα σας
4.  Ρύθμιση παραμέτρων προμήθεια του χρήστη
5.  Αντιστοίχιση χρηστών

![Σενάριο] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Σενάριο")
##<a name="enabling-the-application-integration-for-salesforce-sandbox"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για Salesforce φίλτρου
  
Στόχος αυτής της ενότητας είναι η διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για Salesforce φίλτρου.

###<a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>Για να ενεργοποιήσετε την ενοποίηση εφαρμογής για Salesforce φίλτρου, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Εφαρμογές")

4.  Για να ανοίξετε τη **Συλλογή εφαρμογής**, κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής του**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής για την εταιρεία μου για να χρησιμοποιήσετε**.

    ![Τι θέλετε να κάνετε;] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Τι θέλετε να κάνετε;")

5.  Στο **πλαίσιο αναζήτησης**, πληκτρολογήστε **Salesforce φίλτρου**.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Συλλογή εφαρμογών")

6.  Στο παράθυρο αποτελεσμάτων, επιλέξτε **Salesforce φίλτρου**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.

    ![Salesforce φίλτρου] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce φίλτρου")
##<a name="configuring-single-sign-on"></a>Ρύθμιση παραμέτρων Καθολικής σύνδεσης
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα Salesforce με το λογαριασμό στο Azure AD χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους της καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **Φίλτρου Salesforce** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

2.  Στη σελίδα **Πώς θέλετε οι χρήστες να πραγματοποιούν είσοδο Salesforce φίλτρου** , επιλέξτε **Microsoft Azure AD καθολικής σύνδεσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Salesforce φίλτρου] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce φίλτρου")

3.  Στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** , στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL σας χρησιμοποιώντας το παρακάτω μοτίβο `http://company.my.salesforce.com`, και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Ρύθμιση παραμέτρων εφαρμογής διεύθυνσης URL")

4. Εάν έχετε ήδη ρυθμίσει καθολικής σύνδεσης για μια άλλη παρουσία του φίλτρου Salesforce στον κατάλογό σας, στη συνέχεια, πρέπει επίσης να ρυθμίσετε το **αναγνωριστικό** να έχουν την ίδια τιμή με την **είσοδο στη διεύθυνση URL**. Το πεδίο " **αναγνωριστικό** " μπορείτε να βρείτε, επιλέγοντας το πλαίσιο ελέγχου **Εμφάνιση ρυθμίσεων για προχωρημένους** στη σελίδα **Ρύθμιση παραμέτρων URL εφαρμογής** από το παράθυρο διαλόγου.

4.  Στη σελίδα **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Salesforce φίλτρου** , κάντε κλικ στην επιλογή **λήψη πιστοποιητικό**και, στη συνέχεια, αποθηκεύστε το αρχείο πιστοποιητικού στον υπολογιστή σας.

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

5.  Σε ένα παράθυρο προγράμματος περιήγησης web διαφορετικά, συνδεθείτε στο σας Salesforce φίλτρου ως διαχειριστής.

6.  Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **εγκατάσταση**.

    ![Το πρόγραμμα εγκατάστασης] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Το πρόγραμμα εγκατάστασης")

7.  Στο παράθυρο περιήγησης στα αριστερά, κάντε κλικ στην επιλογή **Ελέγχους ασφαλείας**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρυθμίσεις καθολικής εισόδου**.

    ![Ρυθμίσεις Καθολικής σύνδεσης] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Ρυθμίσεις Καθολικής σύνδεσης")

8.  Στην ενότητα ρυθμίσεις καθολικής σύνδεσης, ακολουθήστε τα παρακάτω βήματα:

    ![Ρυθμίσεις Καθολικής σύνδεσης] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Ρυθμίσεις Καθολικής σύνδεσης")

    μια.  Επιλέξτε **SAML με δυνατότητα**.
    
    β.  Κάντε κλικ στην επιλογή **νέα**.

9.  Στην ενότητα SAML εισόδου ρυθμίσεις καθολικής, ακολουθήστε τα παρακάτω βήματα:

    ![SAML εισόδου ρυθμίσεις καθολικής] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML εισόδου ρυθμίσεις καθολικής")

    μια.  Στο πλαίσιο κειμένου όνομα, πληκτρολογήστε το όνομα της ρύθμισης παραμέτρων (π.χ.: *SPSSOWAAD\_δοκιμής*).
    
    β.  Στην κλασική πύλη Azure, στη σελίδα διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Salesforce φίλτρου** , αντιγράψτε την τιμή **Διεύθυνσης URL εκδότη** και, στη συνέχεια επικολλήστε το στο πλαίσιο κειμένου **εκδότη** .
    
    c.  Στο πλαίσιο κειμένου **Το αναγνωριστικό οντότητας** , πληκτρολογήστε **https://test.salesforce.com** , εάν πρόκειται για την πρώτη παρουσία Salesforce φίλτρου που προσθέτετε στον κατάλογό σας. Εάν έχετε ήδη προσθέσει μια παρουσία της Salesforce φίλτρου, στη συνέχεια, για τον τύπο **Αναγνωριστικό οντότητας** στο την **Είσοδο στη διεύθυνση URL**, το οποίο πρέπει να είναι σε αυτήν τη μορφή:`http://company.my.salesforce.com`
    
    d.  Κάντε κλικ στο κουμπί **Αναζήτηση** για να αποστείλετε το πιστοποιητικό που έχετε λάβει.
    
    ε.  Ως **Τύπος ταυτότητας SAML**, επιλέξτε **διεκδίκηση περιέχει το Αναγνωριστικό Ομοσπονδία από το αντικείμενο χρήστη**.
    
    f.  Ως **Θέση ταυτότητας SAML**, επιλέξτε **ταυτότητα είναι στο στοιχείο NameIdentifier της πρότασης θέμα**.
    
    γ.  Στην κλασική πύλη Azure, στη σελίδα διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης στο Salesforce φίλτρου** , αντιγράψτε την τιμή **Διεύθυνση URL απομακρυσμένης σύνδεσης** και, στη συνέχεια, επικολλήστε το στο πλαίσιο κειμένου **Διεύθυνση URL σύνδεσης υπηρεσίας παροχής ταυτότητας** .
    
    h.  SFDC δεν υποστηρίζει SAML αποσυνδεθείτε.  Ως λύση, επικολλήστε 'https://login.windows.net/common/wsfederation?wa=wsignout1.0' στο πλαίσιο κειμένου **Διεύθυνση URL αποσυνδεθείτε υπηρεσία παροχής ταυτότητας** .
    
    να.  Ως **Υπηρεσία παροχής που ξεκινούν από αίτηση βιβλιοδεσία**, επιλέξτε **ΔΗΜΟΣΊΕΥΣΗ HTTP**.
    
    j. Κάντε κλικ στην επιλογή **Αποθήκευση**.

10. Στην κλασική Azure πύλη, επιλέξτε την καθολική σύνδεση παραμέτρων επιβεβαίωσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων Καθολικής σύνδεσης** .

    ![Ρύθμιση παραμέτρων Καθολικής σύνδεσης] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Ρύθμιση παραμέτρων Καθολικής σύνδεσης")

##<a name="enabling-your-domain"></a>Ενεργοποίηση του τομέα σας
  
Αυτή η ενότητα προϋποθέτει ότι έχετε ήδη έχετε δημιουργήσει έναν τομέα.  
Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Ορισμός ονόματος τομέα σας](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

###<a name="to-enable-your-domain-perform-the-following-steps"></a>Για να ενεργοποιήσετε τον τομέα σας, ακολουθήστε τα παρακάτω βήματα:

1.  Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Διαχείριση τομέα**και, στη συνέχεια, κάντε κλικ στην επιλογή **μου τομέα.**

    ![Τομέα μου] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Τομέα μου")

    >[AZURE.NOTE]Βεβαιωθείτε ότι τον τομέα σας έχει ρυθμιστεί σωστά.

2.  Στην ενότητα **Ρυθμίσεις σελίδας σύνδεσης** , κάντε κλικ στην επιλογή **Επεξεργασία**, στη συνέχεια, ως **Υπηρεσία ελέγχου ταυτότητας**, επιλέξτε το όνομα της SAML καθολική σύνδεση ρύθμισης από την προηγούμενη ενότητα και τέλος κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Τομέα μου] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Τομέα μου")
  
Μόλις έχετε έναν τομέα που έχει ρυθμιστεί, οι χρήστες πρέπει να χρησιμοποιήσετε τη διεύθυνση URL του τομέα σας για να συνδεθείτε με το φίλτρο Salesforce.  
Για να λάβετε την τιμή της διεύθυνσης URL, κάντε κλικ στο προφίλ SSO που δημιουργήσατε στην προηγούμενη ενότητα.
##<a name="configuring-user-provisioning"></a>Ρύθμιση παραμέτρων προμήθεια του χρήστη
  
Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο για να ενεργοποιήσετε την προμήθεια του χρήστη από λογαριασμούς χρήστη υπηρεσίας καταλόγου Active Directory για να Salesforce φίλτρου.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Για να ρυθμίσετε τις παραμέτρους προμήθεια του χρήστη, εκτελέστε τα ακόλουθα βήματα:

1.  Στην πύλη του Salesforce, στην επάνω γραμμή περιήγησης, επιλέξτε το όνομά σας για να αναπτύξετε το μενού χρήστη:

    ![Ρυθμίσεις μου] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Ρυθμίσεις μου")

2.  Από το μενού χρήστη, επιλέξτε **Ρυθμίσεις μου** για να ανοίξετε τη σελίδα **Μου ρυθμίσεων** .

3.  Στο αριστερό παράθυρο, κάντε κλικ στην επιλογή **προσωπικά** για να αναπτύξετε την ενότητα προσωπικά και, στη συνέχεια, κάντε κλικ στην επιλογή **Επαναφορά μου κωδικό ασφαλείας**:

    ![Ρυθμίσεις μου] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Ρυθμίσεις μου")

4.  Στη σελίδα **Επαναφορά μου κωδικό ασφαλείας** , κάντε κλικ στην επιλογή **Επαναφορά διακριτικού ασφαλείας** για να ζητήσετε από ένα μήνυμα ηλεκτρονικού ταχυδρομείου που περιέχει το διακριτικό ασφαλείας Salesforce.com.

    ![Νέα διακριτικού] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "Νέα διακριτικού")

5.  Ελέγξτε τα Εισερχόμενά σας ηλεκτρονικού ταχυδρομείου για ένα μήνυμα ηλεκτρονικού ταχυδρομείου από το Salesforce.com με "**salesforce.com.com ασφαλείας επιβεβαίωσης**" ως θέμα.

6.  Διαβάστε το μήνυμα ηλεκτρονικού ταχυδρομείου και αντιγράψτε την τιμή διακριτικού ασφαλείας.

7.  Στην κλασική πύλη Azure, στη σελίδα ενοποίησης εφαρμογής **salesforce φίλτρου** , κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων προμήθεια χρήστη** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων προμήθεια του χρήστη** .

    ![Ρύθμιση παραμέτρων χρήστη προμήθειας] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Ρύθμιση παραμέτρων χρήστη προμήθειας")

8.  Στη σελίδα **εισαγάγετε τα διαπιστευτήριά σας Salesforce φίλτρου για να ενεργοποιήσετε την προμήθεια του αυτόματων χρήστη** , δώστε τις ακόλουθες ρυθμίσεις παραμέτρων:

    ![Salesforce φίλτρου] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce φίλτρου")

    μια.  Στο πλαίσιο κειμένου **Όνομα χρήστη διαχειριστή φίλτρου Salesforce** , πληκτρολογήστε ένα όνομα λογαριασμού για την φίλτρου Salesforce που έχει στο προφίλ **Διαχειριστή συστήματος** στο Salesforce.com στους οποίους έχουν ανατεθεί.

    β.  Στο πλαίσιο κειμένου **Κωδικό πρόσβασης διαχειριστή φίλτρου Salesforce** , πληκτρολογήστε τον κωδικό πρόσβασης για αυτόν το λογαριασμό.

    c.  Στο πλαίσιο κειμένου **Διακριτικού ασφαλείας χρήστη** , επικολλήστε την τιμή διακριτικού ασφαλείας.

    d.  Κάντε κλικ στην επιλογή **επικύρωση** για να επαληθεύσετε τις παραμέτρους.

    ε.  Κάντε κλικ στο κουμπί **Επόμενο** για να ανοίξετε τη σελίδα **επιβεβαίωσης** .

9.  Στη σελίδα **επιβεβαίωσης** , κάντε κλικ στην επιλογή **ολοκλήρωσης** για την αποθήκευση της ρύθμισης παραμέτρων.
##<a name="assigning-users"></a>Αντιστοίχιση χρηστών
  
Για να ελέγξετε τη ρύθμιση των παραμέτρων σας, πρέπει να εκχωρήσετε Azure AD χρηστών που θέλετε να επιτρέψετε χρησιμοποιώντας την εφαρμογή έχουν πρόσβαση σε αυτό αναθέτοντας σε αυτά.

###<a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Για να εκχωρήσετε στους χρήστες Salesforce φίλτρου, ακολουθήστε τα παρακάτω βήματα:

1.  Στην κλασική Azure πύλη, δημιουργήστε ένα λογαριασμό δοκιμής.

2.  Στη σελίδα ενοποίησης εφαρμογής **Φίλτρου Salesforce **, κάντε κλικ στην επιλογή **εκχωρήσετε στους χρήστες**.

    ![Αντιστοίχιση χρηστών] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Αντιστοίχιση χρηστών")

3.  Επιλέξτε το χρήστη έλεγχος, κάντε κλικ στην επιλογή **Εκχώρηση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε την ανάθεση.

    ![Ναι] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ναι")
  
Μπορείτε τώρα θα πρέπει να περιμένετε 10 λεπτά και να βεβαιωθείτε ότι ο λογαριασμός έχει συγχρονιστεί σε Salesforce φίλτρου.
  
Εάν θέλετε να ελέγξετε τις ρυθμίσεις καθολικής εισόδου, ανοίξτε τον πίνακα της Access. Για περισσότερες λεπτομέρειες σχετικά με τον πίνακα της Access, ανατρέξτε στο θέμα [Εισαγωγή στο πλαίσιο πρόσβασης](https://msdn.microsoft.com/library/dn308586).