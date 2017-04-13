<properties
    pageTitle="Ανάλυση πλατφόρμες: καταιγίδας Apache σύγκρισης για ανάλυση ροή | Microsoft Azure"
    description="Λάβετε καθοδήγηση επιλέγοντας μια πλατφόρμα ανάλυση cloud, χρησιμοποιώντας μια σύγκριση καταιγίδας Apache για ροή ανάλυσης. Κατανόηση των δυνατοτήτων και διαφορές."
    keywords="ανάλυση πλατφόρμα, πλατφόρμες αναλυτικών στοιχείων, cloud ανάλυση πλατφόρμα, καταιγίδας σύγκρισης"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="help-choosing-a-streaming-analytics-platform-apache-storm-comparison-to-azure-stream-analytics"></a>Βοήθεια επιλέγοντας μια ροή πλατφόρμα ανάλυση: καταιγίδας Apache σύγκρισης για ανάλυση ροή Azure

Λάβετε καθοδήγηση επιλέγοντας μια πλατφόρμα ανάλυση cloud με τη χρήση αυτής της σύγκρισης καταιγίδας Apache για ανάλυση ροή Azure. Κατανόηση περιπτώσεις χρήσης την τιμή όρους της ανάλυσης ροή έναντι καταιγίδας Apache ως μια διαχειριζόμενη υπηρεσία σε Azure HDInsight, ώστε να μπορείτε να επιλέξετε τη σωστή λύση για την επιχείρησή σας.

Και οι δύο πλατφόρμες αναλυτικών στοιχείων παρέχει πλεονεκτήματα μιας λύσης PaaS, υπάρχουν μερικά κύριες δυνατότητες διακριτικό που διαφοροποιήσετε. Δυνατότητες καθώς και τους περιορισμούς από αυτές τις υπηρεσίες παρατίθενται πιο κάτω για να σας βοηθήσει θα μεταβαίνετε σε η λύση που χρειάζεστε για να επιτύχετε τους στόχους σας.

## <a name="storm-comparison-to-stream-analytics-general-features"></a>Έφοδο σύγκρισης για ανάλυση ροή: γενικές δυνατότητες ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Ανάλυση Azure ροής</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Καταιγίδας Apache στην HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Άνοιγμα αρχείου προέλευσης</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Όχι, Azure ροή ανάλυσης είναι μια κοινή Microsoft σας δίνει τη δυνατότητα.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ναι, καταιγίδας Apache είναι μια τεχνολογία Apache με άδεια χρήσης.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Υποστήριξη της Microsoft</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ναι </p>
            </td>
            <td width="246" valign="top">
                <p>
Ναι </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Απαιτήσεις υλικού</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Υπάρχουν απαιτήσεις υλικού. Azure ροή ανάλυσης είναι μια υπηρεσία Azure.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Υπάρχουν απαιτήσεις υλικού. Καταιγίδας Apache είναι μια υπηρεσία Azure.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Επάνω επιπέδου μονάδας</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Με τους πελάτες ανάλυσης ροή Azure ανάπτυξη και παρακολούθηση της ροής εργασιών.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Με καταιγίδας Apache σε πελάτες HDInsight ανάπτυξη και εποπτεία ένα ολόκληρο σύμπλεγμα, που μπορεί να φιλοξενήσει πολλές εργασίες καταιγίδας, καθώς και άλλες φόρτους εργασίας (με δέσμη).
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Τιμή</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ανάλυση ροή είναι σε τιμές από όγκο δεδομένων που επεξεργάζεται και απαιτείται ο αριθμός των μονάδων ροής (την ώρα που εκτελείται η εργασία).
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">Περαιτέρω τις πληροφορίες τιμολόγησης μπορείτε να βρείτε εδώ.</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Για Apache καταιγίδας στην HDInsight, τη μονάδα αγοράς βασίζεται στο σύμπλεγμα και θα χρεωθεί με βάση την ώρα του συμπλέγματος εκτελείται, ανεξάρτητα από τις εργασίες που έχουν αναπτυχθεί.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">Περαιτέρω τις πληροφορίες τιμολόγησης μπορείτε να βρείτε εδώ.</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Σύνταξη από κοινού σε κάθε πλατφόρμα ανάλυσης##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Ανάλυση Azure ροής</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Καταιγίδας Apache στην HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Δυνατότητες: DSL SQL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ναι, μια εύχρηστη υποστήριξη γλώσσας SQL είναι διαθέσιμη.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Όχι, οι χρήστες πρέπει να σύνταξη κώδικα στο Java C# ή χρησιμοποιήστε Trident APIs.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Δυνατότητες: Τελεστές χρονικό</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Παράθυρα συγκεντρώσεις και χρονικό σύνδεσμοι που υποστηρίζονται από το πλαίσιο.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Χρονικό τελεστές πρέπει να εφαρμοστεί από το χρήστη.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Εμπειρία ανάπτυξης</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Αλληλεπιδραστική σύνταξης και εμπειρία μέσω πύλης Azure στο δείγμα δεδομένων εντοπισμού.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ανάπτυξη, εντοπισμού σφαλμάτων και παρακολούθησης εμπειρία παρέχεται μέσω του Visual Studio εμπειρία για τους χρήστες .NET, ενώ για Java και άλλες γλώσσες προγραμματιστές μπορεί να χρησιμοποιεί το IDE της επιλογής τους.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Υποστήριξη εντοπισμού σφαλμάτων</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ανάλυση ροή προσφέρει κατάσταση των βασικών εργασιών και αρχεία καταγραφής λειτουργίες ως ένας τρόπος εντοπισμού, αλλά αυτήν τη στιγμή δεν παρέχει ευελιξία στη τι/πώς ιδιαίτερα συμπεριλαμβάνεται στα αρχεία καταγραφής ie: λεπτομερούς.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Λεπτομερή αρχεία καταγραφής είναι διαθέσιμες για σκοπούς εντοπισμού σφαλμάτων. Υπάρχουν δύο τρόποι για να επιφάνειας αρχεία καταγραφής χρήστη, μέσω του visual Studio ή το χρήστη να RDP σε σύμπλεγμα για την πρόσβαση σε αρχεία καταγραφής.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Υποστήριξη για UDF (ορίζονται από το χρήστη συναρτήσεις)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Προς το παρόν δεν υπάρχει υποστήριξη για UDF.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
UDF μπορούν να εγγραφούν στο C#, Java ή τη γλώσσα της επιλογής σας.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Δυνατότητα επέκτασης - προσαρμοσμένου κώδικα</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Δεν υπάρχει υποστήριξη για extensible κωδικό στο ροή ανάλυσης.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ναι, υπάρχει διαθεσιμότητα για να γράψετε προσαρμοσμένου κώδικα στο C#, Java ή άλλες γλώσσες που υποστηρίζονται στην καταιγίδας.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Προελεύσεις δεδομένων και εκροές##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Ανάλυση Azure ροής</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Καταιγίδας Apache στην HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Προελεύσεις δεδομένων εισαγωγής</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>Τις υποστηριζόμενες προελεύσεις εισόδου είναι Azure συμβάν διανομείς και αντικείμενα BLOB Azure.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Υπάρχουν συνδέσεις που είναι διαθέσιμα για διανομείς συμβάντος, υπηρεσία Bus, Kafka, κ.λπ. Μη υποστηριζόμενες γραμμές σύνδεσης μπορεί να εφαρμοστεί μέσω προσαρμοσμένου κώδικα.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Μορφές εισαγωγής δεδομένων</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Υποστηριζόμενες μορφές εισόδου είναι Avro, JSON, CSV.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Οποιαδήποτε μορφή μπορεί να εφαρμοστεί μέσω προσαρμοσμένου κώδικα.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Εξόδους</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Μια ροή εργασίας μπορεί να έχει πολλές εξόδους. Υποστηριζόμενες εξόδους: Διανομείς Azure συμβάντος, χώρος αποθήκευσης αντικειμένων Blob του Azure, Azure πίνακες, DB Azure SQL και PowerBI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Υποστήριξη για πολλές εξόδους σε μια τοπολογία, κάθε εξόδου μπορεί να έχει προσαρμοσμένη λογική για μετάδοση επεξεργασία. Έξοδος από το πλαίσιο καταιγίδας περιλαμβάνει γραμμές σύνδεσης PowerBI, Azure συμβάν διανομείς, χώρος αποθήκευσης αντικειμένων Blob του Azure, Azure DocumentDB, SQL και HBase. Μη υποστηριζόμενες γραμμές σύνδεσης μπορεί να εφαρμοστεί μέσω προσαρμοσμένου κώδικα.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Μορφές κωδικοποίησης δεδομένων</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ανάλυση ροή απαιτεί μορφή δεδομένων UTF-8 για να χρησιμοποιηθούν.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Οποιαδήποτε μορφή κωδικοποίησης δεδομένων μπορεί να εφαρμοστεί μέσω προσαρμοσμένου κώδικα.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Διαχείριση και λειτουργίες##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Ανάλυση Azure ροής</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Καταιγίδας Apache στην HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Μοντέλο ανάπτυξης έργου</strong>
                </p>
                <p>
                    - <strong>Πύλη του Azure</strong>
                </p>
                <p>
                    - <strong>Visual Studio</strong>
                </p>
                <p>
                    - <strong>PowerShell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ανάπτυξη έχει υλοποιηθεί μέσω Azure πύλη, PowerShell και REST API του Te104090127.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment έχει υλοποιηθεί μέσω Azure πύλη, PowerShell, Visual Studio και APIs ΥΠΌΛΟΙΠΟ.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Παρακολούθηση (στατιστικές, μετρητών, κ.λπ.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Παρακολούθηση έχει υλοποιηθεί μέσω Azure πύλη και REST API του Yammer.
                </p>
                <p>
Ο χρήστης επίσης να ρυθμίσετε τις ειδοποιήσεις Azure.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Παρακολούθηση έχει υλοποιηθεί μέσω του περιβάλλοντος εργασίας Χρήστη καταιγίδας και REST API του Yammer.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Κλιμάκωση</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Αριθμός μονάδων ροής για κάθε εργασία. Κάθε μονάδα ροής επεξεργάζεται έως και 1 MB/s. Max 50 μονάδων από προεπιλογή. Κλήση για να αυξήσετε το όριο.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Αριθμός κόμβους του συμπλέγματος καταιγίδας HDI. Υπάρχει όριο στον αριθμό των κόμβους (που ορίζονται από το όριο του Azure επάνω όριο). Κλήση για να αυξήσετε το όριο.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Περιορισμοί επεξεργασίας δεδομένων</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Οι χρήστες μπορούν να κλίμακα προς τα επάνω ή προς τα κάτω αριθμό μονάδων ροής για να αυξήσετε επεξεργασίας δεδομένων ή βελτιστοποίηση κόστους.
                </p>
                <p>
Κλιμάκωση έως 1 GB/s </p>
            </td>
            <td width="246" valign="top">
                <p>
Χρήστης μπορεί να κλιμακωθεί προς τα επάνω ή προς τα κάτω μέγεθος συμπλέγματος ώστε να ανταποκρίνεται στις ανάγκες.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Διακοπή/βιογραφικού σημειώματος</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Διακοπή και συνέχιση από το τελευταίο σημείο διακοπεί.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Διακοπή και συνέχιση από το τελευταίο τοποθετήσετε σταμάτησε με βάση το υδατογράφημα.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ενημέρωση υπηρεσίας και πλαίσιο</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Αυτόματη ενημέρωση με χωρίς χρόνου εκτός λειτουργίας.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Αυτόματη ενημέρωση με χωρίς χρόνου εκτός λειτουργίας.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Συνεχής λειτουργία μέσω της ιδιαίτερα υπηρεσίας διαθέσιμο με εγγυημένη SLA</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
SLA της συνεχούς 99,9% </p>
                <p>
Η αυτόματη ανάκτηση από αποτυχίες </p>
                <p>
Ανάκτηση της κατάστασης χρονικό τελεστές είναι ενσωματωμένη.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
SLA της συνεχούς 99,9% του συμπλέγματος καταιγίδας. Καταιγίδας Apache είναι μια πλατφόρμα ανοχή σφαλμάτων ροής σφαλμάτων, ωστόσο είναι ευθύνη των πελατών για να βεβαιωθείτε ότι εκτελείται χωρίς διακοπή ροής εργασιών τους.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Δυνατότητες για προχωρημένους##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Ανάλυση Azure ροής</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Καταιγίδας Apache στην HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Καθυστερημένη άφιξης και χειρισμού συμβάντων εκτός λειτουργίας</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ενσωματωμένη με δυνατότητα ρύθμισης παραμέτρων πολιτικών για να αλλάξετε τη σειρά, αποθέστε συμβάντα ή προσαρμόστε ώρα συμβάντος.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Χρήστη πρέπει να υλοποίηση της λογικής να χειρίζεται αυτό το σενάριο.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Δεδομένα αναφοράς</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Τα δεδομένα αναφοράς διαθέσιμη από το Azure αντικείμενα blob με το μέγιστο μέγεθος του 100 MB αναζήτησης στη μνήμη cache. Ανανέωση των δεδομένων αναφοράς γίνεται από την υπηρεσία.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Χωρίς όρια μεγέθους των δεδομένων. Διαθέσιμο για HBase, DocumentDB, SQL Server και Azure γραμμές σύνδεσης. Μη υποστηριζόμενες γραμμές σύνδεσης μπορεί να εφαρμοστεί μέσω προσαρμοσμένου κώδικα.
                </p>
                <p>
Ανανέωση των δεδομένων αναφοράς πρέπει να χειρίζεται προσαρμοσμένου κώδικα.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ενοποίηση με μηχανικής εκμάθησης</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Κατά τη ρύθμιση των παραμέτρων δημοσιευτεί Azure μηχανικής εκμάθησης μοντέλων ως συναρτήσεις κατά τη διάρκεια δημιουργίας εργασία ASA <a href="http://blogs.msdn.com/b/streamanalytics/archive/2015/05/24/real-time-scoring-of-streaming-data-using-machine-learning-models.aspx">(ιδιωτικό preview)</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Διαθέσιμο έως βίδες καταιγίδας.
                </p>
            </td>
        </tr>
    </tbody>
</table>
