<properties
    pageTitle="Επισυνάψτε ένα δίσκο σε μια Εικονική | Microsoft Azure"
    description="Επισυνάψτε ένα δίσκο δεδομένων σε μια εικονική μηχανή των Windows που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης και προετοιμασία του."
    services="virtual-machines-windows, storage"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="cynthn"/>

# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Επισυνάψτε ένα δίσκο δεδομένων σε μια εικονική μηχανή των Windows που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Εάν θέλετε να χρησιμοποιήσετε τη νέα πύλη, δείτε [πώς μπορείτε να επισυνάψετε ένα δίσκο δεδομένων σε μια Εικονική Windows στην πύλη του Azure](virtual-machines-windows-attach-disk-portal.md).

Εάν χρειάζεστε ένα δίσκο πρόσθετα δεδομένα, μπορείτε να επισυνάψετε ένα κενό δίσκο ή σε έναν υπάρχοντα δίσκο με δεδομένα σε μια Εικονική. Και στις δύο περιπτώσεις, η δίσκων είναι .vhd αρχεία που βρίσκονται σε ένα λογαριασμό Azure χώρου αποθήκευσης. Στην περίπτωση ενός νέου δίσκου, αφού συνδέσετε το δίσκο, μπορείτε επίσης θα χρειαστεί να προετοιμάσετε ώστε να είναι έτοιμος για χρήση από μια Εικονική των Windows.

Για περισσότερες λεπτομέρειες σχετικά με την δίσκων, ανατρέξτε στο θέμα [σχετικά με δίσκων και VHD για εικονικές μηχανές](virtual-machines-windows-about-disks-vhds.md).


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>Προετοιμάστε το δίσκο

1. Σύνδεση με την εικονική μηχανή. Για οδηγίες, ανατρέξτε στο θέμα [πώς μπορείτε να συνδεθείτε με μια εικονική μηχανή εκτελούν τον Windows Server][logon].

2. Αφού συνδεθείτε με την εικονική μηχανή, ανοίξτε τη **Διαχείριση διακομιστή**. Στο αριστερό παράθυρο, επιλέξτε **αρχείο και υπηρεσίες αποθήκευσης**.

    ![Άνοιγμα της διαχείρισης διακομιστή](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Αναπτύξτε το μενού και επιλέξτε **δίσκων**.

4. Στην ενότητα **δίσκων** παραθέτει δίσκων. Στις περισσότερες περιπτώσεις, θα έχει δίσκο 0, 1, και δίσκο 2. Δίσκος 0 είναι το λειτουργικό σύστημα, δίσκος 1 είναι το προσωρινό και δίσκος 2 είναι ο δίσκος δεδομένων που έχετε επισυνάψει απλώς να την εικονική Μηχανή. Το νέο δίσκο δεδομένων θα εμφανιστούν τα διαμερίσματα ως **Άγνωστη**. Κάντε δεξί κλικ στο δίσκο και επιλέξτε **Προετοιμασία**.

5.  Ειδοποιείστε ότι όλα τα δεδομένα θα διαγραφούν όταν ο δίσκος έχει προετοιμαστεί. Κάντε κλικ στο κουμπί **Ναι** για να αποδεχτείτε την προειδοποίηση και να προετοιμάσετε το δίσκο. Μόλις ολοκληρωθεί η λήψη, το Partion θα εμφανίζεται ως **GPT**. Κάντε ξανά δεξί κλικ στο δίσκο και επιλέξτε **Νέα ένταση ήχου**.

6.  Ολοκλήρωση του οδηγού χρησιμοποιώντας τις προεπιλεγμένες τιμές. Όταν ολοκληρωθεί ο οδηγός, στην ενότητα **όγκους** παραθέτει την ένταση νέα. Ο δίσκος είναι τώρα σύνδεση και έτοιμη για την αποθήκευση δεδομένων.

    ![Προετοιμασία με επιτυχία έντασης ήχου](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] Το μέγεθος του η Εικονική καθορίζει τον αριθμό των δίσκων μπορείτε να επισυνάψετε σε αυτό. Για λεπτομέρειες, ανατρέξτε στο θέμα [μεγέθη για εικονικές μηχανές](virtual-machines-linux-sizes.md).

## <a name="additional-resources"></a>Πρόσθετοι πόροι

[Πώς να αποσπάσετε ένα δίσκο από μια εικονική μηχανή των Windows](virtual-machines-windows-classic-detach-disk.md)

[Σχετικά με την δίσκων και VHD για εικονικές μηχανές](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md
