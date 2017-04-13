Με το Azure διαχείριση πόρων, ορίζετε παραμέτρους για τις τιμές που θέλετε να καθορίσετε πότε έχει αναπτυχθεί το πρότυπο. Το πρότυπο περιλαμβάνει μια ενότητα που ονομάζεται παράμετροι που περιέχει όλες τις τιμές παραμέτρων.
Θα πρέπει να μπορείτε να καθορίσετε μια παράμετρο για τις τιμές που θα ποικίλλουν ανάλογα με το έργο που αναπτύσσετε ή με βάση το περιβάλλον αναπτύσσετε σε. Δεν ορίσετε τις παραμέτρους για τιμές που θα παραμείνει πάντα το ίδιο. Κάθε τιμή παραμέτρου χρησιμοποιείται στο πρότυπο για να ορίσετε τους πόρους που είναι ανάπτυξη. 

Κατά τον ορισμό παραμέτρων, χρησιμοποιήστε το πεδίο **allowedValues** για να καθορίσετε τις τιμές που μπορούν να παρέχουν ένα χρήστη κατά την ανάπτυξη. Χρησιμοποιήστε το πεδίο " **προεπιλεγμένη τιμή** " για να εκχωρήσετε μια τιμή για την παράμετρο, εάν δεν υπάρχει τιμή παρέχεται κατά την ανάπτυξη.

Θα περιγραφή κάθε παράμετρο στο πρότυπο.

### <a name="logicappname"></a>logicAppName

Το όνομα της εφαρμογής λογικής για να δημιουργήσετε.

    "logicAppName": {
        "type": "string"
    }