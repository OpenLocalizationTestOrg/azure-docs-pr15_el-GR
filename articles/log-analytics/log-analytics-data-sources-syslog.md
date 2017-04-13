<properties 
   pageTitle="SYSLOG μηνυμάτων στο αρχείο καταγραφής ανάλυσης | Microsoft Azure"
   description="SYSLOG είναι ένα πρωτόκολλο καταγραφής συμβάντων που είναι κοινά και για Linux.   Σε αυτό το άρθρο περιγράφει τον τρόπο ρύθμισης των παραμέτρων συλλογή Syslog μηνυμάτων στο αρχείο καταγραφής ανάλυσης και λεπτομέρειες από τις εγγραφές που δημιουργούν στο αποθετήριο OMS."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />


# <a name="syslog-data-sources-in-log-analytics"></a>Προελεύσεις δεδομένων SYSLOG στο αρχείο καταγραφής ανάλυσης

SYSLOG είναι ένα πρωτόκολλο καταγραφής συμβάντων που είναι κοινά και για Linux.  Θα σας στέλνει μηνύματα που μπορεί να είναι αποθηκευμένο στον τοπικό υπολογιστή ή να παραδοθεί σε ένα συλλογής Syslog εφαρμογές.  Όταν είναι εγκατεστημένες τον παράγοντα OMS για Linux, ρυθμίζει την τοπική μονού Syslog για την προώθηση μηνυμάτων στον παράγοντα.  Ο παράγοντας, στη συνέχεια, στέλνει το μήνυμα σε ανάλυση καταγραφής όπου έχει δημιουργηθεί μια αντίστοιχη εγγραφή στο αποθετήριο OMS.  

> [AZURE.NOTE]Ανάλυση καταγραφής υποστηρίζει τη συλλογή των μηνυμάτων που αποστέλλονται από rsyslog ή syslog ση. Η προεπιλεγμένη μονού syslog στην έκδοση 5 Linux Enterprise καπέλο κόκκινο, CentOS και Oracle Linux έκδοσης (sysklog) δεν υποστηρίζεται για τη συλλογή συμβάντων syslog. Για τη συλλογή δεδομένων syslog από αυτήν την έκδοση του αυτές τις κατανομές, θα πρέπει να είναι εγκατεστημένο και να έχει ρυθμιστεί ώστε να αντικαταστήσετε sysklog το [rsyslog μονού](http://rsyslog.com) .

![Συλλογή SYSLOG](media/log-analytics-data-sources-syslog/overview.png)


## <a name="configuring-syslog"></a>Ρύθμιση παραμέτρων Syslog
Ο παράγοντας OMS για Linux μόνο θα συλλέξει συμβάντων με τις εγκαταστάσεις και severities που έχουν καθοριστεί με τη ρύθμιση παραμέτρων.  Μπορείτε να ρυθμίσετε Syslog μέσω της πύλης OMS ή με τη διαχείριση των αρχείων ρύθμισης παραμέτρων σχετικά με τις παράγοντες Linux.


### <a name="configure-syslog-in-the-oms-portal"></a>Ρύθμιση παραμέτρων Syslog στην πύλη του OMS

Ρύθμιση παραμέτρων Syslog από το [μενού "δεδομένα" στις ρυθμίσεις του αρχείου καταγραφής ανάλυσης](log-analytics-data-sources.md#configuring-data-sources).  Αυτή η ρύθμιση παραμέτρων παραδίδεται στο αρχείο ρύθμισης παραμέτρων σε κάθε παράγοντα Linux.

Μπορείτε να προσθέσετε μια νέα δυνατότητα, πληκτρολογώντας στο όνομά του και κάνοντας κλικ στην επιλογή **+**.  Για κάθε δυνατότητα, θα συλλεχθεί μόνο τα μηνύματα με το επιλεγμένο severities.  Ελέγξτε το severities για τη συγκεκριμένη εγκατάσταση που θέλετε να συλλέξετε.  Δεν μπορείτε να δώσετε επιπλέον κριτήρια για να φιλτράρει μηνύματα.

![Ρύθμιση παραμέτρων Syslog](media/log-analytics-data-sources-syslog/configure.png)


Από προεπιλογή, όλες οι αλλαγές ρύθμισης παραμέτρων προωθούνται αυτόματα σε όλα παράγοντες.  Εάν θέλετε να ρυθμίσετε τις παραμέτρους Syslog με μη αυτόματο τρόπο σε κάθε παράγοντα Linux, στη συνέχεια, καταργήστε την επιλογή από το πλαίσιο *εφαρμογή κάτω από τη ρύθμιση παραμέτρων για να μου μηχανές Linux*.


### <a name="configure-syslog-on-linux-agent"></a>Ρύθμιση παραμέτρων Syslog στο Linux παράγοντα

Όταν το που [παράγοντας OMS έχει εγκατασταθεί σε έναν υπολογιστή-πελάτη Linux](log-analytics-linux-agents.md), εγκαθίσταται ένα προεπιλεγμένο αρχείο ρύθμισης παραμέτρων syslog που καθορίζει την εγκατάσταση και της σοβαρότητας των μηνυμάτων που έχουν συλλεχθεί.  Μπορείτε να τροποποιήσετε αυτό το αρχείο για να αλλάξετε τη ρύθμιση παραμέτρων.  Το αρχείο ρύθμισης παραμέτρων είναι διαφορετικά, ανάλογα με το μονού Syslog που έχει εγκατασταθεί το πρόγραμμα-πελάτη.

> [AZURE.NOTE] Εάν επεξεργαστείτε τη ρύθμιση παραμέτρων syslog, πρέπει να επανεκκινήσετε το μονού syslog για να τεθούν σε ισχύ οι αλλαγές.

#### <a name="rsyslog"></a>rsyslog

Το αρχείο ρύθμισης παραμέτρων για rsyslog βρίσκεται στο **/etc/rsyslog.d/95-omsagent.conf**.  Τα προεπιλεγμένα περιεχόμενα εμφανίζονται παρακάτω.  Αυτό συλλέγει syslog μηνύματα που στέλνονται από τον τοπικό παράγοντα για όλες τις εγκαταστάσεις με ένα επίπεδο προειδοποίησης ή νεότερη έκδοση.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Μπορείτε να καταργήσετε μια εγκατάσταση, καταργώντας την ενότητα από το αρχείο ρύθμισης παραμέτρων.  Μπορείτε να περιορίσετε την severities που συλλέγονται για μια συγκεκριμένη δυνατότητα τροποποιώντας καταχώρησης της εγκατάστασης.  Για παράδειγμα, για να περιορίσετε τη δυνατότητα χρηστών σε μηνύματα με μια της σοβαρότητας του σφάλματος ή νεότερη έκδοση μπορείτε να τροποποιήσετε αυτήν τη γραμμή από το αρχείο ρύθμισης παραμέτρων για τα εξής:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>ση SYSLOG

Το αρχείο ρύθμισης παραμέτρων για rsyslog είναι θέση στο **/etc/syslog-ng/syslog-ng.conf**.  Τα προεπιλεγμένα περιεχόμενα εμφανίζονται παρακάτω.  Αυτό συλλέγει syslog μηνύματα που στέλνονται από τον τοπικό παράγοντα για όλες τις εγκαταστάσεις και severities όλων.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };
    
    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };
    
    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };
    
    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };
    
    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };
    
    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };
    
    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Μπορείτε να καταργήσετε μια εγκατάσταση, καταργώντας την ενότητα από το αρχείο ρύθμισης παραμέτρων.  Μπορείτε να περιορίσετε την severities που έχουν συλλεχθεί για μια συγκεκριμένη δυνατότητα καταργώντας τους από τη λίστα.  Για παράδειγμα, για να περιορίσετε τη δυνατότητα χρήστη να απλώς ειδοποίησης και κρίσιμες μηνύματα, θα μπορείτε να τροποποιήσετε αυτό το τμήμα του αρχείου ρύθμισης παραμέτρων για τα εξής:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>Αλλαγή της θύρας Syslog

Ο παράγοντας OMS αναμένει Syslog μηνύματα στον τοπικό υπολογιστή πελάτη στη θύρα 25224.  Μπορείτε να αλλάξετε αυτήν τη θύρα, προσθέτοντας την παρακάτω ενότητα στο αρχείο ρύθμισης παραμέτρων παράγοντας OMS βρίσκεται στο **/etc/opt/microsoft/omsagent/conf/omsagent.conf**.  Αντικαταστήστε 25224 στην εγγραφή **θύρα** με τον αριθμό θύρας που θέλετε.  Σημειώστε ότι θα πρέπει επίσης να τροποποιήσετε το αρχείο ρύθμισης παραμέτρων για το μονού Syslog για την αποστολή μηνυμάτων σε αυτήν τη θύρα.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Συλλογή δεδομένων

Ο παράγοντας OMS αναμένει Syslog μηνύματα στον τοπικό υπολογιστή πελάτη στη θύρα 25224. Το αρχείο ρύθμισης παραμέτρων για το μονού Syslog προωθεί Syslog τα μηνύματα που αποστέλλονται από την εφαρμογή σε αυτήν τη θύρα όπου συγκεντρώσει από αρχείο καταγραφής ανάλυσης.


## <a name="syslog-record-properties"></a>Ιδιότητες εγγραφής SYSLOG

SYSLOG εγγραφές έχουν ένα τύπο **Syslog** και τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| Υπολογιστή | Υπολογιστής που έχουν συλλεχθεί το συμβάν από. |
| Εγκατάσταση | Καθορίζει το τμήμα του συστήματος που δημιούργησε το μήνυμα. |
| HostIP | Διεύθυνση IP του συστήματος η αποστολή του μηνύματος.  |
| Όνομα κεντρικού υπολογιστή | Το όνομα του συστήματος η αποστολή του μηνύματος. |
| SeverityLevel | Επίπεδο σοβαρότητας του συμβάντος. |
| SyslogMessage | Κείμενο του μηνύματος. |
| Αναγνωριστικό διεργασίας | Το Αναγνωριστικό της διαδικασίας που δημιούργησε το μήνυμα. |
| EventTime | Ημερομηνία και ώρα που δημιουργήθηκε το συμβάν.



## <a name="log-queries-with-syslog-records"></a>Αρχείο καταγραφής ερωτημάτων με τις εγγραφές Syslog

Ο παρακάτω πίνακας παρέχει παραδείγματα διαφορετικό αρχείο καταγραφής ερωτημάτων που ανακτούν Syslog εγγραφές.

| Ερώτημα | Περιγραφή |
|:--|:--|
| Τύπος = Syslog | Όλα Syslogs. |
| Τύπος = Syslog SeverityLevel = σφάλματος | Όλες τις εγγραφές Syslog με της σοβαρότητας του σφάλματος. |
| Τύπος = Syslog & #124- μέτρηση count() από τον υπολογιστή | Καταμέτρηση των Syslog εγγραφές από τον υπολογιστή. |
| Τύπος = Syslog & #124- μέτρηση count() εγκατάσταση | Καταμέτρηση των Syslog εγγραφών ανά εγκατάστασης. |

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε σχετικά με το [αρχείο καταγραφής αναζητήσεις](log-analytics-log-searches.md) για να αναλύσετε τα δεδομένα που συλλέγονται από προελεύσεις δεδομένων και τις λύσεις. 
- Χρήση [Προσαρμοσμένων πεδίων](log-analytics-custom-fields.md) ανάλυσης δεδομένων από τις εγγραφές syslog σε μεμονωμένα πεδία.
- [Ρύθμιση παραμέτρων Linux παράγοντες](log-analytics-linux-agents.md) για τη συλλογή άλλους τύπους δεδομένων. 
