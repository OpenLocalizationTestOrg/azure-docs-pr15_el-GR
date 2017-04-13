<properties
    pageTitle="Ανάπτυξη προτύπων με το PowerShell σε στοίβα Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να αναπτύξετε μια εικονική μηχανή με χρήση ενός προτύπου για τη διαχείριση πόρων και PowerShell."
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
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Ανάπτυξη προτύπων σε στοίβα Azure χρησιμοποιώντας το PowerShell

Χρήση του PowerShell για ανάπτυξη προτύπων για τη διαχείριση πόρων Azure για το Azure POC στοίβας.  Διαχείριση πόρων πρότυπα ανάπτυξη και παροχή όλους τους πόρους για την εφαρμογή σας σε μια ενιαία, συντονισμένη λειτουργία.

## <a name="run-azurerm-powershell-cmdlets"></a>Εκτελέστε το cmdlet του AzureRM PowerShell

Σε αυτό το παράδειγμα, εκτελέστε μια δέσμη ενεργειών για να αναπτύξετε μια εικονική μηχανή σε POC στοίβας Azure χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων.  Πριν να συνεχίσετε, βεβαιωθείτε ότι έχετε [εγκαταστήσει και να ρυθμίσει τις παραμέτρους του PowerShell](azure-stack-connect-powershell.md)  

VHD χρησιμοποιείται σε αυτό το πρότυπο παράδειγμα είναι μια προεπιλεγμένη εικόνα marketplace (WindowsServer-2012-R2-κέντρο δεδομένων).

1.  Μεταβείτε σε <http://aka.ms/AzureStackGitHub>, αναζητήστε το πρότυπο **101-απλών-windows-εικονική** και αποθηκεύστε το στην εξής θέση: c:\\πρότυπα\\azuredeploy-101-απλών-windows-vm.json.

2.  Στο PowerShell, εκτελέστε την ακόλουθη δέσμη ενεργειών ανάπτυξης. Αντικαταστήστε το *όνομα χρήστη* και *τον κωδικό πρόσβασης* με το όνομα χρήστη και τον κωδικό πρόσβασης. Σε επόμενες χρήσεις, αυξήσετε την τιμή για την παράμετρο *$myNum* για να μην αντικατασταθεί ανάπτυξής σας.

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  Ανοίξτε την πύλη του Azure στοίβα, κάντε κλικ στο κουμπί **Αναζήτηση**, κάντε κλικ στην επιλογή **εικονικές μηχανές**και αναζητήστε το νέο εικονική μηχανή (*myDeployment001*).

## <a name="video-example-hybrid-virtual-machine-deployment"></a>Παράδειγμα βίντεο: υβριδική ανάπτυξη εικονική μηχανή

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>Επόμενα βήματα

[Ανάπτυξη προτύπων με το Visual Studio](azure-stack-deploy-template-visual-studio.md)
