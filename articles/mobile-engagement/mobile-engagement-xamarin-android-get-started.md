<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure δέσμευση κινητές συσκευές για Xamarin.Android"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Mobile δέσμευση με ανάλυση και τις ειδοποιήσεις Push για τις εφαρμογές Xamarin.Android."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Γρήγορα αποτελέσματα με το Azure δέσμευση κινητές συσκευές για τις εφαρμογές Xamarin.Android

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Αυτό το θέμα σας δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure Mobile δέσμευση για να κατανοήσετε τις χρήσης εφαρμογής και πώς μπορείτε να στείλετε τις ειδοποιήσεις push σε τμηματική χρήστες μιας εφαρμογής Xamarin.Android.
Αυτό το πρόγραμμα εκμάθησης δείχνει το απλό σενάριο εκπομπής χρησιμοποιώντας δέσμευση Mobile. Σε αυτό, μπορείτε να δημιουργήσετε μια κενή εφαρμογή Xamarin.Android που συλλέγει βασικές δεδομένα και να λαμβάνει τις ειδοποιήσεις push χρησιμοποιώντας Google Cloud ανταλλαγής μηνυμάτων (GCM).

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ [Xamarin Studio](http://xamarin.com/studio). Μπορείτε επίσης να χρησιμοποιήσετε το Visual Studio με Xamarin, αλλά αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί Xamarin Studio. Για οδηγίες εγκατάστασης, ανατρέξτε στο θέμα [ρύθμιση και εγκατάσταση για Visual Studio και Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ [Κινητές συσκευές δέσμευση Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).

##<a id="setup-azme"></a>Ρύθμιση κινητών δέσμευση για την εφαρμογή σας Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Σύνδεση της εφαρμογής σας στον υπολογιστή στο παρασκήνιο Mobile δέσμευση

Αυτό το πρόγραμμα εκμάθησης παρουσιάζει μια "βασικές ολοκλήρωσης", που είναι το ελάχιστο σύνολο που απαιτείται για τη συλλογή δεδομένων και να στείλετε μια ειδοποίηση push. 

Θα δημιουργήσουμε μια βασική εφαρμογή με Xamarin Studio για μια επίδειξη την ενοποίηση.

###<a name="create-a-new-xamarinandroid-project"></a>Δημιουργήστε ένα νέο έργο Xamarin.Android

1. Εκκίνηση **Xamarin Studio** μεταβείτε στο **αρχείο** -> **νέα** -> **λύσης** 

    ![][1]

2. Επιλέξτε **Εφαρμογή Android** , στη συνέχεια, βεβαιωθείτε ότι είναι επιλεγμένη γλώσσα **C#** και κάντε κλικ στο κουμπί **Επόμενο**.

    ![][2]

3. Συμπληρώστε το **Όνομα εφαρμογής** και το **αναγνωριστικό εταιρείας**. Βεβαιωθείτε ότι στο σημάδι ελέγχου **Google Play υπηρεσιών** και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. 

    ![][3]
    
4. Ενημερώστε το **Όνομα του έργου**, **Όνομα λύσης** και τη **θέση** , εάν απαιτείται και κάντε κλικ στην επιλογή **Δημιουργία**.

    ![][4]
 
Xamarin Studio θα δημιουργήσει την εφαρμογή στην οποία θα σας θα ενοποίηση δέσμευση Mobile. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Σύνδεση της εφαρμογής σας στο κινητό δέσμευση παρασκηνίου

1. Κάντε δεξί κλικ στο φάκελο **πακέτων** στα παράθυρα λύσης και επιλέξτε **Προσθήκη πακέτων...**

    ![][5]

2. Αναζήτηση για το **Microsoft Azure Mobile δέσμευση Xamarin SDK** και προσθέστε το στη λύση σας.  

    ![][6]
   
3. Ανοίξτε το **MainActivity.cs** και προσθέστε το εξής χρήση προτάσεων:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. Στο το `OnCreate` μέθοδο, προσθέστε τα ακόλουθα προετοιμασίας της σύνδεσης με το κινητό δέσμευση παρασκηνίου. Βεβαιωθείτε ότι για να προσθέσετε το **συμβολοσειρά σύνδεσης**. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###<a name="add-permissions-and-a-service-declaration"></a>Προσθήκη δικαιωμάτων και μια δήλωση υπηρεσίας

1. Άνοιγμα του αρχείου **Manifest.xml** κάτω από το φάκελο ιδιότητες. Επιλέξτε καρτέλα προέλευση, έτσι ώστε να μπορείτε να ενημερώσετε απευθείας στην προέλευση XML.
 
2. Προσθέστε αυτά τα δικαιώματα για το Manifest.xml (το οποίο μπορείτε να βρείτε κάτω από το φάκελο **Ιδιότητες** ) του έργου σας αμέσως πριν ή μετά την `<application>` ετικέτα:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. Προσθέστε τα ακόλουθα μεταξύ του `<application>` και `</application>` ετικέτες για να δηλώσετε την υπηρεσία παράγοντα:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. Στον κώδικα που επικολλήσατε, αντικαταστήστε `"<Your application name>"` στην ετικέτα. Αυτό εμφανίζεται στο μενού " **Ρυθμίσεις** ", όπου οι χρήστες μπορούν να δουν οι υπηρεσίες που εκτελούνται στη συσκευή. Μπορείτε να προσθέσετε τη λέξη "Υπηρεσίας" σε αυτήν την ετικέτα για παράδειγμα.

###<a name="send-a-screen-to-mobile-engagement"></a>Αποστολή σε οθόνη στο κινητό δέσμευση

Για να ξεκινήσετε την αποστολή δεδομένων και ταυτόχρονα εξασφαλίζετε ότι οι χρήστες είναι ενεργό, πρέπει να στείλετε τουλάχιστον μία οθόνη στον υπολογιστή στο παρασκήνιο δέσμευση Mobile. Για αυτόν τον τρόπο-βεβαιωθείτε ότι το `MainActivity` μεταβιβάζονται από `EngagementActivity` αντί για `Activity`.

    public class MainActivity : EngagementActivity
    
Εναλλακτικά, εάν δεν είναι δυνατό να μεταβιβάζονται από `EngagementActivity` , στη συνέχεια, θα πρέπει να προσθέσετε `.StartActivity` και `.EndActivity` μεθόδων στο `OnResume` και `OnPause` αντίστοιχα.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a id="monitor"></a>Σύνδεση εφαρμογής με παρακολούθηση σε πραγματικό χρόνο

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ενεργοποίηση ειδοποιήσεων push και της εφαρμογής ανταλλαγής μηνυμάτων

Δέσμευση Mobile σάς επιτρέπει να αλληλεπιδρούν και ΕΠΊΤΕΥΞΗ τους χρήστες σας με τις ειδοποιήσεις push και την ανταλλαγή μηνυμάτων στο περιβάλλον της εκστρατείες της εφαρμογής. Η ενότητα αυτή ονομάζεται REACH στην πύλη του δέσμευση Mobile.
Οι παρακάτω ενότητες ρυθμίζει την εφαρμογή σας για τη λήψη τους.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
