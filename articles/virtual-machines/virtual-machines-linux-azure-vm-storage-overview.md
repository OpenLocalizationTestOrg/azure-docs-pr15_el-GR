<properties
  pageTitle="Azure και αποθήκευσης Εικονική Linux | Microsoft Azure"
  description="Περιγράφει τυπική Azure και Premium χώρου αποθήκευσης με Linux εικονικές μηχανές."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines-linux"
  authors="vlivech"
  manager="timlt"
  editor=""/>

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="NA"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure"
  ms.date="10/04/2016"
  ms.author="v-livech"/>

# <a name="azure-and-linux-vm-storage"></a>Azure και Εικονική Linux χώρου αποθήκευσης

Azure χώρου αποθήκευσης είναι η λύση χώρο αποθήκευσης στο cloud για μοντέρνα εφαρμογές που βασίζονται σε διάρκεια ζωής, διαθεσιμότητα και κλιμάκωση για τις ανάγκες των πελατών τους.  Εκτός από τη δυνατότητα στους προγραμματιστές για να δημιουργήσετε εφαρμογές ευρείας κλίμακας για την υποστήριξη νέα σενάρια, αποθήκευσης Azure παρέχει επίσης η βάση χώρου αποθήκευσης για εικονικές μηχανές Windows Azure.

## <a name="azure-storage-standard-and-premium"></a>Azure αποθήκευσης: Τυπικές και Premium

Azure Εικονική μπορεί να δημιουργηθεί κατά την τυπική αποθήκευσης δίσκων ή δίσκων αποθήκευσης premium.  Όταν χρησιμοποιείτε την πύλη για να επιλέξετε το Εικονική πρέπει να μπορείτε να χρησιμοποιήσετε μια αναπτυσσόμενη λίστα στην οθόνη βασικά στοιχεία για να προβάλετε και της τυπικής και premium δίσκων.  Το παρακάτω στιγμιότυπο οθόνης επισημαίνει το μενού εναλλαγής.  Κατά την εναλλαγή σε SSD, μόνο premium χώρου αποθήκευσης θα εμφανίζεται με δυνατότητα ΣΠΣ, όλα αντίγραφα ασφαλείας των SSD μονάδες.  Κατά την εναλλαγή σε σκληρό ΔΊΣΚΟ, τυπική χώρου αποθήκευσης με δυνατότητα ΣΠΣ μονάδων δίσκου αντίγραφα ασφαλείας περιστρεφόμενο θα εμφανίζεται, μαζί με το χώρο αποθήκευσης premium ΣΠΣ αντίγραφα ασφαλείας των SSD.

  ![screen1](../virtual-machines/media/virtual-machines-linux-azure-vm-storage-overview/screen1.png)

Όταν δημιουργείτε μια Εικονική από το `azure-cli` μπορείτε να επιλέξετε ανάμεσα σε τυπική και premium κατά την επιλογή του μεγέθους Εικονική μέσω του `-z` ή `--vm-size` cli σημαία.

### <a name="create-a-vm-with-standard-storage-vm-on-the-cli"></a>Δημιουργήστε μια Εικονική με τυπική χώρο αποθήκευσης Εικονική στη το cli

Η σημαία cli `-z` επιλέγει Standard_A1 με A1 είναι μια τυπική χώρου αποθήκευσης που βασίζεται Εικονική Linux.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_A1
```

### <a name="create-a-vm-with-premium-storage-on-the-cli"></a>Δημιουργήστε μια Εικονική με premium χώρο αποθήκευσης στη το cli

Η σημαία cli `-z` επιλέγει Standard_DS1 με DS1 που έχει τεθεί σε ένα χώρο αποθήκευσης premium βάσει Εικονική Linux.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_DS1
```

## <a name="standard-storage"></a>Τυπική χώρου αποθήκευσης

Azure τυπική χώρου αποθήκευσης είναι ο προεπιλεγμένος τύπος χώρου αποθήκευσης.  Τυπική χώρου αποθήκευσης είναι κόστους αποτελεσματική ενώ ταυτόχρονα performant.  

## <a name="premium-storage"></a>Χώρος αποθήκευσης Premium

Azure Premium αποθήκευσης παρέχει υποστήριξη υψηλών επιδόσεων, χαμηλής αδράνειας δισκέτα για εικονικές μηχανές εκτελούνται φόρτους εργασίας να/O-εντατική. Εικονική μηχανή (Εικονική) δίσκων που χρησιμοποιούν το χώρο αποθήκευσης Premium αποθήκευση δεδομένων στις μονάδες δίσκων συμπαγές κατάστασης (SSD). Μπορείτε να μετεγκαταστήσετε δίσκων Εικονική της εφαρμογής σας με το χώρο αποθήκευσης Premium Azure για να επωφεληθείτε από την ταχύτητα και τις επιδόσεις του αυτών των δίσκων.

Κορυφαίες δυνατότητες του χώρου αποθήκευσης:

- Δίσκων αποθήκευσης Premium: Azure Premium αποθήκευσης υποστηρίζει Εικονική δίσκων που μπορεί να συνδεθεί σε σειρά DS, DSv2 ή GS ΣΠΣ Azure.

- Σελίδα Blob Premium: Αποθήκευση Premium υποστηρίζει αντικείμενα BLOB σελίδα Azure, η οποία χρησιμοποιείται για την αναμονή μόνιμη δίσκων για εικονικές μηχανές Windows Azure (ΣΠΣ).

- Premium τοπικά πλεονάζοντα αποθήκευσης: Ένα λογαριασμό του χώρου αποθήκευσης Premium μόνο υποστηρίζει τοπικά πλεονάζοντα χώρο αποθήκευσης (LRS) με την επιλογή αναπαραγωγή και διατηρεί τρία αντίγραφα των δεδομένων μέσα σε μία μόνο περιοχή.

- [Χώρος αποθήκευσης Premium](../storage/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Χώρος αποθήκευσης Premium υποστηρίζονται ΣΠΣ

Αποθήκευση Premium υποστηρίζει σειράς DS, DSv2-σειρά, GS σειρά και Fs σειρά εικονικές μηχανές Windows Azure (ΣΠΣ). Μπορείτε να χρησιμοποιήσετε τυπικές και Premium δίσκων χώρου αποθήκευσης με Premium αποθήκευσης που υποστηρίζεται ΣΠΣ. Αλλά δεν μπορείτε να χρησιμοποιήσετε δίσκων Premium χώρου αποθήκευσης με Εικονική σειράς που δεν είναι συμβατά αποθήκευσης Premium.

Ακολουθούν κατανομές Linux που θα σας επικυρωθεί με αποθήκευσης Premium.

| Κατανομή | Έκδοση                 | Υποστηριζόμενες πυρήνα    |
|--------------|-------------------------|---------------------|
| Ubuntu       | 12.04                   | 3.2.0-75.110+       |
| Ubuntu       | 14.04                   | 3.13.0-44.73+       |
| Debian       | 7.x, 8.x                | 3.16.7-ckt4-1+      |
| SLES         | SLES 12                 | 3.12.36-38.1+       |
| SLES         | SLES 11 SP4             | 3.0.101-0.63.1+     |
| CoreOS       | 584.0.0+                | 3.18.4+             |
| Centos       | 6.5, 6.6, 6,7, 7.0, 7.1 | 3.10.0-229.1.2.el7+ |
| RHEL         | 6.8 +, 7.2 +              |                     |


## <a name="file-storage"></a>Αποθήκευση αρχείων

Azure χώρο αποθήκευσης αρχείων προσφέρει κοινόχρηστα στοιχεία αρχείων στο cloud χρησιμοποιώντας το τυπικό πρωτόκολλο Ερωτήσεων. Azure αρχεία, μπορείτε να μετεγκαταστήσετε εταιρικές εφαρμογές που βασίζονται σε διακομιστές αρχείων για να Azure. Εφαρμογές που εκτελούνται στο Azure εύκολα να ενεργοποίησης κοινόχρηστα στοιχεία αρχείων από Azure εικονικές μηχανές εκτελούνται Linux. Και με την πιο πρόσφατη έκδοση του χώρου αποθήκευσης αρχείων, μπορείτε επίσης να τοποθετήσετε ένα κοινόχρηστο αρχείο από μια εφαρμογή εσωτερικής εγκατάστασης που υποστηρίζει 3.0 Ερωτήσεων.  Επειδή τα κοινόχρηστα στοιχεία αρχείων Ερωτήσεων κοινόχρηστα στοιχεία, μπορείτε να αποκτήσετε πρόσβαση σε αυτά μέσω τυπικό αρχείο API του συστήματος.

Αποθήκευση αρχείων είναι ενσωματωμένη στην ίδια τεχνολογία ως χώρο αποθήκευσης αντικειμένων Blob, πίνακα και ουρά, ώστε να προσφέρει χώρο αποθήκευσης αρχείων τη διαθεσιμότητα, διάρκεια ζωής, κλιμάκωση και παν πλεονασμού που είναι ενσωματωμένο στο της πλατφόρμας Azure χώρου αποθήκευσης. Για λεπτομέρειες σχετικά με τους στόχους επιδόσεων χώρου αποθήκευσης αρχείων και τα όρια, ανατρέξτε στο θέμα Azure κλιμάκωση χώρου αποθήκευσης και τους στόχους επιδόσεων.

- [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αρχείων Azure με Linux](../storage/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Συντόμευσης χώρου αποθήκευσης

Η σειρά Azure συντόμευσης χώρου αποθήκευσης είναι βελτιστοποιημένη για την αποθήκευση των δεδομένων που είναι δυνατή η πρόσβαση στις συχνά.  Συντόμευσης χώρου αποθήκευσης είναι ο προεπιλεγμένος τύπος χώρου αποθήκευσης για χώρους αποθήκευσης αντικειμένων blob.

## <a name="cool-storage"></a>Εντυπωσιακές χώρου αποθήκευσης

Η σειρά Azure εντυπωσιακές χώρου αποθήκευσης είναι βελτιστοποιημένη για την αποθήκευση των δεδομένων που είναι συχνά πρόσβαση και μεγάλης διάρκειας. Παράδειγμα περιπτώσεις χρήσης για εντυπωσιακές χώρο αποθήκευσης περιλαμβάνουν αντίγραφα ασφαλείας, περιεχομένου πολυμέσων, επιστημονική δεδομένων, συμμόρφωση και αρχειοθέτησης δεδομένων. Σε γενικές γραμμές, τα δεδομένα που είναι προσβάσιμη σπάνια είναι ιδανική υποψήφια για εντυπωσιακές χώρου αποθήκευσης.

|                             | Επίπεδο συντόμευσης χώρου αποθήκευσης      | Επίπεδο εντυπωσιακές χώρου αποθήκευσης     |
|:----------------------------|:---------------------:|:---------------------:|
| Διαθεσιμότητα                | 99,9%                 | 99%                   |
| Διαθεσιμότητα (RA Εξοπλισμό διαβάζει) | 99,99%                | 99,9%                 |
| Χρεώσεις χρήσης               | Αύξηση του κόστους χώρου αποθήκευσης  | Χαμηλότερο κόστος χώρου αποθήκευσης   |
|                             | Κάτω πρόσβασης          | Υψηλότερη πρόσβασης         |
|                             | και το κόστος συναλλαγής | και το κόστος συναλλαγής |


## <a name="redundancy"></a>Πλεονασμού

Τα δεδομένα στο χώρο αποθήκευσης στο λογαριασμό σας Microsoft Azure αναπαράγεται πάντα για να βεβαιωθείτε ότι διάρκεια ζωής και υψηλή διαθεσιμότητα, το SLA αποθήκευσης Azure ακόμα λόγω αποτυχίες μεταβατικές υλικού της σύσκεψης.

Όταν δημιουργείτε ένα λογαριασμό του χώρου αποθήκευσης, πρέπει να επιλέξετε μία από τις παρακάτω επιλογές αναπαραγωγής:

- Τοπικά πλεονάζοντα χώρο αποθήκευσης (LRS)
- Ζώνη πλεονάζοντα χώρο αποθήκευσης (ZRS)
- Παν πλεονάζοντα χώρο αποθήκευσης (Εξοπλισμό)
- Πρόσβαση για ανάγνωση παν πλεονάζοντα αποθήκευσης (RA-Εξοπλισμό)

### <a name="locally-redundant-storage"></a>Τοπικά πλεονάζοντα χώρου αποθήκευσης

Τοπικά πλεονάζοντα χώρο αποθήκευσης (LRS) αναπαράγει τα δεδομένα σας στο εσωτερικό της περιοχής στο οποίο δημιουργήσατε το λογαριασμό χώρου αποθήκευσης. Για να μεγιστοποιήσετε διάρκεια ζωής, κάθε αίτηση που υποβάλλεται σε σχέση με δεδομένα στο λογαριασμό σας στο χώρο αποθήκευσης αναπαράγεται τρεις φορές. Αυτά τα τρία αντίγραφα κάθε που βρίσκονται σε ξεχωριστά σφαλμάτων τομέων και αναβάθμισης τομέων.  Μια αίτηση με επιτυχία επιστρέφει μόνο μία φορά έχει γραφτεί σε όλα τα τρία αντίγραφα.

### <a name="zone-redundant-storage"></a>Ζώνη πλεονάζοντα χώρου αποθήκευσης

Ζώνη πλεονάζοντα χώρο αποθήκευσης (ZRS) αναπαράγει τα δεδομένα σας σε δύο ή τρεις εγκαταστάσεις, είτε σε μία μόνο περιοχή ή σε δύο περιοχές, παρέχοντας μεγαλύτερη διάρκεια ζωής από LRS. Εάν ο λογαριασμός χώρου αποθήκευσης έχει ενεργοποιηθεί ZRS, στη συνέχεια, τα δεδομένα σας είναι διαρκή ακόμα και στην περίπτωση αποτυχίας σε μία από τις εγκαταστάσεις.

### <a name="geo-redundant-storage"></a>Παν πλεονάζοντα χώρου αποθήκευσης

Παν πλεονάζοντα χώρο αποθήκευσης (Εξοπλισμό) αναπαράγει τα δεδομένα σας σε μια δευτερεύουσα περιοχή που είναι εκατοντάδες μίλι μακριά από την κύρια περιοχή. Εάν ο λογαριασμός σας χώρο αποθήκευσης έχει ενεργοποιηθεί Εξοπλισμό, στη συνέχεια, τα δεδομένα σας είναι διαρκή ακόμα και στην περίπτωση μια ολοκληρωμένη τοπικές διακοπή ρεύματος ή μια καταστροφή στην οποία η κύρια περιοχή δεν είναι ανακτήσιμα.

### <a name="read-access-geo-redundant-storage"></a>Χώρος αποθήκευσης παν πλεονάζοντα πρόσβαση για ανάγνωση

Πρόσβαση για ανάγνωση παν πλεονάζοντα αποθήκευσης (RA-Εξοπλισμό) Μεγιστοποίηση διαθεσιμότητα για το λογαριασμό σας στο χώρο αποθήκευσης, παρέχοντας πρόσβαση μόνο για ανάγνωση στα δεδομένα στη δευτερεύουσα θέση, εκτός από την αναπαραγωγή σε δύο περιοχές που παρέχεται από Εξοπλισμό. Σε περίπτωση που δεδομένων δεν είναι διαθέσιμη στην κύρια περιοχή, η εφαρμογή σας μπορεί να διαβάζει δεδομένα από τη δευτερεύουσα περιοχή.

Για βαθύ κατάρρευση σε Azure αποθήκευσης πλεονασμού ανατρέξτε στο θέμα:

- [Azure αναπαραγωγής χώρου αποθήκευσης](../storage/storage-redundancy.md)

## <a name="scalability"></a>Κλιμάκωση

Azure χώρου αποθήκευσης είναι μαζικά με δυνατότητα κλιμάκωσης, ώστε να μπορείτε να αποθηκεύσετε και να απαιτούνται διαδικασία εκατοντάδες των terabyte δεδομένων για την υποστήριξη τα σενάρια μεγάλο δεδομένων από επιστημονική, οικονομική ανάλυση και εφαρμογές πολυμέσων. Ή μπορείτε να αποθηκεύσετε τα μικρά βήματα δεδομένων που απαιτούνται για μια τοποθεσία Web για μικρές επιχειρήσεις. Όπου και αν κυμαίνεται τις ανάγκες σας, μπορείτε να πληρώσετε μόνο για τα δεδομένα που είναι αποθηκευμένο. Azure αποθήκευσης αυτήν τη στιγμή αποθηκεύει δεκάδες trillions αντικειμένων μοναδικό πελατών και χειρισμού εκατομμύρια αιτήσεις ανά δευτερόλεπτο κατά μέσο όρο.

Για λογαριασμούς τυπική χώρου αποθήκευσης: μια τυπική αποθήκευσης λογαριασμός έχει μέγιστο αίτημα συνολικό ποσοστό 20.000 IOP Προέλευσης. Το συνολικό IOP Προέλευσης κατά μήκος όλων των δίσκων σας εικονική μηχανή σε λογαριασμό τυπική χώρου αποθήκευσης δεν πρέπει να υπερβαίνει αυτό το όριο.

Για λογαριασμούς του χώρου αποθήκευσης premium: μέγιστη ταχύτητα μετάδοσης συνολικό ποσοστό 50 Gbps περιλαμβάνει ένα άρτιο χώρου αποθήκευσης. Ο συνολικός αριθμός μεταξύ όλων των δίσκων σας Εικονική δεν πρέπει να υπερβαίνει αυτό το όριο.

## <a name="availability"></a>Διαθεσιμότητα

Θα σας εγγυάται ότι τουλάχιστον 99,99% (99,9% για εντυπωσιακές επίπεδο πρόσβασης) του χρόνου, θα με επιτυχία επεξεργαστούμε αιτήσεις ανάγνωσης δεδομένων από λογαριασμούς πρόσβαση για ανάγνωση-παν πλεονάζοντα αποθήκευσης (RA-Εξοπλισμό), υπό την προϋπόθεση ότι θα επαναληφθεί αποτυχημένων προσπαθειών ανάγνωσης δεδομένων από την κύρια περιοχή στο τη δευτερεύουσα περιοχή.

Θα σας εγγυάται ότι τουλάχιστον 99,9% (99% για εντυπωσιακές επίπεδο πρόσβασης) του χρόνου, θα με επιτυχία επεξεργαστούμε αιτήσεις για την ανάγνωση δεδομένων από τοπικά πλεονάζοντα χώρο αποθήκευσης (LRS), ζώνη πλεονάζοντα χώρο αποθήκευσης (ZRS) και λογαριασμούς παν πλεονάζοντα χώρο αποθήκευσης Εξοπλισμό.

Θα σας εγγυάται ότι τουλάχιστον 99,9% (99% για εντυπωσιακές επίπεδο πρόσβασης) του χρόνου, θα με επιτυχία επεξεργαστούμε αιτήσεις για την εγγραφή δεδομένων τοπικά πλεονάζοντα χώρο αποθήκευσης (LRS), ζώνη πλεονάζοντα χώρο αποθήκευσης (ZRS), και λογαριασμούς παν πλεονάζοντα χώρο αποθήκευσης Εξοπλισμό και πρόσβαση για ανάγνωση-παν πλεονάζοντα αποθήκευσης (RA-Εξοπλισμό).

- [Azure SLA για χώρο αποθήκευσης](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)


## <a name="regions"></a>Περιοχές

Azure είναι γενικά διαθέσιμο σε 30 περιοχές του κόσμου και έχει ανακοινώσει σχέδια για 4 επιπλέον περιοχές. Γεωγραφικό επέκτασης είναι μια προτεραιότητα Azure επειδή δίνει τη δυνατότητα οι πελάτες μας για την επίτευξη υψηλότερες επιδόσεις και την υποστήριξη τους απαιτήσεις και προτιμήσεις που αφορούν θέση δεδομένων.  Τελευταία περιοχή azures εκκίνηση είναι στη Γερμανία.

Η Microsoft Cloud Γερμανίας παρέχει μια επιλογή διαφοροποιημένης με τις υπηρεσίες Microsoft Cloud ήδη διαθέσιμα σε όλη την Ευρώπη, τη δημιουργία αυξημένη ευκαιρίες καινοτομιών και οικονομικής ανάπτυξης για ιδιαίτερα οργανωμένη συνεργάτες και πελάτες στη Γερμανία, της Ευρωπαϊκής Ένωσης (ΕΕ) και το Ευρωπαϊκής δωρεάν συναλλαγών (ΕΖΕΣ).

Θα γίνεται η Διαχείριση πελατών δεδομένων σε αυτές τις νέες κέντρα δεδομένων, σε Magdeburg και Φραγκφούρτη, κάτω από το στοιχείο ελέγχου έναν επιμελητή δεδομένων, διεθνή συστήματα T, μια ανεξάρτητη γερμανικά εταιρεία και θυγατρική της Telekom γερμανικά. Υπηρεσίες τυπογραφικής cloud της Microsoft στο αυτά τα κέντρα δεδομένων συμμορφώνονται με Γερμανικά δεδομένων χειρισμός κανονισμούς και δώστε τους πελάτες πρόσθετες επιλογές με τον τρόπο και το σημείο όπου υποβάλλεται σε επεξεργασία δεδομένων.


- [Azure περιοχές χάρτη](https://azure.microsoft.com/regions/)

## <a name="security"></a>Ασφάλεια

Azure αποθήκευσης παρέχει ένα ολοκληρωμένο σύνολο δυνατοτήτων ασφαλείας που επιτρέπουν μαζί τους προγραμματιστές για τη δημιουργία ασφαλούς εφαρμογές. Μπορείτε να προστατεύσετε τα ίδιο το λογαριασμό χώρου αποθήκευσης με χρήση του ελέγχου πρόσβασης βάσει ρόλων και Azure Active Directory. Μπορείτε να προστατεύσετε τα δεδομένα κατά τη μεταφορά ανάμεσα σε μια εφαρμογή και Azure χρησιμοποιώντας κρυπτογράφηση πλευρά του προγράμματος-πελάτη, HTTPS ή 3.0 Ερωτήσεων. Δεδομένα μπορούν να λάβουν αυτόματα είναι κρυπτογραφημένα κατά την εγγραφή στο χώρο αποθήκευσης Azure χρησιμοποιώντας κρυπτογράφηση υπηρεσία αποθήκευσης (SSE). Μπορεί να οριστεί δίσκων λειτουργικό σύστημα και τα δεδομένα που χρησιμοποιούνται από εικονικές μηχανές κρυπτογράφησης χρησιμοποιεί κρυπτογράφηση δίσκου Azure. Ανάθεση πρόσβαση στα αντικείμενα δεδομένων στο χώρο αποθήκευσης Azure μπορούν να λάβουν χρήση υπογραφών πρόσβασης σε κοινή χρήση.

### <a name="management-plane-security"></a>Ασφάλεια επιπέδου διαχείρισης

Το επίπεδο διαχείρισης αποτελείται από τους πόρους που χρησιμοποιούνται για να διαχειριστείτε το λογαριασμό χώρου αποθήκευσης. Σε αυτήν την ενότητα, θα αναφερθούμε το μοντέλο ανάπτυξης διαχείρισης πόρων Azure και πώς μπορείτε να χρησιμοποιήσετε έλεγχο πρόσβασης βάσει ρόλων (RBAC) για να ελέγχετε την πρόσβαση σε λογαριασμούς χώρου αποθήκευσης. Επίσης, θα σας θα μιλήσετε σχετικά με τη διαχείριση των αριθμών-κλειδιών λογαριασμού χώρου αποθήκευσης και πώς μπορείτε να αναδημιουργήσετε τις.

### <a name="data-plane-security"></a>Ασφάλεια επιπέδου δεδομένων

Σε αυτήν την ενότητα, θα εξετάσουμε παρέχει πρόσβαση στα αντικείμενα πραγματικά δεδομένα σε λογαριασμού χώρου αποθήκευσης, όπως αντικείμενα blob, αρχεία, ουρές και πίνακες, χρήση υπογραφών πρόσβασης σε κοινή χρήση και είναι αποθηκευμένο πολιτικές πρόσβασης. Θα ασχοληθούμε συσχετισμών Ασφαλείας συστοιχίας και συσχετισμούς Ασφαλείας επίπεδο λογαριασμού. Επίσης, θα σας θα δείτε πώς μπορείτε να περιορίσετε την πρόσβαση σε μια συγκεκριμένη διεύθυνση IP (ή περιοχή διευθύνσεων IP), πώς να περιορίσετε το πρωτόκολλο που χρησιμοποιείται σε HTTPS και πώς μπορείτε να ανακαλέσετε μια υπογραφή πρόσβασης σε κοινή χρήση χωρίς να περιμένουν για να λήξει.

## <a name="encryption-in-transit"></a>Κρυπτογράφηση κατά τη μεταφορά

Αυτή η ενότητα περιγράφει τον τρόπο για την ασφάλιση δεδομένων κατά τη μεταφορά μέσα ή έξω από το χώρο αποθήκευσης Azure. Θα αναφερθούμε την προτεινόμενη χρήση HTTPS και την κρυπτογράφηση που χρησιμοποιείται από το 3.0 Ερωτήσεων για Azure κοινόχρηστα στοιχεία αρχείων. Επίσης, θα σας θα Ρίξτε μια ματιά κρυπτογράφηση πλευρά του προγράμματος-πελάτη, η οποία σας επιτρέπει να κρυπτογραφεί τα δεδομένα πριν να μεταφερθούν στο χώρο αποθήκευσης σε μια εφαρμογή προγράμματος-πελάτη και να αποκρυπτογραφήσετε τα δεδομένα αφού μεταφέρεται από το χώρο αποθήκευσης.

## <a name="encryption-at-rest"></a>Κρυπτογράφηση στο υπόλοιπο

Θα αναφερθούμε κρυπτογράφησης υπηρεσία αποθήκευσης (SSE) και πώς μπορείτε να ενεργοποιήσετε για ένα λογαριασμό χώρου αποθήκευσης, σας μπλοκ αντικείμενα blob, αντικείμενα BLOB σελίδας, με αποτέλεσμα και να προσαρτήσετε αντικείμενα BLOB που κρυπτογραφούνται αυτόματα κατά την εγγραφή στο χώρο αποθήκευσης Azure. Θα επίσης εξετάσουμε πώς μπορείτε να χρησιμοποιήσετε κρυπτογράφηση δίσκου Azure και να εξερευνήσετε το βασικές διαφορές και τα κουτιά κρυπτογράφησης δίσκου έναντι SSE έναντι της κρυπτογράφησης πλευρά του προγράμματος-πελάτη. Θα περιγράφει συνοπτικά εξετάσουμε συμμόρφωση FIPS για υπολογιστές κυβέρνησης των ΗΠΑ.

- [Οδηγός ασφάλειας Azure χώρου αποθήκευσης](../storage/storage-security-guide.md)

## <a name="cost-savings"></a>Εξοικονόμηση κόστους

- [Κόστος χώρου αποθήκευσης](https://azure.microsoft.com/pricing/details/storage/)

- [Χώρος αποθήκευσης κόστους Αριθμομηχανή](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Όρια χώρου αποθήκευσης

- [Όρια χώρου αποθήκευσης υπηρεσίας](../azure-subscription-service-limits.md#storage-limits)
