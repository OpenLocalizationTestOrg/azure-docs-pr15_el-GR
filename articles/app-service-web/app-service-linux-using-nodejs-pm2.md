<properties 
    pageTitle="Χρήση ΑΣ2 ρύθμισης παραμέτρων για NodeJS στο Web Apps σε Linux | Microsoft Azure" 
    description="Χρήση ΑΣ2 ρύθμισης παραμέτρων για NodeJS στο Web Apps σε Linux" 
    keywords="Azure εφαρμογής υπηρεσίας, εφαρμογή web, nodejs, ΑΣ2, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>Χρήση ΑΣ2 ρύθμισης παραμέτρων για Node.js στο Web Apps σε Linux

Εάν ορίσετε το στοίβα της εφαρμογής για να Node.js για τις εφαρμογές Web στην Linux, θα λάβετε την επιλογή για να ορίσετε ένα αρχείο εκκίνησης Node.js όπως φαίνεται στην παρακάτω εικόνα.

![][1]

Μπορείτε να το χρησιμοποιήσετε σε ένα

-   Καθορίστε τη δέσμη ενεργειών εκκίνησης για την εφαρμογή σας Node.js (για παράδειγμα: /bin/server.js)
-   Καθορίστε το αρχείο ρύθμισης παραμέτρων ΑΣ2 για να χρησιμοποιήσετε για την εφαρμογή σας Node.js (για παράδειγμα: /foo/process.json)

 >[AZURE.NOTE] Εάν θέλετε τις διεργασίες κόμβο σας για να επανεκκινήσετε αυτόματα όταν τροποποιούνται ορισμένα αρχεία, θα πρέπει να χρησιμοποιήσετε τη ρύθμιση παραμέτρων ΑΣ2. Διαφορετικά την εφαρμογή σας δεν θα επανεκκίνηση όταν λάβει ειδοποιήσεις αλλαγών από στοιχεία, όπως συνεχούς ανάπτυξης όταν αλλάζει κώδικα της εφαρμογής σας.

Μπορείτε να ελέγξετε την Node.js [διαδικασία αρχείο τεκμηρίωση](http://pm2.keymetrics.io/docs/usage/application-declaration/) για όλες τις επιλογές, αλλά κάτω από το στοιχείο είναι ένα δείγμα του τι μπορείτε να χρησιμοποιήσετε ως αρχείο process.json

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Είναι σημαντικά πράγματα που πρέπει να λάβετε υπόψη σε αυτήν τη ρύθμιση 

-   Η ιδιότητα "δέσμη ενεργειών" Καθορίζει δέσμης ενεργειών Έναρξη της εφαρμογής σας.
-   Η ιδιότητα "εμφανίσεις" Καθορίζει πόσες εμφανίσεις της διαδικασίας κόμβο εκκίνηση. Εάν χρησιμοποιείτε την εφαρμογή σας σχετικά με το μεγαλύτερο μέγεθος Εικονική που έχουν πολλών πυρήνων, που θέλετε να μεγιστοποιήσετε τους πόρους σας, ορίζοντας μια υψηλότερη τιμή εδώ.
-   Ο πίνακας "Παρακολούθηση" καθορίζει όλα τα αρχεία για την αλλαγή των οποίων θέλετε να επανεκκινήσετε τις διεργασίες κόμβο.
-   Για το "watch_options", προς το παρόν πρέπει να καθορίσετε "usePolling" ως true λόγω τον τρόπο που έχει τοποθετηθεί το περιεχόμενο της εφαρμογής.


## <a name="next-steps"></a>Επόμενα βήματα ##

* [Τι είναι η υπηρεσία εφαρμογής στην Linux;](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png