<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure οθόνη | Microsoft Azure"
    description="Γρήγορα αποτελέσματα χρησιμοποιώντας την εποπτεία Azure για να αποκτήσετε πληροφορίες για τη λειτουργία τους πόρους σας και να εκτελέσετε ενέργειες με βάση καταργήσω δεδομένων."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-azure-monitor"></a>Γρήγορα αποτελέσματα με την εποπτεία Azure

Οθόνη Azure είναι η υπηρεσία πλατφόρμα που παρέχει μια μεμονωμένη προέλευση για παρακολούθηση Azure πόρων. Με οθόνη Azure, μπορείτε να απεικόνιση ερωτήματος, δρομολόγηση, αρχειοθέτησης και εκτέλεση ενεργειών σε μετρήσεις και αρχεία καταγραφής που προέρχονται από τους πόρους στο Azure. Μπορείτε να εργαστείτε με αυτά τα δεδομένα χρησιμοποιώντας την οθόνη πύλης blade, [Cmdlet του PowerShell οθόνη](./insights-powershell-samples.md)"," [CLI πλατφόρμες](insights-cli-samples.md), ή " [Azure οθόνη REST API](https://msdn.microsoft.com/library/dn931943.aspx). Σε αυτό το άρθρο θα σας καθοδηγήσουμε σε μερικά από τα βασικά στοιχεία της οθόνη Azure, με την πύλη για επίδειξη.

1. Στην πύλη του, επιλέξτε **περισσότερες υπηρεσίες** και εντοπίστε την επιλογή **οθόνη** . Κάντε κλικ στο εικονίδιο του αστερίσκου για να προσθέσετε αυτήν την επιλογή στη λίστα Αγαπημένα του, ώστε να είναι πάντα εύκολα προσβάσιμα από τη γραμμή αριστερό πλαίσιο περιήγησης.

    ![Παρακολούθηση της από τη λίστα υπηρεσιών](./media/monitoring-get-started/monitor-more-services.png)

2. Κάντε κλικ στην επιλογή **οθόνη** για να ανοίξετε την **οθόνη** blade. Αυτό blade συγκεντρώνει όλες τις παρακολούθησης ρυθμίσεις σας και δεδομένα σε μία προβολή συνολική εικόνα. Πρώτα ανοίγει στην ενότητα **αρχείο καταγραφής δραστηριοτήτων** .

    ![Οθόνη blade περιήγησης](./media/monitoring-get-started/monitor-blade-nav.png)

    > [AZURE.WARNING] Οι επιλογές **υπηρεσία ειδοποιήσεων** και **ομάδες ειδοποίησης** εμφανίζεται παραπάνω θα εμφανίζεται μόνο με εκείνα που έχουν συνδεθεί η ιδιωτική προεπισκόπηση από αυτές τις δυνατότητες.

    Azure οθόνη έχει τρεις βασικές κατηγορίες παρακολούθηση δεδομένων: το **αρχείο καταγραφής δραστηριοτήτων** **μετρικά**και **αρχεία καταγραφής διαγνωστικών**.

3. Κάντε κλικ στην επιλογή **σύνδεση δραστηριότητας** για να βεβαιωθείτε ότι εμφανίζεται η ενότητα καταγραφής δραστηριότητας.

    ![Blade καταγραφής δραστηριότητας](./media/monitoring-get-started/monitor-act-log-blade.png)

    [**Αρχείο καταγραφής δραστηριοτήτων**](./monitoring-overview-activity-logs.md) περιγράφονται όλες οι λειτουργίες που εκτελούνται σε πόρους στη συνδρομή σας. Χρησιμοποιώντας το αρχείο καταγραφής της δραστηριότητας, μπορείτε να προσδιορίσετε το ' Τι, ποιος, και πότε ' για οποιαδήποτε δημιουργία, ενημέρωση ή διαγραφή λειτουργίες σε πόρους στη συνδρομή σας. Για παράδειγμα, το αρχείο καταγραφής της δραστηριότητας σας ενημερώνει όταν μια εφαρμογή web διακόπηκε και ποιος το διακοπεί. Δραστηριότητα καταγραφής συμβάντων είναι αποθηκευμένη στην πλατφόρμα και διαθέσιμες ερώτημα για 90 ημέρες.
   
    Μπορείτε να δημιουργήσετε και αποθήκευση ερωτημάτων για κοινά φίλτρα, στη συνέχεια, καρφιτσώσετε τα πιο σημαντικά ερωτήματα σε έναν πίνακα εργαλείων πύλης, ώστε να ξέρετε πάντα εάν έχουν προκύψει συμβάντα που πληρούν τα κριτήριά σας.

4. Φιλτράρισμα της προβολής σε μια συγκεκριμένη ομάδα πόρων πάνω από την τελευταία εβδομάδα και, στη συνέχεια, κάντε κλικ στο κουμπί **Αποθήκευση** .

    ![Αποθήκευση ερωτήματος καταγραφής δραστηριότητας](./media/monitoring-get-started/monitor-act-log-save.png)

5. Τώρα, κάντε κλικ στο κουμπί **καρφιτσώματος** .

    ![Κάντε κλικ στην επιλογή pin για το αρχείο καταγραφής δραστηριοτήτων](./media/monitoring-get-started/monitor-act-log-pin.png)

    Μπορεί να είναι καρφιτσωμένη περισσότερες από τις προβολές σε αυτές τις οδηγίες σε έναν πίνακα εργαλείων. Αυτό σας βοηθά να δημιουργήσετε μια ενιαία πηγή πληροφοριών για λειτουργικές δεδομένα σχετικά με τις υπηρεσίες σας. 

6. Επιστρέψτε στον πίνακα εργαλείων σας. Τώρα, μπορείτε να δείτε ότι το ερώτημα (και τον αριθμό των αποτελεσμάτων) εμφανίζεται στον πίνακα εργαλείων σας. Αυτό είναι χρήσιμο εάν θέλετε να δείτε γρήγορα τις ενέργειες υψηλού προφίλ που έχουν προκύψει πρόσφατα στη συνδρομή σας, π.χ. ένα νέο ρόλο έχει εκχωρηθεί ή έχει διαγραφεί μια Εικονική.

    ![Αρχείο καταγραφής δραστηριοτήτων καρφιτσωμένα στον πίνακα εργαλείων](./media/monitoring-get-started/monitor-act-log-db.png)

7. Επιστρέψτε στο πλακίδιο **οθόνη** και κάντε κλικ στην ενότητα **μετρικά** . Πρέπει πρώτα να επιλέξετε έναν πόρο, φιλτράρισμα και επιλέγοντας χρησιμοποιώντας το αναπτυσσόμενο επιλογές στο επάνω μέρος του blade.

    ![Φιλτράρισμα πόρων για μετρικά](./media/monitoring-get-started/monitor-met-filter.png)

    Όλοι οι πόροι Azure εκπέμπει [**μετρήσεις**](./monitoring-overview-metrics.md). Αυτή η προβολή συγκεντρώνει όλες τις μετρήσεις σε ένα τμήμα παραθύρου από γυαλί, ώστε να μπορείτε εύκολα να κατανοήσετε πώς αποδίδει τους πόρους σας.

8. Αφού έχετε επιλέξει έναν πόρο, όλα τα διαθέσιμα μετρικά εμφανίζονται στην αριστερή πλευρά της το blade. Μπορείτε να γράφημα πολλά μετρικά ταυτόχρονα επιλέγοντας το στοιχείο μετρήσεις και να τροποποιήσετε την περιοχή graph τύπο και την ώρα. Μπορείτε επίσης να προβάλετε όλες τις ειδοποιήσεις μετρικό ρύθμιση σε αυτόν τον πόρο.

    ![Μετρικό blade](./media/monitoring-get-started/monitor-metric-blade.png)

    > [AZURE.NOTE] Ορισμένες μετρικά είναι διαθέσιμες μόνο με ενεργοποίηση [Εφαρμογής ιδέες](../application-insights/app-insights-overview.md) ή/και Windows ή Linux Azure Διαγνωστικά για τον πόρο.

9. Όταν είστε ικανοποιημένοι με το γράφημά σας, μπορείτε να χρησιμοποιήσετε το κουμπί **Pin** για να καρφιτσώσετε στον πίνακα εργαλείων σας.

10. Επιστρέψτε στο blade την **οθόνη** και κάντε κλικ στην επιλογή **αρχεία καταγραφής διαγνωστικών**.

    ![Αρχεία καταγραφής διαγνωστικών blade](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Αρχεία καταγραφής διαγνωστικών**](monitoring-overview-of-diagnostic-logs.md) είναι αρχεία καταγραφής που εκπέμπει *από* έναν πόρο που παρέχουν δεδομένα σχετικά με τη λειτουργία του συγκεκριμένο πόρο. Για παράδειγμα, μετρητές κανόνα ομάδα ασφαλείας δικτύου και αρχεία καταγραφής ροής εργασίας εφαρμογή λογικής είναι και οι δύο τύποι αρχείων καταγραφής διαγνωστικών. Αυτά τα αρχεία καταγραφής μπορεί να είναι αποθηκευμένα σε ένα λογαριασμό του χώρου αποθήκευσης, ροής με ένα διανομέα συμβάν ή/και αποστέλλονται στο [Αρχείο καταγραφής ανάλυσης](../log-analytics/log-analytics-overview.md). Αρχείο καταγραφής ανάλυσης είναι προϊόν λειτουργικές ευφυΐας της Microsoft για προχωρημένους αναζήτηση και ειδοποίησης.
   
    Στην πύλη, μπορείτε να προβάλετε και να φιλτράρετε μια λίστα με όλους τους πόρους στο τη συνδρομή σας για να προσδιορίσετε εάν έχουν ενεργοποιηθεί αρχεία καταγραφής διαγνωστικών.

11. Κάντε κλικ σε έναν πόρο στο τα αρχεία καταγραφής διαγνωστικών blade. Εάν τα αρχεία καταγραφής διαγνωστικών που αποθηκεύονται σε ένα λογαριασμό του χώρου αποθήκευσης, θα δείτε μια λίστα με ωριαία αρχεία καταγραφής που απευθείας, μπορείτε να κάνετε λήψη.

    ![Αρχεία καταγραφής διαγνωστικών για έναν πόρο](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    Μπορείτε επίσης να επιλέξετε **Διαγνωστικών ρυθμίσεων**, οι οποίες σας επιτρέπει να ρυθμίσετε ή να τροποποιήσετε τις ρυθμίσεις σας για αρχειοθέτηση σε ένα λογαριασμό του χώρου αποθήκευσης, ροής με διανομείς συμβάν ή αποστολή σε ένα χώρο εργασίας του αρχείου καταγραφής ανάλυσης.

    ![Ενεργοποίηση αρχείων καταγραφής διαγνωστικών](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Εάν έχετε ρυθμίσει αρχεία καταγραφής διαγνωστικών για ανάλυση καταγραφής, μπορείτε να τα αναζητήσετε, στη συνέχεια, στην ενότητα **Αναζήτηση καταγραφής** της πύλης.

12. Μεταβείτε στην ενότητα **ειδοποιήσεις** από την οθόνη blade.

    ![ειδοποιήσεις blade για δημόσια](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Εδώ μπορείτε να διαχειριστείτε όλες τις [**ειδοποιήσεις**](./monitoring-overview-alerts.md) σας Azure πόροι. Περιλαμβάνονται και οι προειδοποιήσεις σε μετρήσεις, δραστηριότητα καταγραφής συμβάντων (ιδιωτικό preview), ιδέες εφαρμογής web δοκιμές (θέσεις) και διαγνωστικά έγκαιρη εφαρμογή ιδέες. Ειδοποιήσεις μπορεί να προκαλέσει την αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου ή μια ΔΗΜΟΣΊΕΥΣΗ HTTP σε μια διεύθυνση URL webhook.
   
13. Κάντε κλικ στην επιλογή **Προσθήκη μετρικό ειδοποίησης** για να δημιουργήσετε μια ειδοποίηση.

    ![Προσθήκη μετρικό ειδοποίησης](./media/monitoring-get-started/monitor-alerts-add.png)

    Στη συνέχεια, μπορείτε να καρφιτσώσετε μια ειδοποίηση στον πίνακα εργαλείων σας για να βλέπετε εύκολα στην κατάσταση οποιαδήποτε στιγμή.

14. Στην ενότητα οθόνη περιλαμβάνει επίσης συνδέσεις σε [Εφαρμογή ιδέες](../application-insights/app-insights-overview.md) εφαρμογές και λύσεις διαχείρισης [Καταγραφής ανάλυσης](../log-analytics/log-analytics-overview.md) . Αυτά τα άλλα προϊόντα της Microsoft έχουν ενοποίηση με οθόνη Azure.

15. Εάν δεν χρησιμοποιείτε εφαρμογή ιδέες ή ανάλυσης αρχείου καταγραφής, το πιθανότερο είναι ότι Azure οθόνη έχει μια συνεργασία με το τρέχον παρακολούθησης, καταγραφή και προειδοποίησης προϊόντα. Ανατρέξτε στο θέμα μας [συνεργάτες σελίδας](./monitoring-partners.md) για μια πλήρη λίστα και οδηγίες για το πώς μπορείτε να ενοποιήσετε.

Ακολουθώντας αυτά τα βήματα και Καρφίτσωμα όλα τα πλακίδια σχετικές σε έναν πίνακα εργαλείων, μπορείτε να δημιουργήσετε ολοκληρωμένη προβολές από την εφαρμογή και την υποδομή όπως αυτή:

![Azure οθόνη πίνακα εργαλείων](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Επόμενα βήματα
- Διαβάστε την [Επισκόπηση των οθονών Azure](./monitoring-overview.md)
