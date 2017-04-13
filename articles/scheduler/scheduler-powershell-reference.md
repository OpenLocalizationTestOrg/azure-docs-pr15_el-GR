<properties
 pageTitle="Αναφορά των cmdlet του PowerShell χρονοδιαγράμματος"
 description="Αναφορά των cmdlet του PowerShell χρονοδιαγράμματος"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-powershell-cmdlets-reference"></a>Αναφορά των cmdlet του PowerShell χρονοδιαγράμματος

Ο παρακάτω πίνακας περιγράφει και συνδέσεων στη σελίδα αναφοράς κάθε ένα από τα κύρια cmdlet στο χρονοδιάγραμμα Azure.

Για να εγκαταστήσετε Azure PowerShell και να συσχετίσετε με τη συνδρομή σας στο Azure, δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md). 

Για περισσότερες πληροφορίες σχετικά με τα [cmdlet διαχείρισης πόρων Azure](https://msdn.microsoft.com/library/mt125356\(v=azure.200\).aspx), ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure](../powershell-azure-resource-manager.md).

|Cmdlet|Περιγραφή cmdlet|
|---|---|
[Απενεργοποίηση AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490133\(v=azure.200\).aspx) |Απενεργοποιεί μια εργασία συλλογής. 
[Ενεργοποίηση AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490135\(v=azure.200\).aspx) |Ενεργοποιεί μια εργασία συλλογής.
[Get-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490125\(v=azure.200\).aspx) |Λαμβάνει το Χρονοδιάγραμμα εργασιών.
[Get-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490132\(v=azure.200\).aspx) |Λαμβάνει εργασία συλλογών.
[Get-AzureRmSchedulerJobHistory](https://msdn.microsoft.com/library/mt490126\(v=azure.200\).aspx) |Ιστορικό εργασίας λαμβάνει.
[Νέα AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136\(v=azure.200\).aspx) |Δημιουργεί μια εργασία HTTP.
[Νέα AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141\(v=azure.200\).aspx) |Δημιουργεί μια εργασία συλλογής.
[Νέα AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134\(v=azure.200\).aspx) |Δημιουργεί μια εργασία ουρά bus υπηρεσίας.
[Νέα AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142\(v=azure.200\).aspx) |Δημιουργεί μια εργασία υπηρεσίας bus το θέμα.
[Νέα AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127\(v=azure.200\).aspx) |Δημιουργεί μια εργασία ουρά χώρου αποθήκευσης. 
[Κατάργηση AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490140\(v=azure.200\).aspx) |Καταργεί ένα χρονοδιάγραμμα έργου.  
[Κατάργηση AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490131\(v=azure.200\).aspx) |Καταργεί μια εργασία συλλογής. 
[Ορισμός AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490130\(v=azure.200\).aspx) |Τροποποιεί μια εργασία HTTP χρονοδιαγράμματος.
[Ορισμός AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490129\(v=azure.200\).aspx) |Τροποποιεί μια εργασία συλλογής. 
[Ορισμός AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490143\(v=azure.200\).aspx) |Τροποποιεί μια εργασία ουρά bus υπηρεσίας.  
[Ορισμός AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490137\(v=azure.200\).aspx) |Τροποποιεί μια εργασία υπηρεσίας bus το θέμα. 
[Ορισμός AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490128\(v=azure.200\).aspx) |Τροποποιεί μια εργασία ουρά χώρου αποθήκευσης.   

Για πιο λεπτομερείς πληροφορίες, μπορείτε να εκτελέσετε οποιαδήποτε από τα ακόλουθα cmdlet: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Δείτε επίσης


 [Τι είναι το χρονοδιάγραμμα;](scheduler-intro.md)

 [Azure έννοιες χρονοδιάγραμμα, ορολογία και οντότητα ιεραρχία](scheduler-concepts-terms.md)

 [Γρήγορα αποτελέσματα με το χρονοδιάγραμμα στην πύλη του Azure](scheduler-get-started-portal.md)

 [Προγράμματα και Τιμολόγηση στο χρονοδιάγραμμα Azure](scheduler-plans-billing.md)

 [Αναφορά Azure Scheduler REST API](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler υψηλή διαθεσιμότητα και την αξιοπιστία](scheduler-high-availability-reliability.md)

 [Azure όρια χρονοδιάγραμμα, προεπιλεγμένες τιμές και τους κωδικούς σφάλματος](scheduler-limits-defaults-errors.md)

 [Azure έλεγχο ταυτότητας εξερχομένων χρονοδιαγράμματος](scheduler-outbound-authentication.md)
