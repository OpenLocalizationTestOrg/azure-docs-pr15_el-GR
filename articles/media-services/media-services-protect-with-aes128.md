<properties
    pageTitle="Χρήση δυναμική κρυπτογράφηση AES 128 και η υπηρεσία παράδοσης κλειδιού | Microsoft Azure"
    description="Microsoft Azure Media Services σάς επιτρέπει να κάνετε το περιεχόμενό σας κρυπτογραφημένα με πλήκτρα κρυπτογράφηση AES 128 bit. Υπηρεσίες πολυμέσων παρέχει επίσης την υπηρεσία παράδοσης αριθμό-κλειδί που προσφέρει κλειδιών κρυπτογράφησης σε εξουσιοδοτημένους χρήστες. Αυτό το θέμα δείχνει πώς μπορείτε να δυναμικά κρυπτογράφηση με AES 128 και να χρησιμοποιήσετε την υπηρεσία παράδοσης κλειδιού."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>

#<a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Χρήση δυναμική κρυπτογράφηση AES 128 και η υπηρεσία παράδοσης κλειδιού

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-aes128.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Επισκόπηση

>[AZURE.NOTE] Δείτε [αυτό](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) το βίντεο για μια επισκόπηση του τρόπου προστασίας του περιεχομένου πολυμέσων με κρυπτογράφηση AES.

Microsoft Azure Media Services σάς επιτρέπει να κάνετε Http Live-ροής (HLS) και ομαλή ροών κρυπτογραφημένα με Advanced πρότυπο κρυπτογράφησης (AES) (με χρήση κλειδιών κρυπτογράφησης 128 bit). Υπηρεσίες πολυμέσων παρέχει επίσης την υπηρεσία παράδοσης αριθμό-κλειδί που προσφέρει κλειδιών κρυπτογράφησης σε εξουσιοδοτημένους χρήστες. Εάν θέλετε για τις υπηρεσίες πολυμέσων για την κρυπτογράφηση ενός περιουσιακού στοιχείου, πρέπει να συσχετίσετε ενός κλειδιού κρυπτογράφησης με παγίου και επίσης ρύθμιση παραμέτρων πολιτικών εξουσιοδότησης για τον αριθμό-κλειδί. Όταν μια ροή ζητείται από ένα πρόγραμμα αναπαραγωγής, Media Services χρησιμοποιεί τον καθορισμένο αριθμό-κλειδί για την κρυπτογράφηση δυναμικά το περιεχόμενό σας χρησιμοποιεί κρυπτογράφηση AES. Για να αποκρυπτογραφήσετε τη ροή, το πρόγραμμα αναπαραγωγής θα ζητήσει τον αριθμό-κλειδί από την υπηρεσία παράδοσης κλειδιού. Για να αποφασίσετε αν ο χρήστης έχει δικαίωμα για να λάβετε τον αριθμό-κλειδί, την υπηρεσία υπολογίζει τις πολιτικές εξουσιοδότησης που έχετε καθορίσει για τον αριθμό-κλειδί.

Υπηρεσίες πολυμέσων υποστηρίζει πολλές τρόποι για τον έλεγχο ταυτότητας τους χρήστες που κάνουν αιτήσεις κλειδιού. Η πολιτική περιεχομένου εξουσιοδότησης κλειδιού θα μπορούσε να έχει ένα ή περισσότερα περιορισμοί άδειας: άνοιγμα ή διακριτικό περιορισμού. Η πολιτική διακριτικού περιορισμένα πρέπει να συνοδεύεται από ένα διακριτικό εκδοθεί από μια ασφαλούς διακριτικού υπηρεσίας (STS). Υπηρεσίες πολυμέσων υποστηρίζει τα διακριτικά σε το [Απλό διακριτικά Web](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) και της μορφής [JSON Web διακριτικού](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων πολιτική εξουσιοδότησης του αριθμού-κλειδιού του περιεχομένου](media-services-protect-with-aes128.md#configure_key_auth_policy).

Για να επωφεληθείτε από δυναμική κρυπτογράφηση, πρέπει να έχετε ενός περιουσιακού στοιχείου που περιέχει ένα σύνολο πολλούς-ρυθμό μετάδοσης bit MP4 αρχεία ή αρχεία προέλευσης ομαλή ροή πολλούς-ρυθμό μετάδοσης bit. Πρέπει επίσης να ρυθμίσετε τις παραμέτρους της πολιτικής παράδοσης του παγίου (που περιγράφεται παρακάτω σε αυτό το θέμα). Στη συνέχεια, με βάση τη μορφή που καθορίζεται στη ροή διεύθυνση URL, ο διακομιστής On ζήτηση ροής εξασφαλίζει ότι η ροή παραδίδεται στο πρωτόκολλο που έχετε επιλέξει. Ως αποτέλεσμα, θα πρέπει να αποθηκεύετε και να πληρώσετε για τα αρχεία σε μορφή μία χώρου αποθήκευσης και υπηρεσία Media Services θα δημιουργήσετε και θα λειτουργήσει την κατάλληλη απάντηση που βασίζεται σε αιτήσεις από ένα πρόγραμμα-πελάτη.

Αυτό το θέμα θα ήταν χρήσιμες στους προγραμματιστές που λειτουργούν σε εφαρμογές που κάνουν προστατευμένα πολυμέσα. Το θέμα δείχνει τον τρόπο ρύθμισης παραμέτρων της υπηρεσίας κλειδιού παράδοσης με τις πολιτικές εξουσιοδότησης, έτσι ώστε μόνο εξουσιοδοτημένοι υπολογιστές-πελάτες ενδέχεται να λάβετε τα κλειδιά κρυπτογράφησης. Εμφανίζει επίσης πώς μπορείτε να χρησιμοποιήσετε δυναμική κρυπτογράφηση.

>[AZURE.NOTE]Για να ξεκινήσετε τη χρήση δυναμική κρυπτογράφηση, πρέπει να πρώτα να αποκτήσετε τουλάχιστον μία κλίμακα μονάδας (γνωστό και ως ροή μονάδα). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος για να κλιμακωθεί υπηρεσίας πολυμέσων](media-services-portal-manage-streaming-endpoints.md).

##<a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Δυναμική κρυπτογράφηση AES 128 και τη ροή εργασίας υπηρεσίας κλειδιού παράδοσης

Οι παρακάτω είναι γενικά βήματα που θα πρέπει να εκτελέσετε κατά την κρυπτογράφηση τους πόρους σας με AES, χρησιμοποιώντας την υπηρεσία παράδοσης κλειδιού Media Services και χρησιμοποιώντας επίσης δυναμική κρυπτογράφηση.

1. [Δημιουργία ενός περιουσιακού στοιχείου και την αποστολή αρχείων σε περιουσιακού στοιχείου](media-services-protect-with-aes128.md#create_asset).
1. [Κωδικοποίηση παγίου που περιέχει το αρχείο για να το προσαρμόσιμη ρυθμό μετάδοσης bit MP4 ρύθμιση](media-services-protect-with-aes128.md#encode_asset).
1. [Δημιουργήστε έναν αριθμό-κλειδί περιεχομένου και να συσχετίσετε με το κωδικοποιημένο περιουσιακών στοιχείων](media-services-protect-with-aes128.md#create_contentkey). Στις υπηρεσίες πολυμέσων, το κλειδί περιεχομένου περιέχει κλειδιού κρυπτογράφησης του παγίου.
1. [Ρύθμιση παραμέτρων της πολιτικής εξουσιοδότησης του περιεχομένου κλειδιού](media-services-protect-with-aes128.md#configure_key_auth_policy). Η πολιτική περιεχομένου εξουσιοδότησης κλειδιού πρέπει να έχει ρυθμιστεί από εσάς και να πληρούνται από το πρόγραμμα-πελάτη για τον αριθμό-κλειδί περιεχομένου να παραδοθεί στον υπολογιστή-πελάτη.
1. [Ρύθμιση παραμέτρων της πολιτικής παράδοσης για ενός περιουσιακού στοιχείου](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Οι ρυθμίσεις πολιτικής παράδοσης περιλαμβάνουν: βασικές acquisition διεύθυνση URL και ανύσματος προετοιμασίας (IV) (AES 128 απαιτεί το ίδιο IV πρέπει να παρέχονται κατά την κρυπτογράφηση και αποκρυπτογράφηση), παράδοσης protocol (για παράδειγμα, MPEG ΠΑΎΛΑΣ, HLS, σκληροί ΔΊΣΚΟΙ, ομαλή ροή ή όλες), τον τύπο της δυναμική κρυπτογράφηση (για παράδειγμα, φακέλου ή χωρίς δυναμική κρυπτογράφηση).

Μπορείτε να εφαρμόσετε διαφορετική πολιτική για κάθε πρωτόκολλο στο ίδιο πάγιο. Για παράδειγμα, μπορείτε να εφαρμόσετε PlayReady κρυπτογράφησης για ομαλές/ΠΑΎΛΑ και AES φακέλου για να HLS. Όλα τα πρωτόκολλα που δεν έχουν οριστεί σε μια πολιτική παράδοσης (για παράδειγμα, προσθέτετε μια μεμονωμένη πολιτική που καθορίζει μόνο HLS ως το πρωτόκολλο) θα αποκλειστούν από ροής. Η εξαίρεση σε αυτό είναι όταν έχετε καμία πολιτική παράδοσης περιουσιακών στοιχείων που ορίζονται από το καθόλου. Στη συνέχεια, όλα τα πρωτόκολλα θα επιτρέπεται στο Απαλοιφή.

1. [Δημιουργία μιας προσδιοριστικό OnDemand](media-services-protect-with-aes128.md#create_locator) για να λάβετε μια ροή διεύθυνση URL.

Το θέμα θα εμφανίζει επίσης [Πώς μια εφαρμογή προγράμματος-πελάτη μπορούν να ζητήσουν έναν αριθμό-κλειδί από την υπηρεσία παράδοσης κλειδιού](media-services-protect-with-aes128.md#client_request).

Θα βρείτε μια ολοκληρωμένη.NET [παράδειγμα](media-services-protect-with-aes128.md#example) στο τέλος του θέματος.

Η παρακάτω εικόνα δείχνει τη ροή εργασίας που περιγράφονται παραπάνω. Δείτε το διακριτικό χρησιμοποιείται για τον έλεγχο ταυτότητας.

![Προστασία με AES 128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Τα υπόλοιπα αυτό το θέμα παρέχει λεπτομερείς εξηγήσεις παραδείγματα κώδικα και συνδέσεις σε θέματα που σας δείχνουν πώς μπορείτε να επιτύχετε τις εργασίες που περιγράφονται παραπάνω.

##<a name="current-limitations"></a>Τρέχουσα περιορισμοί

Εάν κάνετε προσθήκη ή ενημέρωση πολιτικής παράδοσης του περιουσιακού στοιχείου, πρέπει να διαγράψετε μια υπάρχουσα locator (εάν υπάρχουν) και να δημιουργήσετε ένα νέο προσδιοριστικό.

##<a id="create_asset"></a>Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε αρχεία σε παγίου

Για να διαχειριστείτε, κωδικοποίηση και ροή τα βίντεό σας, πρέπει να αποστείλετε πρώτα το περιεχόμενό σας στο Microsoft Azure Media Services. Μόλις αποσταλεί, το περιεχόμενό σας αποθηκεύονται με ασφάλεια στο cloud για περαιτέρω επεξεργασία και ροής. 

Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Αποστολή αρχείων σε λογαριασμό του Media Services](media-services-dotnet-upload-files.md).

##<a id="encode_asset"></a>Κωδικοποίηση παγίου που περιέχει το αρχείο για να το προσαρμόσιμη ρυθμό μετάδοσης bit Ορισμός MP4

Με δυναμική κρυπτογράφηση μόνο που χρειάζεστε είναι για τη δημιουργία ενός περιουσιακού στοιχείου που περιέχει ένα σύνολο πολλούς-ρυθμό μετάδοσης bit MP4 αρχεία ή αρχεία προέλευσης ομαλή ροή πολλούς-ρυθμό μετάδοσης bit. Στη συνέχεια, ανάλογα με την καθορισμένη μορφή στην αίτηση δηλωτικό ή τμήματος, η ροή On απαιτήσεων διακομιστή θα εξασφαλίσετε ότι θα λαμβάνετε ροής στο πρωτόκολλο που έχετε επιλέξει. Ως αποτέλεσμα, θα πρέπει να αποθηκεύετε και να πληρώσετε για τα αρχεία σε μορφή μία χώρου αποθήκευσης και υπηρεσία Media Services θα δημιουργήσετε και θα λειτουργήσει την κατάλληλη απάντηση που βασίζεται σε αιτήσεις από ένα πρόγραμμα-πελάτη. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση δυναμικής συσκευασία](media-services-dynamic-packaging-overview.md) .

Για οδηγίες σχετικά με τον τρόπο για να κωδικοποιήσετε, δείτε [πώς μπορείτε να κωδικοποιήσετε ενός περιουσιακού στοιχείου με χρήση Media Encoder τυπική](media-services-dotnet-encode-with-media-encoder-standard.md).

##<a id="create_contentkey"></a>Δημιουργήστε έναν αριθμό-κλειδί περιεχομένου και να συσχετίσετε με το κωδικοποιημένο περιουσιακών στοιχείων

Στις υπηρεσίες πολυμέσων, το κλειδί περιεχομένου περιέχει τον αριθμό-κλειδί που θέλετε για την κρυπτογράφηση ενός περιουσιακού στοιχείου με.

Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία περιεχομένου κλειδί](media-services-dotnet-create-contentkey.md).

##<a id="configure_key_auth_policy"></a>Ρύθμιση παραμέτρων της πολιτικής εξουσιοδότησης του περιεχομένου κλειδιού

Υπηρεσίες πολυμέσων υποστηρίζει πολλές τρόποι για τον έλεγχο ταυτότητας τους χρήστες που κάνουν αιτήσεις κλειδιού. Η πολιτική περιεχομένου εξουσιοδότησης κλειδιού πρέπει να έχει ρυθμιστεί από εσάς και να πληρούνται από το πρόγραμμα-πελάτη (πρόγραμμα αναπαραγωγής) για το κλειδί για να παραδοθεί στον υπολογιστή-πελάτη. Η πολιτική περιεχομένου εξουσιοδότησης κλειδιού θα μπορούσε να έχει ένα ή περισσότερα περιορισμοί άδειας: άνοιγμα, διακριτικό περιορισμού ή ο περιορισμός διευθύνσεων IP.

Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων πολιτικής εξουσιοδότησης περιεχομένου αριθμού-κλειδιού](media-services-dotnet-configure-content-key-auth-policy.md).

##<a id="configure_asset_delivery_policy"></a>Ρύθμιση παραμέτρων της πολιτικής παράδοσης περιουσιακών στοιχείων 

Ρύθμιση παραμέτρων της πολιτικής παράδοσης για σας περιουσιακών στοιχείων. Ορισμένα πράγματα που περιλαμβάνει τη ρύθμιση παραμέτρων της πολιτικής παράδοσης περιουσιακών στοιχείων:

- Η διεύθυνση URL acquisition αριθμού-κλειδιού. 
- Την προετοιμασία ανύσματος (IV) για να χρησιμοποιήσετε για την κρυπτογράφηση φακέλου. AES 128 απαιτεί το ίδιο IV πρέπει να παρέχονται κατά την κρυπτογράφηση και αποκρυπτογράφηση. 
- Το περιουσιακών στοιχείων παράδοσης πρωτόκολλο (για παράδειγμα, MPEG ΠΑΎΛΑΣ, HLS, σκληροί ΔΊΣΚΟΙ, ομαλή ροή ή όλες).
- Ο τύπος του δυναμική κρυπτογράφηση (για παράδειγμα, AES φακέλου) ή χωρίς δυναμική κρυπτογράφηση. 

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

##<a id="client_request"></a>Πώς να το πρόγραμμα-πελάτη ζητήσετε έναν αριθμό-κλειδί από την υπηρεσία κλειδιού παράδοσης;

Στο προηγούμενο βήμα, συνταχθεί τη διεύθυνση URL που οδηγεί σε ένα αρχείο δηλώσεων. Το πρόγραμμα-πελάτης πρέπει να εξαγάγετε τις απαραίτητες πληροφορίες από τη ροή δήλωσης αρχείων προκειμένου να κάνετε μια αίτηση για την υπηρεσία παράδοσης κλειδιού.

###<a name="manifest-files"></a>Δήλωσης αρχείων

Ο υπολογιστής-πελάτης πρέπει να εξαγάγετε τη διεύθυνση URL (που περιέχει επίσης περιεχομένου κλειδί αναγνωριστικό (παιδί)) τιμή από το αρχείο δήλωσης. Το πρόγραμμα-πελάτη, στη συνέχεια, θα προσπαθήσει να λάβει του κλειδιού κρυπτογράφησης από την υπηρεσία παράδοσης κλειδιού. Ο υπολογιστής-πελάτης πρέπει επίσης να εξαγάγετε στην τιμή IV και χρησιμοποιήστε το αποκρυπτογραφήσετε τη ροή. Στο ακόλουθο τμήματος κώδικα του <Protection> στοιχείο το δηλωτικό ομαλή ροή.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

Στην περίπτωση HLS, το ριζικό κατάλογο δηλωτικό είναι εσφαλμένος σε αρχεία του τμήματος. 

Για παράδειγμα, είναι το ριζικό κατάλογο δηλωτικό: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) και περιέχει μια λίστα των ονομάτων αρχείων τμήματος.
    
    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Εάν ανοίγετε ένα από τα αρχεία του τμήματος στο πρόγραμμα επεξεργασίας κειμένου (για παράδειγμα, http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), θα πρέπει να περιέχει #EXT X-ΚΛΕΙΔΙΟΎ που υποδηλώνει ότι το αρχείο είναι κρυπτογραφημένο.
    
    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST
    
###<a name="request-the-key-from-the-key-delivery-service"></a>Αίτηση για τον αριθμό-κλειδί από την υπηρεσία παράδοσης κλειδιού

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να στείλετε μια αίτηση με την υπηρεσία κλειδιού παράδοσης Media Services χρησιμοποιώντας ένα πλήκτρο παράδοσης Uri (που έχει εξαχθεί από το δηλωτικό) και ενός διακριτικού (αυτό το θέμα δεν μιλήσετε σχετικά με το πώς μπορείτε να λάβετε τα διακριτικά απλό Web από μια υπηρεσία διακριτικού ασφαλούς).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);
                
        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;
    
        var response = request.GetResponse();
     
        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }
    
        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();
    
        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }
    
##<a id="example"></a>Παράδειγμα

1. Δημιουργήστε ένα νέο έργο κονσόλας.
1. Χρησιμοποιήστε NuGet για να εγκαταστήσετε και να προσθέσετε Azure Media Services .NET SDK επεκτάσεις. Κατά την εγκατάσταση αυτού του πακέτου, εγκαθιστά Media Services .NET SDK και επίσης προσθέτει όλες τις άλλες απαιτούμενες εξαρτήσεις.
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

1. Αντικατάσταση του κώδικα στο αρχείο σας Program.cs με τον κωδικό που εμφανίζονται σε αυτήν την ενότητα.
    
    Βεβαιωθείτε ότι για να ενημερώσετε μεταβλητές, ώστε να οδηγούν σε φακέλους όπου βρίσκονται τα αρχεία εισόδου σας.
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace AESDynamicEncryptionAndKeyDeliverySvc
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // A Uri describing the issuer of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                // The Audience or Scope of the token.  
                // Must match the value in the token for the token to be considered valid.
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
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
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
        
                        // Generate a test token based on the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
        
                        //The GenerateTestToken method returns the token without the word “Bearer” in front
                        //so you have to add it in front of the token string. 
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the bit.ly/aesplayer Flash player to test the URL 
                    // (with open authorization policy). 
                    // Paste the URL and click the Update button to play the video. 
                    //
                    string URL = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
                    Console.WriteLine();
                    Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
                    Console.WriteLine();
        
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
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
        
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
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Create a task with the encoding details, using a string preset.
                    // In this case "H264 Multiple Bitrate 720p" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "H264 Multiple Bitrate 720p",
                        TaskOptions.None);
        
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
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
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy             
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("Open Authorization Policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                        new List<ContentKeyAuthorizationPolicyRestriction>();
        
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        
                    string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));
            
                    // When configuring delivery policy, you can choose to associate it
                    // with a key acquisition URL that has a KID appended or
                    // or a key acquisition URL that does not have a KID appended  
                    // in which case a content key can be reused. 

                    // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
                    // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

                    // The following policy configuration specifies: 
                    // key url that will have KID=<Guid> appended to the envelope and
                    // the Initialization Vector (IV) to use for the envelope encryption.
                    
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                    {
                                {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
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
                    Console.WriteLine("Adding Asset Delivery Policy: " +
                        assetDeliveryPolicy.AssetDeliveryPolicyType);
                }
        
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                EndsWith(".ism")).
                                                FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
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
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
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
            }
        }


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
