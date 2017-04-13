<properties
    pageTitle="Δημιουργήστε μια Εικονική στην πύλη του κλασική | Microsoft Azure"
    description="Δημιουργήστε μια εικονική μηχανή Windows στην πύλη του Azure κλασική."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="cynthn"/>

# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Δημιουργήστε μια εικονική μηχανή με Windows στην πύλη του Azure κλασική

> [AZURE.SELECTOR]
- [Azure κλασική πύλη](virtual-machines-windows-classic-tutorial.md)
- [PowerShell: Ανάπτυξη κλασική](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-machines-windows-hero-tutorial.md) με τη **νέα πύλη Azure**. 

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να δημιουργήσετε μια Azure εικονική μηχανή (Εικονική) με Windows στην πύλη του Azure κλασική. Θα χρησιμοποιήσουμε μια εικόνα του Windows Server ως παράδειγμα, αλλά αυτό είναι μόνο μία από τις πολλές εικόνες προσφέρει Azure. Σημειώστε ότι οι επιλογές σας εικόνα εξαρτώνται από τη συνδρομή σας. Για παράδειγμα, μπορεί να είναι διαθέσιμη για τους συνδρομητές του MSDN εικόνες στην επιφάνεια εργασίας των Windows.

Αυτή η ενότητα σας δείχνει πώς μπορείτε να χρησιμοποιήσετε την επιλογή **Από τη συλλογή** στην πύλη του Azure κλασική για να δημιουργήσετε την εικονική μηχανή. Αυτή η επιλογή παρέχει περισσότερες επιλογές ρύθμισης παραμέτρων από την επιλογή " **Γρήγορη δημιουργία** ". Για παράδειγμα, εάν θέλετε να συμμετάσχετε σε μια εικονική μηχανή σε εικονικό δίκτυο, θα χρειαστεί να χρησιμοποιήσετε την επιλογή **Από τη συλλογή** .

Μπορείτε επίσης να δημιουργήσετε ΣΠΣ χρησιμοποιώντας [τις δικές σας εικόνες](virtual-machines-windows-classic-createupload-vhd.md). Για να μάθετε σχετικά με αυτήν και άλλες μεθόδους, ανατρέξτε στο θέμα [διαφορετικούς τρόπους για να δημιουργήσετε μια εικονική μηχανή των Windows](virtual-machines-windows-creation-choices.md).



## <a name="video-walkthrough"></a>Αναλυτικές οδηγίες για βίντεο

Παρακάτω θα δείτε αναλυτικές οδηγίες για αυτό το πρόγραμμα εκμάθησης.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>Δημιουργία η εικονική μηχανή

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε πώς μπορείτε να [δημιουργήσετε μια Εικονική χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-machines-windows-hero-tutorial.md) στην πύλη του νέου Azure. 

- Συνδεθείτε με την εικονική μηχανή. Για οδηγίες, ανατρέξτε στο θέμα [σύνδεση μια εικονική μηχανή εκτελούν τον Windows Server](virtual-machines-windows-classic-connect-logon.md).

- Επισυνάψτε ένα δίσκο για την αποθήκευση δεδομένων. Μπορείτε να επισυνάψετε τόσο κενή δίσκων και δίσκων που περιέχουν δεδομένα. Για οδηγίες, ανατρέξτε στο θέμα την [Επισύναψη ένα δίσκο δεδομένων σε μια εικονική μηχανή των Windows που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης](virtual-machines-windows-classic-attach-disk.md).
