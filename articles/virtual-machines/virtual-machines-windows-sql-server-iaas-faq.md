<properties
    pageTitle="SQL Server στο Azure εικονικές μηχανές συνήθεις Ερωτήσεις | Microsoft Azure"
    description="Σε αυτό το άρθρο παρέχει απαντήσεις σε συνήθεις ερωτήσεις σχετικά με την εκτέλεση του SQL Server σε VM Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-server-on-azure-virtual-machines-faq"></a>SQL Server στο Azure εικονικές μηχανές συνήθεις Ερωτήσεις

Αυτό το θέμα παρέχει απαντήσεις σε ορισμένες από τις πιο συνηθισμένες ερωτήσεις σχετικά με την εκτέλεση του [SQL Server σε εικονικές μηχανές Windows Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Συνήθεις ερωτήσεις

1. **Πώς μπορώ να δημιουργήσω μια εικονική μηχανή Azure με τον SQL Server;**

    Υπάρχουν δύο τρόποι να το κάνετε αυτό. Η πιο εύκολος λύση είναι να δημιουργήσετε μια εικονική μηχανή που περιλαμβάνει το SQL Server. Για ένα πρόγραμμα εκμάθησης στην εγγραφή στο Azure και τη δημιουργία μια Εικονική SQL από την πύλη, ανατρέξτε στο θέμα [παροχή μια εικονική μηχανή SQL Server στην πύλη του Azure](virtual-machines-windows-portal-sql-server-provision.md). Έχετε επίσης την επιλογή με μη αυτόματο τρόπο την εγκατάσταση του SQL Server σε μια Εικονική και επαναχρησιμοποίηση μιας άδειας χρήσης εσωτερικής εγκατάστασης με [Άδεια χρήσης φορητότητα μέσω εξασφάλισης αναβάθμισης λογισμικού σε Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Ποια είναι η διαφορά μεταξύ του ΣΠΣ SQL και την υπηρεσία βάσης δεδομένων SQL;**

    Εννοιολογικά, εκτελεί τον SQL Server σε μια εικονική μηχανή Azure δεν που διαφέρει από το εκτελεί τον SQL Server σε έναν απομακρυσμένο κέντρα δεδομένων. Αντίθετα, [Βάση δεδομένων SQL](../sql-database/sql-database-technical-overview.md) προσφέρει βάσης δεδομένων ως-a-υπηρεσίας. Με βάση δεδομένων SQL, δεν έχετε πρόσβαση στα μηχανήματα που φιλοξενεί τις βάσεις δεδομένων. Για μια πλήρη σύγκριση, ανατρέξτε στο θέμα [Επιλέξτε μια επιλογή SQL Server στο cloud: βάση δεδομένων SQL Azure (PaaS) ή του SQL Server σε Azure VM (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Πώς μπορώ να μετεγκαταστήσω τη βάση δεδομένων SQL Server εσωτερικής εγκατάστασης στο cloud;**

    Πρώτα να δημιουργήσετε μια εικονική μηχανή Azure με μια παρουσία του SQL Server. Στη συνέχεια, να μετεγκαταστήσετε τις βάσεις δεδομένων εσωτερικής εγκατάστασης αυτήν την παρουσία. Για στρατηγικές μετεγκατάστασης δεδομένων, ανατρέξτε στο θέμα [μετεγκατάσταση μιας βάσης δεδομένων SQL Server με τον SQL Server σε μια Εικονική Azure](virtual-machines-windows-migrate-sql.md).

2. **Μπορώ να αλλάξω τις εγκατεστημένες δυνατότητες ή να εγκαταστήσετε μια δεύτερη παρουσία του SQL Server σε το ίδιο Εικονική;**

    Ναι. Το μέσο εγκατάστασης του SQL Server βρίσκεται σε ένα φάκελο στη μονάδα δίσκου **C** . Εκτελέστε το **Setup.exe** από εκείνη τη θέση για να προσθέσετε νέες παρουσίες του SQL Server ή για να αλλάξετε άλλες δυνατότητες που είναι εγκατεστημένες του SQL Server στον υπολογιστή.

3. **Πώς μπορώ να αναβαθμίσω σε μια νέα έκδοση/έκδοση του SQL Server σε μια Εικονική Azure;**

    Προς το παρόν, δεν υπάρχει καμία αναβάθμιση στην ίδια θέση για το SQL Server που εκτελείται σε μια Εικονική Azure. Δημιουργήστε μια νέα Azure εικονική μηχανή με την επιθυμητή έκδοση SQL Server/έκδοση και, στη συνέχεια, να μετεγκαταστήσετε τις βάσεις δεδομένων στο νέο διακομιστή με τυπική [τεχνικών μετεγκατάστασης δεδομένων](virtual-machines-windows-migrate-sql.md).

4. **Πώς μπορώ να εγκαταστήσω το αντίγραφό μου με την άδεια χρήσης του SQL Server σε μια Εικονική Azure;**

    Αντιγράψτε το μέσο εγκατάστασης του SQL Server για την εικονική Μηχανή Windows Server και, στη συνέχεια, εγκατάσταση του SQL Server σε η Εικονική. Για άδειες χρήσης λόγοι, πρέπει να έχετε [Άδεια χρήσης φορητότητα μέσω εξασφάλισης αναβάθμισης λογισμικού σε Azure](https://azure.microsoft.com/pricing/license-mobility/).

5. **Πρέπει να πληρώσετε τις τιμές κόστους SQL μια Εικονική εάν χρησιμοποιείται μόνο για αναμονή/ανακατεύθυνση;**

    Εάν θέλετε να δημιουργήσετε την Εικονική SQL μέσω της συλλογής, στη συνέχεια, πρέπει να έχετε ξεχωριστή άδεια χρήσης για το αναμονής Εικονική SQL και την τιμολόγηση είναι η ίδια. Εάν εγκαταστήσετε το SQL Server με μη αυτόματο τρόπο σε μια εικονική μηχανή με [Φορητότητα άδειας χρήσης](https://azure.microsoft.com/pricing/license-mobility/), έχετε την επιλογή για να έχετε μία δωρεάν παθητικές SQL παρουσία για ανακατεύθυνση. Εξετάστε τους περιορισμούς και τις απαιτήσεις.

6. **Πώς ενημερώσεις και service pack εφαρμόζονται σε μια Εικονική SQL Server;**

    Εικονικές μηχανές επιτρέπουν να ελέγχετε τον κεντρικό υπολογιστή, όπως πότε και πώς μπορείτε να εφαρμόσετε ενημερώσεις. Για το λειτουργικό σύστημα, μπορείτε να εφαρμόσετε με μη αυτόματο τρόπο ενημερωμένες εκδόσεις των windows ή μπορείτε να ενεργοποιήσετε μια υπηρεσία προγραμματισμού που ονομάζεται [Αυτόματη Διόρθωση](virtual-machines-windows-classic-sql-automated-patching.md). Αυτόματη εγκατάσταση ενημερώσεων τις ενημερώσεις που έχουν επισημανθεί ως σημαντικά, συμπεριλαμβανομένων των ενημερώσεων του SQL Server σε αυτή την κατηγορία. Άλλες προαιρετικές ενημερώσεις για SQL Server πρέπει να έχει εγκατασταθεί με μη αυτόματο τρόπο.

7. **Είναι δυνατή η ρύθμιση παραμέτρων δεν εμφανίζεται στη συλλογή εικονική μηχανή (για παράδειγμα Windows 2008 R2 + SQL Server 2012);**

    Όχι. Εικονική μηχανή συλλογή εικόνων που περιλαμβάνουν το SQL Server, πρέπει να επιλέξετε μία από τις εικόνες που παρέχεται.

9. **Πώς μπορώ να εγκαταστήσω Εργαλεία δεδομένων SQL σε μου Εικονική Azure;**

    Κάντε λήψη και να εγκαταστήσετε τα Εργαλεία δεδομένων SQL από το [Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2013 ](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Πόροι

Για μια επισκόπηση του SQL Server σε εικονικές μηχανές Windows Azure, παρακολουθήστε το βίντεο [Εικονική Azure είναι η καλύτερη πλατφόρμα για SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016). Μπορείτε επίσης να λάβετε μια καλή εισαγωγή στο θέμα, [SQL Server σε εικονικές μηχανές Windows Azure Επισκόπηση](virtual-machines-windows-sql-server-iaas-overview.md).

Άλλοι πόροι περιλαμβάνουν τα εξής:

- [Προμήθεια μια εικονική μηχανή SQL Server στην πύλη του Azure](virtual-machines-windows-portal-sql-server-provision.md)
- [Μετεγκατάσταση βάσης δεδομένων με τον SQL Server σε μια Εικονική Azure](virtual-machines-windows-migrate-sql.md)
- [Υψηλή διαθεσιμότητα και την αποκατάσταση για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-high-availability-dr.md)
- [Απόδοση βέλτιστες πρακτικές για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-performance.md)
- [Εφαρμογή μοτίβων και στρατηγικές ανάπτυξης για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)
