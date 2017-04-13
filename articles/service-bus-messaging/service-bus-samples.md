<properties 
    pageTitle="Υπηρεσία Bus μηνυμάτων δειγμάτων Επισκόπηση | Microsoft Azure"
    description="Κατηγοριοποιεί και περιγράφει Bus υπηρεσία ανταλλαγής μηνυμάτων δείγματα με συνδέσεις σε καθένα."
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

# <a name="service-bus-messaging-samples"></a>Υπηρεσία Bus δείγματα ανταλλαγής μηνυμάτων

Τα δείγματα Bus υπηρεσία ανταλλαγής μηνυμάτων παρουσιάζουν βασικές δυνατότητες του [Bus υπηρεσία ανταλλαγής μηνυμάτων](https://azure.microsoft.com/services/service-bus/) (υπηρεσία cloud) και [Υπηρεσία Bus για Windows Server](https://msdn.microsoft.com/library/dn282144.aspx). Σε αυτό το άρθρο κατηγοριοποιεί και περιγράφει τα δείγματα διαθέσιμη, με συνδέσεις σε καθένα.

>[AZURE.NOTE] Δείγματα Bus υπηρεσίας δεν είναι εγκατεστημένα με το SDK. Για να αποκτήσετε τα δείγματα, επισκεφθείτε τη [σελίδα δείγματα Azure SDK](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Επιπλέον, υπάρχει μια ενημερωμένη σύνολο Bus υπηρεσία ανταλλαγής μηνυμάτων δείγματα [εδώ](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) (όπως αυτής της εγγραφής, δεν περιγράφονται σε αυτό το άρθρο).  

Για μετάδοση δείγματα, ανατρέξτε στο θέμα [δείγματα μεταγωγής Bus υπηρεσίας](../service-bus-relay/service-bus-relay-samples.md).

## <a name="service-bus-messaging"></a>Υπηρεσία Bus μηνυμάτων

Στα παρακάτω παραδείγματα δείχνουν πώς να συντάξετε εφαρμογές που χρησιμοποιούν Bus υπηρεσία ανταλλαγής μηνυμάτων.

Σημειώστε ότι τα μηνύματα δείγματα απαιτούν μια συμβολοσειρά σύνδεσης για να αποκτήσετε πρόσβαση σας χώρο ονομάτων Bus υπηρεσίας.

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

### <a name="getting-started-samples"></a>Γρήγορα αποτελέσματα δείγματα

Αυτά τα δείγματα περιγράφουν τη βασική λειτουργικότητα ανταλλαγής μηνυμάτων.

|Δείγμα ονόματος|Περιγραφή|Έκδοση ελάχιστη SDK|Διαθεσιμότητα|
|---|---|---|---|
|[Γρήγορα αποτελέσματα: Μηνυμάτων με ουρές](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3)|Δείχνει πώς να χρησιμοποιείτε το Microsoft Azure Service Bus για αποστολή και λήψη μηνυμάτων από μια ουρά.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Γρήγορα αποτελέσματα: Μηνυμάτων με θέματα](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5)|Δείχνει πώς να χρησιμοποιείτε το Microsoft Azure Service Bus για αποστολή και λήψη μηνυμάτων από ένα θέμα με πολλές συνδρομές.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|

### <a name="exploring-features"></a>Εξερεύνηση δυνατοτήτων

Στα παρακάτω παραδείγματα δείχνουν διάφορες δυνατότητες του Bus υπηρεσίας.

|Δείγμα ονόματος|Περιγραφή|Έκδοση ελάχιστη SDK|Διαθεσιμότητα|
|---|---|---|---|
|[Υπηρεσίες παροχής διακριτικό HTTP](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5)|Δείχνει το διαφορετικούς τρόπους τον έλεγχο ταυτότητας ενός προγράμματος-πελάτη HTTP/ΥΠΌΛΟΙΠΑ με Bus υπηρεσίας.|2.1|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Υπηρεσία Bus HTTP προγράμματος-πελάτη](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a)|Παρουσιάζει πώς μπορείτε να στείλετε ένα μήνυμα και να λαμβάνετε μηνύματα από Bus υπηρεσίας μέσω HTTP/ΥΠΌΛΟΙΠΟ.|2.3|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Υπηρεσία Bus Autoforwarding](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b)|Παρουσιάζει τον τρόπο για την Αυτόματη προώθηση μηνυμάτων από μια ουρά, συνδρομή ή ουρά αδρανούς αλληλογραφίας σε άλλη ουρά ή το θέμα. Δείχνει επίσης πώς μπορείτε να στείλετε ένα μήνυμα σε ουρά ή το θέμα μέσω μιας ουράς μεταφορά.|2.3|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Δείγμα περιόδου λειτουργίας WCF καναλιού](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451)|Δείχνει πώς να χρησιμοποιείτε το Microsoft Azure Service Bus με τη χρήση καναλιών Windows Communication Foundation (WCF). Το δείγμα εμφανίζει τη χρήση των καναλιών WCF για αποστολή και λήψη μηνυμάτων μέσω μιας Bus υπηρεσία ουράς. Το δείγμα εμφανίζει την περίοδο λειτουργίας και μη λειτουργίας επικοινωνίας πάνω από την υπηρεσία Bus.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: συναλλαγές](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e)|Δείχνει πώς να χρησιμοποιήσετε δυνατότητες μέσα σε ένα πεδίο συναλλαγής την ανταλλαγή μηνυμάτων του Microsoft Azure υπηρεσίας Bus για να βεβαιωθείτε ότι είναι δεσμευμένη δέσμες μηνυμάτων λειτουργίες atomically.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Λειτουργίες διαχείρισης χρησιμοποιώντας ΥΠΌΛΟΙΠΟ](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88)|Παρουσιάζει πώς μπορείτε να εκτελέσετε λειτουργίες διαχείρισης στην υπηρεσία Bus χρησιμοποιώντας ΥΠΌΛΟΙΠΟ.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Υπηρεσία παροχής πόρων APIs ΥΠΌΛΟΙΠΟ](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203)|Παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε τα νέα API ΥΠΌΛΟΙΠΟ RDFE Bus υπηρεσίας για να διαχειριστείτε πεδία ονομάτων και μηνυμάτων οντοτήτων.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Δείγμα περιόδου λειτουργίας υπηρεσίας WCF](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2)|Δείχνει πώς να χρησιμοποιείτε το Microsoft Azure Service Bus χρησιμοποιώντας το μοντέλο υπηρεσία WCF. Το δείγμα δείχνει τη χρήση του μοντέλου υπηρεσία WCF για την ολοκλήρωση επικοινωνία μέσω μια ουρά Bus υπηρεσία που βασίζεται σε περίοδο λειτουργίας.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Απόκριση αίτησης](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8)|Παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε το Microsoft Azure Service Bus και τη λειτουργικότητα αίτηση/απάντηση. Το δείγμα εμφανίζει απλές προγράμματα-πελάτες και διακομιστές επικοινωνία μέσω μιας Bus υπηρεσία ουράς.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: ουρά γράμμα νεκρού](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8)|Δείχνει πώς να χρησιμοποιείτε το Microsoft Azure Service Bus και τη λειτουργικότητα ανταλλαγής μηνυμάτων "νεκρού γράμμα ουρά". Το δείγμα εμφανίζει μια απλή αποστολέα και παραλήπτη επικοινωνία μέσω μιας Bus υπηρεσία ουράς.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Αναβάλει μηνυμάτων](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879)|Παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε τη δυνατότητα αναβολή μηνύματος του Microsoft Azure Service Bus. Το δείγμα εμφανίζει μια απλή αποστολέα και παραλήπτη επικοινωνία μέσω μιας Bus υπηρεσία ουράς.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Μηνύματα περιόδου λειτουργίας](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4)|Δείχνει πώς να χρησιμοποιείτε το Microsoft Azure Service Bus και τη λειτουργικότητα ανταλλαγής μηνυμάτων περιόδου λειτουργίας. Το δείγμα εμφανίζει απλές αποστολέων και παραληπτών επικοινωνία μέσω μιας Bus υπηρεσία ουράς.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Το θέμα αίτηση απόκρισης](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e)|Δείχνει πώς μπορείτε να υλοποιήσετε το μοτίβο αίτηση/απάντηση με χρήση του Microsoft Azure Service Bus θέματα και συνδρομών. Το δείγμα εμφανίζει απλές προγράμματα-πελάτες και διακομιστές επικοινωνία μέσω ενός θέματος Bus υπηρεσίας.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Ουρά απόκρισης αίτηση](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf)|Δείχνει πώς να χρησιμοποιείτε το Microsoft Azure Service Bus και τη λειτουργικότητα αίτηση/απάντηση. Το δείγμα εμφανίζει απλές προγράμματα-πελάτες και διακομιστές επικοινωνία μέσω δύο ουρές Bus υπηρεσίας.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Εντοπισμός διπλοτύπων](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25)|Παρουσιάζει πώς μπορείτε να χρησιμοποιείτε τον εντοπισμό διπλότυπων μήνυμα Microsoft Azure Service Bus με ουρές. Δημιουργεί δύο ουρές, μία με ενεργοποιημένο τον εντοπισμό διπλοτύπων και άλλες μία χωρίς διπλοτύπων.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Μηνυμάτων ασύγχρονης](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74)|Δείχνει πώς να χρησιμοποιείτε το Microsoft Azure Service Bus για αποστολή και λήψη μηνυμάτων ασύγχρονα από μια ουρά. Ουρά παρέχει οι αποσυνδεδεμένοι, ασύγχρονης επικοινωνία μεταξύ του αποστολέα και οποιονδήποτε αριθμό δέκτες (εδώ, ένα μεμονωμένο δέκτη).|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Σύνθετα φίλτρα](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)|Δείχνει πώς να χρησιμοποιείτε το Microsoft Azure Service Bus δημοσίευση/εγγραφή σύνθετα φίλτρα. Δημιουργεί ένα θέμα και 3 συνδρομές με διαφορετικό φίλτρο ορισμών, στέλνει μηνύματα στο θέμα και λαμβάνει όλα τα μηνύματα από συνδρομές.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Όπου μεσολαβούν μηνυμάτων: Προφόρτωσης μηνυμάτων](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d)|Παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε τη δυνατότητα Microsoft Azure Service Bus μηνύματα προφόρτωσης. Αυτό παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε τη δυνατότητα προφόρτωσης μηνύματα κατά την παραλαβή.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|

## <a name="service-bus-reference-tools"></a>Εργαλεία αναφοράς Bus υπηρεσίας

Στα παρακάτω παραδείγματα δείχνουν διάφορες άλλες δυνατότητες της υπηρεσίας.

|Δείγμα ονόματος|Περιγραφή|Έκδοση ελάχιστη SDK|Διαθεσιμότητα|
|---|---|---|---|
|[Εξερεύνηση Bus υπηρεσίας](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)|Η Εξερεύνηση Bus υπηρεσία επιτρέπει στους χρήστες για να συνδεθείτε σε ένα χώρο ονομάτων υπηρεσίας Bus υπηρεσίας και διαχείριση μηνυμάτων οντοτήτων με εύκολο τρόπο. Το εργαλείο παρέχει δυνατότητες για προχωρημένους όπως λειτουργίες εισαγωγής/εξαγωγής και τη δυνατότητα να ελέγξετε οντοτήτων ανταλλαγής μηνυμάτων και τις υπηρεσίες μεταγωγής.|1.8|Bus υπηρεσίας Microsoft Azure; Υπηρεσία Bus για τον Windows Server|
|[Εξουσιοδότηση: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93)|Αυτό το δείγμα παρουσιάζει τον τρόπο δημιουργίας και διαχείρισης των ταυτοτήτων υπηρεσίας στο Microsoft Azure Active Directory ελέγχου πρόσβασης (γνωστό και ως υπηρεσία ελέγχου πρόσβασης ή ACS) για χρήση με την υπηρεσία Bus.|Δ/Υ|Bus υπηρεσίας Microsoft Azure|

## <a name="next-steps"></a>Επόμενα βήματα

Ανατρέξτε στα παρακάτω θέματα για εννοιολογική Επισκόπηση Bus υπηρεσίας.

- [Επισκόπηση μηνυμάτων Bus υπηρεσίας](service-bus-messaging-overview.md)
- [Αρχιτεκτονική Bus υπηρεσίας](service-bus-architecture.md)
- [Υπηρεσία Bus τα θεμελιώδη στοιχεία](service-bus-fundamentals-hybrid-solutions.md)
