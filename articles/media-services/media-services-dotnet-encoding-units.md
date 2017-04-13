<properties 
    pageTitle="Πώς μπορείτε να προσθέσετε κωδικοποίησης μονάδες" 
    description="Μάθετε πώς μπορείτε να τον τρόπο προσθήκης κωδικοποίησης μονάδες με .NET"  
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
    ms.date="09/01/2016"
    ms.author="juliako;milangada;gtrifonov"/>


#<a name="how-to-scale-encoding-with-net-sdk"></a>Τρόπος για να κλιμακωθεί κωδικοποίηση με .NET SDK

> [AZURE.SELECTOR]
- [Πύλη](media-services-portal-scale-media-processing.md )
- [.NET](media-services-dotnet-encoding-units.md)
- [ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Επισκόπηση

>[AZURE.IMPORTANT] Βεβαιωθείτε ότι για να δείτε το θέμα [Επισκόπηση](media-services-scale-media-processing-overview.md) για να λάβετε περισσότερες πληροφορίες σχετικά με την κλιμάκωση πολυμέσων επεξεργασίας το θέμα.
 
Για να αλλάξετε τον τύπο δεσμευμένη μονάδας και τον αριθμό των κωδικοποίηση δεσμευμένη μονάδες χρησιμοποιώντας .NET SDK, κάντε τα εξής:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);
    
    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

##<a name="opening-a-support-ticket"></a>Ανοίγει ένα δελτίο υποστήριξης

Από προεπιλογή για κάθε λογαριασμό Media Services μπορούν να κλίμακα σε έως 25 κωδικοποίησης και 5 On Demand ροής δεσμευμένη μονάδες. Μπορείτε να ζητήσετε ανώτερο όριο, ανοίγοντας ένα δελτίο υποστήριξης.

###<a name="open-a-support-ticket"></a>Ανοίξτε ένα δελτίο υποστήριξης

Για να ανοίξετε μια υποστήριξη δελτίων κάντε τα εξής:

1. Κάντε κλικ στην επιλογή [Λήψη υποστήριξης](https://manage.windowsazure.com/?getsupport=true). Εάν δεν είστε συνδεδεμένοι, θα σας ζητηθεί να εισαγάγετε τα διαπιστευτήριά σας.

1. Επιλέξτε τη συνδρομή σας.

1. Στην περιοχή Τύπος υποστήριξης, επιλέξτε "Τεχνικής".

1. Κάντε κλικ στο στοιχείο "Δημιουργία δελτίου".

1. Επιλέξτε "Υπηρεσιών Azure Media Services" στη λίστα προϊόντων που παρουσιάζονται στην επόμενη σελίδα.

1. Επιλέξτε έναν τύπο"θέμα" που είναι κατάλληλη για το πρόβλημα.

1. Κάντε κλικ στο κουμπί συνέχεια.

1. Ακολουθήστε τις οδηγίες στην επόμενη σελίδα και, στη συνέχεια, εισαγάγετε λεπτομέρειες σχετικά με το πρόβλημα.

1. Κάντε κλικ στην επιλογή υποβολή για να ανοίξετε το δελτίο.



##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
