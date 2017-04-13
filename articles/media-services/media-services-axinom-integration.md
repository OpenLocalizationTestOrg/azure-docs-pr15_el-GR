<properties 
    pageTitle="Χρήση Axinom για την παράδοση Widevine άδειες χρήσης σε υπηρεσιών Azure Media Services | Microsoft Azure" 
    description="Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε Azure Media Services (αθροιστικής μέτρησης ενισχύσεων) για να παρέχουν μια ροή που είναι κρυπτογραφημένα δυναμικά από αθροιστικής μέτρησης ενισχύσεων με PlayReady και Widevine DRMs. Η άδεια χρήσης PlayReady προέρχεται από διακομιστή αδειών χρήσης PlayReady υπηρεσίες πολυμέσων και παραδίδεται Widevine άδειας χρήσης από Axinom άδεια χρήσης του διακομιστή." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"   
    ms.author="willzhan;Mingfeiy;rajputam;Juliako"/>

#<a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Χρήση Axinom για την παράδοση Widevine άδειες χρήσης σε υπηρεσιών Azure Media Services  

> [AZURE.SELECTOR]
- [castLabs](media-services-castlabs-integration.md)
- [Axinom](media-services-axinom-integration.md)

##<a name="overview"></a>Επισκόπηση

Azure Media Services (αθροιστικής μέτρησης ενισχύσεων) έχει προσθέσει Google Widevine δυναμικής προστασίας (ανατρέξτε στο θέμα [ιστολόγιο του Mingfei](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) για λεπτομέρειες). Επιπλέον, πρόγραμμα αναπαραγωγής πολυμέσων Azure (AMP) έχει προσθέσει επίσης υποστήριξη Widevine (ανατρέξτε στο θέμα [AMP εγγράφου](http://amp.azure.net/libs/amp/latest/docs/) για λεπτομέρειες). Αυτή είναι μια κύρια εκπλήρωση στη ροή ΠΑΎΛΑΣ περιεχόμενο που προστατεύεται από CENC με πολλών-native-DRM (PlayReady και Widevine) σε σύγχρονα προγράμματα περιήγησης που διαθέτει MSE και EME.

Ξεκινώντας από το Media Services .NET SDK έκδοση 3.5.2, Media Services σάς επιτρέπει να ρυθμίσετε τις παραμέτρους Widevine άδεια χρήσης του προτύπου και λάβετε Widevine άδειες χρήσης. Μπορείτε επίσης να χρησιμοποιήσετε την παρακάτω συνεργάτες αθροιστικής μέτρησης ενισχύσεων θα σας βοηθήσουν να κάνουν Widevine άδειες χρήσης: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να ενοποιήσετε και να ελέγξετε Widevine διακομιστή αδειών χρήσης που ελέγχονται από Axinom. Συγκεκριμένα, καλύπτει:  

- Ρύθμιση παραμέτρων δυναμική κοινές κρυπτογράφηση με πολλούς-DRM (PlayReady και Widevine) με αντίστοιχες διευθύνσεις URL acquisition άδεια χρήσης;
- Δημιουργία ενός διακριτικού JWT προκειμένου να πληροί τις απαιτήσεις διακομιστή άδεια χρήσης;
- Ανάπτυξη app Azure Media Player το οποίο χειρίζεται απόκτηση άδειας χρήσης με έλεγχο ταυτότητας διακριτικού JWT;

Το σύνολο του συστήματος και τη ροή περιεχομένου βασικά, κλειδιού Αναγνωριστικό, βασικών σπόρων, διακριτικό JTW και τις απαιτήσεις μπορεί να είναι καλύτερη που περιγράφεται από το παρακάτω διάγραμμα.

![ΠΑΎΛΑ και CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

##<a name="content-protection"></a>Προστασία περιεχομένου

Για τη ρύθμιση των παραμέτρων δυναμικής προστασίας και πολιτικής κλειδιού παράδοσης, ανατρέξτε στο θέμα ιστολόγιο του Mingfei: [πώς μπορείτε να ρυθμίσετε τις παραμέτρους Widevine συσκευασία με υπηρεσιών Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Μπορείτε να ρυθμίσετε δυναμικής προστασίας CENC με πολλούς DRM για ΠΑΎΛΑ ροής χρειάζεται και τα δύο από τα εξής:

1. Προστασία PlayReady για το MS άκρου και IE11, που θα μπορούσε να έχει ένα περιορισμούς διακριτικού εξουσιοδότησης. Η πολιτική διακριτικού περιορισμένα πρέπει να συνοδεύεται από ένα διακριτικό εκδοθεί από μια ασφαλούς διακριτικού υπηρεσίας (STS), όπως το Azure Active Directory;
1. Widevine προστασίας για το Chrome, αυτό μπορεί να απαιτείται έλεγχος ταυτότητας διακριτικού με διακριτικό εκδοθεί από μια άλλη STS. 

Ανατρέξτε στο θέμα [Δημιουργία διακριτικού JWT](media-services-axinom-integration.md#jwt-token-generation) στην ενότητα για γιατί Azure Active Directory δεν μπορεί να χρησιμοποιηθεί ως ένα STS για διακομιστή αδειών χρήσης του Axinom Widevine.

###<a name="considerations"></a>Ζητήματα

1. Πρέπει να χρησιμοποιήσετε το Axinom που καθορίζεται βασικών σπόρων (8888000000000000000000000000000000000000) και σας που έχει δημιουργηθεί ή επιλεγμένο Αναγνωριστικό για να δημιουργήσετε το κλειδί περιεχομένου για τη ρύθμιση υπηρεσία παράδοσης κλειδιού κλειδιού. Διακομιστής αδειών χρήσης Axinom θα θεμάτων όλες τις άδειες χρήσης που περιέχει περιεχομένου κλειδιά που βασίζεται στο ίδιο κλειδιού σπόρο, που ισχύει για τον έλεγχο και παραγωγής.
1. Τη διεύθυνση URL απόκτηση της άδειας χρήσης Widevine για σκοπούς δοκιμής: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). Επιτρέπονται HTTP και HTTS.

##<a name="azure-media-player-preparation"></a>Προετοιμασία προγράμματος αναπαραγωγής πολυμέσων Azure

AMP v1.4.0 υποστηρίζει αναπαραγωγή περιεχομένου αθροιστικής μέτρησης ενισχύσεων που περιέχεται δυναμικά με PlayReady και Widevine DRM.
Εάν Widevine άδεια χρήσης server δεν απαιτεί έλεγχο ταυτότητας διακριτικού, δεν υπάρχει τίποτα επιπλέον πρέπει να κάνετε για να δοκιμάσετε μια ΠΑΎΛΑ περιεχόμενο που προστατεύεται από Widevine. Για παράδειγμα, στην ομάδα AMP παρέχει ένα απλό [δείγμα](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), όπου μπορείτε να δείτε την εργασία στο άκρο και IE11 με PlayReady και το Chrome με Widevine.
Ο διακομιστής αδειών χρήσης Widevine που παρέχεται από Axinom απαιτεί έλεγχο ταυτότητας διακριτικού JWT. Το διακριτικό JWT πρέπει να υποβάλλονται με άδεια χρήσης σε σύσκεψη σε μια κεφαλίδα HTTP "X-AxDRM-μήνυμα". Για το σκοπό αυτό, πρέπει να προσθέσετε το παρακάτω κώδικα javascript σε μια ιστοσελίδα φιλοξενίας AMP πριν από τη ρύθμιση της προέλευσης:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

Το υπόλοιπο του κώδικα AMP είναι τυπική API AMP όπως στο έγγραφο AMP [εδώ](http://amp.azure.net/libs/amp/latest/docs/).

Σημειώστε ότι τα παραπάνω javascript για τη ρύθμιση προσαρμοσμένης εξουσιοδότησης κεφαλίδα εξακολουθεί να είναι μια προσέγγιση σύντομο χρονικό διάστημα πριν ο υπάλληλος κυκλοφορήσει μακροπρόθεσμη προσέγγιση στο AMP.

##<a name="jwt-token-generation"></a>JWT διακριτικού γενιάς

Axinom Widevine διακομιστή αδειών χρήσης για σκοπούς δοκιμής απαιτεί έλεγχο ταυτότητας διακριτικού JWT. Επιπλέον, μία από τις απαιτήσεις στο διακριτικό JWT είναι ένα τύπο αντικειμένου σύνθετες αντί για τον τύπο δεδομένων στοιχειώδεις.

Δυστυχώς, Azure AD μόνο να εκδώσετε διακριτικά JWT με στοιχειώδεις τύπους. Ομοίως, .NET Framework API (System.IdentityModel.Tokens.SecurityTokenHandler και JwtPayload) μόνο σάς επιτρέπει να εισαγωγής τύπος αντικειμένου σύνθετες ως αξιώσεων. Ωστόσο, το αξιώσεων εξακολουθεί να σειριοποιηθεί ως συμβολοσειρά. Επομένως, δεν είναι δυνατό να χρησιμοποιήσουμε οποιαδήποτε από τις δύο για τη δημιουργία το διακριτικό JWT για την αίτηση Widevine άδεια χρήσης.


Ιωάννης Sheehan του [πακέτου JWT Nuget](https://www.nuget.org/packages/JWT) ανταποκρίνεται στις ανάγκες, ώστε να πρόκειται να χρησιμοποιήσετε αυτό το πακέτο Nuget.

Τα παρακάτω είναι ο κωδικός για δημιουργία διακριτικό JWT με την απαιτούμενη αξιώσεων όπως απαιτείται από το διακομιστή αδειών χρήσης Axinom Widevine για σκοπούς δοκιμής:

    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;
    
    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.
    
                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };
    
                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);
    
                return token;
            }
    
            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }
    
                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }
    
                return HexAsBytes;
            }
    
        }  
    
    }  

Axinom Widevine διακομιστή αδειών χρήσης

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

###<a name="considerations"></a>Ζητήματα

1.  Παρόλο που απαιτεί η υπηρεσία παράδοσης PlayReady αθροιστικής μέτρησης ενισχύσεων άδεια χρήσης "φορέα =" πριν από ένα διακριτικό ελέγχου ταυτότητας, Axinom Widevine άδεια χρήσης του διακομιστή δεν χρησιμοποιεί το.
2.  Το κλειδί επικοινωνίας Axinom χρησιμοποιείται ως κλειδί υπογραφής. Σημειώστε ότι το κλειδί είναι ένα δεκαεξαδική συμβολοσειρά, ωστόσο πρέπει να αντιμετωπιστεί ως μια σειρά byte δεν μια συμβολοσειρά όταν κωδικοποίηση. Αυτό είναι δυνατό με τη μέθοδο ConvertHexStringToByteArray.

##<a name="retrieving-key-id"></a>Ανάκτηση Αναγνωριστικό κλειδιού

Ίσως έχετε παρατηρήσει ότι στον κώδικα για τη δημιουργία ενός JWT διακριτικού, κλειδιού ID απαιτείται. Επειδή το κλειδί του JWT διακριτικού πρέπει να είναι έτοιμοι πριν από τη φόρτωση AMP player, Αναγνωριστικό πρέπει να ανακτηθούν προκειμένου να δημιουργήσετε το διακριτικό JWT.

Υπάρχουν πολλοί τρόποι για να λάβετε κρατήστε πατημένο το πλήκτρο του αριθμού-κλειδιού κύκλου μαθημάτων ID σας. Για παράδειγμα, μπορεί να αποθηκεύσετε ένα Αναγνωριστικό κλειδιού μαζί με το περιεχόμενο μετα-δεδομένων σε μια βάση δεδομένων. Ή μπορείτε να ανακτήσετε Αναγνωριστικό από αρχείο MPD ΠΑΎΛΑ (πολυμέσων παρουσίασης περιγραφή) κλειδιού. Ο παρακάτω κώδικας είναι ο τελευταίος.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }
    
        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();
    
        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);
    
        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }
    
        return key_id;
    }

##<a name="summary"></a>Σύνοψη

Με πιο πρόσφατη Προσθήκη Widevine υποστήριξης στο Azure Media Services προστασίας του περιεχομένου και Azure Media Player, είναι δυνατή η για την υλοποίηση της ροής της ΠΑΎΛΑΣ + πολλών-native-DRM (PlayReady + Widevine) με την υπηρεσία και τα δύο PlayReady άδειας χρήσης στο αθροιστικής μέτρησης ενισχύσεων και Widevine διακομιστή αδειών χρήσης από Axinom για τα ακόλουθα σύγχρονα προγράμματα περιήγησης:

- Chrome
- Microsoft Edge στα Windows 10
- IE 11 Windows 8.1 και Windows 10
- Firefox (υπολογιστής) και Safari στο Mac (δεν iOS) υποστηρίζονται επίσης μέσω Silverlight και την ίδια διεύθυνση URL με το πρόγραμμα αναπαραγωγής πολυμέσων Azure

Τις ακόλουθες παραμέτρους απαιτούνται στο διακομιστή αδειών χρήσης χρησιμοποιώντας Axinom Widevine μίνι-λύση. Με εξαίρεση το κλειδί ID σας, το υπόλοιπο των παραμέτρων παρέχονται από Axinom με βάση τους εγκατάσταση server Widevine.


Παράμετρος|Πώς χρησιμοποιείται
---|---
Αναγνωριστικό κλειδιού επικοινωνίας|Πρέπει να συμπεριλαμβάνονται ως τιμή του τη διεκδίκηση "com_key_id" στο διακριτικό JWT (ανατρέξτε στο θέμα [αυτήν](media-services-axinom-integration.md#jwt-token-generation) την ενότητα).
Πλήκτρο επικοινωνίας|Πρέπει να χρησιμοποιείται ως το κλειδί υπογραφής διακριτικού JWT (ανατρέξτε στο θέμα [αυτήν](media-services-axinom-integration.md#jwt-token-generation) την ενότητα).
Πλήκτρο σπόρων|Πρέπει να χρησιμοποιούνται για τη δημιουργία περιεχομένου κλειδιού με οποιοδήποτε περιεχόμενο που δίνεται Αναγνωριστικό κλειδιού (ανατρέξτε στο θέμα [αυτήν](media-services-axinom-integration.md#content-protection) την ενότητα).
Διεύθυνση URL acquisition Widevine άδειας χρήσης|Πρέπει να χρησιμοποιείται με τη ρύθμιση παραμέτρων πολιτικής παράδοσης περιουσιακού στοιχείου για ΠΑΎΛΑ ροής (ανατρέξτε στο θέμα [αυτήν](media-services-axinom-integration.md#content-protection) την ενότητα).
Αναγνωριστικό κλειδιού περιεχομένου|Πρέπει να περιλαμβάνονται ως μέρος της τιμής του δικαιώματος μήνυμα διεκδίκηση διακριτικού JWT (ανατρέξτε στο θέμα [αυτήν](media-services-axinom-integration.md#jwt-token-generation) την ενότητα). 


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Επιβεβαίωση 

Προτείνεται να αποδεχτείτε τα ακόλουθα άτομα που συνεισφέρει για τη δημιουργία αυτού του εγγράφου: Kristjan Jõgi της Axinom, Mingfei Yan και Amit Rajput.
