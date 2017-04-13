<properties 
    pageTitle="Windows Phone Silverlight SDK διαδικασίες αναβάθμισης" 
    description="Windows Phone Silverlight SDK διαδικασίες αναβάθμισης για Azure δέσμευση κινητές συσκευές"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK διαδικασίες αναβάθμισης

Εάν έχετε ήδη έχετε συνδέσει μια παλαιότερη έκδοση του SDK μας στην εφαρμογή σας, πρέπει να λάβετε υπόψη σας τα ακόλουθα σημεία κατά την αναβάθμιση του SDK.

Ίσως χρειαστεί να ακολουθήσετε διάφορες διαδικασίες, εάν έχετε παραβλέψει πολλές εκδόσεις του SDK. Για παράδειγμα, εάν κάνετε μετεγκατάσταση από 0.10.1 σε 0.11.0 πρέπει να πρώτα να ακολουθήστε τη διαδικασία "από 0.9.0 να 0.10.1", στη συνέχεια, τη διαδικασία "από 0.10.1 να 0.11.0".

##<a name="from-200-to-330"></a>Από το 2.0.0 να 3.3.0

### <a name="test-logs"></a>Αρχεία καταγραφής ελέγχου

Αρχεία καταγραφής κονσόλας που δημιουργήθηκαν με το SDK τώρα μπορεί να είναι ενεργοποιημένες/απενεργοποίησης/φιλτραρισμένη. Για να προσαρμόσετε έτσι, ενημερώστε την ιδιότητα `EngagementAgent.Instance.TestLogEnabled` σε μία από την τιμή που είναι διαθέσιμα από το `EngagementTestLogLevel` απαρίθμηση, για παράδειγμα:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

##<a name="from-111-to-200"></a>Από 1.1.1 να 2.0.0

Παρακάτω περιγράφεται ο τρόπος για να μετεγκαταστήσετε μια ενοποίηση SDK από την υπηρεσία Capptain που παρέχεται από Capptain συσχετισμούς Ασφαλείας σε μια εφαρμογή που υποστηρίζεται από Azure Mobile δέσμευση. 

> [Azure.IMPORTANT] Capptain και δέσμευση Mobile δεν είναι τις ίδιες υπηρεσίες και τη διαδικασία που ακολουθεί επισημαίνει μόνο με τη μετεγκατάσταση στην εφαρμογή υπολογιστή-πελάτη. Μετεγκατάσταση στο SDK στην εφαρμογή θα δεν μετεγκατάσταση των δεδομένων σας από τους διακομιστές Capptain με τους διακομιστές Mobile δέσμευση

Εάν κάνετε μετεγκατάσταση από μια προηγούμενη έκδοση, συμβουλευτείτε την τοποθεσία web Capptain για μετεγκατάσταση σε 1.1.1 πρώτα και, στη συνέχεια, εφαρμόστε την ακόλουθη διαδικασία

### <a name="nuget-package"></a>Το πακέτο Nuget

Αντικατάσταση **Capptain.WindowsPhone** από το πακέτο Nuget **MicrosoftAzure.MobileEngagement** .

### <a name="applying-mobile-engagement"></a>Εφαρμογή δέσμευση κινητές συσκευές

Το SDK χρησιμοποιεί τον όρο `Engagement`. Πρέπει να ενημερώσετε το έργο σας ώστε να ταιριάζει με αυτήν την αλλαγή.

Πρέπει να καταργήσετε την εγκατάσταση του τρέχοντος πακέτου nuget Capptain. Λάβετε υπόψη ότι θα καταργηθούν όλες οι αλλαγές σας στο φάκελο Capptain πόρους. Εάν θέλετε να διατηρήσετε αυτά τα αρχεία και, στη συνέχεια, δημιουργήστε ένα αντίγραφο του τους.

Μετά από αυτό, εγκαταστήστε το νέο πακέτο nuget Microsoft Azure δέσμευση στο έργο σας. Μπορείτε να το βρείτε απευθείας σε [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Αυτή η ενέργεια αντικαθιστά όλα τα αρχεία τους πόρους που χρησιμοποιούνται από δέσμευση και προσθέτει το νέο DLL δέσμευση για αναφορές του έργου σας.

Πρέπει να εκκαθαρίσετε αναφορές του έργου σας με τη διαγραφή Capptain DLL αναφορές. Εάν δεν κάνετε αυτό, θα υπάρχει διένεξη μεταξύ της έκδοσης του Capptain και θα συμβεί σφάλματα.

Εάν έχετε προσαρμόσει Capptain πόρους, αντιγράψτε το περιεχόμενό σας παλιά αρχεία και επικολλήστε τα στο τα νέα αρχεία δέσμευση. Σημειώστε ότι τα αρχεία xaml και στρατηγικής συνεργασίας ανά χώρα πρέπει να ενημερωθούν.

Όταν ολοκληρωθούν αυτά τα βήματα, έχετε μόνο για να αντικαταστήσετε το παλιό Capptain αναφορές από τις νέες αναφορές δέσμευση.

1. Όλοι οι χώροι ονομάτων Capptain πρέπει να ενημερωθούν.

    Πριν από την μετεγκατάσταση:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Μετά τη μετεγκατάσταση:
    
        using Microsoft.Azure.Engagement;

2. Όλες οι κλάσεις Capptain που περιέχουν "Capptain" πρέπει να περιέχει "Δέσμευση".

    Πριν από την μετεγκατάσταση:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Μετά τη μετεγκατάσταση:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Για τα αρχεία xaml Capptain χώρο ονομάτων και χαρακτηριστικά επίσης να αλλάξουν.

    Πριν από την μετεγκατάσταση:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Μετά τη μετεγκατάσταση:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Για τους άλλους πόρους όπως οι εικόνες Capptain, έχετε υπόψη ότι αυτά επίσης έχουν μετονομαστεί για να χρησιμοποιήσετε "Δέσμευση".

### <a name="application-id--sdk-key"></a>Αναγνωριστικό εφαρμογής / κλειδί SDK

Δέσμευση χρησιμοποιεί μια συμβολοσειρά σύνδεσης. Δεν χρειάζεται να καθορίσετε ένα Αναγνωριστικό εφαρμογής και ένας αριθμός-κλειδί SDK με δέσμευση Mobile, έχετε μόνο για να καθορίσετε μια συμβολοσειρά σύνδεσης. Μπορείτε να την ορίσετε στο αρχείο σας EngagementConfiguration.

Η ρύθμιση παραμέτρων δέσμευση μπορούν να ρυθμιστούν στο σας `Resources\EngagementConfiguration.xml` αρχείο του έργου σας.

Επεξεργαστείτε αυτό το αρχείο για να καθορίσετε:

-   Η συμβολοσειρά σύνδεσης εφαρμογής μεταξύ ετικετών `<connectionString>` και `<\connectionString>`.

Εάν θέλετε να καθορίσετε κατά το χρόνο εκτέλεσης, μπορείτε να καλέσετε την ακόλουθη μέθοδο πριν από την προετοιμασία παράγοντας δέσμευση:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

Η συμβολοσειρά σύνδεσης για την εφαρμογή σας εμφανίζεται στην πύλη κλασική Azure.

### <a name="items-name-change"></a>Αλλαγή του ονόματος στοιχείων

Όλα τα στοιχεία με το όνομα *capptain* έχετε ονομαστεί *Δέσμευση*. Ομοίως για *Capptain* *Δέσμευση*.

Παραδείγματα των στοιχείων που χρησιμοποιείται συχνά Capptain:

-   Τώρα που ονομάζεται EngagementConfiguration CapptainConfiguration
-   Τώρα που ονομάζεται EngagementAgent CapptainAgent
-   Τώρα που ονομάζεται EngagementReach CapptainReach
-   Τώρα που ονομάζεται EngagementHttpConfig CapptainHttpConfig
-   Τώρα που ονομάζεται GetEngagementPageName GetCapptainPageName

Σημειώστε ότι η μετονομασία επηρεάζει επίσης αντικατασταθεί μεθόδων.



 
