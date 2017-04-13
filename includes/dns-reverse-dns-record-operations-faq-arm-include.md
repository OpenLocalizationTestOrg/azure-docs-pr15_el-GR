<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Συνήθεις Ερωτήσεις - αντίστροφη DNS για τη διεύθυνση IP που έχει εκχωρηθεί Azure

### <a name="how-much-do-reverse-dns-records-cost"></a>Πόσο αντίστροφη κόστος εγγραφές DNS;
Είναι δωρεάν!  Δεν υπάρχει χωρίς πρόσθετο κόστος για αντίστροφη τις εγγραφές DNS ή ερωτήματα.

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>Επιλύει τις αντίστροφη εγγραφές DNS για το Azure ανατεθεί δημόσια διεύθυνση IP από το internet;
Ναι. Αφού ορίσετε την ιδιότητα αντίστροφη DNS για τη δημόσια διεύθυνση IP, Azure διαχειρίζεται όλες τις αναθέσεις DNS και τις ζώνες DNS που απαιτούνται για να βεβαιωθείτε ότι επιλύει αντίστροφη εγγραφή DNS για όλους τους χρήστες στο internet.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>Θα δημιουργηθεί μια προεπιλεγμένη αντίστροφη εγγραφή DNS για τις δημόσιες μου διευθύνσεις IP;
Όχι. Αντίστροφη DNS είναι μια δυνατότητα συμμετοχής στο. Δεν υπάρχει προεπιλεγμένη αντίστροφη εγγραφές DNS δημιουργούνται εάν επιλέξετε να μην ρυθμίσετε τις παραμέτρους τους.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Ποια είναι η μορφή για το όνομα πλήρως προσδιορισμένο τομέα (FQDN);
FQDN καθορίζονται με τη σειρά προς τα εμπρός και πρέπει να τελειώνει με τελεία (π.χ., "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Τι συμβαίνει εάν αποτύχει τους ελέγχους επικύρωσης για την αντίστροφη DNS που έχετε καθορίσει;
Όπου η επικύρωση για αντίστροφη DNS ελέγχει αποτύχει, θα αποτύχει στη λειτουργία της υπηρεσίας διαχείρισης. Διορθώστε την αντίστροφη τιμή DNS, όπως απαιτείται, και προσπαθήστε ξανά.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Μπορώ να διαχειριστώ αντίστροφη DNS για την τοποθεσία Web μου Azure;
Αντίστροφη DNS δεν υποστηρίζεται για τοποθεσίες Web Azure. Αντίστροφη DNS υποστηρίζεται για εικονικές μηχανές Windows Azure.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>Να η ρύθμιση παραμέτρων πολλών αντίστροφη εγγραφές DNS για τη δημόσια διεύθυνση IP μου;
Όχι. Azure υποστηρίζει αντίστροφη μία εγγραφή DNS για κάθε δημόσια διεύθυνση IP. Ωστόσο, κάθε δημόσια διεύθυνση IP μπορούν να έχουν τις δικές τους αντίστροφη εγγραφή DNS.

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>Να η ρύθμιση παραμέτρων αντίστροφη τις εγγραφές DNS για μια δημόσια διεύθυνση IP IPv6;
Όχι.  Προς το παρόν, αντίστροφη τις εγγραφές DNS που υποστηρίζονται για τις δημόσιες διευθύνσεις IP IPv4 μόνο.

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>Να η ρύθμιση παραμέτρων αντίστροφη εγγραφής DNS για τη δημόσια διεύθυνση IP μου χωρίς να χρειάζεται μια DomainNameLabel που καθορίζεται;
Όχι. Για να αξιοποιήσετε αντίστροφη τις εγγραφές DNS για τις δημόσιες διευθύνσεις IP, πρέπει να καθορίσετε την ιδιότητα DomainNameLabel.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Μπορώ να στείλω μηνύματα ηλεκτρονικού ταχυδρομείου σε εξωτερικούς τομείς από τις υπηρεσίες τον υπολογισμό Azure μου;
Όχι. [Azure τον υπολογισμό υπηρεσίες δεν υποστηρίζουν την αποστολή μηνυμάτων ηλεκτρονικού ταχυδρομείου σε εξωτερικούς τομείς](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
