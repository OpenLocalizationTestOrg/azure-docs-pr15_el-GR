<properties
    pageTitle="Επισκόπηση των μετρήσεων στο Microsoft Azure | Microsoft Azure"
    description="Επισκόπηση των μετρήσεων και τις χρήσεις στο Microsoft Azure"
    authors="kamathashwin"
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
    ms.date="09/26/2016"
    ms.author="ashwink"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Επισκόπηση των μετρήσεων στο Microsoft Azure 

Σε αυτό το άρθρο περιγράφει τι είναι οι μετρήσεις στο Microsoft Azure, τους πλεονεκτήματα και πώς μπορείτε να επιτύχετε γρήγορα αποτελέσματα με τη χρήση τους.  

## <a name="what-are-metrics"></a>Τι είναι οι μετρήσεις;

Azure οθόνη σάς επιτρέπει να εκμετάλλευση τηλεμετρίας για να αποκτήσετε ορατότητα στην τις επιδόσεις και την εύρυθμη λειτουργία της το φόρτο εργασίας στους Azure. Οι πιο σημαντικές τύποι Azure τηλεμετρίας δεδομένων είναι τα μετρικά (ονομάζεται επίσης μετρητές επιδόσεων) που εκπέμπει πιο Azure πόρους. Οθόνη Azure παρέχει διάφορους τρόπους για να ρυθμίσετε τις παραμέτρους και κατανάλωση αυτές τις μετρήσεις για την παρακολούθηση και αντιμετώπιση προβλημάτων.


## <a name="what-can-you-do-with-metrics"></a>Τι μπορείτε να κάνετε με μετρικά;

Μετρικά είναι πολύτιμη πηγή τηλεμετρίας και σας επιτρέπει να κάνετε τα εξής:

- **Παρακολούθηση των επιδόσεων** του πόρου (όπως μια Εικονική, τοποθεσία Web ή εφαρμογή λογικής) κατά τη σχεδίαση τις μετρήσεις σε ένα γράφημα πύλης και Καρφίτσωμα αυτό το γράφημα σε έναν πίνακα εργαλείων.
- **Ενημερωθείτε για ένα ζήτημα** επηρεάζουν τις επιδόσεις του πόρου, όταν ένα μετρικό διασταυρώνεται με ένα συγκεκριμένο όριο.
- **Ρύθμιση παραμέτρων αυτοματοποιημένη ενέργειες**, όπως autoscaling έναν πόρο ή ενεργοποίηση ενός runbook όταν ένα μετρικό διασταυρώνεται με ένα συγκεκριμένο όριο.
- **Εκτέλεση ανάλυσης για προχωρημένους** ή αναφοράς σε τάσεις απόδοσης ή χρήση των πόρων σας.
- **Αρχειοθέτηση** το ιστορικό απόδοση ή την εύρυθμη λειτουργία των πόρων **για τον έλεγχο** συμμόρφωσης/τους σκοπούς σας.

##  <a name="metric-characteristics"></a>Μετρικό χαρακτηριστικά
Μετρικά έχουν τα εξής χαρακτηριστικά:

- Όλα τα μετρικά έχουν **συχνότητα 1 λεπτών**. Λαμβάνετε μια μετρική τιμή κάθε λεπτό από τον πόρο, παρέχοντας κοντά σε πραγματικό χρόνο ορατότητα στην κατάσταση και την εύρυθμη λειτουργία του πόρου.
- Μετρικά είναι **διαθέσιμη εκτός του-οι έτοιμες χωρίς να χρειάζεται να επιλέξετε** ή για τη ρύθμιση του επιπλέον Διαγνωστικά.
- Μπορείτε να αποκτήσετε πρόσβαση **30 ημερών του ιστορικού** για κάθε μετρικό σύστημα. Μπορείτε γρήγορα να δείτε τα πρόσφατα και μηνιαία τάσεις σε την απόδοση ή την εύρυθμη λειτουργία του πόρου.

Μπορείς:

- Ανακαλύψτε το εύκολα, πρόσβασης και την **Προβολή όλα τα μετρικά** μέσω της πύλης Azure όταν επιλέγετε έναν πόρο και σχεδιάστε τους σε ένα γράφημα. 
- Ρύθμιση παραμέτρων μετρικό **ειδοποίησης κανόνα που στέλνει μια ειδοποίηση ή αυτοματοποιημένη αναλαμβάνει** όταν τη μέτρηση διασταυρώνεται με το όριο που έχετε ορίσει. Autoscale είναι μια ειδική αυτοματοποιημένη ενέργεια που σας επιτρέπει να κλιμακωθεί τον πόρο για να πληρούν τα εισερχόμενα αιτήματα ή φόρτωση στην τοποθεσία web ή τον υπολογισμό πόρους. Μπορείτε να ρυθμίσετε έναν κανόνα Autoscale τη ρύθμιση για να κλιμακωθεί ανάληψης/στο που βασίζεται σε ένα μετρικό Διασταύρωση μια οριακή τιμή.
- **Αρχειοθέτηση** μετρικά για μεγαλύτερο χρονικό διάστημα ή να τις χρησιμοποιήσετε για την αναφορά χωρίς σύνδεση. Μπορείτε να δρομολογήσετε την μετρήσεων για αντικειμένων blob χώρου αποθήκευσης όταν ρυθμίζετε τις παραμέτρους διαγνωστικών ρυθμίσεων για τον πόρο.
- **Ροή** μετρήσεις σε ένα συμβάν διανομέα, επιτρέποντάς σας για να δρομολογήσετε την τους ανάλυση ροή Azure ή προσαρμοσμένες εφαρμογές για την ανάλυση κοντά σε πραγματικό χρόνο. Μπορείτε να κάνετε τη χρήση διαγνωστικών ρυθμίσεων.
- **Δρομολόγηση** όλων των μετρήσεων για ανάλυση καταγραφής OMS για να ξεκλειδώσετε άμεση ανάλυση, αναζήτηση και προσαρμοσμένες ειδοποίησης για μετρικά δεδομένα από τους πόρους σας.
- **Ανάλωση** τα μετρικά μέσω νέα Azure οθόνη REST API.
- Χρησιμοποιείτε το cmdlet του PowerShell ή πλατφόρμες REST API μετρικά **ερωτήματος** .

 ![Δρομολόγηση μετρικών στην Εποπτεία Azure](./media/monitoring-overview-metrics/MetricsOverview0.png)

## <a name="access-metrics-via-portal"></a>Μετρήσεις πρόσβασης μέσω πύλης
Ακολουθεί μια γρήγορη αναλυτικές οδηγίες για τη δημιουργία γραφήματος μετρικό μέσω Azure πύλη

### <a name="view-metrics-after-creating-a-resource"></a>Προβολή μετρικά μετά τη δημιουργία ενός πόρου
1. Άνοιγμα Azure πύλη
2. Δημιουργία νέας εφαρμογής υπηρεσίας - τοποθεσία Web.
3. Αφού δημιουργήσετε μια τοποθεσία Web, μεταβείτε στο το blade Επισκόπηση της τοποθεσίας web.
4. Μπορείτε να προβάλετε νέων μετρικών ως πλακίδιο 'Παρακολούθηση'. Μπορείτε να επεξεργαστείτε την παράθεση και να επιλέξετε περισσότερες μετρήσεις

 ![Μετρήσεις σε έναν πόρο στην Εποπτεία Azure](./media/monitoring-overview-metrics/MetricsOverview1.png)    

### <a name="access-all-metrics-in-a-single-place"></a>Πρόσβαση σε όλες τις μετρήσεις σε ένα μόνο σημείο
1. Ανοίξτε την πύλη του Azure 
2. Μεταβείτε στη νέα καρτέλα οθόνη και ενεργοποιήστε την επιλογή μετρικά από κάτω 
3. Μπορείτε να επιλέξετε τη συνδρομή σας, ομάδα πόρων και το όνομα του πόρου από την αναπτυσσόμενη λίστα. 
4. Τώρα, μπορείτε να προβάλετε τη λίστα διαθέσιμες μετρήσεις. 
5. Επιλέξτε το μετρικό που σας ενδιαφέρει και σχεδιάστε τον. 
6. Μπορείτε να καρφιτσώσετε το στον πίνακα εργαλείων, κάνοντας κλικ στην το Καρφίτσωμα στην επάνω δεξιά γωνία.

 ![Πρόσβαση σε όλες τις μετρήσεις σε ένα μόνο σημείο στην Εποπτεία Azure](./media/monitoring-overview-metrics/MetricsOverview2.png) 


>[AZURE.NOTE] Μπορείτε να αποκτήσετε πρόσβαση σε μετρικά host επιπέδου από ΣΠΣ (Azure από διαχειριστή πόρων βάσει) και τα σύνολα κλίμακα Εικονική χωρίς τυχόν πρόσθετες διαγνωστικών εγκατάστασης. Αυτών των νέων μετρικών host επιπέδου είναι διαθέσιμες για τα Windows και Linux παρουσίες. Αυτές οι μετρήσεις είναι δεν πρέπει να συγχέονται με το επίπεδο μετρικά επισκέπτη OS που έχετε πρόσβαση σε όταν ενεργοποιείτε Διαγνωστικά Azure σε ΣΠΣ ή VMSS. Για να μάθετε περισσότερα σχετικά με τη ρύθμιση των παραμέτρων Διαγνωστικά Azure, ανατρέξτε στο θέμα [Τι είναι τα Διαγνωστικά του Microsoft Azure](../azure-diagnostics.md).

## <a name="access-metrics-via-rest-api"></a>Μετρήσεις πρόσβασης μέσω REST API
Azure μετρικά είναι δυνατή η πρόσβαση μέσω του Azure APIs οθόνη. Υπάρχουν δύο API που σας βοηθήσουν να ανακαλύψετε και να αποκτάτε πρόσβαση μετρήσεις. Χρήση του: 

- [Azure οθόνη μετρικό ορισμών REST API](https://msdn.microsoft.com/library/mt743621.aspx) για να εμφανιστεί η λίστα των μετρήσεων που είναι διαθέσιμες για μια υπηρεσία.
- Για να αποκτήσετε πρόσβαση στα δεδομένα πραγματικού μετρικά [Azure οθόνη μετρικά REST API](https://msdn.microsoft.com/library/mt743622.aspx)

>[AZURE.NOTE] Σε αυτό το άρθρο καλύπτει τα μετρικά μέσω του [νέου API για μετρικά](https://msdn.microsoft.com/library/dn931930.aspx) για Azure πόρους. Η έκδοση API για τους νέους ορισμούς μετρικό API είναι 2016 03--01 και την έκδοση για μετρικά API είναι 2016-09-01. Οι παλαιού τύπου μετρικό ορισμοί και μετρήσεις είναι δυνατή η πρόσβαση με την έκδοση api 2014 04--01.

Για μια πιο λεπτομερή περιγραφή χρησιμοποιώντας τα Azure οθόνη REST API, ανατρέξτε στο θέμα [Azure οθόνη REST API οδηγίες](monitoring-rest-api-walkthrough.md).

## <a name="export-options-for-metrics"></a>Επιλογές για μετρικά εξαγωγής
Μπορείτε να μεταβείτε σε το blade αρχεία καταγραφής διαγνωστικών κάτω από την καρτέλα οθόνη και να προβάλετε τις επιλογές εξαγωγής για μετρήσεις. Μπορείτε να επιλέξετε μετρήσεις (και αρχεία καταγραφής διαγνωστικών) για να δρομολογηθούν σε χώρο αποθήκευσης αντικειμένων Blob, διανομείς συμβάν ή OMS καταγραφής ανάλυσης για περιπτώσεις χρήσης που αναφέρονται προηγουμένως σε αυτό το άρθρο. 

 ![Επιλογές εξαγωγής για μετρήσεις σε οθόνη Azure](./media/monitoring-overview-metrics/MetricsOverview3.png)   

Μπορείτε να το ρυθμίσετε μέσω πρότυπα διαχείρισης πόρων, [PowerShell](insights-powershell-samples.md), [CLI](insights-cli-samples.md) ή [REST API](https://msdn.microsoft.com/library/dn931943.aspx). 

## <a name="take-action-on-metrics"></a>Εκτέλεση ενεργειών σε μετρήσεις
Μπορείτε να λαμβάνετε ειδοποιήσεις ή να εκτελέσετε αυτοματοποιημένη ενέργειες σε μετρικό δεδομένα. Μπορείτε να ρυθμίσετε ειδοποίησης κανόνες ή Autoscale ρυθμίσεις για να το κάνετε.

### <a name="alert-rules"></a>Κανόνες ειδοποίησης
Μπορείτε να ρυθμίσετε τις παραμέτρους ειδοποίησης κανόνες μετρικά. Αυτούς τους κανόνες ειδοποίησης να ελέγξετε εάν ένα μετρικό έχει τέμνονται ένα συγκεκριμένο όριο και να σας ειδοποιεί μέσω ηλεκτρονικού ταχυδρομείου ή fire ένα webhook που μπορούν να χρησιμοποιηθούν, στη συνέχεια, να εκτελέσει οποιαδήποτε προσαρμοσμένων δεσμών ενεργειών. Μπορείτε επίσης να χρησιμοποιήσετε το webhook για τη ρύθμιση παραμέτρων 3η ενοποιήσεις προϊόντος.

 ![Μετρήσεις και ειδοποίησης κανόνες στην Εποπτεία Azure](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale"></a>Autoscale
Ορισμένοι πόροι Azure υποστηρίζει πολλές παρουσίες για να κλιμακωθεί ανάληψη ή σε χειρισμού σας φόρτους εργασίας. Autoscale ισχύει για τις υπηρεσίες εφαρμογών (εφαρμογές Web), σύνολα κλίμακα εικονική μηχανή (VMSS) και κλασική υπηρεσίες Cloud. Μπορείτε να ρυθμίσετε autoscale κανόνες για να κλιμακωθεί ανάληψη ή σε όταν ένα συγκεκριμένο μετρικό επηρεάζουν το φόρτο εργασίας τέμνει μια οριακή τιμή που καθορίζετε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση autoscaling](monitoring-overview-autoscale.md).

 ![Μετρήσεις και autoscale στην Εποπτεία Azure](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="supported-services-and-metrics"></a>Υποστηριζόμενες υπηρεσίες και μετρήσεις
Azure οθόνη είναι μια νέα υποδομή μετρήσεις. Παρέχει υποστήριξη για τις ακόλουθες υπηρεσίες Azure στην πύλη του Azure και τη νέα έκδοση του Azure API οθόνη:

- ΣΠΣ (Azure από διαχειριστή πόρων βάσει)
- Εικονική κλίμακα σύνολα
- Μαζική
- Χώρος ονομάτων διανομέα συμβάντος 
- Χώρος ονομάτων υπηρεσίας Bus (Premium για SKU μόνο)
- SQL (έκδοση 12)
- Χώρος συγκέντρωσης ελαστικότητας SQL
- Τοποθεσίες Web
- Συμπλεγμάτων διακομιστή Web
- Λογική εφαρμογών
- Διανομείς IoT
- Redis Cache
- Δικτύωση: Εφαρμογή πυλών
- Αναζήτηση

Μπορείτε να προβάλετε μια μια λεπτομερής λίστα με όλες τις υποστηριζόμενες υπηρεσίες και τις μετρήσεις στο [Azure οθόνη μετρικά - υποστηριζόμενες μετρικά ανά τύπο πόρου](monitoring-supported-metrics.md). 


## <a name="next-steps"></a>Επόμενα βήματα

Ανατρέξτε στις συνδέσεις σε αυτό το άρθρο. Επιπλέον, μάθετε:  

- σχετικά με τις [κοινές μετρήσεις για autoscaling](insights-autoscale-common-metrics.md)
- πώς μπορείτε να [δημιουργήσετε κανόνες ειδοποίησης](insights-alerts-portal.md)



