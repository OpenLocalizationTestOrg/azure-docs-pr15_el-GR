<properties
    pageTitle="Azure Active Directory PowerShell προεπισκόπηση cmdlet για Διαχείριση ομάδας στο Azure AD | Microsoft Azure"
    description="Αυτή η σελίδα παρέχει παραδείγματα του PowerShell για να σας βοηθήσει να διαχειριστείτε τις ομάδες σας στην υπηρεσία καταλόγου Azure Active Directory"
    keywords="Azure AD, Azure Active Directory, PowerShell, ομάδες, ομάδα διαχείρισης"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure Active Directory προεπισκόπηση cmdlet για Διαχείριση ομάδας

> [AZURE.SELECTOR]
- [Πύλη του Azure](active-directory-groups-create-azure-portal.md)
- [Azure κλασική πύλη](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Το ακόλουθο έγγραφο θα σας δώσει με παραδείγματα του τρόπου χρήσης του PowerShell για να διαχειριστείτε τις ομάδες στο Azure Active Directory (Azure AD).  Παρέχει επίσης πληροφορίες σχετικά με τον τρόπο για να ρυθμίσετε με τη λειτουργική μονάδα Azure AD PowerShell preview. Αρχικά, πρέπει να [κάνετε λήψη της λειτουργικής μονάδας Azure AD PowerShell](http://go.microsoft.com/fwlink/p/?LinkId=828627).

## <a name="installing-the-azure-ad-powershell-module"></a>Κατά την εγκατάσταση της λειτουργικής μονάδας Azure AD PowerShell

Για να εγκαταστήσετε τη λειτουργική μονάδα προεπισκόπηση AzureAD PowerShell, χρησιμοποιήστε τις παρακάτω εντολές:

    PS C:\Windows\system32> install-module azureadpreview

Για να βεβαιωθείτε ότι έχει εγκατασταθεί στη λειτουργική μονάδα preview, χρησιμοποιήστε την ακόλουθη εντολή:

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

Τώρα μπορείτε να ξεκινήσετε να χρησιμοποιείτε τα cmdlet στη λειτουργική μονάδα. Για μια πλήρη περιγραφή των τα cmdlet στη λειτουργική μονάδα AzureAD Preview, ανατρέξτε στην [τεκμηρίωση online αναφοράς](https://msdn.microsoft.com/library/azure/mt757216.aspx).

## <a name="connecting-to-the-directory"></a>Σύνδεση με τον κατάλογο
Πριν να ξεκινήσετε τη διαχείριση ομάδων χρησιμοποιώντας Azure AD PowerShell cmdlet για προεπισκόπηση, πρέπει να συνδέσετε την περίοδο λειτουργίας PowerShell στον κατάλογο που θέλετε να διαχειριστείτε. Για να κάνετε αυτό, χρησιμοποιήστε την ακόλουθη εντολή:

    PS C:\Windows\system32> Connect-AzureAD -Force

Το cmdlet θα γίνεται ερώτηση για τα διαπιστευτήρια που θέλετε να χρησιμοποιήσετε για να αποκτήσετε πρόσβαση στον κατάλογο. Σε αυτό το παράδειγμα, χρησιμοποιούμε karen@drumkit.onmicrosoft.com για πρόσβαση στον κατάλογο επίδειξης. Το cmdlet θα επιστρέψει μια επιβεβαίωσης για να εμφανίσετε την περίοδο λειτουργίας συνδέθηκε με επιτυχία στον κατάλογό σας:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Τώρα μπορείτε να ξεκινήσετε να χρησιμοποιείτε τα cmdlet προεπισκόπηση AzureAD για τη διαχείριση ομάδων στον κατάλογό σας.

## <a name="retrieving-groups"></a>Ανάκτηση ομάδων
Για να ανακτήσετε υπάρχουσες ομάδες από τον κατάλογο, μπορείτε να χρησιμοποιήσετε το cmdlet Get-AzureADGroups. Για να ανακτήσετε όλες τις ομάδες στον κατάλογο, χρησιμοποιήστε το cmdlet χωρίς παραμέτρους:

    PS C:\Windows\system32> get-azureadgroup

Το cmdlet θα επιστρέψει όλες τις ομάδες του συνδεδεμένου καταλόγου.

Μπορείτε να χρησιμοποιήσετε την παράμετρο - objectID για να ανακτήσετε μια συγκεκριμένη ομάδα για την οποία καθορίζετε objectID της ομάδας:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Το cmdlet τώρα θα επιστρέψει την ομάδα των οποίων objectID συμφωνεί με την τιμή της παραμέτρου που πληκτρολογήσατε:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Μπορείτε να κάνετε αναζήτηση για μια συγκεκριμένη ομάδα με χρήση του - παράμετρος φίλτρου. Αυτή η παράμετρος λαμβάνει έναν όρο φίλτρου ODATA και επιστρέφει όλες τις ομάδες που ταιριάζουν με το φίλτρο, όπως στο ακόλουθο παράδειγμα:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Σημειώστε ότι τα cmdlet του AzureAD PowerShell υλοποιήσετε το πρότυπο ερωτήματος OData, μπορείτε να βρείτε περισσότερες πληροφορίες [εδώ](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Δημιουργία ομάδων
Για να δημιουργήσετε μια νέα ομάδα στον κατάλογό σας, χρησιμοποιήστε το cmdlet New-AzureADGroup. Αυτό το cmdlet δημιουργεί μια νέα ομάδα ασφαλείας που ονομάζεται "Μάρκετινγκ":

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Ενημέρωση ομάδων
Για να ενημερώσετε μια υπάρχουσα ομάδα, χρησιμοποιήστε το cmdlet Set-AzureADGroup. Σε αυτό το παράδειγμα, θα σας πρόκειται να αλλάξετε την ιδιότητα εμφανιζόμενο όνομα της ομάδας "Διαχειριστές Intune." Πρώτα, θα σας βρει την ομάδα χρησιμοποιώντας το cmdlet Get-AzureADGroup και να φιλτράρετε χρησιμοποιώντας το χαρακτηριστικό εμφανιζόμενο όνομα:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Στη συνέχεια, θα σας να αλλάξετε την ιδιότητα περιγραφή για τη νέα τιμή "Διαχειριστές συσκευή Intune":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Τώρα εάν θα σας βρείτε την ομάδα ξανά, μπορούμε να δούμε την ιδιότητα περιγραφή ενημερώνεται για να απεικονίσει τη νέα τιμή:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Διαγραφή ομάδων
Για να διαγράψετε ομάδες από τον κατάλογο, χρησιμοποιήστε το cmdlet κατάργηση AzureADGroup ως εξής:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Διαχείριση μελών των ομάδων
Εάν θέλετε να προσθέσετε νέα μέλη σε μια ομάδα, χρησιμοποιήστε το cmdlet Προσθήκη AzureADGroupMember. Αυτή η εντολή προσθέτει ένα μέλος της ομάδας διαχειριστών Intune χρησιμοποιήσαμε στο προηγούμενο παράδειγμα:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Η παράμετρος - ObjectId Αναγνωριστικό_αντικειμένου της ομάδας στην οποία θέλετε να προσθέσετε ένα μέλος και το RefObjectId - Αναγνωριστικό_αντικειμένου του χρήστη για να προσθέσετε ως μέλος στην ομάδα.

Για να λάβετε τα υπάρχοντα μέλη της ομάδας, χρησιμοποιήστε το cmdlet Get-AzureADGroupMember, όπως σε αυτό το παράδειγμα:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Για να καταργήσετε το μέλος που έχουμε προσθέσει προηγουμένως στην ομάδα, χρησιμοποιήστε το cmdlet κατάργηση AzureADGroupMember, όπως εμφανίζεται εδώ:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Για να επαληθεύσετε το membership(s) ομάδας ενός χρήστη, χρησιμοποιήστε το cmdlet επιλέξτε AzureADGroupIdsUserIsMemberOf. Αυτό το cmdlet λαμβάνει ως παραμέτρων της Αναγνωριστικό_αντικειμένου του χρήστη για τον οποίο θα έλεγχος των μελών της ομάδας και μια λίστα των ομάδων για την οποία θα έλεγχος των μελών. Τη λίστα των ομάδων πρέπει να είναι που παρέχονται στη φόρμα μιας μεταβλητής μιγαδικός του τύπου "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck", επομένως, πρώτα πρέπει να δημιουργήσουμε μια μεταβλητή με αυτόν τον τύπο:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Στη συνέχεια, παρέχουμε τιμές για το groupIds να μεταβιβάσετε τον έλεγχο το χαρακτηριστικό "GroupIds" αυτής της μεταβλητής μιγαδικού:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Τώρα, εάν θέλουμε να έλεγχος των μελών ομάδας ενός χρήστη με 72cd4bbd-2594-40a2-935c-016f3cfeeeea ObjectID σύμφωνα με τις ομάδες στο $g, θα πρέπει να χρησιμοποιήσουμε:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Η τιμή που επιστρέφεται είναι μια λίστα των ομάδων από τις οποίες αυτός ο χρήστης είναι μέλος. Μπορείτε επίσης να εφαρμόσετε αυτήν τη μέθοδο για να ελέγξετε τις επαφές, ομάδες ή υπηρεσία αρχές συμμετοχή ως μέλος για μια δεδομένη λίστα των ομάδων, χρησιμοποιώντας την επιλογή AzureADGroupIdsContactIsMemberOf, επιλέξτε AzureADGroupIdsGroupIsMemberOf ή επιλέξτε AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="managing-owners-of-groups"></a>Διαχείριση κατόχων των ομάδων
Για να προσθέσετε κατόχους σε μια ομάδα, χρησιμοποιήστε το cmdlet Προσθήκη AzureADGroupOwner:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Η παράμετρος - ObjectId Αναγνωριστικό_αντικειμένου της ομάδας στην οποία θέλετε να προσθέσετε έναν κάτοχο και την RefObjectId - Αναγνωριστικό_αντικειμένου του χρήστη για να προσθέσετε ως έναν κάτοχο της ομάδας.

Για να ανακτήσετε οι κάτοχοι μιας ομάδας, χρησιμοποιήστε το Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Το cmdlet θα επιστρέψει τη λίστα των κατόχων για την καθορισμένη ομάδα:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Εάν θέλετε να καταργήσετε έναν κάτοχο από μια ομάδα, χρησιμοποιήστε κατάργηση AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να βρείτε περισσότερες τεκμηρίωση Azure Active Directory PowerShell στο [Cmdlet του Azure Active Directory](http://go.microsoft.com/fwlink/p/?LinkId=808260).

* [Διαχείριση της πρόσβασης σε πόρους με ομάδες Azure Active Directory](active-directory-manage-groups.md)

* [Ενοποίηση του ταυτοτήτων εσωτερικής εγκατάστασης με το Azure Active Directory](active-directory-aadconnect.md)
