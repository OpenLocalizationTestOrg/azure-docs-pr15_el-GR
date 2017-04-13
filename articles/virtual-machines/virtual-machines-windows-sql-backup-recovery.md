<properties
    pageTitle="Δημιουργία αντιγράφων ασφαλείας και επαναφοράς για τον SQL Server | Microsoft Azure"
    description="Περιγράφει θέματα αντιγράφων ασφαλείας και επαναφοράς για βάσεις δεδομένων SQL Server που εκτελείται σε εικονικές μηχανές Windows Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Δημιουργία αντιγράφων ασφαλείας και επαναφοράς για τον SQL Server σε εικονικές μηχανές Windows Azure

## <a name="overview"></a>Επισκόπηση

Δημιουργία αντιγράφων ασφαλείας των δεδομένων σε βάσεις δεδομένων SQL Server είναι ένα σημαντικό τμήμα της στρατηγικής όσον αφορά την προστασία από την απώλεια δεδομένων λόγω σφαλμάτων εφαρμογή ή χρήστη. Αυτό είναι εξίσου true για τον SQL Server που εκτελείται σε εικονικές μηχανές Windows Azure (ΣΠΣ).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Για τον SQL Server που εκτελείται στο ΣΠΣ Azure, μπορείτε να χρησιμοποιήσετε εγγενούς δημιουργίας αντιγράφων ασφαλείας και να επαναφέρετε τεχνικές χρήση συνημμένων δίσκων για τον προορισμό των τα αρχεία αντιγράφων ασφαλείας. Ωστόσο, υπάρχει όριο στον αριθμό των δίσκων που μπορείτε να επισυνάψετε μια εικονική μηχανή Azure, με βάση το [μέγεθος του η εικονική μηχανή](virtual-machines-linux-sizes.md). Είναι επίσης τα έξοδα διαχείρισης δίσκου για να λάβετε υπόψη σας.

Ξεκινώντας με το SQL Server 2014, μπορείτε να αντιγράφων ασφαλείας και επαναφοράς για χώρο αποθήκευσης αντικειμένων Blob Microsoft Azure. SQL Server 2016 παρέχει επίσης βελτιώσεις για αυτήν την επιλογή. Επιπλέον, για τα αρχεία βάσης δεδομένων που είναι αποθηκευμένα στο χώρο αποθήκευσης αντικειμένων Blob Microsoft Azure, ο πίνακας "SQL Server 2016" παρέχει μια επιλογή για σχεδόν άμεση δημιουργία αντιγράφων ασφαλείας και για γρήγορη επαναφέρει χρησιμοποιώντας Azure στιγμιότυπα. Σε αυτό το άρθρο παρέχει μια επισκόπηση των αυτές τις επιλογές και, μπορείτε να βρείτε πρόσθετες πληροφορίες στο [αντίγραφο ασφαλείας του SQL Server και επαναφορά με υπηρεσία αποθήκευσης αντικειμένων Blob Microsoft Azure](https://msdn.microsoft.com/library/jj919148.aspx).

>[AZURE.NOTE] Για πληροφορίες σχετικά με τις επιλογές για τη δημιουργία αντιγράφων ασφαλείας πολύ μεγάλες βάσεις δεδομένων, ανατρέξτε στο θέμα [Πολλούς-Terabyte SQL Server βάσης δεδομένων αντιγράφων ασφαλείας στρατηγικές για εικονικές μηχανές Windows Azure](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

Οι παρακάτω ενότητες περιλαμβάνουν πληροφορίες σχετικά με τις διαφορετικές εκδόσεις του SQL Server που υποστηρίζονται σε μια εικονική μηχανή Azure.

## <a name="sql-server-virtual-machines"></a>SQL Server εικονικές μηχανές

Όταν σας παρουσία του SQL Server εκτελείται σε ένα Azure εικονική μηχανή, τα αρχεία βάσης δεδομένων σας βρίσκονται ήδη δίσκων δεδομένων στο Azure. Αυτές οι δίσκων βρίσκονται στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Επομένως, τους λόγους για τη δημιουργία αντιγράφων ασφαλείας για τη βάση δεδομένων και τις μεθόδους που λαμβάνουν αλλαγή ελαφρώς. Λάβετε υπόψη τα εξής. 

- Δεν χρειάζεται πλέον να εκτελείτε αντίγραφα ασφαλείας της βάσης δεδομένων για την παροχή προστασίας από αποτυχία υλικού ή πολυμέσων, επειδή το Microsoft Azure παρέχει προστασία αυτού του ως μέρος της υπηρεσίας Windows Azure.

- Εξακολουθείτε να χρειάζεστε για να εκτελέσετε αντίγραφα ασφαλείας της βάσης δεδομένων για την παροχή προστασίας από σφάλματα χρήστη, ή για σκοπούς αρχειοθέτησης, ρυθμιστικοί λόγοι για τους ή διοικητικά.

- Μπορείτε να αποθηκεύσετε το αρχείο αντιγράφου ασφαλείας απευθείας στο Azure. Για περισσότερες πληροφορίες, ανατρέξτε στις παρακάτω ενότητες που παρέχουν οδηγίες για τις διαφορετικές εκδόσεις του SQL Server.

## <a name="sql-server-2016"></a>SQL Server 2016

Microsoft SQL Server 2016 υποστηρίζει δυνατότητες [δημιουργίας αντιγράφων ασφαλείας και επαναφορά με αντικείμενα BLOB Azure](https://msdn.microsoft.com/library/jj919148.aspx) που βρέθηκε στο SQL Server 2014. Αλλά επίσης περιλαμβάνει τις ακόλουθες βελτιώσεις:

| Βελτίωση 2016               | Λεπτομέρειες                          |
|---------------------|-------------------------------|
| **Επιμερισμό**              | Όταν αντίγραφα ασφαλείας σε χώρο αποθήκευσης αντικειμένων blob Windows Azure, SQL Server 2016 υποστηρίζει τη δημιουργία αντιγράφων ασφαλείας πολλά αντικείμενα BLOB για να ενεργοποιήσετε τη δημιουργία αντιγράφων ασφαλείας μεγάλων βάσεων δεδομένων, έως και 12,8 TB.      |
| **Στιγμιότυπο δημιουργίας αντιγράφων ασφαλείας**                | Με τη χρήση Azure στιγμιότυπα, αντιγράφου ασφαλείας του SQL Server αρχείο στιγμιότυπου παρέχει σχεδόν άμεση δημιουργία αντιγράφων ασφαλείας και ταχεία επαναφέρει για αρχεία βάσης δεδομένων που είναι αποθηκευμένα με την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure. Αυτή η δυνατότητα σάς επιτρέπει να απλοποιήσετε το αντίγραφο ασφαλείας και επαναφορά πολιτικές. Στιγμιότυπο αρχείο αντιγράφου ασφαλείας υποστηρίζει επίσης σημείο στο χρόνο επαναφοράς. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Στιγμιότυπο δημιουργίας αντιγράφων ασφαλείας για τα αρχεία βάσης δεδομένων στο Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).   |
| **Διαχείριση ασφαλείας προγραμματισμού**            | SQL Server διαχειριζόμενων αντίγραφα ασφαλείας για Azure υποστηρίζει πλέον προσαρμοσμένων χρονοδιαγραμμάτων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [SQL Server διαχειριζόμενων αντίγραφα ασφαλείας για Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx).   |

Για ένα πρόγραμμα εκμάθησης τις δυνατότητες του SQL Server 2016 κατά τη χρήση χώρος αποθήκευσης αντικειμένων Blob του Azure, ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: με την υπηρεσία αποθήκευσης αντικειμένων Blob Microsoft Azure με βάσεις δεδομένων SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014

SQL Server 2014 περιλαμβάνει τις ακόλουθες βελτιώσεις:

1. **Δημιουργία αντιγράφων ασφαλείας και επαναφορά για να Azure**:

 - *Δημιουργία αντιγράφων ασφαλείας SQL Server σε διεύθυνση URL* τώρα διαθέτει υποστήριξη στο SQL Server Management Studio. Την επιλογή για να δημιουργήσετε αντίγραφα ασφαλείας σε Azure τώρα είναι διαθέσιμη όταν χρησιμοποιείτε εργασίας δημιουργίας αντιγράφων ασφαλείας ή επαναφορά ή Οδηγός πρόγραμμα συντήρησης σε SQL Server Management Studio. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας SQL Server σε διεύθυνση URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - *SQL Server διαχειριζόμενων δημιουργίας αντιγράφων ασφαλείας για να Azure* περιλαμβάνει νέες λειτουργίες που σας δίνει τη δυνατότητα αυτόματης δημιουργίας αντιγράφων ασφαλείας διαχείρισης. Αυτό είναι ιδιαίτερα χρήσιμο για την αυτοματοποίηση της δημιουργίας αντιγράφων ασφαλείας διαχείρισης για το SQL Server 2014 παρουσιών που εκτελούνται σε έναν υπολογιστή Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [SQL Server διαχειριζόμενων αντίγραφα ασφαλείας για Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
 - *Αυτόματη δημιουργία αντιγράφων ασφαλείας* παρέχει πρόσθετες αυτοματισμού για να ενεργοποιήσετε αυτόματα *SQL Server διαχειριζόμενων αντίγραφα ασφαλείας για Azure* σε όλες τις υπάρχουσες και τις νέες βάσεις δεδομένων για μια Εικονική SQL Server στα Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αυτόματης δημιουργίας αντιγράφων ασφαλείας για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-automated-backup.md).
 - Για μια επισκόπηση του όλες τις επιλογές για SQL Server 2014 αντίγραφα ασφαλείας για Azure, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας του SQL Server και επαναφορά με υπηρεσία αποθήκευσης αντικειμένων Blob Microsoft Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Κρυπτογράφηση**: SQL Server 2014 υποστηρίζει κρυπτογράφηση δεδομένων κατά τη δημιουργία αντιγράφου ασφαλείας. Υποστηρίζει πολλές αλγόριθμοι κρυπτογράφησης και τη χρήση ασύμμετρη πλήκτρο ή osf ένα πιστοποιητικό. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [Κρυπτογράφηση δημιουργίας αντιγράφων ασφαλείας](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012

Για λεπτομερείς πληροφορίες σχετικά με δημιουργία αντιγράφων ασφαλείας του SQL Server και επαναφορά στον SQL Server 2012, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας και επαναφορά της βάσεις δεδομένων SQL Server (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Εκκίνηση στο SQL Server 2012 SP1 αθροιστική ενημερωμένη έκδοση 2, για να αντίγραφο ασφαλείας και επαναφορά από την υπηρεσία αποθήκευσης αντικειμένων Blob Azure. Αυτή η βελτίωση μπορεί να χρησιμοποιηθεί για τη δημιουργία αντιγράφου ασφαλείας βάσεις δεδομένων SQL Server σε ένα διακομιστή SQL που εκτελείται σε μια εικονική μηχανή Azure ή μια παρουσία εσωτερικής εγκατάστασης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας του SQL Server και επαναφορά με υπηρεσία αποθήκευσης αντικειμένων Blob του Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Ορισμένα από τα πλεονεκτήματα της χρήσης της υπηρεσίας χώρο αποθήκευσης αντικειμένων Blob του Azure περιλαμβάνουν τη δυνατότητα να παρακάμψει το όριο 16 δίσκου για προσαρτημένο δίσκο, διευκολύνετε τη διαχείριση, τη διαθεσιμότητα απευθείας από το αρχείο αντιγράφου ασφαλείας για μια άλλη παρουσία του παρουσία του SQL Server που εκτελείται σε μια εικονική μηχανή Azure ή μια παρουσία εσωτερικής εγκατάστασης για σκοπούς αποκατάστασης από καταστροφή ή μετεγκατάστασης. Για μια πλήρη λίστα των οφέλη από τη χρήση μια υπηρεσία αποθήκευσης αντικειμένων blob του Azure για αντίγραφα ασφαλείας SQL Server, ανατρέξτε στην ενότητα *πλεονεκτήματα* στο [αντίγραφο ασφαλείας του SQL Server και επαναφορά με υπηρεσία αποθήκευσης αντικειμένων Blob του Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Για βέλτιστη πρακτική συστάσεις και τις πληροφορίες αντιμετώπισης προβλημάτων, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας και επαναφορά βέλτιστες πρακτικές (υπηρεσία αποθήκευσης αντικειμένων Blob Azure)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008

Για δημιουργία αντιγράφων ασφαλείας του SQL Server και επαναφορά στον SQL Server 2008 R2, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας και επαναφορά των βάσεων δεδομένων στον SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

Για δημιουργία αντιγράφων ασφαλείας του SQL Server και επαναφορά στον SQL Server 2008, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας και επαναφορά των βάσεων δεδομένων στον SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Επόμενα βήματα

Εάν σχεδιάζετε την ανάπτυξη του SQL Server σε μια Εικονική Azure, μπορείτε να βρείτε οδηγίες παροχής με το εξής πρόγραμμα εκμάθησης: [προμήθεια του SQL Server εικονικό μηχάνημα σε Azure με τη διαχείριση πόρων Azure](virtual-machines-windows-portal-sql-server-provision.md).

Αν και δημιουργία αντιγράφων ασφαλείας και επαναφοράς μπορεί να χρησιμοποιηθεί για τη μετεγκατάσταση των δεδομένων σας, υπάρχουν ενδεχομένως ευκολότερη διαδρομές μετεγκατάστασης δεδομένων με τον SQL Server σε μια Εικονική Azure. Για πλήρεις πληροφορίες σχετικά με επιλογές μετεγκατάστασης και συστάσεις, ανατρέξτε στο θέμα [Μετεγκατάσταση βάσης δεδομένων με τον SQL Server σε μια Εικονική Azure](virtual-machines-windows-migrate-sql.md).

Αναθεώρηση άλλους [πόρους για την εκτέλεση του SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-server-iaas-overview.md).