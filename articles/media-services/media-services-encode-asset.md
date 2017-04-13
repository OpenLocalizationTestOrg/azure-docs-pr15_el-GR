<properties 
    pageTitle="Επισκόπηση και σύγκριση των Azure στην κωδικοποιητές πολυμέσων ζήτηση | Microsoft Azure" 
    description="Αυτό το θέμα παρέχει μια επισκόπηση και σύγκριση των Azure σε ζήτηση κωδικοποιητές πολυμέσων." 
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
    ms.date="09/19/2016" 
    ms.author="juliako"/>

#<a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Επισκόπηση και σύγκριση των Azure σε ζήτηση κωδικοποιητές πολυμέσων

##<a name="encoding-overview"></a>Επισκόπηση κωδικοποίησης

Azure Media Services παρέχει πολλές επιλογές για την κωδικοποίηση των πολυμέσων στο cloud.

Κατά την εκκίνηση με τις υπηρεσίες πολυμέσων, είναι σημαντικό να κατανοήσετε τη διαφορά μεταξύ των μορφών κωδικοποιητών και αρχείων.
Οι κωδικοποιητές είναι το λογισμικό που εφαρμόζει τους αλγορίθμους συμπίεση/αποσυμπίεση ότι οι μορφές αρχείου είναι κοντέινερ που περιέχει το συμπιεσμένο βίντεο.

Υπηρεσίες πολυμέσων παρέχει δυναμική συσκευασίας η οποία σας επιτρέπει να παραδώσετε το περιεχόμενό σας προσαρμόσιμες ρυθμό μετάδοσης bit MP4 ή ομαλή ροή κωδικοποιημένη σε ροή μορφές που υποστηρίζονται από τις υπηρεσίες πολυμέσων (MPEG ΠΑΎΛΑ, HLS, ομαλή ροή, σκληροί ΔΊΣΚΟΙ) χωρίς να χρειάζεται να συσκευάσετε ξανά σε αυτές τις μορφές ροής.

Για να επωφεληθείτε από [δυναμική συσκευασία](media-services-dynamic-packaging-overview.md), πρέπει να κάνετε τα εξής:

- Κωδικοποίηση του αρχείου θυγατρική (προέλευση) σε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 ή αρχείων ομαλή ροή προσαρμόσιμη ρυθμό μετάδοσης bit (τα βήματα κωδικοποίησης εκδηλώνονται παρακάτω σε αυτό το πρόγραμμα εκμάθησης).
- Λήψη τουλάχιστον μία On Demand ροής μονάδας για τη ροή τελικό σημείο από τον οποίο σκοπεύετε να παράδοσης το περιεχόμενό σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να On Demand ροής δεσμευμένη μονάδες κλίμακας](media-services-portal-manage-streaming-endpoints.md).

Υπηρεσίες πολυμέσων υποστηρίζει τα εξής στην κωδικοποιητές ζήτηση που περιγράφονται σε αυτό το άρθρο:

- [Τυπική κωδικοποιητή πολυμέσων](media-services-encode-asset.md#media-encoder-standard)
- [Ροή εργασίας Premium κωδικοποιητή πολυμέσων](media-services-encode-asset.md#media-encoder-premium-workflow)

Σε αυτό το άρθρο παρέχει μια σύντομη επισκόπηση των ζήτηση κωδικοποιητές πολυμέσων και παρέχει συνδέσεις προς άρθρα που παρέχουν πιο λεπτομερείς πληροφορίες. Το θέμα παρέχει επίσης σύγκριση των το κωδικοποιητές.

Σημειώστε ότι από προεπιλογή κάθε λογαριασμό Media Services μπορεί να έχει ένα ενεργό κωδικοποίησης εργασίας κάθε φορά. Μπορείτε να δεσμεύσετε κωδικοποίησης μονάδες που σας επιτρέπουν να έχετε πολλές κωδικοποίησης εργασίες ταυτόχρονα, εκτελεί ένα για κάθε κωδικοποίησης δεσμευμένη μονάδα αγορά. Για πληροφορίες, ανατρέξτε στο θέμα [Κλιμάκωση κωδικοποίηση μονάδες](media-services-scale-media-processing-overview.md).

##<a name="media-encoder-standard"></a>Τυπική κωδικοποιητή πολυμέσων

###<a name="how-to-use"></a>Πώς μπορείτε να χρησιμοποιήσετε

[Πώς μπορείτε να κωδικοποιήσετε με τυπική κωδικοποιητή πολυμέσων](media-services-dotnet-encode-with-media-encoder-standard.md)

###<a name="formats"></a>Μορφές

[Μορφές και τους κωδικοποιητές](media-services-media-encoder-standard-formats.md)

###<a name="presets"></a>Υποδείγματα

Media Encoder τυπική ρυθμίζεται χρησιμοποιώντας ένα από τα encoder υποδείγματα περιγράφεται [εδώ](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Μετα-δεδομένα εισόδου και εξόδου

Τα μετα-δεδομένα εισαγωγής κωδικοποιητές περιγράφεται [εδώ](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Τα μετα-δεδομένα εξόδου κωδικοποιητές περιγράφεται [εδώ](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###<a name="generate-thumbnails"></a>Δημιουργία μικρογραφίες

Για πληροφορίες, ανατρέξτε στο θέμα [Πώς να δημιουργείτε μικρογραφίες, χρησιμοποιώντας Media Encoder τυπική](media-services-custom-mes-presets-with-dotnet.md#thumbnails).

###<a name="trim-videos-clipping"></a>Αποκοπή βίντεο (απόσπασμα)

Για πληροφορίες, ανατρέξτε στο θέμα [τον τρόπο αποκοπής βίντεο με χρήση του Media Encoder τυπική](media-services-custom-mes-presets-with-dotnet.md#trim_video).

###<a name="create-overlays"></a>Δημιουργία επικαλύψεων

Για πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε επικαλύψεων χρησιμοποιώντας Media Encoder τυπική](media-services-custom-mes-presets-with-dotnet.md#overlay).

###<a name="see-also"></a>Δείτε επίσης

[Το ιστολόγιο του Media Services](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##<a name="media-encoder-premium-workflow"></a>Ροή εργασίας Premium κωδικοποιητή πολυμέσων

###<a name="overview"></a>Επισκόπηση

[Εισαγωγή Premium κωδικοποίηση στο Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###<a name="how-to-use"></a>Πώς μπορείτε να χρησιμοποιήσετε

Ροή εργασίας Premium κωδικοποιητή πολυμέσων έχει ρυθμιστεί ώστε να χρήση σύνθετων ροών εργασίας. Αρχεία της ροής εργασίας ήταν δυνατή η δημιουργία και ενημέρωση χρησιμοποιώντας το εργαλείο [Σχεδίασης ροής εργασίας](media-services-workflow-designer.md) .

[Πώς μπορείτε να χρησιμοποιήσετε Premium κωδικοποίηση στο Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###<a name="known-issues"></a>Γνωστά θέματα

Εάν το βίντεο εισαγωγής δεν περιέχει κλειστές λεζάντες, το αποτέλεσμα του περιουσιακού στοιχείου θα εξακολουθεί να περιέχει ένα κενό αρχείο TTML. 


##<a id="compare_encoders"></a>Σύγκριση κωδικοποιητές

###<a id="billing"></a>Μετρητής χρεώσεων που χρησιμοποιούνται από κάθε encoder

Όνομα επεξεργαστής πολυμέσων|Τις τιμές που εφαρμόζονται|Σημειώσεις
---|---|---
**Τυπική κωδικοποιητή πολυμέσων** |ENCODER|Κωδικοποίηση εργασίες θα χρεωθεί σύμφωνα με το μέγεθος του αποτελέσματος περιουσιακών στοιχείων, στο GBytes, με το επιτόκιο που καθορίζεται [εδώ][1], κάτω από τη στήλη ΚΩΔΙΚΟΠΟΙΗΤΉ.
**Ροή εργασίας Premium κωδικοποιητή πολυμέσων** |Ο ΚΩΔΙΚΟΠΟΙΗΤΉΣ PREMIUM|Κωδικοποίηση εργασίες θα χρεωθεί σύμφωνα με το μέγεθος του αποτελέσματος περιουσιακών στοιχείων, στο GBytes, με το επιτόκιο που καθορίζεται [εδώ][1], κάτω από τη στήλη PREMIUM ENCODER.


Αυτή η ενότητα συγκρίνει τις δυνατότητες κωδικοποίησης του **Media Encoder τυπική** και τη **Ροή εργασίας Premium κωδικοποιητή πολυμέσων**.


###<a name="input-containerfile-formats"></a>Εισαγάγετε μορφές αρχείου/κοντέινερ

Εισαγάγετε μορφές αρχείου/κοντέινερ|Τυπική κωδικοποιητή πολυμέσων|Ροή εργασίας Premium κωδικοποιητή πολυμέσων
---|---|---
Adobe® Flash® F4V           |Ναι|Ναι
MXF/SMPTE 377M              |Ναι|Ναι
GXF                         |Ναι|Ναι
Ροή μεταφοράς MPEG-2    |Ναι|Ναι
Οι ροές προγράμματος MPEG-2      |Ναι|Ναι
MPEG-4/MP4                  |Ναι|Ναι
Windows Media/ASF           |Ναι|Ναι
AVI (αρχεία μη συμπιεσμένων 8 bit/10 bit)|Ναι|Ναι
3GPP/3GPP2                  |Ναι|Όχι
Ομαλή ροή μορφή αρχείου (PIFF 1.3)|Ναι|Όχι
[Ψηφιακές Microsoft Recording(DVR-MS) βίντεο](https://msdn.microsoft.com/library/windows/desktop/dd692984)|Ναι|Όχι
Matroska/WebM               |Ναι|Όχι
QuickTime (.mov) |Ναι|Όχι

###<a name="input-video-codecs"></a>Κωδικοποιητές βίντεο εισαγωγής

Κωδικοποιητές βίντεο εισαγωγής|Τυπική κωδικοποιητή πολυμέσων|Ροή εργασίας Premium κωδικοποιητή πολυμέσων
---|---|---
AVC 8 bit/10-bit, έως και 4:2:2, συμπεριλαμβανομένων των AVCIntra   |8 bit 4:2:0 και 4:2:2|Ναι
Φανατικός DNxHD (στο MXF)                                 |Ναι|Ναι
DVCPro/DVCProHD (στο MXF)                            |Ναι|Ναι
JPEG2000                                            |Ναι|Ναι
MPEG-2 (έως 422 προφίλ και υψηλού επιπέδου, συμπεριλαμβανομένων των παραλλαγές όπως XDCAM, XDCAM HD, XDCAM IMX, CableLabs® και D10)|Έως 422 προφίλ|Ναι
MPEG-1                                              |Ναι|Ναι
Windows Media Video/VC-1                            |Ναι|Ναι
Canopus τμήμα HQ/HQX                                      |Όχι|Όχι
MPEG-4 μέρος 2                                       |Ναι|Όχι
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ναι|Όχι
Apple ProRes 422    |Ναι|Όχι
Apple ProRes 422 LT |Ναι|Όχι
Τμήμα HQ ProRes 422 Apple |Ναι|Όχι
Διακομιστής μεσολάβησης ProRes Apple|Ναι|Όχι
Apple ProRes 4444 |Ναι|Όχι
Apple ProRes 4444 XQ |Ναι|Όχι

###<a name="input-audio-codecs"></a>Εισαγάγετε κωδικοποιητές ήχου

Εισαγάγετε κωδικοποιητές ήχου|Τυπική κωδικοποιητή πολυμέσων|Ροή εργασίας Premium κωδικοποιητή πολυμέσων
---|---|---
AES (SMPTE 331 M και 302 M, AES3-2003)        |Όχι|Ναι
Dolby® E                                    |Όχι|Ναι
Ψηφιακή Dolby® (AC3)                        |Όχι|Ναι
Ψηφιακή Dolby® Plus (E-AC3)                 |Όχι|Ναι
AAC (AAC-LC, AAC-HE και AAC-HEv2 έως 5.1)|Ναι|Ναι
MPEG επιπέδου 2|Ναι|Ναι
MP3 (ήχου επίπεδο MPEG-1 3)|Ναι|Ναι
Windows Media Audio|Ναι|Ναι
WAV/PCM|Ναι|Ναι
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ναι|Όχι
[Opus](https://en.wikipedia.org/wiki/Opus_(audio_format)) |Ναι|Όχι
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ναι|Όχι


###<a name="output-containerfile-formats"></a>Μορφές αρχείου/κοντέινερ εξόδου

Μορφές αρχείου/κοντέινερ εξόδου|Τυπική κωδικοποιητή πολυμέσων|Ροή εργασίας Premium κωδικοποιητή πολυμέσων
---|---|---
Adobe® Flash® F4V|Όχι|Ναι
MXF (OP1a, XDCAM και AS02)|Όχι|Ναι
DPP (συμπεριλαμβανομένων των AS11)|Όχι|Ναι
GXF|Όχι|Ναι
MPEG-4/MP4|Ναι|Ναι
MPEG-TS|Ναι|Ναι
Windows Media/ASF|Όχι|Ναι
AVI (αρχεία μη συμπιεσμένων 8 bit/10 bit)|Όχι|Ναι
Ομαλή ροή μορφή αρχείου (PIFF 1.3)|Όχι|Ναι

###<a name="output-video-codecs"></a>Κωδικοποιητές βίντεο εξόδου

Κωδικοποιητές βίντεο εξόδου|Τυπική κωδικοποιητή πολυμέσων|Ροή εργασίας Premium κωδικοποιητή πολυμέσων
---|---|---
AVC (H.264; 8 bit, έως υψηλής προφίλ, επίπεδο 5.2; 4 K εξαιρετικά HD; Εντός AVC)|Μόνο 8 bit 4:2:0|Ναι
Φανατικός DNxHD (στο MXF)|Όχι|Ναι
MPEG-2 (έως 422 προφίλ και υψηλού επιπέδου, συμπεριλαμβανομένων των παραλλαγές όπως XDCAM, XDCAM HD, XDCAM IMX, CableLabs® και D10)|Όχι|Ναι
MPEG-1|Όχι|Ναι
Windows Media Video/VC-1|Όχι|Ναι
Δημιουργία μικρογραφιών JPEG|Όχι|Ναι

###<a name="output-audio-codecs"></a>Κωδικοποιητές ήχου εξόδου

Κωδικοποιητές ήχου εξόδου|Τυπική κωδικοποιητή πολυμέσων|Ροή εργασίας Premium κωδικοποιητή πολυμέσων
---|---|---
AES (SMPTE 331 M και 302 M, AES3-2003)|Όχι|Ναι
Ψηφιακή Dolby® (AC3)|Όχι|Ναι
Dolby® ψηφιακή συν (E-AC3) έως και 7.1|Όχι|Ναι
AAC (AAC-LC, AAC-HE και AAC-HEv2 έως 5.1)|Ναι|Ναι
MPEG επιπέδου 2|Όχι|Ναι
MP3 (ήχου επίπεδο MPEG-1 3)|Όχι|Ναι
Windows Media Audio|Όχι|Ναι


##<a name="error-codes"></a>Κωδικοί σφάλματος  

Ο παρακάτω πίνακας παραθέτει τους κωδικούς σφάλματος που μπορεί να επιστραφεί σε περίπτωση που Παρουσιάστηκε σφάλμα κατά την εκτέλεση εργασιών κωδικοποίησης.  Για να λάβετε λεπτομέρειες του σφάλματος στον κώδικα .NET, χρησιμοποιήστε την κλάση [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) . Για να λάβετε λεπτομέρειες του σφάλματος στον κώδικα ΥΠΌΛΟΙΠΟ, χρησιμοποιήστε το REST API [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) .

ErrorDetail.Code|Πιθανές αιτίες για σφάλματος
-----|-----------------------
Άγνωστη κατάσταση| Άγνωστο σφάλμα κατά την εκτέλεση της εργασίας
ErrorDownloadingInputAssetMalformedContent|Κατηγορία σφαλμάτων που καλύπτει σφάλματα κατά τη λήψη εισαγωγής περιουσιακών στοιχείων όπως ονόματα αρχείων εσφαλμένες, μηδέν αρχεία μήκους, εσφαλμένη μορφές και ούτω καθεξής.
ErrorDownloadingInputAssetServiceFailure|Κατηγορία σφαλμάτων που καλύπτει προβλήματα στην πλευρά της υπηρεσίας - για παράδειγμα δικτύου ή χώρο αποθήκευσης σφάλματα κατά τη λήψη.
ErrorParsingConfiguration|Κατηγορία σφαλμάτων εργασιών όπου <see cref="MediaTask.PrivateData"/> (ρύθμιση παραμέτρων) δεν είναι έγκυρο, για παράδειγμα τη ρύθμιση παραμέτρων δεν είναι ένα έγκυρο σύστημα προκαθορισμένες ή περιέχει μη έγκυρους XML.
ErrorExecutingTaskMalformedContent|Κατηγορία σφάλματα κατά την εκτέλεση της εργασίας όπου θέματα μέσα τα αρχεία πολυμέσων εισαγωγής προκαλούν την αποτυχία.
ErrorExecutingTaskUnsupportedFormat|Κατηγορία σφαλμάτων όπου ο επεξεργαστής πολυμέσων δεν μπορεί να επεξεργαστεί τα αρχεία που παρέχονται - μορφή πολυμέσων, δεν υποστηρίζεται ή δεν ταιριάζουν με τη ρύθμιση παραμέτρων. Για παράδειγμα, προσπαθείτε να παραγάγετε μόνο ήχου το αποτέλεσμα από ενός περιουσιακού στοιχείου που περιέχει μόνο βίντεο
ErrorProcessingTask|Κατηγορία άλλα σφάλματα που συναντά ο επεξεργαστής πολυμέσων κατά την επεξεργασία της εργασίας που σχετίζεται με το περιεχόμενο.
ErrorUploadingOutputAsset|Κατηγορία σφάλματα κατά την αποστολή του παγίου εξόδου
ErrorCancelingTask|Κατηγορία σφαλμάτων για να καλύψετε αποτυχίες κατά την προσπάθεια για να ακυρώσετε την εργασία
TransientError|Κατηγορία σφαλμάτων για να καλύψετε μεταβατικές θέματα (π.χ. προσωρινό κοινωνικής δικτύωσης θέματα με το χώρο αποθήκευσης Azure)


Για να λάβετε βοήθεια από την ομάδα **Media Services** , ανοίξτε μια [δελτίων υποστήριξης](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-articles"></a>Σχετικά άρθρα

- [Εκτέλεση σύνθετων κωδικοποίησης εργασιών, προσαρμόζοντας Media Encoder τυπική υποδείγματα](media-services-custom-mes-presets-with-dotnet.md)
- [Τα όρια και περιορισμοί](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
