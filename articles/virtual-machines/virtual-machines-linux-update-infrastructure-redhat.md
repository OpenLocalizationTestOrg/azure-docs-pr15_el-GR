<properties
   pageTitle="Κόκκινο καπέλο Update υποδομή (RHUI) | Microsoft Azure"
   description="Μάθετε σχετικά με το κόκκινο καπέλο Update υποδομή (RHUI) για τις παρουσίες κόκκινο καπέλο Enterprise Linux σε ζήτηση στο Microsoft Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="borisb"/>

# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Κόκκινο καπέλο Update υποδομή (RHUI) για on demand κόκκινο καπέλο Enterprise Linux ΣΠΣ στο Azure

Εικονικές μηχανές που δημιουργήθηκε από τις εικόνες Linux Enterprise καπέλο κόκκινο χρώμα (RHEL) σε ζήτηση διαθέσιμα από το Azure Marketplace έχετε καταχωρήσει για να αποκτήσετε πρόσβαση το μοντέλο κόκκινο καπέλο Update υποδομή (RHUI) που αναπτύσσεται στο Azure.  Οι εμφανίσεις RHEL στην απαιτήσεων έχουν πρόσβαση σε ένα αποθετήριο δεδομένων τοπικές yum και μπορούν να λαμβάνουν ενημερώσεις αυξάνονται.

Λίστα αποθετήριο yum, την οποία γίνεται από RHUI, έχει ρυθμιστεί στην παρουσία RHEL κατά την προμήθεια του. Δεν χρειάζεται να κάνετε οποιαδήποτε επιπλέον ρύθμιση παραμέτρων - εκτελέστε `yum update` μετά την παρουσία RHEL σας είναι έτοιμη για να λάβετε τις πιο πρόσφατες ενημερώσεις.

> [AZURE.NOTE] Azure υποδομή RHUI έχει ενημερωθεί πρόσφατα (Σεπτεμβρίου 2016) και απαιτεί αλλαγές στη ρύθμιση παραμέτρων του υπάρχοντος παρουσίες RHEL για απρόσκοπτη πρόσβαση σε το RHUI Azure. Ανατρέξτε στην ενότητα Update υποδομή Azure RHUI για λεπτομέρειες.


## <a name="rhui-azure-infrastructure-update"></a>RHUI Update υποδομή του Azure
Το 2016 Σεπτεμβρίου, Azure έχει ένα νέο σύνολο διακομιστών κόκκινο καπέλο Update υποδομή (RHUI). Αυτοί οι διακομιστές αναπτύσσονται με τη [Διαχείριση κίνηση Azure]( https://azure.microsoft.com/services/traffic-manager/) ώστε ένα μεμονωμένο τελικό σημείο (rhui-1.micrsoft.com) μπορεί να χρησιμοποιηθεί από οποιαδήποτε Εικονική ανεξάρτητα από την περιοχή. Μπορούν επίσης να χρησιμοποιήσουν ένα πιστοποιητικό SSL που είναι συνδεδεμένη με με τις γνωστές αρχή έκδοσης πιστοποιητικών (Baltimore ρίζα). Πραγματοποίηση αυτόματης αυτής της ενημερωμένης έκδοσης θα ήταν επικίνδυνος για ορισμένες τους πελάτες που έχουν ACL ή προσαρμοσμένο πίνακες δρομολόγησης για τους διακομιστές ενημέρωση RHUI, ώστε να είναι αυτής της ενημερωμένης έκδοσης "επιλέξετε στο." Μη αυτόματα βήματα για προσθήκης λογαριασμών σε αυτούς τους διακομιστές νέα είναι διαθέσιμες σε αυτήν τη σελίδα και μια ολοκληρωμένη δέσμη ενεργειών για προσθήκης λογαριασμών με ένα αυτοματοποιημένο τρόπο (κατά την επαλήθευση μεμονωμένα βήματα). Οι νέες RHEL PAYG εικόνες από το Azure Marketplace (χρονολογημένη Σεπτεμβρίου 2016 ή νεότερες εκδόσεις) θα αυτόματα οδηγούν στους διακομιστές του νέου Azure RHUI και δεν απαιτούν οποιαδήποτε πρόσθετη ενέργεια.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>Νέα λωρίδα χρόνου προσθήκης λογαριασμών υποδομή Azure RHUI

| Ημερομηνία | Σημείωση |
| --- | --- |
|22 Σεπτεμβρίου 2016 | Οι διακομιστές RHUI και τις οδηγίες εγκατάστασης που είναι διαθέσιμη για χρήση. ΣΠΣ αναπτυχθεί με τη χρήση του νέου (ημερομηνία 2016 Σεπτεμβρίου) RHEL PAYG marketplace εικόνες θα χρησιμοποιήσει αυτόματα τους νέους διακομιστές RHUI, αλλά είναι υπάρχουσα ΣΠΣ "επιλέξετε στο"
|1 Νοεμβρίου 2016 | Παλαιού τύπου εικόνες Εικονική PAYG RHEL (που χρησιμοποιούν το παλιό διακομιστές Azure RHUI) θα καταργηθεί από τη συλλογή Azure Marketplace
|16 Ιανουαρίου 2017 | Το παλιό διακομιστές Azure RHUI θα είναι εκτός λειτουργίας. Ενημέρωση όλων των σας που επηρεάζεται ΣΠΣ RHEL PAYG με αυτήν τη στιγμή για να διατηρήσετε την πρόσβαση στις Azure RHUI

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>Είναι το διευθύνσεις IP για το νέο διακομιστές RHUI παράδοσης περιεχομένου

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Μη αυτόματη ενημέρωση διαδικασία για να χρησιμοποιήσετε τους διακομιστές του νέου Azure RHUI

Λήψη (μέσω καμπύλη) την υπογραφή δημόσιο κλειδί

```
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Επαλήθευση του κλειδιού που έχουν ληφθεί

```
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Ελέγξτε το αποτέλεσμα, επιβεβαιώστε `keyid` και `user ID packet`:

```
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

Εγκαταστήστε το δημόσιο κλειδί

```
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Λήψη, επαληθεύστε και εγκατάσταση του προγράμματος-πελάτη RPM

Λήψη: Για RHEL 6

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Για RHEL 7

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Επαλήθευση:

```
rpm -Kv azureclient.rpm
```

Έλεγχος στο αποτέλεσμα αυτήν την υπογραφή του πακέτου είναι OK

```
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Εγκαταστήστε το RPM

```
sudo rpm -U azureclient.rpm
```

Μετά την ολοκλήρωση, βεβαιωθείτε ότι έχετε πρόσβαση σε φόρμα Azure RHUI την εικονική Μηχανή

### <a name="all-in-one-script-for-automating-the-above-task"></a>Όλα σε ένα δέσμη ενεργειών για την αυτοματοποίηση της παραπάνω εργασίας
Χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών όπως απαιτείται για την αυτοματοποίηση την εργασία της ενημέρωσης που επηρεάζεται ΣΠΣ του νέου τους διακομιστές Azure RHUI.

```
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>Επισκόπηση RHUI
[Κόκκινο καπέλο Update υποδομή](https://access.redhat.com/products/red-hat-update-infrastructure) προσφέρει μια ιδιαίτερα μεταβλητού μεγέθους λύση για τη Διαχείριση περιεχομένου αποθετήριο yum για παρουσίες cloud κόκκινο καπέλο Enterprise Linux που φιλοξενούνται από υπηρεσίες παροχής cloud πιστοποίηση καπέλο κόκκινο χρώμα. Με βάση την επόμενη έργου πολτό, RHUI επιτρέπει υπηρεσίες παροχής cloud για να τοπικά αντικριστά που φιλοξενείται στο κόκκινο καπέλο αποθετήριο περιεχομένου, Δημιουργία προσαρμοσμένου αποθετήρια με το δικό τους περιεχόμενο και να κάνετε αυτές τις αποθετήρια διαθέσιμη σε μια μεγάλη ομάδα τελικούς χρήστες μέσω ενός συστήματος εξισορρόπηση φόρτου περιεχομένου παράδοσης.

## <a name="regions-where-rhui-is-available"></a>Περιοχές όπου είναι διαθέσιμο RHUI
RHUI είναι διαθέσιμο σε όλες τις περιοχές όπου υπάρχουν διαθέσιμες εικόνες σε ζήτηση RHEL. Προς το παρόν περιλαμβάνει δημόσια όλες τις περιοχές που αναφέρονται στη σελίδα [πίνακα εργαλείων Azure κατάστασης](https://azure.microsoft.com/status/) και τις περιοχές Azure ΜΑΣ για δημόσιους οργανισμούς. RHUI πρόσβασης για την παροχή της υπηρεσίας από τις εικόνες στη ζήτηση RHEL ΣΠΣ περιλαμβάνεται στην τιμή τους. Διαθεσιμότητα επιπλέον cloud τοπικές ρυθμίσεις/national θα ενημερώνεται καθώς επεκτείνουμε διαθεσιμότητα σε ζήτηση RHEL στο μέλλον.

> [AZURE.NOTE] Πρόσβαση σε φιλοξενούμενο Azure RHUI περιορίζεται στα του ΣΠΣ μέσα σε [περιοχές διευθύνσεων IP του Microsoft Azure κέντρου δεδομένων](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="get-updates-from-another-update-repository"></a>Λήψη ενημερώσεων από ένα άλλο αποθετήριο update

Εάν χρειάζεστε για να λάβετε ενημερώσεις από ένα αποθετήριο διαφορετική ενημέρωση (αντί για το Azure που φιλοξενείται RHUI) θα πρέπει να unregister σας παρουσιών από RHUI και καταχωρήστε ξανά με το επιθυμητή update υποδομή (όπως δορυφορική καπέλο κόκκινο ή κόκκινο χρώμα καπέλο πελατών πύλη CDN). Θα χρειαστεί κατάλληλες συνδρομές καπέλο κόκκινο για αυτές τις υπηρεσίες και την καταγραφή για [Πρόσβαση στο Cloud καπέλο κόκκινο στο Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).

Για να unregister RHUI και να επαναλάβετε την καταχώρηση για να την υποδομή ενημερωμένη έκδοση, ακολουθήστε τα παρακάτω βήματα.

1.  Επεξεργασία /etc/yum.repos.d/rh-cloud.repo και να αλλάξετε όλες `enabled=1` να `enabled=0`. Για παράδειγμα:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.  Επεξεργασία /etc/yum/pluginconf.d/rhnplugin.conf και να αλλάξετε `enabled=0` να `enabled=1`.
3.  Στη συνέχεια, καταχωρήστε την επιθυμητή υποδομή, όπως η πύλη πελάτη καπέλο κόκκινο χρώμα. Ακολουθήστε Οδηγός λύσης καπέλο κόκκινο σχετικά [με την καταχώρηση και την εγγραφή ενός συστήματος στην πύλη πελάτη καπέλο κόκκινο χρώμα](https://access.redhat.com/solutions/253273).

> [AZURE.NOTE] Πρόσβαση σε το RHUI Azure που φιλοξενείται περιλαμβάνεται στην τιμή εικόνα RHEL Pay-As-You-Go (PAYG). Κατάργηση καταχώρησης μια Εικονική RHEL PAYG από το RHUI Azure που φιλοξενείται δεν μετατρέπει η εικονική μηχανή στον τύπο μεταφορά σας-κάτοχος-αδειών χρήσης (BYOL) Εικονική και ως εκ τούτου που μπορεί να τις διπλές χρεώσεις εάν μπορείτε να καταχωρήσετε την ίδια εικονική Μηχανή με κάποια άλλη προέλευση ενημερώσεις. 
> 
> Εάν χρειάζεστε με συνέπεια για να χρησιμοποιήσετε μια update υποδομή εκτός από Azure που φιλοξενείται RHUI μπορείτε να τη δημιουργία και ανάπτυξη τις δικές σας εικόνες (τύπος BYOL), όπως περιγράφεται στο άρθρο [Δημιουργία και αποστολή καπέλο κόκκινο βάσει εικονική μηχανή για Azure](virtual-machines-linux-redhat-create-upload-vhd.md) .

## <a name="next-steps"></a>Επόμενα βήματα
Για να δημιουργήσετε μια Εικονική κόκκινο καπέλο Enterprise Linux από εικόνα Azure Marketplace Pay-As-You-Go και αξιοποίηση Azure που φιλοξενείται RHUI μεταβείτε [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). Θα μπορείτε να χρησιμοποιήσετε `yum update` στην παρουσία RHEL χωρίς οποιαδήποτε επιπλέον εγκατάστασης.