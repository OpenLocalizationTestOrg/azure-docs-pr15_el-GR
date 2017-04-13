<properties
   pageTitle="Πώς μπορείτε να προσθέσετε ετικέτες σε μια Εικονική | Microsoft Azure"
   description="Μάθετε περισσότερα σχετικά με την προσθήκη ετικετών σε μια εικονική μηχανή Windows που έχουν δημιουργηθεί με Azure χρησιμοποιώντας το μοντέλο ανάπτυξης για τη διαχείριση πόρων"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Πώς μπορείτε να προσθέσετε ετικέτες σε μια εικονική μηχανή Windows Azure


Σε αυτό το άρθρο περιγράφει διαφορετικούς τρόπους για να προσθέσετε ετικέτα μια εικονική μηχανή Windows στο Azure μέσω του μοντέλου ανάπτυξης διαχείρισης πόρων. Οι ετικέτες είναι ζεύγη που ορίζονται από το χρήστη κλειδιού/τιμής που μπορούν να τοποθετηθούν απευθείας σε έναν πόρο ή μια ομάδα πόρων. Azure υποστηρίζει επί του παρόντος έως 15 ετικέτες ανά πόρο και ομάδα πόρων. Οι ετικέτες μπορεί να είναι τοποθετείται σε έναν πόρο κατά τη δημιουργία του ή να προστεθεί σε έναν υπάρχοντα πόρο. Σημειώστε ότι οι ετικέτες που υποστηρίζονται για τους πόρους που δημιουργήθηκε μέσω μόνο το μοντέλο ανάπτυξης διαχείρισης πόρων. Εάν θέλετε να προσθέσετε ετικέτες σε μια εικονική μηχανή Linux, δείτε [πώς μπορείτε να προσθέσετε ετικέτες σε μια εικονική μηχανή Linux στο Azure](virtual-machines-linux-tag.md).

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Προσθήκη ετικετών με το PowerShell

Για να δημιουργήσετε, προσθήκη και διαγραφή ετικετών μέσω του PowerShell, πρέπει πρώτα να ρυθμίσετε το [περιβάλλον του PowerShell με το διαχειριστή πόρων Azure][]. Αφού ολοκληρώσετε τη ρύθμιση, μπορείτε να τοποθετήσετε ετικέτες σε πόρους υπολογισμού, δικτύου και χώρου αποθήκευσης στο δημιουργίας ή μετά τη δημιουργία του πόρου μέσω του PowerShell. Σε αυτό το άρθρο θα συγκεντρωθείτε σε προβολή/επεξεργασία ετικετών που τοποθετείται σε εικονικές μηχανές.

Αρχικά, μεταβείτε σε μια εικονική μηχανή έως το `Get-AzureRmVM` cmdlet.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Εάν το εικονικό μηχάνημα περιέχει ήδη ετικετών, θα δείτε, στη συνέχεια, όλες τις ετικέτες για τον πόρο:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Εάν θέλετε να προσθέσετε ετικέτες μέσω του PowerShell, μπορείτε να χρησιμοποιήσετε το `Set-AzureRmResource` εντολή. Λάβετε υπόψη κατά την ενημέρωση ετικετών μέσω του PowerShell, ετικέτες ενημερώνονται ως σύνολο. Εάν πρόκειται να προσθέσετε μια ετικέτα σε έναν πόρο που διαθέτει ήδη ετικέτες, θα πρέπει να συμπεριλάβετε όλες τις ετικέτες που θέλετε να τοποθετηθεί στον πόρο. Ακολουθεί ένα παράδειγμα του τρόπου προσθήκης επιπλέον ετικετών σε έναν πόρο με cmdlet του PowerShell.

Αυτό το cmdlet πρώτη ορίζει όλων των ετικετών που τοποθετείται στο *MyTestVM* στη μεταβλητή *$tags* , χρησιμοποιώντας το `Get-AzureRmResource` και `Tags` την ιδιότητα.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

Η δεύτερη εντολή εμφανίζει τις ετικέτες για τη δεδομένη μεταβλητή.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

Η τρίτη εντολή προσθέτει μια επιπλέον ετικέτα στη μεταβλητή *$tags* . Σημειώστε τη χρήση του **+=** για να προσαρτήσετε το νέο ζεύγος κλειδιού/τιμής στη λίστα *$tags* .

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

Η τέταρτη εντολή ορίζει όλες τις ετικέτες που ορίζονται από το στη μεταβλητή *$tags* στον συγκεκριμένο πόρο. Σε αυτήν την περίπτωση, είναι MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Η εντολή πέμπτο εμφανίζει όλες τις ετικέτες στον πόρο. Όπως μπορείτε να δείτε, *θέση* τώρα ορίζεται ως μια ετικέτα με *MyLocation* ως η τιμή.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

Για να μάθετε περισσότερα σχετικά με την προσθήκη ετικετών σε μέσω του PowerShell, δείτε τα [Cmdlet του Azure πόρων][].

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Επόμενα βήματα

* Για να μάθετε περισσότερα σχετικά με την προσθήκη ετικετών σε Azure τους πόρους σας, ανατρέξτε στο θέμα [Επισκόπηση διαχείρισης πόρων Azure][] και [Χρήση ετικετών για να οργανώσετε τους πόρους σας Azure][].
* Για να δείτε πώς οι ετικέτες μπορούν να σας βοηθήσουν να διαχειριστείτε τη χρήση του Azure πόρους, ανατρέξτε στο θέμα [Κατανόηση της χρέωσής σας Azure][] και να [αποκτήσει ιδέες σε σας Microsoft Azure κατανάλωση πόρων][].

[Περιβάλλον PowerShell με το διαχειριστή πόρων Azure]: ../powershell-azure-resource-manager.md
[Cmdlet του Azure πόρων]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Επισκόπηση της διαχείρισης πόρων]: ../azure-resource-manager/resource-group-overview.md
[Χρήση ετικετών για να οργανώσετε τους πόρους σας Azure]: ../resource-group-using-tags.md
[Κατανόηση της χρέωσής σας Azure]: ../billing/billing-understand-your-bill.md
[Ενίσχυση ιδέες σε την κατανάλωση πόρων Microsoft Azure]: ../billing-usage-rate-card-overview.md
