<properties
    pageTitle="Παρακολούθηση VMware λύσης στο αρχείο καταγραφής ανάλυσης | Microsoft Azure"
    description="Μάθετε σχετικά με το πώς η λύση VMware παρακολούθηση μπορεί να σας βοηθήσει διαχειριστείτε τα αρχεία καταγραφής και εποπτεία του ESXi hosts."
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
    ms.date="10/28/2016"
    ms.author="banders"/>

# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>Λύση VMware παρακολούθησης (έκδοση Preview) στο αρχείο καταγραφής αναλυτικών στοιχείων

Η λύση VMware παρακολούθησης στο αρχείο καταγραφής ανάλυσης είναι μια λύση που σας βοηθά να δημιουργήσετε μια κεντρική καταγραφή και παρακολούθηση προσέγγιση για μεγάλα αρχεία καταγραφής VMware. Σε αυτό το άρθρο περιγράφει πώς μπορείτε να αντιμετώπιση προβλημάτων, καταγραφή, και να διαχειριστείτε τους κεντρικούς υπολογιστές ESXi μια ενιαία θέση χρησιμοποιώντας τη λύση. Με τη λύση, μπορείτε να δείτε λεπτομερή δεδομένα για όλες τις hosts ESXi σε μία μόνο θέση. Μπορείτε να δείτε καταμετρά επάνω συμβάντος, κατάσταση και τάσεων Εικονική και ESXi κεντρικών υπολογιστών που παρέχεται μέσω των αρχείων καταγραφής ESXi κεντρικού υπολογιστή. Μπορείτε να αντιμετωπίσετε κατά την προβολή και αναζήτηση κεντρική αρχεία καταγραφής host ESXi. Και, μπορείτε να δημιουργήσετε ειδοποιήσεις που βασίζονται σε ερωτήματα αναζήτησης αρχείου καταγραφής.

Η λύση χρησιμοποιεί εγγενούς syslog λειτουργικότητα του κεντρικού υπολογιστή ESXi push δεδομένα σε έναν προορισμό Εικονική, η οποία έχει OMS παράγοντα. Ωστόσο, η λύση δεν Συντάξτε αρχεία στο syslog μέσα σε Εικονική προορισμού. Ο παράγοντας OMS ανοίγει θύρα 1514 και παρακολουθεί σε αυτό. Μόλις που λαμβάνει τα δεδομένα, τον παράγοντα OMS προωθεί τα δεδομένα σε OMS.

## <a name="installing-and-configuring-the-solution"></a>Εγκατάσταση και ρύθμιση παραμέτρων της λύσης

Χρησιμοποιήστε τις ακόλουθες πληροφορίες για να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους της λύσης.

- Προσθήκη της λύσης VMware παρακολούθησης στο χώρο εργασίας σας OMS χρησιμοποιώντας τη διαδικασία που περιγράφεται στις [λύσεις Προσθήκη αρχείου καταγραφής ανάλυση από τη συλλογή λύσεων](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>Υποστηριζόμενες VMware ESXi hosts
vSphere ESXi Host 5,5 και 6.0

#### <a name="prepare-a-linux-server"></a>Προετοιμασία ενός διακομιστή Linux
Δημιουργήστε ένα λειτουργικό σύστημα Linux Εικονική για τη λήψη όλων των δεδομένων syslog από τους κεντρικούς υπολογιστές ESXi. Ο [Παράγοντας Linux OMS](log-analytics-linux-agents.md) είναι το σημείο συλλογής για όλα τα δεδομένα syslog ESXi κεντρικού υπολογιστή. Μπορείτε να χρησιμοποιήσετε πολλούς κεντρικούς υπολογιστές ESXi για την προώθηση των αρχείων καταγραφής σε ένα μεμονωμένο διακομιστή Linux, όπως στο παρακάτω παράδειγμα.  

   ![SYSLOG ροής](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Ρύθμιση παραμέτρων συλλογής syslog

1. Ρύθμιση της προώθησης syslog για VSphere. Για λεπτομερείς πληροφορίες για να ρυθμίσετε την προώθηση syslog, ανατρέξτε στο θέμα [τη ρύθμιση των παραμέτρων syslog στην ESXi 5.x και 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Μεταβείτε στις επιλογές **Ρύθμιση παραμέτρων κεντρικού υπολογιστή ESXi** > **λογισμικό** > **ρυθμίσεις για προχωρημένους** > **Syslog**.
  ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  

2. Στο πεδίο *Syslog.global.logHost* , προσθέστε το διακομιστή Linux και του αριθμού θύρας *1514*. Για παράδειγμα, `tcp://hostname:1514` ή`tcp://123.456.789.101:1514`

3. Ανοίξτε το τείχος προστασίας host ESXi για syslog. **Ρύθμιση παραμέτρων κεντρικού υπολογιστή ESXi** > **λογισμικό** > **Προφίλ ασφαλείας** > **τείχος προστασίας** και άνοιγμα **Ιδιότητες**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  

4. Ελέγξτε το vSphere Console για να βεβαιωθείτε ότι αυτό syslog έχει οριστεί σωστά προς τα επάνω. Επιβεβαίωση στον κεντρικό υπολογιστή ESXI αυτήν τη θύρα **1514** έχει ρυθμιστεί.

5. Ελέγξτε τη σύνδεση μεταξύ του διακομιστή Linux και τον κεντρικό υπολογιστή του ESXi χρησιμοποιώντας το `nc` εντολής στον κεντρικό υπολογιστή ESXi. Για παράδειγμα:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

6. Λήψη και εγκατάσταση τον παράγοντα OMS για Linux στο διακομιστή Linux. Για περισσότερες πληροφορίες, ανατρέξτε στην [τεκμηρίωση για παράγοντας OMS για Linux](https://github.com/Microsoft/OMS-Agent-for-Linux).

7. Μετά την εγκατάσταση του τον παράγοντα OMS για Linux, μεταβείτε στον κατάλογο /etc/opt/microsoft/omsagent/sysconf/omsagent.d και αντιγράψτε το αρχείο vmware_esxi.conf στον κατάλογο /etc/opt/microsoft/omsagent/conf/omsagent.d και την αλλαγή του κατόχου/ομάδας και τα δικαιώματα του αρχείου. Για παράδειγμα:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```

8.  Επανεκκινήστε τον παράγοντα OMS για Linux, εκτελώντας `sudo /opt/microsoft/omsagent/bin/service_control restart`.

9. Στην πύλη του OMS, εκτελέστε αναζήτηση καταγραφής για `Type=VMware_CL`. Όταν OMS συλλέγει τα δεδομένα syslog, διατηρεί τη μορφοποίηση syslog. Στην πύλη, ορισμένα συγκεκριμένα πεδία καταγράφονται, όπως το *όνομα κεντρικού υπολογιστή* και το *όνομα διεργασίας*.  

    ![Τύπος](./media/log-analytics-vmware/type.png)  

    Εάν τα αποτελέσματα της αναζήτησης καταγραφής προβολής είναι παρόμοια με την παραπάνω εικόνα, είστε ρυθμίσετε για χρήση του πίνακα εργαλείων παρακολούθησης VMware OMS λύση.  

## <a name="vmware-data-collection-details"></a>Λεπτομέρειες συλλογής δεδομένων VMware

Η λύση παρακολούθησης VMware συλλέγει διάφορες μετρήσεις και καταγραφής δεδομένων για τις επιδόσεις από ESXi hosts με τους παράγοντες OMS για Linux που έχετε ενεργοποιήσει.

Ο παρακάτω πίνακας εμφανίζει μεθόδους συλλογής δεδομένων και άλλες λεπτομέρειες σχετικά με τον τρόπο που συλλέγονται δεδομένα.

| πλατφόρμα | Παράγοντας OMS για Linux | Παράγοντας SCOM | Azure χώρου αποθήκευσης | SCOM απαιτείται; | SCOM παράγοντας δεδομένων που αποστέλλονται μέσω ομάδας διαχείρισης | συχνότητα συλλογής |
|---|---|---|---|---|---|---|
|Linux|![Ναι](./media/log-analytics-vmware/oms-bullet-green.png)|![Όχι](./media/log-analytics-vmware/oms-bullet-red.png)|![Όχι](./media/log-analytics-vmware/oms-bullet-red.png)|            ![Όχι](./media/log-analytics-containers/oms-bullet-red.png)|![Όχι](./media/log-analytics-vmware/oms-bullet-red.png)| κάθε 3 λεπτά|


Στον παρακάτω πίνακα εμφανίζονται παραδείγματα των πεδίων δεδομένων που συλλέγονται από τη λύση VMware παρακολούθηση:

| όνομα πεδίου | Περιγραφή |
| --- | --- |
| Device_s| Συσκευές αποθήκευσης VMware |
| ESXIFailure_s | Αποτυχία τύπων |
| EventTime_t | ώρα που παρουσιάστηκε συμβάντος |
| HostName_s | Όνομα κεντρικού υπολογιστή ESXi |
| Operation_s | Δημιουργία Εικονική ή διαγραφή Εικονική |
| ProcessName_s | όνομα συμβάντος |
| ResourceId_s | το όνομα του κεντρικού υπολογιστή VMware |
| ResourceLocation_s | VMware |
| ResourceName_s | VMware |
| ResourceType_s | Hyper-V |
| SCSIStatus_s | Κατάσταση VMware SCSI |
| SyslogMessage_s | SYSLOG δεδομένων |
| UserName_s | χρήστης που έχουν δημιουργηθεί ή διαγραφεί Εικονική |
| VMName_s | Εικονική όνομα |
| Υπολογιστή | κεντρικός υπολογιστής |
| TimeGenerated | χρόνο που έχει δημιουργηθεί με τα δεδομένα |
| DataCenter_s | Κέντρο δεδομένων VMware |
| StorageLatency_s | λανθάνων χρόνος αποθήκευσης (ms) |

## <a name="vmware-monitoring-solution-overview"></a>Επισκόπηση παρακολούθησης VMware λύσης

Το πλακίδιο VMware εμφανίζεται στην πύλη του OMS. Παρέχει μια προβολή υψηλού επιπέδου οποιαδήποτε αποτυχιών. Όταν κάνετε κλικ στο πλακίδιο, μεταβείτε στην προβολή του πίνακα εργαλείων.

![πλακίδιο](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Μεταβείτε στην προβολή πίνακα εργαλείων

Στην προβολή πίνακα εργαλείων **VMware** , λεπίδες οργανώνονται κατά:

- Μέτρηση κατάστασης αποτυχίας
- Καταμετρά Host επάνω από το συμβάν
- Καταμετρά επάνω συμβάντος
- Δραστηριότητες εικονική μηχανή
- ESXi Host δίσκου συμβάντα


![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Κάντε κλικ σε οποιαδήποτε blade για να ανοίξετε το παράθυρο αναζήτησης αρχείου καταγραφής ανάλυσης που εμφανίζει συγκεκριμένες για την blade λεπτομερείς πληροφορίες.

Από εδώ, μπορείτε να επεξεργαστείτε το ερώτημα αναζήτησης για να το τροποποιήσετε για ένα συγκεκριμένο. Για ένα πρόγραμμα εκμάθησης στην τα βασικά στοιχεία του OMS αναζήτησης, ανατρέξτε στο θέμα το [πρόγραμμα εκμάθησης αναζήτησης καταγραφής OMS.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>Βρείτε ESXi host συμβάντα

Έναν κεντρικό υπολογιστή ESXi δημιουργεί πολλά αρχεία καταγραφής, με βάση τις διαδικασίες. Η λύση παρακολούθησης VMware συγκεντρώνει τους και συνοψίζει τις μετρήσεις του συμβάντος. Αυτή η συγκεντρωτική προβολή σάς βοηθά να κατανοήσετε ποιος κεντρικός υπολογιστής ESXi έχει μεγάλο όγκο συμβάντα και ποια συμβάντα προκύπτουν συχνότερα στο περιβάλλον σας.

![συμβάν](./media/log-analytics-vmware/events.png)

Μπορείτε να κάνετε γενίκευση περαιτέρω κάνοντας κλικ σε μια υπηρεσία παροχής φιλοξενίας ESXi ή ένας τύπος συμβάντος.

Όταν κάνετε κλικ σε ένα όνομα κεντρικού υπολογιστή ESXi, μπορείτε να προβάλετε πληροφορίες από αυτόν τον κεντρικό υπολογιστή ESXi. Εάν θέλετε να περιορίσετε τα αποτελέσματα με τον τύπο του συμβάντος, προσθέστε `“ProcessName_s=EVENT TYPE”` στο ερώτημα αναζήτησης. Μπορείτε να επιλέξετε **όνομα διεργασίας** στο φίλτρο αναζήτησης. Που περιορίζει τις πληροφορίες για εσάς.

![Διερεύνηση](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Εύρεση υψηλή Εικονική δραστηριοτήτων

Μια εικονική μηχανή είναι δυνατό να δημιουργηθούν και να διαγραφούν από οποιαδήποτε ESXi κεντρικού υπολογιστή. Είναι χρήσιμο διαχειριστή, για να προσδιορίσετε πόσες ΣΠΣ δημιουργεί μια υπηρεσία παροχής φιλοξενίας ESXi. Αυτό στο-ενεργοποίηση, σάς βοηθά να κατανοήσετε επιδόσεων και χωρητικότητας. Παρακολούθηση των συμβάντων δραστηριότητας Εικονική είναι κρίσιμης σημασίας κατά τη Διαχείριση το περιβάλλον σας.

![Διερεύνηση](./media/log-analytics-vmware/vmactivities1.png)

Εάν θέλετε να δείτε πρόσθετα δεδομένα δημιουργίας ESXi host Εικονική, κάντε κλικ σε ένα όνομα κεντρικού υπολογιστή ESXi.

![Διερεύνηση](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Κοινά ερωτήματα αναζήτησης

Η λύση περιλαμβάνει άλλες χρήσιμες ερωτήματα που σας βοηθούν να διαχειριστείτε το ESXi κεντρικούς υπολογιστές, όπως το χώρο αποθήκευσης υψηλού, λανθάνων χρόνος χώρου αποθήκευσης και αποτυχία διαδρομή.

![ερωτήματα](./media/log-analytics-vmware/queries.png)

#### <a name="save-queries"></a>Αποθήκευση ερωτημάτων

Αποθήκευση ερωτημάτων αναζήτησης είναι μια τυπική δυνατότητα OMS και να σας βοηθήσει να διατηρήσετε όλα τα ερωτήματα που βρήκατε χρήσιμες. Αφού δημιουργήσετε ένα ερώτημα το οποίο μπορείτε να βρείτε χρήσιμες, αποθηκεύστε το κάνοντας κλικ στην επιλογή **"Αγαπημένα"**. Ένα αποθηκευμένο ερώτημα σάς επιτρέπει να εύκολα το χρησιμοποιήσετε ξανά αργότερα από τη σελίδα [Πίνακα εργαλείων μου](log-analytics-dashboards.md) όπου μπορείτε να δημιουργήσετε το δικό σας προσαρμοσμένο πίνακες εργαλείων.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Δημιουργία ειδοποιήσεων από ερωτήματα

Αφού έχετε δημιουργήσει τα ερωτήματά σας, μπορείτε να χρησιμοποιήσετε τα ερωτήματα για να σας ειδοποιεί όταν παρουσιάζονται συγκεκριμένα συμβάντα. Για πληροφορίες σχετικά με τη δημιουργία ειδοποιήσεων, ανατρέξτε στο θέμα [ειδοποιήσεις στο αρχείο καταγραφής ανάλυσης](log-analytics-alerts.md) . Για παραδείγματα ειδοποίησης ερωτήματα και άλλα παραδείγματα ερωτημάτων, ανατρέξτε στην καταχώρηση ιστολογίου [VMware οθόνη με ανάλυση καταγραφής OMS](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) .

## <a name="frequently-asked-questions"></a>Συνήθεις ερωτήσεις

### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Τι πρέπει να στην το ESXi να φιλοξενήσετε τη ρύθμιση; Ποιον αντίκτυπο θα έχει στο τρέχον περιβάλλον μου;
Η λύση χρησιμοποιεί τα εγγενή Syslog Host ESXi προώθησης μηχανισμό. Δεν χρειάζεται οποιοδήποτε πρόσθετο λογισμικό Microsoft στον κεντρικό υπολογιστή ESXi για να καταγράψετε τα αρχεία καταγραφής. Θα πρέπει να έχει μια χαμηλή επίδραση στο υπάρχον περιβάλλον του. Ωστόσο, πρέπει να ρυθμίσετε την προώθηση syslog, δηλαδή ESXI λειτουργικότητα.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Πρέπει να επανεκκινήσετε το host ESXi;
Όχι. Αυτή η διαδικασία δεν απαιτεί επανεκκίνηση. Ορισμένες φορές, vSphere δεν ενημερώνει σωστά το syslog. Σε αυτήν την περίπτωση, συνδεθείτε στον κεντρικό υπολογιστή ESXi και επανάληψη φόρτωσης του syslog. Ξανά, δεν χρειάζεται να επανεκκινήσετε τον κεντρικό υπολογιστή, ώστε αυτή η διαδικασία δεν ενδέχεται να το περιβάλλον σας διακόψουν.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Να μπορούν να αυξήσετε ή να μειώσετε την ένταση ήχου αποστέλλονται OMS δεδομένα του αρχείου καταγραφής;
Ναι, μπορείτε να. Μπορείτε να χρησιμοποιήσετε τις ρυθμίσεις επιπέδου καταγραφής Host ESXi στο vSphere. Συλλογή αρχείων καταγραφής είναι με βάση το επίπεδο *πληροφοριών* . Έτσι, εάν θέλετε να ελέγξετε Εικονική δημιουργίας ή διαγραφής, πρέπει να διατηρήσετε το επίπεδο *πληροφοριών* στην Hostd. Για περισσότερες πληροφορίες, ανατρέξτε στο άρθρο της [Γνωσιακής βάσης VMware](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Γιατί είναι Hostd δεν παρέχει δεδομένα σε OMS; Η ρύθμιση καταγραφής μου έχει οριστεί σε πληροφορίες.
Παρουσιάστηκε ένα σφάλμα ESXi κεντρικού υπολογιστή για τη χρονική σήμανση syslog. Για περισσότερες πληροφορίες, ανατρέξτε στο άρθρο της [Γνωσιακής βάσης VMware](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Αφού εφαρμόσετε τη λύση, Hostd θα πρέπει να λειτουργεί κανονικά.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Μπορώ να έχω πολλές υπηρεσίες παροχής φιλοξενίας ESXi προώθηση syslog δεδομένων σε ένα μεμονωμένο Εικονική με omsagent;
Ναι. Μπορείτε να έχετε πολλούς κεντρικούς υπολογιστές ESXi προώθηση σε ένα μεμονωμένο Εικονική με omsagent.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Γιατί δεν βλέπω ροή σε OMS δεδομένων;

Μπορεί να υπάρχουν πολλοί λόγοι:

- Ο κεντρικός υπολογιστής ESXi δεν προώθηση σωστά δεδομένα σε η Εικονική εκτελεί omsagent. Για να ελέγξετε, εκτελέστε τα ακόλουθα βήματα:
    1. Για να επιβεβαιώσετε, συνδεθείτε με τον κεντρικό υπολογιστή ESXi χρησιμοποιώντας ssh και εκτελέστε την ακόλουθη εντολή:`nc -z ipaddressofVM 1514`

        Εάν αυτό δεν είναι επιτυχής, είναι πιθανό vSphere ρυθμίσεις στη ρύθμιση παραμέτρων για προχωρημένους δεν διορθώνει. Για πληροφορίες σχετικά με το πώς μπορείτε να ρυθμίσετε τον κεντρικό υπολογιστή ESXi για την προώθηση syslog, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων syslog συλλογής](#configure-syslog-collection) .

    2. Εάν συνδεσιμότητας θύρα syslog είναι επιτυχής, αλλά εξακολουθείτε να βλέπετε δεδομένα, στη συνέχεια, φορτώσετε ξανά το syslog στον κεντρικό υπολογιστή ESXi χρησιμοποιώντας ssh για να εκτελέσετε την ακόλουθη εντολή:` esxcli system syslog reload`

- Η Εικονική με τον παράγοντα OMS δεν έχει ρυθμιστεί σωστά. Για να δοκιμάσετε αυτό, ακολουθήστε τα παρακάτω βήματα:
    1. OMS αναμένει στη θύρα 1514 και προωθεί δεδομένα σε OMS. Για να βεβαιωθείτε ότι είναι ανοιχτό, εκτελέστε την ακόλουθη εντολή:`netstat -a | grep 1514`
    2. Θα πρέπει να βλέπετε θύρα `1514/tcp` Άνοιγμα. Εάν δεν το κάνετε, βεβαιωθείτε ότι το omsagent έχει εγκατασταθεί σωστά. Εάν δεν βλέπετε τις πληροφορίες θύρας, στη συνέχεια, τη θύρα syslog δεν είναι ανοιχτό στο η Εικονική.
        1. Βεβαιωθείτε ότι ο παράγοντας OMS εκτελείται με τη χρήση `ps -ef | grep oms`. Εάν δεν εκτελείται, ξεκινήστε τη διαδικασία, εκτελέστε την εντολή` sudo /opt/microsoft/omsagent/bin/service_control start`
        2. Άνοιγμα του `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` αρχείου.

            Βεβαιωθείτε ότι η σωστή χρήστη και η ρύθμιση ομάδας είναι έγκυρο, παρόμοια με:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

            Εάν δεν υπάρχει το αρχείο ή το χρήστη και η ρύθμιση ομάδας είναι λάθος, λαμβάνουν διορθωτικές ενέργειες από [ένα διακομιστή Linux προετοιμασία](#prepare-a-linux-server).

## <a name="next-steps"></a>Επόμενα βήματα

- Χρησιμοποιήστε [Αναζητήσεις καταγραφής](log-analytics-log-searches.md) στο αρχείο καταγραφής ανάλυσης για να δείτε λεπτομερή δεδομένα host VMware.
- [Δημιουργήστε το δικό σας πίνακες εργαλείων](log-analytics-dashboards.md) που εμφανίζει δεδομένα VMware κεντρικού υπολογιστή.
- [Δημιουργία ειδοποιήσεων](log-analytics-alerts.md) όταν παρουσιάζονται συγκεκριμένα συμβάντα host VMware.
