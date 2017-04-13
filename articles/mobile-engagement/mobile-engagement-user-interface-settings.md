<properties 
   pageTitle="Περιβάλλον εργασίας χρήστη Azure δέσμευση κινητές συσκευές - ρυθμίσεις" 
   description="Μάθετε πώς μπορείτε να διαχειριστείτε τις καθολικές ρυθμίσεις της εφαρμογής σας με χρήση Azure Mobile δέσμευση" 
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

# <a name="how-to-manage-the-global-settings-of-your-application"></a>Πώς μπορείτε να διαχειριστείτε τις καθολικές ρυθμίσεις της εφαρμογής σας

Οι **Ρυθμίσεις** μενού διαθέσιμες επιλογές για ένα διαφορετικό εφαρμογής, ανάλογα με την πλατφόρμα της εφαρμογής και τα δικαιώματα που σας έχουν παραχωρηθεί για την εφαρμογή. Ρυθμίσεις περιλαμβάνουν τα εξής: λεπτομέρειες έργα, εγγενή Push, Push ταχύτητα, ετικέτα (πληροφορίες app) και εμπορική πίεση. Η επιλογή μενού (πληροφορίες app) ετικέτας από την ενότητα ρυθμίσεις μπορεί να γίνει διαχείριση από την εφαρμογή σας (χρησιμοποιώντας το SDK) ή από τον υπολογιστή στο παρασκήνιο (με χρήση του API συσκευή). 


>[AZURE.NOTE] Πολλές ενότητες της πύλης **Δέσμευση Mobile** περιβάλλοντος εργασίας Χρήστη περιέχουν το κουμπί **ΕΜΦΆΝΙΣΗ ΒΟΉΘΕΙΑΣ** . Πατήστε αυτό το κουμπί για να λάβετε περισσότερες πληροφορίες με βάση τα συμφραζόμενα σχετικά με μια ενότητα.

## <a name="details"></a>Λεπτομέρειες

Σας επιτρέπει να αλλάξετε το όνομα και μια περιγραφή της εφαρμογής σας, να προβάλετε τον κάτοχο του τα δικαιώματά σας ρόλο και της εφαρμογής σας. 

Ρύθμιση παραμέτρων ανάλυση σάς επιτρέπει να προβάλετε ή να αλλάξετε την ημέρα εβδομάδες Έναρξη στην και το χρονικό διάστημα διατήρησης σε ημέρες.
 
  ![settings1][46]
 
## <a name="projects"></a>Έργα

Σας επιτρέπει να επιλέξετε όλα τα έργα που θέλετε να εμφανίζονται στην εφαρμογή σας. 

Επίσης, μπορείτε να πραγματοποιήσετε αναζήτηση για ένα έργο και να προβάλετε το όνομα, περιγραφή, κάτοχο και τα δικαιώματά σας ρόλο οποιαδήποτε εφαρμογή σας αποτελεί μέρος του έργου.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα: [Τεκμηρίωση του περιβάλλοντος εργασίας Χρήστη – για οικιακή χρήση][Link 13]
 
  ![settings3][48]

## <a name="native-push"></a>Εγγενής Push

Μπορείτε να καταχωρήσετε ένα νέο πιστοποιητικό ή διαγραφή και ένα υπάρχον πιστοποιητικό για χρήση με εγγενή push. Εγγενής Push επιτρέπει δέσμευση Mobile Azure για να προωθήσετε στην εφαρμογή σας οποιαδήποτε στιγμή, ακόμα και όταν δεν εκτελείται. 

Μετά την παροχή διαπιστευτήρια ή τα πιστοποιητικά για τουλάχιστον μία υπηρεσία εγγενούς Push, μπορείτε να επιλέξετε "Οποιαδήποτε στιγμή" κατά τη δημιουργία επίτευξη εκστρατείες, καθώς και χρησιμοποιήστε την παράμετρο "Ειδοποίηση" στο PUSH API.



### <a name="apple-push-notification-service-apns"></a>Υπηρεσία ειδοποιήσεων Push Apple (APNS)

Για να ενεργοποιήσετε την με την υπηρεσία ειδοποιήσεων Push Apple Push εγγενούς θα πρέπει να καταχωρήσετε το πιστοποιητικό. Θα πρέπει να καθορίσετε τον τύπο του πιστοποιητικού ως ανάπτυξης (Αποκλίσεις) ή παραγωγής (ΕΙΔΏΝ). Στη συνέχεια, που θα πρέπει αποστείλετε το πιστοποιητικό σας και τον κωδικό πρόσβασης.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα: [τεκμηρίωση SDK - iOS - τον τρόπο προετοιμασίας της εφαρμογής για τις ειδοποιήσεις Push της Apple][Link 5]
 
![settings4][49]
 
### <a name="windows-push-notification-service-wpns"></a>Υπηρεσία ειδοποιήσεων Push των Windows (WPNS)

Για να ενεργοποιήσετε χρησιμοποιώντας Windows υπηρεσία ειδοποιήσεων Push εγγενή, πρέπει να δώσετε τα διαπιστευτήρια της εφαρμογής σας. Θα χρειαστείτε το αναγνωριστικό ασφάλεια πακέτου (αναγνωριστικό ΑΣΦΑΛΕΊΑΣ) και το μυστικό κλειδί.
 
![settings5][50]
 
### <a name="google-cloud-messaging-for-android-gcm"></a>Google Cloud ανταλλαγής μηνυμάτων για Android (GCM)

Για να ενεργοποιήσετε την εγγενή Push χρησιμοποιώντας GCM, που πρέπει να ακολουθήσετε τις οδηγίες από το Google. Στη συνέχεια, πρέπει να το επικολλήσετε ένα διακομιστή απλού API κλειδί, έχει ρυθμιστεί χωρίς περιορισμούς διευθύνσεων IP. Απαιτεί ενοποίηση με το SDK για Android v1.12.0 +.

Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα: 

- [Android τεκμηρίωση SDK πώς μπορείτε να ενοποιήσετε GCM][Link 5]
- [Οδηγός GCM Google για προγραμματιστές](http://developer.android.com/guide/google/gcm/gs.html)
 
### <a name="amazon-device-messaging-for-android-adm"></a>Συσκευή Amazon ανταλλαγής μηνυμάτων για Android (ADM)

Για να ενεργοποιήσετε την εγγενή Push χρησιμοποιώντας ADM, πρέπει να παρέχετε Amazon <OAuth credentials> που αποτελείται από ένα Αναγνωριστικό υπολογιστή-πελάτη και μυστικό προγράμματος-πελάτη (απαιτείται η ενοποίηση με το SDK για Android v2.1.0 +).

Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα: 

- [Android τεκμηρίωση SDK πώς μπορείτε να ενοποιήσετε ADM][Link 5]
- [Τεκμηρίωση ADM Amazon για προγραμματιστές](https://developer.amazon.com/sdk/adm/credentials.html#Getting)
 
![settings6][51]

## <a name="push-speed"></a>Ταχύτητα Push

Εμφανίζει την τρέχουσα ταχύτητα push της εφαρμογής σας και σας επιτρέπει να ορίσετε την ταχύτητα push της εφαρμογής σας.
 
  ![settings7][52]

## <a name="tag-app-info"></a>Ετικέτα (πληροφορίες app)

![settings11][56]
  
## <a name="commercial-pressure"></a>Εμπορική πίεσης


![settings12][57]


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
 
