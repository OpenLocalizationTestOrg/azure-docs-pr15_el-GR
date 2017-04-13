<properties 
    pageTitle="Πώς μπορείτε να κάνετε μια τηλεφωνική κλήση από Twilio (Java) | Microsoft Azure" 
    description="Μάθετε πώς να πραγματοποιήσω μια τηλεφωνική κλήση από μια ιστοσελίδα χρησιμοποιώντας Twilio σε μια εφαρμογή Java σε Azure." 
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

# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Πώς μπορείτε να κάνετε μια τηλεφωνική κλήση με χρήση Twilio σε μια εφαρμογή Java στο Azure 

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Twilio για να πραγματοποιήσετε μια κλήση από μια ιστοσελίδα που φιλοξενείται στο Azure. Την εφαρμογή που προκύπτει θα γίνεται ερώτηση στο χρήστη για τιμές τηλεφωνικής κλήσης, όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης.

![Φόρμα Azure κλήσης με χρήση του Twilio και Java][twilio_java]

Θα πρέπει να κάνετε τα εξής για να χρησιμοποιήσετε τον κωδικό σε αυτό το θέμα:

1. Απόκτηση ένα λογαριασμό Twilio και έλεγχος ταυτότητας διακριτικού. Για να ξεκινήσετε με Twilio, να αξιολογήσετε τις τιμές στο [http://www.twilio.com/pricing][twilio_pricing]. Μπορείτε να εγγραφείτε στο [https://www.twilio.com/try-twilio][try_twilio]. Για πληροφορίες σχετικά με το API που παρέχεται από Twilio, ανατρέξτε στο θέμα [http://www.twilio.com/api][twilio_api].
2. Αποκτήστε το Twilio ΒΆΖΟ. Στο [https://github.com/twilio/twilio-java][twilio_java_github], μπορείτε να λάβετε τις προελεύσεις GitHub και δημιουργήστε τη δική σας ΒΆΖΟ, ή λήψη μιας προ-δομημένες ΒΆΖΟ (με ή χωρίς εξαρτήσεις).
Ο κώδικας σε αυτό το θέμα έχει συνταχθεί με τα προ-δομημένες ΒΆΖΟ TwilioJava 3.3.8 με εξαρτήσεις.
3. Προσθέστε το ΒΆΖΟ σας διαδρομή Δόμηση Java.
4. Εάν χρησιμοποιείτε Έκλειψη για να δημιουργήσετε αυτήν την εφαρμογή Java, συμπεριλάβετε το ΒΆΖΩΝ Twilio στο αρχείο ανάπτυξης εφαρμογής (ΠΟΛΈΜΟΥ) χρησιμοποιώντας τη δυνατότητα συγκρότησης ανάπτυξης του Έκλειψη. Εάν δεν χρησιμοποιείτε Έκλειψη για να δημιουργήσετε αυτήν την εφαρμογή Java, βεβαιωθείτε ότι το Twilio ΒΆΖΩΝ περιλαμβάνεται εντός του ίδιου Azure ρόλου με την εφαρμογή σας Java και προστεθεί στη διαδρομή κλάσης της εφαρμογής σας.
5. Βεβαιωθείτε ότι το keystore cacerts περιέχει το πιστοποιητικό Equifax ασφαλούς αρχή έκδοσης πιστοποιητικών με 67:CB:9 αποτυπώματος MD5 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (ο σειριακός αριθμός που είναι 35:DE:F4:CF και το αποτύπωμα SHA1 D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Αυτό είναι το πιστοποιητικό αρχή έκδοσης πιστοποιητικών (CA) πιστοποιητικό για την [https://api.twilio.com] [ twilio_api_service] υπηρεσίας, η οποία ονομάζεται κατά τη χρήση Twilio APIs. Για πληροφορίες σχετικά με την προσθήκη αυτού του πιστοποιητικού CA σας JDK cacert αποθήκευσης, ανατρέξτε στο θέμα [Προσθήκη ενός πιστοποιητικού στο χώρο αποθήκευσης πιστοποιητικών CA Java][add_ca_cert].

Επιπλέον, εξοικείωση με τις πληροφορίες στη [Δημιουργία Hello World εφαρμογών με χρήση του Κιτ εργαλείων Azure για Έκλειψη][azure_java_eclipse_hello_world], ή με άλλες τεχνικές για τη φιλοξενία εφαρμογών Java στο Azure, εάν δεν χρησιμοποιείτε Έκλειψη, συνιστάται ιδιαίτερα.

## <a name="create-a-web-form-for-making-a-call"></a>Δημιουργία φόρμας web για την πραγματοποίηση κλήσης

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να δημιουργήσετε μια φόρμα web για την ανάκτηση δεδομένων χρήστη για την πραγματοποίηση κλήσης. Για αυτό το παράδειγμα, ένα νέο έργο δυναμικού περιεχομένου web, που ονομάζεται **TwilioCloud**, που δημιουργήθηκε και **callform.jsp** προστέθηκε ως αρχείο JSP.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Δημιουργία του κώδικα για την πραγματοποίηση της κλήσης
Ο ακόλουθος κώδικας, που ονομάζεται όταν ο χρήστης ολοκληρώσει τη φόρμα που εμφανίζεται με callform.jsp, δημιουργεί το μήνυμα κλήσης και την κλήση. Για αυτό το παράδειγμα, το αρχείο JSP ονομάζεται **makecall.jsp** και προστέθηκε στο έργο **TwilioCloud** . (Χρησιμοποιήστε το λογαριασμό Twilio και έλεγχος ταυτότητας διακριτικό αντί για τις τιμές του πλαισίου κράτησης θέσης που έχουν εκχωρηθεί σε **accountSID** και **authToken** τον παρακάτω κώδικα.)

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Εκτός από την πραγματοποίηση της κλήσης, makecall.jsp εμφανίζει το τελικό σημείο Twilio, έκδοση API και την κατάσταση της κλήσης. Ένα παράδειγμα είναι το παρακάτω στιγμιότυπο οθόνης:

![Απάντηση κλήσης Azure χρησιμοποιώντας Twilio και Java][twilio_java_response]

## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή
Παρακάτω δίνονται τα βήματα υψηλού επιπέδου για να εκτελέσετε την εφαρμογή σας; λεπτομέρειες για αυτά τα βήματα μπορείτε να βρείτε στη [Δημιουργία Hello World εφαρμογών με χρήση του Κιτ εργαλείων Azure για Έκλειψη][azure_java_eclipse_hello_world].

1. Εξαγωγή του ΠΟΛΈΜΟΥ TwilioCloud στο φάκελο Azure **approot** . 
2. Τροποποίηση **startup.cmd** αποσυμπίεση του ΠΟΛΈΜΟΥ TwilioCloud.
3. Η μεταγλώττιση της εφαρμογής για το προσομοίωσης υπολογισμού.
4. Ξεκινήστε την ανάπτυξη του προσομοίωσης υπολογισμού.
5. Ανοίξτε ένα πρόγραμμα περιήγησης και εκτελέστε **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Εισαγάγετε τιμές στη φόρμα, κάντε κλικ στην επιλογή **Κάντε αυτήν την κλήση**και, στη συνέχεια, δείτε τα αποτελέσματα σε makecall.jsp.

Όταν είστε έτοιμοι να αναπτύξετε σε Azure, μεταγλωττίστε για ανάπτυξη στο cloud, ανάπτυξη Azure και εκτελέστε http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp στο πρόγραμμα περιήγησης (αντικαταστήστε την τιμή για το *your_hosted_name*).

## <a name="next-steps"></a>Επόμενα βήματα
Αυτός ο κωδικός παρέχεται για να σας δείξουν βασικές λειτουργίες χρησιμοποιώντας Twilio σε Java στο Azure. Πριν αναπτύξετε για Azure παραγωγή, ενδέχεται να θέλετε να προσθέσετε περισσότερες χειρισμού σφαλμάτων ή άλλες δυνατότητες. Για παράδειγμα:

* Αντί να χρησιμοποιήσετε μια φόρμα web, μπορείτε να χρησιμοποιήσετε για την αποθήκευση αριθμών τηλεφώνου και καλέστε κειμένου Azure χώρο αποθήκευσης αντικειμένων blob ή βάση δεδομένων SQL. Για πληροφορίες σχετικά με τη χρήση Azure χώρο αποθήκευσης αντικειμένων blob σε Java, δείτε [πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία αποθήκευσης αντικειμένων Blob του Java][howto_blob_storage_java]. Για πληροφορίες σχετικά με τη χρήση της βάσης δεδομένων SQL σε Java, ανατρέξτε στο θέμα [Χρήση βάσης δεδομένων SQL σε Java][howto_sql_azure_java].
* Μπορείτε να χρησιμοποιήσετε **RoleEnvironment.getConfigurationSettings** για την ανάκτηση του Αναγνωριστικού λογαριασμού Twilio και διακριτικού ελέγχου ταυτότητας από την ανάπτυξη ρυθμίσεις παραμέτρων, αντί για τις τιμές σε makecall.jsp σκληρό κωδικοποίησης. Για πληροφορίες σχετικά με την κλάση **RoleEnvironment** , ανατρέξτε στο θέμα [χρήση της βιβλιοθήκης χρόνου εκτέλεσης υπηρεσίας Azure στο JSP] [ azure_runtime_jsp] και την τεκμηρίωση πακέτου χρόνος εκτέλεσης υπηρεσίας Azure στο [http://dl.windowsazure.com/javadoc][azure_javadoc].
* Ο κώδικας makecall.jsp εκχωρεί δόθηκε Twilio διεύθυνση URL, [http://twimlets.com/message][twimlet_message_url], με τη **διεύθυνση Url** μεταβλητή. Αυτή η διεύθυνση URL παρέχει μια απόκριση Twilio Markup Language (TwiML) που σας πληροφορεί Twilio πώς μπορείτε να συνεχίσετε την κλήση. Για παράδειγμα, το TwiML που επιστρέφεται μπορεί να περιέχει ένα ** &lt;Καλώς&gt; ** Ρηματικές που προκύπτει στο κείμενο που εκφωνείται προς τον παραλήπτη κλήσης. Αντί να χρησιμοποιήσετε τη διεύθυνση URL που παρέχεται από τη Twilio, ενδέχεται να μπορείτε να δημιουργήσετε τη δική σας υπηρεσία να απαντήσετε σε πρόσκληση του Twilio; Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να Twilio Χρήση φωνής και δυνατότητες SMS στο Java][howto_twilio_voice_sms_java]. Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με TwiML στο [http://www.twilio.com/docs/api/twiml][twiml], και περισσότερες πληροφορίες σχετικά με ** &lt;Καλώς&gt; ** και άλλα ρήματα Twilio μπορείτε να βρείτε στο [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Διαβάστε τις οδηγίες ασφαλείας Twilio στο [https://www.twilio.com/docs/security][twilio_docs_security].

Για πρόσθετες πληροφορίες σχετικά με το Twilio, ανατρέξτε στο θέμα [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Δείτε επίσης
* [Πώς να χρησιμοποιείτε Twilio για φωνή και δυνατότητες SMS στο Java][howto_twilio_voice_sms_java]
* [Προσθήκη ενός πιστοποιητικού στο χώρο αποθήκευσης πιστοποιητικών CA Java][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
