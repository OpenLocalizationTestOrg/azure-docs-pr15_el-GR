<properties
    pageTitle="Σύνδεση συσκευών τομέα με Azure AD για Windows 10 αντιμετωπίζει | Microsoft Azure"
    description="Εξηγεί πώς οι διαχειριστές μπορούν να ρυθμίσετε τις παραμέτρους μιας πολιτικής ομάδας για να ενεργοποιήσετε τις συσκευές για να συνδεθεί τομέα στο εταιρικό δίκτυο."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Σύνδεση συσκευών τομέα με Azure AD για Windows 10 εμπειρίες

Συμμετοχή σε τομέα είναι η εταιρείες παραδοσιακό τρόπο έχετε συνδέσει συσκευές για την εργασία για τα τελευταία 15 έτη και πολλά άλλα. Έχει ενεργοποιηθεί στους χρήστες να συνδεθείτε για να τις συσκευές χρησιμοποιώντας την εργασία τους Windows Server υπηρεσίας καταλόγου Active Directory (Active Directory) ή σχολικών λογαριασμών και να επιτρέπεται IT για τη Διαχείριση πλήρως αυτές τις συσκευές. Εταιρείες συνήθως βασίζονται σε απεικόνισης μεθόδους για συσκευές προμήθεια στους χρήστες και να χρησιμοποιήσετε γενικά συστήματος κέντρο ρύθμισης παραμέτρων Manager (SCCM) ή μια πολιτική ομάδας για να διαχειριστείτε τους.

Συμμετοχή σε τομέα στα Windows 10 παρέχει τα ακόλουθα πλεονεκτήματα μετά τη σύνδεση συσκευών στο Azure Active Directory (Azure AD):

- Καθολικής σύνδεσης (SSO) με πόρους Azure AD από οπουδήποτε
- Πρόσβαση στην εταιρεία του Windows Store, χρησιμοποιώντας εταιρικό ή σχολικό λογαριασμούς (δεν απαιτείται λογαριασμός Microsoft)
- Για μεγάλες επιχειρήσεις συμβατών περιαγωγής των ρυθμίσεων χρήστη όλες τις συσκευές χρησιμοποιώντας εταιρικό ή σχολικό λογαριασμούς (δεν απαιτείται λογαριασμός Microsoft)
- Αυστηρό έλεγχο ταυτότητας και εύκολη εισόδου για εταιρικό ή σχολικό λογαριασμούς με το Microsoft Passport και Windows Γεια σας
- Δυνατότητα για να περιορίσετε την πρόσβαση μόνο σε συσκευές που συμμορφώνονται με τις ρυθμίσεις πολιτικής ομάδας εταιρικό συσκευής

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Συμμετοχή σε τομέα εξακολουθεί να είναι χρήσιμες. Ωστόσο, για να λάβετε τα οφέλη Azure AD SSO, περιαγωγής ρυθμίσεις εργασίας ή σχολείου λογαριασμούς και πρόσβασης του Windows Store με εταιρικό ή σχολικό λογαριασμούς, θα χρειαστείτε τα εξής:

- Azure AD συνδρομή
- Azure AD Connect για να επεκτείνετε τον κατάλογο εσωτερικής εγκατάστασης για να Azure AD
- Πολιτική που έχει οριστεί να συνδεθείτε τομέα συσκευές Azure AD
- Windows 10 Δόμηση (build 10551 ή νεότερη έκδοση) για συσκευές

Για να ενεργοποιήσετε Microsoft Passport εργασίας και Windows Γεια σας, θα πρέπει επίσης τα εξής:

- Υποδομή δημόσιου κλειδιού (PKI) για το χρήστη έκδοσης πιστοποιητικών.
- Διαχείριση ομάδας παραμέτρων Κέντρου συστήματος έκδοσης 1509 για Technical Preview. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Microsoft συστήματος κέντρο ρύθμισης παραμέτρων Manager Technical Preview](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) και το [Ιστολόγιο ομάδας Διαχείριση ομάδας παραμέτρων Κέντρου συστήματος](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx). Αυτό είναι απαραίτητο για την ανάπτυξη των πιστοποιητικών χρήστη που βασίζεται σε Microsoft Passport πλήκτρα.

Ως εναλλακτική λύση για να την απαίτηση ανάπτυξης PKI, μπορείτε να κάνετε τα εξής:

- Έχετε μερικές ελεγκτές τομέα με Windows Server 2016 Active Directory υπηρεσίες τομέα της.

Για να ενεργοποιήσετε την πρόσβαση υπό όρους, μπορείτε να δημιουργήσετε ρυθμίσεις πολιτικής ομάδας που επιτρέπει την πρόσβαση σε συσκευές τομέα με χωρίς επιπλέον αναπτύξεις. Για να διαχειριστείτε έλεγχο πρόσβασης βάσει συμμόρφωσης της συσκευής, θα πρέπει τα εξής:

- Διαχείριση ομάδας παραμέτρων Κέντρου συστήματος έκδοσης 1509 για Technical Preview για Passport σενάρια

## <a name="deployment-instructions"></a>Οδηγίες ανάπτυξης



### <a name="step-1-deploy-azure-active-directory-connect"></a>Βήμα 1: Ανάπτυξη καταλόγου Azure Active Directory σύνδεση

Azure AD Connect θα σας επιτρέψει να παρέχετε υπολογιστές εσωτερικής εγκατάστασης ως αντικείμενα συσκευής στο cloud. Για να αναπτύξετε Azure AD Connect, ανατρέξτε στις "Εγκατάσταση Azure AD Connect" στο άρθρο [ενσωμάτωση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md#install-azure-ad-connect).

 - Στην περίπτωση που ακολουθήσατε μια [προσαρμοσμένη εγκατάσταση για Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md) (όχι την εγκατάσταση Express), στη συνέχεια, ακολουθήστε τη διαδικασία **Δημιουργία σύνδεσης υπηρεσίας οδηγούν στην υπηρεσία καταλόγου Active Directory εσωτερικής εγκατάστασης**, παρακάτω σε αυτό το βήμα.
 - Εάν έχετε μια ρύθμιση παραμέτρων ομόσπονδη με Azure AD πριν από την εγκατάσταση του Azure AD Connect (για παράδειγμα, εάν έχετε αναπτύξει υπηρεσίες Active Directory Federation Services (AD FS) πριν από την), στη συνέχεια, ακολουθήστε τη διαδικασία **Ρύθμιση παραμέτρων AD FS διεκδίκηση κανόνες** , παρακάτω σε αυτό το βήμα.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Δημιουργήστε ένα σημείο σύνδεσης της υπηρεσίας καταλόγου Active Directory εσωτερικής εγκατάστασης

Συσκευές τομέα θα χρησιμοποιήσει το σημείο σύνδεσης υπηρεσίας για να ανακαλύψετε πληροφορίες για το μισθωτή Azure AD τη στιγμή της αυτόματης καταχώρησης με την υπηρεσία δήλωσης Azure συσκευή.

Στο διακομιστή Azure AD Connect, εκτελέστε τις ακόλουθες εντολές του PowerShell:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Όταν εκτελείται το cmdlet $aadAdminCred = Get-διαπιστευτηρίων, χρησιμοποιήστε τη μορφή *user@example.com* για το όνομα χρήστη από τα διαπιστευτήρια που έχει εισαχθεί, όταν εμφανιστεί το αναδυόμενο παράθυρο Get-διαπιστευτηρίων.

Όταν εκτελείται το cmdlet προετοιμασία ADSyncDomainJoinedComputerSync..., αντικαταστήστε το [*όνομα λογαριασμού σύνδεσης*] με το λογαριασμό στον τομέα που χρησιμοποιείται ως το λογαριασμό σύνδεσης της υπηρεσίας καταλόγου Active Directory.

#### <a name="configure-ad-fs-claim-rules"></a>Ρύθμιση παραμέτρων AD FS διεκδίκηση κανόνων
Ρύθμιση παραμέτρων των κανόνων διεκδίκηση AD FS επιτρέπει τη στιγμιαία καταχώρηση ενός υπολογιστή με υπηρεσία δήλωσης Azure συσκευή, επιτρέποντας υπολογιστές για τον έλεγχο ταυτότητας με χρήση του Kerberos/NTLM μέσω AD FS. Χωρίς αυτό το βήμα, υπολογιστές θα σας βοηθήσουν να Azure AD σε Καθυστερημένο, (υπόκεινται Azure AD Connect συγχρονισμού φορές).

>[AZURE.NOTE]
Εάν δεν έχετε AD FS ως την Ομοσπονδία διακομιστή εσωτερικής εγκατάστασης, ακολουθήστε τις οδηγίες του προμηθευτή σας για να δημιουργήσετε τη διεκδίκηση κανόνες.

Στο διακομιστή AD FS (ή στην περίοδο λειτουργίας συνδεδεμένοι με το διακομιστή AD FS), εκτελέστε τις ακόλουθες εντολές του PowerShell:

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Windows 10 υπολογιστές θα κάνει έλεγχο ταυτότητας με χρήση του Windows ενσωματωμένη του ελέγχου ταυτότητας active τελικού σημείου WS αξιοπιστίας φιλοξενούνται από AD FS. Βεβαιωθείτε ότι είναι ενεργοποιημένη αυτό το τελικό σημείο. Εάν χρησιμοποιείτε το διακομιστή μεσολάβησης ελέγχου ταυτότητας Web, βεβαιωθείτε επίσης ότι αυτό το τελικό σημείο δημοσιεύεται μέσω του διακομιστή μεσολάβησης. Μπορείτε να το κάνετε αυτό, επιλέγοντας το adfs/υπηρεσίες/αξιοπιστίας/13/windowstransport. Θα πρέπει να εμφανίζεται ως ενεργοποιημένα στην κονσόλα διαχείρισης της AD FS στην περιοχή **υπηρεσία** > **τελικά σημεία**.


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Βήμα 2: Ρύθμιση αυτόματων συσκευή εγγραφής μέσω μιας πολιτικής ομάδας στην υπηρεσία καταλόγου Active Directory

Μπορείτε να χρησιμοποιήσετε πολιτικής ομάδας στην υπηρεσία καταλόγου Active Directory για να ρυθμίσετε τις παραμέτρους του Windows 10 συσκευών τομέα για την αυτόματη καταχώρηση με Azure AD.

> [AZURE.NOTE]
> Για να δείτε πιο πρόσφατες οδηγίες σχετικά με τον τρόπο ρύθμισης των αυτόματων συσκευή εγγραφής, [πώς μπορείτε να ρυθμίσετε την αυτόματη καταχώρηση του τομέα Windows συνδεθεί συσκευές με Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> Αυτό το πρότυπο πολιτικής ομάδας έχει μετονομαστεί στα Windows 10. Εάν χρησιμοποιείτε το εργαλείο πολιτικής ομάδας από έναν υπολογιστή Windows 10, η πολιτική θα εμφανίζεται ως: <br>
> **Καταχώρηση υπολογιστές τομέα συνδεθεί ως συσκευές**<br>
> Η πολιτική βρίσκεται στην εξής θέση:<br>
> ***Δήλωση στοιχείων/συσκευή υπολογιστή ρύθμισης παραμέτρων/πολιτικές/διαχείρισης πρότυπα/των Windows***


## <a name="additional-information"></a>Πρόσθετες πληροφορίες
* [Windows 10 για την εταιρεία: τρόποι για να χρησιμοποιήσετε συσκευές για εργασία](active-directory-azureadjoin-windows10-devices-overview.md)
* [Επέκταση cloud δυνατότητες σε συσκευές με Windows 10 έως συμμετοχή Azure Active Directory](active-directory-azureadjoin-user-upgrade.md)
* [Μάθετε σχετικά με τη χρήση σενάρια για το Azure AD συμμετοχή](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Σύνδεση συσκευών τομέα με Azure AD για Windows 10 εμπειρίες](active-directory-azureadjoin-devices-group-policy.md)
* [Ρύθμιση του Azure AD συμμετοχή](active-directory-azureadjoin-setup.md)
