<properties
    pageTitle="Τη ρύθμιση του Azure Active Directory για διαχείριση πρόσβασης εφαρμογών αυτοεξυπηρέτησης | Microsoft Azure"
    description="Διαχείριση ομάδας από το χρήστη επιτρέπει στους χρήστες να δημιουργήσετε και να διαχειριστείτε τις ομάδες ασφαλείας ή ομάδων του Office 365 στην υπηρεσία καταλόγου Azure Active Directory και προσφέρει στους χρήστες τη δυνατότητα να ομάδα ασφαλείας αίτηση ή μέλη ομάδας του Office 365"
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Ρύθμιση Azure Active Directory για τη Διαχείριση ομάδας από το χρήστη

Διαχείριση ομάδας από το χρήστη επιτρέπει στους χρήστες για να δημιουργήσετε και να διαχειριστείτε τις ομάδες ασφαλείας ή ομάδων του Office 365 στο Azure Active Directory (Azure AD). Οι χρήστες να ζητήσετε επίσης ομάδα ασφαλείας ή μέλη ομάδας του Office 365 και, στη συνέχεια, τον κάτοχο της ομάδας μπορούν να εγκρίνουν ή να απορρίψετε την ιδιότητα μέλους. Με αυτόν τον τρόπο, καθημερινή τον έλεγχο της ιδιότητα μέλους ομάδας μπορεί να ανατεθεί σε άτομα που κατανοούν περιβάλλον επιχειρήσεις για αυτήν την ιδιότητα μέλους. Δυνατότητες διαχείρισης ομάδα από το χρήστη είναι διαθέσιμες μόνο για τις ομάδες ασφαλείας και ομάδων του Office 365, αλλά όχι για τις ομάδες ασφαλείας με δυνατότητα mail ή λίστες διανομής.

Διαχείριση ομάδας από το χρήστη αυτήν τη στιγμή αποτελείται από δύο βασικά σενάρια: Διαχείριση ομάδας από το χρήστη και ομάδα διαχείρισης με ανάθεση.

- **Ομάδα διαχείρισης με ανάθεση** 
   ένα παράδειγμα είναι ο διαχειριστής που η διαχείριση της πρόσβασης σε μια εφαρμογή ΑΔΑ που χρησιμοποιεί η εταιρεία. Διαχείριση αυτά τα δικαιώματα πρόσβασης γίνεται εύκολα, ώστε ο διαχειριστής σας ρωτά ο κάτοχος της επιχείρησης για να δημιουργήσετε μια νέα ομάδα. Ο διαχειριστής εκχωρεί πρόσβαση για την εφαρμογή στη νέα ομάδα και προσθέτει στην ομάδα όλα τα άτομα που ήδη πρόσβαση στην εφαρμογή. Ο κάτοχος της επιχείρησης, στη συνέχεια, να προσθέσετε περισσότερους χρήστες και αυτοί οι χρήστες έχουν παρασχεθεί αυτόματα με την εφαρμογή. Ο κάτοχος της επιχείρησης δεν χρειάζεται να περιμένουν για το διαχειριστή για να διαχειριστείτε την πρόσβαση για τους χρήστες. Εάν ο διαχειριστής εκχωρεί τα ίδια δικαιώματα σε ένα διαχειριστή σε μια ομάδα διαφορετική επιχειρηματική, στη συνέχεια, αυτό το άτομο επίσης να διαχειριστείτε την πρόσβαση για το δικό τους χρήστες. Ο κάτοχος της επιχείρησης ούτε ο διαχειριστής μπορεί να προβάλετε ή να διαχειριστείτε μεταξύ τους χρήστες. Ο διαχειριστής μπορεί να εξακολουθείτε να βλέπετε όλους τους χρήστες που έχουν πρόσβαση στην εφαρμογή και δικαιώματα πρόσβασης μπλοκ εάν είναι απαραίτητο.

- **Διαχείριση ομάδας από το χρήστη** 
   ένα παράδειγμα αυτό το σενάριο είναι δύο χρήστες με δύο τοποθεσίες του SharePoint Online που ρύθμιση ανεξάρτητα. Που θέλουν να δώσετε πρόσβαση στις ομάδες του άλλου τους τοποθεσίες. Για να γίνει αυτό, να μπορούν να δημιουργήσουν μια ομάδα στο Azure AD και στο SharePoint Online, κάθε μία από αυτές επιλέγει αυτήν την ομάδα για να παρέχουν πρόσβαση σε τοποθεσίες τους. Όταν κάποιος ξεκινά access, που ζητούν από τον πίνακα της Access και μετά την έγκριση μπορεί αποκτήσει αυτόματα πρόσβαση σε δύο τοποθεσίες του SharePoint Online. Αργότερα, μία από αυτές αποφασίσει ότι όλα τα άτομα την πρόσβαση στην τοποθεσία θα πρέπει επίσης να αποκτήσετε πρόσβαση σε μια συγκεκριμένη εφαρμογή ΑΔΑ. Ο διαχειριστής της εφαρμογής ΑΔΑ να προσθέσετε δικαιώματα πρόσβασης για την εφαρμογή στην τοποθεσία του SharePoint Online. Από και, στη συνέχεια, στο, τα αιτήματα που να εγκριθεί παρέχει πρόσβαση στις δύο τοποθεσίες SharePoint Online, καθώς και σε αυτήν την εφαρμογή ΑΔΑ.

## <a name="making-a-group-available-for-end-user-self-service"></a>Διάθεση μια ομάδα για τελικούς χρήστες από το χρήστη

1. Στην [πύλη του Azure κλασική](https://manage.windowsazure.com), ανοίξτε τον κατάλογο Azure AD.

2. Στην καρτέλα **Ρύθμιση παραμέτρων** , ορίστε **Διαχείριση ομάδας ανάθεση** σε ενεργοποίηση.

3. Ορίστε **τους χρήστες να δημιουργήσετε ομάδες ασφαλείας** ή **οι χρήστες μπορούν να δημιουργήσουν ομάδες του Office** σε ενεργοποίηση.

Όταν **οι χρήστες μπορούν να δημιουργήσουν τις ομάδες ασφαλείας** είναι ενεργοποιημένη, όλοι οι χρήστες στον κατάλογό σας επιτρέπεται να δημιουργήσετε νέες ομάδες ασφαλείας και να προσθέσετε μέλη σε αυτές τις ομάδες. Αυτές οι νέες ομάδες επίσης θα εμφανίζονται στο πλαίσιο πρόσβασης για όλους τους άλλους χρήστες. Εάν η ρύθμιση πολιτικής στην ομάδα το επιτρέπει, οι άλλοι χρήστες μπορεί να δημιουργήσει αιτήσεις για να συμμετάσχετε σε αυτές τις ομάδες. Εάν **οι χρήστες μπορούν να δημιουργήσουν τις ομάδες ασφαλείας** είναι απενεργοποιημένη, οι χρήστες δεν είναι δυνατό να δημιουργήσετε ομάδες και είναι δυνατή η αλλαγή υπάρχουσες ομάδες για τις οποίες είναι έναν κάτοχο. Ωστόσο, εξακολουθούν να μπορούν διαχείριση των μελών αυτών των ομάδων και έγκριση αιτήσεις από άλλους χρήστες για να συμμετάσχετε σε ομάδες τους.

Μπορείτε επίσης να χρησιμοποιήσετε **τους χρήστες που μπορούν να χρησιμοποιούν από το χρήστη για τις ομάδες ασφαλείας** για να επιτύχετε μια πιο λεπτομερή access έλεγχο Διαχείριση ομάδας από το χρήστη για τους χρήστες σας. Όταν **οι χρήστες μπορούν να δημιουργήσουν ομάδες** είναι ενεργοποιημένη, όλοι οι χρήστες στον κατάλογό σας επιτρέπεται να δημιουργήσετε νέες ομάδες και να προσθέσετε μέλη σε αυτές τις ομάδες. Ορίζοντας επίσης **ποιος μπορεί να χρησιμοποιήσει από το χρήστη για τις ομάδες ασφαλείας οι χρήστες** σε ορισμένες, είστε περιορισμού ομάδας διαχείρισης μόνο σε περιορισμένη ομάδα χρηστών. Όταν αυτός ο διακόπτης έχει οριστεί σε ορισμένες, πρέπει να προσθέσετε χρήστες στην ομάδα SSGMSecurityGroupsUsers πριν μπορούν να δημιουργήσετε νέες ομάδες και να τους προσθέσετε μέλη. Ορίζοντας **χρήστες ποιος μπορεί να χρησιμοποιήσει από το χρήστη για τις ομάδες ασφαλείας** σε όλους, μπορείτε να ενεργοποιήσετε όλους τους χρήστες στον κατάλογό σας, για να δημιουργήσετε νέες ομάδες.

Μπορείτε επίσης να χρησιμοποιήσετε το πλαίσιο **ομάδα που μπορεί να χρησιμοποιήσει από το χρήστη για τις ομάδες ασφαλείας** για να καθορίσετε ένα προσαρμοσμένο όνομα για την ομάδα των οποίων τα μέλη να χρησιμοποιήσετε από το χρήστη.

## <a name="additional-information"></a>Πρόσθετες πληροφορίες

Τα άρθρα αυτά παρέχουν πρόσθετες πληροφορίες σχετικά με Azure Active Directory.

* [Διαχείριση της πρόσβασης σε πόρους με ομάδες Azure Active Directory](active-directory-manage-groups.md)

* [Azure Active Directory cmdlet για τη ρύθμιση των παραμέτρων ρυθμίσεων ομάδας](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)

* [Τι είναι το Azure Active Directory;](active-directory-whatis.md)

* [Ενοποίηση του ταυτοτήτων εσωτερικής εγκατάστασης με το Azure Active Directory](active-directory-aadconnect.md)