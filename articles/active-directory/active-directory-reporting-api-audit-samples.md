<properties
    pageTitle="Azure Active Directory αναφοράς ελέγχου δείγματα API | Microsoft Azure"
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
    ms.date="09/28/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-reporting-audit-api-samples"></a>Azure Active Directory ελέγχου API δείγματα αναφορών

Αυτό το θέμα είναι μέρος μιας συλλογής θεμάτων σχετικά με το Azure Active Directory αναφοράς API.  
Azure AD αναφοράς σάς παρέχει ένα API που σας επιτρέπει να αποκτήσετε πρόσβαση σε δεδομένα ελέγχου με χρήση κώδικα ή σχετικά εργαλεία.
Η εμβέλεια αυτού του θέματος είναι να σας παρέχει δείγμα κώδικα για το **API ελέγχου**.

Ανατρέξτε στα θέματα:

- Για περισσότερες πληροφορίες εννοιολογική [αρχείων καταγραφής ελέγχου](active-directory-reporting-azure-portal.md#audit-logs)

- [Γρήγορα αποτελέσματα με το Azure Active Directory αναφοράς API](active-directory-reporting-api-getting-started.md) για περισσότερες πληροφορίες σχετικά με το API αναφοράς.

Για ερωτήσεις, θέματα ή σχόλια, επικοινωνήστε με την [Αναφορά συμβάλλει AAD](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Μπορείτε να χρησιμοποιήσετε τα δείγματα σε αυτό το θέμα, πρέπει να ολοκληρώσετε τις [προϋποθέσεις για την πρόσβαση του Azure AD αναφοράς API](active-directory-reporting-api-prerequisites.md).  
  

## <a name="known-issue"></a>Γνωστό πρόβλημα

Εφαρμογή Auth δεν θα λειτουργεί εάν είναι μισθωτή σας στην περιοχή ΕΕ. Χρησιμοποιήστε έλεγχο ταυτότητας χρήστη για πρόσβαση σε το API ελέγχου ως λύση, μέχρι να μας διορθώσετε το πρόβλημα. 


## <a name="powershell-script"></a>Δέσμη ενεργειών του PowerShell
    # This script will require registration of a Web Application in Azure Active Directory (see https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)

    # Constants
    $ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a Globally Unique ID (registered by Global Admin)
    $ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client Key/Secret string
    $loginURL       = "https://login.microsoftonline.com"     # AAD Instance; for example https://login.microsoftonline.com
    $tenantdomain   = "your-tenant-domain.onmicrosoft.com"    # AAD Tenant; for example, contoso.onmicrosoft.com
    $resource       = "https://graph.windows.net"             # Azure AD Graph API resource URI
    $7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
    Write-Output "Searching for events starting $7daysago"

    # Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    # Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
    if ($oauth.access_token -ne $null) {   
        $i=0
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
        $url = 'https://graph.windows.net/' + $tenantdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago

        # loop through each query page (1 through n)
        Do{
            # display each event on the console window
            Write-Output "Fetching data using Uri: $url"
            $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
            foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
                Write-Output ($event | ConvertTo-Json)
            }
        
            # save the query page to an output file
            Write-Output "Save the output to a file audit$i.json"
            $myReport.Content | Out-File -FilePath audit$i.json -Force
            $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
            $i = $i+1
        } while($url -ne $null)
    } else {
        Write-Host "ERROR: No Access Token"
        }

    Write-Host "Press any key to continue ..."
    $x = $host.UI.RawUI.ReadKey("NoEcho,IncludeKeyDown")


### <a name="executing-the-powershell-script"></a>Εκτέλεση της δέσμης ενεργειών του PowerShell
Όταν ολοκληρώσετε την επεξεργασία της δέσμης ενεργειών, εκτελέστε το και βεβαιωθείτε ότι τα αναμενόμενα δεδομένα από τον έλεγχο καταγράφει αναφοράς επιστρέφεται.

Η δέσμη ενεργειών επιστρέφει εξόδου από την αναφορά ελέγχου στη μορφή JSON. Επίσης, δημιουργεί μια `audit.json` αρχείο με το ίδιο αποτέλεσμα. Μπορείτε να πειραματιστείτε με την τροποποίηση της δέσμης ενεργειών για την επιστροφή δεδομένων από άλλες αναφορές και σχολιάζετε τις μορφές εξόδου που δεν χρειάζεστε.


## <a name="bash-script"></a>Πάρτι δέσμης ενεργειών

    #!/bin/bash

    # Author: Ken Hoff (kenhoff@microsoft.com)
    # Date: 2015.08.20
    # NOTE: This script requires jq (https://stedolan.github.io/jq/)

    CLIENT_ID="your-application-client-id-here"         # Should be a ~35 character string insert your info here
    CLIENT_SECRET="your-application-client-secret-here" # Should be a ~44 character string insert your info here
    LOGIN_URL="https://login.windows.net"
    TENANT_DOMAIN="your-directory-name-here.onmicrosoft.com"    # For example, contoso.onmicrosoft.com

    TOKEN_INFO=$(curl -s --data-urlencode "grant_type=client_credentials" --data-urlencode "client_id=$CLIENT_ID" --data-urlencode "client_secret=$CLIENT_SECRET" "$LOGIN_URL/$TENANT_DOMAIN/oauth2/token?api-version=1.0")

    TOKEN_TYPE=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.token_type')
    ACCESS_TOKEN=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.access_token')

    # get yesterday's date

    YESTERDAY=$(date --date='1 day ago' +'%Y-%m-%d')

    URL="https://graph.windows.net/$TENANT_DOMAIN/activities/audit?api-version=beta&$filter=eventTime%20gt%20$YESTERDAY"


    REPORT=$(curl -s --header "Authorization: $TOKEN_TYPE $ACCESS_TOKEN" $URL)

    echo $REPORT | ./jq-win64.exe -r '.value' | ./jq-win64.exe -r ".[]"

## <a name="python-script"></a>Python δέσμης ενεργειών

    # Author: Michael McLaughlin (michmcla@microsoft.com)
    # Date: January 20, 2016
    # This requires the Python Requests module: http://docs.python-requests.org

    import requests
    import datetime
    import sys

    client_id = 'your-application-client-id-here'
    client_secret = 'your-application-client-secret-here'
    login_url = 'https://login.windows.net/'
    tenant_domain = 'your-directory-name-here.onmicrosoft.com'

    # Get an OAuth access token
    bodyvals = {'client_id': client_id,
                'client_secret': client_secret,
                'grant_type': 'client_credentials'}

    request_url = login_url + tenant_domain + '/oauth2/token?api-version=1.0'
    token_response = requests.post(request_url, data=bodyvals)

    access_token = token_response.json().get('access_token')
    token_type = token_response.json().get('token_type')

    if access_token is None or token_type is None:
        print "ERROR: Couldn't get access token"
        sys.exit(1)

    # Use the access token to make the API request
    yesterday = datetime.date.strftime(datetime.date.today() - datetime.timedelta(days=1), '%Y-%m-%d')

    header_params = {'Authorization': token_type + ' ' + access_token}
    request_string = 'https://graph.windows.net/' + tenant_domain + 'activities/audit?api-version=beta&$filter=eventTime%20gt%20' + yesterday   
    response = requests.get(request_string, headers = header_params)

    if response.status_code is 200:
        print response.content
    else:
        print 'ERROR: API request failed'





## <a name="next-steps"></a>Επόμενα βήματα

- Θα θέλατε να προσαρμόσετε τα δείγματα σε αυτό το θέμα; Δείτε το [Azure Active Directory ελέγχου API αναφοράς](active-directory-reporting-api-audit-reference.md). 

- Εάν θέλετε να δείτε μια πλήρη επισκόπηση της χρήσης του Azure Active Directory αναφοράς API, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure Active Directory αναφοράς API](active-directory-reporting-api-getting-started.md).

- Εάν θέλετε να μάθετε περισσότερα σχετικά με τη δημιουργία αναφορών Azure Active Directory, ανατρέξτε στο θέμα [Azure Active Directory αναφοράς Οδηγός](active-directory-reporting-guide.md).  
