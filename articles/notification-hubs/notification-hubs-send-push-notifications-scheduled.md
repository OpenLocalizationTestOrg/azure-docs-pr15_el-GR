<properties
    pageTitle="Πώς μπορείτε να στείλετε τις προγραμματισμένες ειδοποιήσεις | Microsoft Azure"
    description="Αυτό το θέμα περιγράφει τη χρήση ειδοποιήσεων που έχει προγραμματιστεί με Azure ειδοποίηση διανομείς."
    services="notification-hubs"
    documentationCenter=".net"
    keywords="ειδοποιήσεις Push, ειδοποιήσεων push, Προγραμματισμός ειδοποιήσεων push"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="how-to-send-scheduled-notifications"></a>Τρόπος: Αποστολή προγραμματισμένη ειδοποιήσεων


##<a name="overview"></a>Επισκόπηση

Εάν έχετε ένα σενάριο στο οποίο θέλετε να στείλετε μια ειδοποίηση σε κάποιο σημείο στο μέλλον, αλλά δεν έχετε ένας εύκολος τρόπος για να επαναφέρετε τον κωδικό παρασκηνίου για να στείλετε την ειδοποίηση προς τα επάνω. Τυπικό επίπεδο διανομείς ειδοποίηση υποστηρίζει μια δυνατότητα που σας επιτρέπει να προγραμματίζετε τις ειδοποιήσεις του έως 7 ημέρες στο μέλλον.

Όταν στέλνετε μια ειδοποίηση, απλώς να χρησιμοποιήσουμε την κλάση [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) στο SDK διανομείς ειδοποίηση όπως φαίνεται στο ακόλουθο παράδειγμα:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Επίσης, μπορείτε να ακυρώσετε μια ειδοποίηση έχει προγραμματιστεί προηγουμένως χρησιμοποιώντας το notificationId:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Δεν υπάρχουν όρια στον αριθμό των προγραμματισμένων ειδοποιήσεις, μπορείτε να στείλετε.