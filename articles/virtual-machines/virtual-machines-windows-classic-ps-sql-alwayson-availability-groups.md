<properties
    pageTitle="Ρύθμιση παραμέτρων πάντα στην ομάδα διαθεσιμότητα σε Εικονική Azure με το PowerShell"
    description="Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί τους πόρους που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης και χρησιμοποιεί το PowerShell για να δημιουργήσετε μια πάντα στη διαθεσιμότητα ομάδα στο Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm-with-powershell"></a>Ρύθμιση παραμέτρων πάντα στην ομάδα διαθεσιμότητα σε Εικονική Azure με το PowerShell

> [AZURE.SELECTOR]
- [Διαχείριση πόρων: προτύπου](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Διαχείριση πόρων: μη αυτόματος](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Κλασική: περιβάλλον εργασίας Χρήστη](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Κλασική: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Μπορεί να σας βοηθήσει διαχειριστές βάσεων δεδομένων για την υλοποίηση της κάτω στο κόστος μιας υψηλή διαθεσιμότητα σύστημα του SQL Server Azure εικονικές μηχανές (ΣΠΣ). Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να υλοποιήσετε μια ομάδα διαθεσιμότητα με το SQL Server πάντα στο τέλος-σε-end μέσα σε ένα περιβάλλον Azure. Στο τέλος του προγράμματος εκμάθησης, SQL Server πάντα σε τη λύση σας στο Azure αποτελείται από τα εξής στοιχεία:

- Ένα εικονικό δίκτυο που περιέχει πολλά δευτερεύοντα δίκτυα, όπως μια προσκηνίου και παρασκηνίου υποδίκτυο

- Ελεγκτή τομέα με έναν τομέα Active Directory (AD)

- Δύο SQL Server ΣΠΣ αναπτύσσεται στο υποδίκτυο παρασκηνίου και να συνδεθεί με τον τομέα AD

- Ένα σύμπλεγμα WSFC 3-κόμβο με το μοντέλο απαρτίας μεγαλύτερο μέρος κόμβου

- Μια ομάδα διαθεσιμότητα με δύο αντίγραφα σύγχρονη ολοκλήρωσης μιας βάσης δεδομένων διαθεσιμότητας

Αυτό το σενάριο επιλέγεται για την απλότητα, όχι για την αποτελεσματικότητα κόστος ή άλλους παράγοντες στην Azure. Για παράδειγμα, μπορείτε να ελαχιστοποιήσετε τον αριθμό των ΣΠΣ για μια ομάδα δύο ρεπλίκα διαθεσιμότητας για να αποθηκεύσετε σε ώρες υπολογισμού στο Azure με τη χρήση του ελεγκτή τομέα ως το μαρτυρίας κοινή χρήση αρχείου απαρτίας σε ένα σύμπλεγμα WSFC 2-κόμβο. Αυτή η μέθοδος μειώνει το πλήθος Εικονική με μία από τις παραπάνω παραμέτρους.

Αυτό το πρόγραμμα εκμάθησης προορίζεται για να βλέπετε τα βήματα που απαιτούνται για να ρυθμίσετε τη λύση που περιγράφεται παραπάνω χωρίς κατάρτιση σε τις λεπτομέρειες της κάθε βήμα. Επομένως, αντί να παρουσιάζει τα βήματα ρύθμισης παραμέτρων Γραφικών, χρησιμοποιεί PowerShell δέσμες ενεργειών για να μεταβαίνετε γρήγορα σε κάθε βήμα. Προϋποθέτει τα εξής:

- Έχετε ήδη ένα λογαριασμό Azure με τη συνδρομή εικονική μηχανή.

- Έχετε εγκαταστήσει τα [cmdlet του Azure PowerShell](../powershell-install-configure.md).

- Έχετε ήδη ένα συμπαγές Κατανόηση της πάντα σε ομάδες διαθεσιμότητα για εσωτερική λύσεις. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πάντα σε ομάδες διαθεσιμότητας (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Συνδεθείτε στη συνδρομή σας στο Azure και να δημιουργήσετε το εικονικό δίκτυο

1. Σε ένα παράθυρο του PowerShell στον τοπικό σας υπολογιστή, εισαγάγετε τη λειτουργική μονάδα Azure, κάντε λήψη του αρχείου ρυθμίσεις δημοσίευσης στον υπολογιστή σας και συνδέστε την περίοδο λειτουργίας PowerShell στη συνδρομή σας στο Azure, εισάγοντας τις ρυθμίσεις δημοσίευσης που έχετε λάβει.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    Η εντολή **Get-AzurePublishgSettingsFile** δημιουργεί αυτόματα μια διαχείρισης πιστοποιητικού με Azure κάνει λήψη στον υπολογιστή σας. Θα ανοίξει αυτόματα ένα πρόγραμμα περιήγησης και θα σας ζητηθεί να εισαγάγετε τα διαπιστευτήρια λογαριασμού Microsoft για τη συνδρομή σας στο Azure. Το αρχείο λήψης **.publishsettings** περιέχει όλες τις πληροφορίες που χρειάζεστε για να διαχειριστείτε τη συνδρομή σας στο Azure. Μετά την αποθήκευση αυτού του αρχείου σε έναν τοπικό κατάλογο, εισαγάγετε χρησιμοποιώντας την εντολή **Εισαγωγή AzurePublishSettingsFile** .

    >[AZURE.NOTE] Το αρχείο publishsettings περιέχει τα διαπιστευτήριά σας (χωρίς κωδικοποίηση) που χρησιμοποιούνται για τη διαχείριση των συνδρομών Azure και των υπηρεσιών. Η βέλτιστη πρακτική ασφαλείας για αυτό το αρχείο είναι να αποθηκεύσετε προσωρινά εκτός του κατάλογοι προέλευσης (για παράδειγμα, στο φάκελο Libraries\Documents) και, στη συνέχεια, διαγράψτε το μόλις ολοκληρωθεί η εισαγωγή. Ένα κακόβουλο χρήστη τη δυνατότητα πρόσβασης στο αρχείο publishsettings να επεξεργασία, δημιουργία και διαγραφή των υπηρεσιών Azure σας.

1. Ορισμός μιας σειράς μεταβλητές που θα χρησιμοποιήσετε για να δημιουργήσετε το cloud την υποδομή των τμημάτων.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Προσέξτε με το εξής για να βεβαιωθείτε ότι οι εντολές σας θα είναι επιτυχής αργότερα:

    - Μεταβλητές **$storageAccountName** και **$dcServiceName** πρέπει να είναι μοναδικό επειδή χρησιμοποιούνται για τον προσδιορισμό cloud χώρο αποθήκευσης στο λογαριασμό και cloud διακομιστή, αντίστοιχα, στο internet.

    - Ονόματα που έχουν καθοριστεί για μεταβλητές **$affinityGroupName** και **$virtualNetworkName** έχουν ρυθμιστεί στο έγγραφο ρύθμισης παραμέτρων εικονικού δικτύου που θα χρησιμοποιήσετε αργότερα.

    - **$sqlImageName** Καθορίζει το ενημερωμένο όνομα της εικόνας Εικονική που περιέχει SQL Server 2012 Service Pack 1 Enterprise Edition.

    - Για λόγους ευκολίας, **Contoso! 000** έχει τον ίδιο κωδικό πρόσβασης που χρησιμοποιούνται σε όλο το πρόγραμμα εκμάθησης ολόκληρο.

1. Δημιουργήστε μια ομάδα συσχέτισης.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

1. Δημιουργήστε ένα εικονικό δίκτυο με την εισαγωγή ενός αρχείου ρύθμισης παραμέτρων.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Το αρχείο ρύθμισης παραμέτρων περιέχει το ακόλουθο έγγραφο XML. Σύντομη, καθορίζει ένα εικονικό δίκτυο που ονομάζεται **ContosoNET** στην ομάδα συσχέτισης που ονομάζεται **ContosoAG**και περιλαμβάνει το χώρο διευθύνσεων **10.10.0.0/16** και έχει δύο δευτερευόντων δικτύων, **10.10.1.0/24** και **10.10.2.0/24**, το οποίο είναι το υποδίκτυο εμπρός και πίσω υποδίκτυο, αντίστοιχα. Το εμπρός υποδίκτυο είναι όπου μπορείτε να τοποθετήσετε σε εφαρμογές προγράμματος-πελάτη, όπως το Microsoft SharePoint και το πίσω υποδίκτυο είναι όπου θα τοποθετήσετε το ΣΠΣ SQL Server. Εάν αλλάξετε τις μεταβλητές **$affinityGroupName** και **$virtualNetworkName** νωρίτερα, πρέπει επίσης να αλλάξετε τα αντίστοιχα ονόματα παρακάτω.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. Δημιουργία λογαριασμού χώρου αποθήκευσης που σχετίζεται με την ομάδα συσχέτισης που δημιουργήσατε και ορίστε την ως τον τρέχοντα λογαριασμό χώρου αποθήκευσης στη συνδρομή σας.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

1. Δημιουργία διακομιστή ελεγκτή Τομέα στο νέο cloud υπηρεσίας και τη διαθεσιμότητα σύνολο.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Αυτή η σειρά εκτροπή εντολές, κάντε τα εξής στοιχεία:

    - **Δημιουργία AzureVMConfig** δημιουργεί μια Εικονική ρύθμιση παραμέτρων.

    - **Προσθήκη AzureProvisioningConfig** παρέχει τη ρύθμιση παραμέτρων ενός μεμονωμένου διακομιστή των Windows.

    - **Προσθήκη AzureDataDisk** προσθέτει το δίσκο δεδομένων που θα χρησιμοποιήσετε για την αποθήκευση δεδομένων της υπηρεσίας καταλόγου Active Directory, με προσωρινή αποθήκευση καμία επιλογή.

    - **Δημιουργία AzureVM** δημιουργεί μια νέα υπηρεσία στο cloud και δημιουργεί τη νέα Εικονική Azure στην νέα υπηρεσία cloud.

1. Περιμένετε για τη νέα Εικονική για να είναι πλήρως προμήθεια και κάντε λήψη του απομακρυσμένου υπολογιστή αρχείου στον κατάλογο εργασίας. Επειδή το νέο Εικονική Azure διαρκεί πολύ χρόνο για την παροχή των, το χωρίς επανάληψη εξακολουθεί να ψηφοφορία με τη νέα Εικονική μέχρι να είναι έτοιμο για χρήση.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

Ο διακομιστής ελεγκτή Τομέα τώρα παρέχεται με επιτυχία. Στη συνέχεια, θα μπορείτε να ρυθμίσετε τον τομέα της υπηρεσίας καταλόγου Active Directory σε αυτόν το διακομιστή ελεγκτή Τομέα. Αφήστε ανοιχτό το παράθυρο του PowerShell στον τοπικό σας υπολογιστή. Θα χρησιμοποιήσετε πάλι αργότερα για τη δημιουργία του δύο ΣΠΣ SQL Server.

## <a name="configure-the-domain-controller"></a>Ρύθμιση παραμέτρων του ελεγκτή τομέα

1. Συνδεθείτε με το διακομιστή ελεγκτή Τομέα, εάν πραγματοποιηθεί εκκίνηση του απομακρυσμένου υπολογιστή αρχείου. Χρησιμοποιήστε το όνομα χρήστη AzureAdmin και τον κωδικό πρόσβασης του διαχειριστή υπολογιστή **Contoso! 000**, που έχει καθοριστεί κατά τη δημιουργία του νέου Εικονική.

1. Ανοίξτε ένα παράθυρο του PowerShell σε κατάσταση λειτουργίας διαχειριστή.

1. Εκτελέστε την ακόλουθη **DCPROMO. EXE** εντολή για να ρυθμίσετε τον τομέα **corp.contoso.com** , με τα δεδομένα σε καταλόγους στη μονάδα δίσκου M.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Μόλις ολοκληρωθεί η εντολή, η Εικονική επανεκκίνηση του αυτόματα.

1. Συνδεθείτε με το διακομιστή ελεγκτή Τομέα ξανά, εάν πραγματοποιηθεί εκκίνηση του απομακρυσμένου υπολογιστή αρχείου. Αυτήν τη στιγμή, συνδεθείτε ως **CORP\Administrator**.

1. Ανοίξτε ένα παράθυρο του PowerShell σε κατάσταση λειτουργίας διαχειριστή και εισαγωγή της λειτουργικής μονάδας Active Directory PowerShell χρησιμοποιώντας την ακόλουθη εντολή:

        Import-Module ActiveDirectory

1. Εκτελέστε τις ακόλουθες εντολές για να προσθέσετε τρία χρήστες στον τομέα.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** χρησιμοποιείται για τη ρύθμιση παραμέτρων όλα τα στοιχεία που σχετίζονται με τις παρουσίες υπηρεσία SQL Server, το σύμπλεγμα WSFC και στην ομάδα διαθεσιμότητα. **CORP\SQLSvc1** και **CORP\SQLSvc2** χρησιμοποιούνται ως τους λογαριασμούς υπηρεσίας του SQL Server για τα δύο ΣΠΣ SQL Server.

1. Στη συνέχεια, εκτελέστε τις ακόλουθες εντολές για να δώσετε **CORP\Install** τα δικαιώματα για τη δημιουργία αντικειμένων υπολογιστή στον τομέα.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    Το GUID που αναφέρονται παραπάνω είναι το GUID για τον τύπο του αντικειμένου υπολογιστή. Ο λογαριασμός **CORP\Install** πρέπει το δικαίωμα **Ανάγνωσης όλες τις ιδιότητες** και **Δημιουργία αντικειμένων υπολογιστή** για να δημιουργήσετε ένα ενεργό απευθείας αντικειμένων για το σύμπλεγμα WSFC. Το δικαίωμα **Ανάγνωσης όλες τις ιδιότητες** ήδη δίνεται σε CORP\Install από προεπιλογή, επομένως δεν χρειάζεται να εκχωρήσετε το ρητά. Για περισσότερες πληροφορίες σχετικά με τα δικαιώματα που απαιτούνται για να δημιουργήσετε το σύμπλεγμα WSFC, ανατρέξτε στο θέμα [Οδηγός βήμα προς βήμα σύμπλεγμα ανακατεύθυνσης: ρύθμιση παραμέτρων λογαριασμών στην υπηρεσία καταλόγου Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Τώρα που έχετε ολοκληρώσει τη ρύθμιση των παραμέτρων υπηρεσίας καταλόγου Active Directory και των αντικειμένων χρήστη, θα δημιουργήσετε δύο SQL Server ΣΠΣ και συμμετοχή τους σε αυτόν τον τομέα.

## <a name="create-the-sql-server-vms"></a>Δημιουργία του ΣΠΣ SQL Server

1. Συνεχίστε να χρησιμοποιείτε το παράθυρο του PowerShell που είναι ανοικτό στον τοπικό σας υπολογιστή. Ορίστε τις ακόλουθες πρόσθετες μεταβλητές:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    Η διεύθυνση IP **10.10.0.4** αντιστοιχίζεται συνήθως την πρώτη Εικονική δημιουργείτε στο δευτερεύον **10.10.0.0/16** του Azure εικονικού δικτύου σας. Θα πρέπει να μπορείτε να επαληθεύσετε αυτή είναι η διεύθυνση του διακομιστή σας ελεγκτή Τομέα, εκτελώντας **IPCONFIG**.

1. Εκτελέστε τις ακόλουθες εκτροπή εντολές για να δημιουργήσετε την πρώτη εικονική Μηχανή στο σύμπλεγμα WSFC, με το όνομα **ContosoQuorum**:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Λάβετε υπόψη τα εξής σχετικά με την παραπάνω εντολή:

    - **Δημιουργία AzureVMConfig** δημιουργεί μια Εικονική ρύθμιση παραμέτρων με το όνομα του συνόλου επιθυμητή διαθεσιμότητα. Τα επόμενα ΣΠΣ θα δημιουργηθεί με το ίδιο όνομα σύνολο διαθεσιμότητα ώστε να συνδέονται με το ίδιο σύνολο διαθεσιμότητα.

    - **Προσθήκη AzureProvisioningConfig** ενώνει τα Εικονική για τον τομέα Active Directory που δημιουργήσατε.

    - **Ορισμός AzureSubnet** τοποθετεί την εικονική Μηχανή στο δευτερεύον πίσω.

    - **Δημιουργία AzureVM** δημιουργεί μια νέα υπηρεσία στο cloud και δημιουργεί τη νέα Εικονική Azure στην νέα υπηρεσία cloud. Η παράμετρος **DnsSettings** Καθορίζει ότι ο διακομιστής DNS για τους διακομιστές στην νέα υπηρεσία cloud έχει το IP address **10.10.0.4**, που είναι η διεύθυνση IP του διακομιστή ελεγκτή Τομέα. Αυτή η παράμετρος είναι απαραίτητη για να ενεργοποιήσετε το νέο ΣΠΣ στην υπηρεσία cloud για να συμμετάσχετε στον τομέα της υπηρεσίας καταλόγου Active Directory με επιτυχία. Χωρίς αυτήν την παράμετρο, πρέπει να με μη αυτόματο τρόπο να ορίσετε τις ρυθμίσεις IPv4 σε Εικονική σας για να χρησιμοποιήσετε το Κέντρο Δεδομένων του διακομιστή ως πρωτεύοντα διακομιστή DNS μετά την εικονική Μηχανή παρέχεται και, στη συνέχεια, συμμετοχή η Εικονική στον τομέα της υπηρεσίας καταλόγου Active Directory.

1. Εκτελέστε τις ακόλουθες εκτροπή εντολές για τη δημιουργία του ΣΠΣ διακομιστή SQL, με το όνομα **ContosoSQL1** και **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Λάβετε υπόψη τα εξής σχετικά με τις εντολές της παραπάνω:

    - **Δημιουργία AzureVMConfig** χρησιμοποιεί το ίδιο όνομα διαθεσιμότητα του συνόλου με το διακομιστή ελεγκτή Τομέα, και χρησιμοποιεί την εικόνα του SQL Server 2012 Service Pack 1 Enterprise Edition στη συλλογή εικονική μηχανή. Ορίζει το δίσκο του λειτουργικού συστήματος με την ανάγνωση-προσωρινή αποθήκευση μόνο (χωρίς cache κατά την εγγραφή). Συνιστάται να μετεγκαταστήσετε τα αρχεία βάσης δεδομένων σε ένα δίσκο χωριστά δεδομένα που επισύναψη η Εικονική και ρυθμίστε τις παραμέτρους του με χωρίς ανάγνωση ή εγγραφή σε cache. Ωστόσο, το επόμενο καλύτερο είναι να καταργήσετε εγγραφής cache στο δίσκο το λειτουργικό σύστημα, εφόσον δεν μπορείτε να καταργήσετε διαβάζει cache στο δίσκο το λειτουργικό σύστημα.

    - **Προσθήκη AzureProvisioningConfig** ενώνει τα Εικονική για τον τομέα Active Directory που δημιουργήσατε.

    - **Ορισμός AzureSubnet** τοποθετεί την εικονική Μηχανή στο δευτερεύον πίσω.

    - **Προσθήκη AzureEndpoint** προσθέτει τα τελικά σημεία πρόσβασης ώστε να εφαρμογές προγράμματος-πελάτη να αποκτήσετε πρόσβαση σε αυτές τις περιπτώσεις υπηρεσίες SQL Server στο internet. Διαφορετικές θύρες δίνονται σε ContosoSQL1 και ContosoSQL2.

    - **Δημιουργία AzureVM** δημιουργεί τη νέα Εικονική SQL Server στην ίδια υπηρεσία cloud ως ContosoQuorum. Πρέπει να μπορείτε να τοποθετήσετε το ΣΠΣ στην ίδια υπηρεσία cloud, εάν θέλετε να είναι στο ίδιο σύνολο διαθεσιμότητα.

1. Περιμένετε για κάθε Εικονική για να είναι πλήρως προμήθεια και κάντε λήψη του απομακρυσμένου υπολογιστή αρχείο στον κατάλογο εργασίας. Η επανάληψη εναλλάξετε τα τρία νέα ΣΠΣ και εκτελεί τις εντολές στο εσωτερικό του ανώτατου επιπέδου άγκιστρα, για κάθε μία από αυτές.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    Τώρα έχουν παρασχεθεί του ΣΠΣ SQL Server και εκτελείται, αλλά αυτές είναι εγκατεστημένα με τον SQL Server με τις προεπιλεγμένες επιλογές.

## <a name="initialize-the-wsfc-cluster-vms"></a>Προετοιμασία του ΣΠΣ WSFC συμπλέγματος

Σε αυτήν την ενότητα, πρέπει να τροποποιήσετε το τρεις διακομιστές που θα χρησιμοποιήσετε στο σύμπλεγμα WSFC και την εγκατάσταση του SQL Server. Συγκεκριμένα:

- (Όλους τους διακομιστές) Πρέπει να εγκαταστήσετε τη δυνατότητα **Συμπλέγματος ανακατεύθυνσης** .

- (Όλους τους διακομιστές) Πρέπει να προσθέσετε **CORP\Install** ως **διαχειριστής**του υπολογιστή.

- (ContosoSQL1 και ContosoSQL2 μόνο) Πρέπει να προσθέσετε **CORP\Install** ως ένα ρόλο **sysadmin** στην προεπιλεγμένη βάση δεδομένων.

- (ContosoSQL1 και ContosoSQL2 μόνο) Πρέπει να προσθέσετε **NT AUTHORITY\System** ως μια σύνδεση με τα ακόλουθα δικαιώματα:

    - Τροποποίηση οποιαδήποτε ομάδας διαθεσιμότητας

    - Σύνδεση SQL

    - Κατάσταση προβολής διακομιστή

- (ContosoSQL1 και ContosoSQL2 μόνο) Το πρωτόκολλο **TCP** έχει ήδη ενεργοποιηθεί για την εικονική Μηχανή του SQL Server. Ωστόσο, εξακολουθείτε να χρειάζεστε για να ανοίξετε το τείχος προστασίας για απομακρυσμένη πρόσβαση του SQL Server.

Τώρα, είστε έτοιμοι να ξεκινήσετε. Ξεκινώντας με το **ContosoQuorum**, ακολουθήστε τα παρακάτω βήματα:

1. Συνδεθείτε στο **ContosoQuorum** , εάν πραγματοποιηθεί εκκίνηση τα αρχεία απομακρυσμένης επιφάνειας εργασίας. Χρησιμοποιήστε το όνομα χρήστη **AzureAdmin** και τον κωδικό πρόσβασης του διαχειριστή υπολογιστή **Contoso! 000**, που έχει καθοριστεί κατά τη δημιουργία του ΣΠΣ.

1. Βεβαιωθείτε ότι οι υπολογιστές έχουν συνδεθεί με επιτυχία σε **corp.contoso.com**.

1. Περιμένετε για την εγκατάσταση του SQL Server για να ολοκληρώσετε την εκτέλεση των εργασιών αυτοματοποιημένη προετοιμασίας πριν να συνεχίσετε.

1. Ανοίξτε ένα παράθυρο του PowerShell σε κατάσταση λειτουργίας διαχειριστή.

1. Εγκαταστήστε τη δυνατότητα συμπλέγματος ανακατεύθυνσης των Windows.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Προσθήκη **CORP\Install** ως τοπικού διαχειριστή.

        net localgroup administrators "CORP\Install" /Add

1. Αποσυνδεθείτε από το ContosoQuorum. Ολοκληρώσετε με αυτόν το διακομιστή τώρα.

        logoff.exe

Στη συνέχεια, προετοιμασία **ContosoSQL1** και **ContosoSQL2**. Ακολουθήστε τα παρακάτω βήματα, που είναι πανομοιότυπες για τα δύο του ΣΠΣ SQL Server.

1. Συνδεθείτε με τα δύο ΣΠΣ SQL Server, εάν πραγματοποιηθεί εκκίνηση τα αρχεία απομακρυσμένης επιφάνειας εργασίας. Χρησιμοποιήστε το όνομα χρήστη **AzureAdmin** και τον κωδικό πρόσβασης του διαχειριστή υπολογιστή **Contoso! 000**, που έχει καθοριστεί κατά τη δημιουργία του ΣΠΣ.

1. Βεβαιωθείτε ότι οι υπολογιστές έχουν συνδεθεί με επιτυχία σε **corp.contoso.com**.

1. Περιμένετε για την εγκατάσταση του SQL Server για να ολοκληρώσετε την εκτέλεση των εργασιών αυτοματοποιημένη προετοιμασίας πριν να συνεχίσετε.

1. Ανοίξτε ένα παράθυρο του PowerShell σε κατάσταση λειτουργίας διαχειριστή.

1. Εγκαταστήστε τη δυνατότητα συμπλέγματος ανακατεύθυνσης των Windows.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Προσθήκη **CORP\Install** ως τοπικού διαχειριστή

        net localgroup administrators "CORP\Install" /Add

1. Εισαγάγετε την υπηρεσία παροχής του PowerShell του SQL Server.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking

1. Προσθήκη **CORP\Install** με το ρόλο sysadmin για την προεπιλεγμένη παρουσία του SQL Server.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."

1. Προσθήκη **NT AUTHORITY\System** ως μια σύνδεση με τα τρία δικαιώματα που περιγράφονται παραπάνω.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."

1. Ανοίξτε το τείχος προστασίας για απομακρυσμένη πρόσβαση του SQL Server.

        netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP

1. Αποσυνδεθείτε από δύο ΣΠΣ.

        logoff.exe

Τέλος, είστε έτοιμοι να ρυθμίσετε τις παραμέτρους της ομάδας διαθεσιμότητας. Θα χρησιμοποιήσετε την υπηρεσία παροχής του PowerShell του SQL Server για να εκτελέσετε όλες τις εργασίες σε **ContosoSQL1**.

## <a name="configure-the-availability-group"></a>Ρύθμιση παραμέτρων της διαθεσιμότητας ομάδας

1. Συνδεθείτε στο **ContosoSQL1** ξανά, εάν πραγματοποιηθεί εκκίνηση τα αρχεία απομακρυσμένης επιφάνειας εργασίας. Αντί για καταγραφή χρησιμοποιώντας το λογαριασμό του υπολογιστή, συνδεθείτε χρησιμοποιώντας **CORP\Install**.

1. Ανοίξτε ένα παράθυρο του PowerShell σε κατάσταση λειτουργίας διαχειριστή.

1. Ορίστε τις παρακάτω μεταβλητές:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"

1. Εισαγωγή του PowerShell υπηρεσία παροχής SQL Server.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking

1. Αλλαγή του λογαριασμού υπηρεσίας SQL Server για ContosoSQL1 σε CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Αλλαγή του λογαριασμού υπηρεσίας SQL Server για ContosoSQL2 σε CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Κάντε λήψη του **CreateAzureFailoverCluster.ps1** από [Δημιουργία σύμπλεγμα WSFC για πάντα στη διαθεσιμότητα ομάδες σε Εικονική Azure](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) στον τοπικό κατάλογο εργασίας. Θα χρησιμοποιήσετε αυτήν τη δέσμη ενεργειών για να δημιουργήσετε ένα σύμπλεγμα λειτουργική WSFC. Για σημαντικές πληροφορίες σχετικά με τον τρόπο WSFC αλληλεπίδρασης με το δίκτυο του Azure, ανατρέξτε στο θέμα [Υψηλή διαθεσιμότητα και την αποκατάσταση για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-high-availability-dr.md).

1. Αλλαγή σε κατάλογο εργασίας και δημιουργήστε το σύμπλεγμα WSFC με τη δέσμη ενεργειών που έχετε λάβει.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"

1. Ενεργοποίηση πάντα σε ομάδες διαθεσιμότητα για τις παρουσίες του SQL Server προεπιλεγμένη σε **ContosoSQL1** και **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Δημιουργία αντιγράφου ασφαλείας καταλόγου και εκχώρηση δικαιωμάτων για τους λογαριασμούς υπηρεσίας του SQL Server. Θα χρησιμοποιήσετε αυτόν τον κατάλογο για την προετοιμασία της βάσης δεδομένων διαθεσιμότητας στη δευτερεύουσα ρεπλίκα.

        $backup = "C:\backup"
        New-Item $backup -ItemType directory
        net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
        icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")

1. Δημιουργία μιας βάσης δεδομένων σε **ContosoSQL1** που ονομάζεται **MyDB1**, λαμβάνουν ένα πλήρες αντίγραφο ασφαλείας και ένα αντίγραφο ασφαλείας του αρχείου καταγραφής και επαναφοράς τους σε **ContosoSQL2** με την επιλογή **Με NORECOVERY** .

        Invoke-SqlCmd -Query "CREATE database $db"
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery

1. Δημιουργήστε τη διαθεσιμότητα τελικά σημεία ομάδας σε του ΣΠΣ SQL Server και ορίστε τα κατάλληλα δικαιώματα στην τα τελικά σημεία.

        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server1\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"
        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server2\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"

        Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
        Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2

1. Δημιουργήστε αντίγραφα τη διαθεσιμότητα.

        $primaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server1 `
            -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate
        $secondaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server2 `
            -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate

1. Τέλος, να δημιουργήσετε την ομάδα διαθεσιμότητα και τη συμμετοχή στη δευτερεύουσα ρεπλίκα στην ομάδα διαθεσιμότητα.

        New-SqlAvailabilityGroup `
            -Name $ag `
            -Path "SQLSERVER:\SQL\$server1\Default" `
            -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
            -Database $db
        Join-SqlAvailabilityGroup `
            -Path "SQLSERVER:\SQL\$server2\Default" `
            -Name $ag
        Add-SqlAvailabilityDatabase `
            -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
            -Database $db

## <a name="next-steps"></a>Επόμενα βήματα
Έχετε τώρα με επιτυχία υλοποιήσει SQL Server πάντα στην, δημιουργώντας μια ομάδα διαθεσιμότητα στο Azure. Για να ρυθμίσετε τις παραμέτρους μιας υπηρεσίας ακρόασης για αυτήν την ομάδα διαθεσιμότητας, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ενός ILB ακρόασης για πάντα στη διαθεσιμότητα ομάδες στο Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Για άλλες πληροφορίες σχετικά με τη χρήση SQL Server Azure, ανατρέξτε στο θέμα [SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-server-iaas-overview.md).
