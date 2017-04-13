<properties
    pageTitle="Ρύθμιση παραμέτρων ενοποίησης Azure θάλαμο κλειδιού για τον SQL Server στο Azure ΣΠΣ (κλασικό)"
    description="Μάθετε πώς μπορείτε να αυτοματοποιήσετε τη ρύθμιση παραμέτρων της κρυπτογράφησης SQL Server για χρήση με το Azure κλειδί θάλαμο. Αυτό το θέμα εξηγεί τον τρόπο χρήσης του Azure κλειδί θάλαμο ενοποίηση με τον SQL Server Δημιουργήστε εικονικές μηχανές στο μοντέλο κλασική ανάπτυξης."
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
    ms.date="09/26/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-classic"></a>Ρύθμιση παραμέτρων ενοποίησης Azure θάλαμο κλειδιού για τον SQL Server στο Azure ΣΠΣ (κλασικό)

> [AZURE.SELECTOR]
- [Διαχείριση πόρων](virtual-machines-windows-ps-sql-keyvault.md)
- [Κλασικό](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Επισκόπηση
Υπάρχουν πολλές δυνατότητες κρυπτογράφησης SQL Server, όπως η [κρυπτογράφηση διαφανή δεδομένων (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), η [στήλη επιπέδου κρυπτογράφηση (α)](https://msdn.microsoft.com/library/ms173744.aspx)και [δημιουργίας αντιγράφων ασφαλείας κρυπτογράφησης](https://msdn.microsoft.com/library/dn449489.aspx). Αυτές οι φόρμες κρυπτογράφησης απαιτεί να διαχειριστείτε και να αποθηκεύσετε τα κλειδιά κρυπτογράφησης που χρησιμοποιείτε για την κρυπτογράφηση. Η υπηρεσία θάλαμο κλειδί Azure (AKV) έχει σχεδιαστεί για να βελτιώσετε την ασφάλεια και τη διαχείριση αυτών των πλήκτρων σε μια θέση ασφαλούς και ιδιαίτερα διαθέσιμο. Η [Γραμμή σύνδεσης του SQL Server](http://www.microsoft.com/download/details.aspx?id=45344) προσφέρει SQL Server για να χρησιμοποιήσετε αυτά τα πλήκτρα από θάλαμο κλειδί Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Εάν χρησιμοποιείτε το SQL Server με μηχανές εσωτερικής εγκατάστασης, υπάρχουν [βήματα που μπορείτε να ακολουθήσετε για να αποκτήσετε πρόσβαση Azure θάλαμο κλειδί από τον υπολογιστή σας SQL Server εσωτερικής εγκατάστασης](https://msdn.microsoft.com/library/dn198405.aspx). Αλλά για τον SQL Server στο ΣΠΣ Azure, μπορείτε να εξοικονομήσετε χρόνο χρησιμοποιώντας τη δυνατότητα *Ενοποίησης θάλαμο Azure αριθμού-κλειδιού* . Με λίγα Azure PowerShell cmdlet για να ενεργοποιήσετε αυτήν τη δυνατότητα, μπορείτε να αυτοματοποιήσετε τη ρύθμιση παραμέτρων είναι απαραίτητο για μια Εικονική SQL για να αποκτήσετε πρόσβαση του κλειδιού θάλαμο.

Όταν αυτή η δυνατότητα είναι ενεργοποιημένη, το αυτόματα εγκαθιστά τη γραμμή σύνδεσης του SQL Server, ρυθμίζει την υπηρεσία παροχής EKM για να αποκτήσετε πρόσβαση θάλαμο κλειδί Azure και δημιουργεί τα διαπιστευτήρια για να σας επιτρέπει να αποκτήσετε πρόσβαση σας θάλαμο. Εάν είδατε τα βήματα στην τεκμηρίωση της παραπάνω στην εσωτερική εγκατάσταση, μπορείτε να δείτε ότι αυτή η δυνατότητα αυτοματοποιεί τα βήματα 2 και 3. Το μόνο που εξακολουθείτε να χρειάζεστε για να κάνετε με μη αυτόματο τρόπο είναι για να δημιουργήσετε τα πλήκτρα και κλειδιού θάλαμο. Από εκεί, την εγκατάσταση ολόκληρου του σας Εικονική SQL είναι αυτοματοποιημένο. Μόλις η δυνατότητα αυτή ολοκληρωθεί αυτό το πρόγραμμα εγκατάστασης, μπορείτε να εκτελέσετε προτάσεις T-SQL για να ξεκινήσει η κρυπτογράφηση βάσεις δεδομένων ή δημιουργία αντιγράφων ασφαλείας σας όπως θα κάνατε κανονικά.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>Ρύθμιση παραμέτρων ενοποίησης AKV
Χρήση του PowerShell για τη ρύθμιση παραμέτρων ενοποίησης θάλαμο Azure αριθμού-κλειδιού. Οι παρακάτω ενότητες παρέχουν μια επισκόπηση των τις απαιτούμενες παραμέτρους και, στη συνέχεια, ένα δείγμα δέσμης ενεργειών του PowerShell.

### <a name="install-the-sql-server-iaas-extension"></a>Εγκαταστήστε την επέκταση IaaS του SQL Server

Πρώτα, [εγκαταστήστε την επέκταση IaaS του SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>Κατανοήστε τις παραμέτρους εισόδου
Ο παρακάτω πίνακας παραθέτει τις παραμέτρους που απαιτούνται για την εκτέλεση της δέσμης ενεργειών του PowerShell στην επόμενη ενότητα.

|Παράμετρος|Περιγραφή|Παράδειγμα|
|---|---|---|
|**$akvURL**|**Η διεύθυνση URL θάλαμο κλειδιού**|"https://contosokeyvault.vault.azure.net/"|
|**$spName**|**Κύριο όνομα υπηρεσίας**|"fde2b411 - 33 δ 5-4e11-af04eb07b669ccf2"|
|**$spSecret**|**Μυστικό κεφάλαιο υπηρεσίας**|"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM ="|
|**$credName**|**Όνομα διαπιστευτηρίων**: ενοποίηση AKV δημιουργεί μια πιστοποίηση μέσα σε SQL Server, επιτρέποντας την εικονική Μηχανή να έχουν πρόσβαση σε του κλειδιού θάλαμο. Επιλέξτε ένα όνομα για αυτά τα διαπιστευτήρια.|"mycred1"|
|**$vmName**|**Εικονική μηχανή όνομα**: το όνομα του μια Εικονική SQL που δημιουργήσατε προηγουμένως.|"myvmname"|
|**$serviceName**|**Όνομα υπηρεσίας**: όνομα την υπηρεσία Cloud που είναι συσχετισμένη με την Εικονική SQL.|"mycloudservicename"|

### <a name="enable-akv-integration-with-powershell"></a>Ενεργοποίηση AKV ενοποίηση με το PowerShell
Το cmdlet **New-AzureVMSqlServerKeyVaultCredentialConfig** δημιουργεί ένα αντικείμενο ρύθμισης παραμέτρων για τη δυνατότητα ενοποίησης θάλαμο Azure αριθμού-κλειδιού. Το **Σύνολο AzureVMSqlServerExtension** ρυθμίζει αυτή η ενοποίηση με την παράμετρο **KeyVaultCredentialSettings** . Τα ακόλουθα βήματα δείχνουν πώς μπορείτε να χρησιμοποιήσετε αυτές τις εντολές.

1. Στο Azure PowerShell, πρώτα να ρυθμίσετε τις παραμέτρους εισόδου με τις συγκεκριμένες τιμές όπως περιγράφεται στις προηγούμενες ενότητες αυτού του θέματος. Η ακόλουθη δέσμη ενεργειών είναι ένα παράδειγμα.

        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2.  Στη συνέχεια, χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών για να ρυθμίσετε τις παραμέτρους και να ενεργοποιήσετε την ενοποίηση του AKV.

        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

Η επέκταση SQL IaaS παράγοντας θα ενημερωθεί η Εικονική SQL με αυτήν τη νέα ρύθμιση.

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
