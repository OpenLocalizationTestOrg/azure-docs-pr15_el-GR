<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid (PHP) | Microsoft Azure" 
    description="Μάθετε πώς να στείλετε μήνυμα ηλεκτρονικού ταχυδρομείου με την υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid στην Azure. Δείγματα κώδικα γραμμένο σε PHP." 
    documentationCenter="php" 
    services="" 
    manager="sendgrid" 
    editor="mollybos" 
    authors="thinkingserious"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com"/>
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>Πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid από PHP

Αυτός ο οδηγός παρουσιάζει πώς μπορείτε να εκτελείτε συνήθεις εργασίες προγραμματισμού με την υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid στην Azure. Τα δείγματα που είναι γραμμένες σε PHP.
Τα σενάρια καλύπτεται περιλαμβάνουν **η κατασκευή ηλεκτρονικού ταχυδρομείου**, **τα μηνύματα ηλεκτρονικού ταχυδρομείου**και **Προσθήκη συνημμένων**. Για περισσότερες πληροφορίες σχετικά με SendGrid και αποστολή ηλεκτρονικού ταχυδρομείου, ανατρέξτε στην ενότητα [Επόμενα βήματα](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Τι είναι η υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid;

SendGrid είναι μια [υπηρεσία ηλεκτρονικού ταχυδρομείου που βασίζεται στο cloud] που παρέχει αξιόπιστη [παράδοση μηνυμάτων ηλεκτρονικού ταχυδρομείου συναλλαγών], κλιμάκωση και σε πραγματικό χρόνο ανάλυση μαζί με ευέλικτο API που διευκολύνουν την προσαρμοσμένη ενοποίησης. Κοινά σενάρια χρήσης SendGrid περιλαμβάνουν τα εξής:

-   Αυτόματη αποστολή αποδεικτικών στους πελάτες
-   Διαχείριση διανομής λίστες για την αποστολή τους πελάτες μηνιαία e-fliers και ειδικές προσφορές
-   Συλλογή σε πραγματικό χρόνο μετρήσεις για στοιχεία, όπως αποκλεισμένων ηλεκτρονικού ταχυδρομείου, και την απόκριση πελατών
-   Δημιουργία αναφορών για να σας βοηθήσουν να εντοπίσετε τάσεις
-   Προώθηση ερωτημάτων του πελάτη
- Ειδοποιήσεις ηλεκτρονικού ταχυδρομείου από την εφαρμογή σας

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [https://sendgrid.com][].

## <a name="create-a-sendgrid-account"></a>Δημιουργία λογαριασμού SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Χρήση SendGrid από την εφαρμογή σας PHP

Χρήση SendGrid σε μια εφαρμογή του Azure PHP απαιτεί χωρίς ειδική κωδικοποίηση ή ρύθμισης παραμέτρων. Επειδή SendGrid είναι μια υπηρεσία, θα έχουν πρόσβαση στο ακριβώς από την εφαρμογή σύννεφο μπορεί από μια εφαρμογή εσωτερικής εγκατάστασης.

## <a name="how-to-send-an-email"></a>Πώς μπορείτε να: Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου

Μπορείτε να στείλετε μήνυμα ηλεκτρονικού ταχυδρομείου με τη χρήση SMTP ή το API Web που παρέχεται από SendGrid.

### <a name="smtp-api"></a>SMTP API

Για να στείλετε μήνυμα ηλεκτρονικού ταχυδρομείου με χρήση του API SMTP SendGrid, χρησιμοποιήστε *Σουίφτ λάβατε μέσω ηλεκτρονικού ταχυδρομείου*, μια βιβλιοθήκη που βασίζεται σε στοιχεία για την αποστολή μηνυμάτων ηλεκτρονικού ταχυδρομείου από τις εφαρμογές PHP. Μπορείτε να κάνετε λήψη της βιβλιοθήκης *Σουίφτ λάβατε μέσω ηλεκτρονικού ταχυδρομείου* από [http://swiftmailer.org/download][] v5.3.0 (χρήση [σύνθεσης] για να εγκαταστήσετε το Σουίφτ λάβατε μέσω ηλεκτρονικού ταχυδρομείου). Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου με τη βιβλιοθήκη περιλαμβάνει τη δημιουργία των εμφανίσεων το <span class="auto-style2">Σουίφτ\_SmtpTransport</span>, <span class="auto-style2">Σουίφτ\_λάβατε μέσω ηλεκτρονικού ταχυδρομείου</span>, και <span class="auto-style2">Σουίφτ\_μήνυμα</span> κλάσεις, ορίζοντας τις κατάλληλες ιδιότητες και καλείτε τη <span class="auto-style2">Σουίφτ\_Mailer::send</span> μέθοδο.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */ 
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>API Web

Χρήση του PHP [curl συνάρτηση][] για την αποστολή ηλεκτρονικού ταχυδρομείου με χρήση του API Web SendGrid.

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD'; 

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );
       
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

API Web του SendGrid είναι παρόμοια με μια REST API, αν και δεν είναι πραγματικά RESTful API δεδομένου ότι, σε περισσότερες κλήσεις, και τα δύο ΓΡΉΓΟΡΑ και ΚΑΤΑΧΏΡΗΣΗ ρήματα μπορούν να χρησιμοποιηθούν εναλλακτικά.

## <a name="how-to-add-an-attachment"></a>Πώς μπορείτε να: Προσθήκη συνημμένου

### <a name="smtp-api"></a>SMTP API

Αποστολή συνημμένου με χρήση του API SMTP περιλαμβάνει ένα επιπλέον γραμμή κώδικα στη δέσμη ενεργειών παράδειγμα για την αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου με Σουίφτ λάβατε μέσω ηλεκτρονικού ταχυδρομείου.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     
     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';
     
     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);
     
     // Create a message (subject)
     $message = new Swift_Message($subject);
     
     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

Η πρόσθετη γραμμή του κώδικα είναι ως εξής:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Αυτή η γραμμή του κώδικα καλεί τη μέθοδο Επισύναψη σε το <span class="auto-style2">Σουίφτ\_μήνυμα</span> αντικειμένου και χρησιμοποιεί στατική μέθοδο <span class="auto-style2">fromPath</span> σε το <span class="auto-style2">Σουίφτ\_συνημμένου</span> κλάσης για τη λήψη και να επισυνάψετε ένα αρχείο σε ένα μήνυμα.

### <a name="web-api"></a>API Web

Αποστολή συνημμένου με χρήση του API Web είναι παρόμοια με την αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου με χρήση του API Web. Ωστόσο, σημειώστε ότι στο παράδειγμα που ακολουθεί, ο πίνακας παραμέτρου πρέπει να περιέχει αυτό το στοιχείο:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Παράδειγμα:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';
     
     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );
     
     print_r($params);
     
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Πώς μπορείτε να: χρησιμοποιήστε φίλτρα για να ενεργοποιήσετε την υποσέλιδα, παρακολούθηση και ανάλυση

SendGrid παρέχει λειτουργικότητα επιπλέον ηλεκτρονικού ταχυδρομείου με τη χρήση 'φίλτρα'. Αυτές είναι οι ρυθμίσεις που μπορούν να προστεθούν σε ένα μήνυμα ηλεκτρονικού ταχυδρομείου για να ενεργοποιήσετε συγκεκριμένες λειτουργίες όπως την ενεργοποίηση, κάντε κλικ στην επιλογή παρακολούθηση, Google analytics, συνδρομή παρακολούθησης και ούτω καθεξής.

Φίλτρα μπορούν να εφαρμοστούν σε ένα μήνυμα, χρησιμοποιώντας την ιδιότητα φίλτρα. Κάθε φίλτρο έχει οριστεί από τον κατακερματισμός που περιέχει ρυθμίσεις για συγκεκριμένο φίλτρο. Το παρακάτω παράδειγμα ενεργοποιεί το φίλτρο υποσέλιδο και καθορίζει ένα μήνυμα κειμένου που θα προσαρτηθούν στο κάτω μέρος του μηνύματος ηλεκτρονικού ταχυδρομείου.
Για αυτό το παράδειγμα, θα χρησιμοποιήσουμε [sendgrid php βιβλιοθήκη].
Χρησιμοποιήστε [σύνθεσης] για να εγκαταστήσετε το βιβλιοθήκης:
    
    php composer.phar require sendgrid/sendgrid 2.1.1

Παράδειγμα:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have 
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from 
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If 
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version). 
     # text is your plain-text email 
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html> 
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία για την υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα.

-   Τεκμηρίωση SendGrid: <https://sendgrid.com/docs>
-   Βιβλιοθήκη SendGrid PHP: <https://github.com/sendgrid/sendgrid-php>
-   SendGrid ειδική προσφορά για τους πελάτες του Azure: <https://sendgrid.com/windowsazure.html>

Για περισσότερες πληροφορίες, ανατρέξτε επίσης στο [Κέντρο για προγραμματιστές PHP](/develop/php/).


  [https://sendgrid.com]: https://sendgrid.com
  [https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
  [special offer]: https://www.sendgrid.com/windowsazure.html
  [Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
  [http://swiftmailer.org/Download]: http://swiftmailer.org/download
  [συνάρτηση καμπύλη]: http://php.net/curl
  [υπηρεσία ηλεκτρονικού ταχυδρομείου που βασίζεται στο cloud]: https://sendgrid.com/email-solutions
  [παράδοση συναλλαγών μηνυμάτων ηλεκτρονικού ταχυδρομείου]: https://sendgrid.com/transactional-email
  [βιβλιοθήκη sendgrid php]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
  [Σύνθεσης]: https://getcomposer.org/download/
