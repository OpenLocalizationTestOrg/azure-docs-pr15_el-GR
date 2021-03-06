<properties 
    pageTitle="Εφαρμογή χάρτη σε εφαρμογή ιδέες | Microsoft Azure" 
    description="Μια οπτική παρουσίαση από τις εξαρτήσεις μεταξύ των στοιχείων της εφαρμογής, με την ετικέτα με KPI και ειδοποιήσεις." 
    services="application-insights" 
    documentationCenter=""
    authors="SoubhagyaDash" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2016" 
    ms.author="awills"/>
 
# <a name="application-map-in-application-insights"></a>Εφαρμογή του χάρτη στο ιδέες εφαρμογής

Στο [Visual Studio εφαρμογή ιδέες](app-insights-overview.md), αντιστοίχιση εφαρμογής είναι μια οπτική διάταξη των σχέσεων εξάρτησης από την εφαρμογή στοιχεία. Κάθε στοιχείο εμφανίζει KPI όπως φόρτωση, επιδόσεων, αποτυχίες και ειδοποιήσεις, για να σας βοηθήσουν να ανακαλύψετε οποιοδήποτε στοιχείο προκαλεί ένα ζήτημα επιδόσεων ή την αποτυχία. Μπορείτε να κάνετε κλικ μέσα από οποιοδήποτε στοιχείο πιο λεπτομερείς Διαγνωστικά, και τα δύο από εφαρμογή ιδέες, και - εάν η εφαρμογή σας χρησιμοποιεί το Azure υπηρεσίες - Διαγνωστικά του Azure, όπως τις συστάσεις Σύμβουλος βάσης δεδομένων SQL.

Όπως άλλα γραφήματα, μπορείτε να καρφιτσώσετε μια αντιστοίχιση εφαρμογής στον πίνακα εργαλείων Azure, όπου είναι πλήρως λειτουργικό. 

## <a name="open-the-application-map"></a>Ανοίξτε την εφαρμογή του χάρτη

Ανοίξτε το χάρτη από την επισκόπηση blade για την εφαρμογή σας:

![Ανοίξτε το app χάρτη](./media/app-insights-app-map/01.png)

![εφαρμογή χάρτη](./media/app-insights-app-map/02.png)

Εμφανίζει το χάρτη:

* Διαθεσιμότητα δοκιμές
* Στοιχείο πλευρά του προγράμματος-πελάτη (παρακολουθούνται με το SDK JavaScript)
* Στοιχείο πλευρά του διακομιστή
* Εξαρτήσεις από τα στοιχεία προγράμματος-πελάτη και διακομιστή

Μπορείτε να αναπτύξετε και να συμπτύξετε τις ομάδες εξάρτηση σύνδεσης:

![Σύμπτυξη](./media/app-insights-app-map/03.png)
 
Εάν έχετε πολλές εξαρτήσεις από έναν τύπο (SQL, HTTP κ.λπ.), ενδέχεται να εμφανίζονται ομαδοποιημένα. 


![ομαδοποιημένη εξαρτήσεων](./media/app-insights-app-map/03-2.png)
 
 
## <a name="spot-problems"></a>Σημείο προβλήματα

Κάθε κόμβος έχει ενδείξεις σχετικές επιδόσεων, όπως τη φόρτωση, την απόδοση και αποτυχία χρεώσεις για αυτό το στοιχείο. 

Προειδοποίηση εικονίδια επισημάνετε πιθανά προβλήματα. Μια προειδοποίηση πορτοκαλί σημαίνει ότι υπάρχουν αποτυχίες αιτήσεις, προβολές σελίδας ή εξάρτηση κλήσεις. Κόκκινο σημαίνει χρέωσης αποτυχία πάνω από 5%.


![Αποτυχία εικονίδια](./media/app-insights-app-map/04.png)

 
Ενεργό ειδοποιεί επίσης εμφάνιση του: 


![ενεργό ειδοποιήσεων](./media/app-insights-app-map/05.png)
 
Εάν χρησιμοποιείτε το SQL Azure, υπάρχει ένα εικονίδιο που εμφανίζει πότε υπάρχουν προτάσεις σχετικά με πώς μπορείτε να βελτιώσετε τις επιδόσεις. 


![Σύσταση Azure](./media/app-insights-app-map/06.png)

Κάντε κλικ σε οποιοδήποτε εικονίδιο για να δείτε περισσότερες λεπτομέρειες:


![σύσταση Azure](./media/app-insights-app-map/07.png)
 
 
## <a name="diagnostic-click-through"></a>Διαγνωστικών κάντε κλικ στην επιλογή μέσω

Κάθε έναν από τους κόμβους στο χάρτη προσφέρει προορισμού κάντε κλικ στην επιλογή μέσω για τα Διαγνωστικά. Οι επιλογές διαφέρουν ανάλογα με τον τύπο του κόμβου.

![Επιλογές διακομιστή](./media/app-insights-app-map/09.png)

 
Για τα στοιχεία που φιλοξενούνται στο Azure, οι επιλογές περιλαμβάνουν απευθείας συνδέσεις σε αυτά.


## <a name="filters-and-time-range"></a>Φίλτρα και περιοχής ώρας

Από προεπιλογή, το χάρτη συνοψίζει όλα τα δεδομένα που είναι διαθέσιμες για το επιλεγμένο χρονικό διάστημα. Αλλά μπορείτε να φιλτράρετε, να συμπεριλάβετε μόνο συγκεκριμένα λειτουργία ονόματα ή τις εξαρτήσεις.

* Όνομα λειτουργίας: περιλαμβάνει προβολές σελίδων και τύπων αίτηση πλευρά του διακομιστή. Με αυτήν την επιλογή, ο χάρτης εμφανίζει το KPI στον κόμβο πλευρά του διακομιστή/προγράμματος-πελάτη για την επιλεγμένη λειτουργίες μόνο. Εμφανίζει τις εξαρτήσεις που ονομάζεται στο περιβάλλον της αυτές τις συγκεκριμένες εργασίες.
* Όνομα βάσης εξάρτηση: περιλαμβάνει τις εξαρτήσεις πλευρά του προγράμματος περιήγησης AJAX και εξαρτήσεις πλευρά του διακομιστή. Αν αναφέρετε τηλεμετρίας προσαρμοσμένη εξάρτηση με το API TrackDependency, αυτά θα εμφανίζονται επίσης εδώ. Μπορείτε να επιλέξετε τις εξαρτήσεις για να εμφανίσετε στο χάρτη. Έχετε υπόψη ότι αυτήν τη στιγμή, αυτό θα δεν φιλτράρει τις αιτήσεις πλευρά του διακομιστή ή τις προβολές σελίδας πλευρά του προγράμματος-πελάτη.


![Ορισμός φίλτρων](./media/app-insights-app-map/11.png)

 
 
## <a name="save-filters"></a>Αποθήκευση φίλτρων

Για να αποθηκεύσετε τα φίλτρα που έχετε εφαρμόσει, Καρφίτσωμα στην φιλτραρισμένη προβολή σε έναν [πίνακα εργαλείων](app-insights-dashboards.md).


![Καρφίτσωμα στον πίνακα εργαλείων](./media/app-insights-app-map/12.png)
 


## <a name="feedback"></a>Σχόλια

Επικοινωνήστε [παροχή σχολίων μέσω της επιλογής πύλης σχολίων](app-insights-get-dev-support.md).


![Εικόνα MapLink-1](./media/app-insights-app-map/13.png)


