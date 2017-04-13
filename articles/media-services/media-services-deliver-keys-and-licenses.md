<properties 
    pageTitle="Χρήση των υπηρεσιών Azure Media Services για παράδοση DRM άδειες χρήσης ή AES πλήκτρα" 
    description="Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε Azure Media Services (αθροιστικής μέτρησης ενισχύσεων) για την παράδοση PlayReady ή/και Widevine αδειών χρήσης και πλήκτρα AES, αλλά χωρίς τα υπόλοιπα (κωδικοποίηση, η κρυπτογράφηση, η ροή) με τους διακομιστές εσωτερικής εγκατάστασης." 
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


#<a name="use-azure-media-services-to-deliver-drm-licenses-or-aes-keys"></a>Χρήση των υπηρεσιών Azure Media Services για παράδοση DRM άδειες χρήσης ή AES πλήκτρα

Azure Media Services (αθροιστικής μέτρησης ενισχύσεων) σάς επιτρέπει να ingest, κωδικοποίηση, Προσθήκη προστασίας του περιεχομένου και ροή περιεχομένου σας (ανατρέξτε [σε αυτό](media-services-protect-with-drm.md) το άρθρο για λεπτομέρειες). Ωστόσο, υπάρχουν οι πελάτες που θέλετε μόνο να χρησιμοποιήσετε αθροιστικής μέτρησης ενισχύσεων για την παράδοση άδειες χρήσης ή/και αριθμούς-κλειδιά και κάντε κωδικοποίηση, η κρυπτογράφηση και η ροή χρησιμοποιώντας τους διακομιστές εσωτερικής εγκατάστασης. Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε αθροιστικής μέτρησης ενισχύσεων για παράδοση PlayReady ή/και Widevine άδειες χρήσης, αλλά να κάνετε τα υπόλοιπα με τους διακομιστές εσωτερικής εγκατάστασης. 


## <a name="overview"></a>Επισκόπηση

Υπηρεσίες πολυμέσων παρέχει μια υπηρεσίες για την παροχή PlayReady και Widevine DRM αδειών χρήσης και πλήκτρα AES 128. Υπηρεσίες πολυμέσων παρέχει επίσης API που σας επιτρέπουν να διαμορφώσετε τα δικαιώματα και τους περιορισμούς που θέλετε για το περιβάλλον εκτέλεσης DRM για να επιβάλετε όταν ένας χρήστης αναπαράγει το DRM προστασία περιεχομένου. Όταν ένας χρήστης ζητήσει στο προστατευμένο περιεχόμενο, η εφαρμογή του προγράμματος αναπαραγωγής θα ζητήσετε μια άδεια χρήσης από την υπηρεσία αθροιστικής μέτρησης ενισχύσεων άδεια χρήσης. Η υπηρεσία άδειας χρήσης αθροιστικής μέτρησης ενισχύσεων θα θεμάτων την άδεια χρήσης για το πρόγραμμα αναπαραγωγής, (εάν επιτρέπεται). Τις άδειες χρήσης PlayReady και Widevine περιέχουν το κλειδί αποκρυπτογράφησης που μπορούν να χρησιμοποιηθούν από το πρόγραμμα αναπαραγωγής του προγράμματος-πελάτη για να αποκρυπτογραφήσετε και ροή του περιεχομένου.

Υπηρεσίες πολυμέσων υποστηρίζει πολλούς τρόπους εξουσιοδότησης χρήστες που κάνετε άδειας χρήσης ή προσκλήσεις κλειδιού. Ρυθμίζετε τις παραμέτρους του περιεχομένου κλειδιού πολιτικής εξουσιοδότησης και την πολιτική θα μπορούσε να έχει ένα ή περισσότερα περιορισμούς: άνοιγμα ή διακριτικό περιορισμού. Η πολιτική διακριτικού περιορισμένα πρέπει να συνοδεύεται από ένα διακριτικό εκδοθεί από μια ασφαλούς διακριτικού υπηρεσίας (STS). Υπηρεσίες πολυμέσων υποστηρίζει τα διακριτικά σε το απλό διακριτικά Web (SWT) και της μορφής διακριτικού JSON Web (JWT).


Το παρακάτω διάγραμμα παρουσιάζει τα κύρια βήματα που πρέπει να κάνετε για να χρησιμοποιήσετε αθροιστικής μέτρησης ενισχύσεων να κάνουν PlayReady ή/και Widevine άδειες χρήσης, αλλά κάντε το υπόλοιπο με τους διακομιστές εσωτερικής εγκατάστασης.

![Προστασία με PlayReady](./media/media-services-deliver-keys-and-licenses/media-services-diagram1.png)

##<a name="download-sample"></a>Λήψη δείγματος

Μπορείτε να κάνετε λήψη του δείγματος που περιγράφονται σε αυτό το άρθρο από [εδώ](https://github.com/Azure/media-services-dotnet-deliver-drm-licenses).

##<a name="net-code-example"></a>Παράδειγμα κώδικα .NET

Το παράδειγμα κώδικα σε αυτό το θέμα δείχνει πώς μπορείτε να δημιουργήσετε ένα κλειδί κοινές περιεχομένου και να PlayReady ή Widevine διευθύνσεις URL acquisition άδεια χρήσης. Πρέπει να λάβετε τα παρακάτω τμήματα των πληροφοριών από αθροιστικής μέτρησης ενισχύσεων και ρύθμιση παραμέτρων του διακομιστή εσωτερικής εγκατάστασης: **περιεχομένου κλειδί**, **αναγνωριστικό κλειδιού**, **διεύθυνση URL acquisition άδεια χρήσης**. Αφού ρυθμίσετε το διακομιστή εσωτερικής εγκατάστασης, θα μπορούσε να κατευθύνετε από τη δική σας ροή διακομιστή. Επειδή το κρυπτογραφημένο ροή σημεία αθροιστικής μέτρησης ενισχύσεων άδειας χρήσης διακομιστή, το πρόγραμμα αναπαραγωγής θα ζητήσει μια άδεια χρήσης από αθροιστικής μέτρησης ενισχύσεων. Εάν επιλέξετε διακριτικού ελέγχου ταυτότητας, ο διακομιστής αδειών χρήσης αθροιστικής μέτρησης ενισχύσεων θα επικυρώσει το διακριτικό που αποστέλλονται μέσω HTTPS και (εάν είναι έγκυρη) θα κάνετε την άδεια χρήσης ξανά στη συσκευή αναπαραγωγής. (Το παράδειγμα κώδικα μόνο δείχνει πώς μπορείτε να δημιουργήσετε ένα κλειδί κοινές περιεχομένου και να PlayReady ή Widevine διευθύνσεις URL acquisition άδεια χρήσης. Εάν θέλετε να πλήκτρα παράδοσης AES 128, πρέπει να δημιουργήσετε ένα κλειδί περιεχομένου φακέλου και να λάβετε μια διεύθυνση URL κλειδιού acquisition και [αυτό](media-services-protect-with-aes128.md) το άρθρο περιγράφει πώς μπορείτε να το κάνετε).
    
    
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
    
    
    namespace DeliverDRMLicenses
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
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                bool tokenRestriction = true;
                string tokenTemplateString = null;
    
    
                IContentKey key = CreateCommonTypeContentKey();
    
                // Print out the key ID and Key in base64 string format
                Console.WriteLine("Created key {0} with key value {1} ", 
                    key.Id, System.Convert.ToBase64String(key.GetClearKeyValue()));
    
                Console.WriteLine("PlayReady License Key delivery URL: {0}", 
                    key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
    
                Console.WriteLine("Widevine License Key delivery URL: {0}",
                    key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine));
    
                if (tokenRestriction)
                    tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                else
                    AddOpenAuthorizationPolicy(key);
    
                Console.WriteLine("Added authorization policy: {0}", 
                    key.AuthorizationPolicyId);
                Console.WriteLine();
                Console.ReadLine();
            }
    
            static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
            {
    
                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          
    
                List<ContentKeyAuthorizationPolicyRestriction> restrictions = 
                    new List<ContentKeyAuthorizationPolicyRestriction>
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
    
                List<ContentKeyAuthorizationPolicyRestriction> restrictions = 
                    new List<ContentKeyAuthorizationPolicyRestriction>
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
    
                //The PlayReadyLicenseResponseTemplate class represents the template 
                //for the response sent back to the end user. 
                //It contains a field for a custom data string between the license server 
                //and the application (may be useful for custom app logic) 
                //as well as a list of one or more license templates.
    
                PlayReadyLicenseResponseTemplate responseTemplate = 
                    new PlayReadyLicenseResponseTemplate();
    
                // The PlayReadyLicenseTemplate class represents a license template 
                // for creating PlayReady licenses
                // to be returned to the end users. 
                // It contains the data on the content key in the license 
                // and any rights or restrictions to be 
                // enforced by the PlayReady DRM runtime when using the content key.
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
    
                // Configure whether the license is persistent 
                // (saved in persistent storage on the client) 
                // or non-persistent (only held in memory while the player is using the license).  
                licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
    
                // AllowTestDevices controls whether test devices can use the license or not.  
                // If true, the MinimumSecurityLevel property of the license
                // is set to 150.  If false (the default), 
                // the MinimumSecurityLevel property of the license is set to 2000.
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
    
                // IMPORTANT: These types of restrictions can be very powerful 
                // but can also affect the consumer experience. 
                // If the output protections are configured too restrictive, 
                // the content might be unplayable on some clients. 
                // For more information, see the PlayReady Compliance Rules document.
    
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
                            required_output_protection = 
                                new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
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
    
    
            static public IContentKey CreateCommonTypeContentKey()
            {
                // Create envelope encryption content key
                Guid keyId = Guid.NewGuid();
                byte[] contentKey = GetRandomBuffer(16);
    
                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);
    
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
    
    
        }
    }
    

##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Δείτε επίσης

[Χρήση PlayReady ή/και Widevine δυναμικής κοινές κρυπτογράφησης](media-services-protect-with-drm.md)

[Χρήση δυναμική κρυπτογράφηση AES 128 και η υπηρεσία παράδοσης κλειδιού](media-services-protect-with-aes128.md)

[Χρήση συνεργάτες για την παράδοση Widevine άδειες χρήσης σε υπηρεσιών Azure Media Services](media-services-licenses-partner-integration.md)