<properties
    pageTitle="Ρυθμίστε τις παραμέτρους ενός προσαρμοσμένου ονόματος τομέα για μια εφαρμογή web στην υπηρεσία εφαρμογής Azure που χρησιμοποιεί διαχείριση κίνηση για την εξισορρόπηση φόρτου."
    description="Χρήση προσαρμοσμένου ονόματος τομέα για μια μια εφαρμογή web στην υπηρεσία εφαρμογής Azure που περιλαμβάνει τη Διαχείριση κίνηση για την εξισορρόπηση φόρτου."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Ρύθμιση των παραμέτρων ενός προσαρμοσμένου ονόματος τομέα για μια εφαρμογή web σε Azure εφαρμογής υπηρεσίας, χρησιμοποιώντας τη Διαχείριση κίνηση

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Σε αυτό το άρθρο παρέχει γενικής χρήσης οδηγίες για τη χρήση προσαρμοσμένου ονόματος τομέα με Azure εφαρμογής υπηρεσίας που χρησιμοποιούν Manager κίνηση για την εξισορρόπηση φόρτου.

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## <a name="understanding-dns-records"></a>Κατανόηση των εγγραφών DNS

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## <a name="configure-your-web-apps-for-standard-mode"></a>Ρύθμιση των παραμέτρων σας web apps για τυπική λειτουργία

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## <a name="add-a-dns-record-for-your-custom-domain"></a>Προσθήκη μιας εγγραφής DNS για τον προσαρμοσμένο τομέα σας

> [AZURE.NOTE] Εάν έχετε αγοράσει τομέα μέσω του Azure εφαρμογής υπηρεσίας Web Apps, στη συνέχεια, παραλείψτε ακολουθώντας τα βήματα και αναφέρονται στο τελικό βήμα του άρθρου [Αγορά τομέα για τις εφαρμογές Web](custom-dns-web-site-buydomains-web-app.md) .

Για να συσχετίσετε τον προσαρμοσμένο τομέα σας με μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας, που πρέπει να προσθέσετε μια νέα καταχώρηση στον πίνακα DNS για τον προσαρμοσμένο τομέα σας, χρησιμοποιώντας εργαλεία που παρέχονται από το μητρώο καταχώρησης ονομάτων τομέων που έχετε αγοράσει το όνομα του τομέα σας από. Χρησιμοποιήστε τα ακόλουθα βήματα για να εντοπίσετε και να χρησιμοποιήσετε τα εργαλεία DNS.

1. Πραγματοποιήστε είσοδο στο λογαριασμό σας στο μητρώο καταχώρησης ονομάτων και αναζητήστε μια σελίδα για τη διαχείριση των εγγραφών DNS. Αναζητήστε συνδέσεις ή περιοχές της τοποθεσίας με την ετικέτα ως **Το όνομα του τομέα**, **DNS**ή **Το όνομα διακομιστή διαχείρισης**. Συχνά μπορείτε να βρείτε μια σύνδεση σε αυτήν τη σελίδα είναι προβολή πληροφοριών του λογαριασμού σας και, στη συνέχεια, να αναζητάτε μια σύνδεση όπως **τομείς μου**.

1. Αφού βρείτε τη σελίδα διαχείρισης για το όνομα του τομέα σας, αναζητήστε μια σύνδεση που σας επιτρέπει να επεξεργαστείτε τις εγγραφές DNS. Αυτό μπορεί να αναφέρεται ως **αρχείο ζώνης**, **Τις εγγραφές DNS**, ή ως μια σύνδεση ρύθμιση παραμέτρων **για προχωρημένους** .

    * Στη σελίδα πιθανότατα θα υπάρχει μερικές εγγραφές ήδη δημιουργήσει, όπως μια καταχώρηση συσχετίζοντας '**@**'ή'\*' με μια σελίδα 'τομέα στάθμευσης'. Μπορεί επίσης να περιέχει τις εγγραφές για κοινά δευτερεύοντες τομείς, όπως **www**.
    * Η σελίδα θα αναφέρετε **τις εγγραφές CNAME**ή παρέχουν μια αναπτυσσόμενη για να επιλέξετε έναν τύπο εγγραφής. Αυτό μπορεί να αναφέρετε επίσης άλλες εγγραφές, όπως **μια εγγραφές** και **εγγραφές MX**. Σε ορισμένες περιπτώσεις, θα ονομάζεται εγγραφές CNAME από άλλα ονόματα όπως μια **Alias Record**.
    * Η σελίδα θα έχει επίσης τα πεδία που σας επιτρέπουν να **χάρτη** από ένα **όνομα κεντρικού υπολογιστή** ή **το όνομα του τομέα** με άλλο όνομα τομέα.

1. Ενώ το λεπτομέρειες σχετικά με κάθε μητρώο καταχώρησης ονομάτων τομέων ποικίλλουν, γενικά αντιστοιχίζετε *από* το προσαρμοσμένο όνομα τομέα (όπως **contoso.com**,) *για* το όνομα τομέα Manager κίνηση (**contoso.trafficmanager.net**) που χρησιμοποιείται για την εφαρμογή web.

    > [AZURE.NOTE] Εναλλακτικά, εάν μια εγγραφή χρησιμοποιείται ήδη και πρέπει να δεσμεύσετε preemptively τις εφαρμογές σας σε αυτό, μπορείτε να δημιουργήσετε μια επιπλέον εγγραφή CNAME. Για παράδειγμα, για να συνδέσετε preemptively **www.contoso.com** σε εφαρμογή web, δημιουργήστε μια εγγραφή CNAME από **awverify.www** σε **contoso.trafficmanager.net**. Μπορείτε να προσθέσετε, στη συνέχεια, "www.contoso.com" σε εφαρμογή Web χωρίς να αλλάξετε την εγγραφή CNAME "www". Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία εγγραφών DNS για μια εφαρμογή web σε έναν προσαρμοσμένο τομέα][CREATEDNS].

1. Αφού ολοκληρώσετε την προσθήκη ή τροποποίηση εγγραφών DNS στο μητρώο, αποθηκεύστε τις αλλαγές.

<a name="enabledomain"></a>
## <a name="enable-traffic-manager"></a>Ενεργοποίηση διαχείρισης κίνηση

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές του Node.js](/develop/nodejs/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
