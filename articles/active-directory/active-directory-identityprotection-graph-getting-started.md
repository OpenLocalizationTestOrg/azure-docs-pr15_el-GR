<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure Active Directory ταυτότητας προστασία και το Microsoft Graph | Microsoft Azure"
    description="Παρέχει μια εισαγωγή σχετικά με το Microsoft Graph ερωτήματος για μια λίστα με τα συμβάντα κινδύνου και σχετικές πληροφορίες από το Azure Active Directory."
    services="active-directory"
    keywords="προστασία ταυτότητας καταλόγου Azure active directory, κινδύνου, ευπάθεια, πολιτική ασφαλείας, Microsoft Graph"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Γρήγορα αποτελέσματα με το Azure Active Directory ταυτότητας προστασίας και το Microsoft Graph

Το Microsoft Graph είναι τελικού σημείου ενοποιημένου API της Microsoft και κεντρική [Azure Active Directory προστασία ταυτότητας του](active-directory-identityprotection.md) APIs. Το πρώτο API, **identityRiskEvents**, σας επιτρέπει να ερωτήματος Microsoft Graph για μια λίστα με [τα συμβάντα κινδύνου](active-directory-identityprotection-risk-events-types.md) και σχετικές πληροφορίες. Σε αυτό το άρθρο σάς παρουσιάζει υποβολή ερωτημάτων αυτό το API. Για μια σε βάθος εισαγωγή, πλήρης τεκμηρίωση και της πρόσβασης στην Εξερεύνηση γραφήματος, ανατρέξτε στην [τοποθεσία Microsoft Graph](https://graph.microsoft.io/).

Υπάρχουν τρία βήματα για την πρόσβαση σε δεδομένα προστασία ταυτότητας μέσω του Microsoft Graph:

1. Προσθέστε μια εφαρμογή με ένα μυστικό προγράμματος-πελάτη. 

2. Χρησιμοποιήστε αυτό το μυστικό και μερικά άλλα κομμάτια πληροφορίες για τον έλεγχο ταυτότητας στο Microsoft Graph, όπου εμφανίζεται ένα διακριτικό έλεγχο ταυτότητας. 

3. Χρησιμοποιήστε αυτό το διακριτικό να κάνουν αιτήσεις για το τελικό σημείο API και να επιστρέψετε προστασία ταυτότητας δεδομένων.

Πριν ξεκινήσετε, θα πρέπει:

- Δικαιώματα διαχειριστή για να δημιουργήσετε την εφαρμογή σε Azure AD
- Το όνομα του τομέα σας του μισθωτή (για παράδειγμα, contoso.onmicrosoft.com)



## <a name="add-an-application-with-a-client-secret"></a>Προσθήκη μιας εφαρμογής με ένα μυστικό προγράμματος-πελάτη


1. [Πραγματοποιήστε είσοδο](https://manage.windowsazure.com) στην πύλη Azure κλασική ως διαχειριστής. 

1. Στην στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**. 

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **εφαρμογές**.

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής ανάπτυξη την εταιρεία μου**.

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)


5. Στο παράθυρο διαλόγου **πείτε μας σχετικά με την εφαρμογή σας** , ακολουθήστε τα παρακάτω βήματα:

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)

    μια. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε ένα όνομα για την εφαρμογή σας (π.χ.: εφαρμογή API συμβάν κινδύνου AADIP).

    β. Ως **Τύπος**, επιλέξτε **εφαρμογή ή/και Web API Web**.

    c. Κάντε κλικ στο κουμπί **Επόμενο**.


5. Στο παράθυρο διαλόγου **Ιδιότητες εφαρμογής** , ακολουθήστε τα παρακάτω βήματα:

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)

    μια. Στο πλαίσιο κειμένου **Εισόδου στη διεύθυνση URL** , πληκτρολογήστε `http://localhost`.

    β. Στο πλαίσιο κειμένου **URI Αναγνωριστικό εφαρμογής** , πληκτρολογήστε `http://localhost`.

    c. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.


Μπορείτε να ρυθμίσετε τώρα την εφαρμογή σας.

![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)



## <a name="grant-your-application-permission-to-use-the-api"></a>Εκχωρείτε το δικαίωμα εφαρμογής για να χρησιμοποιήσετε το API


1. Στη σελίδα της εφαρμογής σας, στο μενού στο επάνω μέρος, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**. 

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)

2. Στην ενότητα **δικαιώματα σε άλλες εφαρμογές** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής**.

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)


2. Στο παράθυρο διαλόγου **δικαιώματα σε άλλες εφαρμογές** , ακολουθήστε τα παρακάτω βήματα:

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)

    μια. Επιλέξτε **Microsoft Graph**.

    β. Κάντε κλικ στην επιλογή **Ολοκλήρωση**.


1. Κάντε κλικ στην επιλογή **δικαιώματα εφαρμογής: 0**, και, στη συνέχεια, επιλέξτε **διαβάσετε όλες τις πληροφορίες συμβάντων κινδύνου ταυτότητα**.

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)

1. Κάντε κλικ στην επιλογή " **Αποθήκευση** " στο κάτω μέρος της σελίδας.

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)


## <a name="get-an-access-key"></a>Λάβετε ένα πλήκτρο πρόσβασης

1. Στη σελίδα της εφαρμογής σας, στην ενότητα **πλήκτρα** , επιλέξτε 1 έτος ως διάρκεια.

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)

1. Κάντε κλικ στην επιλογή " **Αποθήκευση** " στο κάτω μέρος της σελίδας.

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

1. στην ενότητα πλήκτρα, αντιγράψτε την τιμή του αριθμού-κλειδιού που έχουν δημιουργηθεί πρόσφατα και, στη συνέχεια, να την επικολλήσετε σε ασφαλή θέση.

    ![Τη δημιουργία μιας εφαρμογής](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)

    > [AZURE.NOTE] Εάν χάσετε αυτό το κλειδί, θα πρέπει να επιστρέψετε σε αυτήν την ενότητα και δημιουργήστε ένα νέο αριθμό-κλειδί. Διατήρηση αυτού του πλήκτρου έναν μυστικό: μπορεί να οποιονδήποτε έχει πρόσβαση στα δεδομένα σας.


1. Στην ενότητα **Ιδιότητες** , αντιγράψτε το **Αναγνωριστικό υπολογιστή-πελάτη**και, στη συνέχεια, να την επικολλήσετε σε ασφαλή θέση. 


## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Έλεγχος ταυτότητας στο Microsoft Graph και ερωτήματος το API ταυτότητας κινδύνου συμβάντα

Σε αυτό το σημείο, θα πρέπει να έχετε:

- Το Αναγνωριστικό πελάτη που αντιγράψατε παραπάνω

- Το κλειδί που αντιγράψατε παραπάνω

- Το όνομα του τομέα σας του μισθωτή


Για τον έλεγχο ταυτότητας, στείλτε μια πρόσκληση σε δημοσίευση για να `https://login.microsoft.com` με τις ακόλουθες παραμέτρους στο κύριο σώμα του:

- grant_type: "**client_credentials**"

- πόρων: "**https://graph.microsoft.com**"

- client_id:<your client ID>

- client_secret:<your key>


> [AZURE.NOTE] Πρέπει να παράσχετε τιμές για το **client_id** και την παράμετρο **client_secret** .

Εάν είναι επιτυχής, αυτό επιστρέφει ένα διακριτικό έλεγχο ταυτότητας.  
Για να καλέσετε το API, δημιουργήστε μια επικεφαλίδα με την παράμετρο παρακάτω:

    `Authorization`=”<token_type> <access_token>"


Κατά τον έλεγχο ταυτότητας, μπορείτε να βρείτε τον τύπο διακριτικού και το διακριτικό πρόσβασης στο επιστρεφόμενο διακριτικό.

Αποστολή αυτή η κεφαλίδα ως μια αίτηση για να την παρακάτω διεύθυνση URL API:`https://graph.microsoft.com/beta/identityRiskEvents`

Η απόκριση, εάν είναι επιτυχής, είναι μια συλλογή από συμβάντα κινδύνου ταυτότητας και σχετικών δεδομένων με τη μορφή OData JSON, που μπορούν να αναλυθούν και να γίνεται ως ανατρέξτε στο θέμα Προσαρμογή.

Παρακάτω θα δείτε δείγματα κώδικα για τον έλεγχο ταυτότητας και καλεί το API χρήση του Powershell.  
Απλώς προσθέστε το Αναγνωριστικό υπολογιστή-πελάτη, βασικές και μισθωτή τομέα.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Επόμενα βήματα

Συγχαρητήρια, που μόλις δημιουργήσατε την πρώτη κλήση στο Microsoft Graph!  
Τώρα μπορείτε να ερωτήματος συμβάντα κινδύνου ταυτότητας και να χρησιμοποιήσετε τα δεδομένα, ωστόσο, ανατρέξτε στο θέμα προσαρμόζετε.

Για να μάθετε περισσότερα σχετικά με το Microsoft Graph και πώς μπορείτε να δημιουργήσετε εφαρμογές με χρήση του API Graph, ελέγξτε την [τεκμηρίωση](https://graph.microsoft.io/docs) και πολύ πιο στην [τοποθεσία Microsoft Graph](https://graph.microsoft.io/). Επίσης, βεβαιωθείτε ότι για να προσθέσετε σελιδοδείκτη στη σελίδα [προστασία Azure AD ταυτότητας API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) που παραθέτει όλα τα API προστασίας ταυτότητας που είναι διαθέσιμες στο γράφημα. Καθώς προσθέσουμε νέοι τρόποι εργασίας με προστασία ταυτότητας μέσω API, θα βλέπετε τους σε αυτήν τη σελίδα.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Προστασία ταυτότητας καταλόγου Azure Active Directory](active-directory-identityprotection.md)

- [Τύποι συμβάντα κινδύνου εντοπιστεί από το Azure Active Directory ταυτότητας προστασίας](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graph](https://graph.microsoft.io/)

- [Επισκόπηση της Microsoft Graph](https://graph.microsoft.io/docs)

- [Azure AD ταυτότητας προστασίας υπηρεσίας ρίζας](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)
