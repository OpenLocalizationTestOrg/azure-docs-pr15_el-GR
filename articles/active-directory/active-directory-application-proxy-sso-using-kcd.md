<properties
    pageTitle="Καθολικής σύνδεσης με το διακομιστή μεσολάβησης εφαρμογής | Microsoft Azure"
    description="Περιγράφει πώς μπορείτε να δώσετε καθολικής σύνδεσης με χρήση διακομιστή μεσολάβησης εφαρμογής Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="kgremban"/>


# <a name="single-sign-on-with-application-proxy"></a>Καθολικής σύνδεσης με το διακομιστή μεσολάβησης εφαρμογής

Καθολικής σύνδεσης είναι βασικό στοιχείο του διακομιστή μεσολάβησης εφαρμογής Azure AD. Παρέχει την καλύτερη εμπειρία χρήστη με τα ακόλουθα βήματα:

1. Ένας χρήστης πραγματοποιεί είσοδο στο cloud  
2. Όλες οι επικυρώσεις ασφαλείας συμβεί στο cloud (προκαταρκτικό έλεγχο ταυτότητας)  
3. Όταν η αίτηση αποστέλλεται στην εφαρμογή εσωτερικής εγκατάστασης, η γραμμή σύνδεσης διακομιστή μεσολάβησης εφαρμογής μιμείται το χρήστη. Η εφαρμογή παρασκηνίου θεωρεί ότι αυτό είναι τακτικός χρήστης που προέρχονται από μια συσκευή τομέα.

![Διάγραμμα πρόσβαση από τελικού χρήστη, μέσω του διακομιστή μεσολάβησης της εφαρμογής, για να του εταιρικού δικτύου](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)

Διακομιστής μεσολάβησης εφαρμογής Azure AD σάς βοηθά να προσφέρετε μια εμπειρία καθολικής σύνδεσης (SSO) για τους χρήστες σας. Χρησιμοποιήστε τις παρακάτω οδηγίες για να δημοσιεύσετε τις εφαρμογές σας χρησιμοποιώντας SSO:


## <a name="sso-for-on-prem-iwa-apps-using-kcd-with-application-proxy"></a>SSO για τις εφαρμογές IWA εσωτερικής εγκατάστασης με χρήση KCD με το διακομιστή μεσολάβησης εφαρμογής
Μπορείτε να ενεργοποιήσετε καθολικής σύνδεσης για τις εφαρμογές σας χρησιμοποιώντας ενσωματωμένο έλεγχο ταυτότητας των Windows (IWA), δίνοντας γραμμές σύνδεσης διακομιστή μεσολάβησης εφαρμογής δικαιωμάτων στην υπηρεσία καταλόγου Active Directory για τους χρήστες, μίμηση και αποστολή και λήψη διακριτικά για λογαριασμό τους.


### <a name="network-diagram"></a>Διάγραμμα δικτύου

Σε αυτό το διάγραμμα εξηγεί τη ροή όταν ένας χρήστης επιχειρεί να αποκτήσει πρόσβαση σε μια εφαρμογή εσωτερικής εγκατάστασης που χρησιμοποιεί IWA.

![Διάγραμμα ροής του Microsoft AAD ελέγχου ταυτότητας](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. Ο χρήστης εισάγει τη διεύθυνση URL για πρόσβαση στην εφαρμογή εσωτερικής εγκατάστασης μέσω του διακομιστή μεσολάβησης εφαρμογής.
2. Διακομιστής μεσολάβησης ανακατευθύνει την αίτηση Azure AD υπηρεσίες ελέγχου ταυτότητας για να preauthenticate. Σε αυτό το σημείο, Azure AD ισχύει οποιαδήποτε υπάρχει έλεγχος ταυτότητας και εξουσιοδότηση πολιτικές, όπως έλεγχο ταυτότητας πολλαπλών παραγόντων. Εάν ο χρήστης έχει επαληθευτεί, Azure AD δημιουργεί ένα διακριτικό και αποστέλλει ο χρήστης.
3. Ο χρήστης μεταβιβάζει το διακριτικό στο διακομιστή μεσολάβησης εφαρμογής.
4. Διακομιστής μεσολάβησης επικυρώσει το διακριτικό ανακτά την χρήστη κύριο όνομα (UPN) από το και, στη συνέχεια, στέλνει την αίτηση, το UPN και το κύριο όνομα υπηρεσίας (SPN) στη γραμμή σύνδεσης μέσω ασφαλούς καναλιού διπλή με έλεγχο ταυτότητας.
5. Η γραμμή σύνδεσης εκτελεί διαπραγμάτευσης περιορίζεται ανάθεση Kerberos (KCD) με το εσωτερικής εγκατάστασης AD, απομίμηση το χρήστη για να λάβετε ένα διακριτικό Kerberos στην εφαρμογή.
6. Υπηρεσία καταλόγου Active Directory στέλνει το διακριτικό Kerberos για την εφαρμογή στη γραμμή σύνδεσης.
7. Η γραμμή σύνδεσης στέλνει την αρχική αίτηση στο διακομιστή της εφαρμογής, χρησιμοποιώντας το διακριτικό Kerberos που έλαβε από AD.
8. Η εφαρμογή στέλνει την απάντηση στη γραμμή σύνδεσης που επιστρέφεται, στη συνέχεια, με την υπηρεσία του διακομιστή μεσολάβησης εφαρμογής και τέλος στο χρήστη.

### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Πριν ξεκινήσετε την SSO για εφαρμογή του διακομιστή μεσολάβησης, βεβαιωθείτε ότι το περιβάλλον σας είναι έτοιμο με τις παρακάτω ρυθμίσεις και ρυθμίσεις παραμέτρων:

- Έχουν οριστεί τις εφαρμογές σας, όπως εφαρμογές Web του SharePoint, για να χρησιμοποιήσετε την ενσωματωμένη ελέγχου ταυτότητας των Windows. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ενεργοποίηση της υποστήριξης για τον έλεγχο ταυτότητας Kerberos](https://technet.microsoft.com/library/dd759186.aspx)ή για το SharePoint ανατρέξτε στο θέμα [Σχεδιασμός για τον έλεγχο ταυτότητας Kerberos στο SharePoint 2013](https://technet.microsoft.com/library/ee806870.aspx).

- Όλες οι εφαρμογές σας έχουν ονόματα κεφάλαιο υπηρεσίας.

- Διακομιστή στον οποίο εκτελείται η γραμμή σύνδεσης και διακομιστή στον οποίο εκτελείται η εφαρμογή είναι τομέα συμμετέχει και τμήμα του ίδιου τομέα ή στους αξιόπιστους τομείς. Για περισσότερες πληροφορίες σχετικά με συμμετοχή σε τομέα, ανατρέξτε στο θέμα [συμμετοχή σε έναν υπολογιστή σε έναν τομέα](https://technet.microsoft.com/library/dd807102.aspx).

- Διακομιστή στον οποίο εκτελείται η γραμμή σύνδεσης έχει πρόσβαση για ανάγνωση του TokenGroupsGlobalAndUniversal για τους χρήστες. Αυτή είναι μια προεπιλεγμένη ρύθμιση που ενδέχεται να έχουν επηρεάζεται από το περιβάλλον ενίσχυσης ασφαλείας. Λάβετε περισσότερη βοήθεια σχετικά με αυτό με [KB2009157](https://support.microsoft.com/en-us/kb/2009157).

### <a name="active-directory-configuration"></a>Ρύθμιση παραμέτρων του Active Directory

Η ρύθμιση παραμέτρων της υπηρεσίας καταλόγου Active Directory ποικίλλει, ανάλογα με το εάν το σύνδεσης διακομιστή μεσολάβησης εφαρμογής και το δημοσιευμένο διακομιστή είναι στον ίδιο τομέα ή όχι.

#### <a name="connector-and-published-server-in-the-same-domain"></a>Γραμμή σύνδεσης και δημοσιευμένη server στον ίδιο τομέα

1. Στην υπηρεσία καταλόγου Active Directory, επιλέξτε διαδοχικά **Εργαλεία** > **χρήστες και υπολογιστές**.
2. Επιλέξτε διακομιστή στον οποίο εκτελείται η γραμμή σύνδεσης.
3. Κάντε δεξί κλικ και επιλέξτε την εντολή **Ιδιότητες** > **ανάθεση**.
4. Επιλέξτε **αξιόπιστο αυτόν τον υπολογιστή για ανάθεση καθορισμένη μόνο σε υπηρεσίες** και στην περιοχή **τις υπηρεσίες στις οποίες αυτός ο λογαριασμός μπορεί να παρουσιάσει ανάθεση διαπιστευτηρίων**, προσθέστε την τιμή για την ταυτότητα κύριο όνομα Υπηρεσίας του διακομιστή εφαρμογών.
5. Αυτή η δυνατότητα επιτρέπει τη γραμμή σύνδεσης διακομιστή μεσολάβησης εφαρμογής μίμηση χρήστες στο AD σύμφωνα με τις εφαρμογές που ορίζονται από το στη λίστα.

![Στιγμιότυπο οθόνης παραθύρου ιδιοτήτων SVR γραμμής σύνδεσης](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-published-server-in-different-domains"></a>Γραμμή σύνδεσης και δημοσιευμένη διακομιστή σε διαφορετικούς τομείς

1. Για μια λίστα με τις προϋποθέσεις για την εργασία με KCD σε όλους τους τομείς, ανατρέξτε στο θέμα [Ανάθεση Kerberos περιορίζεται σε όλους τους τομείς](https://technet.microsoft.com/library/hh831477.aspx).
2. Στο Windows 2012 R2, χρησιμοποιήστε το `principalsallowedtodelegateto` ιδιότητα στο διακομιστή σύνδεσης για να ενεργοποιήσετε το διακομιστή μεσολάβησης εφαρμογής στον πληρεξούσιο για το διακομιστή σύνδεσης, όπου είναι δημοσιευμένο διακομιστή `sharepointserviceaccount` και εκχώρηση ο διακομιστής είναι `connectormachineaccount`.

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount`μπορεί να είναι ο λογαριασμός υπολογιστή SPS ή ένα λογαριασμό υπηρεσίας με το οποίο εκτελείται το χώρο συγκέντρωσης εφαρμογών ΥΦΠ.


### <a name="azure-classic-portal-configuration"></a>Azure κλασική ρύθμισης παραμέτρων πύλης

1. Δημοσίευση εφαρμογής σας σύμφωνα με τις οδηγίες που περιγράφονται σε [δημοσίευση εφαρμογών με το διακομιστή μεσολάβησης εφαρμογής](active-directory-application-proxy-publish.md). Βεβαιωθείτε ότι έχετε επιλέξει **Azure Active Directory** ως **Μέθοδο προκαταρκτικό έλεγχο ταυτότητας**.
2. Μετά την εφαρμογή σας εμφανίζεται στη λίστα των εφαρμογών, επιλέξτε την και κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**.
3. Στην ενότητα **Ιδιότητες**, ορίστε **Εσωτερικό μεθόδου ελέγχου ταυτότητας** σε **Ενσωματωμένο έλεγχο ταυτότητας των Windows**.  
  ![Ρύθμιση παραμέτρων για προχωρημένους εφαρμογής](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  
4. Εισαγάγετε το **Κύριο όνομα εσωτερικού εφαρμογής Υπηρεσίας** του διακομιστή εφαρμογών. Σε αυτό το παράδειγμα, το κύριο όνομα Υπηρεσίας για μας δημοσιευμένη εφαρμογή είναι http/lob.contoso.com.  

>[AZURE.IMPORTANT] Εάν το UPN εσωτερικής εγκατάστασης και του UPN στην υπηρεσία καταλόγου Azure Active Directory δεν είναι πανομοιότυπες, θα πρέπει να ρυθμίσετε τις παραμέτρους την [ταυτότητα σύνδεσης με ανάθεση](#delegated-login-identity) για προκαταρκτικού ελέγχου ταυτότητας για να εργαστείτε.

|  |  |
| --- | --- |
| Μέθοδος εσωτερικού ελέγχου ταυτότητας | Εάν χρησιμοποιείτε το Azure AD για προκαταρκτικό έλεγχο ταυτότητας, μπορείτε να ορίσετε μια μέθοδο εσωτερικού ελέγχου ταυτότητας για να ενεργοποιήσετε τους χρήστες σας να επωφεληθείτε από καθολικής σύνδεσης (SSO) σε αυτήν την εφαρμογή. <br><br> Εάν η εφαρμογή σας χρησιμοποιεί IWA και μπορείτε να ρυθμίσετε τις παραμέτρους του Kerberos περιορίζεται ανάθεση (KCD) για να ενεργοποιήσετε την SSO για αυτήν την εφαρμογή, επιλέξτε **Ενσωματωμένο έλεγχο ταυτότητας των Windows** (IWA). Εφαρμογές που χρησιμοποιούν IWA πρέπει να είναι διαμορφωμένη χρησιμοποιώντας KCD, διαφορετικά διακομιστή μεσολάβησης εφαρμογής δεν θα μπορούν να δημοσιεύσουν αυτές τις εφαρμογές. <br><br> Επιλέξτε " **κανένα** " Εάν η εφαρμογή σας δεν χρησιμοποιεί IWA. |
| Εσωτερική εφαρμογή κύριο όνομα Υπηρεσίας | Αυτό είναι το όνομα κεφάλαιο υπηρεσίας (κύριο όνομα Υπηρεσίας) από την εσωτερική εφαρμογή, όπως έχει ρυθμιστεί στο το εσωτερικής εγκατάστασης Azure AD. Το κύριο όνομα Υπηρεσίας χρησιμοποιείται από τη γραμμή σύνδεσης διακομιστή μεσολάβησης εφαρμογής για τη λήψη διακριτικά Kerberos για την εφαρμογή χρησιμοποιώντας KCD. |


## <a name="sso-for-non-windows-apps"></a>SSO για τις εφαρμογές εκτός των Windows
Η ροή ανάθεση Kerberos στο διακομιστή μεσολάβησης εφαρμογής Azure AD ξεκινά όταν Azure AD πραγματοποιεί έλεγχο ταυτότητας χρήστη στο cloud. Όταν η αίτηση φτάσει στην εσωτερική εγκατάσταση, το Azure Connector διακομιστή μεσολάβησης εφαρμογής AD προβλημάτων του δελτίου Kerberos εκ μέρους του χρήστη με αλληλεπίδραση με τοπική υπηρεσία καταλόγου Active Directory. Αυτή η διαδικασία είναι γνωστή ως ανάθεση του Kerberos περιορίζεται (KCD). Στην επόμενη φάση, αποστέλλεται μια αίτηση στην εφαρμογή υπολογιστή στο παρασκήνιο με αυτό δελτίο Kerberos. Υπάρχουν κάποια από τα πρωτόκολλα που καθορίζουν πώς μπορείτε να στείλετε αυτές τις αιτήσεις. Οι περισσότεροι διακομιστές μη Windows αναμενόμενο Negotiate/SPNego που υποστηρίζεται τώρα στο διακομιστή μεσολάβησης εφαρμογής Azure AD.

![Διάγραμμα SSO μη-Windows](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### <a name="delegated-login-identity"></a>Ανάθεση login ταυτότητας
Ανάθεση login ταυτότητα σάς βοηθά να χειριστείτε δύο διαφορετικούς login σενάρια:

- Μη-εφαρμογές των Windows που συνήθως λήψη ταυτότητα χρήστη στη φόρμα όνομα χρήστη ή όνομα λογαριασμού SAM, όχι μια διεύθυνση ηλεκτρονικού ταχυδρομείου (username@domain).
- Ρυθμίσεις παραμέτρων εναλλακτικών login όπου το UPN στο Azure AD και το UPN του καταλόγου Active Directory εσωτερικής εγκατάστασης είναι διαφορετικό.

Με το διακομιστή μεσολάβησης εφαρμογής, μπορείτε να επιλέξετε ποια ταυτότητα για να χρησιμοποιήσετε για τη λήψη του δελτίου Kerberos. Αυτή η ρύθμιση είναι ανά εφαρμογή. Ορισμένες από αυτές τις επιλογές είναι κατάλληλη για συστήματα που να μην γίνει αποδεκτή μορφή διεύθυνσης ηλεκτρονικού ταχυδρομείου, άλλα άτομα έχουν σχεδιαστεί για το εναλλακτικό login.

![Στιγμιότυπο οθόνης της παραμέτρου ταυτότητας ανάθεση σύνδεσης](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Εάν χρησιμοποιείται η ανάθεση login ταυτότητα, η τιμή ενδέχεται να μην είναι μοναδικά για όλες τις τομέων ή συμπλεγμάτων δομών στην εταιρεία σας. Μπορείτε να αποφύγετε αυτό το ζήτημα, δημοσιεύοντάς αυτές τις εφαρμογές δύο φορές με δύο διαφορετικές ομάδες γραμμή σύνδεσης. Δεδομένου ότι κάθε εφαρμογή έχει ένα ακροατήριο διαφορετικό χρήστη, μπορείτε να συνδέσετε τις γραμμές σύνδεσης σε έναν διαφορετικό τομέα.


## <a name="working-with-sso-when-on-premises-and-cloud-identities-are-not-identical"></a>Εργασία με SSO όταν εσωτερικής εγκατάστασης και cloud ταυτότητες δεν είναι πανομοιότυπες
Εκτός και αν έχει ρυθμιστεί διαφορετικά, διακομιστή μεσολάβησης εφαρμογής προϋποθέτει ότι οι χρήστες έχουν ακριβώς την ίδια ταυτότητα στο cloud και εσωτερικής εγκατάστασης. Μπορείτε να ρυθμίσετε τις παραμέτρους, για κάθε εφαρμογή, ποια ταυτότητα θα χρησιμοποιηθεί κατά την εκτέλεση καθολικής σύνδεσης.  

Αυτή η δυνατότητα επιτρέπει πολλές εταιρείες που έχουν διαφορετικές ταυτότητες εσωτερικής εγκατάστασης και cloud να έχουν SSO από το cloud εσωτερικής εγκατάστασης εφαρμογών χωρίς να απαιτείται τους χρήστες για να εισαγάγετε διαφορετικές τα ονόματα των χρηστών και τους κωδικούς πρόσβασης. Σε αυτούς περιλαμβάνονται οργανισμοί που:

- Έχετε πολλούς τομείς εσωτερικά (joe@us.contoso.com, joe@eu.contoso.com) και έναν τομέα μόνο στο cloud (joe@contoso.com).

- Έχετε το όνομα του τομέα που δεν υποστηρίζει δρομολόγηση εσωτερικά (joe@contoso.usa) και ένα νομικό στο cloud.

- Μην χρησιμοποιείτε εσωτερικά ονόματα τομέα (Αλέξανδρος)

- Χρησιμοποιήστε διαφορετικό ψευδώνυμα εσωτερικής εγκατάστασης και στο cloud. Π.χ. joe-johns@contoso.comσύγκριση.joej@contoso.com  

Αυτό θα σας βοηθήσει επίσης με τις εφαρμογές που να μην γίνεται αποδοχή διευθύνσεις με τη μορφή της διεύθυνσης ηλεκτρονικού ταχυδρομείου, η οποία είναι ένα πολύ συνηθισμένο σενάριο για τους διακομιστές παρασκηνίου εκτός των Windows.


### <a name="setting-sso-for-different-cloud-and-on-prem-identities"></a>Ρύθμιση SSO για διαφορετικές ταυτότητες cloud και εσωτερικής εγκατάστασης

1. Ρύθμιση παραμέτρων Azure AD Connect, ώστε η κύρια ταυτότητα θα είναι η διεύθυνση ηλεκτρονικού ταχυδρομείου (αλληλογραφία). Αυτό γίνεται ως μέρος της διαδικασίας προσαρμογή, αλλάζοντας το πεδίο **Κύριο όνομα χρήστη** στις ρυθμίσεις συγχρονισμού. Σημειώστε ότι αυτές οι ρυθμίσεις επίσης καθορίζουν πώς οι χρήστες συνδεθείτε στο Office 365, Windows10 συσκευές, και άλλες εφαρμογές που χρησιμοποιούν Azure AD ως τους χώρο αποθήκευσης ταυτότητα.  
  ![Εντοπισμός στιγμιότυπο οθόνης χρήστες - αναπτυσσόμενη λίστα κύριο όνομα χρήστη](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Στις ρυθμίσεις παραμέτρων εφαρμογής για την εφαρμογή που θέλετε να τροποποιήσετε, επιλέξτε την **Ταυτότητα σύνδεσης με ανάθεση** θα χρησιμοποιηθεί:
  - Κύριο όνομα χρήστη:joe@contoso.com  
  - Εναλλακτικό κύριο όνομα χρήστη:joed@contoso.local  
  - Όνομα χρήστη μέρος κύριο όνομα χρήστη: Αλέξανδρος  
  - Όνομα χρήστη μέρος εναλλακτικό κύριο όνομα χρήστη: joed  
  - Όνομα λογαριασμού SAM εσωτερικής εγκατάστασης: ανάλογα με ρύθμιση παραμέτρων ελεγκτή τομέα εσωτερικής εγκατάστασης

  ![Στιγμιότυπο οθόνης αναπτυσσόμενο μενού ταυτότητας ανάθεση σύνδεσης](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)  

### <a name="troubleshooting-sso-for-different-identities"></a>Αντιμετώπιση προβλημάτων SSO για διαφορετικές ταυτότητες
Εάν υπάρχει ένα σφάλμα κατά τη διαδικασία SSO θα εμφανίζεται στο αρχείο καταγραφής συμβάντων υπολογιστή σύνδεσης όπως εξηγείται στο θέμα [Αντιμετώπιση προβλημάτων](active-directory-application-proxy-troubleshoot.md).
Ωστόσο, σε ορισμένες περιπτώσεις, η αίτηση θα με επιτυχία σταλεί στην εφαρμογή υπολογιστή στο παρασκήνιο ενώ αυτή η εφαρμογή θα απάντηση στις διάφορες άλλες απαντήσεις HTTP. Αντιμετώπιση προβλημάτων σε αυτές τις περιπτώσεις θα πρέπει να ξεκινήσετε εξετάζοντας αριθμός συμβάντος 24029 στον υπολογιστή σύνδεσης στο αρχείο καταγραφής συμβάντων της περιόδου λειτουργίας διακομιστή μεσολάβησης εφαρμογής. Η ταυτότητα χρήστη που χρησιμοποιήθηκε για ανάθεση θα εμφανίζεται στο πεδίο "χρήστης" μέσα σε τις λεπτομέρειες του συμβάντος. Για να ενεργοποιήσετε την περίοδο λειτουργίας καταγραφής, επιλέξτε **Εμφάνιση αναλυτικών και αρχεία καταγραφής εντοπισμού σφαλμάτων** σε περίπτωση προβολής προβολή μενού.


## <a name="see-also"></a>Δείτε επίσης

- [Δημοσίευση εφαρμογών με το διακομιστή μεσολάβησης εφαρμογής](active-directory-application-proxy-publish.md)
- [Αντιμετώπιση προβλημάτων που αντιμετωπίζετε με το διακομιστή μεσολάβησης εφαρμογής](active-directory-application-proxy-troubleshoot.md)
- [Εργασία με εφαρμογές που αναγνωρίζουν αξιώσεων](active-directory-application-proxy-claims-aware-apps.md)
- [Ενεργοποίηση της πρόσβασης υπό όρους](active-directory-application-proxy-conditional-access.md)

Για τις πιο πρόσφατες ειδήσεις και ενημερώσεις, ανατρέξτε στο θέμα το [ιστολόγιο του διακομιστή μεσολάβησης εφαρμογής](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg
