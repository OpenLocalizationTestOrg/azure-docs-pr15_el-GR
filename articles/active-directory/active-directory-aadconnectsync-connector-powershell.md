<properties
   pageTitle="Γραμμή σύνδεσης PowerShell | Microsoft Azure"
   description="Σε αυτό το άρθρο περιγράφει τον τρόπο ρύθμισης των παραμέτρων του Microsoft Windows PowerShell σύνδεσης."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="windows-powershell-connector-technical-reference"></a>Τεχνική αναφορά του Windows PowerShell σύνδεσης
Σε αυτό το άρθρο περιγράφει τη γραμμή σύνδεσης του Windows PowerShell. Το άρθρο ισχύει για τα ακόλουθα προϊόντα:

- Διαχείριση ταυτοτήτων Microsoft 2016 (MIM2016)
- Διαχείριση ταυτοτήτων Forefront 2010 R2 (FIM2010R2)
    -   Πρέπει να χρησιμοποιήσετε την άμεση επιδιόρθωση 4.1.3671.0 ή νεότερη έκδοση [KB3092178](https://support.microsoft.com/kb/3092178).

Για MIM2016 και FIM2010R2, η σύνδεση είναι διαθέσιμο για λήψη από το [Κέντρο λήψης της Microsoft](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Επισκόπηση της γραμμής σύνδεσης του PowerShell
Η γραμμή σύνδεσης PowerShell σάς επιτρέπει να ενοποίηση της υπηρεσίας συγχρονισμού με εξωτερικά συστήματα που προσφέρουν APIs βάσει του Windows PowerShell. Η γραμμή σύνδεσης παρέχει γέφυρα μεταξύ τις δυνατότητες του παράγοντα διαχείρισης βάσει κλήση επεκτάσιμη συνδεσιμότητας 2 framework (ECMA2) και του Windows PowerShell. Για περισσότερες πληροφορίες σχετικά με το πλαίσιο ECMA, ανατρέξτε στο άρθρο [Επεκτάσιμη 2.2 διαχείρισης παράγοντας αναφοράς για τη συνδεσιμότητα](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Πριν χρησιμοποιήσετε τη γραμμή σύνδεσης, βεβαιωθείτε ότι έχετε τις ακόλουθες ενέργειες από το διακομιστή συγχρονισμού:

- 4.5.2 του Microsoft .NET Framework ή νεότερη έκδοση
- 2.0, 3.0 ή 4.0 του Windows PowerShell

Η πολιτική εκτέλεσης στο διακομιστή υπηρεσίας συγχρονισμού πρέπει να ρυθμιστεί για να επιτρέψετε τη γραμμή σύνδεσης για να εκτελέσετε τις δέσμες ενεργειών του Windows PowerShell. Εκτός εάν οι δέσμες ενεργειών η γραμμή σύνδεσης εκτελείται έχουν ψηφιακή υπογραφή, ρύθμιση παραμέτρων της πολιτικής εκτέλεσης κατά την εκτέλεση αυτής της εντολής:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Δημιουργήστε μια νέα γραμμή σύνδεσης
Για να δημιουργήσετε μια γραμμή σύνδεσης του Windows PowerShell της υπηρεσίας συγχρονισμού, πρέπει να δώσετε μια σειρά των δεσμών ενεργειών του Windows PowerShell που εκτελεί τα βήματα που ζητήθηκε από την υπηρεσία συγχρονισμού. Ανάλογα με την προέλευση δεδομένων, μπορείτε να συνδεθείτε σε και τη λειτουργικότητα που χρειάζεστε, οι δέσμες ενεργειών, πρέπει να εφαρμόσετε διαφέρει. Αυτή η ενότητα περιγράφει κάθε μία από τις δέσμες ενεργειών που μπορεί να υλοποιηθεί και όταν είναι απαραίτητο.

Η γραμμή σύνδεσης του Windows PowerShell έχει σχεδιαστεί για να αποθηκεύσετε κάθε μία από τις δέσμες ενεργειών μέσα στη βάση δεδομένων υπηρεσίας συγχρονισμού. Ενώ είναι δυνατό να εκτελέσετε τις δέσμες ενεργειών που είναι αποθηκευμένα στο σύστημα αρχείων, είναι πιο εύκολο να εισαγάγετε στο κυρίως σώμα του κάθε δέσμη ενεργειών απευθείας στη ρύθμιση παραμέτρων της σύνδεσης.

Για να δημιουργήσετε μια γραμμή σύνδεσης PowerShell, στην **Υπηρεσία συγχρονισμού** επιλέξτε **Παράγοντας διαχείρισης** και **Δημιουργία**. Επιλέξτε τη γραμμή σύνδεσης **PowerShell (Microsoft)** .

![Δημιουργία σύνδεσης](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Συνδεσιμότητα
Δώστε παραμέτρους για τη σύνδεση σε ένα απομακρυσμένο σύστημα. Αυτές οι τιμές αποθηκεύονται από την υπηρεσία συγχρονισμού με ασφάλεια και κάνατε διαθέσιμα για τις δέσμες ενεργειών του Windows PowerShell όταν εκτελείται η γραμμή σύνδεσης.

![Συνδεσιμότητα](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Μπορείτε να ρυθμίσετε τις ακόλουθες παραμέτρους σύνδεσης:

**Συνδεσιμότητα**

Παράμετρος | Προεπιλεγμένη τιμή | Σκοπός
--- | --- | ---
Διακομιστή | <Blank> | Όνομα διακομιστή που θα πρέπει να συνδεθείτε στη γραμμή σύνδεσης.
Τομέα | <Blank> | Τομέα του των διαπιστευτηρίων για την αποθήκευση για χρήση κατά την εκτέλεση στη γραμμή σύνδεσης.
Χρήστη | <Blank> | Το όνομα χρήστη από τα διαπιστευτήρια για την αποθήκευση για χρήση κατά την εκτέλεση στη γραμμή σύνδεσης.
Κωδικός πρόσβασης | <Blank> | Τον κωδικό πρόσβασης του των διαπιστευτηρίων για την αποθήκευση για χρήση κατά την εκτέλεση στη γραμμή σύνδεσης.
Μίμηση λογαριασμός σύνδεσης | FALSE | Όταν είναι true, η υπηρεσία συγχρονισμού εκτελείται των δεσμών ενεργειών του Windows PowerShell στο περιβάλλον της τα διαπιστευτήρια που παρέχονται. Όταν είναι δυνατό, συνιστάται να ότι η παράμετρος **$Credentials** μεταβιβάζεται σε κάθε δέσμη ενεργειών χρησιμοποιείται αντί για απομίμησης. Για περισσότερες πληροφορίες σχετικά με τα πρόσθετα δικαιώματα που απαιτούνται για να χρησιμοποιήσετε αυτήν την επιλογή, ανατρέξτε στην ενότητα [Πρόσθετες ρυθμίσεις παραμέτρων για απομίμησης](#additional-configuration-for-impersonation).
Φόρτωση προφίλ χρήστη όταν μιμείται | FALSE | Καθοδηγεί Windows για να φορτώσετε το προφίλ χρήστη από τη γραμμή σύνδεσης διαπιστευτήρια κατά τη διάρκεια της απομίμησης. Εάν ο χρήστης απομίμησης έχει ένα σταθερό προφίλ, τη γραμμή σύνδεσης δεν φορτώνει το προφίλ περιαγωγής. Για περισσότερες πληροφορίες σχετικά με τα πρόσθετα δικαιώματα που απαιτούνται για να χρησιμοποιήσετε αυτήν την παράμετρο, ανατρέξτε στην ενότητα [Πρόσθετες ρυθμίσεις παραμέτρων για απομίμησης](#additional-configuration-for-impersonation).
Τύπος σύνδεσης όταν μιμείται | Κανένας | Τύπος σύνδεσης κατά τη διάρκεια της απομίμησης. Για περισσότερες πληροφορίες, ανατρέξτε στην το [dwLogonType] [ dw] τεκμηρίωση.
Μόνο συνδεδεμένος δέσμες ενεργειών | FALSE | Εάν είναι true, η γραμμή σύνδεσης του Windows PowerShell επαληθεύει ότι κάθε δέσμη ενεργειών έχει μια έγκυρη ψηφιακή υπογραφή. Εάν είναι false, βεβαιωθείτε ότι η πολιτική εκτέλεσης του Windows PowerShell του διακομιστή υπηρεσίας συγχρονισμού είναι RemoteSigned ή χωρίς περιορισμό.

**Κοινές λειτουργική μονάδα**  
Η γραμμή σύνδεσης σάς επιτρέπει να αποθηκεύετε μια κοινόχρηστη μονάδα Windows PowerShell στη ρύθμιση παραμέτρων. Όταν η γραμμή σύνδεσης εκτελεί μια δέσμη ενεργειών, τη λειτουργική μονάδα Windows PowerShell παραλαμβάνεται στο σύστημα αρχείων, ώστε να μπορεί να εισαχθεί από κάθε δέσμη ενεργειών.

Για εισαγωγή, εξαγωγή και συγχρονισμό κωδικού πρόσβασης δέσμες ενεργειών, της κοινής λειτουργικής μονάδας παραλαμβάνεται φάκελο MAData τη γραμμή σύνδεσης. Για το σχήμα, επικύρωσης, ιεραρχία και διαμερίσματα εντοπισμού οι δέσμες ενεργειών, της κοινής λειτουργικής μονάδας εξάγεται το φάκελο % TEMP %. Και στις δύο περιπτώσεις, η που έχουν εξαχθεί δέσμη ενεργειών κοινές λειτουργική μονάδα ονομάζεται σύμφωνα με τη ρύθμιση κοινό όνομα δέσμης ενεργειών λειτουργική μονάδα.

Για να φορτώσετε μια λειτουργική μονάδα που ονομάζεται FIMPowerShellConnectorModule.psm1 από το φάκελο MAData, χρησιμοποιήστε την ακόλουθη πρόταση:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Για να φορτώσετε μια λειτουργική μονάδα που ονομάζεται FIMPowerShellConnectorModule.psm1 από το φάκελο % TEMP %, χρησιμοποιήστε την ακόλουθη πρόταση:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Παράμετρος επικύρωσης**  
Δέσμη ενεργειών επικύρωσης είναι μια προαιρετική δέσμη ενεργειών του Windows PowerShell που μπορούν να χρησιμοποιηθούν για να βεβαιωθείτε ότι είναι έγκυρες παράμετροι σύνδεσης που παρέχονται από το διαχειριστή. Κατά την επαλήθευση διακομιστή, τα διαπιστευτήρια σύνδεσης και παράμετροι συνδεσιμότητας είναι κοινές χρήσεις της δέσμης ενεργειών επικύρωσης. Δέσμη ενεργειών επικύρωσης ονομάζεται μετά από τις παρακάτω καρτέλες και παράθυρα διαλόγου τροποποιούνται:

- Συνδεσιμότητα
- Καθολικές παραμέτρους
- Ρύθμιση παραμέτρων partition

Δέσμη ενεργειών επικύρωσης λαμβάνει τις ακόλουθες παραμέτρους από τη γραμμή σύνδεσης:

Όνομα | Τύπος δεδομένων | Περιγραφή
--- | --- | ---
ConfigParameterPage | [ConfigParameterPage][cpp] | Η καρτέλα ρύθμιση παραμέτρων ή στο παράθυρο διαλόγου που την ενεργοποίησε την αίτηση επικύρωσης.
ConfigParameters | [KeyedCollection] [ keyk] [συμβολοσειρά, [ConfigParameter][cp]] | Πίνακας με παραμέτρους για τη σύνδεση.
Διαπιστευτήρια | [PSCredential][pscred] | Περιέχει τα διαπιστευτήρια που εισάγονται από το διαχειριστή της καρτέλας συνδεσιμότητας.

Δέσμη ενεργειών επικύρωσης πρέπει να επιστρέψετε σε ένα αντικείμενο ParameterValidationResult τη διαδικασία.

**Εντοπισμός σχήματος**  
Η δέσμη ενεργειών εντοπισμού σχήματος είναι υποχρεωτικό. Αυτή η δέσμη ενεργειών επιστρέφει το τύποι αντικειμένων, χαρακτηριστικά και χαρακτηριστικό περιορισμούς που χρησιμοποιεί την υπηρεσία συγχρονισμού όταν ρυθμίζετε τις παραμέτρους κανόνες ροής χαρακτηριστικό. Η δέσμη ενεργειών εντοπισμού σχήματος εκτελείται κατά τη δημιουργία της σύνδεσης και συμπληρώνει τη γραμμή σύνδεσης σχήματος. Χρησιμοποιείται επίσης από την ενέργεια Ανανέωση σχήματος στη Διαχείριση υπηρεσίας συγχρονισμού.

Η δέσμη ενεργειών εντοπισμού σχήματος λαμβάνει τις ακόλουθες παραμέτρους από τη γραμμή σύνδεσης:

Όνομα | Τύπος δεδομένων | Περιγραφή
--- | --- | ---
ConfigParameters | [KeyedCollection] [ keyk] [συμβολοσειρά, [ConfigParameter][cp]] | Πίνακας με παραμέτρους για τη σύνδεση.
Διαπιστευτήρια | [PSCredential][pscred] | Περιέχει τα διαπιστευτήρια που εισάγονται από το διαχειριστή της καρτέλας συνδεσιμότητας.

Η δέσμη ενεργειών πρέπει να επιστρέφει ένα μεμονωμένο [σχήμα] [ schema] αντικειμένου για τη διαδικασία. Το αντικείμενο σχήματος αποτελείται από [SchemaType] [ schemaT] αντικείμενα που αναπαριστά τύποι αντικειμένων (για παράδειγμα: χρήστες και ομάδες). Το αντικείμενο SchemaType περιλαμβάνει μια συλλογή [SchemaAttribute] [ schemaA] αντικείμενα που αντιπροσωπεύουν τα χαρακτηριστικά (για παράδειγμα: όνομα, επώνυμο και ταχυδρομική διεύθυνση) του τύπου.

**Πρόσθετες παραμέτρους**  
Εκτός από τις ρυθμίσεις παραμέτρων τυπικό, μπορείτε να ορίσετε πρόσθετες προσαρμοσμένες ρυθμίσεις παραμέτρων που σχετίζονται με την παρουσία της γραμμής σύνδεσης. Αυτές οι παράμετροι μπορεί να οριστεί με τη γραμμή σύνδεσης, διαμερίσματα, ή εκτέλεση βήμα επίπεδα και έχετε πρόσβαση σε αυτές από τη σχετική δέσμη ενεργειών του Windows PowerShell. Προσαρμοσμένη ρύθμιση παραμέτρων ρυθμίσεων μπορούν να αποθηκευτούν στη βάση δεδομένων υπηρεσίας συγχρονισμού σε μορφή απλού κειμένου ή μπορεί να είναι η κρυπτογραφημένα. Κρυπτογραφεί αυτόματα την υπηρεσία συγχρονισμού και αποκρυπτογραφεί ασφαλούς ρυθμίσεις παραμέτρων όταν απαιτείται.

Για να καθορίσετε ρυθμίσεις προσαρμοσμένη ρύθμιση παραμέτρων, διαχωρίστε το όνομα του κάθε παράμετρο με ένα κόμμα (,).

Για να αποκτήσετε πρόσβαση σε προσαρμοσμένη ρύθμιση παραμέτρων ρυθμίσεων από μια δέσμη ενεργειών, πρέπει να επίθημα το όνομα με το χαρακτήρα υπογράμμισης ( \_ ) και το εύρος της παραμέτρου (Global, διαμερίσματα ή RunStep). Για παράδειγμα, για πρόσβαση την παράμετρο καθολικού όνομα αρχείου, χρησιμοποιήστε το τμήμα κώδικα:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Δυνατότητες
Στην καρτέλα δυνατότητες του προγράμματος σχεδίασης παράγοντας διαχείρισης ορίζει τη συμπεριφορά και τις λειτουργίες της γραμμής σύνδεσης. Οι επιλογές που κάνατε σε αυτή την καρτέλα δεν μπορεί να τροποποιηθεί, αφού δημιουργηθεί η γραμμή σύνδεσης. Αυτός ο πίνακας παραθέτει τις ρυθμίσεις δυνατοτήτων.

![Δυνατότητες](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

Η δυνατότητα | Περιγραφή |
--- | --- |
[Αποκλειστικό όνομα στυλ][dnstyle] | Υποδεικνύει εάν η γραμμή σύνδεσης υποστηρίζει αποκλειστικό ονόματα και επομένως, τι στυλ.
[Τύπος εξαγωγής][exportT] | Καθορίζει τον τύπο των αντικειμένων που παρουσιάζονται στη δέσμη ενεργειών εξαγωγής. <li>AttributeReplace – περιλαμβάνει το πλήρες σύνολο των τιμών για ένα χαρακτηριστικό πολλών τιμών, όταν αλλάζει το χαρακτηριστικό.</li><li>AttributeUpdate – περιλαμβάνει μόνο τα δέλτα σε ένα χαρακτηριστικό πολλών τιμών, όταν αλλάζει το χαρακτηριστικό.</li><li>MultivaluedReferenceAttributeUpdate - περιλαμβάνει ένα πλήρες σύνολο των τιμών για μη-αναφορά με πολλές τιμές χαρακτηριστικά και μόνο δέλτα για χαρακτηριστικά αναφορά με πολλές τιμές.</li><li>ObjectReplace – περιλαμβάνει όλα τα χαρακτηριστικά για ένα αντικείμενο όταν οποιαδήποτε χαρακτηριστικών αλλαγές</li>
[Κανονικοποίηση δεδομένων][DataNorm] | Καθοδηγεί της υπηρεσίας συγχρονισμού θα κανονικοποιηθεί χαρακτηριστικά αγκύρωσης πριν παρέχονται δεσμών ενεργειών.
[Αντικείμενο επιβεβαίωσης][oconf] | Ρυθμίζει τις παραμέτρους της συμπεριφοράς σε εκκρεμότητα εισαγωγή στην υπηρεσία συγχρονισμού. <li>Κανονική – προεπιλεγμένη συμπεριφορά που αναμένεται από όλα τα εξαγόμενα αλλαγές θα επιβεβαιωθεί μέσω εισαγωγής</li><li>NoDeleteConfirmation – όταν διαγράφεται ένα αντικείμενο, δεν υπάρχει καμία εισαγωγή σε εκκρεμότητα που δημιουργούνται.</li><li>NoAddAndDeleteConfirmation – όταν ένα αντικείμενο έχει δημιουργηθεί ή διαγραφεί, δεν υπάρχει καμία εισαγωγή σε εκκρεμότητα που δημιουργούνται.</li>
Χρήση DN ως αγκύρωσης | Εάν το αποκλειστικό όνομα στυλ έχει οριστεί σε LDAP, το χαρακτηριστικό αγκύρωσης για το χώρο σύνδεσης είναι επίσης το αποκλειστικό όνομα.
Ταυτόχρονες λειτουργίες του πολλές γραμμές σύνδεσης | Όταν είναι επιλεγμένο, να εκτελέσετε ταυτόχρονα πολλές γραμμές σύνδεσης του Windows PowerShell.
Τα διαμερίσματα | Όταν είναι επιλεγμένο, η γραμμή σύνδεσης υποστηρίζει πολλές διαμερίσματα και εντοπισμού διαμερίσματα.
Ιεραρχία | Όταν είναι επιλεγμένο, η γραμμή σύνδεσης υποστηρίζει μια ιεραρχική δομή στυλ LDAP.
Ενεργοποίηση της εισαγωγής | Όταν είναι επιλεγμένο, η γραμμή σύνδεσης εισάγει δεδομένα μέσω εισαγωγής δεσμών ενεργειών.
Ενεργοποίηση της εισαγωγής Delta | Όταν είναι επιλεγμένο, η γραμμή σύνδεσης να ζητήσετε δέλτα από τις δέσμες ενεργειών εισαγωγής.
Ενεργοποίηση εξαγωγής | Όταν είναι επιλεγμένο, η γραμμή σύνδεσης εξάγει δεδομένα μέσω εξαγωγής δεσμών ενεργειών.
Ενεργοποίηση πλήρους εξαγωγής | Όταν είναι επιλεγμένο, οι δέσμες ενεργειών εξαγωγής υποστηρίξει το διάστημα ολόκληρη γραμμή σύνδεσης. Για να χρησιμοποιήσετε αυτήν την επιλογή, ενεργοποίηση εξαγωγή πρέπει επίσης να ελέγχεται.
Χωρίς αναφορά τιμές στην πρώτη εξαγωγή φάση | Όταν είναι επιλεγμένο, εξάγονται χαρακτηριστικά αναφορά σε μια δεύτερη φάση εξαγωγή.
Ενεργοποίηση Μετονομασία αντικειμένου | Όταν είναι επιλεγμένο, αποκλειστικό ονομάτων μπορεί να τροποποιηθεί.
Διαγραφή-Προσθήκη ως αντικατάσταση | Όταν είναι επιλεγμένο, διαγραφή-Προσθήκη λειτουργίες εξάγονται ως μεμονωμένο αντικατάσταση.
Ενεργοποίηση λειτουργιών κωδικού πρόσβασης | Όταν είναι επιλεγμένο, υποστηρίζονται δέσμες ενεργειών συγχρονισμού τον κωδικό πρόσβασης.
Ενεργοποίηση κωδικού πρόσβασης εξαγωγή στην πρώτη φάση | Όταν είναι επιλεγμένο, τους κωδικούς πρόσβασης ορίσετε κατά την προμήθεια εξάγονται όταν δημιουργείται το αντικείμενο.

### <a name="global-parameters"></a>Καθολικές παραμέτρους
Στην καρτέλα παράμετροι καθολικού στη σχεδίαση παράγοντας διαχείρισης σάς επιτρέπει να ρυθμίσετε τις παραμέτρους των δεσμών ενεργειών του Windows PowerShell που εκτελούνται με τη γραμμή σύνδεσης. Μπορείτε επίσης να ρυθμίσετε καθολικού τιμές για τις ρυθμίσεις προσαρμοσμένη ρύθμιση παραμέτρων που ορίζονται στην καρτέλα συνδεσιμότητας.

**Εντοπισμός partition**  
Ένα διαμερίσματα είναι ένα νέο χώρο ονομάτων μέσα σε ένα κοινόχρηστο σχήματα. Για παράδειγμα, στην υπηρεσία καταλόγου Active Directory κάθε τομέας είναι ένα διαμερίσματα μέσα σε ένα σύμπλεγμα δομών. Ένα διαμερίσματα είναι η λογική ομαδοποίηση για εισαγωγή και εξαγωγή λειτουργίες. Εισαγωγή και εξαγωγή έχουν διαμερίσματα με ένα περιβάλλον και όλες οι λειτουργίες θα συμβεί σε αυτό το περιβάλλον. Τα διαμερίσματα είναι πρέπει να αντιπροσωπεύει μια ιεραρχία στο LDAP. Το αποκλειστικό όνομα από ένα διαμερίσματα χρησιμοποιείται σε εισαγωγή για να βεβαιωθείτε ότι όλα τα αντικείμενα που επιστρέφεται είναι εντός της εμβέλειας ένα διαμερίσματα. Το αποκλειστικό όνομα partition χρησιμοποιείται επίσης κατά την προμήθεια από το metaverse στο χώρο σύνδεσης για να προσδιορίσετε τα διαμερίσματα πρέπει να σχετίζονται με ένα αντικείμενο κατά την εξαγωγή.

Η δέσμη ενεργειών εντοπισμού partition λαμβάνει τις ακόλουθες παραμέτρους από τη γραμμή σύνδεσης:

Όνομα | Τύπος δεδομένων | Περιγραφή
--- | --- | ---
ConfigParameters  | [KeyedCollection][keyk][συμβολοσειρά, [ConfigParameter][cp]] | Πίνακας με παραμέτρους για τη σύνδεση.
Διαπιστευτήρια | [PSCredential][pscred] | Περιέχει τα διαπιστευτήρια που εισάγονται από το διαχειριστή της καρτέλας συνδεσιμότητας.

Η δέσμη ενεργειών πρέπει να επιστρέφει μια είτε ένα [διαμερίσματα] [ part] αντικείμενο ή μια λίστα [T] Partition αντικειμένων για τη διαδικασία.

**Ιεραρχία εντοπισμού**  
Η δέσμη ενεργειών εντοπισμού ιεραρχία χρησιμοποιείται μόνο όταν η δυνατότητα αποκλειστικό όνομα στυλ είναι LDAP. Η δέσμη ενεργειών χρησιμοποιείται για να σας επιτρέπει να περιηγηθείτε και επιλέξτε ένα σύνολο κοντέινερ που θεωρείται ή σμίκρυνση εμβέλειας για εισαγωγή και εξαγωγή λειτουργίες. Η δέσμη ενεργειών πρέπει να παρέχει μόνο μια λίστα με τους κόμβους που είναι άμεσα θυγατρικές του ριζικού κόμβου που παρέχονται στη δέσμη ενεργειών.

Η δέσμη ενεργειών εντοπισμού ιεραρχία λαμβάνει τις ακόλουθες παραμέτρους από τη γραμμή σύνδεσης:

Όνομα | Τύπος δεδομένων | Περιγραφή
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][συμβολοσειρά, [ConfigParameter][cp]] | Πίνακας με παραμέτρους για τη σύνδεση.
Διαπιστευτήρια | [PSCredential][pscred] | Περιέχει τα διαπιστευτήρια που εισάγονται από το διαχειριστή της καρτέλας συνδεσιμότητας.
ParentNode | [HierarchyNode][hn] | Ο κόμβος ρίζας της ιεραρχίας βάσει των οποίων η δέσμη ενεργειών πρέπει να επιστρέψει άμεση θυγατρικά στοιχεία.

Η δέσμη ενεργειών πρέπει να επιστρέφει μια είτε ένα μεμονωμένο θυγατρικό HierarchyNode αντικείμενο ή μια λίστα [T] εξαρτημένα HierarchyNode αντικείμενα για τη διαδικασία.

#### <a name="import"></a>Εισαγωγή
Γραμμές σύνδεσης που υποστηρίζουν λειτουργίες εισαγωγής πρέπει να υλοποιήσετε τρεις δεσμών ενεργειών.

**Έναρξη εισαγωγής**  
Εκτελείται η δέσμη ενεργειών εισαγωγής έναρξης στην αρχή της ένα βήμα Εκτέλεση εισαγωγής. Στη διάρκεια αυτού του βήματος, μπορείτε να δημιουργούν μια σύνδεση με το σύστημα και να κάνετε βήματα προετοιμασίας πριν από την εισαγωγή δεδομένων από το συνδεδεμένο σύστημα.

Η δέσμη ενεργειών εισαγωγής ξεκινήστε να λαμβάνει τις ακόλουθες παραμέτρους από τη γραμμή σύνδεσης:

Όνομα | Τύπος δεδομένων | Περιγραφή
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][συμβολοσειρά, [ConfigParameter][cp]] | Πίνακας με παραμέτρους για τη σύνδεση.
Διαπιστευτήρια | [PSCredential][pscred] | Περιέχει τα διαπιστευτήρια που εισάγονται από το διαχειριστή της καρτέλας συνδεσιμότητας.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Ενημερώνει τη δέσμη ενεργειών σχετικά με τον τύπο του partition εισαγωγής Εκτέλεση (delta ή πλήρης έκδοση), ιεραρχίας, υδατογράφημα και μέγεθος σελίδας αναμενόμενο.
Τύποι | [Σχήμα][schema] | Σχήμα για το χώρο σύνδεσης που εισάγεται.

Η δέσμη ενεργειών πρέπει να επιστρέφει μια μεμονωμένη [OpenImportConnectionResults] [ oicres] αντικείμενο για τη διαδικασία, για παράδειγμα:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Εισαγωγή δεδομένων**  
Η δέσμη ενεργειών εισαγωγής δεδομένων ονομάζεται με τη γραμμή σύνδεσης μέχρι τη δέσμη ενεργειών σημαίνει ότι δεν υπάρχουν άλλα δεδομένα για την εισαγωγή. Η γραμμή σύνδεσης του Windows PowerShell έχει μέγεθος σελίδας 9.999 αντικειμένων. Εάν η δέσμη ενεργειών επιστρέφει περισσότερα από 9.999 αντικειμένων για εισαγωγή, πρέπει να υποστηρίζει σελιδοποίησης. Η γραμμή σύνδεσης εκθέτει ονομάζεται μια ιδιότητα προσαρμοσμένα δεδομένα που μπορείτε να χρησιμοποιήσετε σε ένα χώρο αποθήκευσης ένα υδατογράφημα, έτσι ώστε κάθε φορά που η δέσμη ενεργειών εισαγωγής δεδομένων, δέσμη ενεργειών σας συνεχίζει εισαγωγή αντικειμένων όπου σταματήσατε.

Η δέσμη ενεργειών εισαγωγής δεδομένων λαμβάνει τις ακόλουθες παραμέτρους από τη γραμμή σύνδεσης:

Όνομα | Τύπος δεδομένων | Περιγραφή
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][συμβολοσειρά, [ConfigParameter][cp]] | Πίνακας με παραμέτρους για τη σύνδεση.
Διαπιστευτήρια | [PSCredential][pscred] | Περιέχει τα διαπιστευτήρια που εισάγονται από το διαχειριστή της καρτέλας συνδεσιμότητας.
GetImportEntriesRunStep | [ImportRunStep][irs] | Διατηρεί το υδατογράφημα (CustomData) που μπορούν να χρησιμοποιηθούν κατά τη διάρκεια σελίδων εισαγωγές και εισάγει delta.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Ενημερώνει τη δέσμη ενεργειών σχετικά με τον τύπο του partition εισαγωγής Εκτέλεση (delta ή πλήρης έκδοση), ιεραρχίας, υδατογράφημα και μέγεθος σελίδας αναμενόμενο.
Τύποι | [Σχήμα][schema] | Σχήμα για το χώρο σύνδεσης που εισάγεται.

Η δέσμη ενεργειών εισαγωγής δεδομένων πρέπει να συντάξετε μια λίστα [[CSEntryChange][csec]] αντικειμένου για τη διαδικασία. Αυτή η συλλογή αποτελείται από CSEntryChange χαρακτηριστικά που αντιπροσωπεύουν κάθε αντικείμενο που εισάγεται. Κατά την εκτέλεση μιας πλήρους εισαγωγής, αυτή η συλλογή θα πρέπει να έχετε ένα πλήρες σύνολο CSEntryChange αντικείμενα στα οποία έχουν όλα τα χαρακτηριστικά για κάθε αντικείμενο. Κατά την εισαγωγή Delta το αντικείμενο CSEntryChange πρέπει να περιέχει είτε το χαρακτηριστικό επιπέδου δέλτα για κάθε αντικείμενο για να εισαγάγετε, ή μια ολοκληρωμένη αναπαράσταση των αντικειμένων που έχουν αλλάξει (λειτουργία αντικατάστασης).

**Τέλος εισαγωγής**  
Μετά την ολοκλήρωση της εισαγωγής εκτέλεση, εκτελείται η δέσμη ενεργειών εισαγωγή τέλος. Αυτή η δέσμη ενεργειών θα πρέπει να εκτελέσετε εκκαθάριση εργασίες είναι απαραίτητο (για παράδειγμα, Κλείσιμο συνδέσεις σε συστήματα) και να απαντάτε αποτυχίες.

Η δέσμη ενεργειών εισαγωγής τέλος λαμβάνει τις ακόλουθες παραμέτρους από τη γραμμή σύνδεσης:

Όνομα | Τύπος δεδομένων | Περιγραφή
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][συμβολοσειρά, [ConfigParameter][cp]] | Πίνακας με παραμέτρους για τη σύνδεση.
Διαπιστευτήρια | [PSCredential][pscred] | Περιέχει τα διαπιστευτήρια που εισάγονται από το διαχειριστή της καρτέλας συνδεσιμότητας.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Ενημερώνει τη δέσμη ενεργειών σχετικά με τον τύπο του partition εισαγωγής Εκτέλεση (delta ή πλήρης έκδοση), ιεραρχίας, υδατογράφημα και μέγεθος σελίδας αναμενόμενο.
CloseImportConnectionRunStep | [CloseImportConnectionRunStep][cecrs] | Σχετικά με το λόγο που έχει λήξει η εισαγωγή που σας ενημερώνει τη δέσμη ενεργειών.

Η δέσμη ενεργειών πρέπει να επιστρέφει μια μεμονωμένη [CloseImportConnectionResults] [ cicres] αντικείμενο για τη διαδικασία, για παράδειγμα:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Εξαγωγή
Ίδια με την εισαγωγή αρχιτεκτονική της γραμμής σύνδεσης, γραμμές σύνδεσης που υποστηρίζει την εξαγωγή πρέπει να υλοποιήσετε τρεις δεσμών ενεργειών.

**Ξεκινήστε εξαγωγής**  
Εκτελείται η δέσμη ενεργειών εξαγωγής έναρξης στην αρχή της ένα βήμα εξαγωγή εκτέλεση. Σε αυτό το βήμα, μπορείτε να δημιουργούν μια σύνδεση με το σύστημα προέλευσης και διεξαγωγή όλα τα βήματα προετοιμασίας πριν από την εξαγωγή δεδομένων στο συνδεδεμένο σύστημα.

Η έναρξη δέσμης ενεργειών εξαγωγής λαμβάνει τις ακόλουθες παραμέτρους από τη γραμμή σύνδεσης:

Όνομα | Τύπος δεδομένων | Περιγραφή
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][συμβολοσειρά, [ConfigParameter][cp]] | Πίνακας με παραμέτρους για τη σύνδεση.
Διαπιστευτήρια | [PSCredential][pscred] | Περιέχει τα διαπιστευτήρια που εισάγονται από το διαχειριστή της καρτέλας συνδεσιμότητας.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Ενημερώνει τη δέσμη ενεργειών σχετικά με τον τύπο των διαμερισμάτων εξαγωγή Εκτέλεση (delta ή πλήρης έκδοση), ιεραρχία και μέγεθος σελίδας αναμενόμενο.
Τύποι | [Σχήμα][schema] | Σχήμα για το χώρο σύνδεσης που εξάγεται.

Η δέσμη ενεργειών δεν θα πρέπει να επιστρέφει κανένα αποτέλεσμα για να τη διαδικασία.

**Εξαγωγή δεδομένων**  
Η υπηρεσία συγχρονισμού κλήσεις της δέσμης ενεργειών εξαγωγής δεδομένων όσες φορές είναι απαραίτητο για την επεξεργασία κάθε εξαγωγή σε εκκρεμότητα. Εάν το διάστημα σύνδεσης έχει περισσότερες σε εκκρεμότητα εξαγωγές από τη γραμμή σύνδεσης μέγεθος σελίδας, το δέσμης ενεργειών εξαγωγής δεδομένων μπορεί να ονομάζεται πολλές φορές και πιθανώς πολλές φορές για το ίδιο αντικείμενο.

Το δέσμης ενεργειών εξαγωγής δεδομένων λαμβάνει τις ακόλουθες παραμέτρους από τη γραμμή σύνδεσης:

Όνομα | Τύπος δεδομένων | Περιγραφή
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][συμβολοσειρά, [ConfigParameter][cp]] | Πίνακας με παραμέτρους για τη σύνδεση.
Διαπιστευτήρια | [PSCredential][pscred] | Περιέχει τα διαπιστευτήρια που εισάγονται από το διαχειριστή της καρτέλας συνδεσιμότητας.
CSEntries | IList[CSEntryChange][csec] | Λίστα με όλα τα σύνδεσης χώρο αντικείμενα με σε εκκρεμότητα εξαγωγές προς επεξεργασία κατά τη διάρκεια αυτής της φάσης.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Ενημερώνει τη δέσμη ενεργειών σχετικά με τον τύπο του partition εξαγωγή Εκτέλεση (delta ή πλήρης έκδοση), ιεραρχία και μέγεθος σελίδας αναμενόμενο.
Τύποι | [Σχήμα][schema] | Σχήμα για το χώρο σύνδεσης που εξάγεται.

Το δέσμης ενεργειών εξαγωγής δεδομένων πρέπει να επιστρέφει μια [PutExportEntriesResults] [ peeres] αντικειμένου για τη διαδικασία. Αυτό το αντικείμενο δεν χρειάζεται να συμπεριλάβετε πληροφορίες των αποτελεσμάτων για κάθε γραμμή σύνδεσης που έχουν εξαχθεί, εκτός εάν παρουσιαστεί σφάλμα ή μια αλλαγή στο χαρακτηριστικό αγκύρωσης. Για παράδειγμα, για να επαναφέρετε ένα αντικείμενο PutExportEntriesResults της διοχέτευσης:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Εξαγωγή λήξης**  
Μετά την ολοκλήρωση της την εκτέλεση, εξαγωγή της δέσμης ενεργειών εξαγωγής τέλος για να εκτελέσετε. Αυτή η δέσμη ενεργειών θα πρέπει να εκτελέσετε εκκαθάριση εργασίες είναι απαραίτητο (για παράδειγμα, Κλείσιμο συνδέσεις σε συστήματα) και να απαντάτε αποτυχίες.

Το τέλος δέσμης ενεργειών εξαγωγής λαμβάνει τις ακόλουθες παραμέτρους από τη γραμμή σύνδεσης:

Όνομα | Τύπος δεδομένων | Περιγραφή
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][συμβολοσειρά, [ConfigParameter][cp]] | Πίνακας με παραμέτρους για τη σύνδεση.
Διαπιστευτήρια | [PSCredential][pscred] | Περιέχει τα διαπιστευτήρια που εισάγονται από το διαχειριστή της καρτέλας συνδεσιμότητας.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Ενημερώνει τη δέσμη ενεργειών σχετικά με τον τύπο του partition εξαγωγή Εκτέλεση (delta ή πλήρης έκδοση), ιεραρχία και μέγεθος σελίδας αναμενόμενο.
CloseExportConnectionRunStep | [CloseExportConnectionRunStep][cecrs] | Ενημερώνει τη δέσμη ενεργειών σχετικά με το λόγο για τον η εξαγωγή έχει λήξει.

Η δέσμη ενεργειών δεν θα πρέπει να επιστρέφει κανένα αποτέλεσμα για να τη διαδικασία.

#### <a name="password-synchronization"></a>Συγχρονισμός κωδικού πρόσβασης
Γραμμές σύνδεσης του Windows PowerShell μπορεί να χρησιμοποιηθεί ως προορισμός για αλλαγές/επαναφορά κωδικών πρόσβασης.

Η δέσμη ενεργειών του κωδικού πρόσβασης λαμβάνει τις ακόλουθες παραμέτρους από τη γραμμή σύνδεσης:

Όνομα | Τύπος δεδομένων | Περιγραφή
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][συμβολοσειρά, [ConfigParameter][cp]] | Πίνακας με παραμέτρους για τη σύνδεση.
Διαπιστευτήρια | [PSCredential][pscred] | Περιέχει τα διαπιστευτήρια που εισάγονται από το διαχειριστή της καρτέλας συνδεσιμότητας.
Partition | [Partition][part] | Διαμερίσματα καταλόγου που είναι το CSEntry στο.
CSEntry | [CSEntry][cse] | Καταχώρηση χώρου Connector για το αντικείμενο που έχει λάβει μια αλλαγή κωδικού πρόσβασης ή επαναφορά.
OperationType | Συμβολοσειρά | Υποδεικνύει εάν η λειτουργία είναι μια επαναφορά (**SetPassword**) ή αλλαγή (**η Αλλαγή_κωδικού_πρόσβασης**).
PasswordOptions | [PasswordOptions][pwdopt] | Σημαίες που καθορίζουν τον κωδικό πρόσβασης προβλεπόμενο επαναφορά συμπεριφορά. Η παράμετρος αυτή είναι διαθέσιμη μόνο εάν OperationType είναι **SetPassword**.
Παλιός κωδικός πρόσβασης | Συμβολοσειρά | Συμπληρωμένο με το αντικείμενο παλιό κωδικό πρόσβασης για αλλαγές του κωδικού πρόσβασης. Η παράμετρος αυτή είναι διαθέσιμη μόνο εάν OperationType είναι **η Αλλαγή_κωδικού_πρόσβασης**.
Νέος κωδικός πρόσβασης | Συμβολοσειρά | Συμπληρωμένο με το αντικείμενο νέο κωδικό πρόσβασης που πρέπει να ορίσετε τη δέσμη ενεργειών.

Η δέσμη ενεργειών του κωδικού πρόσβασης δεν αναμένεται για να επιστρέψετε κανένα αποτέλεσμα της διοχέτευσης του Windows PowerShell. Εάν προκύψει σφάλμα στη δέσμη ενεργειών τον κωδικό πρόσβασης, η δέσμη ενεργειών θα πρέπει να δημιουργήσει μία από τις εξής εξαιρέσεις για να σας ενημερώσει για την υπηρεσία συγχρονισμού σχετικά με το πρόβλημα:

- [PasswordPolicyViolationException] [ pwdex1] –, εάν ο κωδικός πρόσβασης δεν πληροί την πολιτική στο συνδεδεμένο σύστημα.
- [PasswordIllFormedException] [ pwdex2] –, εάν ο κωδικός πρόσβασης δεν είναι αποδεκτή για το συνδεδεμένο σύστημα.
- [PasswordExtension] [ pwdex3] – δημιουργήθηκε για όλα τα άλλα σφάλματα στη δέσμη ενεργειών τον κωδικό πρόσβασης.

## <a name="sample-connectors"></a>Δείγμα γραμμών σύνδεσης
Για μια πλήρη επισκόπηση των συνδέσεων διαθέσιμη δείγμα, ανατρέξτε στο θέμα [Windows PowerShell σύνδεσης δείγμα σύνδεσης συλλογής][samp].

## <a name="other-notes"></a>Άλλες σημειώσεις

### <a name="additional-configuration-for-impersonation"></a>Πρόσθετες ρυθμίσεις παραμέτρων για απομίμησης
Παραχωρήστε στο χρήστη που είναι απομίμηση τα ακόλουθα δικαιώματα στο διακομιστή υπηρεσίας συγχρονισμού:

Πρόσβαση για ανάγνωση στα παρακάτω κλειδιά μητρώου:

- HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
- HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

Για να προσδιορίσετε το αναγνωριστικό ασφαλείας (SID) του λογαριασμού της υπηρεσίας συγχρονισμού υπηρεσίας, εκτελέστε τις ακόλουθες εντολές του PowerShell:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Πρόσβαση για ανάγνωση για τους παρακάτω φακέλους αρχείων συστήματος:

- %ProgramFiles%\Microsoft forefront ταυτότητας Manager\2010\Synchronization Service\Extensions
- %ProgramFiles%\Microsoft forefront ταυτότητας Manager\2010\Synchronization Service\ExtensionsCache
- %ProgramFiles%\Microsoft forefront ταυτότητας Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Αντικαταστήστε το όνομα της γραμμής σύνδεσης του Windows PowerShell για το σύμβολο κράτησης θέσης {ConnectorName}.

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

-   Για πληροφορίες σχετικά με τον τρόπο για να ενεργοποιήσετε την καταγραφή για την αντιμετώπιση προβλημάτων στη γραμμή σύνδεσης, ανατρέξτε στο θέμα [πώς μπορείτε να ενεργοποιήσετε την ανίχνευση ETW για γραμμές σύνδεσης](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
