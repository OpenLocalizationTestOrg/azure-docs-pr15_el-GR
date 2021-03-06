## <a name="repeatability-during-copy"></a>Επαναληψιμότητα κατά την αντιγραφή

Κατά την αντιγραφή δεδομένων από και προς σχεσιακές αποθηκεύει, πρέπει να έχετε επαναληψιμότητας υπόψη σας για να αποφύγετε τα ανεπιθύμητα αποτελέσματα. 

Μια φέτα μπορεί να είναι εκτελέστε ξανά αυτόματα στο Azure εργοστασίου δεδομένων σύμφωνα με την πολιτική "Επανάληψη" που καθορίζεται. Συνιστάται να ορίσετε μια πολιτική "Επανάληψη" για να προστατευτείτε από μεταβατικές αποτυχίες. Ως εκ τούτου επαναληψιμότητας είναι σημαντική πτυχή για να καλύψετε κατά την κίνησή δεδομένων. 

**Ως προέλευση:**

> [AZURE.NOTE] Στα παρακάτω παραδείγματα είναι για Azure SQL, αλλά ισχύουν για οποιαδήποτε χώρου αποθήκευσης δεδομένων που υποστηρίζει ορθογώνια συνόλων δεδομένων. Ίσως χρειαστεί να προσαρμόσετε τον **τύπο** της προέλευσης και η ιδιότητα **ερωτήματος** (για παράδειγμα: ερωτήματος αντί για sqlReaderQuery) για τα δεδομένα αποθήκευση.   

Συνήθως, κατά την ανάγνωση από σχεσιακές καταστήματα, θα θέλετε να διαβάσετε μόνο τα δεδομένα που αντιστοιχεί σε αυτήν τη φέτα. Ένας τρόπος για να το κάνετε θα ήταν χρησιμοποιώντας τις μεταβλητές WindowStart και WindowEnd είναι διαθέσιμες στο Azure εργοστασίου δεδομένων. Διαβάστε σχετικά με τις μεταβλητές και συναρτήσεων στο Azure εργοστασίου δεδομένων εδώ στο άρθρο [Προγραμματισμός και εκτέλεση](../articles/data-factory/data-factory-scheduling-and-execution.md) . Παράδειγμα: 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

Αυτό το ερώτημα διαβάζει δεδομένα από 'Πίνακας' που βρίσκεται στην περιοχή φέτα διάρκεια. Επανεκτέλεση αυτό φέτας επίσης πάντα να διασφαλίζουν αυτήν τη συμπεριφορά. 

Σε άλλες περιπτώσεις, ίσως θελήσετε να διαβάσετε ολόκληρο τον πίνακα (ας υποθέσουμε ότι για μετακίνηση μόνο μία φορά) και μπορεί να ορίσει τα sqlReaderQuery ως εξής:

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
