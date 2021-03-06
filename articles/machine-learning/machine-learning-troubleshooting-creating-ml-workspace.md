<properties
    pageTitle="Αντιμετώπιση προβλημάτων: Δημιουργία και να συνδεθείτε με ένα χώρο εργασίας μηχανικής εκμάθησης | Microsoft Azure"
    description="Λύσεις για συνήθη ζητήματα στη δημιουργία και τη σύνδεση σε ένα χώρο εργασίας του Azure μηχανικής εκμάθησης"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="garye"/>


# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Οδηγός αντιμετώπισης προβλημάτων: δημιουργία και να συνδεθείτε με ένα χώρο εργασίας μηχανικής εκμάθησης

Αυτός ο οδηγός παρέχει λύσεις για ορισμένες αντιμετώπισε συχνά προκλήσεις όταν ρυθμίζετε χώρων εργασίας Azure μηχανικής εκμάθησης.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Κάτοχος χώρου εργασίας

Όταν δημιουργείτε ένα νέο χώρο εργασίας μηχανικής εκμάθησης, το Αναγνωριστικό που εισάγετε στο πεδίο ΚΑΤΌΧΟΥ χώρου ΕΡΓΑΣΊΑΣ πρέπει να είναι ένα έγκυρο λογαριασμό Microsoft (πρώην Windows Live ID), για παράδειγμα, john-contoso@live.com ή john-contoso@hotmail.com. Δεν μπορεί να είναι ένα λογαριασμό δεν ανήκουν στη Microsoft, όπως το εταιρικός λογαριασμός ηλεκτρονικού ταχυδρομείου. Για να δημιουργήσετε έναν δωρεάν λογαριασμό Microsoft, μεταβείτε στο [www.live.com](http://www.live.com).

Σημειώστε ότι το λογαριασμό που χρησιμοποιήσατε για να πραγματοποιήσετε είσοδο στην πύλη του Azure για να δημιουργήσετε το χώρο εργασίας δεν αυτόματα έχει δικαίωμα να *ανοίξετε* αυτόν το χώρο εργασίας, εκτός εάν καθορίσετε αυτόν το λογαριασμό ως ο κάτοχος του. Για να ανοίξετε ένα χώρο εργασίας στο μηχανικής εκμάθησης Studio, που πρέπει να εισέλθετε στο λογαριασμό Microsoft που έχει οριστεί ως ο κάτοχος του χώρου εργασίας, ή πρέπει να λάβουν μια πρόσκληση από τον κάτοχο για να συμμετάσχετε στο χώρο εργασίας. Από την πύλη του Azure κλασική, ωστόσο, μπορείτε να *διαχειριστείτε* το χώρο εργασίας, η οποία περιλαμβάνει τη δυνατότητα να αλλάξετε τον κάτοχο και να ρυθμίσετε τις παραμέτρους πρόσβασης.

Για περισσότερες πληροφορίες σχετικά με τη διαχείριση ενός χώρου εργασίας, ανατρέξτε στο θέμα [Διαχείριση ενός χώρου εργασίας Azure μηχανικής εκμάθησης].

[Διαχείριση ενός χώρου εργασίας Azure μηχανικής εκμάθησης]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Οι επιτρεπόμενοι περιοχές

Μηχανικής εκμάθησης είναι διαθέσιμη αυτήν τη στιγμή σε περιορισμένο αριθμό των περιοχών. Εάν η συνδρομή σας δεν περιλαμβάνει μία από αυτές τις περιοχές, ενδέχεται να εμφανιστεί το μήνυμα σφάλματος, "Έχετε δεν υπάρχει στις περιοχές επιτρεπόμενων."

Για να ζητήσετε ότι μια περιοχή προστεθούν στη συνδρομή σας, επιλέξτε **Επικοινωνία με την υποστήριξη της Microsoft** από την πύλη κλασική Azure, επιλέξτε **χρέωση** ως τύπο πρόβλημα και ακολουθήστε τις οδηγίες για την υποβολή της αίτησης.

![Επικοινωνήστε με την υποστήριξη της Microsoft][screen1]

## <a name="storage-account"></a>Το λογαριασμό χώρου αποθήκευσης

Η υπηρεσία μηχανικής εκμάθησης ανάγκες ενός λογαριασμού χώρου αποθήκευσης για την αποθήκευση δεδομένων. Μπορείτε να χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης ή μπορείτε να δημιουργήσετε ένα νέο λογαριασμό του χώρου αποθήκευσης όταν δημιουργείτε το νέο χώρο εργασίας μηχανικής εκμάθησης (Εάν έχετε ορίου για να δημιουργήσετε ένα νέο λογαριασμό του χώρου αποθήκευσης).

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Δημιουργία χώρου εργασίας][screen2]

Αφού δημιουργηθεί το νέο χώρο εργασίας μηχανικής εκμάθησης, μπορείτε να συνδεθείτε μηχανικής εκμάθησης Studio, χρησιμοποιώντας το λογαριασμό Microsoft που έχετε καθορίσει ως ο κάτοχος του χώρου εργασίας. Εάν αντιμετωπίσετε το μήνυμα σφάλματος, "Χώρος εργασίας δεν βρέθηκε" (παρόμοιο με το παρακάτω στιγμιότυπο οθόνης), χρησιμοποιήστε τα παρακάτω βήματα για να διαγράψετε τα cookies του προγράμματος περιήγησης.

![Χώρος εργασίας δεν βρέθηκε][screen3]

**Για να διαγράψετε τα cookies του προγράμματος περιήγησης**

Εάν χρησιμοποιείτε τον Internet Explorer, κάντε κλικ στο κουμπί **Εργαλεία** στην επάνω δεξιά γωνία και επιλέξτε **Επιλογές Internet**.  

![Επιλογές Internet][screen4]

Στην καρτέλα **Γενικά** , κάντε κλικ στην επιλογή **Διαγραφή...**

![Καρτέλα "Γενικά"][screen5]

Στο παράθυρο διαλόγου **Διαγραφή ιστορικού περιήγησης** , βεβαιωθείτε ότι είναι επιλεγμένο **Cookies και δεδομένα τοποθεσιών Web** και κάντε κλικ στην επιλογή **Διαγραφή**.

![Διαγραφή των cookies][screen6]

Μετά τη διαγραφή των cookies, επανεκκινήστε το πρόγραμμα περιήγησης και, στη συνέχεια, μεταβείτε στη σελίδα του [Microsoft Azure μηχανικής εκμάθησης](https://studio.azureml.net) . Όταν σας ζητηθεί το όνομα χρήστη και κωδικό πρόσβασης, πληκτρολογήστε τον ίδιο λογαριασμό Microsoft που έχετε καθορίσει ως ο κάτοχος του χώρου εργασίας.

Στόχος μας είναι να κάνετε ως απρόσκοπτη όσο το δυνατόν πιο την εμπειρία μηχανικής εκμάθησης. Καταχωρήστε τυχόν σχόλια και θέματα στο [φόρουμ Azure μηχανικής εκμάθησης](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) για να μας που εξυπηρετούν καλύτερα.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
