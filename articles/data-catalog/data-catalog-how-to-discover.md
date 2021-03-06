<properties
   pageTitle="Πώς μπορείτε να τον εντοπισμό προελεύσεων δεδομένων | Microsoft Azure"
   description="Το άρθρο επισήμανση πώς μπορείτε να ανακαλύψετε καταχωρημένες δεδομένων στοιχείων με Azure καταλόγου δεδομένων, όπως η αναζήτηση και το φιλτράρισμα και χρησιμοποιώντας την ακρίβεια επισήμανση δυνατότητες από την πύλη του καταλόγου δεδομένων του Azure οδηγίες."
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
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="how-to-discover-data-sources"></a>Πώς μπορείτε να τον εντοπισμό προελεύσεων δεδομένων

## <a name="introduction"></a>Εισαγωγή
**Κατάλογος δεδομένων Microsoft Azure** είναι μια υπηρεσία cloud πλήρως διαχειριζόμενων που χρησιμοποιείται ως ενός συστήματος ταξινόμησης και το σύστημα του εντοπισμού για προελεύσεις δεδομένων για μεγάλες επιχειρήσεις. Με άλλα λόγια, **Καταλόγου Azure δεδομένων** είναι όλες οι πληροφορίες για βοήθεια στην άτομα ανακαλύψετε, να κατανοήσετε και να χρησιμοποιήσετε προελεύσεις δεδομένων και παροχή βοήθειας εταιρείες για να λάβετε περισσότερες τιμή από τα υπάρχοντα δεδομένα. Όταν μια προέλευση δεδομένων έχει καταχωρηθεί με **Azure κατάλογο δεδομένων**, τα μετα-δεδομένα είναι στο ευρετήριο από την υπηρεσία, έτσι ώστε οι χρήστες εύκολα να κάνετε αναζήτηση για να εντοπίσετε τα δεδομένα που χρειάζονται.

## <a name="searching-and-filtering"></a>Αναζήτηση και το φιλτράρισμα

Εντοπισμού στον **Κατάλογο δεδομένων Azure** χρησιμοποιεί δύο πρωτεύοντα μηχανισμούς: αναζήτηση και το φιλτράρισμα.

Αναζήτηση έχει σχεδιαστεί ώστε να είναι και διαισθητική και ισχυρές-από προεπιλογή, αντιστοιχίζονται όρους αναζήτησης με οποιαδήποτε ιδιότητα στον κατάλογο, συμπεριλαμβανομένων των σχολίων που παρέχονται από χρήστη.

Φιλτράρισμα έχει σχεδιαστεί για τη συμπλήρωση της αναζήτησης. Οι χρήστες μπορούν να επιλέξουν συγκεκριμένα χαρακτηριστικά όπως ειδικούς, τύπος προέλευσης δεδομένων, τύπος αντικειμένου και ετικέτες, για να προβάλετε μόνο τα αντίστοιχα στοιχεία δεδομένων και για να περιορίσετε τα αποτελέσματα αναζήτησης για να αντιστοίχιση καθώς και στοιχεία.

Με τη χρήση συνδυασμού αναζήτηση και το φιλτράρισμα, οι χρήστες να περιηγηθείτε γρήγορα τις προελεύσεις δεδομένων που έχουν καταχωρηθεί με τον **Κατάλογο δεδομένων Azure** για να ανακαλύψετε τις προελεύσεις δεδομένων που χρειάζονται.

## <a name="search-syntax"></a>Σύνταξη αναζήτησης

Παρόλο που η προεπιλεγμένη αναζήτηση ελεύθερου κειμένου είναι απλό και διαισθητική, οι χρήστες επίσης να χρησιμοποιήσετε σύνταξη αναζήτηση **Καταλόγου δεδομένων του Azure**του να έχει μεγαλύτερο έλεγχο σε αποτελέσματα αναζήτησης. Αναζήτηση **Καταλόγου δεδομένων Azure** υποστηρίζει τις εξής τεχνικές:

| Τεχνική                 | Χρήση                                                                                                                                     | Παράδειγμα                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Βασική αναζήτηση              | Βασική αναζήτηση χρησιμοποιώντας έναν ή περισσότερους όρους αναζήτησης. Τα αποτελέσματα είναι οποιαδήποτε στοιχεία που αντιστοιχούν σε οποιαδήποτε ιδιότητα με ένα ή περισσότερα από τους όρους που καθορίζονται. | δεδομένα πωλήσεων                                                |
| Ιδιότητα εμβέλεια          | Επιστρέφει μόνο τις προελεύσεις δεδομένων όπου είναι αντιστοιχισμένος τον όρο αναζήτησης με την καθορισμένη ιδιότητα                                                   | όνομα: οικονομικό                                              |
| Οι τελεστές Boolean         | Διεύρυνση ή τον περιορισμό μια αναζήτηση χρησιμοποιώντας δυαδική λειτουργίες                                                                                     | οικονομικό δεν εταιρικό                                     |
| Ομαδοποίηση με παρένθεση | Χρήση παρενθέσεων σε ομάδα τμήματα του ερωτήματος για να επιτύχετε λογική απομόνωσης, ιδίως σε συνδυασμό με τελεστές Boolean              | όνομα: οικονομικό AND (ετικέτες: τρ1 ή ετικέτες: Τρ2) |
| Τελεστές σύγκρισης      | Χρήση συγκρίσεις εκτός από ισότητας για τις ιδιότητες που έχουν τύποι δεδομένων αριθμητικό και ημερομηνία                                                | modifiedTime > "05/11/2014"                                 |

Για περισσότερες πληροφορίες σχετικά με την αναζήτηση **Καταλόγου Azure δεδομένων** , ανατρέξτε στο θέμα [https://msdn.microsoft.com/library/azure/mt267594.aspx](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="hit-highlighting"></a>Πατήσετε επισήμανσης
Κατά την προβολή αποτελεσμάτων αναζήτησης, θα επισημανθούν οποιαδήποτε εμφανιζόμενη ιδιότητες που ταιριάζουν με τους καθορισμένους όρους αναζήτησης – όπως το όνομα δεδομένων περιουσιακού στοιχείου, περιγραφή και ετικέτες – για να σας διευκολύνουν να προσδιορίσετε γιατί μια δεδομένη δεδομένων περιουσιακών στοιχείων επιστράφηκε από μια δεδομένη αναζήτηση.

> [AZURE.NOTE] Οι χρήστες να ενεργοποιήσετε επισκέψεων επισήμανση εκτός αν θέλετε, χρησιμοποιώντας το διακόπτη "Επισήμανση" στην πύλη του **Καταλόγου δεδομένων του Azure** .

Κατά την προβολή αποτελεσμάτων αναζήτησης, μπορεί να μην είναι πάντα εμφανή γιατί ενός περιουσιακού στοιχείου δεδομένων περιλαμβάνεται, ακόμα και με επισήμανση επισκέψεων με δυνατότητα. Επειδή όλες οι ιδιότητες γίνεται αναζήτηση από προεπιλογή, μια περιουσιακών στοιχείων δεδομένων ενδέχεται να επιστραφούν λόγω συμφωνία με βάση μια ιδιότητα επιπέδου στηλών. Και επειδή πολλοί χρήστες μπορεί να προσθέτει σχόλια καταχωρημένες δεδομένων παγίων με τις δικές τους ετικέτες και περιγραφές, όχι με όλα μετα-δεδομένων ενδέχεται να εμφανίζεται στη λίστα των αποτελεσμάτων αναζήτησης.

Στην προεπιλεγμένη προβολή σε παράθεση, κάθε πλακίδιο εμφανίζεται στα αποτελέσματα αναζήτησης θα περιλαμβάνει ένα εικονίδιο "Προβολή όρος αναζήτησης ταιριάζει με", το οποίο επιτρέπει στο χρήστη για να προβάλετε γρήγορα τον αριθμό των συμφωνίες και τη θέση τους και για να μεταπηδήσετε σε αυτούς, εάν θέλετε.

 ![Πατήστε επισήμανση και αναζήτησης αντιστοιχεί στην πύλη του καταλόγου δεδομένων του Azure](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Σύνοψη
Εγγραφή για μια προέλευση δεδομένων με **Τον κατάλογο δεδομένων Azure** διευκολύνει την αυτή την προέλευση δεδομένων για τον εντοπισμό και την κατανόηση, αντιγράφοντας δομικά και περιγραφικό μετα-δεδομένα από το αρχείο προέλευσης δεδομένων στην υπηρεσία καταλόγου. Αφού καταχωρηθεί ένα αρχείο προέλευσης δεδομένων, οι χρήστες να μπορούν να εντοπίζουν χρησιμοποιώντας φιλτράρισμα και αναζήτηση από την πύλη του **Καταλόγου δεδομένων του Azure** .

## <a name="see-also"></a>Δείτε επίσης
- [Γρήγορα αποτελέσματα με τον κατάλογο δεδομένων Azure](data-catalog-get-started.md) πρόγραμμα εκμάθησης για αναλυτικές πληροφορίες σχετικά με τον τρόπο για να ανακαλύψετε προελεύσεις δεδομένων.
