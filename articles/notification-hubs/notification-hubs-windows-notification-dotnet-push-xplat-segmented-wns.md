<properties
    pageTitle="Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις (Windows Universal)"
    description="Χρησιμοποιήστε Azure ειδοποίηση διανομείς με ετικέτες στην καταχώρηση για να στείλετε πρόσφατες ειδήσεις για μια καθολική εφαρμογή για Windows."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>


<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Επισκόπηση

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση Azure μετάδοση πρόσφατες ειδήσεις ειδοποιήσεις σε Windows Store ή την εφαρμογή Windows Phone 8.1 (μη Silverlight). Εάν στοχεύετε σε Windows Phone 8.1 Silverlight, ανατρέξτε στην έκδοση [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) . Όταν ολοκληρωθεί η εγκατάσταση, θα έχετε τη δυνατότητα να εγγραφείτε για διακοπή κατηγορίες συζητήσεων που σας ενδιαφέρει και λήψη μόνο ειδοποιήσεων push για αυτές τις κατηγορίες. Αυτό το σενάριο είναι ένα κοινό μοτίβο για πολλές εφαρμογές όπου έχουν ειδοποιήσεις θα αποσταλούν σε ομάδες χρηστών που έχουν προηγουμένως δηλωθεί ενδιαφέροντος σε αυτά, π.χ. πρόγραμμα ανάγνωσης RSS, εφαρμογές για το ανεμιστήρες μουσικής, και ούτω καθεξής. 

Εκπομπή σενάρια είναι ενεργοποιημένα, συμπεριλαμβάνοντας μία ή περισσότερες _ετικέτες_ κατά τη δημιουργία μιας εγγραφής στην ενότητα ειδοποίησης. Όταν μια ετικέτα αποστέλλονται ειδοποιήσεις, όλες τις συσκευές που έχετε καταχωρήσει για την ετικέτα θα λάβετε την ειδοποίηση. Επειδή οι ετικέτες είναι απλώς συμβολοσειρές, δεν χρειάζεται να γίνει παροχή εκ των προτέρων. Για περισσότερες πληροφορίες σχετικά με τις ετικέτες, ανατρέξτε [ειδοποίηση διανομείς δρομολόγηση και παραστάσεις ετικέτα](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το θέμα δημιουργεί στην εφαρμογή του που δημιουργήσατε [γρήγορα]αποτελέσματα με το διανομείς ειδοποίηση[get-started]. Πριν να ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ήδη ολοκληρώσει [Γρήγορα αποτελέσματα με διανομείς ειδοποίηση][get-started].

##<a name="add-category-selection-to-the-app"></a>Προσθήκη επιλογή κατηγορίας στην εφαρμογή

Το πρώτο βήμα είναι να προσθέσετε τα στοιχεία του περιβάλλοντος εργασίας Χρήστη του υπάρχοντος κύρια σελίδα που επιτρέπει στο χρήστη να επιλέξει κατηγορίες για την καταχώρηση. Οι κατηγορίες που επιλέγονται από ένα χρήστη είναι αποθηκευμένα στη συσκευή. Κατά την εκκίνηση της εφαρμογής, μια καταχώρηση της συσκευής έχει δημιουργηθεί στο κέντρο σας ειδοποίηση με επιλεγμένες κατηγορίες ως ετικέτες.

1. Ανοίξτε το αρχείο έργου MainPage.xaml και, στη συνέχεια, αντιγράψτε τον παρακάτω κώδικα στο στοιχείο **πλέγματος** :

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. Κάντε δεξί κλικ στο έργο **κοινή χρήση** και προσθέσετε μια νέα κατηγορία με το όνομα της **ειδοποιήσεις**, προσθέστε τη **δημόσια** πλήκτρο τροποποίησης στον ορισμό κλάσης, στη συνέχεια, προσθέστε τις παρακάτω προτάσεις **χρησιμοποιώντας** το νέο αρχείο κώδικα:

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Αντιγράψτε τον παρακάτω κώδικα τη νέα κλάση **ειδοποιήσεις** :

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }

    Αυτή η κλάση χρησιμοποιεί την τοπική αποθήκευση για να αποθηκεύσετε τις κατηγορίες συζητήσεων που έχει αυτήν τη συσκευή για να λάβετε. Σημειώστε ότι αντί για κλήση της μεθόδου *RegisterNativeAsync* ονομάζουμε *RegisterTemplateAsync* για την καταχώρηση για τις κατηγορίες χρησιμοποιώντας ένα πρότυπο εγγραφής. 
    
    Επίσης, παρέχουμε ένα όνομα για το πρότυπο ("simpleWNSTemplateExample"), επειδή μπορεί να θέλουμε να καταχωρήσετε περισσότερα από ένα πρότυπο (για παράδειγμα μία για τις ειδοποιήσεις αναδυόμενη) και μία για τα πλακίδια και πρέπει να του δώσετε ένα όνομα για να έχετε τη δυνατότητα να ενημερώσετε ή να διαγράψετε τους.

    Λάβετε υπόψη ότι εάν μια συσκευή καταγράφει πολλά πρότυπα με την ίδια ετικέτα, μια εισερχόμενων μηνυμάτων στόχευσης που οδηγούν σε πολλές ειδοποιήσεις ετικέτα παραδίδονται στη συσκευή (μία για κάθε πρότυπο). Αυτή η συμπεριφορά είναι χρήσιμη όταν το ίδιο μήνυμα λογική πρέπει να έχει ως αποτέλεσμα πολλές οπτικές ειδοποιήσεις, για παράδειγμα που εμφανίζει μια ένδειξη και μια αναδυόμενη σε μια εφαρμογή του Windows Store.

    Για περισσότερες πληροφορίες σχετικά με τα πρότυπα, ανατρέξτε στο θέμα [πρότυπα](notification-hubs-templates-cross-platform-push-messages.md).




4. Στο αρχείο έργου App.xaml.cs, προσθέστε την ακόλουθη ιδιότητα στην κλάση **εφαρμογής** :

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    Αυτή η ιδιότητα χρησιμοποιείται για να δημιουργήσετε και να αποκτήσετε πρόσβαση σε μια παρουσία **ειδοποιήσεις** .

    Στο παραπάνω κώδικα, αντικαταστήστε το `<hub name>` και `<connection string with listen access>` σύμβολα κράτησης θέσης με το όνομα διανομέας ειδοποίησης και τη συμβολοσειρά σύνδεσης για *DefaultListenSharedAccessSignature* που λάβατε νωρίτερα.

    > [AZURE.NOTE] Επειδή τα διαπιστευτήρια που διανέμονται με μια εφαρμογή προγράμματος-πελάτη δεν είναι γενικά ασφαλής, πρέπει να διανείμετε μόνο το κλειδί για ακρόαση πρόσβαση με την εφαρμογή υπολογιστή-πελάτη σας. Ακούστε access παρέχει τη δυνατότητα να εγγραφείτε για τις ειδοποιήσεις, αλλά υπάρχουσες καταχωρήσεις εφαρμογή σας δεν μπορεί να τροποποιηθεί και δεν είναι δυνατό να αποστέλλονται ειδοποιήσεις. Χρησιμοποιείται το κλειδί πλήρη πρόσβαση σε μια υπηρεσία ασφαλούς υποστήριξης για την αποστολή ειδοποιήσεων και αλλαγή υπάρχουσες καταχωρήσεις.

5. Στο MainPage.xaml.cs σας, προσθέστε την ακόλουθη γραμμή:

        using Windows.UI.Popups;

6. Στο αρχείο έργου MainPage.xaml.cs, προσθέστε την ακόλουθη μέθοδο:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    Αυτή η μέθοδος δημιουργεί μια λίστα κατηγοριών και χρησιμοποιεί την κλάση **ειδοποιήσεις** για την αποθήκευση της λίστας στο χώρο αποθήκευσης της τοπικής και καταχώρηση τις αντίστοιχες ετικέτες με το Κέντρο ειδοποίηση σας. Όταν αλλάζουν οι κατηγορίες, η καταχώρηση αναδημιουργείται με τις νέες κατηγορίες.

Εφαρμογή σας είναι τώρα μπορείτε να αποθηκεύσετε ένα σύνολο κατηγοριών στον τοπικό χώρο αποθήκευσης στη συσκευή και καταχώρηση με την ενότητα "Ειδοποίηση" κάθε φορά που ο χρήστης αλλάζει την επιλογή των κατηγοριών.

##<a name="register-for-notifications"></a>Εγγραφή για ειδοποιήσεις

Αυτά τα βήματα καταχωρήστε την ενότητα "Ειδοποίηση" κατά την εκκίνηση χρησιμοποιώντας τις κατηγορίες που έχουν αποθηκευτεί στον τοπικό χώρο αποθήκευσης.

> [AZURE.NOTE] Επειδή το κανάλι URI που έχουν εκχωρηθεί από την υπηρεσία ειδοποιήσεων Windows (WNS) μπορεί να αλλάξετε οποιαδήποτε στιγμή, πρέπει να δηλώσετε για τις ειδοποιήσεις συχνά για να αποφύγετε την ειδοποίηση αποτυχίες. Αυτό το παράδειγμα καταχωρεί για ειδοποίηση κάθε φορά που ξεκινά η εφαρμογή. Για τις εφαρμογές που εκτελούνται συχνά, περισσότερες από μία φορά την ημέρα, μπορείτε να πιθανώς παραλείψετε εγγραφής για να διατηρήσετε το εύρος ζώνης, εάν έχει περάσει μικρότερη από την ημέρα από την προηγούμενη εγγραφή.

1. Ανοίξτε το αρχείο App.xaml.cs και να ενημερώσετε τη μέθοδο **InitNotificationsAsync** για να χρησιμοποιήσετε το `notifications` τάξης για να εγγραφείτε που βασίζονται σε κατηγορίες.

        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
    
        var result = await notifications.SubscribeToCategories();

    Αυτό διασφαλίζει ότι κάθε φορά που ξεκινά η εφαρμογή ανακτά τις κατηγορίες από τοπικού χώρου αποθήκευσης και ζητά ένα registeration για αυτές τις κατηγορίες. Η μέθοδος **InitNotificationsAsync** δημιουργήθηκε ως μέρος του το παράθυρο [Γρήγορα αποτελέσματα με διανομείς ειδοποίηση] [ get-started] το πρόγραμμα εκμάθησης.

3. Στο αρχείο έργου MainPage.xaml.cs, προσθέστε τον ακόλουθο κώδικα στη μέθοδο *OnNavigatedTo* :

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }

    Αυτή η επιλογή ενημερώνει την κύρια σελίδα με βάση την κατάσταση των κατηγοριών είχε αποθηκευτεί προηγουμένως.

Η εφαρμογή είναι τώρα ολοκληρωμένο και να αποθηκεύσετε ένα σύνολο κατηγορίες σε συσκευή τοπικό χώρο αποθήκευσης χρησιμοποιούνται για την καταχώρηση με την ενότητα "Ειδοποίηση" κάθε φορά που ο χρήστης αλλάζει την επιλογή των κατηγοριών. Στη συνέχεια, θα σας θα ορίσετε έναν υπολογιστή στο παρασκήνιο που μπορεί να σας στείλει ειδοποιήσεις κατηγορία αυτής της εφαρμογής.

##<a name="sending-tagged-notifications"></a>Αποστολή ειδοποιήσεων με ετικέτες

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Εκτελέστε την εφαρμογή και Δημιουργία ειδοποιήσεων

1. Στο Visual Studio, πατήστε το πλήκτρο F5 για να μεταγλωττίσετε και να ξεκινήσετε την εφαρμογή.

    ![][1]

    Σημειώστε ότι η εφαρμογή περιβάλλοντος εργασίας Χρήστη παρέχει ένα σύνολο εναλλαγή που σας επιτρέπει να επιλέξετε τις κατηγορίες για να εγγραφείτε για.

2. Ενεργοποίηση ενός ή περισσότερων εναλλαγή κατηγορίες και, στη συνέχεια, κάντε κλικ στην επιλογή **εγγραφή**.

    Η εφαρμογή μετατρέπει επιλεγμένες κατηγορίες σε ετικέτες και τις αιτήσεις μια νέα καταχώρηση της συσκευής για το επιλεγμένο ετικέτες από την ενότητα "Ειδοποίηση". Οι καταχωρημένες κατηγορίες είναι επιστρέφεται και εμφανίζεται σε ένα παράθυρο διαλόγου.

    ![][19]

4. Στείλτε μια ειδοποίηση για νέα από υπολογιστή στο παρασκήνιο, με έναν από τους εξής τρόπους:

    + **Εφαρμογής κονσόλας:** εκκινήστε την εφαρμογή κονσόλας.

    + **Java/PHP:** εκτέλεση εφαρμογή/δέσμη ενεργειών σας.

    Ειδοποιήσεις για επιλεγμένες κατηγορίες εμφανίζονται ως αναδυόμενη ειδοποιήσεις.

    ![][14]

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, θα σας μάθατε τον τρόπο εκπομπής πρόσφατες ειδήσεις ανά κατηγορία. Εξετάστε την ολοκλήρωση ένα από τα παρακάτω προγράμματα εκμάθησης που επισημαίνουν άλλα σενάρια διανομείς ειδοποίησης για προχωρημένους:

+ [Χρήση ειδοποιήσεων διανομείς μετάδοση μεταφρασμένα πρόσφατες ειδήσεις]

    Μάθετε πώς μπορείτε να αναπτύξετε την εφαρμογή πρόσφατες ειδήσεις για να ενεργοποιήσετε την αποστολή ειδοποιήσεων μεταφρασμένα.



<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Χρήση ειδοποιήσεων διανομείς μετάδοση μεταφρασμένα πρόσφατες ειδήσεις]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
