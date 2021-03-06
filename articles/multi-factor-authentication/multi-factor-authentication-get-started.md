<properties
    pageTitle="Azure διακομιστή και στο cloud MFA | Microsoft Azure"
    description="Επιλέξτε τη λύση secutiry έλεγχο ταυτότητας πολλών παραγόντων που βρίσκεται δεξιά για εσάς, που σας ρωτά τι ΠΜ i προσπαθεί να ασφαλούς και πού βρίσκονται οι χρήστες μου βρίσκεται.  Στη συνέχεια, επιλέξτε cloud, MFA διακομιστή ή AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Επιλέξτε τη λύση έλεγχο ταυτότητας πολλών παραγόντων Azure για εσάς

Επειδή υπάρχουν αρκετές μπορεί να είναι του ελέγχου ταυτότητας πολλαπλών παραγόντων Azure (MFA), θα σας πρέπει να απαντήσετε σε μερικές ερωτήσεις για να υπολογίσετε ποια έκδοση είναι το πρώτο γράμμα κάθε λέξης για να χρησιμοποιήσετε.  Θα βρείτε αυτές τις ερωτήσεις:

-   [Τι να προσπαθεί να ασφαλούς](#what-am-i-trying-to-secure)
-   [Πού βρίσκονται οι χρήστες](#where-are-the-users-located)
- [Ποιες δυνατότητες χρειάζομαι;](#what-featured-do-i-need)

Οι παρακάτω ενότητες παρέχουν οδηγίες σχετικά Καθορισμός κάθε μία από αυτές τις απαντήσεις.

## <a name="what-am-i-trying-to-secure"></a>Τι να προσπαθεί να ασφαλούς;

Για να προσδιορίσετε τη λύση επαλήθευσης σωστή δύο βημάτων, πρώτα θα σας πρέπει να απάντηση στην ερώτηση του τι που προσπαθείτε να ασφαλούς με μια δεύτερη μέθοδο ελέγχου ταυτότητας.  Πρόκειται για μια εφαρμογή που βρίσκεται στο Azure;  Ή ένα σύστημα απομακρυσμένη πρόσβαση;  Κατά τον καθορισμό αυτό θα σας προσπαθείτε να ασφαλούς, θα σας να απαντήσετε στην ερώτηση του πού πρέπει να ενεργοποιηθεί έλεγχο ταυτότητας πολλών παραγόντων.  


Τι προσπαθείτε να ασφαλίσετε| Έλεγχος ταυτότητας πολλών παραγόντων στο cloud|Διακομιστής ελέγχου ταυτότητας πολλαπλών παραγόντων
------------- | :-------------: | :-------------: |
Εφαρμογές του Microsoft ιδίου κατασκευαστή|● |● |
Εφαρμογές ΑΔΑ στη συλλογή εφαρμογών|● |● |
Εφαρμογές των υπηρεσιών IIS που δημοσιεύονται μέσω διακομιστή μεσολάβησης εφαρμογής AD Azure|● |● |
Εφαρμογές των υπηρεσιών IIS που δεν δημοσιεύονται μέσω διακομιστή μεσολάβησης εφαρμογής AD Azure | |● |
Απομακρυσμένη πρόσβαση όπως VPN, RDG| |● |



## <a name="where-are-the-users-located"></a>Πού βρίσκονται οι χρήστες

Στη συνέχεια, εξετάζοντας όπου βρίσκονται οι χρήστες μας σάς βοηθά να προσδιορίσετε τη σωστή λύση για να χρησιμοποιήσετε, είτε στο cloud ή εσωτερικής εγκατάστασης χρησιμοποιώντας το διακομιστή MFA.



Θέση χρήστη| Έλεγχος ταυτότητας πολλών παραγόντων στο cloud|Διακομιστής ελέγχου ταυτότητας πολλαπλών παραγόντων
------------- | :-------------: | :-------------: |
Καταλόγου Azure Active Directory|● | |
Azure AD και εσωτερικής εγκατάστασης με χρήση Ομοσπονδία με AD FS AD|● |● |
Azure AD και εσωτερικής AD χρησιμοποιώντας DirSync, Azure AD Sync, Azure AD Connect - χωρίς συγχρονισμό κωδικού πρόσβασης|● |● |
Azure AD και εσωτερικής AD χρήση DirSync, Azure AD Sync, Azure AD Connect - με το συγχρονισμό κωδικού πρόσβασης|● | |
Υπηρεσία καταλόγου Active Directory εσωτερικής εγκατάστασης| |● |

## <a name="what-features-do-i-need"></a>Ποιες δυνατότητες χρειάζομαι;

Ο παρακάτω πίνακας είναι μια σύγκριση των δυνατοτήτων που είναι διαθέσιμες με έλεγχο ταυτότητας πολλών παραγόντων στο cloud και ο διακομιστής ελέγχου ταυτότητας πολλαπλών παραγόντων.

 | Έλεγχος ταυτότητας πολλών παραγόντων στο cloud | Διακομιστής ελέγχου ταυτότητας πολλαπλών παραγόντων
------------- | :-------------: | :-------------: |
Ειδοποίηση εφαρμογής για κινητές συσκευές ως δεύτερο παράγοντα | ● | ● |
Εφαρμογή για κινητές συσκευές κωδικό επαλήθευσης ως δεύτερο παράγοντα | ● | ●
Τηλεφωνική κλήση ως δεύτερο παραγόντων | ● | ●
Μονόπλευρη SMS ως δεύτερο παραγόντων | ● | ●
Αμφίδρομη SMS ως δεύτερο παραγόντων |  | ●
Διακριτικά υλικού ως δεύτερο παραγόντων |  | ●
Εφαρμογή τους κωδικούς πρόσβασης για τους πελάτες που δεν υποστηρίζουν MFA | ● |  
Διαχείριση έλεγχο μεθόδους ελέγχου ταυτότητας | ● | ●
Λειτουργία PIN |  | ●
Ειδοποίηση απάτης | ● | ●
Αναφορές MFA | ● | ●
Μεμονωμένη παράκαμψης |  | ●
Προσαρμοσμένη Χαιρετισμοί για τηλεφωνικές κλήσεις | ● | ●
Αναγνωριστικό καλούντος προσαρμόσιμων για τηλεφωνικές κλήσεις | ● | ●
Αξιόπιστη διευθύνσεις IP | ● | ●
Να θυμάστε MFA για συσκευές αξιόπιστη  | ● |  
Πρόσβαση υπό όρους | ● | ●
Cache |  | ●

Τώρα που θα σας έχετε προσδιορίσει εάν θα χρησιμοποιήσετε έλεγχο ταυτότητας πολλών παραγόντων cloud ή το MFA διακομιστή εσωτερικής εγκατάστασης, θα σας μπορούν να ξεκινήσουν ρύθμιση και χρήση του ελέγχου ταυτότητας πολλαπλών παραγόντων Azure. **Επιλέξτε το εικονίδιο που αντιπροσωπεύει το σενάριό σας!**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
