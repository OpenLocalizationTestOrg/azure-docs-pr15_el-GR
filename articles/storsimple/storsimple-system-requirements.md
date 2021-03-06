<properties
   pageTitle="Απαιτήσεις συστήματος για το StorSimple | Microsoft Azure"
   description="Περιγράφει το λογισμικό, κοινωνικής δικτύωσης, και απαιτήσεις υψηλής διαθεσιμότητας και βέλτιστες πρακτικές για μια λύση Microsoft Azure StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/31/2016"
   ms.author="alkohli"/>

# <a name="storsimple-software-high-availability-and-networking-requirements"></a>Λογισμικό StorSimple, υψηλή διαθεσιμότητα και κοινωνικής δικτύωσης απαιτήσεις

## <a name="overview"></a>Επισκόπηση

Καλώς ορίσατε στο Microsoft Azure StorSimple. Σε αυτό το άρθρο περιγράφει σημαντική σύστημα απαιτήσεις και βέλτιστες πρακτικές για τη συσκευή σας StorSimple και για τα προγράμματα-πελάτες χώρου αποθήκευσης που έχει πρόσβαση στη συσκευή. Συνιστάται να εξετάσετε τις πληροφορίες προσεκτικά πριν από την ανάπτυξη του συστήματος StorSimple και, στη συνέχεια, παραπέμπουν σε αυτό είναι απαραίτητο κατά τη διάρκεια της ανάπτυξης και οι επόμενες λειτουργίας.

Οι απαιτήσεις συστήματος περιλαμβάνουν τα εξής:

- **Απαιτήσεις λογισμικού για προγράμματα-πελάτες του χώρου αποθήκευσης** - περιγράφει τα υποστηριζόμενα λειτουργικά συστήματα και τυχόν πρόσθετες απαιτήσεις για αυτά τα λειτουργικά συστήματα.
- **Απαιτήσεις για τη συσκευή StorSimple δικτύου** - παρέχει πληροφορίες σχετικά με τις θύρες που πρέπει να είναι ανοιχτές στο τείχος προστασίας σας για να επιτρέψετε για iSCSI, cloud ή διαχείριση κυκλοφορίας.
- **Απαιτήσεις υψηλής διαθεσιμότητας για StorSimple** - περιγράφει τις απαιτήσεις υψηλής διαθεσιμότητας και βέλτιστες πρακτικές για τον StorSimple συσκευή και κεντρικού υπολογιστή. 


## <a name="software-requirements-for-storage-clients"></a>Απαιτήσεις λογισμικού για προγράμματα-πελάτες του χώρου αποθήκευσης

Είναι τις ακόλουθες απαιτήσεις λογισμικού για τα προγράμματα-πελάτες χώρου αποθήκευσης που σας συσκευής StorSimple πρόσβασης.

| Υποστηριζόμενα λειτουργικά συστήματα | Απαιτούμενη έκδοση | Πρόσθετες απαιτήσεις/σημειώσεις |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008R2 SP1, 2012, 2012R2 |StorSimple iSCSI όγκους υποστηρίζονται για χρήση σε μόνο οι παρακάτω τύποι δίσκου των Windows:<ul><li>Απλή τόμου σε βασικό δίσκο</li><li>Απλό και αντικριστά τόμου σε δυναμικό δίσκο</li></ul>Λεπτό προμήθεια του Windows Server 2012 και ODX δυνατότητες που υποστηρίζονται Εάν χρησιμοποιείτε μια ένταση iSCSI StorSimple.<br><br>Να δημιουργήσετε StorSimple αραιά προμήθεια του φακέλου και πλήρως προμήθεια του φακέλου όγκους. Δεν μπορεί να δημιουργήσει όγκους μερικώς προμήθεια του φακέλου.<br><br>Επανάληψη της μορφοποίησης ενός αραιά προμήθεια του φακέλου τόμου ενδέχεται να χρειαστούν μεγάλο χρονικό διάστημα. Συνιστάται η διαγραφή την ένταση ήχου και, στη συνέχεια, τη δημιουργία μιας νέας αντί για να μορφοποιήσετε ξανά. Ωστόσο, εάν εξακολουθείτε προτιμούν να αλλάξτε τη μορφοποίηση ενός τόμου:<ul><li>Εκτελέστε την ακόλουθη εντολή πριν από την Διαμορφώστε ξανά για να αποφύγετε καθυστερήσεις ανάκτηση χώρο: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Όταν ολοκληρωθεί η μορφοποίηση, χρησιμοποιήστε την ακόλουθη εντολή για να ενεργοποιήσετε εκ νέου την ανάκτηση του χώρου:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Εφαρμόστε την επείγουσα επιδιόρθωση Windows Server 2012, όπως περιγράφεται στο [KB 2878635](https://support.microsoft.com/kb/2870270) σε υπολογιστή με Windows Server.</li></ul></li></ul></ul> Εάν ρυθμίζετε Διαχείριση στιγμιοτύπων StorSimple ή StorSimple προσαρμογέα για το SharePoint, μεταβείτε στις [απαιτήσεις λογισμικού για το προαιρετικών στοιχείων](#software-requirements-for-optional-components).|
| VMWare ESX | 5,5 και 6.0 | Υποστηρίζονται με VMWare vSphere ως iSCSI προγράμματος-πελάτη. Η δυνατότητα VAAI μπλοκ υποστηρίζεται με VMware vSphere σε συσκευές StorSimple.
| Linux RHEL/CentOS | 5, 6 και 7 | Υποστήριξη για προγράμματα-πελάτες iSCSI Linux με εκδόσεις προετοιμασίας Άνοιγμα iSCSI 5, 6 και 7. |
| Linux | SUSE Linux 11 | |
 > [AZURE.NOTE] IBM AIX αυτήν τη στιγμή δεν υποστηρίζεται με StorSimple.

## <a name="software-requirements-for-optional-components"></a>Απαιτήσεις λογισμικού για το προαιρετικών στοιχείων

Είναι τις ακόλουθες απαιτήσεις λογισμικού για τα προαιρετικά στοιχεία StorSimple (Διαχείριση στιγμιοτύπων StorSimple και StorSimple προσαρμογέα του SharePoint).

| Το στοιχείο | Πλατφόρμα κεντρικού υπολογιστή | Πρόσθετες απαιτήσεις/σημειώσεις |
| --------------------------- | ---------------- | ------------- |
| Διαχείριση στιγμιοτύπων StorSimple | Windows Server SP1 2008R2, 2012, 2012R2 | Χρήση του StorSimple στιγμιότυπο Manager σε Windows Server απαιτείται για δημιουργία αντιγράφων ασφαλείας και επαναφορά αντικριστά δυναμικών δίσκων και για οποιαδήποτε εφαρμογή συνεπή δημιουργίας αντιγράφων ασφαλείας.<br> Διαχείριση στιγμιοτύπων StorSimple υποστηρίζεται μόνο σε Windows Server 2008 R2 SP1 (64 bit), τα Windows 2012 R2 και Windows Server 2012.<ul><li>Εάν χρησιμοποιείτε το παράθυρο Server 2012, πρέπει να εγκαταστήσετε το .NET 3.5 – διαίρεσης 4,5 πριν από την εγκατάσταση Διαχείριση στιγμιοτύπων StorSimple.</li><li>Εάν χρησιμοποιείτε Windows Server 2008 R2 SP1, πρέπει να εγκαταστήσετε το Windows Management Framework 3.0 πριν από την εγκατάσταση Διαχείριση στιγμιοτύπων StorSimple.</li></ul> |
| Προσαρμογέα StorSimple για το SharePoint | Windows Server SP1 2008R2, 2012, 2012R2 |<ul><li>Προσαρμογέα StorSimple για το SharePoint υποστηρίζεται μόνο στο SharePoint 2010 και SharePoint 2013.</li><li>RBS απαιτεί SQL Server Enterprise Edition, την έκδοση 2008 R2 ή 2012.</li></ul>|

## <a name="networking-requirements-for-your-storsimple-device"></a>Απαιτήσεις για τη συσκευή σας StorSimple δικτύου

Η συσκευή σας StorSimple είναι μια κλειδωμένη συσκευή. Ωστόσο, οι θύρες πρέπει να είναι δυνατό το άνοιγμα στο τείχος προστασίας σας για να επιτρέψετε για iSCSI, cloud και διαχείριση κυκλοφορίας. Ο παρακάτω πίνακας παραθέτει τις θύρες που πρέπει να είναι δυνατό το άνοιγμα στο τείχος προστασίας σας. Σε αυτόν τον πίνακα *στο* ή *εισερχομένων* αναφέρεται προς την κατεύθυνση από την οποία εισερχόμενες αιτήσεις προγράμματος-πελάτη πρόσβαση τη συσκευή σας. *Ανάληψη* ή *εξερχομένων* αναφέρεται στην κατεύθυνση στην οποία συσκευή StorSimple στέλνει δεδομένα εξωτερικά, πέρα από την ανάπτυξη: για παράδειγμα, εξερχομένων στο Internet.

| Κωδικός θύρα<sup>1,2</sup> | Ή σμίκρυνση | Εμβέλεια θύρα | Απαιτείται | Σημειώσεις |
|------------------------|-----------|------------|----------|-------|
|TCP 80 (HTTP)<sup>3</sup>|  Ανάληψη |  WAN | Όχι |<ul><li>Θύρα εξερχομένων χρησιμοποιείται για πρόσβαση στο Internet για να ανακτήσετε ενημερώσεις.</li><li>Ο διακομιστής μεσολάβησης εξερχομένων web είναι χρήστη με δυνατότητα ρύθμισης παραμέτρων.</li><li>Για να επιτρέψετε ενημερώσεις συστήματος, αυτήν τη θύρα πρέπει να είναι ανοιχτό για τον ελεγκτή σταθερής διευθύνσεις IP.</li></ul> |
|TCP 443 (HTTPS)<sup>3</sup>| Ανάληψη | WAN | Ναι |<ul><li>Θύρα εξερχομένων χρησιμοποιείται για την πρόσβαση στα δεδομένα του στο cloud.</li><li>Ο διακομιστής μεσολάβησης εξερχομένων web είναι χρήστη με δυνατότητα ρύθμισης παραμέτρων.</li><li>Για να επιτρέψετε ενημερώσεις συστήματος, αυτήν τη θύρα πρέπει να είναι ανοιχτό για τον ελεγκτή σταθερής διευθύνσεις IP.</li><li>Αυτή η θύρα χρησιμοποιείται επίσης στους δύο ελεγκτές συλλογής απορριμμάτων.</li></ul>|
|UDP 53 (DNS) | Ανάληψη | WAN | Σε ορισμένες περιπτώσεις. Ανατρέξτε στις σημειώσεις. |Αυτή η θύρα είναι απαραίτητη μόνο εάν χρησιμοποιείτε ένα διακομιστή DNS που βασίζονται στο Internet. |
| UDP 123 (ΣΤΔ) | Ανάληψη | WAN | Σε ορισμένες περιπτώσεις. Ανατρέξτε στις σημειώσεις. |Αυτή η θύρα είναι απαραίτητη μόνο εάν χρησιμοποιείτε ένα διακομιστή NTP βασίζονται στο Internet. |
| TCP 9354 | Ανάληψη | WAN | Ναι |Η εξερχόμενη θύρα χρησιμοποιείται από τη συσκευή StorSimple για να επικοινωνήσετε με την υπηρεσία διαχείρισης StorSimple. |
| 3260 (iSCSI) | Στο | LAN | Όχι | Αυτή η θύρα χρησιμοποιείται για πρόσβαση σε δεδομένα μέσω iSCSI.|
| 5985 | Στο | LAN | Όχι | Θύρας εισερχομένων χρησιμοποιείται κατά τη Διαχείριση στιγμιοτύπων StorSimple για να επικοινωνήσετε με τη συσκευή StorSimple.<br>Αυτή η θύρα χρησιμοποιείται επίσης όταν απομακρυσμένα συνδέεστε με το Windows PowerShell για StorSimple μέσω HTTP. |
| 5986 | Στο | LAN | Όχι | Αυτήν τη θύρα χρησιμοποιείται όταν απομακρυσμένα συνδέεστε με το Windows PowerShell για StorSimple μέσω HTTPS. |

<sup>1</sup> δεν υπάρχει θύρες εισερχομένων που πρέπει να είναι δυνατό το άνοιγμα σε δημόσια στο Internet.

<sup>2</sup> αν πολλές θύρες εκτελέσετε μια ρύθμιση παραμέτρων της πύλης, την εξερχόμενη κυκλοφορία δρομολόγησης σειρά θα προσδιοριστεί με βάση τη σειρά δρομολόγησης θύρα περιγράφεται στη [θύρα δρομολόγηση](#routing-metric), παρακάτω.

<sup>3</sup> στον ελεγκτή σταθερής διευθύνσεις IP στη συσκευή σας StorSimple πρέπει να δυνατότητα δρομολόγησης και μπορείτε να συνδεθείτε στο Internet. Οι σταθερές διευθύνσεις IP που χρησιμοποιούνται για τη συντήρηση των ενημερώσεων στη συσκευή. Εάν οι ελεγκτές συσκευή δεν μπορεί να συνδεθεί στο Internet μέσω του σταθερού διευθύνσεις IP, δεν θα μπορείτε να ενημερώσετε τη συσκευή σας StorSimple.

> [AZURE.IMPORTANT] Βεβαιωθείτε ότι το τείχος προστασίας δεν τροποποίηση ή αποκρυπτογράφηση οποιαδήποτε κυκλοφορία SSL μεταξύ της συσκευής StorSimple και Azure.

### <a name="url-patterns-for-firewall-rules"></a>Διεύθυνση URL μοτίβα για κανόνες τείχους προστασίας

Οι διαχειριστές δικτύου συχνά να ρυθμίσετε τις παραμέτρους με βάση τα μοτίβα διεύθυνσης URL για να φιλτράρετε τα εισερχόμενα και την εξερχόμενη κυκλοφορία κανόνες τείχους προστασίας για προχωρημένους. Συσκευή StorSimple και της υπηρεσίας διαχείρισης StorSimple εξαρτώνται από άλλες εφαρμογές της Microsoft όπως Azure Service Bus, Azure Active Directory τον έλεγχο πρόσβασης, λογαριασμούς χώρου αποθήκευσης και διακομιστές Microsoft Update. Τα μοτίβα διεύθυνση URL που σχετίζονται με αυτές τις εφαρμογές μπορεί να χρησιμοποιηθεί για τη ρύθμιση παραμέτρων κανόνες τείχους προστασίας. Είναι σημαντικό να κατανοήσετε ότι μπορούν να αλλάξουν τα μοτίβα διεύθυνση URL που σχετίζονται με αυτές τις εφαρμογές. Αυτό με τη σειρά απαιτεί το διαχειριστή του δικτύου για την παρακολούθηση και την ενημέρωση κανόνες τείχους προστασίας για το StorSimple ως και όταν είναι απαραίτητο.

Συνιστάται να ορίσετε το κανόνες τείχους προστασίας για την εξερχόμενη κυκλοφορία, με βάση σταθερές liberally διευθύνσεις IP, στις περισσότερες περιπτώσεις StorSimple. Ωστόσο, μπορείτε να χρησιμοποιήσετε τις παρακάτω πληροφορίες για να ορίσετε κανόνες τείχους προστασίας για προχωρημένους που είναι απαραίτητα για τη δημιουργία ασφαλούς περιβάλλοντα.

> [AZURE.NOTE] Η συσκευή (προέλευση) διευθύνσεις IP πάντα πρέπει να οριστεί σε όλες τις διασυνδέσεις με δυνατότητα δικτύου. Ο προορισμός πρέπει να έχει οριστεί διευθύνσεις IP του [κέντρου δεδομένων του Azure περιοχές διευθύνσεων IP](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).

#### <a name="url-patterns-for-azure-portal"></a>Διεύθυνση URL μοτίβα για την πύλη Azure
| Διεύθυνση URL μοτίβου                                                      | / Λειτουργικότητας του στοιχείου                                           | Διευθύνσεις IP συσκευή                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | Υπηρεσία διαχείρισης StorSimple<br>Η υπηρεσία ελέγχου πρόσβασης<br>Bus Azure υπηρεσίας| Διασυνδέσεις δικτύου με δυνατότητα cloud        |
|`https://*.backup.windowsazure.com`|Η καταχώρηση της συσκευής| ΔΕΔΟΜΈΝΑ 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Ανάκλησης πιστοποιητικών |Διασυνδέσεις δικτύου με δυνατότητα cloud |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Λογαριασμοί Azure χώρου αποθήκευσης και παρακολούθηση | Διασυνδέσεις δικτύου με δυνατότητα cloud        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Οι διακομιστές Microsoft Update<br>                             | Σταθερή μόνο διευθύνσεις IP του ελεγκτή               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Σταθερή μόνο διευθύνσεις IP του ελεγκτή   |
| `https://*.partners.extranet.microsoft.com/*`                    | Πακέτο υποστήριξης                                                  | Διασυνδέσεις δικτύου με δυνατότητα cloud        |

#### <a name="url-patterns-for-azure-government-portal"></a>Διεύθυνση URL μοτίβα για δημόσιους οργανισμούς Azure πύλη
| Διεύθυνση URL μοτίβου                                                      | / Λειτουργικότητας του στοιχείου                                           | Διευθύνσεις IP συσκευή                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`   | Υπηρεσία διαχείρισης StorSimple<br>Η υπηρεσία ελέγχου πρόσβασης<br>Bus Azure υπηρεσίας| Διασυνδέσεις δικτύου με δυνατότητα cloud        |
|`https://*.backup.windowsazure.us`|Η καταχώρηση της συσκευής| ΔΕΔΟΜΈΝΑ 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Ανάκλησης πιστοποιητικών |Διασυνδέσεις δικτύου με δυνατότητα cloud |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Λογαριασμοί Azure χώρου αποθήκευσης και παρακολούθηση | Διασυνδέσεις δικτύου με δυνατότητα cloud        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Οι διακομιστές Microsoft Update<br>                             | Σταθερή μόνο διευθύνσεις IP του ελεγκτή               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Σταθερή μόνο διευθύνσεις IP του ελεγκτή   |
| `https://*.partners.extranet.microsoft.com/*`                    | Πακέτο υποστήριξης                                                  | Διασυνδέσεις δικτύου με δυνατότητα cloud        |

### <a name="routing-metric"></a>Μέτρο δρομολόγησης

Ένα μέτρο δρομολόγησης είναι συσχετισμένη με το διασυνδέσεων και την πύλη που δρομολόγηση τα δεδομένα στο καθορισμένο δίκτυα. Μέτρο δρομολόγησης χρησιμοποιείται από το πρωτόκολλο δρομολόγησης για να υπολογίσετε την καλύτερη διαδρομή για μια δεδομένη προορισμού, εάν το μαθαίνει υπάρχουν πολλές διαδρομές για τον ίδιο προορισμό. Το κάτω το μέτρο δρομολόγησης, όσο υψηλότερη την προτίμηση.

Στο πλαίσιο της StorSimple, αν πολλές διασυνδέσεις δικτύου και των πυλών έχουν ρυθμιστεί να κυκλοφορίας καναλιού, το μετρικά δρομολόγησης θα αρχίσουν να αναπαραγωγή για να καθορίσετε τη σχετική σειρά με την οποία θα λάβετε χρησιμοποιούνται τα περιβάλλοντα εργασίας. Η δρομολόγηση μετρικά δεν μπορεί να αλλάξει από το χρήστη. Ωστόσο, μπορείτε να χρησιμοποιήσετε το `Get-HcsRoutingTable` cmdlet για να εκτυπώσετε το δρομολόγησης πίνακα (και μετρήσεις) στη συσκευή σας StorSimple. Περισσότερες πληροφορίες σχετικά με το cmdlet Get-HcsRoutingTable σε [Ανάπτυξη StorSimple αντιμετώπιση προβλημάτων](storsimple-troubleshoot-deployment.md).

Η δρομολόγηση μετρικό αλγόριθμοι είναι διαφορετικά, ανάλογα με την έκδοση του λογισμικού εκτελείται στη συσκευή σας StorSimple.

**Εκδόσεις πριν από την ενημέρωση 1**

Περιλαμβάνονται και οι εκδόσεις λογισμικού πριν από την ενημέρωση 1 όπως το GA, 0,1, 0,2 ή 0,3 κυκλοφορίας. Η σειρά με βάση μετρικά δρομολόγησης είναι ως εξής:

   *Τελευταία έχει ρυθμιστεί 10 διασύνδεση δικτύου GbE > διασύνδεση δικτύου άλλες 10 GbE > τελευταία έχει ρυθμιστεί 1 διασύνδεση δικτύου GbE > διασύνδεση δικτύου άλλες GbE 1*


**Εκδόσεις ξεκινώντας από 1 ενημέρωση και πριν από την έκδοση 2**

Περιλαμβάνονται και οι εκδόσεις λογισμικού όπως το 1, 1.1 ή 1.2. Η σειρά με βάση μετρικά δρομολόγησης καθορίζεται ως εξής:

   *ΔΕΔΟΜΈΝΑ 0 > ρυθμιστεί τελευταία 10 διασύνδεση δικτύου GbE > διασύνδεση δικτύου άλλες 10 GbE > τελευταία έχει ρυθμιστεί 1 διασύνδεση δικτύου GbE > διασύνδεση δικτύου άλλες GbE 1*

   Ενημέρωση 1, γίνεται τη δρομολόγηση μέτρηση δεδομένων 0 τη χαμηλότερη; Επομένως, όλα τα cloud-κίνηση δρομολογείται μέσω ΔΕΔΟΜΈΝΑ 0. Σημειώστε αυτό εάν υπάρχουν περισσότερα από ένα περιβάλλον εργασίας με δυνατότητα cloud δικτύου στη συσκευή σας StorSimple.


**Εκδόσεις ξεκινώντας από την ενημερωμένη έκδοση 2**

Ενημέρωση 2 διαθέτει αρκετές βελτιώσεις που σχετίζονται με το δίκτυο και τα μετρικά δρομολόγηση έχει αλλάξει. Η συμπεριφορά μπορεί να αναλυθεί ως εξής.

- Ένα σύνολο τιμών προκαθορισμένο έχουν εκχωρηθεί σε διασυνδέσεις δικτύου.   

- Εξετάστε το ενδεχόμενο ενός πίνακα παράδειγμα που φαίνεται παρακάτω, με τιμές που έχουν εκχωρηθεί σε τις διάφορες διασυνδέσεις δικτύου, όταν είναι cloud-ενεργοποιημένες ή cloud-disabled αλλά με μια πύλη έχει ρυθμιστεί. Σημείωση Οι τιμές που έχουν εκχωρηθεί εδώ είναι παραδείγματα τιμών.


  	| Περιβάλλον εργασίας δικτύου | Με δυνατότητα cloud | Cloud-disabled με πύλης |
  	|-----|---------------|---------------------------|
  	| Δεδομένα 0  | 1            | -                        |
  	| Σύνολο δεδομένων 1  | 2            | 20                       |
  	| Σύνολο δεδομένων 2  | 3            | 30                       |
  	| Δεδομένων 3  | 4            | 40                       |
  	| Δεδομένων 4  | 5            | 50                       |
  	| Δεδομένων 5  | 6            | 60                       |


- Η σειρά με την οποία θα δρομολογούνται την κυκλοφορία cloud μέσω τις διασυνδέσεις δικτύου είναι:

    *Δεδομένα 0 > δεδομένων 1 > ημερομηνία 2 > δεδομένων 3 > δεδομένων 4 > δεδομένων 5*

    Αυτό μπορεί να αναλυθεί με το παρακάτω παράδειγμα.

    Εξετάστε το ενδεχόμενο μια συσκευή StorSimple με δύο περιβάλλοντα εργασίας με δυνατότητα cloud δικτύου, δεδομένα 0 και 5 δεδομένων. Σύνολο δεδομένων 1 έως 4 δεδομένων είναι cloud-disabled αλλά έχει ρυθμισμένη πύλη. Η σειρά με την οποία θα δρομολογούνται κίνηση για αυτήν τη συσκευή θα είναι:

    *Δεδομένα 0 (1) > δεδομένων 5 (6) > δεδομένων 1 (20) > δεδομένων 2 (30) > δεδομένων 3 (40) > δεδομένων 4 (50)*

    *όπου τους αριθμούς σε παρενθέσεις υποδεικνύουν το αντίστοιχο μετρικά δρομολόγησης.*

    Εάν αποτύχει δεδομένα 0, η κίνηση cloud θα να δρομολογηθούν έως 5 δεδομένων. Δεδομένου ότι μια πύλη έχει ρυθμιστεί σε όλες τις άλλες δίκτυο, εάν και τα δεδομένα 0 και 5 δεδομένων να αποτύχει, η κίνηση cloud θα δούμε δεδομένα 1.


- Εάν αποτύχει μια διασύνδεση δικτύου με δυνατότητα cloud, τότε είναι 3 επαναλήψεις με 30 δεύτερο καθυστέρηση για να συνδεθείτε με το περιβάλλον εργασίας. Εάν όλα τα επαναλήψεις αποτύχει, η κίνηση δρομολογείται την επόμενη διαθέσιμη με δυνατότητα cloud περιβάλλον εργασίας όπως καθορίζεται από τον πίνακα δρομολόγησης. Εάν όλα στο cloud με δυνατότητα δίκτυο διασυνδέσεις ΑΠΟΤΥΧΙΑ, στη συνέχεια, στη συσκευή θα αποτύχει πάνω από στον άλλο ελεγκτή (χωρίς επανεκκίνηση σε αυτήν την περίπτωση).

- Εάν υπάρχει μια αποτυχία VIP ένα περιβάλλον εργασίας με δυνατότητα iSCSI δικτύου, θα είναι 3 επαναλήψεις με μια καθυστέρηση 2 δευτερόλεπτα. Αυτή η συμπεριφορά έχει παρέμεινε το ίδιο από τις προηγούμενες εκδόσεις. Εάν όλες τις διασυνδέσεις δικτύου iSCSI αποτύχει, στη συνέχεια, θα παρουσιαστεί (συνοδεύεται από επανεκκίνηση) ελεγκτή ανακατεύθυνσης.


- Μια ειδοποίηση ενεργοποιείται επίσης στη συσκευή σας StorSimple όταν υπάρχει μια αποτυχία VIP. Για περισσότερες πληροφορίες, μεταβείτε στην [ειδοποίηση γρήγορης αναφοράς](storsimple-manage-alerts.md).

- Όσον αφορά τις προσπάθειες, iSCSI θα προηγούνται cloud.

    Εξετάστε το ακόλουθο παράδειγμα: A StorSimple συσκευή έχει δύο διασυνδέσεις δικτύου με δυνατότητα, δεδομένα 0 και 1 δεδομένων. Δεδομένα 0 είναι cloud με δυνατότητα ότι 1 δεδομένων είναι και τα δύο cloud και με δυνατότητα iSCSI. Άλλες διασυνδέσεις δικτύου σε αυτήν τη συσκευή είναι ενεργοποιημένες για cloud ή iSCSI.

    Εάν αποτύχει δεδομένα 1, δίνεται το είναι η τελευταία διασύνδεση δικτύου iSCSI, αυτό θα έχει ως αποτέλεσμα ανακατεύθυνσης ελεγκτή σε 1 δεδομένων σε άλλον ελεγκτή.


### <a name="networking-best-practices"></a>Βέλτιστες πρακτικές δικτύωση

Εκτός από τις παραπάνω απαιτήσεις κοινωνικής δικτύωσης, για τη βέλτιστη απόδοση της λύσης σας StorSimple, επικοινωνήστε συμμορφώνεται με τις ακόλουθες βέλτιστες πρακτικές:

- Βεβαιωθείτε ότι η συσκευή σας StorSimple διαθέτει ένα αποκλειστικό 40 εύρους ζώνης Mbps (ή περισσότερα) διαθέσιμα ανά πάσα στιγμή. Σε αυτό το εύρος ζώνης δεν θα πρέπει να είναι κοινόχρηστη (ή θα πρέπει να διασφαλίζεται εκχώρησης χρησιμοποιώντας πολιτικές ποιότητας υπηρεσίας) με τις άλλες εφαρμογές.

- Βεβαιωθείτε ότι η συνδεσιμότητα του δικτύου στο Internet είναι διαθέσιμη πάντα. Συνδέσεις στο Internet σποραδική ή μη αξιόπιστη στις συσκευές, συμπεριλαμβανομένων χωρίς σύνδεση στο Internet οποιαδήποτε, θα έχει ως αποτέλεσμα μια μη υποστηριζόμενη ρύθμιση παραμέτρων.

- Απομόνωση την κυκλοφορία iSCSI και cloud από αντιμετωπίζετε αφοσιωμένη διασυνδέσεις δικτύου στη συσκευή σας για πρόσβαση iSCSI και cloud. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα πώς μπορείτε να [τροποποιήσετε διασυνδέσεις δικτύου](storsimple-modify-device-config.md#modify-network-interfaces) στη συσκευή σας StorSimple.

- Μην χρησιμοποιείτε μια ρύθμιση παραμέτρων του πρωτοκόλλου ελέγχου Συνάθροιση συνδέσεων (LACP) για τις διασυνδέσεις δικτύου. Αυτή είναι μια ρύθμιση παραμέτρων που δεν υποστηρίζονται.


## <a name="high-availability-requirements-for-storsimple"></a>Απαιτήσεις υψηλής διαθεσιμότητας για StorSimple

Την πλατφόρμα υλικού που συνοδεύει τη λύση StorSimple έχει δυνατοτήτων διαθεσιμότητα και την αξιοπιστία που παρέχουν μια βάση για μια υποδομή ιδιαίτερα διαθέσιμη, ανοχή χώρου αποθήκευσης στο κέντρο δεδομένων σας. Ωστόσο, υπάρχουν απαιτήσεις και βέλτιστες πρακτικές που θα πρέπει να συμμορφώνεστε με για να διασφαλίσετε τη διαθεσιμότητα της λύσης σας StorSimple. Πριν από την ανάπτυξη StorSimple, προσεκτικά τις παρακάτω απαιτήσεις και βέλτιστες πρακτικές για τη συσκευή StorSimple και συνδεδεμένο κεντρικών υπολογιστών.

Για περισσότερες πληροφορίες σχετικά με την παρακολούθηση και τη διατήρηση τα στοιχεία υλικού της συσκευής σας StorSimple, μεταβείτε με [χρήση της υπηρεσίας διαχείρισης StorSimple για την παρακολούθηση της στοιχεία υλικού και την κατάσταση](storsimple-monitor-hardware-status.md) και [Αντικατάσταση στοιχείο StorSimple υλικού](storsimple-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Απαιτήσεις υψηλής διαθεσιμότητας και διαδικασίες για τη συσκευή σας StorSimple

Εξετάστε τις ακόλουθες πληροφορίες προσεκτικά για να εξασφαλίσετε την υψηλή διαθεσιμότητα της συσκευής σας StorSimple.

#### <a name="pcms"></a>PCMs

Οι συσκευές StorSimple περιλαμβάνουν πλεονάζοντα, αντιμετάθεσης τροφοδοσίας και ψύξης λειτουργικές μονάδες (PCMs). Κάθε PCM έχει αρκετή χωρητικότητα για την παροχή υπηρεσίας για ολόκληρο το πλαίσιο. Για να εξασφαλίσετε υψηλή διαθεσιμότητα, πρέπει να έχει εγκατασταθεί τόσο PCMs.

- Η σύνδεση σας PCMs με διάφορες πηγές ενέργειας για την παροχή διαθεσιμότητα εάν αποτύχει μια πηγή ενέργειας.
- Εάν αποτύχει μια PCM, ζητήστε αντικατάσταση αμέσως.
- Κατάργηση μιας αποτυχίας PCM μόνο όταν έχετε αντικατάσταση και είστε έτοιμοι να το εγκαταστήσετε.
- Μην καταργήσετε PCMs και τα δύο ταυτόχρονα. Η λειτουργική μονάδα PCM περιλαμβάνει τη λειτουργική μονάδα μπαταριών δημιουργίας αντιγράφων ασφαλείας. Κατάργηση και τα δύο από τα PCMs θα έχει ως αποτέλεσμα έναν τερματισμό χωρίς προστασία μπαταριών και την κατάσταση της συσκευής δεν θα αποθηκευτούν. Για περισσότερες πληροφορίες σχετικά με το μπαταριών, μεταβείτε στη [Διατήρηση της λειτουργικής μονάδας μπαταριών δημιουργίας αντιγράφων ασφαλείας](storsimple-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Λειτουργικές μονάδες ελεγκτή

StorSimple συσκευές περιλαμβάνονται οι λειτουργικές μονάδες ελεγκτή πλεονάζοντα, αντιμετάθεσης. Οι λειτουργικές μονάδες ελεγκτή λειτουργούν με τον τρόπο ενεργό/παθητικό. Οποιαδήποτε στιγμή, μια λειτουργική μονάδα ελεγκτή είναι ενεργή και παρέχει επίσης την υπηρεσία, ενώ η άλλες λειτουργική μονάδα ελεγκτή είναι παθητικές. Η ενότητα παθητικές ελεγκτή είναι ενεργοποιημένη και τίθεται σε λειτουργία εάν τη λειτουργική μονάδα ενεργό ελεγκτή αποτύχει ή καταργηθεί. Κάθε λειτουργική μονάδα ελεγκτή έχει αρκετή χωρητικότητα για την παροχή υπηρεσίας για ολόκληρο το πλαίσιο. Πρέπει να έχει εγκατασταθεί τόσο λειτουργικές μονάδες ελεγκτή για να βεβαιωθείτε ότι υψηλής διαθεσιμότητας.

- Βεβαιωθείτε ότι έχετε εγκαταστήσει και τις δύο λειτουργικές μονάδες ελεγκτή πάντα.

- Εάν αποτύχει μια λειτουργική μονάδα ελεγκτή, ζητήστε αντικατάσταση αμέσως.

- Καταργήστε μια λειτουργική μονάδα αποτυχίας ελεγκτή μόνο όταν έχετε αντικατάσταση και είστε έτοιμοι να το εγκαταστήσετε. Κατάργηση μιας λειτουργικής μονάδας για το εκτεταμένο περίοδοι θα επηρεάσει τη ροή του αέρα και συνεπώς την ψύξη του συστήματος.

- Βεβαιωθείτε ότι είναι ταυτόσημες τις συνδέσεις δικτύου με δύο λειτουργικές μονάδες ελεγκτή, και μια ρύθμιση παραμέτρων όμοια δικτύου έχει το περιβάλλον συνδεδεμένο δίκτυο.

- Εάν μια λειτουργική μονάδα ελεγκτή αποτύχει ή χρειάζεται αντικατάστασης, βεβαιωθείτε ότι τα άλλα λειτουργική μονάδα ελεγκτή είναι σε μη ενεργή κατάσταση πριν από την αντικατάσταση της λειτουργικής μονάδας αποτυχίας ελεγκτή. Για να επαληθεύσετε ότι ένα ελεγκτή είναι ενεργό, μεταβείτε στην [Περιγραφή του ενεργού ελεγκτή στη συσκευή σας](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Μην καταργήσετε δύο λειτουργικές μονάδες ελεγκτή την ίδια στιγμή. Εάν ανακατεύθυνσης ελεγκτή βρίσκεται σε εξέλιξη, δεν τερματισμός της λειτουργικής μονάδας αναμονής ελεγκτή ή να καταργήσετε από το πλαίσιο.

- Μετά την ανακατεύθυνσης ελεγκτή, περιμένετε τουλάχιστον πέντε λεπτά πριν από την κατάργηση είτε λειτουργική μονάδα ελεγκτή.

#### <a name="network-interfaces"></a>Διασυνδέσεις δικτύου

StorSimple συσκευή ελεγκτή λειτουργικές μονάδες κάθε έχουν τέσσερις 1 Gigabit και 10 δύο διασυνδέσεις δικτύου Gigabit Ethernet.

- Βεβαιωθείτε ότι είναι ταυτόσημες τις συνδέσεις δικτύου με δύο λειτουργικές μονάδες ελεγκτή, και διασυνδέσεις του δικτύου που είναι συνδεδεμένοι για τη λειτουργική μονάδα που ελεγκτή να έχετε μια ρύθμιση παραμέτρων όμοια δικτύου.

- Όταν είναι δυνατό, ανάπτυξη των συνδέσεων δικτύου σε διαφορετικούς διακόπτες για να βεβαιωθείτε ότι διαθεσιμότητα υπηρεσίας σε περίπτωση αποτυχίας συσκευή δικτύου.

- Κατά την αποσύνδεση ο μοναδικός ή το τελευταίο υπόλοιπο με δυνατότητα iSCSI περιβάλλοντος εργασίας (με διευθύνσεις IP που έχουν εκχωρηθεί), απενεργοποιήστε πρώτα το περιβάλλον εργασίας και, στη συνέχεια, αποσυνδέστε τα καλώδια. Εάν το περιβάλλον εργασίας αποσυνδεθεί πρώτα, στη συνέχεια, θα προκαλέσει την ενεργή ελεγκτή ανακατευθύνει στον ελεγκτή παθητικές. Εάν το παθητικό ελεγκτή έχει επίσης τις αντίστοιχες διασυνδέσεις αποσυνδεθεί, στη συνέχεια, τους δύο ελεγκτές θα γίνει επανεκκίνηση πολλές φορές πριν από την εκκαθάριση στον ελεγκτή μία.

- Συνδεθείτε στο δίκτυο τουλάχιστον δύο διασυνδέσεων ΔΕΔΟΜΈΝΑ από κάθε λειτουργική μονάδα ελεγκτή.

- Εάν έχετε ενεργοποιήσει τις δύο 10 GbE διασυνδέσεων, ανάπτυξη αυτές σε διαφορετικούς διακόπτες.

- Όταν είναι δυνατό, χρησιμοποιήστε MPIO σε διακομιστές για να εξασφαλίσετε ότι οι διακομιστές μπορεί να αποδεχτεί τις μια σύνδεση, το δίκτυο ή σφάλμα στο περιβάλλον.

Για περισσότερες πληροφορίες σχετικά με τα δίκτυα τη συσκευή σας για υψηλή διαθεσιμότητα και επιδόσεων, μεταβείτε για να [εγκαταστήσετε τη συσκευή σας StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) ή [τη συσκευή σας StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD και σκληροί

Συσκευές StorSimple περιλαμβάνουν δίσκων συμπαγές κατάστασης (SSD) και μονάδες σκληρού δίσκου (σκληροί) που προστατεύονται με χρήση κατοπτρίζεται κενά διαστήματα. Χρησιμοποιήστε αντικριστά κενών διαστημάτων εξασφαλίζει ότι η συσκευή είναι σε θέση ανοχή την αποτυχία της μίας ή περισσότερων SSD ή σκληροί.

- Βεβαιωθείτε ότι έχουν εγκατασταθεί όλες οι λειτουργικές μονάδες SSD και σκληρού ΔΊΣΚΟΥ.

- Εάν αποτύχει μια SSD ή σκληρού ΔΊΣΚΟΥ, ζητήστε αντικατάσταση αμέσως.

- Εάν μια SSD ή σκληρού ΔΊΣΚΟΥ αποτύχει ή απαιτεί αντικατάστασης, βεβαιωθείτε ότι καταργήσετε μόνο τις SSD ή σκληρού ΔΊΣΚΟΥ που απαιτεί αντικατάστασης.

- Μην καταργήσετε περισσότερες από μία SSD ή σκληρού ΔΊΣΚΟΥ από το σύστημα σε οποιοδήποτε σημείο στο χρόνο.
Αποτυχία 2 ή περισσότερων δίσκων συγκεκριμένου τύπου (σκληρού ΔΊΣΚΟΥ, SSD) ή την αποτυχία διαδοχικές μέσα σε ένα πλαίσιο Σύντομη ώρα ενδέχεται να οδηγήσει σε σύστημα δυσλειτουργία και τις πιθανές απώλεια δεδομένων. Εάν αυτό συμβαίνει, [Επικοινωνήστε με την υποστήριξη της Microsoft](storsimple-contact-microsoft-support.md) για βοήθεια.

- Κατά τη διάρκεια της αντικατάστασης, παρακολουθείτε την **Κατάσταση υλικού** στη σελίδα **Συντήρηση** για τις μονάδες στην SSD και σκληροί. Κατάσταση πράσινο σημάδι ελέγχου υποδεικνύει ότι δίσκων είναι σε καλή κατάσταση ή OK, ότι ένα κόκκινο θαυμαστικό σημείου υποδεικνύει μια αποτυχίας SSD ή σκληρού ΔΊΣΚΟΥ.

- Συνιστάται να ρυθμίζετε τις παραμέτρους στιγμιότυπα cloud για όλες τις όγκους που χρειάζεστε για την προστασία σε περίπτωση αποτυχίας του συστήματος.

#### <a name="ebod-enclosure"></a>EBOD περίβλημα

Μοντέλο της συσκευής StorSimple 8600 περιλαμβάνει ένα εκτεταμένο Bunch των δίσκων (EBOD) περίβλημα εκτός από το κύριο περίβλημα. Μια EBOD περιέχει EBOD ελεγκτές και μονάδες σκληρού δίσκου (σκληροί) που προστατεύονται με χρήση κατοπτρίζεται κενά διαστήματα. Χρησιμοποιήστε αντικριστά κενών διαστημάτων εξασφαλίζει ότι η συσκευή είναι σε θέση ανοχή την αποτυχία της μίας ή περισσότερων σκληροί. Το περίβλημα EBOD είναι συνδεδεμένο με το κύριο περίβλημα μέσω πλεονάζοντα καλώδια συσχετισμών Ασφαλείας.

- Βεβαιωθείτε ότι και οι δύο EBOD περίβλημα ελεγκτή λειτουργικές μονάδες, τόσο καλώδια συσχετισμών Ασφαλείας, όσο και όλες τις μονάδες σκληρού δίσκου είναι εγκατεστημένα πάντα.

- Εάν αποτύχει μια λειτουργική μονάδα EBOD περίβλημα ελεγκτή, ζητήστε αντικατάσταση αμέσως.

- Εάν αποτύχει μια λειτουργική μονάδα EBOD περίβλημα ελεγκτή, βεβαιωθείτε ότι τα άλλα λειτουργική μονάδα ελεγκτή είναι ενεργό, πριν να αντικαταστήσετε τη λειτουργική μονάδα αποτυχίας. Για να επαληθεύσετε ότι ένα ελεγκτή είναι ενεργό, μεταβείτε στην [Περιγραφή του ενεργού ελεγκτή στη συσκευή σας](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Κατά τη διάρκεια μιας EBOD ελεγκτή λειτουργική μονάδα αντικατάστασης, συνεχώς παρακολουθείτε την κατάσταση του στοιχείου στην υπηρεσία διαχείρισης StorSimple με πρόσβαση σε **Συντήρηση** > **Κατάσταση υλικού**.

- Εάν ένα καλώδιο συσχετισμών Ασφαλείας αποτύχει ή απαιτεί αντικατάστασης (υπηρεσία υποστήριξης της Microsoft θα πρέπει να συμμετέχουν για να κάνετε μια τέτοια απόφαση), βεβαιωθείτε ότι μπορείτε να καταργήσετε μόνο το καλώδιο συσχετισμών Ασφαλείας που απαιτεί αντικατάστασης.

- Δεν ταυτόχρονα καταργήσετε δύο καλώδια συσχετισμών Ασφαλείας από το σύστημα σε οποιοδήποτε σημείο στο χρόνο.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Υψηλή διαθεσιμότητα συστάσεις για το κεντρικών υπολογιστών

Προσεκτικά αυτές τις βέλτιστες πρακτικές για να βεβαιωθείτε ότι η υψηλή διαθεσιμότητα κεντρικούς υπολογιστές που είναι συνδεδεμένη με τη συσκευή σας StorSimple.

- Ρύθμιση παραμέτρων StorSimple με [ρυθμίσεις παραμέτρων συμπλέγματος διακομιστών αρχείο δύο κόμβου][1]. Με την κατάργηση μόνο σημεία του αποτυχία και τη δημιουργία στο πλεονασμού στην πλευρά κεντρικού υπολογιστή, ολόκληρη τη λύση θα είναι ιδιαίτερα διαθέσιμο.

- Χρήση συνεχώς διαθέσιμη (CA) καθιστά διαθέσιμη με το Windows Server 2012 (Ερωτήσεων 3.0) για υψηλή διαθεσιμότητα κατά την ανακατεύθυνση τους ελεγκτές αποθήκευσης. Για πρόσθετες πληροφορίες για τη ρύθμιση παραμέτρων συμπλεγμάτων διακομιστή του αρχείου και συνεχώς διαθέσιμη μετοχές με Windows Server 2012, ανατρέξτε σε αυτό το [Βίντεο επίδειξης](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Επόμενα βήματα

- [Μάθετε σχετικά με τα όρια του συστήματος StorSimple](storsimple-limits.md).
- [Μάθετε πώς μπορείτε να αναπτύξετε τη λύση σας StorSimple](storsimple-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
