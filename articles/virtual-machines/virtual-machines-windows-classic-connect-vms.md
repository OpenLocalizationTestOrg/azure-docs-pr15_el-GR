<properties
    pageTitle="Σύνδεση Windows ΣΠΣ σε μια υπηρεσία cloud | Microsoft Azure"
    description="Σύνδεση εικονικές μηχανές Windows που δημιουργήθηκε με το μοντέλο κλασική ανάπτυξης σε μια υπηρεσία Azure cloud ή εικονικού δικτύου."
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
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Σύνδεση εικονικές μηχανές Windows που δημιουργήθηκε με το μοντέλο κλασική ανάπτυξης με μια υπηρεσία εικονικού δικτύου ή στο cloud

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Εικονικές μηχανές Windows που δημιουργήθηκε με το μοντέλο κλασική ανάπτυξης τοποθετούνται πάντα σε μια υπηρεσία στο cloud. Η υπηρεσία cloud λειτουργεί ως ένα κοντέινερ και παρέχει ένα μοναδικό όνομα δημόσια DNS, μια δημόσια διεύθυνση IP και ένα σύνολο τελικά σημεία για να αποκτήσετε πρόσβαση στην εικονική μηχανή μέσω του Internet. Η υπηρεσία cloud μπορεί να είναι σε ένα εικονικό δίκτυο, αλλά που δεν απαιτείται. Μπορείτε επίσης να [συνδέσετε Linux εικονικές μηχανές με μια υπηρεσία εικονικού δικτύου ή στο cloud](virtual-machines-linux-classic-connect-vms.md).

Εάν μια υπηρεσία cloud, δεν είναι σε ένα εικονικό δίκτυο, ονομάζεται μια *μεμονωμένη* υπηρεσία στο cloud. Τις εικονικές μηχανές σε μια υπηρεσία cloud μεμονωμένη μόνο να μπορούν να επικοινωνούν με άλλες εικονικές μηχανές χρησιμοποιώντας τις άλλες εικονικές μηχανές των δημόσιων ονόματα DNS και την κυκλοφορία διανύει μέσω του Internet. Εάν μια υπηρεσία cloud είναι σε ένα εικονικό δίκτυο, τις εικονικές μηχανές σε αυτήν την υπηρεσία cloud μπορούν να επικοινωνούν όλες οι άλλες εικονικές μηχανές στο εικονικό δίκτυο χωρίς να το στείλετε οποιαδήποτε κυκλοφορία μέσω του Internet.

Εάν τοποθετήσετε το εικονικές μηχανές στην ίδια μεμονωμένη υπηρεσία cloud, μπορείτε να χρησιμοποιήσετε εξισορρόπηση φόρτου και διαθεσιμότητα σύνολα. Για λεπτομέρειες, ανατρέξτε στο θέμα [εικονικές μηχανές εξισορρόπηση φόρτου](virtual-machines-windows-load-balance.md) και [Διαχείριση τη διαθεσιμότητα των εικονικές μηχανές](virtual-machines-windows-manage-availability.md). Ωστόσο, δεν μπορείτε να οργανώσετε τις εικονικές μηχανές σε δευτερεύοντα δίκτυα ή σύνδεση μια μεμονωμένη υπηρεσία στο cloud με το δίκτυο εσωτερικής εγκατάστασης. Ακολουθεί ένα παράδειγμα:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Αφού δημιουργήσετε μια εικονική μηχανή, είναι καλή ιδέα να [προσθέσετε ένα δίσκο δεδομένων](virtual-machines-windows-classic-attach-disk.md) ώστε να μην χρειάζεται μια θέση για την αποθήκευση δεδομένων υπηρεσιών σας και την φόρτους εργασίας. 