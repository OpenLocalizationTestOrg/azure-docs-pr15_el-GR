<properties 
   pageTitle="Έλεγχος ταυτότητας Active Directory και διαχείριση πόρων | Microsoft Azure"
   description="Ένας προγραμματιστής οδηγό με οδηγίες για τον έλεγχο ταυτότητας με το API διαχείρισης πόρων Azure και την υπηρεσία καταλόγου Active Directory για την ενοποίηση εφαρμογής με άλλες συνδρομές Azure."
   services="azure-resource-manager,active-directory"
   documentationCenter="na"
   authors="dushyantgill"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="dugill;tomfitz" />

# <a name="how-to-use-azure-active-directory-and-resource-manager-to-manage-a-customers-resources"></a>Πώς να χρησιμοποιείτε Azure Active Directory και διαχείριση πόρων για τη διαχείριση ενός πελάτη πόρων

## <a name="introduction"></a>Εισαγωγή

Εάν είστε προγραμματιστής λογισμικού που πρέπει να δημιουργήσει μια εφαρμογή που διαχειρίζεται το Azure πόροι του πελάτη, αυτό το θέμα δείχνει πώς μπορείτε να τον έλεγχο ταυτότητας με το API διαχείρισης πόρων Azure και να αποκτήσει πρόσβαση σε πόρους σε άλλες συνδρομές. 

Εφαρμογή σας να αποκτήσετε πρόσβαση τα API διαχείρισης πόρων σε δύο τρόπους:

1. **Χρήστη + εφαρμογή access**: για εφαρμογές που έχει πρόσβαση σε πόρους εκ μέρους ενός χρήστη συνδεδεμένοι στο. Αυτή η προσέγγιση λειτουργεί για τις εφαρμογές, όπως εφαρμογές web και εργαλεία γραμμής εντολών, που αφορούν μόνο "αλληλεπιδραστικών διαχείρισης" Azure πόρων.
1. **Μόνο για εφαρμογή access**: για τις εφαρμογές που χρησιμοποιούν τις υπηρεσίες μονού και προγραμματισμένες εργασίες. Η εφαρμογή ταυτότητας παρέχεται άμεση πρόσβαση στους πόρους. Αυτή η προσέγγιση λειτουργεί για τις εφαρμογές που απαιτούν μακροπρόθεσμες "πρόσβαση χωρίς σύνδεση" για να Azure.

Αυτό το θέμα παρέχει οδηγίες βήμα προς βήμα για να δημιουργήσετε μια εφαρμογή που χρησιμοποιεί αυτές τις δύο μεθόδους εξουσιοδότησης. Αυτό δείχνει πώς μπορείτε να εκτελέσετε κάθε βήμα με REST API ή C#. Την πλήρη εφαρμογή ASP.NET MVC είναι διαθέσιμη στο [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

Όλα τα κώδικα για αυτό το θέμα εκτελείται ως μια εφαρμογή web που μπορείτε να δοκιμάσετε στο [http://vipswapper.azurewebsites.net/cloudsense](http://vipswapper.azurewebsites.net/cloudsense). 

## <a name="what-the-web-app-does"></a>Τι κάνει η εφαρμογή web

Η εφαρμογή web:

1. Σύμβολα στο ένας χρήστης του Azure.
2. Ζητάει από το χρήστη για να εκχωρήσετε το πρόσβαση εφαρμογής web για τη διαχείριση πόρων.
3. Λαμβάνει χρήστη + διακριτικό πρόσβασης εφαρμογής για να αποκτήσετε πρόσβαση από διαχειριστή πόρων.
4. Χρησιμοποιεί το διακριτικό (από το βήμα 3) για να καλέσετε από διαχειριστή πόρων και να εκχωρήσετε στην εφαρμογή υπηρεσίας κεφάλαιο σε ένα ρόλο στην συνδρομής, πράγμα που παρέχει πρόσβαση μακροπρόθεσμες εφαρμογή με τη συνδρομή.
5. Αποκτά πρόσβαση μόνο για εφαρμογή διακριτικού.
6. Χρησιμοποιεί το διακριτικό (από το βήμα 5) για να διαχειριστείτε πόρους στην συνδρομής μέσω της διαχείρισης πόρων.

Ακολουθεί η ροή λήξης στο τέλος της εφαρμογής web.

![Έλεγχος ταυτότητας από διαχειριστή πόρων ροής](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Ως χρήστης, μπορείτε να παρέχετε το αναγνωριστικό εγγραφής για τη συνδρομή που θέλετε να χρησιμοποιήσετε:

![παροχή αναγνωριστικό συνδρομής](./media/resource-manager-api-authentication/sample-ux-1.png)

Επιλέξτε το λογαριασμό για να χρησιμοποιήσετε για τη σύνδεση.

![Επιλέξτε το λογαριασμό](./media/resource-manager-api-authentication/sample-ux-2.png)

Δώστε τα διαπιστευτήριά σας.

![παρέχει διαπιστευτήρια](./media/resource-manager-api-authentication/sample-ux-3.png)

Παραχωρήστε πρόσβαση εφαρμογή Azure τις συνδρομές σας:
 
![Εκχώρηση πρόσβασης](./media/resource-manager-api-authentication/sample-ux-4.png)
 
Διαχειριστείτε τις συνδρομές σας στο συνδεδεμένο:

![Σύνδεση συνδρομή](./media/resource-manager-api-authentication/sample-ux-7.png)


## <a name="register-application"></a>Καταχώρηση εφαρμογής

Πριν να ξεκινήσετε κωδικοποίηση, καταχωρήστε την εφαρμογή web της με το Azure Active Directory (AD). Η καταχώρηση εφαρμογής δημιουργεί μια κεντρική ταυτότητας για την εφαρμογή σας Azure AD. Κατέχει βασικές πληροφορίες σχετικά με την εφαρμογή σας, όπως το διακριτικό Αναγνωριστικό υπολογιστή-πελάτη, απάντηση διευθύνσεις URL και τα διαπιστευτήρια που χρησιμοποιεί την εφαρμογή σας για τον έλεγχο ταυτότητας και την πρόσβαση Azure APIs διαχείριση πόρων. Η καταχώρηση εφαρμογής καταγράφει επίσης τα διάφορα δικαιώματα πληρεξούσιου που χρειάζεται η εφαρμογή σας όταν έχουν πρόσβαση σε API Microsoft εκ μέρους του χρήστη. 

Επειδή η εφαρμογή σας αποκτά πρόσβαση σε άλλη συνδρομή, πρέπει να τη ρυθμίσετε ως εφαρμογή πολλών μισθωτή. Για να μεταβιβάσετε επικύρωσης, δώστε έναν τομέα που σχετίζεται με την υπηρεσία καταλόγου Active Directory. Για να δείτε τους τομείς που σχετίζεται με την υπηρεσία καταλόγου Active Directory, συνδεθείτε [πύλη κλασική](https://manage.windowsazure.com). Επιλέξτε την υπηρεσία καταλόγου Active Directory και, στη συνέχεια, επιλέξτε **τομείς**.

Το παρακάτω παράδειγμα δείχνει πώς να καταχωρήσετε την εφαρμογή με χρήση του Azure PowerShell. Πρέπει να έχετε την πιο πρόσφατη έκδοση (Αυγούστου 2016) του Azure PowerShell για αυτήν την εντολή για να εργαστείτε. 

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true
    
Για να συνδεθείτε ως η εφαρμογή AD, χρειάζεστε το αναγνωριστικό εφαρμογής και τον κωδικό πρόσβασης. Για να δείτε το αναγνωριστικό εφαρμογής που επιστρέφεται από την προηγούμενη εντολή, χρησιμοποιήστε:

    $app.ApplicationId

Το παρακάτω παράδειγμα δείχνει πώς να καταχωρήσετε την εφαρμογή με τη χρήση Azure CLI. 

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

Τα αποτελέσματα περιλαμβάνουν το αναγνωριστικό εφαρμογής, το οποίο πρέπει κατά τον έλεγχο ταυτότητας με την εφαρμογή.

### <a name="optional-configuration---certificate-credential"></a>Προαιρετική ρύθμιση παραμέτρων - διαπιστευτηρίων πιστοποιητικού

Azure AD υποστηρίζει επίσης πιστοποιητικό διαπιστευτήρια για εφαρμογές: δημιουργείτε ένα αυτο-υπογεγραμμένου πιστοποιητικού, διατηρήσετε ιδιωτικό κλειδί και προσθέστε το δημόσιο κλειδί για την εγγραφή σας Azure AD εφαρμογής. Για τον έλεγχο ταυτότητας, την εφαρμογή σας στέλνει ένα μικρό φορτίο Azure AD συνδεδεμένοι με το δημόσιο κλειδί και Azure AD επικυρώσει την υπογραφή χρησιμοποιώντας το δημόσιο κλειδί που καταχωρήσατε.

Για πληροφορίες σχετικά με τη δημιουργία μιας εφαρμογής AD με ένα πιστοποιητικό, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure για να δημιουργήσετε μια υπηρεσία κεφαλαίου για να αποκτήσετε πρόσβαση σε πόρους](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) ή [Χρήση Azure CLI για να δημιουργήσετε μια υπηρεσία κεφαλαίου για να αποκτήσετε πρόσβαση σε πόρους](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Λήψη μισθωτή ταυτότητας από αναγνωριστικό συνδρομής

Για να ζητήσετε ενός διακριτικού που μπορούν να χρησιμοποιηθούν για να καλέσετε από διαχειριστή πόρων, την εφαρμογή σας πρέπει να γνωρίζετε το Αναγνωριστικό μισθωτή του μισθωτή του Azure AD που φιλοξενεί το Azure συνδρομής. Το πιο πιθανό τα αναγνωριστικά τους συνδρομή γνωρίζουν οι χρήστες σας, αλλά ενδέχεται να μην γνωρίζουν τους μισθωτή αναγνωριστικά για την υπηρεσία καταλόγου Active Directory. Για να λάβετε το αναγνωριστικό του χρήστη μισθωτή, ζητήστε από το χρήστη για το αναγνωριστικό συνδρομής. Όταν στέλνετε μια πρόσκληση σε σχετικά με τη συνδρομή, δώστε αυτό το αναγνωριστικό συνδρομής:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

Η αίτηση αποτυγχάνει, επειδή ο χρήστης δεν έχει συνδεθεί ακόμα, αλλά μπορείτε να ανακτήσετε το αναγνωριστικό μισθωτή από την απάντηση. Σε αυτήν την εξαίρεση, η ανάκτηση το αναγνωριστικό μισθωτή από την τιμή της κεφαλίδας απόκρισης για **Τον έλεγχο ταυτότητας WWW**. Μπορείτε να δείτε αυτήν την υλοποίηση της μεθόδου [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) .

## <a name="get-user--app-access-token"></a>Λήψη χρήστη + διακριτικό πρόσβασης εφαρμογής

Η εφαρμογή σας ανακατευθύνει το χρήστη να Azure AD με ένα διακριτικό 2.0 εγκρίνετε αίτηση - να ελέγχουν την ταυτότητα τα διαπιστευτήρια του χρήστη και να επιστρέψετε έναν κωδικό εξουσιοδότησης. Η εφαρμογή σας χρησιμοποιεί τον κωδικό εξουσιοδότησης για να λάβετε μια πρόσβαση διακριτικού για διαχείριση πόρων. Η μέθοδος [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) δημιουργεί μια αίτηση άδειας.

Αυτό το θέμα εξηγεί τις αιτήσεις REST API για τον έλεγχο ταυτότητας χρήστη. Μπορείτε επίσης να χρησιμοποιήσετε βιβλιοθήκες Βοήθειας για την εκτέλεση του ελέγχου ταυτότητας στον κώδικά σας. Για περισσότερες πληροφορίες σχετικά με αυτές τις βιβλιοθήκες, ανατρέξτε στο θέμα [Azure Active Directory ελέγχου ταυτότητας βιβλιοθήκες](./active-directory/active-directory-authentication-libraries.md). Για οδηγίες σχετικά με την ενοποίηση διαχείρισης των ταυτοτήτων σε μια εφαρμογή, ανατρέξτε στο θέμα [Οδηγός Azure Active Directory για προγραμματιστές](./active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Αίτηση ελέγχου ταυτότητας (διακριτικό 2.0)

Ζητήματος μια ανοιχτή Αναγνωριστικό σύνδεση/OAuth2.0 επιτρέπουν την αίτηση για το τελικό σημείο Azure AD εξουσιοδότηση:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

Τις παραμέτρους συμβολοσειράς ερωτήματος που είναι διαθέσιμες για αυτήν την αίτηση περιγράφονται στο θέμα [αίτηση έναν κωδικό εξουσιοδότησης](./active-directory/active-directory-protocols-oauth-code.md#request-an-authorization-code) .

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να ζητήσετε εξουσιοδότηση OAuth2.0:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD πραγματοποιεί έλεγχο ταυτότητας χρήστη και, εάν είναι απαραίτητο, ζητά από το χρήστη για να εκχωρήσετε δικαιώματα για την εφαρμογή. Επιστρέφει τον κωδικό εξουσιοδότησης στη διεύθυνση URL απάντηση της εφαρμογής σας. Ανάλογα με την απαιτούμενη response_mode, Azure AD είτε στέλνει τα δεδομένα στη συμβολοσειρά ερωτήματος ή ως καταχώρηση δεδομένων.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Αίτηση ελέγχου ταυτότητας (σύνδεση Άνοιγμα ID)

Εάν δεν μόνο επιθυμείτε να έχετε πρόσβαση από διαχειριστή πόρων Azure εκ μέρους του χρήστη, αλλά επίσης επιτρέπει στο χρήστη να πραγματοποιήσουν είσοδο στην εφαρμογή σας χρησιμοποιώντας το λογαριασμό της Azure AD, θεμάτων μια ανοιχτή Αναγνωριστικό σύνδεση επιτρέπουν την αίτηση. Με ανοιχτό Αναγνωριστικό σύνδεση με επιτυχία, η εφαρμογή σας λαμβάνει μια id_token από το Azure AD που μπορούν να χρησιμοποιούν την εφαρμογή σας για να πραγματοποιήσετε είσοδο στο χρήστη.

Τις παραμέτρους συμβολοσειράς ερωτήματος που είναι διαθέσιμες για αυτήν την αίτηση περιγράφονται στο θέμα [στείλτε την πρόσκληση εισόδου](./active-directory/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) .

Ένα παράδειγμα άνοιγμα σύνδεση Αναγνωριστικό αίτηση είναι:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD πραγματοποιεί έλεγχο ταυτότητας χρήστη και, εάν είναι απαραίτητο, ζητά από το χρήστη για να εκχωρήσετε δικαιώματα για την εφαρμογή. Επιστρέφει τον κωδικό εξουσιοδότησης στη διεύθυνση URL απάντηση της εφαρμογής σας. Ανάλογα με την απαιτούμενη response_mode, Azure AD είτε στέλνει τα δεδομένα στη συμβολοσειρά ερωτήματος ή ως καταχώρηση δεδομένων.

Ένα παράδειγμα άνοιγμα σύνδεση Αναγνωριστικό απόκρισης είναι:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Αίτηση διακριτικού (OAuth2.0 κώδικα εκχώρηση ροής)

Τώρα που η εφαρμογή σας έχει λάβει τον κωδικό εξουσιοδότησης από το Azure AD, είναι ώρα να αποκτήσει πρόσβαση διακριτικού για τη διαχείριση πόρων Azure.  Δημοσιεύστε μια OAuth2.0 κώδικα εκχώρηση διακριτικού αίτηση στο Azure AD διακριτικού τελικό σημείο: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Τις παραμέτρους συμβολοσειράς ερωτήματος που είναι διαθέσιμες για αυτήν την αίτηση περιγράφονται στο θέμα [χρήση του κωδικού άδειας](./active-directory/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) .

Το παρακάτω παράδειγμα εμφανίζει μια αίτηση για διακριτικό εκχώρηση κώδικα με κωδικό πρόσβασης διαπιστευτηρίων:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Όταν εργάζεστε με τα διαπιστευτήρια πιστοποιητικού, δημιουργήστε ένα διακριτικού JSON Web (JWT) και εισόδου (RSA SHA256) χρησιμοποιώντας το ιδιωτικό κλειδί των διαπιστευτηρίων της εφαρμογής σας πιστοποιητικό. Οι τύποι διεκδίκηση για το διακριτικό εμφανίζονται στο [JWT διακριτικού αξιώσεων](./active-directory/active-directory-protocols-oauth-code.md#jwt-token-claims). Για αναφορά, δείτε τον [κώδικα βιβλιοθήκη ελέγχου ταυτότητας Active Directory (.NET)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) για να πραγματοποιήσετε διακριτικά JWT διεκδίκηση προγράμματος-πελάτη.

Δείτε το [Άνοιγμα σύνδεση Αναγνωριστικό προδιαγραφές](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) για λεπτομέρειες σχετικά με έλεγχο ταυτότητας του προγράμματος-πελάτη. 

Το παρακάτω παράδειγμα εμφανίζει μια αίτηση για διακριτικό εκχώρηση κώδικα με το πιστοποιητικό διαπιστευτήρια:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1
    
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012
    
    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Ένα παράδειγμα απόκριση για διακριτικό εκχώρηση κώδικα: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Χειρισμός κώδικα εκχώρηση διακριτικού απόκρισης

Επιτυχής διακριτικού απόκριση περιέχει (χρήστη + εφαρμογής) διακριτικό πρόσβασης για τη διαχείριση πόρων Azure. Η εφαρμογή σας χρησιμοποιεί αυτό το διακριτικό πρόσβασης για να αποκτήσετε πρόσβαση από διαχειριστή πόρων εκ μέρους του χρήστη. Η διάρκεια ζωής των κωδικών πρόσβασης που εκδίδονται από Azure AD είναι μία ώρα. Δεν είναι πιθανό ότι χρειάζεται η εφαρμογή σας web για να ανανεώσετε (χρήστη + εφαρμογής) διακριτικό πρόσβασης. Εάν χρειάζεται να ανανεώσετε το διακριτικό πρόσβασης, χρησιμοποιήστε το διακριτικό ανανέωση που λαμβάνει την εφαρμογή σας στην διακριτικού απάντηση. Καταχώρηση ενός OAuth2.0 διακριτικού αίτηση στο Azure AD διακριτικού τελικό σημείο: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Οι παράμετροι για χρήση με την αίτηση ανανέωσης περιγράφονται στην [Ανανέωση το διακριτικό πρόσβασης](./active-directory/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο χρήσης της ανανέωσης διακριτικού:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Παρόλο που τα διακριτικά ανανέωσης μπορεί να χρησιμοποιηθεί για να λάβετε νέα διακριτικά πρόσβασης για τη διαχείριση πόρων Azure, δεν είναι κατάλληλη για πρόσβαση χωρίς σύνδεση από την εφαρμογή σας. Περιορίζεται η διάρκεια ζωής διακριτικά ανανέωση και ανανέωση διακριτικά είναι συνδεδεμένες με το χρήστη. Εάν ο χρήστης αποχωρεί από τον οργανισμό, η εφαρμογή που χρησιμοποιεί το διακριτικό ανανέωση χάνει πρόσβασης. Αυτή η προσέγγιση δεν είναι κατάλληλη για τις εφαρμογές που χρησιμοποιούνται από τις ομάδες για να διαχειριστείτε τους πόρους Azure.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Ελέγξτε εάν χρήστης μπορεί να εκχωρήσει πρόσβαση σε συνδρομή

Η εφαρμογή σας τώρα έχει έναν κωδικό πρόσβασης διαχειριστή πόρων Azure εκ μέρους του χρήστη. Το επόμενο βήμα είναι να συνδέσετε την εφαρμογή σας με τη συνδρομή. Μετά τη σύνδεση, την εφαρμογή σας μπορούν να διαχειρίζονται αυτές τις συνδρομές, ακόμα και όταν ο χρήστης δεν υπάρχει (μακροπρόθεσμες πρόσβαση χωρίς σύνδεση). 

Για κάθε εγγραφή για να συνδεθείτε, καλέστε τα [δικαιώματα διαχειριστή πόρων λίστας](https://msdn.microsoft.com/library/azure/dn906889.aspx) API για να προσδιορίσετε αν ο χρήστης έχει πρόσβαση διαχείρισης δικαιωμάτων για τη συνδρομή.

Η μέθοδος [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) της εφαρμογής δείγμα ASP.NET MVC υλοποιεί αυτήν την κλήση.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να ζητήσετε δικαιώματα χρήστη σε μια συνδρομή. 83cfe939-2402-4581-b761-4f59b0a041e4 είναι το αναγνωριστικό της συνδρομής.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Ένα παράδειγμα της απόκρισης για να λάβετε δικαιώματα του χρήστη στη συνδρομή είναι:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

Τα δικαιώματα API επιστρέφει πολλά δικαιώματα. Κάθε δικαίωμα αποτελείται από επιτρεπόμενων ενέργειες (ενέργειες) και δεν επιτρέπονται ενέργειες (notactions). Εάν υπάρχει μια ενέργεια από τη λίστα επιτρεπόμενων ενέργειες από οποιοδήποτε δικαίωμα και δεν υπάρχει στη λίστα notactions αυτό το δικαίωμα, ο χρήστης επιτρέπεται να εκτελέσετε αυτή την ενέργεια. **Microsoft.Authorization/roleassignments/Write** είναι η ενέργεια που που εκχωρεί πρόσβαση διαχείρισης δικαιωμάτων. Η εφαρμογή σας πρέπει να ανάλυση το αποτέλεσμα δικαιώματα για να αναζητήσετε ταιριάζουν regex σε αυτήν τη συμβολοσειρά ενέργεια στις ενέργειες και notactions κάθε δικαιώματος.

## <a name="get-app-only-access-token"></a>Πρόσβαση μόνο για εφαρμογή διακριτικού

Τώρα, μπορείτε να γνωρίζετε εάν ο χρήστης μπορεί να εκχωρήσει πρόσβαση στη συνδρομή Azure. Τα επόμενα βήματα είναι τα εξής:

1. Αντιστοιχίστε τον κατάλληλο ρόλο RBAC της εφαρμογής σας ταυτότητα της συνδρομής.
2. Επικυρώστε την αντιστοίχιση της access, υποβολή ερωτημάτων για την εφαρμογή δικαιωμάτων σε τη συνδρομή ή με την πρόσβαση από διαχειριστή πόρων χρησιμοποιώντας το διακριτικό μόνο για εφαρμογή.
1. Εγγραφή τη σύνδεση σε εφαρμογές "συνδεδεμένοι συνδρομές" δομή των δεδομένων σας - συνεχίζεται το αναγνωριστικό της συνδρομής.

Ας ρίξουμε μια ματιά πιο κοντά στο πρώτο βήμα. Για να αντιστοιχίσετε τον κατάλληλο ρόλο RBAC την ταυτότητα της εφαρμογής, πρέπει να καθορίσετε:

- Το αναγνωριστικό αντικειμένου του ταυτότητα της εφαρμογής σας σε του χρήστη Azure Active Directory
- Το αναγνωριστικό του ρόλου RBAC που απαιτεί την εφαρμογή της συνδρομής

Όταν η εφαρμογή σας πραγματοποιεί έλεγχο ταυτότητας χρήστη από ένα Azure AD, δημιουργεί ένα αντικείμενο κεφαλαίου υπηρεσίας για την εφαρμογή σας στο συγκεκριμένο Azure AD. Azure επιτρέπει ρόλους RBAC θα εκχωρηθούν αρχές υπηρεσίας για να εκχωρήσετε πρόσβαση απευθείας αντίστοιχο σε εφαρμογές Azure πόρους. Αυτή η ενέργεια είναι ακριβώς τι Ευχόμαστε οι για να το κάνετε. Ερώτημα το API Azure AD Graph για να προσδιορίσετε το αναγνωριστικό της αρχής υπηρεσίας της εφαρμογής σας από το χρήστη συνδεδεμένοι στο του Azure AD.

Έχετε μόνο ένα διακριτικό πρόσβασης για τη διαχείριση πόρων Azure - χρειάζεστε έναν νέο κωδικό πρόσβασης για να καλέσετε το API Azure AD Graph. Κάθε εφαρμογή σε Azure AD έχει δικαίωμα ερωτήματος δικό του κεφαλαίου αντικείμενο υπηρεσίας, ώστε να είναι αρκετό ένα διακριτικό πρόσβασης μόνο για εφαρμογή.

<a id="app-azure-ad-graph">
### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Πρόσβαση μόνο για εφαρμογή διακριτικού για το API Azure AD Graph

Για να ελέγχουν την ταυτότητα της εφαρμογής σας και λήψη ενός διακριτικού για API Azure AD Graph, υποβάλει μια αίτηση διακριτικού ροής προγράμματος-πελάτη διαπιστευτηρίων εκχώρηση OAuth2.0 Azure AD διακριτικού τελικό σημείο (**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/διακριτικό**).

Η μέθοδος [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) το δείγμα εφαρμογής ASP.net MVC λαμβάνει μόνο για εφαρμογή access διακριτικού για το Graph API χρησιμοποιώντας τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory για .NET.

Τις παραμέτρους συμβολοσειράς ερωτήματος που είναι διαθέσιμες για αυτήν την αίτηση περιγράφονται στο θέμα [αίτηση ενός διακριτικού πρόσβασης](./active-directory/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) .

Ένα παράδειγμα αίτηση για διακριτικό εκχώρηση διαπιστευτηρίων προγράμματος-πελάτη: 

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Ένα παράδειγμα απόκριση για διακριτικό εκχώρηση διαπιστευτηρίων προγράμματος-πελάτη: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Λήψη ObjectId του κεφαλαίου εφαρμογής υπηρεσίας στο χρήστη Azure AD

Τώρα, μπορείτε να χρησιμοποιήσετε το διακριτικό πρόσβασης μόνο για εφαρμογή ερώτημα για την [υπηρεσία Azure AD Graph αρχές](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API για να προσδιορίσετε το αναγνωριστικό αντικειμένου του κεφαλαίου στον κατάλογο της εφαρμογής υπηρεσίας.

Η μέθοδος [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) από το δείγμα εφαρμογής ASP.net MVC υλοποιεί αυτήν την κλήση.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να ζητήσετε μια εφαρμογή υπηρεσίας κεφάλαιο. a0448380-c346-4f9f-b897-c18733de9394 είναι το αναγνωριστικό πελάτη της εφαρμογής.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Το παρακάτω παράδειγμα εμφανίζει μια απάντηση στην αίτηση για μια εφαρμογή υπηρεσίας κεφαλαίου 

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Η λήψη αναγνωριστικού ρόλο Azure RBAC

Για να αντιστοιχίσετε τον κατάλληλο ρόλο RBAC την υπηρεσία κεφαλαίου, πρέπει να προσδιορίσετε το αναγνωριστικό του ρόλου Azure RBAC.

Το σωστό ρόλο RBAC για την εφαρμογή σας:

- Εάν η εφαρμογή σας παρακολουθεί μόνο τη συνδρομή, χωρίς να κάνετε αλλαγές, απαιτείται πρόγραμμα ανάγνωσης δικαιώματα μόνο της συνδρομής. Να αναθέσει το ρόλο **Reader** .
- Εάν η εφαρμογή σας διαχειρίζεται Azure τη συνδρομή, τη δημιουργία/τροποποίηση/διαγραφή οντοτήτων, απαιτεί ένα από τα δικαιώματα συμβολής.
  - Για να διαχειριστείτε έναν συγκεκριμένο τύπο πόρου, αντιστοιχίσετε τους ρόλους συμβολής συγκεκριμένο πόρο (εικονική μηχανή συμβολής, εικονικό συμβολής δικτύου, συμβολής λογαριασμού χώρου αποθήκευσης, κ.λπ.)
  - Για να διαχειριστείτε οποιονδήποτε τύπο πόρου, να αναθέσει το ρόλο του **συνεργάτη** .

Η εκχώρηση ρόλων για την εφαρμογή σας είναι ορατή στους χρήστες, επομένως, επιλέξτε ελάχιστο απαιτούνται δικαιώματα.

Καλέστε τον [ορισμό ρόλου διαχειριστή πόρων API](https://msdn.microsoft.com/library/azure/dn906879.aspx) σε λίστα όλους τους ρόλους Azure RBAC και αναζήτησης και, στη συνέχεια, επαναλάβετε επάνω στο αποτέλεσμα για να βρείτε τον ορισμό επιθυμητή ρόλο με βάση το όνομα.

Η μέθοδος [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) της εφαρμογής δείγμα ASP.net MVC υλοποιεί αυτήν την κλήση.

Το παρακάτω παράδειγμα αίτηση εμφανίζει πώς μπορώ να αποκτήσω Azure RBAC ρόλο αναγνωριστικό. 09cbd307-aa71-4aca-b346-5f253e6e3ebb είναι το αναγνωριστικό της συνδρομής.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Η απάντηση είναι με την εξής μορφή: 

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Δεν χρειάζεται να καλέσετε αυτό το API σε συνεχή βάση. Όταν έχετε καθορίσει το γνωστό GUID του ορισμού ρόλου, μπορείτε να δημιουργήσετε το αναγνωριστικό ορισμού ρόλο ως:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Εδώ θα βρείτε τα γνωστές GUID με τους ρόλους που χρησιμοποιούνται συχνά ενσωματωμένη:

| Ρόλος | GUID |
| ----- | ------ |
| Πρόγραμμα ανάγνωσης | acdd72a7-3385-48ef-bd42-f606fba81ae7
| Συμβολής | b24988ac-6180-42a0-ab88-20f7382dd24c
| Εικονική μηχανή συμβολής | d73bb868-a0df-4d4d-bd69-98a00b01fccb
| Εικονικό δίκτυο συμβολής | b34d265f-36f7-4a0d-a4d4-e158ca92e90f
| Συνεργάτης λογαριασμού χώρου αποθήκευσης | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25
| Τοποθεσία Web συνεργάτη | de139f84-1756-47ae-9be6-808fbbe84772
| Συνεργάτης σχεδίου Web | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b
| SQL Server συμβολής | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437
| Συνεργάτης DB SQL | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec

### <a name="assign-rbac-role-to-application"></a>Εκχώρηση ρόλου RBAC σε εφαρμογή

Έχετε όλα όσα χρειάζεστε για να αντιστοιχίσετε τον κατάλληλο ρόλο RBAC στην υπηρεσία σας κεφαλαίου, χρησιμοποιώντας τη [Διαχείριση πόρων δημιουργία εκχώρηση ρόλων](https://msdn.microsoft.com/library/azure/dn906887.aspx) API.

Η μέθοδος [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) της εφαρμογής δείγμα ASP.net MVC υλοποιεί αυτήν την κλήση.

Ένα παράδειγμα αίτηση για Ανάθεση ρόλου RBAC σε εφαρμογή: 

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

Στην πρόσκληση σε, χρησιμοποιούνται οι ακόλουθες τιμές:

| GUID | Περιγραφή |
| ------ | --------- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb | το αναγνωριστικό της συνδρομής
| c3097b31-7309-4c59-b4e3-770f8406bad2 | το αναγνωριστικό αντικειμένου του αρχικού υπηρεσίας της εφαρμογής
| acdd72a7-3385-48ef-bd42-f606fba81ae7 | το αναγνωριστικό του ρόλου reader
| 4f87261d-2816-465d-8311-70a27558df4c | νέο guid που έχει δημιουργηθεί για τη νέα εκχώρηση ρόλου

Η απάντηση είναι με την εξής μορφή: 

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Πρόσβαση μόνο για εφαρμογή διακριτικού για τη διαχείριση πόρων Azure

Για την επικύρωση εφαρμογών που έχει το επιθυμητό πρόσβαση της συνδρομής, εκτελέστε μια εργασία δοκιμής της συνδρομής με χρήση ενός διακριτικού μόνο για εφαρμογή.

Για να λάβετε μια πρόσβαση μόνο για εφαρμογή διακριτικού, ακολουθήστε οδηγίες από την ενότητα [πρόσβαση μόνο για εφαρμογή διακριτικού για το API Azure AD Graph](#app-azure-ad-graph), με μια διαφορετική τιμή για την παράμετρο πόρων: 

    https://management.core.windows.net/

Η μέθοδος [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) το δείγμα εφαρμογής ASP.NET MVC λαμβάνει μόνο για εφαρμογή access διακριτικού για τη διαχείριση πόρων Azure χρησιμοποιώντας τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory για .net.

#### <a name="get-applications-permissions-on-subscription"></a>Λήψη της εφαρμογής δικαιώματα συνδρομής

Για να βεβαιωθείτε ότι η εφαρμογή σας διαθέτει την επιθυμητή πρόσβαση σε μια συνδρομή του Azure, μπορείτε επίσης να καλέσετε τα [Δικαιώματα διαχειριστή πόρων](https://msdn.microsoft.com/library/azure/dn906889.aspx) API. Αυτή η προσέγγιση είναι παρόμοια με τον τρόπο που καθορίζεται αν ο χρήστης έχει πρόσβαση διαχείρισης δικαιωμάτων για τη συνδρομή. Ωστόσο, αυτήν τη στιγμή, καλέστε τα δικαιώματα API με το διακριτικό μόνο για εφαρμογή access που έχετε λάβει στο προηγούμενο βήμα.

Η μέθοδος [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) της εφαρμογής δείγμα ASP.NET MVC υλοποιεί αυτήν την κλήση.

## <a name="manage-connected-subscriptions"></a>Διαχείριση των συνδρομών συνδεδεμένοι

Όταν τον κατάλληλο ρόλο RBAC αντιστοιχίζεται στην υπηρεσία της εφαρμογής σας κεφαλαίου της συνδρομής, την εφαρμογή σας να διατηρήσετε παρακολούθηση/Διαχείριση χρησιμοποιώντας τα διακριτικά μόνο για εφαρμογή access για τη διαχείριση πόρων Azure.

Εάν μια κάτοχος της συνδρομής καταργεί εκχώρηση ρόλων της εφαρμογής σας χρησιμοποιώντας την πύλη κλασική ή εργαλεία γραμμής εντολών, η εφαρμογή σας δεν είναι πλέον μπορούν να έχουν πρόσβαση σε αυτήν τη συνδρομή. Σε αυτή την περίπτωση, πρέπει να ειδοποιήσετε το χρήστη που ήταν δυνατόν τη σύνδεση με τη συνδρομή από έξω από την εφαρμογή και να τους δώσετε μια επιλογή "επιδιόρθωση" της σύνδεσης. "Επιδιόρθωση" απλώς θα δημιουργήσετε ξανά την εκχώρηση ρόλων που έχει διαγραφεί χωρίς σύνδεση.

Όπως ακριβώς έχετε ενεργοποιήσει το χρήστη για να συνδεθείτε συνδρομές με την εφαρμογή σας, πρέπει να επιτρέψετε στο χρήστη να αποσυνδέσετε πολύ συνδρομές. Από μια πρόσβαση διαχείρισης άποψη, αποσύνδεση σημαίνει ότι η κατάργηση εκχώρησης ρόλου που περιλαμβάνει την εφαρμογή υπηρεσίας κεφάλαιο της συνδρομής. Προαιρετικά, οποιοδήποτε μέλος στην εφαρμογή για τη συνδρομή ενδέχεται να καταργηθούν πολύ. Μόνο οι χρήστες με δικαίωμα πρόσβασης διαχείρισης της συνδρομής, θα μπορείτε να αποσυνδέσετε τη συνδρομή.

Η [μέθοδος RevokeRoleFromServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) της εφαρμογής δείγμα ASP.net MVC υλοποιεί αυτήν την κλήση.

Που είναι η κατάλληλη επιλογή-οι χρήστες να τώρα εύκολα σύνδεση και να διαχειριστείτε τους Azure συνδρομές με την εφαρμογή σας.

