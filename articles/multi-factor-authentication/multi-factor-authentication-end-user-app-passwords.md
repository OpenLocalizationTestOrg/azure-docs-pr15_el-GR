<properties
    pageTitle="Τι είναι η εφαρμογή τους κωδικούς πρόσβασης στο Azure MFA;"
    description="Αυτή η σελίδα θα σας βοηθήσει τους χρήστες να κατανοήσουν τι είναι η εφαρμογή κωδικών πρόσβασης και τι χρησιμοποιούνται για με υπόψη Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>



# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Τι είναι η εφαρμογή τους κωδικούς πρόσβασης στο Azure έλεγχο ταυτότητας πολλών παραγόντων;

Ορισμένες εφαρμογές προγραμμάτων περιήγησης, όπως το πρόγραμμα-πελάτη ηλεκτρονικού ταχυδρομείου εγγενούς Apple που χρησιμοποιεί το Exchange Active Sync, προς το παρόν δεν υποστηρίζει έλεγχο ταυτότητας πολλών παραγόντων. Έλεγχος ταυτότητας πολλών παραγόντων είναι ενεργοποιημένη ανά χρήστη. Αυτό σημαίνει ότι εάν ένας χρήστης έχει ενεργοποιηθεί για έλεγχο ταυτότητας πολλών παραγόντων και προσπαθούν να χρησιμοποιήσετε τις εφαρμογές προγραμμάτων περιήγησης, θα είναι δυνατό να το κάνετε. Κωδικός πρόσβασης εφαρμογή σας επιτρέπει να συμβεί αυτό.

>[AZURE.NOTE] Σύγχρονος έλεγχος ταυτότητας για τα προγράμματα-πελάτες του Office 2013
>
> Τώρα, προγράμματα-πελάτες του Office 2013 (συμπεριλαμβανομένου του Outlook) υποστηρίζει νέα πρωτόκολλα ελέγχου ταυτότητας και μπορεί να ενεργοποιηθεί για να υποστηρίζει έλεγχο ταυτότητας πολλών παραγόντων.  Αυτό σημαίνει ότι μόλις ενεργοποιηθεί, εφαρμογή τους κωδικούς πρόσβασης δεν είναι απαραίτητα για χρήση με προγράμματα-πελάτες του Office 2013.  Για περισσότερες πληροφορίες ανατρέξτε στο θέμα [Office 2013 Σύγχρονος έλεγχος ταυτότητας δημόσια προεπισκόπηση ανακοινώθηκε](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

## <a name="how-to-use-app-passwords"></a>Πώς μπορείτε να χρησιμοποιήσετε τους κωδικούς πρόσβασης εφαρμογής

Ακολουθούν ορισμένα πράγματα που πρέπει να θυμάστε σχετικά με τη χρήση κωδικών πρόσβασης εφαρμογής.

- Ο κωδικός πρόσβασης πραγματική δημιουργείται αυτόματα και δεν παρέχεται από το χρήστη. Αυτό συμβαίνει επειδή είναι δυσκολότερη ένας εισβολέας να μαντέψει τον κωδικό πρόσβασης που δημιουργούνται αυτόματα, και είναι πιο ασφαλή.
- Αυτήν τη στιγμή υπάρχει όριο 40 τους κωδικούς πρόσβασης ανά χρήστη. Εάν προσπαθήσετε να δημιουργήσετε ένα αφού έχετε φτάσει το όριο, θα σας ζητηθεί να διαγράψετε έναν από τους κωδικούς πρόσβασής σας υπάρχουσα εφαρμογή για να δημιουργήσετε ένα νέο.
- Συνιστάται να τη δημιουργία εφαρμογής τους κωδικούς πρόσβασης ανά συσκευή και όχι ανά εφαρμογή. Για παράδειγμα, μπορείτε να δημιουργήσετε έναν κωδικό πρόσβασης της εφαρμογής για το φορητό υπολογιστή σας και να χρησιμοποιήσετε τον κωδικό πρόσβασης της εφαρμογής για όλες τις εφαρμογές σας στο συγκεκριμένο φορητό υπολογιστή.
- Σας δίνεται μια εφαρμογή κωδικού πρόσβασης την πρώτη φορά που εισόδου.  Εάν χρειάζεστε επιπλέον αυτά, μπορείτε να δημιουργήσετε τους.

![Το πρόγραμμα εγκατάστασης](./media/multi-factor-authentication-end-user-app-passwords/app.png)

Όταν έχετε μια εφαρμογή κωδικού πρόσβασης, μπορείτε χρησιμοποιήσετε στη θέση τον αρχικό κωδικό πρόσβασης με αυτές τις εφαρμογές προγραμμάτων περιήγησης.  Επομένως, για παράδειγμα, εάν χρησιμοποιείτε τον έλεγχο ταυτότητας πολλών παραγόντων και το πρόγραμμα-πελάτη ηλεκτρονικού ταχυδρομείου εγγενούς Apple στο τηλέφωνό σας.  Χρησιμοποιήστε τον κωδικό πρόσβασης της εφαρμογής, έτσι ώστε να μπορούν να παρακάμψει έλεγχο ταυτότητας πολλών παραγόντων και να συνεχίσετε να εργάζεστε.

## <a name="creating-and-deleting-app-passwords"></a>Δημιουργία και διαγραφή εφαρμογής κωδικών πρόσβασης
Κατά την αρχική εισόδου σας δίνεται έναν κωδικό πρόσβασης εφαρμογή που μπορείτε να χρησιμοποιήσετε.  Επιπλέον μπορείτε επίσης να δημιουργήσετε και να διαγράψετε αργότερα εφαρμογή κωδικών πρόσβασης.  Πώς να κάνετε αυτό εξαρτάται από το πώς μπορείτε να χρησιμοποιήσετε τον έλεγχο ταυτότητας πολλών παραγόντων.  Επιλέξτε το ώστε το μεγαλύτερο ισχύει για εσάς.

Πώς μπορείτε να χρησιμοποιήσετε authentiation πολλαπλών παραγόντων|Περιγραφή
:------------- | :------------- |
[Να χρησιμοποιήσω με το Office 365](#creating-and-deleting-app-passwords-with-office-365)|  Αυτό σημαίνει ότι θα θέλετε να δημιουργήσετε εφαρμογής τους κωδικούς πρόσβασης μέσω της πύλης του Office 365.
[Δεν ξέρω](#creating-and-deleting-app-passwords-with-myapps-portal)|Αυτό σημαίνει ότι θα θέλετε να δημιουργήσετε εφαρμογής τους κωδικούς πρόσβασης μέσω [https://myapps.microsoft.com](https://myapps.microsoft.com)
[Να χρησιμοποιήσω με το Microsoft Azure](#create-app-passwords-in-the-azure-portal)| Αυτό σημαίνει ότι θα θέλετε να δημιουργήσετε εφαρμογής τους κωδικούς πρόσβασης μέσω της πύλης Azure.

## <a name="creating-and-deleting-app-passwords-with-office-365"></a>Δημιουργία και διαγραφή τους κωδικούς πρόσβασης εφαρμογή με το Office 365

Εάν χρησιμοποιείτε τον έλεγχο ταυτότητας πολλών παραγόντων με το Office 365 που θα θέλετε να δημιουργήσετε και να διαγράψετε τους κωδικούς πρόσβασης εφαρμογή μέσω της πύλης του Office 365.

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Για να δημιουργήσετε εφαρμογής τους κωδικούς πρόσβασης στην πύλη του Office 365
--------------------------------------------------------------------------------

1. Συνδεθείτε στην [πύλη του Office 365](https://login.microsoftonline.com/).
2. Στην επάνω δεξιά γωνία, επιλέξτε το widget και επιλέξτε ρυθμίσεις του Office 365.
3. Κάντε κλικ στην επιλογή επαλήθευση πρόσθετη ασφάλεια.
4. Στα δεξιά, κάντε κλικ στη σύνδεση που αναφέρει ότι **Ενημέρωση οι αριθμοί τηλεφώνου μου χρησιμοποιούνται για την ασφάλεια λογαριασμού.** 
 ![Εγκατάστασης](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Αυτό θα σας μεταφέρει στη σελίδα που θα σας επιτρέψει να αλλάξετε τις ρυθμίσεις σας.
![Cloud](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Στο επάνω μέρος, δίπλα στην επιλογή επαλήθευση πρόσθετη ασφάλεια, κάντε κλικ στην **εφαρμογή κωδικών πρόσβασης.**
7. Κάντε κλικ στην επιλογή **Δημιουργία**.
![Cloud](./media/multi-factor-authentication-end-user-app-passwords-create-o365/apppass.png)
8. Πληκτρολογήστε ένα όνομα για τον κωδικό πρόσβασης της εφαρμογής και κάντε κλικ στο κουμπί **Επόμενο**.
![Δημιουργία μιας εφαρμογής κωδικού πρόσβασης](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
9. Αντιγράψτε τον κωδικό πρόσβασης της εφαρμογής στο Πρόχειρο και επικολλήστε την σε εφαρμογή σας.
![Δημιουργία μιας εφαρμογής κωδικού πρόσβασης](./media/multi-factor-authentication-end-user-app-passwords/create2.png)


### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>Για να διαγράψετε εφαρμογή κωδικών πρόσβασης με την πύλη του Office 365
--------------------------------------------------------------------------------


1. Συνδεθείτε στην [πύλη του Office 365](https://login.microsoftonline.com/).
2. Στην επάνω δεξιά γωνία, επιλέξτε το widget και επιλέξτε ρυθμίσεις του Office 365.
3. Κάντε κλικ στην επιλογή επαλήθευση πρόσθετη ασφάλεια.
4. Στα δεξιά, κάντε κλικ στη σύνδεση που αναφέρει ότι **Ενημέρωση οι αριθμοί τηλεφώνου μου χρησιμοποιούνται για την ασφάλεια λογαριασμού.** 
 ![Εγκατάστασης](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Αυτό θα σας μεταφέρει στη σελίδα που θα σας επιτρέψει να αλλάξετε τις ρυθμίσεις σας.
![Cloud](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Στο επάνω μέρος, δίπλα στην επιλογή επαλήθευση πρόσθετη ασφάλεια, κάντε κλικ στην **εφαρμογή κωδικών πρόσβασης.**
7. Δίπλα στο στοιχείο που θέλετε να διαγράψετε τον κωδικό πρόσβασης εφαρμογής, κάντε κλικ στην επιλογή **Διαγραφή**.
![Διαγραφή ενός κωδικού πρόσβασης εφαρμογής](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
8. Επιβεβαιώστε τη διαγραφή, κάνοντας κλικ στην επιλογή **Ναι**.
![Επιβεβαιώστε τη διαγραφή](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
9. Όταν ο κωδικός πρόσβασης εφαρμογή διαγράφεται μπορείτε να επιλέξετε **Κλείσιμο**.
![Κλείσιμο](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="creating-and-deleting-app-passwords-with-myapps-portal"></a>Δημιουργία και διαγραφή εφαρμογής τους κωδικούς πρόσβασης με την πύλη στοιχείο οι εφαρμογές μου.
Εάν δεν είστε βέβαιοι πώς μπορείτε να χρησιμοποιήσετε τον έλεγχο ταυτότητας πολλών παραγόντων, στη συνέχεια, μπορείτε να πάντα δημιουργήσετε και διαγράψτε τους κωδικούς πρόσβασης εφαρμογή μέσω της πύλης στοιχείο οι εφαρμογές μου.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Για να δημιουργήσετε μια εφαρμογή τον κωδικό πρόσβασης, με την πύλη στοιχείο οι εφαρμογές μου

1. Είσοδος στο [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Στο επάνω μέρος, επιλέξτε το προφίλ.
3. Επιλέξτε επαλήθευση πρόσθετη ασφάλεια.
![Cloud](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Αυτό θα σας μεταφέρει στη σελίδα που θα σας επιτρέψει να αλλάξετε τις ρυθμίσεις σας.
![Το πρόγραμμα εγκατάστασης](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Στο επάνω μέρος, δίπλα στην επιλογή επαλήθευση πρόσθετη ασφάλεια, κάντε κλικ στην **εφαρμογή κωδικών πρόσβασης.**
6. Κάντε κλικ στην επιλογή **Δημιουργία**.
![Δημιουργία μιας εφαρμογής κωδικού πρόσβασης](./media/multi-factor-authentication-end-user-app-passwords/create3.png)
7. Πληκτρολογήστε ένα όνομα για τον κωδικό πρόσβασης της εφαρμογής και κάντε κλικ στο κουμπί **Επόμενο**.
![Δημιουργία μιας εφαρμογής κωδικού πρόσβασης](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
8. Αντιγράψτε τον κωδικό πρόσβασης της εφαρμογής στο Πρόχειρο και επικολλήστε την σε εφαρμογή σας.
![Δημιουργία μιας εφαρμογής κωδικού πρόσβασης](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Για να διαγράψετε μια εφαρμογή τον κωδικό πρόσβασης, με την πύλη στοιχείο οι εφαρμογές μου

1. Είσοδος στο [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Στο επάνω μέρος, επιλέξτε το προφίλ.
3. Επιλέξτε επαλήθευση πρόσθετη ασφάλεια.
![Cloud](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Αυτό θα σας μεταφέρει στη σελίδα που θα σας επιτρέψει να αλλάξετε τις ρυθμίσεις σας.
![Το πρόγραμμα εγκατάστασης](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Στο επάνω μέρος, δίπλα στην επιλογή επαλήθευση πρόσθετη ασφάλεια, κάντε κλικ στην **εφαρμογή κωδικών πρόσβασης.**
6. Δίπλα στο στοιχείο που θέλετε να διαγράψετε τον κωδικό πρόσβασης εφαρμογής, κάντε κλικ στην επιλογή **Διαγραφή**.
![Διαγραφή ενός κωδικού πρόσβασης εφαρμογής](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
7. Επιβεβαιώστε τη διαγραφή, κάνοντας κλικ στην επιλογή **Ναι**.
![Επιβεβαιώστε τη διαγραφή](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
8. Όταν ο κωδικός πρόσβασης εφαρμογή διαγράφεται μπορείτε να επιλέξετε **Κλείσιμο**.
![Κλείσιμο](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="create-app-passwords-in-the-azure-portal"></a>Δημιουργία εφαρμογής κωδικών πρόσβασης στην πύλη του Azure

Εάν χρησιμοποιείτε τον έλεγχο ταυτότητας πολλών παραγόντων με Azure που θα θέλετε να δημιουργήσετε εφαρμογής τους κωδικούς πρόσβασης μέσω της πύλης Azure.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Για να δημιουργήσετε εφαρμογής τους κωδικούς πρόσβασης στην πύλη του Azure

1. Πραγματοποιήστε είσοδο στην πύλη διαχείρισης Azure.
2. Στο επάνω μέρος, κάντε δεξί κλικ στο όνομα χρήστη σας και επιλέξτε πρόσθετες επαλήθευσης ασφαλείας.
3. Στη σελίδα proofup, στο επάνω μέρος, επιλέξτε εφαρμογή κωδικών πρόσβασης
4. Κάντε κλικ στην επιλογή **Δημιουργία**.
5. Πληκτρολογήστε ένα όνομα για την εφαρμογή κωδικού πρόσβασης και κάντε κλικ στο κουμπί **Επόμενο**
6. Αντιγράψτε τον κωδικό πρόσβασης της εφαρμογής στο Πρόχειρο και επικολλήστε την σε εφαρμογή σας.
![Cloud](./media/multi-factor-authentication-end-user-app-passwords-create-azure/app2.png)

### <a name="to-delete-app-passwords-in-the-azure-portal"></a>Για να διαγράψετε τους κωδικούς πρόσβασης εφαρμογής στην πύλη του Azure

1. Πραγματοποιήστε είσοδο στην πύλη διαχείρισης Azure.
2. Στο επάνω μέρος, κάντε δεξί κλικ στο όνομα χρήστη σας και επιλέξτε πρόσθετες επαλήθευσης ασφαλείας.
3. Στο επάνω μέρος, δίπλα στην επιλογή επαλήθευση πρόσθετη ασφάλεια, κάντε κλικ στην **εφαρμογή κωδικών πρόσβασης.**
4. Δίπλα στο στοιχείο που θέλετε να διαγράψετε τον κωδικό πρόσβασης εφαρμογής, κάντε κλικ στην επιλογή **Διαγραφή**.
5. Επιβεβαιώστε τη διαγραφή, κάνοντας κλικ στην επιλογή **Ναι**.
6. Όταν ο κωδικός πρόσβασης εφαρμογή διαγράφεται μπορείτε να επιλέξετε **Κλείσιμο**.
![Κλείσιμο](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)