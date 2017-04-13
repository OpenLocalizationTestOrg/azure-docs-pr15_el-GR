
<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε Azure RemoteApp με λογαριασμούς χρήστη του Office 365 | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure RemoteApp με μου λογαριασμούς χρήστη του Office 365"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Πώς μπορείτε να χρησιμοποιήσετε Azure RemoteApp με λογαριασμούς χρήστη του Office 365

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Εάν έχετε μια συνδρομή στο Office 365 που έχετε μια Azure Active Directory που αποθηκεύει τα ονόματα χρηστών και κωδικών πρόσβασης που χρησιμοποιούνται για να αποκτήσετε πρόσβαση σε υπηρεσίες του Office 365. Για παράδειγμα, όταν οι χρήστες σας ενεργοποίηση του Office 365 ProPlus εκτελούν έλεγχο ταυτότητας έναντι Azure AD για να ελέγξετε για άδειες χρήσης. Οι περισσότεροι πελάτες θα θέλατε να χρησιμοποιήσετε το ίδιο κατάλογο με το Azure RemoteApp.

Εάν αναπτύσσετε Azure RemoteApp πιθανώς χρησιμοποιείτε μια συνδρομή Azure που είναι συσχετισμένη με μια διαφορετική Azure AD. Για να χρησιμοποιήσετε το Office 365 καταλόγου, θα πρέπει να μετακινήσετε το Azure συνδρομής σε αυτόν τον κατάλογο.

Για πληροφορίες σχετικά με τον τρόπο ανάπτυξης εφαρμογών προγράμματος-πελάτη του Office 365, ανατρέξτε στο θέμα [πώς μπορείτε να χρησιμοποιήσετε με το Azure RemoteApp συνδρομή στο Office 365](remoteapp-officesubscription.md).
 
## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Φάση 1: Καταχώρηση δωρεάν συνδρομή σας στο Office 365 Azure Active Directory
Εάν χρησιμοποιείτε το Azure κλασική πύλη, χρησιμοποιήστε τα βήματα στο [μητρώο σας δωρεάν συνδρομή Azure Active Directory](https://technet.microsoft.com/library/dn832618.aspx) για να λάβετε δικαιώματα πρόσβασης διαχειριστή για να σας Azure AD μέσω της πύλης διαχείρισης Azure. Ως το αποτέλεσμα αυτής της διαδικασίας θα πρέπει να μπορείτε να συνδεθείτε στην πύλη του Azure και να δείτε τον κατάλογό σας εκεί – σε αυτό το σημείο δεν θα βλέπετε πολύ πιο εφόσον η πλήρης Azure συνδρομή που χρησιμοποιείτε με το Azure RemoteApp βρίσκεται σε διαφορετικό κατάλογο.

Να θυμάστε το όνομα και τον κωδικό πρόσβασης του λογαριασμού του διαχειριστή που δημιουργήσατε σε αυτό το βήμα-θα χρειαστούν σε φάση 2.

Εάν χρησιμοποιείτε την πύλη του Azure, δείτε [πώς μπορείτε να καταχωρήσετε και να ενεργοποιήσετε μια δωρεάν Azure Active Directory χρησιμοποιώντας την πύλη του Office 365](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>Φάση 2: Αλλαγή του Azure AD που σχετίζεται με τη συνδρομή σας στο Azure.
Θα σας πρόκειται να αλλάξετε τη συνδρομή σας στο Azure από του τρέχοντος καταλόγου στο Office 365 κατάλογο μας εργαστεί στο φάση 1.

Ακολουθήστε τις οδηγίες που περιγράφονται στο θέμα [Αλλαγή του μισθωτή Azure Active Directory στο Azure RemoteApp](remoteapp-changetenant.md). Δώστε ιδιαίτερη προσοχή στα παρακάτω βήματα:

- Βήμα #1: Εάν έχετε αναπτύξει RemoteApp Azure (ARA) σε αυτήν την εγγραφή, βεβαιωθείτε ότι καταργείτε όλους τους λογαριασμούς χρηστών Azure AD από όλες τις συλλογές ARA πρώτα, πριν να επιχειρήσετε οτιδήποτε άλλο. Εναλλακτικά, μπορείτε να τη διαγραφή οποιαδήποτε υπάρχοντα συλλογών.
- Βήμα #2: Αυτό είναι ένα κρίσιμο βήμα. Πρέπει να χρησιμοποιήσετε ένα λογαριασμό Microsoft (π.χ. @outlook.com) ως διαχειριστής υπηρεσιών της συνδρομής, αυτό συμβαίνει επειδή δεν έχουμε όλοι οι λογαριασμοί χρηστών από την υπάρχουσα Azure AD συνδέονται με τη συνδρομή – αυτή την περίπτωση, δεν θα μπορείτε να το μετακινήσετε σε ένα διαφορετικό Azure AD.
- Βήμα #4: Όταν προσθέτετε έναν υπάρχοντα κατάλογο, το σύστημα θα σας ζητήσει να συνδεθείτε με το λογαριασμό διαχειριστή για αυτόν τον κατάλογο. Φροντίστε να χρησιμοποιήσετε το λογαριασμό διαχειριστή από φάση 1.
- Βήμα #5: Αλλάξτε τον κατάλογο γονική της συνδρομής στον κατάλογό σας Office 365. Το τελικό αποτέλεσμα θα πρέπει να είναι ότι στην περιοχή Ρυθμίσεις -> συνδρομές τη συνδρομή σας λίστες του καταλόγου του Office 365. 
![Αλλαγή του γονικού καταλόγου της συνδρομής](./media/remoteapp-o365user/settings.png)
 

Σε αυτό το σημείο τη συνδρομή σας στο Azure RemoteApp είναι συσχετισμένη με το Office 365 Azure AD; Μπορείτε να χρησιμοποιήσετε τους υπάρχοντες λογαριασμούς χρήστη του Office 365 με Azure RemoteApp!



