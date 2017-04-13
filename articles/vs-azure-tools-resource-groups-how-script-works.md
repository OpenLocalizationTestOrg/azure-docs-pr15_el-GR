<properties
    pageTitle="Επισκόπηση της δέσμης ενεργειών ανάπτυξης Azure ομάδα πόρων έργου | Microsoft Azure"
    description="Περιγράφει πώς λειτουργεί η δέσμη ενεργειών PowerShell του έργου ανάπτυξης Azure ομάδα πόρων."
    services="visual-studio-online"
    documentationCenter="na"
    authors="tfitzmac"
    manager="timlt"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/26/2016"
    ms.author="tomfitz" />

# <a name="overview-of-the-azure-resource-group-project-deployment-script"></a>Επισκόπηση της δέσμης ενεργειών ανάπτυξης Azure ομάδα πόρων έργου

Ομάδα πόρων Azure ανάπτυξης έργα σας βοηθούν να σταδίου και να αναπτύξετε τα αρχεία και άλλα αντικείμενα σε Azure. Όταν δημιουργείτε ένα έργο διαχείρισης πόρων Azure ανάπτυξης στο Visual Studio, μια δέσμη ενεργειών του PowerShell που ονομάζεται **Ανάπτυξη AzureResourceGroup.ps1** προστίθεται στο έργο. Αυτό το θέμα παρέχει λεπτομέρειες σχετικά με το τι κάνει αυτή η δέσμη ενεργειών και πώς μπορείτε να το εκτελέσει εντός και εκτός του Visual Studio.

## <a name="what-does-the-script-do"></a>Τι κάνει η δέσμη ενεργειών;

Η δέσμη ενεργειών ανάπτυξης AzureResourceGroup.ps1 εκτελεί δύο πράγματα που είναι σημαντικά για τη ροή εργασίας ανάπτυξης.

- Αποστείλετε τα αρχεία και τα αντικείμενα που χρειάζονται για την ανάπτυξη προτύπου
- Ανάπτυξη του προτύπου

Το πρώτο τμήμα της δέσμης ενεργειών μεταφέρει τα αρχεία και τα αντικείμενα για ανάπτυξη και το τελευταίο cmdlet στη δέσμη ενεργειών ανάπτυξης στην πραγματικότητα το πρότυπο. Για παράδειγμα, εάν μια εικονική μηχανή πρέπει να ρυθμιστούν με μια δέσμη ενεργειών, η δέσμη ενεργειών ανάπτυξης πρώτα με ασφάλεια αποστέλλει τη δέσμη ενεργειών ρύθμισης παραμέτρων για ένα λογαριασμό Azure χώρου αποθήκευσης. Έτσι, είναι διαθέσιμες για τη διαχείριση πόρων Azure για τη ρύθμιση παραμέτρων η εικονική μηχανή κατά την προμήθεια του.

Επειδή δεν όλες τις αναπτύξεις πρότυπο πρέπει να έχουν επιπλέον αντικείμενα που πρέπει να αποσταλεί, αξιολόγηση της παραμέτρου αλλαγής που ονομάζεται *uploadArtifacts* . Εάν οποιαδήποτε αντικείμενα πρέπει να αποσταλεί, ορίστε το διακόπτη *uploadArtifacts* κατά την κλήση της δέσμης ενεργειών. Σημειώστε ότι το αρχείο προτύπου κύριο και το αρχείο παραμέτρων δεν πρέπει να αποσταλεί. Μόνο άλλα αρχεία, όπως δέσμες ενεργειών ρύθμισης παραμέτρων, ένθετα πρότυπα ανάπτυξης και αρχεία εφαρμογών πρέπει να αποσταλεί.

## <a name="detailed-script-description"></a>Δέσμη ενεργειών λεπτομερή περιγραφή

Ακολουθεί μια περιγραφή των ποια επιλογή ενοτήτων του δέσμης ενεργειών του PowerShell Azure AzureResourceGroup.ps1 ανάπτυξη.

>[AZURE.NOTE] Αυτό περιγράφει τη δέσμη ενεργειών ανάπτυξης AzureResourceGroup.ps1 έκδοση 1.0.

1.  Δηλώστε παραμέτρους που απαιτούνται από διαχειριστή πόρων Azure ανάπτυξη του έργου. Ορισμένες παράμετροι έχουν προεπιλεγμένες τιμές που έχουν οριστεί κατά τη δημιουργία του έργου. Μπορείτε να αλλάξετε αυτές τις προεπιλεγμένες τιμές στη δέσμη ενεργειών ή να προσθέσετε διαφορετικές τιμές παραμέτρου πριν από την εκτέλεση της δέσμης ενεργειών.

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

  	|Παράμετρος|Περιγραφή|
  	|---|---|
  	|$ResourceGroupLocation|Η περιοχή ή δεδομένα κέντρο θέση για την ομάδα πόρων, όπως **Δυτική ΗΠΑ** ή **Ανατολικής Ασίας**.|
  	|$ResourceGroupName|Το όνομα της ομάδας Azure πόρων.|
  	|$UploadArtifacts|Μια δυαδική τιμή που υποδεικνύει εάν αντικείμενα πρέπει να αποσταλεί Azure από το σύστημά σας.|
  	|$StorageAccountName|Το όνομα του λογαριασμού σας Azure αποθήκευσης όπου έχουν αποσταλεί σας αντικείμενα.|
  	|$StorageAccountResourceGroupName|Το όνομα της ομάδας πόρων Azure που περιέχει το λογαριασμό χώρου αποθήκευσης.|
  	|$StorageContainerName|Το όνομα του κοντέινερ χώρου αποθήκευσης χρησιμοποιούνται για την αποστολή αντικείμενα.|
  	|$TemplateFile|Τη διαδρομή προς το αρχείο ανάπτυξης (`<app name>.json`) στο έργο σας ομάδα πόρων Azure.|
  	|$TemplateParametersFile|Τη διαδρομή προς το αρχείο παραμέτρων (`<app name>.parameters.json`) στο έργο σας ομάδα πόρων Azure.|
  	|$ArtifactStagingDirectory|Η διαδρομή του συστήματός σας όπου αντικείμενα είναι τοπικά αποσταλεί, συμπεριλαμβανομένων των στον ριζικό φάκελο δέσμη ενεργειών PowerShell. Αυτή η διαδρομή μπορεί να είναι απόλυτες ή σε σχέση με τη θέση της δέσμης ενεργειών.|
  	|$AzCopyPath|Η διαδρομή όπου το εργαλείο AzCopy.exe αντιγράφει τα αρχεία .zip, καθώς και τον ριζικό φάκελο δέσμη ενεργειών PowerShell. Αυτή η διαδρομή μπορεί να είναι απόλυτες ή σε σχέση με τη θέση της δέσμης ενεργειών.|
  	|$DSCSourceFolder|Η διαδρομή για το φάκελο προέλευσης DSC (επιθυμητοί κατάσταση ρύθμιση παραμέτρων), όπως τον ριζικό φάκελο δέσμης ενεργειών του PowerShell. Αυτή η διαδρομή μπορεί να είναι απόλυτες ή σε σχέση με τη θέση της δέσμης ενεργειών. Ανατρέξτε στο θέμα [Εισαγωγή την επέκταση DSC PowerShell Azure (επιθυμητοί κατάσταση ρύθμιση παραμέτρων)](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx), εάν χρειάζεται, για περισσότερες πληροφορίες.|

1.  Ελέγξτε για να δείτε εάν αντικείμενα πρέπει να αποσταλεί Azure. Εάν όχι, μεταβείτε στο βήμα 11. Διαφορετικά, ακολουθήστε τα παρακάτω βήματα.

1.  Μετατροπή οποιεσδήποτε μεταβλητές με σχετικές διαδρομές σε απόλυτες διαδρομές. Για παράδειγμα, να αλλάξετε μια διαδρομή όπως `..\Tools\AzCopy.exe` να `C:\YourFolder\Tools\AzCopy.exe`. Επίσης, η προετοιμασία των μεταβλητών *ArtifactsLocationName* και *ArtifactsLocationSasTokenName* σε null. *ArtifactsLocation* και *SaSToken* μπορεί να τις παραμέτρους για το πρότυπο. Εάν τις τιμές τους είναι null μετά ανάγνωσης στο αρχείο παραμέτρων, η δέσμη ενεργειών δημιουργεί τιμών για τους.

    Τα εργαλεία Azure χρησιμοποιήστε τις τιμές παραμέτρων *_artifactsLocation* και *_artifactsLocationSasToken* στο πρότυπο για να διαχειριστείτε αντικείμενα. Εάν η δέσμη ενεργειών PowerShell εντοπίζει τις παραμέτρους με αυτά τα ονόματα, αλλά δεν παρέχονται τις τιμές παραμέτρων, η δέσμη ενεργειών αποστέλλει τα αντικείμενα και επιστρέφει τις κατάλληλες τιμές για αυτές τις παραμέτρους. Στη συνέχεια, περάσει για τους στο cmdlet μέσω `@OptionsParameters`.

  	|Μεταβλητή|Περιγραφή|
  	|---|---|
  	|ArtifactsLocationName|Η διαδρομή όπου βρίσκονται τα αντικείμενα Azure.|
  	|ArtifactsLocationSasTokenName|Το όνομα διακριτικού συσχετισμών Ασφαλείας (κοινόχρηστα υπογραφή πρόσβασης) που χρησιμοποιείται από τη δέσμη ενεργειών για τον έλεγχο ταυτότητας Bus υπηρεσίας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Θέσει σε κοινή χρήση Access υπογραφή τον έλεγχο ταυτότητας με Bus υπηρεσίας](service-bus-shared-access-signature-authentication.md) .|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  Αυτό ενότητα ελέγχει εάν το <app name>. parameters.json αρχείο (αναφέρεται ως "αρχείο παραμέτρων") έχει ένα γονικό κόμβο με το όνομα **παράμετροι** (στο το `else` μπλοκ). Διαφορετικά, που έχει χωρίς γονικό κόμβο. Μορφή είναι αποδεκτή.
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  Διαδοχικές προσεγγίσεις μέσα στη συλλογή των παραμέτρων JSON. Εάν έχει εκχωρηθεί μια τιμή παραμέτρου για να *_artifactsLocation* ή *_artifactsLocationSasToken*, στη συνέχεια, ορίστε τη μεταβλητή *$OptionalParameters* με αυτές τις τιμές. Αυτό αποτρέπει τη δέσμη ενεργειών από αντικαταστήσει κατά λάθος τις τιμές παραμέτρων που παρέχετε.

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  Λάβετε το κλειδί λογαριασμού χώρου αποθήκευσης και περιβάλλον για τον πόρο λογαριασμού χώρου αποθήκευσης που χρησιμοποιούνται για τη διατήρηση τα αντικείμενα για ανάπτυξη.

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  Εάν χρησιμοποιείτε το PowerShell DSC για να ρυθμίσετε μια εικονική μηχανή, την επέκταση DSC απαιτεί τα αντικείμενα σε ένα αρχείο zip μία. Επομένως, δημιουργήστε ένα αρχείο αρχειοθέτησης .zip για τη ρύθμιση παραμέτρων DSC. Για να το κάνετε αυτό, ελέγξτε εάν υπάρχει $DSCSourceFolder. Εάν υπάρχει μια ρύθμιση παραμέτρων DSC, καταργήστε την και, στη συνέχεια, δημιουργήστε ένα νέο αρχείο συμπιεσμένο που ονομάζεται dsc.zip.

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Εάν δεν υπάρχει διαδρομή για Azure αντικείμενα παρέχεται στο αρχείο παραμέτρων, ορίστε μια διαδρομή για τη δέσμη ενεργειών του PowerShell για χρήση κατά την αποστολή αντικείμενα. Για να το κάνετε αυτό, δημιουργήστε μια διαδρομή χρησιμοποιώντας ένα συνδυασμό των διαδρομή του λογαριασμού του χώρου αποθήκευσης του τελικού σημείου συν το όνομα κοντέινερ χώρου αποθήκευσης. Στη συνέχεια, μπορείτε να ενημερώσετε το αρχείο παράμετροι με αυτήν τη νέα διαδρομή.

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  Χρησιμοποιήστε το βοηθητικό πρόγραμμα **AzCopy** (περιλαμβάνεται στο φάκελο " **Εργαλεία** " του έργου σας ομάδα πόρων Azure ανάπτυξης) για να αντιγράψετε τα αρχεία από την τοπική διαδρομή απόθεσης χώρου αποθήκευσης στο λογαριασμό σας στο online αποθήκευσης Azure. Εάν αυτό το βήμα αποτύχει, κλείστε τη δέσμη ενεργειών από την ανάπτυξη δεν είναι πιθανό να αποτύχει χωρίς την απαιτούμενη αντικείμενα.

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  Εάν δεν παρέχεται ένα διακριτικό συσχετισμών Ασφαλείας για τη θέση αντικείμενα στο αρχείο παραμέτρων, δημιουργήστε ένα για την παροχή προσωρινή πρόσβαση μόνο για ανάγνωση στο online κοντέινερ χώρου αποθήκευσης. Στη συνέχεια, μεταβιβάζουν που διακριτικό συσχετισμών Ασφαλείας με το cmdline ως ένα "optionalParameter." Σημειώστε ότι οποιεσδήποτε παραμέτρους αντανάκλαση, το cmdline θα προηγούνται των τιμών που παρέχονται στο αρχείο παραμέτρων.

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  Δημιουργήστε την ομάδα των πόρων, εάν δεν είναι ήδη υπάρχει και ελέγξτε το αρχείο προτύπου και τις παραμέτρους για τυχόν σφάλματα επικύρωσης που θα αποτρέψει την ανάπτυξη της.

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. Τέλος, αναπτύξτε το πρότυπο. Αυτός ο κώδικας δημιουργεί ένα μοναδικό όνομα για την ανάπτυξη χρησιμοποιώντας μια χρονική σήμανση.

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## <a name="deploy-the-resource-group"></a>Ανάπτυξη της ομάδας πόρων

### <a name="to-deploy-the-resource-group-in-visual-studio"></a>Για να αναπτύξετε την ομάδα των πόρων στο Visual Studio

1. Στο μενού συντόμευσης του Azure ομάδα πόρων έργου, επιλέξτε **Ανάπτυξη** > **Νέας ανάπτυξης**.

    ![][0]

1. Στο παράθυρο διαλόγου **ανάπτυξη σε ομάδα πόρων** , είτε επιλέξτε μια υπάρχουσα ομάδα πόρων στο αναπτυσσόμενο πλαίσιο λίστας για να αναπτύξετε ή να επιλέξετε ** &lt;Δημιουργία νέου... &gt;** Για να δημιουργήσετε μια νέα ομάδα πόρων.

    ![][1]

1. Εάν σας ζητηθεί, πληκτρολογήστε ένα όνομα ομάδας πόρων και μια θέση στο παράθυρο διαλόγου **Δημιουργία ομάδα πόρων** και, στη συνέχεια, επιλέξτε το κουμπί " **Δημιουργία** ".

    ![][2]

1. Επιλέξτε το κουμπί **Επεξεργασία παραμέτρους** για να προβάλετε το παράθυρο διαλόγου **Παράμετροι επεξεργασία** και, στη συνέχεια, πληκτρολογήστε τις τιμές παραμέτρων που λείπουν.

    ![][3]

    >[AZURE.NOTE] Εάν οποιεσδήποτε απαραίτητες παραμέτρους χρειάζεστε τιμές, αυτό το παράθυρο διαλόγου εμφανίζεται αυτόματα κατά την ανάπτυξη.

    ![][4]

1. Όταν ολοκληρώσετε τη διαδικασία εισαγάγετε τιμές παραμέτρων, επιλέξτε το κουμπί " **Αποθήκευση** " και, στη συνέχεια, επιλέξτε το κουμπί " **Ανάπτυξη** ".

    Εκτελείται η δέσμη ενεργειών ανάπτυξης (ανάπτυξη-AzureResourceGroup.ps1) και σας πρότυπο, μαζί με οποιαδήποτε αντικείμενα, ανάπτυξη σε Azure.

### <a name="to-deploy-the-resource-group-by-using-powershell"></a>Για να αναπτύξετε την ομάδα των πόρων με χρήση του PowerShell

Εάν θέλετε να εκτελέσετε τη δέσμη ενεργειών χωρίς να χρησιμοποιήσετε την εντολή ανάπτυξη του Visual Studio και το περιβάλλον εργασίας Χρήστη, στο μενού συντόμευσης για τη δέσμη ενεργειών, επιλέξτε **Άνοιγμα με το PowerShell ISE**.

![][5]


## <a name="command-deployment-examples"></a>Παραδείγματα ανάπτυξης εντολών

### <a name="deploy-using-default-values"></a>Ανάπτυξη χρησιμοποιώντας προεπιλεγμένες τιμές

Αυτό το παράδειγμα δείχνει πώς μπορείτε να εκτελέσετε τη δέσμη ενεργειών χρησιμοποιώντας τις προεπιλεγμένες τιμές παραμέτρων. (Επειδή η παράμετρος θέση δεν έχει μια προεπιλεγμένη τιμή, πρέπει να δώσετε ένα.)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### <a name="deploy-overriding-the-default-values"></a>Ανάπτυξη παράκαμψη τις προεπιλεγμένες τιμές

Αυτό το παράδειγμα δείχνει πώς μπορείτε να εκτελέσετε τη δέσμη ενεργειών για την ανάπτυξη του προτύπου και τις παραμέτρους αρχείων που διαφέρουν από τις προεπιλεγμένες τιμές.

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### <a name="deploy-using-uploadartifacts-for-staging"></a>Ανάπτυξη χρησιμοποιώντας UploadArtifacts για ενδιάμεσου σταδίου

Αυτό το παράδειγμα δείχνει πώς μπορείτε να εκτελέσετε τη δέσμη ενεργειών για την αποστολή αντικείμενα από το φάκελο κυκλοφορίας και ανάπτυξη μη προεπιλεγμένων προτύπων.

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

Αυτό το παράδειγμα δείχνει πώς μπορείτε να εκτελέσετε τη δέσμη ενεργειών σε μια εργασία του PowerShell Azure στο Visual Studio Online.

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## <a name="next-steps"></a>Επόμενα βήματα
Μάθετε περισσότερα σχετικά με τη διαχείριση πόρων Azure [Επισκόπηση της διαχείρισης πόρων Azure](azure-resource-manager/resource-group-overview.md).

Για περισσότερα παραδείγματα με την εργασία με έργα ομάδα πόρων Azure, ανατρέξτε στο θέμα [ανάπτυξη και διαχείριση Azure πόρους](https://github.com/Microsoft/HealthClinic.biz/wiki/Deploy-and-manage-Azure-resources) από τη σύνδεση [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [επίδειξη](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Για περισσότερες γρήγορες εκκινήσεις από την επίδειξη HealthClinic.biz, ανατρέξτε στο θέμα [Γρήγορες εκκινήσεις εργαλεία για προγραμματιστές Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
