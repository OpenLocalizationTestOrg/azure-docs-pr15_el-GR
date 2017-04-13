<properties 
    pageTitle="Media Encoder τυπικές μορφές και τους κωδικοποιητές" 
    description="Αυτό το θέμα παρέχει μια επισκόπηση των κωδικοποιητών και Media Encoder τυπικές μορφές." 
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
    ms.date="10/10/2016"
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-standard-formats-and-codecs"></a>Τυπικές μορφές κωδικοποιητή πολυμέσων και τους κωδικοποιητές


Αυτό το έγγραφο περιέχει μια λίστα με τις πιο συνηθισμένες εισαγωγής και μορφές αρχείου εξαγωγής που μπορείτε να χρησιμοποιήσετε με Media Encoder Standard.


##<a name="input-containerfile-formats"></a>Εισαγάγετε μορφές αρχείου/κοντέινερ

Μορφές αρχείων (επεκτάσεις αρχείου)|Υποστηρίζονται
---|---|---|---
FLV (με H.264 και AAC κωδικοποιητές) (.flv)          |Ναι 
MXF (.mxf)                  |Ναι 
GXF (.gxf)                  |Ναι 
MPEG2-PS, MPEG2-TS, 3GP (.ts, .ps, .3gp, .3gpp, .mpg)   |Ναι 
Βίντεο Windows Media (WMV) / ASF (.wmv, .asf) |Ναι 
AVI (αρχεία μη συμπιεσμένων 8 bit/10 bit) (.avi)|Ναι 
MP4 (.mp4, .m4a, .m4v) / ISMV (.isma, .ismv)|Ναι 
[Ψηφιακές Microsoft Recording(DVR-MS) βίντεο](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Ναι 
Matroska/WebM (.mkv)        |Ναι 
ΚΎΜΑΤΟΣ/WAV (.wav) |Ναι 
QuickTime (.mov) |Ναι

>[AZURE.NOTE] Είναι πάνω από μια λίστα με τις πιο συχνά αντιμετώπισε επεκτάσεις. Media Encoder τυπική υποστηρίζει πολλά άλλα (για παράδειγμα: .m2ts, .mpeg2video, .qt). Εάν προσπαθείτε να κωδικοποιήσετε ένα αρχείο και να λάβετε ένα μήνυμα σφάλματος σχετικά με τη μορφή που δεν υποστηρίζονται, δώστε σχολίων [εδώ](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).

###<a name="audio-formats-in-input-containers"></a>Μορφές ήχου στο κοντέινερ εισαγωγής 

Media Encoder τυπική υποστηρίζει που μεταφέρουν τις ακόλουθες μορφές ήχου στο κοντέινερ εισαγωγής:

- MXF, GXF και QuickTime αρχεία που έχουν κομμάτια ήχου με παρεμβαλλόμενη στερεοφωνικού ή 5.1 δειγμάτων

ή

- Αρχεία MXF, GXF και QuickTime όπου ο ήχος πραγματοποιείται ως ξεχωριστή κομμάτια PCM αλλά την αντιστοίχιση καναλιού (σε στερεοφωνικού ή 5.1) μπορούν να συναχθούν από τα μετα-δεδομένα του αρχείου

Σημειώστε αυτήν την υποστήριξη για αντιστοίχισης καναλιών ρητή/που παρέχει ο χρήστης θα παρέχονται στο άμεσο μέλλον.


##<a name="input-video-codecs"></a>Κωδικοποιητές βίντεο εισαγωγής

Κωδικοποιητές βίντεο εισαγωγής|Υποστηρίζονται
---|---|---|---
AVC 8 bit/10-bit, έως και 4:2:2, συμπεριλαμβανομένων των AVCIntra   |8 bit 4:2:0 και 4:2:2 
Φανατικός DNxHD (στο MXF)                                 |Ναι 
DVCPro/DVCProHD (στο MXF)                            |Ναι 
Ψηφιακό βίντεο (DV) (σε αρχεία AVI)                   |Ναι
JPEG 2000                                           |Ναι 
MPEG-2 (έως 422 προφίλ και υψηλού επιπέδου, συμπεριλαμβανομένων των παραλλαγές όπως XDCAM, XDCAM HD, XDCAM IMX, CableLabs® και D10)|Έως 422 προφίλ 
MPEG-1                                              |Ναι 
VC-1/WMV9                                           |Ναι 
Canopus τμήμα HQ/HQX                                      |Όχι 
MPEG-4 μέρος 2                                       |Ναι 
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ναι 
YUV420 χωρίς συμπίεση ή θυγατρική                   |Ναι
Apple ProRes 422                                    |Ναι
Apple ProRes 422 LT |Ναι
Τμήμα HQ ProRes 422 Apple |Ναι
Διακομιστής μεσολάβησης ProRes Apple|Ναι
Apple ProRes 4444 |Ναι
Apple ProRes 4444 XQ |Ναι



##<a name="input-audio-codecs"></a>Εισαγάγετε κωδικοποιητές ήχου

Εισαγάγετε κωδικοποιητές ήχου|Υποστηρίζονται
---|---|---|---
AAC (AAC-LC, AAC-HE και AAC-HEv2 έως 5.1)|Ναι 
MPEG επιπέδου 2|Ναι 
MP3 (ήχου επίπεδο MPEG-1 3)|Ναι 
Windows Media Audio|Ναι 
WAV/PCM|Ναι 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ναι 
[Opus](http://go.microsoft.com/fwlink/?LinkId=822667) |Ναι 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ναι 
AMR (επιτόκιο προσαρμόσιμη πολλών)|Ναι
AES (SMPTE 331 M και 302 M, AES3-2003)        |Όχι 
Dolby® E                                    |Όχι 
Ψηφιακή Dolby® (AC3)                        |Όχι 
Ψηφιακή Dolby® Plus (E-AC3)                 |Όχι 


##<a name="output-formats-and-codecs"></a>Μορφές εξόδου και τους κωδικοποιητές

Ο παρακάτω πίνακας παραθέτει τις μορφές κωδικοποιητών και αρχείων που υποστηρίζονται για εξαγωγή.


Μορφή αρχείου|Κωδικοποιητής βίντεο|Κωδικοποιητής ήχου
---|---|---
MP4 <br/><br/>(συμπεριλαμβανομένων των κοντέινερ MP4 πολλούς-ρυθμό μετάδοσης bit) |H.264 (υψηλή, κύριο και προφίλ γραμμής βάσης)|AAC-LC, HE-AAC v1, HE-AAC v2 
MPEG2 TS |H.264 (υψηλή, κύριο και προφίλ γραμμής βάσης)|AAC-LC, HE-AAC v1, HE-AAC v2 



##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Δείτε επίσης

[Κωδικοποίηση On Demand περιεχομένου με τις υπηρεσίες πολυμέσων Azure](media-services-encode-asset.md)

[Πώς μπορείτε να κωδικοποιήσετε με τυπική κωδικοποιητή πολυμέσων](media-services-dotnet-encode-with-media-encoder-standard.md)
