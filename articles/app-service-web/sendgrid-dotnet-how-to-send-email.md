<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid (.NET) | Microsoft Azure" 
    description="Μάθετε πώς να στείλετε μήνυμα ηλεκτρονικού ταχυδρομείου με την υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid στην Azure. Κωδικός δείγματα γραμμένη σε C# και χρησιμοποιήστε το API .NET." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/14/2016" 
    ms.author="team-pi@sendgrid.com"/>





# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Πώς μπορείτε να στείλετε μήνυμα ηλεκτρονικού ταχυδρομείου με χρήση SendGrid με Azure


## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός παρουσιάζει πώς μπορείτε να εκτελείτε συνήθεις εργασίες προγραμματισμού με την υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid στην Azure. Τα δείγματα που είναι γραμμένες σε C\#
και χρήση του API .NET. Τα σενάρια καλύπτεται περιλαμβάνουν **η κατασκευή ηλεκτρονικού ταχυδρομείου**, **τα μηνύματα ηλεκτρονικού ταχυδρομείου**, **Προσθήκη συνημμένων**και **χρήση των φίλτρων**. Για περισσότερες πληροφορίες σχετικά με SendGrid και αποστολή ηλεκτρονικού ταχυδρομείου, ανατρέξτε στην ενότητα [επόμενα βήματα][] .

## <a name="what-is-the-sendgrid-email-service"></a>Τι είναι η υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid;

SendGrid είναι μια [υπηρεσία ηλεκτρονικού ταχυδρομείου που βασίζεται στο cloud] που παρέχει αξιόπιστη [παράδοση μηνυμάτων ηλεκτρονικού ταχυδρομείου συναλλαγών], κλιμάκωση και σε πραγματικό χρόνο ανάλυση μαζί με ευέλικτες API που διευκολύνουν την προσαρμοσμένη ενοποίησης. Κοινά σενάρια χρήσης SendGrid περιλαμβάνουν τα εξής:

-   Αυτόματη αποστολή αποδεικτικών στους πελάτες.
-   Διαχείριση διανομής λίστες για την αποστολή τους πελάτες μηνιαία e-fliers και ειδικές προσφορές.
-   Συλλογή σε πραγματικό χρόνο μετρήσεις για πράγματα όπως τα αποκλεισμένα μηνύματα ηλεκτρονικού ταχυδρομείου, και την απόκριση πελατών.
-   Δημιουργία αναφορών για να προσδιορίσουν τις τάσεις.
-   Προώθηση ερωτήσεις του πελάτη.
-   Επεξεργασία των εισερχόμενων μηνυμάτων ηλεκτρονικού ταχυδρομείου.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [https://sendgrid.com](https://sendgrid.com) ή τη [βιβλιοθήκη C#][sendgrid csharp]

## <a name="create-a-sendgrid-account"></a>Δημιουργία λογαριασμού SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Αναφορά στη βιβλιοθήκη κλάσης SendGrid .NET

Το [πακέτο SendGrid NuGet](https://www.nuget.org/packages/Sendgrid) είναι ο ευκολότερος τρόπος για να λάβετε το API SendGrid και να ρυθμίσετε τις παραμέτρους της εφαρμογής σας με όλες τις εξαρτήσεις. NuGet είναι μια επέκταση Visual Studio που περιλαμβάνεται στο Microsoft Visual Studio 2015 που διευκολύνει την εγκατάσταση και ενημέρωση εργαλεία και βιβλιοθήκες. 

> [AZURE.NOTE] Για να εγκαταστήσετε NuGet εάν εκτελείτε μια έκδοση του Visual Studio παλαιότερη από το Visual Studio 2015, επισκεφθείτε [http://www.nuget.org](http://www.nuget.org)και κάντε κλικ στο κουμπί **Εγκατάσταση NuGet** .

Για να εγκαταστήσετε το πακέτο SendGrid NuGet στην εφαρμογή σας, κάντε τα εξής:

1.  Δημιουργία νέου έργου.

    ![Δημιουργία νέου έργου][create-new-project]

2.  Επιλέξτε ένα πρότυπο.

    ![Επιλέξτε ένα πρότυπο][select-a-template]

3.  Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ σε **αναφορές**και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.

4.  Αναζήτηση **SendGrid** και επιλέξτε το στοιχείο **SendGrid** στη λίστα των αποτελεσμάτων.

    ![Το πακέτο SendGrid NuGet][SendGrid-NuGet-package]

5.  Κάντε κλικ στην επιλογή **εγκατάσταση** για να ολοκληρώσετε την εγκατάσταση και, στη συνέχεια, κλείστε το παράθυρο διαλόγου.

Βιβλιοθήκη κλάσεων .NET του SendGrid ονομάζεται **SendGridMail**. Περιέχει τα παρακάτω πεδία ονομάτων:

-   **SendGridMail** για τη δημιουργία και εργασία με τα στοιχεία ηλεκτρονικού ταχυδρομείου.
-   **SendGridMail.Transport** για την αποστολή ηλεκτρονικού ταχυδρομείου με χρήση του πρωτοκόλλου **SMTP** ή πρωτοκόλλου HTTP 1.1 με **Web/ΥΠΌΛΟΙΠΑ**.

Προσθέστε τις ακόλουθες δηλώσεις χώρος ονομάτων κώδικα στο επάνω μέρος κάθε C\# αρχείο στο οποίο θέλετε να μέσω προγραμματισμού πρόσβαση στην υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid.
**System.Net** και **System.Net.Mail** είναι οι χώροι ονομάτων .NET Framework που περιλαμβάνονται, επειδή αυτές περιλαμβάνονται οι τύποι που χρησιμοποιείτε συχνά με το API SendGrid.

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## <a name="how-to-create-an-email"></a>Πώς μπορείτε να: Δημιουργία μηνύματος ηλεκτρονικού ταχυδρομείου

Χρησιμοποιήστε το αντικείμενο **SendGridMessage** για να δημιουργήσετε ένα μήνυμα ηλεκτρονικού ταχυδρομείου. Αφού δημιουργηθεί το αντικείμενο μηνύματος, μπορείτε να ορίσετε ιδιότητες και τις μεθόδους, όπως τον αποστολέα ηλεκτρονικού ταχυδρομείου, ο παραλήπτης ηλεκτρονικού ταχυδρομείου, το αντικείμενο και το σώμα του μηνύματος ηλεκτρονικού ταχυδρομείου.

Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να δημιουργήσετε ένα αντικείμενο πλήρως συμπληρωμένα ηλεκτρονικού ταχυδρομείου:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

Για περισσότερες πληροφορίες σχετικά με όλες τις ιδιότητες και τις μεθόδους που υποστηρίζονται από τον τύπο **SendGrid** , ανατρέξτε στο θέμα [sendgrid csharp][] GitHub.

## <a name="how-to-send-an-email"></a>Πώς μπορείτε να: Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου

Αφού δημιουργήσετε ένα μήνυμα ηλεκτρονικού ταχυδρομείου, μπορείτε να το στείλετε με χρήση του API Web που παρέχεται από SendGrid. Εναλλακτικά, μπορείτε να [χρήση. Ενσωματωμένη του Καθαρής στη βιβλιοθήκη](https://sendgrid.com/docs/Code_Examples/csharp.html).

Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου απαιτεί που δίνετε το πλήκτρο API SendGrid ή διαπιστευτήρια λογαριασμού SendGrid (όνομα χρήστη και κωδικός πρόσβασης). Πλήκτρο API είναι η προτιμώμενη μέθοδος. Εάν χρειάζεστε λεπτομέρειες σχετικά με το πώς μπορείτε να ρυθμίσετε τις παραμέτρους API κλειδιά, επισκεφθείτε την [τεκμηρίωση](https://sendgrid.com/docs/Classroom/Send/api_keys.html)

Ενδέχεται να μπορείτε να αποθηκεύσετε αυτά τα διαπιστευτήρια μέσω της πύλης Azure, κάνοντας κλικ στην επιλογή ρύθμιση ΠΑΡΑΜΈΤΡΩΝ και προσθέτοντας τα ζεύγη κλειδιού/τιμής στην περιοχή "Ρυθμίσεις εφαρμογής".

 ![Ρυθμίσεις εφαρμογής Azure][azure_app_settings]

 Στη συνέχεια, που μπορεί να αποκτήσετε πρόσβαση σε αυτά ως εξής: 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USERNAME"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASSWORD");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

Χρησιμοποιώντας τα διαπιστευτήρια:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

Χρήση του API του αριθμού-κλειδιού:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να στείλετε ένα μήνυμα με χρήση του API Web.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## <a name="how-to-add-an-attachment"></a>Πώς μπορείτε να: Προσθήκη συνημμένου

Μπορείτε να προσθέσετε συνημμένα σε ένα μήνυμα, κλήση της μεθόδου **AddAttachment** και να καθορίσετε το όνομα και τη διαδρομή του αρχείου που θέλετε να επισυνάψετε.
Μπορείτε να συμπεριλάβετε πολλά συνημμένα, καλώντας αυτήν τη μέθοδο όταν για κάθε αρχείο που θέλετε να επισυνάψετε. Το παρακάτω παράδειγμα παρουσιάζει Προσθήκη συνημμένου σε ένα μήνυμα:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
Μπορείτε επίσης να προσθέσετε συνημμένα από τα δεδομένα **ροής**. Μπορεί να γίνει καλώντας την ίδια μέθοδο ως παραπάνω, **AddAttachment**, αλλά περνώντας στη ροή των δεδομένων και το όνομα του αρχείου που θέλετε να εμφανίζονται όπως το μήνυμα. Σε αυτήν την περίπτωση θα πρέπει να προσθέσετε στη βιβλιοθήκη System.IO.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## <a name="how-to-use-apps-to-enable-footers-tracking-and-analytics"></a>Πώς μπορείτε να: χρήση εφαρμογών για να ενεργοποιήσετε την υποσέλιδα, παρακολούθηση και ανάλυση

SendGrid παρέχει λειτουργικότητα επιπλέον ηλεκτρονικού ταχυδρομείου με τη χρήση εφαρμογών. Αυτές είναι οι ρυθμίσεις που μπορούν να προστεθούν σε ένα μήνυμα ηλεκτρονικού ταχυδρομείου για να ενεργοποιήσετε συγκεκριμένες λειτουργίες όπως κάντε κλικ στην επιλογή παρακολούθηση, Google analytics, παρακολούθηση, τη συνδρομή και ούτω καθεξής. Για μια πλήρη λίστα των εφαρμογών, ανατρέξτε στο θέμα [Των ρυθμίσεων της εφαρμογής][].

Οι εφαρμογές μπορούν να εφαρμοστούν σε μηνύματα ηλεκτρονικού ταχυδρομείου **SendGrid** χρησιμοποιώντας μεθόδους που εφαρμόζονται ως μέρος της κλάσης **SendGrid** .

Τα παρακάτω παραδείγματα δείχνουν το υποσέλιδο και κάντε κλικ στην επιλογή Παρακολούθηση φίλτρα:

### <a name="footer"></a>Υποσέλιδο

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### <a name="click-tracking"></a>Κάντε κλικ στην επιλογή Παρακολούθηση

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Πώς μπορείτε να: χρήση πρόσθετων SendGrid υπηρεσιών

SendGrid προσφέρει API και webhooks που μπορείτε να χρησιμοποιήσετε για να αξιοποιήσετε πρόσθετες λειτουργίες SendGrid από την εφαρμογή του Azure βασίζεται στο web. Για πλήρεις λεπτομέρειες, ανατρέξτε στην [τεκμηρίωση SendGrid API][].

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία για την υπηρεσία ηλεκτρονικού ταχυδρομείου SendGrid, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα.

*   SendGrid C\# repo βιβλιοθήκη: [sendgrid csharp][]
*   Τεκμηρίωση SendGrid API: <https://sendgrid.com/docs>
*   SendGrid ειδική προσφορά για τους πελάτες του Azure: [https://sendgrid.com](https://sendgrid.com)

  [Επόμενα βήματα]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [sendgrid csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [Ρυθμίσεις εφαρμογής]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [Τεκμηρίωση SendGrid API]: https://sendgrid.com/docs
  
  [υπηρεσία ηλεκτρονικού ταχυδρομείου που βασίζεται στο cloud]: https://sendgrid.com/email-solutions
  [παράδοση συναλλαγών μηνυμάτων ηλεκτρονικού ταχυδρομείου]: https://sendgrid.com/transactional-email
 
