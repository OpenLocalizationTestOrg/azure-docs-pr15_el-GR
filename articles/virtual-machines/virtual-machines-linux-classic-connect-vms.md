<properties
    pageTitle="Σύνδεση ΣΠΣ Linux σε μια υπηρεσία cloud | Microsoft Azure"
    description="Σύνδεση Linux εικονικές μηχανές που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης για μια υπηρεσία Azure cloud ή εικονικού δικτύου."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="connect-linux-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Σύνδεση Linux εικονικές μηχανές που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης με μια υπηρεσία εικονικού δικτύου ή στο cloud

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Linux εικονικές μηχανές που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης τοποθετούνται πάντα σε μια υπηρεσία στο cloud. Η υπηρεσία cloud λειτουργεί ως ένα κοντέινερ και παρέχει ένα μοναδικό όνομα δημόσια DNS, μια δημόσια διεύθυνση IP και ένα σύνολο τελικά σημεία για να αποκτήσετε πρόσβαση στην εικονική μηχανή μέσω του Internet. Η υπηρεσία cloud μπορεί να είναι σε ένα εικονικό δίκτυο, αλλά που δεν απαιτείται. Μπορείτε επίσης να [συνδέσετε εικονικές μηχανές Windows με μια υπηρεσία εικονικού δικτύου ή στο cloud](virtual-machines-windows-classic-connect-vms.md).

Εάν μια υπηρεσία cloud, δεν είναι σε ένα εικονικό δίκτυο, ονομάζεται μια *μεμονωμένη* υπηρεσία στο cloud. Τις εικονικές μηχανές σε μια υπηρεσία cloud μεμονωμένη μόνο να μπορούν να επικοινωνούν με άλλες εικονικές μηχανές χρησιμοποιώντας τις άλλες εικονικές μηχανές των δημόσιων ονόματα DNS και την κυκλοφορία διανύει μέσω του Internet. Εάν μια υπηρεσία cloud είναι σε ένα εικονικό δίκτυο, τις εικονικές μηχανές σε αυτήν την υπηρεσία cloud μπορούν να επικοινωνούν όλες οι άλλες εικονικές μηχανές στο εικονικό δίκτυο χωρίς να το στείλετε οποιαδήποτε κυκλοφορία μέσω του Internet.

Εάν τοποθετήσετε το εικονικές μηχανές στην ίδια μεμονωμένη υπηρεσία cloud, μπορείτε να χρησιμοποιήσετε εξισορρόπηση φόρτου και διαθεσιμότητα σύνολα. Για λεπτομέρειες, ανατρέξτε στο θέμα [εικονικές μηχανές εξισορρόπηση φόρτου](virtual-machines-linux-load-balance.md) και [Διαχείριση τη διαθεσιμότητα των εικονικές μηχανές](virtual-machines-linux-manage-availability.md). Ωστόσο, δεν μπορείτε να οργανώσετε τις εικονικές μηχανές σε δευτερεύοντα δίκτυα ή σύνδεση μια μεμονωμένη υπηρεσία στο cloud με το δίκτυο εσωτερικής εγκατάστασης. Ακολουθεί ένα παράδειγμα:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Αφού δημιουργήσετε μια εικονική μηχανή, είναι καλή ιδέα να [προσθέσετε ένα δίσκο δεδομένων](virtual-machines-linux-classic-attach-disk.md) ώστε να μην χρειάζεται μια θέση για την αποθήκευση δεδομένων υπηρεσιών σας και την φόρτους εργασίας. 



