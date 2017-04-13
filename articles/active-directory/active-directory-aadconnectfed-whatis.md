<properties
    pageTitle="Azure AD Connect και Ομοσπονδία | Microsoft Azure"
    description="Αυτή η σελίδα είναι η κεντρική θέση για όλα τα έγγραφα σχετικά με τις λειτουργίες AD FS χρησιμοποιώντας Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect και federation

Azure AD Connect σάς επιτρέπει να ρύθμιση παραμέτρων ομοσπονδίας με την εσωτερική AD FS και Azure AD. Με το σύμβολο ομοσπονδίας στο, μπορείτε να ενεργοποιήσετε χρήστες να πραγματοποιούν είσοδο υπηρεσίες Azure AD βάσει με τους κωδικούς πρόσβασης εσωτερικής εγκατάστασης και, κατά τη διάρκεια της του εταιρικού δικτύου, χωρίς να χρειάζεται να εισαγάγετε ξανά τους κωδικούς πρόσβασής τους. Η επιλογή Ομοσπονδία με AD FS σάς επιτρέπει να αναπτύξετε μια νέα ή να καθορίσετε μια υπάρχουσα AD FS στη συστοιχία Windows Server 2012 R2.

Αυτό το θέμα είναι το σπίτι για πληροφορίες σχετικά με την Ομοσπονδία που σχετίζονται με λειτουργίες για Azure AD Connect και λίστες συνδέσεις σε όλα τα άλλα θέματα που σχετίζονται με αυτό. Για συνδέσεις σε Azure AD Connect, ανατρέξτε στο θέμα ενοποίηση σας ταυτοτήτων εσωτερικής εγκατάστασης με το Azure Active Directory.

## <a name="azure-ad-connect---federation-topics"></a>Azure AD Connect - Ομοσπονδία θέματα

| Το θέμα | Τι να καλύπτει και πότε πρέπει να διαβάσετε |
|:------|:-----------|
| **Azure AD Connect εισόδου επιλογών χρήστη** ||
| [Κατανόηση των επιλογών εισόδου του χρήστη](active-directory-aadconnect-user-signin.md) | Περιγραφή των διαφόρων εισόδου επιλογών χρήστη και τον τρόπο που επηρεάζουν την εμπειρία χρήστη Azure εισόδου |
| **AD FS εγκατάστασης χρησιμοποιώντας Azure AD Connect**||
| [Προαπαιτούμενα](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | Προαπαιτούμενα για την επιτυχή εγκατάσταση AD FS μέσω Azure AD Connect|
| [Ρύθμιση παραμέτρων συστοιχίας AD FS](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | Πώς να εγκαταστήσετε μια νέα συστοιχία AD FS χρησιμοποιώντας Azure AD Connect |
| **Τροποποίηση της ρύθμισης παραμέτρων AD FS** | |
| [Η επιδιόρθωση της αξιοπιστίας](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Πώς μπορείτε να επιδιορθώσετε τρέχουσα σχέση αξιοπιστίας μεταξύ της εσωτερικής AD FS και το Office 365 / Azure |
| [Προσθήκη ενός νέου διακομιστή AD FS](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | Επέκταση AD FS συστοιχίας με επιπλέον AD FS διακομιστή δημοσίευση αρχική εγκατάσταση |
| [Προσθήκη ενός νέου διακομιστή AD FS WAP](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | Επέκταση AD FS συστοιχίας με επιπλέον WAP διακομιστή δημοσίευση αρχική εγκατάσταση |
| [Προσθέστε μια νέα ομόσπονδο τομέα](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Προσθήκη άλλου τομέα για να είναι ομόσπονδη σχέση Azure AD |
|**Δημοσίευση εργασίες εγκατάστασης**||
| [Προσθέστε το λογότυπο της εταιρείας προσαρμοσμένο / εικόνα](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| Τροποποιήστε την εμπειρία εισόδου, καθορίζοντας το προσαρμοσμένο λογότυπο που θα εμφανίζεται στη σελίδα εισόδου AD FS |
| [Προσθήκη περιγραφής εισόδου](active-directory-aadconnect-federation-management.md#add-sign-in-description) | Αλλαγή περιγραφής εισόδου στη σελίδα εισόδου AD FS | 
| [Η τροποποίηση AD FS διεκδίκηση κανόνων](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Τροποποίηση / προσθήκη κανόνων διεκδίκηση στο AD FS που αντιστοιχεί στη ρύθμιση παραμέτρων συγχρονισμού Azure AD Connect |


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Ενοποίηση του ταυτοτήτων εσωτερικής εγκατάστασης με το Azure Active Directory](active-directory-aadconnect.md)
* [Ανάπτυξη AD FS στο Azure](active-directory-aadconnect-azure-adfs.md)
* [Υψηλή διαθεσιμότητα μεταξύ γεωγραφικών AD FS ανάπτυξης στο Azure με τη Διαχείριση κίνηση Azure](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


