<properties
   pageTitle="Ρυθμίστε το περιβάλλον ανάπτυξης στο Mac OS X | Microsoft Azure"
   description="Εγκαταστήστε το περιβάλλον εκτέλεσης, SDK και εργαλεία και δημιουργήστε ένα σύμπλεγμα τοπικής ανάπτυξης. Αφού ολοκληρώσετε αυτό το πρόγραμμα εγκατάστασης, θα είστε έτοιμοι να δημιουργήσετε εφαρμογές στο Mac OS X."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# <a name="set-up-your-development-environment-on-mac-os-x"></a>Ρυθμίστε το περιβάλλον ανάπτυξης στο Mac OS X

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

Μπορείτε να δημιουργήσετε εφαρμογές υπηρεσίας ύφασμα να εκτελέσετε στο Linux συμπλεγμάτων χρησιμοποιώντας Mac OS X. Σε αυτό το άρθρο περιγράφει πώς μπορείτε να ρυθμίσετε το Mac σας για ανάπτυξη.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Υπηρεσία ύφασμα δεν εκτελείται τοπικά στο λειτουργικό σύστημα OS X. Για να εκτελέσετε ένα τοπικό σύμπλεγμα ύφασμα υπηρεσίας, παρέχουμε ένα προκαθορισμένο Ubuntu εικονικό μηχάνημα με Vagrant και VirtualBox. Πριν ξεκινήσετε, πρέπει:

- [Vagrant (v1.8.4 ή νεότερη έκδοση)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>Δημιουργήστε την τοπική εικονική Μηχανή

Για να δημιουργήσετε την τοπική εικονική Μηχανή που περιέχει ένα σύμπλεγμα ύφασμα υπηρεσίας 5 κόμβου, κάντε τα εξής:

1. Δημιουργία διπλότυπου το repo Vagrantfile

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Μεταβείτε στο την τοπική κλωνοποίηση από το repo

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. (Προαιρετικό) Τροποποίηση των προεπιλεγμένων ρυθμίσεων Εικονική

    Από προεπιλογή, τα τοπικά Εικονική έχει ρυθμιστεί ως εξής:

    - 3 GB μνήμης που έχει εκχωρηθεί
    - Ιδιωτικό κεντρικού υπολογιστή δικτύου που έχει ρυθμιστεί στο IP 192.168.50.50 Ενεργοποίηση διέλευση κίνησης από τον κεντρικό υπολογιστή Mac

    Μπορείτε να αλλάξετε οποιαδήποτε από αυτές τις ρυθμίσεις ή να προσθέσετε άλλες ρύθμισης παραμέτρων για την Εικονική στα το Vagrantfile. Ανατρέξτε στην [τεκμηρίωση Vagrant](http://www.vagrantup.com/docs) για την πλήρη λίστα των επιλογών ρύθμισης παραμέτρων.

4. Δημιουργήστε την εικονική Μηχανή

    ```bash
    vagrant up
    ```

    Αυτό το βήμα κάνει λήψη της προδιαμορφωμένο Εικονική εικόνας, εκκίνησης του τοπικά και, στη συνέχεια, ρύθμιση μιας τοπικής ύφασμα υπηρεσίας συμπλέγματος σε αυτό. Θα πρέπει να αναμένετε να χρειαστούν μερικά λεπτά. Εάν το πρόγραμμα εγκατάστασης ολοκληρωθεί με επιτυχία, θα δείτε ένα μήνυμα στη λίστα εξόδου που υποδεικνύει ότι εκκινείται το σύμπλεγμα.

    ![Εκκίνηση της εγκατάστασης μετά την προμήθεια του Εικονική στο σύμπλεγμα][cluster-setup-script]

5. Δοκιμή που το σύμπλεγμα έχει οριστεί σωστά, μεταβαίνοντας Explorer ύφασμα υπηρεσίας στο http://192.168.50.50:19080/Explorer (αν υποθέσουμε ότι διατηρούνται το προεπιλεγμένο IP ιδιωτικού δικτύου).

    ![Προβολή Εξερεύνησης ύφασμα υπηρεσίας από τον κεντρικό υπολογιστή Mac][sfx-mac]


## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Εγκατάσταση της προσθήκης υπηρεσίας ύφασμα για νέον Έκλειψη (προαιρετικά)

Υπηρεσία ύφασμα παρέχει μια προσθήκη για IDE νέον Έκλειψη που μπορεί να απλοποιήσει της διαδικασίας δημιουργία και ανάπτυξη Java υπηρεσίες.

1. Στο Έκλειψη, βεβαιωθείτε ότι έχετε Buildship έκδοση 1.0.17 ή νεότερη έκδοση εγκατεστημένο. Μπορείτε να ελέγξετε τις εκδόσεις των εγκατεστημένων στοιχείων, επιλέγοντας **Βοήθεια > λεπτομέρειες εγκατάστασης**. Μπορείτε να ενημερώσετε Buildship ακολουθώντας τις οδηγίες [εδώ][buildship-update].

2. Για να εγκαταστήσετε την προσθήκη ύφασμα υπηρεσίας, επιλέξτε **Βοήθεια > εγκατάσταση νέου λογισμικού...**

3. Στο πλαίσιο κειμένου "Δυνατότητα εργασίας με", πληκτρολογήστε: http://dl.windowsazure.com/eclipse/servicefabric.

4. Κάντε κλικ στην επιλογή Προσθήκη.

    ![Προσθήκη νέον Έκλειψη για την υπηρεσία ύφασμα][sf-eclipse-plugin-install]

5. Επιλέξτε την προσθήκη ύφασμα υπηρεσίας και κάντε κλικ στο κουμπί Επόμενο.

6. Συνεχίστε με την εγκατάσταση και αποδεχτείτε την άδεια χρήσης τελικού χρήστη.

## <a name="next-steps"></a>Επόμενα βήματα

- [Δημιουργία της πρώτης εφαρμογής υπηρεσίας ύφασμα για Linux](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

- [Δημιουργήστε ένα σύμπλεγμα ύφασμα υπηρεσίας στην πύλη του Azure](service-fabric-cluster-creation-via-portal.md)
- [Δημιουργήστε ένα σύμπλεγμα ύφασμα υπηρεσία χρησιμοποιώντας τη διαχείριση πόρων Azure](service-fabric-cluster-creation-via-arm.md)
- [Κατανόηση του μοντέλου εφαρμογής υπηρεσίας ύφασμα](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
