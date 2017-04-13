<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure στοίβας κλειδιού θάλαμο | Microsoft Azure"
    description="Γρήγορα αποτελέσματα με το θάλαμο κλειδί στοίβας Azure"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ricardom"/>


# <a name="getting-started-with-key-vault"></a>Γρήγορα αποτελέσματα με το πλήκτρο θάλαμο

Αυτή η ενότητα περιγράφει τα βήματα για να δημιουργήσετε ένα θάλαμο, Διαχείριση πλήκτρα και απόρρητο καθώς και επιτρέπουν στους χρήστες ή εφαρμογές για να καλέσετε πράξεις σε το θάλαμο σε στοίβα Azure. Ακολουθήστε τα παρακάτω βήματα θεωρείται ότι υπάρχει μια συνδρομή μισθωτή και υπηρεσία KeyVault έχει καταχωρηθεί μέσα σε αυτήν τη συνδρομή. Όλες οι εντολές παράδειγμα βασίζονται στις τα cmdlet KeyVault διαθέσιμη ως μέρος του Azure PowerShell SDK.

## <a name="enabling-the-tenant-subscription-for-vault-operations"></a>Ενεργοποίηση τη συνδρομή μισθωτή για λειτουργίες θάλαμο 

Πριν να εκδώσετε λειτουργίες σε σχέση με οποιαδήποτε θάλαμο, πρέπει να βεβαιωθείτε ότι είναι ενεργοποιημένη η συνδρομή σας για λειτουργίες θάλαμο. Μπορείτε να επιβεβαιώσετε που με την έκδοση την παρακάτω εντολή PowerShell:

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -AutoSize

Το αποτέλεσμα της εντολής παραπάνω πρέπει να αναφέρετε "Καταχωρημένο" για κάθε γραμμή της κατάστασης "Καταχώρηση".

    ProviderNamespace RegistrationState ResourceTypes Locations
    Microsoft.KeyVault Registered {operations} {local}
    Microsoft.KeyVault Registered {vaults} {local}
    Microsoft.KeyVault Registered {vaults/secrets} {local}
    

 Εάν που δεν είναι πεζών και κεφαλαίων γραμμάτων, θα πρέπει να καλέσετε την παρακάτω εντολή για να καταχωρήσετε την υπηρεσία KeyVault εντός τη συνδρομή σας:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

Και τα παρακάτω είναι το αποτέλεσμα της εντολής:
    
    ProviderNamespace : Microsoft.KeyVault
    RegistrationState : Registered
    ResourceTypes : {operations, vaults, vaults/secrets}
    Locations : {local}


>[AZURE.NOTE] Εάν λάβετε το μήνυμα σφάλματος: "*η συνδρομή δεν έχει καταχωρηθεί με θάλαμο κλειδί Azure*" κατά την κλήση των cmdlet KeyVault, επιβεβαιώστε που έχετε ενεργοποιήσει την υπηρεσία παροχής του πόρου KeyVault ανά παραπάνω οδηγίες.


## <a name="creating-a-hardened-container-a-vault-in-azure-stack-to-store-and-manage-cryptographic-keys-and-secrets"></a>Δημιουργία ενός κοντέινερ υψηλό επίπεδο ασφαλείας (ένα θάλαμο) σε στοίβα Azure για την αποθήκευση και διαχείριση των κλειδιών κρυπτογράφησης και απορρήτου

Για να δημιουργήσετε ένα θάλαμο, μισθωτή πρέπει πρώτα να δημιουργήσετε μια ομάδα πόρων. Τις παρακάτω εντολές του PowerShell Δημιουργήστε μια ομάδα πόρων και, στη συνέχεια, ένα θάλαμο στη συγκεκριμένη ομάδα πόρων. Στο παράδειγμα περιλαμβάνει επίσης την τυπική έξοδο από αυτό το cmdlet.

### <a name="creating-a-resource-group"></a>Δημιουργία μιας ομάδας πόρων:

    New-AzureRmResourceGroup -Name vaultrg010 -Location local -Verbose -Force

Αποτέλεσμα:

    VERBOSE: Performing the operation "Replacing resource group ..." on target "".
    VERBOSE: 12:52:51 PM - Created resource group 'vaultrg010' in location 'local'
    ResourceGroupName : vaultrg010
    Location : local
    ProvisioningState : Succeeded
    Tags :
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010
    

### <a name="creating-a-vault"></a>Δημιουργία μιας θάλαμο:


    New-AzureRmKeyVault -VaultName vault010 -ResourceGroupName vaultrg010 -Location local -Verbose

Αποτέλεσμα:

    VaultUri : https://vault010.vault.azurestack.local
    TenantId : 5454420b-2e38-4b9e-8b56-1712d321cf33
    TenantName : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Sku : Standard
    EnabledForDeployment : False
    EnabledForTemplateDeployment : False
    EnabledForDiskEncryption : False
    AccessPolicies : {5454420b-2e38-4b9e-8b56-1712d321cf33}
    AccessPoliciesText :
    Tenant ID : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Object ID : ca342e90-f6aa-435b-a11c-dfe5ef0bfeeb
    Application ID :
    Display Name : Tenant Admin (tenantadmin1@msazurestack.onmicrosoft.com)
    Permissions to Keys : get, create, delete, list, update, import, backup, restore
    Permissions to Secrets : all
    OriginalVault : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010/providers/Microsoft.KeyVault/vaults/vault010
    VaultName : vault010
    ResourceGroupName : vaultrg010
    Location : local
    Tags : {}
    TagsTable :
    
Το αποτέλεσμα της αυτό το cmdlet εμφανίζει ιδιοτήτων του κλειδιού θάλαμο που μόλις δημιουργήσατε. Οι δύο σημαντικότερες ιδιότητες είναι οι εξής:

-   **Όνομα θάλαμο**: στο παράδειγμα, αυτό είναι **vault010**. Θα χρησιμοποιήσετε αυτό το όνομα για άλλες cmdlet του αριθμού-κλειδιού θάλαμο.

-   **URI θάλαμο**: στο παράδειγμα, αυτό είναι https://vault010.vault.azurestack.local. Εφαρμογές που χρησιμοποιούν το θάλαμο έως το REST API πρέπει να χρησιμοποιήσετε αυτό URI.

Azure το λογαριασμό σας τώρα είναι εξουσιοδοτημένοι να εκτελούν τις λειτουργίες σε αυτό κλειδιού θάλαμο. Προς το παρόν, κανείς δεν είναι.


## <a name="operating-on-keys-and-secrets"></a>Λειτουργικό σε πλήκτρα και απορρήτου

Αφού δημιουργήσετε ένα θάλαμο, ακολουθήστε τα παρακάτω βήματα για τη δημιουργία, Διαχείριση πλήκτρα και απορρήτου:

### <a name="creating-a-key"></a>Δημιουργώντας ένα κλειδί

Για να δημιουργήσετε έναν αριθμό-κλειδί, χρησιμοποιήστε το **Πρόσθετο AzureKeyVaultKey** ανά το παρακάτω παράδειγμα. Μετά την επιτυχή δημιουργία βασικών, το cmdlet θα εξόδου τις σημαντικές λεπτομέρειες που έχουν δημιουργηθεί πρόσφατα.

    Add-AzureKeyVaultKey -VaultName \$vaultName -Name\$keyVaultKeyName -Verbose -Destination Software

Το ακόλουθο είναι το αποτέλεσμα της το cmdlet *Προσθήκη AzureKeyVaultKey* :

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff
    
Μπορείτε τώρα να αναφέρετε αυτό το κλειδί που δημιουργήσατε ή αποστολή του σε θάλαμο κλειδί Azure, χρησιμοποιώντας το URI. Χρησιμοποιήστε **πλήκτρα/https://vault010.vault.azurestack.local:443/keyVaultKeyName001** για να έχετε πάντα την τρέχουσα έκδοση; και χρησιμοποιήστε **https://vault010.vault.azurestack.local:443/πλήκτρα/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff** για να λάβετε αυτήν τη συγκεκριμένη έκδοση.

### <a name="retrieving-a-key"></a>Ανάκτηση ενός κλειδιού

Χρησιμοποιήστε το **Get-AzureKeyVaultKey** για να ανακτήσετε ένα κλειδί και τις λεπτομέρειές ανά το παρακάτω παράδειγμα:

    Get-AzureKeyVaultKey -VaultName vault010 -Name keyVaultKeyName001

Τα παρακάτω είναι το αποτέλεσμα της Get-AzureKeyVaultKey

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff

### <a name="setting-a-secret"></a>Ρύθμιση μιας μυστικό

    $secretvalue = ConvertTo-SecureString 'User@123' -AsPlainText -Force
    Set-AzureKeyVaultSecret -Name MySecret-VaultName vault010 -SecretValue $secretvalue
    
Εξόδου

    Vault Name : vault010
    Name : MySecret
    Version : 65a387f2ed4a416180e852b970846f5b
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret/65a387f2ed4a416180e852b970846f5b
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags : 

### <a name="retrieving-a-secret"></a>Ανάκτηση ενός μυστικό

    Get-AzureKeyVaultSecret -VaultName vault010

Εξόδου

    Vault Name : vault010
    Name : MySecret
    Version :
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags :

Τώρα, το πλήκτρο θάλαμο και αριθμού-κλειδιού ή μυστικό είναι έτοιμη για να χρησιμοποιήσετε τις εφαρμογές.
Πρέπει να εγκρίνετε εφαρμογές για να τις χρησιμοποιήσετε.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Εγκρίνετε την εφαρμογή για να χρησιμοποιήσετε το κλειδί ή το μυστικό 

Για να εγκρίνετε την εφαρμογή για να αποκτήσετε πρόσβαση το κλειδί ή το μυστικό στο το θάλαμο, χρησιμοποιήστε το cmdlet Set -**AzureRmKeyVaultAccessPolicy** .

Για παράδειγμα, εάν το όνομα θάλαμο είναι *ContosoKeyVault* και την εφαρμογή που θέλετε να εξουσιοδοτήσετε περιλαμβάνει ένα *Αναγνωριστικό υπολογιστή-πελάτη* του *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*και θέλετε να εγκρίνετε την εφαρμογή για να αποκρυπτογραφήσετε και συνδεθείτε με πλήκτρα στο θάλαμο σας, εκτελέστε τα εξής:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Εάν θέλετε να εγκρίνετε ίδια αίτηση για να διαβάσετε απόρρητο στο θάλαμο σας, εκτελέστε τα εξής:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get


## <a name="next-steps"></a>Επόμενα βήματα
[Ανάπτυξη μια Εικονική με κωδικό πρόσβασης θάλαμο αριθμού-κλειδιού](azure-stack-kv-deploy-vm-with-secret.md)

[Ανάπτυξη μια Εικονική με ένα πιστοποιητικό θάλαμο αριθμού-κλειδιού](azure-stack-kv-push-secret-into-vm.md)