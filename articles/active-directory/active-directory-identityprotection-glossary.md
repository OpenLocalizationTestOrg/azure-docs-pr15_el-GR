<properties
    pageTitle="Γλωσσάρι προστασία ταυτότητας καταλόγου Azure Active Directory | Microsoft Azure"
    description="Γλωσσάρι προστασία ταυτότητας καταλόγου Azure Active Directory"
    services="active-directory"
    keywords="προστασία ταυτότητας καταλόγου Azure active directory, εντοπισμού εφαρμογή cloud, τη Διαχείριση εφαρμογών, ασφαλείας, κινδύνου, επίπεδο κινδύνου, ευπάθεια, πολιτική ασφαλείας, Γλωσσάρι"
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
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-identity-protection-glossary"></a>Γλωσσάρι προστασία ταυτότητας καταλόγου Azure Active Directory 


### <a name="at-risk-user"></a>Σε κίνδυνο (χρήστη)  
Ένας χρήστης με ένα ή περισσότερα συμβάντα ενεργός κίνδυνος. 

### <a name="atypical-sign-in-location"></a>Θέση ασυνήθιστης εισόδου   
Μια εισόδου από μια γεωγραφική θέση που δεν είναι τυπικές για το συγκεκριμένο χρήστη, παρόμοια χρήστες ή μισθωτή.

### <a name="azure-ad-identity-protection"></a>Προστασία Azure AD ταυτότητας    
Μια λειτουργική μονάδα ασφαλείας του Azure Active Directory που παρέχει μια ενοποιημένη προβολή σε συμβάντα κινδύνου και ενδεχόμενες ευπάθειες που επηρεάζουν τις ταυτότητες του οργανισμού.

### <a name="conditional-access"></a>Πρόσβαση υπό όρους  
Μια πολιτική για την ασφάλεια των πρόσβασης σε πόρους. Κανόνες υπό όρους πρόσβασης αποθηκεύονται στο Azure Active Directory και αξιολογούνται με Azure AD πριν από την εκχώρηση πρόσβασης στον πόρο.  Παράδειγμα κανόνων περιλαμβάνουν περιορισμός πρόσβασης τοποθεσίας χρήστη, με βάση τη συσκευή μεθόδου ελέγχου ταυτότητας χρηστών ή εύρυθμης λειτουργίας.

### <a name="credentials"></a>Τα διαπιστευτήρια     
Πληροφορίες που περιλαμβάνει αναγνώριση και απόδειξη αναγνώρισης που χρησιμοποιείται για να αποκτήσετε πρόσβαση σε τοπική και πόρους δικτύου. Παραδείγματα διαπιστευτηρίων είναι τα ονόματα χρηστών και κωδικούς πρόσβασης, έξυπνες κάρτες και πιστοποιητικά.

### <a name="event"></a>Συμβάν   
Μια εγγραφή δραστηριότητας στο Azure Active Directory.

### <a name="false-positive-risk-event"></a>Ψευδές θετικό (κίνδυνος συμβάν) 
Μια κατάσταση συμβάν κινδύνου οριστεί με μη αυτόματο τρόπο από ένα χρήστη προστασία ταυτότητας, που υποδεικνύει ότι το συμβάν κινδύνου ήταν διερεύνηση και λάθος έχει επισημανθεί ως κινδύνου.

### <a name="identity"></a>Ταυτότητα    
Ένα πρόσωπο ή οντότητα που πρέπει να έχει επαληθευτεί με έλεγχο ταυτότητας, με βάση κριτήρια όπως ο κωδικός πρόσβασης ή ένα πιστοποιητικό.

### <a name="identity-risk-event"></a>Ταυτότητα κινδύνου συμβάντος     
Συμβάν AAD που έχει επισημανθεί ως ύπαρξη με προστασία ταυτότητας και μπορεί να υποδεικνύουν ότι έχει παραβιαστεί μια ταυτότητα.

### <a name="ignored-risk-event"></a>Παραβλέπεται (κίνδυνος συμβάν)    
Μια κατάσταση συμβάν κινδύνου οριστεί με μη αυτόματο τρόπο από ένα χρήστη προστασία ταυτότητας, που υποδεικνύει ότι το συμβάν κινδύνου είναι κλειστό χωρίς να κάνετε μια ενέργεια αποκατάσταση εύρυθμης λειτουργίας.

### <a name="impossible-travel-from-atypical-locations"></a>Αδύνατη ταξίδια από ασυνήθιστης θέσεις   
Ένα συμβάν κινδύνου ενεργοποιείται όταν εντοπιστούν δύο πρόσθετα εισόδου για τον ίδιο χρήστη, όπου τουλάχιστον μία από αυτές είναι από μια ασυνήθιστης θέση εισόδου και το σημείο όπου το χρονικό διάστημα μεταξύ τα πρόσθετα εισόδου είναι μικρότερη από τον ελάχιστο χρόνο, θα χρειαστεί να μεταβαίνουν φυσικά ανάμεσα σε αυτές τις τοποθεσίες.  

###<a name="investigation"></a>Έρευνα    
Η διαδικασία εξετάζει τις δραστηριότητες, αρχεία καταγραφής και άλλες σχετικές πληροφορίες που σχετίζονται με ένα συμβάν κίνδυνο για να αποφασίσετε εάν απαιτούνται βήματα αποκατάσταση εύρυθμης λειτουργίας ή μετριασμού, Κατανόηση εάν και πώς η ταυτότητα έχει παραβιαστεί, και να κατανοήσετε τον τρόπο που χρησιμοποιήθηκε η ταυτότητα έχει παραβιαστεί.

### <a name="leaked-credentials"></a>Διαρροής διαπιστευτήρια  

Ένα συμβάν κινδύνου ενεργοποιείται όταν βρεθούν διαπιστευτήρια του τρέχοντος χρήστη (όνομα χρήστη και κωδικός πρόσβασης) στο κοινό δημοσιευτεί σε σκούρο web από μας ερευνητές.

### <a name="mitigation"></a>Μετριασμού  
Μια ενέργεια για να περιορίσετε ή να αποκλείσετε τη δυνατότητα της ένας εισβολέας για την εκμετάλλευση μια ταυτότητα έχει παραβιαστεί ή συσκευή χωρίς να επαναφέρετε την ταυτότητα ή τη συσκευή σε ασφαλή κατάσταση. Μια μετριασμού δεν επιλύσει προηγούμενα κινδύνου συμβάντα που σχετίζονται με την ταυτότητα ή τη συσκευή.

### <a name="multi-factor-authentication"></a>Έλεγχος ταυτότητας πολλών παραγόντων 
Έχει μεθόδου ελέγχου ταυτότητας που απαιτεί δύο ή περισσότερες μεθόδους ελέγχου ταυτότητας, τα οποία μπορεί να περιλαμβάνουν κάτι από το χρήστη, όπως ένα πιστοποιητικό; κάτι που ο χρήστης γνωρίζει, όπως ονόματα χρηστών, κωδικούς πρόσβασης ή φράσεις πρόσβασης; φυσική χαρακτηριστικά, όπως μια αποτύπωση; και προσωπικά χαρακτηριστικά, όπως μια προσωπική υπογραφή.

### <a name="offline-detection"></a>Εντοπισμός για εργασία χωρίς σύνδεση   
Τον εντοπισμό ανωμαλίες και την αξιολόγηση του κινδύνου του συμβάντος, όπως είσοδος προσπάθεια μετά το γεγονός, για ένα συμβάν που έχει ήδη συμβεί.

### <a name="policy-condition"></a>Πολιτική συνθήκης    
Μέρος μια πολιτική ασφαλείας που ορίζει το οντοτήτων (ομάδες, χρήστες, εφαρμογές, πλατφόρμες συσκευών, μέλη συσκευή, περιοχές διευθύνσεων IP, τύπων προγράμματος-πελάτη) την πολιτική συμπεριλαμβάνονται ή εξαιρούνται από αυτό.

### <a name="policy-rule"></a>Πολιτική κανόνα     
Το τμήμα της μια πολιτική ασφαλείας που περιγράφει τις συνθήκες που θα προκαλέσει την πολιτική και τις ενέργειες που λαμβάνονται κατά την ενεργοποίηση της πολιτικής.

### <a name="prevention"></a>Αποτροπή  
Μια ενέργεια για να αποτραπεί βλάβη στον οργανισμό μέσω κατάχρηση ταυτότητας ή συσκευή ύποπτα ή ότι πρέπει να είναι έχει παραβιαστεί. Μια ενέργεια αποτροπής δεν ασφαλούς στη συσκευή ή ταυτότητα και δεν επιλύσει προηγούμενα συμβάντα κινδύνου.

### <a name="privileged-user"></a>Δικαιώματα (χρήστη)
Ένας χρήστης που είχαν δικαιώματα διαχειριστή μόνιμων και προσωρινών σε μία ή περισσότερες πόρο Azure Active Directory, όπως ο Καθολικός διαχειριστής, τη στιγμή της ένα συμβάν κινδύνου, διαχειριστής χρεώσεων, διαχειριστή της υπηρεσίας, το διαχειριστή του χρήστη και τον κωδικό πρόσβασης διαχειριστή. 

###<a name="real-time"></a>Σε πραγματικό χρόνο    
Ανατρέξτε στο θέμα εντοπισμός σε πραγματικό χρόνο.

###<a name="real-time-detection"></a>Εντοπισμός σε πραγματικό χρόνο  
Επιτρέπεται να συνεχίσετε την ανίχνευση ανωμαλίες και την αξιολόγηση του κινδύνου του συμβάντος, όπως είσοδος προσπάθεια πριν από το συμβάν.

### <a name="remediated-risk-event"></a>Remediated (κίνδυνος συμβάν)     
Μια κατάσταση συμβάν κινδύνου που ορίζονται αυτόματα από την προστασία ταυτότητας, που υποδεικνύει ότι το συμβάν κινδύνου ήταν remediated χρησιμοποιώντας την τυπική αποκατάσταση εύρυθμης λειτουργίας ενέργεια για αυτόν τον τύπο του κινδύνου. Για παράδειγμα, όταν γίνει επαναφορά του κωδικού πρόσβασης χρήστη, πολλά συμβάντα κινδύνου που υποδεικνύουν ότι έχει παραβιαστεί προηγούμενο κωδικό πρόσβασης αυτόματα remediated.

### <a name="remediation"></a>Αποκατάσταση εύρυθμης λειτουργίας     
Μια ενέργεια για να ασφαλίσετε μια ταυτότητα ή σε μια συσκευή που ήταν προηγουμένως ύποπτα ή γνωστά να παραβιαστεί. Μια ενέργεια αποκατάσταση εύρυθμης λειτουργίας επαναφέρει την ταυτότητα ή τη συσκευή σε κατάσταση ασφαλείς και επιλύει προηγούμενα κινδύνου συμβάντα που σχετίζονται με την ταυτότητα ή τη συσκευή.

### <a name="resolved-risk-event"></a>Επιλυθεί (κίνδυνος συμβάν)   
Μια κατάσταση συμβάν κίνδυνο οριστεί με μη αυτόματο τρόπο από ένα χρήστη προστασία ταυτότητας, που υποδεικνύει ότι ο χρήστης έλαβε μια ενέργεια κατάλληλη αποκατάσταση εύρυθμης λειτουργίας εκτός προστασία ταυτότητας και ότι θα πρέπει να θεωρούνται το συμβάν κίνδυνο κλειστό.

###<a name="risk-event-status"></a>Κατάσταση συμβάν κινδύνου    
Μια ιδιότητα του κινδύνου, που υποδεικνύει εάν το συμβάν είναι ενεργή και, εάν κλείσει, ο λόγος για το κλείσιμο της.

###<a name="risk-event-type"></a>Τύπος συμβάντος κινδύνου  
Μια κατηγορία για το συμβάν κινδύνου, που υποδεικνύει τον τύπο ανωμαλία που προκάλεσαν το συμβάν πρέπει να θεωρούνται επικίνδυνη.

###<a name="risk-level-risk-event"></a>Επίπεδο κινδύνου (κίνδυνος συμβάν)  
Ένδειξη (υψηλό, μεσαίο ή χαμηλή) το της σοβαρότητας του κινδύνου συμβάντος για να βοηθά τους χρήστες προστασία ταυτότητας ιεράρχηση τις ενέργειες που ακολουθήσει για να μειωθεί ο κίνδυνος για την εταιρεία τους. 

###<a name="risk-level-sign-in"></a>Επίπεδο κινδύνου (είσοδος) 
Ένδειξη (υψηλό, μεσαίο ή χαμηλή) της πιθανότητας ότι για μια συγκεκριμένη εισόδου, κάποιος άλλος προσπαθεί να χρησιμοποιήσετε την ταυτότητα του χρήστη.

###<a name="risk-level-user-compromise"></a>Επίπεδο κινδύνου (παραβίαση χρήστη) 
Ένδειξη (υψηλό, μεσαίο ή χαμηλή) της πιθανότητας ότι έχει παραβιαστεί μια ταυτότητα.

### <a name="risk-level-vulnerability"></a>Επίπεδο κινδύνου (ευπάθεια)  
Ένδειξη (υψηλό, μεσαίο ή χαμηλή) της σοβαρότητας των την ευπάθεια βοηθά τους χρήστες προστασία ταυτότητας ιεράρχηση τις ενέργειες που ακολουθήσει για να μειωθεί ο κίνδυνος για την εταιρεία τους.

### <a name="secure-identity"></a>Ασφαλούς (ταυτότητα)   
Εκτέλεση ενεργειών αποκατάσταση εύρυθμης λειτουργίας όπως αλλαγή του κωδικού πρόσβασης ή μηχανική reimaging για να επαναφέρετε μια ταυτότητα ενδεχομένως έχει παραβιαστεί σε κατάσταση χωρίς συμβιβασμούς.

### <a name="security-policy"></a>Πολιτική ασφαλείας
Μια συλλογή των κανόνων πολιτικής και των συνθήκης. Μια πολιτική μπορούν να εφαρμοστούν σε οντοτήτων όπως χρήστες, ομάδες, εφαρμογές, συσκευές, πλατφόρμες συσκευών, συσκευή καταστάσεις, περιοχές διευθύνσεων IP και Auth2.0 τύπων προγράμματος-πελάτη. Όταν είναι ενεργοποιημένη μια πολιτική, αξιολογείται κάθε φορά που περιλαμβάνονται στην πολιτική οντότητα εκδίδεται ενός διακριτικού για έναν πόρο.

### <a name="sign-in-v"></a>Πραγματοποιήστε είσοδο στο (v) 
Για τον έλεγχο ταυτότητας για μια ταυτότητα στο Azure Active Directory.

### <a name="sign-in-n"></a>Είσοδος (n) 
Η διαδικασία ή ενέργεια της ταυτότητάς στο Azure Active Directory και το συμβάν που καταγράφει αυτήν τη λειτουργία.

###<a name="sign-in-from-anonymous-ip-address"></a>Είσοδος από ανώνυμη διεύθυνση IP    
Κινδύνου ενεργοποιείται μετά από μια επιτυχημένη εισόδου από τη διεύθυνση IP που έχει προσδιοριστεί ως μια διεύθυνση IP του διακομιστή μεσολάβησης ανώνυμα.

###<a name="sign-in-from-infected-device"></a>Είσοδος από μολυσμένη συσκευή 
Ένα συμβάν κινδύνου που ενεργοποιείται όταν ένα εισόδου που προέρχεται από μια διεύθυνση IP, το οποίο είναι γνωστό θα χρησιμοποιηθεί από ένα ή περισσότερα έχει παραβιαστεί συσκευές, οι οποίες ενεργά προσπαθεί να επικοινωνήσει με ένα διακομιστή bot.

###<a name="sign-in-from-ip-address-with-suspicious-activity"></a>Πραγματοποιήστε είσοδο από τη διεύθυνση IP με ύποπτες δραστηριότητας 
Κινδύνου ενεργοποιήσει μετά από μια επιτυχημένη εισόδου από ένα IP διευθύνσεων με μεγάλο αριθμό ανεπιτυχείς προσπάθειες σύνδεσης σε πολλαπλούς λογαριασμούς χρήστη σε ένα σύντομο χρονικό διάστημα.

###<a name="sign-in-from-unfamiliar-location"></a>Είσοδος από άγνωστα θέση 
Ένα συμβάν κίνδυνο που ενεργοποιείται όταν ένας χρήστης πραγματοποιεί με επιτυχία από μια νέα θέση (IP, γεωγραφικό πλάτος/μήκος και ASN).

###<a name="sign-in-risk"></a>Είσοδος κινδύνου     
Ανατρέξτε στο θέμα κινδύνου επίπεδο (είσοδος)

###<a name="sign-in-risk-policy"></a>Πολιτική εισόδου κινδύνου  
Μια πολιτική πρόσβασης υπό όρους που υπολογίζει τον κίνδυνο σε ένα συγκεκριμένο εισόδου και ισχύει μετριασμούς βάση προκαθορισμένες συνθήκες και κανόνων.

###<a name="user-compromise-risk"></a>Χρήστης παραβίαση του κινδύνου     
Ανατρέξτε στο θέμα κινδύνου επίπεδο (παραβίαση χρήστη)

###<a name="user-risk"></a>Κινδύνου χρήστη    
Ανατρέξτε στο θέμα κινδύνου επίπεδο (παραβίαση χρήστη).

###<a name="user-risk-policy"></a>Πολιτική κινδύνου χρήστη     
Μια πολιτική πρόσβασης υπό όρους που θεωρεί ότι η είσοδος και ισχύει μετριασμούς βάση προκαθορισμένων συνθηκών και κανόνων.

###<a name="users-flagged-for-risk"></a>Οι χρήστες με σημαία για κινδύνου   
Οι χρήστες που έχουν συμβάντα κινδύνου που είναι ενεργοποιημένο ή remediated

###<a name="vulnerability"></a>Ευπάθεια    
Μια ρύθμιση παραμέτρων ή στο Azure Active Directory, οπότε στον κατάλογο ευαίσθητων εκμεταλλεύεται ή απειλές.


## <a name="see-also"></a>Δείτε επίσης

- [Προστασία ταυτότητας καταλόγου Azure Active Directory](active-directory-identityprotection.md)