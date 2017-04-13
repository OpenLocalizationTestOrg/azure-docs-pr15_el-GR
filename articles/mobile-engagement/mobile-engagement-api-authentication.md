<properties 
    pageTitle="Ο έλεγχος ταυτότητας με κινητή δέσμευση APIs ΥΠΌΛΟΙΠΟ"
    description="Περιγράφει τον τρόπο για τον έλεγχο ταυτότητας με Azure Mobile δέσμευση REST API" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/05/2016"
    ms.author="wesmc;ricksal"/>

# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Ο έλεγχος ταυτότητας με κινητή δέσμευση APIs ΥΠΌΛΟΙΠΟ

## <a name="overview"></a>Επισκόπηση

Αυτό το έγγραφο περιγράφει πώς μπορείτε να λάβετε μια έγκυρη Oauth AAD διακριτικού για τον έλεγχο ταυτότητας με το κινητό δέσμευση REST API. 

Θεωρείται ότι έχετε ένα έγκυρο Azure συνδρομής και έχετε δημιουργήσει μια εφαρμογή Mobile δέσμευση χρησιμοποιώντας ένα από τα [Προγράμματα εκμάθησης για προγραμματιστές](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Έλεγχος ταυτότητας

Ένα Microsoft Azure Active Directory με βάση διακριτικό διακριτικό χρησιμοποιείται για τον έλεγχο ταυτότητας. 

Για την αίτηση ελέγχου ταυτότητας API, πρέπει να προστεθεί μια κεφαλίδα εξουσιοδότησης σε κάθε αίτηση η οποία έχει την εξής μορφή:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

>[AZURE.NOTE] Azure Active Directory διακριτικά λήξει σε 1 ώρα.

Υπάρχουν πολλοί τρόποι για να λάβετε ένα διακριτικό. Επειδή τα API ονομάζονται γενικά από μια υπηρεσία cloud, που θέλετε να χρησιμοποιήσετε ένα πλήκτρο API. Ένα κλειδί API στο Azure ορολογία ονομάζεται κεφαλαίου κωδικού πρόσβασης υπηρεσίας. Η παρακάτω διαδικασία περιγράφει ένας τρόπος για να ρυθμίσετε με μη αυτόματο τρόπο.

### <a name="one-time-setup-using-script"></a>Μοναδική εγκατάσταση (με χρήση δέσμης ενεργειών)

Θα πρέπει να ακολουθήσετε το σύνολο των παρακάτω οδηγίες για να εκτελέσετε το πρόγραμμα εγκατάστασης χρησιμοποιώντας μια δέσμη ενεργειών του PowerShell που λαμβάνει τον ελάχιστο χρόνο για τη ρύθμιση, αλλά χρησιμοποιεί τις προεπιλογές πιο αποδεκτή. Προαιρετικά, μπορείτε επίσης να ακολουθήστε τις οδηγίες στο θέμα τη [μη αυτόματη ρύθμιση](mobile-engagement-api-authentication-manual.md) για αυτήν τη διαδικασία από την πύλη του Azure απευθείας και να κάνετε πιο ομαλή ρύθμισης παραμέτρων. 

1. Λάβετε την πιο πρόσφατη έκδοση του Azure PowerShell από [εδώ](http://aka.ms/webpi-azps). Για περισσότερες πληροφορίες σχετικά με τις οδηγίες λήψης, μπορείτε να δείτε αυτήν τη [σύνδεση](../powershell-install-configure.md).  

2. Μόλις εγκαταστήσετε Azure PowerShell, χρησιμοποιήστε τις παρακάτω εντολές για να βεβαιωθείτε ότι έχετε εγκαταστήσει **λειτουργική μονάδα Azure** :

    μια. Βεβαιωθείτε ότι τη λειτουργική μονάδα Azure PowerShell είναι διαθέσιμες στη λίστα διαθέσιμες λειτουργικές μονάδες. 
    
        Get-Module –ListAvailable 

    ![Διαθέσιμες λειτουργικές μονάδες Azure][1]
        
    β. Εάν δεν μπορείτε να βρείτε τη λειτουργική μονάδα Azure PowerShell στη λίστα παραπάνω, στη συνέχεια, πρέπει να εκτελέσετε τα παρακάτω:
        
        Import-Module Azure 
        
3. Σύνδεση με τη διαχείριση πόρων Azure από το PowerShell, εκτελέστε την παρακάτω εντολή και παρέχει το όνομα χρήστη και τον κωδικό πρόσβασης για το λογαριασμό σας Azure: 
        
        Login-AzureRmAccount

4. Εάν έχετε πολλές συνδρομές, στη συνέχεια, θα πρέπει να εκτελέσετε τις ακόλουθες ενέργειες:

    μια. Λάβετε μια λίστα με όλες τις συνδρομές σας και αντιγράψτε την SubscriptionId της συνδρομής που θέλετε να χρησιμοποιήσετε. Βεβαιωθείτε ότι αυτή η συνδρομή είναι το ίδιο που περιλαμβάνει την εφαρμογή δέσμευση Mobile που πρόκειται να αλληλεπιδρούν με τα API. 

        Get-AzureRmSubscription

    β. Εκτελέστε την ακόλουθη εντολή δίνει την SubscriptionId για να ρυθμίσετε τη συνδρομή που θα χρησιμοποιηθεί.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>

5. Αντιγράψτε το κείμενο για τη [Δημιουργία AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) δέσμη ενεργειών στον τοπικό υπολογιστή και αποθηκεύστε το ως ένα cmdlet του PowerShell (π.χ. `APIAuth.ps1`) και εκτελέστε τον `.\APIAuth.ps1`. 
    
6. Η δέσμη ενεργειών θα σας ζητήσει να παρέχουν μια εισαγωγής για **principalName**. Δώστε ένα κατάλληλο όνομα εδώ ότι θέλετε να χρησιμοποιήσετε για να δημιουργήσετε την εφαρμογή υπηρεσίας καταλόγου Active Directory (π.χ., APIAuth). 

7. Μόλις ολοκληρωθεί η δέσμη ενεργειών, θα εμφανίσει τα ακόλουθα τέσσερα τιμές που θα χρειαστείτε για τον έλεγχο ταυτότητας μέσω προγραμματισμού με AD επομένως βεβαιωθείτε ότι για να τα αντιγράψετε. 
        
    **TenantId**, **SubscriptionId**, **αναγνωριστικά εφαρμογής**και **μυστικό**.

    Θα χρησιμοποιήσετε TenantId ως `{TENANT_ID}`, αναγνωριστικά εφαρμογής ως `{CLIENT_ID}` και μυστικού ως `{CLIENT_SECRET}`.

    > [AZURE.NOTE] Η προεπιλεγμένη πολιτική ασφαλείας ενδέχεται να αποκλείσει από την εκτέλεση μιας δεσμών ενεργειών του PowerShell. Εάν Ναι, ρυθμίζετε προσωρινά την πολιτική εκτέλεσης σας για να επιτρέψετε την εκτέλεση της δέσμης ενεργειών χρησιμοποιώντας την ακόλουθη εντολή:

        > Set-ExecutionPolicy RemoteSigned

8. Παρακάτω θα δείτε πώς θα μοιάζει το σύνολο των cmdlet του PS. 

    ![][3]

9. Ελέγξτε στην πύλη διαχείρισης Azure ότι μια νέα εφαρμογή AD δημιουργήθηκε με το όνομα που παρέχονται στη δέσμη ενεργειών που ονομάζεται **principalName** στην περιοχή **Εμφάνιση εφαρμογές η εταιρεία μου είναι ο κάτοχος**.

    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Βήματα για να λάβετε ένα έγκυρο διακριτικό

1. Καλέστε το API με τις ακόλουθες παραμέτρους και βεβαιωθείτε ότι για να αντικαταστήσετε το ΜΙΣΘΩΤΉ\_Αναγνωριστικό, προγράμματος-ΠΕΛΆΤΗ\_Αναγνωριστικό και προγράμματος-ΠΕΛΆΤΗ\_ΜΥΣΤΙΚΌ:

    - **Αίτηση διεύθυνσης URL** ως *https://login.microsoftonline.com/ {ΜΙΣΘΩΤΉ\_Αναγνωριστικό} / oauth2/διακριτικό*
    - **Κεφαλίδα τύπου περιεχομένου HTTP** ως *εφαρμογή/x-www-form-urlencoded*
    - **Σώμα αίτηση HTTP** ως *Εκχώρηση\_τύπος = προγράμματος-πελάτη\_διαπιστευτήρια & client_id = {προγράμματος-ΠΕΛΆΤΗ\_Αναγνωριστικό} & client_secret = {προγράμματος-ΠΕΛΆΤΗ\_ΜΥΣΤΙΚΌ} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*

    Ακολουθεί ένα παράδειγμα αίτηση:

        POST /{TENANT_ID}/oauth2/token HTTP/1.1
        Host: login.microsoftonline.com
        Content-Type: application/x-www-form-urlencoded
        grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
        urce=https%3A%2F%2Fmanagement.core.windows.net%2F

    Ακολουθεί ένα παράδειγμα απόκριση:

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Content-Length: 1234
    
        {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
        5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}

    Αυτό το παράδειγμα περιλαμβάνονται κωδικοποίηση URL των παραμέτρων ΔΗΜΟΣΊΕΥΣΗ, `resource` τιμή είναι στην πραγματικότητα `https://management.core.windows.net/`. Να είστε προσεκτικοί επίσης διεύθυνση URL κωδικοποιείτε `{CLIENT_SECRET}` καθώς μπορεί να περιέχει ειδικούς χαρακτήρες.

    > [AZURE.NOTE] Για τη δοκιμή, μπορείτε να χρησιμοποιήσετε ένα εργαλείο προγράμματος-πελάτη HTTP όπως [Fiddler](http://www.telerik.com/fiddler) ή [επέκταση Chrome Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 

2. Τώρα σε κάθε κλήση API, συμπεριλάβετε την κεφαλίδα αίτησης εξουσιοδότησης:

        Authorization: Bearer {ACCESS_TOKEN}

    Εάν λάβετε ένα 401 κατάσταση που επιστράφηκε, ελέγξτε σώμα απόκρισης, αυτό μπορεί να ενημερώσει το διακριτικό έχει λήξει. Σε αυτή την περίπτωση, λάβετε έναν νέο κωδικό.

##<a name="using-the-apis"></a>Χρησιμοποιώντας τα API

Τώρα που έχετε έναν έγκυρο διακριτικό, είστε έτοιμοι για τις κλήσεις API.

1. Στην πρόσκληση σε κάθε API, θα χρειαστεί να περάσει έγκυρη, υπολείπεται διακριτικό που έχετε λάβει στην προηγούμενη ενότητα.

2. Θα πρέπει να συνδέσετε ορισμένες παράμετροι στην αίτηση URI το οποίο προσδιορίζει την εφαρμογή σας. Η αίτηση URI μοιάζει με το ακόλουθο

        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/

    Για να λάβετε τις παραμέτρους, κάντε κλικ στο όνομα της εφαρμογής σας και κάντε κλικ στην επιλογή Πίνακας εργαλείων και θα δείτε μια σελίδα, όπως τα εξής με όλες τις παραμέτρους 3.

    - **1**`{subscription-id}`
    - **2**`{app-collection}`
    - **3**`{app-resource-name}`
    - **4** η ομάδα πόρων όνομα θα γίνει **MobileEngagement** , εκτός εάν δημιουργηθεί ένα νέο. 

    ![Παράμετροι URI API δέσμευση Mobile][2]

>[AZURE.NOTE] <br/>
>1. Παράβλεψη τη διεύθυνση ριζικό API όπως αυτή ήταν για την προηγούμενη API.<br/>
>2. Εάν έχετε δημιουργήσει την εφαρμογή με κλασική Azure πύλη, στη συνέχεια, πρέπει να χρησιμοποιήσετε το όνομα του πόρου εφαρμογών που είναι διαφορετικό από το όνομα της εφαρμογής. Εάν έχετε δημιουργήσει την εφαρμογή στην πύλη του Azure, στη συνέχεια, θα πρέπει να χρησιμοποιείτε το ίδιο (δεν υπάρχει καμία διαφοροποίηση μεταξύ όνομα πόρου της εφαρμογής και όνομα εφαρμογής για τις εφαρμογές που έχουν δημιουργηθεί με τη νέα πύλη) το όνομα εφαρμογής.  

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



