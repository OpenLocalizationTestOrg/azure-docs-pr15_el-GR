<properties 
    pageTitle="Μετάδοση Bus υπηρεσίας δειγμάτων Επισκόπηση | Microsoft Azure"
    description="Κατηγοριοποιεί και περιγράφει τα δείγματα μεταγωγής Bus υπηρεσίας με συνδέσεις για κάθε."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-samples"></a>Δείγματα μεταγωγής Bus υπηρεσίας

Τα δείγματα μεταγωγής Bus υπηρεσίας παρουσιάζουν βασικές δυνατότητες του [μεταγωγής Bus υπηρεσίας](https://azure.microsoft.com/services/service-bus/). Σε αυτό το άρθρο κατηγοριοποιεί και περιγράφει τα δείγματα διαθέσιμη, με συνδέσεις σε καθένα.

>[AZURE.NOTE] Δείγματα Bus υπηρεσίας δεν είναι εγκατεστημένα με το SDK. Για να αποκτήσετε τα δείγματα, επισκεφθείτε τη [σελίδα δείγματα Azure SDK](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Επιπλέον, υπάρχει μια ενημερωμένη σύνολο δειγμάτων μεταγωγής Bus υπηρεσίας [εδώ](https://github.com/Azure-Samples/azure-servicebus-relay-samples) (όπως αυτής της εγγραφής, δεν περιγράφονται σε αυτό το άρθρο).  

Για δείγματα ανταλλαγής μηνυμάτων, ανατρέξτε στο θέμα [Bus υπηρεσία ανταλλαγής μηνυμάτων δείγματα](../service-bus-messaging/service-bus-samples.md).

## <a name="service-bus-relay"></a>Μετάδοση Bus υπηρεσίας

Στα παρακάτω παραδείγματα δείχνουν πώς να συντάξετε εφαρμογές που χρησιμοποιούν την υπηρεσία αναμετάδοσης Bus υπηρεσίας.

Σημειώστε ότι τα δείγματα μετάδοση απαιτεί μια συμβολοσειρά σύνδεσης για να αποκτήσετε πρόσβαση σας χώρο ονομάτων Bus υπηρεσίας.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Για να αποκτήσετε μια συμβολοσειρά σύνδεσης για Bus υπηρεσίας Azure

1. Συνδεθείτε στην [πύλη του Azure](http://portal.azure.com).

1. Στην αριστερή στήλη, κάντε κλικ στην επιλογή **Υπηρεσία Bus**.

1. Κάντε κλικ στο όνομα του πεδίου ονομάτων στη λίστα.

3. Στο blade το χώρο ονομάτων, κάντε κλικ στην επιλογή **πολιτικές πρόσβασης κοινή χρήση**.

4. Στο το blade **πολιτικές πρόσβασης κοινή χρήση** , κάντε κλικ στην επιλογή **RootManageSharedAccessKey**.

6. Αντιγράψτε τη συμβολοσειρά σύνδεσης στο Πρόχειρο.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Για να αποκτήσετε μια συμβολοσειρά σύνδεσης για την υπηρεσία Bus για Windows Server

1. Εκτελέστε το ακόλουθο cmdlet του PowerShell:

    ```
    get-sbClientConfiguration
    ```

2. Επικολλήστε τη συμβολοσειρά σύνδεσης στο αρχείο App.config για το δείγμα.

## <a name="service-bus-relay"></a>Μετάδοση Bus υπηρεσίας

Παραδείγματα που δείχνουν τη μετάδοση Bus υπηρεσίας.

### <a name="getting-started"></a>Γρήγορα αποτελέσματα

|Δείγμα ονόματος|Περιγραφή|Έκδοση ελάχιστη SDK|Διαθεσιμότητα|
|---|---|---|---|
|[Αναμετάδοση μηνυμάτων: Azure](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3)|Παρουσιάζει πώς μπορείτε να εκτελέσετε μια υπηρεσία Bus προγράμματος-πελάτη και υπηρεσίας στο Azure. Αυτό το δείγμα ρυθμίζει τις παραμέτρους Bus υπηρεσίας μέσω προγραμματισμού. Μόνο οι πληροφορίες περιβάλλοντος και την ασφάλεια αποθηκεύονται στα αρχεία ρύθμισης παραμέτρων.|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων έλεγχος ταυτότητας: Μυστικό κοινόχρηστο](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02)|Παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε ένα όνομα εκδότη και μυστικό εκδότη για τον έλεγχο ταυτότητας με Bus υπηρεσίας.|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων ελέγχου ταυτότητας: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831)|Παρουσιάζει τον τρόπο για να εμφανίσετε μια υπηρεσία HTTP που δεν απαιτεί έλεγχο ταυτότητας χρήστη του προγράμματος-πελάτη.|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων συνδέσεις: WebHttp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0)|Παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε τη σύνδεση **WebHttpRelayBinding** για να επιστρέψετε δυαδικά δεδομένα χρησιμοποιώντας το Web προγραμματισμού μοντέλο.|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων συνδέσεις: Αναμετάδοση NetTcp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692)|Παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε τη σύνδεση **NetTcpRelayBinding** .|1.8|Bus υπηρεσίας Microsoft Azure|

### <a name="exploring-features"></a>Εξερεύνηση δυνατοτήτων

Παραδείγματα που δείχνουν διάφορες δυνατότητες μεταγωγής Bus υπηρεσίας.

|Δείγμα ονόματος|Περιγραφή|Έκδοση ελάχιστη SDK|Διαθεσιμότητα|
|---|---|---|---|
|[Αναμετάδοση μηνυμάτων έλεγχος ταυτότητας: Απλή WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392)|Παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε μια απλή web διακριτικού διαπιστευτηρίων για τον έλεγχο ταυτότητας με Bus υπηρεσίας. Το δείγμα είναι παρόμοιο με το δείγμα αντήχηση, με ορισμένες αλλαγές. Συγκεκριμένα, αυτό το δείγμα προσθέτει μια συμπεριφορά στις εφαρμογές ChannelFactory (προγράμματος-πελάτη) και ServiceHost (υπηρεσία).|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων: Εξισορρόπησης φόρτου](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8)|Δείχνει πώς να χρησιμοποιείτε το Microsoft Azure Service Bus για τη δρομολόγηση μηνυμάτων σε πολλαπλούς δέκτες. Που εμφανίζει πολλές παρουσίες μιας απλής υπηρεσίας επικοινωνεί με ένα πρόγραμμα-πελάτη μέσω της σύνδεσης **NetTcpRelayBinding**|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων συνδέσεις: Καθαρή συμβάν](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977)|Παρουσιάζει τη χρήση της σύνδεσης **NetEventRelayBinding** στο Microsoft Azure Service Bus.|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων συνδέσεις: Περίοδος λειτουργίας WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb)|Παρουσιάζει τη χρήση της σύνδεσης **WS2007HttpRelayBinding** με αξιόπιστων περιόδων λειτουργίας με δυνατότητα. Εμφανίζει επίσης πώς μπορείτε να καθορίσετε τα διαπιστευτήρια Bus υπηρεσίας στο αρχείο ρύθμισης παραμέτρων αντί μέσω προγραμματισμού.|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων συνδέσεις: MsgSecCertificate WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5)|Παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε τη σύνδεση **WS2007HttpRelayBinding** με ασφάλεια μηνύματος για ασφαλή μηνύματα σε ολοκληρωμένες ενώ εξακολουθείτε να απαιτεί προγράμματα-πελάτες για τον έλεγχο ταυτότητας με Bus υπηρεσίας.|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων: Ανταλλαγής μετα-δεδομένων](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e)|Παρουσιάζει τον τρόπο για να εμφανίσετε ένα τελικό σημείο μετα-δεδομένων που χρησιμοποιεί τη σύνδεση μεταγωγή. **Ανταλλαγή μεταδεδομένων** υποστηρίζεται στο τις παρακάτω συνδέσεις μεταγωγής: **NetTcpRelayBinding**, **NetOnewayRelayBinding**, **BasicHttpRelayBinding**και **WS2007HttpRelayBinding**.|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων συνδέσεις: Άμεση NetTcp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161)|Παρουσιάζει πώς μπορείτε να ρυθμίσετε τις παραμέτρους της σύνδεσης **NetTcpRelayBinding** να υποστηρίζει την **υβριδική** λειτουργία σύνδεσης που δημιουργεί πρώτα μια relayed σύνδεση και, εάν είναι δυνατόν, αλλάζει αυτόματα σε μια απευθείας σύνδεση ανάμεσα σε ένα πρόγραμμα-πελάτη και μιας υπηρεσίας.|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων συνδέσεις: Όνομα χρήστη MsgSec NetTcp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392)|Παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε τη σύνδεση **NetTcpRelayBinding** με ασφάλεια μηνύματος.|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων συνδέσεις: Καθαρή Oneway](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a)|Παρουσιάζει πώς μπορείτε να εμφανίσετε και κατανάλωση ενός τελικού σημείου υπηρεσίας, χρησιμοποιώντας τη σύνδεση **NetOnewayRelayBinding** .|1.8|Bus υπηρεσίας Microsoft Azure|
|[Αναμετάδοση μηνυμάτων συνδέσεις: Απλή WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a)|Παρουσιάζει τη χρήση της σύνδεσης **WS2007HttpRelayBinding** . Αυτό δείχνει μια απλή υπηρεσία που χρησιμοποιεί δεν υπάρχουν επιλογές ασφαλείας και δεν απαιτεί προγράμματα-πελάτες για τον έλεγχο ταυτότητας.|1.8|Bus υπηρεσίας Microsoft Azure|

## <a name="next-steps"></a>Επόμενα βήματα

Ανατρέξτε στα παρακάτω θέματα για εννοιολογική Επισκόπηση Bus υπηρεσίας.

- [Επισκόπηση της υπηρεσίας Bus μεταγωγής](service-bus-relay-overview.md)
- [Αρχιτεκτονική Bus υπηρεσίας](../service-bus-messaging/service-bus-architecture.md)
- [Υπηρεσία Bus τα θεμελιώδη στοιχεία](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)