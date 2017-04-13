<properties
    pageTitle="Αποστολή ειδοποιήσεων push με διανομείς ειδοποίηση Azure σε Windows Phone | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς να χρησιμοποιείτε Azure διανομείς ειδοποίησης για τις ειδοποιήσεις push για μια εφαρμογή του Windows Phone 8 ή Windows Phone 8.1 Silverlight."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="ειδοποίηση προώθησης, push ειδοποίηση push του windows phone"
    authors="ysxu"
    manager="erikre"
    editor="erikre"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Αποστολή ειδοποιήσεων push με διανομείς ειδοποίηση Azure σε Windows Phone

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Επισκόπηση

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure ειδοποίηση διανομείς για την αποστολή ειδοποιήσεων push σε μια εφαρμογή του Windows Phone 8 ή Windows Phone 8.1 Silverlight. Εάν στοχεύετε σε Windows Phone 8.1 (μη Silverlight), στη συνέχεια, ανατρέξτε στην έκδοση [Γενικής χρήσης των Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) .
Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε μια κενή εφαρμογή Windows Phone 8 που λαμβάνει τις ειδοποιήσεις push, χρησιμοποιώντας την υπηρεσία ειδοποιήσεων Push της Microsoft (MPNS). Όταν ολοκληρώσετε την εργασία, θα έχετε τη δυνατότητα να χρησιμοποιήσετε το Κέντρο ειδοποίηση σας μετάδοση ειδοποιήσεων push σε όλες τις συσκευές που εκτελεί την εφαρμογή σας.

> [AZURE.NOTE] SDK για Windows Phone διανομείς την ειδοποίηση δεν υποστηρίζει χρησιμοποιώντας την υπηρεσία Push ειδοποιήσεων των Windows (WNS) με τις εφαρμογές του Windows Phone 8.1 Silverlight. Για να χρησιμοποιήσετε WNS (αντί για MPNS) με τις εφαρμογές του Windows Phone 8.1 Silverlight, ακολουθήστε την [Ειδοποίηση διανομείς - πρόγραμμα εκμάθησης Silverlight Windows Phone], που χρησιμοποιεί REST API.

Το πρόγραμμα εκμάθησης δείχνει το απλό σενάριο εκπομπής χρησιμοποιώντας την ειδοποίηση διανομείς.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ [Visual Studio 2012 Express για Windows Phone]ή νεότερη έκδοση.

Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλους άλλα προγράμματα εκμάθησης διανομείς ειδοποίησης για τις εφαρμογές του Windows Phone 8.

##<a name="create-your-notification-hub"></a>Δημιουργία κέντρο σας ειδοποίηση

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Κάντε κλικ στην ενότητα <b>Υπηρεσίες ειδοποίησης</b> (εντός <i>Ρυθμίσεις</i>), κάντε κλικ στο <b>Windows Phone (MPNS)</b> και, στη συνέχεια, επιλέξτε το πλαίσιο ελέγχου <b>Ενεργοποίηση push χωρίς έλεγχο ταυτότητας</b> .</p>
</li>
</ol>

&emsp;&emsp;![Azure πύλης - ενεργοποίηση ειδοποιήσεων push unathenticated](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Το Κέντρο σας τώρα δημιουργείται και έχει ρυθμιστεί για να στείλετε μια ειδοποίηση χωρίς έλεγχο ταυτότητας για Windows Phone.

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί MPNS σε λειτουργία χωρίς έλεγχο ταυτότητας. Λειτουργία χωρίς έλεγχο ταυτότητας MPNS διατίθεται με περιορισμούς τις ειδοποιήσεις που μπορείτε να στείλετε σε κάθε κανάλι. Ειδοποίηση διανομείς υποστηρίζει [MPNS ελεγχθεί η ταυτότητά λειτουργία](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) , επιτρέποντάς σας να αποστείλετε το πιστοποιητικό.

##<a name="connecting-your-app-to-the-notification-hub"></a>Σύνδεση της εφαρμογής σας με την ενότητα "Ειδοποίηση"

1. Στο Visual Studio, δημιουργήστε μια νέα εφαρμογή Windows Phone 8.

    ![Εφαρμογή του Visual Studio - νέο έργο - Windows Phone][13]

    Στο Visual Studio 2013 ενημερωμένη έκδοση 2 ή νεότερες εκδόσεις, μπορείτε αντί για αυτό να δημιουργήσετε μια εφαρμογή του Windows Phone Silverlight.

    ![Visual Studio - νέο έργο - κενή εφαρμογή - Windows Phone Silverlight][11]

2. Στο Visual Studio, κάντε δεξί κλικ τη λύση και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.

    Εμφανίζει το παράθυρο διαλόγου **Διαχείριση πακέτων NuGet** .

3. Αναζήτηση για `WindowsAzure.Messaging.Managed` και κάντε κλικ στην επιλογή **εγκατάσταση**και, στη συνέχεια, αποδεχτείτε τους όρους χρήσης.

    ![Visual Studio - Διαχείριση πακέτου NuGet][20]

    Αυτό στοιχεία λήψης, εγκαθιστά και προσθέτει μια αναφορά στη βιβλιοθήκη μηνυμάτων Azure για Windows με τη χρήση του <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">πακέτου WindowsAzure.Messaging.Managed NuGet</a>.

4. Ανοίξτε το αρχείο App.xaml.cs και προσθέστε το εξής `using` προτάσεις:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. Προσθέστε τον ακόλουθο κώδικα στο επάνω μέρος της μεθόδου **Application_Launching** στο App.xaml.cs:

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());

            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });

    >[AZURE.NOTE] Η τιμή **MyPushChannel** είναι ένα ευρετήριο που χρησιμοποιείται για να αναζητήσετε ένα υπάρχον κανάλι στη συλλογή [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) . Εάν δεν υπάρχει εκεί, δημιουργήστε μια νέα καταχώρηση με αυτό το όνομα.
    
    Βεβαιωθείτε ότι για να εισαγάγετε το όνομα του Κέντρο σας και τη συμβολοσειρά σύνδεσης που ονομάζεται **DefaultListenSharedAccessSignature** που έχετε λάβει στην προηγούμενη ενότητα.
    Αυτός ο κωδικός ανακτά το κανάλι URI για την εφαρμογή από MPNS και, στη συνέχεια, καταγράφει αυτό το κανάλι URI με το Κέντρο ειδοποίηση σας. Το επίσης εγγυάται ότι το κανάλι URI έχει καταχωρηθεί στο κέντρο ειδοποίηση σας κάθε φορά που γίνεται εκκίνηση της εφαρμογής.

    >[AZURE.NOTE]Αυτό το πρόγραμμα εκμάθησης στέλνει μια αναδυόμενη ειδοποίηση στη συσκευή. Όταν στέλνετε μια ειδοποίηση πλακίδιο, μπορείτε αντί για αυτό πρέπει να καλέσετε τη μέθοδο **BindToShellTile** στο κανάλι. Για να υποστηρίζουν και τα δύο αναδυόμενη και πλακιδίου ειδοποιήσεις, καλέστε **BindToShellTile** και **BindToShellToast**.

6. Στην Εξερεύνηση λύσεων, αναπτύξτε το στοιχείο **Ιδιότητες**, ανοίξτε το `WMAppManifest.xml` αρχείων, κάντε κλικ στην καρτέλα **δυνατότητες** και βεβαιωθείτε ότι είναι ενεργοποιημένη η δυνατότητα **ID_CAP_PUSH_NOTIFICATION** .

    ![Visual Studio - Windows Phone App δυνατότητες][14]

    Αυτό εξασφαλίζει ότι η εφαρμογή σας να λαμβάνετε ειδοποιήσεις push. Χωρίς, οποιαδήποτε προσπάθεια για να στείλετε μια ειδοποίηση push εφαρμογή θα αποτύχει.

7. Πατήστε το `F5` κλειδί για να εκτελέσετε την εφαρμογή.

    Εμφανίζεται ένα μήνυμα εγγραφής στην εφαρμογή.

8. Κλείσιμο της εφαρμογής.  

   >[AZURE.NOTE] Για να λάβετε μια αναδυόμενη ειδοποίηση push, η εφαρμογή δεν πρέπει να εκτελείται σε πρώτο πλάνο.

##<a name="send-push-notifications-from-your-backend"></a>Αποστολή ειδοποιήσεων push από τον υπολογιστή στο παρασκήνιο

Μπορείτε να στείλετε τις ειδοποιήσεις push χρησιμοποιώντας διανομείς ειδοποίηση από οποιαδήποτε παρασκηνίου μέσω δημόσιας <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">ΥΠΌΛΟΙΠΑ περιβάλλοντος εργασίας</a>. Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να στείλετε τις ειδοποιήσεις push χρησιμοποιώντας μια εφαρμογή κονσόλας .NET. 

Για ένα παράδειγμα του τρόπου για την αποστολή ειδοποιήσεων push από μια παρασκηνίου ASP.NET WebAPI που είναι ενσωματωμένο στο διανομείς ειδοποίησης, ανατρέξτε στο θέμα [Azure ειδοποίηση διανομείς ειδοποιήστε τους χρήστες με .NET παρασκηνίου](./notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Για ένα παράδειγμα του τρόπου αποστολής τις ειδοποιήσεις push, χρησιμοποιώντας τα [ΥΠΌΛΟΙΠΑ APIs](https://msdn.microsoft.com/library/azure/dn223264.aspx), δείτε [πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση από Java](./notification-hubs-java-push-notification-tutorial.md) και [Πώς να χρησιμοποιήσετε διανομείς ειδοποίηση από PHP](./notification-hubs-php-push-notification-tutorial.md).

1. Κάντε δεξί κλικ τη λύση, επιλέξτε **Προσθήκη** και το **Νέο έργο...**, και, στη συνέχεια, στην περιοχή **Visual C#**, κάντε κλικ στην επιλογή **Windows** και **Εφαρμογής κονσόλας**και κάντε κλικ στο κουμπί **OK**.

    ![Εφαρμογή κονσόλας Visual Studio - νέο έργο-][6]

    Αυτό προσθέτει μια νέα οπτική C# κονσόλα εφαρμογή στη λύση. Μπορείτε επίσης να κάνετε αυτό σε μια ξεχωριστή λύση.

4. Κάντε κλικ στην επιλογή **Εργαλεία**, κάντε κλικ στην επιλογή **Διαχείριση πακέτου βιβλιοθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **Κονσόλα διαχείρισης πακέτου**.

    Εμφανίζει την Κονσόλα διαχείρισης πακέτου.

5.  Στο παράθυρο **Κονσόλα διαχείρισης πακέτου** , ορίστε την **προεπιλεγμένη έργου** στο έργο σας νέα εφαρμογή κονσόλας και, στη συνέχεια, στο παράθυρο της κονσόλας, εκτελέστε την ακόλουθη εντολή:

        Install-Package Microsoft.Azure.NotificationHubs

    Αυτό προσθέτει μια αναφορά στο SDK διανομείς ειδοποίηση Azure χρησιμοποιώντας το <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">πακέτο Microsoft.Azure.Notification NuGet διανομείς</a>.

6. Άνοιγμα του `Program.cs` αρχείων και προσθέστε το εξής `using` δήλωση:

        using Microsoft.Azure.NotificationHubs;

6. Στο το `Program` κλάση, προσθέστε την ακόλουθη μέθοδο:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    Βεβαιωθείτε ότι για να αντικαταστήσετε το `<hub name>` σύμβολο κράτησης θέσης με το όνομα του διανομέα ειδοποίηση που εμφανίζεται στην πύλη. Επίσης, αντικαταστήστε το κείμενο κράτησης θέσης συμβολοσειρά σύνδεσης με τη συμβολοσειρά σύνδεσης που ονομάζεται **DefaultFullSharedAccessSignature** που έχετε λάβει στην ενότητα "Ρύθμιση παραμέτρων του διανομέα ειδοποίησης".

    >[AZURE.NOTE]Βεβαιωθείτε ότι χρησιμοποιείτε τη συμβολοσειρά σύνδεσης με **πλήρη** πρόσβαση, δεν **ακούτε** πρόσβασης. Η συμβολοσειρά ακρόαση πρόσβασης δεν διαθέτει δικαιώματα για την αποστολή ειδοποιήσεων push.

4. Προσθέστε την ακόλουθη γραμμή στο σας `Main` μέθοδο:

         SendNotificationAsync();
         Console.ReadLine();

5. Με το Windows Phone προσομοίωσης εκτελείται και να κλείσει την εφαρμογή σας, ορίστε το έργο εφαρμογής κονσόλας ως του προεπιλεγμένου έργου εκκίνησης και, στη συνέχεια, πατήστε το `F5` κλειδί για να εκτελέσετε την εφαρμογή.

    Θα λάβετε μια αναδυόμενη ειδοποίηση push. Πατώντας διαφημιστικό πλαίσιο αναδυόμενη φορτώνει την εφαρμογή.

Μπορείτε να βρείτε όλων των πιθανών φορτίων στα θέματα [αναδυόμενη καταλόγου] και [πλακιδίου στον κατάλογο] στο MSDN.

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το απλό παράδειγμα μετάδοση ειδοποιήσεων push για όλες τις συσκευές σας Windows Phone 8. 

Για να στοχεύετε σε συγκεκριμένους χρήστες, ανατρέξτε στο το πρόγραμμα εκμάθησης [Χρήση διανομείς ειδοποίησης για τις ειδοποιήσεις push στους χρήστες] . 

Εάν θέλετε να χωρίσετε τους χρήστες σας από τις ομάδες που σας ενδιαφέρουν, μπορείτε να διαβάσετε [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις]. 

Μάθετε περισσότερα σχετικά με τη χρήση διανομείς ειδοποίηση στις [Οδηγίες διανομείς ειδοποίησης].



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express για Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Ειδοποίηση διανομείς καθοδήγηση]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Χρήση διανομείς ειδοποίησης για τις ειδοποιήσεις push στους χρήστες]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[αναδυόμενη καταλόγου]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[πλακίδιο καταλόγου]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Ειδοποίηση διανομείς - πρόγραμμα εκμάθησης Silverlight Windows Phone]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

