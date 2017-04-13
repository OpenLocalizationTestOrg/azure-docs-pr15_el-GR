<properties
    pageTitle="Πληροφορίες για τις εικόνες για εικονικές μηχανές Windows | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με τον τρόπο χρήσης των εικόνων με εικονικές μηχανές Windows στο Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="about-images-for-windows-virtual-machines"></a>Πληροφορίες για τις εικόνες για εικονικές μηχανές Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]



## <a name="working-with-images"></a>Εργασία με εικόνες

Μπορείτε να χρησιμοποιήσετε τη λειτουργική μονάδα Azure PowerShell για να διαχειριστείτε τις εικόνες που είναι διαθέσιμες για τη συνδρομή σας στο Azure. Μπορείτε επίσης να χρησιμοποιήσετε το Azure κλασική πύλη για ορισμένες εργασίες εικόνα, αλλά η γραμμή εντολών σας προσφέρει περισσότερες επιλογές.


-   **Λήψη όλων των εικόνων**:`Get-AzureVMImage`επιστρέφει μια λίστα με όλες τις εικόνες που είναι διαθέσιμη στην τρέχουσα συνδρομή σας: τις εικόνες σας, καθώς και αυτές που παρέχονται από Azure ή συνεργάτες. Αυτό σημαίνει ότι μπορεί να λάβετε μια μεγάλη λίστα. Τα επόμενα παραδείγματα δείχνουν πώς μπορείτε να λάβετε μια πιο σύντομη λίστα.
-   **Λήψη εικόνας οικογένειες**:`Get-AzureVMImage | select ImageFamily` λαμβάνει μια λίστα των οικογενειών εικόνα εμφανίζοντας συμβολοσειρές **ImageFamily** ιδιότητα.
-   **Λήψη όλων των εικόνων σε μια συγκεκριμένη οικογένεια**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **Εύρεση εικόνων Εικονική**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` λειτουργεί με το φιλτράρισμα της ιδιότητας DataDiskConfiguration, η οποία ισχύει μόνο για εικόνες Εικονική. Αυτό το παράδειγμα φίλτρα επίσης το αποτέλεσμα σε μόνο το όνομα της ετικέτας και εικόνα.
-   **Αποθηκεύστε μια γενική εικόνα**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **Αποθήκευση εξειδικευμένες εικόνας**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] Η παράμετρος OSState είναι απαραίτητη, εάν θέλετε να δημιουργήσετε μια Εικονική εικόνα, το οποίο περιλαμβάνει δίσκων δεδομένων, καθώς και ο δίσκος λειτουργικού συστήματος. Εάν δεν μπορείτε να χρησιμοποιήσετε την παράμετρο, το cmdlet δημιουργεί μια εικόνα του λειτουργικού Συστήματος. Η τιμή της παραμέτρου δηλώνει αν η εικόνα είναι γενίκευση ή εξειδικευμένες, με βάση αν ο δίσκος λειτουργικού συστήματος έχει προετοιμαστεί για εκ νέου χρήση.
-   **Διαγραφή εικόνας**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`


## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε επίσης να [δημιουργήσετε έναν υπολογιστή Windows με την πύλη κλασική](virtual-machines-windows-classic-tutorial.md)

