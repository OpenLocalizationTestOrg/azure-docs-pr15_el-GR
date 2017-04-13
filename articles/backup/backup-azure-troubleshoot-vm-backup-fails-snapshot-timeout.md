<properties
   pageTitle="Azure αντίγραφο ασφαλείας Εικονική αποτυγχάνει: δεν ήταν δυνατή η επικοινωνία με τον παράγοντα Εικονική για κατάσταση στιγμιότυπο - έληξε εργασίας sub Εικονική στιγμιότυπο | Microsoft Azure"
   description="Συμπτώματα αιτίες και λύσεις για Εικονική Azure αποτυχίες αντιγράφου ασφαλείας που σχετίζονται με δεν ήταν δυνατή η επικοινωνία με τον παράγοντα Εικονική για κατάσταση στιγμιότυπο. Στιγμιότυπο Εικονική sub εργασίας έληξε σφάλματος"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="cfreeman"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jimpark; markgal;genli"/>

# <a name="azure-vm-backup-fails-could-not-communicate-with-the-vm-agent-for-snapshot-status---snapshot-vm-sub-task-timed-out"></a>Azure αντίγραφο ασφαλείας Εικονική αποτυγχάνει: δεν ήταν δυνατή η επικοινωνία με τον παράγοντα Εικονική για κατάσταση στιγμιότυπο - έληξε Εικονική στιγμιότυπο sub εργασίας

## <a name="summary"></a>Σύνοψη

Μετά την καταχώρηση και τον προγραμματισμό μια εικονική μηχανή (Εικονική) για το αντίγραφο ασφαλείας Azure, την υπηρεσία Azure αντιγράφου ασφαλείας προετοιμάζει την εργασία αντιγράφου ασφαλείας κατά την προγραμματισμένη ώρα κατά την επικοινωνία με την επέκταση αντιγράφου ασφαλείας σε η Εικονική για να καταγράψετε ένα στιγμιότυπο σε δεδομένη χρονική στιγμή. Υπάρχουν συνθήκες που ενδέχεται να μην μπορεί το στιγμιότυπο ενεργοποίησε που οδηγεί σε μια αποτυχία δημιουργίας αντιγράφων ασφαλείας. Σε αυτό το άρθρο παρέχει βήματα αντιμετώπισης προβλημάτων για ζητήματα που σχετίζονται με Εικονική Azure αποτυχίες αντιγράφου ασφαλείας που σχετίζονται με σφάλμα χρονικού ορίου στιγμιότυπο.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Σύμπτωμα

Δημιουργία αντιγράφων ασφαλείας Microsoft Azure για την υποδομή ως υπηρεσία (IaaS) Εικονική αποτυγχάνει και επιστρέφει το ακόλουθο μήνυμα σφάλματος σε τις λεπτομέρειες του σφάλματος εργασίας στην [πύλη του Azure](https://portal.azure.com/):

**Δεν ήταν δυνατή η επικοινωνία με τον παράγοντα Εικονική για κατάσταση στιγμιότυπο - έληξε Εικονική στιγμιότυπο sub εργασίας.**

## <a name="cause"></a>Αιτία
Υπάρχουν τέσσερις συνήθεις αιτίες για αυτό το σφάλμα:

- Η Εικονική δεν έχει πρόσβαση στο Internet.
- Ο παράγοντας Microsoft Azure Εικονική εγκατασταθεί σε η Εικονική έχει λήξει (για ΣΠΣ Linux).
- Το αντίγραφο ασφαλείας επέκταση αποτυγχάνει για να ενημερώσετε ή φόρτωση.
- Η κατάσταση στιγμιότυπα δεν μπορεί να ανακτηθεί ή δεν μπορούν να ληφθούν τα στιγμιότυπα.

## <a name="cause-1-the-vm-does-not-have-internet-access"></a>Αιτία 1: Η Εικονική δεν έχει πρόσβαση στο Internet
Ανά την απαίτηση ανάπτυξης, η Εικονική χωρίς πρόσβαση στο Internet, ή έχει περιορισμούς στη θέση που εμποδίζουν την πρόσβαση σε Azure υποδομής.

Η επέκταση αντιγράφου ασφαλείας απαιτούν σύνδεση με το Azure δημόσιες διευθύνσεις IP για να λειτουργήσουν σωστά. Αυτό συμβαίνει επειδή αποστέλλει εντολές τελικού σημείου αποθήκευσης Azure (HTTP URL) για να διαχειριστείτε τα στιγμιότυπα από την εικονική Μηχανή. Εάν η επέκταση δεν έχει πρόσβαση στο Internet δημόσια, αποτυγχάνει τελικά το αντίγραφο ασφαλείας.

### <a name="solution"></a>Λύση
Σε αυτό το σενάριο, χρησιμοποιήστε μία από τις παρακάτω μεθόδους για να επιλύσετε το πρόβλημα:

- Περιοχές διευθύνσεων IP Whitelist το Azure κέντρο δεδομένων
- Δημιουργήστε μια διαδρομή κυκλοφορία HTTP ροή

### <a name="to-whitelist-the-azure-datacenter-ip-ranges"></a>Για να whitelist το Azure περιοχές διευθύνσεων IP του κέντρου δεδομένων

1. Αποκτήστε τη [λίστα του κέντρου δεδομένων του Azure διευθύνσεις IP](https://www.microsoft.com/download/details.aspx?id=41653) για να whitelisted.
2. Κατάργηση του αποκλεισμού του διευθύνσεις IP, χρησιμοποιώντας το cmdlet New-NetRoute. Εκτελέστε αυτό το cmdlet σε η Εικονική Azure σε ένα αναβαθμισμένο παράθυρο PowerShell (Εκτέλεση ως διαχειριστής).
3. Προσθήκη κανόνων για την ομάδα ασφαλείας δικτύου (NSG), εάν έχετε ένα για να επιτρέψετε την πρόσβαση του διευθύνσεις IP.

### <a name="to-create-a-path-for-http-traffic-to-flow"></a>Για να δημιουργήσετε μια διαδρομή για την κίνηση HTTP ροής

1. Εάν έχετε δικτύου περιορισμούς στη θέση (για παράδειγμα, μια NSG), αναπτύξτε ένα διακομιστή μεσολάβησης HTTP για να δρομολογήσετε την κυκλοφορία.
2. Εάν έχετε μια ομάδα ασφαλείας δικτύου (NSG), προσθέστε κανόνες για να επιτρέψετε την πρόσβαση στο Internet από το διακομιστή μεσολάβησης HTTP.

Μάθετε πώς μπορείτε να [ρυθμίσετε ένα διακομιστή μεσολάβησης HTTP για Εικονική αντίγραφα ασφαλείας](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-microsoft-azure-vm-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Αιτία 2: Εγκαταστήσει σε η Εικονική τον παράγοντα Microsoft Azure Εικονική έχει λήξει (για ΣΠΣ Linux)

### <a name="solution"></a>Λύση
Πιο που σχετίζονται με παράγοντας ή που σχετίζονται με την επέκταση αποτυχίες για ΣΠΣ Linux προκαλούνται από ζητήματα που επηρεάζουν τα παλιά παράγοντας Εικονική. Γενικά, τα πρώτα βήματα για να αντιμετωπίσετε αυτό το ζήτημα είναι οι εξής:

1. [Εγκαταστήστε την πιο πρόσφατη παράγοντας Εικονική Azure](https://github.com/Azure/WALinuxAgent).
2. Βεβαιωθείτε ότι ο παράγοντας Azure εκτελείται σε η Εικονική. Για να κάνετε αυτό, εκτελέστε την ακόλουθη εντολή:```ps -e```

    Εάν αυτή η διαδικασία δεν λειτουργεί, χρησιμοποιήστε τις παρακάτω εντολές για να επανεκκινήσετε το.

    Για Ubuntu:```service walinuxagent start```

    Για άλλες κατανομές: ''' Έναρξη waagent υπηρεσίας
```

3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).

4. Run a new test backup. If the failures persist, please collect logs from the following folders for further debugging.

    We require the following logs from the customer’s VM:

    - /var/lib/waagent/*.xml
    - /var/log/waagent.log
    - /var/log/azure/*

If we require verbose logging for waagent, follow these steps to enable this:

1. In the /etc/waagent.conf file, locate the following line:

    Enable verbose logging (y|n)

2. Change the **Logs.Verbose** value from n to y.
3. Save the change, and then restart waagent by following the previous steps in this section.

## Cause 3: The backup extension fails to update or load
If extensions cannot be loaded, then Backup fails because a snapshot cannot be taken.

### Solution
For Windows guests:

1. Verify that the iaasvmprovider service is enabled and has a startup type of automatic.
2. If this is not the configuration, enable the service to determine whether the next backup succeeds.

For Linux guests:

The latest version of VMSnapshot Linux (extension used by backup) is 1.0.91.0

If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

### To uninstall the extension

1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the particular VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **uninstall**.

This will cause the extension to be reinstalled during the next backup.

## Cause 4: The snapshots status cannot be retrieved or the snapshots cannot be taken
VM backup relies on issuing snapshot command to underlying storage. The backup can fail because it has no access to storage or because of a delay in snapshot task execution.

### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| ----- | ----- |
| VMs that have Microsoft SQL Server Backup configured. By default, VM Backup runs a VSS Full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server Backup is configured, snapshot execution delays may occur. | Set following registry key if you are experiencing backup failures because of snapshot issues.<br><br>[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| VM status is reported incorrectly because the VM is shut down in RDP. If you shut down the virtual machine in RDP, check the portal to determine whether that VM status is reflected correctly. | If it’s not, shut down the VM in the portal by using the ”Shutdown” option in the VM dashboard. |
| Many VMs from the same cloud service are configured to back up at the same time. | It’s a best practice to spread out the VMs from the same cloud service to have different backup schedules. |
| The VM is running at high CPU or memory usage. | If the VM is running at high CPU usage (more than 90 percent) or high memory usage, the snapshot task is queued and delayed and eventually times out. In this situation, try on-demand backup. |
|The VM cannot get host/fabric address from DHCP.|DHCP must be enabled inside the guest for IaaS VM Backup to work.  If the VM cannot get host/fabric address from DHCP response 245, then it cannot download ir run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. View more information about [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md).|
