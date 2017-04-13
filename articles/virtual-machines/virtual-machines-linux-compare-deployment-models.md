<properties
   pageTitle="Υπηρεσίες παροχής υπολογισμού, δικτύου και αποθήκευσης | Microsoft Azure"
   description="Επισκόπηση του υπολογισμού, δικτύου και υπηρεσίες παροχής πόρων αποθήκευσης (Προγ NRP και SRP) για εφαρμογές Linux στο μοντέλο ανάπτυξης διαχείρισης πόρων Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/19/2015"
   ms.author="tomfitz"/>

# <a name="azure-compute-network-and-storage-providers-for-linux-applications-under-azure-resource-manager-deployment-model"></a>Azure παροχής υπολογισμού, δικτύου και χώρου αποθήκευσης για τις εφαρμογές του Linux στην περιοχή Διαχείριση πόρων Azure μοντέλο ανάπτυξης

Η συμπερίληψη δυνατότητες υπολογισμού, δικτύου και χώρου αποθήκευσης με το μοντέλο ανάπτυξης διαχείρισης πόρων Azure ουσιαστικά θα απλοποιήσει την ανάπτυξη και τη διαχείριση της σύνθετες εφαρμογές που εκτελούνται σε IaaS. Πολλές εφαρμογές απαιτούν ένα συνδυασμό των πόρων, όπως ένα εικονικό δίκτυο, το λογαριασμό χώρου αποθήκευσης, εικονική μηχανή και ένα περιβάλλον εργασίας δικτύου. Το μοντέλο ανάπτυξης διαχείρισης πόρων Azure παρέχει τη δυνατότητα για να δημιουργήσετε ένα πρότυπο JSON για να αναπτύξετε και να διαχειριστείτε όλες αυτές οι πόροι μαζί ως μία μόνο εφαρμογή.

[AZURE.INCLUDE [virtual-machines-common-compare-deployment-models](../../includes/virtual-machines-common-compare-deployment-models.md)]
