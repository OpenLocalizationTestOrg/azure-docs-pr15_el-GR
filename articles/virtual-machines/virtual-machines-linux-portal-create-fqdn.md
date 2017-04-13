<properties
   pageTitle="Δημιουργία FQDN για μια Εικονική στην πύλη Azure | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα πλήρως προσδιορισμένο όνομα τομέα ή FQDN για διαχείριση πόρων βάσει εικονική μηχανή στην πύλη του Azure."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/23/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Δημιουργήστε ένα πλήρως προσδιορισμένο όνομα τομέα στην πύλη του Azure
Όταν δημιουργείτε μια εικονική μηχανή (Εικονική) στην [πύλη του Azure](https://portal.azure.com) χρησιμοποιώντας το μοντέλο ανάπτυξης για τη διαχείριση πόρων, δημιουργείται αυτόματα μια δημόσια πόρος διευθύνσεων IP για την εικονική μηχανή. Μπορείτε να χρησιμοποιήσετε αυτήν τη διεύθυνση IP για απομακρυσμένη πρόσβαση η Εικονική. Παρόλο που η πύλη δεν δημιουργεί ένα [πλήρως προσδιορισμένο όνομα τομέα](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)ή FQDN, από προεπιλογή, μπορείτε να προσθέσετε μία όταν δημιουργηθεί η Εικονική. Σε αυτό το άρθρο παρουσιάζει τα βήματα για να δημιουργήσετε ένα όνομα DNS ή το FQDN.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Μπορείτε τώρα να συνδεθείτε από απόσταση για να την εικονική Μηχανή χρησιμοποιώντας αυτό το όνομα DNS όπως με `ssh adminuser@testdnslabel.centralus.cloudapp.azure.com`.

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που Εικονική σας έχει ένα δημόσιο όνομα IP και το DNS, μπορείτε να αναπτύξετε κοινών πλαισίων εφαρμογής ή υπηρεσίες, όπως nginx, MongoDB, Docker, κ.λπ.

Μπορείτε επίσης να διαβάσετε περισσότερα σχετικά με τη [χρήση της διαχείρισης πόρων](../azure-resource-manager/resource-group-overview.md) για συμβουλές σχετικά με τη δημιουργία του Azure αναπτύξεις.