<properties
 pageTitle="Γρήγορα αποτελέσματα με το χρονοδιάγραμμα Azure στην πύλη Azure | Microsoft Azure"
 description="Γρήγορα αποτελέσματα με το χρονοδιάγραμμα Azure στην πύλη του Azure"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/10/2016"
 ms.author="deli"/>

# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Γρήγορα αποτελέσματα με το χρονοδιάγραμμα Azure στην πύλη του Azure

Είναι εύκολο να δημιουργήσετε προγραμματισμένες εργασίες στο χρονοδιάγραμμα Azure. Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς μπορείτε να δημιουργήσετε μια εργασία. Μπορείτε, επίσης, θα μάθετε δυνατοτήτων παρακολούθησης και διαχείρισης του χρονοδιαγράμματος.

## <a name="create-a-job"></a>Δημιουργία μιας εργασίας

1.  Πραγματοποιήστε είσοδο στην [πύλη Azure](https://portal.azure.com/).  

2.  Κάντε κλικ στο κουμπί **+ Δημιουργία** > πληκτρολογήστε _Χρονοδιάγραμμα_ στο πλαίσιο αναζήτησης > Επιλέξτε **χρονοδιαγράμματος** στα αποτελέσματα > κάντε κλικ στην επιλογή **Δημιουργία**.

     ![][marketplace-create]

3.  Ας δημιουργήσουμε μια εργασία που απλώς επισκέψεις http://www.microsoft.com/ με ένα αίτημα GET. Στην οθόνη **Χρονοδιάγραμμα έργου** , εισαγάγετε τις παρακάτω πληροφορίες:

    1.  **Όνομα:**`getmicrosoft`  

    2.  **Συνδρομή:** Azure τη συνδρομή σας   

    3.  **Εργασία συλλογής:** Επιλέξτε μια υπάρχουσα συλλογή εργασία ή κάντε κλικ στην επιλογή **Δημιουργία νέου** > πληκτρολογήστε ένα όνομα.

4.  Στη συνέχεια, στις **Ρυθμίσεις ενέργειας**, καθορίστε τις ακόλουθες τιμές:

    1.  **Τύπος ενέργειας:**` HTTP`  

    2.  **Μέθοδος:**`GET`  

    3.  **Διεύθυνση URL:**` http://www.microsoft.com`  

      ![][action-settings]

5.  Τέλος, ας Ορισμός χρονοδιαγράμματος. Η εργασία μπορεί να οριστεί ως εφάπαξ εργασία, αλλά επιλέξτε ας χρονοδιαγράμματος Περιοδικότητα:

    1. **Περιοδικότητα**:`Recurring`

    2. **Έναρξη**: σημερινή ημερομηνία

    3. **Περιοδικού κάθε**:`12 Hours`

    4. **Λήξη μέχρι**: δύο ημέρες μετά τη σημερινή ημέρα της ημερομηνίας  

      ![][recurrence-schedule]

6.  Κάντε κλικ στην επιλογή **Δημιουργία**

## <a name="manage-and-monitor-jobs"></a>Διαχειριστείτε και να παρακολουθείτε τις εργασίες

Όταν δημιουργηθεί μια εργασία, εμφανίζεται στο κύριο Azure πίνακα εργαλείων. Κάντε κλικ στην εργασία και ένα νέο παράθυρο ανοίγει με τις παρακάτω καρτέλες:

1.  Ιδιότητες  

2.  Ρυθμίσεις ενέργειας  

3.  Χρονοδιάγραμμα  

4.  Ιστορικό

5.  Οι χρήστες

    ![][job-overview]

### <a name="properties"></a>Ιδιότητες

Αυτές οι ιδιότητες μόνο για ανάγνωση περιγράφουν τα μετα-δεδομένα διαχείρισης για το έργο χρονοδιαγράμματος.

   ![][job-properties]


### <a name="action-settings"></a>Ρυθμίσεις ενέργειας

Κάνοντας κλικ σε μια εργασία σε **έργα** οθόνης σάς επιτρέπει να ρυθμίσετε την εργασία. Αυτό σας επιτρέπει να ορίσετε πρόσθετες ρυθμίσεις, εάν δεν μπορείτε να ρυθμίσετε τις παραμέτρους τους στο του Οδηγού γρήγορης δημιουργίας.

Για όλους τους τύπους ενεργειών, μπορείτε να αλλάξετε την πολιτική "Επανάληψη" και την ενέργεια σφάλματος.

Για τους τύπους εργασίας ενέργεια HTTP και HTTPS, μπορείτε να αλλάξετε τη μέθοδο για οποιαδήποτε επιτρεπόμενων Ρηματικές HTTP. Μπορείτε επίσης να προσθέσετε, διαγράψτε ή αλλάξτε τις κεφαλίδες και τις πληροφορίες βασικό έλεγχο ταυτότητας.

Για τους τύπους ενεργειών ουρά χώρου αποθήκευσης, μπορείτε να αλλάξετε το λογαριασμό χώρου αποθήκευσης, όνομα ουράς, διακριτικό συσχετισμών Ασφαλείας και σώμα.

Για τους τύπους ενεργειών bus υπηρεσίας, μπορείτε να αλλάξετε το χώρο ονομάτων, το θέμα ή ουράς διαδρομή, ρυθμίσεις ελέγχου ταυτότητας, τύπος μεταφοράς, ιδιότητες μηνύματος και σώμα του μηνύματος.

   ![][job-action-settings]

### <a name="schedule"></a>Χρονοδιάγραμμα

Αυτό σας επιτρέπει να ρυθμίσει ξανά τις παραμέτρους του χρονοδιαγράμματος, εάν θέλετε να αλλάξετε το χρονοδιάγραμμα που δημιουργήσατε στο του Οδηγού γρήγορης δημιουργίας.

Αυτή είναι μια ευκαιρία για να δημιουργήσετε [σύνθετα χρονοδιαγράμματα και για προχωρημένους Περιοδικότητα στην εργασία σας](scheduler-advanced-complexity.md)

Μπορείτε να αλλάξετε την ημερομηνία έναρξης και ώρα, χρονοδιάγραμμα περιοδικότητα και τέλος ημερομηνία και ώρα (Εάν η εργασία είναι περιοδική.)

   ![][job-schedule]


### <a name="history"></a>Ιστορικό

Στην καρτέλα **ιστορικό** εμφανίζει επιλεγμένο μετρήσεις για κάθε εργασία εκτέλεσης στο σύστημα για την επιλεγμένη εργασία. Αυτές οι μετρήσεις παρέχουν τιμές σε πραγματικό χρόνο σχετικά με την εύρυθμη λειτουργία του χρονοδιαγράμματος σας:

1.  Κατάσταση  

2.  Λεπτομέρειες  

3.  Προσπάθειες επανάληψης

4.  Εμφάνιση: 1ος, 2ος, 3ος, κ.λπ.

5.  Ώρα έναρξης της εκτέλεσης  

6.  Ώρα λήξης της εκτέλεσης

   ![][job-history]

Μπορείτε να κάνετε κλικ σε μια εκτέλεση για να προβάλετε τις **Λεπτομέρειες ιστορικό**, συμπεριλαμβανομένων των ολόκληρων απάντηση για κάθε εκτέλεση. Αυτό το πλαίσιο διαλόγου σας επιτρέπει επίσης να αντιγράψετε την απάντηση στο Πρόχειρο.

   ![][job-history-details]

### <a name="users"></a>Οι χρήστες

Azure βάσει ρόλων πρόσβασης ελέγχου (RBAC) επιτρέπει λεπτομερή πρόσβασης διαχείρισης για το χρονοδιάγραμμα Azure. Για να μάθετε πώς μπορείτε να χρησιμοποιήσετε την καρτέλα χρήστες, ανατρέξτε στο [Στοιχείο ελέγχου πρόσβασης Azure Role-Based](../active-directory/role-based-access-control-configure.md)


## <a name="see-also"></a>Δείτε επίσης

 [Τι είναι το χρονοδιάγραμμα;](scheduler-intro.md)

 [Χρονοδιάγραμμα έννοιες, ορολογία και οντότητα ιεραρχία](scheduler-concepts-terms.md)

 [Προγράμματα και Τιμολόγηση στο χρονοδιάγραμμα Azure](scheduler-plans-billing.md)

 [Πώς μπορείτε να δημιουργήσετε σύνθετα χρονοδιαγράμματα και για προχωρημένους περιοδικότητας με το χρονοδιάγραμμα Azure](scheduler-advanced-complexity.md)

 [Αναφορά REST API του χρονοδιαγράμματος](https://msdn.microsoft.com/library/mt629143)

 [Αναφορά των cmdlet του PowerShell χρονοδιαγράμματος](scheduler-powershell-reference.md)

 [Χρονοδιάγραμμα υψηλή διαθεσιμότητα και την αξιοπιστία](scheduler-high-availability-reliability.md)

 [Όρια χρονοδιάγραμμα, προεπιλεγμένες τιμές και τους κωδικούς σφάλματος](scheduler-limits-defaults-errors.md)

 [Έλεγχος ταυτότητας εξερχομένων χρονοδιαγράμματος](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
