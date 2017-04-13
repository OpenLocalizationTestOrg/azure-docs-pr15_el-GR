<properties
    pageTitle="Εργαλεία Κοινότητας για τη Διαχείριση υπηρεσίας Azure για διαχείριση πόρων Azure μετεγκατάστασης"
    description="Σε αυτό το άρθρο καταχωρεί τα εργαλεία που έχει παραχωρηθεί από την Κοινότητα για να σας βοηθήσει με μετεγκατάστασης IaaS πόρων από τη Διαχείριση υπηρεσιών Azure στη στοίβα Azure διαχείριση πόρων."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="singhkay"/>

# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Εργαλεία Κοινότητας για τη Διαχείριση υπηρεσίας Azure για διαχείριση πόρων Azure μετεγκατάστασης

Σε αυτό το άρθρο καταχωρεί τα εργαλεία που έχει παραχωρηθεί από την Κοινότητα για να σας βοηθήσει με μετεγκατάστασης IaaS πόρων από τη Διαχείριση υπηρεσιών Azure στη στοίβα Azure διαχείριση πόρων.

>[AZURE.NOTE]Αυτά τα εργαλεία δεν υποστηρίζονται επίσημα υποστήριξη της Microsoft. Επομένως, που είναι η προέλευση του στην Github και είμαστε ικανοποιημένοι για αποδοχή PRs επιδιορθώσεις ή πρόσθετα σενάρια. Για να αναφέρετε ένα θέμα, χρησιμοποιήστε τη δυνατότητα θέματα Github.
>
> Μετεγκατάσταση με αυτά τα εργαλεία θα προκαλέσει χρόνου εκτός λειτουργίας για την κλασική εικονική μηχανή. Εάν αναζητάτε πλατφόρμα υποστηρίζεται μετεγκατάστασης, επισκεφθείτε 
>
>- [Μετεγκατάσταση πλατφόρμα υποστηρίζεται IaaS πόρων από κλασική σε στοίβα από διαχειριστή πόρων Azure](./virtual-machines-windows-migration-classic-resource-manager.md)
>- [Τεχνική βαθύ κατάρρευση σε πλατφόρμα υποστηρίζεται μετεγκατάστασης από κλασική διαχείριση πόρων για να Azure](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
>- [Μετεγκατάσταση IaaS πόρους από κλασική να Azure διαχείριση πόρων με χρήση του PowerShell Azure](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## <a name="asm2arm"></a>ASM2ARM

Αυτή είναι μια λειτουργική μονάδα δέσμης ενεργειών του PowerShell για μετεγκατάσταση σας **μία** εικονική μηχανή (Εικονική) από στοίβα Azure υπηρεσία διαχείρισης σε στοίβα από διαχειριστή πόρων Azure. 

[Σύνδεση με την τεκμηρίωση για το εργαλείο](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz

migAz είναι μια επιπλέον επιλογή για να μετεγκαταστήσετε ένα πλήρες σύνολο των πόρων Azure υπηρεσίας διαχείρισης IaaS στο Azure διαχείριση πόρων IaaS πόρους. Η μετεγκατάσταση μπορεί να συμβεί κατά την ίδια συνδρομή ή μεταξύ διάφορες συνδρομές και τύπους συνδρομών (ex: υπηρεσία παροχής Κρυπτογράφησης συνδρομών).

[Σύνδεση με την τεκμηρίωση για το εργαλείο](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)