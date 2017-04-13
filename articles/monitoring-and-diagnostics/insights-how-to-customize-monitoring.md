<properties
    pageTitle="Επισκόπηση των μετρήσεων στο Microsoft Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να προσαρμόσετε παρακολούθησης γραφήματα στο Azure."
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Επισκόπηση των μετρήσεων στο Microsoft Azure

Όλων των υπηρεσιών Azure παρακολούθηση κλειδιού μετρικά που σας επιτρέπει να παρακολουθείτε την εύρυθμη λειτουργία, απόδοση, διαθεσιμότητα και χρήση των υπηρεσιών σας. Μπορείτε να προβάλετε αυτές τις μετρήσεις στην πύλη του Azure και μπορείτε επίσης να χρησιμοποιήσετε το [REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) ή [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) για να αποκτήσετε πρόσβαση το πλήρες σύνολο μετρικά μέσω προγραμματισμού.

Για ορισμένες υπηρεσίες, ίσως χρειαστεί να ενεργοποιήσετε διαγνωστικών για να βλέπετε τις μετρήσεις. Για άλλα άτομα, όπως εικονικές μηχανές, θα λάβετε ένα βασικό σύνολο μετρικά, αλλά πρέπει να ενεργοποιήσετε το πλήρες Ορισμός μετρικά υψηλής συχνότητας. Ανατρέξτε στο θέμα [Ενεργοποίηση παρακολούθησης και διαγνωστικών](insights-how-to-use-diagnostics.md) για να μάθετε περισσότερα.

## <a name="using-monitoring-charts"></a>Χρήση γραφημάτων παρακολούθησης

Μπορείτε να δημιουργήσετε ένα γράφημα οποιοδήποτε από τα μετρικά τους οποιαδήποτε χρονική περίοδο που επιλέγετε.

1. Στην [Πύλη του Azure](https://portal.azure.com/), κάντε κλικ στην επιλογή **Αναζήτηση**και, στη συνέχεια, έναν πόρο που σας ενδιαφέρει παρακολούθησης.

2. Η ενότητα **παρακολούθησης** περιέχει τις πιο σημαντικές μετρήσεις για κάθε πόρο Azure. Για παράδειγμα, μια εφαρμογή web έχει **αιτήσεις και σφάλματα**, όπου με μια εικονική μηχανή να έχουν **ποσοστό της CPU** και **δίσκου ανάγνωσης και εγγραφής**:  ![φακός παρακολούθησης](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. Κάνοντας κλικ σε οποιοδήποτε γράφημα θα σας δείξουν το blade **μετρικό σύστημα** . Στην το blade, εκτός από το γράφημα, είναι ένας πίνακας που δείχνει συγκεντρώσεις του μετρικών (όπως μέσος όρος, ελάχιστο και μέγιστο, επάνω από την περιοχή ώρας που επιλέξατε). Κάτω από το στοιχείο που είναι οι κανόνες ειδοποίησης για τον πόρο.
    ![Μετρικό blade](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. Για να προσαρμόσετε τις γραμμές που εμφανίζονται, κάντε κλικ στο κουμπί **Επεξεργασία** στο γράφημα, ή, η εντολή **Επεξεργασία γραφήματος** στο το μετρικό blade.

5. Στην το blade επεξεργασία ερωτήματος, μπορείτε να κάνετε τρία στοιχεία:
    - Αλλαγή της περιοχής ώρας
    - Αλλαγή της εμφάνισης μεταξύ των ράβδων και γραμμών
    - Επιλέξτε διαφορετική metics ![επεξεργασία ερωτήματος](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. Αλλαγή της περιοχής ώρας είναι τόσο εύκολη όσο επιλέγοντας μια διαφορετική περιοχή (όπως η **Τελευταία ώρα**) και κάνοντας κλικ στην επιλογή " **Αποθήκευση** " στο κάτω μέρος του blade. Μπορείτε επίσης να επιλέξετε **προσαρμοσμένο**, που σας επιτρέπει να επιλέξετε οποιαδήποτε χρονική περίοδο τις τελευταίες 2 εβδομάδες. Για παράδειγμα, μπορείτε να δείτε το ολόκληρη δύο εβδομάδες, ή, απλώς 1 ώρα από χθες. Πληκτρολογήστε στο πλαίσιο κειμένου για να εισαγάγετε μια διαφορετική ώρα.
    ![Προσαρμοσμένο εύρος χρόνου](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. Κάτω από την περιοχή ώρα, το κανάλι που επιλέξτε οποιονδήποτε αριθμό μετρικά για να εμφανίσετε στο γράφημα.

8. Όταν κάνετε κλικ στο κουμπί Αποθήκευση τις αλλαγές σας θα αποθηκευτούν για το συγκεκριμένο πόρο. Για παράδειγμα, εάν έχετε δύο εικονικές μηχανές και μπορείτε να αλλάξετε ένα γράφημα σε μία, δεν θα επηρεάσει το άλλο.

## <a name="creating-side-by-side-charts"></a>Δημιουργία γραφημάτων σε παράθεση

Με την προσαρμογή ισχυρή στην πύλη, μπορείτε να προσθέσετε όσα γραφήματα όπως θέλετε.

1. Στο μενού **...** στο επάνω μέρος του blade, κάντε κλικ στην επιλογή **Προσθήκη πλακιδίων**:  
    ![Προσθήκη μενού](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Στη συνέχεια, μπορείτε να επιλέξτε επιλέξτε ένα γράφημα από τη **συλλογή** στη δεξιά πλευρά της οθόνης σας:  ![συλλογή](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Εάν δεν βλέπετε τη μέτρηση που θέλετε, μπορεί να πάντα προσθέτετε μία από τις προκαθορισμένες μετρικά και να **επεξεργαστείτε** το γράφημα για να εμφανίσετε τη μέτρηση που χρειάζεστε.

## <a name="monitoring-usage-quotas"></a>Παρακολούθηση ορίων χρήσης

Οι περισσότερες μετρικά σάς δείχνουν τάσεις μέσα στο χρόνο, αλλά ορισμένα δεδομένα, όπως ορίου χρήσης, είναι πληροφορίες σε δεδομένη χρονική στιγμή με μια οριακή τιμή.

Μπορείτε επίσης να δείτε ορίων χρήσης σε το blade για τους πόρους που έχουν ορίων:

![Χρήση](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Όπως με μετρήσεις, μπορείτε να χρησιμοποιήσετε το [REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) ή [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) για να αποκτήσετε πρόσβαση το πλήρες σύνολο των ορίων χρήσης μέσω προγραμματισμού.

## <a name="next-steps"></a>Επόμενα βήματα

* [Λάβετε ειδοποιήσεις](insights-receive-alert-notifications.md) κάθε φορά που ένα μετρικό τέμνει μια οριακή τιμή.
* [Ενεργοποίηση παρακολούθησης και διαγνωστικών](insights-how-to-use-diagnostics.md) για τη συλλογή λεπτομερείς μετρικά υψηλής συχνότητας την υπηρεσία.
* [Αυτόματη κλιμάκωση count παρουσία](insights-how-to-scale.md) για να βεβαιωθείτε ότι η υπηρεσία σας είναι διαθέσιμο και αποκρίνεται.
* [Παρακολούθηση εφαρμογών απόδοσης](../application-insights/app-insights-azure-web-apps.md) εάν θέλετε να κατανοήσετε ακριβώς πώς σας κώδικα αποδίδει στο cloud.
* Χρησιμοποιήστε [Εφαρμογή ιδέες για εφαρμογές JavaScript και τις σελίδες web](../application-insights/app-insights-web-track-usage.md) για να λάβετε προγράμματος-πελάτη αναλυτικά στοιχεία σχετικά με τα προγράμματα περιήγησης που επισκεφθείτε την ιστοσελίδα.
* Η [οθόνη διαθεσιμότητα και την απόκριση οποιασδήποτε σελίδας web](../application-insights/app-insights-monitor-web-app-availability.md) με εφαρμογή ιδέες, ώστε να μπορείτε να μάθετε αν σελίδα σας είναι προς τα κάτω.