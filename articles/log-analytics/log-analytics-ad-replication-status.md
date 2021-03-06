<properties
    pageTitle="Ενεργή κατάσταση αναπαραγωγής καταλόγου λύσης στο αρχείο καταγραφής αναλυτικών στοιχείων | Microsoft Azure"
    description="Το πακέτο λύσης κατάσταση αναπαραγωγής Active Directory τακτικά παρακολουθεί το περιβάλλον σας υπηρεσία καταλόγου Active Directory για τυχόν αποτυχίες αναπαραγωγής αναφορές και τα αποτελέσματα στον πίνακα εργαλείων σας OMS."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="active-directory-replication-status-solution-in-log-analytics"></a>Ενεργή κατάσταση αναπαραγωγής καταλόγου λύσης στο αρχείο καταγραφής αναλυτικών στοιχείων

Υπηρεσία καταλόγου Active Directory είναι βασικό στοιχείο του ένα εταιρικό περιβάλλον ΠΛΗΡΟΦΟΡΙΚΉΣ. Για να εξασφαλίσετε υψηλή διαθεσιμότητα και υψηλές επιδόσεις, κάθε ελεγκτή τομέα που έχει το δικό της αντίγραφο της βάσης δεδομένων της υπηρεσίας καταλόγου Active Directory. Ελεγκτές αναπαραγωγή μεταξύ τους για να μεταδώσετε αλλαγές σε ολόκληρη την επιχείρηση. Αποτυχίες σε αυτήν τη διαδικασία αναπαραγωγής μπορεί να προκαλέσει διάφορα προβλήματα σε όλη την εταιρεία.

Το πακέτο λύσης κατάσταση αναπαραγωγής AD τακτικά παρακολουθεί το περιβάλλον σας υπηρεσία καταλόγου Active Directory για τυχόν αποτυχίες αναπαραγωγής αναφορές και τα αποτελέσματα στον πίνακα εργαλείων σας OMS.

## <a name="installing-and-configuring-the-solution"></a>Εγκατάσταση και ρύθμιση παραμέτρων της λύσης
Χρησιμοποιήστε τις ακόλουθες πληροφορίες για να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους της λύσης.

- Παράγοντες πρέπει να έχει εγκατασταθεί σε ελεγκτές τομέα που είναι μέλη του τομέα, πρέπει να υπολογιστεί ή σε διακομιστές μέλος που έχει ρυθμιστεί για την αποστολή δεδομένων αναπαραγωγής AD OMS. Για να κατανοήσετε τον τρόπο σύνδεσης OMS υπολογιστές με Windows, ανατρέξτε στο θέμα [σύνδεση Windows υπολογιστών καταγραφής ανάλυσης](log-analytics-windows-agents.md). Εάν σας ελεγκτή τομέα είναι ήδη μέρος ένα υπάρχον περιβάλλον Operations Manager κέντρο συστήματος που θέλετε να συνδεθείτε με OMS, ανατρέξτε στο θέμα [Σύνδεση Operations Manager για ανάλυση καταγραφής](log-analytics-om-agents.md).
- Προσθήκη της λύσης κατάσταση αναπαραγωγής Active Directory στο χώρο εργασίας σας OMS χρησιμοποιώντας τη διαδικασία που περιγράφεται στις [λύσεις Προσθήκη αρχείου καταγραφής ανάλυση από τη συλλογή λύσεων](log-analytics-add-solutions.md).  Δεν υπάρχει καμία περαιτέρω ρύθμιση παραμέτρων απαιτείται.


## <a name="ad-replication-status-data-collection-details"></a>Λεπτομέρειες συλλογής δεδομένων κατάστασης αναπαραγωγής AD

Ο παρακάτω πίνακας εμφανίζει μεθόδους συλλογής δεδομένων και άλλες λεπτομέρειες σχετικά με τον τρόπο που συλλέγονται δεδομένα για την κατάσταση αναπαραγωγής AD.

| πλατφόρμα | Άμεση παράγοντα | Παράγοντας SCOM | Azure χώρου αποθήκευσης | SCOM απαιτείται; | SCOM παράγοντας δεδομένων που αποστέλλονται μέσω ομάδας διαχείρισης | συχνότητα συλλογής |
|---|---|---|---|---|---|---|
|Windows|![Ναι](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Ναι](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Όχι](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Όχι](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Ναι](./media/log-analytics-ad-replication-status/oms-bullet-green.png)| κάθε 5 ημερών|


## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Προαιρετικά, ενεργοποιήστε μια ελεγκτή μη τομέα για την αποστολή δεδομένων AD να OMS
Εάν δεν θέλετε να συνδέσετε οποιαδήποτε από τις ελεγκτές τομέα απευθείας OMS, μπορείτε να χρησιμοποιήσετε οποιονδήποτε άλλο υπολογιστή συνδεδεμένο OMS στον τομέα σας να συλλέγετε δεδομένα για το πακέτο λύσης AD αναπαραγωγής κατάστασης και να το στείλετε τα δεδομένα.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Για να ενεργοποιήσετε έναν ελεγκτή μη τομέα για την αποστολή δεδομένων AD να OMS
1.  Βεβαιωθείτε ότι ο υπολογιστής είναι μέλος του τομέα που θέλετε για την παρακολούθηση της χρησιμοποιώντας τη λύση AD αναπαραγωγής κατάστασης.
2.  [Σύνδεση του υπολογιστή Windows OMS](log-analytics-windows-agents.md) ή [συνδεθείτε χρησιμοποιώντας το υπάρχον περιβάλλον Operations Manager για να OMS](log-analytics-om-agents.md), εάν δεν είναι ήδη συνδεδεμένοι.
3.  Σε αυτόν τον υπολογιστή, ορίστε το ακόλουθο κλειδί μητρώου:
    - Κλειδί: **ομάδες HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management\<ManagementGroupName > \Solutions\ADReplication**
    - Τιμή: **IsTarge**
    - Δεδομένα τιμής: **true**

    >[AZURE.NOTE]Αυτές οι αλλαγές θα ισχύσει μέχρι την επανεκκίνηση της υπηρεσίας Microsoft Agent παρακολούθησης (HealthService.exe).

## <a name="understanding-replication-errors"></a>Κατανόηση των σφαλμάτων αναπαραγωγής
Όταν έχετε AD αναπαραγωγής κατάστασης τα δεδομένα που αποστέλλονται σε OMS, θα δείτε ένα πλακίδιο παρόμοιο με το ακόλουθο στον πίνακα εργαλείων OMS που υποδεικνύει τον αριθμό σφάλματα αναπαραγωγής που έχετε τη συγκεκριμένη στιγμή.  
![Κατάσταση αναπαραγωγής AD πλακιδίου](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Κρίσιμα σφάλματα αναπαραγωγής** είναι εκείνα που βρίσκονται στο ή επάνω από το 75% των της [ζωής για απενεργοποίηση](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) για το σύμπλεγμα διακομιστών υπηρεσίας καταλόγου Active Directory.

Όταν κάνετε κλικ στο πλακίδιο, θα δείτε περισσότερες πληροφορίες σχετικά με τα σφάλματα.
![Πίνακας εργαλείων αναπαραγωγής κατάστασης AD](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)


### <a name="destination-server-status-and-source-server-status"></a>Κατάσταση του διακομιστή προορισμού και κατάσταση διακομιστή προέλευσης
Αυτά τα πτερύγια εμφανίζουν την κατάσταση των διακομιστών προορισμού και διακομιστές προέλευσης που αντιμετωπίζετε σφάλματα αναπαραγωγής. Ο αριθμός μετά από κάθε όνομα ελεγκτή τομέα υποδεικνύει τον αριθμό των σφαλμάτων αναπαραγωγής αυτού του ελεγκτή τομέα.

Τα σφάλματα για τους διακομιστές προορισμού και διακομιστές προέλευσης εμφανίζονται επειδή ορισμένα προβλήματα είναι πιο εύκολη για την αντιμετώπιση προβλημάτων από την πλευρά του διακομιστή προέλευσης και οι άλλοι χρήστες από την πλευρά του διακομιστή προορισμού.

Σε αυτό το παράδειγμα, μπορείτε να δείτε ότι πολλοί διακομιστές προορισμού περίπου έχουν τον ίδιο αριθμό των σφαλμάτων, αλλά υπάρχει ένα διακομιστή προέλευσης (ADDC35) που έχει πολλά περισσότερα σφάλματα από όλους τους άλλους. Είναι πιθανό ότι υπάρχει κάποιο πρόβλημα ADDC35 που προκαλεί την αποτυχία για την αποστολή δεδομένων τα μέλη αναπαραγωγής. Διόρθωση προβλημάτων στην ADDC35 πιθανώς επιλύει πολλά από τα σφάλματα που εμφανίζονται στο το blade διακομιστή προορισμού.

### <a name="replication-error-types"></a>Τύποι σφάλματος αναπαραγωγής
Αυτό blade παρέχει πληροφορίες σχετικά με τους τύπους των σφαλμάτων εντοπίζονται σε ολόκληρη την εταιρεία σας. Κάθε σφάλμα έχει έναν μοναδικό κωδικό αριθμητικά και σε ένα μήνυμα που σας βοηθούν να προσδιορίσετε την αρχική αιτία του σφάλματος.

Το δακτύλιος στο επάνω μέρος παρέχει μια γενική σύγκριση του των σφαλμάτων εμφανίζονται περισσότερα και λιγότερο συχνά στο περιβάλλον σας.

Αυτό μπορεί να εμφανίσει όταν πολλαπλούς ελεγκτές αντιμετωπίζετε το ίδιο σφάλμα αναπαραγωγής. Σε αυτήν την περίπτωση, ενδέχεται να μπορείτε να ανακαλύψετε προσδιορίστε μια λύση από έναν ελεγκτή τομέα και, στη συνέχεια, επαναλάβετε την σε άλλους ελεγκτές τομέα που επηρεάζονται από το ίδιο σφάλμα.

### <a name="tombstone-lifetime"></a>Ζωής για απενεργοποίηση
Το ζωής για απενεργοποίηση Καθορίζει πόσος χρόνος ένα διαγραμμένου αντικειμένου, αναφέρεται ως ένα διάρκειας, διατηρούνται της βάσης δεδομένων της υπηρεσίας καταλόγου Active Directory. Όταν ένα διαγραμμένου αντικειμένου διέρχεται η ζωής για απενεργοποίηση, μια διαδικασία συλλογής απορριμμάτων την καταργεί αυτόματα από τη βάση δεδομένων της υπηρεσίας καταλόγου Active Directory.

Η προεπιλεγμένη ζωής για απενεργοποίηση είναι 180 ημέρες για τις πιο πρόσφατες εκδόσεις των Windows, αλλά ήταν 60 ημερών σε παλαιότερες εκδόσεις και μπορεί να αλλάξει ρητά από ένα διαχειριστή της υπηρεσίας καταλόγου Active Directory.

Είναι σημαντικό να γνωρίζετε εάν αντιμετωπίζετε σφάλματα αναπαραγωγής που πλησιάζετε ή έχουν λήξει το ζωής για απενεργοποίηση. Εάν δύο ελεγκτές τομέα αντιμετωπίζετε ένα σφάλμα αναπαραγωγής που εξακολουθεί να εμφανίζεται μετά το ζωής για απενεργοποίηση, αναπαραγωγής θα απενεργοποιηθεί ανάμεσα σε αυτές τις δύο ελεγκτές τομέα, ακόμα και αν είναι σταθερό το υποκείμενο σφάλμα αναπαραγωγής.

Το blade ζωής για απενεργοποίηση σάς βοηθά να προσδιορίσετε θέσεις όπου αυτό είναι σε λειτουργία κίνδυνο. Κάθε σφάλμα στο το **μεγαλύτερη από 100% TSL** κατηγορία αντιπροσωπεύει ένα διαμερίσματα που δεν έχει αναπαραγάγει μεταξύ του διακομιστή προέλευσης και προορισμού για τουλάχιστον την ζωής για απενεργοποίηση για το δάσος.

Σε αυτήν την περίπτωση, απλώς διόρθωση του σφάλματος αναπαραγωγή δεν θα είναι αρκετά. Τουλάχιστον, θα πρέπει να εξετάσετε με μη αυτόματο τρόπο για να προσδιορίσετε και Εκκαθάριση διαγραμμένων αντικειμένων πριν να κάνετε επανεκκίνηση της αναπαραγωγής. Ακόμα και ίσως χρειαστεί να Διακοπή χρήσης ελεγκτή τομέα.

Εκτός από τον εντοπισμό σφάλματα αναπαραγωγής που έχει διατηρηθεί πέρα από το ζωής για απενεργοποίηση, θα θέλετε να πληρώσετε την προσοχή σε τυχόν σφάλματα που υπάγονται στις κατηγορίες **TSL 50 75%** ή **TSL 75-100%** .

Αυτά είναι τα σφάλματα που είναι καθαρά επίμονα, δεν μεταβατικές, ώστε να χρειάζονται πιθανώς τη δική σας παρέμβαση για να επιλύσετε. Τα καλά νέα είναι ότι αυτές δεν έχει φτάσει ακόμη η ζωής για απενεργοποίηση. Εάν μπορείτε να το διορθώσετε αυτά τα προβλήματα αμέσως και *πριν να* φτάσουν τα ζωής για απενεργοποίηση, να κάνετε επανεκκίνηση αναπαραγωγής του με ελάχιστους μη αυτόματη παρέμβαση.

Όπως σημειώθηκε προηγούμενα, το πλακίδιο πίνακα εργαλείων για τη λύση AD αναπαραγωγής κατάστασης εμφανίζει τον αριθμό *κρίσιμη* σφάλματα αναπαραγωγής στο περιβάλλον σας, η οποία καθορίζεται ως σφάλματα που βρίσκονται επάνω από 75% ζωής για απενεργοποίηση (συμπεριλαμβανομένων των σφαλμάτων που είναι μεγαλύτερη από 100% των TSL). Προσπαθήστε να διατηρήσετε αυτόν τον αριθμό με 0.

>[AZURE.NOTE] Όλους τους υπολογισμούς διάρκειας ζωής του ποσοστού είναι με βάση το ζωής για απενεργοποίηση πραγματική για το σύμπλεγμα διακομιστών υπηρεσίας καταλόγου Active Directory, ώστε να μπορείτε να εμπιστευτείτε ότι αυτά τα ποσοστά είναι ακριβείς, ακόμα και εάν έχετε μια τιμή διάρκειας ζωής απενεργοποίηση προσαρμοσμένη ρύθμιση.

### <a name="ad-replication-status-details"></a>Λεπτομέρειες κατάστασης αναπαραγωγής AD
Όταν κάνετε κλικ σε οποιοδήποτε στοιχείο σε μία από τις λίστες, θα δείτε πρόσθετες λεπτομέρειες σχετικά με αυτήν με χρήση της καταγραφής αναζήτησης. Τα αποτελέσματα είναι φιλτραρισμένο για εμφάνιση μόνο των σφαλμάτων που σχετίζονται με αυτό το στοιχείο. Για παράδειγμα, εάν κάνετε κλικ στον πρώτο ελεγκτή τομέα που παρατίθενται στην περιοχή **Κατάσταση διακομιστή προορισμού (ADDC02)**, θα δείτε αποτελέσματα αναζήτησης φιλτραρισμένο για εμφάνιση σφαλμάτων με αυτόν τον ελεγκτή τομέα που αναφέρονται ως διακομιστή προορισμού:

![Σφάλματα κατάσταση αναπαραγωγής AD στα αποτελέσματα αναζήτησης](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Από εδώ, μπορείτε να φιλτράρετε περαιτέρω, να τροποποιήσετε το ερώτημα αναζήτησης και ούτω καθεξής. Για περισσότερες πληροφορίες σχετικά με τη χρήση της αναζήτησης καταγραφής, ανατρέξτε στο θέμα [αρχείο καταγραφής αναζητήσεις](log-analytics-log-searches.md).

Το πεδίο **HelpLink** εμφανίζει τη διεύθυνση URL της σελίδας στο TechNet με πρόσθετες λεπτομέρειες σχετικά με αυτό το συγκεκριμένο σφάλμα. Μπορείτε να αντιγράψετε και να επικολλήσετε αυτή τη σύνδεση σε παράθυρο του προγράμματος περιήγησης για να δείτε πληροφορίες σχετικά με την αντιμετώπιση προβλημάτων και τη διόρθωση του σφάλματος.

Μπορείτε επίσης να επιλέξετε την **Εξαγωγή** για να εξαγάγετε τα αποτελέσματα στο Excel. Αυτό σας επιτρέπει να απεικονίσετε δεδομένα σφάλματος αναπαραγωγής με οποιονδήποτε τρόπο που θέλετε.

![εξαγόμενα AD αναπαραγωγής κατάσταση σφάλματα στο Excel](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>Κατάσταση αναπαραγωγής AD συνήθεις Ερωτήσεις
**Ε: πώς συχνά είναι AD αναπαραγωγής κατάστασης δεδομένα ενημερώνονται;**
Α: οι πληροφορίες είναι ενημερωθεί κάθε 5 ημερών.

**Ε: υπάρχει ένας τρόπος για να ρυθμίσετε τη συχνότητα ανανέωσης των αυτά τα δεδομένα;**
Α: αυτήν τη στιγμή.

**Ε: χρειάζομαι για να προσθέσετε όλες οι ελεγκτές τομέα μου στο χώρο εργασίας μου OMS προκειμένου να δουν την κατάσταση αναπαραγωγής;**
Α: όχι, πρέπει να προστεθεί μόνο έναν μεμονωμένο ελεγκτή τομέα. Εάν έχετε πολλούς ελεγκτές τομέα στο χώρο εργασίας σας OMS, τα δεδομένα από όλες τις αποστέλλεται OMS.

**Ερώτηση: δεν θέλετε να προσθέσετε οποιοδήποτε ελεγκτές τομέα στο χώρο εργασίας μου OMS. Μπορώ να χρησιμοποιήσω τη λύση AD αναπαραγωγής κατάσταση;**
Α: Ναι. Μπορείτε να ορίσετε την τιμή ενός κλειδιού μητρώου για να ενεργοποιήσετε αυτή. Ανατρέξτε στο θέμα [για να ενεργοποιήσετε έναν ελεγκτή μη τομέα για την αποστολή δεδομένων AD να OMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**Ε: Τι είναι το όνομα της διαδικασίας που κάνει τη συλλογή δεδομένων;**
Α: AdvisorAssessment.exe

**Ε: πόσος χρόνος χρειάζεται για τα δεδομένα που συλλέγονται;**
Α: τα δεδομένα ώρα συλλογής εξαρτάται από το μέγεθος του περιβάλλοντος υπηρεσίας καταλόγου Active Directory, αλλά συνήθως διαρκεί λιγότερο από 15 λεπτά.

**Ε: Τι τύπο των δεδομένων συλλέγονται;**
Α: αναπαραγωγή πληροφορίες που συλλέγονται μέσω LDAP.

**Ε: υπάρχει ένας τρόπος για να ρυθμίσετε τις παραμέτρους όταν συλλέγονται δεδομένα;**
Α: αυτήν τη στιγμή.

**Ε: ποια δικαιώματα χρειάζομαι για τη συλλογή δεδομένων;**
Α: κανονική χρήστη δικαιώματα για την υπηρεσία καταλόγου Active Directory επαρκούν συνήθως.

## <a name="troubleshoot-data-collection-problems"></a>Αντιμετώπιση προβλημάτων συλλογής δεδομένων
Για να συλλέξετε δεδομένα, το πακέτο λύσης κατάσταση αναπαραγωγής AD απαιτεί τουλάχιστον μία ελεγκτή τομέα για να συνδεθείτε με το χώρο εργασίας OMS. Μέχρι να κάνετε αυτό, θα δείτε ένα μήνυμα που υποδεικνύει που **συλλέγονται δεδομένα**.

Εάν χρειάζεστε βοήθεια για τη σύνδεση σε μία από τις ελεγκτές τομέα, μπορείτε να προβάλετε τεκμηρίωση στο [υπολογιστές σύνδεση Windows την ανάλυση καταγραφής](log-analytics-windows-agents.md). Εναλλακτικά, εάν ελεγκτή του τομέα σας είναι ήδη συνδεδεμένη με ένα υπάρχον περιβάλλον Operations Manager κέντρο συστήματος, μπορείτε να προβάλετε τεκμηρίωση στο [Σύνδεση συστήματος Center Operations Manager για ανάλυση καταγραφής](log-analytics-om-agents.md).

Εάν δεν θέλετε να συνδέσετε οποιοδήποτε από ελεγκτές τομέα απευθείας OMS ή στο SCOM, ανατρέξτε στο θέμα [για να ενεργοποιήσετε έναν ελεγκτή μη τομέα για την αποστολή δεδομένων AD να OMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).


## <a name="next-steps"></a>Επόμενα βήματα

- Χρησιμοποιήστε [αναζητήσεις καταγραφής στο αρχείο καταγραφής αναλυτικών στοιχείων](log-analytics-log-searches.md) για να προβάλετε τα λεπτομερή δεδομένα κατάστασης αναπαραγωγή του Active Directory.
