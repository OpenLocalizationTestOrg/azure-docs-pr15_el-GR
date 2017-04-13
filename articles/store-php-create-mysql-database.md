<properties
    pageTitle="Δημιουργία και να συνδεθείτε με μια βάση δεδομένων MySQL στο Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την πύλη του Azure για να δημιουργήσετε μια βάση δεδομένων MySQL και, στη συνέχεια, συνδεθείτε με το από μια εφαρμογή web PHP στο Azure."
    documentationCenter="php"
    services="app-service\web"
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm;cephalin"/>

# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Δημιουργία και να συνδεθείτε με μια βάση δεδομένων MySQL στο Azure

Αυτό το πρόγραμμα εκμάθησης σας δείχνει πώς μπορείτε να δημιουργήσετε μια βάση δεδομένων MySQL στην [πύλη του Azure](https://portal.azure.com) (είναι η υπηρεσία παροχής [ClearDB](http://www.cleardb.com/)) και πώς μπορείτε να συνδεθείτε σε αυτήν από μια εφαρμογή web PHP εκτελείται στο [Azure εφαρμογής υπηρεσίας](./app-service/app-service-value-prop-what-is.md). 

> [AZURE.NOTE] Μπορείτε επίσης να δημιουργήσετε μια βάση δεδομένων MySQL ως μέρος ενός [προτύπου εφαρμογής Marketplace](./app-service-web/app-service-web-create-web-app-from-marketplace.md).

## <a name="create-a-mysql-database-in-azure-portal"></a>Δημιουργήστε μια βάση δεδομένων MySQL στην πύλη του Azure

Για να δημιουργήσετε μια βάση δεδομένων MySQL στην πύλη του Azure, κάντε τα εξής:

1. Συνδεθείτε [πύλη του Azure](https://portal.azure.com).

2. Από το αριστερό μενού, κάντε κλικ στην επιλογή **Δημιουργία** > **δεδομένων + χώρος αποθήκευσης** > **Βάση δεδομένων MySQL**.

    ![Δημιουργήστε μια βάση δεδομένων MySQL στο Azure - έναρξης](./media/store-php-create-mysql-database/create-db-1-start.png)

2. Η νέα βάση δεδομένων MySQL [blade](azure-portal-overview.md), να ρυθμίσετε τη νέα βάση δεδομένων MySQL ως εξής (*blade*: μια σελίδα της πύλης που ανοίγει οριζόντια):

    - **Όνομα βάσης δεδομένων**: Πληκτρολογήστε ένα όνομα με μοναδικό τρόπο αναγνωρίσιμα
    - **Συνδρομή**: Επιλέξτε τη συνδρομή για να χρησιμοποιήσετε
    - **Τύπος βάσης δεδομένων**: Επιλέξτε **κοινή χρήση** για χαμηλού κόστους ή δωρεάν σειρές ή **Dedicated** για να λάβετε αποκλειστικό πόρους. 
    - **Ομάδα πόρων**: Προσθήκη τη βάση δεδομένων MySQL σε μια υπάρχουσα [ομάδα πόρων](../azure-resource-manager/resource-group-overview.md) ή να το τοποθετήσετε σε ένα νέο. Πόροι της ίδιας ομάδας μπορείτε να διαχειριστείτε εύκολα μαζί.
    - **Θέση**: Επιλέξτε μια θέση Κλείσιμο για να κάνετε. Όταν προσθέτετε σε μια υπάρχουσα ομάδα πόρων, είστε κλειδωμένο στη θέση της ομάδας πόρων.
    - **Σειρά τις τιμές**: κάντε κλικ στην επιλογή **Σειρά τις τιμές**και, στη συνέχεια, επιλέξτε μια επιλογή τιμολόγησης (επίπεδο**υδραργύρου** είναι δωρεάν), και, στη συνέχεια, κάντε κλικ στην **επιλογή**. 
    - **Νομικά όρους**: κάντε κλικ στην επιλογή **Νομική όρους**, εξετάστε τις λεπτομέρειες της αγοράς και κάντε κλικ στην επιλογή **αγορά**.
    - **Καρφίτσωμα στον πίνακα εργαλείων**: Επιλέξτε εάν θέλετε να έχετε πρόσβαση σε αυτά απευθείας από τον πίνακα εργαλείων. Αυτό είναι ιδιαίτερα χρήσιμο εάν δεν είστε εξοικειωμένοι με πύλης περιήγησης ακόμη.
    
    Το παρακάτω στιγμιότυπο οθόνης είναι απλώς ένα παράδειγμα της πώς μπορείτε να ρυθμίσετε τη βάση δεδομένων MySQL.  
    ![Να δημιουργήσετε μια βάση δεδομένων MySQL στο Azure - ρύθμιση παραμέτρων](./media/store-php-create-mysql-database/create-db-2-configure.png)

3. Όταν ολοκληρώσετε τη ρύθμιση των παραμέτρων, κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Να δημιουργήσετε μια βάση δεδομένων MySQL στο Azure - δημιουργία](./media/store-php-create-mysql-database/create-db-3-create.png)

    Θα δείτε μια δυνατότητας αποστολής αναδυόμενη ειδοποίηση που γνωρίζετε ότι έχει ξεκινήσει ανάπτυξης.

    ![Δημιουργήστε μια βάση δεδομένων MySQL στο Azure - σε εξέλιξη](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Θα λάβετε μια άλλη αναδυόμενο αφού ανάπτυξης ολοκληρώθηκε με επιτυχία. Η πύλη επίσης θα ανοίξει το blade βάση δεδομένων MySQL αυτόματα.

<a name="connect"></a>
## <a name="connect-to-your-mysql-database"></a>Σύνδεση με τη βάση δεδομένων MySQL

Για να δείτε τις πληροφορίες σύνδεσης για τη νέα βάση δεδομένων MySQL, απλώς κάντε κλικ στην επιλογή **Ιδιότητες** στο blade της εφαρμογής σας web.
    
![Δημιουργήστε μια βάση δεδομένων MySQL στο Azure - blade βάση δεδομένων MySQL](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Τώρα, μπορείτε να χρησιμοποιήσετε αυτές τις πληροφορίες σύνδεσης σε οποιαδήποτε εφαρμογή web. Ένα δείγμα που δείχνει πώς μπορείτε να χρησιμοποιήσετε τις πληροφορίες σύνδεσης από μια απλή εφαρμογή PHP είναι διαθέσιμη [εδώ](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Σύνδεση μιας εφαρμογής web Laravel (από το PHP γρήγορα αποτελέσματα πρόγραμμα εκμάθησης)

Ας υποθέσουμε ότι που μόλις ολοκληρώσετε τη διαδικασία εκμάθησης [Δημιουργία, ρύθμιση παραμέτρων, και την ανάπτυξη μιας εφαρμογής web PHP σε Azure](./app-service-web/app-service-web-php-get-started.md) και να έχετε μια εφαρμογή web [Laravel](https://www.laravel.com/) εκτελείται στο Azure. Μπορείτε εύκολα να προσθέσετε δυνατότητες βάσης δεδομένων σε εφαρμογή Laravel. Απλώς ακολουθήστε τα παρακάτω βήματα:

>[AZURE.NOTE] Ακολουθήστε τα παρακάτω βήματα λαμβάνεται ως δεδομένο ότι έχετε ολοκληρώσει την εκμάθηση [Δημιουργία, ρύθμιση παραμέτρων, και την ανάπτυξη μιας εφαρμογής web PHP σε Azure](./app-service-web/app-service-web-php-get-started.md).

1. Ρύθμιση παραμέτρων της εφαρμογής Laravel στο περιβάλλον σας τοπικής ανάπτυξης ώστε να οδηγεί στη βάση δεδομένων MySQL. Για να το κάνετε αυτό, ανοίξτε `.env` από την εφαρμογή του Laravel είναι ο ριζικός κατάλογος και να ρυθμίσετε τις επιλογές βάση δεδομένων MySQL.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

    >[AZURE.NOTE] Στο το blade **Ιδιότητες** , το όνομα της βάσης δεδομένων MySQL μπορεί να ή μπορεί να μην είναι αυτό που εμφανίζεται στο πεδίο **ΌΝΟΜΑ βάσης ΔΕΔΟΜΈΝΩΝ** . Είναι προτιμότερο να ελέγξετε την παράμετρο βάσης δεδομένων στο πεδίο " **ΣΥΜΒΟΛΟΣΕΙΡΆ ΣΎΝΔΕΣΗΣ** ". 
    >
    >![Δημιουργήστε μια βάση δεδομένων MySQL στο Azure - σε εξέλιξη](./media/store-php-create-mysql-database/connect-db-1-database-name.png)

2. Ο πιο γρήγορος τρόπος για να βεβαιωθείτε ότι έχετε άμεση πρόσβαση MySQL είναι να χρησιμοποιήσετε [ικριώματος έλεγχος ταυτότητας του Laravel προεπιλογή](https://laravel.com/docs/5.2/authentication#authentication-quickstart). Στο το terminal γραμμής εντολών, εκτελέστε τις ακόλουθες εντολές από το ριζικό κατάλογο της εφαρμογής σας Laravel:

         php artisan migrate
         php artisan make:auth

    Η πρώτη εντολή δημιουργεί τους πίνακες στο Azure που βασίζονται σε προκαθορισμένο μετεγκαταστάσεις στο το `database/migrations` καταλόγου και η δεύτερη εντολή ικριώματα τις βασικές προβολές και τις διαδρομές για δήλωση του χρήστη και τον έλεγχο ταυτότητας.

3. Εκτελέστε το διακομιστή ανάπτυξης τώρα:

        php artisan serve

4. Στο πρόγραμμα περιήγησης, μεταβείτε σε http://localhost:8000 και καταχωρήσετε έναν νέο χρήστη, όπως φαίνεται:

    ![Σύνδεση με βάση δεδομένων MySQL στο Azure - καταχώρηση χρήστη](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Ακολουθήστε το προτρεπτικό μήνυμα περιβάλλοντος εργασίας Χρήστη ολοκληρωθεί η καταχώρηση. Μόλις ολοκληρωθεί η καταχώρηση, θα συνδεθείτε.
    
    ![Σύνδεση με βάση δεδομένων MySQL στο Azure - καταχώρηση χρήστη](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Τώρα αναπτύσσετε την εφαρμογή σας με τη βάση δεδομένων MySQL στο Azure.

5. Τώρα, θα χρειαστεί μόνο να επαναλάβετε την `.env` ρυθμίσεις σε εφαρμογή Azure web. Εκτελέστε τις ακόλουθες εντολές Azure CLI:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

    Μάθετε πώς λειτουργεί στο θέμα [Ρύθμιση παραμέτρων του Azure web app](./app-service-web/app-service-web-php-get-started.md#configure).

6. Στη συνέχεια, πραγματοποιήσετε και να προωθήσετε σε Azure τις τοπικές αλλαγές που κάνατε προηγουμένως κατά την εκτέλεση `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master

7. Αναζητήστε το Azure web app.

        azure site browse

8. Συνδεθείτε χρησιμοποιώντας τα διαπιστευτήρια του χρήστη που δημιουργήσατε νωρίτερα.

    ![Σύνδεση με βάση δεδομένων MySQL στον Azure - αναζητήστε Azure web app](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Μόλις συνδεθείτε, θα πρέπει να βλέπετε την φιλική οθόνη μετά τη σύνδεση.
    
    ![Συνδεθείτε στο Azure - συνδεδεμένοι στη βάση δεδομένων MySQL](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Συγχαρητήρια, εφαρμογή web της PHP στο Azure έχει πλέον πρόσβαση σε δεδομένα από τη βάση δεδομένων MySQL. 

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές PHP](/develop/php/).
