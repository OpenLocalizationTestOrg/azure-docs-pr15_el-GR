<properties
   pageTitle="Κρυπτογράφηση δίσκων σε μια Εικονική Linux | Microsoft Azure"
   description="Πώς μπορείτε να κρυπτογραφήσετε δίσκων σε μια Εικονική Linux χρησιμοποιώντας το CLI Azure και το μοντέλο ανάπτυξης για τη διαχείριση πόρων"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/11/2016"
   ms.author="iainfou"/>

# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli"></a>Κρυπτογράφηση δίσκων σε μια Εικονική Linux χρησιμοποιώντας το CLI Azure
Για βελτιωμένη εικονική μηχανή (Εικονική) ασφάλεια και συμμόρφωση, μπορεί να είναι κρυπτογραφημένα εικονικών δίσκων στο Azure στο υπόλοιπο. Δίσκων είναι κρυπτογραφημένα με χρήση κλειδιών κρυπτογράφησης που είναι ασφαλείς σε ένα θάλαμο κλειδί Azure. Μπορείτε να ελέγξετε αυτών των κλειδιών κρυπτογράφησης και να ελέγξετε τη χρήση τους. Αυτό το άρθρο περιγράφει τον τρόπο κρυπτογράφησης εικονικών δίσκων σε μια Εικονική Linux χρησιμοποιώντας το CLI Azure και το μοντέλο ανάπτυξης διαχείρισης πόρων.


## <a name="quick-commands"></a>Γρήγορες εντολές
Εάν πρέπει να εκτελέσετε γρήγορα την εργασία, τις ακόλουθες λεπτομέρειες ενότητα τη βάση εντολές για την κρυπτογράφηση εικονικών δίσκων στην Εικονική σας. Πιο λεπτομερείς πληροφορίες περιβάλλοντος για κάθε βήμα μπορείτε να βρείτε και το υπόλοιπο του εγγράφου, [ξεκινώντας εδώ](#overview-of-disk-encryption).

Χρειάζεστε την [Πιο πρόσφατη CLI Azure](../xplat-cli-install.md) εγκαταστήσει και συνδεθείτε χρησιμοποιώντας τη λειτουργία διαχείρισης πόρων ως εξής:

```
azure config mode arm
```

Στα παρακάτω παραδείγματα, αντικαταστήστε τα ονόματα παραμέτρων παράδειγμα με τις δικές σας τιμές. Συμπεριλάβετε τα ονόματα παραμέτρων παράδειγμα `myResourceGroup`, `myKeyVault`, και `myVM`.

Πρώτα, ενεργοποιήστε την υπηρεσία παροχής θάλαμο κλειδί Azure εντός Azure τη συνδρομή σας και δημιουργήστε μια ομάδα πόρων. Το ακόλουθο παράδειγμα δημιουργεί ένα όνομα ομάδας πόρων `myResourceGroup` στο το `WestUS` θέση:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Δημιουργήστε ένα Azure κλειδιού θάλαμο. Το ακόλουθο παράδειγμα δημιουργεί μια θάλαμο πλήκτρο με το όνομα `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Δημιουργία ενός κλειδιού κρυπτογράφησης στο θάλαμο τον αριθμό-κλειδί και ενεργοποιήστε το για την κρυπτογράφηση δίσκο. Το ακόλουθο παράδειγμα δημιουργεί έναν αριθμό-κλειδί που ονομάζεται `myKey`:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Δημιουργήστε ένα τελικό σημείο χρησιμοποιώντας Azure Active Directory για το χειρισμό ο έλεγχος ταυτότητας και ανταλλαγή των κλειδιών κρυπτογράφησης από θάλαμο αριθμού-κλειδιού. Το `--home-page` και `--identifier-uris` δεν χρειάζεται να είναι πραγματικές δυνατότητα δρομολόγησης διεύθυνση. Για το υψηλότερο επίπεδο ασφάλειας, πρέπει να χρησιμοποιείται απόρρητο προγράμματος-πελάτη αντί για τους κωδικούς πρόσβασης. Το Azure CLI αυτήν τη στιγμή δεν μπορεί να δημιουργήσει απόρρητο προγράμματος-πελάτη. Απόρρητο προγράμματος-πελάτη μπορεί να δημιουργηθεί μόνο στην πύλη του Azure. Το ακόλουθο παράδειγμα δημιουργεί ένα τελικό σημείο Azure Active Directory με το όνομα `myAADApp` και χρησιμοποιεί έναν κωδικό πρόσβασης του `myPassword`. Καθορίστε το δικό σας κωδικό πρόσβασης ως εξής:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Σημείωση το `applicationId` εμφανίζεται στο αποτέλεσμα από την προηγούμενη εντολή. Σε αυτό το Αναγνωριστικό εφαρμογής χρησιμοποιείται για τα παρακάτω βήματα:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Προσθέστε ένα δίσκο δεδομένων σε μια υπάρχουσα Εικονική. Το παρακάτω παράδειγμα προσθέτει ένα δίσκο δεδομένων σε μια Εικονική με το όνομα `myVM`:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Εξετάστε τις λεπτομέρειες για τον αριθμό-κλειδί θάλαμο και τον αριθμό-κλειδί που δημιουργήσατε. Χρειάζεστε το Αναγνωριστικό θάλαμο κλειδιού, URI και κλειδί διεύθυνση URL στο τελικό βήμα. Το παρακάτω παράδειγμα εξετάζει τις λεπτομέρειες για ένα θάλαμο πλήκτρο με το όνομα `myKeyVault` και κλειδί με το όνομα `myKey`:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Κρυπτογράφηση δίσκων σας ως εξής, πληκτρολογώντας τα δικά σας ονόματα παραμέτρων σε όλο το:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Το CLI Azure δεν παρέχει λεπτομερή σφάλματα κατά τη διαδικασία κρυπτογράφησης. Για πρόσθετες πληροφορίες αντιμετώπισης προβλημάτων, εξετάστε `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Κατά την προηγούμενη εντολή έχει πολλές μεταβλητές και ενδέχεται να μην έχουν πολύ ένδειξη ως προς γιατί η διαδικασία αποτυγχάνει, ένα παράδειγμα ολοκλήρωσης εντολής θα είναι ως εξής:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Τέλος, ελέγξτε την κατάσταση κρυπτογράφησης ξανά για να επιβεβαιώσετε ότι έχετε πλέον κρυπτογραφημένα εικονικού δίσκων σας. Το παρακάτω παράδειγμα ελέγχει την κατάσταση της μια Εικονική με το όνομα `myVM` στο το `myResourceGroup` ομάδα πόρων:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Επισκόπηση της κρυπτογράφησης δίσκου
Εικονικών δίσκων σε VM Linux είναι κρυπτογραφημένα στο υπόλοιπο χρησιμοποιώντας [dm κρυπτογραφημένου](https://wikipedia.org/wiki/Dm-crypt). Δεν υπάρχει χωρίς χρέωση για την κρυπτογράφηση εικονικών δίσκων στο Azure. Κλειδιών κρυπτογράφησης είναι αποθηκευμένες σε θάλαμο κλειδί Azure χρησιμοποιώντας την προστασία λογισμικού, ή μπορείτε να εισαγάγετε ή να δημιουργούν των αριθμών-κλειδιών σε λειτουργικές μονάδες ασφαλείας υλικού (HSMs) πιστοποιηθεί για FIPS 140-2 επιπέδου 2 πρότυπα. Μπορείτε να διατηρήσετε τον έλεγχο των αυτών των κλειδιών κρυπτογράφησης και να ελέγξετε τη χρήση τους. Αυτά τα κλειδιά κρυπτογράφησης που χρησιμοποιούνται για την κρυπτογράφηση και αποκρυπτογράφηση εικονικών δίσκων που έχουν επισυναφθεί σε Εικονική σας. Ένα τελικό σημείο Azure Active Directory παρέχει έναν ασφαλή μηχανισμό για την έκδοση αυτών των κλειδιών κρυπτογράφησης, όπως ΣΠΣ είναι ενεργοποιημένος και απενεργοποίηση.

Η διαδικασία για την κρυπτογράφηση μια Εικονική είναι ως εξής:

1. Δημιουργία ενός κλειδιού κρυπτογράφησης σε ένα θάλαμο κλειδί Azure.
2. Ρυθμίστε τις παραμέτρους του κλειδιού κρυπτογράφησης για να μπορεί να χρησιμοποιηθεί για την κρυπτογράφηση δίσκων.
3. Για να διαβάσετε το κλειδί κρυπτογράφησης από το Azure θάλαμο αριθμού-κλειδιού, δημιουργήστε ένα τελικό σημείο χρησιμοποιώντας Azure Active Directory με τα κατάλληλα δικαιώματα.
4. Εκδώσετε την εντολή για την κρυπτογράφηση εικονικού δίσκων σας, που καθορίζει το τελικό σημείο Azure Active Directory και κατάλληλου κλειδιού κρυπτογράφησης που θα χρησιμοποιηθεί.
5. Το τελικό σημείο Azure Active Directory ζητάει την απαιτούμενη κλειδιού κρυπτογράφησης από θάλαμο κλειδί Azure.
6. Το εικονικών δίσκων είναι κρυπτογραφημένα με χρήση του κλειδιού κρυπτογράφησης που παρέχεται.


## <a name="supporting-services-and-encryption-process"></a>Υποστήριξη υπηρεσιών και διαδικασία κρυπτογράφησης
Κρυπτογράφηση δίσκου εξαρτάται από τα ακόλουθα πρόσθετα στοιχεία:

- **Azure κλειδί θάλαμο** - χρησιμοποιείται για την προστασία των κλειδιών κρυπτογράφησης και απορρήτου που χρησιμοποιείται για τη διαδικασία Κρυπτογράφηση/αποκρυπτογράφηση δίσκο. 
  - Εάν υπάρχει, μπορείτε να χρησιμοποιήσετε μια υπάρχουσα θάλαμο κλειδί Azure. Δεν χρειάζεται να χρησιμοποιηθεί ένα θάλαμο κλειδί για την κρυπτογράφηση δίσκων.
  - Για να διαχωρίσετε διαχείρισης όρια και ορατότητα κλειδιού, μπορείτε να δημιουργήσετε μια αποκλειστική θάλαμο αριθμού-κλειδιού.
- **Azure Active Directory** - χειρίζεται την ασφαλή ανταλλαγή απαιτείται κλειδιών κρυπτογράφησης και ελέγχου ταυτότητας για τις απαιτούμενες ενέργειες. 
  - Συνήθως, μπορείτε να χρησιμοποιήσετε μια υπάρχουσα εμφάνιση Azure Active Directory για κατοικιών την εφαρμογή σας. 
  - Η εφαρμογή είναι περισσότερα από ένα τελικό σημείο για τις υπηρεσίες του αριθμού-κλειδιού θάλαμο και εικονική μηχανή για να ζητήσετε και λάβετε εκδοθεί τα κατάλληλα κλειδιά κρυπτογράφησης. Δεν σχεδιάζετε μια πραγματική εφαρμογή που ενοποιείται με το Azure Active Directory.


## <a name="requirements-and-limitations"></a>Απαιτήσεις και περιορισμοί
Σενάρια που υποστηρίζονται και τις απαιτήσεις για την κρυπτογράφηση δίσκου:

- Το παρακάτω Linux διακομιστή SKU - Ubuntu, CentOS, SUSE και SUSE Linux Enterprise Server (SLES) και κόκκινο καπέλο Enterprise Linux.
- Όλοι οι πόροι (όπως θάλαμο κλειδί λογαριασμού χώρου αποθήκευσης και Εικονική) πρέπει να είναι στην ίδια περιοχή Azure και τη συνδρομή.
- Πρότυπο Α, D, DS, G και GS σειρά ΣΠΣ.

Κρυπτογράφηση δίσκου δεν υποστηρίζεται αυτήν τη στιγμή στα ακόλουθα σενάρια:

- Βασικό επίπεδο ΣΠΣ.
- ΣΠΣ που δημιουργούνται με χρήση του μοντέλου ανάπτυξης κλασική.
- Απενεργοποίηση της κρυπτογράφησης δίσκου λειτουργικού Συστήματος σε VM Linux.
- Ενημέρωση τα κλειδιά κρυπτογράφησης σε μια ήδη κρυπτογραφημένο Εικονική Linux.


## <a name="create-the-azure-key-vault-and-keys"></a>Δημιουργήστε τα πλήκτρα και θάλαμο κλειδί Azure
Για να ολοκληρώσετε το υπόλοιπο αυτού του οδηγού, χρειάζεστε την [Πιο πρόσφατη CLI Azure](../xplat-cli-install.md) εγκαταστήσει και συνδεθείτε χρησιμοποιώντας τη λειτουργία διαχείρισης πόρων ως εξής:

```
azure config mode arm
```

Σε ολόκληρη την εντολή παραδείγματα, αντικαταστήστε όλες τις παραμέτρους παράδειγμα με τα δικά σας ονόματα, θέση και τιμές κλειδιού. Τα παρακάτω παραδείγματα χρησιμοποιούν ένα Συνέδριο της `myResourceGroup`, `myKeyVault`, `myAADApp`κ.λπ.

Το πρώτο βήμα είναι να δημιουργήσετε ένα θάλαμο Azure αριθμού-κλειδιού για την αποθήκευση των κλειδιών κρυπτογράφησης. Azure θάλαμο κλειδί μπορούν να αποθηκεύουν πλήκτρα, απόρρητο ή τους κωδικούς πρόσβασης που σας επιτρέπουν να εφαρμόσετε με ασφάλεια σε εφαρμογές και τις υπηρεσίες σας. Για την κρυπτογράφηση εικονικό δίσκο, μπορείτε να χρησιμοποιήσετε θάλαμο αριθμού-κλειδιού για την αποθήκευση ενός κλειδιού κρυπτογράφησης που χρησιμοποιείται για να κρυπτογραφήσετε ή να αποκρυπτογραφήσετε εικονικού δίσκων σας. 

Ενεργοποιήστε την υπηρεσία παροχής θάλαμο κλειδί Azure στη συνδρομή σας στο Azure και, στη συνέχεια, δημιουργήστε μια ομάδα πόρων. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα πόρων με το όνομα `myResourceGroup` στο το `WestUS` θέση:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Το θάλαμο κλειδί Azure που περιέχει το κλειδιών κρυπτογράφησης και σχετικές υπολογισμού πόρους όπως χώρου αποθήκευσης και η Εικονική ίδια πρέπει να βρίσκονται στην ίδια περιοχή. Το ακόλουθο παράδειγμα δημιουργεί μια θάλαμο κλειδί Azure με το όνομα `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Μπορείτε να αποθηκεύσετε κλειδιών κρυπτογράφησης χρησιμοποιώντας το λογισμικό ή το μοντέλο ασφάλειας υλικού (HSM) προστασίας. Χρήση μιας HSM απαιτεί μια premium θάλαμο αριθμού-κλειδιού. Υπάρχει ένα πρόσθετο κόστος για τη δημιουργία ενός premium θάλαμο κλειδί και όχι τυπική θάλαμο κλειδί όπου αποθηκεύονται τα πλήκτρα που προστατεύεται με λογισμικό. Για να δημιουργήσετε ένα πλήκτρο θάλαμο premium, στο προηγούμενο βήμα προσθήκη `--sku Premium` στην εντολή. Το παρακάτω παράδειγμα χρησιμοποιεί προστατεύεται με λογισμικό κλειδιά εφόσον που δημιουργήσαμε μια τυπική θάλαμο αριθμού-κλειδιού. 

Και οι δύο μοντέλα προστασίας, την πλατφόρμα Azure πρέπει να σας έχει εκχωρηθεί πρόσβαση για να ζητήσετε τα κλειδιά κρυπτογράφησης κατά την εκκίνηση του Εικονική να αποκρυπτογραφήσετε τη εικονικών δίσκων. Δημιουργία ενός κλειδιού κρυπτογράφησης μέσα σε θάλαμο τον αριθμό-κλειδί και, στη συνέχεια, ενεργοποιήσετε για χρήση με κρυπτογράφηση εικονικό δίσκο. Το ακόλουθο παράδειγμα δημιουργεί έναν αριθμό-κλειδί που ονομάζεται `myKey` και, στη συνέχεια, επιτρέπει την κρυπτογράφησης δίσκο:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Δημιουργία της εφαρμογής Azure Active Directory
Όταν εικονικών δίσκων είναι κρυπτογραφημένα ή αποκρυπτογράφηση, μπορείτε να χρησιμοποιήσετε ένα τελικό σημείο για να χειριστείτε τον έλεγχο ταυτότητας και την ανταλλαγή των κλειδιών κρυπτογράφησης από θάλαμο αριθμού-κλειδιού. Αυτό το τελικό σημείο, μια εφαρμογή του Azure Active Directory, επιτρέπει την πλατφόρμα Azure για να ζητήσετε από το κατάλληλο κλειδιών κρυπτογράφησης εκ μέρους του Εικονική. Μια προεπιλεγμένη παρουσία Azure Active Directory είναι διαθέσιμη στη συνδρομή σας, αν και πολλοί οργανισμοί έχουν αφοσιωμένη σε καταλόγους Azure Active Directory.

Καθώς δημιουργείτε δεν μια πλήρη εφαρμογή Azure Active Directory, την `--home-page` και `--identifier-uris` παράμετροι στο παρακάτω παράδειγμα, δεν χρειάζεται να είναι πραγματικές δυνατότητα δρομολόγησης διεύθυνση. Το παρακάτω παράδειγμα καθορίζει επίσης ένα μυστικό με κωδικό πρόσβασης αντί για δημιουργία κλειδιά από εντός της πύλης Azure. Ως αυτήν τη στιγμή, δημιουργίας κλειδιών δεν είναι δυνατό να γίνει από το Azure CLI. 

Δημιουργήστε την εφαρμογή του Azure Active Directory. Το παρακάτω παράδειγμα δημιουργεί μια εφαρμογή με το όνομα `myAADApp` και χρησιμοποιεί έναν κωδικό πρόσβασης του `myPassword`. Καθορίστε το δικό σας κωδικό πρόσβασης ως εξής:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Σημειώστε την `applicationId` που επιστρέφεται από την προηγούμενη εντολή στο αποτέλεσμα. Σε αυτό το Αναγνωριστικό εφαρμογής χρησιμοποιείται σε ορισμένα από τα υπόλοιπα βήματα. Στη συνέχεια, δημιουργήστε ένα κύριο όνομα υπηρεσίας (κύριο όνομα Υπηρεσίας), έτσι ώστε η εφαρμογή είναι δυνατή η πρόσβαση στο περιβάλλον σας. Για να κρυπτογραφήσετε ή να αποκρυπτογραφήσετε εικονικών δίσκων με επιτυχία, δικαιώματα του κλειδιού κρυπτογράφησης είναι αποθηκευμένα σε θάλαμο κλειδί πρέπει να οριστεί στην επιτρέπουν την εφαρμογή Azure Active Directory για να διαβάσετε τα πλήκτρα. 

Δημιουργήστε το κύριο όνομα Υπηρεσίας και ορίστε τα κατάλληλα δικαιώματα ως εξής:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Προσθήκη ενός εικονικού δίσκου και ανασκόπηση της κατάστασης κρυπτογράφησης
Για να κρυπτογραφήσετε στην πραγματικότητα ορισμένες εικονικών δίσκων, σας επιτρέπει να προσθέσετε ένα δίσκο στις υπάρχουσες Εικονική μηχανή. Προσθέστε ένα δίσκο 5Gb δεδομένων σε μια υπάρχουσα Εικονική ως εξής:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Το εικονικών δίσκων δεν είναι κρυπτογραφημένα. Εξετάστε την τρέχουσα κατάσταση κρυπτογράφησης Εικονική σας ως εξής:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Κρυπτογράφηση εικονικών δίσκων
Για να κρυπτογραφήσετε τώρα το εικονικών δίσκων, που συνδυάζουν τα προηγούμενα στοιχεία:

1. Καθορίστε την εφαρμογή Azure Active Directory και τον κωδικό πρόσβασης.
2. Καθορίστε το θάλαμο αριθμού-κλειδιού για την αποθήκευση των μετα-δεδομένων για το κρυπτογραφημένο δίσκων σας.
3. Καθορίστε τα κλειδιά κρυπτογράφησης για να χρησιμοποιήσετε για την πραγματική κρυπτογράφηση και αποκρυπτογράφηση.
4. Καθορίστε εάν θέλετε να κρυπτογραφήσετε το δίσκο λειτουργικό σύστημα, την δίσκων δεδομένων ή όλα.

Σας επιτρέπει να δείτε τις λεπτομέρειες για το θάλαμο κλειδί Azure και τον αριθμό-κλειδί που δημιουργήσατε, χρειάζεστε το Αναγνωριστικό θάλαμο αριθμού-κλειδιού, URI, και, στη συνέχεια, βασικές διεύθυνση URL στο τελικό βήμα:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Κρυπτογράφηση εικονικού δίσκων σας χρησιμοποιώντας το αποτέλεσμα από την `azure keyvault show` και `azure keyvault key show` εντολές, ως εξής:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Κατά την προηγούμενη εντολή έχει πολλές μεταβλητές, το ακόλουθο παράδειγμα είναι η πλήρης εντολή για αναφορά:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Το CLI Azure δεν παρέχει λεπτομερή σφάλματα κατά τη διαδικασία κρυπτογράφησης. Για πρόσθετες πληροφορίες αντιμετώπισης προβλημάτων, εξετάστε `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` σε η Εικονική που η κρυπτογράφηση.

Τέλος, σας επιτρέπει να εξετάσετε την κατάσταση κρυπτογράφησης ξανά για να επιβεβαιώσετε ότι έχετε πλέον κρυπτογραφημένα εικονικού δίσκων σας:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Προσθήκη δίσκων πρόσθετα δεδομένα
Αφού έχετε κρυπτογραφημένα δίσκων σας δεδομένα, μπορείτε αργότερα να προσθέσετε επιπλέον εικονικών δίσκων σας Εικονική και επίσης, να κρυπτογραφήσετε τους. Όταν εκτελείτε το `azure vm enable-disk-encryption` εντολής, να αυξήσετε την έκδοση ακολουθίας που χρησιμοποιούν το `--sequence-version` παραμέτρου. Αυτή η παράμετρος ακολουθίας έκδοση σας επιτρέπει να εκτελέσετε επαναλαμβανόμενων πράξεων με το ίδιο Εικονική.

Για παράδειγμα, σας επιτρέπει να προσθέσετε έναν δεύτερο εικονικό δίσκο για να σας Εικονική ως εξής:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Εκτελέστε ξανά την εντολή για να κρυπτογραφήσετε το εικονικών δίσκων, αυτή η προσθήκη ώρας του `--sequence-version` παραμέτρου και προσαύξηση την τιμή από την πρώτη εκτέλεση ως εξής:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Επόμενα βήματα

- Για περισσότερες πληροφορίες σχετικά με τη Διαχείριση θάλαμο κλειδί Azure, συμπεριλαμβανομένων των διαγραφή κλειδιών κρυπτογράφησης και χώροι φύλαξης, ανατρέξτε στο θέμα [Διαχείριση θάλαμο κλειδί με χρήση CLI](../key-vault/key-vault-manage-with-cli.md).
- Για περισσότερες πληροφορίες σχετικά με την κρυπτογράφηση δίσκου, όπως η προετοιμασία του προσαρμοσμένου Εικονική μηχανή κρυπτογραφημένο για αποστολή στο Azure, ανατρέξτε στην ενότητα [Κρυπτογράφηση δίσκου Azure](../security/azure-security-disk-encryption.md).