<properties
    pageTitle="Πώς μπορείτε να κάνετε μια τηλεφωνική κλήση από Twilio (PHP) | Microsoft Azure"
    description="Μάθετε πώς να πραγματοποιήσω μια τηλεφωνική κλήση και να στείλετε ένα μήνυμα SMS με την υπηρεσία Twilio API σε Azure. Δείγματα είναι για την εφαρμογή PHP."
    documentationCenter="php"
    services=""
    authors="devinrader"
    manager="twilio"
    editor="mollybos"/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="11/25/2014"
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Πώς μπορείτε να κάνετε μια τηλεφωνική κλήση με χρήση Twilio σε μια εφαρμογή PHP στο Azure

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Twilio για να πραγματοποιήσετε μια κλήση από μια ιστοσελίδα PHP που φιλοξενούνται στο Azure. Την εφαρμογή που προκύπτει θα γίνεται ερώτηση στο χρήστη για τιμές τηλεφωνικής κλήσης, όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης.

![Φόρμα Azure κλήσης με χρήση του Twilio και PHP][twilio_php]

Θα πρέπει να κάνετε τα εξής για να χρησιμοποιήσετε τον κωδικό σε αυτό το θέμα:

1. Απόκτηση ένα λογαριασμό Twilio και έλεγχος ταυτότητας διακριτικού. Για να ξεκινήσετε με Twilio, να αξιολογήσετε τις τιμές στο [http://www.twilio.com/pricing][twilio_pricing]. Μπορείτε να εγγραφείτε για μια δοκιμαστική λογαριασμό στο [https://www.twilio.com/try-twilio][try_twilio]. Για πληροφορίες σχετικά με το API που παρέχεται από Twilio, ανατρέξτε στο θέμα [http://www.twilio.com/api][twilio_api].
2. Αποκτήστε τη [βιβλιοθήκη Twilio για PHP](https://github.com/twilio/twilio-php) ή εγκαταστήστε την ως πακέτο τις ΑΧΛΑΔΙΈΣ. Για περισσότερες πληροφορίες, ανατρέξτε στο [αρχείο readme](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Εγκαταστήστε το Azure SDK για PHP. Για μια επισκόπηση της SDK και οδηγίες σχετικά με την εγκατάσταση του, ανατρέξτε στο θέμα [Ρύθμιση του SDK Azure για PHP][setup_php_sdk].

## <a name="create-a-web-form-for-making-a-call"></a>Δημιουργία φόρμας web για την πραγματοποίηση κλήσης

Ο ακόλουθος κώδικας HTML δείχνει πώς μπορείτε να δημιουργήσετε μια ιστοσελίδα (**callform.html**) που ανακτά δεδομένα χρήστη για την πραγματοποίηση κλήσης:

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Δημιουργία του κώδικα για την πραγματοποίηση της κλήσης
Ο ακόλουθος κώδικας εμφανίζει τον τρόπο δημιουργίας μιας σελίδας web (**makecall.php**) που ονομάζεται όταν ο χρήστης υποβάλλει τη φόρμα που εμφανίζεται με **callform.html**. Ο κώδικας που φαίνεται παρακάτω δημιουργεί το μήνυμα κλήσης και την κλήση. (Χρησιμοποιήστε το λογαριασμό Twilio και έλεγχος ταυτότητας διακριτικό αντί για τις τιμές του πλαισίου κράτησης θέσης που έχουν εκχωρηθεί σε **$sid** και **$token** τον παρακάτω κώδικα.)

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number,
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

Εκτός από την πραγματοποίηση της κλήσης, **makecall.php** εμφανίζει ορισμένα κλήση μετα-δεδομένα (παράδειγμα που φαίνεται στο παρακάτω στιγμιότυπο οθόνης). Για περισσότερες πληροφορίες σχετικά με την κλήση μετα-δεδομένων, ανατρέξτε στο θέμα [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Απάντηση κλήσης Azure χρησιμοποιώντας Twilio και PHP][twilio_php_response]

## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή
Το επόμενο βήμα είναι να αναπτύξετε την εφαρμογή σας σε τοποθεσίες Web Azure. Τα ακόλουθα άρθρα περιέχουν τις πληροφορίες για τη δημιουργία μιας τοποθεσίας Web και ανάπτυξη του κώδικα με Git, FTP ή WebMatrix (αν και δεν είναι όλες οι πληροφορίες σε κάθε άρθρο σχετικές):

* [Δημιουργήστε μια τοποθεσία Web Azure PHP MySQL και ανάπτυξη χρησιμοποιώντας Git][website-git]
* [Δημιουργήστε μια τοποθεσία Azure Web PHP MySQL και ανάπτυξη χρησιμοποιώντας FTP][website-ftp]

## <a name="next-steps"></a>Επόμενα βήματα
Αυτός ο κωδικός παρέχεται για να σας δείξουν βασικές λειτουργίες χρησιμοποιώντας Twilio στο PHP στο Azure. Πριν αναπτύξετε για Azure παραγωγή, ενδέχεται να θέλετε να προσθέσετε περισσότερες χειρισμού σφαλμάτων ή άλλες δυνατότητες. Για παράδειγμα:

* Αντί να χρησιμοποιήσετε μια φόρμα web, μπορείτε να χρησιμοποιήσετε για την αποθήκευση αριθμών τηλεφώνου και καλέστε κειμένου Azure χώρο αποθήκευσης αντικειμένων blob ή βάση δεδομένων SQL. Για πληροφορίες σχετικά με τη χρήση Azure χώρο αποθήκευσης αντικειμένων blob PHP, ανατρέξτε στο θέμα [Χρήση του χώρου αποθήκευσης Azure με εφαρμογές PHP][howto_blob_storage_php]. Για πληροφορίες σχετικά με τη χρήση της βάσης δεδομένων SQL PHP, ανατρέξτε στο θέμα [Χρήση βάσης δεδομένων SQL με εφαρμογές PHP][howto_sql_azure_php].
* Ο κώδικας **makecall.php** χρησιμοποιεί διεύθυνση URL που παρέχεται από τη Twilio ([http://twimlets.com/message][twimlet_message_url]) για να δώσετε μια απόκριση Twilio Markup Language (TwiML) που σας πληροφορεί Twilio πώς μπορείτε να συνεχίσετε την κλήση. Για παράδειγμα, το TwiML που επιστρέφεται μπορεί να περιέχει ένα `<Say>` Ρηματικές που προκύπτει στο κείμενο που εκφωνείται προς τον παραλήπτη κλήσης. Αντί να χρησιμοποιήσετε τη διεύθυνση URL που παρέχεται από τη Twilio, ενδέχεται να μπορείτε να δημιουργήσετε τη δική σας υπηρεσία να απαντήσετε σε πρόσκληση του Twilio; Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να Twilio Χρήση φωνής και δυνατότητες SMS στο PHP][howto_twilio_voice_sms_php]. Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με TwiML στο [http://www.twilio.com/docs/api/twiml][twiml], και περισσότερες πληροφορίες σχετικά με το `<Say>` και άλλα ρήματα Twilio μπορείτε να βρείτε στο [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Διαβάστε τις οδηγίες ασφαλείας Twilio στο [https://www.twilio.com/docs/security][twilio_docs_security].

Για πρόσθετες πληροφορίες σχετικά με το Twilio, ανατρέξτε στο θέμα [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Δείτε επίσης
* [Πώς να χρησιμοποιείτε Twilio για φωνή και δυνατότητες SMS στο PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
