<properties 
    pageTitle="Δημιουργία εφαρμογής web PHP MySQL στο Azure εφαρμογής υπηρεσίας και ανάπτυξη χρησιμοποιώντας FTP" 
    description="Ένα πρόγραμμα εκμάθησης που δείχνει πώς μπορείτε να δημιουργήσετε μια εφαρμογή web PHP που αποθηκεύει δεδομένα σε ανάπτυξη FTP MySQL και χρήση σε Azure." 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


#<a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Δημιουργία εφαρμογής web PHP MySQL στο Azure εφαρμογής υπηρεσίας και ανάπτυξη χρησιμοποιώντας FTP

Αυτό το πρόγραμμα εκμάθησης σας δείχνει πώς μπορείτε να δημιουργήσετε μια εφαρμογή web PHP MySQL και τον τρόπο ανάπτυξης χρησιμοποιώντας FTP. Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε [PHP][install-php], [MySQL][install-mysql], σε διακομιστή web, καθώς και ένα πρόγραμμα-πελάτη FTP εγκατεστημένο στον υπολογιστή σας. Μπορείτε να ακολουθήσετε τις οδηγίες σε αυτό το πρόγραμμα εκμάθησης σε οποιοδήποτε λειτουργικό σύστημα, συμπεριλαμβανομένων των Windows, Mac και Linux. Κατά την ολοκλήρωση αυτού του οδηγού, θα έχετε μια εφαρμογή web PHP/MySQL εκτελείται στο Azure.
 
Θα μάθετε:

* Μάθετε πώς μπορείτε να δημιουργήσετε μια εφαρμογή web και μια βάση δεδομένων MySQL με την πύλη Azure. Επειδή PHP είναι ενεργοποιημένη στις εφαρμογές Web, από προεπιλογή, τίποτα ειδική απαιτείται για να εκτελέσετε τον κώδικα PHP.
* Μάθετε πώς μπορείτε να δημοσιεύσετε την εφαρμογή με το Azure μέσω FTP.
 
Ακολουθώντας αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να δημιουργήσετε μια εφαρμογή web απλό εγγραφής στο PHP. Η εφαρμογή θα να φιλοξενούνται σε μια εφαρμογή Web. Στιγμιότυπο οθόνης της εφαρμογής ολοκληρωμένη είναι κάτω από:

![Τοποθεσία Web Azure PHP][running-app]

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή λογαριασμού, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751), όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή web μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Απαιτείται καμία πιστωτικές κάρτες, χωρίς δεσμεύσεις. 


##<a name="create-a-web-app-and-set-up-ftp-publishing"></a>Δημιουργία εφαρμογής web και να ρυθμίσετε FTP δημοσίευσης

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε μια εφαρμογή web και μια βάση δεδομένων MySQL:

1. Συνδεθείτε στην [πύλη του Azure][management-portal].
2. Κάντε κλικ στο **+ νέο** εικονίδιο στην επάνω αριστερή πλευρά της πύλης Azure.

    ![Δημιουργία νέας τοποθεσίας Web Azure][new-website]

3. Στο πλαίσιο Αναζήτηση πληκτρολογήστε **εφαρμογής Web + MySQL** και κάντε κλικ στην **εφαρμογή Web + MySQL**.

    ![Δημιουργία προσαρμοσμένου μια νέα τοποθεσία Web][custom-create]

4. Κάντε κλικ στην επιλογή **Δημιουργία**. Εισαγάγετε ένα όνομα μοναδικό εφαρμογής υπηρεσίας, ένα έγκυρο όνομα για την ομάδα των πόρων και ένα νέο σχέδιο παροχής υπηρεσιών.

    ![Ορισμός όνομα ομάδας πόρων][resource-group]


6. Εισαγάγετε τιμές για τη νέα βάση δεδομένων, συμπεριλαμβανομένων των συμφωνώντας να νομική τους όρους της.

    ![Δημιουργία νέας βάσης δεδομένων MySQL][new-mysql-db]
    
7. Αφού δημιουργηθεί το web app, θα δείτε το νέο blade εφαρμογής υπηρεσίας.


6. Κάντε κλικ στην επιλογή **Ρυθμίσεις** > **ανάπτυξης διαπιστευτήρια**. 

    ![Ορισμός διαπιστευτηρίων ανάπτυξης][set-deployment-credentials]

7. Για να ενεργοποιηθεί η δημοσίευση FTP, πρέπει να δώσετε ένα όνομα χρήστη και τον κωδικό πρόσβασης. Αποθηκεύστε τα διαπιστευτήρια και σημειώστε το όνομα χρήστη και τον κωδικό πρόσβασης που δημιουργείτε.

    ![Δημιουργία δημοσίευσης διαπιστευτήρια][portal-ftp-username-password]

##<a name="build-and-test-your-app-locally"></a>Δημιουργία και να ελέγξετε την εφαρμογή σας τοπικά

Η εφαρμογή δήλωσης είναι μια απλή εφαρμογή PHP που σας επιτρέπει να εγγραφείτε για ένα συμβάν, παρέχοντας το όνομα διεύθυνση ηλεκτρονικού ταχυδρομείου. Πληροφορίες σχετικά με την προηγούμενη εγγεγραμμένοι PartnerDirect εμφανίζεται σε έναν πίνακα. Πληροφορίες εγγραφής αποθηκεύονται σε μια βάση δεδομένων MySQL. Η εφαρμογή αποτελείται από δύο αρχεία:

* **Index.PHP**: Εμφανίζει μια φόρμα για την καταχώρηση και σε έναν πίνακα που περιέχει τις πληροφορίες registrant.
* **CreateTable.PHP**: δημιουργεί το MySQL πίνακα για την εφαρμογή. Αυτό το αρχείο θα χρησιμοποιηθούν μόνο μία φορά.

Για να δημιουργήσετε και να εκτελέσετε την εφαρμογή τοπικά, ακολουθήστε τα παρακάτω βήματα. Σημειώστε ότι αυτά τα βήματα θεωρείται ότι έχετε PHP, MySQL, και σε διακομιστή web ρύθμιση στον τοπικό υπολογιστή σας και που έχετε ενεργοποιήσει την [επέκταση ΠΟΠ για MySQL][pdo-mysql].

1. Δημιουργήστε μια βάση δεδομένων MySQL που ονομάζεται `registration`. Μπορείτε να το κάνετε από τη γραμμή εντολών MySQL με αυτήν την εντολή:

        mysql> create database registration;

2. Στον ριζικό κατάλογο του διακομιστή web, δημιουργήστε ένα φάκελο που ονομάζεται `registration` και να δημιουργήσετε δύο αρχεία σε αυτό - μία που ονομάζεται `createtable.php` και μία που ονομάζεται `index.php`.

3. Άνοιγμα του `createtable.php` αρχείου σε ένα πρόγραμμα επεξεργασίας κειμένου ή IDE και προσθέστε τον παρακάτω κώδικα. Αυτός ο κώδικας θα χρησιμοποιηθεί για τη δημιουργία του `registration_tbl` πίνακα στο το `registration` βάσης δεδομένων.

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > Θα πρέπει να ενημερώσετε τις τιμές για <code>$user</code> και <code>$pwd</code> με το τοπικό όνομα χρήστη MySQL και τον κωδικό πρόσβασης.

4. Ανοίξτε ένα πρόγραμμα περιήγησης web και μεταβείτε [http://localhost/registration/createtable.php][localhost-createtable]. Αυτό θα δημιουργήσει το `registration_tbl` πίνακα στη βάση δεδομένων.

5. Ανοίξτε το αρχείο **index.php** σε ένα πρόγραμμα επεξεργασίας κειμένου ή IDE και προσθέστε το βασικό κώδικα HTML και CSS για τη σελίδα (ο κώδικας PHP θα προστεθούν στα επόμενα βήματα).

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

        ?>
        </body>
        </html>

6. Μέσα στα tag PHP, προσθέστε PHP κώδικα για τη σύνδεση με τη βάση δεδομένων.

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > Ξανά, θα πρέπει να ενημερώσετε τις τιμές για <code>$user</code> και <code>$pwd</code> με το τοπικό όνομα χρήστη MySQL και τον κωδικό πρόσβασης.

7. Μετά τον κωδικό σύνδεσης βάσης δεδομένων, προσθέστε τον κώδικα για την εισαγωγή πληροφοριών δήλωσης στη βάση δεδομένων.

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

8. Τέλος, μετά από τον παραπάνω κώδικα, προσθέστε κώδικα για την ανάκτηση δεδομένων από τη βάση δεδομένων.

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

Τώρα μπορείτε να περιηγηθείτε στο [http://localhost/registration/index.php] [ localhost-index] για να ελέγξετε την εφαρμογή.

##<a name="get-mysql-and-ftp-connection-information"></a>Λήψη πληροφοριών σύνδεσης MySQL και FTP

Για να συνδεθείτε με τη βάση δεδομένων MySQL που εκτελείται σε εφαρμογές Web, σας θα χρειάζονται τις πληροφορίες σύνδεσης. Για να λάβετε πληροφορίες σύνδεσης MySQL, ακολουθήστε τα παρακάτω βήματα:

1. Από την υπηρεσία εφαρμογής web app blade, κάντε κλικ στη σύνδεση ομάδα πόρων:

    ![Επιλογή ομάδας πόρων][select-resourcegroup]

1. Από την ομάδα πόρων, κάντε κλικ στη βάση δεδομένων:

    ![Επιλέξτε βάση δεδομένων][select-database]

2. Από τη βάση δεδομένων της σύνοψης, επιλέξτε **Ρυθμίσεις** > **Ιδιότητες**.

    ![Επιλέξτε την εντολή Ιδιότητες][select-properties]
    
2. Σημειώστε τις τιμές για `Database`, `Host`, `User Id`, και `Password`.

    ![Ιδιότητες σημειώσεων][note-properties]

3. Από την εφαρμογή web, κάντε κλικ στη σύνδεση **λήψης δημοσίευση προφίλ** στην κάτω δεξιά γωνία της σελίδας:

    ![Λήψη δημοσίευση προφίλ][download-publish-profile]

4. Άνοιγμα του `.publishsettings` αρχείου σε ένα πρόγραμμα επεξεργασίας XML. 

3. Βρείτε το `<publishProfile >` στοιχείο με `publishMethod="FTP"` που μοιάζει με αυτήν:

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
Σημειώστε την `publishUrl`, `userName`, και `userPWD` χαρακτηριστικά.

##<a name="publish-your-app"></a>Δημοσίευση της εφαρμογής σας

Αφού ελέγξετε τοπικά την εφαρμογή σας, μπορείτε να το δημοσιεύσετε σε εφαρμογή web με χρήση FTP. Ωστόσο, πρέπει πρώτα να ενημερώσετε τις πληροφορίες σύνδεσης βάσης δεδομένων στην εφαρμογή. Χρησιμοποιώντας τις πληροφορίες σύνδεσης βάσης δεδομένων που έχετε λάβει προηγουμένως (σε **λήψη MySQL και πληροφορίες σύνδεσης FTP** ενότητα), ενημερώστε τις παρακάτω πληροφορίες στο **τόσο** το `createdatabase.php` και `index.php` αρχεία με τις κατάλληλες τιμές:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Τώρα είστε έτοιμοι να δημοσιεύσετε την εφαρμογή σας μέσω FTP.

1. Ανοίξτε το πρόγραμμα-πελάτη FTP της επιλογής.

2. Εισαγάγετε το *τμήμα όνομα κεντρικού υπολογιστή* από το `publishUrl` χαρακτηριστικό σημειώσατε παραπάνω στο πρόγραμμα-πελάτη FTP.

3. Εισαγάγετε το `userName` και `userPWD` σημειώσατε επάνω από τα χαρακτηριστικά που δεν έχουν αλλάξει στο πρόγραμμα-πελάτη FTP.

4. Δημιουργήστε μια σύνδεση.

Αφού έχετε συνδέσει θα μπορούν να αποστολή και λήψη αρχείων όπως απαιτείται. Βεβαιωθείτε ότι πραγματοποιείτε αποστολή των αρχείων στον ριζικό κατάλογο, τα οποία είναι `/site/wwwroot`.

Μετά την αποστολή και τα δύο `index.php` και `createtable.php`, αναζητήστε **http://[site name].azurewebsites.net/createtable.php** για να δημιουργήσετε τον πίνακα MySQL για την εφαρμογή και, στη συνέχεια, αναζητήστε **http://[site name].azurewebsites.net/index.php** για να αρχίσετε να χρησιμοποιείτε την εφαρμογή.
 
## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές PHP](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 
