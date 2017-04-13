<properties
    pageTitle="Azure Active Directory B2C: Κλήση web API από μια εφαρμογή iOS χρήση βιβλιοθηκών τρίτων | Microsoft Azure"
    description="Σε αυτό το άρθρο θα σας δείξουν πώς μπορείτε να δημιουργήσετε μια εφαρμογή 'λίστα εκκρεμών εργασιών' iOS που καλεί μια API web Node.js χρησιμοποιώντας κώδικες φορέα OAuth 2.0, χρησιμοποιώντας μια βιβλιοθήκη του άλλου κατασκευαστή"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< ms.workload="identity ms.service="active directory b2c"ετικέτες" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic= "κύρια-άρθρο"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure AD B2C: Καλέσετε ένα API web από μια εφαρμογή iOS χρησιμοποιώντας μια βιβλιοθήκη του άλλου κατασκευαστή

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Πλατφόρμα ταυτότητα της Microsoft χρησιμοποιεί ανοιχτά πρότυπα όπως OAuth2 και OpenID σύνδεση. Αυτό σας επιτρέπει στους προγραμματιστές να αξιοποιήσετε οποιαδήποτε βιβλιοθήκη που επιθυμούν να ενοποίηση με τις υπηρεσίες μας. Για να βοηθήσει τους προγραμματιστές χρησιμοποιώντας μας πλατφόρμα με άλλες βιβλιοθήκες σας γράφετε μερικά αναλυτικές όπως το εξής για να demonstate τη ρύθμιση των βιβλιοθηκών άλλου κατασκευαστή για να συνδεθείτε με την πλατφόρμα ταυτότητα της Microsoft. Οι περισσότερες βιβλιοθήκες που υλοποιεί [την προδιαγραφή RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) θα μπορείτε να συνδεθείτε με την πλατφόρμα Microsoft ταυτότητα.


Εάν είστε νέος χρήστης OAuth2 ή να συνδεθείτε OpenID ιδιαίτερα αυτής της ρύθμισης παραμέτρων δείγματος δεν ίσως να είναι πολύ λογικό για εσάς. Συνιστάται να κοιτάξετε μια σύντομη [Επισκόπηση της το πρωτόκολλο που θα σας τεκμηριώνονται εδώ](active-directory-b2c-reference-protocols.md).

> [AZURE.NOTE]
    Ορισμένες δυνατότητες της πλατφόρμας μας που έχουν μια παράσταση σε αυτά τα πρότυπα, όπως Διαχείριση πολιτικής υπό όρους πρόσβασης και Intune, απαιτούν να χρησιμοποιείτε το Microsoft Azure ταυτότητας βιβλιοθήκες Άνοιγμα αρχείου προέλευσης. 
   
Δεν όλα τα σενάρια Azure Active Directory και δυνατότητες που υποστηρίζονται από την πλατφόρμα B2C.  Για να καθορίσετε εάν θα πρέπει να χρησιμοποιήσετε την πλατφόρμα B2C, διαβάστε σχετικά με [τους περιορισμούς B2C](active-directory-b2c-limitations.md).


## <a name="get-an-azure-ad-b2c-directory"></a>Λήψη ενός καταλόγου Azure AD B2C

Μπορείτε να χρησιμοποιήσετε Azure AD B2C, πρέπει να δημιουργήσετε έναν κατάλογο, ή να μισθωτή. Ένας κατάλογος είναι ένα κοντέινερ για όλους τους χρήστες σας, εφαρμογές, ομάδες και πολλά άλλα. Ήδη, εάν δεν έχετε [δημιουργήσει έναν κατάλογο B2C](active-directory-b2c-get-started.md) πριν να συνεχίσετε.

## <a name="create-an-application"></a>Δημιουργία μιας εφαρμογής

Στη συνέχεια, πρέπει να δημιουργήσετε μια εφαρμογή στον κατάλογό σας B2C. Αυτό σας δίνει Azure AD πληροφορίες που χρειάζονται για την ασφαλή επικοινωνία με την εφαρμογή σας. Η εφαρμογή και web API αντιπροσωπεύονται από ένα μεμονωμένο **Αναγνωριστικό εφαρμογής** σε αυτήν την περίπτωση, επειδή αυτά περιλαμβάνουν μία λογική εφαρμογής. Για να δημιουργήσετε μια εφαρμογή, ακολουθήστε [αυτές τις οδηγίες](active-directory-b2c-app-registration.md). Βεβαιωθείτε ότι:

- Συμπεριλάβετε μια **κινητή συσκευή σας** στην εφαρμογή.
- Αντιγράψτε το **Αναγνωριστικό εφαρμογής** που έχει αντιστοιχιστεί σε εφαρμογή σας. Θα χρειαστεί επίσης αυτό αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Δημιουργία πολιτικών σας

Στο Azure AD B2C, κάθε εμπειρία χρήστη ορίζεται από μια [πολιτική](active-directory-b2c-reference-policies.md). Αυτή η εφαρμογή περιέχει μία εμπειρία ταυτότητας: μια συνδυαστική είσοδος και εγγραφής. Πρέπει να δημιουργήσετε αυτή την πολιτική κάθε τύπο, όπως περιγράφεται στο [άρθρο αναφοράς πολιτικής](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Όταν δημιουργείτε την πολιτική, βεβαιωθείτε ότι:

- Επιλέξτε το **εμφανιζόμενο όνομα** και τα χαρακτηριστικά εγγραφής στην πολιτική σας.
- Επιλέξτε το **εμφανιζόμενο όνομα** και το **Αναγνωριστικό αντικειμένου** δηλώσεις εφαρμογής σε κάθε πολιτική. Μπορείτε να επιλέξετε καθώς και άλλες αξιώσεων.
- Αντιγράψτε το **όνομα** κάθε πολιτική μετά τη δημιουργία του. Θα πρέπει να έχει το πρόθεμα `b2c_1_`.  Θα χρειαστεί το όνομα της πολιτικής αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Αφού δημιουργήσετε πολιτικές σας, είστε έτοιμοι να δημιουργήσετε την εφαρμογή σας.


## <a name="download-the-code"></a>Κάντε λήψη του κώδικα

Τον κώδικα για αυτό το πρόγραμμα εκμάθησης διατηρείται [σε GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c).  Για να παρακολουθήσετε κατά μήκος, μπορείτε να [κάνετε λήψη της εφαρμογής ως ένα .zip](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip) ή κλωνοποίηση αυτό:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Ή απλώς κάντε λήψη του κώδικα ολοκληρωμένη και να ξεκινήσετε αμέσως: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>Κάντε λήψη του άλλου κατασκευαστή nxoauth2 βιβλιοθήκη και να ξεκινήσετε το χώρο εργασίας

Για αυτόν τον οδηγό, θα χρησιμοποιήσουμε το OAuth2Client από GitHub, μια βιβλιοθήκη OAuth2 για Mac OS X & iOS (κακάο & κακάο αφής). Αυτή η βιβλιοθήκη βασίζεται σε σχέδιο 10 των την προδιαγραφή OAuth2. Το υλοποιεί το προφίλ εγγενή εφαρμογή και υποστηρίζει το τελικό σημείο εξουσιοδότησης τελικού χρήστη. Αυτά είναι όλες τις ενέργειες που θα χρειαστούμε προκειμένου να integrat με το Microsoft ταυτότητας πλατφόρμα.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>Προσθήκη στη βιβλιοθήκη στο έργο σας με χρήση CocoaPods

CocoaPods είναι διαχειριστής εξάρτηση για Xcode έργα. Διαχειρίζεται αυτόματα τα παραπάνω βήματα εγκατάστασης.

```
$ vi Podfile
```
Προσθέστε τα ακόλουθα για να podfile αυτό:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Η φόρτωση τώρα το podfile χρησιμοποιώντας cocoapods. Αυτό θα δημιουργήσει ένα νέο χώρο εργασίας XCode θα φορτώσετε.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>Η δομή του έργου

Έχουμε την ακόλουθη δομή ρύθμιση για το έργο στο το σκελετό:

* **Προβολή υποδείγματος** με ενός παραθύρου εργασιών
* **Προσθήκη προβολής εργασιών** για τα δεδομένα σχετικά με την επιλεγμένη εργασία
* Μια **Προβολή σύνδεσης** που επιτρέπει στο χρήστη να είσοδος στην εφαρμογή.

Θα σας θα μεταπηδήσει σε διάφορα αρχεία του έργου για να προσθέσετε τον έλεγχο ταυτότητας. Άλλα τμήματα του κώδικα όπως τον κώδικα visual δεν είναι germane ταυτότητα και παρέχονται για εσάς.

## <a name="create-the-settingsplist-file-for-your-application"></a>Δημιουργία του `settings.plist` αρχείου για την εφαρμογή σας

Είναι πιο εύκολο να ρυθμίσετε τις παραμέτρους της εφαρμογής αν έχουμε μια κεντρική θέση για να τοποθετήσετε μας τιμές παραμέτρων. Επίσης, σας βοηθά να κατανοήσετε τι κάνει το κάθε ρύθμιση στην εφαρμογή σας. Θα σας θα αξιοποιήσετε τη *Λίστα ιδιοτήτων* για να παρέχουν αυτές τις τιμές με την εφαρμογή.

* Δημιουργία/άνοιγμα του `settings.plist` αρχείων στην περιοχή `Supporting Files` στο χώρο εργασίας σας εφαρμογής

* Εισαγάγετε τις παρακάτω τιμές (θα σας θα ακολουθήσετε με τη λεπτομέρεια σύντομα)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Ας πάμε στο αυτά με λεπτομέρειες.


Για `authURL`, `loginURL`, `bhh`, `tokenURL` θα παρατηρήσετε που πρέπει να συμπληρώσετε το όνομα του μισθωτή. Αυτό είναι το όνομα του μισθωτή του μισθωτή σας B2C που έχει ανατεθεί σε εσάς. Για παράδειγμα, `kidventusb2c.onmicrosoft.com`. Εάν χρησιμοποιείτε το Microsoft Azure ταυτότητας βιβλιοθήκες Άνοιγμα αρχείου προέλευσης θα σας θα αναπτυσσόμενο αυτών των δεδομένων χρησιμοποιώντας το τελικό σημείο μετα-δεδομένων. Θα σας κάνει τη σκληρή δουλειά σας από την εξαγωγή αυτές τις τιμές για εσάς.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Το `keychain` τιμή είναι το κοντέινερ που θα χρησιμοποιήσει τη βιβλιοθήκη NXOAuth2Client για να δημιουργήσετε μια αλυσίδα κλειδιών για την αποθήκευση του διακριτικά. Εάν θέλετε να μεταδώσετε το πολλά εφαρμογές SSO που μπορούν να καθορίσετε την ίδια αλυσίδα κλειδιών σε κάθε μία από τις εφαρμογές σας καθώς και αίτηση για τη χρήση που αλυσίδας κλειδιών στο σας entitements XCode. Καλύπτεται στην τεκμηρίωση της Apple.

Το `<policy name>` στο τέλος της κάθε διεύθυνση URL είναι τα σημεία όπου μπορείτε να τοποθετήσετε την πολιτική που δημιουργήσατε παραπάνω. Η εφαρμογή θα καλέσετε αυτές τις πολιτικές ανάλογα με τη ροή.

Η `taskAPI` είναι το τελικό σημείο ΥΠΌΛΟΙΠΟ θα ονομάζουμε με το διακριτικό B2C είτε για να προσθέσετε εργασίες ή εργασίες υπάρχοντος ερωτήματος. Αυτό έχει οριστεί ειδικά για αυτό το δείγμα. Δεν χρειάζεται να την αλλάξετε για το δείγμα για να εργαστείτε.

Για να χρησιμοποιήσετε τη βιβλιοθήκη και απλώς να δημιουργήσετε θέσεις για να εκτελέσετε τιμές στο περιβάλλον απαιτούνται τα υπόλοιπα από αυτές τις τιμές.

Τώρα που έχουμε το `settings.plist` αρχείο που δημιουργήσατε, χρειαζόμαστε κώδικα για να το διαβάσετε.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>Ρύθμιση μιας κλάσης AppData για να διαβάσετε μας ρυθμίσεις

Ας κάνουμε ένα απλό αρχείο που μόλις αναλύει μας `settngs.plist` αρχείο θα σας δημιουργήθηκε παραπάνω και να κάνετε αυτές τις avaialble ρυθμίσεις στο μέλλον σε οποιαδήποτε κλάση. Εφόσον δεν θέλουμε να δημιουργήσετε ένα νέο αντίγραφο των δεδομένων κάθε φορά που ένα εκπαιδευτικό σας ρωτά για αυτό, θα χρησιμοποιήσετε ένα μοτίβο Singleton και επιστρέφει μόνο την ίδια παρουσία δημιουργείται κάθε φορά που γίνεται μια αίτηση για τις ρυθμίσεις

* Δημιουργήστε μια `AppData.h` αρχείο:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Δημιουργήστε μια `AppData.m` αρχείο:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Τώρα θα μπορώ να αποκτήσω εύκολα με δεδομένα μας, καλώντας απλώς `  AppData *data = [AppData getInstance];` σε οποιαδήποτε μας κλάσεων ως που θα δείτε παρακάτω.



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>Ρύθμιση της βιβλιοθήκης NXOAuth2Client σε AppDelegate σας

Η βιβλιοθήκη NXOAuthClient απαιτεί ορισμένες τιμές για να ρυθμίσετε. Μία φορά που έχει ολοκληρωθεί μπορείτε να χρησιμοποιήσετε το διακριτικό που είναι aquired για να καλέσετε το REST API. Επειδή το γνωρίζουμε που το `AppDelegate` θα καλούνται οποιαδήποτε στιγμή μας φόρτωση της εφαρμογής έχει νόημα που θέτει μας τιμές παραμέτρων σε αυτό το αρχείο.
* Άνοιγμα `AppDelegate.m` αρχείου

* Εισαγωγή ορισμένα αρχεία κεφαλίδας, θα χρησιμοποιήσουμε αργότερα.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Προσθήκη του `setupOAuth2AccountStore` μέθοδο σε το AppDelegate

Πρέπει να δημιουργήσετε ένα AccountStore και τροφοδοσίας του τα δεδομένα που θα σας μόλις διάβασε στο από το `settings.plist` αρχείου.

Υπάρχουν ορισμένα πράγματα που πρέπει να γνωρίζετε σχετικά με την B2C υπηρεσία αυτήν τη στιγμή που θα κάνει πιο κατανοητό αυτόν τον κώδικα:


1. Azure AD B2C χρησιμοποιεί την *πολιτική* σύμφωνα με τις παραμέτρους του ερωτήματος για να εξυπηρέτηση της αίτησής σας. Αυτό σας επιτρέπει Azure Active Directory για να λειτουργήσει ως μια ανεξάρτητη υπηρεσία μόνο για την εφαρμογή σας. Για να παρέχουν αυτές τις επιπλέον ερωτήματος παραμέτρων πρέπει να παρέχουμε το `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` μέθοδο με μας παραμέτρους προσαρμοσμένης πολιτικής. 

2. Azure AD B2C χρησιμοποιεί εύρους περίπου τον ίδιο τρόπο ως άλλους διακομιστές OAuth2. Ωστόσο εφόσον η χρήση της B2C είναι όσο σχετικά με τον έλεγχο ταυτότητας χρήστη ως πρόσβαση σε πόρους ορισμένες εύρους απολύτως απαιτείται για τη ροή για να λειτουργήσει σωστά. Αυτό είναι το `openid` εύρος. Μας ταυτότητας Microsoft SDK παρέχεται αυτόματα το `openid` εμβέλειας για εσάς, ώστε να δεν θα το δείτε που μας ρύθμισης παραμέτρων SDK. Επειδή χρησιμοποιούμε μια βιβλιοθήκη του άλλου κατασκευαστή, ωστόσο, πρέπει να καθορίσετε αυτό το εύρος.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Στη συνέχεια, βεβαιωθείτε ότι καλείτε στο το AppDelegate στην περιοχή `didFinishLaunchingWithOptions:` μέθοδο. 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Δημιουργία μιας `LoginViewController` κλάση που θα χρησιμοποιήσουμε χειρισμού αιτήσεων ελέγχου ταυτότητας

Χρησιμοποιούμε μια προβολής Web για το λογαριασμό εισόδου. Αυτό σας επιτρέπει να γίνεται ερώτηση στο χρήστη για πρόσθετους παράγοντες όπως το μήνυμα κειμένου SMS (εάν έχει ρυθμιστεί) ή να δώσετε μηνύματα σφάλματος πίσω στο χρήστη. Εδώ θα μπορούμε να εγκαταστήσουμε του την προβολή Web και, στη συνέχεια, συντάξτε αργότερα τον κώδικα που χειρίζεται το επιστροφές κλήσης που θα συμβεί σε την προβολή Web από την υπηρεσία Microsoft ταυτότητα.

* Δημιουργία μιας `LoginViewController.h` τάξης

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Θα δημιουργήσουμε κάθε μία από αυτές τις παρακάτω μεθόδους.

> [AZURE.NOTE] 
    Βεβαιωθείτε ότι μπορείτε να συνδέσετε το `loginView` για να την πραγματική προβολής Web που βρίσκεται στο εσωτερικό του πίνακα διάταξης. Διαφορετικά δεν θα έχετε μια προβολή Web που μπορεί να εμφανιστεί κατά την ώρα για τον έλεγχο ταυτότητας.

* Δημιουργία μιας `LoginViewController.m` τάξης

* Προσθήκη ορισμένες μεταβλητές που πρέπει να εκπληρώσει κατάσταση που θα σας τον έλεγχο ταυτότητας

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Παράκαμψη τις μεθόδους προβολής Web για το χειρισμό ελέγχου ταυτότητας

Πρέπει να παρέχουμε την προβολή Web τη συμπεριφορά θέλουμε όταν ο χρήστης πρέπει να συνδεθείτε, όπως περιγράφεται παραπάνω. Απλώς να αποκόψετε και επικολλήστε τον παρακάτω κώδικα.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Σύνταξη κώδικα να χειρίζεται το αποτέλεσμα της αίτησης OAuth2

Θα χρειαστούμε κώδικα που θα χειρίζεται το redirectURL που παρέχεται πίσω από την προβολή Web. Εάν δεν ήταν επιτυχής, θα προσπαθήσουμε ξανά. Στο μεταξύ στη βιβλιοθήκη παρέχει το σφάλμα ότι μπορείτε να βλέπετε στην κονσόλα ή να χειριστείτε λήψη. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Ρυθμίστε το εργοστάσια ειδοποίησης.

Δημιουργούμε την ίδια μέθοδο κάναμε στο το `AppDelegate` παραπάνω, αλλά αυτήν τη στιγμή που θα προσθέσουμε ορισμένα `NSNotification`s για να πείτε μας τι συμβαίνει στην υπηρεσία μας. Μπορούμε να εγκαταστήσουμε παρατηρητής που θα πείτε μας όταν αλλάξει κάτι με το διακριτικό. Μόλις λαμβάνουμε το διακριτικό θα σας επιστρέψει ο χρήστης ξανά το `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Προσθήκη κώδικα που χειρίζεται το χρήστη κάθε φορά που ξεκινά μια αίτηση για εγγενή εισόδου

Ας δημιουργήσουμε μια μέθοδο που θα καλούνται κάθε φορά που έχουμε μια αίτηση για έλεγχο ταυτότητας. Θα είναι αυτή τη μέθοδο που πραγματικά δημιουργεί μια προβολής Web

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Τέλος, ας καλεί όλες αυτές τις μεθόδους που θα σας γράφετε επάνω από κάθε φορά το `LoginViewController` φορτώνεται. Αυτό το κάνουμε προσθέτοντας τις παρακάτω μεθόδους για να μας `viewDidLoad` μέθοδο Apple παρέχει μας

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Τώρα έχετε ολοκληρώσει τη δημιουργία τον κύριο τρόπο θα σας θα αλληλεπιδράτε με την εφαρμογή μας για είσοδο στην. Αφού μας έχετε πραγματοποιήσει είσοδο, θα πρέπει να χρησιμοποιήσετε τα διακριτικά έχουμε λάβει. Για την οποία θα δημιουργήσουμε ορισμένες κώδικα Βοήθειας που θα καλούν τα ΥΠΌΛΟΙΠΑ APIs για μας, χρησιμοποιώντας αυτήν τη βιβλιοθήκη.


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Δημιουργία μιας `GraphAPICaller` κλάση χειρισμού μας αιτήσεων σε μια REST API

Έχουμε μια ρύθμιση παραμέτρων που φορτώνεται κάθε φορά που θα σας φόρτωση μας εφαρμογής. Τώρα πρέπει να κάνετε κάτι με αυτό, μόλις έχουμε ενός διακριτικού. 

* Δημιουργία μιας `GraphAPICaller.h` αρχείου

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Δείτε από αυτόν τον κωδικό που θα σας θα δημιουργούν δύο μεθόδους: μία για να λάβετε τις εργασίες από το API και ένα άλλο για να προσθέσετε εργασίες για το API.

Τώρα που έχετε μπορούμε να εγκαταστήσουμε το περιβάλλον εργασίας, ας προσθέσουμε την πραγματική υλοποίηση:

* Δημιουργήστε μια`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>Εκτελέστε την εφαρμογή δείγματος

Τέλος, δημιουργία και εκτέλεση της εφαρμογής στο Xcode. Εγγραφείτε ή συνδεθείτε με την εφαρμογή και δημιουργία εργασιών για ένα χρήστη συνδεδεμένοι στο. Πραγματοποιήστε έξοδο και συνδεθείτε ξανά με ένα διαφορετικό χρήστη και δημιουργία εργασιών για τον συγκεκριμένο χρήστη.

Παρατηρήστε ότι οι εργασίες είναι αποθηκευμένες ανά χρήστη στην το API, επειδή το API εξάγει την ταυτότητα του χρήστη από το διακριτικό πρόσβασης που λαμβάνει.


## <a name="next-steps"></a>Επόμενα βήματα

Τώρα μπορείτε να μετακινήσετε σε θέματα για προχωρημένους B2C. Μπορείτε να δοκιμάσετε:

[Κλήση ενός API web Node.js από μια εφαρμογή web Node.js]()

[Προσαρμογή του UX για μια εφαρμογή B2C]()
