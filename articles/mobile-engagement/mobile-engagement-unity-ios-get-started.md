<properties
    pageTitle="Γρήγορα αποτελέσματα με δέσμευση Mobile Azure για ανάπτυξη iOS ενότητας"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Mobile δέσμευση με ανάλυση και τις ειδοποιήσεις Push για τις εφαρμογές ενότητας αναπτύξετε για συσκευές iOS."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Γρήγορα αποτελέσματα με δέσμευση Mobile Azure για ανάπτυξη iOS ενότητας

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure Mobile δέσμευση για να κατανοήσετε τις χρήσης εφαρμογής και πώς μπορείτε να στείλετε τις ειδοποιήσεις push για να φέρουν κατά διαστήματα τους χρήστες μιας εφαρμογής ενότητας κατά την ανάπτυξη σε συσκευή iOS.
Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί την κλασική λήψη ενότητας ένα πρόγραμμα εκμάθησης μπάλα ως σημείο εκκίνησης. Θα πρέπει να ακολουθήσετε τα βήματα σε αυτό [το πρόγραμμα εκμάθησης](mobile-engagement-unity-roll-a-ball.md) , πριν να συνεχίσετε με την ενοποίηση δέσμευση Mobile μας επιδεικνύει στην παρακάτω εκμάθηση. 

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ [Πρόγραμμα επεξεργασίας ενότητας](http://unity3d.com/get-unity)
+ [Κινητές συσκευές δέσμευση ενότητας SDK](https://aka.ms/azmeunitysdk)
+ Πρόγραμμα επεξεργασίας XCode

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).

##<a id="setup-azme"></a>Ρύθμιση Mobile δέσμευση για την εφαρμογή σας iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Σύνδεση της εφαρμογής σας στον υπολογιστή στο παρασκήνιο Mobile δέσμευση

###<a name="import-the-unity-package"></a>Εισαγωγή του πακέτου ενότητας

1. Κάντε λήψη του [πακέτου Mobile δέσμευση ενότητας](https://aka.ms/azmeunitysdk) και αποθηκεύστε το στον υπολογιστή σας. 

2. Μεταβείτε στις επιλογές **παγίων -> Εισαγωγή πακέτου -> Προσαρμογή πακέτου** και επιλέξτε το πακέτο που λάβατε στο παραπάνω βήμα. 

    ![][70] 

3. Βεβαιωθείτε ότι όλα τα αρχεία που έχουν επιλεγεί και κάντε κλικ στο κουμπί " **Εισαγωγή** ". 

    ![][71] 

4. Μετά την εισαγωγή είναι επιτυχής, θα δείτε τα εισαγόμενα αρχεία SDK στο έργο σας.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>Ενημέρωση του EngagementConfiguration

1. Άνοιγμα του το αρχείο δέσμης ενεργειών **EngagementConfiguration** από το φάκελο SDK και την ενημέρωση του **IOS\_ΣΎΝΔΕΣΗΣ\_ΣΥΜΒΟΛΟΣΕΙΡΆ** με τη συμβολοσειρά σύνδεσης που λάβατε νωρίτερα από την πύλη του Azure.  

    ![][73]

2. Αποθηκεύστε το αρχείο. 

###<a name="configure-the-app-for-basic-tracking"></a>Ρύθμιση παραμέτρων της εφαρμογής για βασικές παρακολούθησης

1. Άνοιγμα της δέσμης ενεργειών **PlayerController** που συνδέονται με το πρόγραμμα αναπαραγωγής αντικείμενο για επεξεργασία. 

2. Προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση:

        using Microsoft.Azure.Engagement.Unity;

3. Προσθέστε τα εξής για να το `Start()` μέθοδος
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Ανάπτυξη και εκτελέστε την εφαρμογή

1. Συνδέστε μια συσκευή iOS στον υπολογιστή σας. 

2. Άνοιγμα του **αρχείου -> Δημιουργία ρυθμίσεις** 

    ![][40]

3. Επιλέξτε **iOS** και, στη συνέχεια, κάντε κλικ στην **Αλλαγή πλατφόρμα**

    ![][41]

    ![][42]

4. Κάντε κλικ στην επιλογή ρυθμίσεις **προγράμματος αναπαραγωγής** και δώστε ένα έγκυρο αναγνωριστικό πακέτου. 

    ![][53]

5. Τέλος κάντε κλικ στην εντολή **Δημιουργία και εκτέλεση**

    ![][54]

6. Μπορεί να σας ζητηθεί να δώσετε ένα όνομα φακέλου για να αποθηκεύσετε το πακέτο του iOS. 

    ![][43]

7. Εάν όλα τα στοιχεία μεταβαίνει λεπτομερές, στη συνέχεια, το έργο θα μεταγλωττιστεί και θα πρέπει να ανοίξετε το στην εφαρμογή σας XCode. 

8. Βεβαιωθείτε ότι το **αναγνωριστικό ομαδοποιηθούν** είναι σωστή του έργου.  

    ![][75]

10. Τώρα, εκτελέστε την εφαρμογή στο XCode, έτσι ώστε το πακέτο έχει αναπτυχθεί συνδεδεμένο στη συσκευή σας και θα πρέπει να βλέπετε το παιχνίδι ενότητας στο τηλέφωνό σας! 

##<a id="monitor"></a>Σύνδεση εφαρμογής με παρακολούθηση σε πραγματικό χρόνο

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ενεργοποίηση ειδοποιήσεων push και της εφαρμογής ανταλλαγής μηνυμάτων

Δέσμευση Mobile σάς επιτρέπει να αλληλεπιδρούν οι χρήστες σας και να ΕΠΙΚΟΙΝΩΝΉΣΕΤΕ με τις ειδοποιήσεις push και την ανταλλαγή μηνυμάτων στο περιβάλλον της εκστρατείες της εφαρμογής. Η ενότητα αυτή ονομάζεται REACH στην πύλη του δέσμευση Mobile.
Δεν χρειάζεται να κάνετε τυχόν πρόσθετες ρυθμίσεις παραμέτρων στην εφαρμογή για να λαμβάνετε ειδοποιήσεις και έχει ήδη ρυθμιστεί για αυτήν.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
