<properties
    pageTitle="Ρύθμιση παραμέτρων για το Windows καθολικής εφαρμογές δέσμευση SDK για προχωρημένους"
    description="Σύνθετες επιλογές ρύθμισης παραμέτρων για δέσμευση Mobile Azure με καθολική εφαρμογές των Windows"                    
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Ρύθμιση παραμέτρων για το Windows καθολικής εφαρμογές δέσμευση SDK για προχωρημένους

> [AZURE.SELECTOR]
- [Καθολική των Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Αυτή η διαδικασία περιγράφει πώς μπορείτε να ρυθμίσετε τις παραμέτρους διάφορες επιλογές ρύθμισης παραμέτρων για τις εφαρμογές του Azure Mobile δέσμευση Android.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

##<a name="advanced-configuration"></a>Ρύθμιση παραμέτρων για προχωρημένους

### <a name="disable-automatic-crash-reporting"></a>Απενεργοποίηση της αυτόματης σφάλμα αναφοράς

Μπορείτε να απενεργοποιήσετε την αυτόματη σφάλμα δυνατότητα της δέσμευσης αναφοράς. Στη συνέχεια, όταν παρουσιάζεται μια ανεπίλυτη εξαίρεση, δέσμευση δεν έχει κανένα αποτέλεσμα.

> [AZURE.WARNING] Εάν απενεργοποιήσετε αυτήν τη δυνατότητα, στη συνέχεια, όταν εμφανίζεται μια ανεπίλυτη σφάλμα στην εφαρμογή, δέσμευση δεν στέλνει το σφάλμα **και** δεν κλείσει η περίοδος λειτουργίας και οι εργασίες.

Για να απενεργοποιήσετε την αυτόματη σφάλμα αναφοράς, προσαρμογή της ρύθμισης παραμέτρων ανάλογα με τον τρόπο που έχουν δηλωθεί:

#### <a name="from-engagementconfigurationxml-file"></a>Από το `EngagementConfiguration.xml` αρχείου

Ορισμός αναφοράς σφαλμάτων για να `false` μεταξύ `<reportCrash>` και `</reportCrash>` ετικέτες.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Από το `EngagementConfiguration` αντικειμένου κατά το χρόνο εκτέλεσης

Ορισμός αναφοράς σφαλμάτων FALSE (ψευδής) χρησιμοποιώντας το αντικείμενο EngagementConfiguration.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Απενεργοποίηση της αναφοράς πραγματικό χρόνο

Από προεπιλογή, οι αναφορές εξυπηρέτησης δέσμευση καταγράφει σε πραγματικό χρόνο. Εάν η εφαρμογή σας αναφέρει αρχεία καταγραφής συχνά, είναι καλύτερα να buffer τα αρχεία καταγραφής και να αναφέρουν όλα ταυτόχρονα με βάση το κανονικό χρόνο. Αυτή η διαδικασία ονομάζεται "κατάσταση λειτουργίας καταιγισμού".

Για να το κάνετε αυτό, η κλήση της μεθόδου:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Το όρισμα είναι μια τιμή σε **χιλιοστά του δευτερολέπτου**. Κάθε φορά που θέλετε να ενεργοποιήσετε ξανά την καταγραφή σε πραγματικό χρόνο, καλέστε τη μέθοδο χωρίς παραμέτρους ή με την τιμή 0.

Κατάσταση λειτουργίας καταιγισμού ελαφρώς αυξάνεται ο χρόνος ζωής μπαταριών αλλά έχει επιπτώσεις στην οθόνη δέσμευση: όλες οι περίοδοι λειτουργίας και εργασιών διάρκεια στρογγυλοποιούνται με το όριο καταιγισμού (έτσι, περιόδους λειτουργίας και εργασίες που είναι μικρότερες από το όριο καταιγισμού ενδέχεται να μην είναι ορατά). Συνιστάται να χρησιμοποιείτε ένα όριο καταιγισμού δεν είναι πλέον από 30000 (30s). Αποθηκευμένα αρχεία καταγραφής περιορίζονται σε 300 στοιχεία. Εάν η αποστολή είναι πολύ μεγάλη, μπορεί να χάσετε ορισμένα αρχεία καταγραφής.

> [AZURE.WARNING] Το όριο καταιγισμού δεν μπορεί να ρυθμιστεί σε μια περίοδο μικρότερη από ένα δευτερόλεπτο. Εάν κάνετε κάτι τέτοιο, το SDK εμφανίζει μια ανίχνευση με το σφάλμα και επαναφέρει αυτόματα για την προεπιλεγμένη τιμή μηδέν δευτερόλεπτα. Αυτό εκκινεί το SDK για την αναφορά των αρχείων καταγραφής σε πραγματικό χρόνο.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
