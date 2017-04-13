<properties
    pageTitle="Γρήγορα αποτελέσματα με δέσμευση Azure Mobile για Android ενότητας ανάπτυξης"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Mobile δέσμευση με ανάλυση και τις ειδοποιήσεις Push για τις εφαρμογές ενότητας αναπτύξετε για συσκευές iOS."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Γρήγορα αποτελέσματα με δέσμευση Azure Mobile για Android ενότητας ανάπτυξης

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure Mobile δέσμευση για να κατανοήσετε τις χρήσης εφαρμογής και πώς μπορείτε να στείλετε τις ειδοποιήσεις push για να φέρουν κατά διαστήματα τους χρήστες μιας εφαρμογής ενότητας κατά την ανάπτυξη σε μια συσκευή Android.
Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί την κλασική λήψη ενότητας ένα πρόγραμμα εκμάθησης μπάλα ως σημείο εκκίνησης. Θα πρέπει να ακολουθήσετε τα βήματα σε αυτό [το πρόγραμμα εκμάθησης](mobile-engagement-unity-roll-a-ball.md) , πριν να συνεχίσετε με την ενοποίηση δέσμευση Mobile μας επιδεικνύει στην παρακάτω εκμάθηση. 

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ [Πρόγραμμα επεξεργασίας ενότητας](http://unity3d.com/get-unity)
+ [Κινητές συσκευές δέσμευση ενότητας SDK](https://aka.ms/azmeunitysdk)
+ Google Android SDK

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).

##<a id="setup-azme"></a>Ρύθμιση κινητών δέσμευση για την εφαρμογή σας Android

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

1. Άνοιγμα του το αρχείο δέσμης ενεργειών **EngagementConfiguration** από το φάκελο SDK και την ενημέρωση του **ANDROID\_ΣΎΝΔΕΣΗΣ\_ΣΥΜΒΟΛΟΣΕΙΡΆ** με τη συμβολοσειρά σύνδεσης που λάβατε νωρίτερα από την πύλη του Azure.  

    ![][73]

2. Αποθηκεύστε το αρχείο 

3. Εκτέλεση **Δέσμευση -> Αρχείο -> Δημιουργία Android δηλωτικό**. Αυτή είναι η προσθήκη προστεθεί από το SDK δέσμευση Mobile και κάνοντας κλικ σε αυτό θα ενημερωθεί αυτόματα τις ρυθμίσεις του project. 

    ![][74]

> [AZURE.IMPORTANT] Βεβαιωθείτε ότι έχετε εκτελέσει αυτό κάθε φορά που ενημερώνετε το διαφορετικά **EngagementConfiguration** αρχείο τις αλλαγές σας δεν θα εφαρμόζονται στην εφαρμογή. 

###<a name="configure-the-app-for-basic-tracking"></a>Ρύθμιση παραμέτρων της εφαρμογής για βασικές παρακολούθησης

1. Άνοιγμα της δέσμης ενεργειών **PlayerController** που συνδέονται με το πρόγραμμα αναπαραγωγής αντικείμενο για επεξεργασία. 

2. Προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση:

        using Microsoft.Azure.Engagement.Unity;

3. Προσθέστε τα εξής για να το `Start()` μέθοδος
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Ανάπτυξη και εκτελέστε την εφαρμογή
Βεβαιωθείτε ότι έχετε Android SDK εγκατεστημένο στον υπολογιστή σας πριν να επιχειρήσετε να αναπτύξετε αυτής της εφαρμογής ενότητας στη συσκευή σας. 

1. Συνδέστε μια συσκευή Android στον υπολογιστή σας. 

2. Άνοιγμα του **αρχείου -> Δημιουργία ρυθμίσεις** 

    ![][40]

3. Επιλέξτε **Android** και, στη συνέχεια, κάντε κλικ στην **Αλλαγή πλατφόρμα**

    ![][51]

    ![][52]

4. Κάντε κλικ στην επιλογή ρυθμίσεις **προγράμματος αναπαραγωγής** και δώστε ένα έγκυρο αναγνωριστικό πακέτου. 

    ![][53]

5. Τέλος κάντε κλικ στην εντολή **Δημιουργία και εκτέλεση**

    ![][54]

6. Μπορεί να σας ζητηθεί να δώσετε ένα όνομα φακέλου για να αποθηκεύσετε το πακέτο του Android. 

7. Εάν όλα τα στοιχεία μεταβαίνει λεπτομερές, στη συνέχεια, το πακέτο πρόκειται να αναπτυχθούν συνδεδεμένο στη συσκευή σας και θα πρέπει να βλέπετε το παιχνίδι ενότητας στο τηλέφωνό σας! 

##<a id="monitor"></a>Σύνδεση εφαρμογής με παρακολούθηση σε πραγματικό χρόνο

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ενεργοποίηση ειδοποιήσεων push και της εφαρμογής ανταλλαγής μηνυμάτων

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###<a name="update-the-engagementconfiguration"></a>Ενημέρωση του EngagementConfiguration

1. Άνοιγμα του το αρχείο δέσμης ενεργειών **EngagementConfiguration** από το φάκελο SDK και ενημέρωση του **ANDROID\_GOOGLE\_ΑΡΙΘΜΌ** με τον **Αριθμό έργου Google** που λάβατε νωρίτερα από την πύλη για προγραμματιστές του Google Cloud. Αυτή είναι μια συμβολοσειρά τιμή προκειμένου να διασφαλιστεί ότι περικλείστε την σε διπλά εισαγωγικά. 

    ![][75]

2. Αποθηκεύστε το αρχείο. 

3. Εκτέλεση **Δέσμευση -> Αρχείο -> Δημιουργία Android δηλωτικό**. Αυτή είναι η προσθήκη προστεθεί από το SDK δέσμευση Mobile και κάνοντας κλικ σε αυτό θα ενημερωθεί αυτόματα τις ρυθμίσεις του project. 

    ![][74]

###<a name="configure-the-app-to-receive-notifications"></a>Ρύθμιση παραμέτρων της εφαρμογής για να λαμβάνετε ειδοποιήσεις

1. Άνοιγμα της δέσμης ενεργειών **PlayerController** που συνδέονται με το πρόγραμμα αναπαραγωγής αντικείμενο για επεξεργασία. 

2. Προσθέστε τα εξής για να το `Start()` μέθοδος

        EngagementReachAgent.Initialize();

3. Τώρα που η εφαρμογή είναι ενημερωθεί, ανάπτυξης και εκτελέστε την εφαρμογή σε μια συσκευή ανά τις οδηγίες που παρέχονται παρακάτω. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
