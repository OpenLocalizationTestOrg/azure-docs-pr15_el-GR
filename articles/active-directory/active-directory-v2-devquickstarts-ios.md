<properties
    pageTitle="Azure AD v2.0 εφαρμογή iOS | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή iOS που πραγματοποιεί είσοδο εταιρικό ή σχολικό λογαριασμοί και οι χρήστες με δύο προσωπικό λογαριασμό Microsoft, με τη χρήση βιβλιοθηκών τρίτων κατασκευαστών."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Προσθήκη εισόδου σε μια εφαρμογή iOS χρησιμοποιώντας μια βιβλιοθήκη τρίτων κατασκευαστών με API γραφήματος χρησιμοποιώντας το τελικό σημείο v2.0

Πλατφόρμα ταυτότητα της Microsoft χρησιμοποιεί ανοιχτά πρότυπα όπως OAuth2 και OpenID σύνδεση. Οι προγραμματιστές μπορούν να χρησιμοποιούν οποιαδήποτε βιβλιοθήκη που θέλουν να ενοποίηση με τις υπηρεσίες μας. Για να βοηθήσει τους προγραμματιστές να χρησιμοποιήσετε μας πλατφόρμα με άλλες βιβλιοθήκες, θα σας γράφετε μερικά αναλυτικές όπως το εξής για να δείχνουν τον τρόπο ρύθμισης παραμέτρων βιβλιοθήκες τρίτων κατασκευαστών για να συνδεθείτε με την πλατφόρμα ταυτότητα της Microsoft. Οι περισσότερες βιβλιοθήκες που υλοποιεί [την προδιαγραφή RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) να συνδεθείτε με την πλατφόρμα ταυτότητα της Microsoft.

Με την εφαρμογή που δημιουργεί αυτόν τον οδηγό, οι χρήστες μπορούν να εισέλθετε στο οργανισμού τους και, στη συνέχεια, αναζήτηση άλλους χρήστες στον οργανισμό τους με χρήση του API του γραφήματος.

Εάν είστε νέος χρήστης του OAuth2 ή να συνδεθείτε OpenID, ιδιαίτερα αυτής της ρύθμισης παραμέτρων δείγμα ίσως δεν νόημα για εσάς. Συνιστάται να διαβάσετε [τα πρωτόκολλα v2.0 - διακριτικό 2.0 εξουσιοδότησης κώδικα ροής](active-directory-v2-protocols-oauth-code.md) για φόντο.


> [AZURE.NOTE]
    Ορισμένες δυνατότητες της πλατφόρμας μας που έχουν μια παράσταση στα πρότυπα OAuth2 ή OpenID σύνδεση, όπως υπό όρους πρόσβασης και διαχείριση πολιτικών υπηρεσιών Intune, απαιτούν να χρησιμοποιείτε το Microsoft Azure ταυτότητας βιβλιοθήκες Άνοιγμα αρχείου προέλευσης.

Το τελικό σημείο v2.0 δεν υποστηρίζει όλα τα σενάρια Azure Active Directory και δυνατότητες.

> [AZURE.NOTE]
    Για να καθορίσετε εάν θα πρέπει να χρησιμοποιήσετε το τελικό σημείο v2.0, διαβάστε σχετικά με [τους περιορισμούς v2.0](active-directory-v2-limitations.md).

## <a name="download-code-from-github"></a>Κάντε λήψη του κώδικα από GitHub
Τον κώδικα για αυτό το πρόγραμμα εκμάθησης διατηρείται [σε GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).  Για να παρακολουθήσετε κατά μήκος, μπορείτε να [κάνετε λήψη της εφαρμογής σκελετό ως ένα .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) ή κλωνοποίηση το σκελετό:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Μπορείτε επίσης απλώς να κάνετε λήψη το δείγμα και να ξεκινήσετε αμέσως:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Καταχώρηση εφαρμογής
Δημιουργία νέας εφαρμογής στην [πύλη καταχώρηση εφαρμογής](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ή ακολουθήστε τα λεπτομερή βήματα στο θέμα [Πώς να καταχωρήσετε μια εφαρμογή με το τελικό σημείο v2.0](active-directory-v2-app-registration.md).  Βεβαιωθείτε ότι:

- Αντιγράψτε το **Αναγνωριστικό εφαρμογής** που έχει αντιστοιχιστεί σε εφαρμογή σας, επειδή θα τον χρειαστείτε σύντομα.
- Προσθέστε την πλατφόρμα **Mobile** για την εφαρμογή σας.
- Αντιγράψτε το **URI ανακατεύθυνση** από την πύλη. Πρέπει να χρησιμοποιήσετε την προεπιλεγμένη τιμή του `urn:ietf:wg:oauth:2.0:oob`.


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>Λήψη στη βιβλιοθήκη NXOAuth2 τρίτων κατασκευαστών και να δημιουργήσετε ένα χώρο εργασίας

Για αυτόν τον οδηγό, μπορείτε να χρησιμοποιήσετε το OAuth2Client από GitHub, που είναι μια βιβλιοθήκη OAuth2 για το Mac OS X και iOS (κακάο και κακάο αφής). Αυτή η βιβλιοθήκη βασίζεται σε σχέδιο 10 των την προδιαγραφή OAuth2. Το υλοποιεί το προφίλ εγγενή εφαρμογή και υποστηρίζει το τελικό σημείο εξουσιοδότησης του χρήστη. Αυτά είναι όλες τις ενέργειες που θα πρέπει να την ενοποίηση με την πλατφόρμα ταυτότητα της Microsoft.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>Προσθήκη της βιβλιοθήκης στο έργο σας με τη χρήση CocoaPods

CocoaPods είναι διαχειριστής εξάρτηση για Xcode έργα. Διαχειρίζεται αυτόματα τα προηγούμενα βήματα εγκατάστασης.

```
$ vi Podfile
```
1. Προσθέστε τα ακόλουθα για να podfile αυτό:

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. Φόρτωση του podfile με τη χρήση CocoaPods. Αυτό θα δημιουργήσει ένα νέο χώρο εργασίας Xcode που θα φορτώσετε.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>Εξερευνήστε τη δομή του έργου

Η παρακάτω δομή έχει ρυθμιστεί για το έργο στο το σκελετό:

- Μια προβολή υποδείγματος μια αναζήτηση UPN
- Μια προβολή λεπτομερειών για τα δεδομένα σχετικά με τον επιλεγμένο χρήστη
- Μια προβολή Login όπου ένας χρήστης να εισέλθετε στην εφαρμογή ερώτημα για το γράφημα

Θα σας θα μετακινηθεί σε διάφορα αρχεία το σκελετό για να προσθέσετε τον έλεγχο ταυτότητας. Άλλα τμήματα του κώδικα, όπως τον κώδικα visual, δεν σχετίζονται με ταυτότητας αλλά παρέχονται για εσάς.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Ρύθμιση του αρχείου settings.plst στη βιβλιοθήκη

-   Γρήγορη έναρξη του έργου, ανοίξτε το `settings.plist` αρχείου. Αντικαταστήστε τις τιμές από τα στοιχεία της ενότητας για να εμφανίζουν τις τιμές που χρησιμοποιήσατε στην πύλη του Azure. Ο κώδικας θα αναφοράς αυτές τις τιμές κάθε φορά που χρησιμοποιεί τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory.
    -   Η `clientId` είναι το Αναγνωριστικό πελάτη της εφαρμογής σας που αντιγράψατε από την πύλη.
    -   Η `redirectUri` είναι η διεύθυνση URL redirect που παρέχεται από την πύλη.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>Ρύθμιση της βιβλιοθήκης NXOAuth2Client σε LoginViewController σας

Η βιβλιοθήκη NXOAuth2Client απαιτεί ορισμένες τιμές για να ρυθμίσετε. Αφού ολοκληρώσετε αυτήν την εργασία, μπορείτε να χρησιμοποιήσετε το διακριτικό που αποκτήθηκε για να καλέσετε το API του γραφήματος. Επειδή `LoginView` θα είναι που ονομάζεται οποιαδήποτε στιγμή χρειαζόμαστε για τον έλεγχο ταυτότητας, έχει νόημα για να θέσετε τιμές παραμέτρων σε αυτό το αρχείο.

- Ας προσθέσουμε ορισμένα τιμών για την `LoginViewController.m` αρχείο για να ρυθμίσετε το περιβάλλον για τον έλεγχο ταυτότητας και εξουσιοδότηση. Λεπτομέρειες σχετικά με τις τιμές ακολουθήστε τον κώδικα.

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Ας δούμε λεπτομέρειες σχετικά με τον κώδικα.

Η πρώτη συμβολοσειρά είναι για `scopes`.  Το `User.Read` τιμή σάς επιτρέπει να διαβάσετε το βασικό προφίλ του χρήστη για είσοδο στο.

Μπορείτε να μάθετε περισσότερα σχετικά με όλα τα διαθέσιμα τα πεδία στο [Microsoft Graph δικαιωμάτων εύρους](https://graph.microsoft.io/docs/authorization/permission_scopes).

Για `authURL`, `loginURL`, `bhh`, και `tokenURL`, θα πρέπει να χρησιμοποιήσετε τις τιμές που παρέχονται προηγουμένως. Εάν χρησιμοποιείτε το Microsoft Azure ταυτότητας βιβλιοθήκες Άνοιγμα αρχείου προέλευσης, θα σας αναπτυσσόμενο αυτά τα δεδομένα για εσάς, χρησιμοποιώντας το τελικό σημείο μετα-δεδομένων. Θα σας κάνει τη σκληρή δουλειά σας από την εξαγωγή αυτές τις τιμές για εσάς.

Το `keychain` τιμή είναι το κοντέινερ που θα χρησιμοποιήσει τη βιβλιοθήκη NXOAuth2Client για να δημιουργήσετε μια αλυσίδα κλειδιών για την αποθήκευση του διακριτικά. Εάν θέλετε να μεταδώσετε πολλά εφαρμογές καθολικής σύνδεσης (SSO), μπορείτε να καθορίσετε την ίδια αλυσίδα κλειδιών σε κάθε μία από τις εφαρμογές σας και να ζητήσετε τη χρήση που αλυσίδας κλειδιών στο σας δικαιώματα Xcode. Η πολιτική αυτή εξηγείται στην τεκμηρίωση της Apple.

Τα υπόλοιπα από αυτές τις τιμές που απαιτούνται για χρήση στη βιβλιοθήκη και δημιουργία θέσεις για να εκτελέσετε τιμές για το περιβάλλον.

### <a name="create-a-url-cache"></a>Δημιουργία μιας διεύθυνσης URL cache

Μέσα σε `(void)viewDidLoad()`, που ονομάζεται πάντα μετά τη φόρτωση της προβολής, ο ακόλουθος κώδικας primes ένα cache για χρήση μας.

Προσθέστε τον ακόλουθο κώδικα:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Δημιουργία μιας προβολής Web για είσοδο

Μια προβολή Web μπορεί να ζητήσει από το χρήστη για πρόσθετους παράγοντες όπως το μήνυμα κειμένου SMS (εάν έχει ρυθμιστεί) ή επιστροφή μηνύματα σφάλματος στο χρήστη. Εδώ θα ορίσετε επάνω την προβολή Web και, στη συνέχεια, συντάξτε αργότερα τον κώδικα που χειρίζεται το επιστροφές κλήσης που θα συμβεί σε την προβολή Web από τις υπηρεσίες ταυτότητας.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>Παράκαμψη τις μεθόδους προβολής Web για το χειρισμό ελέγχου ταυτότητας

Για να υποδείξετε την προβολή Web τι συμβαίνει όταν ο χρήστης πρέπει να εισέλθετε, όπως περιγράφεται παραπάνω, μπορείτε να επικολλήσετε τον παρακάτω κώδικα.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Σύνταξη κώδικα να χειρίζεται το αποτέλεσμα της αίτησης OAuth2

Ο ακόλουθος κώδικας θα χειρίζεται το redirectURL που επιστρέφει από την προβολή Web. Εάν ο έλεγχος ταυτότητας δεν ήταν επιτυχής, ο κώδικας θα προσπαθήστε ξανά. Στο μεταξύ, η βιβλιοθήκη παρέχει το σφάλμα που μπορείτε να βλέπετε στην κονσόλα ή να χειριστείτε ασύγχρονα.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>Ρύθμιση του περιβάλλοντος OAuth (που ονομάζεται χώρος αποθήκευσης λογαριασμού)

Εδώ μπορείτε να καλέσετε `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` στο χώρο αποθήκευσης κοινόχρηστο λογαριασμό για κάθε υπηρεσία που θέλετε να μπορεί να αποκτήσει πρόσβαση από την εφαρμογή. Ο τύπος λογαριασμού είναι μια συμβολοσειρά που χρησιμοποιείται ως αναγνωριστικό για μια συγκεκριμένη υπηρεσία. Επειδή έχετε πρόσβαση σε το API Graph, τον κώδικα που αναφέρεται σε αυτό ως `"myGraphService"`. Στη συνέχεια, ορίστε παρατηρητής που θα σας ενημερώσει όταν αλλάξει κάτι με το διακριτικό. Αφού λάβετε το διακριτικό, επιστρέψετε στο χρήστη πίσω στην το `masterView`.



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>Ρύθμιση της προβολής υποδείγματος για να αναζητήσετε και να εμφανίσετε τους χρήστες από το API Graph

Μια εφαρμογή υπόδειγμα-προβολή-ελεγκτή (MVC) που εμφανίζει τα δεδομένα που επιστρέφονται στο πλέγμα είναι πέρα από το πεδίο από αυτόν τον οδηγό και πολλά ηλεκτρονικά προγράμματα εκμάθησης εξηγούν τον τρόπο για να δημιουργήσετε μία. Είναι όλα αυτόν τον κωδικό στο αρχείο σκελετό. Ωστόσο, πρέπει να ασχοληθείτε με μερικά πράγματα σε αυτήν την εφαρμογή MVC:

* Σημείο τομής όταν ένας χρήστης πληκτρολογήσει κάτι στο πεδίο αναζήτησης
* Δώστε ένα αντικείμενο δεδομένων προς το MasterView, ώστε το να εμφανίσετε τα αποτελέσματα στο πλέγμα

Θα κάνουμε αυτές παρακάτω.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Προσθέστε ένα σημάδι ελέγχου για να δείτε εάν είστε συνδεδεμένοι

Η εφαρμογή κάνει μικρό εάν ο χρήστης δεν είναι συνδεδεμένοι, ώστε να είναι Έξυπνες για να ελέγξετε εάν υπάρχει ήδη ένα διακριτικό στο cache. Εάν όχι, ανακατευθύνετε τη LoginView για το χρήστη για να πραγματοποιήσετε είσοδο. Εάν μπορείτε να ανακαλέσετε, ο καλύτερος τρόπος για να εκτελέσετε ενέργειες κατά τη φόρτωση μιας προβολής είναι να χρησιμοποιήσετε το `viewDidLoad()` μέθοδος που παρέχει Apple μας.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>Ενημέρωση της προβολής πίνακα κατά τη λήψη δεδομένων

Όταν το API Graph επιστρέφει δεδομένα, πρέπει να εμφανίσετε τα δεδομένα. Για λόγους ευκολίας, θα δείτε όλα τα κώδικα για να ενημερώσετε τον πίνακα. Μπορείτε να επικολλήσετε μόνο τις σωστές τιμές στον κώδικα στερεότυπο MVC.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Παρέχει κάποιο τρόπο για να καλέσετε το API Graph όταν κάποιος πληκτρολογεί στο πεδίο αναζήτησης

Όταν ένας χρήστης πληκτρολογεί μιας αναζήτησης στο πλαίσιο, πρέπει να shove που επάνω από το API του γραφήματος. Το `GraphAPICaller` κλάση, η οποία θα δημιουργήσετε τον παρακάτω κώδικα, διαχωρίζει τη λειτουργικότητα αναζήτησης από την παρουσίαση. Για τώρα, ας εγγραφή του κώδικα που τροφοδοσίες χαρακτήρες αναζήτησης για το API Graph. Αυτό το κάνουμε, παρέχοντας μια μέθοδο που ονομάζεται `lookupInGraph`, που θα λαμβάνει τη συμβολοσειρά που θέλετε να αναζητήσετε.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Γράψτε μια κλάση βοηθητικού προγράμματος για να αποκτήσετε πρόσβαση το API Graph

Αυτό είναι το μέγεθος των κύριων μας εφαρμογής. Ενώ το υπόλοιπο εισαγωγή κώδικα στο προεπιλεγμένο μοτίβο MVC από την Apple, εδώ συντάσσετε κώδικα για την υποβολή ερωτήματος στο graph καθώς ο χρήστης πληκτρολογεί και, στη συνέχεια, επιστρέψετε αυτών των δεδομένων. Δείτε τον κώδικα και ακολουθεί μια αναλυτική εξήγηση.

### <a name="create-a-new-objective-c-header-file"></a>Δημιουργήστε ένα νέο αρχείο κεφαλίδας στόχο C

Ονομάστε το αρχείο `GraphAPICaller.h`, και προσθέστε τον παρακάτω κώδικα.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Εδώ μπορείτε να δείτε ότι μια καθορισμένη μέθοδος λαμβάνει μια συμβολοσειρά και επιστρέφει μια completionBlock. Αυτό completionBlock, όπως που ενδέχεται να έχετε μαντέψει, θα ενημερώσει τον πίνακα, παρέχοντας ένα αντικείμενο με συμπληρωμένα δεδομένα σε πραγματικό χρόνο ως στις αναζητήσεις χρήστη.


### <a name="create-a-new-objective-c-file"></a>Δημιουργήστε ένα νέο αρχείο στόχο C

Ονομάστε το αρχείο `GraphAPICaller.m`, και προσθέστε την ακόλουθη μέθοδο.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

Ας δούμε από αυτήν τη μέθοδο με λεπτομέρειες.

Είναι το μέγεθος των κύριων από αυτόν τον κωδικό σε το `NXOAuth2Request`, μέθοδο που λαμβάνει τις παραμέτρους που που έχετε ήδη ορίσει στο αρχείο settings.plist.

Είναι το πρώτο βήμα για να δημιουργήσετε την κλήση Graph API δεξιά. Επειδή που καλείτε `/users`, που καθορίζετε προσθέτοντάς τον πόρο Graph API μαζί με την έκδοση. Έχει νόημα να το τοποθετήσετε σε ένα αρχείο ρυθμίσεις εξωτερικής επειδή αυτά να αλλάξετε, όπως το API εξελίσσεται.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Στη συνέχεια, πρέπει να καθορίσετε παραμέτρους που δίνετε επίσης στην κλήση API του γραφήματος. Είναι *πολύ σημαντικά* ότι δεν τοποθετείτε τις παραμέτρους στο το τελικό σημείο πόρων επειδή που είναι ακύρωσε για όλους τους με διάκριση χαρακτήρες-URI κατά το χρόνο εκτέλεσης. Όλος ο κώδικας ερωτήματος πρέπει να παρέχεται με τις παραμέτρους.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Ίσως παρατηρήσετε αυτό καλεί μια `convertParamsToDictionary` μέθοδο που έχετε γράψει ακόμη. Ας Κάντε τώρα στο τέλος του αρχείου:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Στη συνέχεια, ας χρησιμοποιήσουμε το `NXOAuth2Request` μέθοδο για να επιστρέψετε δεδομένα από το API στη μορφή JSON.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Τέλος, ας δούμε πώς μπορείτε να επαναφέρετε τα δεδομένα για να το MasterViewController. Τα δεδομένα επιστρέφει ως σειριοποιημένο και πρέπει να είναι η αποσειριοποίηση και φόρτωση σε ένα αντικείμενο το οποίο μπορεί να εκμετάλλευση το MainViewController. Για το σκοπό αυτό περιλαμβάνει το σκελετό μια `User.m/h` αρχείο το οποίο δημιουργεί ένα αντικείμενο χρήστη. Μπορείτε να συμπληρώσετε αυτό το αντικείμενο χρήστη με πληροφορίες από το γράφημα.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>Εκτελέστε το δείγμα

Εάν έχετε χρησιμοποιήσει το σκελετό ή παρακολούθησης μαζί με το αναλυτικές οδηγίες για την εφαρμογή σας πρέπει τώρα να εκτελεστεί. Ξεκινήστε το simulator και κάντε κλικ στην επιλογή **Είσοδος** για να χρησιμοποιήσετε την εφαρμογή.

## <a name="get-security-updates-for-our-product"></a>Λήψη ενημερώσεων ασφαλείας για τα προϊόντα μας

Συνιστάται να λαμβάνετε ειδοποιήσεις όταν προκύπτουν περιστατικά ασφαλείας, αν επισκεφθείτε την τοποθεσία του [TechCenter ασφαλείας](https://technet.microsoft.com/security/dd252948) και εγγραφή συμβουλευτική ειδοποιήσεων ασφαλείας.
