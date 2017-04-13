<properties
   pageTitle="Ενοποίηση με λειτουργίες διαχείρισης οικογένεια (OMS) | Microsoft Azure"
   description="Εκτός από χρησιμοποιώντας τις τυπικές δυνατότητες του OMS, μπορείτε να ενοποιήσετε το με άλλες εφαρμογές διαχείρισης και τις υπηρεσίες για να παρέχουν ένα υβριδικό περιβάλλον διαχείρισης, για την παροχή προσαρμοσμένων διαχείρισης σενάρια μοναδικά στο περιβάλλον του ή για την παροχή μια προσαρμοσμένης διαχείρισης εμπειρία για τους πελάτες σας.  Σε αυτό το άρθρο παρέχει μια επισκόπηση των σας διαφορετικές επιλογές για την ενοποίηση με OMS και συνδέσεις για άρθρα που παρέχουν λεπτομερείς πληροφορίες."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="bwren" />

# <a name="integrating-with-operations-management-suite-oms"></a>Ενοποίηση με λειτουργίες διαχείρισης οικογένεια (OMS)

Λειτουργίες διαχείρισης οικογένεια είναι της Microsoft βασίζεται στο cloud IT λύση διαχείρισης που σας βοηθά να διαχειριστείτε και προστασία σας εσωτερικής εγκατάστασης και cloud υποδομής.  Εκτός από χρησιμοποιώντας τις τυπικές δυνατότητες του OMS, μπορείτε να ενοποιήσετε το με άλλες εφαρμογές διαχείρισης και τις υπηρεσίες για να παρέχουν ένα υβριδικό περιβάλλον διαχείρισης, για την παροχή προσαρμοσμένων διαχείρισης σενάρια μοναδικά στο περιβάλλον του ή για την παροχή μια προσαρμοσμένης διαχείρισης εμπειρία για τους πελάτες σας.  Σε αυτό το άρθρο παρέχει μια επισκόπηση των σας διαφορετικές επιλογές για την ενοποίηση με τις υπηρεσίες OMS και συνδέσεις για άρθρα που παρέχουν λεπτομερείς πληροφορίες. 



## <a name="log-analytics"></a>Ανάλυση αρχείου καταγραφής
Διαχείριση δεδομένα που συλλέγονται από αρχείο καταγραφής ανάλυσης αποθηκεύονται σε ένα αποθετήριο δεδομένων το οποίο βρίσκεται στο Azure.  Όλα τα δεδομένα που είναι αποθηκευμένα στο αποθετήριο είναι διαθέσιμη στο αρχείο καταγραφής αναζητήσεις που παρέχουν γρήγορη ανάλυση σε πολύ μεγάλους όγκους δεδομένων.  Απαιτήσεις ενοποίησης σας μπορεί να είναι για τη συμπλήρωση του αποθετηρίου με νέα δεδομένα διάθεση για την ανάλυση ή για να εξαγάγετε δεδομένα στο αποθετήριο δεδομένων για την παροχή μια νέα απεικόνιση ή ενσωμάτωση σε κάποιο άλλο εργαλείο διαχείρισης.

Κάθε τμήμα των δεδομένων στο αποθετήριο αποθηκεύονται ως εγγραφή.  Κατά τη συμπλήρωση του αποθετηρίου, θα πρέπει να μπορείτε να παρέχετε στους χρήστες με τον τύπο εγγραφής που χρησιμοποιεί τη λύση σας και μια περιγραφή για τις ιδιότητές του.  Όταν κάνετε ανάκτηση δεδομένων, χρειάζεστε αυτές τις πληροφορίες σχετικά με τα οποία εργάζεστε με δεδομένα.

![Συμπλήρωση του αποθετηρίου OMS](media/operations-management-suite-integration/repository.png)


### <a name="populate-the-log-analytics-repository"></a>Συμπλήρωση του αποθετηρίου ανάλυσης αρχείου καταγραφής
Υπάρχουν πολλές μέθοδοι για τη συμπλήρωση του αποθετηρίου OMS.  Η μέθοδος που χρησιμοποιείτε εξαρτάται από παράγοντες όπως όπου βρίσκεται το αρχείο προέλευσης δεδομένων, η μορφή για τα δεδομένα, και τις οποίες χρειάζεστε για την υποστήριξη πελατών.  Μόλις δεδομένων είναι αποθηκευμένη στο αποθετήριο δεδομένων, δεν έχει σημασία ποιον τρόπο έχουν συλλεχθεί.

Οι παρακάτω ενότητες περιγράφουν τις διαφορετικές επιλογές για τη συμπλήρωση του αποθετηρίου OMS.

#### <a name="connected-sources-and-data-sources"></a>Συνδεδεμένο προελεύσεις και προελεύσεων δεδομένων 
Συνδεδεμένα αρχεία προέλευσης είναι οι θέσεις όπου μπορεί να ανακτηθεί δεδομένων για το χώρο αποθήκευσης OMS.  Προελεύσεις δεδομένων και τις λύσεις εκτελούνται σε συνδεδεμένες προελεύσεις και ορισμός τα συγκεκριμένα δεδομένα που συλλέγονται.  Εάν η εφαρμογή σας εγγράφει δεδομένα σε μία από αυτές τις προελεύσεις δεδομένων, στη συνέχεια, μπορείτε να συλλέξετε αυτό κατά τη ρύθμιση των παραμέτρων της προέλευσης δεδομένων.  Για παράδειγμα, εάν η εφαρμογή σας δημιουργεί Syslog συμβάντα, στη συνέχεια, αυτές μπορούν να συλλέγονται από την προέλευση δεδομένων Syslog σε έναν παράγοντα Linux.

- [Προελεύσεις δεδομένων στο αρχείο καταγραφής ανάλυσης](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Λύσεις

Λύσεις επεκτείνετε τις δυνατότητες του OMS.  Μια λύση μπορεί να συλλέγετε δεδομένα από το συνδεδεμένο αρχείο προέλευσης ή την μπορεί να εκτελέσετε ανάλυση σε καρτέλες ήδη που συλλέγονται στο αποθετήριο.  Όλες τις λύσεις που παρέχεται από τη Microsoft έχει ένα άρθρο μεμονωμένα που παρέχει τις λεπτομέρειες σχετικά με τα δεδομένα που που συλλέγει.

- [Λύσεις στο αρχείο καταγραφής ανάλυσης](../log-analytics/log-analytics-add-solutions.md)



#### <a name="http-data-collector-api"></a>Συλλογή δεδομένων HTTP API

Το API συλλογής καταγραφής ανάλυσης HTTP δεδομένων είναι μια REST API που σας επιτρέπει να προσθέσετε δεδομένων JSON του αποθετηρίου καταγραφής ανάλυσης.  Μπορείτε να αξιοποιήσετε αυτό το API όταν έχετε μια εφαρμογή που δεν παρέχει δεδομένων μέσω μία από τις άλλες προελεύσεις δεδομένων ή λύσεις.  Αυτό μπορεί να χρησιμοποιηθεί για τη συμπλήρωση του αποθετηρίου από οποιοδήποτε πρόγραμμα-πελάτη που μπορούν να καλέσουν το API και δεν βασίζεται στο χρονοδιάγραμμα συλλογής οποιουδήποτε αρχείου προέλευσης δεδομένων ή λύση.

- [Αρχείο καταγραφής ανάλυσης δεδομένων HTTP συλλογής API](../log-analytics/log-analytics-data-collector-api.md)


### <a name="retrieve-data-from-the-log-analytics-repository"></a>Ανάκτηση δεδομένων από το χώρο αποθήκευσης ανάλυσης αρχείου καταγραφής

Υπάρχουν πολλές μέθοδοι για την ανάκτηση δεδομένων από το χώρο αποθήκευσης OMS.  Που μπορεί να θέλετε να ανακτήσετε δεδομένα χρησιμοποιώντας την κονσόλα OMS στους χρήστες και δώστε τους διαφορετικά είδη απεικονίσεις και ανάλυση.  Μπορείτε επίσης να ανακτήσετε τα δεδομένα από μια εξωτερική διαδικασία όπως μια άλλη λύση διαχείρισης.

#### <a name="log-searches"></a>Αρχείο καταγραφής αναζητήσεις

Όλα τα δεδομένα που είναι αποθηκευμένα στο αποθετήριο OMS διατίθεται μέσω καταγραφής αναζητήσεις.  Οι χρήστες ενδέχεται να πραγματοποιήσετε τις δικές τους ad hoc ανάλυση στην κονσόλα OMS ή δημιουργία ενός πίνακα εργαλείων με μια απεικόνιση για ένα συγκεκριμένο αρχείο καταγραφής αναζήτησης.  Λύσεις μπορεί να περιέχουν προσαρμοσμένες προβολές με απεικονίσεις που βασίζονται σε προκαθορισμένο αναζητήσεις.  Μπορείτε να χρησιμοποιήσετε το API αναζήτησης καταγραφής για πρόσβαση σε δεδομένα στο αποθετήριο OMS από ένα εξωτερικό εργαλείο εφαρμογής ή διαχείρισης.  

- [Αρχείο καταγραφής αναζητήσεις στο αρχείο καταγραφής ανάλυσης](../log-analytics/log-analytics-log-searches.md)
- [Αρχείο καταγραφής αναλυτικών στοιχείων καταγραφής search REST API](../log-analytics/log-analytics-log-search-api.md)
- [Cmdlet ανάλυση του αρχείου καταγραφής](https://msdn.microsoft.com/library/mt188224.aspx)



#### <a name="custom-views"></a>Προσαρμοσμένες προβολές 
Η προβολή "Σχεδίαση" σάς επιτρέπει να δημιουργήσετε προσαρμοσμένες προβολές στην κονσόλα OMS που παρέχουν οι χρήστες με απεικόνιση και ανάλυση των δεδομένων στη λύση σας.  Κάθε προβολή περιλαμβάνει ένα πλακίδιο που εμφανίζονται στην κύρια σελίδα στην κονσόλα και οποιονδήποτε αριθμό των τμημάτων απεικόνιση που βασίζονται σε αναζητήσεις καταγραφής που έχετε καθορίσει.
  
- [Αρχείο καταγραφής ανάλυσης προβολή σχεδίασης](../log-analytics/log-analytics-view-designer.md)


#### <a name="power-bi"></a>Power BI

Ανάλυση καταγραφής να αυτόματα εξαγάγετε δεδομένα από το χώρο αποθήκευσης OMS στο Power BI, ώστε να μπορείτε να αξιοποιήσετε τις απεικονίσεις και εργαλεία ανάλυσης.  Εκτελέσει αυτή την εξαγωγή σε ένα χρονοδιάγραμμα, ώστε τα δεδομένα να παραμένουν ενημερωμένα. 

- [Εξαγωγή αρχείου καταγραφής ανάλυσης δεδομένων στο Power BI](../log-analytics/log-analytics-powerbi.md)




## <a name="automation"></a>Αυτοματοποίηση

OMS να αυτοματοποιήσετε διεργασίες απάντησης σε δεδομένα που έχουν συλλεχθεί ή για να εκτελέσετε άλλες λειτουργίες διαχείρισης.  Αυτό μπορεί να συλλογή δεδομένων από την εφαρμογή σας και να το εισαγάγετε σε του αποθετηρίου OMS ή ενδέχεται να μπορείτε να αυτοματοποιήσετε τη διόρθωση των ένα γνωστό θέμα απόκριση σε δεδομένα που υπάρχουν στο αποθετήριο. 

![Αυτοματοποίηση](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks

Runbooks στο Azure Automation εκτέλεση δεσμών ενεργειών του PowerShell και ροές εργασίας στο Azure cloud.  Μπορείτε να τις χρησιμοποιήσετε για να διαχειριστείτε πόρους στο Azure ή όλους τους υπόλοιπους πόρους με δυνατότητα πρόσβασης από το cloud.  Runbooks μπορεί επίσης να εκτελεστεί σε ένα τοπικό Κέντρο δεδομένων χρησιμοποιώντας υβριδική Runbook εργασίας.  Μπορείτε να ξεκινήσετε μια runbook από την πύλη του Azure ή από εξωτερική διεργασιών χρησιμοποιώντας έναν αριθμό από μεθόδους όπως PowerShell ή το API αυτοματισμού.

- [Έναρξη μιας runbook σε αυτοματισμού Azure](../automation/automation-starting-a-runbook.md)
- [Cmdlet του Azure αυτοματισμού](https://msdn.microsoft.com/library/dn690262.aspx)
- [Αυτοματοποίηση REST API](https://msdn.microsoft.com/library/mt662285.aspx)
- [Αυτοματοποίηση .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Ειδοποιήσεις

Ειδοποίηση κανόνες εκτελούνται αυτόματα αναζητήσεις καταγραφής σύμφωνα με ένα χρονοδιάγραμμα.  Εάν τα αποτελέσματα ταιριάζουν με συγκεκριμένο κριτήριο την ειδοποίηση που προκύπτει μπορεί να ξεκινήστε μια runbook αυτοματισμού Azure ή να καλέσετε μια webhook που μπορούν να ξεκινούν μια εξωτερική διαδικασία.  Και τα δύο από αυτές τις απαντήσεις μπορούν να περιλαμβάνουν λεπτομέρειες της ειδοποίησης όπως τα δεδομένα που επιστρέφονται στο αρχείο καταγραφής αναζήτηση.

- [Ειδοποιήσεις στο αρχείο καταγραφής ανάλυσης](../log-analytics/log-analytics-alerts.md)
- [API ειδοποίησης ανάλυσης αρχείου καταγραφής](../log-analytics/log-analytics-api-alerts.md)


## <a name="backup-and-site-recovery"></a>Δημιουργία αντιγράφων ασφαλείας και Επαναφορά τοποθεσίας

Azure δημιουργίας αντιγράφων ασφαλείας και Επαναφορά τοποθεσίας παρέχουν υπηρεσίες για την προστασία των δεδομένων σας για μεγάλες επιχειρήσεις και την εξασφάλιση τη διαθεσιμότητα των διακομιστών και εφαρμογές.  Μπορείτε να αξιοποιήσετε αυτές τις υπηρεσίες για την εκτέλεση σενάρια όπως παρέχουν υπηρεσίες δημιουργίας αντιγράφων ασφαλείας για την εφαρμογή σας ή προετοιμασία ανακατεύθυνσης από μια εικονική μηχανή.

- [Azure cmdlet για δημιουργία αντιγράφων ασφαλείας](https://msdn.microsoft.com/library/mt619253.aspx)
- [Επαναφορά τοποθεσίας Azure REST API](https://msdn.microsoft.com/library/azure/mt750497.aspx)
- [Cmdlet αποκατάστασης Azure τοποθεσιών](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Προσαρμοσμένες λύσεις

Μπορείτε να ενσωματώσετε ενοποίησης λογικής σε μια προσαρμοσμένη λύση για να εκτελέσετε στο χώρο εργασίας σας ή στο χώρο εργασίας του πελάτη.  Λύση σας μπορούν να περιλαμβάνουν οποιαδήποτε από τις μεθόδους ενοποίησης σε αυτό το άρθρο εκτός από άλλους πόρους για την παροχή ενός σεναρίου ολοκλήρωση διαχείρισης.  Οι πόροι στη λύση είναι συσκευαστούν έτσι ώστε όταν έχει καταργηθεί η λύση, όλους τους πόρους που δημιούργησε καταργούνται από το χώρο εργασίας OMS και Azure συνδρομής.

Για παράδειγμα, η λύση ενδέχεται να περιλαμβάνουν μια αυτοματισμού runbook για τη συγκέντρωση και επεξεργαστείτε τα δεδομένα και, στη συνέχεια, τη συμπλήρωση του αποθετηρίου καταγραφής αναλυτικών στοιχείων με χρήση του API συλλογής δεδομένων HTTP.  Μπορείτε επίσης να συμπεριλάβετε μια προσαρμοσμένη προβολή που παρουσιάζει και αναλύει τα δεδομένα που έχουν συλλεχθεί.  

- Δημιουργία προσαρμοσμένων λύσεων (σύντομα διαθέσιμο)    

## <a name="next-steps"></a>Επόμενα βήματα
- Αναφορά στο [OMS SDK](operations-management-suite-sdk.md) για τεχνικές πληροφορίες σχετικά με τις υπηρεσίες OMS αυτοματοποίηση.  