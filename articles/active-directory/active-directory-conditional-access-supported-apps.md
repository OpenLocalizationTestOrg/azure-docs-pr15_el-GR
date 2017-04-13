<properties
    pageTitle="Εφαρμογές που χρησιμοποιούν τους κανόνες υπό όρους πρόσβασης στην υπηρεσία καταλόγου Azure Active Directory | Microsoft Azure"
    description="Με τον έλεγχο πρόσβασης υπό όρους, Azure Active Directory ελέγχει για συγκεκριμένες συνθήκες όταν το πραγματοποιεί έλεγχο ταυτότητας χρήστη και να επιτρέψετε την πρόσβαση εφαρμογής."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Εφαρμογές που χρησιμοποιούν τους κανόνες υπό όρους πρόσβασης στην υπηρεσία καταλόγου Azure Active Directory

Κανόνες υπό όρους πρόσβασης που υποστηρίζονται στο Azure Active Directory (Azure AD)-συνδεδεμένοι εφαρμογές, προ-ενσωματωμένες ομόσπονδη λογισμικού ως ένα εφαρμογών υπηρεσίας (ΑΠΑ), οι εφαρμογές που χρησιμοποιούν τον κωδικό πρόσβασης καθολικής σύνδεσης (SSO), γραμμή επιχειρηματικές εφαρμογές και εφαρμογές που χρησιμοποιούν το διακομιστή μεσολάβησης εφαρμογής Azure AD. Για μια λεπτομερή λίστα των εφαρμογών για το οποίο μπορείτε να χρησιμοποιήσετε υπό όρους πρόσβασης, ανατρέξτε στο θέμα [υπηρεσίες με δυνατότητα με πρόσβαση υπό όρους](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access). Υπό όρους πρόσβασης λειτουργεί τόσο με φορητούς και επιτραπέζιους εφαρμογές που χρησιμοποιούν Σύγχρονος έλεγχος ταυτότητας. Σε αυτό το άρθρο, καλύπτονται τα θέματα υπό όρους πώς λειτουργεί πρόσβαση στις εφαρμογές φορητούς και επιτραπέζιους.

Μπορείτε να χρησιμοποιήσετε Azure AD εισόδου σελίδες σε εφαρμογές που χρησιμοποιούν Σύγχρονος έλεγχος ταυτότητας. Με μια σελίδα εισόδου, ζητείται ένα χρήστη για τον έλεγχο ταυτότητας πολλών παραγόντων. Εμφανίζεται ένα μήνυμα αν έχει αποκλειστεί πρόσβασης του χρήστη. Μοντέρνα απαιτείται έλεγχος ταυτότητας για τη συσκευή για τον έλεγχο ταυτότητας με Azure AD, ώστε να υπολογιστούν οι πολιτικές πρόσβασης υπό όρους που βασίζεται σε συσκευή.

Είναι σημαντικό να γνωρίζετε ποιες εφαρμογές μπορούν να χρησιμοποιήσουν τους κανόνες υπό όρους πρόσβασης και τα βήματα που ίσως χρειαστεί να ακολουθήσετε για την ασφάλιση άλλα σημεία εισόδου εφαρμογής.

## <a name="applications-that-use-modern-authentication"></a>Εφαρμογές που χρησιμοποιούν Σύγχρονος έλεγχος ταυτότητας

> [AZURE.NOTE] Εάν έχετε μια πολιτική πρόσβασης υπό όρους σε Azure AD που έχει ένα ισοδύναμο στο Office 365, ρυθμίστε τις παραμέτρους και στις δύο πολιτικές πρόσβασης υπό όρους. Αυτό θα εφαρμοστεί, για παράδειγμα, σε πολιτικές υπό όρους πρόσβασης για το Exchange Online ή το SharePoint Online.

Οι ακόλουθες εφαρμογές υποστήριξη υπό όρους πρόσβασης για το Office 365 και άλλες εφαρμογές υπηρεσιών Azure AD με σύνδεση:

| Υπηρεσία προορισμού  | Πλατφόρμα  | Εφαρμογή                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Exchange Online του Office 365 | Windows 10|Εφαρμογή αλληλογραφίας/ημερολόγιο/επαφές, Outlook 2016, Outlook 2013 (με Σύγχρονος έλεγχος ταυτότητας), το Skype για επιχειρήσεις (με Σύγχρονος έλεγχος ταυτότητας)|
|Exchange Online του Office 365| Windows 8.1, Windows 7 |Outlook 2016, Outlook 2013 (με Σύγχρονος έλεγχος ταυτότητας), το Skype για επιχειρήσεις (με Σύγχρονος έλεγχος ταυτότητας)|
|Exchange Online του Office 365|iOS, Android|  Εφαρμογή Outlook για κινητές συσκευές|
|Exchange Online του Office 365|Mac OS X| Outlook 2016 για έλεγχο ταυτότητας πολλών παραγόντων και θέση. υποστήριξη πολιτικής που βασίζονται σε συσκευή που έχει σχεδιαστεί για το μέλλον, Skype για επιχειρήσεις υποστήριξης που έχει σχεδιαστεί για το μέλλον|
|SharePoint Online στο Office 365|Windows 10| Εφαρμογές του Office 2016, εφαρμογές του Office καθολικής, Office 2013 (με Σύγχρονος έλεγχος ταυτότητας), του OneDrive για επιχειρήσεις app (επόμενο πρόγραμμα-πελάτη συγχρονισμού γενιάς ή NGSC) υποστηρίζουν προγραμματισμένες για το μέλλον, υποστήριξη ομάδες του Office που έχει σχεδιαστεί για το μέλλον, υποστήριξη εφαρμογών του SharePoint που έχει σχεδιαστεί για το μέλλον|
|SharePoint Online στο Office 365|Windows 8.1, Windows 7|Εφαρμογές του Office 2016, του Office 2013 (με Σύγχρονος έλεγχος ταυτότητας), εφαρμογή OneDrive για επιχειρήσεις (Groove προγράμματος-πελάτη συγχρονισμού)|
|SharePoint Online στο Office 365|iOS, Android|  Εφαρμογές του Office mobile |
|SharePoint Online στο Office 365|Mac OS X| Εφαρμογές του Office 2016 για έλεγχο ταυτότητας πολλών παραγόντων και θέση. υποστήριξη πολιτικής που βασίζονται σε συσκευή που έχει σχεδιαστεί για το μέλλον|
|Office 365 Yammer|Windows 10, iOS και Android | Εφαρμογή Yammer του Office|
|Dynamics CRM|Windows 10, Windows 8.1, Windows 7, iOS και Android | Εφαρμογή Dynamics CRM|
|Υπηρεσία PowerBI|Windows 10, Windows 8.1, Windows 7, iOS και Android | Εφαρμογή PowerBI|
|Azure απομακρυσμένης εφαρμογής υπηρεσίας|Windows 10, Windows 8.1, Windows 7, iOS, Android και Mac OS X |Εφαρμογή Azure Remote|
|Οποιαδήποτε υπηρεσία εφαρμογής οι εφαρμογές μου|Android και iOS|Οποιαδήποτε υπηρεσία εφαρμογής οι εφαρμογές μου |


## <a name="applications-that-do-not-use-modern-authentication"></a>Εφαρμογές που δεν χρησιμοποιούν Σύγχρονος έλεγχος ταυτότητας

Προς το παρόν, πρέπει να χρησιμοποιήσετε άλλες μεθόδους για να αποκλείσετε την πρόσβαση σε εφαρμογές που δεν χρησιμοποιούν Σύγχρονος έλεγχος ταυτότητας. Δεν θα εφαρμοστεί τους κανόνες πρόσβασης για τις εφαρμογές που δεν χρησιμοποιείτε Σύγχρονος έλεγχος ταυτότητας από την access υπό όρους. Αυτό είναι κυρίως πρέπει να ληφθούν υπόψη για πρόσβαση Exchange και το SharePoint. Οι περισσότερες παλαιότερες εκδόσεις των εφαρμογών που χρησιμοποιούν παλαιότερα πρωτόκολλα ελέγχου πρόσβασης.

### <a name="control-access-in-office-365-sharepoint-online"></a>Έλεγχος της πρόσβασης στο Office 365 SharePoint Online
Μπορείτε να απενεργοποιήσετε πρωτόκολλα παλαιού τύπου για την πρόσβαση του SharePoint, χρησιμοποιώντας το cmdlet Set-SPOTenant. Χρησιμοποιήστε αυτό το cmdlet για να αποτρέψετε την προγράμματα-πελάτες του Office που χρησιμοποιούν πρωτόκολλα-σύγχρονη ελέγχου ταυτότητας από την πρόσβαση σε πόρους του SharePoint Online.

**Παράδειγμα εντολής**:    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Έλεγχος της πρόσβασης στο Office 365 Exchange Online

Exchange προσφέρει δύο κύριες κατηγορίες πρωτόκολλα. Εξετάστε τις ακόλουθες επιλογές και, στη συνέχεια, επιλέξτε την πολιτική που είναι κατάλληλη για την εταιρεία σας.

-   **Exchange ActiveSync**. Από προεπιλογή, οι πολιτικές υπό όρους πρόσβασης για έλεγχο ταυτότητας πολλών παραγόντων και θέση δεν θα εφαρμοστεί για το Exchange ActiveSync. Πρέπει να προστατεύσετε την πρόσβαση σε αυτές τις υπηρεσίες με ρύθμιση παραμέτρων της πολιτικής Exchange ActiveSync απευθείας, ή με τον αποκλεισμό Exchange ActiveSync με τη χρήση κανόνων υπηρεσίες Active Directory Federation Services (AD FS).
-   **Πρωτόκολλα παλαιού τύπου**. Μπορείτε να αποκλείσετε πρωτόκολλα παλαιού τύπου με AD FS. Αυτό αποκλείει την πρόσβαση σε παλαιότερα προγράμματα-πελάτες Office, όπως το Office 2013 χωρίς σύγχρονα έλεγχος ταυτότητας με δυνατότητα και παλαιότερες εκδόσεις του Office.


### <a name="use-ad-fs-to-block-legacy-protocol"></a>Χρησιμοποιήστε AD FS για να αποκλείσετε πρωτόκολλο παλαιού τύπου

Μπορείτε να χρησιμοποιήσετε τους παρακάτω κανόνες παράδειγμα για τον αποκλεισμό παλαιού τύπου πρωτόκολλο πρόσβασης στο επίπεδο AD FS. Επιλέξτε από δύο συνήθεις ρυθμίσεις παραμέτρων.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Επιλογή 1: Επιτρέπουν Exchange ActiveSync και σε εφαρμογές παλαιού τύπου, αλλά μόνο στο intranet

Με την εφαρμογή τους παρακάτω τρεις κανόνες για την AD FS βασίζεστε αξιοπιστίας κατασκευαστή για πλατφόρμα ταυτότητας του Microsoft Office 365, κίνηση του Exchange ActiveSync, και προγράμματος περιήγησης και την κυκλοφορία Σύγχρονος έλεγχος ταυτότητας, έχετε πρόσβαση. Παλαιού τύπου εφαρμογές που αποκλείονται από το extranet.

##### <a name="rule-1"></a>Κανόνας 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Κανόνας 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Κανόνας 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Επιλογή 2: Επιτρέπει Exchange ActiveSync και αποκλεισμός εφαρμογές παλαιού τύπου

Με την εφαρμογή τους παρακάτω τρεις κανόνες για την AD FS βασίζεστε αξιοπιστίας κατασκευαστή για πλατφόρμα ταυτότητας του Microsoft Office 365, κίνηση του Exchange ActiveSync, και προγράμματος περιήγησης και την κυκλοφορία Σύγχρονος έλεγχος ταυτότητας, έχετε πρόσβαση. Παλαιού τύπου εφαρμογές που αποκλείονται από οποιαδήποτε θέση.

##### <a name="rule-1"></a>Κανόνας 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Κανόνας 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Κανόνας 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
