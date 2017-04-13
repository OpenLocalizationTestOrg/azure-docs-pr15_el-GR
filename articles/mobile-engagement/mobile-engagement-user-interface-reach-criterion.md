<properties 
   pageTitle="Περιβάλλον εργασίας χρήστη Azure δέσμευση κινητές συσκευές - Reach κριτηρίου" 
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε στόχευσης κριτήρια για την αποστολή εκστρατείες push σε ένα υποσύνολο επιλέξτε τους χρήστες σας με χρήση Azure Mobile δέσμευση" 
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


# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Πώς μπορείτε να χρησιμοποιήσετε στόχευσης κριτήρια για να στείλετε εκστρατείες push σε ένα υποσύνολο επιλέξτε τους χρήστες σας

Στόχευσης ακροατηρίου σας βάσει συγκεκριμένων κριτηρίων με το κουμπί "Νέα κριτήρια" είναι μία από τις πιο ισχυρούς έννοιες δέσμευση Mobile Azure που σας βοηθά να στέλνετε σχετικές ειδοποιήσεις push ότι οι πελάτες θα απαντούν σε αντί για αποστολή όλων. Μπορείτε να περιορίσετε το ακροατήριό σας με βάση κριτήρια τυπική και να προσομοιώσετε ωθεί για να καθορίσετε πόσα άτομα θα λάβουν την ειδοποίηση.

**Δείτε επίσης:**

- [Περιβάλλον εργασίας Χρήστη τεκμηρίωση - Reach - νέα εκστρατεία Push][Link 27]

## <a name="audience-criteria-can-include"></a>Κριτήρια ακροατηρίου μπορούν να περιλαμβάνουν:
- **Technicals:** Μπορείτε να στοχεύετε με βάση τις ίδιες τεχνικές πληροφορίες μπορείτε να δείτε στις ενότητες ανάλυση και οθόνη. **Δείτε επίσης:** [Περιβάλλον εργασίας Χρήστη τεκμηρίωση - ανάλυση] [ Link 15], [Τεκμηρίωση περιβάλλοντος εργασίας Χρήστη - οθόνη][Link 16]
- **Θέση:** Εφαρμογές που χρησιμοποιούν "πραγματικό χρόνο θέση αναφοράς" με χωριστή παρουσίαση παν και διαχείριση μπορούν να χρησιμοποιήσουν παν θέση ως κριτήρια στόχευσης ακροατηρίου από τη θέση GPS. Κλήση "Τεμπέλη περιοχή θέση αναφοράς" επίσης να χρησιμοποιηθούν για στοχεύετε ένα ακροατήριο από τη θέση του κινητού τηλεφώνου ("πραγματικό χρόνο θέση αναφοράς" και "Τεμπέλη περιοχή θέση αναφοράς" πρέπει να ενεργοποιηθεί από το SDK). **Δείτε επίσης:** [Τεκμηρίωση SDK - iOS - ενοποίησης] [ Link 5], [- Android - ενοποίησης τεκμηρίωση SDK][Link 5]
- **Επίτευξη σχολίων:** Μπορείτε να προσδιορίσετε το ακροατήριό σας με βάση τα σχόλια από προηγούμενη reach ειδοποιήσεις μέσω reach σχολίων από ανακοινώσεις, ψηφοφορίες και προωθεί δεδομένα. Αυτή η δυνατότητα επιτρέπει καλύτερη προορισμού το ακροατήριό σας μετά από δύο ή τρεις επίτευξη εκστρατείες από αυτήν που μπορεί να την πρώτη φορά. Μπορεί επίσης να χρησιμοποιηθεί για να φιλτράρεται τους χρήστες που ήδη λάβει μια ειδοποίηση με παρόμοιο περιεχόμενο, με τη ρύθμιση μιας εκστρατείας δεν να αποστέλλονται οι χρήστες που ήδη λάβει μια συγκεκριμένη εκστρατεία προηγούμενη. Μπορείτε ακόμα να εξαιρέσετε τους χρήστες που έχουν συμπεριληφθεί μια συγκεκριμένη εκστρατεία που είναι ακόμη ενεργή από τη λήψη νέου ωθεί. **Δείτε επίσης:** [Τεκμηρίωση του περιβάλλοντος εργασίας Χρήστη - επίτευξη - Push περιεχομένου][Link 29]
- **Εγκατάσταση παρακολούθησης:** Μπορείτε να παρακολουθείτε πληροφορίες με βάση όπου οι χρήστες σας εγκαταστήσει την εφαρμογή σας. **Δείτε επίσης:** [Περιβάλλον εργασίας Χρήστη τεκμηρίωση - ρυθμίσεις][Link 20]
- **Προφίλ χρήστη:** Μπορείτε να στοχεύετε σε τυπική βάσει πληροφοριών χρήστη και μπορείτε να προορισμού με βάση τις πληροφορίες προσαρμοσμένης εφαρμογής που έχετε δημιουργήσει. Περιλαμβάνονται και οι χρήστες που είναι συνδεδεμένοι στο και οι χρήστες που έχουν απαντηθεί συγκεκριμένες ερωτήσεις που έχετε ζητήσει να οριστεί στην ίδια την εφαρμογή αντί μόνο πώς τους έχετε απαντήσει σε προηγούμενη εκστρατείες. Όλα τα στοιχεία σας της εφαρμογής που ορίζεται για την εφαρμογή παρουσίασης προς τα επάνω σε αυτήν τη λίστα.
- Τμήματα: Μπορείτε επίσης να προορισμού που βασίζονται σε τμήματα που έχετε δημιουργήσει με βάση τη συμπεριφορά συγκεκριμένο χρήστη που περιέχει πολλά κριτήρια. Όλα τα τμήματα αγοράς που ορίζονται για την εφαρμογή σας εμφανίζονται σε αυτήν τη λίστα. **Δείτε επίσης:** [Περιβάλλον εργασίας Χρήστη τεκμηρίωση - τμήματα][Link 18]
- **Πληροφορίες app:** Προσαρμοσμένες ετικέτες πληροφορίες εφαρμογής μπορεί να δημιουργηθεί από τις "Ρυθμίσεις" για να παρακολουθείτε συμπεριφορά χρήστη. **Δείτε επίσης:** [Περιβάλλον εργασίας Χρήστη τεκμηρίωση - ρυθμίσεις][Link 20]

## <a name="example"></a>Παράδειγμα: 
Εάν θέλετε να προωθήσετε μια ανακοίνωση μόνο σε συνόλου δευτερεύουσες τους χρήστες που έχετε κάνει μια ενέργεια αγοράς της εφαρμογής σας.

1. Μεταβείτε στη σελίδα ρυθμίσεων της εφαρμογής σας, επιλέξτε το μενού "Εφαρμογή πληροφορίες" και επιλέξτε "Νέα εφαρμογή πληροφορίες"
2. Καταχώρηση ενός νέου πληροφορίες δυαδική app που ονομάζεται "inAppPurchase"
3. Κάνετε εφαρμογή σας, ορίστε αυτήν πληροφορίες app "TRUE" όταν ο χρήστης εκτελεί με επιτυχία μια αγορά της εφαρμογής (χρησιμοποιώντας το sendAppInfo ("inAppPurchase",...) συνάρτηση)
4. Εάν δεν θέλετε να το κάνετε αυτό από την εφαρμογή σας, μπορείτε να το κάνετε από τον υπολογιστή στο παρασκήνιο, χρησιμοποιώντας τη συσκευή API)
5. Στη συνέχεια, απλώς πρέπει να δημιουργήσετε την ανακοίνωση, με ένα κριτήριο τον περιορισμό στους χρήστες χρειάζεται "inAppPurchase" ρύθμιση για το ακροατήριό σας "true")
 
> Σημείωση: Στόχευσης με βάση κριτήρια εκτός από το app πληροφορίες ετικετών απαιτεί Azure Mobile δέσμευση για τη συλλογή πληροφοριών από συσκευές των χρηστών σας πριν από την αποστολή του push και έτσι μπορεί να καθυστερήσει. Ρύθμιση παραμέτρων push σύνθετες επιλογές (όπως ενημέρωση σημάτων) μπορούν επίσης να καθυστερήσουν προωθεί. Χρήση μιας εκστρατείας "ένα στιγμιότυπο" από το API Push είναι η απόλυτη πιο ασφαλής μέθοδος push στο Azure Mobile δέσμευση. Χρήση μόνο εφαρμογή ετικετών πληροφορίες ως κριτήρια push για μιας εκστρατείας Reach (είτε από το API επίτευξη ή το περιβάλλον εργασίας Χρήστη) είναι την επόμενη μέθοδο πιο ασφαλής, εφόσον εφαρμογή ετικετών πληροφορίες αποθηκεύονται στο διακομιστή. Με άλλα στόχευσης κριτήρια για μια εκστρατεία push είναι η πιο ευέλικτη αλλά χαμηλότερη μέθοδος push εφόσον έχει δέσμευση Mobile Azure ερώτημα για τις συσκευές για να στείλετε την εκστρατεία.
 
![Reach Criterion1][29] 

## <a name="criterion-options-apply-to"></a>Επιλογές κριτηρίου ισχύουν για:
- **Technicals**     
- Όνομα υλικολογισμικού: όνομα υλικολογισμικού
- Έκδοση υλικολογισμικού: έκδοση υλικολογισμικού
- Μοντέλο συσκευής: μοντέλο της συσκευής
- Τον κατασκευαστή της συσκευής: κατασκευαστή της συσκευής
- Η έκδοση της εφαρμογής: η έκδοση της εφαρμογής
- Όνομα συλλαβισμού: όνομα συλλαβισμού, Απροσδιόριστη
- Χώρα συλλαβισμού: χώρα συλλαβισμού, Απροσδιόριστη
- Τύπος δικτύου: τύπος δικτύου
- Τοπικές ρυθμίσεις: τοπικές ρυθμίσεις
- Μέγεθος οθόνης: μέγεθος οθόνης
- **Θέση**      
- Τελευταία γνωστή περιοχή: χώρα, περιοχή, τοποθεσία
- Πραγματικό χρόνο παν-περίφραξη: λίστα POIs (όνομα, ενέργειες), κυκλική κατεύθυνση δε (όνομα, γεωγραφικό πλάτος, μήκος, ακτίνα σε μέτρα)
- **Επίτευξη σχολίων**     
- Ανακοίνωση σχολίων: ανακοίνωση, σχολίων
- Ψηφοφορία με σχόλια: ψηφοφορίας, σχολίων
- Ψηφοφορία με σχόλια απαντήσεων: ψηφοφορία απαντήσεων σχολίων, ερώτηση, επιλογή
- Σχόλια Push δεδομένων: Push δεδομένων, σχολίων
- **Εγκατάσταση παρακολούθησης**     
- Αποθήκευση: Χώρος αποθήκευσης, δεν οριστεί
- Προέλευση: Προέλευση, Απροσδιόριστη
- **Προφίλ χρήστη**     
- Gender: άντρες ή γυναίκες, Απροσδιόριστη
- Ημερομηνία γέννησης: τελεστή, ημερομηνία Απροσδιόριστη
- Επιλογή: true ή false, δεν έχει οριστεί
- **Πληροφορίες App**      
- Συμβολοσειρά: Συμβολοσειρά, Απροσδιόριστη
- Ημερομηνία: τελεστής, ημερομηνία Απροσδιόριστη
- Ακέραιο: τελεστής, αριθμός, Απροσδιόριστη
- Δυαδική τιμή: τιμή true ή false, Απροσδιόριστη
- **Το τμήμα**    
- Όνομα τμήματα (από την αναπτυσσόμενη λίστα), αποκλεισμού (προορισμού οι χρήστες που δεν είναι μέρος αυτού του τμήματος).

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
 
