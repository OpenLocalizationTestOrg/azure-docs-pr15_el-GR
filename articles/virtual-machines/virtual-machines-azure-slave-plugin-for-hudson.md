<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε το Azure εξαρτώμενη προσθήκης με ενσωμάτωση συνεχής Hudson | Microsoft Azure"
    description="Περιγράφει τον τρόπο χρήσης του Azure εξαρτώμενη προσθήκης με ενσωμάτωση συνεχούς Hudson."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Πώς μπορείτε να χρησιμοποιήσετε το Azure εξαρτώμενη προσθήκης με ενσωμάτωση συνεχής Hudson

Η προσθήκη για Hudson Azure εξαρτώμενη σάς επιτρέπει να προμηθεύσουν εξαρτώμενη κόμβους στην Azure κατά την εκτέλεση κατανεμημένων δημιουργεί.

## <a name="install-the-azure-slave-plug-in"></a>Εγκαταστήστε το Azure εξαρτώμενη προσθήκης

1. Στον πίνακα εργαλείων Hudson, κάντε κλικ στην επιλογή **Διαχείριση Hudson**.

1. Στη σελίδα **Διαχείριση Hudson** , κάντε κλικ στη **Διαχείριση προσθηκών**.

1. Κάντε κλικ στην καρτέλα **διαθέσιμος** .

1. Κάντε κλικ στο κουμπί **Αναζήτηση** και πληκτρολογήστε **Azure** για να περιορίσετε τη λίστα σε σχετικό προσθήκες.

    Εάν επιλέξετε να κύλιση στη λίστα διαθέσιμα προσθηκών, θα βρείτε το Azure εξαρτώμενη προσθήκης στην ενότητα **Διαχείριση συμπλέγματος και κατανεμημένης δημιουργία** στην καρτέλα **άλλα άτομα** .

1. Επιλέξτε το πλαίσιο ελέγχου για **Προσθήκη εξαρτώμενη Azure**.

1. Κάντε κλικ στην επιλογή **εγκατάσταση**.

1. Επανεκκινήστε Hudson.

Τώρα που η προσθήκη είναι εγκατεστημένο, θα ήταν τα επόμενα βήματα για να ρυθμίσετε τις παραμέτρους της προσθήκης με το προφίλ του Azure τη συνδρομή σας και να δημιουργήσετε ένα πρότυπο που θα χρησιμοποιηθεί κατά τη δημιουργία του Εικονική για τον κόμβο εξαρτώμενη.

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Ρύθμιση παραμέτρων του Azure εξαρτώμενη προσθήκης με το προφίλ σας συνδρομή

Ένα προφίλ συνδρομή, επίσης γνωστή ως δημοσίευση ρυθμίσεις, είναι ένα αρχείο XML που περιέχει τα διαπιστευτήρια ασφαλούς και ορισμένες πρόσθετες πληροφορίες που θα πρέπει να εργαστείτε με το Azure στο περιβάλλον ανάπτυξής σας. Για να ρυθμίσετε τις παραμέτρους του Azure εξαρτώμενη προσθήκης, χρειάζεστε:

* Το αναγνωριστικό συνδρομής
* Ένα πιστοποιητικό διαχείρισης για τη συνδρομή σας

Αυτά μπορείτε να βρείτε στο σας [προφίλ συνδρομής]. Ακολουθεί ένα παράδειγμα ενός προφίλ συνδρομής.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Όταν έχετε συνδρομή στο προφίλ σας, ακολουθήστε τα παρακάτω βήματα για να ρυθμίσετε τις παραμέτρους του Azure εξαρτώμενη προσθήκης.

1. Στον πίνακα εργαλείων Hudson, κάντε κλικ στην επιλογή **Διαχείριση Hudson**.

1. Κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων του συστήματος**.

1. Κάντε κύλιση προς τα κάτω στη σελίδα για να βρείτε την ενότητα **Cloud** .

1. Κάντε κλικ στην επιλογή **Προσθήκη νέου cloud > Microsoft Azure**.

    ![Προσθήκη νέου cloud][add new cloud]

    Αυτό θα εμφανίσει τα πεδία όπου θέλετε να εισαγάγετε τις λεπτομέρειες συνδρομής.

    ![ρύθμιση παραμέτρων προφίλ][configure profile]

1. Αντιγράψτε το πιστοποιητικό αναγνωριστικό και διαχείρισης συνδρομής από το προφίλ σας συνδρομή και τις επικολλάτε στα κατάλληλα πεδία.

    Κατά την αντιγραφή του πιστοποιητικού αναγνωριστικό και διαχείρισης συνδρομής, **οι δεν** περιλαμβάνουν τις προσφορές που περικλείσετε τις τιμές.

1. Κάντε κλικ στη **Ρύθμιση παραμέτρων επαλήθευση**.

1. Όταν η ρύθμιση παραμέτρων έχει επαληθευτεί με επιτυχία, κάντε κλικ στην επιλογή **Αποθήκευση**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Ρύθμιση του προτύπου εικονική μηχανή για το Azure εξαρτώμενη προσθήκης

Ένα πρότυπο εικονική μηχανή καθορίζει τις παραμέτρους της προσθήκης θα χρησιμοποιήσετε για να δημιουργήσετε έναν κόμβο εξαρτώμενη στην Azure. Στα παρακάτω βήματα θα σας θα δημιουργήσετε προτύπου για μια Εικονική Ubuntu.

1. Στον πίνακα εργαλείων Hudson, κάντε κλικ στην επιλογή **Διαχείριση Hudson**.

1. Κάντε κλικ στη **Ρύθμιση παραμέτρων του συστήματος**.

1. Κάντε κύλιση προς τα κάτω στη σελίδα για να βρείτε την ενότητα **Cloud** .

1. Μέσα σε ενότητα **Cloud** , βρείτε **Προσθήκη πρότυπο εικονική μηχανή Azure** και κάντε κλικ στο κουμπί **Προσθήκη** .

    ![Προσθήκη εικονική προτύπου][add vm template]

1. Καθορίστε ένα όνομα υπηρεσία cloud στο πεδίο **όνομα** . Εάν το όνομα που καθορίζετε αναφέρεται σε μια υπάρχουσα υπηρεσία cloud, η Εικονική θα αποδοθεί σε αυτήν την υπηρεσία. Διαφορετικά, Azure θα δημιουργήσετε ένα νέο.

1. Στο πεδίο **Περιγραφή** , πληκτρολογήστε κείμενο που περιγράφει το πρότυπο που δημιουργείτε. Αυτές οι πληροφορίες είναι μόνο για σκοπούς εγγράφων και δεν χρησιμοποιείται στο προμήθεια μια Εικονική.

1. Στο πεδίο " **ετικέτες** ", πληκτρολογήστε **linux**. Αυτήν την ετικέτα χρησιμοποιείται για τον προσδιορισμό του προτύπου που θέλετε να δημιουργήσετε και στη συνέχεια χρησιμοποιούνται για την αναφορά στο πρότυπο κατά τη δημιουργία μιας εργασίας Hudson.

1. Επιλέξτε μια περιοχή όπου θα δημιουργηθεί η Εικονική.

1. Επιλέξτε το κατάλληλο μέγεθος Εικονική.

1. Καθορίστε ένα λογαριασμό χώρου αποθήκευσης όπου θα δημιουργηθεί η Εικονική. Βεβαιωθείτε ότι είναι στην ίδια περιοχή ως την υπηρεσία cloud που πρόκειται να χρησιμοποιήσετε. Εάν θέλετε το νέο χώρο αποθήκευσης που θα δημιουργηθεί, μπορείτε να αφήσετε αυτό το πεδίο κενό.

1. Χρόνο διατήρησης καθορίζει τον αριθμό των λεπτών πριν από την Hudson διαγράφει μια εξαρτώμενη αδράνειας. Αφήστε αυτήν την προεπιλεγμένη τιμή 60.

1. Στο πεδίο **Χρήση**, επιλέξτε το κατάλληλο συνθήκη όταν αυτός ο κόμβος εξαρτώμενη θα χρησιμοποιηθεί. Προς το παρόν, επιλέξτε **Utilize αυτόν τον κόμβο όσο είναι δυνατό**.

    Σε αυτό το σημείο, τη φόρμα σας θα μοιάζει κάπως με αυτό:

    ![ρύθμιση παραμέτρων προτύπου][template config]

1. Στην **οικογένεια εικόνα ή το αναγνωριστικό** πρέπει να καθορίσετε ποια εικόνα συστήματος θα εγκατασταθεί στον σας Εικονική. Μπορείτε να επιλέξετε από μια λίστα των οικογενειών εικόνα ή να ορίσετε μια προσαρμοσμένη εικόνα.

    Εάν θέλετε να επιλέξετε από μια λίστα των οικογενειών εικόνα, εισαγάγετε τον πρώτο χαρακτήρα (διάκριση πεζών-κεφαλαίων) από το όνομα της οικογένειας εικόνα. Για παράδειγμα, πληκτρολογώντας **U** θα εμφανιστεί μια λίστα με τις οικογένειες Ubuntu διακομιστή. Αφού επιλέξετε από τη λίστα, Jenkins θα χρησιμοποιήσει την πιο πρόσφατη έκδοση της εικόνας συστήματος από αυτήν την οικογένειά κατά την προμήθεια του Εικονική.

    ![Λίστα οικογενειακών λειτουργικό σύστημα][OS family list]

    Εάν έχετε μια προσαρμοσμένη εικόνα που θέλετε να χρησιμοποιήσετε αντί για αυτό, πληκτρολογήστε το όνομα της προσαρμοσμένης εικόνας. Εικόνα προσαρμοσμένα ονόματα δεν εμφανίζονται σε μια λίστα, ώστε να έχετε για να βεβαιωθείτε ότι το όνομα έχει πληκτρολογηθεί σωστά.    

    Σε αυτό το πρόγραμμα εκμάθησης, πληκτρολογήστε **Α** για να εμφανιστεί μια λίστα με τις εικόνες Ubuntu και επιλέξτε **Ubuntu διακομιστή 14.04 Αποτελεσμάτων**.

1. **Εκκινήστε τη μέθοδο**, επιλέξτε **SSH**.

1. Αντιγράψτε την παρακάτω δέσμη ενεργειών και επικολλήστε στο πεδίο **Προετοιμασία δέσμης ενεργειών** .

        # Install Java

        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk

        # Install git

        sudo apt-get install -y git

        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    Αφού δημιουργηθεί η Εικονική θα εκτελεστεί η **Προετοιμασία δέσμης ενεργειών** . Σε αυτό το παράδειγμα, η δέσμη ενεργειών εγκαθιστά Java, git και ant.

1. Στα πεδία **όνομα χρήστη** και **τον κωδικό πρόσβασης** , πληκτρολογήστε την προτιμώμενη τιμές για το λογαριασμό διαχειριστή που θα δημιουργηθούν στον σας Εικονική.

1. Κάντε κλικ στο **Πρότυπο επαλήθευση** για να ελέγξετε εάν οι παράμετροι που καθορίσατε δεν είναι έγκυρη.

1. Κάντε κλικ στην εντολή **Αποθήκευση**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Δημιουργία μιας εργασίας Hudson που εκτελείται σε έναν κόμβο εξαρτώμενη σε Azure

Σε αυτήν την ενότητα, θα δημιουργήσετε μια εργασία Hudson που θα εκτελείται σε έναν κόμβο εξαρτώμενη σε Azure.

1. Στον πίνακα εργαλείων Hudson, κάντε κλικ στην επιλογή **Νέα εργασία**.

1. Πληκτρολογήστε ένα όνομα για το έργο που δημιουργείτε.

1. Για τον τύπο εργασίας, επιλέξτε **Δημιουργία μιας εργασίας λογισμικού στυλ**.

1. Κάντε κλικ στο **κουμπί OK**.

1. Στη σελίδα Ρύθμιση παραμέτρων εργασίας, επιλέξτε **περιορισμένης όπου μπορεί να εκτελεστεί αυτό το έργο**.

1. Επιλέξτε **κόμβο και ετικέτα μενού** και επιλέξτε **linux** (μας που καθορίζεται η ετικέτα κατά τη δημιουργία του προτύπου εικονική μηχανή στην προηγούμενη ενότητα).

1. Στην ενότητα **Δημιουργία** , κάντε κλικ στην επιλογή **Προσθήκη Δόμηση βήμα** και επιλέξτε **Εκτέλεση κελύφους**.

1. Επεξεργαστείτε την ακόλουθη δέσμη ενεργειών, αντικαθιστώντας **{github όνομα του λογαριασμού σας}**, **{όνομα του έργου σας}**και **{κατάλογό σας έργο}** με κατάλληλες τιμές, και επικολλήστε την επεξεργασμένη δέσμη ενεργειών στην περιοχή κειμένου που εμφανίζεται.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e {your project directory} ]; then

            cd {your project directory}

            git pull origin master

        else

            git clone https://github.com/{your github account name}/{your project name}.git

        fi

        # change directory to project

        cd $currentDir/{your project directory}

        #Execute build task

        ant

1. Κάντε κλικ στην εντολή **Αποθήκευση**.

1. Στον πίνακα εργαλείων Hudson, βρείτε την εργασία που μόλις δημιουργήσατε και κάντε κλικ στο εικονίδιο του **χρονοδιαγράμματος μια Δόμηση** .

Hudson, στη συνέχεια, θα δημιουργήσετε έναν κόμβο εξαρτώμενη χρησιμοποιώντας το πρότυπο που δημιουργήσατε στην προηγούμενη ενότητα και να εκτελέσετε τη δέσμη ενεργειών που καθορίσατε στο βήμα Δόμηση για αυτήν την εργασία.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με τη χρήση Azure με Java, ανατρέξτε στο [Κέντρο για προγραμματιστές του Azure Java].

<!-- URL List -->

[Κέντρο για προγραμματιστές του Azure Java]: https://azure.microsoft.com/develop/java/
[συνδρομή προφίλ]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

