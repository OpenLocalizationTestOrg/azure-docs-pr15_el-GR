<properties
   pageTitle="Azure Διαχείριση κοντέινερ υπηρεσίας κοντέινερ έως το REST API | Microsoft Azure"
   description="Ανάπτυξη κοντέινερ σε ένα σύμπλεγμα Mesos υπηρεσία κοντέινερ Azure χρησιμοποιώντας το REST API Marathon."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, κοντέινερ, μικρής κλίμακας-υπηρεσίες, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-rest-api"></a>Διαχείριση κοντέινερ μέσω του REST API

Κέντρο Δεδομένων/OS παρέχει ένα περιβάλλον για την ανάπτυξη και κλιμάκωση ομαδοποιημένων φόρτους εργασίας, ενώ abstracting το υποκείμενο υλικό. Επάνω από το Κέντρο Δεδομένων/λειτουργικό σύστημα, υπάρχει ένα πλαίσιο που διαχειρίζεται τον προγραμματισμό και την εκτέλεση φόρτους εργασίας του υπολογισμού.

Αν και πλαίσια είναι διαθέσιμες για πολλά δημοφιλή φόρτους εργασίας, αυτό το έγγραφο περιγράφει πώς μπορείτε να δημιουργήσετε και να κλιμάκωση αναπτύξεις κοντέινερ με τη χρήση Marathon. Πριν να εργαστείτε με αυτά τα παραδείγματα, χρειάζεστε ένα σύμπλεγμα Συνεχούς/λειτουργικό σύστημα που έχει ρυθμιστεί στην υπηρεσία κοντέινερ Azure. Πρέπει επίσης να έχετε απομακρυσμένης σύνδεσης σε αυτό το σύμπλεγμα. Για περισσότερες πληροφορίες σχετικά με αυτά τα στοιχεία, ανατρέξτε στα ακόλουθα άρθρα:

- [Ανάπτυξη ένα σύμπλεγμα Azure κοντέινερ υπηρεσίας](container-service-deployment.md)
- [Σύνδεση με μια υπηρεσία κοντέινερ Azure συμπλέγματος](container-service-connect.md)

Αφού είστε συνδεδεμένοι στο σύμπλεγμα Azure κοντέινερ υπηρεσίας, μπορείτε να αποκτήσετε πρόσβαση του ελεγκτή Τομέα/λειτουργικό σύστημα και το σχετικό API του Yammer ΥΠΌΛΟΙΠΑ μέσω http://localhost:local θύρας. Τα παραδείγματα σε αυτό το έγγραφο που λαμβάνεται ως δεδομένο ότι διοχέτευση στη θύρα 80. Για παράδειγμα, μπορούν να σας καλούν το τελικό σημείο Marathon `http://localhost/marathon/v2/`. Για περισσότερες πληροφορίες σχετικά με τα διάφορα API, ανατρέξτε στην τεκμηρίωση δημιουργία νέων για το [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) και το [Chronos API](https://mesos.github.io/chronos/docs/api.html), και την τεκμηρίωση Apache για το [API Mesos χρονοδιαγράμματος](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## <a name="gather-information-from-dcos-and-marathon"></a>Συλλογή πληροφοριών από το Κέντρο Δεδομένων/λειτουργικό σύστημα και Marathon

Πριν να αναπτύξετε το κοντέινερ στο σύμπλεγμα ελεγκτή Τομέα/λειτουργικό σύστημα, συλλογή ορισμένες πληροφορίες σχετικά με το σύμπλεγμα ελεγκτή Τομέα/λειτουργικό σύστημα, όπως τα ονόματα και την τρέχουσα κατάσταση των παραγόντων ελεγκτή Τομέα/λειτουργικό σύστημα. Για να το κάνετε, ερωτήματος το `master/slaves` τελικό σημείο του ελεγκτή Τομέα/OS REST API. Εάν όλα τα στοιχεία μεταβαίνει σωστά, θα δείτε μια λίστα των παραγόντων ελεγκτή Τομέα/λειτουργικό σύστημα και πολλές ιδιότητες για κάθε μία.

```bash
curl http://localhost/mesos/master/slaves
```

Τώρα, χρησιμοποιήστε το Marathon `/apps` τελικό σημείο για να ελέγξετε για την τρέχουσα εφαρμογή αναπτύξεις στο σύμπλεγμα ελεγκτή Τομέα/λειτουργικό σύστημα. Εάν πρόκειται για νέο σύμπλεγμα, θα εμφανιστεί ένας κενός πίνακας για τις εφαρμογές.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Ανάπτυξη ενός κοντέινερ Docker μορφοποίηση

Μπορείτε να αναπτύξετε Docker μορφοποίηση κοντέινερ μέσω Marathon, χρησιμοποιώντας ένα αρχείο JSON που περιγράφει το οποίο προορίζεται ανάπτυξης. Το παρακάτω παράδειγμα θα αναπτύξετε το κοντέινερ Nginx, σύνδεση θύρα 80 του ελεγκτή Τομέα/OS παράγοντα στη θύρα 80 του κοντέινερ. Επίσης, σημειώστε ότι η ιδιότητα 'acceptedResourceRoles' έχει οριστεί σε 'slave_public'. Αυτό θα αναπτύξετε το κοντέινερ σε έναν παράγοντα του συνόλου κλίμακα παράγοντας δημόσια τοποθεσία.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Για να αναπτύξετε ένα κοντέινερ μορφοποίηση Docker, δημιουργήστε το δικό σας αρχείο JSON ή χρησιμοποιήστε το δείγμα που παρέχεται στην [υπηρεσία κοντέινερ Azure επίδειξη](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Αποθηκεύστε το σε μια προσβάσιμη τοποθεσία. Στη συνέχεια, για να αναπτύξετε το κοντέινερ, εκτελέστε την ακόλουθη εντολή. Καθορίστε το όνομα του αρχείου JSON.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Το αποτέλεσμα θα είναι παρόμοιο με το εξής:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Τώρα, αν ερώτημα Marathon για εφαρμογές, αυτή η νέα εφαρμογή θα εμφανιστεί στο αποτέλεσμα.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>Κλίμακα του κοντέινερ

Μπορείτε επίσης να χρησιμοποιήσετε το API Marathon για να κλιμακωθεί ανάληψη ή την κλίμακα σε εφαρμογή αναπτύξεις. Στο προηγούμενο παράδειγμα, μπορείτε να αναπτυχθεί μία παρουσία μιας εφαρμογής του. Ας κλίμακα σε αυτό από τρεις παρουσίες μιας εφαρμογής του. Για να το κάνετε αυτό, δημιουργήστε ένα αρχείο JSON, χρησιμοποιώντας το ακόλουθο κείμενο JSON και αποθηκεύστε το σε μια προσβάσιμη τοποθεσία.

```json
{ "instances": 3 }
```

Εκτελέστε την παρακάτω εντολή για να κλιμακωθεί εκτός της εφαρμογής.

>[AZURE.NOTE] Το URI θα http://localhost/marathon/v2/apps/ και, στη συνέχεια, το Αναγνωριστικό της εφαρμογής για να κλιμακωθεί. Εάν χρησιμοποιείτε το δείγμα Nginx που παρέχεται εδώ, το URI θα ήταν http://localhost/marathon/v2/apps/nginx.

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Τέλος, ερώτημα το τελικό σημείο Marathon για εφαρμογές. Θα δείτε ότι τώρα υπάρχουν τρεις από το κοντέινερ Nginx.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>Χρήση του PowerShell για αυτήν την άσκηση: Marathon REST API αλληλεπίδραση με το PowerShell

Μπορείτε να εκτελέσετε αυτές τις ίδιες ενέργειες, χρησιμοποιώντας τις εντολές του PowerShell σε ένα σύστημα Windows.

Για να συλλέξετε πληροφορίες σχετικά με το σύμπλεγμα ελεγκτή Τομέα/λειτουργικό σύστημα, όπως ονόματα παράγοντας και κατάσταση παράγοντα, εκτελέστε την ακόλουθη εντολή.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Μπορείτε να αναπτύξετε Docker μορφοποίηση κοντέινερ μέσω Marathon, χρησιμοποιώντας ένα αρχείο JSON που περιγράφει το οποίο προορίζεται ανάπτυξης. Το παρακάτω παράδειγμα θα αναπτύξετε το κοντέινερ Nginx, σύνδεση θύρα 80 του ελεγκτή Τομέα/OS παράγοντα στη θύρα 80 του κοντέινερ.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Δημιουργήστε το δικό σας αρχείο JSON ή χρησιμοποιήστε το δείγμα που παρέχεται στην [υπηρεσία κοντέινερ Azure επίδειξη](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Αποθηκεύστε το σε μια προσβάσιμη τοποθεσία. Στη συνέχεια, για να αναπτύξετε το κοντέινερ, εκτελέστε την ακόλουθη εντολή. Καθορίστε το όνομα του αρχείου JSON.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Μπορείτε επίσης να χρησιμοποιήσετε το API Marathon για να κλιμακωθεί ανάληψη ή την κλίμακα σε εφαρμογή αναπτύξεις. Στο προηγούμενο παράδειγμα, μπορείτε να αναπτυχθεί μία παρουσία μιας εφαρμογής του. Ας κλίμακα σε αυτό από τρεις παρουσίες μιας εφαρμογής του. Για να το κάνετε αυτό, δημιουργήστε ένα αρχείο JSON, χρησιμοποιώντας το ακόλουθο κείμενο JSON και αποθηκεύστε το σε μια προσβάσιμη τοποθεσία.

```json
{ "instances": 3 }
```

Εκτελέστε την παρακάτω εντολή για να κλιμακωθεί εκτός της εφαρμογής.

> [AZURE.NOTE] Το URI θα http://localhost/marathon/v2/apps/ και, στη συνέχεια, το Αναγνωριστικό της εφαρμογής για να κλιμακωθεί. Εάν χρησιμοποιείτε το δείγμα Nginx που παρέχονται εδώ, το URI θα ήταν http://localhost/marathon/v2/apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Επόμενα βήματα

- [Διαβάστε περισσότερα σχετικά με τα τελικά σημεία Mesos HTTP]( http://mesos.apache.org/documentation/latest/endpoints/).
- [Διαβάστε περισσότερα σχετικά με το REST API Marathon]( https://mesosphere.github.io/marathon/docs/rest-api.html).
