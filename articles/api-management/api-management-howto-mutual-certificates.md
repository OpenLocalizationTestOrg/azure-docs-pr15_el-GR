<properties 
    pageTitle="Πώς να διασφαλίζουν τις υπηρεσίες παρασκηνίου χρησιμοποιώντας πρόγραμμα-πελάτη πιστοποιητικό ελέγχου ταυτότητας Azure API διαχείρισης" 
    description="Μάθετε πώς να διασφαλίζουν τις υπηρεσίες παρασκηνίου χρησιμοποιώντας τον έλεγχο ταυτότητας πιστοποιητικού προγράμματος-πελάτη Azure API διαχείρισης." 
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

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Πώς να διασφαλίζουν τις υπηρεσίες παρασκηνίου χρησιμοποιώντας πρόγραμμα-πελάτη πιστοποιητικό ελέγχου ταυτότητας Azure API διαχείρισης

API διαχείρισης παρέχει τη δυνατότητα να εξασφάλισης της πρόσβασης στην υπηρεσία υποστήριξης του API χρησιμοποιώντας πιστοποιητικά προγράμματος-πελάτη. Αυτός ο οδηγός παρουσιάζει πώς μπορείτε να διαχειριστείτε τα πιστοποιητικά στην πύλη του publisher API και πώς μπορείτε να ρυθμίσετε τις παραμέτρους API για να χρησιμοποιήσετε ένα πιστοποιητικό για να αποκτήσετε πρόσβαση σε υπηρεσία παρασκηνίου.

Για πληροφορίες σχετικά με τη διαχείριση πιστοποιητικών χρησιμοποιώντας το API διαχείρισης REST API, ανατρέξτε στο θέμα [οντότητα Azure API διαχείρισης REST API του πιστοποιητικού][].

## <a name="prerequisites"> </a>Τις προϋποθέσεις

Αυτός ο οδηγός δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους του παρουσία της υπηρεσίας διαχείρισης API για να χρησιμοποιήσετε τον έλεγχο ταυτότητας πιστοποιητικού προγράμματος-πελάτη για πρόσβαση στην υπηρεσία υποστήριξης για το API. Πριν να ακολουθήσετε τα βήματα σε αυτό το θέμα, θα πρέπει να έχετε την υπηρεσία υποστήριξης που έχει ρυθμιστεί για έλεγχο ταυτότητας πιστοποιητικού προγράμματος-πελάτη ([για να ρυθμίσετε τις παραμέτρους πιστοποιητικό ελέγχου ταυτότητας σε Azure τοποθεσίες Web που αναφέρονται σε αυτό το άρθρο][]) και έχετε πρόσβαση στο το πιστοποιητικό και τον κωδικό πρόσβασης για το πιστοποιητικό για την αποστολή στην πύλη του publisher API διαχείρισης.

## <a name="step1"> </a>Αποστείλετε ένα πιστοποιητικό προγράμματος-πελάτη

Για να ξεκινήσετε, κάντε κλικ στην επιλογή **Διαχείριση** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API. Ενέργεια αυτή σας μεταφέρει στην πύλη του publisher API διαχείρισης.

![Πύλη API του Publisher][api-management-management-console]

>Εάν δεν έχετε δημιουργήσει ακόμη μια παρουσία της υπηρεσίας διαχείρισης API, ανατρέξτε στο θέμα [Δημιουργία μια παρουσία της υπηρεσίας διαχείρισης API][] στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure API διαχείρισης][] .

Κάντε κλικ στην επιλογή " **ασφάλεια** " από το μενού **API διαχείρισης** στην αριστερή πλευρά και κάντε κλικ στην επιλογή **πιστοποιητικά προγράμματος-πελάτη**.

![Πιστοποιητικά προγράμματος-πελάτη][api-management-security-client-certificates]

Για να αποστείλετε ένα νέο πιστοποιητικό, κάντε κλικ στην επιλογή **Αποστολή πιστοποιητικού**.

![Αποστολή πιστοποιητικού][api-management-upload-certificate]

Αναζητήστε το πιστοποιητικό και, στη συνέχεια, πληκτρολογήστε τον κωδικό πρόσβασης για το πιστοποιητικό.

>Το πιστοποιητικό πρέπει να είναι σε μορφή **.pfx** . Επιτρέπονται πιστοποιητικά αυτόματης υπογραφής.

![Αποστολή πιστοποιητικού][api-management-upload-certificate-form]

Κάντε κλικ στην επιλογή **Αποστολή** για να αποστείλετε το πιστοποιητικό.

>Ο κωδικός πρόσβασης πιστοποιητικό είναι επικύρωση αυτήν τη στιγμή. Εάν είναι εσφαλμένες εμφανίζεται ένα μήνυμα σφάλματος.

![Πιστοποιητικό που έχουν αποσταλεί][api-management-certificate-uploaded]

Όταν το πιστοποιητικό έχει αποσταλεί, εμφανίζεται στην καρτέλα **πιστοποιητικά προγράμματος-πελάτη** . Εάν έχετε πολλών πιστοποιητικών, κάντε μια σημείωση για το θέμα ή το τελευταίο τέσσερις χαρακτήρες του την αποτύπωση, που χρησιμοποιούνται για να επιλέξετε το πιστοποιητικό όταν ρυθμίζετε τις παραμέτρους API για να χρησιμοποιήσετε πιστοποιητικά, όπως καλύπτεται στην παρακάτω ενότητα [Ρύθμιση παραμέτρων API για να χρησιμοποιήσετε ένα πιστοποιητικό προγράμματος-πελάτη για έλεγχο ταυτότητας πύλης][] .

>Για να απενεργοποιήσετε την επικύρωση αλυσίδα πιστοποιητικού κατά τη χρήση, για παράδειγμα, ένα αυτο-υπογεγραμμένο πιστοποιητικό, ακολουθήστε τα βήματα που περιγράφονται σε αυτό συνήθεις Ερωτήσεις για το [στοιχείο](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).

## <a name="step1a"> </a>Διαγραφή ενός πιστοποιητικού προγράμματος-πελάτη

Για να διαγράψετε ένα πιστοποιητικό, κάντε κλικ στην επιλογή **Διαγραφή** δίπλα από το πιστοποιητικό που θέλετε.

![Διαγραφή πιστοποιητικού][api-management-certificate-delete]

Κάντε κλικ στην επιλογή **Ναι, διαγράψτε το** για να τον επιβεβαιώσετε.

![Επιβεβαίωση διαγραφής][api-management-confirm-delete]

Εάν το πιστοποιητικό που χρησιμοποιείται από ένα API, στη συνέχεια, εμφανίζεται μια προειδοποίηση οθόνη. Για να διαγράψετε το πιστοποιητικό πρέπει πρώτα να καταργήσετε το πιστοποιητικό από οποιαδήποτε API που έχουν ρυθμιστεί για να το χρησιμοποιήσετε.

![Επιβεβαίωση διαγραφής][api-management-confirm-delete-policy]

## <a name="step2"> </a>Ρύθμιση παραμέτρων API για να χρησιμοποιήσετε ένα πιστοποιητικό προγράμματος-πελάτη για έλεγχο ταυτότητας πύλης

Κάντε κλικ στην επιλογή **APIs** από το **API διαχείρισης** μενού στα αριστερά, κάντε κλικ στο όνομα του επιθυμητή API, και κάντε κλικ στην καρτέλα **ασφάλεια** .

![API ασφαλείας][api-management-api-security]

Επιλέξτε **πιστοποιητικά προγράμματος-πελάτη** από την αναπτυσσόμενη λίστα **με τα διαπιστευτήρια** .

![Πιστοποιητικά προγράμματος-πελάτη][api-management-mutual-certificates]

Επιλέξτε το πιστοποιητικό που θέλετε από την αναπτυσσόμενη λίστα **πιστοποιητικό προγράμματος-πελάτη** . Εάν υπάρχουν πολλά πιστοποιητικά που μπορεί να δείτε το θέμα ή των τελευταίων τεσσάρων χαρακτήρων από την αποτύπωση όπως σημειώνεται στην προηγούμενη ενότητα για να προσδιορίσετε το σωστό πιστοποιητικό.

![Επιλέξτε πιστοποιητικό][api-management-select-certificate]

Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε την αλλαγή της ρύθμισης παραμέτρων για το API.

>Αυτή η αλλαγή τίθεται σε ισχύ αμέσως και κλήσεις σε λειτουργίες του API που θα χρησιμοποιήσετε το πιστοποιητικό για τον έλεγχο ταυτότητας στο διακομιστή παρασκηνίου.

![Αποθήκευση αλλαγών API][api-management-save-api]

>Όταν ένα πιστοποιητικό έχει οριστεί για πύλης ελέγχου ταυτότητας για την υπηρεσία υποστήριξης του API, γίνεται μέρος της πολιτικής για το συγκεκριμένο API και μπορούν να προβληθούν στο πρόγραμμα επεξεργασίας πολιτικής.

![Πολιτική πιστοποιητικού][api-management-certificate-policy]

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με άλλους τρόπους για την ασφάλιση της υπηρεσίας παρασκηνίου, όπως HTTP βασικών ή κοινόχρηστο μυστικό τον έλεγχο ταυτότητας, ανατρέξτε στο θέμα βίντεο που ακολουθεί.

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Γρήγορα αποτελέσματα με το Azure API διαχείρισης]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Δημιουργήστε μια παρουσία της υπηρεσίας διαχείρισης API]: api-management-get-started.md#create-service-instance

[Azure οντότητα API διαχείρισης REST API πιστοποιητικού]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Για να ρυθμίσετε τις παραμέτρους πιστοποιητικό ελέγχου ταυτότητας σε Azure τοποθεσίες Web που αναφέρονται σε αυτό το άρθρο]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Ρύθμιση παραμέτρων API για να χρησιμοποιήσετε ένα πιστοποιητικό προγράμματος-πελάτη για έλεγχο ταυτότητας πύλης]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 
