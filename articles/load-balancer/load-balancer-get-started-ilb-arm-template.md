<properties
   pageTitle="Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου χρησιμοποιώντας ένα πρότυπο στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς να δημιουργείτε μια εσωτερική εξισορρόπηση φόρτου χρησιμοποιώντας ένα πρότυπο στη Διαχείριση πόρων"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-a-template"></a>Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου χρησιμοποιώντας ένα πρότυπο

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][μοντέλο κλασική ανάπτυξης](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Ανάπτυξη του προτύπου με κλικ για την ανάπτυξη

Το δείγμα προτύπου διαθέσιμη στο δημόσιο αποθετήριο χρησιμοποιεί ένα αρχείο παραμέτρων που περιέχει την προεπιλεγμένη τιμών που χρησιμοποιούνται για τη δημιουργία το σενάριο που περιγράφονται παραπάνω. Για να αναπτύξετε αυτό το πρότυπο χρησιμοποιώντας κάντε κλικ στην επιλογή για να αναπτύξετε, ακολουθήστε [αυτήν τη σύνδεση](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), κάντε κλικ στην επιλογή **Ανάπτυξη να Azure**, αντικαταστήστε τις προεπιλεγμένες τιμές παραμέτρων Εάν είναι απαραίτητο, και ακολουθήστε τις οδηγίες στην πύλη του.

## <a name="deploy-the-template-by-using-powershell"></a>Ανάπτυξη του προτύπου με χρήση του PowerShell

Για να αναπτύξετε το πρότυπο που λάβατε με χρήση του PowerShell, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../../articles/powershell-install-configure.md) και ακολουθήστε τις οδηγίες προς το τέλος για να συνδεθείτε στο Azure και επιλέξτε τη συνδρομή σας.
2. Κάντε λήψη του αρχείου παραμέτρων στον τοπικό δίσκο.
3. Επεξεργαστείτε το αρχείο και να το αποθηκεύσετε.
4. Εκτελέστε το cmdlet **New-AzureRmResourceGroupDeployment** για να δημιουργήσετε μια ομάδα πόρων με το πρότυπο.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Ανάπτυξη του προτύπου με τη χρήση του CLI Azure

Για να αναπτύξετε το πρότυπο χρησιμοποιώντας το Azure CLI, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../../articles/xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.
2. Εκτελέστε την εντολή **λειτουργία azure ρύθμισης παραμέτρων** για να μεταβείτε σε λειτουργία διαχείρισης πόρων, όπως φαίνεται παρακάτω.

        azure config mode arm

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        info:    New mode is arm

3. Ανοίξτε το αρχείο παραμέτρων, επιλέξτε τα περιεχόμενά και αποθηκεύστε το σε ένα αρχείο στον υπολογιστή σας. Για αυτό το παράδειγμα, θα σας να αποθηκεύσει το αρχείο παραμέτρους για να *parameters.json*.

4. Εκτελέστε την εντολή **Δημιουργία ομάδας azure ανάπτυξης** για να αναπτύξετε το νέο εσωτερικό εξισορρόπηση φόρτου χρησιμοποιώντας τα αρχεία προτύπου και η παράμετρος που έχουν ληφθεί και τροποποίησης παραπάνω. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται.

        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Επόμενα βήματα

[Ρύθμιση παραμέτρων μια λειτουργία διανομής εξισορρόπησης φόρτου χρησιμοποιώντας συσχέτισης IP προέλευσης](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)



