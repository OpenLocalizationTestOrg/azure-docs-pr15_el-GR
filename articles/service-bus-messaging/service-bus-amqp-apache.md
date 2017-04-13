<properties 
    pageTitle="Πώς να εγκαταστήσετε Apache Qpid Proton-C σε μια Εικονική Linux | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια Εικονική Linux CentOS χρησιμοποιώντας εικονικές μηχανές Windows Azure και πώς μπορείτε να δημιουργήσετε και να εγκαταστήσετε τη βιβλιοθήκη Apache Qpid Proton-C."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="install-apache-qpid-proton-c-on-an-azure-linux-vm"></a>Εγκατάσταση Apache Qpid Proton-C σε μια Εικονική Linux Azure

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Αυτή η ενότητα δείχνει πώς μπορείτε να δημιουργήσετε μια Εικονική Linux CentOS χρησιμοποιώντας εικονικές μηχανές Windows Azure και πώς μπορείτε να κάνετε λήψη, δημιουργία και εγκαταστήστε τη βιβλιοθήκη Apache Qpid Proton-C μαζί με τις συνδέσεις γλώσσας Python και PHP. Αφού ολοκληρώσετε αυτά τα βήματα, θα μπορέσετε να εκτελέσετε τα δείγματα Python και PHP περιλαμβάνεται με αυτόν τον οδηγό.

Το πρώτο βήμα πραγματοποιείται με την [πύλη του Azure κλασική][]. Το παρακάτω στιγμιότυπο οθόνης εμφανίζει πώς δημιουργείται μια Εικονική CentOS με το όνομα "scott-centos":

![Proton σε μια Εικονική Linux Azure][0]

Μετά την προμήθεια, την πύλη Εμφανίζει τα εξής:

![Proton σε μια Εικονική Linux Azure][1]

Για να συνδεθείτε με τον υπολογιστή, πρέπει να γνωρίζετε τη θύρα τελικό σημείο για SSH. Μπορείτε να αποκτήσετε αυτήν την τιμή από την [πύλη του Azure κλασική][] επιλέγοντας το πρόσφατα δημιουργημένο Εικονική και κάνοντας κλικ στην καρτέλα **τελικά σημεία** . Το παρακάτω στιγμιότυπο οθόνης δείχνει ότι η δημόσια θύρα SSH για αυτόν τον υπολογιστή είναι 57146.

![Proton σε μια Εικονική Linux Azure][2]

Τώρα μπορείτε να συνδεθείτε με τον υπολογιστή χρησιμοποιώντας SSH. Αυτό το παράδειγμα χρησιμοποιεί το εργαλείο PuTTY, όπως το παρακάτω στιγμιότυπο οθόνης:

![Proton σε μια Εικονική Linux Azure][3]

Για τις εφαρμογές Python και PHP, αυτό το παράδειγμα χρησιμοποιεί τις βιβλιοθήκες Proton προγράμματος-πελάτη από Apache. Αυτές οι βιβλιοθήκες είναι διαθέσιμο για λήψη από [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Το αρχείο Readme στο πακέτο διανομής εξηγεί τα βήματα που απαιτούνται για να εγκαταστήσετε τις εξαρτήσεις και να δομήσετε Proton. Ακολουθεί μια σύνοψη των βημάτων:

1.  Επεξεργαστείτε το αρχείο ρύθμισης παραμέτρων yum (/ etc/yum.conf) και σχολιάζετε τον αποκλεισμό για ενημερώσεις σε κεφαλίδες πυρήνα (\# εξαίρεση = πυρήνα\*). Αυτό είναι απαραίτητο για να εγκαταστήσετε το πρόγραμμα μεταγλώττισης πρόγραμμα μεταγλώττισης gcc.

2.  Εγκαταστήστε τα προαπαιτούμενα πακέτα:

    ```
    # required dependencies 
    yum install gcc cmake libuuid-devel
    
    # dependencies needed for ssl support
    yum install openssl-devel
    
    # dependencies needed for bindings
    yum install swig python-devel ruby-devel php-devel java-1.6.0-openjdk
    
    # dependencies needed for python docs
    yum install epydoc
    ```

1.  Κάντε λήψη της βιβλιοθήκης Proton:

    ```
    [azureuser@this-user ~]$ wget http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    --2016-04-17 14:45:03--  http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    Resolving apache.panu.it (apache.panu.it)... 81.208.22.71
    Connecting to apache.panu.it (apache.panu.it)|81.208.22.71|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 868226 (848K) [application/x-gzip]
    Saving to: ‘qpid-proton-0.9.tar.gz’
    
    qpid-proton-0.9.tar.gz                               
    
    100%[====================================================================================================================>] 847.88K   102KB/s    in 8.4s    
    
    2016-04-17 14:45:12 (101 KB/s) - ‘qpid-proton-0.9.tar.gz’ saved [868226/868226]
    ```

1.  Εξαγάγετε τον κωδικό Proton από την αρχειοθέτηση διανομής:

    ```
    tar xvfz qpid-proton-0.9.tar.gz
    ```

1.  Δημιουργία και εγκαταστήστε τον κώδικα ακολουθώντας τα παρακάτω βήματα, που λαμβάνονται από το αρχείο Readme:

    ```
    From the directory where you found this README file:    
    
    mkdir build cd build
            
    # Set the install prefix. You may need to adjust depending on your      
    # system.       
    cmake -DCMAKE\_INSTALL\_PREFIX=/usr ..
            
    # Omit the docs target if you do not wish to build or install       
    # documentation.        
    make all docs
            
    # Note that this step will require root privileges.     
    make install
    ```

Αφού εκτελέσετε αυτά τα βήματα, Proton είναι εγκατεστημένο στον υπολογιστή και είστε έτοιμοι για χρήση.

## <a name="next-steps"></a>Επόμενα βήματα

Είστε έτοιμοι για να μάθετε περισσότερα; Επισκεφθείτε την ακόλουθη σύνδεση:

- [Επισκόπηση AMQP Bus υπηρεσίας][]

[Επισκόπηση AMQP Bus υπηρεσίας]: service-bus-amqp-overview.md
[0]: ./media/service-bus-amqp-apache/amqp-apache-1.png
[1]: ./media/service-bus-amqp-apache/amqp-apache-2.png
[2]: ./media/service-bus-amqp-apache/amqp-apache-3.png
[3]: ./media/service-bus-amqp-apache/amqp-apache-4.png

[Azure κλασική πύλη]: http://manage.windowsazure.com


