##<a name="tcp-settings-for-azure-vms"></a>Ρυθμίσεις TCP για ΣΠΣ Azure

Azure ΣΠΣ επικοινωνία με το δημόσια στο Internet με τη χρήση [NAT] [ nat] (μετάφραση διευθύνσεων δικτύου). Συσκευές NAT εκχωρήστε μια δημόσια διεύθυνση IP και θύρα Εικονική μηχανή Azure, επιτρέποντάς που Εικονική για να δημιουργήσετε μια υποδοχή για την επικοινωνία με άλλες συσκευές. Εάν τα πακέτα σταματά η ροή μέσω που socket μετά από ένα συγκεκριμένο χρονικό διάστημα, η συσκευή NAT τερματίζει την αντιστοίχιση και της υποδοχής είναι δωρεάν θα χρησιμοποιηθεί από άλλα ΣΠΣ.

Αυτό είναι ένα κοινό συμπεριφορά NAT, η οποία μπορεί να προκαλέσει ζητήματα επικοινωνίας σε TCP με βάση τις εφαρμογές που αναμένετε μια υποδοχή για διατήρηση πέρα από μια περίοδο λήξης χρονικού ορίου. Υπάρχουν δύο ρυθμίσεις χρονικού ορίου αδράνειας πρέπει να λάβετε υπόψη, για περιόδους λειτουργίας σε κατάσταση *έχει δημιουργηθεί η σύνδεση* :

- **εισερχόμενη** έως τη [μονάδα εξισορρόπησης φόρτου Azure][azure-lb-timeout]. Αυτό το χρονικό όριο 4 λεπτά από προεπιλογή και μπορούν να προσαρμοστούν προς τα επάνω σε 30 λεπτά.
- **εξερχομένων** χρησιμοποιώντας [SNAT] [ snat] (προέλευσης NAT). Αυτό το χρονικό όριο έχει οριστεί σε 4 λεπτά και δεν είναι δυνατό να προσαρμοστούν.

Για να βεβαιωθείτε ότι δεν έχουν χαθεί συνδέσεις πέρα από το όριο χρονικού ορίου, που θα πρέπει να βεβαιωθείτε ότι είτε εφαρμογή σας διατηρεί την περίοδο λειτουργίας ενεργή ή μπορείτε να ρυθμίσετε το υποκείμενο λειτουργικό σύστημα για να το κάνετε. Οι ρυθμίσεις που θα χρησιμοποιηθεί είναι διαφορετικές για Windows και Linux συστήματα, όπως φαίνεται παρακάτω.

Για [Linux][linux], θα πρέπει να αλλάξετε τις μεταβλητές πυρήνα παρακάτω.
NET.IPv4.tcp_keepalive_time = 120 net.ipv4.tcp_keepalive_intvl = 30 net.ipv4.tcp_keepalive_probes = 8
 
Για [τα Windows][windows], θα πρέπει να αλλάξετε τις παρακάτω τιμές μητρώου.
KeepAliveInterval = 30 KeepAliveTime = 120 TcpMaxDataRetransmissions = 8


Οι παραπάνω ρυθμίσεις, βεβαιωθείτε ότι ένα πακέτο ενεργών διατήρηση είναι σταλεί μετά από 2 λεπτά (120 δευτερόλεπτα) του χρόνου αδράνειας και, στη συνέχεια, αποστέλλονται κάθε 30 δευτερόλεπτα. Και εάν αποτύχει 8 από αυτά τα πακέτα, η περίοδος λειτουργίας έχει καταργηθεί.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md