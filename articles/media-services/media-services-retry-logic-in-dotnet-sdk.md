<properties
    pageTitle="Επανάληψη λογικής στο SDK υπηρεσίες πολυμέσων για .NET | Microsoft Azure"
    description="Το θέμα παρέχει μια επισκόπηση της λογικής "Επανάληψη" στο SDK υπηρεσίες πολυμέσων για .NET."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>


# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Επανάληψη λογικής στο SDK υπηρεσίες πολυμέσων για .NET

Όταν εργάζεστε με τις υπηρεσίες του Microsoft Azure, μεταβατικές σφάλματα μπορεί να προκύψουν. Εάν παρουσιάζεται ένα σφάλμα μεταβατικές, στις περισσότερες περιπτώσεις, μετά από μερικά επαναλήψεις η λειτουργία ολοκληρωθεί με επιτυχία. Το SDK υπηρεσίες πολυμέσων για .NET υλοποιεί της λογικής "Επανάληψη" για το χειρισμό μεταβατικές σφαλμάτων που σχετίζονται με εξαιρέσεις και σφάλματα που προκαλούνται από αιτήσεων web, εκτέλεση ερωτημάτων, αποθήκευση αλλαγών και εργασίες αποθήκευσης.  Από προεπιλογή, το SDK υπηρεσίες πολυμέσων για .NET εκτελεί τέσσερις επαναλήψεις πριν από την εκ νέου αποστελλόμενο την εξαίρεση στην εφαρμογή σας. Ο κώδικας στην εφαρμογή σας, στη συνέχεια, πρέπει να χειρίζεται η εξαίρεση αυτή σωστά.  
  
 Ακολουθεί μια σύντομη κατευθυντήριας γραμμής αίτησης Web, χώρος αποθήκευσης, ερωτήματος και SaveChanges πολιτικών:  
  
-   Η πολιτική αποθήκευσης χρησιμοποιείται για λειτουργίες χώρο αποθήκευσης αντικειμένων blob (αποστολές ή λήψη αρχείων περιουσιακών στοιχείων).  
  
-   Η πολιτική αίτησης Web χρησιμοποιείται για αιτήσεις γενικής χρήσης web (για παράδειγμα, για γρήγορα ένα διακριτικό έλεγχο ταυτότητας και επίλυση το τελικό σημείο σύμπλεγμα χρήστες).  
  
-   Η πολιτική ερωτήματος χρησιμοποιείται για την υποβολή ερωτημάτων οντοτήτων από ΥΠΌΛΟΙΠΑ (για παράδειγμα, mediaContext.Assets.Where(...)).  
  
-   Η πολιτική SaveChanges χρησιμοποιείται για όλα τα στοιχεία που αλλάζει δεδομένα κατά την υπηρεσία (για παράδειγμα, δημιουργώντας μια οντότητα ενημέρωση μια οντότητα, κλήση μιας λειτουργίας υπηρεσίας για μια λειτουργία) κάνοντας.  
  
 Αυτό το θέμα παραθέτει τους τύπους εξαίρεσης και τους κωδικούς σφάλματος που αντιμετωπίζονται από το Media Services SDK για .NET της λογικής επανάληψης.  
  
## <a name="exception-types"></a>Τύποι εξαίρεσης  

Ο παρακάτω πίνακας περιγράφει τις εξαιρέσεις που το SDK υπηρεσίες πολυμέσων για .NET χειρίζεται ή δεν χειρίζεται για κάποιες εργασίες που μπορεί να προκαλέσει μεταβατικές.  
  
Εξαίρεση|Αίτηση Web|Χώρος αποθήκευσης|Ερώτημα|SaveChanges
----|------|----|---|---
WebException<br/>Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [WebException κωδικοί κατάστασης](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) .|Ναι|Ναι|Ναι|Ναι  
DataServiceClientException<br/> Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κωδικοί κατάστασης σφάλματος HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Όχι|Ναι|Ναι|Ναι
DataServiceQueryException<br/> Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κωδικοί κατάστασης σφάλματος HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Όχι|Ναι|Ναι|Ναι  
DataServiceRequestException<br/> Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κωδικοί κατάστασης σφάλματος HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Όχι|Ναι|Ναι|Ναι  
DataServiceTransportException|Όχι|Όχι|Ναι|Ναι
TimeoutException|Ναι|Ναι|Ναι|Όχι
SocketException|Ναι|Ναι|Ναι|Ναι  
StorageException|Όχι|Ναι|Όχι|Όχι 
IOException|Όχι|Ναι|Όχι|Όχι
  
###  <a name="WebExceptionStatus"></a>Κωδικοί κατάστασης WebException  

Ο παρακάτω πίνακας εμφανίζει για το ποιες τους κωδικούς σφάλματος WebException εφαρμόζεται η λογική "Επανάληψη". Η απαρίθμηση [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) καθορίζει τους κωδικούς κατάστασης.  
  
Κατάσταση|Αίτηση Web|Χώρος αποθήκευσης|Ερώτημα|SaveChanges  
-----|-----------------|-------------|-----------|----------  
ConnectFailure|Ναι|Ναι|Ναι|Ναι
NameResolutionFailure|Ναι|Ναι|Ναι|Ναι  
ProxyNameResolutionFailure|Ναι|Ναι|Ναι|Ναι  
SendFailure|Ναι|Ναι|Ναι|Ναι
PipelineFailure|Ναι|Ναι|Ναι|Όχι  
ConnectionClosed|Ναι|Ναι|Ναι|Όχι  
KeepAliveFailure|Ναι|Ναι|Ναι|Όχι  
UnknownError|Ναι|Ναι|Ναι|Όχι 
ReceiveFailure|Ναι|Ναι|Ναι|Όχι  
RequestCanceled|Ναι|Ναι|Ναι|Όχι  
Λήξη χρονικού ορίου|Ναι|Ναι|Ναι|Όχι
Σφάλμα <br/>Επανάληψη στο σφάλμα ελέγχεται από τη Διαχείριση κωδικός κατάστασης HTTP. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κωδικοί κατάστασης σφάλματος HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ναι|Ναι|Ναι|Ναι|  
  
###  <a name="HTTPStatusCode"></a>Κωδικοί κατάστασης σφάλματος HTTP  

Όταν λειτουργίες ερωτήματος και SaveChanges εμφανίσουν DataServiceClientException, DataServiceQueryException ή DataServiceQueryException, επιστρέφεται ο κωδικός κατάστασης σφάλματος HTTP στην ιδιότητα StatusCode.  Ο παρακάτω πίνακας εμφανίζει για το ποια κωδικούς σφαλμάτων εφαρμόζεται η λογική "Επανάληψη".  
  
 
Κατάσταση|Αίτηση Web|Χώρος αποθήκευσης|Ερώτημα|SaveChanges 
---|----|----|----|----
401|Όχι|Ναι|Όχι|Όχι
403|Όχι|Ναι<br/>Χειρισμός επαναλήψεων με μεγαλύτερες αναμονή.|Όχι|Όχι  
408|Ναι|Ναι|Ναι|Ναι
429|Ναι|Ναι|Ναι|Ναι  
500|Ναι|Ναι|Ναι|Όχι  
502|Ναι|Ναι|Ναι|Όχι  
503|Ναι|Ναι|Ναι|Ναι  
504|Ναι|Ναι|Ναι|Όχι  
  
Εάν θέλετε να μεταφέρετε μια ματιά την πραγματική εφαρμογή του SDK υπηρεσίες πολυμέσων για .NET λογικής επανάληψης, ανατρέξτε στο θέμα [azure sdk για media services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
