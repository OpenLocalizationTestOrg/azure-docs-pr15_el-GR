<properties
    pageTitle="  Δημοσίευση περιεχομένου με την πύλη του Azure | Microsoft Azure"
    description="Αυτό το πρόγραμμα εκμάθησης σας καθοδηγεί στη διαδικασία δημοσίευσης το περιεχόμενό σας με την πύλη του Azure."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>

# <a name="publish-content-with-the-azure-portal"></a>Δημοσίευση περιεχομένου με την πύλη του Azure

> [AZURE.SELECTOR]
- [Πύλη](media-services-portal-publish.md)
- [.NET](media-services-deliver-streaming-content.md)
- [ΥΠΌΛΟΙΠΟ](media-services-rest-deliver-streaming-content.md)

## <a name="overview"></a>Επισκόπηση

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Azure. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/). 

Για την παροχή του χρήστη με μια διεύθυνση URL που μπορεί να χρησιμοποιηθεί για ροή ή να κάνετε λήψη του περιεχομένου, πρέπει πρώτα να "δημοσίευσης" σας περιουσιακών στοιχείων, δημιουργώντας ένα προσδιοριστικό. Locators παρέχουν πρόσβαση σε αρχεία που περιέχονται στο περιουσιακού στοιχείου. Υπηρεσίες πολυμέσων υποστηρίζει δύο τύπους locators: 

- Η ροή locators (OnDemandOrigin), χρησιμοποιείται για προσαρμόσιμη ροής (για παράδειγμα, για να ροή MPEG ΠΑΎΛΑ, HLS ή ομαλή ροή). Για να δημιουργήσετε μια ροή προσδιοριστικό σας περιουσιακών στοιχείων πρέπει να περιέχει ένα αρχείο .ism. 
- Προοδευτική (συσχετισμών Ασφαλείας) locators, χρησιμοποιούνται για την παράδοση του βίντεο μέσω προοδευτική λήψη.


Μια διεύθυνση URL ροή έχει την εξής μορφή και μπορείτε να το χρησιμοποιήσετε για την αναπαραγωγή ομαλή ροή περιουσιακών στοιχείων.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Για να δημιουργήσετε μια HLS ροή διεύθυνση URL, προσάρτηση (μορφή = m3u8 aapl) στη διεύθυνση URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Για να δημιουργήσετε μια ΠΑΎΛΑ MPEG ροή διεύθυνση URL, προσάρτηση (μορφή = mpd-χρόνου-csf) στη διεύθυνση URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Μια διεύθυνση URL συσχετισμών Ασφαλείας έχει την εξής μορφή.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση περιεχομένου παράδοση](media-services-deliver-content-overview.md).

>[AZURE.NOTE] Εάν χρησιμοποιήσατε την πύλη για να δημιουργήσετε locators πριν Μαρτίου 2015, δημιουργήθηκαν locators με μια ημερομηνία λήξης δύο έτος.  

Για να ενημερώσετε μια ημερομηνία λήξης σε ένα προσδιοριστικό, χρησιμοποιήστε το [ΥΠΌΛΟΙΠΟ](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) ή API [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) . Σημειώστε ότι, όταν ενημερώνετε την ημερομηνία λήξης της ένα προσδιοριστικό συσχετισμών Ασφαλείας, αλλάζει τη διεύθυνση URL.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Να χρησιμοποιήσουν την πύλη για τη δημοσίευση ενός περιουσιακού στοιχείου

Για να χρησιμοποιήσετε την πύλη για τη δημοσίευση ενός περιουσιακού στοιχείου, κάντε τα εξής:

1. Στην [πύλη του Azure](https://portal.azure.com/), επιλέξτε το λογαριασμό σας υπηρεσιών Azure Media Services.
1. Επιλέξτε **Ρυθμίσεις** > **περιουσιακών στοιχείων**.
1. Επιλέξτε το πάγιο που θέλετε να δημοσιεύσετε.
1. Κάντε κλικ στο κουμπί **Δημοσίευση** .
1. Επιλέξτε τον τύπο προσδιοριστικό.
2. Πατήστε **Προσθήκη**.

    ![Δημοσίευση](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Η διεύθυνση URL θα προστεθεί στη λίστα των **Διευθύνσεων URL που δημοσιεύονται**.

## <a name="play-content-from-the-portal"></a>Αναπαραγωγή περιεχομένου από την πύλη

Η πύλη του Azure παρέχει ένα πρόγραμμα αναπαραγωγής περιεχομένου που μπορείτε να χρησιμοποιήσετε για να ελέγξετε το βίντεό σας.

Κάντε κλικ στο επιθυμητό βίντεο και, στη συνέχεια, κάντε κλικ στο κουμπί **αναπαραγωγή** .

![Δημοσίευση](./media/media-services-portal-vod-get-started/media-services-play.png)

Εφαρμογή ορισμένα ζητήματα:

- Βεβαιωθείτε ότι έχει δημοσιευθεί το βίντεο.
- Σε αυτό το **πρόγραμμα αναπαραγωγής πολυμέσων** ακούγεται από την προεπιλεγμένη ροή τελικού σημείου. Εάν θέλετε να κάνετε αναπαραγωγή από ένα τελικό σημείο ροής μη προεπιλεγμένη, κάντε κλικ στην επιλογή για να αντιγράψετε τη διεύθυνση URL και χρησιμοποιήστε ένα άλλο πρόγραμμα αναπαραγωγής. Για παράδειγμα, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
- Πρέπει να εκτελεί τη ροή τελικό σημείο από το οποίο ροή.  
- Για τη ροή από ένα τελικό σημείο ροής, θα πρέπει να προσθέσετε τουλάχιστον μία ροή μονάδα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αυτό](media-services-portal-scale-streaming-endpoints.md) το θέμα.   

##<a name="next-steps"></a>Επόμενα βήματα

Αναθεώρηση Media Services διαδικασίες εκμάθησης.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


