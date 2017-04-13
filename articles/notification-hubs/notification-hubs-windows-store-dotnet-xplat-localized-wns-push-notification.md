<properties
    pageTitle="Ειδοποίηση διανομείς μεταφραστεί πρόσφατες ειδήσεις το πρόγραμμα εκμάθησης"
    description="Μάθετε πώς να χρησιμοποιείτε Azure διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων μεταφρασμένα πρόσφατες ειδήσεις."
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

# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Χρήση διανομείς ειδοποίηση για την αποστολή μεταφρασμένα πρόσφατες ειδήσεις

> [AZURE.SELECTOR]
- [C# του Windows Store](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

##<a name="overview"></a>Επισκόπηση

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε τη δυνατότητα **πρότυπο** του Azure ειδοποίηση διανομείς μετάδοση πρόσφατες ειδήσεις ειδοποιήσεις που έχουν προσαρμοστεί, γλώσσα και τη συσκευή. Σε αυτό το πρόγραμμα εκμάθησης ξεκινάτε με την εφαρμογή Windows Store που έχουν δημιουργηθεί με [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις]. Όταν ολοκληρωθεί η εγκατάσταση, θα μπορείτε να εγγραφείτε για κατηγορίες που σας ενδιαφέρει, καθορίστε μια γλώσσα στην οποία θέλετε να λαμβάνετε τις ειδοποιήσεις και λαμβάνετε μόνο τις ειδοποιήσεις push για επιλεγμένες κατηγορίες στη συγκεκριμένη γλώσσα.


Υπάρχουν δύο μέρη σε αυτό το σενάριο:

- επιτρέπει την εφαρμογή Windows Store προγράμματος-πελάτη συσκευών, για να καθορίσετε μια γλώσσα, καθώς και για να εγγραφείτε για διαφορετικές πρόσφατες ειδήσεις κατηγορίες.

- το παρασκηνίου εκπέμπει τις ειδοποιήσεις, χρησιμοποιώντας την **ετικέτα** και **πρότυπο** feautres του Azure ειδοποίηση διανομείς.



##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Πρέπει να έχετε ήδη ολοκληρώσει το πρόγραμμα εκμάθησης [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις] και έχετε τον κώδικα που είναι διαθέσιμη, επειδή αυτό το πρόγραμμα εκμάθησης βασίζεται απευθείας σε αυτόν τον κώδικα.

Χρειάζεστε επίσης Visual Studio 2012 ή νεότερη έκδοση.


##<a name="template-concepts"></a>Πρότυπο έννοιες

Σε [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις] δημιουργηθεί μια εφαρμογή που χρησιμοποιούνται **ετικέτες** για να εγγραφείτε σε ειδοποιήσεις για ειδήσεις διαφορετικές κατηγορίες.
Πολλές εφαρμογές, ωστόσο, στοχεύστε πολλών αγορές και απαιτούν τοπικής προσαρμογής. Αυτό σημαίνει ότι το περιεχόμενο των ειδοποιήσεων τον εαυτό τους πρέπει να μεταφραστεί και παραδόθηκε στο σωστό σύνολο των συσκευών.
Σε αυτό το θέμα θα δείξουμε πώς μπορείτε να χρησιμοποιήσετε τη δυνατότητα **πρότυπο** της ειδοποίησης διανομείς για παράδοση εύκολα τις μεταφρασμένα πρόσφατες ειδήσεις ειδοποιήσεις.

Σημείωση: είναι ένας τρόπος για να στείλετε τις μεταφρασμένα ειδοποιήσεις για να δημιουργήσετε πολλές εκδόσεις του κάθε ετικέτα. Για παράδειγμα, για την υποστήριξη Αγγλικά, γαλλικά και Mandarin, θα πρέπει τρεις διαφορετικές ετικέτες για διεθνείς ειδήσεις: "world_en", "world_fr" και "world_ch". Θα σας, στη συνέχεια, θα πρέπει να στείλετε μια μεταφρασμένη έκδοση του τα νέα κόσμο σε κάθε μία από αυτές τις ετικέτες. Σε αυτό το θέμα, μπορούμε να χρησιμοποιήσουμε πρότυπα για να αποφύγετε τη διάδοση ετικέτες και την απαίτηση αποστολής πολλά μηνύματα.

Σε υψηλό επίπεδο, πρότυπα είναι ένας τρόπος για να καθορίσετε πώς μια συγκεκριμένη συσκευή θα λαμβάνουν μια ειδοποίηση. Το πρότυπο καθορίζει τη μορφή ακριβή φορτίο με αναφορά σε ιδιότητες που αποτελούν μέρος του μηνύματος που αποστέλλονται από το app υποστήριξης. Σε περίπτωση μας, θα στείλουμε ένα μήνυμα agnostic τοπικές ρυθμίσεις που περιέχει όλες τις υποστηριζόμενες γλώσσες:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Στη συνέχεια, θα σας εξασφαλίζει ότι οι συσκευές καταχώρηση με ένα πρότυπο που αναφέρεται στη σωστή ιδιότητα. Για παράδειγμα, μια εφαρμογή του Windows Store η οποία θέλει να λάβει ένα μήνυμα απλού αναδυόμενη θα καταχωρήσετε για το ακόλουθο πρότυπο με τις αντίστοιχες ετικέτες:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Τα πρότυπα είναι μια πολύ ισχυρή δυνατότητα που μπορείτε να μάθετε περισσότερα σχετικά με την στο άρθρο μας [πρότυπα](notification-hubs-templates-cross-platform-push-messages.md) . 


##<a name="the-app-user-interface"></a>Το περιβάλλον εργασίας χρήστη της εφαρμογής

Τώρα θα σας θα τροποποιήσετε την εφαρμογή διακοπή συζητήσεων που δημιουργήσατε στο θέμα [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις] για να στείλετε μεταφρασμένα πρόσφατες ειδήσεις με τη χρήση προτύπων.

Στην εφαρμογή του Windows Store:

Αλλαγή του MainPage.xaml για να συμπεριλάβετε ένα σύνθετο πλαίσιο τοπικές ρυθμίσεις:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

##<a name="building-the-windows-store-client-app"></a>Δημιουργία της εφαρμογής υπολογιστή-πελάτη του Windows Store

1. Στην τάξη σας ειδοποιήσεις, προσθέστε μια παράμετρο τοπικών ρυθμίσεων σε οι μέθοδοι *StoreCategoriesAndSubscribe* και *SubscribeToCateories* .

        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }

        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }

    Σημειώστε ότι αντί για κλήση της μεθόδου *RegisterNativeAsync* ονομάζουμε *RegisterTemplateAsync*: καταχώρηση σε μορφή συγκεκριμένες ειδοποίηση με την οποία το πρότυπο εξαρτάται από τις τοπικές ρυθμίσεις. Επίσης, παρέχουμε ένα όνομα για το πρότυπο ("localizedWNSTemplateExample"), επειδή μπορεί να θέλουμε να καταχωρήσετε περισσότερα από ένα πρότυπο (για παράδειγμα μία για τις ειδοποιήσεις αναδυόμενη) και μία για τα πλακίδια και πρέπει να του δώσετε ένα όνομα για να έχετε τη δυνατότητα να ενημερώσετε ή να διαγράψετε τους.

    Λάβετε υπόψη ότι εάν μια συσκευή καταγράφει πολλά πρότυπα με την ίδια ετικέτα, μια εισερχόμενη μήνυμα στόχευσης που οδηγούν σε πολλές ειδοποιήσεις ετικέτα παραδίδονται στη συσκευή (μία για κάθε πρότυπο). Αυτή η συμπεριφορά είναι χρήσιμη όταν το ίδιο μήνυμα λογική πρέπει να έχει ως αποτέλεσμα πολλές οπτικές ειδοποιήσεις, για παράδειγμα που εμφανίζει μια ένδειξη και μια αναδυόμενη σε μια εφαρμογή του Windows Store.

2. Προσθέστε την ακόλουθη μέθοδο για να ανακτήσετε τις τοπικές ρυθμίσεις αποθηκευμένες:

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. Στο MainPage.xaml.cs σας, το κουμπί ενημέρωσης κάντε κλικ στην επιλογή πρόγραμμα χειρισμού ανακτώντας την τρέχουσα τιμή του σύνθετου πλαισίου τοπικές ρυθμίσεις και παρέχοντας την στην κλήση για να την κλάση ειδοποιήσεις, όπως φαίνεται:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;

            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);

            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }


4. Τέλος, στο αρχείο σας App.xaml.cs, φροντίστε να ενημερώνετε το `InitNotificationsAsync` μέθοδο για να ανακτήσετε τις τοπικές ρυθμίσεις και να χρησιμοποιήσετε κατά την εγγραφή:

        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


##<a name="send-localized-notifications-from-your-back-end"></a>Αποστολή μεταφρασμένα ειδοποιήσεις από τις παρασκηνίου

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]






<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
