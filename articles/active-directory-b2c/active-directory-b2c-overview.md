<properties
    pageTitle="B2C καταλόγου Azure Active Directory: Επισκόπηση | Microsoft Azure"
    description="Ανάπτυξη εφαρμογών καταναλωτή τοποθεσία με Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Εγγραφείτε Azure Active Directory B2C: Και πραγματοποιήστε είσοδο καταναλωτές στις εφαρμογές σας

Azure Active Directory B2C είναι μια λύση διαχείρισης ταυτοτήτων ολοκληρωμένη cloud για καταναλωτές-τοποθεσία web και εφαρμογές για κινητές συσκευές. Είναι ιδιαίτερα διαθέσιμη καθολικού υπηρεσίας που προσαρμόζεται εκατοντάδες εκατομμύρια καταναλωτή ταυτότητες. Ενσωματωμένη σε μια ασφαλή πλατφόρμα επίπεδου μεγάλης εταιρείας, Azure Active Directory B2C διατηρεί τις εφαρμογές σας, την επιχείρησή σας και οι χρήστες του προστατευμένο.

Στο παρελθόν, οι προγραμματιστές εφαρμογής που θέλετε να εγγραφείτε και πραγματοποιήστε είσοδο καταναλωτές σε εφαρμογές τους θα έχουν εγγραφεί μόνος του κώδικα. Και αυτές θα έχετε χρησιμοποιήσει βάσεις δεδομένων εσωτερικής εγκατάστασης ή συστήματα για να αποθηκεύσετε τα ονόματα των χρηστών και κωδικούς πρόσβασης. Azure Active Directory B2C προσφέρει προγραμματιστές έναν καλύτερο τρόπο για να ενσωματώσετε διαχείρισης των ταυτοτήτων καταναλωτή σε εφαρμογές τους με τη Βοήθεια μια πλατφόρμα ασφαλή, βάσει προτύπων και ένα εμπλουτισμένο σύνολο επεκτάσιμη πολιτικών. Όταν χρησιμοποιείτε Azure Active Directory B2C, οι χρήστες του να εγγραφείτε για τις εφαρμογές σας, χρησιμοποιώντας τους υπαρχόντων λογαριασμών κοινωνικών (Facebook, Google, Amazon, LinkedIn) ή με τη δημιουργία νέας διαπιστευτήρια (διεύθυνση ηλεκτρονικού ταχυδρομείου και τον κωδικό πρόσβασης, ή όνομα χρήστη και κωδικός πρόσβασης) ονομάζουμε την τελευταία "Τοπικοί λογαριασμοί."

## <a name="get-started"></a>Γρήγορα αποτελέσματα

Για να δημιουργήσετε μια εφαρμογή που δέχεται καταναλωτή εγγραφή και την είσοδο, που θα μισθωτή πρώτη πρέπει να καταχωρήσετε την εφαρμογή με ένα B2C Azure Active Directory. Λάβετε τη δική σας μισθωτή, χρησιμοποιώντας τα βήματα που περιγράφονται στο θέμα [Δημιουργία ένα μισθωτή του Azure AD B2C](active-directory-b2c-get-started.md).

Μπορείτε να συντάξετε την εφαρμογή του σε σχέση με την υπηρεσία Azure Active Directory B2C, επιλέγοντας για την αποστολή μηνυμάτων πρωτόκολλο απευθείας, χρησιμοποιώντας το [διακριτικό 2.0](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow) ή [Άνοιγμα Αναγνωριστικό σύνδεσης](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow), είτε με τη χρήση βιβλιοθηκών μας για την εργασία για εσάς. Επιλέξτε την πλατφόρμα αγαπημένων σας στον παρακάτω πίνακα και γρήγορα αποτελέσματα.

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Τι νέο υπάρχει

Ελέγξτε ξανά εδώ συχνά για να μάθετε περισσότερα σχετικά με μελλοντικές αλλαγές για να το B2C Azure Active Directory. Θα σας θα tweet επίσης σχετικά με τις ενημερώσεις, χρησιμοποιώντας @AzureAD.

- Μάθετε σχετικά με την [υποδομή πολιτικής επεκτάσιμη](active-directory-b2c-reference-policies.md) και σχετικά με τους τύπους των πολιτικών που μπορείτε να δημιουργήσετε και να χρησιμοποιήσετε στις εφαρμογές σας.
- Σελιδοδείκτης το [ιστολογίου υπηρεσίας](https://blogs.msdn.microsoft.com/azureadb2c/) για τις ειδοποιήσεις σχετικά με ζητήματα υπηρεσίας μικρή, ενημερώσεις, κατάσταση και μετριασμούς. Για την παρακολούθηση της [κατάστασης Azure πίνακα εργαλείων](https://azure.microsoft.com/status/) καθώς και συνεχίσετε.
- Τρέχουσα [περιορισμούς υπηρεσίας, περιορισμούς, και τους περιορισμούς](active-directory-b2c-limitations.md).
- Τέλος, ένα [δείγμα κώδικα](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) με Azure AD B2C & πυρήνα ASP.NET.

## <a name="how-to-articles"></a>Άρθρα με οδηγίες

Μάθετε πώς να χρησιμοποιείτε συγκεκριμένες δυνατότητες Azure Active Directory B2C:

- Ρύθμιση παραμέτρων λογαριασμών [Facebook](active-directory-b2c-setup-fb-app.md), [Google +](active-directory-b2c-setup-goog-app.md), [λογαριασμό Microsoft που διαθέτετε](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md)και [LinkedIn](active-directory-b2c-setup-li-app.md) για χρήση στις εφαρμογές σας τοποθεσία καταναλωτών.
- [Χρήση προσαρμοσμένα χαρακτηριστικά για τη συλλογή πληροφοριών σχετικά με το καταναλωτών](active-directory-b2c-reference-custom-attr.md).
- [Ενεργοποίηση Azure έλεγχο ταυτότητας πολλών παραγόντων στις εφαρμογές σας τοποθεσία καταναλωτών](active-directory-b2c-reference-mfa.md).
- [Ρύθμιση του από το χρήστη επαναφορά κωδικού πρόσβασης για καταναλωτές σας](active-directory-b2c-reference-sspr.md).
- [Προσαρμογή της εμφάνισης και αίσθησης της εγγραφής, είσοδος, και άλλες σελίδες καταναλωτή τοποθεσία](active-directory-b2c-reference-ui-customization.md) που είναι λειτουργούσαν από Azure Active Directory B2C.
- [Χρήση του API Graph Active Directory Azure μέσω προγραμματισμού δημιουργία, ανάγνωση, ενημέρωση, και διαγραφή τους καταναλωτές](active-directory-b2c-devquickstarts-graph-dotnet.md) στο μισθωτή του Azure Active Directory B2C.

## <a name="next-steps"></a>Επόμενα βήματα

Αυτές οι συνδέσεις θα είναι χρήσιμες για την Εξερεύνηση της υπηρεσίας στον άξονα βάθους:

- Δείτε τις [πληροφορίες τιμολόγησης Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
- Λήψη Βοήθειας σε υπερχείλιση στοίβας με τη χρήση του [καταλόγου azure active](http://stackoverflow.com/questions/tagged/azure-active-directory) ή [adal](http://stackoverflow.com/questions/tagged/adal) ετικέτες.
- Στείλτε μας τις σκέψεις σας με τη χρήση [Φωνής χρήστη](https://feedback.azure.com/forums/169401-azure-active-directory/)--θέλουμε να ακούσετε τους! Χρησιμοποιήστε τη φράση "AzureADB2C:" στον τίτλο της δημοσίευσή σας, ώστε να μπορούμε να τα βρούμε.
- Ελέγξτε την [αναφορά πρωτόκολλο Azure AD B2C](active-directory-b2c-reference-protocols.md).
- Ελέγξτε την [αναφορά διακριτικού Azure AD B2C](active-directory-b2c-reference-tokens.md).
- Διαβάστε το [Συνήθεις ερωτήσεις B2C καταλόγου Azure Active Directory](active-directory-b2c-faqs.md).
- [Αρχείο υποστηρίζουν αιτήσεις για Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Λήψη ενημερώσεων ασφαλείας για τα προϊόντα μας

Συνιστάται να λαμβάνετε ειδοποιήσεις όταν προκύπτουν περιστατικά ασφαλείας, αν επισκεφθείτε την τοποθεσία [αυτήν τη σελίδα](https://technet.microsoft.com/security/dd252948) και εγγραφή συμβουλευτική ειδοποιήσεων ασφαλείας.
