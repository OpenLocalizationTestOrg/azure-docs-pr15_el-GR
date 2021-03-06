<properties
   pageTitle="Πώς μπορείτε να προσθέσετε σχόλια προελεύσεις δεδομένων | Microsoft Azure"
   description="Άρθρο διαδικασιών με την επισήμανση τρόπος για να προσθέσετε σχόλια δεδομένων παγίων στον κατάλογο δεδομένων Azure, όπως το φιλικό όνομα, ετικετών, περιγραφή και τους ειδικούς."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>


# <a name="how-to-annotate-data-sources"></a>Πώς μπορείτε να προσθέσετε σχόλια προελεύσεις δεδομένων

## <a name="introduction"></a>Εισαγωγή
**Κατάλογος δεδομένων Microsoft Azure** είναι μια υπηρεσία cloud πλήρως διαχειριζόμενων που χρησιμοποιείται ως ενός συστήματος ταξινόμησης και το σύστημα του εντοπισμού για προελεύσεις δεδομένων για μεγάλες επιχειρήσεις. Με άλλα λόγια, κατάλογος δεδομένων είναι όλες οι πληροφορίες για βοήθεια στην άτομα ανακαλύψετε, να κατανοήσετε και να χρησιμοποιήσετε προελεύσεις δεδομένων και παροχή βοήθειας εταιρείες για να λάβετε περισσότερες τιμή από τα υπάρχοντα δεδομένα. Όταν μια προέλευση δεδομένων έχει καταχωρηθεί με κατάλογο δεδομένων, τα μετα-δεδομένα αντιγράφεται και σε ευρετήριο από την υπηρεσία, αλλά την ιστορία δεν τελειώνει εκεί. Κατάλογος δεδομένων επιτρέπει στους χρήστες να δώσετε τις δικές τους περιγραφικό μετα-δεδομένων – όπως περιγραφές και οι ετικέτες – για να συμπληρώνουν τα μετα-δεδομένα που έχουν εξαχθεί από το αρχείο προέλευσης δεδομένων και για να κάνετε πιο κατανοητό στην προέλευση δεδομένων σε περισσότερα άτομα.

## <a name="annotation-and-crowdsourcing"></a>Σχόλιο και crowdsourcing
Όλοι έχουν γνώμη. Και αυτό είναι ένα καλό.
Κατάλογος δεδομένων αναγνωρίζει ότι διαφορετικούς χρήστες έχουν διαφορετικές προοπτικές σε προελεύσεις δεδομένων για μεγάλες επιχειρήσεις και ότι κάθε μία από αυτές τις προοπτικές μπορεί να είναι πολύτιμη. Εξετάστε το ακόλουθο σενάριο:

* Ο διαχειριστής του συστήματος γνωρίζει επιπέδου σύμβαση παροχής υπηρεσιών για τα διακομιστών ή τις υπηρεσίες που φιλοξενούν το αρχείο προέλευσης δεδομένων.
* Ο διαχειριστής της βάσης δεδομένων γνωρίζει το χρονοδιάγραμμα αντιγράφων ασφαλείας για κάθε βάση δεδομένων και το windows επιτρεπόμενων ETL επεξεργασίας.
* Ο κάτοχος του συστήματος γνωρίζει τη διαδικασία για τους χρήστες για να ζητήσετε πρόσβαση στην προέλευση δεδομένων.
* Το επιμελητείας δεδομένων γνωρίζει πώς αντιστοιχίζονται τα περιουσιακά στοιχεία και χαρακτηριστικά στο αρχείο προέλευσης δεδομένων στο μοντέλο δεδομένων για μεγάλες επιχειρήσεις.
* Τον αναλυτή γνωρίζει πώς χρησιμοποιούνται τα δεδομένα στο περιβάλλον του επιχειρηματικών διαδικασιών αυτός υποστηρίζει.

Κάθε μία από αυτές τις προοπτικές είναι χρήσιμη και Κατάλογος δεδομένων χρησιμοποιεί μια προσέγγιση crowdsourcing στα μετα-δεδομένα που επιτρέπει σε κάθε μία για να καταγράψει και να χρησιμοποιηθεί για την παροχή μια ολοκληρωμένη εικόνα των προελεύσεων δεδομένων καταχωρημένες. Χρησιμοποιώντας την πύλη του καταλόγου δεδομένων, κάθε χρήστης μπορεί να προσθέτετε και να επεξεργάζεστε δική του σχόλια, κατά τη διαδικασία μπορείτε να προβάλετε σχόλια που παρέχονται από άλλους χρήστες.

## <a name="different-types-of-annotations"></a>Διαφορετικοί τύποι σχόλια
Κατάλογος δεδομένων υποστηρίζει τους ακόλουθους τύπους σχόλια:

| Σχόλιο     | Σημειώσεις                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Φιλικό όνομα  | Το φιλικό όνομα μπορεί να παρασχεθεί στο επίπεδο περιουσιακών στοιχείων δεδομένων, για να κάνετε τα στοιχεία δεδομένων πιο εύκολα κατανοητή. Το φιλικό όνομα είναι πιο χρήσιμες όταν το όνομα της υποκείμενης αντικείμενο είναι δυσνόητη, συντετμημένη ή μην είναι κατανοητή στους χρήστες.                                                                                                                            |
| Περιγραφή    | Περιγραφές μπορεί να παρασχεθεί στο περιουσιακών στοιχείων δεδομένων και χαρακτηριστικό / επίπεδα στήλης. Περιγραφές είναι σύντομο κείμενο ελεύθερης μορφής σχόλια που περιγράφουν του χρήστη προοπτική σε παγίου δεδομένων ή της χρήσης του.                                                                                                                                                              |
| Ετικέτες (χρήστη ετικέτες)          | Ετικέτες μπορεί να παρασχεθεί στο περιουσιακών στοιχείων δεδομένων και χαρακτηριστικό / επίπεδα στήλης. Οι ετικέτες χρήστη είναι ετικέτες που ορίζονται από το χρήστη που μπορούν να χρησιμοποιηθούν για την κατηγοριοποίηση δεδομένων παγίων ή χαρακτηριστικά.                                                                                                                                                                                                    |
| Ετικέτες (Γλωσσάρι ετικέτες)          | Ετικέτες μπορεί να παρασχεθεί στο περιουσιακών στοιχείων δεδομένων και χαρακτηριστικό / επίπεδα στήλης. Γλωσσάρι ετικέτες είναι κεντρικά Γλωσσάρι τους όρους που μπορούν να χρησιμοποιηθούν για την κατηγοριοποίηση δεδομένων παγίων ή χαρακτηριστικά χρησιμοποιώντας μια ταξινόμηση κοινές επιχειρήσεις. Για περισσότερες πληροφορίες, δείτε [πώς μπορείτε να ρυθμίσετε το Business Γλωσσάρι για την προσθήκη ετικετών σε διέπεται](data-catalog-how-to-business-glossary.md)                                                                                                                                                                                                    |
| Τους ειδικούς        | Ειδικούς μπορεί να παρασχεθεί στο επίπεδο περιουσιακών στοιχείων δεδομένων. Ειδικούς προσδιορίστε χρήστες ή ομάδες με expert προοπτικές στα δεδομένα και μπορεί να χρησιμοποιηθεί ως σημείων επαφής για τους χρήστες που Ανακαλύψτε τις προελεύσεις δεδομένων καταχωρημένων και να έχετε ερωτήσεις που απαντώνται δεν από τα υπάρχοντα σχόλια.  |
| Αίτηση για πρόσβαση | Αίτηση πρόσβασης πληροφορίες μπορεί να παρασχεθεί στο επίπεδο περιουσιακών στοιχείων δεδομένων. Αυτές οι πληροφορίες είναι για χρήστες που έχουν Ανακαλύψτε το αρχείο προέλευσης δεδομένων που δεν έχουν ακόμα έχουν δικαιώματα πρόσβασης. Οι χρήστες να εισαγάγετε τη διεύθυνση ηλεκτρονικού ταχυδρομείου του χρήστη ή της ομάδας που εκχωρεί πρόσβαση, τη διεύθυνση URL της διεργασίας ή το εργαλείο που χρειάζονται οι χρήστες για να αποκτήσετε πρόσβαση, ή να εισαγάγετε τη διαδικασία ίδια ως κείμενο. |
| Τεκμηρίωση | Τεκμηρίωση μπορεί να παρασχεθεί στο επίπεδο περιουσιακών στοιχείων δεδομένων. Η τεκμηρίωση περιουσιακού στοιχείου είναι πληροφορίες εμπλουτισμένου κειμένου που μπορούν να περιλαμβάνουν συνδέσεις και εικόνες και, που μπορούν να παρέχουν τις πληροφορίες που δεν μεταφέρονται μέσω περιγραφές και οι ετικέτες. |


## <a name="annotating-multiple-assets"></a>Δημιουργία σχολίων σε πολλά στοιχεία
Όταν επιλέξετε πολλά στοιχεία δεδομένων στην πύλη του καταλόγου δεδομένων, οι χρήστες μπορεί να προσθέτει σχόλια όλων των επιλεγμένων παγίων με μία λειτουργία. Σχόλια θα ισχύουν για όλα τα επιλεγμένα περιουσιακά στοιχεία, διευκολύνοντας την επιλογή και δώστε μια περιγραφή συνεπή και τα σύνολα των ετικετών και τους ειδικούς για στοιχεία σχετικά δεδομένα.

> [AZURE.NOTE] Ετικέτες και τους ειδικούς μπορεί να παρέχεται επίσης όταν καταχώρησης δεδομένων παγίων με τα δεδομένα του καταλόγου δεδομένων προέλευσης εργαλείο εγγραφής.

Όταν η επιλογή πολλών πινάκων και προβολών, μόνο οι στήλες ότι όλα τα επιλεγμένα στοιχεία έχουν κοινά δεδομένα θα εμφανίζονται στην πύλη του καταλόγου δεδομένων. Αυτό σας επιτρέπει στους χρήστες να παρέχουν οι ετικέτες και οι περιγραφές για όλες τις στήλες με το ίδιο όνομα για όλα τα επιλεγμένα πάγια.

## <a name="annotations-and-discovery"></a>Σχόλια και εντοπισμού
Όπως ακριβώς τα μετα-δεδομένα που έχουν εξαχθεί από το αρχείο προέλευσης δεδομένων κατά την εγγραφή προστίθεται στο ευρετήριο αναζήτησης του καταλόγου δεδομένων, μετα-δεδομένων που παρέχεται από το χρήστη είναι επίσης στο ευρετήριο. Αυτό σημαίνει ότι δεν είναι μόνο για σχόλια διευκολύνουν τους χρήστες να κατανοήσουν τα δεδομένα που έχουν εντοπισμού, σχόλια διευκολύνουν επίσης για τους χρήστες για να ανακαλύψετε των περιουσιακών στοιχείων με σχόλια δεδομένων κάνοντας αναζήτηση με χρήση των όρων που έχει νόημα σε αυτά.

## <a name="summary"></a>Σύνοψη
Εγγραφή για μια προέλευση δεδομένων με τον κατάλογο δεδομένων κάνει αυτά τα δεδομένα ανιχνεύσιμα αντιγράφοντας δομικά και περιγραφικό μετα-δεδομένα από το αρχείο προέλευσης δεδομένων στην υπηρεσία καταλόγου. Αφού καταχωρηθεί ένα αρχείο προέλευσης δεδομένων, οι χρήστες μπορούν να παρέχουν σχόλια για να διευκολύνετε την να ανακαλύψετε και να κατανοήσετε από εντός της πύλης του καταλόγου δεδομένων.

## <a name="see-also"></a>Δείτε επίσης
- [Γρήγορα αποτελέσματα με τον κατάλογο δεδομένων Azure](data-catalog-get-started.md) πρόγραμμα εκμάθησης για αναλυτικές πληροφορίες σχετικά με τον τρόπο για να προσθέσετε σχόλια σε προελεύσεις δεδομένων.
