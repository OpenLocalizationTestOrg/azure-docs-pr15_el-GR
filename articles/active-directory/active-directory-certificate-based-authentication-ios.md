<properties 
    pageTitle="Γρήγορα αποτελέσματα με τον έλεγχο ταυτότητας πιστοποιητικού που βασίζεται σε iOS | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τον έλεγχο ταυτότητας πιστοποιητικού που βασίζεται στις λύσεις με συσκευές iOS" 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/21/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-ios---public-preview"></a>Γρήγορα αποτελέσματα με τον έλεγχο ταυτότητας πιστοποιητικού που βασίζεται σε iOS - Public Preview

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android](active-directory-certificate-based-authentication-android.md)


Αυτό το θέμα δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να χρησιμοποιούν με βάση τον έλεγχο ταυτότητας πιστοποιητικού (CBA) σε συσκευή iOS για τους χρήστες του μισθωτές στα προγράμματα του Office 365 για μεγάλες επιχειρήσεις και για εκπαιδευτικά ιδρύματα. 

CBA σάς επιτρέπει να υποβληθούν σε έλεγχο ταυτότητας από το Azure Active Directory με ένα πιστοποιητικό προγράμματος-πελάτη σε συσκευή Android ή iOS κατά τη σύνδεση το λογαριασμό του Exchange online για: 

- Εφαρμογές για κινητές συσκευές Office όπως το Microsoft Outlook και το Microsoft Word   
- Προγράμματα-πελάτες του Exchange ActiveSync (EAS) 

Ρύθμιση παραμέτρων αυτή η δυνατότητα αποκλείει την ανάγκη για να εισαγάγετε ένα συνδυασμό όνομα χρήστη και τον κωδικό πρόσβασης σε ορισμένες αλληλογραφίας και τις εφαρμογές του Microsoft Office στην κινητή συσκευή σας. 
 

## <a name="supported-scenarios-and-requirements"></a>Σενάρια που υποστηρίζονται και απαιτήσεις  



### <a name="general-requirements"></a>Γενικές απαιτήσεις 


Για όλα τα σενάρια σε αυτό το θέμα, απαιτούνται οι ακόλουθες εργασίες:  

- Πρόσβαση σε authority(s) πιστοποιητικό για την έκδοση προγράμματος-πελάτη πιστοποιητικών.  

- Τα πιστοποιητικά authority(s) πρέπει να ρυθμιστεί στο Azure Active Directory. Μπορείτε να βρείτε λεπτομερείς οδηγίες σχετικά με τον τρόπο για να ολοκληρώσετε τη ρύθμιση παραμέτρων στην ενότητα [Γρήγορα αποτελέσματα](#getting-started) .  

- Η αρχή έκδοσης πιστοποιητικών ρίζας και τις αρχές έκδοσης πιστοποιητικών ενδιάμεση πρέπει να ρυθμιστεί στο Azure Active Directory.  

- Κάθε αρχή έκδοσης πιστοποιητικών πρέπει να έχετε μια λίστα ανάκλησης πιστοποιητικών (CRL) που μπορούν να χρησιμοποιηθούν μέσω Internet αντικριστές διεύθυνση URL.  

- Πρέπει να εκδοθεί το πιστοποιητικό προγράμματος-πελάτη για έλεγχο ταυτότητας του προγράμματος-πελάτη.  


- Για το Exchange ActiveSync μόνο πελάτες, στους οποίους το πιστοποιητικό προγράμματος-πελάτη πρέπει να έχετε διεύθυνση δυνατότητα δρομολόγησης ηλεκτρονικού ταχυδρομείου του χρήστη στο Exchange online στο το κύριο όνομα ή την τιμή όνομα RFC822 του θέματος εναλλακτικό όνομα πεδίου. Azure Active Directory αντιστοιχίζει την τιμή RFC822 στο χαρακτηριστικό διεύθυνσης διακομιστή μεσολάβησης στον κατάλογο.  



### <a name="office-mobile-applications-support"></a>Υποστήριξη εφαρμογές για κινητές συσκευές του Office 

| Εφαρμογές                      | Υποστήριξη      |
| ---                       | ---          |
| Το Word / του Excel / PowerPoint | ![Έλεγχος][1]  |
| Το OneNote                   | ![Έλεγχος][1]  |
| OneDrive                  | ![Έλεγχος][1]  |
| Το Outlook                   | ![Έλεγχος][1]  |
| Yammer                    | ![Έλεγχος][1]  |
| Skype για επιχειρήσεις        | Έρχομαι σύντομα  |


### <a name="requirements"></a>Απαιτήσεις  

Η συσκευή OS έκδοση πρέπει να είναι iOS 9 και παραπάνω 

Πρέπει να ρυθμιστεί ενός διακομιστή ομοσπονδίας.  

Ο έλεγχος ταυτότητας Azure απαιτείται για εφαρμογές του Office σε iOS.  

Για το Azure Active Directory για να ανακαλέσετε ένα πιστοποιητικό προγράμματος-πελάτη, το διακριτικό ADFS πρέπει να έχει τις ακόλουθες απαιτήσεις:  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Τον σειριακό αριθμό του πιστοποιητικού προγράμματος-πελάτη) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(Η συμβολοσειρά για τον εκδότη του πιστοποιητικού προγράμματος-πελάτη) 

Azure Active Directory προσθέτει αυτές τις απαιτήσεις για το διακριτικό ανανέωσης, εάν είναι διαθέσιμη στο το διακριτικό ADFS (ή οποιοδήποτε άλλο διακριτικό SAML). Όταν το διακριτικό ανανέωσης πρέπει να επικυρώνονται, αυτές οι πληροφορίες χρησιμοποιούνται για να ελέγξετε το ανάκλησης. 

Ως βέλτιστη πρακτική, θα πρέπει να ενημερώσετε τις σελίδες σφάλματος ADFS με τα εξής:

- Η απαίτηση για την εγκατάσταση της υπηρεσίας ελέγχου ταυτότητας Azure σε iOS

- Οδηγίες σχετικά με τον τρόπο για να λάβετε ένα πιστοποιητικό χρήστη. 

Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Προσαρμογή των AD FS εισόδου σελίδων](https://technet.microsoft.com/library/dn280950.aspx).  



### <a name="exchange-activesync-clients-support"></a>Υποστήριξη προγράμματα-πελάτες του Exchange ActiveSync 


Σε iOS 9 ή νεότερη έκδοση, το πρόγραμμα-πελάτη ηλεκτρονικού ταχυδρομείου εγγενούς iOS υποστηρίζεται. Για όλες τις άλλες εφαρμογές Exchange ActiveSync, για να προσδιορίσετε εάν η δυνατότητα υποστηρίζεται, επικοινωνήστε με τον προγραμματιστή της εφαρμογής.  



## <a name="getting-started"></a>Γρήγορα αποτελέσματα 


Για να ξεκινήσετε, πρέπει να ρυθμίσετε τις παραμέτρους τις αρχές έκδοσης πιστοποιητικών στην υπηρεσία καταλόγου Azure Active Directory. Για κάθε αρχή έκδοσης πιστοποιητικών, αποστείλετε τα εξής: 

- Το δημόσιο τμήμα του πιστοποιητικού, σε μορφή *.cer* 

- Στο Internet αντικριστές διευθύνσεις URL, όπου βρίσκονται τα οι λίστες ανάκλησης πιστοποιητικών (CRL)
 

Ακολουθεί η διάταξη για μια αρχή έκδοσης πιστοποιητικών: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Για να αποστείλετε τις πληροφορίες, μπορείτε να χρησιμοποιήσετε τη λειτουργική μονάδα Azure AD μέσω του Windows PowerShell.  
Ακολουθούν παραδείγματα για την προσθήκη, κατάργηση ή τροποποίηση μια αρχή έκδοσης πιστοποιητικών. 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>Ρύθμιση παραμέτρων του μισθωτή Azure AD για το πιστοποιητικό ελέγχου ταυτότητας με 

1. Εκκινήστε το Windows PowerShell με δικαιώματα διαχειριστή. 

2. Εγκαταστήστε τη λειτουργική μονάδα Azure AD. Πρέπει να εγκαταστήσετε την έκδοση [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) ή νεότερη έκδοση.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Συνδεθείτε στο μισθωτή του προορισμού: 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Προσθήκη ενός νέου αρχή έκδοσης πιστοποιητικών

1. Ορισμός ιδιοτήτων διάφορες από την αρχή έκδοσης πιστοποιητικών και προσθήκη της σε Azure Active Directory: 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. Λάβετε τις αρχές έκδοσης πιστοποιητικών: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>Ανάκτηση τις αρχές έκδοσης πιστοποιητικών λίστας

Ανάκτηση τις αρχές έκδοσης πιστοποιητικών που είναι αποθηκευμένα στο Azure Active Directory για το μισθωτή: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>Κατάργηση μια αρχή έκδοσης πιστοποιητικών

1.  Ανάκτηση τις αρχές έκδοσης πιστοποιητικών: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Καταργήστε το πιστοποιητικό για την αρχή έκδοσης πιστοποιητικών: 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying μια αρχή έκδοσης πιστοποιητικών 

1.  Ανάκτηση τις αρχές έκδοσης πιστοποιητικών: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Τροποποιήστε τις ιδιότητες στην αρχή έκδοσης πιστοποιητικών: 

        $c[0].AuthorityType=1 

3. Ορίστε την **αρχή έκδοσης πιστοποιητικών**: 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>Έλεγχος Office εφαρμογές για κινητές συσκευές  

Για να δοκιμάσετε το πιστοποιητικό ελέγχου ταυτότητας στην εφαρμογή του Office mobile: 

1.  Στη συσκευή σας δοκιμής, εγκαταστήσετε μια εφαρμογή κινητές συσκευές του Office (π.χ., OneDrive) από το App Store.

2.  Βεβαιωθείτε ότι το πιστοποιητικό χρήστη που έχει αποδοθεί στη συσκευή σας δοκιμής. 

3.  Εκκίνηση της εφαρμογής. 

4.  Πληκτρολογήστε το όνομα χρήστη και, στη συνέχεια, επιλέξτε το πιστοποιητικό χρήστη που θέλετε να χρησιμοποιήσετε. 

Που θα πρέπει να είναι είσοδος ολοκληρώθηκε με επιτυχία. 





## <a name="testing-exchange-activesync-client-applications"></a>Δοκιμή εφαρμογές προγράμματος-πελάτη Exchange ActiveSync

Για την πρόσβαση μέσω του ελέγχου ταυτότητας πιστοποιητικού που βασίζεται στο Exchange ActiveSync, πρέπει να είναι διαθέσιμα στην εφαρμογή προφίλ του EAS που περιέχει το πιστοποιητικό προγράμματος-πελάτη. Στο προφίλ EAS πρέπει να περιέχει τις ακόλουθες πληροφορίες:

- Το πιστοποιητικό χρήστη που θα χρησιμοποιηθεί για τον έλεγχο ταυτότητας 

- Το τελικό σημείο EAS πρέπει να είναι outlook.office365.com (όπως αυτή η δυνατότητα υποστηρίζεται αυτήν τη στιγμή μόνο στο περιβάλλον πολλαπλών μισθωτή Exchange online)

Προφίλ του EAS μπορεί να ρυθμίσει τις παραμέτρους και να τοποθετηθεί στη συσκευή μέσω της χρήσης από ένα MDM όπως το Intune ή τοποθετώντας με μη αυτόματο τρόπο το πιστοποιητικό στο προφίλ EAS στη συσκευή.  

### <a name="testing-eas-client-applications-on-ios"></a>Δοκιμές EAS εφαρμογές προγράμματος-πελάτη στο iOS 

Για να δοκιμάσετε το πιστοποιητικό ελέγχου ταυτότητας με την εφαρμογή εγγενής αλληλογραφίας στο iOS 9 ή νεότερη έκδοση: 

1.  Ρυθμίστε τις παραμέτρους ενός προφίλ EAS που πληροί τις απαιτήσεις παραπάνω. 

2.  Εγκαταστήσετε το προφίλ στη συσκευή iOS (είτε χρησιμοποιώντας μια MDM, όπως το Intune ή την εφαρμογή διαμορφωτή Apple)

3.  Μόλις εγκαταστήσετε το προφίλ σωστά, ανοίξτε την εγγενή εφαρμογή αλληλογραφίας και βεβαιωθείτε ότι συγχρονισμό αλληλογραφίας



## <a name="revocation"></a>Ανάκλησης

Για να ανακαλέσετε ένα πιστοποιητικό προγράμματος-πελάτη, Azure Active Directory λαμβάνει λίστα ανάκλησης πιστοποιητικών (CRL) από τις διευθύνσεις URL που έχουν αποσταλεί ως μέρος των πληροφοριών αρχή πιστοποιητικό και αποθηκεύει το. Η τελευταία δημοσίευση χρονικής σήμανσης (**Ημερομηνία ισχύος** ιδιότητα) στο το CRL χρησιμοποιείται για να βεβαιωθείτε ότι το CRL εξακολουθεί να είναι έγκυρο. Η CRL γίνεται περιοδικά αναφορά για να ανακαλέσετε την πρόσβαση σε πιστοποιητικά που αποτελούν μέρος της λίστας.

Εάν απαιτείται μια πιο άμεση ανάκλησης (για παράδειγμα, εάν ένας χρήστης χάσει μια συσκευή), μπορούν να ακυρωθούν το διακριτικό εξουσιοδότησης του χρήστη. Για να ακυρώσει το διακριτικό εξουσιοδότησης, ορίστε το πεδίο **StsRefreshTokenValidFrom** για αυτόν τον συγκεκριμένο χρήστη χρησιμοποιώντας το Windows PowerShell. Πρέπει να ενημερώσετε το πεδίο **StsRefreshTokenValidFrom** για κάθε χρήστη που θέλετε να ανακαλέσετε την πρόσβαση για.
 
Για να βεβαιωθείτε ότι η ανάκληση εξακολουθεί να εμφανίζεται, πρέπει να ορίσετε την **Ημερομηνία ισχύος** του το CRL σε μια ημερομηνία μετά την τιμή οριστεί από **StsRefreshTokenValidFrom** και βεβαιωθείτε ότι το πιστοποιητικό εν λόγω βρίσκεται στο το CRL.
 
Ακολουθήστε τα παρακάτω βήματα περιγράφουν τη διαδικασία για την ενημέρωση και την ακύρωση το διακριτικό εξουσιοδότησης, ορίζοντας το πεδίο **StsRefreshTokenValidFrom** . 

1. Σύνδεση με τα διαπιστευτήρια διαχειριστή για την υπηρεσία MSOL: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  Ανακτήστε την τρέχουσα τιμή StsRefreshTokensValidFrom για ένα χρήστη: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Ρύθμιση παραμέτρων του μια νέα τιμή StsRefreshTokensValidFrom για το χρήστη που ισούται με την τρέχουσα χρονική σήμανση: 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


Η ημερομηνία που ορίζετε πρέπει να είναι στο μέλλον. Εάν η ημερομηνία δεν είναι στο μέλλον, η ιδιότητα **StsRefreshTokensValidFrom** δεν έχει οριστεί. Εάν η ημερομηνία είναι μελλοντική, **StsRefreshTokensValidFrom** έχει οριστεί για την τρέχουσα ώρα (όχι την ημερομηνία που υποδεικνύεται από την εντολή Set-MsolUser). 



<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png