<properties
   pageTitle="Azure Active Directory v2.0 ελέγχου ταυτότητας βιβλιοθήκες | Microsoft Azure"
   description="Βιβλιοθήκες συμβατό πρόγραμμα-πελάτη και διακομιστή ενδιάμεσο βιβλιοθήκες, και σχετική Βιβλιοθήκη προέλευσης και συνδέσεις δείγματα, για το τελικό σημείο v2.0 Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="skwan;bryanla"/>


# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory v2.0 ελέγχου ταυτότητας βιβλιοθήκες
Το τελικό σημείο v2.0 Azure Active Directory (Azure AD) υποστηρίζει τα πρωτόκολλα OAuth 2.0 και 1.0 σύνδεση OpenID βιομηχανικών προτύπων. Μπορείτε να χρησιμοποιήσετε διάφορες βιβλιοθήκες από τη Microsoft και άλλες εταιρείες με το τελικό σημείο v2.0.

Όταν δημιουργείτε μια εφαρμογή που χρησιμοποιεί το τελικό σημείο v2.0, συνιστάται να χρησιμοποιήσετε βιβλιοθήκες που έχουν δημιουργηθεί από τους ειδικούς τομέα πρωτόκολλο που παρακολουθούν μεθοδολογία κύκλου ζωής ανάπτυξης ασφαλείας (SDL), όπως [η μία ακολουθούμενο από τη Microsoft][Microsoft-SDL]. Εάν αποφασίσετε να χεριού κώδικα υποστήριξης για τα πρωτόκολλα, συνιστούμε να ακολουθήσετε SDL μεθοδολογία και παρακολουθείτε στενά τα ζητήματα ασφαλείας στην οι προδιαγραφές πρότυπα για κάθε πρωτόκολλο.

## <a name="types-of-libraries"></a>Τύποι βιβλιοθηκών

Azure AD v2.0 λειτουργεί με δύο τύποι βιβλιοθηκών:

- **Βιβλιοθήκες προγράμματος-πελάτη**. Εγγενή προγράμματα-πελάτες και διακομιστές χρήση βιβλιοθηκών προγράμματος-πελάτη για να λάβετε διακριτικά πρόσβασης για την κλήση ενός πόρου, όπως το Microsoft Graph.
- **Βιβλιοθήκες ενδιάμεσο διακομιστή**. Εφαρμογές Web χρήση βιβλιοθηκών ενδιάμεσο διακομιστή για είσοδος χρήστη. Web APIs χρήση βιβλιοθηκών ενδιάμεσο διακομιστή για να επικυρώσει τα διακριτικά που αποστέλλονται με εγγενή προγράμματα-πελάτες ή με άλλους διακομιστές.

## <a name="library-support"></a>Βιβλιοθήκη υποστήριξης
Επειδή μπορείτε να επιλέξετε οποιαδήποτε βιβλιοθήκη συμβατό με τα πρότυπα, όταν χρησιμοποιείτε το τελικό σημείο v2.0, είναι σημαντικό να γνωρίζετε πού να απευθυνθείτε για την υποστήριξη. Για τα θέματα και αιτήσεις για δυνατότητες σε βιβλιοθήκη κώδικα, επικοινωνήστε με τον κάτοχο της βιβλιοθήκης. Για τα θέματα και αιτήσεις για δυνατότητες στην εφαρμογή υπηρεσίας πλευρά πρωτόκολλο, επικοινωνήστε με τη Microsoft.

Βιβλιοθήκες να προέρχονται από δύο κατηγορίες υποστήριξης:

- **Υποστήριξη της Microsoft**. Η Microsoft παρέχει επιδιορθώσεις για αυτές τις βιβλιοθήκες και ολοκληρωθεί SDL επιμέλεια σε αυτές τις βιβλιοθήκες.
- **Συμβατά**. Η Microsoft έχει ελεγχθεί αυτές τις βιβλιοθήκες σε βασικά σενάρια και επιβεβαιώσει ότι λειτουργούν με το τελικό σημείο v2.0. Η Microsoft δεν παρέχει επιδιορθώσεις αυτές τις βιβλιοθήκες και δεν έχει κάνει μια αναθεώρηση από αυτές τις βιβλιοθήκες. Θέματα και αιτήσεις για δυνατότητες θα πρέπει να κατευθύνεται στο έργο προέλευσης άνοιγμα της βιβλιοθήκης.

Για μια λίστα των βιβλιοθηκών που λειτουργούν με το τελικό σημείο v2.0, ανατρέξτε στις επόμενες ενότητες σε αυτό το άρθρο.

## <a name="microsoft-supported-client-libraries"></a>Βιβλιοθήκες που υποστηρίζονται Microsoft προγράμματος-πελάτη
| Πλατφόρμα| Όνομα της βιβλιοθήκης| Λήψη | Πηγαίος κώδικας | Δείγμα |
| :-: | :-: | :-: | :-: | :-: |
| .NET, Windows Store, Xamarin | Έλεγχος ταυτότητας Microsoft βιβλιοθήκης (MSAL) για το .NET | [Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] | [MSAL για .NET (GitHub)][ClientLib-NET-Repo] | [Δείγμα εγγενές πρόγραμμα-πελάτη υπολογιστή των Windows][ClientLib-NET-Sample] |
| Node.js | Microsoft Azure Active Directory Passport.js προσθήκης | [Passport-Azure-AD (npm)][ClientLib-Node-Lib] | [Passport-Azure-AD (GitHub)][ClientLib-Node-Repo] | Έρχομαι σύντομα |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>Βιβλιοθήκες ενδιάμεσου λογισμικού της Microsoft που υποστηρίζονται server
| Πλατφόρμα| Όνομα της βιβλιοθήκης| Λήψη | Πηγαίος κώδικας | Δείγμα |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | OWIN OpenID σύνδεση ενδιάμεσου λογισμικού για το ASP.NET | [Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] | [Katana έργου (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Δείγμα εφαρμογής Web][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | Ενδιάμεσο φορέα OAuth OWIN για το ASP.NET | [Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] | [Katana έργου (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Δείγμα API Web][ServerLib-Net4-Owin-Oauth-Sample] |
| .NET πυρήνα | OWIN OpenID σύνδεση ενδιάμεσο για .NET πυρήνα | [Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] | [Ασφάλεια ASP.NET (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Δείγμα εφαρμογής Web][ServerLib-NetCore-Owin-Oidc-Sample] |
| .NET πυρήνα | Ενδιάμεσο φορέα OAuth OWIN για .NET πυρήνα | [Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] | [Ασφάλεια ASP.NET (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | Έρχομαι σύντομα |
| Node.js | Microsoft Azure Active Directory Passport.js προσθήκης | [Passport-Azure-AD (npm)][ServerLib-Node-Lib] | [Passport-Azure-AD (GitHub)][ServerLib-Node-Repo] | [Δείγμα εφαρμογής Web][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>Βιβλιοθήκες συμβατό πρόγραμμα-πελάτη
| Πλατφόρμα| Όνομα της βιβλιοθήκης | Δοκιμή έκδοση | Πηγαίος κώδικας | Δείγμα |
| :-: | :-: | :-: | :-: | :-: |
| Android | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [Δείγμα εγγενής εφαρμογής](active-directory-v2-devquickstarts-android.md) |
| iOS | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [Δείγμα εγγενής εφαρμογής](active-directory-v2-devquickstarts-ios.md)|
| JavaScript | [Hello.js](https://adodson.com/hello.js/) | 1.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | Έρχομαι σύντομα |
| Python φιάλη | [Φιάλη OAuthlib](https://github.com/lepture/flask-oauthlib) | 0.9.3 | [Φιάλη OAuthlib](https://github.com/lepture/flask-oauthlib) | Έρχομαι σύντομα |
| Κείμενο φωνητικής γραφής | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>omniauth-oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) | Έρχομαι σύντομα |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>Βιβλιοθήκες ενδιάμεσο συμβατά διακομιστή
Έρχομαι σύντομα

## <a name="related-content"></a>Σχετικό περιεχόμενο
Για περισσότερες πληροφορίες σχετικά με το τελικό σημείο v2.0 Azure AD, ανατρέξτε στο θέμα το [Azure AD εφαρμογή μοντέλο v2.0 Επισκόπηση][AAD-App-Model-V2-Overview].

Για να μας περιορισμός και διαμόρφωση μας περιεχομένου, χρησιμοποιήστε τη δυνατότητα σχολίων Disqus στο τέλος αυτού του άρθρου για την παροχή σχολίων.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:
[ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]:
[ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]:
[ClientLib-Android-Repo]:
[ClientLib-Android-Sample]:
[ClientLib-Js-Lib]:
[ClientLib-Js-Repo]:
[ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
