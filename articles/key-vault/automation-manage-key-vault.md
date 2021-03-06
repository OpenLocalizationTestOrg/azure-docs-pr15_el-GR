<properties
    pageTitle="Διαχείριση θάλαμο κλειδί Azure χρησιμοποιώντας Αυτοματισμός Azure | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με τον τρόπο την υπηρεσία αυτοματισμού Azure μπορεί να χρησιμοποιηθεί για διαχείριση θάλαμο κλειδί Azure."
    services="Key-Vault, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

#<a name="managing-azure-key-vault-using-azure-automation"></a>Διαχείριση θάλαμο κλειδί Azure χρησιμοποιώντας αυτοματισμού Azure

Αυτός ο οδηγός θα Παρουσιάστε με την υπηρεσία αυτοματισμού Azure και πώς μπορούν να χρησιμοποιηθούν για την απλοποίηση της διαχείρισης των αριθμών-κλειδιών και απόρρητο στο Azure κλειδί θάλαμο.

## <a name="what-is-azure-automation"></a>Τι είναι το Azure αυτοματισμού;

[Αυτοματοποίηση Azure](../automation/automation-intro.md) είναι μια Azure υπηρεσία για την απλοποίηση της διαχείρισης cloud μέσω Αυτοματισμός διαδικασιών και ρύθμισης παραμέτρων επιθυμητή κατάσταση. Χρήση Azure αυτοματοποίησης, μη αυτόματη, επαναληφθεί, μεγάλη διάρκεια εκτέλεσης και σφάλματος ευνοεί εργασίες να είναι αυτοματοποιημένη για να αυξήσετε την αξιοπιστία, της αποδοτικότητας και της ώρας της τιμής για την εταιρεία σας.

Azure Αυτοματισμός παρέχει μια μηχανή εκτέλεσης ιδιαίτερα αξιόπιστη, ιδιαίτερα διαθέσιμη ροή εργασίας που κλιμακώνει ώστε να ανταποκρίνεται στις ανάγκες σας. Στη λειτουργία αυτοματισμού Azure, διεργασίες μπορεί να είναι διακοπή με μη αυτόματο τρόπο, με συστήματα άλλου κατασκευαστή ή σε προγραμματισμένα χρονικά διαστήματα, έτσι ώστε οι εργασίες συμβεί ακριβώς όταν είναι απαραίτητο.

Μείωση της επιβάρυνσης λειτουργικές και να ελευθερώσετε IT και το διοικητικό προσωπικό DevOps για εστίαση σε εργασία που προσθέτει επιχειρηματική αξία, μετακινώντας το cloud εργασίες διαχείρισης που θα εκτελεστεί αυτόματα από Azure αυτοματισμού.


## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Πώς να αυτοματισμού Azure Βοήθεια στη Διαχείριση θάλαμο κλειδί Azure;

Πλήκτρο θάλαμο μπορεί να γίνει διαχείριση στο Azure Automation χρησιμοποιώντας τα [AzureRM κλειδί θάλαμο cmdlet] (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) και τα [cmdlet Azure κλασική θάλαμο αριθμού-κλειδιού](https://msdn.microsoft.com/library/azure/dn868052.aspx). Η λειτουργική μονάδα Azure για τη Διαχείριση κλασική θάλαμο αριθμού-κλειδιού είναι διαθέσιμο αυτόματα στο Azure Αυτοματισμός, και μπορείτε να εισαγάγετε τη [λειτουργική μονάδα AzureRM KeyVault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) στο Azure Αυτοματισμός, έτσι ώστε να μπορείτε να εκτελέσετε πολλές από τις εργασίες διαχείρισης θάλαμο κλειδί μέσα στην υπηρεσία. Μπορείτε επίσης να σύζευξη αυτών των cmdlet στο Azure Automation με τα cmdlet για άλλες υπηρεσίες του Azure, για να αυτοματοποιήσετε σύνθετες εργασίες σε Azure υπηρεσίες και συστήματα τρίτου κατασκευαστή.

Με τα cmdlet θάλαμο κλειδί Azure μπορείτε να εκτελέσετε αυτές τις εργασίες μεταξύ άλλων: 

- Δημιουργία και ρύθμιση παραμέτρων ενός κλειδιού θάλαμο
- Δημιουργήστε ή εισαγάγετε έναν αριθμό-κλειδί
- Δημιουργία ή ενημέρωση μιας μυστικό
- Ενημέρωση χαρακτηριστικά ενός κλειδιού
- Λάβετε έναν αριθμό-κλειδί ή μυστικό
- Διαγραφή ενός κλειδιού ή μυστικό

Ακολουθούν ορισμένα παραδείγματα της χρήσης του PowerShell για τη Διαχείριση θάλαμο αριθμού-κλειδιού:  

* [Azure κλειδιού θάλαμο - βήμα προς βήμα](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Ρύθμιση και τη ρύθμιση των παραμέτρων ενός κλειδιού θάλαμο Azure](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)


## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του Azure αυτοματισμού και πώς μπορεί να χρησιμοποιηθεί για τη Διαχείριση θάλαμο κλειδί Azure, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα σχετικά με το Azure αυτοματισμού.

* Ανατρέξτε στο θέμα το Azure Αυτοματισμός [Γρήγορα αποτελέσματα προγραμμάτων εκμάθησης](../automation/automation-first-runbook-graphical.md).
* Δείτε τις [δέσμες ενεργειών PowerShell θάλαμο Azure αριθμού-κλειδιού](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).
