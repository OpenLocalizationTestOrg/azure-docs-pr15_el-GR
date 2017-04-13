<properties
    pageTitle="Ρύθμιση ειδοποιήσεων για ερωτήματα στην ανάλυση ροή | Microsoft Azure"
    description="Κατανόηση των ροή ανάλυση ειδοποίησης"
    keywords="Ρύθμιση ειδοποιήσεων"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Ρύθμιση ειδοποιήσεων για ανάλυση Azure ροής εργασιών

## <a name="introduction-monitor-page"></a>Εισαγωγή: Παρακολούθηση σελίδας

Μπορείτε να ρυθμίσετε ειδοποιήσεις για να ενεργοποιήσετε μια ειδοποίηση όταν ένα μετρικό φτάσει μια συνθήκη που καθορίζετε.

Για παράδειγμα, "Εάν το αποτέλεσμα συμβάντων για τα τελευταία 15 λεπτά είναι < 100, αποστολή ηλεκτρονικού ταχυδρομείου ειδοποίησης ηλεκτρονικού ταχυδρομείου αναγνωριστικό: xyz@company.com”.

Οι κανόνες μπορεί να ρυθμιστεί μετρικά μέσω της πύλης ή μπορεί να είναι ρυθμισμένες [μέσω προγραμματισμού](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) μέσω δεδομένων λειτουργίας αρχείων καταγραφής.

## <a name="set-up-alerts-through-the-azure-classic-portal"></a>Ρύθμιση ειδοποιήσεων μέσω της πύλης κλασική Azure

Υπάρχουν δύο τρόποι για να ρυθμίσετε τις ειδοποιήσεις στην πύλη του Azure κλασική:  

1.  Στην καρτέλα **Εποπτεία** μιας εργασίας ροής ανάλυσης  
2.  Το αρχείο καταγραφής λειτουργίες στις υπηρεσίες διαχείρισης  

## <a name="set-up-alert-through-the-monitor-tab-of-the-job-in-the-portal"></a>Ρύθμιση ειδοποίησης μέσω καρτέλα οθόνη της εργασίας στην πύλη

1.  Επιλέξτε τη μέτρηση στην καρτέλα οθόνη, και κάντε κλικ στο κουμπί **Προσθήκη κανόνα** στο κάτω μέρος του πίνακα εργαλείων και ρύθμισης των κανόνων σας.  

    ![Πίνακας εργαλείων](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  

2.  Ορίστε το όνομα και μια περιγραφή της ειδοποίησης  

    ![Δημιουργία κανόνα](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  

3.  Πληκτρολογήστε τα όρια, το παράθυρο ειδοποίησης αξιολόγησης και τις ενέργειες για την ειδοποίηση  

    ![Ορισμός συνθηκών](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-through-the-operations-logs"></a>Ρύθμιση ειδοποιήσεων μέσω των αρχείων καταγραφής λειτουργίες

1.  Μεταβείτε στην καρτέλα **ειδοποιήσεις** υπηρεσίες διαχείρισης στην [Πύλη κλασική Azure](https://manage.windowsazure.com).  
2.  Κάντε κλικ στην επιλογή **Προσθήκη κανόνα**  

    ![Κριτήρια](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  

3.  Ορίστε το όνομα και μια περιγραφή της ειδοποίησης. Επιλέξτε 'Ροή ανάλυσης' ως τύπος υπηρεσίας και το όνομα της εργασίας ως το όνομα της υπηρεσίας.  

    ![Ορισμός ειδοποίησης](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-in-the-azure-portal"></a>Ρύθμιση ειδοποιήσεων στην πύλη του Azure ##

Στην πύλη του Azure, αναζητήστε το έργο ανάλυση ροής που σας ενδιαφέρουν ειδοποίησης για και κάντε κλικ στην ενότητα **παρακολούθησης** .  Το blade **μετρικό σύστημα** που ανοίγει, κάντε κλικ στην εντολή **Προσθήκη ειδοποίησης** .

  ![Azure ρύθμισης πύλης](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

Μπορείτε να ονομάσετε τον κανόνα σας ειδοποίησης, και επιλέξτε μια περιγραφή που θα εμφανίζεται στο μήνυμα ηλεκτρονικού ταχυδρομείου ειδοποίησης.

Όταν επιλέγετε μετρικά θα μπορείτε να επιλέξετε μια συνθήκη και μια οριακή τιμή για τη μέτρηση.

  ![Azure πύλης επιλέξτε μετρικό σύστημα](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Για περισσότερες λεπτομέρειες σχετικά με τη ρύθμιση ειδοποιήσεων στην πύλη του Azure, ανατρέξτε στο θέμα [Λήψη ειδοποιήσεων υπενθύμισης](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

## <a name="get-help"></a>Λήψη Βοήθειας
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ ανάλυση ροή Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)
