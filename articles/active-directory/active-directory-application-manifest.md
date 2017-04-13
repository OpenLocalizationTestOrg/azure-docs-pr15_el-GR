<properties
   pageTitle="Κατανόηση του καταλόγου Azure Active Directory δήλωση εφαρμογής | Microsoft Azure"
   description="Λεπτομερείς κάλυψης του Azure Active Directory εφαρμογής, το οποίο αντιπροσωπεύει μια εφαρμογή ταυτότητας παραμέτρους σε ένα μισθωτή του Azure AD και χρησιμοποιείται για τη διευκόλυνση της εξουσιοδότησης διακριτικό, συγκατάθεση εμπειρία και άλλα."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>Κατανόηση της δήλωσης εφαρμογή Azure Active Directory

Εφαρμογές που ενοποιούνται με το Azure Active Directory (AD) πρέπει να έχει εγγραφεί σε ένα μισθωτή του Azure AD, παρέχοντας μια ρύθμιση παραμέτρων μόνιμη ταυτότητας για την εφαρμογή. Αυτή η ρύθμιση παραμέτρων είναι τη γνώμη κατά το χρόνο εκτέλεσης, ενεργοποίηση σεναρίων που επιτρέπουν σε μια εφαρμογή για να αναθέσετε εξωτερικό και broker ελέγχου ταυτότητας/εξουσιοδότησης μέσω Azure AD. Για περισσότερες πληροφορίες σχετικά με το μοντέλο Azure AD εφαρμογών, ανατρέξτε στο θέμα η [Προσθήκη, ενημέρωση, και κατάργηση μιας εφαρμογής] [ ADD-UPD-RMV-APP] το άρθρο.

## <a name="updating-an-applications-identity-configuration"></a>Ενημέρωση ρυθμίσεων παραμέτρων μιας εφαρμογής ταυτότητας

Στην πραγματικότητα πολλές επιλογές είναι διαθέσιμες για την ενημέρωση τις ιδιότητες σε μια εφαρμογή ταυτότητας ρύθμιση παραμέτρων, οι οποίες διαφέρουν όσον αφορά δυνατότητες και μοίρες δυσκολίας, συμπεριλαμβανομένων των εξής:

- Το ** [Azure κλασική της πύλης] [ AZURE-CLASSIC-PORTAL] περιβάλλον εργασίας χρήστη του Web** σάς επιτρέπει να ενημερώνετε τις πιο κοινές ιδιότητες μιας εφαρμογής του. Τρόπος είναι ο πιο γρήγορος και τουλάχιστον σφάλματος ευνοεί ενημέρωσης ιδιοτήτων της εφαρμογής σας, αλλά δεν σας παρέχει πλήρη πρόσβαση σε όλες τις ιδιότητες, όπως τις επόμενες δύο μεθόδους.
- Για πιο σύνθετες σενάρια όπου πρέπει να ενημερώσετε τις ιδιότητες που δεν είναι εκτεθειμένη στην πύλη του Azure κλασική, μπορείτε να τροποποιήσετε το **δήλωσης εφαρμογή**. Αυτή είναι η εστίαση αυτού του άρθρου και εξετάζεται με περισσότερες λεπτομέρειες ξεκινώντας στην επόμενη ενότητα.
- Επίσης, είναι πιθανό να **γράψετε μια εφαρμογή που χρησιμοποιεί το [Graph API] [ GRAPH-API] ** για να ενημερώσετε την εφαρμογή σας, που απαιτεί η πιο προσπάθειας. Αυτό μπορεί να μια ελκυστική επιλογή Παρόλο που η, αν συντάσσετε λογισμικό διαχείρισης, ή πρέπει να ενημερώσετε τις ιδιότητες της εφαρμογής σε τακτική βάση με ένα αυτοματοποιημένο τρόπο.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Χρησιμοποιώντας τη δήλωση της εφαρμογής για να ενημερώσετε μια εφαρμογή ρύθμισης παραμέτρων ταυτότητας
Μέσω της [πύλης Azure κλασική][AZURE-CLASSIC-PORTAL], μπορείτε να διαχειριστείτε τη ρύθμιση παραμέτρων ταυτότητα της εφαρμογής σας, κάνοντας λήψη και αποστολή ενός JSON αρχείων αναπαράστασης, η οποία ονομάζεται δήλωση εφαρμογής. Δεν υπάρχει πραγματική αρχείο αποθηκεύεται στον κατάλογο. Η δήλωση εφαρμογής είναι απλώς μια HTTP GET λειτουργία στην εφαρμογή API Azure AD Graph οντότητα και η αποστολή είναι μια λειτουργία HTTP ΚΏΔΙΚΑ στην εφαρμογή οντότητα.

Ως αποτέλεσμα, προκειμένου να κατανοήσετε τη μορφοποίηση και τις ιδιότητες τη δήλωση της εφαρμογής, θα πρέπει να αναφέρονται το Graph API [οντότητα εφαρμογής] [ APPLICATION-ENTITY] τεκμηρίωση. Ενημερώσεις που μπορεί να εκτελεστεί αν και εφαρμογή δήλωσης αποστολής παραδείγματα:

- **Declare δικαιωμάτων εύρους (oauth2Permissions)** που εκτίθεται από το API web. Ανατρέξτε στο θέμα "Εκθέσετε APIs σε άλλες εφαρμογές Web" σε [Εφαρμογές ενοποίηση με το Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] για πληροφορίες σχετικά με την εφαρμογή απομίμησης χρήστη χρησιμοποιώντας το oauth2Permissions πληρεξούσιοι εμβέλειας δικαιωμάτων. Όπως προαναφέρθηκε, ιδιότητες οντότητα εφαρμογής τεκμηριώνονται στο γράφημα API [οντότητα και σύνθετο τύπο] [ APPLICATION-ENTITY] άρθρο αναφοράς, συμπεριλαμβανομένης της ιδιότητας oauth2Permissions η οποία είναι μια συλλογή του τύπου [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
- **Declare ρόλοι της εφαρμογής (appRoles) που εκτίθεται από την εφαρμογή σας**. Η εφαρμογή του οντότητα appRoles ιδιότητα είναι μια συλλογή του τύπου [AppRole][APPLICATION-ENTITY-APP-ROLE]. Ανατρέξτε στο θέμα το [Έλεγχος πρόσβασης στο cloud εφαρμογών χρησιμοποιώντας το Azure AD βάσει ρόλων] [ RBAC-CLOUD-APPS-AZUREAD] άρθρου, για παράδειγμα υλοποίηση.
- **Declare γνωστό προγράμματος-πελάτη εφαρμογές (knownClientApplications)**, που σας επιτρέπουν να λογικά συνδέουν τη συγκατάθεση από τις εφαρμογές καθορισμένο προγράμματος-πελάτη για το API πόρων/web.
- **Αίτηση Azure AD για την έκδοση διεκδίκηση μελών ομάδας** για την είσοδο στο χρήστη (groupMembershipClaims).  Αυτό μπορεί να ρυθμιστεί επίσης την έκδοση αξιώσεων σχετικά με τις ιδιότητες μελών ρόλων καταλόγου του χρήστη. Ανατρέξτε στο θέμα το [εξουσιοδότησης στο Cloud εφαρμογές χρησιμοποιώντας ομάδες AD] [ AAD-GROUPS-FOR-AUTHORIZATION] άρθρου, για παράδειγμα υλοποίηση.
- **Να επιτρέπεται την εφαρμογή σας για την υποστήριξη διακριτικό 2.0 έμμεσα εκχώρηση** ροών (oauth2AllowImplicitFlow). Αυτός ο τύπος της ροής εκχώρηση χρησιμοποιείται με ενσωματωμένο ιστοσελίδες JavaScript ή μία μόνο σελίδα εφαρμογές (SPA). Για περισσότερες πληροφορίες σχετικά με την μη ρητή άδεια εκχώρηση, ανατρέξτε στο θέμα [Κατανόηση του έμμεσα OAuth2 εκχώρηση ροής στο Azure Active Directory][IMPLICIT-GRANT].
- **Χρήση ενεργοποίηση των X509 πιστοποιητικά ως το μυστικό κλειδί** (keyCredentials). Ανατρέξτε στο θέμα [Δημιουργία εφαρμογών υπηρεσίας και μονού στο Office 365] [ O365-SERVICE-DAEMON-APPS] και [για προγραμματιστές του οδηγού για έλεγχο ταυτότητας με το API διαχείρισης πόρων Azure] [ DEV-GUIDE-TO-AUTH-WITH-ARM] άρθρα για να δείτε παραδείγματα υλοποίηση.
- **Προσθήκη ενός νέου URI Αναγνωριστικό εφαρμογής** για την εφαρμογή σας (identifierURIs[]). Εφαρμογή Αναγνωριστικό URI είναι που χρησιμοποιούνται για να προσδιορίσει μοναδικά μια εφαρμογή εντός του μισθωτή Azure AD (ή σε πολλούς μισθωτές του Azure AD, για σενάρια πολλών μισθωτών όταν προσδιορισμένο μέσω επαληθευτεί προσαρμοσμένου τομέα). Χρησιμοποιούνται κατά την αίτηση δικαιώματα για μια εφαρμογή πόρων ή κατά τη λήψη ένα διακριτικό πρόσβασης για μια εφαρμογή του πόρου. Όταν ενημερώνετε αυτό το στοιχείο, πραγματοποιείται η ίδια ενημερωμένη έκδοση προς το αντίστοιχο κεφάλαιο υπηρεσία servicePrincipalNames [] συλλογή, η οποία βρίσκεται στο μισθωτή αρχική της εφαρμογής.

Η δήλωση εφαρμογής παρέχει επίσης ένας καλός τρόπος για να παρακολουθείτε την κατάσταση του την εγγραφή σας εφαρμογής. Επειδή είναι διαθέσιμα σε μορφή JSON, μπορεί να ελεγχθεί η αναπαράσταση αρχείου σε στοιχείο ελέγχου του αρχείου προέλευσης, μαζί με πηγαίο κώδικα της εφαρμογής σας.

## <a name="step-by-step-example"></a>Βήμα με βάση ένα παράδειγμα βήμα
Τώρα σας επιτρέπει να δείτε τα βήματα που απαιτούνται για την ενημέρωση της εφαρμογής σας ταυτότητα ρύθμισης παραμέτρων έως τη δήλωση της εφαρμογής. Ένα από τα προηγούμενα παραδείγματα, θα σας θα το επισημάνετε που δείχνει τον τρόπο για να δηλώσετε ένα νέο πεδίο δικαιωμάτων σε μια εφαρμογή πόρων:

1. Μεταβείτε στην [πύλη του Azure κλασική] [ AZURE-CLASSIC-PORTAL] και συνδεθείτε με ένα λογαριασμό που έχει δικαιώματα διαχειριστή ή από κοινού διαχειριστή υπηρεσίας.

2. Αφού που έχετε με έλεγχο ταυτότητας, κάντε κύλιση προς τα κάτω και επιλέξτε την επέκταση Azure "υπηρεσία καταλόγου Active Directory" στον πίνακα αριστερό παράθυρο περιήγησης (1), στη συνέχεια, κάντε κλικ στο μισθωτή του Azure AD πού βρίσκεται η εφαρμογή σας καταχωρήσει (2).

    ![Επιλέξτε το μισθωτή Azure AD][SELECT-AZURE-AD-TENANT]

3. Όταν εμφανιστεί η σελίδα καταλόγου, κάντε κλικ στην επιλογή "Εφαρμογές" (1) στο επάνω μέρος της σελίδας για να δείτε μια λίστα των εφαρμογών που έχουν καταχωρηθεί στο μισθωτή. Στη συνέχεια, βρείτε την εφαρμογή που θέλετε να ενημερώσετε στη λίστα και κάντε κλικ σε αυτό (2).

    ![Επιλέξτε το μισθωτή Azure AD][SELECT-AZURE-AD-APP]

4. Τώρα που έχετε επιλέξει κύρια σελίδα της εφαρμογής, παρατηρήστε ότι η δυνατότητα "Διαχείριση δήλωσης" στο κάτω μέρος της σελίδας (1). Εάν κάνετε κλικ σε αυτήν τη σύνδεση, θα σας ζητηθεί για να κάνετε λήψη ή να αποστείλετε το αρχείο δήλωσης JSON. Κάντε κλικ στην επιλογή "Λήψη δηλωτικό" (2). Αυτό θα αμέσως ακολουθείται με το παράθυρο διαλόγου επιβεβαίωσης λήψης που σας ζητά να επιβεβαιώσετε κάνοντας κλικ στην επιλογή "Λήψη δήλωσης" (3), στη συνέχεια, ανοίξτε οποιαδήποτε ή αποθηκεύστε το αρχείο τοπικά (4).

    ![Διαχειριστείτε το δηλωτικό, κάντε λήψη επιλογή][MANAGE-MANIFEST-DOWNLOAD]

    ![Κάντε λήψη της δήλωσης][DOWNLOAD-MANIFEST]

5. Σε αυτό το παράδειγμα, θα σας αποθηκεύσατε το αρχείο τοπικά, επιτρέποντάς μας για να ανοίξετε σε ένα πρόγραμμα επεξεργασίας, κάνετε αλλαγές σε το JSON και αποθηκεύστε ξανά. Εδώ είναι η εμφάνιση της δομής JSON εντός της επεξεργασίας της Visual Studio JSON. Σημείωση που ακολουθεί το σχήμα για την [εφαρμογή οντότητα] [ APPLICATION-ENTITY] όπως αναφέρθηκε προηγουμένως:

    ![Ενημερώστε το δήλωσης JSON][UPDATE-MANIFEST]

    Για παράδειγμα, υπό την προϋπόθεση ότι θέλουμε να υλοποίηση/έκθεση ένα νέο δικαίωμα που ονομάζεται "Employees.Read.All" στην εφαρμογή πόρων (API), απλώς προσθέστε ένα νέο/δεύτερο στοιχείο στη συλλογή oauth2Permissions, ie:

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    Η καταχώρηση πρέπει να είναι μοναδικό και, επομένως, πρέπει να δημιουργείτε ένα νέο καθολικό μοναδικό Αναγνωριστικό (GUID) για το `"id"` την ιδιότητα. Σε αυτήν την περίπτωση, επειδή θα σας καθοριστεί `"type": "User"`, αυτό το δικαίωμα μπορεί να δεχθεί να από οποιονδήποτε λογαριασμό έλεγχο ταυτότητας από το Azure AD μισθωτή στο οποίο έχει καταχωρηθεί η εφαρμογή/API του πόρου. Αυτό εκχωρεί το πρόγραμμα-πελάτη εφαρμογής δικαίωμα πρόσβασης εκ μέρους του λογαριασμού. Οι συμβολοσειρές όνομα περιγραφή και εμφάνιση χρησιμοποιούνται κατά τη διάρκεια συγκατάθεση και για εμφάνιση στην πύλη του Azure κλασική.

6. Όταν ολοκληρώσετε την ενημέρωση της δήλωσης, επιστρέψτε στη σελίδα εφαρμογής Azure AD στην πύλη του Azure κλασική και κάντε κλικ στην επιλογή "Διαχείριση δήλωσης" δυνατότητας ξανά (1). Αλλά αυτήν τη φορά, ενεργοποιήστε την επιλογή "Αποστολή δήλωσης" (2). Παρόμοια με τη λήψη, που θα εμφανιστεί ξανά με ένα δεύτερο παράθυρο διαλόγου, που σας ρωτά για τη θέση του αρχείου JSON. Κάντε κλικ στο κουμπί "Αναζήτηση αρχείου..." (3), στη συνέχεια, χρησιμοποιήστε το παράθυρο διαλόγου "Επιλογή αρχείου για Αποστολή" για να επιλέξετε το αρχείο JSON (4) και πατήστε "Άνοιγμα". Μόλις κλείσει το παράθυρο διαλόγου δεν βρίσκομαι στον υπολογιστή, επιλέξτε το σημάδι ελέγχου "OK" (5) και σας δηλωτικό θα αποσταλεί.  

    ![Διαχειριστείτε το δηλωτικό, να αποστείλετε την επιλογή][MANAGE-MANIFEST-UPLOAD]

    ![Αποστείλετε το δήλωσης JSON][UPLOAD-MANIFEST]

    ![Αποστείλετε το δήλωσης JSON - επιβεβαίωσης][UPLOAD-MANIFEST-CONFIRM]

Τώρα που είναι αποθηκευμένο το δηλωτικό, μπορείτε να δώσετε μια καταχωρημένες πρόσβαση εφαρμογή προγράμματος-πελάτη των νέων δικαιωμάτων που έχουμε προσθέσει παραπάνω. Αυτήν τη στιγμή μπορείτε να χρησιμοποιήσετε το Azure κλασική πύλη Web UI αντί για επεξεργασία δηλωτικό της εφαρμογής υπολογιστή-πελάτη:  

1. Πρώτα, μεταβείτε στη σελίδα "Ρύθμιση παραμέτρων" από την εφαρμογή-πελάτη στην οποία θέλετε να προσθέσετε την access για το νέο API και κάντε κλικ στο κουμπί "Προσθήκη εφαρμογής".
2. Στη συνέχεια, θα εμφανιστεί με τη λίστα με τις εφαρμογές καταχωρημένες πόρων (API) στο μισθωτή. Κάντε κλικ στο κουμπί συν / + σύμβολο δίπλα στο όνομα της εφαρμογής πόρων για να το επιλέξετε.  
3. Στη συνέχεια, κάντε κλικ στο σημάδι ελέγχου στην κάτω δεξιά γωνία.
4. Όταν επιστρέψετε στην ενότητα "Προσθήκη εφαρμογής" σελίδα ρύθμισης παραμέτρων του πελάτη σας, θα δείτε τη νέα εφαρμογή πόρων στη λίστα. Εάν έχετε το δείκτη του ποντικιού επάνω από την ενότητα "Ανάθεση δικαιωμάτων" στα δεξιά της γραμμής, θα δείτε μια αναπτυσσόμενη λίστα που εμφανίζεται. Κάντε κλικ στη λίστα και, στη συνέχεια, επιλέξτε το νέο δικαίωμα για να προσθέσετε το πρόγραμμα-πελάτη ζητήθηκε λίστα δικαιωμάτων. Σημείωση: αυτό το δικαίωμα νέα θα αποθηκευτεί με τη ρύθμιση ταυτότητας την εφαρμογή-πελάτη, στην ιδιότητα συλλογής "requiredResourceAccess".

![Δικαιώματα σε άλλες εφαρμογές][PERMS-TO-OTHER-APPS]

![Δικαιώματα σε άλλες εφαρμογές][PERMS-SELECT-APP]

![Δικαιώματα σε άλλες εφαρμογές][PERMS-SELECT-PERMS]

Που είναι. Τώρα τις εφαρμογές σας θα εκτελείται, χρησιμοποιώντας τα νέα ρύθμιση παραμέτρων ταυτότητας.

## <a name="next-steps"></a>Επόμενα βήματα
- Για περισσότερες λεπτομέρειες σχετικά με τη σχέση μεταξύ των εφαρμογών και υπηρεσιών κεφάλαιο αντικειμένων μιας εφαρμογής, ανατρέξτε στο θέμα [εφαρμογή και υπηρεσία κεφαλαίου αντικείμενα στο Azure AD][AAD-APP-OBJECTS].
- Ανατρέξτε στο [Γλωσσάρι για προγραμματιστές Azure AD] [ AAD-DEVELOPER-GLOSSARY] για ορισμούς ορισμένες από τις βασικές έννοιες για προγραμματιστές Azure Active Directory (AD).

Χρησιμοποιήστε την παρακάτω ενότητα DISQUS σχολίων για να στείλετε τα σχόλιά σας και Βοηθήστε μας να περιορίσετε και να διαμορφώσετε μας περιεχόμενο.

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

