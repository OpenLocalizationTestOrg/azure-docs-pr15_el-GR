<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Συνήθεις Ερωτήσεις - αντίστροφη DNS για τη διεύθυνση IP που έχει εκχωρηθεί Azure

### <a name="how-much-do-reverse-dns-records-cost"></a>Πόσο αντίστροφη κόστος εγγραφές DNS;
Είναι δωρεάν!  Δεν υπάρχει χωρίς πρόσθετο κόστος για αντίστροφη εγγραφές DNS ή ερωτήματα.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Επιλύει μου αντίστροφη εγγραφών DNS από το internet;
Ναι. Αφού ορίσετε την ιδιότητα αντίστροφη DNS για την υπηρεσία Cloud, Azure διαχειρίζεται όλες τις αναθέσεις DNS και τις ζώνες DNS που απαιτούνται για να βεβαιωθείτε ότι επιλύει αντίστροφη εγγραφή DNS για όλους τους χρήστες στο internet.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-cloud-services"></a>Θα δημιουργηθεί μια προεπιλεγμένη αντίστροφη εγγραφή DNS για τις υπηρεσίες Cloud μου;
Όχι. Αντίστροφη DNS είναι μια δυνατότητα συμμετοχής στο. Δεν υπάρχει προεπιλεγμένη αντίστροφη εγγραφές DNS δημιουργούνται εάν επιλέξετε να μην ρυθμίσετε τις παραμέτρους τους.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Ποια είναι η μορφή για το όνομα πλήρως προσδιορισμένο τομέα (FQDN);
FQDN καθορίζονται με τη σειρά προς τα εμπρός και πρέπει να τελειώνει με τελεία (π.χ., "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Τι συμβαίνει εάν αποτύχει η ελέγχων επικύρωσης για την αντίστροφη DNS που έχετε καθορίσει;
Όπου η επικύρωση για αντίστροφη DNS ελέγχει αποτύχει, θα αποτύχει στη λειτουργία της υπηρεσίας διαχείρισης. Διορθώστε την αντίστροφη τιμή DNS, όπως απαιτείται, και προσπαθήστε ξανά.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Μπορώ να διαχειριστώ αντίστροφη DNS για την τοποθεσία Web μου Azure;
Αντίστροφη DNS δεν υποστηρίζεται για τοποθεσίες Web Azure. Αντίστροφη DNS υποστηρίζεται για ρόλους Azure PaaS και IaaS εικονικές μηχανές.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-cloud-service"></a>Να η ρύθμιση παραμέτρων πολλών αντίστροφη εγγραφές DNS για την υπηρεσία Cloud μου;
Όχι. Azure υποστηρίζει αντίστροφη μία εγγραφή DNS για κάθε υπηρεσία Cloud Azure. Ωστόσο, κάθε υπηρεσία Cloud Azure μπορούν να έχουν τις δικές τους αντίστροφη εγγραφής DNS.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Μπορώ να στείλω μηνύματα ηλεκτρονικού ταχυδρομείου σε εξωτερικούς τομείς από τις υπηρεσίες τον υπολογισμό Azure μου;
Όχι. [Υπηρεσίες τον υπολογισμό azure δεν υποστηρίζουν την αποστολή μηνυμάτων ηλεκτρονικού ταχυδρομείου σε εξωτερικούς τομείς](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
