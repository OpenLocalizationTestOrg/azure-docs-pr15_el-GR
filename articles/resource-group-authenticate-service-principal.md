<properties
   pageTitle="Δημιουργία Azure υπηρεσίας κεφαλαίου με το PowerShell | Microsoft Azure"
   description="Περιγράφει τον τρόπο χρήσης του Azure PowerShell για τη δημιουργία μιας εφαρμογής υπηρεσίας καταλόγου Active Directory και κεφάλαιο υπηρεσίας και την εκχώρηση πρόσβασης σε πόρους μέσω έλεγχος πρόσβασης βάσει ρόλων. Εμφανίζει τον τρόπο ελέγχου ταυτότητας εφαρμογή με έναν κωδικό πρόσβασης ή πιστοποιητικό."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Χρήση του Azure PowerShell για να δημιουργήσετε ένα κεφάλαιο υπηρεσίας για πρόσβαση σε πόρους

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Πύλη](resource-group-create-service-principal-portal.md)

Όταν έχετε μια εφαρμογή ή δέσμη ενεργειών που πρέπει να αποκτήσετε πρόσβαση σε πόρους, πιθανότατα δεν θέλετε να εκτελέσετε αυτήν τη διαδικασία στην περιοχή τα δικά σας διαπιστευτήρια. Ενδέχεται να έχετε διαφορετικά δικαιώματα που θέλετε για την εφαρμογή και δεν θέλετε η εφαρμογή να συνεχίσετε να χρησιμοποιείτε τα διαπιστευτήριά σας, αν αλλάξουν οι αρμοδιότητές σας. Αντί για αυτό, μπορείτε να δημιουργήσετε μια ταυτότητα για την εφαρμογή που περιλαμβάνει διαπιστευτήρια ελέγχου ταυτότητας και εκχώρησης ρόλου. Κάθε φορά που η εφαρμογή εκτελείται, το πραγματοποιεί έλεγχο ταυτότητας ίδια με αυτά τα διαπιστευτήρια. Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε [Azure PowerShell](powershell-install-configure.md) για να ρυθμίσετε όλα όσα χρειάζεστε για μια εφαρμογή για να εκτελέσετε κάτω από τη δική του διαπιστευτήρια και ταυτότητα.

Με το PowerShell, έχετε δύο επιλογές για τον έλεγχο ταυτότητας εφαρμογή AD σας:

 - κωδικός πρόσβασης
 - πιστοποιητικό

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε και τις δύο επιλογές σε PowerShell. Εάν σκοπεύετε να συνδεθείτε Azure από ένα πλαίσιο προγραμματισμού (όπως Python, φωνητικής γραφής, ή Node.js), ο έλεγχος ταυτότητας κωδικού πρόσβασης μπορεί να είναι η καλύτερη επιλογή. Πριν αποφασίσετε εάν θα χρησιμοποιήσετε έναν κωδικό πρόσβασης ή πιστοποιητικού, ανατρέξτε στην ενότητα [δείγματα εφαρμογών](#sample-applications) για παραδείγματα τον έλεγχο ταυτότητας σε διαφορετική τα πλαίσια.

## <a name="active-directory-concepts"></a>Έννοιες Active Directory

Σε αυτό το άρθρο, μπορείτε να δημιουργήσετε δύο αντικείμενα - εφαρμογή του Active Directory (AD) και η υπηρεσία κεφαλαίου. Η εφαρμογή AD είναι η καθολική αναπαράσταση της εφαρμογής σας. Περιέχει τα διαπιστευτήρια (αναγνωριστικό εφαρμογής και έναν κωδικό πρόσβασης ή πιστοποιητικό). Η υπηρεσία κεφαλαίου είναι το τοπικό αναπαράσταση της εφαρμογής σας σε μια υπηρεσία καταλόγου Active Directory. Περιέχει εκχώρησης ρόλου. Αυτό το θέμα εστιάζει σε μια εφαρμογή μίας μισθωτή, όπου η εφαρμογή προορίζεται για να εκτελέσετε μόνο μία εταιρεία. Συνήθως, χρησιμοποιείτε εφαρμογές μίας μισθωτή για εφαρμογές γραμμής εταιρικά που εκτελούνται εντός της εταιρείας σας. Σε μια εφαρμογή μίας μισθωτή, έχετε μια εφαρμογή του AD και μία υπηρεσία κεφάλαιο.

Ίσως αναρωτιέστε - Γιατί χρειάζομαι και τα δύο αντικείμενα; Αυτή η προσέγγιση διευκολύνει περισσότερες Όταν σκέφτεστε πολλών μισθωτών εφαρμογές. Χρησιμοποιείτε συνήθως πολλών μισθωτών εφαρμογές για το λογισμικό-ως-a-(ΑΠΑ) εφαρμογών υπηρεσίας, όπου εκτελείται η εφαρμογή σας σε πολλές διαφορετικές συνδρομές. Για εφαρμογές πολλών μισθωτών, έχετε μια εφαρμογή του AD και πολλές αρχές υπηρεσίας (μία σε κάθε υπηρεσία καταλόγου Active Directory που εκχωρεί πρόσβαση στην εφαρμογή). Για να ρυθμίσετε μια εφαρμογή πολλών μισθωτών, ανατρέξτε στο θέμα [για προγραμματιστές του οδηγού για την άδεια με το API διαχείρισης πόρων Azure](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Απαιτούμενα δικαιώματα

Για να ολοκληρώσετε αυτό το θέμα, πρέπει να έχετε επαρκή δικαιώματα τόσο το Azure Active Directory και Azure τη συνδρομή σας. Συγκεκριμένα, πρέπει να μπορείτε να δημιουργήσετε μια εφαρμογή στην υπηρεσία καταλόγου Active Directory και να εκχωρήσετε το κεφάλαιο υπηρεσίας σε ένα ρόλο. 

Στην υπηρεσία καταλόγου Active Directory, ο λογαριασμός σας πρέπει να είστε διαχειριστής (όπως **Καθολικού διαχειριστή** ή **Διαχειριστή χρηστών**). Εάν ο λογαριασμός σας έχει εκχωρηθεί ο ρόλος **χρήστη** , πρέπει να έχετε διαχειριστή αυξήσετε τα δικαιώματά σας.

Στη συνδρομή σας, πρέπει να έχετε το λογαριασμό σας `Microsoft.Authorization/*/Write` πρόσβαση, γεγονός που παρέχεται μέσω από το ρόλο [κατόχου](./active-directory/role-based-access-built-in-roles.md#owner) ή το ρόλο [Διαχειριστή πρόσβασης χρήστη](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) . Εάν ο λογαριασμός σας έχει εκχωρηθεί ο ρόλος του **συνεργάτη** , λαμβάνετε ένα μήνυμα σφάλματος κατά την προσπάθεια για να αντιστοιχίσετε το κεφάλαιο υπηρεσίας σε ένα ρόλο. Ξανά, ο διαχειριστής συνδρομή πρέπει να σας εκχωρήσει επαρκή δικαιώματα πρόσβασης.

Τώρα, συνεχίστε με μια ενότητα για [τον κωδικό πρόσβασης](#create-service-principal-with-password) ή [πιστοποιητικό](#create-service-principal-with-certificate) ελέγχου ταυτότητας.

## <a name="create-service-principal-with-password"></a>Δημιουργία της υπηρεσίας κεφαλαίου με κωδικό πρόσβασης

Σε αυτήν την ενότητα, μπορείτε να εκτελέσετε τα βήματα για να:

- Δημιουργία της εφαρμογής AD με κωδικό πρόσβασης
- Δημιουργία του αρχικού υπηρεσίας
- αναθέσει το ρόλο του προγράμματος ανάγνωσης του αρχικού υπηρεσίας

Για να εκτελέσετε γρήγορα αυτά τα βήματα, ανατρέξτε στα παρακάτω τρεις cmdlet. 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Ας δούμε αυτά τα βήματα πιο προσεκτικά για να βεβαιωθείτε ότι γνωρίζετε τη διαδικασία.

1. Πραγματοποιήστε είσοδο στο λογαριασμό σας.

        Add-AzureRmAccount

1. Δημιουργία νέας εφαρμογής υπηρεσίας καταλόγου Active Directory, παρέχοντας ένα εμφανιζόμενο όνομα, το URI που περιγράφει την εφαρμογή σας, το URI που εντοπισμός την εφαρμογή και τον κωδικό πρόσβασης για την ταυτότητα της εφαρμογής.

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"

     Για εφαρμογές μίας μισθωτή, το URIs δεν είναι επικύρωση.
     
     Εάν ο λογαριασμός σας δεν διαθέτει τα [απαραίτητα δικαιώματα](#required-permissions) στην υπηρεσία καταλόγου Active Directory, μπορείτε να δείτε ένα μήνυμα σφάλματος που δηλώνει "Authentication_Unauthorized" ή "βρέθηκε χωρίς συνδρομή στο περιβάλλον".

1. Εξετάστε το αντικείμενο της νέας εφαρμογής. 

        $app
        
     Σημείωση ιδίως της ιδιότητας **αναγνωριστικά εφαρμογής** , η οποία απαιτείται για τη δημιουργία αρχές υπηρεσίας, εκχωρήσεις ρόλων και κατά τη λήψη το διακριτικό πρόσβασης.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}

2. Δημιουργήστε αρχής υπηρεσίας για την εφαρμογή σας, η διαβίβαση το αναγνωριστικό εφαρμογής της εφαρμογής υπηρεσίας καταλόγου Active Directory.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

3. Εκχωρήστε τα δικαιώματα κεφαλαίου υπηρεσίας για τη συνδρομή σας. Σε αυτό το παράδειγμα, μπορείτε να προσθέσετε το κεφάλαιο υπηρεσίας ρόλο **αναγνώστη** , που εκχωρεί δικαιώματα ανάγνωσης όλους τους πόρους στην συνδρομής. Για άλλους ρόλους, ανατρέξτε στο θέμα [RBAC: ενσωματωμένη ρόλους](./active-directory/role-based-access-built-in-roles.md). Για την παράμετρο **ServicePrincipalName** , δώστε τα **αναγνωριστικά εφαρμογής** που χρησιμοποιήσατε κατά τη δημιουργία της εφαρμογής. 

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Εάν ο λογαριασμός σας δεν διαθέτει επαρκή δικαιώματα για να αντιστοιχίσετε ένα ρόλο, θα δείτε ένα μήνυμα σφάλματος. Το μήνυμα αναφέρει το λογαριασμός σας **δεν διαθέτει εξουσιοδότηση για να εκτελέσετε την ενέργεια 'Microsoft.Authorization/roleAssignments/write' πάνω από το εύρος ' / συνδρομές / {guid}'**. 

Αυτό είναι! Την εφαρμογή AD και την υπηρεσία κεφάλαιο έχουν ρυθμιστεί. Η επόμενη ενότητα σας δείχνει πώς μπορείτε να συνδεθείτε με τα διαπιστευτήρια μέσω του PowerShell. Εάν θέλετε να χρησιμοποιήσετε τα διαπιστευτήρια στην εφαρμογή σας κώδικα, μπορείτε να μεταβείτε στις [εφαρμογές του δείγματος](#sample-applications). 

### <a name="provide-credentials-through-powershell"></a>Παρέχει διαπιστευτήρια μέσω του PowerShell

Τώρα, πρέπει να συνδεθείτε ως την εφαρμογή για την εκτέλεση λειτουργιών.

1. Δημιουργήστε ένα αντικείμενο **PSCredential** που περιέχει τα διαπιστευτήριά σας, εκτελέστε την εντολή **Get-διαπιστευτηρίων** . Χρειάζεστε τα **αναγνωριστικά εφαρμογής** πριν από την εκτέλεση αυτής της εντολής επομένως, βεβαιωθείτε ότι έχετε επιλέξει που είναι διαθέσιμο για επικόλληση.

        $creds = Get-Credential

2. Θα σας ζητηθεί να εισαγάγετε τα διαπιστευτήριά σας. Για το όνομα χρήστη, χρησιμοποιήστε τα **αναγνωριστικά εφαρμογής** που χρησιμοποιήσατε κατά τη δημιουργία της εφαρμογής. Για τον κωδικό πρόσβασης, χρησιμοποιήστε το όνομα που καθορίσατε κατά τη δημιουργία του λογαριασμού.

     ![Εισαγάγετε τα διαπιστευτήριά](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

2. Κάθε φορά που συνδέεστε ως αρχής υπηρεσία, πρέπει να παρέχετε το αναγνωριστικό μισθωτή του καταλόγου για την εφαρμογή σας AD. Ένα μισθωτή είναι μια παρουσία της υπηρεσίας καταλόγου Active Directory. Εάν έχετε μόνο μία συνδρομή, μπορείτε να χρησιμοποιήσετε:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Εάν έχετε περισσότερες από μία συνδρομές, καθορίστε τη συνδρομή όπου βρίσκεται το υπηρεσίας καταλόγου Active Directory. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς Azure συνδρομές συσχετίζονται με Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

4. Συνδεθείτε ως το κεφάλαιο υπηρεσίας, καθορίζοντας ότι αυτός ο λογαριασμός είναι αρχής υπηρεσίας και με την παροχή του αντικειμένου διαπιστευτήρια. 

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     Τώρα είστε εξουσιοδοτημένοι ως την κύρια υπηρεσία για την εφαρμογή υπηρεσίας καταλόγου Active Directory που δημιουργήσατε.

### <a name="save-access-token-to-simplify-log-in"></a>Αποθήκευση διακριτικό πρόσβασης για να απλοποιήσετε στο αρχείο καταγραφής

Για να αποφύγετε παρέχοντας τα διαπιστευτήρια κεφαλαίου υπηρεσία κάθε φορά που χρειάζεται να συνδεθείτε στο, μπορείτε να αποθηκεύσετε το διακριτικό πρόσβασης.

1. Για να χρησιμοποιήσετε το τρέχον διακριτικό πρόσβασης σε μια περίοδο λειτουργίας νεότερη έκδοση, αποθηκεύστε το προφίλ.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Ανοίξτε το προφίλ και εξετάστε τα περιεχόμενά της. Παρατηρήστε ότι περιέχει ένα διακριτικό πρόσβασης. 
        
2. Αντί για μη αυτόματη σύνδεση στο ξανά, απλώς να φορτώσετε το προφίλ.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Το διακριτικό πρόσβασης λήξει, ώστε να χρησιμοποιείτε ένα αποθηκευμένο προφίλ λειτουργεί μόνο για όσο διάστημα το διακριτικό δεν είναι έγκυρη.
        
## <a name="create-service-principal-with-certificate"></a>Δημιουργία της υπηρεσίας κεφαλαίου με πιστοποιητικό

Σε αυτήν την ενότητα, μπορείτε να εκτελέσετε τα βήματα για να:

- Δημιουργία πιστοποιητικού αυτόματης υπογραφής
- Δημιουργία της εφαρμογής AD με το πιστοποιητικό
- Δημιουργία του αρχικού υπηρεσίας
- αναθέσει το ρόλο του προγράμματος ανάγνωσης του αρχικού υπηρεσίας

Για να εκτελέσετε αυτά τα βήματα με Azure PowerShell 2.0 γρήγορα σε Windows 10 ή Windows Server 2016 Technical Preview, ανατρέξτε στα παρακάτω cmdlet. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Ας δούμε αυτά τα βήματα πιο προσεκτικά για να βεβαιωθείτε ότι γνωρίζετε τη διαδικασία. Επίσης, αυτό το άρθρο παρουσιάζει πώς μπορείτε να εκτελέσετε τις εργασίες κατά τη χρήση προηγούμενων εκδόσεων του Azure PowerShell ή λειτουργικά συστήματα.

### <a name="create-the-self-signed-certificate"></a>Δημιουργήστε το πιστοποιητικό αυτόματης υπογραφής

Η έκδοση του PowerShell που είναι διαθέσιμες με Windows 10 και Windows Server 2016 Technical Preview περιλαμβάνει μια ενημερωμένη cmdlet **New-SelfSignedCertificate** για τη δημιουργία αυτο-υπογεγραμμένο πιστοποιητικό. Προηγούμενα λειτουργικά συστήματα έχουν το cmdlet New-SelfSignedCertificate αλλά δεν μπορεί να προσφέρει οι παράμετροι που χρειάζονται για αυτό το θέμα. Αντί για αυτό, πρέπει να εισαγάγετε μια λειτουργική μονάδα για τη δημιουργία του πιστοποιητικού. Αυτό το θέμα δείχνει και οι δύο αυτές προσεγγίσεις για τη δημιουργία του πιστοποιητικού με βάση το λειτουργικό σύστημα που έχετε. 

- Εάν έχετε **Windows 10 ή Windows Server 2016 Technical Preview**, εκτελέστε την ακόλουθη εντολή για να δημιουργήσετε ένα πιστοποιητικό αυτόματης υπογραφής: 

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
- Εάν **δεν έχετε Windows 10 ή Windows Server 2016 Technical Preview**, πρέπει να κάνετε λήψη του [αυτο-υπογεγραμμένο πιστοποιητικό γεννήτρια](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) από κέντρο δέσμης ενεργειών της Microsoft. Εξαγάγετε τα περιεχόμενά και εισαγάγετε το cmdlet που χρειάζεστε.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
    
     Στη συνέχεια, δημιουργήστε το πιστοποιητικό.
    
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

Έχετε το πιστοποιητικό σας και να συνεχίσετε με τη δημιουργία της εφαρμογής σας AD.

### <a name="create-the-active-directory-app-and-service-principal"></a>Δημιουργήστε την εφαρμογή υπηρεσίας καταλόγου Active Directory και την υπηρεσία κεφαλαίου

1. Ανακτήστε την τιμή του κλειδιού από το πιστοποιητικό.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

2. Πραγματοποιήστε είσοδο στο λογαριασμό σας στο Azure.

        Add-AzureRmAccount

3. Δημιουργία νέας εφαρμογής υπηρεσίας καταλόγου Active Directory, παρέχοντας ένα εμφανιζόμενο όνομα, το URI που περιγράφει την εφαρμογή σας, το URI που εντοπισμός την εφαρμογή και τον κωδικό πρόσβασης για την ταυτότητα της εφαρμογής.

     Εάν έχετε Azure PowerShell 2.0 (Αύγουστος 2016 ή νεότερη έκδοση), χρησιμοποιήστε το ακόλουθο cmdlet:

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Εάν έχετε Azure PowerShell 1.0, χρησιμοποιήστε το ακόλουθο cmdlet:
    
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Για εφαρμογές μίας μισθωτή, το URIs δεν είναι επικύρωση.
    
    Εάν ο λογαριασμός σας δεν διαθέτει τα [απαραίτητα δικαιώματα](#required-permissions) στην υπηρεσία καταλόγου Active Directory, μπορείτε να δείτε ένα μήνυμα σφάλματος που δηλώνει "Authentication_Unauthorized" ή "βρέθηκε χωρίς συνδρομή στο περιβάλλον".
        
    Εξετάστε το αντικείμενο της νέας εφαρμογής. 

        $app

    Παρατηρήστε ότι η ιδιότητα **αναγνωριστικά εφαρμογής** , που είναι απαραίτητη για τη δημιουργία αρχές υπηρεσίας, εκχωρήσεις ρόλων και κατά τη λήψη διακριτικά πρόσβασης.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}


5. Δημιουργήστε αρχής υπηρεσίας για την εφαρμογή σας, η διαβίβαση το αναγνωριστικό εφαρμογής της εφαρμογής υπηρεσίας καταλόγου Active Directory.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

6. Εκχωρήστε τα δικαιώματα κεφαλαίου υπηρεσίας για τη συνδρομή σας. Σε αυτό το παράδειγμα, μπορείτε να προσθέσετε το κεφάλαιο υπηρεσίας ρόλο **αναγνώστη** , που εκχωρεί δικαιώματα ανάγνωσης όλους τους πόρους στην συνδρομής. Για άλλους ρόλους, ανατρέξτε στο θέμα [RBAC: ενσωματωμένη ρόλους](./active-directory/role-based-access-built-in-roles.md). Για την παράμετρο **ServicePrincipalName** , δώστε τα **αναγνωριστικά εφαρμογής** που χρησιμοποιήσατε κατά τη δημιουργία της εφαρμογής.

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Εάν ο λογαριασμός σας δεν διαθέτει επαρκή δικαιώματα για να αντιστοιχίσετε ένα ρόλο, θα δείτε ένα μήνυμα σφάλματος. Το μήνυμα αναφέρει το λογαριασμός σας **δεν διαθέτει εξουσιοδότηση για να εκτελέσετε την ενέργεια 'Microsoft.Authorization/roleAssignments/write' πάνω από το εύρος ' / συνδρομές / {guid}'**.

Αυτό είναι! Την εφαρμογή AD και την υπηρεσία κεφάλαιο έχουν ρυθμιστεί. Η επόμενη ενότητα σας δείχνει πώς μπορείτε να συνδεθείτε με το πιστοποιητικό μέσω του PowerShell.

### <a name="provide-certificate-through-automated-powershell-script"></a>Παροχή πιστοποιητικού μέσω αυτοματοποιημένη δέσμη ενεργειών του PowerShell

Κάθε φορά που συνδέεστε ως αρχής υπηρεσία, πρέπει να παρέχετε το αναγνωριστικό μισθωτή του καταλόγου για την εφαρμογή σας AD. Ένα μισθωτή είναι μια παρουσία της υπηρεσίας καταλόγου Active Directory. Εάν έχετε μόνο μία συνδρομή, μπορείτε να χρησιμοποιήσετε:

    $tenant = (Get-AzureRmSubscription).TenantId
    
Εάν έχετε περισσότερες από μία συνδρομές, καθορίστε τη συνδρομή όπου βρίσκεται το υπηρεσίας καταλόγου Active Directory. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση του καταλόγου Azure AD](./active-directory/active-directory-administer.md).

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Για τον έλεγχο ταυτότητας στη δέσμη ενεργειών, καθορίστε το λογαριασμό είναι μια υπηρεσία κεφαλαίου και δώστε την αποτύπωση πιστοποιητικού, αναγνωριστικό εφαρμογής και αναγνωριστικό μισθωτή. Για να αυτοματοποιήσετε τη δέσμη ενεργειών σας, μπορείτε να αποθηκεύσετε αυτές τις τιμές ως μεταβλητές περιβάλλοντος και να τις ανακτήσετε κατά την εκτέλεση ή μπορείτε να τις συμπεριλάβετε στη δέσμη ενεργειών σας.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

Τώρα είστε εξουσιοδοτημένοι ως την κύρια υπηρεσία για την εφαρμογή υπηρεσίας καταλόγου Active Directory που δημιουργήσατε.

## <a name="sample-applications"></a>Δείγματα εφαρμογών

Τα ακόλουθα δείγματα εφαρμογών δείχνουν πώς μπορείτε να συνδεθείτε ως το κεφάλαιο υπηρεσίας.

**.NET**

- [Ανάπτυξη μιας SSH με δυνατότητα Εικονική με ένα πρότυπο με το .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Διαχείριση Azure τους πόρους και τις ομάδες πόρων με .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Γρήγορα αποτελέσματα με τους πόρους - ανάπτυξη χρήση προτύπου για τη διαχείριση πόρων Azure - στο Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Γρήγορα αποτελέσματα με τους πόρους - Διαχείριση ομάδας πόρων - στο Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Ανάπτυξη μιας SSH με δυνατότητα Εικονική με ένα πρότυπο στο Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Διαχείριση Azure πόρου και πόρου ομάδες με Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Ανάπτυξη μιας SSH με δυνατότητα Εικονική με ένα πρότυπο στο Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Διαχείριση Azure τους πόρους και τις ομάδες πόρων με Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Κείμενο φωνητικής γραφής**

- [Ανάπτυξη μιας SSH με δυνατότητα Εικονική με ένα πρότυπο στο κείμενο φωνητικής γραφής](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Διαχείριση Azure πόρου και πόρου ομάδες με κείμενο φωνητικής γραφής](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Επόμενα βήματα
  
- Για λεπτομερή βήματα στην ενσωμάτωση μιας εφαρμογής σε Azure για τη διαχείριση των πόρων, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές να εξουσιοδότησης με το API διαχείρισης πόρων Azure](resource-manager-api-authentication.md).
- Για μια πιο λεπτομερή εξήγηση των εφαρμογών και υπηρεσιών αρχές, ανατρέξτε στο θέμα [εφαρμογή και των αντικειμένων κεφάλαιο υπηρεσίας](./active-directory/active-directory-application-objects.md). 
- Για περισσότερες πληροφορίες σχετικά με τον έλεγχο ταυτότητας υπηρεσίας καταλόγου Active Directory, ανατρέξτε στο θέμα [Σενάρια ελέγχου ταυτότητας για Azure AD](./active-directory/active-directory-authentication-scenarios.md).



