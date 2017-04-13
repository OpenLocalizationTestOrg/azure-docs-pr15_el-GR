<properties 
    pageTitle="Ρύθμιση παραμέτρων πολιτικών παράδοσης περιουσιακών στοιχείων με το .NET SDK | Microsoft Azure" 
    description="Αυτό το θέμα δείχνει πώς μπορείτε να ρυθμίσετε τις πολιτικές παράδοσης διαφορετικό περιουσιακών στοιχείων με Azure Media Services .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako;mingfeiy"/>

#<a name="configure-asset-delivery-policies-with-net-sdk"></a>Ρύθμιση παραμέτρων πολιτικών παράδοσης περιουσιακών στοιχείων με το .NET SDK
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

##<a name="overview"></a>Επισκόπηση

Εάν σκοπεύετε να παράδοσης κρυπτογραφημένα στοιχεία, ένα από τα βήματα στη ροή εργασίας παράδοσης περιεχομένου Media Services ρύθμιση των παραμέτρων πολιτικών παράδοσης για πόρους. Η πολιτική παράδοσης περιουσιακών στοιχείων σας ενημερώνει για το Media Services τον τρόπο που θέλετε για το πάγιο να παραδοθεί: σε ποια ροής πρωτόκολλο θα πρέπει να σας περιουσιακών στοιχείων δυναμικά συσκευαστούν (για παράδειγμα, MPEG ΠΑΎΛΑ, HLS, ομαλή ροή ή όλες), εάν θέλετε να κρυπτογραφήσετε δυναμικά σας περιουσιακών στοιχείων ή όχι και τον τρόπο με τον (φακέλου ή κοινές κρυπτογράφησης).

Αυτό το θέμα περιγράφει γιατί και πώς μπορείτε να δημιουργήσετε και να ρυθμίσετε τις πολιτικές παράδοσης περιουσιακών στοιχείων.

>[AZURE.NOTE]Για να μπορέσετε να χρησιμοποιήσετε δυναμικές συσκευασία και δυναμική κρυπτογράφηση, πρέπει να βεβαιωθείτε ότι έχετε τουλάχιστον μία κλίμακα μονάδας (γνωστό και ως ροή μονάδα). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος για να κλιμακωθεί υπηρεσίας πολυμέσων](media-services-portal-manage-streaming-endpoints.md).
>
>Επίσης, σας περιουσιακών στοιχείων πρέπει να περιέχει ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4s ή αρχεία ομαλή ροή προσαρμόσιμη ρυθμό μετάδοσης bit.

Μπορείτε να εφαρμόσετε διαφορετικές πολιτικές στο ίδιο πάγιο. Για παράδειγμα, μπορείτε να εφαρμόσετε PlayReady κρυπτογράφησης σε ομαλή ροή και φάκελος AES κρυπτογράφηση να ΠΑΎΛΑΣ MPEG και HLS. Όλα τα πρωτόκολλα που δεν έχουν οριστεί σε μια πολιτική παράδοσης (για παράδειγμα, προσθέτετε μια μεμονωμένη πολιτική που καθορίζει μόνο HLS ως το πρωτόκολλο) θα αποκλειστούν από ροής. Η εξαίρεση σε αυτό είναι όταν έχετε καμία πολιτική παράδοσης περιουσιακών στοιχείων που ορίζονται από το καθόλου. Στη συνέχεια, όλα τα πρωτόκολλα θα επιτρέπεται στο Απαλοιφή.

Εάν θέλετε να κάνουν ένα κρυπτογραφημένο περιουσιακών στοιχείων χώρου αποθήκευσης, πρέπει να ρυθμίσετε την πολιτική παράδοσης του παγίου. Πριν σας περιουσιακού στοιχείου είναι δυνατό να διοχετεύονται, ο διακομιστής ροής καταργεί την κρυπτογράφηση χώρου αποθήκευσης και ροές το περιεχόμενό σας χρησιμοποιώντας την πολιτική καθορισμένο παράδοσης. Για παράδειγμα, για να προβάλετε σας περιουσιακών στοιχείων κρυπτογραφημένα με κλειδιού κρυπτογράφησης φακέλου για προχωρημένους πρότυπο κρυπτογράφησης (AES), ορίστε τον τύπο πολιτικής σε **DynamicEnvelopeEncryption**. Για να καταργήσετε την κρυπτογράφηση χώρου αποθήκευσης και ροή περιουσιακού στοιχείου σε Απαλοιφή, ορίστε τον τύπο πολιτικής σε **NoDynamicEncryption**. Ακολουθούν παραδείγματα που δείχνουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους των τύπων πολιτικής.

Ανάλογα με το πώς μπορείτε να ρυθμίσετε την πολιτική παράδοσης περιουσιακών στοιχείων που θα έχει τη δυνατότητα να δυναμικά συσκευασία, δυναμικά κρυπτογράφηση και ροή τα ακόλουθα πρωτόκολλα ροής: ομαλή ροή, HLS, ΠΑΎΛΑΣ MPEG και σκληροί ΔΊΣΚΟΙ ροών.

Η παρακάτω λίστα παρουσιάζει τις μορφές που μπορείτε να χρησιμοποιήσετε για τη ροή ομαλά, HLS, ΠΑΎΛΑ και σκληροί ΔΊΣΚΟΙ.

Ομαλή ροή:

{ροής τελικού σημείου υπηρεσίες πολυμέσων όνομα λογαριασμού name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{ροής τελικού σημείου υπηρεσίες πολυμέσων όνομα λογαριασμού name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG ΠΑΎΛΑ

{ροής τελικού σημείου υπηρεσίες πολυμέσων όνομα λογαριασμού name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

ΣΚΛΗΡΟΊ ΔΊΣΚΟΙ

{ροής τελικού σημείου υπηρεσίες πολυμέσων όνομα λογαριασμού name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Για οδηγίες σχετικά με τη δημοσίευση ενός περιουσιακού στοιχείου και να δημιουργήσετε μια ροή διεύθυνση URL, ανατρέξτε στο θέμα [Δημιουργία μιας ροής διεύθυνσης URL](media-services-deliver-streaming-content.md).

##<a name="considerations"></a>Ζητήματα

- Δεν μπορείτε να διαγράψετε ένα AssetDeliveryPolicy που σχετίζεται με ενός περιουσιακού στοιχείου, ενώ υπάρχει ένα προσδιοριστικό OnDemand (ροής) για το συγκεκριμένο περιουσιακών στοιχείων. Η σύσταση είναι να καταργήσετε την πολιτική από παγίου πριν από τη διαγραφή της πολιτικής.
- Μια ροή προσδιοριστικό δεν μπορεί να δημιουργηθεί σε ένα κρυπτογραφημένο περιουσιακών στοιχείων χώρου αποθήκευσης, όταν δεν υπάρχει πολιτική παράδοσης περιουσιακών στοιχείων έχει οριστεί.  Εάν παγίου δεν είναι κρυπτογραφημένα χώρου αποθήκευσης, το σύστημα θα σάς επιτρέπουν να δημιουργήσετε ένα προσδιοριστικό και ροή περιουσιακού στοιχείου σε Απαλοιφή χωρίς μια πολιτική παράδοσης περιουσιακών στοιχείων.
- Μπορείτε να έχετε πολλούς περιουσιακών στοιχείων παράδοσης πολιτικών που σχετίζονται με ένα μεμονωμένο περιουσιακών στοιχείων, αλλά μπορείτε να καθορίσετε μόνο ένας τρόπος χειρισμού μιας δεδομένης AssetDeliveryProtocol.  Δηλαδή, εάν προσπαθείτε να συνδέσετε δύο πολιτικές παράδοσης που καθορίζουν το πρωτόκολλο AssetDeliveryProtocol.SmoothStreaming που θα έχει ως αποτέλεσμα σφάλμα επειδή το σύστημα δεν γνωρίζετε ποιο θέλετε να εφαρμόσετε όταν ένας υπολογιστής-πελάτης δημιουργεί μια αίτηση ομαλή ροή.
- Εάν έχετε ενός περιουσιακού στοιχείου με μια υπάρχουσα ροή προσδιοριστικό, δεν μπορείτε να συνδέσετε μια νέα πολιτική στο πάγιο (μπορείτε να αποσύνδεση μιας υπάρχουσας πολιτικής από περιουσιακού στοιχείου, ή να ενημερώσετε μια πολιτική παράδοσης που σχετίζεται με το περιουσιακών στοιχείων).  Πρέπει πρώτα να καταργήσετε τη ροή locator, να προσαρμόσετε τις πολιτικές και, στη συνέχεια, να δημιουργήσετε ξανά το προσδιοριστικό ροής.  Μπορείτε να χρησιμοποιήσετε την ίδια locatorId όταν αναδημιουργήσετε το προσδιοριστικό ροής, αλλά θα πρέπει να εξασφαλίσετε που δεν θα προκαλέσει ζητήματα για προγράμματα-πελάτες, επειδή το περιεχόμενο έχει δυνατότητα cache από την προέλευση ή ένα CDN ροής.


##<a name="clear-asset-delivery-policy"></a>Απαλοιφή περιουσιακών στοιχείων παράδοσης πολιτικής

Καθορίζει την ακόλουθη μέθοδο **ConfigureClearAssetDeliveryPolicy** για να εφαρμόσετε δεν δυναμική κρυπτογράφηση και για παράδοση της ροής σε οποιοδήποτε από τα ακόλουθα πρωτόκολλα: πρωτόκολλα MPEG ΠΑΎΛΑ, HLS και ομαλή ροή. Ενδέχεται να θέλετε να εφαρμοστεί αυτή η πολιτική για τους πόρους σας κρυπτογραφημένα χώρου αποθήκευσης.

Για πληροφορίες σχετικά με ποιες τιμές μπορείτε να καθορίσετε κατά τη δημιουργία ενός AssetDeliveryPolicy, ανατρέξτε στην ενότητα [τύποι χρησιμοποιείται κατά τον καθορισμό AssetDeliveryPolicy](#types) .

στατική δημόσια void ConfigureClearAssetDeliveryPolicy (IAsset περιουσιακών στοιχείων) {πολιτικής IAssetDeliveryPolicy = _context. AssetDeliveryPolicies.Create ("Απαλοιφή πολιτικής", AssetDeliveryPolicyType.NoDynamicEncryption, AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

περιουσιακών στοιχείων. DeliveryPolicies.Add(policy); }

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>Πολιτική παράδοσης DynamicCommonEncryption περιουσιακών στοιχείων


Την ακόλουθη μέθοδο **CreateAssetDeliveryPolicy** δημιουργεί το **AssetDeliveryPolicy** που έχει ρυθμιστεί για να εφαρμόσετε δυναμική κοινές κρυπτογράφηση (**DynamicCommonEncryption**) σε ένα ομαλή ροή πρωτόκολλο (άλλα πρωτόκολλα θα αποκλειστούν από ροής). Η μέθοδος δέχεται δύο παραμέτρους: **περιουσιακών στοιχείων** (περιουσιακού στοιχείου στο οποίο θέλετε να εφαρμόσετε την πολιτική παράδοσης) και **IContentKey** (το πλήκτρο περιεχομένου του τύπου **CommonEncryption** , για περισσότερες πληροφορίες, ανατρέξτε στο θέμα: [Δημιουργία έναν αριθμό-κλειδί περιεχομένου](media-services-dotnet-create-contentkey.md#common_contentkey)).

Για πληροφορίες σχετικά με ποιες τιμές μπορείτε να καθορίσετε κατά τη δημιουργία ενός AssetDeliveryPolicy, ανατρέξτε στην ενότητα [τύποι χρησιμοποιείται κατά τον καθορισμό AssetDeliveryPolicy](#types) .


στατική δημόσια void CreateAssetDeliveryPolicy (IAsset περιουσιακών στοιχείων, κλειδί IContentKey) {Uri acquisitionUrl = κλειδί. GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

AssetDeliveryPolicyConfiguration λεξικό < AssetDeliveryPolicyConfigurationKey, συμβολοσειρά > νέο λεξικό < AssetDeliveryPolicyConfigurationKey, συμβολοσειρά > = {{AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},};

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }

Azure Media Services σας επιτρέπει επίσης να προσθέσετε κρυπτογράφησης Widevine. Το παρακάτω παράδειγμα παρουσιάζει PlayReady και Widevine που προστίθεται στην πολιτική παράδοσης περιουσιακών στοιχείων.


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
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

>[AZURE.NOTE]Κατά την κρυπτογράφηση με Widevine, μόνο θα μπορούν να κάνουν χρήση ΠΑΎΛΑΣ. Βεβαιωθείτε ότι για να καθορίσετε ΠΑΎΛΑ στο πρωτόκολλο παράδοσης περιουσιακών στοιχείων.


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Πολιτική παράδοσης DynamicEnvelopeEncryption περιουσιακών στοιχείων 

Η παρακάτω μέθοδος **CreateAssetDeliveryPolicy** δημιουργεί το **AssetDeliveryPolicy** που έχει ρυθμιστεί ώστε να ισχύουν δυναμικής φακέλου κρυπτογράφησης (**DynamicEnvelopeEncryption**) για πρωτόκολλα ομαλή ροή, HLS και ΠΑΎΛΑ (αν αποφασίσετε να καθορίσετε ορισμένα πρωτόκολλα, αυτά θα αποκλειστούν από ροής). Η μέθοδος δέχεται δύο παραμέτρους: **περιουσιακών στοιχείων** (περιουσιακού στοιχείου στο οποίο θέλετε να εφαρμόσετε την πολιτική παράδοσης) και **IContentKey** (το πλήκτρο περιεχομένου του τύπου **EnvelopeEncryption** , για περισσότερες πληροφορίες, ανατρέξτε στο θέμα: [Δημιουργία έναν αριθμό-κλειδί περιεχομένου](media-services-dotnet-create-contentkey.md#envelope_contentkey)).


Για πληροφορίες σχετικά με ποιες τιμές μπορείτε να καθορίσετε κατά τη δημιουργία ενός AssetDeliveryPolicy, ανατρέξτε στην ενότητα [τύποι χρησιμοποιείται κατά τον καθορισμό AssetDeliveryPolicy](#types) .   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a id="types"></a>Τύποι που χρησιμοποιούνται κατά τον καθορισμό AssetDeliveryPolicy

###<a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

###<a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>
    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


