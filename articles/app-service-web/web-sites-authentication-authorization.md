<properties 
    pageTitle="Ο έλεγχος ταυτότητας με καταλόγου Active Directory εσωτερικής εγκατάστασης στην εφαρμογή Azure | Microsoft Azure" 
    description="Μάθετε περισσότερα σχετικά με τις διαφορετικές επιλογές για τις εφαρμογές γραμμής εταιρικά στο Azure εφαρμογής υπηρεσίας για τον έλεγχο ταυτότητας με την υπηρεσία καταλόγου Active Directory εσωτερικής εγκατάστασης" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Ο έλεγχος ταυτότητας με καταλόγου Active Directory εσωτερικής εγκατάστασης στην εφαρμογή Azure #

Αυτό το άρθρο παρουσιάζει τον τρόπο για τον έλεγχο ταυτότητας με εσωτερικής υπηρεσίας καταλόγου Active Directory (AD) στο [Azure εφαρμογής υπηρεσίας](../app-service/app-service-value-prop-what-is.md). Azure εφαρμογής φιλοξενείται στο cloud, αλλά υπάρχουν τρόπους για τον έλεγχο ταυτότητας εσωτερικής AD χρήστες με ασφάλεια. 

## <a name="authenticate-through-azure-active-directory"></a>Τον έλεγχο ταυτότητας μέσω του Azure Active Directory
Ένα μισθωτή του Azure Active Directory μπορεί να είναι καταλόγου συγχρονιστεί με μια εσωτερική AD. Αυτή η προσέγγιση επιτρέπει στους χρήστες AD την πρόσβαση της εφαρμογής σας από το internet και τον έλεγχο ταυτότητας χρησιμοποιώντας τα διαπιστευτήριά τους εσωτερικής εγκατάστασης. Επιπλέον, Azure εφαρμογής υπηρεσίας παρέχει μια [λύση Ενεργοποίηση πλήκτρων για αυτήν τη μέθοδο](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Με λίγα μόνο κλικ ενός κουμπιού, μπορείτε να ενεργοποιήσετε τον έλεγχο ταυτότητας με μισθωτή συγχρονίσει καταλόγου για την εφαρμογή του Azure. Αυτή η προσέγγιση έχει τα εξής πλεονεκτήματα:

-   Δεν απαιτεί οποιονδήποτε κωδικό ελέγχου ταυτότητας στην εφαρμογή. Ενημερώστε εφαρμογής υπηρεσίας κάνετε ο έλεγχος ταυτότητας για εσάς και να ξοδέψετε χρόνο σας στην παροχή λειτουργιών στην εφαρμογή.
-   [API Azure AD Graph](http://msdn.microsoft.com/library/azure/hh974476.aspx) επιτρέπει την πρόσβαση σε κατάλογο δεδομένων από την εφαρμογή του Azure.
-   Παρέχει SSO για [όλες τις εφαρμογές που υποστηρίζονται από το Azure Active Directory](/marketplace/active-directory/), συμπεριλαμβανομένου του Office 365, Dynamics CRM Online, Microsoft Intune και χιλιάδες εφαρμογές που δεν ανήκουν στη Microsoft cloud. 
-   Azure Active Directory υποστηρίζει έλεγχο πρόσβασης βάσει ρόλων. Μπορείτε να χρησιμοποιήσετε το μοτίβο [Authorize(Roles="X")] με ελάχιστους αλλαγές στον κώδικά σας.

Για να δείτε πώς να συντάξετε μια εφαρμογή Azure γραμμής εταιρικά που πραγματοποιεί έλεγχο ταυτότητας με το Azure Active Directory, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής Azure γραμμής εταιρικά με έλεγχο ταυτότητας Azure Active Directory](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Τον έλεγχο ταυτότητας μέσω ενός STS εσωτερικής εγκατάστασης
Εάν έχετε μια εσωτερική διακριτικού υπηρεσία ασφαλούς (STS) όπως υπηρεσίες Active Directory Federation Services (AD FS), μπορείτε να χρησιμοποιήσετε που για να δημιουργήσετε Ομοσπονδία ελέγχου ταυτότητας για την εφαρμογή του Azure. Αυτή η προσέγγιση είναι καλύτερη κατά την πολιτική της εταιρείας εμποδίζει που είναι αποθηκευμένα στο Azure AD δεδομένων. Ωστόσο, λάβετε υπόψη τα εξής:

-   STS τοπολογία πρέπει να είναι ανάπτυξη εσωτερικής εγκατάστασης, με τη διαχείριση και κόστος επιβάρυνσης.
-   Μόνο οι διαχειριστές AD FS να ρυθμίσετε τις παραμέτρους [κανόνες διεκδίκηση και υπηρεσία αξιοπιστίας αξιοπιστίας πάρτι](http://technet.microsoft.com/library/dd807108.aspx), που μπορεί να περιορίζει τον προγραμματιστή επιλογές. Από την άλλη πλευρά, είναι δυνατό να διαχειρίζεστε και να προσαρμόζετε [αξιώσεων](http://technet.microsoft.com/library/ee913571.aspx) σε βάση ανά εφαρμογή.
-   Πρόσβαση στην εσωτερική εγκατάσταση AD δεδομένων απαιτεί μια ξεχωριστή λύση μέσα από το εταιρικό τείχος προστασίας.

Για να δείτε πώς να συντάξετε μια εφαρμογή Azure γραμμής εταιρικά που πραγματοποιεί έλεγχο ταυτότητας με μια STS εσωτερικής εγκατάστασης, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής Azure γραμμής εταιρικά με έλεγχο ταυτότητας AD FS](web-sites-dotnet-lob-application-adfs.md).
 