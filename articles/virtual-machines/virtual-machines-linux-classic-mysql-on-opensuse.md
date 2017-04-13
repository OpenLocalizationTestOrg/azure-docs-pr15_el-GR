<properties
    pageTitle="Εγκατάσταση MySQL σε μια Εικονική OpenSUSE | Microsoft Azure"
    description="Μάθετε πώς να εγκαταστήσετε MySQL σε έναν υπολογιστή OpenSUSE Linux VMirtual στο Azure."
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
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Εγκατάσταση MySQL σε μια εικονική μηχανή εκτελείται OpenSUSE Linux στο Azure

[MySQL] [ MySQL] είναι μια βάση δεδομένων SQL δημοφιλή, Άνοιγμα προέλευσης. Αυτό το πρόγραμμα εκμάθησης θα μάθετε πώς να δημιουργήσετε μια εικονική μηχανή εκτελείται OpenSUSE Linux και, στη συνέχεια, εγκαταστήστε MySQL.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


<br>


## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Δημιουργήστε μια εικονική μηχανή εκτελείται OpenSUSE Linux

[AZURE.INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Εγκατάσταση και εκτέλεση MySQL στον υπολογιστή εικονικές

[AZURE.INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Επόμενα βήματα
Για λεπτομέρειες σχετικά με MySQL, ανατρέξτε στην [Τεκμηρίωση MySQL][MySQLDocs].

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com

