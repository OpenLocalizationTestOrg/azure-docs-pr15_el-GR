<properties
    pageTitle="Ειδοποιήσεις αναφοράς καταλόγου Azure Active Directory"
    description="Πώς μπορείτε να χρησιμοποιήσετε το Azure Active Directory αναφοράς ειδοποιήσεις για ύποπτες εισόδου πρόσθετα."
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Ειδοποιήσεις αναφοράς καταλόγου Azure Active Directory

## <a name="what-reports-generate-email-notifications"></a>Ποιες αναφορές Δημιουργία ειδοποιήσεων ηλεκτρονικού ταχυδρομείου

Προς το παρόν, μόνο ακανόνιστο είσοδος εναύσματα αναφορά δραστηριότητας ηλεκτρονικού ταχυδρομείου ειδοποιήσεις.

## <a name="what-is-an-irregular-sign-in"></a>Τι είναι μια "ακανόνιστο είσοδος";

Πρόσθετα ακανόνιστο εισόδου είναι εκείνα που έχουν προσδιοριστεί από μας μηχανικής εκμάθησης αλγόριθμους, με βάση μια συνθήκη "αδύνατη ταξιδιού" σε συνδυασμό με μια θέση ύπαρξη εισόδου και συσκευή. Αυτό μπορεί να δηλώνει ότι ένας εισβολέας έχει προσπαθείτε να πραγματοποιήσετε είσοδο με αυτόν το λογαριασμό.

## <a name="who-receives-the-email-notifications"></a>Άτομα που λαμβάνει τις ειδοποιήσεις ηλεκτρονικού ταχυδρομείου;

Το μήνυμα ηλεκτρονικού ταχυδρομείου αποστέλλεται όλες οι καθολικοί διαχειριστές που έχουν εκχωρηθεί μια άδεια χρήσης του Active Directory Premium. Για να εξασφαλίσετε παραδοθεί, θα σας το στείλετε σε διαχειριστές εναλλακτική διεύθυνση ηλεκτρονικού ταχυδρομείου καθώς και. Οι διαχειριστές θα πρέπει να συμπεριλάβετε aad-alerts-noreply@mail.windowsazure.com στη λίστα ασφαλών αποστολέων τους, ώστε να μην χάσουν το μήνυμα ηλεκτρονικού ταχυδρομείου.

## <a name="how-often-are-these-emails-sent"></a>Πόσο συχνά είναι αυτά τα μηνύματα ηλεκτρονικού ταχυδρομείου που αποστέλλονται;

Το μήνυμα ηλεκτρονικού ταχυδρομείου αποστέλλεται Εάν προκύψουν 10 νέες ακανόνιστο εισόδου δραστηριότητες τις τελευταίες 30 ημέρες ή εφόσον το τελευταίο μήνυμα ηλεκτρονικού ταχυδρομείου έχει σταλεί, όποιο από τα δύο είναι μικρότερο.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Πώς μπορώ να αποκτήσω πρόσβαση την αναφορά που αναφέρεται στο μήνυμα ηλεκτρονικού ταχυδρομείου;

Όταν κάνετε κλικ στη σύνδεση, θα ανακατευθυνθείτε στη σελίδα αναφοράς εντός της Azure κλασική πύλης. Για να έχετε πρόσβαση στην αναφορά, πρέπει να είναι και τα δύο:

- Ένα διαχειριστή ή από κοινού-διαχείριση της συνδρομής σας στο Azure

- Ο Καθολικός διαχειριστής στον κατάλογο, και έχουν εκχωρηθεί μια άδεια χρήσης του Active Directory Premium. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εκδόσεις Azure Active Directory](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Μπορώ να απενεργοποιήσω αυτά τα μηνύματα ηλεκτρονικού ταχυδρομείου;

Ναι, για να απενεργοποιήσετε τις ειδοποιήσεις που σχετίζονται με την ύπαρξη πρόσθετα εισόδου εντός της πύλης Azure κλασική, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**και, στη συνέχεια, επιλέξτε **απενεργοποιημένα** στην περιοχή **ειδοποιήσεων** .

## <a name="whats-next"></a>Τι ακολουθεί
- Ενδιαφέρει τι ασφάλεια, ελέγχου και τις αναφορές δραστηριότητας είναι διαθέσιμες; Ανατρέξτε στο θέμα [Azure AD ασφαλείας, ελέγχου, και τις αναφορές δραστηριότητας](active-directory-view-access-usage-reports.md)
- [Γρήγορα αποτελέσματα με το Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Προσθήκη εταιρείας σελίδες εμπορικής επωνυμίας στην Sign In και πίνακα της Access](active-directory-add-company-branding.md)