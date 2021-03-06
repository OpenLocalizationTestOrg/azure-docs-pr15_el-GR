<properties
   pageTitle="Ένα σύμπλεγμα ύφασμα υπηρεσίας ασφαλούς | Microsoft Azure"
   description="Περιγράφει τα σενάρια ασφαλείας για ένα σύμπλεγμα ύφασμα υπηρεσίας και τις διαφορετικές τεχνολογίες που χρησιμοποιούνται για την υλοποίηση αυτά τα σενάρια."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/19/2016"
   ms.author="chackdan"/>

# <a name="service-fabric-cluster-security-scenarios"></a>Υπηρεσία ύφασμα σενάρια ασφαλείας συμπλέγματος

Ένα σύμπλεγμα ύφασμα υπηρεσίας είναι ένας πόρος που διαθέτετε. Συμπλεγμάτων πρέπει πάντα να διασφαλιστεί για να αποτρέψετε μη εξουσιοδοτημένους χρήστες από τη σύνδεση με το σύμπλεγμά σας, ιδίως όταν έχει παραγωγής φόρτους εργασίας εκτελούνται σε αυτόν. Παρόλο που είναι δυνατό να δημιουργήσετε ένα μη ασφαλές σύμπλεγμα, κάνοντας έτσι θα επιτρέπουν οι ανώνυμοι χρήστες να συνδεθείτε με το εάν εκθέτει τελικά σημεία διαχείρισης δημόσια στο Internet. 

Σε αυτό το άρθρο παρέχει μια επισκόπηση του τα σενάρια ασφαλείας για συμπλεγμάτων εκτελείται σε Azure ή αυτόνομο και τις διάφορες τεχνολογίες που χρησιμοποιούνται για την υλοποίηση αυτά τα σενάρια. Τα σενάρια ασφαλείας συμπλέγματος είναι:

- Κόμβου προς κόμβο ασφαλείας
- Ασφάλεια κόμβος προγράμματος-πελάτη
- Έλεγχος πρόσβασης βάσει ρόλων (RBAC)

## <a name="node-to-node-security"></a>Κόμβου προς κόμβο ασφαλείας
Διασφαλίζει την επικοινωνία μεταξύ του ΣΠΣ ή μηχανές του συμπλέγματος. Αυτό εξασφαλίζει ότι μόνο οι υπολογιστές που είναι εξουσιοδοτημένοι να συμμετάσχουν στο σύμπλεγμα μπορούν να συμμετέχουν σε φιλοξενίας εφαρμογών και υπηρεσιών του συμπλέγματος.

![Διάγραμμα κόμβου προς κόμβο επικοινωνίας][Node-to-Node]

Συμπλεγμάτων εκτελείται σε Azure ή αυτόνομη έκδοση συμπλεγμάτων εκτελείται σε Windows να χρησιμοποιήσετε [Το πιστοποιητικό ασφαλείας](https://msdn.microsoft.com/library/ff649801.aspx) ή [Ασφαλείας των Windows](https://msdn.microsoft.com/library/ff649396.aspx) για υπολογιστές Windows Server.
### <a name="node-to-node-certificate-security"></a>Κόμβου προς κόμβο πιστοποιητικό ασφαλείας
Υπηρεσία ύφασμα χρησιμοποιεί τα πιστοποιητικά διακομιστή X.509 που καθορίζετε ως μέρος της τις ρυθμίσεις παραμέτρων Τύπος κόμβου όταν δημιουργείτε ένα σύμπλεγμα. Μια γρήγορη επισκόπηση του τι είναι αυτά τα πιστοποιητικά και πώς μπορείτε να αποκτήσετε ή να τα δημιουργήσετε παρέχονται στο τέλος αυτού του άρθρου.

Πιστοποιητικό ασφαλείας έχει ρυθμιστεί κατά τη δημιουργία του συμπλέγματος μέσω Azure πύλη, πρότυπα διαχείρισης πόρων Azure ή ένα πρότυπο JSON μεμονωμένη. Μπορείτε να καθορίσετε ένα πρωτεύον πιστοποιητικό και ένα προαιρετικό δευτερεύοντα πιστοποιητικό που χρησιμοποιείται για εικόνες κατάδειξης πιστοποιητικού. Τα πιστοποιητικά κύριας και δευτερεύουσας που καθορίζετε πρέπει να είναι διαφορετικά από το διαχειριστή προγράμματος-πελάτη και τα πιστοποιητικά προγράμματος-πελάτη μόνο για ανάγνωση που καθορίζετε για [την ασφάλεια κόμβος προγράμματος-πελάτη](#client-to-node-security).

Για Azure Διαβάστε [Ρυθμίστε ένα σύμπλεγμα με τη χρήση ενός προτύπου για τη διαχείριση πόρων Azure](service-fabric-cluster-creation-via-arm.md) για να μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους του πιστοποιητικού ασφαλείας σε ένα σύμπλεγμα.

Για μεμονωμένη Windows Server Διαβάστε [ασφαλούς ένα σύμπλεγμα μεμονωμένη στα Windows χρησιμοποιώντας πιστοποιητικά X.509](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Ασφάλεια κόμβου προς κόμβο των windows
Για μεμονωμένη Windows Server Διαβάστε [ασφαλούς ένα σύμπλεγμα μεμονωμένη στα Windows χρησιμοποιώντας ασφαλείας των Windows](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Ασφάλεια κόμβος προγράμματος-πελάτη
Πραγματοποιεί έλεγχο ταυτότητας προγράμματα-πελάτες και διασφαλίζει την επικοινωνία μεταξύ ενός υπολογιστή-πελάτη και μεμονωμένες κόμβους του συμπλέγματος. Αυτός ο τύπος ασφαλείας πραγματοποιεί έλεγχο ταυτότητας και διασφαλίζει τις επικοινωνίες προγράμματος-πελάτη, που διασφαλίζει ότι μόνο σε εξουσιοδοτημένους χρήστες να αποκτήσετε πρόσβαση στο σύμπλεγμα και τις εφαρμογές που έχουν αναπτυχθεί στο σύμπλεγμα. Προγράμματα-πελάτες αναγνωρίζονται με μοναδικό τρόπο μέσω τα διαπιστευτήριά τους ασφαλείας των Windows ή τα διαπιστευτήριά τους πιστοποιητικό ασφαλείας.

![Διάγραμμα επικοινωνίας κόμβος προγράμματος-πελάτη][Client-to-Node]

Συμπλεγμάτων εκτελείται σε Azure ή αυτόνομη έκδοση συμπλεγμάτων εκτελείται σε Windows να χρησιμοποιήσετε [Το πιστοποιητικό ασφαλείας](https://msdn.microsoft.com/library/ff649801.aspx) ή [Ασφαλείας των Windows](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Κόμβος προγράμματος-πελάτη πιστοποιητικό ασφαλείας
 Κόμβος προγράμματος-πελάτη πιστοποιητικό ασφαλείας έχει ρυθμιστεί κατά τη δημιουργία του συμπλέγματος είτε μέσω της πύλης Azure, πρότυπα διαχείρισης πόρων ή ένα πρότυπο JSON μεμονωμένη, καθορίζοντας ένα πιστοποιητικό προγράμματος-πελάτη διαχειριστή ή/και ένα πιστοποιητικό προγράμματος-πελάτη του χρήστη.  Τα διαχείρισης προγράμματος-πελάτη και χρήστη του προγράμματος-πελάτη πιστοποιητικά που καθορίζετε πρέπει να είναι διαφορετικά από τα πιστοποιητικά κύριας και δευτερεύουσας που καθορίζετε για [την ασφάλεια κόμβου προς κόμβο](#node-to-node-security).

Πελάτες που συνδέονται στο σύμπλεγμα χρησιμοποιώντας το πιστοποιητικό διαχείρισης έχετε πλήρη πρόσβαση στις δυνατότητες διαχείρισης.  Πελάτες που συνδέονται στο σύμπλεγμα χρησιμοποιώντας το πιστοποιητικό προγράμματος-πελάτη μόνο για ανάγνωση χρήστη έχουν μόνο πρόσβαση για ανάγνωση δυνατότητες διαχείρισης. Με άλλα λόγια αυτά τα πιστοποιητικά που χρησιμοποιούνται για το ρόλο βάσεις έλεγχο πρόσβασης (RBAC) που περιγράφεται παρακάτω σε αυτό το άρθρο.

Για Azure Διαβάστε [Ρυθμίστε ένα σύμπλεγμα με τη χρήση ενός προτύπου για τη διαχείριση πόρων Azure](service-fabric-cluster-creation-via-arm.md) για να μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους του πιστοποιητικού ασφαλείας σε ένα σύμπλεγμα.

Για μεμονωμένη Windows Server Διαβάστε [ασφαλούς ένα σύμπλεγμα μεμονωμένη στα Windows χρησιμοποιώντας πιστοποιητικά X.509](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Κόμβος προγράμματος-πελάτη Azure Active Directory (AAD) ασφαλείας στο Azure
Εκτελείται σε Azure συμπλεγμάτων επίσης να ασφαλή πρόσβαση σε τα τελικά σημεία διαχείρισης χρησιμοποιώντας Azure Active Directory (AAD). Για πληροφορίες σχετικά με το πώς μπορείτε να δημιουργήσετε τα αντικείμενα AAD είναι απαραίτητο, πώς μπορείτε να συμπληρώσετε τις κατά τη δημιουργία συμπλέγματος και πώς να συνδεθείτε σε αυτές τις συμπλεγμάτων αργότερα, ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα με χρήση ενός προτύπου για τη διαχείριση πόρων Azure](service-fabric-cluster-creation-via-arm.md) .

## <a name="security-recommendations"></a>Συστάσεις ασφαλείας
Για συμπλεγμάτων Azure, συνιστάται να χρησιμοποιήσετε AAD ασφαλείας για τον έλεγχο ταυτότητας προγράμματα-πελάτες και τα πιστοποιητικά για την ασφάλεια κόμβου προς κόμβο.

Για μεμονωμένη Windows Server συμπλεγμάτων συνιστάται να χρησιμοποιήσετε ασφάλεια των Windows με την ομάδα διαχείρισης λογαριασμών (GMA), εάν έχετε Windows Server 2012 R2 και της υπηρεσίας καταλόγου Active Directory. Διαφορετικά εξακολουθεί να χρησιμοποιήσετε ασφάλεια των Windows με τους λογαριασμούς των Windows.

## <a name="role-based-access-control-rbac"></a>Έλεγχος πρόσβασης βάσει ρόλων (RBAC)
Έλεγχος πρόσβασης επιτρέπει στο διαχειριστή συμπλέγματος για να περιορίσετε την πρόσβαση σε ορισμένες λειτουργίες συμπλέγματος για διαφορετικές ομάδες χρηστών, καθιστώντας πιο ασφαλή στο σύμπλεγμα. Δύο τύπους στοιχείων ελέγχου διαφορετικό επίπεδο πρόσβασης που υποστηρίζονται για προγράμματα-πελάτες τη σύνδεση σε ένα σύμπλεγμα: το ρόλο διαχειριστή και ρόλο χρήστη.

Οι διαχειριστές έχουν πλήρη πρόσβαση στις δυνατότητες διαχείρισης (συμπεριλαμβανομένων των δυνατοτήτων μόνο για ανάγνωση). Οι χρήστες, από προεπιλογή, έχουν μόνο πρόσβαση για ανάγνωση για δυνατότητες διαχείρισης (για παράδειγμα, δυνατότητες ερωτήματος) και τη δυνατότητα να επιλύσετε εφαρμογών και υπηρεσιών.

Καθορίστε τους ρόλους προγράμματος-πελάτη διαχειριστή και χρηστών τη στιγμή της δημιουργίας συμπλέγματος, παρέχοντας ξεχωριστές ταυτότητες (πιστοποιητικά, AAD κ.λπ.) για κάθε. Για περισσότερες πληροφορίες σχετικά με τις προεπιλεγμένες ρυθμίσεις ελέγχου πρόσβασης και πώς μπορείτε να αλλάξετε τις προεπιλεγμένες ρυθμίσεις, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης για προγράμματα-πελάτες υπηρεσίας ύφασμα βάσει ρόλων](service-fabric-cluster-security-roles.md).


## <a name="x509-certificates-and-service-fabric"></a>Πιστοποιητικά X.509 και ύφασμα υπηρεσίας
Ψηφιακών πιστοποιητικών X.509 που χρησιμοποιούνται ευρέως για τον έλεγχο ταυτότητας προγράμματα-πελάτες και διακομιστές και για την κρυπτογράφηση και ψηφιακή υπογραφή μηνυμάτων. Για περισσότερες λεπτομέρειες σχετικά με αυτά τα πιστοποιητικά, μεταβείτε στην [εργασία με τα πιστοποιητικά](http://msdn.microsoft.com/library/ms731899.aspx).

Ορισμένα σημαντικά πράγματα που πρέπει να λάβετε υπόψη:

- Πιστοποιητικά που χρησιμοποιούνται στο συμπλεγμάτων εκτελείται παραγωγής φόρτους εργασίας θα πρέπει να είναι δημιουργηθεί με τη χρήση σωστά ρυθμισμένο υπηρεσίας πιστοποιητικό Windows Server ή που λαμβάνονται από ένα εγκεκριμένο [Αρχή έκδοσης πιστοποιητικών (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
- Ποτέ χρησιμοποιήστε οποιαδήποτε προσωρινά αρχεία ή να εξετάσετε πιστοποιητικά στην παραγωγής που έχουν δημιουργηθεί με εργαλεία όπως το MakeCert.exe.
- Μπορείτε να χρησιμοποιήσετε ένα αυτο-υπογεγραμμένο πιστοποιητικό, αλλά μόνο θα πρέπει να κάνετε για έλεγχο συμπλεγμάτων και όχι σε παραγωγής.

### <a name="server-x509-certificates"></a>Τα πιστοποιητικά X.509 διακομιστή

Τα πιστοποιητικά διακομιστή έχουν την κύρια εργασία τον έλεγχο ταυτότητας διακομιστή (κόμβο) στους υπολογιστές-πελάτες ή τον έλεγχο ταυτότητας διακομιστή (κόμβο) σε ένα διακομιστή (κόμβο). Έναν από τους αρχικούς ελέγχους όταν ένα πρόγραμμα-πελάτη ή κόμβου πραγματοποιεί έλεγχο ταυτότητας έναν κόμβο είναι να ελέγξετε την τιμή του το κοινό όνομα στο πεδίο "θέμα". Σε αυτό το κοινό όνομα ή ένα από τα πιστοποιητικά θέμα εναλλακτικές ονόματα πρέπει να υπάρχει στη λίστα των επιτρεπόμενων κοινά ονόματα.

Το ακόλουθο άρθρο περιγράφει πώς μπορείτε να δημιουργήσετε τα πιστοποιητικά με θέμα εναλλακτικές ονόματα (ΣΑΝ): [πώς μπορείτε να προσθέσετε ένα εναλλακτικό όνομα θέματος σε ένα πιστοποιητικό ασφαλούς LDAP](http://support.microsoft.com/kb/931351).

Πεδίο "θέμα" μπορεί να περιέχει πολλές τιμές, κάθε το πρόθεμα μια προετοιμασίας για να υποδείξετε τον τύπο τιμής. Συνήθως, η προετοιμασία είναι "CN" για το κοινό όνομα; Για παράδειγμα, "CN = www.contoso.com". Επίσης, είναι δυνατή για το πεδίο "θέμα" για να είναι κενή. Εάν το προαιρετικό πεδίο εναλλακτικό όνομα θέματος συμπληρώνεται, πρέπει να περιέχει το κοινό όνομα του πιστοποιητικού και μία εγγραφή ανά εναλλακτικό όνομα θέματος. Εισάγονται ως τιμές όνομα DNS.

Η τιμή του πεδίου συγκεκριμένες χρήσεις του πιστοποιητικού θα πρέπει να συμπεριλάβετε μια κατάλληλη τιμή, όπως "Έλεγχος ταυτότητας διακομιστή" ή "Έλεγχος ταυτότητας προγράμματος-πελάτη".

### <a name="client-x509-certificates"></a>Πιστοποιητικά X.509 προγράμματος-πελάτη

Πιστοποιητικά προγράμματος-πελάτη δεν είναι συνήθως εκδοθεί από μια αρχή έκδοσης πιστοποιητικών τρίτων κατασκευαστών. Αντί για αυτό, το προσωπικό χώρο αποθήκευσης από την τρέχουσα θέση χρήστη περιέχει συνήθως πιστοποιητικά προγράμματος-πελάτη τοποθετηθεί εκεί από μια αρχή έκδοσης πιστοποιητικών ρίζας, με έναν προορισμό "Ελέγχου ταυτότητας προγράμματος-πελάτη". Ο υπολογιστής-πελάτης να χρησιμοποιήσετε το πιστοποιητικό όταν απαιτείται έλεγχος ταυτότητας αμοιβαία.

>[AZURE.NOTE] Όλες οι λειτουργίες διαχείρισης σε ένα σύμπλεγμα υπηρεσίας ύφασμα απαιτούν πιστοποιητικά διακομιστή. Πιστοποιητικά προγράμματος-πελάτη δεν μπορεί να χρησιμοποιηθεί για τη διαχείριση.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο παρέχει γενικές πληροφορίες σχετικά με την ασφάλεια σύμπλεγμα. Επόμενο, [Δημιουργήστε ένα σύμπλεγμα στο Azure χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων](service-fabric-cluster-creation-via-arm.md) ή μέσω του [Azure πύλη](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
