<properties
    pageTitle="Active Directory Federation υπηρεσίες διαχείρισης και προσαρμογής με Azure AD Connect | Microsoft Azure"
    description="Διαχείριση AD FS με Azure AD Connect και προσαρμογή του χρήστη AD FS εμπειρία εισόδου με Azure AD Connect και PowerShell."
    keywords="AD FS, ADFS, AD FS διαχείρισης, AAD σύνδεση, σύνδεση, εισόδου, AD FS προσαρμογής, επιδιορθώστε αξιοπιστίας, O365, ομοσπονδίας, υπηρεσία αξιοπιστίας"
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
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Active Directory Federation Services διαχείρισης και προσαρμογής με Azure AD Connect

Σε αυτό το άρθρο εργασίες λεπτομερειών που σχετίζονται με υπηρεσίες Active Directory Federation Services (AD FS) που μπορούν να εκτελεστούν χρησιμοποιώντας το Microsoft Azure Active Directory σύνδεση, καθώς και άλλες κοινές εργασίες AD FS που ενδεχομένως να απαιτούνται για μια πλήρη ρύθμιση παραμέτρων για μια συστοιχία AD FS.

| Το θέμα | Τι να καλύπτει |
|:------|:-------------|
|**AD FS διαχείρισης**|
|[Επιδιορθώστε την αξιοπιστία](#repairthetrust)| Η επιδιόρθωση της αξιοπιστίας Ομοσπονδία με το Office 365 |
|[Προσθέστε ένα διακομιστή AD FS](#addadfsserver) | Επέκταση της συστοιχίας AD FS με μια πρόσθετη διακομιστή AD FS|
|[Προσθέστε ένα διακομιστή μεσολάβησης εφαρμογών web AD FS](#addwapserver) | Επέκταση της συστοιχίας AD FS με μια πρόσθετη διακομιστή WAP|
|[Προσθέστε έναν ομόσπονδο τομέα](#addfeddomain)| Προσθήκη έναν ομόσπονδο τομέα|
| **AD FS προσαρμογής**|
|[Προσθέστε ένα προσαρμοσμένο λογότυπο ή εικόνα](#customlogo)| Προσαρμογή μιας AD FS εισόδου σελίδας με το λογότυπο της εταιρείας και εικόνα |
|[Προσθέστε μια περιγραφή εισόδου](#addsignindescription) | Προσθήκη μια περιγραφή στη σελίδα εισόδου |
|[Τροποποίηση AD FS διεκδίκηση κανόνων](#modclaims) | Τροποποίηση AD FS αξιώσεις για διάφορα Ομοσπονδία σενάρια |

## <a name="ad-fs-management"></a>AD FS διαχείρισης

Azure AD Connect παρέχει διάφορες εργασίες που σχετίζονται με AD FS που μπορούν να εκτελεστούν χρησιμοποιώντας τον Οδηγό Azure AD Connect με παρέμβαση του χρήστη ελάχιστους. Αφού ολοκληρώσετε την εγκατάσταση του Azure AD Connect, εκτελώντας τον οδηγό, μπορείτε να εκτελέσετε τον οδηγό ξανά για να εκτελέσετε πρόσθετες εργασίες.

### Επιδιορθώστε την αξιοπιστία<a name=repairthetrust></a>

Azure AD Connect να ελέγξετε για την τρέχουσα εύρυθμη λειτουργία της σχέσης αξιοπιστίας AD FS και Azure Active Directory και να λαμβάνουν κατάλληλο ενέργειες για να επιδιορθώσετε την αξιοπιστία. Ακολουθήστε τα παρακάτω βήματα για να επιδιορθώσετε το Azure AD και αξιοπιστία AD FS.

1. Επιλέξτε **επιδιόρθωση AAD και ADFS αξιοπιστίας** από τη λίστα των πρόσθετων εργασιών.
![Επιδιόρθωση AAD και ADFS αξιοπιστίας](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. Στη σελίδα **σύνδεση με το Azure AD** , δώστε τα διαπιστευτήριά σας καθολικός διαχειριστής για Azure AD και κάντε κλικ στο κουμπί **Επόμενο**.
![Σύνδεση με Azure AD](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. Στη σελίδα **διαπιστευτηρίων απομακρυσμένης πρόσβασης** , πληκτρολογήστε τα διαπιστευτήρια για το διαχειριστή του τομέα.
![Τα διαπιστευτήρια απομακρυσμένης πρόσβασης](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    Αφού κάνετε κλικ στο κουμπί **Επόμενο**, Azure AD Connect θα ελέγχου εύρυθμης λειτουργίας πιστοποιητικό και εμφάνιση προβλήματα.

    ![Κατάσταση πιστοποιητικών](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    **Είστε έτοιμοι να ρυθμίσετε τις παραμέτρους** της σελίδας θα εμφανίσει τη λίστα των ενεργειών που θα εκτελεστεί για να επιδιορθώσετε την αξιοπιστία.

    ![Είστε έτοιμοι να ρυθμίσετε τις παραμέτρους](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. Κάντε κλικ στην επιλογή **εγκατάσταση** για να επιδιορθώσετε την αξιοπιστία.

>[AZURE.NOTE] Azure AD Connect μπορούν να μόνο επιδιόρθωση ή act για τα πιστοποιητικά που είναι αυτο-υπογεγραμμένο. Azure AD Connect δεν μπορεί να διορθώσει πιστοποιητικά τρίτων.

### Προσθέστε ένα διακομιστή AD FS<a name=addadfsserver></a>

> [AZURE.NOTE] Azure AD Connect απαιτεί το αρχείο πιστοποιητικού PFX για να προσθέσετε ένα διακομιστή AD FS. Γι ' αυτό, μπορείτε να εκτελέσετε αυτήν τη λειτουργία μόνο εάν έχετε ρυθμίσει τις παραμέτρους της συστοιχίας AD FS χρησιμοποιώντας Azure AD Connect.

1. Επιλέξτε **ανάπτυξη ενός πρόσθετου διακομιστή ομοσπονδίας** και κάντε κλικ στο κουμπί **Επόμενο**.
![Διακομιστή ομοσπονδίας επιπλέον](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. Στη σελίδα **σύνδεση με το Azure AD** , εισαγάγετε τα διαπιστευτήριά σας καθολικός διαχειριστής για το Azure AD και κάντε κλικ στο κουμπί **Επόμενο**.
![Σύνδεση με Azure AD](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. Δώστε του τομέα διαπιστευτήρια διαχειριστή.
![Διαπιστευτήρια διαχειριστή τομέα](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Azure AD Connect θα ζητήσουν τον κωδικό πρόσβασης του αρχείου PFX που παρείχατε κατά τη ρύθμιση παραμέτρων συστοιχίας AD FS με Azure AD Connect. Κάντε κλικ στην επιλογή **Εισαγωγή κωδικού πρόσβασης** για να εισαγάγετε τον κωδικό πρόσβασης για το αρχείο PFX.
![Κωδικός πρόσβασης πιστοποιητικού](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![Καθορίστε το πιστοποιητικό SSL](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. Στη σελίδα **AD FS διακομιστές** , πληκτρολογήστε το όνομα του διακομιστή ή τη διεύθυνση IP που θα προστεθεί στη συστοιχία AD FS.
![Οι διακομιστές AD FS](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. Κάντε κλικ στο κουμπί **Επόμενο** και μετάβαση σε στην τελευταία σελίδα **Ρύθμιση παραμέτρων** . Αφού ολοκληρωθεί η προσθήκη τους διακομιστές της συστοιχίας AD FS Azure AD Connect, θα έχετε την επιλογή για να επιβεβαιώσετε τη συνδεσιμότητα.
![Είστε έτοιμοι να ρυθμίσετε τις παραμέτρους](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![Η εγκατάσταση ολοκληρώθηκε](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### Προσθέστε ένα διακομιστή μεσολάβησης εφαρμογών web AD FS<a name=addwapserver></a>

> [AZURE.NOTE] Azure AD Connect απαιτεί το αρχείο πιστοποιητικού PFX για να προσθέσετε ένα διακομιστή μεσολάβησης εφαρμογής web. Γι ' αυτό, θα μπορείτε να εκτελέσετε αυτήν τη λειτουργία μόνο εάν έχετε ρυθμίσει τις παραμέτρους της συστοιχίας AD FS χρησιμοποιώντας Azure AD Connect.

1. Επιλέξτε **Ανάπτυξη διακομιστή μεσολάβησης εφαρμογής Web** από τη λίστα διαθέσιμες εργασίες.
![Ανάπτυξη διακομιστή μεσολάβησης εφαρμογής web](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. Δώστε τα διαπιστευτήρια Azure καθολικός διαχειριστής.
![Σύνδεση με Azure AD](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. Στη σελίδα **το πιστοποιητικό SSL που καθορίζετε** , εισαγάγετε τον κωδικό πρόσβασης για το αρχείο PFX που παρείχατε κατά τη ρύθμιση των παραμέτρων του συμπλέγματος AD FS με Azure AD Connect.
![Κωδικός πρόσβασης πιστοποιητικού](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![Καθορίστε το πιστοποιητικό SSL](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. Προσθέστε το διακομιστή που θα προστεθεί ως ένα διακομιστή μεσολάβησης εφαρμογής web. Επειδή δεν μπορεί να συνδεθεί στο διακομιστή μεσολάβησης εφαρμογής web στον τομέα, ο οδηγός θα ζητήσουν διαπιστευτήρια διαχειριστή στο διακομιστή που προστέθηκε.
![Διαπιστευτήρια διαχειριστή διακομιστή](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. Στη σελίδα **διαπιστευτηρίων αξιοπιστίας διακομιστή μεσολάβησης** , δώστε διαπιστευτήρια διαχειριστή για να ρυθμίσετε τις παραμέτρους του διακομιστή μεσολάβησης αξιοπιστίας και πρόσβασης στον πρωτεύοντα διακομιστή στο σύμπλεγμα AD FS.
![Διαπιστευτήρια αξιοπιστίας διακομιστή μεσολάβησης](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. Στη σελίδα **έτοιμο για ρύθμιση παραμέτρων** , ο οδηγός εμφανίζει τη λίστα των ενεργειών που θα εκτελεστεί.
![Είστε έτοιμοι να ρυθμίσετε τις παραμέτρους](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. Κάντε κλικ στην επιλογή **εγκατάσταση** για να ολοκληρώσετε τη ρύθμιση παραμέτρων. Όταν ολοκληρωθεί η ρύθμιση παραμέτρων, ο οδηγός σάς δίνει την επιλογή για να επιβεβαιώσετε τη συνδεσιμότητα με τους διακομιστές. Κάντε κλικ στην επιλογή **Επαλήθευση** για να ελέγξετε τη σύνδεση.
![Η εγκατάσταση ολοκληρώθηκε](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### Προσθέστε έναν ομόσπονδο τομέα<a name=addfeddomain></a>

Είναι εύκολο να προσθέσετε έναν τομέα για να είναι ομόσπονδη σχέση Azure AD με χρήση του Azure AD Connect. Azure AD Connect προσθέτει τον τομέα για την Ομοσπονδία και τροποποιεί τους κανόνες διεκδίκηση ώστε να αντικατοπτρίζει σωστά τον εκδότη, όταν έχετε πολλούς τομείς συνενωθεί με Azure AD.

1. Για να προσθέσετε έναν ομόσπονδο τομέα, επιλέξτε την εργασία **προσθέσετε έναν επιπλέον τομέα Azure AD**.
![Επιπλέον τομέα Azure AD](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. Στην επόμενη σελίδα του οδηγού, δώστε τα διαπιστευτήρια καθολικός διαχειριστής για Azure AD.
![Σύνδεση με Azure AD](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. Στη σελίδα **διαπιστευτηρίων απομακρυσμένη πρόσβαση** , δώστε του τομέα διαπιστευτήρια διαχειριστή.
![Τα διαπιστευτήρια απομακρυσμένης πρόσβασης](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. Στην επόμενη σελίδα, ο οδηγός παρέχει μια λίστα των τομέων Azure AD με τον οποίο μπορείτε να δημιουργήσετε Ομοσπονδία στον κατάλογο εσωτερικής εγκατάστασης. Επιλέξτε τον τομέα από τη λίστα.
![Azure AD τομέα](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    Αφού επιλέξετε τον τομέα, ο οδηγός θα σας δώσει με κατάλληλες πληροφορίες σχετικά με τις περαιτέρω ενέργειες που θα διαρκέσει ο οδηγός και την επίδραση της ρύθμισης παραμέτρων. Σε ορισμένες περιπτώσεις, εάν επιλέξετε έναν τομέα που δεν έχει επαληθευτεί ακόμη στο Azure AD, ο οδηγός θα σας δώσει με πληροφορίες θα σας βοηθήσουν να επαληθεύσετε τον τομέα. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Προσθήκη το προσαρμοσμένο όνομα τομέα για να Azure Active Directory](active-directory-add-domain.md) .

5. Κάντε κλικ στο κουμπί **Επόμενο**, και **είστε έτοιμοι να ρυθμίσετε τις παραμέτρους** της σελίδας θα εμφανίσει τη λίστα των ενεργειών που θα εκτελέσει Azure AD Connect. Κάντε κλικ στην επιλογή **εγκατάσταση** για να ολοκληρώσετε τη ρύθμιση παραμέτρων.
![Είστε έτοιμοι να ρυθμίσετε τις παραμέτρους](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>AD FS προσαρμογής

Οι παρακάτω ενότητες παρέχουν λεπτομέρειες σχετικά με ορισμένες από τις κοινές εργασίες που ίσως χρειαστεί να εκτελέσετε κατά την προσαρμογή του AD FS στη σελίδα εισόδου.

### Προσθέστε ένα προσαρμοσμένο λογότυπο ή εικόνα<a name=customlogo></a>

Για να αλλάξετε το λογότυπο της εταιρείας που εμφανίζεται στη σελίδα **εισόδου** , χρησιμοποιήστε την παρακάτω cmdlet του Windows PowerShell και τη σύνταξη.

> [AZURE.NOTE] Οι προτεινόμενες διαστάσεις για το λογότυπο είναι 260 x 35 @ 96 dpi με μέγεθος αρχείου δεν είναι μεγαλύτερη από 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] Απαιτείται η παράμετρος *όνομα προορισμού* . Το προεπιλεγμένο θέμα που έχει κυκλοφορήσει με AD FS ονομάζεται προεπιλογή.


### Προσθέστε μια περιγραφή εισόδου<a name=addsignindescription></a>

Για να προσθέσετε μια περιγραφή στη σελίδα εισόδου στη **σελίδα εισόδου**, χρησιμοποιήστε την παρακάτω cmdlet του Windows PowerShell και τη σύνταξη.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### Τροποποίηση AD FS διεκδίκηση κανόνων<a name=modclaims></a>

AD FS υποστηρίζει μια γλώσσα εμπλουτισμένου διεκδίκηση που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε προσαρμοσμένες διεκδίκηση κανόνες. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Το ρόλο της γλώσσας διεκδίκηση κανόνα](https://technet.microsoft.com/library/dd807118.aspx).

Οι παρακάτω ενότητες περιγράφουν πώς μπορείτε να συντάξετε προσαρμοσμένο κανόνες για ορισμένα σενάρια που αφορούν την Azure AD και Ομοσπονδία AD FS.

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Υπό όρους σε μια τιμή που υπάρχουν στο χαρακτηριστικό αμετάβλητες Αναγνωριστικό

Azure AD Connect σάς επιτρέπει να ορίσετε μια ιδιότητα που θα χρησιμοποιηθεί ως ένα σημείο αγκύρωσης προέλευσης όταν τα αντικείμενα είναι συγχρονισμένο με το Azure AD. Εάν η τιμή στο προσαρμοσμένο χαρακτηριστικό δεν είναι κενό, μπορείτε να εκδώσετε ένα αμετάβλητες διεκδίκηση Ταυτότητα. Για παράδειγμα, που μπορεί να επιλέξτε **ms-ds-consistencyguid** ως το χαρακτηριστικό για την αγκύρωση προέλευσης και θέλετε να εκδώσετε **ImmutableID** ως **ms-ds-consistencyguid** , σε περίπτωση που το χαρακτηριστικό έχει μια τιμή σε σχέση με το. Εάν δεν υπάρχει τιμή σε σχέση με το χαρακτηριστικό, ζήτημα **objectGuid** με το αναγνωριστικό αμετάβλητες.  Μπορείτε να δημιουργήσετε το σύνολο των κανόνων προσαρμοσμένο διεκδίκηση όπως περιγράφεται στην επόμενη ενότητα.

**Κανόνας 1: Χαρακτηριστικά ερωτήματος**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

Σε αυτόν τον κανόνα, που άντληση πληροφοριών από τις τιμές του **ms-ds-consistencyguid** και **objectGuid** για το χρήστη από την υπηρεσία καταλόγου Active Directory. Αλλάξτε το όνομα του χώρου αποθήκευσης σε ένα όνομα κατάλληλο store στην ανάπτυξή σας AD FS. Επίσης να αλλάξετε τον τύπο αξιώσεων σε έναν τύπο proper αξιώσεων για την Ομοσπονδία όπως ορίζεται για **objectGuid** και **ms-ds-consistencyguid**.

Επίσης, χρησιμοποιώντας **το ζήτημα**και **Προσθήκη** , να αποφεύγετε την προσθήκη εξερχόμενων πρόβλημα για την οντότητα και να χρησιμοποιήσετε τις τιμές ως ενδιάμεσες τιμές. Θα εκδώσετε το αίτημα σε έναν κανόνα νεότερη έκδοση μετά τη δημιουργία ποια τιμή για χρήση με το αναγνωριστικό αμετάβλητες.

**Κανόνας 2: Ελέγξτε εάν υπάρχει το ms-ds-consistencyguid για το χρήστη**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Αυτός ο κανόνας καθορίζει μια σημαία προσωρινό που ονομάζεται **idflag** που έχει οριστεί σε **useguid** , εάν δεν υπάρχει καμία **ms-ds-concistencyguid** συμπληρωμένο για το χρήστη. Η λογική πίσω από αυτό είναι το γεγονός ότι AD FS δεν επιτρέπει την κενή αξιώσεων. Επομένως, όταν προσθέτετε αξιώσεων http://contoso.com/ws/2016/02/identity/claims/objectguid και http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid του κανόνα 1, τέλος του με μια **msdsconsistencyguid** διεκδίκηση μόνο εάν η τιμή συμπληρώνεται για το χρήστη. Εάν δεν είναι συμπληρωμένο, AD FS βλέπει ότι θα έχει μια κενή τιμή και προσθέτει το αμέσως. Όλα τα αντικείμενα θα έχει **objectGuid**, ώστε η αξίωση να βρίσκεται πάντα εκεί μετά την εκτέλεση κανόνα 1.

**Κανόνας 3: Θεμάτων ms-ds-consistencyguid ως αμετάβλητες ID, αν υπάρχει**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Αυτή είναι μια μη ρητή ελέγχου **υπάρχουν** . Εάν υπάρχει η τιμή για το αίτημα, στη συνέχεια, θεμάτων που με το αναγνωριστικό αμετάβλητες. Το προηγούμενο παράδειγμα χρησιμοποιεί τη διεκδίκηση **nameidentifier** . Θα πρέπει να αλλάξετε αυτήν την επιλογή στον τύπο διεκδίκηση κατάλληλο για το Αναγνωριστικό αμετάβλητες στο περιβάλλον σας.

**Κανόνας 4: Θεμάτων objectGuid ως αμετάβλητες ID, εάν δεν υπάρχει ms-ds-consistencyGuid**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

Σε αυτόν τον κανόνα, απλώς ελέγχετε το προσωρινό σημαία **idflag**. Αποφασίστε αν θα εκδώσετε το αίτημα με βάση την τιμή.

> [AZURE.NOTE] Η ακολουθία από αυτούς τους κανόνες είναι σημαντικές.

#### <a name="sso-with-a-subdomain-upn"></a>SSO με έναν υποτομέα UPN

Μπορείτε να προσθέσετε περισσότερους από έναν τομέα για να είναι ομόσπονδες χρησιμοποιώντας Azure AD Connect, όπως περιγράφεται στο θέμα [Προσθήκη ενός νέου ομόσπονδο τομέα](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain). Πρέπει να τροποποιήσετε το αίτημα UPN, έτσι ώστε το Αναγνωριστικό εκδότη αντιστοιχεί τον ριζικό τομέα και δεν ο δευτερεύων τομέας, επειδή η εξωτερική ριζικό τομέα καλύπτει επίσης το παιδί.

Από προεπιλογή, ο κανόνας διεκδίκηση για Αναγνωριστικό εκδότη έχει οριστεί ως:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Διεκδίκηση Ταυτότητα εκδότη προεπιλεγμένη](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

Ο προεπιλεγμένος κανόνας απλώς λαμβάνει το επίθημα UPN και χρησιμοποιεί στο τη διεκδίκηση Ταυτότητα εκδότη. Για παράδειγμα, John είναι ένας χρήστης στο sub.contoso.com και contoso.com έχει συνενωθεί με Azure AD. Εισάγει John john@sub.contoso.com ως το όνομα χρήστη κατά την είσοδο στο Azure AD, και το προεπιλεγμένο Αναγνωριστικό εκδότη διεκδίκηση κανόνα στο AD FS χειρίζεται το με τον ακόλουθο τρόπο.

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Τιμής δήλωσης:** http://sub.contoso.com/adfs/services/trust/

Για να έχετε μόνο τον ριζικό τομέα στην τιμή διεκδίκηση εκδότη, αλλάξτε τον κανόνα διεκδίκηση ώστε να ταιριάζει με τα εξής.

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με τις [Επιλογές εισόδου του χρήστη](active-directory-aadconnect-user-signin.md).
