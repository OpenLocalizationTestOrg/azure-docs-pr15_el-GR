<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure αναζήτησης σε Java | Microsoft Azure | Υπηρεσία αναζήτησης φιλοξενούμενη cloud"
    description="Πώς να δημιουργείτε μια εφαρμογή αναζήτησης φιλοξενούμενη cloud στο Azure χρησιμοποιώντας Java ως τη γλώσσα προγραμματισμού."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-java"></a>Γρήγορα αποτελέσματα με το Azure αναζήτησης σε Java
> [AZURE.SELECTOR]
- [Πύλη](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Μάθετε πώς μπορείτε να δημιουργήσετε μια προσαρμοσμένη εφαρμογή αναζήτησης Java που χρησιμοποιεί Azure αναζήτησης για την εμπειρία αναζήτησης. Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί το [Azure αναζήτησης υπηρεσίας REST API](https://msdn.microsoft.com/library/dn798935.aspx) για να δημιουργήσετε τα αντικείμενα και τις εργασίες που χρησιμοποιούνται σε αυτήν την άσκηση.

Για να εκτελέσετε αυτό το δείγμα, πρέπει να έχετε μια υπηρεσία αναζήτησης Azure, η οποία μπορείτε να εγγραφείτε στην [Πύλη του Azure](https://portal.azure.com). Για οδηγίες βήμα προς βήμα, ανατρέξτε στο θέμα [Δημιουργία μιας υπηρεσίας αναζήτησης Azure στην πύλη](search-create-service-portal.md) .

Χρησιμοποιήσαμε το ακόλουθο λογισμικό για να δημιουργήσετε και να ελέγξετε αυτό το δείγμα:

- [Έκλειψη IDE για τους προγραμματιστές ΕΕ Java](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Φροντίστε να κάνετε λήψη της έκδοσης ΕΕ. Ένα από τα βήματα επαλήθευσης απαιτεί μια δυνατότητα που υπάρχει μόνο σε αυτήν την έκδοση.

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>Σχετικά με τα δεδομένα

Αυτού του δείγματος εφαρμογής χρησιμοποιεί τα δεδομένα από το [Ηνωμένες Πολιτείες γεωλογικές υπηρεσιών (αυτό ΓΊΝΕΤΑΙ)](http://geonames.usgs.gov/domestic/download_data.htm), φιλτραρισμένο στη την κατάσταση της Ροντ Νησί για να μειώσετε το μέγεθος του συνόλου δεδομένων. Θα χρησιμοποιήσουμε αυτά τα δεδομένα για τη δημιουργία μιας εφαρμογής αναζήτησης που επιστρέφει γνωστών σημείων κτίρια όπως νοσοκομεία και σχολεία, καθώς και γεωλογικές δυνατότητες όπως ροές λίμνες και summits.

Σε αυτήν την εφαρμογή, το πρόγραμμα **SearchServlet.java** δημιουργεί και φορτώνει το ευρετήριο χρησιμοποιώντας ένα [ευρετήριο](https://msdn.microsoft.com/library/azure/dn798918.aspx) κατασκευής, ανακτώντας τα φιλτραρισμένα dataset αυτό ΓΊΝΕΤΑΙ από μια δημόσια βάση δεδομένων SQL Azure. Προκαθορισμένες διαπιστευτήρια και πληροφορίες σύνδεσης στην προέλευση δεδομένων με σύνδεση παρέχονται στον κώδικα πρόγραμμα. Όσον αφορά την πρόσβαση σε δεδομένα, χωρίς επιπλέον ρυθμίσεις παραμέτρων είναι απαραίτητο.

> [AZURE.NOTE] Θα σας να εφαρμοστεί ένα φίλτρο σε αυτό το dataset για να παραμείνετε στην περιοχή του ορίου 10.000 εγγράφων τη δωρεάν σειρά τις πληροφορίες τιμολόγησης. Εάν χρησιμοποιείτε το τυπικό επίπεδο, αυτό το όριο δεν ισχύει και μπορείτε να τροποποιήσετε αυτόν τον κωδικό για να χρησιμοποιήσετε ένα μεγαλύτερο σύνολο δεδομένων. Για λεπτομέρειες σχετικά με τη δυνατότητα για κάθε επίπεδο τιμολόγησης, ανατρέξτε στο θέμα [όρια και περιορισμούς](search-limits-quotas-capacity.md).

## <a name="about-the-program-files"></a>Σχετικά με τα αρχεία προγράμματος

Η παρακάτω λίστα περιγράφει τα αρχεία που έχουν σχέση με αυτό το δείγμα.

- Search.JSP: Παρέχει το περιβάλλον εργασίας χρήστη
- SearchServlet.java: Παρέχει μεθόδους (παρόμοια με έναν ελεγκτή στο MVC)
- SearchServiceClient.java: Αιτήσεις λαβές HTTP
- SearchServiceHelper.java: Μια κλάση βοηθητικού προγράμματος που παρέχει στατική μεθόδους
- Document.Java: Παρέχει το μοντέλο δεδομένων
- Config.Properties: ορίζει τη διεύθυνση URL της υπηρεσίας αναζήτησης και api-κλειδί
- Pom.XML: Maven εξάρτησης

<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Βρείτε το όνομα της υπηρεσίας και το api-κλειδί της υπηρεσίας αναζήτησης Azure

Όλες οι κλήσεις REST API σε αναζήτηση Azure απαιτούν που παρέχετε τη διεύθυνση URL της υπηρεσίας και ένα api-κλειδί. 

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com).
2. Στη γραμμή μεταπήδηση, κάντε κλικ στην **υπηρεσία αναζήτησης** σε λίστα όλων των υπηρεσιών Azure αναζήτησης παρέχεται για τη συνδρομή σας.
3. Επιλέξτε την υπηρεσία που θέλετε να χρησιμοποιήσετε.
4. Στον πίνακα εργαλείων των υπηρεσιών, θα δείτε τα πλακίδια για βασικές πληροφορίες καθώς και το εικονίδιο κλειδιού για να αποκτήσετε πρόσβαση τα πλήκτρα διαχείρισης.

    ![][3]

5. Αντιγράψτε τη διεύθυνση URL της υπηρεσίας και ένα κλειδί διαχείρισης. Θα χρειαστείτε τα αργότερα, κατά την προσθήκη τους στο αρχείο **config.properties** .

## <a name="download-the-sample-files"></a>Λήψη τα δείγματα αρχείων

1. Μεταβείτε σε [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) Github.

2. Κάντε κλικ στην επιλογή **Λήψη ZIP**, αποθηκεύστε το αρχείο .zip στο δίσκο και, στη συνέχεια, να εξαγάγετε όλα τα αρχεία που περιέχει. Εξετάστε το ενδεχόμενο εξαγωγή των αρχείων στο χώρο εργασίας σας Java για να σας διευκολύνουν να βρείτε το έργο αργότερα.

3. Τα δείγματα αρχείων είναι μόνο για ανάγνωση. Κάντε δεξί κλικ ιδιότητες φακέλου και καταργήστε το χαρακτηριστικό μόνο για ανάγνωση.

Όλες οι επόμενες αρχείο τροποποιήσεις και εκτέλεση δηλώσεις θα γίνει σε σχέση με τα αρχεία σε αυτόν το φάκελο.  

## <a name="import-project"></a>Εισαγωγή έργου

1. Σε Έκλειψη, επιλέξτε **αρχείο** > **Εισαγωγή** > **Γενικά** > **Υπάρχοντα έργα σε χώρο εργασίας**.

    ![][4]

2. Στην περιοχή **Επιλέξτε ριζικό κατάλογο**, μεταβείτε στο φάκελο που περιέχει τα αρχεία δείγματος. Επιλέξτε το φάκελο που περιέχει το φάκελο .project. Το έργο θα πρέπει να εμφανίζεται στη λίστα **έργα** ως ένα επιλεγμένο στοιχείο.

    ![][12]

3. Κάντε κλικ στο κουμπί **Τέλος**.

4. Χρησιμοποιήστε την **Εξερεύνηση έργου** για να προβάλετε και να επεξεργαστείτε τα αρχεία. Εάν δεν είναι ήδη ανοιχτή, κάντε κλικ στην επιλογή **παράθυρο** > **Εμφάνιση προβολής** > **Εξερεύνηση έργου** ή να χρησιμοποιήσετε τη συντόμευση για να το ανοίξετε.

## <a name="configure-the-service-url-and-api-key"></a>Ρυθμίστε τη διεύθυνση URL της υπηρεσίας και api του κλειδιού

1. Στην **Εξερεύνηση έργου**, κάντε διπλό κλικ **config.properties** για να επεξεργαστείτε τις ρυθμίσεις παραμέτρων που περιέχει το όνομα του διακομιστή και api του κλειδιού.

2. Ανατρέξτε στα βήματα νωρίτερα σε αυτό το άρθρο, όπου βρέθηκε η διεύθυνση URL της υπηρεσίας και api του κλειδιού στην [Πύλη του Azure](https://portal.azure.com), για να λάβετε τις τιμές που θα εισάγετε τώρα σε **config.properties**.

3. Στο **config.properties**, αντικαταστήστε το "Api κλειδί" με το πλήκτρο api της υπηρεσίας. Στη συνέχεια, όνομα υπηρεσίας (το πρώτο στοιχείο της http://servicename.search.windows.net τη διεύθυνση URL) αντικαθιστά "όνομα υπηρεσίας" στο ίδιο αρχείο.

    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Ρύθμιση παραμέτρων των περιβαλλόντων έργου, Δόμηση και χρόνου εκτέλεσης

1. Στο Έκλειψη στην Εξερεύνηση έργου, κάντε δεξί κλικ στο έργο > **Ιδιότητες** > **Όψεις έργου**.

2. Επιλέξτε **λειτουργική μονάδα δυναμικού περιεχομένου Web**, **Java**και **JavaScript**.

    ![][6]

3. Κάντε κλικ στο κουμπί **εφαρμογή**.

4. Επιλογή **παραθύρου** > **Προτιμήσεις** > **Server** > **περιβάλλοντα χρόνου εκτέλεσης** > **Προσθήκη..**.

5. Αναπτύξτε Apache και επιλέξτε την έκδοση του διακομιστή Apache Tomcat εγκαταστήσατε προηγουμένως. Το σύστημα, θα σας εγκατεστημένη έκδοση 8.

    ![][7]

6. Στην επόμενη σελίδα, καθορίστε τον κατάλογο εγκατάστασης Tomcat. Σε υπολογιστή με Windows, αυτό πιθανότατα θα είναι C:\Program Files\Apache λογισμικού Foundation\Tomcat *έκδοση*.

6. Κάντε κλικ στο κουμπί **Τέλος**.

7. Επιλογή **παραθύρου** > **Προτιμήσεις** > **Java** > **εγκατεστημένο JREs** > **Προσθήκη**.

8. Στην **Προσθήκη JRE**, επιλέξτε **Τυπική Εικονική**.

10. Κάντε κλικ στο κουμπί **Επόμενο**.

11. Στον ορισμό JRE, στο JRE αρχική, κάντε κλικ στην επιλογή **καταλόγου**.

12. Περιήγηση σε **Αρχεία προγράμματος** > **Java** και επιλέξτε το JDK εγκαταστήσατε προηγουμένως. Είναι σημαντικό να επιλέξετε το JDK ως JRE.

13. Στο εγκατεστημένο JREs, επιλέξτε το **JDK**. Ρυθμίσεις σας θα πρέπει να είναι παρόμοιο με το παρακάτω στιγμιότυπο οθόνης.

    ![][9]

14. Προαιρετικά, επιλέξτε **παράθυρο** > **Πρόγραμμα περιήγησης Web** > **Του Internet Explorer** για να ανοίξετε την εφαρμογή σε ένα παράθυρο προγράμματος περιήγησης εξωτερικών. Με ένα εξωτερικό πρόγραμμα περιήγησης σάς παρέχει μια καλύτερη εμπειρία εφαρμογής Web.

    ![][8]

Τώρα έχετε ολοκληρώσει τις εργασίες ρύθμισης παραμέτρων. Στη συνέχεια, θα δημιουργήσετε και εκτέλεση του έργου.

## <a name="build-the-project"></a>Δημιουργία έργου

1. Στην Εξερεύνηση έργου, κάντε δεξί κλικ στο όνομα του έργου και επιλέξτε **Εκτέλεση ως** > **Maven Δημιουργία...** για να ρυθμίσετε τις παραμέτρους του έργου.

    ![][10]

8. Στη ρύθμιση παραμέτρων επεξεργασία, σε στόχους, πληκτρολογήστε "καθαρή εγκατάσταση" και, στη συνέχεια, κάντε κλικ στην επιλογή **Εκτέλεση**.

Μηνύματα κατάστασης είναι εξόδου στο παράθυρο κονσόλας. Θα πρέπει να βλέπετε ΔΌΜΗΣΗ ΕΠΙΤΥΧΊΑΣ που υποδεικνύει ότι το έργο που έχει δημιουργηθεί χωρίς σφάλματα.

## <a name="run-the-app"></a>Εκτελέστε την εφαρμογή

Σε αυτό το τελευταίο βήμα, θα μπορείτε να εκτελέσετε την εφαρμογή σε ένα περιβάλλον χρόνου εκτέλεσης του τοπικού διακομιστή.

Εάν δεν έχετε ορίσει ακόμη ένα περιβάλλον χρόνου εκτέλεσης διακομιστή στο Έκλειψη, θα χρειαστεί να κάνετε πρώτα αυτήν την ενέργεια.

1. Στην Εξερεύνηση έργου, αναπτύξτε **WebContent**.

5. Κάντε δεξί κλικ **Search.jsp** > **Εκτέλεση ως** > **εκτελείται στο διακομιστή**. Επιλέξτε το διακομιστή Apache Tomcat και, στη συνέχεια, κάντε κλικ στην επιλογή **Εκτέλεση**.

> [AZURE.TIP] Αν χρησιμοποιήσατε το χώρο εργασίας μη προεπιλεγμένες για να αποθηκεύσετε το έργο σας, θα πρέπει να τροποποιήσετε **Εκτέλεση ρύθμισης παραμέτρων** ώστε να οδηγεί στη θέση έργου για να αποφύγετε σφάλμα εκκίνησης στο διακομιστή. Στην Εξερεύνηση έργου, κάντε δεξί κλικ **Search.jsp** > **Εκτέλεση ως** > **Εκτέλεση ρυθμίσεις παραμέτρων**. Επιλέξτε το διακομιστή Apache Tomcat. Κάντε κλικ στην επιλογή **ορίσματα**. Κάντε κλικ στο **χώρο εργασίας** ή **Το σύστημα αρχείων** για να ορίσετε το φάκελο που περιέχει το έργο.

Όταν εκτελείτε την εφαρμογή, θα πρέπει να βλέπετε ένα παράθυρο προγράμματος περιήγησης, παρέχοντας ένα πλαίσιο αναζήτησης για την εισαγωγή τους όρους.

Περιμένετε περίπου ένα λεπτό πριν κάνετε κλικ για να δώσετε την ώρα υπηρεσίας για να δημιουργήσετε και να φορτώσετε το ευρετήριο **αναζήτησης** . Εάν λάβετε ένα σφάλμα HTTP 404, απλώς πρέπει να περιμένετε λίγο πλέον πριν να δοκιμάσετε ξανά.

## <a name="search-on-usgs-data"></a>Αναζήτηση σε αυτό ΓΊΝΕΤΑΙ δεδομένα

Το σύνολο δεδομένων ΓΊΝΕΤΑΙ περιλαμβάνει τις εγγραφές που έχουν σχέση για να την κατάσταση της Ροντ Νησί. Εάν κάνετε κλικ στο κουμπί **αναζήτησης** σε ένα πλαίσιο αναζήτησης κενή, θα λάβετε τις καλύτερες εγγραφές 50, ποιο είναι το προεπιλεγμένο.

Εισαγάγετε έναν όρο αναζήτησης θα σας δώσει το μηχανισμό αναζήτησης κάτι για να μεταβείτε στο. Δοκιμάστε να εισαγάγετε ένα όνομα τοπικές ρυθμίσεις. "Roger Χατζή" ήταν ο πρώτος ρυθμιστής της Νησί Ροντ. Πολλά πάρκα, κτίρια και σχολεία ονομάζονται μετά από αυτόν.

![][11]

Μπορείτε επίσης να δοκιμάσετε οποιονδήποτε από τους εξής όρους:

- Pawtucket
- Pembroke
- χήνα + κέιπ

## <a name="next-steps"></a>Επόμενα βήματα

Αυτό είναι το πρώτο πρόγραμμα εκμάθησης Azure αναζήτησης που βασίζονται σε Java και του συνόλου δεδομένων ΓΊΝΕΤΑΙ. Με τον καιρό, θα επεκτείνουμε αυτό το πρόγραμμα εκμάθησης για την επίδειξη δυνατότητες επιπλέον αναζήτησης που μπορεί να θέλετε να χρησιμοποιήσετε στο προσαρμοσμένων λύσεων.

Εάν έχετε ήδη κάποιες φόντου στο πλαίσιο Αναζήτηση Azure, μπορείτε να χρησιμοποιήσετε αυτό το δείγμα ως μια springboard για περαιτέρω πειραματισμός, ίσως αυξάνοντας τη [σελίδα αναζήτησης](search-pagination-page-layout.md)ή εφαρμογή [Περιήγηση με συνθήκη](search-faceted-navigation.md). Μπορείτε επίσης να βελτιώσετε κατά τη σελίδα αποτελεσμάτων αναζήτησης, προσθέτοντας καταμετρά και δέσμης για έγγραφα, έτσι ώστε οι χρήστες να ξεφυλλίσετε τα αποτελέσματα.

Νέος χρήστης του Azure αναζήτησης; Συνιστάται να προσπάθεια άλλα προγράμματα εκμάθησης για να αναπτύξετε την κατανόηση του τι μπορείτε να δημιουργήσετε. Επισκεφθείτε μας [σελίδας τεκμηρίωση](https://azure.microsoft.com/documentation/services/search/) για περισσότερους πόρους. Μπορείτε επίσης να προβάλετε τις συνδέσεις σε μας [λίστα προγραμμάτων εκμάθησης και βίντεο](search-video-demo-tutorial-list.md) για να αποκτήσετε πρόσβαση σε περισσότερες πληροφορίες.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png