<properties
    pageTitle="Αλλαγή υπογραφής κατακερματισμός αλγόριθμο για Office 365 απάντηση αξιοπιστίας πάρτι | Microsoft Azure"
    description="Αυτή η σελίδα παρέχει οδηγίες για την αλλαγή του αλγόριθμου SHA για Ομοσπονδία αξιοπιστίας με το Office 365"
    keywords="SHA1, SHA256, O365, ομοσπονδίας, aadconnect, adfs, ad fs, αλλαγή sha, αξιοπιστίας ομοσπονδίας, βασίζεστε αξιοπιστίας πάρτι"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="samueld"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="change-signature-hash-algorithm-for-office-365-replying-party-trust"></a>Αλλαγή υπογραφής αλγόριθμος κλειδώματος για το Office 365 απάντηση αξιοπιστίας πάρτι

## <a name="overview"></a>Επισκόπηση

Azure υπηρεσίες Active Directory Federation Services (AD FS), τα διακριτικά στο Microsoft Azure Active Directory για να βεβαιωθείτε ότι δεν μπορεί να τροποποιηθεί με τοποθετείται. Αυτή η υπογραφή μπορεί να βασίζονται σε SHA1 ή SHA256. Azure Active Directory υποστηρίζει πλέον τα διακριτικά συνδεθεί με έναν αλγόριθμο SHA256 και, συνιστάται να ρύθμιση αλγόριθμο υπογραφής διακριτικού SHA256 για το υψηλότερο επίπεδο ασφάλειας. Σε αυτό το άρθρο περιγράφει τα βήματα που απαιτούνται για να ορίσετε τον αλγόριθμο υπογραφής διακριτικού σε την πιο ασφαλή SHA256 επιπέδου.

## <a name="change-the-token-signing-algorithm"></a>Αλλαγή του αλγόριθμου υπογραφής διακριτικού

Αφού ορίσετε αλγόριθμο υπογραφής με μία από τις παρακάτω δύο διεργασίες, AD FS υπογράφει τα διακριτικά για το Office 365 βασίζεστε αξιοπιστίας πάρτι με SHA256. Δεν χρειάζεται να κάνετε αλλαγές επιπλέον ρύθμισης παραμέτρων και αυτή η αλλαγή δεν έχει καμία επίδραση τη δυνατότητά σας για να αποκτήσετε πρόσβαση στο Office 365 ή άλλες εφαρμογές Azure AD.

### <a name="ad-fs-management-console"></a>Κονσόλα διαχείρισης AD FS

1. Ανοίξτε την Κονσόλα διαχείρισης AD FS στον πρωτεύοντα διακομιστή AD FS.
2. Αναπτύξτε τον κόμβο AD FS και κάντε κλικ στην επιλογή **Βασίζεστε εμπιστεύεται κατασκευαστή**.
3. Κάντε δεξί κλικ αξιοπιστίας πάρτι σας Office 365/Azure υπολογιστή και επιλέξτε **Ιδιότητες**.
4. Επιλέξτε την καρτέλα **για προχωρημένους** και επιλέξτε αλγόριθμος ασφαλούς κλειδώματος SHA256.
5. Κάντε κλικ στο **κουμπί OK**.

![Αλγόριθμο υπογραφής SHA256--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>Cmdlet του FS PowerShell AD

1. Σε οποιαδήποτε διακομιστής υπηρεσιών AD FS, ανοίξτε το PowerShell με δικαιώματα διαχειριστή.
2. Ορίστε αλγόριθμος ασφαλούς κλειδώματος, χρησιμοποιώντας το cmdlet **Set-AdfsRelyingPartyTrust** .

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Διαβάστε επίσης

* [Επιδιόρθωση του Office 365 αξιοπιστίας με Azure AD Connect](./active-directory-aadconnect-federation-management.md#repairing-the-trust)
