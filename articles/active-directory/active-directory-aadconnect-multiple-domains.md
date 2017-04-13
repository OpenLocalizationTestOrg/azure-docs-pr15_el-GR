<properties
    pageTitle="Azure AD Connect πολλούς τομείς"
    description="Αυτό το έγγραφο περιγράφει ρύθμιση και τη ρύθμιση των παραμέτρων πολλούς τομείς ανώτατου επιπέδου με το O365 και το Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Υποστήριξη πολλών τομέα για την ενοποίηση με το Azure AD
Την ακόλουθη τεκμηρίωση παρέχει οδηγίες σχετικά με τη χρήση πολλών τομέων ανώτατου επιπέδου και υποτομείς κατά την ενοποίηση με το Office 365 ή τομείς Azure AD.

## <a name="multiple-top-level-domain-support"></a>Υποστήριξη πολλών τομέας ανώτατου επιπέδου
Ενοποίηση πολλών, τομείς ανώτατου επιπέδου με Azure AD απαιτεί ορισμένες πρόσθετες ρυθμίσεις παραμέτρων που δεν είναι απαραίτητο, κατά την ενοποίηση με μία τομέας ανώτατου επιπέδου.

Όταν ένας τομέας έχει συνενωθεί με Azure AD, πολλές ιδιότητες ορίζονται για τον τομέα στο Azure.  Ένα σημαντικό είναι IssuerUri.  Αυτό είναι ένα URI που χρησιμοποιείται από Azure AD για να προσδιορίσετε τον τομέα που σχετίζεται με το διακριτικό.  Το URI δεν πρέπει να επιλύεται καθετί άλλο παρά πρέπει να είναι ένα έγκυρο URI.  Από προεπιλογή, Azure AD ορίζει αυτή την επιλογή της τιμής του αναγνωριστικού υπηρεσίας ομοσπονδίας στο σας στην εσωτερική εγκατάσταση AD FS ρύθμισης παραμέτρων.

>[AZURE.NOTE]Το αναγνωριστικό υπηρεσίας Ομοσπονδία είναι ένα URI που προσδιορίζει μοναδικά μια υπηρεσία ομοσπονδίας.  Η υπηρεσία Ομοσπονδία είναι μια παρουσία του AD FS που λειτουργεί ως την υπηρεσία διακριτικού ασφαλείας. 

Μπορείτε να κάνετε vew IssuerUri, χρησιμοποιώντας την εντολή PowerShell `Get-MsolDomainFederationSettings - DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Πρόβλημα προκύπτει όταν θέλουμε να προσθέσετε περισσότερους από έναν τομείς ανώτατου επιπέδου.  Για παράδειγμα, ας υποθέσουμε ότι έχετε ρύθμισης ομοσπονδίας μεταξύ Azure AD και το περιβάλλον εσωτερικής εγκατάστασης.  Για αυτό το έγγραφο χρησιμοποιώ bmcontoso.com.  Τώρα που έχετε προσθέσει μια δεύτερη, ανώτατου επιπέδου τομέα, bmfabrikam.com.

![Τομείς](./media/active-directory-multiple-domains/domains.png)

Όταν θα σας επιχειρήσουν να μετατρέψετε μας bmfabrikam.com τομέα, ώστε να είναι ομόσπονδες, λαμβάνουμε σφάλμα.  Ο λόγος για αυτό είναι, Azure AD έχει έναν περιορισμό που δεν επιτρέπει την ιδιότητα IssuerUri ώστε να έχουν την ίδια τιμή για περισσότερους από έναν τομέα.  
  

![Ομοσπονδία σφάλματος](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>Η παράμετρος SupportMultipleDomain

Για να επιλύσετε αυτό, θα χρειαστεί να προσθέσετε ένα διαφορετικό IssuerUri που μπορεί να γίνει με τη χρήση του `-SupportMultipleDomain` παραμέτρου.  Αυτή η παράμετρος χρησιμοποιείται με τα ακόλουθα cmdlet:
    
- `New-MsolFederatedDomain`
- `Convert-MsolDomaintoFederated`
- `Update-MsolFederatedDomain`

Αυτή η παράμετρος κάνει Azure AD ρύθμιση παραμέτρων του IssuerUri έτσι ώστε να βασίζεται στο όνομα του τομέα.  Αυτό θα είναι μοναδικό σε καταλόγους στο Azure AD.  Χρήση της παραμέτρου επιτρέπει την εντολή PowerShell για να ολοκληρωθεί με επιτυχία.

![Ομοσπονδία σφάλματος](./media/active-directory-multiple-domains/convert.png)

Ανατρέξετε στις ρυθμίσεις του μας νέο τομέα bmfabrikam.com μπορείτε να δείτε τα εξής:

![Ομοσπονδία σφάλματος](./media/active-directory-multiple-domains/settings.png)

Λάβετε υπόψη ότι `-SupportMultipleDomain` δεν αλλάζει τα άλλα τελικά σημεία που εξακολουθούν να έχουν ρυθμιστεί ώστε να οδηγεί μας ομοσπονδιακή υπηρεσία σε adfs.bmcontoso.com.

Κάτι άλλο που `-SupportMultipleDomain` κάνει είναι ότι εξασφαλίζει ότι το σύστημα AD FS περιλαμβάνει τη σωστή τιμή εκδότη στο διακριτικών που εκδίδονται για Azure AD. Αυτό γίνεται με λήψη τμήμα τομέα των χρηστών UPN και ορίσετε αυτήν τη ρύθμιση ως τομέα με το IssuerUri, δηλαδή https://{upn επίθημα} / adfs/υπηρεσίες αξιοπιστίας. 

Επομένως, κατά τη διάρκεια του ελέγχου ταυτότητας Azure AD ή Office 365, το στοιχείο IssuerUri στο διακριτικό του χρήστη χρησιμοποιείται για να εντοπίσετε τον τομέα στο Azure AD.  Εάν δεν είναι δυνατή η εύρεση συμφωνία ο έλεγχος ταυτότητας θα αποτύχει. 

Για παράδειγμα, εάν ένας χρήστης του χρήστη (UPN) είναι bsimon@bmcontoso.com, το στοιχείο IssuerUri στο τα προβλήματα διακριτικό AD FS θα οριστεί http://bmcontoso.com/adfs/services/trust. Αυτό θα ταιριάζουν με τη ρύθμιση παραμέτρων Azure AD και θα ολοκληρωθεί με επιτυχία τον έλεγχο ταυτότητας.

Ακολουθεί ο κανόνας προσαρμοσμένων διεκδίκηση που υλοποιεί αυτή η λογική:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type =   "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


>[AZURE.IMPORTANT]Για να χρησιμοποιήσετε το διακόπτη - SupportMultipleDomain όταν προσπαθείτε να προσθέσετε νέα ή μετατροπή ήδη προστεθεί τομείς, πρέπει να έχετε το πρόγραμμα εγκατάστασης σας ομόσπονδη αξιοπιστίας για την υποστήριξη τα αρχικά.  


## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Πώς μπορείτε να ενημερώσετε τη σχέση αξιοπιστίας μεταξύ AD FS και Azure AD
Εάν μπορείτε να ρυθμίσετε την ομόσπονδη σχέση αξιοπιστίας μεταξύ AD FS και την παρουσία του Azure AD δεν, ίσως χρειαστεί να δημιουργήσετε ξανά αυτή η σχέση αξιοπιστίας.  Αυτό συμβαίνει επειδή, όταν είναι αρχικά εγκατάστασης χωρίς την `-SupportMultipleDomain` παράμετρο, το IssuerUri έχει οριστεί με την προεπιλεγμένη τιμή.  Στο στιγμιότυπο οθόνης κάτω από το στοιχείο που βλέπετε το IssuerUri έχει οριστεί σε https://adfs.bmcontoso.com/adfs/services/trust.

Τώρα, συνεπώς, εάν έχετε προσθέσει με επιτυχία ένα νέο τομέα στην πύλη του Azure AD και, στη συνέχεια, προσπαθήστε να τον μετατρέψετε χρησιμοποιώντας `Convert-MsolDomaintoFederated -DomainName <your domain>`, λαμβάνουμε το ακόλουθο σφάλμα.

![Ομοσπονδία σφάλματος](./media/active-directory-multiple-domains/trust1.png)

Εάν προσπαθήσετε να προσθέσετε το `-SupportMultipleDomain` αλλαγής θα λαμβάνει το ακόλουθο σφάλμα:

![Ομοσπονδία σφάλματος](./media/active-directory-multiple-domains/trust2.png)

Προσπαθείτε απλώς να εκτελέσετε `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` τον αρχικό τομέα θα επίσης έχει ως αποτέλεσμα σφάλμα.

![Ομοσπονδία σφάλματος](./media/active-directory-multiple-domains/trust3.png)

Χρησιμοποιήστε τα παρακάτω βήματα για να προσθέσετε έναν επιπλέον τομέα ανώτατου επιπέδου.  Εάν έχετε ήδη προσθέσει κάποιον τομέα και δεν χρησιμοποιήσατε το `-SupportMultipleDomain` παράμετρο έναρξης με τα βήματα για την κατάργηση και ενημέρωση τον αρχικό τομέα.  Εάν δεν έχετε προσθέσει μια τομέας ανώτατου επιπέδου ακόμα μπορείτε να ξεκινήσετε με τα βήματα για να προσθέσετε έναν τομέα με χρήση του PowerShell Azure AD Connect.

Χρησιμοποιήστε τα παρακάτω βήματα για να καταργήσετε την αξιοπιστία Microsoft Online και να ενημερώσετε τον αρχικό τομέα.

2.  Στο διακομιστή ομοσπονδίας σας AD FS Άνοιγμα **διαχείρισης AD FS.** 
2.  Στα αριστερά, αναπτύξτε το στοιχείο **Σχέσεις αξιοπιστίας** και **Βασίζεστε εμπιστεύεται πάρτι**
3.  Στα δεξιά, διαγράψτε την καταχώρηση **Πλατφόρμα ταυτότητας του Microsoft Office 365** .
![Κατάργηση του Microsoft Online](./media/active-directory-multiple-domains/trust4.png)
1.  Σε υπολογιστή με εγκατεστημένο το [Azure Active Directory λειτουργικής μονάδας για το Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) , εκτελέστε τα εξής: `$cred=Get-Credential`.  
2.  Πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης του ο Καθολικός διαχειριστής για τον τομέα Azure AD ενοποίηση με το
2.  Στο PowerShell εισαγάγετε`Connect-MsolService -Credential $cred`
4.  Στο PowerShell πληκτρολογήστε `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Πρόκειται για τον αρχικό τομέα.  Επομένως, χρησιμοποιώντας το παραπάνω τομείς που μπορεί να είναι:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`


Χρησιμοποιήστε τα ακόλουθα βήματα για να προσθέσετε το νέο τομέας ανώτατου επιπέδου με χρήση του PowerShell

1.  Σε υπολογιστή με εγκατεστημένο το [Azure Active Directory λειτουργικής μονάδας για το Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) , εκτελέστε τα εξής: `$cred=Get-Credential`.  
2.  Πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης του ο Καθολικός διαχειριστής για τον τομέα Azure AD ενοποίηση με το
2.  Στο PowerShell εισαγάγετε`Connect-MsolService -Credential $cred`
3.  Στο PowerShell εισαγάγετε`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Χρησιμοποιήστε τα ακόλουθα βήματα για να προσθέσετε το νέο τομέας ανώτατου επιπέδου με χρήση Azure AD Connect.

1.  Εκκίνηση Azure AD Connect από την επιφάνεια εργασίας ή μενού "Έναρξη"
2.  Επιλέξτε "Προσθήκη επιπλέον τομέα Azure AD" ![προσθέσετε έναν επιπλέον τομέα Azure AD](./media/active-directory-multiple-domains/add1.png)
3.  Εισαγάγετε το Azure AD και τα διαπιστευτήρια της υπηρεσίας καταλόγου Active Directory
4.  Επιλέξτε τον δεύτερο τομέα που θέλετε να ρυθμίσετε τις παραμέτρους για την Ομοσπονδία.
![Προσθήκη μιας επιπλέον τομέα Azure AD](./media/active-directory-multiple-domains/add2.png)
5.  Κάντε κλικ στην επιλογή εγκατάσταση


### <a name="verify-the-new-top-level-domain"></a>Επαληθεύστε τον νέο τομέα ανώτατου επιπέδου
Χρησιμοποιώντας την εντολή PowerShell `Get-MsolDomainFederationSettings - DomainName <your domain>`μπορείτε να προβάλετε το ενημερωμένο IssuerUri.  Το παρακάτω στιγμιότυπο οθόνης εμφανίζει την Ομοσπονδία ρυθμίσεις ενημερώθηκαν στις μας αρχικό http://bmcontoso.com/adfs/services/trust τομέα

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Και το IssuerUri σε μας νέο τομέα έχει οριστεί σε https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)


##<a name="support-for-sub-domains"></a>Υποστήριξη για δευτερεύοντες τομείς
Όταν προσθέτετε έναν υποτομέα, λόγω των τρόπο Azure AD χειρισμού των τομέων, αυτό θα μεταβιβάζονται οι ρυθμίσεις της γονικής τοποθεσίας.  Αυτό σημαίνει ότι το IssuerUri πρέπει να συμφωνεί με τους γονείς.

Επομένως, σας επιτρέπει να πείτε για παράδειγμα που έχουν bmcontoso.com και, στη συνέχεια, προσθέσετε corp.bmcontoso.com.  Αυτό σημαίνει ότι το IssuerUri για ένα χρήστη από corp.bmcontoso.com θα πρέπει να είναι **http://bmcontoso.com/adfs/services/trust.**  Ωστόσο, ο κανόνας τυπική υλοποιηθεί παραπάνω για Azure AD, θα δημιουργήσει ένα διακριτικό με έναν εκδότη ως **http://corp.bmcontoso.com/adfs/services/trust.** που δεν θα ταιριάζουν με τον τομέα απαιτείται τιμή και ο έλεγχος ταυτότητας θα αποτύχει.

### <a name="how-to-enable-support-for-sub-domains"></a>Πώς μπορείτε να ενεργοποιήσετε την υποστήριξη για δευτερεύοντες τομείς
Για να επιλύσετε αυτό το AD FS υπολογιστή αξιοπιστίας κατασκευαστή για το Microsoft Online πρέπει να ενημερωθεί.  Για να το κάνετε αυτό, πρέπει να ρυθμίσετε έναν κανόνα προσαρμοσμένης διεκδίκηση ώστε να λωρίδες απενεργοποίηση οποιαδήποτε υποτομείς από επιθήματος UPN του χρήστη κατά την κατασκευή την προσαρμοσμένη τιμή εκδότη. 

Το παρακάτω αίτημα θα κάντε τα εξής:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));

Χρησιμοποιήστε τα ακόλουθα βήματα για να προσθέσετε ένα προσαρμοσμένο διεκδίκηση για την υποστήριξη υποτομείς.

1.  Άνοιγμα AD FS διαχείρισης
2.  Κάντε δεξί κλικ την αξιοπιστία Microsoft Online RP και επιλέξτε Επεξεργασία διεκδίκηση κανόνες
3.  Επιλέξτε τον κανόνα τρίτο διεκδίκηση και αντικαταστήστε ![διεκδίκηση επεξεργασία](./media/active-directory-multiple-domains/sub1.png)
4.  Αντικαταστήστε την τρέχουσα απαίτηση:
    
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
        
    με το
    
        `c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));`
    
![Αντικατάσταση διεκδίκηση](./media/active-directory-multiple-domains/sub2.png)
5.  Κάντε κλικ στο κουμπί Ok.  Κάντε κλικ στην επιλογή εφαρμογή.  Κάντε κλικ στο κουμπί Ok.  Κλείστε το AD FS διαχείρισης.

