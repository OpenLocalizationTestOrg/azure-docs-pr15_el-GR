<properties
    pageTitle="Ρύθμιση παραμέτρων ενοποίησης Azure θάλαμο κλειδιού για τον SQL Server στο Azure ΣΠΣ (Διαχείριση πόρων)"
    description="Μάθετε πώς μπορείτε να αυτοματοποιήσετε τη ρύθμιση παραμέτρων της κρυπτογράφησης SQL Server για χρήση με το Azure κλειδί θάλαμο. Αυτό το θέμα εξηγεί τον τρόπο χρήσης Azure κλειδί θάλαμο ενοποίηση με το SQL Server εικονικές μηχανές δημιουργήθηκε με τη διαχείριση πόρων."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-resource-manager"></a>Ρύθμιση παραμέτρων ενοποίησης Azure θάλαμο κλειδιού για τον SQL Server στο Azure ΣΠΣ (Διαχείριση πόρων)

> [AZURE.SELECTOR]
- [Διαχείριση πόρων](virtual-machines-windows-ps-sql-keyvault.md)
- [Κλασικό](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Επισκόπηση
Υπάρχουν πολλές δυνατότητες κρυπτογράφησης SQL Server, όπως η [κρυπτογράφηση διαφανή δεδομένων (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), η [στήλη επιπέδου κρυπτογράφηση (α)](https://msdn.microsoft.com/library/ms173744.aspx)και [κρυπτογράφησης δημιουργίας αντιγράφων ασφαλείας](https://msdn.microsoft.com/library/dn449489.aspx). Αυτές οι φόρμες κρυπτογράφησης απαιτεί να διαχειριστείτε και να αποθηκεύσετε τα κλειδιά κρυπτογράφησης που χρησιμοποιείτε για την κρυπτογράφηση. Η υπηρεσία θάλαμο κλειδί Azure (AKV) έχει σχεδιαστεί για να βελτιώσετε την ασφάλεια και τη διαχείριση αυτών των πλήκτρων σε μια θέση ασφαλούς και ιδιαίτερα διαθέσιμο. Η [Γραμμή σύνδεσης του SQL Server](http://www.microsoft.com/download/details.aspx?id=45344) προσφέρει SQL Server για να χρησιμοποιήσετε αυτά τα πλήκτρα από θάλαμο κλειδί Azure.

Εάν που εκτελεί τον SQL Server με εσωτερικής μηχανήματα, υπάρχουν βήματα [που μπορείτε να ακολουθήσετε για να αποκτήσετε πρόσβαση Azure θάλαμο κλειδί από τον υπολογιστή σας SQL Server εσωτερικής εγκατάστασης](https://msdn.microsoft.com/library/dn198405.aspx). Αλλά για τον SQL Server στο ΣΠΣ Azure, μπορείτε να εξοικονομήσετε χρόνο χρησιμοποιώντας τη δυνατότητα *Ενοποίησης θάλαμο Azure αριθμού-κλειδιού* .

Όταν αυτή η δυνατότητα είναι ενεργοποιημένη, το αυτόματα εγκαθιστά τη γραμμή σύνδεσης του SQL Server, ρυθμίζει την υπηρεσία παροχής EKM για να αποκτήσετε πρόσβαση θάλαμο κλειδί Azure και δημιουργεί τα διαπιστευτήρια για να σας επιτρέπει να αποκτήσετε πρόσβαση σας θάλαμο. Εάν είδατε τα βήματα στην τεκμηρίωση της παραπάνω στην εσωτερική εγκατάσταση, μπορείτε να δείτε ότι αυτή η δυνατότητα αυτοματοποιεί τα βήματα 2 και 3. Το μόνο που θα εξακολουθείτε να χρειάζεστε για να το κάνετε με μη αυτόματο τρόπο είναι για να δημιουργήσετε τα πλήκτρα και κλειδιού θάλαμο. Από εκεί, την εγκατάσταση ολόκληρου του σας Εικονική SQL είναι αυτοματοποιημένο. Μόλις η δυνατότητα αυτή ολοκληρωθεί αυτό το πρόγραμμα εγκατάστασης, μπορείτε να εκτελέσετε προτάσεις T-SQL για να ξεκινήσει η κρυπτογράφηση βάσεις δεδομένων ή δημιουργία αντιγράφων ασφαλείας σας όπως θα κάνατε κανονικά.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Ενεργοποίηση και ρύθμιση παραμέτρων ενοποίησης AKV
Μπορείτε να ενεργοποιήσετε την ενοποίηση AKV κατά την προμήθεια του ή ρυθμίστε τις παραμέτρους του για υπάρχοντα ΣΠΣ.

### <a name="new-vms"></a>Νέα ΣΠΣ
Εάν προμήθεια μια νέα εικονική μηχανή SQL Server με τη διαχείριση πόρων, η πύλη του Azure παρέχει ένα βήμα για να ενεργοποιήσετε την ενοποίηση του Azure κλειδί θάλαμο. Η δυνατότητα θάλαμο κλειδί Azure είναι διαθέσιμη μόνο για μεγάλες επιχειρήσεις, προγραμματιστής και αξιολόγησης εκδόσεις του SQL Server.

![Ενοποίηση του SQL Azure θάλαμο κλειδιού](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Για μια λεπτομερή περιγραφή της προετοιμασίας, ανατρέξτε στο θέμα [παροχή μια εικονική μηχανή SQL Server στην πύλη του Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Υπάρχουσα ΣΠΣ
Για υπάρχοντα εικονικές μηχανές SQL Server, επιλέξτε την εικονική μηχανή SQL Server. Στη συνέχεια, επιλέξτε την ενότητα **ρύθμισης παραμέτρων του SQL Server** από το blade **Ρυθμίσεις** .

![Ενοποίηση AKV SQL για υπάρχοντα ΣΠΣ](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Στο το blade **ρύθμισης παραμέτρων του SQL Server** , κάντε κλικ στο κουμπί **Επεξεργασία** στην ενότητα ενοποίηση αυτοματοποιημένη θάλαμο αριθμού-κλειδιού.

![Ρύθμιση παραμέτρων ενοποίησης AKV SQL για υπάρχοντα ΣΠΣ](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Όταν τελειώσετε, κάντε κλικ στο κουμπί **OK** στο κάτω μέρος του blade **ρύθμισης παραμέτρων του SQL Server** για να αποθηκεύσετε τις αλλαγές σας.

>[AZURE.NOTE] Μπορείτε επίσης να ρυθμίσετε ενοποίηση AKV χρησιμοποιώντας ένα πρότυπο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πρότυπο Azure γρήγορης έναρξης για την ενοποίηση Azure κλειδί θάλαμο](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
