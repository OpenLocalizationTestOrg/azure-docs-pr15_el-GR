<properties
    pageTitle="WebJobs στο Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς να δημιουργείτε WebJobs εκτέλεση ελέγχων φόντου, αλληλεπιδρούν με τις υπηρεσίες όπως το χώρο αποθήκευσης και Bus υπηρεσίας και τη δημιουργία προγραμματισμένες εργασίες."
    services="app-service"
    documentationCenter=""
    authors="christopheranderson"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="chrande"/>

# <a name="using-webjobs-in-azure-app-service"></a>Χρήση WebJobs στο Azure εφαρμογής υπηρεσίας

Σε αυτό το άρθρο παρέχει συνδέσεις σε πόρους τεκμηρίωσης σχετικά με τη χρήση Azure WebJobs και το SDK WebJobs Azure. Azure WebJobs παρέχουν έναν εύκολο τρόπο για να εκτελέσετε δέσμες ενεργειών ή προγράμματα ως διεργασίες παρασκηνίου στην [Εφαρμογή υπηρεσίας Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Μπορείτε να αποστείλετε και να εκτελέσετε ένα εκτελέσιμο αρχείο όπως ως cmd, Μπατ, exe (.NET), ps1, ε, php, γραφή, js και βάζο. Αυτά τα προγράμματα εκτελούνται ως WebJobs σε ένα χρονοδιάγραμμα (cron) ή συνεχώς.

Το SDK WebJobs διευκολύνει να χρησιμοποιήσετε το χώρο αποθήκευσης Azure. Το SDK WebJobs έχει σύνδεσης και σύστημα έναυσμα, το οποίο λειτουργεί με αντικείμενα blob του Microsoft Azure χώρου αποθήκευσης, ουρές και πίνακες καθώς και ουρές Bus υπηρεσίας.

Δημιουργία, την ανάπτυξη και τη Διαχείριση WebJobs είναι απρόσκοπτη με ενσωματωμένη tooling στο Visual Studio. Μπορείτε να δημιουργήσετε WebJobs από πρότυπα, δημοσίευση, και διαχείριση (εκτέλεση/διακοπή/οθόνη/εντοπισμός σφαλμάτων) τους.

Ο πίνακας εργαλείων WebJobs στην πύλη του Azure παρέχει δυνατότητες ισχυρή διαχείρισης που να έχετε πλήρη έλεγχο σε την εκτέλεση της WebJobs, συμπεριλαμβανομένης της δυνατότητας για να καλέσετε μεμονωμένα συναρτήσεων μέσα σε WebJobs. Στον πίνακα εργαλείων εμφανίζει επίσης συνάρτηση χρόνους εκτέλεσης και καταγραφή εξόδου.

[AZURE.INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]
