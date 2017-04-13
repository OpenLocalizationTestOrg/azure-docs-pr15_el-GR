<properties
    pageTitle="Εντοπισμός πρόσωπο και τα συναισθήματα με αναλυτικά στοιχεία πολυμέσων Azure | Microsoft Azure"
    description="Αυτό το θέμα παρουσιάζει τον τρόπο για να εντοπίσετε όψεις και συναισθήματα με αναλυτικά στοιχεία πολυμέσων Azure."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"   
    ms.author="milanga;juliako;"/>
 
#<a name="detect-face-and-emotion-with-azure-media-analytics"></a>Εντοπισμός πρόσωπο και τα συναισθήματα με αναλυτικά στοιχεία πολυμέσων Azure

##<a name="overview"></a>Επισκόπηση

Ο επεξεργαστής πολυμέσων **Azure Media πρόσωπο ανίχνευσης** (πολλών Επεξεργαστών) σάς επιτρέπει να μέτρηση, να παρακολουθείτε τις κινήσεις και ακόμα και να μετρήσετε συμμετοχής του ακροατηρίου και αντίδρασης μέσω εκφράσεις του προσώπου. Αυτή η υπηρεσία περιέχει δύο δυνατότητες: 

- **Εντοπισμός πρόσωπο**

    Εντοπισμός πρόσωπο που εντοπίζει και παρακολουθεί human όψεις μέσα σε ένα βίντεο. Πολλαπλές όψεις να εντοπιστεί και στη συνέχεια να παρακολουθούνται όπως μετακινήσεων, με ώρα και την τοποθεσία μετα-δεδομένων επιστρέφονται σε ένα αρχείο JSON. Κατά την παρακολούθηση, θα επιχειρήσει να δώσετε μια συνεπή Ταυτότητα για να το ίδιο πρόσωπο, ενώ το άτομο μετακίνηση μέσα στην οθόνη, ακόμα και αν είναι διακόπτεται εμπόδια ή σύντομα αποχώρηση από το πλαίσιο.

    >[AZURE.NOTE]Υπηρεσίες αυτό δεν εκτελεί την αναγνώριση του προσώπου. Ένα άτομο που αφήνει το πλαίσιο ή γίνεται διακόπτεται εμπόδια για μεγάλο χρονικό διάστημα θα έχουν ένα νέο Αναγνωριστικό όταν μπορούν επιστρέψουν.

- **Εντοπισμός συναισθήματα**
    
    Εντοπισμός συναισθήματα είναι ένα προαιρετικό στοιχείο του επεξεργαστή πρόσωπο εντοπισμού πολυμέσων που επιστρέφει ανάλυση σε πολλαπλά συναισθηματικούς χαρακτηριστικά από τις όψεις εντοπιστεί, συμπεριλαμβανομένων των ευτυχία, sadness, φόβου, anger και πολλά άλλα. 

Το **Πρόγραμμα ανίχνευσης πρόσωπο πολυμέσων Azure** πολλών Επεξεργαστών είναι αυτήν τη στιγμή στην προεπισκόπηση.

Αυτό το θέμα παρέχει λεπτομέρειες σχετικά με το **Πρόγραμμα ανίχνευσης πρόσωπο πολυμέσων Azure** και δείχνει πώς μπορείτε να το χρησιμοποιήσετε με το Media Services SDK για .NET.

##<a name="face-detector-input-files"></a>Πρόσωπο εισαγωγής αρχείων ανίχνευσης

Αρχεία βίντεο. Προς το παρόν, υποστηρίζονται οι παρακάτω μορφές: MP4, MOV και WMV.

##<a name="face-detector-output-files"></a>Πρόσωπο αρχεία εξόδου ανίχνευσης

Το API ανίχνευσης και παρακολούθηση πρόσωπο παρέχει ανίχνευσης θέσης προσώπων υψηλής ακρίβειας και παρακολούθησης που μπορεί να ανιχνεύσει έως 64 human όψεις σε ένα βίντεο. Απροκάλυπτο όψεις παρέχουν τα καλύτερα αποτελέσματα, ενώ πλαϊνές όψεις και μικρές όψεις (μικρότερη ή ίση με 24 x 24 pixel) ενδέχεται να μην είναι ακριβή.

Τις όψεις Εντοπίστηκε και εντοπισμένες επιστρέφονται με συντεταγμένες (αριστερά, επάνω, πλάτος και ύψος) που υποδεικνύει τη θέση του όψεις στην εικόνα στην pixels, καθώς και έναν αριθμό Αναγνωριστικού πρόσωπο που υποδεικνύει την παρακολούθηση του. Αριθμούς των Αναγνωριστικών πρόσωπο είναι πιθανό να επαναφέρετε υπό συνθήκες, όταν το πρόσωπο μετωπικής χαθεί ή επικαλυπτόμενη στο πλαίσιο, αποτέλεσμα ορισμένα άτομα γρήγορα στους οποίους έχουν ανατεθεί πολλαπλές ταυτότητες.

###<a id="output_elements"></a>Στοιχεία του αρχείου JSON εξόδου

Για τον εντοπισμό πρόσωπο και λειτουργίας παρακολούθησης, το αποτέλεσμα εξόδου περιέχει τα μετα-δεδομένα από τις όψεις εντός του αρχείου που δίνεται στη μορφή JSON.

Τον εντοπισμό πρόσωπο και παρακολούθηση JSON περιλαμβάνει τα παρακάτω χαρακτηριστικά:

Στοιχείο|Περιγραφή
---|---
Έκδοση|Αυτό αναφέρεται στην έκδοση του API βίντεο.
Κλίμακα χρόνου|"Υποδιαιρέσεις" ανά δευτερόλεπτο του βίντεο.
Μετατόπιση|Αυτή είναι η ώρα μετατόπιση χρονικές σημάνσεις. Στην έκδοση 1.0 των API του βίντεο, αυτό θα είναι πάντα 0. Στο μέλλον υποστηρίζουμε σενάρια, αυτή η τιμή ενδέχεται να αλλάξουν.
Ταχύτητα καρέ|Καρέ το δευτερόλεπτο του βίντεο.
Τμήματα|Τα μετα-δεδομένα είναι κατατμημένη προς τα επάνω σε διαφορετικά τμήματα που ονομάζεται τμήματα. Κάθε τμήμα περιέχει μια έναρξης, διάρκεια, αριθμό διάστημα και συμβάντα.
Έναρξη|Την ώρα έναρξης του το πρώτο συμβάν 'υποδιαιρέσεις'.
Διάρκεια|Το μήκος του τμήματος, στο "Υποδιαιρέσεις".
Χρονικό διάστημα|Το χρονικό διάστημα από κάθε εγγραφή συμβάντος εντός του τμήματος, στο "Υποδιαιρέσεις".
Συμβάντα|Κάθε συμβάντος περιέχει τις όψεις εντοπίζονται και παρακολουθούνται μέσα σε συγκεκριμένη χρονική διάρκεια. Είναι ένας πίνακας με πίνακα συμβάντων. Ο εξωτερικός πίνακας αντιπροσωπεύει ένα χρονικό διάστημα. Ο εσωτερικός πίνακας αποτελείται από 0 ή περισσότερα συμβάντα που έγιναν σε αυτό το σημείο στο χρόνο. Μια κενή αγκύλη [] σημαίνει ότι δεν υπάρχει όψεις εντοπίστηκαν.
ΑΝΑΓΝΩΡΙΣΤΙΚΌ| Το Αναγνωριστικό του το πρόσωπο που παρακολουθείται. Αυτός ο αριθμός μπορεί να αλλάξει κατά λάθος αν γίνει δεν εντοπίζεται ένα πρόσωπο. Ένα συγκεκριμένο άτομο θα πρέπει να έχουν το ίδιο Αναγνωριστικό σε όλο το συνολικό βίντεο, αλλά αυτό δεν μπορεί να εξασφαλιστεί λόγω περιορισμών στο ο αλγόριθμος ανίχνευσης (φραγής της, κ.λπ.)
X, Y|Η επάνω αριστερά X και Y συντεταγμένες το πρόσωπο οριοθέτησης σε μια κανονικοποιημένη κλίμακα 0,0 να 1.0. <br/>-X και Y συντεταγμένες είναι σχετική σε οριζόντιο πάντα, επομένως εάν έχετε έναν κατακόρυφο βίντεο (ή ανάποδα, στην περίπτωση iOS), θα πρέπει να αντιμεταθέσετε τις συντεταγμένες αντίστοιχα.
Πλάτος, ύψος|Το πλάτος και το ύψος του το πρόσωπο οριοθέτησης σε μια κανονικοποιημένη κλίμακα 0,0 να 1.0.
facesDetected|Αυτό βρίσκεται στο τέλος των αποτελεσμάτων JSON και συνοψίζει τον αριθμό των όψεις που εντοπίστηκε αλγόριθμο κατά τη διάρκεια του βίντεο. Επειδή τα αναγνωριστικά μπορεί να γίνει επαναφορά κατά λάθος αν γίνει δεν εντοπίζεται ένα πρόσωπο (π.χ. πρόσωπο μεταβαίνει εκτός οθόνης, εμφανίσεις δεν βρίσκομαι στον υπολογιστή), αυτός ο αριθμός δεν μπορεί να ισούται πάντα τον αριθμό true όψεις του βίντεο.

Πρόγραμμα ανίχνευσης πρόσωπο χρησιμοποιεί τεχνικές κατακερματισμός (όπου τα μετα-δεδομένα μπορεί να είναι χωρισμένα σε μπλοκ βάσει χρόνου και μπορείτε να κάνετε λήψη μόνο ό, τι χρειάζεστε) και αγοράς (όπου τα συμβάντα είναι χωρισμένα σε περίπτωση που λαμβάνουν πολύ μεγάλο). Ορισμένες απλών υπολογισμών μπορεί να σας βοηθήσει να μετασχηματισμός των δεδομένων. Για παράδειγμα, εάν ένα συμβάν αποτελέσματα στο 6300 (υποδιαιρέσεις), με κλίμακα χρόνου της 2997 (υποδιαιρέσεις/sec) και ταχύτητα καρέ του 29,97 (καρέ/sec), στη συνέχεια:

- Έναρξη/χρονικής κλίμακας = 2.1 δευτερόλεπτα
- Δευτερόλεπτα x (ταχύτητα καρέ/κλίμακα χρόνου) = 63 πλαισίων

Ακολουθεί ένα απλό παράδειγμα από την εξαγωγή του JSON σε ένα ανά μορφή πλαισίων για πρόσωπο ανίχνευσης και παρακολούθηση:
    
    var faceDetectionResultJsonString = operationResult.ProcessingResult;
    var faceDetecionTracking = 
         JsonConvert.DeserializeObject<FaceDetectionResult>(faceDetectionResultJsonString, settings);


##<a name="face-detection-input-and-output-example"></a>Πρόσωπο εντοπισμού εισόδου και εξόδου παράδειγμα

###<a name="input-video"></a>Το βίντεο εισαγωγής

[Εισαγωγή βίντεο](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Ρύθμιση παραμέτρων εργασίας (προεπιλογή)

Κατά τη δημιουργία μιας εργασίας με **Ανίχνευσης πρόσωπο πολυμέσων Azure**, πρέπει να καθορίσετε μια προκαθορισμένη ρύθμιση παραμέτρων. Το παρακάτω προκαθορισμένης ρύθμισης παραμέτρων είναι μόνο για τον εντοπισμό πρόσωπο.

    {"version":"1.0"}

###<a name="json-output"></a>JSON εξόδου

Το παρακάτω παράδειγμα JSON εξόδου ήταν δεκαδικά ψηφία περικόπτονται.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

##<a name="emotion-detection-input-and-output-example"></a>Εντοπισμός συναισθήματα εισόδου και εξόδου παράδειγμα


###<a name="input-video"></a>Το βίντεο εισαγωγής

[Εισαγωγή βίντεο](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Ρύθμιση παραμέτρων εργασίας (προεπιλογή)

Κατά τη δημιουργία μιας εργασίας με **Ανίχνευσης πρόσωπο πολυμέσων Azure**, πρέπει να καθορίσετε μια προκαθορισμένη ρύθμιση παραμέτρων. Καθορίζει την εξής προκαθορισμένη ρύθμιση παραμέτρων για να δημιουργήσετε με βάση τον εντοπισμό συναισθήματα JSON.
    
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


####<a name="attribute-descriptions"></a>Περιγραφές των χαρακτηριστικών

Όνομα χαρακτηριστικού|Περιγραφή
---|---
Κατάσταση λειτουργίας|Όψεις: Πρόσωπο μόνο ανίχνευσης <br/>AggregateEmotion: Τιμές επιστροφής average συναισθήματα για όλες τις όψεις στο πλαίσιο.
AggregateEmotionWindowMs|Χρησιμοποιήστε, εάν η επιλεγμένη λειτουργία AggregateEmotion. Καθορίζει το μήκος του βίντεο που χρησιμοποιούνται για την παραγωγή κάθε συγκεντρωτικών αποτελεσμάτων, σε χιλιοστά του δευτερολέπτου.
AggregateEmotionIntervalMs|Χρησιμοποιήστε, εάν η επιλεγμένη λειτουργία AggregateEmotion. Καθορίζει με τι συχνότητα για την παραγωγή συγκεντρωτικών αποτελεσμάτων.

####<a name="aggregate-defaults"></a>Προεπιλογές συγκεντρωτικών αποτελεσμάτων

Κάτω από το στοιχείο συνιστώνται τιμές των συγκεντρωτικών αποτελεσμάτων ρυθμίσεων παράθυρο και το χρονικό διάστημα. AggregateEmotionWindowMs πρέπει να είναι μεγαλύτερη από AggregateEmotionIntervalMs.

   |Προεπιλογές (s)|Max(s)|Min(s)
---|---|---|---
AggregateEmotionWindowMs|0,5|2|0,25
AggregateEmotionIntervalMs|0,5|1|0,25

###<a name="json-output"></a>JSON εξόδου

JSON την έξοδο για συγκέντρωση συναισθήματα (περικοπεί):
 
    
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


##<a name="limitations"></a>Περιορισμοί

- Υποστηριζόμενες μορφές βίντεο εισαγωγής περιλαμβάνουν MP4, MOV και WMV.
- Η περιοχή μέγεθος ανιχνεύσιμες πρόσωπο είναι 24 x 24 σε 2048 x 2048 pixel. Δεν είναι δυνατός τις όψεις από αυτήν την περιοχή.
- Για κάθε βίντεο, ο μέγιστος αριθμός όψεις που επιστρέφεται είναι 64.
- Ορισμένες όψεις δεν μπορεί να ανιχνεύονται λόγω τεχνικών δυσκολιών; π.χ. πολύ μεγάλες πρόσωπο γωνίες (κεφαλής ενέχει) και μεγάλο φραγής της. Απροκάλυπτο και κοντά μετωπικής όψεις έχουν καλύτερα αποτελέσματα.


## <a name="sample-code"></a>Δείγμα κώδικα

Τα ακόλουθα πρόγραμμα παρουσιάζει πώς μπορείτε να:

1. Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε ένα αρχείο πολυμέσων σε περιουσιακού στοιχείου.
1. Δημιουργεί μια εργασία με μια εργασία εντοπισμού πρόσωπο που βασίζεται σε ένα αρχείο ρύθμισης παραμέτρων που περιέχει το παρακάτω υπόδειγμα json. 
                    
        {
            "version": "1.0"
        }

1. Η λήψη των αρχείων JSON εξόδου. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceDetection
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                static void Main(string[] args)
                {
        
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
        
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
        
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
        
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
        
                    // Launch the job.
                    job.Submit();
        
                    // Check job execution and wait for job to finish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        
                    progressJobTask.Wait();
        
                    // If job state is Error, the event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                        Console.WriteLine(string.Format("Error: {0}. {1}",
                                                        error.Code,
                                                        error.Message));
                        return null;
                    }
        
                    return job.OutputMediaAssets[0];
                }
        
                static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
                {
                    IAsset asset = _context.Assets.Create(assetName, options);
        
                    var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                    assetFile.Upload(filePath);
        
                    return asset;
                }
        
                static void DownloadAsset(IAsset asset, string outputDirectory)
                {
                    foreach (IAssetFile file in asset.AssetFiles)
                    {
                        file.Download(Path.Combine(outputDirectory, file.Name));
                    }
                }
        
                static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors
                        .Where(p => p.Name == mediaProcessorName)
                        .ToList()
                        .OrderBy(p => new Version(p.Version))
                        .LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor",
                                                                   mediaProcessorName));
        
                    return processor;
                }
        
                static private void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
        
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine();
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
                            // Display or log error details as needed.
                            // LogJobStop(job.Id);
                            break;
                        default:
                            break;
                    }
                }
        
            }
        }


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Σχετικές συνδέσεις

[Επισκόπηση ανάλυσης υπηρεσίες πολυμέσων Azure](media-services-analytics-overview.md)

[Azure επιδείξεις αναλυτικά στοιχεία πολυμέσων](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)