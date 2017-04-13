<properties
    pageTitle="Επεξεργασία ευρείας κλίμακας συνόλων δεδομένων χρησιμοποιώντας εργοστασίου δεδομένων και τη δέσμη | Microsoft Azure"
    description="Περιγράφει τον τρόπο για να επεξεργαστείτε τεράστιες ποσότητες δεδομένων σε μια διαδικασία εργοστασίου δεδομένων Azure χρησιμοποιώντας τις δυνατότητες επεξεργασίας παράλληλες της δέσμης Azure."
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
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="spelluru"/>

# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a>Διεργασία συνόλων δεδομένων ευρείας κλίμακας χρησιμοποιώντας εργοστασίου δεδομένων και τη δέσμη
Σε αυτό το άρθρο περιγράφει μια αρχιτεκτονική μιας λύσης δείγμα που μετακινείται και επεξεργάζεται συνόλων δεδομένων ευρείας κλίμακας με τον τρόπο αυτόματης και τις προγραμματισμένες. Επίσης, παρέχει μια ολοκληρωμένες για αναλυτικές οδηγίες για την υλοποίηση της λύσης χρησιμοποιώντας Azure εργοστασίου δεδομένων και τη δέσμη Azure. 

Σε αυτό το άρθρο είναι μεγαλύτερο από το τυπικό άρθρο επειδή περιέχει αναλυτικές οδηγίες για μια λύση σύνολο του δείγματος. Εάν είστε εξοικειωμένοι με τη δέσμη και εργοστασίου δεδομένων, μπορείτε να μάθετε σχετικά με αυτές τις υπηρεσίες και πώς λειτουργούν μαζί. Εάν γνωρίζετε κάτι σχετικά με τις υπηρεσίες και σχεδίαση/Εξειδικευμένος μια λύση, ενδέχεται να μπορείτε να εστιάσετε μόνο στην [ενότητα αρχιτεκτονική](#architecture-of-sample-solution) αυτού του άρθρου και εάν σχεδιάζετε μια πρωτοτύπου ή μια λύση, μπορεί επίσης να θέλετε να δοκιμάσετε οδηγίες βήμα προς βήμα σε το [αναλυτικές οδηγίες](#implementation-of-sample-solution). Σας προσκαλούμε τα σχόλιά σας σχετικά με αυτό το περιεχόμενο και πώς να χρησιμοποιήσετε.

Πρώτα, ας ρίξουμε μια ματιά πώς υπηρεσίες δεδομένων εργοστασίου και δέσμη μπορεί να σας βοηθήσει με την επεξεργασία μεγάλων συνόλων δεδομένων στο cloud.     

## <a name="why-azure-batch"></a>Γιατί Azure δέσμη;
Azure δέσμη σάς επιτρέπει να εκτελείτε εφαρμογές ευρείας κλίμακας παράλληλες και υψηλών επιδόσεων υπολογιστική (HPC) αποτελεσματικά στο cloud. Πρόκειται για μια υπηρεσία πλατφόρμα που προγραμματίζει την εργασία στενής υπολογισμού για να εκτελέσετε σε μια διαχειριζόμενη συλλογή εικονικές μηχανές και να αυτόματα κλίμακα τον υπολογισμό πόρους για να ανταποκρίνονται στις ανάγκες των εργασιών σας.

Με την υπηρεσία δέσμη, μπορείτε να καθορίσετε τους πόρους Azure υπολογισμού για να εκτελέσετε τις εφαρμογές σας παράλληλα και με κλίμακα. Μπορείτε να εκτελέσετε σε ζήτηση ή προγραμματισμένες εργασίες και δεν χρειάζεται να με μη αυτόματο τρόπο δημιουργία, ρύθμιση παραμέτρων και διαχείριση ένα σύμπλεγμα HPC, μεμονωμένα εικονικές μηχανές, εικονικό δίκτυα ή μια σύνθετη εργασία και υποδομή του προγραμματισμού εργασιών.

Ανατρέξτε στα ακόλουθα άρθρα, εάν δεν είστε εξοικειωμένοι με τη μαζική Azure καθώς συμβάλλει με κατανόηση την αρχιτεκτονική/εφαρμογή της λύσης που περιγράφονται σε αυτό το άρθρο.   

- [Βασικά στοιχεία της δέσμης Azure](../batch/batch-technical-overview.md)
- [Επισκόπηση δυνατοτήτων δέσμης](../batch/batch-api-basics.md)

(προαιρετικό) Για να μάθετε περισσότερα σχετικά με τη δέσμη Azure, ανατρέξτε στο θέμα [διαδρομή εκμάθησης για μαζική Azure](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Γιατί εργοστασίου Azure δεδομένων;
Προέλευση δεδομένων είναι μια υπηρεσία ενοποίησης δεδομένων που βασίζεται στο cloud που orchestrates και αυτοματοποιεί την κίνηση και Μετασχηματισμός των δεδομένων. Με την υπηρεσία εργοστασίου δεδομένων, μπορείτε να δημιουργήσετε αγωγούς διαχειριζόμενων δεδομένων που μετακίνηση δεδομένων από εσωτερικής εγκατάστασης και cloud αποθηκεύει δεδομένα σε ένα χώρο αποθήκευσης κεντρική δεδομένων (για παράδειγμα: χώρος αποθήκευσης αντικειμένων Blob του Azure), και διαδικασία/μετασχηματισμού δεδομένων με τη χρήση υπηρεσιών όπως Azure HDInsight και Azure μηχανικής εκμάθησης. Μπορείτε επίσης να προγραμματίσετε αγωγούς δεδομένων για να εκτελέσετε σε μια προγραμματισμένη τρόπο (ανά ώρα, ημερήσια, εβδομαδιαία, κ.λπ.) και την οθόνη και να διαχειριστείτε τους με μια ματιά για τον προσδιορισμό προβλημάτων και την εκτέλεση ενεργειών. 

Ανατρέξτε στα ακόλουθα άρθρα, εάν δεν είστε εξοικειωμένοι με το Azure εργοστασίου δεδομένων καθώς συμβάλλει με κατανόηση την αρχιτεκτονική/εφαρμογή της λύσης που περιγράφονται σε αυτό το άρθρο.  

- [Εισαγωγή δεδομένων Azure εργοστασίου](data-factory-introduction.md)
- [Δημιουργία του πρώτου διοχέτευσης δεδομένων](data-factory-build-your-first-pipeline.md)   

(προαιρετικό) Για να μάθετε περισσότερα σχετικά με το Azure εργοστασίου δεδομένων, ανατρέξτε στο θέμα [διαδρομή εκμάθησης για το Azure εργοστασίου δεδομένων](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Προέλευση δεδομένων και τη δέσμη μαζί
Εργοστασίου δεδομένων περιλαμβάνει ενσωματωμένη δραστηριότητες όπως η αντιγραφή δραστηριότητας για αντιγραφή/μετακίνηση δεδομένων από ένα χώρο αποθήκευσης δεδομένων προέλευσης σε ένα χώρο αποθήκευσης δεδομένων προορισμού και Hive δραστηριότητας την επεξεργασία δεδομένων χρησιμοποιώντας συμπλεγμάτων Hadoop (HDInsight) στο Azure. Ανατρέξτε στο θέμα [Δραστηριότητες μετασχηματισμού δεδομένων](data-factory-data-transformation-activities.md) για μια λίστα των δραστηριοτήτων υποστηριζόμενες μετασχηματισμό. 

Επίσης, μπορείτε να δημιουργήσετε προσαρμοσμένες δραστηριότητες .NET για να μετακινήσετε ή να επεξεργάζονται δεδομένα με το δικό σας λογική και εκτέλεση αυτών των δραστηριοτήτων σε ένα σύμπλεγμα Azure HDInsight ή σε ένα χώρο συγκέντρωσης δέσμη Azure του ΣΠΣ. Όταν χρησιμοποιείτε δέσμη Azure, μπορείτε να ρυθμίσετε το χώρο συγκέντρωσης για αυτόματη κλίμακα (Προσθήκη ή κατάργηση ΣΠΣ με βάση το φόρτο εργασίας) που βασίζεται σε έναν τύπο που παρέχετε.     

## <a name="architecture-of-sample-solution"></a>Αρχιτεκτονική λύσης δείγματος
Παρόλο που είναι η αρχιτεκτονική που περιγράφονται σε αυτό το άρθρο για μια απλή λύση, είναι σχετική με σύνθετα σενάρια όπως κινδύνου μοντελοποίησης οικονομικές υπηρεσίες, επεξεργασίας εικόνας και απόδοσης και γονιδιωματικής ανάλυσης. 

Το διάγραμμα παρουσιάζει 1) πώς εργοστασίου δεδομένων orchestrates κίνηση δεδομένων και την επεξεργασία και 2) πώς δέσμη Azure επεξεργάζεται τα δεδομένα σε παράλληλες. Λήψη ή εκτύπωση του διαγράμματος για εύκολη αναφορά (11 x 17 στο. ή του μεγέθους A3): [ενορχήστρωσης HPC και δεδομένων με χρήση δέσμης Azure και εργοστασίου δεδομένων](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Διάγραμμα ευρείας κλίμακας επεξεργασίας δεδομένων](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

Η παρακάτω λίστα παρέχει τα βασικά βήματα της διαδικασίας. Η λύση περιλαμβάνει κώδικα και επεξηγήσεις για τη δημιουργία της λύσης για ολοκληρωμένες.

1.  **Ρύθμιση παραμέτρων δέσμη Azure με ένα χώρο συγκέντρωσης κόμβους υπολογιστικών (ΣΠΣ)**. Μπορείτε να καθορίσετε τον αριθμό των κόμβους και το μέγεθος των κάθε κόμβο.

2.  **Δημιουργία μια παρουσία Azure εργοστασίου δεδομένων** που έχει ρυθμιστεί με οντοτήτων που αντιπροσωπεύουν χώρο αποθήκευσης αντικειμένων blob του Azure, υπηρεσία υπολογισμού δέσμη Azure, δεδομένα εισόδου/εξόδου και μια ροή εργασίας/σωλήνωσης με δραστηριότητες που μετακίνηση και μετασχηματισμού δεδομένων.

3.   **Δημιουργία μιας προσαρμοσμένης δραστηριότητας .NET στη διοχέτευση εργοστασίου δεδομένων**. Η δραστηριότητα είναι σας κώδικα χρήστη που εκτελείται στο χώρο συγκέντρωσης δέσμη Azure.

4.  **Μεγάλες ποσότητες δεδομένων εισαγωγής ως αντικείμενα blob στο χώρο αποθήκευσης Azure χώρου αποθήκευσης**. Δεδομένα χωρίζεται σε λογικές φέτες (συνήθως ανά ώρα).

5.  **Δεδομένα εργοστασίου αντιγράφει τα δεδομένα που υποβάλλεται σε επεξεργασία παράλληλα** στη δευτερεύουσα θέση.

6.  **Δεδομένα εργοστασίου εκτελείται η προσαρμοσμένη δραστηριότητα με χρήση του χώρου συγκέντρωσης εκχωρηθεί από δέσμη**. Εργοστασίου δεδομένων μπορούν να εκτελούνται δραστηριότητες ταυτόχρονα. Κάθε δραστηριότητα επεξεργάζεται ένα κομμάτι των δεδομένων. Τα αποτελέσματα είναι αποθηκευμένα στο χώρο αποθήκευσης Azure.

7.  **Δεδομένα εργοστασίου μετακινεί τα τελικά αποτελέσματα σε μια τρίτη θέση**, είτε για διανομή μέσω μιας εφαρμογής, ή για περαιτέρω επεξεργασία από άλλα εργαλεία.

## <a name="implementation-of-sample-solution"></a>Εφαρμογή του δείγματος λύσης
Η λύση δείγμα είναι σκόπιμα απλές και για να σας δείξουν πώς μπορείτε να χρησιμοποιήσετε δεδομένα εργοστασίου και δέσμη μαζί για την επεξεργασία των συνόλων δεδομένων. Τη λύση απλώς την καταμέτρηση του αριθμού των εμφανίσεων από έναν όρο αναζήτησης ("Microsoft") στο εισαγωγής αρχείων οργανωμένα σε μια σειρά ώρας. Εμφανίζει το πλήθος σε αρχεία εξόδου.

**Ώρα**: Εάν είστε εξοικειωμένοι με τις βασικές δυνατότητες του Azure, δεδομένα εργοστασίου και δέσμης και έχετε ολοκληρώσει τις προϋποθέσεις που παρατίθενται παρακάτω, θα σας εκτίμηση αυτήν τη λύση σας μεταφέρει 1-2 ώρες μέχρι να ολοκληρωθεί.

### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

#### <a name="azure-subscription"></a>Azure συνδρομή
Εάν δεν έχετε μια συνδρομή του Azure, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Ανατρέξτε στο θέμα [δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Λογαριασμός Azure χώρου αποθήκευσης
Μπορείτε να χρησιμοποιήσετε ένα λογαριασμό Azure χώρου αποθήκευσης για την αποθήκευση των δεδομένων σε αυτό το πρόγραμμα εκμάθησης. Εάν δεν έχετε ένα λογαριασμό Azure χώρου αποθήκευσης, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού χώρου αποθήκευσης](../storage/storage-create-storage-account.md#create-a-storage-account). Η λύση δείγμα χρησιμοποιεί χώρο αποθήκευσης αντικειμένων blob.

#### <a name="azure-batch-account"></a>Λογαριασμός Azure δέσμης
Δημιουργήστε ένα λογαριασμό Azure δέσμη με την [πύλη του Azure](http://manage.windowsazure.com/). Ανατρέξτε στο θέμα [Δημιουργία και διαχείριση λογαριασμού δέσμη Azure](../batch/batch-account-create-portal.md). Σημείωση το πλήκτρο δέσμη Azure λογαριασμό όνομα και το λογαριασμό. Μπορείτε επίσης να χρησιμοποιήσετε το cmdlet [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) για να δημιουργήσετε ένα λογαριασμό δέσμη Azure. Για λεπτομερείς οδηγίες σχετικά με τη χρήση αυτό το cmdlet, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το cmdlet του PowerShell δέσμη Azure](../batch/batch-powershell-cmdlets-get-started.md) .

Η λύση δείγμα χρησιμοποιεί δέσμη Azure (έμμεσα μέσω ενός εργοστασίου δεδομένων Azure διοχέτευσης) την επεξεργασία δεδομένων σε παράλληλες, σε ένα χώρο συγκέντρωσης κόμβους υπολογιστικών (μια συλλογή διαχειριζόμενων εικονικές μηχανές).

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a>Azure συγκέντρωσης δέσμη εικονικές μηχανές (ΣΠΣ)
Δημιουργία ενός **χώρου συγκέντρωσης Azure δέσμη** με τουλάχιστον 2 κόμβους υπολογιστικών.

1.  Στην [πύλη του Azure](https://portal.azure.com), κάντε κλικ στην επιλογή **Αναζήτηση** στο αριστερό μενού και κάντε κλικ στην επιλογή **Λογαριασμοί δέσμης**. 
2. Επιλέξτε το λογαριασμό σας Azure δέσμη για να ανοίξετε το blade **Δέσμη λογαριασμού** . 
3. Κάντε κλικ στο πλακίδιο **χώρους συγκέντρωσης** .
4. Στο blade τα **σύνολα** , κάντε κλικ στο κουμπί Προσθήκη στη γραμμή εργαλείων για να προσθέσετε ένα χώρο συγκέντρωσης.
    1. Πληκτρολογήστε ένα Αναγνωριστικό για το χώρο συγκέντρωσης (**Αναγνωριστικό χώρου συγκέντρωσης**). Σημειώστε το **Αναγνωριστικό του χώρου συγκέντρωσης**; χρειάζεστε κατά τη δημιουργία της λύσης εργοστασίου δεδομένων. 
    2. Καθορίστε **Windows Server 2012 R2** για τη ρύθμιση της οικογένειας λειτουργικού συστήματος.
    3. Επιλέξτε ένα **επίπεδο τιμολόγησης κόμβο**.
    4. Εισαγάγετε **2** ως τιμή για τη ρύθμιση **Αφοσιωμένη προορισμού** .
    5. Εισαγάγετε **2** ως τιμή για τη ρύθμιση **Max εργασίες ανά κόμβο** .
    6. Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το χώρο συγκέντρωσης. 
    
#### <a name="azure-storage-explorer"></a>Εξερεύνηση Azure χώρου αποθήκευσης   
[Azure αποθήκευσης Explorer 6 (εργαλείο)](https://azurestorageexplorer.codeplex.com/) ή [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (από λογισμικό ClumsyLeaf). Μπορείτε να χρησιμοποιήσετε αυτά τα εργαλεία για έλεγχο και την τροποποίηση των δεδομένων σε έργα σας Azure χώρου αποθήκευσης, όπως τα αρχεία καταγραφής των εφαρμογών σας που φιλοξενείται στο cloud.

1.  Δημιουργία κοντέινερ που ονομάζεται **mycontainer** με ιδιωτική access (χωρίς ανώνυμης πρόσβασης)

2.  Εάν χρησιμοποιείτε **CloudXplorer**, δημιουργήστε φακέλους και υποφακέλους με την ακόλουθη δομή:

    ![](./media/data-factory-data-processing-using-batch/image3.png)

    **Inputfolder** και **outputfolder** είναι ανώτατου επιπέδου φακέλων στο **mycontainer,** και το **inputfolder** έχει υποφακέλους με σημάνσεις ημερομηνίας-ώρας (εεεε-MM-DD-HH).

    Εάν χρησιμοποιείτε **Azure Εξερεύνηση χώρου αποθήκευσης**, στο επόμενο βήμα, πρέπει να αποστείλετε αρχεία με ονόματα: inputfolder/2015-11-16-00/file.txt, inputfolder/2015-11-16-01/file.txt και ούτω καθεξής. Αυτό το βήμα δημιουργεί αυτόματα τους φακέλους.

3.  Δημιουργήστε ένα αρχείο κειμένου **αρχείο file.txt** στον υπολογιστή σας με το περιεχόμενο που έχει τη λέξη-κλειδί **της Microsoft**. Για παράδειγμα: "δοκιμή προσαρμοσμένη δραστηριότητα Microsoft δοκιμής προσαρμοσμένη δραστηριότητα Microsoft".

4.  Αποστείλετε το αρχείο για να τους παρακάτω φακέλους εισαγωγής στο χώρο αποθήκευσης αντικειμένων blob του Azure.

    ![](./media/data-factory-data-processing-using-batch/image4.png)

    Εάν χρησιμοποιείτε **Azure Εξερεύνηση χώρου αποθήκευσης**, αποστείλετε το αρχείο **αρχείο file.txt** **mycontainer**. Στη γραμμή εργαλείων για να δημιουργήσετε ένα αντίγραφο του blob του, κάντε κλικ στην επιλογή **Αντιγραφή** . Στο παράθυρο διαλόγου **Αντιγραφή αντικειμένων Blob** , αλλάξτε το **όνομα blob προορισμού** για να **inputfolder/2015-11-16-00/file.txt.** Επαναλάβετε αυτό το βήμα για να δημιουργήσετε inputfolder/2015-11-16-01/file.txt, inputfolder/2015-11-16-02/file.txt, inputfolder/2015-11-16-03/file.txt, inputfolder/2015-11-16-04/file.txt και ούτω καθεξής. Αυτή η ενέργεια δημιουργεί αυτόματα τους φακέλους.

3.  Δημιουργήστε ένα άλλο κοντέινερ που ονομάζεται: **customactivitycontainer**. Μπορείτε να αποστείλετε το αρχείο zip προσαρμοσμένη δραστηριότητα σε αυτό το κοντέινερ.

#### <a name="visual-studio"></a>Visual Studio
Εγκαταστήστε το Microsoft Visual Studio 2012 ή νεότερη έκδοση για να δημιουργήσετε την προσαρμοσμένη δραστηριότητα δέσμη να χρησιμοποιηθεί με τη λύση εργοστασίου δεδομένων.

### <a name="high-level-steps-to-create-the-solution"></a>Βήματα υψηλού επιπέδου για τη δημιουργία της λύσης

1.  Δημιουργήστε μια προσαρμοσμένη δραστηριότητα που περιέχει τη λογική επεξεργασίας δεδομένων.
2.  Δημιουργία ενός εργοστασίου Azure δεδομένων που χρησιμοποιεί την προσαρμοσμένη δραστηριότητα:

### <a name="create-the-custom-activity"></a>Δημιουργήστε την προσαρμοσμένη δραστηριότητα

Η προσαρμοσμένη δραστηριότητα εργοστασίου δεδομένων είναι η καρδιά αυτής της λύσης δείγμα. Η λύση δείγμα χρησιμοποιεί Azure δέσμη για να εκτελέσετε την προσαρμοσμένη δραστηριότητα. Ανατρέξτε στο θέμα [Χρήση προσαρμοσμένες δραστηριότητες σε μια διαδικασία Azure εργοστασίου δεδομένων](data-factory-use-custom-activities.md) για τις βασικές πληροφορίες για την ανάπτυξη προσαρμοσμένες δραστηριότητες και να τις χρησιμοποιήσετε σε αγωγούς εργοστασίου δεδομένων Azure.

Για να δημιουργήσετε μια προσαρμοσμένη δραστηριότητα .NET που μπορείτε να χρησιμοποιήσετε μια διοχέτευση εργοστασίου δεδομένων Azure, πρέπει να δημιουργήσετε ένα έργο **Βιβλιοθήκη κλάσεων .NET** με μια κλάση που υλοποιεί αυτήν τη διασύνδεση **IDotNetActivity** . Αυτή η διασύνδεση έχει μόνο μία μέθοδο: **Εκτέλεση**. Ακολουθεί η υπογραφή της μεθόδου:

    public IDictionary<string, string> Execute(
                IEnumerable<LinkedService> linkedServices,
                IEnumerable<Dataset> datasets,
                Activity activity,
                IActivityLogger logger)

Η μέθοδος έχει ορισμένα βασικά στοιχεία που πρέπει να κατανοήσετε.

-   Η μέθοδος λαμβάνει τέσσερις παράμετροι:

    1.  **linkedServices**. Ένα αριθμήσιμο λίστα των συνδεδεμένων υπηρεσιών που σύνδεση προελεύσεων δεδομένων εισόδου/εξόδου (για παράδειγμα: χώρος αποθήκευσης αντικειμένων Blob του Azure) για την προέλευση δεδομένων. Σε αυτό το δείγμα, υπάρχει μόνο ένα συνδεδεμένο της υπηρεσίας τύπου αποθήκευσης Azure που χρησιμοποιείται για εισόδου και εξόδου.

    2.  **σύνολα δεδομένων**. Αυτή είναι μια λίστα αριθμήσιμο των συνόλων δεδομένων. Μπορείτε να χρησιμοποιήσετε αυτήν την παράμετρο για να λάβετε τις θέσεις και τα σχήματα που ορίζονται από σύνολα δεδομένων εισόδου και εξόδου.

    3.  **δραστηριότητα**. Αυτή η παράμετρος αντιπροσωπεύει την τρέχουσα υπολογισμού οντότητα - σε αυτήν την περίπτωση, μια υπηρεσία δέσμη Azure.

    4.  **πρόγραμμα καταγραφής**. Στο πρόγραμμα καταγραφής σάς επιτρέπει να γράψετε σχόλια εντοπισμού σφαλμάτων που surface με την καταγραφή "Χρήστης" για τη διαδικασία.

-   Η μέθοδος επιστρέφει ένα λεξικό που μπορεί να χρησιμοποιηθεί προκειμένου να συνδέσει προσαρμοσμένες δραστηριότητες στο μέλλον. Αυτή η δυνατότητα δεν έχει υλοποιηθεί ακόμα, επομένως, επιστρέφει ένα κενό λεξικό από τη μέθοδο. 

#### <a name="procedure-create-the-custom-activity"></a>Διαδικασία: Δημιουργία την προσαρμοσμένη δραστηριότητα

1.  Δημιουργήστε ένα έργο βιβλιοθήκη κλάσεων .NET στο Visual Studio.

    1.  Εκκίνηση του **Visual Studio 2012**/**2013/2015**.

    2.  Κάντε κλικ στο **αρχείο**, επιλέξτε **Δημιουργία**και κάντε κλικ στο **έργο**.

    3.  Αναπτύξτε **πρότυπα**και επιλέξτε **οπτική C\#**. Σε αυτόν τον οδηγό, μπορείτε να χρησιμοποιήσετε C\#, αλλά μπορείτε να χρησιμοποιήσετε οποιαδήποτε γλώσσα .NET για να αναπτύξετε την προσαρμοσμένη δραστηριότητα.

    4.  Επιλέξτε **Βιβλιοθήκη κλάσεων** από τη λίστα των τύπων έργου στη δεξιά πλευρά.

    5.  Πληκτρολογήστε **MyDotNetActivity** για το **όνομα**.

    6.  Επιλέξτε **C:\\ADF** για τη **θέση**. Δημιουργήστε το φάκελο **ADF** , εάν δεν υπάρχει.

    7.  Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο.

2.  Κάντε κλικ στην επιλογή **Εργαλεία**, οδηγεί στη **Διαχείριση πακέτου NuGet**και κάντε κλικ στην επιλογή **Κονσόλα διαχείρισης πακέτου**.

3.  Στην **Κονσόλα διαχείρισης πακέτου**, εκτελέστε την ακόλουθη εντολή για να εισαγάγετε **Microsoft.Azure.Management.DataFactories**.

            Install-Package Microsoft.Azure.Management.DataFactories

4.  Εισαγωγή του πακέτου NuGet **Αποθήκευσης Azure** στο στο έργο. Χρειάζεστε αυτού του πακέτου, επειδή μπορείτε να χρησιμοποιήσετε το χώρο αποθήκευσης αντικειμένων Blob API σε αυτό το δείγμα.

        Install-Package Azure.Storage

5.  Προσθέστε τις ακόλουθες οδηγίες **χρησιμοποιώντας** το αρχείο προέλευσης του έργου.

        using System.IO;
        using System.Globalization;
        using System.Diagnostics;
        using System.Linq;

        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Runtime;

        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

6.  Αλλάξτε το όνομα του πεδίου **χώρο ονομάτων** σε **MyDotNetActivityNS**.

        namespace MyDotNetActivityNS

7.  Αλλάξτε το όνομα της κατηγορίας σε **MyDotNetActivity** και προέρχεται από το περιβάλλον εργασίας **IDotNetActivity** , όπως φαίνεται παρακάτω.

        public class MyDotNetActivity : IDotNetActivity

8.  Υλοποίηση της μεθόδου (Προσθήκη) η **Εκτέλεση** του περιβάλλοντος εργασίας **IDotNetActivity** στην κλάση **MyDotNetActivity** και αντιγράψτε τον παρακάτω κώδικα δείγμα τη μέθοδο. Ανατρέξτε στην ενότητα [Εκτέλεση μεθόδου](#execute-method) για επεξήγηση για της λογικής που χρησιμοποιούνται σε αυτήν τη μέθοδο.

        /// <summary>
        /// Execute method is the only method of IDotNetActivity interface you must implement.
        /// In this sample, the method invokes the Calculate method to perform the core logic.  
        /// </summary>
        public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
        {

            // declare types for input and output data stores
            AzureStorageLinkedService inputLinkedService;

            Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
            foreach (LinkedService ls in linkedServices)
                logger.Write("linkedService.Name {0}", ls.Name);

            // using First method instead of Single since we are using the same
            // Azure Storage linked service for input and output.
            inputLinkedService = linkedServices.First(
                linkedService =>
                linkedService.Name ==
                inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
                as AzureStorageLinkedService;

            string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
            string folderPath = GetFolderPath(inputDataset);
            string output = string.Empty; // for use later.

            // create storage client for input. Pass the connection string.
            CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
            CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();

            // initialize the continuation token before using it in the do-while loop.
            BlobContinuationToken continuationToken = null;
            do
            {   // get the list of input blobs from the input storage client object.
                BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                         true,
                                         BlobListingDetails.Metadata,
                                         null,
                                         continuationToken,
                                         null,
                                         null);

                // Calculate method returns the number of occurrences of
                // the search term (“Microsoft”) in each blob associated
                // with the data slice.
                //
                // definition of the method is shown in the next step.
                output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

            } while (continuationToken != null);

            // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
            Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

            folderPath = GetFolderPath(outputDataset);

            logger.Write("Writing blob to the folder: {0}", folderPath);

            // create a storage object for the output blob.
            CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
            // write the name of the file.
            Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

            logger.Write("output blob URI: {0}", outputBlobUri.ToString());
            // create a blob and upload the output text.
            CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
            logger.Write("Writing {0} to the output blob", output);
            outputBlob.UploadText(output);

            // The dictionary can be used to chain custom activities together in the future.
            // This feature is not implemented yet, so just return an empty dictionary.
            return new Dictionary<string, string>();
        }


9.  Προσθέστε τις ακόλουθες μεθόδους Βοήθειας για την τάξη. Αυτές οι μέθοδοι ενεργοποιούνται από τη μέθοδο **Execute** . Σημαντικότερο, η μέθοδος **Υπολογισμός** απομονώνει τον κώδικα που επαναλαμβάνεται μέσω κάθε blob.

        /// <summary>
        /// Gets the folderPath value from the input/output dataset.
        /// </summary>
        private static string GetFolderPath(Dataset dataArtifact)
        {
            if (dataArtifact == null || dataArtifact.Properties == null)
            {
                return null;
            }

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

            return blobDataset.FolderPath;
        }

        /// <summary>
        /// Gets the fileName value from the input/output dataset.
        /// </summary>

        private static string GetFileName(Dataset dataArtifact)
        {
            if (dataArtifact == null || dataArtifact.Properties == null)
            {
                return null;
            }

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

            return blobDataset.FileName;
        }

        /// <summary>
        /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
        /// and prepares the output text that is written to the output blob.
        /// </summary>

        public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
        {
            string output = string.Empty;
            logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
            foreach (IListBlobItem listBlobItem in Bresult.Results)
            {
                CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
                if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
                {
                    string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                    logger.Write("input blob text: {0}", blobText);
                    string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                    var matchQuery = from word in source
                                     where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                     select word;
                    int wordCount = matchQuery.Count();
                    output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
                }
            }
            return output;
        }


    Η μέθοδος **GetFolderPath** επιστρέφει τη διαδρομή στο φάκελο που δείχνει το σύνολο δεδομένων και τη μέθοδο **GetFileName** επιστρέφει το όνομα του blob/αρχείου που δείχνει το σύνολο δεδομένων.

        "name": "InputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "file.txt",
                "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",


    Η μέθοδος **Υπολογισμός** υπολογίζει τον αριθμό των παρουσιών της λέξης-κλειδιού **Microsoft** στη εισαγωγής αρχείων (αντικείμενα blob στο φάκελο). Τον όρο αναζήτησης ("Microsoft") είναι σχεδιασμένου στον κώδικα.

10.  Μεταγλώττιση του έργου. Κάντε κλικ στην επιλογή **Δημιουργία** από το μενού και κάντε κλικ στην επιλογή **Δημιουργία λύσης**.

11.  Εκκίνηση της **Εξερεύνησης των Windows**και μεταβείτε στις επιλογές **Ανακύκλωσης\\εντοπισμός σφαλμάτων** ή **Ανακύκλωσης\\αφήστε** φάκελο ανάλογα με τον τύπο του build.

12.  Δημιουργήστε ένα αρχείο zip **MyDotNetActivity.zip** που περιέχει όλα τα δυαδικά αρχεία στην το ** \\Ανακύκλωσης\\εντοπισμός σφαλμάτων** φακέλου. Ενδέχεται να θέλετε να συμπεριλάβετε το MyDotNetActivity. του αρχείου **PDB** ώστε να εμφανίζονται πρόσθετες λεπτομέρειες, όπως τον αριθμό γραμμών στον πηγαίο κώδικα που προκάλεσαν το ζήτημα κατά την αποτυχία παρουσιάζεται.

    ![](./media/data-factory-data-processing-using-batch/image5.png)

13.  Αποστολή **MyDotNetActivity.zip** ως blob στο κοντέινερ αντικειμένων blob: **customactivitycontainer** στο το χώρο αποθήκευσης αντικειμένων blob του Azure ότι το **StorageLinkedService** συνδεδεμένο υπηρεσίας στο το **ADFTutorialDataFactory** χρησιμοποιεί. Δημιουργήστε το κοντέινερ αντικειμένων blob **customactivitycontainer** , εάν δεν υπάρχει ήδη.

#### <a name="execute-method"></a>Εκτέλεση μεθόδου

Αυτή η ενότητα παρέχει περισσότερες λεπτομέρειες και τις σημειώσεις σχετικά με τον κωδικό της μεθόδου Execute.

1.  Τα μέλη για διαδοχικές προσεγγίσεις μέσα στη συλλογή εισαγωγής βρίσκονται στο χώρο ονομάτων [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) . Διαδοχικές προσεγγίσεις μέσα στη συλλογή blob απαιτεί τη χρήση της κλάσης **BlobContinuationToken** . Στην πραγματικότητα, πρέπει να χρησιμοποιήσετε μια-κατά την επανάληψη με το διακριτικό ως μηχανισμό για την έξοδο από το βρόχο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από .NET](../storage/storage-dotnet-how-to-use-blobs.md). Ένα βασικό βρόχο εμφανίζεται εδώ:

        // Initialize the continuation token.
        BlobContinuationToken continuationToken = null;
        do
        {
        // Get the list of input blobs from the input storage client object.
        BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                true,
                                          BlobListingDetails.Metadata,
                                          null,
                                          continuationToken,
                                          null,
                                          null);
        // Return a string derived from parsing each blob.
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

        } while (continuationToken != null);

    Ανατρέξτε στην τεκμηρίωση για τη μέθοδο [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) για λεπτομέρειες.

2.  Τον κωδικό για την εργασία από το σύνολο των αντικειμένων blob λογικά μεταβαίνει σε το-κατά την επανάληψη. Στη μέθοδο **Execute** , το-ενώ βρόχος μεταφέρει τη λίστα των αντικειμένων blob μια μέθοδο που ονομάζεται **Υπολογισμός**. Η μέθοδος επιστρέφει μια μεταβλητή συμβολοσειράς με το όνομα **εξόδου** που είναι το αποτέλεσμα της αντιμετωπίζετε iterated μέσω όλα τα αντικείμενα blob στο τμήμα.

    Επιστρέφει τον αριθμό των εμφανίσεων από τον όρο αναζήτησης (**Microsoft**) στο αντικείμενο blob που του μεταβιβάστηκε η μέθοδος **Υπολογισμός** .

        output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);

3.  Όταν η μέθοδος **Υπολογισμός** ολοκληρωθεί η εργασία, πρέπει να εγγράφονται σε ένα νέο blob. Επομένως, για κάθε σύνολο αντικείμενα BLOB υποβάλλονται σε επεξεργασία, ένα νέο blob μπορούν να εγγραφούν με τα αποτελέσματα. Για να γράψετε σε ένα νέο blob, πρέπει πρώτα να βρείτε το σύνολο δεδομένων εξόδου.

        // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

4.  Ο κώδικας καλεί επίσης μια μέθοδο βοηθητική εφαρμογή του: **GetFolderPath** για να ανακτήσετε τη διαδρομή του φακέλου (το όνομα κοντέινερ χώρου αποθήκευσης).

        folderPath = GetFolderPath(outputDataset);

    Το **GetFolderPath** μετατρέπει ρητά το αντικείμενο σύνολο δεδομένων σε ένα AzureBlobDataSet, η οποία έχει μια ιδιότητα με το όνομα FolderPath.

        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

        return blobDataset.FolderPath;

5.  Ο κώδικας καλεί τη μέθοδο **GetFileName** για να ανακτήσετε το όνομα του αρχείου (όνομα blob). Ο κώδικας είναι παρόμοιο με το παραπάνω κωδικός για να λάβετε τη διαδρομή του φακέλου.

        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

        return blobDataset.FileName;

6.  Το όνομα του αρχείου είναι γραμμένο με τη δημιουργία ενός αντικειμένου URI. Η κατασκευή URI χρησιμοποιεί την ιδιότητα **BlobEndpoint** για να επιστρέψει το όνομα του κοντέινερ. Για να δημιουργήσετε το αντικείμενο blob εξόδου URI προστίθεται το φάκελο διαδρομή και το όνομα αρχείου.  

        // Write the name of the file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

7.  Το όνομα του αρχείου που έχει εγγραφεί και τώρα μπορείτε να καταγράψετε τη συμβολοσειρά εξόδου από τη μέθοδο **Υπολογισμός** σε ένα νέο blob:

        // Create a blob and upload the output text.
        CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
        logger.Write("Writing {0} to the output blob", output);
        outputBlob.UploadText(output);


### <a name="create-the-data-factory"></a>Δημιουργήστε την προέλευση δεδομένων

Στην ενότητα [Δημιουργία την προσαρμοσμένη δραστηριότητα](#create-the-custom-activity) , μπορείτε να δημιουργήσετε μια προσαρμοσμένη δραστηριότητα και αποστείλει το αρχείο zip με τα δυαδικά αρχεία και το αρχείο PDB σε ένα κοντέινερ αντικειμένων blob του Azure. Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια Azure **εργοστασίου δεδομένων** με μια **διαδικασία** που χρησιμοποιεί την **προσαρμοσμένη δραστηριότητα**.

Το σύνολο δεδομένων εισόδου για την προσαρμοσμένη δραστηριότητα αντιπροσωπεύει τα αντικείμενα BLOB (αρχεία) στο φάκελο εισαγωγής (mycontainer\\inputfolder) στο χώρο αποθήκευσης αντικειμένων blob. Το σύνολο δεδομένων εξόδου για τη δραστηριότητα αντιπροσωπεύει τα αντικείμενα BLOB εξόδου στο φάκελο εξόδου (mycontainer\\outputfolder) στο χώρο αποθήκευσης αντικειμένων blob.

Απόθεση ενός ή περισσότερων αρχείων στους φακέλους εισαγωγής:

    mycontainer -\> inputfolder
        2015-11-16-00
        2015-11-16-01
        2015-11-16-02
        2015-11-16-03
        2015-11-16-04

Για παράδειγμα, αποθέστε ενός αρχείου (αρχείο file.txt) με το παρακάτω περιεχόμενο σε κάθε έναν από τους φακέλους.

    test custom activity Microsoft test custom activity Microsoft

Κάθε φάκελος εισόδου αντιστοιχεί σε ένα κομμάτι του Azure δεδομένων εργοστασίου ακόμα και αν ο φάκελος έχει 2 ή περισσότερα αρχεία. Κατά την επεξεργασία κάθε φέτα μέσω της διοχέτευσης, επαναλαμβάνεται η προσαρμοσμένη δραστηριότητα σε όλα τα αντικείμενα blob στο φάκελο εισόδου για αυτήν τη φέτα.

Βλέπετε πέντε αρχεία εξόδου με το ίδιο περιεχόμενο. Για παράδειγμα, το αρχείο εξόδου από την επεξεργασία του αρχείου στο φάκελο 2015-11-16-00 περιλαμβάνει το παρακάτω περιεχόμενο:

    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.

Εάν απόρριψη πολλών αρχείων (αρχείο file.txt, file2.txt, file3.txt) με το ίδιο περιεχόμενο στο φάκελο εισαγωγής, εμφανίζεται το παρακάτω περιεχόμενο στο αρχείο εξόδου. Κάθε φάκελο (2015-11-16-00, κ.λπ.) αντιστοιχεί σε ένα κομμάτι σε αυτό το δείγμα, παρόλο που ο φάκελος έχει πολλά αρχεία εισόδου.

    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.


Το αρχείο εξόδου έχει τρεις γραμμές τώρα, μία για κάθε αρχείο εισόδου (blob) στο φάκελο που σχετίζεται με τη φέτα (2015-11-16-00).

Για κάθε δραστηριότητα εκτέλεση δημιουργείται μια εργασία. Σε αυτό το δείγμα, υπάρχει μόνο μία δραστηριότητα στη διοχέτευση. Κατά την επεξεργασία μιας φέτα μέσω της διοχέτευσης, την προσαρμοσμένη δραστηριότητα εκτελείται σε Azure μαζική επεξεργασία στη φέτα. Επειδή υπάρχουν πέντε φέτες (κάθε φέτα μπορεί να έχει πολλά αντικείμενα BLOB ή αρχείο), υπάρχουν πέντε εργασίες που έχουν δημιουργηθεί σε δέσμη Azure. Κατά την εκτέλεση μιας εργασίας στη δέσμη, είναι στην πραγματικότητα την προσαρμοσμένη δραστηριότητα που εκτελείται.

Η ακόλουθη αναλυτική παρουσίαση παρέχει περισσότερες λεπτομέρειες.

#### <a name="step-1-create-the-data-factory"></a>Βήμα 1: Δημιουργήστε την προέλευση δεδομένων

1.  Μετά την καταγραφή στο [Azure πύλη](https://portal.azure.com/), κάντε τα εξής βήματα:

    1.  Κάντε κλικ στην επιλογή " **ΔΗΜΙΟΥΡΓΊΑ** " στο αριστερό μενού.

    2.  Κάντε κλικ στην επιλογή **δεδομένων + αναλυτικών στοιχείων** στο blade τη **νέα** .

    3.  Κάντε κλικ στην εντολή **Εργοστασίου δεδομένων** blade την **ανάλυση δεδομένων** .

2.  Στο blade τη **νέα προέλευση δεδομένων** , πληκτρολογήστε **CustomActivityFactory** για το όνομα. Το όνομα του εργοστασίου Azure δεδομένων πρέπει να είναι μοναδικό καθολικό. Εάν λαμβάνετε το μήνυμα σφάλματος: **όνομα εργοστασίου δεδομένων "CustomActivityFactory" δεν είναι διαθέσιμη**, αλλάξτε το όνομα του εργοστασίου δεδομένων (για παράδειγμα, **yournameCustomActivityFactory**) και δοκιμάστε να δημιουργήσετε ξανά.

3.  Κάντε κλικ στο **ΌΝΟΜΑ της ΟΜΆΔΑΣ ΠΌΡΩΝ**, και επιλέξτε μια υπάρχουσα ομάδα πόρων ή δημιουργήστε μια ομάδα πόρων.

4.  Βεβαιωθείτε ότι χρησιμοποιείτε το σωστό συνδρομή και περιοχής όπου θέλετε το εργοστασίου δεδομένων θα δημιουργηθεί.

5.  Κάντε κλικ στην εντολή **Δημιουργία** blade τη **νέα προέλευση δεδομένων** .

6.  Μπορείτε να δείτε την προέλευση δεδομένων που δημιουργούνται στον πίνακα **εργαλείων** της πύλης Azure.

7.  Μετά την προέλευση δεδομένων έχει δημιουργηθεί με επιτυχία, μπορείτε να δείτε σελίδας εργοστασίου δεδομένων, η οποία εμφανίζονται τα περιεχόμενα του εργοστασίου δεδομένων.

 ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Βήμα 2: Δημιουργία συνδεδεμένων υπηρεσιών

Συνδεδεμένες υπηρεσίες σύνδεση χώροι αποθήκευσης δεδομένων ή τον υπολογισμό υπηρεσίες σε μια εργοστασίου Azure δεδομένων. Σε αυτό το βήμα, μπορείτε να συνδέσετε το λογαριασμό **Χώρου αποθήκευσης Azure** και λογαριασμός **Azure δέσμη** για την προέλευση δεδομένων.

#### <a name="create-azure-storage-linked-service"></a>Δημιουργία χώρου αποθήκευσης Azure συνδεδεμένες υπηρεσίας

1.  Κάντε κλικ στην επιλογή το **συντάκτη και ανάπτυξη** πλακιδίου στην το blade **ΕΡΓΟΣΤΑΣΊΟΥ ΔΕΔΟΜΈΝΩΝ** για το **CustomActivityFactory**. Μπορείτε να δείτε το πρόγραμμα επεξεργασίας εργοστασίου δεδομένων.

2.  Κάντε κλικ στην επιλογή **Αποθήκευση νέα δεδομένα** στη γραμμή εντολών και επιλέξτε **Azure χώρου αποθήκευσης.** Θα πρέπει να δείτε τη δέσμη ενεργειών JSON για τη δημιουργία μια υπηρεσία αποθήκευσης Azure που συνδέονται στο πρόγραμμα επεξεργασίας.

    ![](./media/data-factory-data-processing-using-batch/image7.png)

3.  Αντικαταστήστε **το όνομα του λογαριασμού** με το όνομα του Azure χώρο αποθήκευσης στο λογαριασμό σας και το **κλειδί λογαριασμού** με το πλήκτρο πρόσβασης του λογαριασμού Azure χώρου αποθήκευσης. Για να μάθετε πώς μπορείτε να λάβετε τον αριθμό-κλειδί πρόσβασης χώρου αποθήκευσης, ανατρέξτε στο θέμα [Προβολή "," Αντιγραφή "και" πλήκτρα πρόσβασης regenerate χώρου αποθήκευσης](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

4.  Στη γραμμή εντολών για την ανάπτυξη της συνδεδεμένων υπηρεσίας, κάντε κλικ στην επιλογή **Ανάπτυξη** .

    ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a>Δημιουργία δέσμης Azure συνδεδεμένες υπηρεσίας

Σε αυτό το βήμα, μπορείτε να δημιουργήσετε ένα συνδεδεμένο υπηρεσίας για το λογαριασμό σας **Δέσμη Azure** που χρησιμοποιείται για να εκτελέσετε την προσαρμοσμένη δραστηριότητα εργοστασίου δεδομένων.

1.  Κάντε κλικ στην επιλογή **Δημιουργία τον υπολογισμό** στη γραμμή εντολών και επιλέξτε **Azure δέσμη.** Θα πρέπει να δείτε τη δέσμη ενεργειών JSON για τη δημιουργία μιας υπηρεσίας της δέσμης Azure που συνδέονται στο πρόγραμμα επεξεργασίας.

2.  Στη δέσμη ενεργειών JSON:

    1.  Αντικαταστήστε **το όνομα του λογαριασμού** με το όνομα του λογαριασμού σας δέσμη Azure.

    2.  Αντικαταστήστε **το πλήκτρο πρόσβασης** με το πλήκτρο πρόσβασης του λογαριασμού δέσμη Azure.

    3.  Πληκτρολογήστε το Αναγνωριστικό του χώρου συγκέντρωσης για την ιδιότητα **poolName** **.** Για αυτήν την ιδιότητα, μπορείτε να καθορίσετε είτε όνομα χώρου συγκέντρωσης ή να χώρο συγκέντρωσης αναγνωριστικό.

    4.  Εισαγάγετε τη δέσμη URI για την ιδιότητα JSON **batchUri** . 
    
        > [AZURE.IMPORTANT] Η **διεύθυνση URL** από το **Azure δέσμη λογαριασμό blade** είναι με την εξής μορφή: \<όνομα λογαριασμού\>. \<περιοχή\>. batch.azure.com. Για την ιδιότητα **batchUri** στο το JSON, πρέπει να **καταργήσετε "όνομα λογαριασμού".** από τη διεύθυνση URL. Παράδειγμα: `"batchUri": "https://eastus.batch.azure.com"`.

        ![](./media/data-factory-data-processing-using-batch/image9.png)

        Για την ιδιότητα **poolName** , μπορείτε επίσης να καθορίσετε το Αναγνωριστικό του χώρου συγκέντρωσης αντί για το όνομα του χώρου συγκέντρωσης.

        > [AZURE.NOTE] Η υπηρεσία εργοστασίου δεδομένων δεν υποστηρίζει μια επιλογή στην απαιτήσεων για μαζική Azure όπως και για HDInsight. Μπορείτε να χρησιμοποιήσετε το δικό σας σύνολο δέσμη Azure μόνο σε ένα εργοστασίου Azure δεδομένων.

    5.  Καθορίστε **StorageLinkedService** για την ιδιότητα **linkedServiceName** . Δημιουργήσατε αυτήν την υπηρεσία συνδεδεμένο στο προηγούμενο βήμα. Αυτός ο χώρος αποθήκευσης χρησιμοποιείται ως περιοχή ενδιάμεσου σταδίου για τα αρχεία και τα αρχεία καταγραφής.

3.  Στη γραμμή εντολών για την ανάπτυξη της συνδεδεμένων υπηρεσίας, κάντε κλικ στην επιλογή **Ανάπτυξη** .

#### <a name="step-3-create-datasets"></a>Βήμα 3: Δημιουργία συνόλων δεδομένων

Σε αυτό το βήμα, δημιουργείτε σύνολα δεδομένων για την αναπαράσταση δεδομένων εισόδου και εξόδου.

#### <a name="create-input-dataset"></a>Δημιουργία συνόλου δεδομένων εισαγωγής

1.  Στο πρόγραμμα **επεξεργασίας** για την προέλευση δεδομένων, κάντε κλικ στο κουμπί **νέο σύνολο δεδομένων** στη γραμμή εργαλείων και κάντε κλικ στην επιλογή **χώρος αποθήκευσης αντικειμένων Blob του Azure** από το αναπτυσσόμενο μενού.

2.  Αντικαταστήστε το JSON στο δεξιό παράθυρο με το παρακάτω τμήμα κώδικα JSON:

        {
            "name": "InputDataset",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
                "typeProperties": {
                    "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
                    "format": {
                        "type": "TextFormat"
                    },
                    "partitionedBy": [
                        {
                            "name": "Year",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "yyyy"
                            }
                        },
                        {
                            "name": "Month",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "MM"
                            }
                        },
                        {
                            "name": "Day",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "dd"
                            }
                        },
                        {
                            "name": "Hour",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "HH"
                            }
                        }
                    ]
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        }


     Μπορείτε να δημιουργήσετε μια διοχέτευση αργότερα σε αυτόν τον οδηγό με ώρα έναρξης: 2015-11-16T00:00:00Z και λήξης χρόνου: 2015-11-16T05:00:00Z. Έχει προγραμματιστεί για την παραγωγή δεδομένων **ανά ώρα**, ώστε να υπάρχουν 5 φέτες εισόδου/εξόδου (μεταξύ **00**: 00:00 -\> **05**: 00:00).

     Τη **συχνότητα** και το **χρονικό διάστημα** για το σύνολο δεδομένων εισόδου έχει οριστεί σε **ώρα** και **1**, που σημαίνει ότι η εισαγωγής φέτα είναι διαθέσιμα ανά ώρα.

     Εδώ θα βρείτε τις ώρες έναρξης για κάθε φέτα, το οποίο αντιπροσωπεύεται από μεταβλητή συστήματος **SliceStart** στο το παραπάνω τμήμα κώδικα JSON.

  	| **Φέτα** | **Ώρα έναρξης**          |
  	|-----------|-------------------------|
  	| 1         | 2015-11-16T**00**: 00:00 |
  	| 2         | 2015-11-16T**01**: 00:00 |
  	| 3         | 2015-11-16T**02**: 00:00 |
  	| 4         | 2015-11-16T**03**: 00:00 |
  	| 5         | 2015-11-16T**04**: 00:00 |

     Η **folderPath** υπολογίζεται χρησιμοποιώντας το έτος, μήνας, ημέρα και ώρα τμήμα της ώρας έναρξης φέτα (**SliceStart**). Γι ' αυτό, θα δείτε τον τρόπο εισαγωγής ενός φακέλου έχει αντιστοιχιστεί σε ένα κομμάτι.

  	| **Φέτα** | **Ώρα έναρξης**          | **Φάκελος εισόδου**  |
  	|-----------|-------------------------|-------------------|
  	| 1         | 2015-11-16T**00**: 00:00 | 2015-11-16 -**00** |
  	| 2         | 2015-11-16T**01**: 00:00 | 2015-11-16 -**01** |
  	| 3         | 2015-11-16T**02**: 00:00 | 2015-11-16 -**02** |
  	| 4         | 2015-11-16T**03**: 00:00 | 2015-11-16 -**03** |
  	| 5         | 2015-11-16T**04**: 00:00 | 2015-11-16 -**04** |

3.  Κάντε κλικ στο κουμπί " **Ανάπτυξη** " στη γραμμή εργαλείων για να δημιουργήσετε και να αναπτύξετε τον πίνακα **InputDataset** . 

#### <a name="create-output-dataset"></a>Δημιουργία συνόλου δεδομένων εξόδου

Σε αυτό το βήμα, μπορείτε να δημιουργήσετε ένα άλλο σύνολο δεδομένων AzureBlob για να αντιπροσωπεύει τα δεδομένα εξόδου του τύπου.

1.  Στο πρόγραμμα **επεξεργασίας** για την προέλευση δεδομένων, κάντε κλικ στο κουμπί **νέο σύνολο δεδομένων** στη γραμμή εργαλείων και κάντε κλικ στην επιλογή **χώρος αποθήκευσης αντικειμένων Blob του Azure** από το αναπτυσσόμενο μενού.

2.  Αντικαταστήστε το JSON στο δεξιό παράθυρο με το παρακάτω τμήμα κώδικα JSON:

        {
            "name": "OutputDataset",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
                "typeProperties": {
                    "fileName": "{slice}.txt",
                    "folderPath": "mycontainer/outputfolder",
                    "partitionedBy": [
                        {
                            "name": "slice",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "yyyy-MM-dd-HH"
                            }
                        }
                    ]
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }


    Αρχείο εξόδου blob/δημιουργείται για κάθε φέτα εισαγωγής. Παρακάτω θα δείτε πώς ονομάζεται ένα αρχείο εξόδου για κάθε φέτα. Όλα τα αρχεία εξόδου δημιουργούνται σε ένα φάκελο εξόδου: **mycontainer\\outputfolder**.


  	| **Φέτα** | **Ώρα έναρξης**          | **Αρχείο εξόδου**       |
  	|-----------|-------------------------|-----------------------|
  	| 1         | 2015-11-16T**00**: 00:00 | 2015-11-16 -**00. txt** |
  	| 2         | 2015-11-16T**01**: 00:00 | 2015-11-16 -**01. txt** |
  	| 3         | 2015-11-16T**02**: 00:00 | 2015-11-16 -**02. txt** |
  	| 4         | 2015-11-16T**03**: 00:00 | 2015-11-16 -**03. txt** |
  	| 5         | 2015-11-16T**04**: 00:00 | 2015-11-16 -**04. txt** |

     Να θυμάστε ότι όλα τα αρχεία σε ένα φάκελο του εισόδου (για παράδειγμα: 2015-11-16-00) αποτελούν μέρος μιας φέτα με την ώρα έναρξης: 2015-11-16-00. Κατά την επεξεργασία αυτό φέτα, την προσαρμοσμένη δραστηριότητα σαρώνει μέσω κάθε αρχείο και δημιουργεί μια γραμμή στο αρχείο εξόδου με τον αριθμό των εμφανίσεων της όρο αναζήτησης ("Microsoft"). Εάν υπάρχουν τρία αρχεία στο φάκελο 2015-11-16-00, υπάρχουν τρεις γραμμές στο αρχείο εξόδου: 2015-11-16-00.txt.

3.  Κάντε κλικ στο κουμπί " **Ανάπτυξη** " στη γραμμή εργαλείων για να δημιουργήσετε και να αναπτύξετε το **OutputDataset**.

#### <a name="step-4-create-and-run-the-pipeline-with-custom-activity"></a>Βήμα 4: Δημιουργία και εκτέλεση της διοχέτευσης με προσαρμοσμένη δραστηριότητα

Σε αυτό το βήμα, δημιουργείτε μια διαδικασία με μία δραστηριότητα, την προσαρμοσμένη δραστηριότητα που δημιουργήσατε νωρίτερα.

> [AZURE.IMPORTANT] Εάν δεν έχετε αποστείλει το **αρχείο file.txt** για εισαγωγή τους φακέλους στο κοντέινερ αντικειμένων blob, κάνετε πριν από τη δημιουργία της διοχέτευσης. Η ιδιότητα **isPaused** έχει οριστεί σε false στη διοχέτευση JSON, ώστε να της διοχέτευσης εκτελεί αμέσως, όπως η ημερομηνία **έναρξης** είναι στο παρελθόν.

1.  Στο πρόγραμμα επεξεργασίας εργοστασίου τα δεδομένα, κάντε κλικ στην επιλογή **νέα διοχέτευσης** στη γραμμή εντολών. Εάν δεν βλέπετε την εντολή, κάντε κλικ στην επιλογή **... (Αποσιωπητικά)** Για να το δείτε.

2.  Αντικαταστήστε το JSON στο δεξιό παράθυρο, με την ακόλουθη δέσμη ενεργειών JSON:

        {
            "name": "PipelineCustom",
            "properties": {
                "description": "Use custom activity",
                "activities": [
                    {
                        "type": "DotNetActivity",
                        "typeProperties": {
                            "assemblyName": "MyDotNetActivity.dll",
                            "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                            "packageLinkedService": "AzureStorageLinkedService",
                            "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                        },
                        "inputs": [
                            {
                                "name": "InputDataset"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "OutputDataset"
                            }
                        ],
                        "policy": {
                            "timeout": "00:30:00",
                            "concurrency": 5,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MyDotNetActivity",
                        "linkedServiceName": "AzureBatchLinkedService"
                    }
                ],
                "start": "2015-11-16T00:00:00Z",
                "end": "2015-11-16T05:00:00Z",
                "isPaused": false
           }
        }

    Λάβετε υπόψη τα εξής σημεία:

    -   Υπάρχει μόνο μία δραστηριότητα στη διοχέτευση και που είναι τύπου: **DotNetActivity**.

    -   **AssemblyName** έχει οριστεί στο όνομα του αρχείου DLL: **MyDotNetActivity.dll**.

    -   **EntryPoint** έχει οριστεί σε **MyDotNetActivityNS.MyDotNetActivity**. Είναι στην ουσία \<χώρο ονομάτων\>. \<όνομα κλάσης\> στον κώδικά σας.

    -   **PackageLinkedService** έχει οριστεί σε **StorageLinkedService** που οδηγεί στο χώρο αποθήκευσης αντικειμένων blob που περιέχει το αρχείο zip προσαρμοσμένη δραστηριότητα. Εάν χρησιμοποιείτε διαφορετικό χώρο αποθήκευσης Azure λογαριασμούς για τα αρχεία εισόδου/εξόδου και το αρχείο zip προσαρμοσμένη δραστηριότητα, πρέπει να δημιουργήσετε μια άλλη υπηρεσία αποθήκευσης Azure συνδεδεμένες. Σε αυτό το άρθρο προϋποθέτει ότι χρησιμοποιείτε τον ίδιο λογαριασμό αποθήκευσης Azure.

    -   **PackageFile** έχει οριστεί σε **customactivitycontainer/MyDotNetActivity.zip**. Έχει τη μορφή: \<containerforthezip\>/\<nameofthezip.zip\>.

    -   Η προσαρμοσμένη δραστηριότητα λαμβάνει **InputDataset** ως είσοδο και **OutputDataset** ως αποτέλεσμα.

    -   Η ιδιότητα **linkedServiceName** την προσαρμοσμένη δραστηριότητα σημεία του **AzureBatchLinkedService**, που σας ενημερώνει για εργοστασίου δεδομένων Azure που την προσαρμοσμένη δραστηριότητα πρέπει να εκτελεστούν σε δέσμη Azure.

    -   Η ρύθμιση **ταυτόχρονης εκτέλεσης** είναι σημαντικές. Εάν χρησιμοποιείτε την προεπιλεγμένη τιμή, η οποία είναι 1, ακόμα και αν έχετε 2 ή περισσότερη υπολογιστική κόμβους στο χώρο συγκέντρωσης δέσμη Azure, επεξεργάζονται τις φέτες του ένα μετά το άλλο. Επομένως, δεν αξιοποιείτε της δυνατότητας παράλληλες επεξεργασίας της δέσμης Azure. Εάν ορίσετε **ταυτόχρονης εκτέλεσης** σε υψηλότερη τιμή, ας υποθέσουμε 2, αυτό σημαίνει ότι δύο χωρίζει (αντιστοιχεί σε δύο εργασίες σε δέσμη Azure) είναι δυνατή η επεξεργασία την ίδια στιγμή, σε αυτήν την περίπτωση και οι δύο του ΣΠΣ στη δέσμη Azure είναι χρησιμοποιούμενη χώρου συγκέντρωσης. Γι ' αυτό, ορίστε την ιδιότητα ταυτόχρονης εκτέλεσης σωστά.

    -   Μία μόνο εργασία (φέτα) εκτελείται σε μια Εικονική σε οποιοδήποτε σημείο από προεπιλογή. Η αιτία είναι ότι, από προεπιλογή, η **μέγιστη εργασίες ανά Εικονική** έχει οριστεί σε 1 για ένα χώρο συγκέντρωσης δέσμη Azure. Ως μέρος της τις προϋποθέσεις, δημιουργήσατε ένα χώρο συγκέντρωσης με αυτήν την ιδιότητα ρυθμισμένη σε 2, ώστε να δύο φέτες εργοστασίου δεδομένων μπορεί να εκτελείται σε μια Εικονική την ίδια στιγμή.


    -   ιδιότητα **isPaused** έχει οριστεί σε false από προεπιλογή. Της διοχέτευσης εκτελεί αμέσως σε αυτό το παράδειγμα, επειδή τις φέτες Έναρξη στο παρελθόν. Μπορείτε να ορίσετε αυτήν την ιδιότητα στην τιμή true για παύση της διοχέτευσης και την τιμή false για να ξεκινήσετε ξανά.

    -   Η ώρα **έναρξης** και ώρα **λήξης** είναι πέντε ώρες απομακρύνετε και φέτες παράγονται ανά ώρα, ώστε να πέντε φέτες παράγονται από τη διαδικασία.

3.  Κάντε κλικ στο κουμπί " **Ανάπτυξη** " στη γραμμή εντολών για την ανάπτυξη της διοχέτευσης.

#### <a name="step-5-test-the-pipeline"></a>Βήμα 5: Έλεγχος της διοχέτευσης

Σε αυτό το βήμα, μπορείτε να ελέγξετε τη διαδικασία αποθέτοντας αρχεία στους φακέλους εισαγωγής. Ας ξεκινήσουμε με τον έλεγχο της διοχέτευσης με ένα αρχείο ανά ένα φάκελο εισαγωγής.

1.  Στο το blade εργοστασίου δεδομένων στην πύλη του Azure, κάντε κλικ στην επιλογή **διαγράμματος**.

    ![](./media/data-factory-data-processing-using-batch/image10.png)

2.  Στην προβολή διαγράμματος, κάντε διπλό κλικ στο σύνολο δεδομένων εισόδου: **InputDataset**.

    ![](./media/data-factory-data-processing-using-batch/image11.png)

3.  Θα πρέπει να βλέπετε το blade **InputDataset** με έτοιμοι πέντε φέτες. Παρατηρήστε την **ΏΡΑ ΈΝΑΡΞΗΣ ΦΈΤΑ** και την **ΏΡΑ ΛΉΞΗΣ ΦΈΤΑ** για κάθε φέτα.

    ![](./media/data-factory-data-processing-using-batch/image12.png)

4.  Στην **Προβολή "Διάγραμμα"**, επιλέξτε τώρα **OutputDataset**.

5.  Θα πρέπει να βλέπετε ότι τις φέτες πέντε εξόδου είναι σε κατάσταση έτοιμη Εάν ήδη έχουν παραχθεί.

    ![](./media/data-factory-data-processing-using-batch/image13.png)

6.  Χρησιμοποιήστε Azure πύλη για να προβάλετε τις **εργασίες** που σχετίζονται με τις **φέτες** και να δείτε ποια Εικονική κάθε φέτα εκτελέσατε σε. Ανατρέξτε στο θέμα [εργοστασίου δεδομένων και την ενοποίηση δέσμη](#data-factory-and-batch-integration) ενότητα για λεπτομέρειες. 

7.  Θα πρέπει να βλέπετε τα αρχεία εξόδου στο το **outputfolder** του **mycontainer** στο χώρο αποθήκευσης αντικειμένων blob του Azure του.

    ![](./media/data-factory-data-processing-using-batch/image15.png)

    Θα πρέπει να βλέπετε πέντε αρχεία εξόδου, μία για κάθε φέτα εισαγωγής. Κάθε μία από το αρχείο εξόδου θα πρέπει να έχετε περιεχόμενο παρόμοιο με το εξής αποτέλεσμα:

        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.

    Το παρακάτω διάγραμμα παρουσιάζει πώς τις φέτες εργοστασίου δεδομένων αντιστοίχιση σε εργασίες στο Azure δέσμης. Σε αυτό το παράδειγμα, μια φέτα έχει μόνο μία εκτέλεση.

    ![](./media/data-factory-data-processing-using-batch/image16.png)

8.  Τώρα, ας δοκιμάσουμε με πολλαπλά αρχεία σε ένα φάκελο. Δημιουργία αρχείων: **file2.txt**, **file3.txt**, **file4.txt**και **file5.txt** με το ίδιο περιεχόμενο όπως στο αρχείο file.txt στο φάκελο: **2015 11-06--01**.

9.  Στο φάκελο εξόδου, **Διαγράψτε** το αρχείο εξόδου: **2015-11-16-01.txt**.

10. Τώρα, στο το blade **OutputDataset** , κάντε δεξί κλικ στη φέτα με **ΏΡΑ ΈΝΑΡΞΗΣ ΦΈΤΑ** οριστεί **16/11/2015 01:00:00 πμ**, και κάντε κλικ στο κουμπί **Εκτέλεση** για να Επανεκτέλεση/επαναλήψεις-process στη φέτα. Τώρα, η φέτα έχει πέντε αρχεία αντί για ένα αρχείο.

    ![](./media/data-factory-data-processing-using-batch/image17.png)

11. Μετά την εκτέλεση στη φέτα και η κατάστασή του είναι **έτοιμη**, επιβεβαιώστε το περιεχόμενο στο αρχείο εξόδου για αυτήν τη φέτα (**2015-11-16-01.txt**) στο το **outputfolder** του **mycontainer** στο χώρο αποθήκευσης αντικειμένων blob του. Πρέπει να υπάρχει μια γραμμή για κάθε αρχείο δεδομένων του.

        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.


> [AZURE.NOTE] Εάν δεν διαγράψατε το αποτέλεσμα του αρχείου 2015-11-16-01.txt πριν να επιχειρήσετε με πέντε αρχεία εισόδου, θα δείτε μια γραμμή από την προηγούμενη εκτέλεση φέτα και πέντε γραμμές από την τρέχουσα εκτέλεση φέτα. Από προεπιλογή, το περιεχόμενο είναι προσαρτημένο στο αρχείο εξόδου, εάν υπάρχει ήδη.

#### <a name="data-factory-and-batch-integration"></a>Ενοποίηση δεδομένων εργοστασίου και δέσμης
Η υπηρεσία εργοστασίου δεδομένων δημιουργεί μια εργασία στη δέσμη Azure με το όνομα: **adf-poolname:job-xxx**. 

![Azure Factory δεδομένων - μαζικές εργασίες](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Δημιουργείται μια εργασία στην εργασία για κάθε Εκτέλεση δραστηριοτήτων ένα κομμάτι. Εάν υπάρχουν 10 φέτες έτοιμο προς επεξεργασία, δημιουργούνται 10 εργασίες της εργασίας. Μπορείτε να έχετε περισσότερες από μία φέτα εκτελείται παράλληλα εάν έχετε πολλές κόμβους υπολογιστικών στο χώρο συγκέντρωσης. Εάν το μέγιστο εργασίες ανά κόμβος έχει οριστεί σε > 1, μπορεί να είναι περισσότερες από μία φέτα λειτουργεί με το ίδιο υπολογισμού.

Σε αυτό το παράδειγμα, υπάρχουν πέντε φέτες, επομένως πέντε εργασίες σε δέσμη Azure. Με το **ταυτόχρονης εκτέλεσης** οριστεί σε **5** στη διοχέτευση JSON στο Azure δεδομένων εργοστασίου και **μέγιστη εργασίες ανά Εικονική** οριστεί στο **2** στο χώρο συγκέντρωσης Azure δέσμη με **2** ΣΠΣ, η γρήγορη εργασίες εκτελείται (ανατρέξτε στο θέμα ώρες έναρξης και λήξης για τις εργασίες).

Χρησιμοποιήστε την πύλη για να προβάλετε τη μαζική εργασία και τις εργασίες που σχετίζονται με τις **φέτες** και δείτε τι Εικονική κάθε φέτα εκτελέσατε σε. 

![Azure Factory δεδομένων - δέσμη εργασιών έργου](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>Εντοπισμός σφαλμάτων της διοχέτευσης

Εντοπισμός σφαλμάτων αποτελείται από ορισμένες βασικές τεχνικές:

1.  Εάν στη φέτα εισαγωγής δεν έχει ρυθμιστεί να **είστε έτοιμοι**, επιβεβαιώστε ότι η δομή του φακέλου εισόδου είναι σωστή και αρχείο file.txt υπάρχει στους φακέλους εισαγωγής.

    ![](./media/data-factory-data-processing-using-batch/image3.png)

2.  Στη μέθοδο **Execute** την προσαρμοσμένη δραστηριότητά σας, χρησιμοποιήστε το αντικείμενο **IActivityLogger** για να καταγράψετε τις πληροφορίες που σας βοηθά να αντιμετωπίσετε τα προβλήματα. Εμφανίζονται τα μηνύματα συνδεδεμένος στο χρήστη\_αρχείο καταγραφής τιμή 0.για.

    Στο το blade **OutputDataset** , κάντε κλικ στη φέτα για να δείτε το blade **ΦΈΤΑ ΔΕΔΟΜΈΝΩΝ** για αυτήν τη φέτα. Μπορείτε να δείτε την **Εκτέλεση δραστηριοτήτων** για αυτήν τη φέτα. Θα πρέπει να δείτε ένα δραστηριότητας εκτέλεση για τη φέτα. Εάν κάνετε κλικ στο κουμπί " **Εκτέλεση** " στη γραμμή εντολών, μπορείτε να ξεκινήσετε μια άλλη δραστηριότητα εκτέλεση για την ίδια φέτα.

    Όταν κάνετε κλικ την εκτέλεση δραστηριοτήτων, μπορείτε να δείτε το blade **ΛΕΠΤΟΜΈΡΕΙΕΣ ΕΚΤΈΛΕΣΗ ΔΡΑΣΤΗΡΙΟΤΉΤΩΝ** με μια λίστα των αρχείων καταγραφής. Δείτε συνδεδεμένος τα μηνύματα του **χρήστη\_καταγραφής τιμή 0.για** αρχείου. Όταν παρουσιάζεται ένα σφάλμα, μπορείτε να δείτε τρεις εκτελείται δραστηριότητας επειδή το πλήθος των επαναλήψεων έχει οριστεί σε 3 στη διοχέτευση/δραστηριότητα JSON. Όταν κάνετε κλικ την εκτέλεση δραστηριοτήτων, μπορείτε να δείτε τα αρχεία καταγραφής που μπορείτε να δείτε την για την αντιμετώπιση προβλημάτων του σφάλματος.

    ![](./media/data-factory-data-processing-using-batch/image18.png)

    Στη λίστα των αρχείων καταγραφής, κάντε κλικ στην επιλογή το **0.log χρήστη**. Στο δεξιό παράθυρο είναι τα αποτελέσματα της χρησιμοποιώντας τη μέθοδο **IActivityLogger.Write** .

    ![](./media/data-factory-data-processing-using-batch/image19.png)

    Ελέγξτε για τυχόν μηνύματα σφάλματος και εξαιρέσεις συστήματος-0.log.

        Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...

        Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...

        Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module

        Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully

3.  Συμπεριλάβετε το αρχείο **PDB** στο αρχείο zip, ώστε να τις λεπτομέρειες του σφάλματος έχουν πληροφορίες όπως **στοίβα κλήσεων** όταν παρουσιάζεται ένα σφάλμα.

4.  Όλα τα αρχεία στο αρχείο zip για την προσαρμοσμένη δραστηριότητα πρέπει να είναι στο **ανώτατο επίπεδο** με χωρίς υποφακέλων.

    ![](./media/data-factory-data-processing-using-batch/image20.png)

5.  Βεβαιωθείτε ότι το **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip) και **packageLinkedService** (πρέπει να κατευθύνουν στο χώρο αποθήκευσης αντικειμένων blob του Azure που περιέχει το αρχείο zip) έχουν ρυθμιστεί με τις σωστές τιμές.

6.  Εάν σταθερής σφάλμα και θέλετε να επαναλάβει την επεξεργασία στη φέτα, κάντε δεξί κλικ στη φέτα στο το blade **OutputDataset** και κάντε κλικ στην επιλογή **Εκτέλεση**.

    ![](./media/data-factory-data-processing-using-batch/image21.png)

    > [AZURE.NOTE] 
    > Μπορείτε να δείτε ένα **κοντέινερ** στο χώρο αποθήκευσης αντικειμένων Blob του Azure του με το όνομα: **adfjobs**. Αυτό το κοντέινερ δεν διαγράφονται αυτόματα, αλλά μπορείτε να το διαγράψετε με ασφάλεια αφού ολοκληρώσετε δοκιμή της λύσης. Ομοίως, η λύση εργοστασίου δεδομένων δημιουργεί μια δέσμη Azure **εργασία** με το όνομα: **adf -\</όνομα Ταυτότητας του χώρου συγκέντρωσης\>: εργασία 0000000001**. Μπορείτε να διαγράψετε αυτήν την εργασία μετά τον έλεγχο της λύσης εάν θέλετε.
7. Η προσαρμοσμένη δραστηριότητα δεν χρησιμοποιεί το αρχείο **app.config** από το πακέτο. Επομένως, εάν σας κώδικα διαβάζει τυχόν συμβολοσειρές σύνδεσης από το αρχείο ρύθμισης παραμέτρων, δεν λειτουργεί κατά το χρόνο εκτέλεσης. Η βέλτιστη πρακτική όταν χρήση δέσμης Azure είναι να κρατάτε οποιαδήποτε απόρρητο σε μια **Azure KeyVault**, χρησιμοποιήστε αρχής υπηρεσία που βασίζεται σε πιστοποιητικό για να προστατεύσετε το keyvault και διανομή το πιστοποιητικό στο χώρο συγκέντρωσης Azure δέσμης. Η προσαρμοσμένη δραστηριότητα .NET, στη συνέχεια, να αποκτήσετε πρόσβαση απόρρητο από το KeyVault κατά το χρόνο εκτέλεσης. Αυτή η λύση είναι ένα γενικό και να περιορίσετε το μέγεθος σε οποιονδήποτε τύπο μυστικό, όχι μόνο συμβολοσειρά σύνδεσης.

    Υπάρχει μια λύση ευκολότερη (αλλά όχι βέλτιστη πρακτική): μπορείτε να δημιουργήσετε μια **υπηρεσία συνδεδεμένες Azure SQL** με σύνδεση ρυθμίσεων συμβολοσειράς, δημιουργήστε ένα σύνολο δεδομένων που χρησιμοποιεί την υπηρεσία συνδεδεμένων, και ποδηλάτου του συνόλου δεδομένων ως μια εικονική συνόλου δεδομένων εισόδου για την προσαρμοσμένη δραστηριότητα .NET. Στη συνέχεια, μπορείτε να αποκτήσετε πρόσβαση στην υπηρεσία συνδεδεμένων συμβολοσειρά σύνδεσης στον κώδικα προσαρμοσμένη δραστηριότητα και θα πρέπει να λειτουργεί κανονικά κατά το χρόνο εκτέλεσης.  

#### <a name="extend-the-sample"></a>Επέκταση του δείγματος

Μπορείτε να επεκτείνετε αυτό το δείγμα για να μάθετε περισσότερα σχετικά με τις δυνατότητες εργοστασίου δεδομένων Azure και δέσμη Azure. Για παράδειγμα, για να επεξεργαστείτε τις φέτες σε διαφορετικό χρονικό διάστημα, κάντε τα εξής βήματα:

1.  Προσθέστε τους παρακάτω υποφακέλους στο το **inputfolder**: 2015-11-16-05, 2015-11-16-06, 201-11-16-07, 2011-11-16-08, 2015-11-16-09 και σημείο εισαγωγής αρχείων σε αυτούς τους φακέλους. Αλλάξτε την ώρα λήξης για τη διαδικασία από `2015-11-16T05:00:00Z` να `2015-11-16T10:00:00Z`. Στην **Προβολή "Διάγραμμα"**, κάντε διπλό κλικ το **InputDataset**και επιβεβαιώστε ότι είναι έτοιμες τις φέτες του εισαγωγής. Κάντε διπλό κλικ **OuptutDataset** για να δείτε την κατάσταση των φετών εξόδου. Εάν είναι σε κατάσταση είστε έτοιμοι, ελέγξτε το outputfolder για τα αρχεία εξόδου.

2.  Αυξήστε ή μειώστε τη ρύθμιση **ταυτόχρονης εκτέλεσης** για να κατανοήσετε τον τρόπο αυτό επηρεάζει τις επιδόσεις του λύση, ιδίως της επεξεργασίας που παρουσιάζεται στο Azure δέσμης. (Ανατρέξτε στο βήμα 4: δημιουργία και εκτέλεση της διοχέτευσης για περισσότερες πληροφορίες σχετικά με τη ρύθμιση **ταυτόχρονης εκτέλεσης** .)

3.  Δημιουργία χώρου συγκέντρωσης με υψηλότερη/κάτω **μέγιστη εργασίες ανά Εικονική**. Για να χρησιμοποιήσετε το νέο σύνολο που δημιουργήσατε, ενημερώστε την υπηρεσία δέσμη Azure που συνδέεται στη λύση εργοστασίου δεδομένων. (Ανατρέξτε στο βήμα 4: δημιουργία και εκτέλεση της διοχέτευσης για περισσότερες πληροφορίες σχετικά με τη ρύθμιση **μέγιστη εργασίες ανά Εικονική** .)

4.  Δημιουργία ενός χώρου συγκέντρωσης δέσμη Azure **autoscale** τη δυνατότητα. Αυτόματη κλιμάκωση κόμβους υπολογιστικών σε ένα σύνολο Azure δέσμη είναι η δυναμική Προσαρμογή επεξεργασίας power που χρησιμοποιούνται από την εφαρμογή σας. Για παράδειγμα, ενδέχεται να μπορείτε να δημιουργήσετε ένα χώρο συγκέντρωσης azure δέσμη με 0 αποκλειστικό ΣΠΣ και έναν τύπο autoscale με βάση τον αριθμό των εκκρεμών εργασιών:
 
    Μια Εικονική ανά εκκρεμεί εργασία κάθε φορά (για παράδειγμα: Δώστε εκκρεμών εργασιών -> πέντε ΣΠΣ):

        pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
        $TargetDedicated = max(pendingTaskSampleVector);

    Μέγιστος αριθμός Εικονική μία κάθε φορά ανεξάρτητα από τον αριθμό των εκκρεμών εργασιών:

        pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
        $TargetDedicated = (max(pendingTaskSampleVector)>0)?1:0;

    Για λεπτομέρειες, ανατρέξτε στο θέμα [Αυτόματη κλίμακα τον υπολογισμό κόμβοι σε ένα σύνολο Azure δέσμης](../batch/batch-automatic-scaling.md) . 

    Εάν το χώρο συγκέντρωσης χρησιμοποιεί την προεπιλεγμένη [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), την υπηρεσία δέσμη μπορεί να διαρκέσει 15-30 λεπτά για να προετοιμάσετε το Εικονική πριν από την εκτέλεση της προσαρμοσμένης δραστηριότητας.  Εάν το χώρο συγκέντρωσης χρησιμοποιεί μια διαφορετική autoScaleEvaluationInterval, την υπηρεσία δέσμη μπορεί να διαρκέσει autoScaleEvaluationInterval + 10 λεπτά. 
     
5. Στο δείγμα λύση, η μέθοδος **Execute** καλεί τη μέθοδο **Υπολογισμός** που επεξεργάζεται μια φέτα δεδομένα εισόδου για την παραγωγή μια φέτα δεδομένων εξόδου. Μπορείτε να συντάξετε τη δική σας μέθοδο για την επεξεργασία δεδομένων εισόδου και αντικαταστήστε την κλήση μεθόδου υπολογισμός στη μέθοδο Execute με μια κλήση προς τη μέθοδο.

 


### <a name="next-steps-consume-the-data"></a>Επόμενα βήματα: εκμετάλλευση των δεδομένων

Μετά την επεξεργασία δεδομένων, μπορείτε να την κατανάλωση με ηλεκτρονικά εργαλεία όπως **Το Microsoft Power BI**. Εδώ θα βρείτε συνδέσεις για να σας βοηθήσει να κατανοήσετε Power BI και το πώς μπορείτε να το χρησιμοποιήσετε στο Azure:

-   [Εξερεύνηση ένα σύνολο δεδομένων στο Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-data/)

-   [Γρήγορα αποτελέσματα με το Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/)

-   [Ανανέωση δεδομένων στο Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-refresh-data/)

-   [Azure και το Power BI - βασικές Επισκόπηση](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Αναφορές

-   [Εργοστασιακές Azure δεδομένων](https://azure.microsoft.com/documentation/services/data-factory/)

    -   [Εισαγωγή στην υπηρεσία εργοστασίου δεδομένων Azure](data-factory-introduction.md)

    -   [Γρήγορα αποτελέσματα με το Azure εργοστασίου δεδομένων](data-factory-build-your-first-pipeline.md)

    -   [Χρησιμοποιήστε προσαρμοσμένες δραστηριότητες σε μια διαδικασία εργοστασίου δεδομένων Azure](data-factory-use-custom-activities.md)

-   [Azure δέσμης](https://azure.microsoft.com/documentation/services/batch/)

    -   [Βασικά στοιχεία της δέσμης Azure](../batch/batch-technical-overview.md)

    -   [Επισκόπηση των δυνατοτήτων δέσμης Azure](../batch/batch-api-basics.md)

    -   [Δημιουργία και διαχείριση λογαριασμού δέσμη Azure στην πύλη του Azure](../batch/batch-account-create-portal.md)

    -   [Γρήγορα αποτελέσματα με το .NET βιβλιοθήκη δέσμη Azure](../batch/batch-dotnet-get-started.md)


[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx

