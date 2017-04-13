<properties
    pageTitle="Δημιουργία ενός ενιαίου Windows πλατφόρμα (UWP) που χρησιμοποιεί σε εφαρμογές του Mobile | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης για να ξεκινήσετε με τη χρήση παρασκήνιο Azure εφαρμογής για κινητές συσκευές για ανάπτυξη εφαρμογών καθολικής πλατφόρμας των Windows (UWP) στο C#, Visual Basic ή JavaScript."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-windows-app"></a>Δημιουργήστε μια εφαρμογή για Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να προσθέσετε μια υπηρεσία υποστήριξης που βασίζεται στο cloud σε μια εφαρμογή καθολικής πλατφόρμας των Windows (UWP). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι οι εφαρμογές του Mobile](app-service-mobile-value-prop.md). Ακολουθούν οι καταγραφές οθόνης από την ολοκληρωμένη εφαρμογή:

![Ολοκλήρωση της εφαρμογής υπολογιστή](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Εκτελείται σε έναν επιτραπέζιο υπολογιστή. 

![Εφαρμογή ολοκληρωμένων phone](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Εκτελείται σε κινητό τηλέφωνο

Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλους άλλα προγράμματα εκμάθησης εφαρμογή Mobile για τις εφαρμογές UWP. 

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

* Λογαριασμού Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να εγγραφείτε για μια δοκιμαστική έκδοση του Azure και να λάβετε έως και 10 δωρεάν εφαρμογές για κινητές συσκευές που μπορείτε να συνεχίσετε να χρησιμοποιείτε ακόμα και μετά την δοκιμαστική τελειώνει. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio Κοινότητας 2015] ή νεότερη έκδοση.

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν εγγραφείτε για ένα λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](https://tryappservice.azure.com/?appServiceName=mobile). Εκεί, μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή για κινητές συσκευές μικρής διάρκειας starter στην εφαρμογή υπηρεσίας — απαιτείται πιστωτική κάρτα και χωρίς δεσμεύσεις.

##<a name="create-a-new-azure-mobile-app-backend"></a>Δημιουργήστε μια νέα εφαρμογή Mobile Azure παρασκηνίου

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε μια νέα εφαρμογή Mobile παρασκηνίου.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Τώρα που έχουν παρασχεθεί μια εφαρμογή Mobile Azure παρασκηνίου που μπορούν να χρησιμοποιηθούν από το πρόγραμμα-πελάτη κινητών εφαρμογές. Στη συνέχεια, θα λαμβάνετε ένα έργο διακομιστή για μια απλή "λίστα todo" παρασκηνίου και δημοσιεύστε το στο Azure.

## <a name="configure-the-server-project"></a>Ρύθμιση παραμέτρων του project server

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-client-project"></a>Κάντε λήψη και εκτελέστε το έργο προγράμματος-πελάτη

Αφού έχετε ρυθμίσει τις παραμέτρους του παρασκηνίου εφαρμογή Mobile, μπορείτε να δημιουργήσετε μια νέα εφαρμογή προγράμματος-πελάτη ή να τροποποιήσετε μια υπάρχουσα εφαρμογή για να συνδεθείτε με Azure. Σε αυτήν την ενότητα, μπορείτε να αποκτήσετε ένα έργο UWP εφαρμογή προτύπου που έχει προσαρμοστεί για να συνδεθείτε με την εφαρμογή Mobile παρασκηνίου.

1. Πίσω στο το blade **γρήγορης εκκίνησης** για την εφαρμογή Mobile παρασκηνίου, κάντε κλικ στην επιλογή **Δημιουργία νέας εφαρμογής** > **λήψη**, στη συνέχεια, εξαγάγετε το συμπιεσμένο έργο αρχεία στον τοπικό σας υπολογιστή.

    ![Κάντε λήψη του Windows γρήγορη έναρξη έργου](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. (Προαιρετικό) Προσθέστε το έργο εφαρμογής UWP στην ίδια λύση ως του project server. Αυτό διευκολύνει να εντοπισμός σφαλμάτων και να ελέγξετε την εφαρμογή και υπόβαθρο στην ίδια λύση Visual Studio, εάν επιλέξετε να το κάνετε. Για να προσθέσετε ένα έργο εφαρμογής UWP τη λύση, πρέπει χρησιμοποιείτε το Visual Studio 2015 ή νεότερη έκδοση.

4. Με την εφαρμογή UWP με το έργο της εκκίνησης, πιέστε το πλήκτρο F5 για να αναπτύξετε και να εκτελέσετε την εφαρμογή.

5. Στην εφαρμογή, πληκτρολογήστε επεξηγηματικό κείμενο, όπως η *Ολοκλήρωση το πρόγραμμα εκμάθησης*, στο πλαίσιο κειμένου **εισαγάγετε έναν TodoItem** και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Γρήγορη έναρξη των Windows ολοκλήρωσης επιφάνειας εργασίας](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Στέλνει την αίτηση ΚΑΤΑΧΏΡΗΣΗ για το νέο παρασκηνίου εφαρμογής για κινητές συσκευές που φιλοξενείται στο Azure.

6. (Προαιρετικό) Διακοπή της εφαρμογής και επανεκκινήστε το σε μια διαφορετική συσκευή ή κινητές συσκευές προσομοίωσης.

    ![Ολοκλήρωση τηλέφωνο γρήγορη έναρξη των Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Παρατηρήστε ότι δεδομένα που αποθηκεύονται από το προηγούμενο βήμα φορτώνεται από το Azure μετά την εκκίνηση της εφαρμογής UWP. 

##<a name="next-steps"></a>Επόμενα βήματα

* [Προσθήκη ελέγχου ταυτότητας για την εφαρμογή σας](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Μάθετε πώς να ελέγχουν την ταυτότητα χρηστών από την εφαρμογή σας με μια υπηρεσία παροχής ταυτότητας.

* [Προσθέστε τις ειδοποιήσεις push για την εφαρμογή σας](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Μάθετε πώς μπορείτε να προσθέσετε push ειδοποιήσεις υποστήριξη για να την εφαρμογή σας και ρύθμιση παραμέτρων σας εφαρμογή Mobile υποστήριξης για την αποστολή ειδοποιήσεων push Azure ειδοποίηση διανομείς.

* [Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή σας](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Μάθετε πώς μπορείτε να προσθέσετε την εφαρμογή σας χρησιμοποιώντας μια εφαρμογή Mobile παρασκηνίου υποστήριξη χωρίς σύνδεση. Συγχρονισμού χωρίς σύνδεση επιτρέπει στους τελικούς χρήστες να αλληλεπιδράσετε με μια εφαρμογή για κινητές συσκευές&mdash;προβολή, προσθήκη ή τροποποίηση δεδομένων&mdash;ακόμα και όταν δεν υπάρχει σύνδεση δικτύου.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Κοινότητα του Visual Studio 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
