<properties
    pageTitle="Μετεγκατάσταση Azure IaaS εικονικές μηχανές από μία περιοχή Azure σε κάποιο άλλο με Επαναφορά τοποθεσίας | Microsoft Azure"
    description="Χρησιμοποιήστε Azure Επαναφορά τοποθεσίας για τη μετεγκατάσταση Azure IaaS εικονικές μηχανές από μία περιοχή Azure σε μια άλλη."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="raynew"/>

#  <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Μετεγκατάσταση Azure IaaS εικονικές μηχανές μεταξύ Azure περιοχές με Επαναφορά τοποθεσίας Azure

## <a name="overview"></a>Επισκόπηση

Καλώς ορίσατε στο Επαναφορά τοποθεσίας Azure! Χρησιμοποιήστε αυτό το άρθρο, εάν θέλετε να μετεγκαταστήσετε ΣΠΣ Azure μεταξύ Azure περιοχές. Πριν ξεκινήσετε, λάβετε υπόψη ότι:

- Azure έχει δύο διαφορετικές ανάπτυξης μοντέλα για τη δημιουργία και εργασία με πόρους: Διαχείριση πόρων Azure και κλασική. Azure έχει επίσης δύο πύλες – Azure κλασική πύλης που υποστηρίζει το μοντέλο κλασική ανάπτυξης και το Azure πύλης με την υποστήριξη για δύο μοντέλα ανάπτυξης. Τα βασικά βήματα για τη μετεγκατάσταση είναι η ίδια είτε διαμορφώνετε τις παραμέτρους Επαναφορά τοποθεσίας στη Διαχείριση πόρων ή στην κλασική. Ωστόσο, το περιβάλλον εργασίας Χρήστη οδηγίες και στιγμιότυπα οθόνης σε αυτό το άρθρο είναι σχετικά με την πύλη του Azure.
- **Αυτήν τη στιγμή μπορείτε μόνο να μετεγκαταστήσετε από μία περιοχή σε ένα άλλο. Μπορεί να αποτύχει μέσω ΣΠΣ από μία περιοχή Azure σε μια άλλη, αλλά που δεν μπορεί να αποτύχει τους πίσω ξανά.**
- Η μετεγκατάσταση οδηγίες σε αυτό το άρθρο είναι με βάση τις οδηγίες για την αναπαραγωγή μιας φυσικής μηχανής να Azure. Περιλαμβάνει συνδέσεις για τα βήματα που περιγράφονται σε [ΣΠΣ VMware αναπαραγωγή ή φυσικής διακομιστές Azure](site-recovery-vmware-to-azure.md), που περιγράφει τον τρόπο για να αναπαραγάγετε μια φυσική διακομιστή στην πύλη του Azure.
- Εάν ρυθμίζετε Επαναφορά τοποθεσίας στην πύλη του κλασική, ακολουθήστε τις λεπτομερείς οδηγίες σε [αυτό το άρθρο](site-recovery-vmware-to-azure-classic.md). **Δεν είναι πλέον πρέπει να χρησιμοποιήσετε** τις οδηγίες σε αυτό το [άρθρο παλαιού τύπου](site-recovery-vmware-to-azure-classic-legacy.md).

Δημοσίευση σχόλια ή ερωτήσεις στο κάτω μέρος αυτού του άρθρου ή στο [Φόρουμ υπηρεσίες ανάκτησης Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Ακολουθεί ό, τι χρειάζεστε για αυτήν την ανάπτυξη:

- **Ρύθμιση παραμέτρων διακομιστή**: μια Εικονική εσωτερικής εγκατάστασης με Windows Server 2012 R2 που λειτουργεί ως διακομιστής ρύθμισης παραμέτρων. Εγκαταστήσετε τα άλλα στοιχεία τοποθεσίας αποκατάστασης (όπως η διαδικασία διακομιστή και διακομιστή κύρια προορισμού) σε αυτό Εικονική πολύ. Διαβάστε περισσότερα στο [σενάριο αρχιτεκτονική](site-recovery-vmware-to-azure.md#scenario-architecture) και [τις προϋποθέσεις διακομιστή ρύθμισης παραμέτρων](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **IaaS εικονικές μηχανές**: Το ΣΠΣ που θέλετε να μετεγκατασταθούν. Μπορείτε να μετεγκαταστήσετε αυτά τα ΣΠΣ, θεωρώντας τους ως φυσικής μηχανές.

## <a name="deployment-steps"></a>Βήματα ανάπτυξης

Αυτή η ενότητα περιγράφει τα βήματα ανάπτυξης στην πύλη του νέου Azure. Εάν χρειάζεστε αυτά τα βήματα ανάπτυξης για επαναφορά τοποθεσίας στην πύλη του κλασική, ανατρέξτε [σε αυτό το άρθρο](site-recovery-vmware-to-azure-classic.md).

1. [Δημιουργία μιας θάλαμο](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Ανάπτυξη διακομιστή ρύθμισης παραμέτρων](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Αφού έχετε αναπτύξει το διακομιστή ρύθμισης παραμέτρων, ελέγξτε ότι μπορεί να επικοινωνήσει με το ΣΠΣ που θέλετε να μετεγκαταστήσετε.
4. [Ρύθμιση ρυθμίσεων αναπαραγωγής](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Δημιουργία πολιτικής αναπαραγωγής και αντιστοιχίστε στο διακομιστή ρύθμισης παραμέτρων.
5. [Εγκατάσταση της υπηρεσίας φορητότητα](site-recovery-vmware-to-azure.md#step-6-replication-application). Κάθε Εικονική που επιθυμείτε να προστατεύσετε ανάγκες εγκατεστημένη την υπηρεσία φορητότητα. Αυτή η υπηρεσία στέλνει δεδομένα στο διακομιστή διαδικασία. Την υπηρεσία φορητότητα μπορεί να είναι εγκατεστημένο με μη αυτόματο τρόπο ή να προωθηθεί και να εγκατασταθεί αυτόματα από το διακομιστή διαδικασία όταν είναι ενεργοποιημένη η προστασία για την εικονική Μηχανή. Κανόνες τείχους προστασίας για το ΣΠΣ που θέλετε να μετεγκαταστήσετε θα πρέπει να ρυθμιστεί ώστε να επιτρέπεται η εγκατάσταση push αυτής της υπηρεσίας.
6. [Ενεργοποίηση της αναπαραγωγής](site-recovery-vmware-to-azure.md#enable-replication). Ενεργοποίηση αναπαραγωγής για το ΣΠΣ που θέλετε να μετεγκατασταθούν. Μπορείτε να ανακαλύψετε τις εικονικές μηχανές IaaS που θέλετε να μετεγκαταστήσετε στο Azure χρησιμοποιώντας την ιδιωτική διεύθυνση IP από τις εικονικές μηχανές. Βρείτε αυτήν τη διεύθυνση στον πίνακα εργαλείων εικονική μηχανή στο Azure. Όταν ενεργοποιείτε την αναπαραγωγή, ορίστε τον τύπο του υπολογιστή για του ΣΠΣ ως φυσικής μηχανές.
7. [Εκτελέστε μια μη προγραμματισμένη ανακατεύθυνσης](site-recovery-failover.md#run-an-unplanned-failover). Αφού ολοκληρωθεί η αρχική αναπαραγωγής, μπορείτε να εκτελέσετε μια μη προγραμματισμένη ανακατεύθυνσης από μία περιοχή Azure σε κάποιον άλλο. Προαιρετικά, μπορείτε να δημιουργήσετε ένα σχέδιο αποκατάστασης και να εκτελέσετε μια μη προγραμματισμένη ανακατεύθυνσης, για να μετεγκαταστήσετε πολλές εικονικές μηχανές μεταξύ περιοχών. [Μάθετε περισσότερα](site-recovery-create-recovery-plans.md) σχετικά με σχέδια ανάκτησης.

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με τα άλλα σενάρια αναπαραγωγής στο [Τι είναι η Επαναφορά τοποθεσίας Azure;](site-recovery-overview.md)
