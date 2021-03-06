<properties
   pageTitle="Λήψη ιδεών από το Κέντρο ασφάλειας Azure δεδομένων με το Power BI | Microsoft Azure"
   description="Το πακέτο περιεχομένου Azure ασφάλεια κέντρου Power BI καθιστά εύκολη την εύρεση ειδοποιήσεων ασφαλείας, συστάσεις, επιθέσεις πόρους και την εξέλιξη, με βάση ένα σύνολο δεδομένων που έχει δημιουργηθεί για την αναφορά σας."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Λήψη ιδεών από το Κέντρο ασφάλειας Azure δεδομένων με το Power BI
Του [Πίνακα εργαλείων του Power BI](http://aka.ms/azure-security-center-power-bi) για το Κέντρο ασφάλειας Azure σάς επιτρέπει να απεικονίσετε, ανάλυση και το φιλτράρισμα συστάσεις και οι προειδοποιήσεις ασφαλείας από οπουδήποτε, όπως την κινητή συσκευή σας. Χρησιμοποιήστε τον πίνακα εργαλείων του Power BI για να αποκαλύψετε τάσεις και επιθέσεις μοτίβα - προβολή προειδοποιήσεις ασφαλείας, πόρων ή τη διεύθυνση IP προέλευσης και unaddressed τους κινδύνους ασφαλείας, πόρο ή μια ηλικία. 

Μπορείτε να επίσης συνδυάσετε συστάσεις Κέντρο ασφάλειας και οι προειδοποιήσεις ασφαλείας με άλλα δεδομένα με ενδιαφέροντες τρόπους, για παράδειγμα, χρησιμοποιώντας δεδομένα από [Αρχεία καταγραφής ελέγχου Azure](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) και [Έλεγχος βάσης δεδομένων SQL Azure](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/). Και τα δύο προσφέρουν πίνακες εργαλείων του Power BI και μπορείτε επίσης να εξαγάγετε τα δεδομένα στο Excel για την εύκολη αναφορά σχετικά με την κατάσταση ασφαλείας από τους πόρους σας στο cloud.

##<a name="using-azure-security-center-dashboard-to-access-power-bi"></a>Χρήση του πίνακα εργαλείων στο Κέντρο ασφάλειας Azure για πρόσβαση στο Power BI
Μπορείτε επίσης να χρησιμοποιήσετε το Κέντρο ασφάλειας Azure πίνακα εργαλείων για να αποκτήσετε πρόσβαση σε αναφορές του Power BI. Ακολουθήστε τα βήματα για να εκτελέσετε αυτήν την εργασία: 

1. Στο **Κέντρο ασφάλειας Azure** πίνακα εργαλείων, κάντε κλικ στο κουμπί **Εξερεύνηση στο Power BI** .

    ![Σύνδεση στο Κέντρο ασφάλειας Azure με χρήση του Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new10.png) 

2. Η **Εξερεύνηση στο Power BI** blade ανοίγει στη δεξιά πλευρά, όπως φαίνεται στην παρακάτω οθόνη:

    ![Σύνδεση στο Κέντρο ασφάλειας Azure με χρήση του Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new2.png)

3. Εάν θέλετε να δημιουργήσετε τον πίνακα εργαλείων του Power BI για πρώτη φορά, μπορείτε να επιλέξετε μία από τις παρακάτω επιλογές στο το blade **Εξερεύνηση στο Power BI** : 

    - **Πίνακας εργαλείων ιδέες ασφαλείας**: Ενεργοποιήστε αυτήν την επιλογή εάν θέλετε να δημιουργήσετε έναν πίνακα εργαλείων που περιλαμβάνει την κατάσταση ασφαλείας, νήματα και τις ανιχνεύσεις. Αυτή η επιλογή είναι ένα κοινό περισσότερα για το ρόλο DevOps που είναι υπεύθυνος για την ανάλυση της κατάστασής τους προστασίας και εντοπίζονται ειδοποιήσεις στις συνδρομές.
    - **Πίνακας εργαλείων διαχείρισης πολιτικής**: Ενεργοποιήστε αυτήν την επιλογή εάν θέλετε να εξερευνήσετε πολιτικής διαχείρισης και επιβολής.  Αυτή η επιλογή είναι ένα κοινό περισσότερες για κεντρική IT που είναι πιο εστιασμένη σε διακυβέρνησης. Μπορούν να χρησιμοποιήσουν αυτόν τον πίνακα εργαλείων για να αποκτήσετε ορατότητα και τις ιδέες σε τήρησης πολιτική ασφαλείας σε ολόκληρη την εταιρεία τους.
    - Εάν έχετε ήδη έναν πίνακα εργαλείων του Power BI, κάντε κλικ στην επιλογή **, μεταβείτε στον πίνακα εργαλείων σας τρέχουσα Power BI**.

4. Για αυτό το παράδειγμα, κάντε κλικ στην επιλογή επιλογή **πίνακα εργαλείων ιδέες ασφαλείας** . Εάν αυτή είναι η πρώτη φορά, δημιουργείτε έναν πίνακα εργαλείων του Power BI για το Κέντρο ασφάλειας που θα σας ζητηθεί να εγκαταστήσετε το πακέτο περιεχομένου. Κάντε κλικ στο κουμπί **λήψη** στο παράθυρο **περιεχομένου πακέτα για το Power BI** , όπως φαίνεται στην παρακάτω οθόνη:

    ![Azure ασφαλείας Κέντρο ασφάλειας ιδέες πίνακα εργαλείων](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)

5. Εμφανίζεται το παράθυρο **σύνδεση με το Azure ασφαλείας Κέντρο ασφάλειας ιδέες** . Βεβαιωθείτε ότι η μέθοδος **ελέγχου ταυτότητας** είναι **oAuth2** όπως φαίνεται παρακάτω και κάντε κλικ στο κουμπί **Είσοδος** .
    
    ![Έλεγχος ταυτότητας](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)

6. Μπορεί να σας ζητηθεί για τον έλεγχο ταυτότητας ξανά με το Azure τα διαπιστευτήριά σας. Μετά την πραγματοποίηση ελέγχου ταυτότητας στον πίνακα εργαλείων σας θα δημιουργηθεί. Όταν δημιουργηθεί ο πίνακας εργαλείων μπορείτε να δείτε μια αναφορά με τη δομή παρόμοια όπως φαίνεται στην παρακάτω οθόνη:

    ![Πίνακας εργαλείων του Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)


> [AZURE.NOTE] Ανανέωση της αναφοράς είναι προγραμματισμένη να πραγματοποιείται σε καθημερινή βάση. Σε περίπτωση που συναντήσετε ένα σφάλμα κατά την ανανέωση σε αυτό, διαβάστε [Πιθανά ζητήματα Ανανέωση με το Power BI Azure ασφαλείας κέντρο](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/), για περισσότερες πληροφορίες σχετικά με τον τρόπο αντιμετώπισης προβλημάτων.

Εδώ μπορείτε να δείτε τον αριθμό των ειδοποιήσεων ασφαλείας και συστάσεις, καθώς και τον αριθμό των ΣΠΣ, βάσεις δεδομένων Azure SQL και παρακολουθείται από το Κέντρο ασφάλειας Azure πόρους δικτύου.

Μια σύνδεση προς το Κέντρο ασφάλειας Azure ανακατευθύνει στην πύλη του Azure. Τα γραφήματα διευκολύνουν την απεικόνιση πληροφορίες σχετικά με συστάσεις ασφαλείας και ειδοποιήσεις, όπως:

- Κατάσταση ασφαλείας πόρων
- Συστάσεις σε εκκρεμότητα
- Εικονική συστάσεων
- Ειδοποιήσεις βάσει χρόνου
- Προσβεβλημένα πόροι
- Διευθύνσεις IP του προσβεβλημένα

Πίσω από κάθε γράφημα, υπάρχουν επιπλέον ιδέες. Επιλέξτε ένα πλακίδιο για να δείτε περισσότερες πληροφορίες. Για παράδειγμα, το πλακίδιο **Κατάσταση ασφαλείας πόρων** δείχνει πρόσθετες λεπτομέρειες σχετικά με σε εκκρεμότητα συστάσεις από τους πόρους όπως φαίνεται στην παρακάτω οθόνη:

![Συστάσεις](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Εάν κάνετε κλικ σε κάθε γραμμή του αυτό το γράφημα, οι άλλες πρόκειται να γκρι ανάληψη και εστίαση μπορείτε μόνο σε αυτό που επιλέξατε. Για να επιστρέψετε στον πίνακα εργαλείων, κάντε κλικ στην επιλογή **Κέντρο ασφάλειας Azure** κάτω από την επιλογή **πίνακες εργαλείων** στο αριστερό τμήμα του παραθύρου αυτής της σελίδας.

> [AZURE.NOTE] Εάν θέλετε να προσαρμόσετε τις αναφορές σας με την προσθήκη επιπλέον πεδία ή αλλαγή υπάρχουσας οπτικά εφέ, μπορείτε να επεξεργαστείτε την αναφορά. Διαβάστε [Γλώσσες Προτ με μια αναφορά σε προβολή επεξεργασίας στο Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) για περισσότερες πληροφορίες.

Τα πλακίδια **ειδοποιήσεις διάρκεια του χρόνου, επιθέσεις πόρους** και **Διευθύνσεις IP εισβολέας** θα έχει το αποτέλεσμα του παρόμοια όταν κάνετε κλικ σε κάθε μία του. Αυτό συμβαίνει επειδή η αναφορά συγκεντρώνει πληροφορίες σχετικά με όλες αυτές τις τρεις μεταβλητές και κλήσεις **πόρους στην περιοχή επίθεση** όπως φαίνεται στην παρακάτω οθόνη:

![Πόροι στην περιοχή επίθεση](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

Σε αυτό το σημείο μπορείτε να επίσης αποθηκεύσετε ένα αντίγραφο του αυτήν την αναφορά, εκτυπώστε το ή δημοσίευση στο web, χρησιμοποιώντας τις επιλογές που είναι διαθέσιμες στο μενού **αρχείο** .

![Μενού "αρχείο"](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Εξερεύνηση των δεδομένων σας Κέντρο ασφάλειας Azure με υπηρεσίες του Power BI

Σύνδεση με τις [Υπηρεσίες πακέτου του Power BI περιεχομένου](https://msit.powerbi.com/groups/me/getdata/services) στο Power BI και εκτελέστε τα ακόλουθα βήματα:

1. Στο παράθυρο **Περιεχομένων πακέτου για το Power BI** θα δείτε δύο επιλογές όπως φαίνεται παρακάτω.

    ![Πακέτο περιεχομένου για το Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

    >[AZURE.NOTE] Εάν ήδη εκτελεστεί το πρώτο μέρος αυτού του άρθρου θα βλέπετε μόνο μία επιλογή, η οποία είναι η Διαχείριση πολιτικής Κέντρο ασφάλειας Azure.

2. Για αυτό το παράδειγμα, κάντε κλικ στην επιλογή **λήψη** στο πλακίδιο **Διαχείριση πολιτικών Κέντρο ασφαλείας Azure** .

3. Στο παράθυρο **σύνδεση με τη διαχείριση πολιτικών ασφαλείας κέντρο Azure** , βεβαιωθείτε ότι για να επιλέξετε **oAuth2** στην περιοχή **Μεθόδου ελέγχου ταυτότητας** αναπτυσσόμενης λίστας, όπως φαίνεται παρακάτω και κάντε κλικ στο κουμπί **Είσοδος** .

    ![Παράθυρο διαχείρισης πολιτικής](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)

4. Θα ανακατευθυνθείτε σε μια σελίδα ελέγχου ταυτότητας όπου θα πρέπει να πληκτρολογήσετε τα διαπιστευτήρια που χρησιμοποιείτε για να συνδεθείτε στο Κέντρο ασφάλειας Azure. Μετά την ολοκλήρωση της διαδικασίας ελέγχου ταυτότητας, θα ξεκινήσετε την εισαγωγή δεδομένων για τη δημιουργία αναφορών του Power BI. Σε αυτό το διάστημα, μπορείτε να δείτε το ακόλουθο μήνυμα στην δεξιά γωνία του προγράμματος περιήγησης:

    ![Σύνδεση στο Κέντρο ασφάλειας Azure με χρήση του Power BI](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] Όταν ο πίνακας εργαλείων δημιουργείται για πρώτη φορά μπορεί να διαρκέσει περισσότερο από το συνηθισμένο, κυρίως για σενάρια όπου έχετε πολλές συνδρομές. 

5. Μόλις ολοκληρωθεί η διαδικασία, στον πίνακα εργαλείων σας Power BI Azure ασφάλεια κέντρου θα φορτώνεται με την αναφορά **Πολιτικής διαχείρισης** παρόμοιο με αυτό που φαίνεται παρακάτω:

    ![Πίνακας εργαλείων διαχείρισης πολιτικής](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Δείτε επίσης
Σε αυτό το έγγραφο, μάθατε πώς μπορείτε να χρησιμοποιήσετε το Power BI στο Κέντρο ασφάλειας Azure. Για να μάθετε περισσότερα σχετικά με το Κέντρο ασφάλειας Azure, ανατρέξτε στα παρακάτω:

- [Οδηγός λειτουργίες προγραμματισμού Κέντρο ασφάλειας Azure και](security-center-planning-and-operations-guide.md) — μάθετε πώς να σχεδιάσετε υιοθέτησης Κέντρο ασφάλειας Azure.
- [Ρύθμιση πολιτικών ασφάλειας στο Κέντρο ασφάλειας Azure](security-center-policies.md) — μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους ασφαλείας στο Κέντρο ασφάλειας Azure
- [Διαχείριση και απάντηση σε ειδοποιήσεις ασφαλείας στο Κέντρο ασφάλειας Azure](security-center-managing-and-responding-alerts.md) — μάθετε πώς μπορείτε να διαχειριστείτε και να απαντούν σε ειδοποιήσεων ασφαλείας
- [Συνήθεις Ερωτήσεις για το Κέντρο ασφάλειας Azure](security-center-faq.md) — Εύρεση συνήθεις ερωτήσεις σχετικά με τη χρήση της υπηρεσίας
- [Ιστολόγιο ασφαλείας Azure](http://blogs.msdn.com/b/azuresecurity/) — βρείτε καταχωρήσεις ιστολογίου σχετικά με Azure ασφάλεια και συμμόρφωση
