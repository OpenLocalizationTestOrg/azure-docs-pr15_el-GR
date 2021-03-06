<properties
   pageTitle="Azure έργα Visual Studio ομάδα πόρων | Microsoft Azure"
   description="Χρησιμοποιήστε το Visual Studio για να δημιουργήσετε ένα ομαδικό έργο Azure πόρων και ανάπτυξη των πόρων Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="tomfitz" />

# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Δημιουργία και ανάπτυξη ομάδων πόρων Azure μέσω του Visual Studio

Με το Visual Studio και το [Azure SDK](https://azure.microsoft.com/downloads/), μπορείτε να δημιουργήσετε ένα έργο που αναπτύσσει τον υποδομή και τον κωδικό για Azure. Για παράδειγμα, μπορείτε να ορίσετε το κεντρικό web, τοποθεσία web και βάσης δεδομένων για την εφαρμογή σας, και να υλοποιήσετε υποδομής μαζί με τον κωδικό. Εναλλακτικά, μπορείτε να ορίσετε μια εικονική μηχανή, εικονικού δικτύου και το λογαριασμό χώρου αποθήκευσης και ανάπτυξη υποδομής μαζί με μια δέσμη ενεργειών που εκτελείται στον υπολογιστή εικονική. Το έργο ανάπτυξη **Ομάδα πόρων Azure** σάς επιτρέπει να αναπτύξετε όλους τους πόρους που απαιτείται σε μια ενιαία, επαναλαμβανόμενη λειτουργία. Για περισσότερες πληροφορίες σχετικά με την ανάπτυξη και τη διαχείριση των πόρων σας, ανατρέξτε στο θέμα [Επισκόπηση της διαχείρισης πόρων Azure](azure-resource-manager/resource-group-overview.md).

Ομάδα πόρων Azure έργα περιέχει πρότυπα Azure JSON για τη διαχείριση πόρων, τα οποία ορίζουν τους πόρους που αναπτύσσετε σε Azure. Για να μάθετε σχετικά με τα στοιχεία του προτύπου για τη διαχείριση πόρων, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](resource-group-authoring-templates.md). Visual Studio σάς επιτρέπει να επεξεργαστείτε αυτά τα πρότυπα και παρέχει εργαλεία που θα απλοποιήσει με την εργασία με τα πρότυπα.

Σε αυτό το θέμα, μπορείτε να αναπτύξετε μια εφαρμογή web και βάση δεδομένων SQL. Ωστόσο, τα βήματα είναι σχεδόν ίδια για οποιονδήποτε τύπο πόρου. Μπορείτε να ως εύκολα αναπτύξετε μια εικονική μηχανή και των σχετικών πόρων. Visual Studio παρέχει πολλά starter διαφορετικά πρότυπα για την ανάπτυξη συνηθισμένα σενάρια.

Αυτό το άρθρο παρουσιάζει Visual Studio 2015 ενημερωμένη έκδοση 2 και Microsoft Azure SDK για .NET 2.9. Εάν χρησιμοποιείτε το Visual Studio 2013 με Azure SDK 2.9, την εμπειρία σας είναι σε μεγάλο βαθμό το ίδιο. Μπορείτε να χρησιμοποιήσετε εκδόσεις του SDK Azure από 2.6 ή νεότερη έκδοση; Ωστόσο, την εμπειρία σας με το περιβάλλον εργασίας χρήστη ενδέχεται να είναι διαφορετικές από το περιβάλλον εργασίας χρήστη που εμφανίζονται σε αυτό το άρθρο. Συνιστάται να εγκαταστήσετε την πιο πρόσφατη έκδοση του [Azure SDK](https://azure.microsoft.com/downloads/) πριν να ξεκινήσετε τα βήματα. 

## <a name="create-azure-resource-group-project"></a>Δημιουργία ομάδας πόρων Azure έργου

Σε αυτήν τη διαδικασία, μπορείτε να δημιουργήσετε ένα έργο Azure ομάδα πόρων με ένα πρότυπο **Web app + SQL** .

1. Στο Visual Studio, επιλέξτε **αρχείο**, το **Νέο έργο**, επιλέξτε **C#** ή **Visual Basic**. Στη συνέχεια, επιλέξτε **Cloud**και, στη συνέχεια, επιλέξτε **Ομάδα πόρων Azure** έργου.

    ![Σύννεφο ανάπτυξη του έργου](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Επιλέξτε το πρότυπο που θέλετε να αναπτύξετε για τη διαχείριση πόρων Azure. Ειδοποίηση είναι πολλές διαφορετικές επιλογές με βάση τον τύπο του έργου που θέλετε να αναπτύξετε. Για αυτό το θέμα, επιλέξτε το πρότυπο **εφαρμογής Web + SQL** .

    ![Επιλέξτε ένα πρότυπο](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Το πρότυπο που επιλέγετε είναι απλώς ένα σημείο εκκίνησης; Μπορείτε να προσθέσετε και να καταργήσετε πόρους για να ικανοποιεί το σενάριό σας.

    >[AZURE.NOTE] Visual Studio ανακτά μια λίστα με τα διαθέσιμα πρότυπα online. Ενδέχεται να αλλάξουν τη λίστα.

    Visual Studio δημιουργεί ένα έργο ανάπτυξης ομάδα πόρων για την εφαρμογή web και τη βάση δεδομένων SQL.

1. Για να δείτε τι που δημιουργήσατε, αναπτύξτε τους κόμβους του έργου ανάπτυξης.

    ![Εμφάνιση κόμβοι](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Επειδή το επιλέγουμε το Web app + πρότυπο SQL για αυτό το παράδειγμα, μπορείτε να δείτε τα ακόλουθα αρχεία: 

  	|Όνομα αρχείου|Περιγραφή|
  	|---|---|
  	|Ανάπτυξη AzureResourceGroup.ps1|Μια δέσμη ενεργειών του PowerShell που ενεργοποιεί τις εντολές του PowerShell για να αναπτύξετε για τη διαχείριση πόρων Azure.<br />**Σημείωση** Visual Studio χρησιμοποιεί αυτήν τη δέσμη ενεργειών του PowerShell για να αναπτύξετε το πρότυπό σας. Οι αλλαγές που κάνετε σε αυτήν τη δέσμη ενεργειών επηρεάζουν ανάπτυξης στο Visual Studio, ώστε να είστε προσεκτικοί.|
  	|WebSiteSQLDatabase.json|Το πρότυπο διαχείρισης πόρων που καθορίζει την υποδομή θέλετε ανάπτυξη Azure και οι παράμετροι που μπορείτε να παράσχετε κατά την ανάπτυξη. Καθορίζει επίσης τις εξαρτήσεις μεταξύ τους πόρους, ώστε να διαχείριση πόρων αναπτύσσει τους πόρους στη σωστή σειρά.|
  	|WebSiteSQLDatabase.parameters.json|Ένα αρχείο παραμέτρων που περιέχει τιμές που απαιτούνται από το πρότυπο. Περάσετε σε τιμές παραμέτρων για να προσαρμόσετε κάθε ανάπτυξης.|

    Όλα τα έργα ανάπτυξης ομάδα πόρων περιέχει αυτά τα βασικά αρχεία. Άλλα έργα ενδέχεται να περιέχουν επιπλέον αρχεία υποστήριξης άλλες λειτουργίες.

## <a name="customize-the-resource-manager-template"></a>Προσαρμογή του προτύπου για τη διαχείριση πόρων

Μπορείτε να προσαρμόσετε ένα έργο ανάπτυξης τροποποιώντας τα πρότυπα JSON που περιγράφουν τους πόρους που θέλετε να αναπτύξετε. JSON σημαίνει σημειογραφίας αντικειμένων JavaScript και είναι μια μορφή αλληλουχίας δεδομένων που είναι εύκολο να εργαστείτε. Τα αρχεία JSON Χρησιμοποιήστε ένα σχήμα που μπορείτε να κάνετε αναφορά στο επάνω μέρος κάθε αρχείο. Εάν θέλετε να κατανοήσετε το σχήμα, μπορείτε να κάνετε λήψη και να αναλύσετε. Το σχήμα ορίζει τα στοιχεία είναι έγκυρες, τους τύπους και τις μορφές των πεδίων, οι πιθανές τιμές Αριθμημένου τιμών και ούτω καθεξής. Για να μάθετε σχετικά με τα στοιχεία του προτύπου για τη διαχείριση πόρων, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](resource-group-authoring-templates.md).

Για να εργαστείτε στο δικό σας πρότυπο, ανοίξτε **WebSiteSQLDatabase.json**.

Πρόγραμμα επεξεργασίας Visual Studio παρέχει εργαλεία για να σας βοηθήσει με την επεξεργασία του προτύπου για τη διαχείριση πόρων. Το παράθυρο **Διάρθρωσης JSON** διευκολύνει για να δείτε τα στοιχεία που ορίζονται στο πρότυπό σας.

![Εμφάνιση JSON διάρθρωσης](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Επιλογή οποιοδήποτε από τα στοιχεία στη διάρθρωση σας μεταφέρει στο συγκεκριμένο τμήμα του προτύπου και επισημαίνει το αντίστοιχο JSON.

![περιηγηθείτε JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Μπορείτε να προσθέσετε έναν πόρο, είτε επιλέγοντας το κουμπί **Προσθήκη πόρων** στο επάνω μέρος του παραθύρου διάρθρωσης JSON ή κάνοντας δεξί κλικ **πόρους** και επιλέγοντας **Προσθήκη νέου πόρου**.

![Προσθήκη πόρου](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Για αυτό το πρόγραμμα εκμάθησης, επιλέξτε **Το λογαριασμό χώρου αποθήκευσης** και να της δώσετε ένα όνομα. Δώστε ένα όνομα που είναι λιγότερα από 11 χαρακτήρες και να περιέχει μόνο αριθμούς και πεζά γράμματα.

![Προσθήκη χώρου αποθήκευσης](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Παρατηρήστε ότι ήταν όχι μόνο ο πόρος που προσθέσατε, αλλά επίσης μια παράμετρο για ο τύπος λογαριασμού χώρου αποθήκευσης και μια μεταβλητή για το όνομα του λογαριασμού χώρου αποθήκευσης.

![Εμφάνιση διάρθρωσης](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Η παράμετρος **storageType** είναι προκαθορισμένες με επιτρεπόμενων τύπων και έναν προεπιλεγμένο τύπο. Μπορείτε να αφήσετε αυτές τις τιμές ή την επεξεργασία τους για το σενάριό σας. Εάν δεν θέλετε όλα τα άτομα για να αναπτύξετε ένα λογαριασμό χώρου αποθήκευσης **Premium_LRS** μέσω αυτού του προτύπου, καταργήστε την από το επιτρεπόμενων τύπων. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studio παρέχει επίσης intellisense για να σας βοηθήσει να κατανοήσετε ποιες ιδιότητες είναι διαθέσιμες κατά την επεξεργασία του προτύπου. Για παράδειγμα, για να επεξεργαστείτε τις ιδιότητες για το πρόγραμμα εφαρμογής υπηρεσίας, μεταβείτε στον πόρο **HostingPlan** και προσθέστε μια τιμή για τις **Ιδιότητες**. Παρατηρήστε το intellisense που εμφανίζει τις διαθέσιμες τιμές και παρέχει μια περιγραφή για αυτήν την τιμή.

![Εμφάνιση intellisense](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Μπορείτε να ορίσετε **numberOfWorkers** σε 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## <a name="deploy-the-resource-group-project-to-azure"></a>Αναπτύξτε το έργο ομάδα πόρων για να Azure

Τώρα είστε έτοιμοι να αναπτύξετε το έργο σας. Κατά την ανάπτυξη ενός έργου, η ομάδα πόρων Azure, τον αναπτύσσετε σε μια ομάδα Azure πόρων. Η ομάδα πόρων είναι μια λογική ομαδοποίηση των πόρων που έχουν ένα κοινό κύκλου ζωής.

1. Στο μενού συντόμευσης του κόμβου ανάπτυξη του έργου, επιλέξτε **Ανάπτυξη** > **Νέας ανάπτυξης**.

    ![Ανάπτυξη, το στοιχείο μενού νέας ανάπτυξης](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Εμφανίζεται το παράθυρο διαλόγου **ανάπτυξη σε ομάδα πόρων** .

    ![Ανάπτυξη στο παράθυρο διαλόγου ομάδα πόρων](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. Στο πλαίσιο αναπτυσσόμενης λίστας " **ομάδα πόρων** ", επιλέξτε μια υπάρχουσα ομάδα πόρων ή δημιουργήστε ένα νέο. Για να δημιουργήσετε μια ομάδα πόρων, ανοίξτε το αναπτυσσόμενο πλαίσιο " **Ομάδα πόρων** " και επιλέξτε **Δημιουργία νέου**.

    ![Ανάπτυξη στο παράθυρο διαλόγου ομάδα πόρων](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Εμφανίζεται το παράθυρο διαλόγου **Δημιουργία ομάδας πόρων** . Δώστε την ομάδα σας ένα όνομα και μια θέση και κάντε κλικ στο κουμπί **Δημιουργία** .

    ![Δημιουργία πλαισίου διαλόγου ομάδα πόρων](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Επεξεργαστείτε τις παραμέτρους για την ανάπτυξη, επιλέγοντας το κουμπί **Επεξεργασία παραμέτρους** .

    ![Κουμπί "Επεξεργασία" παράμετροι](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)

1. Δώστε τις τιμές για τις παραμέτρους κενή και κάντε κλικ στο κουμπί **Αποθήκευση** . Οι παράμετροι κενή είναι **hostingPlanName**, **administratorLogin**, **administratorLoginPassword**και **όνομα βάσης δεδομένων**.

    **hostingPlanName** καθορίζει ένα όνομα για το [πρόγραμμα εφαρμογής υπηρεσίας](./app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) για να δημιουργήσετε. 
    
    **administratorLogin** Καθορίζει το όνομα χρήστη για το διαχειριστή του SQL Server. Μην χρησιμοποιείτε ονόματα κοινών διαχείρισης όπως **σα** ή το **διαχειριστή**. 
    
    Η **administratorLoginPassword** καθορίζει έναν κωδικό πρόσβασης για το διαχειριστή του SQL Server. Η επιλογή **Αποθήκευση κωδικών πρόσβασης ως απλού κειμένου στο αρχείο παραμέτρων** δεν είναι ασφαλή; Επομένως, μην επιλέξετε αυτήν την επιλογή. Επειδή ο κωδικός πρόσβασης δεν αποθηκεύεται ως απλό κείμενο, θα πρέπει να δώσετε αυτόν τον κωδικό πρόσβασης ξανά κατά την ανάπτυξη. 
    
    **όνομα βάσης δεδομένων** καθορίζει ένα όνομα για τη βάση δεδομένων για να δημιουργήσετε. 

    ![Παράθυρο διαλόγου παράμετροι επεξεργασία](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
1. Επιλέξτε το κουμπί **ανάπτυξης** για να αναπτύξετε το έργο σε Azure. Ανοίγει κονσόλα PowerShell εκτός της παρουσίας του Visual Studio. Πληκτρολογήστε τον κωδικό πρόσβασης διαχειριστή του SQL Server στην κονσόλα PowerShell όταν σας ζητηθεί. **Κονσόλα σας PowerShell μπορεί να αποκρύπτεται πίσω από άλλα στοιχεία ή ελαχιστοποιημένο στη γραμμή εργασιών.** Αναζήτηση για αυτήν την κονσόλα και επιλέξτε για να εισαγάγετε τον κωδικό πρόσβασης.

    >[AZURE.NOTE] Visual Studio μπορεί να σας ζητήσει να εγκαταστήσετε το cmdlet του Azure PowerShell. Χρειάζεστε τα cmdlet Azure PowerShell για να υλοποιήσετε τις ομάδες πόρων. Εάν σας ζητηθεί, εγκαταστήστε τα.
    
1. Ανάπτυξη ενδέχεται να χρειαστούν μερικά λεπτά. Στο παράθυρο **εξόδου** , μπορείτε να δείτε την κατάσταση της ανάπτυξης. Όταν ολοκληρωθεί η ανάπτυξη, το τελευταίο μήνυμα που υποδεικνύει μια επιτυχημένη ανάπτυξη με το κάτι παρόμοιο με:

        ... 
        18:00:58 - Successfully deployed template 'c:\users\user\documents\visual studio 2015\projects\azureresourcegroup1\azureresourcegroup1\templates\websitesqldatabase.json' to resource group 'DemoSiteGroup'.


1. Σε ένα πρόγραμμα περιήγησης, ανοίξτε την [πύλη του Azure](https://portal.azure.com/) και πραγματοποιήστε είσοδο στο λογαριασμό σας. Για να δείτε την ομάδα των πόρων, επιλέξτε **ομάδες πόρων** και την ομάδα των πόρων που αναπτυχθεί σε.

    ![Επιλέξτε την ομάδα](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Μπορείτε να δείτε όλους τους πόρους που αναπτύσσονται. Παρατηρήστε ότι το όνομα του λογαριασμού χώρου αποθήκευσης δεν είναι ακριβώς τι που καθορίσατε κατά την προσθήκη αυτού του πόρου. Το λογαριασμό χώρου αποθήκευσης πρέπει να είναι μοναδικό. Το πρότυπο προσθέτει αυτόματα μια συμβολοσειρά χαρακτήρων για το όνομα που παρέχονται για να δώσετε ένα μοναδικό όνομα. 

    ![Εμφάνιση πόρων](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Εάν κάνετε αλλαγές και θέλετε να αναπτύξετε εκ νέου το έργο σας, επιλέξτε την υπάρχουσα ομάδα πόρων από το μενού συντόμευσης του Azure πόρων ομάδα έργου. Στο μενού συντόμευσης, επιλέξτε **Ανάπτυξη**και, στη συνέχεια, επιλέξτε την ομάδα των πόρων που αναπτύσσεται.

    ![Ομάδα Azure πόρων αναπτυχθεί](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Ανάπτυξη κώδικα με την υποδομή σας

Σε αυτό το σημείο, έχετε αναπτύξει την υποδομή για την εφαρμογή σας, αλλά υπάρχει πραγματική κωδικός αναπτυχθεί με το έργο. Αυτό το θέμα δείχνει πώς μπορείτε να αναπτύξετε μια εφαρμογή web και πινάκων βάσης δεδομένων SQL κατά τη διάρκεια της ανάπτυξης. Εάν αναπτύσσετε μια εικονική μηχανή αντί για μια εφαρμογή web, που θέλετε να εκτελέσετε ορισμένες κώδικα στον υπολογιστή ως μέρος της ανάπτυξης. Η διαδικασία για την ανάπτυξη κώδικα για μια εφαρμογή web ή για τη ρύθμιση μια εικονική μηχανή είναι σχεδόν ίδια.

1. Προσθέστε ένα έργο στη λύση σας Visual Studio. Κάντε δεξί κλικ της λύσης και επιλέξτε **Προσθήκη** > **Νέο έργο**.

    ![Προσθήκη έργου](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. Προσθήκη μιας **εφαρμογής ASP.NET Web**. 

    ![Προσθήκη εφαρμογής web](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Επιλέξτε **MVC** και καταργήστε το πεδίο για τον **κεντρικό υπολογιστή στο cloud** , επειδή το έργο ομάδα πόρων εκτελεί αυτήν την εργασία.

    ![Επιλέξτε MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Μετά τη Visual Studio δημιουργεί την εφαρμογή web σας, μπορείτε να δείτε και τα δύο έργα στη λύση.

    ![Εμφάνιση έργων](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Τώρα, πρέπει να βεβαιωθείτε ότι το έργο σας ομάδα πόρων είναι γνωρίζετε το νέο έργο. Επιστρέψτε στο έργο σας ομάδα πόρων (AzureResourceGroup1). Κάντε δεξί κλικ σε **αναφορές** και επιλέξτε **Προσθήκη αναφοράς**.

    ![Προσθήκη αναφοράς](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Επιλέξτε το έργο εφαρμογής web που δημιουργήσατε.

    ![Προσθήκη αναφοράς](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Προσθέτοντας μια αναφορά, σύνδεση το έργο εφαρμογής web με το έργο ομάδα πόρων και να ρυθμίσει αυτόματα τρεις βασικές ιδιότητες. Μπορείτε να δείτε αυτές τις ιδιότητες στο παράθυρο **διαλόγου Ιδιότητες** για την αναφορά.

      ![ανατρέξτε στο θέμα αναφορά](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
    
    Οι ιδιότητες είναι:

    - Οι **Πρόσθετες ιδιότητες** περιέχει το πακέτο ανάπτυξης web ενδιάμεσου θέση που προωθείται το χώρο αποθήκευσης Azure. Σημείωση το φάκελο (ExampleApp) και το αρχείο (package.zip). Που παρέχει αυτές τις τιμές ως παραμέτρους κατά την ανάπτυξη της εφαρμογής. 
    - Η **Διαδρομή του αρχείου περιλαμβάνουν** περιέχει τη διαδρομή όπου έχει δημιουργηθεί το πακέτο. Η **Συμπερίληψη των προορισμών** περιέχει την εντολή που εκτελεί την ανάπτυξη. 
    - Δημιουργία της προεπιλεγμένης τιμής του **; Πακέτο** σας δίνει τη δυνατότητα ανάπτυξης για τη δημιουργία και δημιουργία πακέτου ανάπτυξης web (package.zip).  
    
    Δεν χρειάζεται ένα προφίλ δημοσίευση ως ανάπτυξη λαμβάνει τις απαραίτητες πληροφορίες από τις ιδιότητες για να δημιουργήσετε το πακέτο.
      
1. Προσθήκη πόρου στο πρότυπο.

    ![Προσθήκη πόρου](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Αυτήν τη στιγμή, επιλέξτε **Ανάπτυξη Web για τις εφαρμογές Web**. 

    ![Προσθήκη περιεχομένου web ανάπτυξη](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Αναπτύξτε ξανά το έργο σας ομάδα πόρων για την ομάδα των πόρων. Αυτήν τη στιγμή υπάρχουν ορισμένες παράμετροι νέα. Δεν χρειάζεται να παράσχετε τιμές για **_artifactsLocation** ή **_artifactsLocationSasToken** επειδή το Visual Studio δημιουργεί αυτόματα αυτές τις τιμές. Ωστόσο, πρέπει να ορίσετε το φάκελο και το όνομα αρχείου στη διαδρομή που περιέχει το πακέτο ανάπτυξης (εμφανίζεται ως **ExampleAppPackageFolder** και **ExampleAppPackageFileName** στην παρακάτω εικόνα). Παρέχουν τις τιμές που είδατε προηγουμένως στις ιδιότητες αναφοράς (**ExampleApp** και **package.zip**).

    ![Προσθήκη περιεχομένου web ανάπτυξη](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    Για το **λογαριασμό χώρου αποθήκευσης αντικείμενο**, επιλέξτε μία αναπτυχθεί με αυτήν την ομάδα πόρων.
    
1. Αφού ολοκληρωθεί η ανάπτυξη, επιλέξτε την εφαρμογή web σας στην πύλη. Επιλέξτε τη διεύθυνση URL για να μεταβείτε στην τοποθεσία.

    ![Αναζήτηση τοποθεσίας](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Παρατηρήστε ότι έχετε αναπτύξει με επιτυχία την προεπιλεγμένη εφαρμογή ASP.NET.

    ![Εμφάνιση ανεπτυγμένος εφαρμογής](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε περισσότερα σχετικά με τη διαχείριση των πόρων σας μέσω της πύλης, ανατρέξτε στο θέμα [με την πύλη Azure για να διαχειριστείτε τους πόρους σας Azure](./azure-portal/resource-group-portal.md).
- Για να μάθετε περισσότερα σχετικά με τα πρότυπα, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](resource-group-authoring-templates.md).
