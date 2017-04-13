Azure εργοστασίου δεδομένων υποστηρίζει τις ακόλουθες μετασχηματισμό δραστηριότητες που μπορούν να προστεθούν σε αγωγούς είτε μεμονωμένα ή συνδεδεμένες με μια άλλη δραστηριότητα.

Δραστηριότητα μετασχηματισμού δεδομένων |  Τον υπολογισμό περιβάλλον 
:----------------------- | :--------------------
[Ομάδα](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Γουρούνι](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop ροής](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Πόρος εκμάθησης δραστηριότητες: εκτέλεση δέσμης και ενημέρωση πόρων](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Εικονική Azure 
[Αποθηκευμένη διαδικασία](../articles/data-factory/data-factory-stored-proc-activity.md) | Azure SQL, αποθήκη δεδομένων του Azure SQL ή SQL Server |
[U-SQL λίμνης ανάλυση δεδομένων](../articles/data-factory/data-factory-usql-activity.md) | Ανάλυση λίμνης Azure δεδομένων 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | HDInsight [Hadoop] ή Azure δέσμης
   
> [AZURE.NOTE] 
> Μπορείτε να χρησιμοποιήσετε MapReduce δραστηριότητας για να εκτελέσετε τους προγράμματα σε το σύμπλεγμά σας HDInsight τους. Για λεπτομέρειες, ανατρέξτε στο θέμα [προγράμματα κλήση τους από το Azure εργοστασίου δεδομένων](../articles/data-factory/data-factory-spark.md) .
> Μπορείτε να δημιουργήσετε μια προσαρμοσμένη δραστηριότητα για να εκτελούν δέσμες ενεργειών R σε το σύμπλεγμά σας HDInsight με R εγκατεστημένο. Ανατρέξτε στο θέμα [Εκτέλεση R δέσμη ενεργειών που χρησιμοποιεί Azure εργοστασίου δεδομένων](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).