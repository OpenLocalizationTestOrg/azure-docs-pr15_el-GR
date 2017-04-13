<properties
    pageTitle="Azure ειδοποίηση διανομείς ειδοποιήστε τους χρήστες για Android με το .NET παρασκηνίου"
    description="Μάθετε πώς μπορείτε να στείλετε τις ειδοποιήσεις push για τους χρήστες στον Azure. Δείγματα κώδικα γραμμένο σε Java για Android"
    documentationCenter="android"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-notify-users-for-android-with-net-backend"></a>Azure ειδοποίηση διανομείς ειδοποιήστε τους χρήστες για Android με το .NET παρασκηνίου


[AZURE.INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

##<a name="overview"></a>Επισκόπηση

Υποστήριξη ειδοποιήσεων Push στο Azure επιτρέπει να αποκτήσετε πρόσβαση σε έναν εύκολο για χρήση multiplatform και κλιμάκωση ανάληψης push υποδομής που απλοποιεί σε μεγάλο βαθμό την εφαρμογή των ειδοποιήσεων push για καταναλωτές και για μεγάλες επιχειρήσεις στις εφαρμογές για κινητές συσκευές πλατφόρμες. Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων push σε μια συγκεκριμένη εφαρμογή χρήστη σε μια συγκεκριμένη συσκευή. Μια παρασκηνίου ASP.NET WebAPI χρησιμοποιείται για τον έλεγχο ταυτότητας προγράμματα-πελάτες και για τη δημιουργία ειδοποιήσεων, όπως φαίνεται στο θέμα οδηγίες [καταχωρήσεις από την εφαρμογή παρασκηνίου](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend). Αυτό το πρόγραμμα εκμάθησης δημιουργεί στην ενότητα ειδοποίησης που δημιουργήσατε στην εκμάθηση [Γρήγορα αποτελέσματα με ειδοποίηση διανομείς (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) .

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε δημιουργήσει και ρυθμιστεί το Κέντρο ειδοποίηση σας, όπως περιγράφεται σε [Γρήγορα αποτελέσματα με ειδοποίηση διανομείς (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="create-the-android-project"></a>Δημιουργία του Android έργου

Το επόμενο βήμα είναι να δημιουργήσετε την εφαρμογή Android.

1. Ακολουθήστε το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με ειδοποίηση διανομείς (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) για να δημιουργήσετε και να ρυθμίσετε την εφαρμογή σας για να λαμβάνετε ειδοποιήσεις push από GCM.

2. Ανοίξτε το αρχείο σας **res/layout/activity_main.xml** , αντικαταστήστε το με τους παρακάτω ορισμούς περιεχομένου.

    Αυτό προσθέτει νέα στοιχεία ελέγχου τον τύπο EditText για τη σύνδεση ως χρήστης. Επίσης, ένα πεδίο προστίθεται για μια ετικέτα όνομα χρήστη που θα αποτελεί μέρος των ειδοποιήσεων που στέλνετε:

        <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
            android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

        <EditText
            android:id="@+id/usernameText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/usernameHint"
            android:layout_above="@+id/passwordText"
            android:layout_alignParentEnd="true" />
        <EditText
            android:id="@+id/passwordText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/passwordHint"
            android:inputType="textPassword"
            android:layout_above="@+id/buttonLogin"
            android:layout_alignParentEnd="true" />
        <Button
            android:id="@+id/buttonLogin"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/loginButton"
            android:onClick="login"
            android:layout_above="@+id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:layout_marginBottom="24dp" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="WNS on"
            android:textOff="WNS off"
            android:id="@+id/toggleButtonWNS"
            android:layout_toLeftOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="GCM on"
            android:textOff="GCM off"
            android:id="@+id/toggleButtonGCM"
            android:checked="true"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="APNS on"
            android:textOff="APNS off"
            android:id="@+id/toggleButtonAPNS"
            android:layout_toRightOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessageTag"
            android:layout_below="@id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_tag_hint" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessage"
            android:layout_below="@+id/editTextNotificationMessageTag"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_hint" />
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/send_button"
            android:id="@+id/sendbutton"
            android:onClick="sendNotificationButtonOnClick"
            android:layout_below="@+id/editTextNotificationMessage"
            android:layout_centerHorizontal="true" />
        </RelativeLayout>



3. Ανοίξτε το αρχείο **res/values/strings.xml** σας και να αντικαταστήσετε το `send_button` ορισμού με τις ακόλουθες γραμμές που επανακαθορίσετε τη συμβολοσειρά για την `send_button` και προσθέστε συμβολοσειρές για τα άλλα στοιχεία ελέγχου:

        <string name="usernameHint">Username</string>
        <string name="passwordHint">Password</string>
        <string name="loginButton">1. Log in</string>
        <string name="send_button">2. Send Notification</string>
        <string name="notification_message_tag_hint">
            Recipient username tag
        </string>

    Διάταξη γραφικών σας main_activity.xml πρέπει τώρα να μοιάζει ως εξής:

    ![][A1]

4. Δημιουργία μιας νέας κλάσης με το όνομα **RegisterClient** στο ίδιο πακέτο ως σας `MainActivity` τάξης. Χρησιμοποιήστε τον κωδικό κάτω από το στοιχείο για το νέο αρχείο κλάσης.

        import java.io.IOException;
        import java.io.UnsupportedEncodingException;
        import java.util.Set;

        import org.apache.http.HttpResponse;
        import org.apache.http.HttpStatus;
        import org.apache.http.client.ClientProtocolException;
        import org.apache.http.client.HttpClient;
        import org.apache.http.client.methods.HttpPost;
        import org.apache.http.client.methods.HttpPut;
        import org.apache.http.client.methods.HttpUriRequest;
        import org.apache.http.entity.StringEntity;
        import org.apache.http.impl.client.DefaultHttpClient;
        import org.apache.http.util.EntityUtils;
        import org.json.JSONArray;
        import org.json.JSONException;
        import org.json.JSONObject;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.util.Log;

        public class RegisterClient {
            private static final String PREFS_NAME = "ANHSettings";
            private static final String REGID_SETTING_NAME = "ANHRegistrationId";
            private String Backend_Endpoint;
            SharedPreferences settings;
            protected HttpClient httpClient;
            private String authorizationHeader;

            public RegisterClient(Context context, String backendEnpoint) {
                super();
                this.settings = context.getSharedPreferences(PREFS_NAME, 0);
                httpClient =  new DefaultHttpClient();
                Backend_Endpoint = backendEnpoint + "/api/register";
            }

            public String getAuthorizationHeader() {
                return authorizationHeader;
            }

            public void setAuthorizationHeader(String authorizationHeader) {
                this.authorizationHeader = authorizationHeader;
            }

            public void register(String handle, Set<String> tags) throws ClientProtocolException, IOException, JSONException {
                String registrationId = retrieveRegistrationIdOrRequestNewOne(handle);

                JSONObject deviceInfo = new JSONObject();
                deviceInfo.put("Platform", "gcm");
                deviceInfo.put("Handle", handle);
                deviceInfo.put("Tags", new JSONArray(tags));

                int statusCode = upsertRegistration(registrationId, deviceInfo);

                if (statusCode == HttpStatus.SC_OK) {
                    return;
                } else if (statusCode == HttpStatus.SC_GONE){
                    settings.edit().remove(REGID_SETTING_NAME).commit();
                    registrationId = retrieveRegistrationIdOrRequestNewOne(handle);
                    statusCode = upsertRegistration(registrationId, deviceInfo);
                    if (statusCode != HttpStatus.SC_OK) {
                        Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                        throw new RuntimeException("Error upserting registration");
                    }
                } else {
                    Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                    throw new RuntimeException("Error upserting registration");
                }
            }

            private int upsertRegistration(String registrationId, JSONObject deviceInfo)
                    throws UnsupportedEncodingException, IOException,
                    ClientProtocolException {
                HttpPut request = new HttpPut(Backend_Endpoint+"/"+registrationId);
                request.setEntity(new StringEntity(deviceInfo.toString()));
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                request.addHeader("Content-Type", "application/json");
                HttpResponse response = httpClient.execute(request);
                int statusCode = response.getStatusLine().getStatusCode();
                return statusCode;
            }

            private String retrieveRegistrationIdOrRequestNewOne(String handle) throws ClientProtocolException, IOException {
                if (settings.contains(REGID_SETTING_NAME))
                    return settings.getString(REGID_SETTING_NAME, null);

                HttpUriRequest request = new HttpPost(Backend_Endpoint+"?handle="+handle);
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                HttpResponse response = httpClient.execute(request);
                if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                    Log.e("RegisterClient", "Error creating registrationId: " + response.getStatusLine().getStatusCode());
                    throw new RuntimeException("Error creating Notification Hubs registrationId");
                }
                String registrationId = EntityUtils.toString(response.getEntity());
                registrationId = registrationId.substring(1, registrationId.length()-1);

                settings.edit().putString(REGID_SETTING_NAME, registrationId).commit();

                return registrationId;
            }
        }

    Αυτό το στοιχείο υλοποιεί τα ΥΠΌΛΟΙΠΑ κλήσεις απαιτείται να επικοινωνήσετε με την εφαρμογή παρασκηνίου, για να εγγραφείτε για τις ειδοποιήσεις push. Αποθηκεύει επίσης τοπικά *registrationIds* που δημιουργήθηκε από την ενότητα "Ειδοποίηση" όπως περιγράφεται στο [καταχωρήσεις από την εφαρμογή παρασκηνίου](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend). Σημειώστε ότι χρησιμοποιεί ένα διακριτικό εξουσιοδότησης που είναι αποθηκευμένο στον τοπικό χώρο αποθήκευσης όταν κάνετε κλικ στο κουμπί **στο αρχείο καταγραφής** .

5. Στο σας `MainActivity` κλάσης καταργήσετε ή να σχολιάσετε το πεδίο private για `NotificationHub`, και να προσθέσετε ένα πεδίο για το `RegisterClient` κλάσης και μια συμβολοσειρά για το τελικό σημείο του υπολογιστή στο παρασκήνιο ASP.NET. Φροντίστε να αντικαταστήσετε `<Enter Your Backend Endpoint>` με το το τελικό σημείο πραγματική παρασκηνίου που λαμβάνονται προηγουμένως. Για παράδειγμα, `http://mybackend.azurewebsites.net`.


        //private NotificationHub hub;
        private RegisterClient registerClient;
        private static final String BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";


6. Στο σας `MainActivity` κλάση, με το `onCreate` μέθοδο, κατάργηση ή σχόλιο εκτός της προετοιμασίας των το `hub` πεδίο και την κλήση σε το `registerWithNotificationHubs` μέθοδο. Στη συνέχεια, προσθέστε κώδικα για την προετοιμασία της παρουσίας της το `RegisterClient` τάξης. Η μέθοδος πρέπει να περιέχει τις ακόλουθες γραμμές:

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            MyHandler.mainActivity = this;
            NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);
            gcm = GoogleCloudMessaging.getInstance(this);

            //hub = new NotificationHub(HubName, HubListenConnectionString, this);
            //registerWithNotificationHubs();

            registerClient = new RegisterClient(this, BACKEND_ENDPOINT);

            setContentView(R.layout.activity_main);
        }

7. Στο σας `MainActivity` κλάση, να διαγράψετε ή να σχολιάσετε ολόκληρη `registerWithNotificationHubs` μέθοδο. Αυτό δεν θα χρησιμοποιηθεί σε αυτό το πρόγραμμα εκμάθησης.

8. Προσθέστε τα ακόλουθα `import` προτάσεων για το αρχείο **MainActivity.java** .

        import android.widget.Button;
        import java.io.UnsupportedEncodingException;
        import android.content.Context;
        import java.util.HashSet;
        import android.widget.Toast;
        import org.apache.http.client.ClientProtocolException;
        import java.io.IOException;
        import org.apache.http.HttpStatus;


9. Στη συνέχεια, προσθέστε τις ακόλουθες μεθόδους για να χειριστείτε το κουμπί **σύνδεση στο** , κάντε κλικ στην επιλογή συμβάν και αποστολή ειδοποιήσεων push.

        @Override
        protected void onStart() {
            super.onStart();
            Button sendPush = (Button) findViewById(R.id.sendbutton);
            sendPush.setEnabled(false);
        }

        public void login(View view) throws UnsupportedEncodingException {
            this.registerClient.setAuthorizationHeader(getAuthorizationHeader());

            final Context context = this;
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        String regid = gcm.register(SENDER_ID);
                        registerClient.register(regid, new HashSet<String>());
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to register", e.getMessage());
                        return e;
                    }
                    return null;
                }

                protected void onPostExecute(Object result) {
                    Button sendPush = (Button) findViewById(R.id.sendbutton);
                    sendPush.setEnabled(true);
                    Toast.makeText(context, "Logged in and registered.",
                            Toast.LENGTH_LONG).show();
                }
            }.execute(null, null, null);
        }

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
            return basicAuthHeader;
        }

        /**
         * This method calls the ASP.NET WebAPI backend to send the notification message
         * to the platform notification service based on the pns parameter.
         *
         * @param pns     The platform notification service to send the notification message to. Must
         *                be one of the following ("wns", "gcm", "apns").
         * @param userTag The tag for the user who will receive the notification message. This string
         *                must not contain spaces or special characters.
         * @param message The notification message string. This string must include the double quotes
         *                to be used as JSON content.
         */
        public void sendPush(final String pns, final String userTag, final String message)
                throws ClientProtocolException, IOException {
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {

                        String uri = BACKEND_ENDPOINT + "/api/notifications";
                        uri += "?pns=" + pns;
                        uri += "&to_tag=" + userTag;

                        HttpPost request = new HttpPost(uri);
                        request.addHeader("Authorization", "Basic "+ getAuthorizationHeader());
                        request.setEntity(new StringEntity(message));
                        request.addHeader("Content-Type", "application/json");

                        HttpResponse response = new DefaultHttpClient().execute(request);

                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            DialogNotify("MainActivity - Error sending " + pns + " notification",
                                response.getStatusLine().toString());
                            throw new RuntimeException("Error sending notification");
                        }
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to send " + pns + " notification ", e.getMessage());
                        return e;
                    }

                    return null;
                }
            }.execute(null, null, null);
        }


    Το `login` πρόγραμμα χειρισμού για το κουμπί **σύνδεση στο** δημιουργεί ένα βασικό έλεγχο ταυτότητας διακριτικό χρησιμοποιώντας στην εισαγωγής όνομα χρήστη και τον κωδικό πρόσβασης (Σημειώστε ότι αυτό αντιπροσωπεύει οποιοδήποτε διακριτικό χρησιμοποιεί το συνδυασμό ελέγχου ταυτότητας) και, στη συνέχεια, που χρησιμοποιεί `RegisterClient` για να καλέσετε υπόβαθρο καταχώρησης.

    Το `sendPush` μέθοδο κλήσεις υπόβαθρο για να ενεργοποιήσει μια ειδοποίηση ασφαλή στο χρήστη με βάση την ετικέτα χρήστη. Ειδοποίηση πλατφόρμα την υπηρεσία που `sendPush` εξαρτάται από τους στόχους του `pns` συμβολοσειρά που εισήχθησαν στο.

10. Στο σας `MainActivity` κλάση, ενημερώστε το `sendNotificationButtonOnClick` μέθοδο για να καλέσετε το `sendPush` μέθοδο με το χρήστη επιλεγμένο υπηρεσίες ειδοποίηση πλατφόρμας ως εξής.

        /**
         * Send Notification button click handler. This method sends the push notification
         * message to each platform selected.
         *
         * @param v The view
         */
        public void sendNotificationButtonOnClick(View v)
                throws ClientProtocolException, IOException {

            String nhMessageTag = ((EditText) findViewById(R.id.editTextNotificationMessageTag))
                    .getText().toString();
            String nhMessage = ((EditText) findViewById(R.id.editTextNotificationMessage))
                    .getText().toString();

            // JSON String
            nhMessage = "\"" + nhMessage + "\"";

            if (((ToggleButton)findViewById(R.id.toggleButtonWNS)).isChecked())
            {
                sendPush("wns", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonGCM)).isChecked())
            {
                sendPush("gcm", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonAPNS)).isChecked())
            {
                sendPush("apns", nhMessageTag, nhMessage);
            }
        }



## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή


1. Εκτελέστε την εφαρμογή σε μια συσκευή ή ένα προσομοίωσης με Android Studio.

2. Στην εφαρμογή Android, πληκτρολογήστε ένα όνομα χρήστη και τον κωδικό πρόσβασης. Πρέπει να και τα δύο είναι η ίδια τιμή συμβολοσειράς και δεν πρέπει να περιέχουν κενά διαστήματα ή ειδικούς χαρακτήρες.

3. Στην εφαρμογή Android, κάντε κλικ στην επιλογή **σύνδεση στο**. Αναμονή για μια αναδυόμενη μήνυμα που αναφέρει **καταγεγραμμένα και καταχωρημένες**. Αυτό θα ενεργοποιήσει το κουμπί **Αποστολή ειδοποίησης** .

    ![][A2]

4. Κάντε κλικ στην επιλογή το στοιχείο εναλλαγής κουμπιά για να ενεργοποιήσετε όλες τις πλατφόρμες όπου έχετε εκτελέσατε την εφαρμογή και καταχωρηθεί ένα χρήστη.
5. Πληκτρολογήστε το όνομα του χρήστη που θα λάβουν το μήνυμα ειδοποίησης. Αυτός ο χρήστης πρέπει να έχει εγγραφεί για τις ειδοποιήσεις στις συσκευές προορισμού.

6. Πληκτρολογήστε ένα μήνυμα για το χρήστη να λαμβάνει ως ένα μήνυμα ειδοποίησης push.
7. Κάντε κλικ στην επιλογή **Αποστολή ειδοποίησης**.  Κάθε συσκευή που έχει μια εγγραφή με την αντίστοιχη ετικέτα όνομα χρήστη θα λάβουν την ειδοποίηση push.


[A1]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users.png
[A2]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users-enter-password.png
