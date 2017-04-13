<properties
    pageTitle="Ανάπτυξη προτύπων με τη γραμμή εντολών σε στοίβα Azure | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε το περιβάλλον γραμμής εντολών πλατφόρμες (CLI) για την ανάπτυξη προτύπων από μέσα του ClientVM ή μετά τη χρήση του VPN για να συνδεθείτε σε στοίβα Azure."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Ανάπτυξη προτύπων σε στοίβα Azure χρησιμοποιώντας τη γραμμή εντολών

Χρησιμοποιήστε τη γραμμή εντολών για την ανάπτυξη πρότυπα διαχείρισης πόρων Azure για το POC στοίβας Azure. Azure πρότυπα διαχείρισης πόρων ανάπτυξη και παροχή όλους τους πόρους για την εφαρμογή σας σε μια ενιαία, συντονισμένη λειτουργία.

## <a name="download-template"></a>Λήψη προτύπου        
Για να δοκιμάσετε μια ανάπτυξη με το CLI, κάντε λήψη του τα αρχεία azuredeploy.json και azuredeploy.parameters.json από τη [Δημιουργία προτύπου στο παράδειγμα το λογαριασμό χώρου αποθήκευσης](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Ανάπτυξη προτύπου
Μεταβείτε στο φάκελο όπου αυτά τα αρχεία έχουν ληφθεί και εκτελέστε την ακόλουθη εντολή για την ανάπτυξη του προτύπου:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Αυτή η εντολή αναπτύσσει το πρότυπο για την ομάδα πόρων **cliRG** στην προεπιλεγμένη θέση του Azure POC στοίβας.

## <a name="validate-template-deployment"></a>Επικυρώστε ανάπτυξη προτύπου
Για να δείτε αυτό λογαριασμού ομάδας και την αποθήκευση πόρων, χρησιμοποιήστε τις παρακάτω εντολές:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Επόμενα βήματα

[Διαχείριση δικαιωμάτων χρήστη](azure-stack-manage-permissions.md)
