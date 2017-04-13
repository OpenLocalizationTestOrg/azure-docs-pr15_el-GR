
<properties 
    pageTitle="Azure AD + απαιτήσεις υπηρεσίας καταλόγου Active Directory για Azure RemoteApp | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να ρυθμίσετε την υπηρεσία καταλόγου Active Directory για να εργαστείτε με Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + απαιτήσεις υπηρεσίας καταλόγου Active Directory για Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.


Για τη συλλογή υβριδική Azure RemoteApp ή για μια συλλογή cloud που θέλετε να δημιουργήσετε Ομοσπονδία με σύνδεση AD, πρέπει να κάνετε τα εξής.

### <a name="connect-azure-ad-and-active-directory"></a>Σύνδεση Azure AD και υπηρεσίας καταλόγου Active Directory

Εάν θέλετε να συνδέσετε το μισθωτή του Azure AD και σας περιβάλλοντα της υπηρεσίας καταλόγου Active Directory εσωτερικής εγκατάστασης, χρησιμοποιήστε AD Connect. Θα χρειαστεί να μόνο [4 κλικ](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) για να συνδέσετε τα δύο σε καταλόγους.

Σημείωση - συγχρονισμού καταλόγου είναι απαραίτητη για το υβριδικό συλλογών.

### <a name="make-sure-your-domaincom-match"></a>Βεβαιωθείτε ότι το "@domain.com" ταιριάζει με
Πριν ξεκινήσετε, βεβαιωθείτε ότι το UPN για το σύμπλεγμα διακομιστών εσωτερικής εγκατάστασης ταιριάζει με το επίθημα του τομέα σας Azure AD. 

Αφού ρυθμίσετε το επίθημα τομέα UPN στο Azure AD, όλες οι χρήστες που συνδέονται στο Azure RemoteApp θα συνδεθείτε ως “user@ <the suffix you set up>". Βεβαιωθείτε ότι οι χρήστες μπορούν να επίσης συνδεθείτε με το ίδιο user@suffix σε τομέα εσωτερικής εγκατάστασης. Σε ορισμένες περιπτώσεις, μπορείτε να ρυθμίσετε το όνομα ενός τομέα στο Azure AD ενώ ο καθορισμός επίθημα διαφορετικό τομέα για το χρήστη στην-prem. Σε αυτήν την περίπτωση, οι χρήστες δεν θα μπορείτε να συνδεθείτε σε οποιονδήποτε τομέα υπολογιστές ή σε πόρους μέσω Azure RemoteApp.

Για παράδειγμα, εάν ρυθμίσετε το επιθήματος UPN τομέα στο AAD ως contoso.com, αλλά ορισμένων χρηστών στην εσωτερική εγκατάσταση/AD έχουν ρυθμιστεί για να συνδεθείτε με @contoso.uk, , στη συνέχεια, αυτοί οι χρήστες δεν θα μπορούν να συνδέονται σωστά στη συλλογή ARA. Οι χρήστες UPN στο AAD και AD πρέπει να είναι το ίδιο για να είναι δυνατή η σύνδεση"

### <a name="create-objects-for-azure-remoteapp"></a>Δημιουργία αντικειμένων για Azure RemoteApp
Πρέπει επίσης να δημιουργήσετε το ακόλουθο εσωτερικής αντικείμενα υπηρεσίας καταλόγου Active Directory:

- Ένας λογαριασμός υπηρεσίας για την παροχή πρόσβασης σε πόρους του τομέα για προγράμματα RemoteApp κατά τη συμμετοχή σε RDSH τελικά σημεία στον τομέα εσωτερικής εγκατάστασης.
- Μια οργανική μονάδα (OU) ώστε να περιέχει αντικείμενα υπολογιστή RemoteApp. Χρήση των την οργανική Μονάδα προτεινόμενα (αλλά δεν απαιτείται) για την απομόνωση τους λογαριασμούς και τις πολιτικές που θα χρησιμοποιήσετε με RemoteApp.

Χρειάζεστε και των δύο αυτών των αντικειμένων, όταν δημιουργείτε τη συλλογή RemoteApp, επομένως, φροντίστε να κάνετε πρώτα τα παρακάτω βήματα.