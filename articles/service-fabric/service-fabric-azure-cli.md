<properties
   pageTitle="Αλληλεπίδραση με ύφασμα υπηρεσία συμπλεγμάτων χρησιμοποιώντας CLI | Microsoft Azure"
   description="Πώς μπορείτε να χρησιμοποιήσετε Azure CLI για αλληλεπίδραση με ένα σύμπλεγμα ύφασμα υπηρεσίας"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Χρήση του Azure CLI για αλληλεπίδραση με ένα σύμπλεγμα ύφασμα υπηρεσίας

Μπορείτε να αλληλεπιδράσετε με σύμπλεγμα ύφασμα υπηρεσίας από μηχανές Linux χρησιμοποιώντας το Azure CLI στο Linux.

Το πρώτο βήμα είναι γρήγορα την πιο πρόσφατη έκδοση του CLI από το rep git και ρύθμιση στη διαδρομή σας χρησιμοποιώντας τις παρακάτω εντολές:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Για κάθε εντολή, υποστηρίζει, μπορείτε να πληκτρολογήσετε το όνομα της εντολής για να λάβετε Βοήθεια για αυτήν την εντολή. Αυτόματη συμπλήρωση υποστηρίζεται για τις εντολές. Για παράδειγμα, την παρακάτω εντολή παρέχει βοήθεια για όλες τις εντολές της εφαρμογής. 

```sh
 azure servicefabric application 
```

Επιπλέον, μπορείτε να φιλτράρετε τη Βοήθεια για μια συγκεκριμένη εντολή, όπως φαίνεται στο παρακάτω παράδειγμα:

```sh
 azure servicefabric application  create
```

Για να ενεργοποιήσετε την αυτόματη καταχώρηση στο το CLI, εκτελέστε τις ακόλουθες εντολές:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Τις παρακάτω εντολές, συνδεθείτε με το σύμπλεγμα και εμφάνιση τους κόμβους του συμπλέγματος:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Για να χρησιμοποιήσετε ονομαστικές παραμέτρους και βρείτε τι είναι, μπορείτε να πληκτρολογήσετε--Βοηθήστε μετά από μια εντολή. Για παράδειγμα:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Κατά τη σύνδεση σε ένα σύμπλεγμα πολλαπλών παραγωγής από ένα μηχάνημα **αυτό σημαίνει ότι δεν είναι μέλος του συμπλέγματος**, χρησιμοποιήστε την ακόλουθη εντολή:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Αντικαταστήστε την ετικέτα PublicIPorFQDN με το πραγματικό IP ή το FQDN ανάλογα με την περίπτωση. Κατά τη σύνδεση σε ένα σύμπλεγμα πολλαπλών παραγωγής από έναν υπολογιστή **που αποτελεί μέρος του συμπλέγματος**, χρησιμοποιήστε την ακόλουθη εντολή:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Μπορείτε να χρησιμοποιήσετε το PowerShell ή CLI για να αλληλεπιδράσετε με το σύμπλεγμά Linux υπηρεσίας ύφασμα που δημιουργήθηκε μέσω της πύλης Azure. 

**Προσοχή:** Αυτών των συμπλεγμάτων δεν είναι ασφαλής, επομένως, που μπορεί να ανοίγματος των σας το ένα πλαίσιο, προσθέτοντας στη δημόσια διεύθυνση IP στο σύμπλεγμα δηλωτικό.



## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Χρήση του Azure CLI για σύνδεση με ένα σύμπλεγμα ύφασμα υπηρεσίας

Τις παρακάτω εντολές Azure CLI περιγράφουν τον τρόπο για να συνδεθείτε με μια ασφαλή σύμπλεγμα. Λεπτομέρειες του πιστοποιητικού πρέπει να συμφωνεί με ένα πιστοποιητικό σε τους κόμβους συμπλέγματος.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```
 
Εάν το πιστοποιητικό έχει τις αρχές έκδοσης πιστοποιητικών (CA), πρέπει να προσθέσετε την παράμετρο--ca-πιστοποιητικού-διαδρομή όπως το παρακάτω παράδειγμα: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Εάν έχετε πολλές αρχές έκδοσης πιστοποιητικών, χρησιμοποιήστε ένα κόμμα ως οριοθέτης.
 
Εάν το κοινό όνομα στο πιστοποιητικό δεν ταιριάζουν με το τελικό σημείο σύνδεσης, μπορείτε να χρησιμοποιήσετε την παράμετρο `--strict-ssl` να παρακάμψει το μήνυμα επαλήθευσης, όπως φαίνεται στην παρακάτω εντολή: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl false 
```
 
Εάν θέλετε να παραλείπεται η επαλήθευση CA, θα μπορούσατε να προσθέσετε--μη εξουσιοδοτημένη πρόσβαση απόρριψη παραμέτρου όπως φαίνεται στην παρακάτω εντολή: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized false 
```
 
Μετά τη σύνδεση, θα πρέπει να μπορείτε να εκτελέσετε άλλες CLI εντολές για να αλληλεπιδράσετε με το σύμπλεγμα. 

## <a name="deploying-your-service-fabric-application"></a>Ανάπτυξη εφαρμογής υπηρεσίας ύφασμα σας

Εκτελέστε τις ακόλουθες εντολές για να αντιγράψετε, καταχώρηση και ξεκινήστε την εφαρμογή ύφασμα υπηρεσίας:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Αναβάθμιση εφαρμογής σας

Η διαδικασία είναι παρόμοια με τη [διαδικασία στα Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Δημιουργία, αντιγραφή, καταχώρηση και δημιουργήστε την εφαρμογή από το ριζικό κατάλογο του έργου. Εάν σας παρουσία της εφαρμογής ονομάζεται ύφασμα: / MySFApp και ο τύπος είναι MySFApp, οι εντολές θα είναι ως εξής:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Κάντε τις αλλαγές στην εφαρμογή σας και εκ νέου δημιουργία της υπηρεσίας που έχουν τροποποιηθεί.  Ενημέρωση της υπηρεσίας έχουν τροποποιηθεί αρχείο δηλώσεων (ServiceManifest.xml) με τις ενημερωμένες εκδόσεις για την υπηρεσία (και κώδικα ή ρύθμισης παραμέτρων ή δεδομένα ανάλογα με την περίπτωση). Επίσης, τροποποιήσετε δηλωτικό της εφαρμογής (ApplicationManifest.xml) με τον αριθμό ενημερωμένη έκδοση για την εφαρμογή και την υπηρεσία που έχουν τροποποιηθεί.  Στη συνέχεια, αντιγράψτε και καταχώρηση την εφαρμογή ενημερωμένων σας χρησιμοποιώντας τις παρακάτω εντολές:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Τώρα, μπορείτε να ξεκινήσετε την αναβάθμιση εφαρμογής με την ακόλουθη εντολή:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

Τώρα, μπορείτε να παρακολουθείτε την αναβάθμιση εφαρμογών χρησιμοποιώντας SFX. Μετά από μερικά λεπτά, η εφαρμογή θα έχουν ενημερωθεί.  Επίσης, μπορείτε να δοκιμάσετε μια ενημερωμένη εφαρμογή με το μήνυμα σφάλματος και να ελέγξτε τη λειτουργικότητα αυτόματης επαναφοράς σε ύφασμα υπηρεσίας.

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

### <a name="copying-of-the-application-package-does-not-succeed"></a>Αντιγραφή του πακέτου εφαρμογών δεν ολοκληρωθεί με επιτυχία

Ελέγξτε αν `openssh` είναι εγκατεστημένο. Από προεπιλογή, Ubuntu επιφάνειας εργασίας δεν διαθέτει εγκατεστημένο. Εγκαταστήστε το, χρησιμοποιώντας την ακόλουθη εντολή:

```
 sudo apt-get install openssh-server openssh-client**
```

Εάν το πρόβλημα δεν επιλυθεί, δοκιμάστε να απενεργοποιήσετε PAM για ssh, αλλάζοντας το αρχείο **sshd_config** χρησιμοποιώντας τις παρακάτω εντολές:

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Εάν το πρόβλημα εξακολουθεί να παραμένει, δοκιμάστε να αυξήσετε τον αριθμό των ssh περιόδους λειτουργίας με την εκτέλεση τις παρακάτω εντολές:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Χρήση των πλήκτρων για ssh ελέγχου ταυτότητας (αντί για τους κωδικούς πρόσβασης) δεν είναι ακόμη υποστηρίζονται (από την πλατφόρμα χρησιμοποιεί ssh για να αντιγράψετε τα πακέτα), επομένως αντί για αυτό, χρησιμοποιήστε τον κωδικό πρόσβασης τον έλεγχο ταυτότητας.


## <a name="next-steps"></a>Επόμενα βήματα

Ρυθμίστε το περιβάλλον ανάπτυξης και ανάπτυξη μιας εφαρμογής υπηρεσίας ύφασμα για ένα σύμπλεγμα Linux.
