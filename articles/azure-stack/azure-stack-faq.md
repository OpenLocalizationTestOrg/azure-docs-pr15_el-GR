<properties
    pageTitle="Συνήθεις ερωτήσεις για τη στοίβα Azure | Microsoft Azure"
    description="Azure στοίβας συνήθεις ερωτήσεις."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Συνήθεις ερωτήσεις για τη στοίβα Azure

## <a name="deployment"></a>Ανάπτυξη

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>Πρέπει να μορφοποιήσετε μου δίσκων δεδομένων πριν από την έναρξη ή επανεκκίνηση μιας εγκατάστασης;

Δίσκων πρέπει να είναι σε μορφή ανεπεξέργαστα. Εάν κάνετε επανεγκατάσταση του λειτουργικού συστήματος, ίσως χρειαστεί να ελέγξετε εάν το παλιό χώρο συγκέντρωσης χώρου αποθήκευσης είναι εξακολουθούν να υπάρχουν και να διαγράψετε τα παρακάτω βήματα:

1. Ανοίξτε τη Διαχείριση διακομιστή.
2. Επιλέξτε τα σύνολα χώρου αποθήκευσης.
3. Δείτε εάν εμφανίζεται ένα χώρο συγκέντρωσης χώρου αποθήκευσης.
4. Κάντε δεξί κλικ **χώρου συγκέντρωσης χώρου αποθήκευσης** , εάν παρατίθενται και ενεργοποιήσετε την ανάγνωση / εγγραφή.
5. Κάντε δεξί κλικ **εικονικού σκληρού δίσκου** (κάτω αριστερή γωνία) και επιλέξτε Διαγραφή.
6. Κάντε δεξιό κλικ στο **Χώρο αποθήκευσης στο χώρο συγκέντρωσης** και κάντε κλικ στην επιλογή Διαγραφή.
7. Εκκινήστε πάλι στοίβας Azure δέσμης ενεργειών και βεβαιωθείτε ότι η επαλήθευση δίσκου μεταβιβάζει.

Προαιρετικά, μπορεί να χρησιμοποιηθεί η ακόλουθη δέσμη ενεργειών:

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>Μπορώ να χρησιμοποιήσω όλων των δίσκων SSD για το χώρο αποθήκευσης στην εγκατάσταση POC;

Αυτή η ρύθμιση παραμέτρων δεν υποστηρίζεται σε αυτήν την έκδοση.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Οδηγός απαιτήσεις](azure-stack-deploy.md) για περισσότερες πληροφορίες.

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>Μπορώ να χρησιμοποιήσω δίσκων NVMe δεδομένων για το Microsoft Azure στοίβας POC;

Ενώ αποθήκευσης κενά διαστήματα απευθείας υποστηρίζει δίσκων NVMe, στοίβα Azure υποστηρίζει μόνο ένα υποσύνολο των τύπων πιθανές μονάδα δίσκου και δυνατούς συνδυασμούς για απευθείας αποθήκευσης κενά διαστήματα. 

### <a name="how-can-i-reinstall-azure-stack"></a>Πώς μπορώ να εγκαταστήσω ξανά το Azure στοίβα;
Μπορείτε να ακολουθήσετε τα βήματα στον [Οδηγό επανάληψη ανάπτυξης](azure-stack-redeploy.md).  

## <a name="tenant"></a>Μισθωτή

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>Μπορώ να αναπτύξω το δικό μου εικόνες ως μισθωτή;

Ναι, όπως ακριβώς στο Azure, μισθωτή να αποστείλετε εικόνες σε στοίβα Azure, εκτός από χρησιμοποιώντας τις εικόνες από το διαχειριστή της υπηρεσίας. Για μια επισκόπηση, ανατρέξτε στο θέμα η [Προσθήκη μιας εικόνας Εικονική](azure-stack-add-vm-image.md). 

## <a name="testing"></a>Δοκιμές

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>Μπορώ να χρησιμοποιήσω ένθετων συναρτήσεων Virtualization για να ελέγξετε το Microsoft Azure στοίβας POC;

Ένθετες virtualization δεν υποστηρίζεται ή ελεγχθεί με Azure στοίβας Technical Preview 2.

## <a name="virtual-machines"></a>Εικονικές μηχανές

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Στοίβα Azure υποστηρίζει δυναμικών δίσκων για εικονικές μηχανές;

Azure στοίβας δεν υποστηρίζει δυναμικών δίσκων.

## <a name="next-steps"></a>Επόμενα βήματα

[Αντιμετώπιση προβλημάτων](azure-stack-troubleshooting.md)
