<properties
    pageTitle="Γρήγορα αποτελέσματα με το συμβάν διανομείς στο C και C# | Microsoft Azure"
    description="Παρακολουθήστε αυτήν την εκμάθηση για να ξεκινήσετε να χρησιμοποιείτε διανομείς συμβάν Azure; Αποστολή συμβάντα στο C και λαμβάνετε hem στο C# χρησιμοποιώντας το EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="csharp"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Γρήγορα αποτελέσματα με το συμβάν διανομείς

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Εισαγωγή

Διανομείς συμβάν είναι ένα σύστημα ιδιαίτερα μεταβλητού μεγέθους κατάποσης που μπορεί να πρόσληψη εκατομμύρια συμβάντων ανά δευτερόλεπτο, ενεργοποιώντας μια εφαρμογή για να επεξεργαστείτε και να αναλύσετε τα τεράστιες ποσότητες δεδομένων που παράγονται από τις συνδεδεμένες συσκευές και εφαρμογές. Μόλις συγκεντρώσει σε διανομείς συμβάντος, μπορείτε να μετασχηματισμός και αποθήκευση δεδομένων χρησιμοποιώντας οποιαδήποτε υπηρεσία παροχής σε πραγματικό χρόνο ανάλυση ή σύμπλεγμα χώρου αποθήκευσης.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση διανομείς συμβάντων][].

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς να ingest μηνύματα σε ένα συμβάν διανομέα, χρησιμοποιώντας μια εφαρμογή κονσόλας στο C και να τις ανακτήσετε παράλληλες χρησιμοποιώντας τη βιβλιοθήκη C# [Host επεξεργαστής συμβάν][] .

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, θα χρειαστείτε τα εξής:

+ Ένα περιβάλλον ανάπτυξης C. Για αυτό το πρόγραμμα εκμάθησης, θα υποθέσουμε ότι το πρόγραμμα μεταγλώττισης gcc στοίβας σε μια [Εικονική Linux Azure](../virtual-machines/virtual-machines-linux-quick-create-cli.md) με Ubuntu 14.04. Οδηγίες για άλλα περιβάλλοντα θα παρέχονται σε εξωτερικές συνδέσεις.

+ Microsoft Visual Studio Express για Windows

+ Λογαριασμού Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1.  Εκτελέστε το **ακουστικό** έργο από το Visual Studio και, στη συνέχεια, περιμένετε για να ξεκινήσει η δέκτες για όλα τα διαμερίσματα.

    ![][21]

2.  Εκτέλεση του προγράμματος **αποστολέα** και δείτε τα συμβάντα που εμφανίζονται στο παράθυρο του παραλήπτη.

    ![][24]

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει μια εφαρμογή εργασία η οποία δημιουργεί ένα συμβάν διανομέα και στέλνει και λαμβάνει δεδομένων, μπορείτε να μετακινήσετε σε στα ακόλουθα σενάρια:

- Μια ολοκληρωμένη [δείγμα εφαρμογής που χρησιμοποιεί το συμβάν διανομείς][].
- Το δείγμα [κλίμακα ανάληψη συμβάν επεξεργασίας με διανομείς συμβάν][] .
- [Επισκόπηση διανομείς συμβάντων][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Κεντρικός υπολογιστής επεξεργαστής συμβάντος]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Επισκόπηση διανομείς συμβάντων]: event-hubs-overview.md
[δείγμα εφαρμογής που χρησιμοποιεί το συμβάν διανομείς]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Διαβάθμιση της εκδήλωσης επεξεργασίας με διανομείς συμβάντος]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
