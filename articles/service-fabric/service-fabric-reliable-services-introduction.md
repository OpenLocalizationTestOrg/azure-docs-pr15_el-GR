<properties
   pageTitle="Επισκόπηση του μοντέλου προγραμματισμού ύφασμα αξιόπιστη η υπηρεσία | Microsoft Azure"
   description="Μάθετε σχετικά με το μοντέλο προγραμματισμού αξιόπιστη υπηρεσία ύφασμα της υπηρεσίας και να ξεκινήσετε να γράφετε το δικό σας υπηρεσίες."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor="vturecek; mani-ramaswamy"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="masnider;vturecek"/>

# <a name="reliable-services-overview"></a>Επισκόπηση των υπηρεσιών αξιόπιστη
Azure Service ύφασμα απλοποιεί γράφετε και τη Διαχείριση αξιόπιστων υπηρεσιών χωρίς κατάσταση και με κατάσταση. Αυτό το έγγραφο θα μιλήσετε σχετικά με:

- Το αξιόπιστων υπηρεσιών μοντέλο προγραμματισμού για τις υπηρεσίες χωρίς κατάσταση και με κατάσταση.
- Οι επιλογές που πρέπει να κάνετε όταν γράφετε μια αξιόπιστη υπηρεσία.
- Ορισμένα σενάρια και παραδείγματα πότε να χρησιμοποιείτε τις υπηρεσίες αξιόπιστη και τον τρόπο εγγραφής.

Αξιόπιστη Services είναι ένα από τα μοντέλα προγραμματισμού που είναι διαθέσιμα σε ύφασμα υπηρεσίας. Για περισσότερες πληροφορίες σχετικά με το μοντέλο προγραμματισμού αξιόπιστη ηθοποιών, ανατρέξτε στο θέμα [Εισαγωγή στην υπηρεσία ύφασμα αξιόπιστη ηθοποιών](service-fabric-reliable-actors-introduction.md).

Στην υπηρεσία ύφασμα, μια υπηρεσία είναι αποτελούνται από τη ρύθμιση παραμέτρων, κώδικα της εφαρμογής, και κατάστασης (προαιρετικά).

Υπηρεσία ύφασμα διαχειρίζεται της διάρκειας ζωής των υπηρεσιών, από την προμήθεια και την ανάπτυξη μέσω αναβάθμιση και διαγραφή, μέσω [Διαχείριση εφαρμογών υπηρεσίας ύφασμα](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Τι είναι οι υπηρεσίες αξιόπιστη;
Αξιόπιστη υπηρεσίες σάς παρέχει ένα απλό, ισχυρή, ανώτατου επιπέδου μοντέλο προγραμματισμού θα σας βοηθήσουν να εκφράσετε τι είναι σημαντικό για την εφαρμογή σας. Με αξιόπιστων υπηρεσιών προγραμματισμού μοντέλο, έχετε:

- Για τις υπηρεσίες με κατάσταση, το μοντέλο προγραμματισμού αξιόπιστων υπηρεσιών σάς επιτρέπει να με συνέπεια και αξιόπιστα αποθηκεύετε το δικαίωμά σας κατάσταση εντός της υπηρεσίας, χρησιμοποιώντας αξιόπιστη συλλογών. Αυτό είναι ένα απλό σύνολο κλάσεων ιδιαίτερα διαθέσιμη συλλογής που θα είναι οικεία για οποιοδήποτε άτομο έχει χρησιμοποιήσει συλλογές C#. Παραδοσιακά, υπηρεσίες χρειάζεται εξωτερικά συστήματα για τη Διαχείριση αξιόπιστη κατάσταση. Με αξιόπιστη συλλογές, μπορείτε να αποθηκεύσετε την κατάσταση δίπλα του υπολογισμού με την ίδια υψηλή διαθεσιμότητα και την αξιοπιστία έχετε έρθει πρέπει να περιμένουν από ιδιαίτερα διαθέσιμα καταστήματα εξωτερικών και με τις βελτιώσεις επιπλέον λανθάνων χρόνος που από κοινού κατά τον εντοπισμό του υπολογισμού και κατάσταση παρέχουν.

- Ένα απλό μοντέλο για την εκτέλεση του δικού σας κώδικα που έχει προγραμματισμού που χρησιμοποιούνται για μοντέλα. Τον κωδικό έχει ένα σημείο εισόδου ευδιάκριτο και εύκολα διαχειριζόμενου κύκλου ζωής.

- Ένα μοντέλο επικοινωνίας με δυνατότητα σύνδεσης. Χρησιμοποιήστε τη μεταφορά της επιλογής σας, όπως HTTP με [Το API Web](service-fabric-reliable-services-communication-webapi.md), WebSockets, προσαρμοσμένα πρωτόκολλα TCP, κ.λπ. Αξιόπιστη υπηρεσίες παρέχουν κάποια εξαιρετικές επιλογές εκτός του πλαισίου που μπορείτε να χρησιμοποιήσετε ή μπορείτε να παρέχετε τις δικές σας.

## <a name="what-makes-reliable-services-different"></a>Τι κάνει διαφορετικές αξιόπιστων υπηρεσιών;
Αξιόπιστη υπηρεσιών σε ύφασμα υπηρεσίας είναι διαφορετική από τις υπηρεσίες που ενδέχεται να έχετε γράψει πριν από. Υπηρεσία ύφασμα παρέχει αξιοπιστία, διαθεσιμότητα, συνέπειας και κλιμάκωση.  

- **Αξιοπιστία**--υπηρεσία σας θα παραμείνει του ακόμη και σε αναξιόπιστες περιβάλλοντα όπου οι υπολογιστές σας ενδέχεται να αποτύχει ή να πατήσετε ζητημάτων δικτύου.

- **Διαθεσιμότητα**--υπηρεσία σας θα είναι δυνατή η πρόσβαση και να αποκρίνεται. (Αυτό δεν σημαίνει ότι δεν μπορείτε να έχετε τις υπηρεσίες που δεν είναι δυνατή η εύρεση ή Μετάβαση στη σελίδα από εκτός.)

- **Κλιμάκωση**--υπηρεσίες είναι αποσυνδεδεμένη από συγκεκριμένο υλικό και να μεγέθυνση ή σμίκρυνση ανάλογα με τις ανάγκες μέσω της προσθήκης ή κατάργησης του υλικού ή εικονικοί πόροι. Υπηρεσίες εύκολα διαμερίσματα (ιδιαίτερα στην περίπτωση κατάστασης) να εξασφαλίσετε ότι ανεξάρτητο τμήματα της υπηρεσίας μπορεί να κλιμακωθεί και απάντηση σε αποτυχίες ανεξάρτητα. Τέλος, υπηρεσία ύφασμα ενθαρρύνει τη υπηρεσιών να είναι πιο απλές, επιτρέποντας χιλιάδες υπηρεσίες προμήθεια με μια απλή διαδικασία, και όχι που απαιτεί ή διαθέσετε ολόκληρο παρουσίες του λειτουργικού Συστήματος με μια μεμονωμένη παρουσία από ένα συγκεκριμένο φόρτο εργασίας.

- Μπορεί να είναι εγγυημένη **συνέπειας**--τις πληροφορίες που είναι αποθηκευμένες σε αυτήν την υπηρεσία για να είναι συνεπείς (ισχύει μόνο για τις υπηρεσίες με κατάσταση - περισσότερα σε αυτό αργότερα)

## <a name="service-lifecycle"></a>Κύκλος ζωής υπηρεσίας
Εάν η υπηρεσία είναι κατάστασης ή χωρίς κατάσταση, αξιόπιστη υπηρεσίες παρέχουν μια απλή κύκλου ζωής που σας επιτρέπει να γρήγορα συνδέστε τον κωδικό και γρήγορα αποτελέσματα.  Υπάρχουν μόνο μία ή δύο μεθόδους που χρειάζεστε για την υλοποίηση για να λάβετε την υπηρεσία λειτουργία.

- **CreateServiceReplicaListeners/CreateServiceInstanceListeners** - αυτό είναι όπου η υπηρεσία ορίζει στη στοίβα επικοινωνίας που θέλει να χρησιμοποιήσετε. Στη στοίβα επικοινωνίας, όπως [Το API Web](service-fabric-reliable-services-communication-webapi.md), είναι τι Καθορίζει το τελικού σημείου ακρόασης ή τελικά σημεία για την υπηρεσία (πώς προγράμματα-πελάτες θα φτάνουν το). Καθορίζει επίσης πώς τα μηνύματα που εμφανίζονται καταλήγουν αλληλεπίδραση με το υπόλοιπο του κώδικα υπηρεσίας.

- **RunAsync** - αυτό είναι όπου η υπηρεσία σας εκτελείται το επιχειρηματικής λογικής. Το διακριτικό ακύρωσης που παρέχεται είναι ένα σήμα για το πότε θα πρέπει να διακόψετε την που λειτουργούν. Για παράδειγμα, εάν έχετε μια υπηρεσία που πρέπει να αποσπάσετε μηνύματα από μια αξιόπιστη ουρά και να τις επεξεργαστείτε συνεχώς, αυτό είναι όπου θα συμβεί που λειτουργούν.

### <a name="service-startup"></a>Εκκίνηση της υπηρεσίας

Τα κύρια συμβάντα στο τον κύκλο ζωής των μια αξιόπιστη υπηρεσία είναι:

1. Έχει συνταχθεί του αντικειμένου υπηρεσίας (το πράγμα που προέρχεται από την υπηρεσία χωρίς κατάσταση ή κατάστασης).

2. Το `CreateServiceReplicaListeners` / `CreateServiceInstanceListeners` καλείται μέθοδος, παρέχοντας την υπηρεσία ευκαιρία για να λάβετε ένα ή περισσότερα προγράμματα ακρόασης επικοινωνίας της επιλογής.
  - Σημειώστε ότι αυτό είναι προαιρετικό, παρόλο που οι περισσότερες υπηρεσίες θα εκθέσει απευθείας ορισμένες τελικού σημείου.

3. Όταν δημιουργούνται τα ακροατών επικοινωνίας, ανοίγει.
  - Επικοινωνία ακροατών έχουν μια μέθοδο που ονομάζεται `OpenAsync()`, που ονομάζεται σε αυτό το σημείο και που επιστρέφει η διεύθυνση ακρόασης για την υπηρεσία. Εάν η υπηρεσία αξιόπιστη σας χρησιμοποιεί ένα από τα ενσωματωμένα ICommunicationListeners, αυτό γίνεται για εσάς.

4. Μόλις το ακροατήριο επικοινωνίας είναι ανοιχτή, το `RunAsync()` μέθοδος υπηρεσία του κύριου καλείται.
  - Λάβετε υπόψη ότι `RunAsync()` είναι προαιρετικό. Εάν η υπηρεσία υποστηρίζει όλες τις εργασίες απευθείας σε απόκριση χρήστη καλεί μόνο, δεν χρειάζεται για να υλοποιήσετε `RunAsync()`.

### <a name="service-shutdown"></a>Τερματισμός της υπηρεσίας

Όταν η υπηρεσία τερματίζεται (να διαγραφεί, αναβαθμισμένο ή έχει μετακινηθεί) κατοπτρίζεται τη σειρά κλήσης: πρώτα, κατέχει το διακριτικό ακύρωσης `RunAsync()` ακυρώνεται; στη συνέχεια, `CloseAsync()` καλείται το ακροατών επικοινωνίας.

Υπάρχουν μερικά σημαντικά πράγματα που πρέπει να λάβετε υπόψη σχετικά με τον τερματισμό για τις υπηρεσίες με κατάσταση:

- Υπηρεσία ύφασμα δεν θα προωθήσετε μια άλλη ρεπλίκα της υπηρεσίας σας στην κύρια κατάσταση μέχρι `CloseAsync` και `RunAsync` έχουν επιστραφεί. Εάν χρησιμοποιείτε μια ενσωματωμένη επικοινωνίας παρακολούθηση, την `CloseAsync` γίνεται μέθοδο για εσάς.

- Ενώ δεν υπάρχει κανένα χρονικό όριο σχετικά με την επαναφορά από τις παρακάτω μεθόδους, αμέσως χάσετε τη δυνατότητα εγγραφής σε αξιόπιστη συλλογές και, επομένως, δεν μπορεί να ολοκληρωθεί οποιαδήποτε πραγματική εργασία. Συνιστάται η επιστροφή όσο το δυνατόν κατά τη λήψη της αίτησης ακύρωσης.

## <a name="example-services"></a>Παράδειγμα υπηρεσιών
Εάν γνωρίζετε το μοντέλο προγραμματισμού, ρίξουμε μια γρήγορη ματιά στο δύο διαφορετικές υπηρεσίες του για να δείτε πώς αυτά τα τμήματα συνδυάζονται μεταξύ τους.

### <a name="stateless-reliable-services"></a>Χωρίς κατάσταση αξιόπιστων υπηρεσιών
Χωρίς κατάσταση υπηρεσίας είναι ένα σημείο όπου ως έχουν είναι μέλη δεν διατηρούνται κατά την υπηρεσία ή την κατάσταση που υπάρχει είναι διαθέσιμο εξ ολοκλήρου και δεν απαιτεί συγχρονισμού, αναπαραγωγής, διατήρησης ή υψηλής διαθεσιμότητας.

Για παράδειγμα, εξετάστε το ενδεχόμενο έναν υπολογιστή που δεν διαθέτει μνήμη και λαμβάνει όλους τους όρους και λειτουργιών, για να εκτελέσετε ταυτόχρονα.

Σε αυτήν την περίπτωση, η RunAsync() της υπηρεσίας μπορεί να είναι κενή, εφόσον δεν υπάρχει καμία εργασίας-επεξεργασία στο παρασκήνιο που χρειάζεται η υπηρεσία για να το κάνετε. Όταν δημιουργείται η υπηρεσία Αριθμομηχανή, θα επιστρέψει μια ICommunicationListener (για παράδειγμα [Το API Web](service-fabric-reliable-services-communication-webapi.md)) που ανοίγει ένα τελικό σημείο ακρόασης σε ορισμένες θύρα. Αυτό το τελικό σημείο ακρόασης θα σύνδεση με τις διάφορες μεθόδους (παράδειγμα: "Προσθήκη (n1, n2)") που ορίζουν δημόσια API της Αριθμομηχανής.

Όταν γίνεται μια κλήση από ένα πρόγραμμα-πελάτη, την κατάλληλη μέθοδο ενεργοποιείται και την υπηρεσία Αριθμομηχανή εκτελεί τις λειτουργίες τα δεδομένα που παρέχονται και επιστρέφει το αποτέλεσμα. Δεν μπορεί να αποθηκεύσει κάθε μέλος.

Δεν αποθηκεύει οποιαδήποτε εσωτερική κατάσταση, σε αυτό το παράδειγμα Αριθμομηχανής καθιστά πολύ απλής. Αλλά οι περισσότερες υπηρεσίες δεν είναι πραγματικά χωρίς κατάσταση. Αντί για αυτό, externalize τους κατάσταση ορισμένες άλλες χώρο αποθήκευσης. (Για παράδειγμα, οποιαδήποτε εφαρμογή web που βασίζεται σε διατήρηση κατάσταση περιόδου λειτουργίας σε χώρου αποθήκευσης αντιγράφων ασφαλείας ή cache δεν είναι πλήρως χωρίς κατάσταση.)

Ένα συνηθισμένο παράδειγμα πώς χρησιμοποιούνται χωρίς κατάσταση υπηρεσιών σε ύφασμα υπηρεσίας είναι προσκήνιο που εκθέτει το API δημόσιας για μια εφαρμογή web. Η υπηρεσία προσκηνίου ακρόασης, στη συνέχεια, να κατάστασης υπηρεσιών για να ολοκληρώσετε μια αίτηση χρήστη. Σε αυτήν την περίπτωση, κλήσεις από προγράμματα-πελάτες απευθύνονται σε γνωστά θύρα, όπως 80, όπου ακρόαση της υπηρεσίας χωρίς κατάσταση. Αυτή η υπηρεσία χωρίς κατάσταση λαμβάνει την κλήση και καθορίζει εάν η κλήση είναι αξιόπιστο κατασκευαστή, όπως επίσης και ως ποια υπηρεσία προορίζεται για.  Στη συνέχεια, η υπηρεσία χωρίς κατάσταση προωθεί την κλήση σε τη σωστή διαμερίσματα της κατάστασης υπηρεσίας και περιμένει την απάντηση. Όταν η υπηρεσία χωρίς κατάσταση λαμβάνει μια απάντηση, απαντήσεις προς το αρχικό υπολογιστή-πελάτη.

### <a name="stateful-reliable-services"></a>Κατάσταση αξιόπιστων υπηρεσιών
Κατάσταση υπηρεσίας είναι μια τιμή που πρέπει να έχετε ένα τμήμα της κατάστασης διατηρούνται συνεπή και παρουσίαση για την υπηρεσία συνάρτηση. Εξετάστε το ενδεχόμενο μια υπηρεσία που συνεχώς τύπος υπολογίζει ένα κυλιόμενο μέσο όρο των κάποια τιμή με βάση που λαμβάνει ενημερώσεις. Για να το κάνετε αυτό, αυτό πρέπει να έχετε το τρέχον σύνολο εισερχόμενες προσκλήσεις που χρειάζονται για την διαδικασία, καθώς και ο τρέχων μέσος όρος. Οποιαδήποτε υπηρεσία που ανακτά, επεξεργάζεται και αποθηκεύει τις πληροφορίες σε ένα εξωτερικό χώρο αποθήκευσης (όπως ένα Azure blob ή πίνακα κατάστημα σήμερα) είναι κατάστασης. Διατηρεί μόνο στην κατάσταση στο χώρο αποθήκευσης εξωτερικών κατάσταση.

Οι περισσότερες υπηρεσίες αποθηκεύουν σήμερα τους κατάσταση εξωτερικά, επειδή είναι ο εξωτερικός χώρος αποθήκευσης τι παρέχει αξιοπιστία, διαθεσιμότητα, κλιμάκωση και συνέπειας για το συγκεκριμένο μέλος. Σε ύφασμα υπηρεσίας, κατάστασης υπηρεσιών δεν χρειάζεται να αποθηκεύετε την κατάσταση εξωτερικά; Υπηρεσία ύφασμα αναλαμβάνει αυτές τις απαιτήσεις για τον κωδικό και την κατάσταση της υπηρεσίας.

Ας υποθέσουμε ότι θέλουμε να συντάξετε μια υπηρεσία που λαμβάνει αιτήσεις για μια σειρά από τις μετατροπές που πρέπει να εκτελεστούν σε μια εικόνα και την εικόνα που πρέπει να μετατραπούν.  Για αυτήν την υπηρεσία, αυτό θα επιστρέψει επικοινωνίας ακρόασης (ας υποθέσουμε ότι το API Web) που ανοίγει μια θύρα επικοινωνίας και σας επιτρέπει να σας αρέσει υποβολές μέσω API `ConvertImage(Image i, IList<Conversion> conversions)`. Σε αυτό το API, της υπηρεσίας θα μπορούσε να λαμβάνει τις πληροφορίες και αποθηκεύστε την αίτηση σε μια αξιόπιστη ουρά και, στη συνέχεια, επιστρέψετε ορισμένες διακριτικό στον υπολογιστή-πελάτη, ώστε να αυτό θα μπορούσε να παρακολουθείτε την αίτηση (επειδή οι αιτήσεις μπορεί να χρειαστεί κάποιος χρόνος).

Σε αυτήν την υπηρεσία, μπορεί να είναι πιο σύνθετη RunAsync. Η υπηρεσία θα μπορούσε να έχει ένα βρόχο εντός του RunAsync που χρησιμοποιεί τα αιτήματα από IReliableQueue, εκτελεί τις μετατροπές που παρατίθενται και αποθηκεύει τα αποτελέσματα σε IReliableDictionary, έτσι ώστε όταν ο υπολογιστής-πελάτης πάλι, μπορεί να αποκτήσει τις εικόνες που έχουν μετατραπεί. Για να βεβαιωθείτε ότι ακόμα και αν κάτι αποτύχει η εικόνα δεν έχει χαθεί, αυτή η υπηρεσία αξιόπιστη θα ελκυστική εκτός ουρά, εκτελέστε τις μετατροπές, και να αποθηκεύσετε το αποτέλεσμα σε μια συναλλαγή. Σε αυτήν την περίπτωση, το μήνυμα καταργείται στην πραγματικότητα μόνο από την ουρά και τα αποτελέσματα είναι αποθηκευμένα στο λεξικό αποτέλεσμα όταν ολοκληρώσετε τις μετατροπές. Εάν κάποιο στοιχείο αποτύχει στη μέση (όπως αυτή η παρουσία του κώδικα εκτελείται στον υπολογιστή), η πρόσκληση σε παραμένει στην ουρά σε αναμονή για επεξεργασία ξανά.

Ένα σημείο για να λάβετε υπόψη σχετικά με αυτήν την υπηρεσία είναι ότι φαίνεται σαν μια κανονική υπηρεσία .NET. Η μόνη διαφορά είναι που παρέχονται από την υπηρεσία ύφασμα τις δομές δεδομένων που χρησιμοποιείται (IReliableQueue και IReliableDictionary) και, επομένως πραγματοποιούνται ιδιαίτερα αξιόπιστο, διαθέσιμο και συνεπή.

## <a name="when-to-use-reliable-services-apis"></a>Πότε να χρησιμοποιήσετε αξιόπιστη APIs υπηρεσιών
Εάν οποιοδήποτε από τα εξής χαρακτηρίζουν υπηρεσίας ανάγκες της εφαρμογής σας, στη συνέχεια, θα πρέπει να αξιόπιστη APIs υπηρεσίες:

- Πρέπει να παρέχετε συμπεριφορά της εφαρμογής σε πολλές μονάδες κατάστασης (π.χ., παραγγελίες και ταξινόμηση στοιχείων γραμμής).

- Κατάσταση της εφαρμογής σας μπορεί να διαμορφωθεί ομαλά ως αξιόπιστη λεξικά και ουρές.

- Την κατάσταση πρέπει να είναι ιδιαίτερα διαθέσιμες με την access χαμηλής λανθάνοντος χρόνου.

- Χρειάζεται η εφαρμογή σας για τον έλεγχο της ταυτόχρονης εκτέλεσης ή υποδιαίρεση των λειτουργιών που πραγματοποιήθηκαν σε μία ή περισσότερες συλλογές αξιόπιστη.

- Θέλετε να διαχειριστείτε τις επικοινωνίες ή στοιχεία ελέγχου του συνδυασμού διαμερισμάτων της υπηρεσίας.

- Τον κωδικό ανάγκες περιβάλλον με ελεύθερα νήματα χρόνου εκτέλεσης.

- Η εφαρμογή σας πρέπει να δυναμικά Δημιουργία ή καταστροφή αξιόπιστη λεξικά ή ουρές κατά το χρόνο εκτέλεσης.

- Πρέπει να μέσω προγραμματισμού ελέγχου που παρέχονται από ύφασμα υπηρεσίας δημιουργίας αντιγράφων ασφαλείας και επαναφορά δυνατότητες για την υπηρεσία κατάστασης *.

- Χρειάζεται για να διατηρήσετε το ιστορικό αλλαγών για τις μονάδες κατάσταση * η εφαρμογή σας.

- Θέλετε να αναπτύξετε ή να εκμετάλλευση τρίτου κατασκευαστή-ανεπτυγμένες, προσαρμοσμένη κατάσταση υπηρεσίες παροχής *.

> [AZURE.NOTE] * Δυνατότητες που είναι διαθέσιμες στο SDK γενικής διαθεσιμότητας.


## <a name="next-steps"></a>Επόμενα βήματα
+ [Αξιόπιστη υπηρεσίες γρήγορης εκκίνησης](service-fabric-reliable-services-quick-start.md)
+ [Αξιόπιστη υπηρεσίες χρήση για προχωρημένους](service-fabric-reliable-services-advanced-usage.md)
+ [Το μοντέλο αξιόπιστη παραγόντων προγραμματισμού](service-fabric-reliable-actors-introduction.md)