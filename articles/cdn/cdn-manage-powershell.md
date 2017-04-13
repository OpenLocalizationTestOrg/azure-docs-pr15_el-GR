<properties
    pageTitle="Διαχείριση Azure CDN με το PowerShell | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε το cmdlet του Azure PowerShell για τη Διαχείριση Azure CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="casoper"/>


# <a name="manage-azure-cdn-with-powershell"></a>Διαχείριση Azure CDN με το PowerShell

PowerShell παρέχει μία από τις πιο ευέλικτη μεθόδους για τη Διαχείριση προφίλ Azure CDN και τα τελικά σημεία.  Μπορείτε να χρησιμοποιήσετε PowerShell αλληλεπιδραστικά ή με τη σύνταξη δεσμών ενεργειών για την αυτοματοποίηση εργασιών διαχείρισης.  Αυτό το πρόγραμμα εκμάθησης παρουσιάζει πολλές από τις πιο συνηθισμένες εργασίες που μπορείτε να εκτελέσετε με το PowerShell για τη διαχείριση των προφίλ Azure CDN και τα τελικά σημεία.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να χρησιμοποιήσετε PowerShell για τη Διαχείριση προφίλ Azure CDN και τα τελικά σημεία, πρέπει να έχετε εγκατεστημένη τη λειτουργική μονάδα Azure PowerShell.  Για να μάθετε πώς να εγκαταστήσετε το Azure PowerShell και συνδεθείτε με τη χρήση του Azure το `Login-AzureRmAccount` cmdlet, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

>[AZURE.IMPORTANT] Πρέπει να συνδεθείτε με `Login-AzureRmAccount` πριν μπορείτε να εκτελέσετε τα cmdlet του Azure PowerShell.

## <a name="listing-the-azure-cdn-cmdlets"></a>Παραθέτει τα cmdlet Azure CDN

Μπορείτε να εμφανίσετε λίστα όλα τα cmdlet Azure CDN χρησιμοποιώντας το `Get-Command` cmdlet.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Λήψη Βοήθειας

Μπορείτε να λάβετε Βοήθεια με οποιοδήποτε από τα αυτών των cmdlet χρησιμοποιώντας το `Get-Help` cmdlet.  `Get-Help`παρέχει χρήση και τη σύνταξη και προαιρετικά εμφανίζει παραδείγματα.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Καταχώρηση υπάρχοντα προφίλ Azure CDN

Το `Get-AzureRmCdnProfile` cmdlet χωρίς παραμέτρους ανακτά όλα υπάρχοντα CDN τα προφίλ σας.

```powershell
Get-AzureRmCdnProfile
```

Αυτό το αποτέλεσμα μπορούν να διοχετευθούν σε cmdlet για απαρίθμηση.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

Μπορείτε επίσης να επιστρέψετε ένα προφίλ, καθορίζοντας την ομάδα όνομα και πόρων προφίλ.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

>[AZURE.TIP] Είναι δυνατό να έχετε πολλά CDN προφίλ με το ίδιο όνομα, εφόσον είναι σε διαφορετικές ομάδες πόρων.  Παράλειψη του `ResourceGroupName` παράμετρος επιστρέφει όλα τα προφίλ με το όνομα που να ταιριάζει.

## <a name="listing-existing-cdn-endpoints"></a>Καταχώρηση υπάρχοντα CDN τα τελικά σημεία

`Get-AzureRmCdnEndpoint`να ανακτήσετε ένα τελικό σημείο μεμονωμένα ή όλα τα τελικά σημεία σε ένα προφίλ.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Δημιουργία προφίλ CDN και τα τελικά σημεία

`New-AzureRmCdnProfile`και `New-AzureRmCdnEndpoint` που χρησιμοποιούνται για τη δημιουργία προφίλ CDN και τα τελικά σημεία.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Έλεγχος διαθεσιμότητας όνομα τελικού σημείου

`Get-AzureRmCdnEndpointNameAvailability`Επιστρέφει ένα αντικείμενο που υποδεικνύει εάν υπάρχει ένα όνομα για το τελικό σημείο.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Προσθήκη προσαρμοσμένου τομέα

`New-AzureRmCdnCustomDomain`Προσθέτει ένα προσαρμοσμένο όνομα τομέα σε ένα υπάρχον τελικό σημείο.

>[AZURE.IMPORTANT] Πρέπει να ρυθμίσετε την εγγραφή CNAME με την υπηρεσία παροχής DNS, όπως περιγράφεται στο θέμα [Πώς να αντιστοιχίσετε προσαρμοσμένο τομέα τελικό σημείο περιεχομένου παράδοσης δικτύου (CDN)](./cdn-map-content-to-custom-domain.md).  Μπορείτε να ελέγξετε την αντιστοίχιση πριν από την τροποποίηση σας χρησιμοποιώντας το τελικό σημείο `Test-AzureRmCdnCustomDomain`.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Τροποποίηση ενός ορίου

`Set-AzureRmCdnEndpoint`τροποποιεί μια υπάρχουσα τελικού σημείου.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Εκκαθάριση/προ-loading CDN περιουσιακών στοιχείων

`Unpublish-AzureRmCdnEndpointContent`Εκκαθαρίζει στο cache περιουσιακών στοιχείων, ενώ `Publish-AzureRmCdnEndpointContent` προ-φορτώνει περιουσιακών στοιχείων σε υποστηριζόμενα τελικά σημεία.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Έναρξη/διακοπή CDN τελικά σημεία
`Start-AzureRmCdnEndpoint`και `Stop-AzureRmCdnEndpoint` μπορεί να χρησιμοποιηθεί για την έναρξη και διακοπή μεμονωμένα τελικά σημεία ή ομάδες τελικά σημεία.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>Διαγραφή CDN πόροι

`Remove-AzureRmCdnProfile`και `Remove-AzureRmCdnEndpoint` μπορούν να χρησιμοποιηθούν για να καταργήσετε προφίλ και τα τελικά σημεία.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε πώς μπορείτε να αυτοματοποιήσετε CDN Azure με [.NET](./cdn-app-dev-net.md) ή [Node.js](./cdn-app-dev-node.md).

Για να μάθετε σχετικά με τις δυνατότητες CDN, ανατρέξτε στο θέμα [Επισκόπηση CDN](./cdn-overview.md).


