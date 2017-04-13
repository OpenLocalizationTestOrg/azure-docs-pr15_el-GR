<properties
    pageTitle="Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob του Azure | Microsoft Azure"
    description="Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob του Azure"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev;sachouks" />

# <a name="move-data-to-and-from-azure-blob-storage"></a>Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob του Azure

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Οδηγίες για τις τεχνολογίες που χρησιμοποιούνται για τη μετακίνηση δεδομένων σε ή/και από το χώρο αποθήκευσης αντικειμένων Blob του Azure είναι συνδεδεμένες εδώ:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Ποια μέθοδο είναι καλύτερη για εσάς εξαρτάται από το σενάριό σας. Το άρθρο [σενάρια για προηγμένη ανάλυση στο Azure μηχανικής εκμάθησης](machine-learning-data-science-plan-sample-scenarios.md) σάς βοηθά να προσδιορίσετε τους πόρους που χρειάζεστε για μια ποικιλία ροές εργασίας φυσικής δεδομένων που χρησιμοποιούνται στη διαδικασία προηγμένη ανάλυση.

> [AZURE.NOTE] Για μια πλήρη εισαγωγή με το χώρο αποθήκευσης αντικειμένων blob του Azure, ανατρέξτε για [Βασικά στοιχεία αντικειμένων Blob του Azure](../storage/storage-dotnet-how-to-use-blobs.md) και την [Υπηρεσία αντικειμένων Blob του Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).

Ως εναλλακτική λύση, μπορείτε να χρησιμοποιήσετε [Azure εργοστασίου δεδομένων](https://azure.microsoft.com/services/data-factory/) για να: 

- Δημιουργία και προγραμματισμός ενός δικτύου αγωγών που κάνει λήψη δεδομένων από το χώρο αποθήκευσης αντικειμένων blob του Azure, 
- μεταβίβαση το σε μια υπηρεσία web Azure μηχανικής εκμάθησης δημοσιευμένη, 
- λάβετε τα αποτελέσματα ανάλυσης πρόβλεψης, και 
- Στείλτε τα αποτελέσματα με το χώρο αποθήκευσης. 

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία πρόβλεψης αγωγούς χρησιμοποιώντας εργοστασίου δεδομένων Azure και Azure μηχανικής εκμάθησης](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το έγγραφο προϋποθέτει ότι έχετε μια συνδρομή του Azure, ένα λογαριασμό του χώρου αποθήκευσης και το αντίστοιχο κλειδί χώρου αποθήκευσης για τον συγκεκριμένο λογαριασμό. Πριν από την αποστολή/λήψη δεδομένων, πρέπει να γνωρίζετε Azure χώρου αποθήκευσης και το όνομα του λογαριασμού το κλειδί λογαριασμού σας.

- Για να ρυθμίσετε μια συνδρομή του Azure, ανατρέξτε στο θέμα [δωρεάν δοκιμαστική έκδοση ενός μήνα](https://azure.microsoft.com/pricing/free-trial/).
- Για οδηγίες σχετικά με τη δημιουργία ενός λογαριασμού χώρου αποθήκευσης και γρήγορα λογαριασμό και βασικές πληροφορίες, ανατρέξτε στο θέμα [σχετικά με το Azure λογαριασμοί χώρου αποθήκευσης](../storage/storage-create-storage-account.md).
