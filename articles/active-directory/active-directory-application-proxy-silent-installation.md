<properties
    pageTitle="Πώς να εγκαταστήσετε σιωπηρά το Azure Connector διακομιστή μεσολάβησης εφαρμογής AD | Microsoft Azure"
    description="Περιγράφει πώς μπορείτε να εκτελέσετε μια εγκατάσταση χωρίς μηνύματα Azure AD εφαρμογής διακομιστή μεσολάβησης γραμμή σύνδεσης για την παροχή ασφαλούς απομακρυσμένη πρόσβαση στις εφαρμογές εσωτερικής εγκατάστασης."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>Πώς να εγκαταστήσετε σιωπηρά το Azure Connector διακομιστή μεσολάβησης εφαρμογής AD

Θέλετε να έχετε τη δυνατότητα να στείλετε μια δέσμη ενεργειών εγκατάστασης σε πολλούς διακομιστές Windows ή με διακομιστές των Windows που δεν έχετε το περιβάλλον εργασίας χρήστη με δυνατότητα. Αυτό το θέμα εξηγεί πώς μπορείτε να δημιουργήσετε μια δέσμη ενεργειών του Windows PowerShell που επιτρέπει την εγκατάσταση χωρίς παρακολούθηση για να εγκαταστήσετε και να καταχωρήσετε το Azure σύνδεσης διακομιστή μεσολάβησης εφαρμογής AD.

## <a name="enabling-access"></a>Ενεργοποίηση της πρόσβασης
Διακομιστής μεσολάβησης λειτουργεί κατά την εγκατάσταση του μικρού μεγέθους υπηρεσίας Windows Server που ονομάζεται στη γραμμή σύνδεσης στο εσωτερικό σας δίκτυο. Για τη γραμμή σύνδεσης διακομιστή μεσολάβησης εφαρμογής για να εργαστείτε σε αυτό πρέπει να έχει εγγραφεί σε σας καταλόγου Azure AD χρησιμοποιώντας καθολικός διαχειριστής και τον κωδικό πρόσβασης. Συνήθως αυτό καταχωρείται κατά την εγκατάσταση της σύνδεσης στο παράθυρο διαλόγου pop. Εναλλακτικά, μπορείτε να χρησιμοποιήσετε το Windows PowerShell για τη δημιουργία ενός αντικειμένου διαπιστευτηρίων για να εισαγάγετε πληροφορίες εγγραφής σας ή μπορείτε να δημιουργήσετε το δικό σας διακριτικό και να το χρησιμοποιήσετε για να εισαγάγετε τις πληροφορίες εγγραφής.

## <a name="step-1--install-the-connector-without-registration"></a>Βήμα 1: Εγκαταστήστε τη γραμμή σύνδεσης χωρίς εγγραφής


Εγκαταστήστε το MSI γραμμή σύνδεσης χωρίς να δηλώσετε τη γραμμή σύνδεσης ως εξής:


1. Ανοίξτε μια γραμμή εντολών.
2. Εκτελέστε την ακόλουθη εντολή στο οποίο η/q σημαίνει εγκατάσταση χωρίς μηνύματα - η εγκατάσταση θα σας ζητά να αποδεχτείτε την άδεια χρήσης τελικού χρήστη.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Βήμα 2: Καταχώρηση στη γραμμή σύνδεσης με το Azure Active Directory
Αυτό μπορεί να πραγματοποιηθεί χρησιμοποιώντας μία από τις παρακάτω μεθόδους:


- Καταχώρηση στη γραμμή σύνδεσης, χρησιμοποιώντας ένα αντικείμενο διαπιστευτηρίων του Windows PowerShell
- Καταχώρηση στη γραμμή σύνδεσης με έναν κωδικό που δημιουργήσατε για εργασία χωρίς σύνδεση

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Καταχώρηση στη γραμμή σύνδεσης, χρησιμοποιώντας ένα αντικείμενο διαπιστευτηρίων του Windows PowerShell


1. Δημιουργία του αντικειμένου διαπιστευτήρια των Windows PowerShell, εκτελώντας τα εξής, όπου "<username>"και"<password>" θα πρέπει να αντικατασταθεί με το όνομα χρήστη και κωδικό πρόσβασης του καταλόγου σας:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. Μεταβείτε στην **C:\Program Files\Microsoft AAD εφαρμογή διακομιστή μεσολάβησης σύνδεσης** και να εκτελέσετε τη δέσμη ενεργειών χρησιμοποιώντας το αντικείμενο τα διαπιστευτήρια του PowerShell που δημιουργήσατε, όπου $cred είναι το όνομα του το PowerShell διαπιστευτήρια αντικείμενο που δημιουργήσατε:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>Καταχώρηση στη γραμμή σύνδεσης με έναν κωδικό που δημιουργήσατε για εργασία χωρίς σύνδεση

1. Δημιουργήστε ένα διακριτικό χωρίς σύνδεση με την κλάση AuthenticationContext χρησιμοποιώντας τις τιμές στο τμήμα κώδικα:


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. Όταν έχετε ολοκληρώσει το διακριτικό Δημιουργήστε μια SecureString χρησιμοποιώντας το διακριτικό: <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. Εκτελέστε την ακόλουθη εντολή του Windows PowerShell, όπου SecureToken είναι το όνομα του διακριτικού που δημιουργήσατε παραπάνω και tenantID GUID του μισθωτή σας: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>Δείτε επίσης

- [Ενεργοποίηση μεσολάβησης εφαρμογής για την υπηρεσία καταλόγου Azure Active Directory](active-directory-application-proxy-enable.md)
- [Δημοσίευση εφαρμογών χρησιμοποιώντας το δικό σας όνομα τομέα](active-directory-application-proxy-custom-domains.md)
- [Ενεργοποίηση της καθολικής σύνδεσης σε](active-directory-application-proxy-sso-using-kcd.md)
- [Αντιμετώπιση προβλημάτων που αντιμετωπίζετε με το διακομιστή μεσολάβησης εφαρμογής](active-directory-application-proxy-troubleshoot.md)

Για τις πιο πρόσφατες ειδήσεις και ενημερώσεις, ανατρέξτε στο θέμα το [ιστολόγιο του διακομιστή μεσολάβησης εφαρμογής](http://blogs.technet.com/b/applicationproxyblog/)
