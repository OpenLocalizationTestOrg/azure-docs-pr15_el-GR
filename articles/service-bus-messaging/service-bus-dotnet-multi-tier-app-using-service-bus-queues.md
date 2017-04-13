<properties
    pageTitle="Εφαρμογή πολλαπλών επιπέδων .NET | Microsoft Azure"
    description="Ένα πρόγραμμα εκμάθησης .NET που σας βοηθά να αναπτύξετε μια εφαρμογή πολλαπλών επιπέδων στο Azure που χρησιμοποιεί ουρές Bus υπηρεσία για την επικοινωνία μεταξύ βαθμίδες."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>

# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>Εφαρμογή πολλαπλών επιπέδων .NET χρησιμοποιώντας ουρές Bus υπηρεσίας Azure

## <a name="introduction"></a>Εισαγωγή

Ανάπτυξη για το Microsoft Azure είναι εύκολη με χρήση του Visual Studio και το δωρεάν SDK Azure για .NET. Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί σε τα βήματα για να δημιουργήσετε μια εφαρμογή που χρησιμοποιεί πολλούς πόρους Azure που εκτελείται στο περιβάλλον σας τοπικά. Τα βήματα θεωρείται ότι έχετε χωρίς προηγούμενη εμπειρία με χρήση Azure.

Θα μάθετε τα εξής:

-   Μάθετε πώς μπορείτε να ενεργοποιήσετε τον υπολογιστή σας για την ανάπτυξη Azure με μία λήψη και εγκατάσταση.
-   Πώς μπορείτε να χρησιμοποιήσετε το Visual Studio για την ανάπτυξη για Azure.
-   Μάθετε πώς μπορείτε να δημιουργήσετε μια εφαρμογή πολλαπλών επιπέδων στο Azure μέσω web και εργαζόμενου ρόλων.
-   Πώς να επικοινωνούν μεταξύ των σειρών χρησιμοποιώντας ουρές Bus υπηρεσίας.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

Σε αυτό το πρόγραμμα εκμάθησης που θα δημιουργήσετε και εκτελέστε την εφαρμογή πολλαπλών επιπέδων σε μια υπηρεσία Azure cloud. Προσκηνίου της θα ρόλου web ASP.NET MVC και παρασκηνίου θα είναι ένα ρόλο εργασίας που χρησιμοποιεί μια ουρά Bus υπηρεσίας. Μπορείτε να δημιουργήσετε την ίδια εφαρμογή πολλαπλών επιπέδων με προσκηνίου της ως ένα έργο web που έχει αναπτυχθεί σε μια τοποθεσία Web Azure αντί για μια υπηρεσία στο cloud. Για οδηγίες σχετικά με το τι πρέπει να κάνετε διαφορετικά σε μια τοποθεσία Web του Azure προσκηνίου, ανατρέξτε στην ενότητα [επόμενα βήματα](#nextsteps) . Μπορείτε επίσης να δοκιμάσετε το πρόγραμμα εκμάθησης [εφαρμογής στην-εσωτερικής εγκατάστασης/cloud υβριδική .NET](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) .

Το παρακάτω στιγμιότυπο οθόνης εμφανίζει οι δυνατότητες της εφαρμογής.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Επισκόπηση σενάριο: επικοινωνίας μεταξύ ρόλων

Για να υποβάλετε μια σειρά για επεξεργασία, το στοιχείο προσκηνίου περιβάλλοντος εργασίας Χρήστη, εκτελείται στο ρόλο web, πρέπει να αλληλεπιδρούν της λογικής μεσαίας εκτελείται στο ρόλο εργασίας. Σε αυτό το παράδειγμα χρησιμοποιεί υπηρεσία Bus όπου μεσολαβούν ανταλλαγής μηνυμάτων για την επικοινωνία μεταξύ τις σειρές.

Χρήση όπου μεσολαβούν ανταλλαγής μηνυμάτων ανάμεσα στο web και το μεσαίο βαθμίδες αποσυνδέει τα δύο στοιχεία. Σε αντίθεση με το άμεσων μηνυμάτων (δηλαδή, TCP ή HTTP), η σειρά web δεν συνδέεται στο μεσαίο επίπεδο απευθείας. αντί για αυτό το προωθεί μονάδες εργασίας, με τα μηνύματα, σε Bus υπηρεσίας, η οποία αξιόπιστα διατηρεί τους μέχρι το μεσαίο επίπεδο είναι έτοιμη για χρήση και την επεξεργασία τους.

Υπηρεσία Bus παρέχει δύο οντοτήτων για την υποστήριξη όπου μεσολαβούν μηνυμάτων: ουρές και θέματα. Με ουρές, κάθε μήνυμα που αποστέλλεται σε ουρά καταναλώνεται από ένα μεμονωμένο δέκτη. Θέματα υποστήριξης το μοτίβο δημοσίευση/εγγραφή με την οποία γίνεται διαθέσιμο σε μια συνδρομή που έχουν καταχωρηθεί στο θέμα κάθε δημοσιευμένο μήνυμα. Κάθε συνδρομής λογικά διατηρεί τη δική του ουρά των μηνυμάτων. Συνδρομές επίσης μπορούν να ρυθμιστούν με φίλτρο κανόνες που περιορίζουν το σύνολο των μηνυμάτων που του μεταβιβάστηκε η ουρά συνδρομή σε αυτές που ταιριάζουν με το φίλτρο. Το παρακάτω παράδειγμα χρησιμοποιεί ουρές Bus υπηρεσίας.

![][1]

Αυτός ο μηχανισμός επικοινωνίας περιλαμβάνει πολλά πλεονεκτήματα σε σχέση με την ανταλλαγή άμεσων μηνυμάτων:

-   **Χρονικό αποσύνδεση.** Με την ανταλλαγή μηνυμάτων ασύγχρονης μοτίβο, παραγωγών και καταναλωτών δεν χρειάζεται να online την ίδια στιγμή. Υπηρεσία Bus αποθηκεύει αξιόπιστα μηνύματα μέχρι το καταναλώνει μέρος είναι έτοιμη για λήψη τους. Αυτή η δυνατότητα επιτρέπει τα στοιχεία της κατανεμημένης εφαρμογής να διακοπεί, είτε προαιρετικά, για παράδειγμα, για συντήρηση ή λόγω σφάλμα στοιχείου, χωρίς να επηρεάζουν το σύστημα ως σύνολο. Επιπλέον, η εφαρμογή καταναλώνει μόνο ίσως χρειαστεί να παραδίδεται online κατά τη διάρκεια ορισμένων ώρες της ημέρας.

-   **Η φόρτωση ισοστάθμιση.** Σε πολλές εφαρμογές, φόρτωση του συστήματος ποικίλλει ανάλογα με τον καιρό, ενώ ο χρόνος επεξεργασίας που απαιτούνται για κάθε μονάδα εργασίας είναι συνήθως σταθερές. Intermediating μήνυμα παραγωγών και καταναλωτών με μια ουρά σημαίνει ότι η εφαρμογή καταναλώνει (ο εργαζόμενος) χρειάζεται μόνο προμήθεια ώστε να περιλαμβάνει average φόρτωση και όχι φόρτωση κορύφωσης. Το βάθος της ουράς αναπτύσσεται και τις συμβάσεις όπως η φόρτωση εισερχόμενες ποικίλλει. Αυτό αποθηκεύει απευθείας χρήματα όσον αφορά το ποσό της υποδομής που απαιτείται για την εξυπηρέτηση η φόρτωση της εφαρμογής.

-   **Εξισορρόπηση φόρτου.** Όπως φόρτωση αυξάνεται, περισσότερες διαδικασίες εργασίας μπορούν να προστεθούν σε ανάγνωση από την ουρά. Κάθε μήνυμα υποβάλλεται σε επεξεργασία από έναν μόνο από οι διαδικασίες εργασίας. Επιπλέον, αυτό εξισορρόπησης φόρτου βάσει ελκυστική επιτρέπει βέλτιστη χρήση τις μηχανές εργασίας ακόμα και αν οι υπολογιστές εργασίας διαφέρουν όσον αφορά την ισχύ επεξεργασίας, όπως αυτά θα ελκυστική μηνύματα με το δικό τους μέγιστο χρέωση. Αυτό το μοτίβο είναι συχνά ονομάζονται το μοτίβο *κάθε καταναλωτών* .

    ![][2]

Οι παρακάτω ενότητες περιγράφουν τον κώδικα που υλοποιεί αυτήν την αρχιτεκτονική.

## <a name="set-up-the-development-environment"></a>Ρυθμίστε το περιβάλλον ανάπτυξης

Πριν να ξεκινήσετε Azure εφαρμογές προγραμματισμού, λάβετε τα εργαλεία και να ρυθμίσετε το περιβάλλον ανάπτυξης.

1.  Εγκαταστήστε το Azure SDK για .NET στο [λήψη εργαλεία και SDK][].

2.  Κάντε κλικ στην επιλογή **εγκατάσταση του SDK** για την έκδοση του Visual Studio που χρησιμοποιείτε. Τα βήματα σε αυτό το πρόγραμμα εκμάθησης Χρησιμοποιήστε το Visual Studio 2015.

4.  Όταν σας ζητηθεί να εκτελέσετε ή να αποθηκεύσετε το πρόγραμμα εγκατάστασης, κάντε κλικ στην επιλογή **Εκτέλεση**.

5.  Στο **Πρόγραμμα εγκατάστασης πλατφόρμας Web**, κάντε κλικ στην επιλογή **εγκατάσταση** και συνεχίσετε με την εγκατάσταση.

6.  Μόλις ολοκληρωθεί η εγκατάσταση, θα έχετε όλα όσα χρειάζεται για να ξεκινήσετε την ανάπτυξη της εφαρμογής. Το SDK περιλαμβάνει εργαλεία που σας επιτρέπουν να αναπτύξετε εύκολα Azure εφαρμογές στο Visual Studio. Εάν δεν έχετε εγκατεστημένο το Visual Studio, το SDK εγκαθιστά το δωρεάν Visual Studio Express.

## <a name="create-a-namespace"></a>Δημιουργία ενός χώρου ονομάτων

Το επόμενο βήμα είναι να δημιουργήσετε ένα χώρο ονομάτων υπηρεσίας και αποκτήσετε έναν αριθμό-κλειδί θέσει σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας). Ένας χώρος ονομάτων παρέχει όριο μιας εφαρμογής για κάθε εφαρμογή εκτίθεται μέσω Bus υπηρεσίας. Δημιουργείται ένα κλειδί συσχετισμών Ασφαλείας από το σύστημα όταν δημιουργείται ένα χώρο ονομάτων. Ο συνδυασμός του χώρου ονομάτων και κλειδί συσχετισμών Ασφαλείας παρέχει τα διαπιστευτήρια για Bus Service για τον έλεγχο ταυτότητας πρόσβαση σε μια εφαρμογή του.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Δημιουργήστε ένα ρόλο web

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε το προσκήνιο της εφαρμογής σας. Πρώτα, μπορείτε να δημιουργήσετε τις σελίδες που εμφανίζει η εφαρμογή σας.
Μετά από αυτό, προσθέστε κώδικα που υποβάλλει στοιχείων σε μια υπηρεσία Bus ουρά και να εμφανίζει πληροφορίες κατάστασης σχετικά με την ουρά.

### <a name="create-the-project"></a>Δημιουργία έργου

1.  Με δικαιώματα διαχειριστή, ξεκινήστε Microsoft Visual Studio. Για να ξεκινήσετε Visual Studio με δικαιώματα διαχειριστή, κάντε δεξί κλικ στο εικονίδιο του προγράμματος **Visual Studio** και, στη συνέχεια, κάντε κλικ στην επιλογή **Εκτέλεση ως διαχειριστής**. Το προσομοίωσης Azure υπολογισμού, περιγράφονται παρακάτω σε αυτό το άρθρο, απαιτεί ότι Visual Studio να ξεκινά με δικαιώματα διαχειριστή.

    Στο Visual Studio, στο μενού **αρχείο** , κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **έργο**.

2.  Από τα **Εγκατεστημένα πρότυπα**, στην περιοχή **Visual C#**, κάντε κλικ στην επιλογή **Cloud** και, στη συνέχεια, κάντε κλικ στην επιλογή **Υπηρεσία Cloud Azure**. Το όνομα του έργου **MultiTierApp**. Στη συνέχεια, κάντε κλικ στο **κουμπί OK**.

    ![][9]

3.  Από τους ρόλους **διαίρεσης 4,5 .NET Framework** , κάντε διπλό κλικ στο **Ρόλο Web ASP.NET**.

    ![][10]

4.  Δείκτη του ποντικιού επάνω στην περιοχή **υπηρεσία Cloud Azure λύση** **WebRole1** , κάντε κλικ στο εικονίδιο μολυβιού και μετονομάστε το ρόλο web σε **FrontendWebRole**. Στη συνέχεια, κάντε κλικ στο **κουμπί OK**. (Βεβαιωθείτε ότι μπορείτε να εισαγάγετε "Frontend" με ένα πεζά "e," Όχι "FrontEnd".)

    ![][11]

5.  Από το παράθυρο διαλόγου **Νέο έργο ASP.NET** , στη λίστα **Επιλέξτε ένα πρότυπο** , κάντε κλικ στην επιλογή **MVC**.

    ![][12]

6. Ακόμη στο παράθυρο διαλόγου **Νέο έργο ASP.NET** , κάντε κλικ στο κουμπί **Αλλαγή ελέγχου ταυτότητας** . Στο παράθυρο διαλόγου **Αλλαγή ελέγχου ταυτότητας** , κάντε κλικ στην επιλογή **Χωρίς έλεγχο ταυτότητας**και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**. Για αυτό το πρόγραμμα εκμάθησης, θα ανάπτυξη μια εφαρμογή που δεν χρειάζεται μια σύνδεση χρήστη.

    ![][16]

7. Πίσω στο παράθυρο διαλόγου **Νέο έργο ASP.NET** , κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο.

6.  Στην **Εξερεύνηση λύσεων**, μέσα στο έργο **FrontendWebRole** , κάντε δεξί κλικ σε **αναφορές**και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.

7.  Κάντε κλικ στην καρτέλα **Αναζήτηση** και, στη συνέχεια, αναζητήστε το `Microsoft Azure Service Bus`. Κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης.

    ![][13]

    Σημειώστε ότι τώρα αναφέρονται οι συγκροτήσεις απαιτείται πρόγραμμα-πελάτη και έχουν προστεθεί ορισμένα νέα αρχεία κώδικα.

9.  Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ σε **μοντέλα** και κάντε κλικ στο κουμπί **Προσθήκη**και κατόπιν κάντε κλικ στην επιλογή **τάξης**. Στο πλαίσιο **όνομα** , πληκτρολογήστε το όνομα **OnlineOrder.cs**. Στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

### <a name="write-the-code-for-your-web-role"></a>Συντάξτε τον κώδικα για το ρόλο του web

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε τις διάφορες σελίδες που εμφανίζει η εφαρμογή σας.

1.  Στο αρχείο OnlineOrder.cs στο Visual Studio, αντικαταστήστε τον υπάρχοντα ορισμό χώρο ονομάτων με τον ακόλουθο κώδικα:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  Στην **Εξερεύνηση λύσεων**, κάντε διπλό κλικ **Controllers\HomeController.cs**. Προσθέστε τις παρακάτω προτάσεις **Χρήση** στο επάνω μέρος του αρχείου για να συμπεριλάβετε τα πεδία ονομάτων για το μοντέλο που μόλις δημιουργήσατε, καθώς και Bus υπηρεσίας.

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  Επίσης στο αρχείο HomeController.cs στο Visual Studio, αντικαταστήστε τον υπάρχοντα ορισμό χώρο ονομάτων με τον ακόλουθο κώδικα. Αυτός ο κωδικός περιέχει μεθόδους για το χειρισμό την υποβολή των στοιχείων στην ουρά.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  Στο μενού " **Δημιουργία** ", κάντε κλικ στην επιλογή **Δημιουργία λύση** για να ελέγξετε την ακρίβεια της εργασίας σας μέχρι στιγμής.

5.  Τώρα, δημιουργήστε την προβολή για το `Submit()` μέθοδο που δημιουργήσατε νωρίτερα. Κάντε δεξί κλικ μέσα το `Submit()` μέθοδο (το υπερφόρτωσης του `Submit()` που δεν δέχεται παραμέτρους), και, στη συνέχεια, επιλέξτε **Προσθήκη προβολής**.

    ![][14]

6.  Εμφανίζεται ένα παράθυρο διαλόγου για τη δημιουργία της προβολής. Στη λίστα **πρότυπο** , επιλέξτε **Δημιουργία**. Στη λίστα **κλάση μοντέλου** , κάντε κλικ στην επιλογή την κλάση **OnlineOrder** .

    ![][15]

7.  Κάντε κλικ στην επιλογή **Προσθήκη**.

8.  Τώρα, μπορείτε να αλλάξετε το όνομα της εφαρμογής σας που εμφανίζεται. Στην **Εξερεύνηση λύσεων**, κάντε διπλό κλικ το **Views\Shared\\_Layout.cshtml** αρχείο για να το ανοίξετε στο πρόγραμμα επεξεργασίας Visual Studio.

9.  Αντικαταστήστε όλες τις εμφανίσεις της **Εφαρμογής μου ASP.NET** με **προϊόντα του LITWARE**.

10. Καταργήστε τις συνδέσεις ** **για οικιακή χρήση**, και **επαφή** **. Διαγράψτε τον επιλεγμένο κώδικα:

    ![][28]

11. Τέλος, τροποποιήστε τη σελίδα υποβολής για να συμπεριλάβετε ορισμένες πληροφορίες σχετικά με την ουρά. Στην **Εξερεύνηση λύσεων**, κάντε διπλό κλικ στο αρχείο **Views\Home\Submit.cshtml** για να το ανοίξετε στο πρόγραμμα επεξεργασίας Visual Studio. Προσθέστε την ακόλουθη γραμμή μετά `<h2>Submit</h2>`. Προς το παρόν, η `ViewBag.MessageCount` είναι κενό. Θα το συμπληρώσετε αργότερα.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. Τώρα έχετε υλοποιήσει το περιβάλλον εργασίας Χρήστη. Μπορείτε να πατήσετε **F5** για να εκτελέσετε την εφαρμογή σας και επιβεβαιώστε ότι εμφανίζεται όπως αναμένεται.

    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Συντάξτε τον κώδικα για την υποβολή στοιχείων σε μια υπηρεσία Bus ουρά

Τώρα, προσθέστε κώδικα για την υποβολή στοιχείων σε μια ουρά. Πρώτα, μπορείτε να δημιουργήσετε ένα εκπαιδευτικό που περιέχει τις πληροφορίες σύνδεσης Bus υπηρεσίας ουράς. Προετοιμασία, στη συνέχεια, τη σύνδεση από Global.aspx.cs. Τέλος, ενημερώστε τον κώδικα υποβολής που δημιουργήσατε νωρίτερα σε HomeController.cs για να υποβάλετε πραγματικά στοιχεία σε μια υπηρεσία Bus ουρά.

1.  Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ **FrontendWebRole** (δεξιό κλικ στο έργο, όχι το ρόλο). Κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **τάξης**.

2.  Το όνομα της κλάσης **QueueConnector.cs**. Κάντε κλικ στο κουμπί **Προσθήκη** για να δημιουργήσετε την κλάση.

3.  Τώρα, προσθέστε κώδικα που ενσωματώνει τις πληροφορίες σύνδεσης και προετοιμάζει τη σύνδεση με μια υπηρεσία Bus ουρά. Αντικαταστήστε όλα τα περιεχόμενα του QueueConnector.cs με τον ακόλουθο κώδικα και καταχωρήστε τιμές για `your Service Bus namespace` (το όνομά σας χώρο ονομάτων) και `yourKey`, ποιο είναι το **πρωτεύον κλειδί** που είχατε αποκτήσει από την πύλη του Azure.

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Τώρα, βεβαιωθείτε ότι η μέθοδος **Προετοιμασία** λαμβάνει κλήσης. Στην **Εξερεύνηση λύσεων**, κάντε διπλό κλικ **Global.asax\Global.asax.cs**.

5.  Προσθέστε την ακόλουθη γραμμή κώδικα στο τέλος της μεθόδου **Application_Start** .

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Τέλος, ενημερώστε τον κώδικα web που δημιουργήσατε νωρίτερα, για να υποβάλετε τα στοιχεία στην ουρά. Στην **Εξερεύνηση λύσεων**, κάντε διπλό κλικ **Controllers\HomeController.cs**.

7.  Ενημέρωση του `Submit()` μέθοδο (το υπερφόρτωσης που τίθεται χωρίς παραμέτρους) ως εξής για να λάβετε το μήνυμα πλήθος για ουρά.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Ενημέρωση του `Submit(OnlineOrder order)` μέθοδο (το υπερφόρτωσης που λαμβάνει μία παράμετρο) ως εξής για να υποβάλουν τις πληροφορίες παραγγελίας στην ουρά.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  Τώρα μπορείτε να εκτελέσετε ξανά την εφαρμογή. Κάθε φορά που υποβάλετε μια σειρά, το πλήθος μήνυμα αυξάνεται.

    ![][18]

## <a name="create-the-worker-role"></a>Δημιουργία του ρόλου εργαζόμενου

Τώρα θα δημιουργήσετε το ρόλο εργαζόμενου που επεξεργάζεται το υποβολές σειρά. Αυτό το παράδειγμα χρησιμοποιεί το πρότυπο έργου Visual Studio **Εργαζόμενου ρόλων με ουρά Bus υπηρεσίας** . Έχετε ήδη λάβει τα απαιτούμενα διαπιστευτήρια από την πύλη.

1. Βεβαιωθείτε ότι έχετε συνδέσει Visual Studio Azure λογαριασμό σας.

2.  Στο Visual Studio, στην **Εξερεύνηση λύσεων** , κάντε δεξί κλικ στο φάκελο **ρόλους** κάτω από το έργο **MultiTierApp** .

3.  Κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέο έργο ρόλο εργασίας**. Εμφανίζεται το παράθυρο διαλόγου **Προσθήκη νέου έργου ρόλο** .

    ![][26]

4.  Στο παράθυρο διαλόγου **Προσθήκη νέου έργου ρόλου** , κάντε κλικ στην επιλογή **Εργαζόμενου ρόλων με ουρά Bus υπηρεσίας**.

    ![][23]

5.  Στο πλαίσιο **όνομα** , ονομάστε το έργο **OrderProcessingRole**. Στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

6.  Αντιγράψτε τη συμβολοσειρά σύνδεσης που προέκυψε στο βήμα 9 της ενότητας "Δημιουργία ενός χώρου ονομάτων Bus υπηρεσίας" στο Πρόχειρο.

7.  Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ **OrderProcessingRole** που δημιουργήσατε στο βήμα 5 (βεβαιωθείτε ότι κάνετε δεξί κλικ στην περιοχή **τους ρόλους**και όχι την κλάση **OrderProcessingRole** ). Στη συνέχεια, κάντε κλικ στην επιλογή **Ιδιότητες**.

8.  Στην καρτέλα " **Ρυθμίσεις** " του παραθύρου διαλόγου **Ιδιότητες** , κάντε κλικ μέσα στο πλαίσιο **τιμή** για **Microsoft.ServiceBus.ConnectionString**και, στη συνέχεια, επικολλήστε την τιμή τελικού σημείου που αντιγράψατε στο βήμα 6.

    ![][25]

9.  Δημιουργία μιας κλάσης **OnlineOrder** για να αντιπροσωπεύει τις παραγγελίες κατά την επεξεργασία τους από την ουρά. Μπορείτε να χρησιμοποιήσετε ξανά μια τάξης που έχετε ήδη δημιουργήσει. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ την κλάση **OrderProcessingRole** (δεξιό κλικ στο εικονίδιο τάξης, όχι το ρόλο). Κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **Υπάρχον στοιχείο**.

10. Αναζήτηση για να τον υποφάκελο για **FrontendWebRole\Models**και, στη συνέχεια, κάντε διπλό κλικ στο **OnlineOrder.cs** για να το προσθέσετε σε αυτό το έργο.

11. Στο **WorkerRole.cs**, αλλάξτε την τιμή της μεταβλητής **Όνομα_ουράς** από `"ProcessingQueue"` να `"OrdersQueue"` όπως φαίνεται στο ακόλουθο κώδικα.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. Προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση στο επάνω μέρος του αρχείου WorkerRole.cs.

    ```
    using FrontendWebRole.Models;
    ```

13. Στο το `Run()` λειτουργούν, εσωτερικά το `OnMessage()` κλήσεων, αντικαταστήστε τα περιεχόμενα του το `try` όρος με τον ακόλουθο κώδικα.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. Έχετε ολοκληρώσει την εφαρμογή. Μπορείτε να ελέγξετε την πλήρη εφαρμογή κάνοντας δεξί κλικ στο έργο MultiTierApp στην Εξερεύνηση λύσεων, επιλέγοντας **Ορισμός ως εκκίνησης έργου**και, στη συνέχεια, πατώντας το πλήκτρο F5. Σημειώστε ότι το πλήθος μήνυμα δεν προσαυξάνει, επειδή ο ρόλος εργαζόμενου επεξεργάζεται στοιχεία από την ουρά και τις επισημαίνει ως ολοκληρωμένη. Μπορείτε να δείτε το αποτέλεσμα ανίχνευση του ρόλου σας εργαζόμενου, προβάλλοντας Azure τον υπολογισμό προσομοίωσης περιβάλλον εργασίας Χρήστη του. Μπορείτε να το κάνετε κάνοντας δεξί κλικ στο εικονίδιο προσομοίωσης στην περιοχή ειδοποιήσεων της γραμμής εργασιών σας και επιλέγοντας **Εμφάνιση τον υπολογισμό προσομοίωσης περιβάλλοντος εργασίας Χρήστη**.

    ![][19]

    ![][20]

## <a name="next-steps"></a>Επόμενα βήματα  

Για να μάθετε περισσότερα σχετικά με την υπηρεσία Bus, ανατρέξτε στους ακόλουθους πόρους:  

* [Bus Azure υπηρεσίας][sbmsdn]  
* [Σελίδα υπηρεσίας Bus υπηρεσίας][sbwacom]  
* [Πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας][sbwacomqhowto]  

Για να μάθετε περισσότερα σχετικά με τα σενάρια πολλαπλών επιπέδων, ανατρέξτε στα θέματα:  

* [Εφαρμογή πολλαπλών επιπέδων .NET χρησιμοποιώντας πίνακες χώρου αποθήκευσης, ουρές και αντικείμενα BLOB][mutitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Λήψη εργαλεία και SDK]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  