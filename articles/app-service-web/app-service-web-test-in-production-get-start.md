<properties
    pageTitle="Γρήγορα αποτελέσματα με το δοκιμής παραγωγή για τις εφαρμογές Web"
    description="Μάθετε περισσότερα σχετικά με τον έλεγχο σε δυνατότητα παραγωγής (Συμβουλή) στο Azure εφαρμογής υπηρεσίας Web Apps."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-test-in-production-for-web-apps"></a>Γρήγορα αποτελέσματα με το δοκιμής παραγωγή για τις εφαρμογές Web

Έλεγχο της παραγωγής ή live-δοκιμή του web app χρησιμοποιώντας κίνηση ζωντανή πελατών, είναι μια στρατηγική δοκιμής που ενοποιούνται αυξανόμενη προγραμματιστές εφαρμογών σε τους μεθοδολογία [ανάπτυξης ευέλικτη](https://en.wikipedia.org/wiki/Agile_software_development) . Σας επιτρέπει να ελέγξετε την ποιότητα από τις εφαρμογές σας με την κυκλοφορία ζωντανή χρήστη στο περιβάλλον παραγωγής, σε αντίθεση με συνθετική δεδομένα σε περιβάλλον δοκιμής. Μέσω της έκθεσης τη νέα εφαρμογή σε πραγματικούς χρήστες, που μπορούν να ενημερωθούν στη το πραγματικό προβλήματα με την εφαρμογή σας μπορεί να κάνει μετά την ανάπτυξή. Μπορείτε να επαληθεύσετε τη λειτουργικότητα, επιδόσεων και τιμή από τις ενημερώσεις σας εφαρμογή σε σχέση με την ένταση ταχύτητας και ποικιλία πραγματικό κίνηση των χρηστών, όπου μπορείτε να προσέγγιση ποτέ σε περιβάλλον δοκιμής.

## <a name="traffic-routing-in-app-service-web-apps"></a>Κίνηση τη δρομολόγηση σε εφαρμογές Web της εφαρμογής υπηρεσίας

Με τη δυνατότητα δρομολόγησης κίνηση στο [Azure εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=529714), μπορείτε να κατευθύνετε ένα τμήμα του live χρήστη κίνηση σε μία ή περισσότερες [υποδοχές ανάπτυξης](web-sites-staged-publishing.md)και, στη συνέχεια, να αναλύσετε την εφαρμογή σας με το [Azure εφαρμογή ιδέες](/services/application-insights/) ή [Azure HDInsight](/services/hdinsight/), ή ένα εργαλείο άλλου κατασκευαστή, όπως [Νέα Relic](/marketplace/partners/newrelic/newrelic/) για να επικυρώσετε την αλλαγή. Για παράδειγμα, μπορείτε να εφαρμόσετε τα ακόλουθα σενάρια με εφαρμογή υπηρεσίας:

- Εντοπισμός σφαλμάτων λειτουργική ή επιδόσεων επιβράδυνση των διαδικασιών στη τις ενημερώσεις σας πριν από την ανάπτυξη σε ολόκληρη την τοποθεσία pinpoint
- Εκτέλεση "ελεγχόμενη δοκιμή πτήσεις" των αλλαγών σας με τη μέτρηση μετρικά usibility στην εφαρμογή του βήτα
- Αυξήσετε σταδιακά έως και μια νέα ενημερωμένη έκδοση και ομαλά Δημιουργήστε αντίγραφα προς τα κάτω στην τρέχουσα έκδοση εάν προκύψει σφάλμα 
- Βελτιστοποιήστε τα αποτελέσματα της επιχείρησής σας της εφαρμογής, εκτελώντας [A / B ελέγχει](https://en.wikipedia.org/wiki/A/B_testing) ή [multivariate δοκιμές](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) σε πολλές υποδοχές ανάπτυξης

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Απαιτήσεις για τη χρήση δρομολόγηση κίνηση στις εφαρμογές Web

- Εφαρμογή web της πρέπει να εκτελέσετε σε **Τυπική** ή **Premium** σειρά, όπως απαιτείται για πολλές υποδοχές ανάπτυξης.
- Για να λειτουργήσει σωστά, δρομολόγηση κίνηση απαιτεί τα cookies για να είναι ενεργοποιημένο στο πρόγραμμα περιήγησης των χρηστών. Κίνηση δρομολόγησης χρησιμοποιεί cookies για να καρφιτσώσετε ένα πρόγραμμα περιήγησης υπολογιστή-πελάτη σε μια υποδοχή ανάπτυξης για τη διάρκεια ζωής της περιόδου λειτουργίας του προγράμματος-πελάτη.
- Κίνηση δρομολόγησης υποστηρίζει σύνθετα σενάρια συμβουλή μέσω των cmdlet του Azure PowerShell.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>Δρομολόγηση του τμήματος κίνηση σε μια υποδοχή ανάπτυξης

Επίπεδο βασικές σε κάθε σενάριο TiP, δρομολογείτε προκαθορισμένο ποσοστό του live κυκλοφορία σας σε μια υποδοχή ανάπτυξη μη παραγωγής. Για να κάνετε αυτό, ακολουθήστε τα παρακάτω βήματα:

>[AZURE.NOTE] Τα βήματα που περιγράφονται εδώ προϋποθέτει ότι έχετε ήδη μια [Υποδοχή μη παραγωγής ανάπτυξης](web-sites-staged-publishing.md) και ότι το περιεχόμενο της εφαρμογής web που θέλετε είναι ήδη [αναπτυχθεί](web-sites-deploy.md) σε αυτήν.

1. Σύνδεση στο [Azure πύλη](https://portal.azure.com/).
2. Στο blade της εφαρμογής σας web, κάντε κλικ στην επιλογή **Ρυθμίσεις** > **Δρομολόγηση κίνηση**.
  ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Επιλέξτε την υποδοχή που θέλετε να δρομολογήσετε την κυκλοφορία στο και το ποσοστό του συνόλου κίνησης που επιθυμούν, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![](./media/app-service-web-test-in-production/02-select-slot.png)

4. Μεταβείτε στο blade την υποδοχή του ανάπτυξης. Τώρα θα πρέπει να βλέπετε ζωντανή κίνηση δρομολογούνται σε αυτήν.

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Όταν έχει ρυθμιστεί η δρομολόγηση κίνηση, στο συγκεκριμένο ποσοστό των υπολογιστών-πελατών θα δρομολογούνται τυχαία για να σας υποδιαίρεση μη παραγωγής. Ωστόσο, είναι σημαντικό να λάβετε υπόψη ότι όταν ένα πρόγραμμα-πελάτη δρομολογείται αυτόματα σε μια συγκεκριμένη υποδοχή, αυτό θα είναι "καρφιτσωμένη" σε αυτή την Υποδοχή για τη διάρκεια ζωής αυτήν την περίοδο λειτουργίας προγράμματος-πελάτη. Αυτό γίνεται χρησιμοποιώντας ένα cookie για να καρφιτσώσετε την περίοδο λειτουργίας χρήστη. Εάν μπορείτε να ελέγξετε τις αιτήσεις HTTP, θα βρείτε μια `TipMix` cookie σε κάθε οι επόμενες αίτησης.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Επιβολή στον υπολογιστή-πελάτη αιτήσεις σε μια συγκεκριμένη υποδοχή

Εκτός από την κυκλοφορία αυτόματη δρομολόγηση, εφαρμογής υπηρεσίας είναι δυνατό να δρομολόγηση αιτήσεων σε μια συγκεκριμένη υποδοχή. Αυτό είναι χρήσιμο όταν θέλετε οι χρήστες να μπορούν να επιλέγουν εάν σε ή εξαίρεση από την εφαρμογή σας βήτα. Για να το κάνετε αυτό, μπορείτε χρησιμοποιήσετε την `x-ms-routing-name` ερώτημα παραμέτρων.

Αναδρομολόγηση χρήστες σε μια συγκεκριμένη υποδοχή χρησιμοποιώντας `x-ms-routing-name`, πρέπει να βεβαιωθείτε ότι η εσοχή έχει ήδη προστεθεί στη λίστα δρομολόγησης κίνηση. Επειδή το που θέλετε για τη δρομολόγηση σε μια υποδοχή ρητά, το πραγματικό ποσοστό δρομολόγησης που ορίζετε δεν έχει σημασία. Εάν θέλετε, μπορείτε να δημιουργήσει "βήτα σύνδεσης", που μπορούν να επιλέγουν οι χρήστες για να αποκτήσετε πρόσβαση στην εφαρμογή βήτα.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Επιλογή χρηστών από την εφαρμογή βήτα

Για να επιτρέψετε σε χρήστες εξαίρεση από την εφαρμογή σας beta, για παράδειγμα, μπορείτε να τα βάλετε αυτήν τη σύνδεση στην ιστοσελίδα σας:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

Η συμβολοσειρά `x-ms-routing-name=self` Καθορίζει την υποδοχή παραγωγής. Όταν το πρόγραμμα περιήγησης του προγράμματος-πελάτη πρόσβαση στη σύνδεση, όχι μόνο ανακατευθύνεται στο διάστημα παραγωγής, αλλά θα περιέχει κάθε αίτηση οι επόμενες το `x-ms-routing-name=self` cookie που κωδικοί PIN την περίοδο λειτουργίας για να την υποδοχή παραγωγής.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Επιλογή χρηστών σε εφαρμογή βήτα

Για να επιτρέψετε στους χρήστες να επιλέξετε την εφαρμογή beta, ρυθμίστε το ίδιο παραμέτρου ερωτήματος στο όνομα της εσοχής μη παραγωγής, για παράδειγμα:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Περισσότεροι πόροι ##

-   [Ρύθμιση του ενδιάμεσου περιβάλλοντα για τις εφαρμογές web στο Azure εφαρμογής υπηρεσίας](web-sites-staged-publishing.md)
-   [Ανάπτυξη μιας σύνθετης εφαρμογής προβλέψιμα στο Azure](app-service-deploy-complex-application-predictably.md)
-   [Ανάπτυξη ευέλικτη λογισμικού με Azure εφαρμογής υπηρεσίας](app-service-agile-software-development.md)
-   [Αποτελεσματική χρήση περιβάλλοντα DevOps για τις εφαρμογές web](app-service-web-staged-publishing-realworld-scenarios.md)