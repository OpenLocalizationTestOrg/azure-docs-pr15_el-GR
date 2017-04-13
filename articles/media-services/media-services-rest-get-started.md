<properties 
    pageTitle="Γρήγορα αποτελέσματα με την παράδοση περιεχομένου σε ζήτηση χρησιμοποιώντας ΥΠΌΛΟΙΠΑ | Microsoft Azure" 
    description="Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί στα βήματα της εφαρμογής στην αίτηση παράδοσης περιεχομένου απαιτήσεων με υπηρεσίες πολυμέσων Azure χρησιμοποιώντας REST API." 
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
    ms.date="10/11/2016" 
    ms.author="juliako"/>

#<a name="get-started-with-delivering-content-on-demand-using-rest"></a>Γρήγορα αποτελέσματα με την παράδοση περιεχομένου σε ζήτηση χρησιμοποιώντας ΥΠΌΛΟΙΠΟ 

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Azure. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](/pricing/free-trial/?WT.mc_id=A261C142F). 


Αυτή η γρήγορη έναρξη σάς καθοδηγεί σε τα βήματα της εφαρμογής μιας εφαρμογής παράδοσης περιεχομένου Video-on-Demand (VoD) χρησιμοποιώντας APIs ΥΠΌΛΟΙΠΑ Azure Media Services (αθροιστικής μέτρησης ενισχύσεων). 

Το πρόγραμμα εκμάθησης παρουσιάζει τη βασική ροή εργασίας Media Services και τα πιο συνηθισμένα προγραμματισμού αντικείμενα και εργασίες που απαιτούνται για την ανάπτυξη Media Services. Κατά την ολοκλήρωση του προγράμματος εκμάθησης, θα μπορείτε να μεταφέρετε με ροή ή σταδιακά λήψη ενός δείγματος αρχείου πολυμέσων που έχουν αποσταλεί, κωδικοποιημένη και λήψη.  

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Απαιτούνται τα εξής προαπαιτούμενα στοιχεία για να ξεκινήσετε την ανάπτυξη με τις υπηρεσίες πολυμέσων με REST API.

- Κατανόηση του τρόπου ανάπτυξης με Media Services REST API. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πολυμέσων--υπόλοιπο-επισκόπηση των υπηρεσιών](http://msdn.microsoft.com/library/azure/hh973616.aspx).
- Μια εφαρμογή της επιλογής σας που μπορούν να αποστείλουν HTTP προσκλήσεων και απαντήσεων. Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί [Fiddler](http://www.telerik.com/download/fiddler). 

Τις ακόλουθες εργασίες εμφανίζονται σε αυτήν τη Γρήγορη εκκίνηση.

1.  Δημιουργήστε ένα λογαριασμό Media Services (με την πύλη Azure).
2.  Ρύθμιση παραμέτρων ροής τελικού σημείου (με την πύλη Azure).
1.  Σύνδεση με το λογαριασμό Media Services με REST API.
1.  Δημιουργήστε ένα νέο πάγιο και στείλτε ένα αρχείο βίντεο με REST API.
1.  Ρύθμιση παραμέτρων ροής μονάδες με REST API.
2.  Κωδικοποίηση του αρχείου προέλευσης σε ένα σύνολο αρχείων MP4 προσαρμόσιμη ρυθμό μετάδοσης bit με REST API.
1.  Δημοσιεύστε το περιουσιακών στοιχείων και λήψη ροής και προοδευτική λήψη διευθύνσεις URL με REST API. 
1.  Η αναπαραγωγή του περιεχομένου. 

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Δημιουργήστε ένα λογαριασμό των υπηρεσιών Azure Media Services χρησιμοποιώντας την πύλη του Azure

Τα βήματα σε αυτήν την ενότητα δείχνουν πώς μπορείτε να δημιουργήσετε ένα λογαριασμό αθροιστικής μέτρησης ενισχύσεων.

1. Συνδεθείτε στην [πύλη του Azure](https://portal.azure.com/).
2. Κάντε κλικ στο κουμπί **+ νέο** > **πολυμέσων + CDN** > **υπηρεσίες πολυμέσων**.

    ![Δημιουργία υπηρεσίες πολυμέσων](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **ΔΗΜΙΟΥΡΓΊΑ** λογαριασμού ΥΠΗΡΕΣΊΕΣ ΠΟΛΥΜΈΣΩΝ εισαγάγετε απαιτούμενες τιμές.

    ![Δημιουργία υπηρεσίες πολυμέσων](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Στο πεδίο **Όνομα λογαριασμού**, πληκτρολογήστε το όνομα του νέου λογαριασμού αθροιστικής μέτρησης ενισχύσεων. Ένα όνομα λογαριασμού Media Services είναι όλα τα πεζά αριθμούς ή τα γράμματα χωρίς κενά διαστήματα και είναι 3 έως 24 χαρακτήρες.
    2. Στο συνδρομή, επιλέξετε από τις διαφορετικές συνδρομές Azure που έχετε πρόσβαση.
    
    2. Στην **Ομάδα πόρων**, επιλέξτε τον πόρο νέο ή υπάρχον.  Η ομάδα πόρων είναι μια συλλογή από τους πόρους που κάνουν κοινή χρήση κύκλου ζωής, δικαιώματα και πολιτικές. Μάθετε περισσότερα [εδώ](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Στην **τοποθεσία**, επιλέξτε τη γεωγραφική περιοχή θα χρησιμοποιείται για να αποθηκεύσετε τις εγγραφές μέσα και μετα-δεδομένων για το λογαριασμό σας Media Services. Αυτήν την περιοχή χρησιμοποιείται για την επεξεργασία και ροή πολυμέσων σας. Μόνο τις διαθέσιμες υπηρεσίες πολυμέσων περιοχές που εμφανίζονται στο πλαίσιο αναπτυσσόμενης λίστας. 
    
    3. Στο **Χώρο αποθήκευσης λογαριασμού**, επιλέξτε ένα λογαριασμό χώρου αποθήκευσης για την παροχή χώρος αποθήκευσης αντικειμένων blob του περιεχομένου πολυμέσων από το λογαριασμό σας Media Services. Μπορείτε να επιλέξετε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης στην ίδια γεωγραφική περιοχή με το λογαριασμό σας Media Services ή μπορείτε να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης. Δημιουργείται ένα νέο λογαριασμό του χώρου αποθήκευσης στην ίδια περιοχή. Οι κανόνες για τα ονόματα λογαριασμού χώρου αποθήκευσης είναι η ίδια όπως για λογαριασμούς Media Services.

        Μάθετε περισσότερα σχετικά με το χώρο αποθήκευσης [εδώ](storage-introduction.md).

    4. Επιλέξτε **το Pin στον πίνακα εργαλείων** για να δείτε την πρόοδο της ανάπτυξης λογαριασμού.
    
7. Κάντε κλικ στην επιλογή **Δημιουργία** στο κάτω μέρος της φόρμας.

    Αφού δημιουργηθεί το λογαριασμό με επιτυχία, η κατάσταση αλλάζει για να **εκτελείται**. 

    ![Ρυθμίσεις των υπηρεσιών πολυμέσων](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Για να διαχειριστείτε το λογαριασμό σας αθροιστικής μέτρησης ενισχύσεων (για παράδειγμα, αποστολή βίντεο, κωδικοποιείτε περιουσιακών στοιχείων, παρακολούθηση της προόδου εργασίας) Χρησιμοποιήστε το παράθυρο διαλόγου **Ρυθμίσεις** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Ρύθμιση παραμέτρων ροής τελικά σημεία με την πύλη Azure

Όταν εργάζεστε με υπηρεσιών Azure Media Services ένα από τα πιο συνηθισμένα σενάρια την εκτέλεση του βίντεο μέσω προσαρμόσιμη ρυθμό μετάδοσης bit ροής για τους πελάτες σας. Υπηρεσίες πολυμέσων υποστηρίζει το παρακάτω προσαρμόσιμη ρυθμό με μετάδοσης bit ροής τεχνολογίες: HTTP Live ροής (HLS), ομαλή ροή, ΠΑΎΛΑΣ MPEG και σκληροί ΔΊΣΚΟΙ (για αδειών Adobe PrimeTime/πρόσβαση μόνο).

Υπηρεσίες πολυμέσων παρέχει δυναμική συσκευασίας, η οποία σας επιτρέπει να κάνουν σας προσαρμόσιμες ρυθμό μετάδοσης bit MP4 κωδικοποιημένη περιεχομένου σε ροή μορφές που υποστηρίζονται από τις υπηρεσίες πολυμέσων (MPEG ΠΑΎΛΑ, HLS, ομαλή ροή, σκληροί ΔΊΣΚΟΙ) μόνο στιγμή, χωρίς να χρειάζεται να αποθηκεύετε προ-συσκευασμένη εκδόσεις του κάθε μία από αυτές τις μορφές ροής.

Για να επωφεληθείτε από δυναμική συσκευασία, πρέπει να κάνετε τα εξής:

- Κωδικοποίηση του αρχείου θυγατρική (προέλευση) σε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 αρχείων (τα βήματα κωδικοποίησης εκδηλώνονται παρακάτω σε αυτό το πρόγραμμα εκμάθησης).  
- Δημιουργία τουλάχιστον μία ροή μονάδα για τη *ροή τελικό σημείο* από το οποίο σκοπεύετε να παράδοσης το περιεχόμενό σας. Τα παρακάτω βήματα δείχνουν πώς μπορείτε να αλλάξετε τον αριθμό των μονάδων ροής.

Με δυναμική συσκευασία, μόνο πρέπει να αποθηκεύετε και να πληρώσετε για τα αρχεία σε μορφή μία χώρου αποθήκευσης και Media Services δημιουργεί και εξυπηρετεί την κατάλληλη απάντηση που βασίζεται σε αιτήσεις από ένα πρόγραμμα-πελάτη.

Για να δημιουργήσετε και να αλλάξετε τον αριθμό των ροής δεσμευμένη μονάδες, κάντε τα εξής:


1. Στο παράθυρο " **Ρυθμίσεις** ", κάντε κλικ στην επιλογή **ροής τελικά σημεία**. 

2. Κάντε κλικ στην προεπιλεγμένη ροή τελικού σημείου. 

    Εμφανίζεται το παράθυρο **ΠΡΟΕΠΙΛΕΓΜΈΝΗ ΡΟΉ ΛΕΠΤΟΜΈΡΕΙΕΣ τελικού ΣΗΜΕΊΟΥ** .

3. Για να καθορίσετε τον αριθμό των μονάδων ροής, σύρετε το ρυθμιστικό **μονάδες ροής** .

    ![Η ροή μονάδες](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε τις αλλαγές σας.

    >[AZURE.NOTE]Η εκχώρηση οποιαδήποτε νέα μονάδων μπορεί να διαρκέσει έως και 20 λεπτά για να ολοκληρωθεί.

## <a id="connect"></a>Σύνδεση με το λογαριασμό Media Services με REST API

Κατά την πρόσβαση των υπηρεσιών Azure Media Services απαιτούνται οι δύο πράγματα: ένα διακριτικό πρόσβασης που παρέχεται από τις υπηρεσίες ελέγχου πρόσβασης Azure (ACS) και τις υπηρεσίες URI πολυμέσων ίδια. Μπορείτε να χρησιμοποιήσετε οποιοδήποτε μέσο θέλετε κατά τη δημιουργία αυτές τις αιτήσεις με την προϋπόθεση ότι μπορείτε να καθορίσετε τις τιμές σωστή κεφαλίδα και να μεταβίβαση στο διακριτικό πρόσβασης σωστά κατά την κλήση σε υπηρεσίες πολυμέσων.

Ακολουθήστε τα παρακάτω βήματα περιγράφουν τα πιο συνηθισμένα ροής εργασίας όταν χρησιμοποιείτε το Media Services REST API για να συνδεθεί με τις υπηρεσίες πολυμέσων:

1. Γρήγορα ένα διακριτικό πρόσβασης. 
2. Σύνδεση με τις υπηρεσίες πολυμέσων URI.  

    Να θυμάστε ότι μετά τη σύνδεση με επιτυχία στο https://media.windows.net, λαμβάνετε μια 301 redirect καθορίζοντας ένα άλλο URI υπηρεσίες πολυμέσων. Πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI. Ενδέχεται επίσης να λάβετε μια απόκριση HTTP/1.1 200 που περιέχει την περιγραφή μετα-δεδομένων ODATA API.
3. Καταχώρησης τις επόμενες κλήσεις API για τη νέα διεύθυνση URL. 
    
    Για παράδειγμα, εάν μετά τη δοκιμή για να συνδεθείτε, έχετε στη διάθεσή σας τα εξής:
        
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Θα πρέπει να καταχωρείτε τις επόμενες κλήσεις API https://wamsbayclus001rest-hs.cloudapp.net/api/.

###<a name="getting-an-access-token"></a>Γρήγορα ένα διακριτικό πρόσβασης

Για να αποκτήσετε πρόσβαση σε υπηρεσίες πολυμέσων απευθείας από το REST API, ανακτούν ένα διακριτικό πρόσβασης από ACS και χρησιμοποιήστε το κατά τη διάρκεια κάθε αίτηση HTTP που κάνετε στην υπηρεσία. Δεν χρειάζεται τυχόν άλλες προϋποθέσεις πριν συνδεθείτε απευθείας στο Media Services.

Το παρακάτω παράδειγμα εμφανίζει την κεφαλίδα αίτηση HTTP και σώμα που χρησιμοποιείται για την ανάκτηση ενός διακριτικού.

**Κεφαλίδα**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Οργανισμός**:

Πρέπει να αποδείξει τις τιμές client_id και client_secret στο σώμα της πρόσκλησης σε αυτό; client_id και client_secret αντιστοιχούν τις τιμές όνομα λογαριασμού και AccountKey, αντίστοιχα. Αυτές οι τιμές παρέχονται σε εσάς από τις υπηρεσίες πολυμέσων κατά τη ρύθμιση του λογαριασμού σας. 

Το AccountKey για το λογαριασμό σας Media Services πρέπει να είναι κωδικοποίηση URL κατά τη χρήση του ως η τιμή client_secret στην πρόσκληση σε διακριτικό πρόσβασης.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Για παράδειγμα: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


Το παρακάτω παράδειγμα εμφανίζει την απόκριση HTTP που περιέχει την πρόσβαση διακριτικού στο σώμα απόκρισης.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Συνιστάται να τις τιμές "access_token" και "expires_in" σε ένα εξωτερικό χώρο αποθήκευσης στο cache. Τα δεδομένα του διακριτικού θα μπορούσε να αργότερα ανακτηθούν από την αποθήκευση και εκ νέου χρησιμοποιείται σε κλήσεις σας Media Services REST API. Αυτό είναι ιδιαίτερα χρήσιμο για σενάρια όπου το διακριτικό μπορεί να είναι κοινόχρηστη με ασφάλεια μεταξύ πολλών διαδικασιών ή υπολογιστές.

Φροντίστε να παρακολουθείτε τις τιμές "expires_in" το διακριτικό πρόσβασης και να ενημερώσετε τις κλήσεις σας REST API με νέα διακριτικά όπως απαιτείται.

###<a name="connecting-to-the-media-services-uri"></a>Σύνδεση με τις υπηρεσίες πολυμέσων URI

Ο ριζικός κατάλογος URI για τις υπηρεσίες πολυμέσων είναι https://media.windows.net/. Αρχικά, θα πρέπει να συνδέεστε σε αυτό URI και, εάν λάβετε ένα 301 redirect πίσω στο απόκρισης, θα πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI. Επιπλέον, μην χρησιμοποιείτε οποιοδήποτε λογικής αυτόματης-redirect/παρακολούθηση στις προσκλήσεις σε. Ρήματα HTTP και οι οργανισμοί αίτηση δεν θα προωθούνται για το νέο URI.

Το ριζικό κατάλογο URI για αποστολή και λήψη αρχείων περιουσιακού στοιχείου είναι https://yourstorageaccount.blob.core.windows.net/ όπου το όνομα του λογαριασμού χώρου αποθήκευσης είναι το ίδιο που χρησιμοποιήσατε κατά τη διάρκεια της ρύθμισης του λογαριασμού σας Media Services.

Το παρακάτω παράδειγμα παρουσιάζει μια αίτηση HTTP στη ρίζα Media Services URI (https://media.windows.net/). Η πρόσκληση σε λαμβάνει μια 301 redirect πίσω στην απάντηση. Οι επόμενες αίτηση χρησιμοποιεί το νέο URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**Αίτηση HTTP**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**Απόκριση HTTP**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**Αίτηση HTTP** (χρησιμοποιώντας το νέο URI):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**Απόκριση HTTP**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Από τώρα και στο νέο URI χρησιμοποιείται σε αυτό το πρόγραμμα εκμάθησης.

## <a id="upload"></a>Δημιουργήστε ένα νέο πάγιο και στείλτε ένα αρχείο βίντεο με REST API

Στις υπηρεσίες πολυμέσων, μπορείτε να αποστείλετε τα αρχεία σας ψηφιακού σε ενός περιουσιακού στοιχείου. Η οντότητα **περιουσιακών στοιχείων** μπορεί να περιέχει βίντεο, ήχο, εικόνες, μικρογραφιών συλλογές, κείμενο κομμάτια και κλειστές λεζάντες αρχείων (και τα μετα-δεδομένα σχετικά με αυτά τα αρχεία.)  Όταν τα αρχεία έχουν αποσταλεί στο περιουσιακού στοιχείου, το περιεχόμενό σας αποθηκεύονται με ασφάλεια στο cloud για περαιτέρω επεξεργασία και ροής. 

Μία από τις τιμές που έχετε για την παροχή κατά τη δημιουργία ενός περιουσιακού στοιχείου είναι επιλογές για τη δημιουργία περιουσιακού στοιχείου. Η ιδιότητα **Επιλογές** είναι μια τιμή απαρίθμησης που περιγράφει τις επιλογές κρυπτογράφησης που μπορούν να δημιουργηθούν ενός περιουσιακού στοιχείου με. Μια έγκυρη τιμή είναι μία από τις τιμές από τη λίστα παρακάτω, δεν ένας συνδυασμός τιμών από αυτήν τη λίστα:

 
- **Καμία** = χρησιμοποιείται**0** - χωρίς κρυπτογράφηση. Όταν χρησιμοποιείτε αυτήν την επιλογή το περιεχόμενό σας δεν είναι προστατευμένο κατά τη μεταφορά ή στο υπόλοιπο στο χώρο αποθήκευσης.
    Εάν σκοπεύετε να κάνετε μια MP4 χρησιμοποιώντας προοδευτική λήψη, χρησιμοποιήστε αυτήν την επιλογή. 
- **StorageEncrypted** = **1** - κρυπτογραφεί Απαλοιφή περιεχομένου σας τοπικά χρησιμοποιεί κρυπτογράφηση AES-256 bit και το αποστέλλει, στη συνέχεια, χώρο αποθήκευσης Azure όπου είναι αποθηκευμένο κρυπτογραφημένα στο υπόλοιπο. Στοιχεία που προστατεύονται με κρυπτογράφηση χώρου αποθήκευσης είναι αυτόματα χωρίς κρυπτογράφηση τοποθετείται σε ένα σύστημα κρυπτογραφημένο αρχείο πριν από την κωδικοποίηση και προαιρετικά κρυπτογραφημένα εκ νέου πριν από την αποστολή ξανά ως νέο πάγιο εξόδου. Βασική χρήση συμβαίνει κρυπτογράφησης χώρου αποθήκευσης είναι όταν θέλετε να προστατεύσετε τα αρχεία πολυμέσων εισαγωγής υψηλής ποιότητας με ισχυρή κρυπτογράφηση στο υπόλοιπο στο δίσκο.
- **CommonEncryptionProtected** = **2** - Χρησιμοποιήστε αυτή την επιλογή εάν στέλνετε περιεχομένου που έχει ήδη κρυπτογραφημένα και προστατεύεται με κοινές κρυπτογράφηση ή PlayReady DRM (για παράδειγμα, ομαλή ροή προστατεύεται με PlayReady DRM).
- **EnvelopeEncryptionProtected** = **4** – Χρησιμοποιήστε αυτήν την επιλογή εάν στέλνετε HLS κρυπτογραφημένα με AES. Τα αρχεία πρέπει να έχουν κωδικοποιημένη και κρυπτογραφημένα με μετασχηματισμός Manager.

### <a name="create-an-asset"></a>Δημιουργία ενός περιουσιακού στοιχείου

Ενός περιουσιακού στοιχείου είναι ένα κοντέινερ για πολλούς τύπους ή σύνολα αντικειμένων σε υπηρεσίες πολυμέσων, όπως βίντεο, ήχο, εικόνες, συλλογές μικρογραφιών, κομμάτια κειμένου και αρχεία κλειστές λεζάντες. Στο το REST API, τη δημιουργία ενός περιουσιακού στοιχείου απαιτεί Αποστολή αίτησης ΔΗΜΟΣΊΕΥΣΗ στις υπηρεσίες πολυμέσων και τη διάθεση οποιαδήποτε ιδιότητα πληροφορίες σχετικά με το περιουσιακών στοιχείων στο σώμα της αίτησης.

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο δημιουργίας ενός περιουσιακού στοιχείου.

**Αίτηση HTTP**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45
    
    {"Name":"BigBuckBunny.mp4", "Options":"0"}
    

**Απόκριση HTTP**

Εάν είναι επιτυχής, επιστρέφεται τα εξής:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
### <a name="create-an-assetfile"></a>Δημιουργήστε μια AssetFile

Η οντότητα [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) αντιπροσωπεύει ένα αρχείο βίντεο ή ήχου που είναι αποθηκευμένο σε ένα κοντέινερ αντικειμένων blob. Ένα αρχείο περιουσιακού στοιχείου είναι πάντα συσχετισμένη με ενός περιουσιακού στοιχείου και ενός περιουσιακού στοιχείου μπορεί να περιέχει μία ή πολλές AssetFiles. Η εργασία Media Encoder υπηρεσίες αποτυγχάνει εάν ένα αντικείμενο αρχείου περιουσιακών στοιχείων δεν έχει συσχετιστεί με ένα ψηφιακό αρχείο σε ένα κοντέινερ αντικειμένων blob.

Μετά την αποστολή του αρχείου σας ψηφιακών πολυμέσων σε ένα κοντέινερ αντικειμένων blob, μπορείτε να χρησιμοποιήσετε την αίτηση HTTP **ΣΥΓΧΏΝΕΥΣΗ** για να ενημερώσετε το AssetFile με πληροφορίες σχετικά με το αρχείο σας πολυμέσων (όπως φαίνεται παρακάτω στο θέμα). 

**Αίτηση HTTP**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**Απόκριση HTTP**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Δημιουργία του AccessPolicy με δικαιώματα εγγραφής. 

Πριν από την αποστολή των αρχείων στο χώρο αποθήκευσης αντικειμένων blob, ορίστε την πρόσβαση πολιτική δικαιώματα για την εγγραφή σε ενός περιουσιακού στοιχείου. Για να το κάνετε, ΔΗΜΟΣΙΕΎΣΤΕ μια αίτηση HTTP στο σύνολο οντότητα AccessPolicies. Ορισμός της τιμής DurationInMinutes κατά τη δημιουργία ή λαμβάνετε μηνύματος σφάλματος 500 εσωτερικό διακομιστή πίσω στην απάντηση. Για περισσότερες πληροφορίες σχετικά με AccessPolicies, ανατρέξτε στο θέμα [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια AccessPolicy:
        
**Αίτηση HTTP**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**Απόκριση HTTP**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-the-upload-url"></a>Λήψη της διεύθυνσης URL αποστολής

Για να λάβετε τη διεύθυνση URL του πραγματικού αποστολής, δημιουργήστε ένα προσδιοριστικό συσχετισμών Ασφαλείας. Locators Ορισμός της ώρας έναρξης και τον τύπο του τελικού σημείου σύνδεσης για προγράμματα-πελάτες που θέλετε να αποκτήσετε πρόσβαση σε αρχεία σε ενός περιουσιακού στοιχείου. Μπορείτε να δημιουργήσετε πολλών οντοτήτων προσδιοριστικό για μια δεδομένη ζεύγος AccessPolicy και περιουσιακών στοιχείων χειρισμού των προσκλήσεων σε διαφορετικό πρόγραμμα-πελάτη και τις ανάγκες. Κάθε μία από αυτές τις Locators χρησιμοποιεί η τιμή ώρας έναρξης συν την τιμή DurationInMinutes το AccessPolicy για να καθορίσετε το χρονικό διάστημα που μπορεί να χρησιμοποιηθεί μια διεύθυνση URL. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [προσδιοριστικό](http://msdn.microsoft.com/library/azure/hh974308.aspx).


Μια διεύθυνση URL συσχετισμών Ασφαλείας έχει την παρακάτω μορφή:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Εφαρμογή ορισμένα ζητήματα:

- Δεν μπορείτε να έχετε περισσότερους από πέντε μοναδικό Locators που σχετίζεται με μια δεδομένη περιουσιακών στοιχείων κάθε φορά. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα προσδιοριστικό.
- Εάν χρειαστεί να αποστείλετε τα αρχεία σας αμέσως, θα πρέπει να μπορείτε να ορίσετε την τιμή ώρας έναρξης έως πέντε λεπτά πριν από την τρέχουσα ώρα. Αυτό συμβαίνει επειδή μπορεί να υπάρχουν ρολόι skew μεταξύ του υπολογιστή-πελάτη και τις υπηρεσίες πολυμέσων σας. Επίσης, πρέπει να είναι η τιμή ώρας έναρξης σας με την εξής μορφή ημερομηνίας/ώρας: εεεε-MM-DDTHH:mm:ssZ (για παράδειγμα, "2014-05-23T17:53:50Z").   
- Μπορεί να υπάρχουν σε δευτερόλεπτα 30-40 καθυστέρηση αφού δημιουργηθεί ένα προσδιοριστικό να όταν είναι διαθέσιμο για χρήση. Αυτό το θέμα ισχύει για το URL συσχετισμών Ασφαλείας και Origin Locators.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια διεύθυνση URL Locator συσχετισμών Ασφαλείας, όπως καθορίζεται από την ιδιότητα τύπου στο σώμα της αίτησης ("1" για ένα προσδιοριστικό συσχετισμών Ασφαλείας) και "2" για ένα προσδιοριστικό origin On ζήτηση. Η ιδιότητα **διαδρομή** επιστρέφονται περιέχει τη διεύθυνση URL που πρέπει να χρησιμοποιήσετε για να αποστείλετε το αρχείο σας.
    
**Αίτηση HTTP**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178
    
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**Απόκριση HTTP**

Εάν είναι επιτυχής, επιστρέφεται η ακόλουθη απάντηση:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Αποστολή αρχείου σε ένα κοντέινερ χώρου αποθήκευσης αντικειμένων blob
    
Μόλις η AccessPolicy και ορισμός προσδιοριστικό, το ίδιο το αρχείο αποστέλλεται σε ένα κοντέινερ χώρο αποθήκευσης Blob του Azure χρησιμοποιώντας τα Azure αποθήκευσης REST API. Μπορείτε να αποστείλετε στη σελίδα ή να αποκλείσετε αντικείμενα blob. 

>[AZURE.NOTE] Πρέπει να προσθέσετε το όνομα του αρχείου για το αρχείο που θέλετε να αποστείλετε στην τιμή προσδιοριστικό **διαδρομή** λάβει στην προηγούμενη ενότητα. Για παράδειγμα, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Για περισσότερες πληροφορίες σχετικά με την εργασία με αντικείμενα BLOB Azure χώρου αποθήκευσης, ανατρέξτε στο θέμα [Blob υπηρεσίας REST API](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>Ενημέρωση του AssetFile 

Τώρα που έχετε αποστείλει το αρχείο σας, ενημερώστε τις πληροφορίες FileAsset μέγεθος (και άλλα). Για παράδειγμα:
    
    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**Απόκριση HTTP**

Εάν επιτυχής, τα ακόλουθα επιστρέφεται: HTTP/1.1 204 χωρίς περιεχόμενο

## <a name="delete-the-locator-and-accesspolicy"></a>Διαγράψτε το προσδιοριστικό και AccessPolicy 

**Αίτηση HTTP**


    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    
**Απόκριση HTTP**

Εάν είναι επιτυχής, επιστρέφεται τα εξής:

    HTTP/1.1 204 No Content 
    ...

**Αίτηση HTTP**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**Απόκριση HTTP**

Εάν είναι επιτυχής, επιστρέφεται τα εξής:

    HTTP/1.1 204 No Content 
    ...

 
## <a id="configure_streaming_units"></a>Ρύθμιση παραμέτρων ροής μονάδες με REST API

Όταν εργάζεστε με υπηρεσιών Azure Media Services ένα από τα πιο συνηθισμένα σενάρια προσφέρει προσαρμόσιμη ρυθμό μετάδοσης bit ροής για τους πελάτες σας. Με ροή προσαρμόσιμη ρυθμό μετάδοσης bit, το πρόγραμμα-πελάτη να μεταβείτε σε μια ροή ρυθμό μετάδοσης bit μεγαλύτερη ή μικρότερη, όπως το βίντεο εμφανίζεται με βάση το τρέχον εύρος ζώνης δικτύου, η χρήση της CPU και άλλους παράγοντες. Υπηρεσίες πολυμέσων υποστηρίζει το παρακάτω προσαρμόσιμη ρυθμό με μετάδοσης bit ροής τεχνολογίες: HTTP Live ροής (HLS), ομαλή ροή, ΠΑΎΛΑΣ MPEG και σκληροί ΔΊΣΚΟΙ (για αδειών Adobe PrimeTime/πρόσβαση μόνο). 

Υπηρεσίες πολυμέσων παρέχει δυναμική συσκευασίας, η οποία σας επιτρέπει να παραδώσετε το περιεχόμενό σας προσαρμόσιμες ρυθμό μετάδοσης bit MP4 ή ομαλή ροή κωδικοποιημένη σε ροή μορφές που υποστηρίζονται από τις υπηρεσίες πολυμέσων (MPEG ΠΑΎΛΑ, HLS, ομαλή ροή, σκληροί ΔΊΣΚΟΙ) χωρίς να χρειάζεται να δημιουργήσετε νέα συσκευασία σε αυτές τις μορφές ροής. 

Για να επωφεληθείτε από δυναμική συσκευασία, πρέπει να κάνετε τα εξής:

- λήψη τουλάχιστον μία ροή μονάδας για τη **ροή τελικό σημείο **από το οποίο σκοπεύετε να διαθέσετε το περιεχόμενό σας (που περιγράφεται σε αυτήν την ενότητα).
- Μετατρέπει το θυγατρική (προέλευση) αρχείου σε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 ή προσαρμόσιμη ρυθμό μετάδοσης bit ομαλή ροή αρχείων (τα βήματα κωδικοποίησης εκδηλώνονται παρακάτω σε αυτό το πρόγραμμα εκμάθησης), ή κωδικοποίηση  

Με δυναμική συσκευασία, μόνο πρέπει να αποθηκεύετε και να πληρώσετε για τα αρχεία σε μορφή μία χώρου αποθήκευσης και Media Services δημιουργεί και εξυπηρετεί την κατάλληλη απάντηση που βασίζεται σε αιτήσεις από ένα πρόγραμμα-πελάτη. 


>[AZURE.NOTE] Για πληροφορίες σχετικά με τις πληροφορίες τιμολόγησης λεπτομέρειες, ανατρέξτε στο θέμα [Λεπτομέρειες τιμολόγησης υπηρεσίες πολυμέσων](http://go.microsoft.com/fwlink/?LinkId=275107).

Για να αλλάξετε τον αριθμό των ροής δεσμευμένη μονάδες, κάντε τα εξής:
    
### <a name="get-the-streaming-endpoint-you-want-to-update"></a>Λάβετε το ροής τελικό σημείο που θέλετε να ενημερώσετε

Για παράδειγμα, ας ξεκινήσουμε το πρώτο τελικό σημείο ροής στο λογαριασμό σας (μπορείτε να έχετε έως και δύο τελικά σημεία ροής στην κατάσταση που εκτελείται την ίδια στιγμή.)

**Αίτηση HTTP**:

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints()?$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**Απόκριση HTTP**
    
Εάν είναι επιτυχής, επιστρέφεται τα εξής:
    
    HTTP/1.1 200 OK
    . . . 
    
### <a name="scale-the-streaming-endpoint"></a>Κλίμακα το τελικό σημείο ροής
 
**Αίτηση HTTP**:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints('nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486')/Scale HTTP/1.1
    Content-Type: application/json;odata=verbose
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {"scaleUnits":1}

**Απόκριση HTTP**

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    x-ms-request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:16:43 GMT
    Content-Length: 0

    
### <a id="long_running_op_status"></a>Έλεγχος της κατάστασης μιας λειτουργίας μεγάλη διάρκεια εκτέλεσης

Η εκχώρηση οποιαδήποτε νέα μονάδων χρειάζεται περίπου 20 λεπτά για να ολοκληρωθεί. Για να ελέγξετε την κατάσταση της λειτουργίας χρήσης της μεθόδου **Λειτουργίες** και καθορίστε το αναγνωριστικό της λειτουργίας. Η λειτουργία αναγνωριστικό επιστράφηκε στην απάντηση στην πρόσκληση σε **κλίμακα** .

    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
 
**Αίτηση HTTP**:
    
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7') HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
**Απόκριση HTTP**
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 515
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 829e1a89-3ec2-4836-a04d-802b5aeff5e8
    request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    x-ms-request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:57:39 GMT
    
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Operation"
          },
          "Id":"nb:opid:UUID:cc339c28-6bba-4f7d-bee5-91ea4a0a907e",
          "State":"Succeeded",
          "TargetEntityId":"nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486",
          "ErrorCode":null,
          "ErrorMessage":null
       }
    }


## <a id="encode"></a>Κωδικοποίηση του αρχείου προέλευσης σε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 αρχείων

Μετά την ingesting μπορεί να είναι κωδικοποιημένη περιουσιακών στοιχείων σε υπηρεσίες πολυμέσων πολυμέσων, transmuxed, Υδατογραφημένο, και ούτω καθεξής, πριν παραδοθεί στους υπολογιστές-πελάτες. Αυτές οι δραστηριότητες είναι προγραμματισμένη και να εκτελεστεί σε πολλές παρουσίες ρόλο φόντου για να βεβαιωθείτε ότι υψηλές επιδόσεις και τη διαθεσιμότητα. Αυτές οι δραστηριότητες ονομάζονται εργασίες και κάθε [εργασία](http://msdn.microsoft.com/library/azure/hh974289.aspx) αποτελείται από μεμονωμένες εργασίες που κάνετε την πραγματική εργασία στο αρχείο περιουσιακών στοιχείων. 

Όπως αναφέρθηκε προηγουμένως, όταν εργάζεστε με Azure Media Services ένα από τα πιο συνηθισμένα σενάρια προσφέρει προσαρμόσιμη ρυθμό μετάδοσης bit ροής για τους πελάτες σας. Υπηρεσίες πολυμέσων δυναμικά να συσκευάσετε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 αρχείων σε μια από τις παρακάτω μορφές: HTTP Live ροής (HLS), ομαλή ροή, ΠΑΎΛΑΣ MPEG και σκληροί ΔΊΣΚΟΙ (για αδειών Adobe PrimeTime/πρόσβαση μόνο). 

Για να επωφεληθείτε από δυναμική συσκευασία, πρέπει να κάνετε τα εξής:

- Μετατρέπει το θυγατρική (προέλευση) αρχείου σε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 αρχεία ή αρχεία ομαλή ροή προσαρμόσιμη ρυθμό μετάδοσης bit, ή κωδικοποίηση  
- λήψη τουλάχιστον μία ροή μονάδας για τη ροή τελικό σημείο από το οποίο σκοπεύετε να διαθέσετε το περιεχόμενό σας. 

Στην παρακάτω ενότητα δείχνει πώς μπορείτε να δημιουργήσετε μια εργασία που περιέχει μία εργασία κωδικοποίησης. Η εργασία καθορίζει μετατρέπει το αρχείο θυγατρική σε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4s χρησιμοποιώντας **Media Encoder τυπική**. Επίσης, η ενότητα δείχνει τον τρόπο για την παρακολούθηση της επεξεργασίας της προόδου εργασίας. Όταν ολοκληρωθεί η εργασία, θα μπορούν να δημιουργούν locators που απαιτούνται για να αποκτήσετε πρόσβαση για τους πόρους σας. 

### <a name="get-a-media-processor"></a>Λάβετε ένα πρόγραμμα επεξεργασίας πολυμέσων

Στις υπηρεσίες πολυμέσων, ένα πρόγραμμα επεξεργασίας πολυμέσων είναι ένα στοιχείο που χειρίζεται μια εργασία συγκεκριμένες επεξεργασίας, όπως η κωδικοποίηση, μετατροπή μορφής περιεχομένου κρυπτογράφηση ή αποκρυπτογράφηση πολυμέσων. Για την εργασία κωδικοποίησης που εμφανίζονται σε αυτό το πρόγραμμα εκμάθησης, θα σας πρόκειται να χρησιμοποιήσετε την τυπική κωδικοποιητή πολυμέσων.

Ο ακόλουθος κώδικας ζητά ο κωδικοποιητής αναγνωριστικό. 

**Αίτηση HTTP**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**Απόκριση HTTP**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Δημιουργία μιας εργασίας

Κάθε εργασία μπορεί να έχει μία ή περισσότερες εργασίες ανάλογα με τον τύπο της επεξεργασίας που θέλετε να εκτελέσετε. Έως το REST API, μπορείτε να δημιουργήσετε εργασίες και τις σχετικές εργασίες με έναν από τους δύο τρόπους: εργασίες μπορεί να είναι καθορισμένο ενσωματωμένη μέσω της ιδιότητας περιήγηση εργασίες στην εργασία οντοτήτων, ή μέσω OData μαζική επεξεργασία. Το Media Services SDK χρησιμοποιεί μαζική επεξεργασία. Ωστόσο, για την αναγνωσιμότητα των τα παραδείγματα κώδικα σε αυτό το θέμα, οι εργασίες είναι καθορισμένο ενσωματωμένη. Για πληροφορίες σχετικά με την επεξεργασία δέσμης, ανατρέξτε στο θέμα [Επεξεργασία δέσμης Open Data Protocol (OData)](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

Το παρακάτω παράδειγμα δείχνει πώς να δημιουργήσετε και να καταχωρήσετε μια εργασία με μία εργασία ρυθμιστεί για την κωδικοποίηση βίντεο σε μια συγκεκριμένη ανάλυση και την ποιότητα. Στην παρακάτω ενότητα τεκμηρίωση περιέχει τη λίστα με όλα τα [υποδείγματα εργασίας](http://msdn.microsoft.com/library/mt269960) που υποστηρίζονται από το Media Encoder τυπική επεξεργαστή.  

**Αίτηση HTTP**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**Απόκριση HTTP**

Εάν είναι επιτυχής, επιστρέφεται η ακόλουθη απάντηση:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  
    
             ]
          }
       }
    }


Υπάρχουν μερικά σημαντικά πράγματα που πρέπει να λάβετε υπόψη σε οποιαδήποτε αίτηση εργασίας:

- Ιδιότητες TaskBody πρέπει να χρησιμοποιήσετε το ακριβές XML για να ορίσετε τον αριθμό των εισόδου ή εξόδου πόρους που χρησιμοποιούνται από την εργασία. Το θέμα της εργασίας περιέχει ο ορισμός σχήματος XML για το αρχείο XML.
- Στον ορισμό TaskBody, τιμή για κάθε εσωτερική <inputAsset> και <outputAsset> πρέπει να οριστεί ως JobInputAsset(value) ή JobOutputAsset(value).
- Μια εργασία μπορεί να έχει πολλά στοιχεία εξόδου. Μία JobOutputAsset(x) μπορεί να χρησιμοποιηθεί μόνο μία φορά ως το αποτέλεσμα μιας εργασίας σε ένα έργο.
- Μπορείτε να καθορίσετε JobInputAsset ή JobOutputAsset ως ενός περιουσιακού στοιχείου εισαγωγής μιας εργασίας.
- Εργασίες δεν πρέπει να αποτελούν έναν κύκλο.
- Η παράμετρος value που έχετε στείλει JobInputAsset ή JobOutputAsset αντιπροσωπεύει την τιμή ευρετηρίου για ενός περιουσιακού στοιχείου. Τα πραγματικά στοιχεία ορίζονται στις ιδιότητες InputMediaAssets και OutputMediaAssets περιήγησης στον ορισμό οντότητα εργασίας. 

>[AZURE.NOTE] Επειδή το Media Services είναι ενσωματωμένη σε OData v3, των μεμονωμένων περιουσιακών στοιχείων InputMediaAssets και OutputMediaAssets συλλογές ιδιοτήτων περιήγησης αναφέρονται μέσω ενός "__metadata: uri" ζεύγος ονόματος-τιμής. 

- InputMediaAssets αντιστοιχεί σε ένα ή περισσότερα στοιχεία που έχετε δημιουργήσει στις υπηρεσίες πολυμέσων. OutputMediaAssets δημιουργούνται από το σύστημα. Αυτά παραπέμπει σε μια υπάρχουσα περιουσιακών στοιχείων.
- OutputMediaAssets μπορείτε να δώσετε όνομα χρησιμοποιώντας το χαρακτηριστικό assetName. Εάν δεν υπάρχει αυτό το χαρακτηριστικό και, στη συνέχεια, το όνομα του OutputMediaAsset είναι ανεξάρτητα από την τιμή εσωτερικό κείμενο της το <outputAsset> στοιχείο είναι με κατάληξη είτε την τιμή όνομα έργου ή την τιμή αναγνωριστικό εργασίας (στην περίπτωση όπου δεν έχει οριστεί η ιδιότητα Name). Για παράδειγμα, εάν ορίσετε μια τιμή για assetName "Δείγμα", στη συνέχεια, η ιδιότητα OutputMediaAsset όνομα θα οριστεί σε "Δείγμα". Ωστόσο, εάν δεν έχει οριστεί τιμή για assetName, αλλά έχει αλλάξει ορίστε το όνομα της εργασίας για να "NewJob", στη συνέχεια, το όνομα OutputMediaAsset θα ήταν "_NewJob JobOutputAsset (τιμή)". 

    Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να ορίσετε το χαρακτηριστικό assetName:
    
        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"


- Για να ενεργοποιήσετε την αλυσιδωτή εργασίας:

    - Μια εργασία, πρέπει να έχετε τουλάχιστον δύο εργασίες
    - Πρέπει να υπάρχει τουλάχιστον μία εργασία των οποίων εισόδου είναι εξόδου από μια άλλη εργασία της εργασίας.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία μιας εργασίας κωδικοποίηση με το Media Services REST API](http://msdn.microsoft.com/library/azure/jj129574.aspx).

### <a name="monitor-processing-progress"></a>Οθόνη επεξεργασίας προόδου

Μπορείτε να ανακτήσετε την κατάσταση της εργασίας, χρησιμοποιώντας την ιδιότητα μέλους, όπως φαίνεται στο παρακάτω παράδειγμα. 

**Αίτηση HTTP**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**Απόκριση HTTP**

Εάν είναι επιτυχής, επιστρέφεται η ακόλουθη απάντηση:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT
    
    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Ακύρωση μιας εργασίας

Υπηρεσίες πολυμέσων σάς επιτρέπει να ακυρώσετε εκτελείται εργασίες μέσω της συνάρτησης CancelJob. Αυτή η κλήση επιστρέφει έναν κωδικό 400 σφάλματος εάν προσπαθήσετε να ακυρώσετε μια εργασία όταν ακυρωθεί η κατάστασή, ακύρωση, σφάλμα ή ολοκληρωθεί.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να καλέσετε CancelJob.


**Αίτηση HTTP**


    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Εάν είναι επιτυχής, ένας κωδικός 204 απάντηση επιστρέφεται με χωρίς σώμα του μηνύματος.

>[AZURE.NOTE] Πρέπει να κωδικοποίηση URL το αναγνωριστικό εργασίας (συνήθως nb:jid:UUID: somevalue) κατά τη διέλευση με ως παράμετρο, για να CancelJob.


### <a name="get-the-output-asset"></a>Λήψη του παγίου εξόδου 

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να ζητήσετε παγίου εξόδου αναγνωριστικό. 


**Αίτηση HTTP**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**Απόκριση HTTP**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>Δημοσίευση του περιουσιακού στοιχείου και λήψη ροής και προοδευτική λήψη διευθύνσεις URL με REST API

Ροή ή λήψη ενός περιουσιακού στοιχείου, πρέπει πρώτα να "Δημοσίευση", δημιουργώντας ένα προσδιοριστικό. Locators παρέχουν πρόσβαση σε αρχεία που περιέχονται στο περιουσιακού στοιχείου. Υπηρεσίες πολυμέσων υποστηρίζει δύο τύπους locators: locators OnDemandOrigin, χρησιμοποιούνται για ροή πολυμέσων (για παράδειγμα, MPEG ΠΑΎΛΑΣ, HLS ή ομαλή ροή) και locators υπογραφή (συσχετισμών Ασφαλείας πρόσβασης), που χρησιμοποιείται για τη λήψη αρχείων πολυμέσων.

Αφού δημιουργήσετε το locators, μπορείτε να δημιουργήσετε τις διευθύνσεις URL που χρησιμοποιούνται για τη ροή ή λήψη των αρχείων σας. 


Ροή διεύθυνση URL για την ομαλή ροή έχει την παρακάτω μορφή:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Ροή διεύθυνση URL για την HLS έχει την παρακάτω μορφή:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Ροή διεύθυνση URL για την ΠΑΎΛΑ MPEG έχει την παρακάτω μορφή:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Μια διεύθυνση URL συσχετισμών Ασφαλείας που χρησιμοποιείται για να κάνετε λήψη αρχείων έχει την παρακάτω μορφή:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Αυτή η ενότητα δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες είναι απαραίτητο να "δημοσίευσης" τους πόρους σας.  

- Δημιουργία του AccessPolicy με δικαίωμα ανάγνωσης 
- Δημιουργία μιας διεύθυνσης URL συσχετισμών Ασφαλείας για τη λήψη περιεχομένου 
- Δημιουργία μιας διεύθυνσης URL προέλευσης για ροή περιεχομένου 

###<a name="creating-the-accesspolicy-with-read-permission"></a>Δημιουργία του AccessPolicy με δικαίωμα ανάγνωσης

Πριν από τη λήψη ή οποιοδήποτε περιεχόμενο πολυμέσων ροής, ορίστε μια AccessPolicy με δικαιώματα ανάγνωσης και δημιουργήστε την κατάλληλη οντότητα προσδιοριστικό που καθορίζει τον τύπο του μηχανισμού παράδοσης που θέλετε να ενεργοποιήσετε για τους πελάτες σας. Για περισσότερες πληροφορίες σχετικά με τις ιδιότητες που είναι διαθέσιμες, ανατρέξτε στο θέμα [Ιδιότητες οντότητας AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx#accesspolicy_properties).

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να καθορίσετε το AccessPolicy για δικαιώματα ανάγνωσης για μια δεδομένη περιουσιακών στοιχείων.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Εάν είναι επιτυχής, επιστρέφεται κωδικό 201 επιτυχίας που περιγράφει την οντότητα AccessPolicy που δημιουργήσατε. Στη συνέχεια, χρησιμοποιήστε το αναγνωριστικό AccessPolicy μαζί με το αναγνωριστικό περιουσιακών στοιχείων του περιουσιακού στοιχείου που περιέχει το αρχείο που θέλετε για την παράδοση (όπως ενός περιουσιακού στοιχείου εξόδου) για να δημιουργήσετε το προσδιοριστικό οντότητα.

>[AZURE.NOTE]
Αυτή η ροή εργασίας βασικές είναι ίδια με την αποστολή ενός αρχείου όταν ingesting ενός περιουσιακού στοιχείου (όπως αναφέρθηκε προηγουμένως σε αυτό το θέμα). Επίσης, όπως αποστολή αρχείων, εάν εσείς (ή τους πελάτες σας), πρέπει να αμέσως πρόσβαση στα αρχεία σας, ορίστε την τιμή ώρας έναρξης έως πέντε λεπτά πριν από την τρέχουσα ώρα. Αυτή η ενέργεια είναι απαραίτητο, επειδή μπορεί να υπάρχουν ρολόι skew μεταξύ του προγράμματος-πελάτη και τις υπηρεσίες πολυμέσων. Η τιμή ώρας έναρξης πρέπει να είναι με την εξής μορφή ημερομηνίας και ώρας: εεεε-MM-DDTHH:mm:ssZ (για παράδειγμα, "2014-05-23T17:53:50Z").


###<a name="creating-a-sas-url-for-downloading-content"></a>Δημιουργία μιας διεύθυνσης URL συσχετισμών Ασφαλείας για τη λήψη περιεχομένου 

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να λάβετε μια διεύθυνση URL που μπορούν να χρησιμοποιηθούν για να κάνετε λήψη ενός αρχείου πολυμέσων δημιουργούνται και να αποστείλει προηγουμένως. Το AccessPolicy έχει ανάγνωσης Ορισμός δικαιωμάτων και τη διαδρομή προσδιοριστικό αναφέρεται σε μια διεύθυνση URL λήψης συσχετισμών Ασφαλείας.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Εάν είναι επιτυχής, επιστρέφεται η ακόλουθη απάντηση:

    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


Η ιδιότητα επιστρέφεται **διαδρομή** περιέχει τη διεύθυνση URL συσχετισμών Ασφαλείας.

>[AZURE.NOTE]
Εάν κάνετε λήψη περιεχομένου χώρου αποθήκευσης κρυπτογραφημένα, που πρέπει να με μη αυτόματο τρόπο αποκρυπτογραφήσει πριν από την απόδοση του, ή χρησιμοποιήστε το MediaProcessor αποκρυπτογράφηση χώρου αποθήκευσης σε μια εργασία επεξεργασίας για επεξεργασία αρχεία στο Απαλοιφή σε ένα OutputAsset εξόδου και, στη συνέχεια, κάντε λήψη του από συγκεκριμένο περιουσιακών στοιχείων. Για περισσότερες πληροφορίες σχετικά με την επεξεργασία, ανατρέξτε στο θέμα Δημιουργία μιας εργασίας κωδικοποίηση με το Media Services REST API. Επίσης, δεν μπορεί να ενημερωθεί συσχετισμών Ασφαλείας διεύθυνσης URL Locators αφού που έχουν δημιουργήσει. Για παράδειγμα, δεν μπορείτε να χρησιμοποιήσετε ξανά το ίδιο προσδιοριστικό με μια ενημερωμένη η τιμή ώρας έναρξης. Αυτό είναι λόγω τον τρόπο που δημιουργούνται διευθύνσεις URL συσχετισμών Ασφαλείας. Εάν θέλετε να αποκτήσει πρόσβαση ενός περιουσιακού στοιχείου για λήψη μετά ένα προσδιοριστικό έχει λήξει, πρέπει να μπορείτε να δημιουργήσετε ένα νέο με μια νέα ώρα έναρξης.

###<a name="download-files"></a>Λήψη αρχείων

Μόλις η AccessPolicy και προσδιοριστικό ρύθμιση, μπορείτε να κάνετε λήψη αρχείων χρησιμοποιώντας τα Azure αποθήκευσης REST API.  

>[AZURE.NOTE] Πρέπει να προσθέσετε το όνομα του αρχείου για το αρχείο που θέλετε να κάνετε λήψη της τιμής προσδιοριστικό **διαδρομή** λάβει στην προηγούμενη ενότητα. Για παράδειγμα, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Για περισσότερες πληροφορίες σχετικά με την εργασία με αντικείμενα BLOB Azure χώρου αποθήκευσης, ανατρέξτε στο θέμα [Blob υπηρεσίας REST API](http://msdn.microsoft.com/library/azure/dd135733.aspx).

Λόγω της κωδικοποίησης εργασίας που έχετε εκτελέσει προηγούμενες (κωδικοποίηση στο σύνολο προσαρμόσιμων MP4), έχετε πολλά αρχεία MP4 που σταδιακά, μπορείτε να κάνετε λήψη. Για παράδειγμα:    
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Δημιουργία μιας διεύθυνσης URL ροής για ροή περιεχομένου


Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να δημιουργήσετε μια ροή προσδιοριστικό διεύθυνση URL:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Εάν είναι επιτυχής, επιστρέφεται η ακόλουθη απάντηση:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Για τη ροή ομαλή ροή origin διεύθυνσης URL σε ένα πρόγραμμα αναπαραγωγής ροής, πρέπει να προσαρτήσετε τη διαδρομή ιδιότητα με το όνομα του την ομαλή ροή αρχείο, ακολουθούμενο από δήλωσης "/ δήλωσης".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

Για τη ροή HLS, προσάρτηση (μορφή = m3u8 aapl) μετά το "/ δήλωσης".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

Για τη ροή MPEG ΠΑΎΛΑ, προσάρτηση (μορφή = mpd-χρόνου-csf) μετά το "/ δήλωσης".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Αναπαραγωγή περιεχομένου σας  

Για τη ροή βίντεο, χρησιμοποιήστε το [Πρόγραμμα αναπαραγωγής υπηρεσίες πολυμέσων Azure](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Για να ελέγξετε προοδευτική λήψη, επικολλήστε μια διεύθυνση URL στο πρόγραμμα περιήγησης (για παράδειγμα, IE, Chrome, Safari).


##<a name="next-steps-media-services-learning-paths"></a>Επόμενα βήματα: Media Services διαδικασίες εκμάθησης

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="looking-for-something-else"></a>Αναζητάτε κάτι άλλο;

Εάν αυτό το θέμα δεν περιέχουν τι περιμένατε, κάτι που λείπει ή σε κάποιο άλλο τρόπο δεν καλύπτει τις ανάγκες σας, πρέπει να δώσετε μας με χρήση του παρακάτω Disqus νήματος τα σχόλιά σας.



