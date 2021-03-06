<properties
    pageTitle="Azure Active Directory ταυτότητας προστασίας playbook | Microsoft Azure"
    description="Μάθετε πώς προστασία Azure AD ταυτότητας σάς επιτρέπει να περιορίσετε τη δυνατότητα ένας εισβολέας για την εκμετάλλευση μια ταυτότητα έχει παραβιαστεί ή τη συσκευή και να διασφαλίσετε μια ταυτότητα ή μια συσκευή που ήταν προηγουμένως ύποπτα ή γνωστά να έχει παραβιαστεί."
    services="active-directory"
    keywords="προστασία ταυτότητας καταλόγου Azure active directory, εντοπισμού εφαρμογή cloud, τη Διαχείριση εφαρμογών, ασφαλείας, κινδύνου, επίπεδο κινδύνου, ευπάθεια, πολιτική ασφαλείας"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-playbook"></a>Azure playbook προστασία ταυτότητας Active Directory 

Σε αυτό το playbook σάς βοηθά να:

- Συμπλήρωση δεδομένων σε περιβάλλον προστασία ταυτότητας προσομοιώνουν συμβάντα κινδύνου και ευπάθειες
- Ρύθμιση πολιτικών βάσει του κινδύνου υπό όρους πρόσβασης και να ελέγξετε την επίδραση αυτών των πολιτικών


## <a name="simulating-risk-events"></a>Προσομοίωση συμβάντα κινδύνου

Αυτή η ενότητα σάς παρέχει τα βήματα για την προσομοίωση τους ακόλουθους τύπους συμβάντων κινδύνου:

- Σύμβολο πρόσθετα από ανώνυμου διευθύνσεις IP (εύκολη)
- Σύμβολο πρόσθετα από άγνωστα θέσεις (επόπτευση)
- Αδύνατη ταξίδια ασυνήθιστης θέσεις (δύσκολος)

Δεν είναι δυνατή η προσομοίωση άλλα συμβάντα κινδύνου με ασφαλή τρόπο.


### <a name="sign-ins-from-anonymous-ip-addresses"></a>Σύμβολο πρόσθετα από ανώνυμου διευθύνσεις IP

Αυτός ο τύπος συμβάντος κινδύνου προσδιορίζει τους χρήστες που έχουν συνδεθεί με επιτυχία από μια διεύθυνση IP που έχει προσδιοριστεί ως μια διεύθυνση IP του διακομιστή μεσολάβησης ανώνυμα. Αυτές οι διακομιστές μεσολάβησης που χρησιμοποιούνται από άτομα που θέλετε να αποκρύψετε τη διεύθυνση IP της συσκευής τους και μπορούν να χρησιμοποιηθούν για κακόβουλες επιθέσεις.

**Για να προσομοιώσετε μια εισόδου από μια ανώνυμη IP, ακολουθήστε τα παρακάτω βήματα**:

1.  Κάντε λήψη του [Προγράμματος περιήγησης Tor](https://www.torproject.org/projects/torbrowser.html.en).
2.  Χρήση του προγράμματος περιήγησης Tor, μεταβείτε [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3.  Εισαγάγετε τα διαπιστευτήρια του λογαριασμού που θέλετε να εμφανίζεται στην αναφορά **εισόδου πρόσθετα από ανώνυμου διευθύνσεις IP** .

Η είσοδος θα εμφανίζεται στον πίνακα εργαλείων προστασία ταυτότητας σε 5 λεπτά. 


###<a name="sign-ins-from-unfamiliar-locations"></a>Σύμβολο πρόσθετα από άγνωστα θέσεις

Ο κίνδυνος άγνωστα θέσεις είναι ένας μηχανισμός αξιολόγησης σε πραγματικό χρόνο εισόδου που θεωρεί πέρα από τις θέσεις εισόδου (IP, γεωγραφικό πλάτος / γεωγραφικού πλάτους και μήκους ASN) για να καθορίσετε νέα / άγνωστα θέσεις. Το σύστημα αποθηκεύει προηγούμενο διευθύνσεις IP, γεωγραφικό πλάτος / μήκος και ASNs ενός χρήστη και λαμβάνει υπόψη τα εξής για να εξοικειωθείτε θέσεις. Μια θέση εισόδου θεωρείται Άγνωστα, εάν η θέση εισόδου δεν ταιριάζουν με οποιαδήποτε από τις υπάρχουσες γνωστές θέσεις.

Προστασία ταυτότητας καταλόγου Azure Active Directory:  

 - έχει μια περίοδο εκμάθησης αρχική 14 ημέρες κατά τις οποίες δεν επισημαίνει νέες θέσεις ως άγνωστα θέσεις.
 - παραβλέπει το όρισμα εισόδου πρόσθετα από γνωστές συσκευές και τις θέσεις που είναι γεωγραφικά σε μια υπάρχουσα τοποθεσία εξοικειωμένοι.

Για να προσομοιώσετε άγνωστα θέσεις, πρέπει να συνδεθείτε στο από μια θέση και συσκευή με το λογαριασμό έχει δεν έχετε συνδεθεί από πριν. 


**Για να προσομοιώσετε μια εισόδου από μια θέση άγνωστα, ακολουθήστε τα παρακάτω βήματα**:

1.  Επιλέξτε ένα λογαριασμό που έχει τουλάχιστον ένα 14 ημερών εισόδου στο ιστορικό. 

2.  Κάντε ένα:
    
    μια. Κατά τη χρήση του VPN, μεταβείτε στο [https://myapps.microsoft.com](https://myapps.microsoft.com) και πληκτρολογήστε τα διαπιστευτήρια του λογαριασμού που θέλετε να προσομοιώσετε το συμβάν κινδύνου για.

    β. Ζητήστε από ένα συνεργάτη σε διαφορετική θέση για να πραγματοποιήσετε είσοδο χρησιμοποιώντας τα διαπιστευτήρια του λογαριασμού (δεν συνιστάται).

Η είσοδος θα εμφανίζεται στον πίνακα εργαλείων προστασία ταυτότητας σε 5 λεπτά.
 
### <a name="impossible-travel-to-atypical-location"></a>Αδύνατη ταξίδια ασυνήθιστης θέση
Προσομοίωση τη συνθήκη αδύνατη ταξίδια είναι δύσκολη, επειδή ο αλγόριθμος χρησιμοποιεί μηχανικής εκμάθησης για να weed ανάληψη false-θετικά όπως αδύνατη ταξίδια από γνωστές συσκευές ή πρόσθετα εισόδου από VPN που χρησιμοποιούνται από άλλους χρήστες στον κατάλογο. Επιπλέον, ο αλγόριθμος απαιτεί ένα ιστορικό εισόδου 3 έως 14 ημερών για το χρήστη πριν να ξεκινήσει η δημιουργία συμβάντα κινδύνου.

**Για να προσομοιώσετε μια αδύνατη ταξίδια ασυνήθιστης θέση, ακολουθήστε τα παρακάτω βήματα**:

1.  Χρησιμοποιώντας το τυπικό πρόγραμμα περιήγησης, μεταβείτε [https://myapps.microsoft.com](https://myapps.microsoft.com).  

2.  Εισαγάγετε τα διαπιστευτήρια του λογαριασμού που θέλετε να δημιουργήσετε ένα συμβάν κινδύνου αδύνατη ταξίδια για.

3.  Αλλάξτε τον παράγοντα χρήστη. Μπορείτε να αλλάξετε παράγοντα χρήστη στον Internet Explorer από τα εργαλεία για προγραμματιστές ή να αλλάξετε τον παράγοντα χρήστη στο Firefox ή το Chrome χρησιμοποιώντας ένα πρόσθετο κουμπί εναλλαγής παράγοντα χρήστη.

4.  Αλλαγή της διεύθυνσης IP. Μπορείτε να αλλάξετε τη διεύθυνση IP σας χρησιμοποιώντας ένα VPN, ένα πρόσθετο Tor, ή περιστροφής έναν νέο υπολογιστή στο Azure σε ένα κέντρο διαφορετικά δεδομένα.

5.  Είσοδος για να [https://myapps.microsoft.com](https://myapps.microsoft.com) χρησιμοποιώντας τα ίδια διαπιστευτήρια όπως και πριν και μετά από μερικά λεπτά μετά το προηγούμενο εισόδου.

Η είσοδος θα εμφανίζεται στον πίνακα εργαλείων προστασία ταυτότητας μέσα σε 2-4 ώρες.<br>
Επειδή τα σύνθετα μηχανικής εκμάθησης μοντέλων που περιλαμβάνονται, υπάρχει ευκαιρία δεν θα λάβετε επιλέξατε προς τα επάνω.<br> Ενδέχεται να θέλετε να αναπαραγάγετε αυτά τα βήματα για πολλούς λογαριασμούς Azure AD.


## <a name="simulating-vulnerabilities"></a>Προσομοίωση ευπάθειες 
Ευπάθειες είναι μειονεκτήματα σε ένα περιβάλλον Azure AD που μπορούν να χρησιμοποιηθούν από έναν παράγοντα ακατάλληλη. Προς το παρόν 3 τύπους ευπάθειες είναι ολοκλήρωση της προετοιμασίας στην προστασία Azure AD ταυτότητας που αξιοποιήσετε άλλες δυνατότητες του Azure AD. Αυτά τα θέματα ευπάθειας θα εμφανίζονται στον πίνακα εργαλείων προστασία ταυτότητας αυτόματα αφού ορίσετε αυτές τις δυνατότητες.

-   Azure AD [έλεγχο ταυτότητας πολλών παραγόντων;](../multi-factor-authentication/multi-factor-authentication.md)
-   Azure AD [Cloud εφαρμογή εντοπισμού](active-directory-cloudappdiscovery-whatis.md).
-   Azure AD [διαχείρισης των ταυτοτήτων Προνομιούχους](active-directory-privileged-identity-management-configure.md). 



##<a name="user-compromise-risk"></a>Χρήστης παραβίαση του κινδύνου

**Για να ελέγξετε τον κίνδυνο παραβίαση του χρήστη, εκτελέστε τα ακόλουθα βήματα**:

1.  Είσοδος στο [https://portal.azure.com](https://portal.azure.com) με διαπιστευτήρια καθολικός διαχειριστής για το μισθωτή.

2.  Μεταβείτε στην **προστασία ταυτότητας**. 

3.  Το κύριο blade **προστασία Azure AD ταυτότητας** , επιλέξτε **Ρυθμίσεις**. 

4.  Στην το blade **Ρυθμίσεις πύλης** , στην περιοχή **κανόνες ασφαλείας**, κάντε κλικ **χρήστη υπονομεύσει κινδύνου**. 

5.  Στην blade **Είσοδος κινδύνου** , απενεργοποίηση **Ενεργοποίηση του κανόνα** και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση** ρυθμίσεων.

6.  Για ένα λογαριασμό χρήστη που δίνεται, προσομοιώνουν μια άγνωστα θέσεις ή ένα συμβάν κινδύνου ανώνυμου διευθύνσεων IP. Αυτό θα αναβάθμιση **Μεσαίο**το επίπεδο κινδύνου χρήστη για τον συγκεκριμένο χρήστη.

7.  Περιμένετε λίγα λεπτά και, στη συνέχεια, βεβαιωθείτε ότι το επίπεδο χρήστη για το χρήστη είναι **Μεσαίο**.

8.  Μεταβείτε στο το blade **Ρυθμίσεις πύλης** .

9.  Στην blade του **Κινδύνου παραβίαση του χρήστη** , στην ενότητα **Ενεργοποίηση του κανόνα**, επιλέξτε **σε** . 

10. Επιλέξτε μία από τις ακόλουθες επιλογές:

    μια. Για να αποκλείσετε, επιλέξτε **Μεσαίο** στην περιοχή **Είσοδος μπλοκ**.

    β. Για να επιβάλετε αλλαγή ασφαλούς κωδικού πρόσβασης, επιλέξτε **Μεσαίο** στην περιοχή **απαιτείται έλεγχος ταυτότητας πολλών παραγόντων**.

13. Κάντε κλικ στην επιλογή **Αποθήκευση**.

14. Τώρα, μπορείτε να ελέγξετε πρόσβασης υπό όρους που βασίζεται σε κίνδυνο πραγματοποιώντας είσοδο στη χρήση ενός χρήστη με αναβαθμισμένα δικαιώματα κινδύνου επίπεδο. Εάν ο κίνδυνος χρήστη είναι Μεσαίο, ανάλογα με τη ρύθμιση παραμέτρων της πολιτικής σας, το εισόδου είναι είτε έχει αποκλειστεί ή είστε υποχρεωμένοι να αλλάξετε τον κωδικό πρόσβασης. 
<br><br>
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
<br>

 
##<a name="sign-in-risk"></a>Είσοδος κινδύνου

 
**Για να δοκιμάσετε ένα σύμβολο σε κινδύνου, εκτελέστε τα ακόλουθα βήματα:**

1.  Είσοδος στο [https://portal.azure.com](https://portal.azure.com) με διαπιστευτήρια καθολικός διαχειριστής για το μισθωτή.

2.  Μεταβείτε στην **προστασία ταυτότητας**.

3.  Το κύριο blade **προστασία Azure AD ταυτότητας** , επιλέξτε **Ρυθμίσεις**. 

4.  Στην το blade **Ρυθμίσεις πύλης** , στην περιοχή **κανόνες ασφαλείας**, επιλέξτε **Είσοδος κινδύνου**.

5.  Στην blade **Είσοδος κινδύνου **, επιλέξτε **σε** στην ενότητα **Ενεργοποίηση του κανόνα**. 

7.  Επιλέξτε μία από τις ακόλουθες επιλογές:

    μια. Για να αποκλείσετε, επιλέξτε **Μεσαίο** στην περιοχή **Είσοδος μπλοκ**

    β. Για να επιβάλετε αλλαγή ασφαλούς κωδικού πρόσβασης, επιλέξτε **Μεσαίο** στην περιοχή **απαιτείται έλεγχος ταυτότητας πολλών παραγόντων**.

8.  Για να αποκλείσετε, επιλέξτε Μεσαίο στην περιοχή αποκλεισμός είσοδος.

9.  Για να επιβάλετε έλεγχο ταυτότητας πολλών παραγόντων, επιλέξτε **Μεσαίο** στην περιοχή **απαιτείται έλεγχος ταυτότητας πολλών παραγόντων**.

10. Κάντε κλικ στην εντολή **Αποθήκευση**.

11. Τώρα, μπορείτε να ελέγξετε πρόσβασης υπό όρους που βασίζεται σε κίνδυνο με προσομοίωση τις άγνωστα τοποθεσίες ή συμβάντα κινδύνου ανώνυμου IP, επειδή είναι και τα δύο συμβάντα κινδύνου **Μεσαίο** .

<br>
![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")
<br>


## <a name="see-also"></a>Δείτε επίσης

 - [Προστασία ταυτότητας καταλόγου Azure Active Directory](active-directory-identityprotection.md)
