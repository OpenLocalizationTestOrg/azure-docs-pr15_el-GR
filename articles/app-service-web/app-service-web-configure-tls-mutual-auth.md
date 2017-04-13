<properties 
    pageTitle="Πώς μπορείτε να ρυθμίσετε τις παραμέτρους TLS αμοιβαία ελέγχου ταυτότητας για την εφαρμογή Web" 
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους του web app για να χρησιμοποιήσετε τον έλεγχο ταυτότητας πιστοποιητικού προγράμματος-πελάτη σε TLS." 
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/08/2016" 
    ms.author="naziml"/>    

# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>Πώς μπορείτε να ρυθμίσετε τις παραμέτρους TLS αμοιβαία ελέγχου ταυτότητας για την εφαρμογή Web

## <a name="overview"></a>Επισκόπηση ##
Μπορείτε να περιορίσετε την πρόσβαση σε εφαρμογή Azure web με διαφορετικούς τύπους ελέγχου ταυτότητας για την ενεργοποίηση. Είναι ένας τρόπος να το κάνετε αυτό για τον έλεγχο ταυτότητας με ένα πιστοποιητικό προγράμματος-πελάτη όταν η αίτηση είναι μέσω TLS/SSL. Αυτός ο μηχανισμός ονομάζεται TLS αμοιβαία ελέγχου ταυτότητας ή τον έλεγχο ταυτότητας και σε αυτό το άρθρο θα περιγράφει λεπτομερώς πώς μπορείτε να εγκαταστήσετε την εφαρμογή web της για να χρησιμοποιήσετε τον έλεγχο ταυτότητας πιστοποιητικού προγράμματος-πελάτη πιστοποιητικό προγράμματος-πελάτη.

> **Σημείωση:** Εάν έχετε πρόσβαση την τοποθεσία σας μέσω HTTP και HTTPS δεν, δεν θα λαμβάνετε τυχόν πιστοποιητικό προγράμματος-πελάτη. Επομένως, εάν η εφαρμογή σας απαιτεί πιστοποιητικά προγράμματος-πελάτη που δεν πρέπει να επιτρέπει αιτήσεις στην εφαρμογή σας μέσω HTTP.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="configure-web-app-for-client-certificate-authentication"></a>Ρύθμιση παραμέτρων του Web App για τον έλεγχο ταυτότητας πιστοποιητικού προγράμματος-πελάτη ##
Για να ρυθμίσετε την εφαρμογή web της ώστε να απαιτεί πιστοποιητικά προγράμματος-πελάτη, πρέπει να προσθέσετε τη ρύθμιση τοποθεσίας clientCertEnabled για την εφαρμογή web της και να τη ρυθμίσετε στην τιμή true. Αυτή η ρύθμιση δεν είναι διαθέσιμη προς το παρόν έως την εμπειρία διαχείρισης στην πύλη και το REST API θα πρέπει να χρησιμοποιηθεί για να πραγματοποιήσετε αυτές.

Μπορείτε να χρησιμοποιήσετε το [εργαλείο ARMClient](https://github.com/projectkudu/ARMClient) ώστε να μπορείτε εύκολα να δημιουργήσει την κλήση REST API. Μόλις συνδεθείτε με το εργαλείο θα πρέπει να εισαγάγετε την ακόλουθη εντολή:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose
    
αντικαθιστώντας τα πάντα σε {} με πληροφορίες για την εφαρμογή web και τη δημιουργία ενός αρχείου που ονομάζεται enableclientcert.json με την εξής JSON περιεχομένου:

> {"θέση": "Web App της τοποθεσίας μου",   
>   "Ιδιότητες": {  
>     "clientCertEnabled": true}}  

Βεβαιωθείτε ότι για να αλλάξετε την τιμή "θέση" όπου βρίσκεται η εφαρμογή web της π.χ. Βόρεια κεντρική ΜΑΣ ή Δυτική ΗΠΑ κ.λπ.

> **Σημείωση:** Εάν εκτελείτε ARMClient από Powershell, θα πρέπει να διαφυγής το @ σύμβολο για το αρχείο JSON με μια πίσω υποδιαίρεσης '.

## <a name="accessing-the-client-certificate-from-your-web-app"></a>Πρόσβαση σε το πιστοποιητικό προγράμματος-πελάτη από την εφαρμογή Web ##
Εάν χρησιμοποιείτε το ASP.NET και ρύθμιση παραμέτρων την εφαρμογή σας για να χρησιμοποιήσετε τον έλεγχο ταυτότητας πιστοποιητικού προγράμματος-πελάτη, το πιστοποιητικό θα είναι διαθέσιμες μέσω της ιδιότητας **HttpRequest.ClientCertificate** . Για άλλες στοίβες εφαρμογής, θα είναι διαθέσιμα στην εφαρμογή μέσω μιας τιμής κωδικοποίηση base64 στην κεφαλίδα "X-ΈΧΟΥΝ-ClientCert" αίτηση του πιστοποιητικού προγράμματος-πελάτη. Η εφαρμογή σας να δημιουργήσετε ένα πιστοποιητικό από αυτήν την τιμή και, στη συνέχεια, να το χρησιμοποιήσετε για τον έλεγχο ταυτότητας και εξουσιοδότηση σκοπούς στην εφαρμογή σας.

## <a name="special-considerations-for-certificate-validation"></a>Ειδικά ζητήματα για την επικύρωση πιστοποιητικών ##
Το πιστοποιητικό προγράμματος-πελάτη που έχει σταλεί στην εφαρμογή δεν θα περνούν από καμία επικύρωση από την πλατφόρμα Azure Web Apps. Επικύρωση αυτό το πιστοποιητικό είναι ευθύνη της εφαρμογής web. Ακολουθεί δείγμα ASP.NET κώδικα που επικυρώσει Ιδιότητες πιστοποιητικού για σκοπούς ελέγχου ταυτότητας.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;
                
                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;
                
                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
