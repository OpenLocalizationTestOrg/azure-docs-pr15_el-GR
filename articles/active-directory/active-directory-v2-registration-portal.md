<properties
    pageTitle="Καταχώρηση εφαρμογής πύλης θέματα στη Βοήθεια | Microsoft Azure"
    description="Μια περιγραφή του διάφορες δυνατότητες στην πύλη εγγραφής του Microsoft εφαρμογής."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="app-registration-reference"></a>Αναφορά καταχώρησης εφαρμογής
Αυτό το έγγραφο παρέχει περιβάλλοντος και περιγραφές για διάφορες δυνατότητες που υπάρχουν στις την πύλη καταχώρηση εφαρμογής Microsoft [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Οι εφαρμογές μου
Αυτή η λίστα περιέχει όλες τις εφαρμογές που έχουν καταχωρηθεί για χρήση με το τελικό σημείο v2.0 Azure AD.  Αυτές οι εφαρμογές έχουν τη δυνατότητα να εισέλθετε οι χρήστες με δύο προσωπικοί λογαριασμοί από το εταιρικό/σχολικό λογαριασμούς από το Azure Active Directory και λογαριασμό Microsoft που διαθέτετε.  Για να μάθετε περισσότερα σχετικά με το τελικό σημείο v2.0 Azure AD, ανατρέξτε στο θέμα μας [Επισκόπηση v2.0](active-directory-appmodel-v2-overview.md).  Αυτές οι εφαρμογές μπορούν να χρησιμοποιηθούν για την ενοποίηση με το τελικό σημείο ελέγχου ταυτότητας λογαριασμού Microsoft, `https://login.live.com`.

## <a name="live-sdk-applications"></a>Εφαρμογές του Live SDK
Αυτή η λίστα περιέχει όλες τις εφαρμογές που έχουν καταχωρηθεί για χρήση αποκλειστικά με λογαριασμό Microsoft που διαθέτετε.  Δεν είναι ενεργοποιημένες για χρήση με το Azure Active Directory οποιαδήποτε.  Αυτό είναι το σημείο όπου θα βρείτε τις εφαρμογές που είχε ήδη καταχωρηθεί με την πύλη για προγραμματιστές MSA στο `https://account.live.com/developers/applications`.  Όλες οι συναρτήσεις που έχετε εκτελέσει προηγουμένως στο `https://account.live.com/developers/applications` τώρα μπορεί να εκτελεστεί σε αυτήν την νέα πύλη, `https://apps.dev.microsoft.com`.  Εάν έχετε περισσότερες ερωτήσεις σχετικά με τις εφαρμογές σας λογαριασμό Microsoft, επικοινωνήστε μαζί μας.

## <a name="application-secrets"></a>Εφαρμογή απορρήτου
Εφαρμογή απόρρητο είναι τα διαπιστευτήρια που επιτρέπουν την εφαρμογή σας για να εκτελέσετε αξιόπιστη [ελέγχου ταυτότητας προγράμματος-πελάτη](http://tools.ietf.org/html/rfc6749#section-2.3) με το Azure AD.  Στο OAuth & σύνδεση OpenID, μια εφαρμογή απόρρητο συνήθως αναφέρεται ως μια `client_secret`.  Στο πρωτόκολλο v2.0, οποιαδήποτε εφαρμογή που λαμβάνει έναν κωδικό ασφαλείας σε μια θέση web μπορούν να χρησιμοποιηθούν (με χρήση μιας `https` συνδυασμό) πρέπει να χρησιμοποιεί μια εφαρμογή μυστικό αναγνώριση της ταυτότητάς του Azure AD κατά την εξαργύρωση που διακριτικού ασφαλείας.  Επιπλέον, οποιοδήποτε εγγενές πρόγραμμα-πελάτη, διακριτικά που λαμβάνει σε μια συσκευή θα απαγορεύεται από τη χρήση μιας εφαρμογής μυστικό για να εκτελέσετε τον έλεγχο ταυτότητας προγράμματος-πελάτη, ώστε να αποθαρρύνει την αποθήκευση του απορρήτου σε μη ασφαλή περιβάλλοντα.

Κάθε εφαρμογή μπορεί να περιέχει δύο απόρρητο έγκυρη εφαρμογή σε οποιοδήποτε σημείο δεδομένη στιγμή.  Διατηρώντας δύο απόρρητο, έχετε την ablilty για να εκτελέσετε περιοδικό αναστροφής κλειδιού σε ολόκληρο το περιβάλλον της εφαρμογής σας.  Μόλις μετεγκατάσταση το σύνολο της εφαρμογής σας σε μια νέα μυστικό, που μπορεί να διαγράψτε το παλιό μυστικό και παροχή ένα νέο.

Αυτήν τη στιγμή, μόνο δύο τύποι του απορρήτου εφαρμογή επιτρέπεται στην πύλη καταχώρηση εφαρμογής.  Επιλογή **Δημιουργία νέου κωδικού πρόσβασης** θα δημιουργήσετε και να αποθηκεύσετε ένα κοινό μυστικό στο χώρο αποθήκευσης αντίστοιχο δεδομένων, το οποίο μπορείτε να χρησιμοποιήσετε στην εφαρμογή σας.  Επιλογή **Δημιουργία νέου ζεύγους κλειδιών** θα δημιουργήσετε ένα νέο ζεύγος κλειδιού δημόσια/ιδιωτική που μπορούν να έχουν ληφθεί και χρησιμοποιούνται για τον έλεγχο ταυτότητας του προγράμματος-πελάτη για να Azure AD.

## <a name="profile"></a>Προφίλ
Στην ενότητα προφίλ της πύλης καταχώρηση εφαρμογής μπορεί να χρησιμοποιηθεί για να προσαρμόσετε το στη σελίδα εισόδου για την εφαρμογή σας.  Αυτήν τη στιγμή που μπορούν να τροποποιήσουν είσοδος λογότυπο εφαρμογής της σελίδας, τους όρους διεύθυνση URL της υπηρεσίας και τη δήλωση προστασίας προσωπικών δεδομένων.  Το λογότυπο δεν πρέπει να είναι διαφανούς 48 x 48 ή 50 x 50 pixel εικόνας σε ένα αρχείο GIF, PNG ή JPEG που είναι 15 KB ή μικρότερο.  Δοκιμάστε να αλλάξετε τις τιμές και προβολή που προκύπτει είσοδος στη σελίδα!

## <a name="live-sdk-support"></a>Υποστήριξη Live SDK
Όταν ενεργοποιείτε "Live SDK υποστήριξη", οποιαδήποτε εφαρμογή του απορρήτου δημιουργείτε θα αποδοθεί σε τόσο το Azure AD και αποθηκεύει δεδομένα λογαριασμό Microsoft που διαθέτετε.  Αυτό θα σας επιτρέψει να ενοποιήσετε απευθείας με την υπηρεσία λογαριασμό Microsoft (login.live.com) η εφαρμογή σας.  Εάν θέλετε να δημιουργήσετε μια εφαρμογή χρησιμοποιώντας λογαριασμό Microsoft απευθείας (αντί για χρησιμοποιώντας το τελικό σημείο v2.0 Azure AD), θα πρέπει να βεβαιωθείτε ότι είναι ενεργοποιημένη η υποστήριξη SDK Live.

Απενεργοποίηση της υποστήριξης Live SDK θα βεβαιωθείτε ότι το μυστικό της εφαρμογής προορίζεται μόνο τα δεδομένα του Azure AD αποθήκευση.  Τα δεδομένα του Azure AD store ενσωματώνει κανονισμών επίπεδου μεγάλης εταιρείας που σας επιτρέπει να ικανοποιούν ορισμένα πρότυπα, όπως FISMA συμμόρφωσης.  Εάν ενεργοποιήσετε την υποστήριξη Live SDK, η εφαρμογή σας δεν μπορεί να επιτύχετε συμμόρφωση με ορισμένα από αυτά τα πρότυπα.

Εάν σκοπεύετε κάποτε μόνο για να χρησιμοποιήσετε το τελικό σημείο v2.0 Azure AD, μπορείτε να απενεργοποιήσετε με ασφάλεια Live SDK υποστήριξης.

