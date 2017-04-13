<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>Συνήθεις Ερωτήσεις - φιλοξενίας στη ζώνη ARPA του Azure DNS

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Μπορώ να φιλοξενήσετε ARPA ζώνες για μου μπλοκ IP ISP που εκχωρούνται σε Azure DNS;
Ναι. Φιλοξενεί τις ζώνες ARPA για τη δική σας περιοχές διευθύνσεων IP στο Azure DNS υποστηρίζεται πλήρως.

Απλώς [Δημιουργήστε τη ζώνη στο Azure DNS](dns-getstarted-create-dnszone.md), στη συνέχεια, εργασία με την υπηρεσία παροχής Internet για να [αναθέσετε τη ζώνη](dns-domain-delegation.md).  Στη συνέχεια, μπορείτε να διαχειριστείτε τις εγγραφές PTR για κάθε αντίστροφης αναζήτησης με τον ίδιο τρόπο όπως άλλους τύπους εγγραφών.

Μπορείτε επίσης να [εισαγάγετε μια υπάρχουσα ζώνη αντίστροφης αναζήτησης με χρήση του Azure CLI](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Πόσο σημαίνει που φιλοξενεί το κόστος ζώνη ARPA;
Μπλοκ στο Azure DNS φιλοξενίας τη ζώνη ARPA για το IP που έχει εκχωρηθεί ISP σας θα χρεωθεί με [τυπικές χρεώσεις Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Μπορώ να φιλοξενήσετε ARPA ζώνες για διευθύνσεις IPv4 και IPv6 στο Azure DNS;
Ναι.