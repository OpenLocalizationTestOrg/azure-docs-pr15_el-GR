<properties
    pageTitle="Θεωρείται κατανομή Linux | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με Linux στην θεωρηθεί Azure κατανομές, συμπεριλαμβανομένων των οδηγιών για Ubuntu, OpenLogic, Oracle και SUSE."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="szark"/>



#<a name="linux-on-azure-endorsed-distributions"></a>Linux στην κατανομές θεωρηθεί Azure

> [AZURE.NOTE] Εάν έχετε λίγα λεπτά, Βοηθήστε μας να βελτιώσουμε την τεκμηρίωση Εικονική Linux Azure παρακολουθώντας αυτό [γρήγορης έρευνας](https://aka.ms/linuxdocsurvey) με την εμπειρία σας. Κάθε απάντηση βοηθούν στη Βοήθεια για να εργαστείτε.

Οι εικόνες Linux από τη συλλογή Azure ή Marketplace παρέχονται από έναν αριθμό από τους συνεργάτες και εργαζόμαστε με διάφορες Κοινότητες Linux για να προσθέσετε ακόμα περισσότερες μπορεί να είναι στη λίστα διανομής θεωρηθεί. Στο μεταξύ, για διανομή δεν είναι διαθέσιμη από τη συλλογή μπορείτε πάντα να μεταφορά-σας-κάτοχος-Linux, ακολουθώντας τις οδηγίες σε [αυτήν τη σελίδα](virtual-machines-linux-classic-create-upload-vhd.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="supported-distributions--versions"></a>Υποστηριζόμενες κατανομές και εκδόσεις ##

Ο παρακάτω πίνακας παραθέτει τις κατανομές Linux και τις εκδόσεις που υποστηρίζονται σε Azure. Ανατρέξτε επίσης παροχής για την [υποστήριξη για τις εικόνες Linux Microsoft Azure](https://support.microsoft.com/en-us/kb/2941892) για πιο λεπτομερείς πληροφορίες.

Τα προγράμματα οδήγησης υπηρεσίες ενοποίησης Linux (LIS) για το Hyper-V και Azure είναι λειτουργικές μονάδες πυρήνα συνεισφοράς Microsoft απευθείας με την επόμενη πυρήνα Linux.  Τα προγράμματα οδήγησης LIS είτε είναι ενσωματωμένες στο την κατανομή πυρήνα από προεπιλογή, ή για παλαιότερες RHEL CentOS με/κατανομές είναι διαθέσιμα ως ξεχωριστό στοιχείο λήψης [εδώ](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409).  Ανατρέξτε [σε αυτό το άρθρο](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements) για περισσότερες πληροφορίες σχετικά με τα προγράμματα οδήγησης LIS.

Ο παράγοντας Linux Azure είναι ήδη προ-εγκατεστημένο από τη συλλογή Azure τις εικόνες και είναι συνήθως διαθέσιμες από την κατανομή πακέτου αποθετήριο.  Πηγαίος κώδικας μπορεί να βρεθεί στην [GitHub](https://github.com/azure/walinuxagent).

Κατανομή|Έκδοση|Προγράμματα οδήγησης|Παράγοντας
---|---|---|---
CentOS με OpenLogic | CentOS 6.3 +, 7.0 + | CentOS 6.3: [λήψη LIS](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: Στον πυρήνα | Πακέτο: Στο [OpenLogic repo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) στην περιοχή "WALinuxAgent" <br/>Πηγαίος κώδικας: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) | 494.4.0+ | Στον πυρήνα | Πηγαίος κώδικας: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian | Debian 7.9 +, 8.2 + | Στον πυρήνα | Πακέτο: στο repo στην περιοχή "waagent" <br/>Πηγαίος κώδικας: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle Linux | 6.4 +, 7.0 + | Στον πυρήνα | Πακέτο: στο repo στην περιοχή "WALinuxAgent" <br/>Πηγαίος κώδικας: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Κόκκινο καπέλο Linux για μεγάλες επιχειρήσεις | RHEL 6,7 + 7.1 + | Στον πυρήνα|Πακέτο: στο repo στην περιοχή "WALinuxAgent" <br/>Πηγαίος κώδικας: [GitHub](https://github.com/Azure/WALinuxAgent)
Για μεγάλες επιχειρήσεις Linux SUSE | SLES 11 SP4, SLES 12 SP1 + και <p> SLES για SAP 11 SP3 + | Στον πυρήνα | Πακέτο: Στο [Cloud: Εργαλεία](https://build.opensuse.org/project/show/Cloud:Tools) repo στην περιοχή "παράγοντα-azure-python" <br/>Πηγαίος κώδικας: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE | openSUSE 13.2 πρέπει + | Στον πυρήνα | Πακέτο: Στο [Cloud: Εργαλεία](https://build.opensuse.org/project/show/Cloud:Tools) repo στην περιοχή "παράγοντα-azure-python" <br/>Πηγαίος κώδικας: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04, 14.04, 16.04, 16.10 | Στον πυρήνα | Πακέτο: στο repo στην περιοχή "walinuxagent" <br/>Πηγαίος κώδικας: [GitHub](https://github.com/Azure/WALinuxAgent)


## <a name="partners"></a>Συνεργάτες

### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/Azure](http://www.openlogic.com/azure)

OpenLogic είναι μια υπηρεσία παροχής στην αρχή των λύσεων Άνοιγμα αρχείου προέλευσης για μεγάλες επιχειρήσεις για το cloud και του κέντρου δεδομένων. OpenLogic βοηθά εκατοντάδες αποτέλεσμα για μεγάλες επιχειρήσεις σε ένα ευρύ φάσμα κλάδοι ασφαλής απόκτησης, υποστηρίζει και έλεγχος λογισμικού Άνοιγμα αρχείου προέλευσης. OpenLogic παρέχει τεχνική υποστήριξη εμπορική-βαθμολογίας και αποζημίωση για 600 Άνοιγμα αρχείου προέλευσης πακέτα αντίγραφα από την Κοινότητα ειδικός OpenLogic, συμπεριλαμβανομένης της υποστήριξης επιπέδου για μεγάλες επιχειρήσεις για CentOS, καθώς και να γίνεται εκκίνηση συνεργάτη για την παροχή εικόνες που βασίζονται σε CentOS σε Azure.

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/Running-coreos/cloud-providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Από την τοποθεσία Web CoreOS:

*CoreOS έχει σχεδιαστεί για την ασφάλεια, συνέπειας και την αξιοπιστία. Αντί για την εγκατάσταση πακέτων μέσω yum ή κατάλληλη, CoreOS χρησιμοποιεί Linux κοντέινερ για να διαχειριστείτε τις υπηρεσίες σας σε υψηλότερο επίπεδο της αφαίρεσης. Μια μεμονωμένη υπηρεσία κώδικα και όλων των εξαρτήσεων είναι συσκευαστούν μέσα σε ένα κοντέινερ που μπορεί να εκτελεστεί σε μία ή πολλές μηχανές CoreOS.*


### <a name="credativ"></a>Credativ
[http://www.credativ.CO.UK/credativ-blog/debian-images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ είναι μια ανεξάρτητη συμβουλευτικές και εταιρεία παροχής υπηρεσιών, ειδικούς την ανάπτυξη και εφαρμογή επαγγελματική λύσεις με τη χρήση του δωρεάν λογισμικού. Ως αρχικό ειδικών Άνοιγμα αρχείου προέλευσης, Credative περιλαμβάνει διεθνείς αναγνώριση με πολλά τμήματα IT με την υποστήριξη. Σε συνδυασμό με τη Microsoft, Credativ προς το παρόν προετοιμασία αντίστοιχο Debian εικόνων για Debian 8 (Jessie) και Debian πριν από 7 (Wheezy), τα οποία έχουν σχεδιαστεί ειδικά για να εκτελέσετε στο Azure και μπορούν να διαχειριστούν εύκολα μέσω της πλατφόρμας. Credativ υποστηρίζουν επίσης τη διατήρηση μακροπρόθεσμη και την ενημέρωση των Debian εικόνων για Azure έως τα κέντρα υποστήριξης Άνοιγμα προέλευσης.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/Topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Στρατηγική της Oracle είναι να προσφέρουν μια μεγάλη γκάμα λύσεων για σύννεφων δημόσια και ιδιωτικά, κατά τη διάρκεια πελάτες επιλογή και ευελιξία πώς ανάπτυξη του λογισμικού Oracle στο Oracle σύννεφων, καθώς και άλλες σύννεφων.  Συνεργασίας της Oracle με τη Microsoft επιτρέπει στους πελάτες ανάπτυξης λογισμικού Oracle στο Microsoft δημόσια και ιδιωτικά σύννεφων την αξιόπιστη πιστοποίησης και υποστήριξης από την Oracle.  Της Oracle δέσμευσης και επένδυσης σε λύσεις δημόσια και ιδιωτικά cloud Oracle έχει αλλάξει.

### <a name="red-hat"></a>Κόκκινο καπέλο
[http://www.redhat.com/en/Partners/strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Στον κόσμο αρχικών παροχής λύσεων Άνοιγμα αρχείου προέλευσης, κόκκινο καπέλο βοηθά περισσότερες από 90% των εταιρείες του Fortune 500 επίλυση επιχειρήσεις προκλήσεις, στοίχιση τους IT και επιχειρήσεις στρατηγικές, και να προετοιμαστείτε για το μέλλον της τεχνολογίας. Κόκκινο καπέλο το κάνει αυτό, παρέχοντας ασφαλούς λύσεων σε ένα μοντέλο Άνοιγμα επαγγελματικών και ένα μοντέλο οικονομική, προβλέψιμα συνδρομή.

### <a name="suse"></a>SUSE
[http://www.SUSE.com/SUSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server στην Azure είναι μια αποδεδειγμένη πλατφόρμα που παρέχει προϊσταμένου αξιοπιστία και την ασφάλεια για υπολογιστικό νέφος. Ευέλικτη πλατφόρμα Linux του SUSE Ενοποιείται άψογα με τις υπηρεσίες Azure cloud για να παρέχουν ένα περιβάλλον εύκολα διαχειρίσιμα cloud. Και με περισσότερες από 9,200 πιστοποιημένου εφαρμογές από πάνω από 1 800 ανεξάρτητους προμηθευτές λογισμικού για SUSE Linux Enterprise Server, SUSE εξασφαλίζει ότι εκτελείται υποστηρίζεται στο κέντρο δεδομένων φόρτους εργασίας μπορούν να αναπτυχθούν να στην Azure.

### <a name="canonical"></a>Κανονική
[http://www.ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Κανονικής μηχανικής και διακυβέρνηση ανοικτή Κοινότητα μονάδα επιτυχίας του Ubuntu στο πρόγραμμα-πελάτη, διακομιστή και το σύννεφο υπολογιστική, συμπεριλαμβανομένων των προσωπικών cloud services για τους καταναλωτές. Της κανονικής οπτικό μια δωρεάν πλατφόρμα στο Ubuntu, μέσω τηλεφώνου με το cloud, με μια οικογένεια συνεκτική περιβάλλοντα εργασίας για το τηλέφωνο, tablet, Τηλεοπτική και επιφάνειας εργασίας, κάνει Ubuntu την πρώτη επιλογή διάφορων τύπων ιδρύματα από υπηρεσίες παροχής δημόσια cloud για να το κατασκευαστών ηλεκτρονικές και Αγαπημένα μεταξύ των μεμονωμένων technologists.

Με τους προγραμματιστές και μηχανικής κέντρα όλο τον κόσμο, Canonical με μοναδικό τρόπο τοποθέτησης στο συνεργάτη με κατασκευαστές υλικού, υπηρεσίες παροχής περιεχομένου και προγραμματιστές λογισμικού για να μεταφέρετε Ubuntu λύσεων για να προωθήσετε, από υπολογιστές σε διακομιστές και φορητές συσκευές.

