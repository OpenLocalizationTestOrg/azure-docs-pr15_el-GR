Σε αυτήν την ενότητα, μπορείτε να ενημερώσετε κώδικα στο υπάρχον έργο παρασκηνίου εφαρμογές του Mobile για να στείλετε μια ειδοποίηση push κάθε φορά που προστίθεται ένα νέο στοιχείο. Αυτό είναι υποστηρίζεται από τη δυνατότητα [πρότυπο](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) ειδοποίηση οι διανομείς, ενεργοποίηση ωθεί πλατφόρμες. Τα διάφορα προγράμματα-πελάτες που έχουν καταχωρηθεί για τις ειδοποιήσεις προώθησης με τη χρήση προτύπων και ένα μεμονωμένο καθολικής push να μεταβείτε σε όλες τις πλατφόρμες προγράμματος-πελάτη.

Επιλέξτε την παρακάτω διαδικασία που ταιριάζει με τον τύπο έργου παρασκηνίου&mdash; [παρασκηνίου .NET](#dotnet) ή [Node.js παρασκηνίου](#nodejs).

### <a name="dotnet"></a>Έργο παρασκηνίου .NET
1. Στο Visual Studio, κάντε δεξί κλικ του project server και κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**, αναζητήστε `Microsoft.Azure.NotificationHubs`, στη συνέχεια, κάντε κλικ στην επιλογή **εγκατάσταση**. Αυτό εγκαθιστά στη βιβλιοθήκη διανομείς ειδοποιήσεων για την αποστολή ειδοποιήσεων από τον υπολογιστή στο παρασκήνιο.

3. Στο του project server, ανοίξτε το **ελεγκτές** > **TodoItemController.cs**, και προσθέστε το εξής χρήση προτάσεων:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. Στη μέθοδο **PostTodoItem** , προσθέστε τον ακόλουθο κώδικα μετά την κλήση σε **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Αυτή στέλνει μια ειδοποίηση πρότυπο που περιέχει το στοιχείο. Κείμενο όταν προστίθεται ένα νέο στοιχείο.

4. Αναδημοσίευση του project server. 

### <a name="nodejs"></a>Node.js παρασκηνίου έργου

1. Εάν να έχετε ήδη κάνει, [κάντε λήψη του έργου παρασκηνίου γρήγορη έναρξη](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) ή να χρησιμοποιήσετε τον [online πρόγραμμα επεξεργασίας στην πύλη του Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. Αντικαταστήστε τον υπάρχοντα κωδικό στο todoitem.js με τα εξής:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    Αυτή στέλνει μια ειδοποίηση πρότυπο που περιέχει το item.text όταν προστίθεται ένα νέο στοιχείο.

2. Όταν επεξεργάζεστε το αρχείο στον τοπικό σας υπολογιστή, δημοσιεύστε ξανά το project server.
