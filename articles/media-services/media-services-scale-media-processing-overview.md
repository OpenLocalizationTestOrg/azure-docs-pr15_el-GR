<properties
    pageTitle="Κλιμάκωση επεξεργασίας πολυμέσων Επισκόπηση | Microsoft Azure"
    description="Αυτό το θέμα είναι μια επισκόπηση της κλίμακας επεξεργασίας πολυμέσων με υπηρεσιών Azure Media Services."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="juliako"/>


# <a name="scaling-media-processing-overview"></a>Επισκόπηση κλίμακας επεξεργασίας πολυμέσων

Αυτή η σελίδα σάς παρέχει μια επισκόπηση του τρόπου και γιατί για να κλιμακωθεί επεξεργασία media. 

## <a name="overview"></a>Επισκόπηση

Ένα λογαριασμό Media Services είναι συσχετισμένη με μια δεσμευμένη τύπο μονάδας, ο οποίος καθορίζει την ταχύτητα με την οποία πραγματοποιείται τα πολυμέσα επεξεργασία εργασιών. Μπορείτε να επιλέξετε μεταξύ των παρακάτω τύπων δεσμευμένη μονάδας: **S1**, **S2**ή **S3**. Για παράδειγμα, το ίδιο κωδικοποίησης έργο εκτελείται πιο γρήγορα όταν χρησιμοποιείτε τη σύγκριση **S2** δεσμευμένες μονάδας τύπου με τον τύπο **S1** . Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα οι [Δεσμευμένες τύποι μονάδα](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Εκτός από την που καθορίζει τον τύπο δεσμευμένη μονάδας, μπορείτε να καθορίσετε την προμήθεια το λογαριασμό σας με δεσμευμένες μονάδες. Ο αριθμός των μονάδων προμήθεια του φακέλου δεσμευμένη καθορίζει τον αριθμό των εργασιών πολυμέσων που είναι δυνατή η επεξεργασία ταυτόχρονα σε ένα συγκεκριμένο λογαριασμό. Για παράδειγμα, εάν ο λογαριασμός σας έχει πέντε δεσμευμένη μονάδες, στη συνέχεια, εργασίες πέντε πολυμέσων θα εκτελεί ταυτόχρονα πολύς χρόνος ως υπάρχουν εργασίες για επεξεργασία. Οι υπόλοιπες εργασίες θα περιμένετε στην ουρά και θα λάβετε επιλέξατε για την επεξεργασία διαδοχικά όταν ολοκληρωθεί μια εργασία που εκτελείται. Εάν δεν έχει ένα λογαριασμό οποιαδήποτε δεσμευμένη μονάδες παροχή της υπηρεσίας, στη συνέχεια, εργασίες θα να εντοπιστεί διαδοχικά. Σε αυτήν την περίπτωση, το χρόνο αναμονής μεταξύ ολοκλήρωσης μιας εργασίας και στη συνέχεια μία έναρξης θα εξαρτώνται από τη διαθεσιμότητα των πόρων του συστήματος.

## <a name="choosing-between-different-reserved-unit-types"></a>Επιλογή μεταξύ διαφορετική μονάδα δεσμευμένη τύπων

Ο παρακάτω πίνακας σάς βοηθά να κάνετε λήψη αποφάσεων κατά την επιλογή μεταξύ διαφορετικές ταχύτητες κωδικοποίησης. Επίσης, παρέχει ορισμένα συγκριτικών υποθέσεις και παρέχει διευθύνσεις URL συσχετισμών Ασφαλείας που μπορείτε να χρησιμοποιήσετε για να κάνετε λήψη βίντεο στην οποία μπορείτε να εκτελέσετε τη δική σας δοκιμές:

Σενάρια|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Προβλεπόμενη χρήση πεζών-κεφαλαίων| Μονό κωδικοποίηση ρυθμό μετάδοσης bit. <br/>Αρχεία σε SD ή κάτω από το στοιχείο αναλύσεις, χρόνου δεν διάκριση πεζών-κεφαλαίων, χαμηλό κόστος.|Μία ρυθμό μετάδοσης bit και πολλές κωδικοποίηση ρυθμό μετάδοσης bit.<br/>Κανονική χρήση για την κωδικοποίηση SD και HD. |Μία ρυθμό μετάδοσης bit και πολλές κωδικοποίηση ρυθμό μετάδοσης bit.<br/>Πλήρης HD και 4K ανάλυση βίντεο. Χρονική κωδικοποίηση επιστροφής διάκριση πεζών-κεφαλαίων, πιο γρήγορα. 
Συγκριτικών|[Αρχείο εισαγωγής: 5 λεπτά μεγάλη 640x360p στο 29,97 πλαισίων/δευτερόλεπτο](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Κωδικοποίηση σε ένα μεμονωμένο ρυθμό μετάδοσης bit MP4 αρχείου, με την ίδια ανάλυση, διαρκεί περίπου 11 λεπτά.|[Το αρχείο εισαγωγής: 5 λεπτά μεγάλη 1280x720p στο 29,97 πλαισίων/δευτερόλεπτο](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Κωδικοποίηση με "H264 μία ρυθμό μετάδοσης bit 720p" προκαθορισμένες διαρκεί περίπου 5 λεπτά.<br/><br/>Κωδικοποίηση με "H264 πολλών ρυθμό μετάδοσης bit 720p" προκαθορισμένα διαρκεί περίπου 11.5 λεπτά.|[Αρχείο εισαγωγής: 5 λεπτά μεγάλη 1920x1080p στο 29,97 πλαισίων/δευτερόλεπτο](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>Κωδικοποίηση με "H264 μία ρυθμό μετάδοσης bit 1080p" προκαθορισμένες διαρκεί περίπου 2.7 λεπτά.<br/><br/>Κωδικοποίηση με "H264 πολλών ρυθμό μετάδοσης bit 1080p" προκαθορισμένα διαρκεί περίπου 5.7 λεπτά.

##<a name="considerations"></a>Ζητήματα

>[AZURE.IMPORTANT] Αναθεώρηση ζητήματα που περιγράφονται σε αυτήν την ενότητα.  

- Δεσμευμένη μονάδες εργάζονται parallelizing όλα επεξεργασίας πολυμέσων, συμπεριλαμβανομένης της δημιουργίας ευρετηρίου για εργασίες χρησιμοποιώντας Azure Media ευρετηρίου.  Ωστόσο, σε αντίθεση με κωδικοποίηση, εργασιών ευρετηρίου δεν λάβετε επεξεργασία πιο γρήγορα με πιο γρήγορα δεσμευμένη μονάδες.

- Εάν χρησιμοποιείτε το κοινόχρηστο χώρο συγκέντρωσης, αυτό σημαίνει ότι χωρίς οποιαδήποτε δεσμευμένη μονάδες, στη συνέχεια, σας κωδικοποίηση εργασίες έχουν τις ίδιες επιδόσεις όπως με S1 RUs. Ωστόσο, δεν υπάρχει καμία ανώτατο όριο το χρόνο που μπορεί να ξοδεύετε τις εργασίες σας σε κατάσταση στην ουρά και οποιαδήποτε στιγμή, μία εργασία είναι να εκτελείται.

- Το παρακάτω κέντρα δεδομένων δεν προσφέρουν τον τύπο μονάδας **S2** δεσμευμένες: Νότια Βραζιλίας, Δυτική Ινδίας, Ινδίας κεντρική και Νότια Ινδίας.

- Το παρακάτω κέντρα δεδομένων δεν προσφέρουν τον τύπο μονάδας **S3** δεσμευμένες: Νότια Βραζιλίας, Δυτική Ινδίας, Ινδίας Central.

- Στο μεγαλύτερο αριθμό μονάδων που έχουν καθοριστεί για την περίοδο 24 ωρών χρησιμοποιείται στον υπολογισμό του κόστους.


##<a name="quotas-and-limitations"></a>Τα όρια και περιορισμοί

Για πληροφορίες σχετικά με τα όρια και περιορισμοί και πώς μπορείτε να ανοίξετε ένα δελτίο υποστήριξης, ανατρέξτε στο θέμα [όρια και περιορισμοί](media-services-quotas-and-limitations.md).

##<a name="next-step"></a>Επόμενο βήμα

Επίτευξη της κλίμακας εργασίας επεξεργασίας πολυμέσων με μία από αυτές τις τεχνολογίες: 

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Πύλη](media-services-portal-scale-media-processing.md)
- [ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
