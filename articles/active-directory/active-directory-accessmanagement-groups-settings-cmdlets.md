<properties
    pageTitle="Azure Active Directory cmdlet για τη ρύθμιση των παραμέτρων ρυθμίσεων ομάδας | Microsoft Azure"
    description="Πώς να διαχειριστείτε τις ρυθμίσεις για χρήση των cmdlet Azure Active Directory τις ομάδες."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory cmdlet για τη ρύθμιση των παραμέτρων ρυθμίσεων ομάδας

Τις ακόλουθες ρυθμίσεις για ενοποιημένες ομάδες μπορεί να ρυθμιστεί στον κατάλογό σας:

1.  Ταξινομήσεις: η διαχωρισμένες με κόμματα λίστα ταξινομήσεις που να ρυθμίσετε τους χρήστες σε μια ομάδα. Παραδείγματα θα ήταν "Classified", "Μυστικό" και "Επάνω μυστικό."

2.  Χρήση οδηγίες URL: μια διεύθυνση URL που οδηγεί χρηστών για τους όρους χρήσης για τη χρήση ενοποιημένης ομάδες, όπως καθορίζεται από την εταιρεία σας. Αυτή η διεύθυνση URL θα εμφανίζονται στο περιβάλλον εργασίας χρήστη του όπου οι χρήστες χρησιμοποιούν το ομάδες.

3.  Ομαδοποίηση με δυνατότητα δημιουργίας: Εάν καμία, επιτρέπονται ορισμένων ή όλων των χρηστών για να δημιουργήσετε ομάδες ενοποιημένο σύστημα. Όταν ορίσετε, όλοι οι χρήστες να δημιουργήσετε ομάδες. Εάν οριστεί σε απενεργοποιημένη, χωρίς οι χρήστες να δημιουργήσετε ομάδες. Όταν απενεργοποιήσετε, μπορείτε επίσης να καθορίσετε μια ομάδα ασφαλείας των οποίων οι χρήστες που επιτρέπονται για να δημιουργήσετε ομάδες.

Αυτές οι παράμετροι ρυθμίζονται χρησιμοποιώντας ένα τις ρυθμίσεις και τα αντικείμενα SettingsTemplate. Αρχικά, δεν θα βλέπετε τυχόν ρυθμίσεις αντικείμενα στον κατάλογό σας. Αυτό σημαίνει ότι οι παράμετροι του καταλόγου σας έχουν ρυθμιστεί με τις προεπιλεγμένες ρυθμίσεις. Για να αλλάξετε τις προεπιλεγμένες ρυθμίσεις, θα δημιουργήσετε ένα νέο αντικείμενο ρυθμίσεων χρησιμοποιώντας ένα πρότυπο ρυθμίσεις. Ρυθμίσεις πρότυπα ορίζονται από τη Microsoft.

Μπορείτε να κάνετε λήψη της λειτουργικής μονάδας που περιέχει τα cmdlet που χρησιμοποιούνται για αυτές τις λειτουργίες από την [τοποθεσία Microsoft σύνδεση](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).

## <a name="create-settings-at-the-directory-level"></a>Δημιουργία ρυθμίσεων στο επίπεδο καταλόγου

Αυτά τα βήματα δημιουργήσετε ρυθμίσεις στο επίπεδο καταλόγου, που εφαρμόζονται σε όλες τις ομάδες Office στον κατάλογο.

1. Εάν δεν γνωρίζετε ποια SettingTemplate για να χρησιμοποιήσετε, αυτό το cmdlet επιστρέφει τη λίστα των προτύπων ρυθμίσεις:

    `Get-MsolAllSettingTemplate`

    ![Λίστα ρυθμίσεων προτύπων](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. Για να προσθέσετε μια διεύθυνση URL χρήση κατευθυντήριας γραμμής, πρέπει πρώτα να λάβετε το αντικείμενο SettingsTemplate που καθορίζει την τιμή διεύθυνσης URL χρήση κατευθυντήριας γραμμής. Αυτό σημαίνει ότι το πρότυπο Group.Unified:

    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. Στη συνέχεια, δημιουργήστε ένα νέο αντικείμενο ρυθμίσεων που βασίζεται σε αυτό το πρότυπο:

    `$setting = $template.CreateSettingsObject()`

4. Στη συνέχεια, ενημερώστε την τιμή κατευθυντήριας γραμμής χρήση:

    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. Τέλος, να εφαρμόσετε τις ρυθμίσεις:

    `New-MsolSettings –SettingsObject $setting`

    ![Προσθήκη μιας διεύθυνσης URL χρήση κατευθυντήριες γραμμές](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Εδώ θα βρείτε τις ρυθμίσεις που ορίζονται στο το Group.Unified SettingsTemplate.

 **Ρύθμιση**                          | **Περιγραφή**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>Τύπος: συμβολοσειρά<li>Προεπιλογή: ""                  | Μια λίστα κόμματα έγκυρη ταξινόμηση τιμών που μπορούν να εφαρμοστούν σε ομάδες ενοποιημένο σύστημα.                
 <ul><li>EnableGroupCreation<li>Τύπος: δυαδική<li>Προεπιλεγμένη: True              | Η σημαία που υποδεικνύει εάν επιτρέπεται η δημιουργία της ομάδας ενοποιημένης στον κατάλογο.                               
 <ul><li>GroupCreationAllowedGroupId<li>Τύπος: συμβολοσειρά<li>Προεπιλογή: ""         | GUID της ομάδας ασφαλείας που επιτρέπεται για να δημιουργήσετε ομάδες ενοποιημένης ακόμα και όταν EnableGroupCreation == false.
 <ul><li>UsageGuidelinesUrl<li>Τύπος: συμβολοσειρά<li>Προεπιλογή: ""                  | Μια σύνδεση προς τις οδηγίες χρήση ομάδας.                                                                       

## <a name="read-settings-at-the-directory-level"></a>Ανάγνωση των ρυθμίσεων στο επίπεδο καταλόγου

Αυτά τα βήματα Διαβάστε ρυθμίσεις στο επίπεδο καταλόγου, που εφαρμόζονται σε όλες τις ομάδες Office στον κατάλογο.

1. Διαβάστε όλες τις υπάρχουσες ρυθμίσεις καταλόγου:

    `Get-MsolAllSettings`

2. Διαβάστε όλες τις ρυθμίσεις για μια συγκεκριμένη ομάδα:

    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. Διαβάστε ρυθμίσεις συγκεκριμένο κατάλογο, χρησιμοποιώντας το SettingId GUID:

    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

    ![Ρυθμίσεις Αναγνωριστικό GUID](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>Ενημέρωση ρυθμίσεων στο επίπεδο καταλόγου

Αυτά τα βήματα, ενημερώστε τις ρυθμίσεις στο επίπεδο καταλόγου, που ισχύουν για όλες τις ομάδες Office στον κατάλογο.

1. Λήψη του αντικειμένου υπάρχουσες ρυθμίσεις:

    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. Λάβετε την τιμή που θέλετε να ενημερώσετε:

    `$value = $Setting.GetSettingsValue()`

3. Ενημερώστε την τιμή:

    `$value["AllowToAddGuests"] = "false"`

4. Ενημερώστε τη ρύθμιση:

    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>Κατάργηση ρυθμίσεων στο επίπεδο καταλόγου

Αυτό το βήμα καταργεί ρυθμίσεις στο επίπεδο καταλόγου, που εφαρμόζονται σε όλες τις ομάδες Office στον κατάλογο.

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Αναφορά σύνταξης cmdlet

Μπορείτε να βρείτε περισσότερες τεκμηρίωση Azure Active Directory PowerShell στο [Cmdlet του Azure Active Directory](http://go.microsoft.com/fwlink/p/?LinkId=808260).

## <a name="settingstemplate-object-reference-groupunified-settingstemplate-object"></a>Αναφορά σε αντικείμενο SettingsTemplate (Group.Unified SettingsTemplate αντικείμενο)

- "όνομα": "EnableGroupCreation", "Τύπος": "System.Boolean", "προεπιλεγμένη τιμή": "true", "Περιγραφή": "Μια σημαία δυαδικής τιμής που υποδεικνύει εάν η δυνατότητα δημιουργίας ομάδας ενοποιημένης είναι στην."

- "όνομα": "GroupCreationAllowedGroupId", "Τύπος": "System.Guid", "προεπιλεγμένη τιμή": "", "Περιγραφή": "GUID της ομάδας ασφαλείας που είναι whitelisted για να δημιουργήσετε ομάδες ενοποιημένης."

- "όνομα": "ClassificationList", "Τύπος": "System.String", "προεπιλεγμένη τιμή": "", "Περιγραφή": "Μια διαχωρισμένο με κόμματα λίστα έγκυρη ταξινόμηση των τιμών που μπορούν να εφαρμοστούν σε ομάδες ενοποιημένης."

- "όνομα": "UsageGuidelinesUrl", "Τύπος": "System.String", "προεπιλεγμένη τιμή": "", "Περιγραφή": "Μια σύνδεση για τις οδηγίες χρήση ομάδα."

Όνομα | Τύπος | προεπιλεγμένη τιμή | Περιγραφή
----------  | ----------  | ---------  | ----------
"EnableGroupCreation"  | "System.Boolean"  | "true"  | "Μια σημαία δυαδικής τιμής που υποδεικνύει εάν η δυνατότητα δημιουργίας ομάδας ενοποιημένης βρίσκεται σε."
"GroupCreationAllowedGroupId"  | "System.Guid"  | ""  | "GUID της ομάδας ασφαλείας που είναι whitelisted για να δημιουργήσετε ομάδες ενοποιημένης."
"ClassificationList"  | "System.String"  | ""  | "Ένα διαχωρισμένο με κόμματα λίστα έγκυρη ταξινόμηση των τιμών που μπορούν να εφαρμοστούν σε ομάδες ενοποιημένης."
"UsageGuidelinesUrl"  | "System.String"  | ""  | "Μια σύνδεση για τις οδηγίες χρήση ομάδα."

## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να βρείτε περισσότερες τεκμηρίωση Azure Active Directory PowerShell στο [Cmdlet του Azure Active Directory](http://go.microsoft.com/fwlink/p/?LinkId=808260).

Πρόσθετες οδηγίες από το διευθυντή πρόγραμμα Microsoft Jong de Ιωάννης είναι διαθέσιμη στο [Ιστολόγιο του Ιωάννης ομάδες](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad).

* [Διαχείριση της πρόσβασης σε πόρους με ομάδες Azure Active Directory](active-directory-manage-groups.md)

* [Ενοποίηση του ταυτοτήτων εσωτερικής εγκατάστασης με το Azure Active Directory](active-directory-aadconnect.md)
