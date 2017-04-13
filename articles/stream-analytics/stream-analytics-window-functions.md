<properties
    pageTitle="Εισαγωγή στις συναρτήσεις ροή αναλυτικών στοιχείων παραθύρου | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με τις τρεις λειτουργίες παραθύρου στην ανάλυση ροής (tumbling, διασκέδαση, κύλιση)."
    keywords="tumbling παράθυρο, η ρύθμιση προς το παράθυρο, διασκέδαση παραθύρου"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="introduction-to-stream-analytics-window-functions"></a>Εισαγωγή στις συναρτήσεις ροή αναλυτικών στοιχείων παραθύρου

Σε πολλές πραγματικό χρόνο streaming σενάρια, είναι απαραίτητο να εκτελέσετε λειτουργίες μόνο για τα δεδομένα που περιέχονται στο χρονικό των windows. Εγγενή υποστήριξη για άνοιγμα παραθύρου συναρτήσεις είναι μια βασική λειτουργία του Azure ροή ανάλυσης που μετακινεί βελόνας παραγωγικότητα προγραμματιστής σύνταξης επεξεργασία σύνθετων ροή εργασιών. Ανάλυση ροή επιτρέπει στους προγραμματιστές να χρησιμοποιήσετε [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) και [**ολισθαίνουσες**](https://msdn.microsoft.com/library/dn835051.aspx) windows για να εκτελέσετε λειτουργίες χρονικό στη ροή δεδομένων. Είναι αξίζει να αναφερθούν ότι όλες οι λειτουργίες [παράθυρο](https://msdn.microsoft.com/library/dn835019.aspx) εξόδου αποτελέσματα στο **Τέλος** του παραθύρου. Το αποτέλεσμα του παραθύρου θα μεμονωμένο συμβάν με βάση τη συνάρτηση συγκεντρωτικών αποτελεσμάτων που χρησιμοποιείται. Το συμβάν θα έχει τη χρονική σήμανση από τη λήξη του παραθύρου και όλες τις συναρτήσεις παραθύρου ορίζονται με καθορισμένο μήκος. Τέλος είναι σημαντικό να λάβετε υπόψη ότι όλες οι λειτουργίες παραθύρου πρέπει να χρησιμοποιηθεί σε έναν όρο [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) .

![Ροή έννοιες συναρτήσεις αναλυτικών στοιχείων παραθύρου](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Παράθυρο tumbling

Tumbling παραθύρου συναρτήσεις που χρησιμοποιούνται για να χωρίσετε μια ροή δεδομένων σε τμήματα διακριτές χρόνου και να εκτελέσετε μια συνάρτηση σε σχέση με αυτά, όπως το παρακάτω παράδειγμα. Το πλήκτρο παράγοντες διαφοροποίησης ενός παραθύρου Tumbling είναι ότι αυτά Επαναλάβετε, μην επικαλύπτονται και ένα συμβάν δεν είναι δυνατό να ανήκουν σε περισσότερες από μία tumbling παραθύρου.

![Συναρτήσεις αναλυτικών στοιχείων παραθύρου ροή tumbling εισαγωγή](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Διασκέδαση παραθύρου

Διασκέδαση μεταπήδηση συναρτήσεις παραθύρου προς τα εμπρός σε χρόνο από μια καθορισμένη περίοδο. Μπορεί να είναι εύκολο να φανταστείτε τις ως windows Tumbling που μπορεί να επικαλύπτονται, ώστε να συμβάντα μπορούν να ανήκουν σε περισσότερα από ένα σύνολο αποτελεσμάτων παράθυρο Hopping. Για να δημιουργήσετε ένα παράθυρο Hopping ίδια με μια Tumbling παραθύρου μία θα απλώς να προσδιορίσετε το μέγεθος μεταπήδηση να είναι το ίδιο με το μέγεθος του παραθύρου. 

![Συναρτήσεις αναλυτικών στοιχείων παραθύρου ροή διασκέδαση εισαγωγή](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Κυλιόμενο παράθυρο

Κυλιόμενο παράθυρο συναρτήσεις, σε αντίθεση με τα windows Tumbling ή Hopping, παράγει ένα αποτέλεσμα **μόνο** όταν παρουσιάζεται κάποιο συμβάν. Κάθε παραθύρου θα έχει τουλάχιστον μία συμβάντων και το παράθυρο συνεχώς Μετακίνηση προς τα εμπρός από μια € (Έψιλον). Όπως το Windows διασκέδαση, συμβάντα μπορούν να ανήκουν σε περισσότερα από ένα παράθυρα ολίσθηση.

![Συναρτήσεις παράθυρο ανάλυσης ροή η ρύθμιση προς εισαγωγή](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Λήψη Βοήθειας με συναρτήσεις παραθύρου

Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ ανάλυση ροή Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)
