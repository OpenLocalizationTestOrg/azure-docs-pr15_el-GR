<properties
    pageTitle="Προαπαιτούμενα στοιχεία για να αποκτήσετε πρόσβαση του Azure AD αναφοράς API. | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με τις προϋποθέσεις για να αποκτήσετε πρόσβαση του Azure AD αναφοράς API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Προαπαιτούμενα στοιχεία για να αποκτήσετε πρόσβαση του Azure AD αναφοράς API 

Το [Azure AD αναφοράς APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) παρέχει πρόσβαση μέσω προγραμματισμού στα δεδομένα μέσω ένα σύνολο βάσει REST API. Μπορείτε να καλέσετε αυτά τα API από μια ποικιλία προγραμματισμού γλώσσες και τα εργαλεία.

Το API αναφοράς χρησιμοποιεί [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) ώστε να επιτρέπουν την πρόσβαση στο web APIs. 

Για να προετοιμάσετε την πρόσβασή σας για το API αναφοράς, πρέπει:

1. Δημιουργία μιας εφαρμογής στο μισθωτή του Azure AD 

2. Εκχωρήστε τα κατάλληλα δικαιώματα εφαρμογής για να αποκτήσετε πρόσβαση στα δεδομένα του Azure AD

3. Συλλογή ρυθμίσεων παραμέτρων από τον κατάλογο

Για ερωτήσεις, θέματα ή σχόλια, επικοινωνήστε με την [Αναφορά συμβάλλει AAD](mailto:aadreportinghelp@microsoft.com).


## <a name="create-an-azure-ad-application"></a>Δημιουργία μιας εφαρμογής Azure AD

Για να ρυθμίσετε τις παραμέτρους του καταλόγου σας για να αποκτήσετε πρόσβαση του Azure AD αναφοράς API, πρέπει να εισέλθετε στην πύλη του Azure κλασική με ένα λογαριασμό διαχειριστή Azure συνδρομή που είναι επίσης ένα μέλος του ρόλου καθολικού διαχειριστή καταλόγου στο μισθωτή του Azure AD.

> [AZURE.IMPORTANT] Εφαρμογές που εκτελούνται στην περιοχή τα διαπιστευτήρια με δικαιώματα "Διαχείριση", όπως αυτό μπορεί να είναι πολύ ισχυρή, επομένως βεβαιωθείτε ότι έχετε για να διατηρήσει ασφαλείς διαπιστευτήρια Αναγνωριστικό/μυστικό της εφαρμογής.


1. Στην [πύλη του Azure κλασική](https://manage.windowsazure.com), στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Από τη λίστα της **υπηρεσίας καταλόγου active directory** , επιλέξτε τον κατάλογο.

3. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **εφαρμογές**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Στην κάτω γραμμή, κάντε κλικ στην επιλογή **Προσθήκη**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/03.png) 

5. Στην το **Τι θέλετε να κάνετε;** παράθυρο διαλόγου, κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής ανάπτυξη την εταιρεία μου**. 

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/04.png) 


6. Στο παράθυρο διαλόγου **πείτε μας σχετικά με την εφαρμογή σας** , ακολουθήστε τα παρακάτω βήματα: 

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/05.png) 

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε ένα όνομα (π.χ.: εφαρμογή API αναφοράς).

    β. Επιλέξτε **την εφαρμογή Web ή/και το API web**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.


7. Στο παράθυρο διαλόγου **Ιδιότητες εφαρμογής** , ακολουθήστε τα παρακάτω βήματα: 

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/06.png) 

    μια. Στο πλαίσιο κειμένου **εισόδου στη διεύθυνση URL** , πληκτρολογήστε `https://localhost`.

    β. Στο πλαίσιο κειμένου **URI Αναγνωριστικό εφαρμογής** , πληκτρολογήστε ```https://localhost/ReportingApiApp```.

    c. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.



## <a name="grant-your-application-permission-to-use-the-api"></a>Εκχωρείτε το δικαίωμα εφαρμογής για να χρησιμοποιήσετε το API

1. Στην [πύλη του Azure κλασική](https://manage.windowsazure.com/), στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Από τη λίστα της **υπηρεσίας καταλόγου active directory** , επιλέξτε τον κατάλογο.

3. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **εφαρμογές**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/02.png)


3. Στη λίστα εφαρμογών, επιλέξτε την εφαρμογή που μόλις δημιουργήθηκε.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/07.png)

4. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/08.png)


5. Στην ενότητα **δικαιώματα σε άλλες εφαρμογές** , για τον πόρο **Azure Active Directory** , κάντε κλικ στην αναπτυσσόμενη λίστα **Δικαιωμάτων εφαρμογών** και, στη συνέχεια, επιλέξτε **Ανάγνωση καταλόγου δεδομένων**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/09.png)


5. Στην κάτω γραμμή, κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/10.png)


## <a name="gather-configuration-settings-from-your-directory"></a>Συλλογή ρυθμίσεων παραμέτρων από τον κατάλογο

Αυτή η ενότητα σας δείχνει πώς μπορείτε να λάβετε τις ακόλουθες ρυθμίσεις από τον κατάλογο:

- Το όνομα του τομέα
- Αναγνωριστικό υπολογιστή-πελάτη
- Μυστικό προγράμματος-πελάτη

Χρειάζεστε αυτές τις τιμές κατά τη ρύθμιση παραμέτρων κλήσεων για το API αναφοράς. 


### <a name="get-your-domain-name"></a>Λάβετε το όνομα τομέα σας

1. Στην [πύλη του Azure κλασική](https://manage.windowsazure.com), στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Από τη λίστα της **υπηρεσίας καταλόγου active directory** , επιλέξτε τον κατάλογο.

3. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **τομείς**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/11.png) 

4. Στη στήλη **Όνομα τομέα** , αντιγράψτε το όνομα τομέα σας.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/12.png) 


### <a name="get-the-applications-client-id"></a>Λήψη της εφαρμογής Αναγνωριστικό υπολογιστή-πελάτη

1. Στην [πύλη του Azure κλασική](https://manage.windowsazure.com), στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Από τη λίστα της **υπηρεσίας καταλόγου active directory** , επιλέξτε τον κατάλογο.

3. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **εφαρμογές**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Στη λίστα εφαρμογών, επιλέξτε την εφαρμογή που μόλις δημιουργήθηκε.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/07.png)

4. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/08.png)

4. Αντιγράψτε το **Αναγνωριστικό υπολογιστή-πελάτη**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/13.png)


### <a name="get-the-applications-client-secret"></a>Λήψη της εφαρμογής υπολογιστή-πελάτη μυστικό

Για να λάβετε την εφαρμογή υπολογιστή-πελάτη μυστικό, πρέπει να δημιουργήσετε ένα νέο αριθμό-κλειδί και να αποθηκεύσετε την τιμή κατά την αποθήκευση του νέου αριθμού-κλειδιού, επειδή δεν είναι δυνατή για να ανακτήσετε αργότερα πλέον αυτήν την τιμή.

1. Στην [πύλη του Azure κλασική](https://manage.windowsazure.com), στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Από τη λίστα της **υπηρεσίας καταλόγου active directory** , επιλέξτε τον κατάλογο.

3. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **εφαρμογές**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Στη λίστα εφαρμογών, επιλέξτε την εφαρμογή που μόλις δημιουργήθηκε.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/07.png)

4. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/08.png)

5. Στην ενότητα **πλήκτρα** , ακολουθήστε τα παρακάτω βήματα: 

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/14.png)

    μια. Από τη λίστα διάρκεια, επιλέξτε μια διάρκεια

    β. Στην κάτω γραμμή, κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Καταχώρηση εφαρμογής](./media/active-directory-reporting-api-prerequisites/10.png)

    c. Αντιγράψτε την τιμή του κλειδιού.

## <a name="next-steps"></a>Επόμενα βήματα

- Θα θέλατε να αποκτήσετε πρόσβαση στα δεδομένα από το Azure AD αναφοράς API σε μέσω προγραμματισμού, Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure Active Directory αναφοράς API](active-directory-reporting-api-getting-started.md).

- Εάν θέλετε να μάθετε περισσότερα σχετικά με τη δημιουργία αναφορών Azure Active Directory, ανατρέξτε στο θέμα [Azure Active Directory αναφοράς Οδηγός](active-directory-reporting-guide.md).  
