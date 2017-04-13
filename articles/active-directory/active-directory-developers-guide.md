<properties
   pageTitle="Οδηγός Azure Active Directory για προγραμματιστές του | Microsoft Azure"
   description="Σε αυτό το άρθρο παρέχει μια ολοκληρωμένη Οδηγός προσανατολισμένος σε developer πόρους για Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="mbaldwin"/>


# <a name="azure-active-directory-developers-guide"></a>Azure Active Directory για προγραμματιστές του οδηγού

## <a name="overview"></a>Επισκόπηση
Ως ένα διαχείρισης των ταυτοτήτων ως πλατφόρμα υπηρεσίας (IDMaaS), Azure Active Directory (AD) παρέχει στους προγραμματιστές ένας αποτελεσματικός τρόπος να ενσωματώσετε διαχείρισης των ταυτοτήτων σε εφαρμογές τους. Τα ακόλουθα άρθρα παρέχει επισκοπήσεις υλοποίηση και βασικές δυνατότητες του Azure AD. Προτείνουμε να που διαβάζετε σε σειρά ή Μετάβαση σε [Γρήγορα αποτελέσματα](#getting-started) Εάν είστε έτοιμοι για να ψάξετε.


1. [Τα πλεονεκτήματα της ενοποίησης Azure AD](./develop/active-directory-how-to-integrate.md): Ανακαλύψτε γιατί ενοποίηση με το Azure AD προσφέρει η καλύτερη λύση για ασφαλή εισόδου και εξουσιοδότησης.

1. [Azure AD ελέγχου ταυτότητας σενάρια](active-directory-authentication-scenarios.md): εκμεταλλευτείτε απλοποιημένη ελέγχου ταυτότητας σε Azure AD για την παροχή καθολικής σύνδεσης στην εφαρμογή σας.

1. [Ενσωμάτωση εφαρμογών με το Azure AD](active-directory-integrating-applications.md): Μάθετε πώς μπορείτε να προσθέσετε, να ενημερώσετε και κατάργηση εφαρμογών από το Azure AD και σχετικά με τις κατευθυντήριες γραμμές εμπορικής προσαρμογής για ενσωματωμένες εφαρμογές.

1. [Azure AD Graph API](active-directory-graph-api.md): χρήση του API Azure AD Graph για πρόσβαση μέσω προγραμματισμού Azure AD μέσω REST API τελικά σημεία. Το API Azure AD Graph είναι επίσης προσβάσιμο μέσω του [Microsoft Graph](https://graph.microsoft.io/). Το Microsoft Graph παρέχει ένα ενοποιημένου API που παρέχει πρόσβαση σε πολλά υπηρεσία cloud της Microsoft API, μέσω ένα μεμονωμένο τελικό σημείο REST API και με διακριτικό πρόσβασης μόνο.

1. [Βιβλιοθήκες ελέγχου ταυτότητας Azure AD](active-directory-authentication-libraries.md): εύκολα τον έλεγχο ταυτότητας χρηστών για να αποκτήσετε διακριτικά πρόσβασης με τη χρήση βιβλιοθηκών Azure AD ελέγχου ταυτότητας για .NET, JavaScript, στόχος-C, Android και άλλα.


## <a name="getting-started"></a>Γρήγορα αποτελέσματα

Αυτά τα προγράμματα εκμάθησης είναι προσαρμοσμένες για πολλές πλατφόρμες και μπορούν να σας βοηθήσουν να ξεκινήσετε γρήγορα ανάπτυξη με το Azure Active Directory. Ως προϋπόθεση, πρέπει να [λάβετε ένα μισθωτή του Azure Active Directory](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Οδηγοί γρήγορης εκκίνησης εφαρμογών Mobile και του Υπολογιστή σας

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Παγκόσμια των Windows](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![Διακριτικό 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Παγκόσμια των Windows](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[Ενοποίηση απευθείας με το διακριτικό 2.0](active-directory-protocols-oauth-code.md)|

### <a name="web-application-quick-start-guides"></a>Οδηγοί γρήγορης εκκίνησης εφαρμογών Web

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![JavaScript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![Σύνδεση OpenID](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[Ενοποίηση απευθείας με OpenID σύνδεση](active-directory-protocols-openid-connect-code.md)|

### <a name="web-api-quick-start-guides"></a>Οδηγοί γρήγορης εκκίνησης για το API Web

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### <a name="querying-the-directory-quickstart-guide"></a>Υποβολή ερωτημάτων τον Οδηγό γρήγορη έναρξη καταλόγου

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Graph API](active-directory-graph-api-quickstart.md)|

## <a name="how-tos"></a>How-TOS

Αυτά τα άρθρα περιγράφουν τον τρόπο για να εκτελέσετε συγκεκριμένες εργασίες χρησιμοποιώντας Azure Active Directory:

- [Λάβετε ένα μισθωτή του Azure AD](active-directory-howto-tenant.md)
- [Συνδεθείτε σε οποιονδήποτε χρήστη Azure AD χρησιμοποιώντας το μοτίβο εφαρμογή πολλών μισθωτή](active-directory-devhowto-multi-tenant-overview.md)
- Ενεργοποίηση σταυρό εφαρμογή SSO χρησιμοποιώντας ADAL, [Android](active-directory-sso-android.md) και συσκευές [iOS](active-directory-sso-ios.md)
- [Πραγματοποίηση της εφαρμογής AppSource πιστοποιηθεί για Azure AD](active-directory-devhowto-appsource-certified.md)
- [Λίστα την εφαρμογή στη συλλογή εφαρμογών Azure AD](active-directory-app-gallery-listing.md)
- [Υποβολή εφαρμογές web για το Office 365 στον πίνακα εργαλείων πωλητή](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Κατανόηση της δήλωσης εφαρμογή Azure Active Directory](active-directory-application-manifest.md)
- [Κατανόηση του εμπορικής προσαρμογής οδηγίες για τα κουμπιά acquisition εισόδου και εφαρμογή στην εφαρμογή υπολογιστή-πελάτη](active-directory-branding-guidelines.md)
- [Προεπισκόπηση: Πώς να δημιουργείτε εφαρμογές που είσοδος χρήστες με λογαριασμούς τόσο προσωπικών & εργασίας ή του σχολείου](active-directory-appmodel-v2-overview.md)
- [Προεπισκόπηση: Πώς να δημιουργείτε εφαρμογές που εγγράφονται & είσοδος τους καταναλωτές](../active-directory-b2c/active-directory-b2c-overview.md)
- [Προεπισκόπηση: ρύθμιση των παραμέτρων διακριτικού διάρκεια ζωής σε Azure AD](active-directory-configurable-token-lifetimes.md) χρήση του PowerShell. Ανατρέξτε στο θέμα [λειτουργίες πολιτικής](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) και της [πολιτικής οντότητα](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) για λεπτομέρειες σχετικά με τη ρύθμιση μέσω του API Azure AD Graph.

## <a name="reference"></a>Αναφορά

Τα άρθρα αυτά παρέχουν μια αναφορά foundation για το ΥΠΌΛΟΙΠΟ και βιβλιοθήκη ελέγχου ταυτότητας APIs, τα πρωτόκολλα, σφάλματα, δείγματα κώδικα και τα τελικά σημεία.  

###  <a name="support"></a>Υποστήριξη
- [Ερωτήσεις με ετικέτα](http://stackoverflow.com/questions/tagged/azure-active-directory): Εύρεση Azure Active Directory λύσεις σε υπερχείλιση στοίβας κάνοντας αναζήτηση για ετικέτες [καταλόγου azure active](http://stackoverflow.com/questions/tagged/azure-active-directory) και [adal](http://stackoverflow.com/questions/tagged/adal).
- Ανατρέξτε στο θέμα [Azure AD προγραμματιστής Γλωσσάρι](active-directory-dev-glossary.md) για ορισμούς ορισμένων από τους όρους που χρησιμοποιούνται συχνά σχετικά με την ανάπτυξη εφαρμογών και ενοποίηση.

### <a name="code"></a>Κωδικός

- [Βιβλιοθήκες ανοιχτού κώδικα Azure Active Directory](http://github.com/AzureAD): είναι ο ευκολότερος τρόπος για να βρείτε μια βιβλιοθήκη προέλευσης χρησιμοποιώντας τη [λίστα της βιβλιοθήκης](active-directory-authentication-libraries.md).

- [Δείγματα Azure Active Directory](https://github.com/azure-samples?query=active-directory): Ο ευκολότερος τρόπος για να περιηγηθείτε στη λίστα των δειγμάτων είναι χρησιμοποιώντας το [ευρετήριο δείγματα κώδικα](active-directory-code-samples.md).

- [Active Directory ελέγχου ταυτότητας βιβλιοθήκης (ADAL) για το .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) - τεκμηρίωση αναφοράς είναι διαθέσιμη για [την πιο πρόσφατη κύρια έκδοση](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) και την [προηγούμενη κύρια έκδοση](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory).

### <a name="graph-api"></a>Graph API

- [Graph API αναφορά](https://msdn.microsoft.com/library/azure/hh974476.aspx): αναφορά ΥΠΌΛΟΙΠΟ για το API Azure Active Directory Graph. [Προβάλετε την αλληλεπιδραστική εμπειρία αναφορά API του γραφήματος](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Graph API δικαιωμάτων εύρους](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): διακριτικό 2.0 δικαιωμάτων εύρους που χρησιμοποιούνται για τον έλεγχο της πρόσβασης που έχει μια εφαρμογή του καταλόγου δεδομένων στο μισθωτή.

### <a name="authentication-and-authorization-protocols"></a>Έλεγχος ταυτότητας και εξουσιοδότηση πρωτόκολλα

- [Σχετικά με την είσοδο κατάδειξη αριθμό-κλειδί στο Azure AD](active-directory-signing-key-rollover.md): Μάθετε σχετικά με την είσοδο Azure AD cadence αναστροφής κλειδιού και πώς μπορείτε να ενημερώσετε το κλειδί για τα πιο συνηθισμένα σενάρια εφαρμογής.

- [Πρωτόκολλο διακριτικό 2.0: με την εκχώρηση άδειας κώδικα](active-directory-protocols-oauth-code.md): μπορείτε να χρησιμοποιήσετε εκχώρηση κώδικα εξουσιοδότησης του πρωτοκόλλου OAuth 2.0, για να εξουσιοδοτήσετε πρόσβαση σε εφαρμογές Web και μισθωτή APIs Web στο σας Azure Active Directory.

- [Πρωτόκολλο διακριτικό 2.0: Κατανόηση την μη ρητή εκχώρηση](active-directory-dev-understanding-oauth2-implicit-grant.md): Μάθετε περισσότερα σχετικά με την μη ρητή άδεια εκχώρηση, και αν είναι κατάλληλο για την εφαρμογή σας.

- [Πρωτόκολλο διακριτικό 2.0: υπηρεσία για να διαπιστευτήρια υπηρεσίας κλήσεις με χρήση προγράμματος-πελάτη](active-directory-protocols-oauth-service-to-service.md): εκχώρηση το διακριτικό 2.0 προγράμματος-πελάτη διαπιστευτήρια επιτρέπει σε μια υπηρεσία web (μια εμπιστευτικές προγράμματος-πελάτη) για να χρησιμοποιήσετε το δικό της διαπιστευτήρια για τον έλεγχο ταυτότητας όταν καλείτε μια άλλη υπηρεσία web, αντί για απομίμηση ενός χρήστη. Σε αυτό το σενάριο, το πρόγραμμα-πελάτη είναι συνήθως μια υπηρεσία web μεσαίου επιπέδου, μια υπηρεσία μονού ή τοποθεσία Web.

- [Πρωτόκολλο 1.0 σύνδεση OpenID: είσοδος και τον έλεγχο ταυτότητας](active-directory-protocols-openid-connect-code.md): πρωτόκολλο 1.0 η σύνδεση OpenID επεκτείνει διακριτικό 2.0 για χρήση ως ένα πρωτόκολλο ελέγχου ταυτότητας. Μια εφαρμογή προγράμματος-πελάτη μπορούν να λαμβάνουν ένα id_token για να διαχειριστείτε τη διαδικασία εισόδου ή αυξήσετε τη ροή κώδικα εξουσιοδότησης για να λάβετε τον κωδικό τόσο ένα id_token και εξουσιοδότησης.

- [Αναφορά πρωτόκολλο SAML 2.0](active-directory-saml-protocol-reference.md): το SAML 2.0 πρωτόκολλο επιτρέπει σε εφαρμογές να παρέχουν μια εμπειρία καθολικής σύνδεσης για τους χρήστες.

- [Πρωτόκολλο WS Ομοσπονδία 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory υποστηρίζει Ομοσπονδία WS 1.2 σύμφωνα με την προδιαγραφή Web υπηρεσίες Ομοσπονδία έκδοση 1.2. Για περισσότερες πληροφορίες σχετικά με το έγγραφο Ομοσπονδία μετα-δεδομένων, ανατρέξτε στο θέμα [Ομοσπονδία μετα-δεδομένων](active-directory-federation-metadata.md).

- [Υποστηριζόμενοι τύποι διακριτικού και αίτηση](active-directory-token-and-claims.md): μπορείτε να χρησιμοποιήσετε αυτόν τον οδηγό για να κατανοήσετε και να αξιολογήσετε το αξιώσεων στο τα διακριτικά SAML 2.0 και διακριτικά Web JSON (JWT).

## <a name="videos"></a>Βίντεο

### <a name="build"></a>Δημιουργία

Αυτές οι παρουσιάσεις Επισκόπηση στην ανάπτυξη εφαρμογών χρησιμοποιώντας τα ηχεία δυνατότητα Azure Active Directory που εργάζονται απευθείας από την ομάδα των υπηρεσιών μηχανικής. Οι παρουσιάσεις καλύπτουν θεμελιώδεις θέματα, συμπεριλαμβανομένων των IDMaaS, έλεγχος ταυτότητας, Ομοσπονδιακών ταυτοτήτων και καθολικής σύνδεσης.

- [Ταυτότητα της Microsoft: Κατάσταση της Ένωσης και μελλοντική κατεύθυνση](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Τρόπος διαχείρισης των ταυτοτήτων ως υπηρεσία για σύγχρονη εφαρμογές](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Ανάπτυξη εφαρμογών web σύγχρονο με Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Ανάπτυξη σύγχρονο εγγενείς εφαρμογές με το Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure παρασκευή
[Azure παρασκευή](https://azure.microsoft.com/documentation/videos/azure-friday/) είναι μια περιοδική παρασκευή συνεντεύξεις 1:1 σειρά βίντεο που είναι αφοσιωμένη στο να μεταφέρετε που σύντομο (10-15 λεπτά) με τους ειδικούς σε διάφορα θέματα Azure.  Χρησιμοποιήστε τη δυνατότητα φιλτραρίσματος υπηρεσίες στη σελίδα για να δείτε όλα τα βίντεο Azure Active Directory.

- [Azure ταυτότητας 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure ταυτότητας 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure ταυτότητας 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Κοινωνικές

- [Ιστολόγιο ομάδας Active Directory](http://blogs.technet.com/b/ad/): την πιο πρόσφατη εξέλιξη στον κόσμο του Azure Active Directory.

- [Ομάδα Azure Active Directory Graph ιστολογίου](http://blogs.msdn.com/b/aadgraphteam): Azure Active Directory πληροφορίες που αφορούν ειδικά για το API Graph.

- [Ταυτότητα cloud](http://www.cloudidentity.net): σκέψεις σας σχετικά με τη Διαχείριση ταυτοτήτων ως υπηρεσία, από μια κεφαλαίου Azure Active Directory μ.μ..  

- [Azure Active Directory στο Twitter](https://twitter.com/azuread): ανακοινώσεις Azure Active Directory στο 140 χαρακτήρες ή λιγότερους.

## <a name="windows-server-on-premises-development"></a>Στην ανάπτυξη εσωτερικής εγκατάστασης του Windows Server
Για οδηγίες σχετικά με τη χρήση ανάπτυξης του Windows Server και Active Directory Federation Services (ADFS), ανατρέξτε στα θέματα:

- [AD FS σενάρια για τους προγραμματιστές](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): παρέχει μια επισκόπηση των στοιχείων AD FS και πώς λειτουργεί, με λεπτομέρειες σχετικά με τα υποστηριζόμενα ελέγχου ταυτότητας/εξουσιοδότησης σενάρια.
- [Αναλυτικές παρουσιάσεις AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): μια λίστα με άρθρα Γνωρίστε, που παρέχουν οδηγίες βήμα προς βήμα σχετικά με την εφαρμογή των σχετικών ελέγχου ταυτότητας/εξουσιοδότησης ροών.
