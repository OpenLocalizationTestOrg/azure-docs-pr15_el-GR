# <a name="securing-your-iot-deployment"></a>Διασφάλιση της ανάπτυξης IoT

Αυτό το άρθρο παρέχει το επόμενο επίπεδο λεπτομερειών για τη διασφάλιση της υποδομής βασίζεται σε Azure IoT Internet του πράγματα (IoT). Συνδέεται με την εφαρμογή επίπεδο λεπτομερειών για τη ρύθμιση παραμέτρων και την ανάπτυξη σε κάθε στοιχείο. Παρέχει επίσης συγκρίσεις και επιλογές μεταξύ των διαφόρων μεθόδων ανταγωνιστικών.

Διασφάλιση της ανάπτυξης Azure IoT μπορεί να διαιρεθεί σε οι ακόλουθοι χώροι ασφαλείας τρεις:

- **Συσκευή ασφαλείας**: προστασία της συσκευής IoT ενώ είναι αναπτυγμένο από το φυσικό περιβάλλον.
- **Ασφάλεια σύνδεσης**: εξασφαλίζει όλα τα δεδομένα που μεταδίδονται μεταξύ της συσκευής IoT και IoT διανομέα είναι εμπιστευτικές και απαραβίαστος.
- **Σύννεφο ασφαλείας**: παροχή ενός μέσου για την προστασία των δεδομένων, ενώ μετακινείται μέσω και αποθηκεύονται στο σύννεφο.

![Τρεις περιοχές ασφαλείας][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Ασφαλής συσκευή προμήθεια και τον έλεγχο ταυτότητας

Η οικογένεια προγραμμάτων IoT Azure ασφαλίζει IoT συσκευές από τις ακόλουθες δύο μεθόδους:

- Παρέχοντας ένα κλειδί μοναδική ταυτότητα (κωδικοί ασφαλείας) για κάθε συσκευή, η οποία μπορεί να χρησιμοποιηθεί από τη συσκευή για την επικοινωνία με το διανομέα IoT.

- Χρησιμοποιώντας ένα στη συσκευή [πιστοποιητικό X.509] [ lnk-x509] και το ιδιωτικό κλειδί ως μέσο για τον έλεγχο ταυτότητας της συσκευής με το διανομέα IoT. Αυτή η μέθοδος ελέγχου ταυτότητας, εξασφαλίζει ότι το ιδιωτικό κλειδί της συσκευής δεν είναι γνωστός έξω από τη συσκευή ανά πάσα στιγμή, παρέχοντας υψηλότερο επίπεδο ασφάλειας.

Η μέθοδος διακριτικού ασφαλείας παρέχει έλεγχο ταυτότητας για κάθε κλήση που γίνεται από τη συσκευή με διανομέα IoT συσχετίζοντας το συμμετρικό κλειδί σε κάθε κλήση. Έλεγχος ταυτότητας που βασίζεται σε X.509 επιτρέπει τον έλεγχο ταυτότητας μιας συσκευής IoT στο φυσικό επίπεδο ως μέρος της εγκατάστασης σύνδεσης TLS. Η μέθοδος με το διακριτικό ασφαλείας μπορεί να χρησιμοποιηθεί χωρίς τον έλεγχο ταυτότητας X.509 που είναι ένα μοτίβο λιγότερο ασφαλής. Η επιλογή μεταξύ των δύο μεθόδων είναι κυρίως υπαγορεύεται από πόσο ασφαλή τη συσκευή ελέγχου ταυτότητας πρέπει να είναι, και διαθεσιμότητα της ασφαλούς αποθήκευσης στη συσκευή (για να αποθηκεύσετε με ασφάλεια το ιδιωτικό κλειδί).

## <a name="iot-hub-security-tokens"></a>Κωδικοί ασφαλείας διανομέα IoT

Διανομέας IoT χρησιμοποιεί διακριτικά ασφάλειας για τον έλεγχο ταυτότητας συσκευών και υπηρεσιών για να αποφευχθούν τα κλειδιά στο δίκτυο. Επιπλέον, οι κωδικοί ασφαλείας περιορίζονται με χρονική ισχύς και το εύρος. SDK διανομέα IoT Azure αυτόματη δημιουργία διακριτικά χωρίς να απαιτείται οποιαδήποτε ειδική ρύθμιση παραμέτρων. Ορισμένα σενάρια, ωστόσο, απαιτούν από το χρήστη για να δημιουργήσετε και να χρησιμοποιήσετε απευθείας κωδικοί ασφαλείας. Αυτά περιλαμβάνουν την απευθείας χρήση των επιφανειών MQTT, AMQP ή HTTP, ή για την εφαρμογή του μοτίβου υπηρεσία διακριτικού.

Μπορείτε να βρείτε περισσότερες λεπτομέρειες σχετικά με τη δομή του διακριτικού ασφαλείας και τη χρήση στα ακόλουθα άρθρα:

-   [Δομή διακριτικού ασφαλείας][lnk-security-tokens]
-   [Χρήση διακριτικών SAS ως συσκευή][lnk-sas-tokens]

Κάθε διανομέας IoT διαθέτει ένα [Μητρώο ταυτότητα συσκευής] [ lnk-identity-registry] που μπορεί να χρησιμοποιηθεί για τη δημιουργία πόρων ανά συσκευή στην υπηρεσία, όπως μια ουρά που περιέχει μηνύματα εν πτήσει σύννεφο σε συσκευή, και να επιτρέψετε την πρόσβαση των τελικών σημείων της συσκευής με σύνδεση. Το μητρώο ταυτότητα IoT διανομέας παρέχει ασφαλή αποθήκευση των ταυτοτήτων συσκευή και τα κλειδιά ασφαλείας για μια λύση. Μεμονωμένες ή ομαδικές ταυτοτήτων συσκευή μπορούν να προστεθούν σε μια λίστα επιτρεπόμενων χρηστών ή μια λίστα αποκλεισμένων επιτρέπει πλήρη έλεγχο πρόσβασης της συσκευής. Τα παρακάτω άρθρα παρέχουν περισσότερες λεπτομέρειες σχετικά με τη δομή του μητρώου ταυτότητα συσκευής και υποστηριζόμενες λειτουργίες.

[Ο διανομέας IoT υποστηρίζει πρωτόκολλα όπως MQTT, AMQP και HTTP][lnk-protocols]. Κάθε ένα από αυτά τα πρωτόκολλα χρήση διακριτικών ασφαλείας από τη συσκευή IoT IoT διανομέα με διαφορετικό τρόπο:

- AMQP: SASL ΑΠΛΈΣ και βασισμένος σε δηλώσεις AMQP ασφαλείας ({policyName}@sas.root.{iothubName} όταν πρόκειται για διανομέα επίπεδο διακριτικά; {deviceId} στην περίπτωση ειδικών συσκευών διακριτικά).

- MQTT: Η ΣΎΝΔΕΣΗ χρησιμοποιεί το πακέτο {deviceId} ως {ClientId} {IoThubhostname} / {deviceId} στο πεδίο **όνομα χρήστη** και ένα διακριτικό SAS στο πεδίο **κωδικού πρόσβασης** .

- HTTP: Έγκυρο διακριτικό είναι στην κεφαλίδα αίτησης άδειας.

IoT διανομέα συσκευή ταυτότητα μητρώου μπορεί να χρησιμοποιηθεί για να ρυθμίσετε τις πιστοποιήσεις ασφαλείας ανά συσκευή και τον έλεγχο πρόσβασης. Ωστόσο, εάν μια λύση IoT έχει ήδη μια σημαντική επένδυση σε ένα [συνδυασμό ταυτότητα προσαρμοσμένη συσκευή μητρώου ή/και ελέγχου ταυτότητας][lnk-custom-auth], μπορούν να ενταχθούν σε μια υπάρχουσα υποδομή με διανομέα IoT, δημιουργώντας μια υπηρεσία διακριτικού.

### <a name="x509-certificate-based-device-authentication"></a>Έλεγχος ταυτότητας της συσκευής που βασίζεται σε πιστοποιητικό X.509

Η χρήση ενός [πιστοποιητικού X.509 που βασίζεται σε συσκευή] [ lnk-use-x509] και τα συσχετισμένα ιδιωτικές και δημόσιες ζεύγος κλειδιών επιτρέπει πρόσθετου ελέγχου ταυτότητας στο φυσικό επίπεδο. Το ιδιωτικό κλειδί είναι αποθηκευμένο με ασφάλεια τη συσκευή και δεν είναι δυνατός ο εντοπισμός έξω από τη συσκευή. Το πιστοποιητικό X.509 περιλαμβάνει πληροφορίες σχετικά με τη συσκευή, όπως το Αναγνωριστικό συσκευής, και άλλες οργανωτικές λεπτομέρειες. Δημιουργείται μια υπογραφή του πιστοποιητικού, χρησιμοποιώντας το ιδιωτικό κλειδί.

Υψηλού επιπέδου συσκευής παροχής ροής:

- Για να συσχετίσετε ένα αναγνωριστικό σε μια φυσική συσκευή – ταυτότητα συσκευής ή/και πιστοποιητικό X.509 που σχετίζεται με τη συσκευή κατά την κατασκευή ή τη θέση σε λειτουργία τη συσκευή.

- Δημιουργήστε μια αντίστοιχη καταχώρηση ταυτότητα στο διανομέα IoT – ταυτότητα συσκευής και πληροφορίες σχετική συσκευή στο μητρώο IoT διανομέα τη συσκευή.

- Ασφαλή φύλαξη αποτύπωση πιστοποιητικού X.509 στο μητρώο συσκευής IoT διανομέα.

### <a name="root-certificate-on-device"></a>Το πιστοποιητικό ρίζας στη συσκευή

Κατά τη δημιουργία μιας ασφαλούς σύνδεσης TLS με το διανομέα IoT, η συσκευή IoT πραγματοποιεί έλεγχο ταυτότητας IoT διανομέα, χρησιμοποιώντας ένα πιστοποιητικό ρίζας, η οποία αποτελεί μέρος της συσκευής SDK. Για το πρόγραμμα-πελάτη C SDK το πιστοποιητικό που βρίσκεται κάτω από το φάκελο "\\c\\πιστοποιητικά" κάτω από τη ρίζα του το repo. Παρόλο που αυτά τα πιστοποιητικά ρίζας είναι μεγάλης διάρκειας, να εξακολουθεί να μπορεί να λήξουν ή να ανακληθούν. Εάν δεν υπάρχει τρόπος ενημέρωσης του πιστοποιητικού της συσκευής, η συσκευή ενδέχεται να μην μπορείτε να συνδεθείτε στη συνέχεια με το διανομέα IoT (ή οποιαδήποτε άλλη υπηρεσία νέφους). Έχοντας ένα μέσο για να ενημερώσετε το πιστοποιητικό ρίζας, όταν αναπτύσσεται η συσκευή IoT θα αποτελεσματικά τον περιορισμό αυτόν τον κίνδυνο.

## <a name="securing-the-connection"></a>Διασφάλιση της σύνδεσης

Σύνδεση στο Internet μεταξύ της συσκευής IoT και IoT διανομέα είναι ασφαλής χρησιμοποιώντας το πρότυπο Transport Layer Security (TLS). Azure IoT υποστηρίζει [TLS 1.2][lnk-tls12], TLS 1.1 και TLS 1.0, με αυτή τη σειρά. Υποστήριξη για το TLS 1.0 παρέχεται μόνο για λόγους συμβατότητας. Συνιστάται να χρησιμοποιήσετε TLS 1.2, επειδή προσφέρει μεγαλύτερη ασφάλεια.

Οικογένεια IoT Azure υποστηρίζει τα ακόλουθα προγράμματα κρυπτογράφησης, με αυτή τη σειρά.

| Η οικογένεια προγραμμάτων κρυπτογράφησης | Μήκος |
|--------------|--------|
| TLS\_ECDHE\_RSA\_με\_AES\_256\_CBC\_SHA384 secp384r1 ECDH (0xc028) (εξίσωσης FS 7680 bit RSA) | 256    |
| TLS\_ECDHE\_RSA\_με\_AES\_128\_CBC\_SHA256 secp256r1 ECDH (0xc027) (εξίσωσης FS 3072 bit RSA) | 128    |
| TLS\_ECDHE\_RSA\_με\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (εξίσωσης FS 7680 bit RSA)           | 256    |
| TLS\_ECDHE\_RSA\_με\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (εξίσωσης FS 3072 bit RSA)           | 128    |
| TLS\_RSA\_με\_AES\_256\_GCM\_SHA384 (0x9d) | 256    |
| TLS\_RSA\_με\_AES\_128\_GCM\_SHA256 (0x9c) | 128    |
| TLS\_RSA\_με\_AES\_256\_CBC\_SHA256 (0x3d) | 256    |
| TLS\_RSA\_με\_AES\_128\_CBC\_SHA256 (0x3c) | 128    |
| TLS\_RSA\_με\_AES\_256\_CBC\_SHA (0x35)    | 256    |
| TLS\_RSA\_με\_AES\_128\_CBC\_SHA (0x2f)    | 128    |
| TLS\_RSA\_με\_3DES\_ο ΟΡΌΣ\_CBC\_SHA (0xa)    | 112    |

## <a name="securing-the-cloud"></a>Ασφάλεια σύννεφο

Ο διανομέας IoT Azure επιτρέπει ορισμό των [πολιτικών ελέγχου πρόσβασης] [ lnk-protocols] για κάθε κλειδί ασφαλείας. Για να παραχωρήσετε πρόσβαση σε καθεμία από τις απολήξεις του διανομέα IoT χρησιμοποιεί το ακόλουθο σύνολο δικαιωμάτων. Δικαιώματα περιορίσει την πρόσβαση σε ένα διανομέα IoT με βάση τη λειτουργία.

- **RegistryRead**. Παραχωρεί δικαιώματα ανάγνωσης στο μητρώο ταυτότητα συσκευής. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [Μητρώου ταυτότητα συσκευής][lnk-identity-registry].

- **RegistryReadWrite**. Παραχωρεί πρόσβαση ανάγνωσης και εγγραφής στο μητρώο ταυτότητα συσκευής. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [Μητρώου ταυτότητα συσκευής][lnk-identity-registry].

- **ServiceConnect**. Παραχωρεί πρόσβαση σύννεφου στραμμένου υπηρεσία επικοινωνίας και παρακολούθησης τελικά σημεία. Για παράδειγμα, εκχωρεί δικαιώματα για υπηρεσίες νέφους υποστήριξης για συσκευή-σύννεφο μηνύματα, αποστολή μηνυμάτων σύννεφο σε συσκευή και ανάκτησης το αντίστοιχο αναγνωρίσεις παράδοσης.

- **DeviceConnect**. Παραχωρεί πρόσβαση σε τελικά σημεία επικοινωνίας με τη συσκευή. Για παράδειγμα, εκχωρεί δικαιώματα για αποστολή μηνυμάτων συσκευή-σύννεφο και να λαμβάνετε μηνύματα σύννεφο σε συσκευή. Το δικαίωμα αυτό χρησιμοποιείται από συσκευές.

Υπάρχουν δύο τρόποι για να αποκτήσετε δικαιώματα **DeviceConnect** με το διανομέα IoT με [διακριτικά ασφάλειας][lnk-sas-tokens]: χρησιμοποιώντας ένα κλειδί ταυτότητας συσκευή ή ένα κλειδί πολιτικής κοινόχρηστη πρόσβαση. Επιπλέον, είναι σημαντικό να σημειωθεί ότι εκτίθεται όλες τις λειτουργίες προσβάσιμες από συσκευές από τη σχεδίαση στις απολήξεις με πρόθεμα `/devices/{deviceId}`.

[Στοιχεία της υπηρεσίας μπορεί μόνο να δημιουργήσει διακριτικά ασφάλειας] [ lnk-service-tokens] η χρήση κοινών πολιτικών πρόσβασης χορήγηση τα κατάλληλα δικαιώματα.

Διανομέας IoT Azure και άλλες υπηρεσίες που μπορεί να είναι μέρος της λύσης επιτρέπεται η Διαχείριση χρηστών χρησιμοποιώντας το Azure υπηρεσίας καταλόγου Active Directory.

Δεδομένα κατάποση από διανομέα IoT Azure μπορεί να χρησιμοποιηθεί από μια ποικιλία υπηρεσιών όπως Azure Analytics ροής και αποθήκευσης αντικειμένων blob Azure. Οι υπηρεσίες αυτές επιτρέπουν πρόσβαση διαχείρισης. Διαβάστε περισσότερα σχετικά με αυτές τις υπηρεσίες και τις παρακάτω διαθέσιμες επιλογές:

- [Azure DocumentDB][lnk-docdb]: μια υπηρεσία με δυνατότητα κλιμάκωσης, πλήρως ευρετηρίου βάσης δεδομένων για ημι-δομημένων δεδομένων που διαχειρίζεται μετα-δεδομένων για τις συσκευές που διάταξης, όπως τα χαρακτηριστικά, τις παραμέτρους και τις ιδιότητες ασφαλείας. DocumentDB προσφέρει υψηλής απόδοσης και υψηλής απόδοσης επεξεργασία, σχήμα agnostic ευρετήριο δεδομένων και μια εμπλουτισμένη διασύνδεση ερωτήματος SQL.

- [Ανάλυση ροής Azure][lnk-asa]: η ροή σε πραγματικό χρόνο επεξεργασίας στο σύννεφο που επιτρέπει την ταχεία ανάπτυξη και τη Διαχείριση ανάλυσης χαμηλού κόστους λύση να αποκαλύπτουν πληροφορίες πραγματικού χρόνου από συσκευές, αισθητήρες, υποδομή και εφαρμογές. Τα δεδομένα από αυτήν την υπηρεσία πλήρους διαχείρισης μπορούν να κλιμακωθούν σε οποιονδήποτε τόμο ενώ εξακολουθεί να την επίτευξη υψηλής απόδοσης, χαμηλού λανθάνοντος χρόνου και ανοχής.

- [Υπηρεσίες App Azure][lnk-appservices]: ένα σύννεφο πλατφόρμα για τη δημιουργία ισχυρών web και φορητές εφαρμογές που συνδέονται με δεδομένα οπουδήποτε; στο σύννεφο ή εσωτερικής εγκατάστασης. Δημιουργία ασκεί φορητές εφαρμογές για iOS, Android και Windows. Ενοποίηση με το λογισμικό ως υπηρεσία (ΑΠΑ) και εταιρικές εφαρμογές out of box συνδεσιμότητα σε δεκάδες υπηρεσίες που βασίζεται στο νέφος και εταιρικές εφαρμογές. Ο κωδικός σας αγαπημένη γλώσσα και IDE (.NET, NodeJS, PHP, Python ή Java) για τη δημιουργία εφαρμογών web και APIs ταχύτερα από ποτέ.

- [Λογική εφαρμογών][lnk-logicapps]: Η λογική εφαρμογών τη δυνατότητα της υπηρεσίας App Azure σας βοηθά να ενοποιήσουν τη λύση σας IoT με τα υπάρχοντα συστήματα γραμμής επιχείρησης και την αυτοματοποίηση διαδικασιών ροής εργασίας. Λογική εφαρμογών επιτρέπει στους προγραμματιστές να ροές εργασιών σχεδίασης που ξεκινούν από ένα έναυσμα και, στη συνέχεια, εκτελεί μια σειρά από βήματα — κανόνες και ενέργειες που πρέπει να χρησιμοποιείτε ισχυρές συνδέσεις για την ενσωμάτωση με τις επιχειρηματικές διαδικασίες. Λογική εφαρμογών προσφέρει συνδεσιμότητα out of box ένα τεράστιο περιβάλλον εμπορικής προσαρμογής της ΑΔΑ, βασιζόμενη σε σύννεφα, και σε εγκαταστάσεις εφαρμογών.

- [Αποθήκευσης αντικειμένων blob Azure][lnk-blob]: αξιόπιστες, οικονομικός σύννεφο αποθήκευσης για τα δεδομένα που αποστέλλουν οι συσκευές για το σύννεφο.

## <a name="conclusion"></a>Συμπέρασμα

Αυτό το άρθρο παρέχει επισκόπηση της υλοποίησης επίπεδο λεπτομερειών για τη σχεδίαση και την ανάπτυξη μιας υποδομής IoT χρησιμοποιώντας Azure IoT. Ρύθμιση παραμέτρων κάθε στοιχείο να είναι ασφαλές είναι το κλειδί στη διασφάλιση της συνολικής υποδομής IoT. Το σχέδιο επιλογές διαθέσιμες στο Azure IoT παρέχουν ορισμένες επίπεδο ευελιξία και τη δυνατότητα; Ωστόσο, κάθε επιλογή ενδέχεται να έχει συνέπειες στην ασφάλεια. Συνιστάται η κάθε μία από αυτές τις επιλογές αξιολογούνται με μια αξιολόγηση κινδύνου/κόστους.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-as-a-device
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#using-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
