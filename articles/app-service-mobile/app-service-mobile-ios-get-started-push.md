<properties
    pageTitle="Προσθήκη τις ειδοποιήσεις Push σε iOS εφαρμογής με εφαρμογές του Mobile Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε εφαρμογές του Mobile Azure για την αποστολή ειδοποιήσεων push σε εφαρμογή iOS."
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>Προσθέστε τις ειδοποιήσεις Push για την εφαρμογή iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Επισκόπηση
Σε αυτό το πρόγραμμα εκμάθησης, προσθέτετε τις ειδοποιήσεις push στο έργο [iOS Γρήγορη εκκίνηση] , έτσι ώστε να αποστέλλεται μια ειδοποίηση push στη συσκευή κάθε φορά που έχει εισαχθεί μια εγγραφή.

Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, θα χρειαστεί το πακέτο επέκτασης ειδοποιήσεων push. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

Το του [iOS simulator δεν υποστηρίζει τις ειδοποιήσεις προώθησης](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Χρειάζεστε μια συσκευή iOS φυσικής και ένα [πρόγραμμα για προγραμματιστές Apple συμμετοχή ως μέλος](https://developer.apple.com/programs/ios/).

##<a name="configure-hub"></a>Ρύθμιση παραμέτρων ειδοποιήσεων διανομέα

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Καταχώρηση εφαρμογής για τις ειδοποιήσεις push

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Ρύθμιση παραμέτρων του Azure για την αποστολή ειδοποιήσεων push

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>Ενημέρωση στοιχείου διακομιστή για την αποστολή ειδοποιήσεων push

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Προσθήκη ειδοποιήσεων push σε εφαρμογή

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Ειδοποιήσεις push δοκιμής

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>Περισσότερα

* Πρότυπα ευελιξία για την αποστολή ωθεί πλατφόρμες και μεταφρασμένα ωθεί. [Πώς μπορείτε να χρησιμοποιήστε iOS βιβλιοθήκη προγράμματος-πελάτη για εφαρμογές του Mobile Azure](app-service-mobile-ios-how-to-use-client-library.md#templates) δείχνει πώς μπορείτε να καταχωρήσετε τα πρότυπα.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS γρήγορης εκκίνησης]: app-service-mobile-ios-get-started.md
