<properties
    pageTitle="Γρήγορα αποτελέσματα με το συμβάν διανομείς σε C# με καταιγίδας Apache | Microsoft Azure"
    description="Παρακολουθήστε αυτήν την εκμάθηση για να ξεκινήσετε να χρησιμοποιείτε διανομείς συμβάν Azure; Αποστολή συμβάντων στο C# και λήψη τους σε ένα σύμπλεγμα Apache καταιγίδας."
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
    ms.topic="article" 
    ms.date="09/06/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Γρήγορα αποτελέσματα με το συμβάν διανομείς

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Εισαγωγή

Διανομείς συμβάν είναι ένα σύστημα ιδιαίτερα μεταβλητού μεγέθους κατάποσης που μπορεί να πρόσληψη εκατομμύρια συμβάντων ανά δευτερόλεπτο, ενεργοποιώντας μια εφαρμογή για να επεξεργαστείτε και να αναλύσετε τα τεράστιες ποσότητες δεδομένων που παράγονται από τις συνδεδεμένες συσκευές και εφαρμογές. Μόλις συγκεντρώσει σε διανομείς συμβάντος, μπορείτε να μετασχηματισμός και αποθήκευση δεδομένων χρησιμοποιώντας οποιαδήποτε υπηρεσία παροχής σε πραγματικό χρόνο ανάλυση ή σύμπλεγμα χώρου αποθήκευσης.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα η [Επισκόπηση συμβάν διανομείς].

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς να ingest μηνύματα σε ένα συμβάν διανομέα, χρησιμοποιώντας μια εφαρμογή κονσόλας σε C# και να τις ανακτήσετε παράλληλα με χρήση Apache καταιγίδας.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης που θα χρειαστείτε τα εξής:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Ένα περιβάλλον ανάπτυξης Java έχει ρυθμιστεί για την εκτέλεση [Maven](http://maven.apache.org/). Για αυτό το πρόγραμμα εκμάθησης, θα υποθέσουμε ότι [Έκλειψη](https://www.eclipse.org/)

+ Λογαριασμού Azure active. <br/>Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν δωρεάν λογαριασμό σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure δωρεάν δοκιμαστικής έκδοσης</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1.  Εκτελέστε την κλάση **LogTopology** από Έκλειψη και, στη συνέχεια, περιμένετε για να ξεκινήσει η δέκτες για όλα τα διαμερίσματα.

2.  Εκτελέστε το έργο **αποστολέα** , πατήστε το πλήκτρο **Enter** στο παράθυρο της κονσόλας και δείτε τα συμβάντα που εμφανίζονται στο παράθυρο του παραλήπτη.

    ![][22]

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει μια εφαρμογή εργασία η οποία δημιουργεί ένα συμβάν διανομέα και στέλνει και λαμβάνει δεδομένων, μπορείτε να μετακινήσετε σε στα ακόλουθα σενάρια:

- Μια ολοκληρωμένη [δείγμα εφαρμογής που χρησιμοποιεί το συμβάν διανομείς][].
- Το δείγμα [κλίμακα ανάληψη συμβάν επεξεργασίας με διανομείς συμβάν][] .

<!-- Images. -->
[22]: ./media/event-hubs-csharp-storm-getstarted/receive-storm1.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Επισκόπηση διανομείς συμβάντων]: event-hubs-overview.md
[δείγμα εφαρμογής που χρησιμοποιεί το συμβάν διανομείς]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Διαβάθμιση της εκδήλωσης επεξεργασίας με διανομείς συμβάντος]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 