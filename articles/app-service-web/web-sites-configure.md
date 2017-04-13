<properties 
    pageTitle="Ρύθμιση παραμέτρων εφαρμογών web στο Azure εφαρμογής υπηρεσίας" 
    description="Τρόπος ρύθμισης των παραμέτρων μια εφαρμογή web στις υπηρεσίες εφαρμογή Azure" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>Ρύθμιση παραμέτρων εφαρμογών web στο Azure εφαρμογής υπηρεσίας #

Αυτό το θέμα εξηγεί τον τρόπο ρύθμισης των παραμέτρων μιας εφαρμογής web με την [Πύλη Azure].

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>Ρυθμίσεις εφαρμογής

1. Στην [Πύλη του Azure], ανοίξτε το blade για την εφαρμογή web.
2. Κάντε κλικ στην επιλογή **όλες οι ρυθμίσεις**.
3. Κάντε κλικ στην επιλογή **Ρυθμίσεις εφαρμογής**.

![Ρυθμίσεις εφαρμογής][configure01]

Το blade **Ρυθμίσεις εφαρμογής** έχει ρυθμίσεις ομαδοποιημένα κάτω από διάφορες κατηγορίες.

### <a name="general-settings"></a>Γενικές ρυθμίσεις

**Πλαίσιο εκδόσεις**. Εάν η εφαρμογή σας χρησιμοποιεί οποιοδήποτε αυτά τα πλαίσια, ορίστε τις εξής επιλογές: 

- **.NET Framework**: Ορισμός της έκδοσης .NET framework. 
- **PHP**: Ορισμός έκδοση PHP ή **ΑΠΕΝΕΡΓΟΠΟΊΗΣΗΣ** , για να απενεργοποιήσετε PHP. 
- **Java**: Επιλέξτε την έκδοση Java ή **ΑΠΕΝΕΡΓΟΠΟΙΉΣΤΕ** , για να απενεργοποιήσετε τη Java. Χρησιμοποιήστε την επιλογή **Web κοντέινερ** για να επιλέξετε μεταξύ των εκδόσεων Tomcat και Jetty.
- **Python**: Επιλέξτε το Python έκδοση ή για να απενεργοποιήσετε την Python **ΑΝΕΝΕΡΓΌ** .

Για λόγους τεχνικής, ενεργοποίηση Java για την εφαρμογή σας απενεργοποιεί τις επιλογές .NET, PHP και Python.

<a name="platform"></a>
**Πλατφόρμα**. Επιλογή αν την εφαρμογή web της εκτελείται σε ένα περιβάλλον 32-bit ή 64-bit. Το περιβάλλον 64-bit απαιτεί Basic ή τυπική λειτουργία. Δωρεάν και κοινή χρήση λειτουργιών να εκτελείτε πάντα σε ένα περιβάλλον 32-bit.

**Web Sockets**. Ορισμός **Ενεργοποιηση** για να ενεργοποιήσετε το πρωτόκολλο WebSocket; Για παράδειγμα, εάν η εφαρμογή web της χρησιμοποιεί [ASP.NET SignalR] ή [socket.io].

<a name="alwayson"></a>
**Πάντα σε**. Από προεπιλογή, εφαρμογές web είναι καταργείται αν είναι σε αδράνεια για ορισμένες χρονικό διάστημα. Αυτό σας επιτρέπει να το σύστημα διατήρηση των πόρων. Στη βασική ή τυπική λειτουργία, μπορείτε να ενεργοποιήσετε **Πάντα σε** για να διατηρήσετε την εφαρμογή φορτωμένου πάντα. Εάν την εφαρμογή σας εκτελεί εργασίες συνεχής web, θα πρέπει να ενεργοποιείτε **Πάντα σε**ή τις εργασίες web ενδέχεται να μην λειτουργούν αξιόπιστα.

**Διαχείριση διοχέτευσης έκδοση**. Ορίζει τη των υπηρεσιών IIS [λειτουργία διοχέτευσης]. Αφήστε αυτό το σύνολο σε ολοκληρωμένων (προεπιλογή), εκτός αν έχετε μια εφαρμογή παλαιού τύπου που απαιτεί μια παλαιότερη έκδοση των υπηρεσιών IIS.

**Αυτόματη εναλλαγή**. Εάν ενεργοποιήσετε αυτόματης αντιμετάθεσης για μια υποδοχή ανάπτυξης, εφαρμογής υπηρεσίας θα αυτόματη εναλλαγή της εφαρμογής web σε παραγωγή, όταν πατάτε μια ενημέρωση σε αυτή την υποδοχή. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ανάπτυξη να ενδιάμεσου υποδοχές για τις εφαρμογές web στο Azure εφαρμογής υπηρεσίας] (web-τοποθεσίες-τοποθετούνται-publishing.md).

### <a name="debugging"></a>Εντοπισμός σφαλμάτων

**Ο απομακρυσμένος εντοπισμός σφαλμάτων**. Επιτρέπει ο απομακρυσμένος εντοπισμός σφαλμάτων. Όταν είναι ενεργοποιημένη, μπορείτε να χρησιμοποιήσετε το απομακρυσμένο πρόγραμμα εντοπισμού σφαλμάτων στο Visual Studio για να συνδεθείτε απευθείας με την εφαρμογή web. Ο απομακρυσμένος εντοπισμός σφαλμάτων παραμένουν ενεργοποιημένες για 48 ώρες. 

### <a name="app-settings"></a>Ρυθμίσεις εφαρμογής

Αυτή η ενότητα περιέχει ζεύγη ονόματος/τιμής που έχετε στο web app θα φορτώνεται από το μενού Έναρξη προς τα επάνω. 

- Για τις εφαρμογές .NET, αυτές οι ρυθμίσεις έχουν εισαχθεί σε της ρύθμισης παραμέτρων του .NET `AppSettings` κατά το χρόνο εκτέλεσης, παράκαμψη υπάρχουσες ρυθμίσεις. 

- Εφαρμογές PHP, Python, Java και κόμβου να αποκτήσετε πρόσβαση σε αυτές τις ρυθμίσεις ως μεταβλητές περιβάλλοντος κατά το χρόνο εκτέλεσης. Για κάθε ρύθμιση εφαρμογής, δημιουργούνται δύο μεταβλητές περιβάλλοντος; μία με το όνομα που καθορίζεται από την καταχώρηση τη ρύθμιση της εφαρμογής, καθώς και μια άλλη με πρόθεμα της APPSETTING_. Και τα δύο περιέχουν την ίδια τιμή.

### <a name="connection-strings"></a>Συμβολοσειρές σύνδεσης

Συμβολοσειρές σύνδεσης για τους πόρους που είναι συνδεδεμένες. 

Για τις εφαρμογές .NET, αυτές οι συμβολοσειρές σύνδεσης έχουν εισαχθεί σε της ρύθμισης παραμέτρων του .NET `connectionStrings` ρυθμίσεις κατά το χρόνο εκτέλεσης, παράκαμψη υπάρχουσες καταχωρήσεις όπου το πλήκτρο ισούται με το όνομα συνδεδεμένη βάση δεδομένων. 

Για εφαρμογές PHP, Python, Java και κόμβου, αυτές οι ρυθμίσεις θα είναι διαθέσιμες ως μεταβλητές περιβάλλοντος κατά το χρόνο εκτέλεσης, το πρόθεμα με τον τύπο σύνδεσης. Τα προθέματα μεταβλητή περιβάλλοντος είναι οι εξής: 

- SQL Server:`SQLCONNSTR_`
- MySQL:`MYSQLCONNSTR_`
- Βάση δεδομένων SQL:`SQLAZURECONNSTR_`
- Προσαρμογή:`CUSTOMCONNSTR_`

Για παράδειγμα, εάν μια συμβολοσειρά σύνδεσης MySql έχουν ονομάζεται `connectionstring1`, αυτό θα είναι δυνατή η πρόσβαση μέσω τη μεταβλητή περιβάλλοντος `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Προεπιλεγμένα έγγραφα

Το προεπιλεγμένο έγγραφο είναι η σελίδα web που εμφανίζεται στη ρίζα διεύθυνση URL για μια τοποθεσία Web.  Χρησιμοποιείται το πρώτο αρχείο που ταιριάζει στη λίστα. 

Εφαρμογές Web μπορεί να χρησιμοποιεί λειτουργικές μονάδες που δρομολόγηση που βασίζονται σε διεύθυνση URL, αντί να εξυπηρετεί στατικό περιεχόμενο, οπότε δεν είναι χωρίς προεπιλεγμένο έγγραφο ως τέτοια.    

### <a name="handler-mappings"></a>Πρόγραμμα χειρισμού αντιστοιχίσεων

Χρησιμοποιήστε αυτήν την περιοχή για να προσθέσετε προσαρμοσμένη δέσμη ενεργειών επεξεργαστές χειρισμού των αιτήσεων για το συγκεκριμένο αρχείο επεκτάσεις. 

- **Επέκταση**. Η επέκταση αρχείου προς επεξεργασία, όπως *.php ή handler.fcgi. 
- **Διαδρομή επεξεργαστή δέσμης ενεργειών**. Η απόλυτη διαδρομή του επεξεργαστή δέσμης ενεργειών. Θα γίνεται η επεξεργασία των αιτήσεων για αρχεία που ταιριάζουν με την επέκταση αρχείου από τον επεξεργαστή δέσμης ενεργειών. Χρησιμοποιήστε τη διαδρομή `D:\home\site\wwwroot` για να ανατρέξετε στις ριζικό κατάλογο της εφαρμογής σας.
- **Πρόσθετα ορίσματα**. Προαιρετικά ορίσματα της γραμμής εντολών για τον επεξεργαστή δέσμης ενεργειών 


### <a name="virtual-applications-and-directories"></a>Εικονικές εφαρμογές και καταλόγων 
 
Για να ρυθμίσετε εικονικές εφαρμογές και σε καταλόγους, καθορίστε κάθε εικονικού καταλόγου και η αντίστοιχη φυσική διαδρομή σε σχέση με το ριζικό κατάλογο της τοποθεσίας Web. Προαιρετικά, μπορείτε να επιλέξετε το πλαίσιο ελέγχου **εφαρμογή** για να επισημάνετε ένα εικονικό κατάλογο ως εφαρμογή.


## <a name="enabling-diagnostic-logs"></a>Ενεργοποίηση αρχείων καταγραφής διαγνωστικών

Για να ενεργοποιήσετε αρχεία καταγραφής διαγνωστικών:

1. Στο το blade για την εφαρμογή web σας, κάντε κλικ στην επιλογή **όλες οι ρυθμίσεις**.
2. Κάντε κλικ στην επιλογή **αρχεία καταγραφής διαγνωστικών**. 

Επιλογές για τη σύνταξη αρχείων καταγραφής διαγνωστικών από μια εφαρμογή web που υποστηρίζει καταγραφή: 

- **Καταγραφή εφαρμογής**. Συντάσσει αρχεία καταγραφής εφαρμογών στο σύστημα αρχείων. Καταγραφή διαρκεί για μια περίοδο 12 ώρες. 

**Επίπεδο**. Όταν είναι ενεργοποιημένη η καταγραφή από την εφαρμογή, αυτή η επιλογή καθορίζει την ποσότητα των πληροφοριών που θα καταγραφεί (σφάλμα, προειδοποίηση, πληροφορίες ή λεπτομερής).

**Καταγραφή διακομιστή web**. Αποθήκευση αρχείων καταγραφής σε μορφή αρχείου εκτεταμένης καταγραφής W3C. 

**Μηνύματα σφάλματος λεπτομερείς**. Αρχεία .htm μηνύματα σφάλματος λεπτομερείς αποθηκεύει. Τα αρχεία αποθηκεύονται στην περιοχή /LogFiles/DetailedErrors. 

**Απέτυχε η αίτηση ανίχνευση**. Αρχεία καταγραφής απέτυχε αιτήσεις σε αρχεία XML. Τα αρχεία αποθηκεύονται στην περιοχή /LogFiles/W3SVC*xxx*, όπου xxx είναι ένα μοναδικό αναγνωριστικό. Αυτός ο φάκελος περιέχει ένα αρχείο XSL και ένα ή περισσότερα αρχεία XML. Βεβαιωθείτε ότι κάνετε λήψη του αρχείου XSL, επειδή παρέχει λειτουργικότητα για τη μορφοποίηση και το φιλτράρισμα των περιεχομένων των αρχείων XML.

Για να προβάλετε τα αρχεία καταγραφής, πρέπει να δημιουργήσετε διαπιστευτήρια FTP, ως εξής:

1. Στο το blade για την εφαρμογή web σας, κάντε κλικ στην επιλογή **όλες οι ρυθμίσεις**.
2. Κάντε κλικ στο κουμπί **ανάπτυξης διαπιστευτήρια**.
3. Πληκτρολογήστε ένα όνομα χρήστη και τον κωδικό πρόσβασης.
4. Κάντε κλικ στην επιλογή **Αποθήκευση**.

![Ορισμός διαπιστευτηρίων ανάπτυξης][configure03]

Το πλήρες όνομα χρήστη FTP είναι "app\username" όπου *εφαρμογής* είναι το όνομα της εφαρμογής web. Το όνομα χρήστη εμφανίζεται σε το blade εφαρμογής web, στην περιοχή **βασικά στοιχεία**.  

![Διαπιστευτήρια FTP ανάπτυξης][configure02]

## <a name="other-configuration-tasks"></a>Άλλες εργασίες ρύθμισης παραμέτρων

### <a name="ssl"></a>SSL 

Στη βασική ή τυπική λειτουργία, μπορείτε να αποστείλετε τα πιστοποιητικά SSL για έναν προσαρμοσμένο τομέα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ενεργοποίηση HTTPS για μια εφαρμογή web]. 

Για να προβάλετε τα πιστοποιητικά που έχουν αποσταλεί, κάντε κλικ στην επιλογή **Όλες οι ρυθμίσεις** > **προσαρμοσμένων τομέων και SSL**.

### <a name="domain-names"></a>Ονόματα τομέων

Προσθέστε προσαρμοσμένα ονόματα τομέα για την εφαρμογή web σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ρύθμιση παραμέτρων ενός προσαρμοσμένου ονόματος τομέα για μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας].

Για να προβάλετε σας ονόματα τομέων, κάντε κλικ στην επιλογή **Όλες οι ρυθμίσεις** > **προσαρμοσμένων τομέων και SSL**.

### <a name="deployments"></a>Αναπτύξεις

- Ρύθμιση του συνεχούς ανάπτυξης. Ανατρέξτε στο θέμα [Χρήση Git για την ανάπτυξη εφαρμογών Web στο Azure εφαρμογής υπηρεσίας](./web-sites-deploy.md).
- Ανάπτυξη υποδοχές. Ανατρέξτε στο θέμα [Ανάπτυξη περιβάλλοντα ενδιάμεσου σταδίου για τις εφαρμογές Web στο Azure εφαρμογής υπηρεσίας].


Για να προβάλετε τις υποδοχές ανάπτυξης, κάντε κλικ στην επιλογή **Όλες οι ρυθμίσεις** > **υποδοχές ανάπτυξης**.

### <a name="monitoring"></a>Παρακολούθηση

Στη βασική ή τυπική λειτουργία, μπορείτε να ελέγξετε τη διαθεσιμότητα των HTTP ή HTTPS τελικά σημεία από έως και τρεις θέσεις κατανεμημένο παν. Δοκιμασία παρακολούθησης αποτυγχάνει εάν ο κώδικας απόκρισης HTTP είναι σφάλμα (4xx ή 5xx) ή την απάντηση διαρκεί περισσότερο από 30 δευτερόλεπτα. Ένα τελικό σημείο θεωρείται διαθέσιμο Εάν ολοκληρωθεί με επιτυχία τους ελέγχους που είναι παρακολούθησης από όλες τις καθορισμένες θέσεις. 

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος: παρακολούθηση της κατάστασης τελικό σημείο web].

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας], όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή web μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

## <a name="next-steps"></a>Επόμενα βήματα

- [Ρύθμιση των παραμέτρων ενός προσαρμοσμένου ονόματος τομέα στο Azure εφαρμογής υπηρεσίας]
- [Ενεργοποίηση HTTPS για μια εφαρμογή στο Azure εφαρμογής υπηρεσίας]
- [Κλίμακα μια εφαρμογή web Azure εφαρμογής υπηρεσίας]
- [Παρακολούθηση βασικά στοιχεία για τις εφαρμογές Web στο Azure εφαρμογής υπηρεσίας]

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[Πύλη του Azure]: https://portal.azure.com/
[Ρύθμιση των παραμέτρων ενός προσαρμοσμένου ονόματος τομέα στο Azure εφαρμογής υπηρεσίας]: ./web-sites-custom-domain-name.md
[Ανάπτυξη περιβάλλοντα ενδιάμεσου σταδίου για τις εφαρμογές Web στο Azure εφαρμογής υπηρεσίας]: ./web-sites-staged-publishing.md
[Ενεργοποίηση HTTPS για μια εφαρμογή στο Azure εφαρμογής υπηρεσίας]: ./web-sites-configure-ssl-certificate.md
[Πώς μπορείτε να: παρακολούθηση της κατάστασης τελικό σημείο web]: http://go.microsoft.com/fwLink/?LinkID=279906
[Παρακολούθηση βασικά στοιχεία για τις εφαρμογές Web στο Azure εφαρμογής υπηρεσίας]: ./web-sites-monitor.md
[λειτουργία διοχέτευσης]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Κλίμακα μια εφαρμογή web Azure εφαρμογής υπηρεσίας]: ./web-sites-scale.md
[socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Δοκιμάστε εφαρμογής υπηρεσίας]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png