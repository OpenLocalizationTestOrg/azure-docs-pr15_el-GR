<properties
    pageTitle="Αντιμετώπιση προβλημάτων συμπίεση αρχείων στο Azure CDN | Microsoft Azure"
    description="Αντιμετώπιση προβλημάτων με συμπίεση αρχείων Azure CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-file-compression"></a>Αντιμετώπιση προβλημάτων CDN συμπίεση αρχείων

Σε αυτό το άρθρο σάς βοηθά να αντιμετωπίσετε τα προβλήματα [CDN συμπίεση αρχείων](cdn-improve-performance.md).

Εάν χρειάζεστε περισσότερη βοήθεια σε οποιοδήποτε σημείο σε αυτό το άρθρο, μπορείτε να επικοινωνήσετε με το Azure ειδικούς [το Azure MSDN](https://azure.microsoft.com/support/forums/)και τα φόρουμ υπερχείλισης στοίβας. Εναλλακτικά, μπορείτε επίσης να υποβάλετε ένα περιστατικό Azure υποστήριξης. Μεταβείτε στην [τοποθεσία υποστήριξης Azure](https://azure.microsoft.com/support/options/) και κάντε κλικ στην επιλογή **Λήψη υποστήριξης**.

## <a name="symptom"></a>Σύμπτωμα

Συμπίεση για το τελικό σημείο σας είναι ενεργοποιημένη, αλλά τα αρχεία που επιστρέφονται αρχεία μη συμπιεσμένων.

>[AZURE.TIP] Για να ελέγξετε αν τα αρχεία σας που επιστρέφονται συμπιεσμένο, πρέπει να χρησιμοποιήσετε ένα εργαλείο όπως [Fiddler](http://www.telerik.com/fiddler) ή του προγράμματος περιήγησής σας [Εργαλεία για προγραμματιστές](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Κεφαλίδες απόκρισης ελέγχου HTTP επιστρέφονται με σας στο cache CDN περιεχομένου.  Εάν υπάρχει μια κεφαλίδα με το όνομα `Content-Encoding` με μια τιμή **gzip**, **bzip2**ή **κοίλωμα**, συμπιέζεται το περιεχόμενό σας.
>
>![Κωδικοποίηση περιεχομένου κεφαλίδας](./media/cdn-troubleshoot-compression/cdn-content-header.png)

## <a name="cause"></a>Αιτία

Υπάρχουν αρκετές πιθανές αιτίες, όπως:

- Το περιεχόμενο που ζητήθηκε δεν είναι κατάλληλη για τη συμπίεση.
- Συμπίεση δεν είναι ενεργοποιημένη για τον τύπο αρχείου που ζητήθηκε.
- Η αίτηση HTTP δεν συμπεριλάβατε μια κεφαλίδα αίτηση έναν τύπο έγκυρη συμπίεση.

## <a name="troubleshooting-steps"></a>Βήματα αντιμετώπισης προβλημάτων

> [AZURE.TIP] Όπως με την ανάπτυξη νέου τελικά σημεία, αλλαγές στη ρύθμιση παραμέτρων CDN χρειαστεί κάποιος χρόνος για τη μετάδοση μέσω του δικτύου.  Συνήθως, οι αλλαγές εφαρμόζονται εντός 90 λεπτά.  Εάν αυτή είναι η πρώτη φορά που έχετε ρυθμίσει τη συμπίεση για το τελικό σημείο CDN, θα πρέπει να περιμένουν 1-2 ώρες για να βεβαιωθείτε ότι η συμπίεση έχει έχουν μεταδοθεί ρυθμίσεις για να εμφανίζεται το. 

### <a name="verify-the-request"></a>Επαληθεύστε την αίτηση

Πρώτα, θα πρέπει να κάνουμε ένα σημάδι ελέγχου ασφαλείας γρήγορα στην αίτηση.  Μπορείτε να χρησιμοποιήσετε το πρόγραμμα περιήγησης [Εργαλεία για προγραμματιστές](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) για να προβάλετε τα αιτήματα.

- Επαληθεύστε την αίτηση αποστέλλεται σε διεύθυνση URL του τελικού σημείου, `<endpointname>.azureedge.net`, και όχι την προέλευση.
- Επαληθεύστε την αίτηση περιέχει μια κεφαλίδα **Κωδικοποίηση αποδοχή** και την τιμή για αυτήν την κεφαλίδα **gzip**, **κοίλωμα**ή **bzip2**.

> [AZURE.NOTE] Προφίλ **CDN Azure από Akamai** υποστηρίζει μόνο κωδικοποίηση **gzip** .

![Κεφαλίδες αίτησης CDN](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Επαληθεύστε τις ρυθμίσεις συμπίεσης (τυπική CDN προφίλ)

> [AZURE.NOTE] Αυτό το βήμα ισχύει μόνο εάν το προφίλ σας CDN είναι ένα προφίλ του **Azure CDN τυπική από Verizon** ή **Azure CDN τυπική από Akamai** . 

Μεταβείτε το τελικό σημείο σας στην [πύλη του Azure](https://portal.azure.com) και κάντε κλικ στο κουμπί **Ρύθμιση παραμέτρων** .

- Επιβεβαιώστε τη συμπίεση είναι ενεργοποιημένη.
- Επαληθεύστε τον τύπο MIME για το περιεχόμενο για να συμπιεστεί περιλαμβάνεται στη λίστα των συμπιεσμένο μορφές.

![Ρυθμίσεις συμπίεσης CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Επαληθεύστε τις ρυθμίσεις συμπίεσης (Premium CDN προφίλ)

> [AZURE.NOTE] Αυτό το βήμα ισχύει μόνο εάν το προφίλ σας CDN είναι ένα προφίλ **Premium CDN Azure από Verizon** .

Μεταβείτε το τελικό σημείο σας στην [πύλη του Azure](https://portal.azure.com) και κάντε κλικ στο κουμπί **Διαχείριση** .  Συμπληρωματικοί πύλη θα ανοίξει.  Hover πάνω από την καρτέλα **Μεγάλο HTTP** , στη συνέχεια, hover πάνω από το αναδυόμενο **Ρυθμίσεις του Cache** .  Κάντε κλικ στην επιλογή **συμπίεση**. 

- Επιβεβαιώστε τη συμπίεση είναι ενεργοποιημένη.
- Επαληθεύστε τη λίστα **Τύπων αρχείων** περιέχει μια λίστα διαχωρισμένες με κόμματα (χωρίς κενά διαστήματα) των τύπων MIME.
- Επαληθεύστε τον τύπο MIME για το περιεχόμενο για να συμπιεστεί περιλαμβάνεται στη λίστα των συμπιεσμένο μορφές.

![Ρυθμίσεις συμπίεσης premium CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Επαλήθευση του περιεχομένου είναι στο cache

> [AZURE.NOTE] Αυτό το βήμα ισχύει μόνο εάν το προφίλ σας CDN είναι ένα προφίλ **CDN Azure από Verizon** (τυπική ή Premium).

Χρησιμοποιώντας εργαλεία για προγραμματιστές του προγράμματος περιήγησής σας, ελέγξτε τις κεφαλίδες απόκρισης για να βεβαιωθείτε ότι το αρχείο είναι στο cache της περιοχής όπου είναι που ζητήθηκε.

- Επιλέξτε την κεφαλίδα απόκρισης του **διακομιστή** .  Η κεφαλίδα θα πρέπει να έχετε τη μορφή **πλατφόρμα (POP/διακομιστή ID)**, όπως φαίνεται στο παρακάτω παράδειγμα.
- Επιλέξτε την κεφαλίδα απόκρισης **X-Cache** .  Κεφαλίδα πρέπει να διαβάσετε **ΠΑΤΉΣΕΤΕ**.  

![Κεφαλίδες απόκρισης CDN](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Επιβεβαιώστε το αρχείο πληροί τις απαιτήσεις μέγεθος

> [AZURE.NOTE] Αυτό το βήμα ισχύει μόνο εάν το προφίλ σας CDN είναι ένα προφίλ **CDN Azure από Verizon** (τυπική ή Premium).

Για να πληρούν τις απαιτήσεις για τη συμπίεση, ένα αρχείο πρέπει να ικανοποιεί τις ακόλουθες απαιτήσεις μέγεθος:

- Μεγαλύτερο από 128 byte.
- Μικρότερη από 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Ελέγξτε την αίτηση στο διακομιστή προέλευσης για μια επικεφαλίδα **μέσω**

Στην κεφαλίδα του **μέσω του** πρωτοκόλλου HTTP υποδεικνύει στο διακομιστή web που είναι που διαβιβάζεται την αίτηση, διακομιστή μεσολάβησης.  Διακομιστές web των υπηρεσιών IIS Microsoft από προεπιλογή δεν συμπιέσετε αποκρίσεις όταν η αίτηση περιέχει μια κεφαλίδα **μέσω** .  Για να παρακάμψετε αυτήν τη συμπεριφορά, εκτελέστε τα εξής:

- **IIS 6**: [Ορισμός HcNoCompressionForProxies = "FALSE" στις ιδιότητες μετα-βάσης των υπηρεσιών IIS](https://msdn.microsoft.com/library/ms525390.aspx)
- **Των υπηρεσιών IIS 7 και νεότερες εκδόσεις**: [Ορισμός **noCompressionForHttp10** και **noCompressionForProxies** σε False στη ρύθμιση παραμέτρων του διακομιστή](http://www.iis.net/configreference/system.webserver/httpcompression)

