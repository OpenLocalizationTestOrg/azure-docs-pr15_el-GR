<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Ρύθμιση των παραμέτρων εργάσιμης ημέρας για το συγχρονισμό της εισερχόμενης | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε εργάσιμης ημέρας ως προέλευση δεδομένων ταυτότητας για Azure Active Directory." 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />


#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Πρόγραμμα εκμάθησης: Ρύθμιση των παραμέτρων εργάσιμης ημέρας για το συγχρονισμό της εισερχομένων


Είναι ο στόχος αυτού του προγράμματος εκμάθησης για να βλέπετε τα βήματα που πρέπει να εκτελέσετε στο εργάσιμης ημέρας και Azure AD για να εισαγάγετε επαφές από εργάσιμης ημέρας στο Azure AD. 

Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη τα ακόλουθα στοιχεία:

-   Μια έγκυρη συνδρομή Azure AD Premium
-   Ένας μισθωτής στη συνάρτηση Workday
  
Το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης αποτελείται από τα εξής δομικά στοιχεία:

1. Ενεργοποίηση την ενοποίηση εφαρμογής για εργάσιμης ημέρας 


2. Δημιουργία ενός χρήστη συστήματος ενοποίησης 


3. Δημιουργία μιας ομάδας ασφαλείας 


4. Εκχώρηση χρήστη ενοποίησης συστήματος για την ομάδα ασφαλείας 


5. Ρύθμιση παραμέτρων επιλογών ομάδας ασφαλείας 


6. Ενεργοποίηση αλλαγών πολιτική ασφαλείας 


7. Ρύθμιση των παραμέτρων εισαγωγής χρήστη στο Azure AD 



##<a name="enabling-the-application-integration-for-workday"></a>Ενεργοποίηση την ενοποίηση εφαρμογής για εργάσιμης ημέρας
  
Είναι ο στόχος αυτής της ενότητας για να εφαρμόσετε διάρθρωση πώς μπορείτε να ενεργοποιήσετε την ενοποίηση εφαρμογής για εργάσιμης ημέρας.

### <a name="steps"></a>Τα βήματα:

1.  Στην κλασική πύλη Azure, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory] (./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "Υπηρεσία καταλόγου Active Directory")

2.  Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3.  Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές] (./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "Εφαρμογές")

4.  Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Προσθήκη εφαρμογής] (./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "Προσθήκη εφαρμογής")

  
5. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **εργάσιμης ημέρας**.

    ![Προσθήκη εφαρμογής από gallerry] (./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "Προσθήκη εφαρμογής από gallerry")

6. Στο παράθυρο αποτελεσμάτων, επιλέξτε εργάσιμης ημέρας και, στη συνέχεια, κάντε κλικ στην επιλογή ολοκλήρωση, για να προσθέσετε την εφαρμογή.

    ![Συλλογή εφαρμογών] (./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "Συλλογή εφαρμογών")





## <a name="creating-an-integration-system-user"></a>Δημιουργία ενός χρήστη συστήματος ενοποίησης

### <a name="steps"></a>Τα βήματα:


1. Στη **Διάθεσή σας εργάσιμης ημέρας**, πληκτρολογήστε Δημιουργία χρήστη στο πλαίσιο αναζήτησης και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία ενοποίηση συστήματος χρήστη**. 

    ![Δημιουργία χρήστη] (./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Δημιουργία χρήστη")



2. Ολοκλήρωση της εργασίας **Create User συστήματος ενοποίησης** , παρέχοντας ένα όνομα χρήστη και τον κωδικό πρόσβασης για έναν νέο χρήστη ενοποίηση του συστήματος.  Αφήστε το απαιτούν νέο κωδικό πρόσβασης στο επόμενο Sign In επιλογή απενεργοποιημένο, επειδή αυτός ο χρήστης θα είναι σύνδεση μέσω προγραμματισμού. Αφήστε τα λεπτά χρονικού ορίου περίοδο λειτουργίας με την προεπιλεγμένη τιμή του 0, η οποία θα αποτρέψει περίοδοι λειτουργίας του χρήστη περιόδου πρόωρα. 

    ![Δημιουργία χρήστη συστήματος ενοποίησης] (./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Δημιουργία χρήστη συστήματος ενοποίησης")
 




## <a name="creating-a-security-group"></a>Δημιουργία μιας ομάδας ασφαλείας

Για το σενάριο που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, πρέπει να δημιουργήσετε μια ομάδα ασφαλείας του συστήματος Καταστήστε ενοποίηση και να εκχωρήσετε στο χρήστη σε αυτήν.

### <a name="steps"></a>Τα βήματα:

1. Εισαγάγετε Δημιουργία ομάδας ασφαλείας στο πλαίσιο αναζήτησης και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία ομάδας ασφαλείας**. 

    ![Ομάδα CreateSecurity] (./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "Ομάδα CreateSecurity")
 

2. Ολοκλήρωση της εργασίας Δημιουργία ομάδας ασφαλείας.  Επιλέξτε ομάδα ασφαλείας συστήματος ενοποίησης — απεριόριστη από την αναπτυσσόμενη λίστα Τύπος ομάδας ασφαλείας μισθωμένη, για να δημιουργήσετε μια ομάδα ασφαλείας στην οποία θα είναι ρητά Προσθήκη μελών. 

    ![Ομάδα CreateSecurity] (./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "Ομάδα CreateSecurity")
 



## <a name="assigning-the-integration-system-user-to-the-security-group"></a>Εκχώρηση χρήστη ενοποίησης συστήματος για την ομάδα ασφαλείας

### <a name="steps"></a>Τα βήματα:


1. Εισαγάγετε Επεξεργασία ομάδας ασφαλείας στο πλαίσιο αναζήτησης και, στη συνέχεια, κάντε κλικ στην επιλογή **Επεξεργασία ομάδας ασφαλείας**. 

    ![Επεξεργασία ομάδας ασφαλείας] (./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Επεξεργασία ομάδας ασφαλείας")
 
 

2. Αναζητήστε και επιλέξτε τη νέα ομάδα ασφαλείας ενοποίηση με βάση το όνομα. 

    ![Επεξεργασία ομάδας ασφαλείας] (./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Επεξεργασία ομάδας ασφαλείας")

 

3. Προσθήκη νέου χρήστη συστήματος ενοποίησης για τη νέα ομάδα ασφαλείας. 

    ![Ομάδα ασφαλείας συστήματος] (./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Ομάδα ασφαλείας συστήματος")  




## <a name="configuring-security-group-options"></a>Ρύθμιση παραμέτρων επιλογών ομάδας ασφαλείας

Σε αυτό το βήμα, μπορείτε να εκχωρήσετε σε τα νέα δικαιώματα ομάδα ασφαλείας για να **μεταφέρετε** και **Τοποθέτηση** λειτουργίες στα αντικείμενα ασφαλές από τις παρακάτω πολιτικές ασφαλείας τομέα:

- Προμήθεια του εξωτερικού λογαριασμού

- Εργαζόμενου δεδομένων: Αναφορές δημόσια εργαζόμενου

- Εργασίας δεδομένων: Όλες οι θέσεις

- Δεδομένα εργαζόμενου: Τρέχοντα προσωπικού πληροφορίες

- Εργασίας δεδομένα: Τίτλος επιχειρήσεις εργαζόμενου προφίλ

 
### <a name="steps"></a>Τα βήματα:

1. Εισαγάγετε πολιτικές ασφαλείας τομέα στο πλαίσιο αναζήτησης και, στη συνέχεια, κάντε κλικ στη σύνδεση, πολιτικές ασφαλείας τομέα για λειτουργική περιοχή.  

    ![Πολιτικές ασφαλείας τομέα] (./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Πολιτικές ασφαλείας τομέα")  
 

2. Αναζήτηση για το σύστημα και επιλέξτε την περιοχή λειτουργικό **σύστημα** .  Κάντε κλικ στο **κουμπί OK**.  

    ![Πολιτικές ασφαλείας τομέα] (./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Πολιτικές ασφαλείας τομέα")  


3. Στη λίστα των πολιτικών ασφάλειας για την περιοχή λειτουργικό σύστημα, αναπτύξτε το στοιχείο Διαχείριση ασφαλείας και επιλέξτε την πολιτική ασφαλείας τομέα, εξωτερική προμήθεια του λογαριασμού.  

    ![Πολιτικές ασφαλείας τομέα] (./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Πολιτικές ασφαλείας τομέα")  


4. Κάντε κλικ στην επιλογή **Επεξεργασία δικαιωμάτων**και, στη συνέχεια, στη σελίδα του παραθύρου διαλόγου **Επεξεργασία δικαιωμάτων**, προσθέστε τη νέα ομάδα ασφαλείας στη λίστα των ομάδων ασφαλείας με δικαιώματα ενοποίησης **γρήγορα** και να **τοποθετήσετε** . 

    ![Επεξεργασία δικαιωμάτων] (./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Επεξεργασία δικαιωμάτων")  

 

5. Επαναλάβετε το βήμα 1, παραπάνω, για να επιστρέψετε στην οθόνη για την επιλογή λειτουργικές περιοχές, και αυτήν τη στιγμή, αναζητήστε στελέχωσης, επιλέξτε την περιοχή λειτουργική προσωπικό και κάντε κλικ στο κουμπί **OK**.

    ![Πολιτικές ασφαλείας τομέα] (./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Πολιτικές ασφαλείας τομέα")  
 

6. Στη λίστα των πολιτικών ασφάλειας για τη λειτουργική περιοχή προσωπικό, αναπτύξτε το στοιχείο εργαζόμενου δεδομένων: προσωπικό και επαναλάβετε το βήμα 4 παραπάνω για κάθε μία από αυτές τις υπόλοιπες πολιτικές ασφαλείας:

     - Εργαζόμενου δεδομένων: Αναφορές δημόσια εργαζόμενου

     - Εργασίας δεδομένων: Όλες οι θέσεις

     - Δεδομένα εργαζόμενου: Τρέχοντα προσωπικού πληροφορίες

     - Εργασίας δεδομένα: Τίτλος επιχειρήσεις εργαζόμενου προφίλ


    ![Πολιτικές ασφαλείας τομέα] (./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Πολιτικές ασφαλείας τομέα")  







## <a name="activating-security-policy-changes"></a>Ενεργοποίηση αλλαγών πολιτική ασφαλείας

### <a name="steps"></a>Τα βήματα:

1. Εισαγάγετε ενεργοποιήσετε στο πλαίσιο αναζήτησης και, στη συνέχεια, κάντε κλικ στη σύνδεση, ενεργοποίηση αλλαγές πολιτικής σε εκκρεμότητα ασφαλείας. 

    ![Ενεργοποίηση] (./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Ενεργοποίηση") 
 

2. Ξεκινήστε την εργασία Ενεργοποίηση αλλαγές πολιτικής σε εκκρεμότητα ασφαλείας, εισάγοντας ένα σχόλιο για σκοπούς ελέγχου και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**. 

    ![Ενεργοποίηση σε εκκρεμότητα ασφαλείας] (./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Ενεργοποίηση σε εκκρεμότητα ασφαλείας")   
 

3. Ολοκλήρωση της εργασίας στην επόμενη οθόνη, επιλέγοντας το πλαίσιο ελέγχου επισήμανση για επιβεβαίωση και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**. 

    ![Ενεργοποίηση σε εκκρεμότητα ασφαλείας] (./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Ενεργοποίηση σε εκκρεμότητα ασφαλείας")  





## <a name="configuring-user-import-in-azure-ad"></a>Ρύθμιση των παραμέτρων εισαγωγής χρήστη στο Azure AD

Στόχος αυτής της ενότητας είναι να περιγράψει τον τρόπο ρύθμισης παραμέτρων Azure AD για να εισαγάγετε επαφές από εργάσιμης ημέρας.


### <a name="steps"></a>Τα βήματα:


1. Στη σελίδα ενοποίησης εφαρμογής **εργάσιμης ημέρας** , κάντε κλικ στην επιλογή **Εισαγωγή ρύθμιση παραμέτρων χρήστη** για να ανοίξετε το παράθυρο διαλόγου **Ρύθμιση παραμέτρων προμήθειας** .


2. Στη σελίδα **ρυθμίσεων και διαπιστευτήρια διαχειριστή** , ακολουθήστε τα παρακάτω βήματα και, στη συνέχεια, κάντε κλικ στην επιλογή **Επόμενο**: 

    ![Ρυθμίσεις και διαπιστευτήρια διαχειριστή] (./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "Ρυθμίσεις και διαπιστευτήρια διαχειριστή")  
 
    μια. Στο πλαίσιο κειμένου όνομα Workday διαχείρισης χρήστη, πληκτρολογήστε το όνομα του χρήστη που έχετε δημιουργήσει στις τη δημιουργία μια ενότητα ενοποίηση συστήματος χρήστη.

    β. Στο πλαίσιο κειμένου κωδικός πρόσβασης διαχειριστή εργάσιμης ημέρας, πληκτρολογήστε τον κωδικό πρόσβασης του χρήστη που έχετε δημιουργήσει στις τη δημιουργία μια ενότητα ενοποίηση συστήματος χρήστη.

    c. Στο πλαίσιο κειμένου διεύθυνση URL Workday μισθωτή, πληκτρολογήστε τη διεύθυνση URL ή το μισθωτή εργάσιμης ημέρας.


3. Στη σελίδα **Δοκιμή σύνδεσης** , κάντε κλικ στην επιλογή **Έναρξη δοκιμή** για να επιβεβαιώσετε τη σύνδεση και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. 

    ![Δοκιμή σύνδεσης] (./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "Δοκιμή σύνδεσης")  
 

4. Στη σελίδα επιλογών **Provisioning** , κάντε κλικ στο κουμπί **Επόμενο**. 

    ![Επιλογές προετοιμασίας] (./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "Επιλογές προετοιμασίας")


5. Στο παράθυρο διαλόγου **Έναρξη προμήθειας** , κάντε κλικ στην επιλογή **ολοκληρώθηκε**. 

    ![Έναρξη προμήθειας] (./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "Έναρξη προμήθειας")
 

Τώρα, μπορείτε να μεταβείτε στην ενότητα **χρήστες** και να ελέγξετε εάν το χρήστη συνάρτηση Workday έχει εισαχθεί.



## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)
