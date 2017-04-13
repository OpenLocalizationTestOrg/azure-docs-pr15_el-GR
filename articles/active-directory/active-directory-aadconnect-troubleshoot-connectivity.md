<properties
    pageTitle="Azure AD Connect: Αντιμετώπιση προβλημάτων συνδεσιμότητας | Microsoft Azure"
    description="Εξηγεί τον τρόπο για την αντιμετώπιση προβλημάτων συνδεσιμότητας με το Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Αντιμετώπιση προβλημάτων συνδεσιμότητας με το Azure AD Connect
Σε αυτό το άρθρο εξηγεί πώς λειτουργεί η συνδεσιμότητα μεταξύ Azure AD Connect και Azure AD και αντιμετώπιση προβλημάτων συνδεσιμότητας. Αυτά τα θέματα είναι πιο πιθανό να είναι ορατό σε ένα περιβάλλον με ένα διακομιστή μεσολάβησης.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Αντιμετώπιση προβλημάτων συνδεσιμότητας στον Οδηγό εγκατάστασης
Azure AD Connect χρησιμοποιεί Σύγχρονος έλεγχος ταυτότητας (χρησιμοποιώντας τη βιβλιοθήκη ADAL) για τον έλεγχο ταυτότητας. Ο Οδηγός εγκατάστασης και ο μηχανισμός συγχρονισμού proper απαιτούν machine.config να ρυθμιστούν σωστά, επειδή πρόκειται για εφαρμογές .NET.

Σε αυτό το άρθρο θα δείξουμε πώς Fabrikam συνδέεται με Azure AD μέσω του διακομιστή μεσολάβησης. Ο διακομιστής μεσολάβησης ονομάζεται fabrikamproxy και χρησιμοποιεί θύρα 8080.

Πρώτα πρέπει να βεβαιωθείτε ότι έχει ρυθμιστεί σωστά [**machine.config**](active-directory-aadconnect-prerequisites.md#connectivity) .  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

>[AZURE.NOTE]
Σε ορισμένες ιστολόγια δεν ανήκουν στη Microsoft το τεκμηριώνονται ότι θα πρέπει να γίνουν αλλαγές σε miiserver.exe.config αντί για αυτό. Ωστόσο, αυτό το αρχείο έχει αντικατασταθεί κατά την αναβάθμιση της κάθε συνεπώς, ακόμη και αν λειτουργεί κατά την αρχική εγκατάσταση, το σύστημα σταματά να λειτουργεί κατά την αναβάθμιση της πρώτης. Για αυτόν το λόγο συνιστάται η ενημέρωση machine.config αντί για αυτό.

Ο διακομιστής μεσολάβησης πρέπει να έχετε τις απαιτούμενες διευθύνσεις URL ανοίξει. Λίστα επίσημη τεκμηριώνονται στο [Office 365 URL και περιοχές διευθύνσεων IP ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Από τα εξής, ο παρακάτω πίνακας είναι το απόλυτη ελάχιστο για να μπορέσετε να συνδεθείτε με Azure AD καθόλου. Αυτή η λίστα δεν περιλαμβάνει οποιαδήποτε προαιρετικές δυνατότητες, όπως αντιγραφής τον κωδικό πρόσβασης ή υγείας σύνδεση Azure AD. Το τεκμηριώνονται εδώ για να σας βοηθήσουν στην αντιμετώπιση προβλημάτων για την αρχική ρύθμιση παραμέτρων.

ΔΙΕΎΘΥΝΣΗ URL | Θύρα | Περιγραφή
---- | ---- | ----
mscrl.Microsoft.com | HTTP/80 | Χρησιμοποιείται για τη λήψη λίστες CRL.
\*. verisign.com | HTTP/80 | Χρησιμοποιείται για τη λήψη λίστες CRL.
\*. entrust.com | HTTP/80 | Χρησιμοποιείται για τη λήψη λίστες CRL για MFA.
\*. των windows.net | HTTPS/443 | Χρησιμοποιείται για να εισέλθετε στο Azure AD.
Secure.aadcdn.microsoftonline p.com | HTTPS/443 | Χρησιμοποιείται για MFA.
\*. microsoftonline.com | HTTPS/443 | Χρησιμοποιείται για να ρυθμίσετε τις παραμέτρους του καταλόγου Azure AD και εισαγωγή/εξαγωγή δεδομένων.

## <a name="errors-in-the-wizard"></a>Σφάλματα του οδηγού
Ο Οδηγός εγκατάστασης χρησιμοποιεί δύο διαφορετικά πλαίσια ασφαλείας. Στη σελίδα **σύνδεση με το Azure AD** χρησιμοποιεί αυτήν τη στιγμή συνδεδεμένος χρήστη. Στη σελίδα **Ρύθμιση παραμέτρων** του αλλάζει για το [λογαριασμό εκτελεί την υπηρεσία του μηχανισμού συγχρονισμού](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts). Τις ρυθμίσεις παραμέτρων διακομιστή μεσολάβησης που κάνουμε είναι καθολικές για τον υπολογιστή, ώστε να εάν υπάρχει κάποιο πρόβλημα, το πρόβλημα θα πιο πιθανώς να εμφανίζονται ήδη στη σελίδα **σύνδεση με το Azure AD** στον οδηγό.

Αυτά είναι τα πιο συνηθισμένα σφάλματα, θα δείτε στον Οδηγό εγκατάστασης.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Ο Οδηγός εγκατάστασης δεν έχει ρυθμιστεί σωστά
Αυτό το σφάλμα εμφανίζεται όταν ο ίδιος ο οδηγός δεν μπορούν να συνδεθούν στο διακομιστή μεσολάβησης.
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

- Εάν βλέπετε αυτό, επαληθεύστε την [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) έχει ρυθμιστεί σωστά.
- Εάν που φαίνεται σωστή, ακολουθήστε τα βήματα στο θέμα [Επαλήθευση συνδεσιμότητας διακομιστή μεσολάβησης](#verify-proxy-connectivity) για να δείτε εάν υπάρχει το ζήτημα εκτός καθώς και τον οδηγό.

### <a name="the-mfa-endpoint-cannot-be-reached"></a>Δεν μπορείτε να αποκτήσετε πρόσβαση το τελικό σημείο MFA
Αυτό το σφάλμα εμφανίζεται εάν το τελικό σημείο **https://secure.aadcdn.microsoftonline-p.com** δεν μπορείτε να αποκτήσετε πρόσβαση και να σας καθολικός διαχειριστής έχει ενεργοποιηθεί MFA.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

- Εάν βλέπετε αυτό, βεβαιωθείτε ότι το τελικό σημείο secure.aadcdn.microsoftonline-p.com έχει προστεθεί στο διακομιστή μεσολάβησης.

### <a name="the-password-cannot-be-verified"></a>Δεν είναι δυνατό να έχει επαληθευτεί για τον κωδικό πρόσβασης
Εάν ο Οδηγός εγκατάστασης είναι επιτυχής κατά τη σύνδεση Azure AD, αλλά ίδιος ο κωδικός πρόσβασης δεν μπορεί να επαληθευτεί, θα δείτε αυτό: ![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

- Είναι ο κωδικός πρόσβασης ένα προσωρινό κωδικό πρόσβασης και πρέπει να αλλάξει; Είναι στην πραγματικότητα τον σωστό κωδικό πρόσβασης; Δοκιμάστε να εισέλθετε στο https://login.microsoftonline.com (σε άλλον υπολογιστή από το διακομιστή Azure AD Connect) και να επαληθεύσετε το λογαριασμό δεν μπορεί να χρησιμοποιηθεί.

### <a name="verify-proxy-connectivity"></a>Επαλήθευση συνδεσιμότητας διακομιστή μεσολάβησης
Για να επαληθεύσετε εάν ο διακομιστής Azure AD Connect έχει πραγματική τη σύνδεση με το διακομιστή μεσολάβησης και Internet, θα χρησιμοποιήσουμε ορισμένες PowerShell για να δείτε εάν ο διακομιστής μεσολάβησης επιτρέποντάς αιτήσεις web ή όχι. Στη γραμμή εντολών του PowerShell, εκτελέστε `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Τεχνικά η πρώτη κλήση είναι να https://login.microsoftonline.com και αυτό θα λειτουργεί επίσης, αλλά το άλλο URI είναι πιο γρήγορο να απαντήσετε.)

PowerShell θα χρησιμοποιήσει τη ρύθμιση παραμέτρων του machine.config να επικοινωνήσετε με το διακομιστή μεσολάβησης. Οι ρυθμίσεις στο winhttp/netsh δεν θα πρέπει να επηρεάσουν αυτών των cmdlet.

Εάν ο διακομιστής μεσολάβησης έχει ρυθμιστεί σωστά, θα πρέπει να λάβετε μια κατάσταση επιτυχίας: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Εάν λάβετε **δεν είναι δυνατή η σύνδεση με τον απομακρυσμένο διακομιστή** , στη συνέχεια, PowerShell προσπαθεί να κάνετε μια άμεση κλήση χωρίς να χρησιμοποιείτε το διακομιστή μεσολάβησης ή DNS δεν έχει ρυθμιστεί σωστά. Βεβαιωθείτε ότι **το αρχείο Machine.config** έχει ρυθμιστεί σωστά.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Εάν ο διακομιστής μεσολάβησης δεν έχει ρυθμιστεί σωστά, θα σας θα λάβετε ένα σφάλμα: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

Σφάλμα | Κείμενο σφάλματος | Σχόλιο
---- | ---- | ---- |
403 | Δεν επιτρέπεται | Δεν έχει ανοίξει το διακομιστή μεσολάβησης για τη διεύθυνση URL που ζητήθηκε. Τρόπος για να αναθεωρήσετε τη ρύθμιση παραμέτρων διακομιστή μεσολάβησης και βεβαιωθείτε ότι έχουν ανοιχτεί του [διευθύνσεις URL](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) .
407 | Απαιτείται έλεγχος ταυτότητας διακομιστή μεσολάβησης | Ο διακομιστής μεσολάβησης απαιτείται είσοδος και καμία παρέχεται. Εάν ο διακομιστής μεσολάβησης απαιτεί έλεγχο ταυτότητας, βεβαιωθείτε ότι έχετε αυτό έχει ρυθμιστεί στο το machine.config. Επίσης, βεβαιωθείτε ότι χρησιμοποιείτε λογαριασμοί τομέα για το χρήστη εκτέλεση του οδηγού, καθώς και για το λογαριασμό της υπηρεσίας.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Το μοτίβο επικοινωνίας μεταξύ Azure AD Connect και Azure AD
Εάν έχετε ακολουθήσει όλα αυτά τα βήματα παραπάνω και ακόμα δεν μπορεί να συνδεθεί να ξεκινήσετε σε αυτό το σημείο εξετάζοντας αρχεία καταγραφής δικτύου. Αυτή η ενότητα είναι τεκμηρίωση ένα μοτίβο κανονικής και επιτυχής συνδεσιμότητας. Επίσης, αυτό καταχώρηση κοινές herrings κόκκινο που μπορεί να αγνοηθεί αν διαβάζετε τα αρχεία καταγραφής του δικτύου.

- Θα υπάρξει κλήσεων στο https://dc.services.visualstudio.com. Δεν απαιτείται να αυτό είναι ανοικτό στο διακομιστή μεσολάβησης για την εγκατάσταση για να ολοκληρωθεί με επιτυχία και αυτές μπορεί να αγνοηθεί.
- Θα δείτε ότι επίλυσης dns θα εμφανιστούν την πραγματική κεντρικών υπολογιστών να ενσωματωθεί το nsatc.net χώρο ονομάτων DNS και άλλα πεδία ονομάτων δεν στην περιοχή microsoftonline.com. Ωστόσο, δεν θα υπάρχει αιτημάτων υπηρεσίας web σε τα ονόματα των διακομιστών πραγματική και δεν χρειάζεται για να τις προσθέσετε στο διακομιστή μεσολάβησης.
- Τα τελικά σημεία adminwebservice και provisioningapi (δείτε παρακάτω στα αρχεία καταγραφής) είναι τελικά σημεία εντοπισμού και χρησιμοποιείται για να βρείτε το πραγματικό τελικό σημείο για να χρησιμοποιήσετε και θα είναι διέφεραν ανάλογα με την περιοχή σας.

### <a name="reference-proxy-logs"></a>Αρχεία καταγραφής διακομιστή μεσολάβησης αναφοράς
Ακολουθεί μια ένδειξη από ένα αρχείο καταγραφής πραγματική διακομιστή μεσολάβησης και η σελίδα του Οδηγού εγκατάστασης από το σημείο όπου λήφθηκε (διπλότυπες καταχωρήσεις στο ίδιο τελικό σημείο έχουν καταργηθεί). Αυτό μπορεί να χρησιμοποιηθεί ως αναφορά για τη δική σας αρχεία καταγραφής δικτύου και διακομιστή μεσολάβησης. Τα τελικά σημεία πραγματική μπορεί να είναι διαφορετικά στο περιβάλλον σας (ιδίως εκείνα *πλάγια γραφή*).

**Σύνδεση με Azure AD**

Ώρα | ΔΙΕΎΘΥΝΣΗ URL
--- | ---
1/11/2016 8:31 | Connect://Login.microsoftonline.com:443
1/11/2016 8:31 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:32 | σύνδεση: / /*bba800 αγκύρωσης*. microsoftonline.com:443
1/11/2016 8:32 | Connect://Login.microsoftonline.com:443
1/11/2016 8:33 | Connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:33 | σύνδεση: / /*bwsc02 μεταγωγής*. microsoftonline.com:443

**Ρύθμιση παραμέτρων**

Ώρα | ΔΙΕΎΘΥΝΣΗ URL
--- | ---
1/11/2016 8:43 | Connect://Login.microsoftonline.com:443
1/11/2016 8:43 | σύνδεση: / /*bba800 αγκύρωσης*. microsoftonline.com:443
1/11/2016 8:43 | Connect://Login.microsoftonline.com:443
1/11/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:44 | σύνδεση: / /*bba900 αγκύρωσης*. microsoftonline.com:443
1/11/2016 8:44 | Connect://Login.microsoftonline.com:443
1/11/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:44 | σύνδεση: / /*bba800 αγκύρωσης*. microsoftonline.com:443
1/11/2016 8:44 | Connect://Login.microsoftonline.com:443
1/11/2016 8:46 | Connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:46 | σύνδεση: / /*bwsc02 μεταγωγής*. microsoftonline.com:443

**Αρχικό συγχρονισμού**

Ώρα | ΔΙΕΎΘΥΝΣΗ URL
--- | ---
1/11/2016 8:48 | Connect://Login.Windows.NET:443
1/11/2016 8:49 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:49 | σύνδεση: / /*bba900 αγκύρωσης*. microsoftonline.com:443
1/11/2016 8:49 | σύνδεση: / /*bba800 αγκύρωσης*. microsoftonline.com:443

## <a name="authentication-errors"></a>Σφάλματα ελέγχου ταυτότητας
Αυτή η ενότητα περιγράφει σφάλματα που μπορεί να επιστραφεί από ADAL (η βιβλιοθήκη ελέγχου ταυτότητας που χρησιμοποιείται από το Azure AD Connect) και PowerShell. Το σφάλμα επεξήγηση θα σας βοηθήσουν να κατανοήσετε στα επόμενα βήματα.

### <a name="invalid-grant"></a>Μη έγκυροι εκχώρηση
Μη έγκυρο όνομα χρήστη ή τον κωδικό πρόσβασης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [δεν είναι δυνατό να έχει επαληθευτεί για τον κωδικό πρόσβασης](#the-password-cannot-be-verified) .

### <a name="unknown-user-type"></a>Τύπος άγνωστη χρήστη
Καταλόγου Azure AD σας δεν είναι δυνατό να βρεθούν ή να επιλυθεί. Ίσως προσπαθείτε να συνδεθείτε με ένα όνομα χρήστη σε ένα μη επαληθευμένη τομέα;

### <a name="user-realm-discovery-failed"></a>Εντοπισμού τομέα χρήστη απέτυχε
Θέματα ρύθμισης παραμέτρων δικτύου ή διακομιστή μεσολάβησης. Το δίκτυο δεν μπορούν να καλούν, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων συνδεσιμότητας στον Οδηγό εγκατάστασης](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Λήξη κωδικού πρόσβασης χρήστη
Τα διαπιστευτήριά σας έχει λήξει. Αλλαγή κωδικού πρόσβασης.

### <a name="authorizationfailure"></a>AuthorizationFailure
Άγνωστη το ζήτημα.

### <a name="authentication-cancelled"></a>Έλεγχος ταυτότητας ακυρώθηκε
Ακυρώθηκε η πρόκληση έλεγχο ταυτότητας πολλών παραγόντων (MFA).

### <a name="connecttomsonline"></a>ConnectToMSOnline
Έλεγχος ταυτότητας ολοκληρώθηκε με επιτυχία, αλλά Azure AD PowerShell περιλαμβάνει ένα πρόβλημα κατά τον έλεγχο ταυτότητας.

### <a name="azurerolemissing"></a>AzureRoleMissing
Έλεγχος ταυτότητας ολοκληρώθηκε με επιτυχία. Δεν είστε καθολικός διαχειριστής.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Έλεγχος ταυτότητας ολοκληρώθηκε με επιτυχία. Έχει ενεργοποιηθεί η Διαχείριση ταυτοτήτων Προνομιούχους και αυτήν τη στιγμή δεν είστε καθολικός διαχειριστής. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος διαχείρισης των ταυτοτήτων Προνομιούχους](active-directory-privileged-identity-management-getting-started.md) .

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Έλεγχος ταυτότητας ολοκληρώθηκε με επιτυχία. Δεν ήταν δυνατή η ανάκτηση των στοιχείων της εταιρείας από το Azure AD.

### <a name="retrievedomains"></a>RetrieveDomains
Έλεγχος ταυτότητας ολοκληρώθηκε με επιτυχία. Δεν ήταν δυνατή η ανάκτηση πληροφοριών τομέα από το Azure AD.

## <a name="troubleshooting-steps-for-previous-releases"></a>Βήματα αντιμετώπισης προβλημάτων για προηγούμενες εκδόσεις.
Εκδόσεις ξεκινώντας με αριθμό build 1.1.105.0 (κυκλοφορήσει Φεβρουαρίου 2016) καταργήθηκε του Βοηθού εισόδου. Αυτή η ενότητα και τη ρύθμιση παραμέτρων δεν θα πρέπει να απαιτείται, αλλά παραμένει διαθέσιμη ως αναφορά.

Για το καθολικής σύνδεσης στο Βοηθό για να εργαστείτε, πρέπει να ρυθμιστεί winhttp. Αυτό μπορεί να γίνει με [**netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>Το Βοηθό εισόδου δεν έχει ρυθμιστεί σωστά
Αυτό το σφάλμα εμφανίζεται όταν το Βοηθό εισόδου δεν μπορούν να συνδεθούν στο διακομιστή μεσολάβησης ή του διακομιστή μεσολάβησης δεν επιτρέπει την αίτηση.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

- Εάν βλέπετε αυτό, αναζητήστε κατά τη ρύθμιση παραμέτρων διακομιστή μεσολάβησης στο [netsh](active-directory-aadconnect-prerequisites.md#connectivity) και βεβαιωθείτε ότι είναι σωστή.
![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
- Εάν που φαίνεται σωστή, ακολουθήστε τα βήματα στο θέμα [Επαλήθευση συνδεσιμότητας διακομιστή μεσολάβησης](#verify-proxy-connectivity) για να δείτε εάν υπάρχει το ζήτημα εκτός καθώς και τον οδηγό.

## <a name="next-steps"></a>Επόμενα βήματα
Μάθετε περισσότερα σχετικά με την [ενσωμάτωση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).
