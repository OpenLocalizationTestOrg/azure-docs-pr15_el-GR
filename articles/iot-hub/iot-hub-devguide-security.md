<properties
 pageTitle="Οδηγός για προγραμματιστές - έλεγχος της πρόσβασης σε διανομέα IoT | Microsoft Azure"
 description="Οδηγός για προγραμματιστές Azure IoT διανομέα - πώς μπορείτε να ελέγχετε την πρόσβαση σε διανομέα IoT και διαχείριση ασφαλείας"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="control-access-to-iot-hub"></a>Έλεγχος της πρόσβασης σε διανομέα IoT

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο περιγράφει τις επιλογές για την ασφάλεια των κέντρο IoT σας. Διανομέα IoT χρησιμοποιεί *δικαιώματα* για να εκχωρήσετε πρόσβαση σε κάθε ενότητα IoT τα τελικά σημεία. Δικαιώματα περιορίζει την πρόσβαση σε ένα διανομέα IoT που βασίζονται σε λειτουργίες.

Σε αυτό το άρθρο περιγράφει:

- Τα διαφορετικά δικαιώματα που μπορείτε να εκχωρήσετε σε μια εφαρμογή συσκευή ή παρασκηνίου για πρόσβαση στο κέντρο IoT σας.
- Η διαδικασία ελέγχου ταυτότητας και τα διακριτικά χρησιμοποιεί για να επαληθεύσετε τα δικαιώματα.
- Μάθετε πώς να εύρος διαπιστευτήρια για να περιορίσετε την πρόσβαση σε συγκεκριμένους πόρους.
- Υποστήριξη IoT διανομέα για πιστοποιητικά X.509.
- Μηχανισμοί ελέγχου ταυτότητας προσαρμοσμένη συσκευή που χρησιμοποιούν υπάρχοντα μητρώα ταυτότητας συσκευή ή συνδυασμούς ελέγχου ταυτότητας.

### <a name="when-to-use"></a>Πότε να χρησιμοποιήσετε

Πρέπει να έχετε τα κατάλληλα δικαιώματα για την πρόσβαση σε οποιαδήποτε από τα τελικά σημεία IoT διανομέα. Για παράδειγμα, μια συσκευή πρέπει να συμπεριλάβετε ένα διακριτικό που περιέχει διαπιστευτηρίων ασφαλείας μαζί με κάθε μήνυμα που στέλνει IoT διανομέα.

## <a name="access-control-and-permissions"></a>Έλεγχος πρόσβασης και δικαιώματα

Μπορείτε να εκχωρήσετε [δικαιώματα](#iot-hub-permissions) με τους εξής τρόπους:

* **Επίπεδο διανομέα πολιτικές πρόσβασης σε κοινή χρήση**. Πολιτικές κοινόχρηστη πρόσβαση μπορεί να εκχωρήσει οποιονδήποτε συνδυασμό [δικαιωμάτων](#iot-hub-permissions). Μπορείτε να ορίσετε πολιτικές στην [πύλη του Azure][lnk-management-portal], ή μέσω προγραμματισμού με τη χρήση της [υπηρεσίας παροχής πόρων διανομέα IoT ΥΠΌΛΟΙΠΑ APIs][lnk-resource-provider-apis]. Διανομέα που έχουν δημιουργηθεί πρόσφατα IoT περιλαμβάνει τις παρακάτω προεπιλεγμένες πολιτικές:

    - **iothubowner**: η πολιτική με όλα τα δικαιώματα.
    - **υπηρεσία**: πολιτική με το δικαίωμα ServiceConnect.
    - **συσκευή**: πολιτική με το δικαίωμα DeviceConnect.
    - **registryRead**: πολιτική με το δικαίωμα RegistryRead.
    - **registryReadWrite**: η πολιτική με δικαιώματα RegistryRead και RegistryWrite.


* **Διαπιστευτήρια ασφαλείας ανά συσκευή**. Κάθε διανομέας IoT περιέχει μια [συσκευή ταυτότητας μητρώου][lnk-identity-registry]. Για κάθε συσκευή σε αυτό το μητρώο, μπορείτε να ρυθμίσετε τα διαπιστευτήρια ασφαλείας που εκχωρείτε δικαιώματα **DeviceConnect** έχει ρυθμιστεί για την αντίστοιχη συσκευή τα τελικά σημεία.

Για παράδειγμα, σε μια τυπική λύση IoT:

- Το στοιχείο Διαχείριση συσκευών χρησιμοποιεί την πολιτική *registryReadWrite* .
- Το στοιχείο επεξεργαστής συμβάντων χρησιμοποιεί την πολιτική της *υπηρεσίας* .
- Το στοιχείο χρόνου εκτέλεσης συσκευή επιχειρηματικής λογικής χρησιμοποιεί την πολιτική της *υπηρεσίας* .
- Μεμονωμένες συσκευές συνδεθείτε χρησιμοποιώντας τα διαπιστευτήρια αποθηκεύονται στο μητρώο του διανομέα IoT ταυτότητα.

## <a name="authentication"></a>Έλεγχος ταυτότητας

Azure διανομέα IoT εκχωρεί πρόσβαση σε τελικά σημεία με επαλήθευση ενός διακριτικού σύμφωνα με τις πολιτικές κοινόχρηστη πρόσβαση και τα διαπιστευτήρια ασφαλείας συσκευής ταυτότητας μητρώου.

Διαπιστευτήρια ασφαλείας, όπως συμμετρικά κλειδιά, αποστέλλονται ποτέ μέσω σύρματος.

> [AZURE.NOTE] Η υπηρεσία παροχής πόρων διανομέα IoT Azure προστατεύεται μέσω Azure συνδρομή σας, όπως είναι όλες τις υπηρεσίες παροχής στη [Διαχείριση πόρων Azure][lnk-azure-resource-manager].

Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να δημιουργήσετε και να χρησιμοποιήσετε διακριτικά ασφαλείας, ανατρέξτε στο θέμα [κωδικοί ασφαλείας διανομέα IoT][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Πρωτόκολλο συγκεκριμένες απαιτήσεις

Κάθε υποστηριζόμενο πρωτόκολλο, όπως MQTT, AMQP και HTTP, μεταφέρει τα διακριτικά με διαφορετικούς τρόπους.

Όταν χρησιμοποιείτε MQTT, το πακέτο ΣΎΝΔΕΣΗ περιλαμβάνει το deviceId ως το ClientId, {iothubhostname} / {deviceId} στο πεδίο όνομα χρήστη και ενός διακριτικού συσχετισμών Ασφαλείας στο πεδίο Κωδικός πρόσβασης. {iothubhostname} πρέπει να είναι η πλήρης CName του διανομέα IoT (για παράδειγμα, contoso.azure-devices.net).

Κατά τη χρήση [AMQP][lnk-amqp], διανομέα IoT υποστηρίζει [ΑΠΛΌ SASL] [ lnk-sasl-plain] και [AMQP αξιώσεων--ασφάλειας βάσει][lnk-cbs].

Εάν χρησιμοποιείτε το AMQP αξιώσεων βάσει-ασφαλείας, το πρότυπο καθορίζει τον τρόπο για τη μετάδοση αυτά τα διακριτικά.

Για το ΑΠΛΌ SASL, μπορεί να είναι το **όνομα χρήστη** :

* `{policyName}@sas.root.{iothubName}`Εάν χρησιμοποιείτε διανομέα επιπέδου διακριτικά.
* `{deviceId}@sas.{iothubname}`Εάν χρησιμοποιείτε συσκευή εμβέλεια διακριτικά.

Και στις δύο περιπτώσεις, το πεδίο κωδικού πρόσβασης περιέχει το διακριτικό, όπως περιγράφεται στο [διανομέα IoT ασφαλείας διακριτικά][lnk-sas-tokens].

HTTP υλοποιεί ελέγχου ταυτότητας, συμπεριλαμβάνοντας έγκυρο διακριτικό στην κεφαλίδα αίτησης **εξουσιοδότησης** .

#### <a name="example"></a>Παράδειγμα

Όνομα χρήστη (DeviceId διάκριση πεζών-κεφαλαίων):`iothubname.azure-devices.net/DeviceId`

Κωδικός πρόσβασης (δημιουργία συσχετισμών Ασφαλείας με την Εξερεύνηση συσκευή):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] Το [Azure IoT διανομέα SDK] [ lnk-sdks] αυτόματη δημιουργία διακριτικά κατά τη σύνδεση με την υπηρεσία. Σε ορισμένες περιπτώσεις, η SDK δεν υποστηρίζουν όλα τα πρωτόκολλα ή όλες οι μέθοδοι ελέγχου ταυτότητας.

### <a name="special-considerations-for-sasl-plain"></a>Ειδικά ζητήματα για ΑΠΛΌ SASL

Όταν χρησιμοποιείτε ΑΠΛΌ SASL με AMQP, ένα πρόγραμμα-πελάτη τη σύνδεση με ένα διανομέα IoT να χρησιμοποιήσετε ένα μεμονωμένο διακριτικό για κάθε σύνδεση TCP. Όταν λήξει το διακριτικό, η σύνδεση TCP αποσυνδέει από την υπηρεσία και ενεργοποιεί επανασύνδεση. Αυτή η συμπεριφορά, ενώ δεν είναι προβληματικό για ένα στοιχείο παρασκηνίου εφαρμογής, είναι πολύ επιβλαβής για μια εφαρμογή πλευρά συσκευή για τους ακόλουθους λόγους:

*  Σύνδεση συνήθως πύλες εκ μέρους πολλές συσκευές. Όταν χρησιμοποιείτε το ΑΠΛΌ SASL, έχουν για να δημιουργήσετε μια σύνδεση TCP distinct για κάθε συσκευή τη σύνδεση με ένα διανομέα IoT. Αυτό το σενάριο σημαντικά αυξάνει την κατανάλωση ενέργειας και πόροι δικτύωσης και αυξάνεται η αδράνεια των κάθε σύνδεση της συσκευής.
* Περιορίζεται πόρων συσκευές επηρεάζονται αρνητικά από την αύξηση της χρήσης των πόρων για να συνδεθείτε ξανά μετά από κάθε διακριτικού λήξης.

## <a name="scope-hub-level-credentials"></a>Διαπιστευτήρια διανομέα επιπέδου εμβέλειας

Μπορείτε να εμβέλεια πολιτικών ασφάλειας σε επίπεδο διανομέα, δημιουργώντας διακριτικά με περιορισμένα πόρου URI. Για παράδειγμα, το τελικό σημείο για την αποστολή μηνυμάτων συσκευής στο cloud από μια συσκευή είναι **/devices/ {deviceId} / μηνύματα/συμβάντων**. Μπορείτε επίσης να χρησιμοποιήσετε μια πολιτική πρόσβασης κοινόχρηστου επιπέδου διανομέα με **DeviceConnect** δικαιώματα για την υπογραφή ενός διακριτικού των οποίων resourceURI είναι **/devices/ {deviceId}**. Αυτή η προσέγγιση δημιουργεί ένα διακριτικό που είναι μόνο μπορεί να χρησιμοποιηθεί για την αποστολή μηνυμάτων εκ μέρους συσκευή **deviceId**.

Αυτός ο μηχανισμός είναι παρόμοια με την [πολιτική εκδότη διανομείς συμβάν][lnk-event-hubs-publisher-policy], και σας επιτρέπει να υλοποιήσετε μεθόδους ελέγχου ταυτότητας προσαρμοσμένο.

## <a name="security-tokens"></a>Κωδικοί ασφαλείας

Ενότητα IoT χρησιμοποιεί διακριτικά ασφάλειας για τον έλεγχο ταυτότητας συσκευές και τις υπηρεσίες για να αποφύγετε την αποστολή πλήκτρα του καλωδίου. Επιπλέον, τα διακριτικά ασφαλείας περιορίζονται σε χρονική ισχύς και εύρος. [Azure SDK διανομέα IoT] [ lnk-sdks] αυτόματη δημιουργία διακριτικά χωρίς να απαιτείται οποιαδήποτε ειδική ρύθμιση παραμέτρων. Ορισμένα σενάρια, ωστόσο, απαιτούν από το χρήστη για να δημιουργήσετε και να χρησιμοποιήσετε απευθείας διακριτικά ασφαλείας. Αυτά περιλαμβάνουν την άμεση χρήση των επιφανειών MQTT, AMQP ή HTTP, ή για την εφαρμογή του μοτίβου διακριτικού υπηρεσία, όπως εξηγείται στο θέμα [Έλεγχος ταυτότητας συσκευή προσαρμοσμένη][lnk-custom-auth].

Ενότητα IoT επιτρέπει επίσης συσκευές για τον έλεγχο ταυτότητας με διανομέα IoT χρησιμοποιώντας [πιστοποιητικά X.509][lnk-x509]. 

### <a name="security-token-structure"></a>Δομή διακριτικού ασφαλείας
Μπορείτε να χρησιμοποιήσετε τα διακριτικά ασφαλείας για να εκχωρήσετε δεσμεύεται ώρα πρόσβαση σε συσκευές και υπηρεσίες σε συγκεκριμένες λειτουργίες στο IoT διανομέα. Για να εξασφαλίσετε ότι μόνο εξουσιοδοτημένοι συσκευές και υπηρεσίες να συνδεθείτε, τα διακριτικά ασφαλείας πρέπει να συνδεθείτε με έναν αριθμό-κλειδί πολιτικής κοινόχρηστη πρόσβαση ή μια συμμετρική κλειδί που είναι αποθηκευμένο με μια ταυτότητα συσκευής στο μητρώο ταυτότητα.

Ένα διακριτικό συνδεδεμένοι με μια πρόσβαση κλειδιού επιχορηγήσεις πολιτικής κοινόχρηστη πρόσβαση σε όλες τις λειτουργίες που σχετίζονται με τα δικαιώματα πολιτικής κοινόχρηστη πρόσβαση. Από την άλλη πλευρά, ενός διακριτικού συνδεθεί με αριθμό-κλειδί συμμετρική μιας συσκευής ταυτότητας εκχωρεί μόνο το δικαίωμα **DeviceConnect** για την ταυτότητα συσχετισμένη συσκευή.

Το διακριτικό ασφαλείας έχει την παρακάτω μορφή:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

Αυτές είναι οι αναμενόμενες τιμές:

| Τιμή | Περιγραφή |
| ----- | ----------- |
| {υπογραφή} | Μια συμβολοσειρά υπογραφή HMAC SHA256 της φόρμας: `{URL-encoded-resourceURI} + "\n" + expiry`. **ΣΗΜΑΝΤΙΚΟ**: τον αριθμό-κλειδί είναι αποκωδικοποιείται από base64 και να χρησιμοποιηθεί ως αριθμό-κλειδί για να εκτελέσετε τον υπολογισμό HMAC SHA256. |
| {resourceURI} | Πρόθεμα URI (από το τμήμα) τα τελικά σημεία με δυνατότητα πρόσβασης με αυτό το διακριτικό, ξεκινώντας με το όνομα κεντρικού υπολογιστή του την ενότητα IoT (χωρίς πρωτόκολλο). Για παράδειγμα,`myHub.azure-devices.net/devices/device1` |
| {Λήξη} | Συμβολοσειρές UTF8 για τον αριθμό των δευτερολέπτων μετά τη χρονική σήμανση UTC 00:00:00 την 1η Ιανουαρίου 1970. |
| {Διεύθυνση URL-κωδικοποιημένα-resourceURI} | Κωδικοποίηση URL του πόρου πεζούς URI στην υπόθεση κάτω |
| {Όνομα_πολιτικής} | Το όνομα της πολιτικής κοινόχρηστη πρόσβαση στην οποία αναφέρεται αυτό το διακριτικό. Δεν υπάρχει στην περίπτωση διακριτικά που αναφέρονται σε συσκευή μητρώου διαπιστευτήρια. |

**Σημείωση σχετικά με πρόθεμα**: το URI πρόθεμα υπολογίζεται από το τμήμα και όχι κατά ένα χαρακτήρα. Για παράδειγμα `/a/b` είναι ένα πρόθεμα για `/a/b/c` , αλλά όχι για `/a/bc`.

Το παρακάτω τμήμα κώδικα Node.js εμφανίζει μια συνάρτηση που ονομάζεται **generateSasToken** , η οποία υπολογίζει το διακριτικό από τα δεδομένα εισόδου `resourceUri, signingKey, policyName, expiresInMins`. Οι επόμενες ενότητες με λεπτομέρειες πώς να προετοιμάσετε το διαφορετικό εισόδων για τις περιπτώσεις διαφορετικές διακριτικού χρήση.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Ως μια σύγκριση, τον αντίστοιχο κώδικα Python για τη δημιουργία ενός διακριτικού ασφαλείας είναι:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] Εφόσον η χρονική ισχύς του διακριτικού επικύρωση σε υπολογιστές IoT διανομέα, είναι σημαντικό ότι η μετατόπιση στο ρολόι του υπολογιστή που δημιουργεί το διακριτικό είναι ελάχιστο.

### <a name="use-sas-tokens-in-a-device-client"></a>Χρησιμοποιήστε τα διακριτικά συσχετισμών Ασφαλείας σε ένα πρόγραμμα-πελάτη συσκευή

Υπάρχουν δύο τρόποι για να αποκτήσετε δικαιώματα **DeviceConnect** με διανομέα IoT με ασφάλεια τα διακριτικά: Χρησιμοποιήστε ένα [κλειδί συμμετρική συσκευής από το μητρώο ταυτότητας συσκευή](#use-a-symmetric-key-in-the-identity-registry)ή χρησιμοποιήστε ένα [κλειδί πολιτικής κοινόχρηστη πρόσβαση](#use-a-shared-access-policy).

Να θυμάστε ότι όλες τις λειτουργίες προσβάσιμες από συσκευές εκτίθεται από σχέδιο σε τελικά σημεία με πρόθεμα `/devices/{deviceId}`.

> [AZURE.IMPORTANT] Ο μόνος τρόπος ότι διανομέα IoT πραγματοποιεί έλεγχο ταυτότητας μια συγκεκριμένη συσκευή χρησιμοποιεί το κλειδί συμμετρική ταυτότητα συσκευής. Στις περιπτώσεις κατά τη χρήση μιας πολιτικής κοινόχρηστη πρόσβαση για να αποκτήσετε πρόσβαση σε λειτουργία συσκευής, της λύσης πρέπει να εξετάσετε το στοιχείο έκδοσης του διακριτικού ασφαλείας ως ένα δευτερεύον στοιχείο αξιόπιστη.

Τα τελικά σημεία άμεσα προσβάσιμη από τη συσκευή είναι (ανεξάρτητα από το πρωτόκολλο):

| Τελικό σημείο | Λειτουργίες |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Αποστολή μηνυμάτων συσκευής στο cloud. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Λαμβάνετε μηνύματα cloud-σε-συσκευή. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Χρήση συμμετρική αριθμού-κλειδιού από το μητρώο ταυτότητας

Κατά τη χρήση μιας συσκευής ταυτότητας συμμετρική κλειδί για τη δημιουργία ενός διακριτικού το Όνομα_πολιτικής (`skn`) παραλείπεται το στοιχείο του διακριτικού.

Για παράδειγμα, ενός διακριτικού δημιουργήθηκε για να αποκτήσετε πρόσβαση σε όλες τις λειτουργίες συσκευή θα πρέπει να έχετε τις εξής παραμέτρους:

* URI πόρου: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* κλειδί υπογραφής: οποιοδήποτε συμμετρική πλήκτρο για την `{device id}` ταυτότητα,
* δεν υπάρχει όνομα πολιτικής,
* οποιαδήποτε ώρα λήξης.

Χρήση της συνάρτησης κόμβο παραπάνω παράδειγμα πρέπει να είναι:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Το αποτέλεσμα, η οποία παρέχει πρόσβαση σε όλες τις λειτουργίες για device1, πρέπει να είναι:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] Είναι δυνατή η δημιουργία ενός διακριτικού ασφαλούς χρησιμοποιώντας το εργαλείο .NET [Explorer συσκευή][lnk-device-explorer].

### <a name="use-a-shared-access-policy"></a>Χρησιμοποιήστε μια πολιτική κοινόχρηστη πρόσβαση

Κατά τη δημιουργία ενός διακριτικού από μια πολιτική κοινόχρηστο πρόσβασης, η πολιτική όνομα πεδίου `skn` πρέπει να οριστεί στο όνομα της πολιτικής χρησιμοποιούνται. Επίσης, είναι απαραίτητη ότι η πολιτική εκχωρεί το δικαίωμα **DeviceConnect** .

Τα δύο σενάρια κύριο για τη χρήση των πολιτικών κοινόχρηστη πρόσβαση για να αποκτήσετε πρόσβαση σε λειτουργία συσκευής είναι:

* [στο cloud πύλες πρωτοκόλλου][lnk-endpoints],
* [διακριτικό υπηρεσίες] [ lnk-custom-auth] χρησιμοποιείται για την εφαρμογή συνδυασμούς προσαρμοσμένου ελέγχου ταυτότητας.

Επειδή η πολιτική κοινόχρηστη πρόσβαση ενδεχομένως να εκχωρήσετε πρόσβαση για να συνδεθείτε ως οποιαδήποτε συσκευή, είναι σημαντικό να χρησιμοποιήσετε το σωστό URI πόρου, όταν δημιουργείτε κωδικοί ασφαλείας. Αυτό είναι ιδιαίτερα σημαντικό για διακριτικού υπηρεσίες, που πρέπει να εύρος το διακριτικό για συγκεκριμένες συσκευές που χρησιμοποιούν το URI πόρου. Αυτό το σημείο είναι λιγότερο σχετικά για πύλες πρωτοκόλλου όπως αυτά ήδη μεσολάβησης κίνηση για όλες τις συσκευές.

Ως παράδειγμα, μια υπηρεσία διακριτικού χρησιμοποιώντας την πολιτική δημιουργηθεί εκ των προτέρων κοινόχρηστη πρόσβαση ονομάζεται **συσκευή** θα δημιουργήσετε ενός διακριτικού με τις ακόλουθες παραμέτρους:

* URI πόρου: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* κλειδί υπογραφής: ένα από τα πλήκτρα με τα `device` πολιτικής,
* Όνομα πολιτικής: `device`,
* οποιαδήποτε ώρα λήξης.

Χρήση της συνάρτησης κόμβο παραπάνω παράδειγμα πρέπει να είναι:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Το αποτέλεσμα, η οποία παρέχει πρόσβαση σε όλες τις λειτουργίες για device1, πρέπει να είναι:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Μια πύλη πρωτόκολλο μπορούν να χρησιμοποιήσουν το ίδιο διακριτικό για όλες τις συσκευές απλώς τη ρύθμιση URI πόρου σε `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Χρησιμοποιήστε ασφάλεια διακριτικά από στοιχεία της υπηρεσίας

Στοιχεία της υπηρεσίας μπορεί να δημιουργήσει μόνο κώδικες ασφαλείας, χρησιμοποιώντας πολιτικές κοινόχρηστη πρόσβαση εκχώρηση τα κατάλληλα δικαιώματα, όπως εξηγείται προηγουμένως.

Αυτές είναι οι συναρτήσεις υπηρεσίας που εκτίθενται στο τα τελικά σημεία:

| Τελικό σημείο | Λειτουργίες |
| ----- | ----------- |
| `{iot hub host name}/devices` | Δημιουργία, ενημέρωση, ανάκτηση και διαγραφή ταυτότητες συσκευή. |
| `{iot hub host name}/messages/events` | Λαμβάνετε μηνύματα συσκευής στο cloud. |
| `{iot hub host name}/servicebound/feedback` | Συγκεντρώστε σχόλια για τα μηνύματα cloud-σε-συσκευή. |
| `{iot hub host name}/devicebound` | Αποστολή μηνυμάτων cloud-σε-συσκευή. |

Ως παράδειγμα, οι δημιουργίας μιας υπηρεσίας χρησιμοποιώντας την πολιτική δημιουργηθεί εκ των προτέρων κοινόχρηστη πρόσβαση που ονομάζεται **registryRead** θα δημιουργήσετε ενός διακριτικού με τις ακόλουθες παραμέτρους:

* URI πόρου: `{IoT hub name}.azure-devices.net/devices`,
* κλειδί υπογραφής: ένα από τα πλήκτρα με τα `registryRead` πολιτικής,
* Όνομα πολιτικής: `registryRead`,
* οποιαδήποτε ώρα λήξης.

    τελικό σημείο VAR = "myhub.azure-devices.net/devices";   VAR Όνομα_πολιτικής = "συσκευή"   VAR policyKey = '...';

    VAR διακριτικό = generateSasToken (τελικό σημείο, policyKey, Όνομα_πολιτικής, 60);

Το αποτέλεσμα, το οποίο θέλετε να εκχωρήσετε πρόσβαση για να διαβάσετε όλες τις ταυτότητες συσκευή, πρέπει να είναι:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Υποστηριζόμενες πιστοποιητικά X.509

Μπορείτε να χρησιμοποιήσετε οποιοδήποτε πιστοποιητικό X.509 για τον έλεγχο ταυτότητας μια συσκευή με το Κέντρο IoT. Αυτό περιλαμβάνει:

-   **Ένα υπάρχον πιστοποιητικό X.509**. Μια συσκευή μπορεί να έχετε ήδη ένα πιστοποιητικό X.509 που σχετίζονται με αυτό. Η συσκευή να χρησιμοποιήσετε αυτό το πιστοποιητικό για τον έλεγχο ταυτότητας με IoT διανομέα.

-   **Ένα αυτο-δημιουργημένες και αυτο-υπογεγραμμένο πιστοποιητικό X-509**. Μια κατασκευαστή της συσκευής ή deployer εντός της εταιρείας μπορεί να δημιουργήσει αυτά τα πιστοποιητικά και να αποθηκεύσετε το αντίστοιχο ιδιωτικό κλειδί (και το πιστοποιητικό) στη συσκευή. Μπορείτε να χρησιμοποιήσετε εργαλεία όπως το [OpenSSL] [ lnk-openssl] και [Windows SelfSignedCertificate] [ lnk-selfsigned] βοηθητικού προγράμματος για το σκοπό.

-   **Πιστοποιητικό X.509 υπογεγραμμένο αρχή έκδοσης Πιστοποιητικών**. Μπορείτε επίσης να χρησιμοποιήσετε ένα πιστοποιητικό X.509 που δημιουργούνται και υπογραφή από μια αρχή έκδοσης πιστοποιητικών (CA) για να προσδιορίσετε μια συσκευή και τον έλεγχο ταυτότητας μια συσκευή με το Κέντρο IoT.

Μια συσκευή μπορεί να χρησιμοποιήσετε ένα πιστοποιητικό X.509 ή έναν κωδικό ασφαλείας για τον έλεγχο ταυτότητας, αλλά όχι και τις δύο.

### <a name="register-an-x509-client-certificate-for-a-device"></a>Καταχωρήσετε ένα πιστοποιητικό X.509 προγράμματος-πελάτη για μια συσκευή

Το [Azure IoT υπηρεσίας SDK για C#] [ lnk-service-sdk] (έκδοση 1.0.8+) υποστηρίζει την καταχώρηση μιας συσκευής που χρησιμοποιεί ένα πιστοποιητικό X.509 προγράμματος-πελάτη για έλεγχο ταυτότητας. Άλλες APIs όπως εισαγωγή/εξαγωγή από συσκευές υποστηρίζουν επίσης πιστοποιητικά X.509 προγράμματος-πελάτη.

### <a name="c-support"></a>C\# υποστήριξης

Η κλάση **RegistryManager** παρέχει έναν τρόπο προγραμματισμού για να καταχωρήσετε μια συσκευή. Συγκεκριμένα, οι μέθοδοι **AddDeviceAsync** και **UpdateDeviceAsync** ενεργοποίηση χρήστη για να καταχωρήσετε και να ενημερώσετε μια συσκευή στο μητρώο διανομέα Iot συσκευή ταυτότητα. Αυτές τις δύο μεθόδους λαμβάνουν μια παρουσία **συσκευής** ως είσοδο. Η κλάση της **συσκευής** περιλαμβάνει μια ιδιότητα **ελέγχου ταυτότητας** που επιτρέπει στο χρήστη να καθορίσει κύριας και δευτερεύουσας thumbprints πιστοποιητικό X.509. Την αποτύπωση αντιπροσωπεύει Κατακερματισμός SHA-1 από το πιστοποιητικό X.509 (αποθηκευμένη χρήση δυαδικό κωδικοποίησης DER). Οι χρήστες διαθέτουν την επιλογή να καθορίσετε μια κύρια αποτύπωση ή μια δευτερεύουσα αποτύπωση ή και τα δύο. Για να χειριστείτε σενάρια κατάδειξης πιστοποιητικό υποστηρίζονται κύριας και δευτερεύουσας thumbprints.

> [AZURE.NOTE] Ενότητα IoT δεν το απαιτεί και αποθήκευση ολόκληρου X.509 πιστοποιητικό-πελάτη, μόνο την αποτύπωση.

Ακολουθεί ένα δείγμα C\# τμήμα κώδικα για να καταχωρήσετε μια συσκευή χρησιμοποιώντας ένα πιστοποιητικό X.509 υπολογιστή-πελάτη:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>Χρησιμοποιήστε ένα πιστοποιητικό X.509 υπολογιστή-πελάτη κατά τη διάρκεια του χρόνου εκτέλεσης λειτουργιών

Η [συσκευή Azure IoT SDK για .NET] [ lnk-client-sdk] (έκδοση 1.0.11+) υποστηρίζει τη χρήση των πιστοποιητικών X.509 προγράμματος-πελάτη.

### <a name="c-support"></a>C\# υποστήριξης

Η κλάση **DeviceAuthenticationWithX509Certificate** υποστηρίζει τη δημιουργία των παρουσιών  **DeviceClient** χρησιμοποιώντας ένα πιστοποιητικό X.509 υπολογιστή-πελάτη.

Ακολουθεί ένα δείγμα κώδικα:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Έλεγχος ταυτότητας προσαρμοσμένη συσκευή

Μπορείτε να χρησιμοποιήσετε το διανομέα IoT [μητρώου ταυτότητα συσκευής] [ lnk-identity-registry] για ρύθμιση παραμέτρων διαπιστευτηρίων ασφαλείας ανά συσκευή και αποκτήστε πρόσβαση στο στοιχείο ελέγχου, χρησιμοποιώντας [τα διακριτικά][lnk-sas-tokens]. Ωστόσο, εάν μια λύση IoT έχει ήδη μια σημαντική επένδυσης σε ένα συνδυασμό προσαρμοσμένη συσκευή ταυτότητας μητρώου ή/και τον έλεγχο ταυτότητας, μπορείτε να ενοποιήσετε αυτήν την υπάρχουσα υποδομή με IoT διανομέα, δημιουργώντας μια *υπηρεσία διακριτικού*. Με αυτόν τον τρόπο, μπορείτε να χρησιμοποιήσετε άλλες δυνατότητες IoT στη λύση σας.

Η υπηρεσία διακριτικού είναι μια υπηρεσία cloud προσαρμοσμένο. Χρησιμοποιεί ένα διανομέα IoT *πολιτικής κοινόχρηστη πρόσβαση* με δικαιώματα **DeviceConnect** για να δημιουργήσετε διακριτικά *εμβέλεια συσκευή* . Αυτά τα διακριτικά ενεργοποιήσετε μια συσκευή για να συνδεθείτε με το Κέντρο IoT σας.

  ![Τα βήματα του μοτίβου υπηρεσία διακριτικού][img-tokenservice]

Αυτά είναι τα κύρια βήματα του μοτίβου διακριτικού υπηρεσίας:

1. Δημιουργήστε μια πολιτική πρόσβασης διανομέα IoT θέσει σε κοινή χρήση με **DeviceConnect** δικαιώματα για το Κέντρο IoT σας. Μπορείτε να δημιουργήσετε αυτή την πολιτική στην [πύλη του Azure] [ lnk-management-portal] ή μέσω προγραμματισμού. Η υπηρεσία διακριτικού χρησιμοποιεί αυτήν την πολιτική για να πραγματοποιήσετε τα διακριτικά που δημιουργεί.
2. Όταν μια συσκευή χρειάζεται για πρόσβαση στο κέντρο σας IoT, αιτήσεις ενός υπογεγραμμένου διακριτικού από την υπηρεσία διακριτικού. Η συσκευή να τον έλεγχο ταυτότητας με το συνδυασμό προσαρμοσμένη συσκευή ταυτότητας μητρώου/ελέγχου ταυτότητας για να προσδιορίσετε την ταυτότητα της συσκευής που χρησιμοποιεί την υπηρεσία διακριτικού για να δημιουργήσετε το διακριτικό.
3. Η υπηρεσία διακριτικού επιστρέφει ένα διακριτικό. Το διακριτικό έχει δημιουργηθεί με τη χρήση `/devices/{deviceId}` ως `resourceURI`, με `deviceId` ως τη συσκευή που έχει τεθεί σε έλεγχο ταυτότητας. Η υπηρεσία διακριτικού χρησιμοποιεί την πολιτική κοινόχρηστη πρόσβαση για να δημιουργήσετε το διακριτικό.
4. Η συσκευή χρησιμοποιεί το διακριτικό απευθείας με την ενότητα IoT.

> [AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε την κλάση .NET [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] ή της κλάσης Java [IotHubServiceSasToken] [ lnk-java-sas] για τη δημιουργία ενός διακριτικού στην υπηρεσία διακριτικού.

Η υπηρεσία διακριτικού να ορίσετε τη λήξη διακριτικού όπως θέλετε. Όταν λήξει το διακριτικό, την ενότητα IoT διακόψει τη σύνδεση της συσκευής. Στη συνέχεια, η συσκευή πρέπει να ζητήσετε έναν νέο κωδικό από την υπηρεσία διακριτικού. Εάν χρησιμοποιείτε μια ώρα λήξης σύντομο, αυξάνει το φορτίο και τη συσκευή και την υπηρεσία διακριτικού.

Για μια συσκευή για να συνδεθείτε με το Κέντρο σας, πρέπει να εξακολουθεί να προσθέτετε το μητρώο διανομέα IoT συσκευή ταυτότητας — ακόμη και αν η συσκευή χρησιμοποιεί ένα διακριτικό και όχι ένα κλειδί συσκευή για να συνδεθείτε. Γι ' αυτό, μπορείτε να συνεχίσετε να χρησιμοποιείτε έλεγχος πρόσβασης ανά συσκευή με δυνατότητα ενεργοποίησης ή απενεργοποίησης ταυτότητες συσκευής στο [μητρώο ταυτότητας διανομέα IoT] [ lnk-identity-registry] όταν η συσκευή πραγματοποιεί έλεγχο ταυτότητας με διακριτικό. Αυτό mitigates τους κινδύνους που ενέχει χρησιμοποιώντας τα διακριτικά με το χρόνο λήξης ώρες.

### <a name="comparison-with-a-custom-gateway"></a>Σύγκριση με μια προσαρμοσμένη πύλη

Το μοτίβο υπηρεσία διακριτικού είναι το συνιστώμενο τρόπο για την υλοποίηση ενός συνδυασμού μητρώου/ελέγχου ταυτότητας προσαρμοσμένης ταυτότητας με IoT διανομέα. Συνιστάται να επειδή διανομέα IoT εξακολουθεί να χειριστείτε μεγαλύτερο μέρος της κυκλοφορίας λύση. Ωστόσο, υπάρχουν περιπτώσεις όπου το σχήμα προσαρμοσμένου ελέγχου ταυτότητας επομένως intertwined με το πρωτόκολλο ότι απαιτείται μια υπηρεσία επεξεργασίας όλη την κυκλοφορία (*προσαρμοσμένες πύλης*). Παράδειγμα αυτή είναι η [ασφάλεια επιπέδου μεταφοράς (TLS) και ήδη κοινόχρηστα κλειδιά (PSKs)][lnk-tls-psk]. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα της [πύλης πρωτόκολλο] [ lnk-protocols] το θέμα.

## <a name="reference-topics"></a>Αναφορά θέματα:

Τα παρακάτω θέματα αναφοράς που παρέχουν περισσότερες πληροφορίες σχετικά με τον έλεγχο πρόσβασης για το Κέντρο IoT σας.

## <a name="iot-hub-permissions"></a>Δικαιώματα IoT διανομέα

Ο παρακάτω πίνακας παραθέτει τα δικαιώματα που μπορείτε να χρησιμοποιήσετε για τον έλεγχο της πρόσβασης στο κέντρο IoT σας.

| Δικαιωμάτων            | Σημειώσεις |
| --------------------- | ----- |
| **RegistryRead**      | Πρόσβαση στο μητρώο ταυτότητας συσκευή ανάγνωσης επιχορηγήσεις. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [μητρώου ταυτότητα συσκευής][lnk-identity-registry]. |
| **RegistryReadWrite** | Επιχορηγήσεις πρόσβαση ανάγνωσης και εγγραφής στο μητρώο ταυτότητας συσκευή. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [μητρώου ταυτότητα συσκευής][lnk-identity-registry]. |
| **ServiceConnect**    | Επιχορηγήσεις πρόσβαση στο cloud υπηρεσίας τοποθεσία επικοινωνίας και παρακολούθηση τελικά σημεία. Για παράδειγμα, εκχωρεί δικαιώματα για τις υπηρεσίες cloud παρασκηνίου για να λαμβάνετε μηνύματα συσκευής στο cloud και αποστολή μηνυμάτων cloud-σε-συσκευή ανακτήσετε την αντίστοιχη επιβεβαιώσεις παράδοσης. |
| **DeviceConnect**     | Πρόσβαση επιχορηγήσεις τελικά σημεία επικοινωνίας άμεσα προσβάσιμη από τη συσκευή. Για παράδειγμα, εκχωρεί δικαιώματα για αποστολή μηνυμάτων συσκευής στο cloud και λήψη μηνυμάτων cloud-σε-συσκευή. Αυτό το δικαίωμα χρησιμοποιείται από συσκευές. |

## <a name="additional-reference-material"></a>Υλικό επιπλέον αναφοράς

Άλλα θέματα αναφοράς στον οδηγό για προγραμματιστές περιλαμβάνουν τα εξής:

- [Τα τελικά σημεία διανομέα IoT] [ lnk-endpoints] περιγράφει τα διάφορα τελικά σημεία που κάθε ενότητα IoT εκθέτει για διαχείριση χρόνου εκτέλεσης και λειτουργίες.
- [Περιορισμού και του ορίου] [ lnk-quotas] περιγράφει τα όρια που ισχύουν για την υπηρεσία IoT διανομέας και τη συμπεριφορά επιτάχυνσης να περιμένει όταν χρησιμοποιείτε την υπηρεσία.
- [SDK συσκευής και της υπηρεσίας διανομέα IoT] [ lnk-sdks] παραθέτει τα διάφορα γλώσσας SDK που μη χρήση κατά την ανάπτυξη και συσκευή και υπηρεσία εφαρμογές που αλληλεπιδρούν με IoT διανομέα.
- [Το ερώτημα γλώσσα για twins, μεθόδους και εργασίες] [ lnk-query] περιγράφει τη γλώσσα ερωτήματος που μπορείτε να χρησιμοποιήσετε για να ανακτήσετε πληροφορίες από διανομέα IoT σχετικά με τη συσκευή twins, μεθόδους και εργασίες.
- [Υποστήριξη IoT διανομέα MQTT] [ lnk-devguide-mqtt] παρέχει περισσότερες πληροφορίες σχετικά με την υποστήριξη IoT διανομέα για το πρωτόκολλο MQTT.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα έχετε μάθει πώς μπορείτε να ελέγχετε την πρόσβαση IoT διανομέα, μπορεί να σας ενδιαφέρουν τα παρακάτω θέματα Οδηγός για προγραμματιστές:

- [Χρησιμοποιήστε twins συσκευή για να συγχρονίσετε κατάσταση και ρυθμίσεις παραμέτρων][lnk-devguide-device-twins]
- [Κλήση μιας μεθόδου απευθείας σε μια συσκευή][lnk-devguide-directmethods]
- [Προγραμματισμός εργασιών σε πολλές συσκευές][lnk-devguide-jobs]

Εάν θέλετε να δοκιμάσετε μερικά από τα θέματα που περιγράφονται σε αυτό το άρθρο, ενδέχεται να σας ενδιαφέρουν τα παρακάτω προγράμματα εκμάθησης IoT διανομέα:

- [Γρήγορα αποτελέσματα με το Κέντρο IoT Azure][lnk-getstarted-tutorial]
- [Πώς μπορείτε να στέλνετε μηνύματα cloud-σε-συσκευή με το Κέντρο IoT][lnk-c2d-tutorial]
- [Πώς μπορείτε να επεξεργαστείτε διανομέα IoT συσκευή-σε-cloud μηνύματα][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md