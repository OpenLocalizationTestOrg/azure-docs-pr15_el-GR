<properties
    pageTitle="Πώς μπορείτε να δημιουργήσετε και να αναπτύξετε μια υπηρεσία cloud | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε και να αναπτύξετε μια υπηρεσία στο cloud χρησιμοποιώντας τη μέθοδο γρήγορης δημιουργίας στο Azure."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Πώς μπορείτε να δημιουργήσετε και να αναπτύξετε μια υπηρεσία στο Cloud

> [AZURE.SELECTOR]
- [Πύλη του Azure](cloud-services-how-to-create-deploy-portal.md)
- [Azure κλασική πύλη](cloud-services-how-to-create-deploy.md)

Η πύλη του Azure κλασική παρέχει δύο τρόποι για να δημιουργήσετε και να αναπτύξετε μια υπηρεσία cloud: **Γρήγορης δημιουργίας** και **Δημιουργήστε προσαρμοσμένες**.

Αυτό το θέμα εξηγεί πώς μπορείτε να χρησιμοποιήσετε τη μέθοδο Γρήγορη δημιουργία για να δημιουργήσετε μια νέα υπηρεσία στο cloud και, στη συνέχεια, χρησιμοποιήστε **Αποστολή** για να αποστείλετε και να αναπτύξετε ένα πακέτο υπηρεσία cloud στο Azure. Όταν χρησιμοποιείτε αυτήν τη μέθοδο, Azure κλασική πύλη κάνει διαθέσιμη εύχρηστες συνδέσεις για την ολοκλήρωση όλων των απαιτήσεων καθώς εργάζεστε. Εάν είστε έτοιμοι να αναπτύξετε την υπηρεσία cloud, κατά τη δημιουργία της, μπορείτε να κάνετε και τα δύο την ίδια ώρα με χρήση **Προσαρμοσμένης δημιουργία**.

> [AZURE.NOTE] Εάν σκοπεύετε να δημοσιεύσετε την υπηρεσία cloud από το Visual Studio ομάδας υπηρεσιών (VSTS), χρησιμοποιήστε γρήγορης δημιουργίας και, στη συνέχεια, ρύθμιση VSTS δημοσίευσης από **Γρήγορης εκκίνησης** ή στον πίνακα εργαλείων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Συνεχής παράδοση Azure από χρησιμοποιώντας υπηρεσίες ομάδας Visual Studio][TFSTutorialForCloudService], ή ανατρέξτε στο θέμα Βοήθεια για τη σελίδα **Γρήγορης εκκίνησης** .

## <a name="concepts"></a>Έννοιες
Για την ανάπτυξη μιας εφαρμογής ως μια υπηρεσία στο cloud στο Azure είναι απαιτείται τρία στοιχεία:

- **Ορισμός υπηρεσίας**  
  Το αρχείο ορισμού υπηρεσία cloud (.csdef) ορίζει το μοντέλο υπηρεσίας, συμπεριλαμβανομένου του αριθμού των ρόλων.

- **Ρύθμιση παραμέτρων της υπηρεσίας**  
  Το αρχείο ρύθμισης παραμέτρων υπηρεσίας cloud (.cscfg) παρέχει ρυθμίσεις παραμέτρων για το cloud υπηρεσία και μεμονωμένες ρόλους, συμπεριλαμβανομένου του αριθμού των παρουσιών ρόλο.

- **Υπηρεσία πακέτου**  
  Το πακέτο υπηρεσίας (.cspkg) περιέχει τον κώδικα της εφαρμογής και ρυθμίσεις παραμέτρων και το αρχείο ορισμού υπηρεσίας.
  
Μπορείτε να μάθετε περισσότερα σχετικά με αυτές και πώς μπορείτε να δημιουργήσετε ένα πακέτο [εδώ](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Προετοιμασία της εφαρμογής σας
Για να αναπτύξετε μια υπηρεσία cloud, πρέπει να δημιουργήσετε το πακέτο υπηρεσία cloud (.cspkg) από κώδικα της εφαρμογής σας και ένα αρχείο ρύθμισης παραμέτρων υπηρεσίας cloud (.cscfg). Το SDK Azure παρέχει εργαλεία για την προετοιμασία αυτών των αρχείων απαιτείται ανάπτυξη. Μπορείτε να εγκαταστήσετε το SDK από τη σελίδα [Λήψεων Azure](https://azure.microsoft.com/downloads/) , στη γλώσσα στην οποία που προτιμάτε για την ανάπτυξη κώδικα της εφαρμογής σας.

Τρεις δυνατότητες υπηρεσίας cloud απαιτούν ειδική ρύθμιση παραμέτρων πριν από την εξαγωγή ενός πακέτου υπηρεσίας:

- Εάν θέλετε να αναπτύξετε μια υπηρεσία cloud που χρησιμοποιεί Secure Sockets Layer (SSL) για την κρυπτογράφηση δεδομένων, [Ρύθμιση παραμέτρων της εφαρμογής σας](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) για το SSL.

- Εάν θέλετε να ρυθμίσετε τις παραμέτρους συνδέσεις απομακρυσμένης επιφάνειας εργασίας σε παρουσίες των ρόλων, [Ρύθμιση παραμέτρων τους ρόλους](cloud-services-role-enable-remote-desktop.md) για σύνδεση απομακρυσμένης επιφάνειας εργασίας.

- Εάν θέλετε να ρυθμίσετε τις παραμέτρους λεπτομερές παρακολούθησης για την υπηρεσία cloud, ενεργοποίηση Διαγνωστικά του Azure για την υπηρεσία cloud. *Ελάχιστο παρακολούθησης* (η προεπιλογή παρακολούθηση επιπέδου) χρησιμοποιεί μετρητές επιδόσεων συγκεντρώσατε από τα λειτουργικά συστήματα κεντρικού υπολογιστή για τις παρουσίες ρόλο (εικονικές μηχανές). "Λεπτομερής παρακολούθηση * συγκεντρώνει πρόσθετων μετρήσεων που βασίζονται σε δεδομένα επιδόσεων κατά τις παρουσίες ρόλο για να ενεργοποιήσετε την πιο κοντινή ανάλυσης των θεμάτων που προκύπτουν κατά την επεξεργασία της εφαρμογής. Για να μάθετε πώς μπορείτε να ενεργοποιήσετε τα Διαγνωστικά Azure, ανατρέξτε στο θέμα [Ενεργοποίηση Διαγνωστικά στο Azure](cloud-services-dotnet-diagnostics.md).

Για να δημιουργήσετε μια υπηρεσία cloud με αναπτύξεις ρόλους web ή εργαζόμενου ρόλους, πρέπει να [δημιουργήσετε το πακέτο υπηρεσίας](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

- Εάν δεν έχετε εγκαταστήσει το SDK Azure, κάντε κλικ στην επιλογή **Εγκατάσταση SDK Azure** για να ανοίξετε τη [σελίδα λήψεων του Azure](https://azure.microsoft.com/downloads/)και, στη συνέχεια, κάντε λήψη του SDK για τη γλώσσα στην οποία που προτιμάτε για την ανάπτυξη του κώδικα. (Θα έχετε την ευκαιρία να κάνετε αυτό αργότερα.)

- Εάν όλες τις εμφανίσεις ρόλο απαιτούν ένα πιστοποιητικό, δημιουργήστε τα πιστοποιητικά. Υπηρεσίες cloud απαιτούν αρχείο .pfx με ένα ιδιωτικό κλειδί. Μπορείτε να [αποστείλετε τα πιστοποιητικά για να Azure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) που μπορείτε να δημιουργήσετε και να αναπτύξετε την υπηρεσία cloud.

- Εάν σκοπεύετε να αναπτύξετε την υπηρεσία cloud σε μια ομάδα συσχέτισης, δημιουργήστε την ομάδα συσχέτισης. Μπορείτε να χρησιμοποιήσετε μια ομάδα συσχέτισης για να αναπτύξετε την υπηρεσία cloud και άλλες υπηρεσίες του Azure στην ίδια θέση σε μια περιοχή. Μπορείτε να δημιουργήσετε την ομάδα συσχέτισης στην περιοχή **δίκτυα** του Azure κλασική πύλη, στη σελίδα **ομάδες συνάφεια** .


## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Πώς μπορείτε να: Δημιουργήστε μια υπηρεσία στο cloud χρησιμοποιώντας γρήγορης δημιουργίας

1. Στην [πύλη του Azure κλασική](http://manage.windowsazure.com/), κάντε κλικ στην επιλογή **Δημιουργία**>**τον υπολογισμό**>**Υπηρεσία Cloud**>**Γρήγορης δημιουργίας**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. Στη **διεύθυνση URL**, πληκτρολογήστε ένα όνομα δευτερεύοντος τομέα για να χρησιμοποιήσετε στη δημόσια διεύθυνση URL για πρόσβαση στην υπηρεσία cloud σε αναπτύξεις παραγωγής. Η μορφή διεύθυνσης URL για αναπτύξεις παραγωγής είναι: http://*myURL*. cloudapp.net.

3. Στην **περιοχή ή ομάδα συσχέτισης**, επιλέξτε το γεωγραφική περιοχή ή ομάδα συσχέτισης για να αναπτύξετε την υπηρεσία cloud για να. Εάν θέλετε να αναπτύξετε την υπηρεσία cloud στην ίδια θέση με άλλες υπηρεσίες του Azure μέσα σε μια περιοχή, επιλέξτε μια ομάδα συσχέτισης.

4. Κάντε κλικ στην επιλογή **Δημιουργία υπηρεσία στο Cloud**.

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    Μπορείτε να παρακολουθείτε την κατάσταση της διαδικασίας στην περιοχή του μηνύματος στο κάτω μέρος του παραθύρου.

    Στην περιοχή **Υπηρεσίες Cloud** ανοίγει, με τη νέα υπηρεσία cloud που εμφανίζεται. Όταν η κατάσταση αλλάξει σε δημιουργήθηκε, δημιουργία υπηρεσιών cloud ολοκληρώθηκε με επιτυχία.

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Πώς μπορείτε να: αποστείλετε ένα πιστοποιητικό για μια υπηρεσία στο cloud

1. Στην [πύλη του Azure κλασική](http://manage.windowsazure.com/), κάντε κλικ στην επιλογή **Υπηρεσίες Cloud**, κάντε κλικ στο όνομα της υπηρεσίας cloud και, στη συνέχεια, κάντε κλικ στην επιλογή **πιστοποιητικά**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)


2. Κάντε κλικ στην επιλογή είτε **αποστείλετε ένα πιστοποιητικό** ή **Αποστολή**.

3. Στο **αρχείο**, χρησιμοποιήστε **Αναζήτηση** για να επιλέξετε το πιστοποιητικό (αρχείο .pfx).

4. Στο πλαίσιο **κωδικός πρόσβασης**, πληκτρολογήστε το ιδιωτικό κλειδί για το πιστοποιητικό.

5. Κάντε κλικ στο **κουμπί OK** (σημάδι ελέγχου).

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    Μπορείτε να παρακολουθήσετε την πρόοδο της αποστολής στην περιοχή του μηνύματος, φαίνεται παρακάτω. Όταν ολοκληρωθεί η αποστολή, το πιστοποιητικό προστίθεται στον πίνακα. Στην περιοχή του μηνύματος, κάντε κλικ στο κουμπί OK για να κλείσετε το μήνυμα.

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Πώς μπορείτε να: ανάπτυξη μια υπηρεσία στο cloud

1. Στην [πύλη του Azure κλασική](http://manage.windowsazure.com/), κάντε κλικ στην επιλογή **Υπηρεσίες Cloud**, κάντε κλικ στο όνομα της υπηρεσίας cloud και, στη συνέχεια, κάντε κλικ στην επιλογή **πίνακα εργαλείων**.

2. Επιλέξτε **Αποστολή μιας νέας ανάπτυξης παραγωγής** ή **Αποστολή**.

3. Στην **Ανάπτυξη ετικέτας**, πληκτρολογήστε ένα όνομα για το νέο ανάπτυξη - για παράδειγμα, MyCloudServicev4.

3. Στο **πακέτο**, χρησιμοποιήστε **Αναζήτηση** για να επιλέξετε το αρχείο πακέτου υπηρεσίας (.cspkg) για να χρησιμοποιήσετε.

4. Με τη **ρύθμιση των παραμέτρων**, χρησιμοποιήστε **Αναζήτηση** για να επιλέξετε το αρχείο ρύθμιση παραμέτρων της υπηρεσίας (.cscfg) για να χρησιμοποιήσετε.

5. Εάν η υπηρεσία cloud θα περιλαμβάνει όλους τους ρόλους με μία μόνο εμφάνιση, επιλέξτε το πλαίσιο ελέγχου **Ανάπτυξη ακόμα και εάν μία ή περισσότερες ρόλοι περιέχουν μία παρουσία** για να ενεργοποιήσετε την ανάπτυξη για να συνεχίσετε.

    Azure μόνο να εγγυάται τοις εκατό 99.95 πρόσβασης στην υπηρεσία cloud κατά την εφαρμογή ενημερώσεων συντήρησης και της υπηρεσίας εάν κάθε ρόλο έχει τουλάχιστον δύο παρουσίες. Εάν είναι απαραίτητο, μπορείτε να προσθέσετε επιπλέον ρόλο παρουσίες στη σελίδα " **κλίμακα** " μετά την ανάπτυξη της υπηρεσίας cloud. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τις συμβάσεις επιπέδου υπηρεσιών](https://azure.microsoft.com/support/legal/sla/).

6. Κάντε κλικ στο **κουμπί OK** (σημάδι ελέγχου) για να ξεκινήσετε την ανάπτυξη υπηρεσία cloud.

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    Μπορείτε να παρακολουθείτε την κατάσταση της ανάπτυξης στην περιοχή του μηνύματος. Κάντε κλικ στο κουμπί OK για να αποκρύψετε το μήνυμα.

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Επαληθεύστε την ανάπτυξη ολοκληρώθηκε με επιτυχία

1. Κάντε κλικ στην επιλογή **πίνακα εργαλείων**.

    Η κατάσταση θα πρέπει να εμφανίζουν ότι η υπηρεσία είναι **εκτελείται**.

2. Στην περιοχή **γρήγορη ματιά**, επιλέξτε τη διεύθυνση URL της τοποθεσίας για να ανοίξετε την υπηρεσία cloud σε ένα πρόγραμμα περιήγησης web.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


[TFSTutorialForCloudService]: cloud-services-continuous-delivery-use-vso.md
 
## <a name="next-steps"></a>Επόμενα βήματα

* [Γενικές ρύθμιση παραμέτρων για την υπηρεσία cloud](cloud-services-how-to-configure.md).
* Ρύθμιση παραμέτρων ένα [προσαρμοσμένο όνομα τομέα](cloud-services-custom-domain-name.md).
* [Διαχείριση της υπηρεσίας cloud](cloud-services-how-to-manage.md).
* Ρύθμιση παραμέτρων [πιστοποιητικά ssl](cloud-services-configure-ssl-certificate.md).