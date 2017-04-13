<properties
    pageTitle="Διαχείριση Bus υπηρεσίας Azure χρησιμοποιώντας Αυτοματισμός Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία αυτοματισμού Azure για τη Διαχείριση Bus υπηρεσίας Azure."
    services="service-bus, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

# <a name="managing-azure-service-bus-using-azure-automation"></a>Διαχείριση Bus υπηρεσίας Azure χρησιμοποιώντας αυτοματισμού Azure

Αυτός ο οδηγός θα εισαγάγει που η υπηρεσία αυτοματισμού Azure και πώς μπορούν να χρησιμοποιηθούν για την απλοποίηση της διαχείρισης του Azure Service Bus.

## <a name="what-is-azure-automation"></a>Τι είναι το Azure αυτοματισμού;

[Αυτοματοποίηση Azure](../automation/automation-intro.md) είναι μια Azure υπηρεσία για την απλοποίηση της διαχείρισης cloud μέσω Αυτοματισμός διαδικασιών και ρύθμισης παραμέτρων επιθυμητή κατάσταση. Χρήση Azure αυτοματοποίησης, μη αυτόματη, επαναληφθεί, μεγάλη διάρκεια εκτέλεσης και σφάλματος ευνοεί εργασίες να είναι αυτοματοποιημένη για να αυξήσετε την αξιοπιστία, της αποδοτικότητας και της ώρας της τιμής για την εταιρεία σας.

Azure αυτοματισμού παρέχει μια μηχανή εκτέλεσης ιδιαίτερα αξιόπιστη, ιδιαίτερα διαθέσιμη ροή εργασίας που κλιμακώνει ώστε να ανταποκρίνεται στις ανάγκες σας. Στη λειτουργία αυτοματισμού Azure, διεργασίες μπορεί να είναι διακοπή με μη αυτόματο τρόπο, με συστήματα άλλου κατασκευαστή ή σε προγραμματισμένα χρονικά διαστήματα, έτσι ώστε οι εργασίες συμβεί ακριβώς όταν είναι απαραίτητο.

Μείωση της επιβάρυνσης λειτουργικές και να ελευθερώσετε IT και το διοικητικό προσωπικό DevOps για εστίαση σε εργασία που προσθέτει επιχειρηματική αξία, μετακινώντας το cloud εργασίες διαχείρισης που θα εκτελεστεί αυτόματα από Αυτοματισμός Azure.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Πώς να Αυτοματισμός Azure Βοήθεια στη Διαχείριση Bus υπηρεσίας Azure;

Μπορείτε να διαχειριστείτε Bus υπηρεσίας με Αυτοματισμός Azure χρησιμοποιώντας την [Υπηρεσία Bus REST API](https://msdn.microsoft.com/library/azure/mt639375.aspx). Μέσα σε Azure Αυτοματισμός, μπορείτε να εκτελέσετε δεσμών ενεργειών του PowerShell για να εκτελούν πολλές από τις εργασίες σας Bus υπηρεσία χρησιμοποιώντας το REST API. Μπορείτε επίσης να σύζευξη αυτές τις κλήσεις REST API στο Azure Automation με τα cmdlet για άλλες υπηρεσίες του Azure, για να αυτοματοποιήσετε σύνθετες εργασίες σε Azure υπηρεσίες και συστήματα τρίτου κατασκευαστή.

Ακολουθούν ορισμένα παραδείγματα της χρήσης του PowerShell για τη Διαχείριση Azure Bus υπηρεσίας:

* [Προσαρμοσμένα cmdlet του PowerShell για να διαχειριστείτε ουρές Bus υπηρεσίας Azure](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [Πώς μπορείτε να δημιουργήσετε ουρές Bus υπηρεσίας, θέματα και συνδρομές χρησιμοποιώντας μια δέσμη ενεργειών PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Δημιουργία Azure Bus χώροι ονομάτων των υπηρεσιών με χρήση του PowerShell](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

Μπορείτε να κάνετε λήψη της λειτουργικής μονάδας PowerShell για την εργασία με bus Azure service στην αυτοματοποίηση runbooks από τη [Συλλογή του PowerShell](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0).


## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του αυτοματισμού Azure και πώς μπορεί να χρησιμοποιηθεί για τη Διαχείριση Azure Bus υπηρεσίας, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα σχετικά με το Azure αυτοματισμού.

* Ανατρέξτε στο άρθρο Azure αυτοματισμού [Γρήγορα αποτελέσματα προγραμμάτων εκμάθησης](../automation/automation-first-runbook-graphical.md)
* Δείτε πώς μπορείτε να [διαχειριστείτε την υπηρεσία Bus με το PowerShell](service-bus-powershell-how-to-provision.md)
