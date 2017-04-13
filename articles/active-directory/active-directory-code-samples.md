<properties
   pageTitle="Δείγματα κώδικα καταλόγου Azure Active Directory | Microsoft Azure"
   description="Ένα ευρετήριο δείγματα κώδικα Azure Active Directory, οργανωμένες ανά σενάριο."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="priyamohanram"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-code-samples"></a>Δείγματα κώδικα καταλόγου Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Μπορείτε να χρησιμοποιήσετε το Microsoft Azure Active Directory (Azure AD) για να προσθέσετε τον έλεγχο ταυτότητας και εξουσιοδότηση web APIs και τις εφαρμογές web. Αυτή η ενότητα σας συνδέει με δείγματα κώδικα που σας δείχνουν πώς γίνεται και τμήματα κώδικα που μπορείτε να χρησιμοποιήσετε στις εφαρμογές σας. Στη σελίδα δείγμα κώδικα, θα βρείτε λεπτομερείς ανάγνωση-εμένα θέματα που σας βοηθούν με τις απαιτήσεις, εγκατάσταση και εγκατάστασης. Και ο κώδικας είναι σχόλιο για να σας βοηθήσει να κατανοήσετε τις κρίσιμες ενότητες.

Για να κατανοήσετε το βασικό σενάριο για κάθε τύπο δείγμα, ανατρέξτε στο θέμα σενάρια ελέγχου ταυτότητας για Azure AD.

Συνεργάτης για να μας δείγματα GitHub: [Microsoft Azure Active Directory δείγματα και την τεκμηρίωση](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Πρόγραμμα περιήγησης Web σε εφαρμογή Web

Αυτά τα παραδείγματα δείχνουν τον τρόπο για να γράψετε μια εφαρμογή web που κατευθύνει το πρόγραμμα περιήγησης του χρήστη για να πραγματοποιήσετε είσοδο στο Azure AD.



| Γλώσσα/πλατφόρμα | Δείγμα | Περιγραφή
| ----------------- | ------ | -----------
| C# / .NET | [WebApp-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Χρησιμοποιήστε OpenID σύνδεση (OWIN σύνδεση OpenID ASP.Net ενδιάμεσο) για τον έλεγχο ταυτότητας χρηστών από ένα μισθωτή του Azure AD.
| C# / .NET | [WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Πολλαπλή μισθωτή .NET MVC εφαρμογής web που χρησιμοποιεί OpenID σύνδεση (OWIN σύνδεση OpenID ASP.Net ενδιάμεσο) για τον έλεγχο ταυτότητας χρηστών από πολλούς μισθωτές του Azure AD.
| C# / .NET | [WebApp-WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | Χρήση WS-Ομοσπονδία (OWIN WS Ομοσπονδία ASP.Net ενδιάμεσο) για τον έλεγχο ταυτότητας χρηστών από ένα μισθωτή του Azure AD.






## <a name="single-page-application-spa"></a>Μεμονωμένη σελίδα εφαρμογής (SPA)

Αυτό το δείγμα δείχνει πώς να συντάξετε μια μεμονωμένη σελίδα εφαρμογή ασφαλές με Azure AD.  

| Γλώσσα/πλατφόρμα | Δείγμα | Περιγραφή
| ----------------- | ------ | -----------
| JavaScript, C# / .NET | [SinglePageApp DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | Χρησιμοποιήστε ADAL για JavaScript και Azure AD για την ασφάλιση μιας εφαρμογής βάσει AngularJS μεμονωμένη σελίδα εφαρμόζονται με τέλος πίσω web API ASP.NET.


## <a name="native-application-to-web-api"></a>Εγγενή εφαρμογή για να API Web

Αυτά τα δείγματα κώδικα δείχνουν πώς μπορείτε να δημιουργήσετε εφαρμογές εγγενές πρόγραμμα-πελάτη που καλούν web API που προστατεύονται από Azure AD. Χρησιμοποιούν [Βιβλιοθήκη ελέγχου ταυτότητας Azure AD (ADAL)](active-directory-authentication-libraries.md) και το [διακριτικό 2.0 στο Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Γλώσσα/πλατφόρμα | Δείγμα | Περιγραφή
| ----------------- | ------ | -----------
| JavaScript | [NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) | Χρησιμοποιήστε την προσθήκη ADAL για Apache Cordova για να δημιουργήσετε μια εφαρμογή Apache Cordova που καλεί μια API web και χρησιμοποιεί Azure AD για έλεγχο ταυτότητας.
| C# / .NET | [NativeClient DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) | Μια εφαρμογή .NET WPF που καλεί μια API web που προστατεύεται με τη χρήση Azure AD.
| C# / .NET | [NativeClient WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Μια εφαρμογή του Windows Store που καλεί μια API web που προστατεύεται με Azure AD.
| C# / .NET | [NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) | Μια κλήση web πολλών μισθωτών API που προστατεύεται με Azure AD εφαρμογή Windows Store.
| C# / .NET | [WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) | Μια εφαρμογή εγγενές πρόγραμμα-πελάτη που καλεί μια API, το οποίο λαμβάνει ενός διακριτικού για εκτέλεση ενεργειών εκ μέρους του αρχικού χρήστη και, στη συνέχεια, να χρησιμοποιεί το διακριτικό για να καλέσετε μια άλλη API web στο web.
| C# / .NET | [NativeClient WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) | Μια εφαρμογή του Windows Store για Windows Phone 8.1 που καλεί μια API web που προστατεύεται από Azure AD.
| ObjC | [NativeClient iOS](https://github.com/Azure-Samples/active-directory-ios) | Μια εφαρμογή iOS που καλεί μια API web που απαιτεί Azure AD για έλεγχο ταυτότητας.
| C# / .NET | [WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) | Μια εφαρμογή εγγενές πρόγραμμα-πελάτη που περιλαμβάνει λογική για την επεξεργασία ενός διακριτικού JWT στο web API, αντί να χρησιμοποιήσετε το ενδιάμεσο OWIN.
| C# / Xamarin | [NativeClient-Xamarin-Android](https://github.com/Azure-Samples/active-directory-xamarin-android) | Μια σύνδεση Xamarin για τα εγγενή Azure AD ελέγχου ταυτότητας βιβλιοθήκη (ADAL) για τη βιβλιοθήκη Android.
| C# / Xamarin | [NativeClient-Xamarin-iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) | Μια σύνδεση Xamarin για τα εγγενή Azure AD ελέγχου ταυτότητας βιβλιοθήκη (ADAL) για iOS.
| C# / Xamarin | [NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) | Ένα έργο Xamarin που ως προορισμό πέντε πλατφόρμες και καλεί μια API web που προστατεύεται από Azure AD.
| C# / .NET | [NativeClient χωρίς κεφαλίδα DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) | Μια εγγενή εφαρμογή που πραγματοποιεί έλεγχο ταυτότητας μη αλληλεπιδραστική και καλεί μια API web που προστατεύεται από Azure AD.



## <a name="web-application-to-web-api"></a>Εφαρμογή Web στο Web API

Αυτά τα δείγματα κώδικα εμφάνιση Χρησιμοποιήστε πώς [2.0 διακριτικό στο Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) για να δημιουργήσετε εφαρμογές web που καλούν web API που προστατεύονται από Azure AD.

| Γλώσσα/πλατφόρμα | Δείγμα | Περιγραφή
| ----------------- | ------ | -----------
| C# / .NET | [WebApp-WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) | Κλήση ενός API web με συνδεδεμένοι στο χρήστη δικαιώματα.
|  C# / .NET | [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) | Κλήση ενός API web με την εφαρμογή δικαιωμάτων.
| C# / .NET | [WebApp-WebAPI-OAuth2-UserIdentity-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | Προσθέστε μια υπάρχουσα εφαρμογή web εξουσιοδότησης με το [διακριτικό 2.0 στο Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) , ώστε να μπορούν να καλέσουν το web API.
| JavaScript | [WebAPI Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) | Ρύθμιση υπηρεσίας REST API που είναι ενσωματωμένο στο Azure AD API προστασίας. Περιλαμβάνει ένα διακομιστή Node.js με ένα API Web.
| C# / .NET | [WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |  Εφαρμογή που χρησιμοποιεί OpenID σύνδεση (OWIN σύνδεση OpenID ASP.Net ενδιάμεσο) για τον έλεγχο ταυτότητας χρηστών από ένα μισθωτή του Azure AD web πολλών μισθωτή MVC. Χρησιμοποιεί έναν κωδικό εξουσιοδότησης για να καλέσετε το API του γραφήματος.

## <a name="server-or-daemon-application-to-web-api"></a>Διακομιστής ή εφαρμογή υπηρεσίας στο Web API

Αυτά τα δείγματα κώδικα δείχνουν πώς μπορείτε να δημιουργήσετε μια εφαρμογή μονού ή διακομιστή που λαμβάνει πόρους από ένα API web χρησιμοποιώντας [Βιβλιοθήκη ελέγχου ταυτότητας Azure AD (ADAL)](active-directory-authentication-libraries.md) και το [διακριτικό 2.0 στο Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Γλώσσα/πλατφόρμα | Δείγμα | Περιγραφή
| ----------------- | ------ | -----------
| C# / .NET | [Μονού DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) | Μια εφαρμογή κονσόλας κλήσεις web API. Τα διαπιστευτήρια προγράμματος-πελάτη είναι έναν κωδικό πρόσβασης.
| C# / .NET | [Μονού-CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) | Μια εφαρμογή κονσόλας που καλεί μια API web. Τα διαπιστευτήρια προγράμματος-πελάτη είναι ένα πιστοποιητικό.


## <a name="calling-azure-ad-graph-api"></a>Κλήση API Azure AD Graph

Αυτά τα δείγματα κώδικα δείχνουν πώς μπορείτε να δημιουργήσετε εφαρμογές που καλούν του Azure AD Graph API για ανάγνωση και εγγραφή δεδομένων καταλόγου.

| Γλώσσα/πλατφόρμα | Δείγμα | Περιγραφή
| ----------------- | ------ | -----------
| Java | [WebApp-GraphAPI-Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) | Μια εφαρμογή web που χρησιμοποιεί το API Graph για να αποκτήσετε πρόσβαση σε δεδομένα καταλόγου Azure AD.
| PHP | [WebApp-GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) | Μια εφαρμογή web που χρησιμοποιεί το API γράφημα για να αποκτήσετε πρόσβαση σε δεδομένα καταλόγου Azure AD.
| C# / .NET | [WebApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Μια εφαρμογή web που χρησιμοποιεί το API Graph για να αποκτήσετε πρόσβαση σε δεδομένα καταλόγου Azure AD.
| C# / .NET | [ConsoleApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) | Αυτή η εφαρμογή κονσόλας παρουσιάζει κοινές προσκλήσεις ανάγνωσης και εγγραφής για το API Graph και δείχνει πώς μπορείτε να εκτελέσει εκχώρηση αδειών χρήσης χρήστη και ενημέρωση ενός χρήστη μικρογραφιών συνδέσεις σε φωτογραφία και.
| C# / .NET | [ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) | Μια εφαρμογή κονσόλας που χρησιμοποιεί το ερώτημα διακριτική στο γράφημα API για να λάβετε περιοδικό αλλαγών σε αντικείμενα χρήστη σε ένα μισθωτή του Azure AD.
| C# / .NET | [WebApp-GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) | Μια εφαρμογή MVC χρησιμοποιεί ερωτήματα Graph API για τη δημιουργία ενός οργανογράμματος απλό εταιρείας.
| PHP | [WebApp-GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) | Μια εφαρμογή PHP που καλεί το API του γραφήματος για να καταχωρήσετε μια επέκταση και, στη συνέχεια, ανάγνωση, ενημέρωση και διαγραφή τιμές στο χαρακτηριστικό επέκταση.


## <a name="authorization"></a>Εξουσιοδότηση

Αυτά τα δείγματα κώδικα δείχνουν πώς μπορείτε να χρησιμοποιήσετε Azure AD για άδεια.

| Γλώσσα/πλατφόρμα | Δείγμα | Περιγραφή
| ----------------- | ------ | -----------
| C# / .NET | [WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Εκτέλεση έλεγχος πρόσβασης βάσει ρόλων (RBAC) που χρησιμοποιώντας Azure Active Directory δηλώσεις ομάδας σε μια εφαρμογή που είναι συνδεδεμένο με το Azure AD.
| C# / .NET | [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Εκτέλεση ελέγχου πρόσβασης βάσει ρόλων (RBAC) στο με ρόλους εφαρμογής Azure Active Directory σε μια εφαρμογή που είναι συνδεδεμένο με το Azure AD.


## <a name="legacy-walkthroughs"></a>Αναλυτικές παλαιού τύπου

Αυτές τις αναλυτικές χρησιμοποιούν ελαφρώς παλαιότερη τεχνολογία, αλλά εξακολουθείτε να μπορεί να είναι που σας ενδιαφέρουν.

| Γλώσσα/πλατφόρμα | Δείγμα | Περιγραφή
| ----------------- | ------ | -----------
| C# / .NET | [Βάσει ρόλων και ACL βάσει εξουσιοδότησης σε μια εφαρμογή του Microsoft Azure AD](http://go.microsoft.com/fwlink/?LinkId=331694) | Εκτέλεση εξουσιοδότηση βάσει ρόλων (RBAC) και ACL βάσει εξουσιοδότησης σε μια εφαρμογή που είναι συνδεδεμένο με το Azure AD.
| C# / .NET |  [Έλεγχος ταυτότητας AAL - εφαρμογή Windows Store στην υπηρεσία ΥΠΌΛΟΙΠΟ-](http://go.microsoft.com/fwlink/?LinkId=330605) |  Χρησιμοποιήστε [Τη βιβλιοθήκη ελέγχου ταυτότητας Azure AD (ADAL)](active-directory-authentication-libraries.md) (πρώην AAL) για την έκδοση Beta του Windows Store για να προσθέσετε δυνατότητες τον έλεγχο ταυτότητας χρήστη σε μια εφαρμογή του Windows Store.
| C# / .NET | [ADAL - εγγενούς εφαρμογής υπηρεσίας ΥΠΌΛΟΙΠΟ - τον έλεγχο ταυτότητας με AAD μέσω του παραθύρου διαλόγου του προγράμματος περιήγησης](http://go.microsoft.com/fwlink/?LinkId=259814) |  Χρησιμοποιήστε [Τη βιβλιοθήκη ελέγχου ταυτότητας Azure AD (ADAL)](active-directory-authentication-libraries.md) για να προσθέσετε δυνατότητες τον έλεγχο ταυτότητας χρήστη σε ένα πρόγραμμα-πελάτη WPF.
| C# / .NET | [ADAL - εγγενούς εφαρμογής υπηρεσίας ΥΠΌΛΟΙΠΟ - τον έλεγχο ταυτότητας με ACS μέσω του παραθύρου διαλόγου του προγράμματος περιήγησης](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  Χρησιμοποιήστε [Τη βιβλιοθήκη ελέγχου ταυτότητας Azure AD (ADAL)](active-directory-authentication-libraries.md) και [2.0 υπηρεσία ελέγχου πρόσβασης (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) για να προσθέσετε δυνατότητες τον έλεγχο ταυτότητας χρήστη σε ένα πρόγραμμα-πελάτη WPF.
| C# / .NET | [ADAL - έλεγχος ταυτότητας διακομιστή προς διακομιστή](http://go.microsoft.com/fwlink/?LinkId=259816) | Χρησιμοποιήστε [Βιβλιοθήκη ελέγχου ταυτότητας Azure AD (ADAL)](active-directory-authentication-libraries.md) για την ασφάλιση κλήσεις υπηρεσίας από μια διεργασία πλευρά του διακομιστή σε μια υπηρεσία MVC4 Web API REST.
| C# / .NET | [Προσθήκη σύνδεσης στην εφαρμογή Web σας με χρήση Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Ρύθμιση παραμέτρων μιας εφαρμογής .NET για να εκτελέσετε web καθολικής σύνδεσης σε σχέση με το Azure AD κατάλογο της επιχείρησης.
| C# / .NET | [Ανάπτυξη εφαρμογών Web πολλών μισθωτή με Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Χρησιμοποιήστε Azure AD για να προσθέσετε τις καθολικής σύνδεσης και τις δυνατότητες πρόσβασης καταλόγου μίας .NET εφαρμογής, ώστε να λειτουργεί σε πολλές εταιρείες.
JAVA | [Εφαρμογή δείγματος Java για Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkId=263969) | Χρήση του API Graph για πρόσβαση σε δεδομένα καταλόγου από Azure AD.
PHP | [Εφαρμογή δείγματος PHP για Azure AD Graph API](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | Χρήση του API Graph για πρόσβαση σε δεδομένα καταλόγου από Azure AD.
| C# / .NET | [Δείγμα εφαρμογής για Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkID=262648) | Χρήση του API Graph για πρόσβαση σε δεδομένα καταλόγου από Azure AD.
| C# / .NET | [Δείγμα εφαρμογής για Azure AD Graph διακριτική ερωτήματος](http://go.microsoft.com/fwlink/?LinkId=275398) | Χρησιμοποιήστε το ερώτημα διαφορά στη το API Graph για περιοδικές αλλαγών σε αντικείμενα χρήστη σε ένα μισθωτή του Azure AD.
| C# / .NET | [Δείγμα εφαρμογής για την ενσωμάτωση εφαρμογής πολλών μισθωτών Cloud για Azure AD](http://go.microsoft.com/fwlink/?LinkId=275397) | Μπορείτε να ενσωματώσετε μια εφαρμογή πολλών μισθωτών στη Azure AD.
| C# / .NET | [Ασφάλιση μια εφαρμογή του Windows Store και μια υπηρεσία Web REST χρησιμοποιώντας Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Δημιουργία ενός πόρου απλό API web και μια εφαρμογή προγράμματος-πελάτη του Windows Store, χρησιμοποιώντας Azure AD και τη [Βιβλιοθήκη ελέγχου ταυτότητας Azure AD (ADAL)](active-directory-authentication-libraries.md).
| C# / .NET| [Χρήση του γραφήματος API ερώτημα Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Ρύθμιση παραμέτρων μιας εφαρμογής Microsoft .NET για να χρησιμοποιήσετε το API Azure AD γράφημα για να αποκτήσετε πρόσβαση σε δεδομένα από έναν κατάλογο μισθωτή Azure AD.

## <a name="see-also"></a>Δείτε επίσης

##### <a name="other-resources"></a>Άλλοι πόροι

[Οδηγός καταλόγου Azure Active Directory για προγραμματιστές](active-directory-developers-guide.md)

[Azure AD Graph API εννοιολογική και αναφορά](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Βιβλιοθήκη Βοήθειας API Azure AD Graph](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)

