<properties
    pageTitle="Δημιουργία εφαρμογής Cordova στην Azure εφαρμογής υπηρεσίας εφαρμογές για κινητές συσκευές | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης για να ξεκινήσετε με τη χρήση παρασκήνιο Azure εφαρμογής για κινητές συσκευές για την ανάπτυξη Apache Cordova"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova, javascript, πρόγραμμα-πελάτη κινητών," />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>Δημιουργία μιας Apache Cordova εφαρμογής

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να προσθέσετε μια υπηρεσία υποστήριξης που βασίζεται στο cloud για μια εφαρμογή για κινητές συσκευές Apache Cordova, χρησιμοποιώντας μια εφαρμογή για κινητές συσκευές Azure παρασκηνίου.  Θα δημιουργήσετε έναν νέο υπολογιστή στο παρασκήνιο εφαρμογής για κινητές συσκευές και μια απλή _λίστα Todo_ εφαρμογή Apache Cordova που αποθηκεύει τα δεδομένα εφαρμογής σε Azure.

Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλους άλλα προγράμματα εκμάθησης Apache Cordova σχετικά με τη χρήση της δυνατότητας εφαρμογές του Mobile Azure εφαρμογής υπηρεσίας.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

* Σε Υπολογιστή με το [Visual Studio Κοινότητας 2015] ή νεότερη έκδοση.
* Τα [Εργαλεία του visual Studio για Apache Cordova].
* Ένας [λογαριασμός Azure active](https://azure.microsoft.com/pricing/free-trial/).

Μπορείτε επίσης μπορεί να παρακάμψει Visual Studio και να χρησιμοποιήσετε τη γραμμή εντολών Apache Cordova απευθείας.  Αυτό είναι χρήσιμο όταν ολοκληρωθεί το πρόγραμμα εκμάθησης σε υπολογιστή Mac.  Μεταγλώττιση εφαρμογές προγράμματος-πελάτη Apache Cordova χρησιμοποιώντας τη γραμμή εντολών δεν καλύπτεται από αυτό το πρόγραμμα εκμάθησης.

## <a name="create-a-new-azure-mobile-app-backend"></a>Δημιουργήστε έναν νέο υπολογιστή στο παρασκήνιο Azure εφαρμογής για κινητές συσκευές

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Παρακολουθήστε ένα βίντεο που δείχνει παρόμοια βήματα](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Ρύθμιση παραμέτρων του project server

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Κάντε λήψη και εκτελέστε την εφαρμογή Apache Cordova

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που ολοκληρωθεί αυτό το πρόγραμμα εκμάθησης γρήγορης εκκίνησης, μετακίνηση ένα από τα παρακάτω προγράμματα εκμάθησης:

* [Προσθέστε τον έλεγχο ταυτότητας] του Apache Cordova εφαρμογής.
* [Προσθέστε τις ειδοποιήσεις Push] σας Apache Cordova εφαρμογής.

Μάθετε περισσότερα σχετικά με τις βασικές έννοιες με Azure εφαρμογής υπηρεσίας.

* [Έλεγχος ταυτότητας]
* [Ειδοποιήσεις Push]

Μάθετε πώς μπορείτε να χρησιμοποιήσετε το SDK.

* [Apache Cordova SDK]
* [Διακομιστής ASP.NET SDK]
* [Node.js διακομιστή SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Κοινότητα του Visual Studio 2015]: http://www.visualstudio.com/
[Εργαλεία του Visual Studio για Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Προσθέστε τον έλεγχο ταυτότητας]: app-service-mobile-cordova-get-started-users.md
[Προσθέστε τις ειδοποιήσεις Push]: app-service-mobile-cordova-get-started-push.md
[Έλεγχος ταυτότητας]: app-service-mobile-auth.md
[Ειδοποιήσεις Push]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[Διακομιστής ASP.NET SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js διακομιστή SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
