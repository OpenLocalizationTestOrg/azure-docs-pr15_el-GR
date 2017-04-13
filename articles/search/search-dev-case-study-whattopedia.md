<properties 
    pageTitle="Azure αναζήτησης για προγραμματιστές περιπτωσιολογική μελέτη: Τρόπος WhatToPedia δημιουργείται μια πύλη infomedia στο Microsoft Azure | Microsoft Azure | Υπηρεσία αναζήτησης φιλοξενούμενη cloud" 
    description="Μάθετε πώς να δημιουργείτε ένα πληροφορίες πύλης και μετα μηχανισμό αναζήτησης με χρήση της αναζήτησης Azure, μια υπηρεσία cloud φιλοξενούνται αναζήτησης για τους προγραμματιστές." 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

# <a name="azure-search-developer-case-study"></a>Περιπτωσιολογική μελέτη προγραμματιστής Azure αναζήτησης

## <a name="how-whattopediacomhttpwhattopediacom-built-an-infomedia-portal-on-microsoft-azure"></a>Πώς [WhatToPedia.com](http://whattopedia.com/) δημιουργείται μια πύλη infomedia στο Microsoft Azure

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">Η γενική ιδέα</font> 


Μας ιδέα είναι να δημιουργήσετε μια πύλη πληροφοριών που βοηθούν οι πελάτες να συνδεθείτε με μεταπωλητές μέσω μιας υψηλής συνάφειας, εύρος αναζήτησης εμπειρία, παρόμοια με τον τρόπο ταξιδιού τουρίστες match πύλες του με το ξενοδοχεία, εστιατόρια και διασκέδαση όταν σε uncharted περιοχών. 

Η πύλη μας αναμένετε θα παραδώσετε μια εμπειρία αναζήτησης εξαιρετικά υψηλής ποιότητας επάνω από κατάστημα λιανικής πώλησης δεδομένα σε μια δεδομένη αγορά, βοηθώντας οι πελάτες Εύρεση αποθηκεύει βάση θέση και άλλες θετικά το κατάστημα λιανικής πώλησης παρέχει. Θα σας θα σπείρεται το μηχανισμό αναζήτησης με ένα αρχικό σύνολο δεδομένων, αλλά θα δημιουργηθεί βαθύτερη τιμή διάρκεια του χρόνου, με τη Βοήθεια των συνδρομητών κατάστημα λιανικής πώλησης που πραγματοποιούν δημοσιεύσεις πληροφορίες σχετικά με την επιχείρησή τους. Προσφορές νέο εμπόρευμα, δημοφιλείς σήματα, εσωτερικά εξειδικευμένες εφαρμογές υπηρεσιών-– όλα είναι παραδείγματα των δεδομένων που προσθέτει τιμή για την τοποθεσία μας. Αυτά τα δεδομένα είναι αυτο-αναφερθεί και ενσωματωμένο στο του σώματος αναζήτησης, όταν το κατάστημα λιανικής πώλησης εγγράφεται ως συνδρομητή. Διαφημίσεις, καθώς και το μοντέλο συνδρομή, δώστε στη ροή εσόδων για το νέο επιχειρήσεις.

Αναζήτηση θα μοντέλο αλληλεπίδραση κύριο λόγο χρήστη, σε μια πλατφόρμα αμιγώς cloud. Για σκοπούς κλίμακα και χαμηλό κόστος, σχεδόν ό, τι κάνουμε, από την πύλη εμπειρία στο στοιχείο ελέγχου πηγαίου, θα είναι σε μια ηλεκτρονική υπηρεσία. Χρησιμοποιώντας μια μηχανή αναζήτησης που παρέχει τις δυνατότητες χρειαζόμαστε από το πλαίσιο, μπορούμε να δημιουργήσουμε μια εφαρμογή αναζήτησης γρήγορα, χωρίς να χρειάζεται να δημιουργήσετε και να διαχειριστείτε μια αναζήτηση μηχανισμό αναφερόμαστε.

## <a name="what-we-built"></a>Τι θα σας δημιουργηθεί

WhatToPedia είναι μια εταιρεία infomedia εκκίνησης. Θα σας ενσωματωμένη [WhatToPedia.com](http://whattopedia.com/) – - αυτήν τη στιγμή στο δοκιμής-αγορά στη Βόρεια Ευρώπη με live Μετάβαση ημερομηνία 2 Φεβρουαρίου 2015. Μας βάση πελατών είναι κυρίως καταστήματα ορθογώνιο και κονιάματος που χρειάζεται μια οικονομική ηλεκτρονικής παρουσίας που είναι εύκολο να διαχειριστείτε και να διατηρήσετε.

Εργασία μας είναι να προσελκύσετε οι πελάτες μέσω μια εμπειρία εξαιρετική αναζήτηση στο Internet, ενίσχυση αποτελεσμάτων που βασίζεται στην πόλη ή περιοχή, σήματα, αποθήκευση ονομάτων ή αποθηκεύουν τύπους. Προσέλκυση οι πελάτες έχει αλυσιδωτή επίδραση, motivating μεταπωλητές για να εγγραφείτε για να μας τοποθεσίας πύλης. Οι εγγραφές είναι αμοιβή, με μια οικονομική χρέωση.

 ![][7] 

Αφού εγγραφείτε για μια συνδρομή, ένα κατάστημα λιανικής πώλησης μεταφέρει πάνω από το υπάρχον προφίλ (που δημιουργήθηκε αρχικά από εμάς από αγορά δεδομένων), ενημέρωση της με πρόσθετα δεδομένα σχετικά με τις προσφορές, διαθέσιμης σήματα ή ανακοινώσεις. Οι δυνατότητες εντός της εταιρείας, όπως γλώσσες της, απαντηθεί νομισματικές μονάδες, πρατήρια αγορών, μπορεί να είναι αυτο-αναφερόμενου για να προσελκύσετε καλύτερα οι πελάτες που αναζητάτε αυτά τα πλεονεκτήματα της περιοχής.

## <a name="who-we-are"></a>Που θα σας είναι

Το όνομά μου βρίσκεται Παπαδόπουλος Segato (Microsoft πρόγραμμα συμβουλών) και να εργαστεί με Boelling Ιωσήφ, υποψήφιου πελάτη για προγραμματιστές στο WhatToPedia, για να σχεδιάσετε τη λύση. 

WhatToPedia είναι μια εκκίνησης, δοκιμής μάρκετινγκ δραστηριότητάς νέα πύλη στη Σουηδία, όπου οι περισσότερες από το 60.000 μεταπωλητές είναι ορθογώνιο και κονιάματος ΜΜΕ (μικρές μικρομεσαίες επιχειρήσεις). Επειδή θα σας ενημερώσει ότι ένα άτομο αγορών στην Ευρώπη εκφωνεί πολλές γλώσσες και φέρει πολλές νομισματικές μονάδες, μπορέσουμε να δημιουργήσουμε λύσεις που περιλαμβάνει μια πολυγλωσσική πελάτης. Εργαζόμαστε χρειάζεται και βρεθεί, μια μηχανή αναζήτησης που υποστηρίζει μας πολύγλωσσες απαιτήσεις στο Azure αναζήτησης.

Azure αναζήτησης έχει μια παιχνίδι συσκευή εναλλαγής για το έργο. Πριν από τη διαθεσιμότητα των Azure αναζήτησης, θα σας αναλωθεί αξιοσημείωτη ενέργειας εργασία μέσω του kinks της δόμησης τη δική μας μηχανή αναζήτησης. Έχοντας Azure αναζήτησης, όπως μια ηλεκτρονική υπηρεσία έχει καταργηθεί η μεγαλύτερων αποκλεισμού τεχνική και διοικητική από τη λύση, που προορίζονται γρήγορα για να προωθήσετε πιο γρήγορα και με μια πιο ισχυρό εμπειρία αναζήτησης.  

## <a name="how-we-did-it"></a>Πώς κάναμε

Μας όραση ήταν για να δημιουργήσετε μια ολοκληρωμένη υποδομή που βασίζεται στο cloud services μόνο. Microsoft έχει επιλεγεί ως την πλατφόρμα στρατηγική επειδή ήταν η υπηρεσία παροχής που παρέχεται τις απαραίτητες υπηρεσίες (τόσο για συνεργασία και ανάπτυξη), κλιμάκωση σε ζήτηση και οικονομική τις πληροφορίες τιμολόγησης.
 
### <a name="high-level-components"></a>Στοιχεία υψηλού επιπέδου

Θα σας δημιουργηθεί μια επιχείρηση, όχι μόνο μια τοποθεσία. Υποστήριξη της ολόκληρο προσπάθειας απαιτείται ένα πλήρες εύρος των εργαλείων και εφαρμογών. Θα σας υιοθετήσει Visual Studio και το Visual Studio ομάδας υπηρεσίες για ανάπτυξη, Online υπηρεσία Foundation ομάδας (TFS) για το στοιχείο ελέγχου πηγαίου scrum διαχείριση και, στο Office 365 για επικοινωνίας και συνεργασίας και, φυσικά Microsoft Azure για όλες τις εργασίες που σχετίζονται με την τοποθεσία και χώρου αποθήκευσης. Με το Visual Studio, IDE παρέχονται απευθείας προμήθεια Azure, με την ενοποίηση με το Online TFS παρέχει μια ενίσχυση επιπλέον την παραγωγικότητα.

Το παρακάτω διάγραμμα παρουσιάζει τα στοιχεία υψηλού επιπέδου που χρησιμοποιούνται στην υποδομής WhatToPedia.

   ![][8]

### <a name="how-we-use-microsoft-azure"></a>Πώς χρησιμοποιούμε Microsoft Azure

Εξετάζοντας πράσινο πλαίσια στο προηγούμενο διάγραμμα, θα δείτε ότι η λύση WhatToPedia είναι ενσωματωμένη σε αυτές τις υπηρεσίες:

- [Azure αναζήτησης](https://azure.microsoft.com/services/search/)
- [Azure τοποθεσίες Web χρησιμοποιώντας MVC 4](https://azure.microsoft.com/services/websites/)
- [Azure WebJobs για τις προγραμματισμένες εργασίες](../app-service-web/websites-webjobs-resources.md)
- [Βάση δεδομένων SQL Azure](https://azure.microsoft.com/services/sql-database/)
- [Χώρος αποθήκευσης αντικειμένων BLOB του Azure](https://azure.microsoft.com/services/storage/)
- [Παράδοση μηνυμάτων ηλεκτρονικού ταχυδρομείου SendGrid](https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

Η πολύ καρδιά της λύσης είναι τα δεδομένα και αναζήτησης. Η ροή δεδομένων από την υπηρεσία παροχής μεταπωλητή στον πελάτη τέλος απεικονίζεται παρακάτω:

  ![][9]

Χώρος αποθήκευσης δεδομένων πρωτεύοντος είναι η μεταπωλητή και λογιστικά δεδομένα στη βάση δεδομένων SQL Azure. Αυτό αποτελείται από το αρχικό σύνολο δεδομένων, καθώς και τα δεδομένα που αφορούν συγκεκριμένα το κατάστημα λιανικής πώλησης προστίθενται μέσα στο χρόνο. Χρησιμοποιούμε τη μια WebJob Azure για να δημοσιεύσετε ενημερώσεις από βάση δεδομένων SQL του σώματος αναζήτησης στο πλαίσιο Αναζήτηση Azure.

### <a name="presentation-layer"></a>Επίπεδο παρουσίασης

Η πύλη είναι μια τοποθεσία Web του Azure, υλοποιηθεί σε MVC 4 και [Twitter εκκίνησης](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29). Επιλέγουμε MVC, επειδή παρέχει μια πολύ πιο καθαρή προσέγγιση σε HTML από ανάπτυξης βασίζεται σε φόρμες ASP.NET. Για να μην χρειάζεται να δημιουργήσετε εφαρμογές για πολλές συσκευές και να διατηρήσετε πολλές πλατφόρμες κινητές συσκευές, Twitter εκκίνησης έχει επιλέξει να υποστηρίζουν όλες τις συσκευές και πλατφόρμες.

### <a name="authentication-permissions-and-sensitive-data"></a>Έλεγχος ταυτότητας, δικαιώματα και ευαίσθητα δεδομένα

Οι πελάτες ανώνυμα περιήγηση στην τοποθεσία. Ως εκ τούτου, υπάρχουν login απαιτήσεις για οι πελάτες, ούτε κάντε αποθηκεύει τα δεδομένα καταναλωτή. 

Μεταπωλητές είναι διαφορετικές κειμένου. Εδώ, αποθηκεύει τις πληροφορίες του προφίλ δημόσια τοποθεσία, τις πληροφορίες χρέωσης και περιεχομένου πολυμέσων που θέλουν να εκθέσετε στην τοποθεσία. Κάθε κατάστημα λιανικής πώλησης που στο οποίο έχει εγγραφεί στην τοποθεσία λάβετε μια σύνδεση χρήστη, χρησιμοποιούνται για τον έλεγχο ταυτότητας χρήστη πριν από την πραγματοποίηση ενημερώσεις στο προφίλ του συνδρομητή.  Αποθηκεύει με ασφάλεια όλων των συνδρομητών δεδομένων σε βάση δεδομένων SQL Azure και αντικειμένων BLOB του Azure χώρου αποθήκευσης.
Θα σας επιλέξει για ένα μοντέλο έλεγχο ταυτότητας βάσει τον έλεγχο ταυτότητας βάσει φορμών .NET. Επιλέγουμε αυτήν την προσέγγιση για την απλότητα; δεν χρειαζόμαστε τους ρόλους, υποστήριξη του περιβάλλοντος εργασίας Χρήστη και άλλες άσχετοι δυνατότητες που παρέχονται με άλλες μεθόδους. 

Για να εξασφαλίσετε ότι μεταπωλητές βλέπουν μόνο τα δεδομένα που ανήκει σε αυτά, που δημιουργήσαμε ένα κατάστημα λιανικής πώλησης Αναγνωριστικό για κάθε κατάστημα λιανικής πώλησης που χρησιμοποιείται αργότερα σε όλους λειτουργίες ανάγνωσης και εγγραφής που αφορούν δεδομένων για το συγκεκριμένο κατάστημα λιανικής πώλησης. Με αυτήν την προσέγγιση, που βρέθηκαν που δεν πρέπει να εκχωρήσετε δικαιώματα βάσης δεδομένων σε μεμονωμένα λιανικής. Όλοι οι μεταπωλητές αλληλεπίδραση με το σύστημα κάτω από ένα ρόλο μία βάση δεδομένων, με το Αναγνωριστικό κατάστημα λιανικής πώλησης ως μας τεχνική απομόνωσης δεδομένων.

Επειδή μας επιχειρήσεις είναι όλα σχετικά με τα εφέ ροής (οδήγησης περισσότερες επιχειρήσεις σε πωλητές λιανικής πώλησης, τη δημιουργία να κοινοποιήσετε και εγγραφή), θα σας να σχεδιάσετε τη γραμμή στο χειρισμός αγορές στο Web. Ως εκ τούτου, δεν θα βρείτε στο καλάθι αγορών στην τοποθεσία μας, η οποία απλοποιεί μας τις απαιτήσεις ασφαλείας. 

Μια άλλη απλοποίηση που χρησιμοποιήθηκαν ήταν για να μεταβιβάζουν μας πληρωτέων λειτουργίες χρέωσης και λογαριασμών. Με πληροφορίες πληρωμής πελατών δρομολόγησης απευθείας σε έναν τρίτο ([SveaWebPay](http://www.sveawebpay.se/)), μπορούμε να μειώσουμε τους κινδύνους συσχετίζοντας με την αποθήκευση και την προστασία ευαίσθητα δεδομένα μας χώροι αποθήκευσης δεδομένων. 

### <a name="search-engine"></a>Μηχανισμός αναζήτησης

Το μέγεθος των κύριων μας λύσης είναι το μηχανισμό αναζήτησης που βασίζεται στην υπηρεσία αναζήτησης Azure. Αρχικά, θα σας δημιουργηθεί ένας μηχανισμός προσαρμοσμένης αναζήτησης, αλλά κατά τη διάρκεια αυτής της διαδικασίας, θα σας πραγματοποιείται την πολυπλοκότητα προσπάθειας ήταν πολύ υψηλή στην πραγματικότητα και σας ζητηθεί να λάβετε υπόψη άλλες εναλλακτικές λύσεις. 

Βασικές δυνατότητες που έχουν πιο σημαντικές μας περιλαμβάνονται:

- Φίλτρα
- Περιήγηση με συνθήκη
- Ενίσχυση της αποτελεσμάτων
- Σελιδοποίηση AJAX

Μια αναζήτηση στο internet έφερε μας για το βίντεο που ακολουθεί, που ενέπνευσαν μας για να δώσετε αναζήτησης Azure δοκιμή: [Βαθύ κατάρρευση στο TechEd Europe](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

Μετά την παρακολούθηση του βίντεο, ήταν έτοιμοι να δημιουργήσετε ένα πρωτότυπο που βασίζονται σε τι θα σας είδατε. Επειδή έπρεπε ήδη ένα μοντέλο δεδομένων στο MVC, τη δημιουργία το πρωτότυπο είχε απλές επειδή τα δεδομένα που περιέχονται όρους αναζήτησης και θα σας είχε ήδη ολοκληρώθηκε με επιτυχία τις απαιτήσεις για τον τρόπο θέλουμε να ταξινομήσετε, όψη, και να φιλτράρετε τα δεδομένα. 

Αυτό είναι πώς θα σας δημιουργηθεί το πρωτότυπο.

**Ρύθμιση παραμέτρων υπηρεσίας αναζήτησης Azure**

1. Συνδεθείτε στην πύλη κλασική Azure και προσθήκη της υπηρεσίας αναζήτησης για να μας συνδρομής. Χρησιμοποιήσαμε την κοινόχρηστη έκδοση (δωρεάν με τη συνδρομή μας).
2. Δημιουργία ευρετηρίου. Για το πρωτότυπο, χρησιμοποιήσαμε την πύλη του περιβάλλοντος εργασίας Χρήστη για να ορίσετε τα πεδία αναζήτησης και να δημιουργήσετε τα βαθμολογίας προφίλ. Μας βαθμολογίας προφίλ βασίζεται σε δεδομένα τοποθεσίας: χώρα | Πόλη | διεύθυνση (ανατρέξτε στο θέμα: Προσθήκη βαθμολογίας προφίλ).
3. Αντιγράψτε τη διεύθυνση URL της υπηρεσίας και διαχείρισης api-κλειδί για να μας αρχεία ρύθμισης παραμέτρων. Αυτό το κλειδί είναι στη σελίδα υπηρεσίας αναζήτησης στην πύλη και χρησιμοποιείται για τον έλεγχο ταυτότητας για την υπηρεσία.
    
**Ανάπτυξη μιας εργασίας ευρετήριο αναζήτησης – κονσόλας Windows**

1. Διαβάστε όλα μεταπωλητών από βάση δεδομένων.
2. Καλέστε το Azure API της υπηρεσίας αναζήτησης για να αποστείλετε ένα προς ένα μεταπωλητών (ανατρέξτε στο θέμα: http://msdn.microsoft.com/library/azure/dn798930.aspx).
3. Ορισμός ιδιότητας σε βάση δεδομένων που συμπεριλαμβάνεται στο ευρετήριο μεταπωλητή για τη δημιουργία ευρετηρίου αυξάνονται. Κάναμε αυτό, προσθέτοντας ένα πεδίο 'ευρετήριο' που αποθηκεύει την κατάσταση ευρετηρίου κάθε προφίλ (με ευρετήριο ή όχι). 

Ανατρέξτε στο θέμα προσάρτημα για το τμήμα κώδικα που δημιουργεί το έργο ευρετηρίου.

**Ανάπτυξη μιας πύλης Web αναζήτησης – MVC**

1. Κλήση Azure υπηρεσίας αναζήτησης για να λάβετε όλα τα έγγραφα από την αναζήτηση (ανατρέξτε στο θέμα: http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. Εξαγάγετε παρακολούθηση από την ανταπόκριση της υπηρεσίας αναζήτησης (χρησιμοποιώντας json.net http://james.newtonking.com/json)
   - Αποτελέσματα
   - Όψεις
   - Καταμετρά αποτέλεσμα
   - Αναπτύξτε ένα περιβάλλον εργασίας χρήστη για την εμφάνιση αποτελεσμάτων αναζήτησης, όψεις και το πλήθος (ήδη έπρεπε αυτό).

Αυτός είναι ο κωδικός χρησιμοποιήσαμε για να λάβετε τα αποτελέσματα από την αναζήτηση Azure:

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**Ενίσχυση της κατά θέση**

Πιθανώς η πιο σημαντικές απαίτηση για να επαληθεύσετε στο το πρωτότυπο περιλαμβάνονται προσθέτοντας μια λέξη-κλειδί αναζήτησης θέση στο ερώτημα. Είναι απαραίτητο να μας πύλη, η οποία αν ένας χρήστης εισάγει το όνομα μιας πόλης στην αναζήτηση ερωτήματος, που οι μεταπωλητές στην πόλη που δίνεται θα έχει υψηλότερη κατάταξη από μεταπωλητές που έχουν τη λέξη-κλειδί Πόλη στην περιγραφή. Για αυτήν την απαίτηση, χρησιμοποιήσαμε βαθμολογίας προφίλ κατάταξης του στοιχείου στο πεδίο Πόλη υψηλότερα από άλλα πεδία.

**Υποστήριξη πολλών γλωσσών**

Θα σας που απαιτούνται για να εμφανίζονται σωστά αποτελέσματα αναζήτησης σε σωστή γλώσσες και παρέχουν μια επιλογή για την εύρεση τα ίδια αποτελέσματα σε διαφορετικές γλώσσες. Ήταν οι δύο πλευρές σε αυτό το πρόβλημα: 

- Αναζήτηση για λέξεις σε πολλές γλώσσες
- Εμφάνιση των αποτελεσμάτων αναζήτησης στη σωστή γλώσσα

Θα σας επιλυθεί το τμήμα παρουσίαση με την προσθήκη ενός εγγράφου για κάθε γλώσσα με μεταφρασμένα κείμενο και μια ιδιότητα με τη γλώσσα. Όταν ένας χρήστης εισαγάγει έναν όρο αναζήτησης, θα σας χρήστη `$filter` παραστάσεις για να φιλτράρετε με βάση τη γλώσσα ο χρήστης έχει επιλέξει.

Κάθε ένα από τα έγγραφα που έχει κρυφά ιδιότητα που ονομάζεται "πόλεις" ενσωματωμένη σε ο τύπος συλλογής. Αυτή η ιδιότητα αποθηκεύει ονόματα των πόλεων σε όλες τις γλώσσες, επιτρέπει στο χρήστη για να πραγματοποιήσετε αναζήτηση σε πολλές γλώσσες.

###<a name="data-storage"></a>Χώρος αποθήκευσης δεδομένων

Όλα τα δεδομένα (προφίλ, τη συνδρομή και λογιστική) αποθηκεύονται στη βάση δεδομένων SQL. Όλα τα αρχεία πολυμέσων αποθηκεύονται στο χώρο αποθήκευσης αντικειμένων BLOB του Azure, όπως εικόνες και βίντεο που παρέχονται από το κατάστημα λιανικής πώλησης. Χρήση ξεχωριστό χώρο αποθήκευσης αντικειμένων BLOB απομονώνει την επίδραση της αποστολής αρχείων. τα αρχεία είναι ποτέ από κοινού mingled στην τοποθεσία Web, επομένως δεν πρέπει να ξαναδημιουργήσετε στην τοποθεσία, κάθε φορά που θα σας Προσθήκη αρχείων.

Ένα σημαντικό πλεονέκτημα της μας σχεδίαση χώρου αποθήκευσης είναι ότι πολλές τους προγραμματιστές να κάνετε κοινή χρήση ένα μεμονωμένο ανάπτυξης χώρου αποθήκευσης. Μία από τις απαιτήσεις για το έργο WhatToPedia ήταν για να μπορέσετε να δημιουργήσετε ένα περιβάλλον ανάπτυξης μέσα σε 15 λεπτά, συμπεριλαμβανομένων των δεδομένων μεταπωλητή, εικόνων και βίντεο. Λήψη των πιο πρόσφατων δεδομένων από TFS Online, εκτελεί μια δέσμη ενεργειών SQL και εκτελεί την εργασία εισαγωγής, ένα πλήρες περιβάλλον μπορεί να είναι stood προς τα επάνω σε χρόνο μηδέν σε όλα. Αυτή η πρακτική βελτιώνει επίσης τη διεργασία τοποθέτησης.

###<a name="webjobs"></a>WebJobs

Χρησιμοποιούμε Azure WebJobs για να ενημερώσετε δεδομένα στο ευρετήριο. Με τη δημιουργία μιας εργασίας ευρετήριο αναζήτησης, το τμήμα ευρετηρίου ήταν πολύ εύκολη να ενσωματώσετε σε λύσης μας. Η μόνη αλλαγή κώδικα που θα σας κάνει ήταν ώστε να περιλαμβάνει την εργασία ευρετηρίου ήταν για να προσθέσετε μια `Indexed` πεδίο μας μοντέλο δεδομένων για να υποδείξετε την κατάσταση ευρετηρίου. Κάθε φορά που προστίθεται ένα νέο προφίλ ή ενημερωθεί, το `Indexed` πεδίο έχει οριστεί σε false. Το ίδιο ισχύει, εάν το κατάστημα λιανικής πώλησης αλλάζει το φάκελό δεδομένων προφίλ μέσω της πύλης.  

Η εργασία έχει για όλες τις γραμμές που αντιμετωπίζετε `Indexed` την τιμή false. Όταν εντοπίζει τη γραμμή, το έγγραφο καταχωρείται στον Azure αναζήτηση, και, στη συνέχεια, το `Indexed` πεδίο έχει οριστεί στην τιμή true. Δεν έχουμε για το σχεδιασμό για την προσθήκη και ενημέρωση δεδομένων, επειδή η υπηρεσία αναζήτησης Azure στην πραγματικότητα αναλαμβάνει αυτό. Εάν προσθέσετε ένα έγγραφο που υπάρχει ήδη, η υπηρεσία θα κάνετε αυτόματα μια ενημερωμένη έκδοση.

Όλες οι εργασίες web έχουν αναπτυχθεί ως κονσόλας εφαρμογές που μπορούν να αποστείλει σε Azure τοποθεσίες web ως αρχεία ZIP, αποσυμπιεσμένο αρχείο, και, στη συνέχεια, έχει προγραμματιστεί.

Η εργασία είναι προγραμματισμένη για να εκτελέσετε κάθε 5 λεπτά ως web προγραμματισμένη εργασία. Θα σας υπολογισμού που χρειάζεται η υπηρεσία περίπου τρία λεπτά για να αποστείλετε 3.000 έγγραφα, που έχει εντός μας απαιτήσεις. 

> [AZURE.NOTE] Υπάρχει μια δυνατότητα ευρετηρίου πρωτοτύπου που παρουσιάστηκε πρόσφατα στην αναζήτηση Azure. Η δυνατότητα αυτή ήταν πολύ αργά για να χρησιμοποιήσετε το πρόγραμμα πρώτης κυκλοφορίας, αλλά εμφανίζεται για να επιλύσετε το ίδιο πρόβλημα χρησιμοποιήσαμε μας ευρετηρίου το έργο, που είναι για την αυτοματοποίηση Λειτουργίες φόρτωσης δεδομένων.


###<a name="backup-strategy"></a>Στρατηγική αντιγράφων ασφαλείας

Θα σας έχει σχεδιαστεί μια στρατηγική αντιγράφων ασφαλείας πολλαπλών επιπέδων για την ανάκτηση από μια περιοχή σενάρια, από καταστροφική αποτυχία, προς τα κάτω, αποκατάστασης από μια συγκεκριμένη συναλλαγή. Τα στοιχεία για την προστασία περιλαμβάνει τρία είδη δεδομένων (τοποθεσία web, δεδομένα συνδρομητή και αρχεία πολυμέσων). 

Πρώτα, διατηρώντας τον πηγαίο κώδικα της τοποθεσίας web στο TFS Online, γνωρίζουμε ότι εάν μεταβαίνει στην τοποθεσία προς τα κάτω, θα σας μπορεί να δημιουργήσετε ξανά με αναδημοσίευση από TFS. 

Συνδρομητών δεδομένων σε βάση δεδομένων SQL Azure είναι η πιο ευαίσθητα περιουσιακών στοιχείων. Θα σας ξανά αυτό χρησιμοποιώντας τα ενσωματωμένα δυνατοτήτων (ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας της βάσης δεδομένων SQL Azure και επαναφορά](http://msdn.microsoft.com/library/azure/jj650016.aspx)). Το χρονοδιάγραμμα αντιγράφων ασφαλείας είναι αντίγραφο ασφαλείας βάσης δεδομένων πλήρους μία φορά την εβδομάδα, διακριτική βάση δεδομένων μία φορά την ημέρα, και συναλλαγής καταγραφής αντιγράφων ασφαλείας κάθε 5 λεπτά.  Δεδομένο το μέγεθος των δεδομένων, αυτή η λύση είναι μεγαλύτερο από το κανονικό για μας όγκους δεδομένων άμεση και προβλεπόμενη.

Τέλος, αποθηκεύει αρχεία εικόνας και βίντεο στο χώρο αποθήκευσης αντικειμένων BLOB του Azure. Θα σας εξακολουθούν να αξιολόγησης το ultimate πρόγραμμα δημιουργίας αντιγράφων ασφαλείας για αυτά τα δεδομένα, εξετάζοντας Explorer Cloudberry για Azure ως πιθανή λύση. Προς το παρόν, μπορούμε να χρησιμοποιήσουμε ένα WebJob για να αντιγράψετε εικόνες και βίντεο σε μια άλλη θέση.

##<a name="what-we-learned"></a>Τι θα σας μάθατε

Επειδή έπρεπε ήδη δεδομένα, ήταν εύκολο να δημιουργήσετε απόδειξη λειτουργίας. Μέσα σε ώρες, έπρεπε πρωτότυπο με όψεις και μετρητές, σελιδοποίηση, κατάταξης προφίλ και τα αποτελέσματα αναζήτησης. Τα αποτελέσματα της αναζήτησης έχουν επομένως ακριβείς, θα σας αποφασίσει να καταργήσετε ορισμένα από τα φίλτρα που παρουσιάζονται στον πελάτη τέλος. 

Το μεγαλύτερων εκπλήξεις για μας ήταν της ταχύτητας κίνησης θα σας θα μπορούσε να μάθετε Azure αναζήτησης και πόσο θα σας στη διάθεσή σας ξανά. Ως έχουν, θα σας έχει δημιουργηθεί απόδειξη λειτουργίας σε μερικές ώρες (δείτε τη σημείωση παρακάτω), αντικαθιστώντας 500 γραμμές κώδικα με 3 γραμμές του κώδικα στην εφαρμογή του προσκηνίου (σύμβολο συν μια νέα WebJob) και επίτευξη καλύτερων αποτελεσμάτων. 

Στο παρελθόν, μας κώδικα υλοποιηθεί σελιδοποίησης, πλήθος και άλλες συμπεριφορές που είναι τυπική για να πραγματοποιήσετε αναζήτηση. Με χρήση της αναζήτησης Azure, τα αποτελέσματα θα σας να επιστρέψετε περιλαμβάνουν τα επισκέψεων αναζήτησης, όψεις, σελιδοποίηση δεδομένων, πλήθος--όλα τα στοιχεία χρησιμοποιούμε χρειάζεται και χρειάζεται να παρέχετε αναφερόμαστε. Περιλαμβάνεται επίσης ενίσχυση και την ενσωματωμένη περιήγηση με συνθήκη, που δεν υπήρχαν στο αρχικό λύσης μας.

Η μεγαλύτερη πρόκληση κατά τη διάρκεια της εφαρμογής ήταν ότι ήταν μια προέκδοση και μπορείτε να βρείτε πληροφορίες και κοινόχρηστο εμπειρίες ήταν δύσκολη. Μόλις μας συνδεθεί μερικά τελείες, που βρέθηκαν ότι με υπηρεσία αναζήτησης Azure ήταν πολύ απλό λόγω τη μορφοποίηση δεδομένων REST API και JSON. Θα μπορούσε να ονομάζουμε πλαίσιο απευθείας από το πιο ανοιχτή προσθήκες προέλευσης όπως JQuery JSON.Net και θα μπορούσε να χρησιμοποιήσουμε εργαλεία όπως Fiddler για γρήγορη πειραματισμός και τον εντοπισμό σφαλμάτων. 

> [AZURE.NOTE] Εκτός από το με τα δεδομένα προετοιμαστεί, βοήθησε αυτά του μας δημιουργία ήδη το πρωτότυπο κατανοητή πως λειτουργεί τεχνολογία, καθιστώντας μας πιο παραγωγικοί και πιο appreciative ενσωματωμένες δυνατότητες η αναζήτηση. Εάν θέλετε να εκπαιδευτείτε κατασκευή ερωτήματος αναζήτησης, περιήγηση με συνθήκη, φίλτρα, κ.λπ. θα πρέπει να αναμένετε τη δημιουργία πρωτοτύπων για να διαρκέσει περισσότερο. 

###<a name="controlling-facets-in-the-search-presentation-page"></a>Έλεγχος όψεις στη σελίδα αναζήτησης παρουσίασης

Έναν από τους learnings κατά τη διάρκεια του απόδειξη λειτουργίας ήταν σχεδιασμού όψεις προσεκτικά προκαταβολικά. Μετά τη φόρτωση πολλά δεδομένα σε τη λύση, θα σας είδατε ότι ήταν πολύ υψηλή για να παρουσιάσετε στους χρήστες του μεγάλου όγκου όψεις. 

Θα σας επιλυθεί, ο περιορισμός την παράμετρο μέτρηση όψη. Η παράμετρος count επιβάλλει ένα όριο στον αριθμό των όψεις επιστρέφονται στο χρήστη. Μπορείτε να βρείτε μια σύνδεση που περιλαμβάνει μια συζήτηση σχετικά με την παράμετρο μέτρηση [εδώ](search-faceted-navigation.md).

###<a name="webjobs-for-scheduling-tasks"></a>WebJobs για τον προγραμματισμό εργασιών

Azure αναζήτησης δεν το μόνο ευχάριστη εκπλήξεις για μας. Θα σας εντόπισε ότι χρησιμοποιώντας WebJobs για την αυτοματοποίηση μας λειτουργίες φόρτωσης δεδομένων στην αναζήτηση Azure ήταν σημαντικά ανώτερη από το προηγούμενο προσέγγιση, η οποία συνεπάγεται επιπτώσεις χρησιμοποιώντας μια αποκλειστική Εικονική που εκτελεί Windows Scheduler, με τις προγραμματισμένες εργασίες για την ενημέρωση του ευρετηρίου αναζήτησης. WebJobs ήταν απλούστερο να ρυθμίσετε τις παραμέτρους και διευκολύνουν την εντοπισμού σφαλμάτων και, φυσικά ιδιαίτερα κοστίζει από χρειάζεται να πληρώσετε για ένα αποκλειστικό Εικονική.

###<a name="azure-blob-storage-explorer-for-updating-images"></a>Azure Εξερεύνηση χώρου αποθήκευσης αντικειμένων BLOB για την ενημέρωση των εικόνων

Εντοπίσαμε που χρησιμοποιώντας την [Εξερεύνηση χώρου αποθήκευσης αντικειμένων BLOB του Azure](https://azurestorageexplorer.codeplex.com/) (διαθέσιμη στο codeplex) να είναι πολύ χρήσιμο στη διαχείριση των ενημερώσεων εικόνας και βίντεο στην τοποθεσία. Χρησιμοποιούμε την ως εργαλείο προγραμματιστή για να ενημερώσετε με μη αυτόματο τρόπο εικόνες και βίντεο που αποτελούν μέρος της μας κύρια τοποθεσία. Εργαζόμαστε βρέθηκε να είναι πιο ευέλικτη από την ανάπτυξη αλλαγές στην πύλη και αποκλείει μια πλήρη έλεγχο διαδοχικών προσεγγίσεων κάθε φορά που πρέπει να ενημερώσετε μια εικόνα. 

##<a name="a-few-final-words"></a>Μερικές λέξεις τελικό

Σας ευχαριστούμε για την εξαιρετική άτομα στο WhatToPedia επιτρέποντας μας για την κοινή χρήση τους ιστορίας!  

Θα σας ελπίζετε βρέθηκε αυτό περιπτωσιολογική μελέτη χρήσιμες. Εάν μεταβείτε για να χρησιμοποιήσετε την αναζήτηση Azure, να συνιστάται μερικά πόρους για να επιταχύνετε που κατά μήκος:

- [Φόρουμ MSDN αφοσιωμένη στο να Azure αναζήτησης](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [StackOverflow περιλαμβάνει επίσης μια ετικέτα](http://stackoverflow.com/questions/tagged/azure-search)
- [Τεκμηρίωση σελίδας στην Azure.com](https://azure.microsoft.com/documentation/services/search/)
- [Azure τεκμηρίωση αναζήτησης στο MSDN](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##<a name="appendix-search-indexer-webjob"></a>Προσάρτημα: WebJob ευρετήριο αναζήτησης

Ο ακόλουθος κώδικας δημιουργεί το ευρετήριο που αναφέρονται στην ενότητα σχετικά με τη δημιουργία του πρωτοτύπου.

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Subheading 4]: #subheading-4
[Subheading 5]: #subheading-5
[Next steps]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: ../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
 
