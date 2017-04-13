<properties
    pageTitle="Επισκόπηση δυναμικής συσκευασία | Microsoft Azure"
    description="Το θέμα παρέχει και Επισκόπηση δυναμικής συσκευασίας."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


# <a name="dynamic-packaging"></a>Δυναμική συσκευασία

##<a name="overview"></a>Επισκόπηση

Προστασία περιεχομένου μορφές με μια ποικιλία τεχνολογιών προγράμματος-πελάτη και Microsoft Azure Media Services μπορούν να χρησιμοποιηθούν για την παράδοση πολλά προέλευσης μορφές αρχείων πολυμέσων, ροή μορφές, πολυμέσων (για παράδειγμα, iOS, XBOX, Silverlight, Windows 8). Αυτά τα προγράμματα-πελάτες Κατανόηση διαφορετικά πρωτόκολλα, για παράδειγμα iOS απαιτεί μια μορφή V4 HTTP Live ροής (HLS) και Silverlight και Xbox απαιτούν ομαλή ροή. Εάν έχετε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit (πολλούς-ρυθμό μετάδοσης bit) MP4 αρχεία (ISO πολυμέσων βάσης 14496-12) ή ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit ομαλή ροή τα αρχεία που θέλετε να χρησιμοποιηθεί σε υπολογιστές-πελάτες που Κατανόηση ΠΑΎΛΑΣ MPEG, HLS ή ομαλή ροή, που θα πρέπει να επωφεληθείτε από δυναμική συσκευασία Media Services.

Με δυναμική συσκευασία μόνο που χρειάζεστε είναι για τη δημιουργία ενός περιουσιακού στοιχείου που περιέχει ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 αρχεία ή αρχεία ομαλή ροή προσαρμόσιμη ρυθμό μετάδοσης bit. Στη συνέχεια, ανάλογα με την καθορισμένη μορφή στην αίτηση δηλωτικό ή τμήματος, η ροή On απαιτήσεων διακομιστή θα εξασφαλίσετε ότι θα λαμβάνετε ροής στο πρωτόκολλο που έχετε επιλέξει. Ως αποτέλεσμα, θα πρέπει να αποθηκεύετε και να πληρώσετε για τα αρχεία σε μορφή μία χώρου αποθήκευσης και υπηρεσία Media Services θα δημιουργήσετε και θα λειτουργήσει την κατάλληλη απάντηση που βασίζεται σε αιτήσεις από ένα πρόγραμμα-πελάτη.

Το παρακάτω διάγραμμα παρουσιάζει την παραδοσιακή κωδικοποίηση και τη ροή εργασίας στατική συσκευασία.

![Στατική κωδικοποίηση](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Το παρακάτω διάγραμμα παρουσιάζει τη ροή εργασίας δυναμικής συσκευασία.

![Δυναμική κωδικοποίηση](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]Για να επωφεληθείτε από δυναμική συσκευασία, πρέπει να λάβετε πρώτα τουλάχιστον μία On demand ροής μονάδας για τη ροή τελικό σημείο από το οποίο σκοπεύετε να παράδοσης το περιεχόμενό σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να κλίμακα Media Services](media-services-portal-manage-streaming-endpoints.md).

##<a name="common-scenario"></a>Συνηθισμένο σενάριο

1. Στείλτε ένα αρχείο εισόδου (που ονομάζεται αρχείο θυγατρική). Για παράδειγμα, H.264, MP4 ή WMV (για τη λίστα των υποστηριζόμενων μορφών ανατρέξτε στο θέμα [Μορφές που υποστηρίζονται από το Media Encoder τυπική](media-services-media-encoder-standard-formats.md).

1. Κωδικοποίηση του αρχείου θυγατρική σε σύνολα H.264 MP4 προσαρμόσιμη ρυθμό μετάδοσης bit.

1. Δημοσίευση του περιουσιακού στοιχείου που περιέχει την προσαρμόσιμη ρυθμό μετάδοσης bit MP4 Ορισμός δημιουργώντας το προσδιοριστικό On ζήτηση.

1. Δημιουργήστε τις διευθύνσεις URL ροής για να αποκτήσετε πρόσβαση και ροή του περιεχομένου.


##<a name="preparing-assets-for-dynamic-streaming"></a>Προετοιμασία περιουσιακών στοιχείων για τη δυναμική ροή

Για να προετοιμάσετε το περιουσιακού στοιχείου για δυναμική ροής, έχετε δύο επιλογές:

1. [Αποστολή αρχείου κύριας](media-services-dotnet-upload-files.md).
2. [Χρησιμοποιήστε το Media Encoder τυπική κωδικοποιητή για την παραγωγή H.264 MP4 προσαρμόσιμη ρυθμό μετάδοσης bit σύνολα](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Ροή το περιεχόμενό σας](media-services-deliver-content-overview.md).


##<a id="unsupported_formats"></a>Μορφές που δεν υποστηρίζονται από δυναμική συσκευασία

Οι ακόλουθες μορφές αρχείων προέλευσης δεν υποστηρίζονται από δυναμική συσκευασία.

- Αρχεία ψηφιακών mp4 Dolby.
- Ψηφιακή ομαλή αρχεία Dolby.

##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]