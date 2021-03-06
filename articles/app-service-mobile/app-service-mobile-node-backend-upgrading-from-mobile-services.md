<properties
    pageTitle="Αναβάθμιση από τις υπηρεσίες κινητές συσκευές στο Azure εφαρμογής υπηρεσίας - Node.js"
    description="Μάθετε πώς μπορείτε να αναβαθμίσετε εύκολα την εφαρμογή υπηρεσιών Mobile σε μια εφαρμογή της εφαρμογής υπηρεσίας Mobile"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Αναβάθμιση του υπάρχοντος Node.js Azure υπηρεσίας κινητών τηλεφώνων εφαρμογής υπηρεσίας

Εφαρμογή υπηρεσίας Mobile είναι ένα νέο τρόπο για να δημιουργήσετε εφαρμογές για κινητές συσκευές με Windows Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι οι εφαρμογές του Mobile;].

Αυτό το θέμα περιγράφει τον τρόπο για να αναβαθμίσετε μια υπάρχουσα εφαρμογή παρασκηνίου Node.js από τις υπηρεσίες Mobile Azure σε μια νέα εφαρμογή υπηρεσίας οι εφαρμογές του Mobile. Ενώ πραγματοποιείτε αυτήν την αναβάθμιση, την υπάρχουσα εφαρμογή υπηρεσιών Mobile μπορεί να συνεχίσει να λειτουργεί.  Εάν πρέπει να κάνετε αναβάθμιση μιας εφαρμογής παρασκηνίου Node.js, ανατρέξτε στην [αναβάθμιση των υπηρεσιών σας .NET Mobile](./app-service-mobile-net-upgrading-from-mobile-services.md).

Κατά την αναβάθμιση σε υπηρεσία εφαρμογής Azure έναν φορητό υπολογιστή στο παρασκήνιο, θα έχει πρόσβαση σε όλες τις δυνατότητες εφαρμογής υπηρεσίας και χρέωσης σύμφωνα με [τις τιμές εφαρμογής υπηρεσίας], τις τιμές και όχι Mobile υπηρεσιών.

## <a name="migrate-vs-upgrade"></a>Μετεγκατάσταση έναντι αναβάθμισης

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Συνιστάται να που [μετεγκατάσταση](app-service-mobile-migrating-from-mobile-services.md) πριν μεταβείτε μέσω αναβάθμισης. Αυτόν τον τρόπο, μπορείτε να θέσετε και στις δύο εκδόσεις της εφαρμογής σας σε το ίδιο πρόγραμμα υπηρεσίας εφαρμογή και να αναλαμβάνουν χωρίς πρόσθετο κόστος.

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Βελτιώσεις στο διακομιστή Node.js εφαρμογές του Mobile SDK

Αναβάθμιση στο νέο [SDK εφαρμογές του Mobile](https://www.npmjs.com/package/azure-mobile-apps) παρέχει πολλές βελτιώσεις, όπως:

- Με βάση το [Express framework](http://expressjs.com/en/index.html), το νέο SDK κόμβου είναι απλή και έχει σχεδιαστεί για να διατηρήσετε με νέες εκδόσεις κόμβου καθώς προκύπτουν. Μπορείτε να προσαρμόσετε τη συμπεριφορά της εφαρμογής με το ενδιάμεσο Express.

- Βελτιώσεις επιδόσεων σημαντική σε σύγκριση με το SDK υπηρεσίες Mobile.

- Τώρα μπορείτε να φιλοξενήσετε μια τοποθεσία Web μαζί με τις κινητές συσκευές παρασκηνίου; Ομοίως, είναι εύκολο να προσθέσετε στο SDK Mobile Azure σε οποιαδήποτε υπάρχουσα εφαρμογή express.v4.

- Δημιουργηθεί για τις πλατφόρμες και τοπική ανάπτυξη, το SDK εφαρμογές του Mobile μπορεί να είναι που αναπτύχθηκε και εκτελέστε τοπικά σε πλατφόρμες Windows, Linux και OSX. Τώρα είναι εύκολο για χρήση κοινές τεχνικές ανάπτυξης κόμβο όπως εκτέλεση ελέγχων [μόκα](https://mochajs.org/) πριν από την ανάπτυξη.

## <a name="overview"></a>Γενική επισκόπηση αναβάθμισης

Για βοήθεια κατά την αναβάθμιση μιας Node.js παρασκηνίου, Azure εφαρμογής υπηρεσίας έχει παράσχει ένα πακέτο συμβατότητας.  Μετά την αναβάθμιση, θα έχετε μια τοποθεσία niew που μπορεί να αναπτυχθεί σε μια νέα τοποθεσία εφαρμογής υπηρεσίας.

Το πρόγραμμα-πελάτη κινητών υπηρεσιών SDK είναι **δεν** είναι συμβατή με το νέο διακομιστή εφαρμογές του Mobile SDK. Προκειμένου να εξασφαλιστεί συνέχειας της υπηρεσίας για την εφαρμογή σας, που δεν θα πρέπει να δημοσιεύσετε τις αλλαγές σε μια τοποθεσία αυτήν τη στιγμή που εξυπηρετούν δημοσιευμένα προγράμματα-πελάτες. Αντί για αυτό, θα πρέπει να μπορείτε να δημιουργήσετε μια νέα εφαρμογή για κινητές συσκευές που χρησιμοποιείται ως ένα διπλότυπο. Μπορείτε να τοποθετήσετε αυτής της εφαρμογής στο το ίδιο πρόγραμμα εφαρμογής υπηρεσίας για να αποφύγετε τις πρόσθετες οικονομικών κόστος.

Στη συνέχεια, θα έχετε δύο εκδόσεις της εφαρμογής: ένα που να είναι η ίδια και εξυπηρετεί δημοσιευτεί εφαρμογές σε το φυσικό περιβάλλον, και το άλλο που μπορείτε να κάνετε στη συνέχεια, αναβάθμιση και προορισμού με μια νέα έκδοση προγράμματος-πελάτη. Μπορείτε να μετακινήσετε και να ελέγξετε τον κωδικό με το ρυθμό, αλλά θα πρέπει να βεβαιωθείτε ότι τυχόν διορθώσεις σφαλμάτων που κάνετε να εφαρμοστούν και τα δύο. Μόλις πιστεύετε ότι ένα επιθυμητό αριθμό του προγράμματος-πελάτη εφαρμογές από το φυσικό περιβάλλον έχουν ενημερωθεί στην πιο πρόσφατη έκδοση, μπορείτε να διαγράψετε την αρχική εφαρμογή μετεγκαταστάθηκαν εάν που θέλετε. Αυτό δεν περιλαμβάνουν οποιοδήποτε πρόσθετο νομισματικά κόστος, εάν σε το ίδιο πρόγραμμα εφαρμογής υπηρεσίας με την εφαρμογή Mobile.

Το πλήρες διάρθρωσης για τη διαδικασία αναβάθμισης είναι ως εξής:

1. Κάντε λήψη του υπάρχοντος (μετεγκαταστάθηκαν) Azure υπηρεσίας κινητών τηλεφώνων.
2. Μετατροπή του έργου σε μια εφαρμογή Mobile Azure χρησιμοποιώντας το πακέτο συμβατότητας.
3. Διορθώστε τυχόν διαφορές (όπως ρυθμίσεις ελέγχου ταυτότητας).
4. Αναπτύξτε το έργο σας έχει μετατραπεί εφαρμογή Mobile Azure σε μια νέα υπηρεσία εφαρμογής.
4. Αφήστε μια νέα έκδοση της εφαρμογής σας προγράμματος-πελάτη που χρησιμοποιούν τη νέα εφαρμογή Mobile.
5. (Προαιρετικό) Διαγραφή της εφαρμογής σας αρχικό μετεγκαταστάθηκαν υπηρεσία κινητών τηλεφώνων.

Διαγραφή μπορεί να προκύψει όταν δεν μπορείτε να δείτε οποιαδήποτε κίνηση την αρχική μετεγκαταστάθηκαν κινητή υπηρεσία.

## <a name="install-npm-package"></a>Εγκαταστήστε το προαπαιτούμενα

Θα πρέπει να εγκαταστήσετε [κόμβο] στον τοπικό υπολογιστή σας.  Μπορείτε, επίσης, θα πρέπει να εγκαταστήσετε το πακέτο συμβατότητας.  Μετά την εγκατάσταση του κόμβου, μπορείτε να εκτελέσετε την ακόλουθη εντολή από μια νέα cmd ή εντολών του PowerShell:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Λάβετε τις δέσμες ενεργειών υπηρεσιών Azure κινητές συσκευές

- Συνδεθείτε [πύλη του Azure].
- Με **Όλους τους πόρους** ή **Τις υπηρεσίες εφαρμογών**, βρείτε την τοποθεσία Mobile Services.
- Μέσα στην τοποθεσία, κάντε κλικ στην επιλογή **Εργαλεία** -> **Kudu** -> **μεταβείτε** για να ανοίξετε την τοποθεσία Kudu.
- Κάντε κλικ στην **Κονσόλα εντοπισμός σφαλμάτων** -> του**PowerShell** για να ανοίξετε την κονσόλα εντοπισμού σφαλμάτων.
- Μεταβείτε στο `site/wwwroot/App_Data/config` , κάνοντας κλικ σε κάθε κατάλογο με τη σειρά
- Κάντε κλικ στο εικονίδιο "Λήψη" δίπλα στην το `scripts` καταλόγου.

Αυτό θα κάνει λήψη των δεσμών ενεργειών σε μορφή ZIP.  Δημιουργία ενός νέου καταλόγου στον τοπικό υπολογιστή σας και αποσυσκευασία το `scripts.ZIP` αρχείου εντός του καταλόγου.  Αυτό θα δημιουργήσει μια `scripts` καταλόγου.

## <a name="scaffold-app"></a>Scaffold νέα υπόβαθρο εφαρμογές του Mobile Azure

Εκτελέστε την ακόλουθη εντολή από τον κατάλογο που περιέχει τον κατάλογο δεσμών ενεργειών:

```scaffold-mobile-app scripts out```

Αυτό θα δημιουργήσει μια scaffolded παρασκηνίου εφαρμογές του Mobile Azure στο το `out` καταλόγου.  Παρόλο που δεν απαιτείται, είναι καλή ιδέα να ελέγξετε το `out` καταλόγου σε ένα αποθετήριο κώδικα προέλευσης της επιλογής σας.

## <a name="deploy-ama-app"></a>Ανάπτυξη του παρασκηνίου εφαρμογές του Mobile Azure

Κατά την ανάπτυξη, θα πρέπει να κάνετε τα εξής:

1. Δημιουργήστε μια νέα εφαρμογή Mobile στην [Πύλη του Azure].
2. Εκτελέστε το `createViews.sql` δέσμης ενεργειών στη συνδεδεμένη βάση δεδομένων σας.
3. Σύνδεση βάσης δεδομένων που είναι συνδεδεμένο με την υπηρεσία κινητών στη νέα υπηρεσία εφαρμογής.
4. Σύνδεση τυχόν άλλους πόρους (όπως διανομείς ειδοποίηση) με τη νέα υπηρεσία εφαρμογής.
5. Ανάπτυξη κώδικα που δημιουργήθηκε στη νέα σας τοποθεσία.

### <a name="create-a-new-mobile-app"></a>Δημιουργήστε μια νέα εφαρμογή Mobile

1. Συνδεθείτε στην [πύλη του Azure].

2. Κάντε κλικ στο κουμπί **+ ΝΈΑ** > **Web + Mobile** > **Εφαρμογή Mobile**, στη συνέχεια, δώστε ένα όνομα για την εφαρμογή Mobile παρασκηνίου.

3. Για την **Ομάδα πόρων**, επιλέξτε μια υπάρχουσα ομάδα πόρων ή δημιουργήστε ένα νέο (χρησιμοποιώντας το ίδιο όνομα με την εφαρμογή σας.) 
 
    Μπορείτε να επιλέξετε κάποιο άλλο πρόγραμμα εφαρμογής υπηρεσίας ή να δημιουργήστε ένα νέο. Για περισσότερες πληροφορίες σχετικά με τις υπηρεσίες εφαρμογών προγράμματος και πώς μπορείτε να δημιουργήσετε ένα νέο πρόγραμμα σε ένα διαφορετικό τιμολόγησης σειρά και στην επιθυμητή θέση σας, ανατρέξτε στο θέμα [Επισκόπηση αναλυτικά σχέδια Azure εφαρμογής υπηρεσίας](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. Για το **πρόγραμμα εφαρμογής υπηρεσίας**, το πρόγραμμα προεπιλεγμένη (σε [τυπική σειρά](https://azure.microsoft.com/pricing/details/app-service/)) είναι επιλεγμένο. Μπορείτε επίσης να επιλέξετε ένα διαφορετικό πρόγραμμα ή [Δημιουργήστε ένα νέο](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Ρυθμίσεις του σχεδίου εφαρμογής υπηρεσίας καθορίζουν τη [θέση, δυνατότητες, κόστους και τον υπολογισμό τους πόρους](https://azure.microsoft.com/pricing/details/app-service/) που σχετίζονται με την εφαρμογή σας. 

    Αφού αποφασίσετε στο σχέδιο, κάντε κλικ στην επιλογή **Δημιουργία**. Με τον τρόπο αυτό δημιουργείται η εφαρμογή Mobile παρασκηνίου. 


### <a name="run-createviewssql"></a>Εκτέλεση CreateViews.SQL

Η εφαρμογή scaffolded περιέχει ένα αρχείο που ονομάζεται `createViews.sql`.  Αυτή η δέσμη ενεργειών πρέπει να εκτελείται σε σχέση με τη βάση δεδομένων προορισμού.  Η συμβολοσειρά σύνδεσης για τη βάση δεδομένων προορισμού μπορεί να ληφθεί από την μετεγκαταστάθηκαν κινητή υπηρεσία από το blade **Ρυθμίσεις** στην περιοχή **Συμβολοσειρές σύνδεσης**.  Ονομάζεται `MS_TableConnectionString`.

Μπορείτε να εκτελέσετε αυτήν τη δέσμη ενεργειών από μέσα από το SQL Server Management Studio ή Visual Studio.

### <a name="link-the-database-to-your-app-service"></a>Σύνδεση της βάσης δεδομένων της εφαρμογής υπηρεσίας

Σύνδεση της υπάρχουσας βάσης δεδομένων με την υπηρεσία εφαρμογής:

- Στην [Πύλη του Azure], ανοίξτε την εφαρμογή υπηρεσίας.
- Επιλέξτε **όλες τις ρυθμίσεις** -> **συνδέσεις δεδομένων**.
- Κάντε κλικ στο **+ Προσθήκη**.
- Στην αναπτυσσόμενη λίστα, επιλέξτε **Βάση δεδομένων SQL**
- Στην περιοχή **Βάση δεδομένων SQL**, επιλέξτε την υπάρχουσα βάση δεδομένων και, στη συνέχεια, κάντε κλικ στην **επιλογή**.
- Στην περιοχή **συμβολοσειρά σύνδεσης**, πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης για τη βάση δεδομένων και, στη συνέχεια, κάντε κλικ στο **κουμπί OK**.
- Στο το blade **Προσθήκη συνδέσεων δεδομένων** , κάντε κλικ στο **κουμπί OK**.

Μπορείτε να βρείτε το όνομα χρήστη και τον κωδικό πρόσβασης, προβάλλοντας τη συμβολοσειρά σύνδεσης για τη βάση δεδομένων προορισμού στην υπηρεσία μετεγκαταστάθηκαν Mobile.


### <a name="set-up-authentication"></a>Ρύθμιση του ελέγχου ταυτότητας

Azure εφαρμογές του Mobile σάς επιτρέπει να ρυθμίσετε τις παραμέτρους Azure Active Directory, Facebook, Google, Microsoft και Twitter ελέγχου ταυτότητας εντός της υπηρεσίας.  Προσαρμοσμένο έλεγχο ταυτότητας θα πρέπει να αναπτυχθούν ξεχωριστά.  Ανατρέξτε στο τεκμηρίωση [Έννοιες ελέγχου ταυτότητας] και τεκμηρίωση [Γρήγορη έναρξη ελέγχου ταυτότητας] για περισσότερες πληροφορίες.  

## <a name="updating-clients"></a>Ενημέρωση κινητές συσκευές

Όταν έχετε μια λειτουργικές παρασκηνίου εφαρμογή Mobile, μπορείτε να εργαστείτε σε μια νέα έκδοση της εφαρμογής σας προγράμματος-πελάτη που χρησιμοποιεί το. Εφαρμογές του Mobile περιλαμβάνει επίσης μια νέα έκδοση του προγράμματος-πελάτη SDK και παρόμοια με την αναβάθμιση του διακομιστή παραπάνω, θα πρέπει να καταργήσετε όλες τις αναφορές του SDK υπηρεσίες Mobile πριν από την εγκατάσταση των εκδόσεων εφαρμογές του Mobile.

Μία από τις κύριες αλλαγές μεταξύ των εκδόσεων είναι ότι το κατασκευές δεν χρειάζεστε πλέον ένα πλήκτρο εφαρμογής. Τώρα απλώς περάσετε στη διεύθυνση URL της εφαρμογής Mobile. Για παράδειγμα, σε υπολογιστές-πελάτες .NET, το `MobileServiceClient` κατασκευή είναι τώρα:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Μπορείτε να διαβάσετε σχετικά με την εγκατάσταση του νέου SDK και χρησιμοποιώντας τη νέα δομή μέσω τις παρακάτω συνδέσεις:

- [Android έκδοση 2,2 ή νεότερη έκδοση](app-service-mobile-android-how-to-use-client-library.md)
- [iOS έκδοση 3.0.0 ή νεότερη έκδοση](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) έκδοση 2.0.0 ή νεότερη έκδοση](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova έκδοση 2.0 ή νεότερη έκδοση](app-service-mobile-cordova-how-to-use-client-library.md)

Εάν η εφαρμογή σας κάνει χρήση ειδοποιήσεων push, σημειώστε τις οδηγίες συγκεκριμένες εγγραφής για κάθε πλατφόρμα, ως υπήρξε υπάρχουν ορισμένες αλλαγές καθώς και.

Όταν έχετε τη νέα έκδοση προγράμματος-πελάτη που είναι έτοιμη, δοκιμή, σε σχέση με το έργο σας αναβαθμισμένο διακομιστή. Μετά την επαλήθευση ότι λειτουργεί, μπορείτε να αποδεσμεύσετε μια νέα έκδοση της εφαρμογής σας σε "πελάτες". Τελικά, όταν οι πελάτες σας να είχατε την ευκαιρία να λάβετε αυτές τις ενημερωμένες εκδόσεις, μπορείτε να διαγράψετε την έκδοση Mobile υπηρεσιών από την εφαρμογή σας. Σε αυτό το σημείο, έχετε αναβαθμίσει εντελώς σε μια εφαρμογή Mobile εφαρμογής υπηρεσίας, χρησιμοποιώντας το πιο πρόσφατες εφαρμογές του Mobile διακομιστή SDK.

<!-- URLs. -->

[Πύλη του Azure]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Τι είναι οι εφαρμογές του Mobile;]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Εφαρμογή υπηρεσίας τις τιμές]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Έλεγχος ταυτότητας έννοιες]: ../app-service/app-service-authentication-overview.md
[Γρήγορη έναρξη ελέγχου ταυτότητας]: app-service-mobile-auth.md

[Πύλη του Azure]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
