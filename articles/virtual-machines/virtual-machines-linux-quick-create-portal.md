<properties
    pageTitle="Δημιουργήστε μια Εικονική Linux με την πύλη Azure | Microsoft Azure"
    description="Δημιουργήστε μια Εικονική Linux με την πύλη Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Δημιουργήστε μια Εικονική Linux στην Azure χρησιμοποιώντας την πύλη


Σε αυτό το άρθρο σάς δείχνει πώς μπορείτε να χρησιμοποιήσετε την [πύλη του Azure](https://portal.azure.com/) για να δημιουργήσετε μια εικονική μηχανή Linux.

Οι απαιτήσεις είναι:

- [Azure λογαριασμού](https://azure.microsoft.com/pricing/free-trial/)

- [SSH δημόσια και ιδιωτικά αρχεία κλειδιών](virtual-machines-linux-mac-create-ssh-keys.md)


1. Συνδεθείτε στην πύλη του Azure με την ταυτότητά σας λογαριασμός Azure, κάντε κλικ στο κουμπί **+ Δημιουργία** στην επάνω αριστερή γωνία:

    ![screen1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. Κάντε κλικ στην επιλογή **εικονικές μηχανές** στην **Marketplace** , στη συνέχεια, λίστα εικόνων **Ubuntu διακομιστή 14.04 Αποτελεσμάτων** από τις **Προβεβλημένες εφαρμογές** .  Επαλήθευση στο κάτω μέρος του ότι το μοντέλο ανάπτυξης είναι `Resource Manager` και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

    ![screen2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. Στη σελίδα **βασικά στοιχεία** , πληκτρολογήστε:
    - ένα όνομα για την εικονική Μηχανή
    - ένα όνομα χρήστη για το χρήστη του διαχειριστή
    - ο τύπος ελέγχου ταυτότητας που έχει οριστεί σε **δημόσιο κλειδί SSH**
    - το δημόσιο κλειδί SSH ως συμβολοσειρά (από το `~/.ssh/` καταλόγου)
    - ένας πόρος ομαδοποίηση όνομα ή επιλέξτε μια υπάρχουσα ομάδα

    και κάντε κλικ στο **κουμπί OK** για να συνεχίσετε και να επιλέξετε το μέγεθος του Εικονική. Αυτό θα μπορούσε να έχει ως εξής:

    ![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. Επιλέξτε το μέγεθος **DS1** , το οποίο εγκαθιστά Ubuntu σε μια SSD Premium, και κάντε κλικ στην **επιλογή** για να ρυθμίσετε τις παραμέτρους.

    ![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. Στις **Ρυθμίσεις**, αφήστε τις προεπιλογές για τιμές χώρου αποθήκευσης και δικτύου και κάντε κλικ στο **κουμπί OK** για να προβάλετε τη σύνοψη.  Ειδοποίηση τον τύπο του δίσκου έχει οριστεί σε Premium SSD, επιλέγοντας DS1, το **S** notates SSD.

    ![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Επιβεβαιώστε τις ρυθμίσεις για το νέο Εικονική Ubuntu, και κάντε κλικ στο κουμπί **OK**.

    ![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Ανοίξτε τον πίνακα εργαλείων πύλη και στο **διασυνδέσεις δικτύου** , επιλέξτε το NIC

    ![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. Άνοιγμα του μενού διευθύνσεις IP δημόσια στις ρυθμίσεις του NIC

    ![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. SSH σε τη δημόσια IP χρησιμοποιώντας το δημόσιο κλειδί SSH

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει μια Εικονική Linux γρήγορα για να χρησιμοποιήσετε για σκοπούς δοκιμής ή επίδειξης. Για να δημιουργήσετε μια Εικονική Linux προσαρμοσμένο για την υποδομή σας, μπορείτε να παρακολουθείτε οποιαδήποτε από αυτά τα άρθρα.

- [Δημιουργήστε μια Εικονική Linux στην Azure με τη χρήση προτύπων](virtual-machines-linux-cli-deploy-templates.md)
- [Δημιουργία SSH ασφαλές Linux Εικονική μηχανή σε Azure με τη χρήση προτύπων](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Δημιουργήστε μια Εικονική Linux χρησιμοποιώντας το CLI Azure](virtual-machines-linux-create-cli-complete.md)
