<properties
    pageTitle="Δρομολόγηση και παραστάσεις ετικέτας"
    description="Αυτό το θέμα εξηγεί τη δρομολόγηση και ετικέτα παραστάσεις για διανομείς Azure ειδοποίησης."
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

# <a name="routing-and-tag-expressions"></a>Δρομολόγηση και ετικέτα παραστάσεις

##<a name="overview"></a>Επισκόπηση

Ετικέτα παραστάσεις σάς επιτρέπουν να προορισμού συγκεκριμένα σύνολα συσκευές, ή πιο συγκεκριμένα εγγραφές, όταν στέλνετε μια ειδοποίηση push μέσω ειδοποίησης διανομείς.


## <a name="targeting-specific-registrations"></a>Στόχευση συγκεκριμένων καταχωρήσεων

Ο μόνος τρόπος να προορισμού συγκεκριμένες ειδοποίηση καταχωρήσεων είναι να συσχετίσετε ετικέτες με αυτά, στοχεύστε αυτές τις ετικέτες. Όπως περιγράφεται στη [Διαχείριση εγγραφής](notification-hubs-push-notification-registration-management.md), προκειμένου να λαμβάνουν ειδοποιήσεις προώθησης μια εφαρμογή έχει για να καταχωρήσετε μια λαβή συσκευή σε διανομέα ειδοποίησης. Όταν δημιουργηθεί μια καταχώρηση στην ειδοποίηση διανομέα, υπόβαθρο εφαρμογής να στείλετε τις ειδοποιήσεις push σε αυτήν.
Η εφαρμογή παρασκηνίου για να επιλέξετε τις καταχωρήσεις για στόχευση με μια συγκεκριμένη ειδοποίηση με τους εξής τρόπους:

1. **Εκπομπή**: όλες οι εγγραφές στην ενότητα ειδοποίηση λαμβάνουν την ειδοποίηση.
2. **Ετικέτα**: όλες οι εγγραφές που περιέχουν την καθορισμένη ετικέτα λαμβάνουν την ειδοποίηση.
3. **Έκφραση ετικέτας**: όλες οι εγγραφές των οποίων σύνολο των ετικετών ταιριάζει με την καθορισμένη έκφραση λαμβάνουν την ειδοποίηση.

## <a name="tags"></a>Ετικέτες

Μια ετικέτα μπορεί να είναι οποιαδήποτε συμβολοσειρά, έως 120 χαρακτήρες, που περιέχει αλφαριθμητικά και τους ακόλουθους μη αλφαριθμητικούς χαρακτήρες: '_', ‘@’, '#', '. ',':', '-'. Το παρακάτω παράδειγμα εμφανίζει μια εφαρμογή από την οποία μπορείτε να λάβετε ειδοποιήσεις αναδυόμενη σχετικά με τις συγκεκριμένες μουσικής ομάδες. Σε αυτό το σενάριο, έναν απλό τρόπο για τις ειδοποιήσεις δρομολόγηση είναι να καταχωρήσεων ετικέτας με ετικέτες που αντιπροσωπεύουν τις διαφορετικές ζώνες, όπως η παρακάτω εικόνα.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

Σε αυτήν την εικόνα, το μήνυμα με ετικέτες **Beatles** φτάσει μόνο τα tablet που έχει καταχωρηθεί με την ετικέτα **Beatles**.

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία καταχωρήσεων για ετικέτες, ανατρέξτε στο θέμα [Διαχείριση εγγραφής](notification-hubs-push-notification-registration-management.md).

Μπορείτε να στείλετε ειδοποιήσεις σε ετικέτες χρησιμοποιώντας τις μεθόδους αποστολή ειδοποιήσεων από το `Microsoft.Azure.NotificationHubs.NotificationHubClient` τάξης στο [Microsoft Azure ειδοποίηση διανομείς](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Μπορείτε επίσης να χρησιμοποιήσετε Node.js ή τα API ΥΠΌΛΟΙΠΑ ειδοποιήσεις Push.  Ακολουθεί ένα παράδειγμα, χρησιμοποιώντας το SDK.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Οι ετικέτες δεν χρειάζεται να είναι προ-προμήθεια του φακέλου και μπορεί να αναφέρεται σε πολλές έννοιες συγκεκριμένης εφαρμογής. Για παράδειγμα, οι χρήστες αυτής της εφαρμογής παράδειγμα μπορεί να σχολιάσετε ζώνες και θέλετε να λαμβάνετε toasts, όχι μόνο για τα σχόλια σχετικά με τις αγαπημένες ζώνες, αλλά και για όλα τα σχόλια από τους φίλους, ανεξάρτητα από τη ζώνη στην οποία σχολιασμό. Η παρακάτω εικόνα εμφανίζει ένα παράδειγμα αυτό το σενάριο:



![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Σε αυτήν την εικόνα, η Άννα ενδιαφέρεται ενημερωμένες εκδόσεις για το Beatles και ο Μπάμπης ενδιαφέρεται ενημερωμένες εκδόσεις για το Wailers. Ο Μπάμπης είναι επίσης ενδιαφέρει του Κώστας σχόλια και Κώστας είναι στο θέλατε να το Wailers. Όταν αποστέλλεται μια ειδοποίηση για του Κώστας σχόλιο σχετικά με το Beatles, η Άννα και ο Μπάμπης λαμβάνετε αυτό.

Ενώ μπορείτε να κωδικοποιήσετε πολλών ανησυχίες σε ετικέτες (για παράδειγμα, "band_Beatles" ή "follows_Charlie"), οι ετικέτες είναι απλή συμβολοσειρές και όχι ιδιότητες με τιμές. Μια καταχώρηση είναι αντιστοιχισμένος μόνο από την παρουσία ή απουσία από μια συγκεκριμένη ετικέτα.

Για ένα πρόγραμμα εκμάθησης πλήρη βήμα προς βήμα σχετικά με τη χρήση ετικετών για την αποστολή σε ομάδες που σας ενδιαφέρουν, ανατρέξτε στο θέμα [Διακοπή συζητήσεων](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).


## <a name="using-tags-to-target-users"></a>Χρήση ετικετών στους χρήστες προορισμού

Ένας άλλος τρόπος για να χρησιμοποιήσετε ετικέτες είναι για τον προσδιορισμό όλες τις συσκευές ενός συγκεκριμένου χρήστη. Καταχωρήσεις μπορεί να φέρουν ετικέτα με μια ετικέτα που περιέχει ένα αναγνωριστικό χρήστη, όπως η παρακάτω εικόνα:


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Σε αυτήν την εικόνα, το μήνυμα ετικέτα uid:Alice φτάσει όλες οι εγγραφές με ετικέτες uid:Alice; Επομένως, όλες τις συσκευές η Άννα του.


##<a name="tag-expressions"></a>Ετικέτα παραστάσεις

Υπάρχουν περιπτώσεις στις οποίες έχει μια ειδοποίηση για να στοχεύετε σε ένα σύνολο εγγραφών που προσδιορίζεται δεν από μια μεμονωμένη ετικέτα, αλλά με μια δυαδική παράσταση σε ετικέτες.

Εξετάστε το ενδεχόμενο μια εφαρμογή sports που στέλνει μια υπενθύμιση για όλους τους χρήστες στον τοπικό σχετικά με ένα παιχνίδι ανάμεσα στο κόκκινο Sox και Cardinals. Εάν η εφαρμογή προγράμματος-πελάτη καταχωρεί ετικετών για ενδιαφέρον για τις ομάδες και τη θέση, στη συνέχεια, θα πρέπει να είναι στοχευμένες την ειδοποίηση για όλους τους χρήστες στον τοπικό που σας ενδιαφέρουν την Sox κόκκινο χρώμα ή το Cardinals είναι. Αυτή η συνθήκη να εκφράζονται με την ακόλουθη παράσταση Boolean:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Ετικέτα παραστάσεις μπορούν να περιέχουν όλες οι τελεστές Boolean, όπως και (& &), ή (||), και ΌΧΙ (!). Μπορούν επίσης να περιέχουν παρενθέσεις. Οι παραστάσεις ετικέτα είναι περιορίζεται σε ετικέτες 20 εάν περιέχουν μόνο ORs; Διαφορετικά έχουν περιορίζεται σε ετικέτες 6.

Ακολουθεί ένα παράδειγμα για την αποστολή ειδοποιήσεων με χρήση του SDK παραστάσεις ετικέτα.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)"; 

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
