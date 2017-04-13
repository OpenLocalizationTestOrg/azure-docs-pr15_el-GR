<properties
    pageTitle="Προστασία του API διαχείρισης Azure API | Microsoft Azure"
    description="Μάθετε πώς να προστατεύσετε το API με τα όρια και περιορισμού πολιτικές (περιορισμού ταχύτητας)."
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Προστασία του API με τα όρια χρήσης Azure API διαχείρισης

Αυτός ο οδηγός εμφανίζει πόσο εύκολο είναι να προσθέσετε προστασία για το API παρασκηνίου, ρυθμίζοντας τις παραμέτρους πολιτικές όριο και ορίου επιτόκιο Azure API διαχείρισης.

Σε αυτό το πρόγραμμα εκμάθησης, θα δημιουργήσετε ένα προϊόν "Δωρεάν δοκιμαστική έκδοση" API που επιτρέπει στους προγραμματιστές για τις κλήσεις έως και 10 ανά λεπτό και έως 200 κλήσεις ανά εβδομάδα για το API χρησιμοποιώντας το [όριο Ρυθμός κλήσεων ανά συνδρομή](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) και τις πολιτικές [Ορισμός ορίου χρήσης ανά συνδρομή](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) . Μπορείτε, στη συνέχεια, θα δημοσιεύσετε το API και να ελέγξετε την πολιτική όριο επιτόκιο.

Για πιο σύνθετες επιτάχυνσης σενάρια χρησιμοποιώντας τις πολιτικές [επιτόκιο όριο με κλειδί](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) και [ορίου-κλειδί](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) , ανατρέξτε στο θέμα [περιορισμού Azure API διαχείρισης αιτήσεων για προχωρημένους](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>Για να δημιουργήσετε ένα προϊόν

Σε αυτό το βήμα, θα δημιουργήσετε μια δωρεάν δοκιμαστική έκδοση προϊόντος που δεν απαιτεί έγκριση συνδρομής.

>[AZURE.NOTE] Εάν έχετε ήδη ένα προϊόν που έχει ρυθμιστεί και θέλετε να χρησιμοποιήσετε για αυτό το πρόγραμμα εκμάθησης, μπορείτε να μεταβείτε προς τα εμπρός σε [Ρύθμιση παραμέτρων κλήσεων επιτόκιο πολιτικές όριο και ορίου][] και ακολουθήστε το πρόγραμμα εκμάθησης από εκεί χρησιμοποιώντας το προϊόν σας στη θέση του δωρεάν δοκιμαστική έκδοση του προϊόντος.

Για να ξεκινήσετε, κάντε κλικ στην επιλογή **Διαχείριση** στο Classic Azure της υπηρεσίας διαχείρισης API. Ενέργεια αυτή σας μεταφέρει στην πύλη του publisher API διαχείρισης.

![Πύλη του Publisher][api-management-management-console]

>Εάν δεν έχετε δημιουργήσει ακόμη μια παρουσία της υπηρεσίας διαχείρισης API, ανατρέξτε στο θέμα [Δημιουργία μια παρουσία της υπηρεσίας διαχείρισης API][] στην εκμάθηση [Διαχείριση το πρώτο API Azure API διαχείρισης][] .

Κάντε κλικ στην επιλογή **προϊόντα** στο μενού **API διαχείρισης** στα αριστερά για να εμφανίσετε τη σελίδα " **προϊόντα** ".

![Προσθήκη προϊόντων][api-management-add-product]

Κάντε κλικ στην επιλογή **Προσθήκη προϊόντος** για να εμφανίσετε το παράθυρο διαλόγου **Προσθήκη νέου προϊόντος** .

![Προσθήκη νέου προϊόντος][api-management-new-product-window]

Στο πλαίσιο **Τίτλος** , πληκτρολογήστε **Δωρεάν δοκιμαστικής έκδοσης**.

Στο πλαίσιο **Περιγραφή** , πληκτρολογήστε το ακόλουθο κείμενο:  **συνδρομητές θα έχουν τη δυνατότητα να εκτελέσετε τις κλήσεις 10/λεπτό έως 200 κλήσεις/εβδομάδα μετά από την οποία δεν επιτρέπεται η πρόσβαση.**

Προϊόντα API διαχείρισης μπορούν να προστατευτούν ή να ανοίξετε. Προστατευμένο προϊόντα πρέπει να εγγραφείτε για να μπορέσετε να μπορούν να χρησιμοποιηθούν. Άνοιγμα προϊόντα μπορεί να χρησιμοποιηθεί χωρίς συνδρομή. Βεβαιωθείτε ότι **απαιτείται συνδρομή** είναι ενεργοποιημένη για να δημιουργήσετε ένα προστατευμένο προϊόν που απαιτεί μια συνδρομή. Αυτή είναι η προεπιλεγμένη ρύθμιση.

Εάν θέλετε ο διαχειριστής για να εξετάσετε και να αποδεχθείτε ή να απορρίψετε προσπάθειες συνδρομή σε αυτό το προϊόν, επιλέξτε **απαιτείται έγκριση συνδρομής**. Εάν δεν είναι επιλεγμένο το πλαίσιο ελέγχου, προσπάθειες συνδρομή θα αυτόματη έγκριση. Σε αυτό το παράδειγμα, συνδρομές έχουν εγκριθεί αυτόματα, ώστε να μην επιλέξετε το πλαίσιο.

Για να επιτρέψετε προγραμματιστής λογαριασμών για να εγγραφείτε πολλές φορές για το νέο προϊόν, επιλέξτε το πλαίσιο ελέγχου να **επιτρέπονται πολλές συνδρομές ταυτόχρονα** . Αυτό το πρόγραμμα εκμάθησης δεν χρησιμοποιούν πολλές συνδρομές ταυτόχρονα, επομένως, αφήστε το απενεργοποιημένο.

Αφού έχουν καταχωρηθεί όλες τις τιμές, κάντε κλικ στο κουμπί **Αποθήκευση** για να δημιουργήσετε το προϊόν.

![Προϊόν που προστίθεται][api-management-product-added]

Από προεπιλογή, νέα προϊόντα είναι ορατή στους χρήστες στην ομάδα **διαχειριστών** . Πρόκειται να προσθέσετε την ομάδα **τους προγραμματιστές** . Κάντε κλικ στην επιλογή **Δωρεάν δοκιμαστική έκδοση**και, στη συνέχεια, κάντε κλικ στην καρτέλα **ορατότητας** .

>Στην περιοχή Διαχείριση API, ομάδες χρησιμοποιούνται για να διαχειριστείτε την ορατότητα προϊόντων για τους προγραμματιστές. Προϊόντα εκχώρηση ορατότητα σε ομάδες και τους προγραμματιστές να προβάλετε και να εγγραφείτε για τα προϊόντα που είναι ορατές σε τις ομάδες στις οποίες ανήκουν. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε και να χρησιμοποιήσετε ομάδες Azure API διαχείρισης][].

![Προσθήκη ομάδας προγραμματιστές][api-management-add-developers-group]

Επιλέξτε το πλαίσιο ελέγχου **Προγραμματιστής** και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

## <a name="add-api"> </a>Για να προσθέσετε ένα API του προϊόντος

Σε αυτό το βήμα του προγράμματος εκμάθησης, θα προσθέσουμε το API Αντήχηση για το νέο προϊόν δωρεάν δοκιμαστική έκδοση της.

>Κάθε παρουσία της υπηρεσίας διαχείρισης API περιλαμβάνει προκαθορισμένες ρυθμίσεις παραμέτρων με API Αντήχηση που μπορούν να χρησιμοποιηθούν για να πειραματιστείτε με και ενημερωθείτε σχετικά με το API διαχείρισης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση του πρώτου API Azure API διαχείρισης][].

Κάντε κλικ στην επιλογή **προϊόντα** από το **API διαχείρισης** μενού στα αριστερά και, στη συνέχεια, κάντε κλικ στην επιλογή **Δωρεάν δοκιμαστική έκδοση** για να ρυθμίσετε τις παραμέτρους του προϊόντος.

![Ρύθμιση παραμέτρων του προϊόντος][api-management-configure-product]

Κάντε κλικ στην επιλογή **Προσθήκη API του προϊόντος**.

![Προσθήκη API του προϊόντος][api-management-add-api]

Επιλέξτε **Αντήχηση API**και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

![Προσθήκη API αντήχηση][api-management-add-echo-api]

## <a name="policies"> </a>Για να ρυθμίσετε το όριο και ορίων μεγέθους πολιτικές ρυθμός κλήσης

Όρια χρεώσεων και του ορίου έχουν ρυθμιστεί στο πρόγραμμα επεξεργασίας πολιτικής. Κάντε κλικ στην επιλογή **πολιτικές** στην περιοχή του μενού **API διαχείρισης** στην αριστερή πλευρά. Στη λίστα **προϊόντων** , κάντε κλικ στην επιλογή **Δωρεάν δοκιμαστικής έκδοσης**.

![Πολιτική προϊόντος][api-management-product-policy]

Κάντε κλικ στην επιλογή **Προσθήκη πολιτικής** για να εισαγάγετε το πρότυπο πολιτικής και να ξεκινήσετε να δημιουργείτε τη χρέωση πολιτικές όριο και ορίου.

![Προσθήκη πολιτικής][api-management-add-policy]

Για να εισαγάγετε πολιτικές, τοποθετήστε το δρομέα σε είτε το **εισερχομένων** ή **εξερχομένων** ενότητας από το πρότυπο πολιτικής. Πολιτικές όριο και ορίου επιτόκιο είναι εισερχομένων πολιτικές, επομένως, τοποθετήστε το δρομέα στο στοιχείο εισερχομένων.

![Πρόγραμμα επεξεργασίας πολιτικής][api-management-policy-editor-inbound]

Οι δύο πολιτικές μας προσθέτετε σε αυτό το πρόγραμμα εκμάθησης είναι οι πολιτικές [όριο Ρυθμός κλήσεων ανά συνδρομή](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) και [Ορισμός ορίου χρήσης ανά συνδρομή](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) .

![Προτάσεις πολιτικής][api-management-limit-policies]

Αφού ο δρομέας τοποθετείται στο στοιχείο πολιτικής **εισερχομένων** , κάντε κλικ στο βέλος δίπλα στο στοιχείο **όριο Ρυθμός κλήσεων ανά συνδρομή** για να εισαγάγετε το πρότυπο πολιτικής.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Όριο Ρυθμός κλήσεων ανά συνδρομή** μπορούν να χρησιμοποιηθούν στο επίπεδο προϊόντος και μπορούν να χρησιμοποιηθούν σε επίπεδο όνομα μεμονωμένα λειτουργίας και API. Σε αυτό το πρόγραμμα εκμάθησης, χρησιμοποιούνται μόνο οι πολιτικές επίπεδο προϊόντος, ώστε να διαγράψετε τα στοιχεία **api** και τη **λειτουργία** από το στοιχείο **όριο επιτόκιο** , έτσι ώστε μόνο το εξωτερικό **όριο επιτόκιο** στοιχείο παραμένει, όπως φαίνεται στο παρακάτω παράδειγμα.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

Δωρεάν δοκιμαστική έκδοση του προϊόντος, η ταχύτητα μέγιστο επιτρεπόμενο κλήσης είναι 10 κλήσεις ανά λεπτό, επομένως πληκτρολογήστε **10** ως η τιμή για το χαρακτηριστικό **κλήσεις** και **60** για το χαρακτηριστικό **περίοδος ανανέωσης** .

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

Για να ρυθμίσετε τις παραμέτρους της πολιτικής **Ορισμός ορίου χρήσης ανά συνδρομή** , τοποθετήστε το δρομέα ακριβώς κάτω από το στοιχείο που προστέθηκε πρόσφατα **επιτόκιο όριο** μέσα στο στοιχείο **εισερχομένων** και, στη συνέχεια, κάντε κλικ στο βέλος που βρίσκεται στα αριστερά της **Ορισμός ορίου χρήσης ανά συνδρομή**.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Επειδή αυτή η πολιτική προορίζεται επίσης να είναι σε επίπεδο προϊόντος, διαγράψτε τα στοιχεία όνομα **api** και τη **λειτουργία** , όπως φαίνεται στο παρακάτω παράδειγμα.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Όρια μπορεί να βασίζεται στον αριθμό των κλήσεων ανά χρονικό διάστημα, το εύρος ζώνης ή και τα δύο. Σε αυτό το πρόγραμμα εκμάθησης, θα σας δεν ρύθμιση βάσει εύρους δεδομένων, επομένως, διαγράφετε το χαρακτηριστικό **εύρους ζώνης** .

    <quota calls="number" renewal-period="seconds">
    </quota>

Δωρεάν δοκιμαστική έκδοση του προϊόντος, το όριο είναι 200 κλήσεις ανά εβδομάδα. Καθορίστε **200** ως η τιμή για το χαρακτηριστικό **κλήσεις** και, στη συνέχεια, καθορίστε **604800** ως η τιμή για το χαρακτηριστικό **περίοδος ανανέωσης** .

    <quota calls="200" renewal-period="604800">
    </quota>

>Πολιτική χρονικά διαστήματα καθορίζονται σε δευτερόλεπτα. Για να υπολογίσετε το χρονικό διάστημα για μια εβδομάδα, πολλαπλασιάστε τον αριθμό των ημερών (7) με τον αριθμό των ωρών σε μια ημέρα (24) με τον αριθμό των λεπτών σε μία ώρα (60) με τον αριθμό των δευτερολέπτων στο σε λεπτά (60): 7 *24* 60 * 60 = 604800.

Όταν ολοκληρώσετε τη ρύθμιση των παραμέτρων της πολιτικής, αυτό θα πρέπει να συμφωνεί με το παρακάτω παράδειγμα.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Αφού έχουν ρυθμιστεί τις πολιτικές που θέλετε, κάντε κλικ στην επιλογή **Αποθήκευση**.

![Αποθήκευση της πολιτικής][api-management-policy-save]

## <a name="publish-product"></a> Για να δημοσιεύσετε το προϊόν

Τώρα που η προστίθενται τα API και τις πολιτικές που έχουν ρυθμιστεί οι παράμετροι, πρέπει να δημοσιευτεί το προϊόν, ώστε να μπορεί να χρησιμοποιηθεί από προγραμματιστές. Κάντε κλικ στην επιλογή **προϊόντα** από το **API διαχείρισης** μενού στα αριστερά και, στη συνέχεια, κάντε κλικ στην επιλογή **Δωρεάν δοκιμαστική έκδοση** για να ρυθμίσετε τις παραμέτρους του προϊόντος.

![Ρύθμιση παραμέτρων του προϊόντος][api-management-configure-product]

Κάντε κλικ στην επιλογή **Δημοσίευση**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ναι, δημοσιεύστε το** για να τον επιβεβαιώσετε.

![Δημοσίευση του προϊόντος][api-management-publish-product]

## <a name="subscribe-account"> </a>Για να εγγραφείτε ένα λογαριασμό για προγραμματιστές του προϊόντος

Τώρα που το προϊόν έχει δημοσιευθεί, να είναι διαθέσιμο για να έχετε εγγραφεί στο και να χρησιμοποιηθεί από προγραμματιστές.

>Οι διαχειριστές μιας παρουσίας API διαχείρισης αυτόματα έχετε εγγραφεί σε κάθε προϊόν. Σε αυτό το βήμα προγραμμάτων εκμάθησης, θα σας θα εγγραφείτε έναν από τους λογαριασμούς προγραμματιστής δεν είναι διαχειριστής για να το δωρεάν δοκιμαστική έκδοση του προϊόντος. Εάν ο λογαριασμός σας για προγραμματιστές είναι μέρος του ρόλου διαχειριστών, στη συνέχεια, μπορείτε να ακολουθήσετε μαζί με αυτό το βήμα, ακόμα και αν έχετε ήδη εγγραφεί.

Κάντε κλικ στην επιλογή **χρήστες** από το **API διαχείρισης** μενού στα αριστερά και, στη συνέχεια, κάντε κλικ στο όνομα του λογαριασμού σας για προγραμματιστές. Σε αυτό το παράδειγμα, χρησιμοποιούμε το λογαριασμό για προγραμματιστές **Ιωάννου Gragg** .

![Ρύθμιση παραμέτρων για προγραμματιστές][api-management-configure-developer]

Κάντε κλικ στην επιλογή **Προσθήκη συνδρομής**.

![Προσθήκη εγγραφής][api-management-add-subscription-menu]

Επιλέξτε **Δωρεάν δοκιμαστική έκδοση**και, στη συνέχεια, κάντε κλικ στην επιλογή **εγγραφή**.

![Προσθήκη εγγραφής][api-management-add-subscription]

>[AZURE.NOTE] Σε αυτό το πρόγραμμα εκμάθησης, πολλές συνδρομές ταυτόχρονη δεν είναι ενεργοποιημένες για τη δωρεάν δοκιμαστική έκδοση του προϊόντος. Αν βρίσκονταν, που θα σας ζητηθεί να ονομάσετε τη συνδρομή, όπως φαίνεται στο παρακάτω παράδειγμα.

![Προσθήκη εγγραφής][api-management-add-subscription-multiple]

Αφού κάνετε κλικ **εγγραφή**, το προϊόν εμφανίζεται στη λίστα **συνδρομή** για το χρήστη.

![Συνδρομή που προσθέσατε][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Να καλέσετε μια λειτουργία και να ελέγξετε το όριο επιτόκιο

Τώρα που το δωρεάν δοκιμαστική έκδοση του προϊόντος είναι ρυθμίσει τις παραμέτρους και να δημοσιεύσει, να κλήσεων ορισμένες λειτουργίες και να ελέγξετε την πολιτική όριο επιτόκιο.
Μεταβείτε στην πύλη για προγραμματιστές του κάνοντας κλικ στην επιλογή **Προγραμματιστής πύλη** στο μενού επάνω δεξιά.

![Πύλη για προγραμματιστές][api-management-developer-portal-menu]

Κάντε κλικ στην επιλογή **APIs** στο επάνω μενού και, στη συνέχεια, κάντε κλικ στην επιλογή **API Αντήχηση**.

![Πύλη για προγραμματιστές][api-management-developer-portal-api-menu]

Κάντε κλικ στην επιλογή **ΛΉΨΗ πόρων**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δοκιμή**.

![Άνοιγμα κονσόλας][api-management-open-console]

Διατήρηση της προεπιλεγμένης τιμές παραμέτρων και, στη συνέχεια, επιλέξτε τον αριθμό-κλειδί συνδρομή για το προϊόν δωρεάν δοκιμαστικής έκδοσης.

![Πλήκτρο συνδρομή][api-management-select-key]

>[AZURE.NOTE] Εάν έχετε πολλές συνδρομές, φροντίστε να επιλέξετε το κλειδί για **Δωρεάν δοκιμαστική έκδοση**, διαφορετικά δεν θα είναι στην πραγματικότητα τις πολιτικές που έχουν ρυθμιστεί στα προηγούμενα βήματα.

Κάντε κλικ στην επιλογή **Αποστολή**και, στη συνέχεια, να προβάλετε την απάντηση. Σημειώστε την **κατάσταση απόκρισης** **200 OK**.

![Αποτελέσματα λειτουργίας][api-management-http-get-results]

Κάντε κλικ στην επιλογή **Αποστολή** σε ποσοστό μεγαλύτερο από την πολιτική επιτόκιο το όριο των 10 κλήσεων ανά λεπτό. Αφού γίνει υπέρβαση της πολιτικής όριο επιτόκιο, επιστρέφεται η κατάσταση απόκρισης **429 πάρα πολλές αιτήσεις** .

![Αποτελέσματα λειτουργίας][api-management-http-get-429]

Το **περιεχόμενο της απόκρισης** υποδεικνύει το υπόλοιπο χρονικό διάστημα πριν από την επαναλήψεις θα ολοκληρωθεί με επιτυχία.

Όταν η πολιτική επιτόκιο όριο των 10 κλήσεων ανά λεπτό είναι στην πραγματικότητα, οι επόμενες κλήσεις θα αποτύχει μέχρι 60 δευτερόλεπτα έχουν παρέλθει από τα πρώτα 10 επιτυχής κλήσεων με το προϊόν πριν έγινε υπέρβαση του ορίου επιτόκιο. Σε αυτό το παράδειγμα, το υπόλοιπο διάστημα είναι 54 δευτερόλεπτα.

## <a name="next-steps"> </a>Επόμενα βήματα

-   Παρακολουθήστε μια επίδειξη της ρύθμισης όρια χρεώσεων και του ορίου στο βίντεο που ακολουθεί.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Διαχείριση του πρώτου API Azure API διαχείρισης]: api-management-get-started.md
[Πώς μπορείτε να δημιουργήσετε και να χρησιμοποιήσετε ομάδες διαχείρισης API Azure]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Δημιουργήστε μια παρουσία της υπηρεσίας διαχείρισης API]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Ρύθμιση παραμέτρων πολιτικών όριο και ορίων μεγέθους επιτόκιο κλήσης]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
