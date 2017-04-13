<properties 
    pageTitle="Μορφές ροής εργασίας Premium κωδικοποιητή πολυμέσων και τους κωδικοποιητές | Microsoft Azure" 
    description="Αυτό το θέμα παρέχει μια επισκόπηση των κωδικοποιητών και μορφές μορφές ροής εργασίας Premium κωδικοποιητή πολυμέσων" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erik43" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-premium-workflow-formats-and-codecs"></a>Μορφές ροής εργασίας Premium κωδικοποιητή πολυμέσων και τους κωδικοποιητές


>[AZURE.NOTE]Για ερωτήσεις encoder premium, ηλεκτρονικού ταχυδρομείου mepd στο Microsoft.com.
>
>Επεξεργαστής πολυμέσων ροής εργασίας Premium κωδικοποιητή πολυμέσων που περιγράφονται σε αυτό το θέμα δεν είναι διαθέσιμη στην Κίνα. 

Αυτό το έγγραφο περιέχει μια λίστα των εισόδου και εξόδου μορφές αρχείων και τους κωδικοποιητές που υποστηρίζονται από την έκδοση preview δημόσια του κωδικοποιητή **Ροής εργασίας Premium κωδικοποιητή πολυμέσων** .

[Μορφές εισαγωγής Worflow Premium κωδικοποιητή πολυμέσων και τους κωδικοποιητές](#input_formats)

[Μορφές εξόδου Worflow Premium κωδικοποιητή πολυμέσων και τους κωδικοποιητές](#output_formats)

**Ροή εργασίας Premium κωδικοποιητή πολυμέσων** υποστηρίζει κλειστές λεζάντες που περιγράφονται σε [αυτήν](#closed_captioning) την ενότητα. 


##<a id="input_formats"></a>Εισαγωγή ροής εργασίας Premium κωδικοποιητή πολυμέσων μορφές και τους κωδικοποιητές

Στην παρακάτω ενότητα παραθέτει τις μορφές κωδικοποιητών και αρχείων που υποστηρίζει τον επεξεργαστή πολυμέσων ως είσοδο.

###<a name="input-containerfile-formats"></a>Εισαγάγετε μορφές αρχείου/κοντέινερ

- Adobe® Flash® F4V
- MXF/SMPTE 377M
- GXF
- Ροή μεταφοράς MPEG-2
- Οι ροές προγράμματος MPEG-2
- MPEG-4/MP4
- Windows Media/ASF
- AVI (αρχεία μη συμπιεσμένων 8 bit/10 bit)

###<a name="input-video-codecs"></a>Κωδικοποιητές βίντεο εισαγωγής

- AVC 8 bit/10-bit, έως και 4:2:2, συμπεριλαμβανομένων των AVCIntra
- Φανατικός DNxHD (στο MXF)
- DVCPro/DVCProHD (στο MXF)
- JPEG2000
- MPEG-2 (έως 422 προφίλ και υψηλού επιπέδου, συμπεριλαμβανομένων των παραλλαγές όπως XDCAM, XDCAM HD, XDCAM IMX, CableLabs® και D10)
- MPEG-1
- Windows Media Video/VC-1

###<a name="input-audio-codecs"></a>Εισαγάγετε κωδικοποιητές ήχου

- AES (SMPTE 331 M και 302 M, AES3-2003)
- Dolby® E
- Ψηφιακή Dolby® (AC3)
- AAC (AAC-LC, AAC-HE και AAC-HEv2 έως 5.1)
- MPEG επιπέδου 2
- MP3 (ήχου επίπεδο MPEG-1 3)
- Windows Media Audio
- WAV/PCM
 
##<a id="output_format"></a>Μορφές εξόδου της ροής εργασίας Media Encoder Premium και τους κωδικοποιητές

Ενότητα που ακολουθεί παραθέτει τις μορφές κωδικοποιητών και αρχείων που υποστηρίζονται ως έξοδος από αυτόν τον επεξεργαστή πολυμέσων.

###<a name="output-containerfile-formats"></a>Μορφές αρχείου/κοντέινερ εξόδου

- Adobe® Flash® F4V
- MXF (OP1a, XDCAM και AS02)
- DPP (συμπεριλαμβανομένων των AS11)
- GXF
- MPEG-4/MP4
- Windows Media/ASF
- AVI (αρχεία μη συμπιεσμένων 8 bit/10 bit)
- Ομαλή ροή μορφή αρχείου (PIFF 1.3)
- MPEG-TS 


###<a name="output-video-codecs"></a>Κωδικοποιητές βίντεο εξόδου

- AVC (H.264; 8 bit, έως υψηλής προφίλ, επίπεδο 5.2; 4 K εξαιρετικά HD; Εντός AVC)
- Φανατικός DNxHD (στο MXF)
- DVCPro/DVCProHD (στο MXF)
- MPEG-2 (έως 422 προφίλ και υψηλού επιπέδου, συμπεριλαμβανομένων των παραλλαγές όπως XDCAM, XDCAM HD, XDCAM IMX, CableLabs® και D10)
- MPEG-1
- Windows Media Video/VC-1
- Δημιουργία μικρογραφιών JPEG

###<a name="output-audio-codecs"></a>Κωδικοποιητές ήχου εξόδου

- AES (SMPTE 331 M και 302 M, AES3-2003)
- Ψηφιακή Dolby® (AC3)
- Dolby® ψηφιακή συν (E-AC3) έως και 7.1
- AAC (AAC-LC, AAC-HE και AAC-HEv2 έως 5.1)
- MPEG επιπέδου 2
- MP3 (ήχου επίπεδο MPEG-1 3)
- Windows Media Audio

##<a id="closed_captioning"></a>Υποστήριξη για κλειστές λεζάντες

Στο ingest, **Η ροή εργασίας Premium κωδικοποιητή πολυμέσων** υποστηρίζει:

1. Τα αρχεία της SCC
1. Αρχεία SMPTE TT
1. CEA-608/CEA-708 – ως δεδομένα χρήστη (τιμές SEI μηνύματα H.264 δημοτικού ροών, ATSC/53, SCTE20) ή ως βοηθητικές δεδομένα σε αρχεία MXF/GXF
1. Αρχεία υπότιτλο STL

Κατά την έξοδο, τις παρακάτω επιλογές είναι διαθέσιμες:

1. CEA-608 να CEA 708 μετάφρασης
1. CEA-608/CEA-708 διέρχονται από (ενσωματωμένο σε μηνύματα τιμές SEI H.264 δημοτικού ροών ή μεταφέρονται ως βοηθητικές δεδομένα σε αρχεία MXF)
1. ΤΗΣ SCC
1. Κείμενο χρονομέτρηση SMPTE (από προέλευση CEA 608 ανά SMPTE RP2052, συμπεριλαμβανομένης της δημιουργίας αρχείου DFXP)
1. Αρχείο SRT υπότιτλο
1. Ροές υπότιτλο DVB

Σημείωση: όχι για όλες τις παραπάνω μορφές εξόδου που υποστηρίζονται για την παράδοση μέσω ροής στο υπηρεσιών Azure Media Services.

##<a name="known-issues"></a>Γνωστά θέματα

Εάν το βίντεο εισαγωγής δεν περιέχει κλειστές λεζάντες, το αποτέλεσμα του περιουσιακού στοιχείου θα εξακολουθεί να περιέχει ένα κενό αρχείο TTML. 


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
