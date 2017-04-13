<properties
    pageTitle="Πώς να χρησιμοποιείτε Twilio για φωνή και το SMS (PHP) | Microsoft Azure"
    description="Μάθετε πώς να πραγματοποιήσω μια τηλεφωνική κλήση και να στείλετε ένα μήνυμα SMS με την υπηρεσία Twilio API σε Azure. Δείγματα κώδικα γραμμένο σε PHP."
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Πώς να χρησιμοποιείτε Twilio για φωνή και δυνατότητες SMS στο PHP
Αυτός ο οδηγός παρουσιάζει πώς μπορείτε να εκτελείτε συνήθεις εργασίες προγραμματισμού με την υπηρεσία Twilio API σε Azure. Τα σενάρια καλύπτεται περιλαμβάνουν μια τηλεφωνική κλήση και να στείλετε ένα μήνυμα σύντομο μήνυμα υπηρεσίας (SMS). Για περισσότερες πληροφορίες σχετικά με την Twilio και τη χρήση φωνής και SMS στις εφαρμογές σας, ανατρέξτε στην ενότητα [Επόμενα βήματα](#NextSteps) .

## <a id="WhatIs"></a>Τι είναι το Twilio;
Twilio Ενεργοποίηση το μέλλον της επιχειρήσεις επικοινωνίες, επιτρέποντας στους προγραμματιστές να ενσωματώσετε φωνής, VoIP και μηνυμάτων σε εφαρμογές. Αυτά αναπαράσταση όλα υποδομή απαιτείται σε ένα περιβάλλον βασίζεται στο cloud, καθολικό, το εκθέσετε μέσω της πλατφόρμας επικοινωνίες API Twilio. Οι εφαρμογές είναι απλή για να δημιουργήσετε και μεταβλητού μεγέθους. Απολαύστε ευελιξία με πληρωμή-ως-που μεταβείτε τιμολόγησης και επωφεληθείτε από αξιοπιστία cloud.

**Φωνητικό Twilio** επιτρέπει τις εφαρμογές σας για να κάνετε και να λαμβάνετε τηλεφωνικές κλήσεις. **Twilio SMS** επιτρέπει την εφαρμογή σας για αποστολή και λήψη μηνυμάτων κειμένου. **Twilio προγράμματος-πελάτη** σας επιτρέπει να κάνετε κλήσεις VoIP από οποιοδήποτε τηλέφωνο, tablet ή το πρόγραμμα περιήγησης και υποστηρίζει WebRTC.

## <a id="Pricing"></a>Τις τιμές Twilio και ειδικές προσφορές

Azure οι πελάτες λαμβάνουν [ειδική προσφορά] $10 Twilio πιστωτικής κατά την αναβάθμιση το λογαριασμό σας Twilio. Αυτή η πίστωση Twilio μπορούν να εφαρμοστούν σε οποιαδήποτε χρήση Twilio (πίστωση $10 ισοδύναμο με αποστολή μηνυμάτων όσο 1.000 SMS ή τη λήψη μέχρι 1000 εισερχομένων λεπτά φωνής, ανάλογα με τη θέση προορισμού αριθμό και το μήνυμα ή κλήση του τηλεφώνου). Εξαργύρωση αυτό πιστωτικής Twilio και γρήγορα αποτελέσματα στο: [ahoy.twilio.com/azure].

Twilio είναι μια υπηρεσία διανεμητική. Υπάρχουν χωρίς χρέωση εγκατάστασης και μπορείτε να κλείσετε το λογαριασμό σας οποιαδήποτε στιγμή. Μπορείτε να βρείτε περισσότερες λεπτομέρειες σε [Τιμές Twilio] [twilio_pricing].

## <a id="Concepts"></a>Έννοιες
Το API Twilio είναι RESTful API που παρέχει φωνή και λειτουργικότητα SMS για εφαρμογές. Οι βιβλιοθήκες προγράμματος-πελάτη είναι διαθέσιμες σε πολλές γλώσσες. για μια λίστα, ανατρέξτε στο θέμα [Βιβλιοθήκες API Twilio] [twilio_libraries].

Πλήκτρο στοιχεία του Twilio API είναι ρήματα Twilio και Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Ρήματα Twilio
Το API χρησιμοποιεί Twilio ρήματα; Για παράδειγμα, το ** &lt;Καλώς&gt; ** Ρηματικές καθοδηγεί Twilio ευκρίνεια παράδοσης ενός μηνύματος σε κλήση.

Ακολουθεί μια λίστα των ρημάτων Twilio. Μάθετε περισσότερα σχετικά με τις άλλες ρήματα και τις δυνατότητες μέσω [γλώσσα σήμανσης Twilio τεκμηρίωση] [http://www.twilio.com/docs/api/twiml].

* ** &lt;Κλήσης&gt;**: συνδέεται τον καλούντα σε άλλο τηλέφωνο.
* ** &lt;Συγκέντρωση&gt;**: συλλέγει αριθμητικά ψηφία που εισάγονται στο πληκτρολόγιο του τηλεφώνου.
* ** &lt;Κλείσιμο γραμμής&gt;**: τελειώνει μια κλήση.
* ** &lt;Αναπαραγωγή&gt;**: αναπαραγωγή αρχείου ήχου.
* ** &lt;Παύση&gt;**: σιωπηρά περιμένει για έναν ορισμένο αριθμό δευτερολέπτων.
* ** &lt;Εγγραφή&gt;**: εγγραφών φωνής του καλούντος και επιστρέφει μια διεύθυνση URL ενός αρχείου που περιέχει την εγγραφή.
* ** &lt;Ανακατεύθυνση&gt;**: μεταφέρει τον έλεγχο μιας κλήσης ή SMS το TwiML σε μια διαφορετική διεύθυνση URL.
* ** &lt;Απορρίψετε&gt;**: απορρίπτει μια εισερχόμενη κλήση σας αριθμό Twilio χωρίς χρέωση που
* ** &lt;Καλώς&gt;**: μετατρέπει κείμενο σε ομιλία που έχει γίνει σε κλήση.
* ** &lt;Sms&gt;**: στέλνει ένα μήνυμα SMS.

### <a id="TwiML"></a>TwiML
TwiML είναι ένα σύνολο οδηγιών που βασίζονται σε XML με βάση τα Twilio ρήματα που ενημερώνει Twilio σχετικά με τον τρόπο για να επεξεργαστείτε μια κλήση ή SMS.

Ως παράδειγμα, το παρακάτω TwiML θα μετατροπή του κειμένου **Γεια** σε ομιλία.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Όταν η εφαρμογή σας καλεί το API Twilio, μία από τις παραμέτρους API είναι η διεύθυνση URL που επιστρέφει η απόκριση TwiML. Για σκοπούς ανάπτυξης, μπορείτε να χρησιμοποιήσετε τις διευθύνσεις URL που παρέχονται από Twilio για την παροχή των αποκρίσεων TwiML που χρησιμοποιούνται από τις εφαρμογές σας. Μπορείτε επίσης μπορεί να φιλοξενήσετε τη δική σας διευθύνσεις URL για την παραγωγή των αποκρίσεων TwiML και μια άλλη επιλογή είναι να χρησιμοποιήσετε το αντικείμενο **TwiMLResponse** .

Για περισσότερες πληροφορίες σχετικά με την Twilio ρήματα, τα χαρακτηριστικά και TwiML, ανατρέξτε στο θέμα [TwiML] [twiml]. Για πρόσθετες πληροφορίες σχετικά με το API Twilio, ανατρέξτε στο θέμα [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Δημιουργία λογαριασμού Twilio
Όταν είστε έτοιμοι για να λάβετε ένα λογαριασμό Twilio, εγγραφείτε στο [Δοκιμάστε Twilio] [try_twilio]. Μπορείτε να ξεκινήσετε με ένα δωρεάν λογαριασμό και να αναβαθμίσετε το λογαριασμό σας αργότερα.

Όταν εγγραφείτε για ένα λογαριασμό Twilio, λαμβάνετε Αναγνωριστικό λογαριασμού και ενός διακριτικού ελέγχου ταυτότητας. Και τα δύο θα χρειαστούν για την πραγματοποίηση κλήσεων Twilio API. Για να αποτρέψετε τη μη εξουσιοδοτημένη πρόσβαση στο λογαριασμό σας, αφήστε το διακριτικό ελέγχου ταυτότητας ασφαλή. Το Αναγνωριστικό λογαριασμού και έλεγχος ταυτότητας διακριτικού είναι δυνατό να προβληθούν στη [σελίδα του λογαριασμού Twilio] [twilio_account], στο τα πεδία με την ετικέτα **αναγνωριστικό ΑΣΦΑΛΕΊΑΣ ΛΟΓΑΡΙΑΣΜΟΎ** και **ΔΙΑΚΡΙΤΙΚΟΎ AUTH**, αντίστοιχα.

## <a id="create_app"></a>Δημιουργία μιας εφαρμογής PHP
Μια εφαρμογή PHP που χρησιμοποιεί την υπηρεσία Twilio και εκτελείται στο Azure είναι δεν διαφέρει από οποιαδήποτε άλλη εφαρμογή PHP που χρησιμοποιεί την υπηρεσία Twilio. Ενώ Twilio υπηρεσίες βασίζονται στα ΥΠΌΛΟΙΠΑ και μπορεί να ονομάζεται από PHP με διάφορους τρόπους, σε αυτό το άρθρο εστιάζει στην πώς μπορείτε να χρησιμοποιήσετε τις υπηρεσίες Twilio με [Twilio βιβλιοθήκη για PHP από GitHub][twilio_php]. Για περισσότερες πληροφορίες σχετικά με τη χρήση της βιβλιοθήκης Twilio PHP, ανατρέξτε στο θέμα [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Λεπτομερείς οδηγίες για τη δημιουργία και την ανάπτυξη μιας εφαρμογής Twilio/PHP για Azure είναι διαθέσιμες στη [πώς μπορείτε να κάνετε μια τηλεφωνική κλήση Twilio χρησιμοποιώντας σε μια εφαρμογή PHP στο Azure][howto_phonecall_php].

## <a id="configure_app"></a>Ρύθμιση παραμέτρων της εφαρμογής σας για να χρησιμοποιήσετε βιβλιοθήκες Twilio
Μπορείτε να ρυθμίσετε την εφαρμογή σας για να χρησιμοποιήσετε τη βιβλιοθήκη Twilio για PHP με δύο τρόπους:

1. Λήψη της βιβλιοθήκης Twilio για PHP από GitHub ([https://github.com/twilio/twilio-php][twilio_php]) και να προσθέσετε τον κατάλογο **υπηρεσιών** στην εφαρμογή σας.

    -Ή-

2. Εγκαταστήστε τη βιβλιοθήκη Twilio για PHP ως ένα πακέτο τις ΑΧΛΑΔΙΈΣ. Αυτό μπορεί να εγκατασταθεί με τις παρακάτω εντολές:

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Αφού έχετε εγκαταστήσει τη βιβλιοθήκη Twilio για PHP, μπορείτε να προσθέσετε μια πρόταση **require_once** , στη συνέχεια, στο επάνω μέρος των αρχείων σας PHP αναφοράς στη βιβλιοθήκη του:

        require_once 'Services/Twilio.php';

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Πώς μπορείτε να: πραγματοποίηση μιας εξερχόμενης κλήσης
Η ακόλουθη εικόνα δείχνει τον τρόπο δημιουργίας μιας εξερχόμενης κλήσης με χρήση του την κλάση **Services_Twilio** . Αυτός ο κωδικός χρησιμοποιεί επίσης μια τοποθεσία που παρέχεται από τη Twilio για να επιστρέψει την απάντηση Twilio Markup Language (TwiML). Αντικαταστήστε τις τιμές για τους αριθμούς τηλεφώνου **από** και **έως** και βεβαιωθείτε ότι μπορείτε να επαληθεύσετε τον αριθμό τηλεφώνου **από το** για το λογαριασμό σας Twilio πριν από την εκτέλεση του κώδικα.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Όπως αναφέρθηκε, αυτός ο κωδικός χρησιμοποιεί μια τοποθεσία που παρέχεται από τη Twilio για να επιστρέψει την απάντηση TwiML. Αντί για αυτό, μπορείτε να χρησιμοποιήσετε τη δική σας τοποθεσία για την παροχή της απόκρισης TwiML; Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να παρέχουν απαντήσεις TwiML από δική τοποθεσίας Web](#howto_provide_twiml_responses).


- **Σημείωση**: για να αντιμετωπίσετε τα σφάλματα επικύρωσης πιστοποιητικού SSL, ανατρέξτε στο θέμα [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]


## <a id="howto_send_sms"></a>Πώς μπορείτε να: Αποστολή μηνύματος SMS
Η ακόλουθη εικόνα δείχνει πώς μπορείτε να στείλετε ένα μήνυμα SMS χρησιμοποιώντας την κλάση **Services_Twilio** . Ο αριθμός **από** παρέχεται από Twilio για χρήση δοκιμαστικής έκδοσης λογαριασμούς για την αποστολή μηνυμάτων SMS. Ο αριθμός **να** πρέπει να έχει επαληθευτεί για το λογαριασμό σας Twilio πριν από την εκτέλεση του κώδικα.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Πώς μπορείτε να: παρέχει TwiML απαντήσεις από τη δική σας τοποθεσία Web
Όταν η εφαρμογή σας ξεκινά μια κλήση για το API Twilio, Twilio θα στείλει την πρόσκληση σε μια διεύθυνση URL που αναμένεται να επιστρέψει μια απόκριση TwiML. Το παραπάνω παράδειγμα χρησιμοποιεί τη διεύθυνση URL που παρέχεται από τη Twilio [http://twimlets.com/message][twimlet_message_url]. (Ενώ TwiML έχει σχεδιαστεί για χρήση από Twilio, μπορείτε να προβάλετε τους στο πρόγραμμα περιήγησης. Για παράδειγμα, κάντε κλικ στην επιλογή [http://twimlets.com/message] [ twimlet_message_url] για να δείτε μια κενή `<Response>` στοιχείο. ένα άλλο παράδειγμα, κάντε κλικ στην επιλογή [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] για να δείτε μια `<Response>` στοιχείο που περιέχει ένα `<Say>` στοιχείου.)

Αντί να βασίζεστε στη διεύθυνση URL που παρέχεται από τη Twilio, μπορείτε να δημιουργήσετε τη δική σας τοποθεσία που επιστρέφει αποκρίσεις HTTP. Μπορείτε να δημιουργήσετε την τοποθεσία σε οποιαδήποτε γλώσσα που επιστρέφει XML απαντήσεων; Αυτό το θέμα προϋποθέτει θα χρησιμοποιήσετε PHP για να δημιουργήσετε το TwiML.

Στην επόμενη σελίδα PHP ως αποτέλεσμα μια απάντηση TwiML που αναφέρει ότι **Γεια** στην κλήση.

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

Όπως μπορείτε να δείτε από το παραπάνω παράδειγμα, η απόκριση TwiML είναι απλώς ένα έγγραφο XML. Η βιβλιοθήκη Twilio για PHP περιέχει κλάσεις που θα δημιουργήσετε TwiML για εσάς. Το παρακάτω παράδειγμα δημιουργεί ισοδύναμη απάντηση, όπως φαίνεται παραπάνω, αλλά χρησιμοποιεί το **υπηρεσίες\_Twilio\_Twiml** τάξης στη βιβλιοθήκη Twilio για PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Για περισσότερες πληροφορίες σχετικά με το TwiML, ανατρέξτε στο θέμα [https://www.twilio.com/docs/api/twiml][twiml_reference].

Μόλις PHP σελίδα σας ρυθμιστεί ώστε να παρέχουν απαντήσεις TwiML, χρησιμοποιήστε τη διεύθυνση URL της σελίδας PHP όπως η διεύθυνση URL που εισήχθησαν σε το `Services_Twilio->account->calls->create` μέθοδο. Για παράδειγμα, εάν έχετε μια εφαρμογή Web με το όνομα **MyTwiML** αναπτυχθεί σε μια Azure φιλοξενούνται υπηρεσία, και το όνομα της σελίδας PHP είναι **mytwiml.php**, τη διεύθυνση URL μπορούν να περάσουν **Services_Twilio -> λογαριασμού -> κλήσεις -> Δημιουργία** όπως φαίνεται στο ακόλουθο παράδειγμα:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Για πρόσθετες πληροφορίες σχετικά με τη χρήση Twilio Azure με PHP, δείτε [πώς μπορείτε να κάνετε μια τηλεφωνική κλήση Twilio χρησιμοποιώντας σε μια εφαρμογή PHP στο Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Πώς μπορείτε να: χρήση των υπηρεσιών επιπλέον Twilio
Εκτός από τα παραδείγματα που εμφανίζονται εδώ, Twilio προσφέρει API που βασίζεται στο web που μπορείτε να χρησιμοποιήσετε για να αξιοποιήσετε πρόσθετες λειτουργίες Twilio από την εφαρμογή του Azure. Για πλήρεις λεπτομέρειες, ανατρέξτε στην [τεκμηρίωση Twilio API] [twilio_api_documentation].

## <a id="NextSteps"></a>Επόμενα βήματα
Τώρα που μάθατε τα βασικά στοιχεία της υπηρεσίας Twilio, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα:

* [Οδηγίες ασφαλείας Twilio] [twilio_security_guidelines]
* [Οδηγοί Twilio ΔΙΑΔΙΚΑΣΙΕΣ και παράδειγμα κώδικα] [twilio_howtos]
* [Γρήγορη έναρξη Twilio προγράμματα εκμάθησης][twilio_quickstarts]
* [Twilio σε GitHub] [twilio_on_github]
* [Επικοινωνήστε με την υποστήριξη Twilio] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
