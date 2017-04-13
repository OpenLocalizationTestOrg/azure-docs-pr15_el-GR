<properties
    pageTitle="Azure AD iOS γρήγορα αποτελέσματα | Microsoft Azure"
    description="Πώς να δημιουργείτε μια εφαρμογή iOS που ενοποιείται με το Azure AD για είσοδο στο και κλήσεις Azure AD προστατευμένο APIs χρησιμοποιώντας OAuth."
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>Ενοποίηση του Azure AD σε μια εφαρμογή για iOS

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure AD παρέχει τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory ή ADAL, για συσκευές iOS που πρέπει να αποκτήσετε πρόσβαση σε προστατευμένο πόρους.  Σκοπός του ADAL κατά τη διάρκεια ζωής της είναι ώστε να μπορείτε εύκολα για την εφαρμογή σας για να λάβετε διακριτικά πρόσβασης.  Για μια επίδειξη απλώς πόσο εύκολο είναι, εδώ θα σας θα δημιουργήσετε μια λίστα εκκρεμών εργασιών C στόχο εφαρμογή που:

-   Λαμβάνει πρόσβαση διακριτικά για την κλήση του API Azure AD Graph χρησιμοποιεί το [πρωτόκολλο ελέγχου ταυτότητας διακριτικό 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Πραγματοποιεί αναζήτηση σε έναν κατάλογο για τους χρήστες με ένα καθορισμένο ψευδώνυμο.

Για να δημιουργήσετε την πλήρη εφαρμογή εργασία, θα πρέπει να:

2. Καταχωρήστε την εφαρμογή του Azure AD.
3. Εγκατάσταση και ρύθμιση παραμέτρων του ADAL.
5. Χρησιμοποιήστε ADAL για να λάβετε τα διακριτικά από το Azure AD.

Για να ξεκινήσετε, [κάντε λήψη του σκελετό εφαρμογής](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) ή [λήψη ολοκληρωμένο δείγμα](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  Θα χρειαστεί επίσης ένα μισθωτή του Azure AD με την οποία μπορείτε να δημιουργήσετε χρήστες και να καταχωρήσετε μια εφαρμογή του.  Εάν δεν έχετε ήδη ένα μισθωτή, [Μάθετε πώς μπορείτε να αποκτήσετε](active-directory-howto-tenant.md).

> [AZURE.TIP] Δοκιμάστε την προεπισκόπηση του μας νέα [πύλη για προγραμματιστές](https://identity.microsoft.com/Docs/iOS) που θα σας βοηθήσει να ξεκινήσετε με το Azure Active Directory σε λίγα λεπτά!  Η πύλη για προγραμματιστές θα σας καθοδηγήσει της διαδικασίας την καταχώρηση εφαρμογής και ενοποίηση Azure AD μέσα στον κώδικα.  Όταν ολοκληρώσετε την εργασία, θα έχετε μια απλή εφαρμογή που μπορούν να ελέγχουν την ταυτότητα χρηστών στο μισθωτή σας και έναν υπολογιστή στο παρασκήνιο που μπορούν να αποδέχονται τα διακριτικά και εκτελεί επικύρωση. 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1. Καθορίστε τι θα είναι το URI ανακατεύθυνση για iOS*

Για να εκκίνηση με ασφαλή τρόπο σε συγκεκριμένα σενάρια SSO τις εφαρμογές σας ζητήσουμε να δημιουργήσετε ένα **URI ανακατεύθυνση** σε συγκεκριμένη μορφή. Ένα URI ανακατεύθυνση χρησιμοποιείται για να βεβαιωθείτε ότι τα διακριτικά επιστρέφει στην σωστή εφαρμογή που γίνεται ερώτηση για τους.

Η μορφή iOS για ένα URI ανακατεύθυνση είναι:

```
<app-scheme>://<bundle-id>
```

-   **aap συνδυασμού** - αυτό είναι καταχωρημένος στο έργο σας XCode. Είναι πώς άλλες εφαρμογές μπορούν να καλέσουν. Μπορείτε να βρείτε αυτό στην περιοχή Info.plist -> τύποι διεύθυνση URL -> αναγνωριστικό διεύθυνση URL. Θα πρέπει να μπορείτε να δημιουργήσετε ένα, εάν δεν έχετε ήδη ένα ή περισσότερα έχει ρυθμιστεί.
-   **αναγνωριστικό πακέτου** - αυτό είναι το αναγνωριστικό πακέτου βρίσκονται στην περιοχή "ταυτότητα" καταργήστε τις ρυθμίσεις του project στο XCode.

Θα είναι ένα παράδειγμα για αυτόν τον κωδικό γρήγορη έναρξη: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2. καταχώρηση της εφαρμογής DirectorySearcher*
Για να ενεργοποιήσετε την εφαρμογή σας για να λάβετε τα διακριτικά, θα πρέπει πρώτα να καταχωρήσετε στο μισθωτή σας Azure AD και να εκχωρήσετε το δικαίωμα πρόσβασης το API Azure AD Graph:

-   Πραγματοποιήστε είσοδο στο την πύλη διαχείρισης Azure
-   Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην **Υπηρεσία καταλόγου Active Directory**
-   Επιλέξτε έναν μισθωτή στην οποία θέλετε να καταχωρήσετε την εφαρμογή.
-   Κάντε κλικ στην καρτέλα **εφαρμογές** και κάντε κλικ στην επιλογή **Προσθήκη** στο το σχέδιο κάτω.
-   Ακολουθήστε τις οδηγίες και να δημιουργήσετε μια νέα **Εφαρμογή εγγενής προγράμματος-πελάτη**.
    -   Το **όνομα** της εφαρμογής θα περιγράφει την εφαρμογή σας στους τελικούς χρήστες
    -   Η **Ανακατεύθυνση Uri** είναι ένας συνδυασμός συνδυασμό και συμβολοσειρά που Azure AD θα χρησιμοποιήσετε για να λάβετε απαντήσεις διακριτικού.  Εισαγάγετε μια συγκεκριμένη τιμή στην εφαρμογή σας βάσει των παραπάνω πληροφοριών.
-   Όταν ολοκληρώσετε την καταχώρηση, AAD θα εκχωρήσετε την εφαρμογή σας ένα αναγνωριστικό μοναδικό προγράμματος-πελάτη.  Θα χρειαστείτε αυτή την τιμή στις επόμενες ενότητες, επομένως, αντιγράψτε την από την καρτέλα **Ρύθμιση παραμέτρων** .
- Επίσης στην καρτέλα **Ρύθμιση παραμέτρων** , εντοπίστε την ενότητα "Δικαιώματα σε άλλες εφαρμογές".  Για την εφαρμογή "Azure Active Directory", προσθέστε το δικαίωμα **πρόσβασης σας εταιρείας καταλόγου** στην περιοχή **Δικαιώματα με ανάθεση**.  Αυτό θα ενεργοποιήσει την εφαρμογή σας ερώτημα για το API Graph για τους χρήστες.

## <a name="3-install--configure-adal"></a>*3. εγκατάσταση και ρύθμιση παραμέτρων ADAL*
Τώρα που έχετε μια εφαρμογή στο Azure AD, μπορείτε να εγκαταστήσετε ADAL και να γράψετε τον κωδικό που σχετίζονται με την ταυτότητα.  Με τη σειρά για ADAL να μπορούν να επικοινωνούν με Azure AD, πρέπει να παρέχει ορισμένες πληροφορίες σχετικά με την εγγραφή σας εφαρμογή.
-   Ξεκινήστε προσθέτοντας ADAL στο έργο DirectorySearcher χρησιμοποιώντας Cocapods.

```
$ vi Podfile
```
Προσθέστε τα ακόλουθα για να podfile αυτό:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Η φόρτωση τώρα το podfile χρησιμοποιώντας cocoapods. Αυτό θα δημιουργήσει ένα νέο χώρο εργασίας XCode θα φορτώσετε.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   Γρήγορη έναρξη του έργου, ανοίξτε το αρχείο plist `settings.plist`.  Αντικαταστήστε τις τιμές από τα στοιχεία της ενότητας για να εμφανίζουν τις τιμές που εισαγάγει στην πύλη του Azure.  Ο κώδικας θα αναφοράς αυτές τις τιμές κάθε φορά που χρησιμοποιεί ADAL.
    -   Η `tenant` είναι ο τομέας του μισθωτή του Azure AD, π.χ. contoso.onmicrosoft.com
    -   Η `clientId` είναι η clientId της εφαρμογής σας που αντιγράψατε από την πύλη.
    -   Η `redirectUri` είναι η ανακατεύθυνση διεύθυνσης url που έχουν καταχωρηθεί στην πύλη.

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4. Χρησιμοποιήστε ADAL για να λάβετε τα διακριτικά από AAD*
Η βασική αρχή πίσω από ADAL είναι ότι κάθε φορά που την εφαρμογή σας χρειάζεται ένα διακριτικό πρόσβασης, απλώς καλεί μια completionBlock `+(void) getToken : `, και ADAL κάνει τα υπόλοιπα.  

-   Στο το `QuickStart` το έργο, Άνοιγμα `GraphAPICaller.m` και εντοπίστε το `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` σχόλιο κοντά στο επάνω μέρος.  Αυτό είναι όπου περάσετε ADAL τις συντεταγμένες μέσω ενός CompletionBlock για επικοινωνία με το Azure AD και ενημερώστε το cache τα διακριτικά με τον τρόπο.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- Τώρα, πρέπει να χρησιμοποιήσετε αυτό το διακριτικό για να πραγματοποιήσετε αναζήτηση για τους χρήστες στο γράφημα. Βρείτε το `// TODO: implement SearchUsersList` commentThis μέθοδος δημιουργεί ένα αίτημα GET για το API Azure AD Graph ερώτημα για τους χρήστες των οποίων UPN αρχίζει με τον όρο αναζήτησης που δίνεται.  Αλλά για να υποβάλετε ένα ερώτημα το API Graph, πρέπει να συμπεριλάβετε μια access_token στο το `Authorization` κεφαλίδα της αίτησης - αυτό είναι ADAL μπορεί να βοηθήσει.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

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
                         s.name =[keyValuePairs valueForKey:@"givenName"];

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
     }];

}

```
- Όταν η εφαρμογή σας ζητάει ενός διακριτικού καλώντας `getToken(...)`, ADAL θα επιχειρήσει να επιστρέψει ένα διακριτικό χωρίς να ζητήσετε από το χρήστη για τα διαπιστευτήρια.  Εάν ADAL Καθορίζει ότι ο χρήστης πρέπει να εισέλθετε για να λάβετε ένα διακριτικό, θα εμφανιστεί ένα παράθυρο διαλόγου σύνδεση, συλλογής τα διαπιστευτήρια του χρήστη και επιστροφή ενός διακριτικού κατά την επιτυχή έλεγχο ταυτότητας.  Εάν το ADAL είναι δυνατό να επιστρέψει ενός διακριτικού για οποιονδήποτε λόγο, θα εμφανίσουν μια `AdalException`.
- Σημειώστε ότι το `AuthenticationResult` αντικείμενο περιέχει ένα `tokenCacheStoreItem` αντικείμενο το οποίο μπορεί να χρησιμοποιηθεί για τη συλλογή πληροφοριών της εφαρμογής σας μπορεί να χρειαστεί.  Στη Γρήγορη εκκίνηση του, `tokenCacheStoreItem` χρησιμοποιείται για να προσδιορίσετε εάν έχει ήδη παρουσιαστεί authenitcation.


## <a name="step-5-build-and-run-the-application"></a>Βήμα 5: Δημιουργία και εκτέλεση της εφαρμογής



Συγχαρητήρια! Τώρα έχετε μια εφαρμογή iOS εργασία που έχει τη δυνατότητα να ελέγχουν την ταυτότητα χρηστών, καλέστε με ασφάλεια APIs Web χρησιμοποιώντας διακριτικό 2.0 και λήψη βασικών πληροφοριών σχετικά με το χρήστη.  Εάν δεν το έχετε κάνει ήδη, τώρα είναι η ώρα για τη συμπλήρωση του μισθωτή με ορισμένων χρηστών.  Εκτέλεση της εφαρμογής σας γρήγορης έναρξης και συνδεθείτε με έναν από αυτούς τους χρήστες.  Αναζήτηση για άλλους χρήστες με βάση τους UPN.  Κλείστε την εφαρμογή και εκτελέστε ξανά.  Παρατηρήστε πώς παραμένει ανέπαφος περιόδου λειτουργίας του χρήστη.

ADAL διευκολύνει να ενσωματώσετε όλες αυτές τις συνήθεις δυνατότητες ταυτότητας στην εφαρμογή σας.  Το αναλαμβάνει όλες τις εργασίες dirty για εσάς: Διαχείριση cache, η υποστήριξη πρωτοκόλλου διακριτικό, παρουσίαση στο χρήστη με μια σύνδεση περιβάλλοντος εργασίας Χρήστη, η ανανέωση έχει λήξει διακριτικά και πολλά άλλα.  Όλα όσα χρειάζεται να γνωρίζετε είναι μια μεμονωμένη κλήση API, `getToken`.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) παρέχεται [εδώ](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>Πρόσθετα σενάρια
Τώρα, μπορείτε να μετακινήσετε επιπλέον σενάρια.  Εάν θέλετε, μπορείτε να δοκιμάσετε:

- [Ασφαλούς Web Node.JS API με Azure AD](active-directory-devquickstarts-webapi-nodejs.md)
- Μάθετε [πώς μπορείτε να ενεργοποιήσετε την εφαρμογή σταυρό SSO σε iOS χρησιμοποιώντας ADAL](active-directory-sso-ios.md)  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
