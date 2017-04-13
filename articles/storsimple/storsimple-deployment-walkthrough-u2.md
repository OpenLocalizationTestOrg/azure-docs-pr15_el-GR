<properties 
   pageTitle="Ανάπτυξη συσκευή StorSimple (ενημερωμένη έκδοση 2) | Microsoft Azure"
   description="Περιγράφει τα βήματα και τις βέλτιστες πρακτικές για την ανάπτυξη της συσκευής StorSimple ενημερωμένη έκδοση 2 και την υπηρεσία."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="deploy-your-on-premises-storsimple-device-update-2"></a>Ανάπτυξη συσκευή StorSimple εσωτερικής εγκατάστασης (ενημερωμένη έκδοση 2)

> [AZURE.SELECTOR]
- [Ενημερωμένη έκδοση 2 και νεότερες εκδόσεις](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [Ενημέρωση 1](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [Έκδοση GA](../articles/storsimple/storsimple-deployment-walkthrough.md)

## <a name="overview"></a>Επισκόπηση

Καλώς ορίσατε στο Microsoft Azure StorSimple συσκευή ανάπτυξης. Αυτά τα προγράμματα εκμάθησης ανάπτυξης εφαρμογή StorSimple 8000 σειρά ενημερωμένη έκδοση 2. Αυτήν τη σειρά προγραμμάτων εκμάθησης περιλαμβάνει μια λίστα ελέγχου ρύθμισης παραμέτρων, τις προϋποθέσεις ρύθμισης παραμέτρων και βήματα λεπτομερή ρύθμισης παραμέτρων για τη συσκευή σας StorSimple.

Οι πληροφορίες σε αυτά τα προγράμματα εκμάθησης προϋποθέτει ότι έχουν αναθεωρηθεί η προφυλάξεις, και έγινε η αποσυσκευασία, racked, και συνδεδεμένο συσκευή StorSimple. Εάν εξακολουθείτε να χρειάζεστε για να εκτελέσετε αυτές τις εργασίες, ξεκινήστε με εξετάζει τις [προφυλάξεις](storsimple-safety.md). Ακολουθήστε τις οδηγίες συγκεκριμένη συσκευή αποσυσκευασία, rack ενεργοποίησης και καλώδιο τη συσκευή σας.

- [Αποσυσκευασία, rack ενεργοποίησης και καλώδιο σας 8100](storsimple-8100-hardware-installation.md)
- [Αποσυσκευασία, rack ενεργοποίησης και καλώδιο σας 8600](storsimple-8600-hardware-installation.md)

Θα χρειαστείτε δικαιώματα διαχειριστή για να ολοκληρώσετε τη διαδικασία εγκατάστασης και ρύθμισης παραμέτρων. Συνιστάται να εξετάσετε τη λίστα ελέγχου ρύθμισης παραμέτρων πριν να ξεκινήσετε. Η διαδικασία ανάπτυξης και ρύθμισης παραμέτρων μπορεί να χρειαστεί κάποιος χρόνος για να ολοκληρωθεί.

> [AZURE.NOTE] Οι πληροφορίες ανάπτυξης StorSimple δημοσιευτεί στην τοποθεσία Web του Microsoft Azure ισχύουν για συσκευές σειρά StorSimple 8000 μόνο. Για πλήρεις πληροφορίες σχετικά με τις συσκευές 7000 σειρά, ανατρέξτε στο θέμα: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Για πληροφορίες ανάπτυξης σειρά 7000, ανατρέξτε του [StorSimple συστήματος οδηγό γρήγορης εκκίνησης](http://onlinehelp.storsimple.com/111_Appliance/).

## <a name="deployment-steps"></a>Βήματα ανάπτυξης

Εκτέλεση αυτά τα απαραίτητα βήματα για να ρυθμίσετε τη συσκευή σας StorSimple και συνδέστε τη με την υπηρεσία StorSimple Manager. Εκτός από τα απαραίτητα βήματα, υπάρχουν προαιρετικά βήματα και διαδικασίες μπορεί να χρειαστεί κατά την ανάπτυξη. Τις οδηγίες βήμα προς βήμα ανάπτυξης υποδεικνύουν όταν πρέπει να εκτελέσετε κάθε ένα από τα παρακάτω προαιρετικά βήματα.


| Βήμα                                                                                   | Περιγραφή                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ΠΡΟΑΠΑΙΤΟΎΜΕΝΑ ΣΤΟΙΧΕΊΑ**                                                                      | Αυτά πρέπει να ολοκληρωθούν προετοιμασία για την ανάπτυξη επερχόμενες.                                                                                        |
| [Λίστα ελέγχου ρύθμισης παραμέτρων ανάπτυξης](#deployment-configuration-checklist)                                                     | Χρησιμοποιήστε αυτήν τη λίστα ελέγχου για τη συγκέντρωση και την καταγραφή πληροφοριών πριν και κατά την ανάπτυξη.                                                                       |
| [Προαπαιτούμενα ανάπτυξης](#deployment-prerequisites)                                                               | Αυτά τα επικύρωση του περιβάλλοντος είναι έτοιμη για ανάπτυξη.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **ΒΉΜΑ ΠΡΟΣ ΒΉΜΑ ΑΝΆΠΤΥΞΗΣ**                                                                   | Απαιτούνται αυτά τα βήματα για να αναπτύξετε τη συσκευή σας StorSimple στο παραγωγής.                                                                                      |
| [Βήμα 1: Δημιουργήστε μια νέα υπηρεσία](#step-1-create-a-new-service)                                                         | Ρύθμιση διαχείρισης cloud και χώρου αποθήκευσης για τη συσκευή σας StorSimple. *Παράλειψη αυτού του βήματος εάν έχετε μια υπάρχουσα υπηρεσία για άλλες συσκευές StorSimple*.                |
| [Βήμα 2: Λήψη το κλειδί εγγραφής υπηρεσίας](#step-2-get-the-service-registration-key)                                               | Χρησιμοποιήστε αυτό το κλειδί για να καταχωρήσετε και να συνδέσετε τη συσκευή StorSimple με την υπηρεσία διαχείρισης.                                                                         |
| [Βήμα 3: Ρύθμιση παραμέτρων και καταχώρηση στη συσκευή μέσω του Windows PowerShell για StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)    | Συνδέστε τη συσκευή με το δίκτυό σας και καταχωρήστε το Azure για να ολοκληρώσετε τη ρύθμιση χρησιμοποιώντας την υπηρεσία διαχείρισης.                                            |
| [Βήμα 4: Ολοκληρώστε εγκατάσταση ελάχιστη συσκευής](#step-4-complete-minimum-device-setupd)</br>[Προαιρετικό: Ενημέρωση συσκευή StorSimple](#scan-for-and-apply-updates)      | Χρησιμοποιήστε την υπηρεσία διαχείρισης για να ολοκληρώσετε τη ρύθμιση συσκευής και ενεργοποιήστε το για την παροχή χώρου αποθήκευσης.                                                                      |
| [Βήμα 5: Δημιουργία κοντέινερ έντασης ήχου](#step-5-create-a-volume-container)                                                      | Δημιουργία κοντέινερ για παροχή όγκους. Ένα κοντέινερ ένταση έχει το λογαριασμό χώρου αποθήκευσης, το εύρος ζώνης και ρυθμίσεις κρυπτογράφησης για όλα τα όγκους που περιέχονται σε αυτό.    |
| [Βήμα 6: Δημιουργία ενός τόμου](#step-6-create-a-volume)                                                                | Προμήθεια τόμων χώρο αποθήκευσης στη συσκευή StorSimple για τους διακομιστές σας.                                                                                        |
| [Βήμα 7: Ενεργοποίησης και προετοιμασία διαμόρφωση ενός τόμου](#step-7-mount-initialize-and-format-a-volume)</br>[Προαιρετικά: Ρύθμιση παραμέτρων MPIO](storsimple-configure-mpio-windows-server.md)            | Συνδέστε τους διακομιστές σας για την αποθήκευση iSCSI που παρέχεται από τη συσκευή. Προαιρετικά, ρύθμιση παραμέτρων MPIO για να βεβαιωθείτε ότι διακομιστές σας μπορεί να αποδεχτεί τις αποτυχία σύνδεσης, δικτύου και περιβάλλον εργασίας.                                                                                                                                                              |
| [Βήμα 8: Λήψη αντιγράφου ασφαλείας](#step-8-take-a-backup)                                                                  | Ρύθμιση πολιτική ασφαλείας σας για την προστασία των δεδομένων σας                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **ΆΛΛΕΣ ΔΙΑΔΙΚΑΣΊΕΣ**                                                                   | Ίσως χρειαστεί να αναφέρονται σε αυτές τις διαδικασίες που μπορείτε να αναπτύξετε τη λύση σας.                                                                                        |
| [Ρύθμιση παραμέτρων ενός νέου λογαριασμού χώρου αποθήκευσης για την υπηρεσία](#configure-a-new-storage-account-for-the-service)                                      |                                                                                                                                                               |
| [Χρησιμοποιήστε PuTTY για να συνδεθείτε στην κονσόλα σειριακή συσκευή](#use-putty-to-connect-to-the-device-serial-console)                                    |                                                                                                                                                               |
| [Λήψη του IQN ενός κεντρικού υπολογιστή Windows Server](#get-the-iqn-of-a-windows-server-host)                                                   |                                                                                                                                                               |
| [Δημιουργήστε μια μη αυτόματη δημιουργία αντιγράφων ασφαλείας](#create-a-manual-backup)                                                                 | 


## <a name="deployment-configuration-checklist"></a>Λίστα ελέγχου ρύθμισης παραμέτρων ανάπτυξης

Πριν να αναπτύξετε τη συσκευή σας, θα πρέπει να συλλέγει πληροφορίες για τη ρύθμιση παραμέτρων του λογισμικού στη συσκευή σας StorSimple. Προετοιμασία ορισμένες από αυτές τις πληροφορίες εκ των προτέρων θα σας βοηθήσει να εξομαλύνει τη διαδικασία ανάπτυξης στη συσκευή StorSimple στο περιβάλλον σας. Κάντε λήψη και χρησιμοποιήστε αυτήν τη λίστα ελέγχου για να λάβετε υπόψη προς τα κάτω τις λεπτομέρειες της ρύθμισης παραμέτρων όπως αναπτύσσετε τη συσκευή σας.

- [Λήψη StorSimple τη λίστα ελέγχου ρύθμισης παραμέτρων ανάπτυξης](http://www.microsoft.com/download/details.aspx?id=49159)


## <a name="deployment-prerequisites"></a>Προαπαιτούμενα ανάπτυξης

Οι παρακάτω ενότητες εξηγούν τις προϋποθέσεις ρύθμισης παραμέτρων για την υπηρεσία διαχείρισης StorSimple και τη συσκευή σας StorSimple.

### <a name="for-the-storsimple-manager-service"></a>Για την υπηρεσία διαχείρισης StorSimple

Πριν ξεκινήσετε, βεβαιωθείτε ότι:

- Έχετε λογαριασμό Microsoft με διαπιστευτήρια πρόσβασης.

- Έχετε το λογαριασμό σας Microsoft Azure χώρου αποθήκευσης με διαπιστευτήρια πρόσβασης.

- Τη συνδρομή σας στο Microsoft Azure είναι ενεργοποιημένη για την υπηρεσία διαχείρισης StorSimple. Θα πρέπει να είναι αγοράσατε τη συνδρομή σας μέσω της [Σύμβαση Enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/).

- Έχετε πρόσβαση σε λογισμικό προσομοίωσης τερματικού όπως PuTTY.

### <a name="for-the-device-in-the-datacenter"></a>Για τη συσκευή στο κέντρο δεδομένων

Πριν από τη ρύθμιση των παραμέτρων της συσκευής, βεβαιωθείτε ότι τη συσκευή σας είναι πλήρως έγινε η αποσυσκευασία, τοποθετείται σε ένα rack και πλήρως συνδεδεμένο power, δικτύου και σειριακή πρόσβαση όπως περιγράφεται στο θέμα:

-  [Αποσυσκευασία, rack ενεργοποίησης και καλώδιο συσκευή 8100](storsimple-8100-hardware-installation.md)
-  [Αποσυσκευασία, rack ενεργοποίησης και καλώδιο συσκευή 8600](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>Για το δίκτυο από το κέντρο δεδομένων

Πριν ξεκινήσετε, βεβαιωθείτε ότι:

- Για να επιτρέψετε για iSCSI και στο cloud κίνηση, όπως περιγράφεται στο [δίκτυο απαιτήσεις για τη συσκευή σας StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)ανοίγονται τις θύρες του τείχους προστασίας του κέντρου δεδομένων.


## <a name="step-by-step-deployment"></a>Βήμα προς βήμα ανάπτυξης

Χρησιμοποιήστε τις παρακάτω οδηγίες βήμα προς βήμα για την ανάπτυξη συσκευή StorSimple στο κέντρο δεδομένων.

## <a name="step-1-create-a-new-service"></a>Βήμα 1: Δημιουργήστε μια νέα υπηρεσία

Μια υπηρεσία StorSimple Manager για να διαχειριστείτε πολλές συσκευές StorSimple. Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε μια νέα παρουσία της υπηρεσίας διαχείρισης StorSimple.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [AZURE.IMPORTANT] Εάν που δεν έχει ενεργοποιήσει την αυτόματη δημιουργία ενός λογαριασμού χώρου αποθήκευσης με την υπηρεσία, θα πρέπει να δημιουργήσετε τουλάχιστον ένα λογαριασμό χώρου αποθήκευσης αφού δημιουργήσετε με επιτυχία μια υπηρεσία. Αυτόν το λογαριασμό χώρου αποθήκευσης θα χρησιμοποιηθεί κατά τη δημιουργία ενός κοντέινερ έντασης ήχου. 
>
> * Εάν δεν έχετε δημιουργήσει αυτόματα ένα λογαριασμό του χώρου αποθήκευσης, μεταβείτε στις επιλογές [Ρύθμιση παραμέτρων ενός νέου λογαριασμού χώρου αποθήκευσης για την υπηρεσία](#configure-a-new-storage-account-for-the-service) για λεπτομερείς οδηγίες. 
> * Εάν έχετε ενεργοποιήσει την αυτόματη δημιουργία ενός λογαριασμού χώρου αποθήκευσης, μεταβείτε στο [βήμα 2: γρήγορα την υπηρεσία εγγραφής κλειδί](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Βήμα 2: Λήψη το κλειδί εγγραφής υπηρεσίας

Μετά την υπηρεσία διαχείρισης StorSimple εγκατάσταση και λειτουργία, θα πρέπει να λάβετε το κλειδί εγγραφής υπηρεσίας. Αυτό το κλειδί χρησιμοποιείται για να καταχωρήσετε και να συνδέσετε τη συσκευή StorSimple με την υπηρεσία.

Εκτελέστε τα ακόλουθα βήματα στην πύλη διαχείρισης.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Βήμα 3: Ρύθμιση παραμέτρων και καταχώρηση στη συσκευή μέσω του Windows PowerShell για StorSimple

Χρήση του Windows PowerShell για StorSimple για την ολοκλήρωση της αρχικής εγκατάστασης της συσκευής σας StorSimple όπως περιγράφεται σε την ακόλουθη διαδικασία. Θα πρέπει να χρησιμοποιήσετε το λογισμικό προσομοίωσης τερματικού για να ολοκληρώσετε αυτό το βήμα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση PuTTY για να συνδεθείτε στην κονσόλα σειριακή συσκευή](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Βήμα 4: Ολοκληρώστε εγκατάσταση ελάχιστη συσκευής

Για τη ρύθμιση παραμέτρων ελάχιστη συσκευή της συσκευής σας StorSimple, που είναι απαραίτητα για να: 

- Ρυθμίστε το δευτερεύοντα διακομιστή DNS.
- Ενεργοποίηση iSCSI στο περιβάλλον εργασίας τουλάχιστον ένα δίκτυο.
- Εκχώρηση σταθερές διευθύνσεις IP και τα δύο στους ελεγκτές.

Εκτελέστε τα ακόλουθα βήματα στην πύλη διαχείρισης για να ολοκληρώσετε τη ρύθμιση ελάχιστη συσκευή.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Βήμα 5: Δημιουργία κοντέινερ έντασης ήχου

Ένα κοντέινερ ένταση έχει το λογαριασμό χώρου αποθήκευσης, το εύρος ζώνης και ρυθμίσεις κρυπτογράφησης για όλα τα όγκους που περιέχονται σε αυτό. Θα πρέπει να δημιουργήσετε ένα κοντέινερ ένταση πριν μπορέσετε να ξεκινήσετε την προμήθεια όγκους στη συσκευή σας StorSimple. 

Εκτελέστε τα ακόλουθα βήματα στην πύλη διαχείρισης για να δημιουργήσετε ένα κοντέινερ έντασης ήχου.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Βήμα 6: Δημιουργία ενός τόμου

Αφού δημιουργήσετε ένα κοντέινερ ένταση, που μπορούν να προμηθεύσουν ενός τόμου χώρο αποθήκευσης στη συσκευή StorSimple για τους διακομιστές. Εκτελέστε τα ακόλουθα βήματα στην πύλη διαχείρισης για τη δημιουργία ενός τόμου.

> [AZURE.IMPORTANT] Διαχείριση StorSimple μπορεί να δημιουργήσει και τα δύο λεπτό και όγκους πλήρως προμήθεια του φακέλου. Ωστόσο, δεν μπορείτε να δημιουργήσετε όγκους μερικώς προμήθεια του φακέλου. 

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Βήμα 7: Ενεργοποίησης και προετοιμασία διαμόρφωση ενός τόμου

Ακολουθήστε τα παρακάτω βήματα εκτελούνται στην υπηρεσία παροχής φιλοξενίας Windows Server. 


> [AZURE.IMPORTANT]

> - Για την υψηλή διαθεσιμότητα της λύσης σας StorSimple, συνιστάται να ρυθμίζετε MPIO στους διακομιστές σας κεντρικού υπολογιστή (προαιρετικά), πριν από τη ρύθμιση των παραμέτρων iSCSI. Ρύθμιση παραμέτρων MPIO σε διακομιστές κεντρικού υπολογιστή εξασφαλίζει ότι οι διακομιστές μπορεί να αποδεχτεί τις μια σύνδεση, το δίκτυο ή σφάλμα στο περιβάλλον.

> - Για MPIO και iSCSI οδηγίες εγκατάστασης και ρύθμισης παραμέτρων κεντρικού υπολογιστή Windows Server, μεταβείτε στη [Ρύθμιση παραμέτρων MPIO για τη συσκευή σας StorSimple](storsimple-configure-mpio-windows-server.md). Επίσης, αυτά θα περιλαμβάνει τα βήματα για να ενεργοποίησης, να προετοιμάσετε και να μορφοποιήσετε StorSimple όγκους.

> - Για MPIO και iSCSI οδηγίες εγκατάστασης και ρύθμισης παραμέτρων σε μια υπηρεσία παροχής φιλοξενίας Linux, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων MPIO για τον κεντρικό υπολογιστή StorSimple Linux](storsimple-configure-mpio-on-linux.md)

Εάν αποφασίσετε να μην γίνεται ρύθμιση MPIO, εκτελέστε τα ακόλουθα βήματα για να ενεργοποίησης, προετοιμασία και να μορφοποιήσετε το όγκους StorSimple σε ένα κεντρικό υπολογιστή Windows Server.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Βήμα 8: Λήψη αντιγράφου ασφαλείας

Δημιουργία αντιγράφων ασφαλείας παρέχουν προστασία σε δεδομένη χρονική στιγμή του όγκους και τη βελτίωση δυνατότητα ανάκτησης κατά την ελαχιστοποίηση επαναφορά ώρες. Μπορείτε να λάβετε δύο τύπους δημιουργίας αντιγράφων ασφαλείας στη συσκευή σας StorSimple: τοπικό στιγμιότυπα και στιγμιότυπα cloud. Κάθε έναν από τους τύπους αντιγράφων ασφαλείας μπορεί να είναι **προγραμματισμένες** ή **μη αυτόματα**. 

Εκτελέστε τα ακόλουθα βήματα στην πύλη διαχείρισης για να δημιουργήσετε μια προγραμματισμένη δημιουργία αντιγράφων ασφαλείας.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Μπορείτε να κρατήσετε μιας μη αυτόματης δημιουργίας αντιγράφων ασφαλείας οποιαδήποτε στιγμή. Για διαδικασίες, ανατρέξτε στο θέμα [Δημιουργία μια μη αυτόματη δημιουργία αντιγράφων ασφαλείας](#create-a-manual-backup). 

## <a name="configure-a-new-storage-account-for-the-service"></a>Ρύθμιση παραμέτρων ενός νέου λογαριασμού χώρου αποθήκευσης για την υπηρεσία

Αυτό το βήμα είναι προαιρετικό που χρειάζεστε για να εκτελέσετε, σε περίπτωση που δεν έχει ενεργοποιήσει την αυτόματη δημιουργία ενός λογαριασμού χώρου αποθήκευσης με την υπηρεσία. Ένα λογαριασμό Microsoft Azure χώρου αποθήκευσης είναι απαραίτητη για να δημιουργήσετε ένα κοντέινερ ένταση StorSimple.

Εάν χρειάζεστε για να δημιουργήσετε ένα λογαριασμό Azure χώρου αποθήκευσης σε διαφορετική περιοχή, ανατρέξτε στο θέμα [Σχετικά με τους λογαριασμούς Azure χώρου αποθήκευσης](../storage/storage-create-storage-account.md) για οδηγίες βήμα προς βήμα.

Εκτελέστε τα ακόλουθα βήματα στην πύλη διαχείρισης, στη σελίδα **Διαχείριση StorSimple υπηρεσίας** .

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Χρησιμοποιήστε PuTTY για να συνδεθείτε στην κονσόλα σειριακή συσκευή

Για να συνδεθείτε με το Windows PowerShell για StorSimple, πρέπει να χρησιμοποιήσετε το λογισμικό προσομοίωσης τερματικού όπως PuTTY. Μπορείτε να χρησιμοποιήσετε PuTTY κατά την πρόσβασή σας στη συσκευή απευθείας από την κονσόλα σειρά ή με το άνοιγμα μιας περιόδου λειτουργίας telnet από έναν απομακρυσμένο υπολογιστή.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]


## <a name="scan-for-and-apply-updates"></a>Ανίχνευση και εφαρμογή ενημερώσεων

Ενημέρωση τη συσκευή σας μπορεί να διαρκέσει μερικές ώρες. Ακολουθήστε τα παρακάτω βήματα για να αναζητήσετε και να εφαρμόσετε ενημερωμένες εκδόσεις στη συσκευή σας.
<!--can take 1-4 hours--> 

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Για να ενημερώσετε τη συσκευή σας

1.  Στη σελίδα **Γρήγορης εκκίνησης** συσκευή, κάντε κλικ στην επιλογή **συσκευές**. Η φυσική συσκευή, κάντε κλικ **Συντήρηση** και, στη συνέχεια, κάντε κλικ στην επιλογή **Σάρωση ενημερώσεις**.  

2.  Δημιουργείται μια εργασία για τη σάρωση για διαθέσιμες ενημερώσεις. Εάν υπάρχουν διαθέσιμες ενημερώσεις, τις **Ενημερώσεις σάρωση** αλλάζει σε **Εγκατάσταση ενημερώσεων**. Κάντε κλικ στην επιλογή **εγκατάσταση ενημερώσεων**. 

3.  Θα δημιουργηθεί μια εργασία ενημέρωσης. Παρακολούθηση της κατάστασης της ενημέρωσης σας, μεταβαίνοντας σε **έργα**.

    > [AZURE.NOTE] Όταν ξεκινήσει η εργασία ενημέρωσης, εμφανίζεται αμέσως την κατάσταση ως 50 τοις εκατό. Το πεδίο status αλλάζει σε 100 τοις εκατό μόνο μετά την ολοκλήρωση της εργασίας ενημερωμένη έκδοση. Δεν υπάρχει καμία κατάσταση σε πραγματικό χρόνο για τη διαδικασία της ενημέρωσης.

4.  Μετά τη συσκευή είναι ενημερωθεί με επιτυχία, ενεργοποίηση δεδομένων 2 και 3 δεδομένων διασυνδέσεις δικτύου Εάν αυτές έχουν απενεργοποιηθεί.

<!-- In step 2, you may be requested to disable Data 2 and Data 3 prior to installing the updates. You must disable these network interfaces or the updates may fail.-->

## <a name="get-the-iqn-of-a-windows-server-host"></a>Λήψη του IQN ενός κεντρικού υπολογιστή Windows Server

Ακολουθήστε τα παρακάτω βήματα για να λάβετε το iSCSI προσδιορισμένο όνομα (IQN) της κεντρικός υπολογιστής με Windows που εκτελεί Windows Server® 2012.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Δημιουργήστε μια μη αυτόματη δημιουργία αντιγράφων ασφαλείας

Εκτελέστε τα ακόλουθα βήματα στην πύλη διαχείρισης για να δημιουργήσετε ένα αντίγραφο ασφαλείας σε ζήτηση μη αυτόματη για μια μεμονωμένη ένταση στη συσκευή σας StorSimple.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]


## <a name="next-steps"></a>Επόμενα βήματα

- Ρύθμιση παραμέτρων [εικονικού συσκευή](storsimple-virtual-device-u2.md).

- Χρησιμοποιήστε την [υπηρεσία διαχείρισης StorSimple](storsimple-manager-service-administration.md) για να διαχειριστείτε τη συσκευή σας StorSimple.
 