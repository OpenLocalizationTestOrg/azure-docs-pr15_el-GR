<properties
    pageTitle="Γρήγορα αποτελέσματα με εφαρμογές του Mobile Azure εφαρμογής υπηρεσίας για εφαρμογές Xamarin.iOS | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης για γρήγορα αποτελέσματα με τη χρήση εφαρμογών Mobile για την ανάπτυξη Xamarin.iOS."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>Δημιουργία εφαρμογής Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να προσθέσετε μια υπηρεσία υποστήριξης που βασίζεται στο cloud για μια εφαρμογή για κινητές συσκευές Xamarin.iOS, χρησιμοποιώντας μια εφαρμογή για κινητές συσκευές Azure παρασκηνίου.  Μπορείτε να δημιουργήσετε έναν νέο υπολογιστή στο παρασκήνιο εφαρμογής για κινητές συσκευές και μια απλή _λίστα Todo_ εφαρμογή Xamarin.iOS που αποθηκεύει τα δεδομένα εφαρμογής σε Azure.

Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλους άλλα προγράμματα εκμάθησης Xamarin.iOS σχετικά με τη χρήση της δυνατότητας εφαρμογές του Mobile Azure εφαρμογής υπηρεσίας.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τις ακόλουθες προϋποθέσεις:

* Λογαριασμού Azure active. Εάν δεν έχετε ένα λογαριασμό, εγγραφείτε για μια δοκιμαστική έκδοση του Azure και λάβετε έως και 10 δωρεάν εφαρμογές για κινητές συσκευές που μπορείτε να συνεχίσετε να χρησιμοποιείτε ακόμα και μετά την δοκιμαστική τελειώνει. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio με Xamarin. Ανατρέξτε στο θέμα [ρύθμιση και εγκαταστήστε το στοιχείο για το Visual Studio και Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) για οδηγίες.

* Μια Mac με Xcode έκδοση 7.0 ή νεότερη έκδοση και Xamarin Studio Κοινότητας εγκατεστημένο. Ανατρέξτε στο θέμα [ρύθμιση και εγκατάστασης για το Visual Studio και Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) και [εγκατάστασης, εγκατάσταση, και επιθεωρήσεις για χρήστες Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE]Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν εγγραφείτε για ένα λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](https://tryappservice.azure.com/?appServiceName=mobile). Αμέσως, μπορείτε να δημιουργήσετε μια εφαρμογή για κινητές συσκευές μικρής διάρκειας starter στην εφαρμογή υπηρεσίας — απαιτείται πιστωτική κάρτα και χωρίς δεσμεύσεις.

## <a name="create-an-azure-mobile-app-backend"></a>Δημιουργήστε μια εφαρμογή Mobile Azure παρασκηνίου

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε μια εφαρμογή Mobile παρασκηνίου.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Ρύθμιση παραμέτρων του project server

Τώρα που έχουν παρασχεθεί μια εφαρμογή Mobile Azure παρασκηνίου που μπορούν να χρησιμοποιηθούν από το πρόγραμμα-πελάτη κινητών εφαρμογές. Στη συνέχεια, κάντε λήψη του έργου διακομιστή για μια απλή "λίστα todo" παρασκηνίου και δημοσιεύστε το στο Azure.

Ακολουθήστε τα παρακάτω βήματα για τη ρύθμιση παραμέτρων του διακομιστή έργου για να χρησιμοποιήσετε το Node.js ή .NET παρασκηνίου.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Κάντε λήψη και εκτελέστε την εφαρμογή Xamarin.iOS

1. Ανοίξτε την [πύλη του Azure] σε ένα παράθυρο προγράμματος περιήγησης.

2. Στην το blade ρυθμίσεις για την εφαρμογή κινητό σας, κάντε κλικ στην επιλογή **Γρήγορα αποτελέσματα** > **Xamarin.iOS**. Στην περιοχή βήμα 3, κάντε κλικ στην επιλογή **Δημιουργία νέας εφαρμογής** , εάν αυτό δεν είναι ήδη επιλεγμένη.  Στη συνέχεια, κάντε κλικ στο κουμπί **λήψη** .

    Γίνεται λήψη των μια εφαρμογή προγράμματος-πελάτη που συνδέεται με την κινητή παρασκηνίου. Αποθηκεύστε το αρχείο συμπιεσμένο έργου στον τοπικό σας υπολογιστή και σημειώστε όπου το αποθηκεύετε.

3. Εξαγωγή του έργου που έχετε λάβει και, στη συνέχεια, ανοίξτε το στο Xamarin Studio (ή Visual Studio).

    ![][9]

    ![][8]

4. Πατήστε το πλήκτρο F5 για τη δημιουργία του έργου και ξεκινήστε την εφαρμογή του προσομοίωσης iPhone.

5. Στην εφαρμογή, πληκτρολογήστε επεξηγηματικό κείμενο, όπως _Μάθετε Xamarin_, και, στη συνέχεια, κάντε κλικ στην επιλογή το **+** κουμπί.

    ![][10]

    Δεδομένα από την αίτηση εισάγεται στον πίνακα TodoItem. Στοιχεία που είναι αποθηκευμένα στον πίνακα που επιστρέφονται από υπολογιστή στο παρασκήνιο εφαρμογής για κινητές συσκευές και τα δεδομένα εμφανίζονται στη λίστα.

>[AZURE.NOTE]Μπορείτε να δείτε τον κώδικα που έχει πρόσβαση σε σας παρασκηνίου εφαρμογής για κινητές συσκευές στο ερώτημα και να εισαγάγετε δεδομένα στο αρχείο QSTodoService.cs C#.

##<a name="next-steps"></a>Επόμενα βήματα

* [Προσθήκη συγχρονισμού χωρίς σύνδεση για την εφαρμογή σας](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Προσθήκη ελέγχου ταυτότητας για την εφαρμογή σας](app-service-mobile-xamarin-ios-get-started-users.md)
* [Προσθέστε τις ειδοποιήσεις push σας εφαρμογή Xamarin.Android](app-service-mobile-xamarin-ios-get-started-push.md)
* [Πώς μπορείτε να χρησιμοποιήσετε το πρόγραμμα-πελάτη διαχειριζόμενων για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Πύλη του Azure]: https://portal.azure.com/
