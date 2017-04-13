<properties
    pageTitle="Συνεχής παράδοσης με το Visual Studio Team Services στο Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους τα έργα σας Visual Studio Team Services ομάδας για την αυτόματη δημιουργία και ανάπτυξη της δυνατότητας εφαρμογής Web στις υπηρεσίες του Azure εφαρμογής υπηρεσίας ή στο cloud."
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

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Συνεχής παράδοση Azure με υπηρεσίες ομάδας Visual Studio

Μπορείτε να ρυθμίσετε τα έργα σας Visual Studio Team Services ομάδας για την αυτόματη δημιουργία και ανάπτυξη εφαρμογών Azure web ή υπηρεσίες στο cloud.  (Για πληροφορίες σχετικά με τον τρόπο για να ρυθμίσετε μια συνεχόμενη Δόμηση και ανάπτυξη συστήματος με χρήση μιας *εσωτερικής* ομάδας Foundation Server, ανατρέξτε στο θέμα [Συνεχής παράδοσης για τις υπηρεσίες Cloud στο Azure](cloud-services-dotnet-continuous-delivery.md).)

Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε Visual Studio 2013 και το SDK Azure εγκατεστημένο. Εάν δεν έχετε ήδη Visual Studio 2013, κάντε λήψη του, επιλέγοντας τη σύνδεση **Γρήγορα αποτελέσματα δωρεάν** στο [www.visualstudio.com](http://www.visualstudio.com). Εγκαταστήστε το SDK Azure από [εδώ](http://go.microsoft.com/fwlink/?LinkId=239540).

> [AZURE.NOTE]Χρειάζεστε ένα λογαριασμό του Visual Studio Team Services για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης: μπορείτε να [ανοίξετε ένα λογαριασμό του Visual Studio Team Services δωρεάν](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Για να ρυθμίσετε μια υπηρεσία στο cloud για την αυτόματη δημιουργία και ανάπτυξη Azure χρησιμοποιώντας τις υπηρεσίες ομάδας Visual Studio, ακολουθήστε τα παρακάτω βήματα.

## <a name="1-create-a-team-project"></a>1: Δημιουργία ομάδας έργου

Ακολουθήστε τις οδηγίες [εδώ](http://go.microsoft.com/fwlink/?LinkId=512980) για να δημιουργήσετε το έργο της ομάδας σας και να συνδέσετε το Visual Studio. Αυτόν τον Οδηγό προϋποθέτει ότι χρησιμοποιείτε ομάδας Foundation έκδοσης στοιχείου ελέγχου (TFVC) ως τη λύση σας Προέλευση στοιχείου ελέγχου. Εάν θέλετε να χρησιμοποιήσετε Git για τον έλεγχο έκδοσης, δείτε [την έκδοση Git του αυτόν τον Οδηγό](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: Επιλέξτε σε ένα έργο σε στοιχείο ελέγχου προέλευσης

1. Στο Visual Studio, ανοίξτε τη λύση που θέλετε για την ανάπτυξη, ή δημιουργήστε ένα νέο.
Μπορείτε να αναπτύξετε μια εφαρμογή web ή μια υπηρεσία cloud (εφαρμογή Azure), ακολουθώντας τα βήματα σε αυτές τις οδηγίες.
Εάν θέλετε να δημιουργήσετε μια νέα λύση, δημιουργήστε ένα νέο έργο υπηρεσία Cloud Azure ή ένα νέο έργο ASP.NET MVC. Βεβαιωθείτε ότι το έργο ως προορισμό .NET Framework 4 ή διαίρεσης 4,5 και, εάν θέλετε να δημιουργήσετε ένα έργο υπηρεσία cloud, προσθέστε ένα ρόλο web ASP.NET MVC και ένα ρόλο εργαζόμενου και, επιλέξτε εφαρμογή Internet για το ρόλο web. Όταν σας ζητηθεί, επιλέξτε **Εφαρμογή Internet**.
Εάν θέλετε να δημιουργήσετε μια εφαρμογή web, επιλέξτε το πρότυπο έργου εφαρμογής Web ASP.NET και, στη συνέχεια, επιλέξτε MVC. Ανατρέξτε στο θέμα [Δημιουργία εφαρμογής web ASP.NET στο Azure εφαρμογής υπηρεσίας](../app-service-web/web-sites-dotnet-get-started.md).

    > [AZURE.NOTE] Visual Studio Team Services υποστηρίζει μόνο CI αναπτύξεων εφαρμογών Web Visual Studio αυτήν τη στιγμή. Υπάρχουν έργα τοποθεσίας Web από το εύρος.

1. Ανοίξτε το μενού περιβάλλοντος για τη λύση και επιλέξτε **Προσθήκη λύσης στο στοιχείο ελέγχου προέλευσης**.

    ![][5]

1. Αποδεχτείτε ή να αλλάξετε τις προεπιλογές και επιλέξτε το κουμπί **OK** . Μόλις ολοκληρωθεί η διαδικασία, Προέλευση στοιχείου ελέγχου εικονίδια εμφανίζονται στην **Εξερεύνηση λύσεων**.

    ![][6]

1. Ανοίξτε το μενού συντόμευσης για τη λύση και επιλέξτε **Μεταβίβαση ελέγχου**.

    ![][7]

1. Στην περιοχή **Αλλαγές σε εκκρεμότητα** της **Εξερεύνησης ομάδας**, πληκτρολογήστε ένα σχόλιο για τη μεταβίβαση ελέγχου και επιλέξτε το κουμπί **Μεταβίβαση ελέγχου** .

    ![][8]

    Παρατηρήστε τις επιλογές για να συμπεριλάβετε ή να εξαιρέσετε συγκεκριμένες αλλαγές κατά τη μεταβίβαση ελέγχου. Εάν εξαιρεθούν αλλαγές που θέλετε, επιλέξτε τη σύνδεση που **Περιλαμβάνουν όλων** .

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: σύνδεση του έργου με Azure

1. Τώρα που έχετε υπηρεσίες ομάδας και στο ομάδας έργου με ορισμένες πηγαίου κώδικα σε αυτό, είστε έτοιμοι να συνδεθείτε το έργο σας ομάδας Azure.  Στην [πύλη του Azure κλασική](http://go.microsoft.com/fwlink/?LinkID=213885), επιλέξτε την εφαρμογή υπηρεσίας ή web cloud ή δημιουργήστε ένα νέο, επιλέγοντας το **+** εικονίδιο στο κάτω αριστερά και επιλέγοντας **Μια υπηρεσία Cloud** ή **Web App** και, στη συνέχεια, **Γρήγορης δημιουργίας**. Επιλέξτε τη σύνδεση **Ρύθμιση δημοσίευσης με τις υπηρεσίες ομάδας του Visual Studio** .

    ![][10]

1. Στον οδηγό, πληκτρολογήστε το όνομα του λογαριασμού σας Visual Studio Team Services στο πλαίσιο κειμένου και κάντε κλικ στη σύνδεση **Εγκρίνετε τώρα** . Ίσως σας ζητηθεί να εισέλθετε.

    ![][11]

1. Στην **Αίτηση σύνδεσης** αναδυόμενο παράθυρο, επιλέξτε το κουμπί **Αποδοχή** για να εξουσιοδοτήσετε Azure για ρύθμιση παραμέτρων του έργου σας ομάδας στις υπηρεσίες ομάδας και στο.

    ![][12]

1. Όταν εξουσιοδότησης με επιτυχία, μπορείτε να δείτε μια αναπτυσσόμενη λίστα που περιέχει μια λίστα με τα έργα σας Visual Studio Team Services ομάδας. Επιλέξτε το όνομα της ομάδας έργου που δημιουργήσατε στα προηγούμενα βήματα και, στη συνέχεια, επιλέξτε το κουμπί σημάδι ελέγχου του οδηγού.

    ![][13]

1. Αφού το έργο σας είναι συνδεδεμένο, θα δείτε ορισμένες οδηγίες για τον έλεγχο αλλαγών στο έργο σας ομάδας Visual Studio Team Services.  Στην την επόμενη μεταβίβασης ελέγχου, Visual Studio Team Services θα δημιουργήσετε και να αναπτύξετε το έργο σας σε Azure.  Δοκιμάστε αυτό τώρα κάνοντας κλικ στην εντολή τη σύνδεση **Μεταβίβαση ελέγχου από το Visual Studio** , και, στη συνέχεια, τη σύνδεση **Εκκίνηση Visual Studio** (ή το κουμπί ισοδύναμο **Visual Studio** στο κάτω μέρος της οθόνης πύλης).

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: έναυσμα ένα αναδόμηση και αναπτύξτε ξανά το έργο σας

1. Στο Visual Studio **Εξερεύνηση ομάδας**, επιλέξτε τη σύνδεση **Εξερεύνηση προέλευσης στοιχείου ελέγχου** .

    ![][15]

1. Μεταβείτε στο αρχείο σας λύση και ανοίξτε το.

    ![][16]

1. Στην **Εξερεύνηση λύσεων**, ανοίξτε ένα αρχείο και να το αλλάξετε. Για παράδειγμα, να αλλάξετε το αρχείο `_Layout.cshtml` κάτω από τις προβολές\\κοινόχρηστου φακέλου σε ένα ρόλο MVC web.

    ![][17]

1. Επεξεργαστείτε το λογότυπο για την τοποθεσία και επιλέξτε το **Συνδυασμό πλήκτρων Ctrl + S** για να αποθηκεύσετε το αρχείο.

    ![][18]

1. Στην **Εξερεύνηση ομάδας**, επιλέξτε τη σύνδεση **Αλλαγές σε εκκρεμότητα** .

    ![][19]

1. Πληκτρολογήστε ένα σχόλιο και, στη συνέχεια, επιλέξτε το κουμπί **Μεταβίβαση ελέγχου** .

    ![][20]

1. Επιλέξτε το κουμπί **αρχική** για να επιστρέψετε στην αρχική σελίδα του **Εξερεύνηση ομάδας** .

    ![][21]

1. Επιλέξτε τη σύνδεση **δημιουργεί** για να προβάλετε το εκδόσεις σε εξέλιξη.

    ![][22]

    **Εξερεύνηση ομάδας** εμφανίζει ότι ένα build ενεργοποιήθηκε για σας μεταβίβαση ελέγχου.

    ![][23]

1. Κάντε διπλό κλικ στο όνομα του Δημιουργία σε εξέλιξη για να προβάλετε ένα αρχείο καταγραφής λεπτομερείς καθώς εξελίσσεται η δόμηση.

    ![][24]

1. Ενώ η Δόμηση βρίσκεται σε εξέλιξη, Ρίξτε μια ματιά στον ορισμό Δόμηση που δημιουργήθηκε όταν έχετε συνδέσει TFS με Azure χρησιμοποιώντας τον οδηγό.  Ανοίξτε το μενού συντόμευσης για τον ορισμό Δόμηση και επιλέξτε **Επεξεργασία ορισμού δημιουργία**.

    ![][25]

    Στην καρτέλα **έναυσμα** , θα δείτε ότι ο ορισμός Δόμηση έχει οριστεί για να δημιουργήσετε σε κάθε μεταβίβασης ελέγχου από προεπιλογή.

    ![][26]

    Στην καρτέλα **διεργασία** , μπορείτε να δείτε το περιβάλλον ανάπτυξης έχει οριστεί στο όνομα της εφαρμογής υπηρεσίας ή web cloud. Εάν εργάζεστε με τα web apps, τις ιδιότητες που μπορείτε να δείτε θα είναι διαφορετικές από εκείνες που φαίνεται εδώ.

    ![][27]

1. Εάν θέλετε διαφορετικές τιμές από τις προεπιλογές, καθορίστε τιμές για τις ιδιότητες. Οι ιδιότητες για τη δημοσίευση Azure είναι στην ενότητα **Ανάπτυξη** .

    Ο παρακάτω πίνακας παρουσιάζει τις διαθέσιμες ιδιότητες στην ενότητα **Ανάπτυξη** :

  	|Ιδιότητα|Προεπιλεγμένη τιμή|
  	|---|---|
  	|Να επιτρέπεται μη αξιόπιστα πιστοποιητικά|Εάν είναι false, πιστοποιητικά SSL πρέπει να είναι υπογεγραμμένα από μια αρχή έκδοσης πιστοποιητικών ρίζας.|
  	|Να επιτραπεί η αναβάθμιση|Επιτρέπει την ανάπτυξη για να ενημερώσετε μια υπάρχουσα ανάπτυξη αντί να δημιουργήσετε ένα νέο. Διατηρεί τη διεύθυνση IP.|
  	|Μην διαγράφετε|Εάν είναι αληθές, χωρίς αντικατάσταση μιας υπάρχουσας μη σχετιζόμενες ανάπτυξης (επιτρέπεται αναβάθμιση).|
  	|Διαδρομή προς ρυθμίσεις ανάπτυξης|Η διαδρομή προς το αρχείο .pubxml για μια εφαρμογή web, σε σχέση με στον ριζικό φάκελο του repo. Παραβλέπεται για τις υπηρεσίες cloud.|
  	|Περιβάλλον ανάπτυξης του SharePoint|Το ίδιο με το όνομα της υπηρεσίας.|
  	|Περιβάλλον ανάπτυξης του Azure|Το web app ή στο cloud όνομα της υπηρεσίας.|

1. Εάν χρησιμοποιείτε πολλές ρυθμίσεις παραμέτρων υπηρεσίας (αρχεία .cscfg), μπορείτε να καθορίσετε τις παραμέτρους της υπηρεσίας επιθυμητή στη ρύθμιση **Δημιουργία, για προχωρημένους, MSBuild ορίσματα** . Για παράδειγμα, για να χρησιμοποιήσετε ServiceConfiguration.Test.cscfg, ορίστε MSBuild ορίσματα επιλογή γραμμής `/p:TargetProfile=Test`.

    ![][38]

    Με αυτήν τη στιγμή, σας δόμηση θα πρέπει να ολοκληρωθεί με επιτυχία.

    ![][28]

1. Εάν κάνετε διπλό κλικ στο όνομα Δόμηση, Visual Studio εμφανίζει μια **Σύνοψη Δόμηση**, συμπεριλαμβανομένων τυχόν αποτελέσματα δοκιμής από σχετικές μονάδα δοκιμής έργα.

    ![][29]

1. Στην [πύλη του Azure κλασική](http://go.microsoft.com/fwlink/?LinkID=213885), μπορείτε να προβάλετε το συσχετισμένο ανάπτυξης στην καρτέλα **αναπτύξεις** όταν είναι επιλεγμένο το περιβάλλον δημιουργίας σταδίων.

    ![][30]

1.  Μεταβείτε στη διεύθυνση URL της τοποθεσίας σας. Για μια εφαρμογή web, απλώς κάντε κλικ στο κουμπί **Αναζήτηση** στη γραμμή εντολών. Για μια υπηρεσία cloud, επιλέξτε τη διεύθυνση URL στην ενότητα **Γρήγορη Glance** της σελίδας **πίνακα εργαλείων** που εμφανίζει το περιβάλλον ενδιάμεσου σταδίου για μια υπηρεσία στο cloud. Αναπτύξεις από συνεχής ενοποίησης για τις υπηρεσίες cloud δημοσιεύονται στο περιβάλλον ενδιάμεσου σταδίου από προεπιλογή. Μπορείτε να το αλλάξετε, ορίζοντας την ιδιότητα **Εναλλακτικό περιβάλλον υπηρεσία Cloud** **παραγωγή**. Αυτό το στιγμιότυπο οθόνης δείχνει πού βρίσκεται η διεύθυνση URL της τοποθεσίας στη σελίδα πίνακα εργαλείων την υπηρεσία cloud.

    ![][31]

    Θα ανοίξει μια νέα καρτέλα του προγράμματος περιήγησης για να αποκαλύψετε εκτελείται την τοποθεσία σας.

    ![][32]

    Για τις υπηρεσίες cloud, εάν κάνετε άλλες αλλαγές στο έργο σας, έναυσμα που δημιουργεί πιο και θα συγκεντρώσουν πολλές αναπτύξεις. Η πιο πρόσφατη μία έχει επισημανθεί ως ενεργό.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Αναπτύξτε ξανά μια παλιότερη έκδοση

Αυτό το βήμα ισχύει για τις υπηρεσίες cloud και είναι προαιρετικό. Στην κλασική Azure πύλη, επιλέξτε μια προηγούμενη ανάπτυξη και, στη συνέχεια, επιλέξτε το κουμπί **αναπτύξτε ξανά** για να επαναφέρετε την τοποθεσία σας σε μια προηγούμενη μεταβίβαση ελέγχου.  Σημειώστε ότι αυτό θα ενεργοποιήσετε εκ νέου δημιουργία στο TFS και να δημιουργήσετε μια νέα καταχώρηση στο ιστορικό ανάπτυξης.

![][34]

## <a name="6-change-the-production-deployment"></a>6: αλλαγή της ανάπτυξης της παραγωγής

Αυτό το βήμα ισχύει μόνο για τις υπηρεσίες cloud, δεν εφαρμογές web. Όταν είστε έτοιμοι, μπορείτε να προβιβάσετε το περιβάλλον ενδιάμεσου σταδίου στο περιβάλλον παραγωγής, επιλέγοντας το κουμπί **Εναλλαγή** στην πύλη του Azure κλασική. Το περιβάλλον που μόλις ανεπτυγμένος ενδιάμεσου σταδίου προωθείται στο παραγωγής και το προηγούμενο περιβάλλον παραγωγής, αν υπάρχουν, γίνεται ένα περιβάλλον ενδιάμεσου σταδίου. Η ενεργή ανάπτυξη ενδέχεται να είναι διαφορετικές για την παραγωγή και ενδιάμεσου περιβάλλοντα, αλλά το ιστορικό ανάπτυξης του πρόσφατες εκδόσεις είναι η ίδια, ανεξάρτητα από το περιβάλλον.

![][35]

## <a name="7-run-unit-tests"></a>7: Εκτέλεση ελέγχων μονάδας

Αυτό το βήμα ισχύει μόνο για το web apps, όχι στο cloud services. Για να τοποθετήσετε μια πύλη ποιότητα στην ανάπτυξη, μπορείτε να εκτελέσετε δοκιμές μονάδα και αν δεν, μπορείτε να διακόψετε την ανάπτυξη.

1.  Στο Visual Studio, προσθέστε ένα έργο δοκιμής μονάδα.

    ![][39]

1.  Προσθήκη αναφορών έργου στο έργο που θέλετε να ελέγξετε.

    ![][40]

1.  Προσθήκη ορισμένες δοκιμές μονάδα. Για να ξεκινήσετε, δοκιμάστε μια εικονική δοκιμή που θα περάσουν πάντα.

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  Επεξεργαστείτε τον ορισμό Δόμηση, επιλέξτε την καρτέλα **διαδικασία** και αναπτύξτε τον κόμβο **δοκιμής** .

1.  Ορίστε τη **Δόμηση αποτυγχάνουν σε περίπτωση αποτυχίας δοκιμής** στην τιμή True. Αυτό σημαίνει ότι δεν θα προκύψουν ανάπτυξης, εκτός εάν μεταβιβάζουν τις δοκιμές.

    ![][41]

1.  Ουρά μια νέα έκδοση.

    ![][42]

    ![][43]

1. Ενώ συνεχίσετε τη Δόμηση, επιλέξτε κατά την πρόοδο του έργου.

    ![][44]

    ![][45]

1. Όταν ολοκληρωθεί η δημιουργία του, ελέγξτε τα αποτελέσματα της δοκιμής.

    ![][46]

    ![][47]

1.  Δοκιμάστε να δημιουργήσετε έναν έλεγχο που θα αποτύχει. Προσθήκη ενός νέου ελέγχου, αντιγράφοντας το πρώτο, μετονομάστε την και σχολιάσετε τη γραμμή του κώδικα που αναφέρει NotImplementedException εξαιρείται από τον αναμενόμενο.

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. Ελέγξτε την αλλαγή στην ουρά μια νέα έκδοση.

    ![][48]

1. Προβάλετε τα αποτελέσματα δοκιμής για να δείτε λεπτομέρειες σχετικά με την αποτυχία.

    ![][49]

    ![][50]

## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες πληροφορίες σχετικά με τη δοκιμή στο Visual Studio Team Services μονάδων, ανατρέξτε στο θέμα [Εκτέλεση ελέγχων μονάδας στο δημιουργία σας](http://go.microsoft.com/fwlink/p/?LinkId=510474). Εάν χρησιμοποιείτε το Git, ανατρέξτε στο θέμα [κοινή χρήση του κώδικα στο Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) και [συνεχούς ανάπτυξης για να Azure εφαρμογής υπηρεσίας](../app-service-web/app-service-continuous-deployment.md).  Για περισσότερες πληροφορίες σχετικά με τις υπηρεσίες ομάδας Visual Studio, ανατρέξτε στο θέμα [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
