<properties
    pageTitle="Ροή ανάλυσης δεδομένων λίμνης Store εξόδου | Microsoft Azure"
    description="Ρύθμιση παραμέτρων ελέγχου ταυτότητας και εξουσιοδότησης μια Azure χώρου αποθήκευσης λίμνης δεδομένων σε μια εργασία ροής ανάλυσης"
    keywords=""
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="stream-analytics-data-lake-store-output"></a>Ροή ανάλυσης δεδομένων λίμνης Store εξόδου

Ροή εργασιών ανάλυση υποστηρίζει διάφορες μεθόδους εξόδου, μία που έχει τεθεί σε ένα [Χώρο αποθήκευσης λίμνης Azure δεδομένων](https://azure.microsoft.com/services/data-lake-store/). Χώρος αποθήκευσης λίμνης Azure δεδομένων είναι ένα αποθετήριο hyper κλίμακας εταιρικές για μεγάλο δεδομένων αναλυτικό φόρτους εργασίας. Χώρος αποθήκευσης δεδομένων λίμνης σάς επιτρέπει να αποθηκεύσετε δεδομένα από οποιαδήποτε ταχύτητα μέγεθος, τον τύπο και κατάποσης για λειτουργικές και διερευνητικές ανάλυση.

## <a name="authorize-a-data-lake-store-account"></a>Εγκρίνετε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης

1.  Όταν είναι επιλεγμένο χώρο αποθήκευσης λίμνης δεδομένων ως το αποτέλεσμα στην πύλη διαχείρισης Azure, θα σας ζητηθεί να επιτρέπουν τη χρήση των τον υπάρχοντα χώρο αποθήκευσης λίμνης δεδομένων ή να ζητήσετε πρόσβαση σε προεπισκόπηση δεδομένων λίμνης αποθήκευσης μέσω της πύλης κλασική Azure.

    ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  

2.  Εάν έχετε ήδη πρόσβαση στο χώρο αποθήκευσης λίμνης δεδομένων, κάντε κλικ στην επιλογή "Εγκρίνετε τώρα" και για μια σύντομη ώρα μια σελίδα θα εμφανιστεί που υποδεικνύει ότι "Ανακατεύθυνση άδειας...". Η σελίδα θα κλείσει αυτόματα και θα εμφανιστεί με τη σελίδα που θα σας επιτρέπει να ρυθμίσετε τις παραμέτρους του χώρου αποθήκευσης λίμνης δεδομένων εξόδου.

Εάν που δεν έχετε εγγραφεί για προεπισκόπηση Store λίμνης δεδομένων, που μπορούν να ακολουθήσετε τη σύνδεση "Εγγραφείτε τώρα" για να ξεκινήσετε την αίτηση ή ακολουθήστε τα [Γρήγορα αποτελέσματα οδηγίες](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-the-data-lake-store-output-properties"></a>Ρυθμίστε τις ιδιότητες του χώρου αποθήκευσης λίμνης δεδομένων εξόδου

Όταν έχετε το λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης με έλεγχο ταυτότητας, μπορείτε να ρυθμίσετε τις ιδιότητες για το αποτέλεσμα του χώρου αποθήκευσης δεδομένων λίμνης. Ο παρακάτω πίνακας είναι στη λίστα ονόματα ιδιοτήτων και την περιγραφή για τη ρύθμιση παραμέτρων του χώρου αποθήκευσης λίμνης δεδομένων εξόδου.

<table>
<tbody>
<tr>
<td><B>ΌΝΟΜΑ ΙΔΙΌΤΗΤΑΣ</B></td>
<td><B>ΠΕΡΙΓΡΑΦΉ</B></td>
</tr>
<tr>
<td>Ψευδώνυμο εξόδου</td>
<td>Αυτό είναι ένα φιλικό όνομα που χρησιμοποιείται σε ερωτήματα για να κατευθύνετε το αποτέλεσμα του ερωτήματος σε αυτόν το χώρο αποθήκευσης λίμνης δεδομένων.</td>
</tr>
<tr>
<td>Χώρος αποθήκευσης δεδομένων λίμνης λογαριασμού</td>
<td>Το όνομα του λογαριασμού χώρου αποθήκευσης όταν στέλνετε εξόδου σας. Θα εμφανιστεί με μια αναπτυσσόμενη λίστα λογαριασμών χώρου αποθήκευσης λίμνης δεδομένων στο οποίο ο χρήστης πραγματοποιήσει είσοδο στην πύλη έχει πρόσβαση σε.</td>
</tr>
<tr>
<td>Μοτίβο πρόθεμα διαδρομή που είναι [<I>προαιρετικό</I>]</td>
<td>Η διαδρομή του αρχείου που χρησιμοποιείται για να γράψετε τα αρχεία σας στο καθορισμένο λογαριασμό λίμνης χώρου αποθήκευσης δεδομένων. <BR>{date} {χρόνος}<BR>Παράδειγμα 1: Φάκελος_1/αρχεία καταγραφής / {date} / {time}<BR>Παράδειγμα 2: Φάκελος_1/αρχεία καταγραφής / {date}</td>
</tr>
<tr>
<td>Μορφή ημερομηνίας [<I>προαιρετικό</I>]</td>
<td>Εάν το διακριτικό ημερομηνία χρησιμοποιείται στη διαδρομή πρόθεμα, μπορείτε να επιλέξετε τη μορφή ημερομηνίας στην οποία είναι οργανωμένα τα αρχεία σας. Παράδειγμα: Εεεε/μμ/ηη</td>
</tr>
<tr>
<td>Μορφή ώρας [<I>προαιρετικό</I>]</td>
<td>Εάν το διακριτικό ώρας χρησιμοποιείται στη διαδρομή πρόθεμα, καθορίστε τη μορφή ώρας στην οποία είναι οργανωμένα τα αρχεία σας. Προς το παρόν η μόνη τιμή υποστηριζόμενες είναι HH.</td>
</tr>
<tr>
<td>Μορφή σειριοποίησης συμβάντος</td>
<td>Μορφή σειριοποίησης για τα δεδομένα εξόδου. Υποστηρίζονται JSON, CSV και Avro.</td>
</tr>
<tr>
<td>Κωδικοποίηση</td>
<td>Εάν σε μορφή CSV ή JSON, κωδικοποίηση πρέπει να καθοριστούν. UTF-8 είναι η μόνη υποστηριζόμενη μορφή κωδικοποίησης αυτήν τη στιγμή.</td>
</tr>
<tr>
<td>Οριοθέτη</td>
<td>Ισχύει μόνο για την σειριοποίησης CSV. Ανάλυση ροή υποστηρίζει έναν αριθμό κοινών οριοθέτες για σειριοποίησης δεδομένα CSV. Υποστηριζόμενες τιμές είναι κόμμα, ελληνικό ερωτηματικό, κενό διάστημα, tab και κατακόρυφη γραμμή.</td>
</tr>
<tr>
<td>Μορφή</td>
<td>Ισχύει μόνο για την JSON σειριοποίησης. Γραμμή διαχωρισμένες υποδεικνύει ότι το αποτέλεσμα θα μορφοποιηθεί με από κάθε αντικείμενο JSON διαχωρισμένα με μια νέα γραμμή. Ο πίνακας καθορίζει ότι το αποτέλεσμα θα μορφοποιηθεί ως πίνακας JSON αντικειμένων.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Ανανέωση του χώρου αποθήκευσης δεδομένων λίμνης εξουσιοδότησης

Προς το παρόν, υπάρχει περιορισμός όπου το διακριτικό ελέγχου ταυτότητας πρέπει να ανανεωθεί με μη αυτόματο τρόπο κάθε 90 ημέρες για όλες τις εργασίες με το χώρο αποθήκευσης λίμνης δεδομένων εξόδου. Θα πρέπει επίσης να εκ νέου τον έλεγχο ταυτότητας του λογαριασμού χώρου αποθήκευσης δεδομένων λίμνης εάν έχετε αλλάξει τον κωδικό πρόσβασής σας καθώς το έργο έχει δημιουργηθεί ή τελευταία ελεγχθεί η ταυτότητά. Ένα σύμπτωμα αυτού του ζητήματος είναι κανένα αποτέλεσμα εργασίας και ένα σφάλμα στα αρχεία καταγραφής λειτουργία που υποδεικνύει ότι χρειάζεται για την εκ νέου άδεια.

Για να επιλύσετε αυτό το ζήτημα, διακόψετε την εργασία που εκτελείται και μεταβείτε το αποτέλεσμα του χώρου αποθήκευσης δεδομένων λίμνης. Κάντε κλικ στη σύνδεση "Ανανέωσης εξουσιοδότησης" και για μια σύντομη ώρα μια σελίδα θα εμφανιστεί που υποδεικνύει ότι "Ανακατεύθυνση άδειας...". Η σελίδα θα κλείσει αυτόματα και εάν είναι επιτυχής, θα υποδηλώνουν "Εξουσιοδότησης έχει γίνει με επιτυχία Ανανέωση". Μπορείτε, στη συνέχεια, πρέπει να κάντε κλικ στην επιλογή "Αποθήκευση" στο κάτω μέρος της σελίδας, και να συνεχίσετε με την επανεκκίνηση εργασίας από την τελευταία φορά διακόπηκε για να αποφύγετε την απώλεια δεδομένων.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)