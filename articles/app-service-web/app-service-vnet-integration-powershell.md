<properties
    pageTitle="Σύνδεση της εφαρμογής σας με το εικονικό δίκτυο με χρήση του PowerShell"
    description="Οδηγίες σχετικά με το πώς μπορείτε να συνδεθείτε και να εργαστείτε με εικονικών δικτύων με χρήση του PowerShell"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Σύνδεση της εφαρμογής σας με το εικονικό δίκτυο με χρήση του PowerShell #

## <a name="overview"></a>Επισκόπηση ##

Στην υπηρεσία εφαρμογής Azure, μπορείτε να συνδεθείτε την εφαρμογή σας (web, mobile ή API) Azure εικονικού δικτύου (VNet) στη συνδρομή σας. Αυτή η δυνατότητα ονομάζεται VNet ενοποίησης. Τη δυνατότητα ενοποίησης VNet δεν θα πρέπει να συγχέονται με τη δυνατότητα εφαρμογής υπηρεσίας περιβάλλον, το οποίο σας επιτρέπει να εκτελέσετε μια παρουσία του Azure εφαρμογής υπηρεσίας στο δίκτυο εικονικού.

Η δυνατότητα ενοποίησης VNet έχει ένα περιβάλλον εργασίας χρήστη (UI) στην νέα πύλη που μπορείτε να χρησιμοποιήσετε για την ενοποίηση με το εικονικό δίκτυα που έχουν αναπτυχθεί με τη χρήση του μοντέλου κλασική ανάπτυξης ή το μοντέλο ανάπτυξης Azure διαχείριση πόρων. Εάν θέλετε να μάθετε περισσότερα σχετικά με τη δυνατότητα, ανατρέξτε στο θέμα [ενσωματώσουν την εφαρμογή σας με μια Azure εικονικού δικτύου](web-sites-integrate-with-vnet.md).

Σε αυτό το άρθρο είναι δεν σχετικά με το πώς μπορείτε να χρησιμοποιήσετε το περιβάλλον εργασίας Χρήστη αλλά προτιμάτε σχετικά με το πώς μπορείτε να ενεργοποιήσετε την ενοποίηση με χρήση του PowerShell. Επειδή οι εντολές για κάθε μοντέλο ανάπτυξης είναι διαφορετικό, αυτό το άρθρο περιλαμβάνει μια ενότητα για κάθε μοντέλο ανάπτυξης.  

Πριν να συνεχίσετε με αυτό το άρθρο, βεβαιωθείτε ότι έχετε:

- Το πιο πρόσφατο Azure PowerShell SDK εγκατεστημένο. Μπορείτε να το εγκαταστήσετε με το πρόγραμμα εγκατάστασης πλατφόρμας Web.
- Μια εφαρμογή στην υπηρεσία εφαρμογής Azure που εκτελείται σε τυπική ή Premium SKU.

## <a name="classic-virtual-networks"></a>Κλασική εικονικών δικτύων ##

Η ενότητα αυτή εξηγεί τρεις εργασίες για εικονικού δίκτυα που χρησιμοποιούν το μοντέλο κλασική ανάπτυξη:

1. Σύνδεση της εφαρμογής σας σε προϋπάρχουσες εικονικό δίκτυο που έχει μια πύλη και έχει ρυθμιστεί για τη σύνδεση σημείου σε τοποθεσία.
1. Ενημέρωση πληροφοριών ενοποίησης εικονικού δικτύου για την εφαρμογή σας.
1. Αποσύνδεση της εφαρμογής σας από το εικονικό δίκτυο.

### <a name="connect-an-app-to-a-classic-vnet"></a>Σύνδεση μιας εφαρμογής σε μια κλασική VNet ###

Για να συνδέσετε μια εφαρμογή ένα εικονικό δίκτυο, ακολουθήστε αυτά τα τρία βήματα:

1. Δηλώστε στην εφαρμογή web ότι αυτό θα τη συμμετοχή σε μια συγκεκριμένη εικονικού δικτύου. Η εφαρμογή θα δημιουργήσει ένα πιστοποιητικό που παρέχεται με το εικονικό δίκτυο για τη σύνδεση σημείου σε τοποθεσία.
1. Αποστείλετε το πιστοποιητικό εφαρμογής web με το εικονικό δίκτυο και, στη συνέχεια, ανάκτηση του πακέτου VPN σημείου σε τοποθεσία URI.
1. Ενημέρωση σύνδεσης εικονικού δικτύου του web app με το πακέτο σημείου σε τοποθεσία URI.

Το πρώτο και το τρίτο βήματα είναι πλήρως δέσμη ενεργειών, αλλά το δεύτερο βήμα απαιτεί ένα μοναδικό, μη αυτόματη ενέργεια από την πύλη ή access για την εκτέλεση ενεργειών **ΤΟΠΟΘΈΤΗΣΗ** ή **ΚΏΔΙΚΑ** στην το τελικό σημείο για τη διαχείριση πόρων Azure εικονικού δικτύου. Επικοινωνήστε με την υποστήριξη Azure να έχουν αυτήν τη δυνατότητα. Πριν ξεκινήσετε, βεβαιωθείτε ότι έχετε μια κλασική εικονικού δικτύου που έχει ήδη ενεργοποιηθεί συνδεσιμότητας σημείου σε τοποθεσία και ανεπτυγμένος πύλης. Για να δημιουργήσετε την πύλη και να ενεργοποιήσετε τη σύνδεση σημείου σε τοποθεσία, πρέπει να χρησιμοποιήσουν την πύλη, όπως περιγράφεται στην [τη δημιουργία μιας πύλης VPN][createvpngateway].

Το εικονικό δίκτυο κλασική πρέπει να βρίσκεται στην ίδια συνδρομή ως το πρόγραμμα εφαρμογής υπηρεσίας που διατηρεί την εφαρμογή που ενοποίηση με το.

##### <a name="set-up-azure-powershell-sdk"></a>Ρύθμιση του Azure PowerShell SDK #####

Ανοίξτε ένα παράθυρο του PowerShell και να ρυθμίσετε το Azure λογαριασμό και συνδρομής με τη χρήση:

    Login-AzureRmAccount

Η εντολή θα ανοίξει ένα μήνυμα για να λάβετε Azure τα διαπιστευτήριά σας. Μετά την είσοδό σας, χρησιμοποιήστε μία από τις παρακάτω εντολές για να επιλέξετε τη συνδρομή στην οποία θέλετε να χρησιμοποιήσετε. Βεβαιωθείτε ότι χρησιμοποιείτε τη συνδρομή στην οποία είναι το εικονικού δικτύου και το πρόγραμμα εφαρμογής υπηρεσίας στο.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

ή

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>Μεταβλητές που χρησιμοποιούνται σε αυτό το άρθρο #####

Για να απλοποιήσετε εντολές, θα μπορούμε να εγκαταστήσουμε μεταβλητή PowerShell **$Configuration** με τη συγκεκριμένη ρύθμιση παραμέτρων.

Ορίστε μια μεταβλητή ως εξής στο PowerShell με τις ακόλουθες παραμέτρους:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

Η θέση της εφαρμογής πρέπει να είναι στη θέση χωρίς κενά διαστήματα. Για παράδειγμα, Δυτική ΗΠΑ είναι westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

Στο επόμενο στοιχείο είναι όπου πρέπει να γράφονται το πιστοποιητικό. Αυτό πρέπει να είναι μια διαδρομή εγγράψιμο στον τοπικό σας υπολογιστή. Φροντίστε να συμπεριλάβετε .cer στο τέλος.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Για να δείτε τι μπορείτε να ορίσετε, πληκτρολογήστε **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

Τα υπόλοιπα αυτή η ενότητα προϋποθέτει ότι έχετε μια μεταβλητή που δημιουργήθηκε όπως ακριβώς περιγράφεται.

##### <a name="declare-the-virtual-network-to-the-app"></a>Δήλωση το εικονικό δίκτυο στην εφαρμογή #####

Χρησιμοποιήστε την παρακάτω εντολή για να υποδείξετε την εφαρμογή ότι αυτό θα χρησιμοποιούν το συγκεκριμένο εικονικού δικτύου. Αυτό θα προκαλέσει την εφαρμογή για να δημιουργήσετε τα πιστοποιητικά είναι απαραίτητο:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Εάν αυτή η εντολή είναι επιτυχής, **$vnet** θα πρέπει να έχετε μια μεταβλητή **Ιδιότητες** σε αυτό. Η μεταβλητή **Ιδιότητες** πρέπει να περιέχει ένα αποτύπωση πιστοποιητικού και δεδομένων του πιστοποιητικού.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>Αποστείλετε το πιστοποιητικό εφαρμογής web με το εικονικό δίκτυο #####

Μη αυτόματη, μοναδικό βήμα απαιτείται για κάθε συνδρομής και συνδυασμός εικονικού δικτύου. Αυτό σημαίνει ότι, εάν συνδέεστε εφαρμογές στην εγγραφή Α έως το Α εικονικού δικτύου, θα πρέπει να κάνετε αυτό το βήμα μόνο μία φορά ανεξάρτητα από το πόσες εφαρμογές ρυθμίζετε τις παραμέτρους. Εάν πρόκειται να προσθέσετε μια νέα εφαρμογή σε μια άλλη εικονικού δικτύου, θα χρειαστεί να το κάνετε αυτό ξανά. Ο λόγος για αυτό είναι που δημιουργείται ένα σύνολο πιστοποιητικών σε επίπεδο συνδρομή στην Azure εφαρμογής υπηρεσίας και το σύνολο δημιουργείται μία φορά για κάθε εικονικό δίκτυο στο οποίο θα συνδεθείτε στις εφαρμογές.

Τα πιστοποιητικά θα έχετε ρυθμίσει ήδη εάν ακολουθήσατε τα παρακάτω βήματα ή αν συνδέσει με το ίδιο εικονικού δικτύου, χρησιμοποιώντας την πύλη.

Το πρώτο βήμα είναι να δημιουργήσετε το αρχείο .cer. Το δεύτερο βήμα είναι να αποστείλετε το αρχείο .cer στο εικονικού δικτύου σας. Για να δημιουργήσετε το αρχείο .cer από την κλήση API σε ένα προηγούμενο βήμα, εκτελέστε τις ακόλουθες εντολές.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Το πιστοποιητικό που βρίσκονται στη θέση που **$Configuration.GeneratedCertificatePath** καθορίζει.

Για να αποστείλετε το πιστοποιητικό με μη αυτόματο τρόπο, χρησιμοποιήστε την [πύλη του Azure] [ azureportal] και **Αναζητήστε εικονικού δικτύου (κλασική)** > **συνδέσεις VPN** > **σημείο σε τοποθεσία** > **Διαχείριση πιστοποιητικών**. Από εδώ, αποστείλετε το πιστοποιητικό.

##### <a name="get-the-point-to-site-package"></a>Λήψη του πακέτου σημείου σε τοποθεσία #####

Το επόμενο βήμα στη ρύθμιση σύνδεσης εικονικού δικτύου σε μια εφαρμογή web είναι για τη λήψη του πακέτου σημείου σε τοποθεσία και παρέχει σε εφαρμογή web.

Αποθηκεύστε το ακόλουθο πρότυπο σε ένα αρχείο που ονομάζεται GetNetworkPackageUri.json κάπου στον υπολογιστή σας, για παράδειγμα, C:\Azure\Templates\GetNetworkPackageUri.json.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Ρύθμιση παραμέτρων εισαγωγής:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Κλήση της δέσμης ενεργειών:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


Η μεταβλητή **$output. Outputs.packageUri** τώρα θα περιέχει το πακέτο URI να δοθεί σε εφαρμογή web.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Αποστείλετε το πακέτο του σημείου σε τοποθεσία για την εφαρμογή σας #####

Το τελικό βήμα είναι να δώσετε την εφαρμογή με αυτό το πακέτο. Απλώς εκτελέστε την επόμενη εντολή:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Εάν ένα μήνυμα σας ζητά να επιβεβαιώσετε ότι αντικαθιστάτε έναν υπάρχοντα πόρο, βεβαιωθείτε ότι για να επιτρέψετε την.

Όταν ολοκληρωθεί με επιτυχία αυτήν την εντολή, της εφαρμογής σας θα πρέπει τώρα να είναι συνδεδεμένη το εικονικό δίκτυο. Για να επιβεβαιώσετε την επιτυχία, μεταβείτε στην κονσόλα εφαρμογής και πληκτρολογήστε τα εξής:

    SET WEBSITE_

Εάν υπάρχει μια μεταβλητή περιβάλλοντος που ονομάζεται WEBSITE_VNETNAME που έχει μια τιμή που αντιστοιχεί στο όνομα του προορισμού εικονικού δικτύου, όλες τις ρυθμίσεις παραμέτρων έχετε ολοκληρώθηκε με επιτυχία.

### <a name="update-classic-vnet-integration-information"></a>Ενημέρωση πληροφοριών ενοποίησης κλασική VNet ###

Για να ενημερώσετε ή να γίνει επανασυγχρονισμός τις πληροφορίες σας, απλά επαναλάβετε τα βήματα που ακολουθήσατε όταν δημιουργήσατε την ενοποίηση αρχικά. Αυτά τα βήματα είναι τα εξής:

1. Ορισμός πληροφορίες ρύθμισης παραμέτρων σας.
1. Δηλώστε το εικονικό δίκτυο στην εφαρμογή.
1. Λήψη του πακέτου σημείου σε τοποθεσία.
1. Αποστείλετε το πακέτο του σημείου σε τοποθεσία για την εφαρμογή σας.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Αποσύνδεση της εφαρμογής σας από μια κλασική VNet ###

Για να αποσυνδέσετε την εφαρμογή, χρειάζεστε τις πληροφορίες ρύθμισης παραμέτρων που ορίστηκε κατά τη διάρκεια της ενοποίησης εικονικού δικτύου. Χρησιμοποιώντας αυτές τις πληροφορίες, υπάρχει, στη συνέχεια, μία εντολή για την αποσύνδεση της εφαρμογής σας από το δίκτυό σας εικονικού.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Διαχείριση πόρων εικονικών δικτύων ##

Διαχείριση πόρων εικονικού δίκτυα έχουν Azure API διαχείρισης πόρων, που απλοποιήσετε ορισμένες διαδικασίες σε σύγκριση με κλασική εικονικού δίκτυα. Έχουμε μια δέσμη ενεργειών που θα σας βοηθήσουν να ολοκληρώσετε τις παρακάτω εργασίες:

- Δημιουργήστε ένα εικονικό δίκτυο διαχείρισης πόρων και ενοποίηση την εφαρμογή σας με το.
- Δημιουργία πύλης, να ρυθμίσετε τη σύνδεση σημείου σε τοποθεσία σε ένα δίκτυο εικονικού προϋπάρχουσες διαχείριση πόρων και, στη συνέχεια, να ενοποιήσετε την εφαρμογή σας με αυτό.
- Ενοποίηση της εφαρμογής σας με ένα προϋπάρχουσες εικονικό δίκτυο διαχείρισης πόρων που έχει μια πύλη και σημείο σε τοποθεσία με δυνατότητα σύνδεσης.
- Αποσύνδεση της εφαρμογής σας από το εικονικό δίκτυο.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Διαχείριση πόρων VNet εφαρμογής υπηρεσίας ενοποίησης ενεργειών ###

Αντιγράψτε την ακόλουθη δέσμη ενεργειών και αποθηκεύστε το σε ένα αρχείο. Εάν δεν θέλετε να χρησιμοποιήσετε τη δέσμη ενεργειών, μην διστάσεις για να μάθετε από, για να δείτε πώς μπορείτε να ρυθμίσετε στοιχεία με ένα εικονικό δίκτυο διαχείρισης πόρων.


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Αποθηκεύστε ένα αντίγραφο της δέσμης ενεργειών. Σε αυτό το άρθρο ονομάζεται V2VnetAllinOne.ps1, αλλά μπορείτε να χρησιμοποιήσετε ένα διαφορετικό όνομα. Υπάρχουν χωρίς ορίσματα για αυτήν τη δέσμη ενεργειών. Μπορείτε απλώς το εκτελέσετε. Το πρώτο πράγμα που θα κάνει η δέσμη ενεργειών είναι σας ζητηθεί να εισέλθετε. Μετά την είσοδό σας, η δέσμη ενεργειών αποκτά λεπτομέρειες σχετικά με το λογαριασμό σας και επιστρέφει μια λίστα των συνδρομών. Δεν μετρώντας την αίτηση για τα διαπιστευτήριά σας, την εκτέλεση της αρχικής δέσμης ενεργειών μοιάζει κάπως έτσι:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Επίδειξη συνδρομή (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) Δοκιμή MS (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Μοβ επίδειξη συνδρομή (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Ενεργοποιήστε μια επιλογή: 3

    Λογαριασμός: ccompy@microsoft.com περιβάλλον: AzureCloud συνδρομή: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d μισθωτή: 722278f-fef1-499f-91ab-2323d011db47

    Πληκτρολογήστε την ομάδα των πόρων της εφαρμογής: hcdemo rg, πληκτρολογήστε το όνομα της εφαρμογής: v2vnetpowershell τι θέλετε να κάνετε;

    1) Προσθήκη ΝΈΟΥ εικονικού δικτύου σε μια εφαρμογή
    2) Προσθήκη ένα ΥΠΆΡΧΟΝ εικονικού δικτύου σε μια εφαρμογή
    3) Κατάργηση εικονικού δικτύου από μια εφαρμογή

Τα υπόλοιπα αυτή η ενότητα εξηγεί κάθε μία από αυτές τις τρεις επιλογές.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Δημιουργία μιας VNet διαχείρισης πόρων και ενοποίηση με το ###

Για να δημιουργήσετε μια νέα εικονικού δικτύου που χρησιμοποιεί το μοντέλο ανάπτυξης διαχείρισης πόρων και την ενοποίηση με την εφαρμογή σας, επιλέξτε **1) Προσθήκη ΝΈΟΥ εικονικού δικτύου σε μια εφαρμογή**. Αυτό θα σας ειδοποιήσει για το όνομα του εικονικού δικτύου. Σε περίπτωση μου, όπως μπορείτε να δείτε στις ακόλουθες ρυθμίσεις, να χρησιμοποιείται το όνομα, v2pshell.

Η δέσμη ενεργειών σάς παρέχει τις λεπτομέρειες σχετικά με το εικονικό δίκτυο που δημιουργείται. Εάν θέλετε να, μπορώ να αλλάξω κάποια από τις τιμές. Σε αυτό το παράδειγμα εκτέλεσης, να δημιουργήσει ένα εικονικό δίκτυο που έχει τις ακόλουθες ρυθμίσεις:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Εάν θέλετε να αλλάξετε οποιαδήποτε από τις τιμές, πληκτρολογήστε **Y** και κάντε τις αλλαγές. Όταν είστε ικανοποιημένοι με τις ρυθμίσεις εικονικού δικτύου, πληκτρολογήστε **N** ή απλώς πατήστε το πλήκτρο Enter, όταν σας ζητηθεί σχετικά με την αλλαγή των ρυθμίσεων. Από εκεί στην έως ότου ολοκληρωθεί, η δέσμη ενεργειών θα σας ενημερώσει ορισμένα από αυτό που ' πώς πρόκειται κάνοντας μέχρι να ξεκινά για τη δημιουργία της πύλης εικονικού δικτύου. Αυτό το βήμα μπορεί να χρειαστεί έως και μία ώρα. Δεν υπάρχει καμία ένδειξη προόδου σε αυτήν τη φάση, αλλά η δέσμη ενεργειών θα σας ενημερώσει όταν έχει δημιουργηθεί η πύλη.

Όταν ολοκληρωθεί η δέσμη ενεργειών, θα εμφανίζεται η ένδειξη **ολοκληρώθηκε**. Σε αυτό το σημείο, θα έχετε ένα εικονικό δίκτυο διαχείρισης πόρων που περιλαμβάνει το όνομα και τις ρυθμίσεις που έχετε επιλέξει. Αυτό το νέο δίκτυο εικονικού εμβέλειας επίσης με την εφαρμογή σας.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Ενοποίηση της εφαρμογής σας με μια προϋπάρχουσες VNet από διαχειριστή πόρων ###

Όταν ενοποίηση με προϋπάρχουσες εικονικό δίκτυο, εάν παρέχεται ένα εικονικό δίκτυο διαχείρισης πόρων που δεν έχει μια πύλη ή τη σύνδεση σημείου σε τοποθεσία, η δέσμη ενεργειών θα ρυθμιστεί που. Εάν η VNET έχει ήδη αυτά τα στοιχεία ρυθμίσετε, η δέσμη ενεργειών μετάβαση απευθείας την ενοποίηση εφαρμογής. Για να ξεκινήσετε αυτήν τη διαδικασία, επιλέξτε απλώς **2) Προσθήκη ενός ΥΠΆΡΧΟΝΤΟΣ εικονικού δικτύου σε μια εφαρμογή**.

Η επιλογή αυτή λειτουργεί μόνο εάν έχετε ένα προϋπάρχουσες εικονικό δίκτυο διαχείρισης πόρων που βρίσκεται στην ίδια συνδρομή του ως την εφαρμογή σας. Αφού επιλέξετε την επιλογή, θα εμφανιστεί μια λίστα εικονικού δίκτυά σας για τη διαχείριση πόρων.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Ενεργοποιήστε μια επιλογή: 5

Επιλέξτε απλώς το εικονικό δίκτυο που θέλετε να ενσωματώσετε με. Εάν έχετε ήδη μια πύλη που έχει ενεργοποιημένη συνδεσιμότητας σημείου σε τοποθεσία, η δέσμη ενεργειών θα ενσωματώσετε απλώς την εφαρμογή σας με το δίκτυό σας εικονικού. Εάν δεν έχετε μια πύλη, θα πρέπει να καθορίσετε το υποδίκτυο πύλης. Πρέπει να σας υποδίκτυο πύλη στο χώρο σας διεύθυνση εικονικού δικτύου και δεν μπορεί να είναι σε οποιοδήποτε άλλο υποδικτύου. Εάν έχετε ένα εικονικό δίκτυο χωρίς μια πύλη και να εκτελέσετε αυτό το βήμα, πράγματα μοιάζει κάπως έτσι:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

Σε αυτό το παράδειγμα, να δημιουργήσει μια πύλη εικονικού δικτύου που έχει τις ακόλουθες ρυθμίσεις:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Εάν θέλετε να αλλάξετε οποιαδήποτε από αυτές τις ρυθμίσεις, μπορείτε να το κάνετε. Διαφορετικά, πατήστε το πλήκτρο Enter και τη δέσμη ενεργειών θα δημιουργήσετε την πύλη και να επισυνάψετε την εφαρμογή σας σε εικονικού δικτύου σας. Της ώρας δημιουργίας πύλης εξακολουθεί να είναι σε ώρες, όμως, οπότε βεβαιωθείτε ότι που έχετε υπόψη. Όταν όλα τα στοιχεία έχει ολοκληρωθεί, η δέσμη ενεργειών θα εμφανίζεται η ένδειξη **ολοκληρώθηκε**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Αποσύνδεση της εφαρμογής σας από ένα VNet από διαχειριστή πόρων ###

Αποσύνδεση από την εφαρμογή από το δίκτυό σας εικονικού δεν λαμβάνουν προς τα κάτω στην πύλη ή να απενεργοποιήσετε τη σύνδεση σημείου σε τοποθεσία. Τελικά, να χρησιμοποιείτε το για κάτι άλλο. Το επίσης αποσυνδέετε το από κάποια από τις άλλες εφαρμογές εκτός από το που παρέχονται. Για να εκτελέσετε αυτήν την ενέργεια, επιλέξτε **3) καταργείτε ένα εικονικό δίκτυο από μια εφαρμογή**. Όταν κάνετε αυτό, θα δείτε κάτι παρόμοιο με το εξής:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Παρόλο που η δέσμη ενεργειών εμφανίζεται η ένδειξη διαγραφής, δεν διαγράφει το εικονικό δίκτυο. Αυτό μόνο η κατάργηση την ενοποίηση. Αφού βεβαιωθείτε ότι αυτό είναι το τι θέλετε να κάνετε, η εντολή Επεξεργασία αρκετά γρήγορα και σάς ενημερώνει **True** όταν γίνεται.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
