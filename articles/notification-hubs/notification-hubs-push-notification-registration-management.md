<properties
    pageTitle="Διαχείριση εγγραφής"
    description="Αυτό το θέμα εξηγεί τον τρόπο για να καταχωρήσετε τις συσκευές με ειδοποίηση διανομείς προκειμένου να λαμβάνουν ειδοποιήσεις προώθησης."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="registration-management"></a>Διαχείριση εγγραφής

##<a name="overview"></a>Επισκόπηση

Αυτό το θέμα εξηγεί τον τρόπο για να καταχωρήσετε τις συσκευές με ειδοποίηση διανομείς προκειμένου να λαμβάνουν ειδοποιήσεις προώθησης. Το θέμα περιγράφει καταχωρήσεων σε υψηλό επίπεδο, στη συνέχεια, παρουσιάζει τα δύο κύρια μοτίβα για την εγγραφή σας συσκευές: την καταχώρηση από τη συσκευή απευθείας με την ενότητα "Ειδοποίηση" και την εγγραφή σας μέσω μιας εφαρμογής παρασκηνίου. 


##<a name="what-is-device-registration"></a>Τι είναι η καταχώρηση της συσκευής

Η καταχώρηση της συσκευής με ένα διανομέα ειδοποίηση πραγματοποιείται χρησιμοποιώντας μια **καταχώρηση** ή με την **εγκατάσταση**.

#### <a name="registrations"></a>Καταχωρήσεων
Μια καταχώρηση συσχετίζει τη λαβή υπηρεσία ειδοποιήσεων πλατφόρμα (Γραμματίων) για μια συσκευή με ετικέτες και πιθανώς ένα πρότυπο. Τη λαβή Γραμματίων θα μπορούσε να είναι μια ChannelURI, διακριτικό συσκευή ή αναγνωριστικό δήλωσης GCM. Οι ετικέτες που χρησιμοποιούνται για τη δρομολόγηση ειδοποιήσεις για να το σωστό σύνολο λαβές συσκευή. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δρομολόγηση και παραστάσεις ετικέτα](notification-hubs-tags-segment-push-message.md). Τα πρότυπα που χρησιμοποιούνται για την υλοποίηση μετασχηματισμό ανά καταχώρηση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πρότυπα](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Εγκαταστάσεις
Μια εγκατάσταση είναι ένα βελτιωμένο ιδιότητες σχετικής καταχώρηση που περιλαμβάνει μια τσάντα της push. Είναι η πιο πρόσφατες και καλύτερη προσέγγιση για την εγγραφή για τις συσκευές σας. Ωστόσο, δεν υποστηρίζεται από πελάτη .NET SDK ([SDK διανομέα ειδοποίησης για λειτουργίες παρασκηνίου](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) από τον ακόμη.  Αυτό σημαίνει ότι εάν την καταχώρηση από το πρόγραμμα-πελάτη την ίδια τη συσκευή, θα πρέπει να χρησιμοποιήσετε την προσέγγιση [Ειδοποίηση διανομείς REST API](https://msdn.microsoft.com/library/mt621153.aspx) για την υποστήριξη εγκαταστάσεις. Εάν χρησιμοποιείτε μια υπηρεσία υποστήριξης, θα πρέπει να μπορούν να χρησιμοποιήσουν [SDK διανομέα ειδοποίησης για τις εργασίες παρασκηνίου](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Ακολουθούν ορισμένα βασικά πλεονεκτήματα χρήσης εγκαταστάσεις:

* Δημιουργία ή ενημέρωση μιας εγκατάστασης είναι πλήρως idempotent. Επομένως, μπορείτε να εκτελέσετε ξανά το χωρίς οποιαδήποτε ανησυχίες σχετικά με τις διπλότυπες καταχωρήσεις.
* Το μοντέλο εγκατάστασης διευκολύνει να κάνετε μεμονωμένες ωθεί - στόχευσης συγκεκριμένη συσκευή. Μια ετικέτα συστήματος **"$InstallationId: [installationId]"** προστίθεται αυτόματα με κάθε καταχώρηση εγκατάστασης με βάση. Επομένως, μπορείτε να καλέσετε μια αποστολή σε αυτήν την ετικέτα για να στοχεύουν σε μια συγκεκριμένη συσκευή χωρίς να χρειάζεται να κάνετε οποιαδήποτε επιπλέον κωδικοποίησης.
* Χρήση εγκαταστάσεις σας επιτρέπει επίσης να κάνετε μερική registration ενημερώνεται. Η μερική ενημέρωση μιας εγκατάστασης ζητήθηκε με μια μέθοδο ενημέρωσης ΚΏΔΙΚΑ, χρησιμοποιώντας την [Τυπική JSON κώδικα](https://tools.ietf.org/html/rfc6902). Αυτό είναι ιδιαίτερα χρήσιμη όταν θέλετε να ενημερώσετε ετικέτες για την καταχώρηση. Δεν χρειάζεται να αναπτυσσόμενο ολόκληρη την καταχώρηση και, στη συνέχεια, στείλτε ξανά όλες τις προηγούμενες ετικέτες.

Μια εγκατάσταση μπορεί να περιέχει το τις ακόλουθες ιδιότητες. Για μια ολοκληρωμένη λίστα με την εγκατάσταση ιδιότητες ανατρέξτε στο θέμα, [Δημιουργία ή αντικατάσταση μιας εγκατάστασης με REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) ή [Ιδιότητες εγκατάστασης](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) για το.

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }

 

Είναι σημαντικό να λάβετε υπόψη ότι δεν είναι πλέον λήξει καταχωρήσεων και εγκαταστάσεων από προεπιλογή.

Καταχωρήσεις και εγκαταστάσεων πρέπει να περιέχει μια έγκυρη λαβή Γραμματίων για κάθε συσκευή/κανάλι. Επειδή το λαβές Γραμματίων μπορείτε να αποκτήσετε μόνο σε μια εφαρμογή προγράμματος-πελάτη από τη συσκευή, είναι ένα μοτίβο για να καταχωρήσετε απευθείας σε αυτήν τη συσκευή με την εφαρμογή υπολογιστή-πελάτη. Από την άλλη πλευρά, ζητήματα ασφαλείας και επιχειρηματικής λογικής που σχετίζονται με ετικέτες ενδέχεται να απαιτεί να διαχειριστείτε καταχώρηση της συσκευής στο παρασκηνίου εφαρμογής. 

#### <a name="templates"></a>Πρότυπα

Εάν θέλετε να χρησιμοποιήσετε [πρότυπα](notification-hubs-templates-cross-platform-push-messages.md), η εγκατάσταση της συσκευής κρατήστε επίσης όλα τα πρότυπα που σχετίζεται με αυτήν τη συσκευή σε μια JSON μορφοποίηση (βλέπε παραπάνω δείγμα). Τα ονόματα πρότυπο βοηθούν προορισμού διαφορετικά πρότυπα για την ίδια συσκευή.

Σημειώστε ότι κάθε όνομα προτύπου χάρτες με ένα πρότυπο σώμα και ένα προαιρετικό σύνολο ετικετών. Επιπλέον, κάθε πλατφόρμα μπορεί να έχει το πρότυπο πρόσθετες ιδιότητες. Για το Windows Store (χρησιμοποιώντας WNS) και το Windows Phone 8 (χρησιμοποιώντας MPNS), ένα πρόσθετο σύνολο κεφαλίδων μπορεί να είναι μέρος του προτύπου. Στην περίπτωση APNs, μπορείτε να ορίσετε μια ιδιότητα λήξη μια σταθερά ή μια παράσταση πρότυπο. Για μια ολοκληρωμένη λίστα με την εγκατάσταση ιδιότητες ανατρέξτε στο θέμα [Δημιουργία ή αντικατάσταση μιας εγκατάστασης με το ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt621153.aspx) το θέμα.

#### <a name="secondary-tiles-for-windows-store-apps"></a>Δευτερεύουσα πλακίδια για τις εφαρμογές του Windows Store

Για εφαρμογές προγράμματος-πελάτη του Windows Store, η αποστολή ειδοποιήσεων σε δευτερεύοντα τα πλακίδια είναι ίδια με την αποστολή τους σε κύρια. Υποστηρίζεται επίσης στις εγκαταστάσεις. Σημειώστε ότι δευτερεύοντα πλακίδια έχουν ένα διαφορετικό ChannelUri, όπου το SDK στην εφαρμογή υπολογιστή-πελάτη σας χειρίζεται με διαφάνεια.

Το λεξικό SecondaryTiles χρησιμοποιεί το ίδιο TileId που χρησιμοποιείται για να δημιουργήσετε το αντικείμενο SecondaryTiles στην εφαρμογή Windows Store.
Όπως συμβαίνει με τα πρωτεύοντα ChannelUri, να αλλάξετε ChannelUris των πλακιδίων δευτερεύοντα ανά πάσα στιγμή. Για να διατηρήσετε τις εγκαταστάσεις στην ενότητα "Ειδοποίηση" ενημέρωση, η συσκευή πρέπει να ανανεώσετε τους με το τρέχον ChannelUris από τα δευτερεύοντα πλακίδια.


##<a name="registration-management-from-the-device"></a>Διαχείριση καταχώρηση από τη συσκευή

Κατά τη Διαχείριση καταχώρηση της συσκευής από τις εφαρμογές προγράμματος-πελάτη, υπόβαθρο μόνο είναι υπεύθυνος για την αποστολή ειδοποιήσεων. Οι εφαρμογές προγράμματος-πελάτη Διατηρήστε ενημερωμένες τις λαβές Γραμματίων και καταχώρηση ετικέτες. Η παρακάτω εικόνα παρουσιάζει αυτό το μοτίβο.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Η συσκευή πρώτα ανακτά τη λαβή Γραμματίων από το Γραμματίων και στη συνέχεια καταγράφει απευθείας με την ενότητα "Ειδοποίηση". Μετά την καταχώρηση είναι επιτυχής, η εφαρμογή παρασκηνίου να στείλετε μια ειδοποίηση στόχευσης ότι η καταχώρηση. Για περισσότερες πληροφορίες σχετικά με την αποστολή ειδοποιήσεων, ανατρέξτε στο θέμα [Δρομολόγηση και παραστάσεις ετικέτα](notification-hubs-tags-segment-push-message.md).
Σημειώστε ότι σε αυτήν την περίπτωση, θα χρησιμοποιήσετε ακρόαση μόνο δικαιώματα πρόσβασης σας διανομείς ειδοποίηση από τη συσκευή. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [ασφάλεια](notification-hubs-push-notification-security.md).

Καταχώρηση από τη συσκευή είναι η πιο απλή μέθοδος, αλλά έχει ορισμένα μειονεκτήματα.
Το πρώτο μειονέκτημα είναι ότι μια εφαρμογή προγράμματος-πελάτη μόνο να ενημερώσετε τις ετικέτες όταν η εφαρμογή είναι ενεργή. Για παράδειγμα, εάν ένας χρήστης έχει δύο συσκευές που καταχώρηση ετικέτες που σχετίζονται με τις ομάδες άθλημα, κατά την πρώτη συσκευή καταχωρεί για μια επιπλέον ετικέτα (για παράδειγμα, η Ελλάδα), η δεύτερη συσκευή δεν θα λαμβάνετε σε τις ειδοποιήσεις σχετικά με την Ελλάδα μέχρι να εκτελεστεί η εφαρμογή από τη δεύτερη συσκευή είναι μια δεύτερη φορά. Γενικότερα, όταν ετικέτες επηρεάζονται από πολλές συσκευές, τη Διαχείριση ετικετών από υπολογιστή στο παρασκήνιο είναι επιθυμητό μια επιλογή.
Το δεύτερο μειονέκτημα της διαχείρισης εγγραφής από την εφαρμογή υπολογιστή-πελάτη είναι ότι, επειδή οι εφαρμογές μπορούν να είναι έγινε εισβολή, ασφάλιση την καταχώρηση σε συγκεκριμένες ετικέτες απαιτεί επιπλέον περίθαλψη, όπως εξηγείται στην ενότητα "ασφάλεια σε επίπεδο ετικέτα".



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Παράδειγμα κώδικα για την καταχώρηση με ένα διανομέα ειδοποίηση από μια συσκευή χρησιμοποιώντας μια εγκατάσταση 

Προς το παρόν, υποστηρίζεται μόνο με χρήση του [Ειδοποίηση διανομείς REST API](https://msdn.microsoft.com/library/mt621153.aspx).

Μπορείτε επίσης να χρησιμοποιήσετε τη μέθοδο ενημέρωσης ΚΏΔΙΚΑ χρησιμοποιώντας την [Τυπική JSON κώδικα](https://tools.ietf.org/html/rfc6902) για την ενημέρωση της εγκατάστασης.

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }

   

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Παράδειγμα κώδικα για την καταχώρηση με ένα διανομέα ειδοποίηση από μια συσκευή χρησιμοποιώντας μια καταχώρηση


Αυτές οι μέθοδοι Δημιουργία ή ενημέρωση μιας εγγραφής για τη συσκευή στην οποία ονομάζονται. Αυτό σημαίνει ότι για να ενημερώσετε τη λαβή ή τις ετικέτες, πρέπει να αντικαταστήσετε ολόκληρη την καταχώρηση. Να θυμάστε ότι καταχωρήσεων είναι μεταβατικές, έτσι θα πρέπει να έχετε πάντα μια αξιόπιστη χώρου αποθήκευσης με την τρέχουσα ετικέτες που χρειάζεται μια συγκεκριμένη συσκευή.


    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }
     
    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Διαχείριση εγγραφής από έναν υπολογιστή στο παρασκήνιο

Διαχείριση καταχωρήσεων από υπολογιστή στο παρασκήνιο απαιτεί σύνταξη πρόσθετων κώδικα. Η εφαρμογή από τη συσκευή πρέπει να παρέχουν την ενημερωμένη Γραμματίων χειρισμού στον υπολογιστή στο παρασκήνιο κάθε φορά που ξεκινά η εφαρμογή (μαζί με τις ετικέτες και τα πρότυπα) και υπόβαθρο πρέπει να ενημερώσετε αυτό λαβή στην ενότητα ειδοποίηση. Η παρακάτω εικόνα παρουσιάζει αυτήν τη σχεδίαση.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Τα πλεονεκτήματα της τη διαχείριση καταχωρήσεων από υπολογιστή στο παρασκήνιο περιλαμβάνουν τη δυνατότητα για την τροποποίηση ετικετών εγγραφών, ακόμα και όταν η αντίστοιχη εφαρμογή στη συσκευή είναι ανενεργός και για τον έλεγχο ταυτότητας στην εφαρμογή υπολογιστή-πελάτη πριν να προσθέσετε μια ετικέτα για την καταχώρηση.


#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Παράδειγμα κώδικα για την καταχώρηση με ένα διανομέα ειδοποίηση από έναν υπολογιστή στο παρασκήνιο χρησιμοποιώντας μια εγκατάσταση

Η συσκευή-πελάτη εξακολουθεί να παίρνει τη λαβή Γραμματίων και σχετικές εγκατάστασης ιδιότητες ως πριν από την και καλεί ένα προσαρμοσμένο API στο υπόβαθρο που μπορούν να εκτελούν την καταχώρηση και να εγκρίνετε ετικέτες κ.λπ. Υπόβαθρο να αξιοποιήσετε στο [SDK διανομέα ειδοποίησης για τις εργασίες παρασκηνίου](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Μπορείτε επίσης να χρησιμοποιήσετε τη μέθοδο ενημέρωσης ΚΏΔΙΚΑ χρησιμοποιώντας την [Τυπική JSON κώδικα](https://tools.ietf.org/html/rfc6902) για την ενημέρωση της εγκατάστασης.
 

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Παράδειγμα κώδικα για την καταχώρηση με ένα διανομέα ειδοποίηση από μια συσκευή χρησιμοποιώντας ένα αναγνωριστικό δήλωσης

Από την εφαρμογή παρασκηνίου, μπορείτε να εκτελέσετε βασικές λειτουργίες CRUDS σε καταχωρήσεις. Για παράδειγμα:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");
            
    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


Υπόβαθρο πρέπει να χειριστείτε ταυτόχρονης εκτέλεσης μεταξύ registration ενημερώνεται. Υπηρεσία Bus προσφέρει βέλτιστου ελέγχου για τη Διαχείριση εγγραφής. Στο επίπεδο HTTP, έχει υλοποιηθεί με τη χρήση του ETag για τις ενέργειες διαχείρισης εγγραφής. Αυτή η δυνατότητα χρησιμοποιείται με διαφάνεια από Microsoft SDK, το οποίο εμφανίσουν εξαίρεση εάν μια ενημέρωση απορριφθεί για λόγους ταυτόχρονης εκτέλεσης. Υπόβαθρο εφαρμογή είναι υπεύθυνος για το χειρισμό αυτές τις εξαιρέσεις και επανάληψη την ενημερωμένη έκδοση, εάν απαιτείται.