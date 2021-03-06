<properties
    pageTitle="Συνεχής παράδοσης με Git και Visual Studio Team Services στο Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους τα έργα σας Visual Studio Team Services ομάδας για να χρησιμοποιήσετε Git για την αυτόματη δημιουργία και ανάπτυξη της δυνατότητας εφαρμογής Web στις υπηρεσίες του Azure εφαρμογής υπηρεσίας ή στο cloud."
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Συνεχής παράδοσης για χρήση του Visual Studio Team Services και Git Azure

Μπορείτε να χρησιμοποιήσετε τα έργα Visual Studio Team Services ομάδας για τη φιλοξενία ένα αποθετήριο Git για τον πηγαίο κώδικα, και αυτόματα δημιουργία και ανάπτυξη εφαρμογών Azure web ή κάθε φορά που πατάτε μια ολοκλήρωση στο αποθετήριο δεδομένων στο cloud services.

Θα χρειαστείτε Visual Studio 2013 και το SDK Azure εγκατεστημένο. Εάν δεν έχετε ήδη Visual Studio 2013, κάντε λήψη του, επιλέγοντας τη σύνδεση **Γρήγορα αποτελέσματα δωρεάν** στο [www.visualstudio.com](http://www.visualstudio.com). Εγκαταστήστε το SDK Azure από [εδώ](http://go.microsoft.com/fwlink/?LinkId=239540).


> [AZURE.NOTE]Χρειάζεστε ένα λογαριασμό του Visual Studio Team Services για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης: μπορείτε να [ανοίξετε ένα λογαριασμό του Visual Studio Team Services δωρεάν](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Για να ρυθμίσετε μια υπηρεσία στο cloud για την αυτόματη δημιουργία και ανάπτυξη Azure χρησιμοποιώντας τις υπηρεσίες ομάδας Visual Studio, ακολουθήστε τα παρακάτω βήματα.

## <a name="1-create-a-git-repository"></a>1: Δημιουργήστε ένα αποθετήριο Git

1. Εάν δεν έχετε ήδη ένα λογαριασμό του Visual Studio Team Services, μπορείτε να αποκτήσετε ένα [εδώ](http://go.microsoft.com/fwlink/?LinkId=397665). Όταν δημιουργείτε το έργο σας ομάδας, επιλέξτε Git ως σύστημα προέλευσης στοιχείου ελέγχου. Ακολουθήστε τις οδηγίες για να συνδεθείτε Visual Studio στο έργο σας ομάδας.

2. Στην **Εξερεύνηση ομάδας**, επιλέξτε τη σύνδεση **κλωνοποίηση αυτό το αποθετήριο δεδομένων** .

    ![][3]

3. Καθορίστε τη θέση του τοπικού αντιγράφου και, στη συνέχεια, επιλέξτε το κουμπί **αντιγραφής** .

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: Δημιουργία έργου και το δεσμεύσετε στο αποθετήριο δεδομένων

1. Στην **Εξερεύνηση ομάδας**, στην ενότητα **λύσεων** , επιλέξτε τη σύνδεση **Δημιουργία** για να δημιουργήσετε ένα νέο έργο στο τοπικό αποθετήριο.

    ![][4]

2. Μπορείτε να αναπτύξετε μια εφαρμογή web ή μια υπηρεσία cloud (εφαρμογή Azure), ακολουθώντας τα βήματα σε αυτές τις οδηγίες. Δημιουργήστε ένα νέο έργο υπηρεσία Cloud Azure ή ένα νέο έργο ASP.NET MVC. Βεβαιωθείτε ότι το έργο ως προορισμό το 4 .NET Framework ή νεότερη έκδοση. Εάν θέλετε να δημιουργήσετε ένα έργο υπηρεσία cloud, προσθέστε ένα ρόλο web ASP.NET MVC και ένα ρόλο εργασίας.
Εάν θέλετε να δημιουργήσετε μια εφαρμογή web, επιλέξτε το πρότυπο έργου **Εφαρμογής Web ASP.NET** και, στη συνέχεια, επιλέξτε **MVC**. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία εφαρμογής web ASP.NET στο Azure εφαρμογής υπηρεσίας](../app-service-web/web-sites-dotnet-get-started.md) .

3. Άνοιγμα του μενού συντόμευσης για τη λύση και επιλέξτε **Ολοκλήρωση**.

    ![][7]

4. Εάν αυτή είναι η πρώτη φορά που έχετε χρησιμοποιήσει Git στο Visual Studio Team Services, θα πρέπει να παρέχουν ορισμένες πληροφορίες για να προσδιορίσετε τον εαυτό σας στον Git. Στην περιοχή **Αλλαγές σε εκκρεμότητα** της **Εξερεύνησης ομάδας**, πληκτρολογήστε το όνομα χρήστη διεύθυνση ηλεκτρονικού ταχυδρομείου. Πληκτρολογήστε ένα σχόλιο για την υποβολή και, στη συνέχεια, επιλέξτε το κουμπί " **Υποβολή** ".

    ![][8]

5. Παρατηρήστε τις επιλογές για να συμπεριλάβετε ή να εξαιρέσετε συγκεκριμένες αλλαγές κατά τη μεταβίβαση ελέγχου. Εάν εξαιρεθούν τις αλλαγές που θέλετε, επιλέξτε **Συμπερίληψη όλων**.

6. Τώρα που έχετε δεσμευμένου τις αλλαγές στο τοπικό αντίγραφο του αποθετηρίου. Στη συνέχεια, μπορείτε να συγχρονίσετε αυτές τις αλλαγές με το διακομιστή, επιλέγοντας τη σύνδεση **συγχρονισμού** .

## <a name="3-connect-the-project-to-azure"></a>3: σύνδεση του έργου με Azure

1. Τώρα που έχετε ένα αποθετήριο Git στο Visual Studio Team Services με ορισμένες πηγαίου κώδικα σε αυτό, είστε έτοιμοι να συνδεθείτε σας αποθετήριο git Azure.  Στην [πύλη του Azure κλασική](http://go.microsoft.com/fwlink/?LinkID=213885), επιλέξτε την εφαρμογή υπηρεσίας ή web cloud ή δημιουργήστε ένα νέο, επιλέγοντας το εικονίδιο στην κάτω αριστερά και επιλέγοντας **Μια υπηρεσία Cloud** ή **Web App** και, στη συνέχεια, **Γρήγορης δημιουργίας**+.

    ![][9]

3. Για τις υπηρεσίες cloud, επιλέξτε τη σύνδεση **Ρύθμιση δημοσίευσης με τις υπηρεσίες ομάδας του Visual Studio** . Για τις εφαρμογές web, επιλέξτε τη σύνδεση **Ρύθμιση ανάπτυξης από το στοιχείο ελέγχου προέλευσης** .

    ![][10]

2. Στον οδηγό, πληκτρολογήστε το όνομα του λογαριασμού σας Visual Studio Team Services στο πλαίσιο κειμένου και επιλέξτε τη σύνδεση **Εγκρίνετε τώρα** . Ίσως σας ζητηθεί να εισέλθετε.

    ![][11]

3. Στην **Αίτηση σύνδεσης** αναδυόμενο παράθυρο, επιλέξτε **Αποδοχή** για να εξουσιοδοτήσετε Azure για ρύθμιση παραμέτρων του έργου σας ομάδας στο Visual Studio Team Services.

    ![][12]

4. Όταν ολοκληρωθεί με επιτυχία εξουσιοδότησης, μπορείτε να δείτε μια αναπτυσσόμενη λίστα που περιέχει τα έργα σας Visual Studio Team Services ομάδας.  Επιλέξτε το όνομα της ομάδας έργου που δημιουργήσατε στα προηγούμενα βήματα και επιλέξτε το κουμπί σημάδι ελέγχου του οδηγού.

    ![][13]

    Την επόμενη φορά που πατάτε μια ολοκλήρωση στο αποθετήριο δεδομένων σας, θα Δημιουργήστε και αναπτύξτε το έργο σας να Azure Visual Studio Team Services.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: έναυσμα ένα αναδόμηση και αναπτύξτε ξανά το έργο σας

1. Στο Visual Studio, ανοίξτε το αρχείο και να το αλλάξετε. Για παράδειγμα, να αλλάξετε το αρχείο `_Layout.cshtml` κάτω από τις προβολές\\κοινόχρηστου φακέλου σε ένα ρόλο MVC web.

    ![][17]

2. Επεξεργαστείτε το κείμενο υποσέλιδου για την τοποθεσία και αποθηκεύστε το αρχείο.

    ![][18]

3. Στην **Εξερεύνηση λύσεων**, ανοίξτε το μενού συντόμευσης για τον κόμβο λύση, τον κόμβο του έργου ή το αρχείο που έχετε αλλάξει και, στη συνέχεια, επιλέξτε **Ολοκλήρωση**.

4. Πληκτρολογήστε ένα σχόλιο και επιλέξτε **Ολοκλήρωση**.

    ![][20]

5. Επιλέξτε τη σύνδεση **συγχρονισμού** .

    ![][38]

6. Επιλέξτε τη σύνδεση **Push** για να προωθήσετε την ολοκλήρωση του αποθετηρίου στο Visual Studio Team Services. (Μπορείτε επίσης να χρησιμοποιήσετε το κουμπί **Συγχρονισμός** για να αντιγράψετε τις δεσμεύσεις του αποθετηρίου. Η διαφορά είναι ότι **ο συγχρονισμός** συγκεντρώνει επίσης τις πιο πρόσφατες αλλαγές από το αρχείο φύλαξης.)

    ![][39]

7. Επιλέξτε το κουμπί **αρχική** για να επιστρέψετε στην αρχική σελίδα του **Εξερεύνηση ομάδας** .

    ![][21]

8. Επιλέξτε **δημιουργεί** για να προβάλετε το εκδόσεις σε εξέλιξη.

    ![][22]

    **Εξερεύνηση ομάδας** εμφανίζει ότι ένα build ενεργοποιήθηκε για σας μεταβίβαση ελέγχου.

    ![][23]

9. Για να προβάλετε ένα αρχείο καταγραφής λεπτομερείς καθώς εξελίσσεται η δημιουργία, κάντε διπλό κλικ στο όνομα του Δημιουργία σε εξέλιξη.

10. Ενώ η Δόμηση βρίσκεται σε εξέλιξη, Ρίξτε μια ματιά στον ορισμό Δόμηση που δημιουργήθηκε όταν χρησιμοποιήσατε τον οδηγό για να συνδέσετε με Azure.  Ανοίξτε το μενού συντόμευσης για τον ορισμό Δόμηση και επιλέξτε **Επεξεργασία ορισμού δημιουργία**.

    ![][25]

11. Στην καρτέλα **έναυσμα** , θα δείτε ότι ο ορισμός Δόμηση έχει οριστεί για να δημιουργήσετε σε κάθε μεταβίβασης ελέγχου, από προεπιλογή. (Για μια υπηρεσία cloud, Visual Studio Team Services δημιουργεί και αναπτύσσει το πρωτότυπο κλάδο για το περιβάλλον δημιουργίας σταδίων αυτόματα. Εξακολουθείτε να έχετε για να κάνετε μια μη αυτόματη βήμα για την ανάπτυξη στην ενεργή τοποθεσία. Για μια εφαρμογή web που δεν έχει ενδιάμεσου περιβάλλον, αναπτύσσει το πρωτότυπο κλάδο απευθείας στην τοποθεσία του live.

    ![][26]

1. Στην καρτέλα **διεργασία** , μπορείτε να δείτε το περιβάλλον ανάπτυξης έχει οριστεί στο όνομα της εφαρμογής υπηρεσίας ή web cloud.

    ![][27]

1. Εάν θέλετε διαφορετικές τιμές από τις προεπιλογές, καθορίστε τιμές για τις ιδιότητες. Οι ιδιότητες για τη δημοσίευση Azure είναι στην ενότητα **ανάπτυξης** και ίσως χρειαστεί επίσης να ορίσετε τις παραμέτρους MSBuild. Για παράδειγμα, σε ένα σύννεφο project υπηρεσίας, για να καθορίσετε μια ρύθμιση παραμέτρων της υπηρεσίας εκτός από το "Cloud", ορίστε τις παραμέτρους MSbuild για να `/p:TargetProfile=[YourProfile]` όπου *[YourProfile]* ταιριάζει με ένα αρχείο ρύθμισης παραμέτρων υπηρεσίας με όνομα όπως ServiceConfiguration. *YourProfile*.cscfg.

    Ο παρακάτω πίνακας παρουσιάζει τις διαθέσιμες ιδιότητες στην ενότητα **Ανάπτυξη** :

  	|Ιδιότητα|Προεπιλεγμένη τιμή|
  	|---|---|
  	|Να επιτρέπεται μη αξιόπιστα πιστοποιητικά|Εάν είναι false, πιστοποιητικά SSL πρέπει να είναι υπογεγραμμένα από μια αρχή έκδοσης πιστοποιητικών ρίζας.|
  	|Να επιτραπεί η αναβάθμιση|Επιτρέπει την ανάπτυξη για να ενημερώσετε μια υπάρχουσα ανάπτυξη αντί να δημιουργήσετε ένα νέο. Διατηρεί τη διεύθυνση IP.|
  	|Μην διαγράφετε|Εάν είναι αληθές, χωρίς αντικατάσταση μιας υπάρχουσας μη σχετιζόμενες ανάπτυξης (επιτρέπεται αναβάθμιση).|
  	|Διαδρομή προς ρυθμίσεις ανάπτυξης|Η διαδρομή προς το αρχείο .pubxml για μια εφαρμογή web, σε σχέση με στον ριζικό φάκελο του repo. Παραβλέπεται για τις υπηρεσίες cloud.|
  	|Περιβάλλον ανάπτυξης του SharePoint|Το ίδιο με το όνομα της υπηρεσίας.|
  	|Περιβάλλον ανάπτυξης του Azure|Το web app ή στο cloud όνομα της υπηρεσίας.|

1. Με αυτήν τη στιγμή, σας δόμηση θα πρέπει να ολοκληρωθεί με επιτυχία.

    ![][28]

1. Εάν κάνετε διπλό κλικ στο όνομα Δόμηση, Visual Studio εμφανίζει μια **Σύνοψη Δόμηση**, συμπεριλαμβανομένων τυχόν αποτελέσματα δοκιμής από σχετικές μονάδα δοκιμής έργα.

    ![][29]

1. Στην [πύλη του Azure κλασική](http://go.microsoft.com/fwlink/?LinkID=213885), μπορείτε να προβάλετε το συσχετισμένο ανάπτυξης στην καρτέλα **αναπτύξεις** όταν είναι επιλεγμένο το περιβάλλον δημιουργίας σταδίων.

    ![][30]

1.  Μεταβείτε στη διεύθυνση URL της τοποθεσίας σας. Για μια εφαρμογή web, επιλέξτε μόνο το κουμπί **Αναζήτηση** στην πύλη. Για μια υπηρεσία cloud, επιλέξτε τη διεύθυνση URL στην ενότητα **Γρήγορη Glance** της σελίδας **πίνακα εργαλείων** που εμφανίζει το περιβάλλον ενδιάμεσου σταδίου.

    Αναπτύξεις από συνεχής ενοποίησης για τις υπηρεσίες cloud δημοσιεύονται στο περιβάλλον ενδιάμεσου σταδίου από προεπιλογή. Μπορείτε να το αλλάξετε, ορίζοντας την ιδιότητα **Εναλλακτικό περιβάλλον υπηρεσία Cloud** **παραγωγή**. Παρακάτω θα δείτε πού βρίσκεται η διεύθυνση URL της τοποθεσίας στη σελίδα πίνακα εργαλείων την υπηρεσία cloud.

    ![][31]

    Θα ανοίξει μια νέα καρτέλα του προγράμματος περιήγησης για να αποκαλύψετε εκτελείται την τοποθεσία σας.

    ![][32]

1.  Εάν κάνετε άλλες αλλαγές στο έργο σας, έναυσμα που δημιουργεί πιο και θα συγκεντρώσουν πολλές αναπτύξεις. Η πιο πρόσφατη μία έχει επισημανθεί ως ενεργό.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Αναπτύξτε ξανά μια παλιότερη έκδοση

Αυτό το βήμα είναι προαιρετικό. Στην κλασική Azure πύλη, επιλέξτε μια προηγούμενη ανάπτυξη και επιλέξτε **αναπτύξτε ξανά** για να επαναφέρετε την τοποθεσία σας σε μια προηγούμενη μεταβίβαση ελέγχου. Σημειώστε ότι αυτό θα ενεργοποιήσετε εκ νέου δημιουργία στο TFS και να δημιουργήσετε μια νέα καταχώρηση στο ιστορικό ανάπτυξης.

![][34]

## <a name="6-change-the-production-deployment"></a>6: αλλαγή της ανάπτυξης της παραγωγής

Όταν είστε έτοιμοι, μπορείτε να προβιβάσετε το περιβάλλον ενδιάμεσου σταδίου στο περιβάλλον παραγωγής, επιλέγοντας **Εναλλαγή** στην πύλη του Azure κλασική. Το περιβάλλον που μόλις ανεπτυγμένος ενδιάμεσου σταδίου προωθείται στο παραγωγής και το προηγούμενο περιβάλλον παραγωγής, αν υπάρχουν, γίνεται ένα περιβάλλον ενδιάμεσου σταδίου. Η ενεργή ανάπτυξη ενδέχεται να είναι διαφορετικές για την παραγωγή και ενδιάμεσου περιβάλλοντα, αλλά το ιστορικό ανάπτυξης του πρόσφατες εκδόσεις είναι η ίδια, ανεξάρτητα από το περιβάλλον.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: ανάπτυξη από μια εργασία διακλάδωση.

Όταν χρησιμοποιείτε Git, συνήθως κάνετε αλλαγές σε μια εργασία διακλάδωση και να ενσωματώσετε στη το πρωτότυπο κλάδο όταν σας ανάπτυξης φτάσει μια ολοκληρωμένη κατάσταση. Κατά τη φάση ανάπτυξης ενός έργου, θα θέλετε να δημιουργήστε και αναπτύξτε τον κλάδο εργασίας στον Azure.

1. Στην **Εξερεύνηση ομάδας**, επιλέξτε το κουμπί **για οικιακή χρήση** και, στη συνέχεια, επιλέξτε το κουμπί **διακλαδώσεις** .

    ![][40]

2. Επιλέξτε τη σύνδεση **Νέας διακλάδωση** .

    ![][41]

3. Εισαγάγετε το όνομα του κλάδου, όπως "εργασία", και επιλέξτε **Δημιουργία διακλάδωση**. Αυτό δημιουργεί μια νέα τοπική διακλάδωση.

    ![][42]

4. Δημοσίευση στον κλάδο. Επιλέξτε το όνομα κλάδο **αδημοσίευτων διακλαδώσεις**, και επιλέξτε το στοιχείο **Δημοσίευση**.

    ![][44]

6. Από προεπιλογή, μόνο οι αλλαγές στον κλάδο κύρια έναυσμα μια συνεχόμενη Δόμηση. Για να ρυθμίσετε συνεχής Δόμηση για μια εργασία κλάδο, επιλέξτε τη σελίδα που **δημιουργεί** στην **Εξερεύνηση ομάδας**και επιλέξτε **Επεξεργασία ορισμού δημιουργία**.

7. Ανοίξτε την καρτέλα **Ρυθμίσεις προέλευσης** . Στην περιοχή **εγγεγραμμένων διακλαδώνεται για συνεχή ενοποίηση και δημιουργία**, επιλέξτε **κάντε κλικ εδώ για να προσθέσετε μια νέα γραμμή**.

    ![][47]

8. Καθορίστε τον κλάδο που δημιουργήσατε, όπως τιμές REF/κεφαλές εργασία.

    ![][48]

9. Κάνετε μια αλλαγή στον κώδικα, ανοίξτε το μενού συντόμευσης για το τροποποιημένο αρχείο και, στη συνέχεια, επιλέξτε **Ολοκλήρωση**.

    ![][43]

10. Επιλέξτε τη σύνδεση **Μη συγχρονισμένη δεσμεύσεις** και επιλέξτε το κουμπί **Συγχρονισμός** ή τη σύνδεση **Push** για να αντιγράψετε τις αλλαγές στο αντίγραφο του κλάδου εργασία στο Visual Studio Team Services.

    ![][45]

11. Μεταβείτε στην προβολή **δημιουργεί** και βρείτε την έκδοση που έχετε μόλις ενεργοποίησε στον κλάδο της εργασίας.

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερες συμβουλές σχετικά με τη χρήση Git με τις υπηρεσίες ομάδας του Visual Studio, ανατρέξτε στο θέμα [ανάπτυξη και την κοινή χρήση του κώδικα στη χρήση του Visual Studio Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) και πληροφορίες σχετικά με τη χρήση του αποθετηρίου Git που δεν γίνεται από τη Visual Studio Team Services για να δημοσιεύσετε στο Azure, ανατρέξτε στο θέμα [Συνεχούς ανάπτυξης για να Azure εφαρμογής υπηρεσίας](../app-service-web/app-service-continuous-deployment.md). Για περισσότερες πληροφορίες σχετικά με τις υπηρεσίες ομάδας Visual Studio, ανατρέξτε στο θέμα [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
