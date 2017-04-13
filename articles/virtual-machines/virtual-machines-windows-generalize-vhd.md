<properties
    pageTitle="Γενίκευση Windows VHD | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε το Sysprep γενίκευσης μια Εικονική των Windows για να χρησιμοποιήσετε με το μοντέλο ανάπτυξης διαχείρισης πόρων."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
    
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Γενίκευση μια εικονική μηχανή Windows χρησιμοποιώντας το Sysprep

Αυτή η ενότητα σας δείχνει τον τρόπο γενίκευσης σας Windows εικονικό μηχάνημα για χρήση ως εικόνα. Sysprep καταργεί όλα προσωπικών πληροφοριών του λογαριασμού σας, μεταξύ άλλων, και προετοιμάζει τον υπολογιστή θα χρησιμοποιηθεί ως εικόνα. Για λεπτομέρειες σχετικά με το Sysprep, ανατρέξτε στο θέμα [τον τρόπο χρήσης Sysprep: μια εισαγωγή](http://technet.microsoft.com/library/bb457073.aspx).

Βεβαιωθείτε ότι τους ρόλους διακομιστή που εκτελούνται στον υπολογιστή που υποστηρίζονται από το Sysprep. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Υποστήριξη Sysprep για τους ρόλους διακομιστή](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

>[AZURE.IMPORTANT] Εάν εκτελείτε Sysprep πριν από την αποστολή του VHD σε Azure για πρώτη φορά, βεβαιωθείτε ότι έχετε επιλέξει [το Εικονική προετοιμασμένοι](virtual-machines-windows-prepare-for-upload-vhd-image.md) πριν από την εκτέλεση του Sysprep. 

1. Πραγματοποιήστε είσοδο στο η εικονική μηχανή των Windows.

2. Ανοίξτε το παράθυρο γραμμής εντολών ως διαχειριστής. Αλλάξτε τον κατάλογο με **%windir%\system32\sysprep**και, στη συνέχεια, εκτελέστε `sysprep.exe`.

3. Στο παράθυρο διαλόγου **Εργαλείο προετοιμασίας συστήματος** , επιλέξτε **Εισαγωγή συστήματος της πρώτης εμπειρία (OOBE)**, και βεβαιωθείτε ότι είναι επιλεγμένο το πλαίσιο ελέγχου **Generalize** .

4. Στις **Επιλογές τερματισμού**, επιλέξτε **τερματισμού**.

5. Κάντε κλικ στο **κουμπί OK**.

    ![Έναρξη Sysprep](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

6. Όταν ολοκληρωθεί το Sysprep, τερματίζεται η εικονική μηχανή. 

## <a name="next-steps"></a>Επόμενα βήματα

- Εάν η Εικονική είναι εσωτερικής εγκατάστασης, μπορείτε να κάνετε άμεση [Αποστολή VHD να Azure](virtual-machines-windows-upload-image.md).
- Εάν η Εικονική είναι ήδη στο Azure, μπορείτε να πλέον να [δημιουργήσετε μια εικόνα από το γενικευμένη Εικονική](virtual-machines-windows-capture-image.md).