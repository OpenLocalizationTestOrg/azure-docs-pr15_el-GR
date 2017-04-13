<properties
 pageTitle="Όρια χρονοδιάγραμμα και προεπιλογών"
 description="Όρια χρονοδιάγραμμα και προεπιλογών"
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

# <a name="scheduler-limits-and-defaults"></a>Όρια χρονοδιάγραμμα και προεπιλογών

## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Όρια χρονοδιάγραμμα, όρια, προεπιλογών και περιορισμοί

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>Στην κεφαλίδα x-ms--αναγνωριστικό αίτησης

Κάθε αίτηση που υποβάλλεται σε σχέση με την υπηρεσία χρονοδιαγράμματος επιστρέφει μια κεφαλίδα απόκρισης με το όνομα**x-ms--αναγνωριστικό αίτησης**. Αυτή η κεφαλίδα περιέχει μια τιμή αδιαφανές που προσδιορίζει με μοναδικό τρόπο την αίτηση.

Εάν μια αίτηση με συνέπεια αποτυγχάνει και έχετε επαληθεύσει ότι η αίτηση είναι τυποποιημένη σωστά, μπορείτε να χρησιμοποιήσετε αυτή την τιμή για να αναφέρετε το σφάλμα στη Microsoft. Στην αναφορά σας, συμπεριλάβετε την τιμή του x-ms--αναγνωριστικό αίτησης, το χρόνο κατά προσέγγιση που έγινε την αίτηση, το αναγνωριστικό τη συνδρομή, εργασία συλλογής, ή/και του έργου και τον τύπο της λειτουργίας που Επιχειρήθηκε η πρόσκληση σε.

## <a name="see-also"></a>Δείτε επίσης


 [Τι είναι το χρονοδιάγραμμα;](scheduler-intro.md)

 [Azure έννοιες χρονοδιάγραμμα, ορολογία και οντότητα ιεραρχία](scheduler-concepts-terms.md)

 [Γρήγορα αποτελέσματα με το χρονοδιάγραμμα στην πύλη του Azure](scheduler-get-started-portal.md)

 [Προγράμματα και Τιμολόγηση στο χρονοδιάγραμμα Azure](scheduler-plans-billing.md)

 [Αναφορά Azure Scheduler REST API](https://msdn.microsoft.com/library/mt629143)

 [Αναφορά Azure cmdlet του PowerShell χρονοδιαγράμματος](scheduler-powershell-reference.md)

 [Azure Scheduler υψηλή διαθεσιμότητα και την αξιοπιστία](scheduler-high-availability-reliability.md)

 [Azure έλεγχο ταυτότητας εξερχομένων χρονοδιαγράμματος](scheduler-outbound-authentication.md)
