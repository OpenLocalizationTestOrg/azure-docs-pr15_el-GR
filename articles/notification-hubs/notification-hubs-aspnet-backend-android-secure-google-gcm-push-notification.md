<properties
    pageTitle="Αποστολή ειδοποιήσεων Push ασφαλούς με διανομείς Azure ειδοποίησης"
    description="Μάθετε πώς μπορείτε να στείλετε τις ειδοποιήσεις push ασφαλή για Android εφαρμογής από το Azure. Δείγματα κώδικα γραμμένο σε Java και C#."
    documentationCenter="android"
    keywords="ειδοποίηση, τις ειδοποιήσεις push, Push push μηνύματα, τις ειδοποιήσεις android push"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

#<a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Αποστολή ειδοποιήσεων Push ασφαλούς με διανομείς Azure ειδοποίησης

> [AZURE.SELECTOR]
- [Παγκόσμια των Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

##<a name="overview"></a>Επισκόπηση

> [AZURE.IMPORTANT] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Υποστήριξη ειδοποιήσεων Push στο Microsoft Azure επιτρέπει να αποκτήσετε πρόσβαση σε ένα εύκολο για χρήση, multiplatform, κλιμάκωση ανάληψης push υποδομής μηνύματος που απλοποιεί σε μεγάλο βαθμό την εφαρμογή των ειδοποιήσεων push για καταναλωτές και για μεγάλες επιχειρήσεις στις εφαρμογές για κινητές συσκευές πλατφόρμες.

Λόγω ρυθμιστικές ή τους περιορισμούς ασφαλείας, μερικές φορές μια εφαρμογή μπορεί να θέλετε να συμπεριλάβετε κάτι στην ειδοποίηση που δεν είναι δυνατό να μεταδοθούν μέσω της υποδομής κοινοποίηση τυπική push. Αυτό το πρόγραμμα εκμάθησης περιγράφει τον τρόπο για να επιτύχετε το ίδιο εμπειρία με την αποστολή ευαίσθητες πληροφορίες μέσω ασφαλή, με έλεγχο ταυτότητας σύνδεσης μεταξύ της συσκευής Android προγράμματος-πελάτη και την εφαρμογή παρασκηνίου.

Σε υψηλό επίπεδο, η ροή είναι ως εξής:

1. Η εφαρμογή παρασκηνίου:
    - Ασφαλής φορτίο αποθηκεύονται στη βάση δεδομένων παρασκηνίου.
    - Στέλνει το Αναγνωριστικό του αυτή η ειδοποίηση στη συσκευή Android (καμία ασφαλούς πληροφορία δεν αποστέλλεται).
2. Η εφαρμογή από τη συσκευή, όταν λάβετε την ειδοποίηση:
    - Η συσκευή Android επαφών του παρασκηνίου που ζητά το φορτίο ασφαλή.
    - Η εφαρμογή μπορεί να εμφανίσει το φορτίο ως μια ειδοποίηση στη συσκευή.

Είναι σημαντικό να λάβετε υπόψη ότι το προηγούμενο ροής (και σε αυτό το πρόγραμμα εκμάθησης), θα σας λαμβάνεται ως δεδομένο ότι η συσκευή αποθηκεύει ένα διακριτικό ελέγχου ταυτότητας στον τοπικό χώρο αποθήκευσης, αφού ο χρήστης συνδέεται. Αυτό εγγυάται μια εντελώς απρόσκοπτη εμπειρία, όπως η συσκευή να ανακτήσετε την ειδοποίηση ασφαλούς φορτίο χρησιμοποιώντας αυτό το διακριτικό. Εάν η εφαρμογή σας δεν αποθηκεύει κωδικοί ελέγχου ταυτότητας στη συσκευή ή εάν αυτά τα διακριτικά μπορεί να έχει λήξει, την εφαρμογή συσκευή, μόλις λάβετε την ειδοποίηση push πρέπει να εμφανίζει μια γενική ειδοποίηση ερώτηση στο χρήστη για να ξεκινήσετε την εφαρμογή. Η εφαρμογή πραγματοποιεί έλεγχο ταυτότητας χρήστη, στη συνέχεια, και εμφανίζει το φορτίο ειδοποίησης.

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να στείλετε τις ειδοποιήσεις push ασφαλή. Δημιουργεί στην το πρόγραμμα εκμάθησης [Ειδοποιήστε τους χρήστες](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) , έτσι θα πρέπει να ολοκληρώσετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης πρώτα εάν δεν το έχετε κάνει ήδη.

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε δημιουργήσει και ρυθμιστεί το Κέντρο ειδοποίηση σας, όπως περιγράφεται σε [Γρήγορα αποτελέσματα με ειδοποίηση διανομείς (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Τροποποίηση του Android έργου

Τώρα που τροποποιήσατε σας εφαρμογή παρασκηνίου για να στείλετε απλώς το *αναγνωριστικό* του push ειδοποίησης, πρέπει να αλλάξετε την εφαρμογή σας Android για να χειριστείτε αυτήν την ειδοποίηση και επιστροφή κλήσης σας παρασκηνίου για να ανακτήσετε το ασφαλές μήνυμα να εμφανίζεται.
Για να γίνει αυτό, πρέπει να βεβαιωθείτε ότι η εφαρμογή σας Android γνωρίζει πώς να τον έλεγχο ταυτότητας του εαυτού με σας παρασκηνίου όταν λαμβάνει τις ειδοποιήσεις push.

Τώρα, θα σας θα τροποποιήσετε ροής της *σύνδεσης* για να αποθηκεύσετε την τιμή κεφαλίδας ελέγχου ταυτότητας στις προτιμήσεις "Κοινόχρηστα" της εφαρμογής. Μηχανισμοί ανάλογος μπορεί να χρησιμοποιηθεί για την αποθήκευση οποιοδήποτε διακριτικό ελέγχου ταυτότητας (π.χ. OAuth διακριτικά) που η εφαρμογή θα πρέπει να χρησιμοποιήσετε χωρίς που απαιτεί διαπιστευτήρια χρήστη.

1. Στο έργο σας εφαρμογή Android, προσθέστε τις ακόλουθες σταθερές στο επάνω μέρος της κλάσης **MainActivity** :

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. Εξακολουθείτε να στην τάξη **MainActivity** , ενημερώστε το `getAuthorizationHeader()` μέθοδο ώστε να περιέχει τον ακόλουθο κώδικα:

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. Προσθέστε τα ακόλουθα `import` δηλώσεις στο επάνω μέρος του αρχείου **MainActivity** :

        import android.content.SharedPreferences;

Τώρα θα αλλάξουμε το πρόγραμμα χειρισμού που ονομάζεται κατά τη λήψη την ειδοποίηση.

4. Στο το **MyHandler** κλάση αλλαγή το `OnReceive()` μέθοδο για να συμπεριλάβετε:

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. Στη συνέχεια, προσθέστε το `retrieveNotification()` μέθοδο, αντικαθιστώντας το σύμβολο κράτησης θέσης `{back-end endpoint}` με το τελικό σημείο παρασκηνίου που λαμβάνονται κατά την ανάπτυξη του παρασκηνίου:

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


Αυτή η μέθοδος κλήσεις σας εφαρμογή παρασκηνίου για να ανακτήσετε το περιεχόμενο ειδοποίηση χρησιμοποιώντας τα διαπιστευτήρια που είναι αποθηκευμένα στις προτιμήσεις κοινόχρηστο και το εμφανίζει ως κανονική ειδοποίηση. Η ειδοποίηση εμφανίζεται για να ο χρήστης εφαρμογή ακριβώς όπως οποιαδήποτε άλλα ειδοποιήσεων push.

Σημειώστε ότι είναι προτιμότερο να χειρίζεται τις περιπτώσεις δεν υπάρχει ιδιότητα κεφαλίδα ελέγχου ταυτότητας ή απόρριψη από το παρασκηνίου. Το συγκεκριμένο χειρισμό από αυτές τις περιπτώσεις εξαρτώνται από κυρίως την εμπειρία χρήστη προορισμού. Μία επιλογή είναι να εμφανίζεται μια ειδοποίηση με ένα γενικό μήνυμα για το χρήστη για τον έλεγχο ταυτότητας για να ανακτήσετε την πραγματική ειδοποίηση.

## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή

Για να εκτελέσετε την εφαρμογή, κάντε τα εξής:

1. Βεβαιωθείτε ότι έχει αναπτυχθεί **AppBackend** στο Azure. Εάν χρησιμοποιείτε το Visual Studio, εκτελέστε την εφαρμογή Web API **AppBackend** . Εμφανίζεται μια σελίδα web ASP.NET.

2. Στο Έκλειψη, εκτελέστε την εφαρμογή σε μια φυσική συσκευή Android ή το προσομοίωσης.

3. Στο Android εφαρμογή περιβάλλοντος εργασίας Χρήστη, πληκτρολογήστε ένα όνομα χρήστη και τον κωδικό πρόσβασης. Αυτά μπορεί να είναι οποιαδήποτε συμβολοσειρά, αλλά ο χρήστης πρέπει να έχει την ίδια τιμή.

4. Στην Android εφαρμογή περιβάλλοντος εργασίας Χρήστη, κάντε κλικ στην επιλογή **σύνδεση στο**. Στη συνέχεια, κάντε κλικ στην επιλογή **Αποστολή push**.
