<properties 
    pageTitle="Σημειώσεις έκδοσης ανάλυση ροή | Microsoft Azure" 
    description="Σημειώσεις έκδοσης ανάλυση ροής" 
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
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="stream-analytics-release-notes"></a>Σημειώσεις έκδοσης ροή ανάλυσης

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση 04/15/2016 του ροή ανάλυσης ##

Σε αυτήν την έκδοση περιέχει την ακόλουθη ενημερωμένη έκδοση.

Τίτλος | Περιγραφή
---|---
Εξάγει γενικής διαθεσιμότητας για το Power BI  | [Εξάγει Power BI](stream-analytics-power-bi-dashboard.md) είναι τώρα διαθέσιμα Γενικά. Στη λήξη εξουσιοδότησης 90 ημερών για το Power BI έχει καταργηθεί. Για περισσότερες πληροφορίες σχετικά με σενάρια όπου εξουσιοδότησης πρέπει να ανανεωθεί, ανατρέξτε στην ενότητα [εξουσιοδότηση ανανέωση](stream-analytics-power-bi-dashboard.md#Renew-authorization) της δημιουργίας ενός πίνακα εργαλείων του Power BI.

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση 03/03/2016 του ροή ανάλυσης ##

Σε αυτήν την έκδοση περιέχει την ακόλουθη ενημερωμένη έκδοση.

Τίτλος | Περιγραφή
---|---
Νέα στοιχεία γλώσσας ερωτήματος ανάλυσης ροής  | SAQL τώρα διαθέτει [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "Σελίδα MSDN GetType"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "Σελίδα MSDN TRY_CAST") και [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH MSDN σελίδας").

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση της ροής ανάλυση 10/12/2015 ##

Σε αυτήν την έκδοση περιέχει την ακόλουθη ενημερωμένη έκδοση.

Τίτλος | Περιγραφή
---|---
Ενημέρωση έκδοση REST API | Η έκδοση REST API έχει ενημερωθεί σε 2015 10--01. Μπορείτε να βρείτε λεπτομέρειες στο MSDN στο [Ροή ανάλυση διαχείρισης REST API αναφορά](https://msdn.microsoft.com/library/azure/dn835031.aspx) και την [ενοποίηση μηχανικής εκμάθησης στην ανάλυση ροής](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md).
Ενοποίηση του Azure μηχανικής εκμάθησης | Με αυτήν την έκδοση παρέχεται υποστήριξη για Azure μηχανικής εκμάθησης συναρτήσεις που ορίζονται από το χρήστη. Ανατρέξτε στο θέμα το [πρόγραμμα εκμάθησης](stream-analytics-machine-learning-integration-tutorial.md) για περισσότερες πληροφορίες, καθώς και την [ανακοίνωση γενικά ιστολογίου](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx).

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση 12/11/2015 του ροή ανάλυσης ##

Σε αυτήν την έκδοση περιέχει την ακόλουθη ενημερωμένη έκδοση.

Τίτλος | Περιγραφή
---|---
ΕΠΙΛΈΞΤΕ νέα συμπεριφορά | ΕΠΙΛΈΞΤΕ στην ανάλυση ροή έχει επεκταθεί για να επιτρέψετε * ως ένα στοιχείο πρόσβασης ιδιότητας των ένθετων εγγραφής. Για περισσότερες πληροφορίες, συμβουλευτείτε [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Σύνθετους τύπους δεδομένων").

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση της ροής ανάλυση 22/10/2015 ##

Σε αυτήν την έκδοση περιλαμβάνει τις ακόλουθες ενημερώσεις.

Τίτλος | Περιγραφή
---|---
Δυνατότητες γλωσσών πρόσθετες ερωτήματος | Ανάλυση ροή έχει αναπτυχθεί τη γλώσσα ερωτήματος, συμπεριλαμβάνοντας τις εξής δυνατότητες: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx), [CEILING](https://msdn.microsoft.com/library/azure/mt605286.aspx), [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [FLOOR](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [ΕΙΣΌΔΟΥ](https://msdn.microsoft.com/library/azure/mt605290.aspx), [ΤΕΤΡΆΓΩΝΟ](https://msdn.microsoft.com/library/azure/mt605288.aspx)και [SQRT](https://msdn.microsoft.com/library/azure/mt605238.aspx).
Κατάργηση περιορισμοί συγκεντρωτικών αποτελεσμάτων  | Σε αυτήν την έκδοση καταργεί τον περιορισμό των 15 συγκεντρώσεων σε ένα ερώτημα. Τώρα δεν υπάρχει όριο στον αριθμό των συγκεντρώσεων ανά ερώτημα.
Προστέθηκε η δυνατότητα ΟΜΆΔΑΣ από System.Timestamp | Η συνάρτηση [ΟΜΑΔΟΠΟΊΗΣΗ κατά](https://msdn.microsoft.com/library/azure/dn835023.aspx) σάς επιτρέπει πλέον για window_type ή [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx).
Προστέθηκε ΜΕΤΑΤΌΠΙΣΗ για Tumbling και διασκέδαση των windows | Από προεπιλογή, [Tumbling](https://msdn.microsoft.com/library/azure/dn835055.aspx) και [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) των windows είναι στοιχισμένο με το χρόνο μηδέν (1/1/0001 UTC 12:00:00 πμ). Η νέα (προαιρετικό) παράμετρος 'offsetsize' επιτρέπει καθορίζοντας ένα προσαρμοσμένο offset (ή στοίχιση).


## <a name="notes-for-09292015-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση 29/09/2015 του ροή ανάλυσης ##

Σε αυτήν την έκδοση περιλαμβάνει τις ακόλουθες ενημερώσεις.

Τίτλος | Περιγραφή
---|---
Προεπισκόπηση δημόσια οικογένεια Azure IoT | Ανάλυση ροή περιλαμβάνεται στην προεπισκόπηση της οικογένειας προγραμμάτων του Azure IoT δημόσια.
Ενοποίηση του Azure πύλη | Εκτός από τη συνεχή παρουσία στην πύλη διαχείρισης Azure, ανάλυση ροή έχει πλέον ενοποιηθεί στην [Πύλη του Azure](https://azure.microsoft.com/overview/preview-portal/). Σημειώστε ότι ανάλυση ροή λειτουργικότητα στην πύλη του Preview είναι αυτήν τη στιγμή προσφέρεται ένα υποσύνολο των λειτουργιών της στην πύλη διαχείρισης Azure, χωρίς την υποστήριξη για τη δοκιμή ερωτήματος στο πρόγραμμα περιήγησης, Power BI εξόδου ρύθμιση παραμέτρων, και περιήγηση σε ή τη δημιουργία νέα εισόδου και εξόδου πόρων σε συνδρομές του έχετε πρόσβαση.
Υποστήριξη για DocumentDB εξόδου | Ροή εργασιών ανάλυσης τώρα να έξοδο σε [DocumentDB](https://azure.microsoft.com/services/documentdb/).
Υποστήριξη για διανομέα IoT εισαγωγής | Ροή εργασιών ανάλυσης τώρα να ingest δεδομένα από IoT διανομείς.
Χρονική ΣΉΜΑΝΣΗ από για ετερογενή συμβάντα | Όταν μια ροή δεδομένων μονής ακρίβειας περιέχει πολλούς τύπους συμβάντων αντιμετωπίζετε χρονικές σημάνσεις σε διαφορετικά πεδία, μπορείτε να τώρα χρησιμοποιήσετε [Χρονικής ΣΉΜΑΝΣΗΣ,](http://msdn.microsoft.com/library/mt573293.aspx) με τις παραστάσεις για να καθορίσετε διαφορετικό χρονικής σήμανσης πεδία για κάθε υπόθεση.

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση 10/09/2015 του ροή ανάλυσης ##

Σε αυτήν την έκδοση περιλαμβάνει τις ακόλουθες ενημερώσεις.

Τίτλος|Περιγραφή
---|---
Υποστήριξη για ομάδες PowerBI|Για να ενεργοποιήσετε την κοινή χρήση δεδομένων με άλλους χρήστες του Power BI, ανάλυση ροή εργασιών να τώρα γράψετε σε [ομάδες PowerBI](stream-analytics-define-outputs.md#power-bi) μέσα σε λογαριασμό σας Power BI.

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση 08/20/2015 του ροή ανάλυσης ##

Σε αυτήν την έκδοση περιλαμβάνει τις ακόλουθες ενημερώσεις.

Τίτλος|Περιγραφή
---|---
Προσθέσατε ΤΕΛΕΥΤΑΊΑ συνάρτηση |Η συνάρτηση [ΤΕΛΕΥΤΑΊΑ](http://msdn.microsoft.com/library/mt421186.aspx) είναι πλέον διαθέσιμη σε ανάλυση ροής εργασιών, μπορείτε να ανακτήσετε το πιο πρόσφατο συμβάν σε μια ροή συμβάν σε ένα συγκεκριμένο χρονικό πλαίσιο.
Νέες συναρτήσεις πίνακα|Συναρτήσεις πίνακα [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) και [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) είναι πλέον διαθέσιμες.
Νέες συναρτήσεις εγγραφής|Εγγραφή συναρτήσεις [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) και [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) είναι πλέον διαθέσιμες.

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση 30/07/2015 του ροή ανάλυσης ##

Σε αυτήν την έκδοση περιλαμβάνει τις ακόλουθες ενημερώσεις.

Τίτλος|Περιγραφή
---|---
Αναγνωριστικό εταιρείας BI Power αποσυνδεδεμένη από αναγνωριστικό Azure|Αυτή η δυνατότητα επιτρέπει [εξόδου Power BI](stream-analytics-power-bi-dashboard.md) για ASA εργασίες κάτω από οποιονδήποτε τύπο λογαριασμού Azure (Live Id ή αναγνωριστικό εταιρείας). Επιπλέον, μπορείτε να έχετε ένα αναγνωριστικό εταιρείας για το λογαριασμό σας Azure και να χρησιμοποιήσετε ένα διαφορετικό για να εξουσιοδοτήσετε εξόδου Power BI.
Υποστήριξη για την υπηρεσία Bus ουρές εξόδου|[Υπηρεσία Bus ουρές](stream-analytics-define-outputs.md#service-bus-queues) εξόδους είναι πλέον διαθέσιμες σε ανάλυση ροή εργασιών.
Υποστήριξη για θέματα Bus υπηρεσία εξόδου|[Θέματα Bus υπηρεσία](stream-analytics-define-outputs.md#service-bus-topics) εκροές είναι πλέον διαθέσιμες σε ανάλυση ροή εργασιών.

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση της ροής ανάλυση 09/07/2015 ##

Σε αυτήν την έκδοση περιλαμβάνει τις ακόλουθες ενημερώσεις.


Τίτλος|Περιγραφή
---|---
Προσαρμοσμένη αντικειμένων Blob διαμερισμάτων εξόδου|Εξόδους χώρο αποθήκευσης αντικειμένων blob επιτρέπουν τώρα μια επιλογή για να καθορίσετε τη συχνότητα που είναι γραμμένες αντικείμενα BLOB εξόδου και της δομής και μορφοποίηση της δομής εξόδου δεδομένων διαδρομή φακέλου. 

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση της ροής ανάλυση 03/05/2015 ##

Σε αυτήν την έκδοση περιλαμβάνει τις ακόλουθες ενημερώσεις.


Τίτλος|Περιγραφή
---|---
Αυξημένη μέγιστη τιμή για το παράθυρο ανοχή εκτός σειράς|Το μέγιστο μέγεθος για το παράθυρο ανοχή εκτός σειράς είναι τώρα 59:59 (λλ: δδ)
Μορφή εξόδου JSON: Γραμμής διαχωρισμένα ή έναν πίνακα|Τώρα υπάρχει μια επιλογή όταν αποθηκεύεται το χώρο αποθήκευσης αντικειμένων Blob ή διανομέα συμβάντος για να εξαγάγετε ως είτε πίνακας JSON αντικειμένων ή διαχωρίζοντας JSON αντικείμενα με μια νέα γραμμή. 

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Σημειώσεις για την έκδοση 16/04/2015 του ροή ανάλυσης ##


Τίτλος|Περιγραφή
---|---
Καθυστέρηση στη ρύθμιση παραμέτρων λογαριασμού αποθήκευσης Azure|Κατά τη δημιουργία μιας εργασίας ροής αναλυτικών στοιχείων σε μια περιοχή για πρώτη φορά, θα σας ζητηθεί να δημιουργήσετε έναν νέο λογαριασμό χώρου αποθήκευσης ή να καθορίσετε έναν υπάρχοντα λογαριασμό για παρακολούθηση ανάλυση ροή εργασιών σε αυτήν την περιοχή. Λόγω λανθάνων χρόνος με τη ρύθμιση παραμέτρων παρακολούθησης, δημιουργώντας μια άλλη εργασία ροής ανάλυσης στην ίδια περιοχή εντός 30 λεπτών θα γίνεται ερώτηση για να τα καθορίσετε από ένα δεύτερο λογαριασμό χώρου αποθήκευσης, αντί για αυτό πρόσφατα έχει ρυθμιστεί στο παρακολούθηση λογαριασμού χώρου αποθήκευσης αναπτυσσόμενο μενού που εμφανίζει την. Για να αποφύγετε τη δημιουργία λογαριασμού χώρου αποθήκευσης δεν είναι απαραίτητες, περιμένετε 30 λεπτά μετά τη δημιουργία μιας εργασίας σε μια περιοχή για πρώτη φορά πριν από την προμήθεια πρόσθετες εργασίες σε αυτήν την περιοχή.
Αναβάθμιση εργασίας|Προς το παρόν, ροή ανάλυση δεν υποστηρίζει ζωντανή αλλαγές στον ορισμό ή ρύθμιση παραμέτρων για μια εργασία που εκτελείται. Για να αλλάξετε την είσοδο, έξοδο, ερώτημα, κλίμακα ή ρύθμιση παραμέτρων για μια εργασία που εκτελείται, πρέπει πρώτα να διακόψετε την εργασία.
Τύποι δεδομένων που προκύπτουν από προέλευσης εισαγωγής|Εάν δεν χρησιμοποιείται μια πρόταση CREATE TABLE, ο τύπος εισόδου προέρχεται από μορφή εισόδου, για παράδειγμα όλα τα πεδία από CSV είναι συμβολοσειρά. Τα πεδία πρέπει να μετατραπούν ρητά το σωστό τύπο με χρήση της συνάρτησης CAST για να αποφύγετε σφάλματα ασυμφωνία τύπου.
Πεδία που λείπουν είναι εκτυπωθεί ως τιμές null|Αναφορά σε ένα πεδίο που δεν υπάρχει στο αρχείο προέλευσης εισαγωγής θα έχει ως αποτέλεσμα οι τιμές null το συμβάν εξόδου.
ΜΕ δηλώσεις πρέπει να προηγείται προτάσεις SELECT|Στο ερώτημά σας, οι προτάσεις SELECT πρέπει να ακολουθήσετε δευτερεύοντα ερωτήματα που ορίζονται από το με δηλώσεις.
Το ζήτημα εκτός ανεπαρκούς μνήμης|Επανεκκινήστε ροής εργασιών ανάλυσης με μεγάλο ανοχή για συμβάντα εκτός σειράς ή/και σύνθετα ερωτήματα τη διατήρηση σε μεγάλο όγκο κατάσταση μπορεί να προκαλέσει την εργασία να εξαντλείται ο μνήμη, με αποτέλεσμα μια εργασία. Οι λειτουργίες έναρξης και τερματισμού θα είναι ορατά στα αρχεία καταγραφής λειτουργία του έργου. Για να αποφύγετε αυτήν τη συμπεριφορά, κλίμακα το ερώτημα θέμα σε περισσότερα από ένα διαμερίσματα. Σε μια μελλοντική έκδοση, αυτός ο περιορισμός θα αποσταλεί με μειωθεί η απόδοση στην επηρεαζόμενη εργασίες αντί για επανεκκίνηση τους.
Μεγάλη blob εισόδων χωρίς φορτίο χρονικής σήμανσης μπορεί να προκαλέσει το ζήτημα εκτός ανεπαρκούς μνήμης|Κατανάλωση μεγάλων αρχείων από χώρο αποθήκευσης αντικειμένων Blob μπορεί να προκαλέσει την ανάλυση ροή εργασιών σφάλμα εάν δεν έχει καθοριστεί ένα πεδίο χρονικής σήμανσης μέσω χρονικής ΣΉΜΑΝΣΗΣ κατά. Για να αποφύγετε αυτό το ζήτημα, μην κάθε blob μικρότερα από 10MB μέγεθος.
Βάση δεδομένων SQL του συμβάντος ένταση περιορισμός|Κατά τη χρήση της βάσης δεδομένων SQL ως ένα προορισμός εξόδου, πολύ υψηλή όγκους δεδομένων εξόδου μπορεί να προκαλέσει την εργασία ροής ανάλυση χρονικού ορίου. Για να επιλύσετε αυτό το ζήτημα, μειώστε την ένταση εξόδου, χρησιμοποιώντας συγκεντρωτικά αποτελέσματα ή τελεστές φίλτρου ή επιλέξτε χώρο αποθήκευσης αντικειμένων Blob του Azure ή διανομείς συμβάν ως ένα προορισμός εξόδου.
Σύνολα δεδομένων PowerBI μπορεί να περιέχει μόνο έναν πίνακα|PowerBI δεν υποστηρίζει περισσότερα από ένα πίνακα σε ένα συγκεκριμένο σύνολο δεδομένων.

## <a name="get-help"></a>Λήψη Βοήθειας
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ ανάλυση ροή Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 