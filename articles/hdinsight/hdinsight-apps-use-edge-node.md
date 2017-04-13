<properties
    pageTitle="Χρήση κόμβους κενή άκρο στο HDInsight | Microsoft Azure"
    description="Πώς μπορείτε να προσθέσετε έναν κόμβο άκρη ampty σύμπλεγμα HDInsight που μπορούν να χρησιμοποιηθούν ως ένα πρόγραμμα-πελάτη και τις εφαρμογές σας HDInsight δοκιμής/κεντρικού υπολογιστή."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>Χρήση κόμβους κενή άκρο στο HDInsight

Μάθετε πώς μπορείτε να προσθέσετε έναν κόμβο κενή άκρο σε ένα σύμπλεγμα βάσει Linux HDInsight. Μια κενή άκρη κόμβου είναι μια εικονική μηχανή Linux με τα ίδια εργαλεία προγράμματος-πελάτη εγκατασταθεί και ρυθμιστεί όπως το headnodes, αλλά με τις υπηρεσίες του hadoop δεν εκτελείται. Μπορείτε να χρησιμοποιήσετε τον κόμβο άκρη για πρόσβαση σε σύμπλεγμα, δοκιμές σας εφαρμογές προγράμματος-πελάτη και φιλοξενεί τις εφαρμογές προγράμματος-πελάτη σας. 

Μπορείτε να προσθέσετε έναν κόμβο κενή άκρο σε ένα υπάρχον σύμπλεγμα HDInsight, σε ένα νέο σύμπλεγμα κατά τη δημιουργία του συμπλέγματος. Προσθήκη έναν κόμβο κενή άκρο γίνεται χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure.  Το παρακάτω παράδειγμα παρουσιάζει πώς γίνεται χρησιμοποιώντας ένα πρότυπο:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Όπως φαίνεται στο δείγμα, μπορείτε προαιρετικά να καλέσετε μια [ενέργεια δέσμη ενεργειών](hdinsight-hadoop-customize-cluster-linux.md) για να εκτελέσετε πρόσθετες ρυθμίσεις, όπως την εγκατάσταση [Απόχρωση Apache](hdinsight-hadoop-hue-linux.md) στον κόμβο άκρο.

Αφού δημιουργήσετε έναν κόμβο ακμή, μπορείτε να συνδεθείτε στον κόμβο άκρη χρησιμοποιώντας SSH και εκτελέστε εργαλεία προγράμματος-πελάτη για να αποκτήσετε πρόσβαση στο σύμπλεγμα Hadoop στη HDInsight.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Προσθέστε έναν κόμβο άκρο στο υπάρχον σύμπλεγμα

Σε αυτήν την ενότητα, μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων για να προσθέσετε έναν κόμβο άκρο σε ένα υπάρχον σύμπλεγμα HDInsight.  Μπορείτε να βρείτε το πρότυπο διαχείρισης πόρων στο [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode). Το πρότυπο διαχείρισης πόρων κλήσεις μιας δέσμης ενεργειών της δέσμης ενεργειών που βρίσκεται στο https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Η δέσμη ενεργειών δεν εκτελέστε τις ενέργειες.  Πρόκειται για μια επίδειξη κλήσης δέσμης ενεργειών ενέργεια από ένα πρότυπο από διαχειριστή πόρων.

**Για να προσθέσετε έναν κόμβο κενή άκρο σε ένα υπάρχον σύμπλεγμα**

1. Δημιουργήστε ένα σύμπλεγμα HDInsight, εάν δεν έχετε ακόμη.  Ανατρέξτε στο θέμα [Hadoop πρόγραμμα εκμάθησης: γρήγορα αποτελέσματα με το Linux βάσει Hadoop στο HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Κάντε κλικ στην παρακάτω εικόνα για να συνδεθείτε Azure και ανοίξτε το πρότυπο διαχείρισης πόρων Azure στην πύλη του Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Ρυθμίστε τις ακόλουθες ιδιότητες:

    - CLUSTERNAME: Πληκτρολογήστε το όνομα του ένα υπάρχον σύμπλεγμα HDInsight.
    - EDGENODESIZE: Επιλέξτε ένα από τα μεγέθη Εικονική.
    - EDGENODEPREFIX: Η προεπιλεγμένη τιμή είναι **νέα**.  Η προεπιλεγμένη τιμή, το όνομα του κόμβου άκρο είναι **νέα edgenode**.  Μπορείτε να προσαρμόσετε το πρόθεμα από την πύλη. Μπορείτε επίσης να προσαρμόσετε το πλήρες όνομα από το πρότυπο.


4. Κάντε κλικ στο **κουμπί OK** για να αποθηκεύσετε τις αλλαγές.
5. Στην **ομάδα πόρων**, επιλέξτε μια ομάδα πόρων.
6. Κάντε κλικ στην επιλογή **Αναθεώρηση νομικές τους όρους**και, στη συνέχεια, κάντε κλικ στην επιλογή **αγορά**.
7. Επιλέξτε **το Pin στον πίνακα εργαλείων**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Προσθέστε έναν κόμβο άκρη κατά τη δημιουργία ενός συμπλέγματος

Σε αυτήν την ενότητα, μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων για να δημιουργήσετε σύμπλεγμα HDInsight με έναν κόμβο άκρο.  Μπορείτε να βρείτε το πρότυπο διαχείρισης πόρων σε δημόσια [κοντέινερ χώρου αποθήκευσης αντικειμένων Blob του Azure](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json). Το πρότυπο διαχείρισης πόρων κλήσεις μιας δέσμης ενεργειών της δέσμης ενεργειών που βρίσκεται στο https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Η δέσμη ενεργειών δεν εκτελέστε τις ενέργειες.  Πρόκειται για μια επίδειξη κλήσης δέσμης ενεργειών ενέργεια από ένα πρότυπο από διαχειριστή πόρων.

**Για να προσθέσετε έναν κόμβο κενή άκρο σε ένα υπάρχον σύμπλεγμα**

1. Δημιουργήστε ένα σύμπλεγμα HDInsight, εάν δεν έχετε ακόμη.  Ανατρέξτε στο θέμα [Hadoop πρόγραμμα εκμάθησης: γρήγορα αποτελέσματα με το Linux βάσει Hadoop στο HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Κάντε κλικ στην παρακάτω εικόνα για να συνδεθείτε Azure και ανοίξτε το πρότυπο διαχείρισης πόρων Azure στην πύλη του Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Ρυθμίστε τις ακόλουθες ιδιότητες:
        
    - CLUSTERNAME: Πληκτρολογήστε ένα όνομα για το νέο σύμπλεγμα για να δημιουργήσετε.
    - CLUSTERLOGINUSERNAME: Πληκτρολογήστε το όνομα χρήστη Hadoop HTTP.  Το προεπιλεγμένο όνομα είναι **διαχειριστής**.
    - CLUSTERLOGINPASSWORD: Εισαγάγετε τον κωδικό πρόσβασης χρήστη Hadoop HTTP.
    - SSHUSERNAME: Πληκτρολογήστε το όνομα χρήστη SSH. Το προεπιλεγμένο όνομα είναι **sshuser**.
    - SSHPASSWORD: Εισαγάγετε τον κωδικό πρόσβασης χρήστη SSH.
    - ΘΈΣΗ: Πληκτρολογήστε τη θέση του στο όνομα της ομάδας πόρων, το σύμπλεγμα και τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης.
    - CLUSTERTYPE: Η προεπιλεγμένη τιμή είναι **hadoop**.
    - CLUSTERWORKERNODECOUNT: Η προεπιλεγμένη τιμή είναι 2.
    - EDGENODESIZE: Επιλέξτε ένα από τα μεγέθη Εικονική.
    - EDGENODEPREFIX: Η προεπιλεγμένη τιμή είναι **νέα**.  Η προεπιλεγμένη τιμή, το όνομα του κόμβου άκρο είναι **νέα edgenode**.  Μπορείτε να προσαρμόσετε το πρόθεμα από την πύλη. Μπορείτε επίσης να προσαρμόσετε το πλήρες όνομα από το πρότυπο.

4. Κάντε κλικ στο **κουμπί OK** για να αποθηκεύσετε τις αλλαγές.
5. Στην **ομάδα πόρων**, πληκτρολογήστε ένα νέο όνομα ομάδας πόρων.
6. Κάντε κλικ στην επιλογή **Αναθεώρηση νομική τους όρους**και, στη συνέχεια, κάντε κλικ στην επιλογή **αγορά**.
7. Επιλέξτε **το Pin στον πίνακα εργαλείων**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**. 


## <a name="access-an-edge-node"></a>Πρόσβαση σε έναν κόμβο άκρου

Ο κόμβος άκρη ssh τελικού σημείου είναι <EdgeNodeName>. <ClusterName>-ssh.azurehdinsight.net:22.  Για παράδειγμα, νέα-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Ο κόμβος άκρη εμφανίζεται ως μια εφαρμογή στην πύλη του Azure.  Η πύλη παρέχει τις πληροφορίες για να αποκτήσετε πρόσβαση στον κόμβο άκρη χρησιμοποιώντας SSH.

**Για να επαληθεύσετε το τελικό σημείο SSH κόμβο άκρου**

1. Πραγματοποιήστε είσοδο [πύλη του Azure](https://portal.azure.com).
2. Ανοίξτε το σύμπλεγμα HDInsight με έναν κόμβο άκρο.
3. Κάντε κλικ στην επιλογή **εφαρμογές** από το σύμπλεγμα blade. Βλέπετε τον κόμβο άκρο.  Το προεπιλεγμένο όνομα είναι **νέα edgenode**.
4. Κάντε κλικ στον κόμβο άκρο. Βλέπετε το τελικό σημείο SSH.

**Για να χρησιμοποιήσετε Hive στον κόμβο άκρου**

5. Χρησιμοποιήστε SSH για να συνδεθείτε με τον κόμβο άκρο.  Ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, Unix, ή λειτουργικό σύστημα OS X](hdinsight-hadoop-linux-use-ssh-unix.md) ή [Χρησιμοποιήστε SSH με βάσει Linux Hadoop σε HDInsight από τα Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Αφού έχετε συνδεθεί στον κόμβο άκρη χρησιμοποιώντας SSH, χρησιμοποιήστε την ακόλουθη εντολή για να ανοίξετε την κονσόλα ομάδα:

        hive
7. Εκτελέστε την παρακάτω εντολή για να εμφανίσετε πίνακες Hive στο σύμπλεγμα:

        show tables;

## <a name="delete-an-edge-node"></a>Διαγράψτε έναν κόμβο άκρου

Μπορείτε να διαγράψετε έναν κόμβο άκρο από την πύλη του Azure.

**Για να αποκτήσετε πρόσβαση σε έναν κόμβο άκρου**

1. Πραγματοποιήστε είσοδο [πύλη του Azure](https://portal.azure.com).
2. Ανοίξτε το σύμπλεγμα HDInsight με έναν κόμβο άκρο.
3. Κάντε κλικ στην επιλογή **εφαρμογές** από το σύμπλεγμα blade. Βλέπετε μια λίστα με τους κόμβους άκρο.  
4. Κάντε δεξί κλικ στον κόμβο άκρο που θέλετε να διαγράψετε και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαγραφή**.
5. Κάντε κλικ στο κουμπί **Ναι** για επιβεβαίωση.

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο μάθατε πώς μπορείτε να προσθέσετε έναν κόμβο edge και πώς μπορείτε να αποκτήσετε πρόσβαση στον κόμβο άκρο. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

- [Εγκατάσταση HDInsight εφαρμογές](hdinsight-apps-install-applications.md): Μάθετε πώς μπορείτε να εγκαταστήσετε μια εφαρμογή HDInsight σε σας συμπλεγμάτων.
- [Εγκατάσταση προσαρμοσμένες εφαρμογές HDInsight](hdinsight-apps-install-custom-applications.md): Μάθετε πώς να αναπτύξετε μια εφαρμογή του αδημοσίευτων HDInsight με το HDInsight.
- [Δημοσίευση HDInsight εφαρμογές](hdinsight-apps-publish-applications.md): Μάθετε πώς μπορείτε να δημοσιεύσετε τις προσαρμοσμένες εφαρμογές HDInsight με το Azure Marketplace.
- [MSDN: εγκατάσταση μιας εφαρμογής HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Μάθετε πώς μπορείτε να ορίσετε τις εφαρμογές HDInsight.
- [Προσαρμογή Linux βάσει HDInsight συμπλεγμάτων με χρήση δέσμης ενεργειών](hdinsight-hadoop-customize-cluster-linux.md): Μάθετε πώς μπορείτε να χρησιμοποιήσετε την ενέργεια δέσμη ενεργειών για να εγκαταστήσετε πρόσθετες εφαρμογές.
- [Δημιουργία Linux βάσει Hadoop συμπλεγμάτων στο HDInsight με τη χρήση προτύπων για τη διαχείριση πόρων](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Μάθετε πώς μπορείτε να καλέσετε από διαχειριστή πόρων πρότυπα για να δημιουργήσετε συμπλεγμάτων HDInsight.

