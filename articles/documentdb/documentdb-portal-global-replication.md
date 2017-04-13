<properties
    pageTitle="Αναπαραγωγή καθολικού βάσης δεδομένων DocumentDB | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να διαχειριστείτε την καθολική αναπαραγωγής του λογαριασμού σας DocumentDB μέσω της πύλης Azure."
    services="documentdb"
    keywords="καθολικό βάσης δεδομένων, η αναπαραγωγή"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>Πώς να εκτελείτε DocumentDB αναπαραγωγής καθολικού βάσης δεδομένων με την πύλη Azure

Μάθετε πώς να χρησιμοποιήσουν την πύλη του Azure για την αναπαραγωγή δεδομένων σε πολλές περιοχές για καθολική της διαθεσιμότητας των δεδομένων σε Azure DocumentDB.

Για πληροφορίες σχετικά με τον τρόπο καθολικού βάσης δεδομένων λειτουργίας στο DocumentDB αναπαραγωγής, ανατρέξτε στο θέμα [δεδομένα κατανομή καθολικά με DocumentDB](documentdb-distribute-data-globally.md). Για πληροφορίες σχετικά με την εκτέλεση μέσω προγραμματισμού αναπαραγωγή καθολικού βάσης δεδομένων, ανατρέξτε στο θέμα [αναπτυσσόμενες με λογαριασμούς DocumentDB πολλών περιοχών](documentdb-developing-with-multiple-regions.md).

> [AZURE.NOTE] Καθολικό κατανομή των βάσεων δεδομένων DocumentDB είναι γενικά διαθέσιμο και ενεργοποιηθεί αυτόματα για οποιονδήποτε λογαριασμό DocumentDB που μόλις δημιουργήθηκε. Προσπαθούμε να ενεργοποιήσετε την καθολική διανομή σε όλους τους λογαριασμούς υπάρχουσα, αλλά στο μεταξύ, εάν θέλετε το καθολικό κατανομή με δυνατότητα στο λογαριασμό σας, επικοινωνήστε [Επικοινωνήστε με την υποστήριξη](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) και θα σας θα ενεργοποιήσετε για εσάς τώρα.

## <a id="addregion"></a>Προσθήκη περιοχών καθολικού βάσης δεδομένων

DocumentDB είναι διαθέσιμο σε περισσότερες [περιοχές Azure] [azureregions]. Αφού επιλέξετε το προεπιλεγμένο επίπεδο συνέπειας για το λογαριασμό σας βάση δεδομένων, μπορείτε να συσχετίσετε μία ή περισσότερες περιοχές (ανάλογα με την επιλογή σας ανάγκες επιπέδου και καθολικού διανομής συνέπειας προεπιλεγμένη).

1. Στην [πύλη του Azure](https://portal.azure.com/), με το Jumpbar, κάντε κλικ στην επιλογή **Λογαριασμοί DocumentDB**.
2. Στο το blade **DocumentDB λογαριασμού** , επιλέξτε το λογαριασμό της βάσης δεδομένων για να τροποποιήσετε.
3. Στο blade το λογαριασμό, κάντε κλικ στην επιλογή **αναπαραγωγή καθολικά δεδομένα** από το μενού.
4. Στο blade **καθολικά αναπαραγωγή δεδομένων** , επιλέξτε τις περιοχές για να προσθέσετε ή να καταργήσετε και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**. Υπάρχει μια τιμή κόστους για προσθήκη περιοχές, ανατρέξτε στο θέμα η [Τιμολόγηση σελίδα](https://azure.microsoft.com/pricing/details/documentdb/) ή στο άρθρο [δεδομένα κατανομή καθολικά με DocumentDB](documentdb-distribute-data-globally.md) για περισσότερες πληροφορίες.

    ![Κάντε κλικ στην επιλογή τις περιοχές στο χάρτη για να προσθέσετε ή να καταργήσετε τους][1]

### <a name="selecting-global-database-regions"></a>Επιλογή περιοχών καθολικού βάσης δεδομένων

Όταν ρυθμίζετε τις παραμέτρους δύο ή περισσότερες περιοχές, συνιστάται να ότι έχουν επιλεγεί περιοχές με βάση τα ζεύγη περιοχής που περιγράφονται σε το [επιχειρήσεις συνέχειας και καταστροφή ανάκτησης (BCDR): περιοχές σύζευξη του Azure]  [ bcdr] το άρθρο.

Συγκεκριμένα, όταν ρυθμίζετε τις παραμέτρους σε πολλές περιοχές, βεβαιωθείτε ότι για να επιλέξετε τον ίδιο αριθμό των περιοχών (+/-1 για μονές/ζυγές) από κάθε μία από τις στήλες ζεύγη περιοχής. Για παράδειγμα, εάν θέλετε να αναπτύξετε σε τέσσερις περιοχές των η.π.α., επιλέξτε δύο περιοχές η.π.α. από την αριστερή στήλη και δύο από τα δεξιά. Επομένως, τα ακόλουθα θα είναι ένα κατάλληλο σύνολο: Δυτική η.π.α., Ανατολικής η.π.α., Βόρεια κεντρική ΜΑΣ και Νότια κεντρικής ΜΑΣ.

Αυτές οι οδηγίες είναι σημαντικό να ακολουθήσετε, όταν έχουν ρυθμιστεί οι παράμετροι μόνο δύο περιοχές για σενάρια αποκατάστασης από καταστροφή. Για περισσότερες από δύο περιοχές, ακολουθώντας τις οδηγίες σε αυτό είναι καλή πρακτική, αλλά όχι κρίσιμη με την προϋπόθεση ότι ορισμένες από τις περιοχές επιλεγμένο συμμορφώνονται με αυτό σύζευξη.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>Επόμενα βήματα

Μάθετε πώς μπορείτε να διαχειριστείτε τη συνέπεια του λογαριασμού σας από αναπαραγωγή καθολικά διαβάζοντας [συνέπειας επίπεδα στο DocumentDB](documentdb-consistency-levels.md).

Για πληροφορίες σχετικά με τον τρόπο καθολικού βάσης δεδομένων λειτουργίας στο DocumentDB αναπαραγωγής, ανατρέξτε στο θέμα [δεδομένα κατανομή καθολικά με DocumentDB](documentdb-distribute-data-globally.md). Για πληροφορίες σχετικά με την αναπαραγωγή μέσω προγραμματισμού των δεδομένων σε πολλές περιοχές, ανατρέξτε στο θέμα [αναπτυσσόμενες με λογαριασμούς DocumentDB πολλών περιοχών](documentdb-developing-with-multiple-regions.md).

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
