<properties
    pageTitle="Δημιουργία εφαρμογής web PHP MySQL στο Azure εφαρμογής υπηρεσίας και ανάπτυξη χρησιμοποιώντας Git"
    description="Ένα πρόγραμμα εκμάθησης που δείχνει πώς μπορείτε να δημιουργήσετε μια εφαρμογή web PHP που αποθηκεύει δεδομένα σε MySQL και να χρησιμοποιήσετε Git ανάπτυξης για να Azure."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Δημιουργία εφαρμογής web PHP MySQL στο Azure εφαρμογής υπηρεσίας και ανάπτυξη χρησιμοποιώντας Git

Αυτό το πρόγραμμα εκμάθησης σας δείχνει πώς μπορείτε να δημιουργήσετε μια εφαρμογή web PHP MySQL και πώς μπορείτε να αναπτύξετε για [Εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=529714) με χρήση Git. Θα χρησιμοποιήσετε [PHP][install-php], το εργαλείο γραμμής εντολών MySQL (μέρος του [MySQL][install-mysql]), και [Git] [ install-git] εγκατεστημένο στον υπολογιστή σας. Μπορείτε να ακολουθήσετε τις οδηγίες σε αυτό το πρόγραμμα εκμάθησης σε οποιοδήποτε λειτουργικό σύστημα, συμπεριλαμβανομένων των Windows, Mac και Linux. Κατά την ολοκλήρωση αυτού του οδηγού, θα έχετε μια εφαρμογή web PHP/MySQL εκτελείται στο Azure.

Θα μάθετε:

* Πώς μπορείτε να δημιουργήσετε μια εφαρμογή web και μια βάση δεδομένων MySQL με την [Πύλη Azure][management-portal]. Επειδή PHP είναι ενεργοποιημένη στις [Εφαρμογές Web της εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=529714) από προεπιλογή, τίποτα ειδική απαιτείται για να εκτελέσετε τον κώδικα PHP.
* Μάθετε πώς μπορείτε να δημοσιεύσετε και να δημοσιεύσετε εκ νέου την εφαρμογή με το Azure χρησιμοποιώντας Git.
* Πώς μπορείτε να ενεργοποιήσετε την επέκταση σύνθεσης για την αυτοματοποίηση σύνθεσης εργασίες σε κάθε `git push`.

Ακολουθώντας αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να δημιουργήσετε μια εφαρμογή web απλό εγγραφής στο PHP. Η εφαρμογή θα να φιλοξενούνται σε εφαρμογές Web. Στιγμιότυπο οθόνης της εφαρμογής ολοκληρωμένη είναι κάτω από:

![Τοποθεσία web Azure PHP][running-app]

## <a name="set-up-the-development-environment"></a>Ρυθμίστε το περιβάλλον ανάπτυξης

Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε [PHP][install-php], το εργαλείο γραμμής εντολών MySQL (μέρος του [MySQL][install-mysql]), και [Git] [ install-git] εγκατεστημένο στον υπολογιστή σας.

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>Δημιουργία εφαρμογής web και να ρυθμίσετε Git δημοσίευσης

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε μια εφαρμογή web και μια βάση δεδομένων MySQL:

1. Συνδεθείτε στην [πύλη του Azure][management-portal].
2. Κάντε κλικ στο εικονίδιο **Δημιουργία** .

3. Κάντε κλικ στην επιλογή **Δείτε όλων** δίπλα στο στοιχείο **Marketplace**. 

4. Κάντε κλικ στην επιλογή **Web + Mobile**, στη συνέχεια, **στο Web app + MySQL**. Στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

4. Πληκτρολογήστε ένα έγκυρο όνομα για την ομάδα πόρων.

    ![Ορισμός όνομα ομάδας πόρων][resource-group]

5. Εισαγάγετε τιμές για τη νέα εφαρμογή web.

    ![Δημιουργία εφαρμογής web][new-web-app]

6. Εισαγάγετε τιμές για τη νέα βάση δεδομένων, συμπεριλαμβανομένων των συμφωνώντας να νομική τους όρους της.

    ![Δημιουργία νέας βάσης δεδομένων MySQL][new-mysql-db]

7. Αφού δημιουργηθεί το web app, θα δείτε το νέο blade εφαρμογής web.

7. Στις **Ρυθμίσεις** , κάντε κλικ στην επιλογή στην **Ανάπτυξη συνεχής**και, στη συνέχεια, κάντε κλικ στη _Ρύθμιση παραμέτρων απαιτούμενες ρυθμίσεις_.

    ![Ρύθμιση Git δημοσίευσης][setup-publishing]

8. Επιλέξτε **Τοπικό Git αποθετήριο δεδομένων** για την προέλευση.

    ![Ρύθμιση του αποθετηρίου Git][setup-repository]


9. Για να ενεργοποιήσετε Git δημοσίευσης, πρέπει να δώσετε ένα όνομα χρήστη και τον κωδικό πρόσβασης. Σημειώστε το όνομα χρήστη και τον κωδικό πρόσβασης που δημιουργείτε. (Εάν έχετε ρυθμίσει ένα αποθετήριο Git πριν, αυτό το βήμα θα παραλειφθούν.)

    ![Δημιουργία δημοσίευσης διαπιστευτήρια][credentials]


## <a name="get-remote-mysql-connection-information"></a>Λήψη πληροφοριών σύνδεσης απομακρυσμένης MySQL

Για να συνδεθείτε με τη βάση δεδομένων MySQL που εκτελείται σε εφαρμογές Web, σας θα χρειάζονται τις πληροφορίες σύνδεσης. Για να λάβετε πληροφορίες σύνδεσης MySQL, ακολουθήστε τα παρακάτω βήματα:

1. Από την ομάδα πόρων, κάντε κλικ στη βάση δεδομένων:

    ![Επιλέξτε βάση δεδομένων][select-database]

2. Από τη βάση δεδομένων **Ρυθμίσεις**, επιλέξτε **Ιδιότητες**.

    ![Επιλέξτε την εντολή Ιδιότητες][select-properties]

2. Σημειώστε τις τιμές για `Database`, `Host`, `User Id`, και `Password`.

    ![Ιδιότητες σημειώσεων][note-properties]

## <a name="build-and-test-your-app-locally"></a>Δημιουργία και να ελέγξετε την εφαρμογή σας τοπικά

Τώρα που έχετε δημιουργήσει μια εφαρμογή web, μπορείτε να αναπτύξετε την εφαρμογή τοπικά και έπειτα ανάπτυξή του μετά τη δοκιμή.

Η εφαρμογή δήλωσης είναι μια απλή εφαρμογή PHP που σας επιτρέπει να εγγραφείτε για ένα συμβάν, παρέχοντας το όνομα διεύθυνση ηλεκτρονικού ταχυδρομείου. Πληροφορίες σχετικά με την προηγούμενη εγγεγραμμένοι PartnerDirect εμφανίζεται σε έναν πίνακα. Πληροφορίες εγγραφής αποθηκεύονται σε μια βάση δεδομένων MySQL. Η εφαρμογή αποτελείται από ένα αρχείο (διαθέσιμη κάτω από αντιγράψτε και επικολλήστε κωδικός):

* **Index.PHP**: Εμφανίζει μια φόρμα για την καταχώρηση και σε έναν πίνακα που περιέχει τις πληροφορίες registrant.

Για να δημιουργήσετε και να εκτελέσετε την εφαρμογή τοπικά, ακολουθήστε τα παρακάτω βήματα. Σημειώστε ότι αυτά τα βήματα θεωρείται ότι έχετε το εργαλείο γραμμής εντολών MySQL (μέρος του MySQL) που ρύθμιση στον τοπικό υπολογιστή σας και PHP και ότι έχετε ενεργοποιήσει την [επέκταση ΠΟΠ για MySQL][pdo-mysql].

1. Σύνδεση με τον απομακρυσμένο διακομιστή MySQL, χρησιμοποιώντας την τιμή για το `Data Source`, `User Id`, `Password`, και `Database` που ανακτήσατε παλαιότερη έκδοση:

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. Γραμμή εντολών MySQL θα εμφανίζονται:

        mysql>

3. Επικολλήστε το εξής `CREATE TABLE` εντολή για να δημιουργήσετε το `registration_tbl` πίνακα στη βάση δεδομένων σας:

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. Στον ριζικό κατάλογο του φακέλου σας τοπική εφαρμογή Δημιουργία αρχείου **index.php** .

5. Ανοίξτε το αρχείο **index.php** σε ένα πρόγραμμα επεξεργασίας κειμένου ή IDE και προσθέστε τον ακόλουθο κώδικα και ολοκληρώστε τις απαραίτητες αλλαγές που επισημαίνεται με `//TODO:` σχόλια.


        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
            if(!empty($_POST)) {
            try {
                $name = $_POST['name'];
                $email = $_POST['email'];
                $date = date("Y-m-d");
                // Insert data
                $sql_insert = "INSERT INTO registration_tbl (name, email, date)
                           VALUES (?,?,?)";
                $stmt = $conn->prepare($sql_insert);
                $stmt->bindValue(1, $name);
                $stmt->bindValue(2, $email);
                $stmt->bindValue(3, $date);
                $stmt->execute();
            }
            catch(Exception $e) {
                die(var_dump($e));
            }
            echo "<h3>Your're registered!</h3>";
            }
            // Retrieve data
            $sql_select = "SELECT * FROM registration_tbl";
            $stmt = $conn->query($sql_select);
            $registrants = $stmt->fetchAll();
            if(count($registrants) > 0) {
                echo "<h2>People who are registered:</h2>";
                echo "<table>";
                echo "<tr><th>Name</th>";
                echo "<th>Email</th>";
                echo "<th>Date</th></tr>";
                foreach($registrants as $registrant) {
                    echo "<tr><td>".$registrant['name']."</td>";
                    echo "<td>".$registrant['email']."</td>";
                    echo "<td>".$registrant['date']."</td></tr>";
                }
                echo "</table>";
            } else {
                echo "<h3>No one is currently registered.</h3>";
            }
        ?>
        </body>
        </html>

4.  Στο terminal μια Μετάβαση στο φάκελο εφαρμογής και πληκτρολογήστε την παρακάτω εντολή:

        php -S localhost:8000

Τώρα μπορείτε να περιηγηθείτε στο **http://localhost:8000 /** για να ελέγξετε την εφαρμογή.


## <a name="publish-your-app"></a>Δημοσίευση της εφαρμογής σας

Αφού ελέγξετε τοπικά την εφαρμογή σας, μπορείτε να το δημοσιεύσετε σε εφαρμογές Web χρησιμοποιώντας Git. Θα προετοιμασία του τοπικού αποθετήριο Git και δημοσίευση της εφαρμογής.

> [AZURE.NOTE]
> Αυτά είναι τα ίδια βήματα που εμφανίζονται στην πύλη του Azure στο τέλος της τη δημιουργία μιας εφαρμογής web και ρύθμιση του παραπάνω ενότητα Git δημοσίευσης.

1. (Προαιρετικό)  Εάν έχετε ξεχάσει ή χωρίς συγχρονισμό διεύθυνσης URL απομακρυσμένης repostitory Git, μεταβείτε στις ιδιότητες εφαρμογής web στην πύλη του Azure.

1. Άνοιγμα GitBash (ή μια terminal, εάν είναι Git σε σας `PATH`), αλλαγή σε καταλόγους στον ριζικό κατάλογο της εφαρμογής σας και εκτελέστε τις ακόλουθες εντολές:

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    Θα σας ζητηθεί για τον κωδικό πρόσβασης που δημιουργήσατε νωρίτερα.

    ![Αρχικό Push για Azure μέσω Git][git-initial-push]

2. Αναζήτηση για να **http://[site name].azurewebsites.net/index.php** για να αρχίσετε να χρησιμοποιείτε την εφαρμογή (αυτές οι πληροφορίες θα αποθηκευτούν στον πίνακα εργαλείων σας λογαριασμό):

    ![Τοποθεσία web Azure PHP][running-app]

Μετά τη δημοσίευση της εφαρμογής σας, μπορείτε να αρχίσετε να κάνετε αλλαγές σε αυτό και να χρησιμοποιήσετε Git δημοσίευσή τους.

## <a name="publish-changes-to-your-app"></a>Δημοσίευση αλλαγών σε εφαρμογή σας

Για να δημοσιεύσετε τις αλλαγές της εφαρμογής σας, ακολουθήστε τα παρακάτω βήματα:

1. Κάντε τις αλλαγές της εφαρμογής σας τοπικά.
2. Άνοιγμα GitBash (ή μια terminal, it Git βρίσκεται σε σας `PATH`), αλλαγή σε καταλόγους στον ριζικό κατάλογο της εφαρμογής και εκτελέστε τις ακόλουθες εντολές:

        git add .
        git commit -m "comment describing changes"
        git push azure master

    Θα σας ζητηθεί για τον κωδικό πρόσβασης που δημιουργήσατε νωρίτερα.

    ![Προώθηση αλλαγών στην τοποθεσία σε Azure μέσω Git][git-change-push]

3. Αναζήτηση για να **http://[site name].azurewebsites.net/index.php** για να δείτε την εφαρμογή σας και έχετε κάνει αλλαγές:

    ![Τοποθεσία web Azure PHP][running-app]

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751), όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή web μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>Ενεργοποίηση σύνθεσης αυτοματισμού με την επέκταση σύνθεσης

Από προεπιλογή, της διαδικασίας ανάπτυξης git στην εφαρμογή υπηρεσίας δεν κάνετε τίποτα με composer.json, εάν έχετε μία στο έργο σας PHP. Μπορείτε να ενεργοποιήσετε την επεξεργασία κατά τη διάρκεια composer.json `git push` , ενεργοποιώντας την επέκταση σύνθεσης.

1. Στο PHP σας στο web blade της εφαρμογής στην [πύλη του Azure][management-portal], κάντε κλικ στην επιλογή **Εργαλεία** > **επεκτάσεις**.

    ![Ρυθμίσεις επέκταση σύνθεσης][composer-extension-settings]

2. Κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **σύνθεσης**.

    ![Προσθήκη επέκτασης σύνθεσης][composer-extension-add]
    
3. Κάντε κλικ στο **κουμπί OK** για να αποδεχτείτε τους όρους νομική. Κάντε κλικ στο **κουμπί OK** ξανά για να προσθέσετε την επέκταση.

    Το blade **εγκατεστημένες επεκτάσεις** τώρα θα εμφανίσει την επέκταση σύνθεσης.  
    ![Προβολή επέκταση σύνθεσης][composer-extension-view]
    
4. Τώρα, εκτελέστε `git add`, `git commit`, και `git push` στην προηγούμενη ενότητα. Τώρα βλέπετε ότι σύνθεσης κατά την εγκατάσταση των εξαρτήσεων ορίζεται στο composer.json.

    ![Επιτυχίας επέκταση σύνθεσης][composer-extension-success]

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές PHP](/develop/php/).

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
