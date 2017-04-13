<properties 
    pageTitle="Store-sendgrid-Java-How-to-send-email-example" 
    description="Πώς μπορείτε να στείλετε μήνυμα ηλεκτρονικού ταχυδρομείου με χρήση SendGrid από Java σε μια ανάπτυξη του Azure" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Πώς μπορείτε να στείλετε μήνυμα ηλεκτρονικού ταχυδρομείου με χρήση SendGrid από Java σε μια ανάπτυξη του Azure

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε SendGrid για να στείλετε μηνύματα ηλεκτρονικού ταχυδρομείου από μια ιστοσελίδα που φιλοξενείται στο Azure. Την εφαρμογή που προκύπτει θα γίνεται ερώτηση στο χρήστη για τιμές ηλεκτρονικού ταχυδρομείου, όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης.

![Φόρμα ηλεκτρονικού ταχυδρομείου][emailform]

Το μήνυμα ηλεκτρονικού ταχυδρομείου που προκύπτει θα είναι παρόμοιο με το παρακάτω στιγμιότυπο οθόνης.

![Μήνυμα ηλεκτρονικού ταχυδρομείου][emailsent]

Θα πρέπει να κάνετε τα εξής για να χρησιμοποιήσετε τον κωδικό σε αυτό το θέμα:

1. Αποκτήστε το πλατύστομα javax.mail, για παράδειγμα από <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Προσθέστε το πλατύστομα σας διαδρομή Δόμηση Java.
3. Εάν χρησιμοποιείτε Έκλειψη για να δημιουργήσετε αυτήν την εφαρμογή Java, μπορείτε να συμπεριλάβετε τις βιβλιοθήκες SendGrid στο αρχείο ανάπτυξης εφαρμογής (ΠΟΛΈΜΟΥ) χρησιμοποιώντας τη δυνατότητα συγκρότησης ανάπτυξης του Έκλειψη. Εάν δεν χρησιμοποιείτε Έκλειψη για να δημιουργήσετε αυτήν την εφαρμογή Java, βεβαιωθείτε ότι οι βιβλιοθήκες είναι περιλαμβάνεται εντός του ίδιου Azure ρόλου με την εφαρμογή σας Java και προστεθεί στη διαδρομή κλάσης της εφαρμογής σας.


Μπορείτε, επίσης, πρέπει να έχετε το δικό σας όνομα χρήστη SendGrid και τον κωδικό πρόσβασης, για να μπορέσετε να στείλετε το μήνυμα ηλεκτρονικού ταχυδρομείου. Για να ξεκινήσετε με SendGrid, δείτε [πώς μπορείτε να στείλετε μήνυμα ηλεκτρονικού ταχυδρομείου με χρήση SendGrid από Java](store-sendgrid-java-how-to-send-email.md).

Επιπλέον, εξοικείωση με τις πληροφορίες κατά [τη δημιουργία μιας Hello World εφαρμογής για το Azure στο Έκλειψη](http://msdn.microsoft.com/library/windowsazure/hh690944)ή με άλλες τεχνικές για τη φιλοξενία εφαρμογών Java στο Azure, εάν δεν χρησιμοποιείτε Έκλειψη, συνιστάται ιδιαίτερα.

## <a name="create-a-web-form-for-sending-email"></a>Δημιουργία φόρμας web για την αποστολή ηλεκτρονικού ταχυδρομείου

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να δημιουργήσετε μια φόρμα web για την ανάκτηση δεδομένων χρήστη για την αποστολή ηλεκτρονικού ταχυδρομείου. Για σκοπούς αυτού του περιεχομένου, το αρχείο JSP ονομάζεται **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>Δημιουργία του κώδικα για να στείλετε το μήνυμα ηλεκτρονικού ταχυδρομείου

Ο ακόλουθος κώδικας, που ονομάζεται όταν ολοκληρώσετε τη φόρμα σε emailform.jsp, δημιουργεί το μήνυμα ηλεκτρονικού ταχυδρομείου και αποστέλλει. Για σκοπούς αυτού του περιεχομένου, το αρχείο JSP ονομάζεται **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

Εκτός από την αποστολή του μηνύματος ηλεκτρονικού ταχυδρομείου, emailform.jsp παρέχει ένα αποτέλεσμα για το χρήστη. ένα παράδειγμα είναι το παρακάτω στιγμιότυπο οθόνης:

![Αποστολή αλληλογραφίας αποτέλεσμα][emailresult]

## <a name="next-steps"></a>Επόμενα βήματα

Αναπτύξτε την εφαρμογή με το προσομοίωσης υπολογισμού και ένα πρόγραμμα περιήγησης, εκτελέστε emailform.jsp, εισαγάγετε τιμές στη φόρμα, κάντε κλικ στην επιλογή **στείλετε το μήνυμα ηλεκτρονικού ταχυδρομείου**και, στη συνέχεια, ανατρέξτε στο θέμα έχει ως αποτέλεσμα sendemail.jsp.

Αυτός ο κωδικός παρέχεται για να σας δείξουν πώς μπορείτε να χρησιμοποιήσετε SendGrid σε Java σε Azure. Πριν αναπτύξετε για Azure παραγωγή, ενδέχεται να θέλετε να προσθέσετε περισσότερες χειρισμού σφαλμάτων ή άλλες δυνατότητες. Για παράδειγμα: 

* Μπορείτε να χρησιμοποιήσετε Azure χώρο αποθήκευσης αντικειμένων blob ή βάση δεδομένων SQL για να αποθηκεύσετε τις διευθύνσεις ηλεκτρονικού ταχυδρομείου και μηνυμάτων ηλεκτρονικού ταχυδρομείου, αντί να χρησιμοποιήσετε μια φόρμα web. Για πληροφορίες σχετικά με τη χρήση Azure χώρο αποθήκευσης αντικειμένων blob σε Java, δείτε [πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία αποθήκευσης αντικειμένων Blob του Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Για πληροφορίες σχετικά με τη χρήση της βάσης δεδομένων SQL σε Java, ανατρέξτε στο θέμα [Χρήση βάσης δεδομένων SQL σε Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* Θα μπορούσατε να χρησιμοποιήσετε `RoleEnvironment.getConfigurationSettings` για να ανακτήσετε τα SendGrid όνομα χρήστη και τον κωδικό πρόσβασης από τις ρυθμίσεις παραμέτρων την ανάπτυξη, αντί να χρησιμοποιείτε τη φόρμα web για να ανακτήσετε αυτές τις τιμές. Για πληροφορίες σχετικά με το `RoleEnvironment` κλάση, ανατρέξτε στο θέμα [χρήση της βιβλιοθήκης χρόνου εκτέλεσης υπηρεσίας Azure στο JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) και την τεκμηρίωση πακέτου χρόνος εκτέλεσης υπηρεσίας Azure στο <http://dl.windowsazure.com/javadoc>.
* Για περισσότερες πληροφορίες σχετικά με τη χρήση SendGrid σε Java, δείτε [πώς μπορείτε να στείλετε μήνυμα ηλεκτρονικού ταχυδρομείου με χρήση SendGrid από Java](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
