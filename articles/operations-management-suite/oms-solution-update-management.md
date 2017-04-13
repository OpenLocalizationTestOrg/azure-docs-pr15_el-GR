<properties
    pageTitle="Ενημέρωση της λύσης διαχείρισης στο OMS | Microsoft Azure"
    description="Σε αυτό το άρθρο προορίζονται για να σας βοηθήσει να κατανοήσετε τον τρόπο χρήσης αυτής της λύσης για να διαχειριστείτε ενημερώσεις για τους υπολογιστές σας Windows και Linux."
    services="operations-management-suite"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="operations-management-suite"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="magoedte"/>

# <a name="update-management-solution-in-omsmediaoms-solution-update-managementupdate-management-solution-iconpng-update-management-solution-in-oms"></a>![Ενημέρωση της λύσης διαχείρισης στο OMS](./media/oms-solution-update-management/update-management-solution-icon.png) Ενημέρωση της λύσης διαχείρισης στο OMS

Της λύσης διαχείρισης ενημέρωσης στο OMS σάς επιτρέπει να διαχειριστείτε ενημερώσεις για τους υπολογιστές σας Windows και Linux.  Μπορείτε γρήγορα να αξιολογεί την κατάσταση των διαθέσιμων ενημερώσεων σε όλους τους υπολογιστές παράγοντας και να ξεκινήσετε τη διαδικασία εγκατάστασης απαιτούμενες ενημερώσεις για τους διακομιστές. 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

-   Παράγοντες Windows είτε πρέπει να ρυθμιστεί για να επικοινωνήσετε με ένα διακομιστή Windows Server Update Services (WSUS) ή να έχετε πρόσβαση στο Microsoft Update.  

    >[AZURE.NOTE] Τον παράγοντα Windows δεν είναι δυνατό να γίνεται ταυτόχρονα με System Center Configuration Manager.  
  
-   Παράγοντες Linux πρέπει να έχουν πρόσβαση σε ένα αποθετήριο ενημερωμένη έκδοση.  Ο παράγοντας OMS για Linux μπορούν να ληφθούν από [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Ρύθμιση παραμέτρων

Ακολουθήστε τα παρακάτω βήματα για να προσθέσετε τη λύση διαχείριση ενημερώσεων του χώρου εργασίας σας OMS και προσθήκη Linux παραγόντων.  Παράγοντες Windows προστίθενται αυτόματα με χωρίς πρόσθετη ρύθμιση παραμέτρων.

1.  Προσθήκη της λύσης διαχείρισης ενημέρωσης στο χώρο εργασίας σας OMS χρησιμοποιώντας τη διαδικασία που περιγράφεται στην [Προσθήκη OMS λύσεις](../log-analytics/log-analytics-add-solutions.md) από τη συλλογή λύσεων.  
2.  Στην πύλη του OMS, επιλέξτε **Ρυθμίσεις** και, στη συνέχεια, **Συνδεδεμένοι προελεύσεις**.  Σημειώστε το **Αναγνωριστικό χώρου εργασίας** και το **πρωτεύον κλειδί** ή **δευτερεύον κλειδί**.
3.  Ακολουθήστε τα παρακάτω βήματα για κάθε υπολογιστή Linux.

    μια.  Εγκαταστήστε την πιο πρόσφατη έκδοση του παράγοντα OMS για Linux, εκτελώντας τις παρακάτω εντολές.  Αντικατάσταση <Workspace ID> με το Αναγνωριστικό του χώρου εργασίας και <Key> με το πρωτεύον ή δευτερεύον κλειδί.

        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

     β. Για να καταργήσετε τον παράγοντα, εκτελέστε την ακόλουθη εντολή.

        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Πακέτα διαχείρισης

Εάν σας ομάδα διαχείρισης συστήματος Center Operations Manager είναι συνδεδεμένη στο χώρο εργασίας σας OMS, στη συνέχεια, τα ακόλουθα πακέτα διαχείρισης θα εγκατασταθεί σε Operations Manager όταν προσθέτετε αυτήν τη λύση. Δεν υπάρχει ρύθμιση παραμέτρων ή συντήρηση των αυτά τα πακέτα διαχείρισης απαιτείται. 

-   Κέντρο συστήματος Microsoft Σύμβουλος ενημέρωση αξιολόγησης πληροφοριών πακέτο (Microsoft.IntelligencePacks.UpdateAssessment)
-   Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
-   Ενημέρωση ανάπτυξης πολλών Επεξεργαστών

Για περισσότερες πληροφορίες σχετικά με πώς μπορείτε να ενημερώσετε τα πακέτα διαχείρισης λύση, ανατρέξτε στο θέμα [Σύνδεση Operations Manager για ανάλυση καταγραφής](../log-analytics/log-analytics-om-agents.md).

## <a name="data-collection"></a>Συλλογή δεδομένων

### <a name="supported-agents"></a>Υποστηριζόμενες παραγόντων

Ο παρακάτω πίνακας περιγράφει τα συνδεδεμένα αρχεία προέλευσης που υποστηρίζονται από αυτήν τη λύση.

Συνδεδεμένο αρχείο προέλευσης | Υποστηρίζονται | Περιγραφή|
----------|----------|----------|
Παράγοντες των Windows | Ναι | Η λύση συλλέγει πληροφορίες σχετικά με τις ενημερώσεις συστήματος από Windows παράγοντες και ξεκινά εγκατάσταση των ενημερώσεων απαιτείται.|
Παράγοντες Linux | Ναι | Η λύση συλλέγει πληροφορίες σχετικά με τις ενημερώσεις συστήματος από παράγοντες Linux.|
Ομάδα διαχείρισης Operations Manager | Ναι | Η λύση συλλέγει πληροφορίες σχετικά με τις ενημερώσεις συστήματος από παράγοντες σε μια ομάδα διαχείρισης συνδεδεμένοι.<br>Δεν απαιτείται μια απευθείας σύνδεση από τον παράγοντα Operations Manager με ανάλυση καταγραφής. Δεδομένα προωθείται από την ομάδα διαχείρισης του αποθετηρίου OMS.|
Λογαριασμός Azure χώρου αποθήκευσης | Όχι | Azure χώρου αποθήκευσης δεν περιλαμβάνει πληροφορίες σχετικά με ενημερώσεις του συστήματος.|  

### <a name="collection-frequency"></a>Συχνότητα συλλογής

Για κάθε διαχειριζόμενων υπολογιστή των Windows, εκτελείται μια σάρωση δύο φορές την ημέρα.  Κατά την εγκατάσταση μιας ενημέρωσης, τις πληροφορίες είναι ενημερωθεί μέσα σε 15 λεπτά.  

Για κάθε διαχειριζόμενων Linux υπολογιστή, μια σάρωση εκτελείται κάθε 3 ώρες.  

## <a name="using-the-solution"></a>Χρήση της λύσης

Κατά την προσθήκη της λύσης διαχείρισης ενημέρωσης στο χώρο εργασίας σας OMS, το πλακίδιο **Διαχείριση ενημερώσεων** θα προστεθεί στον πίνακα εργαλείων σας OMS. Αυτό το πλακίδιο εμφανίζει ένα σύνολο και γραφική αναπαράσταση του αριθμού των υπολογιστών στο περιβάλλον σας αυτήν τη στιγμή που απαιτεί ενημερωμένες εκδόσεις του συστήματος.<br><br>
![Ενημέρωση σύνοψης πλακίδιο διαχείρισης](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Προβολή αξιολογήσεις ενημέρωσης

Κάντε κλικ στο πλακίδιο **Διαχείριση ενημερώσεων** για να ανοίξετε τον πίνακα εργαλείων **Διαχείρισης ενημερωμένη έκδοση** . Πίνακας εργαλείων περιλαμβάνει τις στήλες στον παρακάτω πίνακα. Κάθε στήλη παραθέτει έως δέκα στοιχεία που συμφωνούν με κριτήρια αυτής της στήλης για την καθορισμένη εμβέλεια και το χρονικό διάστημα. Μπορείτε να εκτελέσετε μια αναζήτηση καταγραφής που επιστρέφει όλες τις εγγραφές, κάνοντας κλικ στην επιλογή **δείτε όλες** στο κάτω μέρος της στήλης ή κάνοντας κλικ στην κεφαλίδα της στήλης.

Στήλη | Περιγραφή|
----------|----------|
**Υπολογιστές που λείπουν ενημερώσεις** ||
Κρίσιμη ή ενημερώσεις ασφαλείας | Λίστες επάνω δέκα υπολογιστές που λείπουν ενημερώσεις ταξινομημένα κατά τον αριθμό των ενημερώσεων βρίσκονται λείπει. Κάντε κλικ σε ένα όνομα υπολογιστή για να εκτελέσετε μια αναζήτηση καταγραφής επιστρέφει όλα ενημερώνετε εγγραφές για αυτόν τον υπολογιστή.|
Κρίσιμη ή ενημερώσεις ασφαλείας που είναι παλαιότερες από 30 ημέρες| Αριθμός των υπολογιστών που προσδιορίζει λείπουν κρίσιμων ή ομαδοποιημένα κατά τη διάρκεια του χρόνου μετά την ενημέρωση δημοσιεύτηκε ενημερώσεις ασφαλείας. Κάντε κλικ σε μία από τις καταχωρήσεις για την εκτέλεση μιας αναζήτησης καταγραφής επιστρέφει όλες τις ενημερώσεις που λείπουν και κρίσιμη.|
**Απαιτούνται ενημερώσεις που λείπουν**||
Κρίσιμη ή ενημερώσεις ασφαλείας | Παραθέτει σε λίστα ταξινομήσεις ενημερώσεις ότι υπολογιστές λείπουν ταξινομημένες κατά τον αριθμό των υπολογιστών που λείπουν ενημερώσεις στην κατηγορία. Κάντε κλικ σε μια ταξινόμηση για να εκτελέσετε μια αναζήτηση καταγραφής επιστρέφοντας όλα ενημέρωση εγγραφών για αυτήν την κατηγορία.|
**Ενημέρωση αναπτύξεις**||
Ενημέρωση αναπτύξεις | Αριθμός προγραμματισμένη ενημερωμένης έκδοσης αναπτύξεων και της διάρκειας μέχρι την επόμενη προγραμματισμένη εκτέλεση.  Κάντε κλικ στο πλακίδιο για προβολή χρονοδιαγραμμάτων, εκτελούνται τη συγκεκριμένη στιγμή και ολοκληρωμένες ενημερώσεις ή για να προγραμματίσετε μια νέα ανάπτυξη.|  
<br>  
![Ενημέρωση σύνοψης πίνακα εργαλείων διαχείρισης](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Ενημέρωση προβολή υπολογιστή πίνακα εργαλείων διαχείρισης](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Ενημέρωση προβολής του πακέτου εργαλείων διαχείρισης](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Εγκατάσταση ενημερώσεων

Αφού έχουν εκτιμήσετε ενημερώσεις για όλους τους υπολογιστές των Windows στο περιβάλλον σας, μπορεί να διαθέτετε τα απαραίτητα εγκαταστήσει, δημιουργώντας μια *Ενημέρωση, η ανάπτυξη*ενημερώσεις.  Μια ανάπτυξη ενημερωμένη έκδοση είναι μια προγραμματισμένη εγκατάσταση απαιτούμενες ενημερώσεις για έναν ή περισσότερους υπολογιστές των Windows.  Μπορείτε να καθορίσετε την ημερομηνία και ώρα για την ανάπτυξη εκτός από τον υπολογιστή ή ομάδα υπολογιστών που θα πρέπει να συμπεριλαμβάνονται.  

Ενημερώσεις εγκαθίστανται από runbooks στο Azure αυτοματισμού.  Προς το παρόν, δεν μπορείτε να προβάλετε αυτά τα runbooks, και δεν απαιτούν οποιαδήποτε ρύθμιση παραμέτρων.  Όταν δημιουργείται μια ανάπτυξη ενημέρωση, δημιουργείται ένα χρονοδιάγραμμα στο που ξεκινά μια κύρια ενημέρωση runbook την καθορισμένη ώρα για τους υπολογιστές που περιλαμβάνονται.  Αυτό κύρια runbook ξεκινά μια runbook θυγατρικό κάθε παράγοντα Windows που πραγματοποιεί εγκατάσταση των απαιτούμενες ενημερώσεις.  

### <a name="viewing-update-deployments"></a>Προβολή ενημερωμένης έκδοσης αναπτύξεων

Κάντε κλικ στο πλακίδιο **Ανάπτυξης ενημέρωσης** για να προβάλετε τη λίστα των υπάρχοντα ενημερωμένης έκδοσης αναπτύξεων.  Να είναι ομαδοποιημένα κατά κατάσταση – **προγραμματισμένη**, **εκτελεί**και **ολοκληρώθηκε**.<br><br> ![Σελίδα χρονοδιαγράμματος ενημερωμένης έκδοσης αναπτύξεων](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

Στον παρακάτω πίνακα περιγράφονται οι ιδιότητες που εμφανίζονται για κάθε ενημέρωση, η ανάπτυξη.

Ιδιότητα | Περιγραφή|
----------|----------|
Όνομα | Όνομα της ανάπτυξης ενημερωμένη έκδοση.|
Χρονοδιάγραμμα | Τύπος του χρονοδιαγράμματος.  *OneTime* αυτήν τη στιγμή είναι η μόνη πιθανή τιμή.|
Ώρα έναρξης|Ημερομηνία και ώρα που έχει προγραμματιστεί ανάπτυξης ενημέρωσης για να ξεκινήσετε.|
Διάρκεια | Αριθμός των λεπτών που επιτρέπεται να εκτελεστεί η ενημέρωση, η ανάπτυξη.  Εάν δεν έχουν εγκατασταθεί όλες τις ενημερώσεις μέσα σε αυτήν τη διάρκεια, στη συνέχεια, τις υπόλοιπες ενημερωμένες εκδόσεις πρέπει να περιμένετε μέχρι την επόμενη ενημέρωση, η ανάπτυξη.|
Οι διακομιστές | Αριθμός των υπολογιστών που επηρεάζονται από την ανάπτυξη ενημερωμένη έκδοση.|
Κατάσταση | Τρέχουσα κατάσταση της ανάπτυξης ενημερωμένη έκδοση.<br><br>Πιθανές τιμές είναι:<br>-Δεν έχει αρχίσει<br>-Εκτελείται<br>-Ολοκληρώθηκε|  

Κάντε κλικ σε μια ανάπτυξη ενημέρωσης για να προβάλετε την οθόνη λεπτομερειών που περιλαμβάνει τις στήλες στον παρακάτω πίνακα.  Αυτές οι στήλες δεν θα είναι συμπληρώνεται εάν ανάπτυξης Update δεν έχει αρχίσει ακόμα.<br>

Στήλη | Περιγραφή|
----------|----------|
**Αποτελέσματα υπολογιστή**||
Ολοκληρώθηκε με επιτυχία | Παραθέτει τον αριθμό των υπολογιστών στην ανάπτυξη ενημέρωση κατά κατάσταση.  Κάντε κλικ σε μια κατάσταση για να εκτελέσετε μια αναζήτηση καταγραφής επιστρέφει όλα ενημερώνετε εγγραφές με αυτήν την κατάσταση για την ανάπτυξη ενημέρωση.|
Κατάσταση εγκατάστασης του υπολογιστή| Παραθέτει σε λίστα τους υπολογιστές που σχετίζονται με την ανάπτυξη της ενημερωμένης έκδοσης και το ποσοστό των ενημερώσεων που εγκαταστάθηκε με επιτυχία. Κάντε κλικ σε μία από τις καταχωρήσεις για την εκτέλεση μιας αναζήτησης καταγραφής επιστρέφει όλες τις ενημερώσεις που λείπουν και κρίσιμη.|
**Ενημέρωση των αποτελεσμάτων παρουσίας**||
Εμφάνιση κατάστασης εγκατάστασης | Παραθέτει σε λίστα ταξινομήσεις ενημερώσεις ότι υπολογιστές λείπουν ταξινομημένες κατά τον αριθμό των υπολογιστών που λείπουν ενημερώσεις στην κατηγορία. Κάντε κλικ σε έναν υπολογιστή για να εκτελέσετε μια αναζήτηση καταγραφής επιστρέφει όλα ενημερώνετε εγγραφές για αυτόν τον υπολογιστή.|  
<br><br> ![Επισκόπηση των αποτελεσμάτων ανάπτυξης ενημέρωσης](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Δημιουργία μιας ανάπτυξης ενημέρωσης

Δημιουργία νέας ανάπτυξης ενημέρωσης, κάνοντας κλικ στο κουμπί **Προσθήκη** στο επάνω μέρος της οθόνης για να ανοίξετε τη σελίδα **Νέας ανάπτυξης ενημερωμένη έκδοση** .  Πρέπει να δώσετε τιμές για τις ιδιότητες στον παρακάτω πίνακα.

Ιδιότητα | Περιγραφή|
----------|----------|
Όνομα | Μοναδικό όνομα για τον προσδιορισμό ανάπτυξης ενημερωμένη έκδοση.|
Ζώνη ώρας | Ζώνη ώρας για να χρησιμοποιήσετε για την ώρα έναρξης.|
Ώρα έναρξης | Ημερομηνία και ώρα για να ξεκινήσετε την ανάπτυξη ενημερωμένη έκδοση.|
Διάρκεια | Αριθμός των λεπτών που επιτρέπεται να εκτελεστεί η ενημέρωση, η ανάπτυξη.  Εάν δεν έχουν εγκατασταθεί όλες τις ενημερώσεις μέσα σε αυτήν τη διάρκεια, στη συνέχεια, τις υπόλοιπες ενημερωμένες εκδόσεις πρέπει να περιμένετε μέχρι την επόμενη ενημέρωση, η ανάπτυξη.|
Υπολογιστές | Ονόματα υπολογιστές ή ομάδες υπολογιστών για να συμπεριλάβετε στην ανάπτυξη ενημερωμένη έκδοση.  Επιλέξτε μία ή περισσότερες εγγραφές από την αναπτυσσόμενη λίστα.|
<br><br> ![Νέα σελίδα ανάπτυξης ενημέρωσης](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Περιοχής ώρας

Από προεπιλογή, το εύρος των δεδομένων ανάλυση της λύσης διαχείρισης ενημερωμένη έκδοση είναι από όλες τις ομάδες διαχείρισης συνδεδεμένου δημιουργηθεί μέσα την τελευταία ημέρα 1. 

Για να αλλάξετε την περιοχή χρόνο των δεδομένων, επιλέξτε τη **βάση δεδομένων** στο επάνω μέρος του πίνακα εργαλείων. Μπορείτε να επιλέξετε εγγραφές δημιουργήθηκε ή ενημερώθηκε τις τελευταίες 7 ημέρες, 1 ημέρα ή 6 ώρες. Ή μπορείτε να επιλέξετε **προσαρμοσμένο** και να καθορίσετε μια προσαρμοσμένη περιοχή ημερομηνιών.<br><br> ![Επιλογή προσαρμοσμένης περιοχής ώρας](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Ανάλυση εγγραφών του αρχείου καταγραφής

Η λύση διαχείριση ενημερώσεων δημιουργεί δύο τύποι εγγραφών στο αποθετήριο OMS.

### <a name="update-records"></a>Ενημέρωση εγγραφών

Μια εγγραφή με έναν τύπο **Ενημέρωση** δημιουργείται για κάθε ενημερωμένη έκδοση που είναι εγκατεστημένη ή απαιτείται σε κάθε υπολογιστή. Ενημερώνετε εγγραφές έχουν τις ιδιότητες στον παρακάτω πίνακα.

Ιδιότητα | Περιγραφή|
----------|----------|
Τύπος | *Ενημέρωση*|
SourceSystem | Η προέλευση που έχουν εγκριθεί εγκατάστασης της ενημέρωσης.<br>Πιθανές τιμές είναι:<br>-Microsoft Update<br>-Windows Update<br>-SCCM<br>-Οι διακομιστές Linux (λήψη από το πακέτο διαχειριστές)|
Έγκριση | Καθορίζει εάν έχει εγκριθεί για την εγκατάσταση της ενημερωμένης έκδοσης.<br> Για τους διακομιστές Linux αυτό είναι προαιρετικό αυτήν τη στιγμή ως ενημερωμένων δεν διαχειρίζεται OMS.|
Ταξινόμηση για Windows | Ταξινόμηση της ενημέρωσης.<br>Πιθανές τιμές είναι:<br>-Εφαρμογές<br>-Κρίσιμων ενημερώσεων<br>-Ενημερώσεις ορισμού<br>-Πακέτα δυνατοτήτων<br>-Ενημερώσεις ασφαλείας<br>-Τα service pack<br>-Συναθροίσεις<br>-Ενημερώσεις|
Ταξινόμηση για Linux | Cassification της ενημέρωσης.<br>Πιθανές τιμές είναι:<br>-Κρίσιμων ενημερώσεων<br>-Ενημερώσεις ασφαλείας<br>-Άλλες ενημερώσεις|
Υπολογιστή | Το όνομα του υπολογιστή.|
InstallTimeAvailable | Καθορίζει εάν είναι διαθέσιμη από άλλους παράγοντες που εγκατεστημένη την ίδια ενημέρωση το χρόνο εγκατάστασης.|
InstallTimePredictionSeconds | Εκτιμώμενη εγκατάστασης χρόνος σε δευτερόλεπτα που βασίζονται σε άλλους παράγοντες που εγκατεστημένη την ίδια ενημερωμένη έκδοση.|
KBID | Το Αναγνωριστικό του άρθρου της Γνωσιακής Βάσης που περιγράφει την ενημερωμένη έκδοση.|
ManagementGroupName | Όνομα της ομάδας διαχείρισης για SCOM παράγοντες.  Για άλλες παράγοντες, αυτή είναι AOI -<workspace ID>.|
MSRCBulletinID | Αναγνωριστικό από το ενημερωτικό δελτίο ασφαλείας που περιγράφει την ενημερωμένη έκδοση.|
MSRCSeverity | Της σοβαρότητας του ενημερωτικού δελτίου ασφαλείας της Microsoft.<br>Πιθανές τιμές είναι:<br>-Κρίσιμη<br>-Σημαντικό<br>-Εποπτεία|
Προαιρετικό | Καθορίζει εάν η ενημέρωση είναι προαιρετικό.|
Προϊόν | Είναι το όνομα του προϊόντος την ενημερωμένη έκδοση.  Κάντε κλικ στην επιλογή **Προβολή** για να ανοίξετε το άρθρο σε ένα πρόγραμμα περιήγησης.|
PackageSeverity | Το της σοβαρότητας του την ευπάθεια σταθερή σε αυτήν την ενημέρωση, όπως αναφέρονται από τους προμηθευτές distro Linux. | 
PublishDate | Ημερομηνία και ώρα που έχει εγκατασταθεί η ενημερωμένη έκδοση.|
RebootBehavior | Καθορίζει εάν η ενημέρωση επιβάλει επανεκκίνηση.<br>Πιθανές τιμές είναι:<br>-canrequestreboot<br>-neverreboots|
RevisionNumber | Ο αριθμός αναθεώρησης της ενημέρωσης.|
SourceComputerId | GUID για να προσδιορίσει μοναδικά στον υπολογιστή.|
TimeGenerated | Ημερομηνία και ώρα που έγινε η τελευταία ενημέρωση της εγγραφής.|
Τίτλος | Ο τίτλος της ενημέρωσης.|
Αναγνωριστικό ενημέρωσης | GUID για να προσδιορίσει μοναδικά την ενημερωμένη έκδοση.|
UpdateState | Καθορίζει εάν η ενημέρωση έχει εγκατασταθεί σε αυτόν τον υπολογιστή.<br>Πιθανές τιμές είναι:<br>-Η ενημέρωση εγκατεστημένο - είναι εγκατεστημένο σε αυτόν τον υπολογιστή.<br>-Απαιτείται - την ενημέρωση δεν έχει εγκατασταθεί και είναι απαραίτητη σε αυτόν τον υπολογιστή.|  

<br>
Όταν πραγματοποιείτε οποιαδήποτε αναζήτηση καταγραφής που επιστρέφει τις εγγραφές με έναν τύπο της **ενημερωμένης έκδοσης** , μπορείτε να επιλέξετε την προβολή **ενημερώσεων** που εμφανίζει ένα σύνολο των πλακιδίων συνοψίζει τις ενημερώσεις που επιστρέφονται από την αναζήτηση. Μπορείτε να κάνετε κλικ στις εγγραφές στη **λείπουν και έχουν εφαρμοστεί ενημερώσεις** και **ενημερώσεις απαραίτητων και προαιρετικών** πλακιδίων στη εμβέλεια της προβολής σε αυτό το σύνολο των ενημερώσεων. Επιλέξτε την προβολή **λίστας** ή τον **πίνακα** για να επιστρέψει τις μεμονωμένες εγγραφές.<br> 

![Προβολή ενημερωμένη έκδοση του αρχείου καταγραφής αναζήτησης με τύπο εγγραφής ενημέρωση](./media/oms-solution-update-management/update-la-view-updates.png)  

Στην προβολή του **πίνακα** , μπορείτε να επιλέξετε το **KBID** για οποιαδήποτε εγγραφή για να ανοίξετε ένα πρόγραμμα περιήγησης με το άρθρο της Γνωσιακής Βάσης. Αυτό σας επιτρέπει να διαβάσετε γρήγορα σχετικά με τις λεπτομέρειες της συγκεκριμένης ενημέρωσης.<br> 

![Προβολή πίνακα αναζήτησης καταγραφής με τα πλακίδια τύπο εγγραφής ενημερώσεις](./media/oms-solution-update-management/update-la-view-table.png)

Στην προβολή **λίστας** , κάντε κλικ στη σύνδεση **Προβολή** δίπλα του KBID για να ανοίξετε το άρθρο της Γνωσιακής Βάσης.<br>

![Αρχείο καταγραφής αναζήτησης προβολής λίστας με τα πλακίδια τύπο εγγραφής ενημερώσεις](./media/oms-solution-update-management/update-la-view-list.png)


###<a name="updatesummary-records"></a>UpdateSummary εγγραφές

Μια εγγραφή με έναν τύπο **UpdateSummary** δημιουργείται για κάθε υπολογιστή παράγοντας Windows. Αυτή η εγγραφή ενημερώνεται κάθε φορά που σαρωθεί ο υπολογιστής για ενημερώσεις. **UpdateSummary** εγγραφές έχουν τις ιδιότητες στον παρακάτω πίνακα.

Ιδιότητα | Περιγραφή|
----------|----------|
Τύπος | UpdateSummary|
SourceSystem | OpsManager |
Υπολογιστή | Το όνομα του υπολογιστή.|
CriticalUpdatesMissing | Αριθμός των κρίσιμων ενημερώσεων που λείπουν στον υπολογιστή.|
ManagementGroupName | Όνομα της ομάδας διαχείρισης για SCOM παράγοντες. Για άλλες παράγοντες, αυτή είναι AOI -<workspace ID>.|
NETRuntimeVersion | Έκδοση του χρόνου εκτέλεσης .NET εγκατεστημένη στον υπολογιστή.|
OldestMissingSecurityUpdateBucket | Δημοσιεύτηκε χρωματισμού να κατηγοριοποιήσετε την ώρα από την παλιότερη που λείπουν ενημερωμένη έκδοση ασφαλείας σε αυτόν τον υπολογιστή.<br>Πιθανές τιμές είναι:<br>-Παλαιότερων<br>-από 180 ημέρες<br>-από 150 ημέρες<br>-από 120 ημέρες<br>-πριν από 90 ημέρες<br>-από 60 ημέρες<br>-Μεταβείτε 30 ημέρες<br>-Πρόσφατα|
OldestMissingSecurityUpdateInDays | Δημοσιεύτηκε αριθμός ημερών από την παλιότερη που λείπουν ενημερωμένη έκδοση ασφαλείας σε αυτόν τον υπολογιστή.|
OsVersion | Η έκδοση του λειτουργικού συστήματος στον υπολογιστή.|
OtherUpdatesMissing | Αριθμός άλλων ενημερώσεων στον υπολογιστή.|
SecurityUpdatesMissing | Αριθμός ενημερώσεις ασφαλείας που λείπουν στον υπολογιστή.|
SourceComputerId | GUID για να προσδιορίσει μοναδικά στον υπολογιστή.|
TimeGenerated | Ημερομηνία και ώρα που έγινε η τελευταία ενημέρωση της εγγραφής.|
TotalUpdatesMissing |Συνολικός αριθμός των ενημερώσεων που λείπουν στον υπολογιστή.|
WindowsUpdateAgentVersion | Αριθμός έκδοσης του τον παράγοντα Windows Update στον υπολογιστή.|
WindowsUpdateSetting | Ρύθμιση για τον τρόπο ο υπολογιστής θα εγκαταστήσετε σημαντικές ενημερώσεις.<br>Πιθανές τιμές είναι:<br>-Απενεργοποίησης<br>-Ειδοποίηση πριν από την εγκατάσταση<br>-Προγραμματισμένη εγκατάσταση|
WSUSServer | Διεύθυνση URL του WSUS διακομιστή εάν ο υπολογιστής έχει ρυθμιστεί για να χρησιμοποιήσετε ένα.|  

## <a name="sample-log-searches"></a>Δείγμα αρχείου καταγραφής αναζητήσεις

Ο παρακάτω πίνακας παρέχει δείγμα αρχείου καταγραφής αναζητά ενημερώνετε εγγραφές που συλλέγονται από αυτήν τη λύση. 

Ερώτημα | Περιγραφή|
----------|----------|
Όλοι οι υπολογιστές με τις ενημερώσεις που λείπουν | Τύπος = UpdateState ενημέρωση = απαιτείται προαιρετικό = false & #124- Επιλέξτε τον υπολογιστή, τίτλο, KBID, ταξινόμηση, UpdateSeverity, PublishedDate|
Λείπουν ενημερώσεις για τον υπολογιστή "COMPUTER01.contoso.com" (με αντικατάσταση με το δικό σας όνομα υπολογιστή) | Τύπος = UpdateState ενημέρωση = απαιτείται προαιρετικό = false υπολογιστή = "COMPUTER01.contoso.com" & #124- Επιλέξτε τον υπολογιστή, τίτλο, KBID, προϊόν, UpdateSeverity, PublishedDate|
Όλους τους υπολογιστές με λείπουν κρίσιμες ή ενημερώσεις ασφαλείας | Τύπος = UpdateState ενημέρωση = απαιτείται προαιρετικό = false (ταξινόμηση = ταξινόμηση ή "Ενημερώσεις ασφαλείας" = "Κρίσιμες ενημερώσεις")|
Κρίσιμη ή την ασφάλεια ενημερώσεις που απαιτούνται από υπολογιστές όπου εφαρμόζονται με μη αυτόματο τρόπο ενημερώσεις | Τύπος = UpdateState ενημέρωση = απαιτείται προαιρετικό = false (ταξινόμηση = "Ενημερώσεις ασφαλείας" ή ταξινόμηση = "Κρίσιμες ενημερώσεις") σε υπολογιστή {τύπος = UpdateSummary WindowsUpdateSetting = εγχειρίδιο & #124- Διακριτό υπολογιστή} & #124- Διακριτό KBID|
Συμβάντα σφάλματος για υπολογιστές που έχουν λείπουν κρίσιμες ή την ασφάλεια, απαιτούνται ενημερώσεις | Τύπος = EventLevelName συμβάν = σφάλματος σε υπολογιστή {τύπος = ενημέρωση (ταξινόμηση = "Ενημερώσεις ασφαλείας" ή ταξινόμηση = "Κρίσιμες ενημερώσεις") UpdateState = απαιτείται προαιρετικό = false & #124- Διακριτό υπολογιστή}|
Όλοι οι υπολογιστές με συναθροίσεις που λείπουν | Τύπος = προαιρετικό ενημέρωση = false ταξινόμηση = "Συναθροίσεις" UpdateState = απαιτείται & #124- Επιλέξτε τον υπολογιστή, τίτλο, KBID, ταξινόμηση, UpdateSeverity, PublishedDate|
Διακριτό που λείπουν ενημερώσεις σε όλους τους υπολογιστές | Τύπος = UpdateState ενημέρωση = απαιτείται προαιρετικό = false & #124- Διακριτό τίτλου|
Ιδιότητα μέλους WSUS υπολογιστή | Τύπος = UpdateSummary & #124- μέτρηση count() με WSUSServer|
Αυτόματη ενημέρωση ρύθμισης παραμέτρων | Τύπος = UpdateSummary & #124- μέτρηση count() με WindowsUpdateSetting|
Υπολογιστές με αυτόματη ενημέρωση απενεργοποιημένη | Τύπος = UpdateSummary WindowsUpdateSetting = ο μη αυτόματος|  
Λίστα με όλους τους υπολογιστές Linux που έχουν διαθέσιμη μια ενημερωμένη έκδοση πακέτου | Τύπος = ενημέρωσης και OSType = Linux και UpdateState! = "Δεν απαιτούνται" & #124- μέτρηση count() από τον υπολογιστή|
Λίστα με όλους τους υπολογιστές Linux που έχουν διαθέσιμη μια ενημερωμένη έκδοση πακέτου που αντιμετωπίζει ευπάθεια κρίσιμη ή την ασφάλεια | Τύπος = ενημέρωσης και OSType = Linux και UpdateState! = "Δεν απαιτούνται" και (ταξινόμηση = ή ταξινόμηση "Κρίσιμες ενημερώσεις" = "Ενημερώσεις ασφαλείας") & #124- μέτρηση count() από τον υπολογιστή|
Λίστα με όλα τα πακέτα που έχουν διαθέσιμη ενημερωμένη έκδοση | Τύπος = ενημέρωσης και OSType = Linux και UpdateState! = "Δεν απαιτούνται"|
Λίστα με όλα τα πακέτα που έχουν διαθέσιμη ενημερωμένη έκδοση που αντιμετωπίζει ευπάθεια κρίσιμη ή την ασφάλεια | Τύπος = ενημέρωσης και OSType = Linux και UpdateState! = "Δεν απαιτούνται" και (ταξινόμηση = ή ταξινόμηση "Κρίσιμες ενημερώσεις" = "Ενημερώσεις ασφαλείας")|
Λίστα με όλους τους υπολογιστές "Ubuntu" με οποιαδήποτε διαθέσιμη ενημέρωση | Τύπος = ενημέρωσης και OSType = Linux και OSName = Ubuntu & #124- μέτρηση count() από τον υπολογιστή|

## <a name="next-steps"></a>Επόμενα βήματα

- Χρησιμοποιήστε αναζητήσεις καταγραφής στο [Αρχείο καταγραφής ανάλυσης](../log-analytics/log-analytics-log-searches.md) για να προβάλετε λεπτομερείς ενημέρωσης δεδομένων.

- [Δημιουργήστε το δικό σας πίνακες εργαλείων](../log-analytics/log-analytics-dashboards.md) που εμφανίζει συμβατότητα ενημέρωσης για διαχειριζόμενη υπολογιστές σας.

- [Δημιουργία ειδοποιήσεων](../log-analytics/log-analytics-alerts.md) όταν εντοπίζονται κρίσιμες ενημερώσεις ότι λείπουν από υπολογιστές ή σε έναν υπολογιστή που έχει απενεργοποιήσει τις Αυτόματες ενημερώσεις.  



