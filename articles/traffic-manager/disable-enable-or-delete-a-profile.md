<properties
   pageTitle="Απενεργοποίηση, να ενεργοποιήσετε ή διαγραφή προφίλ Manager κίνηση | Microsoft Azure"
   description="Σε αυτό το άρθρο θα σας βοηθήσει να εργαστείτε με το προφίλ της διαχείρισης κίνηση."
   services="traffic-manager"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="disable-enable-or-delete-a-profile"></a>Απενεργοποίηση της, να ενεργοποιήσετε ή να διαγράψετε ένα προφίλ


Μπορείτε να απενεργοποιήσετε ένα υπάρχον προφίλ κίνηση Manager, έτσι ώστε το δεν θα αναφέρεται αιτήσεις χρήστη για τη ρύθμιση παραμέτρων τελικά σημεία. Όταν απενεργοποιείτε ένα προφίλ διαχείρισης κίνηση, το ίδιο το προφίλ και τις πληροφορίες που περιέχονται στο προφίλ θα παραμένουν ανέπαφες και μπορεί να είναι δυνατή η επεξεργασία του στο περιβάλλον εργασίας διαχείρισης κίνηση. Όταν θέλετε να ενεργοποιήσετε ξανά το προφίλ, μπορείτε εύκολα να κάνετε, ώστε να στο το Azure συνέχιση πύλη και των αναφορών. Όταν δημιουργείτε ένα προφίλ διαχείρισης κίνηση στην πύλη του Azure, ενεργοποιείται αυτόματα.

## <a name="to-disable-a-profile"></a>Για να απενεργοποιήσετε ένα προφίλ

1. Τροποποίηση της εγγραφής πόρου DNS στο διακομιστή DNS στο Internet για να χρησιμοποιήσετε το κατάλληλο record type και το δείκτη του ποντικιού σε ένα άλλο όνομα ή τη διεύθυνση IP του μια συγκεκριμένη θέση στο Internet. Αλλάξτε την εγγραφή DNS πόρων στο διακομιστή DNS στο Internet με άλλα λόγια, έτσι ώστε να χρησιμοποιεί δεν είναι πλέον μια εγγραφή πόρου CNAME που οδηγεί στο όνομα τομέα του προφίλ σας Manager κίνηση.
1. Κίνηση θα σταματήσει να κατευθύνουν να τα τελικά σημεία μέσω των ρυθμίσεων προφίλ Manager κίνηση.
1. Επιλέξτε το προφίλ που θέλετε να απενεργοποιήσετε. Για να επιλέξετε το προφίλ, στη σελίδα Διαχείριση κίνηση, επισημάνετε το προφίλ κάνοντας κλικ στη στήλη δίπλα στο όνομα του προφίλ. Μην κάνετε κλικ στο όνομα του προφίλ ή στο βέλος δίπλα στο όνομα, όπως θα μεταβείτε στη σελίδα ρυθμίσεων για το προφίλ.
1. Αφού επιλέξετε το προφίλ, κάντε κλικ στην επιλογή Απενεργοποίηση στο κάτω μέρος της σελίδας.

## <a name="to-enable-a-profile"></a>Για να ενεργοποιήσετε ένα προφίλ

1. Επιλέξτε το προφίλ που θέλετε να ενεργοποιήσετε. Για να επιλέξετε το προφίλ, στη σελίδα Διαχείριση κίνηση, επισημάνετε το προφίλ κάνοντας κλικ στη στήλη δίπλα στο όνομα του προφίλ. Μην κάνετε κλικ στο όνομα του προφίλ ή στο βέλος δίπλα στο όνομα, όπως θα μεταβείτε στη σελίδα ρυθμίσεων για το προφίλ.
1. Αφού επιλέξετε το προφίλ, κάντε κλικ στην επιλογή Ενεργοποίηση στο κάτω μέρος της σελίδας.
1. Τροποποίηση της εγγραφής πόρου DNS στο διακομιστή DNS στο Internet για να χρησιμοποιήσετε τον τύπο εγγραφής CNAME που αντιστοιχίζει όνομα τομέα της εταιρείας σας με το όνομα τομέα του προφίλ σας Manager κίνηση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [σημείο τομέα Internet εταιρείας σε έναν τομέα Manager κίνηση](traffic-manager-point-internet-domain.md).
1. Κίνηση θα αρχίσει να κατευθύνουν ξανά τα τελικά σημεία.

## <a name="delete-a-profile"></a>Διαγραφή προφίλ


1. Βεβαιωθείτε ότι η εγγραφή πόρου DNS στο διακομιστή DNS στο Internet χρησιμοποιεί δεν είναι πλέον μια εγγραφή πόρου CNAME που οδηγεί στο όνομα τομέα του προφίλ σας Manager κίνηση.
1. Επιλέξτε το προφίλ που θέλετε να διαγράψετε. Για να επιλέξετε το προφίλ, στη σελίδα Διαχείριση κίνηση, επισημάνετε το προφίλ με
1. κάνοντας κλικ στη στήλη δίπλα στο προφίλ. Μην κάνετε κλικ στο όνομα του προφίλ ή στο βέλος δίπλα στο όνομα, όπως θα μεταβείτε στη σελίδα ρυθμίσεων για το προφίλ.
1. Αφού επιλέξετε το προφίλ, κάντε κλικ στην επιλογή Διαγραφή στο κάτω μέρος της σελίδας.

## <a name="next-steps"></a>Επόμενα βήματα

[Κίνηση Manager - Απενεργοποίηση ή ενεργοποίηση ενός ορίου](disable-or-enable-an-endpoint.md)

[Ρύθμιση παραμέτρων μέθοδο δρομολόγησης ανακατεύθυνσης](traffic-manager-configure-failover-routing-method.md)

[Ρύθμιση παραμέτρων μέθοδο δρομολόγησης round robin](traffic-manager-configure-round-robin-routing-method.md)

[Ρύθμιση παραμέτρων μέθοδο δρομολόγησης επιδόσεων](traffic-manager-configure-performance-routing-method.md)

[Διαχείριση αντιμετώπισης προβλημάτων κίνηση υποβαθμισμένο κατάσταση](traffic-manager-troubleshooting-degraded.md)

