Του συστήματος ονομάτων τομέα (DNS) χρησιμοποιείται για να εντοπίσετε στοιχεία στο internet. Για παράδειγμα, όταν εισάγετε μια διεύθυνση στο πρόγραμμα περιήγησης, ή κάντε κλικ σε μια σύνδεση σε μια ιστοσελίδα, χρησιμοποιεί DNS για να μεταφράσετε τον τομέα σε μια διεύθυνση IP. Η διεύθυνση IP είναι sort των όπως μια διεύθυνση, αλλά δεν είναι πολύ human φιλικούς προς. Για παράδειγμα, είναι πιο εύκολο να θυμάστε ένα όνομα DNS όπως **contoso.com** από ό, τι να θυμάστε μια διεύθυνση IP όπως 192.168.1.88 ή 2001:0:4137:1f67:24a2:3888:9cce:fea3.

Το σύστημα DNS βασίζεται σε *εγγραφές*. Εγγραφές συσχετίσετε ένα συγκεκριμένο *όνομα*, όπως **contoso.com**, με μια διεύθυνση IP ή ένα άλλο όνομα DNS. Όταν μια εφαρμογή, όπως ένα πρόγραμμα περιήγησης web, αναζητά ένα όνομα στο DNS, εντοπίζει την εγγραφή και χρησιμοποιεί ό, τι να δείχνει ως τη διεύθυνση. Εάν η τιμή που να οδηγεί στο είναι μια διεύθυνση IP, το πρόγραμμα περιήγησης θα χρησιμοποιήσει αυτήν την τιμή. Εάν να οδηγεί στο άλλο όνομα DNS, στη συνέχεια, την εφαρμογή διαθέτει για να το κάνετε ανάλυση ξανά. Ο τελικός όλα επίλυση ονομάτων θα τελειώνουν σε μια διεύθυνση IP.

Όταν δημιουργείτε μια τοποθεσία Web του Azure, ένα όνομα DNS εκχωρείται αυτόματα στην τοποθεσία. Αυτό το όνομα έχει τη μορφή ** &lt;yoursitename&gt;. azurewebsites.net**. Κατά την προσθήκη της τοποθεσίας Web ως τελικού σημείου Azure κίνηση Manager, η τοποθεσία Web, στη συνέχεια, είναι προσβάσιμα μέσω του ** &lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** τομέα.

> [AZURE.NOTE] Όταν η τοποθεσία Web σας έχει ρυθμιστεί ως ένα τελικό σημείο Manager κίνηση, χρησιμοποιήστε το **. trafficmanager.net** διεύθυνση κατά τη δημιουργία εγγραφών DNS.

> Μπορείτε να χρησιμοποιήσετε μόνο τις εγγραφές CNAME με τη Διαχείριση κίνηση

Υπάρχουν επίσης πολλούς τύπους εγγραφών, κάθε μία με τις δικές τους συναρτήσεις και τους περιορισμούς, αλλά για τοποθεσίες Web που έχει ρυθμιστεί ώστε να ως τελικά σημεία Manager κίνηση, θα σας μόνο φροντίσει σχετικά με ένα; Εγγραφές *CNAME* .

###<a name="cname-or-alias-record"></a>Εγγραφή CNAME ή ψευδώνυμο

Μια εγγραφή CNAME αντιστοιχίζει ένα *συγκεκριμένο* όνομα DNS, όπως **mail.contoso.com** ή **www.contoso.com**, σε ένα άλλο όνομα τομέα (κανονική). Στην περίπτωση Azure τοποθεσίες Web χρησιμοποιώντας τη Διαχείριση κυκλοφορία, το όνομα τομέα κανονικής είναι το ** &lt;myapp >. trafficmanager.net** όνομα του τομέα σας προφίλ διαχείρισης κυκλοφορία. Μετά τη δημιουργία, την εγγραφή CNAME δημιουργεί ένα ψευδώνυμο για το ** &lt;myapp >. trafficmanager.net** το όνομα του τομέα. Η εγγραφή CNAME επιλύει για τη διεύθυνση IP του σας ** &lt;myapp >. trafficmanager.net** ονόματος τομέα αυτόματα, ώστε να εάν αλλάξει τη διεύθυνση IP της τοποθεσίας Web, δεν χρειάζεται να κάνετε κάποια ενέργεια.

Μόλις κίνηση φτάσει στο Manager κίνηση, στη συνέχεια, δρομολογεί την κίνηση στην τοποθεσία Web σας, χρησιμοποιώντας το έχει ρυθμιστεί για τη μέθοδο εξισορρόπησης φόρτου. Αυτό είναι απόλυτα διαφανής στους επισκέπτες της τοποθεσίας Web σας. Θα βλέπουν μόνο το προσαρμοσμένο όνομα τομέα στο πρόγραμμα περιήγησης.

> [AZURE.NOTE] Ορισμένες μητρώα καταχώρησης ονομάτων τομέα μόνο σάς επιτρέπουν να αντιστοιχίσετε δευτερεύοντες τομείς, όταν χρησιμοποιείτε μια εγγραφή CNAME, όπως **www.contoso.com**και δεν ριζικό κατάλογο ονομάτων, όπως **contoso.com**. Για περισσότερες πληροφορίες σχετικά με τις εγγραφές CNAME, ανατρέξτε στην τεκμηρίωση που παρέχεται από το μητρώο καταχώρησης ονομάτων, <a href="http://en.wikipedia.org/wiki/CNAME_record">την καταχώρηση Wikipedia στην εγγραφή CNAME</a>ή το έγγραφο <a href="http://tools.ietf.org/html/rfc1035">IETF ονόματα τομέων – εφαρμογή και προδιαγραφές</a> .
