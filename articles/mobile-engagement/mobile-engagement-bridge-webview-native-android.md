<properties 
    pageTitle="Android προβολής Web γέφυρα με εγγενή Mobile δέσμευση Android SDK" 
    description="Περιγράφει τον τρόπο δημιουργίας γέφυρα μεταξύ προβολής Web εκτέλεση Javascript και το εγγενές SDK Android Mobile δέσμευση"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Android προβολής Web γέφυρα με εγγενή Mobile δέσμευση Android SDK

> [AZURE.SELECTOR]
- [Android γέφυρας](mobile-engagement-bridge-webview-native-android.md)
- [iOS Bridge](mobile-engagement-bridge-webview-native-ios.md)

Ορισμένες εφαρμογές για κινητές συσκευές είναι σχεδιασμένες ως εφαρμογής υβριδική όπου ίδια την εφαρμογή αναπτύσσεται με χρήση εγγενούς Android ανάπτυξη, αλλά ορισμένα ή ακόμη και όλες τις οθόνες αποδίδονται μέσα σε μια προβολή Web Android. Μπορείτε εξακολουθούν να μπορούν να εκμετάλλευση Android SDK δέσμευση Mobile μέσα σε αυτές οι εφαρμογές και αυτό το πρόγραμμα εκμάθησης περιγράφει τον τρόπο για να το κάνετε αυτό. Το παρακάτω δείγμα κώδικα βασίζεται στην τεκμηρίωση του Android [εδώ](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Περιγράφει τον τρόπο αυτήν την προσέγγιση τεκμηρίωση θα μπορούσαν να χρησιμοποιηθούν για την υλοποίηση το ίδιο για τις μεθόδους που χρησιμοποιούνται συχνά Mobile δέσμευση Android SDK του τέτοια ώστε μια προβολή Web από μια εφαρμογή υβριδική μπορεί επίσης να προκαλέσει αιτήσεις για την παρακολούθηση συμβάντων, εργασίες, σφάλματα, πληροφορίες app ενώ σωληνώσεων τους μέσω μας Android SDK. 

1. Πρώτον, πρέπει να βεβαιωθείτε ότι έχουν περάσει μέσω μας [ξεκινήσατε το πρόγραμμα εκμάθησης](mobile-engagement-android-get-started.md) για την ενσωμάτωση του Android SDK δέσμευση Mobile στην εφαρμογή υβριδική. Μόλις γίνει αυτό, σας `OnCreate` μέθοδο θα μοιάζει με την παρακάτω.  
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }

2. Τώρα, βεβαιωθείτε ότι η εφαρμογή σας υβριδική έχει μια οθόνη με μια προβολή Web σε αυτό. Τον κώδικα για αυτό θα είναι παρόμοιο με το ακόλουθο όπου θα σας τη φόρτωση ενός τοπικού HTML αρχείο **Sample.html** στο την προβολή Web σε το `onCreate` μέθοδο της οθόνης σας. 

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }

        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }

3. Τώρα Δημιουργήστε ένα αρχείο γέφυρα που ονομάζεται **WebAppInterface** , η οποία δημιουργεί μια περιτυλίγματος σε σύγκριση με ορισμένες που χρησιμοποιείται συχνά μεθόδους Mobile δέσμευση Android SDK χρησιμοποιώντας το `@JavascriptInterface` προσέγγιση που περιγράφεται στην [τεκμηρίωση του Android](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):

        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
        
        import com.microsoft.azure.engagement.EngagementAgent;
        
        import org.json.JSONArray;
        import org.json.JSONObject;
        
        public class WebAppInterface {
            Context mContext;
        
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
        
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
        
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
        
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
        
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  

4. Μόλις δημιουργήσαμε παραπάνω αρχείου bridge, πρέπει να βεβαιωθείτε ότι έχει συσχετιστεί με την προβολή Web. Για να συμβεί αυτό, θα πρέπει να επεξεργαστείτε το `SetWebview` μέθοδο έτσι ώστε να μοιάζει με τα εξής:

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }

5. Στο τμήμα παραπάνω, θα σας ονομάζεται `addJavascriptInterface` για να συσχετίσετε μας γέφυρα τάξης με μας προβολής Web και επίσης να δημιουργήσετε μια λαβή που ονομάζεται **EngagementJs** για να καλέσετε τις μεθόδους από το αρχείο γέφυρα. 

6. Τώρα δημιουργήστε το ακόλουθο αρχείο που ονομάζεται **Sample.html** στο έργο σας σε ένα φάκελο που ονομάζεται **περιουσιακών στοιχείων** που φορτώνεται στη την προβολή Web και όπου θα ονομάζουμε τις μεθόδους από το αρχείο γέφυρα.

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
                      log('window.onerror: ' + err);
                    }
        
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
        
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
        
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
        
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
        
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
        
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
        
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
        
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>

8. Λάβετε υπόψη τα εξής σημεία σχετικά με το παραπάνω αρχείο HTML:

    -   Περιέχει ένα σύνολο από όπου μπορείτε να παρέχετε δεδομένα για να χρησιμοποιηθούν ως ονόματα για το συμβάν, εργασία, του σφάλματος, AppInfo πλαίσια εισόδου. Όταν κάνετε κλικ στο κουμπί δίπλα της, μια κλήση πραγματοποιείται προς το Javascript που τελικά καλεί τις μεθόδους από το αρχείο γέφυρα για τη μεταβίβαση αυτής της κλήσης στο κινητό SDK Android δέσμευση. 
    -   Θα σας Προσθήκη ετικετών σε ορισμένες στατική επιπλέον πληροφορίες για τα συμβάντα, εργασίες και ακόμα και σφάλματα με το οποίο επιδεικνύεται πώς αυτό μπορεί να γίνει. Αυτό επιπλέον πληροφορίες αποστέλλονται ως JSON μια συμβολοσειρά που, εάν κάνετε Διερεύνηση σε το `WebAppInterface` αρχείων, είναι η ανάλυση και να τοποθετήσετε σε συσκευή Android `Bundle` και μεταβιβάζεται μαζί με αποστολή συμβάντα, εργασίες, σφάλματα. 
    -   Μια εργασία κινητό δέσμευση διακοπή με το όνομα που καθορίσατε στο πλαίσιο εισαγωγής, εκτελέστε για 10 δευτερόλεπτα και τερματισμός. 
    -   Ένα κινητό δέσμευση appinfo ή ετικέτες μεταβιβάζεται με 'customer_name' ως το στατικό κλειδί και την τιμή που καταχωρήσατε στην είσοδο ως η τιμή για την ετικέτα. 
 
9. Εκτελέστε την εφαρμογή και θα δείτε τα εξής. Τώρα, παρέχουν ορισμένες όνομα για ένα συμβάν δοκιμής όπως το εξής και κάντε κλικ στην επιλογή **Αποστολή** κάτω από αυτό. 

    ![][1]

10. Τώρα εάν μεταβείτε στην καρτέλα **Εποπτεία** των εφαρμογών και κοιτάξτε στην περιοχή **συμβάντα -> λεπτομέρειες**, θα δείτε αυτό το συμβάν που εμφανίζονται μαζί με τις στατική εφαρμογή-πληροφορίες που θα σας στέλνουν μηνύματα. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
