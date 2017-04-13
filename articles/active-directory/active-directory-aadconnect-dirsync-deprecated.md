<properties
    pageTitle="Αναβάθμιση από το DirSync και Azure AD Sync | Microsoft Azure"
    description="Περιγράφει πώς μπορείτε να κάνετε αναβάθμιση από το DirSync και Azure AD Sync στο Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Αναβάθμιση του συγχρονισμού καταλόγου Azure Active Directory των Windows ("DirSync") και ο συγχρονισμός καταλόγου Azure Active Directory ("Azure AD Sync")
Azure AD Connect είναι ο καλύτερος τρόπος για να συνδεθείτε στον κατάλογο εσωτερικής εγκατάστασης με το Azure AD και το Office 365. Αυτή είναι μια εξαιρετική ώρα για αναβάθμιση σε Azure AD Connect από το Windows Azure Active Directory Sync (DirSync) ή Azure AD Sync, όπως αυτά τα εργαλεία τώρα έχουν καταργηθεί και θα φτάσει στο τέλος της υποστήριξης σε 13 Απριλίου 2017.

Τα εργαλεία συγχρονισμού δύο ταυτότητας που έχουν καταργηθεί έχουν προσφέρεται για μία μόνο δάσος πελάτες (DirSync) και για πολλαπλή δάσος και άλλα για προχωρημένους πελάτες (Azure AD Sync). Αυτά τα εργαλεία παλαιότερων έχουν αντικατασταθεί με μια λύση που είναι διαθέσιμη για όλα τα σενάρια: Azure AD Connect. Προσφέρει νέες λειτουργίες, οι βελτιώσεις δυνατοτήτων και υποστήριξη για νέα σενάρια. Για να μπορέσετε να συνεχίσετε να συγχρονίσετε τα δεδομένα σας ταυτοτήτων εσωτερικής εγκατάστασης για να Azure AD και Office 365, σας συνιστούμε να κάνετε αναβάθμιση στο Azure AD Connect.

Η τελευταία έκδοση του DirSync έχει κυκλοφορήσει στο Ιούλιος 2014 και η τελευταία έκδοση του Azure AD Sync έχει κυκλοφορήσει στο Μάιος 2015.

## <a name="what-is-azure-ad-connect"></a>Τι είναι το Azure AD Connect
Azure AD Connect είναι η εξαρτώμενη εργασία να DirSync και Azure AD Sync. Συνδυάζει όλα τα σενάρια αυτοί οι δύο υποστηρίζονται. Μπορείτε να διαβάσετε περισσότερα σχετικά με την από την [ενοποίηση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Χρονοδιάγραμμα με

Ημερομηνία | Σχόλιο
 --- | ---
13 Απριλίου 2016 | Windows Azure Active Directory Sync ("DirSync") και Microsoft Azure Active Directory Sync ("Azure AD Sync") είναι ανακοινώσει όπως καταργηθεί.
13 Απριλίου 2017 | Υποστήριξη τελειώνει. Οι πελάτες δεν είναι πλέον θα μπορείτε να ανοίξετε μια υπόθεση υποστήριξης χωρίς να αναβαθμίσετε Azure AD Connect πρώτα.

## <a name="how-to-transition-to-azure-ad-connect"></a>Τρόπος μετάβασης σε Azure AD Connect
Εάν εκτελείτε DirSync υπάρχουν δύο τρόποι που μπορείτε να κάνετε αναβάθμιση: στην ίδια θέση αναβάθμισης ή παράλληλα ανάπτυξης. Μια άμεση αναβάθμιση συνιστάται για τους περισσότερους πελάτες και εάν έχετε ένα πρόσφατο λειτουργικό σύστημα και μικρότερη από 50.000 αντικείμενα. Σε άλλες περιπτώσεις συνιστάται να κάνετε μια παράλληλη ανάπτυξη όπου της ρύθμισης παραμέτρων του DirSync μετακινείται στο νέο διακομιστή που εκτελεί Azure AD Connect.

Εάν χρησιμοποιείτε μια άμεση αναβάθμιση συνιστάται το Azure AD Sync. Εάν θέλετε να, είναι δυνατό να εγκαταστήσετε μια νέα διακομιστής Azure AD Connect παράλληλα και να κάνετε μια μετεγκατάσταση εναλλασσόμενης από το διακομιστή σας Azure AD Sync στο Azure AD Connect.

Λύση | Σενάριο
----- | -----
[Αναβάθμιση από DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Εάν έχετε μια υπάρχουσα διακομιστής DirSync εκτελείται ήδη.</li>
[Αναβάθμιση από το Azure AD Sync](active-directory-aadconnect-upgrade-previous-version.md)| <li>Εάν θέλετε να μετακινήσετε από το Azure AD Sync.</li>

Εάν θέλετε να δείτε πώς μπορείτε να κάνετε μια άμεση αναβάθμιση από το DirSync για να Azure AD Connect, στη συνέχεια, ανατρέξτε σε αυτό το βίντεο 9 κανάλι:

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
**Ε: μπορώ να έχετε λάβει μια ειδοποίηση ηλεκτρονικού ταχυδρομείου από την ομάδα του Azure ή/και ένα μήνυμα από το Κέντρο μηνυμάτων του Office 365, αλλά χρησιμοποιώ σύνδεση.**  
Ειδοποίηση στάλθηκε επίσης για τους πελάτες που χρησιμοποιούν Azure AD Connect με έναν αριθμό build 1.0. \*.0 (χρησιμοποιώντας μια έκδοση pre 1.1). Η Microsoft συνιστά στους πελάτες να παραμείνετε ενημερωμένοι Azure AD Connect εκδόσεις. 1.1 τη δυνατότητα [αυτόματης αναβάθμισης](active-directory-aadconnect-feature-automatic-upgrade.md) θα είναι πιο εύκολα να έχετε πάντα μια πρόσφατη έκδοση του Azure AD Connect εγκατεστημένες.

**Ε: θα DirSync/Azure AD Sync σταματήσουν να λειτουργούν σε 13 Απριλίου 2017;**  
Όχι. Η ημερομηνία όταν αυτές δεν θα είναι πλέον δυνατή η επικοινωνία με το Azure AD θα είναι ανακοινώσει αργότερα. Θα μπορείτε να βρείτε αυτές τις πληροφορίες σε αυτό το θέμα όταν είναι διαθέσιμες.

**Ε: ποιες εκδόσεις DirSync μπορώ να κάνω αναβάθμιση από;**  
Υποστηρίζεται για να κάνετε αναβάθμιση από οποιαδήποτε έκδοση DirSync που χρησιμοποιείται τη συγκεκριμένη στιγμή.

**Ε: Τι γίνεται με το Azure AD Connector για FIM/MIM;**  
Η γραμμή σύνδεσης Azure AD για FIM/MIM **δεν** έχει ανακοινώθηκε όπως καταργηθεί. Να έχει **δυνατότητα σταθεροποίηση**; χωρίς νέες λειτουργίες προστίθεται και λαμβάνει χωρίς διορθώσεις σφαλμάτων. Η Microsoft συνιστά οι πελάτες που χρησιμοποιείτε για το σχεδιασμό για να μετακινηθείτε από το Azure AD Connect. Συνιστάται να μην εκκινούν οποιαδήποτε νέα αναπτύξεις χρησιμοποιώντας το. Αυτή η γραμμή σύνδεσης θα είναι ανακοινώσει καταργηθεί στο μέλλον.

## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Ενοποίηση σας ταυτοτήτων εσωτερικής εγκατάστασης με το Azure Active Directory](active-directory-aadconnect.md)
