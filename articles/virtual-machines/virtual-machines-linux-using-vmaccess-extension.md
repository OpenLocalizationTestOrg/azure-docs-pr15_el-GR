<properties
    pageTitle="Επαναφορά πρόσβασης στο ΣΠΣ Linux Azure χρησιμοποιώντας την επέκταση VMAccess | Microsoft Azure"
    description="Επαναφορά πρόσβασης στο ΣΠΣ Linux Azure χρησιμοποιώντας την επέκταση VMAccess."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Διαχείριση χρηστών, SSH και ελέγχου ή επιδιόρθωση δίσκων σε ΣΠΣ Linux Azure χρησιμοποιώντας την επέκταση VMAccess

Σε αυτό το άρθρο σάς δείχνει πώς μπορείτε να χρησιμοποιήσετε την επέκταση VMAcesss Azure για να ελέγξετε ή επιδιόρθωση ένα δίσκο, επαναφορά πρόσβασης των χρηστών, Διαχείριση λογαριασμών χρηστών ή επαναφορά της ρύθμισης παραμέτρων SSHD σε Linux. Το άρθρο απαιτεί τα εξής:

- Azure λογαριασμού ([λάβετε μια δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/)).

- το [Azure CLI](../xplat-cli-install.md) συνδεθεί με `azure login`.

- στη λειτουργία διαχείρισης πόρων Azure _πρέπει να είναι σε_ Azure CLI `azure config mode arm`.

## <a name="quick-commands"></a>Γρήγορες εντολές

Υπάρχουν δύο τρόποι χρήσης του VMAccess σας ΣΠΣ Linux:

- Χρήση του Azure CLI και τις απαιτούμενες παραμέτρους.
- Χρήση ανεπεξέργαστα αρχεία JSON που VMAccess επεξεργάζεται και, στη συνέχεια, ενεργούν.

Για την ενότητα γρήγορη εντολή, θα σας πρόκειται να χρησιμοποιήσετε το Azure CLI `azure vm reset-access` μέθοδο. Στα παρακάτω παραδείγματα εντολή, αντικαταστήστε τις τιμές που περιέχουν "παράδειγμα" με τις τιμές από τη δική σας περιβάλλον.

## <a name="create-a-resource-group-and-linux-vm"></a>Δημιουργήστε μια ομάδα πόρων και Εικονική Linux

```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Δημιουργήστε μια Εικονική Debian

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-g myResourceGroup \
-l westus \
-y Linux \
-n myVM \
-Q Debian
```

## <a name="reset-root-password"></a>Επαναφορά κωδικού πρόσβασης ρίζας

Για να επαναφέρετε τον κωδικό πρόσβασης του ριζικού:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u root \
-p myNewPassword
```

## <a name="ssh-key-reset"></a>Επαναφορά κλειδιού SSH

Για να επαναφέρετε το κλειδί SSH μη ριζικό χρήστη:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Δημιουργία χρήστη

Για να δημιουργήσετε ένα χρήστη:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-p myAdminUserPassword
```

## <a name="remove-a-user"></a>Κατάργηση χρήστη

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-R myRemovedUser
```

## <a name="reset-sshd"></a>Επαναφορά SSHD

Για να επαναφέρετε τις ρυθμίσεις παραμέτρων SSHD:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM
-r
```


## <a name="detailed-walkthrough"></a>Λεπτομερή ανάλυση

### <a name="vmaccess-defined"></a>VMAccess που ορίζονται από:

Ο δίσκος σας Εικονική Linux εμφανίζει σφάλματα. Με κάποιον τρόπο επαναφέρει τον κωδικό πρόσβασής ρίζας σας Εικονική Linux ή διαγραφεί κατά λάθος σας SSH ιδιωτικό κλειδί. Εάν που συνέβη πίσω στο τις ημέρες του κέντρου δεδομένων, θα πρέπει να υπάρχει η μονάδα δίσκου και, στη συνέχεια, ανοίξτε το KVM για να αξιοποιήσετε στο την κονσόλα του διακομιστή. Σκεφτείτε την επέκταση Azure VMAccess ως που διακόπτη KVM που σας επιτρέπει να αποκτήσετε πρόσβαση στην κονσόλα για να επαναφέρετε access Linux ή συντήρηση επιπέδου δίσκου.

Για τη λεπτομερή ανάλυση, θα σας πρόκειται να χρησιμοποιήσετε την αναλυτική μορφή VMAccess, η οποία χρησιμοποιεί ανεπεξέργαστα JSON αρχεία.  Αυτά τα αρχεία VMAccess JSON ονομάζεται επίσης και από τα πρότυπα που Azure.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Χρήση VMAccess για να ελέγξετε ή να επιδιορθώσετε το δίσκο από μια Εικονική Linux

Χρήση VMAccess μπορείτε να κάνετε μια fsck εκτελέστε στο δίσκο στην περιοχή σας Εικονική Linux.  Μπορείτε επίσης να κάνετε ένα σημάδι ελέγχου δίσκου και ένα δίσκο επιδιόρθωση, χρησιμοποιώντας μια VMAccess.

Για να ελέγξετε και, στη συνέχεια, να επιδιορθώσετε το δίσκο Χρησιμοποιήστε αυτήν τη δέσμη ενεργειών VMAccess:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Εκτέλεση της δέσμης ενεργειών VMAccess με:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Χρήση VMAccess για να επαναφέρετε πρόσβασης χρήστη Linux

Εάν έχετε χάσει access στη ρίζα στην σας Εικονική Linux, μπορείτε να εκκινήσετε μια δέσμη ενεργειών VMAccess για να επαναφέρετε τον κωδικό πρόσβασης του ριζικού.

Για να επαναφέρετε τον κωδικό πρόσβασης του ριζικού, χρησιμοποιήστε αυτήν τη δέσμη ενεργειών VMAccess:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

Εκτέλεση της δέσμης ενεργειών VMAccess με:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Για να επαναφέρετε το κλειδί SSH μη ριζικό χρήστη, χρησιμοποιήστε αυτήν τη δέσμη ενεργειών VMAccess:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

Εκτέλεση της δέσμης ενεργειών VMAccess με:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>Χρήση VMAccess για τη Διαχείριση λογαριασμών χρηστών στο Linux

VMAccess είναι μια δέσμη ενεργειών Python που μπορούν να χρησιμοποιηθούν για να διαχειριστείτε τους χρήστες σας Εικονική Linux χωρίς σύνδεση στο και με τη χρήση sudo ή το λογαριασμό ριζικό κατάλογο.

Για να δημιουργήσετε ένα χρήστη, χρησιμοποιήστε αυτήν τη δέσμη ενεργειών VMAccess:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

Εκτέλεση της δέσμης ενεργειών VMAccess με:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Για να διαγράψετε ένα χρήστη, χρησιμοποιήστε αυτήν τη δέσμη ενεργειών VMAccess:

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

Εκτέλεση της δέσμης ενεργειών VMAccess με:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>Χρήση VMAccess για να επαναφέρετε τις ρυθμίσεις παραμέτρων SSHD

Εάν κάνετε αλλαγές στη ρύθμιση παραμέτρων SSHD ΣΠΣ Linux και κλείστε τη σύνδεση SSH πριν την επαλήθευση τις αλλαγές, ενδέχεται να μην μπορείτε να SSH'ing ξανά.  VMAccess μπορεί να χρησιμοποιηθεί για να επαναφέρετε τις ρυθμίσεις παραμέτρων SSHD μια γνωστή καλή ρύθμιση παραμέτρων χωρίς να είστε συνδεδεμένοι πάνω από SSH.

Για να επαναφέρετε τις ρυθμίσεις παραμέτρων SSHD Χρησιμοποιήστε αυτήν τη δέσμη ενεργειών VMAccess:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Εκτέλεση της δέσμης ενεργειών VMAccess με:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Επόμενα βήματα

Ενημέρωση Linux χρήση Azure VMAccess επεκτάσεις είναι μία μέθοδος για να κάνετε αλλαγές σε μια Εικονική εκτελείται Linux.  Μπορείτε επίσης να χρησιμοποιήσετε εργαλεία όπως το cloud την προετοιμασία και το Azure πρότυπα για να τροποποιήσετε το Εικονική Linux κατά την εκκίνηση.

[Σχετικά με τις δυνατότητες και επεκτάσεις εικονική μηχανή](virtual-machines-linux-extensions-features.md)

[Σύνταξη από κοινού από διαχειριστή πόρων Azure πρότυπα με επεκτάσεις Εικονική Linux](virtual-machines-linux-extensions-authoring-templates.md)

[Χρησιμοποιώντας την προετοιμασία cloud για να προσαρμόσετε μια Εικονική Linux κατά τη δημιουργία](virtual-machines-linux-using-cloud-init.md)
