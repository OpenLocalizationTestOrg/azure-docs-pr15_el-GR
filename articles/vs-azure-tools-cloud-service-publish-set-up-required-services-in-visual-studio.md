<properties
   pageTitle="Προετοιμασία για να δημοσιεύσετε ή να αναπτύξετε μια εφαρμογή του Azure από το Visual Studio | Microsoft Azure"
   description="Μάθετε τις διαδικασίες για να ρυθμίσετε υπηρεσίες cloud και αποθήκευσης λογαριασμού και ρύθμιση παραμέτρων του Azure εφαρμογής."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Προετοιμασία για να δημοσιεύσετε ή να αναπτύξετε μια εφαρμογή από το Visual Studio του Azure

## <a name="overview"></a>Επισκόπηση

Μπορείτε να δημοσιεύσετε ένα έργο υπηρεσία cloud, πρέπει να ορίσετε τις ακόλουθες υπηρεσίες:

- Μια **υπηρεσία cloud** για να εκτελέσετε το τους ρόλους στο περιβάλλον του Azure

- Ένας **λογαριασμός χώρου αποθήκευσης** που παρέχει πρόσβαση στις υπηρεσίες Blob, ουρά και πίνακα.

Χρησιμοποιήστε τις ακόλουθες διαδικασίες για να ρυθμίσετε αυτές τις υπηρεσίες και ρύθμιση παραμέτρων της εφαρμογής σας


## <a name="create-a-cloud-service"></a>Δημιουργήστε μια υπηρεσία στο cloud

Για να δημοσιεύσετε μια υπηρεσία cloud Azure, πρέπει πρώτα να δημιουργήσετε μια υπηρεσία cloud, η οποία εκτελείται ρόλους σας στο περιβάλλον Azure. Μπορείτε να δημιουργήσετε μια υπηρεσία cloud στην [πύλη του Azure κλασική](http://go.microsoft.com/fwlink/?LinkID=213885), όπως περιγράφεται στην ενότητα **για να δημιουργήσετε μια υπηρεσία cloud, χρησιμοποιώντας την πύλη του Azure κλασική**, παρακάτω σε αυτό το θέμα. Μπορείτε επίσης να δημιουργήσετε μια υπηρεσία cloud στο Visual Studio, χρησιμοποιώντας τον Οδηγό δημοσίευσης.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Για να δημιουργήσετε μια υπηρεσία cloud με χρήση του Visual Studio

1. Ανοίξτε το μενού συντόμευσης για το έργο Azure και επιλέξτε το στοιχείο **Δημοσίευση**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)

1. Εάν δεν έχετε πραγματοποιήσει είσοδο, συνδεθείτε με το όνομα χρήστη και τον κωδικό πρόσβασης για το λογαριασμό Microsoft ή εταιρικό λογαριασμό που σχετίζεται με τη συνδρομή σας στο Azure.

1. Επιλέξτε το κουμπί " **Επόμενο** " για να προχωρήσετε τη σελίδα " **Ρυθμίσεις** ".

    ![Συνήθεις ρυθμίσεις δημοσίευσης οδηγού](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)

1. Στη λίστα **Cloud Services** , επιλέξτε **Δημιουργία νέου**. Εμφανίζεται το παράθυρο διαλόγου **Δημιουργία υπηρεσιών Azure** .

1. Πληκτρολογήστε το όνομα της υπηρεσίας σας cloud. Το όνομα αποτελεί τμήμα της διεύθυνσης URL για την υπηρεσία σας και, επομένως, πρέπει να είναι μοναδικό καθολικά. Το όνομα δεν είναι διάκριση πεζών-κεφαλαίων.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Για να δημιουργήσετε μια υπηρεσία στο cloud χρησιμοποιώντας την πύλη κλασική του Azure

1. Είσοδος στην [πύλη του Azure κλασική](http://go.microsoft.com/fwlink/?LinkId=253103) στην τοποθεσία Web της Microsoft.

1. (προαιρετικό) Για να εμφανίσετε μια λίστα με τις υπηρεσίες cloud που έχετε ήδη δημιουργήσει, επιλέξτε τη σύνδεση υπηρεσίες Cloud στην αριστερή πλευρά της σελίδας.

1. Επιλέξτε το **+** εικονίδιο στην κάτω αριστερή γωνία και, στη συνέχεια, επιλέξτε **Μια υπηρεσία Cloud** στο μενού που εμφανίζεται. Εμφανίζεται μια άλλη οθόνη με δύο επιλογές, **Γρήγορης δημιουργίας** και **Δημιουργήστε προσαρμοσμένες**. Εάν επιλέξετε **Γρήγορης δημιουργίας**, μπορείτε να δημιουργήσετε μια υπηρεσία cloud απλώς, καθορίζοντας τη διεύθυνση URL και της περιοχής όπου αυτό θα φυσικά βρίσκεται. Εάν επιλέξετε να **Δημιουργήσετε προσαρμοσμένες**, μπορείτε να δημοσιεύσετε αμέσως μια υπηρεσία cloud, καθορίζοντας ένα πακέτο (αρχείο .cspkg), ένα αρχείο ρύθμισης παραμέτρων (.cscfg) και ένα πιστοποιητικό. Δημιουργία προσαρμοσμένου δεν απαιτείται εάν σκοπεύετε να δημοσιεύσετε την υπηρεσία cloud, χρησιμοποιώντας την εντολή **Δημοσίευση** σε ένα έργο Azure. Η εντολή " **Δημοσίευση** " είναι διαθέσιμη στο μενού συντόμευσης για ένα έργο Azure.

1. Επιλέξτε **Γρήγορης δημιουργίας** αργότερα δημοσίευσης την υπηρεσία cloud με τη χρήση του Visual Studio.

1. Καθορίστε ένα όνομα για την υπηρεσία cloud. Την πλήρη διεύθυνση URL που εμφανίζεται δίπλα στο όνομα.

1. Στη λίστα, επιλέξτε την περιοχή όπου βρίσκονται οι περισσότεροι από τους χρήστες.

1. Στο κάτω μέρος του παραθύρου, επιλέξτε τη σύνδεση **Δημιουργία υπηρεσία στο Cloud** .

## <a name="create-a-storage-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης

Ένας λογαριασμός χώρου αποθήκευσης παρέχει πρόσβαση στις υπηρεσίες Blob, ουρά και πίνακα. Μπορείτε να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης με χρήση του Visual Studio ή του [Azure κλασική πύλη](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Για να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης με χρήση του Visual Studio

1. Στην **Εξερεύνηση λύσεων**, ανοίξτε το μενού συντόμευσης για τον κόμβο **χώρου αποθήκευσης** και, στη συνέχεια, επιλέξτε **Δημιουργία λογαριασμού χώρου αποθήκευσης**.

    ![Δημιουργία νέου λογαριασμού Azure χώρου αποθήκευσης](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)

1. Επιλέξτε ή πληκτρολογήστε τις ακόλουθες πληροφορίες για το νέο λογαριασμό του χώρου αποθήκευσης στο παράθυρο διαλόγου **Δημιουργία λογαριασμού χώρου αποθήκευσης** .
    - Το Azure συνδρομή στην οποία θέλετε να προσθέσετε το λογαριασμό χώρου αποθήκευσης.
    - Το όνομα που θέλετε να χρησιμοποιήσετε για το νέο λογαριασμό του χώρου αποθήκευσης.
    - Η περιοχή ή ομάδα συσχέτισης (όπως Δυτική ΗΠΑ ή Ανατολικής Ασίας).
    - Ο τύπος της αναπαραγωγής που θέλετε να χρησιμοποιήσετε για το λογαριασμό χώρου αποθήκευσης, όπως παν πλεονάζοντα.

1. Όταν ολοκληρώσετε τη διαδικασία, επιλέξτε **Δημιουργία**. Το νέο λογαριασμό του χώρου αποθήκευσης εμφανίζεται στη λίστα **χώρου αποθήκευσης** στο **Διακομιστή Explorer**.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Για να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης, χρησιμοποιώντας την πύλη κλασική του Azure

1. Είσοδος στην [πύλη του Azure κλασική](http://go.microsoft.com/fwlink/?LinkId=253103) στην τοποθεσία Web της Microsoft.

1. (Προαιρετικό) Για να προβάλετε τους λογαριασμούς σας στο χώρο αποθήκευσης, επιλέξτε τη σύνδεση του **χώρου αποθήκευσης** στον πίνακα στην αριστερή πλευρά της σελίδας.

1. Στην κάτω αριστερή γωνία της σελίδας, επιλέξτε το **+** εικονίδιο.

1. Στο μενού που εμφανίζεται, επιλέξτε **χώρου αποθήκευσης**και, στη συνέχεια, επιλέξτε **Γρήγορης δημιουργίας**.

1. Δώστε ένα όνομα που θα έχει ως αποτέλεσμα ένα μοναδικό url το λογαριασμό χώρου αποθήκευσης.

1. Δώστε την υπηρεσία cloud ένα όνομα. Την πλήρη διεύθυνση URL που εμφανίζεται δίπλα στο όνομα.

1. Στη λίστα των περιοχών, επιλέξτε μια περιοχή όπου βρίσκονται οι περισσότεροι από τους χρήστες.

1. Καθορίστε εάν θέλετε να ενεργοποιήσετε την αναπαραγωγή παν. Εάν ενεργοποιήσετε παν αναπαραγωγής, τα δεδομένα σας θα αποθηκευτούν σε πολλές θέσεις φυσικής για να μειώσετε την πιθανότητα των ζημιών. Αυτή η δυνατότητα κάνει πιο καταναλώνοντας χώρο αποθήκευσης, αλλά μπορείτε να μειώσετε το κόστος, ενεργοποιώντας παν θέση όταν δημιουργείτε το λογαριασμό χώρου αποθήκευσης αντί για να προσθέσετε τη δυνατότητα αργότερα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [παν αναπαραγωγής](http://go.microsoft.com/fwlink/?LinkId=253108).

1. Στο κάτω μέρος του παραθύρου, επιλέξτε τη σύνδεση **Δημιουργία λογαριασμού χώρου αποθήκευσης** .

Αφού δημιουργήσετε το λογαριασμό χώρου αποθήκευσης, θα δείτε τις διευθύνσεις URL που μπορείτε να χρησιμοποιήσετε για να αποκτήσετε πρόσβαση σε πόρους σε κάθε μία από τις υπηρεσίες Azure χώρου αποθήκευσης, καθώς και των πλήκτρων πρόσβασης κύριας και δευτερεύουσας για το λογαριασμό σας. Μπορείτε να χρησιμοποιήσετε τα πλήκτρα για τον έλεγχο ταυτότητας αιτήματα σε σχέση με τις υπηρεσίες του χώρου αποθήκευσης.

>[AZURE.NOTE] Το κλειδί δευτερεύοντα access παρέχει την ίδια πρόσβαση στο λογαριασμό σας χώρο αποθήκευσης ως access πρωτεύον κλειδί και δημιουργείται ως αντίγραφο ασφαλείας, θα πρέπει να παραβιαστεί κλειδιού κύρια πρόσβαση. Επιπλέον, συνιστάται να αναδημιουργήσετε το πλήκτρα πρόσβασης σε τακτική βάση. Μπορείτε να τροποποιήσετε μια ρύθμιση συμβολοσειρά σύνδεσης για να χρησιμοποιήσετε το δευτερεύον κλειδί, ενώ μπορείτε να αναδημιουργήσετε το πρωτεύον κλειδί, στη συνέχεια, μπορείτε να το τροποποιήσετε για να χρησιμοποιήσετε το πρωτεύον κλειδί δημιουργηθεί ξανά, ενώ μπορείτε να αναδημιουργήσετε το δευτερεύον κλειδί.

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Ρύθμιση παραμέτρων την εφαρμογή σας για να χρησιμοποιήσετε τις υπηρεσίες που παρέχονται από το λογαριασμό χώρου αποθήκευσης

Πρέπει να ρυθμίσετε οποιονδήποτε ρόλο που έχει πρόσβαση σε υπηρεσίες αποθήκευσης για τη χρήση των υπηρεσιών Azure χώρου αποθήκευσης που έχετε δημιουργήσει. Για να το κάνετε αυτό, μπορείτε να χρησιμοποιήσετε πολλές ρυθμίσεις παραμέτρων υπηρεσίας για το έργο σας Azure. Από προεπιλογή, δύο δημιουργούνται στο έργο σας Azure. Με πολλές ρυθμίσεις παραμέτρων υπηρεσίας, μπορείτε να χρησιμοποιήσετε την ίδια συμβολοσειρά σύνδεσης στον κώδικά σας αλλά έχουν μια διαφορετική τιμή για μια συμβολοσειρά σύνδεσης σε κάθε ρύθμιση παραμέτρων της υπηρεσίας. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε μία ρύθμιση παραμέτρων της υπηρεσίας για να εκτελέσετε και εντοπισμός σφαλμάτων την εφαρμογή σας τοπικά χρησιμοποιώντας το προσομοίωσης Azure χώρου αποθήκευσης και ρύθμιση παραμέτρων διαφορετική υπηρεσία για να δημοσιεύσετε την εφαρμογή με το Azure. Για περισσότερες πληροφορίες σχετικά με τις ρυθμίσεις παραμέτρων υπηρεσίας, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων σας Azure έργου με χρήση πολλών υπηρεσίας ρυθμίσεις παραμέτρων](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Για να ρυθμίσετε την εφαρμογή σας για να χρησιμοποιήσετε τις υπηρεσίες που παρέχει το λογαριασμό χώρου αποθήκευσης

1. Στο Visual Studio, ανοίξτε τη λύση Azure σας. Στην Εξερεύνηση λύσεων, ανοίξτε το μενού συντόμευσης για κάθε ρόλο στο έργο σας Azure που αποκτά πρόσβαση τις υπηρεσίες του χώρου αποθήκευσης και επιλέξτε **Ιδιότητες**. Μια σελίδα με το όνομα του ρόλου εμφανίζεται στο πρόγραμμα επεξεργασίας Visual Studio. Η σελίδα εμφανίζει τα πεδία για την καρτέλα **ρύθμισης παραμέτρων** .

1. Στις σελίδες ιδιοτήτων για το ρόλο, επιλέξτε **Ρυθμίσεις**.

1. Στη λίστα **Ρύθμιση παραμέτρων της υπηρεσίας** , επιλέξτε το όνομα της ρύθμισης παραμέτρων της υπηρεσίας που θέλετε να επεξεργαστείτε. Εάν θέλετε να κάνετε αλλαγές σε όλες τις ρυθμίσεις παραμέτρων υπηρεσίας για τον συγκεκριμένο ρόλο, μπορείτε να επιλέξετε **Όλες τις παραμέτρους**.  Για περισσότερες πληροφορίες σχετικά με το πώς μπορείτε να ενημερώσετε τις ρυθμίσεις παραμέτρων υπηρεσίας, ανατρέξτε στην ενότητα **Διαχείριση συμβολοσειρές σύνδεσης για τους λογαριασμούς χώρου αποθήκευσης** στο θέμα [Ρύθμιση παραμέτρων τους ρόλους για μια υπηρεσία Cloud Azure με το Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Για να τροποποιήσετε τις ρυθμίσεις συμβολοσειρά σύνδεσης, επιλέξτε το **...** κουμπί δίπλα στο στοιχείο τη ρύθμιση. Εμφανίζεται το παράθυρο διαλόγου **Δημιουργία συμβολοσειράς σύνδεσης χώρο αποθήκευσης** .

1. Στην περιοχή **σύνδεση με χρήση**, ενεργοποιήστε την επιλογή **τη συνδρομή σας** .

1. Στη λίστα **συνδρομής** , επιλέξτε τη συνδρομή σας. Εάν η λίστα συνδρομών δεν περιλαμβάνει αυτό που θέλετε, επιλέξτε τη σύνδεση **Ρυθμίσεων δημοσίευση λήψης** .

1. Στη λίστα **όνομα λογαριασμού** , επιλέξτε το όνομα του λογαριασμού χώρου αποθήκευσης. Εργαλεία Azure λαμβάνει αυτόματα τα διαπιστευτήρια λογαριασμού χώρου αποθήκευσης, χρησιμοποιώντας το αρχείο .publishsettings. Για να καθορίσετε τα διαπιστευτήριά σας λογαριασμό χώρου αποθήκευσης με μη αυτόματο τρόπο, ενεργοποιήστε την επιλογή που **έχουν εισαχθεί μη αυτόματα τα διαπιστευτήρια** και, στη συνέχεια, συνεχίστε με αυτήν τη διαδικασία. Μπορείτε να λάβετε το όνομα λογαριασμού χώρου αποθήκευσης και το πρωτεύον κλειδί από την [πύλη του Azure κλασική](http://go.microsoft.com/fwlink/p/?LinkID=213885). Εάν δεν θέλετε να καθορίσετε το χώρο αποθήκευσης ρυθμίσεις λογαριασμού με μη αυτόματο τρόπο, επιλέξτε το κουμπί **OK** για να κλείσετε το παράθυρο διαλόγου.

1. Επιλέξτε τη σύνδεση διαπιστευτήρια **λογαριασμού χώρου αποθήκευσης Enter** .

1. Στο πλαίσιο **όνομα λογαριασμού** , πληκτρολογήστε το όνομα του λογαριασμού σας χώρου αποθήκευσης.

    >[AZURE.NOTE] Συνδεθείτε με την [πύλη του Azure κλασική](http://go.microsoft.com/fwlink/?LinkID=213885)και, στη συνέχεια, επιλέξτε το κουμπί " **Αποθήκευση** ". Η πύλη Εμφανίζει μια λίστα με τους λογαριασμούς χώρου αποθήκευσης. Εάν επιλέξετε ένα λογαριασμό, ανοίγει μια σελίδα για αυτό. Μπορείτε να αντιγράψετε το όνομα του λογαριασμού χώρου αποθήκευσης από αυτήν τη σελίδα. Εάν χρησιμοποιείτε μια προηγούμενη έκδοση του κλασική πύλη, το όνομα του λογαριασμού σας χώρο αποθήκευσης εμφανίζεται στην προβολή **Λογαριασμούς χώρου αποθήκευσης** . Για να αντιγράψετε αυτό το όνομα, επισημάνετε το στο παράθυρο **Ιδιότητες** αυτής της προβολής και, στη συνέχεια, επιλέξτε τα πλήκτρα Ctrl-C. Για να επικολλήσετε το όνομα στο Visual Studio, επιλέξτε το πλαίσιο κειμένου **όνομα λογαριασμού** και, στη συνέχεια, επιλέξτε τα πλήκτρα Ctrl + V.

1. Στο πλαίσιο **κλειδί λογαριασμού** , το πρωτεύον κλειδί, πληκτρολογήστε ή αντιγράψτε και επικολλήστε την από την [πύλη του Azure κλασική](http://go.microsoft.com/fwlink/?LinkID=213885).
    Για να αντιγράψετε αυτό το κλειδί:

    1. Στο κάτω μέρος της σελίδας για το λογαριασμό κατάλληλη αποθήκευση, επιλέξτε το κουμπί **Διαχείριση κλειδιών** .

    1. Στη σελίδα **Διαχείριση πλήκτρα πρόσβασης** , επιλέξτε το κείμενο του access πρωτεύοντος κλειδιού και, στη συνέχεια, επιλέξτε τα πλήκτρα Ctrl + C.

    1. Στα εργαλεία Azure, επικολλήστε τον αριθμό-κλειδί στο πλαίσιο **κλειδί λογαριασμού** .

    1. Πρέπει να επιλέξετε μία από τις παρακάτω επιλογές για να καθορίσετε πώς θα έχουν πρόσβαση στο λογαριασμό του χώρου αποθήκευσης της υπηρεσίας:
        - **Χρήση HTTP**. Αυτή είναι η τυπική επιλογή. Για παράδειγμα, `http://<account name>.blob.core.windows.net`.
        - **Χρήση HTTPS** για ασφαλή σύνδεση. Για παράδειγμα, `https://<accountname>.blob.core.windows.net`.
        - **Καθορισμός προσαρμοσμένου τελικά σημεία** για κάθε μία από τις τρεις υπηρεσίες. Στη συνέχεια, μπορείτε να πληκτρολογήσετε αυτά τα τελικά σημεία στο πεδίο για τη συγκεκριμένη υπηρεσία.

        >[AZURE.NOTE] Εάν δημιουργείτε προσαρμοσμένα τελικά σημεία, μπορείτε να δημιουργήσετε μια πιο σύνθετη συμβολοσειρά σύνδεσης. Όταν χρησιμοποιείτε αυτήν τη μορφή συμβολοσειρά, μπορείτε να καθορίσετε τελικά σημεία υπηρεσία αποθήκευσης που περιλαμβάνουν ένα προσαρμοσμένο όνομα τομέα που έχετε καταχωρήσει για το λογαριασμό χώρου αποθήκευσης με την υπηρεσία Blob. Επίσης, μπορείτε να εκχωρήσετε πρόσβαση μόνο σε πόρους blob σε ένα μεμονωμένο κοντέινερ μέσω υπογραφής κοινόχρηστη πρόσβαση. Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προσαρμοσμένου τελικά σημεία, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων Azure συμβολοσειρές σύνδεσης χώρου αποθήκευσης](storage-configure-connection-string.md).

1. Για να αποθηκεύσετε αυτές τις αλλαγές συμβολοσειρά σύνδεσης, επιλέξτε το κουμπί **OK** και, στη συνέχεια, επιλέξτε το κουμπί **Αποθήκευση** στη γραμμή εργαλείων. Αφού αποθηκεύσετε αυτές τις αλλαγές, μπορείτε να λάβετε την τιμή αυτής της συμβολοσειράς σύνδεσης στον κώδικά σας με τη χρήση [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Όταν δημοσιεύετε την εφαρμογή με το Azure, επιλέξτε τη ρύθμιση παραμέτρων της υπηρεσίας που περιέχει το λογαριασμό Azure χώρου αποθήκευσης για τη συμβολοσειρά σύνδεσης. Μετά τη δημοσίευση την εφαρμογή σας, βεβαιωθείτε ότι η εφαρμογή λειτουργεί όπως αναμένεται σε σχέση με τις υπηρεσίες Azure χώρου αποθήκευσης

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με τη δημοσίευση εφαρμογών Azure από το Visual Studio, ανατρέξτε στο θέμα [δημοσίευση μια υπηρεσία στο Cloud χρησιμοποιώντας τα εργαλεία Azure](vs-azure-tools-publishing-a-cloud-service.md).
