<properties
   pageTitle="Λεπτομερή ανάλυση της χρήσης της προεπισκόπησης συνεργασίας Azure Active Directory B2B | Microsoft Azure"
   description="Συνεργασίας Azure Active Directory B2B υποστηρίζει τις σχέσεις μεταξύ εταιρεία σας, ενεργοποιώντας συνεργάτες επιλεκτικής πρόσβασης εταιρικών εφαρμογών σας"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Προεπισκόπηση συνεργασίας Azure AD B2B: λεπτομερή ανάλυση

Αυτή η αναλυτική παρουσίαση περιγράφει τον τρόπο χρήσης του Azure AD B2B συνεργασίας. Ως διαχειριστής IT της Contoso, θέλουμε να κάνετε κοινή χρήση εφαρμογών στους υπαλλήλους από τρεις εταιρείες συνεργάτη. Καμία από τις εταιρείες συνεργάτη πρέπει να έχετε Azure AD.

- Η Άννα από συνεργάτη απλό οργανόγραμμα
- Ο Μπάμπης, από το μέσο οργανισμού συνεργάτη, χρειάζονται πρόσβαση σε ένα σύνολο των εφαρμογών
- Carol, από σύνθετη οργανισμού συνεργάτη, χρειάζονται πρόσβαση σε ένα σύνολο εφαρμογές και ιδιότητα μέλους σε ομάδες στο Contoso

Αφού προσκλήσεων αποστέλλονται στους χρήστες του συνεργάτη, θα σας μπορεί να ρυθμίσετε τις παραμέτρους τους στο Azure AD για να εκχωρήσετε πρόσβαση σε εφαρμογές και ιδιότητα μέλους σε ομάδες μέσω της πύλης Azure. Ας ξεκινήσουμε με την προσθήκη Άννα.

## <a name="adding-alice-to-the-contoso-directory"></a>Προσθήκη η Άννα στον κατάλογο Contoso
1. Δημιουργήστε ένα αρχείο .csv με τις κεφαλίδες, όπως φαίνεται, συμπληρώνετε μόνο η Άννα του **ηλεκτρονικού ταχυδρομείου**, **εμφανιζόμενο όνομα**και **InviteContactUsUrl**. **Εμφανιζόμενο όνομα** είναι το όνομα που εμφανίζεται στην πρόσκληση, καθώς και το όνομα που εμφανίζεται στον κατάλογο Contoso Azure AD. **InviteContactUsUrl** είναι ένας τρόπος για την Άννα να επικοινωνήσετε με το Contoso. Στο παρακάτω παράδειγμα, InviteContactUsUrl Καθορίζει το προφίλ LinkedIn της Contoso. Είναι σημαντικό να τον ορθογραφικό τις ετικέτες στην πρώτη γραμμή του αρχείου .csv ακριβώς όπως καθορίζονται στην [αναφορά μορφή αρχείου CSV](active-directory-b2b-references-csv-file-format.md).  
![Παράδειγμα αρχείου CSV για η Άννα](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. Στην πύλη του Azure, Προσθήκη χρήστη στον κατάλογο Contoso (υπηρεσία καταλόγου Active Directory > Contoso > χρήστες > Προσθήκη χρήστη). Στο αναπτυσσόμενο μενού "Τύπος χρήστης" προς τα κάτω, επιλέξτε "Χρήστες σε εταιρείες συνεργάτη". Αποστείλετε το αρχείο .csv. Βεβαιωθείτε ότι το αρχείο .csv είναι κλειστό πριν από την αποστολή.  
![Αποστολή αρχείου CSV για η Άννα](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Η Άννα τώρα αποδίδεται με έναν εξωτερικό χρήστη στον κατάλογο Contoso Azure AD.  
![Εμφανίζεται η Άννα στο Azure AD](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Η Άννα λαμβάνει το ακόλουθο μήνυμα ηλεκτρονικού ταχυδρομείου.  
![Μήνυμα ηλεκτρονικού ταχυδρομείου πρόσκλησης για Άννα](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Η Άννα κάνει κλικ στη σύνδεση και είναι θα σας ζητηθεί για να αποδεχτείτε την πρόσκληση και να συνδεθείτε χρησιμοποιώντας τα διαπιστευτήρια της εργασίας. Εάν η Άννα δεν είναι του καταλόγου Azure AD, η Άννα ζητείται να εγγραφείτε.  
![Εγγραφείτε μετά την πρόσκληση για η Άννα](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Η Άννα θα ανακατευθύνεται σε εφαρμογή Access πίνακα, κενό μέχρι να κάνει έχει εκχωρηθεί πρόσβαση στις εφαρμογές.  
![Πίνακα της Access για η Άννα](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Αυτή η διαδικασία επιτρέπει απλούστερος μορφή B2B συνεργασίας. Ως χρήστης στον κατάλογο Contoso Azure AD, η Άννα μπορεί να σας εκχωρηθεί πρόσβαση σε εφαρμογές και ομάδες μέσω της πύλης Azure. Τώρα ας προσθέσουμε ο Μπάμπης, που χρειάζονται πρόσβαση στις εφαρμογές Moodle και Salesforce.

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Προσθήκη ο Μπάμπης στον κατάλογο Contoso και την εκχώρηση πρόσβασης σε εφαρμογές
1. Χρήση του Windows PowerShell με τη λειτουργική μονάδα Azure AD εγκατεστημένο για να βρείτε την εφαρμογή αναγνωριστικά Moodle και Salesforce. Τα αναγνωριστικά μπορεί να ανακτηθεί χρησιμοποιώντας το cmdlet: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` αυτόν τον τρόπο εμφανίζεται μια λίστα με όλες τις διαθέσιμες εφαρμογές στο Contoso και τους AppPrincialIds.  
![Ανάκτηση αναγνωριστικών για ο Μπάμπης](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Δημιουργήστε ένα αρχείο .csv που περιέχει ηλεκτρονικού ταχυδρομείου του Μπάμπη και εμφανιζόμενο όνομα, **InviteAppID**, **InviteAppResources**και InviteContactUsUrl. Συμπλήρωση **InviteAppResources** με το AppPrincipalIds Moodle και Salesforce βρέθηκε από το PowerShell, διαχωρισμένα με διάστημα. Συμπλήρωση **InviteAppId** με το ίδιο Moodle AppPrincipalId για εμπορική προσαρμογή του μηνύματος ηλεκτρονικού ταχυδρομείου και πραγματοποιήστε είσοδο στο σελίδες με ένα λογότυπο Moodle.  
![Παράδειγμα αρχείου CSV για ο Μπάμπης](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. Αποστείλετε το αρχείο .csv μέσω της πύλης Azure, όπως ακριβώς θα ήταν γίνεται για Άννα. Ο Μπάμπης είναι τώρα έναν εξωτερικό χρήστη στον κατάλογο Contoso Azure AD.

4. Ο Μπάμπης λαμβάνει το ακόλουθο μήνυμα ηλεκτρονικού ταχυδρομείου.  
![Μήνυμα ηλεκτρονικού ταχυδρομείου πρόσκλησης για ο Μπάμπης](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Ο Μπάμπης κάνει κλικ στη σύνδεση και ζητείται να αποδεχτεί την πρόσκληση. Αφού αυτός είναι συνδεδεμένος, ο ίδιος κατευθύνεται στο πλαίσιο πρόσβασης και να χρησιμοποιήσετε ήδη Moodle και Salesforce.  
![Πίνακα της Access για ο Μπάμπης](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

Θα προσθέσουμε Carol στη συνέχεια, που χρειάζεται πρόσβαση σε εφαρμογές καθώς και την ιδιότητα μέλους σε ομάδες στον κατάλογο Contoso.

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Προσθήκη Carol στον κατάλογο Contoso, την εκχώρηση πρόσβασης σε εφαρμογές και δίνει την ιδιότητα μέλους ομάδας

1. Χρήση του Windows PowerShell με τη λειτουργική μονάδα Azure AD εγκατεστημένο για να βρείτε την εφαρμογή αναγνωριστικά και τα αναγνωριστικά ομάδας μέσα σε Contoso.
 - Ανάκτηση AppPrincipalId χρησιμοποιώντας cmdlet `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`, με το ίδιο όπως για ο Μπάμπης
 - Ανάκτηση ObjectId για ομάδες χρησιμοποιώντας cmdlet `Get-MsolGroup | fl DisplayName, ObjectId`. Αυτόν τον τρόπο εμφανίζεται μια λίστα με όλες τις ομάδες στο Contoso και τους ObjectIds. Ομάδα αναγνωριστικά επίσης μπορεί να ανακτηθεί, όπως το Αναγνωριστικό αντικειμένου στην καρτέλα ιδιότητες της ομάδας στην πύλη του Azure.  
![Ανάκτηση αναγνωριστικά και τις ομάδες για Carol](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. Δημιουργία αρχείου .csv, συμπληρώνετε της Carol ηλεκτρονικού ταχυδρομείου εμφανιζόμενο όνομα, InviteAppID, InviteAppResources, **InviteGroupResources**και InviteContactUsUrl. **InviteGroupResources** συμπληρώνεται από το ObjectIds των ομάδων MyGroup1 και Externals, διαχωρισμένα με διάστημα.  
![Παράδειγμα αρχείου CSV για Carol](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Αποστείλετε το αρχείο .csv μέσω της πύλης Azure.

4. Carol είναι ένας χρήστης στον κατάλογο Contoso και είναι επίσης ένα μέλος των ομάδων MyGroup1 και Externals, όπως φαίνεται στην πύλη του Azure.  
![Carol εμφανίζεται σε μια ομάδα στο Azure AD](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Carol λαμβάνει ένα μήνυμα ηλεκτρονικού ταχυδρομείου που περιέχει μια σύνδεση για να αποδεχτείτε την πρόσκληση. Αφού χρήστης πραγματοποιεί είσοδο, θα ανακατευθύνεται στο πλαίσιο εφαρμογή πρόσβασης για να έχετε πρόσβαση στο Moodle και Salesforce.  

Που είναι όλα υπάρχει με την προσθήκη χρηστών από το συνεργάτη επιχειρήσεις σε συνεργασία με το Azure AD B2B. Πώς μπορείτε να προσθέσετε χρήστες, η Άννα, ο Μπάμπης και Carol στον κατάλογο Contoso χρησιμοποιώντας τρία ξεχωριστά αρχεία .csv εμφάνιζε αυτόν τον οδηγό. Αυτή η διαδικασία μπορούν να γίνουν πιο εύκολη από συμπύκνωση το ξεχωριστά αρχεία .csv σε ένα αρχείο.  
![Παράδειγμα αρχείου CSV για Άννα, ο Μπάμπης και Carol](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>Σχετικά άρθρα
Αναζήτηση μας άλλα άρθρα στο Azure AD B2B συνεργασίας:

- [Τι είναι το Azure AD B2B συνεργασίας;](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Πώς λειτουργεί](active-directory-b2b-how-it-works.md)
- [Αναφορά μορφή αρχείου CSV](active-directory-b2b-references-csv-file-format.md)
- [Μορφή διακριτικού εξωτερικών χρηστών](active-directory-b2b-references-external-user-token-format.md)
- [Αλλαγές χαρακτηριστικό αντικειμένου εξωτερικών χρηστών](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Τρέχουσα περιορισμοί preview](active-directory-b2b-current-preview-limitations.md)
- [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)
