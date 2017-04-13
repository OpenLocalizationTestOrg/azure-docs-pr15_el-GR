<properties
    pageTitle="Εργαλεία και PaaS υπηρεσίες για στοίβας Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ξεκινήσετε με τις υπηρεσίες PaaS σε στοίβα Azure."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Εργαλεία για τη στοίβα Azure

Μπορείτε να κάνετε λήψη των εργαλείων που περιγράφεται παρακάτω. Εάν θέλετε να ειδοποιείστε μετά νέων υπηρεσιών και εργαλεία, ακολουθήστε #AzureStack στο Twitter.

## <a name="template-tools"></a>Εργαλεία προτύπου

### <a name="azure-stack-github-templates"></a>Azure πρότυπα Github στοίβας
Εξερευνήστε τη αναπτυσσόμενη συλλογή [Προτύπων GitHub στοίβας Azure](https://github.com/Azure/AzureStack-QuickStart-Templates). Όπως ακριβώς [Azure](https://github.com/Azure/azure-quickstart-templates), αυτά τα πρότυπα "Γρήγορη εκκίνηση" Διαχείριση πόρων Azure στόχος για να ξεκινήσετε με απλή μπλοκ δόμησης και παραδείγματα, είστε έτοιμοι να αναπτύξετε στο Microsoft Azure στοίβας Technical Preview απόδειξη της έννοια περιβάλλον. Περιλαμβάνονται πρώτη παραδείγματα φόρτο εργασίας κατασκευαστή για την [ad-μη-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha), [sql-2014-μη-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha), [sharepoint-2013-μη-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha), καθώς επίσης και ως αρκετά πρότυπα απλό 101 όπως [101-απλών-windows-εικονική](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm).


### <a name="marketplace-item-packaging-tool-and-sample"></a>Αγορά του στοιχείου συσκευασία εργαλείο και το δείγμα
[Λήψη και χρήση του εργαλείου συσκευασία](http://www.aka.ms/azurestackmarketplaceitem) για να δημιουργήσετε στοιχεία marketplace για τα δικά σας προσαρμοσμένα πρότυπα για να προσθέσετε το στοίβας Azure marketplace. Μπορείτε να βρείτε οδηγίες σχετικά με τον τρόπο δημιουργίας ενός στοιχείου marketplace και να είναι διαθέσιμο σε μισθωτές σας στο [στοιχείο δημιουργία Marketplace](azure-stack-create-and-publish-marketplace-item.md).

## <a name="developer-tools"></a>Εργαλεία για προγραμματιστές


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>Χρήση του Visual Studio και TP2 στοίβας Azure στον υπολογιστή εικονικές κατά ΜΆΖΑ CON01
Εάν θέλετε να χρησιμοποιήσετε Visual Studio στην κονσόλα Εικονική για να εργαστείτε με τα πρότυπα στοίβας Azure, πρέπει να εγκαταστήσετε τις σωστές εκδόσεις των τα απαιτούμενα εργαλεία. Χρησιμοποιήστε την ακόλουθη διαδικασία για να εγκαταστήσετε τις υποστηριζόμενες εκδόσεις για TP2.

1. Χρησιμοποιήστε σύνδεση απομακρυσμένης επιφάνειας εργασίας για να συνδεθείτε με την εικονική μηχανή κατά ΜΆΖΑ CON01 με τα διαπιστευτήρια azurestack\azurestackadmin.
2. Εγκατάσταση και ανοίξτε το πρόγραμμα εγκατάστασης πλατφόρμας Web.
3. Εύρεση και εγκατάσταση του **Visual Studio Κοινότητας 2015 με το Microsoft Azure SDK - 2.9.5**.
4. Χρήση **Προσθαφαίρεση προγραμμάτων**, καταργήστε την εγκατάσταση του **Microsoft Azure PowerShell (Σεπτεμβρίου)** που λαμβάνει εγκατασταθεί ως μέρος του SDK.
5. Ανοίξτε το πρόγραμμα εγκατάστασης πλατφόρμας Web.
6. Εύρεση και εγκατάσταση του **Microsoft Azure PowerShell - Azure στοίβας Technical Preview 2**. 
7. Επανεκκινήστε την εικονική μηχανή κατά ΜΆΖΑ CON01.
8. Χρησιμοποιήστε σύνδεση απομακρυσμένης επιφάνειας εργασίας για να συνδεθείτε με την εικονική μηχανή κατά ΜΆΖΑ CON01 με τα διαπιστευτήρια azurestack\azurestackadmin.
9. Ανοίξτε το Visual Studio και επαλήθευση ότι μπορείτε να συνδεθείτε στο περιβάλλον στοίβας Azure, να λήψη προτύπων και ούτω καθεξής. 

### <a name="azure-powershell-sdk"></a>Azure PowerShell SDK
Azure PowerShell είναι μια λειτουργική μονάδα που παρέχει το cmdlet για τη Διαχείριση Azure και Azure στοίβα με το Windows PowerShell. Μπορείτε να χρησιμοποιήσετε τα cmdlet για να δημιουργήσετε, να δοκιμάσετε, ανάπτυξη και διαχείριση λύσεων και τις υπηρεσίες που παρέχονται μέσω της πλατφόρμας Azure στοίβας.
[Κάντε λήψη του SDK PowerShell Azure](http://aka.ms/azStackPsh) και [εγκατάσταση του PowerShell](azure-stack-connect-powershell.md).

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure διασταύρωσης διασυνδέσεων γραμμής εντολών πλατφόρμα
Εγκαταστήστε γρήγορα το περιβάλλον γραμμής εντολών Azure (Azure CLI) για να χρησιμοποιήσετε ένα σύνολο εντολών βάσει κελύφους Άνοιγμα προέλευσης για τη δημιουργία και τη διαχείριση των πόρων σε στοίβα Microsoft Azure.

[Κάντε λήψη του Windows CLI](http://aka.ms/azstack-windows-cli)

[Κάντε λήψη του Mac CLI](http://aka.ms/azstack-linux-cli)

[Κάντε λήψη του CLI Linux](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + Εάν είστε σε υπολογιστή Mac ή Linux, μπορείτε επίσης να λάβετε το CLI, χρησιμοποιώντας την εντολή`npm install -g azure-cli@0.9.11`</br>
> + Εάν λαμβάνετε θεμάτων επικύρωσης πιστοποιητικού, εκτελέστε την εντολή`set NODE_TLS_REJECT_UNAUTHORIZED=0`
