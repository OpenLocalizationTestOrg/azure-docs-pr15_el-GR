<properties
    pageTitle="Παρακολούθηση εύρυθμη λειτουργία των υπηρεσιών Azure χρησιμοποιώντας αρχεία καταγραφής του Azure εποπτεία δραστηριότητας | Microsoft Azure"
    description="Δείτε πότε Azure εντόπισε επιδόσεων υποβάθμιση ή υπηρεσία διακοπές. "
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="track-azure-service-health-using-azure-monitor-activity-logs"></a>Παρακολούθηση εύρυθμη λειτουργία των υπηρεσιών Azure χρησιμοποιώντας αρχεία καταγραφής του Azure εποπτεία δραστηριότητας

Azure publicizes κάθε φορά που υπάρχει μια υποβάθμιση υπηρεσίας διακοπές ή επιδόσεων. Μπορείτε να περιηγηθείτε σε αυτά τα συμβάντα στην πύλη του Azure και μπορείτε επίσης να χρησιμοποιήσετε το [REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx) ή [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) για να αποκτήσετε πρόσβαση το πλήρες σύνολο των συμβάντων μέσω προγραμματισμού.

## <a name="browse-the-service-health-logs-for-your-subscription"></a>Αναζητήστε τα αρχεία καταγραφής εύρυθμης λειτουργίας υπηρεσίας για τη συνδρομή σας

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com/).

2. Στην **κεντρική καρτέλα** , θα πρέπει να βλέπετε ένα πλακίδιο που ονομάζεται **εύρυθμη λειτουργία των υπηρεσιών**. Κάντε κλικ σε αυτό.

    ![Κεντρική](./media/insights-service-health/Insights_Home.png)

3. Μπορείτε να δείτε μια λίστα με όλες τις περιοχές στο Azure. Κάντε κλικ σε οποιαδήποτε περιοχή για να εμφανίσετε το ερώτημα αρχείο καταγραφής δραστηριοτήτων που εμφανίζει περιστατικά υπηρεσίας που έχουν οποιαδήποτε από τις συνδρομές σας επηρεάζεται τις τελευταίες 24 ώρες.

    ![Δραστηριότητα καταγραφής συνδρομή εύρυθμη λειτουργία των υπηρεσιών](./media/insights-service-health/AzureActivityLogServiceHealth3.png)

4. Μπορείτε να δείτε τις λεπτομέρειες των μεμονωμένων περιστατικό, κάνοντας κλικ σε αυτό το συμβάν στον πίνακα.

5. Αλλάξτε το **χρονικό διάστημα** για να δείτε ένα μεγαλύτερο χρονικό πλαίσιο.

## <a name="next-steps"></a>Επόμενα βήματα

* Η [οθόνη διαθεσιμότητα και την απόκριση οποιασδήποτε σελίδας web](../application-insights/app-insights-monitor-web-app-availability.md) με εφαρμογή ιδέες, ώστε να μπορείτε να μάθετε αν σελίδα σας είναι προς τα κάτω.
