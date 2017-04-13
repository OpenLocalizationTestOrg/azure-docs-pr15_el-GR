<properties
    pageTitle="Εμφάνιση Javadoc περιεχόμενο σε Έκλειψη για το πακέτο Azure βιβλιοθήκες Java"
    description="Πώς μπορείτε να εμφανίσετε το περιεχόμενο Javadoc για τις βιβλιοθήκες Azure στο Έκλειψη."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Εμφάνιση Javadoc περιεχόμενο σε Έκλειψη για το πακέτο Azure βιβλιοθήκες Java #

Το περιεχόμενο Javadoc για τις βιβλιοθήκες Azure για Java μπορούν να προβληθούν στο περιβάλλον σας Έκλειψη, συσχετίζοντας το περιεχόμενο Javadoc στις βιβλιοθήκες Azure για Java. Ακολουθήστε τα παρακάτω βήματα δείξουμε πώς μπορείτε να χρησιμοποιήσετε αυτήν τη λειτουργικότητα εντός Έκλειψη.

Αυτή η διαδικασία προϋποθέτει έχετε ήδη προσθέσει τη βιβλιοθήκη Azure για Java σας διαδρομή Δόμηση.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Για να εμφανίσετε περιεχόμενο Javadoc σε Έκλειψη για τις βιβλιοθήκες Azure για Java ##

* Εντός του Έκλειψη Εξερεύνηση έργου, στην ενότητα **Βιβλιοθήκες στα οποία γίνεται αναφορά** του έργου σας, ανοίξτε το μενού περιβάλλοντος για τη βιβλιοθήκη Azure αυτή για ΒΆΖΩΝ Java. Για παράδειγμα, **microsoft-windowsazure-api-0.1.0.jar** (ο αριθμός έκδοσης μπορεί να είναι διαφορετικά, ανάλογα με την έκδοση που έχετε εγκαταστήσει).
* Κάντε κλικ στην επιλογή **Ιδιότητες**.
* Το παράθυρο διαλόγου **Ιδιότητες** , στο αριστερό παράθυρο, κάντε κλικ στην επιλογή **Θέση Javadoc**. Εμφανίζεται το παράθυρο διαλόγου **Javadoc θέση** .
* Μπορείτε να καθορίσετε μια **Διεύθυνση URL Javadoc**ή ένα **Javadoc σε αρχείο**.
    * Εάν επιλέξετε να καθορίσετε μια **Διεύθυνση URL Javadoc**, χρησιμοποιήστε τις διευθύνσεις URL όπως **http://dl.windowsazure.com/javadoc** ή **http://dl.windowsazure.com/storage/javadoc**.
    * Εάν επιλέξετε να χρησιμοποιήσετε **Javadoc σε αρχείο**, μπορείτε να καθορίσετε ένα εξωτερικό αρχείο ή σε ένα αρχείο χώρου εργασίας.
    Κάντε την επιλογή σας και αναζήτηση/επικύρωση όπως απαιτείται. Το παρακάτω παράδειγμα συσχετίζει τις βιβλιοθήκες Azure για Java με την αντίστοιχη Javadoc ΒΆΖΩΝ που έχει ληφθεί τοπικά σε ένα φάκελο που ονομάζεται **c:\MyAzureJARs**.
    ![][ic553487]
* *Προαιρετικό*: κάντε κλικ στην επιλογή **επικύρωση**. Πιθανά ζητήματα με το ΒΆΖΩΝ Javadoc ήταν δυνατό να εμφανιστούν εδώ.
* Κάντε κλικ στο **κουμπί OK**.

Μόλις που σχετίζεται με τη βιβλιοθήκη, πρέπει να εμφανίζει το περιεχόμενο Javadoc εντός του IDE Έκλειψη. Για παράδειγμα, εάν `blob` ορίζεται τύπου `CloudBlockBlob` εντός του κώδικα, τα παρακάτω είναι ένα παράδειγμα Javadoc περιεχομένου που εμφανίζεται όταν πληκτρολογείτε `blob.acquireLease` στον κώδικα:

![][ic553488]

## <a name="see-also"></a>Δείτε επίσης ##

[Azure Κιτ εργαλείων για Έκλειψη][]

[Δημιουργία εφαρμογής κόσμο Hello για Azure στο Έκλειψη][]

[Κατά την εγκατάσταση του Κιτ εργαλείων Azure για Έκλειψη][] 

Για περισσότερες πληροφορίες σχετικά με τη χρήση Azure με Java, ανατρέξτε στο [Κέντρο για προγραμματιστές του Azure Java][].

<!-- URL List -->

[Κέντρο για προγραμματιστές του Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Κιτ εργαλείων για Έκλειψη]: http://go.microsoft.com/fwlink/?LinkID=699529
[Δημιουργία εφαρμογής κόσμο Hello για Azure στο Έκλειψη]: http://go.microsoft.com/fwlink/?LinkID=699533
[Κατά την εγκατάσταση του Κιτ εργαλείων Azure για Έκλειψη]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png