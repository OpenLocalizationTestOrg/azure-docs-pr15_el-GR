<properties
   pageTitle="Αντιγράψτε δεδομένα από αντικείμενα blob του Azure χώρου αποθήκευσης στο χώρο αποθήκευσης δεδομένων λίμνης | Microsoft Azure"
   description="Χρησιμοποιήστε το εργαλείο AdlCopy για να αντιγράψετε δεδομένα από αντικείμενα blob του Azure χώρου αποθήκευσης στο χώρο αποθήκευσης δεδομένων λίμνης"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Αντιγράψτε δεδομένα από αντικείμενα blob του Azure χώρου αποθήκευσης στο χώρο αποθήκευσης δεδομένων λίμνης

> [AZURE.SELECTOR]
- [Χρήση DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Χρήση AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)

Χώρος αποθήκευσης λίμνης δεδομένων Azure παρέχει ένα εργαλείο γραμμής εντολών, [AdlCopy](http://aka.ms/downloadadlcopy), για να αντιγράψετε δεδομένα από τις παρακάτω πηγές:

* Από το Azure χώρο αποθήκευσης αντικειμένων blob στο χώρο αποθήκευσης λίμνης δεδομένων. Δεν μπορείτε να χρησιμοποιήσετε AdlCopy για να αντιγράψετε δεδομένα από το χώρο αποθήκευσης δεδομένων λίμνης Azure χώρο αποθήκευσης αντικειμένων blob.

* Μεταξύ δύο χώρου αποθήκευσης λίμνης δεδομένων Azure λογαριασμών. 

Επίσης, μπορείτε να χρησιμοποιήσετε το εργαλείο AdlCopy σε δύο διαφορετικές καταστάσεις λειτουργίας:

* **Μεμονωμένη**, όπου το εργαλείο χρησιμοποιεί χώρο αποθήκευσης δεδομένων λίμνης πόρους για να εκτελέσετε την εργασία.

* **Χρησιμοποιώντας ένα λογαριασμό ανάλυση λίμνης δεδομένων**, όπου οι μονάδες που έχουν εκχωρηθεί στο λογαριασμό σας ανάλυση λίμνης δεδομένων που χρησιμοποιούνται για να εκτελέσετε τη λειτουργία αντιγραφής. Ενδέχεται να θέλετε να χρησιμοποιήσετε αυτήν την επιλογή όταν θέλετε να εκτελέσετε τις εργασίες αντίγραφο σε προβλέψιμα τρόπο.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

- Κοντέινερ **Αντικειμένων blob του Azure χώρου αποθήκευσης** με ορισμένα δεδομένα.

- **Λογαριασμός azure δεδομένων λίμνης ανάλυσης (προαιρετικό)** - ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure δεδομένων λίμνης ανάλυσης](../data-lake-analytics/data-lake-analytics-get-started-portal.md) για οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.

- **Εργαλείο AdlCopy**. Εγκαταστήστε το εργαλείο AdlCopy από [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>Σύνταξη του εργαλείου AdlCopy

Χρησιμοποιήστε την ακόλουθη σύνταξη για να εργαστείτε με το εργαλείο AdlCopy

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

Παρακάτω περιγράφονται οι παράμετροι στη σύνταξη:

| Επιλογή    | Περιγραφή                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Αρχείο προέλευσης    | Καθορίζει τη θέση των δεδομένων προέλευσης στο το Azure χώρο αποθήκευσης αντικειμένων blob. Η προέλευση μπορεί να είναι ένα κοντέινερ αντικειμένων blob, ένα blob ή άλλο λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων.                                                                                                                                                                                                                                                                                                 |
| Προορισμού      | Καθορίζει τον προορισμό χώρου αποθήκευσης λίμνης δεδομένων για να αντιγράψετε.                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Καθορίζει το πλήκτρο πρόσβασης χώρου αποθήκευσης για το αρχείο προέλευσης Azure χώρο αποθήκευσης αντικειμένων blob. Αυτό είναι απαραίτητο, μόνο εάν το αρχείο προέλευσης είναι ένα κοντέινερ αντικειμένων blob ή ένα blob.                                                                                                                                                                                                                                                                                                                                                 |
| Λογαριασμός   | **Προαιρετικό**. Χρησιμοποιήστε αυτήν την επιλογή εάν θέλετε να χρησιμοποιήσετε λογαριασμός Azure δεδομένων λίμνης ανάλυση για να εκτελέσετε την αντιγραφή έργου. Εάν χρησιμοποιήσετε την επιλογή /Account στη σύνταξη, αλλά δεν καθορίσετε έναν λογαριασμό ανάλυσης δεδομένων λίμνης, AdlCopy χρησιμοποιεί έναν προεπιλεγμένο λογαριασμό για να εκτελέσετε την εργασία. Επίσης, εάν χρησιμοποιήσετε αυτήν την επιλογή, πρέπει να προσθέσετε (Azure χώρο αποθήκευσης αντικειμένων Blob) προέλευσης και προορισμού (Store λίμνης Azure δεδομένων) ως αρχεία προέλευσης δεδομένων για το λογαριασμό σας ανάλυση λίμνης δεδομένων.  |
| Μονάδες     |  Καθορίζει τον αριθμό των μονάδων ανάλυσης λίμνης δεδομένων που θα χρησιμοποιηθεί για την αντιγραφή έργου. Αυτή η επιλογή είναι υποχρεωτική, εάν χρησιμοποιείτε την επιλογή **/Account** για να καθορίσετε το λογαριασμό ανάλυση λίμνης δεδομένων.
| Μοτίβο   | Καθορίζει ένα μοτίβο regex που υποδεικνύει ποια αντικείμενα BLOB ή αρχεία για να αντιγράψετε. AdlCopy χρησιμοποιεί Ταίριασμα πεζών-κεφαλαίων. Είναι το προεπιλεγμένο μοτίβο χρησιμοποιούνται όταν έχει καθοριστεί χωρίς μοτίβο για να αντιγράψετε όλα τα στοιχεία. Καθορισμός μοτίβα πολλών αρχείων δεν υποστηρίζεται.                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>Χρήση AdlCopy (ως μεμονωμένου) για να αντιγράψετε δεδομένα από μια Azure χώρο αποθήκευσης αντικειμένων blob

1. Ανοίξτε μια γραμμή εντολών και μεταβείτε στον κατάλογο όπου AdlCopy είναι εγκατεστημένο, συνήθως `%HOMEPATH%\Documents\adlcopy`.

2. Εκτελέστε την παρακάτω εντολή για να αντιγράψετε ένα συγκεκριμένο blob από το κοντέινερ προέλευσης σε ένα χώρο αποθήκευσης λίμνης δεδομένων:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Για παράδειγμα:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] Η παραπάνω σύνταξη Καθορίζει το αρχείο προς αντιγραφή σε φάκελο στο λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων. Εργαλείο AdlCopy δημιουργεί ένα φάκελο, εάν δεν υπάρχει το καθορισμένο όνομα φακέλου.

    Θα σας ζητηθεί να εισαγάγετε τα διαπιστευτήρια για τη συνδρομή Azure στην οποία έχετε το λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης. Θα δείτε το αποτέλεσμα παρόμοιο με το εξής:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Μπορείτε επίσης να αντιγράψετε όλα τα αντικείμενα BLOB από ένα κοντέινερ με το λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων χρησιμοποιώντας την ακόλουθη εντολή:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Για παράδειγμα:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>Χρήση AdlCopy (ως αυτόνομης υπηρεσίας) για να αντιγράψετε δεδομένα από άλλο λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης

Μπορείτε επίσης να χρησιμοποιήσετε AdlCopy για να αντιγράψετε δεδομένα μεταξύ των δύο λογαριασμούς χώρου αποθήκευσης λίμνης δεδομένων.

1. Ανοίξτε μια γραμμή εντολών και μεταβείτε στον κατάλογο όπου AdlCopy είναι εγκατεστημένο, συνήθως `%HOMEPATH%\Documents\adlcopy`.

2. Εκτελέστε την παρακάτω εντολή για να αντιγράψετε ένα συγκεκριμένο αρχείο από ένα λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων σε μια άλλη.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Για παράδειγμα:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] Η παραπάνω σύνταξη Καθορίζει το αρχείο προς αντιγραφή σε φάκελο στον προορισμό λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων. Εργαλείο AdlCopy δημιουργεί ένα φάκελο, εάν δεν υπάρχει το καθορισμένο όνομα φακέλου.

    Θα σας ζητηθεί να εισαγάγετε τα διαπιστευτήρια για τη συνδρομή Azure στην οποία έχετε το λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης. Θα δείτε το αποτέλεσμα παρόμοιο με το εξής:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Την παρακάτω εντολή αντιγράφει όλα τα αρχεία από ένα συγκεκριμένο φάκελο στην προέλευση λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων σε ένα φάκελο σε ο προορισμός αποθήκευσης λίμνης δεδομένων λογαριασμού.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Χρήση AdlCopy (με το λογαριασμό ανάλυσης δεδομένων λίμνης) για να αντιγράψετε δεδομένα

Μπορείτε επίσης να χρησιμοποιήσετε το λογαριασμό σας ανάλυση λίμνης δεδομένων για να εκτελέσετε την εργασία AdlCopy για να αντιγράψετε δεδομένα από το Azure χώρο αποθήκευσης αντικειμένων blob στο χώρο αποθήκευσης λίμνης δεδομένων. Συνήθως θα χρησιμοποιήσετε αυτήν την επιλογή όταν τα δεδομένα θα μετακινηθούν βρίσκεται στην περιοχή gigabytes και terabytes και θέλετε μετάδοσης καλύτερη και προβλέψιμα επιδόσεων.

Για να χρησιμοποιήσετε το λογαριασμό σας ανάλυση λίμνης δεδομένων με AdlCopy για να αντιγράψετε από ένα αντικειμένων Blob του Azure χώρου αποθήκευσης, το αρχείο προέλευσης (Azure χώρο αποθήκευσης αντικειμένων Blob) πρέπει να προστεθεί ως προέλευση δεδομένων για το λογαριασμό σας ανάλυση λίμνης δεδομένων. Για οδηγίες σχετικά με την προσθήκη πρόσθετων προελεύσεων δεδομένων στο λογαριασμό σας ανάλυση λίμνης δεδομένων, ανατρέξτε στο θέμα [Διαχείριση ανάλυσης λίμνης δεδομένων λογαριασμού προελεύσεις δεδομένων](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

>[AZURE.NOTE] Εάν θέλετε να αντιγράψετε από έναν λογαριασμό του χώρου αποθήκευσης λίμνης Azure δεδομένων ως αρχείο προέλευσης χρησιμοποιώντας ένα λογαριασμό ανάλυση λίμνης δεδομένων, δεν χρειάζεται να συσχετίσετε το λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων με το λογαριασμό ανάλυση λίμνης δεδομένων. Η απαίτηση για να συσχετίσετε το χώρο αποθήκευσης προέλευσης με το λογαριασμό δεδομένων λίμνης ανάλυσης είναι μόνο όταν το αρχείο προέλευσης είναι ένας λογαριασμός Azure χώρου αποθήκευσης.

Εκτελέστε την παρακάτω εντολή για να αντιγράψετε από μια Azure χώρο αποθήκευσης αντικειμένων blob σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης χρησιμοποιώντας λογαριασμό ανάλυση λίμνης δεδομένων:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Για παράδειγμα:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


Ομοίως, εκτελέστε την ακόλουθη εντολή για να αντιγράψετε από μια Azure χώρο αποθήκευσης αντικειμένων blob σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης χρησιμοποιώντας λογαριασμό ανάλυση λίμνης δεδομένων:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Χρήση AdlCopy για την αντιγραφή δεδομένων με τη χρήση μοτίβου αντιστοίχισης

Σε αυτήν την ενότητα, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε AdlCopy για να αντιγράψετε δεδομένα από μια προέλευση (στο παράδειγμά μας παρακάτω χρησιμοποιούμε Azure χώρο αποθήκευσης Blob) σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης προορισμού χρησιμοποιώντας μοτίβου αντιστοίχισης. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε τα παρακάτω βήματα για να αντιγράψετε όλα τα αρχεία με επέκταση .csv από το αντικείμενο blob προέλευσης στον προορισμό.

1. Ανοίξτε μια γραμμή εντολών και μεταβείτε στον κατάλογο όπου AdlCopy είναι εγκατεστημένο, συνήθως `%HOMEPATH%\Documents\adlcopy`.

2. Εκτελέστε την παρακάτω εντολή για να αντιγράψετε όλα τα αρχεία με επέκταση *.csv από ένα συγκεκριμένο blob από το κοντέινερ προέλευσης σε ένα χώρο αποθήκευσης λίμνης δεδομένων:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Για παράδειγμα:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>Χρέωση

* Εάν χρησιμοποιείτε το εργαλείο AdlCopy ως αυτόνομης υπηρεσίας θα χρεώνεστε για εξόδου κόστους για τη μετακίνηση δεδομένων, εάν το αρχείο προέλευσης λογαριασμός Azure χώρου αποθήκευσης δεν είναι στην ίδια περιοχή ως ο χώρος αποθήκευσης δεδομένων λίμνης.

* Εάν χρησιμοποιείτε το εργαλείο AdlCopy με το λογαριασμό σας ανάλυση λίμνης δεδομένων, θα εφαρμοστούν τυπικές [χρεώσεις χρεώσεων ανάλυση λίμνης δεδομένων](https://azure.microsoft.com/pricing/details/data-lake-analytics/) .

## <a name="considerations-for-using-adlcopy"></a>Ζητήματα σχετικά με τη χρήση AdlCopy

* AdlCopy (για την έκδοση 1.0.5), υποστηρίζει αντιγραφής δεδομένα από προελεύσεις που αποτελούν συνολικά έχει περισσότερες από χιλιάδες αρχείων και φακέλων. Ωστόσο, εάν αντιμετωπίσετε προβλήματα αντιγραφή ένα μεγάλο σύνολο δεδομένων, μπορείτε να κατανομή των αρχείων/φακέλων σε διαφορετικούς υποφακέλους και να χρησιμοποιήσετε τη διαδρομή σε αυτούς τους φακέλους δευτερευόντων ως προέλευση αντί για αυτό.

## <a name="next-steps"></a>Επόμενα βήματα

- [Διασφάλιση των δεδομένων στο χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-secure-data.md)
- [Χρήση ανάλυσης λίμνης Azure δεδομένων με το χώρο αποθήκευσης δεδομένων λίμνης](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Χρήση Azure HDInsight με το χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-hdinsight-hadoop-use-portal.md)
