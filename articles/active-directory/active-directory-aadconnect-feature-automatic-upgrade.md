<properties
   pageTitle="Azure AD Connect: Αυτόματη αναβάθμιση | Microsoft Azure"
   description="Αυτό το θέμα περιγράφει τη δυνατότητα αυτόματης αναβάθμισης ενσωματωμένο, στο Azure AD Connect συγχρονισμός."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/24/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Αυτόματη αναβάθμιση
Αυτή η δυνατότητα Παρουσιάστηκε με Δόμηση 1.1.105.0 (κυκλοφορήσει Φεβρουαρίου 2016).

## <a name="overview"></a>Επισκόπηση
Επιβεβαίωση της εγκατάστασης του Azure AD Connect είναι πάντα ενημερωμένα είναι ευκολότερη από ποτέ με τη δυνατότητα **αυτόματης αναβάθμισης** . Αυτή η δυνατότητα είναι ενεργοποιημένη από προεπιλογή για τις εγκαταστάσεις express αλλά και DirSync. Όταν κυκλοφορήσει μια νέα έκδοση, αναβαθμίζεται αυτόματα την εγκατάσταση.

Αυτόματη αναβάθμιση είναι ενεργοποιημένη από προεπιλογή για τα εξής:

- Express ρυθμίσεις εγκατάστασης και αναβαθμίσεις DirSync.
- Χρήση SQL Express LocalDB, που είναι τι ρυθμίσεις Express χρησιμοποιείται πάντα. DirSync με SQL Express Χρησιμοποιήστε επίσης LocalDB.
- Ο λογαριασμός AD είναι τον προεπιλεγμένο λογαριασμό MSOL_ που δημιουργήθηκε από ρυθμίσεις Express και DirSync.
- Έχετε λιγότερο ιεραρχική αντικείμενα στο το metaverse.

Η τρέχουσα κατάσταση της αυτόματης αναβάθμισης μπορούν να προβληθούν με το cmdlet του PowerShell `Get-ADSyncAutoUpgrade`. Έχει τα ακόλουθα μέλη:

Κατάσταση | Σχόλιο
---- | ----
Με δυνατότητα | Αυτόματη αναβάθμιση είναι ενεργοποιημένη.
Σε αναστολή | Ορισμός από το σύστημα μόνο. Το σύστημα δεν είναι πλέον κατάλληλη για να λαμβάνετε αυτόματες αναβαθμίσεις του στοιχείου.
Απενεργοποιημένα | Αυτόματη αναβάθμιση είναι απενεργοποιημένη.

Μπορείτε να αλλάξετε μεταξύ **ενεργοποιημένο** και **απενεργοποίησης** με `Set-ADSyncAutoUpgrade`. Μόνο το σύστημα θα πρέπει να ορίσετε την κατάσταση **σε αναστολή**.

Αυτόματη αναβάθμιση χρησιμοποιεί υγείας Azure AD σύνδεση για την υποδομή αναβάθμισης. Για αυτόματη αναβάθμιση ώστε να λειτουργεί, βεβαιωθείτε ότι έχετε ανοίξει τις διευθύνσεις URL στο διακομιστή μεσολάβησης για **Υγείας Azure AD σύνδεση** όπως περιγράφεται στις [διευθύνσεις URL του Office 365 και περιοχές διευθύνσεων IP](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Εάν η **Διαχείριση υπηρεσίας συγχρονισμού** περιβάλλοντος εργασίας Χρήστη εκτελείται στο διακομιστή, στη συνέχεια, την αναβάθμιση αναστέλλεται ώσπου το περιβάλλον εργασίας Χρήστη είναι κλειστό.

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων
Εάν την εγκατάσταση του σύνδεση δεν αναβάθμιση ίδιο με τον αναμενόμενο τρόπο, στη συνέχεια, ακολουθήστε τα παρακάτω βήματα για να μάθετε τι μπορεί να συμβαίνει.

Πρώτα, δεν πρέπει να περιμένουν την αυτόματη αναβάθμιση για να εφαρμοστούν την πρώτη ημέρα έχει κυκλοφορήσει μια νέα έκδοση. Υπάρχει μια εσκεμμένα τύχη πριν από την αναβάθμιση επιχειρείται έτσι δεν είναι προειδοποίησης, εάν δεν έχει αναβαθμιστεί αμέσως την εγκατάσταση.

Εάν πιστεύετε ότι κάτι που δεν είναι σωστή, στη συνέχεια, πρώτης εκτέλεσης `Get-ADSyncAutoUpgrade` για να βεβαιωθείτε ότι είναι ενεργοποιημένη η Αυτόματη αναβάθμιση.

Στη συνέχεια, βεβαιωθείτε ότι έχετε ανοίξει τις απαιτούμενες διευθύνσεις URL στο τείχος προστασίας ή διακομιστή μεσολάβησης. Αυτόματη ενημέρωση χρησιμοποιεί υγείας Azure AD σύνδεση όπως περιγράφεται στην [Επισκόπηση](#overview). Εάν χρησιμοποιείτε ένα διακομιστή μεσολάβησης, βεβαιωθείτε ότι έχει ρυθμιστεί ώστε να χρησιμοποιεί ένα [διακομιστή μεσολάβησης](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)εύρυθμης λειτουργίας. Επίσης να ελέγξετε την [εύρυθμη λειτουργία συνδεσιμότητας](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) με Azure AD.

Με τη σύνδεση με Azure AD επαληθευτεί, είναι ώρα να ερευνήσει το καταγραφές συμβάντων. Ξεκινήστε το πρόγραμμα προβολής συμβάντων και αναζητήστε το αρχείο καταγραφής συμβάντων **εφαρμογής** . Προσθέστε ένα φίλτρο καταγραφής συμβάντων για την προέλευση **Azure AD σύνδεση αναβάθμιση** και το συμβάν αναγνωριστικό περιοχή **300-399**.  
![Αρχείο καταγραφής συμβάντων φίλτρο για αυτόματη αναβάθμιση](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Τώρα μπορείτε να δείτε το καταγραφές συμβάντων που σχετίζονται με την κατάσταση για αυτόματη αναβάθμιση.  
![Αρχείο καταγραφής συμβάντων φίλτρο για αυτόματη αναβάθμιση](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

Ο κωδικός αποτελέσματος έχει ένα πρόθεμα με μια επισκόπηση της κατάστασης.

Πρόθεμα κωδικού αποτέλεσμα | Περιγραφή
--- | ---
Επιτυχίας | Η εγκατάσταση αναβαθμίστηκε με επιτυχία.
UpgradeAborted | Προσωρινό συνθήκη διακοπή της αναβάθμισης. Αυτό θα επαναληφθεί ξανά και την προοπτική είναι ότι επιτυγχάνει αργότερα.
UpgradeNotSupported | Το σύστημα έχει μια ρύθμιση παραμέτρων που αποκλείει το σύστημα από γίνεται αυτόματα αναβάθμιση. Θα επαναληφθεί το για να δείτε εάν αλλάζει η κατάσταση, αλλά το προσδοκίες είναι ότι το σύστημα πρέπει να αναβαθμιστούν με μη αυτόματο τρόπο.

Ακολουθεί μια λίστα με τα πιο συνηθισμένα μηνύματα, μπορείτε να βρείτε. Δεν εμφανίζει όλα, αλλά το μήνυμα αποτέλεσμα πρέπει να καταργήστε την επιλογή με το τι πρόβλημα είναι.

Αποτέλεσμα μηνύματος | Περιγραφή
--- | ---
**UpgradeAborted** |
UpgradeAbortedCouldNotSetUpgradeMarker | Δεν ήταν δυνατή η εγγραφή στο μητρώο.
UpgradeAbortedInsufficientDatabasePermissions | Την ομάδα των διαχειριστών ενσωματωμένη δεν έχει δικαιώματα για τη βάση δεδομένων. Η αναβάθμιση στην πιο πρόσφατη έκδοση του Azure AD Connect για να αντιμετωπίσετε αυτό το ζήτημα με μη αυτόματο τρόπο.
UpgradeAbortedInsufficientDiskSpace | Δεν υπάρχει αρκετός χώρος στο δίσκο για υποστήριξη αναβάθμισης.
UpgradeAbortedSecurityGroupsNotPresent | Δεν ήταν δυνατό να βρείτε και να επιλύσετε όλες τις ομάδες ασφαλείας που χρησιμοποιούνται από το μηχανισμό συγχρονισμού.
UpgradeAbortedServiceCanNotBeStarted | Απέτυχε η υπηρεσία NT **Microsoft Azure AD Sync** για να ξεκινήσετε.
UpgradeAbortedServiceCanNotBeStopped | Η υπηρεσία NT **Microsoft Azure AD Sync** απέτυχε να διακόψετε.
UpgradeAbortedServiceIsNotRunning | Δεν εκτελείται η υπηρεσία NT **Microsoft Azure AD Sync** .
UpgradeAbortedSyncCycleDisabled | Στην επιλογή "SyncCycle" το [Χρονοδιάγραμμα](active-directory-aadconnectsync-feature-scheduler.md) έχει απενεργοποιηθεί.
UpgradeAbortedSyncExeInUse | Η [Διαχείριση συγχρονισμού υπηρεσίας περιβάλλοντος εργασίας Χρήστη](active-directory-aadconnectsync-service-manager-ui.md) είναι ανοιχτό στο διακομιστή.
UpgradeAbortedSyncOrConfigurationInProgress | Εκτελείται ο Οδηγός εγκατάστασης ή συγχρονισμού είχε προγραμματιστεί έξω από το χρονοδιάγραμμα.
**UpgradeNotSupported** |
UpgradeNotSupportedCustomizedSyncRules | Έχετε προσθέσει το δικό σας προσαρμοσμένο κανόνες για τη ρύθμιση παραμέτρων.
UpgradeNotSupportedDeviceWritebackEnabled | Έχετε ενεργοποιήσει τη δυνατότητα [αντιγραφής συσκευή](active-directory-aadconnect-feature-device-writeback.md) .
UpgradeNotSupportedGroupWritebackEnabled | Έχετε ενεργοποιήσει τη δυνατότητα [αντιγραφής ομάδας](active-directory-aadconnect-feature-preview.md#group-writeback) .
UpgradeNotSupportedInvalidPersistedState | Η εγκατάσταση δεν είναι μια ρυθμίσεις Express ή μια αναβάθμιση DirSync.
UpgradeNotSupportedMetaverseSizeExceeeded | Έχετε ιεραρχική αντικείμενα στο το metaverse.
UpgradeNotSupportedMultiForestSetup | Πρόκειται να συνδεθείτε με περισσότερες από μία δάσος. Γρήγορη εγκατάσταση μόνο συνδέεται με ένα δάσος.
UpgradeNotSupportedNonLocalDbInstall | Δεν χρησιμοποιείτε μια βάση δεδομένων SQL Server Express LocalDB.
UpgradeNotSupportedNonMsolAccount | Ο [λογαριασμός σύνδεσης AD](active-directory-aadconnect-accounts-permissions.md#active-directory-account) δεν είναι πλέον τον προεπιλεγμένο λογαριασμό MSOL_.
UpgradeNotSupportedStagingModeEnabled | Ο διακομιστής έχει ρυθμιστεί να είναι σε [λειτουργία ενδιάμεσου](active-directory-aadconnectsync-operations.md#staging-mode).
UpgradeNotSupportedUserWritebackEnabled | Έχετε ενεργοποιήσει τη δυνατότητα [αντιγραφής χρήστη](active-directory-aadconnect-feature-preview.md#user-writeback) .

## <a name="next-steps"></a>Επόμενα βήματα
Μάθετε περισσότερα σχετικά με την [ενσωμάτωση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).
