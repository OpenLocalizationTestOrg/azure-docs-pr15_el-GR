<properties
    pageTitle="Azure AD Android γρήγορα αποτελέσματα | Microsoft Azure"
    description="Πώς να δημιουργείτε μια εφαρμογή Android που ενοποιείται με το Azure AD για είσοδο στο και κλήσεις Azure AD προστατευμένο APIs χρησιμοποιώντας OAuth."
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>Ενοποίηση του Azure AD σε εφαρμογή της Android

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Εάν αναπτύσσετε μια εφαρμογή υπολογιστή, Azure AD καθιστά απλή και άμεση για να τον έλεγχο ταυτότητας τους χρήστες σας, χρησιμοποιώντας τους λογαριασμούς υπηρεσίας καταλόγου Active Directory.  Ενεργοποιεί επίσης την εφαρμογή σας για την ασφαλή εκμετάλλευση οποιαδήποτε API που προστατεύονται από Azure AD, όπως τα API Office 365 ή το API Azure web.

Για Android προγράμματα-πελάτες που πρέπει να αποκτήσετε πρόσβαση σε προστατευμένο πόρους, Azure AD παρέχει τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory ή ADAL.  Σκοπός του ADAL κατά τη διάρκεια ζωής της είναι ώστε να μπορείτε εύκολα για την εφαρμογή σας για να λάβετε διακριτικά πρόσβασης.  Για μια επίδειξη απλώς πόσο εύκολο είναι, εδώ θα σας θα δημιουργήσετε μια εφαρμογή του Android λίστα εκκρεμών εργασιών που:

-   Λαμβάνει πρόσβαση διακριτικά για την κλήση ενός API λίστα εκκρεμών εργασιών χρησιμοποιώντας το [πρωτόκολλο ελέγχου ταυτότητας διακριτικό 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Λαμβάνει λίστα εκκρεμών εργασιών ενός χρήστη
-   Σύμβολα χρήστες εκτός.

Για να ξεκινήσετε, θα χρειαστείτε ένα μισθωτή του Azure AD με την οποία μπορείτε να δημιουργήσετε χρήστες και να καταχωρήσετε μια εφαρμογή του.  Εάν δεν έχετε ήδη ένα μισθωτή, [Μάθετε πώς μπορείτε να αποκτήσετε](active-directory-howto-tenant.md).

> [AZURE.TIP] Δοκιμάστε την προεπισκόπηση του μας νέα [πύλη για προγραμματιστές](https://identity.microsoft.com/Docs/Android) που θα σας βοηθήσει να ξεκινήσετε με το Azure Active Directory σε λίγα λεπτά!  Η πύλη για προγραμματιστές θα σας καθοδηγήσει της διαδικασίας την καταχώρηση εφαρμογής και ενοποίηση Azure AD μέσα στον κώδικα.  Όταν ολοκληρώσετε την εργασία, θα έχετε μια απλή εφαρμογή που μπορούν να ελέγχουν την ταυτότητα χρηστών στο μισθωτή σας και έναν υπολογιστή στο παρασκήνιο που μπορούν να αποδέχονται τα διακριτικά και εκτελεί επικύρωση. 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Βήμα 1: Λήψη και να εκτελέσετε το διακομιστή Node.js REST API TODO δείγματος

Αυτό το δείγμα έχει συνταχθεί ειδικά ώστε να λειτουργεί σε σχέση με υπάρχουσες δείγμα μας για τη δημιουργία ένα μεμονωμένο μισθωτή εκκρεμών εργασιών REST API για το Microsoft Azure Active Directory. Αυτό είναι προαπαιτούμενα για τη Γρήγορη εκκίνηση.

Για πληροφορίες σχετικά με τον τρόπο ρύθμισης αυτό, επισκεφθείτε το υπάρχον δείγματα εδώ:

* [Υπηρεσία API ΥΠΌΛΟΙΠΑ Microsoft Azure Active Directory δείγμα για Node.js](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>Βήμα 2: Καταχώρηση σας API Web με το μισθωτή AD του Windows Azure

**Τι να και εγώ;**

*Microsoft Active Directory υποστηρίζει προσθέτοντας δύο τύπους εφαρμογών. Web API που προσφέρουν υπηρεσίες για τους χρήστες και τις εφαρμογές (είτε στο web ή σε μια εφαρμογή που εκτελείται σε μια συσκευή) που έχουν πρόσβαση αυτών των API Web. Σε αυτό το βήμα καταχώρηση το API Web εκτελείτε τοπικά για τη δοκιμή αυτού του δείγματος. Κανονικά αυτό το API Web θα ήταν μια υπηρεσία ΥΠΌΛΟΙΠΟ που προσφέρει λειτουργίες θέλετε μια εφαρμογή για να αποκτήσετε πρόσβαση. Microsoft Azure Active Directory να προστατεύσετε οποιαδήποτε τελικού σημείου!*

*Εδώ θα σας υπό την προϋπόθεση ότι καταχώρηση το REST API TODO που αναφέρονται παραπάνω, αλλά αυτό λειτουργεί για οποιαδήποτε API Web μπορεί να θέλετε Azure Active Directory για την προστασία.*

Βήματα για να καταχωρήσετε ένα API Web με το Microsoft Azure AD

1. Είσοδος στην [πύλη διαχείρισης Azure](https://manage.windowsazure.com).
2. Κάντε κλικ στην υπηρεσία καταλόγου Active Directory στο την πλοήγηση αριστερής
3. Κάντε κλικ στο μισθωτή του καταλόγου όπου θέλετε να καταγράψετε το δείγμα εφαρμογής.
4. Κάντε κλικ στην καρτέλα εφαρμογές.
5. Στο το σχέδιο, κάντε κλικ στην επιλογή Προσθήκη.
6. Κάντε κλικ στην επιλογή "Προσθήκη εφαρμογής ανάπτυξη εταιρεία μου".
7. Πληκτρολογήστε ένα φιλικό όνομα για την εφαρμογή, για παράδειγμα "TodoListService", επιλέξτε "API Web ή/και εφαρμογή Web", και κάντε κλικ στο κουμπί Επόμενο.
8. Για τη διεύθυνση URL σύνδεσης, εισαγάγετε τη βασική διεύθυνση URL για το δείγμα, η οποία είναι από προεπιλογή `https://localhost:8080`.
9. Πληκτρολογήστε το Αναγνωριστικό URI εφαρμογής, `https://<your_tenant_name>/TodoListService`, αντικατάσταση `<your_tenant_name>` με το όνομα του μισθωτή του Azure AD.  Κάντε κλικ στο κουμπί OK για να ολοκληρώσετε την καταχώρηση.
10. Ενώ εξακολουθείτε να στην πύλη του Azure, κάντε κλικ στην καρτέλα ρύθμιση παραμέτρων της εφαρμογής σας.
11. **Εύρεση της τιμής Αναγνωριστικό υπολογιστή-πελάτη και αντιγραφή του Εξοικονομήστε**, θα χρειαστεί αυτό αργότερα κατά τη ρύθμιση παραμέτρων της εφαρμογής σας.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Βήμα 3: Καταχώρηση το δείγμα εφαρμογής Android Native Client

Εγγραφή για την εφαρμογή web σας είναι το πρώτο βήμα. Στη συνέχεια, θα πρέπει να πείτε Azure Active Directory για την εφαρμογή σας καθώς και. Αυτό σας επιτρέπει την εφαρμογή σας για να επικοινωνήσετε με το απλώς καταχωρημένες API Web

**Τι να και εγώ;**  

*Όπως αναφέρεται παραπάνω, το Microsoft Azure Active Directory υποστηρίζει προσθέτοντας δύο τύπους εφαρμογών. Web API που προσφέρουν υπηρεσίες για τους χρήστες και τις εφαρμογές (είτε στο web ή σε μια εφαρμογή που εκτελείται σε μια συσκευή) που έχουν πρόσβαση αυτών των API Web. Σε αυτό το βήμα καταχώρηση της εφαρμογής σε αυτό το δείγμα. Θα πρέπει να κάνετε για αυτήν την εφαρμογή για να μπορέσετε να αίτηση για πρόσβαση στο το API Web έχετε καταχωρήσει. Azure Active Directory θα αρνηθεί να ακόμη και να επιτρέψετε την εφαρμογή σας για να ζητήσετε εισόδου, εκτός εάν έχει καταχωρηθεί! Που είναι μέρος της ασφάλειας του μοντέλου.*

*Εδώ θα σας υπό την προϋπόθεση ότι καταχώρηση αυτού του δείγματος εφαρμογής αναφέρεται παραπάνω, αλλά αυτό λειτουργεί για οποιαδήποτε εφαρμογή που αναπτύσσετε.*

**Γιατί να να τοποθετείτε μια εφαρμογή και ένα API Web σε ένα μισθωτή;**

*Όπως που ενδέχεται να έχετε μαντέψει, ενδέχεται να μπορείτε να δημιουργήσετε μια εφαρμογή που αποκτά πρόσβαση σε μια εξωτερική API που έχει καταχωρηθεί στο Azure Active Directory από μια άλλη μισθωτή. Εάν κάνετε αυτό, οι πελάτες σας θα του ζητηθεί να τη συγκατάθεσή σας με τη χρήση του API της εφαρμογής. Είναι καλό είναι το τμήμα, βιβλιοθήκη ελέγχου ταυτότητας Active Directory για iOS αναλαμβάνει αυτό συγκατάθεση για εσάς! Καθώς λαμβάνουμε πιο σύνθετες δυνατότητες, θα δείτε αυτό είναι ένα σημαντικό τμήμα της εργασίας που απαιτείται για την πρόσβαση την οικογένεια προγραμμάτων των API του Microsoft από το Azure και Office, καθώς και οποιαδήποτε άλλη υπηρεσία παροχής υπηρεσιών. Προς το παρόν, επειδή έχετε καταχωρήσει τόσο το Web API και εφαρμογή κάτω από το ίδιο μισθωτή δεν θα δείτε τις οδηγίες για τη συγκατάθεσή. Αυτό συμβαίνει συνήθως, εάν δημιουργείτε μια εφαρμογή του μόνο για την εταιρεία σας για να χρησιμοποιήσετε.*

1. Είσοδος στην [πύλη διαχείρισης Azure](https://manage.windowsazure.com).
2. Κάντε κλικ στην υπηρεσία καταλόγου Active Directory στο την πλοήγηση αριστερής
3. Κάντε κλικ στο μισθωτή του καταλόγου όπου θέλετε να καταγράψετε το δείγμα εφαρμογής.
4. Κάντε κλικ στην καρτέλα εφαρμογές.
5. Στο το σχέδιο, κάντε κλικ στην επιλογή Προσθήκη.
6. Κάντε κλικ στην επιλογή "Προσθήκη εφαρμογής ανάπτυξη εταιρεία μου".
7. Πληκτρολογήστε ένα φιλικό όνομα για την εφαρμογή, για παράδειγμα "TodoListClient Android", επιλέξτε "εγγενή εφαρμογή προγράμματος-πελάτη", και κάντε κλικ στο κουμπί Επόμενο.
8. Για το URI ανακατεύθυνση, εισαγάγετε `http://TodoListClient`.  Κάντε κλικ στο κουμπί Τέλος.
9. Κάντε κλικ στην καρτέλα ρύθμιση παραμέτρων της εφαρμογής.
10. Εύρεση της τιμής Αναγνωριστικό υπολογιστή-πελάτη και αντιγραφή του Εξοικονομήστε, θα χρειαστεί αυτό αργότερα κατά τη ρύθμιση παραμέτρων της εφαρμογής σας.
11. Στις "Δικαιώματα σε άλλες εφαρμογές", κάντε κλικ στην επιλογή "Προσθήκη εφαρμογής".  Επιλέξτε "Άλλο" στην αναπτυσσόμενη λίστα "Εμφάνιση" και κάντε κλικ στο επάνω σημάδι ελέγχου.  Εντοπίστε και κάντε κλικ στην εντολή το TodoListService και κάντε κλικ στο σημάδι ελέγχου κάτω για να προσθέσετε την εφαρμογή.  Επιλέξτε "Access TodoListService" από την αναπτυσσόμενη λίστα "Ανάθεση δικαιωμάτων" και αποθηκεύστε τη ρύθμιση παραμέτρων.



Για να δημιουργήσετε με Maven, μπορείτε να χρησιμοποιήσετε το pom.xml στο ανώτατο επίπεδο

  * Δημιουργία διπλότυπου αυτό repo σε σε έναν κατάλογο της επιλογής σας:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * Ακολουθήστε τα βήματα στην [ενότητα Prerequests ρύθμισης σας maven για android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * Ρύθμιση προσομοίωσης με SDK 19
  * Μεταβείτε στον ριζικό φάκελο όπου κλωνοποιηθεί το repo
  * Εκτελέστε την εντολή: mvn καθαρή εγκατάσταση
  * Αλλάξτε τον κατάλογο στο δείγμα γρήγορης εκκίνησης: cd samples\hello
  * Εκτελέστε την εντολή: mvn android: ανάπτυξη android: εκτέλεση
  * Θα πρέπει να δείτε την εκκίνηση της εφαρμογής
  * Εισαγάγετε τα διαπιστευτήρια χρήστη δοκιμή για να δοκιμάσετε!

Πακέτα βάζο θα επίσης υποβληθεί δίπλα από το πακέτο aar.

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Βήμα 4: Κάντε λήψη του Android ADAL και προσθήκη της στο χώρο εργασίας σας Έκλειψη

Έχουμε κάνει εύκολο για να έχετε πολλές επιλογές για να χρησιμοποιήσετε αυτήν τη βιβλιοθήκη στο έργο σας Android:

* Μπορείτε να χρησιμοποιήσετε τον κωδικό προέλευσης για να εισαγάγετε αυτήν τη βιβλιοθήκη στο Έκλειψη και σύνδεση στην εφαρμογή σας.
* Εάν χρησιμοποιείτε Android Studio, μπορείτε να χρησιμοποιήσετε μορφή πακέτου *aar* και να αναφέρονται τα δυαδικά δεδομένα.

####<a name="option-1-source-zip"></a>Επιλογή 1: Προέλευσης Zip

Για να κάνετε λήψη ενός αντιγράφου του κώδικα προέλευσης, κάντε κλικ στην επιλογή "Λήψη ZIP" στη δεξιά πλευρά της σελίδας ή κάντε κλικ [εδώ](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

####<a name="option-2-source-via-git"></a>Επιλογή 2: Προέλευση μέσω Git

Για να λάβετε τον κωδικό προέλευσης του SDK μέσω git απλώς πληκτρολογήστε:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>Επιλογή 3: Δυαδικών αρχείων μέσω Gradle

Μπορείτε να λάβετε τα δυαδικά αρχεία από κεντρική repo Maven. Το πακέτο AAR μπορούν να συμπεριληφθούν ως εξής στο έργο σας με AndroidStudio:

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
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>Επιλογή 4: aar μέσω Maven

Εάν χρησιμοποιείτε την προσθήκη m2e στο Έκλειψη, μπορείτε να καθορίσετε την εξάρτηση στο αρχείο σας pom.xml:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>Επιλογή 5: βάζο πακέτου μέσα σε φάκελο βιβλιοθηκών
Για να λάβετε το αρχείο βάζο από maven το repo και αποθέστε το φάκελο *βιβλιοθηκών* στο έργο σας. Πρέπει να αντιγράψετε τους πόρους που απαιτείται για το έργο σας καθώς και δεδομένου ότι τα πακέτα βάζο δεν περιλαμβάνουν τους.


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>Βήμα 5: Προσθήκη αναφορές σε Android ADAL στο έργο σας


2. Προσθέστε μια αναφορά στο έργο σας και το καθορίσετε με μια βιβλιοθήκη Android. Εάν δεν είστε βέβαιοι πώς να το κάνετε αυτό, [κάντε κλικ εδώ για περισσότερες πληροφορίες] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. Προσθέστε την εξάρτηση έργου για τον εντοπισμό σφαλμάτων σε στις ρυθμίσεις του έργου σας

4. Ενημέρωση του έργου σας AndroidManifest.xml αρχείου για να συμπεριλάβετε:

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. Δημιουργήστε μια παρουσία του AuthenticationContext στο κύριο δραστηριότητά σας. Οι λεπτομέρειες αυτής της κλήσης είναι πέρα από το πεδίο αυτού του αρχείου Readme, αλλά μπορείτε να λάβετε μια καλή έναρξης, εξετάζοντας [Android δείγμα εγγενές πρόγραμμα-πελάτη](https://github.com/AzureADSamples/NativeClient-Android). Ακολουθεί ένα παράδειγμα:

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * ΣΗΜΕΊΩΣΗ: mContext είναι ένα πεδίο σε τη δραστηριότητά σας

8. Αντιγράψτε αυτό το μπλοκ κώδικα για το χειρισμό στο τέλος της AuthenticationActivity αφού χρήστης εισαγάγει διαπιστευτήρια και λαμβάνει κωδικό εξουσιοδότησης:

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. Για να ζητήσετε από έναν κωδικό, μπορείτε να ορίσετε μια επιστροφή κλήσης

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. Τέλος, ζητήστε από έναν κωδικό χρησιμοποιώντας επιστροφή κλήσης:

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

Εξήγηση των παραμέτρων:

  * Απαιτείται πόρων και είναι ο πόρος που προσπαθείτε να αποκτήσετε πρόσβαση.
  * Απαιτείται Clientid και συνοδεύεται από την πύλη AzureAD.
  * Μπορείτε να ρυθμίσετε redirectUri ως packagename σας. Δεν απαιτείται που διατίθενται για την κλήση acquireToken.
  * PromptBehavior σάς βοηθά να ρωτήσετε για τα διαπιστευτήρια για να μεταβείτε γρήγορα σε cache και για τα cookies.
  * Επιστροφή κλήσης θα καλούνται αφού κωδικό εξουσιοδότησης ανταλλάσσονται έναν κωδικό.

  Η επιστροφή κλήσης, θα έχετε ένα αντικείμενο της AuthenticationResult που έχει accesstoken, ημερομηνία έχει λήξει, και idtoken πληροφορίες.

Προαιρετικό: **acquireTokenSilent**

Μπορείτε να καλέσετε **acquireTokenSilent** χειρισμού σε cache και ανανεώστε το διακριτικό. Παρέχει επίσης την έκδοση συγχρονισμού. Να δέχεται αναγνωριστικό χρήστη με την παράμετρο.

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **Μεσολαβητής**: εφαρμογή εταιρικής πύλης Intune της Microsoft παρέχει το στοιχείο broker. ADAL θα Χρησιμοποιήστε το λογαριασμό broker, εάν υπάρχει ένα λογαριασμό χρήστη δημιουργείται στο αυτή η υπηρεσία ελέγχου ταυτότητας και προγραμματιστής επιλέξετε να μην παραλείψετε. Για προγραμματιστές να παραλείψετε το χρήστη broker με:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 Για προγραμματιστές πρέπει να καταχωρήσετε ειδική redirectUri για χρήση broker. RedirectUri είναι στο πλαίσιο μορφή msauth://packagename/Base64UrlencodedSignature. Μπορείτε να το redirecturi για την εφαρμογή σας χρησιμοποιώντας τη δέσμη ενεργειών "brokerRedirectPrint.ps1" ή να χρησιμοποιήσετε mContext.getBrokerRedirectUri κλήση API. Υπογραφή σχετίζεται με τα πιστοποιητικά υπογραφής σας.

 Τρέχον μοντέλο broker είναι για ένα χρήστη. AuthenticationContext παρέχει API μέθοδο για να λάβετε το broker χρήστη.

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 Χρήστης Broker που θα επιστραφεί εάν ο λογαριασμός είναι έγκυρη.

 Το δηλωτικό εφαρμογή θα πρέπει να έχετε δικαιώματα για τη χρήση λογαριασμών AccountManager: http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


Με αυτόν τον οδηγό, θα πρέπει να έχετε όλα όσα χρειάζεστε για την ενοποίηση με επιτυχία με το Azure Active Directory. Για περισσότερα παραδείγματα αυτήν την εργασία, επισκεφθείτε το AzureADSamples / αποθετήριο δεδομένων σε GitHub.

## <a name="important-information"></a>Σημαντικές πληροφορίες

### <a name="customization"></a>Προσαρμογή

Βιβλιοθήκη πόρων έργου μπορούν να αντικατασταθούν από τους πόρους σας εφαρμογής. Αυτό συμβαίνει κατά τη δημιουργία της εφαρμογής σας. Για αυτόν το λόγο, μπορείτε να προσαρμόσετε τον έλεγχο ταυτότητας δραστηριότητας διάταξη τον τρόπο που θέλετε. Πρέπει να βεβαιωθείτε ότι για να διατηρήσετε το αναγνωριστικό του τα στοιχεία ελέγχου που ADAL uses(Webview).

### <a name="broker"></a>Μεσολαβητής

Στοιχείο Broker θα παραδίδονται με εφαρμογή εταιρικής πύλης Intune της Microsoft. Λογαριασμός θα δημιουργηθεί στη Διαχείριση λογαριασμού. Τύπος λογαριασμού είναι "com.microsoft.workaccount". Επιτρέπει μόνο μία SSO λογαριασμού. Θα δημιουργήσει cookie SSO για αυτόν το χρήστη μετά την ολοκλήρωση της συσκευής πρόκληση για μία από τις εφαρμογές.

### <a name="authority-url-and-adfs"></a>Αρχή έκδοσης πιστοποιητικών Url και ADFS

ADFS δεν αναγνωρίζεται ως παραγωγής STS, ώστε να πρέπει να απενεργοποιήσετε τη εντοπισμού παρουσία και παρέλθει false σε κατασκευή AuthenticationContext.

Αρχή έκδοσης πιστοποιητικών διεύθυνση url πρέπει STS παρουσία και το όνομα του μισθωτή: https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>Υποβολή ερωτημάτων στοιχείων cache

ADAL παρέχει προεπιλεγμένες cache στο SharedPreferences με μερικές απλές cache συναρτήσεις ερωτήματος. Μπορείτε να λάβετε το τρέχον cache από AuthenticationContext με:
```Java
 ITokenCacheStore cache = mContext.getCache();
```
Μπορείτε επίσης να παρέχετε την υλοποίηση cache, εάν θέλετε να προσαρμόσετε.
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL παρέχει την επιλογή για να Καθορίστε συμπεριφορά μηνύματος. PromptBehavior.Auto θα εμφανιστεί περιβάλλοντος εργασίας Χρήστη εάν ανανέωση διακριτικό δεν είναι έγκυρο και απαιτούνται διαπιστευτήρια χρήστη. PromptBehavior.Always θα παραλείψετε τη χρήση μνήμης cache και θα εμφανίζονται πάντα περιβάλλοντος εργασίας Χρήστη.

### <a name="silent-token-request-from-cache-and-refresh"></a>Σιωπηρή αίτηση διακριτικού από το cache και ανανέωσης

Αυτή η μέθοδος δεν χρησιμοποιεί το περιβάλλον εργασίας Χρήστη pop και να δεν απαιτεί μια δραστηριότητα. Θα επιστρέψει διακριτικού από το cache εάν είναι διαθέσιμη. Εάν λήξει το διακριτικό, θα προσπαθήσει να την ανανεώσετε. Εάν το διακριτικό ανανέωση έχει λήξει ή απέτυχε, θα επιστρέψει AuthenticationException.

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

Μπορείτε επίσης να κάνετε κλήση με αυτήν τη μέθοδο συγχρονισμού. Μπορείτε να ορίσετε null για επιστροφή κλήσης ή να χρησιμοποιήσετε acquireTokenSilentSync.

### <a name="diagnostics"></a>Εργαλεία διαγνωστικών

Οι παρακάτω είναι το κύριο προελεύσεις των πληροφοριών για τη διάγνωση προβλημάτων:

+ Εξαιρέσεις
+ Αρχεία καταγραφής
+ Ανιχνεύσεις δικτύου

Επίσης, σημειώστε ότι τα αναγνωριστικά συσχέτισης είναι central για να τα Διαγνωστικά στη βιβλιοθήκη. Μπορείτε να ορίσετε το συσχέτισης αναγνωριστικά σε βάση ανά αίτηση εάν θέλετε να συσχετίσετε ένα ADAL αίτηση με άλλες λειτουργίες στον κώδικα. Εάν δεν μπορείτε να ορίσετε ένα αναγνωριστικό συσχετίσεων, στη συνέχεια, ADAL θα δημιουργήσει μια τυχαία ένα and όλα καταγραφής μηνυμάτων και κλήσεων δικτύου θα έχουν σήμανση με το αναγνωριστικό συσχέτισης. Η αυτο-υπογεγραμμένο που δημιουργούνται από το αναγνωριστικό αλλαγές σε κάθε αίτηση.

#### <a name="exceptions"></a>Εξαιρέσεις

Αυτό είναι εμφανώς το πρώτης diagnostic. Προσπαθούμε να παρέχουν μηνύματα σφάλματος χρήσιμα. Εάν δεν μπορείτε να βρείτε ένα που δεν είναι χρήσιμο υποβάλετε κάποιο πρόβλημα και ενημερώστε μας. Πρέπει επίσης να δώσετε πληροφορίες συσκευή, όπως μοντέλο και SDK #.

#### <a name="logs"></a>Αρχεία καταγραφής

Μπορείτε να ρυθμίσετε τη βιβλιοθήκη για να δημιουργήσετε μηνύματα καταγραφής που μπορείτε να χρησιμοποιήσετε για να βοηθά στη διάγνωση προβλημάτων. Ρύθμιση παραμέτρων καταγραφής, κάνοντας τα εξής κλήση για να ρυθμίσετε τις παραμέτρους μιας επιστροφής κλήσης που ADAL θα χρησιμοποιήσετε για την παράδοση απενεργοποίηση κάθε μήνυμα καταγραφής όπως δημιουργείται.


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
Τα μηνύματα μπορούν να εγγραφούν σε ένα προσαρμοσμένο αρχείο καταγραφής, όπως φαίνεται παρακάτω. Δυστυχώς, δεν υπάρχει τρόπος τυπική επιτύχετε αρχεία καταγραφής από μια συσκευή. Υπάρχουν ορισμένες υπηρεσίες που μπορούν να σας βοηθήσουν με αυτό. Μπορείτε να επίσης πληκτρολογήστε το δικό σας, όπως η αποστολή του αρχείου σε ένα διακομιστή.

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>Επίπεδα καταγραφής

+ Error(Exceptions)
+ Warn(Warning)
+ Πληροφορίες (πληροφορίες σκοπούς)
+ Λεπτομερής (περισσότερες λεπτομέρειες)

Μπορείτε να ορίσετε το επίπεδο καταγραφής ως εξής:
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 Όλα τα μηνύματα καταγραφής στην οποία αποστέλλονται logcat εκτός από τις επιστροφές κλήσης προσαρμοσμένου αρχείου καταγραφής.
Μπορείτε να λάβετε καταγραφής σε ένα αρχείο logcat φόρμας ως φαίνεται belog:

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Περισσότερα παραδείγματα σχετικά με την adb εντολών: https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>Ανιχνεύσεις δικτύου

Μπορείτε να χρησιμοποιήσετε διάφορα εργαλεία για να καταγράψετε την κυκλοφορία HTTP που δημιουργεί ADAL.  Αυτό είναι ιδιαίτερα χρήσιμο εάν είστε εξοικειωμένοι με το πρωτόκολλο OAuth ή εάν θέλετε να παρέχετε πληροφορίες διαγνωστικών για Microsoft ή άλλα κανάλια υποστήριξης.

Fiddler είναι η πιο εύκολος εργαλείο ανίχνευσης HTTP.  Χρησιμοποιήστε τις παρακάτω συνδέσεις για να ορίσετε το έως σωστά εγγραφής ADAL κίνηση του δικτύου.  Για να είναι χρήσιμες είναι απαραίτητο να ρυθμίσετε τις παραμέτρους fiddler ή οποιοδήποτε άλλο εργαλείο όπως Charles, για να καταγράψετε την κυκλοφορία χωρίς κρυπτογράφηση SSL.  ΣΗΜΕΊΩΣΗ: Ανιχνεύσεις που δημιουργούνται με αυτόν τον τρόπο μπορεί να περιέχει ιδιαίτερα δικαιώματα πληροφορίες όπως διακριτικά πρόσβασης, τα ονόματα των χρηστών και κωδικούς πρόσβασης.  Εάν χρησιμοποιείτε λογαριασμούς παραγωγής, δεν κάνετε κοινή χρήση αυτές τις ανιχνεύσεις με 3η μέρη.  Εάν θέλετε να δώσετε μια ανίχνευση σε κάποιον για να λάβετε υποστήριξη, αναπαραγάγετε το ζήτημα με ένα προσωρινό λογαριασμό με τα ονόματα των χρηστών και κωδικούς πρόσβασης που δεν σας πειράζει κοινής χρήσης.

+ [Ρύθμιση Fiddler για Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [Ρύθμιση παραμέτρων Fiddler κανόνες για ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>Παράθυρο διαλόγου λειτουργία
η μέθοδος acquireToken χωρίς δραστηριότητα υποστηρίζει εμφάνιση μηνύματος παραθύρου διαλόγου.

### <a name="encryption"></a>Κρυπτογράφηση

ADAL κρυπτογραφεί τα διακριτικά και αποθήκευσης σε SharedPreferences από προεπιλογή. Μπορείτε να δείτε την κλάση StorageHelper για να δείτε τις λεπτομέρειες. Android Παρουσιάστηκε AndroidKeyStore για την ασφαλή αποθήκευση 4.3(API18) ιδιωτικά κλειδιά. ADAL που χρησιμοποιεί για API18 και άνω. Εάν θέλετε να χρησιμοποιήσετε ADAL για κάτω SDK εκδόσεις, πρέπει να παρέχουν μυστικό κλειδί στο AuthenticationSettings.INSTANCE.setSecretKey

### <a name="oauth2-bearer-challenge"></a>Πρόκληση Oauth2 φορέα

Κλάση AuthenticationParameters παρέχει λειτουργικότητα για να λάβετε το authorization_uri από το Oauth2 φορέα πρόκλησης.

### <a name="session-cookies-in-webview"></a>Τα cookies περιόδου λειτουργίας στο προβολής Web

Android προβολής Web δεν καταργήστε τα cookies περιόδου λειτουργίας μετά την εφαρμογή είναι κλειστό. Μπορείτε να αντιμετωπίσετε αυτό το ζήτημα με το παρακάτω δείγμα κώδικα:
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Περισσότερα σχετικά με τα cookies: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>Αντικαθιστά τη ρύθμιση ο πόρος

Η βιβλιοθήκη ADAL περιλαμβάνει στα Αγγλικά συμβολοσειρές για τα ακόλουθα δύο μηνύματα ProgressDialog.

Εφαρμογή σας θα πρέπει να αντικαταστήσετε τους, εάν θέλετε μεταφρασμένες συμβολοσειρές.

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>Παράθυρο διαλόγου NTLM
Adal έκδοση 1.1.0 υποστηρίζει το παράθυρο διαλόγου NTLM που υποβάλλεται σε επεξεργασία μέσω του συμβάντος onReceivedHttpAuthRequest από WebViewClient. Παράθυρο διαλόγου διάταξη και συμβολοσειρές μπορούν να προσαρμοστούν.

### <a name="cross-app-sso"></a>Εφαρμογή σταυρό SSO
Μάθετε [Πώς να ενεργοποιείτε σταυρό εφαρμογή SSO σε Android με ADAL](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
