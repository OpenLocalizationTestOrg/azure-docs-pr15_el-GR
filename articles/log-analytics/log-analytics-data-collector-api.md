<properties
    pageTitle="Συνδεθείτε συλλογής δεδομένων HTTP ανάλυση API | Microsoft Azure"
    description="Μπορείτε να χρησιμοποιήσετε το API καταγραφής ανάλυσης HTTP δεδομένων συλλογής για να προσθέσετε δεδομένα JSON ΔΗΜΟΣΊΕΥΣΗ στο χώρο αποθήκευσης του αρχείου καταγραφής ανάλυσης από οποιοδήποτε πρόγραμμα-πελάτη που μπορούν να καλέσουν το REST API. Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε το API και περιλαμβάνει παραδείγματα του τρόπου για τη δημοσίευση δεδομένων με τη χρήση διαφορετικών γλωσσών προγραμματισμού."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="bwren"/>


# <a name="log-analytics-http-data-collector-api"></a>Αρχείο καταγραφής ανάλυσης δεδομένων HTTP συλλογής API

Κατά τη χρήση του API συλλογής Azure καταγραφής ανάλυσης HTTP δεδομένων, μπορείτε να προσθέσετε δεδομένα σημειογραφίας αντικειμένων JavaScript ΔΗΜΟΣΊΕΥΣΗ (JSON) στο χώρο αποθήκευσης του αρχείου καταγραφής ανάλυσης από οποιοδήποτε πρόγραμμα-πελάτη που μπορούν να καλέσουν το REST API. Χρησιμοποιώντας αυτήν τη μέθοδο, μπορείτε να στείλετε δεδομένα από εφαρμογές τρίτων κατασκευαστών ή από δέσμες ενεργειών, όπως από ένα runbook στο Azure αυτοματισμού.  

## <a name="create-a-request"></a>Δημιουργήστε μια αίτηση

Τα επόμενα δύο πίνακες παρατίθενται τα χαρακτηριστικά που απαιτούνται για κάθε αίτηση για το API συλλογής δεδομένων του αρχείου καταγραφής ανάλυσης HTTP. Περιγραφή κάθε χαρακτηριστικό με περισσότερες λεπτομέρειες αργότερα στο άρθρο.

### <a name="request-uri"></a>Αίτηση URI

| Χαρακτηριστικό | Ιδιότητα |
|:--|:--|
| Μέθοδος | ΔΗΜΟΣΊΕΥΣΗ |
| URI | https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Τύπος περιεχομένου | εφαρμογή/json |

### <a name="request-uri-parameters"></a>Παράμετροι URI αίτησης
| Παράμετρος | Περιγραφή |
|:--|:--|
| Κωδικός πελάτη  | Το μοναδικό αναγνωριστικό για το χώρο εργασίας οικογένεια προγραμμάτων του Microsoft λειτουργίες διαχείρισης. |
| Πόρων    | Το όνομα του πόρου API: / api/αρχεία καταγραφής. |
| Η έκδοση API | Η έκδοση του API για χρήση με αυτήν την αίτηση. Προς το παρόν, είναι 2016-04-01. |

### <a name="request-headers"></a>Κεφαλίδες αίτησης
| Κεφαλίδα | Περιγραφή |
|:--|:--|
| Εξουσιοδότηση | Η υπογραφή εξουσιοδότησης. Αργότερα στο άρθρο, μπορείτε να διαβάσετε σχετικά με τον τρόπο για να δημιουργήσετε μια κεφαλίδα HMAC SHA256. |
| Τύπος αρχείου καταγραφής | Καθορίστε τον τύπο εγγραφής των δεδομένων που υποβάλλεται. Προς το παρόν, ο τύπος αρχείου καταγραφής υποστηρίζει μόνο άλφα χαρακτήρες. Δεν υποστηρίζει αριθμητικές τιμές ή ειδικούς χαρακτήρες. |
| x-ms-ημερομηνία | Η ημερομηνία που έγινε επεξεργασία της αίτησης, σε μορφή RFC 1123. |
| χρόνο που δημιουργούνται από το πεδίο | Το όνομα ενός πεδίου των δεδομένων που περιέχει τη χρονική σήμανση του στοιχείου δεδομένων. Εάν καθορίσετε ένα πεδίο, στη συνέχεια, τα περιεχόμενά που χρησιμοποιούνται για **TimeGenerated**. Εάν δεν έχει καθοριστεί σε αυτό το πεδίο, η προεπιλογή για **TimeGenerated** είναι ο χρόνος που εισάγεται στο μήνυμα. Τα περιεχόμενα του πεδίου μηνύματος πρέπει να ακολουθήσετε τη μορφή ISO 8601 εεεε-MM-DDThh:mm:ssZ. |


## <a name="authorization"></a>Εξουσιοδότηση

Οποιαδήποτε αίτηση για το API συλλογής δεδομένων του αρχείου καταγραφής ανάλυσης HTTP πρέπει να συμπεριλάβετε μια κεφαλίδα ελέγχου ταυτότητας. Για τον έλεγχο ταυτότητας αίτησης, πρέπει να εισέλθετε την αίτηση με το πρωτεύον ή το δευτερεύον κλειδί για το χώρο εργασίας που κάνει την αίτηση. Στη συνέχεια, μεταβιβάζουν αυτήν την υπογραφή ως μέρος της αίτησης.   

Εδώ είναι η μορφή για την κεφαλίδα εξουσιοδότησης:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* είναι το μοναδικό αναγνωριστικό για το χώρο εργασίας οικογένεια προγραμμάτων διαχείρισης λειτουργίες. *Υπογραφή* είναι ένα [Αρχείο με βάση κατακερματισμός μήνυμα ελέγχου ταυτότητας κώδικα (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) που έχει συνταχθεί από την αίτηση και, στη συνέχεια, υπολογιστεί με τη χρήση του [αλγόριθμου SHA256](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Στη συνέχεια, κωδικοποιήσετε το χρησιμοποιώντας κωδικοποίηση Base64.

Χρησιμοποιήστε αυτήν τη μορφή για να κωδικοποιήσετε τη συμβολοσειρά υπογραφή **SharedKey** :

```
StringToSign = VERB + "\n" +
               Content-Length + "\n" +
               Content-Type + "\n" +
               x-ms-date + "\n" +
               "/api/logs";
```

Ακολουθεί ένα παράδειγμα μιας συμβολοσειράς υπογραφής:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Όταν έχετε τη συμβολοσειρά υπογραφής, κωδικοποίηση, χρησιμοποιώντας τον αλγόριθμο HMAC SHA256 στη συμβολοσειρά κωδικοποίηση UTF-8 και, στη συνέχεια, να κωδικοποιήσετε το αποτέλεσμα ως Base64. Χρησιμοποιήστε αυτήν τη μορφή:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Δείγμα κώδικα για να δημιουργήσετε μια κεφαλίδα εξουσιοδότησης πρέπει τα δείγματα στις επόμενες ενότητες.

## <a name="request-body"></a>Σώμα αίτησης

Στο σώμα του μηνύματος πρέπει να είναι στο JSON. Αυτό πρέπει να περιλαμβάνουν μία ή περισσότερες εγγραφές με την ιδιότητα ονόματα και ζεύγη τιμών σε αυτήν τη μορφή:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

Μπορείτε να μαζική πολλές εγγραφές μαζί σε μία μόνο αίτηση, χρησιμοποιώντας την εξής μορφή. Όλες οι εγγραφές πρέπει να είναι του ίδιου τύπου εγγραφής.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Τύπος εγγραφής και τις ιδιότητες

Μπορείτε να ορίσετε έναν προσαρμοσμένο τύπο καρτέλας κατά την υποβολή δεδομένων μέσω του API συλλογής δεδομένων του αρχείου καταγραφής ανάλυσης HTTP. Προς το παρόν, δεν μπορείτε να εγγράψετε δεδομένων σε υπάρχοντες τύπους καρτελών που έχουν δημιουργηθεί από άλλους τύπους δεδομένων και τις λύσεις. Ανάλυση καταγραφής διαβάζει τα εισερχόμενα δεδομένα και, στη συνέχεια, δημιουργεί ιδιότητες που ταιριάζουν με τους τύπους δεδομένων από τις τιμές που καταχωρείτε.

Κάθε αίτηση για το αρχείο καταγραφής API ανάλυσης πρέπει να συμπεριλάβετε μια κεφαλίδα **Τύπος αρχείου καταγραφής** με το όνομα για τον τύπο εγγραφής. Το επίθημα **_CL** προσαρτώνται αυτόματα το όνομα που καταχωρήσατε για να διακρίνετε από τους άλλους τύπους καταγραφής ως ένα προσαρμοσμένο αρχείο καταγραφής. Για παράδειγμα, εάν πληκτρολογήσετε το όνομα **MyNewRecordType**, καταγραφής ανάλυσης δημιουργεί μια εγγραφή με τον τύπο **MyNewRecordType_CL**. Αυτό σας βοηθά να εξασφαλίσετε ότι δεν υπάρχουν διενέξεις μεταξύ ονομάτων τύπου που δημιουργήθηκαν από το χρήστη και αυτών που αποστέλλονται στο τρέχον ή μελλοντικές λύσεις του Microsoft.

Για να προσδιορίσετε μια ιδιότητα τύπος δεδομένων, αρχείο καταγραφής ανάλυσης προσθέτει την κατάληξη για το όνομα της ιδιότητας. Εάν μια ιδιότητα περιέχει μια τιμή null, την ιδιότητα δεν περιλαμβάνεται σε αυτήν την εγγραφή. Αυτός ο πίνακας παραθέτει τον τύπο δεδομένων ιδιότητα και το αντίστοιχο επίθημα:

| Τύπος δεδομένων ιδιοτήτων | Επίθημα |
|:--|:--|
| Συμβολοσειρά    | _Εναλλαγή |
| Δυαδική τιμή   | _b |
| Δύο    | _d |
| Ημερομηνία/ώρα | _Σφι |
| GUID      | _g |


Ο τύπος δεδομένων που χρησιμοποιεί το αρχείο καταγραφής ανάλυσης για κάθε ιδιότητα εξαρτάται από το αν υπάρχει ήδη τον τύπο εγγραφής για τη νέα εγγραφή.

- Εάν δεν υπάρχει στον τύπο εγγραφής, καταγραφής ανάλυσης δημιουργεί ένα νέο. Αρχείο καταγραφής ανάλυσης χρησιμοποιεί την ανίχνευση τύπου JSON για να προσδιορίσετε τον τύπο δεδομένων για κάθε ιδιότητας για τη νέα εγγραφή.
- Εάν υπάρχει στον τύπο εγγραφής, ανάλυση καταγραφής προσπαθεί να δημιουργήστε μια νέα εγγραφή με βάση υπάρχουσες ιδιότητες. Εάν τον τύπο δεδομένων για μια ιδιότητα στη νέα εγγραφή, δεν ταιριάζουν με και δεν μπορεί να μετατραπεί σε τον υπάρχοντα τύπο ή εάν η εγγραφή περιλαμβάνει μια ιδιότητα που δεν υπάρχει, καταγραφής ανάλυσης δημιουργεί μια νέα ιδιότητα που έχει το σχετικό επίθημα.

Για παράδειγμα, αυτή η καταχώρηση υποβολής θα δημιουργήσετε μια εγγραφή με τρεις ιδιότητες, **number_d**, **boolean_b**και **string_s**:

![Δείγμα εγγραφή 1](media/log-analytics-data-collector-api/record-01.png)

Εάν υποβάλει αυτή την επόμενη εγγραφή, στη συνέχεια, με όλες τις τιμές που μορφοποιούνται ως συμβολοσειρές, δεν θα αλλάξει τις ιδιότητες. Αυτές οι τιμές μπορούν να μετατραπούν σε υπάρχοντες τύπους δεδομένων:

![Δείγμα εγγραφή 2](media/log-analytics-data-collector-api/record-02.png)

Αλλά, εάν έχετε κάνει, στη συνέχεια, την επόμενη υποβολή, ανάλυση καταγραφής θα δημιουργήσετε το νέο ιδιότητες **boolean_d** και **string_d**. Είναι δυνατή η μετατροπή αυτές τις τιμές:

![Δείγμα εγγραφή 3](media/log-analytics-data-collector-api/record-03.png)

Αν έχετε υποβάλει την παρακάτω καταχώρηση, στη συνέχεια, πριν από τον τύπο εγγραφής που δημιουργήθηκε, ανάλυση καταγραφής να δημιουργήσετε μια εγγραφή με τρεις ιδιότητες, **αριθμός_επιτυχιών**, **boolean_s**και **string_s**. Σε αυτή την καταχώρηση, κάθε μία από τις αρχικές τιμές μορφοποιείται ως συμβολοσειρά:

![Δείγμα εγγραφή 4](media/log-analytics-data-collector-api/record-04.png)


## <a name="data-limits"></a>Όρια δεδομένων
Υπάρχουν ορισμένες περιορισμούς γύρω από τα δεδομένα που καταχώρησε το API στη συλλογή καταγραφής ανάλυσης δεδομένων.

- Έως και 30 MB ανά δημοσίευση στο αρχείο καταγραφής ανάλυσης δεδομένων συλλογής API. Αυτό είναι ένα όριο μεγέθους για μια μεμονωμένη δημοσίευση. Εάν τα δεδομένα από ένα μόνο καταχωρήσεις που υπερβαίνει 30 MB, πρέπει να διαιρέσετε τα δεδομένα έως και μικρότερο μέγεθος μπλοκ και να τους στείλετε ταυτόχρονα. 
- Μέγιστο όριο 32 KB για τιμές πεδίων. Εάν η τιμή του πεδίου είναι μεγαλύτερο από 32 KB, τα δεδομένα θα περικοπεί. 
- Συνιστάται μέγιστος αριθμός των πεδίων για ένα συγκεκριμένο τύπο είναι 50. Αυτό είναι πρακτικό όριο από χρηστικότητα και προοπτική εμπειρία αναζήτησης.  


## <a name="return-codes"></a>Κωδικοί επιστροφής

Ο κωδικός κατάστασης HTTP 202 σημαίνει ότι έχει γίνει αποδεκτή την αίτηση για επεξεργασία, αλλά η επεξεργασία δεν έχει ακόμα ολοκληρωθεί. Αυτό υποδεικνύει ότι η λειτουργία ολοκληρώθηκε με επιτυχία.

Αυτός ο πίνακας παραθέτει το πλήρες σύνολο των κωδικών κατάστασης που ενδέχεται να επιστρέφουν την υπηρεσία:

| Κωδικός | Κατάσταση | Κωδικός σφάλματος | Περιγραφή |
|:--|:--|:--|:--|
| 202 | Αποδοχή |  | Η αίτηση έγινε αποδεκτή με επιτυχία. |
| 400 | Ακατάλληλη αίτηση | InactiveCustomer | Στο χώρο εργασίας έχει κλείσει. |
| 400 | Ακατάλληλη αίτηση | InvalidApiVersion | Η έκδοση API που καθορίσατε δεν αναγνωρίζεται από την υπηρεσία. |
| 400 | Ακατάλληλη αίτηση | InvalidCustomerId | Το καθορισμένο Αναγνωριστικό χώρου εργασίας δεν είναι έγκυρη. |
| 400 | Ακατάλληλη αίτηση | InvalidDataFormat | Μη έγκυροι JSON υποβλήθηκε. Στο σώμα απόκριση ενδέχεται να περιέχουν περισσότερες πληροφορίες σχετικά με τον τρόπο για να επιλύσετε το σφάλμα. |
| 400 | Ακατάλληλη αίτηση | InvalidLogType | Ο τύπος αρχείου καταγραφής καθοριστεί περιεχόμενα ειδικούς χαρακτήρες ή αριθμητικές τιμές. |
| 400 | Ακατάλληλη αίτηση | MissingApiVersion | Η έκδοση API δεν που καθορίζεται. |
| 400 | Ακατάλληλη αίτηση | MissingContentType | Δεν καθορίσατε τον τύπο περιεχομένου. |
| 400 | Ακατάλληλη αίτηση | MissingLogType | Ο τύπος αρχείου καταγραφής απαιτούμενη τιμή δεν καθοριστεί. |
| 400 | Ακατάλληλη αίτηση | UnsupportedContentType | Ο τύπος περιεχομένου δεν ορίστηκε σε **εφαρμογή/json**. |
| 403 | Δεν επιτρέπεται | InvalidAuthorization | Η υπηρεσία απέτυχε για τον έλεγχο ταυτότητας της αίτησης. Βεβαιωθείτε ότι το κλειδί Αναγνωριστικό και σύνδεσης του χώρου εργασίας είναι έγκυρο. |
| 500 | Εσωτερικό σφάλμα διακομιστή | UnspecifiedError | Η υπηρεσία αντιμετώπισε εσωτερικό σφάλμα. Επαναλάβετε την αίτηση. |
| 503 | Υπηρεσία δεν είναι διαθέσιμη | ServiceUnavailable | Η υπηρεσία αυτήν τη στιγμή δεν είναι διαθέσιμη για τη λήψη αιτήσεις. Δοκιμάστε ξανά την αίτησή σας. |

## <a name="query-data"></a>Δεδομένα ερωτήματος

Για να ερωτήματος που υποβάλλονται από το API καταγραφής ανάλυσης HTTP δεδομένων συλλογής δεδομένων, κάντε αναζήτηση για εγγραφές με τον **τύπο** που ισούται με την τιμή **LogType** που καθορισμένο, προσαρτημένο με **_CL**. Για παράδειγμα, εάν χρησιμοποιήσατε **MyCustomLog**, στη συνέχεια, που θα επιστρέψει όλες τις εγγραφές με **Τύπος = MyCustomLog_CL**.


## <a name="sample-requests"></a>Δείγματα προσκλήσεων

Στις επόμενες ενότητες, θα βρείτε δείγματα σχετικά με τον τρόπο για την υποβολή δεδομένων για το API συλλογής καταγραφής ανάλυσης HTTP δεδομένων με τη χρήση διαφορετικών γλωσσών προγραμματισμού.

Για κάθε δείγμα, κάντε τα παρακάτω βήματα για να ορίσετε τις μεταβλητές για την κεφαλίδα εξουσιοδότησης:

1. Στην πύλη του λειτουργίες οικογένεια προγραμμάτων διαχείρισης, επιλέξτε το πλακίδιο **Ρυθμίσεις** και, στη συνέχεια, επιλέξτε την καρτέλα **Συνδεδεμένοι προελεύσεις** .
2. Στα δεξιά του **Αναγνωριστικού χώρου εργασίας**, επιλέξτε το εικονίδιο αντιγραφή και, στη συνέχεια, επικολλήστε το Αναγνωριστικό ως η τιμή της μεταβλητής **Αναγνωριστικό πελάτη** .
3. Στα δεξιά του **Πρωτεύοντος κλειδιού**, επιλέξτε το εικονίδιο αντιγραφή και, στη συνέχεια, επικολλήστε το Αναγνωριστικό ως η τιμή της μεταβλητής **Θέσει σε κοινή χρήση αριθμού-κλειδιού** .

Εναλλακτικά, μπορείτε να αλλάξετε τις μεταβλητές για τον τύπο αρχείου καταγραφής και δεδομένων JSON.

### <a name="powershell-sample"></a>Δείγμα PowerShell

```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>Δείγμα C#

```
using System;
using System.Net;
using System.Security.Cryptography;

namespace OIAPIExample
{
    class ApiExample
    {
// An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField1"":""DemoValue3"",""DemoField2"":""DemoValue4""}]";

// Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

// For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

// LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

// You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
// Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

// Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

// Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            string url = "https://"+ customerId +".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";
            using (var client = new WebClient())
            {
                client.Headers.Add(HttpRequestHeader.ContentType, "application/json");
                client.Headers.Add("Log-Type", LogName);
                client.Headers.Add("Authorization", signature);
                client.Headers.Add("x-ms-date", date);
                client.Headers.Add("time-generated-field", TimeStampField);
                client.UploadString(new Uri(url), "POST", json);
            }
        }
    }
}
```

### <a name="python-sample"></a>Δείγμα Python

```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code == 202):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Επόμενα βήματα

- Χρησιμοποιήστε την [Προβολή σχεδίασης](log-analytics-view-designer.md) για να δημιουργήσετε προσαρμοσμένες προβολές για τα δεδομένα που υποβάλλετε.
