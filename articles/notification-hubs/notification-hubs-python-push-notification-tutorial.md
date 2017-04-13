<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση με Python" 
    description="Μάθετε πώς να χρησιμοποιείτε Azure ειδοποίηση διανομείς από μια παρασκηνίου Python." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-python"></a>Πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση από Python
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
Μπορείτε να αποκτήσετε πρόσβαση σε όλες τις δυνατότητες διανομείς ειδοποίηση από μια Java/PHP/Python/φωνητικής γραφής παρασκηνίου χρησιμοποιώντας το περιβάλλον εργασίας ΥΠΌΛΟΙΠΑ διανομέα ειδοποίηση όπως περιγράφεται στο θέμα MSDN [APIs ΥΠΌΛΟΙΠΑ διανομείς ειδοποίησης](http://msdn.microsoft.com/library/dn223264.aspx).

> [AZURE.NOTE] Αυτό είναι ένα δείγμα εφαρμογής αναφοράς για την εφαρμογή στέλνει την ειδοποίηση στο Python και δεν είναι το επίσημα υποστηριζόμενες SDK Python διανομέα ειδοποιήσεις.
>
> Αυτό το δείγμα είναι γραμμένο με χρήση Python 3.4.

Σε αυτό το θέμα θα σας δείξουμε πώς μπορείτε να:

* Δημιουργήστε ένα πρόγραμμα-πελάτη ΥΠΌΛΟΙΠΑ για τις δυνατότητες διανομείς ειδοποίηση στο Python.
* Αποστολή ειδοποιήσεων με χρήση του περιβάλλοντος εργασίας Python για τα API ΥΠΌΛΟΙΠΑ διανομέα ειδοποίησης. 
* Λήψη μιας ένδειξης της αίτησης/απόκρισης HTTP (REST) για τον εντοπισμό σφαλμάτων/εκπαιδευτικά σκοπό. 

Μπορείτε να ακολουθήσετε τα [Γρήγορα αποτελέσματα πρόγραμμα εκμάθησης](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) για την πλατφόρμα σας κινητές συσκευές της επιλογής, εφαρμογή το τμήμα υποστήριξης στο Python.

> [AZURE.NOTE] Το εύρος του δείγματος περιορίζεται μόνο για την αποστολή ειδοποιήσεων και δεν μπορεί να κάνει οποιαδήποτε διαχείρισης εγγραφής.

## <a name="client-interface"></a>Περιβάλλον εργασίας του προγράμματος-πελάτη
Περιβάλλον εργασίας του κύριου προγράμματος-πελάτη μπορούν να παρέχουν τις ίδιες μεθόδους που είναι διαθέσιμες στο [.NET ειδοποίηση διανομείς SDK](http://msdn.microsoft.com/library/jj933431.aspx). Αυτό θα σας επιτρέψει να μεταφράσετε απευθείας όλα τα προγράμματα εκμάθησης και δείγματα αυτήν τη στιγμή διαθέσιμα σε αυτήν την τοποθεσία και συνεισφέρει από την Κοινότητα στο internet.

Μπορείτε να βρείτε τον κώδικα που διαθέσιμη στο [ΥΠΌΛΟΙΠΟ Python περιτυλίγματος δείγμα].

Για παράδειγμα, για να δημιουργήσετε ένα πρόγραμμα-πελάτη:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
Για να στείλετε μια αναδυόμενη ειδοποίηση των Windows:
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## <a name="implementation"></a>Εφαρμογή
Εάν δεν είναι ήδη εγγράψατε, ακολουθήστε μας [Γρήγορα αποτελέσματα πρόγραμμα εκμάθησης] μέχρι την τελευταία ενότητα όπου πρέπει να υλοποιήσετε το παρασκηνίου.

Μπορείτε να βρείτε όλες τις λεπτομέρειες για την υλοποίηση μιας πλήρους περιτυλίγματος ΥΠΌΛΟΙΠΟ στο [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). Σε αυτήν την ενότητα θα σας θα περιγράφει την εφαρμογή Python της τα κύρια βήματα που απαιτούνται για την πρόσβαση στο ΥΠΌΛΟΙΠΟ διανομείς ειδοποίηση τελικά σημεία και την αποστολή ειδοποιήσεων

1. Ανάλυση τη συμβολοσειρά σύνδεσης
2. Δημιουργία το διακριτικό εξουσιοδότησης
3. Στείλτε μια ειδοποίηση με HTTP REST API

### <a name="parse-the-connection-string"></a>Ανάλυση τη συμβολοσειρά σύνδεσης

Εδώ είναι η κύρια κλάση εφαρμογής του προγράμματος-πελάτη, των οποίων κατασκευή αναλύει τη συμβολοσειρά σύνδεσης:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Δημιουργία διακριτικού ασφαλείας
Είναι διαθέσιμες οι λεπτομέρειες της δημιουργίας διακριτικού ασφαλείας [εδώ](http://msdn.microsoft.com/library/dn495627.aspx).
Έχετε τις παρακάτω μεθόδους για να προστεθεί η κλάση **NotificationHub** για να δημιουργήσετε το διακριτικό με βάση το URI της τρέχουσας αίτησης και τα διαπιστευτήρια που έχουν εξαχθεί από τη συμβολοσειρά σύνδεσης.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Στείλτε μια ειδοποίηση με HTTP REST API
Χρήση πρώτης, σας επιτρέπουν να ορίσετε κλάση που αντιπροσωπεύει μια ειδοποίηση.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Αυτή η κατηγορία είναι ένα κοντέινερ για μια ειδοποίηση εγγενούς σώμα ή ένα σύνολο ιδιοτήτων σε περίπτωση μια ειδοποίηση πρότυπο, ένα σύνολο κεφαλίδων το οποίο περιέχει μορφοποίηση (εγγενούς πλατφόρμα ή πρότυπο) και τις ιδιότητες συγκεκριμένης πλατφόρμας (όπως την ιδιότητα λήξης Apple και κεφαλίδες WNS).

Ανατρέξτε στην [τεκμηρίωση ειδοποίηση διανομείς ΥΠΌΛΟΙΠΑ APIs](http://msdn.microsoft.com/library/dn495827.aspx) και τις πλατφόρμες των ειδοποιήσεων συγκεκριμένες μορφές για όλες τις διαθέσιμες επιλογές.

Τώρα με αυτήν την κλάση, θα σας να συντάξετε Αποστολή ειδοποίησης μεθόδους μέσα σε τάξη **NotificationHub** .

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

Τις παραπάνω μεθόδους στείλετε μια αίτηση HTTP POST στο τελικό σημείο /messages από το Κέντρο ειδοποίηση σας, με τις σωστές σώμα και κεφαλίδες για να στείλετε την ειδοποίηση.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Χρησιμοποιώντας την ιδιότητα εντοπισμού σφαλμάτων για να ενεργοποιήσετε την καταγραφή λεπτομερείς
Ενεργοποίηση της ιδιότητας εντοπισμού σφαλμάτων κατά την προετοιμασία την ενότητα "Ειδοποίηση" θα σύνταξη λεπτομερή καταγραφή πληροφοριών σχετικά με μια αίτηση HTTP και ένδειξης ανταπόκρισης, καθώς και λεπτομερείς μήνυμα ειδοποίησης αποστολή αποτέλεσμα. Έχουμε προσθέσει πρόσφατα αυτήν την ιδιότητα που ονομάζεται [ιδιότητα TestSend διανομείς ειδοποίησης](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) , η οποία επιστρέφει λεπτομερείς πληροφορίες σχετικά με το αποτέλεσμα Αποστολή ειδοποίησης. Για να το χρησιμοποιήσετε - Προετοιμάστε χρησιμοποιώντας τα εξής:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Η αίτηση αποστολής διανομέα ειδοποιήσεων HTTP URL λαμβάνει προσαρτάται με μια συμβολοσειρά ερωτήματος "δοκιμή" ως αποτέλεσμα. 

##<a name="complete-tutorial"></a>Ολοκληρώστε το πρόγραμμα εκμάθησης
Τώρα μπορείτε να ολοκληρώσετε το πρόγραμμα εκμάθησης γρήγορα αποτελέσματα με την αποστολή την ειδοποίηση από μια παρασκηνίου Python.

Προετοιμασία του υπολογιστή-πελάτη σας ειδοποίηση διανομείς (substitute το όνομα συμβολοσειράς και διανομέα σύνδεσης με instructed στην [Γρήγορα αποτελέσματα εκμάθηση]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Στη συνέχεια, προσθέστε τον κώδικα αποστολή ανάλογα με τις κινητές συσκευές πλατφόρμα προορισμού. Αυτό το δείγμα προσθέτει επίσης το ανώτερο επίπεδο μεθόδους για να ενεργοποιήσετε την αποστολή ειδοποιήσεων με βάση την πλατφόρμα, π.χ. send_windows_notification για windows. send_apple_notification (για το apple) κ.λπ. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store και Windows Phone 8.1 (μη Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 και 8.1 Silverlight

    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Εκτέλεση του κώδικα Python θα πρέπει να δημιουργήσουν μια ειδοποίηση που εμφανίζεται στη συσκευή σας προορισμού.

## <a name="examples"></a>Παραδείγματα:

### <a name="enabling-debug-property"></a>Ενεργοποίηση της ιδιότητας εντοπισμού σφαλμάτων
Όταν ενεργοποιήσετε σημαία εντοπισμού σφαλμάτων κατά την προετοιμασία του NotificationHub, στη συνέχεια, θα δείτε λεπτομερείς αίτηση HTTP και ένδειξης ανταπόκρισης, καθώς και NotificationOutcome όπως οι εξής όπου μπορείτε να κατανοήσετε τι κεφαλίδες HTTP μεταβιβάζονται στην πρόσκληση σε και τι απόκριση HTTP ελήφθη από την ενότητα "Ειδοποίηση":    ![][1]

Θα δείτε λεπτομερείς αποτέλεσμα διανομέα ειδοποίηση π.χ. 

- Όταν το μήνυμα αποστέλλεται με επιτυχία με την υπηρεσία ειδοποιήσεων Push. 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- Εάν δεν υπήρχαν χωρίς των προορισμών που βρέθηκε οποιαδήποτε ειδοποιήσεων push, στη συνέχεια, πιθανώς πρόκειται για να δείτε τα εξής στην απάντηση (το οποίο υποδεικνύει ότι δεν υπήρχαν δεν βρέθηκε για παράδοση την ειδοποίηση πιθανώς επειδή τις καταχωρήσεις είχε ορισμένες ετικέτες που δεν συμφωνούν καταχωρήσεων)

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Εκπομπή αναδυόμενη ειδοποίηση για Windows 

Παρατηρήστε τις κεφαλίδες που να αποστέλλεται όταν στέλνετε μια εκπομπής αναδυόμενη ειδοποίηση στον πελάτη των Windows. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Αποστολή ειδοποίησης που καθορίζει μια ετικέτα (ή παράσταση ετικέτα)

Παρατηρήστε την κεφαλίδα HTTP ετικέτες που προστίθενται σε μια αίτηση HTTP (στο παρακάτω παράδειγμα, θα σας στέλνουν μηνύματα την ειδοποίηση μόνο σε εγγραφών με 'sports' φορτίου)

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Αποστολή ειδοποίησης που καθορίζει πολλές ετικέτες

Παρατηρήστε πώς αλλάζει την κεφαλίδα HTTP ετικέτες, όταν αποστέλλονται πολλές ετικέτες. 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Ειδοποίηση προτύπου

Παρατηρήστε ότι οι αλλαγές κεφαλίδα HTTP μορφή και το σώμα φορτίο αποστέλλεται ως μέρος του σώματος αίτηση HTTP:

**Πλευρά του προγράμματος-πελάτη - καταχωρημένο πρότυπο**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**Για διακομιστή - αποστολή το φορτίο**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το θέμα θα σας εμφάνιζε πώς μπορείτε να δημιουργήσετε ένα απλό πρόγραμμα-πελάτη Python ΥΠΌΛΟΙΠΟ για διανομείς ειδοποίησης. Από εδώ, μπορείτε να:

* Κάντε λήψη του πλήρους [ΥΠΌΛΟΙΠΑ Python περιτυλίγματος δείγμα], που περιέχει όλα τα παραπάνω κώδικα.
* Συνεχίστε να ενημερώνεστε για διανομείς ειδοποίηση προσθήκης ετικετών δυνατότητα στην [Εκμάθηση διακοπή συζητήσεων]
* Συνεχίστε να ενημερώνεστε σχετικά με τη δυνατότητα ειδοποίησης διανομείς πρότυπα στην [Εκμάθηση Localizing συζητήσεων]

<!-- URLs -->
[Δείγμα περιτυλίγματος Python ΥΠΌΛΟΙΠΟ]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Γρήγορα αποτελέσματα πρόγραμμα εκμάθησης]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Πρόγραμμα εκμάθησης διακοπή συζητήσεων]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Πρόγραμμα εκμάθησης localizing συζητήσεων]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 
