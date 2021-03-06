<properties
    pageTitle="Γρήγορα αποτελέσματα με την Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview) | Microsoft Azure"
    description="Διαχείριση πόρων Azure αποθήκευσης με την Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="getting-started-with-storage-explorer-preview"></a>Γρήγορα αποτελέσματα με την Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview)

## <a name="overview"></a>Επισκόπηση 

Εξερεύνηση χώρου αποθήκευσης του Microsoft Azure (έκδοση Preview) είναι μια μεμονωμένη εφαρμογή που σας επιτρέπει να διευκόλυνση εργασιών με δεδομένα του Azure χώρου αποθήκευσης στο Windows, σε λειτουργικό σύστημα OS X και Linux. Σε αυτό το άρθρο, θα μάθετε τους διαφορετικούς τρόπους τους τη σύνδεση και να διαχειριστείτε τους λογαριασμούς σας Azure χώρου αποθήκευσης.

![Εξερεύνηση χώρου αποθήκευσης Microsoft Azure (έκδοση Preview)][15]

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- [Λήψη και εγκατάσταση του Εξερεύνηση χώρου αποθήκευσης (έκδοση preview)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Σύνδεση με το λογαριασμό χώρου αποθήκευσης ή υπηρεσίας

Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview) παρέχει μια αναρίθμητα τρόποι για να συνδεθείτε με λογαριασμούς του χώρου αποθήκευσης. Αυτό περιλαμβάνει τη σύνδεση σε λογαριασμούς χώρου αποθήκευσης που σχετίζεται με το Azure τις συνδρομές σας, τη σύνδεση σε λογαριασμούς χώρου αποθήκευσης και κοινή χρήση από άλλες συνδρομές Azure, τις υπηρεσίες και ακόμα και τη σύνδεση σε και διαχείριση τοπικού χώρου αποθήκευσης με χρήση του Azure προσομοίωσης χώρου αποθήκευσης:

- [Σύνδεση με μια συνδρομή του Azure](#connect-to-an-azure-subscription) - Διαχείριση χώρου αποθήκευσης πόροι που ανήκουν στη συνδρομή σας στο Azure.
- [Εργασία με το χώρο αποθήκευσης τοπικής ανάπτυξης](#work-with-local-development-storage) - Διαχείριση τοπικού χώρου αποθήκευσης με χρήση του Azure προσομοίωσης χώρου αποθήκευσης. 
- [Επισύναψη σε εξωτερικό χώρο αποθήκευσης](#attach-or-detach-an-external-storage-account) - Διαχείριση χώρου αποθήκευσης πόροι που ανήκουν σε μια άλλη Azure συνδρομή χρησιμοποιώντας το όνομα του λογαριασμού και αριθμού-κλειδιού του λογαριασμού του χώρου αποθήκευσης.
- [Επισύναψη αποθήκευσης λογαριασμού με χρήση συσχετισμών Ασφαλείας](#attach-storage-account-using-sas) - Διαχείριση χώρου αποθήκευσης πόροι που ανήκουν σε μια άλλη Azure συνδρομή χρησιμοποιώντας μια συσχετισμών Ασφαλείας.
- [Επισύναψη υπηρεσία χρησιμοποιώντας συσχετισμών Ασφαλείας](#attach-service-using-sas) - διαχείριση μια υπηρεσία αποθήκευσης συγκεκριμένες (blob κοντέινερ, ουρά ή πίνακα) που ανήκουν σε μια άλλη Azure συνδρομή χρησιμοποιώντας μια συσχετισμών Ασφαλείας.

## <a name="connect-to-an-azure-subscription"></a>Σύνδεση με μια συνδρομή του Azure

> [AZURE.NOTE] Εάν δεν έχετε λογαριασμό Azure, μπορείτε να [εγγραφείτε για μια δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ή να [ενεργοποιήσετε το Visual Studio πλεονεκτήματα συνδρομητών](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Στην Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview), επιλέξτε **Ρυθμίσεις λογαριασμού Azure**. 

    ![Ρυθμίσεις λογαριασμού Azure][0]

1. Αριστερό τμήμα του παραθύρου θα εμφανίζουν τώρα όλους τους λογαριασμούς Microsoft που έχετε συνδεθεί στο. Για να συνδεθείτε σε άλλο λογαριασμό, επιλέξτε **Προσθήκη λογαριασμού**και ακολουθήστε τα παράθυρα διαλόγου για να συνδεθείτε με ένα λογαριασμό Microsoft που συσχετίζεται με τουλάχιστον μία ενεργή συνδρομή του Azure.

    ![Προσθήκη λογαριασμού][1]

1. Μόλις πραγματοποιήσετε με επιτυχία με ένα λογαριασμό Microsoft, θα συμπληρώσετε το αριστερό τμήμα του παραθύρου με τις συνδρομές Azure που σχετίζονται με αυτόν το λογαριασμό. Επιλέξτε τις συνδρομές Azure με την οποία θέλετε να εργαστείτε και, στη συνέχεια, επιλέξτε **εφαρμογή**. (Επιλογή **όλων των συνδρομών** εναλλαγή επιλογή όλων ή καμία από τις συνδρομές που παρατίθενται Azure).

    ![Επιλέξτε συνδρομές του Azure][3]

1. Αριστερό τμήμα του παραθύρου θα εμφανίζουν τώρα τους λογαριασμούς του χώρου αποθήκευσης που σχετίζονται με τις επιλεγμένες συνδρομές Azure.

    ![Επιλεγμένες εγγραφές στο Azure][4]

## <a name="work-with-local-development-storage"></a>Εργασία με ανάπτυξης του τοπικού χώρου αποθήκευσης

Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview) σάς επιτρέπει να εργαστείτε σε σχέση με τοπική αποθήκευση χρησιμοποιώντας το Azure προσομοίωσης χώρου αποθήκευσης. Αυτό σας επιτρέπει να γράψετε κώδικα σε σχέση με και να ελέγξετε χώρου αποθήκευσης χωρίς να χρειάζεται απαραίτητα ένα λογαριασμό χώρου αποθήκευσης αναπτυχθεί σε Azure (εφόσον το λογαριασμό χώρου αποθήκευσης είναι που προσομοιωμένο από το Azure προσομοίωσης χώρο αποθήκευσης).

>[AZURE.NOTE] Το Azure προσομοίωσης χώρου αποθήκευσης αυτήν τη στιγμή υποστηρίζεται μόνο για τα Windows. 

1. Στο αριστερό παράθυρο της Εξερεύνησης χώρου αποθήκευσης (έκδοση Preview), αναπτύξτε το **(τοπική και συνημμένο** > **Λογαριασμούς χώρου αποθήκευσης** > κόμβου**(ανάπτυξη)** .

    ![Τοπική ανάπτυξη κόμβου][21]

1. Εάν δεν έχετε εγκαταστήσει ακόμη το Azure προσομοίωσης χώρου αποθήκευσης, θα σας ζητηθεί να το κάνετε μέσω μια γραμμή πληροφοριών. Εάν εμφανίζεται στη γραμμή πληροφοριών, επιλέξτε **λήψη της πιο πρόσφατης έκδοσης**και εγκαταστήστε το προσομοίωσης. 

    ![Λήψη εντολών Azure προσομοίωσης χώρου αποθήκευσης][22]

1. Μόλις εγκαταστήσετε το προσομοίωσης, θα έχετε τη δυνατότητα να δημιουργήσετε και να εργαστείτε με τοπικά αντικείμενα blob, ουρές και πίνακες. Για να μάθετε πώς μπορείτε να εργαστείτε με κάθε τύπο λογαριασμού χώρου αποθήκευσης, επιλέξτε στην κατάλληλη σύνδεση παρακάτω:

    - [Διαχείριση πόρων αποθήκευσης αντικειμένων blob του Azure](./vs-azure-tools-storage-explorer-blobs.md)
    - Διαχείριση πόρων αποθήκευσης Azure αρχείο κοινή χρήση - *σύντομα διαθέσιμα*
    - Διαχείριση πόρων αποθήκευσης Azure ουρά - *σύντομα διαθέσιμο*
    - Διαχείριση πόρων αποθήκευσης πινάκων του Azure - *σύντομα διαθέσιμο*

## <a name="attach-or-detach-an-external-storage-account"></a>Επισύναψη ή απόσπαση λογαριασμού εξωτερικού χώρου αποθήκευσης

Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview) παρέχει τη δυνατότητα να επισυνάψετε σε εξωτερικό χώρο αποθήκευσης λογαριασμούς, έτσι ώστε να μοιράζεστε εύκολα λογαριασμούς χώρου αποθήκευσης. Αυτή η ενότητα εξηγεί πώς να Επισύναψη (και αποσύνδεση από) λογαριασμούς εξωτερικού χώρου αποθήκευσης.

### <a name="get-the-storage-account-credentials"></a>Λάβετε τα διαπιστευτήρια λογαριασμού χώρου αποθήκευσης

Για να κάνετε κοινή χρήση ενός λογαριασμού εξωτερικού χώρου αποθήκευσης, ο κάτοχος του αυτόν το λογαριασμό πρέπει να πρώτα να λάβει τα διαπιστευτήρια - όνομα λογαριασμού και αριθμού-κλειδιού - για το λογαριασμό και, στη συνέχεια, να κάνετε κοινή χρήση πληροφοριών με το άτομο που θέλετε να επισυνάψετε σε αυτόν το λογαριασμό (εξωτερικό). Απόκτηση τα διαπιστευτήρια λογαριασμού χώρου αποθήκευσης μπορεί να γίνει μέσω της πύλης Azure, ακολουθώντας τα παρακάτω βήματα: 

1.  Είσοδος στην [πύλη του Azure](https://portal.azure.com).
1.  Επιλέξτε **Αναζήτηση**.
1.  Επιλέξτε **Λογαριασμοί χώρου αποθήκευσης**.
1.  Στο το blade **Λογαριασμούς χώρου αποθήκευσης** , επιλέξτε το λογαριασμό χώρου αποθήκευσης που θέλετε.
1.  Στο το blade **ρυθμίσεων** για τον επιλεγμένο χώρο αποθήκευσης λογαριασμό, επιλέξτε **τα πλήκτρα πρόσβασης**.

    ![Επιλογή πλήκτρα της Access][5]
    
1.  Στο τα **πλήκτρα πρόσβασης** blade, αντιγράψτε τις τιμές **ΌΝΟΜΑ ΛΟΓΑΡΙΑΣΜΟΎ χώρου ΑΠΟΘΉΚΕΥΣΗΣ** και **ΚΛΕΙΔΊ 1** για χρήση κατά την επισύναψη με το λογαριασμό χώρου αποθήκευσης. 

    ![Πλήκτρα πρόσβασης][6]

### <a name="attach-to-an-external-storage-account"></a>Επισύναψη σε ένα λογαριασμό εξωτερικού χώρου αποθήκευσης
Για να επισυνάψετε ένα λογαριασμό εξωτερικού χώρου αποθήκευσης, θα χρειαστείτε όνομα του λογαριασμού και το κλειδί. Στην ενότητα *γρήγορα τα διαπιστευτήρια λογαριασμού χώρου αποθήκευσης* εξηγεί τον τρόπο για να λάβετε αυτές τις τιμές από την πύλη του Azure. Ωστόσο, σημειώστε ότι στην πύλη, το κλειδί λογαριασμού ονομάζεται "βασικές 1" ώστε το σημείο όπου η Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview) ζητά ένα κλειδί λογαριασμού, που θα εισαγάγετε (ή επικόλληση) την τιμή "βασικές 1". 
 
1.  Στην Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview), επιλέξτε **σύνδεση με το χώρο αποθήκευσης Azure**.

    ![Σύνδεση με την επιλογή αποθήκευσης Azure][23]

1.  Στο παράθυρο διαλόγου **σύνδεση με το χώρο αποθήκευσης Azure** , καθορίστε τον αριθμό-κλειδί λογαριασμού (τιμή "βασικές 1" από την πύλη του Azure) και, στη συνέχεια, επιλέξτε **Επόμενο**.

    ![Σύνδεση στο παράθυρο διαλόγου Azure χώρου αποθήκευσης][24] 

1.  Στο παράθυρο διαλόγου **Επισύναψη εξωτερικού χώρου αποθήκευσης** , εισαγάγετε το όνομα του λογαριασμού χώρου αποθήκευσης στο πλαίσιο **όνομα λογαριασμού** , καθορίστε οποιεσδήποτε άλλες ρυθμίσεις θέλετε, και επιλέξτε **Επόμενο** όταν τελειώσετε. 

    ![Επισύναψη διαλόγου εξωτερικού χώρου αποθήκευσης][8]

1.  Στο παράθυρο διαλόγου **Σύνδεσης σύνοψης** , επαληθεύστε τις πληροφορίες. Εάν θέλετε να αλλάξετε τίποτα, επιλέξτε **ξανά** και εισαγάγετε ξανά τις ρυθμίσεις που θέλετε. Αφού ολοκληρώσετε τη διαδικασία, επιλέξτε **σύνδεση**.

1.  Μετά τη σύνδεση, ο λογαριασμός εξωτερικού χώρου αποθήκευσης θα εμφανίζεται με το κείμενο **(εξωτερικό)** προσαρτάται στο όνομα του λογαριασμού χώρου αποθήκευσης. 

    ![Αποτέλεσμα από τη σύνδεση με ένα λογαριασμό εξωτερικού χώρου αποθήκευσης][9]

### <a name="detach-from-an-external-storage-account"></a>Απόσπαση από έναν λογαριασμό του εξωτερικού χώρου αποθήκευσης

1.  Κάντε δεξί κλικ το λογαριασμό εξωτερικού χώρου αποθήκευσης που θέλετε να αποσπάσετε και - από το μενού περιβάλλοντος - επιλέξτε **Αποσύνδεση**.

    ![Αποσυνδεθείτε από την επιλογή αποθήκευσης][10]

1.  Όταν εμφανιστεί το πλαίσιο μήνυμα επιβεβαίωσης, επιλέξτε **Ναι** για να επιβεβαιώσετε την αποκόλλησης από το λογαριασμό του εξωτερικού χώρου αποθήκευσης.

## <a name="attach-storage-account-using-sas"></a>Επισύναψη λογαριασμό χώρου αποθήκευσης με τη χρήση συσχετισμών Ασφαλείας

Μια [συσχετισμών Ασφαλείας (κοινόχρηστα υπογραφή Access)](storage/storage-dotnet-shared-access-signature-part-1.md) παρέχει ο διαχειριστής της συνδρομής Azure τη δυνατότητα να εκχωρήσετε πρόσβαση σε ένα λογαριασμό χώρου αποθήκευσης προσωρινά χωρίς να χρειάζεται να παρέχετε τα διαπιστευτήριά τους Azure συνδρομής. 

Για να παρουσιάζουν αυτό το παράδειγμα, ας υποθέσουμε ότι ΧρήστηΑ είναι διαχειριστής της συνδρομής Azure και ΧρήστηΑ θέλει να επιτρέψετε δικαιώματα για να αποκτήσετε πρόσβαση σε λογαριασμό του χώρου αποθήκευσης για ένα περιορισμένο χρονικό διάστημα με ορισμένα δικαιώματα:

1. ΧρήστηΑ δημιουργεί μια συσχετισμών Ασφαλείας (που αποτελείται από τη συμβολοσειρά σύνδεσης για το λογαριασμό χώρου αποθήκευσης) για μια συγκεκριμένη χρονική περίοδο και με τα δικαιώματα που θέλετε.
1. ΧρήστηΑ μοιράζεται τις συσχετίσεις Ασφαλείας με το άτομο που θέλετε πρόσβαση στο χώρο αποθήκευσης λογαριασμό - δικαιώματα, στο παράδειγμά μας.  
1. Δικαιώματα χρησιμοποιεί Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview) για να επισυνάψετε το λογαριασμό που ανήκουν σε ΧρήστηΑ χρησιμοποιώντας το παρεχόμενο συσχετισμών Ασφαλείας. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Λήψη ενός συσχετισμών Ασφαλείας για το λογαριασμό που θέλετε να κάνετε κοινή χρήση

1.  Στην Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview), κάντε δεξί κλικ στο λογαριασμό χώρου αποθήκευσης που θέλετε να κάνετε κοινή χρήση και - από το μενού περιβάλλοντος - επιλέξτε **Υπογραφή γρήγορα πρόσβαση σε κοινή χρήση**.

    ![Λήψη επιλογή μενού περιβάλλοντος συσχετισμών Ασφαλείας][13]

1. Στο παράθυρο διαλόγου **Υπογραφή πρόσβασης σε κοινή χρήση** , καθορίστε το χρονικό πλαίσιο και τα δικαιώματα που θέλετε για το λογαριασμό και επιλέξτε **Δημιουργία**.

    ![Λήψη παραθύρου διαλόγου συσχετισμών Ασφαλείας][14]
 
1. Ένα δεύτερο παράθυρο διαλόγου **Κοινή χρήση Access υπογραφή** θα εμφανίζεται εμφανίζοντας τις συσχετίσεις Ασφαλείας. Επιλέξτε **Αντιγραφή** δίπλα στο στοιχείο τη **Συμβολοσειρά σύνδεσης** για να το αντιγράψετε στο Πρόχειρο. Επιλέξτε **Κλείσιμο** για να κλείσετε το παράθυρο διαλόγου.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Επισύναψη στο κοινόχρηστο λογαριασμό χρησιμοποιώντας τις συσχετίσεις Ασφαλείας

1.  Στην Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview), επιλέξτε **σύνδεση με το χώρο αποθήκευσης Azure**.

    ![Σύνδεση με την επιλογή αποθήκευσης Azure][23]

1.  Στο παράθυρο διαλόγου **σύνδεση με το χώρο αποθήκευσης Azure** , καθορίστε τη συμβολοσειρά σύνδεσης και, στη συνέχεια, επιλέξτε **Επόμενο**.

    ![Σύνδεση στο παράθυρο διαλόγου Azure χώρου αποθήκευσης][24] 

1.  Στο παράθυρο διαλόγου **Σύνδεσης σύνοψης** , επαληθεύστε τις πληροφορίες. Εάν θέλετε να αλλάξετε τίποτα, επιλέξτε **ξανά** και εισαγάγετε ξανά τις ρυθμίσεις που θέλετε. Αφού ολοκληρώσετε τη διαδικασία, επιλέξτε **σύνδεση**.

1.  Μόλις συνημμένα, το λογαριασμό χώρου αποθήκευσης θα εμφανίζεται με το κείμενο (συσχετισμών Ασφαλείας) προσαρτάται στο όνομα του λογαριασμού που πληκτρολογήσατε.

    ![Έχει ως αποτέλεσμα της που έχουν επισυναφθεί σε ένα λογαριασμό χρησιμοποιώντας συσχετισμών Ασφαλείας][17]

## <a name="attach-service-using-sas"></a>Επισύναψη υπηρεσίας χρησιμοποιώντας συσχετισμών Ασφαλείας

Στην ενότητα [Επισύναψη λογαριασμό χώρου αποθήκευσης με τη χρήση συσχετισμών Ασφαλείας](#attach-storage-account-using-sas) απεικονίζει τον τρόπο διαχειριστής Azure συνδρομή μπορεί να εκχωρήσει προσωρινή πρόσβαση σε ένα λογαριασμό του χώρου αποθήκευσης, δημιουργώντας (και κοινή χρήση) μια συσχετισμών Ασφαλείας για το λογαριασμό χώρου αποθήκευσης. Ομοίως, μπορεί να δημιουργηθεί ένα συσχετισμών Ασφαλείας για μια συγκεκριμένη υπηρεσία (blob κοντέινερ, ουρά ή πίνακα) μέσα σε ένα λογαριασμό του χώρου αποθήκευσης.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Δημιουργία ενός συσχετισμών Ασφαλείας για την υπηρεσία που θέλετε να κάνετε κοινή χρήση

Σε αυτό το πλαίσιο, μια υπηρεσία μπορεί να είναι ένα κοντέινερ αντικειμένων blob, ουρά ή πίνακα. Οι παρακάτω ενότητες εξηγούν τον τρόπο για να δημιουργήσετε τις συσχετίσεις Ασφαλείας για την υπηρεσία που παρατίθενται:

- [Λάβετε τις συσχετίσεις Ασφαλείας για ένα κοντέινερ αντικειμένων blob](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Λάβετε τις συσχετίσεις Ασφαλείας για ένα κοινόχρηστο αρχείο - *σύντομα διαθέσιμο*
- Λάβετε τις συσχετίσεις Ασφαλείας για μια ουρά - *σύντομα διαθέσιμο*
- Λάβετε τις συσχετίσεις Ασφαλείας για έναν πίνακα - *σύντομα διαθέσιμο*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Επισύναψη με την υπηρεσία κοινόχρηστο λογαριασμό χρησιμοποιώντας τις συσχετίσεις Ασφαλείας

1.  Στην Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview), επιλέξτε **σύνδεση με το χώρο αποθήκευσης Azure**.

    ![Σύνδεση με την επιλογή αποθήκευσης Azure][23]

1.  Στο παράθυρο διαλόγου **σύνδεση με το χώρο αποθήκευσης Azure** , καθορίστε το URI συσχετισμών Ασφαλείας και, στη συνέχεια, επιλέξτε **Επόμενο**.

    ![Σύνδεση στο παράθυρο διαλόγου Azure χώρου αποθήκευσης][24] 

1.  Στο παράθυρο διαλόγου **Σύνδεσης σύνοψης** , επαληθεύστε τις πληροφορίες. Εάν θέλετε να αλλάξετε τίποτα, επιλέξτε **ξανά** και εισαγάγετε ξανά τις ρυθμίσεις που θέλετε. Αφού ολοκληρώσετε τη διαδικασία, επιλέξτε **σύνδεση**.

1.  Μόλις συνημμένα, την υπηρεσία που μόλις συνημμένο θα εμφανίζονται κάτω από τον κόμβο **(Υπηρεσία συσχετισμών Ασφαλείας)** .

    ![Αποτέλεσμα της Επισύναψη σε μια κοινόχρηστη υπηρεσία χρησιμοποιώντας συσχετισμών Ασφαλείας][20]

## <a name="search-for-storage-accounts"></a>Αναζήτηση για λογαριασμούς του χώρου αποθήκευσης

Εάν έχετε μια μεγάλη λίστα λογαριασμών χώρου αποθήκευσης, ένας γρήγορος τρόπος για να εντοπίσετε ένα λογαριασμό του συγκεκριμένου χώρου αποθήκευσης είναι να χρησιμοποιήσετε το πλαίσιο αναζήτησης στο επάνω μέρος του αριστερού τμήματος παραθύρου. 

Καθώς πληκτρολογείτε στο πλαίσιο αναζήτησης, αριστερό τμήμα του παραθύρου θα εμφανίσει μόνο το χώρο αποθήκευσης οι λογαριασμοί που ταιριάζουν με την τιμή αναζήτησης που έχετε εισαγάγει προς τα επάνω που οδηγούν. Το παρακάτω στιγμιότυπο οθόνης παρουσιάζει ένα παράδειγμα όπου να έχετε εκτελέσει αναζήτηση για όλους τους λογαριασμούς αποθήκευσης όπου το όνομα του λογαριασμού χώρου αποθήκευσης περιέχει το κείμενο "tarcher".

![Αναζήτηση λογαριασμού χώρου αποθήκευσης][11]
    
Για να απαλείψετε τα αποτελέσματα της αναζήτησης, επιλέξτε το κουμπί **x** στο πλαίσιο αναζήτησης.

## <a name="next-steps"></a>Επόμενα βήματα
- [Διαχείριση πόρων αποθήκευσης αντικειμένων blob του Azure με την Εξερεύνηση χώρου αποθήκευσης (έκδοση Preview)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
