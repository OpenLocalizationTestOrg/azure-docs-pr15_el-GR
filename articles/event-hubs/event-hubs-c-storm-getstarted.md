<properties
    pageTitle="Γρήγορα αποτελέσματα με το συμβάν διανομείς με C και καταιγίδας Apache | Microsoft Azure"
    description="Παρακολουθήστε αυτήν την εκμάθηση για να ξεκινήσετε να χρησιμοποιείτε διανομείς συμβάν Azure; Αποστολή συμβάντα στο C και λήψη τους σε ένα σύμπλεγμα Apache καταιγίδας."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="java"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Γρήγορα αποτελέσματα με το συμβάν διανομείς

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Εισαγωγή

Διανομείς συμβάν είναι ένα σύστημα ιδιαίτερα μεταβλητού μεγέθους κατάποσης που μπορεί να πρόσληψη εκατομμύρια συμβάντων ανά δευτερόλεπτο, ενεργοποιώντας μια εφαρμογή για να επεξεργαστείτε και να αναλύσετε τα τεράστιες ποσότητες δεδομένων που παράγονται από τις συνδεδεμένες συσκευές και εφαρμογές. Μόλις συγκεντρώσει σε διανομείς συμβάντος, μπορείτε να μετασχηματισμός και αποθήκευση δεδομένων χρησιμοποιώντας οποιαδήποτε υπηρεσία παροχής σε πραγματικό χρόνο ανάλυση ή σύμπλεγμα χώρου αποθήκευσης.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση συμβάν διανομείς].

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς να ingest μηνύματα σε ένα συμβάν διανομέα, χρησιμοποιώντας μια εφαρμογή κονσόλας στο C και να τις ανακτήσετε παράλληλα με χρήση Apache καταιγίδας.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, θα χρειαστείτε τα εξής:

+ Ένα περιβάλλον ανάπτυξης C. Για αυτό το πρόγραμμα εκμάθησης, θα υποθέσουμε ότι το πρόγραμμα μεταγλώττισης gcc στοίβας σε μια [Εικονική Linux Azure](../virtual-machines/virtual-machines-linux-quick-create-cli.md) με Ubuntu 14.04. Οδηγίες για άλλα περιβάλλοντα θα παρέχονται σε εξωτερικές συνδέσεις.

+ Ένα περιβάλλον ανάπτυξης Java έχει ρυθμιστεί για την εκτέλεση [Maven](http://maven.apache.org/). Για αυτό το πρόγραμμα εκμάθησης, θα υποθέσουμε ότι [Έκλειψη](https://www.eclipse.org/).

+ Λογαριασμού Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν δωρεάν λογαριασμό σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1.  Εκτελέστε την κλάση **LogTopology** από Έκλειψη και, στη συνέχεια, περιμένετε για να ξεκινήσει η δέκτες για όλα τα διαμερίσματα.

2.  Εκτέλεση του προγράμματος **αποστολέα** και δείτε τα συμβάντα που εμφανίζονται στο παράθυρο του παραλήπτη.

    ![][23]

> [AZURE.NOTE] Σε αυτό το πρόγραμμα εκμάθησης μόνο, χρησιμοποιήστε καταιγίδας σε τοπική λειτουργία για σκοπούς ανάπτυξης. Ανατρέξτε στην [Επισκόπηση καταιγίδας HDInsight] και την επίσημη [Καταιγίδας Apache] τεκμηρίωση για περισσότερες πληροφορίες αναπτύξεις καταιγίδας και μοτίβα.

## <a name="next-steps"></a>Επόμενα βήματα

Οι ακόλουθοι πόροι είναι διαθέσιμοι για την ανάπτυξη εφαρμογών ενοποίηση διανομείς συμβάντων και καταιγίδας.

- [Την ανάλυση δεδομένων αισθητήρα με καταιγίδας και HDInsight][] είναι ένα πρόγραμμα εκμάθησης πλήρους σενάριο χρησιμοποιώντας διανομείς συμβάντος, καταιγίδας και HBase να ingest αισθητήρα δεδομένων σε ένα σύμπλεγμα Hadoop.
- [Ανάπτυξη ροή εφαρμογές επεξεργασίας δεδομένων με SCP.NET και C# καταιγίδας και HDInsight][] είναι ένα πρόγραμμα εκμάθησης σχετικά με τον τρόπο για να γράψετε αγωγούς καταιγίδας χρησιμοποιώντας C#.

<!-- Images. -->
[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Επισκόπηση διανομείς συμβάντων]: event-hubs-overview.md

[Apache καταιγίδας]: https://storm.incubator.apache.org
[Επισκόπηση HDInsight καταιγίδας]: ../hdinsight/hdinsight-storm-overview.md/
[Την ανάλυση δεδομένων αισθητήρα με καταιγίδας και HDInsight]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Ανάπτυξη εφαρμογών επεξεργασίας δεδομένων με SCP.NET και C# καταιγίδας και HDInsight ροής]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
