<properties
    pageTitle="Linux και Άνοιγμα-προέλευσης υπολογιστική στην Azure | Microsoft Azure"
    description="Παραθέτει σε λίστα Linux και Άνοιγμα προέλευσης υπολογιστική άρθρα στο Azure, συμπεριλαμβανομένων βασική χρήση Linux, ορισμένες βασικές έννοιες σχετικά με την εκτέλεση ή την αποστολή εικόνων Linux στην Azure και άλλο περιεχόμενο σχετικά με τις συγκεκριμένες τεχνολογίες και βελτιστοποιήσεις."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="06/27/2016"
    ms.author="rasquill"/>



# <a name="linux-and-open-source-computing-on-azure"></a>Linux και Άνοιγμα-προέλευσης υπολογιστική στο Azure

Βρείτε όλη την τεκμηρίωση που πρέπει να δημιουργήσετε και να διαχειριστείτε βάσει Linux εικονικές μηχανές του μοντέλου κλασική ανάπτυξης.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>Γρήγορα αποτελέσματα
- [Εισαγωγή για Linux στο Azure](virtual-machines-linux-intro-on-azure.md)
- [Συνήθεις ερώτηση σχετικά με τις εικονικές μηχανές Windows Azure που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης](virtual-machines-linux-classic-faq.md)
- [Πληροφορίες για τις εικόνες για εικονικές μηχανές](virtual-machines-linux-classic-about-images.md)
- [Αποστολή τη δική σας εικόνα Distro](virtual-machines-linux-classic-create-upload-vhd.md) (και επίσης οδηγίες χρησιμοποιώντας μια [Κατανομή Azure-Endorsed](virtual-machines-linux-endorsed-distros.md))
- [Συνδεθείτε με μια Εικονική Linux χρησιμοποιώντας την πύλη του Azure κλασική](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="set-up"></a>Ρύθμιση

- [Εγκατάσταση του Azure περιβάλλον γραμμής εντολών (Azure CLI)](../xplat-cli-install.md)


## <a name="tutorials"></a>Προγράμματα εκμάθησης

- [Εγκατάσταση ΛΆΜΠΑ στοίβα σε μια εικονική μηχανή Linux στο Azure](virtual-machines-linux-create-lamp-stack.md)
- [Κείμενο φωνητικής γραφής στην εφαρμογή Web ράβδους σε μια Εικονική Azure](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
- [Πώς μπορείτε να: εγκατάσταση Apache Qpid Proton-C για AMQP και Bus υπηρεσίας](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Βάσεις δεδομένων
- [Βελτιστοποίηση των επιδόσεων του MySQL στο Azure](virtual-machines-linux-classic-optimize-mysql.md)
- [MySQL συμπλεγμάτων](virtual-machines-linux-classic-mysql-cluster.md)
- [Εκτέλεση Cassandra με Linux σε Azure και την πρόσβαση από Node.js](virtual-machines-linux-classic-cassandra-nodejs.md)
- [Δημιουργήστε ένα σύμπλεγμα πολλαπλή MariaDbs](virtual-machines-linux-classic-mariadb-mysql-cluster.md)

### <a name="hpc"></a>HPC
- [Γρήγορα αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-linux-classic-hpcpack-cluster.md)
- [Εκτέλεση NAMD με το πακέτο του Microsoft HPC σε Linux υπολογισμού τους κόμβους Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
- [Ρύθμιση του ένα σύμπλεγμα Linux RDMA για να εκτελέσετε εφαρμογές MPI](virtual-machines-linux-classic-rdma-cluster.md)

### <a name="docker"></a>Docker
- [Χρησιμοποιώντας την επέκταση Εικονική Docker από το Azure γραμμής εντολών περιβάλλοντος εργασίας (Azure CLI)](virtual-machines-linux-classic-cli-use-docker.md)
- [Χρησιμοποιώντας την επέκταση Εικονική Docker από την πύλη του Azure](virtual-machines-linux-classic-portal-use-docker.md)
- [Πώς μπορείτε να χρησιμοποιήσετε docker υπολογιστή στο Azure](virtual-machines-linux-docker-machine.md)

### <a name="ubuntu"></a>Ubuntu
- [Πώς μπορείτε να: MySQL συμπλεγμάτων](virtual-machines-linux-classic-mysql-cluster.md)
- [Πώς μπορείτε να: Node.js και Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)

### <a name="opensuse"></a>OpenSUSE
- [Πώς μπορείτε να: εγκατάσταση και εκτέλεση MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)

### <a name="coreos"></a>CoreOS
- [Πώς μπορείτε να: χρήση CoreOS στο Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="planning"></a>Σχεδιασμός
- [Οδηγίες υλοποίησης υπηρεσίες υποδομής Azure](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md)
- [Επιλογή Linux τα ονόματα των χρηστών](virtual-machines-linux-usernames.md)
- [Τρόπος ρύθμισης των παραμέτρων διαθεσιμότητα για εικονικές μηχανές του μοντέλου κλασική ανάπτυξης](virtual-machines-linux-classic-configure-availability.md)
- [Πώς μπορείτε να προγραμματίσετε προγραμματισμένη συντήρηση στη ΣΠΣ Azure](virtual-machines-linux-planned-maintenance-schedule.md)
- [Διαχειριστείτε τη διαθεσιμότητα των εικονικές μηχανές](virtual-machines-linux-manage-availability.md)
- [Προγραμματισμένη συντήρηση για Linux εικονικές μηχανές στο Azure](virtual-machines-linux-planned-maintenance.md)


## <a name="deployment"></a>Ανάπτυξη
- [Δημιουργήστε μια προσαρμοσμένη εικονική μηχανή εκτελείται Linux](virtual-machines-linux-classic-createportal.md)
- [Τα βασικά στοιχεία: Καταγραφή μια Εικονική Linux για να δημιουργήσετε ένα πρότυπο](virtual-machines-linux-classic-capture-image.md)
- [Πληροφορίες για μη θεωρηθεί κατανομές](virtual-machines-linux-create-upload-generic.md)


## <a name="management"></a>Διαχείριση

- [SSH](virtual-machines-linux-mac-create-ssh-keys.md)
- [Πώς μπορείτε να επαναφέρετε έναν κωδικό πρόσβασης ή ιδιότητες SSH για Linux](virtual-machines-linux-classic-reset-access.md)
- [Χρήση ρίζας](virtual-machines-linux-use-root-privileges.md)


## <a name="azure-resources"></a>Azure πόροι

- [Ο παράγοντας Linux Azure](virtual-machines-linux-agent-user-guide.md)
- [Επεκτάσεις Azure Εικονική και δυνατότητες](virtual-machines-windows-extensions-features.md)
- [Έγχυση προσαρμοσμένα δεδομένα σε μια Εικονική για χρήση με την προετοιμασία Cloud](virtual-machines-windows-classic-inject-custom-data.md)


## <a name="storage"></a>Χώρος αποθήκευσης

- [Επισύναψη ένα δίσκο δεδομένων σε μια Εικονική Linux](virtual-machines-linux-classic-attach-disk.md)
- [Αποσύνδεση ένα δίσκο δεδομένων από μια Εικονική Linux](virtual-machines-linux-classic-detach-disk.md)
- [RAID](virtual-machines-linux-configure-raid.md)


## <a name="networking"></a>Δικτύωση
- [Πώς μπορείτε να ρυθμίσετε τα τελικά σημεία σε κλασική εικονικό μηχάνημα στο Azure](virtual-machines-linux-classic-setup-endpoints.md)


## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων
- [Αντιμετώπιση προβλημάτων συνδέσεων ασφαλούς κελύφους (SSH) σε ένα στοιχείο με βάση Linux Azure εικονικό μηχάνημα](virtual-machines-linux-troubleshoot-ssh-connection.md)
- [Αντιμετώπιση προβλημάτων κλασική ανάπτυξης με τη δημιουργία ενός νέου Linux εικονική μηχανή στο Azure](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md)  
- [Αντιμετώπιση προβλημάτων κλασική ανάπτυξης επανεκκίνηση ή την αλλαγή μεγέθους ενός υπάρχοντος Linux εικονική μηχανή στο Azure](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md) 


## <a name="reference"></a>Αναφορά

- [Azure CLI εντολές σε λειτουργία Azure Service Management (asm)](../virtual-machines-command-line-tools.md)
- [Διαχείριση υπηρεσίας Azure REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx)




## <a name="general-links"></a>Συνδέσεις Γενικά
Οι ακόλουθες συνδέσεις είναι για ιστολόγια Microsoft, Technet σελίδες, και εξωτερικές τοποθεσίες και όχι τεκμηρίωση Azure.com όπως παραπάνω. Καθώς Azure και την κόσμο των υπολογιστών Άνοιγμα προέλευσης είναι γρήγορη μετακίνηση των προορισμών, είναι σχεδόν βέβαιοι ότι οι ακόλουθες συνδέσεις προέρχονται από την ημερομηνία, *παρά* το γεγονός ότι πρέπει κάνουμε τις καλύτερες ευχές μας για να προσθέσετε νεότερη θέματα και να καταργήσετε αυτά τα παλιά συνεχώς. Εάν θα σας έχετε παραβλέψει μία, πληκτρολογήστε Επιτρέψτε μας γνωρίζετε στα σχόλια ή υποβάλετε μια αίτηση ελκυστική για να μας [GitHub repo](https://github.com/Azure/azure-content/).

- [Εκτέλεση ASP.NET 5 σε Linux χρησιμοποιώντας κοντέινερ Docker](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
- [Πώς να αναπτύξετε μια εικόνα Εικονική CentOS από OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
- [SUSE Update υποδομή](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
- [SUSE Linux Enterprise Server για τη βιβλιοθήκη συσκευής Cloud SAP](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
- [Δημιουργία Linux ιδιαίτερα διαθέσιμη σε Azure στα βήματα 12](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
- [Αυτοματοποίηση προμήθεια Linux στην Azure με Azure CLI, node.js, jhawk](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
- [GlusterFS στο Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
- [Εκτέλεση FreeBSD στο Azure](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
- [Εύκολη ανάπτυξη FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
- [Για την ανάπτυξη ενός προσαρμοσμένου ειδώλου FreeBSD](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
- [Για διακομιστή αρχείων Linux Kaspersky AV](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL

- [8 βάσεις δεδομένων NoSql Άνοιγμα προέλευσης για Azure](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
- [Slideshare (MSOpenTech): Εμπειρίες με CouchDb στο Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
- [Εκτελεί CouchDB-ως-a-υπηρεσία με node.js, CORS και Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)

- [Redis των Windows στην υπηρεσία Cache Azure Redis](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
- [Ανακοίνωση περιόδου λειτουργίας ASP.NET παροχής για την έκδοση Preview Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)

- [Ιστολόγιο: RavenHQ τώρα διαθέσιμη από το Azure Marketplace](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Μεγάλο δεδομένων
- [Εγκατάσταση Hadoop σε ΣΠΣ Azure Linux](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
- [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Σχεσιακή βάση δεδομένων
- [Αρχιτεκτονική διαθεσιμότητα υψηλής MySQL στο Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
- [Κατά την εγκατάσταση Postgres με corosync, pg_bouncer χρησιμοποιώντας ILB](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>(HPC) υπολογιστική υψηλών επιδόσεων Linux

- [Πρότυπο γρήγορη έναρξη: περιστροφή ένα σύμπλεγμα SLURM](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (και [καταχώρηση ιστολογίου](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
- [Γρήγορη έναρξη πρότυπο: Δημιουργήστε ένα σύμπλεγμα HPC με κόμβους υπολογιστικών Linux](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>Devops, διαχείριση και βελτιστοποίηση

Ως κόσμο των devops, διαχείριση και βελτιστοποίηση είναι αρκετά εκτεταμένης και αλλαγή πολύ γρήγορα, πρέπει να εξετάσετε τη λίστα κάτω από το σημείο εκκίνησης.

- [Βίντεο: Azure εικονικές μηχανές: χρήση Chef, Puppet και Docker για τη Διαχείριση ΣΠΣ Linux](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)

- [Ολοκλήρωση οδηγού για την αυτοματοποιημένη ανάπτυξη σύμπλεγμα Kubernetes με CoreOS και πλέξη](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
- [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

- [Εξαρτώμενη Jenkins προσθήκης για Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
- [GitHub repo: Jenkins αποθήκευσης προσθήκης για Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)

- [Τρίτων: Εξαρτώμενη Hudson Προσθήκη για Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
- [Τρίτων: Χώρος αποθήκευσης Hudson Προσθήκη για Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

- [Βίντεο: Τι είναι Chef και πώς λειτουργεί αυτό;](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

- [Βίντεο: Πώς να χρησιμοποιήσετε Azure αυτοματισμό με ΣΠΣ Linux](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

- [Ιστολόγιο: Πώς να κάνετε Powershell DSC για Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
- [GitHub: Πρόγραμμα-πελάτη Docker DSC](https://github.com/anweiss/DockerClientDSC)

- [Προσθήκη συσκευαστή για Azure](https://github.com/msopentech/packer-azure)
