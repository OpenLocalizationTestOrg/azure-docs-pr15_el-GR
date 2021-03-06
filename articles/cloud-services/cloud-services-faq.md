<properties
    pageTitle="Συνήθεις Ερωτήσεις για υπηρεσίες cloud | Microsoft Azure"
    description="Συνήθεις ερωτήσεις σχετικά με τις υπηρεσίες Cloud."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>Συνήθεις Ερωτήσεις για τις υπηρεσίες cloud
Σε αυτό το άρθρο παρέχει απαντήσεις σε ορισμένες συνήθεις ερωτήσεις σχετικά με τις υπηρεσίες Cloud της Microsoft Azure. Μπορείτε επίσης να επισκεφθείτε [Συνήθεις ερωτήσεις για υποστήριξη Azure](http://go.microsoft.com/fwlink/?LinkID=185083) για γενικές πληροφορίες Azure τιμές και υποστήριξη. Μπορείτε επίσης να συμβουλευτείτε [Cloud Services Εικονική μέγεθος σελίδας](cloud-services-sizes-specs.md) για πληροφορίες μέγεθος.

## <a name="certificates"></a>Πιστοποιητικά

### <a name="where-should-i-install-my-certificate"></a>Πού πρέπει να εγκαταστήσω το πιστοποιητικό μου;

- **Μου**  
Εφαρμογή πιστοποιητικό με ιδιωτικό κλειδί (\*.pfx, \*.p12).

- **ΑΡΧΉ ΈΚΔΟΣΗΣ ΠΙΣΤΟΠΟΙΗΤΙΚΏΝ**  
Όλα τα πιστοποιητικά σας ενδιάμεσου μεταβείτε σε αυτόν το χώρο αποθήκευσης (πολιτικής και Sub CA).

- **ΡΊΖΑΣ**  
Η αρχή έκδοσης Πιστοποιητικών ρίζας αποθηκεύσετε, ώστε να σας κύριο ριζικό πιστοποιητικό CA θα πρέπει να μεταβείτε εδώ.

### <a name="i-cant-remove-expired-certificate"></a>Δεν μπορώ να καταργήσω πιστοποιητικό που έχει λήξει

Azure που αποτρέπει την κατάργηση πιστοποιητικού ενώ βρίσκεται σε χρήση. Πρέπει να είτε διαγραφή ανάπτυξης που χρησιμοποιεί το πιστοποιητικό, ή να ενημερώσετε την ανάπτυξη του με μια διαφορετική ή ανανεωθεί πιστοποιητικό.

### <a name="delete-an-expired-certificate"></a>Διαγράψτε ένα πιστοποιητικό που έχει λήξει

Με την προϋπόθεση ότι το πιστοποιητικό δεν είναι σε χρήση, μπορείτε να χρησιμοποιήσετε το cmdlet του PowerShell [AzureCertificate κατάργηση](https://msdn.microsoft.com/library/azure/mt589145.aspx) για να καταργήσετε ένα πιστοποιητικό.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Να έχει λήξει πιστοποιητικά που ονομάζεται διαχείριση της υπηρεσίας Windows Azure για επεκτάσεις

Αυτά τα πιστοποιητικά δημιουργούνται κάθε φορά που προστίθεται μια επέκταση την υπηρεσία cloud, όπως την επέκταση απομακρυσμένης επιφάνειας εργασίας. Αυτά τα πιστοποιητικά χρησιμοποιούνται μόνο για την κρυπτογράφηση και αποκρυπτογράφηση την προσωπική ρύθμιση παραμέτρων της επέκτασης. Δεν έχει σημασία αν λήξει αυτά τα πιστοποιητικά. Η ημερομηνία λήξης δεν είναι επιλεγμένο.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Τα πιστοποιητικά που έχετε διαγράψει να επανεμφανίζεται

Αυτά να επανεμφανίζεται πιθανώς εξαιτίας ενός εργαλείου που χρησιμοποιείτε, όπως το Visual Studio. Κάθε φορά που συνδέεστε ξανά με το εργαλείο που χρησιμοποιεί ένα πιστοποιητικό, θα να αποσταλεί ξανά σε Azure.

### <a name="my-certificates-keep-disappearing"></a>Διατήρηση να εξαφανίζονται τα πιστοποιητικά μου

Όταν η παρουσία εικονική μηχανή ανακυκλώνεται, όλες οι τοπικές αλλαγές θα χαθούν. Χρησιμοποιήστε μια [εργασία εκκίνησης](cloud-services-startup-tasks.md) για να εγκαταστήσετε τα πιστοποιητικά στην εικονική μηχανή της κάθε φορά που ξεκινά το ρόλο.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Δεν μπορώ να βρω τα πιστοποιητικά μου διαχείρισης στην πύλη

[Διαχείριση πιστοποιητικών](..\azure-api-management-certs.md) είναι διαθέσιμες στην πύλη κλασική Azure μόνο. Πύλη του τρέχοντος Azure δεν χρησιμοποιεί διαχείριση πιστοποιητικών. 

### <a name="how-can-i-disable-a-management-certificate"></a>Πώς μπορώ να απενεργοποιήσω ένα πιστοποιητικό διαχείρισης;

[Διαχείριση πιστοποιητικών](..\azure-api-management-certs.md) δεν μπορεί να απενεργοποιηθεί. Διαγράφετε τους μέσω της πύλης κλασική Azure όταν δεν θέλετε να χρησιμοποιηθεί πλέον.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Πώς μπορώ να δημιουργήσω ένα πιστοποιητικό SSL για μια συγκεκριμένη διεύθυνση IP;

Ακολουθήστε τις οδηγίες σε τη [Δημιουργία ένα πρόγραμμα εκμάθησης πιστοποιητικού](cloud-services-certs-create.md). Χρησιμοποιήστε τη διεύθυνση IP ως το όνομα DNS.

## <a name="security"></a>Ασφάλεια

### <a name="disable-ssl-30"></a>Απενεργοποίηση SSL 3.0

Για να απενεργοποιήσετε SSL 3.0 και να χρησιμοποιήσετε ασφάλεια TLS, δημιουργήστε μια εργασία εκκίνησης που τεκμηριώνονται σε αυτήν τη δημοσίευση ιστολογίου: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>Κλίμακα μια υπηρεσία στο cloud

### <a name="i-cannot-scale-beyond-x-instances"></a>Να δεν κλίμακα πέρα από το X παρουσίες

Τη συνδρομή σας στο Azure έχει όριο στον αριθμό των πυρήνων που μπορείτε να χρησιμοποιήσετε. Κλιμάκωση δεν λειτουργεί εάν έχετε χρησιμοποιήσει το πυρήνων που είναι διαθέσιμες. Για παράδειγμα, εάν έχετε ένα όριο 100 πυρήνων, αυτό σημαίνει ότι θα μπορούσε να έχετε 100 παρουσίες εικονική μηχανή A1 μεγέθους για την υπηρεσία cloud ή 50 A2 μεγέθους παρουσίες εικονική μηχανή.

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Δεν μπορώ να να δεσμεύσετε ένα IP σε μια υπηρεσία cloud πολλούς VIP

Πρώτα, βεβαιωθείτε ότι είναι ενεργοποιημένη η παρουσία εικονικό μηχάνημα που προσπαθείτε να δεσμεύσετε το IP για το. Δεύτερον, βεβαιωθείτε ότι χρησιμοποιείτε δεσμευμένες διευθύνσεις IP για σας ενοχλήσει ξανά το αναπτύξεις δημιουργίας σταδίων και παραγωγής. **Δεν πρέπει** να αλλάξετε τις ρυθμίσεις κατά την αναβάθμιση της ανάπτυξης.

