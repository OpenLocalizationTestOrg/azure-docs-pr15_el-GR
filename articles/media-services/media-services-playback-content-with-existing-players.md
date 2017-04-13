<properties 
    pageTitle="Αναπαραγωγή περιεχομένου σας | Microsoft Azure" 
    description="Αυτό το θέμα παραθέτει υπάρχοντα αναπαραγωγής που μπορείτε να χρησιμοποιήσετε για αναπαραγωγή του περιεχομένου." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="juliako"/>


#<a name="playing-your-content-with-existing-players"></a>Αναπαραγωγή περιεχομένου σας με τις υπάρχουσες συσκευές αναπαραγωγής

Azure Media Services υποστηρίζει πολλές δημοφιλείς ροής μορφές, όπως ομαλή ροή HTTP Live ροής και MPEG-παύλας. Αυτό το θέμα οδηγεί σε υπάρχουσα αναπαραγωγής που μπορείτε να χρησιμοποιήσετε για να ελέγξετε τη ροή.

>[AZURE.NOTE]Για αναπαραγωγή δυναμικά συσκευαστούν ή κρυπτογραφημένο δυναμικό περιεχόμενο, βεβαιωθείτε ότι για να λάβετε τουλάχιστον μία ροή μονάδας για τη ροή τελικό σημείο από το οποίο σκοπεύετε να διαθέσετε το περιεχόμενό σας. Για πληροφορίες σχετικά με τη ροή μονάδες κλίμακας, ανατρέξτε στο θέμα: [Τρόπος για να κλιμακωθεί ροής μονάδες](media-services-portal-manage-streaming-endpoints.md).

### <a name="the-azure-portal-media-services-content-player"></a>Το Azure πύλης Media Services περιεχομένου player

Η πύλη **Azure** παρέχει ένα πρόγραμμα αναπαραγωγής περιεχομένου που μπορείτε να χρησιμοποιήσετε για να ελέγξετε το βίντεό σας.

Κάντε κλικ στο επιθυμητό βίντεο (βεβαιωθείτε ότι ήταν [δημοσιευτεί](media-services-portal-publish.md)) και κάντε κλικ στο κουμπί " **αναπαραγωγή** " στο κάτω μέρος της πύλης.

Εφαρμογή ορισμένα ζητήματα:

- Το **Πρόγραμμα ΑΝΑΠΑΡΑΓΩΓΉΣ ΠΟΛΥΜΈΣΩΝ ΥΠΗΡΕΣΊΕΣ ΠΕΡΙΕΧΟΜΈΝΟΥ** ακούγεται από την προεπιλεγμένη ροή τελικού σημείου. Εάν θέλετε να κάνετε αναπαραγωγή από ένα τελικό σημείο ροής μη προεπιλεγμένες, χρησιμοποιήστε ένα άλλο πρόγραμμα αναπαραγωγής. Για παράδειγμα, [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###<a name="azure-media-player"></a>Πρόγραμμα αναπαραγωγής πολυμέσων Azure

Χρήση [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) για αναπαραγωγή το περιεχόμενό σας (απαλοιφή ή προστατευμένο) σε οποιαδήποτε από τις παρακάτω μορφές:

- Ομαλή ροή
- MPEG ΠΑΎΛΑ
- HLS
- Προοδευτική MP4


###<a name="flash-player"></a>Flash Player

####<a name="aes-encrypted-with-token"></a>Κρυπτογράφηση AES με διακριτικό

[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Προγράμματα αναπαραγωγής Silverlight

####<a name="monitoring"></a>Παρακολούθηση

[http://smf.cloudapp.NET/healthmonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>PlayReady με διακριτικό

[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>ΠΑΎΛΑ αναπαραγωγής

[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>Άλλες

Για να ελέγξετε HLS διευθύνσεις URL μπορείτε επίσης να χρησιμοποιήσετε:

- Σε συσκευή iOS **safari** ή
- **πρόγραμμα αναπαραγωγής HLS 3ivx** στα Windows.

##<a name="developing-video-players"></a>Ανάπτυξη προγραμμάτων αναπαραγωγής βίντεο

Για πληροφορίες σχετικά με τον τρόπο για να αναπτύξετε τις δικές σας παίκτες, ανατρέξτε στο θέμα [αναπτυσσόμενες αναπαραγωγής βίντεο](media-services-develop-video-players.md)




##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
