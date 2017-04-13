<properties 
    pageTitle="Προστασία του περιεχομένου με Apple FairPlay ή/και Microsoft PlayReady HLS | Microsoft Azure" 
    description="Αυτό το θέμα παρέχει μια επισκόπηση και εμφανίζει τον τρόπο χρήσης των υπηρεσιών Azure Media Services για την κρυπτογράφηση δυναμικά το περιεχόμενό σας HTTP Live ροής (HLS) με το Apple FairPlay. Εμφανίζει επίσης πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία παράδοσης Media Services άδειας χρήσης για την παράδοση FairPlay άδειες χρήσης σε υπολογιστές-πελάτες." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016"
    ms.author="juliako"/>

# <a name="protect-your-hls-content-with-apple-fairplay-andor-microsoft-playready"></a>Προστασία του περιεχομένου με Apple FairPlay ή/και Microsoft PlayReady HLS

Azure Media Services σάς επιτρέπει να κρυπτογραφήσετε δυναμικά το περιεχόμενό σας HTTP Live ροής (HLS) χρησιμοποιώντας τις παρακάτω μορφές:  

- **Απαλοιφή κλειδί AES 128 φακέλου** 

    Ολόκληρο μπλοκ είναι κρυπτογραφημένη χρησιμοποιώντας τη λειτουργία **CBC AES 128** . Η αποκρυπτογράφηση της ροής υποστηρίζεται iOS και OSX player εγγενώς. Για περισσότερες πληροφορίες, ανατρέξτε [σε αυτό το άρθρο](media-services-protect-with-aes128.md).

- **Apple FairPlay** 

    Τα μεμονωμένα δείγματα βίντεο και ήχου είναι κρυπτογραφημένα με χρήση της λειτουργίας **AES 128 CBC** . **Η ροή FairPlay** (FPS) είναι ενσωματωμένο στο τα λειτουργικά συστήματα συσκευή, με την εγγενή υποστήριξη σε iOS και Τηλεοπτική Apple. Safari OS X επιτρέπει FPS χρησιμοποιώντας υποστήριξη περιβάλλοντος εργασίας κρυπτογραφημένα επεκτάσεις πολυμέσων (EME).
- **Microsoft PlayReady**

Η παρακάτω εικόνα δείχνει τη ροή εργασίας **HLS + FairPlay ή/και PlayReady δυναμική κρυπτογράφηση** .

![Προστασία με FairPlay](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

Αυτό το θέμα περιγράφει τον τρόπο χρήσης των υπηρεσιών Azure Media Services για την κρυπτογράφηση δυναμικά το περιεχόμενό σας HLS με Apple FairPlay. Εμφανίζει επίσης πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία παράδοσης Media Services άδειας χρήσης για την παράδοση FairPlay άδειες χρήσης σε υπολογιστές-πελάτες.

>[AZURE.NOTE] Εάν θέλετε να κρυπτογραφήσετε το περιεχόμενό σας HLS με PlayReady, πρέπει να δημιουργήσετε μια κοινές κλειδί περιεχομένου και να συσχετίσετε με το περιουσιακών στοιχείων. Πρέπει επίσης να ρυθμίσετε τις παραμέτρους του περιεχομένου κλειδιού εξουσιοδότησης πολιτικής, όπως περιγράφεται στο θέμα [Χρήση PlayReady δυναμική κοινές κρυπτογράφηση](media-services-protect-with-drm.md) .

    
## <a name="requirements-and-considerations"></a>Απαιτήσεις και τα θέματα

- Απαιτούνται τα παρακάτω κατά τη χρήση αθροιστικής μέτρησης ενισχύσεων για παράδοση HLS κρυπτογραφημένα με FairPlay και για την παράδοση FairPlay άδειες χρήσης.

    - Ένας λογαριασμός Azure. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](/pricing/free-trial/?WT.mc_id=A261C142F).
    - Ένας λογαριασμός Media Services. Για να δημιουργήσετε ένα λογαριασμό Media Services, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού](media-services-portal-create-account.md).
    - Εγγραφή με [Πρόγραμμα ανάπτυξης Apple](https://developer.apple.com/).
    - Apple απαιτεί ο κάτοχος του περιεχομένου για τη λήψη του [πακέτου ανάπτυξης](https://developer.apple.com/contact/fps/). Κατάσταση της αίτησης ήδη υλοποιηθεί KSM (πλήκτρο ενότητα ασφάλεια) με υπηρεσιών Azure Media Services και ότι υποβάλετε αίτηση για το τελικό πακέτο FPS. Θα υπάρχουν οδηγίες που εμφανίζονται στο τελικό πακέτο FPS για να δημιουργήσετε πιστοποίησης και αποκτήσετε ASK, το οποίο θα χρησιμοποιήσετε για να ρυθμίσετε τις παραμέτρους FairPlay. 

    - Azure Media Services .NET SDK έκδοση **3.6.0** ή νεότερη έκδοση.

- Τα εξής στοιχεία πρέπει να οριστεί στην πλευρά κλειδιού παράδοσης αθροιστικής μέτρησης ενισχύσεων:
    - **Εφαρμογή πιστοποιητικού (AC)** - αρχείο .pfx που περιέχει ιδιωτικό κλειδί. Αυτό το αρχείο δημιουργείται από τον πελάτη και κρυπτογράφηση με κωδικό πρόσβασης στον ίδιο πελάτη. 
        
        Όταν ο πελάτης ρυθμίζει τις παραμέτρους πολιτικής κλειδιού παράδοσης, πρέπει να παρέχουν τον κωδικό πρόσβασης και το .pfx σε μορφή base64.

        Ακολουθήστε τα παρακάτω βήματα περιγράφουν τον τρόπο για να δημιουργήσετε ένα πιστοποιητικό pfx για FairPlay.
        
        1. Εγκατάσταση OpenSSL από https://slproweb.com/products/Win32OpenSSL.html
        
            Μεταβείτε στο φάκελο όπου είναι το πιστοποιητικό FairPlay και άλλα αρχεία παραδίδονται από την Apple.
        
        2. Γραμμή εντολών για να μετατρέψετε το προαιρετικό pem:
        
            "C:\OpenSSL-Win32\bin\openssl.exe" x509-ενημερώνει der-στο fairplay.cer-ανάληψη fairplay out.pem
        
        3. Γραμμή εντολών για να μετατρέψετε pem pfx με το ιδιωτικό κλειδί (στη συνέχεια, ζητείται τον κωδικό πρόσβασης για το αρχείο pfx από OpenSSL).
        
            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-εξαγωγή - ανάληψη fairplay out.pfx-inkey privatekey.pem-στο fairplay out.pem - passin file:privatekey-pem-pass.txt 
        
    - **Κωδικού πρόσβασης πιστοποιητικού εφαρμογή** - πελάτη τον κωδικό πρόσβασης για να δημιουργήσετε το αρχείο .pfx.
    - **Αναγνωριστικό του κωδικού πρόσβασης πιστοποιητικού εφαρμογή** - πελάτη πρέπει να αποστείλετε τον κωδικό πρόσβασης που είναι παρόμοιο με το πώς αποστολή άλλα πλήκτρα αθροιστικής μέτρησης ενισχύσεων και με τιμή απαρίθμησης **ContentKeyType.FairPlayPfxPassword** . Ως αποτέλεσμα, θα δουν αθροιστικής μέτρησης ενισχύσεων αναγνωριστικό αυτό είναι ό, τι χρειάζονται για να χρησιμοποιήσετε μέσα στην επιλογή πολιτικής κλειδιού παράδοσης.
    - **iv** - 16 byte τυχαία τιμή, πρέπει να συμφωνεί με το iv στην πολιτική παράδοσης περιουσιακών στοιχείων. Ο πελάτης δημιουργεί το IV και το τοποθετεί σε δύο θέσεις: περιουσιακών στοιχείων παράδοσης πολιτικής και κλειδί πολιτικής επιλογή παράδοσης. 
    - **ΕΡΏΤΗΣΗ** - ASK (πλήκτρο εφαρμογής μυστικό) λαμβάνεται κατά τη δημιουργία της πιστοποίησης με πύλη Apple προγραμματιστής. Κάθε ομάδα ανάπτυξης θα λάβει μια μοναδική ASK. Αποθηκεύστε ένα αντίγραφο του η ΕΡΏΤΗΣΗ και αποθηκεύστε το σε ασφαλές σημείο. Θα πρέπει να ρυθμίσετε τις παραμέτρους ASK ως FairPlayAsk να υπηρεσιών Azure Media Services αργότερα. 
    -  **Ζητήστε από ID** - λαμβάνονται όταν ο πελάτης αποστέλλει ASk σε αθροιστικής μέτρησης ενισχύσεων. Ο πελάτης πρέπει να αποστείλετε ASk με τιμή απαρίθμησης **ContentKeyType.FairPlayASk** . Ως αποτέλεσμα, επιστρέφεται το αναγνωριστικό αθροιστικής μέτρησης ενισχύσεων και αυτό είναι τι πρέπει να χρησιμοποιείται κατά τη ρύθμιση στην επιλογή πολιτικής κλειδιού παράδοσης.

- Πρέπει να οριστούν τα εξής στοιχεία από την πλευρά του προγράμματος-πελάτη FPS:
    - **Εφαρμογή πιστοποιητικού (AC)** -.cer/.der αρχείο που περιέχει δημόσιο κλειδί που χρησιμοποιεί λειτουργικό σύστημα για την κρυπτογράφηση ορισμένες φορτίο. Αθροιστικής μέτρησης ενισχύσεων πρέπει να γνωρίζετε σχετικά με το γιατί απαιτείται από το πρόγραμμα αναπαραγωγής. Η υπηρεσία παράδοσης κλειδιού αποκρυπτογραφεί χρησιμοποιώντας το αντίστοιχο ιδιωτικό κλειδί.

- Για αναπαραγωγή μια ροή κρυπτογραφημένο FairPlay, πρέπει να λάβετε πραγματικό ASK πρώτη και, στη συνέχεια, να δημιουργήσετε ένα πιστοποιητικό πραγματικό. Η διαδικασία σύνδεσης δημιουργεί όλα τα τμήματα 3:

    -  .DER, 
    -  .pfx και 
    -  ο κωδικός πρόσβασης για το .pfx.
 
- Προγράμματα-πελάτες που υποστηρίζουν HLS με κρυπτογράφηση **AES 128 CBC** : Safari OS X, Apple Τηλεόραση, iOS.

##<a name="steps-for-configuring-fairplay-dynamic-encryption-and-license-delivery-services"></a>Βήματα για τη ρύθμιση των παραμέτρων FairPlay δυναμική κρυπτογράφηση και την άδεια χρήσης υπηρεσίες παράδοσης

Οι παρακάτω είναι γενικά βήματα που θα πρέπει να εκτελέσετε κατά την προστασία τους πόρους σας με FairPlay, χρησιμοποιώντας την υπηρεσία παράδοσης άδειας χρήσης Media Services και χρησιμοποιώντας επίσης δυναμική κρυπτογράφηση.

1. Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε τα αρχεία σε περιουσιακού στοιχείου. 
1. Κωδικοποίηση παγίου που περιέχει το αρχείο για να το προσαρμόσιμη ρυθμό μετάδοσης bit MP4 ρύθμιση.
1. Δημιουργήστε έναν αριθμό-κλειδί περιεχομένου και να συσχετίσετε με το κωδικοποιημένο περιουσιακών στοιχείων.  
1. Ρύθμιση παραμέτρων πολιτική εξουσιοδότησης του αριθμού-κλειδιού του περιεχομένου. Κατά τη δημιουργία της πολιτικής εξουσιοδότησης κλειδιού περιεχομένου, πρέπει να καθορίσετε τα εξής: 
    
    - Μέθοδος παράδοσης (σε αυτήν την περίπτωση, FairPlay), 
    - Ρύθμιση παραμέτρων επιλογών πολιτικής FairPlay. Για λεπτομέρειες σχετικά με τον τρόπο ρύθμισης των παραμέτρων FairPlay, ανατρέξτε στην ενότητα μέθοδος ConfigureFairPlayPolicyOptions() στο δείγμα παρακάτω.
    
        >[AZURE.NOTE] Συνήθως, θα θέλετε να ρυθμίσετε επιλογές πολιτική FairPlay μόνο μία φορά, επειδή θα έχετε μόνο ένα σύνολο πιστοποίησης και ASK.
-περιορισμούς (ανοιχτό ή διακριτικού) - και συγκεκριμένες πληροφορίες για τον τύπο κλειδιού παράδοσης που καθορίζει τον τρόπο τον αριθμό-κλειδί παραδίδεται στον υπολογιστή-πελάτη. 
    
2. Ρύθμιση πολιτικής παράδοσης περιουσιακών στοιχείων. Η ρύθμιση παραμέτρων της πολιτικής παράδοσης περιλαμβάνει τα εξής: 

    - παράδοση protocol (HLS), 
    - τον τύπο της δυναμικής κρυπτογράφησης (κοινές CBC κρυπτογράφηση), 
    - διεύθυνση URL απόκτηση της άδειας χρήσης. 
    
    >[AZURE.NOTE]Εάν θέλετε να κάνουν μια ροή που είναι κρυπτογραφημένα με FairPlay + άλλο DRM, πρέπει να ρύθμιση παραμέτρων πολιτικών ξεχωριστή παράδοσης 
    >
    >- Μία IAssetDeliveryPolicy για ρύθμιση παραμέτρων ΠΑΎΛΑΣ με CENC (PlayReady + WideVine) και ομαλές με PlayReady. 
    >- Ένα άλλο IAssetDeliveryPolicy για ρύθμιση παραμέτρων FairPlay για HLS

1. Δημιουργήστε ένα προσδιοριστικό OnDemand για να λάβετε μια ροή διεύθυνση URL.

##<a name="using-fairplay-key-delivery-by-playerclient-apps"></a>Χρήση FairPlay κλειδιού παράδοσης με εφαρμογές του προγράμματος αναπαραγωγής/προγράμματος-πελάτη

Οι πελάτες θα μπορούσε να αναπτύξτε εφαρμογές προγράμματος αναπαραγωγής χρησιμοποιώντας iOS SDK. Για να μπορέσετε να κάνετε αναπαραγωγή περιεχομένου FairPlay πελάτες πρέπει να υλοποιήσετε πρωτόκολλο άδεια χρήσης του exchange. Το πρωτόκολλο exchange άδεια χρήσης δεν έχει καθοριστεί από την Apple. Είναι έως και κάθε εφαρμογή πώς μπορείτε να στείλετε προσκλήσεις κλειδιού παράδοσης. Τις υπηρεσίες κλειδιού παράδοσης αθροιστικής μέτρησης ενισχύσεων FairPlay αναμένει το SPC αναμένεται ως μήνυμα κωδικοποιημένο δημοσίευση www-form-url με την εξής μορφή: 

    spc=<Base64 encoded SPC>

>[AZURE.NOTE] Azure Media Player δεν υποστηρίζει την αναπαραγωγή FairPlay από το πλαίσιο. Οι πελάτες πρέπει να αποκτήσετε το πρόγραμμα αναπαραγωγής δείγματος από το λογαριασμό για προγραμματιστές Apple για να λάβετε FairPlay αναπαραγωγή στο MAC OSX. 
 
##<a name="streaming-urls"></a>Ροή διευθύνσεις URL

Σας περιουσιακών στοιχείων ήταν κρυπτογραφημένα με περισσότερες από μία DRM, θα πρέπει να χρησιμοποιήσετε μια ετικέτα κρυπτογράφησης στη ροή διεύθυνση URL: (μορφή = 'm3u8-aapl' κρυπτογράφησης = 'xxx').

Ισχύουν τα ακόλουθα θέματα:

- Μπορεί να καθοριστεί μόνο μηδέν ή έναν τύπο κρυπτογράφησης.
- Τύπος κρυπτογράφησης δεν πρέπει να έχει καθοριστεί στη διεύθυνση url, εάν μόνο μία κρυπτογράφησης έχει εφαρμοστεί σε περιουσιακού στοιχείου.
- Τύπος κρυπτογράφησης είναι διάκριση πεζών-κεφαλαίων.
- Μπορείτε να καθορίσετε τους ακόλουθους τύπους κρυπτογράφησης:  
    - **cenc**: κοινές κρυπτογράφησης (Playready ή Widevine)
    - **cbcs aapl**: Fairplay
    - **CBC**: κρυπτογράφηση AES φακέλου.


##<a name="net-example"></a>Παράδειγμα .NET


Το παρακάτω παράδειγμα παρουσιάζει λειτουργικότητα που παρουσιάστηκε στο Azure Media Services SDK για .net-έκδοση 3.6.0 (η δυνατότητα χρήσης των υπηρεσιών Azure Media Services για την παράδοση του περιεχομένου που είναι κρυπτογραφημένα με FairPlay). Η παρακάτω εντολή πακέτο Nuget που χρησιμοποιήθηκε για την εγκατάσταση του πακέτου:

    PM> Install-Package windowsazure.mediaservices -Version 3.6.0


1. Δημιουργήστε ένα έργο κονσόλας.
1. Χρησιμοποιήστε NuGet για να εγκαταστήσετε και να προσθέσετε Azure Media Services .NET SDK.
2. Προσθήκη επιπλέον αναφορές: System.Configuration.
2. Προσθήκη αρχείου ρύθμισης παραμέτρων που περιέχει το όνομα λογαριασμού και πληροφορίες κλειδιού:
    
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
              <appSettings>
            
                <add key="MediaServicesAccountName" value="AccountName"/>
                <add key="MediaServicesAccountKey" value="AccountKey"/>
            
                <add key="Issuer" value="http://testacs.com"/>
                <add key="Audience" value="urn:test"/>
              </appSettings>
        </configuration>

1. Λήψη τουλάχιστον μία ροή μονάδας για τη ροή τελικό σημείο από τον οποίο σκοπεύετε να παράδοσης το περιεχόμενό σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα: [Ρύθμιση παραμέτρων ροής τελικά σημεία](media-services-dotnet-get-started.md#configure-streaming-endpoint-using-the-portal).

1. Αντικατάσταση του κώδικα στο αρχείο σας Program.cs με τον κωδικό που εμφανίζονται σε αυτήν την ενότητα.
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
        using Newtonsoft.Json;
        using System.Security.Cryptography.X509Certificates;
        
        namespace DynamicEncryptionWithFairPlay
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                private static readonly Uri _sampleAudience =
                    new Uri(ConfigurationManager.AppSettings["Audience"]);
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
                    Console.WriteLine();
        
                    if (tokenRestriction)
                        tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                    else
                        AddOpenAuthorizationPolicy(key);
        
                    Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
                    Console.WriteLine();
        
                    CreateAssetDeliveryPolicy(encodedAsset, key);
                    Console.WriteLine("Created asset delivery policy. \n");
                    Console.WriteLine();
        
                    if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
                    {
                        // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                        // back into a TokenRestrictionTemplate class instance.
                        TokenRestrictionTemplate tokenTemplate =
                            TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
        
                        // Generate a test token based on the the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                                                                DateTime.UtcNow.AddDays(365));
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);
        
                    Console.ReadLine();
                }
        
                static public IAsset UploadFileAndCreateAsset(string singleFilePath)
                {
                    if (!File.Exists(singleFilePath))
                    {
                        Console.WriteLine("File does not exist.");
                        return null;
                    }
        
                    var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);
        
                    var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    var policy = _context.AccessPolicies.Create(
                                            assetName,
                                            TimeSpan.FromDays(30),
                                            AccessPermissions.Write | AccessPermissions.List);
        
                    var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
        
                    Console.WriteLine("Upload {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    locator.Delete();
                    policy.Delete();
        
                    return inputAsset;
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
                {
                    var encodingPreset = "H264 Multiple Bitrate 720p";
        
                    IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                            inputAsset.Name,
                                            encodingPreset));
        
                    var mediaProcessors =
                        _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();
        
                    var latestMediaProcessor =
                        mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
        
                    ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
                    encodeTask.InputAssets.Add(inputAsset);
                    encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset),    AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
                {
                    // Create HLS SAMPLE AES encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryptionCbcs);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy          
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                            {
                                new ContentKeyAuthorizationPolicyRestriction
                                {
                                    Name = "Open",
                                    KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                                    Requirements = null
                                }
                            };
        
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.FairPlay,
                        restrictions,
                        FairPlayConfiguration);
        
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key.
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                            {
                                new ContentKeyAuthorizationPolicyRestriction
                                {
                                    Name = "Token Authorization Policy",
                                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                                    Requirements = tokenTemplateString,
                                }
                            };
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                               ContentKeyDeliveryType.FairPlay,
                               restrictions,
                               FairPlayConfiguration);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
                }
        
                private static string ConfigureFairPlayPolicyOptions()
                {
                    // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK. 
                    // However, for production you must use a real ASK from Apple bound to a real prod certificate.
                    byte[] askBytes = Guid.NewGuid().ToByteArray();
                    var askId = Guid.NewGuid();
                    // Key delivery retrieves askKey by askId and uses this key to generate the response.
                    IContentKey askKey = _context.ContentKeys.Create(
                                            askId,
                                            askBytes,
                                            "askKey",
                                            ContentKeyType.FairPlayASk);
        
                    //Customer password for creating the .pfx file.
                    string pfxPassword = "<customer password for creating the .pfx file>";
                    // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
                    var pfxPasswordId = Guid.NewGuid();
                    byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
                    IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                                            pfxPasswordId,
                                            pfxPasswordBytes,
                                            "pfxPasswordKey",
                                            ContentKeyType.FairPlayPfxPassword);
        
                    // iv - 16 bytes random value, must match the iv in the asset delivery policy.
                    byte[] iv = Guid.NewGuid().ToByteArray();
        
                    //Specify the .pfx file created by the customer.
                    var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);
        
                    string FairPlayConfiguration =
                        Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                            appCert,
                            pfxPassword,
                            pfxPasswordId,
                            askId,
                            iv);
        
                    return FairPlayConfiguration;
                }
        
                static private string GenerateTokenRequirements()
                {
                    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
        
                    template.PrimaryVerificationKey = new SymmetricVerificationKey();
                    template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
                    template.Audience = _sampleAudience.ToString();
                    template.Issuer = _sampleIssuer.ToString();
                    template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
        
                    return TokenRestrictionTemplateSerializer.Serialize(template);
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();
        
                    var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);
        
                    FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);
        
                    // Get the FairPlay license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);
        
                    // The reason the below code replaces "https://" with "skd://" is because
                    // in the IOS player sample code which you obtained in Apple developer account, 
                    // the player only recognizes a Key URL that starts with skd://. 
                    // However, if you are using a customized player, 
                    // you can choose whatever protocol you want. 
                    // For example, "https". 

                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                            {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
                        };
        
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
                        AssetDeliveryProtocol.HLS,
                        assetDeliveryPolicyConfiguration);
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
        
                }
        
        
                /// <summary>
                /// Gets the streaming origin locator.
                /// </summary>
                /// <param name="assets"></param>
                /// <returns></returns>
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                 EndsWith(".ism")).
                                                 FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                        TimeSpan.FromDays(30),
                        AccessPermissions.Read);
        
                    // Create a locator to the streaming content on an origin. 
                    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                        policy,
                        DateTime.UtcNow.AddMinutes(-5));
        
                    // Create a URL to the manifest file. 
                    return originLocator.Path + assetFile.Name;
                }
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int length)
                {
                    var returnValue = new byte[length];
        
                    using (var rng =
                        new System.Security.Cryptography.RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(returnValue);
                    }
        
                    return returnValue;
                }
            }
        }


##<a name="next-steps-media-services-learning-paths"></a>Επόμενα βήματα: Media Services διαδικασίες εκμάθησης

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
