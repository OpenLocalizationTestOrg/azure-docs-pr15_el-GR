<properties
    pageTitle="Υπηρεσίες Azure δημόσιους οργανισμούς | Microsoft Azure"
    description="Πληροφορίες σχετικά με τη Διαχείριση τη συνδρομή σας στο δημόσιο Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Διαχείριση και τη σύνδεση με τη συνδρομή σας στο δημόσιο Azure

Azure δημόσιους οργανισμούς έχει μοναδικές διευθύνσεις URL και τελικά σημεία για τη διαχείριση του περιβάλλοντός σας. Είναι σημαντικό να χρησιμοποιήσετε τη δεξιά συνδέσεις για να διαχειριστείτε το περιβάλλον σας από την πύλη ή PowerShell. Όταν είστε συνδεδεμένοι στο περιβάλλον Azure για δημόσιους οργανισμούς, το κανονικό λειτουργίες για τη Διαχείριση υπηρεσίας λειτουργεί εάν έχει αναπτυχθεί το στοιχείο.

## <a name="connecting-via-the-portal"></a>Σύνδεση μέσω της πύλης
Η πύλη είναι ο κύριος τρόπος που συνδέονται οι περισσότεροι για δημόσιους οργανισμούς Azure.  Για να συνδεθείτε, μεταβείτε στην πύλη του στη [https://portal.azure.us](https://portal.azure.us).  Το παλαιότερης έκδοσης της πύλης Azure είναι δυνατή η πρόσβαση μέσω [https://manage.windowsazure.us](https://manage.windowsazure.us).

Συνδρομές μπορούν να δημιουργηθούν για το λογαριασμό σας με τη σύνδεση στο [https://account.windowsazure.us](https://account.windowsazure.us).

## <a name="connecting-via-powershell"></a>Σύνδεση μέσω του PowerShell

Αν χρησιμοποιείτε Azure PowerShell για τη διαχείριση μεγάλου συνδρομή μέσω δέσμη ενεργειών ή access δυνατότητες που δεν είναι διαθέσιμη αυτήν τη στιγμή στην πύλη του Azure που πρέπει να συνδεθείτε με δημόσιους οργανισμούς Azure αντί για δημόσια Azure.  Εάν έχετε χρησιμοποιήσει PowerShell σε δημόσια Azure, είναι κυρίως το ίδιο.  Οι διαφορές σε δημόσιους οργανισμούς Azure είναι:

+ Σύνδεση με το λογαριασμό
+ Ονόματα των περιοχών

>[AZURE.NOTE] Εάν δεν έχετε χρησιμοποιήσει ακόμη PowerShell, ανατρέξτε στο θέμα [Εισαγωγή στις Azure PowerShell](../powershell-install-configure.md).

Κατά την εκκίνηση του PowerShell, πρέπει να πείτε Azure PowerShell για να συνδεθείτε στο Azure για δημόσιους οργανισμούς, καθορίζοντας μια παράμετρο περιβάλλον.  Η παράμετρος εξασφαλίζει ότι PowerShell συνδέεται με τη σωστή τελικά σημεία.  Η συλλογή τελικά σημεία προσδιορίζεται κατά τη σύνδεση συνδεθείτε στο λογαριασμό σας.  Διαφορετικά APIs απαιτούν διαφορετικές εκδόσεις του διακόπτη περιβάλλον:

Τύπος σύνδεσης | Εντολή
---|----
[Διαχείριση της υπηρεσίας](https://msdn.microsoft.com/library/dn708504.aspx) εντολές | `Add-AzureAccount -Environment AzureUSGovernment`
[Διαχείριση πόρων](https://msdn.microsoft.com/library/mt125356.aspx) εντολές | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
Εντολές [Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx) | `Connect-MsolService -AzureEnvironment UsGovernment`
[Azure v2 εντολή υπηρεσίας καταλόγου Active Directory](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

Μπορείτε επίσης μπορεί να χρησιμοποιήστε το διακόπτη περιβάλλον κατά τη σύνδεση σε ένα λογαριασμό χώρου αποθήκευσης με νέα AzureStorageContext και να καθορίσετε AzureUSGovernment.

### <a name="determining-region"></a>Καθορισμός περιοχής

Όταν είστε συνδεδεμένοι, υπάρχει μια επιπλέον διαφορά – τις περιοχές που χρησιμοποιούνται για τη στόχευση υπηρεσίας.  Κάθε Azure cloud έχει διαφορετικές περιοχές.  Μπορείτε να δείτε τους που αναφέρονται στη σελίδα διαθεσιμότητα υπηρεσίας.  Κανονικά, χρησιμοποιήστε την περιοχή στην παράμετρο θέση μιας εντολής.

Υπάρχει ένα προϊόντων.  Τις περιοχές για δημόσιους οργανισμούς Azure πρέπει να είναι διαφορετικά από τα ονόματά τους κοινές μορφοποίηση:

Κοινό όνομα | Εντολή
---|----
Βιρτζίνια Gov η.π.α. | USGov Βιρτζίνια
Iowa Gov η.π.α. | USGov Iowa

>[AZURE.NOTE] Δεν υπάρχει κανένα διάστημα μεταξύ των ΗΠΑ και Gov όταν χρησιμοποιείτε την παράμετρο θέση.

Εάν θέλετε να για να επικυρώσετε τις διαθέσιμες περιοχές Azure για δημόσιους οργανισμούς, μπορείτε να εκτελέσετε τις παρακάτω εντολές και να εκτυπώσετε την τρέχουσα λίστα:

    Get-AzureLocation

Εάν είστε ενδιαφέρει τα διαθέσιμα περιβάλλοντα κατά μήκος Azure, μπορείτε να εκτελέσετε:

    Get-AzureEnvironment

## <a name="next-steps"></a>Επόμενα βήματα

Εάν αναζητάτε περισσότερες πληροφορίες, μπορείτε να ανατρέξετε στο θέμα:

+ [Έγγραφα του PowerShell στο GitHub](https://github.com/Azure/azure-powershell)
+ [Οδηγίες βήμα προς βήμα σχετικά με τη σύνδεση Διαχείριση πόρων](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [Azure PowerShell έγγραφα στο MSDN](https://msdn.microsoft.com/library/mt619274.aspx)

Για συμπληρωματικές πληροφορίες και ενημερώσεις εγγραφή σε [ιστολόγιο του Microsoft Azure για δημόσιους οργανισμούς] (https://blogs.msdn.microsoft.com/azuregov/)
