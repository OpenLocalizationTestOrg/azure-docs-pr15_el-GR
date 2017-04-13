<properties
   pageTitle="Εγκατάσταση ενημέρωσης 3 στη συσκευή σας StorSimple | Microsoft Azure"
   description="Εξηγεί τον τρόπο εγκατάστασης StorSimple 8000 σειρά ενημέρωση 3 στη συσκευή σας StorSimple 8000 σειρά."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2016"
   ms.author="alkohli" />

# <a name="install-update-3-on-your-storsimple-device"></a>Εγκατάσταση ενημέρωσης 3 στη συσκευή σας StorSimple

## <a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης εξηγεί τον τρόπο για να εγκαταστήσετε την ενημερωμένη έκδοση 3 σε μια συσκευή StorSimple χρησιμοποιεί μια παλαιότερη έκδοση του λογισμικού μέσω Azure κλασική πύλη και χρησιμοποιώντας τη μέθοδο επείγουσα επιδιόρθωση. Η μέθοδος επείγουσα επιδιόρθωση χρησιμοποιείται όταν μια πύλη έχει ρυθμιστεί στο περιβάλλον εργασίας δικτύου εκτός από ΔΕΔΟΜΈΝΑ 0 της συσκευής StorSimple και προσπαθείτε να κάνετε ενημέρωση από μια έκδοση πριν από την ενημέρωση 1 του λογισμικού.

Ενημέρωση 3 περιλαμβάνει λογισμικό της συσκευής, το πρόγραμμα οδήγησης LSI και υλικολογισμικό, Storport και ενημερώνει Spaceport. Εάν ενημερώνετε από ενημερωμένη έκδοση 2 ή παλαιότερη έκδοση, επίσης θα απαιτείται για να εφαρμόσετε iSCSI, WMI, και σε ορισμένες περιπτώσεις, δίσκο ενημερωμένες εκδόσεις υλικολογισμικού. Το λογισμικό της συσκευής, WMI, iSCSI, LSI πρόγραμμα οδήγησης, Spaceport και Storport επιδιορθώσεις χωρίς προβλήματα στη λειτουργία ενημερωμένες εκδόσεις και μπορούν να εφαρμοστούν μέσω Azure κλασική πύλη. Οι ενημερωμένες εκδόσεις υλικολογισμικού δίσκου ενοχλητικούς ενημερωμένες εκδόσεις και μπορούν να εφαρμοστούν μόνο μέσω του περιβάλλοντος εργασίας του Windows PowerShell της συσκευής. 

> [AZURE.IMPORTANT]

> - Ένα σύνολο μη αυτόματες και αυτόματες προ-ελέγχων γίνεται πριν από την εγκατάσταση για να προσδιορίσετε την εύρυθμη λειτουργία της συσκευής όσον αφορά υλικό κατάσταση και σύνδεσης δικτύου. Αυτοί οι προ-έλεγχοι εκτελούνται μόνο εάν εφαρμόσετε τις ενημερώσεις από την πύλη του Azure κλασική.
> - Συνιστάται να εγκαταστήσετε τις ενημερώσεις λογισμικού και προγραμμάτων οδήγησης μέσω του Azure κλασική πύλη. Μπορείτε μόνο θα πρέπει να μεταβείτε στο περιβάλλον εργασίας του Windows PowerShell της συσκευής (για να εγκαταστήσετε τις ενημερώσεις) εάν αποτύχει ο έλεγχος πριν από την ενημέρωση πύλης στην πύλη. Ανάλογα με την έκδοση που θέλετε να ενημερώσετε από, τις ενημερώσεις, ενδέχεται να χρειαστούν 1,5-2,5 ώρες για την εγκατάσταση. Πρέπει να εγκαταστήσετε τις ενημερώσεις λειτουργία συντήρησης μέσω του περιβάλλοντος εργασίας του Windows PowerShell της συσκευής. Καθώς οι ενημερώσεις λειτουργία συντήρησης είναι ενοχλητικούς ενημερώσεις, αυτά θα έχει ως αποτέλεσμα κάτω χρόνο για τη συσκευή σας.
> - Εάν εκτελείται η Διαχείριση προαιρετικών στιγμιότυπο StorSimple, βεβαιωθείτε ότι έχετε αναβαθμίσει τη δική σας έκδοση Διαχείριση στιγμιοτύπων σε ενημερωμένη έκδοση 2 πριν από την ενημέρωση της συσκευής.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-the-azure-classic-portal"></a>Εγκατάσταση του 3 ενημέρωση μέσω κλασική πύλη του Azure

Ακολουθήστε τα παρακάτω βήματα για να ενημερώσετε τη συσκευή σας για να [ενημερώσετε 3](storsimple-update3-release-notes.md).


> [AZURE.NOTE]
Εάν εφαρμόζετε ενημερωμένη έκδοση 2 ή νεότερη έκδοση (συμπεριλαμβανομένων των ενημερωμένη έκδοση 2.1), θα μπορείτε να αποσπάσετε πρόσθετες πληροφορίες διαγνωστικών από τη συσκευή Microsoft. Ως αποτέλεσμα, όταν ομάδα μας λειτουργίες προσδιορίζει συσκευές που αντιμετωπίζετε προβλήματα, Ζητούμε καλύτερα δυνατότητα να συλλέξετε πληροφορίες από τη συσκευή και διάγνωση θεμάτων. Με την αποδοχή ενημερωμένη έκδοση 2 ή νεότερη έκδοση, επιτρέπετε μας για την παροχή αυτό πριν από την ενεργοποίηση υποστήριξης.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Βεβαιωθείτε ότι η συσκευή σας εκτελείται **StorSimple 8000 σειράς ενημέρωσης 3 (6.3.9600.17759)**. Η **τελευταία ενημέρωση ημερομηνία** πρέπει να τροποποιηθούν. 

    Εάν θέλετε να ενημερώσετε από μια έκδοση πριν από την έκδοση 2, θα δείτε επίσης ότι οι ενημερώσεις λειτουργία συντήρησης είναι διαθέσιμες (αυτό το μήνυμα ενδέχεται να εξακολουθούν να εμφανίζονται για έως και 24 ώρες μετά την εγκατάσταση των ενημερώσεων).

    Οι ενημερώσεις λειτουργία συντήρησης είναι ενοχλητικούς ενημερώσεις που έχει ως αποτέλεσμα συσκευή χρόνου εκτός λειτουργίας και μπορούν να εφαρμοστούν μόνο μέσω του περιβάλλοντος εργασίας του Windows PowerShell της συσκευής σας. Σε ορισμένες περιπτώσεις όταν εκτελείτε την ενημερωμένη έκδοση 1.2, υλικολογισμικού σας δίσκου μπορεί να είναι ήδη ενημερωμένο, οπότε δεν χρειάζεται να εγκαταστήσετε τις ενημερώσεις λειτουργία συντήρησης.

    Εάν είστε ενημέρωσης ενημερωμένη έκδοση 2 ή νεότερη έκδοση, τη συσκευή σας πρέπει τώρα να ενημερωμένο. Μπορείτε να παραλείψετε τα υπόλοιπα βήματα.

13. Πραγματοποιήστε λήψη των ενημερώσεων λειτουργία συντήρησης, χρησιμοποιώντας τα βήματα που αναφέρονται σε [για να κάνετε λήψη επιδιορθώσεις](#to-download-hotfixes) για να αναζητήσετε και να κάνετε λήψη KB3121899, των ενημερώσεων υλικολογισμικού δίσκου εγκαταστάσεις (οι άλλες ενημερωμένες εκδόσεις θα πρέπει να έχει ήδη εγκατασταθεί με now).

13. Ακολουθήστε τα βήματα που αναφέρονται στο [εγκαταστήσετε και να επαληθεύσετε επιδιορθώσεις λειτουργία συντήρησης](#to-install-and-verify-maintenance-mode-hotfixes) για να εγκαταστήσετε τις ενημερώσεις λειτουργία συντήρησης. 

  

## <a name="install-update-3-as-a-hotfix"></a>Εγκατάσταση ενημέρωσης 3 ως επείγουσα επιδιόρθωση

Χρησιμοποιήστε αυτήν τη διαδικασία εάν αποτύχει η πύλη ελέγχου όταν προσπαθείτε να εγκαταστήσετε τις ενημερώσεις μέσω της Azure κλασική πύλης. Ο έλεγχος αποτυγχάνει ενώ έχετε μια πύλη που έχουν εκχωρηθεί σε ένα περιβάλλον εργασίας 0 δικτύου χωρίς ΔΕΔΟΜΈΝΑ και τη συσκευή σας εκτελεί μια έκδοση λογισμικού πριν από την ενημέρωση 1.

Οι εκδόσεις λογισμικού που μπορεί να αναβαθμιστεί χρησιμοποιώντας τη μέθοδο επείγουσα επιδιόρθωση είναι:

- Ενημέρωση 0,1, 0,2, 0,3
- Ενημέρωση 1, 1.1, 1.2
- Ενημέρωση 2, 2.1, 2.2 

> [AZURE.IMPORTANT]
>
> - Εάν σας συσκευή εκτελεί έκδοση κυκλοφορίας (GA), επικοινωνήστε με την [Υποστήριξη της Microsoft](storsimple-contact-microsoft-support.md) για να σας βοηθήσει με την ενημέρωση.

Η μέθοδος επείγουσα επιδιόρθωση περιλαμβάνει τα ακόλουθα τρία βήματα:

1.  Κάντε λήψη του επιδιορθώσεις από τον κατάλογο του Microsoft Update.

2.  Εγκατάσταση και βεβαιωθείτε ότι το επιδιορθώσεις κανονική λειτουργία.

3.  Εγκατάσταση και βεβαιωθείτε ότι η επείγουσα επιδιόρθωση λειτουργία συντήρησης (μόνο κατά την ενημέρωση από λογισμικό 2 πριν από την ενημέρωση).


#### <a name="download-updates-for-your-device"></a>Λήψη ενημερώσεων για τη συσκευή σας

**Εάν τη συσκευή σας λειτουργεί 2.1 ενημέρωση ή 2.2**, που πρέπει να κάνετε λήψη και να τις εγκαταστήσετε παρακάτω με την προκαθορισμένη σειρά:

| Σειρά  | KB        | Περιγραφή                    | Τύπος ενημέρωσης  | Εγκατάσταση του χρόνου |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3186843 | Ενημερωμένη έκδοση λογισμικού & #42;  |  Κανονική <br></br>Χωρίς προβλήματα     | ~ 45 λεπτά |
| 2.      | KB3186859 | Πρόγραμμα οδήγησης LSI και υλικολογισμικού             |  Κανονική <br></br>Χωρίς προβλήματα      | ~ 20 λεπτά |
| 3.      | KB3121261 | Επιδιόρθωση Storport και Spaceport </br> Windows Server 2012 R2 |  Κανονική <br></br>Χωρίς προβλήματα      | ~ 20 λεπτά |

& #42;  *Σημείωση, λογισμικό update αποτελείται από δύο δυαδικών αρχείων: ενημέρωση λογισμικού συσκευή πριν από αυτό, `all-hcsmdssoftwareupdate` και τον παράγοντα Cis και Mds πριν από αυτό, `all-cismdsagentupdatebundle`. Πρέπει να εγκαταστήσετε την ενημέρωση του λογισμικού συσκευή πριν από τον παράγοντα Cis και Mds. Επίσης, πρέπει να επανεκκινήσετε τον ενεργό ελεγκτή μέσω του `Restart-HcsController` ενημέρωση cmdlet μετά την εφαρμογή τον παράγοντα Cis και Mds (και πριν από την εφαρμογή τα υπόλοιπα ενημερώνει).* 


**Εάν η συσκευή σας είναι εκτελεί ενημερωμένη έκδοση 0.1, 0,2, 0,3, 1.0, 1.1, 1.2, ή 2.0**, πρέπει να κάνετε λήψη και εγκαταστήσετε τις ακόλουθες επείγουσες επιδιορθώσεις εκτός από το λογισμικό, το πρόγραμμα οδήγησης LSI και ενημερώσεις υλικολογισμικού (φαίνεται στον προηγούμενο πίνακα), η προκαθορισμένη σειρά:

| Σειρά  | KB        | Περιγραφή                    | Τύπος ενημέρωσης  | Εγκατάσταση του χρόνου |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3146621 | πακέτο iSCSI | Κανονική <br></br>Χωρίς προβλήματα  | ~ 20 λεπτά |
| 5.      | KB3103616 | Πακέτο WMI |  Κανονική <br></br>Χωρίς προβλήματα      | ~ 12 λεπτά |


<br></br>

**Εάν η συσκευή σας είναι εκτελεί εκδόσεις 0,2, 0,3, 1.0, 1.1 και 1.2**, πρέπει επίσης να εγκατάσταση των ενημερώσεων του υλικολογισμικού δίσκο επάνω σε όλες τις ενημερώσεις που φαίνεται στην προηγούμενη τους πίνακες. Μπορείτε να επαληθεύσετε εάν χρειάζεστε τις ενημερώσεις υλικολογισμικού του δίσκου, εκτελώντας το `Get-HcsFirmwareVersion` cmdlet. Εάν χρησιμοποιείτε αυτές τις εκδόσεις υλικολογισμικού: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, στη συνέχεια, δεν χρειάζεται να εγκαταστήσετε αυτές τις ενημερώσεις.


| Σειρά  | KB        | Περιγραφή                    | Τύπος ενημέρωσης  | Εγκατάσταση του χρόνου |
|--------|-----------|-------------------------|------------- |-------------|
| 6.      | KB3121899 | Υλικολογισμικό δίσκου              |  Συντήρηση <br></br>Ενοχλητικούς      | ~ 30 λεπτά |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Αυτή η διαδικασία πρέπει να εκτελεστούν μόνο μία φορά για να εφαρμόσετε ενημερωμένη έκδοση 3. Μπορείτε να χρησιμοποιήσετε το Azure κλασική πύλη για να εφαρμοστούν οι επόμενες ενημερώσεις.
> - Εάν ενημερώνετε από ενημερωμένη έκδοση 2.2, ο χρόνος συνολικό εγκατάσταση είναι κοντά 1.1 ώρες.
> - Πριν να χρησιμοποιήσετε αυτήν τη διαδικασία για να εφαρμόσετε την ενημερωμένη έκδοση, βεβαιωθείτε ότι τόσο το ελεγκτές συσκευή είναι σε σύνδεση και όλα τα στοιχεία υλικού είναι σε καλή κατάσταση.

Ακολουθήστε τα παρακάτω βήματα για να κάνετε λήψη και να τις εγκαταστήσετε.

[AZURE.INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με την [έκδοση 3 ενημερωμένη έκδοση](storsimple-update3-release-notes.md).