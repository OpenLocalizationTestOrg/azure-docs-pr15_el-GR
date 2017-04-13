<properties 
    pageTitle="Οδηγός για τους προγραμματιστές Azure δημόσιους οργανισμούς" 
    description="Αυτό παρέχει ένα όσον αφορά τα των δυνατοτήτων και καθοδήγηση στην ανάπτυξη εφαρμογών για δημόσιους οργανισμούς Azure" 
    services="" 
    cloud="gov"
    documentationCenter="" 
    authors="Joharve2" 
    manager="Chrisnie" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="10/29/2015" 
    ms.author="jharve"/>


#  <a name="microsoft-azure-government-developer-guide"></a>Οδηγός για προγραμματιστές του Microsoft Azure για δημόσιους οργανισμούς 

<p> Microsoft Azure δημόσιο είναι μια φυσική και απομόνωσης παρουσία δικτύου του Windows Azure.  Αυτός ο οδηγός προγραμματιστές παρέχει λεπτομέρειες σχετικά με τις διαφορές που τους προγραμματιστές εφαρμογών και οι διαχειριστές θα πρέπει να αλληλεπιδρούν και να εργαστείτε με αυτά τα ξεχωριστές περιοχές του Azure.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>Σε αυτό το θέμα


+ [Επισκόπηση](#Overview)
+ [Οδηγίες για προγραμματιστές](#Guidance)
+ [Δυνατότητες που είναι ακόμη διαθέσιμες στο Microsoft Azure για δημόσιους οργανισμούς](#Features)
+ [Αντιστοίχιση τελικού σημείου](#Endpoint)
+ [Επόμενα βήματα](#next)


## <a name="Overview"></a>Επισκόπηση

Microsoft Azure για δημόσιους οργανισμούς είναι μια ξεχωριστή παρουσία της υπηρεσίας Microsoft Azure καλύπτει τις ανάγκες ασφάλεια και συμμόρφωση federal αρχές των ΗΠΑ, κατάσταση και τοπικού κυβερνήσεις και τις υπηρεσίες παροχής λύσεων. Azure δημόσιους οργανισμούς προσφέρει φυσικής και δικτύου απομόνωσης από αναπτύξεις κυβέρνηση εκτός των η.π.α. και παρέχει φιλτραρισμένο προσωπικό η.π.α.. 

Η Microsoft παρέχει ένα πλήθος εργαλείων για να δημιουργήσετε και να αναπτύξετε εφαρμογές cloud καθολικός υπηρεσίες Microsoft Azure ("Καθολικός υπηρεσίας") και τις υπηρεσίες του Microsoft Azure για δημόσιους οργανισμούς της Microsoft.

Κατά τη δημιουργία και την ανάπτυξη εφαρμογών για τους προγραμματιστές των υπηρεσιών για δημόσιους οργανισμούς Azure, σε αντίθεση με την καθολική υπηρεσία, πρέπει να γνωρίζετε τις βασικές διαφορές από τις δύο υπηρεσίες.  Ειδικά γύρω από τη ρύθμιση και τη ρύθμιση των παραμέτρων τους περιβάλλον προγραμματισμού, τη ρύθμιση των παραμέτρων τελικά σημεία, σύνταξη εφαρμογών και την ανάπτυξη τους ως υπηρεσίες για δημόσιους οργανισμούς Azure.

Οι πληροφορίες σε αυτό το έγγραφο συνοψίζει αυτές τις διαφορές και να συμπληρώνει τις πληροφορίες που είναι διαθέσιμες στην τοποθεσία(http://www.azure.com/gov "Για δημόσιους οργανισμούς Azure") [Azure για δημόσιους οργανισμούς]και το [Microsoft Azure Τεχνική βιβλιοθήκη](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") στο MSDN. Επίσημη πληροφορίες μπορεί να είναι επίσης διαθέσιμες σε πολλές άλλες θέσεις όπως το [Κέντρο αξιοπιστίας της Microsoft Azure] (https://azure.microsoft.com/support/trust-center/ "Κέντρο αξιοπιστίας της Microsoft Azure" /), [Κέντρο τεκμηρίωση Azure](https://azure.microsoft.com/documentation/) και στο [Azure ιστολόγια] (https://azure.microsoft.com/blog/ "Azure ιστολόγια" /). 

Αυτό το περιεχόμενο προορίζεται για συνεργάτες και οι προγραμματιστές που για την ανάπτυξη στον Microsoft Azure για δημόσιους οργανισμούς.



## <a name="Guidance"></a>Οδηγίες για προγραμματιστές
Επειδή το μεγαλύτερο μέρος του τεχνική περιεχομένου που είναι διαθέσιμη προς το παρόν προϋποθέτει ότι οι εφαρμογές αναπτύσσονται για την καθολική υπηρεσία και όχι για το Microsoft Azure για δημόσιους οργανισμούς, είναι σημαντικό για εσάς για να βεβαιωθείτε ότι γνωρίζετε βασικές διαφορές για τις εφαρμογές που αναπτύχθηκε να φιλοξενηθούν σε δημόσιους οργανισμούς Azure προγραμματιστές.

- Πρώτα, υπάρχουν υπηρεσίες και τις διαφορές δυνατοτήτων, αυτό σημαίνει ότι ορισμένες δυνατότητες που είναι σε συγκεκριμένες περιοχές της υπηρεσίας καθολικής ενδέχεται να μην είναι διαθέσιμη στο Azure για δημόσιους οργανισμούς.

- Δεύτερον, για τις δυνατότητες που παρέχονται σε δημόσιους οργανισμούς Azure, υπάρχουν διαφορές ρύθμισης παραμέτρων από την καθολική υπηρεσία.  Επομένως, πρέπει να εξετάσετε το δείγμα κώδικα, ρυθμίσεις παραμέτρων και βήματα για να βεβαιωθείτε ότι η δημιουργία και εκτέλεση του περιβάλλοντος Azure τις υπηρεσίες Cloud για δημόσιους οργανισμούς.


## <a name="Features"></a>Δυνατότητες που είναι ακόμη διαθέσιμες στο Microsoft Azure για δημόσιους οργανισμούς
Azure δημόσιους οργανισμούς έχει αυτήν τη στιγμή τις ακόλουθες υπηρεσίες που είναι διαθέσιμες σε ΜΑΣ GOV IOWA και GOV ΜΑΣ ΒΙΡΤΖΊΝΙΑ περιοχές:

- Εικονικές μηχανές
- Υπηρεσίες cloud
- Χώρος αποθήκευσης
- Υπηρεσία καταλόγου Active Directory
- Χρονοδιάγραμμα
- Εικονικό δίκτυο
- Βάση δεδομένων SQL
- Αρχεία του Azure
- Υπηρεσίες πολυμέσων
- Διαχείριση κίνηση
- Υπηρεσία Bus
- StorSimple
- Redis Cache
- Δημιουργία αντιγράφων ασφαλείας Azure
- Αυτοματοποίηση
- ExpressRoute
- κ.λπ.

Άλλες υπηρεσίες είναι διαθέσιμες και περισσότερες υπηρεσίες θα προστεθεί σε συνεχή βάση.  Για την πιο πρόσφατη λίστα των υπηρεσιών, ανατρέξτε στο θέμα [περιοχές σελίδας](https://azure.microsoft.com/regions/#services) η οποία θα επισημάνετε κάθε διαθέσιμο περιοχής και τις υπηρεσίες.  

Προς το παρόν, Iowa GOV ΜΑΣ και Βιρτζίνια GOV ΜΑΣ είναι τα κέντρα δεδομένων υποστήριξης, για δημόσιους οργανισμούς Azure.  Ανατρέξτε στη σελίδα περιοχές παραπάνω για την τρέχουσα κέντρα δεδομένων και υπηρεσίες που είναι διαθέσιμες.

## <a name="Endpoint"></a>Αντιστοίχιση τελικού σημείου

Χρησιμοποιήστε τον παρακάτω πίνακα για να σας καθοδηγήσει κατά την αντιστοίχιση δημόσια τελικά σημεία Microsoft Azure και βάση δεδομένων SQL για δημόσιους οργανισμούς Azure συγκεκριμένες τελικά σημεία.


Τύπος υπηρεσίας|Δημόσια Azure|Azure δημόσιους οργανισμούς
---|---|---
Πύλη διαχείρισης|Manage.windowsazure.com|Manage.windowsazure.US
Γενικά|*. των windows.net|*. usgovcloudapi.net
Πυρήνα|*. core.windows.net|*. core.usgovcloudapi.net
Υπολογισμός|*. cloudapp.net|*. usgovcloudapp.net
Χώρος αποθήκευσης αντικειμένων blob|*. blob.core.windows.net|   *. blob.core.usgovcloudapi.net
Ουρά χώρου αποθήκευσης|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Χώρος αποθήκευσης πινάκων|*. table.core.windows.net|*. table.core.usgovcloudapi.net
Διαχείριση της υπηρεσίας|Management.Core.Windows.NET|Management.Core.usgovcloudapi.NET
Βάση δεδομένων SQL|*. database.windows.net|*. database.usgovcloudapi.net
Τελικό σημείο εξισορρόπηση φόρτου ARM|https://Management.Windows.NET|https://Management.usgovcloudapi.NET  

* Για έλεγχο ταυτότητας ARM μέσω Azure AD, αναφορά [Τον έλεγχο ταυτότητας των αιτήσεων για τη διαχείριση πόρων Azure](https://msdn.microsoft.com/library/azure/dn790557.aspx)

## <a name="next"></a>Επόμενα βήματα

Εάν σας ενδιαφέρει να μάθετε περισσότερα και για δημόσιους οργανισμούς Azure επικοινωνήστε αξιοποιήσετε ορισμένες από τις παρακάτω συνδέσεις.

- **[Εγγραφείτε για μια δοκιμαστική έκδοση](https://azuregov.microsoft.com/trial/azuregovtrial)**

- **[Κατά τη λήψη και την πρόσβαση σε δημόσιους οργανισμούς Azure](http://azure.com/gov)**

- **[Επισκόπηση Azure δημόσιους οργανισμούς](/azure-government-overview)**

- **[Ιστολόγιο του Azure για δημόσιους οργανισμούς](http://blogs.msdn.com/b/azuregov/)**

- **[Azure συμμόρφωσης](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md
