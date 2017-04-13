<properties
    pageTitle="Κοινή χρήση υπογραφών Access Επισκόπηση | Microsoft Azure"
    description="Τι είναι οι υπογραφές πρόσβασης σε κοινή χρήση, πώς λειτουργούν, και πώς μπορείτε να τις χρησιμοποιήσετε από κόμβο, PHP και C#."
    services="service-bus"
    documentationCenter="na"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/02/2016"
    ms.author="darosa;sethm"/>

# <a name="shared-access-signatures"></a>Κοινόχρηστη πρόσβαση υπογραφών

*Κοινή χρήση υπογραφών πρόσβασης* (Συσχετισμών Ασφαλείας) είναι ο μηχανισμός πρωτεύοντος ασφαλείας για υπηρεσία Bus, συμπεριλαμβανομένων των συμβάντων διανομείς, όπου μεσολαβούν ανταλλαγής μηνυμάτων (ουρές και θέματα) και αναμετάδοση μηνυμάτων. Σε αυτό το άρθρο περιγράφει θέσει σε κοινή χρήση υπογραφών Access, πώς λειτουργούν και πώς να τους χρησιμοποιήσετε με τον τρόπο πλατφόρμα agnostic.

## <a name="overview-of-sas"></a>Επισκόπηση των συσχετισμών Ασφαλείας

Κοινόχρηστο υπογραφές πρόσβασης είναι ένας μηχανισμός ελέγχου ταυτότητας που βασίζεται σε ασφαλή Κατακερματισμοί SHA-256 ή URIs. Συσχετίσεις Ασφαλείας είναι ένας μηχανισμός ιδιαίτερα ισχυρή που χρησιμοποιείται από όλες τις υπηρεσίες Bus υπηρεσίας. Πραγματική χρησιμοποιείται, συσχετισμών Ασφαλείας έχουν δύο στοιχεία: μια *Πολιτική πρόσβασης σε κοινή χρήση* και *Θέσει σε κοινή χρήση υπογραφής πρόσβασης* (συχνά ονομάζονται ενός *διακριτικού*).

Μπορείτε να βρείτε πιο λεπτομερείς πληροφορίες σχετικά με τις υπογραφές πρόσβασης σε κοινή χρήση με υπηρεσία Bus [υπογραφή πρόσβασης σε κοινή χρήση](service-bus-shared-access-signature-authentication.md)του ελέγχου ταυτότητας με Bus υπηρεσίας.

## <a name="shared-access-policy"></a>Πολιτική κοινόχρηστο πρόσβασης

Ένα σημαντικό σημείο για την κατανόηση συσχετισμών Ασφαλείας είναι ότι όλα ξεκινά με μια πολιτική. Για κάθε πολιτική, αποφασίσετε σε τρία τμήματα των πληροφοριών: **το όνομα**, το **αντικείμενο**και **δικαιώματα**. Το **όνομα** είναι ακριβώς αυτό; ένα μοναδικό όνομα μέσα σε αυτό το εύρος. Το πεδίο εφαρμογής είναι αρκετά εύκολο: είναι το URI του συγκεκριμένου πόρου. Για ένα πεδίο Bus υπηρεσία ονομάτων, το πεδίο εφαρμογής είναι το πλήρως προσδιορισμένο όνομα τομέα (FQDN), όπως `https://<yournamespace>.servicebus.windows.net/`.

Τα διαθέσιμα δικαιώματα για μια πολιτική είναι σε μεγάλο βαθμό αυτονόητες:

  + Αποστολή
  + Ακρόαση
  + Διαχείριση

Αφού δημιουργήσετε την πολιτική, εκχωρείται ένα *Πρωτεύον κλειδί* και ένα *Δευτερεύον κλειδί*. Αυτά είναι τα πλήκτρα με κρυπτογράφηση ισχυρό. Μην χάσετε τους ή προκαλέσει απώλεια τους - αυτά θα είναι πάντα διαθέσιμη στην [πύλη του Azure][]. Μπορείτε να χρησιμοποιήσετε ένα από τα πλήκτρα που έχει δημιουργηθεί και μπορείτε να αναδημιουργήσετε τους οποιαδήποτε στιγμή. Ωστόσο, αν αναδημιουργήσετε ή αλλαγή του πρωτεύοντος κλειδιού στην πολιτική, θα ακυρωθούν δημιουργήθηκε από το τις υπογραφές πρόσβασης σε κοινή χρήση.

Όταν δημιουργείτε ένα χώρο ονομάτων Bus υπηρεσίας, μια πολιτική δημιουργείται αυτόματα για ολόκληρα τα πεδία ονομάτων που ονομάζεται **RootManageSharedAccessKey**και αυτή την πολιτική περιλαμβάνει όλα τα δικαιώματα. Δεν μπορείτε να συνδεθείτε ως **ριζική**, ώστε να μην χρησιμοποιήσετε αυτή την πολιτική, εκτός εάν υπάρχει μια πραγματικά καλός λόγος. Μπορείτε να δημιουργήσετε επιπλέον πολιτικές στην καρτέλα **Ρύθμιση παραμέτρων** για το χώρο ονομάτων στην πύλη. Είναι σημαντικό να σημείωση ότι μία δέντρου επίπεδο στην υπηρεσία Bus (χώρο ονομάτων, ουρά, ενότητα συμβάντων, κ.λπ.) μπορεί να έχει μόνο έως και 12 πολιτικές που έχουν επισυναφθεί σε αυτό.

## <a name="shared-access-signature-token"></a>Κοινόχρηστο Access υπογραφή (διακριτικό)

Η πολιτική δεν είναι το διακριτικό πρόσβασης για Bus υπηρεσίας. Είναι το αντικείμενο από την οποία δημιουργείται το διακριτικό πρόσβασης - χρησιμοποιώντας είτε το πρωτεύον ή δευτερεύον κλειδί. Το διακριτικό δημιουργείται δημιουργώντας προσεκτικά μια συμβολοσειρά με την εξής μορφή:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Όπου `signature-string` είναι ο κατακερματισμός SHA-256 της εμβέλειας του διακριτικού (**εύρος** όπως περιγράφεται στην προηγούμενη ενότητα) με ένα CRLF προσαρτημένο και την ώρα λήξης (σε δευτερόλεπτα από τη χρονική σήμανση: `00:00:00 UTC` την 1η Ιανουαρίου 1970).

Ο κατακερματισμός μοιάζει με τον ακόλουθο κώδικα ψευδο και επιστρέφει 32 byte.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Οι τιμές μη κατακερματίζεται είναι στη συμβολοσειρά **SharedAccessSignature** έτσι, ώστε ο παραλήπτης μπορεί να υπολογιστεί ο κατακερματισμός με τις ίδιες παραμέτρους, για να βεβαιωθείτε ότι επιστρέφει το ίδιο αποτέλεσμα. Το URI Καθορίζει την εμβέλεια και το όνομα του κλειδιού προσδιορίζει την πολιτική που θα χρησιμοποιηθεί για τον υπολογισμό ο κατακερματισμός. Αυτό είναι σημαντικό από την πλευρά του ασφαλείας. Εάν η υπογραφή δεν ταιριάζουν με αυτό το οποίο υπολογίζει τον παραλήπτη (υπηρεσία Bus), στη συνέχεια, δεν επιτρέπεται η πρόσβαση. Σε αυτό το σημείο μπορείτε να βεβαιωθείτε ότι ο αποστολέας είχαν πρόσβαση στο κλειδί και θα πρέπει να εκχωρούνται τα δικαιώματα που καθορίζονται στην πολιτική.

## <a name="generating-a-signature-from-a-policy"></a>Δημιουργία υπογραφής από μια πολιτική

Πώς στην πραγματικότητα κάνετε αυτό στον κώδικα; Ας ρίξουμε μια ματιά σε μερικά από τα εξής.

### <a name="nodejs"></a>NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### <a name="java"></a>Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### <a name="php"></a>PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### <a name="c35"></a>C & #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="using-the-shared-access-signature-at-http-level"></a>Χρησιμοποιώντας την υπογραφή πρόσβασης σε κοινή χρήση (στο επίπεδο HTTP)
 
Τώρα που γνωρίζετε πώς μπορείτε να δημιουργήσετε θέσει σε κοινή χρήση υπογραφών πρόσβασης για οποιοδήποτε οντοτήτων Bus υπηρεσίας, είστε έτοιμοι για να εκτελέσετε μια ΔΗΜΟΣΊΕΥΣΗ HTTP:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
Να θυμάστε, αυτό λειτουργεί για όλα τα στοιχεία. Μπορείτε να δημιουργήσετε συσχετίσεις Ασφαλείας για μια ουρά, το θέμα, τη συνδρομή, διανομέα συμβάν ή μετάδοση. Εάν χρησιμοποιείτε ταυτότητας ανά publisher για διανομείς συμβάντος, μπορείτε απλώς να προσαρτήσετε `/publishers/< publisherid>`.

Εάν μπορείτε να δώσετε έναν αποστολέα ή έναν υπολογιστή-πελάτη ενός διακριτικού συσχετισμών Ασφαλείας, δεν διαθέτουν το κλειδί απευθείας και αυτές δεν είναι δυνατό να αντιστρέψετε ο κατακερματισμός για να την αποκτήσετε. Ως εκ τούτου, έχετε έλεγχο επάνω σε τι μπορούν να έχουν πρόσβαση και για πόσο χρόνο. Σημαντικό να θυμάστε είναι ότι εάν αλλάξετε το πρωτεύον κλειδί στην πολιτική, θα ακυρώνονται δημιουργήθηκε από το τις υπογραφές πρόσβασης σε κοινή χρήση.

## <a name="using-the-shared-access-signature-at-amqp-level"></a>Χρησιμοποιώντας την υπογραφή πρόσβασης σε κοινή χρήση (στο επίπεδο AMQP)

Στην προηγούμενη ενότητα, είδατε πώς μπορείτε να χρησιμοποιήσετε το διακριτικό συσχετισμών Ασφαλείας με μια αίτηση HTTP POST για την αποστολή δεδομένων σε Bus την υπηρεσία. Όπως γνωρίζετε, μπορείτε να αποκτήσετε πρόσβαση σε υπηρεσία Bus χρησιμοποιώντας τη σύνθετη μήνυμα ουράς πρωτόκολλο (AMQP) που είναι το πρωτόκολλο που προτιμάτε να χρησιμοποιήσετε για λόγους απόδοσης, σε πολλά σενάρια. Η χρήση διακριτικού συσχετισμών Ασφαλείας με AMQP περιγράφεται στο έγγραφο [AMQP Claim-Based ασφαλείας έκδοση 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) που βρίσκεται στο Πρόχειρο εργασία από 2013 αλλά καλά υποστηρίζεται από Azure σήμερα.

Πριν να ξεκινήσετε την αποστολή δεδομένων προς Bus υπηρεσία, πρέπει να στείλετε τον εκδότη το διακριτικό συσχετισμών Ασφαλείας μέσα σε ένα μήνυμα AMQP σε ευδιάκριτο AMQP κόμβο με το όνομα **$cbs** (μπορείτε να το δείτε ως ουρά "ειδική" χρησιμοποιούνται από την υπηρεσία για να αποκτήσετε και να επικυρώσετε όλα τα διακριτικά συσχετισμών Ασφαλείας). Το publisher πρέπει να καθορίσετε το πεδίο **τη** μέσα στο μήνυμα AMQP; Αυτό είναι ο κόμβος στο οποίο η υπηρεσία απαντήσεις για τον εκδότη με το αποτέλεσμα του διακριτικού επικύρωσης (μια απλή αίτηση/απάντηση μοτίβο μεταξύ του publisher και υπηρεσία). Αυτός ο κόμβος απάντηση δημιουργείται "στη διάρκεια της λειτουργίας," μιλούν πληροφορίες σχετικά με "δυναμική δημιουργία απομακρυσμένο κόμβο", όπως περιγράφεται από την προδιαγραφή AMQP 1.0. Μετά τον έλεγχο ότι το διακριτικό συσχετισμών Ασφαλείας δεν είναι έγκυρο, τον εκδότη να μετάβαση προς τα εμπρός και να ξεκινήσετε την αποστολή δεδομένων για την υπηρεσία.

Τα ακόλουθα βήματα δείχνουν πώς μπορείτε να στείλετε το διακριτικό συσχετισμών Ασφαλείας με χρήση της βιβλιοθήκης [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) πρωτόκολλο AMQP. Αυτό είναι χρήσιμο εάν δεν μπορείτε να χρησιμοποιήσετε την επίσημη SDK Bus υπηρεσία (για παράδειγμα στο WinRT, .net Framework συμπαγής, .net Framework μικρής κλίμακας και μονοφωνικό) ανάπτυξη C\#. Φυσικά, αυτή η βιβλιοθήκη είναι χρήσιμο για να σας βοηθήσει να κατανοήσετε τον τρόπο βάσει αξιώσεων ασφαλείας λειτουργεί στο επίπεδο AMQP, όπως περιγράφηκε τον τρόπο που λειτουργεί στο επίπεδο HTTP (με μια αίτηση HTTP POST και το διακριτικό συσχετισμών Ασφαλείας που στέλνεται μέσα στην κεφαλίδα "Έγκριση"). Εάν δεν χρειάζεστε όπως πολύ καλή γνώση AMQP, μπορείτε να χρησιμοποιήσετε την επίσημη SDK Bus υπηρεσίας με .net Framework εφαρμογές, οι οποίες θα κάνει για εσάς.

### <a name="c35"></a>C & #35;

```
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

Το `PutCbsToken()` μέθοδος λαμβάνει τη *σύνδεση* (AMQP σύνδεσης παρουσίας κλάσης σύμφωνα με τη [βιβλιοθήκη AMQP .NET Lite](https://github.com/Azure/amqpnetlite)) που αντιπροσωπεύει τη σύνδεση TCP με την υπηρεσία και την παράμετρο *sasToken* που είναι το διακριτικό συσχετισμών Ασφαλείας για να στείλετε. 

> [AZURE.NOTE] Είναι σημαντικό ότι η σύνδεση δημιουργείται με **SASL ελέγχου ταυτότητας μηχανισμό οριστεί σε ΕΞΩΤΕΡΙΚΈΣ** (και δεν το ΑΠΛΌ προεπιλεγμένη με το όνομα χρήστη και τον κωδικό πρόσβασης που χρησιμοποιήσατε όταν δεν χρειάζεστε για να στείλετε το διακριτικό συσχετισμών Ασφαλείας).

Στη συνέχεια, το publisher δημιουργεί δύο AMQP συνδέσεις για το διακριτικό συσχετισμών Ασφαλείας η αποστολή και λήψη της απάντησης (το αποτέλεσμα διακριτικού επικύρωσης) από την υπηρεσία.

Το μήνυμα AMQP περιέχει ένα σύνολο ιδιοτήτων και περισσότερες πληροφορίες από ένα απλό μήνυμα. Το διακριτικό συσχετισμών Ασφαλείας είναι το σώμα του μηνύματος (χρησιμοποιώντας την κατασκευή). Η ιδιότητα **"Τη"** έχει οριστεί για το όνομα του κόμβου για τη λήψη το αποτέλεσμα επικύρωσης στη σύνδεση ακουστικό (μπορείτε να αλλάξετε το όνομα εάν θέλετε, και θα δημιουργηθεί δυναμικά από την υπηρεσία). Η τελευταία τρία εφαρμογή/προσαρμοσμένες ιδιότητες χρησιμοποιούνται από την υπηρεσία για να υποδείξετε ότι το είδος της εργασίας που έχει να εκτελέσει. Όπως περιγράφεται από την προδιαγραφή πρόχειρη CBS, πρέπει να το **όνομα της λειτουργίας** ("Τοποθέτηση-το διακριτικό"), η **Πληκτρολογήστε διακριτικού** (σε αυτήν την περίπτωση, μια "servicebus.windows.net:sastoken") και το **"όνομα" του ακροατηρίου** για τον οποίο το διακριτικό ισχύει (ολόκληρο οντότητα).

Μετά την αποστολή το διακριτικό συσχετισμών Ασφαλείας στη σύνδεση αποστολέα, τον εκδότη πρέπει να διαβάσει την απάντηση στη σύνδεση δέκτη. Η απάντηση είναι ένα απλό μήνυμα AMQP με μια ιδιότητα εφαρμογή με το όνομα **"Κωδικός κατάστασης"** που μπορεί να περιέχει τις ίδιες τιμές με έναν κωδικό κατάστασης HTTP. 

## <a name="next-steps"></a>Επόμενα βήματα

Ανατρέξτε στο άρθρο [αναφορά υπηρεσίας Bus REST API](https://msdn.microsoft.com/library/azure/hh780717.aspx) για περισσότερες πληροφορίες σχετικά με το τι μπορείτε να κάνετε με αυτά τα διακριτικά συσχετισμών Ασφαλείας.

Για περισσότερες πληροφορίες σχετικά με τον έλεγχο ταυτότητας Bus υπηρεσίας, ανατρέξτε στο θέμα [Bus υπηρεσία ελέγχου ταυτότητας και εξουσιοδότησης](service-bus-authentication-and-authorization.md). 

Περισσότερα παραδείγματα των συσχετισμών Ασφαλείας στο C# και δέσμη ενεργειών Java είναι σε [αυτήν τη δημοσίευση ιστολογίου](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx).

[Πύλη του Azure]: https://portal.azure.com