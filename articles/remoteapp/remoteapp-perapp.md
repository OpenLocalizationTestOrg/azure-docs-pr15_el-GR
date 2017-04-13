<properties
   pageTitle="Δημοσίευση εφαρμογών για μεμονωμένους χρήστες σε μια συλλογή Azure RemoteApp (έκδοση Preview) | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημοσιεύσετε εφαρμογές σε μεμονωμένους χρήστες, αντί για ανάλογα με τις ομάδες, στο Azure RemoteApp."
   services="remoteapp-preview"
   documentationCenter=""
   authors="piotrci"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="piotrci"/>

# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Δημοσίευση εφαρμογών για μεμονωμένους χρήστες σε μια συλλογή Azure RemoteApp (έκδοση Preview)

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να δημοσιεύσετε εφαρμογές σε μεμονωμένους χρήστες σε μια συλλογή Azure RemoteApp. Νέα λειτουργία του Azure RemoteApp, είναι αυτήν τη στιγμή στην "Προεπισκόπηση ιδιωτικό" και διαθέσιμο μόνο για να επιλέξετε πρώτοι για λόγους αξιολόγησης.

Αρχικά Azure RemoteApp με δυνατότητα μονόδρομος εφαρμογών "δημοσίευσης": ο διαχειριστής θα δημοσιεύσετε εφαρμογές από την εικόνα και θα είναι ορατά σε όλους τους χρήστες της συλλογής.

Ένα συνηθισμένο σενάριο είναι να συμπεριλάβετε πολλές εφαρμογές σε μια μεμονωμένη εικόνα και να αναπτύξετε ένα συλλογή προκειμένου να μείωση του κόστους διαχείρισης. Συνήθως δεν όλες τις εφαρμογές που έχουν σχέση με όλους τους χρήστες, οι διαχειριστές θα προτιμούσατε δημοσίευση εφαρμογών σε μεμονωμένους χρήστες, ώστε να μην μπορούν να δουν τις εφαρμογές στην εφαρμογή τους τροφοδοσίας.

Αυτό είναι τώρα δυνατή στο Azure RemoteApp – αυτήν τη στιγμή ως δυνατότητα περιορισμένη preview. Ακολουθεί μια σύντομη περίληψη της τις νέες λειτουργίες:

1. Μια συλλογή μπορεί να οριστεί σε μία από τις δύο λειτουργίες:
 
  - στην αρχική "Συλλογή λειτουργία", όπου όλοι οι χρήστες σε μια συλλογή μπορούν να βλέπουν όλες τις δημοσιευμένες εφαρμογές. Αυτή είναι η προεπιλεγμένη κατάσταση λειτουργίας.
  - η νέα "λειτουργία εφαρμογής", όπου οι χρήστες βλέπουν μόνο τις εφαρμογές που έχουν ανατεθεί αποκλειστικά

2. Προς το παρόν τη λειτουργία εφαρμογής μπορεί να ενεργοποιηθεί μόνο με χρήση των cmdlet του Azure RemoteApp PowerShell.

  - Εάν οριστεί σε λειτουργία εφαρμογής, εκχώρηση χρήστη στη συλλογή δεν μπορεί να γίνεται μέσω της πύλης Azure. Εκχώρηση χρήστη πρέπει να γίνεται η διαχείριση κατά cmdlet του PowerShell.

3. Οι χρήστες θα βλέπουν μόνο τις εφαρμογές που δημοσιεύονται απευθείας σε αυτές. Ωστόσο, μπορεί να εξακολουθεί να είναι δυνατή για ένα χρήστη για να εμφανίσετε τις άλλες εφαρμογές που είναι διαθέσιμες στην εικόνα με πρόσβαση σε αυτά απευθείας από το λειτουργικό σύστημα.
  - Αυτή η δυνατότητα δεν παρέχει μια ασφαλή κλειδώματος των εφαρμογών. περιορίζει μόνο ορατότητα στην εφαρμογή τροφοδοσίας.
  - Εάν χρειάζεστε για την απομόνωση χρηστών από τις εφαρμογές, θα πρέπει να χρησιμοποιήσετε ξεχωριστό συλλογές για αυτό.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Πώς μπορώ να αποκτήσω cmdlet του Azure RemoteApp PowerShell

Για να δοκιμάσετε τη νέα λειτουργικότητα preview, θα πρέπει να χρησιμοποιήσετε το cmdlet του Azure PowerShell. Προς το παρόν δεν είναι δυνατό να χρησιμοποιήσουν την πύλη διαχείρισης Azure για να ενεργοποιήσετε τη λειτουργία δημοσίευσης νέας εφαρμογής.

Πρώτα, βεβαιωθείτε ότι έχετε εγκαταστήσει [λειτουργική μονάδα Azure PowerShell](../powershell-install-configure.md) .

Στη συνέχεια, εκκίνηση της κονσόλας του PowerShell σε κατάσταση λειτουργίας διαχειριστή και εκτελέστε το ακόλουθο cmdlet:

        Add-AzureAccount

Αυτό θα σας ζητήσει το όνομα Azure χρήστη και τον κωδικό πρόσβασης. Αφού εισέλθετε, θα μπορέσετε να εκτελέσετε cmdlet του Azure RemoteApp σε όλες τις συνδρομές σας Azure.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Πώς μπορώ να ελέγξω ποια κατάσταση λειτουργίας μια συλλογή βρίσκεται σε

Εκτελέστε το ακόλουθο cmdlet:

        Get-AzureRemoteAppCollection <collectionName>

![Ελέγξτε την κατάσταση λειτουργίας της συλλογής](./media/remoteapp-perapp/araacllelvel.png)

Η ιδιότητα AclLevel μπορεί να έχει τις ακόλουθες τιμές:

- Συλλογή: η αρχική δημοσίευσης κατάσταση. Όλοι οι χρήστες βλέπουν όλες τις δημοσιευμένες εφαρμογές.
- Εφαρμογή: η νέα δημοσίευσης κατάσταση. Οι χρήστες βλέπουν μόνο τις εφαρμογές που έχουν δημοσιευτεί απευθείας σε αυτές.

## <a name="how-to-switch-to-application-publishing-mode"></a>Πώς μπορείτε να μεταβείτε σε λειτουργία δημοσίευσης εφαρμογής

Εκτελέστε το ακόλουθο cmdlet:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Εφαρμογή δημοσίευσης κατάσταση θα διατηρηθούν: αρχικά όλοι οι χρήστες θα βλέπουν όλες τις εφαρμογές του αρχικού δημοσιευμένα.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Πώς να λίστα χρηστών που μπορούν να δουν μια συγκεκριμένη εφαρμογή

Εκτελέστε το ακόλουθο cmdlet:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Αυτό εμφανίζει όλους τους χρήστες που μπορούν να δουν την εφαρμογή.

Σημείωση: Μπορείτε να δείτε τα ψευδώνυμα εφαρμογής (που ονομάζεται "εφαρμογή ψευδώνυμο" στη σύνταξη παραπάνω), εκτελώντας Get-AzureRemoteAppProgram - CollectionName <collectionName>.

## <a name="how-to-assign-an-application-to-a-user"></a>Πώς μπορείτε να αντιστοιχίσετε μια εφαρμογή σε ένα χρήστη

Εκτελέστε το ακόλουθο cmdlet:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Ο χρήστης θα δείτε τώρα την εφαρμογή στο πρόγραμμα-πελάτη Azure RemoteApp και θα έχουν τη δυνατότητα να συνδεθείτε.

## <a name="how-to-remove-an-application-from-a-user"></a>Πώς μπορείτε να καταργήσετε μια εφαρμογή από ένα χρήστη

Εκτελέστε το ακόλουθο cmdlet:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Παροχή σχολίων
Εκτιμούμε ιδιαίτερα τα σχόλια και προτάσεις σχετικά με αυτήν τη δυνατότητα προεπισκόπησης. Συμπληρώστε την [έρευνα](http://www.instant.ly/s/FDdrb) για να μας ενημερώσετε γνώμη σας.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Δεν μάθατε ευκαιρία για να δοκιμάσετε τη δυνατότητα προεπισκόπησης;
Εάν που έχετε δεν συμμετάσχει στην προεπισκόπηση ακόμη, χρησιμοποιήστε αυτήν την [έρευνα](http://www.instant.ly/s/AY83p) για να ζητήσετε πρόσβαση.