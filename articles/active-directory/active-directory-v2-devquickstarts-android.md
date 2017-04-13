<properties
    pageTitle="Εφαρμογή Android v2.0 καταλόγου Azure Active Directory | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή Android που πραγματοποιεί είσοδο οι χρήστες με προσωπικό λογαριασμό Microsoft που διαθέτετε και εργασίας ή σχολείου λογαριασμούς και κλήσεις το API Graph με χρήση βιβλιοθηκών άλλου κατασκευαστή."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

#  <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Προσθήκη εισόδου σε μια εφαρμογή Android χρησιμοποιώντας μια βιβλιοθήκη τρίτων κατασκευαστών με API γραφήματος χρησιμοποιώντας το τελικό σημείο v2.0

Πλατφόρμα ταυτότητα της Microsoft χρησιμοποιεί ανοιχτά πρότυπα όπως OAuth2 και OpenID σύνδεση. Οι προγραμματιστές μπορούν να χρησιμοποιούν οποιαδήποτε βιβλιοθήκη που θέλουν να ενοποίηση με τις υπηρεσίες μας. Για να βοηθήσει τους προγραμματιστές να χρησιμοποιήσετε μας πλατφόρμα με άλλες βιβλιοθήκες, θα σας γράφετε μερικά αναλυτικές όπως το εξής για να δείχνουν τον τρόπο ρύθμισης παραμέτρων βιβλιοθήκες τρίτων κατασκευαστών για να συνδεθείτε με την πλατφόρμα ταυτότητα της Microsoft. Οι περισσότερες βιβλιοθήκες που υλοποιεί [την προδιαγραφή RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) να συνδεθείτε με την πλατφόρμα ταυτότητα της Microsoft.

Με την εφαρμογή που δημιουργεί αυτόν τον οδηγό, οι χρήστες μπορούν να εισέλθετε στο οργανισμού τους και, στη συνέχεια, αναζητήστε τον εαυτό τους στον οργανισμό τους με τη χρήση του API Graph.

Εάν είστε νέος χρήστης του OAuth2 ή να συνδεθείτε OpenID, ιδιαίτερα αυτής της ρύθμισης παραμέτρων δείγμα ίσως δεν νόημα για εσάς. Συνιστάται να διαβάσετε [2.0 πρωτόκολλα - διακριτικό 2.0 εξουσιοδότησης κώδικα ροής](active-directory-v2-protocols-oauth-code.md) για φόντο.

> [AZURE.NOTE] Ορισμένες δυνατότητες της πλατφόρμας μας που έχουν μια παράσταση στα πρότυπα OAuth2 ή OpenID σύνδεση, όπως υπό όρους πρόσβασης και διαχείριση πολιτικών υπηρεσιών Intune, απαιτούν να χρησιμοποιείτε το Microsoft Azure ταυτότητας βιβλιοθήκες Άνοιγμα αρχείου προέλευσης.

Το τελικό σημείο v2.0 δεν υποστηρίζει όλα τα σενάρια Azure Active Directory και δυνατότητες.

> [AZURE.NOTE] Για να καθορίσετε εάν θα πρέπει να χρησιμοποιήσετε το τελικό σημείο v2.0, διαβάστε σχετικά με [τους περιορισμούς v2.0](active-directory-v2-limitations.md).


## <a name="download-the-code-from-github"></a>Κάντε λήψη του κώδικα από GitHub
Τον κώδικα για αυτό το πρόγραμμα εκμάθησης διατηρείται [σε GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).  Για να παρακολουθήσετε κατά μήκος, μπορείτε να [κάνετε λήψη της εφαρμογής σκελετό ως ένα .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) ή κλωνοποίηση το σκελετό:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Μπορείτε επίσης απλώς να κάνετε λήψη το δείγμα και να ξεκινήσετε αμέσως:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Καταχώρηση εφαρμογής
Δημιουργία νέας εφαρμογής στην [πύλη καταχώρηση εφαρμογής](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ή ακολουθήστε τα λεπτομερή βήματα στο θέμα [Πώς να καταχωρήσετε μια εφαρμογή με το τελικό σημείο v2.0](active-directory-v2-app-registration.md).  Βεβαιωθείτε ότι:

- Αντιγράψτε το **Αναγνωριστικό εφαρμογής** που έχει αντιστοιχιστεί σε εφαρμογή σας, επειδή θα τον χρειαστείτε σύντομα.
- Προσθέστε την πλατφόρμα **Mobile** για την εφαρμογή σας.

> Σημείωση: Η πύλη καταχώρηση εφαρμογής παρέχει μια τιμή **Ανακατεύθυνση URI** . Ωστόσο, σε αυτό το παράδειγμα πρέπει να χρησιμοποιήσετε την προεπιλεγμένη τιμή του `https://login.microsoftonline.com/common/oauth2/nativeclient`.


## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>Λήψη στη βιβλιοθήκη τρίτων κατασκευαστών NXOAuth2 και να δημιουργήσετε ένα χώρο εργασίας

Για αυτόν τον οδηγό, μπορείτε να χρησιμοποιήσετε το OIDCAndroidLib από GitHub, που είναι μια βιβλιοθήκη OAuth2 με βάση τον κωδικό OpenID σύνδεση του Google. Το υλοποιεί το προφίλ εγγενή εφαρμογή και υποστηρίζει το τελικό σημείο εξουσιοδότησης του χρήστη. Αυτά είναι όλα τα πράγματα που θα πρέπει να την ενοποίηση με την πλατφόρμα ταυτότητα της Microsoft.

Δημιουργία διπλότυπου το repo OIDCAndroidLib στον υπολογιστή σας.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Ρύθμιση του περιβάλλοντος του Android Studio

1. Δημιουργήστε ένα νέο έργο Android Studio και αποδεχτείτε τις προεπιλογές στον οδηγό.

    ![Δημιουργία νέου έργου στο Android Studio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

    ![Συσκευές Android προορισμού](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

    ![Προσθήκη μιας δραστηριότητας mobile](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. Για να ρυθμίσετε λειτουργικές μονάδες του έργου σας, μετακινήστε το κλωνοποιημένο repo στη θέση του έργου. Μπορείτε επίσης να δημιουργήσετε το έργο και, στη συνέχεια, να το αντιγράψετε απευθείας στη θέση του έργου.

    ![Λειτουργικές μονάδες έργου](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. Ανοίξτε τις ρυθμίσεις λειτουργικές μονάδες του έργου χρησιμοποιώντας το μενού περιβάλλοντος ή χρησιμοποιώντας τη συντόμευση Ctrl + Alt + πλέ + S.

    ![Ρυθμίσεις λειτουργικές μονάδες έργου](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. Καταργήστε την προεπιλεγμένη λειτουργική μονάδα εφαρμογή επειδή θέλετε μόνο τις ρυθμίσεις του κοντέινερ έργου.

    ![Η προεπιλεγμένη λειτουργική μονάδα εφαρμογής](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. Εισαγωγή λειτουργικές μονάδες από το κλωνοποιημένο repo στο τρέχον έργο.

    ![Εισαγωγή έργου gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG)
    ![Δημιουργία νέας σελίδας λειτουργική μονάδα](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. Επαναλάβετε αυτά τα βήματα για την `oidlib-sample` λειτουργική μονάδα.

7. Επιλέξτε τις εξαρτήσεις oidclib κατά την `oidlib-sample` λειτουργική μονάδα.

    ![oidclib εξαρτήσεων από τη λειτουργική μονάδα oidlib δειγμάτων](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. Κάντε κλικ στο κουμπί **OK** και περιμένετε gradle συγχρονισμού.

    Το settings.gradle πρέπει να μοιάζει ως:

    ![Στιγμιότυπο οθόνης της settings.gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. Δημιουργία της εφαρμογής δείγμα για να βεβαιωθείτε ότι το δείγμα λειτουργεί σωστά.

    Δεν θα μπορείτε να χρησιμοποιήσετε το συγκεκριμένο με Azure Active Directory ακόμη. Θα πρέπει να ρυθμίσετε πρώτα ορισμένες τελικά σημεία. Πρόκειται για να βεβαιωθείτε ότι δεν έχετε ένα θέματα Android Studio πριν θα σας ξεκινήσετε την προσαρμογή της εφαρμογής δείγμα.

10. Δημιουργία και εκτέλεση `oidlib-sample` ως προορισμό στο Android Studio.

    ![Την πρόοδο των δειγμάτων oidlib Δόμηση](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. Διαγραφή του `app ` καταλόγου που έχει απομείνει όταν έχετε καταργήσει τη λειτουργική μονάδα από το έργο, επειδή Android Studio δεν διαγράφει το για την ασφάλεια.

    ![Δομή του αρχείου που περιλαμβάνει τον κατάλογο εφαρμογών](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. Ανοίξτε το μενού **Επεξεργασία ρυθμίσεις παραμέτρων** για να καταργήσετε την εκτέλεση ρύθμιση παραμέτρων που έχει επίσης προς τα αριστερά κατά την κατάργηση της λειτουργικής μονάδας από το έργο.

    ![Μενού "Επεξεργασία" ρυθμίσεις παραμέτρων](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![εκτέλεση ρύθμιση των παραμέτρων της εφαρμογής](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>Ρύθμιση παραμέτρων τα τελικά σημεία του δείγματος

Τώρα που έχετε το `oidlib-sample` εκτελείται με επιτυχία, ας επεξεργαστούμε ορισμένα τελικά σημεία για να λάβετε αυτήν την εργασία με Azure Active Directory.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>Ρύθμιση παραμέτρων του υπολογιστή-πελάτη σας με την επεξεργασία του αρχείου oidc_clientconf.xml

1. Επειδή χρησιμοποιείτε ροών OAuth2 μόνο για τη λήψη ενός διακριτικού και καλέστε το API Graph, ορίστε το πρόγραμμα-πελάτη για να κάνετε μόνο το OAuth2. OIDC θα προέρχονται από ένα παράδειγμα νεότερη έκδοση.

    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```

2. Ρύθμιση παραμέτρων το Αναγνωριστικό πελάτη που έχετε λάβει από την πύλη εγγραφής.

    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```

3. Ρύθμιση παραμέτρων η ανακατεύθυνση URI με τη μία παρακάτω.

    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```

4. Ρυθμίστε τις παραμέτρους του εύρους που χρειάζεστε για να αποκτήσετε πρόσβαση το API του γραφήματος.

    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

Το `User.Read` τιμή του ορίσματος `oidc_scopes` σάς επιτρέπει να διαβάσετε το βασικό προφίλ το υπογεγραμμένο από τον χρήστη.
Μπορείτε να μάθετε περισσότερα σχετικά με όλα τα διαθέσιμα τα πεδία στο [Microsoft Graph δικαιωμάτων εύρους](https://graph.microsoft.io/docs/authorization/permission_scopes).

Εάν θέλετε εξηγήσεις σχετικά με το `openid` ή `offline_access` ως εμβέλειες σε σύνδεση OpenID, ανατρέξτε στο θέμα [2.0 πρωτόκολλα - διακριτικό 2.0 εξουσιοδότησης κώδικα ροής](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>Ρύθμιση παραμέτρων του προγράμματος-πελάτη τελικά σημεία με την επεξεργασία του αρχείου oidc_endpoints.xml

- Άνοιγμα του `oidc_endpoints.xml` αρχείων και να κάνετε τις ακόλουθες αλλαγές:

    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Αυτά τα τελικά σημεία ποτέ πρέπει να αλλάξετε, εάν χρησιμοποιείτε το OAuth2 ως το πρωτόκολλο.

> [AZURE.NOTE]
Τα τελικά σημεία για `userInfoEndpoint` και `revocationEndpoint` αυτήν τη στιγμή δεν υποστηρίζονται από το Azure Active Directory. Εάν αφήσετε αυτές με την προεπιλεγμένη τιμή example.com, θα γίνει υπενθύμιση ότι δεν είναι διαθέσιμη στο δείγμα :-)


## <a name="configure-a-graph-api-call"></a>Ρύθμιση παραμέτρων σε μια κλήση Graph API

- Άνοιγμα του `HomeActivity.java` αρχείων και να κάνετε τις ακόλουθες αλλαγές:

    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Δείτε εδώ μια απλή κλήση Graph API επιστρέφει μας πληροφορίες.

Αυτές είναι όλες οι αλλαγές που πρέπει να κάνετε. Εκτελέστε το `oidlib-sample` εφαρμογή, και κάντε κλικ στην επιλογή **Είσοδος**.

Αφού που έχετε έλεγχος ταυτότητας με επιτυχία, επιλέξτε το κουμπί **Αίτηση προστατευμένη πόρων** για τον έλεγχο της κλήσης για το API Graph.

## <a name="get-security-updates-for-our-product"></a>Λήψη ενημερώσεων ασφαλείας για τα προϊόντα μας

Σας προτείνουμε να λάβετε ειδοποίηση σχετικά με περιστατικά ασφαλείας, αν επισκεφθείτε την τοποθεσία του [TechCenter ασφαλείας](https://technet.microsoft.com/security/dd252948) και εγγραφή συμβουλευτική ειδοποιήσεων ασφαλείας.
