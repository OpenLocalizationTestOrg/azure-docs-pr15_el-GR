<properties 
    pageTitle="Σύνδεση με ένα λογαριασμό DocumentDB με πρωτόκολλο υποστήριξη για MongoDB | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να συνδεθείτε με ένα λογαριασμό DocumentDB με πρωτόκολλο υποστήριξη για MongoDB, τώρα διαθέσιμη για προεπισκόπηση. Σύνδεση με χρήση του MongoDB συμβολοσειρά σύνδεσης." 
    keywords="συμβολοσειρά σύνδεσης mongodb"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>Πώς μπορείτε να συνδεθείτε με ένα λογαριασμό DocumentDB με πρωτόκολλο υποστήριξη για MongoDB

Μάθετε πώς μπορείτε να συνδεθείτε με ένα λογαριασμό Azure DocumentDB με πρωτόκολλο υποστήριξη για MongoDB χρησιμοποιώντας την τυπική μορφή URI συμβολοσειρά σύνδεσης MongoDB.  

## <a name="get-the-accounts-connection-string-information"></a>Λήψη πληροφοριών συμβολοσειρά σύνδεσης του λογαριασμού

1. Σε ένα νέο παράθυρο, πραγματοποιήστε είσοδο στο [Azure πύλη](https://portal.azure.com).
2. Στη γραμμή **Περιήγησης αριστερά** από το Blade λογαριασμού, κάντε κλικ στην επιλογή **Συμβολοσειρά σύνδεσης**. Για να το **Blade λογαριασμού**, στην το Jumpbar κάντε κλικ στην επιλογή **Περισσότερες υπηρεσίες**, επιλέξτε **DocumentDB (NoSQL)** και, στη συνέχεια, επιλέξτε το λογαριασμό DocumentDB με πρωτόκολλο υποστήριξη για MongoDB.

    ![Στιγμιότυπο οθόνης από το blade όλες τις ρυθμίσεις](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. Το blade **Πληροφορίες συμβολοσειράς σύνδεσης** ανοίγει και περιλαμβάνει όλες τις πληροφορίες που είναι απαραίτητο για να συνδεθείτε με το λογαριασμό, χρησιμοποιώντας ένα πρόγραμμα οδήγησης για MongoDB, όπως μια συμβολοσειρά σύνδεσης κατασκευαστεί εκ των προτέρων.

    ![Στιγμιότυπο οθόνης από το blade συμβολοσειρά σύνδεσης](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Απαιτήσεις συμβολοσειρά σύνδεσης

Είναι σημαντικό να λάβετε υπόψη ότι DocumentDB υποστηρίζει την τυπική MongoDB URI μορφή της συμβολοσειράς σύνδεσης, με μερικά συγκεκριμένες απαιτήσεις: λογαριασμοί DocumentDB απαιτείται έλεγχος ταυτότητας και ασφαλή επικοινωνία μέσω SSL.  Έτσι, είναι η μορφή της συμβολοσειράς σύνδεσης:

    mongodb://username:password@host:port/[database]?ssl=true

Όπου οι τιμές αυτής της συμβολοσειράς είναι διαθέσιμες στο τη συμβολοσειρά σύνδεσης blade φαίνεται παραπάνω.

- Όνομα χρήστη (απαιτείται)
    - Το όνομα του λογαριασμού DocumentDB
- Κωδικός πρόσβασης (απαιτείται)
    - DocumentDB κωδικό πρόσβασης του λογαριασμού
- Host (απαιτείται)
    - Λογαριασμός FQDN του DocumentDB
- Θύρα (απαιτείται)
    - 10250
- Βάση δεδομένων (προαιρετικό)
    - Η προεπιλεγμένη βάση δεδομένων που χρησιμοποιείται από τη σύνδεση
- SSL = true (απαιτείται)

Για παράδειγμα, μπορείτε να το λογαριασμό που εμφανίζονται στο παραπάνω πληροφορίες συμβολοσειράς σύνδεσης.  Είναι μια έγκυρη συμβολοσειρά σύνδεσης:
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>Σύνδεση με το πρόγραμμα οδήγησης C# για MongoDB
Όπως έχει ήδη αναφερθεί, όλοι οι λογαριασμοί DocumentDB απαιτείται έλεγχος ταυτότητας και ασφαλή επικοινωνία μέσω SSL. Ενώ η μορφή URI της συμβολοσειράς σύνδεσης MongoDB υποστηρίζει ένα ssl = true ερωτήματος παραμέτρου, εργασία με το MongoDB C# πρόγραμμα οδήγησης απαιτεί τη χρήση του αντικειμένου MongoClientSettings κατά τη δημιουργία ενός MongoClient.  Λόγω των παραπάνω πληροφοριών λογαριασμού, το παρακάτω τμήμα κώδικα δείχνει πώς μπορείτε να συνδεθείτε με το λογαριασμό και να εργαστείτε με τη βάση δεδομένων "Εργασίες".

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>Επόμενα βήματα


- Μάθετε πώς να [χρησιμοποιήσετε MongoChef](documentdb-mongodb-mongochef.md) με ένα λογαριασμό DocumentDB με πρωτόκολλο υποστήριξης για MongoDB.
- Εξερευνήστε DocumentDB με πρωτόκολλο υποστήριξη για MongoDB [δείγματα](documentdb-mongodb-samples.md).

 
