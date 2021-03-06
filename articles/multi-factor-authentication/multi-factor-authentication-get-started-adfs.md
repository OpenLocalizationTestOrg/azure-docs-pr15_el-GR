<properties
    pageTitle="Azure MFA και AD FS | Microsoft Azure"
    description="Αυτή είναι η σελίδα ελέγχου ταυτότητας πολλαπλών παραγόντων Azure που περιγράφει πώς μπορείτε να ξεκινήσετε με το Azure MFA και AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na" ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Γρήγορα αποτελέσματα με έλεγχο ταυτότητας πολλών παραγόντων Azure και υπηρεσίες Active Directory Federation Services



<center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Εάν η εταιρεία σας έχει ομόσπονδες σας στην εσωτερική εγκατάσταση υπηρεσίας καταλόγου Active Directory με Azure Active Directory με χρήση AD FS, υπάρχουν δύο επιλογές για τη χρήση του ελέγχου ταυτότητας πολλαπλών παραγόντων Azure.

- Εξασφάλιση cloud πόρων με τη χρήση ελέγχου ταυτότητας πολλαπλών παραγόντων Azure ή υπηρεσίες Active Directory Federation Services
- Εξασφάλιση πόρων cloud και εσωτερικής εγκατάστασης με χρήση διακομιστή ελέγχου ταυτότητας πολλαπλών παραγόντων Azure

Ο παρακάτω πίνακας συνοψίζει την εμπειρία επαλήθευσης μεταξύ ασφάλεια των πόρων με έλεγχο ταυτότητας πολλών παραγόντων Azure και AD FS

|Εμπειρία επαλήθευσης - εφαρμογές που βασίζονται σε αναζήτηση|Εμπειρία επαλήθευσης - εφαρμογές που δεν βασίζονται σε πρόγραμμα περιήγησης
:------------- | :------------- | :------------- |
Ασφάλεια των πόρων Azure AD με χρήση ελέγχου ταυτότητας πολλαπλών παραγόντων Azure |<li>Το πρώτο βήμα επαλήθευσης εκτελείται εσωτερικής εγκατάστασης με χρήση AD FS.</li> <li>Το δεύτερο βήμα είναι μια μέθοδο μέσω τηλεφώνου πραγματοποιείται με χρήση ελέγχου ταυτότητας cloud.</li>|Στους τελικούς χρήστες να χρησιμοποιήσετε τους κωδικούς πρόσβασης εφαρμογή εισόδου.
Ασφάλεια των Azure AD πόρους χρησιμοποιώντας υπηρεσίες Active Directory Federation Services |<li>Το πρώτο βήμα επαλήθευσης εκτελείται εσωτερικής εγκατάστασης με χρήση AD FS.</li><li>Το δεύτερο βήμα είναι να έχει πραγματοποιηθεί στην εσωτερική εγκατάσταση honoring την απαίτηση.</li>|Στους τελικούς χρήστες να χρησιμοποιήσετε τους κωδικούς πρόσβασης εφαρμογή εισόδου.

Προειδοποιήσεις με εφαρμογή τους κωδικούς πρόσβασης για εξωτερικούς χρήστες:

- Εφαρμογή κωδικών πρόσβασης έχουν επαληθευτεί χρησιμοποιώντας τον έλεγχο ταυτότητας cloud, ώστε αυτές παρακάμπτουν ομοσπονδίας. Ομοσπονδία χρησιμοποιείται μόνο ενεργά κατά τον ορισμό ενός κωδικού πρόσβασης εφαρμογής.
- Δεν είναι η πραγματοποίηση ρυθμίσεις προγράμματος-πελάτη έλεγχος πρόσβασης εσωτερικής εγκατάστασης με εφαρμογή κωδικών πρόσβασης.
- Θα χάσετε εσωτερικής δυνατότητα καταγραφής ελέγχου ταυτότητας για την εφαρμογή τους κωδικούς πρόσβασης.
- Απενεργοποίηση/διαγραφή του λογαριασμού ενδέχεται να χρειαστούν έως και τρεις ώρες για συγχρονισμό καταλόγου, να καθυστερήσει απενεργοποίηση/διαγραφή της εφαρμογής τους κωδικούς πρόσβασης στην ταυτότητα cloud.

## <a name="next-steps"></a>Επόμενα βήματα

Για πληροφορίες σχετικά με τη ρύθμιση του ελέγχου ταυτότητας πολλαπλών παραγόντων Azure ή ο διακομιστής ελέγχου ταυτότητας πολλαπλών παραγόντων Azure με AD FS, ανατρέξτε στα ακόλουθα άρθρα:

- [Εξασφάλιση πόρων cloud με χρήση του ελέγχου ταυτότητας πολλαπλών παραγόντων Azure και AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
- [Εξασφάλιση πόρων cloud και εσωτερικής εγκατάστασης με χρήση διακομιστή ελέγχου ταυτότητας πολλαπλών παραγόντων Azure με Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Εξασφάλιση πόρων cloud και εσωτερικής εγκατάστασης με χρήση διακομιστή ελέγχου ταυτότητας πολλαπλών παραγόντων Azure με AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)
