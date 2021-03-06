<properties 
   pageTitle="Ενεργοποίηση ή απενεργοποίηση της συσκευής σας StorSimple | Microsoft Azure"
   description="Εξηγεί πώς μπορείτε να ενεργοποιήσετε μια νέα συσκευή StorSimple, ενεργοποιήστε την επιλογή μιας συσκευής που έχει τερματιστεί ή ρεύματος και απενεργοποιήστε μια ενσωματωμένη συσκευή."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/23/2016"
   ms.author="alkohli" />

# <a name="turn-your-storsimple-device-on-or-off"></a>Ενεργοποίηση ή απενεργοποίηση της συσκευής σας StorSimple 

## <a name="overview"></a>Επισκόπηση

Τερματισμός μιας συσκευής Microsoft Azure StorSimple δεν απαιτείται ως μέρος της λειτουργίας κανονική συστήματος. Ωστόσο, ίσως χρειαστεί να ενεργοποιήσετε μια νέα συσκευή ή σε μια συσκευή που είχαν να τερματιστεί. Γενικά, ένας Τερματισμός απαιτείται στις περιπτώσεις στις οποίες που πρέπει να αντικαταστήσετε αποτυχίας υλικού, φυσικά Μετακίνηση μια μονάδα ή λαμβάνουν μια συσκευή από υπηρεσία. Αυτό το πρόγραμμα εκμάθησης περιγράφει τη διαδικασία που απαιτείται για την ενεργοποίηση και τον τερματισμό συσκευή StorSimple σε διαφορετικά σενάρια.

Ο παρακάτω πίνακας παραθέτει διάφορα σενάρια για ενεργοποίηση και τον τερματισμό συσκευή StorSimple και παρέχει συνδέσεις προς τις κατάλληλες διαδικασίες.

|Σενάριο|Θέματα της αναφοράς|
|:-------|:---------------|
|Ενεργοποιήστε μια νέα συσκευή|[Ενεργοποιήστε μια νέα συσκευή](#turn-on-a-new-device)<ul><li>[Νέα συσκευή με κύρια περίβλημα μόνο](#new-device-with-primary-enclosure-only)</li><li>[Νέα συσκευή με EBOD περίβλημα](#new-device-with-ebod-enclosure)</li></ul>|
|Ενεργοποίηση σε μια συσκευή μετά τον τερματισμό|[Ενεργοποίηση σε μια συσκευή μετά τον τερματισμό](#turn-on-a-device-after-shutdown)<ul><li>[Συσκευή με κύρια περίβλημα μόνο](#device-with-primary-enclosure-only)</li><li>[Συσκευή με EBOD περίβλημα](#device-with-ebod-enclosure)</li></ul>|
|Ενεργοποίηση σε μια συσκευή μετά την απώλεια ενέργειας|[Ενεργοποίηση σε μια συσκευή μετά την απώλεια ενέργειας](#turn-on-a-device-after-a-power-loss)<ul><li>[Συσκευή με κύρια περίβλημα μόνο](#8100)</li><li>[Συσκευή με EBOD περίβλημα](#8600)</li></ul>|
|Ενεργοποιήστε μια συσκευή μετά το πρωτεύον περίβλημα και έχει χαθεί η σύνδεση EBOD|[Ενεργοποιήστε μια συσκευή μετά την κύρια και έχει χαθεί η σύνδεση περίβλημα EBOD](#turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost)|
|Τερματισμός μιας συσκευής που εκτελείται|[Απενεργοποιήστε την επιλογή μιας συσκευής που εκτελείται](#turn-off-a-running-device)<ul><li>[Συσκευή με κύρια περίβλημα μόνο](#8100a)</li><li>[Συσκευή με EBOD περίβλημα](#8600a)</li></ul>|

## <a name="turn-on-a-new-device"></a>Ενεργοποιήστε μια νέα συσκευή

Τα βήματα για την ενεργοποίηση μια συσκευή StorSimple για πρώτη φορά διαφέρουν ανάλογα με το αν η συσκευή είναι μια 8100 ή ένα μοντέλο 8600. Το 8100 έχει ένα μόνο κύριο περίβλημα, ότι το 8600 είναι μια συσκευή διπλής περίβλημα με ένα πρωτεύον περίβλημα και ένα περίβλημα EBOD. Λεπτομερείς οδηγίες για δύο μοντέλα καλύπτονται στις ενότητες που ακολουθούν.

- [Νέα συσκευή με κύρια περίβλημα μόνο](#new-device-with-primary-enclosure-only)

- [Νέα συσκευή με EBOD περίβλημα](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Νέα συσκευή με κύρια περίβλημα μόνο

Το μοντέλο StorSimple 8100 είναι μια συσκευή ίδιο περίβλημα. Τη συσκευή σας περιλαμβάνει πλεονάζοντα Power και ψύξης λειτουργικές μονάδες (PCMs). Τόσο PCMs πρέπει να εγκατασταθεί και να είναι συνδεδεμένη με διάφορες πηγές ενέργειας για να βεβαιωθείτε ότι υψηλής διαθεσιμότητας.

Ακολουθήστε τα παρακάτω βήματα για τη συσκευή σας για το power καλώδιο.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]Ρύθμιση ολοκλήρωσης συσκευή και καλωδίωση οδηγίες, μεταβείτε για να [εγκαταστήσετε τη συσκευή σας StorSimple 8100](storsimple-8100-hardware-installation.md). Βεβαιωθείτε ότι έχετε ακολουθήσει ακριβώς τις οδηγίες.

### <a name="new-device-with-ebod-enclosure"></a>Νέα συσκευή με EBOD περίβλημα

Το μοντέλο StorSimple 8600 έχει ένα πρωτεύον περίβλημα και ένα περίβλημα EBOD. Αυτό απαιτεί τις μονάδες που θα είναι συνδεδεμένο μαζί για συνδεσιμότητας σειριακή συνημμένη SCSI (συσχετισμών Ασφαλείας) και power.

Όταν ορίζετε αυτήν τη συσκευή για πρώτη φορά, ακολουθήστε τα βήματα για συσχετισμούς Ασφαλείας καλωδίωση πρώτα και, στη συνέχεια, ολοκληρώστε τα βήματα για το power καλωδίωση.

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]Ρύθμιση ολοκλήρωσης συσκευή και καλωδίωση οδηγίες, μεταβείτε για να [εγκαταστήσετε τη συσκευή σας StorSimple 8600](storsimple-8600-hardware-installation.md). Βεβαιωθείτε ότι έχετε ακολουθήσει ακριβώς τις οδηγίες.

## <a name="turn-on-a-device-after-shutdown"></a>Ενεργοποίηση σε μια συσκευή μετά τον τερματισμό

Τα βήματα για την ενεργοποίηση μιας συσκευής StorSimple μετά την έχει τερματιστεί διαφέρουν ανάλογα με το αν η συσκευή είναι μια 8100 ή ένα μοντέλο 8600. Το 8100 έχει ένα μόνο κύριο περίβλημα, ότι το 8600 είναι μια συσκευή διπλής περίβλημα με ένα πρωτεύον περίβλημα και ένα περίβλημα EBOD.

- [Συσκευή με κύρια περίβλημα μόνο](#device-with-primary-enclosure-only)

- [Συσκευή με EBOD περίβλημα](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Συσκευή με κύρια περίβλημα μόνο

Μετά από έναν τερματισμό, χρησιμοποιήστε την ακόλουθη διαδικασία για να ενεργοποιήσετε μια συσκευή StorSimple με ένα πρωτεύον περίβλημα και χωρίς περίβλημα EBOD.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Για να ενεργοποιήσετε μια συσκευή με μόνο ένα πρωτεύον περίβλημα

1. Βεβαιωθείτε ότι το power αλλάζει σε δύο Power και ψύξης λειτουργικές μονάδες (PCMs) είναι στη θέση ΑΝΕΝΕΡΓΌ. Εάν οι διακόπτες δεν είναι στη θέση ΑΝΕΝΕΡΓΌ, στη συνέχεια, αναστροφή τους στη θέση ΑΝΕΝΕΡΓΌ και περιμένετε οι ενδείξεις για να μεταβείτε.

2. Ενεργοποιήστε τη συσκευή, αναστροφή τους διακόπτες power σε δύο PCMs στη θέση Ενεργοποίηση. Θα πρέπει να ενεργοποιήσετε τη συσκευή.

3. Ελέγξτε τα εξής για να επαληθεύσετε ότι η συσκευή είναι πλήρως σε:

    1. Το κουμπί OK LED σε δύο λειτουργικές μονάδες PCM είναι πράσινο.

    2. Η κατάσταση LED σε δύο ελεγκτές είναι σταθεροποιηθεί σε πράσινο χρώμα.

    3. Το μπλε Ένδειξη σε έναν από τους ελεγκτές αναβοσβήνει, που δείχνει ότι η ελεγκτή είναι ενεργό.

    Εάν οποιαδήποτε από αυτές τις συνθήκες δεν ισχύει, στη συνέχεια, τη συσκευή σας δεν είναι σε καλή κατάσταση. Επικοινωνήστε [με την υποστήριξη της Microsoft](storsimple-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Συσκευή με EBOD περίβλημα

Μετά από έναν τερματισμό, χρησιμοποιήστε την ακόλουθη διαδικασία για να ενεργοποιήσετε μια συσκευή StorSimple με ένα πρωτεύον περίβλημα και ένα περίβλημα EBOD. Εκτέλεση κάθε βήμα ακριβώς όπως περιγράφεται στην ακολουθία. Αποτυχία για να γίνει αυτό θα μπορούσε να έχει ως αποτέλεσμα απώλεια δεδομένων.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Για να ενεργοποιήσετε μια συσκευή με ένα πρωτεύον και ένα περίβλημα EBOD

1. Βεβαιωθείτε ότι το περίβλημα EBOD είναι συνδεδεμένο με το κύριο περίβλημα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εγκατάσταση συσκευή StorSimple 8600](storsimple-8600-hardware-installation.md).

2. Βεβαιωθείτε ότι το Power και ψύξης λειτουργικές μονάδες (PCMs) και στην EBOD και κύρια εσώκλειστων στοιχείων είναι στη θέση ΑΝΕΝΕΡΓΌ. Εάν οι διακόπτες δεν είναι στη θέση ΑΝΕΝΕΡΓΌ, στη συνέχεια, αναστροφή τους στη θέση ΑΝΕΝΕΡΓΌ και περιμένετε οι ενδείξεις για να μεταβείτε.

3. Ενεργοποιήστε το περίβλημα EBOD πρώτο, αναστροφή τους διακόπτες power σε δύο PCMs στη θέση Ενεργοποίηση. Το LED PCM πρέπει να είναι πράσινο. Ένα πράσινο ελεγκτή EBOD LED σε αυτή τη μονάδα υποδεικνύει ότι το περίβλημα EBOD είναι σε.

4. Ενεργοποιήστε το πρωτεύον περίβλημα, αναστροφή τους διακόπτες power σε δύο PCMs στη θέση Ενεργοποίηση. Ολόκληρο το σύστημα θα πρέπει τώρα να σε.

5. Βεβαιωθείτε ότι το LED συσχετισμών Ασφαλείας είναι πράσινο χρώμα, που διασφαλίζει ότι η σύνδεση μεταξύ θάλαμο EBOD και τα κύρια περίβλημα είναι καλή.

## <a name="turn-on-a-device-after-a-power-loss"></a>Ενεργοποίηση σε μια συσκευή μετά την απώλεια ενέργειας

Μια ρεύματος ή διακοπή να τερματίσετε μια συσκευή StorSimple. Το ρεύματος μπορεί να συμβεί σε μία από τις προμήθειες power ή και τα δύο προμήθειες power. Τα βήματα αποκατάστασης διαφέρουν ανάλογα με το αν η συσκευή είναι ένα μοντέλο 8100 είτε ένα 8600. Το 8100 έχει ένα μόνο κύριο περίβλημα, ότι το 8600 είναι μια συσκευή διπλής περίβλημα με ένα πρωτεύον περίβλημα και ένα περίβλημα EBOD. Αυτή η ενότητα περιγράφει τη διαδικασία ανάκτησης για κάθε σενάριο.

- [Συσκευή με κύρια περίβλημα μόνο](#8100)

- [Συσκευή με EBOD περίβλημα](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Συσκευή με κύρια περίβλημα μόνο<a name="8100">

Το σύστημα να συνεχίσετε την κανονική λειτουργία, εάν υπάρχει απώλεια ενέργειας σε μία από τις προμήθειες power. Ωστόσο, για να εξασφαλίσετε υψηλή διαθεσιμότητα της συσκευής, επαναφέρετε power για την παροχή ενέργειας όσο το δυνατόν πιο σύντομα.

Εάν υπάρχει μια ρεύματος ή διακοπής τροφοδοσίας δύο προαιρετική power, το σύστημα θα τερματιστεί σε μη τακτοποιημένο και ελεγχόμενη τρόπο. Όταν γίνεται επαναφορά της ισχύος, το σύστημα θα μετατραπεί αυτόματα.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Συσκευή με EBOD περίβλημα<a name="8600">

#### <a name="power-loss-on-one-power-supply"></a>Παρέχετε απώλεια ενέργειας σε ένα power

Το σύστημα να συνεχίσετε την κανονική λειτουργία, εάν υπάρχει απώλεια ενέργειας σε μία από τις προμήθειες power στην κύρια θάλαμο ή το περίβλημα EBOD. Ωστόσο, για να εξασφαλίσετε υψηλή διαθεσιμότητα της συσκευής, επαναφέρετε power για την παροχή ενέργειας όσο το δυνατόν πιο σύντομα.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Απώλεια ενέργειας και τα δύο προαιρετική power στην κύρια και EBOD εσώκλειστων στοιχείων

Εάν υπάρχει μια ρεύματος ή διακοπής τροφοδοσίας δύο προαιρετική power, το περίβλημα EBOD θα τερματιστεί αμέσως και θα τερματιστεί το πρωτεύον περίβλημα σε μη τακτοποιημένο και ελεγχόμενη τρόπο. Όταν γίνεται επαναφορά power, η συσκευή θα ξεκινήσει αυτόματα.

Εάν το ενεργοποιούνται με μη αυτόματο τρόπο, στη συνέχεια, ακολουθήστε τα παρακάτω βήματα για να επαναφέρετε power στο σύστημα.

1. Ενεργοποιήστε το περίβλημα EBOD.

2. Αφού το περίβλημα EBOD είναι ενεργοποιημένη, ενεργοποιήστε το πρωτεύον περίβλημα.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Απώλεια ενέργειας και τα δύο προαιρετική power σε EBOD περίβλημα

Όταν ρυθμίζετε τα καλώδια, πρέπει να βεβαιωθείτε ότι το EBOD είναι ποτέ συνδεδεμένος μόνο σε ένα ξεχωριστό PDU. Εάν η EBOD και η κύρια περίβλημα αποτύχει την ίδια στιγμή, το σύστημα θα ανακτήσετε.

Εάν μόνο το περίβλημα EBOD αποτυγχάνει σε δύο power προμήθειες, δεν θα αυτόματη ανάκτηση του συστήματος. Ακολουθήστε τα παρακάτω βήματα για να ενεργοποιήσετε το σύστημα και να τον επαναφέρετε σε καλή κατάσταση:

1. Εάν το πρωτεύον περίβλημα είναι ενεργοποιημένη, εναλλαγή απενεργοποιήσετε Power και ψύξης λειτουργικές μονάδες (PCMs).

2. Περιμένετε μερικά λεπτά για να τερματίσετε το σύστημα.

3. Ενεργοποιήστε το περίβλημα EBOD.

4. Αφού το περίβλημα EBOD είναι ενεργοποιημένη, ενεργοποιήστε το πρωτεύον περίβλημα.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Ενεργοποιήστε μια συσκευή μετά την κύρια και έχει χαθεί η σύνδεση περίβλημα EBOD

Εάν χάνεται η σύνδεση μεταξύ του ελεγκτή αναμονής και το αντίστοιχο ελεγκτή EBOD, η συσκευή εξακολουθούν να λειτουργούν. Εάν χάνεται η σύνδεση μεταξύ του ελεγκτή ενεργό σύστημα και το αντίστοιχο ελεγκτή EBOD, πρέπει να πραγματοποιείται ανακατεύθυνσης και στη συσκευή θα πρέπει να συνεχίσετε να εργάζεστε ως συνήθως.

Όταν καταργούνται και τα δύο καλώδια σειριακή συνημμένη SCSI (συσχετισμών Ασφαλείας) ή διακοπή της σύνδεσης μεταξύ του EBOD περίβλημα και το πρωτεύον περίβλημα, η συσκευή σταματά να λειτουργεί. Σε αυτό το σημείο, ακολουθήστε τα παρακάτω βήματα.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Για να ενεργοποιήσετε τη συσκευή μετά έχει χαθεί η σύνδεση

1. Αποκτήστε πρόσβαση στο πίσω μέρος της συσκευής.

2. Εάν η σύνδεση καλώδιο συσχετισμούς Ασφαλείας μεταξύ θάλαμο EBOD και τα κύρια περίβλημα έχει διακοπεί, όλα συσχετισμών Ασφαλείας λωρίδων LED στο θάλαμο EBOD θα είναι απενεργοποιημένη.

3. Τερματισμός ενέργειας και ψύξης λειτουργικές μονάδες (PCMs) σε θάλαμο EBOD και τα κύρια περίβλημα.

4. Περιμένετε μέχρι να απενεργοποιήσετε όλες οι ενδείξεις στο πίσω μέρος τόσο το εσώκλειστων στοιχείων.

5. Τοποθετήστε τα καλώδια συσχετισμών Ασφαλείας και βεβαιωθείτε ότι υπάρχει μια καλή σύνδεση μεταξύ θάλαμο EBOD και το πρωτεύον περίβλημα.

6. Ενεργοποιήστε το περίβλημα EBOD πρώτο, αναστροφή και τα δύο διακόπτες PCM στη θέση Ενεργοποίηση.

7. Βεβαιωθείτε ότι το περίβλημα EBOD είναι σε κατά τον έλεγχο ότι η πράσινη Ένδειξη είναι Ενεργοποιημένη.

8. Ενεργοποιήστε το πρωτεύον περίβλημα.

9. Βεβαιωθείτε ότι είναι το κύριο περίβλημα στην κατά τον έλεγχο ότι η Ένδειξη ελεγκτή πράσινο είναι Ενεργοποιημένη.

10. Βεβαιωθείτε ότι η σύνδεση περίβλημα EBOD με το πρωτεύον περίβλημα είναι καλή, επιλέγοντας ότι το διάδρομο συσχετισμών Ασφαλείας LED (τέσσερις ανά ελεγκτή EBOD) είναι Ενεργοποίηση όλων.

>[AZURE.IMPORTANT] Εάν τα καλώδια συσχετισμών Ασφαλείας είναι ελαττωματικό ή τη σύνδεση μεταξύ του περίβλημα EBOD και το πρωτεύον περίβλημα είναι δεν σας αρέσει, όταν ενεργοποιείτε το σύστημα, θα γίνει η μετάβαση σε κατάσταση λειτουργίας αποκατάστασης. Επικοινωνήστε [με την υποστήριξη της Microsoft](storsimple-contact-microsoft-support.md) Εάν συμβαίνει αυτό.

## <a name="turn-off-a-running-device"></a>Απενεργοποιήστε την επιλογή μιας συσκευής που εκτελείται

Μια ενσωματωμένη συσκευή StorSimple μπορεί να χρειαστεί να τερματιστεί εάν για τη μετακίνηση, που λαμβάνονται από την υπηρεσία, ή να έχει ένα στοιχείο δεν λειτουργεί σωστά που πρέπει να αντικατασταθεί. Τα βήματα διαφέρουν ανάλογα με το αν η συσκευή StorSimple είναι μια 8100 ή ένα μοντέλο 8600. Το 8100 έχει ένα μόνο κύριο περίβλημα, ότι το 8600 είναι μια συσκευή διπλής περίβλημα με ένα πρωτεύον περίβλημα και ένα περίβλημα EBOD. Αυτή η ενότητα λεπτομέρειες για τα βήματα για να τερματίσετε τη συσκευή που χρησιμοποιείτε.

- [Συσκευή με κύρια περίβλημα](#8100a)

- [Συσκευή με EBOD περίβλημα](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Συσκευή με κύρια περίβλημα<a name="8100a"> 

Για να τερματίσετε τη συσκευή με τον τρόπο τακτοποιημένο και ελεγχόμενη, μπορείτε να το κάνετε μέσω της πύλης Azure κλασική ή μέσω του Windows PowerShell για StorSimple. 

>[AZURE.IMPORTANT] Τερματισμός μιας συσκευής που εκτελείται, χρησιμοποιώντας το κουμπί τροφοδοσίας στο πίσω μέρος της συσκευής.
>
>Πριν να τερματίσετε τη συσκευή, βεβαιωθείτε ότι όλα τα στοιχεία συσκευή είναι σε καλή κατάσταση. Στην κλασική Azure πύλη, μεταβείτε σε **συσκευές** > **Συντήρηση** > την**Κατάσταση υλικού**και βεβαιωθείτε ότι η κατάσταση όλων των στοιχείων είναι πράσινο. Αυτό ισχύει μόνο για ένα σύστημα σε καλή κατάσταση. Εάν το σύστημα είναι που Τερματισμός λειτουργίας για να αντικατάσταση ενός στοιχείου που δεν λειτουργεί σωστά, θα δείτε ένα αποτυχίας (κόκκινο) ή υποβαθμισμένο (κίτρινο) κατάστασης για το αντίστοιχο στοιχείο στο την **Κατάσταση υλικού**.

Μετά την πρόσβαση του Windows PowerShell για StorSimple ή Azure κλασική πύλη, ακολουθήστε τα βήματα στο [τερματίσετε μια συσκευή StorSimple](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Συσκευή με EBOD περίβλημα<a name="8600a">

>[AZURE.IMPORTANT] Πριν να τερματίσετε την κύρια περίβλημα και το περίβλημα EBOD, βεβαιωθείτε ότι όλα τα στοιχεία συσκευή είναι σε καλή κατάσταση. Στην κλασική Azure πύλη, μεταβείτε σε **συσκευές** > **Συντήρηση** > την**Κατάσταση υλικού**και βεβαιωθείτε ότι όλα τα στοιχεία είναι σε καλή κατάσταση.

#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Για να τερματίσετε μια ενσωματωμένη συσκευή με EBOD περίβλημα

1. Ακολουθήστε όλα τα βήματα που αναφέρονται σε [τερματίσετε μια συσκευή StorSimple](storsimple-manage-device-controller.md#shut-down-a-storsimple-device) για το πρωτεύον περίβλημα.

2. Αφού τερματιστεί το πρωτεύον περίβλημα, κλείσετε το EBOD αναστροφή απενεργοποίηση διακόπτες Power και ψύξης λειτουργική μονάδα (PCM).

3. Για να επαληθεύσετε ότι το EBOD έχει τερματιστεί, βεβαιωθείτε ότι οι ενδείξεις στο πίσω μέρος θάλαμο EBOD είναι απενεργοποιημένη.

>[AZURE.NOTE] Τα καλώδια συσχετισμών Ασφαλείας που χρησιμοποιούνται για να συνδεθείτε στο θάλαμο EBOD με το πρωτεύον περίβλημα δεν πρέπει να καταργηθεί μέχρι μετά το σύστημα έχει τερματιστεί.

## <a name="next-steps"></a>Επόμενα βήματα

[Επικοινωνήστε με την υποστήριξη της Microsoft](storsimple-contact-microsoft-support.md) Εάν αντιμετωπίσετε προβλήματα κατά την ενεργοποίηση της ή τερματισμός μιας συσκευής StorSimple.

