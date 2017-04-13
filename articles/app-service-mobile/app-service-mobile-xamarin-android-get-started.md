<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure εφαρμογών για κινητές συσκευές Xamarin.Android εφαρμογές"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης για να ξεκινήσετε τη χρήση εφαρμογών του Azure Mobile για Xamarin Android ανάπτυξη"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>Δημιουργία εφαρμογής Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να προσθέσετε μια υπηρεσία υποστήριξης που βασίζεται στο cloud σε μια εφαρμογή Xamarin.Android. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι οι εφαρμογές του Mobile](app-service-mobile-value-prop.md).

Στιγμιότυπο οθόνης από την εφαρμογή ολοκληρωμένων είναι κάτω από το στοιχείο:

![][0]

Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλα άλλες εφαρμογές του Mobile τα προγράμματα εκμάθησης για τις εφαρμογές Xamarin.Android.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τις ακόλουθες προϋποθέσεις:

* Λογαριασμού Azure active. Εάν δεν έχετε ένα λογαριασμό, εγγραφείτε για μια δοκιμαστική Azure και λάβετε έως και 10 δωρεάν εφαρμογές του Mobile. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio με Xamarin. Ανατρέξτε στο θέμα [ρύθμιση και εγκαταστήστε το στοιχείο για το Visual Studio και Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) για οδηγίες.

>[AZURE.NOTE]Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](https://tryappservice.azure.com/?appServiceName=mobile).  Αμέσως, μπορείτε να δημιουργήσετε μια μικρής διάρκειας starter εφαρμογή Mobile στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

## <a name="create-an-azure-mobile-app-backend"></a>Δημιουργήστε μια εφαρμογή Mobile Azure παρασκηνίου

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε μια εφαρμογή Mobile παρασκηνίου.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Τώρα που έχουν παρασχεθεί μια εφαρμογή Mobile Azure παρασκηνίου που μπορούν να χρησιμοποιηθούν από το πρόγραμμα-πελάτη κινητών εφαρμογές. Στη συνέχεια, κάντε λήψη του έργου διακομιστή για μια απλή "λίστα todo" παρασκηνίου και δημοσιεύστε το στο Azure.

## <a name="configure-the-server-project"></a>Ρύθμιση παραμέτρων του project server

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Κάντε λήψη και εκτελέστε την εφαρμογή Xamarin.Android

1. Στην περιοχή, **κάντε λήψη και εκτελέστε το έργο σας Xamarin.Android**, κάντε κλικ στο κουμπί **λήψη** .

    Αποθηκεύστε το αρχείο συμπιεσμένο έργου στον τοπικό σας υπολογιστή και σημειώστε όπου το αποθηκεύετε.

2. Πατήστε το πλήκτρο **F5** για τη δημιουργία του έργου και να ξεκινήσετε την εφαρμογή.

3. Στην εφαρμογή, πληκτρολογήστε επεξηγηματικό κείμενο, όπως η _ολοκλήρωση του προγράμματος εκμάθησης_ και, στη συνέχεια, κάντε κλικ στο κουμπί **Προσθήκη** .

    ![][10]

    Δεδομένα από την αίτηση εισάγεται στον πίνακα TodoItem. Στοιχεία που είναι αποθηκευμένα στον πίνακα που επιστρέφονται από υπολογιστή στο παρασκήνιο εφαρμογής για κινητές συσκευές και τα δεδομένα που εμφανίζονται στη λίστα.

    > [AZURE.NOTE] Μπορείτε να δείτε τον κώδικα που έχει πρόσβαση σε σας υπόβαθρο εφαρμογής για κινητές συσκευές για να υποβάλετε ερώτημα και εισαγωγή δεδομένων, η οποία βρίσκεται στο αρχείο ToDoActivity.cs C#.

##<a name="next-steps"></a>Επόμενα βήματα

* [Προσθήκη συγχρονισμού χωρίς σύνδεση για την εφαρμογή σας](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Προσθήκη ελέγχου ταυτότητας για την εφαρμογή σας](app-service-mobile-xamarin-android-get-started-users.md)
* [Προσθέστε τις ειδοποιήσεις push σας εφαρμογή Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md)
* [Πώς μπορείτε να χρησιμοποιήσετε το πρόγραμμα-πελάτη διαχειριζόμενων για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
