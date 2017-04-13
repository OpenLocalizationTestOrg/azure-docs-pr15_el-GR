<properties 
   pageTitle="Ζώνες DNS και εγγραφές | Microsoft Azure" 
   description="Επισκόπηση της υποστήριξης για τη φιλοξενία DNS ζώνες και εγγραφές στο Microsoft Azure DNS." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>Ζώνες DNS και εγγραφές

Η σελίδα αυτή εξηγεί τις βασικές έννοιες των τομέων, ζώνες DNS, και τις εγγραφές DNS και σύνολα εγγραφών και τον τρόπο που υποστηρίζονται στο Azure DNS.

## <a name="domain-names"></a>Ονόματα τομέων
 
Το σύστημα όνομα τομέα είναι μια ιεραρχία από τους τομείς. Ξεκινά την ιεραρχία από τον τομέα 'ρίζας', του οποίου το όνομα είναι απλώς '**.**'.  Κάτω από αυτό προέρχονται τομείς ανώτατου επιπέδου, όπως 'com', 'κίνηση', 'Οργανόγραμμα', 'ΗΒ' ή 'ΞΡ'.  Κάτω από αυτές είναι οι τομείς δεύτερου επιπέδου, όπως 'org.uk' ή 'co.jp'. Καθολικά διανέμονται τους τομείς στην ιεραρχία του DNS, φιλοξενούνται από τους διακομιστές ονομάτων DNS όλο τον κόσμο.

Ένα μητρώο καταχώρησης ονομάτων τομέα είναι μια εταιρεία που σας επιτρέπει να αγοράσετε ένα όνομα τομέα, όπως 'contoso.com'.  Αγορά ένα όνομα τομέα σας δίνει στα δεξιά για να ελέγξετε την ιεραρχία DNS με το συγκεκριμένο όνομα, για παράδειγμα επιτρέποντάς σας να κατευθύνετε το όνομα 'www.contoso.com' στην τοποθεσία web της εταιρείας. Το μητρώο καταχώρησης ονομάτων μπορεί να φιλοξενήσετε τομέα με τη δική τους διακομιστές ονομάτων για λογαριασμό σας, ή Εναλλακτικά, μπορείτε να καθορίσετε τους διακομιστές ονομάτων εναλλακτικές.

Azure DNS παρέχει μια υποδομή διακομιστή καθολικά κατανέμεται, υψηλής διαθεσιμότητας όνομα, το οποίο μπορείτε να χρησιμοποιήσετε για τη φιλοξενία του τομέα σας. Με φιλοξενίας τους τομείς σας στο Azure DNS, μπορείτε να διαχειριστείτε τις εγγραφές σας DNS με τα ίδια διαπιστευτήρια, APIs, εργαλεία, χρέωση και υποστήριξη ως άλλα Azure των υπηρεσιών σας.

Azure DNS δεν υποστηρίζει τη συγκεκριμένη στιγμή αγορά των ονομάτων τομέων. Εάν θέλετε να αγοράσετε ένα όνομα τομέα, πρέπει να χρησιμοποιήσετε ένα μητρώο καταχώρησης ονομάτων τομέων τρίτων κατασκευαστών. Το μητρώο καταχώρησης ονομάτων τομέων συνήθως θα σας χρεώνει μια μικρή ετήσια χρέωση. Οι τομείς, στη συνέχεια, μπορούν να φιλοξενηθούν σε Azure DNS για τη διαχείριση των εγγραφών DNS. Για λεπτομέρειες, ανατρέξτε στο θέμα [πληρεξουσίου τομέα στο Azure DNS](dns-domain-delegation.md) .

## <a name="dns-zones"></a>Ζώνες DNS 

Μια ζώνη DNS χρησιμοποιείται για τη φιλοξενία τις εγγραφές DNS για έναν συγκεκριμένο τομέα. Για να ξεκινήσετε τον τομέα σας στο Azure DNS φιλοξενίας, πρέπει να δημιουργήσετε μια ζώνη DNS για το συγκεκριμένο όνομα τομέα. Κάθε εγγραφή DNS για τον τομέα σας, στη συνέχεια, δημιουργείται μέσα σε αυτήν τη ζώνη DNS. 

Για παράδειγμα, ο τομέας «contoso.com» μπορεί να περιέχει πολλές εγγραφές DNS, όπως 'mail.contoso.com' (για το διακομιστή ηλεκτρονικού ταχυδρομείου) και 'www.contoso.com' (για μια τοποθεσία web).

Όταν δημιουργείτε μια ζώνη DNS σε Azure DNS, το όνομα της ζώνης πρέπει να είναι μοναδικό μέσα στην ομάδα πόρων. Το ίδιο όνομα ζώνης μπορεί να χρησιμοποιηθεί ξανά σε μια ομάδα διαφορετικό πόρο ή μια διαφορετική συνδρομή του Azure. Το σημείο όπου πολλαπλές ζώνες έχουν το ίδιο όνομα, κάθε παρουσία εκχωρείται διευθύνσεις διακομιστή διαφορετικό όνομα. Μόνο ένα σύνολο των διευθύνσεων μπορούν να ρυθμιστούν με το μητρώο καταχώρησης ονομάτων τομέων.

>[AZURE.NOTE] Δεν χρειάζεται να κάτοχος ένα όνομα τομέα για να δημιουργήσετε μια ζώνη DNS με το συγκεκριμένο όνομα τομέα στο Azure DNS. Ωστόσο, πρέπει να ο κάτοχος του τομέα για να ρυθμίσετε τους διακομιστές ονομάτων Azure DNS ως τους διακομιστές σωστό όνομα για το όνομα του τομέα με το μητρώο καταχώρησης ονομάτων τομέων.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πληρεξουσίου τομέα στο Azure DNS](dns-domain-delegation.md).

## <a name="dns-records"></a>Εγγραφές DNS

### <a name="record-types"></a>Τύποι εγγραφών

Κάθε εγγραφή DNS έχει ένα όνομα και έναν τύπο. Εγγραφές είναι οργανωμένα σε διάφορους τύπους σύμφωνα με τα δεδομένα που περιέχουν. Ο πιο κοινός τύπος είναι ένα 'A' εγγραφή, το οποίο αντιστοιχίζει ένα όνομα σε μια διεύθυνση IPv4. Μια άλλη κοινός τύπος είναι μια εγγραφή 'MX', η οποία αντιστοιχεί ένα όνομα σε ένα διακομιστή αλληλογραφίας.

Azure DNS υποστηρίζει όλα τα κοινά τύπων εγγραφών DNS: Α, AAAA, CNAME, MX, NS, PTR, SOA, SRV και TXT.

### <a name="record-names"></a>Εγγραφή ονομάτων

Στο Azure DNS, καθορίζονται εγγραφές με σχετική ονόματα. Ένα όνομα τομέα (FQDN) *πλήρως προσδιορισμένη* περιλαμβάνει το όνομα της ζώνης, ότι ένα *σχετικό* όνομα δεν. Για παράδειγμα, το σχετικό όνομα εγγραφή "www" στη ζώνη 'contoso.com' παρέχει το όνομα της καρτέλας πλήρως προσδιορισμένη 'www.contoso.com'.

Μια εγγραφή *κορυφή* είναι μια εγγραφή DNS από το ριζικό κατάλογο (ή *κορυφή*) από μια ζώνη DNS. Για παράδειγμα, στη ζώνη DNS 'contoso.com', μια εγγραφή κορυφή έχει επίσης το πλήρως προσδιορισμένο όνομα 'contoso.com' (αυτό μερικές φορές ονομάζεται *ελεύθερη* τομέα).  Κατά συνθήκη, το σχετικό όνομα '@' χρησιμοποιείται για τη δημιουργία εγγραφών κορυφή.

### <a name="record-sets"></a>Σύνολα εγγραφών
Μερικές φορές πρέπει να δημιουργήσετε περισσότερες από μία εγγραφές DNS με ένα καθορισμένο όνομα και τύπο. Για παράδειγμα, ας υποθέσουμε ότι η τοποθεσία web 'www.contoso.com' φιλοξενείται σε δύο διαφορετικές διευθύνσεις IP. Στην τοποθεσία Web απαιτεί δύο διαφορετικές μια εγγραφές, μία για κάθε διεύθυνση IP. Αυτό είναι ένα παράδειγμα ενός συνόλου εγγραφών:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS διαχειρίζεται τις εγγραφές DNS χρησιμοποιώντας *σύνολα εγγραφής*. Ένα σύνολο καρτελών (γνωστό και ως ένα σύνολο εγγραφών *πόρων* ) είναι η συλλογή των εγγραφών DNS σε μια ζώνη που έχουν το ίδιο όνομα και είναι του ίδιου τύπου. Τα περισσότερα σύνολα εγγραφών περιέχει μία εγγραφή, αλλά παραδείγματα όπως αυτή, στις οποίες ένα σύνολο εγγραφών περιέχει περισσότερες από μία εγγραφές, δεν είναι ασυνήθιστο.

Για παράδειγμα, ας υποθέσουμε ότι έχετε ήδη δημιουργήσει μια εγγραφή A "www" στη ζώνη 'contoso.com', που δείχνει προς το IP διεύθυνση σε '134.170.185.46' (της πρώτης εγγραφής παραπάνω).  Για τη δημιουργία της δεύτερης εγγραφής που θα προσθέσετε τη συγκεκριμένη εγγραφή στο υπάρχον σύνολο εγγραφών, αντί να δημιουργήσετε ένα νέο σύνολο εγγραφών.

Οι τύποι εγγραφών SOA και CNAME είναι εξαιρέσεις. Τα πρότυπα DNS δεν επιτρέπουν τη χρήση πολλών εγγραφών με το ίδιο όνομα για αυτούς τους τύπους, επομένως αυτά τα σύνολα εγγραφών μπορεί να περιέχει μόνο μία εγγραφή.

### <a name="time-to-live"></a>Time to live

Η διάρκεια ζωής ή TTL, καθορίζει πόσο χρόνο κάθε εγγραφή αποθηκεύονται στο cache από προγράμματα-πελάτες πριν από την εκ νέου υποβάλλεται ερώτημα. Στο παραπάνω παράδειγμα, η τιμή TTL είναι 3.600 δευτερόλεπτα ή 1 ώρα.

Στο Azure DNS, την τιμή TTL έχει οριστεί για το σύνολο εγγραφών, όχι για κάθε εγγραφή, ώστε να χρησιμοποιείται η ίδια τιμή για όλες τις εγγραφές μέσα σε αυτό το σύνολο εγγραφών.  Μπορείτε να καθορίσετε οποιαδήποτε τιμή TTL από 1 έως 2,147,483,647 δευτερόλεπτα.

### <a name="wildcard-records"></a>Χαρακτήρας μπαλαντέρ εγγραφών

Azure DNS υποστηρίζει [τις εγγραφές μπαλαντέρ](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Εγγραφές μπαλαντέρ επιστρέφονται ως απόκριση σε οποιοδήποτε ερώτημα με όνομα που να ταιριάζει (εκτός αν υπάρχει μια πιο κοντινή αντιστοιχία από ένα σύνολο εγγραφών μη μπαλαντέρ). Σύνολα εγγραφών μπαλαντέρ υποστηρίζονται για όλους τους τύπους καρτελών εκτός από NS και SOA.  

Για να δημιουργήσετε ένα σύνολο εγγραφών μπαλαντέρ, χρησιμοποιήστε το όνομα του συνόλου εγγραφών '\*'. Εναλλακτικά, μπορείτε επίσης να χρησιμοποιήσετε ένα όνομα με το '\*'ως την πιο αριστερή ετικέτα του, για παράδειγμα,'\*.foo'.

### <a name="cname-records"></a>Εγγραφές CNAME

Σύνολα εγγραφών CNAME δεν είναι δυνατό να συνυπάρχουν με άλλα σύνολα εγγραφών με το ίδιο όνομα. Για παράδειγμα, δεν μπορείτε να δημιουργήσετε μια εγγραφή CNAME που έχουν οριστεί με το σχετικό όνομα "www" και μια εγγραφή A με το σχετικό όνομα "www" την ίδια στιγμή.

Επειδή η κορυφή ζώνη (όνομα = ‘@’) θα περιέχει πάντοτε το NS και SOA εγγραφή σύνολα που έχουν δημιουργηθεί κατά τη δημιουργία της ζώνης, δεν μπορείτε να δημιουργήσετε μια εγγραφή CNAME που έχει οριστεί στην κορυφή ζώνη.

Αυτούς τους περιορισμούς προκύπτουν από τα πρότυπα DNS και δεν είναι περιορισμοί Azure DNS.

### <a name="ns-records"></a>Εγγραφές NS

Ένα σύνολο εγγραφών NS δημιουργείται αυτόματα στην κορυφή του κάθε ζώνη (όνομα = ‘@’), και διαγράφονται αυτόματα όταν διαγράφεται τη ζώνη (δεν είναι δυνατή η διαγραφή ξεχωριστά).  Μπορείτε να τροποποιήσετε την τιμή TTL αυτού του συνόλου εγγραφών, αλλά δεν μπορείτε να τροποποιήσετε τις εγγραφές, που είναι προρυθμισμένες παραμέτρους, ώστε να αναφέρεται τους διακομιστές ονομάτων Azure DNS στους οποίους έχουν ανατεθεί στη ζώνη.

Μπορείτε να δημιουργήσετε και να διαγράψετε άλλες εγγραφές NS μέσα στη ζώνη, όχι στην κορυφή ζώνη.  Αυτό σας επιτρέπει να ρυθμίσετε τις παραμέτρους θυγατρικό ζώνες (ανατρέξτε στο θέμα [αντιπροσώπευση υποτομείς στο Azure DNS](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns)).

### <a name="soa-records"></a>Εγγραφές SOA

Ένα σύνολο εγγραφών SOA δημιουργείται αυτόματα στην κορυφή του κάθε ζώνη (όνομα = ‘@’), και διαγράφονται αυτόματα όταν διαγράφεται της ζώνης.  Εγγραφές SOA δεν είναι δυνατό να δημιουργηθεί ή να διαγραφούν ξεχωριστά. 

Μπορείτε να τροποποιήσετε όλες τις ιδιότητες της εγγραφής SOA εκτός από την ιδιότητα "κεντρικού υπολογιστή", η οποία είναι προρυθμισμένες παραμέτρους, ώστε να αναφέρεται το όνομα του διακομιστή κύριο όνομα που παρέχεται από Azure DNS.

### <a name="spf-records"></a>Εγγραφές SPF

Καρτέλες Framework πολιτικής (SPF) αποστολέα χρησιμοποιούνται για να καθορίσετε ποιους διακομιστές ηλεκτρονικού ταχυδρομείου επιτρέπεται να στέλνουν μηνύματα ηλεκτρονικού ταχυδρομείου εκ μέρους ενός ονόματος τομέα που δίνεται.  Σωστή ρύθμιση παραμέτρων για τις εγγραφές SPF είναι σημαντικό να εμποδίσετε τους παραλήπτες τη σήμανση ηλεκτρονικής αλληλογραφίας ως "Ανεπιθύμητη αλληλογραφία".

Τα RFC DNS εισαχθεί αρχικά έναν νέο τύπο εγγραφής 'SPF' για την υποστήριξη αυτό το σενάριο. Για την υποστήριξη παλαιότερων διακομιστών ονομάτων, τους επίσης να επιτρέπεται η χρήση του τύπου εγγραφής TXT για να καθορίσετε τις εγγραφές SPF.  Αυτό ασαφειών οδήγησε σε σύγχυση, το οποίο επιλύθηκε με [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1).  Αυτό αναφέρεται ότι θα πρέπει να δημιουργηθούν μόνο με τον τύπο εγγραφής TXT εγγραφές SPF και καταργηθεί τον τύπο εγγραφής SPF. 

**Εγγραφές SPF υποστηρίζονται από το Azure DNS και θα πρέπει να δημιουργηθεί χρησιμοποιώντας τον τύπο εγγραφής TXT.** Η παλιά τύπο εγγραφής SPF δεν υποστηρίζεται. Όταν [η εισαγωγή μιας DNS ζώνη αρχείο](dns-import-export.md), τυχόν εγγραφές SPF χρησιμοποιώντας τον τύπο εγγραφής SPF μετατρέπονται σε τον τύπο εγγραφής TXT. 

### <a name="srv-records"></a>Εγγραφές SRV

[Εγγραφές SRV που](https://en.wikipedia.org/wiki/SRV_record) χρησιμοποιούνται από διάφορες υπηρεσίες για να καθορίσετε θέσεις διακομιστή. Όταν καθορίζετε μια εγγραφή SRV στο Azure DNS:

- Η *υπηρεσία* και *πρωτόκολλο* , πρέπει να καθοριστεί ως μέρος του ονόματος του συνόλου εγγραφών, το πρόθεμα χαρακτήρες υπογράμμισης.  Για παράδειγμα, '\_sip. \_tcp.name'.  Για μια εγγραφή στην κορυφή ζώνη, δεν χρειάζεται να καθορίσετε '@' στο όνομα της εγγραφής, απλώς να χρησιμοποιήσουμε την υπηρεσία και πρωτόκολλο, π.χ. '\_sip. \_tcp'.
- Η *προτεραιότητα*, *βάρους*, *θύρας*και *προορισμού* έχουν καθοριστεί ως παράμετροι από κάθε εγγραφή στο σύνολο εγγραφών. 

## <a name="tags-and-metadata"></a>Ετικέτες και μετα-δεδομένων

### <a name="tags"></a>Ετικέτες
Ετικέτες είναι μια λίστα με ζεύγη ονόματος-τιμής και χρησιμοποιούνται από τη διαχείριση πόρων Azure με ετικέτα πόρους.  Azure διαχείριση πόρων χρησιμοποιεί ετικέτες για να ενεργοποιήσετε την φιλτραρισμένες προβολές του λογαριασμού σας Azure και σάς επιτρέπει να ορίσετε μια πολιτική στην οποία απαιτούνται ετικέτες. Για περισσότερες πληροφορίες σχετικά με τις ετικέτες, ανατρέξτε στο θέμα [Χρήση ετικετών για να οργανώσετε το Azure πόρους](../resource-group-using-tags.md).

Azure DNS υποστηρίζει τη χρήση ετικετών από διαχειριστή πόρων Azure πόροι ζώνης DNS.  Δεν υποστηρίζει ετικέτες σε σύνολα εγγραφών DNS.

### <a name="metadata"></a>Μετα-δεδομένων

Ως εναλλακτική λύση για ετικέτες σύνολο εγγραφών, Azure DNS υποστηρίζει τη δημιουργία σχολίων σύνολα εγγραφών με χρήση 'μετα-δεδομένων'.  Παρόμοια με ετικέτες, μετα-δεδομένων σάς επιτρέπει να συσχετίσετε ζεύγη τιμών-ονομάτων με κάθε σύνολο εγγραφών.  Αυτό μπορεί να είναι χρήσιμο, για παράδειγμα για την εγγραφή το σκοπό της κάθε σύνολο εγγραφών.  Σε αντίθεση με ετικέτες, μετα-δεδομένων δεν μπορούν να χρησιμοποιηθούν για την παροχή μια φιλτραρισμένη προβολή του λογαριασμού σας Azure και δεν μπορεί να έχει καθοριστεί σε μια πολιτική διαχείρισης πόρων Azure.

## <a name="limits"></a>Όρια

Τα ακόλουθα όρια προεπιλεγμένη εφαρμογή κατά τη χρήση Azure DNS:

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>Επόμενα βήματα

- Για να ξεκινήσετε με τη χρήση Azure DNS, μάθετε πώς μπορείτε να [δημιουργήσετε μια ζώνη DNS](dns-getstarted-create-dnszone-portal.md) και να [δημιουργήσετε τις εγγραφές DNS](dns-getstarted-create-recordset-portal.md).
- Για τη μετεγκατάσταση ενός υπάρχοντος ζώνης DNS, μάθετε τον τρόπο [εισαγωγής και DNS zone file](dns-import-export.md).