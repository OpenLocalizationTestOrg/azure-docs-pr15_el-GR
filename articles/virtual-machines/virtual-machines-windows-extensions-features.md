<properties
 pageTitle="Δυνατότητες και επεκτάσεις εικονική μηχανή | Microsoft Azure"
 description="Μάθετε τι επεκτάσεις είναι διαθέσιμες για Azure εικονικές μηχανές, ομαδοποιημένα κατά τι να παρέχουν ή να βελτιώσετε."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/30/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Σχετικά με τις δυνατότητες και επεκτάσεις εικονική μηχανή

## <a name="azure-vm-extensions"></a>Επεκτάσεις Εικονική Azure

Azure επεκτάσεις εικονική μηχανή είναι μικρές εφαρμογές που παρέχουν ρύθμισης παραμέτρων και την αυτοματοποίηση εργασιών ανάπτυξης δημοσίευση σε εικονικές μηχανές Windows Azure. Για παράδειγμα, εάν μια εικονική μηχανή απαιτεί λογισμικό που είναι εγκατεστημένο, προστασίας από ιούς προστασίας ή ρύθμισης παραμέτρων Docker, μια επέκταση Εικονική μπορεί να χρησιμοποιηθεί για την ολοκλήρωση αυτών των εργασιών. Azure Εικονική επεκτάσεις μπορούν να εκτελεστούν χρησιμοποιώντας το Azure CLI, PowerShell, διαχείριση πόρων πρότυπα και την πύλη του Azure. Επεκτάσεις μπορεί να μαζί με μια νέα ανάπτυξη εικονική μηχανή ή να εκτελεστεί σε οποιαδήποτε υπάρχουσα συστήματος.

Αυτό το έγγραφο παρέχει τις προϋποθέσεις για την επέκταση Azure εικονική μηχανή και καθοδήγηση σχετικά με τον τρόπο για να εντοπίσετε διαθέσιμες Εικονική επεκτάσεις. 

## <a name="azure-vm-agent"></a>Παράγοντας Εικονική Azure

Ο παράγοντας Εικονική Azure διαχειρίζεται επικοινωνία μεταξύ μια εικονική μηχανή Azure και του ελεγκτή ύφασμα Azure. Ο παράγοντας Εικονική είναι υπεύθυνο για πολλές λειτουργική πτυχές της ανάπτυξης και τη Διαχείριση εικονικές μηχανές Windows Azure, συμπεριλαμβανομένης της εκτέλεσης Εικονική επεκτάσεις. Ο παράγοντας Εικονική Azure είναι προεγκατεστημένα στα Azure συλλογή εικόνων και μπορεί να εγκατασταθεί σε λειτουργικά συστήματα που υποστηρίζονται. 

Για πληροφορίες σχετικά με υποστηριζόμενα λειτουργικά συστήματα και οδηγίες εγκατάστασης, ανατρέξτε στο θέμα [Παράγοντας εικονική μηχανή Azure](./virtual-machines-windows-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Ανακαλύψτε Εικονική επεκτάσεις

Πολλές διαφορετικές επεκτάσεις Εικονική είναι διαθέσιμες για χρήση με εικονικές μηχανές Windows Azure. Για να δείτε μια πλήρη λίστα, εκτελέστε την ακόλουθη εντολή με το Azure CLI, αντικαθιστώντας τη θέση με τη θέση της επιλογής.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>Κοινές επεκτάσεις Εικονική

|Όνομα επέκτασης   |Περιγραφή   |Περισσότερες πληροφορίες   |
|---|---|---|
|Επέκταση προσαρμοσμένη δέσμη ενεργειών για Windows  | Εκτελούν δέσμες ενεργειών σε σχέση με μια Azure εικονική μηχανή  |[Επέκταση προσαρμοσμένη δέσμη ενεργειών για Windows](./virtual-machines-windows-extensions-customscript.md)   |
|Επέκταση DSC για Windows | Επέκταση του PowerShell DSC (επιθυμητή κατάσταση ρύθμιση παραμέτρων).  | [Επέκταση Εικονική docker](./virtual-machines-windows-extensions-dsc-overview.md)  |
|Επέκταση Διαγνωστικά του Azure | Διαχείριση Διαγνωστικά του Azure |[Επέκταση Διαγνωστικά του Azure](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
