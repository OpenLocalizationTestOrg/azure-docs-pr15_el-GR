<properties
    pageTitle="Αργές επιδόσεις εφαρμογής web στην υπηρεσία εφαρμογής | Microsoft Azure"
    description="Σε αυτό το άρθρο σάς βοηθά να αντιμετωπίσετε τα προβλήματα επιδόσεων εφαρμογής web αργή στο Azure εφαρμογής υπηρεσίας."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="Web app επιδόσεις, αργή εφαρμογή, εφαρμογή αργή"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Αντιμετώπιση προβλημάτων αργή web app επιδόσεων στο Azure εφαρμογής υπηρεσίας

Σε αυτό το άρθρο σάς βοηθά να αντιμετωπίσετε τα προβλήματα επιδόσεων εφαρμογής web αργή στο [Azure εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=529714).

Εάν χρειάζεστε περισσότερη βοήθεια σε οποιοδήποτε σημείο σε αυτό το άρθρο, μπορείτε να επικοινωνήσετε με το Azure ειδικούς [το Azure MSDN](https://azure.microsoft.com/support/forums/)και τα φόρουμ υπερχείλισης στοίβας. Εναλλακτικά, μπορείτε επίσης να υποβάλετε ένα περιστατικό Azure υποστήριξης. Μεταβείτε στην [τοποθεσία υποστήριξης Azure](https://azure.microsoft.com/support/options/) και κάντε κλικ στην εντολή **Λήψη υποστήριξης**.

## <a name="symptom"></a>Σύμπτωμα

Όταν πραγματοποιείτε περιήγηση της εφαρμογής web, η φόρτωση σελίδες αργά και μερικές φορές χρονικού ορίου.

## <a name="cause"></a>Αιτία

Αυτό το πρόβλημα οφείλεται συχνά επιπέδου θέματα εφαρμογών, όπως:

-   αιτήσεις διαρκεί πολύ χρόνο
-   εφαρμογή με χρήση μνήμης υψηλής/CPU
-   εφαρμογή παρουσιάζει σφάλμα οφείλεται σε μια εξαίρεση.

## <a name="troubleshooting-steps"></a>Βήματα αντιμετώπισης προβλημάτων

Αντιμετώπιση προβλημάτων μπορεί να διαιρεθεί σε τρεις σημαντικές εργασίες, με διαδοχική σειρά:

1.  [Παρατηρήστε και την παρακολούθηση της συμπεριφοράς της εφαρμογής](#observe)
2.  [Συλλογή δεδομένων](#collect)
3.  [Μετριασμός το ζήτημα](#mitigate)

[Εφαρμογή υπηρεσίας Web Apps](/services/app-service/web/) σάς παρέχει διάφορες επιλογές σε κάθε βήμα.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. παρατηρήσετε και την παρακολούθηση της συμπεριφοράς της εφαρμογής

#### <a name="track-service-health"></a>Παρακολούθηση εύρυθμης λειτουργίας υπηρεσιών

Microsoft Azure publicizes κάθε φορά που υπάρχει μια υποβάθμιση υπηρεσίας διακοπές ή επιδόσεων. Μπορείτε να παρακολουθείτε την εύρυθμη λειτουργία της υπηρεσίας στην [Πύλη του Azure](https://portal.azure.com/). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Παρακολούθηση εύρυθμης λειτουργίας υπηρεσιών](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Παρακολούθηση της εφαρμογής σας web

Αυτή η επιλογή σάς επιτρέπει να διαπιστώσετε εάν η εφαρμογή σας αντιμετωπίζει προβλήματα. Στο blade της εφαρμογής σας web, κάντε κλικ στο πλακίδιο **αιτήσεις και σφάλματα** . Το blade **μετρικό σύστημα** θα σας δείξουν όλα τα μετρικά που μπορείτε να προσθέσετε.

Ορισμένα από τα μετρικά που μπορεί να θέλετε να παρακολουθείτε για την εφαρμογή web

-   Μέσος όρος μνήμη σύνολο εργασιών
-   Μέσος χρόνος απόκρισης
-   Χρόνος CPU
-   Το σύνολο εργασίας μνήμης
-   Προσκλήσεις

![παρακολούθηση των επιδόσεων του web app](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

-   [Εποπτεία εφαρμογών Web στο Azure εφαρμογής υπηρεσίας](web-sites-monitor.md)
-   [Λήψη ειδοποιήσεων υπενθύμισης](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Παρακολούθηση της κατάστασης τελικό σημείο web

Εάν χρησιμοποιείτε την εφαρμογή web της σε **Τυπική** τιμολόγησης επίπεδο, Web Apps σάς επιτρέπει να παρακολουθείτε 2 τελικά σημεία από διαφορετικές γεωγραφικές θέσεις 3.

Παρακολούθηση τελικού σημείου ρυθμίζει τις παραμέτρους δοκιμές web από κατανεμημένο παν θέσεις τις οποίες δοκιμή χρόνο απόκρισης και συνεχούς διευθύνσεις URL web. Η δοκιμή εκτελεί μια λειτουργία HTTP GET στη διεύθυνση URL του web για να καθορίσετε το χρόνο απόκρισης και συνεχούς από κάθε θέση. Κάθε ρυθμισμένη θέση εκτελείται μια δοκιμαστική κάθε πέντε λεπτά.

Συνεχούς παρακολουθείται χρησιμοποιώντας τους κωδικούς απόκρισης HTTP και χρόνο απόκρισης μετράται σε χιλιοστά του δευτερολέπτου. Δοκιμασία παρακολούθησης αποτυγχάνει εάν ο κώδικας απόκρισης HTTP είναι μεγαλύτερο ή ίσο με 400 ή εάν η απόκριση διαρκεί περισσότερο από 30 δευτερόλεπτα. Ένα τελικό σημείο θεωρείται διαθέσιμο Εάν ολοκληρωθεί με επιτυχία το παρακολούθησης δοκιμές από όλες τις καθορισμένες θέσεις.

Για να ρυθμίσετε, ανατρέξτε στο θέμα [Τρόπος: παρακολούθηση της κατάστασης τελικό σημείο web](web-sites-monitor.md#webendpointstatus).

Επίσης, ανατρέξτε στο θέμα [Διατήρηση τοποθεσίες Web Azure του καθώς και εποπτεία του τελικού σημείου - με Stefan Schackow](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) για ένα βίντεο σχετικά με την εποπτεία τελικού σημείου.

#### <a name="application-performance-monitoring-using-extensions"></a>Εφαρμογή παρακολούθηση των επιδόσεων χρησιμοποιώντας επεκτάσεις

Μπορείτε επίσης να παρακολουθήσετε την απόδοση της εφαρμογής από αξιοποίηση _επεκτάσεις τοποθεσίας_.

Κάθε εφαρμογή web εφαρμογής υπηρεσίας παρέχει ένα τελικό σημείο επεκτάσιμη διαχείρισης που σας επιτρέπει να αξιοποιήσετε ένα ισχυρό σύνολο εργαλείων αναπτυχθεί ως επεκτάσεις τοποθεσίας. Αυτά τα εργαλεία κυμαίνονται από συντάκτες κώδικα προέλευσης όπως το [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) για εργαλεία διαχείρισης για συνδεδεμένο τους πόρους, όπως μια βάση δεδομένων MySQL συνδεδεμένοι σε μια εφαρμογή web.

[Azure εφαρμογή ιδέες](/services/application-insights/) και [Νέα Relic](/marketplace/partners/newrelic/newrelic/) είναι δύο από την τοποθεσία επεκτάσεις που είναι διαθέσιμες παρακολούθηση των επιδόσεων. Για να χρησιμοποιήσετε νέα Relic, μπορείτε να εγκαταστήσετε έναν παράγοντα κατά το χρόνο εκτέλεσης. Για να χρησιμοποιήσετε Azure εφαρμογή ιδέες, εκ νέου δημιουργία του κώδικα με μια SDK και μπορείτε, επίσης, να εγκαταστήσετε μια επέκταση που παρέχει πρόσβαση σε πρόσθετα δεδομένα. Το SDK σάς επιτρέπει να γράψετε κώδικα για την παρακολούθηση της χρήσης και της απόδοσης της εφαρμογής με περισσότερες λεπτομέρειες.

Για να χρησιμοποιήσετε την εφαρμογή ιδέες, ανατρέξτε στο θέμα [Παρακολούθηση απόδοσης στις εφαρμογές web](../application-insights/app-insights-web-monitor-performance.md).

Για να χρησιμοποιήσετε νέα Relic, ανατρέξτε στο θέμα [Νέα διαχείριση επιδόσεων εφαρμογών Relic στην Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### <a name="2-collect-data"></a>2. συλλογής δεδομένων

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>Ενεργοποίηση της καταγραφής διαγνωστικών για την εφαρμογή web

Το περιβάλλον Web Apps παρέχει λειτουργικότητα διαγνωστικών για πληροφορίες καταγραφής από το διακομιστή web και την εφαρμογή web. Αυτά τα λογικά διαχωρίζονται σε διαγνωστικά διακομιστή web και τα Διαγνωστικά εφαρμογής.

##### <a name="web-server-diagnostics"></a>Διαγνωστικά διακομιστή Web

Μπορείτε να ενεργοποιήσετε ή να απενεργοποιήσετε τα παρακάτω είδη των αρχείων καταγραφής:

-   **Λεπτομερής καταγραφή σφαλμάτων** - λεπτομερείς πληροφορίες για τους κωδικούς κατάστασης HTTP που δηλώνουν αποτυχία (κωδικός κατάστασης 400 ή μεγαλύτερη). Αυτό μπορεί να περιέχει πληροφορίες που μπορούν να σας βοηθήσουν να προσδιορίσετε γιατί ο διακομιστής επέστρεψε τον κωδικό σφάλματος.
-   **Απέτυχε η αίτηση ανίχνευσης** - λεπτομερείς πληροφορίες σχετικά με αποτυχημένων αιτήσεων, όπως μια ανίχνευση τα στοιχεία των υπηρεσιών IIS που χρησιμοποιείται για την επεξεργασία της αίτησης και την ώρα που λαμβάνονται σε κάθε στοιχείο. Αυτό μπορεί να είναι χρήσιμο εάν προσπαθείτε να βελτιώσετε την απόδοση της εφαρμογής web ή να απομονώσετε τι προκαλεί ένα συγκεκριμένο σφάλμα HTTP.
-   **Καταγραφή διακομιστή web** - πληροφορίες σχετικά με τις συναλλαγές HTTP χρησιμοποιώντας τη μορφή αρχείου εκτεταμένης καταγραφής W3C. Αυτό είναι χρήσιμο κατά τον καθορισμό της συνολικής μετρικά εφαρμογής web, όπως τον αριθμό των χειρισμού των αιτήσεων ή πόσες αιτήσεις είναι από μια συγκεκριμένη διεύθυνση IP.

##### <a name="application-diagnostics"></a>Διαγνωστικά εφαρμογής

Εφαρμογή Διαγνωστικά σάς επιτρέπει να καταγράψετε τις πληροφορίες που παράγονται από μια εφαρμογή web. Εφαρμογές ASP.NET μπορούν να χρησιμοποιήσουν το `System.Diagnostics.Trace` κλάσης την καταγραφή πληροφοριών στο αρχείο καταγραφής διαγνωστικών εφαρμογών.

Για λεπτομερείς οδηγίες σχετικά με τον τρόπο ρύθμισης των παραμέτρων της εφαρμογής για καταγραφή, ανατρέξτε στο θέμα [Ενεργοποίηση καταγραφής για τις εφαρμογές web στην υπηρεσία εφαρμογής Azure Διαγνωστικά](web-sites-enable-diagnostic-log.md).

#### <a name="use-remote-profiling"></a>Χρήση απομακρυσμένου δημιουργία προφίλ

Στην υπηρεσία εφαρμογής Azure, εφαρμογές Web, εφαρμογών API και WebJobs μπορεί να είναι απομακρυσμένα επαγγελματικά. Εάν τη διαδικασία εκτελείται πιο αργά από το αναμενόμενο, ή η αδράνεια των αιτήσεων HTTP είναι μεγαλύτερη από το κανονικό και η χρήση της CPU της διεργασίας είναι επίσης υψηλό, μπορείτε να προφίλ τη διαδικασία και να λάβετε το CPU δειγματοληψία στοίβες κλήσης για την ανάλυση της διαδικασίας δραστηριότητας και της συντόμευσης διαδρομές κώδικα από απόσταση.

Για περισσότερες πληροφορίες σχετικά με, ανατρέξτε στο θέμα [Δημιουργία απομακρυσμένου προφίλ υποστήριξης στο Azure εφαρμογής υπηρεσίας](/blog/remote-profiling-support-in-azure-app-service).


#### <a name="use-the-azure-app-service-support-portal"></a>Χρησιμοποιήστε την πύλη υποστήριξη Azure εφαρμογής υπηρεσίας

Web Apps σάς δίνει τη δυνατότητα για την αντιμετώπιση προβλημάτων που σχετίζονται με την εφαρμογή web σας, εξετάζοντας HTTP αρχεία καταγραφής, αρχεία καταγραφής συμβάντων, οι ενδείξεις διαδικασία και άλλα. Μπορείτε να αποκτήσετε πρόσβαση σε όλες αυτές τις πληροφορίες με την πύλη υποστήριξης στο **http://&lt;το όνομα εφαρμογής >.scm.azurewebsites.net/Support**

Η πύλη του Azure εφαρμογής υπηρεσίας υποστήριξης που παρέχει με τρεις ξεχωριστές καρτέλες για την υποστήριξη τα τρία βήματα του ένα συνηθισμένο σενάριο αντιμετώπισης προβλημάτων:

1.  Παρατηρήστε τρέχουσα συμπεριφορά
2.  Ανάλυση από τη συλλογή πληροφοριών διαγνωστικών και εκτελεί την ενσωματωμένη αναλυτές
3.  Μετριασμός

Εάν το ζήτημα συμβαίνει αυτήν τη στιγμή, κάντε κλικ στην επιλογή **ανάλυση** > **Διαγνωστικά** > **Διάγνωση τώρα** για να δημιουργήσετε μια περίοδο λειτουργίας διαγνωστικών για εσάς, που θα συλλέγει HTTP συνδέεται, αρχεία καταγραφής πρόγραμμα προβολής συμβάντων, οι ενδείξεις, PHP αρχείων καταγραφής σφαλμάτων και PHP Επεξεργασία αναφοράς μνήμης.

Όταν τα δεδομένα που συλλέγονται, θα επίσης να εκτελέσετε μια ανάλυση τα δεδομένα και παρέχει μια αναφορά HTML.

Σε περίπτωση που θέλετε να κάνετε λήψη των δεδομένων, από προεπιλογή, θα είναι αποθηκευμένα στο φάκελο D:\home\data\DaaS.

Για περισσότερες πληροφορίες σχετικά με την πύλη Azure εφαρμογής υπηρεσίας υποστήριξης, ανατρέξτε στο θέμα [Νέες ενημερώσεις για επέκταση τοποθεσία υποστήριξης για τοποθεσίες Web Azure](/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Χρησιμοποιήστε την κονσόλα Kudu εντοπισμού σφαλμάτων

Εφαρμογές Web συνοδεύεται από μια κονσόλα εντοπισμού σφαλμάτων που μπορείτε να χρησιμοποιήσετε για τον εντοπισμό σφαλμάτων, Εξερεύνηση, αποστολή αρχείων, καθώς και τα τελικά σημεία JSON για τη λήψη πληροφοριών σχετικά με το περιβάλλον σας. Αυτό ονομάζεται στην _Κονσόλα Kudu_ ή του _Πίνακα εργαλείων SCM_ για την εφαρμογή web σας.

Μπορείτε να αποκτήσετε πρόσβαση σε αυτόν τον πίνακα εργαλείων, μεταβαίνοντας στη σύνδεση **https://&lt;το όνομα εφαρμογής >.scm.azurewebsites.net/**.

Ορισμένες δυνατότητες που παρέχει Kudu είναι:

-   ρυθμίσεις περιβάλλοντος για την εφαρμογή σας
-   ροή καταγραφής
-   ένδειξη διαγνωστικών
-   εντοπισμός σφαλμάτων κονσόλας με την οποία μπορείτε να εκτελέσετε βασικές εντολές DOS και cmdlet του Powershell.


Μια άλλη χρήσιμη δυνατότητα του Kudu είναι ότι, σε περίπτωση που η εφαρμογή σας είναι αποστελλόμενο εξαιρέσεις πρώτης ευκαιρίας, μπορείτε να χρησιμοποιήσετε Kudu και αποθηκεύει το εργαλείο SysInternals Procdump για να δημιουργήσετε μνήμης. Αυτές οι ενδείξεις μνήμης είναι στιγμιότυπα της διαδικασίας και συχνά μπορεί να σας βοηθήσει να αντιμετωπίσετε τα πιο σύνθετη προβλήματα με την εφαρμογή web.

Για περισσότερες πληροφορίες σχετικά με δυνατότητες που είναι διαθέσιμες στο Kudu, ανατρέξτε στο θέμα [Εργαλεία Azure τοποθεσίες Web ομάδας υπηρεσιών που θα πρέπει να γνωρίζετε σχετικά με](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. μετριασμός το ζήτημα

####    <a name="scale-the-web-app"></a>Κλίμακα του web app

Στην υπηρεσία εφαρμογής Azure, για αυξημένες επιδόσεις και μετάδοσης, μπορείτε να προσαρμόσετε την κλίμακα στην οποία εκτελείται η εφαρμογή σας. Η κλιμάκωση προς τα επάνω μια εφαρμογή web περιλαμβάνει δύο σχετικών ενεργειών: αλλάζοντας το πρόγραμμά σας εφαρμογής υπηρεσίας σε ανώτερο επίπεδο τιμολόγησης και τη ρύθμιση των παραμέτρων ορισμένες ρυθμίσεις, αφού έχετε τοποθετήσει στο ανώτερο επίπεδο τις πληροφορίες τιμολόγησης.

Για περισσότερες πληροφορίες σχετικά με την κλιμάκωση, ανατρέξτε στο θέμα [κλίμακα μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας](web-sites-scale.md).

Επιπλέον, μπορείτε να επιλέξετε να εκτελέσετε την εφαρμογή σας σε περισσότερες από μία παρουσία. Αυτό όχι μόνο σάς παρέχει περισσότερες δυνατότητες επεξεργασίας, αλλά επίσης παρέχει ορισμένα χρονικό ανοχή σφαλμάτων. Εάν η διαδικασία δεν λειτουργεί σε μία παρουσία, την άλλη παρουσία θα συνεχίσουν εξυπηρέτηση αιτήσεων.

Μπορείτε να ορίσετε την κλίμακα του να είναι εγχειρίδιο ή αυτόματος.

####    <a name="use-autoheal"></a>Χρήση AutoHeal

AutoHeal ανακυκλώνει διαδικασίας εργασίας για την εφαρμογή σας με βάση τις ρυθμίσεις που επιλέγετε (όπως αλλαγές στη ρύθμιση παραμέτρων, αιτήσεις, με βάση τη μνήμη όρια ή ο χρόνος που απαιτείται για την εκτέλεση μιας αίτησης). Τις περισσότερες φορές, Ανακύκλωσης της διαδικασίας είναι ο πιο γρήγορος τρόπος για την ανάκτηση από κάποιο πρόβλημα. Αν και μπορείτε πάντα να κάνετε επανεκκίνηση της εφαρμογής web από το απευθείας μέσα από την πύλη Azure, AutoHeal θα κάνει αυτόματα για εσάς. Πρέπει να κάνετε είναι να προσθέσετε κάποια από τα εναύσματα στο το web.config ρίζας για την εφαρμογή web σας. Σημειώστε ότι αυτές οι ρυθμίσεις θα λειτουργεί με τον ίδιο τρόπο, ακόμα και εάν η εφαρμογή σας δεν είναι μια .net ένα.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αυτόματης Διόρθωσης Azure τοποθεσίες Web](/blog/auto-healing-windows-azure-web-sites/).

####    <a name="restart-the-web-app"></a>Επανεκκινήστε την εφαρμογή web

Αυτό είναι συχνά ο πιο απλός τρόπος για την ανάκτηση από εφάπαξ θέματα. Στην [Πύλη του Azure](https://portal.azure.com/), σε blade της εφαρμογής web σας, έχετε τις επιλογές για να διακόψετε ή να επανεκκινήσετε την εφαρμογή σας.

 ![Επανεκκινήστε την εφαρμογή web για να επιλύσετε θέματα απόδοσης](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Μπορείτε, επίσης, να διαχειριστείτε την εφαρμογή web με χρήση του Powershell Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure](../powershell-azure-resource-manager.md).