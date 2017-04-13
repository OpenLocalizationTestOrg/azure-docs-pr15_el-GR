<properties 
   pageTitle="Περιβάλλον εργασίας χρήστη Azure δέσμευση κινητές συσκευές - Reach" 
   description="Μάθετε πώς μπορείτε να την επικοινωνία με τους χρήστες της εφαρμογής σας με χρήση Azure Mobile δέσμευσης ειδοποιήσεις push" 
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


# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Πώς να επικοινωνήστε με τους χρήστες της εφαρμογής σας με τις ειδοποιήσεις push

Σε αυτό το άρθρο περιγράφει την καρτέλα **ΕΠΊΤΕΥΞΗ** της πύλης **Δέσμευση Mobile** . Μπορείτε να χρησιμοποιήσετε την πύλη **Mobile δέσμευση** να παρακολουθούν και να διαχειριστείτε τις εφαρμογές για κινητές συσκευές. Σημειώστε ότι για να ξεκινήσετε με την πύλη πρέπει πρώτα να δημιουργήσετε ένα λογαριασμό **Δέσμευση Mobile Azure** . Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού δέσμευση Mobile Azure](mobile-engagement-create.md).

Στην ενότητα Reach του περιβάλλοντος εργασίας Χρήστη του είναι το εργαλείο διαχείρισης εκστρατείας Push όπου μπορείτε να δημιουργήσετε ή επεξεργασία/Ενεργοποίηση/λήξη/στην οθόνη και λάβετε στατιστικά στοιχεία σχετικά με εκστρατείες ειδοποιήσεων Push και τις δυνατότητες που έχετε πρόσβαση μέσω του API επίτευξη (και ορισμένα στοιχεία του χαμηλού επιπέδου Push API). Να θυμάστε ότι αν χρησιμοποιείτε το API ή το περιβάλλον εργασίας Χρήστη, θα πρέπει να ενοποιήσετε το Azure Mobile δέσμευση και Reach στην εφαρμογή σας για κάθε πλατφόρμα με το SDK να μπορέσετε να χρησιμοποιήσετε επίτευξη εκστρατείες.

>[AZURE.NOTE] Πολλές ενότητες της πύλης **Δέσμευση Mobile** περιβάλλοντος εργασίας Χρήστη περιέχουν το κουμπί **ΕΜΦΆΝΙΣΗ ΒΟΉΘΕΙΑΣ** . Πατήστε αυτό το κουμπί για να λάβετε περισσότερες πληροφορίες με βάση τα συμφραζόμενα σχετικά με μια ενότητα.

## <a name="four-types-of-push-notifications"></a>Τέσσερις τύπους ειδοποιήσεων Push
1.    Ανακοινώσεις - σας επιτρέπει να στείλετε μηνύματα διαφημίσεις σε χρήστες που τους ανακατευθύνετε σε άλλη θέση εντός της εφαρμογής σας ή να τους στείλετε σε μια ιστοσελίδα ή store εκτός της εφαρμογής σας. 
2.    Ψηφοφορίες - σάς επιτρέπουν να συλλογή πληροφοριών από τελικούς χρήστες, κάντε τις ερωτήσεις.
3.    Ωθεί δεδομένων - σάς επιτρέπουν να στείλετε ένα αρχείο δεδομένων δυαδικών ή base64. Οι πληροφορίες που περιέχονται σε μια push δεδομένων αποστέλλεται στην εφαρμογή σας για να τροποποιήσετε την τρέχουσα εμπειρία των χρηστών σας στην εφαρμογή. Η εφαρμογή σας πρέπει να έχετε τη δυνατότητα να επεξεργάζονται τα δεδομένα σε ένα push δεδομένων.

## <a name="campaign-details"></a>Λεπτομέρειες εκστρατείας

Μπορείτε να επεξεργαστείτε, κλωνοποίηση, διαγραφή, ή ενεργοποίηση εκστρατείες που δεν έχει ενεργοποιηθεί ακόμα τοποθετώντας επάνω από τα ονόματά τους ή μπορείτε να κάνετε κλικ για να τα ανοίξετε. Μπορείτε να κλωνοποίηση εκστρατείες που έχει ήδη ενεργοποιηθεί τοποθετώντας επάνω από τα ονόματά τους ή μπορείτε να κάνετε κλικ για να τα ανοίξετε. Ωστόσο, δεν μπορείτε να αλλάξετε μια εκστρατεία όταν έχει ενεργοποιηθεί.
 
![Reach1][18]

## <a name="reach-feedback"></a>Επίτευξη σχολίων

Κάντε κλικ στο για να δείτε τις λεπτομέρειες μιας εκστρατείας Reach **Στατιστικά στοιχεία** . Η **απλή** προβολή παρέχει μια οπτική αναπαράσταση στη φόρμα από ένα γράφημα ράβδων στήλης σχετικά με το τι συνέβη αφού ενεργοποιήθηκε μιας εκστρατείας. Η προβολή **για προχωρημένους** παρέχει πιο λεπτομερές λεπτομέρειες σχετικά με την εκστρατεία push. Δεν θα είναι διαθέσιμη εάν στέλνετε μιας εκστρατείας δοκιμής δηλαδή ένα push που αποστέλλονται σε μια συσκευή δοκιμής αυτές τις λεπτομέρειες. Παρακάτω περιγράφεται ο τρόπος που θα πρέπει να ερμηνεύσει αυτές τις λεπτομέρειες:

1. **Pushed** - καθορίζει τον αριθμό των μηνυμάτων προωθούνται στις συσκευές. Αυτός ο αριθμός θα εξαρτώνται από το ακροατήριο που καθορίσατε κατά τη δημιουργία της εκστρατείας push. Εάν δεν καθορίσετε οποιαδήποτε ακροατήριο προορισμού, στη συνέχεια, αυτό push θα σταλεί σε όλες τις συσκευές καταχωρημένες. Όπως και όλες οι άλλες υπηρεσίες push, θα σας δεν push τις ειδοποιήσεις απευθείας σε συσκευές αλλά αντί για αυτό push αυτά τα αντίστοιχα πλατφόρμα συγκεκριμένες υπηρεσίες ειδοποιήσεων Push (Γραμματίων - GCM/APNS/WNS), έτσι ώστε να μπορούν να παρέχουν οι ειδοποιήσεις για τις συσκευές. 

2.  **Delivered** - καθορίζει τον αριθμό των μηνυμάτων που παραδίδονται από το Γραμματίων στη συσκευή και αναγνωρίζεται με επιτυχία ως received από SDK δέσμευση Mobile. 
        
    *Λόγοι για Delivered μέτρηση είναι μικρότερη ωθούμενες count:*
    
    1. Εάν ο χρήστης έχει καταργηθεί η εγκατάσταση της εφαρμογής από τη συσκευή, αλλά το Γραμματίων δεν γνωρίζει αυτήν τη στιγμή στείλουμε το πάτημα για να το Γραμματίων, στη συνέχεια, το μήνυμα θα καταργηθεί.
    2. Εάν η συσκευή διαθέτει την εφαρμογή, αλλά οι συσκευές τον εαυτό τους ήταν χωρίς σύνδεση για μεγάλα χρονικά διαστήματα, θα αποτύχει η Γραμματίων για την παράδοση του μηνύματος στη συσκευή. 
    3. Εάν λάβετε παράδοση του μηνύματος στη συσκευή, αλλά το SDK δέσμευση Mobile στην εφαρμογή δεν αναγνωρίζει το περιεχόμενο του μηνύματος, στη συνέχεια, κλείνει αυτό το μήνυμα. Αυτό θα μπορούσε να συμβεί εάν η προσαρμογή της ειδοποίησης στην εφαρμογή δημιουργεί μια εξαίρεση που θα σας να ενημερωθείτε στο SDK και αποθέστε το μήνυμα. Επίσης, αυτό μπορεί να συμβεί εάν η εφαρμογή στη συσκευή είναι χρησιμοποιώντας μια έκδοση του SDK δέσμευση Mobile που δεν είναι δυνατό να κατανοήσετε τη νεότερη έκδοση του μηνύματος push αποστέλλεται από την πλατφόρμα, αλλά αυτό είναι μόνο όταν η εφαρμογή αναβαθμίστηκε μετά την ειδοποίηση είχε αποσταλεί από την πλατφόρμα υπηρεσίας. Στην καρτέλα **για προχωρημένους** θα σας ενημερώσει απορρίφθηκαν τον αριθμό των μηνυμάτων. 
    4. Σε συσκευές iOS, μηνύματα μερικές φορές δεν παραδοθούν εάν είτε η συσκευή είναι ενεργοποιημένη χαμηλής ενέργειας ή εάν η εφαρμογή καταναλώνει σημαντική ενέργεια κατά την επεξεργασία απομακρυσμένο ειδοποιήσεις. Αυτός είναι ένας περιορισμός από τις συσκευές iOS.   

3.  **Εμφάνιση** - καθορίζει τον αριθμό των μηνυμάτων που εμφανίζονται με επιτυχία στο χρήστη εφαρμογή στη συσκευή με τη μορφή μια ειδοποίηση push/ανάληψης-του-app συστήματος στο κέντρο ειδοποίηση ή ειδοποίηση της εφαρμογής κατά την εφαρμογή για κινητές συσκευές.  Στην καρτέλα **για προχωρημένους** θα σας καθοδηγήσουν πόσες έχουν ειδοποιήσεις συστήματος και πόσες από αυτές τις ειδοποιήσεις της εφαρμογής. 
    
    *Λόγοι για εμφανίζεται μέτρηση είναι μικρότερη Delivered count (αναμονή για να εμφανιστούν)*
    
    1. Εάν η ειδοποίηση εκστρατείας είχε μια ημερομηνία λήξης σε αυτό, στη συνέχεια, είναι πιθανό ότι η ειδοποίηση παραδόθηκε αλλά όταν ήταν ο χρόνος για να ανοίξετε και να εμφανίσετε στο χρήστη εφαρμογή, αυτό έχει ήδη λήξει, ώστε να εμφανιζόταν ποτέ.   
    2. Εάν η ειδοποίηση είναι μια ειδοποίηση της εφαρμογής, στη συνέχεια, η ειδοποίηση εμφανίζεται μόνο όταν ο χρήστης εφαρμογή ανοίγει την εφαρμογή. Στις περιπτώσεις όπου ο χρήστης εφαρμογή δεν έχει ανοίξει την εφαρμογή, το SDK θα αναφέρει ότι την ειδοποίηση παραδόθηκε αλλά δεν έχουν ακόμα εμφανίζεται μέχρι να ανοίξετε την εφαρμογή. 
    2. Εάν η ειδοποίηση είναι μια ειδοποίηση της εφαρμογής και έχει ρυθμιστεί για να εμφανιστεί σε μια συγκεκριμένη δραστηριότητα/οθόνη, στη συνέχεια, επίσης θα αναφερθούν την ειδοποίηση ως παραδόθηκε αλλά δεν έχουν ακόμα παραδόθηκε μέχρι ο χρήστης ανοίγει η εφαρμογή σε μια συγκεκριμένη οθόνη. 
    
4.  **Αλληλεπιδράσεις χρήστη** - αυτή καθορίζει τον αριθμό των μηνυμάτων που ο χρήστης εφαρμογή έχει κάποια ενέργεια με και θα περιλαμβάνουν τα μηνύματα που actioned ή εξέλθουν. 

    - *Ο χρήστης εφαρμογής να ενέργεια μια ειδοποίηση με έναν από τους εξής τρόπους:*
            
        1. Εάν η ειδοποίηση είναι μια ειδοποίηση συστήματος/ανάληψης-του-app ή ειδοποίηση της εφαρμογής αποστέλλεται ειδοποίηση μόνο, στη συνέχεια, ο χρήστης εφαρμογή κάνει κλικ στην ειδοποίηση.
        2. Εάν η ειδοποίηση είναι μια ειδοποίηση στην εφαρμογή με ένα κείμενο ή προβολή web ή ψηφοφορίες, στη συνέχεια, ο χρήστης εφαρμογή κάνει κλικ του κουμπιού ενέργειας στην ειδοποίηση.
        3. Εάν η ειδοποίηση είναι μια ειδοποίηση στην εφαρμογή με μια προβολή web, στη συνέχεια, η εφαρμογή χρήστης κάνει κλικ σε μια διεύθυνση URL του web μόνο στην προβολή [Android]
    
    - *Ο χρήστης εφαρμογής να κλείσετε μια ειδοποίηση με έναν από τους εξής τρόπους:*
    
        1. Κάνοντας κλικ στο κουμπί Κλείσιμο την ειδοποίηση απευθείας. 
        2. Σάρωση δεν βρίσκομαι στον υπολογιστή ή τη διαγραφή την ειδοποίηση. 
        3. Ειδοποιήσεις της εφαρμογής με περιεχόμενο κειμένου/web και ψηφοφορίες συνήθως εμφανίζονται στο χρήστη εφαρμογής σε μια διαδικασία δύο βημάτων. Δείτε μια ειδοποίηση πρώτα και όταν κάνουν κλικ σε αυτό, να δει το οι επόμενες περιεχόμενο web/κείμενο/ψηφοφορίας. Ο χρήστης εφαρμογής να πραγματοποιήσετε έξοδο από μια ειδοποίηση σε ένα από αυτά τα βήματα και τις λεπτομέρειες της προβολής για προχωρημένους καταγράφει αυτό. 

5.  **Actioned** - καθορίζει τον αριθμό των μηνυμάτων που έχουν ρητά actioned από το χρήστη της εφαρμογής. Αυτός είναι ο πιο ενδιαφέρουσα αριθμός όπως αυτό σας ενημερώνει για το πόσοι χρήστες της εφαρμογής έχουν σας ενδιαφέρουν από το μήνυμα που προωθηθεί στην ειδοποίηση. 
 
> [AZURE.NOTE] Στο iOS & Windows Άνοιγμα πλατφόρμες, εάν ο χρήστης έχει της εφαρμογής και της εκστρατείας έχει μια εκστρατεία "Οποιαδήποτε στιγμή", στη συνέχεια, είναι πιθανό ότι και τα δύο από το app και τις ειδοποιήσεις της εφαρμογής θα εμφανίζονται ταυτόχρονα. Αυτό μπορεί να προκαλέσει υψηλότερα από τα Delivered εμφανίζεται μέτρηση. Εάν ο χρήστης αλληλεπιδρά ή ενέργειες την ειδοποίηση, στη συνέχεια, ακόμα και το πλήθος των αλληλεπιδράσεών χρήστη/Actioned μπορεί να είναι μεγαλύτερη από Delivered. 


![Reach2][19]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
