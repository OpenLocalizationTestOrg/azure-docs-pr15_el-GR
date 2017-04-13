<properties
    pageTitle="Καταγράψτε μια εικόνα από μια Εικονική Windows Azure | Microsoft Azure"
    description="Καταγράψτε μια εικόνα από μια εικονική μηχανή Azure Windows που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης."
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
    ms.date="09/27/2016"
    ms.author="cynthn"/>

#<a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Καταγράψτε μια εικόνα από μια εικονική μηχανή Azure Windows που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Για τη διαχείριση πόρων μοντέλο πληροφορίες, ανατρέξτε [Δημιουργία αντιγράφου Εικονική Windows εκτελείται στο Azure](virtual-machines-windows-vhd-copy.md).


Αυτό το άρθρο παρουσιάζει τον τρόπο για να καταγράψετε μια Azure εικονική μηχανή με Windows, ώστε να μπορείτε να την χρησιμοποιήσετε ως εικόνα για να δημιουργήσετε άλλες εικονικές μηχανές. Αυτή η εικόνα περιλαμβάνει ο δίσκος λειτουργικού συστήματος και οποιαδήποτε δίσκων δεδομένων που είναι συνημμένα σε η εικονική μηχανή. Δεν περιλαμβάνει τις παραμέτρους δικτύου, θα χρειαστεί να ρυθμίσετε τις παραμέτρους αυτές όταν δημιουργείτε τις άλλες εικονικές μηχανές που χρησιμοποιούν την εικόνα.

Azure αποθηκεύει την εικόνα στην περιοχή **Εικόνες μου**. Αυτή είναι η ίδια θέση όπου είναι αποθηκευμένες οι τις εικόνες που έχετε αποστείλει. Για λεπτομέρειες σχετικά με εικόνες, ανατρέξτε στο θέμα [πληροφορίες για τις εικόνες για εικονικές μηχανές](virtual-machines-linux-classic-about-images.md).

##<a name="before-you-begin"></a>Πριν ξεκινήσετε##

Τα παρακάτω βήματα λαμβάνεται ως δεδομένο ότι έχετε ήδη δημιουργήσει μια εικονική μηχανή Azure και έχει ρυθμιστεί το λειτουργικό σύστημα, συμπεριλαμβανομένων των επισύναψη οποιαδήποτε δίσκων δεδομένων. Εάν δεν το έχετε κάνει ακόμα, δείτε αυτές τις οδηγίες:

- [Δημιουργήστε μια εικονική μηχανή από μια εικόνα](virtual-machines-windows-classic-createportal.md)
- [Πώς να επισυνάψετε ένα δίσκο δεδομένων σε μια εικονική μηχανή](virtual-machines-windows-classic-attach-disk.md)
- Βεβαιωθείτε ότι τους ρόλους διακομιστή που υποστηρίζονται με το Sysprep. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Υποστήριξη Sysprep για τους ρόλους διακομιστή](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.WARNING] Αυτή η διαδικασία διαγράφει το αρχικό εικονική μηχανή αφού καταγραφής. 

Πριν από την caputuring μια εικόνα από μια εικονική μηχανή Azure, συνιστάται η εικονική μηχανή προορισμού δημιουργία αντιγράφων ασφαλείας. Azure εικονικές μηχανές μπορούν να δημιουργηθούν αντίγραφα χρησιμοποιώντας Azure αντίγραφο ασφαλείας. Για λεπτομέρειες, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας Azure εικονικές μηχανές](../backup/backup-azure-vms.md). Άλλες λύσεις είναι διαθέσιμες από πιστοποιημένους συνεργάτες. Για να μάθετε τι είναι διαθέσιμη αυτήν τη στιγμή, αναζητήστε το Azure Marketplace.


##<a name="capture-the-virtual-machine"></a>Καταγραφή η εικονική μηχανή

1. Στην [πύλη του Azure κλασική](http://manage.windowsazure.com), **σύνδεση** με την εικονική μηχανή. Για οδηγίες, ανατρέξτε στο θέμα [πώς μπορείτε να εισέλθετε με ένα εικονικό μηχάνημα που εκτελούν τον Windows Server] [].

2.  Ανοίξτε ένα παράθυρο γραμμής εντολών ως διαχειριστής.

3.  Αλλάξτε τον κατάλογο για να `%windir%\system32\sysprep`, και, στη συνέχεια, να εκτελέσετε το sysprep.exe.

4.  Εμφανίζεται το παράθυρο διαλόγου **Εργαλείο προετοιμασίας συστήματος** . Κάντε τα εξής:

    - Στην **Ενέργεια εκκαθάρισης συστήματος**, επιλέξτε **Εισαγωγή συστήματος της πρώτης εμπειρία (OOBE)** και βεβαιωθείτε ότι είναι επιλεγμένο **Generalize** . Για περισσότερες πληροφορίες σχετικά με τη χρήση του Sysprep, ανατρέξτε στο θέμα [τον τρόπο χρήσης Sysprep: μια εισαγωγή][].

    - Στις **Επιλογές τερματισμού**, επιλέξτε **τερματισμού**.

    - Κάντε κλικ στο **κουμπί OK**.

    ![Εκτελέστε το Sysprep](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)

7.  Sysprep τερματίζει το εικονικό υπολογιστή, το οποίο αλλάζει την κατάσταση των η εικονική μηχανή στην πύλη του Azure κλασική να **διακοπεί**.

8.  Στην κλασική Azure πύλη, κάντε κλικ στην επιλογή **εικονικές μηχανές** και επιλέξτε την εικονική μηχανή που θέλετε να καταγράψετε.

9.  Στη γραμμή εντολών, κάντε κλικ στην επιλογή **Καταγραφή**.

    ![Καταγραφή εικονική μηχανή](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)

    Εμφανίζεται το παράθυρο διαλόγου **Καταγραφή η εικονική μηχανή** .

10. Στο πλαίσιο **Όνομα εικόνας**, πληκτρολογήστε ένα όνομα για τη νέα εικόνα.

11. Πριν να προσθέσετε μια εικόνα διακομιστή των Windows με το σύνολο των προσαρμοσμένων εικόνων, πρέπει να γενίκευση, εκτελώντας Sysprep σύμφωνα με τις οδηγίες στα προηγούμενα βήματα. Κάντε κλικ στην επιλογή **να εκτελέσετε το Sysprep στον υπολογιστή εικονικές** για να υποδείξετε που κάνατε.

12. Κάντε κλικ στο σημάδι ελέγχου για να καταγράψετε την εικόνα. Η νέα εικόνα είναι πλέον διαθέσιμη στην περιοχή **εικόνες**.

    ![Επιτυχής καταγραφή εικόνας](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

##<a name="next-steps"></a>Επόμενα βήματα

Η εικόνα είναι έτοιμος να χρησιμοποιηθεί για τη δημιουργία εικονικές μηχανές. Για να το κάνετε αυτό, θα δημιουργήσετε μια εικονική μηχανή χρησιμοποιώντας το στοιχείο μενού **Από τη συλλογή** και επιλέγοντας την εικόνα που μόλις δημιουργήσατε. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή από μια εικόνα](virtual-machines-windows-classic-createportal.md).



[Πώς μπορείτε να εισέλθετε με ένα εικονικό μηχάνημα που εκτελούν τον Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Πώς μπορείτε να χρησιμοποιήσετε το Sysprep: εισαγωγή]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
