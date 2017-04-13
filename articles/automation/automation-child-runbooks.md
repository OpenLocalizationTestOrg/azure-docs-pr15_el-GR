<properties 
   pageTitle="Runbooks θυγατρικό στο Azure Automation | Microsoft Azure"
   description="Περιγράφει τις διαφορετικές μεθόδους για την εκκίνηση ενός runbook στο Azure αυτοματισμού από μια άλλη runbook και κοινή χρήση πληροφοριών μεταξύ τους."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>Runbooks θυγατρικό στο Azure Automation

Είναι βέλτιστη πρακτική στο Azure αυτοματισμού για να γράψετε με δυνατότητα επανάληψης χρήσης, λειτουργική runbooks με μια συνάρτηση διακριτό που μπορούν να χρησιμοποιηθούν από άλλα runbooks. Ένα γονικό runbook συχνά θα καλέσετε μία ή περισσότερες runbooks θυγατρικό για να εκτελέσετε λειτουργίες που απαιτείται. Υπάρχουν δύο τρόποι για να καλέσετε μια runbook θυγατρικών και κάθε έχει σημαντικές διαφορές που πρέπει να κατανοήσετε έτσι ώστε να μπορείτε να προσδιορίσετε την οποία θα είναι καλύτερη για σας διαφορετικά σενάρια.

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>Κλήση μιας runbook θυγατρικό με χρήση ενσωματωμένων εκτέλεσης

Για να καλέσετε μια ενσωματωμένη runbook από μια άλλη runbook, χρησιμοποιήστε το όνομα του runbook και δώστε τις τιμές για τις παραμέτρους του ακριβώς όπως θα χρησιμοποιούσατε μια δραστηριότητα ή το cmdlet.  Όλες οι runbooks στον ίδιο λογαριασμό αυτοματισμού είναι διαθέσιμες σε όλους τους άλλους να χρησιμοποιηθεί με αυτόν τον τρόπο. Θα περιμένετε γονικής runbook για θυγατρικό runbook για να ολοκληρωθεί πριν περάσετε στην επόμενη γραμμή και κανένα αποτέλεσμα επιστρέφεται απευθείας με το γονικό στοιχείο.

Όταν καλείτε μια ενσωματωμένη runbook, εκτελείται σε την ίδια εργασία ως γονικής runbook. Δεν θα υπάρξει καμία ένδειξη στο ιστορικό εργασία του runbook παιδί που το εκτελέσατε. Τυχόν εξαιρέσεις και οποιαδήποτε ροή εξόδου από το θυγατρικό runbook θα συσχετιστεί με το γονικό στοιχείο. Αυτό έχει ως αποτέλεσμα λιγότερα εργασίες και κάνουν ευκολότερη για την παρακολούθηση και για την αντιμετώπιση προβλημάτων εφόσον τυχόν εξαιρέσεις που ανακύπτουν από runbook θυγατρικό και κάποια από τα αποτελέσματά ροή έχουν συσχετιστεί με την εργασία γονικό στοιχείο.

Όταν δημοσιευτεί ένα runbook, πρέπει να δημοσιευτεί ήδη οποιαδήποτε runbooks παιδί που καλεί. Αυτό συμβαίνει επειδή αυτοματισμού Azure δημιουργεί μια συσχέτιση με οποιαδήποτε runbooks θυγατρικό όταν ένα runbook έχει μεταγλωττιστεί. Εάν δεν είναι, γονικής runbook θα εμφανίζονται για να δημοσιεύσετε σωστά, αλλά θα δημιουργήσει μια εξαίρεση κατά την εκκίνηση. Εάν συμβαίνει αυτό, μπορείτε να αναδημοσίευση γονικής runbook προκειμένου να αναφέρονται σωστά το runbooks παιδί. Δεν χρειάζεται να δημοσιεύσετε ξανά γονικής runbook εάν οποιοδήποτε από τα runbooks θυγατρικό αλλάζουν, επειδή η συσχέτιση θα έχουν ήδη δημιουργηθεί.

Τις παραμέτρους μιας runbook παιδί που ονομάζεται ενσωματωμένη μπορεί να είναι οποιονδήποτε τύπο δεδομένων, συμπεριλαμβανομένων των σύνθετα αντικείμενα και υπάρχει καμία [σειριοποίησης JSON](automation-starting-a-runbook.md#runbook-parameters) καθώς δεν υπάρχει κατά την εκκίνηση του runbook χρησιμοποιώντας την πύλη διαχείρισης Azure ή με το cmdlet AzureRmAutomationRunbook Έναρξη.


### <a name="runbook-types"></a>Τύποι Runbook

Ποιοι τύποι μπορούν να καλέσουν μεταξύ τους:

- Μια [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) και [γραφικών runbooks](automation-runbook-types.md#graphical-runbooks) να καλέσετε κάθε άλλες ενσωματωμένες (και τα δύο είναι βάσει του PowerShell).
- Μια [ροή εργασίας του PowerShell runbook](automation-runbook-types.md#powershell-workflow-runbooks) και τη ροή εργασίας γραφικών PowerShell runbooks μπορούν να καλέσουν κάθε άλλες ενσωματωμένες (και τα δύο είναι PowerShell ροής εργασίας με βάση)
- Οι τύποι PowerShell και τους τύπους PowerShell ροής εργασίας δεν είναι δυνατό να καλέστε ενσωματωμένη μεταξύ τους και πρέπει να χρησιμοποιήσετε AzureRmAutomationRunbook Έναρξη.
    
Όταν δημοσιεύσετε θέμα σειρά:

- Η σειρά δημοσίευση των runbooks υποθέσεις μόνο για το PowerShell ροής εργασίας και τη ροή εργασίας γραφικών PowerShell runbooks.


Όταν καλείτε μια runbook θυγατρικό Graphical ή ροή εργασίας του PowerShell ενσωματωμένη εκτέλεσης, μπορείτε απλώς να χρησιμοποιήσετε το όνομα του runbook.  Όταν καλείτε μια runbook θυγατρικό PowerShell, που πρέπει να βέλος που βρίσκεται δίπλα στο όνομά της με *.\\ * Για να καθορίσετε ότι η δέσμη ενεργειών βρίσκεται στον τοπικό κατάλογο. 

### <a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα καλεί μια runbook θυγατρικό δοκιμής που δέχεται τρεις παράμετροι, μια σύνθετη αντικειμένου, ακέραιος και μια τιμή boolean. Το αποτέλεσμα του runbook θυγατρικό έχει αντιστοιχιστεί σε μια μεταβλητή.  Σε αυτήν την περίπτωση, runbook θυγατρικό είναι μια runbook PowerShell ροής εργασίας

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Ακολουθεί το ίδιο παράδειγμα που χρησιμοποιεί μια runbook PowerShell ως το παιδί.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>Ξεκινώντας μια runbook θυγατρικό χρησιμοποιώντας cmdlet

Μπορείτε να χρησιμοποιήσετε το cmdlet [AzureRmAutomationRunbook έναρξης](https://msdn.microsoft.com/library/mt603661.aspx) για να ξεκινήσετε μια runbook όπως περιγράφεται στο [για να ξεκινήσετε μια runbook με το Windows PowerShell](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Υπάρχουν δύο τρόποι χρήσης για αυτό το cmdlet.  Σε μια λειτουργία, το cmdlet επιστρέφει το αναγνωριστικό εργασίας, μόλις η εργασία θυγατρικό δημιουργείται για runbook θυγατρικό.  Σε τα άλλα λειτουργία, που ενεργοποιείτε, καθορίζοντας την **-περιμένετε** παράμετρο, το cmdlet θα περιμένετε έως ότου το παιδί ολοκληρώσει την εργασία και θα επιστρέψει την έξοδο από το θυγατρικό runbook.

Η εργασία από ένα θυγατρικό runbook αποτελέσματα με μια cmdlet θα εκτελείται σε μια ξεχωριστή εργασία από το γονικό στοιχείο runbook. Αυτό έχει ως αποτέλεσμα περισσότερες εργασίες από την κλήση τα ενσωματωμένα runbook και κάνει πιο δύσκολο για την παρακολούθηση. Το γονικό στοιχείο μπορεί να ξεκινήσει πολλών runbooks θυγατρικό ασύγχρονα χωρίς αναμονή για κάθε για να ολοκληρωθεί. Για συγκεκριμένη ίδιο είδος παράλληλη εκτέλεση καλεί τα ενσωματωμένα runbooks παιδί, γονικής runbook θα πρέπει να χρησιμοποιήσετε το [παράλληλο λέξεων-κλειδιών](automation-powershell-workflow.md#parallel-processing).

Παράμετροι για ένα θυγατρικό runbook αποτελέσματα με μια cmdlet παρέχονται ως ένα hashtable όπως περιγράφεται στο [Runbook παράμετροι](automation-starting-a-runbook.md#runbook-parameters). Μόνο οι τύποι απλά δεδομένα τα οποία μπορεί να χρησιμοποιηθεί. Εάν η runbook έχει μια παράμετρο με έναν τύπο σύνθετα δεδομένα, στη συνέχεια, το πρέπει να καλείται ενσωματωμένη.

### <a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα ξεκινά μια runbook θυγατρικό με παραμέτρους και, στη συνέχεια, περιμένει για την ολοκλήρωση χρησιμοποιώντας την έναρξη-AzureRmAutomationRunbook-περιμένετε παραμέτρου. Μόλις ολοκληρωθεί, τα αποτελέσματά που συλλέγονται από το θυγατρικό runbook.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Σύγκριση των μεθόδων για την κλήση μιας runbook παιδί

Ο παρακάτω πίνακας συνοψίζει τις διαφορές μεταξύ των δύο μεθόδων για την κλήση μιας runbook από μια άλλη runbook.

| | Ενσωματωμένη| Cmdlet|
|:---|:---|:---|
|Εργασία|Εκτελέστε runbooks θυγατρικό στην ίδια δουλειά ως το γονικό στοιχείο.|Δημιουργείται μια ξεχωριστή εργασία για runbook θυγατρικό.|
|Εκτέλεση|Γονικής runbook αναμένει θυγατρικό runbook για να ολοκληρωθεί πριν να συνεχίσετε.|Γονικής runbook συνεχίζει αμέσως μετά την εκκίνηση runbook θυγατρικό *ή* γονικής runbook χρειάζεται να περιμένει για το έργο θυγατρικό για να ολοκληρώσετε.|
|Εξόδου|Γονικής runbook να μεταβείτε απευθείας εξόδου από το παιδί runbook.|Γονικής runbook πρέπει να ανακτήσετε εξόδου από το θυγατρικό runbook εργασία *ή* γονικής runbook να απευθείας λάβετε εξόδου από το παιδί runbook.|
|Παράμετροι|Τιμές για τις παραμέτρους runbook θυγατρικό καθορίζονται ξεχωριστά και να χρησιμοποιήσετε οποιονδήποτε τύπο δεδομένων.|Τιμές για τις παραμέτρους runbook θυγατρικό πρέπει να συνδυάζονται σε ένα μεμονωμένο hashtable και μπορεί να περιλαμβάνει μόνο απλό, πίνακα και τύπους που σειριοποίησης JSON αξιοποίηση δεδομένων αντικειμένου.|
|Αυτοματοποίηση λογαριασμού|Γονικής runbook να χρησιμοποιήσετε μόνο το παιδί runbook στον ίδιο λογαριασμό αυτοματισμού.|Γονικής runbook να χρησιμοποιήσετε runbook θυγατρικό από οποιονδήποτε λογαριασμό αυτοματισμού από την ίδια συνδρομή Azure και ακόμη και μια διαφορετική συνδρομή, εάν έχετε μια σύνδεση σε αυτό.|
|Δημοσίευση|Θυγατρικό runbook πρέπει να δημοσιεύονται πριν από τη δημοσίευση γονικής runbook.|Θυγατρικό runbook πρέπει να δημοσιευτεί οποιαδήποτε στιγμή, πριν ξεκινήσει γονικής runbook.|

## <a name="next-steps"></a>Επόμενα βήματα

- [Έναρξη μιας runbook σε αυτοματισμού Azure](automation-starting-a-runbook.md)
- [Αποτέλεσμα Runbook και τα μηνύματα στο Azure Automation](automation-runbook-output-and-messages.md)