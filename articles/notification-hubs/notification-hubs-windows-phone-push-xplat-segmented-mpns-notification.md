<properties
    pageTitle="Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις (Windows Phone)"
    description="Χρησιμοποιήστε Azure διανομείς ειδοποίησης για να χρησιμοποιήσετε την ετικέτα στο καταχωρήσεων για να στείλετε πρόσφατες ειδήσεις για μια εφαρμογή για Windows Phone."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Επισκόπηση

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση Azure μετάδοση πρόσφατες ειδήσεις ειδοποιήσεων για μια εφαρμογή για Windows Phone 8.0/8.1 Silverlight. Εάν στοχεύετε Windows Store ή εφαρμογή για Windows Phone 8.1, ανατρέξτε στην έκδοση [Γενικής χρήσης των Windows](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) . Όταν ολοκληρωθεί η εγκατάσταση, θα έχετε τη δυνατότητα να εγγραφείτε για διακοπή κατηγορίες συζητήσεων που σας ενδιαφέρει και λήψη μόνο ειδοποιήσεων push για αυτές τις κατηγορίες. Αυτό το σενάριο είναι ένα κοινό μοτίβο για πολλές εφαρμογές όπου έχουν ειδοποιήσεις θα αποσταλούν σε ομάδες χρηστών που έχουν προηγουμένως δηλωθεί ως ενδιαφέροντος σε αυτά, π.χ., πρόγραμμα ανάγνωσης RSS, εφαρμογές για το ανεμιστήρες μουσικής, κ.λπ.

Εκπομπή σενάρια είναι ενεργοποιημένα, συμπεριλαμβάνοντας μία ή περισσότερες _ετικέτες_ κατά τη δημιουργία μιας εγγραφής στην ενότητα ειδοποίησης. Όταν μια ετικέτα αποστέλλονται ειδοποιήσεις, όλες τις συσκευές που έχετε καταχωρήσει για την ετικέτα θα λάβετε την ειδοποίηση. Επειδή οι ετικέτες είναι απλώς συμβολοσειρές, δεν χρειάζεται να γίνει παροχή εκ των προτέρων. Για περισσότερες πληροφορίες σχετικά με τις ετικέτες, ανατρέξτε [ειδοποίηση διανομείς δρομολόγηση και παραστάσεις ετικέτα](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το θέμα δημιουργεί στην εφαρμογή του που δημιουργήσατε [γρήγορα]αποτελέσματα με το ειδοποίηση διανομείς. Πριν να ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ήδη ολοκληρώσει [Γρήγορα αποτελέσματα με ειδοποίηση διανομείς].

##<a name="add-category-selection-to-the-app"></a>Προσθήκη επιλογή κατηγορίας στην εφαρμογή

Το πρώτο βήμα είναι να προσθέσετε τα στοιχεία του περιβάλλοντος εργασίας Χρήστη του υπάρχοντος κύρια σελίδα που επιτρέπει στο χρήστη να επιλέξει κατηγορίες για την καταχώρηση. Οι κατηγορίες που επιλέγονται από ένα χρήστη είναι αποθηκευμένα στη συσκευή. Κατά την εκκίνηση της εφαρμογής, μια καταχώρηση της συσκευής έχει δημιουργηθεί στο κέντρο σας ειδοποίηση με επιλεγμένες κατηγορίες ως ετικέτες.

1. Ανοίξτε το αρχείο έργου MainPage.xaml και, στη συνέχεια, αντικαταστήστε τα στοιχεία του **πλέγματος** με το όνομα `TitlePanel` και `ContentPanel` με τον ακόλουθο κώδικα:

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. Στο έργο, δημιουργία μιας νέας κλάσης με το όνομα της **ειδοποιήσεις**, προσθέστε τη **δημόσια** πλήκτρο τροποποίησης στον ορισμό κλάσης και, στη συνέχεια, προσθέστε τις παρακάτω προτάσεις **χρησιμοποιώντας** το νέο αρχείο κώδικα:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;

3. Αντιγράψτε τον παρακάτω κώδικα τη νέα κλάση **ειδοποιήσεις** :

        private NotificationHub hub;

        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            return await SubscribeToCategories();
        }

        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();

            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;

                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }

            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
            
            return await registrationTask.Task;
        }

        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }

        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";

            // The stored categories tags are passed with the template registration.

            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

            return await registrationTask.Task;
        }

        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;

            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }

            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }


    Αυτή η κλάση χρησιμοποιεί διακριτή αποθήκευση για να αποθηκεύσετε τις κατηγορίες συζητήσεων που είναι αυτήν τη συσκευή για να λάβετε. Περιέχει επίσης μεθόδους για να εγγραφείτε για αυτές τις κατηγορίες με χρήση ενός [προτύπου](notification-hubs-templates-cross-platform-push-messages.md) εγγραφή της ειδοποίησης.


4. Στο αρχείο έργου App.xaml.cs, προσθέστε την ακόλουθη ιδιότητα στην **εφαρμογή** κλάση. Αντικαταστήστε το `<hub name>` και `<connection string with listen access>` σύμβολα κράτησης θέσης με το όνομα διανομέας ειδοποίησης και τη συμβολοσειρά σύνδεσης για *DefaultListenSharedAccessSignature* που λάβατε νωρίτερα.

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    > [AZURE.NOTE] Επειδή τα διαπιστευτήρια που διανέμονται με μια εφαρμογή προγράμματος-πελάτη δεν είναι γενικά ασφαλής, πρέπει να διανείμετε μόνο το κλειδί για ακρόαση πρόσβαση με την εφαρμογή υπολογιστή-πελάτη σας. Ακούστε access παρέχει τη δυνατότητα να εγγραφείτε για τις ειδοποιήσεις, αλλά υπάρχουσες καταχωρήσεις εφαρμογή σας δεν μπορεί να τροποποιηθεί και δεν είναι δυνατό να αποστέλλονται ειδοποιήσεις. Χρησιμοποιείται το κλειδί πλήρη πρόσβαση σε μια υπηρεσία ασφαλούς υποστήριξης για την αποστολή ειδοποιήσεων και αλλαγή υπάρχουσες καταχωρήσεις.

5. Στο MainPage.xaml.cs σας, προσθέστε την ακόλουθη γραμμή:

        using Windows.UI.Popups;

6. Στο αρχείο έργου MainPage.xaml.cs, προσθέστε την ακόλουθη μέθοδο:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
    
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
    
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }

    Αυτή η μέθοδος δημιουργεί μια λίστα κατηγοριών και χρησιμοποιεί την κλάση **ειδοποιήσεις** για την αποθήκευση της λίστας στο χώρο αποθήκευσης της τοπικής και καταχώρηση τις αντίστοιχες ετικέτες με το Κέντρο ειδοποίηση σας. Όταν αλλάζουν οι κατηγορίες, η καταχώρηση αναδημιουργείται με τις νέες κατηγορίες.

Εφαρμογή σας είναι τώρα μπορείτε να αποθηκεύσετε ένα σύνολο κατηγοριών στον τοπικό χώρο αποθήκευσης στη συσκευή και καταχώρηση με την ενότητα "Ειδοποίηση" κάθε φορά που ο χρήστης αλλάζει την επιλογή των κατηγοριών.

##<a name="register-for-notifications"></a>Εγγραφή για ειδοποιήσεις

Αυτά τα βήματα καταχωρήστε την ενότητα "Ειδοποίηση" κατά την εκκίνηση χρησιμοποιώντας τις κατηγορίες που έχουν αποθηκευτεί στον τοπικό χώρο αποθήκευσης.

> [AZURE.NOTE] Επειδή το κανάλι URI που έχουν εκχωρηθεί από τη Microsoft Push ειδοποίηση υπηρεσίας (MPNS) μπορεί να αλλάξετε οποιαδήποτε στιγμή, πρέπει να δηλώσετε για τις ειδοποιήσεις συχνά για να αποφύγετε την ειδοποίηση αποτυχίες. Αυτό το παράδειγμα καταχωρεί για τις ειδοποιήσεις κάθε φορά που ξεκινά η εφαρμογή. Για τις εφαρμογές που εκτελούνται συχνά, περισσότερες από μία φορά την ημέρα, μπορείτε να πιθανώς παραλείψετε εγγραφής για να διατηρήσετε το εύρος ζώνης, εάν έχει περάσει μικρότερη από την ημέρα από την προηγούμενη εγγραφή.


1. Ανοίξτε το αρχείο App.xaml.cs και προσθέστε το πλήκτρο τροποποίησης **ασύγχρονης** μέθοδο **Application_Launching** και αντικαταστήσετε τον κωδικό καταχώρησης διανομείς ειδοποίησης που έχετε προσθέσει στο [Γρήγορα αποτελέσματα με διανομείς ειδοποίηση] με τον ακόλουθο κώδικα:

        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();

            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }

    Αυτό διασφαλίζει ότι κάθε φορά που ξεκινά η εφαρμογή ανακτά τις κατηγορίες από τοπικού χώρου αποθήκευσης και τις αιτήσεις μια εγγραφή για αυτές τις κατηγορίες.

2. Στο αρχείο έργου MainPage.xaml.cs, προσθέστε τον ακόλουθο κώδικα που υλοποιεί τη μέθοδο **OnNavigatedTo** :

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
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

    ![][2]

3. Μετά τη λήψη επιβεβαίωση ότι οι κατηγορίες έχουν ολοκληρώσει τη συνδρομή, εκτελέστε την εφαρμογή κονσόλας για την αποστολή ειδοποιήσεων για κάθε κατηγορίες. Επαλήθευση μόνο λαμβάνετε μια ειδοποίηση για τις κατηγορίες που έχετε εγγραφεί στο.

    ![][3]

Ολοκληρώσατε αυτό το θέμα.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Γρήγορα αποτελέσματα με διανομείς ειδοποίησης]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

