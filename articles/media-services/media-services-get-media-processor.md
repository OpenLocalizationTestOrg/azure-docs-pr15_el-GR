<properties 
    pageTitle="Πώς μπορείτε να δημιουργήσετε ένα πρόγραμμα επεξεργασίας πολυμέσων | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα στοιχείο επεξεργαστής πολυμέσων για να κωδικοποιήσετε, μετατροπή μορφής, κρυπτογράφηση ή αποκρυπτογράφηση περιεχομένου πολυμέσων για υπηρεσιών Azure Media Services. Δείγματα κώδικα είναι γραμμένες σε C# και χρησιμοποιήστε το Media Services SDK για .NET." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="how-to-get-a-media-processor-instance"></a>Πώς μπορείτε να: γρήγορα μια παρουσία επεξεργαστής πολυμέσων

> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [ΥΠΌΛΟΙΠΟ](media-services-rest-get-media-processor.md)


##<a name="overview"></a>Επισκόπηση

Στις υπηρεσίες πολυμέσων ένα πρόγραμμα επεξεργασίας πολυμέσων είναι ένα στοιχείο που χειρίζεται μια εργασία συγκεκριμένες επεξεργασίας, όπως η κωδικοποίηση, μορφοποιήστε μετατροπής, η κρυπτογράφηση ή αποκρυπτογράφηση περιεχομένου πολυμέσων. Συνήθως δημιουργείτε έναν επεξεργαστή πολυμέσων όταν δημιουργείτε μια εργασία για να κωδικοποιήσετε, κρυπτογράφηση ή να μετατρέψετε τη μορφή του περιεχομένου πολυμέσων.

Ο παρακάτω πίνακας παρέχει το όνομα και περιγραφή για κάθε επεξεργαστή διαθέσιμη πολυμέσων.

Όνομα επεξεργαστής πολυμέσων|Περιγραφή|Περισσότερες πληροφορίες
---|---|---
Τυπική κωδικοποιητή πολυμέσων|Παρέχει τυπικές δυνατότητες για κωδικοποίηση στη ζήτηση. |[Επισκόπηση και σύγκριση των Azure σε ζήτηση κωδικοποιητές πολυμέσων](media-services-encode-asset.md)
Ροή εργασίας Premium κωδικοποιητή πολυμέσων|Σας επιτρέπει να εκτελείτε κωδικοποίησης εργασιών με ροή εργασίας Premium κωδικοποιητή πολυμέσων.|[Επισκόπηση και σύγκριση των Azure σε ζήτηση κωδικοποιητές πολυμέσων](media-services-encode-asset.md)
Ευρετήριο πολυμέσων Azure| Σας επιτρέπει να κάνετε τα αρχεία πολυμέσων και το περιεχόμενο με δυνατότητα αναζήτησης, καθώς και να δημιουργούν κλειστές λεζάντες κομμάτια και λέξεις-κλειδιά.|[Ευρετήριο πολυμέσων Azure](media-services-index-content.md)
Azure Media Hyperlapse (έκδοση preview)|Σας επιτρέπει να εξομαλύνει τη "χτυπήματα" στο βίντεό σας με το βίντεο σταθεροποίησης. Σας επιτρέπει επίσης να επιταχύνετε το περιεχόμενό σας σε ένα clip ΑΝΑΛΩΣΙΜΩΝ.|[Hyperlapse πολυμέσων Azure](media-services-hyperlapse-content.md)
Azure Media Encoder|Η απόσβεση
Αποκρυπτογράφηση χώρου αποθήκευσης| Η απόσβεση|
Συσκευασία πολυμέσων Azure|Η απόσβεση|
Μονάδα κρυπτογράφησης πολυμέσων Azure|Η απόσβεση|

##<a name="get-media-processor"></a>Λήψη επεξεργαστής πολυμέσων

Την ακόλουθη μέθοδο δείχνει πώς μπορείτε να λάβετε μια παρουσία επεξεργαστής πολυμέσων. Το παράδειγμα κώδικα προϋποθέτει τη χρήση μιας μεταβλητής επιπέδου λειτουργικής μονάδας με το όνομα **_context** για αναφορά το περιβάλλον του διακομιστή, όπως περιγράφεται στην ενότητα [Τρόπος: σύνδεση με πολυμέσων υπηρεσίες μέσω προγραμματισμού](media-services-dotnet-connect-programmatically.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
        return processor;
    }


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα που γνωρίζετε πώς μπορείτε να λάβετε μια παρουσία επεξεργαστής πολυμέσων, μεταβείτε στο θέμα [πώς μπορείτε να κωδικοποιήσετε ενός περιουσιακού στοιχείου](media-services-dotnet-encode-with-media-encoder-standard.md) που θα σας δείξει πώς να χρησιμοποιήσετε την τυπική κωδικοποιητή πολυμέσων για να κωδικοποιήσετε ενός περιουσιακού στοιχείου.


