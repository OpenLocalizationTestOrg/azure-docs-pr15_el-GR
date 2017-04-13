<properties
   pageTitle="Διαχείριση ενέργειας υπολογισμού σε αποθήκη δεδομένων SQL Azure (Azure πύλη) | Microsoft Azure"
   description="Azure πύλης εργασίες για τη Διαχείριση υπολογιστική ισχύ. Κλίμακα υπολογίσετε τους πόρους, προσαρμόζοντας DWUs. Ή, παύση και συνέχιση υπολογισμού πόρους για να αποθηκεύσετε κόστους."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Διαχείριση ενέργειας υπολογισμού σε αποθήκη δεδομένων SQL Azure (Azure πύλη)

> [AZURE.SELECTOR]
- [Επισκόπηση](sql-data-warehouse-manage-compute-overview.md)
- [Πύλη](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [ΥΠΌΛΟΙΠΟ](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Κλίμακα επιδόσεων, αλλάζοντας την κλίμακα ανάληψη υπολογιστική πόρους και μνήμης για να καλύψει τις μεταβαλλόμενες απαιτήσεις του φόρτου εργασίας σας. Αποθήκευση κόστους από τους πόρους κλίμακας πίσω κατά τη διάρκεια μη κορύφωσης ώρες ή παύση υπολογισμού εντελώς. 

Αυτή η συλλογή εργασίες χρησιμοποιεί το Azure πύλη για να:

- Κλίμακα υπολογισμού
- Παύση υπολογισμού
- Βιογραφικό σημείωμα υπολογισμού

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση τον υπολογισμό Επισκόπηση][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Κλίμακα power υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Για να αλλάξετε τους πόρους υπολογισμού:

1. Ανοίξτε την [πύλη του Azure][], ανοίξτε τη βάση δεδομένων και κάντε κλικ στην επιλογή **κλίμακας**.

    ![Κάντε κλικ στην επιλογή κλίμακας][1]

1. Στο blade την κλίμακα, μετακινήστε το ρυθμιστικό προς τα αριστερά ή δεξιά για να αλλάξετε τη ρύθμιση DWU.

    ![Μετακινήστε το ρυθμιστικό][2]

1. Κάντε κλικ στην επιλογή **Αποθήκευση**. Εμφανίζεται ένα μήνυμα επιβεβαίωσης. Κάντε κλικ στο κουμπί **Ναι** για επιβεβαίωση ή **χωρίς** να ακυρώσετε.

    ![Κάντε κλικ στην επιλογή Αποθήκευση][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Παύση υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Για να διακόψετε προσωρινά μια βάση δεδομένων:

1. Ανοίξτε την [πύλη του Azure][] και ανοίξτε τη βάση δεδομένων. Παρατηρήστε ότι η κατάσταση είναι σε **λειτουργία**. 

    ![Κατάσταση σύνδεσης][6]

1. Για να αναστείλει υπολογισμού και τους πόρους μνήμης, κάντε κλικ στην επιλογή **Παύση**και, στη συνέχεια, ένα μήνυμα επιβεβαίωσης εμφανίζεται. Κάντε κλικ στο κουμπί **Ναι** για επιβεβαίωση ή **χωρίς** να ακυρώσετε.

    ![Επιβεβαίωση παύση][7]

1. Κατά την αποθήκη δεδομένων του SQL εκκίνηση της βάσης δεδομένων, η κατάσταση είναι **προσωρινή διακοπή**.
2. Όταν η κατάσταση είναι **σε παύση**, έχει ολοκληρωθεί η λειτουργία παύση και που δεν είναι πλέον που χρεώνονται για DWUs.

    ![Παύση κατάστασης][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Βιογραφικό σημείωμα υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]Για να συνεχίσετε μια βάση δεδομένων:

1. Ανοίξτε την [πύλη του Azure][] και ανοίξτε τη βάση δεδομένων. Παρατηρήστε ότι η κατάσταση είναι **σε παύση**. 

    ![Παύση βάσης δεδομένων][4]

1. Για να συνεχίσετε τη βάση δεδομένων, κάντε κλικ στο κουμπί **Έναρξη**και, στη συνέχεια, εμφανίζεται ένα μήνυμα επιβεβαίωσης. Κάντε κλικ στο κουμπί **Ναι** για επιβεβαίωση ή **χωρίς** να ακυρώσετε.

    ![Επιβεβαίωση βιογραφικού σημειώματος][5]

1. Κατά την αποθήκη δεδομένων του SQL εκκίνηση της βάσης δεδομένων, η κατάσταση είναι "Resuming".
2. Όταν η κατάσταση είναι σε **σύνδεση**, η βάση δεδομένων είναι έτοιμο.

    ![Κατάσταση σύνδεσης][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση της διαχείρισης][].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Επισκόπηση της διαχείρισης]: ./sql-data-warehouse-overview-manage.md
[Διαχείριση Επισκόπηση υπολογισμού]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Πύλη του Azure]: http://portal.azure.com/
