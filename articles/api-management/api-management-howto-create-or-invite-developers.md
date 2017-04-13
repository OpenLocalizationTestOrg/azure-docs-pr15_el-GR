<properties 
    pageTitle="Πώς να διαχειριστείτε τους λογαριασμούς χρηστών Azure API διαχείρισης | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε ή να προσκαλέσετε χρήστες Azure API διαχείρισης" 
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

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Πώς μπορείτε να διαχειριστείτε τους λογαριασμούς χρηστών Azure API διαχείρισης

API διαχείρισης, οι προγραμματιστές είναι οι χρήστες των API που εκθέτουν χρήση API διαχείρισης. Αυτός ο οδηγός εμφανίζει πώς να δημιουργήσετε και να προσκαλέσετε τους προγραμματιστές να χρησιμοποιήσετε το API και τα προϊόντα που κάνετε διαθέσιμη σε αυτές με την παρουσία API διαχείρισης. Για πληροφορίες σχετικά με τη Διαχείριση λογαριασμών χρηστών μέσω προγραμματισμού, ανατρέξτε στην τεκμηρίωση [οντότητα χρήστη](https://msdn.microsoft.com/library/azure/dn776330.aspx) στην αναφορά [REST API διαχείρισης](https://msdn.microsoft.com/library/azure/dn776326.aspx) .

## <a name="create-developer"> </a>Δημιουργήστε μια νέα για προγραμματιστές

Για να δημιουργήσετε μια νέα προγραμματιστής, κάντε κλικ στην επιλογή **Διαχείριση** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API. Ενέργεια αυτή σας μεταφέρει στην πύλη του publisher API διαχείρισης. Εάν δεν έχετε δημιουργήσει ακόμη μια παρουσία της υπηρεσίας διαχείρισης API, ανατρέξτε στο θέμα [Δημιουργία μια παρουσία της υπηρεσίας διαχείρισης API][] στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure API διαχείρισης][] .

![Πύλη του Publisher][api-management-management-console]

Κάντε κλικ στην επιλογή **χρήστες** από το **API διαχείρισης** μενού στα αριστερά και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.

![Δημιουργία για προγραμματιστές][api-management-create-developer]

Πληκτρολογήστε το **μήνυμα ηλεκτρονικού ταχυδρομείου**, **τον κωδικό πρόσβασης**και το **όνομα** για το νέο προγραμματιστής και κάντε κλικ στην επιλογή **Αποθήκευση**.

![Δημιουργία για προγραμματιστές][api-management-add-new-user]

Από προεπιλογή, οι λογαριασμοί που έχουν δημιουργηθεί πρόσφατα για προγραμματιστές είναι **ενεργή**και που σχετίζεται με την ομάδα **τους προγραμματιστές** .

![Νέα για προγραμματιστές][api-management-new-developer]

Λογαριασμοί για προγραμματιστές που βρίσκονται σε μια **ενεργή** κατάσταση μπορεί να χρησιμοποιηθεί για να αποκτήσετε πρόσβαση σε όλα τα API για τα οποία έχουν συνδρομές. Για να συσχετίσετε ο προγραμματιστής που μόλις δημιουργήθηκε με πρόσθετες ομάδες, δείτε [πώς μπορείτε να συσχετίσετε ομάδες με τους προγραμματιστές][].

## <a name="invite-developer"> </a>Πρόσκληση προγραμματιστής

Για να προσκαλέσετε προγραμματιστής, κάντε κλικ στην επιλογή **χρήστες** από το **API διαχείρισης** μενού στα αριστερά και, στη συνέχεια, κάντε κλικ στην επιλογή **Πρόσκληση χρήστη**.

![Πρόσκληση για προγραμματιστές][api-management-invite-developer]

Πληκτρολογήστε το όνομα διεύθυνση ηλεκτρονικού ταχυδρομείου από τον προγραμματιστή, και κάντε κλικ στην επιλογή **πρόσκληση**.

![Πρόσκληση για προγραμματιστές][api-management-invite-developer-window]

Εμφανίζεται ένα μήνυμα επιβεβαίωσης, αλλά ο προγραμματιστής που μόλις ο προσκεκλημένος δεν εμφανίζεται στη λίστα μέχρι αφού αποδεχτούν την πρόσκληση. 

![Πρόσκληση επιβεβαίωσης][api-management-invite-developer-confirmation]

Όταν ο προγραμματιστής προσκαλείται, ο προγραμματιστής αποστέλλεται ένα μήνυμα ηλεκτρονικού ταχυδρομείου. Το μήνυμα ηλεκτρονικού ταχυδρομείου θα δημιουργείται χρησιμοποιώντας ένα πρότυπο και έχει δυνατότητα προσαρμογής. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων πρότυπα ηλεκτρονικού ταχυδρομείου][].

Μόλις αποδεχτεί την πρόσκληση, ενεργοποιείται το λογαριασμό.

## <a name="block-developer"></a> Απενεργοποίηση ή επανενεργοποίηση ένα λογαριασμό για προγραμματιστές

Από προεπιλογή, οι λογαριασμοί που μόλις δημιουργήσατε ή ο προσκεκλημένος προγραμματιστής είναι **ενεργό**. Για να απενεργοποιήσετε ένα λογαριασμό προγραμματιστής, κάντε κλικ στην επιλογή **μπλοκ**. Για να ενεργοποιήσετε ξανά ένα λογαριασμό αποκλεισμένων προγραμματιστής, κάντε κλικ στην επιλογή **Ενεργοποίηση**. Ένα λογαριασμό αποκλεισμένων προγραμματιστής δεν πρόσβαση την πύλη για προγραμματιστές ή κλήση οποιαδήποτε APIs. Για να διαγράψετε ένα λογαριασμό χρήστη, κάντε κλικ στην επιλογή **Διαγραφή**.

![Μπλοκ για προγραμματιστές][api-management-new-developer]

## <a name="reset-a-user-password"></a>Επαναφορά κωδικού πρόσβασης ενός χρήστη

Για να επαναφέρετε τον κωδικό πρόσβασης για ένα λογαριασμό χρήστη, κάντε κλικ στο όνομα του λογαριασμού.

![Επαναφορά κωδικού πρόσβασης][api-management-view-developer]

Κάντε κλικ στην επιλογή **Επαναφορά κωδικού πρόσβασης** για να στείλετε μια σύνδεση προς το χρήστη να επαναφέρει τον κωδικό πρόσβασής τους.

![Επαναφορά κωδικού πρόσβασης][api-management-reset-password]

Για να εργαστείτε με προγραμματισμό με τους λογαριασμούς χρηστών, ανατρέξτε στην τεκμηρίωση [οντότητα χρήστη](https://msdn.microsoft.com/library/azure/dn776330.aspx) στην αναφορά [REST API διαχείρισης](https://msdn.microsoft.com/library/azure/dn776326.aspx) . Για να επαναφέρετε έναν κωδικό πρόσβασης του λογαριασμού χρήστη με μια συγκεκριμένη τιμή, μπορείτε να χρησιμοποιήσετε τη λειτουργία [ενημέρωση ενός χρήστη](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) και να καθορίσετε τον κωδικό πρόσβασης που θέλετε.

## <a name="pending-verification"></a>Σε εκκρεμότητα επαλήθευσης

![Σε εκκρεμότητα επαλήθευσης][api-management-pending-verification]

## <a name="next-steps"> </a>Επόμενα βήματα

Όταν δημιουργηθεί ένα λογαριασμό προγραμματιστή, μπορείτε να συσχετίσετε με τους ρόλους και την εγγραφή σε προϊόντα και API του Yammer. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε και να χρησιμοποιήσετε ομάδες][].


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Πώς μπορείτε να δημιουργήσετε και να χρησιμοποιήσετε ομάδες]: api-management-howto-create-groups.md
[Πώς μπορείτε να συσχετίσετε ομάδες με τους προγραμματιστές]: api-management-howto-create-groups.md#associate-group-developer

[Γρήγορα αποτελέσματα με το Azure API διαχείρισης]: api-management-get-started.md
[Δημιουργήστε μια παρουσία της υπηρεσίας διαχείρισης API]: api-management-get-started.md#create-service-instance
[Ρύθμιση παραμέτρων για πρότυπα ηλεκτρονικού ταχυδρομείου]: api-management-howto-configure-notifications.md#email-templates