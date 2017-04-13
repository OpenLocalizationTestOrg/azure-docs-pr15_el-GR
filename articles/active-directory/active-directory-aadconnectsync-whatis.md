<properties
    pageTitle="Azure AD Connect συγχρονισμού: Κατανόηση και προσαρμόστε συγχρονισμός | Microsoft Azure"
    description="Εξηγεί τον τρόπο Azure AD Connect συγχρονισμός λειτουργεί και πώς μπορείτε να προσαρμόσετε."
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
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect συγχρονισμού: Κατανόηση και προσαρμογή συγχρονισμού
Οι υπηρεσίες συγχρονισμού σύνδεση Azure Active Directory (Azure AD Connect συγχρονισμός) είναι ένα κύριο στοιχείο του Azure AD Connect. Το αναλαμβάνει όλες οι εργασίες που σχετίζονται με συγχρονισμό ταυτότητας δεδομένων μεταξύ του περιβάλλοντος εσωτερικής εγκατάστασης και του Azure AD. Azure AD Connect συγχρονισμός είναι η εξαρτώμενη εργασία DirSync, Azure AD Sync και Διαχείριση ταυτοτήτων Forefront με το Azure Active Directory γραμμή σύνδεσης έχει ρυθμιστεί.

Αυτό το θέμα είναι το σπίτι για το **συγχρονισμό Azure AD Connect** (ονομάζεται επίσης **μηχανισμού συγχρονισμού**) λίστες και συνδέσεις για όλα τα άλλα θέματα που σχετίζονται με αυτό. Για συνδέσεις σε Azure AD Connect, ανατρέξτε στο θέμα [ενοποίηση σας ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).

Η υπηρεσία συγχρονισμού αποτελείται από δύο στοιχεία, το στοιχείο **Συγχρονισμός Azure AD Connect** εσωτερικής εγκατάστασης και πλευρά της υπηρεσίας Azure AD που ονομάζεται **Azure AD Connect συγχρονισμού υπηρεσίας**. Η υπηρεσία είναι κοινά για DirSync, Azure AD Sync και Azure AD Connect.

## <a name="azure-ad-connect-sync-topics"></a>Θέματα συγχρονισμού Azure AD Connect

Το θέμα | Τι να καλύπτει και πότε πρέπει να διαβάσετε
----- | -----
**Τα θεμελιώδη στοιχεία συγχρονισμού Azure AD Connect** |
[Κατανόηση της αρχιτεκτονικής](active-directory-aadconnectsync-understanding-architecture.md) | Για όσους από εσάς είστε νέος χρήστης του μηχανισμού συγχρονισμού και θέλετε να μάθετε περισσότερα σχετικά με την αρχιτεκτονική και τους όρους που χρησιμοποιούνται.
[Τεχνική έννοιες](active-directory-aadconnectsync-technical-concepts.md) | Μια σύντομη έκδοση του θέματος αρχιτεκτονική και περιγράφει συνοπτικά εξηγεί τους όρους που χρησιμοποιούνται.
[Σύνδεση τοπολογίες για Azure AD](active-directory-aadconnect-topologies.md) | Περιγράφει τις διαφορετικές τοπολογίες και σενάρια που υποστηρίζει το μηχανισμό συγχρονισμού.
**Προσαρμοσμένη ρύθμιση παραμέτρων** |
[Εκτέλεση του Οδηγού εγκατάστασης ξανά](active-directory-aadconnectsync-installation-wizard.md) | Εξηγεί τις επιλογές που είναι διαθέσιμες κατά την εκτέλεση του Οδηγού εγκατάστασης Azure AD Connect ξανά.
[Κατανόηση των δηλωτικό προμήθειας](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| Περιγράφει το μοντέλο ρύθμισης παραμέτρων που ονομάζεται δηλωτικό προμήθειας.
[Κατανόηση των δηλωτικό παροχής παραστάσεις](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | Περιγράφει τη σύνταξη για τη γλώσσα παράστασης που χρησιμοποιείται στο δηλωτικό προμήθειας.
[Κατανόηση της προεπιλεγμένης ρύθμισης παραμέτρων](active-directory-aadconnectsync-understanding-default-configuration.md)| Περιγράφει τους κανόνες της πρώτης και της προεπιλεγμένης ρύθμισης παραμέτρων. Επίσης, περιγράφει πώς οι κανόνες συνεργάζονται για τα σενάρια της πρώτης ώστε να λειτουργεί.
[Κατανόηση των χρηστών και τις επαφές](active-directory-aadconnectsync-understanding-users-and-contacts.md) | Συνεχίζεται στην στο προηγούμενο θέμα και περιγράφει τον τρόπο τις παραμέτρους για τους χρήστες και τις επαφές λειτουργίας μαζί, ιδίως σε ένα περιβάλλον πολλαπλών δάσος.
[Πώς μπορείτε να κάνετε την αλλαγή της προεπιλεγμένης ρύθμισης παραμέτρων](active-directory-aadconnectsync-change-the-configuration.md) | Σας καθοδηγεί σε πώς μπορείτε να κάνετε μια αλλαγή στο χαρακτηριστικό ροών κοινές ρύθμισης παραμέτρων.
[Βέλτιστες πρακτικές για την αλλαγή της προεπιλεγμένης ρύθμισης παραμέτρων](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | Υποστήριξη περιορισμοί και για τις αλλαγές στη ρύθμιση παραμέτρων της πρώτης.
[Ρύθμιση παραμέτρων του φιλτραρίσματος](active-directory-aadconnectsync-configure-filtering.md) | Περιγράφει τις διαφορετικές επιλογές για το πώς μπορείτε να περιορίσετε τα αντικείμενα που συγχρονίζονται με Azure AD και βήμα προς βήμα πώς μπορείτε να ρυθμίσετε αυτές τις επιλογές.
**Δυνατότητες και σενάρια** |
[Αποτροπή τυχαίες διαγραφές](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | Περιγράφει τη δυνατότητα *αποτρέψετε τυχαίες διαγραφές* και πώς μπορείτε να ρυθμίσετε τις παραμέτρους της.
[Χρονοδιάγραμμα](active-directory-aadconnectsync-feature-scheduler.md) | Περιγράφει το ενσωματωμένο scheduler, το οποίο είναι η εισαγωγή, συγχρονισμός και εξαγωγή δεδομένων.
[Υλοποίηση συγχρονισμό κωδικού πρόσβασης](active-directory-aadconnectsync-implement-password-synchronization.md) | Περιγράφει πώς λειτουργεί η συγχρονισμό κωδικού πρόσβασης, πώς μπορείτε να υλοποιήσετε και πώς μπορείτε να λειτουργούν και αντιμετώπιση προβλημάτων.
[Συσκευή αντιγραφής](active-directory-aadconnect-feature-device-writeback.md) | Περιγράφει πώς λειτουργεί η συσκευή αντιγραφής στο Azure AD Connect.
[Επεκτάσεις καταλόγου](active-directory-aadconnectsync-feature-directory-extensions.md) | Περιγράφει τον τρόπο για να επεκτείνετε το σχήμα Azure AD με τα δικά σας προσαρμοσμένα χαρακτηριστικά τους.
**Συγχρονισμός υπηρεσίας** |
[Δυνατότητες υπηρεσίας συγχρονισμού του Azure AD Connect](active-directory-aadconnectsyncservice-features.md) | Περιγράφει πλευρά της υπηρεσίας συγχρονισμού και πώς μπορείτε να αλλάξετε τις ρυθμίσεις συγχρονισμού στο Azure AD.
[Αναπαραγωγή ανοχή χαρακτηριστικό](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | Περιγράφει τον τρόπο ενεργοποίησης και χρήσης ανοχή τιμές Διπλότυπο χαρακτηριστικό **userPrincipalName** και **proxyAddresses** .
**Λειτουργίες και περιβάλλον εργασίας Χρήστη** |
[Διαχείριση συγχρονισμού υπηρεσίας](active-directory-aadconnectsync-service-manager-ui.md) | Περιγράφει το περιβάλλον εργασίας Χρήστη Διαχείριση υπηρεσίας συγχρονισμού, συμπεριλαμβανομένων των [λειτουργιών](active-directory-aadconnectsync-service-manager-ui-operations.md), [γραμμές σύνδεσης](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse σχεδίασης](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)και καρτέλες [Metaverse αναζήτησης](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) .
[Λειτουργικές εργασίες και τα θέματα](active-directory-aadconnectsync-operations.md) | Περιγράφει λειτουργικές ανησυχίες, όπως αποκατάσταση.
**Πώς να...** |
[Επαναφορά του λογαριασμού Azure AD](active-directory-aadconnectsync-howto-azureadaccount.md) | Μάθετε πώς μπορείτε να επαναφέρετε τα διαπιστευτήρια του λογαριασμού της υπηρεσίας που χρησιμοποιούνται για τη σύνδεση από το Azure AD Connect συγχρονισμού για να Azure AD.
**Περισσότερες πληροφορίες και αναφορές** |
[Θύρες](active-directory-aadconnect-ports.md) | Παραθέτει τις θύρες που πρέπει να το ανοίξετε μεταξύ του μηχανισμού συγχρονισμού και σας σε καταλόγους εσωτερικής εγκατάστασης και του Azure AD.
[Χαρακτηριστικά που συγχρονίζονται με Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) | Παραθέτει σε λίστα όλα τα χαρακτηριστικά που συγχρονίζονται μεταξύ της εσωτερικής AD και Azure AD.
[Αναφορά συναρτήσεων](active-directory-aadconnectsync-functions-reference.md) | Παραθέτει σε λίστα όλες τις συναρτήσεις διαθέσιμες στο δηλωτικό προμήθειας.

## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Ενοποίηση του ταυτοτήτων εσωτερικής εγκατάστασης με το Azure Active Directory](active-directory-aadconnect.md)
