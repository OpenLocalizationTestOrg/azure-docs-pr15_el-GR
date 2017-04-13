<properties
   pageTitle="Χρήση των cmdlet του PowerShell με το Azure RemoteApp | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε το cmdlet του Windows PowerShell στο Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>



# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Χρήση των cmdlet του Windows PowerShell με το Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

 Μπορείτε να χρησιμοποιήσετε το cmdlet του Azure RemoteApp PowerShell για να διαχειριστείτε και να διατηρήσετε τις συλλογές. Χρησιμοποιήστε τις ακόλουθες πληροφορίες για να ξεκινήσετε.

## <a name="get-the-cmdlets"></a>Λάβετε τα cmdlet 
-------------
Πρώτα λήψη τα cmdlet του Azure Powershell [εδώ](http://go.microsoft.com/?linkid=9811175), το RemoteApp cmdlet του περιλαμβάνονται σε αυτό. 

Ανατρέξτε στο θέμα του [Azure RemoteApp cmdlet Βοήθεια](https://msdn.microsoft.com/library/mt428031.aspx).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Ρύθμιση παραμέτρων Azure cmdlet για να χρησιμοποιήσετε τη συνδρομή σας
------------------
Ακολουθήστε [αυτόν τον Οδηγό](../powershell-install-configure.md) , ώστε να μπορείτε να χρησιμοποιήσετε τα cmdlet του σε σχέση με τη συνδρομή σας στο Azure.

Μπορείτε να χρησιμοποιήσετε αυτά τα βήματα για γρήγορα αποτελέσματα:

1.  Κάντε λήψη και εγκαταστήστε τα [cmdlet του Azure PowerShell](http://go.microsoft.com/?linkid=9811175).
2.  Εκκινήστε το Windows Azure PowerShell.
3.  Εκτελέστε το **Πρόσθετο AzureAccount** για τον έλεγχο ταυτότητας στη συνδρομή σας στο Azure. Όταν σας ζητηθεί, πληκτρολογήστε το ίδιο όνομα χρήστη και τον κωδικό πρόσβασης που χρησιμοποιείτε για να συνδεθείτε στην πύλη Azure.  
4.  Εκτελέστε το **Get-AzureSubscription** για να εμφανίσετε τις συνδρομές που σχετίζεται με το λογαριασμό χρήστη. 
5.  Εκτελέστε **Επιλέξτε AzureSubscription** και καθορίστε το όνομα της συνδρομής ή το Αναγνωριστικό για να χρησιμοποιήσετε στην κονσόλα του PowerShell.

Συγχαρητήρια, κονσόλα σας Azure PowerShell είναι ρυθμισμένη και έτοιμη για χρήση. Έχετε υπόψη ότι θα πρέπει να επαναλάβετε τα βήματα 2 έως 5 κάθε φορά που ξεκινάτε το την κονσόλα Azure PowerShell.  

## <a name="create-a-cloud-collection"></a>Δημιουργία συλλογής cloud
--------------------
Είναι απλή, εκτελέστε την ακόλουθη εντολή:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Η παραπάνω εντολή δημοσιεύει αυτόματα εφαρμογές του Microsoft Office 365 (Excel, OneNote, Outlook, PowerPoint, Visio και Word).

Δημιουργία συλλογής μπορεί να χρειαστούν 30 λεπτά ή περισσότερο για να ολοκληρωθεί. Επομένως, αυτή η εντολή επιστρέφει ένα Αναγνωριστικό παρακολούθησης που μπορείτε να χρησιμοποιήσετε ως εξής:


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Μόλις ολοκληρωθεί η συλλογή, μπορείτε να προσθέσετε χρήστες στη συλλογή με την ακόλουθη εντολή:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

Και είστε έτοιμοι! Αυτός ο χρήστης πρέπει να μπορείτε να συνδεθείτε με την εφαρμογή χρησιμοποιώντας το πρόγραμμα-πελάτη Azure RemoteApp βρέθηκε [εδώ](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Cmdlet του διαθέσιμη
Υπάρχουν πολλές άλλες εντολές που έχουμε, σύντομα θα σύντομα την τεκμηρίωση για τους:

Βασική cmdlet RemoteApp συλλογών: 

- Νέα AzureRemoteAppCollection
- Get-AzureRemoteAppCollection
- Ορισμός AzureRemoteAppCollection
- Ενημέρωση AzureRemoteAppCollection
- Κατάργηση AzureRemoteAppCollection
- Προσθήκη AzureRemoteAppUser
- Get-AzureRemoteAppUser
- Κατάργηση AzureRemoteAppUser
- Get-AzureRemoteAppSession
- Αποσύνδεση AzureRemoteAppSession
- Κλήση AzureRemoteAppSessionLogoff
- Αποστολή AzureRemoteAppSessionMessage
- Get-AzureRemoteAppProgram
- Get-AzureRemoteAppStartMenuProgram
- Δημοσίευση AzureRemoteAppProgram
- Κατάργηση δημοσίευσης AzureRemoteAppProgram
- Get-AzureRemoteAppCollectionUsageDetails
- Get-AzureRemoteAppCollectionUsageSummary
- Get-AzureRemoteAppPlan

Cmdlet για εικονικού δικτύου RemoteApp:

- Νέα AzureRemoteAppVNet
- Get-AzureRemoteAppVNet
- Ορισμός AzureRemoteAppVNet
- Κατάργηση AzureRemoteAppVNet
- Get-AzureRemoteAppVpnDevice
- Λήψη--AzureRemoteAppVpnDeviceConfigScript
- Επαναφορά AzureRemoteAppVpnSharedKey

Cmdlet για εικόνα προτύπου RemoteApp:

- Νέα AzureRemoteAppTemplateImage
- Get-AzureRemoteAppTemplateImage
- Μετονομασία AzureRemoteAppTemplateImage
- Κατάργηση AzureRemoteAppTemplateImage

Cmdlet για άλλες RemoteApp:

- Get-AzureRemoteAppLocation
- Get-AzureRemoteAppWorkspace
- Ορισμός AzureRemoteAppWorkspace
- Get-AzureRemoteAppOperationResult
 
