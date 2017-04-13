<properties
    pageTitle="Πώς μπορείτε να δημιουργήσετε και να μεταφέρετε προστασία HSM πλήκτρα για Azure θάλαμο αριθμού-κλειδιού | Microsoft Azure"
    description="Χρησιμοποιήστε αυτό το άρθρο θα σας βοηθήσουν να Σχεδιασμός για, δημιουργία και, στη συνέχεια, μεταφέρετέ τα δικά σας πλήκτρα προστασία HSM για χρήση με το Azure θάλαμο αριθμού-κλειδιού."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>
#<a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Πώς μπορείτε να δημιουργήσετε και να μεταφέρετε προστατευμένη HSM πλήκτρα για θάλαμο κλειδί Azure

##<a name="introduction"></a>Εισαγωγή

Για πρόσθετες εξασφάλισης αναβάθμισης, κατά τη χρήση του αριθμού-κλειδιού θάλαμο Azure, μπορείτε να εισαγάγετε ή να δημιουργία των κλειδιών στο υλικό ασφαλείας λειτουργικές μονάδες (HSMs) που ποτέ αποχώρηση από το όριο HSM. Αυτό το σενάριο αναφέρεται συχνά ως *μεταφέρετε το δικό σας κλειδί*ή BYOK. Το HSMs είναι FIPS 140-2 επιπέδου 2 επικύρωση. Azure θάλαμο κλειδί χρησιμοποιεί οικογένεια nShield Thales HSMs για την προστασία των αριθμών-κλειδιών.

Χρησιμοποιήστε τις πληροφορίες σε αυτό το θέμα θα σας βοηθήσουν να Σχεδιασμός για, δημιουργία και, στη συνέχεια, μεταφέρετέ τα δικά σας πλήκτρα HSM προστασία για χρήση με το Azure θάλαμο αριθμού-κλειδιού.

Αυτή η λειτουργία δεν είναι διαθέσιμη για την Κίνα Azure. 

>[AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με το Azure κλειδί θάλαμο, ανατρέξτε στο θέμα [Τι είναι το Azure κλειδί θάλαμο;](key-vault-whatis.md)  
>
>Για ένα γρήγορα αποτελέσματα πρόγραμμα εκμάθησης, που περιλαμβάνει τη δημιουργία ενός κλειδιού θάλαμο για προστασία HSM κλειδιά, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure κλειδί θάλαμο](key-vault-get-started.md).

Περισσότερες πληροφορίες σχετικά με τη δημιουργία και τη μεταφορά του κλειδιού προστασία HSM μέσω του Internet:

- Μπορείτε να δημιουργήσετε τον αριθμό-κλειδί από μια εργασία χωρίς σύνδεση σταθμούς εργασίας, μειώνοντας έτσι στην επιφάνεια επίθεση.

- Το κλειδί είναι κρυπτογραφημένα με μια υπηρεσία αριθμού-κλειδιού Exchange αριθμού-κλειδιού (KEK), η οποία παραμένει κρυπτογραφημένο μέχρι να μεταφερθούν για να το HSMs Azure κλειδί θάλαμο. Μόνο το κρυπτογραφημένο έκδοση του αριθμού-κλειδιού αφήνει το αρχικό σταθμούς εργασίας.

- Το σύνολο των εργαλείων Ορίζει ιδιότητες στο μισθωτή του αριθμού-κλειδιού που συνδέει τον αριθμό-κλειδί στον κόσμο ασφαλείας Azure κλειδί θάλαμο. Επομένως, μετά την HSMs Azure κλειδί θάλαμο λήψη και να αποκρυπτογραφήσετε τον αριθμό-κλειδί, μόνο αυτές τις HSMs να το χρησιμοποιήσετε. Δεν είναι δυνατή η εξαγωγή τον αριθμό-κλειδί. Τίθεται σε ισχύ αυτήν τη σύνδεση με το HSMs Thales.

- Το κλειδί Exchange αριθμού-κλειδιού (KEK) που χρησιμοποιείται για να κρυπτογραφήσετε τον αριθμό-κλειδί δημιουργείται στο εσωτερικό του Azure κλειδί θάλαμο HSMs και δεν είναι δυνατή η εξαγωγή. Το HSMs επιβολή ότι μπορεί να υπάρχει καμία Απαλοιφή έκδοση του το KEK έξω από το HSMs. Επιπλέον, το σύνολο των εργαλείων περιλαμβάνει βεβαίωση από Thales ότι το KEK δεν είναι δυνατή η εξαγωγή και δημιουργήθηκε μέσα σε ένα γνήσιο HSM που έχει κατασκευαστεί από Thales.

- Το σύνολο των εργαλείων περιλαμβάνει βεβαίωση από Thales ότι δημιουργήθηκε επίσης τον κόσμο ασφαλείας Azure θάλαμο αριθμού-κλειδιού σε ένα γνήσιο HSM κατασκευαστεί από Thales. Η εν λόγω βεβαίωση αποδείξει σε εσάς ότι η Microsoft χρησιμοποιεί genuine υλικού.

- Microsoft χρησιμοποιεί ξεχωριστή KEKs και διαχωρίστε κατηγορία ασφαλείας σε κάθε γεωγραφική περιοχή. Ο διαχωρισμός εξασφαλίζει ότι τον αριθμό-κλειδί μπορεί να χρησιμοποιηθεί μόνο σε κέντρα δεδομένων στην περιοχή στην οποία θα το κρυπτογραφημένα. Για παράδειγμα, έναν αριθμό-κλειδί από έναν πελάτη ευρωπαϊκές δεν μπορούν να χρησιμοποιηθούν σε κέντρα δεδομένων στη Βόρεια Αμερική ή Ασία.

##<a name="more-information-about-thales-hsms-and-microsoft-services"></a>Περισσότερες πληροφορίες σχετικά με τις υπηρεσίες Thales HSMs και της Microsoft

Thales e-ασφαλείας είναι μια στην υπηρεσία παροχής καθολικού κρυπτογράφηση δεδομένων και λύσεις cyber ασφαλείας του οικονομικές υπηρεσίες, υψηλής τεχνολογίας, κατασκευής, για δημόσιους οργανισμούς και τεχνολογία τομείς. Με 40 ετών παρακολούθηση record της προστασίας εταιρικό και πληροφορίες για δημόσιους οργανισμούς, Thales λύσεις που χρησιμοποιούνται από τέσσερις ή πέντε μεγαλύτερη ενέργειας και αεροδιαστημική εταιρείες. Λύσεις τους μπορούν επίσης να χρησιμοποιηθούν από 22 ΝΑΤΟ χώρες και ασφαλούς περισσότερο από 80% των συναλλαγών σε όλο τον κόσμο πληρωμής.

Microsoft συνεργάζεται με Thales για τη βελτίωση της κατάστασης art για HSMs. Αυτές οι βελτιώσεις σάς επιτρέπουν να λάβετε το τυπικό πλεονεκτήματα της φιλοξενούμενη υπηρεσίες χωρίς relinquishing έλεγχο των αριθμών-κλειδιών. Συγκεκριμένα, αυτές οι βελτιώσεις σας επιτρέπουν να διαχειρίζεστε το HSMs ώστε να δεν χρειάζεται να Microsoft. Ως μια υπηρεσία cloud, θάλαμο κλειδί Azure κλίμακες προς τα επάνω στο σύντομο ειδοποίηση ώστε να πληρούνται οι αιχμές χρήση της εταιρείας σας. Την ίδια στιγμή, τον αριθμό-κλειδί είναι προστατευμένο εντός της Microsoft HSMs: διατήρηση του ελέγχου επί του κύκλου ζωής κλειδιού, επειδή η δημιουργία του κλειδιού και μεταφέρετε σε HSMs της Microsoft.

##<a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Εφαρμογή μεταφέρετε το δικό σας κλειδί (BYOK) για θάλαμο κλειδί Azure

Χρησιμοποιήστε τις παρακάτω πληροφορίες και διαδικασίες, εάν θα δημιουργήσετε το δικό σας κλειδί HSM προστασία και, στη συνέχεια, μεταφέρετέ την σε θάλαμο κλειδί Azure — η μεταφορά το δικό σας σενάριο κλειδί (BYOK).


##<a name="prerequisites-for-byok"></a>Τις προϋποθέσεις για τα BYOK

Δείτε τον παρακάτω πίνακα για μια λίστα με τις προϋποθέσεις για τα μεταφέρετε το δικό σας κλειδί (BYOK) για το Azure κλειδί θάλαμο.

|Απαίτηση|Περισσότερες πληροφορίες|
|---|---|
|Μια συνδρομή στο Azure|Για να δημιουργήσετε ένα θάλαμο κλειδί Azure, χρειάζεστε μια συνδρομή του Azure: [εγγραφή για δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/)|
|Το επίπεδο υπηρεσιών Azure Premium θάλαμο αριθμού-κλειδιού για την υποστήριξη προστασία HSM πλήκτρα|Για περισσότερες πληροφορίες σχετικά με τις δυνατότητες και επίπεδα υπηρεσίας για θάλαμο κλειδί Azure, ανατρέξτε στην τοποθεσία Web [Τις τιμές του Azure κλειδί θάλαμο](https://azure.microsoft.com/pricing/details/key-vault/) .|
|Thales HSM, έξυπνων καρτών και λογισμικό υποστήριξης|Πρέπει να έχετε πρόσβαση σε μια λειτουργική μονάδα ασφάλειας υλικού Thales και βασικές λειτουργικές γνώσεις Thales HSMs. Ανατρέξτε στο θέμα [Λειτουργική μονάδα ασφάλειας υλικού Thales](https://www.thales-esecurity.com/msrms/buy) για τη λίστα συμβατά μοντέλων, ή για να αγοράσετε ένα HSM, εάν δεν έχετε ένα.|
|Το παρακάτω υλικό και λογισμικό:<ol><li>Μια εργασία χωρίς σύνδεση x64 σταθμούς εργασίας με μια ελάχιστη σύστημα λειτουργίας των Windows του λογισμικού nShield Windows 7 και Thales που είναι τουλάχιστον την έκδοση 11.50.<br/><br/>Εάν αυτό σταθμούς εργασίας εκτελεί Windows 7, θα πρέπει να [εγκαταστήσετε το Microsoft .NET Framework διαίρεσης 4,5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Μια σταθμούς εργασίας που είναι συνδεδεμένοι στο Internet και έχει μια ελάχιστη Windows λειτουργικού συστήματος των Windows 7.</li><li>Μια μονάδα δίσκου USB ή άλλες φορητή συσκευή αποθήκευσης που διαθέτει τουλάχιστον 16 MB ελεύθερο χώρο.</li></ol>|Για λόγους ασφαλείας, συνιστάται να ότι η πρώτη σταθμούς εργασίας δεν συνδέεται με ένα δίκτυο. Ωστόσο, αυτή η σύσταση δεν μέσω προγραμματισμού θα εφαρμοστεί.<br/><br/>Σημειώστε ότι στις οδηγίες που ακολουθούν, αυτό σταθμούς εργασίας αναφέρεται ως το αποσύνδεσης σταθμούς εργασίας.</p></blockquote><br/>Επιπλέον, εάν είναι μισθωτή του αριθμού-κλειδιού για ένα δίκτυο παραγωγής, συνιστάται να χρησιμοποιήσετε μια δεύτερη, ξεχωριστή σταθμούς εργασίας να κάνετε λήψη το σύνολο των εργαλείων και να αποστείλετε το κλειδί μισθωτή. Ωστόσο, για σκοπούς δοκιμής, μπορείτε να χρησιμοποιήσετε την ίδια σταθμούς εργασίας ως το πρώτο.<br/><br/>Σημειώστε ότι στις οδηγίες που ακολουθούν, το δεύτερο σταθμούς εργασίας αναφέρεται ως το σταθμούς εργασίας που συνδέονται μέσω του Internet.</p></blockquote><br/>|

##<a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Δημιουργία και να μεταφέρετε τον αριθμό-κλειδί HSM θάλαμο κλειδί Azure

Θα μπορείτε να χρησιμοποιήσετε τα ακόλουθα πέντε βήματα για να δημιουργήσετε και να μεταφέρετε τον αριθμό-κλειδί σε μια HSM Azure κλειδί θάλαμο:

- [Βήμα 1: Προετοιμασία σας σταθμούς εργασίας που συνδέονται μέσω του Internet](#step-1-prepare-your-internet-connected-workstation)
- [Βήμα 2: Προετοιμασία σας αποσύνδεσης σταθμούς εργασίας](#step-2-prepare-your-disconnected-workstation)
- [Βήμα 3: Δημιουργία τον αριθμό-κλειδί](#step-3-generate-your-key)
- [Βήμα 4: Προετοιμασία τον αριθμό-κλειδί για μεταφορά](#step-4-prepare-your-key-for-transfer)
- [Βήμα 5: Μεταφέρετε τον αριθμό-κλειδί σε θάλαμο κλειδί Azure](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Βήμα 1: Προετοιμασία σας σταθμούς εργασίας που συνδέονται μέσω του Internet
Για αυτό το πρώτο βήμα, κάντε τις ακόλουθες διαδικασίες σε σας σταθμούς εργασίας που είναι συνδεδεμένοι στο Internet.


###<a name="step-11-install-azure-powershell"></a>Βήμα 1.1: Εγκατάσταση του Azure PowerShell

Από το σταθμούς εργασίας που συνδέονται μέσω του Internet, κάντε λήψη και εγκαταστήσετε τη λειτουργική μονάδα Azure PowerShell που περιλαμβάνει το cmdlet για τη Διαχείριση Azure κλειδί θάλαμο. Αυτό απαιτεί μια ελάχιστη έκδοση του 0.8.13.

Για οδηγίες εγκατάστασης, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

###<a name="step-12-get-your-azure-subscription-id"></a>Βήμα 1.2: Λήψη το Αναγνωριστικό συνδρομής Azure

Ξεκινήστε μια περίοδο λειτουργίας Azure PowerShell και πραγματοποιήστε είσοδο στο λογαριασμό σας στο Azure χρησιμοποιώντας την ακόλουθη εντολή:

        Add-AzureAccount
Στο παράθυρο περιήγησης αναδυόμενο, εισαγάγετε το όνομα χρήστη λογαριασμός Azure και τον κωδικό πρόσβασης. Στη συνέχεια, χρησιμοποιήστε την εντολή [Get-AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) :

        Get-AzureSubscription
Από την έξοδο, εντοπίστε το Αναγνωριστικό για τη συνδρομή που θα χρησιμοποιήσετε για το Azure κλειδί θάλαμο. Θα χρειαστείτε αργότερα αυτό το Αναγνωριστικό συνδρομής.

Μην κλείσετε το παράθυρο Azure PowerShell.

###<a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Βήμα 1.3: Λήψη το σύνολο εργαλείων BYOK για θάλαμο κλειδί Azure

Μεταβείτε στο Κέντρο λήψης της Microsoft και [κάντε λήψη το σύνολο εργαλείων BYOK θάλαμο Azure αριθμού-κλειδιού](http://www.microsoft.com/download/details.aspx?id=45345) για τη γεωγραφική περιοχή ή την παρουσία του Azure. Χρησιμοποιήστε τις ακόλουθες πληροφορίες για να προσδιορίσετε το όνομα του πακέτου για λήψη και την αντίστοιχη Κατακερματισμός πακέτου SHA-256:

---

**Βόρεια Αμερική:**

KeyVault-BYOK-εργαλεία-United States.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

---

**Ευρώπη:**

KeyVault-BYOK-εργαλεία-Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

---

**Ασία:**

KeyVault-BYOK-εργαλεία-AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

---

**Λατινική Αμερική:**

KeyVault-BYOK-εργαλεία-LatinAmerica.zip
    
9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

---

**Ιαπωνία:**

KeyVault-BYOK-εργαλεία-Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

---

**Αυστραλία:**

KeyVault-BYOK-εργαλεία-Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

---

[**Azure δημόσιους οργανισμούς:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-εργαλεία-USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

---

**Καναδάς:**

KeyVault-BYOK-εργαλεία-Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

---

**Γερμανία:**

KeyVault-BYOK-εργαλεία-Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

---
**Ινδία:**

KeyVault-BYOK-εργαλεία-India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

---

Για την επικύρωση της ακεραιότητας των σας έχουν ληφθεί σύνολο εργαλείων BYOK, από την περίοδο λειτουργίας Azure PowerShell, χρησιμοποιήστε το cmdlet [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) .

    Get-FileHash KeyVault-BYOK-Tools-*.zip

Το σύνολο των εργαλείων περιλαμβάνει τα εξής:

- Ένα πακέτο κλειδί Exchange αριθμού-κλειδιού (KEK) που περιέχει ένα όνομα που ξεκινά με **BYOK-KEK - pkg-.**
- Ένα πακέτο κόσμο ασφαλείας που έχει ένα όνομα που ξεκινά με **BYOK-SecurityWorld - pkg-.**
- Μια δέσμη ενεργειών python με το όνομα v**erifykeypackage.py.**
- Μια γραμμή εντολών εκτελέσιμη αρχείων με όνομα **KeyTransferRemote.exe** και σχετικές βιβλιοθήκες DLL.
- Ένα Visual C++ με δυνατότητα ανακατανομής πακέτο, με το όνομα **vcredist_x64.exe.**

Αντιγράψτε το πακέτο σε μια μονάδα δίσκου USB ή άλλες portable χώρου αποθήκευσης.

##<a name="step-2-prepare-your-disconnected-workstation"></a>Βήμα 2: Προετοιμασία σας αποσύνδεσης σταθμούς εργασίας

Για αυτό το δεύτερο βήμα, κάντε τα εξής διαδικασίες σχετικά με το σταθμούς εργασίας που δεν είναι συνδεδεμένο σε δίκτυο (είτε στο Internet είτε στο εσωτερικό σας δίκτυο).


###<a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>2.1 βήμα: Προετοιμασία του αποσύνδεσης σταθμούς εργασίας με Thales HSM

Εγκατάσταση του λογισμικού υποστήριξη nCipher (Thales) σε έναν υπολογιστή Windows και, στη συνέχεια, επισυνάψτε ένα HSM Thales σε αυτόν τον υπολογιστή.

Βεβαιωθείτε ότι τα εργαλεία Thales είναι στη διαδρομή σας (**%nfast_home%\bin** και **%nfast_home%\python\bin**). Για παράδειγμα, πληκτρολογήστε τα εξής:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

Για περισσότερες πληροφορίες, ανατρέξτε στον Οδηγό χρήστη περιλαμβάνεται με το HSM Thales.

###<a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Βήμα 2.2: Εγκατάσταση το σύνολο εργαλείων BYOK σε το αποσύνδεσης σταθμούς εργασίας

Αντιγράψτε το πακέτο εργαλείων BYOK από τη μονάδα δίσκου USB ή άλλες portable χώρου αποθήκευσης και, στη συνέχεια, κάντε τα εξής:

1. Εξαγωγή αρχείων από το πακέτο που έχουν ληφθεί σε οποιονδήποτε φάκελο.
2. Από αυτόν το φάκελο, εκτελέστε το vcredist_x64.exe.
3. Ακολουθήστε τις οδηγίες για την εγκατάσταση των στοιχείων χρόνου εκτέλεσης Visual C++ για το Visual Studio 2013.

##<a name="step-3-generate-your-key"></a>Βήμα 3: Δημιουργία τον αριθμό-κλειδί

Για αυτό το τρίτο βήμα, κάντε τα εξής διαδικασίες σχετικά με το αποσύνδεσης σταθμούς εργασίας.

###<a name="step-31-create-a-security-world"></a>Βήμα 3.1: Δημιουργία στον κόσμο ασφαλείας

Ξεκινήστε μια γραμμή εντολών και εκτελέστε το πρόγραμμα Thales νέο κόσμο.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Αυτό το πρόγραμμα δημιουργεί ένα αρχείο **Κόσμο ασφαλείας** στο % NFAST_KMDATA%\local\world, που αντιστοιχεί στο φάκελο C:\ProgramData\nCipher\Key Data\local διαχείρισης. Μπορείτε να χρησιμοποιήσετε διαφορετικές τιμές για το απαρτίας αλλά στο παράδειγμά μας, θα σας ζητηθεί να εισαγάγετε τρεις κενό κάρτες και αριθμοί PIN για κάθε μία. Στη συνέχεια, οποιαδήποτε δύο κάρτες Δώστε πλήρη πρόσβαση στον κόσμο ασφαλείας. Αυτές οι κάρτες γίνονται το **Διαχειριστή κάρτα οριστεί** για τον νέο κόσμο ασφαλείας.

Στη συνέχεια, κάντε τα εξής:

- Δημιουργία αντιγράφου ασφαλείας του αρχείου κόσμο. Ασφαλούς και προστασία του αρχείου κόσμο, κάρτες διαχειριστή και τους αριθμοί PIN και βεβαιωθείτε ότι δεν υπάρχει μόνο άτομο έχει πρόσβαση σε περισσότερες από μία κάρτες.

###<a name="step-32-validate-the-downloaded-package"></a>Βήμα 3.2: Λήψη του πακέτου επικύρωση

Σε αυτό το βήμα είναι προαιρετικό αλλά συνιστάται έτσι ώστε να μπορείτε να επικυρώσετε τα εξής:

- Το κλειδί Exchange αριθμό-κλειδί που περιλαμβάνει το σύνολο των εργαλείων έχει δημιουργηθεί από ένα γνήσιο HSM Thales.
- Ο κατακερματισμός του κόσμου ασφαλείας που περιλαμβάνεται στο το σύνολο των εργαλείων έχει δημιουργηθεί σε ένα γνήσιο HSM Thales.
- Το κλειδί Exchange κλειδί είναι χωρίς δυνατότητα εξαγωγής.

>[AZURE.NOTE]Για να επικυρώσετε τη λήψη πακέτου, η HSM πρέπει να είστε συνδεδεμένοι, ενεργοποιημένη, και πρέπει να έχετε στον κόσμο ασφαλείας σε αυτό (όπως αυτό που μόλις δημιουργήσατε).

Για να επικυρώσετε τη λήψη πακέτου:

1.  Εκτελέστε τη δέσμη ενεργειών verifykeypackage.py κάνοντας ένα από τα εξής, ανάλογα με το γεωγραφική περιοχή ή την παρουσία του Azure δέσμευσης:
    - Για τη Βόρεια Αμερική:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - Για την Ευρώπη:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - Για Ασία:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - Για Λατινική Αμερική:

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - Για την Ιαπωνία:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - Για την Αυστραλία:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
    - Για [Δημόσιους οργανισμούς Azure](https://azure.microsoft.com/features/gov/), που χρησιμοποιεί την παρουσία για δημόσιους οργανισμούς ΗΠΑ Azure:

            python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
    - Για τον Καναδά:

            python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
    - Για τη Γερμανία:

            python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
    - Για την Ινδία:

            python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
    >[AZURE.TIP]Το λογισμικό Thales περιλαμβάνει python στο %NFAST_HOME%\python\bin

2.  Επιβεβαιώστε ότι μπορείτε να δείτε τα ακόλουθα, τα οποία υποδεικνύει επιτυχής επικύρωσης: **αποτέλεσμα: ΕΠΙΤΥΧΊΑ**

Αυτή η δέσμη ενεργειών επικυρώσει η αλυσίδα υπογράφοντα έως το ριζικό κλειδί Thales. Ο κατακερματισμός αυτού του κλειδιού ρίζας είναι ενσωματωμένο στη δέσμη ενεργειών και την τιμή που πρέπει να **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Μπορείτε επίσης να επιβεβαιώσετε αυτήν την τιμή ξεχωριστά, επισκεφθείτε την [τοποθεσία Web Thales](http://www.thalesesec.com/).

Τώρα είστε έτοιμοι να δημιουργήσετε ένα νέο αριθμό-κλειδί.

###<a name="step-33-create-a-new-key"></a>Βήμα 3.3: Δημιουργήστε ένα νέο αριθμό-κλειδί

Δημιουργία ενός κλειδιού, χρησιμοποιώντας το πρόγραμμα **generatekey** Thales.

Εκτελέστε την παρακάτω εντολή για να δημιουργήσετε το κλειδί:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Κατά την εκτέλεση αυτής της εντολής, χρησιμοποιήστε αυτές τις οδηγίες:

- Η παράμετρος *προστασία* πρέπει να οριστεί στην τιμή **λειτουργική μονάδα**, όπως φαίνεται. Αυτό δημιουργεί έναν αριθμό-κλειδί προστασία λειτουργική μονάδα. Το σύνολο εργαλείων BYOK δεν υποστηρίζει πλήκτρα προστασία OCS.

- Αντικαταστήστε την τιμή του *contosokey* για τα **ident** και **plainname** με οποιαδήποτε τιμή συμβολοσειράς. Για να ελαχιστοποιήσετε διαχείρισης διαφάνειες και να μειώσετε τον κίνδυνο σφάλματα, συνιστάται να χρησιμοποιήσετε την ίδια τιμή για τα δύο. Η τιμή **ident** πρέπει να περιέχει μόνο αριθμούς, παύλες, πεζών και κεφαλαίων γραμμάτων.

- Το pubexp θα παραμείνει κενό (προεπιλογή) σε αυτό το παράδειγμα, αλλά μπορείτε να καθορίσετε συγκεκριμένες τιμές. Για περισσότερες πληροφορίες, ανατρέξτε στην τεκμηρίωση Thales.

Αυτή η εντολή δημιουργεί ένα αρχείο με διακριτικά κλειδί στο φάκελο %NFAST_KMDATA%\local με όνομα ξεκινώντας με **key_simple_**, ακολουθούμενο από το **ident** που έχει καθοριστεί από την εντολή. Για παράδειγμα: **key_simple_contosokey**. Αυτό το αρχείο περιέχει ένα κρυπτογραφημένο κλειδί.

Δημιουργία αντιγράφου ασφαλείας αυτό αρχείο κλειδί με διακριτικά σε ασφαλή θέση.

>[AZURE.IMPORTANT] Όταν μπορείτε αργότερα να μεταφέρετε τον αριθμό-κλειδί σε θάλαμο κλειδί Azure, Microsoft δεν είναι δυνατό να εξαγάγετε αυτό το κλειδί σε εσάς, ώστε να μετατραπεί πολύ σημαντικά δημιουργείτε αντίγραφα ασφαλείας των τον κόσμο του αριθμού-κλειδιού και την ασφάλεια με ασφάλεια. Επικοινωνήστε με Thales για οδηγίες και βέλτιστες πρακτικές για δημιουργία αντιγράφων ασφαλείας τον αριθμό-κλειδί.

Τώρα είστε έτοιμοι να αλλάξετε τον αριθμό-κλειδί Azure κλειδί θάλαμο.

##<a name="step-4-prepare-your-key-for-transfer"></a>Βήμα 4: Προετοιμασία τον αριθμό-κλειδί για μεταφορά

Για αυτό το τέταρτο βήμα, κάντε τα εξής διαδικασίες σχετικά με το αποσύνδεσης σταθμούς εργασίας.

###<a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Βήμα 4.1: Δημιουργήστε ένα αντίγραφο του αριθμού-κλειδιού με μειωμένη δικαιώματα

Για να μειώσετε τα δικαιώματα για τον αριθμό-κλειδί, από μια γραμμή εντολών, εκτελέστε ένα από τα εξής, ανάλογα με τη γεωγραφική περιοχή ή την παρουσία του Azure σας:

- Για τη Βόρεια Αμερική:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- Για την Ευρώπη:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- Για Ασία:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- Για Λατινική Αμερική:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- Για την Ιαπωνία:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- Για την Αυστραλία:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
- Για [Δημόσιους οργανισμούς Azure](https://azure.microsoft.com/features/gov/), που χρησιμοποιεί την παρουσία για δημόσιους οργανισμούς ΗΠΑ Azure:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
- Για τον Καναδά:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
- Για τη Γερμανία:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
- Για την Ινδία:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1


Κατά την εκτέλεση αυτής της εντολής, αντικαταστήστε *contosokey* με την ίδια τιμή που καθορίσατε στο **3.3 βήμα: Δημιουργήστε ένα νέο κλειδί** από το βήμα [Δημιουργία τον αριθμό-κλειδί](#step-3-generate-your-key) .

Θα σας ζητηθεί να συνδέσετε τις κάρτες διαχείρισης κόσμο ασφαλείας.

Κατά την ολοκλήρωση της εντολής, μπορείτε να δείτε **αποτέλεσμα: ΕΠΙΤΥΧΊΑ** και το αντίγραφο του αριθμού-κλειδιού με μειωμένη δικαιώματα που υπάρχουν στο αρχείο με το όνομα key_xferacId_<contosokey>.

###<a name="step-42-inspect-the-new-copy-of-the-key"></a>Βήμα 4.2: Έλεγχος το νέο αντίγραφο του αριθμού-κλειδιού

Προαιρετικά, εκτελέστε το Thales βοηθητικά προγράμματα για να επιβεβαιώσετε τα ελάχιστα δικαιώματα στο νέο κλειδί:

- aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
Κατά την εκτέλεση αυτών των εντολών, αντικαταστήστε contosokey με την ίδια τιμή που καθορίσατε στο **3.3 βήμα: Δημιουργήστε ένα νέο κλειδί** από το βήμα [Δημιουργία τον αριθμό-κλειδί](#step-3-generate-your-key) .

###<a name="step-43-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Βήμα 4.3: Κρυπτογράφηση τον αριθμό-κλειδί, χρησιμοποιώντας κλειδί ανταλλαγής κλειδί της Microsoft

Εκτελέστε μία από τις παρακάτω εντολές, ανάλογα με τη γεωγραφική περιοχή ή την παρουσία του Azure σας:

- Για τη Βόρεια Αμερική:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Για την Ευρώπη:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Για Ασία:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Για Λατινική Αμερική:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Για την Ιαπωνία:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Για την Αυστραλία:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Για [Δημόσιους οργανισμούς Azure](https://azure.microsoft.com/features/gov/), που χρησιμοποιεί την παρουσία για δημόσιους οργανισμούς ΗΠΑ Azure:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Για τον Καναδά:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Για τη Γερμανία:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Για την Ινδία:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Κατά την εκτέλεση αυτής της εντολής, χρησιμοποιήστε αυτές τις οδηγίες:

- Αντικαταστήστε το *contosokey* με το αναγνωριστικό που χρησιμοποιήσατε για να δημιουργήσετε το κλειδί στο **3.3 βήμα: Δημιουργήστε ένα νέο κλειδί** από το βήμα [Δημιουργία τον αριθμό-κλειδί](#step-3-generate-your-key) .

- Αντικαταστήστε *SubscriptionID* με το Αναγνωριστικό της συνδρομής Azure που περιέχει το πλήκτρο θάλαμο. Ανάκτηση αυτήν την τιμή στο παρελθόν, στο **1.2 βήμα: λάβετε το Αναγνωριστικό Azure συνδρομή** από το βήμα [Προετοιμασία σας σταθμούς εργασίας που συνδέονται μέσω του Internet](#step-1-prepare-your-internet-connected-workstation) .

- Αντικαταστήστε *ContosoFirstHSMKey* με μια ετικέτα που χρησιμοποιείται για το όνομα του αρχείου εξόδου.

Όταν αυτή ολοκληρωθεί με επιτυχία, εμφανίζει **αποτέλεσμα: ΕΠΙΤΥΧΊΑ** και υπάρχει ένα νέο αρχείο στον τρέχοντα φάκελο που έχει το ακόλουθο όνομα: TransferPackage -*ContosoFirstHSMkey*.byok

###<a name="step-44-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Βήμα 4.4: Αντιγραφή το πακέτο βασικών μεταφοράς για τη σταθμούς εργασίας που συνδέονται μέσω του Internet

Χρησιμοποιήστε μια μονάδα δίσκου USB ή άλλες portable χώρου αποθήκευσης για να αντιγράψετε το αρχείο εξόδου από το προηγούμενο βήμα (KeyTransferPackage-ContosoFirstHSMkey.byok) για να σας σταθμούς εργασίας που συνδέονται μέσω του Internet.

##<a name="step-5-transfer-your-key-to-azure-key-vault"></a>Βήμα 5: Μεταφέρετε τον αριθμό-κλειδί σε θάλαμο κλειδί Azure

Για αυτό το τελευταίο βήμα, σε σταθμούς εργασίας συνδέονται μέσω του Internet, χρησιμοποιήστε το cmdlet [AzureKeyVaultKey Προσθήκη](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) για να αποστείλετε το πακέτο βασικών μεταφοράς που αντιγράψατε από το αποσύνδεσης σταθμούς εργασίας για να το Azure κλειδί θάλαμο HSM:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Εάν η αποστολή είναι επιτυχής, θα δείτε τις ιδιότητες του αριθμού-κλειδιού που μόλις προσθέσατε εμφανίζεται.


##<a name="next-steps"></a>Επόμενα βήματα

Τώρα, μπορείτε να χρησιμοποιήσετε αυτό το κλειδί προστασία HSM στο σας κλειδιού θάλαμο. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα **Εάν θέλετε να χρησιμοποιήσετε μια λειτουργική μονάδα ασφαλείας υλικού (HSM)** στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure κλειδί θάλαμο](key-vault-get-started.md) .
