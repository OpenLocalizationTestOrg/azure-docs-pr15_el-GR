<properties 
    pageTitle="Πώς να χρησιμοποιείτε Twilio για φωνή και το SMS (Java) | Microsoft Azure" 
    description="Μάθετε πώς να πραγματοποιήσω μια τηλεφωνική κλήση και να στείλετε ένα μήνυμα SMS με την υπηρεσία Twilio API σε Azure. Δείγματα κώδικα γραμμένο σε Java." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Πώς να χρησιμοποιείτε Twilio για φωνή και δυνατότητες SMS στο Java

Αυτός ο οδηγός παρουσιάζει πώς μπορείτε να εκτελείτε συνήθεις εργασίες προγραμματισμού με την υπηρεσία Twilio API σε Azure. Τα σενάρια καλύπτεται περιλαμβάνουν μια τηλεφωνική κλήση και να στείλετε ένα μήνυμα σύντομο μήνυμα υπηρεσίας (SMS). Για περισσότερες πληροφορίες σχετικά με την Twilio και τη χρήση φωνής και SMS στις εφαρμογές σας, ανατρέξτε στην ενότητα [Επόμενα βήματα](#NextSteps) .

## <a id="WhatIs"></a>Τι είναι το Twilio;
Twilio είναι ένα API της υπηρεσίας web τηλεφωνίας που σας επιτρέπει να χρησιμοποιείτε το υπάρχον web γλώσσες και δεξιότητες για να δημιουργήσει η φωνή και το SMS εφαρμογές. Twilio είναι μια υπηρεσία τρίτων κατασκευαστών (δεν δυνατότητα του Azure και όχι ένα προϊόν της Microsoft).

**Φωνητικό Twilio** επιτρέπει τις εφαρμογές σας για να κάνετε και να λαμβάνετε τηλεφωνικές κλήσεις. **Twilio SMS** επιτρέπει τις εφαρμογές σας για να κάνετε και να λαμβάνετε μηνύματα SMS. **Πρόγραμμα-πελάτης Twilio** επιτρέπει τις εφαρμογές σας για να ενεργοποιήσετε την φωνητικής επικοινωνίας με υπάρχουσες συνδέσεις στο Internet, συμπεριλαμβανομένων των συνδέσεις κινητού.

## <a id="Pricing"></a>Τις τιμές Twilio και ειδικές προσφορές
Πληροφορίες σχετικά με τις τιμές Twilio είναι διαθέσιμη από [Τις τιμές Twilio] [twilio_pricing]. Οι πελάτες Azure λαμβάνουν μια [ειδική προσφορά][special_offer]: δωρεάν πιστοληπτικής ικανότητας 1000 κείμενα ή 1000 εισερχομένων λεπτά. Για να εγγραφείτε για αυτήν την προσφορά ή να λάβετε περισσότερες πληροφορίες, επισκεφθείτε την τοποθεσία [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Έννοιες
Το API Twilio είναι RESTful API που παρέχει φωνή και λειτουργικότητα SMS για εφαρμογές. Οι βιβλιοθήκες προγράμματος-πελάτη είναι διαθέσιμες σε πολλές γλώσσες. για μια λίστα, ανατρέξτε στο θέμα [Βιβλιοθήκες API Twilio] [twilio_libraries].

Πλήκτρο στοιχεία του Twilio API είναι ρήματα Twilio και Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Ρήματα Twilio
Το API χρησιμοποιεί Twilio ρήματα; Για παράδειγμα, το ** &lt;Καλώς&gt; ** Ρηματικές καθοδηγεί Twilio ευκρίνεια παράδοσης ενός μηνύματος σε κλήση. 

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

Όταν εγγραφείτε για ένα λογαριασμό Twilio, θα λάβετε ένα Αναγνωριστικό λογαριασμού και ενός διακριτικού ελέγχου ταυτότητας. Και τα δύο θα χρειαστούν για την πραγματοποίηση κλήσεων Twilio API. Για να αποτρέψετε τη μη εξουσιοδοτημένη πρόσβαση στο λογαριασμό σας, αφήστε το διακριτικό ελέγχου ταυτότητας ασφαλή. Το Αναγνωριστικό λογαριασμού και έλεγχος ταυτότητας διακριτικού είναι δυνατό να προβληθούν στη [σελίδα του λογαριασμού Twilio] [twilio_account], στο τα πεδία με την ετικέτα **αναγνωριστικό ΑΣΦΑΛΕΊΑΣ ΛΟΓΑΡΙΑΣΜΟΎ** και **ΔΙΑΚΡΙΤΙΚΟΎ AUTH**, αντίστοιχα.

## <a id="create_app"></a>Δημιουργία μιας εφαρμογής Java
1. Αποκτήστε το ΒΆΖΩΝ Twilio και προσθήκη της σε σας διαδρομή Δόμηση Java και ανάπτυξη της ΠΟΛΈΜΟΥ συγκρότησης. Στο [https://github.com/twilio/twilio-java][twilio_java], μπορείτε να λάβετε τις προελεύσεις GitHub και δημιουργήστε τη δική σας ΒΆΖΟ, ή λήψη μιας προ-δομημένες ΒΆΖΟ (με ή χωρίς εξαρτήσεις).
2. Βεβαιωθείτε ότι το JDK **cacerts** keystore περιέχει το πιστοποιητικό Equifax ασφαλούς αρχή έκδοσης πιστοποιητικών με 67:CB:9 αποτυπώματος MD5 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (ο σειριακός αριθμός που είναι 35:DE:F4:CF και το αποτύπωμα SHA1 D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Αυτό είναι το πιστοποιητικό αρχή έκδοσης πιστοποιητικών (CA) πιστοποιητικό για την [https://api.twilio.com] [ twilio_api_service] υπηρεσίας, η οποία ονομάζεται κατά τη χρήση Twilio APIs. Για πληροφορίες σχετικά με τη διασφάλιση ότι το JDK **cacerts** keystore περιέχει το σωστό πιστοποιητικό CA, ανατρέξτε στο θέμα [Προσθήκη ενός πιστοποιητικού στο χώρο αποθήκευσης πιστοποιητικών CA Java][add_ca_cert].

Λεπτομερείς οδηγίες για τη χρήση της βιβλιοθήκης του προγράμματος-πελάτη Twilio για Java είναι διαθέσιμες στη [πώς μπορείτε να κάνετε μια τηλεφωνική κλήση Twilio χρησιμοποιώντας σε μια εφαρμογή Java σε Azure][howto_phonecall_java].

## <a id="configure_app"></a>Ρύθμιση παραμέτρων της εφαρμογής σας για να χρησιμοποιήσετε βιβλιοθήκες Twilio
Στον κωδικό σας, μπορείτε να προσθέσετε **Εισαγωγή** δηλώσεις στο επάνω μέρος αρχεία προέλευσης για τα πακέτα Twilio ή των κατηγοριών που θέλετε να χρησιμοποιήσετε στην εφαρμογή σας. 

Για τα αρχεία προέλευσης Java:

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

Για τα αρχεία προέλευσης σελίδας διακομιστή Java (JSP):

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
Ανάλογα με το ποια πακέτα Twilio ή των κατηγοριών που θέλετε να χρησιμοποιήσετε, τις προτάσεις **Εισαγωγή** ενδέχεται να είναι διαφορετικές.

## <a id="howto_make_call"></a>Πώς μπορείτε να: πραγματοποίηση μιας εξερχόμενης κλήσης
Η ακόλουθη εικόνα δείχνει τον τρόπο δημιουργίας μιας εξερχόμενης κλήσης με χρήση του την κλάση **CallFactory** . Αυτός ο κωδικός χρησιμοποιεί επίσης μια τοποθεσία που παρέχεται από τη Twilio για να επιστρέψει την απάντηση Twilio Markup Language (TwiML). Αντικαταστήστε τις τιμές για τους αριθμούς τηλεφώνου **από** και **έως** και βεβαιωθείτε ότι μπορείτε να επαληθεύσετε τον αριθμό τηλεφώνου **από το** για το λογαριασμό σας Twilio πριν από την εκτέλεση του κώδικα.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

Για περισσότερες πληροφορίες σχετικά με τις παραμέτρους που του μεταβιβάστηκε στο τη μέθοδο **CallFactory.create** , ανατρέξτε στο θέμα [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Όπως αναφέρθηκε, αυτός ο κωδικός χρησιμοποιεί μια τοποθεσία που παρέχεται από τη Twilio για να επιστρέψει την απάντηση TwiML. Αντί για αυτό, μπορείτε να χρησιμοποιήσετε τη δική σας τοποθεσία για την παροχή της απόκρισης TwiML; Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να παρέχουν TwiML απαντήσεις σε μια εφαρμογή Java σε Azure](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Πώς μπορείτε να: Αποστολή μηνύματος SMS
Η ακόλουθη εικόνα δείχνει πώς μπορείτε να στείλετε ένα μήνυμα SMS χρησιμοποιώντας την κλάση **SmsFactory** . **Από** αριθμό, **4155992671**, παρέχεται από Twilio για χρήση δοκιμαστικής έκδοσης λογαριασμούς για την αποστολή μηνυμάτων SMS. Ο αριθμός **να** πρέπει να έχει επαληθευτεί για το λογαριασμό σας Twilio πριν από την εκτέλεση του κώδικα.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
Για περισσότερες πληροφορίες σχετικά με τις παραμέτρους που του μεταβιβάστηκε στο τη μέθοδο **SmsFactory.create** , ανατρέξτε στο θέμα [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Πώς μπορείτε να: παρέχει TwiML απαντήσεις από τη δική σας τοποθεσία Web
Όταν η εφαρμογή σας ξεκινά μια κλήση για το API Twilio, για παράδειγμα μέσω της μεθόδου **CallFactory.create** , Twilio θα στείλει την πρόσκληση σε μια διεύθυνση URL που αναμένεται να επιστρέψει μια απόκριση TwiML. Το παραπάνω παράδειγμα χρησιμοποιεί τη διεύθυνση URL που παρέχεται από τη Twilio [http://twimlets.com/message][twimlet_message_url]. (Ενώ TwiML έχει σχεδιαστεί για χρήση από τις υπηρεσίες Web, μπορείτε να προβάλετε το TwiML στο πρόγραμμα περιήγησης. Για παράδειγμα, κάντε κλικ στην επιλογή [http://twimlets.com/message] [ twimlet_message_url] για να δείτε μια κενή ** &lt;απόκριση&gt; ** στοιχείο. ένα άλλο παράδειγμα, κάντε κλικ στην επιλογή [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] για να δείτε μια ** &lt;απόκριση&gt; ** στοιχείο που περιέχει ένα ** &lt;Καλώς&gt; ** στοιχείου.)

Αντί να βασίζεστε στη διεύθυνση URL που παρέχεται από τη Twilio, μπορείτε να δημιουργήσετε τη δική σας διεύθυνση URL τοποθεσίας που επιστρέφει αποκρίσεις HTTP. Μπορείτε να δημιουργήσετε την τοποθεσία σε οποιαδήποτε γλώσσα που επιστρέφει αποκρίσεις HTTP; Αυτό το θέμα προϋποθέτει θα φιλοξενίας τη διεύθυνση URL σε μια σελίδα JSP.

Στην επόμενη σελίδα JSP ως αποτέλεσμα μια απάντηση TwiML που αναφέρει ότι **Γεια** στην κλήση.

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

Στην επόμενη σελίδα JSP ως αποτέλεσμα μια απόκριση TwiML που αναφέρει ότι κάποιο κείμενο και έχει πολλές παύσεις λέει πληροφοριών σχετικά με την έκδοση Twilio API και το όνομα του Azure ρόλου.


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

Η παράμετρος **ApiVersion** είναι διαθέσιμο σε προσκλήσεις φωνής Twilio (δεν αιτήσεων SMS). Για να δείτε τις παραμέτρους διαθέσιμη αίτηση για Twilio φωνή και προσκλήσεις SMS, ανατρέξτε στο θέμα <https://www.twilio.com/docs/api/twiml/twilio_request> και <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, αντίστοιχα. Η μεταβλητή περιβάλλοντος **όνομα ρόλου** είναι διαθέσιμη ως μέρος μιας Azure ανάπτυξης. (Εάν θέλετε να προσθέσετε προσαρμοσμένο περιβάλλον μεταβλητές, ώστε να μπορεί να επιλέξατε προς τα επάνω από το **System.getenv**, ανατρέξτε στην ενότητα μεταβλητές περιβάλλοντος σε [Διάφορες ρυθμίσεις παραμέτρων ρόλο][misc_role_config_settings].)

Όταν έχετε JSP σελίδα σας ρυθμιστεί ώστε να παρέχουν απαντήσεις TwiML, χρησιμοποιήστε τη διεύθυνση URL της σελίδας JSP ως τη διεύθυνση URL που εισήχθησαν σε τη μέθοδο **CallFactory.create** . Για παράδειγμα, εάν έχετε μια εφαρμογή Web με το όνομα MyTwiML αναπτυχθεί σε μια Azure φιλοξενούνται υπηρεσία, και το όνομα της σελίδας JSP είναι mytwiml.jsp, τη διεύθυνση URL μπορούν να περάσουν **CallFactory.create** όπως φαίνεται παρακάτω:

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

Μια άλλη επιλογή για να ανταποκρίνεται με TwiML είναι μέσω την κλάση **TwiMLResponse** , το οποίο είναι διαθέσιμο στο πακέτο **com.twilio.sdk.verbs** .

Για πρόσθετες πληροφορίες σχετικά με τη χρήση Twilio Azure με Java, δείτε [πώς μπορείτε να κάνετε μια τηλεφωνική κλήση Twilio χρησιμοποιώντας σε μια εφαρμογή Java σε Azure][howto_phonecall_java].

## <a id="AdditionalServices"></a>Πώς μπορείτε να: χρήση των υπηρεσιών επιπλέον Twilio
Εκτός από τα παραδείγματα που εμφανίζονται εδώ, Twilio προσφέρει API που βασίζεται στο web που μπορείτε να χρησιμοποιήσετε για να αξιοποιήσετε πρόσθετες λειτουργίες Twilio από την εφαρμογή του Azure. Για πλήρεις λεπτομέρειες, ανατρέξτε στην [τεκμηρίωση Twilio API] [twilio_api_documentation].

## <a id="NextSteps"></a>Επόμενα βήματα
Τώρα που μάθατε τα βασικά στοιχεία της υπηρεσίας Twilio, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα:

* [Οδηγίες ασφαλείας Twilio] [twilio_security_guidelines]
* [Του Twilio ΔΙΑΔΙΚΑΣΙΕΣ και παράδειγμα κώδικα] [twilio_howtos]
* [Γρήγορη έναρξη Twilio προγράμματα εκμάθησης][twilio_quickstarts] 
* [Twilio σε GitHub] [twilio_on_github]
* [Επικοινωνήστε με την υποστήριξη Twilio] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
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
