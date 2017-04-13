<properties 
    pageTitle="Ρύθμιση παραμέτρων της πολιτικής περιεχομένου εξουσιοδότησης κλειδιού χρησιμοποιώντας Media Services .NET SDK | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να ρυθμίσετε μια πολιτική εξουσιοδότησης για ένα κλειδί περιεχομένου χρησιμοποιώντας SDK .NET υπηρεσίες πολυμέσων." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;mingfeiy"/>



# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Δυναμική κρυπτογράφηση: ρύθμιση παραμέτρων της πολιτικής εξουσιοδότησης κλειδιού περιεχομένου

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

##<a name="overview"></a>Επισκόπηση

Υπηρεσίες πολυμέσων Azure Microsoft σάς επιτρέπει να κάνουν MPEG-ΔΙΑΚΕΚΟΜΜΈΝΗ ομαλή ροή και HTTP Live ροής (HLS) ροές που προστατεύονται με Advanced πρότυπο κρυπτογράφησης (AES) (με χρήση κλειδιών κρυπτογράφησης 128 bit) ή [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). Αθροιστικής μέτρησης ενισχύσεων σάς επιτρέπει επίσης να παράδοση κρυπτογραφημένα με Widevine DRM ροών ΠΑΎΛΑΣ. PlayReady και Widevine είναι κρυπτογραφημένα ανά της προδιαγραφής κοινές κρυπτογράφησης (ISO/IEC CENC 23001-7).

Υπηρεσίες πολυμέσων παρέχει επίσης μια **Υπηρεσία παράδοσης κλειδί/άδειας χρήσης** από το οποίο οι υπολογιστές-πελάτες να προμηθευτείτε AES πλήκτρα ή PlayReady/Widevine άδειες χρήσης για να αναπαραγάγετε το κρυπτογραφημένο περιεχόμενο.

Εάν θέλετε για τις υπηρεσίες πολυμέσων για την κρυπτογράφηση ενός περιουσιακού στοιχείου, πρέπει να συσχετίσετε ενός κλειδιού κρυπτογράφησης (**CommonEncryption** ή **EnvelopeEncryption**) με το περιουσιακών στοιχείων (όπως περιγράφεται [εδώ](media-services-dotnet-create-contentkey.md)) και επίσης ρύθμιση παραμέτρων πολιτικών εξουσιοδότησης για τον αριθμό-κλειδί (όπως περιγράφεται σε αυτό το άρθρο).

Όταν μια ροή ζητείται από ένα πρόγραμμα αναπαραγωγής, Media Services χρησιμοποιεί τον καθορισμένο αριθμό-κλειδί για την κρυπτογράφηση δυναμικά το περιεχόμενό σας χρησιμοποιεί κρυπτογράφηση AES ή DRM. Για να αποκρυπτογραφήσετε τη ροή, το πρόγραμμα αναπαραγωγής θα ζητήσει τον αριθμό-κλειδί από την υπηρεσία παράδοσης κλειδιού. Για να αποφασίσετε αν ο χρήστης έχει δικαίωμα για να λάβετε τον αριθμό-κλειδί, την υπηρεσία υπολογίζει τις πολιτικές εξουσιοδότησης που έχετε καθορίσει για τον αριθμό-κλειδί.

Υπηρεσίες πολυμέσων υποστηρίζει πολλές τρόποι για τον έλεγχο ταυτότητας τους χρήστες που κάνουν αιτήσεις κλειδιού. Η πολιτική περιεχομένου εξουσιοδότησης κλειδιού θα μπορούσε να έχει ένα ή περισσότερα περιορισμοί άδειας: **Άνοιγμα** ή **διακριτικού** περιορισμού. Η πολιτική διακριτικού περιορισμένα πρέπει να συνοδεύεται από ένα διακριτικό εκδοθεί από μια ασφαλούς διακριτικού υπηρεσίας (STS). Υπηρεσίες πολυμέσων υποστηρίζει τα διακριτικά σε το **Απλό διακριτικά Web** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) και της μορφής **JSON Web διακριτικού** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)).

Υπηρεσίες πολυμέσων δεν παρέχει ασφαλούς διακριτικού υπηρεσίες. Μπορείτε να δημιουργήσετε μια προσαρμοσμένη STS ή να αξιοποιήσετε Microsoft Azure ACS για να τα διακριτικά το ζήτημα. Το STS πρέπει να ρυθμιστεί για τη δημιουργία ενός διακριτικού συνδεδεμένοι με το συγκεκριμένο πρόβλημα και αριθμού-κλειδιού αξιώσεων που καθορίσατε στο στη ρύθμιση παραμέτρων διακριτικού περιορισμού (όπως περιγράφεται σε αυτό το άρθρο). Η υπηρεσία παράδοσης κλειδιού Media Services θα επιστρέψει του κλειδιού κρυπτογράφησης στον υπολογιστή-πελάτη, εάν ισχύει το διακριτικό και το αξιώσεων στο διακριτικό ταιριάζουν με αυτές που έχουν ρυθμιστεί για το κλειδί περιεχομένου.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα

[JWT authenitcation διακριτικού](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Ενοποίηση Azure Media Services OWIN MVC βάσει εφαρμογή με το Azure Active Directory και να περιορίσετε την παράδοση περιεχομένου κλειδιού βάσει αξιώσεων JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Χρήση Azure ACS για να τα διακριτικά το ζήτημα](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Εφαρμογή ορισμένα ζητήματα:

- Για να μπορέσετε να χρησιμοποιήσετε δυναμικές συσκευασία και δυναμική κρυπτογράφηση, πρέπει να βεβαιωθείτε ότι έχουν τουλάχιστον μία ροή δεσμευμένη μονάδα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος για να κλιμακωθεί υπηρεσίας πολυμέσων](media-services-portal-manage-streaming-endpoints.md).
- Σας περιουσιακών στοιχείων πρέπει να περιέχει ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4s ή αρχεία ομαλή ροή προσαρμόσιμη ρυθμό μετάδοσης bit. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κωδικοποίηση ενός περιουσιακού στοιχείου](media-services-encode-asset.md).
- Αποστολή και να κωδικοποιείτε τους πόρους σας χρησιμοποιώντας την επιλογή **AssetCreationOptions.StorageEncrypted** .
- Εάν σκοπεύετε να έχει πολλές πλήκτρα περιεχομένου που απαιτούν την ίδια ρύθμιση παραμέτρων της πολιτικής, συνιστάται να δημιουργήσετε μια πολιτική μία εξουσιοδότησης και εκ νέου χρήση με πολλά κλειδιά περιεχομένου.
- Η υπηρεσία παράδοσης κλειδί αποθηκεύει προσωρινά ContentKeyAuthorizationPolicy και τα σχετικά αντικείμενα (επιλογές της πολιτικής και τους περιορισμούς) για 15 λεπτά.  Εάν δημιουργείτε μια ContentKeyAuthorizationPolicy και καθορίστε τη χρήση ενός περιορισμού "Διακριτικού", στη συνέχεια, δοκιμάστε ξανά, και, στη συνέχεια, να ενημερώσετε την πολιτική με περιορισμό "Άνοιγμα", θα χρειαστεί περίπου 15 λεπτά πριν από την πολιτική μεταβαίνει στην έκδοση "Άνοιγμα" της πολιτικής.
- Εάν κάνετε προσθήκη ή ενημέρωση πολιτικής παράδοσης του περιουσιακού στοιχείου, πρέπει να διαγράψετε μια υπάρχουσα locator (εάν υπάρχουν) και να δημιουργήσετε ένα νέο προσδιοριστικό.
- Προς το παρόν, δεν μπορείτε να κρυπτογραφήσετε σκληροί ΔΊΣΚΟΙ μορφή ροής ή προοδευτική στοιχεία λήψης.


##<a name="aes-128-dynamic-encryption"></a>Δυναμική κρυπτογράφηση AES 128

###<a name="open-restriction"></a>Άνοιγμα περιορισμού

Άνοιγμα περιορισμού σημαίνει ότι το σύστημα θα κάνετε τον αριθμό-κλειδί σε οποιονδήποτε κάνει μια αίτηση κλειδιού. Αυτός ο περιορισμός μπορεί να είναι χρήσιμες για σκοπούς δοκιμής.

Το παρακάτω παράδειγμα δημιουργεί μια πολιτική Άνοιγμα εξουσιοδότησης και το προσθέτει στο κλειδί περιεχομένου.

στατική δημόσια void AddOpenAuthorizationPolicy (IContentKey contentKey) {/ / Δημιουργία ContentKeyAuthorizationPolicy με ανοιχτό περιορισμούς / / και Δημιουργία πολιτικής εξουσιοδότησης πολιτικής IContentKeyAuthorizationPolicy = _context.
ContentKeyAuthorizationPolicies.
CreateAsync ("Άνοιγμα εξουσιοδότησης πολιτική"). Αποτέλεσμα;

Λίστα<ContentKeyAuthorizationPolicyRestriction> περιορισμούς = νέα λίστα<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };
    
        restrictions.Add(restriction);
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


###<a name="token-restriction"></a>Ο περιορισμός διακριτικού

Αυτή η ενότητα περιγράφει πώς μπορείτε να δημιουργήσετε μια πολιτική εξουσιοδότησης κλειδιού περιεχομένου και να συσχετίσετε με τον αριθμό-κλειδί περιεχομένου. Η πολιτική εξουσιοδότησης περιγράφει τι απαιτήσεις άδειας πρέπει να πληρούνται για να προσδιορίσετε εάν ο χρήστης έχει δικαίωμα να λάβετε το κλειδί (για παράδειγμα, κάνει τη λίστα "Επαλήθευση κλειδί" περιέχουν τον αριθμό-κλειδί που υπογράφηκε με το διακριτικό).

Για να ρυθμίσετε την επιλογή διακριτικού περιορισμού, πρέπει να χρησιμοποιήσετε ένα XML για να περιγράψετε το διακριτικό εξουσιοδότησης απαιτήσεις. Στη ρύθμιση παραμέτρων διακριτικού περιορισμού XML πρέπει να συμφωνεί με το παρακάτω σχήμα XML.

####<a id="schema"></a>Σχήμα διακριτικού περιορισμού
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Κατά τη ρύθμιση των παραμέτρων του **διακριτικού** περιορισμένων πολιτική, πρέπει να καθορίσετε τις πρωτεύον** κλειδί επαλήθευσης**, **εκδότη** και **ακροατηρίου** παραμέτρους. Το **επαλήθευσης πρωτεύον κλειδί **περιέχει τον αριθμό-κλειδί που υπογράφηκε με το διακριτικό, **εκδότη** είναι η υπηρεσία ασφαλούς διακριτικών που εκδίδει το διακριτικό. Το **ακροατήριο** (μερικές φορές ονομάζεται **εύρος**) περιγράφει πρόθεση το διακριτικό ή ο πόρος το διακριτικό παράσχει εξουσιοδότηση πρόσβασης. Η υπηρεσία παράδοσης κλειδιού Media Services επαληθεύει ότι αυτές οι τιμές στο διακριτικό ταιριάζουν με τις τιμές στο πρότυπο. 

Όταν χρησιμοποιείτε το **Media Services SDK για .NET**, μπορείτε να χρησιμοποιήσετε την κλάση **TokenRestrictionTemplate** για να δημιουργήσετε το διακριτικό περιορισμού.
Το παρακάτω παράδειγμα δημιουργεί μια πολιτική εξουσιοδότησης με ενός διακριτικού περιορισμού. Σε αυτό το παράδειγμα, ο υπολογιστής-πελάτης θα πρέπει να παρουσιάσετε ένα διακριτικό που περιέχει: Είσοδος κλειδί (VerificationKey), μια εκδότη διακριτικού και απαιτείται αξιώσεων.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };
    
        restrictions.Add(restriction);
    
        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    
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

####<a id="test"></a>Έλεγχος διακριτικού

Για να λάβετε ένα διακριτικό δοκιμής με βάση τον περιορισμό διακριτικού που χρησιμοποιήθηκε για την πολιτική εξουσιοδότησης κλειδιού, κάντε τα εξής.
    
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
    
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##<a name="playready-dynamic-encryption"></a>Δυναμική PlayReady κρυπτογράφησης 

Υπηρεσίες πολυμέσων σάς επιτρέπει να ρυθμίσετε τα δικαιώματα και τους περιορισμούς που θέλετε για το περιβάλλον εκτέλεσης PlayReady DRM για να επιβάλετε όταν ένας χρήστης επιχειρεί να αναπαραγάγετε προστατευμένου περιεχομένου. 

Κατά την προστασία του περιεχομένου με PlayReady, ένα από τα πράγματα που πρέπει να καθορίσετε σε σας πολιτική εξουσιοδότησης είναι μια συμβολοσειρά XML που καθορίζει το [πρότυπο PlayReady άδεια χρήσης](media-services-playready-license-template-overview.md). Στο SDK υπηρεσίες πολυμέσων για .NET, οι κλάσεις **PlayReadyLicenseResponseTemplate** και **PlayReadyLicenseTemplate** θα σας βοηθήσει να ορίσετε το πρότυπο PlayReady άδεια χρήσης.

[Αυτό το θέμα](media-services-protect-with-drm.md) δείχνει πώς μπορείτε να κρυπτογραφήσετε το περιεχόμενό σας με **PlayReady** και **Widevine**.

###<a name="open-restriction"></a>Άνοιγμα περιορισμού
    
Άνοιγμα περιορισμού σημαίνει ότι το σύστημα θα κάνετε τον αριθμό-κλειδί σε οποιονδήποτε κάνει μια αίτηση κλειδιού. Αυτός ο περιορισμός μπορεί να είναι χρήσιμες για σκοπούς δοκιμής.

Το παρακάτω παράδειγμα δημιουργεί μια πολιτική Άνοιγμα εξουσιοδότησης και το προσθέτει στο κλειδί περιεχομένου.

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
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
    
    
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

###<a name="token-restriction"></a>Ο περιορισμός διακριτικού

Για να ρυθμίσετε την επιλογή διακριτικού περιορισμού, πρέπει να χρησιμοποιήσετε ένα XML για να περιγράψετε το διακριτικό εξουσιοδότησης απαιτήσεις. Στη ρύθμιση παραμέτρων διακριτικού περιορισμού XML πρέπει να συμφωνεί με το σχήμα XML που εμφανίζονται σε [αυτήν](#schema) την ενότητα.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
                
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
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


Για να λάβετε ένα διακριτικό δοκιμής με βάση τον περιορισμό διακριτικού που χρησιμοποιήθηκε για την έγκριση κλειδιού πολιτικής δείτε [αυτήν](#test) την ενότητα. 

##<a id="types"></a>Τύποι που χρησιμοποιούνται κατά τον καθορισμό ContentKeyAuthorizationPolicy

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

###<a id="TokenType"></a>TokenType

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Επόμενο βήμα
Τώρα που έχετε ρυθμίσει τις παραμέτρους πολιτικής εξουσιοδότησης περιεχομένου κλειδί, μεταβείτε στο θέμα [πώς μπορείτε να ρυθμίσετε τις παραμέτρους πολιτικής παράδοσης περιουσιακών στοιχείων](media-services-dotnet-configure-asset-delivery-policy.md) .
 
