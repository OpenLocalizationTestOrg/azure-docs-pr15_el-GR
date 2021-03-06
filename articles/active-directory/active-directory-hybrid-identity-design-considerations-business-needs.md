<properties
    pageTitle="Azure Active Directory υβριδική ταυτότητας θέματα σχεδίασης - Προσδιορισμός απαιτήσεις ταυτότητας | Microsoft Azure"
    description="Προσδιορίστε τις ανάγκες της εταιρείας που θα οδηγήσει να καθορίσετε τις απαιτήσεις για τη σχεδίαση ταυτότητας υβριδική."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Καθορίστε τις απαιτήσεις ταυτότητας για την υβριδική λύση ταυτότητας
Το πρώτο βήμα στο σχεδιασμό μιας λύσης ταυτότητας υβριδική είναι για να καθορίσετε τις απαιτήσεις για τον οργανισμό επιχειρήσεις που θα αξιοποίηση αυτήν τη λύση.  Υβριδική ταυτότητας ξεκινά ως υποστηρικτικό ρόλο (υποστηρίζει όλες τις άλλες λύσεις cloud, παρέχοντας έλεγχο ταυτότητας) και μεταβαίνει στην παροχή ενδιαφέρον και νέες δυνατότητες που ξεκλείδωμα νέα φόρτο εργασίας για τους χρήστες.  Τα φόρτους εργασίας ή τις υπηρεσίες που θέλετε να χρησιμοποιούν για τους χρήστες σας θα διάρκεια της υπαγόρευσης τις απαιτήσεις για τη σχεδίαση ταυτότητας υβριδική.  Αυτές οι υπηρεσίες και φόρτους εργασίας πρέπει να αξιοποιήσετε υβριδική ταυτότητας δύο εσωτερικής εγκατάστασης και στο cloud.  

Πρέπει να μεταβείτε σε αυτές τις βασικές πτυχές των την επιχείρηση για να κατανοήσετε τι είναι μια απαίτηση τώρα και τι εταιρείας προγράμματος για το μέλλον. Εάν δεν έχετε την ορατότητα της στρατηγικής μακροπρόθεσμη για υβριδική ταυτότητας σχεδίασης, το πιθανότερο είναι ότι τη λύση σας δεν θα είναι με την επιχείρηση ανάγκες μεγέθυνση και να αλλάξετε.   T αυτός διαγράμματος κάτω από το στοιχείο εμφανίζει ένα παράδειγμα μια αρχιτεκτονική ταυτότητας υβριδική και το φόρτο εργασίας που είναι που ξεκλείδωτη για τους χρήστες. Αυτό είναι απλώς ένα παράδειγμα της όλες τις νέες δυνατότητες που μπορούν να ξεκλείδωτη και παραδόθηκε στρατηγική ταυτότητας συμπαγές υβριδική. 
 
Ορισμένα στοιχεία που αποτελούν τμήμα της αρχιτεκτονικής ταυτότητας υβριδική![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Προσδιορίσετε επιχειρηματικές ανάγκες
Κάθε εταιρεία θα έχουν διαφορετικές απαιτήσεις, ακόμα και αν οι εταιρείες αυτές είναι τμήμα του ίδιου κλάδου, την πραγματική επιχειρηματική απαιτήσεις ενδέχεται να διαφέρουν. Εξακολουθείτε να μπορείτε να αξιοποιήσετε βέλτιστες πρακτικές από το κλάδο, αλλά τελικά είναι ανάγκες της εταιρείας που θα οδηγήσει να καθορίσετε τις απαιτήσεις για τη σχεδίαση ταυτότητας υβριδική. 

Φροντίστε να απαντούν στα παρακάτω ερωτήματα για να προσδιορίσετε επιχειρηματικές ανάγκες σας:

- Η εταιρεία σας σκοπεύει να αποκοπή IT λειτουργικές κόστος;
- Η εταιρεία σας σκοπεύει να ασφαλούς cloud Παγίων (ΑΠΑ εφαρμογές, υποδομή);
- Η εταιρεία σας σκοπεύει να modernize IT σας;
  - Είναι οι χρήστες σας πιο κινητές συσκευές και απαιτητικών IT για να δημιουργήσετε εξαιρέσεις σε σας DMZ για να επιτρέψετε σε διαφορετικό τύπο κίνηση για να αποκτήσετε πρόσβαση σε διαφορετικούς πόρους;
  - Η εταιρεία σας διαθέτει παλαιού τύπου εφαρμογές που απαιτείται για να δημοσιευτεί σε αυτούς τους χρήστες σύγχρονα, αλλά δεν είναι εύκολο να επανεγγραφή;
  - Χρειάζεται η εταιρεία σας να χρησιμοποιήσουν όλες αυτές τις εργασίες και να την στην περιοχή έλεγχος ταυτόχρονα;
- Η εταιρεία σας σκοπεύει να ασφαλούς ταυτότητες των χρηστών και μείωση κινδύνου μεταφέροντας νέα εργαλεία που αξιοποιήσετε την εξειδίκευσή το της Microsoft Azure ασφαλείας γνώσεις στην εσωτερική εγκατάσταση;
- Η εταιρεία σας προσπαθεί να καταργήσω το dreaded "εξωτερική" λογαριασμών εσωτερικής εγκατάστασης και να τα μετακινήσετε στο cloud όπου δεν είναι πλέον αδρανείς απειλή μέσα το περιβάλλον εσωτερικής εγκατάστασης;

## <a name="analyze-on-premises-identity-infrastructure"></a>Ανάλυση υποδομής ταυτοτήτων εσωτερικής εγκατάστασης
Τώρα που έχετε μια ιδέα για το σχετικά με τις απαιτήσεις επιχειρήσεις η εταιρεία σας, πρέπει να αξιολογήσετε την υποδομή την ταυτότητά σας εσωτερικής εγκατάστασης. Αυτή η αξιολόγηση είναι σημαντικό για τον ορισμό τις τεχνικές απαιτήσεις για να ενσωματώσετε τη λύση σας ταυτότητα τρέχουσα στο σύστημα διαχείρισης ταυτοτήτων cloud. Φροντίστε να απαντούν στα παρακάτω ερωτήματα:

- Ποια λύση ελέγχου ταυτότητας και εξουσιοδότησης η εταιρεία σας χρήση εσωτερικής εγκατάστασης; 
- Η εταιρεία σας διαθέτει αυτήν τη στιγμή τις υπηρεσίες συγχρονισμού στην εσωτερική εγκατάσταση;
- Η εταιρεία σας χρησιμοποιεί τις υπηρεσίες παροχής ταυτότητας τρίτου κατασκευαστή (IdP);

Πρέπει επίσης να λάβετε υπόψη τις υπηρεσίες cloud που ενδέχεται να έχουν την εταιρεία σας. Εκτέλεση αξιολόγηση για να κατανοήσετε την τρέχουσα ενοποίηση με τα μοντέλα ΑΔΑ, IaaS ή PaaS στο περιβάλλον σας είναι πολύ σημαντικά. Φροντίστε να απαντούν στα παρακάτω ερωτήματα κατά τη διάρκεια αυτής της αξιολόγησης:
- Η εταιρεία σας διαθέτει οποιαδήποτε ενοποίηση με μια υπηρεσία παροχής cloud;
- Εάν επιλέξετε Ναι, των υπηρεσιών που έχουν χρησιμοποιηθεί;
- Χρησιμοποιείται αυτή η ενοποίηση αυτήν τη στιγμή παραγωγής ή είναι μιας πιλοτικής δοκιμής;


>[AZURE.NOTE]
Εάν δεν έχετε μια ακριβή αντιστοίχιση με όλες τις εφαρμογές σας και τις υπηρεσίες cloud, μπορείτε να χρησιμοποιήσετε το εργαλείο εντοπισμού εφαρμογή Cloud. Αυτό το εργαλείο μπορεί να διαθέσετε το τμήμα IT ορατότητα σε επιχειρήσεις όλες της εταιρείας σας και εφαρμογές cloud καταναλωτών. Που κάνει πιο εύκολη από ποτέ να ανακαλύψετε σκιά IT στην εταιρεία σας, όπως λεπτομέρειες σχετικά με τη χρήση μοτίβων και όλοι οι χρήστες πρόσβαση σε εφαρμογές του cloud. Για να αποκτήσετε πρόσβαση σε αυτό το εργαλείο μεταβείτε [https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## <a name="evaluate-identity-integration-requirements"></a>Αξιολόγηση απαιτήσεις ενοποίησης ταυτότητας
Στη συνέχεια, πρέπει να αξιολογήσετε τις απαιτήσεις ενοποίησης ταυτότητα. Αυτή η αξιολόγηση είναι σημαντικό για να ορίσετε τις τεχνικές απαιτήσεις για το πώς θα τον έλεγχο ταυτότητας χρηστών, πώς θα φαίνονται τα παρουσία της εταιρείας στο cloud, πώς η εταιρεία θα επιτρέπουν εξουσιοδότησης και τι της εμπειρίας χρήστη θα γίνει. Φροντίστε να απαντούν στα παρακάτω ερωτήματα:

- Θα την εταιρεία σας χρησιμοποιούν ομοσπονδίας, τυπικό έλεγχο ταυτότητας ή και τα δύο;
- Είναι η Ομοσπονδία απαίτηση;  Λόγω τα εξής:
 - SSO τύπου Kerberos
 - Η εταιρεία σας διαθέτει μια εσωτερική εφαρμογές (είτε ενσωματωμένο εντός της εταιρείας ή τρίτου κατασκευαστή) που χρησιμοποιεί SAML ή παρόμοιες δυνατότητες ομοσπονδίας.
 - MFA μέσω έξυπνες κάρτες. RSA SecurID, κ.λπ.
 - Κανόνες πρόσβασης υπολογιστή-πελάτη που αντιμετωπίζουν στα παρακάτω ερωτήματα:
     1. Μπορώ να αποκλείσω όλα εξωτερικής πρόσβασης στο Office 365 με βάση τη διεύθυνση IP του προγράμματος-πελάτη;
     1. Μπορώ να αποκλείσω όλα εξωτερικής πρόσβασης στο Office 365, εκτός από το Exchange ActiveSync;
     1. Μπορώ να αποκλείσω όλα εξωτερικής πρόσβασης στο Office 365, εκτός από εφαρμογές (OWA, SPO) που βασίζονται σε πρόγραμμα περιήγησης
     1. Μπορώ να αποκλείσω όλα εξωτερικής πρόσβασης στο Office 365 για τα μέλη των ομάδων που έχει οριστεί ως AD
- Ανησυχίες/ελέγχου ασφαλείας
- Ήδη υπάρχουσες επενδύσεις σε εξωτερικές ελέγχου ταυτότητας
- Τι όνομα θα χρησιμοποιήσει της εταιρείας μας για μας τομέα σας στο cloud;
- Η εταιρεία έχει έναν προσαρμοσμένο τομέα;
    1. Είναι αυτόν τον τομέα δημόσια και να ελεγχθεί εύκολα μέσω DNS;
    1. Εάν δεν είναι, στη συνέχεια, έχετε μια δημόσια τομέα που μπορούν να χρησιμοποιηθούν για να καταχωρήσετε μια εναλλακτική UPN στο AD;
- Είναι συνεπείς για αναπαράσταση cloud τα αναγνωριστικά χρήστη; 
- Η εταιρεία έχει εφαρμογές που απαιτούν ενοποίηση με τις υπηρεσίες cloud;
- Η εταιρεία έχει πολλούς τομείς και θα όλες χρησιμοποιεί τυπική ή ομόσπονδη έλεγχο ταυτότητας;

## <a name="evaluate-applications-that-run-in-your-environment"></a>Αξιολόγηση εφαρμογές που εκτελούνται στο περιβάλλον σας
Τώρα που έχετε μια ιδέα για το όσον αφορά την εσωτερικής εγκατάστασης και cloud υποδομής, πρέπει να αξιολογήσετε τις εφαρμογές που εκτελούνται σε αυτά τα περιβάλλοντα. Αυτή η αξιολόγηση είναι σημαντικό για να ορίσετε τις τεχνικές απαιτήσεις για να ενσωματώσετε αυτές τις εφαρμογές στο σύστημα διαχείρισης ταυτοτήτων cloud. Φροντίστε να απαντούν στα παρακάτω ερωτήματα:

- Πού θα live εφαρμογές μας;
- Θα οι χρήστες έχουν πρόσβαση σε εφαρμογές στην εσωτερική εγκατάσταση;  Στο cloud; Ή και τα δύο;
- Υπάρχουν προγράμματα για να τραβήξετε την υπάρχουσα φόρτους εργασίας εφαρμογών και να τις μετακινήσετε στο cloud;
- Υπάρχουν προγράμματα για να αναπτύξετε νέες εφαρμογές που θα βρίσκεται είτε στην εσωτερική εγκατάσταση ή στο cloud που θα χρησιμοποιήσετε στο cloud ελέγχου ταυτότητας;

## <a name="evaluate-user-requirements"></a>Αξιολόγηση απαιτήσεις χρήστη
Πρέπει επίσης να αξιολογήσετε τις απαιτήσεις χρήστη. Αυτή η αξιολόγηση είναι σημαντικό για να ορίσετε τα βήματα που θα χρειαστούν για σε επιβίβασης και βοηθά τους χρήστες, όπως αυτά μετάβασης στο cloud. Φροντίστε να απαντούν στα παρακάτω ερωτήματα:

- Θα οι χρήστες έχουν πρόσβαση σε εφαρμογές στην εσωτερική εγκατάσταση;
- Θα οι χρήστες έχουν πρόσβαση σε εφαρμογές στο cloud;
- Πώς οι χρήστες που συνήθως login τους περιβάλλον εσωτερικής εγκατάστασης;
- Πώς θα χρήστες εισόδου στο cloud;

>[AZURE.NOTE]
Φροντίστε να κρατήσετε σημειώσεις κάθε απάντησης και έχετε κατανοήσει λογικής πίσω από το answer. [Προσδιορισμός απόκριση περιστατικού απαιτήσεις](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) θα μεταβείτε στις επιλογές που είναι διαθέσιμες και οι επαγγελματίες τεχνολογιών πληροφορικής/τα μειονεκτήματα της κάθε επιλογή.  Με αντιμετωπίζετε απαντήσεις σε αυτές τις ερωτήσεις που θα επιλέξετε ποια επιλογή καλύτερη σας εξυπηρετεί την επιχείρησή σας ανάγκες.

## <a name="next-steps"></a>Επόμενα βήματα
[Καθορίστε τις απαιτήσεις συγχρονισμού καταλόγου](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Δείτε επίσης
[Επισκόπηση θέματα σχεδίασης](active-directory-hybrid-identity-design-considerations-overview.md)
