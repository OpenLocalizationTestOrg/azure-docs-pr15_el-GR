<properties 
   pageTitle="Περιβάλλον εργασίας χρήστη Azure δέσμευση κινητές συσκευές - τμήματα" 
   description="Μάθετε πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε τμήματα των χρηστών για να προσδιορίσετε μοτίβα χρήσης χρησιμοποιώντας Azure Mobile δέσμευση" 
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

# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>Πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε τα τμήματα του χρήστες για να προσδιορίσετε μοτίβα χρήσης

Σε αυτό το άρθρο περιγράφει την καρτέλα **ΤΜΉΜΑΤΑ** της πύλης **Δέσμευση Mobile** . Μπορείτε να χρησιμοποιήσετε την πύλη **Mobile δέσμευση** να παρακολουθούν και να διαχειριστείτε τις εφαρμογές για κινητές συσκευές.

Στην ενότητα τμήματα του περιβάλλοντος εργασίας Χρήστη του σάς επιτρέπει να εργαστείτε σε διαίρεση τους χρήστες σας με βάση τις διαφορετικές συμπεριφορά και ανάλυσης που μπορείτε να μεταβείτε από την εφαρμογή και επίσης να αποκτήσετε πρόσβαση μέσω του API τμήματα. Τμήματα υπολογίζονται πρώτα 24 ώρες μετά την δημιουργούνται και αυτές είναι recomputed κάθε 24 ώρες με βάση τις πιο πρόσφατες πληροφορίες ανάλυσης. Μόλις υπολογίζεται ένα τμήμα, εμφανίζεται ένα γράφημα "Day ιστορικό ημέρα" κάθε ημέρα.


>[AZURE.NOTE] Πολλές ενότητες της πύλης **Δέσμευση Mobile** περιβάλλοντος εργασίας Χρήστη περιέχουν το κουμπί **ΕΜΦΆΝΙΣΗ ΒΟΉΘΕΙΑΣ** . Πατήστε αυτό το κουμπί για να λάβετε περισσότερες πληροφορίες με βάση τα συμφραζόμενα σχετικά με μια ενότητα.

## <a name="create-segments"></a>Δημιουργία τμημάτων αγοράς
Μπορείτε να δημιουργήσετε ένα τμήμα με βάση έως και 10 κριτήρια σε μια συγκεκριμένη περίοδο προς τα επάνω σε 60 ημέρες στο παρελθόν από την ενότητα ανάλυση. Για παράδειγμα, μπορείτε να δημιουργήσετε ένα τμήμα με βάση τα άτομα που έχετε προβάλει συγκεκριμένες σελίδες ή να γίνει αναζήτηση για συγκεκριμένο περιεχόμενο μέσα στην εφαρμογή σας από τις τελευταίες 10 ημέρες. Αυτές οι πληροφορίες είναι διαθέσιμες στην ενότητα ανάλυση. Επομένως, μπορείτε να το χρησιμοποιήσετε για να δημιουργήσετε ένα τμήμα και, στη συνέχεια, ρυθμίστε μια ειδοποίηση push στόχευσης αυτό υποσύνολο των χρηστών για να λάβετε να επιστρέψει την εφαρμογή. 
 
> Σημείωση: Όταν έχει υπολογιστεί ένα τμήμα, δεν είναι δυνατή η επεξεργασία του. μόνο η κλωνοποιηθεί (αντιγράφονται) ή καταστρέφονται (διαγράφεται). Ένα τμήμα μπορούν να κλωνοποιηθούν μέσα από την ίδια εφαρμογή (με το ίδιο αναγνωριστικό εφαρμογής) και μπορούν επίσης να κλωνοποιηθούν σε άλλες εφαρμογές (με ένα διαφορετικό αναγνωριστικό εφαρμογής). 
 
 ![segments1][35] 

## <a name="examples-segments"></a>Παραδείγματα τμήματα
 ![segments2][36]

Τμήματα σάς επιτρέπουν να χωρίσετε οι τελικοί χρήστες της εφαρμογής σας.
Διαίρεση τους χρήστες σας είναι μια σημαντική στρατηγική μάρκετινγκ. Azure δέσμευση Mobile σάς επιτρέπει να λάβετε δεδομένα ιστορικού και δημιουργήστε προσαρμοσμένα τμήματα. Αυτό το εργαλείο ισχυρή σάς επιτρέπει να μάθετε περισσότερα σχετικά με την εμπειρία των πελατών σας στην εφαρμογή σας. Μπορείτε εύκολα να ανάλυση σας τμήματα και χρησιμοποιήστε τα τμήματα αγοράς σας ως προορισμών push.
Κοινή χρήση-υπόθεσης είναι ότι θέλετε να στείλετε μια push μια ειδοποίηση για να ενθαρρύνετε τους σας οι τελικοί χρήστες για να χαρακτηρίσετε την εφαρμογή σας στο χώρο αποθήκευσης. Αντί να στείλετε μια ειδοποίηση σε όλους σας τους τελικούς χρήστες, μπορείτε να δημιουργήσετε ένα τμήμα που θα καθορίσετε μόνο οι χρήστες που έχετε χρησιμοποιήσει την εφαρμογή σας καθημερινά για τον τελευταίο μήνα και έπρεπε να βρίσκονται σε μια εξαιρετική εμπειρία χρήστη. Κατά την αποστολή ειδοποιήσεων push λιγότερες, ιδιαίτερα προορισμού, λαμβάνετε μια καλύτερη απόδοση Επένδυσης.
 
 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Τμήματα που μπορείτε να δημιουργήσετε με βάση τα κύρια στοιχεία Azure δέσμευση Mobile:
- Συμβάν: δημιουργία ενός τμήματος των προορισμών ένα συγκεκριμένο συμβάν της εφαρμογής που συνέβη περισσότερες από δύο φορές την εβδομάδα. 
- Περίοδος λειτουργίας: δημιουργία ενός τμήματος των χρηστών που έχετε χρησιμοποιήσει την εφαρμογή περισσότερες από 5 φορές προηγούμενη εβδομάδα.
- Δραστηριότητα: δημιουργία ενός τμήματος των χρηστών που έχετε χρησιμοποιήσει μία σελίδα ή περιεχόμενο περισσότερο ή λιγότερο από 10 ώρα περασμένο μήνα.
- Εργασία: δημιουργία ενός τμήματος των χρηστών που έχουν ολοκληρωθεί μια εργασία περισσότερες από δύο φορές την ημέρα.
- Σφάλμα: δημιουργία ενός τμήματος όλων των χρηστών που είχαν σφάλμα περισσότερους από 10 φορές την τελευταία εβδομάδα. (Θα μπορούσε να πατήσετε αυτού του τμήματος με ένα apology ή ακόμα και ενός τοκομεριδίου!)
- Σφάλμα: δημιουργία ενός τμήματος όλων των χρηστών που έχουν ένα σφάλμα περισσότερα από 100 φορές τελευταίες 3 ημέρες.
- Πληροφορίες App: Δημιουργήστε ένα τμήμα που έχει ως προορισμό μια προσαρμοσμένη εφαρμογή πληροφορίες που έχουν γίνει κατά τη διάρκεια των τελευταίων 25 ημερών.
 
 ![segments4][38]

Για να χρησιμοποιήσετε το τμήμα με βέλτιστη τρόπο, πρέπει να έχετε κάνει μια προσαρμοσμένη ενοποίησης του SDK στην εφαρμογή σας με ένα πρόγραμμα ετικετών "Πληροφορίες εφαρμογής" ετικέτες.
Στη συνέχεια, μεταβείτε στην αρχική σελίδα του περιβάλλοντος εργασίας, επιλέξτε την εφαρμογή που θέλετε και κάντε κλικ στην ενότητα "Τμήματα".

1. Επιλέξτε την ενότητα "Τμήματα".
2. Κάντε κλικ στο στοιχείο "νέο τμήμα" το κουμπί για να δημιουργήσετε ένα νέο τμήμα.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Πραγματική διάρκεια ζωής παράδειγμα: Η δημιουργία ενός απλού τμήματος με βάση τις πληροφορίες "Περίοδος λειτουργίας"
Δημιουργία ενός τμήματος του όλες οι τελικοί χρήστες που έχετε χρησιμοποιήσει την εφαρμογή σας τουλάχιστον 50 φορές στην τελευταία εβδομάδα. Από εκεί, βρείτε μόνο οι τελικοί χρήστες που αναλώσατε τουλάχιστον 30 δευτερόλεπτα σε εφαρμογή σας ανά περίοδο λειτουργίας. Αυτό θα εμφανίζονται όλες οι τελικοί χρήστες που διαθέτουν ένα θετικό εμπειρία στην εφαρμογή. Στη συνέχεια, στο τμήμα που δημιουργήθηκε μπορεί να χρησιμοποιηθεί για να προωθήσετε μια ειδοποίηση σε αυτές τις τελικούς χρήστες να ζητήστε τους να χαρακτηρίσετε την εφαρμογή σας στο χώρο αποθήκευσης.
 
 ![segments5][39]

1. Δώστε το τμήμα σας ένα όνομα για να βρείτε γρήγορα στη λίστα "Τμήμα".
2. Κάντε κλικ στο κουμπί "Δημιουργία".
 
 ![segments6][40]

Επιλέξτε την περίοδο λειτουργίας.
 
 ![segments7][41]

1. Επιλέξτε την περίοδο των "Προηγούμενη εβδομάδα".
2. Κάντε κλικ στο κουμπί Επόμενο.
 
 ![segments8][42]

1. Επιλέξτε τον τελεστή που αφορά μεταξύ της λίστας: =; ≥, ≤.
2. Πληκτρολογήστε το πλήθος που θέλετε.
3. Επιλέξτε την εμφάνιση που θέλετε. 
4. Κάντε κλικ στο κουμπί Επόμενο.
Αυτό το παράδειγμα έχει ρυθμιστεί ώστε να συμφωνούν που επάνω από την προηγούμενη εβδομάδα, με τους χρήστες που έχουν γίνει τουλάχιστον 50 περιόδους λειτουργίας.
 
 ![segments9][43]

Για την περίοδο λειτουργίας αγοράς, μπορείτε να επιλέξετε τη διάρκεια ανά περίοδο λειτουργίας ως κριτήρια.

1. Επιλέξτε τον τελεστή από τη λίστα.
2. Δώστε τη διάρκεια ανά περίοδο λειτουργίας.
3. Κάντε κλικ στο κουμπί Επόμενο.
Σε αυτό το παράδειγμα, εμφανίζεται η ένδειξη που πάνω από όλες τις περιόδους λειτουργίας που έχουν φέρουν κατά διαστήματα στην ενότητα Εμφάνιση, επιλέξτε μόνο τους χρήστες που αναλώσατε περισσότερα από 30 δευτερόλεπτα ανά περίοδο λειτουργίας.
 
 ![segments10][44]

Ονομάστε το κριτήριο για να την ανακτήσετε στο την πλήρη ομαδοποίηση και κάντε κλικ στο κουμπί Τέλος.
 
 ![segments11][45]

Όταν ολοκληρώσετε τη ρύθμιση του το κριτήριο, θα εμφανίζεται στο την ομαδοποίηση του τμήματος.
Επειδή ένα τμήμα βασίζεται σε ανάλυση δεδομένων, τμήματα υπολογίζονται μία φορά την ημέρα.
Σε αυτό το παράδειγμα, 47,7% το συνολικό τελικών χρηστών αντιστοιχιστεί το κριτήριο. Θα πρέπει να τους χρήστες που έχουν μια καλή εμπειρία και θα είναι πιθανό να παρέχουν υψηλότερο χαρακτηρισμό εάν πατήσετε τους μια ειδοποίηση που σας ρωτά τους για να χαρακτηρίσετε την εφαρμογή στο χώρο αποθήκευσης.


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
 
