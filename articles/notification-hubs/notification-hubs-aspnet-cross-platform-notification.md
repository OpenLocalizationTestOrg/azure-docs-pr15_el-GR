<properties
    pageTitle="Αποστολή ειδοποιήσεων πλατφόρμες σε χρήστες με ειδοποίηση διανομείς (ASP.NET)"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε πρότυπα διανομείς ειδοποίησης για να στείλετε, σε μια μεμονωμένη αίτηση, μια ειδοποίηση agnostic πλατφόρμα που ως προορισμό όλες τις πλατφόρμες."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Αποστολή ειδοποιήσεων πλατφόρμες σε χρήστες με διανομείς ειδοποίησης


Στην προηγούμενη εκμάθηση [ειδοποίηση οι χρήστες με ειδοποίηση διανομείς], μάθατε πώς να ειδοποιήσεις push σε όλες τις συσκευές που έχουν καταχωρηθεί από έναν συγκεκριμένο χρήστη με έλεγχο ταυτότητας. Σε αυτό το πρόγραμμα εκμάθησης, πολλές αιτήσεις έχουν απαιτείται για να στείλετε μια ειδοποίηση σε κάθε πλατφόρμα υποστηριζόμενο πρόγραμμα-πελάτη. Ειδοποίηση διανομείς υποστηρίζει πρότυπα, τα οποία σας επιτρέπουν να καθορίσετε πώς μια συγκεκριμένη συσκευή θέλει να λαμβάνω ειδοποιήσεις. Αυτό απλοποιεί αποστολή ειδοποιήσεων πλατφόρμες. Αυτό το θέμα παρουσιάζει πώς μπορείτε να επωφεληθείτε από τα πρότυπα για να στείλετε, σε μια μεμονωμένη αίτηση, μια ειδοποίηση agnostic πλατφόρμα που ως προορισμό όλες τις πλατφόρμες. Για πιο λεπτομερείς πληροφορίες σχετικά με τα πρότυπα, ανατρέξτε στο θέμα [Επισκόπηση διανομείς ειδοποίηση Azure][Templates].

> [AZURE.NOTE] Ειδοποίηση διανομείς επιτρέπει σε μια συσκευή για να καταχωρήσετε πολλά πρότυπα με την ίδια ετικέτα. Σε αυτήν την περίπτωση, ενός εισερχόμενου μηνύματος στόχευσης αυτήν την ετικέτα αποτέλεσμα πολλές ειδοποιήσεις παραδόθηκε στη συσκευή, μία για κάθε πρότυπο. Αυτό σας επιτρέπει να εμφανίσετε το ίδιο μήνυμα σε πολλές οπτικές ειδοποιήσεις, όπως και τα δύο ως σήματος και ως μια αναδυόμενη ειδοποίηση σε μια εφαρμογή για Windows Store.

Ολοκληρώστε τα ακόλουθα βήματα για την αποστολή ειδοποιήσεων πλατφόρμες με τη χρήση προτύπων:

1. Στην Εξερεύνηση λύσεων στο Visual Studio, αναπτύξτε το φάκελο **ελεγκτές** και, στη συνέχεια, ανοίξτε το αρχείο RegisterController.cs.

2. Εντοπίστε το τμήμα κώδικα στη μέθοδο **Δημοσίευση** που δημιουργεί μια νέα καταχώρηση αντικατάσταση του `switch` περιεχομένου με τον ακόλουθο κώδικα:

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    Αυτός ο κώδικας καλεί τη μέθοδο συγκεκριμένης πλατφόρμας για να δημιουργήσετε ένα πρότυπο δήλωσης αντί για μια καταχώρηση εγγενή. Υπάρχουσες καταχωρήσεις δεν πρέπει να τροποποιηθεί, επειδή το πρότυπο καταχωρήσεων προέρχεται από εγγενή καταχωρήσεων.

3. Στον ελεγκτή **ειδοποιήσεις** , αντικαταστήστε τη μέθοδο **sendNotification** με τον ακόλουθο κώδικα:

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    Αυτός ο κωδικός στέλνει μια ειδοποίηση σε όλες τις πλατφόρμες ταυτόχρονα και χωρίς να χρειάζεται να καθορίσετε μια εγγενή φορτίο. Ειδοποίηση διανομείς δημιουργεί και προσφέρει το σωστό φορτίο κάθε συσκευή με την τιμή που παρέχεται _ετικέτα_ , όπως καθορίζονται στο καταχωρημένων προτύπων.

4. Να δημοσιεύσετε εκ νέου το έργο σας WebApi παρασκηνίου.

5. Εκτελέστε ξανά την εφαρμογή υπολογιστή-πελάτη και βεβαιωθείτε ότι είναι επιτυχής εγγραφή.

6. (Προαιρετικό) Ανάπτυξη της εφαρμογής υπολογιστή-πελάτη σε μια δεύτερη συσκευή και, στη συνέχεια, εκτελέστε την εφαρμογή.

    Παρατηρήστε ότι εμφανίζεται μια ειδοποίηση σε κάθε συσκευή.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε ολοκληρώσει αυτό το πρόγραμμα εκμάθησης, μάθετε περισσότερα σχετικά με την ειδοποίηση διανομείς και πρότυπα σε αυτά τα θέματα:

+ **[Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις]** <br/>Παρουσιάζει ένα άλλο σενάριο για τη χρήση προτύπων

+  **[Επισκόπηση διανομείς Azure ειδοποίησης][Templates]**<br/>Το θέμα Επισκόπηση έχει πιο λεπτομερείς πληροφορίες σχετικά με τα πρότυπα.


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Ειδοποιήστε τους χρήστες με διανομείς ειδοποίησης]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
