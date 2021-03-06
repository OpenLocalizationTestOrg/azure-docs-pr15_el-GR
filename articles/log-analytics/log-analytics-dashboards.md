<properties
    pageTitle="Δημιουργία ενός προσαρμοσμένου πίνακα εργαλείων στο αρχείο καταγραφής ανάλυσης | Microsoft Azure"
    description="Αυτός ο οδηγός σάς βοηθά να κατανοήσετε τον τρόπο καταγραφής πίνακες εργαλείων αναλυτικών στοιχείων μπορεί να απεικόνιση όλες τις αναζητήσεις αποθηκευμένο αρχείο καταγραφής, παρέχοντας μια μεμονωμένη φακού για να προβάλετε το περιβάλλον."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="create-a-custom-dashboard-in-log-analytics"></a>Δημιουργία ενός προσαρμοσμένου πίνακα εργαλείων στο αρχείο καταγραφής ανάλυσης

Αυτός ο οδηγός σάς βοηθά να κατανοήσετε τον τρόπο καταγραφής αναλυτικών στοιχείων πινάκων εργαλείων να οπτικοποιήσετε όλες τις αναζητήσεις αποθηκευμένο αρχείο καταγραφής, παρέχοντας ένα μεμονωμένο φακού για να προβάλετε το περιβάλλον σας.

![Παράδειγμα πίνακα εργαλείων](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Όλα τα προσαρμοσμένα πίνακες εργαλείων που δημιουργείτε στην πύλη του OMS είναι επίσης διαθέσιμες στην εφαρμογή Mobile OMS. Ανατρέξτε στο θέμα οι ακόλουθες σελίδες για περισσότερες πληροφορίες σχετικά με τις εφαρμογές.

- [Εφαρμογή για κινητές συσκευές OMS από το χώρο αποθήκευσης της Microsoft](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
- [Εφαρμογή για κινητές συσκευές OMS από το Apple iTunes](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![κινητές συσκευές πίνακα εργαλείων](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Πώς μπορώ να δημιουργήσω πίνακα εργαλείων μου;

Για να ξεκινήσετε, μεταβείτε στη σελίδα Επισκόπηση OMS. Θα δείτε το πλακίδιο **Μου πίνακα εργαλείων** στην αριστερή πλευρά. Κάντε κλικ για να κάνετε Διερεύνηση σε πίνακα εργαλείων σας.

![Επισκόπηση](./media/log-analytics-dashboards/oms-dashboards-overview.png)


## <a name="adding-a-tile"></a>Προσθήκη ένα πλακίδιο

Στους πίνακες εργαλείων, τα πλακίδια είναι υποστηρίζεται από το αποθηκευμένο αρχείο καταγραφής αναζητήσεις. OMS συνοδεύεται από πολλά προ-κάνει αναζητήσεις, το αποθηκευμένο αρχείο καταγραφής, ώστε να μπορείτε να ξεκινήσετε αμέσως. Χρησιμοποιήστε τα ακόλουθα βήματα που περιγράφουν τον τρόπο για να ξεκινήσετε.

Μου πίνακα εργαλείων της προβολής, απλώς κάντε κλικ στην επιλογή **Προσαρμογή** , για να εισαγάγετε Προσαρμογή λειτουργία.

![Εικόνα](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Το παράθυρο που ανοίγει στη δεξιά πλευρά της σελίδας εμφανίζει όλων των αναζητήσεις αποθηκευμένο αρχείο καταγραφής του χώρου εργασίας σας. Για την απεικόνιση μιας αναζήτησης αποθηκευμένο αρχείο καταγραφής ως πλακίδιο, τοποθετήστε το δείκτη επάνω από μια αποθηκευμένη αναζήτηση και, στη συνέχεια, κάντε κλικ στο σύμβολο **συν** .

![Προσθήκη πλακιδίων 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Όταν κάνετε κλικ στο σύμβολο **συν** , ένα νέο πλακίδιο εμφανίζεται στην προβολή πίνακα εργαλείων μου.

![Προσθήκη πλακιδίων 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)


## <a name="edit-a-tile"></a>Επεξεργασία πλακιδίου

Μου πίνακα εργαλείων της προβολής, απλώς κάντε κλικ στην επιλογή **Προσαρμογή** , για να εισαγάγετε Προσαρμογή λειτουργία. Κάντε κλικ στο πλακίδιο που θέλετε να επεξεργαστείτε. Οι αλλαγές δεξιό τμήμα του παραθύρου για να επεξεργαστείτε, και σας προσφέρει μια επιλογή επιλογές:

![Επεξεργασία πλακιδίου](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Επεξεργασία πλακιδίου](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Απεικονίσεις πλακιδίου#
Υπάρχουν τρία είδη απεικονίσεις πλακιδίου για να επιλέξετε:

|Τύπος γραφήματος|Τι κάνει|
|---|---|
|![Γράφημα ράβδων](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Εμφανίζει μια λωρίδα χρόνου των αποτελεσμάτων της αναζήτησης το αποθηκευμένο αρχείο καταγραφής με ένα γράφημα ράβδων ή σε μια λίστα των αποτελεσμάτων από ένα πεδίο ανάλογα με την αναζήτηση καταγραφής συγκεντρώνει τα αποτελέσματα με βάση ένα πεδίο ή όχι.
|![μέτρηση](./media/log-analytics-dashboards/oms-dashboards-metric.png)|Εμφανίζει σας επισκέψεων αποτέλεσμα αναζήτησης συνολικό καταγραφής ως έναν αριθμό σε ένα πλακίδιο. Τα πλακίδια μετρικό σάς επιτρέπουν να ορίσετε μια οριακή τιμή που θα επισημάνετε το πλακίδιο όταν φτάσει το όριο.|
|![γραμμή](./media/log-analytics-dashboards/oms-dashboards-line.png)|Εμφανίζει μια λωρίδα χρόνου σας επισκέψεων αποτέλεσμα αναζήτησης αποθηκευμένο αρχείο καταγραφής με τις τιμές ως γράφημα γραμμών.|

### <a name="threshold"></a>Όριο
Μπορείτε να δημιουργήσετε μια οριακή τιμή σε ένα πλακίδιο χρησιμοποιώντας την απεικόνιση μετρικό. Επιλέξτε στο για να δημιουργήσετε μια οριακή τιμή στο πλακίδιο. Επιλέξτε εάν θέλετε να επισημάνετε το πλακίδιο όταν η τιμή είναι επάνω ή κάτω από το επιλεγμένο όριο, στη συνέχεια, ορίστε την παρακάτω οριακή τιμή.

## <a name="organizing-the-dashboard"></a>Οργάνωση του πίνακα εργαλείων
Για να οργανώσετε στον πίνακα εργαλείων σας, μεταβείτε στην προβολή πίνακα εργαλείων μου και κάντε κλικ στην επιλογή **Προσαρμογή** , για να εισαγάγετε Προσαρμογή λειτουργία. Κάντε κλικ και σύρετε το πλακίδιο που θέλετε να μετακινήσετε και μετακινήστε το στο σημείο όπου θέλετε το πλακίδιο για να.

![Οργάνωση του πίνακα εργαλείων](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Καταργήστε ένα πλακίδιο
Για να καταργήσετε ένα πλακίδιο, μεταβείτε στην προβολή πίνακα εργαλείων μου και κάντε κλικ στην επιλογή **Προσαρμογή** , για να εισαγάγετε Προσαρμογή λειτουργία. Επιλέξτε το πλακίδιο που θέλετε να καταργήσετε και, στη συνέχεια, στο δεξιό παράθυρο, επιλέξτε **Κατάργηση πλακιδίου**.

![Καταργήστε ένα πλακίδιο](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Επόμενα βήματα

- Δημιουργία [ειδοποιήσεων](log-analytics-alerts.md) στο αρχείο καταγραφής αναλυτικών στοιχείων για τη δημιουργία ειδοποιήσεων και για τη διόρθωση προβλημάτων.
