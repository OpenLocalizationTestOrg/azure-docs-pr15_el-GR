<properties
    pageTitle="Προσθέστε τις ειδοποιήσεις push σας εφαρμογή Xamarin.Android | Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς να χρησιμοποιείτε Azure εφαρμογής υπηρεσίας και διανομείς Azure ειδοποίηση για την αποστολή ειδοποιήσεων push σε εφαρμογή Xamarin.Android"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Προσθέστε τις ειδοποιήσεις push σας εφαρμογή Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Επισκόπηση


Σε αυτό το πρόγραμμα εκμάθησης, προσθέτετε τις ειδοποιήσεις push στο έργο [Xamarin.Android Γρήγορη εκκίνηση](app-service-mobile-windows-store-dotnet-get-started.md) , έτσι ώστε να αποστέλλεται μια ειδοποίηση push στη συσκευή κάθε φορά που έχει εισαχθεί μια εγγραφή.

Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, θα χρειαστεί το πακέτο επέκτασης ειδοποιήσεων push. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .


##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ Ένα ενεργό λογαριασμό Google. Μπορείτε να εγγραφείτε για ένα λογαριασμό Google στο [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
+ [Google Cloud μηνυμάτων στοιχείο προγράμματος-πελάτη](http://components.xamarin.com/view/GCMClient/).

##<a name="configure-hub"></a>Ρύθμιση παραμέτρων διανομέα ειδοποίησης

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a id="register"></a>Ενεργοποίηση Firebase στο Cloud ανταλλαγή μηνυμάτων

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

##<a name="configure-azure-to-send-push-requests"></a>Ρύθμιση παραμέτρων του Azure για να στείλετε προσκλήσεις push

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

##<a id="update-server"></a>Ενημέρωση του project server για την αποστολή ειδοποιήσεων push

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>Ρύθμιση παραμέτρων του προγράμματος-πελάτη έργου για τις ειδοποιήσεις push

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Προσθήκη κώδικα ειδοποιήσεων push της εφαρμογής σας

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Ειδοποιήσεις push δοκιμής στην εφαρμογή

Μπορείτε να ελέγξετε την εφαρμογή, χρησιμοποιώντας μια εικονική συσκευή σε το προσομοίωσης. Υπάρχουν επιπλέον βήματα ρύθμισης παραμέτρων απαιτούνται όταν εκτελείται σε ένα προσομοίωσης.

1. Βεβαιωθείτε ότι για την ανάπτυξη στον ή τον εντοπισμό σφαλμάτων σε μια εικονική συσκευή που έχει APIs Google Ορισμός ως προορισμού, όπως φαίνεται παρακάτω στη Διαχείριση Android εικονική συσκευή (AVD).

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Προσθέστε ένα λογαριασμό Google στη συσκευή Android, κάνοντας κλικ στην επιλογή **εφαρμογές** > **Ρυθμίσεις** > **Προσθήκη λογαριασμού**, στη συνέχεια, ακολουθήστε τις οδηγίες.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Εκτελέστε την εφαρμογή todolist ως πριν από και εισαγάγετε ένα νέο στοιχείο todo. Αυτήν τη στιγμή, εμφανίζεται ένα εικονίδιο ειδοποίησης στην περιοχή ειδοποιήσεων. Μπορείτε να ανοίξετε το σχέδιο ειδοποίησης για να προβάλετε το πλήρες κείμενο της ειδοποίησης.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
