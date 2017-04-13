<properties
   pageTitle="Δημιουργία μιας εργασίας ή του σχολείου ταυτότητα στο AAD | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα εταιρικό ή σχολικό ταυτότητα στο Azure Active Directory για να χρησιμοποιήσετε με το Linux εικονικές μηχανές."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-linux-vms"></a>Δημιουργία μιας εργασίας ή σχολείου ταυτότητας σε Azure Active Directory για να χρησιμοποιήσετε με ΣΠΣ Linux

Εάν δημιουργήσατε έναν προσωπικό λογαριασμό Azure ή έχετε μια συνδρομή για προσωπική χρήση MSDN και δημιούργησε το Azure λογαριασμό για να επωφεληθείτε από το MSDN Azure πιστώσεων--χρησιμοποιούσατε μια ταυτότητα *λογαριασμό της Microsoft* για τη δημιουργία του. Πολλά εξαιρετικά χαρακτηριστικά του Azure-- [πρότυπα ομάδα πόρων](../azure-resource-manager/resource-group-overview.md) είναι ένα παράδειγμα--απαιτεί λογαριασμό εργασίας ή του σχολείου (μια ταυτότητα διαχειριζόμενο από το Azure Active Directory) για να εργαστείτε. Μπορείτε να ακολουθήσετε τις παρακάτω οδηγίες για να δημιουργήσετε ένα νέο λογαριασμό εργασίας ή σχολείου επειδή Ευτυχώς, είναι ένα από τα καλύτερα πράγματα σχετικά με τον προσωπικό σας λογαριασμό Azure που παρέχεται με έναν προεπιλεγμένο τομέα Azure Active Directory που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε ένα νέο λογαριασμό εργασίας ή σχολείου που μπορείτε να χρησιμοποιήσετε με το Azure δυνατοτήτων που απαιτούν την.

Ωστόσο, πρόσφατες αλλαγές να είναι δυνατό να διαχειριστείτε τη συνδρομή σας με οποιονδήποτε τύπο λογαριασμός Azure χρησιμοποιώντας το `azure login` μέθοδο αλληλεπιδραστικής σύνδεσης που περιγράφεται [εδώ](../xplat-cli-connect.md). Μπορείτε είτε να χρησιμοποιήσετε αυτό το μηχανισμό ή μπορείτε να ακολουθήσετε τις οδηγίες που ακολουθούν. Μπορείτε επίσης να [δημιουργήσετε έναν εταιρικό ή σχολικό ταυτότητα στο Azure Active Directory για χρήση με το Windows ΣΠΣ](virtual-machines-windows-create-aad-work-id.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]
