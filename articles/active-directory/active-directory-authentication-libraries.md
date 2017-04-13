<properties
   pageTitle="Βιβλιοθήκες ελέγχου ταυτότητας καταλόγου Azure Active Directory | Microsoft Azure"
   description="Το Azure βιβλιοθήκη ελέγχου ταυτότητας AD (ADAL) επιτρέπει προγράμματος-πελάτη προγραμματιστές εφαρμογών για εύκολα τον έλεγχο ταυτότητας χρηστών στο cloud ή εσωτερικής Active Directory (AD) και, στη συνέχεια, να αποκτήσετε διακριτικά πρόσβασης για την ασφάλεια των κλήσεων API."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Βιβλιοθήκες ελέγχου ταυτότητας καταλόγου Azure Active Directory

Ο έλεγχος ταυτότητας Azure AD βιβλιοθήκης (ADAL) επιτρέπει στους προγραμματιστές εφαρμογών προγράμματος-πελάτη για εύκολα τον έλεγχο ταυτότητας χρηστών στο cloud ή εσωτερικής Active Directory (AD), και, στη συνέχεια, αποκτήστε διακριτικά πρόσβασης για την ασφάλεια των κλήσεων API. ADAL έχει πολλές δυνατότητες ότι ο έλεγχος ταυτότητας κάνετε πιο εύκολη για τους προγραμματιστές, όπως ασύγχρονης υποστήριξη, ένα με δυνατότητα ρύθμισης παραμέτρων cache διακριτικού που αποθηκεύει διακριτικά πρόσβασης και ανανέωση διακριτικά, αυτόματης ανανέωσης διακριτικού πότε λήγει ένα διακριτικό πρόσβασης και ενός διακριτικού ανανέωση είναι διαθέσιμο, και πολλά άλλα. Μέσω του χειρισμού περισσότερες από την πολυπλοκότητα, ADAL να Βοήθεια για προγραμματιστές εστίαση σε επιχειρηματικής λογικής στην εφαρμογή τους και να εύκολα εξασφάλιση πόρων χωρίς να είναι ένα ειδικό σχετικά με την ασφάλεια.

ADAL είναι διαθέσιμη σε διάφορες πλατφόρμες.

|Πλατφόρμα|Όνομα πακέτου|Πρόγραμμα-πελάτη/διακομιστή|Λήψη|Πηγαίος κώδικας|Τεκμηρίωση & δείγματα|
|---|---|---|---|---|---|
|.NET προγράμματος-πελάτη, Windows Store, UWP, Xamarin iOS και Android|Βιβλιοθήκη ελέγχου ταυτότητας Active Directory (ADAL) για .NET v3 |Πρόγραμμα-πελάτη|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[ADAL για .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[Τεκμηρίωση](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.NET προγράμματος-πελάτη, χώρου αποθήκευσης των Windows, Windows Phone 8.1 |Βιβλιοθήκη ελέγχου ταυτότητας Active Directory (ADAL) για το .NET v2 |Πρόγραμμα-πελάτη|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[ADAL για .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[Τεκμηρίωση](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|JavaScript|Βιβλιοθήκη ελέγχου ταυτότητας Active Directory (ADAL) για JavaScript|Πρόγραμμα-πελάτη|[ADAL για JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL για JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|Δείγμα: [SinglePageApp-DotNet (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|Λειτουργικό σύστημα OS X, iOS|Βιβλιοθήκη ελέγχου ταυτότητας Active Directory (ADAL) για το στόχο-C|Πρόγραμμα-πελάτη|[ADAL για το στόχο-C (CocoaPods)](http://cocoadocs.org/docsets/ADAL/)|[ADAL για το στόχο-C (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|Δείγμα: [NativeClient-iOS (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android|Βιβλιοθήκη ελέγχου ταυτότητας Active Directory (ADAL) για Android|Πρόγραμμα-πελάτη|[ADAL για Android (το κεντρικό αποθετήριο δεδομένων)](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[ADAL για Android (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-android)|Δείγμα: [NativeClient Android (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|Node.js|Βιβλιοθήκη ελέγχου ταυτότητας Active Directory (ADAL) για Node.js|Πρόγραμμα-πελάτη|[ADAL για Node.js (npm)](https://www.npmjs.com/package/adal-node)|[ADAL για Node.js (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|Δείγμα: [WebAPI-Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|Node.js|Ενδιάμεσο λογισμικό Microsoft Azure Active Directory Passport ελέγχου ταυτότητας για κόμβου|Πρόγραμμα-πελάτη|[Azure Active Directory Passport για Node.js (npm)](https://www.npmjs.com/package/passport-azure-ad)|[Azure Active Directory για Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Βιβλιοθήκη ελέγχου ταυτότητας Active Directory (ADAL) για Java|Πρόγραμμα-πελάτη|[ADAL για Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[ADAL για Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET|Ταυτότητα πρωτοκόλλου επεκτάσεις για το Microsoft .NET Framework διαίρεσης 4,5|Διακομιστή|[Microsoft.IdentityModel.Protocol.Extensions (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Επεκτάσεις μοντέλο ταυτότητας Azure AD για .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|JSON πρόγραμμα χειρισμού διακριτικού Web για το Microsoft .net Framework διαίρεσης 4,5|Διακομιστή|[System.IdentityModel.Tokens.Jwt (NuGet)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Επεκτάσεις μοντέλο ταυτότητας Azure AD για .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|Ενδιάμεσο OWIN που επιτρέπει σε μια εφαρμογή για να χρησιμοποιήσετε τεχνολογία της Microsoft για τον έλεγχο ταυτότητας.|Διακομιστή|[Microsoft.Owin.Security.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET|Ενδιάμεσο OWIN που επιτρέπει σε μια εφαρμογή για να χρησιμοποιήσετε OpenIDConnect για τον έλεγχο ταυτότητας.|Διακομιστή|[Microsoft.Owin.Security.OpenIdConnect (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Δείγμα: [WebApp-OpenIDConnecty-DotNet (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET|Ενδιάμεσο OWIN που επιτρέπει σε μια εφαρμογή για να χρησιμοποιήσετε WS Ομοσπονδία για έλεγχο ταυτότητας.|Διακομιστή|[Microsoft.Owin.Security.WsFederation (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Δείγμα: [WebApp-WSFederation-DotNet (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>Σενάρια

Ακολουθούν τρία συνηθισμένα σενάρια όπου ADAL μπορεί να χρησιμοποιηθεί για τον έλεγχο ταυτότητας.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Έλεγχος ταυτότητας χρηστών από μια εφαρμογή προγράμματος-πελάτη σε έναν απομακρυσμένο πόρο

Σε αυτό το σενάριο, ο προγραμματιστής περιλαμβάνει ένα πρόγραμμα-πελάτη, όπως μια εφαρμογή WPF, που χρειάζεται για να αποκτήσετε πρόσβαση σε έναν απομακρυσμένο πόρο με ασφάλεια με Azure AD, όπως ένα API web. Αποστέλλει έχει μια συνδρομή του Azure, γνωρίζει πώς να καλέσει το API web ροής και γνωρίζει το μισθωτή Azure AD που χρησιμοποιεί το API web. Ως αποτέλεσμα, μπορεί να χρησιμοποιήσει ADAL για τη διευκόλυνση έλεγχο ταυτότητας με Azure AD, είτε με πλήρως μεταβίβαση του ελέγχου ταυτότητας εμπειρία στο ADAL είτε με ρητά χειρισμός διαπιστευτήρια χρήστη. ADAL καθιστά εύκολη την έλεγχο ταυτότητας του χρήστη, αποκτήστε ένα διακριτικό πρόσβασης και ανανέωση διακριτικού από το Azure AD και, στη συνέχεια, χρησιμοποιήστε το διακριτικό πρόσβασης για να κάνετε τις αιτήσεις για το API στο web.

Για ένα δείγμα κώδικα που δείχνει τη χρήση του ελέγχου ταυτότητας για να Azure AD αυτό το σενάριο, ανατρέξτε στο θέμα [Εγγενή εφαρμογή WPF προγράμματος-πελάτη για να το API Web](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Πραγματοποίηση ελέγχου ταυτότητας σε μια εφαρμογή διακομιστή σε έναν απομακρυσμένο πόρο

Σε αυτό το σενάριο, ο προγραμματιστής έχει μια εφαρμογή που εκτελείται σε ένα διακομιστή που χρειάζεται για να αποκτήσετε πρόσβαση σε έναν απομακρυσμένο πόρο με ασφάλεια με Azure AD, όπως ένα API web. Αποστέλλει έχει μια συνδρομή του Azure, γνωρίζει πώς να ενεργοποιήσετε την υπηρεσία ροής και γνωρίζει χρησιμοποιεί ο μισθωτής Azure AD το API web. Ως αποτέλεσμα, μπορεί να χρησιμοποιήσει ADAL να διευκολύνει τον έλεγχο ταυτότητας με Azure AD με ρητά χειρισμός διαπιστευτηρίων της εφαρμογής. ADAL καθιστά εύκολη την ανάκτηση ενός διακριτικού από Azure AD με χρήση διαπιστευτηρίων της εφαρμογής υπολογιστή-πελάτη και, στη συνέχεια, χρησιμοποιήστε αυτό το διακριτικό για να κάνετε τις αιτήσεις για το API στο web. ADAL χειρίζεται επίσης τη διαχείριση της διάρκειας ζωής των το διακριτικό πρόσβασης σε cache του και την ανανέωση ανάλογα με τις απαιτήσεις. Για ένα δείγμα κώδικα που παρουσιάζει αυτό το σενάριο, ανατρέξτε στο θέμα [Εφαρμογή κονσόλας για Web API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>Πραγματοποίηση ελέγχου ταυτότητας σε μια εφαρμογή διακομιστή εκ μέρους ενός χρήστη για πρόσβαση σε έναν απομακρυσμένο πόρο

Σε αυτό το σενάριο, ο προγραμματιστής έχει μια εφαρμογή που εκτελείται σε ένα διακομιστή που χρειάζεται για να αποκτήσετε πρόσβαση σε έναν απομακρυσμένο πόρο με ασφάλεια με Azure AD, όπως ένα API web. Η αίτηση πρέπει επίσης να γίνουν εκ μέρους ενός χρήστη στο Azure AD. Αποστέλλει έχει μια συνδρομή του Azure, γνωρίζει πώς να καλέσετε τη μετάδοση API web και να γνωρίζει το Azure AD μισθωτή χρησιμοποιεί την υπηρεσία. Όταν ο χρήστης έχει γίνει έλεγχος ταυτότητας στην εφαρμογή web, η εφαρμογή να λάβετε έναν κωδικό εξουσιοδότησης για το χρήστη από Azure AD. Η εφαρμογή web, στη συνέχεια, να χρησιμοποιήσετε ADAL για να αποκτήσετε ένα διακριτικό πρόσβασης και ανανέωση διακριτικού εκ μέρους ενός χρήστη χρησιμοποιώντας το εξουσιοδότησης κωδικό και προγράμματος-πελάτη τα διαπιστευτήρια που σχετίζεται με την εφαρμογή από το Azure AD. Μόλις η εφαρμογή web στη διάθεσή του το διακριτικό πρόσβασης, το μπορούν να καλέσουν το API web μέχρι να λήξει το διακριτικό. Όταν λήξει το διακριτικό, η εφαρμογή web να χρησιμοποιήσετε ADAL για να λάβετε έναν νέο κωδικό πρόσβασης με τη χρήση της ανανέωσης διακριτικού που λήφθηκε προηγουμένως.


## <a name="see-also"></a>Δείτε επίσης

[Οδηγός του Azure Active Directory για προγραμματιστές](active-directory-developers-guide.md)

[Σενάρια ελέγχου ταυτότητας για την υπηρεσία καταλόγου Azure Active directory](active-directory-authentication-scenarios.md)

[Δείγματα κώδικα Azure Active Directory](active-directory-code-samples.md)
