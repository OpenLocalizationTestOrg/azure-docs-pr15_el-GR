<properties 
    pageTitle="Πώς μπορείτε να εντοπίσετε σφάλματα βάσει SAML καθολικής σύνδεσης για τις εφαρμογές στο Azure Active Directory | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να εντοπίσετε σφάλματα βάσει SAML καθολικής σύνδεσης για τις εφαρμογές στο Azure Active Directory " 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Πώς μπορείτε να εντοπίσετε σφάλματα βάσει SAML καθολικής σύνδεσης για τις εφαρμογές στο Azure Active Directory

Κατά τον εντοπισμό σφαλμάτων σε μια εφαρμογή που βασίζεται σε SAML ενοποίησης, συχνά είναι χρήσιμο να χρησιμοποιήσετε ένα εργαλείο, όπως [Fiddler](http://www.telerik.com/fiddler) για να δείτε την αίτηση SAML, η απόκριση SAML και το πραγματικό διακριτικό SAML που εκδίδεται για την εφαρμογή. Εξετάζοντας το διακριτικό SAML, μπορείτε να διασφαλίσετε ότι όλα τα απαιτούμενα χαρακτηριστικά, το όνομα χρήστη στο θέμα SAML και τον εκδότη URI προέρχονται όπως αναμένεται.

![][1]

Η απόκριση από Azure AD που περιέχει το διακριτικό SAML συνήθως είναι αυτό που εμφανίζεται μετά την ανακατεύθυνση από https://login.windows.net ένα HTTP 302 και αποστέλλεται στο ρυθμισμένο **Απάντηση διεύθυνση URL** της εφαρμογής. 
 
Μπορείτε να προβάλετε το διακριτικό SAML, επιλέγοντας αυτήν τη γραμμή και, στη συνέχεια, επιλέγοντας το **επιθεωρήσεις > WebForms** καρτέλα στο δεξιό παράθυρο. Από εκεί, κάντε δεξί κλικ την τιμή **SAMLResponse** και επιλέξτε **Αποστολή για να TextWizard**. Στη συνέχεια, επιλέξτε **Από Base64** από το μενού **Μετασχηματισμός** να αποκωδικοποιείτε το διακριτικό και να δείτε τα περιεχόμενά του.
 
**Σημείωση**: για να δείτε τα περιεχόμενα αυτής της αίτησης HTTP, Fiddler μπορεί να σας ζητήσει να ρυθμίσετε τις παραμέτρους αποκρυπτογράφηση της κυκλοφορίας HTTPS, τα οποία θα πρέπει να κάνετε.

## <a name="related-articles"></a>Σχετικά άρθρα

- [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)
- [Ρύθμιση παραμέτρων Καθολικής σύνδεσης για τις εφαρμογές που δεν είναι στη συλλογή εφαρμογών Azure Active Directory](active-directory-saas-custom-apps.md)
- [Πώς μπορείτε να προσαρμόσετε αξιώσεων εκδοθεί στο διακριτικό SAML για προ-ενσωματωμένες εφαρμογές](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png
