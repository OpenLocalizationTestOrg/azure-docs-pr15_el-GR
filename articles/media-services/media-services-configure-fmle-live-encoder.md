<properties 
    pageTitle="Ρύθμιση παραμέτρων ο κωδικοποιητής FMLE για να στείλετε μια ζωντανή ροή μία ρυθμό μετάδοσης bit | Microsoft Azure" 
    description="Αυτό το θέμα δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους ο κωδικοποιητής Flash πολυμέσων Live κωδικοποιητή (FMLE) για να στείλετε μια μεμονωμένη ρυθμό μετάδοσης bit ροή σε κανάλια αθροιστικής μέτρησης ενισχύσεων που έχουν ενεργοποιηθεί για το πραγματικό κωδικοποίηση." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako;cenkdin;anilmur"/>

#<a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a>Χρησιμοποιήστε τον κωδικοποιητή FMLE για να στείλετε μια ζωντανή ροή μία ρυθμό μετάδοσης bit

> [AZURE.SELECTOR]
- [FMLE](media-services-configure-fmle-live-encoder.md)
- [Στοιχειακή Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)

Αυτό το θέμα δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους ο κωδικοποιητής [Flash Live ή κάθετο](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) για να στείλετε μια μεμονωμένη ρυθμό μετάδοσης bit ροή σε κανάλια αθροιστικής μέτρησης ενισχύσεων που έχουν ενεργοποιηθεί για το πραγματικό κωδικοποίηση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εργασία με τα κανάλια που είναι ενεργοποιημένες για να εκτελέσετε Live κωδικοποίηση με υπηρεσιών Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να διαχειριστείτε Azure Media Services (αθροιστικής μέτρησης ενισχύσεων) με το εργαλείο Explorer υπηρεσίες πολυμέσων Azure (AMSE). Αυτό το εργαλείο εκτελείται μόνο σε Υπολογιστή με Windows. Εάν είστε σε Mac ή Linux, χρησιμοποιήστε την πύλη του Azure για τη δημιουργία [καναλιών](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) και των [προγραμμάτων](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).

Σημειώστε ότι αυτό το πρόγραμμα εκμάθησης περιγράφει τη χρήση AAC. Ωστόσο, FMLE δεν υποστηρίζει AAC από προεπιλογή. Θα πρέπει να αγοράσετε μια προσθήκη για AAC κωδικοποίηση όπως από MainConcept: [Προσθήκη AAC](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- [Δημιουργήστε ένα λογαριασμό των υπηρεσιών Azure Media Services](media-services-portal-create-account.md)
- Βεβαιωθείτε ότι υπάρχει ένα τελικό σημείο ροής με τουλάχιστον μία ροή μονάδα που έχει εκχωρηθεί. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση ροής τα τελικά σημεία σε λογαριασμό υπηρεσίες πολυμέσων](media-services-portal-manage-streaming-endpoints.md)
- Εγκαταστήστε την πιο πρόσφατη έκδοση του εργαλείου [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Εκκίνηση του εργαλείου και συνδεθείτε στο λογαριασμό σας αθροιστικής μέτρησης ενισχύσεων.

##<a name="tips"></a>Συμβουλές

- Όποτε είναι δυνατό, χρησιμοποιήστε μια σύνδεση στο internet ενσύρματο.
- Μια καλή πρακτικός κανόνας κατά τον προσδιορισμό των απαιτήσεων εύρους ζώνης είναι να double τη ροή bitrates. Ενώ δεν είναι υποχρεωτική απαίτηση, θα σας βοηθήσει να συμβάλει στην αντιμετώπιση την επίδραση των συμφόρηση δικτύου.
- Όταν χρησιμοποιείτε λογισμικό με βάση κωδικοποιητές, κλείσετε όλα τα περιττά προγράμματα.

## <a name="create-a-channel"></a>Δημιουργία καναλιού

1.  Στο εργαλείο AMSE, μεταβείτε στην καρτέλα **Live** και, κάντε δεξί κλικ μέσα στην περιοχή του καναλιού. Επιλέξτε **Δημιουργία καναλιού...** από το μενού.

![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. Καθορίστε ένα όνομα καναλιού, το πεδίο περιγραφής είναι προαιρετικό. Στην περιοχή ρυθμίσεις καναλιού, επιλέξτε **Τυπική** για την επιλογή Live κωδικοποίηση, με το πρωτόκολλο εισαγωγής που έχει οριστεί σε **RTMP**. Μπορείτε να αφήσετε όλες τις άλλες ρυθμίσεις όπως είναι.


Βεβαιωθείτε ότι είναι επιλεγμένη η **Έναρξη τώρα το νέο κανάλι** .

3. Κάντε κλικ στην επιλογή **Δημιουργία καναλιού**.
![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

>[AZURE.NOTE] Το κανάλι μπορεί να διαρκέσει έως και 20 λεπτά για να ξεκινήσετε.


Κατά την εκκίνηση του καναλιού, μπορείτε να [ρυθμίσετε τις παραμέτρους του κωδικοποιητή](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).

>[AZURE.IMPORTANT] Σημειώστε ότι χρέωση ξεκινά μόλις καναλιού τίθεται σε κατάσταση είστε έτοιμοι. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [μέλη του καναλιού](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_fmle_rtmp></a>Ρύθμιση παραμέτρων ο κωδικοποιητής FMLE

Σε αυτό το πρόγραμμα εκμάθησης χρησιμοποιούνται οι ακόλουθες ρυθμίσεις εξόδου. Τα υπόλοιπα αυτή η ενότητα περιγράφει τα βήματα ρύθμισης παραμέτρων με περισσότερες λεπτομέρειες. 

**Βίντεο**:
 
- Ο κωδικοποιητής: H.264 
- Προφίλ: Υψηλό (επίπεδο 4.0) 
- Ρυθμό μετάδοσης bit: 5000 kbps 
- Βασικό καρέ: 2 δευτερόλεπτα (60 δευτερόλεπτα) 
- Πλαίσιο χρέωση: 30
 
**Ήχου**:

- Ο κωδικοποιητής: AAC (LC) 
- Ρυθμό μετάδοσης bit: 192 kbps 
- Δείγμα χρέωση: 44,1 kHz


###<a name="configuration-steps"></a>Βήματα ρύθμισης παραμέτρων

1. Μεταβείτε στο περιβάλλον εργασίας του Flash πολυμέσων Live του κωδικοποιητή (FMLE) σε υπολογιστή που χρησιμοποιείται.

    Το περιβάλλον εργασίας είναι μία κύρια σελίδα ρυθμίσεων. Αφιερώστε σημείωσης από τις ακόλουθες προτεινόμενες ρυθμίσεις για να ξεκινήσετε με ροή χρησιμοποιώντας FMLE.
    
    - Μορφή: Ρυθμός καρέ H.264: 30.00 
    - Μέγεθος εισόδου: 1280 x 720 
    - Ρυθμός bit: 5000 Kbps (μπορούν να ρυθμιστούν με περιορισμούς του δικτύου)  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

    Όταν χρησιμοποιείτε πεπλεγμένη προελεύσεις, επικοινωνήστε σημάδι ελέγχου στην επιλογή ""Αποσύμπλεξη ""

2. Επιλέξτε το εικονίδιο κλειδιού δίπλα στην επιλογή μορφής, θα πρέπει να είναι αυτές οι πρόσθετες ρυθμίσεις:

    - Προφίλ: κύριες
    - Επίπεδο: 4.0
    - Βασικό καρέ συχνότητα: 2 δευτερόλεπτα 
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)

3. Ορίστε τα παρακάτω σημαντικά ρύθμιση ήχου:
    
    - Μορφή: AAC 
    - Δείγμα χρέωση: 44100 Hz
    - Ρυθμό μετάδοσης bit: 192 Kbps
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)

6. Λήψη του καναλιού εισαγωγής διεύθυνσης URL για να εκχωρήσετε σε το FMLE **RTMP τελικού σημείου**.
    
    Μετάβαση στο εργαλείο AMSE και ελέγξτε την κατάσταση ολοκλήρωσης καναλιού. Όταν η κατάσταση έχει αλλάξει από **Έναρξη** σε **εκτελείται**, μπορείτε να λάβετε τη διεύθυνση URL εισόδου.
      
    Όταν εκτελείται το κανάλι, κάντε δεξί κλικ στο όνομα καναλιού, περιηγηθείτε προς τα κάτω, hover επάνω από τη **Διεύθυνση URL εισαγωγής Αντιγραφή στο Πρόχειρο** και, στη συνέχεια, επιλέξτε **Κύρια διεύθυνση URL εισόδου**.  
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)

7. Επικολλήστε αυτές τις πληροφορίες στο πεδίο **Διεύθυνση URL FMS** της ενότητας εξόδου και αντιστοιχίστε ένα όνομα ροής. 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    Για επιπλέον πλεονασμού, επαναλάβετε αυτά τα βήματα με τη δευτερεύουσα διεύθυνση URL εισόδου.
8. Επιλέξτε **σύνδεση**.

>[AZURE.IMPORTANT]Πριν κάνετε κλικ στο κουμπί " **σύνδεση**", **πρέπει να** βεβαιωθείτε ότι το κανάλι είναι έτοιμοι. 
>Επίσης, βεβαιωθείτε ότι δεν για να αφήσετε το κανάλι σε κατάσταση ετοιμότητας χωρίς μια εισαγωγής συνεισφορά τροφοδοσίας για περισσότερο από > 15 λεπτά.

##<a name="test-playback"></a>Αναπαραγωγή δοκιμής
  
1. Μεταβείτε στο εργαλείο AMSE και, κάντε δεξί κλικ στο κανάλι να ελεγχθεί. Από το μενού, το δείκτη του ποντικιού πάνω από **την προεπισκόπηση αναπαραγωγής** και επιλέξτε **με το Azure Media Player**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

Εάν η ροή εμφανίζεται στο πρόγραμμα αναπαραγωγής, στη συνέχεια, ο κωδικοποιητής έχει τις σωστές ρυθμίσεις για να συνδεθείτε με αθροιστικής μέτρησης ενισχύσεων. 

Εάν λάβατε ένα μήνυμα σφάλματος, το κανάλι θα πρέπει να γίνει επαναφορά και ρυθμίσεις encoder προσαρμοστεί. Ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων](media-services-troubleshooting-live-streaming.md) για οδηγίες.  

##<a name="create-a-program"></a>Δημιουργία ενός προγράμματος

1. Μόλις επιβεβαιωθεί αναπαραγωγή καναλιού, δημιουργήστε ένα πρόγραμμα. Στην καρτέλα **Live** στο εργαλείο AMSE, κάντε δεξί κλικ μέσα στην περιοχή πρόγραμμα και επιλέξτε **Δημιουργία νέου προγράμματος**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)

2. Ονομάστε το πρόγραμμα και, εάν είναι απαραίτητο, προσαρμόστε το **Μήκος του παραθύρου αρχειοθέτησης** (η οποία από προεπιλογή σε 4 ώρες). Μπορείτε επίσης να καθορίσετε μια θέση αποθήκευσης ή να αφήστε ως την προεπιλεγμένη.  
3. Επιλέξτε το πλαίσιο **εκκίνηση του προγράμματος τώρα** .
4. Κάντε κλικ στην επιλογή **Δημιουργία πρόγραμμα**.  
  
    Σημείωση: Πρόγραμμα δημιουργίας διαρκεί λιγότερο χρόνο από τη δημιουργία του καναλιού.    
 
5. Μετά την εκτέλεση του προγράμματος, επιβεβαιώστε αναπαραγωγής, κάνοντας δεξί κλικ στο πρόγραμμα και περιήγηση σε **αναπαραγωγή τα προγράμματα** και, στη συνέχεια, επιλέγοντας **με το Azure Media Player**.  
6. Μόλις επιβεβαιωθεί, κάντε δεξί κλικ το πρόγραμμα ξανά και επιλέξτε **Αντιγραφή της διεύθυνσης URL εξόδου στο Πρόχειρο** (ή ανάκτηση αυτές τις πληροφορίες από την επιλογή **ρυθμίσεις και πληροφορίες για το πρόγραμμα** από το μενού). 

Η ροή είναι τώρα είστε έτοιμοι να είναι ενσωματωμένο σε ένα πρόγραμμα αναπαραγωγής ή κατανεμημένο σε ένα ακροατήριο για ζωντανή προβολή.  


## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

Ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων](media-services-troubleshooting-live-streaming.md) για οδηγίες. 


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]