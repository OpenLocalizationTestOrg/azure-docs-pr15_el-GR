<properties
   pageTitle="Λειτουργίες διαχείρισης οικογένεια ασφάλεια και τη γραμμή βάσης λύση ελέγχου | Microsoft Azure"
   description="Αυτό το έγγραφο εξηγεί πώς μπορείτε να χρησιμοποιήσετε ασφάλεια OMS και ελέγχου λύση για να εκτελέσετε μια εκτίμηση γραμμής βάσης για όλους τους υπολογιστές εποπτευόμενη για συμμόρφωση και την ασφάλεια σκοπό."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>

# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Εκτίμηση γραμμής βάσης σε λειτουργίες διαχείρισης οικογένεια προγραμμάτων ασφαλείας και ελέγχου λύσης

Αυτό το έγγραφο σάς βοηθά να χρησιμοποιεί δυνατότητες εκτίμηση γραμμής βάσης [λειτουργίες διαχείρισης οικογένεια προγραμμάτων (OMS) ασφάλειας και ελέγχου λύσης](operations-management-suite-overview.md) για την πρόσβαση της κατάστασης ασφαλούς εποπτευόμενη τους πόρους σας.

## <a name="what-is-baseline-assessment"></a>Τι είναι η εκτίμηση γραμμής βάσης;

Microsoft, μαζί με τις απαιτήσεις του κλάδου και δημόσιους οργανισμούς σε όλο τον κόσμο, καθορίζει μια ρύθμιση παραμέτρων των Windows που αντιπροσωπεύει αναπτύξεις ιδιαίτερα ασφαλή διακομιστή. Αυτή η ρύθμιση παραμέτρων είναι ένα σύνολο κλειδιά μητρώου, οι ρυθμίσεις πολιτικής ελέγχου και ρυθμίσεις πολιτικής ασφαλείας μαζί με συνιστώμενες τιμές της Microsoft για αυτές τις ρυθμίσεις. Αυτό το σύνολο των κανόνων είναι γνωστή ως γραμμή βάσης ασφαλείας. Δυνατοτήτων εκτίμηση γραμμής βάσης OMS ασφαλείας και ελέγχου μπορεί να ανιχνεύσει απρόσκοπτα όλους τους υπολογιστές σας για τη συμμόρφωση. 

Υπάρχουν τρεις τύποι κανόνων:

- **Κανόνες μητρώου**: Βεβαιωθείτε ότι κλειδιά μητρώου έχουν ρυθμιστεί σωστά.
- **Έλεγχος κανόνες πολιτικής**: κανόνες σχετικά με την πολιτική ελέγχου.
- **Κανόνες πολιτική ασφαλείας**: κανόνες σχετικά με τα δικαιώματα του χρήστη στον υπολογιστή.

> [AZURE.NOTE] Διαβάστε [OMS ασφαλείας που χρησιμοποιούνται για την εκτίμηση γραμμής βάσης ρύθμισης παραμέτρων ασφαλείας](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) για μια σύντομη επισκόπηση αυτής της δυνατότητας.

## <a name="security-baseline-assessment"></a>Αξιολόγηση ασφάλειας γραμμής βάσης

Μπορείτε να δείτε την τρέχουσα εκτίμηση γραμμής βάσης ασφαλείας για όλους τους υπολογιστές που παρακολουθούνται από OMS ασφάλειας και ελέγχου με χρήση του πίνακα εργαλείων.  Εκτελέστε τα ακόλουθα βήματα για να αποκτήσετε πρόσβαση στον πίνακα εργαλείων εκτίμηση γραμμής βάσης ασφαλείας:

1. Στην **Οικογένεια προγραμμάτων του Microsoft λειτουργίες διαχείρισης** κύριου πίνακα εργαλείων του, κάντε κλικ στο πλακίδιο **ασφαλείας και ελέγχου** .
2. Στον πίνακα εργαλείων **ασφαλείας και ελέγχου** , κάντε κλικ στην επιλογή **Εκτίμηση γραμμής βάσης** , στην περιοχή **Τομείς ασφαλείας**. Πίνακας εργαλείων **Εκτίμηση γραμμής βάσης ασφαλείας** εμφανίζεται όπως φαίνεται στην παρακάτω εικόνα:
    
    ![Ασφάλεια OMS και ελέγχου εκτίμηση γραμμής βάσης](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Αυτόν τον πίνακα εργαλείων χωρίζεται σε τρεις σημαντικές περιοχές:

- **Υπολογιστές σε σύγκριση με γραμμή βάσης**: Αυτή η ενότητα παρέχει μια σύνοψη των τον αριθμό των υπολογιστών που έχουν πρόσβαση και το ποσοστό των υπολογιστών που μεταβιβάστηκε η αξιολόγηση. Το παρέχει επίσης το πρώτων 10 υπολογιστές και το αποτέλεσμα ποσοστό για την αξιολόγηση.
- **Κατάσταση κανόνων απαιτείται**: Αυτή η ενότητα περιλαμβάνει το σκοπό σας για να μεταφέρετε αναγνώριση των αποτυχημένων κανόνων, της σοβαρότητας και απέτυχε κανόνες με βάση τον τύπο. Εάν ανατρέξετε στο πρώτο γράφημα μπορείτε να προσδιορίσετε γρήγορα εάν οι περισσότερες τους κανόνες αποτυχίας είναι κρίσιμες, ή όχι. Παρέχει επίσης μια λίστα των πρώτων 10 αποτυχίας κανόνων και τους σοβαρότητας. Το δεύτερο γράφημα εμφανίζει τον τύπο του κανόνα που απέτυχε κατά την αξιολόγηση. 
- **Υπολογιστές που λείπουν εκτίμηση γραμμής βάσης**: Αυτή η ενότητα λίστας τους υπολογιστές που δεν έχουν πρόσβαση σε λόγω ασυμβατότητα λειτουργικό σύστημα ή αποτυχίες. 

### <a name="accessing-computers-compared-to-baseline"></a>Πρόσβαση σε υπολογιστές σε σύγκριση με γραμμή βάσης

Ιδανικά όλους τους υπολογιστές σας θα είναι συμβατή με την εκτίμηση γραμμής βάσης ασφαλείας. Ωστόσο αναμένεται ότι σε ορισμένες περιστάσεις αυτό δεν συμβαίνει. Ως μέρος της διαδικασίας διαχείρισης ασφαλείας, είναι σημαντικό να συμπεριλάβετε αναθεώρηση τους υπολογιστές που δεν μπόρεσε να περάσει όλες τις δοκιμές αξιολόγησης ασφαλείας. Ένας γρήγορος τρόπος για την απεικόνιση που είναι με την επιλογή **πρόσβαση σε υπολογιστές** που βρίσκεται στην ενότητα **υπολογιστές σε σύγκριση με γραμμή βάσης** . Θα πρέπει να βλέπετε το αποτέλεσμα αναζήτησης καταγραφής που εμφανίζει τη λίστα των υπολογιστών, όπως φαίνεται στην παρακάτω οθόνη:

![Η πρόσβαση σε υπολογιστή αποτελεσμάτων](./media/oms-security-baseline/oms-security-baseline-fig2.png)

Εμφανίζεται το αποτέλεσμα αναζήτησης σε μορφή πίνακα, όπου η πρώτη στήλη περιλαμβάνει το όνομα του υπολογιστή και το δεύτερο χρώμα έχει τον αριθμό των κανόνων που απέτυχε. Για να ανακτήσετε τις πληροφορίες σχετικά με τον τύπο του κανόνα που απέτυχε, κάντε κλικ στον αριθμό των αποτυχημένων κανόνες εκτός από το όνομα του υπολογιστή. Θα πρέπει να βλέπετε ένα αποτέλεσμα που είναι παρόμοιο με αυτό που φαίνεται στην παρακάτω εικόνα:

![Λεπτομέρειες αποτελέσματα πρόσβαση σε υπολογιστή](./media/oms-security-baseline/oms-security-baseline-fig3.png)

Σε αυτό το αποτέλεσμα αναζήτησης, μπορείτε να έχετε το σύνολο των κανόνων πρόσβαση, τον αριθμό των κρίσιμων κανόνες που απέτυχε, των κανόνων προειδοποίησης και τους κανόνες πληροφοριών απέτυχε.

### <a name="accessing-required-rules-status"></a>Πρόσβαση σε κατάσταση απαραίτητων κανόνων

Μετά τη λήψη πληροφοριών σχετικά με τον αριθμό ποσοστό των υπολογιστών που μεταβιβάστηκε η αξιολόγηση, μπορεί να θέλετε να λάβετε περισσότερες πληροφορίες σχετικά με τους κανόνες αποτυγχάνουν σύμφωνα με την κρισιμότητα. Αυτή η απεικόνιση σάς βοηθά να δίνετε προτεραιότητα σε ποιους υπολογιστές πρέπει να ληφθούν υπόψη πρώτα για να βεβαιωθείτε ότι θα είναι συμβατή με την επόμενη εκτίμηση. Το δείκτη του ποντικιού πάνω από το κρίσιμες τμήμα του γραφήματος που βρίσκεται στο πλακίδιο **απέτυχε κανόνων από σοβαρότητας** , στην περιοχή **απαιτείται κατάστασης κανόνες** και κάντε κλικ σε αυτό. Θα πρέπει να βλέπετε ένα αποτέλεσμα που είναι παρόμοια με την παρακάτω οθόνη:

![Απέτυχε η κανόνες με λεπτομέρειες της σοβαρότητας](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

Σε αυτό το αποτέλεσμα καταγραφής μπορείτε να δείτε τον τύπο του κανόνα γραμμής βάσης που απέτυχε, την περιγραφή αυτού του κανόνα και το Αναγνωριστικό κοινές απαρίθμηση ρύθμισης παραμέτρων (CCE) αυτού του κανόνα ασφαλείας. Αυτά τα χαρακτηριστικά πρέπει να είναι αρκετά για να εκτελέσετε μια ενέργεια διορθωτικοί για να διορθώσετε αυτό το πρόβλημα στον υπολογιστή προορισμού.

> [AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με το CCE, η πρόσβαση του [National ευπάθεια βάσης δεδομένων](https://nvd.nist.gov/cce/index.cfm).

### <a name="accessing-computers-missing-baseline-assessment"></a>Πρόσβαση σε υπολογιστές που λείπουν εκτίμηση γραμμής βάσης

OMS υποστηρίζει το προφίλ γραμμής βάσης μέλος τομέα σε Windows Server 2008 R2 έως και Windows Server 2012 R2. Windows Server 2016 γραμμής βάσης δεν είναι ακόμα τελικό και θα προστεθεί Μόλις δημοσιευτεί. Όλα τα άλλα λειτουργικά συστήματα σαρωμένη μέσω OMS ασφάλειας και ελέγχου εκτίμηση γραμμής βάσης εμφανίζεται κάτω από την ενότητα **υπολογιστές που λείπουν εκτίμηση γραμμής βάσης** .

## <a name="see-also"></a>Δείτε επίσης

Σε αυτό το έγγραφο, μάθατε σχετικά με την εκτίμηση γραμμής βάσης OMS ασφαλείας και ελέγχου. Για να μάθετε περισσότερα σχετικά με την ασφάλεια OMS, ανατρέξτε στα ακόλουθα άρθρα:

- [Επισκόπηση διαχείρισης οικογένεια προγραμμάτων (OMS) λειτουργίες](operations-management-suite-overview.md)
- [Παρακολούθηση και απάντηση σε ειδοποιήσεις ασφαλείας σε λειτουργίες διαχείρισης οικογένεια προγραμμάτων ασφαλείας και ελέγχου λύσης](oms-security-responding-alerts.md)
- [Παρακολούθηση πόρων σε εργασίες διαχείρισης οικογένεια προγραμμάτων ασφαλείας και ελέγχου λύσης](oms-security-monitoring-resources.md)
