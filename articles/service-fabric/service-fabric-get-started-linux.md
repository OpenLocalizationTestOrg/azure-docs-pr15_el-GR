<properties
   pageTitle="Ρυθμίστε το περιβάλλον ανάπτυξης στην Linux | Microsoft Azure"
   description="Εγκαταστήστε το περιβάλλον εκτέλεσης και SDK και δημιουργήστε ένα σύμπλεγμα τοπικής ανάπτυξης στην Linux. Αφού ολοκληρώσετε αυτό το πρόγραμμα εγκατάστασης, θα είστε έτοιμοι να δημιουργήσετε εφαρμογές."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="seanmck"/>

# <a name="prepare-your-development-environment-on-linux"></a>Προετοιμάσετε το περιβάλλον ανάπτυξης στην Linux


> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Για να αναπτύξετε και να εκτελέσετε [Azure Service ύφασμα εφαρμογές](service-fabric-application-model.md) στον υπολογιστή σας Linux ανάπτυξης, εγκαταστήστε το περιβάλλον εκτέλεσης και κοινές SDK. Μπορείτε επίσης να εγκαταστήσετε προαιρετικά SDK για Java και πυρήνα .NET.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
### <a name="supported-operating-system-versions"></a>Εκδόσεις υποστηριζόμενο λειτουργικό σύστημα
Υποστηρίζονται οι ακόλουθες εκδόσεις λειτουργικό σύστημα για την ανάπτυξη:

- Ubuntu 16.04 (Xenial Xerus)

## <a name="update-your-apt-sources"></a>Ενημερώστε την κατάλληλη προελεύσεις

Για να εγκαταστήσετε το SDK και το πακέτο συσχετισμένη χρόνου εκτέλεσης μέσω κατάλληλη get, πρέπει πρώτα να ενημερώσετε την κατάλληλη προελεύσεις.

1. Ανοίξτε μια terminal.
2. Προσθέστε την υπηρεσία ύφασμα repo στη λίστα προελεύσεων.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Προσθέστε το νέο κλειδί GPG την κατάλληλη αρχείο κλειδιών.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```

4. Ανανέωση λίστες σας πακέτου με βάση το αποθετήρια που προστέθηκε πρόσφατα.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Εγκατάσταση και ρύθμιση του SDK

Αφού ενημερωθούν τις πηγές σας, μπορείτε να εγκαταστήσετε το SDK.

1. Εγκαταστήστε το πακέτο SDK ύφασμα υπηρεσίας. Θα σας ζητηθεί να επιβεβαιώσετε την εγκατάσταση και να συμφωνήσουν για μια άδεια χρήσης.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. Εκτελέστε τη δέσμη ενεργειών της εγκατάστασης του SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-cross-platform-cli"></a>Ρύθμιση του Azure CLI πλατφόρμες

Το [Azure CLI πλατφόρμες] [ azure-xplat-cli-github] περιλαμβάνει εντολές για αλληλεπίδραση με οντοτήτων ύφασμα υπηρεσίας, συμπεριλαμβανομένων των συμπλεγμάτων και εφαρμογές. Βασίζεται σε Node.js, συνεπώς, [Βεβαιωθείτε ότι έχετε εγκαταστήσει το κόμβο] [ install-node] πριν να συνεχίσετε με τις παρακάτω οδηγίες.

1. Δημιουργία διπλότυπου τα github repo στον υπολογιστή σας στην ανάπτυξη.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Εναλλαγή σε το κλωνοποιημένο repo και εγκαταστήστε το CLI εξαρτήσεις χρησιμοποιώντας τη Διαχείριση πακέτου κόμβου (npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Δημιουργήστε μια symlink από το φάκελο Κάδος/azure από το κλωνοποιημένο repo να /usr/bin/azure ώστε να προστεθεί διαδρομή σας και εντολές είναι διαθέσιμες από οποιονδήποτε κατάλογο.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Τέλος, ενεργοποίηση αυτόματης συμπλήρωσης ύφασμα υπηρεσίας εντολών.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Ρύθμιση μιας τοπικής συμπλέγματος

Εάν όλα τα στοιχεία έχει εγκατασταθεί με επιτυχία, θα πρέπει να μπορείτε να ξεκινήσετε ένα τοπικό σύμπλεγμα.

1. Εκτελέστε τη δέσμη ενεργειών ρύθμισης συμπλέγματος.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Ανοίξτε ένα πρόγραμμα περιήγησης web και μεταβείτε σε http://localhost:19080/Explorer. Εάν το σύμπλεγμα έχει ξεκινήσει, θα πρέπει να βλέπετε τον πίνακα εργαλείων Explorer ύφασμα υπηρεσίας.

    ![Εξερεύνηση ύφασμα υπηρεσίας στην Linux][sfx-linux]

Σε αυτό το σημείο, θα μπορείτε να αναπτύξετε προ-δομημένες πακέτων εφαρμογών υπηρεσίας ύφασμα ή νέα έγγραφα που βασίζονται σε κοντέινερ επισκέπτη ή εκτελέσιμα επισκέπτη. Για να δημιουργήσετε νέες υπηρεσίες χρησιμοποιώντας το Java ή .NET πυρήνα SDK, ακολουθήστε τα παρακάτω βήματα προαιρετική εγκατάσταση.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Εγκατάσταση της προσθήκης Java SDK και νέον Έκλειψη που (προαιρετικά)

Το SDK Java παρέχει τις βιβλιοθήκες και τα πρότυπα που απαιτούνται για να δημιουργήσετε υπηρεσίες δομής υπηρεσία χρησιμοποιώντας Java.

1. Εγκαταστήστε το πακέτο Java SDK.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Εκτελέστε τη δέσμη ενεργειών της εγκατάστασης του SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

Μπορείτε να εγκαταστήσετε την προσθήκη Έκλειψη για ύφασμα υπηρεσίας από μέσα του IDE νέον Έκλειψη.

1. Στο Έκλειψη, βεβαιωθείτε ότι έχετε Buildship έκδοση 1.0.17 ή νεότερη έκδοση εγκατεστημένο. Μπορείτε να ελέγξετε τις εκδόσεις των εγκατεστημένων στοιχείων, επιλέγοντας **Βοήθεια > λεπτομέρειες εγκατάστασης**. Μπορείτε να ενημερώσετε Buildship ακολουθώντας τις οδηγίες [εδώ][buildship-update].

2. Για να εγκαταστήσετε την προσθήκη ύφασμα υπηρεσίας, επιλέξτε **Βοήθεια > εγκατάσταση νέου λογισμικού...**

3. Στο πλαίσιο κειμένου "Δυνατότητα εργασίας με", πληκτρολογήστε: http://dl.windowsazure.com/eclipse/servicefabric

4. Κάντε κλικ στην επιλογή Προσθήκη.

    ![Προσθήκη Έκλειψη][sf-eclipse-plugin]

5. Επιλέξτε την προσθήκη ύφασμα υπηρεσίας και κάντε κλικ στο κουμπί Επόμενο.

6. Συνεχίστε με την εγκατάσταση και αποδεχτείτε την άδεια χρήσης τελικού χρήστη.

## <a name="install-the-net-core-sdk-optional"></a>Εγκαταστήστε το .NET πυρήνα SDK (προαιρετικά)

Το μέγεθος των κύριων .NET SDK παρέχει τις βιβλιοθήκες και τα πρότυπα που απαιτούνται για να δημιουργήσετε υπηρεσίες δομής υπηρεσία χρησιμοποιώντας πυρήνα .NET πλατφόρμες.

1. Εγκαταστήστε το πακέτο .NET πυρήνα SDK.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. Εκτελέστε τη δέσμη ενεργειών της εγκατάστασης του SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Επόμενα βήματα

- [Δημιουργία του πρώτου εφαρμογής Java σε Linux](service-fabric-create-your-first-linux-application-with-java.md)

- [Προετοιμάσετε το περιβάλλον ανάπτυξης στην OSX](service-fabric-get-started-mac.md)


<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
