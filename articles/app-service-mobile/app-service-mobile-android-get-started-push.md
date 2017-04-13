<properties
    pageTitle="Προσθήκη ειδοποιήσεων Push σε εφαρμογή Android με το Azure εφαρμογές για κινητές συσκευές"
    description="Μάθετε πώς να χρησιμοποιείτε εφαρμογές του Mobile Azure για την αποστολή ειδοποιήσεων push για την εφαρμογή σας Android."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Προσθέστε τις ειδοποιήσεις Push σας Android εφαρμογή

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Επισκόπηση
Σε αυτό το πρόγραμμα εκμάθησης, προσθέτετε τις ειδοποιήσεις push το έργο [Android γρήγορης εκκίνησης] ώστε να αποστέλλεται μια ειδοποίηση push στη συσκευή κάθε φορά που έχει εισαχθεί μια εγγραφή.

Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, χρειάζεστε το πακέτο επέκτασης ειδοποιήσεων push. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Χρειάζεστε τα εξής:

* IDE ανάλογα με το παρασκηνίου του έργου σας:

    * [Android Studio](https://developer.android.com/sdk/index.html) Εάν αυτή η εφαρμογή έχει έναν υπολογιστή στο παρασκήνιο Node.js.

    * [Visual Studio Κοινότητας 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) ή νεότερη έκδοση, εάν αυτό εφαρμογή έχει έναν υπολογιστή στο παρασκήνιο .net.

* Android 2.3 ή νεότερη έκδοση, αποθετήριο Google αναθεώρησης 27 ή νεότερη έκδοση και τις υπηρεσίες Google Play 9.0.2 ή νεότερη έκδοση για Firebase Cloud μηνυμάτων.

* Ολοκλήρωση [Android γρήγορης εκκίνησης].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Δημιουργήστε ένα έργο που υποστηρίζει Firebase Cloud μηνυμάτων

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Ρύθμιση παραμέτρων διανομέα ειδοποίησης

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Ρύθμιση παραμέτρων του Azure για την αποστολή ειδοποιήσεων push

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Ενεργοποίηση ειδοποιήσεων push για το project server

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Προσθέστε τις ειδοποιήσεις push για την εφαρμογή σας

Σε αυτήν την ενότητα, μπορείτε να ενημερώσετε την εφαρμογή Android προγράμματος-πελάτη για να χειριστείτε τις ειδοποιήσεις προώθησης.

### <a name="verify-android-sdk-version"></a>Επαλήθευση έκδοσης Android SDK

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Το επόμενο βήμα είναι να εγκαταστήσετε το Google Play υπηρεσίες. Google Cloud μηνυμάτων έχει ορισμένες ελάχιστο API επιπέδου απαιτήσεις για ανάπτυξη και δοκιμές, όπου η ιδιότητα **minSdkVersion** στο δηλωτικό πρέπει να συμφωνεί με.

Εάν κάνετε δοκιμές με μια παλαιότερη συσκευή, στη συνέχεια, συμβουλευτείτε [Ορισμός του Google Play υπηρεσίες SDK] για να καθορίσετε πώς μικρό μπορείτε να ορίσετε αυτήν την τιμή, και να ρυθμίσετε σωστά.

### <a name="add-google-play-services-to-the-project"></a>Προσθήκη υπηρεσιών αναπαραγωγή Google στο έργο

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Προσθήκη κώδικα

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Δοκιμή της εφαρμογής σε σχέση με το δημοσιευμένο υπηρεσία κινητών τηλεφώνων

Μπορείτε να ελέγξετε την εφαρμογή επισυνάπτοντας απευθείας τηλέφωνο Android με το καλώδιο USB ή χρησιμοποιώντας μια εικονική συσκευή το προσομοίωσης.

## <a name="more"></a>Περισσότερα

<!-- URLs -->
[Android γρήγορης εκκίνησης]: app-service-mobile-android-get-started.md

[Ρύθμιση υπηρεσιών Google Play SDK]:https://developers.google.com/android/guides/setup
