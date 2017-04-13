<properties 
   pageTitle="Azure κινητή δέσμευση οδηγού - SDK αντιμετώπισης προβλημάτων" 
   description="Αντιμετώπιση προβλημάτων ενοποίησης SDK στο Azure Mobile δέσμευση" 
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

# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Οδηγός αντιμετώπισης προβλημάτων για ζητήματα ενοποίησης SDK

Οι παρακάτω είναι πιθανά ζητήματα που μπορεί να αντιμετωπίσετε με τον τρόπο Ενοποιείται Azure Mobile δέσμευση στην εφαρμογή σας.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>Θέματα SDK εντοπιστεί από αποτυχία σε μια άλλη περιοχή της εφαρμογής σας

### <a name="issue"></a>Το ζήτημα
- Περιβάλλον εργασίας Χρήστη δεδομένων συλλογής αποτυχία (στην ανάλυση, παρακολούθηση, αγοράς ή πίνακες εργαλείων).
- Push αποτυχίες (ωθεί δεν λειτουργούν στην εφαρμογή, Έξοδος από το app ή και τα δύο).
- Για προχωρημένους αποτυχίες δυνατότητα (παρακολούθηση, Γεωεντοπισμός ή πλατφόρμα συγκεκριμένες ωθεί δεν λειτουργούν).
- Αποτυχίες API (APIs ΑΠΟΤΥΧΙΑ συχνά σιωπηρά χωρίς μηνύματα σφάλματος).
- Αποτυχίες υπηρεσίας (καμία δέσμευση Mobile Azure λειτουργεί για την εφαρμογή σας).

### <a name="causes"></a>Έχει ως αποτέλεσμα

- Περισσότερα ζητήματα που πρέπει να επιλυθούν με το Azure Mobile δέσμευση SDK θα εντοπιστούν από ένα σφάλμα στην εφαρμογή σας (όπως αποτυχία συλλογής δεδομένων περιβάλλοντος εργασίας Χρήστη, αποτυχία push, αποτυχία δυνατότητα για προχωρημένους, αποτυχία API, σφάλματα εφαρμογών ή μη διαθεσιμότητα εμφανή υπηρεσίας).  
- Εάν μια συγκεκριμένη δυνατότητα της δέσμευσης Mobile Azure έχει ποτέ σε πριν από την εφαρμογή σας, θα πρέπει να ολοκληρώσετε την ενοποίηση. 
- Εάν μια συγκεκριμένη δυνατότητα της δέσμευσης Mobile Azure λειτουργούσε και διακοπεί, ίσως χρειαστεί να κάνετε αναβάθμιση στην τελευταία έκδοση με το Azure Mobile δέσμευση SDK. Να θυμάστε ότι υπάρχει μια διαφορετική έκδοση του SDK Azure Mobile δέσμευση για κάθε πλατφόρμα που υποστηρίζονται από το Azure δέσμευση Mobile (Android, iOS, τα Windows και Windows Phone).

#### <a name="sdk-integration"></a>Ενοποίηση του SDK

- Azure δέσμευση Mobile ενοποιημένο δεν σωστά σε SDK (ανάλυση).
- Επίτευξη δεν σωστά ενσωματωμένο στο SDK (στην εφαρμογή και εκτός προωθεί το App).
- Το πιστοποιητικό έχει λήξει ή είναι λανθασμένες ΕΙΔΏΝ έναντι Αποκλίσεις (iOS μόνο).
- GCM ή ADM ολοκλήρωμα δεν σωστά στο SDK (Android μόνο - υπηρεσία συγκεκριμένες προωθεί).
- Παρακολούθηση δεν σωστά ενσωματωμένο στο SDK (παρακολούθηση store εγκατάσταση).
- Τεμπέλη θέση ή θέση GPS δεν σωστά ενσωματωμένο στο SDK (Targeting από παν θέση).


**Δείτε επίσης:**

- [Τεκμηρίωση SDK - οδηγοί ενοποίησης][Link 5] 
- [Οδηγός αντιμετώπισης προβλημάτων - Push][Link 23]

#### <a name="sdk-upgrade"></a>Αναβάθμιση SDK

- Πρέπει να κάνετε αναβάθμιση SDK για την επίλυση προβλημάτων με παλαιότερες εκδόσεις του SDK (συχνά που σχετίζονται με νεότερες εκδόσεις της συσκευής OS).
- Καταργήστε όλες τις προηγούμενες εκδόσεις της εφαρμογής σας από τη συσκευή σας και επανεγκατάσταση η νεότερη έκδοση της εφαρμογής, η νέα καταχώρηση το Αναγνωριστικό συσκευής από το Azure Mobile δέσμευση περιβάλλον εργασίας Χρήστη του για να επιβεβαιώσετε ότι η νεότερη έκδοση της εφαρμογής σας χρησιμοποιεί η συσκευή σας.

**Δείτε επίσης:**

- [Τεκμηρίωση SDK - σημειώσεις έκδοσης](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [Τεκμηρίωση SDK - οδηγοί αναβάθμισης](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>SDK άλλες

- Σφάλματα σε εφαρμογή δήλωσης "AndroidManifest.xml" μπορεί να προκαλέσει δέσμευση Mobile Azure δεν θα λειτουργεί (μόνο για Android).
- Ένα κοινό ζήτημα με ενοποίησης SDK και η χρήση API είναι να μπερδέψει κανείς το κλειδί SDK και τον αριθμό-κλειδί API.

**Δείτε επίσης:**

- [Έννοιες - Γλωσσάρι][Link 6]

## <a name="advanced-coding-issues"></a>Κωδικοποίηση θέματα για προχωρημένους

### <a name="issue"></a>Το ζήτημα
-  Πλατφόρμα συγκεκριμένο κωδικό δεν συνδέεται αμέσως με δέσμευση Mobile Azure μπορεί να προκαλέσει ζητήματα σε iOS, Android και Windows Phone.

### <a name="causes"></a>Έχει ως αποτέλεσμα

- Πολλές coding θέματα με το Azure Mobile δέσμευση για προχωρημένους προκαλούνται από εσφαλμένη χειρόγραφων πλατφόρμα συγκεκριμένο κωδικό δεν απευθείας που σχετίζεται με δέσμευση Mobile Azure. Θα πρέπει να συμβουλευτείτε την τεκμηρίωση για την πλατφόρμα που αναπτύσσετε για εκτός από την τεκμηρίωση δέσμευση Mobile Azure (Android, iOS, Web, τα Windows και Windows Phone).
- Δεν σωστά τη ρύθμιση των παραμέτρων "Κατηγορίες", δεν επιτρέπει τη σύνδεση μια ειδοποίηση σε άλλη θέση, είτε βρίσκονται εντός ή εκτός της εφαρμογής (μόνο για Android). 
- Δεν η ρύθμιση "UIKit.framework" σε "προαιρετικό" στον κώδικά σας iOS, παρουσιάζει μια "Δεν βρέθηκε σφάλματος Symbol" ή/και παρουσιάζει σφάλμα σε παλαιότερες συσκευές iOS (iOS μόνο).
- Λήξει πιστοποιητικά ή δεν σωστά χρησιμοποιώντας την έκδοση Αποκλίσεις ή ειδών του του πιστοποιητικού, αιτίες προβλημάτων push (iOS μόνο).
- Υπάρχουν ορισμένοι περιορισμοί εγγενή μια πλατφόρμα που δεν είναι δυνατό να ελέγχουν δέσμευση Mobile Azure (όπως πώς λειτουργεί το Κέντρο συστήματος για από την εφαρμογή προωθεί στο Android και iOS).
- Azure δέσμευση Mobile δημοσιεύει μια πλήρη λίστα των πακέτων του εσωτερικού που χρησιμοποιούνται από Azure Mobile δέσμευση για iOS και Android για αναφορά. Λάβετε υπόψη ότι ορισμένες δυνατότητες του Azure Mobile δέσμευση αφορούν ειδικά την πλατφόρμα (Android, iOS, Web, τα Windows και Windows Phone).

### <a name="see-also"></a>Δείτε επίσης

 - [Οδηγός αντιμετώπισης προβλημάτων - Push][Link 23] 
 - [Τεκμηρίωση SDK - σημειώσεις έκδοσης][Link 5]
 - [Τεκμηρίωση SDK - οδηγοί αναβάθμισης][Link 5]

## <a name="application-crashes"></a>Σφάλματα εφαρμογών

### <a name="issue"></a>Το ζήτημα
- Η εφαρμογή σας παρουσιάζει σφάλμα στη συσκευή τους τελικούς χρήστες.

### <a name="causes"></a>Έχει ως αποτέλεσμα

- Σφάλμα πληροφορίες μπορούν να προβληθούν σε τα *Αναλυτικά στοιχεία περιβάλλοντος εργασίας Χρήστη* ή το *API ανάλυσης*
- Μπορείτε να βρείτε το Αναγνωριστικό της συσκευής σας συσκευής δοκιμής και να λαμβάνουν την ίδια ενέργεια που προκάλεσαν την εφαρμογή με το σφάλμα για τον τελικό χρήστη για να προσδιορίσετε την αιτία της το σφάλμα.
- Μερικές φορές επιλύονται γνωστά θέματα με το Azure Mobile δέσμευση SDK που προκαλούν εφαρμογές να παρουσιάσει σφάλμα κατά την αναβάθμιση στην πιο πρόσφατη έκδοση του SDK. Βεβαιωθείτε ότι για να ελέγξετε τις σημειώσεις έκδοσης σχετικά με την πλατφόρμα σας όταν Διερεύνηση παρουσιάσει σφάλμα.

### <a name="see-also"></a>Δείτε επίσης

- [Τεκμηρίωση SDK - σημειώσεις έκδοσης][Link 5]
- [Τεκμηρίωση SDK - οδηγοί αναβάθμισης][Link 5]

## <a name="app-store-upload-failures"></a>Αποτυχίες αποστολής App store

### <a name="issue"></a>Το ζήτημα
- Σφαλμάτων που σχετίζονται με την αποστολή την πιο πρόσφατη έκδοση της εφαρμογής σας σε Apple, Google ή την εφαρμογή Windows store.

### <a name="causes"></a>Έχει ως αποτέλεσμα

- Εφαρμογή αποθηκεύει μερικές φορές εφαρμογές μπλοκ με ορισμένες δυνατότητες που ενεργοποιούνται (π.χ. το Apple Store, δεν επιτρέπει τη χρήση του IDFV στις εφαρμογές στο χώρο αποθήκευσης και το χώρο αποθήκευσης GooglePlay αποτρέπει την κοινή χρήση των πληροφοριών εφαρμογής μεταξύ των εφαρμογών). 
- Βεβαιωθείτε ότι μπορείτε να ελέγξετε τις σημειώσεις έκδοσης σχετικά με την πλατφόρμα και την τρέχουσα έκδοση του SDK εάν έχετε δυσκολία κατά την αποστολή μιας εφαρμογής για το χώρο αποθήκευσης.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
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
 
