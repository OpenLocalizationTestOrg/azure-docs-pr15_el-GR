<properties
   pageTitle="Παρατηρήστε 1 συνταξιοδότηση οικογένεια OS επισκέπτη | Microsoft Azure"
   description="Παρέχει πληροφορίες σχετικά με το πότε έγινε η αποχώρηση Azure επισκέπτη OS οικογένεια 1 και πώς μπορείτε να προσδιορίσετε αν που επηρεάζονται"
   services="cloud-services"
   documentationCenter="na"
   authors="raiye"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/24/2016"
   ms.author="raiye"/>



# <a name="guest-os-family-1-retirement-notice"></a>Ειδοποίηση συνταξιοδότηση επισκέπτη OS οικογένεια 1

Η αποχώρηση λειτουργικό σύστημα 1 οικογένεια πρώτα ανακοινώθηκε στις 1 Ιουνίου 2013.

**2 Σεπτεμβρίου 2014** Azure φιλοξενούμενο λειτουργικό σύστημα (OS επισκέπτη) οικογένεια 1.x, το οποίο βασίζεται στο λειτουργικό σύστημα Windows Server 2008, επίσημα καταργήθηκε. Όλες οι προσπάθειες για την ανάπτυξη νέων υπηρεσιών ή αναβάθμιση υπάρχουσες υπηρεσίες με χρήση του 1 οικογένεια θα αποτύχει με ένα μήνυμα σφάλματος που σας ενημερώνει ότι το 1 επισκέπτη OS οικογένεια έχει καταργηθεί.

**3 Νοεμβρίου 2014** Εκτεταμένη υποστήριξη για 1 οικογένεια OS επισκέπτη τερματίστηκε και αυτό είναι πλήρως απόσυρση. Όλες οι υπηρεσίες στην οικογένεια 1 θα να αντιμετωπίσουν προβλήματα. Θα σας μπορεί να σταματήσει αυτών των υπηρεσιών οποιαδήποτε στιγμή. Δεν υπάρχει εγγύηση των υπηρεσιών σας θα συνεχίσει να εκτελείται, εκτός εάν με μη αυτόματο τρόπο αναβάθμιση τους στον εαυτό σας.

Εάν έχετε περισσότερες ερωτήσεις, επισκεφθείτε το [Φόρουμ υπηρεσίες Cloud](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) ή [Επικοινωνήστε με την υποστήριξη Azure](https://azure.microsoft.com/support/options/).




## <a name="are-you-affected"></a>Που επηρεάζονται;

Τις υπηρεσίες Cloud επηρεάζονται εάν ισχύει ένα από τα εξής:

1. Έχετε μια τιμή "osFamily ="1"καθορίζεται ρητά στο αρχείο ServiceConfiguration.cscfg για την υπηρεσία Cloud.
2. Δεν έχετε μια τιμή για osFamily καθορίζεται ρητά στο αρχείο ServiceConfiguration.cscfg για την υπηρεσία Cloud. Προς το παρόν, το σύστημα χρησιμοποιεί την προεπιλεγμένη τιμή "1" σε αυτήν την περίπτωση.
3. Πύλη του Azure κλασική παραθέτει την τιμή οικογένειας φιλοξενούμενο λειτουργικό σύστημα ως "Windows Server 2008".

Για να βρείτε ποιες από τις υπηρεσίες cloud εκτελούν ποια οικογένεια προγραμμάτων λειτουργικό σύστημα, μπορείτε να εκτελέσετε τη δέσμη ενεργειών κάτω από το στοιχείο στο Azure PowerShell, παρόλο που πρέπει να [ορίσετε Azure PowerShell](../powershell-install-configure.md) πρώτα. Για πρόσθετες λεπτομέρειες σχετικά με τη δέσμη ενεργειών, ανατρέξτε στο θέμα [Azure επισκέπτη OS οικογένεια 1 τέλος της ζωής: 2014 Ιουνίου](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx). 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Τις υπηρεσίες cloud θα είναι επηρεάζεται από 1 οικογένεια OS συνταξιοδότηση, εάν η στήλη osFamily στη δέσμη ενεργειών είναι κενό ή περιέχει ένα "1".

## <a name="recommendations-if-you-are-affected"></a>Συστάσεις εάν που επηρεάζονται

Συνιστάται να μπορείτε να μετεγκαταστήσετε ρόλους σας υπηρεσία Cloud μία από τις υποστηριζόμενες οικογένειες επισκέπτη στο λειτουργικό σύστημα:

**OS επισκέπτη οικογένειας 4.x** -Windows Server 2012 R2 *(συνιστάται)*

1. Βεβαιωθείτε ότι η εφαρμογή σας χρησιμοποιεί SDK 2.1 ή νεότερη έκδοση με το .NET framework 4.0, διαίρεσης 4,5 ή 4.5.1.
2. Για να ρυθμίσετε το χαρακτηριστικό osFamily "4" στο αρχείο ServiceConfiguration.cscfg και αναπτύξτε ξανά την υπηρεσία cloud.


**OS επισκέπτη οικογένειας 3.x** -Windows Server 2012

1. Βεβαιωθείτε ότι η εφαρμογή σας χρησιμοποιεί SDK 1,8 ή νεότερη έκδοση με το .NET framework 4.0 ή διαίρεσης 4,5.
2. Για να ρυθμίσετε το χαρακτηριστικό osFamily "3" στο αρχείο ServiceConfiguration.cscfg και αναπτύξτε ξανά την υπηρεσία cloud.


**OS επισκέπτη οικογένειας 2.x** -Windows Server 2008 R2

1. Βεβαιωθείτε ότι η εφαρμογή σας χρησιμοποιεί SDK 1.3 και παραπάνω με το .NET framework 3.5 ή 4.0.
2. Για να ρυθμίσετε το χαρακτηριστικό osFamily "2" στο αρχείο ServiceConfiguration.cscfg και αναπτύξτε ξανά την υπηρεσία cloud.


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Εκτεταμένη υποστήριξη για 1 οικογένεια OS επισκέπτη τερματίστηκε 3 Νοεμβρίου 2014
Υπηρεσίες cloud στην οικογένεια προγραμμάτων επισκέπτη λειτουργικό σύστημα 1 δεν υποστηρίζονται πλέον. Επικοινωνήστε μετεγκατάσταση απενεργοποίηση οικογένεια 1 όσο το δυνατόν πιο σύντομα, για να αποφύγετε διαταραχή της υπηρεσίας.  

## <a name="next-steps"></a>Επόμενα βήματα
Αναθεωρήστε τα πιο πρόσφατες [εκδόσεις OS επισκέπτη](cloud-services-guestos-update-matrix.md).
