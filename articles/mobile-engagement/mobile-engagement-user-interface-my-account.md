<properties 
   pageTitle="Περιβάλλον εργασίας χρήστη Azure δέσμευση κινητές συσκευές - ο λογαριασμός μου" 
   description="Μάθετε πώς να διαχειρίζεστε λογαριασμού προφίλ και δοκιμής τις συσκευές σας χρησιμοποιώντας Azure Mobile δέσμευση" 
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

# <a name="how-to-manage-your-account-profile-and-test-devices"></a>Πώς μπορείτε να διαχειριστείτε τις συσκευές προφίλ και τις επιλογές σας λογαριασμό
 
Σε αυτό το άρθρο περιγράφει την **κεντρική** σελίδα της πύλης **Δέσμευση Mobile** . Μπορείτε να χρησιμοποιήσετε την πύλη **Mobile δέσμευση** να παρακολουθούν και να διαχειριστείτε τις εφαρμογές για κινητές συσκευές. 
 
Για να μεταβείτε στη σελίδα " **ο λογαριασμός μου** ", κάντε κλικ στο λογαριασμό σας στο επάνω μέρος της σελίδας.

Στην ενότητα My Account του περιβάλλοντος εργασίας Χρήστη του είναι όπου μπορείτε να προβάλετε και να αλλάξετε τις ρυθμίσεις που σχετίζονται με το λογαριασμό σας, συμπεριλαμβανομένων των ρυθμίσεων του προφίλ σας και να ελέγξετε αναγνωριστικά συσκευής. Αυτές οι ρυθμίσεις περιέχει στοιχεία που έχετε πρόσβαση μέσω του API συσκευή.

![MyAccount1][7]  

## <a name="profile"></a>Προφίλ:
Μπορείτε να προβάλετε ή να αλλάξετε οποιαδήποτε από τις ρυθμίσεις του λογαριασμού που φαίνεται παρακάτω. Μπορείτε επίσης να δώσετε μια άλλη δικαιωμάτων χρήστη για να χρησιμοποιήσετε την εφαρμογή σας με βάση τη διεύθυνση ηλεκτρονικού ταχυδρομείου από το [σπίτι](mobile-engagement-user-interface-home.md).

![MyAccount2][8]  

## <a name="devices"></a>Συσκευές:
Μπορείτε να προβάλετε, να προσθέσετε ή να καταργήσετε δοκιμή συσκευή αναγνωριστικά των συσκευών δοκιμής που μπορείτε να χρησιμοποιήσετε για να ελέγξετε την **επίτευξη** ή **push** εκστρατείες. Με βάση τα συμφραζόμενα οδηγίες για το πώς μπορείτε να βρείτε το Αναγνωριστικό συσκευής συσκευών για κάθε πλατφόρμα (iOS, Android, Windows Phone, κ.λπ.) εμφανίζονται όταν κάνετε κλικ στην επιλογή "Νέα συσκευή". 
 
![MyAccount3][9]  
 
Για να χρησιμοποιήσετε Push API ή τη συσκευή API πρέπει να γνωρίζετε τους χρήστες σας μοναδικό αναγνωριστικό συσκευής (η παράμετρος deviceid). Υπάρχουν πολλοί τρόποι για να ανακτήσετε:
 
1. Από την πλευρά του διακομιστή, μπορείτε να χρησιμοποιήσετε τη δυνατότητα "Λήψη" του API συσκευή για να λάβετε την πλήρη λίστα των αναγνωριστικών συσκευή.
2. Από την εφαρμογή, μπορείτε να χρησιμοποιήσετε το SDK να το λάβετε. (Σε Android, κλήση της συνάρτησης getDeviceID() της κλάσης παράγοντα και σε iOS, διαβάστε την ιδιότητα της κλάσης παράγοντας.)
3. Από μια ανακοίνωση Reach, εάν η διεύθυνση URL ενέργειας που σχετίζεται με την ανακοίνωση περιέχει το μοτίβο {deviceid}, αυτό θα αντικατασταθούν αυτόματα από το αναγνωριστικό της συσκευής Ενεργοποίηση την ενέργεια.
http://<example>.com/registeruser?deviceid = {deviceid} & otherparam = myparamdata θα αντικατασταθούν από: http://<example>.com/registeruser?deviceid = XXXXXXXXXXXXXXXX & otherparam = myparamdata 
4. Από μια ανακοίνωση web Reach, εάν ο κώδικας HTML από την ανακοίνωση περιέχει το μοτίβο {deviceid}, αυτό θα αντικατασταθούν αυτόματα από το αναγνωριστικό της συσκευής εμφανίζει την ανακοίνωση web.
Εδώ είναι το αναγνωριστικό συσκευής: {deviceid} θα αντικατασταθούν από: Εδώ είναι το αναγνωριστικό συσκευής: XXXXXXXXXXXXXXXX
5.  Ανοίξτε την εφαρμογή σας στη συσκευή σας και εκτελέστε συμβάντος στην εφαρμογή που έχει προστεθεί ετικέτα.
Από το "Στοιχεία περιβάλλοντος εργασίας Χρήστη - συμβάντα σας εφαρμογή - οθόνη - -", βρείτε το συμβάν που έχει πραγματοποιηθεί από τη λίστα.
Κάντε κλικ σε αυτό το συμβάν στην οθόνη.
Πρέπει να βρείτε το Αναγνωριστικό συσκευής στη λίστα με τις συσκευές που έχετε πραγματοποιήσει αυτό το συμβάν.
Στη συνέχεια, μπορείτε να αντιγράψετε αυτό το Αναγνωριστικό συσκευής και να καταχωρήσετε στο "Περιβάλλον εργασίας Χρήστη - ο λογαριασμός μου - συσκευές - νέα συσκευή - επιλέξτε την πλατφόρμα σας συσκευής".
>(Έχετε υπόψη ότι όταν IDFA είναι απενεργοποιημένη για iOS, το Αναγνωριστικό συσκευής μπορεί να αλλάξει το σταδιακά Εάν καταργήσετε την εγκατάσταση και επαναλάβετε την εγκατάσταση της εφαρμογής σας.)

##<a name="troubleshooting-guide"></a>Οδηγός αντιμετώπισης προβλημάτων
-  [Οδηγός αντιμετώπισης προβλημάτων - υπηρεσίας][Link 24]

## <a name="see-also"></a>Δείτε επίσης
-  [Περιβάλλον εργασίας Χρήστη τεκμηρίωση - κεντρική][Link 13]


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


 
 
