<properties 
    pageTitle="Σύνδεση στο λογαριασμό υπηρεσίες πολυμέσων χρησιμοποιώντας το .NET" 
    description="Αυτό το θέμα παρουσιάζει πώς μπορείτε να συνδεθείτε με τις υπηρεσίες πολυμέσων uisng .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Σύνδεση στο λογαριασμό υπηρεσίες πολυμέσων με χρήση του SDK υπηρεσίες πολυμέσων για .NET

> [AZURE.SELECTOR]
- [ΥΠΌΛΟΙΠΟ](media-services-rest-connect-programmatically.md)
- [.NET](media-services-dotnet-connect-programmatically.md)


Αυτό το θέμα περιγράφει τον τρόπο για να αποκτήσετε μια σύνδεση μέσω προγραμματισμού στο Microsoft Azure Media Services όταν προγραμματισμού με το SDK υπηρεσίες πολυμέσων για .NET.


## <a name="connecting-to-media-services"></a>Σύνδεση με τις υπηρεσίες πολυμέσων

Για να συνδεθείτε μέσω προγραμματισμού Media Services, που πρέπει να έχετε προηγουμένως ρύθμιση λογαριασμού Azure έχει ρυθμιστεί Media Services σε αυτόν το λογαριασμό και στη συνέχεια, ρυθμίστε ένα έργο Visual Studio για την ανάπτυξη με το SDK υπηρεσίες πολυμέσων για .NET. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα ρύθμιση για την ανάπτυξη με το SDK υπηρεσίες πολυμέσων για .NET.

Στο τέλος της τη διαδικασία ρύθμισης του λογαριασμού Media Services, που λαμβάνονται τις παρακάτω τιμές απαιτείται σύνδεση. Χρησιμοποιήστε αυτές τις για να κάνετε μέσω προγραμματισμού συνδέσεων σε υπηρεσίες πολυμέσων.

- Το όνομα του λογαριασμού Media Services.

- Το κλειδί λογαριασμού Media Services.

Για να βρείτε αυτές τις τιμές, μεταβείτε στην πύλη διαχείρισης του Azure, επιλέξτε το λογαριασμό σας στην υπηρεσία πολυμέσων και κάντε κλικ στο εικονίδιο "**ΔΙΑΧΕΊΡΙΣΗ ΚΛΕΙΔΙΏΝ**" στο κάτω μέρος του παραθύρου του πύλης. Κάνοντας κλικ στο εικονίδιο δίπλα σε κάθε πλαίσιο κειμένου αντιγράφει την τιμή στο Πρόχειρο του συστήματος.


## <a name="creating-a-cloudmediacontext-instance"></a>Δημιουργία μιας παρουσίας CloudMediaContext

Για να ξεκινήσετε προγραμματισμού σε σχέση με τις υπηρεσίες πολυμέσων πρέπει να δημιουργήσετε μια παρουσία **CloudMediaContext** που αντιπροσωπεύει το περιβάλλον του διακομιστή. Το **CloudMediaContext** περιλαμβάνει αναφορές σε σημαντικό συλλογών συμπεριλαμβανομένων εργασίες, πόρους, αρχεία, πολιτικές πρόσβασης και locators.

>[AZURE.NOTE] Η κλάση **CloudMediaContext** δεν διαθέτει ασφάλεια νήματος. Θα πρέπει να δημιουργήσετε μια νέα CloudMediaContext ανά νήματος ή σύνολο των λειτουργιών.


CloudMediaContext έχει πέντε τύποι κατασκευή. Συνιστάται να χρησιμοποιήσετε κατασκευές που μεταφέρουν **MediaServicesCredentials** ως παράμετρο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα **Επαναχρησιμοποίηση διακριτικά υπηρεσία ελέγχου πρόσβασης** που ακολουθεί. 

Το παρακάτω παράδειγμα χρησιμοποιεί τη δημόσια κατασκευή CloudMediaContext(MediaServicesCredentials credentials):

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Επαναχρησιμοποίηση διακριτικά υπηρεσία ελέγχου πρόσβασης

Αυτή η ενότητα δείχνει πώς μπορείτε να χρησιμοποιήσετε ξανά τα διακριτικά υπηρεσία ελέγχου πρόσβασης με τη χρήση CloudMediaContext κατασκευές που μεταφέρουν MediaServicesCredentials ως παράμετρο.


[Έλεγχος πρόσβασης καταλόγου Azure Active Directory](https://msdn.microsoft.com/library/hh147631.aspx) (γνωστό και ως υπηρεσία ελέγχου πρόσβασης ή ACS) είναι μια υπηρεσία που βασίζεται στο cloud που αποτελούν έναν εύκολο τρόπο ελέγχου ταυτότητας και δικαιώματα χρήστη για να αποκτήσετε πρόσβαση σε εφαρμογές web τους. Microsoft Azure Media Services ελέγχου πρόσβασης για τις υπηρεσίες μέσω πρωτοκόλλου διακριτικό που απαιτεί ένα διακριτικό ACS. Υπηρεσίες πολυμέσων λαμβάνει τα διακριτικά ACS από ένα διακομιστή εξουσιοδότησης.

Κατά την ανάπτυξη με το SDK υπηρεσίες πολυμέσων, μπορείτε να επιλέξετε να μην ασχοληθείτε με τα διακριτικά, επειδή το SDK κώδικα διαχειριστές τους για εσάς. Ωστόσο, επιτρέποντας στα στο SDK πλήρως Διαχείριση τα διακριτικά ACS υποψήφιων πελατών σε περιττές διακριτικού αιτήσεις. Αίτηση διακριτικά διαρκεί και καταναλώνει πόρους του υπολογιστή-πελάτη και διακομιστή. Επίσης, ο διακομιστής ACS throttles τις αιτήσεις αν το επιτόκιο είναι πολύ υψηλή. Το όριο είναι 30 αιτήσεις ανά δευτερόλεπτο, ανατρέξτε στο θέμα [Περιορισμοί υπηρεσίας ACS](https://msdn.microsoft.com/library/gg185909.aspx) για περισσότερες λεπτομέρειες.

Ξεκινώντας από το Media Services SDK έκδοση 3.0.0.0, μπορείτε να χρησιμοποιήσετε ξανά τα διακριτικά ACS. Το κατασκευές **CloudMediaContext** που λαμβάνουν **MediaServicesCredentials** ως παράμετρο ενεργοποιήσετε την κοινή χρήση τα διακριτικά ACS ανάμεσα σε πολλά περιβάλλοντα. Η κλάση MediaServicesCredentials ενσωματώνει τα διαπιστευτήρια Media Services. Εάν ένα διακριτικό ACS είναι διαθέσιμο και είναι γνωστό το χρόνο λήξης της, μπορείτε να δημιουργήσετε μια νέα παρουσία MediaServicesCredentials με το διακριτικό και να μεταβιβάσετε κατασκευή του CloudMediaContext. Σημειώστε ότι το Media Services SDK ανανεώνει αυτόματα τα διακριτικά κάθε φορά που λήξης. Υπάρχουν δύο τρόποι για να χρησιμοποιήσετε ξανά τα διακριτικά ACS, όπως φαίνεται στα παρακάτω παραδείγματα.

- Μπορείτε να αποθηκεύσετε προσωρινά το αντικείμενο **MediaServicesCredentials** στη μνήμη (για παράδειγμα, σε μια στατική κλάση μεταβλητή). Στη συνέχεια, μεταβιβάζουν το αντικείμενο στο cache για την κατασκευή CloudMediaContext. Το αντικείμενο MediaServicesCredentials περιέχει ένα διακριτικό ACS που μπορεί να χρησιμοποιηθεί ξανά, εάν είναι εξακολουθεί να ισχύει. Εάν το διακριτικό δεν είναι έγκυρο, θα ανανεωθεί από το Media Services SDK χρησιμοποιώντας τα διαπιστευτήρια που δίδονται στην κατασκευή MediaServicesCredentials.

    Σημειώστε ότι το αντικείμενο **MediaServicesCredentials** διαθέτει ένα έγκυρο διακριτικό μετά το RefreshToken ονομάζεται. Το **CloudMediaContext** καλεί τη μέθοδο **RefreshToken** στην κατασκευή. Εάν σχεδιάζετε να αποθηκεύσετε το διακριτικό τιμές σε ένα εξωτερικό χώρο αποθήκευσης, βεβαιωθείτε ότι για να ελέγξετε εάν η τιμή TokenExpiration είναι έγκυρη πριν από την αποθήκευση των δεδομένων διακριτικού. Εάν δεν είναι έγκυρο, καλέστε RefreshToken πριν από την προσωρινή αποθήκευση.

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- Μπορείτε επίσης να αποθηκεύσετε προσωρινά τη συμβολοσειρά AccessToken και οι τιμές TokenExpiration. Οι τιμές αργότερα μπορεί να χρησιμοποιηθεί για τη δημιουργία ενός νέου αντικειμένου MediaServicesCredentials με τα δεδομένα στο cache διακριτικού.  Αυτό είναι ιδιαίτερα χρήσιμο για σενάρια όπου το διακριτικό μπορεί να είναι κοινόχρηστη με ασφάλεια μεταξύ πολλών διαδικασιών ή υπολογιστές.

    Τα ακόλουθα τμήματα κώδικα καλέσετε τις μεθόδους SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage και UpdateTokenDataInExternalStorageIfNeeded που δεν έχουν οριστεί σε αυτό το παράδειγμα. Θα μπορούσατε να ορίσετε αυτές τις μεθόδους για την αποθήκευση, ανάκτηση και ενημέρωση διακριτικού δεδομένων σε ένα εξωτερικό χώρο αποθήκευσης. 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    Χρήση του αποθηκευμένου διακριτικού τιμές για τη δημιουργία MediaServicesCredentials.


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Ενημερώστε το αντίγραφο διακριτικού σε περίπτωση που το διακριτικό ενημερώθηκε από το Media Services SDK. 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- Εάν έχετε πολλούς λογαριασμούς Media Services (για παράδειγμα, για φόρτωση κοινής χρήσης σκοπούς ή παν κατανομή) μπορείτε να αποθηκεύσετε προσωρινά αντικείμενα MediaServicesCredentials χρησιμοποιώντας τη συλλογή System.Collections.Concurrent.ConcurrentDictionary (στη συλλογή ConcurrentDictionary αντιπροσωπεύει μια συλλογή νήματος ασφαλείς από ζεύγη κλειδιού/τιμής με δυνατότητα πρόσβασης από πολλά νήματα ταυτόχρονα). Στη συνέχεια, μπορείτε να χρησιμοποιήσετε τη μέθοδο GetOrAdd για να λάβετε τα διαπιστευτήρια στο cache. 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Σύνδεση σε ένα λογαριασμό Media Services που βρίσκεται στην περιοχή Βόρειας Κίνα

Εάν ο λογαριασμός σας βρίσκεται στην περιοχή Βόρειας Κίνα, χρησιμοποιήστε την παρακάτω κατασκευή:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Για παράδειγμα:


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Αποθήκευση σύνδεσης τιμών στη ρύθμιση παραμέτρων

Είναι συνιστάται ιδιαίτερα πρακτική για την αποθήκευση τιμών σύνδεσης, ιδιαίτερα ευαίσθητα τιμές όπως το όνομα λογαριασμού και τον κωδικό πρόσβασής στη ρύθμιση παραμέτρων. Επίσης, είναι πρακτική που συνιστάται για την κρυπτογράφηση των ευαίσθητων δεδομένων ρύθμισης παραμέτρων. Μπορείτε να κρυπτογραφήσετε το αρχείο ρύθμισης παραμέτρων ολόκληρο, χρησιμοποιώντας το σύστημα κρυπτογράφησης αρχείων των Windows (EFS). Για να ενεργοποιήσετε το EFS σε ένα αρχείο, κάντε δεξί κλικ στο αρχείο, επιλέξτε **Ιδιότητες**και ενεργοποιήσετε την κρυπτογράφηση στην καρτέλα ρυθμίσεις **για προχωρημένους** . Ή μπορείτε να δημιουργήσετε μια προσαρμοσμένη λύση για την κρυπτογράφηση επιλεγμένα τμήματα ενός αρχείου ρύθμισης παραμέτρων χρησιμοποιώντας τη ρύθμιση παραμέτρων προστατευμένο. Ανατρέξτε στην ενότητα [κρυπτογράφηση ρύθμισης παραμέτρων πληροφορίες χρησιμοποιώντας προστατευμένο παράμετροι](https://msdn.microsoft.com/library/53tyfkaw.aspx).

Το ακόλουθο αρχείο App.config περιέχει τις τιμές απαιτείται σύνδεση. Τις τιμές του <appSettings> στοιχείο είναι τις απαιτούμενες τιμές που λάβατε από τη διαδικασία ρύθμισης του λογαριασμού Media Services.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


Για να ανακτήσετε τιμών σύνδεσης από τις ρυθμίσεις παραμέτρων, μπορείτε να χρησιμοποιήσετε την κλάση **ConfigurationManager** και να στη συνέχεια, αντιστοιχίστε τις τιμές στα πεδία στον κώδικά σας:
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
