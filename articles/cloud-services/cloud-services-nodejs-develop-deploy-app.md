<properties
    pageTitle="Οδηγός για γρήγορα node.js αποτελέσματα | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια απλή εφαρμογή web Node.js και αναπτύσσετε σε μια υπηρεσία Azure cloud."
    services="cloud-services"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Δημιουργήστε και αναπτύξτε μια εφαρμογή Node.js σε μια υπηρεσία Cloud Azure

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να δημιουργήσετε μια απλή εφαρμογή Node.js που εκτελούνται σε μια υπηρεσία Cloud Azure. Υπηρεσίες cloud είναι τα δομικά στοιχεία του cloud με τις εφαρμογές στο Azure. Επιτρέπουν την διαχωρισμού και ανεξάρτητη διαχείρισης και ελέγχου κλίμακα στοιχεία προσκηνίου και παρασκηνίου της εφαρμογής σας.  Υπηρεσίες cloud παρέχουν ένα ισχυρό αποκλειστικό εικονικό μηχάνημα για τη φιλοξενία αξιόπιστα κάθε ρόλο.

Για περισσότερες πληροφορίες σχετικά με τις υπηρεσίες Cloud και πώς συγκρίνουν σε τοποθεσίες Web Azure και εικονικές μηχανές, ανατρέξτε στο θέμα [Azure τοποθεσίες Web, τις υπηρεσίες Cloud και εικονικές μηχανές σύγκρισης].

>[AZURE.TIP] Θέλετε να δημιουργήσετε μια απλή τοποθεσία Web; Εάν το σενάριό σας περιλαμβάνει μόνο μια απλή τοποθεσία Web προσκηνίου, μπορείτε να [χρησιμοποιήσετε μια εφαρμογή web ελαφριά]. Μπορείτε εύκολα να αναβαθμίσετε σε μια υπηρεσία Cloud καθώς εξελίσσεται η εφαρμογή web της και να αλλάξετε τις απαιτήσεις σας.

Ακολουθώντας αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να δημιουργήσετε μια εφαρμογή απλό web που φιλοξενείται μέσα σε ένα ρόλο web. Που θα χρησιμοποιήσετε το προσομοίωσης υπολογισμού για να δοκιμάσετε τοπικά την εφαρμογή σας, στη συνέχεια, ανάπτυξη χρησιμοποιώντας τα εργαλεία της γραμμής εντολών του PowerShell.

Η εφαρμογή είναι μια απλή εφαρμογή "hello world":

![Ένα πρόγραμμα περιήγησης web που εμφανίζει την ιστοσελίδα Γεια][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί Azure PowerShell, την οποία απαιτεί Windows.

- Εγκατάσταση και ρύθμιση παραμέτρων [Azure Powershell].
- Κάντε λήψη και εγκατάσταση του [SDK Azure για το .NET 2.7]. Κατά την εγκατάσταση την εγκατάσταση, επιλέξτε:
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>Δημιουργία ενός έργου, η υπηρεσία Cloud Azure

Εκτελέστε τις ακόλουθες εργασίες για να δημιουργήσετε ένα νέο έργο Azure υπηρεσία Cloud, μαζί με βασικές ικριώματος Node.js:

1. Εκτέλεση **του Windows PowerShell** ως διαχειριστής; από το **Μενού Έναρξη** ή η **Οθόνη έναρξης**, αναζήτηση για το **Windows PowerShell**.

2. [Σύνδεση του PowerShell] για τη συνδρομή σας.

3. Εισαγάγετε το παρακάτω cmdlet του PowerShell για τη δημιουργία για να δημιουργήσετε το έργο:

        New-AzureServiceProject helloworld

    ![Το αποτέλεσμα της εντολής New-AzureService helloworld][The result of the New-AzureService helloworld command]

    Το cmdlet **New-AzureServiceProject** δημιουργεί μια βασική δομή για τη δημοσίευση μιας εφαρμογής Node.js σε μια υπηρεσία Cloud. Περιέχει αρχεία ρύθμισης παραμέτρων που είναι απαραίτητα για τη δημοσίευση Azure. Το cmdlet αλλάζει επίσης τον κατάλογο εργασίας στον κατάλογο για την υπηρεσία.

    Το cmdlet δημιουργεί τα ακόλουθα αρχεία:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** και **ServiceDefinition.csdef**: Azure αφορούν συγκεκριμένα αρχεία είναι απαραίτητο για τη δημοσίευση της εφαρμογής σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση της δημιουργίας μιας υπηρεσίας φιλοξενούνται για Azure].

    -   **deploymentSettings.json**: αποθηκεύει τις τοπικές ρυθμίσεις που χρησιμοποιούνται από τα cmdlet του PowerShell Azure ανάπτυξης.

4.  Πληκτρολογήστε την παρακάτω εντολή για να προσθέσετε ένα νέο ρόλο web:

        Add-AzureNodeWebRole

    ![Το αποτέλεσμα της εντολής Προσθήκη AzureNodeWebRole][The output of the Add-AzureNodeWebRole command]

    Το cmdlet **Προσθήκη AzureNodeWebRole** δημιουργεί μια βασική εφαρμογή Node.js. Τροποποιεί επίσης τα αρχεία **.csfg** και **.csdef** για να προσθέσετε καταχωρήσεις ρύθμισης παραμέτρων για το νέο ρόλο.

    > [AZURE.NOTE] Εάν δεν καθορίσετε ένα όνομα ρόλου, χρησιμοποιείται ένα προεπιλεγμένο όνομα. Μπορείτε να παρέχετε ένα όνομα ως η πρώτη παράμετρος cmdlet:`Add-AzureNodeWebRole MyRole`

Η εφαρμογή Node.js έχει οριστεί στο το αρχείο **server.js**, που βρίσκεται στον κατάλογο του ρόλου web (**WebRole1** από προεπιλογή). Ακολουθεί ο κώδικας:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Αυτός ο κώδικας είναι ουσιαστικά το ίδιο με το δείγμα "Γεια" στην τοποθεσία Web του [nodejs.org] , εκτός από το βήμα που χρησιμοποιεί τον αριθμό θύρας που έχουν εκχωρηθεί από το cloud περιβάλλον.

## <a name="deploy-the-application-to-azure"></a>Ανάπτυξη της εφαρμογής για να Azure

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>Κάντε λήψη του Azure ρυθμίσεις δημοσίευσης

Για να αναπτύξετε την εφαρμογή με το Azure, πρέπει πρώτα να λάβετε τις ρυθμίσεις δημοσίευσης για τη συνδρομή σας Azure.

1.  Εκτελέστε το ακόλουθο cmdlet του PowerShell Azure:

        Get-AzurePublishSettingsFile

    Αυτό θα χρησιμοποιήσει το πρόγραμμα περιήγησης για να μεταβείτε στη σελίδα λήψης ρυθμίσεων δημοσίευση. Ίσως σας ζητηθεί να συνδεθείτε με ένα λογαριασμό Microsoft. Εάν Ναι, χρησιμοποιήστε το λογαριασμό που σχετίζεται με τη συνδρομή σας στο Azure.

    Αποθηκεύστε το προφίλ που έχουν ληφθεί σε μια θέση αρχείων, μπορείτε να έχετε εύκολα πρόσβαση.

2.  Εκτελέστε την παρακάτω cmdlet για να εισαγάγετε το προφίλ δημοσίευσης που λάβατε:

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Μετά την εισαγωγή των ρυθμίσεων δημοσίευση, μπορείτε να διαγράψετε το αρχείο λήψης .publishSettings, επειδή περιέχει πληροφορίες που θα μπορούσε να επιτρέπεται η πρόσβαση στο λογαριασμό σας.

### <a name="publish-the-application"></a>Δημοσίευση της εφαρμογής

Για να δημοσιεύσετε, εκτελέστε τις εξής εντολές:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **Όνομα_υπηρεσίας -** Καθορίζει το όνομα για την ανάπτυξη. Πρέπει να είναι ένα μοναδικό όνομα, διαφορετικά θα αποτύχει η διαδικασία δημοσίευσης. Η εντολή **Get-Date** πρόκες σε μια συμβολοσειρά ημερομηνίας/ώρας που θα πρέπει να βεβαιωθείτε ότι το όνομα μοναδικών τιμών.

- **-Θέση** Καθορίζει το κέντρο δεδομένων που η εφαρμογή θα βρίσκεται στο. Για να δείτε μια λίστα με τις διαθέσιμες κέντρα δεδομένων, χρησιμοποιήστε το cmdlet **Get-AzureLocation** .

- **-Εκκίνηση** ανοίγει ένα παράθυρο προγράμματος περιήγησης και σας μεταφέρει την υπηρεσία μετά την ολοκλήρωση της ανάπτυξης.

Μετά τη δημοσίευση ολοκληρωθεί με επιτυχία, θα δείτε μια απόκριση παρόμοια με τα εξής:

![Το αποτέλεσμα της εντολής AzureService δημοσίευση][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Μπορεί να χρειαστούν αρκετά λεπτά για την εφαρμογή για να αναπτύξετε και να γίνονται διαθέσιμα κατά την πρώτη δημοσίευση.

Μόλις ολοκληρωθεί η ανάπτυξη, ένα παράθυρο προγράμματος περιήγησης θα ανοίξει και να μεταβείτε στην υπηρεσία cloud.

![Ένα παράθυρο προγράμματος περιήγησης που εμφανίζει σελίδα κόσμο Γεια σας. η διεύθυνση URL υποδεικνύει τη σελίδα φιλοξενείται στον Azure.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

Η εφαρμογή σας εκτελείται τώρα στην Azure.

Το cmdlet **Δημοσίευση AzureServiceProject** εκτελεί τα ακόλουθα βήματα:

1.  Δημιουργεί ένα πακέτο για την ανάπτυξη. Το πακέτο περιλαμβάνει όλα τα αρχεία στο φάκελο εφαρμογής.

2.  Δημιουργεί ένα νέο **λογαριασμό χώρου αποθήκευσης** , εάν δεν υπάρχει. Ο λογαριασμός Azure αποθήκευσης χρησιμοποιείται για την αποθήκευση του πακέτου εφαρμογών κατά την ανάπτυξη. Μπορείτε να διαγράψετε το λογαριασμό χώρου αποθήκευσης με ασφάλεια αφού ολοκληρωθεί η ανάπτυξη.

3.  Δημιουργεί μια νέα **υπηρεσία cloud** , εάν δεν υπάρχει ήδη. Μια **υπηρεσία cloud** είναι το κοντέινερ στην οποία φιλοξενείται εφαρμογή σας όταν το έχει αναπτυχθεί σε Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση της δημιουργίας μιας υπηρεσίας φιλοξενούνται για Azure].

4.  Δημοσιεύει του πακέτου ανάπτυξης του Azure.


## <a name="stopping-and-deleting-your-application"></a>Διακοπή και διαγράφοντας την εφαρμογή σας

Μετά την ανάπτυξη της εφαρμογής σας, μπορεί να θέλετε να απενεργοποιήσετε την, ώστε να μπορείτε να αποφύγετε επιπλέον κόστους. Λογαριασμοί Azure web ρόλο παρουσίες ανά ώρα διακομιστή χρόνο που καταναλώθηκε. Χρόνος διακομιστή καταναλώνεται όταν έχει αναπτυχθεί την εφαρμογή σας, ακόμα και αν οι εμφανίσεις δεν λειτουργούν και βρίσκονται σε κατάσταση διακοπής.

1.  Στο παράθυρο του Windows PowerShell, διακόψετε την ανάπτυξη υπηρεσίας που δημιουργήσατε στην προηγούμενη ενότητα με το ακόλουθο cmdlet:

        Stop-AzureService

    Διακοπή της υπηρεσίας ενδέχεται να χρειαστούν αρκετά λεπτά. Όταν η υπηρεσία έχει διακοπεί, λαμβάνετε ένα μήνυμα που δηλώνει ότι έχει διακοπεί.

    ![Η κατάσταση της εντολής διακοπή AzureService][The status of the Stop-AzureService command]

2.  Για να διαγράψετε την υπηρεσία, καλέστε το ακόλουθο cmdlet:

        Remove-AzureService

    Όταν σας ζητηθεί, πληκτρολογήστε **Y** για να διαγράψετε την υπηρεσία.

    Διαγραφή της υπηρεσίας ενδέχεται να χρειαστούν αρκετά λεπτά. Μετά τη διαγραφή της υπηρεσίας λαμβάνετε ένα μήνυμα που δηλώνει ότι η υπηρεσία έχει διαγραφεί.

    ![Η κατάσταση της εντολής κατάργηση AzureService][The status of the Remove-AzureService command]

    > [AZURE.NOTE] Διαγραφή της υπηρεσίας δεν διαγράφει το λογαριασμό χώρου αποθήκευσης που δημιουργήθηκε όταν η υπηρεσία αρχικά δημοσιεύτηκε και θα συνεχίσετε να χρεωθεί για χώρος αποθήκευσης που χρησιμοποιείται. Εάν κανένα άλλο χρησιμοποιεί το χώρο αποθήκευσης, μπορείτε να τη διαγράψετε.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές του Node.js].

<!-- URL List -->

[Azure τοποθεσίες Web, τις υπηρεσίες Cloud και εικονικές μηχανές σύγκρισης]: ../app-service-web/choose-web-site-cloud-service-vm.md
[χρήση εφαρμογής web ελαφριά]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure Powershell]: ../powershell-install-configure.md
[Azure SDK για .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Σύνδεση του PowerShell]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Επισκόπηση της δημιουργίας μια υπηρεσία για Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[Κέντρο για προγραμματιστές του node.js]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
