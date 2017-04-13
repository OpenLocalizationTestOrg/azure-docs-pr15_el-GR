Χρησιμοποιήστε τη διαδικασία που ταιριάζει με τον τύπο έργου παρασκηνίου&mdash; [παρασκηνίου .NET](#dotnet) ή [Node.js παρασκηνίου](#nodejs).

### <a name="dotnet"></a>Έργο παρασκηνίου .NET

1. Στο Visual Studio, κάντε δεξί κλικ του project server και κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**, αναζητήστε `Microsoft.Azure.NotificationHubs`, στη συνέχεια, κάντε κλικ στην επιλογή **εγκατάσταση**. Αυτό εγκαθιστά στη βιβλιοθήκη του προγράμματος-πελάτη ειδοποίηση διανομείς.

2. Στο φάκελο ελεγκτές, ανοίξτε το TodoItemController.cs και προσθέστε το εξής `using` προτάσεις:

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;

3. Αντικαταστήστε το `PostTodoItem` μέθοδο με τον ακόλουθο κώδικα:  

      
        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send the push notification and log the results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

                // Write the success result to the logs.
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                // Write the failure result to the logs.
                config.Services.GetTraceWriter()
                    .Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

4. Αναδημοσίευση του project server.

### <a name="nodejs"></a>Node.js παρασκηνίου έργου

1. Εάν να έχετε ήδη κάνει, [κάντε λήψη του έργου γρήγορη έναρξη](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) ή να χρησιμοποιήσετε τον [online πρόγραμμα επεξεργασίας στην πύλη του Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
 
1. Αντικαταστήσετε τον υπάρχοντα κωδικό στο αρχείο todoitem.js με τα εξής:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
        
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
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

    Αυτή στέλνει μια ειδοποίηση GCM που περιέχει το item.text όταν προστίθεται ένα νέο στοιχείο todo. 

2. Κατά την επεξεργασία του αρχείου στον τοπικό σας υπολογιστή, δημοσιεύστε ξανά το project server. 
