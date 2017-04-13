<properties
   pageTitle="Δοκιμές την προσφορά της υπηρεσίας δεδομένων για το Marketplace | Microsoft Azure"
   description="Κατανόηση πώς μπορείτε να δοκιμάσετε την προσφορά της υπηρεσίας δεδομένων για το Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="testing-your-data-service-offer-in-staging"></a>Δοκιμή την προσφορά της υπηρεσίας δεδομένων σε ενδιάμεσου σταδίου

>[AZURE.IMPORTANT] **Αυτήν τη στιγμή δεν είναι πλέον προσθήκης λογαριασμών οποιαδήποτε νέα εκδότες υπηρεσία δεδομένων. Δεν θα λάβετε εγκριθεί νέα dataservices για τη λίστα.** Εάν έχετε μια εφαρμογή επιχειρήσεις ΑΔΑ που θέλετε να δημοσιεύσετε στο AppSource μπορείτε να βρείτε περισσότερες πληροφορίες [εδώ](https://appsource.microsoft.com/partners). Εάν έχετε μια εφαρμογές IaaS ή προγραμματιστής υπηρεσία που θέλετε να δημοσιεύσετε στο Azure Marketplace, μπορείτε να βρείτε περισσότερες πληροφορίες [εδώ](https://azure.microsoft.com/marketplace/programs/certified/).

Μετά την ολοκλήρωση τα πρώτα δύο βήματα με [τη δημιουργία του λογαριασμού Microsoft Developer](marketplace-publishing-accounts-creation-registration.md) και [τη δημιουργία σας προσφέρουν υπηρεσίας δεδομένων στην πύλη δημοσιεύσεων](marketplace-publishing-data-service-creation.md) είστε έτοιμοι για να κάνετε διαθέσιμη την προσφορά από το Azure Marketplace. Αυτό το θέμα θα σας καθοδηγήσει την πρώτη, ενδιάμεσου, βήμα που ονομάζεται "Οργάνωση"

Ενδιάμεσου σταδίου σημαίνει ότι η προσφορά σε μια ιδιωτική "φίλτρου" όπου μπορείτε να ελέγξετε και να επιβεβαιώσετε τη λειτουργία πριν από την προώθηση σε παραγωγής για την ανάπτυξη. Η προσφορά θα εμφανίζεται σε ενδιάμεσου όπως ακριβώς όπως θα εμφανίζεται σε πελάτη που έχει αναπτυχθεί το.

## <a name="step-1-pushing-your-offer-to-staging"></a>Βήμα 1. Ώθηση την προσφορά για να ενδιάμεσου σταδίου
Ώθηση την προσφορά για να ενδιάμεσου σταδίου σάς επιτρέπει να ελέγξετε την προσφορά πριν θα είναι διαθέσιμο για τους συνδρομητές μελλοντική.  Μπορείτε να δείτε πώς η προσφορά θα εμφανίζονται και λειτουργεί για εκείνους εγγραφή των δεδομένων σας.  

  ![σχέδιο](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1.  Σύνδεση σε τη [Δημοσίευση πύλη](https://publish.windowsazure.com)
2.  Επιλέξτε **Υπηρεσίες δεδομένων** στο αριστερό παράθυρο περιήγησης
3.  Επιλέξτε την προσφορά που θέλετε να προωθήσετε σε ενδιάμεσου σταδίου. Θα εμφανιστεί η οθόνη παραπάνω.
4.  Κάντε κλικ στο κουμπί **Push για να ενδιάμεσου σταδίου** .  
5.  Εάν υπάρχουν προβλήματα με την προσφορά που χρειάζεται να ολοκληρωθούν πριν από την προώθηση σε ενδιάμεσου σταδίου, θα δείτε μια λίστα που εμφανίζεται.  Διορθώστε αυτά τα στοιχεία, κάνοντας κλικ σε κάθε στοιχείο της λίστας. Όταν κάνει διορθώσεις όλα, κάντε κλικ στο κουμπί **προωθήσετε σε ενδιάμεσου σταδίου** ξανά.

Εάν υπάρχουν ζητήματα σχετικά με την προσφορά θα δείτε το παρακάτω αναδυόμενο παράθυρο.  

Εάν είστε δεν σχεδιασμού/δεν εγκριθεί για να εντοπίσει την προσφορά στην πύλη Azure (προς το παρόν έχει περιορισμένη χωρητικότητα), στη συνέχεια, απλώς κλείστε το αναδυόμενο παράθυρο.

Για να δοκιμάσετε την υπηρεσία δεδομένων στην πύλη Azure (εκτός από την πύλη DataMarket), θα χρειαστεί ένα Αναγνωριστικό συνδρομής Azure για να ελέγξετε με.  Αυτό το Αναγνωριστικό συνδρομής θα αναγνωρίζετε το λογαριασμό που θα επιτρέπεται για να ελέγξετε την προσφορά.  

Αποκοπή και επικόλληση το Αναγνωριστικό συνδρομής και κάντε κλικ το σημάδι επιλογής για να συνεχίσετε.

  ![σχέδιο](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [AZURE.NOTE] Αυτές οι συνδρομές Azure αναγνωριστικά είναι μόνο απαιτείται για τον έλεγχο και ενδιάμεσου στην [Πύλη διαχείρισης του Azure](https://manage.windowsazure.com). Δεν είναι απαραίτητες για τον έλεγχο από το Azure Marketplace.

Στην επόμενη οθόνη που εμφανίζεται δείχνει ότι η δημοσίευση πραγματοποιείται, εμφανίζοντας το εικονίδιο "σε εξέλιξη" με επισήμανση κίτρινο παρακάτω. Προώθηση σε ενδιάμεσου σταδίου μεταφέρει μεταξύ 10 έως 15 λεπτά.  Εάν διαρκεί περισσότερο, πρώτα να ανανεώσετε το πρόγραμμα περιήγησης (πατήστε F5 στο IE).  Στις σπάνιων περιπτώσεις όπου η προσφορά εξακολουθεί να προώθηση σε ενδιάμεσου σταδίου μετά από κάθε ώρα, κάντε κλικ στην επαφή μας σύνδεση για να πείτε μας γνωρίζετε ότι υπάρχει κάποιο πρόβλημα.

  ![σχέδιο](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Όταν το πάτημα σε ενδιάμεσου σταδίου ολοκληρωθεί το εικονίδιο "σε εξέλιξη" θα διακοπή μετακίνηση και την κατάσταση θα ενημερωθεί σε "Σταδιακή".  Τώρα είστε έτοιμοι να δοκιμάσετε την προσφορά.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Βήμα 2. Δοκιμή η σταδιακή προσφορά σε DataMarket

Κάντε κλικ στη σύνδεση που ακολουθεί το κείμενο **"Δείτε την υπηρεσία προσφέρουν σε..."** Για να εμφανίσετε την οθόνη που συνδρομητή θα βλέπετε όταν η προσφορά μεταβαίνει στη παραγωγής και θα εμφανιστεί στο DataMarket.

  ![σχέδιο](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Δοκιμή ή επαλήθευση καθένα από τα στοιχεία 12 έχει επισημανθεί παραπάνω για να βεβαιωθείτε ότι όλα λογότυπα, τιμές/συναλλαγές, κείμενο, εικόνες, τεκμηρίωση και οι συνδέσεις είναι σωστή και λειτουργεί σωστά.  Αυτή είναι μια καλή ώρα για να βεβαιωθείτε ότι έχουν αντικατασταθεί οποιαδήποτε τιμές δοκιμής που πληκτρολογήσατε κατά τη δημιουργία την προσφορά με πραγματικές τιμές.

1. Λογότυπο προσφορά
2. Όνομα προσφορά
3. Ο Publisher όνομα/σύνδεση στην τοποθεσία Web της εταιρείας σας
4. Κατηγορίες αναζήτησης για την προσφορά
5. Σύνδεση υποστήριξης την προσφορά για να βοηθήσει τους συνδρομητές
6. Με βάση τα συμφραζόμενα περιγραφή για την προσφορά
7. Πρόγραμμα προσφορά που απεικονίζει λεπτομέρειες χρεώσεων
8. Σύνδεση με εφαρμογή κώδικα
9. Δείγμα εικόνες που απεικονίζουν χρησιμοποιούν την προσφορά δεδομένων
10. Μετα-δεδομένα εισόδου/εξόδου για κάθε υπηρεσία εντός της προσφοράς
11. Όροι χρήσης του προσφορά
12. Προεπισκόπηση των δεδομένων την προσφορά


Τέλος, επιλέξτε την υπηρεσία θα λειτουργούν έως το Datamarket κάνοντας κλικ στη σύνδεση "ΕΞΕΡΕΎΝΗΣΗ ΑΥΤΌ σύνολο ΔΕΔΟΜΈΝΩΝ".  Θα ανοίξει ένα νέο παράθυρο του εργαλείου ονομάζουμε "Εξερεύνηση υπηρεσίας", ώστε να μπορείτε να κάνετε προεπισκόπηση των αποτελεσμάτων ενός ερωτήματος σε σχέση με την υπηρεσία.  Σε αυτό το παράθυρο, μπορείτε να εισαγάγετε τις παραμέτρους ανάγκες και να δείτε τα αποτελέσματα που εμφανίζονται από ένα ερώτημα σε σχέση με την υπηρεσία.   Επίσης, εμφανίζονται είναι η διεύθυνση URL για το ερώτημά σας.  

> [AZURE.NOTE] Φροντίστε να ελέγξετε την περιγραφή της υπηρεσίας εμφανίζεται στο επάνω μέρος σε κείμενο.  Και εάν η προσφορά αποτελείται από περισσότερες από μία υπηρεσίες κλήσεων, κάντε κλικ στις καρτέλες στο κάτω μέρος για να μεταβείτε στην επόμενη υπηρεσία να εξετάσετε και να ελέγξετε.



## <a name="next-step"></a>Επόμενο βήμα
Εάν αντιμετωπίζετε προβλήματα και χρειάζεστε βοήθεια για την επίλυση τους επικοινωνήστε με την [Υποστήριξη Publisher Azure]( http://go.microsoft.com/fwlink/?LinkId=272975).

Εάν είστε ικανοποιημένοι και είστε έτοιμοι να δημοσιεύσετε την προσφορά, διαβάστε την τεκμηρίωση [Αίτηση έγκρισης Push για παραγωγή](marketplace-publishing-push-to-production.md) .

## <a name="see-also"></a>Δείτε επίσης
- [Γρήγορα αποτελέσματα: Πώς μπορείτε να δημοσιεύσετε μια προσφορά για να το Azure Marketplace](marketplace-publishing-getting-started.md)