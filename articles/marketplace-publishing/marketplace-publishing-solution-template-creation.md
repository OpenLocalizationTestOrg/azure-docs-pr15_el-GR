<properties
   pageTitle="Οδηγός για τη δημιουργία ενός προτύπου λύση για το Marketplace | Microsoft Azure"
   description="Λεπτομερείς οδηγίες σχετικά με τον τρόπο δημιουργίας, πιστοποίηση και ανάπτυξη προτύπου πολλούς Εικονική εικόνα λύσης για αγορά στο το Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="07/27/2016"
      ms.author="hascipio; v-divte" />

# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Οδηγός για να δημιουργήσετε ένα πρότυπο λύσης για Azure Marketplace
Μετά την ολοκλήρωση του βήματος 1, [η δημιουργία λογαριασμού και την καταγραφή][link-acct-creation], θα σας ζητηθεί στη δημιουργία ενός προτύπου λύση συμβατό με Azure στο [τεχνική προαπαιτούμενα στοιχεία για τη δημιουργία ενός προτύπου λύση](marketplace-publishing-solution-template-creation-prerequisites.md). Τώρα θα σας θα σας καθοδηγήσει τα βήματα για τη δημιουργία ενός προτύπου λύσης για πολλές ΣΠΣ στην [Πύλη δημοσιεύσεων] [ link-pubportal] για το Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Δημιουργήστε την προσφορά πρότυπο λύση στην πύλη του δημοσίευσης
Μεταβείτε στο [https://publish.windowsazure.com](http://publish.windowsazure.com). Κατά την είσοδο για πρώτη φορά στην [Πύλη δημοσιεύσεων](https://publish.windowsazure.com/), χρησιμοποιήστε τον ίδιο λογαριασμό με τον οποίο έχει καταχωρηθεί προφίλ πωλητή της εταιρείας σας. Αργότερα, μπορείτε να προσθέσετε οποιοδήποτε υπαλλήλων της εταιρείας σας ως από κοινού-διαχειριστή στην πύλη του δημοσίευσης.

### <a name="1-select-solution-templates"></a>1. Επιλέξτε "Λύση πρότυπα"

  ![σχέδιο][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Δημιουργία νέου προτύπου λύσης

  ![σχέδιο][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Έναρξη με τοπολογίες
Το πρότυπο λύση είναι ένα γονικό "στοιχείο" σε όλες τις τοπολογίες. Μπορείτε να ορίσετε πολλές τοπολογίες σε ένα πρότυπο προσφορά/λύση. Όταν μια προσφορά προωθείται ενδιάμεσου σταδίου, προωθείται με όλες τις τοπολογίες. Ακολουθήστε τα παρακάτω βήματα για να ορίσετε την προσφορά:     

- Δημιουργήσετε μια τοπολογία: "Αναγνωριστικό τοπολογίας" είναι συνήθως το όνομα του της τοπολογίας για το πρότυπο λύση. Το αναγνωριστικό τοπολογίας χρησιμοποιείται στη διεύθυνση URL, όπως φαίνεται παρακάτω:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure πύλη: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}

- Προσθέστε μια νέα έκδοση.

### <a name="4-get-your-topology-versions-certified"></a>4. γρήγορα τις εκδόσεις τοπολογία πιστοποιηθεί
Στείλτε ένα αρχείο zip που περιέχει όλα τα απαιτούμενα αρχεία για την προμήθεια που συγκεκριμένη έκδοση του της τοπολογίας. Αυτό το αρχείο zip πρέπει να περιλαμβάνουν τα εξής:

- αρχείο *mainTemplate.json* και *createUiDefinition.json* το ριζικό κατάλογο.
- Οποιαδήποτε συνδεδεμένων πρότυπα και όλα τα απαιτούμενα δεσμών ενεργειών.

  > [AZURE.TIP] Κατά την εργασία σας προγραμματιστές για τη δημιουργία της λύσης τοπολογίες πρότυπο και τους πιστοποιηθεί, την επιχείρηση, να εργαστείτε μάρκετινγκ ή/και νομικές τμήματα της εταιρείας σας για το μάρκετινγκ και νομικές περιεχόμενο.

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που δημιουργήσατε το πρότυπό σας λύση και το αρχείο zip που έχουν αποσταλεί, ακολουθήστε τις οδηγίες του [Οδηγού Marketplace μάρκετινγκ περιεχομένου](marketplace-publishing-push-to-staging.md) πριν από την προώθηση την προσφορά για να ενδιάμεσου σταδίου. Για να δείτε το πλήρες σύνολο marketplace δημοσίευση άρθρων, επισκεφθείτε [Γρήγορα αποτελέσματα: πώς μπορείτε να δημοσιεύσετε μια προσφορά για να το Azure Marketplace](marketplace-publishing-getting-started.md).

Μπορεί επίσης να σας ενδιαφέρουν αυτά τα σχετικά άρθρα:

- Εικονική εικόνες: [Σχετικά με τις εικόνες εικονική μηχανή στο Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
- Εικονική επεκτάσεις: [Εικονική παράγοντας και Επισκόπηση επεκτάσεις Εικονική](https://msdn.microsoft.com/library/azure/dn832621.aspx) και [επεκτάσεις Εικονική Azure και δυνατότητες](https://msdn.microsoft.com/library/azure/dn606311.aspx)
- Azure διαχείριση πόρων: [σύνταξης Azure ARM πρότυπα](../resource-group-authoring-templates.md) και [παραδείγματα πρότυπο απλό ARM](https://github.com/rjmax/ArmExamples)
- Throttles λογαριασμού χώρου αποθήκευσης: [Πώς να οθόνη για περιορισμού λογαριασμού χώρου αποθήκευσης](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) και [Premium χώρου αποθήκευσης](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
