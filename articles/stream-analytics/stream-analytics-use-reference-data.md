<properties
    pageTitle="Χρήση πινάκων δεδομένων "και" Αναζήτηση αναφοράς στην ανάλυση ροή | Microsoft Azure"
    description="Χρήση αναφοράς δεδομένων σε ένα ερώτημα ροή ανάλυσης"
    keywords="πίνακας αναζήτησης, δεδομένα αναφοράς"
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

# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Χρήση πινάκων δεδομένων ή αναζήτησης αναφοράς σε μια ροή εισαγωγής ροή ανάλυσης

Αναφορά δεδομένων (γνωστό και ως πίνακα αναζήτησης) είναι ένα ορισμένο σύνολο δεδομένων που είναι στατική ή επιβράδυνση αλλάζει στο φύσης, χρησιμοποιούνται για την εκτέλεση μιας αναζήτησης ή για να συσχετίσετε με το ροής δεδομένων. Για να κάνετε χρήση των δεδομένων αναφοράς στην εργασία σας Azure ροή ανάλυσης που θα είναι γενικά χρησιμοποιήσετε έναν [Συμμετοχή δεδομένων αναφοράς](https://msdn.microsoft.com/library/azure/dn949258.aspx) στο ερώτημά σας. Ανάλυση ροή χρησιμοποιεί χώρο αποθήκευσης αντικειμένων Blob του Azure ως το επίπεδο του χώρου αποθήκευσης για τα δεδομένα αναφοράς, και με αναφορά εργοστασίου δεδομένων Azure δεδομένων μπορεί να είναι μετασχηματισμό ή/και αντιγραφεί σε χώρο αποθήκευσης αντικειμένων Blob του Azure, για χρήση ως δεδομένα αναφοράς, από [οποιονδήποτε αριθμό cloud με βάση και εσωτερικής χώροι αποθήκευσης δεδομένων](../data-factory/data-factory-data-movement-activities.md). Δεδομένα αναφοράς διαμορφώνεται ως μια ακολουθία αντικείμενα BLOB (που καθορίζεται στη ρύθμιση παραμέτρων εισαγωγής) σε αύξουσα σειρά από την ημερομηνία/ώρα που καθορίζεται στο όνομα του blob. Το υποστηρίζει **μόνο** την προσθήκη μέχρι το τέλος της ακολουθίας χρησιμοποιώντας μια ημερομηνία/ώρα **μεγαλύτερη** από αυτήν που καθορίζεται από την τελευταία blob στη σειρά.

## <a name="configuring-reference-data"></a>Ρύθμιση παραμέτρων δεδομένων αναφοράς

Για να ρυθμίσετε τα δεδομένα αναφοράς σας, πρέπει πρώτα να δημιουργήσετε μια εισαγωγή δεδομένων που είναι του τύπου **Δεδομένων αναφοράς**. Ο παρακάτω πίνακας περιγράφει κάθε ιδιότητα που θα χρειαστεί να παρέχετε κατά τη δημιουργία την εισαγωγή δεδομένων αναφοράς με την περιγραφή:

<table>
<tbody>
<tr>
<td>Όνομα ιδιότητας</td>
<td>Περιγραφή</td>
</tr>
<tr>
<td>Ψευδώνυμο εισαγωγής</td>
<td>Ένα φιλικό όνομα που θα χρησιμοποιηθεί στο ερώτημα εργασία για να αναφέρονται σε αυτήν την είσοδο.</td>
</tr>
<tr>
<td>Το λογαριασμό χώρου αποθήκευσης</td>
<td>Το όνομα του λογαριασμού χώρου αποθήκευσης όπου βρίσκονται τα αρχεία σας blob. Εάν είναι στην ίδια συνδρομή ως εργασίας ανάλυση ροής, μπορείτε να το επιλέξετε από το αναπτυσσόμενο μενού προς τα κάτω.</td>
</tr>
<tr>
<td>Κλειδί λογαριασμού χώρου αποθήκευσης</td>
<td>Το μυστικό κλειδί που σχετίζεται με το λογαριασμό χώρου αποθήκευσης. Αυτό λαμβάνει συμπληρώνονται αυτόματα εάν ο λογαριασμός του χώρου αποθήκευσης είναι στην ίδια συνδρομή του ως την ανάλυση ροής εργασίας.</td>
</tr>
<tr>
<td>Κοντέινερ χώρου αποθήκευσης</td>
<td>Κοντέινερ παρέχουν μια λογική ομαδοποίηση για αντικείμενα BLOB που είναι αποθηκευμένα στην υπηρεσία Microsoft Azure Blob. Όταν κάνετε αποστολή μιας blob στην υπηρεσία Blob, πρέπει να καθορίσετε ένα κοντέινερ για το αντικείμενο blob.</td>
</tr>
<tr>
<td>Διαδρομή μοτίβου</td>
<td>Η διαδρομή του αρχείου που χρησιμοποιείται για να εντοπίσετε το αντικείμενα BLOB μέσα σε καθορισμένο κοντέινερ. Εντός της διαδρομής, μπορείτε να επιλέξετε για να καθορίσετε μία ή περισσότερες παρουσίες του τις παρακάτω μεταβλητές 2:<BR>{date} {χρόνος}<BR>Παράδειγμα 1: products/{date}/{time}/product-list.csv<BR>Παράδειγμα 2: products/{date}/product-list.csv
</tr>
<tr>
<td>Μορφή ημερομηνίας [προαιρετικό]</td>
<td>Εάν έχετε χρησιμοποιήσει {ημερομηνία} εντός του μοτίβου διαδρομή που καθορίσατε, στη συνέχεια, μπορείτε να επιλέξετε τη μορφή ημερομηνίας στην οποία είναι οργανωμένα τα αρχεία σας από το αναπτυσσόμενο υποστηριζόμενες μορφές αρχείων. Παράδειγμα: Εεεε/μμ/ηη</td>
</tr>
<tr>
<td>Μορφή ώρας [προαιρετικό]</td>
<td>Εάν έχετε χρησιμοποιήσει {χρόνος} εντός του μοτίβου διαδρομή που καθορίσατε, στη συνέχεια, μπορείτε να επιλέξετε τη μορφή ώρας στην οποία είναι οργανωμένα τα αρχεία σας από το αναπτυσσόμενο υποστηριζόμενες μορφές αρχείων. Παράδειγμα: HH</td>
</tr>
<tr>
<td>Μορφή σειριοποίησης συμβάντος</td>
<td>Για να βεβαιωθείτε ότι τα ερωτήματά σας λειτουργεί με τον τρόπο που περιμένατε, ροή ανάλυσης πρέπει να γνωρίζετε ποια μορφή σειριοποίησης που χρησιμοποιείτε για εισερχόμενες ροές δεδομένων. Για τα δεδομένα αναφοράς, τις μορφές που υποστηρίζονται είναι CSV και JSON.</td>
</tr>
<tr>
<td>Κωδικοποίηση</td>
<td>Είναι η μόνη υποστηριζόμενη μορφή κωδικοποίησης UTF-8 αυτήν τη στιγμή</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Δημιουργία αναφοράς δεδομένων σε ένα χρονοδιάγραμμα

Εάν τα δεδομένα αναφοράς σας είναι ένα σύνολο δεδομένων αργά αλλαγή, στη συνέχεια, υποστηρίζει για την ανανέωση δεδομένων είναι ενεργοποιημένη, καθορίζοντας ένα μοτίβο διαδρομή στη ρύθμιση παραμέτρων εισαγωγής χρησιμοποιώντας την ημερομηνία {} αναφοράς και {χρόνου} διακριτικά υποκατάστασης. Ανάλυση ροή θα σηκώστε το ορισμών δεδομένων ενημερωμένων αναφοράς που βασίζονται σε αυτό το μοτίβο διαδρομή. Για παράδειγμα, ένα μοτίβο `sample/{date}/{time}/products.csv` με μια μορφή ημερομηνίας **"MM-DD-YYYY"** χρόνο και μια μορφή **"Ωω: λλ"** καθοδηγεί ροή αναλύσεις για σηκώστε το ενημερωμένο blob `sample/2015-04-16/17:30/products.csv` στις 5:30 μ.μ. Απριλίου 2015 16ο UTC ζώνη ώρας.

> [AZURE.NOTE] Προς το παρόν ανάλυση ροή εργασιών αναζητήστε η ανανέωση blob μόνο όταν η ώρα του υπολογιστή προωθεί την ώρα κωδικοποιημένη στο όνομα του blob. Για παράδειγμα θα αναζητήσει το έργο `sample/2015-04-16/17:30/products.csv` μόλις δυνατόν αλλά όχι νωρίτερα από 5:30 μ.μ. Απριλίου 2015 16ο UTC ζώνη ώρας. Αυτό θα *ποτέ* εμφάνισης για ένα αρχείο με παλαιότερη από το τελευταίο αυτό που είναι εντόπισε ένα κωδικοποιημένο ώρα.
> 
> Π.χ. Όταν η εργασία εντοπίζει το αντικείμενο blob `sample/2015-04-16/17:30/products.csv` αυτό θα αγνοήσει τα αρχεία με μια κωδικοποιημένη ημερομηνία προγενέστερη 05:30 μμ 16ο Απρίλιος 2015 επομένως εάν μια καθυστερημένη φτάνει `sample/2015-04-16/17:25/products.csv` blob λαμβάνει δημιουργήθηκε στο ίδιο κοντέινερ η εργασία δεν θα χρησιμοποιήσει το.
> 
> Παρομοίως εάν `sample/2015-04-16/17:30/products.csv` μόνο παράγεται 10:03 μμ 16ο Απρίλιος 2015 αλλά χωρίς blob με μια παλαιότερη ημερομηνία που υπάρχει στο κοντέινερ, η εργασία θα χρησιμοποιήσετε αυτό το αρχείο ξεκινώντας από 10:03 μμ 16ο Απρίλιος 2015 και θα χρησιμοποιήσει τα προηγούμενα δεδομένα αναφοράς μέχρι τότε.
> 
> Μια εξαίρεση σε αυτό είναι όταν η εργασία πρέπει να επεξεργαστεί ξανά δεδομένων πίσω στο χρόνο ή όταν η εργασία είναι πρώτη αποτελέσματα. Ώρα έναρξης της εργασίας είναι αναζητάτε το πιο πρόσφατο blob παραχθεί πριν από την εργασία που καθορίζεται ώρα έναρξης. Αυτό γίνεται για να βεβαιωθείτε ότι υπάρχει ένα σύνολο δεδομένων **δεν είναι κενή** αναφορά κατά την εκκίνηση της εργασίας. Εάν δεν είναι δυνατό να βρεθεί ένα, η εργασία θα εμφανίσει το εξής diagnostic: `Initializing input without a valid reference data blob for UTC time <start time>`.


[Azure εργοστασίου δεδομένων](https://azure.microsoft.com/documentation/services/data-factory/) μπορεί να χρησιμοποιηθεί για να οργανώσετε την εργασία της δημιουργίας το ενημερωμένο αντικείμενα BLOB απαιτούνται από ροή ανάλυσης για την ενημέρωση ορισμών δεδομένων αναφοράς. Προέλευση δεδομένων είναι μια υπηρεσία ενοποίησης δεδομένων που βασίζεται στο cloud που orchestrates και αυτοματοποιεί την κίνηση και Μετασχηματισμός των δεδομένων. Προέλευση δεδομένων υποστηρίζει [τη σύνδεση με ένα μεγάλο αριθμό που βασίζεται στο cloud και αποθηκεύει δεδομένα εσωτερικής εγκατάστασης](../data-factory/data-factory-data-movement-activities.md) και μετακίνηση δεδομένων εύκολα σε τακτά χρονικά διαστήματα που καθορίζετε. Για περισσότερες πληροφορίες και οδηγίες βήμα προς βήμα σχετικά με τον τρόπο για να ρυθμίσετε μια διαδικασία εργοστασίου δεδομένων για τη δημιουργία αναφοράς δεδομένων για ανάλυση ροής που ανανεώνεται σε προκαθορισμένες χρονοδιάγραμμα, δείτε αυτό το [δείγμα GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Συμβουλές σχετικά με την ανανέωση δεδομένων της αναφοράς σας ##

1. Αντικατάσταση αναφορά δεδομένα BLOB δεν θα προκαλέσει την ανάλυση ροής για να φορτώσετε ξανά το αντικείμενο blob και σε ορισμένες περιπτώσεις, μπορεί να προκαλέσει την αποτυχία της εργασίας. Είναι το συνιστώμενο τρόπο για να αλλάξετε τα δεδομένα αναφοράς για να προσθέσετε ένα νέο blob χρησιμοποιώντας το ίδιο κοντέινερ και διαδρομή μοτίβου που ορίζονται από το στην εργασία είσοδο και να χρησιμοποιήσετε μια ημερομηνία/ώρα **μεγαλύτερη** από αυτήν που καθορίζεται από την τελευταία blob στη σειρά.
2.  Τα δεδομένα αναφοράς αντικείμενα BLOB δεν είναι ταξινομημένες κατά το αντικείμενο blob "Τελευταία τροποποίηση" ώρα, αλλά μόνο από το χρόνο και την ημερομηνία που καθορίζεται στο αντικείμενο blob όνομα χρησιμοποιώντας την ημερομηνία {} και {χρόνου} υποκατάστατα.
3.  Μερικές φορές μια εργασία πρέπει να επιστρέψετε στο χρόνο, επομένως αναφορά δεδομένα BLOB πρέπει να δεν είναι τροποποιηθεί ή διαγραφεί.

## <a name="get-help"></a>Λήψη Βοήθειας
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ ανάλυση ροή Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Επόμενα βήματα
Έχετε έχει εισαχθεί ανάλυση ροής, μια διαχειριζόμενη υπηρεσία για ροή ανάλυση σε δεδομένα από το Internet πράγματα. Για να μάθετε περισσότερα σχετικά με αυτήν την υπηρεσία, ανατρέξτε στα θέματα:

- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301