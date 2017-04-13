<properties 
    pageTitle="Πώς μπορείτε να συνδεθείτε συμβάντα διανομείς συμβάν Azure Azure API διαχείρισης | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να συνδεθείτε συμβάντα διανομείς συμβάν Azure Azure API διαχείρισης." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Πώς μπορείτε να συνδεθείτε συμβάντα διανομείς συμβάν Azure Azure API διαχείρισης

Azure διανομείς συμβάν είναι μια υπηρεσία εισόδου ιδιαίτερα με δεδομένα που μπορούν να ingest εκατομμύρια συμβάντα ανά δευτερόλεπτο, έτσι ώστε να μπορείτε να διεργασίας και να αναλύσετε τα τεράστιες ποσότητες δεδομένων που παράγονται από τις συνδεδεμένες συσκευές και εφαρμογές. Συμβάν διανομείς λειτουργεί ως η "θύρα εμπρός" για μια διαδικασία συμβάντος και μετά τη συλλογή δεδομένων σε ένα συμβάν διανομέα, μπορούν να μετατραπούν και είναι αποθηκευμένο χρησιμοποιώντας οποιαδήποτε υπηρεσία παροχής σε πραγματικό χρόνο ανάλυση ή προσαρμογέων δέσμης αποθήκευσης. Συμβάν διανομείς αποσυνδέει την παραγωγή μιας ροής συμβάντα από την κατανάλωση από αυτά τα συμβάντα, έτσι ώστε να καταναλωτές συμβάντων μπορούν να έχουν πρόσβαση τα συμβάντα στο δικό τους χρονοδιάγραμμα.

Σε αυτό το άρθρο συνοδεύει το βίντεο [Ενοποίηση Azure API διαχείρισης με διανομείς συμβάντος](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) και περιγράφει τον τρόπο για την καταγραφή συμβάντων API διαχείρισης χρησιμοποιώντας Azure συμβάν διανομείς.

## <a name="create-an-azure-event-hub"></a>Δημιουργήστε ένα διανομέα Azure συμβάντος

Για να δημιουργήσετε ένα νέο συμβάν διανομέα, εισόδου του [Azure κλασική πύλη](https://manage.windowsazure.com) και κάντε κλικ στην επιλογή **Δημιουργία**->**Εφαρμογή υπηρεσιών**->**Bus υπηρεσίας**->**Διανομέα συμβάν**->**Γρήγορης δημιουργίας**. Πληκτρολογήστε ένα όνομα συμβάντος διανομέα, περιοχή, επιλέξτε μια συνδρομή και, επιλέξτε ένα χώρο ονομάτων. Εάν δεν έχετε προηγουμένως δημιουργήσει ένα χώρο ονομάτων μπορείτε να δημιουργήσετε ένα, πληκτρολογώντας ένα όνομα στο πλαίσιο κειμένου **Namespace** . Όταν έχουν ρυθμιστεί όλες τις ιδιότητες, κάντε κλικ στην επιλογή **Δημιουργία διανομέα νέου συμβάντος** για να δημιουργήσετε την ενότητα συμβάντων.

![Δημιουργία συμβάντος διανομέα][create-event-hub]

Στη συνέχεια, μεταβείτε στην καρτέλα **Ρύθμιση παραμέτρων** για το νέο συμβάν διανομέα και δημιουργήστε δύο **πολιτικές κοινόχρηστη πρόσβαση**. Δώστε ένα όνομα πρώτα μία **Αποστολή** και να της δώσετε δικαιώματα **Αποστολή** .

![Αποστολή πολιτικής][sending-policy]

Ονομάστε το δεύτερη **λαμβάνει**, δώσετε δικαιώματα **ακρόαση** και κάντε κλικ στην επιλογή **Αποθήκευση**.

![Λήψη πολιτικής][receiving-policy]

Κάθε πολιτική κοινόχρηστη πρόσβαση επιτρέπει στις εφαρμογές για αποστολή και λήψη συμβάντα προς και από την ενότητα συμβάντων. Για πρόσβαση των συμβολοσειρών σύνδεσης για αυτές τις πολιτικές, μεταβείτε στην καρτέλα **πίνακα εργαλείων** από την ενότητα συμβάντων και κάντε κλικ στην επιλογή **πληροφορίες σύνδεσης**.

![Συμβολοσειρά σύνδεσης][event-hub-dashboard]

Η συμβολοσειρά σύνδεσης **Αποστολή** χρησιμοποιείται κατά την καταγραφή συμβάντων και τη συμβολοσειρά σύνδεσης **λαμβάνει** χρησιμοποιείται κατά τη λήψη συμβάντων από την ενότητα συμβάντων.

![Συμβολοσειρά σύνδεσης][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Δημιουργία μιας καταγραφής API διαχείρισης

Τώρα που έχετε ένα συμβάν διανομέα, το επόμενο βήμα είναι να ρυθμίσετε τις παραμέτρους μιας [καταγραφής](https://msdn.microsoft.com/library/azure/mt592020.aspx) στην υπηρεσία API διαχείρισης, έτσι ώστε να μπορούν να συνδεθούν συμβάντα ενότητα συμβάντων.

API διαχείρισης καταγραφής ακολουθίας χαρακτήρων πληκτρολόγησης έχουν ρυθμιστεί με χρήση του [API διαχείρισης REST API](http://aka.ms/smapi). Πριν να χρησιμοποιήσετε το REST API για πρώτη φορά, εξετάστε τις [προϋποθέσεις](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) και βεβαιωθείτε ότι έχετε [ενεργοποιήσει την πρόσβαση για το REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI).

Για να δημιουργήσετε ένα πρόγραμμα καταγραφής, κάνετε μια αίτηση HTTP ΤΟΠΟΘΈΤΗΣΗ χρησιμοποιώντας την παρακάτω διεύθυνση URL προτύπου.

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   Αντικατάσταση `{your service}` με το όνομα του την παρουσία της υπηρεσίας διαχείρισης API.
-   Αντικατάσταση `{new logger name}` με το όνομα που θέλετε για το νέο πρόγραμμα καταγραφής. Θα αναφέρεστε σε αυτό το όνομα όταν ρυθμίζετε τις παραμέτρους της πολιτικής [καταγραφής-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)

Προσθέστε τις ακόλουθες κεφαλίδες την αίτηση.

-   Τύπου περιεχομένου: εφαρμογή/json
-   Εξουσιοδότηση: SharedAccessSignature uid =...
    -   Για οδηγίες σχετικά με τη δημιουργία του `SharedAccessSignature` δείτε [Azure API διαχείρισης REST API του ελέγχου ταυτότητας](https://msdn.microsoft.com/library/azure/dn798668.aspx).

Καθορίστε το σώμα αίτηση χρησιμοποιώντας το ακόλουθο πρότυπο.

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`πρέπει να οριστεί σε `AzureEventHub`.
-   `description`παρέχει μια προαιρετική περιγραφή του στο πρόγραμμα καταγραφής και μπορεί να είναι συμβολοσειρά μηδενικού μήκους, εάν θέλετε.
-   `credentials`περιέχει το `name` και `connectionString` σας διανομέα συμβάν Azure.

Όταν κάνετε την αίτηση, εάν το πρόγραμμα καταγραφής δημιουργείται κωδικό κατάστασης `201 Created` επιστρέφεται. 

>[AZURE.NOTE] Για τους λόγους και άλλων πιθανών κωδικών επιστροφής, ανατρέξτε στο θέμα [Δημιουργία μιας καταγραφής](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT). Για να δείτε πώς να εκτελέσετε άλλες λειτουργίες, όπως λίστας, ενημέρωσης και διαγραφής, ανατρέξτε στην τεκμηρίωση οντότητα [καταγραφής](https://msdn.microsoft.com/library/azure/mt592020.aspx) .

## <a name="configure-log-to-eventhubs-policies"></a>Ρύθμιση παραμέτρων πολιτικών καταγραφής-eventhubs

Όταν το πρόγραμμα καταγραφής έχει ρυθμιστεί API διαχείρισης, μπορείτε να ρυθμίσετε τις πολιτικές καταγραφής-eventhubs σας για να συνδεθείτε τα συμβάντα που θέλετε. Η πολιτική καταγραφής-eventhubs μπορεί να χρησιμοποιηθεί σε ενότητα εισερχομένων πολιτική ή στην ενότητα εξερχομένων πολιτικής.

Για να ρυθμίσετε τις πολιτικές, είσοδος στην [πύλη του Azure κλασική](https://manage.windowsazure.com), μεταβείτε της υπηρεσίας διαχείρισης API και, κάντε κλικ στην επιλογή **πύλη publisher** ή **Διαχείριση** για να αποκτήσετε πρόσβαση στη πύλη publisher.

![Πύλη του Publisher][publisher-portal]

Κάντε κλικ στην επιλογή **πολιτικές** στο μενού API διαχείρισης στα αριστερά, επιλέξτε την επιθυμητή προϊόντων και API και κάντε κλικ στην επιλογή **Προσθήκη πολιτικής**. Σε αυτό το παράδειγμα μας προσθέτετε μια πολιτική για το **API Αντήχηση** του προϊόντος **απεριόριστα** .

![Προσθήκη πολιτικής][add-policy]

Τοποθετήστε το δρομέα στο το `inbound` πολιτικής ενότητας και επιλέξτε το **αρχείο καταγραφής για να EventHub** πολιτικής για να εισαγάγετε το `log-to-eventhub` πρότυπο δήλωσης πολιτικής.

![Πρόγραμμα επεξεργασίας πολιτικής][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

Αντικατάσταση `logger-id` με το όνομα του στο πρόγραμμα καταγραφής API διαχείρισης που έχει ρυθμιστεί στο προηγούμενο βήμα.

Μπορείτε να χρησιμοποιήσετε οποιαδήποτε παράσταση που επιστρέφει μια συμβολοσειρά ως η τιμή για το `log-to-eventhub` στοιχείο. Σε αυτό το παράδειγμα, μια συμβολοσειρά που περιέχει την ημερομηνία και ώρα, όνομα υπηρεσίας, αναγνωριστικό αίτησης, αίτηση διεύθυνση ip και όνομα της λειτουργίας είναι συνδεδεμένος.

Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε τη ρύθμιση παραμέτρων της πολιτικής ενημερωμένο. Μόλις είναι αποθηκευμένη η πολιτική είναι ενεργή και συμβάντα καταγράφονται στο διανομέα που έχει οριστεί ως συμβάν.

## <a name="next-steps"></a>Επόμενα βήματα

-   Μάθετε περισσότερα σχετικά με διανομείς συμβάν Azure
    -   [Γρήγορα αποτελέσματα με το Azure διανομείς συμβάντος](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Λήψη μηνυμάτων με EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Οδηγός προγραμματισμού διανομείς συμβάντος](../event-hubs/event-hubs-programming-guide.md)
-   Μάθετε περισσότερα σχετικά με την ενοποίηση του API διαχείρισης και διανομείς συμβάντος
    -   [Αναφορά οντότητας καταγραφής](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [αναφορά αρχείου καταγραφής-eventhub πολιτικής](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Παρακολούθηση σας APIs με Azure API διαχείρισης, το συμβάν διανομείς και Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Παρακολουθήστε ένα βίντεο αναλυτικές οδηγίες

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png






