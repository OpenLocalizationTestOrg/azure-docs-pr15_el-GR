<properties 
    pageTitle="Ενοποίηση Android SDK Azure δέσμευση κινητές συσκευές" 
    description="Πιο πρόσφατες ενημερώσεις και διαδικασίες για Android SDK για δέσμευση Mobile Azure"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="upgrade-procedures"></a>Διαδικασίες αναβάθμισης

Εάν έχετε ήδη έχετε συνδέσει μια παλαιότερη έκδοση του SDK μας στην εφαρμογή σας, πρέπει να λάβετε υπόψη σας τα ακόλουθα σημεία κατά την αναβάθμιση του SDK.

Ίσως χρειαστεί να ακολουθήσετε διάφορες διαδικασίες, εάν έχετε παραβλέψει πολλές εκδόσεις του SDK. Για παράδειγμα, εάν κάνετε μετεγκατάσταση από 1.4.0 σε 1.6.0 πρέπει να πρώτα να ακολουθήστε τη διαδικασία "από 1.4.0 να 1.5.0", στη συνέχεια, τη διαδικασία "από 1.5.0 να 1.6.0".

Ανεξάρτητα από την έκδοση που κάνετε αναβάθμιση από, πρέπει να αντικαταστήσετε το `mobile-engagement-VERSION.jar` με τη νέα.

##<a name="from-420-to-421"></a>Από το 4.2.0 να 4.2.1 ή νεότερη έκδοση

Αυτό το βήμα μπορεί να γίνει στην πραγματικότητα σε οποιαδήποτε έκδοση του SDK, είναι μια βελτίωσης ασφαλείας Όταν ενσωματώνετε Reach δραστηριότητες.

Τώρα θα πρέπει να προσθέσετε `exported="false"` σε όλες τις δραστηριότητες Reach.

Δραστηριότητες reach πρέπει τώρα να μοιάζει κάπως έτσι σας `AndroidManifest.xml`:

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

##<a name="from-400-to-410"></a>Από το 4.0.0 να 4.1.0

Το SDK τώρα λαβή νέο μοντέλο δικαιωμάτων από Android M.

Εάν χρησιμοποιείτε τις δυνατότητες θέση ή ειδοποιήσεις γενική εικόνα, διαβάστε [αυτήν την ενότητα](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Εκτός από το νέο μοντέλο δικαιωμάτων, υποστηρίζουμε τώρα τη ρύθμιση παραμέτρων των δυνατοτήτων θέση κατά το χρόνο εκτέλεσης.
Συνεχίζουμε να εξακολουθεί να είναι συμβατά με τις παραμέτρους δήλωσης για θέση, αλλά αυτό τώρα έχει καταργηθεί. Για να χρησιμοποιήσετε τη ρύθμιση παραμέτρων χρόνου εκτέλεσης, κατάργηση στις παρακάτω ενότητες από το ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

και διαβάστε [αυτήν τη διαδικασία ενημερωμένη](mobile-engagement-android-integrate-engagement.md#location-reporting) για να χρησιμοποιήσετε τη ρύθμιση παραμέτρων χρόνου εκτέλεσης αντί για αυτό.

##<a name="from-300-to-400"></a>Από το 3.0.0 να 4.0.0

### <a name="native-push"></a>Εγγενής push

Εγγενής push (GCM ADM) τώρα χρησιμοποιείται επίσης για σε ειδοποιήσεις της εφαρμογής, ώστε να πρέπει να ρυθμίσετε τα διαπιστευτήρια εγγενούς push για οποιονδήποτε τύπο push εκστρατείας.

Εάν δεν είναι ήδη ακολουθήστε [αυτήν τη διαδικασία](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Ενοποίηση reach έχει τροποποιηθεί σε ``AndroidManifest.xml``.

Αντικαταστήστε το εξής:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Από

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

Υπάρχει πιθανώς σε οθόνη φόρτωσης τώρα όταν κάνετε κλικ σε μια ανακοίνωση (με κείμενο/περιεχόμενο web) ή μια ψηφοφορία.
Πρέπει να προστεθεί για αυτές τις καμπάνιες για να εργαστείτε στο 4.0.0:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Πόροι

Ενσωμάτωση του νέου `res/layout/engagement_loading.xml` αρχείο στο έργο σας.

##<a name="from-240-to-300"></a>Από το 2.4.0 να 3.0.0

Παρακάτω περιγράφεται ο τρόπος για να μετεγκαταστήσετε μια ενοποίηση SDK από την υπηρεσία Capptain που παρέχεται από Capptain συσχετισμούς Ασφαλείας σε μια εφαρμογή που υποστηρίζεται από Azure Mobile δέσμευση. Εάν κάνετε μετεγκατάσταση από μια προηγούμενη έκδοση, επικοινωνήστε με την τοποθεσία web Capptain για τη μετεγκατάσταση στο 2.4.0 πρώτα και, στη συνέχεια, εφαρμόστε την ακόλουθη διαδικασία.

>[AZURE.IMPORTANT] Capptain και δέσμευση Mobile δεν είναι τις ίδιες υπηρεσίες, και η διαδικασία που ακολουθεί επισημαίνει μόνο με τη μετεγκατάσταση στην εφαρμογή υπολογιστή-πελάτη. Μετεγκατάσταση στο SDK στην εφαρμογή θα μετεγκατασταθεί τα δεδομένα σας από τους διακομιστές Capptain με τους διακομιστές δέσμευση Mobile.

### <a name="jar-file"></a>Αρχείο ΒΆΖΟ

Αντικατάσταση `capptain.jar` με `mobile-engagement-VERSION.jar` στο σας `libs` φακέλου.

### <a name="resource-files"></a>Αρχεία πόρων

Κάθε αρχείο πόρων που θα σας δώσει (το πρόθεμα `capptain_`) πρέπει να αντικατασταθεί με τις νέες (με το πρόθεμα `engagement_`).

Εάν έχετε προσαρμόσει αυτά τα αρχεία, πρέπει να εφαρμόσετε εκ νέου Προσαρμογή σας με τα νέα αρχεία, **έχουν μετονομαστεί επίσης όλα τα αναγνωριστικά στα αρχεία του πόρου**.

### <a name="application-id"></a>Αναγνωριστικό εφαρμογής

Δέσμευση χρησιμοποιεί τώρα μια συμβολοσειρά σύνδεσης για να ρυθμίσετε τα αναγνωριστικά SDK όπως το αναγνωριστικό εφαρμογής.

Θα πρέπει να χρησιμοποιήσετε `EngagementAgent.init` μέθοδο σε τη δραστηριότητά σας εκκίνηση ως εξής:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Εμφανίζεται η συμβολοσειρά σύνδεσης για την εφαρμογή σας στην πύλη Azure.

Καταργήστε τυχόν κλήσης σε `CapptainAgent.configure` ως `EngagementAgent.init` αντικαθιστά αυτήν τη μέθοδο.

Το `appId` δεν είναι πλέον μπορεί να ρυθμιστεί μέσω `AndroidManifest.xml`.

Καταργήστε αυτήν την ενότητα από το `AndroidManifest.xml` εάν έχετε:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java API

Κάθε κλήση σε οποιαδήποτε κλάσης Java του μας SDK πρέπει να μετονομαστεί; Για παράδειγμα, `CapptainAgent.getInstance(this)` πρέπει να μετονομαστεί `EngagementAgent.getInstance(this)`, `extends CapptainActivity` πρέπει να μετονομαστεί `extends EngagementActivity` κ.λπ...

Εάν έχουν συνδεδεμένο με αρχεία προτιμήσεων παράγοντας προεπιλογή, το προεπιλεγμένο όνομα αρχείου είναι τώρα `engagement.agent` και είναι το πλήκτρο `engagement:agent`.

Κατά τη δημιουργία ανακοινώσεις web, το ντοσιέ Javascript είναι τώρα `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Πολλές αλλαγές συνέβη εκεί, η υπηρεσία δεν είναι πλέον κοινόχρηστο και πολλές δέκτες δεν είναι πλέον δυνατή η εξαγωγή.

Η υπηρεσία δήλωση τώρα είναι απλούστερο; Καταργήστε το φίλτρο πρόθεση και όλων των μετα-δεδομένων μέσα σε αυτό και προσθέστε `exportable=false`.

Επιπλέον, όλα τα στοιχεία έχει μετονομαστεί για να χρησιμοποιήσετε δέσμευση.

Τώρα έχει τη μορφή:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Όταν θέλετε να ενεργοποιήσετε αρχεία καταγραφής ελέγχου, τα μετα-δεδομένα τώρα έχει μετακινηθεί στην ετικέτα της εφαρμογής και έχει μετονομαστεί:

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

Μόλις έχουν μετονομαστεί όλα άλλα μετα-δεδομένα, δείτε την πλήρη λίστα (Μετονομασία του κύκλου μαθημάτων μόνο αυτές που χρησιμοποιείτε):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play και SmartAd παρακολούθησης έχει καταργηθεί από SDK πρέπει απλώς να καταργήσετε αυτή χωρίς επανατοποθέτηση:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

Οι δραστηριότητες Reach δηλώνονται τώρα ως εξής:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
Εάν έχετε προσαρμοσμένες δραστηριότητες Reach, χρειάζεται να αλλάξετε τις ενέργειες πρόθεση ώστε να ταιριάζει με ένα μόνο `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` ή `com.microsoft.azure.engagement.reach.intent.action.POLL`.

Η εκπομπή δέκτες έχουν μετονομαστεί, καθώς και προσθέτουμε τώρα `exported=false`. Ακολουθεί η πλήρης λίστα δέκτη με τη νέα προδιαγραφή, (του κύκλου μαθημάτων Μετονομασία μόνο αυτές που χρησιμοποιείτε):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Παρακολούθηση δέκτη έχει καταργηθεί, επομένως πρέπει να καταργήσετε αυτήν την ενότητα:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Σημειώστε ότι η δήλωση την εφαρμογή του, ο παραλήπτης εκπομπής **EngagementMessageReceiver** έχει αλλάξει σε το `AndroidManifest.xml`. Αυτό συμβαίνει επειδή το API για την αποστολή και κατάργηση αυθαίρετο XMPP μηνυμάτων από αυθαίρετο οντοτήτων XMPP και το API για αποστολή και λήψη μηνυμάτων μεταξύ συσκευών έχουν καταργηθεί. Έτσι, πρέπει επίσης να διαγράψετε το παρακάτω επιστροφές κλήσης από την υλοποίησή **EngagementMessageReceiver** σας:

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

και

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

στη συνέχεια, διαγράψτε κάθε κλήση στην **EngagementAgent** για:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

και

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard

Ρύθμιση παραμέτρων proguard μπορεί να αντιμετωπίσουν προβλήματα εξαιτίας rebranding, οι κανόνες που αναζητούν τώρα όπως:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
