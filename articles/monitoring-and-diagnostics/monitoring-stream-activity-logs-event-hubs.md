<properties
    pageTitle="Ροή το αρχείο καταγραφής Azure δραστηριοτήτων με διανομείς συμβάν | Microsoft Azure"
    description="Μάθετε πώς να ροή την Azure δραστηριότητας το αρχείο καταγραφής συμβάντων διανομείς."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="johnkem"/>

# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Ροή το αρχείο καταγραφής Azure δραστηριοτήτων με διανομείς συμβάντος
[**Αρχείο καταγραφής δραστηριοτήτων Azure**](./monitoring-overview-activity-logs.md) είναι δυνατό να διοχετεύονται σε άμεσο πραγματικό χρόνο σε οποιαδήποτε εφαρμογή χρησιμοποιώντας την ενσωματωμένη επιλογή "Εξαγωγή" στην πύλη ή ενεργοποιώντας το αναγνωριστικό υπηρεσίας Bus κανόνα σε ένα αρχείο καταγραφής προφίλ μέσω της cmdlet του PowerShell Azure ή Azure CLI.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Τι μπορείτε να κάνετε με το αρχείο καταγραφής δραστηριοτήτων και διανομείς συμβάντος
Εδώ θα βρείτε ορισμένους τρόπους που μπορείτε να χρησιμοποιήσετε τη δυνατότητα ροής για το αρχείο καταγραφής της δραστηριότητας:

- **Ροή για συστήματα καταγραφής και τηλεμετρίας τρίτων κατασκευαστών** – διάρκεια του χρόνου, διανομείς συμβάν ροής θα μετατραπεί σε το μηχανισμό να διοχέτευση το αρχείο καταγραφής δραστηριοτήτων σε SIEMs τρίτων κατασκευαστών και να συνδεθείτε ανάλυση λύσεις.
- **Δημιουργήστε μια προσαρμοσμένη τηλεμετρίας και καταγραφή από την πλατφόρμα** – εάν έχετε ήδη μια πλατφόρμα συγκεκριμένου τηλεμετρίας ή είναι απλώς υπόψη σχετικά με τη δημιουργία μία, ιδιαίτερα με δημοσίευσης εγγραφής φύση του συμβάντος διανομείς σάς επιτρέπει να ευελιξία ingest το αρχείο καταγραφής της δραστηριότητας. [Ανατρέξτε στον οδηγό του Dan Rosanova για χρήση διανομείς συμβάν σε μια πλατφόρμα τηλεμετρίας παγκόσμια κλίμακα εδώ.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Ενεργοποίηση της ροής του αρχείου καταγραφής της δραστηριότητας
Μπορείτε να ενεργοποιήσετε τη ροή του αρχείου καταγραφής της δραστηριότητας μέσω προγραμματισμού ή μέσω της πύλης. Σε κάθε περίπτωση, μπορείτε να επιλέξετε μια υπηρεσία Namespace Bus και μια πολιτική κοινόχρηστη πρόσβαση για αυτό το πεδίο ονομάτων και ένα συμβάν διανομέα δημιουργείται στο συγκεκριμένο χώρο ονομάτων όταν εμφανιστεί το πρώτο νέο συμβάν καταγραφής δραστηριότητας. Εάν δεν έχετε ένα Namespace Bus υπηρεσία, πρέπει πρώτα να δημιουργήσετε ένα. Εάν έχετε προηγουμένως ροή συμβάντα του αρχείου καταγραφής δραστηριότητας για αυτό Namespace Bus υπηρεσίας, την ενότητα συμβάντος που δημιουργήθηκε προηγουμένως θα να χρησιμοποιηθεί ξανά. Η πολιτική κοινόχρηστη πρόσβαση καθορίζει τα δικαιώματα που έχει το μηχανισμό ροής. Σήμερα, η ροή με ένα συμβάν διανομείς απαιτεί δικαιώματα **Διαχείριση**, την **Ανάγνωση**και **Αποστολή** . Μπορείτε να δημιουργήσετε ή να τροποποιήσετε πολιτικές πρόσβασης υπηρεσίας Bus Namespace θέσει σε κοινή χρήση στην πύλη του κλασική κάτω από την καρτέλα "Ρύθμιση παραμέτρων" για την υπηρεσία Namespace Bus. Για να ενημερώσετε το προφίλ καταγραφής καταγραφής δραστηριότητας για να συμπεριλάβετε ροής, ο χρήστης που κάνει την αλλαγή πρέπει να έχετε το δικαίωμα ListKey σε αυτόν τον κανόνα εξουσιοδότησης Bus υπηρεσίας.

### <a name="via-azure-portal"></a>Μέσω Azure πύλη 
1. Μεταβείτε το **Αρχείο καταγραφής δραστηριοτήτων** blade χρησιμοποιώντας το μενού στην αριστερή πλευρά της πύλης.

    ![Μεταβείτε στο αρχείο καταγραφής της δραστηριότητας στην πύλη](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Κάντε κλικ στο κουμπί **Εξαγωγή** στο επάνω μέρος του blade.

    ![Κουμπί "Εξαγωγή" στην πύλη](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Στο το blade που εμφανίζεται, μπορείτε να επιλέξετε τις περιοχές για την οποία θέλετε να συμβάντα ροής και τα Namespace Bus υπηρεσία στην οποία θα θέλατε ένα συμβάν διανομέα που θα δημιουργηθεί για ροή αυτά τα συμβάντα.

    ![Εξαγωγή blade αρχείο καταγραφής δραστηριοτήτων](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε αυτές τις ρυθμίσεις. Οι ρυθμίσεις εφαρμόζονται αμέσως στη συνδρομή σας.


### <a name="via-powershell-cmdlets"></a>Μέσω των cmdlet του PowerShell
Εάν υπάρχει ήδη ένα αρχείο καταγραφής προφίλ, πρέπει πρώτα να καταργήσετε αυτό το προφίλ.

1. Χρήση `Get-AzureRmLogProfile` για να προσδιορίσετε εάν υπάρχει ένα αρχείο καταγραφής προφίλ
2. Εάν Ναι, χρησιμοποιήστε `Remove-AzureRmLogProfile` για να την καταργήσετε.
3. Χρήση `Set-AzureRmLogProfile` για να δημιουργήσετε ένα προφίλ:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

Το Αναγνωριστικό υπηρεσίας Bus κανόνας είναι μια συμβολοσειρά με αυτήν τη μορφή: {υπηρεσίας Αναγνωριστικό πόρου bus} /authorizationrules/ {κλειδιού name}, για παράδειγμα 

### <a name="via-azure-cli"></a>Μέσω Azure CLI
Εάν υπάρχει ήδη ένα αρχείο καταγραφής προφίλ, πρέπει πρώτα να καταργήσετε αυτό το προφίλ.

1. Χρήση `azure insights logprofile list` για να προσδιορίσετε εάν υπάρχει ένα αρχείο καταγραφής προφίλ
2. Εάν Ναι, χρησιμοποιήστε `azure insights logprofile delete` για να την καταργήσετε.
3. Χρήση `azure insights logprofile add` για να δημιουργήσετε ένα προφίλ:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Το Αναγνωριστικό υπηρεσίας Bus κανόνας είναι μια συμβολοσειρά με αυτήν τη μορφή: `{service bus resource ID}/authorizationrules/{key name}`.
 
## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Πώς μπορώ να εκμετάλλευση τα δεδομένα του αρχείου καταγραφής από το συμβάν διανομείς;
[Το σχήμα για το αρχείο καταγραφής της δραστηριότητας είναι διαθέσιμη εδώ](./monitoring-overview-activity-logs.md). Κάθε συμβάντος βρίσκεται σε έναν πίνακα με αντικείμενα BLOB JSON που ονομάζεται "καρτελών".

## <a name="next-steps"></a>Επόμενα βήματα
- [Αρχειοθέτηση του αρχείου καταγραφής δραστηριότητας σε ένα λογαριασμό του χώρου αποθήκευσης](./monitoring-archive-activity-log.md)
- [Διαβάστε την επισκόπηση των το αρχείο καταγραφής της δραστηριότητας Azure](./monitoring-overview-activity-logs.md)
- [Ρύθμιση ειδοποίησης που βασίζεται σε ένα συμβάν καταγραφής δραστηριότητας](./insights-auditlog-to-webhook-email.md)
