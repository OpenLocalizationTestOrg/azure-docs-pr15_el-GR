<properties
    pageTitle="Azure ειδοποιήσεις προστασία ταυτότητας Active Directory | Microsoft Azure"
    description="Μάθετε πώς ειδοποιήσεις υποστηρίζει τις δραστηριότητες έρευνας."
    services="active-directory"
    keywords="προστασία ταυτότητας καταλόγου Azure active directory, cloud εφαρμογή εντοπισμού, τη Διαχείριση εφαρμογών, ασφαλείας, κινδύνου, επίπεδο κινδύνου, ευπάθεια, πολιτική ασφαλείας"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-notifications"></a>Azure ειδοποιήσεις προστασία ταυτότητας Active Directory 


Azure προστασία ταυτότητας AD αποστέλλει δύο τύπους αυτοματοποιημένη ειδοποίηση μηνύματα ηλεκτρονικού ταχυδρομείου σας βοηθούν στη διαχείριση κινδύνου χρήστη και συμβάντα κινδύνου:

- Χρήστης έχει παραβιαστεί ειδοποίησης ηλεκτρονικού ταχυδρομείου

- Εβδομαδιαία σύνοψη ηλεκτρονικού ταχυδρομείου

## <a name="user-compromised-alert-email"></a>Χρήστης έχει παραβιαστεί ειδοποίησης ηλεκτρονικού ταχυδρομείου

Μια ειδοποίηση ηλεκτρονικού ταχυδρομείου έχει παραβιαστεί χρήστη δημιουργείται όταν προστασία Azure AD ταυτότητας προσδιορίζει ένα λογαριασμό που έχει παραβιαστεί. Το μήνυμα ηλεκτρονικού ταχυδρομείου περιλαμβάνει μια σύνδεση για τους χρήστες με σημαία για αναφορά κινδύνου στον πίνακα εργαλείων προστασία ταυτότητας. Συνιστάται να ερευνήσετε αμέσως ειδοποιήσεις έχει παραβιαστεί.


## <a name="weekly-digest-email"></a>Εβδομαδιαία σύνοψη ηλεκτρονικού ταχυδρομείου

Μήνυμα ηλεκτρονικού ταχυδρομείου εβδομαδιαία σύνοψη περιέχει μια σύνοψη των νέων συμβάντων κινδύνου.<br>
Αυτό περιλαμβάνει τα εξής:

- Οι χρήστες σε κίνδυνο
- Ύποπτες δραστηριότητες
- Εντοπίστηκε ευπάθειες
- Συνδέσεις προς τις σχετικές αναφορές στην προστασία ταυτότητας


<br>
![Αποκατάσταση εύρυθμης λειτουργίας](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br> 

Μπορείτε να πραγματοποιήσετε εναλλαγή αποστολή ένα εβδομαδιαίο ηλεκτρονικού ταχυδρομείου σύνοψη.
<br><br>
![Κίνδυνοι χρήστη](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>
 

**Για να ανοίξετε το παράθυρο διαλόγου ρύθμισης παραμέτρων που αφορούν**:

1. Η **προστασία Azure AD ταυτότητας** blade, επιλέξτε **Ρυθμίσεις**.
<br><br>
![Πολιτική κινδύνου χρήστη](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
<br>

2. Στην ενότητα **Γενικά** , κάντε κλικ στην επιλογή **ειδοποιήσεις**.
<br><br>
![Πολιτική κινδύνου χρήστη](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
<br>




## <a name="see-also"></a>Δείτε επίσης

- [Προστασία ταυτότητας καταλόγου Azure Active Directory](active-directory-identityprotection.md) 

