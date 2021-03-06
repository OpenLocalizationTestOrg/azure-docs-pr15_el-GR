Ο παρακάτω πίνακας παραθέτει τα όρια που σχετίζονται με τις βαθμίδες διαφορετική υπηρεσία (S1, S2, S3, F1). Για πληροφορίες σχετικά με το κόστος κάθε *μονάδας* σε κάθε σειρά, ανατρέξτε στο θέμα [Διανομέα IoT τις πληροφορίες τιμολόγησης](https://azure.microsoft.com/pricing/details/iot-hub/).

| Πόρων | Τυπική S1 | Τυπική s2 | Τυπική S3 | Δωρεάν F1 |
| -------- | ----------- | ----------- | ----------- | ------- |
| Μηνύματα/ημέρα | 400,000 | 6,000,000   | 300,000,000 | 8.000   |
| Μέγιστες μονάδες | 200    | 200         | 200         | 1       |

> [AZURE.NOTE] Εάν σκοπεύετε να χρησιμοποιείτε περισσότερα από 200 μονάδες με ένα διανομέα επίπεδο S1 ή S2 ή S3, επικοινωνήστε με την υποστήριξη της Microsoft.

Ο παρακάτω πίνακας παραθέτει τα όρια που ισχύουν για τους πόρους IoT διανομέα:

| Πόρων | Όριο |
| -------- | ----- |
| Μέγιστη που καταβλήθηκε διανομείς IoT ανά Azure συνδρομή | 10 |
| Μέγιστο δωρεάν διανομείς IoT ανά Azure συνδρομή | 1 |
| Μέγιστος αριθμός των ταυτοτήτων συσκευή<br/>  επιστρέφεται σε μία μόνο κλήση | 1000 |
| Ενότητα IoT μήνυμα μέγιστο διατήρησης για συσκευή-cloud μηνύματα | 7 ημέρες |
| Μέγιστο μέγεθος του μηνύματος συσκευής στο cloud | 256 KB |
| Μέγιστο μέγεθος της δέσμης συσκευής στο cloud | 256 KB |
| Μέγιστος αριθμός μηνυμάτων σε δέσμη συσκευής στο cloud | 500 |
| Μέγιστο μέγεθος του μηνύματος cloud-σε-συσκευή | 64 KB |
| Μέγιστη τιμή TTL για μηνύματα cloud-σε-συσκευή | 2 ημέρες |
| Πλήθος μέγιστο παράδοσης για cloud-σε-συσκευή <br/> τα μηνύματα | 100 |
| Καταμέτρηση μέγιστο παράδοσης για μηνύματα σχολίων <br/> απάντηση σε μήνυμα cloud-σε-συσκευή | 100 |
| Μέγιστη τιμή TTL για μηνύματα σχολίων σε <br/> απάντηση σε μήνυμα cloud-σε-συσκευή | 2 ημέρες |

> [AZURE.NOTE] Εάν χρειάζεστε περισσότερους από 10 επί πληρωμή διανομείς IoT σε μια συνδρομή του Azure, επικοινωνήστε με την υποστήριξη της Microsoft.

Η υπηρεσία διανομέα IoT throttles αιτήσεις όταν γίνει υπέρβαση του παρακάτω ορίων:

| Επιτάχυνση | Τιμή ανά διανομέα |
| -------- | ------------- |
| Λειτουργίες μητρώου ταυτότητας <br/> (δημιουργία, ανάκτηση, λίστα, ενημέρωση, διαγραφή), <br/> μεμονωμένα ή μαζικής εισαγωγής/εξαγωγής | 5000/min/μονάδας (S3) <br/> 100/min/μονάδας (S1 και S2). |
| Οι συνδέσεις των συσκευών | 6000/sec/μονάδας (S3), 120/sec/μονάδας (S2), δευτερόλεπτα/12/μονάδα (για S1). <br/>Τουλάχιστον 100/sec. |
| Στέλνει συσκευής στο cloud | 6000/sec/μονάδας (S3), 120/sec/μονάδας (S2), δευτερόλεπτα/12/μονάδα (για S1). <br/>Τουλάχιστον 100/sec. |
| Στέλνει cloud-σε-συσκευή | 5000/min/μονάδας (S3), 100/min/μονάδας (S1 και S2). |
| Λαμβάνει cloud-σε-συσκευή | 50000/min/μονάδας (S3), 1000/min/μονάδας (S1 και S2). |
| Λειτουργίες αποστολής αρχείων | αρχείο 5000 αποστολή ειδοποιήσεων/min/μονάδας (για S3), 100 αρχείο αποστολή ειδοποιήσεων/min/μονάδα (για S1 και S2). <br/> 10000 URIs συσχετισμών Ασφαλείας μπορεί να είναι εκτός για ένα λογαριασμό αποθήκευσης Azure κάθε φορά.<br/> 10 συσχετισμούς Ασφαλείας URIs/συσκευή μπορεί να είναι εκτός κάθε φορά. |
