<properties
   pageTitle="Ρύθμιση παραμέτρων σε μια υπάρχουσα εφαρμογή πύλης για τη φιλοξενία πολλών τοποθεσιών στην πύλη του Azure | Microsoft Azure"
   description="Αυτή η σελίδα παρέχει οδηγίες για να ρυθμίσετε τις παραμέτρους μιας υπάρχουσας πύλη Azure εφαρμογής για τη φιλοξενία πολλές εφαρμογές web στην ίδια πύλη με την πύλη του Azure."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Ρύθμιση παραμέτρων σε μια υπάρχουσα πύλη εφαρμογής για τη φιλοξενία πολλών εφαρμογών web

> [AZURE.SELECTOR]
- [Πύλη του Azure](application-gateway-create-multisite-portal.md)
- [Azure του PowerShell για τη διαχείριση πόρων](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Πολλές φιλοξενίας τοποθεσίας σάς επιτρέπει να αναπτύξετε περισσότερες από μία εφαρμογής web στην ίδια πύλη εφαρμογής. Βασίζεται σε παρουσία της κεφαλίδας κεντρικού υπολογιστή στην εισερχόμενη αίτηση HTTP, για να προσδιορίσετε ποια ακρόασης θα λαμβάνει την κυκλοφορία. Το ακροατήριο, στη συνέχεια, κατευθύνει την κυκλοφορία στο κατάλληλο παρασκηνίου χώρου συγκέντρωσης όπως έχει ρυθμιστεί στον ορισμό κανόνες της πύλης. Στις εφαρμογές web δυνατότητα SSL, πύλη εφαρμογής εξαρτάται από την επέκταση ένδειξη όνομα διακομιστή (SNI) για να επιλέξετε τη σωστή λειτουργία ακρόασης για την κυκλοφορία web. Μια κοινή χρήση για τη φιλοξενία πολλών τοποθεσιών είναι η εξισορρόπηση αιτήσεων για τους τομείς διαφορετικό web σε διαφορετικό διακομιστή παρασκηνίου χώρους συγκέντρωσης. Ομοίως πολλούς δευτερεύοντες τομείς του ίδιου ριζικό τομέα μπορεί επίσης, να φιλοξενηθούν στην ίδια πύλη εφαρμογής.

## <a name="scenario"></a>Σενάριο

Στο παρακάτω παράδειγμα, πύλη εφαρμογής σερβίρισμα κίνηση για contoso.com και fabrikam.com με δύο σύνολα διακομιστή υποστήριξης: contoso σύνολο διακομιστή και σύνολο διακομιστή fabrikam. Παρόμοια ρύθμισης μπορεί να χρησιμοποιηθεί για host δευτερεύοντες τομείς όπως app.contoso.com και blog.contoso.com.

![σενάριο του][multisite]

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Αυτό το σενάριο προσθέτει υποστήριξη πολλών τοποθεσίας σε μια υπάρχουσα πύλη εφαρμογής. Για να ολοκληρώσετε αυτό το σενάριο σενάριο μια υπάρχουσα πύλη εφαρμογής πρέπει να είναι διαθέσιμος για να ρυθμίσετε τις παραμέτρους. Επισκεφθείτε [Δημιουργία μια πύλη εφαρμογής, χρησιμοποιώντας την πύλη](./application-gateway-create-gateway-portal.md) για να μάθετε πώς μπορείτε να δημιουργήσετε μια βασική εφαρμογή πύλης στην πύλη.

## <a name="requirements"></a>Απαιτήσεις

- **Σύνολο διακομιστή παρασκηνίου:** Η λίστα διευθύνσεων IP των διακομιστών παρασκηνίου. Οι διευθύνσεις IP που παρατίθενται θα πρέπει να ανήκουν είτε στο υποδίκτυο εικονικού δικτύου ή πρέπει να είναι μια δημόσια IP/VIP. Μπορεί επίσης να χρησιμοποιηθεί FQDN.
- **Ρυθμίσεων χώρου συγκέντρωσης διακομιστή παρασκηνίου:** Κάθε χώρος συγκέντρωσης έχει ρυθμίσεις όπως θύρα, το πρωτόκολλο και βασίζονται σε cookie συσχέτισης. Αυτές οι ρυθμίσεις είναι συνδεδεμένη με ένα χώρο συγκέντρωσης και εφαρμόζονται σε όλους τους διακομιστές εντός του χώρου συγκέντρωσης.
- **Προσκηνίου θύρας:** Αυτήν τη θύρα είναι η δημόσια θύρα που είναι δυνατό το άνοιγμα της εφαρμογής πύλης. Κίνηση επισκέψεις αυτήν τη θύρα και, στη συνέχεια, γίνεται ανακατεύθυνση σε έναν από τους διακομιστές παρασκηνίου.
- **Ακρόασης:** Το ακροατήριο διαθέτει προσκηνίου θύρα, ένα πρωτόκολλο (Http ή Https, αυτές οι τιμές διάκριση πεζών-κεφαλαίων), και το όνομα του πιστοποιητικού SSL (εάν τη ρύθμιση των παραμέτρων SSL μείωση φόρτου). Για πύλες εφαρμογής με δυνατότητα πολλών τοποθεσίας, όνομα κεντρικού υπολογιστή και δείκτες SNI προστίθενται.
- **Κανόνα:** Ο κανόνας συνδέει το ακροατήριο, το χώρο συγκέντρωσης server παρασκηνίου, και καθορίζει ποιο σύνολο διακομιστή παρασκηνίου την κυκλοφορία πρέπει να απευθύνονται όταν το επισκέψεις μιας συγκεκριμένης υπηρεσίας ακρόασης.
- **Πιστοποιητικά:** Κάθε πρόγραμμα ακρόασης απαιτεί ένα μοναδικό πιστοποιητικό, σε αυτό το παράδειγμα 2 ακροατών δημιουργούνται για πολλές τοποθεσίες. Δύο .pfx πιστοποιητικά και τους κωδικούς πρόσβασης για τους πρέπει να δημιουργηθούν.

## <a name="create-an-application-gateway"></a>Δημιουργήστε μια πύλη εφαρμογής

Ακολουθούν τα βήματα που απαιτούνται για να ενημερώσετε την πύλη εφαρμογής:

1. Δημιουργήστε σύνολα παρασκηνίου για να χρησιμοποιήσετε για κάθε τοποθεσία.
2. Δημιουργήστε ένα νέο πρόγραμμα ακρόασης για κάθε πύλη εφαρμογής τοποθεσίας θα υποστηρίζει.
3. Δημιουργία κανόνων για να αντιστοιχίσετε κάθε ακρόασης με την κατάλληλη παρασκηνίου.

## <a name="create-back-end-pools-for-each-site"></a>Δημιουργήστε σύνολα παρασκηνίου για κάθε τοποθεσία

Θα συγκεκριμένη πύλη εφαρμογής ενός χώρου συγκέντρωσης παρασκηνίου για κάθε τοποθεσία υποστήριξης είναι απαραίτητο, σε αυτήν την περίπτωση 2 θα δημιουργηθεί, μία για contoso11.com και μία για fabrikam11.com.

### <a name="step-1"></a>Βήμα 1

Μεταβείτε σε μια υπάρχουσα πύλη εφαρμογής στην πύλη του Azure (https://portal.azure.com). Επιλέξτε **παρασκηνίου χώρους συγκέντρωσης** και κάντε κλικ στην επιλογή **Προσθήκη**

![Προσθήκη χώρους συγκέντρωσης παρασκηνίου][7]

### <a name="step-2"></a>Βήμα 2

Συμπληρώστε τις πληροφορίες για το χώρο συγκέντρωσης παρασκηνίου **pool1**, προσθέτοντας τις διευθύνσεις ip ή το FQDN για τους διακομιστές παρασκηνίου και κάντε κλικ στο **κουμπί OK**

![ρυθμίσεις pool1 χώρου συγκέντρωσης παρασκηνίου][8]

### <a name="step-3"></a>Βήμα 3

Στην το blade παρασκηνίου σύνολα, κάντε κλικ στην επιλογή **Προσθήκη** για να προσθέσετε μια επιπλέον χώρου συγκέντρωσης παρασκηνίου **pool2**, προσθέτοντας την διευθύνσεις ip ή τα FQDN για τους διακομιστές παρασκηνίου και κάντε κλικ στο **κουμπί OK**

![ρυθμίσεις pool2 χώρου συγκέντρωσης παρασκηνίου][9]

### <a name="create-listeners-for-each-back-end"></a>Δημιουργία ακροατών για κάθε παρασκηνίου

Πύλη εφαρμογής βασίζεται σε HTTP 1.1 κεφαλίδων κεντρικού υπολογιστή για τη φιλοξενία περισσότερες από μία τοποθεσία Web στην ίδια δημόσια διεύθυνση IP και θύρες. Η βασική ακρόαση έχουν δημιουργηθεί με την πύλη δεν περιέχει αυτήν την ιδιότητα.

### <a name="step-1"></a>Βήμα 1

Κάντε κλικ στην επιλογή **ακροατών** της πύλης υπάρχουσα εφαρμογή και κάντε κλικ στην **πολλαπλή τοποθεσίας** για να προσθέσετε την πρώτη παρακολούθηση.

![ακροατών Επισκόπηση blade][1]

### <a name="step-2"></a>Βήμα 2

Συμπληρώστε τις πληροφορίες για το ακροατήριο, σε αυτό το παράδειγμα τερματισμού SSL έχει ρυθμιστεί, δημιουργήστε μια νέα θύρα frontend. Αποστείλετε το πιστοποιητικό .pfx μπορούν να χρησιμοποιηθούν για τερματισμό SSL. Η διαφορά μόνο σε αυτό blade σε σύγκριση με την τυπική βασικές ακρόασης blade είναι το όνομα κεντρικού υπολογιστή.

![ακρόαση ιδιότητες blade][2]

### <a name="step-3"></a>Βήμα 3

Κάντε κλικ στην επιλογή **πολλών τοποθεσιών** και δημιουργήστε μια άλλη ακρόασης όπως περιγράφεται στο προηγούμενο βήμα για τη δεύτερη τοποθεσία. Φροντίστε να χρησιμοποιήσετε ένα διαφορετικό πιστοποιητικό το δεύτερο ακροατήριο. Η διαφορά μόνο σε αυτό blade σε σύγκριση με την τυπική βασικές ακρόασης blade είναι το όνομα κεντρικού υπολογιστή. Συμπληρώστε τις πληροφορίες για το ακροατήριο και κάντε κλικ στο **κουμπί OK**.

![ακρόαση ιδιότητες blade][3]

> [AZURE.NOTE] Δημιουργία ακροατών στην πύλη του Azure για πύλη εφαρμογής είναι μια εργασία που εκτελείται χρόνο, ενδέχεται να χρειαστεί κάποιος χρόνος για να δημιουργήσετε τα δύο ακροατών σε αυτό το σενάριο. Όταν ολοκληρώσετε την προβολή ακροατών στην πύλη του, όπως φαίνεται στην παρακάτω εικόνα.

![Επισκόπηση παρακολούθησης][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>Δημιουργία κανόνων για να αντιστοιχίσετε ακροατών σε χώρους συγκέντρωσης παρασκηνίου

### <a name="step-1"></a>Βήμα 1

Μεταβείτε σε μια υπάρχουσα πύλη εφαρμογής στην πύλη του Azure (https://portal.azure.com). Επιλέξτε **κανόνες** , επιλέξτε το υπάρχον κανόνα προεπιλεγμένη **rule1** και κάντε κλικ στην επιλογή **Επεξεργασία**.

### <a name="step-2"></a>Βήμα 2

Συμπληρώστε την blade κανόνες, όπως φαίνεται στην παρακάτω εικόνα. Επιλέγοντας το πρώτο ακρόασης και πρώτης χώρο συγκέντρωσης και κάνοντας κλικ στην επιλογή **Αποθήκευση** όταν ολοκληρωθεί η εγκατάσταση.

![επεξεργαστείτε υπάρχοντα κανόνα][6]

### <a name="step-3"></a>Βήμα 3

Κάντε κλικ στην επιλογή **βασικό κανόνα** για να δημιουργήσετε τον δεύτερο κανόνα. Συμπληρώστε τη φόρμα με το δεύτερο ακρόασης και δεύτερη παρασκηνίου χώρο συγκέντρωσης και κάντε κλικ στο **κουμπί OK** για να αποθηκεύσετε.

![Προσθήκη blade βασικός κανόνας][10]

Αυτό ολοκληρώνει τη ρύθμιση των παραμέτρων μιας υπάρχουσας πύλη εφαρμογής με την υποστήριξη πολλαπλών τοποθεσιών μέσω της πύλης Azure.

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε πώς να προστατεύσετε τις τοποθεσίες Web με την [Πύλη εφαρμογής - τείχος προστασίας εφαρμογής Web](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png