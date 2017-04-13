<properties 
    pageTitle="Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob Azure χρησιμοποιώντας την Εξερεύνηση χώρου αποθήκευσης Azure | Microsoft Azure" 
    description="Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob Azure χρησιμοποιώντας την Εξερεύνηση χώρου αποθήκευσης Azure" 
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
    ms.date="08/31/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob Azure χρησιμοποιώντας την Εξερεύνηση χώρου αποθήκευσης Azure

Azure Εξερεύνηση χώρου αποθήκευσης είναι ένα εργαλείο δωρεάν από τη Microsoft που σας επιτρέπει να εργάζεστε με δεδομένα του Azure χώρου αποθήκευσης των Windows, macOS και Linux. Αυτό το θέμα περιγράφει πώς μπορείτε να το χρησιμοποιήσετε για την αποστολή και λήψη δεδομένων από το χώρο αποθήκευσης αντικειμένων blob του Azure. Μπορείτε να κάνετε λήψη του εργαλείου από την [Εξερεύνηση χώρου αποθήκευσης του Microsoft Azure](http://storageexplorer.com/).

Οδηγίες για τις τεχνολογίες που χρησιμοποιούνται για τη μετακίνηση δεδομένων σε ή/και από το χώρο αποθήκευσης αντικειμένων Blob του Azure είναι συνδεδεμένες εδώ:
 
[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]   

 
> [AZURE.NOTE] Εάν χρησιμοποιείτε Εικονική που έχει ρυθμιστεί με τις δέσμες ενεργειών που παρέχονται από [δεδομένα Science εικονικού μηχανές στο Azure](machine-learning-data-science-virtual-machines.md), στη συνέχεια, Azure Εξερεύνηση χώρου αποθήκευσης είναι ήδη εγκατεστημένο στον η Εικονική.
 
> [AZURE.NOTE] Για μια πλήρη εισαγωγή με το χώρο αποθήκευσης αντικειμένων blob του Azure, ανατρέξτε στην [Βασικά Blob Azure](../storage/storage-dotnet-how-to-use-blobs.md) και [Η υπηρεσία αντικειμένων Blob του Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).   

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το έγγραφο προϋποθέτει ότι έχετε μια συνδρομή του Azure, ένα λογαριασμό του χώρου αποθήκευσης και το αντίστοιχο κλειδί χώρου αποθήκευσης για τον συγκεκριμένο λογαριασμό. Πριν από την αποστολή/λήψη δεδομένων, πρέπει να γνωρίζετε Azure χώρου αποθήκευσης και το όνομα του λογαριασμού το κλειδί λογαριασμού σας. 

- Για να ρυθμίσετε μια συνδρομή του Azure, ανατρέξτε στο θέμα [δωρεάν δοκιμαστική έκδοση ενός μήνα](https://azure.microsoft.com/pricing/free-trial/).
- Για οδηγίες σχετικά με τη δημιουργία ενός λογαριασμού χώρου αποθήκευσης και γρήγορα λογαριασμό και βασικές πληροφορίες, ανατρέξτε στο θέμα [σχετικά με το Azure λογαριασμοί χώρου αποθήκευσης](../storage/storage-create-storage-account.md). Σημειώστε το πλήκτρο πρόσβασης για το λογαριασμό σας χώρο αποθήκευσης χρειάζεστε αυτό το κλειδί για να συνδεθείτε με το λογαριασμό με το εργαλείο Azure Εξερεύνηση χώρου αποθήκευσης.
- Μπορείτε να κάνετε λήψη του εργαλείου Azure Εξερεύνηση χώρου αποθήκευσης από την [Εξερεύνηση χώρου αποθήκευσης του Microsoft Azure](http://storageexplorer.com/). Αποδεχτείτε τις προεπιλογές κατά την εγκατάσταση.


<a id="explorer"></a>
## <a name="use-azure-storage-explorer"></a>Χρησιμοποιήστε την Εξερεύνηση χώρου αποθήκευσης Azure 

Ακολουθήστε τα παρακάτω βήματα εγγράφου με τον τρόπο αποστολής/λήψης δεδομένων χρησιμοποιώντας την Εξερεύνηση χώρου αποθήκευσης Azure. 

1.  Εκκινήστε το Windows Azure Εξερεύνηση χώρου αποθήκευσης.
2.  Για να εμφανίσετε τον "Οδηγό **συνδεθείτε στο λογαριασμό σας...** ", επιλέξτε **Ρυθμίσεις λογαριασμού Azure** εικονίδιο και, στη συνέχεια, **προσθέστε ένα λογαριασμό** και να εισαγάγετε τα διαπιστευτήριά. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3.  Για να εμφανίσετε τον οδηγό **σύνδεση με το χώρο αποθήκευσης Azure** , επιλέξτε το εικονίδιο **σύνδεση με το χώρο αποθήκευσης Azure** . ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Εισαγάγετε τον αριθμό-κλειδί πρόσβασης από το λογαριασμό σας Azure χώρου αποθήκευσης στη **σύνδεση με το χώρο αποθήκευσης Azure** οδηγό και, στη συνέχεια, **Επόμενο**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Πληκτρολογήστε το όνομα λογαριασμού χώρου αποθήκευσης στο πλαίσιο **όνομα λογαριασμού** και, στη συνέχεια, επιλέξτε **Επόμενο**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Τώρα θα πρέπει να αναφέρεται το λογαριασμό χώρου αποθήκευσης που προσθέσατε. Για να δημιουργήσετε ένα κοντέινερ αντικειμένων blob σε ένα λογαριασμό του χώρου αποθήκευσης, κάντε δεξί κλικ στον κόμβο **Κοντέινερ αντικειμένων Blob** του συγκεκριμένου λογαριασμού, επιλέξτε **Δημιουργία Blob κοντέινερ**και πληκτρολογήστε ένα όνομα.
7. Για να αποστείλετε δεδομένα σε ένα κοντέινερ, επιλέξτε το κοντέινερ προορισμού και κάντε κλικ στο κουμπί **Αποστολή** .![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Κάντε κλικ στην εντολή το **...** στα δεξιά του πλαισίου **αρχείων** , επιλέξτε ένα ή περισσότερα αρχεία για την αποστολή από το σύστημα αρχείων και κάντε κλικ στην επιλογή " **Αποστολή** " για να ξεκινήσει η αποστολή των αρχείων.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
7. Για να κάνετε λήψη δεδομένων, επιλέγοντας το αντικείμενο blob στο αντίστοιχο κοντέινερ για να κάνετε λήψη και κάντε κλικ στην επιλογή **λήψη**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)


