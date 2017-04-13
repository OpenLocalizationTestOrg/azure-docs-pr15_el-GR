<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση με PHP" 
    description="Μάθετε πώς να χρησιμοποιείτε Azure ειδοποίηση διανομείς από μια παρασκηνίου PHP." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/07/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-php"></a>Πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση από PHP
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Μπορείτε να αποκτήσετε πρόσβαση σε όλες τις δυνατότητες διανομείς ειδοποίηση από μια Java/PHP/φωνητικής γραφής παρασκηνίου χρησιμοποιώντας το περιβάλλον εργασίας ΥΠΌΛΟΙΠΑ διανομέα ειδοποίηση όπως περιγράφεται στο θέμα MSDN [APIs ΥΠΌΛΟΙΠΑ διανομείς ειδοποίησης](http://msdn.microsoft.com/library/dn223264.aspx).

Σε αυτό το θέμα θα σας δείξουμε πώς μπορείτε να:

* Δημιουργήστε ένα πρόγραμμα-πελάτη ΥΠΌΛΟΙΠΑ για τις δυνατότητες διανομείς ειδοποίηση στο PHP;
* Ακολουθήστε τα [Γρήγορα αποτελέσματα πρόγραμμα εκμάθησης](notification-hubs-ios-apple-push-notification-apns-get-started.md) για την πλατφόρμα σας κινητές συσκευές της επιλογής, εφαρμογή το τμήμα υποστήριξης στο PHP.

## <a name="client-interface"></a>Περιβάλλον εργασίας του προγράμματος-πελάτη
Περιβάλλον εργασίας του κύριου προγράμματος-πελάτη μπορούν να παρέχουν τις ίδιες μεθόδους που είναι διαθέσιμες στο [.NET ειδοποίηση διανομείς SDK](http://msdn.microsoft.com/library/jj933431.aspx), αυτό θα σας επιτρέψει να μεταφράσετε απευθείας όλα τα προγράμματα εκμάθησης και δείγματα αυτήν τη στιγμή διαθέσιμα σε αυτήν την τοποθεσία, και συνεισφέρει από την Κοινότητα στο internet.

Μπορείτε να βρείτε τον κώδικα που διαθέσιμη στο [ΥΠΌΛΟΙΠΟ PHP περιτυλίγματος δείγμα].

Για παράδειγμα, για να δημιουργήσετε ένα πρόγραμμα-πελάτη:

    $hub = new NotificationHub("connection string", "hubname"); 

Για να στείλετε μια iOS εγγενούς ειδοποίηση:
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Εφαρμογή
Εάν δεν είναι ήδη εγγράψατε, ακολουθήστε μας [Γρήγορα αποτελέσματα πρόγραμμα εκμάθησης] μέχρι την τελευταία ενότητα όπου πρέπει να υλοποιήσετε το παρασκηνίου.
Επίσης, εάν θέλετε μπορείτε να χρησιμοποιήσετε τον κωδικό από το [ΥΠΌΛΟΙΠΟ PHP περιτυλίγματος δείγμα] και να μεταβείτε απευθείας στην ενότητα [ολοκλήρωσης το πρόγραμμα εκμάθησης](#complete-tutorial) .

Μπορείτε να βρείτε όλες τις λεπτομέρειες για την υλοποίηση μιας πλήρους περιτυλίγματος ΥΠΌΛΟΙΠΟ στο [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). Σε αυτήν την ενότητα θα σας θα περιγράφει την εφαρμογή PHP της τα κύρια βήματα που απαιτούνται για την πρόσβαση στο ΥΠΌΛΟΙΠΟ διανομείς ειδοποίηση τελικά σημεία:

1. Ανάλυση τη συμβολοσειρά σύνδεσης
2. Δημιουργία το διακριτικό εξουσιοδότησης
3. Εκτελέστε την κλήση HTTP

### <a name="parse-the-connection-string"></a>Ανάλυση τη συμβολοσειρά σύνδεσης

Εδώ είναι η κύρια κλάση εφαρμογής του προγράμματος-πελάτη, των οποίων κατασκευή που αναλύει τη συμβολοσειρά σύνδεσης:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Δημιουργία διακριτικού ασφαλείας
Είναι διαθέσιμες οι λεπτομέρειες της δημιουργίας διακριτικού ασφαλείας [εδώ](http://msdn.microsoft.com/library/dn495627.aspx).
Έχει την ακόλουθη μέθοδο για να προστεθεί η κλάση **NotificationHub** για να δημιουργήσετε το διακριτικό με βάση το URI της τρέχουσας αίτησης και τα διαπιστευτήρια που έχουν εξαχθεί από τη συμβολοσειρά σύνδεσης.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Στείλτε μια ειδοποίηση
Πρώτα, επιτρέψτε μας να ορίσετε μια κλάση που αντιπροσωπεύει μια ειδοποίηση.

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

Αυτή η κατηγορία είναι ένα κοντέινερ για μια ειδοποίηση εγγενούς σώμα, ή ένα σύνολο ιδιοτήτων στην περίπτωση μιας ειδοποίησης πρότυπο και ένα σύνολο κεφαλίδων το οποίο περιέχει μορφοποίηση (εγγενούς πλατφόρμα ή πρότυπο) και τις ιδιότητες συγκεκριμένης πλατφόρμας (όπως την ιδιότητα λήξης Apple και κεφαλίδες WNS).

Ανατρέξτε στην [τεκμηρίωση ειδοποίηση διανομείς ΥΠΌΛΟΙΠΑ APIs](http://msdn.microsoft.com/library/dn495827.aspx) και τις πλατφόρμες των ειδοποιήσεων συγκεκριμένες μορφές για όλες τις διαθέσιμες επιλογές.

Με αυτήν την κλάση ενεργοποιηθεί, θα σας να γράψετε αποστολή μέθοδοι ειδοποίησης μέσα σε τάξη **NotificationHub** .

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

Τις παραπάνω μεθόδους στείλετε μια αίτηση HTTP POST στο τελικό σημείο /messages από το Κέντρο ειδοποίηση σας, με τις σωστές σώμα και κεφαλίδες για να στείλετε την ειδοποίηση.

##<a name="complete-tutorial"></a>Ολοκληρώστε το πρόγραμμα εκμάθησης
Τώρα μπορείτε να ολοκληρώσετε το πρόγραμμα εκμάθησης γρήγορα αποτελέσματα με την αποστολή την ειδοποίηση από μια παρασκηνίου PHP.

Προετοιμασία του υπολογιστή-πελάτη σας ειδοποίηση διανομείς (substitute το όνομα συμβολοσειράς και διανομέα σύνδεσης με instructed στην [Γρήγορα αποτελέσματα εκμάθηση]):

    $hub = new NotificationHub("connection string", "hubname"); 

Στη συνέχεια, προσθέστε τον κώδικα αποστολή ανάλογα με τις κινητές συσκευές πλατφόρμα προορισμού.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store και Windows Phone 8.1 (μη Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 και 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

Εκτέλεση του κώδικα PHP πρέπει να προκαλεί τώρα μια ειδοποίηση που εμφανίζεται στη συσκευή σας προορισμού.


## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το θέμα θα σας εμφάνιζε πώς μπορείτε να δημιουργήσετε ένα απλό πρόγραμμα-πελάτη ΥΠΌΛΟΙΠΑ Java για διανομείς ειδοποίησης. Από εδώ, μπορείτε να:

* Κάντε λήψη του πλήρους [δείγμα περιτυλίγματος ΥΠΌΛΟΙΠΑ PHP], που περιέχει όλα τα παραπάνω κώδικα.
* Συνεχίστε να ενημερώνεστε για διανομείς ειδοποίηση προσθήκης ετικετών δυνατότητα στην [εκμάθηση διακοπή ειδήσεις]
* Μάθετε περισσότερα σχετικά με την προώθηση ειδοποιήσεις σε μεμονωμένους χρήστες σε [ειδοποιήστε τους χρήστες εκμάθηση]

Για περισσότερες πληροφορίες, ανατρέξτε επίσης στο [Κέντρο για προγραμματιστές PHP](/develop/php/).

[Δείγμα περιτυλίγματος PHP ΥΠΌΛΟΙΠΟ]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Γρήγορα αποτελέσματα πρόγραμμα εκμάθησης]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 
