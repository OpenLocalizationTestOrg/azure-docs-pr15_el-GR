<properties
   pageTitle="Χρήση του Κέντρου ασφαλείας Azure για μια απόκριση περιστατικού | Microsoft Azure"
   description="Αυτό το έγγραφο εξηγεί τον τρόπο για να χρησιμοποιήσετε το Κέντρο ασφάλειας Azure για ένα σενάριο απόκριση περιστατικού."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>

# <a name="using-azure-security-center-for-an-incident-response"></a>Χρήση του Κέντρου ασφαλείας Azure για μια απόκριση περιστατικού
Πολλές εταιρείες μάθετε τον τρόπο ανταπόκρισης σε περιστατικά ασφαλείας μόνο αφού πάσχει επίθεση. Για να μειώσετε το κόστος και οι βλάβες, είναι σημαντικό να έχετε μια απόκριση περιστατικού Σχεδιασμός στη θέση πριν από την επίθεση τίθεται σε ισχύ. Μπορείτε να χρησιμοποιήσετε το Κέντρο ασφάλειας Azure σε διαφορετικά στάδια από μια απόκριση περιστατικού.

## <a name="incident-response-planning"></a>Απόκριση περιστατικού σχεδιασμού

Μια αποτελεσματική πρόγραμμα εξαρτάται από τρεις βασικές δυνατότητες: τη δυνατότητα να προστατεύσετε, να εντοπίσει και να απαντούν σε απειλές. Προστασία είναι σχετικά με την αποτροπή περιστατικά, εντοπισμού είναι σχετικά με τον προσδιορισμό απειλές πρώιμη και η απάντηση είναι σχετικά με την evicting ο εισβολέας και επαναφορά συστήματα για τον περιορισμό των επιπτώσεων παραβιάσεις.

Σε αυτό το άρθρο θα χρησιμοποιήσει τα στάδια απόκριση περιστατικού ασφαλείας από το άρθρο [Απάντηση ασφαλείας του Microsoft Azure στο Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) , όπως φαίνεται στο ακόλουθο διάγραμμα:

![Απόκριση περιστατικού κύκλου ζωής](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Μπορείτε να χρησιμοποιήσετε το Κέντρο ασφάλειας κατά τα στάδια Αυτόματος εντοπισμός, εκτίμηση και διάγνωση. Ακολουθούν παραδείγματα πώς Κέντρο ασφάλειας μπορεί να είναι χρήσιμες κατά τα τρία στάδια αρχική απόκριση περιστατικού:

- **Αυτόματος εντοπισμός**: Ελέγξτε την πρώτη ένδειξη της έρευνας συμβάν.
    - Παράδειγμα: αναθεώρηση το αρχικό μήνυμα επαλήθευσης ότι μια προειδοποίηση ασφαλείας υψηλής προτεραιότητας προκλήθηκε στον πίνακα εργαλείων Κέντρο ασφάλειας.
- **Εκτίμηση**: εκτέλεση αρχική εκτίμηση για να λάβετε περισσότερες πληροφορίες σχετικά με τη δραστηριότητα ύποπτες.
    - Παράδειγμα: λάβετε περισσότερες πληροφορίες σχετικά με την προειδοποίηση ασφαλείας.
- **Διάγνωση**: διεξαγωγή μια τεχνική έρευνα και να την προσδιορίσετε στρατηγικές περιορισμού, μετριασμού και λύση.
    - Παράδειγμα: ακολουθήστε τα βήματα αποκατάσταση εύρυθμης λειτουργίας που περιγράφεται από το Κέντρο ασφάλειας στην ειδοποίηση που συγκεκριμένης ασφαλείας.

Το σενάριο που ακολουθεί δείχνει πώς μπορείτε να αξιοποιήσετε Κέντρο ασφάλειας κατά τη διάρκεια των σταδίων Αυτόματος εντοπισμός, εκτίμηση και διάγνωση/απόκριση περιστατικού ασφαλείας. Στο Κέντρο ασφάλειας, ένα [συμβάν ασφαλείας](security-center-incident.md) είναι μια συγκέντρωση όλες τις ειδοποιήσεις για έναν πόρο που στοίχιση με μοτίβα [τερματισμού αλυσίδα](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Περιστατικά εμφανίζονται στο πλακίδιο [ειδοποιήσεων ασφαλείας](security-center-managing-and-responding-alerts.md) και blade. Ένα περιστατικό αποκαλύπτει τη λίστα των σχετικών ειδοποιήσεις, που σας επιτρέπει να λάβετε περισσότερες πληροφορίες σχετικά με κάθε παρουσία. Κέντρο ασφάλειας παρουσιάζει επίσης μεμονωμένη ειδοποιήσεων ασφαλείας που μπορούν να χρησιμοποιηθούν για να εντοπίσετε μια ύποπτη δραστηριότητα.

## <a name="scenario"></a>Σενάριο

Contoso πρόσφατα μετεγκατασταθεί ορισμένους από τους πόρους στην εσωτερική εγκατάσταση Azure, συμπεριλαμβανομένων ορισμένες βάσει εικονική μηχανή γραμμής εταιρικά όγκου εργασίας και βάσεις δεδομένων SQL. Προς το παρόν, της Contoso πυρήνα υπολογιστή ασφαλείας περιστατικό απόκριση ομάδας (CSIRT) έχει πρόβλημα Διερεύνηση θέματα ασφάλειας λόγω δεν γίνεται ενοποιημένο με το τρέχον εργαλεία απόκριση περιστατικού πληροφοριών ασφαλείας. Αυτή η έλλειψη ενοποίησης παρουσιάζει πρόβλημα κατά τη διάρκεια του σταδίου εντοπισμός (θετικά πάρα πολλά false), καθώς και κατά την εκτίμηση και διάγνωση στάδια. Ως μέρος της αυτή η μετεγκατάσταση, τους αποφασίσει να επιλέξετε για το Κέντρο ασφάλειας για να βοηθήσει τους διεύθυνση σε αυτό το πρόβλημα.

Η πρώτη φάση της αυτή η μετεγκατάσταση ολοκληρώθηκε μετά από τους onboarded όλους τους πόρους και απευθύνεται όλες τις συστάσεις ασφαλείας από το Κέντρο ασφάλειας. Contoso CSIRT είναι το σημείο εστίασης για την αντιμετώπιση περιστατικά ασφαλείας υπολογιστή. Η ομάδα αποτελείται από μια ομάδα ατόμων με αρμοδιότητες για την αντιμετώπιση τυχόν περιστατικό ασφαλείας. Τα μέλη της ομάδας έχουν που ορίζονται με σαφήνεια καθηκόντων για να βεβαιωθείτε ότι θα παραμείνει χωρίς περιοχή απάντηση ακάλυπτο.

Για αυτό το σενάριο, θα κάνουμε εστίαση στην τους ρόλους από τα εξής φανταστικά πρόσωπα που αποτελούν μέρος της Contoso CSIRT:

![Απόκριση περιστατικού κύκλου ζωής](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Judy είναι σε λειτουργίες ασφαλείας. Το αρμοδιότητες περιλαμβάνουν τα εξής:

- Παρακολούθηση και απάντηση σε κινδύνους ασφάλειας όλο το 24ωρο.
- Κλιμάκωση στον κάτοχο φόρτο εργασίας cloud ή αναλυτής ασφαλείας, όπως απαιτείται.

SAM είναι ένας αναλυτής ασφαλείας και ευθυνών περιλαμβάνουν:

- Διερεύνηση επιθέσεις.
- Αποκατάσταση ειδοποιήσεις.
- Εργασία με κάτοχοι φόρτο εργασίας για να προσδιορίσετε και να εφαρμόσετε μετριασμούς.

Όπως μπορείτε να δείτε, Judy και Sam έχουν διαφορετικές αρμοδιότητες και που πρέπει να συνεργάζονται για κοινή χρήση πληροφοριών Κέντρο ασφάλειας.

## <a name="recommended-solution"></a>Προτεινόμενη λύση

Επειδή το Judy και Sam έχουν διαφορετικούς ρόλους, αυτά θα χρησιμοποιείτε διαφορετικές περιοχές του Κέντρο ασφάλειας για να αποκτήσετε τις σχετικές πληροφορίες για τις καθημερινές δραστηριότητες. Judy θα χρησιμοποιήσει **ειδοποιήσεων ασφαλείας** ως μέρος της καθημερινή εποπτεία.

![Προειδοποιήσεις ασφαλείας](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Judy θα χρησιμοποιήσει ειδοποιήσεων ασφαλείας στη διάρκεια τα στάδια εντοπισμός και εκτίμηση. Αφού ολοκληρωθεί η αρχική εκτίμηση Judy, που μπορεί να Προωθήστε το θέμα στη Sam εάν απαιτείται επιπλέον έρευνα. Σε αυτό το σημείο, Sam θα χρησιμοποιήσει τις πληροφορίες που παρέχεται από το Κέντρο ασφάλειας, μερικές φορές σε συνδυασμό με άλλες προελεύσεις δεδομένων, για να μετακινηθείτε στο στάδιο της διάγνωση.


## <a name="how-to-implement-this-solution"></a>Πώς μπορείτε να υλοποιήσετε αυτήν τη λύση

Για να δείτε πώς θα χρησιμοποιήσετε Κέντρο ασφάλειας Azure σε ένα σενάριο απόκριση περιστατικού, θα σας θα, ακολουθήστε τα βήματα της Judy τα στάδια εντοπισμός και εκτίμηση και, στη συνέχεια, ανατρέξτε στο θέμα Πώς επηρεάζει η Sam για να εντοπίσετε το πρόβλημα.

### <a name="detect-and-assess-incident-response-stages"></a>Εντοπισμός και εκτίμηση στάδια απόκριση περιστατικού

Judy πραγματοποιήσει είσοδο στην πύλη του Azure και λειτουργεί στην κονσόλα Κέντρο ασφάλειας. Ως μέρος της ημερήσια δραστηριότητες παρακολούθησης, κάνει αποτελέσματα εξέταση των ειδοποιήσεων ασφαλείας υψηλής προτεραιότητας, ακολουθώντας τα παρακάτω βήματα:

1. Κάντε κλικ στο πλακίδιο **ειδοποιήσεων ασφαλείας** και πρόσβασης του blade **ειδοποιήσεων ασφαλείας** .
    ![Blade προειδοποίησης ασφαλείας](./media/security-center-incident-response/security-center-incident-response-fig4.png)

    > [AZURE.NOTE] Για αυτό το σενάριο, Judy πρόκειται για να εκτελέσετε μια εκτίμηση στην ειδοποίηση δραστηριότητας κακόβουλο SQL, όπως φαίνεται στην προηγούμενη εικόνα.
2. Κάντε κλικ στην ειδοποίηση **δραστηριότητας κακόβουλο SQL** και αναθεώρηση προσβεβλημένα πόρων σε το blade **δραστηριότητας κακόβουλο SQL** :  ![λεπτομέρειες συμβάντος](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    Σε αυτό το blade, Judy να κρατήσετε σημειώσεις σχετικά με τους πόρους προσβεβλημένα, πώς συνέβη πολλές φορές αυτήν την επίθεση και πότε εντοπίστηκε.
3. Κάντε κλικ στην επιλογή το **επίθεση πόρων** για να λάβετε περισσότερες πληροφορίες σχετικά με αυτήν την επίθεση.

Μετά την ανάγνωση της περιγραφής, Άννα έχει πειστεί ότι δεν πρόκειται για μια ψευδές θετικό αποτέλεσμα και ότι θα πρέπει να Προωθήστε αυτήν την περίπτωση να Sam.

### <a name="diagnose-incident-response-stage"></a>Διάγνωση απόκριση περιστατικού στάδιο

SAM λαμβάνει την υπόθεση από Judy και ξεκινά η εξέταση των βημάτων αποκατάσταση εύρυθμης λειτουργίας που προτείνονται Κέντρο ασφάλειας.

![Απόκριση περιστατικού κύκλου ζωής](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>Πρόσθετοι πόροι

Στην ομάδα απόκριση περιστατικού επίσης επωφεληθείτε από τη δυνατότητα [Power BI Κέντρο ασφάλειας](security-center-powerbi.md) για να δείτε διάφορους τύπους αναφορών. Αυτές οι αναφορές μπορούν να σας βοηθήσουν τους κατά τη διάρκεια περαιτέρω έρευνας για να απεικονίσετε, ανάλυση και το φιλτράρισμα συστάσεις και οι προειδοποιήσεις ασφαλείας. Για εταιρείες που χρησιμοποιούν τις πληροφορίες ασφαλείας και συμβάντων λύση διαχείρισης (SIEM) κατά τη διαδικασία της έρευνας, επίσης, μπορούν να [ενοποιήσετε το Κέντρο ασφάλειας με τη λύση τους](security-center-integrating-alerts-with-log-integration.md). Επίσης, μπορείτε να ενοποιήσετε αρχεία καταγραφής ελέγχου Azure και συμβάντα ασφαλείας εικονική μηχανή (Εικονική), χρησιμοποιώντας το [εργαλείο ενοποίησης Azure καταγραφής](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/). Για να εξερευνήσετε επίθεση, μπορείτε να χρησιμοποιήσετε αυτές τις πληροφορίες σε συνδυασμό με τις πληροφορίες που παρέχει το Κέντρο ασφάλειας.


## <a name="conclusion"></a>Ολοκλήρωση

Συναρμολόγησης μιας ομάδας πριν από τον ένα περιστατικό είναι πολύ σημαντικό για την εταιρεία σας και θα θετικά επηρεάζουν τον τρόπο χειρισμού συμβάντων. Οι στα σωστά εργαλεία για την παρακολούθηση της πόροι μπορούν να αυτήν την ομάδα για να τα ακριβή βήματα για τη διόρθωση ένα συμβάν ασφαλείας. Κέντρο ασφάλειας [δυνατότητες ανίχνευσης](security-center-detection-capabilities.md) μπορεί να βοηθήσει IT για να απαντήσετε σε περιστατικά ασφαλείας γρήγορα και διόρθωση θέματα ασφάλειας.