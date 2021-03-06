<properties
pageTitle="Μετεγκατάσταση από το HDInsight που βασίζεται σε Windows με το HDInsight βάσει Linux | Azure"
description="Μάθετε πώς μπορείτε να κάνετε μετεγκατάσταση από ένα σύμπλεγμα HDInsight που βασίζεται σε Windows σε ένα σύμπλεγμα βάσει Linux HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Μετεγκατάσταση από ένα σύμπλεγμα HDInsight που βασίζεται σε Windows σε ένα σύμπλεγμα βάσει Linux

Ενώ HDInsight που βασίζεται σε Windows παρέχει έναν εύκολο τρόπο για να χρησιμοποιήσετε Hadoop στο cloud, ενδέχεται να ανακαλύψετε ότι χρειάζεστε ένα σύμπλεγμα βάσει Linux για να επωφεληθείτε από εργαλεία και τις τεχνολογίες που απαιτούνται για τη λύση. Πολλά πράγματα στο στο περιβάλλον εμπορικής προσαρμογής Hadoop έχουν αναπτυχθεί σε συστήματα που βασίζονται σε Linux και ορισμένα ενδέχεται να μην είναι διαθέσιμη για χρήση με HDInsight που βασίζεται στα Windows. Επιπλέον, πολλά βιβλία, βίντεο και άλλα εκπαιδευτικό υλικό λαμβάνεται ως δεδομένο ότι χρησιμοποιείτε ένα σύστημα Linux όταν εργάζεστε με Hadoop.

Αυτό το έγγραφο παρέχει λεπτομέρειες σχετικά με τις διαφορές μεταξύ HDInsight Windows και Linux και οδηγίες σχετικά με τη μετεγκατάσταση υπάρχοντος φόρτους εργασίας σε ένα σύμπλεγμα βάσει Linux.

> [AZURE.NOTE] HDInsight συμπλεγμάτων Χρησιμοποιήστε Ubuntu μακροπρόθεσμη υποστήριξης (Αποτελεσμάτων) ως το λειτουργικό σύστημα για τους κόμβους του συμπλέγματος. Για πληροφορίες σχετικά με την έκδοση του Ubuntu διαθέσιμη με το HDInsight, μαζί με άλλες πληροφορίες του στοιχείου διαχείρισης εκδόσεων, ανατρέξτε στο θέμα [εκδόσεις των στοιχείων του HDInsight](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Εργασίες μετεγκατάστασης

Η γενική ροή εργασίας για μετεγκατάσταση είναι ως εξής.

![Διάγραμμα ροής εργασίας μετεγκατάστασης](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1.  Διαβάστε κάθε ενότητα αυτού του εγγράφου για να κατανοήσετε τις αλλαγές που ενδεχομένως να απαιτούνται κατά τη μετεγκατάσταση την υπάρχουσα ροή εργασίας, εργασίες, κ.λπ., σε ένα σύμπλεγμα βάσει Linux.

2.  Δημιουργήστε ένα σύμπλεγμα βάσει Linux ως ένα περιβάλλον δοκιμής/ποιότητας εξασφάλισης αναβάθμισης. Για περισσότερες πληροφορίες σχετικά με τη δημιουργία ενός συμπλέγματος που βασίζεται σε Linux, ανατρέξτε στο θέμα [Δημιουργία Linux βάσει συμπλεγμάτων στο HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3.  Αντιγραφή υπάρχοντα έργα, προελεύσεις δεδομένων και δέκτες για το νέο περιβάλλον. Δείτε τα δεδομένα αντιγράφου στην ενότητα περιβάλλον δοκιμής για περισσότερες λεπτομέρειες.

4.  Εκτέλεση δοκιμών επικύρωση για να βεβαιωθείτε ότι οι εργασίες σας λειτουργούν όπως αναμένεται στο νέο σύμπλεγμα.

Αφού επαληθεύσετε ότι όλα λειτουργούν όπως αναμένεται, Προγραμματισμός χρόνου εκτός λειτουργίας για τη μετεγκατάσταση. Κατά τη διάρκεια του χρόνου εκτός λειτουργίας σε αυτό, κάντε τις ακόλουθες ενέργειες.

1.  Δημιουργία αντιγράφων ασφαλείας οποιαδήποτε μεταβατικές δεδομένα που είναι αποθηκευμένα τοπικά στο κόμβους συμπλέγματος. Για παράδειγμα, εάν έχετε δεδομένα που είναι αποθηκευμένα απευθείας σε μια κεφαλή κόμβο.

2.  Διαγραφή του συμπλέγματος που βασίζεται στα Windows.

3.  Δημιουργήστε ένα σύμπλεγμα βάσει Linux χρησιμοποιώντας την ίδια προεπιλεγμένη χώρου αποθήκευσης δεδομένων που χρησιμοποιούνται στο σύμπλεγμα που βασίζεται στα Windows. Αυτό θα σας επιτρέψει να συνεχίσετε να εργάζεστε σε σχέση με τα υπάρχοντα δεδομένα παραγωγής νέο σύμπλεγμα.

4.  Εισαγάγετε οποιαδήποτε δεδομένα μεταβατικές είχατε δημιουργήσει αντίγραφα ασφαλείας.

5.  Έναρξη εργασιών/συνεχίσετε την επεξεργασία χρησιμοποιώντας το νέο σύμπλεγμα.

### <a name="copy-data-to-the-test-environment"></a>Αντιγραφή δεδομένων για το περιβάλλον δοκιμής

Υπάρχουν πολλές μέθοδοι για να αντιγράψετε τα δεδομένα και τις εργασίες, ωστόσο οι δύο που αναφέρονται σε αυτήν την ενότητα είναι οι πιο απλός μέθοδοι για να απευθείας μετακινήσετε τα αρχεία σε ένα σύμπλεγμα δοκιμής.

#### <a name="hdfs-dfs-copy"></a>Αντιγραφή HDFS DFS

Μπορείτε να χρησιμοποιήσετε την εντολή Hadoop HDFS απευθείας αντιγραφής δεδομένων από το χώρο αποθήκευσης για το υπάρχον σύμπλεγμα παραγωγής, ο χώρος αποθήκευσης για ένα νέο σύμπλεγμα δοκιμής, ακολουθώντας τα παρακάτω βήματα.

1. Βρείτε τις πληροφορίες χώρο αποθήκευσης στο λογαριασμό και προεπιλεγμένη κοντέινερ για το υπάρχον σύμπλεγμα. Μπορείτε να το κάνετε αυτό, χρησιμοποιώντας την ακόλουθη δέσμη ενεργειών Azure PowerShell.

        $clusterName="Your existing HDInsight cluster name"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
        write-host "Default container: $clusterInfo.DefaultStorageContainer"

2. Ακολουθήστε τα βήματα στο θέμα των συμπλεγμάτων βάσει δημιουργία Linux στο έγγραφο HDInsight για να δημιουργήσετε ένα νέο περιβάλλον δοκιμής. Διακοπή πριν από τη δημιουργία του συμπλέγματος και αντί για αυτό, επιλέξτε **Προαιρετική ρύθμιση παραμέτρων**.

3. Από την προαιρετική ρύθμιση παραμέτρων blade, επιλέξτε **Συνδεδεμένους λογαριασμούς χώρου αποθήκευσης**.

4. Επιλέξτε **Προσθήκη έναν αριθμό-κλειδί χώρου αποθήκευσης**και, όταν σας ζητηθεί, επιλέξτε το λογαριασμό χώρου αποθήκευσης που επιστράφηκε από τη δέσμη ενεργειών PowerShell στο βήμα 1. Κάντε κλικ στην **επιλογή** σε κάθε blade να τις κλείσετε. Τέλος, δημιουργήστε το σύμπλεγμα.

5. Αφού δημιουργηθεί το σύμπλεγμα, συνδεθείτε χρησιμοποιώντας **SSH.** Εάν δεν είστε εξοικειωμένοι με τη χρήση SSH με το HDInsight, ανατρέξτε σε ένα από τα ακόλουθα άρθρα.

    * [Χρήση SSH με βάσει Linux HDInsight από προγράμματα-πελάτες των Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Χρήση SSH με βάσει Linux HDInsight από προγράμματα-πελάτες του Linux, Unix και Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

6. Από την περίοδο λειτουργίας SSH, χρησιμοποιήστε την ακόλουθη εντολή για την αντιγραφή αρχείων από το χώρο αποθήκευσης στο συνδεδεμένο λογαριασμό στον νέο προεπιλεγμένο λογαριασμό χώρου αποθήκευσης. Αντικαταστήστε το ΚΟΝΤΈΙΝΕΡ και ΛΟΓΑΡΙΑΣΜΟΎ με το κοντέινερ και τις πληροφορίες λογαριασμού που επιστρέφονται από τη δέσμη ενεργειών του PowerShell στο βήμα 1. Αντικαταστήστε τη διαδρομή προς δεδομένων με τη διαδρομή προς ένα αρχείο δεδομένων.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    [AZURE.NOTE] Εάν δεν υπάρχει τη δομή του καταλόγου που περιέχει τα δεδομένα σε περιβάλλον δοκιμής, μπορείτε να δημιουργήσετε χρησιμοποιώντας την ακόλουθη εντολή.

        hdfs dfs -mkdir -p /new/path/to/create

    Το `-p` διακόπτη επιτρέπει τη δημιουργία όλους τους φακέλους στη διαδρομή.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Άμεση αντιγραφή μεταξύ Azure χώρο αποθήκευσης αντικειμένων blob

Εναλλακτικά, μπορεί να θέλετε να χρησιμοποιήσετε το `Start-AzureStorageBlobCopy` Azure PowerShell cmdlet για να αντιγράψετε αντικείμενα BLOB μεταξύ λογαριασμών αποθήκευσης εκτός HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα Πώς να διαχειριστείτε αντικείμενα BLOB Azure ενότητα Χρήση του PowerShell Azure με το χώρο αποθήκευσης Azure.

##<a name="client-side-technologies"></a>Τεχνολογίες πλευρά του προγράμματος-πελάτη

Σε γενικές γραμμές, τεχνολογίες πλευρά του προγράμματος-πελάτη, όπως τα [cmdlet του Azure PowerShell](../powershell-install-configure.md), [Azure CLI](../xplat-cli-install.md) ή το [.NET SDK για Hadoop](https://hadoopsdk.codeplex.com/) θα συνεχίσουν να λειτουργούν με τον ίδιο με Linux βάσει συμπλεγμάτων, κατά την οποία βασίζονται σε REST API που είναι τα ίδια κατά μήκος και οι δύο τύποι σύμπλεγμα OS.

##<a name="server-side-technologies"></a>Τεχνολογίες πλευρά του διακομιστή

Ο παρακάτω πίνακας παρέχει καθοδήγηση στη μετεγκατάσταση στοιχεία διακομιστή που είναι Windows συγκεκριμένες.

| Εάν χρησιμοποιείτε αυτήν την τεχνολογία... | Κάντε αυτήν την ενέργεια... |
| ----- | ----- |
| **PowerShell** (δέσμες ενεργειών του διακομιστή, συμπεριλαμβανομένων των ενεργειών δέσμης ενεργειών χρησιμοποιείται κατά τη δημιουργία συμπλέγματος) | Επανεγγραφή ως πάρτι δέσμες ενεργειών. Για ενέργειες δέσμης ενεργειών, ανατρέξτε στο θέμα [Προσαρμογή Linux βάσει HDInsight με ενέργειες δέσμης ενεργειών](hdinsight-hadoop-customize-cluster-linux.md) και [δέσμη ενεργειών ανάπτυξης ενέργεια για βάσει Linux HDInsight](hdinsight-hadoop-script-actions-linux.md). |
| **Azure CLI** (δέσμες ενεργειών του διακομιστή) | Ενώ το Azure CLI είναι διαθέσιμη στο Linux, δεν προέρχεται προ-εγκατεστημένες σε τους κόμβους κεφαλής σύμπλεγμα HDInsight. Εάν χρειάζεστε για την απενεργοποίηση των δεσμών ενεργειών του διακομιστή, ανατρέξτε στο θέμα [εγκατάσταση του Azure CLI](../xplat-cli-install.md) για πληροφορίες σχετικά με την εγκατάσταση σε πλατφόρμες που βασίζεται στο Linux. |
| **Στοιχεία .NET** | .NET δεν υποστηρίζεται πλήρως στο συμπλεγμάτων βάσει Linux HDInsight. Βάσει Linux καταιγίδας στην HDInsight συμπλεγμάτων δημιουργηθεί μετά την υποστήριξη 28/10/2017 C# καταιγίδας τοπολογίες χρησιμοποιώντας το πλαίσιο SCP.NET. Επιπλέον υποστήριξη για .NET θα προστεθεί σε μελλοντικές ενημερώσεις. |
| **Στοιχεία Win32 ή άλλη τεχνολογία μόνο με Windows** | Καθοδήγηση εξαρτάται από το στοιχείο ή τεχνολογία. ενδέχεται να μπορείτε να βρείτε μια έκδοση που είναι συμβατή με Linux ή ίσως χρειαστεί να βρείτε μια εναλλακτική λύση ή επανεγγραφή αυτό το στοιχείο. |

##<a name="cluster-creation"></a>Δημιουργία συμπλέγματος

Αυτή η ενότητα παρέχει πληροφορίες σχετικά με τις διαφορές κατά τη δημιουργία συμπλέγματος.

### <a name="ssh-user"></a>SSH χρήστη

Βάσει Linux συμπλεγμάτων HDInsight χρησιμοποιεί το πρωτόκολλο **Secure κελύφους (SSH)** για την παροχή απομακρυσμένη πρόσβαση στους κόμβους συμπλέγματος. Σε αντίθεση με συμπλεγμάτων που βασίζεται σε απομακρυσμένο υπολογιστή για Windows, τα περισσότερα προγράμματα-πελάτες SSH δεν παρέχουν μια εμπειρία χρήστη με γραφικά, αλλά αντί για αυτό παρέχει μια γραμμή εντολών που σας επιτρέπει να εκτελέσετε εντολές στο σύμπλεγμα. Ορισμένα προγράμματα-πελάτες (όπως [MobaXterm](http://mobaxterm.mobatek.net/),) παρέχουν ένα πρόγραμμα περιήγησης σύστημα αρχείων γραφικών εκτός από έναν απομακρυσμένο γραμμής εντολών.

Κατά τη δημιουργία συμπλέγματος, πρέπει να δώσετε έναν χρήστη SSH και έναν **κωδικό πρόσβασης** ή **πιστοποιητικό με δημόσιο κλειδί** για τον έλεγχο ταυτότητας.

Συνιστάται να χρησιμοποιείτε πιστοποιητικό δημόσιου κλειδιού, όπως είναι πιο ασφαλή από χρησιμοποιώντας έναν κωδικό πρόσβασης. Πιστοποιητικό ελέγχου ταυτότητας λειτουργεί με δημιουργία υπογεγραμμένου δημόσια/ιδιωτική ζεύγος κλειδιών και, στη συνέχεια, παρέχοντας το δημόσιο κλειδί κατά τη δημιουργία του συμπλέγματος. Όταν συνδέεστε με το διακομιστή χρησιμοποιώντας SSH, το ιδιωτικό κλειδί του υπολογιστή-πελάτη παρέχει έλεγχο ταυτότητας για τη σύνδεση.

Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με το HDInsight, ανατρέξτε στα θέματα:

- [Χρήση SSH με HDInsight από προγράμματα-πελάτες των Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

- [Χρήση SSH με HDInsight από προγράμματα-πελάτες του Linux, Unix και OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Προσαρμογή συμπλέγματος

**Ενέργειες δέσμης ενεργειών** που χρησιμοποιείται με Linux βάσει συμπλεγμάτων πρέπει να εγγράφονται σε πάρτι δέσμη ενεργειών. Ενώ ενέργειες δέσμης ενεργειών μπορούν να χρησιμοποιηθούν κατά τη δημιουργία συμπλέγματος, για βάσει Linux συμπλεγμάτων μπορούν επίσης να χρησιμοποιείται για την εκτέλεση προσαρμογής μετά από ένα σύμπλεγμα είναι προς τα επάνω και εκτελείται. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Προσαρμογή Linux βάσει HDInsight με ενέργειες δέσμης ενεργειών](hdinsight-hadoop-customize-cluster-linux.md) και [δέσμη ενεργειών ανάπτυξης ενέργεια για βάσει Linux HDInsight](hdinsight-hadoop-script-actions-linux.md).

Μια άλλη δυνατότητα προσαρμογής είναι **εκκίνησης**. Για Windows συμπλεγμάτων, αυτό σας επιτρέπει να καθορίσετε τη θέση των επιπλέον βιβλιοθήκες για χρήση με την ομάδα. Μετά τη δημιουργία συμπλέγματος, είναι αυτόματα διαθέσιμο για χρήση με την ομάδα ερωτήματα, χωρίς να χρειάζεται να χρησιμοποιήσετε αυτές τις βιβλιοθήκες `ADD JAR`.

Εκκίνησης για βάσει Linux συμπλεγμάτων δεν παρέχει αυτή η λειτουργία. Αντί για αυτό, χρησιμοποιήστε την ενέργεια δέσμη ενεργειών που τεκμηριώνονται σε [Προσθήκη ομάδας βιβλιοθηκών κατά τη δημιουργία συμπλέγματος](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Εικονικό δίκτυα

HDInsight που βασίζεται σε Windows συμπλεγμάτων μόνο την εργασία με κλασική εικονικού δίκτυα, ενώ συμπλεγμάτων βάσει Linux HDInsight απαιτούν δίκτυα εικονική διαχείριση πόρων. Εάν έχετε πόρων σε μια κλασική εικονικού δικτύου που πρέπει να συνδεθείτε με το σύμπλεγμα Linux HDInsight, ανατρέξτε στο θέμα [σύνδεση κλασική εικονικό δίκτυο σε δίκτυο εικονική διαχείριση πόρων](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Για περισσότερες πληροφορίες σχετικά με τις απαιτήσεις ρύθμισης παραμέτρων για τη χρήση δικτύων εικονικού Azure με HDInsight, ανατρέξτε στο θέμα [δυνατότητες επέκταση HDInsight, χρησιμοποιώντας μια εικονική δικτύου](hdinsight-extend-hadoop-virtual-network.md).

##<a name="management-and-monitoring"></a>Διαχείριση και την παρακολούθηση

Πολλά από το web περιβάλλοντα εργασίας χρήστη που ενδέχεται να έχετε χρησιμοποιήσει με HDInsight που βασίζεται στα Windows, όπως ιστορικού εργασίας ή νήματα περιβάλλοντος εργασίας Χρήστη, είναι διαθέσιμες μέσω Ambari. Επιπλέον, η προβολή Hive Ambari παρέχει έναν τρόπο για να εκτελέσετε Hive ερωτημάτων με το πρόγραμμα περιήγησης web. Το περιβάλλον εργασίας Χρήστη Web Ambari είναι διαθέσιμη στο Linux βάσει συμπλεγμάτων στο https://CLUSTERNAME.azurehdinsight.net.

Για περισσότερες πληροφορίες σχετικά με την εργασία με Ambari, δείτε τα ακόλουθα έγγραφα:

- [Ambari Web](hdinsight-hadoop-manage-ambari.md)

- [Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ειδοποιήσεις Ambari

Ambari έχει ένα σύστημα προειδοποίησης που μπορούν να σας πιθανών προβλημάτων με το σύμπλεγμα. Ειδοποιήσεις εγγραφές εμφανίζονται ως κόκκινο ή κίτρινο στο περιβάλλον Εργασίας Web Ambari, ωστόσο μπορείτε να τις ανακτήσετε επίσης έως το REST API.

> [AZURE.IMPORTANT] Ειδοποιήσεις Ambari υποδείξετε που *μπορεί να* είναι ένα πρόβλημα, δεν υπάρχουν *είναι* ένα πρόβλημα. Για παράδειγμα, μπορεί να λαμβάνετε μια ειδοποίηση ότι δεν είναι δυνατή η πρόσβαση HiveServer2, παρόλο που μπορούν να έχουν πρόσβαση κανονικά.
>
> Πολλά ειδοποιήσεις σχετικά με την εφαρμογή ως ερωτήματα σε σχέση με μια υπηρεσία που βασίζεται στο χρονικό διάστημα και περιμένετε απάντηση σε ένα συγκεκριμένο χρονικό διάστημα. Ώστε να την ειδοποίηση απαραίτητα δεν σημαίνει ότι η υπηρεσία είναι προς τα κάτω, απλώς ότι δεν επιστρέφει αποτελέσματα εντός το αναμενόμενο χρονικό πλαίσιο.

Σε γενικές γραμμές, θα πρέπει να αξιολογήσετε εάν μια ειδοποίηση έχει γίνει που μεσολαβούν για μεγάλο χρονικό, ή να αντικατοπτρίζει προβλήματα χρηστών που ήταν προηγουμένως έχουν αναφερθεί με το σύμπλεγμα πριν κάνετε κάποια ενέργεια σε αυτό.

##<a name="file-system-locations"></a>Θέσεις αρχείων συστήματος

Το σύστημα αρχείων συμπλέγματος Linux είναι διάταξη παρόμοια με διαφορετικό τρόπο από συμπλεγμάτων HDInsight που βασίζεται στα Windows. Χρησιμοποιήστε τον παρακάτω πίνακα για να βρείτε που χρησιμοποιούνται ευρέως αρχεία.

| Πρέπει να βρείτε... | Βρίσκεται... |
| ----- | ----- |
| Ρύθμιση παραμέτρων | `/etc`. Για παράδειγμα,`/etc/hadoop/conf/core-site.xml` |
| Αρχεία καταγραφής | `/var/logs` |
| Δεδομένα Hortonworks πλατφόρμα (HDP) | `/usr/hdp`. Υπάρχουν δύο καταλόγους που βρίσκεται εδώ, που είναι η τρέχουσα έκδοση HDP (για παράδειγμα, `2.2.9.1-1`,) και `current`. Το `current` καταλόγου περιέχει συμβολικής συνδέσεων σε αρχεία και καταλόγους που βρίσκεται στον κατάλογο αριθμού έκδοσης και παρέχεται ως έναν εύκολο τρόπο πρόσβαση σε αρχεία HDP από την έκδοση αριθμός θα αλλάξει την έκδοση HDP έχει ενημερωθεί. |
| hadoop streaming.jar | `/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Σε γενικές γραμμές, εάν γνωρίζετε το όνομα του αρχείου, μπορείτε να χρησιμοποιήσετε την ακόλουθη εντολή από μια περίοδο λειτουργίας SSH για να βρείτε τη διαδρομή του αρχείου:

    find / -name FILENAME 2>/dev/null

Μπορείτε επίσης να χρησιμοποιήσετε χαρακτήρες μπαλαντέρ με το όνομα του αρχείου. Για παράδειγμα, `find / -name *streaming*.jar 2>/dev/null` θα επιστρέψει τη διαδρομή σε οποιαδήποτε αρχεία βάζο που περιέχουν τη λέξη "ροή' ως μέρος του ονόματος αρχείου.

##<a name="hive-pig-and-mapreduce"></a>Ομάδα γουρούνι και MapReduce

Γουρούνι και MapReduce φόρτους εργασίας είναι παρόμοιες σε βάσει Linux συμπλεγμάτων - η κύρια διαφορά είναι εάν χρησιμοποιείτε απομακρυσμένης επιφάνειας εργασίας για να συνδεθείτε σε ένα σύμπλεγμα που βασίζεται σε Windows και να εκτελέσετε εργασίες, θα χρησιμοποιήσετε SSH με Linux βάσει συμπλεγμάτων.

- [Χρήση γουρούνι με SSH](hdinsight-hadoop-use-pig-ssh.md)

- [Χρήση MapReduce με SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Ομάδα

Το ακόλουθο διάγραμμα παρέχει οδηγίες σχετικά με τη μετεγκατάσταση σας φόρτους εργασίας ομάδας.

| Στην βασίζεται στα Windows, να χρησιμοποιήστε... | Στην βάσει Linux... |
| ----- | ----- |
| **Επεξεργασία ομάδας** | [Προβολή της ομάδας στο Ambari](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Για να ενεργοποιήσετε την Tez | Tez είναι ο προεπιλεγμένος μηχανισμός εκτέλεσης για Linux βάσει συμπλεγμάτων, ώστε η πρόταση σύνολο δεν είναι πλέον απαραίτητη. |
| Αρχεία CMD ή δέσμες ενεργειών στο διακομιστή κλήση ως μέρος μιας ομάδας εργασίας | χρήση δεσμών ενεργειών πάρτι |
| `hive`εντολή από απομακρυσμένης επιφάνειας εργασίας | Χρήση [Beeline](hdinsight-hadoop-use-hive-beeline.md) ή [Hive από μια περίοδο λειτουργίας SSH](hdinsight-hadoop-use-hive-ssh.md) |

##<a name="storm"></a>Καταιγίδας

| Στην βασίζεται στα Windows, να χρησιμοποιήστε... | Στην βάσει Linux... |
| ----- | ----- |
| Πίνακας εργαλείων καταιγίδας | Πίνακας εργαλείων καταιγίδας δεν είναι διαθέσιμη. Ανατρέξτε στο θέμα [ανάπτυξη και διαχείριση καταιγίδας τοπολογίες στην βάσει Linux HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md) για τρόπους για να υποβάλετε τοπολογίες |
| Καταιγίδας περιβάλλοντος εργασίας Χρήστη | Περιβάλλον εργασίας Χρήστη του καταιγίδας είναι διαθέσιμη από την https://CLUSTERNAME.azurehdinsight.net/stormui |
| Visual Studio για να δημιουργήσετε, να αναπτύξετε και να διαχειριστείτε C# ή υβριδικό τοπολογίες | Visual Studio μπορεί να χρησιμοποιηθεί για να δημιουργήσετε, ανάπτυξη και διαχείριση C# (SCP.NET) ή υβριδικό τοπολογίες στην βάσει Linux καταιγίδας στην συμπλεγμάτων HDInsight που δημιουργήθηκαν μετά 28/10/2017. |

##<a name="hbase"></a>HBase

Στην Linux βάσει συμπλεγμάτων, είναι το γονικό στοιχείο znode για HBase `/hbase-unsecure`. Πρέπει να ορίσετε τα αυτή η ρύθμιση παραμέτρων για οποιονδήποτε υπολογιστή-πελάτη Java εφαρμογές που χρησιμοποιούν API Java εγγενούς HBase.

Ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής Java βάσει HBase](hdinsight-hbase-build-java-maven.md) για ένα πρόγραμμα-πελάτη παράδειγμα που ορίζει αυτήν την τιμή.

##<a name="spark"></a>Τους

Συμπλεγμάτων τους ήταν διαθέσιμα σε Windows συμπλεγμάτων κατά την προεπισκόπηση; Ωστόσο, για την έκδοση, τους είναι διαθέσιμη μόνο με Linux βάσει συμπλεγμάτων. Δεν έχει διαδρομή μετεγκατάστασης από ένα σύμπλεγμα προεπισκόπηση τους που βασίζεται σε Windows σε ένα σύμπλεγμα βάσει Linux τους κυκλοφορίας.

##<a name="known-issues"></a>Γνωστά θέματα

### <a name="azure-data-factory-custom-net-activities"></a>Azure εργοστασίου δεδομένων προσαρμοσμένες δραστηριότητες .NET

Azure εργοστασίου δεδομένων προσαρμοσμένες δραστηριότητες .NET δεν υποστηρίζονται αυτήν τη στιγμή σε συμπλεγμάτων βάσει Linux HDInsight. Αντί για αυτό, πρέπει να χρησιμοποιήσετε μία από τις παρακάτω μεθόδους για την υλοποίηση προσαρμοσμένες δραστηριότητες ως μέρος του διοχέτευσης ADF.

-   Εκτέλεση δραστηριοτήτων .NET στη δέσμη Azure χώρου συγκέντρωσης. Ανατρέξτε στην ενότητα υπηρεσία χρήση Azure δέσμη συνδεδεμένες [Χρήση προσαρμοσμένων](../data-factory/data-factory-use-custom-activities.md#AzureBatch) δραστηριοτήτων σε μια διαδικασία εργοστασίου δεδομένων Azure

-   Υλοποίηση τη δραστηριότητα ως δραστηριότητα MapReduce. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ενεργοποίηση προγραμμάτων MapReduce από την προέλευση δεδομένων](../data-factory/data-factory-map-reduce.md) .

### <a name="line-endings"></a>Τέλους γραμμής

Σε γενικές γραμμές, τέλους γραμμής σε συστήματα που βασίζονται στα Windows χρησιμοποιούν CRLF, ενώ Linux συστήματα χρησιμοποιούν LF. Εάν αγροτικά προϊόντα ή αναμενόμενο, τα δεδομένα με CRLF τέλους γραμμής, ίσως χρειαστεί να τροποποιήσετε το παραγωγών ή καταναλωτών ώστε να λειτουργεί με την κατάληξη γραμμής LF.

Για παράδειγμα, χρήση του Azure PowerShell HDInsight ερώτημα σε ένα σύμπλεγμα που βασίζεται σε Windows θα επιστρέψει δεδομένα με CRLF. Το ίδιο ερώτημα με ένα σύμπλεγμα βάσει Linux θα επιστρέψει LF. Σε πολλές περιπτώσεις, αυτό δεν έχει σημασία του καταναλωτή δεδομένων, ωστόσο αυτό θα πρέπει να είναι Διερεύνηση πριν από τη μετεγκατάσταση σε ένα σύμπλεγμα βάσει Linux.

Εάν έχετε δέσμες ενεργειών που θα εκτελεστεί απευθείας σε τους κόμβους Linux σύμπλεγμα (όπως μια δέσμη ενεργειών Python χρησιμοποιείται με την ομάδα ή μια εργασία MapReduce), πρέπει πάντα να χρησιμοποιείτε LF ως κατάληξη γραμμής. Εάν χρησιμοποιείτε το CRLF, μπορεί να δείτε σφάλματα κατά την εκτέλεση των δεσμών ενεργειών σε ένα σύμπλεγμα βάσει Linux.

Εάν γνωρίζετε ότι οι δέσμες ενεργειών δεν περιέχουν συμβολοσειρές με ενσωματωμένο CR χαρακτήρες, μπορείτε να πραγματοποιήσετε μαζική αλλαγή τέλους γραμμής χρησιμοποιώντας μία από τις παρακάτω μεθόδους:

-   **Εάν έχετε δέσμες ενεργειών που σχεδιάζετε να κάνετε αποστολή στο σύμπλεγμα**, χρησιμοποιήστε τις ακόλουθες εντολές του PowerShell για να αλλάξετε τέλους γραμμής από CRLF σε LF πριν από την αποστολή της δέσμης ενεργειών στο σύμπλεγμα.

        $original_file ='c:\path\to\script.py'
        $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
        [IO.File]::WriteAllText($original_file, $text)

-   **Εάν έχετε δέσμες ενεργειών που βρίσκονται ήδη στο χώρο αποθήκευσης της χρησιμοποιούνται από το σύμπλεγμα**, μπορείτε να χρησιμοποιήσετε την ακόλουθη εντολή από μια περίοδο λειτουργίας SSH στο σύμπλεγμα βάσει Linux για να τροποποιήσετε τη δέσμη ενεργειών.

        hdfs dfs -get wasbs:///path/to/script.py oldscript.py
        tr -d '\r' < oldscript.py > script.py
        hdfs dfs -put -f script.py wasbs:///path/to/script.py

##<a name="next-steps"></a>Επόμενα βήματα

-   [Μάθετε πώς να δημιουργείτε βάσει Linux HDInsight συμπλεγμάτων](hdinsight-hadoop-provision-linux-clusters.md)

-   [Σύνδεση σε ένα σύμπλεγμα βάσει Linux χρησιμοποιώντας SSH από ένα πρόγραμμα-πελάτη των Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

-   [Σύνδεση σε ένα σύμπλεγμα βάσει Linux χρησιμοποιώντας SSH από ένα πρόγραμμα-πελάτη Linux, Unix ή Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

-   [Διαχείριση ένα σύμπλεγμα βάσει Linux χρησιμοποιώντας Ambari](hdinsight-hadoop-manage-ambari.md)
