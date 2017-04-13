<properties
   pageTitle="Ενημερώστε τη συσκευή σας StorSimple | Microsoft Azure"
   description="Εξηγεί πώς μπορείτε να χρησιμοποιήσετε τη δυνατότητα ενημέρωσης StorSimple για την εγκατάσταση ενημερώσεων λειτουργία κανονική και συντήρηση και επιδιορθώσεις."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="06/28/2016"
   ms.author="v-sharos" />

# <a name="update-your-storsimple-8000-series-device"></a>Ενημερώστε τη συσκευή σας σειρά 8000 StorSimple

## <a name="overview"></a>Επισκόπηση

Οι δυνατότητες StorSimple ενημερώσεις σάς επιτρέπουν να εύκολα διατηρείτε ενημερωμένη τη συσκευή σας StorSimple. Ανάλογα με τον τύπο ενημέρωσης, μπορείτε να εφαρμόσετε ενημερωμένες εκδόσεις στη συσκευή μέσω του Azure κλασική πύλη ή μέσω του περιβάλλοντος εργασίας του Windows PowerShell. Αυτό το πρόγραμμα εκμάθησης περιγράφει τους τύπους ενημέρωσης και πώς να εγκαταστήσετε το καθένα από αυτά.

Μπορείτε να εφαρμόσετε δύο τύπους ενημερώσεις συσκευών: 

- Κανονική (ή σε κανονική κατάσταση λειτουργίας) ενημερώσεις
- Ενημερώσεις λειτουργία συντήρησης

Μπορείτε να εγκαταστήσετε το κανονικό τις ενημερώσεις μέσω του Azure κλασική πύλη ή το Windows PowerShell; Ωστόσο, πρέπει να χρησιμοποιείτε Windows PowerShell για να εγκαταστήσετε ενημερώσεις λειτουργία συντήρησης. 

Κάθε τύπος ενημέρωσης είναι περιγράφεται ξεχωριστά, παρακάτω.

### <a name="regular-updates"></a>Τακτικές ενημερώσεις

Οι ενημερώσεις κανονική είναι χωρίς προβλήματα στη λειτουργία ενημερώσεις που μπορεί να εγκατασταθεί όταν η συσκευή είναι σε κανονική κατάσταση λειτουργίας. Αυτές οι ενημερωμένες εκδόσεις εφαρμόζονται μέσω της τοποθεσίας Web του Microsoft Update κάθε ελεγκτή συσκευής. 

> [AZURE.IMPORTANT] Ανακατεύθυνσης ελεγκτή μπορεί να προκύψουν κατά τη διαδικασία ενημέρωσης. Ωστόσο, αυτό δεν θα επηρεάσει διαθεσιμότητα συστήματος ή μια λειτουργία.

- Για λεπτομέρειες σχετικά με τον τρόπο εγκατάστασης κανονική τις ενημερώσεις μέσω Azure κλασική πύλη, ανατρέξτε στο θέμα [εγκατάσταση κανονική τις ενημερώσεις μέσω του Azure κλασική portal(#install-regular-updates-via-the-azure-classic-portal).

- Μπορείτε επίσης να εγκαταστήσετε κανονική τις ενημερώσεις μέσω του Windows PowerShell για StorSimple. Για λεπτομέρειες, ανατρέξτε στο θέμα [εγκατάσταση κανονική τις ενημερώσεις μέσω του Windows PowerShell για StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Ενημερώσεις λειτουργία συντήρησης

Οι ενημερώσεις λειτουργία συντήρησης είναι ενοχλητικούς ενημερώσεις όπως αναβαθμίσεις υλικολογισμικού δίσκου. Αυτές οι ενημερώσεις απαιτούν τη συσκευή για να τεθούν σε λειτουργία συντήρησης. Για λεπτομέρειες, ανατρέξτε στο θέμα [βήμα 2: Εισαγάγετε συντήρησης λειτουργία](#step2). Δεν μπορείτε να χρησιμοποιήσετε το Azure κλασική πύλη για την εγκατάσταση ενημερώσεων λειτουργία συντήρησης. Αντί για αυτό, πρέπει να χρησιμοποιήσετε Windows PowerShell για StorSimple. 

Για λεπτομέρειες σχετικά με τον τρόπο εγκατάστασης ενημερώσεων λειτουργία συντήρησης, ανατρέξτε στο θέμα [Εγκατάσταση συντήρησης λειτουργία τις ενημερώσεις μέσω του Windows PowerShell για StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [AZURE.IMPORTANT] Συντήρηση λειτουργία ενημερώσεις θα πρέπει να εφαρμοστεί ξεχωριστά σε κάθε ελεγκτή. 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Εγκατάσταση ενημερώσεων κανονική μέσω κλασική πύλη του Azure

Μπορείτε να χρησιμοποιήσετε το Azure κλασική πύλη για να εφαρμόσετε ενημερωμένες εκδόσεις στη συσκευή σας StorSimple.

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Εγκατάσταση κανονική τις ενημερώσεις μέσω του Windows PowerShell για StorSimple

Εναλλακτικά, μπορείτε να χρησιμοποιήσετε για StorSimple του Windows PowerShell για να εφαρμόσετε ενημερώσεις κανονική (κανονική κατάσταση λειτουργίας).

> [AZURE.IMPORTANT] Παρόλο που μπορείτε να εγκαταστήσετε τακτικές ενημερώσεις χρησιμοποιώντας το Windows PowerShell για StorSimple, συνιστάται να εγκαταστήσετε τακτικές ενημερώσεις μέσω της Azure κλασική πύλης. Ξεκινώντας από το 1 ενημέρωση, προ-ελέγχων θα εκτελεστεί πριν από την εγκατάσταση των ενημερώσεων από την πύλη. Αυτοί οι προ-έλεγχοι θα εκτελούνται πριν αποτυχίες και βεβαιωθείτε ότι καλύτερη εμπειρία. 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Εγκατάσταση ενημερώσεων λειτουργία συντήρησης μέσω του Windows PowerShell για StorSimple

Μπορείτε να χρησιμοποιήσετε για StorSimple του Windows PowerShell για να εφαρμόσετε ενημερώσεις λειτουργία συντήρησης στη συσκευή σας StorSimple. Όλων των αιτήσεων εισόδου διακόπτονται σε αυτήν τη λειτουργία. Υπηρεσίες, όπως μνήμη RAM μόνιμης (NVRAM) ή την υπηρεσία συμπλέγματος επίσης τερματίζονται. Δύο ελεγκτές είναι επανεκκίνηση κατά την είσοδο ή έξοδος από αυτήν την κατάσταση λειτουργίας. Κατά την έξοδο από αυτήν την κατάσταση λειτουργίας, όλες τις υπηρεσίες που θα συνεχίσετε και πρέπει να είναι σε καλή κατάσταση. (Αυτό μπορεί να χρειαστούν μερικά λεπτά.)

Εάν χρειάζεστε για να εφαρμόσετε ενημερώσεις λειτουργία συντήρησης, θα λάβετε μια ειδοποίηση μέσω της πύλης Azure κλασική ότι έχετε ενημερώσεις που πρέπει να έχει εγκατασταθεί. Αυτή η ειδοποίηση θα περιλαμβάνει οδηγίες για τη χρήση του Windows PowerShell για StorSimple για να εγκαταστήσετε τις ενημερώσεις. Αφού ενημερώσετε τη συσκευή σας, χρησιμοποιήστε την ίδια διαδικασία για να αλλάξετε τη συσκευή σε κανονική κατάσταση λειτουργίας. Για οδηγίες βήμα προς βήμα, ανατρέξτε στο θέμα [βήμα 4: λειτουργία έξοδος συντήρησης](#step4).

> [AZURE.IMPORTANT] 
> 
> - Πριν από την εισαγωγή λειτουργία συντήρησης, βεβαιωθείτε ότι και οι δύο ελεγκτές συσκευή είναι σε καλή κατάσταση, επιλέγοντας την **Κατάσταση υλικού** στη σελίδα **Συντήρηση** στην πύλη του Azure κλασική. Εάν δεν είναι σε καλή κατάσταση του ελεγκτή, επικοινωνήστε με υποστήριξης της Microsoft για τα επόμενα βήματα. Για περισσότερες πληροφορίες, μεταβείτε στην επικοινωνία με την υποστήριξη της Microsoft. 
> - Όταν βρίσκεστε σε λειτουργία συντήρησης, πρέπει να εφαρμόσετε πρώτα την ενημέρωση σε μία ελεγκτή και, στη συνέχεια, σε άλλον ελεγκτή.

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Βήμα 1: Σύνδεση με την κονσόλα σειρά<a name="step1">

Πρώτα, χρησιμοποιήστε μια εφαρμογή όπως το PuTTY για να αποκτήσετε πρόσβαση στην κονσόλα σειρά. Η παρακάτω διαδικασία εξηγεί τον τρόπο χρήσης PuTTY για να συνδεθείτε στην κονσόλα σειρά.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Βήμα 2: Εισαγωγή λειτουργίας συντήρησης<a name="step2">

Μετά τη σύνδεση στην κονσόλα, προσδιορίστε αν υπάρχουν ενημερωμένες εκδόσεις να εγκαταστήσετε και να εισαγάγετε λειτουργία συντήρησης να τις εγκαταστήσετε.

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Βήμα 3: Εγκαταστήστε τις ενημερώσεις σας<a name="step3">

Στη συνέχεια, εγκαταστήστε τις ενημερώσεις σας.

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Βήμα 4: Λειτουργία συντήρησης εξόδου<a name="step4">

Τέλος, Έξοδος από τη λειτουργία συντήρησης.

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Εγκατάσταση επιδιορθώσεις μέσω του Windows PowerShell για StorSimple

Σε αντίθεση με τις ενημερώσεις για το Microsoft Azure StorSimple, επιδιορθώσεις εγκαθίστανται από έναν κοινόχρηστο φάκελο. Όπως με τις ενημερώσεις, υπάρχουν δύο τύποι επιδιορθώσεις: 

- Κανονική επιδιορθώσεις 
- Επιδιορθώσεις λειτουργία συντήρησης  

Τις ακόλουθες διαδικασίες εξηγούν τον τρόπο χρήσης του Windows PowerShell για StorSimple για να εγκαταστήσετε το κανονικών και επιδιορθώσεις λειτουργία συντήρησης.

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Τι συμβαίνει με ενημερώσεις εάν εκτελείτε επαναφορά εργοστασίου της συσκευής;

Εάν μια συσκευή είναι επαναφέρετε στις εργοστασιακές ρυθμίσεις, στη συνέχεια, όλες τις ενημερώσεις θα χαθούν. Αφού η συσκευή factory-επαναφορά είναι καταχωρημένος και να ρυθμίσει τις παραμέτρους, θα πρέπει να εγκαταστήσετε με μη αυτόματο τρόπο ενημερώσεις με το Azure κλασική πύλη ή/και Windows PowerShell για StorSimple. Για περισσότερες πληροφορίες σχετικά με τις εργοστασιακές επαναφορά, ανατρέξτε στο θέμα [Επαναφορά της συσκευής στις εργοστασιακές προεπιλεγμένες ρυθμίσεις](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε περισσότερα σχετικά με τη [χρήση του Windows PowerShell για StorSimple για να διαχειριστείτε τη συσκευή σας StorSimple](storsimple-windows-powershell-administration.md).
- Μάθετε περισσότερα σχετικά με τη [χρήση της υπηρεσίας διαχείρισης StorSimple για να διαχειριστείτε τη συσκευή σας StorSimple](storsimple-manager-service-administration.md).