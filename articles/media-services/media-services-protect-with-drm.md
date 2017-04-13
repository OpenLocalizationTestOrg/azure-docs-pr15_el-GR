<properties
    pageTitle="Χρήση PlayReady ή/και Widevine δυναμική κοινές κρυπτογράφηση | Microsoft Azure"
    description="Microsoft Azure Media Services σάς επιτρέπει να κάνετε ροές MPEG-ΔΙΑΚΕΚΟΜΜΈΝΗ ομαλή ροή και Http Live ροής (HLS) που προστατεύεται με Microsoft PlayReady DRM. Σας επιτρέπει επίσης να παράδοσης ΠΑΎΛΑΣ κρυπτογραφημένα με Widevine DRM. Αυτό το θέμα δείχνει πώς μπορείτε να κρυπτογραφήσετε δυναμικά με PlayReady και Widevine DRM."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="09/27/2016"
    ms.author="juliako"/>


#<a name="using-playready-andor-widevine-dynamic-common-encryption"></a>Χρήση PlayReady ή/και Widevine δυναμικής κοινές κρυπτογράφησης

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-drm.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

Microsoft Azure Media Services σάς επιτρέπει να κάνετε ροές MPEG-ΔΙΑΚΕΚΟΜΜΈΝΗ ομαλή ροή και HTTP Live ροής (HLS) που προστατεύεται με [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). Σας επιτρέπει επίσης να κάνουν κρυπτογραφημένες ροές ΠΑΎΛΑΣ με Widevine DRM άδειες χρήσης. PlayReady και Widevine είναι κρυπτογραφημένα ανά της προδιαγραφής κοινές κρυπτογράφησης (ISO/IEC CENC 23001-7). Μπορείτε να χρησιμοποιήσετε [Αθροιστικής μέτρησης ενισχύσεων .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (ξεκινώντας με την έκδοση 3.5.1) ή REST API για να ρυθμίσετε τις παραμέτρους του AssetDeliveryConfiguration για να χρησιμοποιήσετε Widevine.

Υπηρεσίες πολυμέσων παρέχει μια υπηρεσία για την παροχή PlayReady και Widevine DRM άδειες χρήσης. Υπηρεσίες πολυμέσων παρέχει επίσης API που σας επιτρέπουν να διαμορφώσετε τα δικαιώματα και τους περιορισμούς που θέλετε για το περιβάλλον εκτέλεσης PlayReady ή Widevine DRM για να επιβάλετε όταν ένας χρήστης αναπαράγει προστασία περιεχομένου. Όταν ένας χρήστης ζητά ένα περιεχόμενο με προστασία DRM, η εφαρμογή του προγράμματος αναπαραγωγής θα ζητήσετε μια άδεια χρήσης από την υπηρεσία αθροιστικής μέτρησης ενισχύσεων άδεια χρήσης. Η υπηρεσία άδειας χρήσης αθροιστικής μέτρησης ενισχύσεων θα θεμάτων μια άδεια χρήσης για το πρόγραμμα αναπαραγωγής εάν έχει εξουσιοδότηση. Μια άδεια χρήσης PlayReady ή Widevine περιέχει το κλειδί αποκρυπτογράφησης που μπορούν να χρησιμοποιηθούν από το πρόγραμμα αναπαραγωγής του προγράμματος-πελάτη για να αποκρυπτογραφήσετε και ροή του περιεχομένου.


Μπορείτε επίσης να χρησιμοποιήσετε την παρακάτω συνεργάτες αθροιστικής μέτρησης ενισχύσεων θα σας βοηθήσουν να κάνουν Widevine άδειες χρήσης: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα: ενοποίηση με το [Axinom](media-services-axinom-integration.md) και [castLabs](media-services-castlabs-integration.md).

Υπηρεσίες πολυμέσων υποστηρίζει πολλούς τρόπους εξουσιοδότησης οι χρήστες που κάνουν αιτήσεις κλειδιού. Η πολιτική περιεχομένου εξουσιοδότησης κλειδιού θα μπορούσε να έχει ένα ή περισσότερα περιορισμοί άδειας: άνοιγμα ή διακριτικό περιορισμού. Η πολιτική διακριτικού περιορισμένα πρέπει να συνοδεύεται από ένα διακριτικό εκδοθεί από μια ασφαλούς διακριτικού υπηρεσίας (STS). Υπηρεσίες πολυμέσων υποστηρίζει τα διακριτικά σε το [Απλό διακριτικά Web](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) και της μορφής [JSON Web διακριτικού](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα ρύθμιση παραμέτρων πολιτική εξουσιοδότησης του αριθμού-κλειδιού του περιεχομένου.

Για να επωφεληθείτε από δυναμική κρυπτογράφηση, πρέπει να έχετε ενός περιουσιακού στοιχείου που περιέχει ένα σύνολο πολλούς-ρυθμό μετάδοσης bit MP4 αρχεία ή αρχεία προέλευσης ομαλή ροή πολλούς-ρυθμό μετάδοσης bit. Πρέπει επίσης να ρυθμίσετε τις παραμέτρους των πολιτικών παράδοσης του παγίου (που περιγράφεται παρακάτω σε αυτό το θέμα). Στη συνέχεια, με βάση τη μορφή που καθορίζεται στη ροή διεύθυνση URL, ο διακομιστής On ζήτηση ροής εξασφαλίζει ότι η ροή παραδίδεται στο πρωτόκολλο που έχετε επιλέξει. Ως αποτέλεσμα, θα πρέπει να αποθηκεύετε και να πληρώσετε για τα αρχεία σε μορφή μία χώρου αποθήκευσης και Media Services θα δημιουργήσετε και θα λειτουργήσει την κατάλληλη απάντηση HTTP που βασίζονται σε κάθε αίτηση από ένα πρόγραμμα-πελάτη.

Αυτό το θέμα θα ήταν χρήσιμες στους προγραμματιστές που λειτουργούν σε εφαρμογές που κάνουν πολυμέσων που προστατεύεται με πολλές DRMs, όπως PlayReady και Widevine. Το θέμα που δείχνει τον τρόπο ρύθμισης παραμέτρων της υπηρεσίας PlayReady άδειας χρήσης παράδοσης με τις πολιτικές εξουσιοδότησης, έτσι ώστε μόνο εξουσιοδοτημένοι υπολογιστές-πελάτες ενδέχεται να λάβετε PlayReady ή Widevine άδειες χρήσης. Εμφανίζει επίσης πώς μπορείτε να χρησιμοποιήσετε κρυπτογράφηση δυναμική κρυπτογράφηση με PlayReady ή Widevine DRM μέσω ΠΑΎΛΑΣ.

>[AZURE.NOTE]Για να ξεκινήσετε τη χρήση δυναμική κρυπτογράφηση, πρέπει να πρώτα να αποκτήσετε τουλάχιστον μία κλίμακα μονάδας (γνωστό και ως ροή μονάδα). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος για να κλιμακωθεί υπηρεσίας πολυμέσων](media-services-portal-manage-streaming-endpoints.md).


##<a name="download-sample"></a>Λήψη δείγματος

Μπορείτε να κάνετε λήψη του δείγματος που περιγράφονται σε αυτό το άρθρο από [εδώ](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).

##<a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Ρύθμιση παραμέτρων δυναμική κοινές κρυπτογράφηση και υπηρεσίες παράδοσης DRM άδειας χρήσης

Οι παρακάτω είναι γενικά βήματα που θα πρέπει να εκτελέσετε κατά την προστασία τους πόρους σας με PlayReady, χρησιμοποιώντας την υπηρεσία παράδοσης άδειας χρήσης Media Services και χρησιμοποιώντας επίσης δυναμική κρυπτογράφηση.

1. Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε τα αρχεία σε περιουσιακού στοιχείου.
1. Κωδικοποίηση παγίου που περιέχει το αρχείο για να το προσαρμόσιμη ρυθμό μετάδοσης bit MP4 ρύθμιση.
1. Δημιουργήστε έναν αριθμό-κλειδί περιεχομένου και να συσχετίσετε με το κωδικοποιημένο περιουσιακών στοιχείων. Στις υπηρεσίες πολυμέσων, το κλειδί περιεχομένου περιέχει κλειδιού κρυπτογράφησης του παγίου.
1. Ρύθμιση παραμέτρων πολιτική εξουσιοδότησης του αριθμού-κλειδιού του περιεχομένου. Η πολιτική περιεχομένου εξουσιοδότησης κλειδιού πρέπει να έχει ρυθμιστεί από εσάς και να πληρούνται από το πρόγραμμα-πελάτη για τον αριθμό-κλειδί περιεχομένου να παραδοθεί στον υπολογιστή-πελάτη.

Κατά τη δημιουργία της πολιτικής εξουσιοδότησης κλειδιού περιεχομένου, πρέπει να καθορίσετε τα εξής: μέθοδος (PlayReady ή Widevine), περιορισμούς παράδοσης (ανοιχτό ή διακριτικού) και πληροφορίες σχετικά με τον τύπο κλειδιού παράδοσης που καθορίζει τον τρόπο τον αριθμό-κλειδί παραδίδεται στον υπολογιστή-πελάτη (πρότυπο άδειας χρήσης[PlayReady](media-services-playready-license-template-overview.md) ή [Widevine](media-services-widevine-license-template-overview.md) ).
1. Ρύθμιση παραμέτρων της πολιτικής παράδοσης για ενός περιουσιακού στοιχείου. Οι ρυθμίσεις πολιτικής παράδοσης περιλαμβάνουν: παράδοσης protocol (για παράδειγμα, MPEG ΠΑΎΛΑΣ, HLS, σκληροί ΔΊΣΚΟΙ, ομαλή ροή ή όλες), τον τύπο της δυναμική κρυπτογράφηση (για παράδειγμα, κοινά κρυπτογράφηση), PlayReady ή Widevine άδεια χρήσης απόκτηση μιας διεύθυνσης URL.

Μπορείτε να εφαρμόσετε διαφορετική πολιτική για κάθε πρωτόκολλο στο ίδιο πάγιο. Για παράδειγμα, μπορείτε να εφαρμόσετε PlayReady κρυπτογράφησης για ομαλές/ΠΑΎΛΑ και AES φακέλου για να HLS. Όλα τα πρωτόκολλα που δεν έχουν οριστεί σε μια πολιτική παράδοσης (για παράδειγμα, προσθέτετε μια μεμονωμένη πολιτική που καθορίζει μόνο HLS ως το πρωτόκολλο) θα αποκλειστούν από ροής. Η εξαίρεση σε αυτό είναι όταν έχετε καμία πολιτική παράδοσης περιουσιακών στοιχείων που ορίζονται από το καθόλου. Στη συνέχεια, όλα τα πρωτόκολλα θα επιτρέπεται στο Απαλοιφή.
1. Δημιουργήστε ένα προσδιοριστικό OnDemand για να λάβετε μια ροή διεύθυνση URL.

Θα βρείτε μια ολοκληρωμένη παράδειγμα .NET στο τέλος του θέματος.

Η παρακάτω εικόνα δείχνει τη ροή εργασίας που περιγράφονται παραπάνω. Δείτε το διακριτικό χρησιμοποιείται για τον έλεγχο ταυτότητας.

![Προστασία με PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Τα υπόλοιπα αυτό το θέμα παρέχει λεπτομερείς εξηγήσεις παραδείγματα κώδικα και συνδέσεις σε θέματα που σας δείχνουν πώς μπορείτε να επιτύχετε τις εργασίες που περιγράφονται παραπάνω.

##<a name="current-limitations"></a>Τρέχουσα περιορισμοί

Εάν κάνετε προσθήκη ή ενημέρωση μιας πολιτικής παράδοσης περιουσιακών στοιχείων, πρέπει να διαγράψετε τη συσχετισμένη locator (εάν υπάρχουν) και να δημιουργήσετε ένα νέο προσδιοριστικό.

Περιορισμός κατά την κρυπτογράφηση με Widevine με τις υπηρεσίες πολυμέσων Azure: προς το παρόν, δεν υποστηρίζονται πολλά κλειδιά περιεχομένου.

##<a name="create-an-asset-and-upload-files-into-the-asset"></a>Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε αρχεία σε παγίου

Για να διαχειριστείτε, κωδικοποίηση και ροή τα βίντεό σας, πρέπει να αποστείλετε πρώτα το περιεχόμενό σας στο Microsoft Azure Media Services. Μόλις αποσταλεί, το περιεχόμενό σας αποθηκεύονται με ασφάλεια στο cloud για περαιτέρω επεξεργασία και ροής.

Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Αποστολή αρχείων σε λογαριασμό του Media Services](media-services-dotnet-upload-files.md).

##<a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Κωδικοποίηση παγίου που περιέχει το αρχείο για να το προσαρμόσιμη ρυθμό μετάδοσης bit Ορισμός MP4

Με δυναμική κρυπτογράφηση μόνο που χρειάζεστε είναι για τη δημιουργία ενός περιουσιακού στοιχείου που περιέχει ένα σύνολο πολλούς-ρυθμό μετάδοσης bit MP4 αρχεία ή αρχεία προέλευσης ομαλή ροή πολλούς-ρυθμό μετάδοσης bit. Στη συνέχεια, με βάση την καθορισμένη μορφή στην πρόσκληση σε δηλωτικό και τμήμα, ο διακομιστής ροής On απαιτήσεων θα εξασφαλίσετε ότι θα λαμβάνετε ροής στο πρωτόκολλο που έχετε επιλέξει. Ως αποτέλεσμα, θα πρέπει να αποθηκεύετε και να πληρώσετε για τα αρχεία σε μορφή μία χώρου αποθήκευσης και υπηρεσία Media Services θα δημιουργήσετε και θα λειτουργήσει την κατάλληλη απάντηση που βασίζεται σε αιτήσεις από ένα πρόγραμμα-πελάτη. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση δυναμικής συσκευασία](media-services-dynamic-packaging-overview.md) .

Για οδηγίες σχετικά με τον τρόπο για να κωδικοποιήσετε, δείτε [πώς μπορείτε να κωδικοποιήσετε ενός περιουσιακού στοιχείου με χρήση Media Encoder τυπική](media-services-dotnet-encode-with-media-encoder-standard.md).


##<a id="create_contentkey"></a>Δημιουργήστε έναν αριθμό-κλειδί περιεχομένου και να συσχετίσετε με το κωδικοποιημένο περιουσιακών στοιχείων

Στις υπηρεσίες πολυμέσων, το κλειδί περιεχομένου περιέχει τον αριθμό-κλειδί που θέλετε για την κρυπτογράφηση ενός περιουσιακού στοιχείου με.

Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία περιεχομένου κλειδί](media-services-dotnet-create-contentkey.md).


##<a id="configure_key_auth_policy"></a>Ρύθμιση παραμέτρων της πολιτικής εξουσιοδότησης του περιεχομένου κλειδιού

Υπηρεσίες πολυμέσων υποστηρίζει πολλές τρόποι για τον έλεγχο ταυτότητας τους χρήστες που κάνουν αιτήσεις κλειδιού. Η πολιτική περιεχομένου εξουσιοδότησης κλειδιού πρέπει να έχει ρυθμιστεί από εσάς και να πληρούνται από το πρόγραμμα-πελάτη (πρόγραμμα αναπαραγωγής) για το κλειδί για να παραδοθεί στον υπολογιστή-πελάτη. Η πολιτική περιεχομένου εξουσιοδότησης κλειδιού θα μπορούσε να έχει ένα ή περισσότερα περιορισμοί άδειας: άνοιγμα ή διακριτικό περιορισμού.

Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων πολιτικής εξουσιοδότησης περιεχομένου αριθμού-κλειδιού](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).

##<a id="configure_asset_delivery_policy"></a>Ρύθμιση παραμέτρων της πολιτικής παράδοσης περιουσιακών στοιχείων 

Ρύθμιση παραμέτρων της πολιτικής παράδοσης για σας περιουσιακών στοιχείων. Ορισμένα πράγματα που περιλαμβάνει τη ρύθμιση παραμέτρων της πολιτικής παράδοσης περιουσιακών στοιχείων:

- Το URL acquisition DRM άδεια χρήσης. 
- Το περιουσιακών στοιχείων παράδοσης πρωτόκολλο (για παράδειγμα, MPEG ΠΑΎΛΑΣ, HLS, σκληροί ΔΊΣΚΟΙ, ομαλή ροή ή όλες). 
- Ο τύπος του δυναμική κρυπτογράφηση (σε αυτήν την περίπτωση, κοινά κρυπτογράφησης). 

Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων περιουσιακών στοιχείων παράδοσης πολιτικής ](media-services-rest-configure-asset-delivery-policy.md).

##<a id="create_locator"></a>Δημιουργήστε μια OnDemand ροή προσδιοριστικό για να λάβετε μια διεύθυνση URL ροής

Θα χρειαστεί να παρέχετε χρήστη σας με τη ροή διεύθυνση URL για ομαλές, ΠΑΎΛΑ ή HLS.

>[AZURE.NOTE]Εάν κάνετε προσθήκη ή ενημέρωση πολιτικής παράδοσης του περιουσιακού στοιχείου, πρέπει να διαγράψετε μια υπάρχουσα locator (εάν υπάρχουν) και να δημιουργήσετε ένα νέο προσδιοριστικό.

Για οδηγίες σχετικά με τη δημοσίευση ενός περιουσιακού στοιχείου και να δημιουργήσετε μια ροή διεύθυνση URL, ανατρέξτε στο θέμα [Δημιουργία μιας ροής διεύθυνσης URL](media-services-deliver-streaming-content.md).

##<a name="get-a-test-token"></a>Λάβετε έναν έλεγχο διακριτικού

Λάβετε έναν έλεγχο διακριτικού με βάση τον περιορισμό διακριτικού που χρησιμοποιήθηκε για την πολιτική εξουσιοδότησης κλειδιού.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

    
Μπορείτε να χρησιμοποιήσετε το [Πρόγραμμα αναπαραγωγής αθροιστικής μέτρησης ενισχύσεων](http://amsplayer.azurewebsites.net/azuremediaplayer.html) για να ελέγξετε το ροής.

##<a id="example"></a>Παράδειγμα


Το παρακάτω παράδειγμα παρουσιάζει λειτουργικότητα που παρουσιάστηκε στο Azure Media Services SDK για .net-έκδοση 3.5.2 (συγκεκριμένα, η δυνατότητα να ορίσετε ένα πρότυπο Widevine άδεια χρήσης και αίτηση για άδεια χρήσης Widevine από υπηρεσιών Azure Media Services). Η παρακάτω εντολή πακέτο Nuget που χρησιμοποιήθηκε για την εγκατάσταση του πακέτου:

    PM> Install-Package windowsazure.mediaservices -Version 3.5.2


1. Δημιουργήστε ένα νέο έργο κονσόλας.
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
    
    Βεβαιωθείτε ότι για να ενημερώσετε μεταβλητές, ώστε να οδηγούν σε φακέλους όπου βρίσκονται τα αρχεία εισόδου σας.
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
        using Newtonsoft.Json;
        
        namespace DynamicEncryptionWithDRM
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
        
                    IContentKey key = CreateCommonTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
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
        
                    // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
                    // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).
                     
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);
        
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
        
        
                static public IContentKey CreateCommonTypeContentKey(IAsset asset)
                {
                    
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryption);
        
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
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("", 
                            ContentKeyDeliveryType.Widevine, 
                            restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with no restrictions").
                                Result;
        
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
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
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.Widevine,
                                restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
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
        
                static private string ConfigurePlayReadyLicenseTemplate()
                {
                    // The following code configures PlayReady License Template using .NET classes
                    // and returns the XML string.
        
                    //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
                    //It contains a field for a custom data string between the license server and the application 
                    //(may be useful for custom app logic) as well as a list of one or more license templates.
                    PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
        
                    // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
                    // to be returned to the end users. 
                    //It contains the data on the content key in the license and any rights or restrictions to be 
                    //enforced by the PlayReady DRM runtime when using the content key.
                    PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
                    //Configure whether the license is persistent (saved in persistent storage on the client) 
                    //or non-persistent (only held in memory while the player is using the license).  
                    licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
        
                    // AllowTestDevices controls whether test devices can use the license or not.  
                    // If true, the MinimumSecurityLevel property of the license
                    // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
                    licenseTemplate.AllowTestDevices = true;
        
                    // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
                    // It grants the user the ability to playback the content subject to the zero or more restrictions 
                    // configured in the license and on the PlayRight itself (for playback specific policy). 
                    // Much of the policy on the PlayRight has to do with output restrictions 
                    // which control the types of outputs that the content can be played over and 
                    // any restrictions that must be put in place when using a given output.
                    // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
                    //then the DRM runtime will only allow the video to be displayed over digital outputs 
                    //(analog video outputs won’t be allowed to pass the content).
        
                    //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
                    // If the output protections are configured too restrictive, 
                    // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.
        
                    // For example:
                    //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);
        
                    responseTemplate.LicenseTemplates.Add(licenseTemplate);
        
                    return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
                }
        
        
                private static string ConfigureWidevineLicenseTemplate()
                {
                    var template = new WidevineMessage
                    {
                        allowed_track_types = AllowedTrackTypes.SD_HD,
                        content_key_specs = new[]
                        {
                            new ContentKeySpecs
                            {
                                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                                security_level = 1,
                                track_type = "SD"
                            }
                        },
                        policy_overrides = new
                        {
                            can_play = true,
                            can_persist = true,
                            can_renew = false
                        }
                    };
        
                    string configuration = JsonConvert.SerializeObject(template);
                    return configuration;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    // Get the PlayReady license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
                
                    // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
                    // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
                    // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
                    // to append /? KID =< keyId > to the end of the url when creating the manifest.
                    // As a result Widevine license acquisition URL will have KID appended twice, 
                    // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.
            
                    Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
                    UriBuilder uriBuilder = new UriBuilder(widevineUrl);
                    uriBuilder.Query = String.Empty;
                    widevineUrl = uriBuilder.Uri;
        
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                            {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}
        
                        };
        
                    // In this case we only specify Dash streaming protocol in the delivery policy,
                    // All other protocols will be blocked from streaming.
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryption,
                        AssetDeliveryProtocol.Dash,
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


## <a name="next-step"></a>Επόμενο βήμα

Αναθεώρηση Media Services διαδικασίες εκμάθησης.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Δείτε επίσης

[CENC με πολλούς DRM και έλεγχος πρόσβασης](media-services-cenc-with-multidrm-access-control.md)

[Ρύθμιση παραμέτρων Widevine συσκευασία με αθροιστικής μέτρησης ενισχύσεων](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Ανακοίνωση Google Widevine υπηρεσίες παράδοσης άδεια χρήσης σε υπηρεσιών Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
