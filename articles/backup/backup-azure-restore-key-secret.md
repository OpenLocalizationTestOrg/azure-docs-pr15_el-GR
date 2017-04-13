<properties
    pageTitle="Επαναφορά αριθμού-κλειδιού θάλαμο κλειδί και μυστικό για κρυπτογραφημένο ΣΠΣ χρήση των αντιγράφων ασφαλείας Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να επαναφέρετε κλειδί θάλαμο κλειδί και μυστικό στο αντίγραφο ασφαλείας Azure χρησιμοποιώντας το PowerShell"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Επαναφορά αριθμού-κλειδιού θάλαμο κλειδί και μυστικό για κρυπτογραφημένο ΣΠΣ χρήση των αντιγράφων ασφαλείας Azure
Σε αυτό το άρθρο ακρόασης σχετικά με τη χρήση αντίγραφο ασφαλείας Εικονική Azure για να εκτελέσετε επαναφορά της κρυπτογραφημένης Azure ΣΠΣ, εάν το κλειδί και μυστικό δεν υπάρχουν στο του κλειδιού θάλαμο. Αυτά τα βήματα μπορεί επίσης να χρησιμοποιηθεί, εάν θέλετε να διατηρήσετε ένα ξεχωριστό αντίγραφο του κλειδιού (πλήκτρο αριθμού-κλειδιού κρυπτογράφησης) και μυστικό (κλειδιού κρυπτογράφησης BitLocker) για την επαναφορά εικονική Μηχανή.

## <a name="pre-requisites"></a>Προαπαιτούμενα

1. **Δημιουργία αντιγράφων ασφαλείας κρυπτογραφημένα ΣΠΣ** - κρυπτογραφημένα ΣΠΣ Azure έχουν δημιουργηθεί αντίγραφα ασφαλείας χρησιμοποιώντας Azure αντίγραφο ασφαλείας. Ανατρέξτε στο άρθρο [Διαχείριση αντιγράφων ασφαλείας και επαναφοράς του Azure ΣΠΣ χρήση του PowerShell](backup-azure-vms-automation.md) για λεπτομέρειες σχετικά με τον τρόπο δημιουργίας αντιγράφων ασφαλείας για κρυπτογραφημένη ΣΠΣ Azure.

2. **Ρύθμιση παραμέτρων θάλαμο κλειδί Azure** – βεβαιωθείτε ότι βασικών θάλαμο στο οποίο πλήκτρα και απόρρητο πρέπει να γίνει επαναφορά υπάρχει ήδη. Ανατρέξτε στο άρθρο [Γρήγορα αποτελέσματα με το Azure θάλαμο αριθμού-κλειδιού](../key-vault/key-vault-get-started.md) για λεπτομέρειες σχετικά με τη διαχείριση του κλειδιού θάλαμο.

## <a name="setup-recovery-services-vault"></a>Ρύθμιση θάλαμο υπηρεσίες ανάκτησης 
Χρησιμοποιήστε τα ακόλουθα βήματα για να συνδεθείτε με το PowerShell και ορισμός περιβάλλοντος θάλαμο υπηρεσίες ανάκτησης

### <a name="log-in-to-azure-powershell"></a>Συνδεθείτε στο Azure PowerShell 

Συνδεθείτε στο λογαριασμό Azure χρησιμοποιώντας το ακόλουθο cmdlet

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Ορισμός αποκατάστασης υπηρεσίες θάλαμο περιβάλλοντος

Αφού συνδεθεί σε, χρησιμοποιήστε το ακόλουθο cmdlet για να λάβετε τη λίστα των συνδρομών σας διαθέσιμη

```
PS C:\> Get-AzureRmSubscription
```

Επιλέξτε τη συνδρομή στην οποία υπάρχουν διαθέσιμοι πόροι

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

Ορίστε το περιβάλλον θάλαμο χρησιμοποιώντας υπηρεσίες ανάκτησης θάλαμο όπου δημιουργίας αντιγράφων ασφαλείας έχει ενεργοποιηθεί για κρυπτογραφημένο ΣΠΣ

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Λήψη σημείο αποκατάστασης 

Επιλέξτε κοντέινερ στο το θάλαμο που αντιπροσωπεύει το κρυπτογραφημένο Azure εικονική μηχανή

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

Χρησιμοποιώντας αυτό το κοντέινερ, να επιστρέψετε στο στοιχείο για την αντίστοιχη εικονική μηχανή

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Λήψη ενός πίνακα σημείων αποκατάστασης για το επιλεγμένο στοιχείο δημιουργίας αντιγράφων ασφαλείας του μεταβλητής rp

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Επαναφορά κρυπτογραφημένων εικονική μηχανή
Χρησιμοποιήστε τα ακόλουθα βήματα για να επαναφέρετε κρυπτογραφημένα Εικονική το κλειδί και μυστικό.

### <a name="restore-key"></a>Επαναφορά αριθμού-κλειδιού

Ο πίνακας $rp παραπάνω, ταξινομείται με αντίστροφη σειρά του χρόνου με την πιο πρόσφατη σημείο αποκατάστασης στο ευρετήριο 0. Για παράδειγμα: $rp [0] επιλέγει την πιο πρόσφατη σημείο αποκατάστασης.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
Μετά από αυτό το cmdlet εκτελείται με επιτυχία, ένα αρχείο blob λαμβάνει που δημιουργήθηκε στον καθορισμένο φάκελο στον υπολογιστή όπου εκτελείται. Αυτό το αρχείο blob αντιπροσωπεύει αριθμό-κλειδί κρυπτογραφημένα αριθμού-κλειδιού σε κρυπτογραφημένη μορφή.

Επαναφέρετε την κλειδιού πίσω του κλειδιού θάλαμο χρησιμοποιώντας το ακόλουθο cmdlet. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Επαναφορά μυστικό

Επαναφορά μυστικού δεδομένων από το σημείο αποκατάστασης που λαμβάνονται παραπάνω

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
Το κείμενο πριν από την vault.azure.net αντιπροσωπεύει το αρχικό όνομα κλειδιού θάλαμο. Το κείμενο μετά απόρρητο / αντιπροσωπεύει μυστικού όνομα. 

Λάβετε το μυστικό όνομα και την τιμή από την έξοδο από το cmdlet εκτέλεση παραπάνω, σε περίπτωση που θέλετε να χρησιμοποιήσετε το ίδιο όνομα μυστικό. Σε άλλες περιπτώσεις, πρέπει να ενημερωθούν $secretname παρακάτω για να χρησιμοποιήσετε το νέο όνομα μυστικό. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

Ορισμός ετικέτες για το μυστικό, σε περίπτωση που εικονική Μηχανή πρέπει να ανακτηθούν καθώς και. Για την ετικέτα DiskEncryptionKeyFileName, η τιμή πρέπει να περιέχουν το όνομα του το μυστικό που σχεδιάζετε να χρησιμοποιήσετε. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
Τιμή για το DiskEncryptionKeyFileName είναι το ίδιο με μυστικού όνομα που λαμβάνονται παραπάνω. Τιμή για το DiskEncryptionKeyEncryptionKeyURL μπορεί να ληφθεί από θάλαμο κλειδιού μετά την επαναφορά ξανά τα πλήκτρα και χρησιμοποιώντας το cmdlet [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx)   

Ορισμός το μυστικό πίσω μέχρι το θάλαμο κλειδιού

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>Επαναφέρετε εικονική μηχανή
Το παραπάνω cmdlet του PowerShell σας βοηθήσει να επαναφέρετε κλειδί και μυστικού πίσω του κλειδιού θάλαμο, εάν έχετε δημιουργήσει αντίγραφα κρυπτογραφημένο Εικονική χρήση των αντιγράφων ασφαλείας Εικονική Azure. Μετά την επαναφορά τους, ανατρέξτε στο άρθρο [Διαχείριση αντιγράφων ασφαλείας και επαναφοράς του Azure ΣΠΣ χρήση του PowerShell](backup-azure-vms-automation.md) για να επαναφέρετε κρυπτογραφημένα ΣΠΣ.
