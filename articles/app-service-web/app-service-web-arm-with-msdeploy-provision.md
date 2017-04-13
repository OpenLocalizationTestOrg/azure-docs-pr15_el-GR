<properties
    pageTitle="Ανάπτυξη εφαρμογής web με χρήση MSDeploy με το όνομα κεντρικού υπολογιστή και ssl πιστοποιητικού"
    description="Χρήση ενός προτύπου για τη διαχείριση πόρων Azure για την ανάπτυξη μιας εφαρμογής web με χρήση MSDeploy και τη ρύθμιση του προσαρμοσμένου hostname και ένα πιστοποιητικό SSL"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Ανάπτυξη εφαρμογής web με MSDeploy, το προσαρμοσμένο όνομα κεντρικού υπολογιστή και το πιστοποιητικό SSL

Αυτός ο οδηγός καθοδηγεί σε τη δημιουργία μιας ανάπτυξης για ολοκληρωμένες για μια εφαρμογή Web Azure, αξιοποίηση MSDeploy καθώς και προσθέτοντας ένα προσαρμοσμένο όνομα κεντρικού υπολογιστή και πιστοποιητικού SSL στο πρότυπο ARM.

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προτύπων, ανατρέξτε στο θέμα [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure](../resource-group-authoring-templates.md).

###<a name="create-sample-application"></a>Δημιουργία δείγματος εφαρμογής

Πρόκειται να αναπτύξετε μια εφαρμογή web ASP.NET. Το πρώτο βήμα είναι να δημιουργήσετε μια εφαρμογή web απλό (ή θα μπορούσατε να επιλέξετε να χρησιμοποιήσετε ένα υπάρχον - σε αυτήν την περίπτωση, μπορείτε να παραλείψετε αυτό το βήμα).

Ανοίξτε το Visual Studio 2015 και επιλέξτε Αρχείο > νέο έργο. Στο παράθυρο διαλόγου που εμφανίζεται, επιλέξτε Web > εφαρμογής Web ASP.NET. Στην περιοχή πρότυπα, επιλέξτε Web και επιλέξτε το πρότυπο MVC. Επιλέξτε _Τύπος ελέγχου ταυτότητας αλλαγή_ _Χωρίς_έλεγχο ταυτότητας. Αυτό είναι απλώς να κάνετε το δείγμα εφαρμογής όσο πιο απλές.

Σε αυτό το σημείο, έχετε μια βασική εφαρμογή web ASP.Net που είναι έτοιμες για χρήση ως μέρος της διαδικασίας ανάπτυξης.

###<a name="create-msdeploy-package"></a>Δημιουργία πακέτου MSDeploy

Επόμενο βήμα είναι να δημιουργήσετε το πακέτο για την ανάπτυξη της εφαρμογής web σε Azure. Για να κάνετε αυτό, αποθηκεύστε το έργο σας και, στη συνέχεια, εκτελέστε την ακόλουθη από τη γραμμή εντολών:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Αυτό θα δημιουργήσει ένα συμπιεσμένο πακέτο κάτω από το φάκελο PackageLocation. Η εφαρμογή είναι τώρα έτοιμο για ανάπτυξη, που μπορείτε να δημιουργήσετε τώρα ενός προτύπου για τη διαχείριση πόρων Azure για να το κάνετε.

###<a name="create-arm-template"></a>Δημιουργία προτύπου στο ARM
Πρώτα, ας ξεκινήσουμε με ένα βασικό πρότυπο ARM που θα δημιουργήσετε μια εφαρμογή web και ένα πρόγραμμα φιλοξενίας (Σημειώστε ότι παραμέτρους και μεταβλητές δεν εμφανίζονται για συντομία).

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Στη συνέχεια, θα πρέπει να τροποποιήσετε τον πόρο εφαρμογής web για να αποκτήσετε μια ένθετη πόρο MSDeploy. Αυτό θα σας επιτρέψει στην αναφορά του πακέτου δημιουργήσατε νωρίτερα και πείτε Azure από διαχειριστή πόρων για να χρησιμοποιήσετε MSDeploy για να αναπτύξετε το πακέτο του Azure WebApp. Η ακόλουθη εικόνα δείχνει τον πόρο Microsoft.Web/sites με τον πόρο ένθετη MSDeploy:

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Τώρα θα παρατηρήσετε ότι ο πόρος MSDeploy λαμβάνει μια ιδιότητα **packageUri** , η οποία καθορίζεται ως εξής:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

Αυτό **packageUri** μεταφέρει το uri λογαριασμού χώρου αποθήκευσης που οδηγεί στο λογαριασμό χώρου αποθήκευσης όπου θα αποστείλετε το πακέτο zip για να. Η διαχείριση πόρων Azure θα αξιοποιήσετε [Θέσει σε κοινή χρήση υπογραφών πρόσβασης](../storage/storage-dotnet-shared-access-signature-part-1.md) ώστε να έλκει το πακέτο προς τα κάτω τοπικά από το λογαριασμό χώρου αποθήκευσης κατά την ανάπτυξη του προτύπου. Αυτή η διαδικασία θα είναι αυτοματοποιημένη μέσω του PowerShell μια δέσμη ενεργειών που θα αποστείλετε το πακέτο και καλέστε το API διαχείρισης Azure για να δημιουργήσετε τα πλήκτρα απαιτείται και μεταβιβάζουν εκείνες στο πρότυπο ως παράμετροι (*_artifactsLocation* και *_artifactsLocationSasToken*). Θα πρέπει να ορίσετε τις παραμέτρους για το φάκελο και όνομα αρχείου του πακέτου αποστέλλεται στο κάτω από το κοντέινερ χώρου αποθήκευσης.

Επόμενο πρέπει να προσθέσετε σε ένα άλλο ένθετη πόρων για να ρυθμίσετε τις συνδέσεις όνομα κεντρικού υπολογιστή για να αξιοποιήσετε έναν προσαρμοσμένο τομέα. Θα πρέπει πρώτα να διασφαλίσετε ότι είστε κάτοχος το όνομα κεντρικού υπολογιστή και τη ρυθμίσετε ώστε να είναι Επαληθεύτηκε από Azure ότι είστε ο κάτοχος του - ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ενός προσαρμοσμένου ονόματος τομέα στο Azure εφαρμογής υπηρεσίας](web-sites-custom-domain-name.md). Μετά την ολοκλήρωση που μπορείτε να προσθέσετε τα εξής στο πρότυπό σας, στην ενότητα Microsoft.Web/sites πόρων:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Τέλος, πρέπει να προσθέσετε έναν άλλο πόρο επάνω επιπέδου, Microsoft.Web/certificates. Αυτός ο πόρος θα περιέχει το πιστοποιητικό SSL και θα υπάρχει στο ίδιο επίπεδο με το web app και το πρόγραμμα φιλοξενίας.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

Θα πρέπει να έχετε ένα έγκυρο πιστοποιητικό SSL προκειμένου να ρυθμίσετε αυτόν τον πόρο. Όταν έχετε ότι έγκυρη το πιστοποιητικό, στη συνέχεια, πρέπει να εξαγάγετε τα byte pfx ως συμβολοσειρά base64. Μία επιλογή για να εξαγάγετε αυτό είναι να χρησιμοποιήσετε την παρακάτω εντολή PowerShell:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Μπορείτε να, στη συνέχεια, μεταβιβάσετε αυτό ως παράμετρο στο πρότυπό σας ARM ανάπτυξης.

Σε αυτό το σημείο το πρότυπο ARM είναι έτοιμο.

###<a name="deploy-template"></a>Ανάπτυξη προτύπου

Τα τελικά βήματα είναι να τεμάχιο αυτό όλες μαζί σε μια ανάπτυξη πλήρη για ολοκληρωμένες. Για να διευκολύνετε την ανάπτυξη μπορείτε να αξιοποιήσετε τη δέσμη ενεργειών PowerShell **Ανάπτυξη AzureResourceGroup.ps1** που έχει προστεθεί κατά τη δημιουργία ενός έργου, η ομάδα πόρων Azure στο Visual Studio για να σας βοηθήσει με την αποστολή της οποιαδήποτε αντικείμενα που απαιτούνται για το πρότυπο. Απαιτεί να έχετε δημιουργήσει ένα λογαριασμό χώρου αποθήκευσης που θέλετε να χρησιμοποιήσετε εκ των προτέρων. Για αυτό το παράδειγμα, να δημιουργήσει ένα λογαριασμό κοινόχρηστου χώρου αποθήκευσης για το package.zip να αποσταλεί. Η δέσμη ενεργειών θα αξιοποιήσετε AzCopy για να αποστείλετε το πακέτο με το λογαριασμό χώρου αποθήκευσης. Περάσετε στη θέση του φακέλου σας αντικείμενο και τη δέσμη ενεργειών θα αποσταλούν αυτόματα όλα τα αρχεία μέσα σε αυτόν τον κατάλογο στο κοντέινερ με όνομα χώρου αποθήκευσης. Μετά την κλήση ανάπτυξη AzureResourceGroup.ps1 πρέπει να, στη συνέχεια, ενημερώστε τις συνδέσεις SSL για να αντιστοιχίσετε το προσαρμοσμένο όνομα κεντρικού υπολογιστή με το πιστοποιητικό SSL.

Το παρακάτω PowerShell εμφανίζει την πλήρη ανάπτυξη καλώντας την ανάπτυξη-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

Σε αυτό το σημείο την εφαρμογή σας θα πρέπει να έχουν αναπτυχθεί και θα πρέπει να μπορείτε να περιηγηθείτε σε αυτήν μέσω https://www.yourcustomdomain.com
