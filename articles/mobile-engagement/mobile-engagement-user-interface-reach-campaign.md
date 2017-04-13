<properties 
   pageTitle="Περιβάλλον εργασίας χρήστη Azure δέσμευση κινητές συσκευές - Reach εκστρατείας" 
   description="Laern Τρόπος δημιουργίας και διαχείρισης ειδοποιήσεων push εκστρατείες χρησιμοποιώντας Azure Mobile δέσμευση" 
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


# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε εκστρατείες ειδοποιήσεων push
Μπορείτε να χρησιμοποιήσετε την ενότητα Reach το περιβάλλον εργασίας Χρήστη για να δημιουργήσετε μια νέα εκστρατεία Push με έναν σύνθετο τύπο, παρέχοντας όλες τις πληροφορίες που χρειάζεστε για να στείλετε μια ειδοποίηση push. Οι επιλογές μιας εκστρατείας Push ελαφρώς ποικίλλουν ανάλογα με τους τύπους τέσσερις εκστρατείας: ανακοινώσεις, ψηφοφορίες, προωθεί δεδομένων και τα πλακίδια (μόνο για Windows Phone).

### <a name="option-applies-to"></a>Η επιλογή ισχύει για:
- Γλώσσες: Όλα (ανακοινώσεις, ψηφοφορίες, δεδομένα ωθεί, τα πλακίδια)
- Εκστρατεία: Όλα (ανακοινώσεις, ψηφοφορίες, δεδομένα ωθεί, τα πλακίδια)
- Ειδοποίηση: Ανακοινώσεις, ψηφοφοριών
- Content: Μοναδικός για κάθε τύπο εκστρατείας
- Ακροατήριο: Όλα (ανακοινώσεις, ψηφοφορίες, δεδομένα ωθεί, τα πλακίδια)
- Χρονικό πλαίσιο: ανακοινώσεις, ψηφοφορίες, τα πλακίδια
- Έλεγχος: Όλα (ανακοινώσεις, ψηφοφορίες, δεδομένα ωθεί, τα πλακίδια)
 
![Reach Campaign1][20]

## <a name="languages"></a>Γλώσσες
Μπορείτε να χρησιμοποιήσετε το αναπτυσσόμενο μενού γλωσσών για να στείλετε μια διαφορετική έκδοση του Push σας σε συσκευές που έχουν ρυθμιστεί να χρησιμοποιούν διαφορετικές γλώσσες. Από προεπιλογή, όλες τις συσκευές θα λάβουν το ίδιο Push ανεξάρτητα από το ποια γλώσσα έχουν ρυθμιστεί για να χρησιμοποιήσετε. Οι χρήστες με τη συσκευή που έχει οριστεί σε άλλη γλώσσα θα λάβουν την προεπιλεγμένη γλώσσα έκδοση του το πάτημα. Πολλές από τις επιλογές εκστρατείας push σάς επιτρέπουν να καθορίσετε εναλλασσόμενες περιεχομένου για κάθε μία από τις πρόσθετες γλώσσες που επιλέγετε. 
 
![Reach Campaign2][21]

### <a name="language-differences-apply-to"></a>Διαφορές γλώσσας ισχύουν για:
- Γλώσσες: Μοναδικό γλώσσες ενδέχεται να έχει επιλεγεί εκτός από την προεπιλεγμένη γλώσσα
- Εκστρατεία: Ίδια για όλες τις γλώσσες
- Ειδοποίηση: Μοναδικό για κάθε γλώσσα εκτός από την προεπιλεγμένη γλώσσα
- Content: Μοναδικό για κάθε γλώσσα εκτός από την προεπιλεγμένη γλώσσα
- Ακροατήριο: Μπορεί να είναι δυνατό το φιλτράρισμα με βάση ένα κριτήριο ξεχωριστή γλώσσας
- Χρονικό πλαίσιο: ίδιο για όλες τις γλώσσες
- Έλεγχος: Μπορεί να αποσταλούν για κάθε γλώσσα κάθε φορά
 
### <a name="supported-languages"></a>Υποστηριζόμενες γλώσσες:
- Αραβικά (ar) 
- Βουλγαρικά (bg) 
- Καταλανικά (ca) 
- Κινεζικά (zh) 
- Κροατικά (hr) 
- Czech (CS) 
- Δανικά (da) 
- Ολλανδικά (nl) 
- Αγγλικά (en) 
- Φινλανδικά (fi) 
- Γαλλικά (fr) 
- Γερμανικά (de) 
- Ελληνικά (Ελ) 
- Εβραϊκά (αυτός) 
- Χίντι (υψηλής) 
- Ουγγρικά (hu) 
- Ινδονησιακά (id) 
- Ιταλικά (ΙΤ) 
- Ιαπωνικά (ja) 
- Κορεατικά (ko) 
- Λετονικά (lv) 
- Λιθουανικά (δκ) 
- Μαλαϊκά (macrolanguage) (ms) 
- Νορβηγικά Bokmål (nb) 
- Πολωνικά (pl) 
- Πορτογαλικά (pt) 
- Ρουμανικά (ro) 
- Ρωσικά (ru) 
- Σερβικά (sr) 
- Σλοβακικά (sk) 
- Σλοβενικά (sl) 
- Ισπανικά (ες) 
- Σουηδικά (ΔΧ) 
- Ταγκαλόγκ (tl) 
- Ταϊλανδικά (θ) 
- Τουρκικά (tr) 
- Ουκρανικά (uk) 
- Βιετναμικά (vi) 
 
## <a name="campaign"></a>Εκστρατείας
Μπορείτε να χρησιμοποιήσετε την ενότητα εκστρατείας για να ορίσετε το όνομα και η κατηγορία της εκστρατείας σας καθώς και όπως εάν σκοπεύετε να παραβλέψετε την ενότητα ακροατήριο μιας εκστρατείας Push και να στείλετε αυτής της εκστρατείας μέσω του API επίτευξη (και ορισμένα στοιχεία με το χαμηλό επίπεδο Push API) αντί για αυτό. Κατηγορίες μπορεί να χρησιμοποιηθεί με ένα πρότυπο προσαρμοσμένου ειδοποίησης για τις ειδοποιήσεις της εφαρμογής ελέγχου με βάση τις προκαθορισμένες ρυθμίσεις. Μπορείτε να λάβετε μια λίστα με τις υπάρχουσες "Κατηγορίες" μέσω του API επίτευξη.

> Προειδοποίηση: Εάν χρησιμοποιείτε την επιλογή "Παράβλεψη ακροατήριο, push θα σταλεί στους χρήστες μέσω του API" στην ενότητα "Εκστρατείας" μιας εκστρατείας Reach, της εκστρατείας δεν θα στείλει αυτόματα, θα χρειαστεί να στείλετε με μη αυτόματο τρόπο μέσω του API επίτευξη.
 
![Reach Campaign3][22]
 
### <a name="option-applies-to"></a>Η επιλογή ισχύει για:
- Όνομα: όλων
- Κατηγορία: Ανακοινώσεις, ψηφοφοριών
- Παράβλεψη ακροατήριο, push θα αποστέλλονται στους χρήστες μέσω του API: όλων
 
## <a name="notification"></a>Ειδοποίηση
Μπορείτε να χρησιμοποιήσετε την ενότητα ειδοποίησης για να ορίσετε βασικές ρυθμίσεις για το συμπεριλαμβανομένων push: στον τίτλο της το πάτημα, το μήνυμα, μια εικόνα της εφαρμογής, ή εάν είναι αποκρυφτούν. Πολλές ρυθμίσεις ειδοποιήσεων είναι συγκεκριμένες για την πλατφόρμα της συσκευής σας. Μπορείτε να επιλέξετε εάν θα σταλεί σας push "στην εφαρμογή" ή "από την εφαρμογή" ή και τα δύο. (Μην ξεχνάτε ότι οι χρήστες μπορούν να "επιλέξετε στο" ή "εξαίρεση" του "Έξοδος από το app" προωθεί στο το λειτουργικό σύστημα επιπέδου στις συσκευές τους και δέσμευση Mobile Azure δεν θα μπορείτε να παρακάμψετε αυτήν τη ρύθμιση. Επίσης, να θυμάστε ότι το API επίτευξη χειρίζεται "στην εφαρμογή" και "εκτός της εφαρμογής" προωθεί. Το API Push μπορεί να χρησιμοποιηθεί για το χειρισμό "από την εφαρμογή" ωθεί πολύ.) Ωθεί μπορεί να προσαρμοστεί με εικόνες ή περιεχόμενο HTML, συμπεριλαμβανομένων πολλά επίπεδα συνδέσεων για σύνδεση εκτός της εφαρμογής σας ή σε άλλη θέση στην εφαρμογή σας (Android SDK 2.1.0 ή νεότερη έκδοση πρόθεση κατηγορίες απαιτούνται). Μπορείτε να αλλάξετε το εικονίδιο ή iOS σήματος, και να στέλνουν περιεχόμενο κειμένου ή web (ένα αναδυόμενο παράθυρο με html του περιεχομένου, διεύθυνση URL σύνδεσης σε άλλη θέση, είτε βρίσκονται εντός ή εκτός της εφαρμογής). Μπορείτε επίσης να κάνετε κλήση συσκευές Android ή να δόνηση με το πάτημα. (Να θυμάστε ότι θα πρέπει το σωστό αρχείο κλήση ή μια συσκευή δόνηση δήλωσης SDK δικαιώματα στο Android.) Υπάρχει αυτήν τη στιγμή δεν υπάρχει κλάδο τυπική μεγέθη Android "γενική εικόνα", επειδή το μεγέθη οθόνης είναι διαφορετικό σε κάθε συσκευή, αλλά λειτουργεί 400 x 100 εικόνες σε σχεδόν οποιοδήποτε μέγεθος οθόνης.

### <a name="delivery-types"></a>Τύποι παράδοσης:
-    Από την εφαρμογή μόνο: θα παραδίδονται την ειδοποίηση όταν ο χρήστης δεν χρησιμοποιεί την εφαρμογή.
- Το εκτός της ειδοποίησης μόνο εφαρμογή απαιτεί ένα πιστοποιητικό από το Apple ή Google (πιστοποιητικό APNS ή GCM).
- Της εφαρμογής μόνο: Η ειδοποίηση εμφανίζεται μόνο όταν εκτελείται η εφαρμογή.
- Ειδοποίηση χρησιμοποιεί το σύστημα παράδοσης Capptain για να συνδεθεί με το χρήστη. Μπορείτε να προσαρμόσετε πλήρως την οπτική διάταξη/εμφάνιση push σας.
- Οποιαδήποτε στιγμή: Αυτή η επιλογή εξασφαλίζει ότι στείλετε μια ειδοποίηση είτε η εφαρμογή εκτελείται ή όχι.

 
![Reach Campaign4][23]

### <a name="option-applies-to"></a>Η επιλογή ισχύει για:
- Ειδοποίηση: Ανακοινώσεις, ψηφοφοριών
 
## <a name="content"></a>Περιεχόμενο
Μπορείτε να χρησιμοποιήσετε την ενότητα περιεχομένου για να τροποποιήσετε το περιεχόμενο του σας ανακοινώσεις, ψηφοφορίες, προωθεί δεδομένων και τα πλακίδια (μόνο για Windows Phone). Η ρύθμιση περιεχομένου καμπάνιες Push είναι συγκεκριμένη για τον τύπο της εκστρατείας. 

### <a name="see-also"></a>Δείτε επίσης
- [Τεκμηρίωση του περιβάλλοντος εργασίας Χρήστη - επίτευξη - Push περιεχομένου][Link 29]
 
![Reach Campaign5][24]

## <a name="audience"></a>Ακροατηρίου
Μπορείτε να χρησιμοποιήσετε την ενότητα ακροατηρίου για να ορίσετε μια βασική λίστα στοιχείων για να περιορίσετε το εκστρατείας ή όρια εκστρατείας σας που βασίζονται σε προσαρμοσμένα κριτήρια. Το βασικό σύνολο των επιλογών για να περιορίσετε το ακροατήριό σας σάς επιτρέπει να προωθήσετε σε νέα ή να είναι παλιό ή εγγενή push χρήστες μόνο. Μπορείτε επίσης να ορίσετε ένα όριο για να περιορίσετε τον αριθμό των χρηστών που λαμβάνουν το πάτημα. Μπορείτε να επεξεργαστείτε με μη αυτόματο τρόπο την παράσταση για το πώς έχει φιλτραριστεί εκστρατείας σας για να συμπεριλάβετε ένα ή περισσότερα κριτηρίου σε χρήστες του προορισμού. Με μη αυτόματο τρόπο, μπορείτε να πληκτρολογήσετε μια παράσταση ακροατηρίου. Όπως μια παράσταση ρητά πρέπει να καθορίσετε τη σχέση μεταξύ των κριτηρίων. Ένα κριτήριο περιγράφεται από ένα αναγνωριστικό που πρέπει να ξεκινούν με γράμμα κεφαλαίο και δεν μπορούν να περιέχουν κενά διαστήματα. Η σχέση μεταξύ τα κριτήρια μπορεί να είναι περιγράφεται χρησιμοποιώντας 'και', 'ή', 'δεν' τελεστές, καθώς και '(',')'. Παράδειγμα: "Criterion1 ή (Criterion1 και δεν Criterion2)".

> Σημείωση: Με ευρύ κοινό περιλαμβάνονται στις εκστρατείες, την πλευρά του διακομιστή στόχευσης σάρωση μπορεί να είναι αργή, ειδικά εάν προσπαθείτε να ξεκινήσετε πολλά εκστρατείες την ίδια στιγμή.

- Εάν είναι δυνατόν, μόνο έναρξη εκστρατείας μία κάθε φορά.
- Τις πιο, μόνο Έναρξη τέσσερις εκστρατείες κάθε φορά.
- Push μόνο για τους χρήστες σας ενεργό (το πλαίσιο ελέγχου "Εμπλακείτε μόνο οι χρήστες που μπορούν να σας καλούν με χρήση εγγενούς Push" και "Εμπλακείτε μόνο ενεργοί χρήστες"), έτσι ώστε μόνο οι χρήστες σας που εξακολουθεί να εγκαταστήσει την εφαρμογή και χρησιμοποιήστε το θα πρέπει να είναι σάρωση.
Αφού καθοριστεί το ακροατήριό σας, μπορείτε να χρησιμοποιήσετε το κουμπί προσομοίωση για να μάθετε τον αριθμό των χρηστών θα λάβετε αυτό Push. Αυτό θα υπολογίσει τον αριθμό των γνωστών χρήστες ενδεχομένως στοχευμένες μέσω αυτού του ακροατηρίου (αυτή είναι μια εκτίμηση με βάση ένα δείγμα τυχαία από τους χρήστες). Έχετε υπόψη ότι οι χρήστες που έχουν καταργηθεί η εγκατάσταση της εφαρμογής είναι επίσης τμήμα αυτού του ακροατηρίου, αλλά δεν μπορείτε να αποκτήσετε πρόσβαση.

### <a name="see-also"></a>Δείτε επίσης
- [Περιβάλλον εργασίας Χρήστη τεκμηρίωση - Reach - νέα Push κριτηρίου][Link 28]

![Reach Campaign6][25]

### <a name="edit-expression"></a>Επεξεργασία παράστασης
![Reach Campaign7][26]
 
### <a name="limit-your-audience-option-applies-to"></a>Η επιλογή ακροατηρίου ισχύει για το όριο:
- Εμπλακείτε μόνο ένα υποσύνολο των χρηστών: όλα (ανακοινώσεις, ψηφοφορίες, προωθεί δεδομένων, τα πλακίδια)
- Εμπλακείτε μόνο παλιά χρηστών: όλα (ανακοινώσεις, ψηφοφορίες, προωθεί δεδομένων, τα πλακίδια)
- Εμπλακείτε μόνο τους νέους χρήστες: όλα (ανακοινώσεις, ψηφοφορίες, προωθεί δεδομένων, τα πλακίδια)
- Εμπλακείτε μόνο οι αδρανείς χρήστες: ανακοινώσεις, ψηφοφορίες, τα πλακίδια
- Εμπλακείτε μόνο ενεργοί χρήστες: όλα (ανακοινώσεις, ψηφοφορίες, προωθεί δεδομένων, τα πλακίδια)
- Εμπλακείτε μόνο οι χρήστες που μπορούν να σας καλούν με χρήση εγγενούς Push: ανακοινώσεις, ψηφοφοριών
 
## <a name="time-frame"></a>Χρονικού πλαισίου
Μπορείτε να χρησιμοποιήσετε την ενότητα χρονικό πλαίσιο για να ορίσετε κατά το πάτημα θα σταλεί ή μπορείτε να αφήσετε το χρονικό πλαίσιο κενό για να ξεκινήσετε αμέσως της εκστρατείας. Να θυμάστε ότι χρησιμοποιώντας ζώνη ώρας οι τελικοί χρήστες μπορεί να έναρξη εκστρατείας νωρίτερα από αυτήν που περιμένετε σας οι τελικοί χρήστες στην Ασία και αποστολή μικρές δέσμες ωθεί ταυτόχρονα έως ότου όλες τις ζώνες ώρας στον κόσμο συμφωνεί με το χρονικό πλαίσιο για την εκστρατεία την ημέρα. Χρήση ζώνη ώρας οι τελικοί χρήστες μπορεί να προκαλέσουν καθυστερήσεις στις εκστρατείες εφόσον έχει για να ζητήσετε την ώρα από το τηλέφωνο πριν να ξεκινήσετε το πάτημα.

> Σημείωση: Εκστρατείες χωρίς να αποθηκεύσετε προσωρινά, μια ημερομηνία λήξης ωθεί τοπικά και εξακολουθείτε να τα εμφανίσετε αφού εκστρατείες με μη αυτόματο τρόπο ολοκλήρωσης. Για να αποφύγετε αυτήν τη συμπεριφορά, ειδικά μια ώρα λήξης για εκστρατείες.

### <a name="see-also"></a>Δείτε επίσης
- [Επίτευξη - πώς διαδικασίες – προγραμματισμού][Link 3] 
 
![Reach Campaign8][27]

### <a name="settings-apply-to"></a>Ρυθμίσεις εφαρμόζονται σε:
- Χρονικό πλαίσιο: ανακοινώσεις, ψηφοφορίες, τα πλακίδια
 
## <a name="test"></a>Έλεγχος
Μπορείτε να χρησιμοποιήσετε την ενότητα δοκιμή για να στείλετε αυτό push για τη δική σας συσκευής δοκιμής πριν από την αποθήκευση της εκστρατείας. Εάν έχετε ρυθμίσει τις παραμέτρους οποιαδήποτε προσαρμοσμένα γλώσσα για αυτήν την εκστρατεία, μπορείτε να ελέγξετε το πάτημα σε κάθε γλώσσα. Μπορείτε να ρυθμίσετε μια συσκευή δοκιμής από "Ο λογαριασμός μου".
> Σημείωση: Δεν υπάρχει διακομιστή δεδομένων έχει συνδεθεί όταν χρησιμοποιείτε το κουμπί για να "δοκιμή" προωθεί, μόνο τα δεδομένα έχουν καταγραφεί για εκστρατείες πραγματικό push.

### <a name="see-also"></a>Δείτε επίσης
- [Περιβάλλον εργασίας Χρήστη τεκμηρίωση - ο λογαριασμός μου][Link 14]
 
![Reach Campaign9][28]

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
 
