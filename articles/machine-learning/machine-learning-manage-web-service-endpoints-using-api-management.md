<properties
    pageTitle="Μάθετε πώς μπορείτε να διαχειριστείτε υπηρεσίες web AzureML που χρησιμοποιούν API διαχείρισης | Microsoft Azure"
    description="Οδηγός που δείχνει πώς μπορείτε να διαχειριστείτε AzureML υπηρεσίες web με χρήση API διαχείρισης."
    keywords="μηχανικής εκμάθησης, api διαχείρισης"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="roalexan" />


# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>Μάθετε πώς μπορείτε να διαχειριστείτε υπηρεσίες web AzureML που χρησιμοποιούν API διαχείρισης

##<a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός εμφανίζει τον τρόπο γρήγορα αποτελέσματα με την υπηρεσία διαχείρισης API για να διαχειριστείτε τις υπηρεσίες web AzureML.

##<a name="what-is-azure-api-management"></a>Τι είναι η διαχείριση API Azure;

Azure API διαχείρισης είναι μια υπηρεσία Azure που σας επιτρέπει να διαχειρίζεστε τα τελικά σημεία REST API σας με τον ορισμό πρόσβασης των χρηστών, χρήση περιορισμού και παρακολούθηση πίνακα εργαλείων. Κάντε κλικ [εδώ](https://azure.microsoft.com/services/api-management/) για λεπτομέρειες σχετικά με τη Διαχείριση API Azure. Κάντε κλικ [εδώ](../api-management/api-management-get-started.md) για οδηγίες σχετικά με τον τρόπο για να ξεκινήσετε με το Azure API διαχείρισης. Αυτό άλλες τον οδηγό, σύμφωνα με αυτόν τον οδηγό, καλύπτονται περισσότερα θέματα, όπως ρυθμίσεις παραμέτρων ειδοποίησης, σειρά τις τιμές, χειρισμού απόκρισης, έλεγχος ταυτότητας χρήστη, τη δημιουργία προϊόντα, συνδρομές προγραμματιστής και χρήση dashboarding.

##<a name="what-is-azureml"></a>Τι είναι το AzureML;

AzureML είναι μια υπηρεσία Azure για μηχανικής εκμάθησης που σας επιτρέπει να εύκολα δημιουργία, ανάπτυξης και κοινή χρήση λύσεων προηγμένη ανάλυση. Κάντε κλικ [εδώ](https://azure.microsoft.com/services/machine-learning/) για λεπτομέρειες σχετικά με AzureML.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε αυτόν τον οδηγό, πρέπει:

* Ένας λογαριασμός Azure. Εάν δεν έχετε λογαριασμό Azure, κάντε κλικ [εδώ](https://azure.microsoft.com/pricing/free-trial/) για λεπτομέρειες σχετικά με τον τρόπο για να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης.
* Ένας λογαριασμός AzureML. Εάν δεν έχετε ένα λογαριασμό AzureML, κάντε κλικ [εδώ](https://studio.azureml.net/) για λεπτομέρειες σχετικά με τον τρόπο για να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης.
* Το χώρο εργασίας, την υπηρεσία και api_key για μια έρευνα AzureML αναπτυχθεί ως υπηρεσία web. Κάντε κλικ [εδώ](machine-learning-create-experiment.md) για λεπτομέρειες σχετικά με τον τρόπο για να δημιουργήσετε μια έρευνα AzureML. Κάντε κλικ [εδώ](machine-learning-publish-a-machine-learning-web-service.md) για λεπτομέρειες σχετικά με τον τρόπο για την ανάπτυξη μιας έρευνας AzureML ως υπηρεσία web. Εναλλακτικά, παράρτημα A περιλαμβάνει οδηγίες για τον τρόπο δημιουργίας και δοκιμάστε μια απλή AzureML έρευνας και αναπτύξετε ως μια υπηρεσία web.

##<a name="create-an-api-management-instance"></a>Δημιουργήστε μια παρουσία API διαχείρισης

Ακολουθούν τα βήματα για τη χρήση του API διαχείρισης για τη διαχείριση της υπηρεσίας web AzureML. Πρώτα, δημιουργήστε μια παρουσία της υπηρεσίας. Συνδεθείτε [Πύλη κλασική](https://manage.windowsazure.com/) και κάντε κλικ στην επιλογή **Δημιουργία** > **Εφαρμογή υπηρεσιών** > **API διαχείρισης** > **Δημιουργία**.

![Δημιουργία παρουσίας](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Καθορίστε ένα μοναδικό **διεύθυνση URL**. Αυτός ο οδηγός χρησιμοποιεί **demoazureml** -θα πρέπει να επιλέξετε κάτι διαφορετικό. Επιλέξτε την επιθυμητή **συνδρομής** και **περιοχής** για την παρουσία της υπηρεσίας. Αφού κάνετε τις επιλογές σας, κάντε κλικ στο κουμπί Επόμενο.

![Δημιουργία-υπηρεσία-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Καθορίστε μια τιμή για το **Όνομα της εταιρείας**. Αυτός ο οδηγός χρησιμοποιεί **demoazureml** -θα πρέπει να επιλέξετε κάτι διαφορετικό. Εισαγάγετε τη διεύθυνση ηλεκτρονικού ταχυδρομείου στο πεδίο **διαχειριστής ηλεκτρονικού ταχυδρομείου** . Χρησιμοποιείται αυτή η διεύθυνση ηλεκτρονικού ταχυδρομείου για τις ειδοποιήσεις από το σύστημα διαχείρισης API.

![Δημιουργία-υπηρεσία-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Επιλέξτε το πλαίσιο ελέγχου για να δημιουργήσετε την παρουσία της υπηρεσίας. *Χρειάζονται έως και 30 λεπτά για μια νέα υπηρεσία θα δημιουργηθεί*.

##<a name="create-the-api"></a>Δημιουργήστε το API

Όταν δημιουργηθεί η παρουσία της υπηρεσίας, το επόμενο βήμα είναι να δημιουργήσετε το API. API αποτελείται από ένα σύνολο των λειτουργιών που μπορούν να ενεργοποιηθούν από μια εφαρμογή προγράμματος-πελάτη. Λειτουργίες API είναι μέσω διακομιστή μεσολάβησης σε υπάρχουσες υπηρεσίες web. Αυτός ο οδηγός δημιουργεί APIs που διακομιστή μεσολάβησης με τις υπάρχουσες υπηρεσίες web στο AzureML και BES.

API έχουν δημιουργηθεί και έχει ρυθμιστεί από την πύλη publisher API, το οποίο έχετε πρόσβαση μέσω της πύλης κλασική Azure. Για να συνδεθούν με την πύλη του publisher, επιλέξτε την παρουσία της υπηρεσίας.

![Επιλέξτε εμφάνιση υπηρεσίας](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Κάντε κλικ στην επιλογή **Διαχείριση** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API.

![Διαχείριση υπηρεσίας](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

Κάντε κλικ στην επιλογή **APIs** από το **API διαχείρισης** μενού στα αριστερά και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη API**.

![μενού διαχείρισης API](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Πληκτρολογήστε **AzureML επίδειξη API** ως το **όνομα του Web API**. Πληκτρολογήστε **https://ussouthcentral.services.azureml.net** ως τη **διεύθυνση URL της υπηρεσίας Web**. Πληκτρολογήστε **azureml επίδειξη** ως το **επίθημα διεύθυνση URL API Web**. Επιλέξτε **HTTPS** ως το συνδυασμό **Διεύθυνση URL API Web** . Επιλέξτε **Starter** ως **προϊόντα**. Όταν τελειώσετε, κάντε κλικ στο κουμπί **Αποθήκευση** για να δημιουργήσετε το API.

![Προσθήκη-νέα-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##<a name="add-the-operations"></a>Προσθέστε τις λειτουργίες

Κάντε κλικ στην επιλογή **Προσθήκη λειτουργίας** για να προσθέσετε εργασίες σε αυτό το API.

![Προσθήκη λειτουργίας](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Θα εμφανιστεί το παράθυρο **νέα λειτουργία** και θα είναι επιλεγμένη στην καρτέλα **υπογραφή** από προεπιλογή.

##<a name="add-rrs-operation"></a>Η λειτουργία στο προσθήκης

Πρώτα, δημιουργήστε μια λειτουργία για την υπηρεσία στο AzureML. Επιλέξτε **ΚΑΤΑΧΏΡΗΣΗ** ως το **Ρηματικές HTTP**. Τύπος **/services/ /workspaces/ {χώρου εργασίας} {υπηρεσία} / execute?api έκδοσης = {apiversion} & λεπτομέρειες = {λεπτομέρειες}** ως τη **διεύθυνση URL προτύπου**. Πληκτρολογήστε **Εκτέλεση στο** ως το **εμφανιζόμενο όνομα**.

![Προσθήκη στο-λειτουργίας-υπογραφής](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Κάντε κλικ στο κουμπί **αποκρίσεις** > **ΠΡΟΣΘΉΚΗ** στη γραμμή αριστερά και επιλέξτε **200 OK**. Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε αυτήν τη λειτουργία.

![Προσθήκη στο-λειτουργίας-απόκρισης](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##<a name="add-bes-operations"></a>Προσθήκη BES λειτουργίες

Στιγμιότυπα οθόνης δεν περιλαμβάνονται για τις λειτουργίες BES όπως είναι παρόμοια με εκείνα για την προσθήκη της λειτουργίας στο.

###<a name="submit-but-not-start-a-batch-execution-job"></a>Υποβολή (αλλά δεν ξεκινούν) μιας εργασίας εκτέλεσης δέσμης

Κάντε κλικ στην επιλογή **Προσθήκη λειτουργίας** για να προσθέσετε τη λειτουργία AzureML BES για το API. Επιλέξτε **ΚΑΤΑΧΏΡΗΣΗ** για το **Ρηματικές HTTP**. Τύπος **/services/ /workspaces/ {χώρου εργασίας} {υπηρεσία} / jobs?api έκδοσης = {apiversion}** για τη **διεύθυνση URL προτύπου**. Για το **εμφανιζόμενο όνομα**, πληκτρολογήστε **BES υποβολή** . Κάντε κλικ στο κουμπί **αποκρίσεις** > **ΠΡΟΣΘΉΚΗ** στη γραμμή αριστερά και επιλέξτε **200 OK**. Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε αυτήν τη λειτουργία.

###<a name="start-a-batch-execution-job"></a>Έναρξη μιας εργασίας εκτέλεσης δέσμης

Κάντε κλικ στην επιλογή **Προσθήκη λειτουργίας** για να προσθέσετε τη λειτουργία AzureML BES για το API. Επιλέξτε **ΚΑΤΑΧΏΡΗΣΗ** για το **Ρηματικές HTTP**. Τύπος **/jobs/ /services/ {υπηρεσία} /workspaces/ {χώρου εργασίας} {jobid} / start?api έκδοσης = {apiversion}** για τη **διεύθυνση URL προτύπου**. Για το **εμφανιζόμενο όνομα**, πληκτρολογήστε **BES Έναρξη** . Κάντε κλικ στο κουμπί **αποκρίσεις** > **ΠΡΟΣΘΉΚΗ** στη γραμμή αριστερά και επιλέξτε **200 OK**. Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε αυτήν τη λειτουργία.

###<a name="get-the-status-or-result-of-a-batch-execution-job"></a>Λάβετε την κατάσταση ή το αποτέλεσμα μιας εργασίας εκτέλεσης δέσμης

Κάντε κλικ στην επιλογή **Προσθήκη λειτουργίας** για να προσθέσετε τη λειτουργία AzureML BES για το API. Επιλέξτε **ΛΉΨΗ** για το **Ρηματικές HTTP**. Τύπος **/workspaces/ {χώρου εργασίας} {υπηρεσία} /services/ /jobs/ {jobid} ?api έκδοσης = {apiversion}** για τη **διεύθυνση URL προτύπου**. Πληκτρολογήστε **BES κατάστασης** για το **εμφανιζόμενο όνομα**. Κάντε κλικ στο κουμπί **αποκρίσεις** > **ΠΡΟΣΘΉΚΗ** στη γραμμή αριστερά και επιλέξτε **200 OK**. Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε αυτήν τη λειτουργία.

###<a name="delete-a-batch-execution-job"></a>Διαγραφή μιας εργασίας εκτέλεσης δέσμης

Κάντε κλικ στην επιλογή **Προσθήκη λειτουργίας** για να προσθέσετε τη λειτουργία AzureML BES για το API. Επιλέξτε " **ΔΙΑΓΡΑΦΉ** " για το **Ρηματικές HTTP**. Τύπος **/workspaces/ {χώρου εργασίας} {υπηρεσία} /services/ /jobs/ {jobid} ?api έκδοσης = {apiversion}** για τη **διεύθυνση URL προτύπου**. Για το **εμφανιζόμενο όνομα**, πληκτρολογήστε **BES διαγραφή** . Κάντε κλικ στο κουμπί **αποκρίσεις** > **ΠΡΟΣΘΉΚΗ** στη γραμμή αριστερά και επιλέξτε **200 OK**. Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε αυτήν τη λειτουργία.

##<a name="call-an-operation-from-the-developer-portal"></a>Κλήση μιας λειτουργίας από την πύλη για προγραμματιστές

Λειτουργίες μπορεί να ονομάζεται απευθείας από την πύλη για προγραμματιστές, η οποία παρέχει έναν εύχρηστο τρόπο για να προβάλετε και να ελέγξετε τις λειτουργίες του API. Σε αυτό το βήμα οδηγός θα καλέσετε τη μέθοδο **Εκτέλεση στο** που προστέθηκε για το **API επίδειξη AzureML**. Κάντε κλικ στην επιλογή **πύλη για προγραμματιστές** από το μενού στην επάνω δεξιά γωνία της κλασικής πύλη του.

![πύλη για προγραμματιστές](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

Κάντε κλικ στην επιλογή **APIs** από το επάνω μενού και, στη συνέχεια, κάντε κλικ στην επιλογή **AzureML επίδειξη API** για να δείτε τις διαθέσιμες λειτουργίες.

![demoazureml api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Επιλέξτε **Εκτέλεση στο** για τη λειτουργία. Κάντε κλικ στην επιλογή **Δοκιμή**.

![δοκιμή-it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

Παράμετροι αίτησης, πληκτρολογήστε το **χώρο εργασίας**, **υπηρεσία**, **2.0** για την **apiversion**και **true** για τις **Λεπτομέρειες**. Μπορείτε να βρείτε το **χώρο εργασίας** και **υπηρεσία** στον πίνακα εργαλείων υπηρεσίας web AzureML (ανατρέξτε στο θέμα **Έλεγχος της υπηρεσίας web** στο παράρτημα A).

Για κεφαλίδες αίτησης, κάντε κλικ στην επιλογή **Προσθήκη κεφαλίδας** και πληκτρολογήστε **Τύπου περιεχομένου** και **εφαρμογή/json**, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη κεφαλίδας** και πληκτρολογήστε **εξουσιοδότησης** και **φορέα <YOUR AZUREML SERVICE API-KEY> **. Μπορείτε να βρείτε το **api κλειδί** στον πίνακα εργαλείων υπηρεσίας web AzureML (ανατρέξτε στο θέμα **Έλεγχος της υπηρεσίας web** στο παράρτημα A).

Τύπος **{"Εισόδων": {"input1": {"ColumnNames": ["Στηλ2"], "τιμές": [["Αυτή είναι μια καλή ημέρα"]]}}, "GlobalParameters": {}}** για το σώμα της αίτησης.

![azureml-επίδειξη-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Κάντε κλικ στο κουμπί **Αποστολή**.

![Αποστολή](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Αφού μια λειτουργία ενεργοποιείται, την πύλη για προγραμματιστές εμφανίζει το **Ζητήθηκε διεύθυνση URL** από την υπηρεσία υποστήριξης, την **κατάσταση απόκρισης**, τις **κεφαλίδες απόκρισης**και οποιοδήποτε **περιεχόμενο απόκρισης**.

![κατάσταση απόκρισης](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##<a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Παράρτημα A - δημιουργία και έλεγχος μια απλή AzureML υπηρεσία web

###<a name="creating-the-experiment"></a>Δημιουργία έρευνας του

Παρακάτω παρατίθενται τα βήματα για τη δημιουργία μιας απλής AzureML έρευνας και την ανάπτυξή του ως υπηρεσία web. Η μεταφέρει υπηρεσίας web ως εισαγωγής στήλης αυθαίρετο κειμένου και επιστρέφει ένα σύνολο δυνατοτήτων που αναπαρίσταται ως ακέραιοι. Για παράδειγμα:

Κείμενο | Διαγραμμισμένο κειμένου
--- | ---
Αυτή είναι μια καλή ημέρα | 1 1 2 2 0 2 0 1

Πρώτα, χρησιμοποιώντας ένα πρόγραμμα περιήγησης της επιλογής σας, μεταβείτε στη: [https://studio.azureml.net/](https://studio.azureml.net/) και εισαγάγετε τα διαπιστευτήριά σας για να συνδεθείτε. Στη συνέχεια, δημιουργήστε μια νέα κενή έρευνας.

![Αναζήτηση-έρευνας-πρότυπα](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Μετονομάστε το σε **SimpleFeatureHashingExperiment**. Ανάπτυξη **Αποθηκευτεί συνόλων δεδομένων** και σύρετε **Βιβλίο αναθεωρήσεις από το Amazon** στη σας έρευνας.

![απλή-δυνατότητα-ο κατακερματισμός-έρευνας](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Αναπτύξτε το **Μετασχηματισμό δεδομένων** και **χειρισμού** και σύρετε **Επιλογή στηλών στο σύνολο δεδομένων** στη σας έρευνας. Σύνδεση **βιβλίου αναθεωρήσεις από το Amazon** για να **επιλέξετε στήλες στο σύνολο δεδομένων**.

![Επιλογή στηλών](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Κάντε κλικ στην επιλογή **Επιλογή στηλών στο σύνολο δεδομένων** και, στη συνέχεια, κάντε κλικ στην επιλογή **Εκκίνηση επιλογής στήλης** και επιλέξτε **Στηλ2**. Επιλέξτε το σημάδι επιλογής για να εφαρμοστούν αυτές οι αλλαγές.

![Επιλογή στηλών](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Ανάπτυξη **Ανάλυσης κειμένου** και σύρετε **Τη δυνατότητα ο κατακερματισμός** στη την έρευνα. Η σύνδεση **Επιλέξτε στήλες στο σύνολο δεδομένων** με **δυνατότητα ο κατακερματισμός**.

![σύνδεση-έργου-στηλών](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Πληκτρολογήστε **3** για το **Hashing bitsize**. Αυτό θα δημιουργήσει 8 (23) στήλες.

![ο κατακερματισμός bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Σε αυτό το σημείο, μπορείτε να κάντε κλικ στο κουμπί **Εκτέλεση** για να ελέγξετε την έρευνα.

![εκτέλεση](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###<a name="create-a-web-service"></a>Δημιουργία μιας υπηρεσίας web

Τώρα μπορείτε να δημιουργήσετε μια υπηρεσία web. Αναπτύξτε το στοιχείο **Υπηρεσίας Web** και σύρετε **εισαγωγής** στη σας έρευνας. Σύνδεση **ο κατακερματισμός δυνατότητα** **εισαγωγής** . Επίσης σύρετε **εξόδου** σας έρευνας. Σύνδεση **ο κατακερματισμός δυνατότητα** **εξόδου** .

![αποτέλεσμα-σε-δυνατότητα-ο κατακερματισμός](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Κάντε κλικ στην επιλογή **Δημοσίευση υπηρεσία web**.

![δημοσίευση-υπηρεσία web](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Κάντε κλικ στο κουμπί **Ναι** για να δημοσιεύσετε την έρευνα.

![Ναι για να δημοσιεύσετε](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###<a name="test-the-web-service"></a>Δοκιμή της υπηρεσίας web

Μια υπηρεσία web AzureML αποτελείται από RSS (υπηρεσία αίτηση/απάντηση) και BES τα τελικά σημεία (υπηρεσία εκτέλεσης δέσμης). RSS είναι σύγχρονη εκτέλεση. BES είναι για εκτέλεση ασύγχρονης εργασία. Για να δοκιμάσετε την υπηρεσία web με την προέλευση Python δείγματος παρακάτω, ίσως χρειαστεί να κάνετε λήψη και εγκατάσταση του SDK Azure Python (ανατρέξτε στο θέμα: [Πώς να εγκαταστήσετε το Python](../python-how-to-install.md)).

Θα πρέπει επίσης το **χώρο εργασίας**, **την υπηρεσία**και **api_key** της έρευνας σας για το παρακάτω δείγμα προέλευση. Μπορείτε να βρείτε την υπηρεσία και χώρου εργασίας κάνοντας κλικ στην επιλογή **Αίτηση/απάντηση** ή **Δέσμη εκτέλεσης** για την έρευνα στον πίνακα εργαλείων υπηρεσία web.

![Εύρεση χώρου εργασίας-και-υπηρεσίας](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

Μπορείτε να βρείτε το **api_key** κάνοντας κλικ στην επιλογή σας έρευνας στον πίνακα εργαλείων υπηρεσία web.

![Εύρεση-api-κλειδί](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####<a name="test-rrs-endpoint"></a>Τελικό σημείο στο δοκιμής

#####<a name="test-button"></a>Κουμπί δοκιμής

Ένας εύκολος τρόπος για να ελέγξετε το τελικό σημείο στο είναι να κάντε κλικ στην επιλογή **Δοκιμή** στον πίνακα εργαλείων υπηρεσία web.

![δοκιμή](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

Πληκτρολογήστε **αυτή είναι μια καλή ημέρα** για **Στηλ2**. Επιλέξτε το σημάδι ελέγχου.

![Εισαγωγή δεδομένων](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Θα δείτε κάτι παρόμοιο με το

![δείγμα εξόδου](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####<a name="sample-code"></a>Δείγμα κώδικα

Ένας άλλος τρόπος για να ελέγξετε το στο είναι από τον υπολογιστή-πελάτη σας κώδικα. Εάν κάνετε κλικ στο κουμπί **αίτηση/απάντηση** στον πίνακα εργαλείων και κάντε κύλιση προς τα κάτω, θα δείτε δείγμα κώδικα για C#, Python και R. Μπορείτε, επίσης, θα δείτε τη σύνταξη της αίτησης στο, συμπεριλαμβανομένης της αίτησης URI, επικεφαλίδες και σώμα.

Αυτός ο οδηγός εμφανίζει ένα παράδειγμα Python εργασία. Θα πρέπει να το τροποποιήσετε με το **χώρο εργασίας**, **την υπηρεσία**και **api_key** από την έρευνα.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####<a name="test-bes-endpoint"></a>Τελικό σημείο BES δοκιμής
Κάντε κλικ στην επιλογή **Εκτέλεση δέσμης** στον πίνακα εργαλείων και κάντε κύλιση προς τα κάτω. Θα δείτε δείγμα κώδικα για C#, Python και R. Μπορείτε, επίσης, θα δείτε τη σύνταξη των αιτήσεων BES για να υποβάλετε μια εργασία, Έναρξη μιας εργασίας, γρήγορα την κατάσταση ή τα αποτελέσματα μιας εργασίας και να διαγράψετε μια εργασία.

Αυτός ο οδηγός εμφανίζει ένα παράδειγμα Python εργασία. Πρέπει να το τροποποιήσετε με το **χώρο εργασίας**, **την υπηρεσία**και **api_key** από την έρευνα. Επιπλέον, πρέπει να τροποποιήσετε το **όνομα του λογαριασμού χώρου αποθήκευσης**, **κλειδί λογαριασμού χώρου αποθήκευσης**και **το όνομα του κοντέινερ χώρου αποθήκευσης**. Τέλος, θα πρέπει να τροποποιήσετε τη θέση του **αρχείου εισαγωγής** και τη θέση του στο **αρχείο εξόδου**.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
