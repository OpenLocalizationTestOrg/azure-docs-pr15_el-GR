<properties 
    pageTitle="Πώς να χρησιμοποιείτε Twilio για φωνή και το SMS (φωνητικής γραφής) | Microsoft Azure" 
    description="Μάθετε πώς να πραγματοποιήσω μια τηλεφωνική κλήση και να στείλετε ένα μήνυμα SMS με την υπηρεσία Twilio API σε Azure. Δείγματα κώδικα γραμμένο σε κείμενο φωνητικής γραφής." 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Πώς να χρησιμοποιείτε Twilio για φωνή και δυνατότητες SMS στο κείμενο φωνητικής γραφής
Αυτός ο οδηγός παρουσιάζει πώς μπορείτε να εκτελείτε συνήθεις εργασίες προγραμματισμού με την υπηρεσία Twilio API σε Azure. Τα σενάρια καλύπτεται περιλαμβάνουν μια τηλεφωνική κλήση και να στείλετε ένα μήνυμα σύντομο μήνυμα υπηρεσίας (SMS). Για περισσότερες πληροφορίες σχετικά με την Twilio και τη χρήση φωνής και SMS στις εφαρμογές σας, ανατρέξτε στην ενότητα [Επόμενα βήματα](#NextSteps) .

## <a id="WhatIs"></a>Τι είναι το Twilio;
Twilio είναι ένα API της υπηρεσίας web τηλεφωνίας που σας επιτρέπει να χρησιμοποιείτε το υπάρχον web γλώσσες και δεξιότητες για να δημιουργήσει η φωνή και το SMS εφαρμογές. Twilio είναι μια υπηρεσία τρίτων κατασκευαστών (δεν δυνατότητα του Azure και όχι ένα προϊόν της Microsoft).

**Φωνητικό Twilio** επιτρέπει τις εφαρμογές σας για να κάνετε και να λαμβάνετε τηλεφωνικές κλήσεις. **Twilio SMS** επιτρέπει τις εφαρμογές σας για να κάνετε και να λαμβάνετε μηνύματα SMS. **Πρόγραμμα-πελάτης Twilio** επιτρέπει τις εφαρμογές σας για να ενεργοποιήσετε την φωνητικής επικοινωνίας με υπάρχουσες συνδέσεις στο Internet, συμπεριλαμβανομένων των συνδέσεις κινητού.

## <a id="Pricing"></a>Τις τιμές Twilio και ειδικές προσφορές
Πληροφορίες σχετικά με τις τιμές Twilio είναι διαθέσιμη από [Τις τιμές Twilio] [twilio_pricing]. Οι πελάτες Azure λαμβάνουν μια [ειδική προσφορά][special_offer]: δωρεάν πιστοληπτικής ικανότητας 1000 κείμενα ή 1000 εισερχομένων λεπτά. Για να εγγραφείτε για αυτήν την προσφορά ή να λάβετε περισσότερες πληροφορίες, επισκεφθείτε την τοποθεσία [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Έννοιες
Το API Twilio είναι RESTful API που παρέχει φωνή και λειτουργικότητα SMS για εφαρμογές. Οι βιβλιοθήκες προγράμματος-πελάτη είναι διαθέσιμες σε πολλές γλώσσες. για μια λίστα, ανατρέξτε στο θέμα [Βιβλιοθήκες API Twilio] [twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML είναι ένα σύνολο οδηγιών που βασίζονται σε XML που ενημερώνει Twilio σχετικά με τον τρόπο για να επεξεργαστείτε μια κλήση ή SMS.

Ως παράδειγμα, το παρακάτω TwiML θα μετατροπή του κειμένου **Γεια** σε ομιλία.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Όλα τα έγγραφα TwiML έχουν `<Response>` ως τους ριζικό στοιχείο. Από εδώ, μπορείτε να χρησιμοποιήσετε ρήματα Twilio για να ορίσετε τη συμπεριφορά της εφαρμογής σας.

### <a id="Verbs"></a>Ρήματα TwiML
Twilio ρήματα είναι ετικέτες XML που ενημερώνουν το Twilio τι να **κάνετε**. Για παράδειγμα, το ** &lt;Καλώς&gt; ** Ρηματικές καθοδηγεί Twilio ευκρίνεια παράδοσης ενός μηνύματος σε κλήση. 

Ακολουθεί μια λίστα των ρημάτων Twilio.

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

Για περισσότερες πληροφορίες σχετικά με την Twilio ρήματα, τα χαρακτηριστικά και TwiML, ανατρέξτε στο θέμα [TwiML] [twiml]. Για πρόσθετες πληροφορίες σχετικά με το API Twilio, ανατρέξτε στο θέμα [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Δημιουργία λογαριασμού Twilio
Όταν είστε έτοιμοι για να λάβετε ένα λογαριασμό Twilio, εγγραφείτε στο [Δοκιμάστε Twilio] [try_twilio]. Μπορείτε να ξεκινήσετε με ένα δωρεάν λογαριασμό και να αναβαθμίσετε το λογαριασμό σας αργότερα.

Όταν εγγραφείτε για ένα λογαριασμό Twilio, θα λάβετε έναν αριθμό τηλεφώνου δωρεάν για την εφαρμογή σας. Θα λάβετε επίσης ένα λογαριασμό αναγνωριστικό ΑΣΦΑΛΕΊΑΣ και ένα διακριτικό auth. Και τα δύο θα χρειαστούν για την πραγματοποίηση κλήσεων Twilio API. Για να αποτρέψετε τη μη εξουσιοδοτημένη πρόσβαση στο λογαριασμό σας, αφήστε το διακριτικό ελέγχου ταυτότητας ασφαλή. Το λογαριασμό αναγνωριστικό ΑΣΦΑΛΕΊΑΣ και διακριτικού auth είναι δυνατό να προβληθούν στη [σελίδα του λογαριασμού Twilio][twilio_account], στο τα πεδία με την ετικέτα **αναγνωριστικό ΑΣΦΑΛΕΊΑΣ ΛΟΓΑΡΙΑΣΜΟΎ** και **ΔΙΑΚΡΙΤΙΚΟΎ AUTH**, αντίστοιχα.

### <a id="VerifyPhoneNumbers"></a>Επαλήθευση αριθμών τηλεφώνου
Εκτός από τον αριθμό που παρέχονται από Twilio, μπορείτε επίσης να επαληθεύσετε αριθμούς ελέγχου (δηλαδή το κινητό τηλέφωνο ή το σπίτι αριθμός τηλεφώνου) για χρήση στις εφαρμογές σας. 

Για πληροφορίες σχετικά με τον τρόπο για να επιβεβαιώσετε έναν αριθμό τηλεφώνου, ανατρέξτε στο θέμα [Διαχείριση αριθμών] [verify_phone].

## <a id="create_app"></a>Δημιουργία μιας εφαρμογής φωνητικής γραφής
Μια εφαρμογή φωνητικής γραφής που χρησιμοποιεί την υπηρεσία Twilio και εκτελείται στο Azure είναι δεν διαφέρει από οποιαδήποτε άλλη εφαρμογή φωνητικής γραφής που χρησιμοποιεί την υπηρεσία Twilio. Ενώ Twilio υπηρεσίες RESTful και μπορεί να ονομάζεται από κείμενο φωνητικής γραφής με διάφορους τρόπους, σε αυτό το άρθρο εστιάζει σχετικά με τον τρόπο χρήσης των υπηρεσιών Twilio με [Twilio βιβλιοθήκη Βοήθειας για το κείμενο φωνητικής γραφής][twilio_ruby].

Αρχικά, [γίνεται η ρύθμιση μιας νέας Εικονική Linux Azure] [ azure_vm_setup] ως μια υπηρεσία παροχής φιλοξενίας για τη νέα εφαρμογή web φωνητικής γραφής. Παράβλεψη τα βήματα που αφορούν τη δημιουργία μιας εφαρμογής ράβδους του, απλώς ρυθμίζουν την εικονική Μηχανή. Βεβαιωθείτε ότι έχετε δημιουργήσει ένα τελικό σημείο με μια εξωτερική θύρα 80 και μια εσωτερική θύρα 5000.

Στα παρακάτω παραδείγματα, θα χρησιμοποιήσουμε [Sinatra][sinatra], ένα πλαίσιο πολύ απλής web για κείμενο φωνητικής γραφής. Ωστόσο, μπορείτε να χρησιμοποιήσετε σίγουρα στη βιβλιοθήκη Βοήθειας Twilio για κείμενο φωνητικής γραφής με οποιαδήποτε άλλα web του πλαισίου, συμπεριλαμβανομένων φωνητικής γραφής του υπολογιστή.

SSH στο νέο σας Εικονική και να δημιουργήσετε έναν κατάλογο για τη νέα εφαρμογή. Μέσα σε αυτόν τον κατάλογο, δημιουργήστε ένα αρχείο που ονομάζεται Gemfile και αντιγράψτε τον παρακάτω κώδικα σε αυτήν:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Στη γραμμή εντολών, εκτελέστε `bundle install`. Αυτό θα εγκαταστήσει τις εξαρτήσεις παραπάνω. Στη συνέχεια, δημιουργήστε ένα αρχείο που ονομάζεται `web.rb`. Αυτό θα όπου βρίσκεται ο κωδικός για την εφαρμογή web. Επικολλήστε τον ακόλουθο κώδικα σε αυτήν:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Σε αυτό το σημείο θα πρέπει η εκτέλεση της εντολής `ruby web.rb -p 5000`. Αυτό θα αυξομείωσης προς τα επάνω σε διακομιστή web μικρές στη θύρα 5000. Θα πρέπει να μπορείτε να αναζητήσετε αυτής της εφαρμογής στο πρόγραμμα περιήγησής σας, επισκεφθείτε τη διεύθυνση URL που γίνεται η ρύθμιση σας Εικονική Azure. Αφού μπορείτε να επικοινωνήσετε μαζί του web app στο πρόγραμμα περιήγησης, είστε έτοιμοι να ξεκινήσετε να δημιουργείτε μια εφαρμογή Twilio.

## <a id="configure_app"></a>Ρύθμιση παραμέτρων της εφαρμογής σας για να χρησιμοποιήσετε Twilio
Μπορείτε να ρυθμίσετε τις παραμέτρους του web app για να χρησιμοποιήσετε τη βιβλιοθήκη Twilio, ενημερώνοντας το `Gemfile` για να συμπεριλάβετε αυτή τη γραμμή:

    gem 'twilio-ruby'

Στη γραμμή εντολών, εκτελέστε `bundle install`. Ανοίξτε το τώρα `web.rb` και αυτή η γραμμή στο επάνω μέρος, συμπεριλαμβανομένων:

    require 'twilio-ruby'

Είστε τώρα έτοιμοι να χρησιμοποιήσετε τη βιβλιοθήκη Βοήθειας Twilio για κείμενο φωνητικής γραφής στην εφαρμογή web.

## <a id="howto_make_call"></a>Πώς μπορείτε να: πραγματοποίηση μιας εξερχόμενης κλήσης
Η ακόλουθη εικόνα δείχνει τον τρόπο δημιουργίας μιας εξερχόμενης κλήσης. Βασικές έννοιες περιλαμβάνουν χρησιμοποιώντας τη βιβλιοθήκη Βοήθειας Twilio για κείμενο φωνητικής γραφής για την πραγματοποίηση κλήσεων REST API και απόδοση TwiML. Αντικαταστήστε τις τιμές για τους αριθμούς τηλεφώνου **από** και **έως** και βεβαιωθείτε ότι μπορείτε να επαληθεύσετε τον αριθμό τηλεφώνου **από το** για το λογαριασμό σας Twilio πριν από την εκτέλεση του κώδικα.

Αυτή η συνάρτηση για να προσθέσετε `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
Εάν έχετε άνοιγμα προς τα επάνω `http://yourdomain.cloudapp.net/make_call` σε ένα πρόγραμμα περιήγησης, που θα ενεργοποιεί την κλήση για το API Twilio για να κάνετε την κλήση. Τα πρώτα δύο παραμέτρους στο `client.account.calls.create` είναι αρκετά αυτονόητες: ο αριθμός είναι η κλήση `from` και ο αριθμός είναι η κλήση `to`. 

Η τρίτη παράμετρος (`url`) είναι η διεύθυνση URL που ζητά Twilio για να λάβετε οδηγίες σχετικά με το τι να κάνετε όταν η κλήση συνδεθεί. Σε αυτήν την περίπτωση θα σας γίνεται η ρύθμιση μιας διεύθυνσης URL (`http://yourdomain.cloudapp.net`) που επιστρέφει ένα απλό έγγραφο TwiML και χρησιμοποιεί το `<Say>` Ρηματικές για να κάνετε ορισμένες μετατροπής κειμένου σε ομιλία και να πείτε "Πίθηκος Hello" για το άτομο που λαμβάνει την κλήση.

## <a id="howto_recieve_sms"></a>Πώς μπορείτε να: παραλαβή μηνύματος SMS
Στο προηγούμενο παράδειγμα θα σας ξεκίνησε μιας **εξερχόμενης** τηλεφωνικής κλήσης. Αυτό ώρα, ας χρησιμοποιήσουμε τον αριθμό τηλεφώνου που Twilio έδωσε μας κατά τη διάρκεια της εγγραφής για την επεξεργασία ενός **εισερχόμενου** μηνύματος SMS.

Πρώτη, συνδεθείτε στην του [πίνακα εργαλείων Twilio][twilio_account]. Κάντε κλικ στην εντολή "Αριθμών" στην επάνω γραμμή περιήγησης και, στη συνέχεια, κάντε κλικ στον αριθμό Twilio που έχουν δώσει. Θα δείτε δύο διευθύνσεις URL που μπορείτε να ρυθμίσετε τις παραμέτρους. Μια διεύθυνση URL αίτηση φωνή και SMS μια αίτηση διεύθυνσης URL. Αυτές είναι οι διευθύνσεις URL που καλεί Twilio κάθε φορά που γίνεται μια τηλεφωνική κλήση ή μια SMS αποστέλλεται τον αριθμό. Οι διευθύνσεις URL είναι επίσης γνωστό ως "άγκιστρα web".

Προτείνεται να επεξεργασία εισερχόμενων μηνυμάτων SMS, οπότε ας ενημέρωση της διεύθυνσης URL για `http://yourdomain.cloudapp.net/sms_url`. Προχωρήστε και κάντε κλικ στην επιλογή Αποθήκευση αλλαγών στο κάτω μέρος της σελίδας. Τώρα, πάλι `web.rb` ας πρόγραμμα μας εφαρμογής για να αντιμετωπίσετε αυτό:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Μετά την πραγματοποίηση της αλλαγής, βεβαιωθείτε ότι για να ξεκινήσετε ξανά την εφαρμογή web. Τώρα, απομάκρυνση από το τηλέφωνό σας και στείλτε μια SMS τον αριθμό Twilio. Θα πρέπει να πάρετε αμέσως μια απόκριση SMS που αναφέρει ότι "Hey, ευχαριστούμε για την εντολή ping! Twilio και Azure Ροκ! ".

## <a id="additional_services"></a>Πώς μπορείτε να: χρήση των υπηρεσιών επιπλέον Twilio
Εκτός από τα παραδείγματα που εμφανίζονται εδώ, Twilio προσφέρει API που βασίζεται στο web που μπορείτε να χρησιμοποιήσετε για να αξιοποιήσετε πρόσθετες λειτουργίες Twilio από την εφαρμογή του Azure. Για πλήρεις λεπτομέρειες, ανατρέξτε στην [τεκμηρίωση Twilio API] [twilio_api_documentation].

### <a id="NextSteps"></a>Επόμενα βήματα
Τώρα που μάθατε τα βασικά στοιχεία της υπηρεσίας Twilio, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα:

* [Οδηγίες ασφαλείας Twilio] [twilio_security_guidelines]
* [Twilio HowTos και παράδειγμα κώδικα] [twilio_howtos]
* [Γρήγορη έναρξη Twilio προγράμματα εκμάθησης][twilio_quickstarts] 
* [Twilio σε GitHub] [twilio_on_github]
* [Επικοινωνήστε με την υποστήριξη Twilio] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
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
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/