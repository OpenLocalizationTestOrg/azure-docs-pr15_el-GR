<properties
    pageTitle="Διαφορετικούς τρόπους για να δημιουργήσετε μια Εικονική Windows | Microsoft Azure"
    description="Παραθέτει τους διαφορετικούς τρόπους για να δημιουργήσετε μια εικονική μηχανή Windows με τη διαχείριση πόρων."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>Διαφορετικούς τρόπους για να δημιουργήσετε μια εικονική μηχανή Windows με το διαχειριστή πόρων

Azure προσφέρει διαφορετικούς τρόπους για να δημιουργήσετε μια εικονική μηχανή επειδή εικονικές μηχανές είναι κατάλληλες για σκοπούς και διαφορετικούς χρήστες. Αυτό σημαίνει ότι πρέπει να κάνετε ορισμένες επιλογές σχετικά με την εικονική μηχανή και πώς μπορείτε να το δημιουργήσετε. Σε αυτό το άρθρο παρέχει μια σύνοψη των αυτές τις επιλογές και συνδέσεις με οδηγίες.

## <a name="azure-portal"></a>Πύλη του Azure

Με την πύλη Azure είναι ένας απλός τρόπος για να δοκιμάσετε μια εικονική μηχανή, ειδικά εάν που ξεκινάτε με Azure. 

[Δημιουργήστε μια εικονική μηχανή με Windows με την πύλη](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>Πρότυπο

Εικονικές μηχανές απαιτούν ένα συνδυασμό των πόρων (όπως ένα διαθεσιμότητα λογαριασμούς χώρου αποθήκευσης και τα σύνολα). Αντί για την ανάπτυξη και τη διαχείριση του κάθε πόρο ξεχωριστά, μπορείτε να δημιουργήσετε ένα πρότυπο από διαχειριστή πόρων Azure που αναπτύσσεται και διατάξεις όλους τους πόρους σε μια ενιαία, συντονισμένη λειτουργία.

- [Δημιουργήστε μια εικονική μηχανή Windows με ένα πρότυπο από διαχειριστή πόρων](virtual-machines-windows-ps-template.md)


## <a name="azure-powershell"></a>Azure PowerShell

Εάν προτιμάτε να εργάζεστε σε μια εντολή κελύφους, μπορείτε να χρησιμοποιήσετε Azure PowerShell.

- [Δημιουργήστε μια Εικονική Windows με χρήση του PowerShell](virtual-machines-windows-ps-create.md)


## <a name="visual-studio"></a>Visual Studio

Χρησιμοποιήστε το Visual Studio για να δημιουργήσετε, να διαχειριστείτε και ανάπτυξη ΣΠΣ με τα εργαλεία Azure για το Visual Studio και το Azure SDK.

[Εργαλεία Azure για το Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

