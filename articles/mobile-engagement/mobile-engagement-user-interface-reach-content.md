<properties 
   pageTitle="Περιβάλλον εργασίας χρήστη Azure δέσμευση κινητές συσκευές - Reach περιεχομένου" 
   description="Μάθετε πώς μπορείτε να διαχειριστείτε το μοναδικό περιεχόμενο από τους διαφορετικούς τύπους καμπάνιες ειδοποιήσεων push στο Azure Mobile δέσμευση" 
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

# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a>Πώς μπορείτε να διαχειριστείτε το μοναδικό περιεχόμενο από τους διαφορετικούς τύπους εκστρατείες ειδοποιήσεων push
 
Μπορείτε να χρησιμοποιήσετε την ενότητα περιεχομένου μιας νέας εκστρατείας reach για να τροποποιήσετε το περιεχόμενο του σας ανακοινώσεις, ψηφοφορίες, προωθεί δεδομένων και τα πλακίδια (μόνο για Windows Phone). Η ρύθμιση περιεχομένου καμπάνιες Push είναι συγκεκριμένη για τον τύπο της εκστρατείας. 
 
### <a name="content-types"></a>Τύποι περιεχομένου:
- Ανακοινώσεις
- Ψηφοφοριών
- Ωθεί δεδομένων
- Τα πλακίδια (μόνο για Windows Phone)
 
## <a name="content-of-announcements"></a>Περιεχόμενο του ανακοινώσεις
 ![Reach Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a>Επιλέξτε τον τύπο της ανακοίνωση σας:
-    Ειδοποίηση μόνο: είναι μια απλή τυπική ειδοποίηση. Σημαίνει ότι, εάν ένας χρήστης κάνει κλικ σε αυτό, χωρίς επιπλέον προβολή θα εμφανίζονται, αλλά μόνο την ενέργεια που έχει συσχετιστεί με αυτό θα προκύψει.
-    Ανακοίνωση κειμένου: είναι μια ειδοποίηση που εμπλέκεται ο χρήστης να έχει μια ματιά στην προβολή κειμένου.
-    Ανακοίνωση Web: είναι μια ειδοποίηση που εμπλέκεται ο χρήστης να έχει μια ματιά σε μια προβολή web.

### <a name="see-also"></a>Δείτε επίσης
- [Επίτευξη - πώς διαδικασίες - ανακοινώσεις][Link 3] 

### <a name="about-web-view-announcements"></a>Σχετικά με την προβολή Web ανακοινώσεων:
Εμφανίσεις του μοτίβου "{deviceid}" στον κώδικα HTML ή κώδικα JavaScript που παρέχετε εδώ θα αντικατασταθούν αυτόματα από το αναγνωριστικό της συσκευής εμφανίζει την ανακοίνωση. Αυτό είναι ένας εύκολος τρόπος για να ανακτήσετε δέσμευση Mobile Azure συσκευή αναγνωριστικά σε μια υπηρεσία εξωτερικών web που φιλοξενείται στο γραφείο σας πίσω.
Εάν θέλετε να δημιουργήσετε μια προβολή web σε πλήρη οθόνη (χωρίς την προεπιλεγμένη ενέργεια έξοδος κουμπιά και παρέχουμε) μπορείτε να χρησιμοποιήσετε τις ακόλουθες συναρτήσεις από το web ανακοίνωση προβολής κώδικα JavaScript: 

-    εκτέλεση της ενέργειας ανακοίνωση: ReachContent.actionContent()
-    Έξοδος από την ανακοίνωση: ReachContent.exitContent()
 
### <a name="choose-your-action"></a>Επιλέξτε την ενέργεια:

### <a name="about-action-urls"></a>Σχετικά με τις διευθύνσεις URL ενέργεια:
Οποιαδήποτε διεύθυνση URL που μπορεί να ερμηνεύεται από μια συσκευή-στόχο λειτουργικό σύστημα μπορεί να χρησιμοποιηθεί ως μια διεύθυνση URL ενέργειας.
Οποιαδήποτε αφοσιωμένη διεύθυνσης URL που μπορεί να υποστηρίζει την εφαρμογή σας (π.χ. για να κάνετε Μετάβαση σε συγκεκριμένη οθόνη χρήστες) μπορεί επίσης να χρησιμοποιηθεί ως μια διεύθυνση URL ενέργειας.
Κάθε παρουσία του μοτίβου {deviceid} αντικαθίσταται αυτόματα από το αναγνωριστικό της συσκευής εκτέλεση της ενέργειας. Αυτό μπορεί να χρησιμοποιηθεί για την ανάκτηση εύκολα αναγνωριστικά συσκευή δέσμευση Mobile Azure μέσω μιας υπηρεσίας web εξωτερικών που φιλοξενούνται στο γραφείο σας πίσω.

- **Android + iOS ενέργειες**
    - Ανοίξτε μια ιστοσελίδα
    - http://\[τομέα τοποθεσίας web\] 
    - Παράδειγμα: http://www.azure.com
    - Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου
    - mailto:\[ηλεκτρονικού ταχυδρομείου παραλήπτη\]?subject =\[θέμα\]& σώμα =\[μηνύματος\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Αποστολή ενός SMS
    - SMS:\[αριθμός τηλεφώνου\] 
    - Παράδειγμα: sms:2125551212
    - Κλήση αριθμού τηλεφώνου
    - Τηλ.:\[αριθμός τηλεφώνου\] 
    - Παράδειγμα: tel:2125551212
- **Android μόνο ενέργειες**
    - Λήψη μια εφαρμογή για το Play Store
    - market://details?id=\[πακέτο εφαρμογών\] 
    - Παράδειγμα: market://details?id=com.microsoft.office.word
    - Έναρξη μιας αναζήτησης παν μεταφραστεί
    - GEO:0, 0?q =\[ερώτημα αναζήτησης\] 
    - Παράδειγμα: geo:0, 0; ερωτήσεις = starbucks, Παρίσι
- **μόνο ενέργειες iOS**
    - Λήψη μια εφαρμογή για το App Store
    - /id /app/ [όνομα εφαρμογής] http://iTunes.Apple.com/ [country] [αναγνωριστικό εφαρμογής]; mt = 8 
    - Παράδειγμα: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
    - Ενέργειες των Windows
    - Ανοίξτε μια ιστοσελίδα
    - http://\[τομέα τοποθεσίας web\] 
    - Παράδειγμα: http://www.azure.com
    - Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου
    - mailto:\[ηλεκτρονικού ταχυδρομείου παραλήπτη\]?subject =\[θέμα\]& σώμα =\[μηνύματος\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Αποστολή ενός SMS (Skype Store App απαιτείται)
    - SMS:\[αριθμός τηλεφώνου\] 
    - Παράδειγμα: sms:2125551212
    - Κλήση αριθμού τηλεφώνου (Skype Store App απαιτείται)
    - Τηλ.:\[αριθμός τηλεφώνου\] 
    - Παράδειγμα: tel:2125551212
    - Λήψη μια εφαρμογή για το Play Store
    - MS-windows-store: PDP?PFN =\[Αναγνωριστικό πακέτου εφαρμογής\] 
    - Παράδειγμα: ms-windows-store: PDP; PFN = 4d91298a-07cb-40fb-aecc-4cb5615d53c1
    - Έναρξη μιας αναζήτησης bingmaps
    - bingmaps:?q =\[ερώτημα αναζήτησης\] 
    - Παράδειγμα: bingmaps:; ερωτήσεις = starbucks, Παρίσι
    - Χρησιμοποιήστε έναν προσαρμοσμένο συνδυασμό
    - \[προσαρμοσμένο συνδυασμό\]://\[παραμέτρων προσαρμοσμένου συνδυασμού\] 
    - Παράδειγμα: myCustomProtocol://myCustomParams
    - Χρήση του πακέτου δεδομένων (Store App για την επέκταση Διαβάστε απαιτείται)
    - \[φάκελος\]\[δεδομένων\]. \[επέκταση\] 
    - Example:myfolderdata.txt
 
### <a name="build-a-tracking-url"></a>Δημιουργία μιας διεύθυνσης URL παρακολούθηση:
-    Ανατρέξτε στην ενότητα "Ρυθμίσεις" του <UI Documentation> για οδηγίες σχετικά με τη δημιουργία μιας διεύθυνσης URL παρακολούθησης που θα επιτρέπει στους χρήστες για να κάνετε λήψη μία από τις άλλες εφαρμογές.
 
### <a name="define-the-texts-of-your-announcement"></a>Ορισμός του κειμένου των σας ανακοίνωση
Συμπληρώστε την τίτλος, περιεχόμενο και κουμπί κείμενο των ανακοίνωση σας. Μπορείτε να στοχεύετε ένα ακροατήριο μελλοντική εκστρατείας με βάση τα σχόλια reach του πώς οι χρήστες απαντήσει αυτής της εκστρατείας. Προσδιορισμού ακροατηρίου μπορεί να είναι με βάση τα σχόλια του εάν αυτής της εκστρατείας έχει απλώς προωθηθεί, έχει απαντηθεί, actioned ή εξέλθουν.

### <a name="see-also"></a>Δείτε επίσης
- [Περιβάλλον εργασίας Χρήστη τεκμηρίωση - Reach - νέα Push κριτηρίου][Link 28]

## <a name="content-of-polls"></a>Περιεχόμενο του ψηφοφοριών
![Reach Content2][31] συμπληρώστε το τίτλος, περιγραφή και κουμπί κείμενο των σας ανακοίνωση. Στη συνέχεια, προσθέστε ερωτήσεις και επιλογές για τις απαντήσεις στις ερωτήσεις σας.
Μπορείτε να στοχεύετε ένα ακροατήριο μελλοντική εκστρατείας με βάση τα σχόλια reach του πώς οι χρήστες απαντήσει αυτής της εκστρατείας. Προσδιορισμού ακροατηρίου μπορεί να βασίζονται σε εάν αυτής της εκστρατείας έχει απλώς προωθηθεί, έχει απαντηθεί, actioned ή εξέλθουν. Προσδιορισμού ακροατηρίου μπορεί επίσης να βασίζονται σε σχόλια απαντήσεων ψηφοφορίας, όπου η ερώτηση και η επιλογή απαντήσεων που χρησιμοποιούνται ως κριτήρια.

### <a name="see-also"></a>Δείτε επίσης
- [Περιβάλλον εργασίας Χρήστη τεκμηρίωση - Reach - νέα Push κριτηρίου][Link 28]
 
## <a name="content-of-data-pushes"></a>Προωθεί το περιεχόμενο των δεδομένων
![Reach Content3][32] 

### <a name="choose-the-type-of-your-data"></a>Επιλέξτε τον τύπο των δεδομένων σας:
- Κείμενο
- Δυαδικά δεδομένα
- Δεδομένα Base64

### <a name="define-the-content-of-your-data"></a>Ορισμός του περιεχομένου των δεδομένων σας
- Εάν έχετε επιλέξει για να προωθήσετε δεδομένα κειμένου, αντιγράψτε και επικολλήστε το κείμενο στο πλαίσιο "περιεχόμενο".
- Εάν έχετε επιλέξει για να προωθήσετε δυαδικό ή base64 δεδομένων, χρησιμοποιήστε το κουμπί "Αποστολή του αρχείου" για να αποστείλετε το αρχείο σας.
- Μπορείτε να στοχεύετε ένα ακροατήριο μελλοντική εκστρατείας με βάση τα σχόλια reach του πώς οι χρήστες απαντήσει αυτής της εκστρατείας. Προσδιορισμού ακροατηρίου μπορεί να βασίζονται σε εάν αυτής της εκστρατείας έχει απλώς προωθηθεί, έχει απαντηθεί, actioned ή εξέλθουν.

### <a name="see-also"></a>Δείτε επίσης
- [Περιβάλλον εργασίας Χρήστη τεκμηρίωση - Reach - νέα Push κριτηρίου][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>Το περιεχόμενο των πλακιδίων (μόνο για Windows Phone)
![Reach Content4][33]

### <a name="define-the-content-of-your-tile"></a>Ορισμός του περιεχομένου από το πλακίδιο
Το πλακίδιο φορτίο είναι το κείμενο να εμφανίζεται στο πλακίδιο της εφαρμογής σε συσκευές Windows Phone.
Ένα πλακίδιο push είναι η υπηρεσία ειδοποιήσεων Push της Microsoft (MPNS) έκδοση του μιας εγγενούς push για Windows Phone. Ο τύπος push πλακίδιο είναι ο μόνος τύπος push που δεν διαθέτει μια απάντηση και επομένως δεν μπορεί να δημιουργηθεί το ακροατήριο μελλοντική καμπάνιες σχετικά με τα αποτελέσματα μιας εκστρατείας push πλακίδιο. 

### <a name="see-also"></a>Δείτε επίσης
- [API τεκμηρίωση - Reach API - εγγενούς Push][Link 4]

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
 
