<properties
   pageTitle="Προτεινόμενα συνθήκες ονοματοθεσίας για τους πόρους Azure | Καθοδήγηση | Microsoft Azure"
   description="Συνιστάται η συνθήκες ονοματοθεσίας για Azure πόρους. Πώς μπορείτε να ονομάσετε εικονικές μηχανές, λογαριασμούς χώρου αποθήκευσης, δίκτυα, εικονικό δίκτυα, δευτερεύοντα δίκτυα και άλλων οντοτήτων Azure"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2016"
   ms.author="christb"/>
   
# <a name="recommended-naming-conventions-for-azure-resources"></a>Προτεινόμενα συνθήκες ονοματοθεσίας για τους πόρους Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Η επιλογή ενός ονόματος για κάθε πόρο στο Microsoft Azure είναι σημαντικό επειδή:

- Είναι δύσκολο να αλλάξετε ένα όνομα στο μέλλον.
- Ονόματα πρέπει να πληροί τις απαιτήσεις της το συγκεκριμένο τύπο πόρου.

Συνεπείς συμβάσεις ονοματοδοσίας διευκολύνουν τους πόρους για να εντοπίσετε. Επίσης μπορεί να υποδεικνύουν το ρόλο ενός πόρου σε μια λύση.

Σε αυτό το άρθρο είναι μια σύνοψη των τους κανόνες ονοματοθεσίας και περιορισμούς για τους πόρους Azure και ένα σύνολο γραμμής βάσης συστάσεις για κανόνες ονοματοθεσίας.  Μπορείτε να χρησιμοποιήσετε αυτές τις συστάσεις ως σημείο εκκίνησης για τη δική σας συγκεκριμένες συμβάσεις για τις ανάγκες σας.  

Το κλειδί για την επιτυχία με συμβάσεις ονοματοδοσίας για τον καθορισμό και να παρακολουθείτε κατά μήκος των οργανισμών και εφαρμογές. 

## <a name="naming-subscriptions"></a>Ονομασία συνδρομών

Κατά την ονομασία Azure συνδρομές, λεπτομερής ονόματα καθιστούν Κατανόηση το περιβάλλον και το σκοπό της κάθε Απαλοιφή συνδρομής.  Όταν εργάζεστε σε ένα περιβάλλον με πολλές συνδρομές, ακολουθώντας μια κοινόχρηστη κανόνες ονοματοθεσίας μπορεί να βελτιώσει σαφήνεια.

Συνιστάται το μοτίβο για τις συνδρομές ονομασίας είναι:

`<Company> <Department (optional)> <Product Line (optional)> <Environment>`

- Εταιρεία συνήθως θα είναι το ίδιο για κάθε εγγραφή. Ωστόσο, ορισμένες εταιρείες ενδέχεται να έχει θυγατρικό εταιρείες μέσα στην οργανωτική δομή. Αυτές οι εταιρείες ενδέχεται να γίνεται από μια κεντρική ομάδα IT. Σε αυτές τις περιπτώσεις, μπορεί να διαφέρει από τόσο της γονικής επωνυμία της εταιρείας (*Contoso*) και με το όνομα της εταιρείας θυγατρικό (*Βόρεια ανέμου*).

- Τμήμα είναι το όνομα του εντός της εταιρείας όπου εργάζεστε μια ομάδα ατόμων. Αυτό το στοιχείο μέσα στο χώρο ονομάτων ως προαιρετικό.

- Γραμμή προϊόντων είναι ένα συγκεκριμένο όνομα για ένα προϊόν ή μια συνάρτηση που έχει εκτελεστεί από μέσα από το τμήμα.
Αυτό είναι προαιρετικό γενικά για εφαρμογές και υπηρεσίες εσωτερική τοποθεσία. Ωστόσο, συνιστάται ιδιαίτερα να χρησιμοποιήσετε για δημόσιες υπηρεσίες που απαιτούν εύκολο διαχωρισμού και αναγνώρισης (όπως στην περίπτωση Απαλοιφή διαχωρισμό των χρεώσεων εγγραφές).

- Περιβάλλον είναι το όνομα που περιγράφει την ανάπτυξη του κύκλου ζωής της εφαρμογές ή υπηρεσίες, όπως την, Ερωτήσεις ή ειδών.

| Εταιρεία | Τμήμα | Γραμμή προϊόντος ή υπηρεσίας | Περιβάλλον | Ονοματεπώνυμο  |
----------| ---------- | ----------------------- | ----------- | ---------- |
| Το contoso | SocialGaming | AwesomeService | Παραγωγής | Παραγωγής AwesomeService Contoso SocialGaming |
| Το contoso | SocialGaming | AwesomeService | Αποκλίσεις | Αποκλίσεις AwesomeService SocialGaming contoso |
| Το contoso | ΤΟ | InternalApps | Παραγωγής | Contoso IT InternalApps παραγωγής |
| Το contoso | ΤΟ | InternalApps | Αποκλίσεις | Contoso IT αποκλίσεις InternalApps |

<!-- TODO; include more information about organizing subscriptions for application deployment, pods, etc. -->

## <a name="use-affixes-to-avoid-ambiguity"></a>Χρησιμοποιήστε πραγματοποιούν για να αποφύγετε ασαφειών

Κατά την ονομασία πόρους στο Azure, συνιστάται να χρησιμοποιήσετε κοινές προθέματα ή προθέματα για να προσδιορίσετε τον τύπο και το περιβάλλον του πόρου.  Ενώ όλες τις πληροφορίες σχετικά με τον τύπο, μετα-δεδομένα, περιβάλλον, είναι διαθέσιμη μέσω προγραμματισμού, εφαρμογή κοινών πραγματοποιούν απλοποιεί την οπτική αναγνώριση.  Όταν ενσωματώνετε πραγματοποιούν σε κανόνες ονοματοθεσίας σας, είναι σημαντικό να με σαφήνεια Καθορίστε αν η θέτει βρίσκεται στην αρχή του ονόματος (πρόθεμα) ή στο τέλος (επίθημα).  

Για παράδειγμα, εδώ θα βρείτε δύο πιθανές ονομάτων για μια υπηρεσία φιλοξενίας ένας μηχανισμός υπολογισμού:

- SvcCalculationEngine (πρόθεμα)
- CalculationEngineSvc (επίθημα)

Πραγματοποιούν μπορεί να αναφέρεται σε διαφορετικές πτυχές που περιγράφουν τους συγκεκριμένους πόρους. Ο παρακάτω πίνακας δείχνει ορισμένα παραδείγματα χρησιμοποιείται συνήθως.

| Πλευρών | Παράδειγμα | Σημειώσεις |
| ------ | ------- | ----- |
| Περιβάλλον | αποκλίσεις, ειδών, Ερωτήσεις | Προσδιορίζει το περιβάλλον για τον πόρο |
| Θέση | uw (ΜΑΣ Δυτική), ue (ΜΑΣ Ανατολή) | Προσδιορίζει την περιοχή στην οποία έχει αναπτυχθεί ο πόρος |
| Παρουσία | 01, 02 | Για τους πόρους που έχουν περισσότερες από μία περίοδο λειτουργίας με όνομα (διακομιστές web, κ.λπ.). |
| Προϊόντος ή υπηρεσίας | υπηρεσία | Προσδιορίζει το προϊόν, εφαρμογή ή υπηρεσία που υποστηρίζει τον πόρο |
| Ρόλος | SQL, στο web, την ανταλλαγή μηνυμάτων | Προσδιορίζει το ρόλο του συσχετισμένη πόρου |

Κατά την ανάπτυξη μια συγκεκριμένη κανόνες ονοματοθεσίας για την εταιρεία ή έργα, είναι σημαντικό να επιλέξετε ένα κοινό σύνολο πραγματοποιούν και τη θέση τους (επίθημα ή πρόθεμα).

## <a name="naming-rules-and-restrictions"></a>Κανόνες ονοματοθεσίας και περιορισμούς

Κάθε τύπος πόρο ή μια υπηρεσία στο Azure επιβάλλει ένα σύνολο ονομασία τους περιορισμούς και εμβέλεια; οποιαδήποτε κανόνες ονοματοθεσίας ή του μοτίβου πρέπει να συμφωνεί με την απαιτούμενη τους κανόνες ονοματοθεσίας και το εύρος.  Για παράδειγμα, ενώ το όνομα του μια Εικονική αντιστοιχίζει ένα όνομα DNS (και, επομένως, πρέπει να είναι μοναδικό σε όλους Azure), το όνομα του VNET ένα εύρος στην ομάδα πόρων που δημιουργείται μέσα.

Σε γενικές γραμμές, αποφύγετε την ειδικούς χαρακτήρες (`-` ή `_`) ως το πρώτο ή το τελευταίο χαρακτήρα σε οποιοδήποτε όνομα. Αυτούς τους χαρακτήρες θα προκαλέσει οι περισσότεροι κανόνες επικύρωσης αποτυχία.

| Κατηγορία | Υπηρεσία ή οντότητα | Πεδίο εφαρμογής | Μήκος | Περίβλημα | Έγκυροι χαρακτήρες | Προτεινόμενη μοτίβου | Παράδειγμα |
| ------------- | ----------------- | ----- | ------ | ------ | ---------------- | ----------------- | ------- |
| Ομάδα πόρων | Ομάδα πόρων | Καθολικό | 1-64 | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικός χαρακτήρας υπογράμμισης και ενωτικό | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| Ομάδα πόρων | Ορισμός διαθεσιμότητας | Ομάδα πόρων | 1-80 | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικός χαρακτήρας υπογράμμισης και ενωτικό | `<service-short-name>-<context>-as` | `profx-sql-as` |
| Γενικά | Ετικέτα | Συσχετισμένη οντότητα | 512 (όνομα), 256 (τιμή) | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικά | `"key" : "value"` | `"department" : "Central IT"` |
| Υπολογισμός | Εικονική μηχανή | Ομάδα πόρων | 1-15 | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικός χαρακτήρας υπογράμμισης και ενωτικό | `<name>-<role>-<instance>` | `profx-sql-001` |
| Χώρος αποθήκευσης | Όνομα λογαριασμού χώρου αποθήκευσης (δεδομένα) | Καθολικό | 3-24 | Πεζά | Αλφαριθμητικά | `<service short name><type><number>` | `profxdata001` |
| Χώρος αποθήκευσης | Όνομα λογαριασμού χώρου αποθήκευσης (δίσκων) | Καθολικό | 3-24 | Πεζά | Αλφαριθμητικά | `<vm name without dashes>st<number>` | `profxsql001st0` |
| Χώρος αποθήκευσης | Το όνομα του κοντέινερ | Το λογαριασμό χώρου αποθήκευσης | 3-63 |   Πεζά | Αλφαριθμητικός και παύλα | `<context>` | `logs` |
| Χώρος αποθήκευσης | Όνομα BLOB | Κοντέινερ | 1-1024 | Διάκριση πεζών-κεφαλαίων | Οποιαδήποτε διεύθυνση URL char | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Χώρος αποθήκευσης | Όνομα ουράς | Το λογαριασμό χώρου αποθήκευσης | 3-63 | Πεζά | Αλφαριθμητικός και παύλα | `<service short name>-<context>-<num>` | `awesomeservice-messages-001` |
| Χώρος αποθήκευσης | Όνομα πίνακα | Το λογαριασμό χώρου αποθήκευσης | 3-63 |Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικό | `<service short name>-<context>` | `awesomeservice-logs` |
| Χώρος αποθήκευσης | Όνομα αρχείου | Το λογαριασμό χώρου αποθήκευσης | 3-63 | Πεζά | Αλφαριθμητικά | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Δικτύωση | Εικονικό δίκτυο (VNet) | Ομάδα πόρων | 2-64 | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικός, παύλα, χαρακτήρας υπογράμμισης και περίοδο | `<service short name>-[section]-vnet` | `profx-vnet` |
| Δικτύωση | Υποδίκτυο | Γονικό VNet | 2-80 | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικός χαρακτήρας υπογράμμισης, παύλα και περίοδο | `<role>-subnet` | `gateway-subnet` |
| Δικτύωση | Περιβάλλον εργασίας δικτύου | Ομάδα πόρων | 1-80 | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικός, παύλα, χαρακτήρας υπογράμμισης και περίοδο | `<vmname>-<num>nic` | `profx-sql1-1nic` |
| Δικτύωση | Ομάδα ασφαλείας δικτύου | Ομάδα πόρων | 1-80 | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικός, παύλα, χαρακτήρας υπογράμμισης και περίοδο | `<service short name>-<context>-nsg` | `profx-app-nsg` |
| Δικτύωση | Ομάδα ασφαλείας δικτύου κανόνα | Ομάδα πόρων | 1-80 | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικός, παύλα, χαρακτήρας υπογράμμισης και περίοδο | `<descriptive context>` | `sql-allow` |
| Δικτύωση | Δημόσια διεύθυνση IP | Ομάδα πόρων | 1-80 | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικός, παύλα, χαρακτήρας υπογράμμισης και περίοδο | `<vm or service name>-pip` | `profx-sql1-pip` |
| Δικτύωση | Εξισορρόπηση φόρτου | Ομάδα πόρων | 1-80 | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικός, παύλα, χαρακτήρας υπογράμμισης και περίοδο | `<service or role>-lb` | `profx-lb` |
| Δικτύωση | Φόρτωση ρύθμισης παραμέτρων ισορροπημένες κανόνων | Εξισορρόπηση φόρτου | 1-80 | Διάκριση πεζών-κεφαλαίων | Αλφαριθμητικός, παύλα, χαρακτήρας υπογράμμισης και περίοδο | `descriptive context` | `http` |

<!-- TODO fill in the rest of these resources
| Networking | Azure Application Gateway | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `<service or role>-aag` | `profx-aag`
| Networking | Azure Application Gateway Connection | Azure Application Gateway | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `` | TODO
| Networking | Traffic Manager Profile | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | TODO | TODO
-->

## <a name="organizing-resources-with-tags"></a>Οργάνωση πόρους με ετικέτες

Η διαχείριση πόρων Azure υποστηρίζει ετικετών οντοτήτων με συμβολοσειρές κειμένου αυθαίρετο αναγνωρίζετε περιβάλλοντος και να βελτιστοποιήσετε την αυτοματοποίηση.  Για παράδειγμα, η ετικέτα `"sqlVersion: "sql2014ee"` θα μπορούσε να προσδιορίσει ΣΠΣ σε μια ανάπτυξη του εκτελεί SQL Server 2014 Enterprise Edition για την εκτέλεση μιας αυτοματοποιημένη δέσμη ενεργειών σε σχέση με τους.  Οι ετικέτες πρέπει να χρησιμοποιείται για να αυξήσετε και να βελτιώσετε περιβάλλοντος πλευρά των συμβάσεων ονομασίας επιλέξει.

> [AZURE.TIP]Ένα άλλο πλεονέκτημα της ετικέτες είναι ότι οι ετικέτες εκτείνονται σε ομάδες πόρων, επιτρέποντάς σας να συνδέσετε και ο συσχετισμός οντοτήτων σε ανόμοιες αναπτύξεις.

Κάθε πόρο ή μια ομάδα πόρων μπορεί να έχει έως **15** ετικέτες. Το όνομα της ετικέτας περιορίζεται σε 512 χαρακτήρες και η τιμή ετικέτας περιορίζεται σε 256 χαρακτήρες.

Για περισσότερες πληροφορίες σχετικά με εφαρμογή ετικετών πόρων, ανατρέξτε [Χρήση ετικετών για να οργανώσετε το Azure πόρους](../resource-group-using-tags.md).

Ορισμένες από τις συνήθεις περιπτώσεις χρήση ετικετών είναι:

- **Χρέωση**; Ομαδοποίηση πόρων και συσχετισμού με χρέωση ή τόκων πίσω κωδικούς.
- **Υπηρεσία περιβάλλοντος αναγνώρισης**; Προσδιορισμός ομάδων πόρων σε ομάδες πόρων για συνήθεις εργασίες και ομαδοποίησης
- **Έλεγχος πρόσβασης και την ασφάλεια περιεχομένου**; Ρόλος διαχείρισης αναγνώρισης βάση χαρτοφυλακίου, συστήματος, υπηρεσία, εφαρμογή, παρουσία, κ.λπ.

> [AZURE.TIP]Προσθήκη ετικετών σε πρώιμη - ετικέτα συχνότητα.  Καλύτερα να έχετε μια γραμμή βάσης για εφαρμογή ετικετών συνδυασμού στη θέση και τοποθέτηση επάνω από το χρόνο, αντί να χρειάζεται να κάνετε μετατροπές μετά το γεγονός.  

Παράδειγμα ορισμένες κοινές ετικετών προσεγγίσεις:

| Όνομα ετικέτας | Πλήκτρο | Παράδειγμα | Σχόλιο |
| -------- | --- | ------- | ------- |
| Τιμολόγηση / εσωτερική ετοιμάζετε Αναγνωριστικό | 2η  | `IT-Chargeback-1234` | Μια εσωτερική εισόδου/εξόδου ή κωδικό χρέωσης |
| Τελεστής ή απευθείας υπεύθυνοι ατομικές (DRI) | managedBy | `joe@contoso.com`  | Ψευδώνυμο ή διεύθυνση ηλεκτρονικού ταχυδρομείου |
| Όνομα του έργου | όνομα έργου | `myproject`  | Όνομα του έργου ή προϊόν γραμμής |
| Η έκδοση του έργου | έκδοση του έργου | `3.4`  | Έκδοση του έργου ή προϊόν γραμμής |
| Περιβάλλον | περιβάλλον | `<Production, Staging, QA >` | Αναγνωριστικό περιβάλλοντος | 
| Επίπεδο | επίπεδο | `Front End, Back End, Data` | Σειρά ή ρόλο/περιβάλλον αναγνώρισης |
| Δεδομένων προφίλ | dataProfile | `Public, Confidential, Restricted, Internal` | Ευαισθησία των δεδομένων που είναι αποθηκευμένα στον πόρο |
 
## <a name="tips-and-tricks"></a>Συμβουλές και τεχνάσματα

Ορισμένοι τύποι πόρων μπορεί να απαιτεί επιπλέον περίθαλψη στην ονομασία και συμβάσεις.

### <a name="virtual-machines"></a>Εικονικές μηχανές

Ειδικά σε μεγαλύτερο τοπολογίες, προσεκτικά ονομασία εικονικές μηχανές βελτιώνει την προσδιορίζοντας το ρόλο και το σκοπό του κάθε υπολογιστή και ενεργοποιώντας πιο προβλέψιμων δέσμες ενεργειών.

> [AZURE.WARNING]Κάθε εικονική μηχανή στο Azure έχει ένα όνομα πόρου Azure, και ένα όνομα κεντρικού υπολογιστή του λειτουργικού συστήματος.  
> Εάν το όνομα του πόρου και όνομα κεντρικού υπολογιστή είναι διαφορετικά, διαχείριση του ΣΠΣ μπορεί να είναι δύσκολη και θα πρέπει να αποφεύγονται.
> Για παράδειγμα, εάν μια εικονική μηχανή δημιουργείται από ένα .vhd που ήδη περιέχει ένα λειτουργικό σύστημα έχει ρυθμιστεί με ένα όνομα κεντρικού υπολογιστή.

- [Συνθήκες ονοματοθεσίας για τα Windows Server ΣΠΣ](https://support.microsoft.com/en-us/kb/188997)

<!-- TODO - recommendations on naming VMs. -->

### <a name="storage-accounts-and-storage-entities"></a>Λογαριασμοί χώρου αποθήκευσης και οντοτήτων χώρου αποθήκευσης

Υπάρχουν δύο περιπτώσεις πρωτεύοντος χρήσης για τους λογαριασμούς χώρος αποθήκευσης - δημιουργία αντιγράφων δίσκων για ΣΠΣ και την αποθήκευση των δεδομένων στα αντικείμενα blob, ουρές και πίνακες.  Λογαριασμοί χώρου αποθήκευσης που χρησιμοποιούνται για Εικονική δίσκων πρέπει να ακολουθήστε τους κανόνες ονοματοθεσίας της συσχετισμός τους με το γονικό στοιχείο όνομα Εικονική (και με την πιθανή ανάγκη για πολλαπλούς λογαριασμούς χώρου αποθήκευσης για υψηλής ποιότητας SKU Εικονική, εφαρμόστε επίσης τη αριθμού επίθημα).

> [AZURE.TIP]Λογαριασμοί χώρος αποθήκευσης - για δεδομένα ή δίσκων - πρέπει να ακολουθήσετε μια συνθήκη ονοματοθεσίας που επιτρέπει σε πολλούς λογαριασμούς χώρου αποθήκευσης για να χρησιμοποιηθούν (δηλαδή πάντα χρησιμοποιώντας ένα αριθμητικό επίθεμα).

Είναι εφικτό να ρυθμίσετε τις παραμέτρους ενός προσαρμοσμένου ονόματος τομέα για να αποκτήσετε πρόσβαση blob δεδομένων στο λογαριασμό σας στο χώρο αποθήκευσης Azure.
Είναι το προεπιλεγμένο τελικό σημείο για την υπηρεσία Blob `https://mystorage.blob.core.windows.net`.

Αλλά εάν μπορείτε να αντιστοιχίσετε έναν προσαρμοσμένο τομέα (όπως www.contoso.com) στο τελικό σημείο blob για το λογαριασμό χώρου αποθήκευσης, μπορείτε επίσης να αποκτήσετε πρόσβαση αντικειμένων blob δεδομένων στο λογαριασμό σας στο χώρο αποθήκευσης, χρησιμοποιώντας αυτόν τον τομέα. Για παράδειγμα, με ένα προσαρμοσμένο όνομα τομέα, `http://mystorage.blob.core.windows.net/mycontainer/myblob` ήταν δυνατή η πρόσβαση ως `http://www.contoso.com/mycontainer/myblob`.

Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση αυτής της δυνατότητας, ανατρέξτε στις επιλογές [Ρύθμιση παραμέτρων ενός προσαρμοσμένου ονόματος τομέα για το τελικό σημείο αποθήκευσης αντικειμένων Blob](../storage/storage-custom-domain-name.md).

Για περισσότερες πληροφορίες σχετικά με ονομασία αντικείμενα blob, κοντέινερ και πίνακες:

- [Ονομασία και αναφορά σε κοντέινερ, αντικείμενα BLOB και μετα-δεδομένων](https://msdn.microsoft.com/library/dd135715.aspx)
- [Ονομασία ουρές και μετα-δεδομένων](https://msdn.microsoft.com/library/dd179349.aspx)
- [Ονομασία πίνακες](https://msdn.microsoft.com/library/azure/dd179338.aspx)

Ένα όνομα blob μπορούν να περιέχουν οποιονδήποτε συνδυασμό των χαρακτήρων, αλλά πρέπει να είναι σωστά διαφυγής δεσμευμένη διεύθυνση URL χαρακτήρες. Αποφεύγετε τα ονόματα αντικειμένων blob που τελειώνει με τελεία (.), μια κάθετο (/), ή μια ακολουθία ή ένας συνδυασμός των δύο. Κατά συνθήκη, η κάθετος είναι το διαχωριστικό **εικονικού** καταλόγου. Μην χρησιμοποιείτε μια ανάστροφη κάθετο (\) ένα όνομα blob. Το πρόγραμμα-πελάτη APIs μπορεί να επιτραπεί και, αλλά, στη συνέχεια, να κατακερματισμός σωστά αποτύχει και δεν θα ταιριάζουν με τις υπογραφές.

Δεν είναι δυνατό να τροποποιήσετε το όνομα λογαριασμού χώρου αποθήκευσης ή κοντέινερ αφού έχει δημιουργηθεί.
Εάν θέλετε να χρησιμοποιήσετε ένα νέο όνομα, πρέπει να διαγράψετε και να δημιουργήσετε ένα νέο.

> [AZURE.TIP] Συνιστάται να δημιουργήσετε μια συνθήκη ονοματοθεσίας για όλους λογαριασμούς χώρου αποθήκευσης και τύπους πριν επιβιβάζονται σχετικά με την ανάπτυξη του μια νέα υπηρεσία ή την εφαρμογή.

## <a name="example---deploying-an-n-tier-service"></a>Παράδειγμα - για την ανάπτυξη μιας υπηρεσίας ν σειρών

Σε αυτό το παράδειγμα, ορισμού μια ρύθμιση παραμέτρων της υπηρεσίας ν σειρών, που αποτελείται από διακομιστές περιβάλλοντος χρήστη των υπηρεσιών IIS (φιλοξενούνται σε Windows Server VM), με τον SQL Server (φιλοξενούνται σε δύο Windows Server VM), ένα σύμπλεγμα ElasticSearch (φιλοξενούνται σε 6 Linux VM) και των λογαριασμών συσχετισμένη χώρου αποθήκευσης, εικονικό δίκτυα, πόρων ομαδοποίηση και μονάδα εξισορρόπησης φόρτου.

Ας ξεκινήσουμε με τον ορισμό τις συμβάσεις με βάση τα συμφραζόμενα για αυτήν την εφαρμογή:

| Οντότητα | Σύμβαση | Περιγραφή  |
| ------ | ---------- | ------------ |  
| Όνομα της υπηρεσίας | `profx` | Το σύντομο όνομα της εφαρμογής ή υπηρεσία αναπτύσσεται |
| Περιβάλλον | `prod` | Πρόκειται για την ανάπτυξη παραγωγής (αντί για ποιοτικό Έλεγχο, δοκιμής, κ.λπ.) |

Από αυτήν τη γραμμή βάσης θα σας να αντιστοιχίσετε, στη συνέχεια, εκτός των συμβάσεων για κάθε έναν από τους τύπους πόρων:

| Τύπος πόρου | Σύμβαση βάσης | Παράδειγμα |
| ------------- | --------------- | ------- |
| Συνδρομή | `<Company> <Department (optional)> <Product Line (optional)> <Environment>` | `Contoso IT InternalApps Profx Production` |
| Ομάδα πόρων | `servicename-rg` | `profx-rg` |
| Εικονικό δίκτυο | `servicename-vnet` | `profx-vnet` |
| Υποδίκτυο | `role-subnet` | `sql-vnet` |
| Εξισορρόπηση φόρτου | `servicename-lb` | `profx-lb` |
| Εικονική μηχανή | `servicename-role[number]` | `profx-sql0` |
| Το λογαριασμό χώρου αποθήκευσης | `<vmnamenodashes>st<num>` | `profxsql0st0` |

Όπως φαίνεται παρακάτω διάγραμμα:

![διάγραμμα τοπολογία εφαρμογής] (media/guidance-naming-conventions/guidance-naming-convention-example.png "Δείγμα τοπολογία εφαρμογής")

## <a name="sample---azure-cli-script-for-deploying-the-sample-above"></a>Δείγμα - Azure CLI δέσμη ενεργειών για την ανάπτυξη του δείγματος παραπάνω

```bash
#!/bin/sh

#####################################################################
# Sample script using the Azure CLI to build out an application 
# demonstrating naming conventions.  
#
# Note; this script is not intended for production deployment, as it does 
# not create availability sets, configure network security rules, etc.
#####################################################################

# Set up variables to build out the naming conventions for deploying
# the cluster  
LOCATION=eastus2
APP_NAME=profx
ENVIRONMENT=prod
USERNAME=testuser
PASSWORD="testpass"

# Set up the tags to associate with items in the application
TAG_BILLTO="InternalApp-ProFX-12345"
TAGS="billTo=${TAG_BILLTO}"

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
SUBSCRIPTION=3e9c25fc-55b3-4837-9bba-02b6eb204331

# Set up the names of things using recommended conventions
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
VNET_NAME="${APP_NAME}-vnet"

# Set up the postfix variables attached to most CLI commands
POSTFIX="--resource-group ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}"

##########################################################################################
# Set up the VM conventions for Linux and Windows images

# For Windows, get the list of URN's via 
# azure vm image list ${LOCATION} MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

# For Linux, get the list or URN's via 
# azure vm image list ${LOCATION} canonical ubuntuserver
LINUX_BASE_IMAGE=canonical:ubuntuserver:16.04.0-DAILY-LTS:16.04.201602130

#########################################################################################
## Define functions 
create_vm ()
{
    vm_name=$1
    vnet_name=$2
    subnet_name=$3
    os_type=$4
    vhd_path=$5
    vm_size=$6
    diagnostics_storage=$7

    # Create the network interface card for this VM
    azure network nic create --name "${vm_name}-0nic" --subnet-name ${subnet_name} --subnet-vnet-name ${vnet_name} \
        --tags="${TAGS}" ${POSTFIX}

    # Create the storage account for this vm's disks (premium locally redundant storage -> PLRS)
    # Note the ${var//-/} syntax to remove dashes from the vm name
    storage_account_name=${vm_name//-/}st01
    azure storage account create --type=PLRS --tags "${TAGS}" ${POSTFIX} "${storage_account_name}"

    # Map the name of the diagnostics storage account to a blob URI for boot diagnostics
    # This is (currently) required when deploying with a named premium storage account 
    diag_blob="https://${diagnostics_storage}.blob.core.windows.net/"

    # Create the VM
    azure vm create --name ${vm_name} --nic-name "${vm_name}-0nic" --os-type ${os_type} \
        --image-urn ${vhd_path} --vm-size ${vm_size} --vnet-name ${vnet_name} --vnet-subnet-name ${subnet_name} \
        --storage-account-name "${storage_account_name}" --storage-account-container-name vhds --os-disk-vhd "${vm_name}-osdisk.vhd" \
        --admin-username "${USERNAME}" --admin-password "${PASSWORD}" \
        --boot-diagnostics-storage-uri "${diag_blob}" \
        --tags="${TAGS}" ${POSTFIX} 
}

###################################################################################################
# Create resources

# Step 1 - create the enclosing resource group
azure group create --name "${RESOURCE_GROUP}" --location "${LOCATION}" --tags "${TAGS}" --subscription "${SUBSCRIPTION}"

# Step 2 - create the network security groups

# Step 3 - create the networks (VNet and subnets)
azure network vnet create --name "${VNET_NAME}" --address-prefixes="10.0.0.0/8" --tags "${TAGS}" ${POSTFIX}
# TODO - does subnet support tagging?
azure network vnet subnet create --name gateway-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.1.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-master-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.2.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-data-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.3.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name sql-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.4.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}

# Step 4 - define the load balancer and network security rules
azure network lb create --name "${APP_NAME}-lb" ${POSTFIX}
# In a production deployment script, we'd create load balancer rules and 
# network security groups here

# Step 5 - create a diagnostics storage account
diagnostics_storage_account=${APP_NAME//-/}diag
azure storage account create --type=LRS --tags "${TAGS}" ${POSTFIX} "${diagnostics_storage_account}"

# Step 6.1 - Create the gateway VMs
for i in `seq 1 4`;
do
    create_vm "${APP_NAME}-gw${i}" "${APP_NAME}-vnet" "gateway-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}" 
done    

# Step 6.2 - Create the ElasticSearch master and data VMs
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-master${i}" "${APP_NAME}-vnet" "es-master-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-data${i}" "${APP_NAME}-vnet" "es-data-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done

# Step 6.3 - Create the SQL VMs
create_vm "${APP_NAME}-sql0" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
create_vm "${APP_NAME}-sql1" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
```
