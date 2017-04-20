<properties
    pageTitle="Συνδεθείτε με ένα κλασικό VM Azure | Microsoft Azure"
    description="Χρησιμοποιήστε την κλασική πύλη Azure για να συνδεθείτε με ένα εικονικό μηχάνημα Windows που δημιουργήθηκαν με το μοντέλο ανάπτυξης classic."
    services="virtual-machines-windows"
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
    ms.date="07/28/2016"
    ms.author="cynthn"/>


# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-classic-portal"></a>Συνδεθείτε με ένα εικονικό μηχάνημα Windows χρησιμοποιώντας την κλασική πύλη Azure

Στην κλασική πύλη Azure, μπορείτε να χρησιμοποιήσετε το κουμπί " **σύνδεση** " για να ξεκινήσετε μια περίοδο λειτουργίας απομακρυσμένης επιφάνειας εργασίας και να συνδεθείτε με ένα Windows VM.

Θέλετε να συνδεθείτε σε μια εικονική Μηχανή Linux; Δείτε [πώς μπορείτε να συνδεθείτε με ένα εικονικό μηχάνημα με Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας τη νέα πύλη Azure](virtual-machines-windows-connect-logon.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] 

## <a name="video-walkthrough"></a>Αναλυτική παρουσίαση βίντεο

Ακολουθεί μια αναλυτική παρουσίαση βίντεο από τα βήματα σε αυτό το πρόγραμμα εκμάθησης. Καλύπτει επίσης απολήξεις και δημόσιων και ιδιωτικών θύρες που χρησιμοποιούνται για τη σύνδεση με τα Windows VM στο Azure.

[AZURE.VIDEO logging-on-to-vm-running-windows-server-on-azure]


## <a name="connect-to-the-virtual-machine"></a>Συνδεθείτε με την εικονική μηχανή

1. Εισέλθετε στην πύλη κλασική Azure.

2. Κάντε κλικ στο κουμπί **εικονικές μηχανές**και, στη συνέχεια, επιλέξτε την εικονική μηχανή.

3. Στη γραμμή εντολών, στο κάτω μέρος της σελίδας, κάντε κλικ στο κουμπί " **σύνδεση**".

    ![Συνδεθείτε με την εικονική μηχανή](./media/virtual-machines-windows-classic-connect-logon/connectwindows.png)
    
> [AZURE.TIP] Εάν δεν είναι διαθέσιμο το κουμπί **σύνδεσης** , δείτε τις συμβουλές αντιμετώπισης προβλημάτων στο τέλος αυτού του άρθρου.

## <a name="log-on-to-the-virtual-machine"></a>Συνδεθείτε με την εικονική μηχανή

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Επόμενα βήματα

-   Εάν το κουμπί **σύνδεσης** είναι ανενεργό ή αντιμετωπίζετε άλλα προβλήματα με τη σύνδεση απομακρυσμένης επιφάνειας εργασίας, προσπαθήστε να επαναφέρετε τη ρύθμιση παραμέτρων. Από τον πίνακα εργαλείων εικονικής μηχανής, στην ενότητα **Γρήγορη Glance**, κάντε κλικ στο κουμπί **Επαναφορά απομακρυσμένη ρύθμιση παραμέτρων**.
-   Για προβλήματα με τον κωδικό πρόσβασής σας, δοκιμάστε την επαναφορά της. Από τον πίνακα εργαλείων εικονικής μηχανής, στην ενότητα **Γρήγορη Glance**, κάντε κλικ στο κουμπί **Επαναφορά κωδικού πρόσβασης**.

Εάν οι συμβουλές δεν λειτουργεί ή δεν είναι ό, τι χρειάζεστε, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων συνδέσεων απομακρυσμένης επιφάνειας εργασίας σε ένα με Windows Azure εικονική μηχανή](virtual-machines-windows-troubleshoot-rdp-connection.md). Αυτό το άρθρο σας καθοδηγεί μέσω τη διάγνωση και την επίλυση συνηθισμένων προβλημάτων.


