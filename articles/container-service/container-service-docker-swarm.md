<properties
   pageTitle="Azure Διαχείριση κοντέινερ υπηρεσίας κοντέινερ με Docker Swarm | Microsoft Azure"
   description="Ανάπτυξη μιας Swarm Docker στην υπηρεσία κοντέινερ Azure κοντέινερ"
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

# <a name="container-management-with-docker-swarm"></a>Διαχείριση κοντέινερ με Docker Swarm

Docker Swarm παρέχει ένα περιβάλλον για την ανάπτυξη περιεχόμενα φόρτους εργασίας σε ένα σύνολο συνδυασμένου Docker κεντρικών υπολογιστών. Docker Swarm χρησιμοποιεί το εγγενές API Docker. Η ροή εργασίας για τη διαχείριση του κοντέινερ σε μια Swarm Docker είναι σχεδόν ίδια με αυτή που θα ήταν σε ένα μεμονωμένο κοντέινερ κεντρικό υπολογιστή. Αυτό το έγγραφο παρέχει απλές παραδείγματα για την ανάπτυξη περιεχόμενα φόρτους εργασίας σε μια υπηρεσία κοντέινερ Azure παρουσία της Docker Swarm. Για περισσότερες αναλυτικά τεκμηρίωση στην Docker Swarm, ανατρέξτε στο θέμα [Docker Swarm σε Docker.com](https://docs.docker.com/swarm/).

Προαπαιτούμενα για τη ασκήσεις σε αυτό το έγγραφο:

[Δημιουργήστε ένα σύμπλεγμα Swarm στην υπηρεσία κοντέινερ Azure](container-service-deployment.md)

[Σύνδεση με το σύμπλεγμα Swarm στην υπηρεσία κοντέινερ Azure](container-service-connect.md)

## <a name="deploy-a-new-container"></a>Ανάπτυξη ένα νέο κοντέινερ

Για να δημιουργήσετε ένα νέο κοντέινερ το Swarm Docker, χρησιμοποιήστε το `docker run` εντολή (ταυτόχρονα εξασφαλίζετε ότι οι που έχετε ανοίξει διοχέτευση SSH για τα υποδείγματα σύμφωνα με τις προϋποθέσεις παραπάνω). Αυτό το παράδειγμα δημιουργεί ένα κοντέινερ από το `yeasy/simple-web` εικόνα:


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Μετά τη δημιουργία του κοντέινερ, χρησιμοποιήστε `docker ps` για να λάβετε πληροφορίες σχετικά με το κοντέινερ. Παρατηρήστε ότι εμφανίζεται ο παράγοντας Swarm που φιλοξενεί το κοντέινερ:


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Τώρα μπορείτε να αποκτήσετε πρόσβαση στην εφαρμογή που εκτελείται σε αυτό το κοντέινερ έως το δημόσιο όνομα DNS από τη μονάδα εξισορρόπησης φόρτου παράγοντας Swarm. Μπορείτε να βρείτε αυτές τις πληροφορίες στην πύλη του Azure:  


![Πραγματικό επίσκεψη αποτελεσμάτων](media/real-visit.jpg)  

Από προεπιλογή, την εξισορρόπηση φόρτου έχει θύρες 80, Άνοιγμα 8080 και 443. Εάν θέλετε να συνδεθείτε σε μια άλλη θύρα θα πρέπει να το ανοίξετε αυτής της θύρας σε τη μονάδα εξισορρόπησης φόρτου Azure για το χώρο συγκέντρωσης παράγοντα.

## <a name="deploy-multiple-containers"></a>Ανάπτυξη πολλών κοντέινερ

Όπως ξεκινούν πολλών κοντέινερ, εκτελώντας 'εκτέλεση docker' πολλές φορές, μπορείτε να χρησιμοποιήσετε το `docker ps` εντολή για να δείτε που φιλοξενεί το κοντέινερ εκτελούνται σε. Στο παρακάτω παράδειγμα, τρεις κοντέινερ είναι που εκτείνεται ομοιόμορφα τους τρεις Swarm παράγοντες:  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Ανάπτυξη κοντέινερ χρησιμοποιώντας σύνθεση Docker

Μπορείτε να χρησιμοποιήσετε τη σύνθεση Docker για την αυτοματοποίηση του ανάπτυξης και ρύθμισης παραμέτρων πολλών κοντέινερ. Για να το κάνετε, βεβαιωθείτε ότι έχει δημιουργηθεί μια διοχέτευση ασφαλούς κελύφους (SSH) και ότι έχει οριστεί η μεταβλητή DOCKER_HOST (βλέπε παραπάνω προαπαιτούμενα).

Δημιουργήστε ένα αρχείο docker compose.yml στον τοπικό σας σύστημα. Για να κάνετε αυτό, χρησιμοποιήστε αυτό το [δείγμα](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Εκτέλεση `docker-compose up -d` για να ξεκινήσετε το αναπτύξεις κοντέινερ:


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Τέλος, θα επιστραφούν τη λίστα των κοντέινερ. Αυτή η λίστα εμφανίζει τα κοντέινερ που είχαν αναπτυχθεί με τη χρήση Docker σύνθεση:


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Φυσικά, μπορείτε να χρησιμοποιήσετε `docker-compose ps` να εξετάσετε μόνο τα κοντέινερ ορίζεται στο σας `compose.yml` αρχείου.

## <a name="next-steps"></a>Επόμενα βήματα

[Μάθετε περισσότερα σχετικά με Docker Swarm](https://docs.docker.com/swarm/)
