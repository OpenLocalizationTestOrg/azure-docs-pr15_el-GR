<properties
   pageTitle="Σύνδεση σε ένα σύμπλεγμα Azure κοντέινερ υπηρεσίας | Microsoft Azure"
   description="Συνδεθείτε σε ένα σύμπλεγμα Azure κοντέινερ υπηρεσίας, χρησιμοποιώντας ένα σωλήνα SSH."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, κοντέινερ, μικρής κλίμακας-υπηρεσίες, ελεγκτή Τομέα/λειτουργικό σύστημα, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>


# <a name="connect-to-an-azure-container-service-cluster"></a>Σύνδεση σε ένα σύμπλεγμα Azure κοντέινερ υπηρεσίας

Το Κέντρο Δεδομένων/OS και συμπλεγμάτων Docker Swarm που έχουν αναπτυχθεί από την υπηρεσία κοντέινερ Azure εκθέτουν ΥΠΌΛΟΙΠΑ τελικά σημεία. Ωστόσο, αυτά τα τελικά σημεία δεν είναι ανοικτό στον εξωτερικό κόσμο. Για να διαχειριστείτε αυτά τα τελικά σημεία, πρέπει να δημιουργήσετε μια διοχέτευση ασφαλούς κελύφους (SSH). Μετά από μια SSH διοχέτευσης έχει εφαρμοστεί, μπορείτε να εκτελούν εντολές του σε σχέση με τα τελικά σημεία σύμπλεγμα και να προβάλετε το σύμπλεγμα περιβάλλοντος εργασίας Χρήστη μέσω ενός προγράμματος περιήγησης στο δικό σας σύστημα. Αυτό το έγγραφο σάς καθοδηγεί στη δημιουργία διοχέτευση SSH από Linux, OS X και Windows.

>[AZURE.NOTE] Μπορείτε να δημιουργήσετε μια περίοδο λειτουργίας SSH με ενός συστήματος διαχείρισης συμπλέγματος. Ωστόσο, δεν συνιστάται. Εργασία απευθείας σε ένα σύστημα διαχείρισης εκθέτει ο κίνδυνος για αλλαγές στη ρύθμιση παραμέτρων ακούσια.   

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Δημιουργία διοχέτευση SSH σε Linux ή OS X

Το πρώτο πράγμα που μπορείτε να κάνετε όταν δημιουργείτε μια διοχέτευση SSH στο Linux ή OS X είναι για να εντοπίσετε το δημόσιο όνομα DNS εξισορρόπηση φόρτου υποδειγμάτων. Για να το κάνετε αυτό, αναπτύξτε την ομάδα των πόρων, έτσι ώστε να εμφανίζεται κάθε πόρο. Εντοπίστε και επιλέξτε στη δημόσια διεύθυνση IP του διευθυντή. Αυτό θα ανοίξει μια blade που περιέχει πληροφορίες σχετικά με τη δημόσια διεύθυνση IP, η οποία περιλαμβάνει το όνομα DNS προς τα επάνω. Αποθηκεύστε το ίδιο όνομα για μελλοντική χρήση. <br />


![Δημόσιο όνομα DNS](media/pubdns.png)

Τώρα, ανοίξτε ένα κέλυφος και εκτελέστε την ακόλουθη εντολή όπου:

**ΘΎΡΑ** είναι η θύρα από το τελικό σημείο που θέλετε να εμφανίσετε. Για Swarm, αυτό είναι 2375. Για ελεγκτή Τομέα/λειτουργικό σύστημα, χρησιμοποιήστε τη θύρα 80.  
**Όνομα ΧΡΉΣΤΗ** είναι το όνομα χρήστη που σας δόθηκε όταν αναπτυχθεί το σύμπλεγμα.  
**DNSPREFIX** είναι το πρόθεμα DNS που παρείχατε κατά την ανάπτυξη του συμπλέγματος.  
**ΠΕΡΙΟΧΉ** είναι η περιοχή στην οποία βρίσκεται το ομάδα πόρων.  
**PATH_TO_PRIVATE_KEY** [ΠΡΟΑΙΡΕΤΙΚΌ] είναι η διαδρομή προς το ιδιωτικό κλειδί που αντιστοιχεί στο δημόσιο κλειδί που παρείχατε κατά τη δημιουργία του κοντέινερ υπηρεσιών συμπλέγματος. Χρησιμοποιήστε αυτήν την επιλογή με το i - Προσθήκη σημαίας.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> Η θύρα σύνδεσης SSH είναι 2200--όχι την τυπική θύρα 22.

## <a name="dcos-tunnel"></a>Σωλήνα ελεγκτή Τομέα/λειτουργικό σύστημα

Για να ανοίξετε μια διοχέτευση προς τα τελικά σημεία ελεγκτή Τομέα/OS-που σχετίζονται με, να εκτελέσετε μια εντολή που είναι παρόμοιο με το εξής:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Τώρα μπορείτε να αποκτήσετε πρόσβαση του ελεγκτή Τομέα/OS-που σχετίζονται με τα τελικά σημεία στο:

- ΚΈΝΤΡΟ ΔΕΔΟΜΈΝΩΝ/ΛΕΙΤΟΥΡΓΙΚΌ ΣΎΣΤΗΜΑ:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Ομοίως, μπορείτε να επικοινωνήσετε μαζί τα υπόλοιπα APIs για κάθε εφαρμογή μέσω αυτό διοχέτευσης.

## <a name="swarm-tunnel"></a>Swarm σωλήνα

Για να ανοίξετε μια διοχέτευση προς το τελικό σημείο Swarm, να εκτελέσετε μια εντολή που μοιάζει με το εξής:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Τώρα μπορείτε να ορίσετε τη μεταβλητή περιβάλλοντος DOCKER_HOST ως εξής. Μπορείτε να συνεχίσετε να χρησιμοποιείτε το περιβάλλον γραμμής εντολών Docker (CLI) κανονικά.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Δημιουργήστε μια διοχέτευση SSH στα Windows

Υπάρχουν πολλές επιλογές για τη δημιουργία σήραγγες SSH στα Windows. Αυτό το έγγραφο θα περιγράφουν τον τρόπο χρήσης του PuTTY να το κάνετε αυτό.

Κάντε λήψη PuTTY στο σύστημα των Windows και εκτελέστε την εφαρμογή.

Πληκτρολογήστε ένα όνομα κεντρικού υπολογιστή που αποτελείται από το όνομα χρήστη του διαχειριστή συμπλέγματος και το δημόσιο όνομα DNS από το πρώτο υπόδειγμα του συμπλέγματος. Το **Όνομα κεντρικού υπολογιστή** θα μοιάζει κάπως έτσι: `adminuser@PublicDNS`. Εισαγάγετε 2200 για τη **θύρα**.

![Ρύθμιση παραμέτρων puTTY 1](media/putty1.png)

Επιλέξτε **SSH** και **τον έλεγχο ταυτότητας**. Προσθέστε το αρχείο σας ιδιωτικό κλειδί για τον έλεγχο ταυτότητας.

![Ρύθμιση παραμέτρων puTTY 2](media/putty2.png)

Επιλέξτε **σήραγγες** και ρυθμίσετε τις ακόλουθες παραμέτρους προωθούνται θύρες:
- **Θύρα προέλευσης:** Την προτίμησή σας--χρήση 80 για ελεγκτή Τομέα/λειτουργικό σύστημα ή 2375 για Swarm.
- **Προορισμός:** Χρησιμοποιήστε localhost:80 για ελεγκτή Τομέα/λειτουργικό σύστημα ή localhost:2375 για Swarm.

Το παρακάτω παράδειγμα έχει ρυθμιστεί για ελεγκτή Τομέα/λειτουργικό σύστημα, αλλά θα είναι παρόμοιο για Docker Swarm.

>[AZURE.NOTE] Θύρα 80 δεν πρέπει να χρησιμοποιείται κατά τη δημιουργία διοχέτευσης σε αυτό.

![Ρύθμιση παραμέτρων puTTY 3](media/putty3.png)

Όταν ολοκληρώσετε, αποθηκεύστε τη ρύθμιση παραμέτρων σύνδεσης και συνδέστε την περίοδο λειτουργίας PuTTY. Όταν συνδέεστε, μπορείτε να δείτε τις παραμέτρους της θύρας στο αρχείο καταγραφής συμβάντων PuTTY.

![PuTTY αρχείο καταγραφής συμβάντων](media/putty4.png)

Όταν έχετε ρυθμίσει τις παραμέτρους της διοχέτευσης για ελεγκτή Τομέα/λειτουργικό σύστημα, μπορείτε να αποκτήσετε πρόσβαση σε αυτό το τελικό σημείο σχετικά με:

- ΚΈΝΤΡΟ ΔΕΔΟΜΈΝΩΝ/ΛΕΙΤΟΥΡΓΙΚΌ ΣΎΣΤΗΜΑ:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Όταν έχετε ρυθμίσει τις παραμέτρους της διοχέτευσης για Docker Swarm, μπορείτε να αποκτήσετε πρόσβαση στο σύμπλεγμα Swarm έως το CLI Docker. Θα πρέπει πρώτα να ρυθμίσετε τις παραμέτρους ενός μεταβλητή περιβάλλοντος των Windows με το όνομα `DOCKER_HOST` με τιμή ` :2375`.

## <a name="next-steps"></a>Επόμενα βήματα

Ανάπτυξη και διαχείριση κοντέινερ με ελεγκτή Τομέα/λειτουργικό σύστημα ή Swarm:

- [Εργασία με υπηρεσία Azure κοντέινερ και ελεγκτή Τομέα/λειτουργικό σύστημα](container-service-mesos-marathon-rest.md)
- [Εργασία με την υπηρεσία Azure κοντέινερ και Docker Swarm](container-service-docker-swarm.md)
