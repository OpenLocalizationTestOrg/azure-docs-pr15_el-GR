<properties
    pageTitle="Πώς μπορείτε να ενεργοποιήσετε δημοσίευση εφαρμογών εγγενές πρόγραμμα-πελάτη με εφαρμογές του διακομιστή μεσολάβησης | Microsoft Azure"
    description="Περιγράφει τον τρόπο ενεργοποίησης εφαρμογές εγγενές πρόγραμμα-πελάτη για να επικοινωνήσετε με το Azure AD εφαρμογής διακομιστή μεσολάβησης γραμμή σύνδεσης για την παροχή ασφαλούς απομακρυσμένη πρόσβαση στις εφαρμογές εσωτερικής εγκατάστασης."
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

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Πώς μπορείτε να ενεργοποιήσετε εφαρμογές εγγενές πρόγραμμα-πελάτη για να αλληλεπιδράσετε με το διακομιστή μεσολάβησης εφαρμογές

Azure Active Directory διακομιστή μεσολάβησης εφαρμογής χρησιμοποιείται ευρέως για να δημοσιεύσετε εφαρμογές προγράμματος περιήγησης, όπως το SharePoint, το Outlook Web Access και προσαρμοσμένη γραμμή επιχειρηματικές εφαρμογές. Μπορεί επίσης να χρησιμοποιηθεί για δημοσίευση εφαρμογών εγγενές πρόγραμμα-πελάτη, οι οποίες διαφέρουν από τις εφαρμογές web, επειδή εγκαθίστανται σε μια συσκευή. Αυτό γίνεται με την υποστήριξη του Azure AD εκδοθεί διακριτικά που αποστέλλονται σε τυπική εγκρίνετε HTTP κεφαλίδες.

![Σχέση μεταξύ τελικούς χρήστες, Azure Active Directory και δημοσιευμένες εφαρμογές](./media/active-directory-application-proxy-native-client/richclientflow.png)

Η προτεινόμενη μέθοδος για τη δημοσίευση αυτών των αιτήσεων είναι να χρησιμοποιήσετε το Azure βιβλιοθήκη ελέγχου ταυτότητας AD, η οποία αναλαμβάνει όλα τα προβλήματα ελέγχου ταυτότητας και υποστηρίζει πολλές περιβάλλοντα διαφορετικό πρόγραμμα-πελάτη. Διακομιστής μεσολάβησης σχετίζεται με την [Εγγενή εφαρμογή για να το σενάριο Web API](active-directory-authentication-scenarios.md#native-application-to-web-api). Η διαδικασία για να το επιτύχετε είναι ως εξής:

## <a name="step-1-publish-your-application"></a>Βήμα 1: Δημοσίευση την εφαρμογή σας

Δημοσιεύστε την εφαρμογή του διακομιστή μεσολάβησης, όπως θα κάνατε με οποιαδήποτε άλλη εφαρμογή, εκχωρήσετε στους χρήστες και να τους δώσετε premium ή βασικές αδειών χρήσης. Για περισσότερες πληροφορίες ανατρέξτε στο θέμα [δημοσίευση εφαρμογών με το διακομιστή μεσολάβησης εφαρμογής](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Βήμα 2: Ρύθμιση των παραμέτρων σας εφαρμογής

Ρύθμιση των παραμέτρων σας εγγενούς εφαρμογής ως εξής:

1. Είσοδος στην πύλη του Azure κλασική.
2. Επιλέξτε το εικονίδιο της υπηρεσίας καταλόγου Active Directory από το αριστερό μενού και, στη συνέχεια, επιλέξτε τον κατάλογο.
3. Στην επάνω γραμμή μενού, κάντε κλικ στην επιλογή **εφαρμογές**. Εάν δεν υπάρχει εφαρμογές που έχουν προστεθεί στον κατάλογό σας, η σελίδα αυτή θα εμφανίσει μόνο στη σύνδεση **Προσθήκη εφαρμογής** . Κάντε κλικ στη σύνδεση ή Εναλλακτικά, μπορείτε να κάνετε κλικ στο κουμπί **Προσθήκη** στη γραμμή εντολών.
4. Στη σελίδα **Τι θέλετε να κάνετε** , κάντε κλικ σε μια σύνδεση για **Προσθήκη εφαρμογής ανάπτυξη την εταιρεία μου**.
5. Στη σελίδα **πείτε μας σχετικά με την εφαρμογή σας** , καθορίστε ένα όνομα για την εφαρμογή σας και επιλέξτε **εφαρμογή εγγενές πρόγραμμα-πελάτη**. Κάντε κλικ στο εικονίδιο βέλους για να συνεχίσετε.
6. Στη σελίδα **πληροφορίες εφαρμογής** , παρέχουν την **Ανακατεύθυνση URI** για την εφαρμογή εγγενές πρόγραμμα-πελάτη και, στη συνέχεια, κάντε κλικ στην επιλογή το σημάδι επιλογής για να ολοκληρώσετε.

Η εφαρμογή σας έχει προστεθεί και θα μεταφερθείτε στη σελίδα γρήγορης εκκίνησης για την εφαρμογή σας.

## <a name="step-3-grant-access-to-other-applications"></a>Βήμα 3: Εκχώρηση πρόσβασης σε άλλες εφαρμογές

Ενεργοποίηση της εγγενούς εφαρμογής να εμφανίζονται σε άλλες εφαρμογές στον κατάλογό σας:

1. Στην επάνω γραμμή μενού, κάντε κλικ στην επιλογή **εφαρμογές**, επιλέξτε τη νέα εφαρμογή εγγενών και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**.
2. Κάντε κύλιση προς τα κάτω στην ενότητα **δικαιώματα σε άλλες εφαρμογές** . Κάντε κλικ στο κουμπί **Προσθήκη εφαρμογής** και επιλέξτε την εφαρμογή διακομιστή μεσολάβησης που θέλετε να εκχωρήσετε πρόσβαση εγγενή εφαρμογή και κάντε κλικ στο σημάδι ελέγχου στην κάτω δεξιά γωνία. Από το αναπτυσσόμενο μενού **Μεταβιβάζονται δικαιώματα** , επιλέξτε το νέο δικαίωμα.

![Δικαιώματα για άλλες εφαρμογές στιγμιότυπο - Προσθήκη εφαρμογής](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Βήμα 4: Επεξεργαστείτε τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory

Επεξεργαστείτε τον κώδικα εγγενή εφαρμογή στο πλαίσιο ελέγχου ταυτότητας από το ενεργό καταλόγου ελέγχου ταυτότητας βιβλιοθήκης (ADAL) για να περιλαμβάνουν τα εξής:

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

Τις μεταβλητές πρέπει να αντικατασταθεί ως εξής:

- Μπορείτε να βρείτε **TenantId** του GUID τη διεύθυνση URL του σελίδα **Ρύθμιση παραμέτρων** της εφαρμογής, αμέσως μετά το "/ καταλόγου /".
- **Διεύθυνση URL Frontend** είναι η διεύθυνση URL προσκηνίου που καταχωρήσατε στην εφαρμογή του διακομιστή μεσολάβησης και μπορείτε να βρείτε στη σελίδα **Ρύθμιση παραμέτρων** της εφαρμογής διακομιστή μεσολάβησης.
- **Αναγνωριστικό υπολογιστή-πελάτη** της εγγενούς εφαρμογής μπορείτε να βρείτε στη σελίδα **Ρύθμιση παραμέτρων** της εγγενούς εφαρμογής.
- Μπορείτε να βρείτε **Ανακατεύθυνση URI της εγγενούς εφαρμογής** στη σελίδα **Ρύθμιση παραμέτρων** της εγγενούς εφαρμογής.

![Νέα εφαρμογή εγγενή ρύθμιση παραμέτρων στιγμιότυπο οθόνης της σελίδας](./media/active-directory-application-proxy-native-client/new_native_app.png)

Για περισσότερες πληροφορίες σχετικά με τη ροή εγγενή εφαρμογή, ανατρέξτε στο θέμα [εγγενή εφαρμογή για να web API](active-directory-authentication-scenarios.md#native-application-to-web-api).


## <a name="see-also"></a>Δείτε επίσης

- [Δημοσίευση εφαρμογών χρησιμοποιώντας το δικό σας όνομα τομέα](active-directory-application-proxy-custom-domains.md)
- [Ενεργοποίηση της πρόσβασης υπό όρους](active-directory-application-proxy-conditional-access.md)
- [Εργασία με εφαρμογές που αναγνωρίζουν αξιώσεων](active-directory-application-proxy-claims-aware-apps.md)
- [Ενεργοποίηση της καθολικής σύνδεσης σε](active-directory-application-proxy-sso-using-kcd.md)

Για τις πιο πρόσφατες ειδήσεις και ενημερώσεις, ανατρέξτε στο θέμα το [ιστολόγιο του διακομιστή μεσολάβησης εφαρμογής](http://blogs.technet.com/b/applicationproxyblog/)
