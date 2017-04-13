<properties
    pageTitle="Azure AD Connect συγχρονισμού: χαρακτηριστικά συγχρονίζονται με Azure Active Directory | Microsoft Azure"
    description="Παραθέτει τα χαρακτηριστικά που έχουν συγχρονιστεί Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Connect συγχρονισμού: χαρακτηριστικά συγχρονίζονται με Azure Active Directory
Αυτό το θέμα παραθέτει τα χαρακτηριστικά που συγχρονίζονται από Azure AD Connect συγχρονισμού.  
Τα χαρακτηριστικά που έχουν ομαδοποιηθεί με βάση το σχετικό Azure AD εφαρμογής.

## <a name="attributes-to-synchronize"></a>Χαρακτηριστικά για συγχρονισμό
Κοινές ερώτησης είναι *Τι είναι η λίστα των ελάχιστα χαρακτηριστικά για το συγχρονισμό*. Η προεπιλεγμένη και συνιστώμενη προσέγγιση είναι να διατηρήσετε τα προεπιλεγμένα χαρακτηριστικά, ώστε να μπορεί να έχει ένα πλήρες GAL (καθολική λίστα διευθύνσεων) στο cloud και για όλες τις δυνατότητες σε φόρτους εργασίας του Office 365. Σε ορισμένες περιπτώσεις, υπάρχουν ορισμένα χαρακτηριστικά που την εταιρεία σας δεν θέλετε συγχρονισμένη στο cloud δεδομένου ότι περιέχει αυτά τα χαρακτηριστικά διάκριση πεζών-κεφαλαίων ή PII (προσωπικές πληροφορίες) δεδομένων, όπως σε αυτό το παράδειγμα:  
![εσφαλμένες χαρακτηριστικά](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

Σε αυτήν την περίπτωση, ξεκινήστε με τη λίστα των χαρακτηριστικών σε αυτό το θέμα και να την προσδιορίσετε αυτά τα χαρακτηριστικά που θα περιέχει ευαίσθητες ή PII δεδομένων και δεν μπορούν να συγχρονιστούν. Στη συνέχεια, καταργήστε την επιλογή αυτά τα χαρακτηριστικά κατά τη διάρκεια της εγκατάστασης χρησιμοποιώντας την [εφαρμογή Azure AD και το φιλτράρισμα χαρακτηριστικό](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering).

>[AZURE.WARNING] Κατά την κατάργηση επιλογής των χαρακτηριστικά, που θα πρέπει να είστε προσεκτικοί και να μόνο καταργήστε την επιλογή απολύτως δεν είναι δυνατό να συγχρονίσετε αυτά τα χαρακτηριστικά. Καταργώντας την επιλογή άλλες ιδιότητες ενδέχεται να έχει αρνητική επίπτωση σχετικά με τις δυνατότητες.

## <a name="office-365-proplus"></a>Office 365 ProPlus

| Όνομα χαρακτηριστικού| Χρήστη| Σχόλιο |
| --- | :-: | --- |
| accountEnabled| X| Καθορίζει εάν είναι ενεργοποιημένη η λογαριασμού.|
| CN| X|  |
| Εμφανιζόμενο όνομα| X|  |
| objectSID| X| η ιδιότητα μηχανικό. Αναγνωριστικό χρήστη AD που χρησιμοποιείται για τη διατήρηση συγχρονισμού μεταξύ Azure AD και AD.|
| pwdLastSet| X| η ιδιότητα μηχανικό. Χρησιμοποιείται για το πότε πρέπει να ακυρώσει ήδη δημοσιευμένα διακριτικά. Χρησιμοποιείται από συγχρονισμό κωδικού πρόσβασης και federation.|
| sourceAnchor| X| η ιδιότητα μηχανικό. Αμετάβλητες αναγνωριστικό για να διατηρήσετε τη σχέση μεταξύ ΠΡΟΣΘΈΤΕΙ και Azure AD.|
| usageLocation| X| η ιδιότητα μηχανικό. Χώρα/περιοχή του χρήστη. Χρησιμοποιείται για την εκχώρηση αδειών χρήσης.|
| userPrincipalName| X| UPN είναι το Αναγνωριστικό σύνδεσης για το χρήστη. Πιο συχνά την ίδια ως [mail] τιμή.|

## <a name="exchange-online"></a>Exchange Online

| Όνομα χαρακτηριστικού| Χρήστη| Επαφή| Ομάδα| Σχόλιο |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Καθορίζει εάν είναι ενεργοποιημένη η λογαριασμού.|
| Βοηθός| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| από κοινού| X| X|  |  |
| εταιρεία| X| X|  |  |
| Κωδικός| X| X|  |  |
| τμήμα| X| X|  |  |
| Περιγραφή| X| X| X|  |
| Εμφανιζόμενο όνομα| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| ΤηλέφωνοΟικίας| X| X|  |  |
| πληροφορίες| X| X| X| Αυτό το χαρακτηριστικό αυτήν τη στιγμή δεν καταναλώνεται για τις ομάδες.|
| Αρχικά| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailNickname| X| X| X|  |
| mangedBy|  |  | X|  |
| Διαχείριση| X| X|  |  |
| μέλος|  |  | X|  |
| κινητές συσκευές| X| X|  |  |
| msDS-HABSeniorityIndex| X| X| X|  |
| msDS-PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| msExchAssistantName| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| Αυτό το χαρακτηριστικό αυτήν τη στιγμή δεν καταναλώνεται από το Exchange Online. |
| msExchExtensionCustomAttribute2| X| X| X| Αυτό το χαρακτηριστικό αυτήν τη στιγμή δεν καταναλώνεται από το Exchange Online. |
| msExchExtensionCustomAttribute3| X| X| X| Αυτό το χαρακτηριστικό αυτήν τη στιγμή δεν καταναλώνεται από το Exchange Online. |
| msExchExtensionCustomAttribute4| X| X| X| Αυτό το χαρακτηριστικό αυτήν τη στιγμή δεν καταναλώνεται από το Exchange Online. |
| msExchExtensionCustomAttribute5| X| X| X| Αυτό το χαρακτηριστικό αυτήν τη στιγμή δεν καταναλώνεται από το Exchange Online. |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| msExchRecipientTypeDetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| msOrg IsOrganizational|  |  | X|  |
| objectSID| X|  | X| η ιδιότητα μηχανικό. Αναγνωριστικό χρήστη AD που χρησιμοποιείται για τη διατήρηση συγχρονισμού μεταξύ Azure AD και AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| Τηλεειδοποίηση| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| "Ταχυδρομικός κώδικας"| X| X|  |  |
| proxyAddresses| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | η ιδιότητα μηχανικό. Χρησιμοποιείται για το πότε πρέπει να ακυρώσει ήδη δημοσιευμένα διακριτικά. Χρησιμοποιείται από συγχρονισμό κωδικού πρόσβασης και federation.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Προέρχεται από groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| η ιδιότητα μηχανικό. Αμετάβλητες αναγνωριστικό για να διατηρήσετε τη σχέση μεταξύ ΠΡΟΣΘΈΤΕΙ και Azure AD.|
| ST| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Τίτλος| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | η ιδιότητα μηχανικό. Χώρα/περιοχή του χρήστη. Χρησιμοποιείται για την εκχώρηση αδειών χρήσης.|
| userCertificate| X| X|  |  |
| userPrincipalName| X|  |  | UPN είναι το Αναγνωριστικό σύνδεσης για το χρήστη. Πιο συχνά την ίδια ως [mail] τιμή.|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>SharePoint Online

| Όνομα χαρακτηριστικού| Χρήστη| Επαφή| Ομάδα| Σχόλιο |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Καθορίζει εάν είναι ενεργοποιημένη η λογαριασμού.|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| από κοινού| X| X|  |  |
| εταιρεία| X| X|  |  |
| Κωδικός| X| X|  |  |
| τμήμα| X| X|  |  |
| Περιγραφή| X| X| X|  |
| Εμφανιζόμενο όνομα| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| hideDLMembership|  |  | X|  |
| ΤηλέφωνοΟικίας| X| X|  |  |
| πληροφορίες| X| X| X|  |
| αρχικά| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| Αλληλογραφία| X| X| X|  |
| mailnickname| X| X| X|  |
| managedBy|  |  | X|  |
| Διαχείριση| X| X|  |  |
| μέλος|  |  | X|  |
| middleName| X| X|  |  |
| κινητές συσκευές| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| η ιδιότητα μηχανικό. Αναγνωριστικό χρήστη AD που χρησιμοποιείται για τη διατήρηση συγχρονισμού μεταξύ Azure AD και AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| Τηλεειδοποίηση| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| "Ταχυδρομικός κώδικας"| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | η ιδιότητα μηχανικό. Χρησιμοποιείται για το πότε πρέπει να ακυρώσει ήδη δημοσιευμένα διακριτικά. Χρησιμοποιείται από συγχρονισμό κωδικού πρόσβασης και federation.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Προέρχεται από groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| η ιδιότητα μηχανικό. Αμετάβλητες αναγνωριστικό για να διατηρήσετε τη σχέση μεταξύ ΠΡΟΣΘΈΤΕΙ και Azure AD.|
| ST| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Τίτλος| X| X|  |  |
| unauthOrig| X| X| X|  |
| διεύθυνση URL| X| X|  |  |
| usageLocation| X|  |  | η ιδιότητα μηχανικό. Χώρα/περιοχή του χρήστη. Χρησιμοποιείται για την εκχώρηση αδειών χρήσης.|
| userPrincipalName| X|  |  | UPN είναι το Αναγνωριστικό σύνδεσης για το χρήστη. Πιο συχνά την ίδια ως [mail] τιμή.|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Στο Lync Online

| Όνομα χαρακτηριστικού| Χρήστη| Επαφή| Ομάδα| Σχόλιο |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Καθορίζει εάν είναι ενεργοποιημένη η λογαριασμού.|
| c| X| X|  |  |
| CN| X|  | X|  |
| από κοινού| X| X|  |  |
| εταιρεία| X| X|  |  |
| τμήμα| X| X|  |  |
| Περιγραφή| X| X| X|  |
| Εμφανιζόμενο όνομα| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| givenName| X| X|  |  |
| ΤηλέφωνοΟικίας| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| Αλληλογραφία| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| Διαχείριση| X| X|  |  |
| μέλος|  |  | X|  |
| κινητές συσκευές| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| msRTCSIP-ApplicationOptions| X|  |  |  |
| msRTCSIP-DeploymentLocator| X| X|  |  |
| msRTCSIP-γραμμής| X| X|  |  |
| msRTCSIP-OptionFlags| X| X|  |  |
| msRTCSIP-OwnerUrn| X|  |  |  |
| msRTCSIP-PrimaryUserAddress| X| X|  |  |
| msRTCSIP-UserEnabled| X| X|  |  |
| objectSID| X|  | X| η ιδιότητα μηχανικό. Αναγνωριστικό χρήστη AD που χρησιμοποιείται για τη διατήρηση συγχρονισμού μεταξύ Azure AD και AD.|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| "Ταχυδρομικός κώδικας"| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | η ιδιότητα μηχανικό. Χρησιμοποιείται για το πότε πρέπει να ακυρώσει ήδη δημοσιευμένα διακριτικά. Χρησιμοποιείται από συγχρονισμό κωδικού πρόσβασης και federation.|
| securityEnabled|  |  | X| Προέρχεται από groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| η ιδιότητα μηχανικό. Αμετάβλητες αναγνωριστικό για να διατηρήσετε τη σχέση μεταξύ ΠΡΟΣΘΈΤΕΙ και Azure AD.|
| ST| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Τίτλος| X| X|  |  |
| usageLocation| X|  |  | η ιδιότητα μηχανικό. Χώρα/περιοχή του χρήστη. Χρησιμοποιείται για την εκχώρηση αδειών χρήσης.|
| userPrincipalName| X|  |  | UPN είναι το Αναγνωριστικό σύνδεσης για το χρήστη. Πιο συχνά την ίδια ως [mail] τιμή.|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure RMS

| Όνομα χαρακτηριστικού| Χρήστη| Επαφή| Ομάδα| Σχόλιο |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Καθορίζει εάν είναι ενεργοποιημένη η λογαριασμού.|
| CN| X|  | X| Κοινές όνομα ή ψευδώνυμο. Πιο συχνά το πρόθεμα της τιμής [mail].|
| Εμφανιζόμενο όνομα| X| X| X| Μια συμβολοσειρά που αντιπροσωπεύει το όνομα συχνά εμφανίζονται ως το φιλικό όνομα (όνομα, Επώνυμο).|
| Αλληλογραφία| X| X| X| πλήρη διεύθυνση ηλεκτρονικού ταχυδρομείου.|
| μέλος|  |  | X|  |
| objectSID| X|  | X| η ιδιότητα μηχανικό. Αναγνωριστικό χρήστη AD που χρησιμοποιείται για τη διατήρηση συγχρονισμού μεταξύ Azure AD και AD.|
| proxyAddresses| X| X| X| η ιδιότητα μηχανικό. Χρησιμοποιείται από το Azure AD. Περιέχει όλα δευτερεύουσες διευθύνσεις ηλεκτρονικού ταχυδρομείου για το χρήστη.|
| pwdLastSet| X|  |  | η ιδιότητα μηχανικό. Χρησιμοποιείται για το πότε πρέπει να ακυρώσει ήδη δημοσιευμένα διακριτικά.|
| securityEnabled|  |  | X| Που προέρχονται από groupType.|
| sourceAnchor| X| X| X| η ιδιότητα μηχανικό. Αμετάβλητες αναγνωριστικό για να διατηρήσετε τη σχέση μεταξύ ΠΡΟΣΘΈΤΕΙ και Azure AD.|
| usageLocation| X|  |  | η ιδιότητα μηχανικό. Χώρα/περιοχή του χρήστη. Χρησιμοποιείται για την εκχώρηση αδειών χρήσης.|
| userPrincipalName| X|  |  | Σε αυτό το UPN είναι το Αναγνωριστικό σύνδεσης για το χρήστη. Πιο συχνά την ίδια ως [mail] τιμή.|

## <a name="intune"></a>Intune

| Όνομα χαρακτηριστικού| Χρήστη| Επαφή| Ομάδα| Σχόλιο |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Καθορίζει εάν είναι ενεργοποιημένη η λογαριασμού.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Περιγραφή| X| X| X|  |
| Εμφανιζόμενο όνομα| X| X| X|  |
| Αλληλογραφία| X| X| X|  |
| mailnickname| X| X| X|  |
| μέλος|  |  | X|  |
| objectSID| X|  | X| η ιδιότητα μηχανικό. Αναγνωριστικό χρήστη AD που χρησιμοποιείται για τη διατήρηση συγχρονισμού μεταξύ Azure AD και AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | η ιδιότητα μηχανικό. Χρησιμοποιείται για το πότε πρέπει να ακυρώσει ήδη δημοσιευμένα διακριτικά. Χρησιμοποιείται από συγχρονισμό κωδικού πρόσβασης και federation.|
| securityEnabled|  |  | X| Προέρχεται από groupType|
| sourceAnchor| X| X| X| η ιδιότητα μηχανικό. Αμετάβλητες αναγνωριστικό για να διατηρήσετε τη σχέση μεταξύ ΠΡΟΣΘΈΤΕΙ και Azure AD.|
| usageLocation| X|  |  | η ιδιότητα μηχανικό. Χώρα/περιοχή του χρήστη. Χρησιμοποιείται για την εκχώρηση αδειών χρήσης.|
| userPrincipalName| X|  |  | UPN είναι το Αναγνωριστικό σύνδεσης για το χρήστη. Πιο συχνά την ίδια ως [mail] τιμή.|

## <a name="dynamics-crm"></a>Dynamics CRM

| Όνομα χαρακτηριστικού| Χρήστη| Επαφή| Ομάδα| Σχόλιο |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Καθορίζει εάν είναι ενεργοποιημένη η λογαριασμού.|
| c| X| X|  |  |
| CN| X|  | X|  |
| από κοινού| X| X|  |  |
| εταιρεία| X| X|  |  |
| Κωδικός| X| X|  |  |
| Περιγραφή| X| X| X|  |
| Εμφανιζόμενο όνομα| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| Διαχείριση| X| X|  |  |
| μέλος|  |  | X|  |
| κινητές συσκευές| X| X|  |  |
| objectSID| X|  | X| η ιδιότητα μηχανικό. Αναγνωριστικό χρήστη AD που χρησιμοποιείται για τη διατήρηση συγχρονισμού μεταξύ Azure AD και AD.|
| physicalDeliveryOfficeName| X| X|  |  |
| "Ταχυδρομικός κώδικας"| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | η ιδιότητα μηχανικό. Χρησιμοποιείται για το πότε πρέπει να ακυρώσει ήδη δημοσιευμένα διακριτικά. Χρησιμοποιείται από συγχρονισμό κωδικού πρόσβασης και federation.|
| securityEnabled|  |  | X| Προέρχεται από groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| η ιδιότητα μηχανικό. Αμετάβλητες αναγνωριστικό για να διατηρήσετε τη σχέση μεταξύ ΠΡΟΣΘΈΤΕΙ και Azure AD.|
| ST| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| Τίτλος| X| X|  |  |
| usageLocation| X|  |  | η ιδιότητα μηχανικό. Χώρα/περιοχή του χρήστη. Χρησιμοποιείται για την εκχώρηση αδειών χρήσης.|
| userPrincipalName| X|  |  | UPN είναι το Αναγνωριστικό σύνδεσης για το χρήστη. Πιο συχνά την ίδια ως [mail] τιμή.|

## <a name="3rd-party-applications"></a>εφαρμογές άλλων κατασκευαστών
Αυτή η ομάδα είναι ένα σύνολο χαρακτηριστικών χρησιμοποιείται ως το ελάχιστο χαρακτηριστικά που χρειάζονται για μια εφαρμογή ή γενικής χρήσης φόρτο εργασίας. Μπορεί να χρησιμοποιηθεί για ένα φόρτο εργασίας δεν αναφέρεται σε μια άλλη ενότητα ή για μια εφαρμογή δεν ανήκουν στη Microsoft. Ρητά χρησιμοποιείται για τα εξής:

- Yammer (καταναλώνεται μόνο χρήστη)
- [Υβριδική επιχειρήσεις επιχειρήσεων (B2B) σταυρό οργανογράμματος σενάρια συνεργασίας που παρέχεται από πόρους, όπως του SharePoint](http://go.microsoft.com/fwlink/?LinkId=747036)

Αυτή η ομάδα είναι ένα σύνολο χαρακτηριστικών που μπορούν να χρησιμοποιηθούν εάν ο κατάλογος Azure AD δεν χρησιμοποιείται για την υποστήριξη του Office 365, Dynamics ή το Intune. Έχει ένα μικρό σύνολο χαρακτηριστικών πυρήνα.

| Όνομα χαρακτηριστικού| Χρήστη| Επαφή| Ομάδα| Σχόλιο |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Καθορίζει εάν είναι ενεργοποιημένη η λογαριασμού.|
| CN| X|  | X|  |
| Εμφανιζόμενο όνομα| X| X| X|  |
| givenName| X| X|  |  |
| Αλληλογραφία| X|  | X|  |
| managedBy|  |  | X|  |
| mailNickName| X| X| X|  |
| μέλος|  |  | X|  |
| objectSID| X|  |  | η ιδιότητα μηχανικό. Αναγνωριστικό χρήστη AD που χρησιμοποιείται για τη διατήρηση συγχρονισμού μεταξύ Azure AD και AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | η ιδιότητα μηχανικό. Χρησιμοποιείται για το πότε πρέπει να ακυρώσει ήδη δημοσιευμένα διακριτικά. Χρησιμοποιείται από συγχρονισμό κωδικού πρόσβασης και federation.|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| η ιδιότητα μηχανικό. Αμετάβλητες αναγνωριστικό για να διατηρήσετε τη σχέση μεταξύ ΠΡΟΣΘΈΤΕΙ και Azure AD.|
| usageLocation| X|  |  | η ιδιότητα μηχανικό. Χώρα/περιοχή του χρήστη. Χρησιμοποιείται για την εκχώρηση αδειών χρήσης.|
| userPrincipalName| X|  |  | UPN είναι το Αναγνωριστικό σύνδεσης για το χρήστη. Πιο συχνά την ίδια ως [mail] τιμή.|

## <a name="windows-10"></a>Windows 10
Μια computer(device) τομέα Windows 10 συγχρονίζει ορισμένα χαρακτηριστικά Azure AD. Για περισσότερες πληροφορίες σχετικά με τα σενάρια, ανατρέξτε στο θέμα [αντιμετωπίζει σύνδεση τομέα συσκευών Azure AD για Windows 10](active-directory-azureadjoin-devices-group-policy.md). Συγχρονισμός πάντα αυτά τα χαρακτηριστικά και Windows 10 δεν θα εμφανίζεται ως μια εφαρμογή που μπορείτε να καταργήσετε. Έναν υπολογιστή Windows 10 τομέα προσδιορίζεται από το χαρακτηριστικό userCertificate συμπληρωμένο χρειάζεται.

| Όνομα χαρακτηριστικού| Συσκευή| Σχόλιο |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| Τιμή οποίος καθορίζεται για υπολογιστές που είναι συνδεδεμένοι τομέα. |
| Εμφανιζόμενο όνομα | X| |
| MS-DS-CreatorSID | X| Ονομάζεται επίσης registeredOwnerReference.|
| objectGUID | X| Ονομάζεται επίσης deviceID.|
| objectSID | X| Ονομάζεται επίσης onPremisesSecurityIdentifier.|
| λειτουργικό σύστημα | X| Ονομάζεται επίσης deviceOSType.|
| operatingSystemVersion | X| Ονομάζεται επίσης deviceOSVersion.|
| userCertificate | X| |

Αυτά τα χαρακτηριστικά για το **χρήστη** είναι εκτός από τις άλλες εφαρμογές που έχετε επιλέξει.  

| Όνομα χαρακτηριστικού| Χρήστη| Σχόλιο |
| --- | :-: | --- |
| domainFQDN| X| Ονομάζεται επίσης dnsDomainName. Για παράδειγμα, contoso.com.|
| domainNetBios| X| Ονομάζεται επίσης netBiosName. Για παράδειγμα, CONTOSO.|

## <a name="exchange-hybrid-writeback"></a>Exchange υβριδική αντιγραφής
Αυτά τα χαρακτηριστικά εγγράφονται πίσω από το Azure AD υπηρεσίας καταλόγου Active Directory εσωτερικής εγκατάστασης όταν επιλέγετε για να ενεργοποιήσετε την **υβριδική ανάπτυξη του Exchange**. Ανάλογα με την έκδοση του Exchange, ενδέχεται να συγχρονιστούν λιγότερα χαρακτηριστικά.

| Όνομα χαρακτηριστικού| Χρήστη| Επαφή| Ομάδα| Σχόλιο |
| --- | :-: | :-: | :-: | --- |
| msDS-ExternalDirectoryObjectID| X|  |  | Που προέρχονται από cloudAnchor στο Azure AD. Το χαρακτηριστικό είναι νέο στο Exchange 2016.|
| msExchArchiveStatus| X|  |  | Ηλεκτρονική αρχειοθήκη: Επιτρέπει στους πελάτες να αρχειοθετήσετε αλληλογραφίας.|
| msExchBlockedSendersHash| X|  |  | Φιλτράρισμα: Γράφει εσωτερικής φιλτράρισμα και online αποστολέα ασφαλείς και Αποκλεισμένοι δεδομένων από προγράμματα-πελάτες.|
| msExchSafeRecipientsHash| X|  |  | Φιλτράρισμα: Γράφει εσωτερικής φιλτράρισμα και online αποστολέα ασφαλείς και Αποκλεισμένοι δεδομένων από προγράμματα-πελάτες.|
| msExchSafeSendersHash| X|  |  | Φιλτράρισμα: Γράφει εσωτερικής φιλτράρισμα και online αποστολέα ασφαλείς και Αποκλεισμένοι δεδομένων από προγράμματα-πελάτες.|
| msExchUCVoiceMailSettings| X|  |  | Ενεργοποίηση της ενοποιημένης ανταλλαγής μηνυμάτων (UM) - Online Φωνητικό ταχυδρομείο: χρησιμοποιείται από το Microsoft Lync Server ενοποίησης για να υποδείξετε στον Lync Server εσωτερικής εγκατάστασης ότι ο χρήστης έχει Φωνητικό ταχυδρομείο σε ηλεκτρονικές υπηρεσίες.|
| msExchUserHoldPolicies| X|  |  | Αναμονής λόγω διενέξεων: Ενεργοποιεί τις υπηρεσίες cloud για να καθορίσετε τους χρήστες που βρίσκονται από κάτω κρατήστε πατημένο το πλήκτρο λόγω διενέξεων.|
| proxyAddresses| X| X| X| Μόνο τα x500 διευθύνσεων από το Exchange Online έχει εισαχθεί.|

## <a name="device-writeback"></a>Συσκευή αντιγραφής
Αντικείμενα συσκευής δημιουργούνται στην υπηρεσία καταλόγου Active Directory. Αυτά τα αντικείμενα μπορεί να συμμετέχει σε Azure AD συσκευές ή τομέα υπολογιστές Windows 10.

| Όνομα χαρακτηριστικού| Συσκευή| Σχόλιο |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| Εμφανιζόμενο όνομα | X| |
| DN | X| |
| msDS-CloudAnchor | X| |
| msDS DeviceID | X| |
| msDS-DeviceObjectVersion | X| |
| msDS-DeviceOSType | X| |
| msDS-DeviceOSVersion | X| |
| msDS-DevicePhysicalIDs | X| |
| msDS-KeyCredentialLink | X| Μόνο με Windows Server 2016 AD σχήματος |
| msDS-IsCompliant | X| |
| msDS-IsEnabled | X| |
| msDS-IsManaged | X| |
| msDS RegisteredOwner | X| |


## <a name="notes"></a>Σημειώσεις

- Όταν χρησιμοποιείτε μια εναλλακτική Αναγνωριστικό, το userPrincipalName χαρακτηριστικό εσωτερικής εγκατάστασης είναι συγχρονισμένο με το Azure AD onPremisesUserPrincipalName χαρακτηριστικό. Το χαρακτηριστικό εναλλακτικό Αναγνωριστικό, για παράδειγμα αλληλογραφία, είναι συγχρονισμένο με το Azure AD userPrincipalName χαρακτηριστικό.
- Στις λίστες παραπάνω, ο τύπος αντικειμένου **χρήστη** ισχύει επίσης για τον τύπο του αντικειμένου **iNetOrgPerson**.

## <a name="next-steps"></a>Επόμενα βήματα
Μάθετε περισσότερα σχετικά με τη ρύθμιση παραμέτρων [Azure AD Connect συγχρονισμός](active-directory-aadconnectsync-whatis.md) .

Μάθετε περισσότερα σχετικά με την [ενσωμάτωση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).
