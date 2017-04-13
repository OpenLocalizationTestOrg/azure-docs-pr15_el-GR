<properties
   pageTitle="Μορφή αρχείου CSV για προεπισκόπηση συνεργασίας Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B υποστηρίζει τις σχέσεις μεταξύ εταιρεία σας, ενεργοποιώντας συνεργάτες επιλεκτικής πρόσβασης εταιρικών εφαρμογών σας"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Προεπισκόπηση συνεργασίας Azure AD B2B: μορφή αρχείου CSV

Η έκδοση προεπισκόπησης της συνεργασίας Azure AD B2B απαιτεί ένα αρχείο CSV που καθορίζει πληροφορίες χρήστη συνεργάτη να αποσταλεί μέσω της πύλης Azure AD. Το αρχείο CSV πρέπει να περιέχει το παρακάτω απαιτείται ετικέτες και προαιρετικά πεδία όπως απαιτείται. Τροποποιήστε το δείγμα αρχείου CSV (παρακάτω) χωρίς να αλλάξετε την ορθογραφία των ετικετών στην πρώτη γραμμή.

>[AZURE.NOTE] Η πρώτη γραμμή του ετικέτες (όπως ηλεκτρονικού ταχυδρομείου, το εμφανιζόμενο όνομα και ούτω καθεξής) είναι απαραίτητη για το αρχείο CSV για να αναλυθούν με επιτυχία. Την ορθογραφία πρέπει να συμφωνεί με τα πεδία που καθορίζονται παρακάτω. Αυτές οι ετικέτες προσδιορίστε το περιεχόμενο κάτω από αυτές. Για τα προαιρετικά πεδία που δεν είναι απαραίτητες, ετικέτες τους μπορούν να καταργηθούν από το αρχείο CSV. Ολόκληρη η στήλη μπορεί να παραμείνουν κενά.

## <a name="required-fields-br"></a>Τα απαιτούμενα πεδία: <br/>
**Ηλεκτρονικού ταχυδρομείου:** Διεύθυνση ηλεκτρονικού ταχυδρομείου των προσκεκλημένων χρήστη. <br/>
**DisplayName:** Εμφανιζόμενο όνομα για ο προσκεκλημένος χρήστης (συνήθως, πρώτο και το τελευταίο όνομα).<br/>


## <a name="optional-fields-br"></a>Προαιρετικά πεδία: <br/>

**InvitationText:** Προσαρμογή κειμένου της πρόσκλησης ηλεκτρονικού ταχυδρομείου μετά την εφαρμογή εμπορική προσαρμογή και πριν από τη σύνδεση εξαργύρωση.<br/>
**InvitedToApplications:** AppIDs στις εταιρικές εφαρμογές για να εκχωρήσετε στους χρήστες. AppIDs είναι ανακτήσιμη σε PowerShell καλώντας`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** ObjectIDs για τις ομάδες να προσθέσετε χρήστη. ObjectIDs είναι ανακτήσιμη σε PowerShell καλώντας`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:** Διεύθυνση URL για να κατευθύνετε μια ο προσκεκλημένος χρήστης μετά την αποδοχή πρόσκλησης. Αυτό πρέπει να είναι μια διεύθυνση URL ειδικά για την εταιρεία (όπως [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). Εάν δεν έχει καθοριστεί σε αυτό το πεδίο προαιρετικά, ο προσκεκλημένος χρήστης κατευθύνεται στο πλαίσιο πρόσβασης εφαρμογή όπου μπορεί να περιηγηθείτε στις εφαρμογές σας επιλεγμένο εταιρικό. Η διεύθυνση URL εφαρμογής Access πίνακας έχει τη μορφή `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: διεύθυνση για να αντιγράψετε αλληλογραφία πρόσκληση ηλεκτρονικού ταχυδρομείου. Εάν χρησιμοποιείται το πεδίο CcEmailAddress, αυτήν την πρόσκληση δεν μπορούν να χρησιμοποιηθούν για επαλήθευση ηλεκτρονικού ταχυδρομείου χρήστη ή τη δημιουργία μισθωτή.<br/>
**Γλώσσα:** Γλώσσα για πρόσκληση ηλεκτρονικού ταχυδρομείου και εξαργύρωση εμπειρία, με το "en" (στα Αγγλικά) ως προεπιλογή όταν δεν έχει καθοριστεί. Οι άλλες 10 υποστηριζόμενες γλώσσας κωδικοί είναι:<br/>
1. de: Γερμανικά<br/>
2. ES: Ισπανικά<br/>
3. FR: Γαλλικά<br/>
4. το: Ιταλικά<br/>
5. Ja: Ιαπωνικά<br/>
6. Ko: Κορεατικά<br/>
7. PT BR: Πορτογαλικά (Βραζιλίας)<br/>
8. RU: Ρωσικά<br/>
9. zh HANS: απλοποιημένα κινεζικά<br/>
10. zh HANT: παραδοσιακά κινεζικά<br/>

## <a name="sample-csv-file"></a>Δείγμα αρχείου CSV
Ακολουθεί ένα δείγμα CSV που μπορείτε να τροποποιήσετε.

>[AZURE.NOTE] Αντιγράψτε και επικολλήστε αυτή στο Σημειωματάριο και αποθηκεύστε το αρχείο με επέκταση αρχείου '.csv'. Στη συνέχεια, επεξεργαστείτε αυτό στο Excel. Αυτό θα είναι δομημένα σε έναν πίνακα με ετικέτες στην πρώτη γραμμή.

> Προσθέστε περισσότερες προαιρετικά πεδία στο Excel, καθορίζοντας την ετικέτα και συμπληρώνετε τη στήλη κάτω από αυτήν.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Σχετικά άρθρα
Περιήγηση σε μας άλλα άρθρα Azure AD B2B συνεργασία

- [Τι είναι το Azure AD B2B συνεργασίας;](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Πώς λειτουργεί](active-directory-b2b-how-it-works.md)
- [Λεπτομερή ανάλυση](active-directory-b2b-detailed-walkthrough.md)
- [Μορφή διακριτικού εξωτερικών χρηστών](active-directory-b2b-references-external-user-token-format.md)
- [Αλλαγές χαρακτηριστικό αντικειμένου εξωτερικών χρηστών](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Τρέχουσα περιορισμοί preview](active-directory-b2b-current-preview-limitations.md)
- [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)
