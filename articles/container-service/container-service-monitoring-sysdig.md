<properties
   pageTitle="Παρακολούθηση ένα σύμπλεγμα Azure κοντέινερ υπηρεσίας με Sysdig | Microsoft Azure"
   description="Παρακολουθήστε ένα σύμπλεγμα Azure κοντέινερ υπηρεσίας με Sysdig."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Κοντέινερ, ελεγκτή Τομέα/λειτουργικό σύστημα, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Οθόνη ένα σύμπλεγμα Azure κοντέινερ υπηρεσίας με Sysdig

Σε αυτό το άρθρο θα σας θα αναπτύξετε παράγοντες Sysdig σε όλους τους κόμβους παράγοντας το σύμπλεγμά σας Azure κοντέινερ υπηρεσίας. Χρειάζεστε ένα λογαριασμό με Sysdig για αυτήν τη ρύθμιση παραμέτρων. 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία 

[Ανάπτυξη](container-service-deployment.md) και [σύνδεση](container-service-connect.md) ένα σύμπλεγμα έχει ρυθμιστεί από Azure κοντέινερ υπηρεσίας. Εξερεύνηση του [Marathon περιβάλλοντος εργασίας Χρήστη](container-service-mesos-marathon-ui.md). Μεταβείτε στο [http://app.sysdigcloud.com](http://app.sysdigcloud.com) για να ρυθμίσετε ένα λογαριασμό cloud Sysdig. 

## <a name="sysdig"></a>Sysdig

Sysdig είναι μια υπηρεσία παρακολούθησης που σας επιτρέπει να παρακολουθείτε τους χώρους εντός το σύμπλεγμά σας. Sysdig είναι γνωστό για να βοηθήσουν στην αντιμετώπιση προβλημάτων, αλλά διαθέτει επίσης το βασικό μετρικά παρακολούθησης για CPU, τη δικτύωση, μνήμης και εισόδου/εξόδου. Sysdig διευκολύνει για να δείτε ποια κοντέινερ εργάζονται στο hardest ή ουσιαστικά χρησιμοποιώντας τα περισσότερα μνήμης και CPU. Αυτή η προβολή είναι της ενότητας "Επισκόπηση", η οποία είναι αυτήν τη στιγμή στο βήτα. 

![Sysdig περιβάλλοντος εργασίας Χρήστη](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Ρύθμιση παραμέτρων μιας ανάπτυξης Sysdig με Marathon

Τα παρακάτω βήματα θα σας δείξουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους και ανάπτυξη Sysdig εφαρμογών για το σύμπλεγμά σας με Marathon. 

Πρόσβαση του περιβάλλοντος εργασίας Χρήστη του ελεγκτή Τομέα/OS μέσω [http://localhost:80 /](http://localhost:80/) μία φορά στο περιβάλλον εργασίας Χρήστη του ελεγκτή Τομέα/λειτουργικό σύστημα, μεταβείτε με το "Universe", που βρίσκεται στην κάτω αριστερά και, στη συνέχεια, κάντε αναζήτηση για "Sysdig."

![Sysdig σε Universe ελεγκτή Τομέα/λειτουργικό σύστημα](./media/container-service-monitoring-sysdig/sysdig1.png)

Τώρα για να ολοκληρώσετε τη ρύθμιση παραμέτρων χρειάζεστε ένα λογαριασμό cloud Sysdig ή ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης. Όταν έχετε πραγματοποιήσει είσοδο στην τοποθεσία Web cloud Sysdig, κάντε κλικ στο όνομα χρήστη σας και, στη σελίδα, θα πρέπει να βλέπετε το "πλήκτρο πρόσβασης". 

![Πλήκτρο Sysdig API](./media/container-service-monitoring-sysdig/sysdig2.png) 

Στη συνέχεια πληκτρολογήστε τον αριθμό-κλειδί πρόσβασης σε τη ρύθμιση παραμέτρων Sysdig εντός του ελεγκτή Τομέα/OS Universe. 

![Ρύθμιση παραμέτρων Sysdig στο το Universe ελεγκτή Τομέα/λειτουργικό σύστημα](./media/container-service-monitoring-sysdig/sysdig3.png)

Ορισμός τώρα να 10000000 οι παρουσίες ώστε κάθε φορά που προστίθεται ένα νέο κόμβο στο σύμπλεγμα Sysdig θα αυτόματα αναπτύξετε παράγοντας σε αυτόν τον νέο κόμβο. Αυτή είναι μια ενδιάμεση λύση για να βεβαιωθείτε ότι θα αναπτύξετε Sysdig σε όλα τα νέα παράγοντες μέσα στο σύμπλεγμα. 

![Ρύθμιση παραμέτρων Sysdig στο ελεγκτή Τομέα/OS Universe-παρουσίες](./media/container-service-monitoring-sysdig/sysdig4.png)

Αφού εγκαταστήσετε το πακέτο μεταβείτε ξανά στο περιβάλλον εργασίας Χρήστη του Sysdig και θα έχετε τη δυνατότητα να εξερευνήσετε τα μετρικά διαφορετική χρήση για το κοντέινερ μέσα το σύμπλεγμά σας. 