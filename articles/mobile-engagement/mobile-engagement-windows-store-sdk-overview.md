<properties
    pageTitle="Ενοποίηση καθολικής SDK των Windows"
    description="Καθολική ενοποίηση των Windows για SDK για Azure δέσμευση κινητές συσκευές"                                     
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

#<a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Ενοποίηση καθολικής SDK των Windows για Azure δέσμευση κινητές συσκευές

Αυτό το έγγραφο περιγράφει όλες τις ενοποίηση και τη ρύθμιση παραμέτρων διαθέσιμες επιλογές για το SDK καθολικής Windows Azure Mobile δέσμευση.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Πριν να ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να ολοκληρώσετε πρώτα το [πρόγραμμα εκμάθησης διάρκειας 15 λεπτών](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Δυνατότητες για προχωρημένους

### <a name="reporting-features"></a>Δυνατότητες αναφοράς
Μπορείτε να προσθέσετε αυτές τις δυνατότητες:

1. [Δημιουργία αναφορών επιλογές για προχωρημένους](mobile-engagement-windows-store-advanced-reporting.md)

2. [Επιλογές ρύθμιση παραμέτρων για προχωρημένους](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Ειδοποιήσεις

[Πώς μπορείτε να ενοποιήσετε Reach (ειδοποιήσεις) στο Windows καθολική εφαρμογή σας](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Ετικέτα πρόγραμμα εφαρμογής:

[Πώς μπορείτε να χρησιμοποιήσετε το για προχωρημένους δέσμευση Mobile η εφαρμογή ετικετών API στο Windows καθολική εφαρμογή σας](mobile-engagement-windows-store-use-engagement-api.md)

##<a name="release-notes"></a>Σημειώσεις έκδοσης

###<a name="340-04192016"></a>3.4.0 (19/04/2016)

-   Επίτευξη βελτιώσεις επικάλυψης.
-   Προστέθηκε "TestLogLevel" API για ενεργοποίηση/απενεργοποίηση φίλτρου κονσόλα αρχεία καταγραφής που εκπέμπει το SDK.
-   Ξεκινήστε το σταθερό στη δραστηριότητα ειδοποιήσεις στόχευσης η πρώτη δραστηριότητα δεν εμφανίζονται στην εφαρμογή.

Για παλαιότερες εκδόσεις, ανατρέξτε στις [Σημειώσεις έκδοσης ολοκλήρωσης](mobile-engagement-windows-store-release-notes.md)

##<a name="upgrade-procedures"></a>Διαδικασίες αναβάθμισης

Εάν έχετε ήδη έχετε συνδέσει μια παλαιότερη έκδοση του δέσμευση στην εφαρμογή σας, πρέπει να λάβετε υπόψη σας τα ακόλουθα σημεία κατά την αναβάθμιση του SDK.

Εάν έχετε παραβλέψει πολλές εκδόσεις του SDK, ίσως χρειαστεί να ακολουθήσετε διάφορες διαδικασίες. Δείτε την πλήρη [Αναβάθμιση διαδικασίες](mobile-engagement-windows-store-upgrade-procedure.md). Για παράδειγμα, εάν κάνετε μετεγκατάσταση από 0.10.1 σε 0.11.0 πρέπει να πρώτα να ακολουθήστε τη διαδικασία "από 0.9.0 να 0.10.1", στη συνέχεια, τη διαδικασία "από 0.10.1 να 0.11.0".

###<a name="from-330-to-340"></a>Από το 3.3.0 να 3.4.0

####<a name="test-logs"></a>Αρχεία καταγραφής ελέγχου

Αρχεία καταγραφής κονσόλας που δημιουργήθηκαν με το SDK τώρα μπορεί να είναι ενεργοποιημένες/απενεργοποίησης/φιλτραρισμένη. Για να προσαρμόσετε, ενημερώστε την ιδιότητα `EngagementAgent.Instance.TestLogEnabled` σε μία από τις τιμές που είναι διαθέσιμα από το `EngagementTestLogLevel` απαρίθμηση, για παράδειγμα:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

####<a name="resources"></a>Πόροι

Έχουν βελτιωθεί Reach επικάλυψης. Είναι μέρος οι πόροι του πακέτου SDK NuGet.

Κατά την αναβάθμιση στη νέα έκδοση του SDK, μπορείτε να επιλέξετε εάν θέλετε να διατηρήσετε τα υπάρχοντα αρχεία από το φάκελο επικάλυψη τους πόρους σας ή όχι:

* Εάν το προηγούμενο επικάλυψη εργάζεται για εσάς ή που ενοποίηση του `WebView` στοιχεία με μη αυτόματο τρόπο, στη συνέχεια, μπορείτε να αποφασίσετε να διατηρήσετε την έξοδο αρχεία, αυτό θα εξακολουθούν να λειτουργούν.
* Για να ενημερώσετε το νέο επικάλυψη, αντικαταστήστε το σύνολο `overlay` φάκελο από τους πόρους σας με τη νέα από το πακέτο SDK (UWP εφαρμογές: μετά την αναβάθμιση, μπορείτε να λάβετε τον νέο φάκελο επικάλυψη από % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Χρήση της νέας επικάλυψης αντικαθιστούν οι προσαρμογές που έγιναν από την προηγούμενη έκδοση.

### <a name="upgrade-from-older-versions"></a>Αναβάθμιση από παλαιότερες εκδόσεις

Ανατρέξτε στο θέμα [διαδικασίες αναβάθμισης](mobile-engagement-windows-store-upgrade-procedure.md)
