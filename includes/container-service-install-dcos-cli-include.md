<properties
   pageTitle="Εγκατάσταση του ελεγκτή Τομέα/OS CLI | Microsoft Azure"
   description="Εγκατάσταση του ελεγκτή Τομέα/OS CLI."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Κοντέινερ, μικρής κλίμακας-υπηρεσίες, ελεγκτή Τομέα/λειτουργικό σύστημα, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] Πρόκειται για την εργασία με συμπλεγμάτων που βασίζονται σε ελεγκτή Τομέα/λειτουργικό σύστημα ACS. Δεν χρειάζεται να το κάνετε αυτό για συμπλεγμάτων βάσει Swarm ACS.

Αρχικά, [συνδεθείτε με το σύμπλεγμα ACS βάσει ελεγκτή Τομέα/λειτουργικό σύστημα](../articles/container-service/container-service-connect.md). Μόλις το κάνετε αυτό, μπορείτε να εγκαταστήσετε το CLI ελεγκτή Τομέα/λειτουργικό σύστημα στον υπολογιστή-πελάτη με τις παρακάτω εντολές:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Εάν χρησιμοποιείτε μια παλαιότερη έκδοση του Python, ενδέχεται να παρατηρήσετε ορισμένες "InsecurePlatformWarnings". Μπορείτε να αγνοήσετε τα εξής.

Για να ξεκινήσετε χωρίς να γίνει επανεκκίνηση του κελύφους, εκτελέστε:

```bash
source ~/.bashrc
```

Αυτό το βήμα δεν θα είναι απαραίτητο, κατά την εκκίνηση του νέου άλλου κελύφους.

Τώρα μπορείτε να επιβεβαιώσετε ότι έχει εγκατασταθεί το CLI:

```bash
dcos --help
```