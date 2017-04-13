<properties
    pageTitle="Γρήγορα αποτελέσματα με το συμβάν διανομείς σε C# | Microsoft Azure"
    description="Παρακολουθήστε αυτήν την εκμάθηση για να ξεκινήσετε να χρησιμοποιείτε διανομείς συμβάν Azure; Αποστολή συμβάντα σε C# και λήψη τους σε Java χρησιμοποιώντας το EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Γρήγορα αποτελέσματα με το συμβάν διανομείς

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Εισαγωγή

Διανομείς συμβάν είναι μια υπηρεσία που επεξεργάζεται μεγάλες ποσότητες δεδομένων συμβάντων (τηλεμετρίας) από συνδεδεμένες συσκευές και εφαρμογές. Μετά τη συλλογή δεδομένων σε διανομείς συμβάντος, μπορείτε να αποθηκεύσετε τα δεδομένα χρησιμοποιώντας ένα σύμπλεγμα χώρου αποθήκευσης ή Μετασχηματισμός χρησιμοποιώντας μια υπηρεσία παροχής σε πραγματικό χρόνο ανάλυση. Η δυνατότητα συλλογή και επεξεργασία αυτό ευρείας κλίμακας συμβάν είναι βασικό στοιχείο του αρχιτεκτονικές σύγχρονη εφαρμογή όπως το Internet πράγματα (IoT).

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε το Azure κλασική πύλη για να δημιουργήσετε ένα συμβάν διανομέα. Σας επίσης δείχνει πώς μπορείτε να συλλέξετε μηνύματα σε ένα συμβάν διανομέα, χρησιμοποιώντας μια εφαρμογή κονσόλας γραμμένη σε C# και πώς μπορείτε να τις ανακτήσετε παράλληλες χρησιμοποιώντας τη βιβλιοθήκη του κεντρικού υπολογιστή επεξεργαστής συμβάντων Java.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, θα χρειαστείτε τα εξής:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Λογαριασμού Azure active. <br/>Εάν δεν έχετε, μπορείτε να δημιουργήσετε έναν δωρεάν λογαριασμό σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1.  Εκτελέστε το **ακουστικό** έργο.

    ![][21]

2.  Εκτελέστε το έργο **αποστολέα** .

    ![][22]

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει μια εφαρμογή εργασία η οποία δημιουργεί ένα συμβάν διανομέα και στέλνει και λαμβάνει δεδομένων, μπορείτε να μετακινήσετε σε στα ακόλουθα σενάρια:

- Μια ολοκληρωμένη [δείγμα εφαρμογής που χρησιμοποιεί το συμβάν διανομείς][].
- Το δείγμα [κλίμακα ανάληψη συμβάν επεξεργασίας με διανομείς συμβάν][] .
- [Επισκόπηση διανομείς συμβάντων][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Επισκόπηση διανομείς συμβάντων]: event-hubs-overview.md
[δείγμα εφαρμογής που χρησιμοποιεί το συμβάν διανομείς]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Διαβάθμιση της εκδήλωσης επεξεργασίας με διανομείς συμβάντος]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
