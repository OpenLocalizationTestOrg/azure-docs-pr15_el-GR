<properties 
    pageTitle="Azure RemoteApp εύρος ζώνης δικτύου - γενικές οδηγίες | Microsoft Azure"
    description="Κατανόηση ορισμένες οδηγίες εύρους ζώνης βασικού δικτύου για τις συλλογές Azure RemoteApp και εφαρμογές."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp εύρος ζώνης δικτύου - γενικές οδηγίες (Εάν δεν μπορείτε να ελέγξετε τη δική σας)

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Εάν δεν έχετε το χρόνο ή τη δυνατότητα να εκτελέσετε τις [δοκιμές εύρους ζώνης δικτύου](remoteapp-bandwidthtests.md) για το Azure RemoteApp, εδώ θα βρείτε ορισμένες οδηγίες αρκετά γενική που σας βοηθούν να εκτιμήσετε το εύρος ζώνης δικτύου ανά χρήστη.

Εάν έχετε ένα μείγμα από αυτά τα σενάρια, δεν συνιστάται να οτιδήποτε μικρότερο από (ή ίσο με) 10 MB/s ως το εύρος ζώνης δικτύου ΕΛΆΧΙΣΤΗ για σύγχρονη συνδέονται μέσω του Internet εφαρμογές σε περιβάλλον απομακρυσμένο. (Παρόλο που, όπως περιγράφεται, αυτό δεν εγγυάται καλύτερη από την εμπειρία χρήστη average.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Σύνθετη PowerPoint, απλή PowerPoint

Azure RemoteApp λειτουργεί καλύτερα σε τοπικό ΔΊΚΤΥΟ 100 MB. Τα 10 MB/s προφίλ δικτύου, όταν τρεμόπαιγμα παραπάνω 120 ms είναι μεγαλύτερο από 5%, ο χρήστης θα δείτε μια εμπειρία μέσος όρος. Στις 1 MB/s διαφορετικών glaring - για παράδειγμα, σε μια προβολή παρουσίασης, ο χρήστης ενδέχεται να μην βλέπετε εναλλαγές με κίνηση καθόλου επειδή παραλείπονται καρέ.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer, μεικτή PDF, PDF, κείμενο

Προφίλ δικτύου 10 MB/s είναι κοντά LAN σε περισσότερες παραμέτρους. 1 MB/s παρέχει μια εμπειρία OK, παρόλο που μπορεί να υπάρχουν ορισμένες τρεμόπαιγμα όταν ο χρήστης κάνει κύλιση ενώ υπάρχουν εικόνες στην οθόνη.

## <a name="flash-video-youtube"></a>Βίντεο Flash (YouTube)

100 MB/s LAN παρέχει βέλτιστη εμπειρία, ενώ 10 MB/s είναι αποδεκτή (που σημαίνει μας Διατήρηση με το ρυθμό καρέ αλλά jitter αυξάνεται). Στις 1 MB/s, τρεμόπαιγμα είναι πολύ υψηλή και εμφανών.

## <a name="word-typing-word-remote-input"></a>Το Word πληκτρολόγησης (Word απομακρυσμένο είσοδος)
Αυτό είναι ένα σενάριο χρήση χαμηλού εύρους ζώνης. Στα 256 KB/s παρέχουμε ως καλή από μια εμπειρία ως LAN.

## <a name="learn-more"></a>Μάθε περισσότερα
- [Εκτίμηση της χρήσης του εύρους ζώνης δικτύου Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp - πώς εύρος ζώνης δικτύου και την ποιότητα της εμπειρία εργασίας μαζί;](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp - tseting σας χρήσης του εύρους ζώνης δικτύου με ορισμένα κοινά σενάρια](remoteapp-bandwidthtests.md)