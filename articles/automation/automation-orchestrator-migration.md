<properties
   pageTitle="Μετάβαση από το Orchestrator Azure αυτοματισμού | Microsoft Azure"
   description="Περιγράφει τον τρόπο για τη μετεγκατάσταση runbooks και την ενοποίηση πακέτα από το σύστημα κέντρο Orchestrator στο Azure αυτοματισμού."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Μετάβαση από το Orchestrator Azure αυτοματισμού (Beta)

Runbooks στο [Σύστημα κέντρου Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) βασίζονται στις δραστηριότητες από πακέτα ενοποίησης που έχουν συνταχθεί ειδικά για Orchestrator ενώ runbooks στο Azure Automation που βασίζονται σε Windows PowerShell.  [Γραφικών runbooks](automation-runbook-types.md#graphical-runbooks) στο Azure Automation έχουν παρόμοια εμφάνιση σε Orchestrator runbooks με τις δραστηριότητές τους που αντιπροσωπεύει cmdlet του PowerShell, θυγατρικό runbooks και περιουσιακών στοιχείων.

Το [Κιτ εργαλείων μετεγκατάστασης Orchestrator σύστημα κέντρο](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) περιλαμβάνει εργαλεία για να σας βοηθήσει να τη μετατροπή runbooks από Orchestrator Azure αυτοματισμού.  Εκτός από τη μετατροπή του runbooks τον εαυτό τους, πρέπει να μετατρέψετε τα πακέτα ενοποίηση με τις δραστηριότητες που χρησιμοποιούν το runbooks λειτουργικές μονάδες ενοποίηση με το cmdlet του Windows PowerShell.  

Ακολουθεί η βασική διαδικασία για τη μετατροπή Orchestrator runbooks Azure αυτοματισμού.  Κάθε ένα από αυτά τα βήματα περιγράφεται λεπτομερώς στις παρακάτω ενότητες.

1.  Κάντε λήψη του [Κιτ εργαλείων μετεγκατάστασης Orchestrator κέντρο συστήματος](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) που περιέχει τα εργαλεία και οι λειτουργικές μονάδες που αναφέρονται σε αυτό το άρθρο.
2.  Εισαγάγετε [τυπικές δραστηριότητες λειτουργικής μονάδας](#standard-activities-module) στο Azure αυτοματισμού.  Αυτό περιλαμβάνει έχουν μετατραπεί εκδόσεις του τυπικές δραστηριότητες Orchestrator που μπορεί να χρησιμοποιηθεί από runbooks έχουν μετατραπεί.
3.  Εισαγάγετε [Λειτουργικές μονάδες ενοποίησης Orchestrator σύστημα κέντρο](#system-center-orchestrator-integration-modules) στο Azure αυτοματισμού για αυτά τα πακέτα ενοποίησης που χρησιμοποιούνται από το runbooks που πρόσβαση στο κέντρο του συστήματος.
4.  Μετατροπή προσαρμοσμένων και άλλου κατασκευαστή πακέτα ενοποίηση με χρήση του [Μετατροπέα πακέτο ενοποίησης](#integration-pack-converter) και να εισαγάγετε στο Azure αυτοματισμού.
5.  Μετατροπή runbooks Orchestrator με χρήση του [Runbook μετατροπέα](#runbook-converter) και εγκαταστήστε στο Azure αυτοματισμού.
6.  Δημιουργία με μη αυτόματο τρόπο απαιτούμενα στοιχεία Orchestrator στο Azure αυτοματισμού εφόσον το μετατροπέα Runbook δεν μετατρέπει αυτούς τους πόρους.
7.  Ρύθμιση παραμέτρων [Υβριδικού Runbook εργασίας](#hybrid-runbook-worker) στο κέντρο τοπικών δεδομένων για να εκτελέσετε έχει μετατραπεί runbooks που θα έχουν πρόσβαση τοπικούς πόρους.

## <a name="service-management-automation"></a>Αυτοματοποίηση υπηρεσίας διαχείρισης

[Αυτοματοποίηση υπηρεσίας διαχείρισης](http://technet.microsoft.com/library/dn469260.aspx) (SMA) αποθηκεύει και εκτελεί runbooks στο κέντρο τοπικών δεδομένων όπως Orchestrator και χρησιμοποιεί το ίδιο λειτουργικές μονάδες ενοποίησης ως Azure αυτοματισμού. Του [Runbook μετατροπέα](#runbook-converter) μετατρέπει Orchestrator runbooks γραφικών runbooks Παρόλο που δεν υποστηρίζονται στο SMA.  Εξακολουθείτε να μπορείτε να εγκαταστήσετε τη [Βασική λειτουργική μονάδα δραστηριότητες](#standard-activities-module) και οι [Λειτουργικές μονάδες ενοποίησης Orchestrator σύστημα κέντρο](#system-center-orchestrator-integration-modules) σε SMA, αλλά πρέπει να κάνετε με μη αυτόματο τρόπο [επανεγγραφή runbooks σας](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Υβριδική Runbook εργαζόμενου

Runbooks στο Orchestrator είναι αποθηκευμένες σε ένα διακομιστή βάσης δεδομένων και να εκτελεστούν σε διακομιστές runbook, τόσο στο κέντρο τοπικών δεδομένων.  Runbooks στο Azure αυτοματισμού είναι αποθηκευμένα στο Azure cloud και να εκτελέσετε στο κέντρο τοπικών δεδομένων χρησιμοποιώντας [Υβριδική Runbook εργασίας](automation-hybrid-runbook-worker.md).  Πρόκειται για τον τρόπο που θα εκτελείται συνήθως runbooks μετατραπεί από Orchestrator γιατί είναι σχεδιασμένες για να εκτελέσετε σε τοπικούς διακομιστές.

## <a name="integration-pack-converter"></a>Ενοποίηση του πακέτου μετατροπέα

Ο μετατροπέας πακέτο ενοποίησης μετατρέπει πακέτα ενοποίησης που έχουν δημιουργηθεί με χρήση του [Κιτ εργαλείων ενοποίηση Orchestrator (OIT)](http://technet.microsoft.com/library/hh855853.aspx) για να ενοποίησης λειτουργικές μονάδες που βασίζεται σε Windows PowerShell που μπορεί να εισαχθεί στο αυτοματισμού Azure ή η υπηρεσία διαχείρισης αυτοματοποίηση.  

Όταν εκτελείτε το μετατροπέα πακέτο ενοποίησης, εμφανίζεται με έναν οδηγό που θα σας επιτρέψει να επιλέξετε ένα αρχείο πακέτου (.oip) ενοποίησης.  Ο οδηγός, στη συνέχεια, παραθέτει τις δραστηριότητες που περιλαμβάνονται σε εκείνο το πακέτο ενοποίησης και σας επιτρέπει να επιλέξετε την οποία θα μετεγκατασταθούν.  Όταν ολοκληρωθεί ο οδηγός, δημιουργεί μια λειτουργική μονάδα ενοποίησης που περιλαμβάνει μια αντίστοιχη cmdlet για κάθε μία από τις δραστηριότητες στο αρχικό pack ενοποίησης.


### <a name="parameters"></a>Παράμετροι

Οποιαδήποτε ιδιοτήτων μιας δραστηριότητας στο πακέτο ενοποίησης μετατρέπονται σε παραμέτρους το αντίστοιχο cmdlet στη λειτουργική μονάδα ενοποίησης.  Cmdlet του Windows PowerShell έχουν ενός συνόλου [κοινών παράμετροι](http://technet.microsoft.com/library/hh847884.aspx) που μπορούν να χρησιμοποιηθούν με όλα τα cmdlet.  Για παράδειγμα, η - λεπτομερής παράμετρος έχει ως αποτέλεσμα ένα cmdlet του για να εξαγάγετε λεπτομερείς πληροφορίες σχετικά με τη λειτουργία.  Το cmdlet δεν μπορεί να έχει μια παράμετρος με το ίδιο όνομα με την παράμετρο κοινές.  Εάν μια δραστηριότητα έχει μια ιδιότητα με το ίδιο όνομα με κοινές παραμέτρου, ο οδηγός θα σας ζητήσει να καταχωρήσετε ένα άλλο όνομα για την παράμετρο.

### <a name="monitor-activities"></a>Παρακολούθηση δραστηριοτήτων

Οθόνη runbooks στο Orchestrator ξεκινήσετε με μια [δραστηριότητα οθόνη](http://technet.microsoft.com/library/hh403827.aspx) και να εκτελέσετε συνεχώς αναμονή για να χρησιμοποιούνται από ένα συγκεκριμένο συμβάν.  Azure αυτοματισμού δεν υποστηρίζει runbooks οθόνη, ώστε να τις δραστηριότητες οθόνη σε το πακέτο ενοποίησης δεν θα μετατραπεί.  Αντί για αυτό, δημιουργείται ένα σύμβολο κράτησης θέσης cmdlet στη λειτουργική μονάδα ενοποίησης για τη δραστηριότητα οθόνη.  Αυτό το cmdlet δεν διαθέτει λειτουργία, αλλά να επιτρέπει σε οποιονδήποτε έχει μετατραπεί runbook που χρησιμοποιεί για να έχει εγκατασταθεί.  Αυτό runbook δεν θα μπορείτε να εκτελέσετε στο Azure αυτοματισμού, αλλά αυτό μπορεί να εγκατασταθεί, έτσι ώστε να μπορείτε να το τροποποιήσετε.

### <a name="integration-packs-that-cannot-be-converted"></a>Ενοποίηση πακέτα που δεν μπορεί να μετατραπεί

Ενοποίηση πακέτα που δεν έχουν δημιουργηθεί με OIT δεν μπορεί να μετατραπεί με τον μετατροπέα πακέτο ενοποίησης. Υπάρχουν επίσης ορισμένες πακέτα ενοποίησης που παρέχεται από τη Microsoft και τη συγκεκριμένη στιγμή δεν μπορεί να μετατραπεί με αυτό το εργαλείο.  Έχουν μετατραπεί εκδόσεις του αυτά τα πακέτα ενοποίηση έχουν [που παρέχεται για τη λήψη](#system-center-orchestrator-integration-modules) , έτσι ώστε να μπορούν να εγκατασταθούν στο αυτοματισμού Azure ή στην υπηρεσία διαχείρισης αυτοματοποίηση.


## <a name="standard-activities-module"></a>Τυπικές δραστηριότητες λειτουργική μονάδα

Orchestrator περιλαμβάνει ένα σύνολο [τυπικές δραστηριότητες](http://technet.microsoft.com/library/hh403832.aspx) που δεν περιλαμβάνονται σε ένα πακέτο ενοποίησης, αλλά που χρησιμοποιούνται από πολλά runbooks.  Η λειτουργική μονάδα τυπικές δραστηριότητες είναι μια λειτουργική μονάδα ενοποίησης που περιλαμβάνει μια ισοδύναμη cmdlet για κάθε μία από αυτές τις δραστηριότητες.  Πρέπει να εγκαταστήσετε αυτήν τη λειτουργική μονάδα ενοποίησης στο Azure Automation πριν από την εισαγωγή οποιαδήποτε έχουν μετατραπεί runbooks που χρησιμοποιούν μια τυπική δραστηριότητα.

Εκτός από την υποστήριξη έχει μετατραπεί runbooks, τα cmdlet στη λειτουργική μονάδα τυπικές δραστηριότητες μπορεί να χρησιμοποιηθεί από κάποιον εξοικειωμένοι με Orchestrator για να δημιουργήσετε νέα runbooks στο Azure αυτοματισμού.  Κατά τη λειτουργικότητα όλες οι τυπικές δραστηριότητες μπορούν να εκτελεστούν με cmdlet, ενδέχεται να λειτουργούν διαφορετικά.  Τα cmdlet στη λειτουργική μονάδα έχει μετατραπεί τυπικές δραστηριότητες θα λειτουργούν με τον ίδιο ως αντίστοιχο τις δραστηριότητές τους και χρησιμοποιήστε τις ίδιες παραμέτρους.  Αυτό μπορεί να βοηθήσει την υπάρχουσα σύνταξη runbook Orchestrator σε τους Μετάβαση σε runbooks Azure αυτοματισμού.

## <a name="system-center-orchestrator-integration-modules"></a>Λειτουργικές μονάδες ενοποίησης Orchestrator κέντρο του συστήματος

Η Microsoft παρέχει [ενοποίηση πακέτα](http://technet.microsoft.com/library/hh295851.aspx) για τη δημιουργία runbooks για την αυτοματοποίηση στοιχεία κέντρο του συστήματος και άλλα προϊόντα.  Ορισμένα από αυτά τα πακέτα ενοποίησης αυτήν τη στιγμή βάση OIT αλλά αυτήν τη στιγμή δεν μπορεί να μετατραπεί σε λειτουργικές μονάδες ενοποίησης λόγω γνωστά θέματα.  [Λειτουργικές μονάδες ενοποίησης Orchestrator σύστημα κέντρο](https://www.microsoft.com/download/details.aspx?id=49555) περιλαμβάνει έχουν μετατραπεί εκδόσεις των αυτά τα πακέτα ενοποίησης που μπορεί να εισαχθεί στο Azure αυτοματοποίησης και υπηρεσίας διαχείρισης αυτοματοποίησης.  

Από την έκδοση RTM αυτού του εργαλείου, θα δημοσιεύονται ενημερωμένες εκδόσεις των πακέτων ενοποίηση με βάση OIT που μπορεί να μετατραπεί με τον μετατροπέα πακέτο ενοποίησης.  Καθοδήγηση επίσης παρέχεται για να σας βοηθήσει να τη μετατροπή runbooks χρησιμοποιώντας δραστηριότητες από τα πακέτα ενοποίηση δεν βασίζεται σε OIT.

## <a name="runbook-converter"></a>Μετατροπέας Runbook

Ο μετατροπέας Runbook μετατρέπει Orchestrator runbooks σε [runbooks γραφικών](automation-runbook-types.md#graph-runbooks) που μπορεί να εισαχθεί στο Azure αυτοματισμού.  

Μετατροπέας Runbook έχει υλοποιηθεί ως λειτουργική μονάδα PowerShell με ένα cmdlet που ονομάζεται **ConvertFrom SCORunbook** που εκτελεί τη μετατροπή.  Κατά την εγκατάσταση του εργαλείου, θα δημιουργήσει μια συντόμευση σε μια περίοδο λειτουργίας PowerShell που φορτώνει το cmdlet.   

Ακολουθεί η βασική διαδικασία για να μετατρέψετε ένα runbook Orchestrator και εισαγάγετέ το στο Azure αυτοματισμού.  Οι παρακάτω ενότητες παρέχουν περαιτέρω λεπτομέρειες σε χρησιμοποιώντας το εργαλείο και εργασία με runbooks έχουν μετατραπεί.

1. Εξαγωγή ενός ή περισσότερων runbooks από Orchestrator.
2. Προμηθευτείτε ενοποίησης λειτουργικές μονάδες για όλες τις δραστηριότητες στο runbook.
3. Μετατρέψτε το runbooks Orchestrator στο αρχείο εξαγωγής.
4. Διαβάστε πληροφορίες στα αρχεία καταγραφής για να επικυρώσετε τη μετατροπή και για να καθορίσετε τις απαιτούμενες εργασίες μη αυτόματη.
4. Εισαγωγή runbooks έχει μετατραπεί σε Azure αυτοματισμού.
5. Δημιουργήστε τυχόν απαιτούμενα στοιχεία στο Azure αυτοματισμού.
6. Επεξεργασία runbook στο Azure αυτοματισμού για να τροποποιήσετε τις απαιτούμενες δραστηριότητες.

### <a name="using-runbook-converter"></a>Χρήση του μετατροπέα Runbook

Η σύνταξη για **ConvertFrom SCORunbook** είναι ως εξής:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath - διαδρομή του αρχείου εξαγωγής που περιέχει το runbooks για να μετατρέψετε.
- Λειτουργική μονάδα - λίστα οριοθετημένη ενοποίησης λειτουργικές μονάδες που περιέχει δραστηριότητες σε το runbooks.
- OutputFolder - διαδρομή προς το φάκελο για να δημιουργήσετε μετατρέπονται γραφικών runbooks. 


Το ακόλουθο παράδειγμα εντολής μετατρέπει το runbooks σε ένα αρχείο εξαγωγής που ονομάζεται **MyRunbooks.ois_export**.  Αυτές οι runbooks Χρησιμοποιήστε τα πακέτα ενοποίηση καταλόγου Active Directory και διαχείριση προστασίας δεδομένων.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>Αρχεία καταγραφής

Το πρόγραμμα μετατροπής Runbook θα δημιουργήσει τα ακόλουθα αρχεία καταγραφής στην ίδια θέση ως έχει μετατραπεί runbook.  Εάν υπάρχουν ήδη τα αρχεία, θα αντικατασταθούν με τις πληροφορίες από το τελευταίο μετατροπής.

| Αρχείο | Περιεχόμενα |
|:---|:---|
| Μετατροπέας Runbook - Progress.log | Λεπτομερείς οδηγίες της μετατροπής καθώς και πληροφορίες για κάθε δραστηριότητα με επιτυχία μετατρέπονται και προειδοποίηση για κάθε δραστηριότητα δεν μετατρέπονται. |
| Μετατροπέας Runbook - Summary.log  | Σύνοψη της τελευταίας μετατροπής συμπεριλαμβανομένων τυχόν προειδοποιήσεις και να παρακολουθήσετε τις εργασίες που πρέπει να εκτελέσετε όπως για να δημιουργήσετε μια μεταβλητή που απαιτείται για τη μετατροπή runbook.  |

### <a name="exporting-runbooks-from-orchestrator"></a>Εξαγωγή runbooks από Orchestrator

Το πρόγραμμα μετατροπής Runbook λειτουργεί με ένα αρχείο εξαγωγής από Orchestrator που περιέχει μία ή περισσότερες runbooks.  Θα δημιουργήσει μια αντίστοιχη αυτοματισμού Azure runbook για κάθε runbook Orchestrator στο αρχείο εξαγωγής.  

Για να εξαγάγετε ένα runbook από Orchestrator, κάντε δεξί κλικ στο όνομα του runbook στο πρόγραμμα σχεδίασης Runbook και επιλέξτε **Εξαγωγή**.  Για να εξαγάγετε όλα runbooks σε ένα φάκελο, κάντε δεξί κλικ στο όνομα του φακέλου και επιλέξτε **Εξαγωγή**.


### <a name="runbook-activities"></a>Δραστηριότητες Runbook

Το μετατροπέα Runbook μετατρέπει κάθε δραστηριότητα στο Orchestrator runbook αντίστοιχη δραστηριότητα στο Azure αυτοματισμού.  Για τις δραστηριότητες που δεν μπορεί να μετατραπεί, δημιουργείται μια δραστηριότητα κράτησης θέσης στο runbook με κείμενο προειδοποίησης.  Μετά την εισαγωγή runbook έχει μετατραπεί σε Azure αυτοματισμού, πρέπει να αντικαταστήσετε οποιαδήποτε από αυτές τις δραστηριότητες με έγκυρη δραστηριότητες που εκτελούν την απαιτούμενη λειτουργικότητα.

Οποιεσδήποτε δραστηριότητες Orchestrator στην τη [Βασική λειτουργική μονάδα δραστηριότητες](#standard-activities-module) θα μετατραπούν.  Υπάρχουν ορισμένες τυπικές δραστηριότητες Orchestrator που δεν βρίσκονται σε αυτήν τη λειτουργική μονάδα μέσω και δεν μετατρέπονται.  Για παράδειγμα, **Στείλτε το συμβάν πλατφόρμα** έχει χωρίς ισοδύναμη Azure αυτοματισμού εφόσον το συμβάν είναι συγκεκριμένη για Orchestrator.

[Οθόνη δραστηριότητες](https://technet.microsoft.com/library/hh403827.aspx) δεν μετατρέπονται εφόσον δεν υπάρχει αντίστοιχο τους στο Azure αυτοματισμού.  Η εξαίρεση είναι δραστηριότητες οθόνη στο [Μετατροπή ενσωμάτωσης πακέτα](#integration-pack-converter) που θα μετατραπούν σε τη δραστηριότητα κράτησης θέσης.

Οποιαδήποτε δραστηριότητα από μια [Μετατροπή ενσωμάτωσης πακέτο](#integration-pack-converter) θα μετατραπούν Εάν παρέχετε τη διαδρομή προς τη λειτουργική μονάδα ενοποίηση με την παράμετρο **λειτουργικές μονάδες** .  Για πακέτα ενοποίησης κέντρο συστήματος, μπορείτε να χρησιμοποιήσετε τις [Μονάδες ενοποίησης Orchestrator κέντρο συστήματος](#system-center-orchestrator-integration-modules).


### <a name="orchestrator-resources"></a>Orchestrator πόροι

Το πρόγραμμα μετατροπής Runbook μετατρέπει μόνο runbooks, όχι άλλους πόρους Orchestrator όπως μετρητές, μεταβλητές ή συνδέσεις.  Μετρητές δεν υποστηρίζονται στο Azure αυτοματισμού.  Μεταβλητές και συνδέσεις υποστηρίζονται, αλλά πρέπει να τα δημιουργήσετε με μη αυτόματο τρόπο.  Τα αρχεία καταγραφής θα σας ενημερώσει εάν runbook απαιτεί εν λόγω πόρων και καθορίστε αντίστοιχο τους πόρους που χρειάζεστε για να δημιουργήσετε στο Azure αυτοματισμού για μετατροπή runbook για να λειτουργήσει σωστά.

Για παράδειγμα, μια runbook μπορεί να χρησιμοποιεί μια μεταβλητή για να συμπληρώσετε μια συγκεκριμένη τιμή σε μια δραστηριότητα.  Έχουν μετατραπεί runbook θα μετατρέψετε τη δραστηριότητα και καθορίστε έναν μεταβλητή περιουσιακών στοιχείων στο Azure αυτοματισμού με το ίδιο όνομα με τη μεταβλητή Orchestrator.  Αυτό θα να αναφερθεί στο αρχείο **Μετατροπέα Runbook - Summary.log** που δημιουργείται μετά τη μετατροπή.  Θα πρέπει να δημιουργήσετε με μη αυτόματο τρόπο αυτό μεταβλητής περιουσιακών στοιχείων στο Azure Automation πριν από τη χρήση runbook. 

 
### <a name="input-parameters"></a>Παράμετροι εισόδου

Runbooks στο Orchestrator αποδοχή παραμέτρων εισαγωγής με τη δραστηριότητα **Προετοιμασία δεδομένων** .  Εάν runbook τη μετατροπή περιλαμβάνει αυτήν τη δραστηριότητα, στη συνέχεια, δημιουργείται μια [παράμετρο εισόδου](automation-graphical-authoring-intro.md#runbook-input-and-output) στο runbook αυτοματισμού Azure για κάθε παράμετρο στη δραστηριότητα.  Δραστηριότητα [ελέγχου δέσμης ενεργειών ροής εργασίας](automation-graphical-authoring-intro.md#activities) έχει δημιουργηθεί στο έχουν μετατραπεί runbook που ανακτά και επιστρέφει την κάθε παράμετρο.  Οποιαδήποτε δραστηριότητες στο runbook που χρησιμοποιούν μια παράμετρο εισόδου αναφέρουν την έξοδο από αυτήν τη δραστηριότητα.

Ο λόγος που χρησιμοποιείται αυτήν τη στρατηγική είναι η καλύτερη αντικριστά τη λειτουργικότητα στο Orchestrator runbook.  Δραστηριότητες σε νέα γραφικά runbooks πρέπει να ανατρέξετε απευθείας σε μια προέλευση δεδομένων εισαγωγής Runbook παράμετροι εισόδου.


### <a name="invoke-runbook-activity"></a>Κλήση Runbook δραστηριότητας

Runbooks στο Orchestrator ξεκινήστε άλλες runbooks με τη δραστηριότητα **Runbook κλήση** . Εάν runbook τη μετατροπή περιλαμβάνει αυτήν τη δραστηριότητα και ορίστε την επιλογή **Αναμονή για την ολοκλήρωση** , στη συνέχεια, δραστηριότητα runbook δημιουργείται για αυτό στο runbook έχουν μετατραπεί.  Εάν δεν έχει οριστεί η επιλογή **Αναμονή για την ολοκλήρωση** , στη συνέχεια, δέσμη ενεργειών ροής εργασίας δημιουργείται μια δραστηριότητα που χρησιμοποιεί **AzureAutomationRunbook έναρξης** για να ξεκινήσετε runbook.  Μετά την εισαγωγή runbook έχει μετατραπεί σε Azure αυτοματισμού, πρέπει να τροποποιήσετε αυτήν τη δραστηριότητα με τις πληροφορίες που καθορίζονται στη δραστηριότητα.




## <a name="related-articles"></a>Σχετικά άρθρα

- [Σύστημα κέντρο 2012 - Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Αυτοματοποίηση υπηρεσίας διαχείρισης](https://technet.microsoft.com/library/dn469260.aspx)
- [Υβριδική Runbook εργαζόμενου](automation-hybrid-runbook-worker.md)
- [Τυπικές δραστηριότητες orchestrator](http://technet.microsoft.com/library/hh403832.aspx)
- [Κιτ εργαλείων μετεγκατάστασης Orchestrator Κέντρο λήψης αρχείων συστήματος](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 