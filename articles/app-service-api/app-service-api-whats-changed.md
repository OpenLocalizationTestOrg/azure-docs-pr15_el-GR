<properties
    pageTitle="Εφαρμογή υπηρεσίας API εφαρμογές - τι έχει αλλάξει | Microsoft Azure"
    description="Μάθετε τι νέο υπάρχει για εφαρμογές API στο Azure εφαρμογής υπηρεσίας."
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>Εφαρμογή υπηρεσίας API εφαρμογές - τι έχει αλλάξει

Στο το συμβάν Connect() στο Νοέμβριο 2015, έναν αριθμό με τις βελτιώσεις στο Azure εφαρμογής υπηρεσίας έχουν [ανακοινώσει](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Αυτές οι βελτιώσεις περιλαμβάνουν υποκείμενα αλλαγές στις εφαρμογές API για καλύτερη στοίχιση με το κινητό και εφαρμογές Web, μείωση έννοια count και τη βελτίωση ανάπτυξης και επιδόσεων χρόνου εκτέλεσης. Έναρξη νέας API 30 Νοεμβρίου 2015, οι εφαρμογές που δημιουργείτε χρησιμοποιώντας την πύλη διαχείρισης Azure ή την πιο πρόσφατη tooling θα αντανακλούν τις αλλαγές αυτές τις. Σε αυτό το άρθρο περιγράφει αυτές τις αλλαγές, καθώς και πώς μπορείτε να αναπτύξετε εκ νέου υπάρχουσες εφαρμογές για να επωφεληθείτε από τις δυνατότητες.

## <a name="feature-changes"></a>Δυνατότητα αλλαγών
Τις βασικές δυνατότητες του API εφαρμογές-έλεγχος ταυτότητας, CORS και API μετα-δεδομένα – έχουν μετακινηθεί άμεσα σε μια εφαρμογή υπηρεσίας. Με αυτήν την αλλαγή, οι δυνατότητες είναι διαθέσιμες σε Web, Mobile και εφαρμογές API. Στην πραγματικότητα, οι τρεις κοινή χρήση του ίδιου τύπου πόρου **Microsoft.Web/sites** στη Διαχείριση πόρων. Η πύλη API εφαρμογές είναι δεν είναι πλέον χρειάζεται ή παρέχεται με το API εφαρμογές. Αυτό διευκολύνει επίσης να χρησιμοποιήσετε Azure API διαχείρισης, επειδή θα είναι μόνο η μία πύλη API διαχείρισης.

![Επισκόπηση εφαρμογές API](./media/app-service-api-whats-changed/api-apps-overview.png)

Μια αρχή κλειδιού σχεδίασης με την ενημέρωση API εφαρμογές είναι για να μπορείτε να μεταφέρετε το API όπως, την γλώσσα της επιλογής.  Εάν έχετε ήδη αναπτύξει το API σας ως μια εφαρμογή Web ή εφαρμογή Mobile, δεν χρειάζεται να αναπτύξετε ξανά την εφαρμογή σας για να επωφεληθείτε από τις νέες δυνατότητες. Εάν είστε αυτήν τη στιγμή σε εφαρμογές API preview, καθοδήγηση μετεγκατάστασης είναι λεπτομερώς πιο κάτω.

### <a name="authentication"></a>Έλεγχος ταυτότητας
Το υπάρχον προπληρωμένη ολοκληρωμένη API εφαρμογές, τις υπηρεσίες/εφαρμογές του Mobile και εφαρμογές Web ελέγχου ταυτότητας δυνατότητες έχουν γίνει ενοποιημένο σύστημα και είναι διαθέσιμες σε ένα μεμονωμένο blade ελέγχου ταυτότητας Azure εφαρμογής υπηρεσίας στην πύλη διαχείρισης. Για μια εισαγωγή σχετικά με τις υπηρεσίες ελέγχου ταυτότητας στην εφαρμογή υπηρεσίας, ανατρέξτε στο θέμα [επέκταση εφαρμογής υπηρεσίας ελέγχου ταυτότητας / εξουσιοδότησης](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

Για σενάρια API, υπάρχουν κάποια σχετικές νέες δυνατότητες:

- **Υποστήριξη για τη χρήση Azure Active Directory απευθείας**, χωρίς να χρειάζεται να το διακριτικό AAD για ένα διακριτικό περιόδου λειτουργίας του exchange κωδικός προγράμματος-πελάτη: το πρόγραμμα-πελάτη μπορούν να περιλαμβάνουν μόνο τα διακριτικά AAD στην κεφαλίδα της εξουσιοδότησης, σύμφωνα με την προδιαγραφή διακριτικό φορέα. Αυτό επίσης σημαίνει ότι δεν υπάρχει SDK συγκεκριμένης εφαρμογής υπηρεσίας απαιτείται στην πλευρά του προγράμματος-πελάτη ή διακομιστή. 
- **Υπηρεσία εξυπηρέτησης ή access "Εσωτερικού"**: Εάν έχετε μια διαδικασία μονού ή ορισμένες άλλες προγράμματος-πελάτη που χρειάζονται πρόσβαση προς API χωρίς ένα περιβάλλον εργασίας, μπορείτε να ζητήσετε ενός διακριτικού χρησιμοποιώντας ένα κεφάλαιο υπηρεσίας AAD και μεταβιβάζουν για την εφαρμογή υπηρεσίας για τον έλεγχο ταυτότητας με την εφαρμογή σας.
- **Εξουσιοδότηση αναβλήθηκε**: πολλές εφαρμογές έχουν διαφορετικές περιορισμούς πρόσβασης για διαφορετικά τμήματα της εφαρμογής. Ίσως θέλετε ορισμένες API για να είναι διαθέσιμη στο κοινό, ενώ άλλοι χρειάζονται εισόδου. Η αρχική δυνατότητα ελέγχου ταυτότητας/εξουσιοδότησης ήταν ολόκληρη ή καθόλου, με ολόκληρη την τοποθεσία που απαιτεί σύνδεση. Αυτή η επιλογή εξακολουθεί να υπάρχει, αλλά Εναλλακτικά, μπορείτε να επιτρέψετε κώδικα της εφαρμογής σας για την απόδοση αποφάσεις access μετά την εφαρμογή υπηρεσίας έχει έλεγχος ταυτότητας χρήστη.
 
Για περισσότερες πληροφορίες σχετικά με τις νέες δυνατότητες ελέγχου ταυτότητας, ανατρέξτε στο θέμα [Έλεγχος ταυτότητας και εξουσιοδότησης για τις εφαρμογές API στο Azure εφαρμογής υπηρεσίας](app-service-api-authentication.md). Για πληροφορίες σχετικά με τον τρόπο για τη μετεγκατάσταση υπάρχοντος API εφαρμογές από το προηγούμενο μοντέλο εφαρμογών API για το νέο σχόλιο, ανατρέξτε στο θέμα [μετεγκατάσταση υπάρχοντος API εφαρμογές](#migrating-existing-api-apps) αργότερα σε αυτό το άρθρο.
 
### <a name="cors"></a>CORS
Αντί για μια οριοθετημένη **MS_CrossDomainOrigins** εφαρμογή ρύθμιση, υπάρχει πλέον μια blade στην πύλη του Azure διαχείρισης για τη ρύθμιση παραμέτρων CORS. Εναλλακτικά, μπορούν να ρυθμιστούν με χρήση tooling όπως Azure PowerShell, CLI ή [Explorer πόρου](https://resources.azure.com/)από διαχειριστή πόρων. Ορίστε την ιδιότητα **cors** τον τύπο πόρου **Microsoft.Web/sites/config** για το ** &lt;όνομα τοποθεσίας&gt;/web** πόρων. Για παράδειγμα:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API μετα-δεδομένων
Το API blade ορισμού είναι τώρα διαθέσιμο σε Web, Mobile και εφαρμογές API. Στην πύλη διαχείρισης, μπορείτε να καθορίσετε μια σχετική διεύθυνση url ή μια απόλυτη διεύθυνση url που δείχνει σε ένα τελικό σημείο που αναπαριστάται hosts μια 2.0 Swagger το API. Εναλλακτικά, μπορούν να ρυθμιστούν με χρήση tooling διαχείρισης πόρων. Ορίστε την ιδιότητα **apiDefinition** τον τύπο πόρου **Microsoft.Web/sites/config** για το ** &lt;όνομα τοποθεσίας&gt;/web** πόρων. Για παράδειγμα:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

Προς το παρόν, το τελικό σημείο μετα-δεδομένων πρέπει να είναι δυνατή η πρόσβαση στο κοινό χωρίς έλεγχο ταυτότητας για πολλές συσκευές ροής (π.χ. Visual Studio REST API γενιάς προγράμματος-πελάτη και PowerApps ροής "Προσθήκη API") για την εκμετάλλευση του. Αυτό σημαίνει ότι εάν χρησιμοποιείτε τον έλεγχο ταυτότητας εφαρμογής υπηρεσίας και θέλετε να εμφανίσετε τον ορισμό API από μέσα σε ίδια την εφαρμογή, θα χρειαστεί να χρησιμοποιήσετε την επιλογή αναβάλει ελέγχου ταυτότητας που περιγράφεται παραπάνω, έτσι ώστε η δρομολόγηση σε μετα-δεδομένων του Swagger είναι δημόσια.

## <a name="management-portal"></a>Πύλη διαχείρισης
Επιλογή **Δημιουργία > Web + Mobile > εφαρμογή API** στην πύλη θα δημιουργήσετε εφαρμογές API που αντικατοπτρίζουν τις νέες δυνατότητες που περιγράφονται στο άρθρο. **Αναζήτηση > εφαρμογές API** θα Εμφάνιση μόνο αυτές τις νέες εφαρμογές API. Όταν κάνετε αναζήτηση σε μια εφαρμογή API, το blade μοιράζεται τις ίδιες δυνατότητες με εκείνα του Web και εφαρμογές του Mobile και διάταξη. Οι διαφορές μόνο είναι γρήγορη έναρξη περιεχόμενο και την ταξινόμηση των ρυθμίσεων.

Υπάρχουσες εφαρμογές API (ή εφαρμογές Marketplace API που δημιουργήθηκαν από τις εφαρμογές λογικής) με το προηγούμενο Preview δυνατότητες θα εξακολουθεί να είναι ορατό στη σχεδίαση λογικής εφαρμογών και κατά την αναζήτηση σε όλους τους πόρους σε μια ομάδα πόρων.

## <a name="visual-studio"></a>Visual Studio

Οι περισσότερες εφαρμογές Web της tooling θα λειτουργεί με νέες εφαρμογές API εφόσον που κάνουν κοινή χρήση του ίδιου τύπου πόρου υποκείμενα **Microsoft.Web/sites** . Το Visual Studio Azure tooling, ωστόσο, πρέπει να είναι αναβαθμισμένη έκδοση 2.8.1 ή νεότερη έκδοση εφόσον εκθέτει έναν αριθμό προς API συγκεκριμένες δυνατότητες. Κάντε λήψη του SDK από τη [σελίδα Azure στοιχεία λήψης](https://azure.microsoft.com/downloads/).

Με την εξυγίανση από τους τύπους εφαρμογής υπηρεσίας, δημοσίευση επίσης ενοποιημένο σύστημα στην περιοχή **Δημοσίευση > Microsoft Azure εφαρμογής υπηρεσίας**:

![Δημοσίευση εφαρμογών API](./media/app-service-api-whats-changed/api-apps-publish.png)

Για να μάθετε περισσότερα σχετικά με το SDK 2.8.1, διαβάστε τη [Δημοσίευση ιστολογίου](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/)ανακοίνωση.

Εναλλακτικά, μπορείτε να εισαγάγετε με μη αυτόματο τρόπο το προφίλ δημοσίευση από την πύλη διαχείρισης, για να ενεργοποιήσετε τη δημοσίευση. Ωστόσο, SDK 2.8.1 θα χρειαστεί Cloud Explorer, δημιουργία κώδικα και εφαρμογή API επιλογής/δημιουργίας ή νεότερη έκδοση.

## <a name="migrating-existing-api-apps"></a>Μετεγκατάσταση υπάρχοντος API εφαρμογές
Εάν το προσαρμοσμένο API έχει αναπτυχθεί στην προηγούμενη έκδοση προεπισκόπησης API εφαρμογών, ζητάμε ότι κάνετε μετεγκατάσταση σε το νέο μοντέλο για τις εφαρμογές API 31 Δεκεμβρίου 2015. Δεδομένου ότι τόσο το παλιό και το νέο μοντέλο βασίζονται σε APIs Web που φιλοξενείται στο εφαρμογής υπηρεσίας, το μεγαλύτερο μέρος των υπάρχοντα κωδικό μπορεί να χρησιμοποιηθεί ξανά.

### <a name="hosting-and-redeployment"></a>Φιλοξενία και αναδιάταξη
Τα βήματα για επανάληψη ανάπτυξης είναι ίδια με την ανάπτυξη οποιαδήποτε υπάρχοντα API Web στον εφαρμογής υπηρεσίας. Τα βήματα:

1. Δημιουργήστε μια κενή εφαρμογή API. Αυτό μπορεί να γίνει στην πύλη με δημιουργία > εφαρμογή API, στο Visual Studio από δημοσίευση ή από διαχειριστή πόρων tooling. Εάν χρησιμοποιείτε διαχείριση πόρων tooling ή πρότυπα, ορίστε την τιμή **είδος** για **api** τον τύπο πόρου **Microsoft.Web/sites** να έχετε τις γρήγορες εκκινήσεις και τις ρυθμίσεις στην πύλη διαχείρισης προσανατολισμένα προς API σενάρια.
2. Σύνδεση και αναπτύξτε το έργο σας κενή εφαρμογή API με οποιονδήποτε από τους μηχανισμούς ανάπτυξης που υποστηρίζονται από το εφαρμογής υπηρεσίας. Διαβάστε [Azure εφαρμογής υπηρεσίας ανάπτυξης τεκμηρίωση](../app-service-web/web-sites-deploy.md) για να μάθετε περισσότερα. 
  
### <a name="authentication"></a>Έλεγχος ταυτότητας
Οι υπηρεσίες ελέγχου ταυτότητας εφαρμογής υπηρεσίας υποστηρίζει τις ίδιες δυνατότητες που ήταν διαθέσιμες με το προηγούμενο μοντέλο API εφαρμογές. Εάν χρησιμοποιείτε την περίοδο λειτουργίας διακριτικά και απαιτούν SDK, χρησιμοποιήστε την παρακάτω SDK προγράμματος-πελάτη και διακομιστή:

- Πρόγραμμα-πελάτης: [Azure φορητός υπολογιστής-πελάτης SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- Διακομιστής: [Microsoft Azure εφαρμογής για κινητές συσκευές .NET ελέγχου ταυτότητας επέκτασης](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Εάν χρησιμοποιούσατε αντί για αυτό το άλφα εφαρμογής υπηρεσίας SDK, αυτές απορρίπτονται τώρα:

- Πρόγραμμα-πελάτης: [Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- Διακομιστής: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Ιδίως με Azure Active Directory, ωστόσο, δεν υπάρχει συγκεκριμένης εφαρμογής υπηρεσίας απαιτείται εάν χρησιμοποιείτε το διακριτικό AAD απευθείας.

### <a name="internal-access"></a>Εσωτερική πρόσβαση
Το προηγούμενο μοντέλου API εφαρμογές περιλαμβάνονται ένα επίπεδο ενσωματωμένη εσωτερική πρόσβαση. Αυτό απαιτείται χρήση του SDK για αιτήσεις υπογραφής. Όπως περιγράφεται παραπάνω, με το νέο μοντέλο εφαρμογών API, αρχές υπηρεσίας AAD μπορεί να χρησιμοποιηθεί ως μια εναλλακτική για τον έλεγχο ταυτότητας υπηρεσίας εξυπηρέτησης χωρίς να απαιτείται μια εφαρμογή υπηρεσίας συγκεκριμένο SDK. Μάθετε περισσότερα στο [υπηρεσίας κεφαλαίου ελέγχου ταυτότητας για τις εφαρμογές API στο Azure εφαρμογής υπηρεσίας](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Εντοπισμός
Το προηγούμενο μοντέλο εφαρμογών API είχε APIs για εντοπισμός άλλων εφαρμογών API κατά το χρόνο εκτέλεσης της ίδιας ομάδας πόρων πίσω από την ίδια πύλη. Αυτό είναι ιδιαίτερα χρήσιμο σε αρχιτεκτονικές που υλοποίηση μοτίβα microservice. Ενώ αυτή δεν υποστηρίζεται απευθείας, ένας αριθμός επιλογών είναι διαθέσιμες:

1. Χρησιμοποιήστε το Azure API διαχείρισης πόρων για τον εντοπισμό.
2. Τοποθέτηση Azure API διαχείρισης μπροστά από το APIs φιλοξενούνται εφαρμογής υπηρεσίας. Azure API διαχείρισης λειτουργεί ως μια πρόσοψη και μπορούν να παρέχουν ένα σταθερό εξωτερική διεύθυνση url αντικριστές ακόμα και αν αλλάξει εσωτερικού τοπολογίας.
3. Δημιουργήστε τη δική σας εφαρμογή API εντοπισμού και άλλες εφαρμογές API καταχώρηση με την εφαρμογή εντοπισμού κατά την εκκίνηση.
4. Κατά το χρόνο ανάπτυξης, συμπληρώστε τις ρυθμίσεις εφαρμογής όλων των εφαρμογών API (και προγράμματα-πελάτες) με τα τελικά σημεία από τις άλλες εφαρμογές API. Αυτό είναι βιώσιμος στο πρότυπο αναπτύξεις και επειδή το API εφαρμογές σας δώσει τώρα ελέγχου της διεύθυνσης url.

## <a name="using-api-apps-with-logic-apps"></a>Χρήση των εφαρμογών του API με λογική εφαρμογών

Το νέο μοντέλο εφαρμογών API λειτουργεί καλά με [Εφαρμογές λογικής έκδοση σχήματος 2015-08-01](../app-service-logic/app-service-logic-schema-2015-08-01.md).

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, διαβάστε τα άρθρα στην [ενότητα τεκμηρίωση εφαρμογές API](https://azure.microsoft.com/documentation/services/app-service/api/). Έχουν ενημερωθεί ώστε να αντικατοπτρίζει το νέο μοντέλο για τις εφαρμογές του API. Επιπλέον, επικοινωνήστε με στα φόρουμ για περισσότερες λεπτομέρειες ή οδηγίες για τη μετεγκατάσταση:

- [Φόρουμ στο MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [Υπερχείλιση στοίβας](http://stackoverflow.com/questions/tagged/azure-api-apps)