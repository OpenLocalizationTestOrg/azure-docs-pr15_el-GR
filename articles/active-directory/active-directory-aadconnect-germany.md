<properties
    pageTitle="Azure AD Connect στη Microsoft Cloud Γερμανία"
    description="Azure AD Connect θα ενοποίηση σας σε καταλόγους εσωτερικής εγκατάστασης με το Azure Active Directory. Αυτό σας επιτρέπει να δώσετε μια κοινή ταυτότητα για τις εφαρμογές του Office 365, Azure και ΑΔΑ ενσωματωμένο με το Azure AD."
    keywords="Εισαγωγή για να Azure AD Connect, επισκόπηση Azure AD Connect, τι είναι το Azure AD Connect, εγκαταστήστε υπηρεσίας καταλόγου active directory, Γερμανία, μαύρο δάσος"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect στη Microsoft Cloud Γερμανία - δημόσια Preview

## <a name="introduction"></a>Εισαγωγή
Azure AD Connect παρέχει συγχρονισμό μεταξύ του καταλόγου Active Directory εσωτερικής εγκατάστασης και Azure Active Directory.
Προς το παρόν, πολλά από τα σενάρια στη [Microsoft Cloud Γερμανία](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) πρέπει να εκτελεστούν από τον τελεστή. Όταν χρησιμοποιείτε το Microsoft Cloud Γερμανία, θα πρέπει να έχετε υπόψη σας τα εξής:


- Οι ακόλουθες διευθύνσεις URL πρέπει να ανοίγει σε ένα διακομιστή μεσολάβησης για το συγχρονισμό για να προκύψει με επιτυχία:
    - *. microsoftonline.de
    - *. των windows.net
    - + Οι λίστες ανάκλησης πιστοποιητικών

- Κατά την είσοδο στον κατάλογό σας Azure AD, πρέπει να χρησιμοποιείτε ένα λογαριασμό στον τομέα onmicrosoft.de.
- Οι παρακάτω δυνατότητες δεν είναι διαθέσιμες:
    - Azure AD Connect εύρυθμης λειτουργίας
    - Αυτόματες ενημερώσεις
    - Αντιγραφή κωδικού πρόσβασης

## <a name="download"></a>Λήψη
Μπορείτε να κάνετε λήψη Azure AD Connect από το Azure AD Connect blade εντός της πύλης.  Χρησιμοποιήστε τις παρακάτω οδηγίες για να εντοπίσετε το blade Azure AD Connect.

### <a name="the-azure-ad-connect-blade"></a>Το Azure AD Connect Blade

Όταν έχετε πραγματοποιήσει είσοδο στην πύλη του Azure, κάντε τα εξής:

1. Μεταβείτε στην αναζήτηση
2.  Επιλέξτε καταλόγου Azure Active Directory
3.  Στη συνέχεια, επιλέξτε Azure AD Connect

Θα πρέπει να δείτε τα εξής:

![Azure AD Connect Blade](media\active-directory-aadconnect-germany\germany1.png)

 
Ο παρακάτω πίνακας περιγράφει τις δυνατότητες που εμφανίζονται στο το blade.


Τίτλος|Περιγραφή|
----- | ----- |
ΚΑΤΆΣΤΑΣΗ ΣΥΓΧΡΟΝΙΣΜΟΎ|Ας γνωρίζετε εάν ο συγχρονισμός είναι ενεργοποιημένη ή απενεργοποιημένη.|
ΤΕΛΕΥΤΑΊΑ ΣΥΓΧΡΟΝΙΣΜΟΎ|Την τελευταία φορά μια επιτυχής συγχρονισμός ολοκληρώθηκε.|
ΤΟΜΕΊΣ|Εμφανίζει τον αριθμό των τομείς που έχουν ρυθμιστεί.|


## <a name="installation"></a>Εγκατάσταση
Για να εγκαταστήσετε Azure AD Connect, μπορείτε να χρησιμοποιήσετε την τεκμηρίωση [εδώ](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Δυνατότητες για προχωρημένους και πρόσθετες πληροφορίες
Για πρόσθετες πληροφορίες και οδηγίες σχετικά με προσαρμοσμένες ρυθμίσεις ή σύνθετες ρυθμίσεις παραμέτρων, ξεκινήστε με την [ενοποίηση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).  Αυτή η σελίδα παρέχει πληροφορίες και συνδέσεις για πρόσθετες οδηγίες.
