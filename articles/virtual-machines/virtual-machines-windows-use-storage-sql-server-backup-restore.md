<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε Azure χώρου αποθήκευσης για SQL Server δημιουργίας αντιγράφων ασφαλείας και επαναφορά | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε αντίγραφα ασφαλείας SQL Server με τον χώρο αποθήκευσης Azure. Εξηγεί τα οφέλη από τη δημιουργία αντιγράφων ασφαλείας βάσεις δεδομένων SQL με το χώρο αποθήκευσης Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="MikeRayMSFT"
    manager="jhubbard"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="mikeray"/>

# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Χρήση του Azure χώρου αποθήκευσης για το αντίγραφο ασφαλείας του SQL Server και επαναφορά

## <a name="overview"></a>Επισκόπηση

Ξεκινώντας με το SQL Server 2012 SP1 CU2, μπορείτε να γράψετε αντίγραφα ασφαλείας SQL Server απευθείας με την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure. Μπορείτε να χρησιμοποιήσετε αυτήν τη λειτουργικότητα για αντίγραφο ασφαλείας και επαναφορά από την υπηρεσία αντικειμένων Blob του Azure με μια βάση δεδομένων SQL Server εσωτερικής εγκατάστασης ή μια βάση δεδομένων SQL Server σε μια εικονική μηχανή Azure. Δημιουργία αντιγράφων ασφαλείας στο cloud προσφέρει πλεονεκτήματα της διαθεσιμότητας, απεριόριστες αναπαραχθούν παν εκτός χώρου αποθήκευσης και διευκόλυνση της μετεγκατάστασης δεδομένων προς και από το cloud. Μπορείτε να εκδώσετε δηλώσεις ή ΕΠΑΝΑΦΟΡΆΣ αντιγράφου ΑΣΦΑΛΕΊΑΣ με τη χρήση Transact-SQL ή SMO.

SQL Server 2016 παρουσιάζει τις νέες δυνατότητες; Μπορείτε να χρησιμοποιήσετε [στιγμιοτύπων αρχείο αντιγράφου ασφαλείας](http://msdn.microsoft.com/library/mt169363.aspx) για να εκτελέσετε σχεδόν άμεση δημιουργία αντιγράφων ασφαλείας και επαναφέρει πολύ γρήγορα.

Αυτό το θέμα εξηγεί γιατί μπορεί να επιλέξετε να χρησιμοποιήσετε Azure χώρου αποθήκευσης για δημιουργία αντιγράφων ασφαλείας SQL και, στη συνέχεια, περιγράφει τα στοιχεία που περιλαμβάνονται. Μπορείτε να χρησιμοποιήσετε τους πόρους που παρέχονται στο τέλος αυτού του άρθρου, για να αποκτήσετε πρόσβαση σε αναλυτικές διαδικασίες και πρόσθετες πληροφορίες για να ξεκινήσετε να χρησιμοποιείτε αυτήν την υπηρεσία με τα αντίγραφα ασφαλείας του SQL Server.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Πλεονεκτήματα της χρήσης της υπηρεσίας αντικειμένων Blob του Azure για δημιουργία αντιγράφων ασφαλείας του SQL Server

Υπάρχουν πολλές προκλήσεις που αντιμετωπίζουν κατά τη δημιουργία αντιγράφων ασφαλείας SQL Server. Αυτές τις προκλήσεις περιλαμβάνουν Διαχείριση χώρου αποθήκευσης, κίνδυνο αποτυχίας χώρου αποθήκευσης, πρόσβαση σε άλλη χώρου αποθήκευσης και ρύθμιση παραμέτρων υλικού. Πολλές από αυτές τις προκλήσεις εξετάζονται, χρησιμοποιώντας την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure για αντίγραφα ασφαλείας SQL Server. Λάβετε υπόψη τα ακόλουθα πλεονεκτήματα:

- **Διευκόλυνση της χρήσης**: αποθήκευση τα αντίγραφα ασφαλείας σε αντικείμενα BLOB Azure μπορεί να είναι ένα εύχρηστο ευέλικτη και εύκολη πρόσβαση σε άλλη επιλογή. Δημιουργία εκτός χώρου αποθήκευσης για τα αντίγραφα ασφαλείας του SQL Server μπορεί να είναι τόσο εύκολη όσο η τροποποίηση υπάρχοντος δέσμες ενεργειών/εργασιών σας για να χρησιμοποιήσετε τη σύνταξη **Αντίγραφα ΑΣΦΑΛΕΊΑΣ σε διεύθυνση URL** . Εκτός χώρου αποθήκευσης συνήθως πρέπει να είναι αρκετά μακριά από τη θέση της βάσης δεδομένων παραγωγής για να αποτρέψετε μια μεμονωμένη καταστροφή που μπορεί να επηρεάσει δύο θέσεις βάσης δεδομένων εκτός και παραγωγής. Επιλέγοντας [αντικείμενα blob του Azure παν αντιγραφή](../storage/storage-redundancy.md), μπορείτε να έχετε ένα επιπλέον επίπεδο προστασίας σε μια καταστροφή που μπορεί να επηρεάσει ολόκληρη την περιοχή.
- **Δημιουργία αντιγράφων ασφαλείας αρχειοθέτησης**: υπηρεσία αποθήκευσης αντικειμένων Blob του Azure προσφέρει καλύτερη εναλλακτική λύση στην επιλογή ταινίας συχνά χρησιμοποιούνται για την αρχειοθέτηση δημιουργίας αντιγράφων ασφαλείας. Αποθήκευσης ταινία ενδέχεται να απαιτείται η φυσική μεταφορών για μια άλλη εγκατάσταση και μετρήσεις για να προστατεύσετε τα πολυμέσα. Αποθήκευση τα αντίγραφα ασφαλείας σε χώρο αποθήκευσης Blob του Azure παρέχει άμεσων μηνυμάτων, ιδιαίτερα διαθέσιμη, και μια διαρκής αρχειοθέτηση επιλογή.
- **Διαχειριζόμενες υλικού**: δεν υπάρχει καμία επιβάρυνσης της διαχείρισης υλικού με τις υπηρεσίες του Azure. Azure υπηρεσίες Διαχείριση το υλικό και δώστε παν αναπαραγωγής για πλεονασμού και προστασία από αποτυχίες υλικού.
- **Απεριόριστος χώρος αποθήκευσης**: με την ενεργοποίηση ενός αντιγράφου ασφαλείας απευθείας σε αντικείμενα BLOB Azure, έχετε πρόσβαση σε σχεδόν απεριόριστο χώρο αποθήκευσης. Εναλλακτικά, τη δημιουργία αντιγράφων ασφαλείας προς τα επάνω έναν δίσκο Azure εικονική μηχανή έχει όρια βάσει μεγέθους υπολογιστή. Υπάρχει όριο στον αριθμό των δίσκων που μπορείτε να επισυνάψετε μια εικονική μηχανή Azure για δημιουργία αντιγράφων ασφαλείας. Αυτό το όριο είναι 16 δίσκων για μια παρουσία πολύ μεγάλα και λιγότερα για μικρότερες παρουσίες.
- **Δημιουργία αντιγράφων ασφαλείας διαθεσιμότητα**: δημιουργία αντιγράφων ασφαλείας που είναι αποθηκευμένα στο Azure αντικείμενα BLOB είναι διαθέσιμα από οπουδήποτε και οποιαδήποτε στιγμή και είναι δυνατή η πρόσβαση εύκολα για επαναφέρει μια SQL Server εσωτερικής εγκατάστασης ή άλλο SQL Server που εκτελείται σε ένα Azure εικονικό υπολογιστή, χωρίς να χρειάζεται βάσης δεδομένων επισύναψη/απόσπαση ή τη λήψη και την επισύναψη VHD.
- **Κόστος**: πληρωμή μόνο για την υπηρεσία που χρησιμοποιείται. Μπορεί να είναι οικονομικός ως επιλογή αρχειοθέτηση εκτός και δημιουργίας αντιγράφων ασφαλείας. Δείτε το [Azure τιμολόγησης Αριθμομηχανής](http://go.microsoft.com/fwlink/?LinkId=277060 "Αριθμομηχανής τις τιμές")και την [Τιμολόγηση Azure άρθρο](http://go.microsoft.com/fwlink/?LinkId=277059 "τιμολόγησης το άρθρο") για περισσότερες πληροφορίες.
- **Στιγμιότυπα χώρου αποθήκευσης**: όταν αρχείων βάσης δεδομένων που είναι αποθηκευμένα σε μια αντικειμένων blob του Azure και χρησιμοποιείτε το SQL Server 2016, μπορείτε να χρησιμοποιήσετε [στιγμιοτύπων αρχείο αντιγράφου ασφαλείας](http://msdn.microsoft.com/library/mt169363.aspx) για να εκτελέσετε σχεδόν άμεση δημιουργία αντιγράφων ασφαλείας και επαναφέρει πολύ γρήγορα.

Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας του SQL Server και επαναφορά με υπηρεσία αποθήκευσης αντικειμένων Blob του Azure](http://go.microsoft.com/fwlink/?LinkId=271617).

Οι παρακάτω ενότητες δύο Παρουσιάστε την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure, όπως τα απαιτούμενα στοιχεία του SQL Server. Είναι σημαντικό να κατανοήσετε τα στοιχεία και τους επικοινωνίας για την επιτυχή χρήσης των αντιγράφων ασφαλείας και επαναφορά από την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure.

## <a name="azure-blob-storage-service-components"></a>Στοιχεία υπηρεσία αποθήκευσης αντικειμένων Blob του Azure

Τα παρακάτω στοιχεία Azure χρησιμοποιούνται κατά τη δημιουργία αντιγράφων ασφαλείας με την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure.

| Το στοιχείο               | Περιγραφή                          |
|---------------------|-------------------------------|
| **Το λογαριασμό χώρου αποθήκευσης** | Ο λογαριασμός χώρου αποθήκευσης είναι το σημείο εκκίνησης για όλες τις υπηρεσίες του χώρου αποθήκευσης. Για να αποκτήσετε πρόσβαση σε μια υπηρεσία αποθήκευσης αντικειμένων Blob Azure, πρέπει πρώτα να δημιουργήσετε ένα λογαριασμό αποθήκευσης Azure. Για περισσότερες πληροφορίες σχετικά με την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure, δείτε [πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Κοντέινερ** | Ένα κοντέινερ παρέχει μια ομαδοποίηση από ένα σύνολο αντικειμένων blob και μπορούν να αποθηκεύουν απεριόριστες αντικείμενα blob. Για να γράψετε SQL Server αντιγράφου ασφαλείας σε μια υπηρεσία αντικειμένων Blob του Azure, πρέπει να έχετε τουλάχιστον το κοντέινερ ρίζας που δημιουργήθηκε. |
| **BLOB** | Ένα αρχείο από οποιονδήποτε τύπο και το μέγεθος. Αντικείμενα BLOB είναι μπορούν να χρησιμοποιηθούν με την εξής μορφή διεύθυνσης URL: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Για περισσότερες πληροφορίες σχετικά με αντικείμενα BLOB σελίδας, ανατρέξτε στο θέμα [Κατανόηση των μπλοκ και αντικείμενα BLOB σελίδας](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>Στοιχεία του SQL Server

Τα παρακάτω στοιχεία του SQL Server χρησιμοποιούνται κατά τη δημιουργία αντιγράφων ασφαλείας με την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure.

| Το στοιχείο               | Περιγραφή                          |
|---------------------|-------------------------------|
| **ΔΙΕΎΘΥΝΣΗ URL** | Μια διεύθυνση URL καθορίζει ένα ενιαίο αναγνωριστικό πόρου (URI) σε ένα μοναδικό αρχείο αντιγράφου ασφαλείας. Η διεύθυνση URL χρησιμοποιείται για την παροχή τη θέση και το όνομα του αρχείου αντιγράφου ασφαλείας του SQL Server. Η διεύθυνση URL πρέπει να οδηγεί σε μια πραγματική blob, όχι μόνο ένα κοντέινερ. Εάν δεν υπάρχει το αντικείμενο blob, δημιουργείται. Εάν μια υπάρχουσα blob καθορίζεται, αποτύχει η δημιουργία αντιγράφων ΑΣΦΑΛΕΊΑΣ, εκτός εάν το > καθορίζεται η επιλογή με ΜΟΡΦΉ. Ακολουθεί ένα παράδειγμα της διεύθυνσης URL που θα ορίσετε στην εντολή Δημιουργία αντιγράφων ΑΣΦΑΛΕΊΑΣ: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS είναι προτεινόμενα, αλλά δεν απαιτείται. |
| **Διαπιστευτήρια** | Οι πληροφορίες που απαιτούνται για να συνδεθείτε και τον έλεγχο ταυτότητας σε υπηρεσία αποθήκευσης αντικειμένων Blob του Azure αποθηκεύονται ως διαπιστευτηρίου.  Με τη σειρά για τον SQL Server για να γράψετε αντίγραφα ασφαλείας σε αντικειμένων Blob του Azure ή επαναφορά από αυτό, πρέπει να δημιουργηθεί μια πιστοποίηση SQL Server. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαπιστευτηρίων του SQL Server](https://msdn.microsoft.com/library/ms189522.aspx). |

> [AZURE.NOTE] Εάν επιλέξετε να αντιγράψετε και να στείλετε ένα αρχείο αντιγράφου ασφαλείας με την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure, πρέπει να χρησιμοποιήσετε έναν τύπο blob σελίδας ως επιλογή αποθήκευσης εάν σχεδιάζετε να χρησιμοποιήσετε αυτό το αρχείο για λειτουργίες επαναφοράς. ΕΠΑΝΑΦΟΡΆ από έναν τύπο blob μπλοκ θα αποτύχει με το μήνυμα σφάλματος.

## <a name="next-steps"></a>Επόμενα βήματα

1. Δημιουργήστε ένα λογαριασμό Azure, εάν δεν έχετε ήδη ένα. Εάν την αξιολόγηση του Azure, εξετάστε το ενδεχόμενο το [δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/free/).

1. Στη συνέχεια, μεταβείτε σε ένα από τα παρακάτω προγράμματα εκμάθησης που θα σας καθοδηγήσει στη δημιουργία λογαριασμού χώρου αποθήκευσης και της εκτέλεσης μιας επαναφοράς.

    - **SQL Server 2014**: [πρόγραμμα εκμάθησης: δημιουργία αντιγράφων ασφαλείας του SQL Server 2014 και επαναφορά στο Microsoft Azure αντικειμένων Blob υπηρεσία αποθήκευσης στο](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
    - **SQL Server 2016**: [πρόγραμμα εκμάθησης: με την υπηρεσία αποθήκευσης αντικειμένων Blob Microsoft Azure με βάσεις δεδομένων SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx)

1. Αναθεώρηση πρόσθετη τεκμηρίωση ξεκινώντας με το [αντίγραφο ασφαλείας του SQL Server και επαναφορά με υπηρεσία αποθήκευσης αντικειμένων Blob Microsoft Azure](https://msdn.microsoft.com/library/jj919148.aspx).

Εάν έχετε προβλήματα, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας διακομιστή SQL σε διεύθυνση URL βέλτιστες πρακτικές και αντιμετώπιση προβλημάτων](https://msdn.microsoft.com/library/jj919149.aspx).

Για άλλες SQL Server δημιουργία αντιγράφων ασφαλείας και επαναφορά επιλογές, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας και επαναφοράς για τον SQL Server σε εικονικές μηχανές Windows Azure](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
