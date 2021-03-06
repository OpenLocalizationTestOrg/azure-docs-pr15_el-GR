<properties
   pageTitle="Βέλτιστες πρακτικές για ενημερώσεις λογισμικού στο Windows Azure IaaS | Microsoft Azure"
   description="Το άρθρο παρέχει μια συλλογή από βέλτιστες πρακτικές για ενημερώσεις λογισμικού σε ένα περιβάλλον Microsoft Azure IaaS.  Προορίζεται για επαγγελματίες τεχνολογιών ΠΛΗΡΟΦΟΡΙΚΉΣ και οι αναλυτές ασφαλείας που ασχοληθείτε με αλλαγή ελέγχου, λογισμικό ενημερωμένης έκδοσης και της διαχείρισης πόρων σε καθημερινή βάση, συμπεριλαμβανομένων και αυτών που είναι υπεύθυνος για την ασφάλεια και συμμόρφωση προσπάθειες της εταιρείας τους."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="best-practices-for-software-updates-on-microsoft-azure-iaas"></a>Βέλτιστες πρακτικές για ενημερώσεις λογισμικού στο Microsoft Azure IaaS

Πριν να σας σε οποιοδήποτε είδος συζήτησης για τις βέλτιστες πρακτικές για ένα περιβάλλον Azure [IaaS](https://azure.microsoft.com/overview/what-is-iaas/) είναι σημαντικό να κατανοήσετε τι τα σενάρια είναι που θα πρέπει να τη Διαχείριση ενημερώσεις λογισμικού και των αρμοδιοτήτων. Στο παρακάτω διάγραμμα πρέπει να σας βοηθήσει να κατανοήσετε αυτά τα όρια:

![Μοντέλα cloud και ευθύνες](./media/azure-security-best-practices-software-updates-iaas/sec-cloudstack-new.png)

Η πιο αριστερή στήλη εμφανίζει επτά αρμοδιότητες (ορίζονται στις ενότητες που ακολουθούν) ότι οργανισμοί πρέπει να λάβετε υπόψη, τα οποία συνεισφέρουν για την ασφάλεια και προστασία προσωπικών δεδομένων από ένα περιβάλλον υπολογιστών.
 
Ταξινόμηση δεδομένων & ευθύνη και προγράμματος-πελάτη και τελικό σημείο προστασίας είναι τα καθήκοντα που είναι αποκλειστικά στον τομέα των πελατών και αρμοδιότητες φυσικά, κεντρικός υπολογιστής και δικτύου βρίσκονται στον τομέα υπηρεσιών παροχής υπηρεσίας cloud στα μοντέλα PaaS και ΑΔΑ. 

Οι υπόλοιπες αρμοδιότητες είναι κοινόχρηστες μεταξύ τους πελάτες και στο cloud υπηρεσίες παροχής. Ορισμένες αρμοδιότητες απαιτούν την υπηρεσία παροχής Κρυπτογράφησης και πελατών για να διαχειριστείτε και να διαχειριστείτε την ευθύνη μαζί, όπως τον έλεγχο από τους τομείς. Για παράδειγμα, εξετάστε το ενδεχόμενο να ταυτότητας και έχετε πρόσβαση στη Διαχείριση κατά τη χρήση Azure υπηρεσίες Active Directory; η ρύθμιση παραμέτρων για τις υπηρεσίες, όπως τον έλεγχο ταυτότητας πολλών παραγόντων είναι έως και τον πελάτη, αλλά ταυτόχρονα εξασφαλίζετε ότι οι λειτουργίες αποτελεσματικές είναι ευθύνη του Windows Azure.

> [AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με την κοινόχρηστη αρμοδιότητες στο cloud, διαβάστε [Κοινόχρηστο αρμοδιότητες για την υπολογιστική Cloud](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) 

Εφαρμόστε αυτές τις ίδιες βασικές αρχές σε ένα σενάριο υβριδική όπου η εταιρεία σας χρησιμοποιεί το ΣΠΣ IaaS Azure που επικοινωνούν με τους πόρους στην εσωτερική εγκατάσταση όπως φαίνεται στο παρακάτω διάγραμμα.

![Σενάριο τυπικές υβριδική με το Microsoft Azure](./media/azure-security-best-practices-software-updates-iaas/sec-azconnectonpre.png)

## <a name="initial-assessment"></a>Αρχική αξιολόγηση

Ακόμα και εάν η εταιρεία σας χρησιμοποιεί ήδη ένα σύστημα διαχείρισης ενημέρωση και έχετε ήδη πολιτικές ενημέρωσης λογισμικού σε σημείο, είναι σημαντικό να συχνά τρόπος για να αναθεωρήσετε προηγούμενες αξιολογήσεις πολιτικής και να τις ενημερώσετε με βάση τις τρέχουσες απαιτήσεις. Αυτό σημαίνει ότι θα πρέπει να είστε εξοικειωμένοι με την τρέχουσα κατάσταση των πόρων της εταιρείας σας. Για την επίτευξη αυτήν την κατάσταση, πρέπει να γνωρίζετε:

-   Η φυσική και εικονική υπολογιστές στην εταιρεία σας.

-   Λειτουργικά συστήματα και τις εκδόσεις που εκτελείται σε κάθε έναν από αυτούς τους υπολογιστές φυσικών και εικονικών.

-   Ενημερώσεις λογισμικού που είναι εγκατεστημένο σε κάθε υπολογιστή (εκδόσεις service pack, ενημερώσεις λογισμικού και άλλες τροποποιήσεις).

-   Η συνάρτηση κάθε υπολογιστή που εκτελεί της εταιρείας σας.

-   Τις εφαρμογές και τα προγράμματα που εκτελούνται σε κάθε υπολογιστή.

-   Κατοχή και τα στοιχεία επικοινωνίας για κάθε υπολογιστή.

-   Παρουσίαση των περιουσιακών στοιχείων στο περιβάλλον σας και τους σχετική τιμή για να καθορίσετε ποιες περιοχές χρειαστεί περισσότερο την προσοχή και την προστασία.

-   Γνωστά προβλήματα και τις διεργασίες την εταιρεία σας έχει σε σημείο για αναγνώρισης νέα θέματα ασφάλειας ή αλλαγές σε επίπεδο ασφάλειας.

-   Μέτρα που έχουν αναπτυχθεί για την ασφάλιση το περιβάλλον σας.

Θα πρέπει να ενημερώνετε τακτικά αυτές τις πληροφορίες και θα πρέπει να είναι άμεσα διαθέσιμα σε εκείνους που εμπλέκονται στη διαδικασία ενημέρωσης της διαχείρισης σας λογισμικό.

## <a name="establish-a-baseline"></a>Δημιουργήστε μια γραμμή βάσης

Ένα σημαντικό μέρος της διαδικασίας διαχείρισης ενημέρωσης λογισμικού δημιουργεί αρχικό τυπικές εγκαταστάσεις εκδόσεις λειτουργικού συστήματος, εφαρμογών και υλικού για υπολογιστές της εταιρείας σας; αυτά τα πεδία ονομάζονται γραμμές βάσης. Μια γραμμή βάσης είναι η ρύθμιση παραμέτρων ενός προϊόντος ή σύστημα που έχει δημιουργηθεί σε ένα συγκεκριμένο σημείο στο χρόνο. Μια εφαρμογή ή λειτουργικό σύστημα γραμμής βάσης, για παράδειγμα, παρέχει τη δυνατότητα να ξαναδημιουργήσετε υπολογιστή ή υπηρεσίας σε μια συγκεκριμένη κατάσταση.

Γραμμές βάσης παρέχουν τη βάση για την εύρεση και διόρθωση πιθανά προβλήματα και να απλοποιήσετε τη διαδικασία διαχείρισης ενημέρωσης λογισμικού, τόσο από τη μείωση του αριθμού των ενημερώσεις λογισμικού, πρέπει να αναπτύξετε της εταιρείας σας και, αυξάνοντας τη δυνατότητα για την παρακολούθηση της συμμόρφωσης.

Μετά την εκτέλεση του αρχικού ελέγχου της εταιρείας σας, θα πρέπει να χρησιμοποιείτε τις πληροφορίες που λαμβάνονται από τον έλεγχο για να ορίσετε μια γραμμή βάσης λειτουργικές για τα στοιχεία IT μέσα σε περιβάλλον παραγωγής σας. Ένας αριθμός γραμμές βάσης ενδεχομένως να απαιτούνται, ανάλογα με τους διαφορετικούς τύπους υλικού και λογισμικού αναπτυχθεί σε παραγωγής.

Για παράδειγμα, ορισμένοι διακομιστές απαιτεί μια ενημέρωση λογισμικού για να αποτρέψετε τους προεξοχή όταν εισέρχονται τη διεργασία τερματισμού κατά την εκτέλεση του Windows Server 2012. Μια γραμμή βάσης για αυτούς τους διακομιστές πρέπει να περιλαμβάνει αυτής της ενημερωμένης έκδοσης λογισμικού.

Σε μεγάλους οργανισμούς, συχνά είναι χρήσιμο να χωρίσετε το υπολογιστές στην εταιρεία σας σε κατηγορίες περιουσιακών στοιχείων και να διατηρήσετε κάθε κατηγορία σε μια τυπική γραμμή βάσης, χρησιμοποιώντας τις ίδιες εκδόσεις του λογισμικού και ενημερώσεις λογισμικού. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε αυτές τις κατηγορίες περιουσιακών στοιχείων σε μια κατανομή ενημέρωσης λογισμικού προτεραιότητα.

## <a name="subscribe-to-the-appropriate-software-update-notification-services"></a>Εγγραφή με τις υπηρεσίες κατάλληλο λογισμικό ενημέρωση ειδοποίηση

Αφού εκτελέσετε μια αρχική ελέγχου του λογισμικού στο θέμα χρήση της εταιρείας σας, θα πρέπει να μπορείτε να προσδιορίσετε της καλύτερης μεθόδου για να λαμβάνετε ειδοποιήσεις για νέες ενημερώσεις λογισμικού για κάθε προϊόν λογισμικού και την έκδοση. Ανάλογα με το προϊόν λογισμικού, της καλύτερης μεθόδου ειδοποίησης μπορεί να είναι ειδοποιήσεις μέσω ηλεκτρονικού ταχυδρομείου, τοποθεσίες Web ή δημοσιεύσεων υπολογιστή.

Για παράδειγμα, το Microsoft Security απόκριση Center (MSRC) αποκρίνεται σε όλα ανησυχίες σχετικά με την ασφάλεια για προϊόντα της Microsoft και παρέχει την ασφάλεια ενημερωτικό δελτίο υπηρεσία της Microsoft, μια ειδοποίηση δωρεάν ηλεκτρονικού ταχυδρομείου που μόλις προσδιορισμένο ευπάθειας και ενημερώσεις λογισμικού που έχουν κυκλοφορήσει για να αντιμετωπίσει αυτά τα θέματα ευπάθειας. Μπορείτε να εγγραφείτε σε αυτήν την υπηρεσία στο http://www.microsoft.com/technet/security/bulletin/notify.mspx.

## <a name="software-update-considerations"></a>Ζητήματα ενημέρωσης λογισμικού

Αφού εκτελέσετε μια αρχική ελέγχου του λογισμικού στο θέμα χρήση της εταιρείας σας, θα πρέπει να προσδιορίσετε τις απαιτήσεις για να ρυθμίσετε το σύστημα διαχείρισης ενημέρωση λογισμικού, το οποίο εξαρτάται από το σύστημα διαχείρισης για την ενημέρωση λογισμικό που χρησιμοποιείτε. Για ανάγνωση [Βέλτιστες πρακτικές με υπηρεσιών Windows Server Update](https://technet.microsoft.com/library/Cc708536)WSUS, για το κέντρο του συστήματος Διαβάστε [Σχεδιασμός για ενημερώσεις λογισμικού στη Διαχείριση παραμέτρων](https://technet.microsoft.com/library/gg712696).

Ωστόσο, υπάρχουν ορισμένα ζητήματα γενικά και βέλτιστες πρακτικές που μπορείτε να εφαρμόσετε ανεξάρτητα από τη λύση που χρησιμοποιείτε, όπως φαίνεται στις ενότητες που ακολουθεί.

### <a name="setting-up-the-environment"></a>Ρύθμιση του περιβάλλοντος

Λάβετε υπόψη τις ακόλουθες πρακτικές κατά το σχεδιασμό για τη ρύθμιση του λογισμικού ενημέρωση περιβάλλον διαχείρισης:

-   **Δημιουργία συλλογών ενημέρωσης λογισμικού παραγωγής με βάση σταθερό κριτήρια**: σε γενικές γραμμές, χρήση σταθερό κριτήρια για τη δημιουργία συλλογών για το απόθεμα ενημέρωσης λογισμικού και διανομής θα σας βοηθήσουν να απλοποιήσετε όλα τα στάδια της διαδικασίας διαχείρισης ενημέρωση του λογισμικού. Τα κριτήρια σταθερό μπορούν να περιλαμβάνουν την έκδοση του λειτουργικού συστήματος εγκατεστημένο πρόγραμμα-πελάτη και επίπεδο service pack, ρόλο συστήματος ή οργανισμό προορισμού.

-   **Δημιουργία συλλογών πριν από την παραγωγή που περιλαμβάνουν τους υπολογιστές αναφοράς**: Η συλλογή πριν από την παραγωγή θα πρέπει να συμπεριλάβετε αντιπρόσωπος ρυθμίσεις παραμέτρων εκδόσεις του λειτουργικού συστήματος, γραμμή του λογισμικού επιχειρήσεις και άλλο λογισμικό εκτελείται της εταιρείας σας.

Επίσης, θα πρέπει να όπου ο διακομιστής ενημέρωσης λογισμικού θα βρίσκονται, εάν θα είναι στην υποδομής Azure IaaS στο cloud ή εάν θα είναι εσωτερικής εγκατάστασης. Αυτή είναι μια σημαντική απόφαση επειδή πρέπει να αξιολογήσετε τον όγκο κίνησης μεταξύ των πόρων εσωτερικής εγκατάστασης και Azure υποδομή του. Διαβάστε [σύνδεση ένα δίκτυο εσωτερικής εγκατάστασης σε δίκτυο εικονικού Microsoft Azure](https://technet.microsoft.com/library/Dn786406.aspx) για περισσότερες πληροφορίες σχετικά με τον τρόπο σύνδεσης υποδομής σας στην εσωτερική εγκατάσταση Azure.

Τις επιλογές σχεδίασης που θα καθορίσει όπου θα βρίσκονται στο διακομιστή ενημέρωση επίσης θα ποικίλλουν ανάλογα με την τρέχουσα υποδομή και το σύστημα ενημέρωσης λογισμικού που χρησιμοποιείτε τη συγκεκριμένη στιγμή. Για WSUS Διαβάστε [Ανάπτυξη των υπηρεσιών Windows Server Update στην εταιρεία σας](https://technet.microsoft.com/library/hh852340.aspx) και για τη Διαχείριση ομάδας παραμέτρων του συστήματος κέντρο Διαβάστε [Σχεδιασμός για τοποθεσίες και ιεραρχίες στη Διαχείριση παραμέτρων](https://technet.microsoft.com/library/Gg712681.aspx).

### <a name="backup"></a>Δημιουργία αντιγράφων ασφαλείας

Τακτικής δημιουργίας αντιγράφων ασφαλείας είναι σημαντική όχι μόνο για το λογισμικό διαχείρισης πλατφόρμα ενημέρωσης ίδια αλλά και για τους διακομιστές που θα ενημερωθεί. Θα απαιτούν να εταιρείες που διαθέτουν μια [Αλλαγή διαδικασία διαχείρισης](https://technet.microsoft.com/library/cc543216.aspx) IT για να τους λόγους για γιατί ο διακομιστής πρέπει να ενημερωθεί, την εκτιμώμενη χρόνου εκτός λειτουργίας και πιθανή επίδραση στοίχιση. Για να βεβαιωθείτε ότι έχετε μια επαναφορά ρύθμισης παραμέτρων στη θέση σε περίπτωση που μια ενημέρωση αποτύχει, βεβαιωθείτε ότι δημιουργείτε τακτικά αντίγραφα ασφαλείας του συστήματος.

Ορισμένες επιλογές δημιουργίας αντιγράφων ασφαλείας για Azure IaaS περιλαμβάνουν τα εξής:

-   [Azure προστασία IaaS φόρτο εργασίας με χρήση διαχείρισης προστασία δεδομένων](https://azure.microsoft.com/blog/2014/09/08/azure-iaas-workload-protection-using-data-protection-manager/)

-   [Δημιουργία αντιγράφου ασφαλείας Azure εικονικές μηχανές](../backup/backup-azure-vms.md)

### <a name="monitoring"></a>Παρακολούθηση

Θα πρέπει να εκτελέσετε κανονική αναφορές για να παρακολουθείτε τον αριθμό των λείπουν ή εγκαταστήσει ενημερώσεις ή ενημερώσεις με κατάσταση ελλιπή, για κάθε ενημερωμένη έκδοση λογισμικού που είναι εξουσιοδοτημένοι. Ομοίως, αναφοράς για ενημερώσεις λογισμικού που δεν είναι ακόμη εξουσιοδοτημένοι να διευκολύνει τις αποφάσεις πιο εύκολη ανάπτυξη.

Επίσης, θα πρέπει να λάβετε υπόψη τις ακόλουθες εργασίες:

-   Διεξαγωγή ελέγχου της ισχύει και έχουν εγκατασταθεί ενημερωμένες εκδόσεις για όλους τους υπολογιστές στην εταιρεία σας.

-   Εγκρίνετε και αναπτύξτε τις ενημερώσεις του στους υπολογιστές κατάλληλο.

-   Παρακολούθηση το απόθεμα και ενημέρωση κατάστασης εγκατάστασης και την πρόοδο για όλους τους υπολογιστές στην εταιρεία σας.

Επιπλέον για γενικά θέματα που έχουν επεξήγηση σε αυτό το άρθρο, θα πρέπει επίσης να σκεφτείτε κάθε προϊόν του βέλτιστη πρακτικής εξάσκησης, για παράδειγμα: Εάν έχετε μια Εικονική στο Azure με τον SQL Server, βεβαιωθείτε ότι ακολουθείτε σύσταση ενημερώσεις λογισμικού για το συγκεκριμένο προϊόν.

## <a name="next-steps"></a>Επόμενα βήματα

Χρησιμοποιήστε τις οδηγίες που περιγράφονται σε αυτό το άρθρο για να βοηθηθείτε στον καθορισμό τις καλύτερες επιλογές για ενημερώσεις λογισμικού για εικονικές μηχανές μέσα σε Azure IaaS. Υπάρχουν πολλές ομοιότητες μεταξύ βέλτιστες πρακτικές ενημέρωσης λογισμικού σε ένα κέντρο δεδομένων παραδοσιακή έναντι Azure IaaS, επομένως συνιστάται να αξιολογήσετε το τρέχον πολιτικές ενημέρωσης λογισμικού για να συμπεριλάβετε ΣΠΣ Azure και να συμπεριλάβετε τις σχετικές βέλτιστες πρακτικές από αυτό το άρθρο στη συνολική διαδικασία ενημέρωσης λογισμικού.
