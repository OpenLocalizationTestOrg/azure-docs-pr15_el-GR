<properties
   pageTitle="Δημιουργία FQDN για μια Εικονική στην πύλη Azure | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα πλήρως προσδιορισμένο όνομα τομέα ή FQDN για διαχείριση πόρων βάσει εικονική μηχανή στην πύλη του Azure."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Δημιουργήστε ένα πλήρως προσδιορισμένο όνομα τομέα στην πύλη του Azure
Όταν δημιουργείτε μια εικονική μηχανή (Εικονική) στην [πύλη του Azure](https://portal.azure.com) χρησιμοποιώντας το μοντέλο ανάπτυξης για τη διαχείριση πόρων, δημιουργείται αυτόματα μια δημόσια πόρος διευθύνσεων IP για την εικονική μηχανή. Μπορείτε να χρησιμοποιήσετε αυτήν τη διεύθυνση IP για απομακρυσμένη πρόσβαση η Εικονική. Παρόλο που η πύλη δεν δημιουργεί ένα [πλήρως προσδιορισμένο όνομα τομέα](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)ή FQDN, από προεπιλογή, μπορείτε να δημιουργήσετε έναν όταν δημιουργηθεί η Εικονική. Σε αυτό το άρθρο παρουσιάζει τα βήματα για να δημιουργήσετε ένα όνομα DNS ή το FQDN.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Μπορείτε τώρα να συνδεθείτε απομακρυσμένα με την εικονική Μηχανή χρησιμοποιώντας αυτό το όνομα DNS όπως για το πρωτόκολλο απομακρυσμένης επιφάνειας εργασίας (RDP).

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που Εικονική σας έχει ένα δημόσιο όνομα IP και το DNS, μπορείτε να αναπτύξετε κοινών πλαισίων κανόνων εφαρμογής ή υπηρεσίες, όπως των υπηρεσιών IIS, SQL ή το SharePoint.

Μπορείτε επίσης να διαβάσετε περισσότερα σχετικά με τη [χρήση της διαχείρισης πόρων](../azure-resource-manager/resource-group-overview.md) για συμβουλές σχετικά με τη δημιουργία του Azure αναπτύξεις.