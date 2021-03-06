<properties
    pageTitle="Δημιουργία του πρώτου εργοστασίου δεδομένων (Azure πύλη) | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε μια διοχέτευση εργοστασίου δεδομένων Azure δείγμα χρησιμοποιώντας το πρόγραμμα επεξεργασίας εργοστασίου δεδομένων στην πύλη του Azure."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="09/14/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Πρόγραμμα εκμάθησης: Δημιουργία του πρώτου εργοστασίου Azure δεδομένων με Azure πύλη
> [AZURE.SELECTOR]
- [Επισκόπηση και τις προϋποθέσεις](data-factory-build-your-first-pipeline.md)
- [Πύλη του Azure](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Το πρότυπο διαχείρισης πόρων](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Σε αυτό το άρθρο, μπορείτε να μάθετε πώς μπορείτε να χρησιμοποιήσετε την [πύλη του Azure](https://portal.azure.com/) για να δημιουργήσετε το πρώτο εργοστασίου Azure δεδομένων. 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία        
1. Διαβάστε το άρθρο [Επισκόπηση πρόγραμμα εκμάθησης](data-factory-build-your-first-pipeline.md) και ολοκληρώστε τα βήματα **προϋπόθεση** .
2. Σε αυτό το άρθρο δεν παρέχει μια επισκόπηση της υπηρεσίας Azure εργοστασίου δεδομένων. Συνιστάται να ακολουθήσετε άρθρο [Εισαγωγή στις εργοστασιακές Azure δεδομένων](data-factory-introduction.md) για μια λεπτομερή επισκόπηση της υπηρεσίας.  

## <a name="create-data-factory"></a>Δημιουργία εργοστασίου δεδομένων
Μια προέλευση δεδομένων μπορεί να έχει ένα ή περισσότερα αγωγούς. Μια διαδικασία μπορεί να έχει μία ή περισσότερες δραστηριότητες σε αυτό. Για παράδειγμα, μια δραστηριότητα Αντιγραφή για να αντιγράψετε δεδομένα από ένα αρχείο προέλευσης σε ένα χώρο αποθήκευσης δεδομένων προορισμού και δραστηριότητα HDInsight Hive για να εκτελέσετε Hive δέσμη ενεργειών για τη μετατροπή των δεδομένων εισαγωγής με προϊόν εξόδου δεδομένων. Ας ξεκινήσουμε με τη δημιουργία την προέλευση δεδομένων σε αυτό το βήμα. 

1.  Συνδεθείτε [πύλη του Azure](https://portal.azure.com/).
2.  Κάντε κλικ στην επιλογή **ΔΗΜΙΟΥΡΓΊΑ** από το αριστερό μενού, κάντε κλικ στην επιλογή **δεδομένα + ανάλυση**και επιλέξτε **Εργοστασίου δεδομένων**.
        
    ![Δημιουργία blade](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)

2.  Στο blade τη **νέα προέλευση δεδομένων** , πληκτρολογήστε **GetStartedDF** για το όνομα.

    ![Νέα blade εργοστασίου δεδομένων](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

    > [AZURE.IMPORTANT] 
    > Το όνομα του εργοστασίου Azure δεδομένων πρέπει να είναι **μοναδικό καθολικό**. Εάν λαμβάνετε το μήνυμα σφάλματος: **όνομα εργοστασίου δεδομένων "GetStartedDF" δεν είναι διαθέσιμη**. Αλλαγή του ονόματος του εργοστασίου δεδομένων (για παράδειγμα, yournameGetStartedDF) και δοκιμάστε να δημιουργήσετε ξανά. Ανατρέξτε στο θέμα [Factory δεδομένων - κανόνες ονοματοθεσίας](data-factory-naming-rules.md) για τους κανόνες ονοματοθεσίας για αντικείμενα εργοστασίου δεδομένων.
    > 
    > Το όνομα του εργοστασίου δεδομένων μπορεί να έχει εγγραφεί ως όνομα **DNS** στο μέλλον και συνεπώς γίνονται δημόσια ορατά.

3.  Επιλέξτε τη **συνδρομή Azure** όπου θέλετε το εργοστασίου δεδομένων που θα δημιουργηθεί. 
4.  Επιλέξτε υπάρχουσα **ομάδα πόρων** ή δημιουργήστε μια ομάδα πόρων. Για το πρόγραμμα εκμάθησης, δημιουργήστε μια ομάδα πόρων με το όνομα: **ADFGetStartedRG**. 
5.  Κάντε κλικ στην εντολή **Δημιουργία** blade τη **νέα προέλευση δεδομένων** .

    > [AZURE.IMPORTANT] Για να δημιουργήσετε παρουσίες εργοστασίου δεδομένων, πρέπει να είναι ένα μέλος του ρόλου [Συμβολής εργοστασίου δεδομένων](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) σε επίπεδο ομάδας συνδρομή/πόρων. 
6.  Δείτε την προέλευση δεδομένων που δημιουργήθηκε σε το **Startboard** της πύλης Azure ως εξής:   

    ![Δημιουργία κατάσταση εργοστασίου δεδομένων](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
7. Συγχαρητήρια! Έχετε δημιουργήσει με επιτυχία το πρώτο εργοστασίου δεδομένων. Μετά την προέλευση δεδομένων έχει δημιουργηθεί με επιτυχία, μπορείτε να δείτε σελίδας εργοστασίου δεδομένων, η οποία εμφανίζονται τα περιεχόμενα του εργοστασίου δεδομένων.   

    ![Blade εργοστασίου δεδομένων](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Πριν να δημιουργήσετε μια διαδικασία σε την προέλευση δεδομένων, πρέπει πρώτα να δημιουργήσετε ορισμένα δεδομένα εργοστασίου οντοτήτων. Πρώτα, δημιουργήστε συνδεδεμένες υπηρεσίες για να συνδέσετε αποθηκεύει δεδομένα/τύπος υπολογίζει το χώρο αποθήκευσης δεδομένων, ορισμός εισόδου και εξόδου σύνολα δεδομένων για την αναπαράσταση των δεδομένων εισόδου/εξόδου σε συνδεδεμένα δεδομένα αποθηκεύονται και, στη συνέχεια, να δημιουργήσετε τη διαδικασία με μια δραστηριότητα που χρησιμοποιεί αυτά τα σύνολα δεδομένων. 

## <a name="create-linked-services"></a>Δημιουργία συνδεδεμένων υπηρεσιών
Σε αυτό το βήμα, μπορείτε να συνδέσετε το λογαριασμό σας χώρο αποθήκευσης Azure και ένα σύμπλεγμα Azure HDInsight στη ζήτηση για την προέλευση δεδομένων. Ο λογαριασμός Azure αποθήκευσης διατηρεί τα δεδομένα εισόδου και εξόδου για τη διαδικασία σε αυτό το δείγμα. Για να εκτελέσετε Hive δέσμη ενεργειών που καθορίζονται στη δραστηριότητα από τη διαδικασία σε αυτό το δείγμα χρησιμοποιείται η υπηρεσία HDInsight συνδεδεμένες. Προσδιορίστε ποιες [χώρου αποθήκευσης δεδομένων](data-factory-data-movement-activities.md)/[υπολογίζει τις υπηρεσίες](data-factory-compute-linked-services.md) που χρησιμοποιούνται στη δική σας περίπτωση και να συνδέσετε αυτές τις υπηρεσίες η προέλευση δεδομένων δημιουργώντας συνδεδεμένες υπηρεσίες.  

### <a name="create-azure-storage-linked-service"></a>Δημιουργία χώρου αποθήκευσης Azure συνδεδεμένες υπηρεσίας
Σε αυτό το βήμα, μπορείτε να συνδέσετε το λογαριασμό σας χώρο αποθήκευσης Azure για την προέλευση δεδομένων. Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να χρησιμοποιήσετε τον ίδιο λογαριασμό Azure χώρου αποθήκευσης για την αποθήκευση δεδομένων εισόδου/εξόδου και το αρχείο δέσμης ενεργειών HQL. 

1.  Κάντε κλικ στην επιλογή **συντάκτη και ανάπτυξη** στην το blade **ΕΡΓΟΣΤΑΣΊΟΥ ΔΕΔΟΜΈΝΩΝ** για **GetStartedDF**. Θα πρέπει να βλέπετε το πρόγραμμα επεξεργασίας εργοστασίου δεδομένων. 
     
    ![Σύνταξη και ανάπτυξη των πλακιδίων](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2.  Κάντε κλικ στην επιλογή **Αποθήκευση νέα δεδομένα** και επιλέξτε **Azure χώρου αποθήκευσης**.

    ![Νέο μενού store - αποθήκευσης Azure - δεδομένων](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)

3.  Θα πρέπει να δείτε τη δέσμη ενεργειών JSON για τη δημιουργία μια υπηρεσία αποθήκευσης Azure που συνδέονται στο πρόγραμμα επεξεργασίας. 
    
    ![Υπηρεσία αποθήκευσης συνδεδεμένες του Azure](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
     
4. Αντικαταστήστε **το όνομα του λογαριασμού** με το όνομα του Azure χώρο αποθήκευσης στο λογαριασμό σας και το **κλειδί λογαριασμού** με το πλήκτρο πρόσβασης του λογαριασμού Azure χώρου αποθήκευσης. Για να μάθετε πώς μπορείτε να λάβετε τον αριθμό-κλειδί access χώρου αποθήκευσης, ανατρέξτε στο θέμα [Προβολή "," Αντιγραφή "και" πλήκτρα πρόσβασης regenerate χώρου αποθήκευσης](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
5. Στη γραμμή εντολών για την ανάπτυξη της συνδεδεμένων υπηρεσίας, κάντε κλικ στην επιλογή **Ανάπτυξη** .

    ![Κουμπί ανάπτυξης](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Μετά την υπηρεσία συνδεδεμένων έχει αναπτυχθεί με επιτυχία, θα πρέπει να εξαφανιστεί το παράθυρο " **πρόχειρη-1** " και δείτε **AzureStorageLinkedService** στην προβολή δέντρου στα αριστερά. 
    ![Συνδεδεμένο υπηρεσία αποθήκευσης στο μενού](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)   

 
### <a name="create-azure-hdinsight-linked-service"></a>Δημιουργία Azure HDInsight συνδεδεμένες υπηρεσίας
Σε αυτό το βήμα, μπορείτε να συνδέσετε ένα σύμπλεγμα HDInsight στην απαιτήσεων για την προέλευση δεδομένων. Το HDInsight σύμπλεγμα αυτόματα δημιουργείται κατά το χρόνο εκτέλεσης και διαγραφεί μετά την ολοκλήρωση επεξεργασίας και αδρανής για το καθορισμένο χρονικό διάστημα. 

1. Στο πρόγραμμα **Επεξεργασίας εργοστασίου δεδομένων**, κάντε κλικ στην επιλογή **... Περισσότερες**, κάντε κλικ στην επιλογή **Δημιουργία τον υπολογισμό**και επιλέξτε **σύμπλεγμα On demand HDInsight**.

    ![Νέα υπολογισμού](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Αντιγράψτε και επικολλήστε το παρακάτω τμήμα κώδικα στο παράθυρο **σχέδιο-1** . Το τμήμα κώδικα JSON περιγράφει τις ιδιότητες που χρησιμοποιούνται για να δημιουργήσετε το HDInsight σύμπλεγμα στην-ζήτηση. 

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService"
            }
          }
        }
    
    Ο παρακάτω πίνακας παρέχει περιγραφές για τις ιδιότητες JSON χρησιμοποιείται σε το τμήμα κώδικα:
    
  	| Ιδιότητα | Περιγραφή |
  	| :------- | :---------- |
  	| Έκδοση | Καθορίζει ότι η δημιουργία έκδοσης της το HDInsight να είναι 3,2. | 
  	| ClusterSize | Καθορίζει το μέγεθος του συμπλέγματος HDInsight. | 
  	| Το TimeToLive | Καθορίζει ότι το χρόνο αδράνειας για το σύμπλεγμα HDInsight, πριν να διαγραφεί. |
  	| linkedServiceName | Καθορίζει το λογαριασμό χώρου αποθήκευσης που χρησιμοποιείται για την αποθήκευση των αρχείων καταγραφής που δημιουργούνται από το HDInsight. |

    Λάβετε υπόψη τα εξής σημεία: 
    
    - Η προέλευση δεδομένων δημιουργεί ένα σύμπλεγμα HDInsight **βασίζεται σε Windows** με το JSON. Επίσης, μπορείτε να έχετε το Δημιουργήστε ένα σύμπλεγμα HDInsight **βάσει Linux** . Για λεπτομέρειες, ανατρέξτε στο θέμα [On demand HDInsight συνδεδεμένων υπηρεσιών](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
    - Μπορείτε να χρησιμοποιήσετε **τη δική σας σύμπλεγμα HDInsight** αντί να χρησιμοποιήσετε ένα σύμπλεγμα HDInsight στη ζήτηση. Για λεπτομέρειες, ανατρέξτε στο θέμα [Υπηρεσία συνδεδεμένων HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
    - Το HDInsight σύμπλεγμα δημιουργεί ένα **προεπιλεγμένο κοντέινερ** το χώρο αποθήκευσης αντικειμένων blob που καθορίσατε στο το JSON (**linkedServiceName**). HDInsight δεν διαγράφει αυτό το κοντέινερ όταν διαγράφεται το σύμπλεγμα. Αυτή η συμπεριφορά οφείλεται στη σχεδίαση. Με την υπηρεσία HDInsight συνδεδεμένο σε ζήτηση, δημιουργείται ένα σύμπλεγμα HDInsight κάθε φορά που ένα κομμάτι υποβάλλεται σε επεξεργασία, εκτός εάν υπάρχει ένα υπάρχον σύμπλεγμα ζωντανή (**το timeToLive**). Το σύμπλεγμα διαγράφονται αυτόματα όταν ολοκληρωθεί η επεξεργασία.
    
        Κατά την επεξεργασία περισσότερες φέτες, μπορείτε να δείτε πολλά κοντέινερ στο χώρο αποθήκευσης αντικειμένων blob του Azure του. Εάν δεν χρειάζεστε τους για την αντιμετώπιση προβλημάτων με τις εργασίες, μπορείτε να τα διαγράψετε για να μειώσετε το κόστος χώρου αποθήκευσης. Τα ονόματα από αυτά τα κοντέινερ ακολουθήστε ένα μοτίβο: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Χρήση εργαλείων όπως [Εξερεύνηση χώρου αποθήκευσης](http://storageexplorer.com/) για τη διαγραφή κοντέινερ στο χώρο αποθήκευσης αντικειμένων blob του Azure του.

    Για λεπτομέρειες, ανατρέξτε στο θέμα [On demand HDInsight συνδεδεμένων υπηρεσιών](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) .
3. Στη γραμμή εντολών για την ανάπτυξη της συνδεδεμένων υπηρεσίας, κάντε κλικ στην επιλογή **Ανάπτυξη** . 

    ![Ανάπτυξη σε ζήτηση HDInsight συνδεδεμένες υπηρεσίας](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)

4. Επιβεβαιώστε ότι βλέπετε **AzureStorageLinkedService** και **HDInsightOnDemandLinkedService** στο δέντρο προβολή στην αριστερή πλευρά.

    ![Προβολή δέντρου με τις συνδεδεμένες υπηρεσίες](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Δημιουργία συνόλων δεδομένων
Σε αυτό το βήμα, δημιουργείτε σύνολα δεδομένων για να αντιπροσωπεύει τα δεδομένα εισόδου και εξόδου δεδομένων για την επεξεργασία της ομάδας. Αυτά τα σύνολα δεδομένων που αναφέρονται σε **AzureStorageLinkedService** που δημιουργήσατε νωρίτερα σε αυτό το πρόγραμμα εκμάθησης. Τα σημεία συνδεδεμένων υπηρεσιών σε λογαριασμό αποθήκευσης Azure και συνόλων δεδομένων καθορίσετε κοντέινερ, φάκελο, όνομα αρχείου στο χώρο αποθήκευσης της όπου διατηρείται εισόδου και εξόδου δεδομένων.   

### <a name="create-input-dataset"></a>Δημιουργία συνόλου δεδομένων εισαγωγής

1. Στο πρόγραμμα **Επεξεργασίας εργοστασίου δεδομένων**, κάντε κλικ στην επιλογή **... Περισσότερες** στη γραμμή εντολών, κάντε κλικ στην επιλογή **νέο σύνολο δεδομένων**και επιλέξτε **χώρο αποθήκευσης αντικειμένων Blob του Azure**.

    ![Νέο σύνολο δεδομένων](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Αντιγράψτε και επικολλήστε το παρακάτω τμήμα κώδικα στο παράθυρο σχέδιο-1. Με το τμήμα κώδικα JSON, θέλετε να δημιουργήσετε ένα σύνολο δεδομένων που ονομάζεται **AzureBlobInput** που αντιπροσωπεύει δεδομένα εισόδου για μια δραστηριότητα στη διοχέτευση. Επιπλέον, μπορείτε να καθορίσετε ότι τα δεδομένα εισόδου βρίσκεται στο κοντέινερ αντικειμένων blob που ονομάζεται **adfgetstarted** και στο φάκελο που ονομάζεται **inputdata**.
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
                "typeProperties": {
                    "fileName": "input.log",
                    "folderPath": "adfgetstarted/inputdata",
                    "format": {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    }
                },
                "availability": {
                    "frequency": "Month",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        } 

    Ο παρακάτω πίνακας παρέχει περιγραφές για τις ιδιότητες JSON χρησιμοποιείται σε το τμήμα κώδικα:

  	| Ιδιότητα | Περιγραφή |
  	| :------- | :---------- |
  	| Τύπος | Η ιδιότητα τύπος έχει οριστεί σε AzureBlob επειδή βρίσκονται στο χώρο αποθήκευσης αντικειμένων blob του Azure δεδομένα. |  
  	| linkedServiceName | αναφέρεται το AzureStorageLinkedService που δημιουργήσατε νωρίτερα. |
  	| όνομα αρχείου | Αυτή η ιδιότητα είναι προαιρετικό. Εάν παραλείψετε αυτή την ιδιότητα, όλα τα αρχεία από το folderPath έχουν συλλεχθεί. Σε αυτήν την περίπτωση, γίνεται επεξεργασία μόνο το input.log. |
  	| Τύπος | Τα αρχεία καταγραφής είναι σε μορφή κειμένου, ώστε να χρησιμοποιήσουμε TextFormat. | 
  	| Διαχωριστικό_στήλης | στήλες στα αρχεία καταγραφής είναι διαχωρισμένες με κόμμα () |
  	| συχνότητα/διάστημα | συχνότητα ρυθμιστεί μήνα και διάστημα είναι 1, το οποίο σημαίνει ότι τις φέτες του εισόδου είναι διαθέσιμες κάθε μήνα. | 
  	| εξωτερική | Αυτή η ιδιότητα έχει οριστεί στην τιμή true αν τα δεδομένα εισόδου δεν δημιουργείται από την υπηρεσία εργοστασίου δεδομένων. | 
        
3. Κάντε κλικ στο κουμπί " **Ανάπτυξη** " στη γραμμή εντολών για την ανάπτυξη του συνόλου δεδομένων που έχουν δημιουργηθεί πρόσφατα. Θα πρέπει να βλέπετε του συνόλου δεδομένων στην προβολή δέντρου στα αριστερά. 


### <a name="create-output-dataset"></a>Δημιουργία συνόλου δεδομένων εξόδου
Τώρα, μπορείτε να δημιουργήσετε το σύνολο δεδομένων εξόδου για να αντιπροσωπεύει τα δεδομένα εξόδου που είναι αποθηκευμένα σε το χώρο αποθήκευσης αντικειμένων Blob του Azure. 

1. Στο πρόγραμμα **Επεξεργασίας εργοστασίου δεδομένων**, κάντε κλικ στην επιλογή **... Περισσότερες** στη γραμμή εντολών, κάντε κλικ στην επιλογή **νέο σύνολο δεδομένων**και επιλέξτε **χώρο αποθήκευσης αντικειμένων Blob του Azure**.  
2. Αντιγράψτε και επικολλήστε το παρακάτω τμήμα κώδικα στο παράθυρο σχέδιο-1. Με το τμήμα κώδικα JSON, θέλετε να δημιουργήσετε ένα σύνολο δεδομένων που ονομάζεται **AzureBlobOutput**και καθορίζοντας τη δομή των δεδομένων που παράγεται από τη δέσμη ενεργειών της ομάδας. Επιπλέον, μπορείτε να καθορίσετε ότι τα αποτελέσματα είναι αποθηκευμένα στο κοντέινερ αντικειμένων blob που ονομάζεται **adfgetstarted** και στο φάκελο που ονομάζεται **partitioneddata**. Στην ενότητα **διαθεσιμότητα** Καθορίζει ότι το σύνολο δεδομένων εξόδου παράγεται σε μηνιαία βάση.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adfgetstarted/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

    Ανατρέξτε στην ενότητα **Δημιουργία του συνόλου δεδομένων εισόδου** για περιγραφές από αυτές τις ιδιότητες. Δεν ορίζετε την ιδιότητα εξωτερικών σε ένα σύνολο δεδομένων εξόδου που παράγεται από την υπηρεσία εργοστασίου δεδομένων του συνόλου δεδομένων.
3. Κάντε κλικ στο κουμπί " **Ανάπτυξη** " στη γραμμή εντολών για την ανάπτυξη του συνόλου δεδομένων που έχουν δημιουργηθεί πρόσφατα.
4. Βεβαιωθείτε ότι το σύνολο δεδομένων δημιουργείται με επιτυχία.

    ![Προβολή δέντρου με τις συνδεδεμένες υπηρεσίες](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Δημιουργία διοχέτευσης
Σε αυτό το βήμα, μπορείτε να δημιουργήσετε το πρώτο διοχέτευσης με μια δραστηριότητα **HDInsightHive** . Εισαγωγής φέτα είναι διαθέσιμη κάθε μήνα (συχνότητα: μήνα, διάστημα: 1), φέτα εξόδου παράγεται μηνιαία και είναι επίσης να ορίσετε την ιδιότητα χρονοδιαγράμματος για τη δραστηριότητα σε μηνιαία. Πρέπει να συμφωνεί με τις ρυθμίσεις για το σύνολο δεδομένων εξόδου και το Χρονοδιάγραμμα δραστηριότητας. Προς το παρόν, σύνολο δεδομένων εξόδου είναι τι κατευθύνει το χρονοδιάγραμμα, ώστε να πρέπει να δημιουργήσετε ένα σύνολο δεδομένων εξόδου ακόμα και αν τη δραστηριότητα δεν παράγει κανένα αποτέλεσμα. Εάν η δραστηριότητα μην απαιτηθεί οποιαδήποτε εισαγωγή, μπορείτε να παραλείψετε τη δημιουργία του συνόλου δεδομένων εισόδου. Οι ιδιότητες που χρησιμοποιούνται στο το παρακάτω JSON εξηγούνται στο τέλος αυτής της ενότητας. 

1. Στο πρόγραμμα **Επεξεργασίας εργοστασίου δεδομένων**, κάντε κλικ στην επιλογή **αποσιωπητικά (...) Περισσότερες εντολές** και, στη συνέχεια, κάντε κλικ στην επιλογή **νέα διοχέτευσης**.
    
    ![κουμπί "νέα διαδικασία"](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Αντιγράψτε και επικολλήστε το παρακάτω τμήμα κώδικα στο παράθυρο σχέδιο-1.

    > [AZURE.IMPORTANT] Αντικατάσταση **storageaccountname** με το όνομα του λογαριασμού σας χώρο αποθήκευσης στο το JSON.
        
        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService",
                            "defines": {
                                "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                                "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                            }
                        },
                        "inputs": [
                            {
                                "name": "AzureBlobInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "AzureBlobOutput"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "HDInsightOnDemandLinkedService"
                    }
                ],
                "start": "2016-04-01T00:00:00Z",
                "end": "2016-04-02T00:00:00Z",
                "isPaused": false
            }
        }
 
    Με το τμήμα κώδικα JSON, θέλετε να δημιουργήσετε μια διαδικασία που αποτελείται από μια μεμονωμένη δραστηριότητα που χρησιμοποιεί Hive στη διαδικασία δεδομένων σε ένα σύμπλεγμα HDInsight.
    
    Το αρχείο δέσμης ενεργειών ομάδας, **partitionweblogs.hql**, αποθηκεύεται στο ο λογαριασμός Azure χώρου αποθήκευσης (που καθορίζεται από το scriptLinkedService, που ονομάζεται **AzureStorageLinkedService**) και στο φάκελο **δέσμης ενεργειών** σε το κοντέινερ **adfgetstarted**.

    Η ενότητα **ορίζει** χρησιμοποιείται για να καθορίσετε τις ρυθμίσεις χρόνου εκτέλεσης που μεταβιβάζονται στη δέσμη ενεργειών hive ως τιμές παραμέτρων Hive (π.χ. ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

    Τις ιδιότητες **Έναρξη** και **Λήξη** της διοχέτευσης Καθορίζει την ενεργή περίοδο της διοχέτευσης.

    Στη δραστηριότητα JSON, μπορείτε να καθορίσετε ότι η δέσμη ενεργειών Hive εκτελείται σε του υπολογισμού που καθορίζεται από το **linkedServiceName** – **HDInsightOnDemandLinkedService**.

    > [AZURE.NOTE] Για λεπτομέρειες σχετικά με τις ιδιότητες JSON που χρησιμοποιείται στο παράδειγμα, ανατρέξτε στο θέμα [Ανατομία μια διαδικασία](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 

3. Επιβεβαιώστε τα εξής: 
    1. υπάρχει **Input.log** αρχείο στο φάκελο **inputdata** του κοντέινερ **adfgetstarted** στο το χώρο αποθήκευσης αντικειμένων blob του Azure
    2. υπάρχει **partitionweblogs.hql** αρχείο στο φάκελο **δέσμης ενεργειών** του κοντέινερ **adfgetstarted** στο το χώρο αποθήκευσης αντικειμένων blob του Azure. Ολοκληρώστε την προϋπόθεση βήματα στην [Εκμάθηση Επισκόπηση](data-factory-build-your-first-pipeline.md) Εάν δεν μπορείτε να δείτε αυτά τα αρχεία. 
    3. Επιβεβαιώστε ότι αντικατασταθεί **storageaccountname** με το όνομα του λογαριασμού σας χώρο αποθήκευσης στη διοχέτευση JSON. 
2. Κάντε κλικ στο κουμπί " **Ανάπτυξη** " στη γραμμή εντολών για την ανάπτυξη της διοχέτευσης. Δεδομένου ότι έχουν οριστεί τις ώρες **έναρξης** και **λήξης** στο παρελθόν και **isPaused** έχει οριστεί σε false, της διοχέτευσης (δραστηριότητα στη διοχέτευση) εκτελείται αμέσως μετά την ανάπτυξη. 
4. Επιβεβαιώστε ότι μπορείτε να δείτε τη διαδικασία στην προβολή δέντρου.

    ![Προβολή δέντρου με διοχέτευσης](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
5. Συγχαρητήρια, έχετε δημιουργήσει με επιτυχία το πρώτο διοχέτευσης!

## <a name="monitor-pipeline"></a>Οθόνη διοχέτευσης

### <a name="monitor-pipeline-using-diagram-view"></a>Οθόνη διοχέτευσης χρησιμοποιώντας την προβολή διαγράμματος

6. Κάντε κλικ στο **X** για να κλείσετε το πρόγραμμα επεξεργασίας δεδομένων εργοστασίου λεπίδες και για να επιστρέψετε το blade εργοστασίου δεδομένων και κάντε κλικ στο **διάγραμμα**.
  
    ![Πλακίδιο του διαγράμματος](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
7. Στην προβολή διαγράμματος, μπορείτε να δείτε μια επισκόπηση των αγωγών και των συνόλων δεδομένων που χρησιμοποιούνται σε αυτό το πρόγραμμα εκμάθησης.
    
    ![Προβολή διαγράμματος](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png) 
8. Για να προβάλετε όλες τις δραστηριότητες της διοχέτευσης, κάντε δεξί κλικ διοχέτευσης στο διάγραμμα και κάντε κλικ στην επιλογή Άνοιγμα διοχέτευσης. 

    ![Άνοιγμα διοχέτευσης μενού](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
9. Επιβεβαιώστε ότι μπορείτε να δείτε τη δραστηριότητα HDInsightHive στη διοχέτευση. 
  
    ![Άνοιγμα διοχέτευσης προβολή](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Για να επιστρέψετε στην προηγούμενη προβολή, κάντε κλικ στην επιλογή **προέλευση δεδομένων** στο μενού δυναμικής διαδρομής στο επάνω μέρος. 
10. Στην **Προβολή "Διάγραμμα"**, κάντε διπλό κλικ του συνόλου δεδομένων **AzureBlobInput**. Επιβεβαιώστε ότι η φέτα είναι σε κατάσταση **είστε έτοιμοι** . Ενδέχεται να χρειαστούν μερικά λεπτά για τη φέτα ώστε να εμφανίζεται σε κατάσταση είστε έτοιμοι. Εάν δεν εκτελεστεί μετά περιμένετε κάποια στιγμή, ανατρέξτε στο θέμα εάν έχετε το αρχείο εισόδου (input.log) τοποθετείται στο φάκελο (inputdata) and δεξιά κοντέινερ (adfgetstarted).

    ![Εισαγωγής φέτα έτοιμη κατάσταση](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
11. Κάντε κλικ στο **X** για να κλείσετε **AzureBlobInput** blade. 
12. Στην **Προβολή "Διάγραμμα"**, κάντε διπλό κλικ του συνόλου δεδομένων **AzureBlobOutput**. Που βλέπετε στη φέτα που υποβάλλεται σε επεξεργασία.

    ![Σύνολο δεδομένων](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
9. Όταν ολοκληρωθεί η επεξεργασία, μπορείτε να δείτε τη φέτα **έτοιμη** κατάσταση.
    
>[AZURE.IMPORTANT] Η δημιουργία ένα σύμπλεγμα HDInsight σε ζήτηση συνήθως χρειάζεται κάποια στιγμή (περίπου 20 λεπτά). Επομένως, περιμένετε τη διαδικασία για να **περιέχει έως και 30 λεπτά** για την επεξεργασία στη φέτα.    

    ![Dataset](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png) 
    
10. Όταν στη φέτα βρίσκεται σε κατάσταση **είστε έτοιμοι** , ελέγξτε το φάκελο **partitioneddata** στο κοντέινερ **adfgetstarted** στο χώρο αποθήκευσης αντικειμένων blob του για τα δεδομένα εξόδου.  
 
    ![δεδομένα εξόδου](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
11. Κάντε κλικ στη φέτα για να δείτε λεπτομέρειες σχετικά με αυτήν σε ένα **κομμάτι δεδομένων** blade.

    ![Λεπτομέρειες φέτα δεδομένων](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
12. Κάντε κλικ στην επιλογή μιας δραστηριότητας εκτέλεση στη **λίστα εκτελείται δραστηριότητας** για να δείτε λεπτομέρειες σχετικά με μια δραστηριότητα Εκτέλεση (Hive δραστηριότητα σε σενάριο μας) σε ένα παράθυρο **δραστηριότητας εκτέλεση λεπτομέρειες** .   

    ![Εκτέλεση λεπτομέρειες δραστηριότητας](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)  
    
    Από τα αρχεία καταγραφής, μπορείτε να δείτε το ερώτημα Hive που εκτελέστηκε και οι πληροφορίες κατάστασης. Αυτά τα αρχεία καταγραφής είναι χρήσιμη για την αντιμετώπιση τυχόν προβλημάτων.
Ανατρέξτε στο θέμα [οθόνη και να διαχειριστείτε αγωγούς χρησιμοποιώντας Azure πύλης λεπίδες](data-factory-monitor-manage-pipelines.md) άρθρου για περισσότερες λεπτομέρειες. 

> [AZURE.IMPORTANT] Το αρχείο εισαγωγής λαμβάνει διαγραφεί κατά τη φέτα υποβάλλεται σε επεξεργασία με επιτυχία. Επομένως, εάν θέλετε να εκτελέσετε ξανά στη φέτα ή κάντε ξανά το πρόγραμμα εκμάθησης, στείλτε το αρχείο εισόδου (input.log) στο φάκελο inputdata του κοντέινερ adfgetstarted.

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Παρακολούθηση της διοχέτευσης με οθόνη & Διαχείριση εφαρμογών
Μπορείτε επίσης να χρησιμοποιείτε οθόνη & Διαχείριση εφαρμογών για την παρακολούθηση της σας αγωγούς. Για λεπτομερείς πληροφορίες σχετικά με τη χρήση αυτής της εφαρμογής, ανατρέξτε στο θέμα [οθόνη και να διαχειριστείτε αγωγούς εργοστασίου δεδομένων Azure χρησιμοποιώντας παρακολούθησης και διαχείρισης εφαρμογών](data-factory-monitor-manage-app.md).

1. Κάντε κλικ στην επιλογή **Παρακολούθηση και διαχείριση** πλακίδιο στην αρχική σελίδα για την προέλευση δεδομένων.

    ![Παρακολούθηση και διαχείριση πλακιδίων](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png) 
2. Θα πρέπει να βλέπετε **την οθόνη και διαχείριση εφαρμογών**. Αλλάξτε **την ώρα έναρξης** και **ώρα λήξης** ώστε να ταιριάζει με Έναρξη (04-01-2016 12:00 πμ) και λήξης (04-02-2016 12:00 πμ) της διοχέτευσης σας, και κάντε κλικ στο κουμπί **εφαρμογή**.

    ![Παρακολούθηση και διαχείριση εφαρμογών](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png) 
3. Επιλέξτε ένα παράθυρο δραστηριότητα στη λίστα των **Windows δραστηριότητας** για να δείτε λεπτομέρειες σχετικά με αυτήν. 
    ![Λεπτομέρειες παραθύρου δραστηριότητα](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)


## <a name="summary"></a>Σύνοψη 
Σε αυτό το πρόγραμμα εκμάθησης, δημιουργήσατε ένα εργοστασίου Azure δεδομένων την επεξεργασία δεδομένων, εκτελώντας Hive δέσμης ενεργειών σε ένα σύμπλεγμα hadoop HDInsight. Χρησιμοποιήσατε το πρόγραμμα επεξεργασίας εργοστασίου δεδομένων στην πύλη του Azure για να εκτελέσετε τα παρακάτω βήματα:  

1.  Δημιουργία μιας Azure **εργοστασίου δεδομένων**.
2.  Δημιουργία δύο **συνδεδεμένες υπηρεσίες**:
    1.  Υπηρεσία **Αποθήκευσης azure** συνδεδεμένες για να συνδέσετε το χώρο αποθήκευσης αντικειμένων blob του Azure που περιέχει τα αρχεία εισόδου/εξόδου για την προέλευση δεδομένων.
    2.  **Azure HDInsight** σε ζήτηση συνδεδεμένων υπηρεσία για να συνδέσετε ένα σύμπλεγμα HDInsight Hadoop σε ζήτηση με την προέλευση δεδομένων. Azure εργοστασίου δεδομένων δημιουργεί μια HDInsight Hadoop σύμπλεγμα απλώς-σε-ώρα για την επεξεργασία δεδομένων εισόδου και να παραγάγετε δεδομένα εξόδου. 
3.  Δημιουργία δύο **συνόλων δεδομένων**, που περιγράφουν δεδομένα εισόδου και εξόδου για ομάδα HDInsight δραστηριότητα στη διοχέτευση. 
4.  Δημιουργία μιας **διοχέτευσης** με μια δραστηριότητα **HDInsight Hive** . 

## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το άρθρο, έχετε δημιουργήσει μια διαδικασία με μια δραστηριότητα μετασχηματισμού (HDInsight δραστηριότητα) που εκτελεί μια δέσμη ενεργειών της ομάδας σε ένα σύμπλεγμα HDInsight στη ζήτηση. Για να δείτε πώς μπορείτε να χρησιμοποιήσετε μια δραστηριότητα Αντιγραφή για να αντιγράψετε δεδομένα από ένα αντικειμένων Blob του Azure Azure SQL, ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: αντιγραφή δεδομένων από μια Azure αντικειμένων blob σε Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Δείτε επίσης
| Το θέμα | Περιγραφή |
| :---- | :---- |
| [Δραστηριότητες μετασχηματισμού δεδομένων](data-factory-data-transformation-activities.md) | Σε αυτό το άρθρο παρέχει μια λίστα των δραστηριοτήτων μετασχηματισμό δεδομένων (όπως το HDInsight Hive μετασχηματισμό που χρησιμοποιείται σε αυτό το πρόγραμμα εκμάθησης) υποστηρίζονται από το Azure εργοστασίου δεδομένων. | 
| [Προγραμματισμός και εκτέλεσης](data-factory-scheduling-and-execution.md) | Σε αυτό το άρθρο εξηγεί τις προγραμματισμού και εκτέλεσης πτυχές της εργοστασίου δεδομένων Azure μοντέλο εφαρμογών. |
| [Αγωγούς](data-factory-create-pipelines.md) | Σε αυτό το άρθρο σάς βοηθά να κατανοήσετε αγωγούς και δραστηριοτήτων στις εργοστασιακές δεδομένων Azure και πώς να τους χρησιμοποιήσετε για να δημιουργήσετε ολοκληρωμένες να βασίζονται σε δεδομένα ροές εργασίας για το σενάριο ή επιχειρήσεις. |
| [Σύνολα δεδομένων](data-factory-create-datasets.md) | Σε αυτό το άρθρο σάς βοηθά να κατανοήσετε συνόλων δεδομένων στην προέλευση δεδομένων Azure.
| [Παρακολούθηση και διαχείριση αγωγούς χρήση της εφαρμογής παρακολούθησης](data-factory-monitor-manage-app.md) | Σε αυτό το άρθρο περιγράφει τον τρόπο για την παρακολούθηση, διαχείριση και εντοπισμός σφαλμάτων αγωγούς με την παρακολούθηση & Διαχείριση εφαρμογών. 

  

