<properties
    pageTitle="Δημιουργία εφαρμογής web από το Azure Marketplace | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια νέα εφαρμογή web WordPress από το Azure Marketplace, χρησιμοποιώντας την πύλη Azure."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>Δημιουργία εφαρμογής web από το Azure Marketplace

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Το Azure Marketplace κάνει διαθέσιμη μια ευρεία ποικιλία εφαρμογές web δημοφιλείς που αναπτύχθηκε από τη Microsoft, εταιρείες τρίτων και πρωτοβουλίες λογισμικό Άνοιγμα αρχείου προέλευσης. Για παράδειγμα, WordPress, Umbraco CMS, Drupal, κ.λπ. Αυτές οι εφαρμογές web δημιουργούνται σε ένα ευρύ φάσμα δημοφιλών πλαίσια, όπως [PHP] στο αυτό WordPress παράδειγμα, [.NET], [Node.js], [Java]και [Python], μερικά. Για να δημιουργήσετε μια εφαρμογή web από το Azure Marketplace το λογισμικό μόνο που χρειάζεστε είναι το πρόγραμμα περιήγησης που χρησιμοποιείτε για την [Πύλη Azure].

Σε αυτό το πρόγραμμα εκμάθησης θα μάθετε πώς μπορείτε να:

* Εύρεση και τη δημιουργία εφαρμογών web στο Azure εφαρμογής υπηρεσίας που βασίζεται σε ένα πρότυπο του Azure Marketplace.
* Ρύθμιση παραμέτρων Azure εφαρμογής υπηρεσίας για τη νέα εφαρμογή web.
* Εκκίνηση και να διαχειριστείτε την εφαρμογή web.

Για αυτό το πρόγραμμα εκμάθησης, θα αναπτύξετε μια τοποθεσία ιστολογίου WordPress από το Azure Marketplace. Όταν ολοκληρώσετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα έχετε τη δική σας τοποθεσία WordPress προς τα επάνω και την εκτέλεση στο cloud.

![Παράδειγμα WordPress wep εφαρμογή πίνακα εργαλείων][WordPressDashboard1]

Στην τοποθεσία WordPress που θα αναπτύξετε σε αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί MySQL για τη βάση δεδομένων. Εάν θέλετε να χρησιμοποιήσετε αντί για αυτό βάση δεδομένων SQL για τη βάση δεδομένων, ανατρέξτε στο θέμα [Nami έργου], που είναι επίσης διαθέσιμη μέσω του Azure Marketplace.

> [AZURE.NOTE]
> Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Microsoft Azure. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να [ενεργοποιήσετε των πλεονεκτημάτων της συνδρομής σας Visual Studio] [ activate] ή να [εγγραφείτε για μια δωρεάν δοκιμαστική έκδοση][free trial].
>
> Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν εγγραφείτε για ένα λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας]. Από εκεί μπορείτε να αμέσως να δημιουργήσετε μια εφαρμογή web μικρής διάρκειας starter στην εφαρμογή υπηρεσίας — χωρίς πιστωτική κάρτα απαιτείται και υπάρχουν χωρίς δεσμεύσεις.

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Βρείτε και να δημιουργήσετε μια εφαρμογή Web στο Azure εφαρμογής υπηρεσίας

1. Συνδεθείτε [πύλη του Azure].

1. Κάντε κλικ στην επιλογή **νέα**.
    
    ![Δημιουργήστε ένα νέο πόρο Azure][MarketplaceStart]
    
1. Αναζήτηση **WordPress**και, στη συνέχεια, κάντε κλικ στην επιλογή **WordPress**. (Εάν θέλετε να χρησιμοποιήσετε βάση δεδομένων SQL αντί MySQL, πραγματοποιήστε αναζήτηση για **Το έργο Nami**.)

    ![Αναζήτηση για WordPress στην αγορά][MarketplaceSearch]
    
1. Μετά την ανάγνωση την περιγραφή της εφαρμογής WordPress, κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία εφαρμογής web WordPress][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Ρύθμιση παραμέτρων Azure εφαρμογής υπηρεσίας για τη νέα εφαρμογή Web

1. Αφού δημιουργήσετε μια νέα εφαρμογή web, το blade ρυθμίσεις WordPress θα εμφανίζεται, που θα χρησιμοποιήσετε για να ολοκληρώσετε τα παρακάτω βήματα:

    ![Ρύθμιση παραμέτρων WordPress web app][ConfigStart]

1. Πληκτρολογήστε ένα όνομα για την εφαρμογή web στο πλαίσιο **εφαρμογή Web** .

    Αυτό το όνομα πρέπει να είναι μοναδικό στον τομέα azurewebsites.net, επειδή η διεύθυνση URL της εφαρμογής web θα *{name}*. azurewebsites.net. Εάν το όνομα που εισάγετε δεν είναι μοναδικές, εμφανίζεται ένα κόκκινο θαυμαστικό στο πλαίσιο κειμένου.

    ![Ρύθμιση παραμέτρων το όνομα εφαρμογής web WordPress][ConfigAppName]

1. Εάν έχετε περισσότερες από μία συνδρομές, επιλέξτε αυτήν που θέλετε να χρησιμοποιήσετε. 

    ![Ρύθμιση παραμέτρων τη συνδρομή για την εφαρμογή web][ConfigSubscription]

1. Επιλέξτε μια **Ομάδα πόρων** ή δημιουργήστε ένα νέο.

    Για περισσότερες πληροφορίες σχετικά με τις ομάδες πόρων, ανατρέξτε στο θέμα [Επισκόπηση διαχείρισης πόρων Azure][ResourceGroups].

    ![Ρύθμιση παραμέτρων την ομάδα των πόρων για την εφαρμογή web][ConfigResourceGroup]

1. Επιλέξτε ένα **Πρόγραμμα εφαρμογής υπηρεσίας/θέση** ή δημιουργήστε ένα νέο.

    Για περισσότερες πληροφορίες σχετικά με τα σχέδια εφαρμογής υπηρεσίας, ανατρέξτε στο θέμα [Επισκόπηση προγράμματος Azure εφαρμογής υπηρεσίας][AzureAppServicePlans]. 

    ![Ρύθμιση παραμέτρων το σχέδιο παροχής υπηρεσιών για την εφαρμογή web][ConfigServicePlan]

1. Κάντε κλικ στην επιλογή **βάση δεδομένων**και, στη συνέχεια, στο blade τη **Νέα βάση δεδομένων MySQL** , δώστε τις απαιτούμενες τιμές για τη ρύθμιση των παραμέτρων σας βάση δεδομένων MySQL.

    μια. Πληκτρολογήστε ένα νέο όνομα ή αφήστε το προεπιλεγμένο όνομα.

    β. Αποχώρηση από τον **Τύπο της βάσης δεδομένων** έχει οριστεί σε **κοινή χρήση**.

    c. Επιλέξτε στην ίδια θέση με αυτό που επιλέξατε για την εφαρμογή web.

    d. Επιλέξτε ένα επίπεδο τις πληροφορίες τιμολόγησης. Για αυτό το πρόγραμμα εκμάθησης **υδραργύρου** - που είναι δωρεάν με ελάχιστους συνδέσεις και χώρος στο δίσκο - είναι σωστή.

    ε. Στο blade τη **Νέα βάση δεδομένων MySQL** , αποδεχτείτε τους όρους της νομικές και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**. 

    ![Ρυθμίστε τις παραμέτρους της βάσης δεδομένων για την εφαρμογή web][ConfigDatabase]

1. Στο το blade **WordPress** , αποδεχτείτε τους όρους της νομικές και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**. 

    ![Ολοκληρώστε τις ρυθμίσεις εφαρμογής web και κάντε κλικ στο κουμπί OK][ConfigFinished]

    Azure εφαρμογής υπηρεσίας δημιουργεί την εφαρμογή web, συνήθως σε λιγότερο από ένα λεπτό. Μπορείτε να παρακολουθήσετε την πρόοδο κάνοντας κλικ στο εικονίδιο καμπάνας στο επάνω μέρος της σελίδας της πύλης.

    ![Ένδειξη προόδου][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Εκκίνηση και να διαχειριστείτε την εφαρμογή web της WordPress
    
1. Όταν ολοκληρώσετε τη δημιουργία της εφαρμογής web, μεταβείτε στην πύλη του Azure στην ομάδα πόρων στο οποίο έχετε δημιουργήσει την εφαρμογή και μπορείτε να δείτε την εφαρμογή web και τη βάση δεδομένων.

    Ο πόρος επιπλέον με το εικονίδιο λάμπας είναι [Ιδέες εφαρμογής][ApplicationInsights], που παρέχει υπηρεσίες παρακολούθησης για την εφαρμογή web.

1. Στο blade την **ομάδα πόρων** , κάντε κλικ στη γραμμή της εφαρμογής web.

    ![Επιλέξτε την εφαρμογή web της WordPress][WordPressSelect]

1. Στο blade την εφαρμογή Web, κάντε κλικ στο κουμπί **Αναζήτηση**.

    ![Αναζητήστε την εφαρμογή web της WordPress][WordPressBrowse]

1. Εάν σας ζητηθεί να επιλέξετε τη γλώσσα για το ιστολόγιο WordPress, επιλέξτε τη γλώσσα που θέλετε και, στη συνέχεια, κάντε κλικ στην επιλογή **Continue**.

    ![Ρύθμιση παραμέτρων τη γλώσσα για την εφαρμογή web της WordPress][WordPressLanguage]

1. Στη σελίδα WordPress **Καλώς ορίσατε** , εισαγάγετε τις πληροφορίες ρύθμισης παραμέτρων που απαιτούνται από WordPress και, στη συνέχεια, κάντε κλικ στην επιλογή **Εγκατάσταση WordPress**.

    ![Ρυθμίστε τις παραμέτρους της εφαρμογής web WordPress σας][WordPressConfigure]

1. Συνδεθείτε χρησιμοποιώντας τα διαπιστευτήρια που δημιουργήσατε στη σελίδα **υποδοχής** .  

1. Σελίδα πίνακα εργαλείων της τοποθεσίας σας θα ανοίξει και να εμφανίσετε τις πληροφορίες που παρείχατε.    

    ![Προβολή του πίνακα εργαλείων WordPress][WordPressDashboard2]

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης είδατε πώς μπορείτε να δημιουργήσετε και να αναπτύξετε μια εφαρμογή web παράδειγμα από το Azure Marketplace.

Για περισσότερες πληροφορίες σχετικά με τον τρόπο εργασίας με εφαρμογή υπηρεσίας Web Apps, ανατρέξτε στις συνδέσεις στην αριστερή πλευρά της σελίδας (για windows ευρεία προγράμματος περιήγησης) ή στο επάνω μέρος της σελίδας (για windows στενή προγράμματος περιήγησης).

Για περισσότερες πληροφορίες σχετικά με την ανάπτυξη εφαρμογών web WordPress σε Azure, ανατρέξτε στο θέμα [Ανάπτυξη WordPress την υπηρεσία εφαρμογής Azure][WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Δοκιμάστε εφαρμογής υπηρεσίας]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Πύλη του Azure]: https://portal.azure.com/
[Nami έργου]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
