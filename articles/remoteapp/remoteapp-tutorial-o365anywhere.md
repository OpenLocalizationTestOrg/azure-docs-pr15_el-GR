<properties
   pageTitle="Λάβετε το ίδιο εμπειρία στο Office 365 σε οποιαδήποτε συσκευή με το Azure RemoteApp | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να κάνετε κοινή χρήση οποιαδήποτε εφαρμογή του Office 365 με τους χρήστες σας με χρήση του Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="guscatal;elizapo"/>


# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Λάβετε το ίδιο εμπειρία στο Office 365 σε οποιαδήποτε συσκευή με Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Αυτό το άρθρο περιγράφει πώς μπορείτε να αναπτύξετε το Office 365 σε οποιαδήποτε συσκευή στην εταιρεία σας. Οι χρήστες σας μπορούν να έχετε τις ίδιες δυνατότητες και εμπειρία περιβάλλοντος εργασίας Χρήστη στην Android, Apple και Windows.

Αυτό θα σας θα το εκτελέσετε με Azure RemoteApp, εκτελώντας το Office 365 σε κλίμακα δυνατότητα εικονικές μηχανές στο Azure που οι χρήστες μπορούν να συνδεθούν. Αυτό το σύνολο των εικονικές μηχανές ονομάζουμε μια συλλογή"cloud".

## <a name="create-a-cloud-collection"></a>Δημιουργία συλλογής cloud

Πρώτα αφού έχετε δημιουργήσει ένα λογαριασμό Azure, μεταβείτε **RemoteApp** , κάνοντας κλικ στη σύνδεση στην αριστερή πλευρά.
![Εμφάνιση Azure RemoteApp στην πύλη του Azure](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Στη συνέχεια, συνεχίστε κάνοντας κλικ στην επιλογή **νέα** στο κάτω μέρος και "Γρήγορη δημιουργία" μιας συλλογής. Δώστε ένα όνομα, στην περιοχή, η συνδρομή, το πρόγραμμα και η εικόνα "Office Proffesional 2013" που παρέχουμε.
![Παράθυρο διαλόγου Δημιουργία](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Αφού ολοκληρώσετε τη φόρμα που θα πρέπει να ξεκινήσει η διαδικασία δημιουργίας συλλογής. Αυτό μπορεί να χρειαστεί έως και σε μία ώρα ή για να.

![Αναμονή](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Μετά την ολοκλήρωση της διαδικασίας, θα μοιάζει κάπως έτσι. Εάν κάνουμε κλικ στο κουμπί **Δημοσίευση** μπορούμε να δούμε ότι οι περισσότερες εφαρμογές του Office έχουν δημοσιευτεί για μας ήδη.
![Συλλογή δημιουργήθηκε](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Δημοσιευμένη εφαρμογές](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

Σε αυτό το σημείο μπορείτε επίσης να προσθέσετε περισσότερους χρήστες που έχουν πρόσβαση σε αυτήν τη συλλογή κάνοντας κλικ στην επιλογή **Πρόσβασης χρήστη**.
![Ρύθμιση παραμέτρων πρόσβασης χρήστη](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Τώρα ας δοκιμάστε τη σύνδεση στο Office 365!

## <a name="connect-to-office-365"></a>Σύνδεση στο Office 365

Θα σας θα κεντρικών επάνω από για να [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), κάντε κύλιση προς τα κάτω και κάντε κλικ στην επιλογή **λήψη προγράμματα-πελάτες** για να εγκαταστήσετε το πρόγραμμα-πελάτη Azure RemoteApp στη συσκευή σας είναι. Το παρακάτω στιγμιότυπα οθόνης είναι για Windows.

Μετά την εκκίνηση της εφαρμογής θα σας ζητηθεί να συνδεθείτε με το λογαριασμό Microsoft (παλαιότερα γνωστές ως ένα "Live ID"), χρησιμοποιήστε το ίδιο με το λογαριασμό σας Azure προς το παρόν. Όταν είστε συνδεδεμένοι στο θα πρέπει να δείτε μια ειδοποίηση σχετικά με τις νέες προσκλήσεις, κάντε κλικ στην επιλογή εκεί και θα πρέπει να δείτε μια λίστα όπως παρακάτω. Αποδεχτείτε την πρόσκληση που ταιριάζει με το ηλεκτρονικό ταχυδρομείο σας λογαριασμός Azure κατόχου.

![Νέα πρόσκληση](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Πώς εμφανίζεται όταν υπάρχουν νέα προσκλήσεων.

![Αποδοχή μιας εφαρμογής](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Μόλις αποδεχτείτε την πρόσκληση, θα πρέπει να βλέπετε όλες τις εφαρμογές του Office στο πρόγραμμα-πελάτη Azure RemoteApp.

![Λίστα των εφαρμογών](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Όταν κάνετε κλικ σε οποιαδήποτε από αυτές τις την εφαρμογή πρέπει να ξεκινά η εικονική μηχανή Azure και που πρέπει να είναι έτοιμοι! Απολαύστε!

![Έναρξη](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![το PowerPoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)
