<properties
   pageTitle="Δημιουργήστε την πρώτη εφαρμογή υπηρεσίας ύφασμα στην Linux χρησιμοποιώντας C# | Microsoft Azure"
   description="Δημιουργία και ανάπτυξη μιας εφαρμογής υπηρεσίας ύφασμα χρησιμοποιώντας C#"
   services="service-fabric"
   documentationCenter="csharp"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="csharp"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="subramar"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Δημιουργήστε την πρώτη εφαρμογή ύφασμα υπηρεσιών Azure

> [AZURE.SELECTOR]
- [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Υπηρεσία ύφασμα παρέχει SDK για τη δημιουργία υπηρεσιών σε Linux στο .NET πυρήνα και Java. Σε αυτό το πρόγραμμα εκμάθησης, δείτε πώς μπορείτε να δημιουργήσετε μια εφαρμογή για Linux και να δημιουργήσετε μια υπηρεσία χρησιμοποιώντας C# (.NET πυρήνα).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Πριν ξεκινήσετε, βεβαιωθείτε ότι έχετε [Ρύθμιση του περιβάλλοντος ανάπτυξης του Linux](service-fabric-get-started-linux.md). Εάν χρησιμοποιείτε Mac OS X, μπορείτε να [ρυθμίσετε ένα περιβάλλον ένα πλαίσιο Linux σε μια εικονική μηχανή χρησιμοποιώντας Vagrant](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Δημιουργία της εφαρμογής

Μια εφαρμογή υπηρεσίας ύφασμα μπορεί να περιέχει μία ή περισσότερες υπηρεσίες, κάθε μία με ένα συγκεκριμένο ρόλο στην παράδοση τη λειτουργικότητα της εφαρμογής. Η υπηρεσία SDK ύφασμα για Linux περιλαμβάνει μια γεννήτρια [Yeoman](http://yeoman.io/) που διευκολύνει τη για να δημιουργήσετε την πρώτη υπηρεσία και να προσθέσετε περισσότερους αργότερα. Ας χρησιμοποιήσουμε Yeoman για να δημιουργήσετε μια εφαρμογή με μία μόνο υπηρεσία.

1. Σε μια terminal, πληκτρολογήστε την παρακάτω εντολή για να ξεκινήσετε τη δημιουργία του ικριώματος:`yo azuresfcsharp`

2. Δώστε όνομα στην εφαρμογή σας.

3. Επιλέξτε τον τύπο της πρώτης υπηρεσία σας και ονομάστε το. Για τους σκοπούς αυτής της εκμάθησης, θα σας, επιλέξτε μια αξιόπιστη υπηρεσία παράγοντα.

  ![Γεννήτρια Yeoman ύφασμα υπηρεσίας για C#][sf-yeoman]

>[AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με τις επιλογές, ανατρέξτε στο θέμα [Επισκόπηση προγραμματισμού μοντέλο ύφασμα υπηρεσίας](service-fabric-choose-framework.md).

## <a name="build-the-application"></a>Δημιουργία της εφαρμογής

Τα πρότυπα Yeoman ύφασμα υπηρεσίας περιλαμβάνουν μια δέσμη ενεργειών δόμησης που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε την εφαρμογή από το terminal (μετά την περιήγηση στο φάκελο της εφαρμογής).

  ```bash
 cd myapp 
 ./build.sh 
  ```

## <a name="deploy-the-application"></a>Ανάπτυξη της εφαρμογής

Όταν δημιουργηθεί η εφαρμογή, μπορείτε να το αναπτύξετε στο τοπικό σύμπλεγμα χρησιμοποιώντας το Azure CLI.

1. Συνδεθείτε με το τοπικό σύμπλεγμα ύφασμα υπηρεσίας.

    ```bash
    azure servicefabric cluster connect
    ```

2. Χρησιμοποιήστε τη δέσμη ενεργειών εγκατάσταση που παρέχονται στο πρότυπο για να αντιγράψετε το πακέτο εφαρμογών για το σύμπλεγμα εικόνα αποθήκευσης, καταχωρήστε τον τύπο εφαρμογής και τη δημιουργία μιας παρουσίας της εφαρμογής.

    ```bash
    ./install.sh
    ```

3. Ανοίξτε ένα πρόγραμμα περιήγησης και μεταβείτε σε υπηρεσία ύφασμα Explorer στο http://localhost:19080/Explorer (αντικατάσταση τοπικού κεντρικού υπολογιστή με το ιδιωτικό IP του η Εικονική εάν χρησιμοποιώντας Vagrant στο Mac OS X).

4. Αναπτύξτε τον κόμβο εφαρμογές και Σημειώστε ότι δεν υπάρχει πλέον μια καταχώρηση για τον τύπο εφαρμογής και ένα άλλο για την πρώτη παρουσία αυτού του τύπου.

## <a name="start-the-test-client-and-perform-a-failover"></a>Ξεκινήστε το πρόγραμμα-πελάτη δοκιμής και εκτελέστε ανακατεύθυνσης

Έργα παράγοντα δεν κάνετε τίποτα μόνος του. Απαιτείται άλλη υπηρεσία ή πρόγραμμα-πελάτη για να στείλετε τα μηνύματα. Το πρότυπο παράγοντα περιλαμβάνει μια απλή δοκιμαστική δέσμη ενεργειών που μπορείτε να χρησιμοποιήσετε για να αλληλεπιδράσετε με την υπηρεσία παράγοντα.

1. Εκτελέστε τη δέσμη ενεργειών χρησιμοποιώντας το βοηθητικό πρόγραμμα παρακολούθησης για να δείτε το αποτέλεσμα της υπηρεσίας παράγοντα.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Στην Εξερεύνηση ύφασμα υπηρεσίας, εντοπίστε κόμβο φιλοξενίας πρωτεύον αντίγραφο για την υπηρεσία παράγοντα. Στο παρακάτω στιγμιότυπο, είναι κόμβο 3.

    ![Μπορείτε να βρείτε την κύρια ρεπλίκα στην Εξερεύνηση ύφασμα υπηρεσίας][sfx-primary]

3. Κάντε κλικ στον κόμβο που βρέθηκε στο προηγούμενο βήμα και, στη συνέχεια, επιλέξτε **Απενεργοποίηση (επανεκκίνηση)** από το μενού Ενέργειες. Αυτή η ενέργεια επανεκκίνηση του έναν από τους κόμβους πέντε στο τοπικό σύμπλεγμά σας να επιβάλετε ανακατεύθυνσης σε μια δευτερεύουσα ρεπλίκα σε έναν άλλο κόμβο. Κατά την εκτέλεση αυτής της ενέργειας, προσέξτε στην έξοδο από τη δοκιμή προγράμματος-πελάτη και Σημειώστε ότι ο μετρητής εξακολουθεί να αυξήσετε παρά την ανακατεύθυνση.


## <a name="next-steps"></a>Επόμενα βήματα

- [Μάθετε περισσότερα σχετικά με την αξιόπιστη ηθοποιών](service-fabric-reliable-actors-introduction.md)
- [Αλληλεπίδραση με ύφασμα υπηρεσία συμπλεγμάτων χρησιμοποιώντας το CLI Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png