<properties
    pageTitle="Azure δέσμευση Mobile iOS ενοποίησης SDK | Microsoft Azure"
    description="Πιο πρόσφατες ενημερώσεις και διαδικασίες για iOS SDK για δέσμευση Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-ios"></a>Πώς μπορείτε να ενοποιήσετε δέσμευση στο iOS

> [AZURE.SELECTOR]
- [Παγκόσμια των Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Αυτή η διαδικασία περιγράφει ο πιο απλός τρόπος για να ενεργοποιήσετε την ανάλυση της δέσμευσης και παρακολούθηση συναρτήσεις στην εφαρμογή iOS.

Το SDK δέσμευση απαιτεί iOS6 + και Xcode 8: ο προορισμός ανάπτυξης της εφαρμογής σας πρέπει να είναι τουλάχιστον iOS 6.

> [AZURE.NOTE]
> Εάν κάνετε πραγματικά εξαρτώνται από XCode 7, στη συνέχεια, μπορείτε να χρησιμοποιήσετε το [iOS v3.2.4 SDK δέσμευση](https://aka.ms/r6oouh). Υπάρχει ένα σφάλμα γνωστά στην τη λειτουργική μονάδα Reach αυτής της προηγούμενης έκδοσης ενώ εκτελείται σε συσκευές iOS 10 δείτε [την ενοποίηση λειτουργική μονάδα reach](mobile-engagement-ios-integrate-engagement-reach.md) για περισσότερες λεπτομέρειες. Εάν επιλέξετε να χρησιμοποιήσετε το v3.2.4 SDK και, στη συνέχεια, απλώς μεταβείτε το `UserNotifications.framework` εισαγωγή στο επόμενο βήμα.

Τα ακόλουθα βήματα είναι αρκετά για να ενεργοποιήσετε την αναφορά των αρχείων καταγραφής χρειάζεται, για να υπολογίσετε όλα τα στατιστικά στοιχεία σχετικά με τους χρήστες, οι περίοδοι λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και Technicals. Η έκθεση της καταγραφής που είναι απαραίτητα για τον υπολογισμό άλλων στατιστικών στοιχείων όπως συμβάντα, σφάλματα και οι εργασίες πρέπει να γίνουν με μη αυτόματο τρόπο χρησιμοποιώντας το API δέσμευση (ανατρέξτε στο θέμα [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API στην εφαρμογή iOS](mobile-engagement-ios-use-engagement-api.md) εφόσον αυτά τα στατιστικά στοιχεία έχουν εξαρτάται από την εφαρμογή.

##<a name="embed-the-engagement-sdk-into-your-ios-project"></a>Ενσωμάτωση του SDK δέσμευση στο έργο σας iOS

- Κάντε λήψη του iOS SDK από [εδώ](http://aka.ms/qk2rnj).

- Προσθήκη του SDK δέσμευση στο έργο σας iOS: στο Xcode, κάντε δεξί κλικ στο έργο σας και επιλέξτε **"Προσθήκη αρχείων σε..."** και επιλέξτε το `EngagementSDK` φακέλου.

- Δέσμευση απαιτεί επιπλέον πλαισίων για να εργαστείτε: στην Εξερεύνηση έργου, ανοίξτε το παράθυρο έργου και επιλέξτε τη σωστή προορισμού. Στη συνέχεια, ανοίξτε την καρτέλα **"Δημιουργία φάσεις"** και στο μενού **"δυαδικό με βιβλιοθήκες σύνδεσης"** , προσθέστε τα παρακάτω πλαίσια:

    -   `UserNotifications.framework`-Ορισμός τη σύνδεση ως`Optional`
    -   `AdSupport.framework`-Ορισμός τη σύνδεση ως`Optional`
    -   `SystemConfiguration.framework`
    -   `CoreTelephony.framework`
    -   `CFNetwork.framework`
    -   `CoreLocation.framework`
    -   `libxml2.dylib`

> [AZURE.NOTE] Στο πλαίσιο AdSupport μπορούν να καταργηθούν. Δέσμευση πρέπει το πλαίσιο για τη συλλογή του IDFA. Ωστόσο, μπορεί να απενεργοποιηθεί συλλογής IDFA \<ios-sdk-δέσμευση-idfa\> για συμμόρφωση με τη νέα πολιτική Apple σχετικά με αυτό το αναγνωριστικό.

##<a name="initialize-the-engagement-sdk"></a>Προετοιμασία την πρόσληψη SDK

Πρέπει να τροποποιήσετε τον πληρεξούσιο εφαρμογής:

-   Στο επάνω μέρος του αρχείου σας υλοποίηση, εισαγάγετε τον παράγοντα δέσμευση:

        [...]
        #import "EngagementAgent.h"

-   Προετοιμασία δέσμευση μέσα στη μέθοδο '**applicationDidFinishLaunching:**'ή'**εφαρμογής: didFinishLaunchingWithOptions:**':

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##<a name="basic-reporting"></a>Βασική αναφορά

### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Προτεινόμενη μέθοδος: υπερφόρτωσης σας `UIViewController` κλάσεις

Για να ενεργοποιήσετε την αναφορά της όλα τα αρχεία καταγραφής που απαιτούνται από δέσμευση για τον υπολογισμό χρηστών, οι περίοδοι λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και τεχνικές στατιστικά στοιχεία, μπορείτε να απλώς κάνετε όλων σας `UIViewController` δευτερεύουσες κλάσεις μεταβιβάζονται από το `EngagementViewController` κλάσεις (ίδιο κανόνα για `UITableViewController`  - \> `EngagementTableViewController`).

**Χωρίς δέσμευση:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Με δέσμευση:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Εναλλακτική μέθοδο: κλήση `startActivity()` με μη αυτόματο τρόπο

Εάν δεν μπορείτε ή δεν θέλετε να υπερφόρτωσης σας `UIViewController` κλάσεις, αντί για αυτό, μπορείτε να ξεκινήσετε τις δραστηριότητές σας καλώντας `EngagementAgent`του μεθόδους απευθείας.

> [AZURE.IMPORTANT] Το iOS SDK καλεί αυτόματα το `endActivity()` μέθοδο όταν η εφαρμογή είναι κλειστό. Έτσι, είναι *ΙΔΙΑΊΤΕΡΑ* συνιστάται να καλέσετε το `startActivity` μέθοδο κάθε φορά που τη δραστηριότητα του χρήστη για αλλαγή και *ποτέ* κλήση του `endActivity` μέθοδο, επειδή το καλέσετε αυτή τη μέθοδο επιβάλει την τρέχουσα περίοδο λειτουργίας θα τερματιστεί.

##<a name="location-reporting"></a>Θέση αναφοράς

Apple όροι χρήσης του δεν επιτρέπουν εφαρμογές για να χρησιμοποιήσετε θέση παρακολούθησης μόνο σκοπό στατιστικά στοιχεία. Επομένως, συνιστάται να ενεργοποιήσετε αναφορές θέση μόνο εάν η εφαρμογή σας επίσης να χρησιμοποιήσετε τη θέση παρακολούθησης για άλλο λόγο.

Ξεκινώντας με iOS 8, πρέπει να δώσετε μια περιγραφή για τον τρόπο εφαρμογής σας χρησιμοποιεί τις υπηρεσίες θέση, ορίζοντας μια συμβολοσειρά για το πλήκτρο [NSLocationWhenInUseUsageDescription] ή [NSLocationAlwaysUsageDescription] στο αρχείο Info.plist της εφαρμογής σας. Εάν θέλετε να θέση αναφοράς στο παρασκήνιο με δέσμευση, προσθέστε τον αριθμό-κλειδί NSLocationAlwaysUsageDescription. Σε όλες τις άλλες περιπτώσεις, προσθέστε τον αριθμό-κλειδί NSLocationWhenInUseUsageDescription.

### <a name="lazy-area-location-reporting"></a>Περιοχή τεμπέλη θέση αναφοράς

Τεμπέλη περιοχή αναφοράς θέση σας επιτρέπει να αναφέρετε τη χώρα, περιοχή και τοποθεσία που έχει συσχετιστεί με συσκευές. Αυτός ο τύπος αναφοράς θέση χρησιμοποιεί μόνο θέσεις δικτύου (με βάση το Αναγνωριστικό κελιού ή μέσω Wi-Fi). Στην περιοχή συσκευή αναφέρεται πολύ μία φορά σε κάθε περίοδο λειτουργίας. GPS το έχει χρησιμοποιήσει ποτέ και, επομένως, αυτός ο τύπος της θέσης αναφοράς έχει πολύ λίγα (όχι να πείτε χωρίς) επίδραση στο το μπαταριών.

Αναφερόμενου περιοχές που χρησιμοποιούνται για τον υπολογισμό γεωγραφικά στατιστικά στοιχεία σχετικά με τους χρήστες, οι περίοδοι λειτουργίας, συμβάντα και σφάλματα. Μπορεί επίσης να χρησιμοποιηθεί ως κριτηρίου στο Reach εκστρατείες. Την τελευταία περιοχή γνωστά αναφέρθηκε για μια συσκευή μπορεί να ανακτηθεί ευχαριστήριο το [API συσκευή].

Για να ενεργοποιήσετε την αναφορά θέση τεμπέλη περιοχή, προσθέστε την ακόλουθη γραμμή μετά την προετοιμασία τον παράγοντα δέσμευση:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Πραγματικό χρόνο θέσης αναφοράς

Αναφορά θέση πραγματικό χρόνο σας επιτρέπει να αναφέρετε το γεωγραφικού πλάτους και μήκους που σχετίζονται με τις συσκευές. Από προεπιλογή, αυτός ο τύπος της θέσης αναφοράς χρησιμοποιεί μόνο θέσεις δικτύου (με βάση το Αναγνωριστικό κελιού ή μέσω Wi-Fi) και η αναφορά είναι ενεργή μόνο όταν η εφαρμογή θα εκτελείται σε πρώτο πλάνο (δηλαδή κατά τη διάρκεια μιας περιόδου λειτουργίας).

Θέσεις πραγματικό χρόνο είναι *δεν* χρησιμοποιείται για τον υπολογισμό στατιστικών στοιχείων. Μόνο ο σκοπός τους είναι να επιτρέψετε τη χρήση του πραγματικού χρόνου παν-περίφραξη \<Reach-ακροατηρίου-geofencing\> κριτηρίου στο Reach εκστρατείες.

Για να ενεργοποιήσετε την αναφορά θέση πραγματικό χρόνο, προσθέστε την ακόλουθη γραμμή μετά την προετοιμασία τον παράγοντα δέσμευση:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS βάσει αναφοράς

Από προεπιλογή, αναφοράς θέση πραγματικού χρόνου χρησιμοποιεί μόνο θέσεις δικτύου με βάση. Για να ενεργοποιήσετε τη χρήση θέσεις GPS βάσει (που είναι πολύ πιο ακριβείς), προσθέστε:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Δημιουργία αναφορών φόντου

Από προεπιλογή, αναφοράς θέση πραγματικό χρόνο είναι ενεργή μόνο όταν η εφαρμογή θα εκτελείται σε πρώτο πλάνο (δηλαδή κατά τη διάρκεια μιας περιόδου λειτουργίας). Για να ενεργοποιήσετε το αναφοράς επίσης στο παρασκήνιο, να προσθέσετε:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] Όταν η εφαρμογή θα εκτελείται στο παρασκήνιο, μόνο δικτύου βάσει θέσεις αναφερθούν, ακόμα και αν έχετε ενεργοποιήσει το GPS.

Εφαρμογή αυτής της συνάρτησης θα καλέσετε [startMonitoringSignificantLocationChanges] όταν η εφαρμογή τίθεται σε το φόντο. Έχετε υπόψη ότι αυτό θα εκκινήστε την εφαρμογή στο φόντο αυτόματα εάν φτάνει ένα νέο συμβάν θέση.

##<a name="advanced-reporting"></a>Σύνθετες αναφοράς

Προαιρετικά, εάν θέλετε να αναφέρετε εφαρμογή συγκεκριμένα συμβάντα, σφάλματα και εργασιών, πρέπει να χρησιμοποιήσετε το API δέσμευση μέσω των μεθόδων το `EngagementAgent` τάξης. Αντικείμενο αυτής της κλάσης μπορεί να ανακτηθεί, καλώντας την `[EngagementAgent shared]` στατική μέθοδο.

Το API δέσμευση σας επιτρέπει να χρησιμοποιήσετε όλες οι δυνατότητες για προχωρημένους του δέσμευση και είναι λεπτομερές του άρθρου πώς να χρησιμοποιήσετε το API δέσμευση σε iOS (καθώς και όπως στην τεχνική τεκμηρίωση του το `EngagementAgent` κλάσης).

##<a name="disable-idfa-collection"></a>Απενεργοποίηση της συλλογής IDFA

Από προεπιλογή, δέσμευση θα χρησιμοποιήσει το [IDFA] για να προσδιορίσει μοναδικά ένα χρήστη. Ωστόσο, εάν δεν χρησιμοποιείτε διαφήμιση σε κάποιο άλλο σημείο στην εφαρμογή, που μπορεί να απορριφθεί κατά τη διαδικασία αναθεώρησης App Store. Συλλογή IDFA μπορεί να απενεργοποιηθεί με την προσθήκη της μακροεντολής προεπεξεργαστή `ENGAGEMENT_DISABLE_IDFA` στο αρχείο σας pch (ή με το `Build Settings` της εφαρμογής σας). Αυτό θα εξασφαλίσει ότι δεν υπάρχει καμία αναφορές σε `ASIdentifierManager`, `advertisingIdentifier` ή `isAdvertisingTrackingEnabled` στο build της εφαρμογής.

Ενοποίηση του αρχείου **prefix.pch** :

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Μπορείτε να επαληθεύσετε ότι η συλλογή IDFA σωστά είναι απενεργοποιημένο στην εφαρμογή σας, επιλέγοντας τα αρχεία καταγραφής ελέγχου δέσμευση. Ανατρέξτε στο θέμα η ενοποίηση δοκιμή\<ios-sdk-δέσμευση-έλεγχος-idfa\> τεκμηρίωση για περαιτέρω πληροφορίες.

##<a name="disable-log-reporting"></a>Απενεργοποίηση της αναφοράς αρχείου καταγραφής

### <a name="using-a-method-call"></a>Χρησιμοποιώντας τη μέθοδο κλήσης

Εάν θέλετε δέσμευση να διακόψετε την αποστολή αρχείων καταγραφής, μπορείτε να καλέσετε:

    [[EngagementAgent shared] setEnabled:NO];

Αυτή η κλήση είναι μόνιμη: χρησιμοποιεί `NSUserDefaults` να αποθηκεύσετε τις πληροφορίες.

Μπορείτε να ενεργοποιήσετε ξανά αναφοράς καλώντας την ίδια λειτουργία με το αρχείο καταγραφής `YES`.

### <a name="integration-in-your-settings-bundle"></a>Ενοποίηση του σας ρυθμίσεις πακέτου

Αντί για κλήση αυτής της συνάρτησης, μπορείτε επίσης να ενσωματώσετε αυτήν τη ρύθμιση απευθείας στο του υπάρχοντος `Settings.bundle` αρχείου. Η συμβολοσειρά `engagement_agent_enabled` πρέπει να χρησιμοποιούνται ως μια το αναγνωριστικό προτίμηση και πρέπει να σχετίζονται με ένα διακόπτη εναλλαγής (`PSToggleSwitchSpecifier`).

Το παρακάτω παράδειγμα `Settings.bundle` δείχνει πώς μπορείτε να εφαρμόσετε:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Συσκευή API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
