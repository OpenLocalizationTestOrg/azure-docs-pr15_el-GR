<properties 
    pageTitle="Παρακολούθηση και διαχείριση αγωγούς εργοστασίου δεδομένων Azure" 
    description="Μάθετε πώς να χρησιμοποιείτε Azure πύλη και Azure PowerShell για την παρακολούθηση και διαχείριση εργοστάσια Azure δεδομένων και αγωγών που έχετε δημιουργήσει." 
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
    ms.date="09/06/2016" 
    ms.author="spelluru"/>


# <a name="monitor-and-manage-azure-data-factory-pipelines"></a>Παρακολούθηση και διαχείριση αγωγούς εργοστασίου δεδομένων Azure
> [AZURE.SELECTOR]
- [Χρήση του Azure πύλη/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
- [Χρήση παρακολούθησης και διαχείρισης εφαρμογών](data-factory-monitor-manage-app.md)

Η υπηρεσία εργοστασίου δεδομένων παρέχει αξιόπιστη και πλήρη προβολή των υπηρεσιών σας κίνηση χώρου αποθήκευσης, επεξεργασίας και δεδομένων. Η υπηρεσία παρέχει ένα παρακολούθησης συμβάλλει πίνακα εργαλείων που μπορείτε να χρησιμοποιήσετε για να εκτελέσετε τις ακόλουθες εργασίες: 

- Αξιολογήστε γρήγορα εύρυθμης λειτουργίας διοχέτευσης σε ολοκληρωμένες δεδομένων.
- Αναγνώριση προβλημάτων και αναλάβετε διορθωτικές ενέργειες, εάν είναι απαραίτητο. 
- Παρακολούθηση δεδομένων καταγωγής. 
- Παρακολουθήστε τις σχέσεις μεταξύ των δεδομένων σας σε οποιαδήποτε από τις πηγές σας.
- Προβολή ιστορικού πλήρη λογιστική εργασία εκτέλεσης, εύρυθμης λειτουργίας του συστήματος και εξαρτήσεις.

Σε αυτό το άρθρο περιγράφει τον τρόπο για την παρακολούθηση, διαχείριση και εντοπισμός σφαλμάτων αγωγούς σας. Παρέχει επίσης πληροφορίες σχετικά με τη δημιουργία ειδοποιήσεων και να ενημερώνεστε σε αποτυχίες.

## <a name="understand-pipelines-and-activity-states"></a>Κατανόηση των αγωγούς και μέλη δραστηριότητας
Χρησιμοποιώντας την πύλη του Azure, μπορείτε να:

- Προβάλετε τις εργοστασιακές δεδομένων ως ένα διάγραμμα
- Προβολή δραστηριοτήτων σε μια διαδικασία
- Προβολή συνόλων δεδομένων εισόδου και εξόδου
- και πολλά άλλα. 

Αυτή η ενότητα παρέχει επίσης πώς ένα κομμάτι μεταβάσεις από το ένα μέλος σε άλλο μέλος.   

### <a name="navigate-to-your-data-factory"></a>Μεταβείτε στο εργοστασίου δεδομένων σας
1.  Είσοδος στην [πύλη του Azure](https://portal.azure.com).
2.  Στο μενού στα αριστερά, κάντε κλικ στην επιλογή **εργοστάσια δεδομένων** . Εάν δεν βλέπετε το, κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες >** και κάντε κλικ στο **εργοστάσια δεδομένων** στην κατηγορία **ΠΛΗΡΟΦΟΡΙΏΝ + ΑΝΆΛΥΣΗΣ** . 

    ![Εντοπίστε όλα -> εργοστάσια δεδομένων](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)

    Θα πρέπει να δείτε όλα τα κλάσεων δεδομένων το blade **εργοστάσια δεδομένων** . 
4. Στο blade εργοστάσια τα δεδομένα, επιλέξτε την προέλευση δεδομένων που σας ενδιαφέρει.

    ![Επιλέξτε προέλευση δεδομένων](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)  
5.  και θα πρέπει να δείτε την αρχική σελίδα (**εργοστασίου δεδομένων** blade) για την προέλευση δεδομένων.

    ![Blade εργοστασίου δεδομένων](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Προβολή διαγράμματος του εργοστασίου δεδομένων σας
Προβολή διαγράμματος ενός εργοστασίου δεδομένων παρέχει ένα τμήμα παραθύρου από γυαλί για την παρακολούθηση και διαχείριση την προέλευση δεδομένων και τα στοιχεία.

Για να δείτε την προβολή διαγράμματος του εργοστασίου σας δεδομένα, κάντε κλικ στην επιλογή **διαγράμματος** στην αρχική σελίδα του εργοστασίου δεδομένων.

![Προβολή διαγράμματος](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Μπορείτε να κάνετε μεγέθυνση, σμίκρυνση, ζουμ για να προσαρμοστεί, ζουμ στο 100%, κλειδώστε τη διάταξη του διαγράμματος και ρυθμίζει αυτόματα τη θέση αγωγούς και πίνακες. Μπορείτε επίσης να δείτε τις πληροφορίες καταγωγής δεδομένων (Εμφάνιση στοιχείων νέα και μετάδοση των επιλεγμένων στοιχείων).
 

### <a name="activities-inside-a-pipeline"></a>Δραστηριότητες μέσα σε μια διαδικασία 
1. Κάντε δεξί κλικ της διοχέτευσης και κάντε κλικ στην επιλογή **Άνοιγμα διοχέτευσης** για να δείτε όλες τις δραστηριότητες στη διοχέτευση μαζί με σύνολα δεδομένων εισόδου και εξόδου για τις δραστηριότητες. Αυτή η δυνατότητα είναι χρήσιμη όταν σας διοχέτευσης αποτελείται από περισσότερες από μία δραστηριότητας και θέλετε να κατανοήσετε το λειτουργικές καταγωγής από μια μεμονωμένη σωλήνωση.

    ![Άνοιγμα διοχέτευσης μενού](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)  
2. Στο παρακάτω παράδειγμα, μπορείτε να δείτε δύο δραστηριότητες στη διοχέτευση με τους εισροές και εκροές. Η δραστηριότητα με τίτλο **JoinData** του τύπου δραστηριότητας Hive HDInsight και **EgressDataAzure** του τύπου δραστηριότητας αντίγραφο είναι σε αυτό διοχέτευσης δείγμα. 
    
    ![Δραστηριότητες μέσα σε μια διαδικασία](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png) 
3. Μπορείτε να περιηγηθείτε Επιστροφή στην αρχική σελίδα του εργοστασίου δεδομένων, κάνοντας κλικ στην επιλογή σύνδεση εργοστασίου δεδομένων στην πορεία στην επάνω αριστερή γωνία.

    ![Επιστρέψετε σε μια προέλευση δεδομένων](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-state-of-each-activity-inside-a-pipeline"></a>Κατάσταση προβολής του κάθε δραστηριότητα μέσα σε μια διαδικασία
Μπορείτε να προβάλετε την τρέχουσα κατάσταση μιας δραστηριότητας με την προβολή της κατάστασης των οποιοδήποτε από τα σύνολα δεδομένων που δημιουργήθηκαν με τη δραστηριότητα. 

Για παράδειγμα: στο παρακάτω παράδειγμα, το **BlobPartitionHiveActivity** εκτελέσατε με επιτυχία και παραχθεί ένα σύνολο δεδομένων με το όνομα **PartitionedProductsUsageTable**, που βρίσκεται σε κατάσταση **είστε έτοιμοι** .

![Πολιτεία της διοχέτευσης](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

Κάντε διπλό κλικ το **PartitionedProductsUsageTable** στην προβολή διαγράμματος σάς δείχνουν όλες τις φέτες που παράγονται από διαφορετικό δραστηριότητας εκτελείται μέσα σε μια διαδικασία. Μπορείτε να δείτε ότι το **BlobPartitionHiveActivity** εκτελέστηκε με επιτυχία κάθε μήνα για τελευταία οκτώ μήνες και παραχθεί τις φέτες σε **έτοιμη** κατάσταση.

Το σύνολο δεδομένων φέτες σε εργοστασίου δεδομένων μπορεί να έχει μία από τις εξής καταστάσεις:

<table>
<tr>
    <th align="left">Κατάσταση</th><th align="left">Substate</th><th align="left">Περιγραφή</th>
</tr>
<tr>
    <td rowspan="8">Αναμονή</td><td>ScheduleTime</td><td>Δεν έχει προέρχονται την ώρα για τη φέτα για να εκτελέσετε.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Η επόμενη εξαρτήσεις δεν είναι έτοιμοι.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Οι πόροι υπολογισμού δεν είναι διαθέσιμη.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Όλων των εμφανίσεων δραστηριότητας είναι απασχολημένο εκτελεί άλλες φέτες.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Δραστηριότητα βρίσκεται σε παύση και δεν είναι δυνατό να εκτελέσετε τις φέτες μέχρι το επαναληφθεί.</td>
</tr>
<tr>
<td>"Επανάληψη"</td><td>Επανάληψη εκτέλεσης δραστηριότητα.</td>
</tr>
<tr>
<td>Επικύρωση</td><td>Επικύρωση δεν έχει αρχίσει ακόμα.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Αναμονή για να επαναληφθεί η επικύρωση.</td>
</tr>
<tr>
<tr
<td rowspan="2">"Σε εξέλιξη"</td><td>Επικύρωση</td><td>Επικύρωση σε εξέλιξη.</td>
</tr>
<td></td>
<td>Στη φέτα υποβάλλεται σε επεξεργασία.</td>
</tr>
<tr>
<td rowspan="4">Απέτυχε η</td><td>Λήξη χρόνου ορίου</td><td>Εκτέλεση διαρκεί περισσότερο από το που επιτρέπεται από τη δραστηριότητα.</td>
</tr>
<tr>
<td>Ακυρώθηκε</td><td>Ακυρώθηκε από ενέργεια χρήστη.</td>
</tr>
<tr>
<td>Επικύρωση</td><td>Η επικύρωση απέτυχε.</td>
</tr>
<tr>
<td></td><td>Απέτυχε η δημιουργία ή/και να επικυρώσετε τη φέτα.</td>
</tr>
<td>Είστε έτοιμοι</td><td></td><td>Στη φέτα είναι έτοιμο για κατανάλωση.</td>
</tr>
<tr>
<td>Παράλειψη</td><td></td><td>Στη φέτα δεν υποβάλλεται σε επεξεργασία.</td>
</tr>
<tr>
<td>Κανένας</td><td></td><td>Μια φέτα που χρησιμοποιείται για να υπάρχει με διαφορετική κατάσταση, αλλά έχει γίνει επαναφορά.</td>
</tr>
</table>



Μπορείτε να προβάλετε τις λεπτομέρειες σχετικά με ένα κομμάτι, κάνοντας κλικ σε μια καταχώρηση φέτα στο το blade **Πρόσφατα φέτες ενημερώνονται** .

![Λεπτομέρειες φέτα](./media/data-factory-monitor-manage-pipelines/slice-details.png)
 
Εάν στη φέτα έχει εκτελεστεί πολλές φορές, θα δείτε πολλές γραμμές στη λίστα **εκτελείται δραστηριότητας** . Μπορείτε να προβάλετε λεπτομέρειες σχετικά με μια δραστηριότητα εκτέλεση κάνοντας κλικ στην επιλογή Εκτέλεση καταχώρησης στη λίστα **εκτελείται δραστηριότητα** . Η λίστα εμφανίζει όλα τα αρχεία καταγραφής μαζί με ένα μήνυμα σφάλματος εάν υπάρχει. Αυτή η δυνατότητα είναι χρήσιμη για προβολή και αρχεία καταγραφής εντοπισμού σφαλμάτων χωρίς να χρειάζεται να κλείσετε την προέλευση δεδομένων.

![Εκτέλεση λεπτομέρειες δραστηριότητας](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Εάν στη φέτα δεν είναι σε κατάσταση **είστε έτοιμοι** , μπορείτε να δείτε την επόμενη φέτες που δεν είναι έτοιμα και εμποδίζουν την τρέχουσα φέτα από την εκτέλεση στη λίστα **νέα φέτες που είναι δεν είναι έτοιμο** . Αυτή η δυνατότητα είναι χρήσιμη όταν φέτα σας βρίσκεται σε κατάσταση **Αναμονή** και θέλετε να κατανοήσετε τις εξαρτήσεις νέα στην οποία στη φέτα είναι σε αναμονή.

![Νέα φέτες δεν είναι έτοιμο](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>Διάγραμμα καταστάσεων συνόλου δεδομένων
Αφού αναπτύξετε μια προέλευση δεδομένων και το αγωγούς έχετε μια έγκυρη ενεργή περίοδο, του συνόλου δεδομένων χωρίζει μετάβασης από το ένα μέλος σε μια άλλη. Προς το παρόν την κατάσταση φέτα ακολουθεί το παρακάτω διάγραμμα κατάστασης:

![Διάγραμμα κατάστασης](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

Η ροή μετάβασης κατάσταση συνόλου δεδομένων στην προέλευση δεδομένων: αναμονή -> στο-προόδου/σε εξέλιξη (Validating) -> ετοιμότητα/απέτυχε

Ξεκινήστε τις φέτες σε κατάσταση **Αναμονή** για προ-συνθήκες για να τερματιστεί πριν από την εκτέλεση. Στη συνέχεια, τη δραστηριότητα αρχίζουν να εκτελούνται και στη φέτα μεταβαίνει σε κατάσταση **Σε εξέλιξη** . Την εκτέλεση δραστηριοτήτων μπορεί να ολοκληρωθεί με επιτυχία ή θα αποτύχει. Στη φέτα έχει επισημανθεί ως **έτοιμοι**' ή **απέτυχε** με βάση το αποτέλεσμα της εκτέλεσης. 

Μπορείτε να επαναφέρετε τη φέτα για να επιστρέψετε από την κατάσταση **έτοιμη** ή **απέτυχε** κατάσταση **Αναμονή** . Μπορείτε επίσης να επισημάνετε την κατάσταση φέτα σε **Παράλειψη**, που εμποδίζει την εκτέλεση της δραστηριότητας και επεξεργάζονται στη φέτα.


## <a name="manage-pipelines"></a>Διαχείριση αγωγούς
Μπορείτε να διαχειριστείτε το αγωγούς με χρήση του PowerShell Azure. Για παράδειγμα, μπορείτε να διακόψετε προσωρινά και συνέχιση αγωγούς, εκτελώντας το cmdlet του Azure PowerShell. 

### <a name="pause-and-resume-pipelines"></a>Παύση και συνέχιση αγωγούς
Μπορείτε να παύση/αναστολή αγωγούς χρησιμοποιώντας το cmdlet του Powershell **Αναστολής AzureRmDataFactoryPipeline** . Αυτό το cmdlet είναι χρήσιμη όταν δεν θέλετε να εκτελέσετε σας αγωγούς, μέχρι να επιλυθεί κάποιο πρόβλημα.

Για παράδειγμα: στην παρακάτω οθόνη στιγμιότυπο, Εντοπίστηκε ένα ζήτημα με το **PartitionProductsUsagePipeline** στο **productrecgamalbox1dev** εργοστασίου δεδομένων και θέλουμε να αναστείλει τη διαδικασία.

![Διοχέτευση αναστολή](./media/data-factory-monitor-manage-pipelines/pipeline-to-be-suspended.png)

Να αναστείλει μια διαδικασία, εκτελέστε την ακόλουθη εντολή PowerShell:

    Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Για παράδειγμα:

    Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 

Όταν το ζήτημα έχει καθοριστεί με το **PartitionProductsUsagePipeline**, είναι δυνατή η συνέχιση της σε αναστολή διοχέτευσης, εκτελέστε την παρακάτω εντολή PowerShell:

    Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Για παράδειγμα:

    Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 


## <a name="debug-pipelines"></a>Εντοπισμός σφαλμάτων αγωγούς
Azure εργοστασιακές δεδομένων παρέχει εμπλουτισμένες δυνατότητες μέσω του Azure πύλη και Azure PowerShell για να εντοπίσετε σφάλματα και αντιμετώπιση προβλημάτων αγωγούς.

### <a name="find-errors-in-a-pipeline"></a>Εύρεση σφαλμάτων σε μια διαδικασία
Εάν αποτύχει η εκτέλεση δραστηριοτήτων στο δίκτυο αγωγών, του συνόλου δεδομένων που δημιουργήθηκαν με τη διαδικασία είναι σε κατάσταση σφάλματος λόγω την αποτυχία. Μπορείτε να εντοπισμός σφαλμάτων και αντιμετώπιση σφαλμάτων στο Azure εργοστασιακές δεδομένων χρησιμοποιώντας την παρακάτω μηχανισμούς.

#### <a name="use-azure-portal-to-debug-an-error"></a>Χρησιμοποιήστε Azure πύλη για τον εντοπισμό σφαλμάτων σε ένα μήνυμα σφάλματος:

3.  Στο blade του **ΠΊΝΑΚΑ** , κάντε κλικ στη φέτα πρόβλημα με **την ΚΑΤΆΣΤΑΣΗ** οριστεί **απέτυχε**.

    ![Blade πίνακα με το πρόβλημα φέτα](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
4.  Στο το blade **ΦΈΤΑ ΔΕΔΟΜΈΝΩΝ** , επιλέξτε τη δραστηριότητα που απέτυχε η εκτέλεση.
    
    ![Dataslice με το μήνυμα σφάλματος](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
5.  Στο το blade **ΛΕΠΤΟΜΈΡΕΙΕΣ ΕΚΤΈΛΕΣΗ ΔΡΑΣΤΗΡΙΌΤΗΤΑΣ** , μπορείτε να κάνετε λήψη των αρχείων που σχετίζονται με την επεξεργασία HDInsight. Κάντε κλικ στην επιλογή λήψη για την κατάσταση/stderr για να κάνετε λήψη του αρχείου καταγραφής σφαλμάτων που περιέχει λεπτομέρειες σχετικά με το σφάλμα.

    ![Δραστηριότητα εκτέλεση λεπτομέρειες blade με το σφάλμα](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)  

#### <a name="use-the-powershell-to-debug-an-error"></a>Χρήση του PowerShell για τον εντοπισμό σφαλμάτων σε ένα μήνυμα σφάλματος
1.  Εκκινήστε το **Azure PowerShell**.
3.  Εκτελέστε την εντολή **Get-AzureRmDataFactorySlice** για να δείτε τις φέτες και τις καταστάσεις τους. Θα πρέπει να δείτε μια φέτα με την κατάσταση: **απέτυχε**.       

            Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    Για παράδειγμα:
        
            Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00

    Αντικαταστήστε **StartDateTime** με την τιμή StartDateTime που ορίσατε για το σύνολο-AzureRmDataFactoryPipelineActivePeriod.
4. Τώρα, εκτελέστε το cmdlet **Get-AzureRmDataFactoryRun** για να δείτε λεπτομέρειες σχετικά με τη δραστηριότητα εκτέλεση για τη φέτα.

        Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] 
        <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    Για παράδειγμα:

        Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"

    Η τιμή του StartDateTime είναι ώρας έναρξης στη φέτα σφάλματος/πρόβλημα σημειώσατε από το προηγούμενο βήμα. Η ημερομηνία-ώρα θα πρέπει να περικλείεται σε διπλά εισαγωγικά.
5.  Θα πρέπει να βλέπετε τα αποτελέσματα με λεπτομέρειες σχετικά με το σφάλμα (παρόμοιο με το ακόλουθο):

            Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
            ResourceGroupName       : ADF
            DataFactoryName         : LogProcessingFactory3
            TableName               : EnrichedGameEventsTable
            ProcessingStartTime     : 10/10/2014 3:04:52 AM
            ProcessingEndTime       : 10/10/2014 3:06:49 AM
            PercentComplete         : 0
            DataSliceStart          : 5/5/2014 12:00:00 AM
            DataSliceEnd            : 5/6/2014 12:00:00 AM
            Status                  : FailedExecution
            Timestamp               : 10/10/2014 3:04:52 AM
            RetryAttempt            : 0
            Properties              : {}
            ErrorMessage            : Pig script failed with exit code '5'. See wasb://     adfjobs@spestore.blob.core.windows.net/PigQuery
                                            Jobs/841b77c9-d56c-48d1-99a3-
                        8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                        more details.
            ActivityName            : PigEnrichLogs
            PipelineName            : EnrichGameLogsPipeline
            Type                    :
    
    
6.  Μπορείτε να εκτελέσετε το cmdlet **Αποθήκευση AzureRmDataFactoryLog** με τιμή αναγνωριστικό που βλέπετε από την έξοδο και κάντε λήψη των αρχείων καταγραφής χρησιμοποιώντας το **-DownloadLogsoption** για το cmdlet.

            Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"


## <a name="rerun-failures-in-a-pipeline"></a>Εκτελέστε ξανά το αποτυχίες σε μια διαδικασία

### <a name="using-azure-portal"></a>Χρήση του Azure πύλη

Μόλις αντιμετώπιση προβλημάτων και εντοπισμός σφαλμάτων αποτυχίες σε μια διαδικασία, μπορείτε να εκτελέσετε ξανά αποτυχίες περιήγηση στη φέτα σφάλματος και κάνοντας κλικ στο κουμπί **Εκτέλεση** στη γραμμή εντολών.

![Εκτελέστε ξανά μια αποτυχίας φέτα](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

Στην περίπτωση που η στη φέτα έχει η επικύρωση απέτυχε λόγω αποτυχία πολιτικής (για ex: δεδομένων δεν είναι διαθέσιμη), μπορείτε να διορθώσετε το σφάλμα και να επικυρώσετε ξανά, κάνοντας κλικ στο κουμπί **επικύρωση** στη γραμμή εντολών.
![Διόρθωση σφαλμάτων και επικύρωση](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="using-azure-powershell"></a>Χρήση του Azure PowerShell

Μπορείτε να εκτελέσετε ξανά αποτυχίες, χρησιμοποιώντας το cmdlet Set-AzureRmDataFactorySliceStatus. Ανατρέξτε στο θέμα [Ορισμός AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) για σύνταξη και άλλες λεπτομέρειες σχετικά με το cmdlet. 

**Παράδειγμα:** Το παρακάτω παράδειγμα ορίζει την κατάσταση όλων των φετών για τον πίνακα 'DAWikiAggregatedData' σε 'Αναμονή' στο το Azure δεδομένων εργοστασιακές 'WikiADF'.

Το UpdateType έχει οριστεί σε UpstreamInPipeline, γεγονός που σημαίνει ότι καταστάσεις κάθε φέτα για τον πίνακα και όλα τα εξαρτημένα πινάκων (νέα) έχει οριστεί σε "Αναμονή". Άλλες δυνατή τιμή για αυτήν την παράμετρο είναι "Άτομο".

    Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -TableName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00


## <a name="create-alerts"></a>Δημιουργία ειδοποιήσεων
Azure αρχεία καταγραφής συμβάντων χρήστη όταν ένας πόρος Azure (για παράδειγμα, ενός εργοστασίου δεδομένα) είναι δημιουργίας, ενημέρωση ή διαγραφεί. Μπορείτε να δημιουργήσετε ειδοποιήσεις σε αυτά τα συμβάντα. Προέλευση δεδομένων σάς επιτρέπει να καταγράψετε διάφορες μετρήσεις και Δημιουργία ειδοποιήσεων μετρικά. Συνιστάται να χρησιμοποιήσετε συμβάντα σε πραγματικό χρόνο παρακολούθησης και μετρήσεις για λόγους ιστορικού. 

### <a name="alerts-on-events"></a>Ειδοποιήσεις σχετικά με τα συμβάντα
Azure συμβάντα παρέχουν χρήσιμες ιδέες σε τι συμβαίνει στο Azure τους πόρους σας. Azure αρχεία καταγραφής συμβάντων χρήστη όταν ένας πόρος Azure (για παράδειγμα, ενός εργοστασίου δεδομένα) είναι δημιουργίας, ενημέρωση ή διαγραφεί. Όταν χρησιμοποιείτε την προέλευση δεδομένων Azure, συμβάντα δημιουργούνται όταν:

- Azure εργοστασίου δεδομένων είναι δημιουργήσει/ενημέρωση/διαγραφεί.
- Επεξεργασία δεδομένων (που ονομάζεται ως εκτελείται) αποτελέσματα/ολοκληρώθηκε.
- Ένα σύμπλεγμα HDInsight σε ζήτηση δημιουργείται και καταργούνται.

Μπορείτε να δημιουργήσετε ειδοποιήσεις σε αυτά τα συμβάντα χρηστών και να ρυθμίσετε τις παραμέτρους τους για την αποστολή ειδοποιήσεων ηλεκτρονικού ταχυδρομείου στο διαχειριστή και συνδιαχειριστών της συνδρομής. Επιπλέον, μπορείτε να καθορίσετε επιπλέον διευθύνσεων ηλεκτρονικού ταχυδρομείου των χρηστών που πρέπει να λαμβάνετε ειδοποιήσεις ηλεκτρονικού ταχυδρομείου όταν πληρούνται οι συνθήκες. Αυτή η δυνατότητα είναι χρήσιμη όταν θέλετε να λάβετε ειδοποιήσεις σε αποτυχίες και δεν θέλετε να παρακολουθείτε συνεχώς εργοστασίου δεδομένων σας.

> [AZURE.NOTE] Προς το παρόν, η πύλη δεν εμφανίζονται ειδοποιήσεις σχετικά με τα συμβάντα. Χρησιμοποιήστε το [παρακολούθησης και διαχείρισης εφαρμογής](data-factory-monitor-manage-app.md) για να δείτε όλες τις ειδοποιήσεις.

#### <a name="specifying-an-alert-definition"></a>Καθορισμός μιας ειδοποίησης ορισμού:
Για να καθορίσετε μια ειδοποίηση ορισμού, μπορείτε να δημιουργήσετε ένα αρχείο JSON που περιγράφει τις ενέργειες που θέλετε να λαμβάνετε ειδοποιήσεις στην. Στο παρακάτω παράδειγμα, ειδοποίηση αποστέλλει μια ειδοποίηση ηλεκτρονικού ταχυδρομείου για τη λειτουργία RunFinished. Για να είναι συγκεκριμένες, αποστέλλεται μια ειδοποίηση ηλεκτρονικού ταχυδρομείου όταν έχει ολοκληρωθεί ένα εκτέλεση στα την προέλευση δεδομένων και απέτυχε η εκτέλεση (κατάσταση = FailedExecution).

    {
        "contentVersion": "1.0.0.0",
         "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters": {},
        "resources": 
        [
            {
                "name": "ADFAlertsSlice",
                "type": "microsoft.insights/alertrules",
                "apiVersion": "2014-04-01",
                "location": "East US",
                "properties": 
                {
                    "name": "ADFAlertsSlice",
                    "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                    "isEnabled": true,
                    "condition": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                        "dataSource": 
                        {
                            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                            "operationName": "RunFinished",
                            "status": "Failed",
                            "subStatus": "FailedExecution"   
                        }
                    },
                    "action": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails": [ "<your alias>@contoso.com" ]
                    }
                }
            }
        ]
    }

Από τον ορισμό JSON, εάν δεν θέλετε να λαμβάνετε ειδοποιήσεις σχετικά με μια συγκεκριμένη αποτυχία **δευτερεύουσα κατάσταση** , μπορούν να καταργηθούν.

Αυτό το παράδειγμα ρυθμίζει την ειδοποίηση για όλα τα εργοστάσια δεδομένων στη συνδρομή σας. Εάν θέλετε την ειδοποίηση για να ορίσετε για χρήση με ένα συγκεκριμένο δεδομένων εργοστασίου, μπορείτε να καθορίσετε εργοστασίου δεδομένων **resourceUri** στο το **αρχείο προέλευσης δεδομένων**:

    "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"

Ο παρακάτω πίνακας παρέχει τη λίστα διαθέσιμες λειτουργίες και καταστάσεις (και δευτερεύουσες καταστάσεις).

Όνομα της λειτουργίας | Κατάσταση | Κατάσταση Sub
-------------- | ------ | ----------
RunStarted | Αποτελέσματα | Έναρξη
RunFinished | Απέτυχε η / ολοκληρώθηκε με επιτυχία | FailedResourceAllocation<br/><br/>Ολοκληρώθηκε με επιτυχία<br/><br/>FailedExecution<br/><br/>Λήξη χρόνου ορίου<br/><br/>< ακυρώθηκε<br/><br/>FailedValidation<br/><br/>Εγκαταλειφθούν
OnDemandClusterCreateStarted | Αποτελέσματα
OnDemandClusterCreateSuccessful | Ολοκληρώθηκε με επιτυχία
OnDemandClusterDeleted | Ολοκληρώθηκε με επιτυχία

Για λεπτομέρειες σχετικά με τα στοιχεία JSON που χρησιμοποιείται στο παράδειγμα, ανατρέξτε στο θέμα [Δημιουργία ειδοποίησης κανόνα](https://msdn.microsoft.com/library/azure/dn510366.aspx) . 

#### <a name="deploying-the-alert"></a>Ειδοποίηση για την ανάπτυξη 
Για να αναπτύξετε την ειδοποίηση, χρησιμοποιήστε το cmdlet του Azure PowerShell: **Δημιουργία AzureRmResourceGroupDeployment**, όπως φαίνεται στο ακόλουθο παράδειγμα:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  

Μόλις ολοκληρωθεί με επιτυχία η ανάπτυξη ομάδα πόρων, μπορείτε να δείτε τα ακόλουθα μηνύματα:

    VERBOSE: 7:00:48 PM - Template is valid.
    WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
    Please update scripts to remove this parameter.
    VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
    VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :

> [AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε το REST API [Δημιουργία ειδοποίησης κανόνα](https://msdn.microsoft.com/library/azure/dn510366.aspx) για να δημιουργήσετε μια ειδοποίηση κανόνα. Το φορτίο JSON είναι παρόμοιο με το παράδειγμα JSON.  

#### <a name="retrieving-the-list-of-azure-resource-group-deployments"></a>Ανάκτηση της λίστας Azure αναπτύξεις ομάδα πόρων
Για να ανακτήσετε τη λίστα των ανεπτυγμένος ομάδα πόρων Azure αναπτύξεις, χρησιμοποιήστε το cmdlet: **Get-AzureRmResourceGroupDeployment**, όπως φαίνεται στο ακόλουθο παράδειγμα:

    Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :


#### <a name="troubleshooting-user-events"></a>Αντιμετώπιση προβλημάτων συμβάντα χρηστών


1. Μπορείτε να δείτε όλα τα συμβάντα που δημιουργούνται αφού κάνετε κλικ στο πλακίδιο **Λειτουργίες και μετρήσεις** .

    ![Πλακίδιο μετρικά και λειτουργίες](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)

2. Κάντε κλικ στο πλακίδιο **συμβάντων** για να δείτε τα συμβάντα. 

    ![Πλακίδιο συμβάντα](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. Στο το blade **συμβάντα** , μπορείτε να δείτε λεπτομέρειες σχετικά με τα συμβάντα, να φιλτράρετε συμβάντα και ούτω καθεξής. 

    ![Blade συμβάντα](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. Κάντε κλικ σε μια **λειτουργία** στη λίστα εργασιών που προκαλεί σφάλμα.
    
    ![Επιλέξτε μια λειτουργία](./media/data-factory-monitor-manage-pipelines/select-operation.png) 
5. Κάντε κλικ σε ένα συμβάν **σφάλματος** για να δείτε λεπτομέρειες σχετικά με το σφάλμα.

    ![Σφάλμα συμβάντος](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)
  

Ανατρέξτε στο άρθρο [Cmdlet του Azure πληροφορίες για](https://msdn.microsoft.com/library/mt282452.aspx) το άρθρο για cmdlet του PowerShell που μπορείτε να χρησιμοποιήσετε για να πετύχετε/Προσθαφαίρεση ειδοποιήσεων. Ακολουθούν μερικά παραδείγματα χρησιμοποιώντας το cmdlet **Get-AlertRule** : 


    PS C:\> get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
        
            Properties :
            Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
            Condition   :
            DataSource :
            EventName             :
            Category              :
            Level                 :
            OperationName         : RunFinished
            ResourceGroupName     :
            ResourceProviderName  :
            ResourceId            :
            Status                : Failed
            SubStatus             : FailedExecution
            Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
            Condition   :
            Description : One or more of the data slices for the Azure Data Factory has failed processing.
            Status      : Enabled
            Name:       : ADFAlertsSlice
            Tags       :
            $type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
            Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
            Location   : West US
            Name       : ADFAlertsSlice
    
    PS C:\> Get-AlertRule -res $resourceGroup

            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
            Location   : West US
            Name       : FailedExecutionRunsWest0
    
            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
            Location   : West US
            Name       : FailedExecutionRunsWest3

    PS C:\> Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
    
            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
            Location   : West US
            Name       : FailedExecutionRunsWest0

Εκτελέστε τις ακόλουθες εντολές λήψη Βοήθειας για να δείτε λεπτομέρειες και παραδείγματα για το cmdlet Get-AlertRule. 

    get-help Get-AlertRule -detailed 
    get-help Get-AlertRule -examples


- Εάν βλέπετε τα συμβάντα ειδοποίησης γενιάς σε την πύλη blade, αλλά δεν μπορείτε να λάβετε ειδοποιήσεις ηλεκτρονικού ταχυδρομείου, ελέγξτε αν η διεύθυνση ηλεκτρονικού ταχυδρομείου που καθορίζεται έχει οριστεί να λαμβάνετε μηνύματα ηλεκτρονικού ταχυδρομείου από εξωτερικούς αποστολείς. Τα μηνύματα ηλεκτρονικού ταχυδρομείου ειδοποίησης ενδέχεται να έχει αποκλειστεί από τις ρυθμίσεις ηλεκτρονικού ταχυδρομείου σας.

### <a name="alerts-on-metrics"></a>Ειδοποιήσεις μετρικά
Προέλευση δεδομένων σάς επιτρέπει να καταγράψετε διάφορες μετρήσεις και Δημιουργία ειδοποιήσεων μετρικά. Μπορείτε να παρακολουθείτε και να δημιουργήσετε ειδοποιήσεις τα ακόλουθα μετρικά για τις φέτες του εργοστασίου δεδομένων σας.
 
- Αποτυχία εκτελείται
- Επιτυχής εκτελείται

Αυτές οι μετρήσεις είναι χρήσιμα και σας επιτρέπει να δείτε μια επισκόπηση των συνολικά απέτυχε και επιτυχής εκτελείται στο τους εργοστασίου δεδομένων. Μετρικά έχουν αποσταλεί κάθε φορά που υπάρχει ένα κομμάτι εκτέλεση. Επάνω από την ώρα, αυτές τις μετρήσεις είναι ενοποιούνται και να προωθηθεί στο λογαριασμό σας χώρου αποθήκευσης. Επομένως, για να ενεργοποιήσετε τις μετρήσεις, ρύθμιση ενός λογαριασμού χώρου αποθήκευσης.


#### <a name="enabling-metrics"></a>Ενεργοποίηση μετρικά:
Για να ενεργοποιήσετε τις μετρήσεις, κάντε κλικ στην επιλογή τα ακόλουθα από blade εργοστασίου δεδομένων:

**Παρακολούθηση** -> **μετρικό σύστημα** -> **διαγνωστικών ρυθμίσεων** -> **διαγνωστικών**

![Διαγνωστικά σύνδεσης](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

Στο το **διαγνωστικών** blade, κάντε κλικ **στο** και επιλέξτε το λογαριασμό χώρου αποθήκευσης και αποθηκεύστε.

![Διαγνωστικά blade](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

Αφού αποθηκεύσετε, ενδέχεται να χρειαστούν έως και μία ώρα για τα μετρικά να είναι ορατός στη το παρακολούθησης blade, επειδή συνάθροιση μετρικά συμβαίνει ανά ώρα.


### <a name="setting-up-alert-on-metrics"></a>Ρύθμιση ειδοποίησης σε μετρήσεις:

Κάντε κλικ στην επιλογή blade **μετρικά εργοστασιακές δεδομένων** : 

![Πλακίδιο μετρικά εργοστασίου δεδομένων](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

Στην το blade **μετρικό σύστημα** , κάντε κλικ στην επιλογή **+ Προσθήκη ειδοποίηση** στη γραμμή εργαλείων. 
![Δεδομένα εργοστασίου μετρικό blade - Προσθήκη ειδοποίησης](./media/data-factory-monitor-manage-pipelines/add-alert.png)

Στη σελίδα **Προσθήκη κανόνα ειδοποίησης** , ακολουθήστε τα παρακάτω βήματα και κάντε κλικ στο κουμπί **OK**.
 
- Πληκτρολογήστε ένα όνομα για την ειδοποίηση (παράδειγμα: Απέτυχε η ειδοποίηση).
- Πληκτρολογήστε μια περιγραφή για την ειδοποίηση (παράδειγμα: Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου όταν προκύπτει σφάλμα).
- Επιλέξτε ένα μετρικό (αποτυχίας εκτελείται έναντι επιτυχημένη εκτελείται).
- Καθορίστε μια συνθήκη και μια οριακή τιμή.   
- Καθορίστε την περίοδο. 
- Καθορίστε εάν πρέπει να σταλεί ένα μήνυμα ηλεκτρονικού ταχυδρομείου για τους κατόχους, οι συνεργάτες και οι αναγνώστες.
- και πολλά άλλα. 

![Δεδομένα εργοστασίου μετρικό blade - Προσθήκη ειδοποίησης](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

Μόλις προστεθεί ο κανόνας ειδοποίησης με επιτυχία, κλείνει το blade και δείτε την ειδοποίηση νέας στη σελίδα **μετρικό σύστημα** . 

![Δεδομένα εργοστασίου μετρικό blade - Προσθήκη ειδοποίησης](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

Μπορείτε, επίσης, θα πρέπει να δείτε τον αριθμό των ειδοποιήσεων στο πλακίδιο **ειδοποιήσεις** . Κάντε κλικ στο πλακίδιο **ειδοποιήσεις** .

![Blade μετρικό εργοστασίου δεδομένων - ειδοποίησης κανόνων](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

Στο το blade **ειδοποιήσεις** , μπορείτε να δείτε οι υπάρχουσες ειδοποιήσεις. Για να προσθέσετε μια ειδοποίηση, κάντε κλικ στην επιλογή **Προσθήκη ειδοποίηση** στη γραμμή εργαλείων.

![Blade ειδοποίησης κανόνων](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a>Οι ειδοποιήσεις:
Όταν ο κανόνας ειδοποίησης ταιριάζει με τη συνθήκη, θα πρέπει να λάβετε μήνυμα ηλεκτρονικού ταχυδρομείου ειδοποίησης ενεργοποιημένη. Όταν το ζήτημα έχει επιλυθεί και η συνθήκη ειδοποίησης δεν ταιριάζουν με οποιαδήποτε άλλα, λαμβάνετε το μήνυμα ειδοποίησης ηλεκτρονικού ταχυδρομείου επιλυθεί.

Αυτή η συμπεριφορά είναι διαφορετική από συμβάντα όπου αποστέλλεται μια ειδοποίηση σε κάθε περίπτωση αποτυχίας για ποια ειδοποίησης κανόνα ισχύει.

### <a name="deploying-alerts-using-powershell"></a>Χρήση του PowerShell ειδοποιήσεων ανάπτυξη
Μπορείτε να αναπτύξετε τις ειδοποιήσεις για μετρικά με τον ίδιο τρόπο όπως θα κάνατε για συμβάντα. 

**Ορισμός ειδοποίησης:**

    {
        "contentVersion" : "1.0.0.0",
        "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters" : {},
        "resources" : [
        {
                "name" : "FailedRunsGreaterThan5",
                "type" : "microsoft.insights/alertrules",
                "apiVersion" : "2014-04-01",
                "location" : "East US",
                "properties" : {
                    "name" : "FailedRunsGreaterThan5",
                    "description" : "Failed Runs greater than 5",
                    "isEnabled" : true,
                    "condition" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                        "dataSource" : {
                            "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                            "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                            "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
    >/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                            "metricName" : "FailedRuns"
                        },
                        "threshold" : 5.0,
                        "windowSize" : "PT3H",
                        "timeAggregation" : "Total"
                    },
                    "action" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails" : ["abhinav.gpt@live.com"]
                    }
                }
            }
        ]
    }
 
Αντικατάσταση subscriptionId, resourceGroupName και dataFactoryName στο δείγμα με τις κατάλληλες τιμές.

*metricName* από τώρα υποστηρίζει δύο τιμές:
- FailedRuns
- SuccessfulRuns

**Ειδοποίηση για την ανάπτυξη:**

Για να αναπτύξετε την ειδοποίηση, χρησιμοποιήστε το cmdlet του Azure PowerShell: **Δημιουργία AzureRmResourceGroupDeployment**, όπως φαίνεται στο ακόλουθο παράδειγμα:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json

Θα πρέπει να βλέπετε παρακολούθησης μηνυμάτων μετά την επιτυχή ανάπτυξη:

    VERBOSE: 12:52:47 PM - Template is valid.
    VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
    VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded
    
    
    DeploymentName    : FailedRunsGreaterThan5
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 7/27/2015 7:52:56 PM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           


Μπορείτε επίσης να χρησιμοποιήσετε το cmdlet **Προσθήκη AlertRule** για την ανάπτυξη μιας ειδοποίησης κανόνα. Ανατρέξτε στο θέμα [Προσθήκη AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) για λεπτομέρειες και παραδείγματα.  

## <a name="move-data-factory-to-a-different-resource-group-or-subscription"></a>Μετακίνηση δεδομένων εργοστασίου σε ομάδα διαφορετική πόρων ή συνδρομή
Μπορείτε να μετακινήσετε μια προέλευση δεδομένων σε μια ομάδα διαφορετικό πόρο ή μια διαφορετική συνδρομή, χρησιμοποιώντας το κουμπί γραμμή εντολή **Μετακίνηση** στην αρχική σελίδα του εργοστασίου δεδομένων σας. 

![Μετακίνηση δεδομένων factory](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Μπορείτε επίσης να μετακινήσετε οποιαδήποτε Σχετικοί πόροι (όπως ειδοποιήσεις που σχετίζονται με την προέλευση δεδομένων) μαζί με την προέλευση δεδομένων.

![Παράθυρο διαλόγου Μετακίνηση πόρων](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
