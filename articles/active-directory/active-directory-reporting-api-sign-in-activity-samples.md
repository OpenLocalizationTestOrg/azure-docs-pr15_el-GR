<properties
    pageTitle="Azure δείγματα αναφορών API εισόδου δραστηριότητας υπηρεσίας καταλόγου Active Directory | Microsoft Azure"
    description="Πώς μπορείτε να ξεκινήσετε με το Azure Active Directory αναφοράς API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure δείγματα αναφορών API εισόδου δραστηριότητας υπηρεσίας καταλόγου Active Directory

Αυτό το θέμα είναι μέρος μιας συλλογής θεμάτων σχετικά με το Azure Active Directory αναφοράς API.  
Azure AD αναφοράς σάς παρέχει ένα API που σας επιτρέπει να έχετε πρόσβαση σε δεδομένα εισόδου δραστηριότητας χρησιμοποιώντας κωδικό ή το σχετικό εργαλεία.  
Η εμβέλεια αυτού του θέματος είναι να σας παρέχει δείγμα κώδικα για την **είσοδο δραστηριότητας API**.

Ανατρέξτε στα θέματα:

- Για περισσότερες πληροφορίες εννοιολογική [αρχείων καταγραφής ελέγχου](active-directory-reporting-azure-portal.md#audit-logs)
- [Γρήγορα αποτελέσματα με το Azure Active Directory αναφοράς API](active-directory-reporting-api-getting-started.md) για περισσότερες πληροφορίες σχετικά με το API αναφοράς.

Για ερωτήσεις, θέματα ή σχόλια, επικοινωνήστε με την [Αναφορά συμβάλλει AAD](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Μπορείτε να χρησιμοποιήσετε τα δείγματα σε αυτό το θέμα, πρέπει να ολοκληρώσετε τις [προϋποθέσεις για την πρόσβαση του Azure AD αναφοράς API](active-directory-reporting-api-prerequisites.md).  


##<a name="powershell-script"></a>Δέσμη ενεργειών του PowerShell

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Εκτέλεση της δέσμης ενεργειών
Όταν ολοκληρώσετε την επεξεργασία της δέσμης ενεργειών, εκτελέστε το και βεβαιωθείτε ότι τα αναμενόμενα δεδομένα από τον έλεγχο καταγράφει αναφοράς επιστρέφεται.

Η δέσμη ενεργειών επιστρέφει εξόδου από την αναφορά σε μορφή JSON εισόδου. Επίσης, δημιουργεί μια `SigninActivities.json` αρχείο με το ίδιο αποτέλεσμα. Μπορείτε να πειραματιστείτε με την τροποποίηση της δέσμης ενεργειών για την επιστροφή δεδομένων από άλλες αναφορές και σχολιάζετε τις μορφές εξόδου που δεν χρειάζεστε.



## <a name="next-steps"></a>Επόμενα βήματα

- Θα θέλατε να προσαρμόσετε τα δείγματα σε αυτό το θέμα; Δείτε το [Azure Active Directory εισόδου δραστηριότητας API αναφοράς](active-directory-reporting-api-sign-in-activity-reference.md). 

- Εάν θέλετε να δείτε μια πλήρη επισκόπηση της χρήσης του Azure Active Directory αναφοράς API, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure Active Directory αναφοράς API](active-directory-reporting-api-getting-started.md).

- Εάν θέλετε να μάθετε περισσότερα σχετικά με τη δημιουργία αναφορών Azure Active Directory, ανατρέξτε στο θέμα [Azure Active Directory αναφοράς Οδηγός](active-directory-reporting-guide.md).  