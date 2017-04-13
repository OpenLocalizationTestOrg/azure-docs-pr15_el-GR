<properties 
    pageTitle="Δημιουργία ContentKeys με .NET" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε περιεχομένου κλειδιά που παρέχουν ασφαλή πρόσβαση σε πόρους." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="create-contentkeys-with-net"></a>Δημιουργία ContentKeys με .NET

> [AZURE.SELECTOR]
- [ΥΠΌΛΟΙΠΟ](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)

Υπηρεσίες πολυμέσων σάς επιτρέπει να δημιουργήσετε και να κάνουν κρυπτογραφημένο περιουσιακών στοιχείων. Μια **ContentKey** παρέχει ασφαλή πρόσβαση σε σας s **περιουσιακών στοιχείων**. 

Όταν δημιουργείτε ένα νέο πάγιο (για παράδειγμα, πριν από την [Αποστολή αρχείων](media-services-dotnet-upload-files.md)), μπορείτε να καθορίσετε τις ακόλουθες επιλογές κρυπτογράφησης: **StorageEncrypted**, **CommonEncryptionProtected**ή **EnvelopeEncryptionProtected**. 

Όταν παραδώσετε περιουσιακών στοιχείων σε υπολογιστές-πελάτες σας, μπορείτε να [ρυθμίσετε τις παραμέτρους για στοιχεία σε δυναμικό είναι κρυπτογραφημένα](media-services-dotnet-configure-asset-delivery-policy.md) με ένα από τα παρακάτω δύο κρυπτογραφήσεις: **DynamicEnvelopeEncryption** ή **DynamicCommonEncryption**.

Κρυπτογραφημένο περιουσιακά στοιχεία πρέπει να συσχετίζονται με **ContentKey**s. Σε αυτό το άρθρο περιγράφει τον τρόπο για να δημιουργήσετε έναν αριθμό-κλειδί περιεχομένου.

>[AZURE.NOTE] Όταν δημιουργείτε ένα νέο πάγιο **StorageEncrypted** χρησιμοποιώντας το Media Services .NET SDK, το **ContentKey** δημιουργείται αυτόματα και που συνδέονται με το περιουσιακών στοιχείων.

##<a name="contentkeytype"></a>ContentKeyType

Μία από τις τιμές που πρέπει να ορίσετε όταν δημιουργείτε ένα περιεχόμενο αριθμού-κλειδιού είναι ο τύπος περιεχομένου κλειδιού. Επιλέξτε μία από τις παρακάτω τιμές. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

##<a id="envelope_contentkey"></a>Δημιουργία φακέλου τύπου ContentKey

Το παρακάτω τμήμα κώδικα δημιουργεί έναν αριθμό-κλειδί περιεχομένου από τον τύπο κρυπτογράφησης του φακέλου. Συσχετίζει, στη συνέχεια, το πλήκτρο με το καθορισμένο περιουσιακών στοιχείων.

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

κλήση

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>Δημιουργία κοινός τύπος ContentKey    

Το παρακάτω τμήμα κώδικα δημιουργεί ένα κλειδί περιεχομένου του κοινές τύπου κρυπτογράφησης. Συσχετίζει, στη συνέχεια, το πλήκτρο με το καθορισμένο περιουσιακών στοιχείων.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
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
κλήση

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
