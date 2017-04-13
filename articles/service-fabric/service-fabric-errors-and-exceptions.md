<properties
   pageTitle="Κοινές FabricClient εξαιρέσεις που ανακύπτουν | Microsoft Azure"
   description="Περιγράφει τις κοινές εξαιρέσεις και σφάλματα που μπορεί να είναι δημιουργήθηκε από το API FabricClient κατά την εκτέλεση της εφαρμογής και λειτουργίες διαχείρισης συμπλεγμάτων."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Συνήθεις εξαιρέσεις και σφάλματα κατά την εργασία με τα API FabricClient
Τα API [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) επιτρέπουν στους διαχειριστές συμπλέγματος και την εφαρμογή για να εκτελέσετε εργασίες διαχείρισης σε μια εφαρμογή υπηρεσίας ύφασμα, υπηρεσίας ή σύμπλεγμα. Για παράδειγμα, ανάπτυξη εφαρμογών, αναβάθμιση και αφαίρεση, τον έλεγχο της εύρυθμης λειτουργίας ένα σύμπλεγμα ή δοκιμές υπηρεσίας. Τους προγραμματιστές εφαρμογών και οι διαχειριστές συμπλέγματος να χρησιμοποιήσετε τα API FabricClient για την ανάπτυξη εργαλεία για τη διαχείριση των ύφασμα υπηρεσιών συμπλέγματος και εφαρμογές.

Υπάρχουν πολλοί διαφορετικοί τύποι λειτουργιών για το οποίο μπορούν να εκτελεστούν χρησιμοποιώντας FabricClient.  Κάθε μέθοδο μπορεί να εμφανίσουν εξαιρέσεις για σφάλματα λόγω εσφαλμένες εισαγωγής, σφάλματα χρόνου εκτέλεσης ή μεταβατικές υποδομή θέματα.  Ανατρέξτε στην τεκμηρίωση αναφοράς API για να βρείτε ποιο εξαιρέσεις προκύπτουν από μια συγκεκριμένη μέθοδο. Υπάρχουν μερικές εξαιρέσεις, ωστόσο, που μπορεί να είναι δημιουργήθηκε από πολλές διαφορετικές API [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) . Ο παρακάτω πίνακας παραθέτει τις εξαιρέσεις που είναι κοινά στις τα API FabricClient.

|Εξαίρεση| Όταν δημιουργήθηκε|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|Το αντικείμενο [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) είναι σε κλειστή κατάσταση. Η διάταξη του αντικειμένου [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) που χρησιμοποιείτε και δημιουργία ενός νέου αντικειμένου [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) . |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|Η λειτουργία έληξε. [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) επιστρέφεται όταν η λειτουργία διαρκεί περισσότερο από MaxOperationTimeout για να ολοκληρωθεί.|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/en-us/library/system.unauthorizedaccessexception.aspx)|Ο έλεγχος πρόσβασης για τη λειτουργία απέτυχε. Επιστρέφεται E_ACCESSDENIED.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Παρουσιάστηκε ένα σφάλμα χρόνου εκτέλεσης κατά την εκτέλεση της λειτουργίας. Οποιαδήποτε από τις μεθόδους FabricClient ενδεχομένως μπορεί να εμφανίσουν [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx), η ιδιότητα [Κωδικός σφάλματος](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx) υποδεικνύει την ακριβή αιτία της εξαίρεσης. Κωδικοί σφάλματος ορίζονται στην απαρίθμηση [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) .|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|Η λειτουργία απέτυχε λόγω μια συνθήκη σφάλματος μεταβατικές κάποιο είδος. Για παράδειγμα, μια λειτουργία ενδέχεται να αποτύχει επειδή απαρτία αντίγραφα προσωρινά δεν είναι δυνατή η πρόσβαση. Μεταβατικές εξαιρέσεις αντιστοιχούν αποτυχίας λειτουργίες που μπορούν να επαναληφθούν.|

Ορισμένα κοινά [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) σφάλματα που μπορεί να επιστραφούν σε ένα [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx):

|Σφάλμα| Η συνθήκη|
|---------|:-----------|
|CommunicationError|Σφάλμα επικοινωνίας προκάλεσε τη λειτουργία για να αποτύχει, επαναλάβετε τη λειτουργία.|
|InvalidCredentialType|Τον τύπο των διαπιστευτηρίων δεν είναι έγκυρη.|
|InvalidX509FindType|Το X509FindType δεν είναι έγκυρη.|
|InvalidX509StoreLocation|Η X509 θέση αποθήκευσης δεν είναι έγκυρη.|
|InvalidX509StoreName|X509 το όνομα του χώρου αποθήκευσης δεν είναι έγκυρη.|
|InvalidX509Thumbprint|Η X509 συμβολοσειρά αποτύπωση πιστοποιητικού δεν είναι έγκυρη.|
|InvalidProtectionLevel|Το επίπεδο προστασίας δεν είναι έγκυρη.|
|InvalidX509Store|Το X509 χώρος αποθήκευσης πιστοποιητικού δεν μπορεί να ανοίξει.|
|InvalidSubjectName|Το όνομα θέματος δεν είναι έγκυρη.|
|InvalidAllowedCommonNameList|Η μορφή της συμβολοσειράς λίστα κοινές όνομα δεν είναι έγκυρη. Αυτό πρέπει να είναι μια λίστα διαχωρισμένες με κόμματα.|
