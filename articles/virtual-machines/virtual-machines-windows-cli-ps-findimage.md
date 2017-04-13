<properties
   pageTitle="Περιήγηση και επιλογή εικόνων Εικονική Windows | Microsoft Azure"
   description="Μάθετε πώς να καθορίσετε το publisher προσφορά και SKU για εικόνες κατά τη δημιουργία μια εικονική μηχανή Windows με το μοντέλο ανάπτυξης διαχείρισης πόρων."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell-or-the-cli"></a>Περιήγηση και επιλογή Windows εικονική μηχανή εικόνων στο Azure με το PowerShell ή το CLI

Αυτό το θέμα περιγράφει πώς μπορείτε να βρείτε Εικονική εικόνα εκδότες, προσφορές, τα SKU και εκδόσεις για κάθε θέση στην οποία μπορεί να αναπτύξετε. Για να δώσετε ένα παράδειγμα, ορισμένες εικόνες που χρησιμοποιούνται συχνά Εικονική Windows είναι οι εξής:

## <a name="table-of-commonly-used-windows-images"></a>Πίνακας που χρησιμοποιείται συχνά εικόνες των Windows


| PublisherName                        | Προσφορά                                 | SKU                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| MicrosoftDynamicsNAV             | DynamicsNAV                                | 2015                             |
| MicrosoftSharePoint              | MicrosoftSharePointServer                  | 2013                             |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Για μεγάλες επιχειρήσεις-βελτιστοποιημένη-για-DW      |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Για μεγάλες επιχειρήσεις-βελτιστοποιημένη-για-Συναλλαγής    |
| MicrosoftWindowsServer           | WindowsServer                              | Κέντρο δεδομένων R2 2012                  |
| MicrosoftWindowsServer           | WindowsServer                              | 2012-κέντρο δεδομένων               |
| MicrosoftWindowsServer           | WindowsServer                              | 2008 R2 SP1 |
| MicrosoftWindowsServer           | WindowsServer                              | Windows Server-Technical Preview |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials                    | WindowsServerEssentials          |
| MicrosoftWindowsServerHPCPack    | WindowsServerHPCPack                       | 2012R2                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
