<properties
   pageTitle="Διαχείριση τελικά σημεία στη Διαχείριση Azure κίνηση | Microsoft Azure"
   description="Σε αυτό το άρθρο θα σας βοηθήσει να Προσθήκη, κατάργηση, ενεργοποίηση και απενεργοποίηση τελικά σημεία από τη Διαχείριση Azure κίνηση."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="add-disable-enable-or-delete-endpoints"></a>Προσθήκη, απενεργοποίηση, ενεργοποιήσετε ή να διαγράψετε τα τελικά σημεία

Η δυνατότητα Web Apps στο Azure εφαρμογής υπηρεσίας παρέχει ήδη ανακατεύθυνσης και την κυκλοφορία round robin δρομολόγησης λειτουργικότητα για τοποθεσίες Web μέσα σε ένα κέντρο δεδομένων, ανεξάρτητα από τη λειτουργία της τοποθεσίας Web. Azure Manager κίνηση σάς επιτρέπει να καθορίσετε ανακατεύθυνσης και την κυκλοφορία round robin δρομολόγησης για τοποθεσίες Web και cloud services σε διαφορετικά κέντρα δεδομένων. Το πρώτο βήμα είναι απαραίτητο να σας παρέχει λειτουργικότητα που είναι να προσθέσετε το τελικό σημείο υπηρεσίας ή στην τοποθεσία Web cloud στη Διαχείριση κίνηση.

>[AZURE.NOTE] Δεν μπορείτε να προσθέσετε κίνηση Διαχείριση προφίλ ή εξωτερικές θέσεις ως τελικά σημεία με την πύλη Azure κλασική. Πρέπει να χρησιμοποιήσετε το REST API [Δημιουργία ορισμού](http://go.microsoft.com/fwlink/p/?LinkId=400772) ή του Windows PowerShell [Προσθήκη AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774).

Μπορείτε επίσης να απενεργοποιήσετε μεμονωμένα τελικά σημεία που αποτελούν μέρος ενός προφίλ διαχείρισης κίνηση. Τα τελικά σημεία περιλαμβάνουν τις υπηρεσίες cloud και τοποθεσίες Web που διαθέτετε. Απενεργοποίηση τελικού σημείου αφήνει το ως μέρος του προφίλ, αλλά το προφίλ συμπεριφέρεται σαν να το τελικό σημείο δεν περιλαμβάνεται σε αυτήν. Αυτή η ενέργεια είναι πολύ χρήσιμο για την κατάργηση προσωρινά ένα τελικό σημείο που βρίσκεται σε κατάσταση λειτουργίας συντήρησης ή να επαναληφθεί η ανάπτυξη. Αφού ξεκινήσετε ξανά το τελικό σημείο, μπορεί να ενεργοποιηθεί.

>[AZURE.NOTE] Απενεργοποίηση τελικού σημείου έχει τίποτα, προκειμένου να κάνετε με την κατάστασή ανάπτυξης στο Azure. Μια καλή τελικού σημείου θα παραμείνει προς τα επάνω και μπορεί να λαμβάνει την κυκλοφορία ακόμα και όταν απενεργοποιημένη στη Διαχείριση κίνηση. Επιπλέον, απενεργοποιώντας ένα τελικό σημείο σε ένα προφίλ δεν επηρεάζει την κατάστασή σε κάποιο άλλο προφίλ.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Για να προσθέσετε ένα σύννεφο τελικού σημείου υπηρεσίας ή στην τοποθεσία Web


1. Στο παράθυρο Διαχείριση κίνηση στην πύλη του Azure κλασική, εντοπίστε το προφίλ της διαχείρισης κίνηση που περιέχει τις ρυθμίσεις τελικό σημείο που θέλετε να τροποποιήσετε και, στη συνέχεια, κάντε κλικ στο βέλος που βρίσκεται δεξιά από το όνομα του προφίλ. Θα ανοίξει η σελίδα "Ρυθμίσεις" για το προφίλ.
2. Στο επάνω μέρος της σελίδας, κάντε κλικ στην επιλογή **τελικά σημεία** για να προβάλετε τα τελικά σημεία που αποτελούν ήδη μέρος της ρύθμισης παραμέτρων.
3. Στο κάτω μέρος της σελίδας, κάντε κλικ στην επιλογή **Προσθήκη** για να αποκτήσετε πρόσβαση στη σελίδα **Τελικά σημεία Προσθήκη υπηρεσίας** . Από προεπιλογή, η σελίδα παραθέτει τις υπηρεσίες cloud στην περιοχή **Υπηρεσία τελικά σημεία**.
4. Για τις υπηρεσίες cloud, επιλέξτε τις υπηρεσίες cloud της λίστας για να ενεργοποιήσετε τους ως τελικά σημεία για αυτό το προφίλ. Καταργώντας το όνομα της υπηρεσίας cloud καταργεί από τη λίστα με τα τελικά σημεία.
5. Για τοποθεσίες Web, κάντε κλικ στην αναπτυσσόμενη λίστα **Τύπος υπηρεσίας** και, στη συνέχεια, επιλέξτε **εφαρμογή Web**.
6. Επιλέξτε τις τοποθεσίες Web από τη λίστα για να τα προσθέσετε ως τελικά σημεία για αυτό το προφίλ. Καταργώντας το όνομα της τοποθεσίας Web καταργεί από τη λίστα με τα τελικά σημεία. Σημειώστε ότι μπορείτε να επιλέξετε μόνο μια μεμονωμένη τοποθεσία Web ανά Azure κέντρο δεδομένων (γνωστό και ως μια περιοχή). Εάν επιλέξετε μια τοποθεσία Web σε ένα κέντρο δεδομένων που φιλοξενεί πολλές τοποθεσίες Web, όταν επιλέγετε την πρώτη τοποθεσία Web, τα άλλα άτομα στο ίδιο κέντρο δεδομένων είναι διαθέσιμες για επιλογή. Επίσης, σημειώστε ότι παρατίθενται μόνο οι τυπικές τοποθεσίες Web που διαθέτετε.
7. Αφού επιλέξετε τα τελικά σημεία για αυτό το προφίλ, κάντε κλικ στην επιλογή το σημάδι επιλογής σε στην κάτω δεξιά γωνία για να αποθηκεύσετε τις αλλαγές σας.

>[AZURE.NOTE] Εάν χρησιμοποιείτε την *ανακατεύθυνση* κίνηση μέθοδο δρομολόγησης, μετά την προσθήκη ή κατάργηση ενός ορίου, φροντίστε να προσαρμόσετε τη λίστα προτεραιότητα ανακατεύθυνσης στη σελίδα Ρύθμιση παραμέτρων για να απεικονίσει τη σειρά ανακατεύθυνσης που θέλετε για τη ρύθμιση παραμέτρων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ανακατεύθυνσης δρομολόγηση κίνηση](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Για να απενεργοποιήσετε ένα τελικό σημείο

1. Στο παράθυρο Διαχείριση κίνηση στην πύλη του Azure κλασική, εντοπίστε το προφίλ της διαχείρισης κίνηση που περιέχει τις ρυθμίσεις τελικό σημείο που θέλετε να τροποποιήσετε και, στη συνέχεια, κάντε κλικ στο βέλος που βρίσκεται δεξιά από το όνομα του προφίλ. Θα ανοίξει η σελίδα "Ρυθμίσεις" για το προφίλ.
2. Στο επάνω μέρος της σελίδας, κάντε κλικ στην επιλογή **τελικά σημεία** για να προβάλετε τα τελικά σημεία που περιλαμβάνονται στη ρύθμιση παραμέτρων σας.
3. Επιλέξτε το τελικό σημείο που θέλετε να απενεργοποιήσετε και, στη συνέχεια, κάντε κλικ στην επιλογή " **Απενεργοποίηση** " στο κάτω μέρος της σελίδας.
4. Κίνηση σταματά να ρέει προς το τελικό σημείο με βάση το DNS χρόνου-σε-Live (TTL) έχουν ρυθμιστεί για το όνομα τομέα Manager κίνηση. Μπορείτε να αλλάξετε την τιμή TTL από τη σελίδα ρύθμισης παραμέτρων του προφίλ Manager κίνηση.

## <a name="to-enable-an-endpoint"></a>Για να ενεργοποιήσετε ένα τελικό σημείο

1. Στο παράθυρο Διαχείριση κίνηση στην πύλη του Azure κλασική, εντοπίστε το προφίλ της διαχείρισης κίνηση που περιέχει τις ρυθμίσεις τελικό σημείο που θέλετε να τροποποιήσετε και, στη συνέχεια, κάντε κλικ στο βέλος που βρίσκεται δεξιά από το όνομα του προφίλ. Θα ανοίξει η σελίδα "Ρυθμίσεις" για το προφίλ.
2. Στο επάνω μέρος της σελίδας, κάντε κλικ στην επιλογή **τελικά σημεία** για να προβάλετε τα τελικά σημεία που περιλαμβάνονται στη ρύθμιση παραμέτρων σας.
3. Επιλέξτε το τελικό σημείο που θέλετε να ενεργοποιήσετε και, στη συνέχεια, κάντε κλικ στην επιλογή " **Ενεργοποίηση** " στο κάτω μέρος της σελίδας.
4. Κίνηση θα ξεκινήσει η ροή στην υπηρεσία ξανά ως υπαγορευμένου από το προφίλ.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Για να διαγράψετε ένα σύννεφο τελικού σημείου υπηρεσίας ή στην τοποθεσία Web


1. Στο παράθυρο Διαχείριση κίνηση στην πύλη του Azure κλασική, εντοπίστε το προφίλ της διαχείρισης κίνηση που περιέχει τις ρυθμίσεις τελικό σημείο που θέλετε να τροποποιήσετε και, στη συνέχεια, κάντε κλικ στο βέλος που βρίσκεται δεξιά από το όνομα του προφίλ. Θα ανοίξει η σελίδα "Ρυθμίσεις" για το προφίλ.
2. Στο επάνω μέρος της σελίδας, κάντε κλικ στην επιλογή **τελικά σημεία** για να προβάλετε τα τελικά σημεία που αποτελούν ήδη μέρος της ρύθμισης παραμέτρων.
3. Στη σελίδα τελικά σημεία, κάντε κλικ στο όνομα του το τελικό σημείο που θέλετε να διαγράψετε από το προφίλ.
4. Στο κάτω μέρος της σελίδας, κάντε κλικ στην επιλογή **Διαγραφή**.

>[AZURE.NOTE] Δεν μπορείτε να διαγράψετε εξωτερικές θέσεις ή προφίλ διαχείρισης κίνηση ως τελικά σημεία με την πύλη Azure κλασική. Πρέπει να χρησιμοποιήσετε Windows PowerShell. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Κατάργηση AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx).

## <a name="next-steps"></a>Επόμενα βήματα

- [Ρύθμιση παραμέτρων μέθοδο δρομολόγησης ανακατεύθυνσης](traffic-manager-configure-failover-routing-method.md)
- [Ρύθμιση παραμέτρων μέθοδο δρομολόγησης round robin](traffic-manager-configure-round-robin-routing-method.md)
- [Ρύθμιση παραμέτρων μέθοδο δρομολόγησης επιδόσεων](traffic-manager-configure-performance-routing-method.md)
- [Διαχείριση αντιμετώπισης προβλημάτων κίνηση υποβαθμισμένο κατάσταση](traffic-manager-troubleshooting-degraded.md)
- [Λειτουργίες στη Διαχείριση κίνηση (REST API υλικό αναφοράς)](http://go.microsoft.com/fwlink/p/?LinkID=313584)