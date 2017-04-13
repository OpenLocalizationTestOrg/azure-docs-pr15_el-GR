<properties
    pageTitle="Αναφορά προγραμματιστών Azure συναρτήσεις | Microsoft Azure"
    description="Κατανόηση συναρτήσεων Azure έννοιες και των στοιχείων που είναι κοινά σε όλες τις γλώσσες και συνδέσεις."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure συναρτήσεις, συναρτήσεις, Επεξεργασία συμβάντος, webhooks, δυναμική υπολογισμού, χωρίς αρχιτεκτονικής"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Αναφορά προγραμματιστών Azure συναρτήσεις

Συναρτήσεις Azure κοινή χρήση μερικές βασικές έννοιες τεχνική και τα στοιχεία, ανεξάρτητα από τη γλώσσα ή μπορείτε να χρησιμοποιήσετε τη σύνδεση. Πριν μεταβείτε σε εκμάθησης λεπτομέρειες ειδικά για μια δεδομένη γλώσσα ή σύνδεσης, φροντίστε να διαβάσετε αυτό επισκόπηση που εφαρμόζεται σε όλες τις.

Σε αυτό το άρθρο προϋποθέτει ότι έχετε ήδη διαβάστε την [Επισκόπηση συναρτήσεων Azure](functions-overview.md) και είστε εξοικειωμένοι με τις [έννοιες WebJobs SDK όπως εναύσματα, συνδέσεις, και το περιβάλλον εκτέλεσης JobHost](../app-service-web/websites-dotnet-webjobs-sdk.md). Συναρτήσεις Azure βασίζεται σε στο SDK WebJobs. 


## <a name="function-code"></a>Συνάρτηση κώδικα

*Συνάρτηση* είναι η κύρια έννοια σε συναρτήσεις Azure. Σύνταξη κώδικα για μια συνάρτηση σε μια γλώσσα της επιλογής σας και αποθηκεύσετε το αρχείο ή τα αρχεία κώδικα και ένα αρχείο ρύθμισης παραμέτρων στον ίδιο φάκελο. Παράμετροι είναι στο JSON, και το όνομα του αρχείου είναι `function.json`. Διάφορες γλώσσες που υποστηρίζονται και κάθε μία έχει ελαφρώς διαφορετικές εμπειρία βελτιστοποιημένη για να λειτουργούν καλύτερα για τη συγκεκριμένη γλώσσα. 

Το `function.json` αρχείο περιέχει ρύθμισης παραμέτρων ειδικά για μια συνάρτηση, καθώς και τις συνδέσεις. Το περιβάλλον εκτέλεσης διαβάζει αυτό το αρχείο για να προσδιορίσετε τα συμβάντα που θα έναυσμα απενεργοποίηση του, των δεδομένων που θα περιλαμβάνονται κατά την κλήση της συνάρτησης και τη θέση για την αποστολή δεδομένων που εισήχθησαν κατά μήκος από την ίδια τη συνάρτηση. 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Μπορείτε να αποτρέψετε το περιβάλλον εκτέλεσης από την εκτέλεση της συνάρτησης, ορίζοντας την `disabled` ιδιότητα που θα `true`.

Το `bindings` η ιδιότητα είναι όπου μπορείτε να πραγματοποιήσετε εναύσματα και συνδέσεις. Κάθε σύνδεση μοιράζεται μερικές κοινές ρυθμίσεις και ορισμένες ρυθμίσεις που σχετίζονται με ένα συγκεκριμένο τύπο σύνδεσης. Κάθε σύνδεση απαιτεί τις ακόλουθες ρυθμίσεις:

|Ιδιότητα|Τιμές/τύπους|Σχόλια|
|---|-----|------|
|`type`|συμβολοσειρά|Τύπος σύνδεσης. Για παράδειγμα, `queueTrigger`.
|`direction`|'στο', 'out'| Υποδηλώνει εάν η σύνδεση είναι για λήψη δεδομένων μέσα στη συνάρτηση ή την αποστολή δεδομένων από τη συνάρτηση.
| `name` | συμβολοσειρά | Το όνομα που θα χρησιμοποιηθεί για το δεσμευμένο δεδομένα στη συνάρτηση. Για C# θα είναι το όνομα του ορίσματος ένα; για JavaScript θα είναι το κλειδί σε μια λίστα κλειδιού/τιμής.

## <a name="function-app"></a>Συνάρτηση εφαρμογής

Μια εφαρμογή συνάρτηση αποτελείται από μία ή περισσότερες συναρτήσεις μεμονωμένα που διαχειρίζεται μαζί Azure εφαρμογής υπηρεσίας. Όλες τις συναρτήσεις σε μια εφαρμογή για συνάρτηση μοιραστείτε το ίδιο πρόγραμμα τιμολόγησης, λόγω των συνεχών ανάπτυξη και έκδοση χρόνου εκτέλεσης. Συναρτήσεις γραμμένο σε πολλές γλώσσες μπορεί να όλοι κάνουν κοινή χρήση της ίδιας εφαρμογής συνάρτηση. Θεωρήστε ότι μια εφαρμογή συνάρτηση είναι ένας τρόπος για να οργανώσετε και να διαχειριστείτε αποτελούν συνολικά τις συναρτήσεις. 

## <a name="runtime-script-host-and-web-host"></a>Κατά το χρόνο εκτέλεσης (κεντρικού υπολογιστή δέσμης ενεργειών και web κεντρικού υπολογιστή)

Το περιβάλλον εκτέλεσης ή κεντρικού υπολογιστή δέσμης ενεργειών, είναι το υποκείμενο κεντρικός υπολογιστής WebJobs SDK που παρακολουθεί για συμβάντα, συλλέγει και στέλνει δεδομένα και ο τελικός εκτελεί τον κωδικό. 

Για να διευκολύνετε εναύσματα HTTP, υπάρχει επίσης μια υπηρεσία παροχής φιλοξενίας web που έχει σχεδιαστεί για να καθίσετε μπροστά από τον κεντρικό υπολογιστή δέσμης ενεργειών σε σενάρια παραγωγής. Αυτό σας βοηθά να απομονώσετε το script host από την κυκλοφορία προσκηνίου που ελέγχονται από τον κεντρικό υπολογιστή του web.

## <a name="folder-structure"></a>Δομή του φακέλου

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Κατά την εγκατάσταση ενός έργου για την ανάπτυξη συναρτήσεων σε μια εφαρμογή συνάρτηση στο Azure εφαρμογής υπηρεσίας, να διαχειρίζεστε αυτήν τη δομή φακέλου ως κώδικα τοποθεσίας. Μπορείτε να χρησιμοποιήσετε υπάρχουσες εργαλεία όπως η συνεχής ενοποίηση και ανάπτυξη ή ανάπτυξη προσαρμοσμένων δέσμες ενεργειών για αυτές τις ενέργειες ανάπτυξη η εγκατάσταση του πακέτου ώρα ή κώδικα transpilation.

>[AZURE.NOTE] Το `wwwroot` φάκελος εδώ είναι όπου θα αναπτύσσονται τα αρχεία σας σε. Ωστόσο, δεν πρέπει να συμπεριλάβετε αυτόν το φάκελο στα αρχεία αναπτύξετε, που μπορεί να οδηγήσει με `wwwroot\wwwroot`. Αντί για αυτό, σας `host.json` φάκελοι αρχείων και συνάρτηση πρέπει να είναι απευθείας στη ρίζα της τι αναπτύξετε.

## <a id="fileupdate"></a>Πώς μπορείτε να ενημερώσετε τα αρχεία εφαρμογής συνάρτηση

Πρόγραμμα επεξεργασίας συνάρτηση ενσωματωμένο στην πύλη του Azure σάς επιτρέπει να ενημερώσετε το αρχείο *function.json* και το αρχείο κώδικα για μια συνάρτηση. Για να στείλετε ή να ενημερώσετε άλλα αρχεία, όπως *package.json* ή *project.json* ή εξαρτήσεις, πρέπει να χρησιμοποιήσετε άλλες μεθόδους ανάπτυξης.

Συνάρτηση εφαρμογές είναι σχεδιασμένες υπηρεσία εφαρμογής, ώστε όλες οι [Επιλογές ανάπτυξης που είναι διαθέσιμες σε εφαρμογές web τυπική](../app-service-web/web-sites-deploy.md) είναι διαθέσιμες για συνάρτηση καθώς και τις εφαρμογές. Εδώ θα βρείτε ορισμένες μεθόδους που μπορείτε να χρησιμοποιήσετε για την αποστολή ή ενημέρωση αρχείων εφαρμογής συνάρτηση. 

#### <a name="to-use-app-service-editor"></a>Για να χρησιμοποιήσετε το πρόγραμμα επεξεργασίας εφαρμογής υπηρεσίας

1. Στην πύλη του Azure συναρτήσεις, κάντε κλικ στην επιλογή **συνάρτησης των ρυθμίσεων της εφαρμογής**.

2. Στην ενότητα **Ρυθμίσεις για προχωρημένους** , κάντε κλικ στην επιλογή **Μετάβαση στις ρυθμίσεις εφαρμογής υπηρεσίας**.

3. Κάντε κλικ στην επιλογή **Εφαρμογή υπηρεσίας επεξεργασίας** στο παράθυρο περιήγησης του μενού εφαρμογής στην περιοχή **ΕΡΓΑΛΕΊΑ ΑΝΆΠΤΥΞΗΣ**.

4.  Κάντε κλικ στο κουμπί **Μετάβαση**.

    Μετά την εφαρμογή υπηρεσίας επεξεργασίας φορτώνει, θα δείτε τους φακέλους αρχείων και συνάρτηση *host.json* στην περιοχή *wwwroot*. 

5. Άνοιγμα αρχείων για να τα επεξεργαστείτε, ή μεταφορά και απόθεση από τον υπολογιστή σας ανάπτυξης για να αποστείλετε αρχεία.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Για να χρησιμοποιήσετε την εφαρμογή της συνάρτησης τελικού σημείου SCM (Kudu)

1. Μεταβείτε στο: `https://<function_app_name>.scm.azurewebsites.net`.

2. Κάντε κλικ στην επιλογή **Εντοπισμός σφαλμάτων κονσόλας > CMD**.

3. Μεταβείτε στο `D:\home\site\wwwroot\` για να ενημερώσετε *host.json* ή `D:\home\site\wwwroot\<function_name>` για να ενημερώσετε μια συνάρτηση αρχεία.

4. Μεταφορά και απόθεση ενός αρχείου που θέλετε να αποστείλετε στον κατάλληλο φάκελο στο πλέγμα του αρχείου. Υπάρχουν δύο περιοχές στο πλέγμα αρχείου όπου μπορείτε να απορρίψετε ένα αρχείο. Για τα αρχεία *.zip* , εμφανίζεται ένα πλαίσιο με την ετικέτα "Σύρετε εδώ για να αποστείλετε και αποσυμπίεση." Για άλλους τύπους αρχείων, αποθέστε το στο πλέγμα αρχείο αλλά έξω από το πλαίσιο "αποσυμπίεση".

#### <a name="to-use-ftp"></a>Για να χρησιμοποιήσετε FTP

1. Ακολουθήστε τις οδηγίες [εδώ](../app-service-web/web-sites-deploy.md#ftp) για να λάβετε FTP έχει ρυθμιστεί.

2. Όταν είστε συνδεδεμένοι στην τοποθεσία εφαρμογών συνάρτηση, αντιγράψτε ένα αρχείο ενημερωμένων *host.json* για να `/site/wwwroot` ή να αντιγράψετε αρχεία συνάρτηση για να `/site/wwwroot/<function_name>`.

#### <a name="to-use-continuous-deployment"></a>Για να χρησιμοποιήσετε συνεχούς ανάπτυξης

Ακολουθήστε τις οδηγίες στο θέμα της [συνεχούς ανάπτυξης για τις συναρτήσεις Azure](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Παράλληλη εκτέλεση

Όταν υπάρχουν πολλά συμβάντα ενεργοποίησης ταχύτερα από ένα περιβάλλον εκτέλεσης μονού νήματος συνάρτηση μπορεί να επεξεργαστεί τους, ο χρόνος εκτέλεσης μπορεί να κλήση συνάρτησης πολλές φορές στο παράλληλα.  Εάν μια εφαρμογή συνάρτηση χρησιμοποιεί το [Πρόγραμμα δυναμικής υπηρεσίας](functions-scale.md#dynamic-service-plan), η συνάρτηση εφαρμογή θα μπορούσε να κλίμακα ανάληψη αυτόματα.  Αν η εφαρμογή εκτελείται σε δυναμικό Σχεδιασμός υπηρεσιών ή μια κανονική [Εφαρμογής υπηρεσίας Σχεδιασμός](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), κάθε παρουσία του την εφαρμογή, η συνάρτηση μπορεί να επεξεργαστεί κλήσεων ταυτόχρονες συνάρτηση παράλληλα με πολλά νήματα.  Ο μέγιστος αριθμός των κλήσεων ταυτόχρονες συνάρτηση σε κάθε περίοδο λειτουργίας εφαρμογής συνάρτηση διαφέρει ανάλογα με το μέγεθος της μνήμης της εφαρμογής συνάρτηση. 

## <a name="azure-functions-pulse"></a>Συναρτήσεις Azure Pulse  

Pulse είναι μια ροή ζωντανή συμβάντων η οποία εμφανίζει πόσο συχνά εκτελείται λειτουργία σας, καθώς και τις επιτυχίες και αποτυχίες. Μπορείτε επίσης να παρακολουθήσετε τις μέσος χρόνος εκτέλεσης. Θα προσθέσουμε περισσότερες δυνατότητες και προσαρμογής με την πάροδο του χρόνου. Μπορείτε να αποκτήσετε πρόσβαση στη σελίδα **Pulse** από την καρτέλα **παρακολούθησης** .

## <a name="repositories"></a>Αποθετήρια

Ο κωδικός για τις συναρτήσεις Azure είναι Άνοιγμα αρχείου προέλευσης και είναι αποθηκευμένα σε αποθετήρια GitHub:

* [Azure συναρτήσεις χρόνου εκτέλεσης](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure πύλη συναρτήσεις](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure συναρτήσεων προτύπων](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Επεκτάσεις Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Συνδέσεις

Ακολουθεί ένας πίνακας με όλες τις συνδέσεις υποστηρίζονται.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Αναφορά προβλημάτων

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στους ακόλουθους πόρους:

* [Azure συναρτήσεις C# αναφορά προγραμματιστών](functions-reference-csharp.md)
* [Azure συναρτήσεις F # αναφορά προγραμματιστών](functions-reference-fsharp.md)
* [Αναφορά προγραμματιστών Azure NodeJS συναρτήσεις](functions-reference-node.md)
* [Azure συναρτήσεις εναύσματα και συνδέσεις](functions-triggers-bindings.md)
* [Azure συναρτήσεις: το ταξίδι](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) στο ιστολόγιο ομάδας του Azure εφαρμογής υπηρεσίας. Ένα ιστορικό των πώς αναπτύχθηκε Azure συναρτήσεις.




