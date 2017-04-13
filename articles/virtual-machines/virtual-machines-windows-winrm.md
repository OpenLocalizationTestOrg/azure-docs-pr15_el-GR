<properties
    pageTitle="Ρύθμιση πρόσβασης των WinRM για εικονικές μηχανές στη Διαχείριση πόρων Azure | Microsoft Azure"
    description="Τρόπος εγκατάστασης του WinRM πρόσβαση για χρήση με ένα διαχειριστή πόρων Azure εικονική μηχανή"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="singhkay"/>

# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Ρύθμιση πρόσβασης των WinRM για εικονικές μηχανές στη Διαχείριση πόρων Azure

## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>WinRM στη Διαχείριση υπηρεσίας Azure και στο διαχειριστή πόρων Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης

* Για μια επισκόπηση της διαχείρισης πόρων Azure, ανατρέξτε σε αυτό [το άρθρο](../azure-resource-manager/resource-group-overview.md)
* Για τις διαφορές μεταξύ Azure υπηρεσίας διαχείρισης και διαχείριση πόρων Azure, ανατρέξτε στο θέμα σε αυτό το [άρθρο](../resource-manager-deployment-model.md)

Η βασική διαφορά στη ρύθμιση παραμέτρων WinRM μεταξύ τις δύο στοίβες είναι πώς το πιστοποιητικό λαμβάνει εγκατεστημένο στον η Εικονική. Στη Διαχείριση πόρων Azure στοίβα, τα πιστοποιητικά είναι διαμορφωθεί ως πόρους που ελέγχονται από την υπηρεσία παροχής του αριθμού-κλειδιού θάλαμο πόρων. Επομένως, ο χρήστης πρέπει να παρέχετε τις δικές τους πιστοποιητικό και στείλτε το στο θάλαμο ένα πλήκτρο πριν να το χρησιμοποιήσετε σε μια Εικονική.

Ακολουθούν τα βήματα που πρέπει να ακολουθήσετε για να ρυθμίσετε μια Εικονική με WinRM συνδεσιμότητας

1. Δημιουργία ενός κλειδιού θάλαμο
2. Δημιουργία πιστοποιητικού αυτόματης υπογραφής
3. Αποστείλετε το πιστοποιητικό αυτόματης υπογραφής στο θάλαμο αριθμού-κλειδιού
4. Λήψη της διεύθυνσης URL για το πιστοποιητικό αυτόματης υπογραφής στο θάλαμο του αριθμού-κλειδιού
5. Αναφορά διεύθυνσης URL αυτο-υπογεγραμμένο πιστοποιητικά κατά τη δημιουργία μια εικονική Μηχανή

## <a name="step-1-create-a-key-vault"></a>Βήμα 1: Δημιουργία ενός κλειδιού θάλαμο

Μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή για να δημιουργήσετε το θάλαμο αριθμού-κλειδιού

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Βήμα 2: Δημιουργία πιστοποιητικού αυτόματης υπογραφής
Μπορείτε να δημιουργήσετε ένα αυτο-υπογεγραμμένο πιστοποιητικό χρησιμοποιώντας αυτήν τη δέσμη ενεργειών PowerShell

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Βήμα 3: Αποστολή σας αυτο-υπογεγραμμένο πιστοποιητικό θάλαμο του αριθμού-κλειδιού

Πριν από την αποστολή του πιστοποιητικού για να το θάλαμο αριθμό-κλειδί που δημιουργήσατε στο βήμα 1, πρέπει να έχει μετατραπεί σε μια μορφή που θα είναι κατανοητή από την υπηρεσία παροχής Microsoft.Compute πόρων. Το παρακάτω PowerShell δέσμης ενεργειών επιτρέπει αυτήν

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Βήμα 4: Λήψη της διεύθυνσης URL για το πιστοποιητικό αυτόματης υπογραφής στο θάλαμο του αριθμού-κλειδιού

Η υπηρεσία παροχής πόρων Microsoft.Compute πρέπει μια διεύθυνση URL για το μυστικό εντός του αριθμού-κλειδιού θάλαμο κατά την προμήθεια του Εικονική. Αυτή η δυνατότητα επιτρέπει την υπηρεσία παροχής πόρων Microsoft.Compute να κάνετε λήψη το μυστικό της και να δημιουργήσετε το πιστοποιητικό ισοδύναμη σε η Εικονική.

>[AZURE.NOTE]Η διεύθυνση URL του το μυστικό πρέπει να περιλαμβάνει επίσης την έκδοση. Ένα παράδειγμα διεύθυνσης URL μοιάζει με κάτω από το https://contosovault.vault.azure.net:443/απόρρητο/contososecret/01h9db0df2cd4300a20ence585a6s7ve


#### <a name="templates"></a>Πρότυπα

Μπορείτε να μεταβείτε στη σύνδεση για τη διεύθυνση URL του προτύπου χρησιμοποιώντας την παρακάτω κώδικα

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell

Μπορείτε να λάβετε αυτό με χρήση διεύθυνσης URL την παρακάτω εντολή PowerShell

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Βήμα 5: Αναφορά διεύθυνσης URL αυτο-υπογεγραμμένο πιστοποιητικά κατά τη δημιουργία μια εικονική Μηχανή

#### <a name="azure-resource-manager-templates"></a>Azure πρότυπα διαχείρισης πόρων

Κατά τη δημιουργία μια Εικονική πρότυπα, το πιστοποιητικό λαμβάνει αναφέρεται στην ενότητα απόρρητο και την ενότητα winRM ως εξής:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Δείγμα προτύπου για τα παραπάνω μπορείτε να βρείτε εδώ στο [201-εικονική-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

Πηγαίος κώδικας για αυτό το πρότυπο μπορεί να βρεθεί στην [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell

    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Βήμα 6: Σύνδεση με την εικονική Μηχανή
Πριν από την μπορείτε να συνδεθείτε με την εικονική Μηχανή θα πρέπει να βεβαιωθείτε ότι ο υπολογιστής σας έχει ρυθμιστεί για WinRM απομακρυσμένης διαχείρισης. Εκκίνηση του PowerShell ως διαχειριστής και εκτελέστε την παρακάτω εντολή για να βεβαιωθείτε ότι είστε έτοιμοι.

    Enable-PSRemoting -Force

>[AZURE.NOTE] Ίσως χρειαστεί να βεβαιωθείτε ότι εκτελείται η υπηρεσία WinRM εάν τα παραπάνω δεν λειτουργεί. Μπορείτε να κάνετε αυτό με χρήση`Get-Service WinRM`

Μόλις ολοκληρωθεί η εγκατάσταση, μπορείτε να συνδεθείτε στο Εικονική χρησιμοποιώντας την παρακάτω εντολή

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate