<properties
    pageTitle="Δημιουργήστε μια Εικονική εικόνα από μια Εικονική Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια γενικευμένη Εικονική εικόνα από μια υπάρχουσα Εικονική Azure δημιουργήσατε στο μοντέλο ανάπτυξης για τη διαχείριση πόρων"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>


# <a name="download-the-template-for-a-vm"></a>Λήψη του προτύπου για μια εικονική Μηχανή

Όταν δημιουργείτε μια Εικονική στο Azure χρησιμοποιώντας την πύλη ή το PowerShell, ένα πρότυπο από διαχειριστή πόρων δημιουργείται αυτόματα για εσάς. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο για να αναπαραγάγετε γρήγορα μια ανάπτυξη. Το πρότυπο περιέχει πληροφορίες σχετικά με όλους τους πόρους σε μια ομάδα πόρων. Για μια εικονική μηχανή, αυτό σημαίνει ότι το κοντέινερ πρότυπο όλα τα στοιχεία που δημιουργείται που υποστηρίζουν την εικονική Μηχανή στη συγκεκριμένη ομάδα πόρων, όπως οι πόροι δικτύωσης.

## <a name="download-the-template-using-the-portal"></a>Λήψη του προτύπου με την πύλη

1. Συνδεθείτε [πύλη του Azure](https://portal.azure.com/).
2. Ένα μενού διανομέα, επιλέξτε **εικονικές μηχανές**.
3. Επιλέξτε την εικονική μηχανή από τη λίστα.
5. Επιλέξτε **αυτοματοποίησης δέσμης ενεργειών**.
6. Επιλέξτε **λήψη** και να αποθηκεύσετε το αρχείο .zip στον τοπικό σας υπολογιστή.
7. Ανοίξτε το αρχείο .zip και να εξαγάγετε τα αρχεία σε ένα φάκελο. Το αρχείο .zip θα περιέχει:
    
    - Deploy.ps1
    - Deploy.Sh 
    - deployer.RB
    - DeploymentHelper.cs
    - Parameters.JSON
    - Template.JSON

Το αρχείο .json είναι το πρότυπο.
    
## <a name="download-the-template-using-powershell"></a>Λήψη του προτύπου με χρήση του PowerShell

Επίσης, μπορείτε να κάνετε λήψη του αρχείου προτύπου .json χρησιμοποιώντας το cmdlet [AzureRMResourceGroup εξαγωγής](https://msdn.microsoft.com/library/mt715427.aspx) . Μπορείτε να χρησιμοποιήσετε το `-path` παράμετρο, για να παρέχετε το όνομα αρχείου και τη διαδρομή για το αρχείο .json. Αυτό το παράδειγμα δείχνει πώς μπορείτε να κάνετε λήψη του προτύπου για την ομάδα πόρων με το όνομα **myResourceGroup** στο φάκελο **C:\users\public\downloads** στον τοπικό σας υπολογιστή.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με την ανάπτυξη πόρους με τη χρήση προτύπων, ανατρέξτε στο θέμα [αναλυτικές οδηγίες για το πρότυπο διαχείρισης πόρων](../resource-manager-template-walkthrough.md).