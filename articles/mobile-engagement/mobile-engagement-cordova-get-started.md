<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure δέσμευση κινητές συσκευές για Cordova/Phonegap"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Mobile δέσμευση με ανάλυση και τις ειδοποιήσεις Push για τις εφαρμογές Cordova/Phonegap."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="hero-article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Γρήγορα αποτελέσματα με το Azure δέσμευση κινητές συσκευές για Cordova/Phonegap

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure Mobile δέσμευση για να κατανοήσετε τις χρήσης εφαρμογής και αποστολή ειδοποιήσεων push σε τμηματική χρηστών για μια εφαρμογή κινητές συσκευές που αναπτύχθηκε με Cordova.

Σε αυτό το πρόγραμμα εκμάθησης, θα δημιουργήσετε μια κενή εφαρμογή Cordova χρησιμοποιώντας Mac και, στη συνέχεια, ενσωμάτωση SDK δέσμευση Mobile. Αυτό συλλέγει βασικές ανάλυση δεδομένων και λαμβάνει τις ειδοποιήσεις push με χρήση του συστήματος ειδοποιήσεων Push της Apple (APNS) για iOS και Google Cloud ανταλλαγής μηνυμάτων (GCM) για Android. Θα σας θα αναπτύξετε αυτό σε iOS ή συσκευή Android για σκοπούς δοκιμής. 

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ XCode, τα οποία μπορείτε να εγκαταστήσετε από το Mac App Store (για την ανάπτυξη σε iOS)
+ [Android SDK & προσομοίωσης](http://developer.android.com/sdk/installing/index.html) (για την ανάπτυξη σε Android)
+ Πιστοποιητικό ειδοποιήσεων Push (.p12) που μπορείτε να λάβετε από το Κέντρο ανάπτυξης Apple APNS
+ Αριθμός GCM έργου που μπορείτε να λάβετε από την κονσόλα σας Google προγραμματιστής για GCM
+ [Προσθήκη Cordova δέσμευση κινητές συσκευές](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] Μπορείτε να βρείτε το αρχείο ReadMe και τον κωδικό προέλευσης για την προσθήκη Cordova σε [Github](https://github.com/Azure/azure-mobile-engagement-cordova)

##<a id="setup-azme"></a>Ρύθμιση κινητών δέσμευση για την εφαρμογή σας Cordova

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Σύνδεση της εφαρμογής σας στον υπολογιστή στο παρασκήνιο Mobile δέσμευση

Αυτό το πρόγραμμα εκμάθησης παρουσιάζει μια "βασικές ενοποίησης" που είναι η ελάχιστη που απαιτείται για τη συλλογή δεδομένων και να στείλετε μια ειδοποίηση push. 

Θα δημιουργήσουμε μια βασική εφαρμογή με Cordova για μια επίδειξη την ενοποίηση:

###<a name="create-a-new-cordova-project"></a>Δημιουργήστε ένα νέο έργο Cordova

1. Εκκίνηση παραθύρου *τερματικού* στον υπολογιστή σας Mac και πληκτρολογήστε τα ακόλουθα στοιχεία που θα δημιουργήσετε ένα νέο έργο Cordova από το προεπιλεγμένο πρότυπο. Βεβαιωθείτε ότι το προφίλ δημοσίευσης χρησιμοποιείτε τελικά για να αναπτύξετε την εφαρμογή iOS χρησιμοποιεί 'com.mycompany.myapp' με το αναγνωριστικό εφαρμογής. 

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

2. Εκτελέστε τα εξής για να ρυθμίσετε τις παραμέτρους του project για **iOS** και εκτελέστε το στο του iOS Simulator:

        $ cordova platform add ios 
        $ cordova run ios

3. Εκτελέστε τα εξής για να ρυθμίσετε τις παραμέτρους του έργου σας για **Android** και εκτελέστε το στο του Android προσομοίωσης. Βεβαιωθείτε ότι τις ρυθμίσεις Android προσομοίωσης SDK έχουν την προορισμού ως Google APIs (Google Inc.) με το CPU / ABI ως Google APIs ARM.  

        $ cordova platform add android
        $ cordova run android

4.  Προσθήκη της προσθήκης κονσόλας Cordova. 

        $ cordova plugin add cordova-plugin-console 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Σύνδεση της εφαρμογής σας στο κινητό δέσμευση παρασκηνίου

1. Εγκατάσταση της προσθήκης Azure Mobile δέσμευση Cordova ενώ παρέχει τις τιμές μεταβλητών για να ρυθμίσετε τις παραμέτρους της προσθήκης:

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Εικονίδιο επίτευξη Android* : πρέπει να είναι το όνομα του πόρου χωρίς οποιαδήποτε επέκταση, ούτε drawable πρόθεμα (ex: mynotificationicon), και το αρχείο εικονιδίου πρέπει να αντιγραφούν σε android το έργο σας (πλατφόρμες/android/ανάλυση/drawable)

*iOS επίτευξη εικονίδιο* : πρέπει να είναι το όνομα του πόρου με την επέκταση (ex: mynotificationicon.png), και το αρχείο εικονιδίου πρέπει να προστεθεί στο έργο σας iOS με XCode (χρησιμοποιώντας το μενού αρχεία Προσθήκη)

##<a id="monitor"></a>Ενεργοποίηση παρακολούθησης σε πραγματικό χρόνο

1. Στο έργο Cordova - επεξεργασία **www/js/index.js** για να προσθέσετε την κλήση στο κινητό δέσμευση για να δηλώσετε μια νέα δραστηριότητα μία φορά στο συμβάν *deviceReady* λήψη.

         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }

2. Εκτελέστε την εφαρμογή:
        
    - **Για iOS**
    
        Στο `Terminal` παραθύρου εκκινήστε την εφαρμογή σας σε μια νέα παρουσία Simulator εκτελώντας τα εξής:

            cordova run ios

    - **Για Android**
        
        Στο `Terminal` παραθύρου εκκινήστε την εφαρμογή σας σε μια νέα παρουσία προσομοίωσης εκτελώντας τα εξής:

            cordova run android

3. Μπορείτε να δείτε τα εξής στο τα αρχεία καταγραφής από την κονσόλα:

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

##<a id="monitor"></a>Σύνδεση εφαρμογής με παρακολούθηση σε πραγματικό χρόνο

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ενεργοποίηση ειδοποιήσεων Push και της εφαρμογής ανταλλαγής μηνυμάτων

Δέσμευση Mobile σάς επιτρέπει να αλληλεπιδρούν οι χρήστες σας χρησιμοποιώντας τις ειδοποιήσεις Push και την ανταλλαγή μηνυμάτων στο περιβάλλον της εκστρατείες της εφαρμογής. Η ενότητα αυτή ονομάζεται REACH στην πύλη του δέσμευση Mobile.
Οι παρακάτω ενότητες θα εγκατάστασης την εφαρμογή σας για τη λήψη τους.

###<a name="configure-push-credentials-for-mobile-engagement"></a>Ρύθμιση παραμέτρων διαπιστευτηρίων Push για κινητό δέσμευση

Για να επιτρέψετε δέσμευση Mobile για την αποστολή ειδοποιήσεων Push για λογαριασμό σας, πρέπει να εκχωρήσετε το πρόσβαση στο Apple iOS πιστοποιητικό ή το κλειδί API GCM διακομιστή. 
    
1. Μεταβείτε στην πύλη δέσμευση Mobile. Βεβαιωθείτε ότι βρίσκεστε στην εφαρμογή σας χρησιμοποιείτε για αυτό το έργο και, στη συνέχεια, κάντε κλικ στο κουμπί **Engage** στο κάτω μέρος:
    
    ![][1]
    
2. Θα θα μεταβαίνετε στη σελίδα ρυθμίσεις στην πύλη δέσμευση. Από εκεί, κάντε κλικ στην ενότητα **Εγγενούς Push** :
    
    ![][2]

3. Ρύθμιση παραμέτρων του iOS κλειδί API πιστοποιητικό/GCM διακομιστή

    **[iOS]**

    μια. Επιλέξτε .p12 σας, στείλτε το και πληκτρολογήστε τον κωδικό πρόσβασής σας:
    
    ![][3]

    **[Android]**

    μια. Κάντε κλικ στο εικονίδιο edit μπροστά από το **API του αριθμού-κλειδιού** στην ενότητα ρυθμίσεις GCM και στο αναδυόμενο παράθυρο που εμφανίζει προς τα επάνω, επικολλήστε το κλειδί GCM διακομιστή και κάντε κλικ στο κουμπί **OK**. 
        
    ![][4]

###<a name="enable-push-notifications-in-the-cordova-app"></a>Ενεργοποίηση ειδοποιήσεων push στην εφαρμογή Cordova

Επεξεργασία **www/js/index.js** για να προσθέσετε την κλήση στο κινητό δέσμευση για να ζητήσετε τις ειδοποιήσεις push και να δηλώσετε ένα πρόγραμμα χειρισμού:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                // on OpenUrl  
                function(_url) {   
                alert(_url);   
                });  
            Engagement.startActivity("myPage",{});  
        }

###<a name="run-the-app"></a>Εκτελέστε την εφαρμογή

**[iOS]**

1. Θα χρησιμοποιήσουμε XCode για τη δημιουργία και ανάπτυξη της εφαρμογής από τη συσκευή για να ελέγξετε τις ειδοποιήσεις push, επειδή το iOS επιτρέπει μόνο τις ειδοποιήσεις push σε μια πραγματική συσκευή. Μεταβείτε στη θέση όπου δημιουργείται το έργο σας Cordova και περιηγηθείτε στη θέση **...\platforms\ios** . Άνοιγμα προς τα επάνω στο αρχείο εγγενούς .xcodeproj στο XCode. 
    
2. Δημιουργήστε και αναπτύξτε την εφαρμογή Cordova στη συσκευή iOS χρησιμοποιώντας το λογαριασμό που έχει το παροχής προφίλ που περιέχει το πιστοποιητικό που μόλις αποσταλεί στην πύλη του δέσμευση Mobile και το αναγνωριστικό εφαρμογής ποια συμφωνίες αυτό που παρείχατε κατά τη δημιουργία της εφαρμογής Cordova. Μπορείτε να ανατρέξετε το *αναγνωριστικό πακέτου* στο σας **πόρους\*-info.plist** αρχείου σε XCode ώστε να ταιριάζει με το προς τα επάνω. 

3. Θα εμφανιστεί το αναδυόμενο παράθυρο τυπική iOS στη συσκευή σας πληροφορεί ότι η εφαρμογή αιτήσεις δικαιωμάτων για την αποστολή ειδοποιήσεων. Εκχωρείτε το δικαίωμα. 

**[Android]**

Απλώς, μπορείτε να χρησιμοποιήσετε το προσομοίωσης για να εκτελέσετε την εφαρμογή Android όπως GCM ειδοποιήσεις που υποστηρίζει το Android προσομοίωσης. 

    cordova run android

##<a id="send"></a>Στείλτε μια ειδοποίηση για την εφαρμογή σας

Τώρα θα δημιουργήσουμε μια απλή εκστρατεία ειδοποιήσεων Push που θα σας στείλει ένα push σε εφαρμογή εκτελείται στη συσκευή:

1. Μεταβείτε στην καρτέλα **φτάσετε** στην πύλη Mobile δέσμευση

2. Κάντε κλικ στην επιλογή **Ανακοίνωση** για τη δημιουργία της εκστρατείας push

    ![][6]

3. Παροχή εισόδων για να δημιουργήσετε το εκστρατεία **[Android]**
    
    - Δώστε ένα **όνομα** για την εκστρατεία σας. 
    - Επιλέξτε τον **Τύπο παράδοσης** ως *Ειδοποίηση συστήματος* *απλό*
    - Επιλέξτε την **ώρα παράδοσης** ως *"οποιαδήποτε ώρα"*
    - Δώστε έναν **τίτλο** για την ειδοποίηση που θα είναι η πρώτη γραμμή στο το πάτημα.
    - Δώστε ένα **μήνυμα** για την ειδοποίηση που θα χρησιμοποιηθεί ως το σώμα του μηνύματος. 

    ![][11]

4. Παροχή εισόδων για να δημιουργήσετε το εκστρατείας **[iOS]**

    - Δώστε ένα **όνομα** για την εκστρατεία σας. 
    - Επιλέξτε την **ώρα παράδοσης** ως *"από την εφαρμογή μόνο"*
    - Δώστε έναν **τίτλο** για την ειδοποίηση που θα είναι η πρώτη γραμμή στο το πάτημα.
    - Δώστε ένα **μήνυμα** για την ειδοποίηση που θα χρησιμοποιηθεί ως το σώμα του μηνύματος. 
 
    ![][12]

5. Κάντε κύλιση προς τα κάτω και, στην ενότητα περιεχόμενο, επιλέξτε **μόνο ειδοποιήσεων**

    ![][8]

6. [Προαιρετικό] Μπορείτε επίσης να παρέχετε μια διεύθυνση URL ενέργειας. Βεβαιωθείτε ότι χρησιμοποιεί ένα συνδυασμό διεύθυνσης URL που παρέχεται κατά τη ρύθμιση παραμέτρων της προσθήκης **AZME\_ΑΝΑΚΑΤΕΎΘΥΝΣΗ\_διεύθυνση URL** μεταβλητή π.χ. *myapp://test*.  

7. Ολοκληρώσετε τη ρύθμιση της πιο βασικής εκστρατείας πιθανές. Τώρα ξανά κύλιση προς τα κάτω και κάντε κλικ στο κουμπί **Δημιουργία** για να αποθηκεύσετε το εκστρατείας.

8. Τέλος **Ενεργοποίηση** εκστρατεία σας
    
    ![][10]

9. Τώρα θα πρέπει να βλέπετε μια ειδοποίηση push στη συσκευή ή προσομοίωσης ως μέρος αυτής της εκστρατείας. 

##<a id="next-steps"></a>Επόμενα βήματα
[Επισκόπηση των όλες οι μέθοδοι είναι διαθέσιμη με Cordova Mobile δέσμευση SDK](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

