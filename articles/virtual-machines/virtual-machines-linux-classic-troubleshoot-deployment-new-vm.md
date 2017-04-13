<properties
   pageTitle="Αντιμετώπιση προβλημάτων Εικονική Linux ανάπτυξης-κλασική | Microsoft Azure"
   description="Αντιμετώπιση προβλημάτων κλασική ανάπτυξης όταν δημιουργείτε μια νέα εικονική μηχανή Linux στο Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/06/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Αντιμετώπιση προβλημάτων κλασική ανάπτυξης με τη δημιουργία ενός νέου Linux εικονική μηχανή στο Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Αρχεία καταγραφής ελέγχου συλλογής

Για να ξεκινήσετε την αντιμετώπιση προβλημάτων, συλλογής των αρχείων καταγραφής ελέγχου για να προσδιορίσετε το σφάλμα που σχετίζεται με το ζήτημα.

Στην πύλη του Azure, κάντε κλικ στην επιλογή **Αναζήτηση** > **εικονικές μηχανές** > *σας Windows εικονική μηχανή* > **Ρυθμίσεις** > **αρχεία καταγραφής ελέγχου**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** Εάν το λειτουργικό σύστημα είναι Linux γενίκευση και το έχει αποσταλεί ή/και καταγράφονται με τη ρύθμιση γενικευμένη, στη συνέχεια, δεν υπάρχει σφάλματα. Ομοίως, εάν είναι το λειτουργικό σύστημα Linux εξειδικευμένες, και αυτό είναι που έχουν αποσταλεί ή/και καταγράφονται με τη ρύθμιση εξειδικευμένες, στη συνέχεια, δεν υπάρχει σφάλματα.

**Αποστολή σφαλμάτων:**

**N<sup>1</sup>:** Εάν το λειτουργικό σύστημα είναι Linux γενίκευση και την αποστολή ως εξειδικευμένες, θα λάβετε ένα σφάλμα χρονικού ορίου παροχής επειδή η Εικονική έχει κολλήσει στο στάδιο της προετοιμασίας.

**N<sup>2</sup>:** Εάν το λειτουργικό σύστημα είναι εξειδικευμένες Linux και την αποστολή ως γενίκευση, θα λάβετε ένα σφάλμα αποτυχίας παροχής επειδή η νέα Εικονική εκτελείται με το αρχικό όνομα υπολογιστή, όνομα χρήστη και τον κωδικό πρόσβασης.

**Ανάλυση:**

Για να επιλύσετε και τα δύο αυτά τα σφάλματα, αποστείλετε το αρχικό VHD, διαθέσιμη εσωτερικής εγκατάστασης, με τις ίδιες ρυθμίσεις με αυτό για το λειτουργικό σύστημα (γενίκευση/εξειδικευμένες). Για να αποστείλετε γενίκευση, θυμηθείτε να εκτελέσετε - deprovision πρώτα. Ανατρέξτε στο θέμα [Δημιουργία και αποστολή ενός εικονικού σκληρού δίσκου που περιέχει το λειτουργικό σύστημα Linux](virtual-machines-linux-classic-create-upload-vhd.md) για περισσότερες πληροφορίες.

**Καταγραφή σφαλμάτων:**

**N<sup>3</sup>:** Εάν το λειτουργικό σύστημα είναι Linux γενίκευση και καταγραφής ως εξειδικευμένες, θα λάβετε ένα σφάλμα χρονικού ορίου παροχής επειδή το αρχικό Εικονική δεν μπορεί να χρησιμοποιηθεί ως έχει σημανθεί ως γενίκευση.

**N<sup>4</sup>:** Εάν το λειτουργικό σύστημα είναι εξειδικευμένες Linux και καταγράφεται ως γενίκευση, θα λάβετε ένα σφάλμα αποτυχίας παροχής επειδή η νέα Εικονική εκτελείται με το αρχικό όνομα υπολογιστή, όνομα χρήστη και τον κωδικό πρόσβασης. Επίσης, το αρχικό Εικονική δεν μπορεί να χρησιμοποιηθεί επειδή έχει σημανθεί ως εξειδικευμένες.

**Ανάλυση:**

Για να επιλύσετε και τα δύο αυτά τα σφάλματα, διαγράψτε την τρέχουσα εικόνα από την πύλη και [επαναλάβετε την καταγραφή από την τρέχουσα VHD](virtual-machines-linux-classic-capture-image.md) με τις ίδιες ρυθμίσεις με αυτό για το λειτουργικό σύστημα (γενίκευση/εξειδικευμένες).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Θέμα: Προσαρμογή / συλλογή / εικόνα marketplace. Αποτυχία εκχώρησης
Αυτό το σφάλμα προκύπτει στις περιπτώσεις, όταν η νέα αίτηση Εικονική αποστέλλεται σε ένα σύμπλεγμα που δεν διαθέτει διαθέσιμου ελεύθερου χώρου για να χωρέσει την αίτηση, είτε δεν μπορεί να υποστηρίξει το μέγεθος Εικονική που ζητήθηκε. Δεν είναι δυνατό να συνδυάσετε διαφορετικών σειρών ΣΠΣ στην ίδια υπηρεσία cloud. Επομένως, εάν θέλετε να δημιουργήσετε μια νέα Εικονική με διαφορετικό μέγεθος από την υπηρεσία cloud που μπορούν να υποστηρίξουν, την αίτηση υπολογισμού θα αποτύχει.

Ανάλογα με τους περιορισμούς του την υπηρεσία cloud που χρησιμοποιείτε για να δημιουργήσετε τη νέα Εικονική, ενδέχεται να παρουσιαστεί σφάλμα οφείλεται σε μία από δύο περιπτώσεις.

**Αιτία 1:** Την υπηρεσία cloud είναι καρφιτσωμένα στη ένα συγκεκριμένο σύμπλεγμα ή είναι συνδεδεμένη με μια ομάδα συσχέτισης και επομένως καρφιτσωμένα σε ένα συγκεκριμένο σύμπλεγμα από τη σχεδίαση. Επομένως, νέο πόρο υπολογισμού προσκλήσεις σε που έχουν δοκιμάσει ομάδα συσχέτισης στο ίδιο σύμπλεγμα όπου φιλοξενούνται οι υπάρχουσες πόροι. Ωστόσο, το ίδιο σύμπλεγμα μπορεί να είτε δεν υποστηρίζουν το απαιτούμενο μέγεθος Εικονική ή έχετε διαθέσιμος χώρος δεν επαρκεί, που προκύπτει σφάλμα εκχώρησης. Αυτό ισχύει αν οι πόροι νέα δημιουργούνται μέσω μια νέα υπηρεσία cloud ή μια υπάρχουσα υπηρεσία cloud.

**Επίλυση 1:**

- Δημιουργήστε μια νέα υπηρεσία στο cloud και να συσχετίσετε με μια περιοχή ή ένα εικονικό δίκτυο που βασίζεται σε περιοχή.
- Δημιουργήστε μια νέα Εικονική στην νέα υπηρεσία cloud.
  Εάν λάβετε ένα μήνυμα σφάλματος όταν προσπαθείτε να δημιουργήσετε μια νέα υπηρεσία cloud, προσπαθήστε ξανά αργότερα ή να αλλάξετε την περιοχή για την υπηρεσία cloud.

> [AZURE.IMPORTANT] Εάν προσπαθήσατε να δημιουργήσετε μια νέα Εικονική σε μια υπάρχουσα υπηρεσία cloud, αλλά δεν ήταν δυνατό και έπρεπε να δημιουργήσετε μια νέα υπηρεσία στο cloud για τον νέο Εικονική, μπορείτε να επιλέξετε για τη συνολική εικόνα όλες τις ΣΠΣ στην ίδια υπηρεσία cloud. Για να το κάνετε αυτό, διαγράψτε το ΣΠΣ στην υπάρχουσα υπηρεσία cloud και επαναλάβετε την καταγραφή τους από τους δίσκων στην νέα υπηρεσία cloud. Ωστόσο, είναι σημαντικό να θυμάστε ότι η νέα υπηρεσία cloud θα έχει ένα νέο όνομα και VIP, έτσι θα πρέπει να ενημερώσετε τα για όλες τις εξαρτήσεις που χρησιμοποιούν αυτήν τη στιγμή αυτές τις πληροφορίες για την υπάρχουσα υπηρεσία cloud.

**Προκαλέσει 2:** Η υπηρεσία cloud είναι συσχετισμένη με μια εικονικού δικτύου που είναι συνδεδεμένη με μια ομάδα συσχέτισης, προκειμένου το είναι καρφιτσωμένα στη ένα συγκεκριμένο σύμπλεγμα από τη σχεδίαση. Όλα τα νέο πόρο υπολογισμού προσκλήσεις σε που ομάδα συσχέτισης, επομένως, είναι δοκιμάσει στο ίδιο σύμπλεγμα όπου φιλοξενούνται οι υπάρχουσες πόροι. Ωστόσο, το ίδιο σύμπλεγμα μπορεί να είτε δεν υποστηρίζει το απαιτούμενο μέγεθος Εικονική ή διαθέτει ανεπαρκές διαθέσιμο χώρο, με αποτέλεσμα σφάλμα εκχώρησης. Αυτό ισχύει αν οι πόροι νέα δημιουργούνται μέσω μια νέα υπηρεσία cloud ή μια υπάρχουσα υπηρεσία cloud.

**Επίλυση 2:**

- Δημιουργήστε μια νέα τοπικές εικονικού δικτύου.
- Δημιουργήστε το νέο Εικονική στο νέο εικονικό δίκτυο.
- [Σύνδεση του υπάρχοντος εικονικού δικτύου](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) με το νέο δίκτυο εικονικού. Δείτε περισσότερες πληροφορίες σχετικά με τις [Τοπικές εικονικών δικτύων](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Εναλλακτικά, μπορείτε να κάνετε [μετεγκατάσταση συσχέτισης-με βάση την ομάδα εικονικού το δίκτυό σας σε τοπικές εικονικό δίκτυο](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), και, στη συνέχεια, δημιουργήστε το νέο Εικονική.

## <a name="next-steps"></a>Επόμενα βήματα
Εάν αντιμετωπίσετε προβλήματα κατά την εκκίνηση ενός σταμάτησε Εικονική Linux ή αλλαγή μεγέθους ενός υπάρχοντος Εικονική Linux στα Azure, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων κλασική ανάπτυξης με επανεκκίνηση ή αλλαγή μεγέθους ενός υπάρχοντος Linux εικονική μηχανή στο Azure](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md).