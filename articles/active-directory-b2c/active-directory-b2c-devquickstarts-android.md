<properties
    pageTitle="Azure Active Directory B2C: Καλέσετε ένα API web από μια εφαρμογή του Android | Microsoft Azure"
    description="Σε αυτό το άρθρο θα σας δείξουν πώς μπορείτε να δημιουργήσετε Android 'λίστα εκκρεμών εργασιών' εφαρμογή που καλεί μια API web Node.js χρησιμοποιώντας τα διακριτικά φορέα OAuth 2.0. Η εφαρμογή Android και το API web Χρησιμοποιήστε Azure Active Directory B2C Διαχείριση ταυτοτήτων χρήστη και τον έλεγχο ταυτότητας χρηστών."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure AD B2C: Καλέσετε ένα API web από μια εφαρμογή του Android

> [AZURE.WARNING] Αυτό το πρόγραμμα εκμάθησης απαιτεί ορισμένες σημαντικές ενημερώσεις, ειδικά για να καταργήσετε τη χρήση του ADAL Android για B2C.  Πρόκειται να δημοσιεύσετε Φρέσκο οδηγίες για τη χρήση Azure AD B2C στις εφαρμογές του Android της επόμενης εβδομάδας και, συνιστάται να κρατάτε μέχρι εκείνη τη στιγμή.  Ωστόσο, εάν θέλετε απλώς να δοκιμάσετε πράγματα ανάληψη, μην διστάσεις να συνεχίσετε με το παρακάτω άρθρο.



Με τη χρήση B2C Azure Active Directory (Azure AD), μπορείτε να προσθέσετε δυνατότητες διαχείρισης ισχυρή ταυτότητας από το χρήστη να web APIs με λίγα βήματα σύντομο και τις εφαρμογές του Android. Σε αυτό το άρθρο θα σας δείξουν πώς μπορείτε να δημιουργήσετε Android "λίστα εκκρεμών εργασιών" εφαρμογή που καλεί μια API web Node.js χρησιμοποιώντας τα διακριτικά φορέα OAuth 2.0. Η εφαρμογή Android και το API web Χρησιμοποιήστε Azure AD B2C Διαχείριση ταυτοτήτων χρήστη και τον έλεγχο ταυτότητας χρηστών.

Αυτή η γρήγορη έναρξη απαιτεί να έχετε ένα API που προστατεύεται από Azure AD με B2C εργασίας πλήρως web. Θα σας έχει δημιουργηθεί ένα για .NET και Node.js για να χρησιμοποιήσετε. Γνωρίστε αυτό προϋποθέτει ότι έχει ρυθμιστεί το δείγμα web API Node.js. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Azure AD B2C web API για το πρόγραμμα εκμάθησης Node.js](active-directory-b2c-devquickstarts-api-node.md).

Για Android προγράμματα-πελάτες που πρέπει να αποκτήσετε πρόσβαση σε προστατευμένο πόρους, Azure AD παρέχει το ενεργό βιβλιοθήκη ελέγχου ταυτότητας καταλόγου (ADAL). Ο μοναδικός σκοπός της ADAL είναι να σας διευκολύνουν την εφαρμογή σας για να λάβετε διακριτικά πρόσβασης. Για μια επίδειξη πόσο εύκολο είναι, σε αυτόν τον οδηγό θα σας θα δημιουργήσετε μια εφαρμογή λίστα εκκρεμών εργασιών Android που:

- Αποκτά πρόσβαση των διακριτικών που καλείτε μια λίστα εκκρεμών εργασιών API, χρησιμοποιώντας το [πρωτόκολλο ελέγχου ταυτότητας διακριτικό 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
- Λαμβάνει λίστες εκκρεμών εργασιών των χρηστών.
- Πρόσημα των χρηστών.

> [AZURE.NOTE] Σε αυτό το άρθρο δεν καλύπτει τον τρόπο για την υλοποίηση της εισόδου, εγγραφής και τη Διαχείριση προφίλ, χρησιμοποιώντας Azure AD B2C. Εστιάζει σχετικά με τον τρόπο για να καλέσετε web APIs μετά τον έλεγχο ταυτότητας του χρήστη. Εάν δεν το έχετε κάνει ήδη, θα πρέπει να μπορείτε να ξεκινήσετε με το [.NET web app ξεκινήσατε το πρόγραμμα εκμάθησης](active-directory-b2c-devquickstarts-web-dotnet.md) για να μάθετε περισσότερα σχετικά με τα βασικά στοιχεία του Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Λήψη ενός καταλόγου Azure AD B2C

Μπορείτε να χρησιμοποιήσετε Azure AD B2C, πρέπει να δημιουργήσετε έναν κατάλογο, ή να μισθωτή. Ένας κατάλογος είναι ένα κοντέινερ για όλους τους χρήστες σας, εφαρμογές, ομάδες και πολλά άλλα. Εάν δεν έχετε ήδη, [Δημιουργήστε έναν κατάλογο B2C](active-directory-b2c-get-started.md) πριν να συνεχίσετε με αυτόν τον οδηγό.

## <a name="create-an-application"></a>Δημιουργία μιας εφαρμογής

Στη συνέχεια, πρέπει να δημιουργήσετε μια εφαρμογή στον κατάλογό σας B2C. Αυτό σας δίνει Azure AD πληροφορίες που χρειάζονται για την ασφαλή επικοινωνία με την εφαρμογή σας. Τόσο η εφαρμογή web και API αντιπροσωπεύονται από ένα μεμονωμένο **Αναγνωριστικό εφαρμογής** σε αυτήν την περίπτωση, επειδή αυτά περιλαμβάνουν μία λογική εφαρμογής. Για να δημιουργήσετε μια εφαρμογή, ακολουθήστε [αυτές τις οδηγίες](active-directory-b2c-app-registration.md). Βεβαιωθείτε ότι:

- Συμπεριλάβετε μια **εφαρμογή web**/**web API** στην εφαρμογή.
- Πληκτρολογήστε `urn:ietf:wg:oauth:2.0:oob` ως **διεύθυνση URL απάντηση**. Είναι η προεπιλεγμένη διεύθυνση URL για αυτό το δείγμα κώδικα.
- Δημιουργία μιας **εφαρμογής μυστικό** για την εφαρμογή σας και αντιγράψτε την. Θα το χρειαστείτε αργότερα. Σημειώστε ότι αυτή η τιμή πρέπει να είναι [διαφυγής XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) πριν να το χρησιμοποιήσετε.
- Αντιγράψτε το **Αναγνωριστικό εφαρμογής** που έχει αντιστοιχιστεί σε εφαρμογή σας. Θα χρειαστεί επίσης αυτό αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Δημιουργία πολιτικών σας

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Στο Azure AD B2C, κάθε εμπειρία χρήστη ορίζεται από μια [πολιτική](active-directory-b2c-reference-policies.md). Αυτή η εφαρμογή περιέχει τρεις εμπειρίες ταυτότητας: εγγραφείτε, πραγματοποιήστε είσοδο στο και πραγματοποιήστε είσοδο χρησιμοποιώντας το Facebook.  Πρέπει να δημιουργήσετε μία πολιτική κάθε τύπο, όπως περιγράφεται στο [άρθρο αναφοράς πολιτικής](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Όταν δημιουργείτε τις πολιτικές τρεις σας, βεβαιωθείτε ότι:

- Επιλέξτε το **εμφανιζόμενο όνομα** και άλλα χαρακτηριστικά εγγραφής από την πολιτική της εγγραφής σας.
- Επιλέξτε το **εμφανιζόμενο όνομα** και το **Αναγνωριστικό αντικειμένου** δηλώσεις εφαρμογής σε κάθε πολιτική. Μπορείτε να επιλέξετε καθώς και άλλες αξιώσεων.
- Αντιγράψτε το **όνομα** κάθε πολιτική μετά τη δημιουργία του. Θα πρέπει να έχει το πρόθεμα `b2c_1_`.  Θα χρειαστείτε αυτά τα ονόματα πολιτικής αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Αφού δημιουργήσετε τις τρεις πολιτικές, είστε έτοιμοι να δημιουργήσετε την εφαρμογή σας.

Σημειώστε ότι αυτό το άρθρο δεν καλύπτει πώς μπορείτε να χρησιμοποιήσετε τις πολιτικές που μόλις δημιουργήσατε. Για να μάθετε πώς λειτουργούν οι πολιτικές στο Azure AD B2C, ξεκινήστε με το [.NET web app ξεκινήσατε το πρόγραμμα εκμάθησης](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Κάντε λήψη του κώδικα

Ο κωδικός για αυτό προγραμμάτων εκμάθησης [διατηρούνται με GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android). Για να δημιουργήσετε το δείγμα καθώς εργάζεστε, μπορείτε να [κάνετε λήψη του σκελετό έργου ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip). Μπορείτε επίσης να αντιγράψετε το σκελετό:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **Να σας ζητηθεί να κάνετε λήψη του σκελετό για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης.** Λόγω την πολυπλοκότητα της εφαρμογής μιας εφαρμογής πλήρως λειτουργικό Android, το σκελετό έχει UX κώδικα που θα εκτελεστεί μετά την ολοκλήρωση αυτού του προγράμματος εκμάθησης. Αυτή είναι μια μέτρηση εξοικονόμηση χρόνου για τους προγραμματιστές. Ο κωδικός UX δεν είναι germane στο θέμα σχετικά με τον τρόπο για να προσθέσετε B2C σε μια εφαρμογή του Android.

Η εφαρμογή ολοκληρωμένων είναι επίσης [διαθέσιμο ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) ή σε το `complete` υποκατάστημα στο ίδιο αποθετήριο.

Για να δημιουργήσετε με Maven, μπορείτε να χρησιμοποιήσετε `pom.xml` στο ανώτερο επίπεδο.

  1. Ακολουθήστε τα βήματα στην [ενότητα τις προϋποθέσεις για να ρυθμίσετε Maven για Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
  2. Ρυθμίστε ένα προσομοίωσης με SDK 21.
  3. Μεταβείτε στον ριζικό φάκελο όπου κλωνοποιηθεί το repo.
  4. Εκτελέστε την εντολή `mvn clean install`.
  5. Αλλάξτε τον κατάλογο στο δείγμα γρήγορη έναρξη `cd samples\hello`.
  6. Εκτελέστε την εντολή `mvn android:deploy android:run`.

Θα πρέπει να βλέπετε την εφαρμογή εκκίνησης. Εισαγάγετε τα διαπιστευτήρια χρήστη δοκιμή για να το δοκιμάσετε.

Αρχειοθέτηση Java (ΒΆΖΟ) πακέτων θα επίσης υποβληθεί δίπλα από το πακέτο Android αρχειοθέτησης (AAR).

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>Κάντε λήψη του Android ADAL και να την προσθέσετε στο χώρο εργασίας σας Android Studio

Έχετε επιλογές για τον τρόπο χρήσης αυτής της βιβλιοθήκης στο έργο σας Android:

* Μπορείτε να χρησιμοποιήσετε τον κωδικό προέλευσης για να εισαγάγετε τη βιβλιοθήκη στο Έκλειψη και σύνδεση στην εφαρμογή σας.
* Εάν χρησιμοποιείτε το Android Studio, μπορείτε να χρησιμοποιήσετε τη μορφή πακέτου AAR και να αναφέρονται τα δυαδικά δεδομένα.

### <a name="option-1-binaries-via-gradle-recommended"></a>Επιλογή 1: Δυαδικών αρχείων μέσω Gradle (συνιστάται)

Μπορείτε να λάβετε τα δυαδικά δεδομένα από την κεντρική repo Maven. Το πακέτο AAR μπορούν να συμπεριληφθούν στο έργο σας με Android Studio (για παράδειγμα, στο `build.gradle`) με αυτόν τον τρόπο:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>Επιλογή 2: AAR μέσω Maven

Εάν χρησιμοποιείτε το `m2e` προσθήκης στο Έκλειψη, μπορείτε να καθορίσετε την εξάρτηση στο σας `pom.xml` αρχείο:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>Επιλογή 3: Προέλευσης μέσω Git (τελευταίες λύση)

Για να λάβετε τον κωδικό προέλευσης του SDK μέσω Git, πληκτρολογήστε:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

Χρήση του κλάδου **σύγκλισης.**

## <a name="set-up-your-configuration-file"></a>Ρυθμίστε το αρχείο ρύθμισης παραμέτρων

Χρησιμοποιήστε τις ρυθμίσεις παραμέτρων που ρυθμίζετε παλαιότερη στην πύλη του B2C για να ρυθμίσετε τις παραμέτρους του Android έργου.

Άνοιγμα `helpes/Constants.java` και συμπληρώστε τις τιμές για τα εξής:

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`: Τα πεδία που έχετε στείλει ο διακομιστής στον οποίο θέλετε να ζητήσετε από το διακομιστή όταν ένας χρήστης πραγματοποιεί είσοδο. Για την προεπισκόπηση B2C, που μεταβιβάζουν `client_id`. Ωστόσο, αυτό είναι αναμενόμενο για να αλλάξετε σε `read scopes` στο μέλλον. Αυτό το έγγραφο θα ενημερωθεί όταν που εμφανίζεται.
- `ADDITIONAL_SCOPES`: Επιπλέον εύρος που μπορεί να θέλετε να χρησιμοποιήσετε για την εφαρμογή σας. Αυτές είναι αναμένατε να χρησιμοποιηθεί στο μέλλον.
- `CLIENT_ID`: Το Αναγνωριστικό εφαρμογής που λάβατε από την πύλη.
- `REDIRECT_URL`: Η ανακατεύθυνση όπου μπορείτε να αναμένετε το διακριτικό θα καταχωρηθεί πίσω.
- `EXTRA_QP`: Επιπλέον που θέλετε για τη μεταβίβαση στο διακομιστή σε μορφή κωδικοποίηση URL.
- `FB_POLICY`: Η πολιτική που χρησιμοποιείτε. Αυτό είναι το τμήμα πιο σημαντικές για Γνωρίστε αυτό.
- `EMAIL_SIGNIN_POLICY`: Η πολιτική που χρησιμοποιείτε. Αυτό είναι το τμήμα πιο σημαντικές για Γνωρίστε αυτό.
- `EMAIL_SIGNUP_POLICY`: Η πολιτική που χρησιμοποιείτε. Αυτό είναι το τμήμα πιο σημαντικές για Γνωρίστε αυτό.

## <a name="add-references-to-android-adal-to-your-project"></a>Προσθήκη αναφορές σε Android ADAL στο έργο σας

> [AZURE.NOTE]  ADAL για Android χρησιμοποιεί ένα μοντέλο βάσει πρόθεσης για να καλέσετε τον έλεγχο ταυτότητας. Στόχοι "διάταξη επάνω από" της εφαρμογής για να εργαστείτε. Αυτό το δείγμα ολόκληρο και όλες ADAL για Android, κέντρα σχετικά με τη Διαχείριση στόχοι και να μεταφέρουν πληροφορίες μεταξύ τους.

Πρώτα, πείτε Android για τη διάταξη της εφαρμογής σας, συμπεριλαμβανομένων των τους στόχους χρωματικής που θέλετε να χρησιμοποιήσετε. Αυτές οι στόχοι θα αναλυθεί με λεπτομέρειες παρακάτω σε αυτό το πρόγραμμα εκμάθησης.

Ενημέρωση του έργου σας `AndroidManifest.xml` αρχείο για να συμπεριλάβετε όλων των στόχων σας:

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

Όπως μπορείτε να δείτε, μπορείτε να καθορίσετε πέντε δραστηριότητες. Θα χρησιμοποιήσετε όλες αυτές τις διαδικασίες.

- `AuthenticationActivity`: Αυτή η τιμή προέρχεται από ADAL και παρέχει την προβολή εισόδου στο web.
- `LoginActivity`: Αυτή εμφανίζει τις πολιτικές εισόδου και τα κουμπιά για κάθε πολιτική.
- `SettingsActivity`: Αυτό μπορείτε να το χρησιμοποιήσετε για να αλλάξετε τις ρυθμίσεις εφαρμογής κατά το χρόνο εκτέλεσης.
- `AddTaskActivity`: Αυτό μπορείτε να το χρησιμοποιήσετε για να προσθέσετε εργασίες σας REST API που προστατεύονται από Azure AD.
- `ToDoActivity`: Αυτή είναι η κύρια δραστηριότητα που εμφανίζει εργασίες.

## <a name="create-the-sign-in-activity"></a>Δημιουργήστε τη δραστηριότητα εισόδου

Δημιουργήστε μια κύρια δραστηριότητα και καλέστε την `LoginActivity`.

Δημιουργήστε ένα αρχείο που ονομάζεται `LoginActivity.java`.

Πρέπει να προετοιμάσει τη δραστηριότητα και να προσθέσετε ορισμένα κουμπιά που θα ελέγχει το περιβάλλον εργασίας Χρήστη. Αυτή είναι οικεία για εσάς, εάν έχετε γράψει Android κώδικα πριν από την.

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
Τώρα που έχετε δημιουργήσει κουμπιά που καλούν σας `ToDoActivity` πρόθεσης (το οποίο καλεί ADAL όταν χρειάζεστε ένα διακριτικό). Μπορούν να το κάνουν αυτό χρησιμοποιώντας τη δραστηριότητά σας ως μια αναφορά και μια επιπλέον παράμετρο. Αυτή η παράμετρος επιπλέον μεταβιβάζεται το `intent.putExtra()` μέθοδο. Μπορείτε να καθορίσετε `"thePolicy"` με τη χρήση που καθορίσατε στο `Constants.java`. Αυτό σας ενημερώνει για το στόχο την πολιτική για να καλέσετε κατά τον έλεγχο ταυτότητας.

## <a name="create-the-settings-activity"></a>Δημιουργήστε τη δραστηριότητα ρυθμίσεων

Αυτή είναι μια δραστηριότητα που συμπληρώνει τις ρυθμίσεις περιβάλλοντος εργασίας Χρήστη.

Δημιουργήστε ένα αρχείο που ονομάζεται `SettingsActivity.java` για απλή δημιουργία, ανάγνωση, ενημέρωση και διαγραφή λειτουργίες (CRUD).

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>Δημιουργήστε τη δραστηριότητα Προσθήκη εργασιών

Μπορείτε να χρησιμοποιήσετε αυτήν τη δραστηριότητα για να προσθέσετε μια εργασία του τελικού σημείου REST API.

Δημιουργήστε ένα αρχείο που ονομάζεται `AddTaskActivity.java` και να γράφετε τα εξής.

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>Δημιουργήστε τη δραστηριότητα λίστα εκκρεμών εργασιών

Αυτή είναι η πιο σημαντική δραστηριότητα. Μπορείτε να χρησιμοποιούν για τη λήψη ενός διακριτικού από Azure AD για μια πολιτική και, στη συνέχεια, χρησιμοποιήστε αυτό το διακριτικό για να καλέσετε το διακομιστή REST API εργασίας.

Δημιουργήστε ένα αρχείο που ονομάζεται `ToDoActivity.java` και να γράφετε τα εξής. (Οι κλήσεις θα αναλυθεί αργότερα.)

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 Ίσως έχετε παρατηρήσει ότι αυτό βασίζεται στις μεθόδους που δεν έχει γραφτεί ακόμα. Περιλαμβάνουν `updateLoggedInUser()`, `clearSessionCookie()`, και `getTasks()`. Θα μπορείτε να συντάξετε αυτές αργότερα σε αυτόν τον οδηγό. Μπορείτε να αγνοήσετε τα σφάλματα στο Android Studio προς το παρόν.

Εξήγηση των παραμέτρων:

  - `SCOPES`: Απαιτείται, τα πεδία που προσπαθείτε να ζητήσετε πρόσβαση για. Για την προεπισκόπηση B2C, αυτή είναι η ίδια με `client_id`, αλλά αυτό είναι αναμενόμενο να αλλάξει στο μέλλον.
  - `POLICY`: Η πολιτική για όταν θέλετε για τον έλεγχο ταυτότητας χρήστη.
  - `CLIENT_ID`: Υποχρεωτικά, παρέχεται από την πύλη του Azure AD.
  - `redirectUri`: Μπορεί να ρυθμιστεί ως το όνομα του πακέτου. Δεν απαιτείται που διατίθενται για το `acquireToken` κλήσεων.
  - `getUserInfo()`: Ο τρόπος για να αναζητήσετε εάν ένας χρήστης είναι ήδη στο cache. Η παράμετρος επίσης περιγράφει πώς να γίνεται ερώτηση στο χρήστη εάν αυτός ο χρήστης δεν βρέθηκε ή εάν διακριτικό πρόσβασης του χρήστη δεν είναι έγκυρο. Αυτή η μέθοδος θα εγγραφεί αργότερα σε αυτόν τον οδηγό.
  - `PromptBehavior.always`: Σάς βοηθά να ρωτήσετε για τα διαπιστευτήρια για να παραλείψετε το cache και για τα cookies.
  - `Callback`: Η οποία ονομάζεται μετά από έναν κωδικό εξουσιοδότησης ανταλλάσσονται έναν κωδικό. Θα έχει ένα αντικείμενο `AuthenticationResult`, που περιέχει διακριτικό πρόσβασης, ημερομηνία λήξης και Αναγνωριστικό διακριτικού πληροφορίες.

> [AZURE.NOTE]  Το στοιχείο broker που παρέχει την εφαρμογή Microsoft Intune εταιρικής πύλης και αυτό μπορεί να είναι εγκατεστημένο στη συσκευή του χρήστη. Η εφαρμογή παρέχει καθολική σύνδεση (SSO) πρόσβαση σε όλες τις εφαρμογές στη συσκευή. Προγραμματιστές θα πρέπει να είστε έτοιμοι για να επιτρέψετε για Intune. ADAL για Android θα χρησιμοποιήσει το λογαριασμό broker, εάν υπάρχει ένα λογαριασμό χρήστη που έχουν δημιουργηθεί με την υπηρεσία ελέγχου ταυτότητας. Για να χρησιμοποιήσετε το broker, ο προγραμματιστής πρέπει να καταχωρήσετε μια ειδική `redirectUri` για το broker για να χρησιμοποιήσετε. `redirectUri`είναι στο πλαίσιο μορφή msauth://packagename/Base64UrlencodedSignature. Μπορείτε να λάβετε `redirectUri` για την εφαρμογή σας, χρησιμοποιώντας τη δέσμη ενεργειών `brokerRedirectPrint.ps1` ή χρησιμοποιώντας την κλήση API `mContext.getBrokerRedirectUri()`. Η υπογραφή είναι που σχετίζονται με τα πιστοποιητικά υπογραφής σας από το Google Play store.

 Μπορείτε να παραλείψετε το χρήστη broker με τη χρήση:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] Για να μειώσετε την πολυπλοκότητα του αυτό γρήγορη έναρξη B2C, θα σας έχει επιλέξει να παραλείψετε το broker στο δείγμα μας.

Στη συνέχεια, δημιουργήστε μεθόδους Βοήθειας που λαμβάνει το διακριτικό μόνο κατά τη διάρκεια ελέγχου ταυτότητας κλήσεις με την εργασία API.

Στην ίδια `ToDoActivity.java` αρχείων, γράψτε τα εξής.

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

Επίσης, προσθέσετε μεθόδους που θα "Ορισμός" και "Λήψη" `AuthenticationResult` (που έχει το διακριτικό) στο καθολικό πρότυπο `Constants`. Παρόλο που τα `ToDoActivity.java` χρησιμοποιεί `sResult` στο τις ροές, πρέπει να προσθέσετε αυτές τις μεθόδους. Εάν δεν το χρησιμοποιείτε, σας άλλες δραστηριότητες δεν θα έχουν πρόσβαση το διακριτικό για την εργασία (όπως να προσθέσετε μια εργασία σε `AddTaskActivity.java`).

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>Δημιουργήστε μια μέθοδο για να λάβετε ένα αναγνωριστικό χρήστη

ADAL για Android αντιπροσωπεύει το χρήστη με τη μορφή ενός `UserIdentifier` αντικειμένου. Διαχειρίζεται το χρήστη. Μπορείτε να χρησιμοποιήσετε το αντικείμενο για να προσδιορίσετε αν το ίδιο χρήστη που χρησιμοποιείται σε κλήσεις σας. Χρησιμοποιώντας αυτές τις πληροφορίες, μπορείτε να εξαρτώνται από το cache, αντί Πραγματοποίηση νέας κλήσης στο διακομιστή. Για να κάνετε πιο εύκολη, που δημιουργήσαμε το `getUserInfo()` μέθοδο που επιστρέφει `UserIdentifier`. Μπορείτε να το χρησιμοποιήσετε με `acquireToken()`. Δημιουργήσαμε επίσης ένα `getUniqueId()` μέθοδο που επιστρέφει το Αναγνωριστικό του `UserIdentifier` στο cache.

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>Βοηθητική εφαρμογή του μεθόδους εγγραφής

Στη συνέχεια, συντάξτε ορισμένες μεθόδους Βοήθειας θα σας βοηθήσουν να καταργήστε την επιλογή από τα cookies και δώστε `AuthenticationCallback`. Αυτές οι μέθοδοι χρησιμοποιούνται καθαρά για το δείγμα για να βεβαιωθείτε ότι βρίσκεστε σε μια καθαρή κατάσταση, όταν καλείτε το `ToDo` δραστηριότητα.

Στο ίδιο αρχείο ονομάζεται `ToDoActivity.java`, γράψτε τα εξής.

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>Κλήση εργασίας API

Αφού έχετε έτοιμη για να καταγράψετε διακριτικά τη δραστηριότητά σας, μπορείτε να συντάξετε το API για να αποκτήσετε πρόσβαση στο διακομιστή εργασιών.

`getTasks`παρέχει έναν πίνακα που αντιπροσωπεύει τις εργασίες στο διακομιστή σας.

Έναρξη με `getTasks`.

Στο ίδιο αρχείο ονομάζεται `ToDoActivity.java`, γράψτε τα εξής.

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

Επίσης, Συντάξτε μια μέθοδο που θα προετοιμασία πίνακές σας στην πρώτη εκτέλεση.

Στο ίδιο αρχείο ονομάζεται `ToDoActivity.java`, γράψτε τα εξής.

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

Μπορείτε να δείτε ότι αυτός ο κωδικός απαιτεί ορισμένες πρόσθετες μεθόδους για να εκτελέσετε τις εργασίες της. Γράψτε αυτές τις Επόμενο.

### <a name="create-an-endpoint-url-generator"></a>Δημιουργία ενός τελικού σημείου γεννήτρια διεύθυνση URL

Πρέπει να δημιουργήσετε τη διεύθυνση URL τελικού σημείου που θα τη σύνδεση με. Κάντε κλικ στην εντολή στο ίδιο αρχείο κλάσης.

**Στο ίδιο αρχείο** ονομάζεται `ToDoActivity.java`, γράψτε τα εξής.

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


Σημειώστε ότι προσθέτετε το διακριτικό πρόσβασης στην αίτηση τον κώδικα που περιγράφονται στην επόμενη ενότητα.

## <a name="write-some-ux-methods"></a>Ορισμένες μεθόδους UX εγγραφής

Android απαιτεί να χειρίζεται ορισμένες επιστροφές κλήσης για τη λειτουργία της εφαρμογής. Αυτές είναι `createAndShowDialog` και `onResume()`. Αυτή είναι οικεία για εσάς, εάν έχετε γράψει Android κώδικα πριν από την.

Στο ίδιο αρχείο ονομάζεται `ToDoActivity.java`, γράψτε τα εξής.

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

Στη συνέχεια, να διαχειριστείτε το παράθυρο διαλόγου επιστροφές κλήσης.

Στο ίδιο αρχείο ονομάζεται `ToDoActivity.java`, γράψτε τα εξής.

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

Πρέπει τώρα να έχετε μια `ToDoActivity.java` αρχείο το οποίο συγκεντρώνει. Ολόκληρο το έργο θα πρέπει επίσης μεταγλώττιση σε αυτό το σημείο.

## <a name="run-the-sample-app"></a>Εκτελέστε την εφαρμογή δείγματος

Τέλος, δημιουργία και εκτέλεση της εφαρμογής στο Android Studio ή Έκλειψη. Εγγραφή ή πραγματοποιήστε είσοδο στην εφαρμογή. Δημιουργία εργασιών για το χρήστη συνδεδεμένοι στο. Πραγματοποιήστε έξοδο και συνδεθείτε ξανά με ένα διαφορετικό χρήστη και, στη συνέχεια, δημιουργία εργασιών για τον συγκεκριμένο χρήστη.

Παρατηρήστε ότι οι εργασίες είναι αποθηκευμένες ανά χρήστη στην το API, επειδή το API εξάγει την ταυτότητα του χρήστη από το διακριτικό πρόσβασης που λαμβάνει.

Για αναφορά, το ολοκληρωμένο δείγμα που [παρέχεται ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip). Μπορείτε, επίσης, να την αντιγράψετε από GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>Σημαντικές πληροφορίες


### <a name="encryption"></a>Κρυπτογράφηση

ADAL κρυπτογραφεί τα διακριτικά και αποθήκευσης στο `SharedPreferences` από προεπιλογή. Μπορείτε να δείτε το `StorageHelper` τάξης για να δείτε τις λεπτομέρειες. Android Παρουσιάστηκε **AndroidKeyStore για 4.3(API18)** ασφαλή αποθήκευση ιδιωτικά κλειδιά. ADAL που χρησιμοποιεί για API18 και άνω. Εάν θέλετε να χρησιμοποιήσετε ADAL για κάτω SDK εκδόσεις, πρέπει να παρέχουν ένα μυστικό κλειδί στο `AuthenticationSettings.INSTANCE.setSecretKey`.

### <a name="session-cookies-in-web-view"></a>Τα cookies περιόδου λειτουργίας σε προβολή στο web

Προβολή Android web δεν καταργήστε τα cookies περιόδου λειτουργίας μετά το κλείσιμο της εφαρμογής. Μπορείτε να χειριστείτε αυτό με το παρακάτω δείγμα κώδικα.
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
[Μάθετε περισσότερα σχετικά με τα cookies](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).
