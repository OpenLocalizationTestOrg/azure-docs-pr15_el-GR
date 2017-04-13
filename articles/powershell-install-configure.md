<properties
    pageTitle="Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure"
    description="Μάθετε πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure."
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure CLI</a></div>

##<a name="what-is-azure-powershell"></a>Τι είναι το Azure PowerShell;
Azure PowerShell είναι ένα σύνολο λειτουργικές μονάδες που παρέχουν cmdlet για τη Διαχείριση Azure με το Windows PowerShell. Μπορείτε να χρησιμοποιήσετε τα cmdlet για να δημιουργήσετε, να δοκιμάσετε, ανάπτυξη και διαχείριση λύσεων και τις υπηρεσίες που παρέχονται μέσω της πλατφόρμας Azure. Στις περισσότερες περιπτώσεις, τα cmdlet μπορεί να χρησιμοποιηθεί για τις ίδιες εργασίες με την πύλη Azure, όπως η δημιουργία και τη ρύθμιση παραμέτρων υπηρεσιών cloud, εικονικές μηχανές, εικονικών δικτύων και εφαρμογές web.

## <a name="how-versioning-works"></a>Πώς λειτουργεί η διαχείριση εκδόσεων

Azure PowerShell χρησιμοποιεί διαχείρισης σημασιολογίας εκδόσεων, γεγονός που σημαίνει ότι αν έκδοση A > έκδοση B, στη συνέχεια, A έχει τα API πιο ενημερωμένη έκδοση. Επίσης, αυτό σημαίνει ότι η μέση κύρια έκδοση ενός πρόσφατες αλλαγές σε μία ή περισσότερες cmdlet για.  Επομένως, για παράδειγμα, έκδοση 1.7.0 είναι μια επείγουσα διεύθυνση μια αλλαγή πρόσφατες στις εκδόσεις 1.x του Azure PowerShell.

Για περισσότερες πληροφορίες σχετικά με τις πρακτικές διαχείρισης σημασιολογίας εκδόσεων στο Azure PowerShell, ανατρέξτε στο θέμα της σημασιολογίας προδιαγραφής διαχείρισης εκδόσεων στο: http://semver.org
 
Για να λάβετε τα πιο πρόσφατα API, θα πρέπει να χρησιμοποιείτε την έκδοση 2.x. Αλλά εάν έχετε γράψει δέσμες ενεργειών σε σχέση με την έκδοση 1.x και δεν θέλετε να απορροφά τις πρόσφατες αλλαγές στην έκδοση 2.x περιγράφεται στο το 2.x [Σημειώσεις έκδοσης](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md), στη συνέχεια, θα πρέπει να εγκαταστήσετε 1.7.0.

Μια ασυμφωνία έκδοση μπορεί να έχει ως αποτέλεσμα εάν είναι εγκατεστημένο την πιο πρόσφατη έκδοση της λειτουργικής μονάδας προφίλ και, στη συνέχεια φορτώνεται μια παλαιότερη έκδοση του μια λειτουργική μονάδα που εξαρτάται από το. Ο πιο απλός τρόπος για να επιλύσετε αυτό το ζήτημα είναι να εγκαταστήσετε από την πιο πρόσφατη .msi. Το .msi εκκαθαρίζει αυτόματα από παλαιότερες εκδόσεις των λειτουργικών μονάδων.
 
###<a name="installing-module-versions-side-by-side"></a>Κατά την εγκατάσταση της λειτουργικής μονάδας εκδόσεις δίπλα-δίπλα

2.1.0 (και έκδοση 1.2.6 για AzureStack) είναι η πρώτη εκδόσεις λειτουργική μονάδα έχει σχεδιαστεί για να έχει εγκατασταθεί και χρησιμοποιούνται σε παράθεση. Επειδή το Azure PowerShell χρησιμοποιεί δυαδικό λειτουργικές μονάδες, πρέπει να ανοίξετε ένα νέο παράθυρο του PowerShell και χρησιμοποιήστε **Εισαγωγή-λειτουργική μονάδα** για να εισαγάγετε μια συγκεκριμένη έκδοση της τα cmdlet AzureRM:

    Import-Module AzureRM -RequiredVersion 2.1.0**

Εκδόσεις πριν από την 2.1.0 (εκτός των 1.2.6) δεν λειτουργεί καλά δίπλα-δίπλα με άλλες εκδόσεις λειτουργική μονάδα Azure PowerShell. Κατά τη φόρτωση σε μια παλαιότερη έκδοση του Azure PowerShell ενότητες χρησιμοποιώντας μια εντολή όπως τα παραπάνω, συμβατή εκδόσεις της λειτουργικής μονάδας **AzureRM.Profile** θα φορτώνεται, με αποτέλεσμα τα cmdlet που σας ζητά να συνδεθείτε στο κάθε φορά που εκτελείτε ένα cmdlet του, ακόμα και αν έχετε συνδεθεί.

Ο ευκολότερος τρόπος για να επιλύσετε αυτό είναι να εγκαταστήσετε την πιο πρόσφατη Azure PowerShell από το WebPI τροφοδοσίας ή .msi – αυτό καταργεί προηγούμενες εκδόσεις των λειτουργικών μονάδων εγκατεστημένο από τη συλλογή. 

Σημειώστε ότι λειτουργικές μονάδες Azure και AzureRM έχουν εξαρτήσεις από κοινού, επομένως εάν χρησιμοποιείτε δύο λειτουργικές μονάδες, κατά την ενημέρωση μία, θα πρέπει να ενημερώσετε και τα δύο. Παλαιότερες εκδόσεις του τη λειτουργική μονάδα Azure έχουν το ίδιο θέμα με λειτουργικής μονάδας σε παράθεση φόρτωσης που προηγούμενες εκδόσεις της λειτουργικής μονάδας AzureRM έχουν.

<a id="Install"></a>
## <a name="step-1-install"></a>Βήμα 1: εγκατάσταση

Ακολουθούν τις δύο μεθόδους σύμφωνα με τις οποίες μπορείτε να εγκαταστήσετε το Azure PowerShell. Μπορείτε να το εγκαταστήσετε από τη συλλογή του PowerShell ή το WebPI.

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>Κατά την εγκατάσταση του PowerShell Azure από τη συλλογή του PowerShell

Ο καλύτερος τρόπος είναι να χρησιμοποιήσετε τη συλλογή του PowerShell. Χρειάζεστε τη λειτουργική μονάδα PowerShellGet να χρησιμοποιήσετε τη συλλογή του PowerShell. Αυτή είναι διαθέσιμη εδώ: [PowerShellGallery.com](https://www.powershellgallery.com/)

Εγκατάσταση του Azure PowerShell 1.3.0 ή μεγαλύτερη από τη συλλογή PowerShell χρησιμοποιώντας μια αναβαθμισμένη εντολών του Windows PowerShell ή περιβάλλον ενσωματωμένη δέσμες ενεργειών στο PowerShell (ISE) χρησιμοποιώντας τις παρακάτω εντολές:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>Περισσότερες πληροφορίες σχετικά με αυτές τις εντολές

- **Εγκατάσταση-λειτουργική μονάδα AzureRM** εγκαθιστά μια λειτουργική μονάδα συνάθροισης για τα cmdlet διαχείρισης πόρων Azure. Εξαρτάται από τη λειτουργική μονάδα AzureRM 
- μια περιοχή συγκεκριμένη έκδοση για κάθε λειτουργική μονάδα Azure διαχείριση πόρων. Στην περιοχή έκδοση περιλαμβάνεται διασφαλίζει ότι δεν υπάρχουν πρόσφατες αλλαγές λειτουργική μονάδα μπορούν να έχουν περιλαμβάνονται κατά την εγκατάσταση του AzureRM λειτουργικές μονάδες με την ίδια κύρια έκδοση. Κατά την εγκατάσταση της λειτουργικής μονάδας AzureRM, οποιαδήποτε λειτουργική μονάδα Azure από διαχειριστή πόρων που δεν έχει εγκατασταθεί προηγουμένως θα ληφθεί και θα εγκατασταθεί από τη συλλογή του PowerShell. Για περισσότερες πληροφορίες σχετικά με το σημασιολογίας διαχείρισης εκδόσεων που χρησιμοποιούνται από λειτουργικές μονάδες Azure PowerShell, ανατρέξτε στο θέμα [semver.org](http://semver.org). 
- **Εγκατάσταση-λειτουργική μονάδα Azure** εγκαθιστά τη λειτουργική μονάδα Azure. Η ενότητα αυτή είναι η λειτουργική μονάδα υπηρεσίας διαχείρισης από το Azure PowerShell 0.9.x. Αυτό θα πρέπει να έχετε όχι των κύριων αλλαγές και να χρησιμοποιηθούν εναλλάξ για την προηγούμενη έκδοση του τη λειτουργική μονάδα Azure.

###<a name="installing-azure-powershell-from-webpi"></a>Κατά την εγκατάσταση του PowerShell Azure από WebPI

Κατά την εγκατάσταση του Azure PowerShell 1.0 και μεγαλύτερες από WebPI είναι η ίδια όπως ήταν για 0.9.x. Κάντε λήψη [Του PowerShell Azure](http://aka.ms/webpi-azps) και ξεκινήστε την εγκατάσταση. Εάν έχετε Azure PowerShell 0.9.x εγκατασταθεί, έκδοση 0.9.x θα καταργηθεί η εγκατάσταση ως μέρος της αναβάθμισης. Εάν έχετε εγκαταστήσει λειτουργικές μονάδες Azure PowerShell από τη συλλογή του PowerShell, το πρόγραμμα εγκατάστασης καταργεί αυτόματα τις ενότητες πριν από την εγκατάσταση για να βεβαιωθείτε ότι ένα συνεπή περιβάλλον Azure PowerShell.

> [AZURE.NOTE] Εάν έχετε εγκαταστήσει προηγουμένως Azure λειτουργικές μονάδες από τη συλλογή του PowerShell, το πρόγραμμα εγκατάστασης θα τα καταργήσετε αυτόματα. Αυτό δεν επιτρέπει σύγχυση σχετικά με το ποιες εκδόσεις λειτουργική μονάδα που έχετε εγκαταστήσει και πού βρίσκονται. Συλλογή PowerShell λειτουργικές μονάδες κανονικά θα εγκαταστήσετε **%ProgramFiles%\WindowsPowerShell\Modules**. Αντίθετα, το πρόγραμμα εγκατάστασης του WebPI θα εγκαταστήσετε το Azure λειτουργικές μονάδες στο * *% ProgramFiles (x86) %\Microsoft SDKs\Azure\PowerShell\**. Εάν προκύψει σφάλμα κατά την εγκατάσταση, μπορείτε να καταργήσετε με μη αυτόματο τρόπο το Azure* φακέλους στο φάκελο **%ProgramFiles%\WindowsPowerShell\Modules** και δοκιμάστε ξανά την εγκατάσταση.

Μόλις ολοκληρωθεί η εγκατάσταση, το ```$env:PSModulePath``` ρύθμιση θα πρέπει να συμπεριλάβετε τους φακέλους που περιέχει τα cmdlet του Azure PowerShell.

> [AZURE.NOTE] Υπάρχει ένα γνωστό θέμα με το PowerShell **$env: PSModulePath** που μπορεί να προκύψουν κατά την εγκατάσταση από WebPI. Εάν ο υπολογιστής σας απαιτεί επανεκκίνηση λόγω ενημερώσεων του συστήματος ή άλλες εγκαταστάσεις, ενδέχεται να προκαλέσει ενημερώσεις **$env: PSModulePath** για να συμπεριλάβετε τη διαδρομή όπου είναι εγκατεστημένο το Azure PowerShell. Αν συμβεί αυτό, ενδέχεται να δείτε ένα μήνυμα 'cmdlet δεν αναγνωρίζεται' όταν προσπαθείτε να χρησιμοποιήσετε το cmdlet του Azure PowerShell μετά την εγκατάσταση ή αναβάθμιση. Αν συμβεί αυτό, με την επανεκκίνηση του υπολογιστή, θα πρέπει να διορθώσετε το πρόβλημα.

Εάν εμφανιστεί ένα μήνυμα όπως το εξής κατά την προσπάθεια φόρτωση ή η εκτέλεση των cmdlet:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

Αυτό μπορεί να διορθωθεί με επανεκκίνηση του υπολογιστή ή εισαγωγή τα cmdlet από C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\ ως εξής (όπου XXXX είναι η έκδοση του PowerShell εγκατεστημένο:

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>Βήμα 2: Έναρξη
Μπορείτε να εκτελέσετε τα cmdlet από την τυπική κονσόλας Windows PowerShell ή από το PowerShell ενσωματωμένη δέσμες ενεργειών περιβάλλον (ISE).
Η μέθοδος που χρησιμοποιείτε για να ανοίξετε είτε κονσόλα εξαρτάται από την έκδοση των Windows που χρησιμοποιείτε:

- Σε υπολογιστή που εκτελεί τουλάχιστον Windows 8 ή Windows Server 2012, μπορείτε να χρησιμοποιήσετε την ενσωματωμένη αναζήτηση. Από την οθόνη **έναρξης** , αρχίστε να πληκτρολογείτε power. Αυτή η διαδικασία επιστρέφει ένα περιορισμένο λίστα των εφαρμογών που περιλαμβάνει το Windows PowerShell. Για να ανοίξετε την κονσόλα, κάντε κλικ είτε εφαρμογή. (Για να καρφιτσώσετε της εφαρμογής στην οθόνη **έναρξης** , κάντε δεξιό κλικ στο εικονίδιο του.)

- Σε έναν υπολογιστή που εκτελεί μια έκδοση παλαιότερη από το Windows 8 ή Windows Server 2012, χρησιμοποιήστε το **μενού "Έναρξη"**. Από το μενού **Έναρξη** , κάντε κλικ στην επιλογή **Όλα τα προγράμματα**, κάντε κλικ στην επιλογή **Βοηθήματα**, κάντε κλικ στο φάκελο **Του Windows PowerShell** και, στη συνέχεια, κάντε κλικ στην επιλογή **Windows PowerShell**.

Μπορείτε επίσης να εκτελέσετε το **Windows PowerShell ISE** να χρησιμοποιήσετε στοιχεία του μενού και συντομεύσεις πληκτρολογίου για να εκτελούν πολλές από τις ίδιες εργασίες που θα πρέπει να κάνετε στην κονσόλα του Windows PowerShell. Για να χρησιμοποιήσετε το ISE, στην κονσόλα του Windows PowerShell, Cmd.exe, ή στο πλαίσιο **Εκτέλεση** , πληκτρολογήστε, **powershell_ise.exe**.

###<a name="commands-to-help-you-get-started"></a>Εντολές για να σας βοηθήσουν να ξεκινήσετε

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>Βήμα 3: σύνδεση
Τα cmdlet πρέπει τη συνδρομή σας, ώστε να μπορούν να διαχειρίζονται τις υπηρεσίες σας. Μπορείτε να αγοράσετε μια συνδρομή του Azure, εάν δεν έχετε ήδη ένα. Για οδηγίες, ανατρέξτε στο θέμα [Πώς να αγοράσετε Azure](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Τύπος **Σύνδεσης AzureRmAccount**

2. Πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου και τον κωδικό πρόσβασης που σχετίζεται με το λογαριασμό σας. Azure πραγματοποιεί έλεγχο ταυτότητας και αποθηκεύει τις πληροφορίες διαπιστευτηρίων και, στη συνέχεια, κλείνει το παράθυρο.

--Ή--

Πραγματοποιήστε είσοδο στο την εργασία σας ή το σχολικό λογαριασμό:

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] Εάν έχετε περισσότερους από έναν μισθωτή που σχετίζεται με τον εταιρικό λογαριασμό σας, καθορίστε την παράμετρο TenantId:

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] Αυτό το αρχείο καταγραφής μη αλληλεπιδραστική στη μέθοδο λειτουργεί μόνο με εταιρικό ή σχολικό λογαριασμό. Έναν εταιρικό ή σχολικό λογαριασμό είναι ένας χρήστης που γίνεται από την εργασία ή το σχολείο, και ορισμός της παρουσίας Azure Active Directory για την εργασία ή το σχολείο. Εάν δεν έχετε έναν εταιρικό ή σχολικό λογαριασμό και χρησιμοποιείτε ένα λογαριασμό Microsoft για να συνδεθείτε στο Azure τη συνδρομή σας, μπορείτε εύκολα να δημιουργήσετε μία ακολουθώντας τα παρακάτω βήματα.

> 1. Συνδεθείτε [πύλη Azure κλασική](https://manage.windowsazure.com)και κάντε κλικ στην **Υπηρεσία καταλόγου Active Directory**.

> 2. Εάν υπάρχει καμία καταλόγου, επιλέξτε **Δημιουργία καταλόγου σας** και δώστε τις απαιτούμενες πληροφορίες.

> 3. Επιλέξτε σας κατάλογο και να προσθέσετε έναν νέο χρήστη. Αυτόν το χρήστη να συνδεθείτε χρησιμοποιώντας έναν εταιρικό ή σχολικό λογαριασμό. Κατά τη δημιουργία του χρήστη, θα είναι που παρέχονται με μια διεύθυνση ηλεκτρονικού ταχυδρομείου για το χρήστη και ένα προσωρινό κωδικό πρόσβασης. Αποθηκεύστε αυτές τις πληροφορίες, όπως χρησιμοποιούνται στο βήμα 5 παρακάτω.

> 4. Από την πύλη του Azure κλασική, επιλέξτε **Ρυθμίσεις** και, στη συνέχεια, επιλέξτε **διαχειριστές**. Επιλέξτε **Προσθήκη**και προσθήκη νέου χρήστη ως διαχειριστής από κοινού. Αυτό σας επιτρέπει την εταιρικό ή σχολικό λογαριασμό για να διαχειριστείτε τη συνδρομή σας στο Azure.

> 5. Τέλος, αποσυνδέεστε από το Azure κλασική πύλη και, στη συνέχεια, συνδεθείτε ξανά χρησιμοποιώντας το εταιρικό ή σχολικό λογαριασμό. Εάν αυτή είναι η πρώτη καταγραφή χρόνου με αυτόν το λογαριασμό, θα σας ζητηθεί να αλλάξετε τον κωδικό πρόσβασης.

> Για περισσότερες πληροφορίες σχετικά με την εγγραφή στο Microsoft Azure με εταιρικό ή σχολικό λογαριασμό, ανατρέξτε στο θέμα [εγγραφή για το Microsoft Azure ως μια εταιρεία](./active-directory/sign-up-organization.md).

> Για περισσότερες πληροφορίες σχετικά με τη Διαχείριση τον έλεγχο ταυτότητας και τη συνδρομή στο Azure, ανατρέξτε στο θέμα [Διαχείριση λογαριασμών, συνδρομές και ρόλοι διαχείρισης](http://go.microsoft.com/fwlink/?LinkId=324796).

### <a name="view-account-and-subscription-details"></a>Προβολή λεπτομερειών λογαριασμού και συνδρομών

Μπορείτε να έχετε πολλούς λογαριασμούς και συνδρομές διαθέσιμο για χρήση από Azure PowerShell. Μπορείτε να προσθέσετε πολλούς λογαριασμούς, εκτελώντας **Προσθήκη AzureRmAccount** περισσότερες από μία φορές.

Για να εμφανίζονται οι διαθέσιμες λογαριασμοί Azure, πληκτρολογήστε **Get-AzureAccount**.

Για να εμφανίσετε όλες τις συνδρομές σας Azure, πληκτρολογήστε **Get-AzureRmSubscription**.

##<a id="Help"></a>Λήψη Βοήθειας##

Αυτοί οι πόροι παρέχει βοήθεια για συγκεκριμένες cmdlet για:


-   Από εντός της κονσόλας, μπορείτε να χρησιμοποιήσετε το ενσωματωμένο σύστημα Βοήθειας. Το cmdlet **Get-Βοήθειας** παρέχει πρόσβαση σε αυτό το σύστημα. 

- Για βοήθεια από την Κοινότητα, δοκιμάστε αυτές τις δημοφιλείς φόρουμ:

 - [Azure φόρουμ στο MSDN]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [StackOverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>Μάθε περισσότερα


Ανατρέξτε στους παρακάτω πόρους για να μάθετε περισσότερα σχετικά με τη χρήση τα cmdlet:

Για βασικές οδηγίες σχετικά με τη χρήση του Windows PowerShell, ανατρέξτε στο θέμα [Χρήση του Windows PowerShell](http://go.microsoft.com/fwlink/p/?LinkId=321939).

Για πληροφορίες αναφοράς σχετικά με τα cmdlet, ανατρέξτε στο θέμα [Αναφορά Cmdlet Azure](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx).

Για δείγματα δεσμών ενεργειών και οδηγίες για να σας βοηθήσει να μάθετε πώς μπορείτε να χρησιμοποιήσετε δέσμες ενεργειών για τη Διαχείριση Azure, ανατρέξτε στο [Κέντρο δεσμών ενεργειών](http://go.microsoft.com/fwlink/p/?LinkId=321940).

