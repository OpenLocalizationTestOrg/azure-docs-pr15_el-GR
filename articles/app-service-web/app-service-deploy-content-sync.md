<properties
    pageTitle="Συγχρονισμός περιεχομένου από ένα φάκελο cloud Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς μπορείτε να αναπτύξετε την εφαρμογή σας σε Azure εφαρμογής υπηρεσίας μέσω του συγχρονισμού περιεχομένου από ένα φάκελο cloud."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Συγχρονισμός περιεχομένου από ένα φάκελο cloud Azure εφαρμογής υπηρεσίας

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να αναπτύξετε σε [Azure εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=529714) με συγχρονισμό του περιεχομένου από τις υπηρεσίες αποθήκευσης δημοφιλείς cloud όπως το Dropbox και το OneDrive. 

## <a name="overview"></a>Επισκόπηση της ανάπτυξης περιεχομένου συγχρονισμού

Συγχρονισμός περιεχομένου σε απαιτήσεων ανάπτυξης είναι υποστηρίζεται από το [μηχανισμό ανάπτυξης Kudu](https://github.com/projectkudu/kudu/wiki) ενσωματωμένο με εφαρμογή υπηρεσίας. Στην [Πύλη του Azure](https://portal.azure.com), μπορείτε να ορίσετε ένα φάκελο στο του χώρου αποθήκευσης στο cloud, εργασία με το κώδικας εφαρμογής και το περιεχόμενο σε αυτόν το φάκελο και συγχρονισμός με εφαρμογής υπηρεσίας με το πάτημα ενός κουμπιού. Συγχρονισμός περιεχομένου χρησιμοποιεί τη διαδικασία Kudu για δημιουργία και ανάπτυξη. 
    
## <a name="contentsync"></a>Τρόπος ενεργοποίησης της ανάπτυξης περιεχομένου συγχρονισμού
Για να ενεργοποιήσετε το συγχρονισμό περιεχομένου από την [Πύλη Azure](https://portal.azure.com), ακολουθήστε τα παρακάτω βήματα:

1. Στο blade της εφαρμογής σας στην πύλη του Azure, κάντε κλικ στην επιλογή **Ρυθμίσεις** > **Ανάπτυξης προέλευσης**. Κάντε κλικ στην επιλογή **Επιλογή αρχείου προέλευσης**και, στη συνέχεια, επιλέξτε **OneDrive** ή το **Dropbox** ως προέλευση για ανάπτυξη. 

    ![Συγχρονισμός περιεχομένου](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] Λόγω υποκείμενα διαφορές σε τα API, **OneDrive για επιχειρήσεις** δεν υποστηρίζεται αυτήν τη στιγμή. 

2. Ολοκλήρωση της ροής εργασίας εξουσιοδότησης για να ενεργοποιήσετε την εφαρμογή υπηρεσίας για να αποκτήσετε πρόσβαση σε μια συγκεκριμένη προκαθορισμένες που έχει οριστεί ως διαδρομή για το OneDrive ή το Dropbox όπου όλου του περιεχομένου εφαρμογής υπηρεσίας θα αποθηκευτεί.  
    Μετά την έγκριση της εφαρμογής υπηρεσίας πλατφόρμα θα σας δώσει την επιλογή για να δημιουργήσετε ένα φάκελο περιεχομένου στην περιοχή του περιεχομένου που έχει οριστεί ως διαδρομής, ή για να επιλέξετε έναν υπάρχοντα φάκελο περιεχομένου στην περιοχή αυτήν τη διαδρομή που έχει οριστεί ως περιεχομένου. Οι διαδρομές που έχει οριστεί ως περιεχομένου στην περιοχή λογαριασμών χώρο αποθήκευσης στο cloud χρησιμοποιούνται για την εφαρμογή υπηρεσίας συγχρονισμού είναι οι εξής:  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **Dropbox**:`Dropbox\Apps\Azure`

3. Μετά την αρχική Συγχρονισμός περιεχομένου μπορεί να αρχίσει το συγχρονισμό περιεχομένου ζήτηση από την πύλη του Azure. Ανάπτυξη του ιστορικού είναι διαθέσιμη με το blade **αναπτύξεις** .

    ![Ανάπτυξη του ιστορικού](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
Περισσότερες πληροφορίες για την ανάπτυξη Dropbox είναι διαθέσιμη στην περιοχή [Ανάπτυξη από το Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 


