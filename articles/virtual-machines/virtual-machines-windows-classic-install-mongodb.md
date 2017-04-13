<properties
    pageTitle="Εγκατάσταση MongoDB σε Windows Εικονική | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να εγκαταστήσετε το MongoDB σε μια Εικονική Azure που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης που εκτελούν τον Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

#<a name="install-mongodb-on-a-windows-vm"></a>Εγκατάσταση MongoDB σε Windows Εικονική

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Για να εγκαταστήσετε και να ρυθμίσετε MongoDB χρησιμοποιώντας το μοντέλο ανάπτυξης για τη διαχείριση πόρων, ανατρέξτε [σε αυτό το άρθρο](virtual-machines-windows-classic-install-mongodb.md).

[MongoDB] [ MongoDB] είναι μια δημοφιλή ανοιχτού κώδικα, υψηλών επιδόσεων NoSQL βάση δεδομένων. Σε αυτό το άρθρο σάς καθοδηγεί τη δημιουργία ενός Windows Server εικονική μηχανή (Εικονική) με την [πύλη του Azure κλασική][AzurePortal]. Μπορείτε, στη συνέχεια, δημιουργία και να επισυνάψετε ένα δίσκο δεδομένων για να την εικονική Μηχανή πριν από την εγκατάσταση και ρύθμιση παραμέτρων MongoDB. Εάν έχετε μια υπάρχουσα Εικονική στο Azure που θέλετε να χρησιμοποιήσετε, μπορείτε να μεταβείτε κατευθείαν στην [εγκατάσταση και ρύθμιση παραμέτρων MongoDB](#install-and-run-mongodb-on-the-virtual-machine).


## <a name="create-a-virtual-machine-running-windows-server"></a>Δημιουργήστε μια εικονική μηχανή εκτελούν τον Windows Server

Ακολουθήστε αυτές τις οδηγίες για να δημιουργήσετε μια εικονική μηχανή.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] Μπορείτε να προσθέσετε ένα τελικό σημείο για MongoDB κατά τη δημιουργία η εικονική μηχανή και ρυθμίστε τις παραμέτρους του ως εξής: ονομάστε το ως **Mongo**, χρησιμοποιήστε **TCP** ως το πρωτόκολλο και ορίστε τις δημόσιες και ιδιωτικές θύρες σε **27017**.

## <a name="attach-a-data-disk"></a>Επισυνάψτε ένα δίσκο δεδομένων
Για την παροχή χώρου αποθήκευσης η εικονική μηχανή, επισυνάψτε ένα δίσκο δεδομένων και, στη συνέχεια, να το προετοιμάσετε, έτσι ώστε τα Windows μπορούν να χρησιμοποιήσουν. Εάν έχετε ήδη ένα δίσκο δεδομένων, μπορείτε να συνδέσετε αυτόν τον υπάρχοντα δίσκο ή μπορείτε να επισυνάψετε ένα κενό δίσκο.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

Για οδηγίες σχετικά με την προετοιμασία του δίσκου, ανατρέξτε στην ενότητα "Πώς να: προετοιμασία ενός νέου δίσκου δεδομένων στον Windows Server" Πώς [να επισυνάψετε ένα δίσκο δεδομένων σε μια εικονική μηχανή των Windows](virtual-machines-windows-classic-attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Εγκατάσταση και εκτέλεση MongoDB στον υπολογιστή εικονικές

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Σύνοψη
Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς να δημιουργήσετε μια εικονική μηχανή εκτελούν τον Windows Server, απομακρυσμένα σύνδεση σε αυτό και επισυνάψτε ένα δίσκο δεδομένων.  Μάθατε επίσης πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους MongoDB στον υπολογιστή εικονικές βασίζεται στα Windows. Μπορείτε τώρα να αποκτήσετε πρόσβαση MongoDB στον υπολογιστή εικονικές βασίζεται στα Windows, ακολουθώντας τα θέματα για προχωρημένους στην [τεκμηρίωση MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com
