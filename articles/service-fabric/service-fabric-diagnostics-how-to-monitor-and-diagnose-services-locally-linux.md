<properties
   pageTitle="Τοπικά εποπτείας και διάγνωση υπηρεσίες γραμμένο με Azure Service ύφασμα | Microsoft Azure"
   description="Μάθετε πώς να παρακολουθείτε και διάγνωση δημιουργηθεί με τη χρήση του Microsoft Azure Service ύφασμα σε υπολογιστή τοπικής ανάπτυξης των υπηρεσιών σας."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Παρακολούθηση και διάγνωση υπηρεσίες σε μια ρύθμιση ανάπτυξης τοπικό υπολογιστή


> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

Παρακολούθηση, τον εντοπισμό, τη διάγνωση και αντιμετώπιση προβλημάτων επιτρέπουν για τις υπηρεσίες για να συνεχίσετε με ελάχιστες διακοπές για την εμπειρία χρήστη. Παρακολούθηση και τα Διαγνωστικά είναι κρίσιμες σε ένα περιβάλλον πραγματική ανεπτυγμένος παραγωγής. Έγκριση μοντέλου παρόμοια κατά την ανάπτυξη των υπηρεσιών εξασφαλίζει ότι η διαγνωστικών διοχέτευση λειτουργεί όταν μετακινείτε σε ένα περιβάλλον παραγωγής. Υπηρεσία ύφασμα διευκολύνει για τους προγραμματιστές υπηρεσία για την υλοποίηση διαγνωστικά που μπορεί να λειτουργεί απρόσκοπτα κατά μήκος των ρυθμίσεων τοπικής ανάπτυξης ενός υπολογιστή και των ρυθμίσεων συμπλέγματος ρεαλιστικό παραγωγής.


## <a name="debugging-service-fabric-java-applications"></a>Εντοπισμός σφαλμάτων εφαρμογών υπηρεσίας ύφασμα Java

Για εφαρμογές Java, [πολλών πλαισίων καταγραφής](http://en.wikipedia.org/wiki/Java_logging_framework) είναι διαθέσιμες. Επειδή το `java.util.logging` είναι η προεπιλεγμένη επιλογή με το JRE, αυτό χρησιμοποιείται επίσης για τα [παραδείγματα κώδικα στο github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  Το παρακάτω συζήτησης εξηγεί πώς μπορείτε να ρυθμίσετε τις παραμέτρους του `java.util.logging` framework. 
 
Χρήση java.util.logging μπορείτε να ανακατευθύνετε τις αρχεία καταγραφής εφαρμογών σε μνήμη, ροές εξόδου, αρχεία κονσόλας ή sockets. Για κάθε μία από αυτές τις επιλογές, υπάρχουν προγράμματα χειρισμού προεπιλεγμένη δώσει ήδη στο πλαίσιο. Μπορείτε να δημιουργήσετε ένα `app.properties` αρχείο για να ρυθμίσετε το σύστημα διαχείρισης αρχείων για την εφαρμογή σας για να ανακατευθύνετε όλα τα αρχεία καταγραφής σε ένα τοπικό αρχείο. 

Το παρακάτω τμήμα κώδικα περιέχει ένα παράδειγμα ρύθμισης παραμέτρων: 

```java 
handlers = java.util.logging.FileHandler
 
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

Ο φάκελος υποδεικνύεται από το `app.properties` αρχείο πρέπει να υπάρχει. Μετά την `app.properties` δημιουργείται, πρέπει να τροποποιήσετε επίσης δέσμη ενεργειών σημείο καταχώρησης, `entrypoint.sh` στο το `<applicationfolder>/<servicePkg>/Code/` φάκελο για να ορίσετε την ιδιότητα `java.util.logging.config.file` να `app.propertes` αρχείο. Η εγγραφή θα πρέπει να μοιάζει με το παρακάτω τμήμα κώδικα:

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```
 
 
Αυτή η ρύθμιση παραμέτρων τα αποτελέσματα στα αρχεία καταγραφής που συλλέγονται με περιστροφή τρόπο στο `/tmp/servicefabric/logs/`. Η **%u** και **%g** επιτρέπουν για τη δημιουργία περισσότερων αρχείων, με τα ονόματα αρχείων mysfapp0.log, mysfapp1.log και ούτω καθεξής. Από προεπιλογή Εάν δεν υπάρχει πρόγραμμα χειρισμού ρητά έχει ρυθμιστεί, το πρόγραμμα χειρισμού κονσόλας έχει καταχωρηθεί. Μία να προβάλετε τα αρχεία καταγραφής σε syslog στην περιοχή /var/log/syslog.
 
Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα τα [παραδείγματα κώδικα στο github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  



## <a name="next-steps"></a>Επόμενα βήματα
Τον ίδιο κωδικό ανίχνευση που προσθέσατε στην εφαρμογή σας λειτουργεί επίσης με τα Διαγνωστικά της εφαρμογής σας σε ένα σύμπλεγμα Azure. Δείτε αυτά τα άρθρα που περιγράφουν τις διάφορες επιλογές για τα εργαλεία και περιγράφουν πώς μπορείτε να ρυθμίσετε τους.
* [Πώς να συλλέξετε αρχεία καταγραφής με Διαγνωστικά του Azure](service-fabric-diagnostics-how-to-setup-lad.md)
