<properties
   pageTitle="Περιβάλλον εργασίας χρήστη Azure δέσμευση κινητές συσκευές - ανάλυση"
   description="Μάθετε πώς μπορείτε να αναλύσετε δεδομένα ιστορικού σχετικά με την εφαρμογή σας χρησιμοποιώντας Azure Mobile δέσμευση"
   services="mobile-engagement"
   documentationCenter=""
   authors="piyushjo"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile"
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="how-to-analyze-historical-data-about-your-application"></a>Πώς μπορείτε να αναλύσετε δεδομένα ιστορικού σχετικά με την εφαρμογή σας

Σε αυτό το άρθρο περιγράφονται στην καρτέλα **ΑΝΆΛΥΣΗ** της πύλης **Δέσμευση Mobile** . Μπορείτε να χρησιμοποιήσετε την πύλη **Mobile δέσμευση** να παρακολουθούν και να διαχειριστείτε τις εφαρμογές για κινητές συσκευές. Σημειώστε ότι για να ξεκινήσετε με την πύλη πρέπει πρώτα να δημιουργήσετε ένα λογαριασμό **Δέσμευση Mobile Azure** .


Στην ενότητα ανάλυση του περιβάλλοντος εργασίας Χρήστη του παρέχει συγκεντρωτική πληροφορίες σχετικά με την εφαρμογή που βασίζεται σε δεδομένα ιστορικού που είναι ενημερωθεί κάθε 24 ώρες. Οι πληροφορίες εμφανίζονται σε διαφορετικούς πίνακες εργαλείων που αποτελείται από τα γραφήματα γραμμών/γραμμή πίτας, πλέγματα και οι χάρτες. Τα δεδομένα μπορούν να ληφθούν επίσης ως αρχεία .csv. Οι περισσότερες από αυτές τις ίδιες πληροφορίες είναι διαθέσιμες σε πραγματικό χρόνο στην ενότητα οθόνη του περιβάλλοντος εργασίας Χρήστη του και αυτό επίσης είναι δυνατή η πρόσβαση από το API ανάλυσης.

>[AZURE.NOTE] Πολλές ενότητες της πύλης **Δέσμευση Mobile** περιβάλλοντος εργασίας Χρήστη περιέχουν το κουμπί **ΕΜΦΆΝΙΣΗ ΒΟΉΘΕΙΑΣ** . Πατήστε αυτό το κουμπί για να λάβετε περισσότερες πληροφορίες με βάση τα συμφραζόμενα σχετικά με μια ενότητα.

## <a name="standard-and-custom-analytics"></a>Τυπικών και προσαρμοσμένων ανάλυσης

Azure δέσμευση Mobile παρέχει ένα σύνολο βασικές, τυπική αναλυτικών πληροφοριών σχετικά με τις εφαρμογές που μπορεί να απεικονιστεί γραφικά μόλις ενοποίηση την εφαρμογή σας με το SDK. Azure δέσμευση Mobile παρέχει επίσης τη δυνατότητα για τη συλλογή πληροφοριών επιπλέον προσαρμοσμένης ανάλυσης που θέλετε σχετικά με τη συμπεριφορά σας τελικών χρηστών. Μπορείτε να το κάνετε αυτό, δημιουργώντας ένα σχέδιο ετικέτας προσαρμοσμένο "ετικέτες (πληροφορίες app)", δημιουργήθηκε από τις **Ρυθμίσεις** ώστε να δέσμευση Mobile Azure μπορεί να συλλέγει αυτή πρόσθετα δεδομένα για εσάς.



## <a name="analytics"></a>Ανάλυση
- Πίνακας εργαλείων: Εμφανίζει γενικές πληροφορίες σχετικά με τους χρήστες σας νέα και ενεργοποιεί και την εξέλιξή τους.
- Χρήστες: Οι χρήστες αναγνωρίζονται από τους αναγνωριστικό συσκευής: αυτό το αναγνωριστικό είναι μοναδικός για κάθε συσκευή (ένα νέο χρήστη είναι στην πραγματικότητα μια νέα συσκευή). Ένας χρήστης θεωρείται ως νέα σε ένα συγκεκριμένο χρονικό διάστημα εάν αυτός έχει πραγματοποιηθεί την πρώτη περίοδο λειτουργίας κατά συγκεκριμένο χρονικό διάστημα. Ένας χρήστης θεωρείται ως διατηρούνται εάν αυτός έχει πραγματοποιηθεί τουλάχιστον μία περίοδο λειτουργίας κατά τις τελευταίες 7 ημέρες. Ενεργοί χρήστες είναι οι χρήστες που κάνει τουλάχιστον μία περίοδο λειτουργίας σε μια δεδομένη περίοδο. Μπορείτε να ταξινομήσετε κατά, μηνιαία, εβδομαδιαία, ημερήσια ή ωριαία χρονικές περιόδους. Όλα τα γραφήματα είναι παρόμοιο, αλλά σας επιτρέπει να φίλτρου από διαφορετικές δυνατότητες, όπως την έκδοση της εφαρμογής σας, και, στη συνέχεια, να ταξινόμηση με βάση μια χρονική περίοδο. Τις τυπικές πληροφορίες που συλλέγονται από την ενοποίηση του SDK περιλαμβάνει τα παρακάτω: ενεργοί χρήστες, νέος χρήστης, τον αριθμό των περιόδων λειτουργίας, μήκος του κάθε περίοδο λειτουργίας, τεχνικές πληροφορίες σχετικά με τη χώρα, τοπικών, θέση, συλλαβισμού γλώσσας, συσκευές, υλικολογισμικού, δικτύου (μέσω Wi-Fi), εκδόσεις της εφαρμογής και του SDK, χρησιμοποιούνται από τους πελάτες. Αυτές οι πληροφορίες μπορούν να προβληθούν σε πραγματικό χρόνο από την ενότητα οθόνη.

> Σημείωση: Το χρονικό διάστημα είναι βάσει της ημερομηνίας από τις ρυθμίσεις συσκευής των χρηστών, ώστε ένα χρήστη του οποίου τηλεφώνου περιλαμβάνει την ημερομηνία που έχει ρυθμιστεί σωστά μπορεί να εμφανίζονται σε λάθος όλη τη διάρκεια.

- Διατήρησης: Ένας χρήστης θεωρείται ως διατηρούνται σε ένα συγκεκριμένο χρονικό διάστημα, εάν αυτός έχει πραγματοποιηθεί την πρώτη περίοδο λειτουργίας κατά συγκεκριμένο χρονικό διάστημα. Μπορείτε να αλλάξετε τα χρονικά διαστήματα κατά την οποία καταμετρούνται διατηρούνται χρήστες (και τους νέους χρήστες) σε ώρες, ημέρες, εβδομάδες ή μήνες. Την ανάλυση διατήρησης χρήστη είναι ενσωματωμένη επάνω σε ζώα. Μια κλάση είναι το σύνολο των όλους τους νέους χρήστες εντοπίζονται για μια δεδομένη περίοδο (π.χ., το σύνολο των χρηστών που εκτελεί την πρώτη περίοδο λειτουργίας κατά τη διάρκεια αυτής της περιόδου). Χρησιμοποιούμε ζώα 1 ημέρας, 2 ημερών, 4 ημερών, 7 ημερών ή 1 μήνα. Δεδομένο μιας κλάσης, 1-καθημερινά, 2-ημέρα, 4 ημερών, 7 ημερών, ή 1 μήνα, Azure κινητή δέσμευση τύπος υπολογίζει το σύνολο όλων των χρηστών που ανήκουν στις κλάσης και είστε και πάλι να ενεργό (π.χ., το σύνολο των χρηστών που έχει πραγματοποιηθεί τουλάχιστον μία περίοδο λειτουργίας κατά τη διάρκεια της περιόδου). Αυτό το σύνολο των χρηστών ονομάζεται μια έκδοση κλάσης. (Azure δέσμευση Mobile μπορεί να σας δείξει τον αριθμό των χρηστών σας εξακολουθούν να χρησιμοποιούν την εφαρμογή σας, αλλά μόνο την πλατφόρμα συγκεκριμένο χώρο αποθήκευσης μπορούν να σας τον αριθμό των χρηστών σας καταργήσατε την εφαρμογή - για παράδειγμα, GooglePlay, iTunes, Windows Store, κ.λπ.).
- Περίοδοι λειτουργίας: Μία χρήση της εφαρμογής από το χρήστη. Περίοδοι λειτουργίας δημιουργούνται από την ακολουθία των δραστηριοτήτων που έχει εκτελεστεί από τους χρήστες (μια δραστηριότητα είναι συνήθως σχετίζεται με τη χρήση των μία οθόνη της εφαρμογής, αλλά αυτό μπορεί να διαφέρει ανάλογα με τον τρόπο που το SDK έχει ενσωματωθεί στην εφαρμογή). Ένας χρήστης να εκτελέσετε μόνο ένα τη φορά: μια περίοδο λειτουργίας ξεκινά μόλις ο χρήστης ξεκινήσει την πρώτη δραστηριότητά και σταματά όταν εσείς ολοκληρώσει την τελευταία δραστηριότητά. Εάν ένας χρήστης να είναι περισσότερες από μερικά δευτερόλεπτα χωρίς να εκτελέσει οποιαδήποτε δραστηριότητα, στη συνέχεια, την ακολουθία των δραστηριοτήτων χωρίζεται σε δύο ξεχωριστές περιόδους λειτουργίας.
- Δραστηριότητες: Τα ονόματα των κάθε οθόνης στην εφαρμογή σας και τη διάρκεια του χρόνου χρήστες αφιερώνετε σε κάθε οθόνη. Οι δραστηριότητες είναι μια προσαρμοσμένη επιλογή ανάλυσης που θα αντιστοιχούν με τις ετικέτες "πληροφορίες εφαρμογής" που έχετε ορίσει για τη δική σας εφαρμογή:
- Διαδρομή χρήστη: Εμφανίζει πώς η περιήγηση τους χρήστες σας μέσω της εφαρμογής σας δραστηριότητες (οθόνες). Μπορείτε να μετακινήσετε το ρυθμιστικό για να προσαρμόσετε το επίπεδο λεπτομερειών. Μπλε κόμβους αντιπροσωπεύουν δραστηριότητες της εφαρμογής σας. Το μέγεθος είναι ανάλογο με το χρόνο που αναλώθηκε χρήστες σε αυτό. Λευκή κόμβους αντιπροσωπεύει την περίοδο λειτουργίας έναρξης και τερματισμού. Κόκκινο κόμβους αντιπροσωπεύουν παρουσιάσει σφάλμα. Συνδέσεις αντιπροσωπεύουν μεταβάσεων μεταξύ των δραστηριοτήτων της εφαρμογής σας (ή μεταξύ των δραστηριοτήτων και παρουσιάζει σφάλμα). Κάντε κλικ σε έναν κόμβο ή μια σύνδεση για να εμφανίσετε μια συμβουλή εργαλείου με περισσότερες πληροφορίες σχετικά με τα δεδομένα σας: ο χρόνος που αναλώθηκε σε μια συγκεκριμένη οθόνη, το πλήθος των μεταβάσεων και το ποσοστό της μεταβάσεις από την προέλευση της δραστηριότητας στη δραστηριότητα προορισμού. (Μια---60%---> B σημαίνει ότι οι χρήστες σχετικά με τη δραστηριότητα A μεταβαίνει στη δραστηριότητα B 60% του χρόνου.) Να αναδιοργάνωση του γραφήματος όπως θέλετε να διευκρινίσετε. τη θέση του αποθηκεύεται κάθε φορά που κάνετε μια αλλαγή. Μπορείτε να εμφανίσετε ή να αποκρύψετε το παρουσιάζει σφάλμα να ανοίξετε το γράφημα.
- Συμβάντα: Συγκεκριμένες ενέργειες που λαμβάνονται από το χρήστη στην εφαρμογή. Η κατανομή των συμβάντων εμφανίζεται ως το πλήθος των συμβάντων ανά χρήστη ανά περίοδο λειτουργίας. Ένα συμβάν αντιπροσωπεύει μια άμεση ενέργεια, για παράδειγμα, ένα κλικ σε ένα κουμπί ή την παραλαβή της ειδοποίησης. (Την έννοια του συμβάντα εξαρτάται από τον τρόπο στο SDK έχει ενσωματωθεί στην εφαρμογή.) Ένα συμβάν μπορεί να προκύψουν κατά τη διάρκεια μιας περιόδου λειτουργίας ή μια εργασία ή μπορεί να είναι αυτόνομη.
- Εργασίες: Παρόμοια με συμβάντα εκτός από αυτά εστίαση σε το μήκος της ενέργειας. Για παράδειγμα, οι εργασίες μπορεί να σας ενημερώσει τεχνικές πληροφορίες σχετικά με το χρονικό διάστημα που απαιτείται περιεχομένου για τη φόρτωση ή μια κλήση στην υπηρεσία web. Επίσης, αυτό μπορεί να εμφανιστεί το χρόνο που χρειάζεται ένας χρήστης να Συμπλήρωση φόρμας, δημιουργήστε ένα λογαριασμό ή να κάνετε μια αγορά. Μια εργασία αντιπροσωπεύει τη διάρκεια μιας εργασίας, για παράδειγμα, η διάρκεια της εργασίας λήψη ή την ώρα ένα πλαίσιο εμφανίζεται στην οθόνη. (Την έννοια των εργασιών εξαρτάται από τον τρόπο στο SDK έχει ενσωματωθεί στην εφαρμογή.) Οι εργασίες είναι συνήθως σχετίζεται με εργασίες στο παρασκήνιο που εκτελούνται εκτός της εμβέλειας μιας περιόδου λειτουργίας (π.χ., χωρίς οποιαδήποτε δραστηριότητα χρήστη).
- Technicals: Τεχνικές πληροφορίες σχετικά με τις συσκευές των χρηστών από την εφαρμογή που μπορείτε να παρακολουθείτε, όπως το τοπικές ρυθμίσεις, συλλαβισμού, δικτύου, τη συσκευή, υλικολογισμικό, και οθόνης μεγέθους των συσκευών των χρηστών και την έκδοση της εφαρμογής σας και την έκδοση SDK χρησιμοποιείται στην εφαρμογή.
- Σφάλματα: Πληροφορίες σχετικά με την τεχνική σφάλματα μέσα στην εφαρμογή που δεν προκαλούν την εφαρμογή για να παρουσιάσει σφάλμα. Σφάλμα αντιπροσωπεύει άμεσα πρόβλημα, για παράδειγμα, μια αποτυχία δικτύου ή μια εσφαλμένη χειρισμό. (Την έννοια του συμβάντα εξαρτάται από τον τρόπο στο SDK έχει ενσωματωθεί στην εφαρμογή.) Ένα σφάλμα μπορεί να προκύψουν κατά τη διάρκεια μιας περιόδου λειτουργίας ή μια εργασία ή μπορεί να είναι αυτόνομη.
- Παρουσιάζει σφάλμα: Πληροφορίες σχετικά με σφάλματα που προκαλούν την εφαρμογή σας για να παρουσιάσει σφάλμα. Σφάλμα είναι μια μη αναμενόμενη κατάσταση όπου η εφαρμογή διακόπτει την εκτέλεση λειτουργιών αναμενόμενο και πρέπει να διακοπεί. Σφάλμα οφείλεται συνήθως σε ένα σφάλμα στην εφαρμογή.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Πρόσβαση σε την επισκόπηση διατήρησης
![Analytics3][12]

Η επισκόπηση διατήρησης είναι εσφαλμένος προς τα κάτω στη μέση σε διάφορες κάρτες, κάθε που εμφανίζει την επισκόπηση για συγκεκριμένο χρονικό διάστημα διατήρησης. Η περίοδος διατήρησης 2 ημερών είναι ορατή στο παράδειγμα. Οι άλλες κάρτες εμφανίζουν τις περιόδους διατήρησης 4 ημερών και 7 ημερών.

## <a name="understanding-the-retention-overview-cards"></a>Κατανόηση των καρτών Επισκόπηση διατήρησης
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Κάθε κάρτα αποτελείται από 3 κύρια μέρη:
1. 1: θεωρούνται κλάσης και της περιόδου
2. 2-4: το διατήρησης για την τρέχουσα περίοδο
3. 5: ένα γράφημα Sparkline του ιστορικού

### <a name="here-is-detailed-information-about-each-element"></a>Ακολουθεί λεπτομερείς πληροφορίες σχετικά με κάθε στοιχείο:
1.    Κλάση και περίοδο: αυτό τίτλος παρέχει τον τύπο της κλάση. Εδώ "2 ημερών περίοδο" σημαίνει ότι θα εξετάσουμε τη συμπεριφορά των χρηστών πάνω από 2 ημέρες, οι χρήστες που έφτασαν σε μια περίοδο 2 ημέρες και, εάν που συνδέονται με την παρακάτω μπλοκ 2 ημερών. Το παραπάνω παράδειγμα θεωρεί τη δραστηριότητα των χρηστών μεταξύ του 21η και 22 Νοεμβρίου.
2.    Το αποτέλεσμα είναι το επιτόκιο διατήρησης πάνω από την 21 και 22 Νοεμβρίου για τους χρήστες που παραδίδονται σε 19 και 20 Νοεμβρίου. Εδώ θα σας είχε 1 του ενεργού χρήστη μεταξύ του 21η και 22, επάνω από το 3 που έχουν νέων χρηστών μεταξύ του 19th και 20.
3.    Αυτό οπτική ένδειξη παρέχει τις ίδιες πληροφορίες όπως παραπάνω γραφική αναπαράσταση. (Η τρίτη του κύκλου είναι από τον αριθμό 33%). Το χρώμα παρέχει πρόσθετες πληροφορίες: το πράσινο δηλώνει ότι αυτός ο αριθμός μεγαλώνει από τον προηγούμενο υπολογισμό. Κίτρινο σημαίνει σταθερή και κόκκινο σημαίνει φθίνουσες.
4.    Αυτό σημαίνει ότι οι τιμές που χρησιμοποιούνται για τον υπολογισμό.
5.    Αυτό είναι ένα γράφημα Sparkline του ιστορικού των τιμών διατήρησης. Σας επιτρέπει να δείτε τις τιμές στο παρελθόν να έχετε μια μεγάλη προβολή του πώς εξέλιξη.


## <a name="see-also"></a>Δείτε επίσης

- [Έννοιες][Link 6]
- [Αντιμετώπιση προβλημάτων του οδηγού][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
