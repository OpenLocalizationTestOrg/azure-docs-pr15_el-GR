<properties 
    pageTitle="Φέρουμε κοντά τα iOS προβολής Web με εγγενή δέσμευση Mobile iOS SDK" 
    description="Περιγράφει τον τρόπο δημιουργίας γέφυρα μεταξύ προβολής Web εκτέλεση Javascript και τα εγγενή iOS δέσμευση Mobile SDK"      
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
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>Φέρουμε κοντά τα iOS προβολής Web με εγγενή δέσμευση Mobile iOS SDK

> [AZURE.SELECTOR]
- [Android γέφυρας](mobile-engagement-bridge-webview-native-android.md)
- [iOS Bridge](mobile-engagement-bridge-webview-native-ios.md)

Ορισμένες εφαρμογές για κινητές συσκευές είναι σχεδιασμένες ως εφαρμογής υβριδική όπου ίδια την εφαρμογή αναπτύσσεται με χρήση εγγενούς iOS στόχο-C ανάπτυξης αλλά ορισμένα ή ακόμη και όλες τις οθόνες αποδίδονται μέσα σε μια προβολή Web iOS. Που μπορεί να εξακολουθεί να εκμετάλλευση δέσμευση Mobile iOS SDK μέσα σε αυτές οι εφαρμογές και αυτό το πρόγραμμα εκμάθησης περιγράφει τον τρόπο για να το κάνετε αυτό. 

Υπάρχουν δύο προσεγγίσεις για να επιτύχετε το εξής αν και και τα δύο είναι τεκμηριωμένη:

- Πρώτα ένα περιγράφεται σε αυτήν τη [σύνδεση](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) που περιλαμβάνει την καταχώρηση ενός `UIWebViewDelegate` σε προβολή web και προϊόντα-και-αμέσως-Ακύρωση μια αλλαγή θέσης κάνει στο Javascript. 
- Δεύτερο μία βασίζεται σε αυτήν την [περίοδο λειτουργίας WWDC 2013](https://developer.apple.com/videos/play/wwdc2013/615), προσέγγισης που είναι σαφές από την πρώτη γραμμή και που θα σας θα ακολουθήσετε για αυτόν τον οδηγό. Σημειώστε ότι αυτή η προσέγγιση λειτουργεί μόνο σε iOS7 και άνω. 

Ακολουθήστε τα παρακάτω βήματα για την iOS φέρουμε κοντά τα δείγματα:

1. Πρώτον, πρέπει να βεβαιωθείτε ότι έχουν περάσει μέσω μας [ξεκινήσατε το πρόγραμμα εκμάθησης](mobile-engagement-ios-get-started.md) για να ενοποιήσετε το iOS δέσμευση Mobile SDK στην εφαρμογή υβριδική. Προαιρετικά, μπορείτε επίσης να ενεργοποιήσετε δοκιμής καταγραφή ως εξής, έτσι ώστε να μπορείτε να δείτε τις μεθόδους SDK καθώς θα σας έναυσμα από την προβολή Web. 
    
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
            [EngagementAgent setTestLogEnabled:YES];
           ....
        }

2. Τώρα, βεβαιωθείτε ότι η εφαρμογή σας υβριδική έχει μια οθόνη με μια προβολή Web σε αυτό. Μπορείτε να την προσθέσετε για το `Main.storyboard` της εφαρμογής. 

3. Συσχετισμός αυτό προβολής Web με το **ViewController** κάνοντας κλικ και σύροντας την προβολή Web από την προβολή σκηνή ελεγκτή για να το `ViewController.h` επεξεργασία οθόνη, τοποθετώντας το ακριβώς κάτω από το `@interface` γραμμής. 

4. Αφού κάνετε αυτό, ένα παράθυρο διαλόγου θα εμφανιστεί που ζητά ένα όνομα. Παροχή του ονόματος ως **προβολής Web**. Σας `ViewController.h` αρχείου θα πρέπει να μοιάζει με την παρακάτω:

        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
        
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
        
        @end

5. Θα ενημερώσουμε το `ViewController.m` αρχείων αργότερα, αλλά πρώτα θα δημιουργήσουμε το bridge αρχείο το οποίο δημιουργεί μια περιτυλίγματος πάνω από κάποια που χρησιμοποιείται συχνά iOS δέσμευση Mobile SDK μεθόδους. Δημιουργήστε ένα νέο αρχείο κεφαλίδας που ονομάζεται **EngagementJsExports.h** που χρησιμοποιεί το `JSExport` μηχανισμού που περιγράφεται στην παραπάνω [περίοδο λειτουργίας](https://developer.apple.com/videos/play/wwdc2013/615) για να εμφανίσετε τις μεθόδους εγγενούς iOS. 

        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
        
        @protocol EngagementJsExports <JSExport>
        
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
        
        @end

        @interface EngagementJs : NSObject <EngagementJsExports>

        @end

6. Στη συνέχεια θα δημιουργήσουμε το δεύτερο τμήμα του αρχείου bridge. Δημιουργήστε ένα αρχείο που ονομάζεται **EngagementJsExports.m** που θα περιέχει την εφαρμογή δημιουργίας την πραγματική προγράμματα εξομοίωσης καλώντας τις μεθόδους SDK iOS δέσμευση Mobile. Επίσης σημείωση που θα σας κατά την ανάλυση του `extras` που διαβιβάζεται από την προβολή Web javascript και τοποθέτηση που σε μια `NSMutableDictionary` αντικειμένου για να διαβιβαστεί με τις κλήσεις μέθοδο δέσμευση SDK.  

        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
        
        @implementation EngagementJs
        
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
        
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
        
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
        
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
        
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
        
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
           
           return extras;
        }
        
        @end

5. Τώρα θα σας επιστρέψει το **ViewController.m** και να ενημερώσετε με τον ακόλουθο κώδικα: 
        
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
        
        @interface ViewController ()
        
        @end
        
        @implementation ViewController
        
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
        
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
           
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
        
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
           
           context[@"EngagementJs"] = [EngagementJs class];
        }
        
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
        
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
        
        @end

6. Λάβετε υπόψη τα εξής σημεία σχετικά με το αρχείο **ViewController.m** :

    - Στο το `loadWebView` μέθοδο, θα σας τη φόρτωση ένα τοπικό αρχείο HTML που ονομάζεται **LocalPage.html** κώδικας των οποίων θα σας θα το εξετάσετε Επόμενο. 
    - Στο το `webViewDidFinishLoad` μέθοδο, θα σας πιάνοντας το `JsContext` και συσχετίζοντας μας περιτυλίγματος τάξης με αυτό. Αυτό θα σας επιτρέψει κλήση μας περιτυλίγματος SDK μεθόδους χρησιμοποιώντας τη λαβή **EngagementJs** από την προβολή Web. 

7. Δημιουργήστε ένα αρχείο που ονομάζεται **LocalPage.html** με τον ακόλουθο κώδικα:

        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
               
               <script type="text/javascript">
               
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
               
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
                   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
                           
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
        
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
                           
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
               
           </head>
           <body>
               <h1>Bridge Tester</h1>
               
               <div id='engagement'>
                   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
                   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
                   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
                   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
               
               </div>
           </body>
        </html>

8. Λάβετε υπόψη τα εξής σημεία σχετικά με το παραπάνω αρχείο HTML:

    -   Περιέχει ένα σύνολο από όπου μπορείτε να παρέχετε δεδομένα για να χρησιμοποιηθούν ως ονόματα για το συμβάν, εργασία, του σφάλματος, AppInfo πλαίσια εισόδου. Όταν κάνετε κλικ στο κουμπί δίπλα της, μια κλήση πραγματοποιείται προς το Javascript που τελικά καλεί τις μεθόδους από το αρχείο γέφυρα για τη μεταβίβαση αυτή την κλήση του iOS δέσμευση Mobile SDK. 
    -   Θα σας Προσθήκη ετικετών σε ορισμένες στατική επιπλέον πληροφορίες για τα συμβάντα, εργασίες και ακόμα και σφάλματα με το οποίο επιδεικνύεται πώς αυτό μπορεί να γίνει. Αυτό επιπλέον πληροφορίες αποστέλλονται ως JSON μια συμβολοσειρά που, εάν κάνετε Διερεύνηση σε το `EngagementJsExports.m` αρχείων, είναι η ανάλυση και μαζί με αποστολή συμβάντα, εργασίες, σφάλματα. 
    -   Μια εργασία κινητό δέσμευση διακοπή με το όνομα που καθορίσατε στο πλαίσιο εισαγωγής, εκτελέστε για 10 δευτερόλεπτα και τερματισμός. 
    -   Ένα κινητό δέσμευση appinfo ή ετικέτες μεταβιβάζεται με 'customer_name' ως το στατικό κλειδί και την τιμή που καταχωρήσατε στην είσοδο ως η τιμή για την ετικέτα. 
 
9. Εκτελέστε την εφαρμογή και θα δείτε τα εξής. Τώρα, παρέχουν ορισμένες όνομα για ένα συμβάν δοκιμής όπως το εξής και κάντε κλικ στην επιλογή **Αποστολή** δίπλα από αυτό το. 

    ![][1]

10. Τώρα εάν μεταβείτε στην καρτέλα **Εποπτεία** των εφαρμογών και κοιτάξτε στην περιοχή **συμβάντα -> λεπτομέρειες**, θα δείτε αυτό το συμβάν που εμφανίζονται μαζί με τις στατική εφαρμογή-πληροφορίες που θα σας στέλνουν μηνύματα. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
