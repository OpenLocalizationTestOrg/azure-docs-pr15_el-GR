<properties
    pageTitle="Azure αναπαραγωγής αποθήκευσης | Microsoft Azure"
    description="Αναπαραγωγή δεδομένων στο λογαριασμό σας Microsoft Azure χώρου αποθήκευσης για διάρκεια ζωής και υψηλή διαθεσιμότητα. Επιλογές αναπαραγωγής περιλαμβάνουν τοπικά πλεονάζοντα χώρο αποθήκευσης (LRS), ζώνη πλεονάζοντα χώρο αποθήκευσης (ZRS), παν πλεονάζοντα χώρο αποθήκευσης (Εξοπλισμό) και πρόσβαση για ανάγνωση παν πλεονάζοντα αποθήκευσης (RA-Εξοπλισμό)."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="azure-storage-replication"></a>Azure αναπαραγωγής χώρου αποθήκευσης

Τα δεδομένα στο χώρο αποθήκευσης στο λογαριασμό σας Microsoft Azure αναπαράγεται πάντα για να βεβαιωθείτε ότι διάρκεια ζωής και υψηλή διαθεσιμότητα. Αναπαραγωγή αντιγράφει τα δεδομένα σας, είτε μέσα στο ίδιο κέντρο δεδομένων ή σε μια δεύτερη κέντρο δεδομένων, ανάλογα με την επιλογή αναπαραγωγής που επιλέγετε. Αναπαραγωγή προστατεύει τα δεδομένα σας και διατηρεί την εφαρμογή του χρόνου σε περίπτωση αποτυχίες μεταβατικές υλικού. Εάν τα δεδομένα σας αναπαράγεται σε ένα δεύτερο κέντρο δεδομένων, που προστατεύει επίσης τα δεδομένα σας σε σχέση με μια καταστροφική αποτυχία στην κύρια θέση.

Αναπαραγωγή εξασφαλίζει ότι ο λογαριασμός σας χώρο αποθήκευσης πληροί την [Υπηρεσία-σύμβαση (SLA) για το χώρο αποθήκευσης](https://azure.microsoft.com/support/legal/sla/storage/) ακόμα λόγω αποτυχίες. Ανατρέξτε στο θέμα το SLA για πληροφορίες σχετικά με το χώρο αποθήκευσης Azure εγγυάται για διάρκεια ζωής και διαθεσιμότητα. 

Όταν δημιουργείτε ένα λογαριασμό του χώρου αποθήκευσης, μπορείτε να επιλέξετε μία από τις παρακάτω επιλογές αναπαραγωγής:  

- [Τοπικά πλεονάζοντα χώρο αποθήκευσης (LRS)](#locally-redundant-storage)
- [Ζώνη πλεονάζοντα χώρο αποθήκευσης (ZRS)](#zone-redundant-storage)
- [Παν πλεονάζοντα χώρο αποθήκευσης (Εξοπλισμό)](#geo-redundant-storage)
- [Πρόσβαση για ανάγνωση παν πλεονάζοντα αποθήκευσης (RA-Εξοπλισμό)](#read-access-geo-redundant-storage)

Πρόσβαση για ανάγνωση παν πλεονάζοντα αποθήκευσης (RA-Εξοπλισμό) είναι η προεπιλεγμένη επιλογή όταν δημιουργείτε ένα νέο λογαριασμό του χώρου αποθήκευσης.

Ο παρακάτω πίνακας παρέχει μια γρήγορη επισκόπηση των διαφορών μεταξύ LRS, ZRS, Εξοπλισμό και RA-Εξοπλισμό, ενώ οι επόμενες ενότητες διεύθυνση κάθε τύπο αναπαραγωγής με περισσότερες λεπτομέρειες.

| Στρατηγική αναπαραγωγής                                                               | LRS | ZRS | ΕΞΟΠΛΙΣΜΌ | RA ΕΞΟΠΛΙΣΜΌ |
|:----------------------------------------------------------------------------------|:---|:---|:---|:------|
| Αναπαραγωγή δεδομένων σε πολλά κέντρα δεδομένων.                                     | Όχι  | Ναι | Ναι | Ναι    |
| Δεδομένα μπορεί να διαβαστεί από τη δευτερεύουσα θέση, καθώς και από την κύρια θέση. | Όχι  | Όχι  | Όχι  | Ναι    |
| Αριθμός των αντιγράφων των δεδομένων που διατηρούνται σε ξεχωριστή κόμβους.                             | 3   | 3   | 6   | 6      |

Ανατρέξτε στο θέμα [Τις τιμές του Azure χώρου αποθήκευσης](https://azure.microsoft.com/pricing/details/storage/) για τις πληροφορίες για τις επιλογές διαφορετικό πλεονασμού τιμολόγησης.

>[AZURE.NOTE] Αποθήκευση Premium υποστηρίζει μόνο τοπικά πλεονάζοντα χώρο αποθήκευσης (LRS). Για πληροφορίες σχετικά με το χώρο αποθήκευσης Premium, ανατρέξτε στο θέμα [αποθήκευσης Premium: χώρος αποθήκευσης υψηλών επιδόσεων για φόρτους εργασίας εικονική μηχανή Azure](storage-premium-storage.md).

## <a name="locally-redundant-storage"></a>Τοπικά πλεονάζοντα χώρου αποθήκευσης

Τοπικά πλεονάζοντα χώρο αποθήκευσης (LRS) αναπαράγει τα δεδομένα σας τρεις φορές μέσα σε μια μονάδα κλίμακα χώρου αποθήκευσης που φιλοξενείται σε ένα κέντρο δεδομένων στην περιοχή στην οποία δημιουργήσατε το λογαριασμό χώρου αποθήκευσης. Μια αίτηση εγγραφής με επιτυχία επιστρέφει μόνο μία φορά έχει γραφτεί σε όλα τα τρία αντίγραφα. Αυτά τα τρία αντίγραφα κάθε που βρίσκονται σε ξεχωριστά σφαλμάτων τομείς και αναβάθμισης τομέων μέσα σε ένα χώρο αποθήκευσης κλίμακα μονάδα. 

Μια μονάδα κλίμακα χώρου αποθήκευσης είναι μια συλλογή σχάρες τους κόμβους χώρου αποθήκευσης. Σφάλμα τομέα (FD) είναι μια ομάδα κόμβους που αντιπροσωπεύει μια φυσική μονάδα αποτυχία και μπορούν να θεωρηθούν ως κόμβοι που ανήκουν στην ίδια φυσική rack. Έναν τομέα αναβάθμισης (UD) είναι μια ομάδα κόμβοι που έχουν αναβαθμιστεί μαζί κατά τη διαδικασία μια αναβάθμιση υπηρεσίας (υλοποίηση). Τα τρία αντίγραφα είναι εκτείνεται UDs και FDs μέσα σε μια μονάδα κλίμακα χώρου αποθήκευσης για να βεβαιωθείτε ότι δεδομένων είναι διαθέσιμο, ακόμα και αν αποτυχία υλικού επηρεάζει ένα μόνο rack ή όταν κόμβους αναβαθμίζονται κατά τη διάρκεια μιας προώθησης. 

LRS είναι η επιλογή χαμηλότερο κόστος και προσφέρει τουλάχιστον διάρκεια ζωής σε σύγκριση με άλλες επιλογές. Σε περίπτωση επιπέδου από καταστροφή ένα κέντρο δεδομένων (fire, κατάκλυση κ.λπ.) όλα τα τρία αντίγραφα μπορεί να είναι που έχουν χαθεί ή ανάκτηση. Για να μετριασμός αυτόν τον κίνδυνο παν πλεονάζοντα χώρο αποθήκευσης Εξοπλισμό συνιστάται για τις περισσότερες εφαρμογές.

Τοπικά πλεονάζοντα χώρο αποθήκευσης μπορεί να εξακολουθεί να είναι επιθυμητό σε ορισμένα σενάρια: 

- Παρέχει υψηλότερη μέγιστο εύρος ζώνης του χώρου αποθήκευσης Azure επιλογές αναπαραγωγής.

- Εάν η εφαρμογή σας αποθηκεύονται τα δεδομένα που μπορούν να ανακατασκευαστεί εύκολα, ενδέχεται να μπορείτε να επιλέξετε για LRS.

- Ορισμένες εφαρμογές περιορίζονται αναπαραγωγή δεδομένων μόνο μέσα σε μια χώρα λόγω απαιτήσεις διακυβέρνησης δεδομένων. Μια περιοχή ζεύγη θα μπορούσε να είναι σε άλλη χώρα. ανατρέξτε στο θέμα [Azure περιοχές](https://azure.microsoft.com/regions/) για πληροφορίες σχετικά με την περιοχή ζεύγη.

## <a name="zone-redundant-storage"></a>Ζώνη πλεονάζοντα χώρου αποθήκευσης

Ζώνη πλεονάζοντα χώρο αποθήκευσης (ZRS) αναπαράγει ασύγχρονα τα δεδομένα σας σε κέντρα δεδομένων μέσα σε ένα ή δύο περιοχές, εκτός από την αποθήκευση τρία αντίγραφα παρόμοια με LRS, παρέχοντας έτσι μεγαλύτερη διάρκεια ζωής από LRS. Δεδομένα που είναι αποθηκευμένα στο ZRS είναι διαρκή ακόμα και εάν το πρωτεύον κέντρο δεδομένων είναι διαθέσιμες ή ανάκτηση.
Πρέπει να γνωρίζουν οι πελάτες που σκοπεύετε να χρησιμοποιήσετε ZRS που: 

- ZRS είναι διαθέσιμη μόνο για αντικείμενα BLOB μπλοκ στους λογαριασμούς γενικής χρήσης χώρου αποθήκευσης και υποστηρίζεται μόνο σε εκδόσεις υπηρεσία αποθήκευσης 2014-02-14 και νεότερη έκδοση. 

- Επειδή η ασύγχρονης αναπαραγωγής περιλαμβάνει μια καθυστέρηση, σε περίπτωση μιας τοπικής καταστροφής είναι πιθανό ότι οι αλλαγές που δεν έχουν ακόμα αναπαραγάγει στο δευτερεύον θα χαθούν εάν είναι δυνατή η ανάκτησή τα δεδομένα από την κύρια.

- Στη ρεπλίκα μπορεί να μην διαθέσιμο έως ότου Microsoft προετοιμάζει ανακατεύθυνσης στο δευτερεύον.

- Λογαριασμοί ZRS δεν μπορεί να μετατραπεί αργότερα να LRS ή Εξοπλισμό. Ομοίως, έναν υπάρχοντα λογαριασμό LRS ή Εξοπλισμό δεν μπορεί να μετατραπεί σε ένα λογαριασμό ZRS.

- Οι λογαριασμοί ZRS δεν διαθέτουν μετρικά ή τη δυνατότητα καταγραφής. 

## <a name="geo-redundant-storage"></a>Παν πλεονάζοντα χώρου αποθήκευσης

Παν πλεονάζοντα χώρο αποθήκευσης (Εξοπλισμό) αναπαράγει τα δεδομένα σας σε μια δευτερεύουσα περιοχή που είναι εκατοντάδες μίλι μακριά από την κύρια περιοχή. Εάν ο λογαριασμός σας χώρο αποθήκευσης έχει ενεργοποιηθεί Εξοπλισμό, στη συνέχεια, τα δεδομένα σας είναι διαρκή ακόμα και στην περίπτωση μια ολοκληρωμένη τοπικές διακοπή ρεύματος ή μια καταστροφή στην οποία η κύρια περιοχή δεν είναι ανακτήσιμα.

Για ένα λογαριασμό χώρου αποθήκευσης με ενεργοποιημένο Εξοπλισμό, μια ενημέρωση πρώτα δεσμεύεται στην κύρια περιοχή, όπου την αναπαραγωγή τρεις φορές. Στη συνέχεια, την ενημέρωση αναπαράγεται ασύγχρονα στην περιοχή δευτερεύουσα, όπου αυτό είναι επίσης αναπαραχθούν τρεις φορές. 

Με Εξοπλισμό τις κύριες και δευτερεύουσες περιοχές Διαχείριση αντίγραφα σε ξεχωριστή σφαλμάτων τομείς και αναβάθμιση τομείς μέσα σε μια μονάδα κλίμακα αποθήκευσης, όπως περιγράφεται με LRS.

Ζητήματα:

- Επειδή η ασύγχρονης αναπαραγωγής περιλαμβάνει μια καθυστέρηση, σε περίπτωση μιας τοπικές καταστροφής είναι πιθανό ότι οι αλλαγές που δεν έχουν ακόμα αναπαραγάγει τη δευτερεύουσα περιοχή θα χαθούν εάν είναι δυνατή η ανάκτησή τα δεδομένα από την κύρια περιοχή.

- Στη ρεπλίκα δεν είναι διαθέσιμη εκτός εάν Microsoft προετοιμάζει ανακατεύθυνσης για τη δευτερεύουσα περιοχή.

- Εάν μια εφαρμογή θέλει να διαβάζετε από τη δευτερεύουσα περιοχή ο χρήστης πρέπει να ενεργοποιείτε RA Εξοπλισμό. 

Όταν δημιουργείτε ένα λογαριασμό του χώρου αποθήκευσης, μπορείτε να επιλέξετε την κύρια περιοχή για το λογαριασμό. Η δευτερεύουσα περιοχή προσδιορίζεται με βάση την κύρια περιοχή και δεν μπορεί να αλλάξει. Ο παρακάτω πίνακας εμφανίζει τα ζεύγη κύριας και δευτερεύουσας περιοχής.

| Κύρια             | Δευτερεύον           |
|---------------------|---------------------|
| Βόρεια κεντρική η.π.α.    | Νότια κεντρικής η.π.α.    |
| Νότια κεντρικής η.π.α.    | Βόρεια κεντρική η.π.α.    |
| Ανατολικής η.π.α.             | Δυτική η.π.α.             |
| Δυτική η.π.α.             | Ανατολικής η.π.α.             |
| ΗΠΑ Ανατολή 2           | Κεντρική η.π.α.          |
| Κεντρική η.π.α.          | ΗΠΑ Ανατολή 2           |
| Βόρεια Ευρώπη        | Δυτική Ευρώπη         |
| Δυτική Ευρώπη         | Βόρεια Ευρώπη        |
| Νότια Ανατολικής Ασίας     | Ανατολικής Ασίας           |
| Ανατολικής Ασίας           | Νότια Ανατολικής Ασίας     |
| Ανατολικής Κίνα          | Βόρεια Κίνα         |
| Βόρεια Κίνα         | Ανατολικής Κίνα          |
| Ιαπωνία Ανατολή          | Δυτική Ιαπωνία          |
| Δυτική Ιαπωνία          | Ιαπωνία Ανατολή          |
| Νότια Βραζιλίας        | Νότια κεντρικής η.π.α.    |
| Ανατολική Αυστραλία      | Αυστραλία νοτιοανατολικής |
| Αυστραλία νοτιοανατολικής | Ανατολική Αυστραλία      |
| Νότια Ινδίας         | Κεντρική Ινδίας       |
| Κεντρική Ινδίας       | Νότια Ινδίας         |
| Iowa Gov η.π.α.         | Βιρτζίνια Gov η.π.α.     |
| Βιρτζίνια Gov η.π.α.     | Iowa Gov η.π.α.         |
| Κεντρική ώρα Καναδά      | Καναδάς Ανατολή          |
| Καναδάς Ανατολή         | Κεντρική ώρα Καναδά      |
| Δυτική ΗΒ             | Νότια ΗΒ            |
| Νότια ΗΒ            | Δυτική ΗΒ             |
| Κεντρική Γερμανίας     | Γερμανία βορειοανατολικά   |
| Γερμανία βορειοανατολικά   | Κεντρική Γερμανίας     |
| 2 Δυτική η.π.α.           | Κεντρική Δυτική η.π.α.     |
| Κεντρική Δυτική η.π.α.     | 2 Δυτική η.π.α.           |

Για ενημερωμένες πληροφορίες σχετικά με τις περιοχές που υποστηρίζονται από το Azure, ανατρέξτε στο θέμα [Azure περιοχές](https://azure.microsoft.com/regions/).
 
## <a name="read-access-geo-redundant-storage"></a>Χώρος αποθήκευσης παν πλεονάζοντα πρόσβαση για ανάγνωση

Πρόσβαση για ανάγνωση παν πλεονάζοντα αποθήκευσης (RA-Εξοπλισμό) Μεγιστοποίηση διαθεσιμότητα για το λογαριασμό σας στο χώρο αποθήκευσης, παρέχοντας πρόσβαση μόνο για ανάγνωση στα δεδομένα στη δευτερεύουσα θέση, εκτός από την αναπαραγωγή σε δύο περιοχές που παρέχεται από Εξοπλισμό. 

Όταν ενεργοποιείτε την πρόσβαση μόνο για ανάγνωση στα δεδομένα σας στη δευτερεύουσα περιοχή, τα δεδομένα σας είναι διαθέσιμη σε ένα δευτερεύον τελικό σημείο, εκτός από το κύριο τελικό σημείο για το λογαριασμό χώρου αποθήκευσης. Το τελικό σημείο δευτερεύοντα είναι παρόμοιο με το πρωτεύον τελικό σημείο, αλλά προσαρτά το επίθημα `–secondary` στο όνομα του λογαριασμού. Για παράδειγμα, εάν είναι το κύριο τελικό σημείο για την υπηρεσία Blob `myaccount.blob.core.windows.net`, τότε είναι το τελικό σημείο δευτερεύοντα `myaccount-secondary.blob.core.windows.net`. Τα πλήκτρα πρόσβασης για το λογαριασμό χώρου αποθήκευσης είναι το ίδιο για τα κύριας και δευτερεύουσας τελικά σημεία.

Ζητήματα:

- Η εφαρμογή σας έχει για τη διαχείριση των τελικού σημείου την αλληλεπίδραση με κατά τη χρήση RA Εξοπλισμό. 

- RA Εξοπλισμό προορίζεται για σκοπούς υψηλής διαθεσιμότητας. Για οδηγίες κλιμάκωση, εξετάστε τα [επιδόσεων λίστα ελέγχου](storage-performance-checklist.md).

## <a name="next-steps"></a>Επόμενα βήματα

- [Τις τιμές Azure χώρου αποθήκευσης](https://azure.microsoft.com/pricing/details/storage/)
- [Σχετικά με τους λογαριασμούς Azure χώρου αποθήκευσης](storage-create-storage-account.md)
- [Κλιμάκωση Azure χώρου αποθήκευσης και τους στόχους επιδόσεων](storage-scalability-targets.md)
- [Επιλογές πλεονασμού αποθήκευσης Microsoft Azure και πρόσβαση για ανάγνωση παν πλεονάζοντα χώρου αποθήκευσης](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [Χαρτί SOSP - Azure χώρου αποθήκευσης: Ιδιαίτερα διαθέσιμη υπηρεσία αποθήκευσης στο Cloud με ισχυρό συνέπειας](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  