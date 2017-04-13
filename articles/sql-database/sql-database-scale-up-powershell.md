<properties 
    pageTitle="Αλλάξτε το επίπεδο επίπεδο και επιδόσεων υπηρεσίας από μια βάση δεδομένων Azure SQL με χρήση του PowerShell | Microsoft Azure" 
    description="Αλλάξτε το επίπεδο υπηρεσιών και επίπεδο επιδόσεων μιας βάσης δεδομένων Azure SQL δείχνει πώς μπορείτε να περιορίσετε το μέγεθος της βάσης δεδομένων SQL προς τα επάνω ή προς τα κάτω με το PowerShell. Αλλαγή σειράς τιμολόγησης από μια βάση δεδομένων Azure SQL με το PowerShell." 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>Αλλαγή υπηρεσίας επίπεδο και επιδόσεων επιπέδου (επίπεδο τις τιμές) μια βάση δεδομένων SQL με το PowerShell


> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-scale-up.md)
- [**PowerShell**](sql-database-scale-up-powershell.md)


Επίπεδα υπηρεσίας και επιδόσεων περιγράφουν τις δυνατότητες και τους πόρους που είναι διαθέσιμες για τη βάση δεδομένων SQL και μπορούν να ενημερωθούν με τις ανάγκες της αλλαγής της εφαρμογής σας. Για λεπτομέρειες, ανατρέξτε στο θέμα [Επίπεδα υπηρεσίας](sql-database-service-tiers.md).

Σημειώστε ότι η αλλαγή το επίπεδο υπηρεσιών ή/και επίπεδο επιδόσεων μιας βάσης δεδομένων δημιουργεί ένα αντίγραφο της αρχικής βάσης δεδομένων με το νέο επίπεδο επιδόσεων και, στη συνέχεια, μεταβαίνει συνδέσεις στη ρεπλίκα. Δεν υπάρχουν δεδομένα θα χαθεί κατά τη διάρκεια αυτής της διαδικασίας αλλά κατά τη στιγμή σύντομη πότε θα σας μετάβαση του στη ρεπλίκα, συνδέσεις στη βάση δεδομένων είναι απενεργοποιημένες, ώστε να ορισμένες συναλλαγές στην πτήσεων μπορεί να επανέλθει. Αυτό το παράθυρο ποικίλλει, αλλά είναι κατά μέσο όρο στην περιοχή 4 δευτερόλεπτα και σε περισσότερους από 99% των κουτιών που είναι μικρότερη από 30 δευτερόλεπτα. Πολύ συχνά, ειδικά εάν υπάρχουν μεγάλη τους αριθμούς των συναλλαγών στην πτήσεων στο τις συνδέσεις ροπών είναι απενεργοποιημένες, αυτό το παράθυρο μπορεί να είναι μεγαλύτερο.  

Στη διάρκεια της ολόκληρη τη διαδικασία κλιμάκωση εξαρτάται από το μέγεθος και υπηρεσία επίπεδο της βάσης δεδομένων πριν και μετά την αλλαγή. Για παράδειγμα, μια βάση δεδομένων 250 GB που αλλάζει για να, από ή μέσα σε ένα επίπεδο τυπική υπηρεσιών, θα πρέπει να ολοκληρωθεί εντός 6 ώρες. Για μια βάση δεδομένων με το ίδιο μέγεθος που αλλάζοντας τα επίπεδα των επιδόσεων μέσα στο επίπεδο υπηρεσίας Premium, θα πρέπει να ολοκληρωθεί εντός 3 ώρες.


- Για να υποβιβάσετε μια βάση δεδομένων, η βάση δεδομένων πρέπει να είναι μικρότερη από το μέγιστο επιτρεπόμενο μέγεθος των το επίπεδο υπηρεσιών προορισμού. 
- Κατά την αναβάθμιση μιας βάσης δεδομένων με με δυνατότητα [Αναπαραγωγής παν](sql-database-geo-replication-portal.md) , που πρέπει πρώτα να αναβαθμίσετε τα δευτερεύοντα βάσεις δεδομένων στο επιθυμητό επιδόσεων επίπεδο πριν από την αναβάθμιση στην κύρια βάση δεδομένων.
- Όταν υποβάθμιση από ένα επίπεδο υπηρεσιών Premium, πρώτα πρέπει να τερματίσετε όλες οι σχέσεις παν αναπαραγωγής. Μπορείτε να ακολουθήσετε τα βήματα που περιγράφονται στο θέμα [ανάκτηση από μια μη διαθεσιμότητα](sql-database-disaster-recovery.md) για να διακόψετε τη διαδικασία αναπαραγωγής μεταξύ των κύριων σελίδων και του ενεργού δευτερεύοντα βάσεις δεδομένων.
- Η επαναφορά προσφορές υπηρεσιών είναι διαφορετικές για τα διάφορα επίπεδα εξυπηρέτησης. Εάν που υποβάθμιση που ενδέχεται να χάσετε τη δυνατότητα να επαναφέρετε ένα σημείο στο χρόνο ή εάν έχετε ένα κάτω περίοδος διατήρησης δημιουργίας αντιγράφων ασφαλείας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας της βάσης δεδομένων SQL Azure και επαναφορά](sql-database-business-continuity.md).
- Οι νέες ιδιότητες για τη βάση δεδομένων δεν εφαρμόζονται μέχρι να ολοκληρωθούν οι αλλαγές.



**Για να ολοκληρώσετε αυτό το άρθρο χρειάζεστε τα εξής:**

- Μια συνδρομή του Azure. Εάν χρειάζεστε μια συνδρομή του Azure απλώς κάντε κλικ στην επιλογή **ΔΩΡΕΆΝ ΛΟΓΑΡΙΑΣΜΌ** στο επάνω μέρος αυτής της σελίδας και, στη συνέχεια, επιστρέψτε στο τέλος αυτού του άρθρου.
- Μια βάση δεδομένων Azure SQL. Εάν δεν έχετε μια βάση δεδομένων SQL, να δημιουργήσετε μία ακολουθώντας τα βήματα σε αυτό το άρθρο: [Δημιουργία του πρώτου βάση δεδομένων SQL Azure](sql-database-get-started.md).
- Azure PowerShell.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>Αλλάξτε το επίπεδο επίπεδο και επιδόσεων υπηρεσίας της βάσης δεδομένων SQL

Εκτελέστε το κλειδί [Set-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) cmdlet και να ορίσετε το **-RequestedServiceObjectiveName** σε επίπεδο επιδόσεων την επιθυμητή τιμολόγησης σειρά, για παράδειγμα *S0*, *S1*, *S2*, *S3*, *P1*, *P2*,...

```
$ResourceGroupName = "resourceGroupName"
    
$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

  

   


## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>Δείγμα δέσμης ενεργειών του PowerShell για να αλλάξετε το επίπεδο επίπεδο και επιδόσεων υπηρεσίας της βάσης δεδομένων SQL

Αντικατάσταση ```{variables}``` με τις τιμές του (περιλαμβάνει τα άγκιστρα).

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"
    
$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"
    
$ServerName = "{server}"
$DatabaseName = "{database}"
    
$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"
    
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId
    
Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```
        


## <a name="next-steps"></a>Επόμενα βήματα

- [Κλιμάκωση ανάληψη και στο](sql-database-elastic-scale-get-started.md)
- [Συνδεθείτε και να υποβάλετε ερώτημα βάσης δεδομένων SQL με SSMS](sql-database-connect-query-ssms.md)
- [Εξαγωγή μια βάση δεδομένων Azure SQL](sql-database-export-powershell.md)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)
- [Τεκμηρίωση βάσης δεδομένων SQL](http://azure.microsoft.com/documentation/services/sql-database/)
- [Cmdlet για βάση δεδομένων azure SQL] (http://msdn.microsoft.com/library/azure/mt574084https :/ / τοποθεσία msdn.microsoft.com/βιβλιοθήκη/azure/mt619433(v=azure.300\).aspx.aspx)