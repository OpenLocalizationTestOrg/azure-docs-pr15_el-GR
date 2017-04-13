<properties
pageTitle="Χρησιμοποιήστε τη γραμμή σύνδεσης βίντεο Office 365 στο εφαρμογές σας λογικής | Microsoft Azure"
description="Γρήγορα αποτελέσματα με τη γραμμή σύνδεσης βίντεο Office 365 σε εφαρμογές λογικής σας Microsoft Azure εφαρμογής υπηρεσίας"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office365-video-connector"></a>Γρήγορα αποτελέσματα με τη γραμμή σύνδεσης βίντεο Office 365
Σύνδεση με βίντεο Office 365 για να λάβετε πληροφορίες σχετικά με το Office 365 βίντεο, λάβετε μια λίστα με βίντεο και πολλά άλλα. Η γραμμή σύνδεσης βίντεο Office 365 μπορούν να χρησιμοποιηθούν από:

- Λογική εφαρμογών 

>[AZURE.NOTE] Αυτήν την έκδοση του άρθρου ισχύει για έκδοση σχήματος 2015-08-01-προεπισκόπηση της λογικής εφαρμογών. Αυτή η γραμμή σύνδεσης δεν υποστηρίζεται σε προηγούμενες εκδόσεις σχήματος.

Με το βίντεο Office 365, μπορείτε:

- Δημιουργήστε την επιχειρηματική ροή με βάση τα δεδομένα που λαμβάνετε από το βίντεο Office 365. 
- Χρήση ενεργειών που έλεγχος της κατάστασης πύλης με βίντεο, λάβετε μια λίστα με όλα βίντεο σε ένα κανάλι και πολλά άλλα. Αυτές οι ενέργειες αποσπάσετε μια απάντηση και, στη συνέχεια, κάντε το αποτέλεσμα είναι διαθέσιμες για άλλες ενέργειες. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε τη γραμμή σύνδεσης Bing αναζήτησης για να πραγματοποιήσετε αναζήτηση για το βίντεο Office 365, και να, στη συνέχεια, χρησιμοποιήστε τη γραμμή σύνδεσης βίντεο Office 365 για να λάβετε πληροφορίες σχετικά με αυτό το βίντεο. Αν το βίντεο πληροί τις απαιτήσεις σας, μπορείτε να καταχωρήσετε σε αυτό το βίντεο στο Facebook. 

Για να προσθέσετε μια πράξη σε εφαρμογές της λογικής, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής λογική](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Εναύσματα και ενέργειες

Η γραμμή σύνδεσης βίντεο Office 365 περιλαμβάνει τις ακόλουθες ενέργειες που είναι διαθέσιμες. Υπάρχουν εναύσματα.

| Εναυσμάτων | Ενέργειες|
| --- | --- |
| Κανένας | <ul><li>Ελέγχει την κατάσταση πύλης βίντεο</li><li>Λήψη όλων των καναλιών ορατό</li><li>Λήψη διεύθυνσης url αναπαραγωγή των υπηρεσιών Azure Media Services δηλωτικό για βίντεο</li><li>Λάβετε το διακριτικό φορέα για να αποκτήσετε πρόσβαση σε αποκρυπτογράφηση του βίντεο</li><li>Λαμβάνει πληροφορίες σχετικά με μια συγκεκριμένη Office 365 βίντεο</li><li>Παραθέτει σε λίστα όλα τα βίντεο Office 365 που είναι παρόντες σε ένα κανάλι</li></ul>

Όλες τις γραμμές σύνδεσης υποστηρίζει δεδομένα σε μορφές JSON και XML. 

## <a name="create-a-connection-to-office365-video-connector"></a>Δημιουργία σύνδεσης με το βίντεο Office 365 σύνδεσης
Όταν προσθέτετε αυτή η γραμμή σύνδεσης για τις εφαρμογές σας λογικής, πρέπει να εισόδου στο λογαριασμό σας στο βίντεο Office 365 και λογική εφαρμογών για να συνδεθείτε στο λογαριασμό σας.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Αφού δημιουργήσετε τη σύνδεση, εισαγάγετε τις ιδιότητες βίντεο Office 365, όπως το όνομα του μισθωτή ή κανάλι αναγνωριστικό. Η **αναφορά REST API** σε αυτό το θέμα περιγράφει αυτές τις ιδιότητες.

>[AZURE.TIP] Μπορείτε να χρησιμοποιήσετε αυτήν τη σύνδεση ίδια βίντεο Office 365 σε άλλες εφαρμογές της λογικής.

## <a name="swagger-rest-api-reference"></a>Αναφορά swagger REST API
Ισχύει για την έκδοση: 1.0.

### <a name="checks-video-portal-status"></a>Ελέγχει την κατάσταση πύλης βίντεο 
Ελέγχει το βίντεο κατάσταση πύλης για να δείτε εάν είναι ενεργοποιημένες τις υπηρεσίες βίντεο.  
```GET: /{tenant}/IsEnabled``` 

| Όνομα| Τύπος δεδομένων|Απαιτείται|Που βρίσκεται στο|Προεπιλεγμένη τιμή|Περιγραφή|
| ---|---|---|---|---|---|
|μισθωτή|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το όνομα του μισθωτή για τον κατάλογο του χρήστη είναι μέρος|


#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|400|BadRequest|
|401|Μη εξουσιοδοτημένο|
|404|Δεν βρέθηκε|
|500|Εσωτερικό σφάλμα διακομιστή|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|



### <a name="get-all-viewable-channels"></a>Λήψη όλων των καναλιών ορατό 
Λαμβάνει όλα τα κανάλια που ο χρήστης έχει πρόσβαση σε προβολή.  
```GET: /{tenant}/Channels``` 

| Όνομα| Τύπος δεδομένων|Απαιτείται|Που βρίσκεται στο|Προεπιλεγμένη τιμή|Περιγραφή|
| ---|---|---|---|---|---|
|μισθωτή|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το όνομα του μισθωτή για τον κατάλογο του χρήστη είναι μέρος|


#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|400|BadRequest|
|401|Μη εξουσιοδοτημένο|
|404|Δεν βρέθηκε|
|500|Εσωτερικό σφάλμα διακομιστή|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Παραθέτει σε λίστα όλα τα βίντεο Office 365 που είναι παρόντες σε ένα κανάλι 
Παραθέτει σε λίστα όλα τα βίντεο Office 365 που είναι παρόντες σε ένα κανάλι.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| Όνομα| Τύπος δεδομένων|Απαιτείται|Που βρίσκεται στο|Προεπιλεγμένη τιμή|Περιγραφή|
| ---|---|---|---|---|---|
|μισθωτή|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το όνομα του μισθωτή για τον κατάλογο του χρήστη είναι μέρος|
|channelId|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το αναγνωριστικό καναλιού από το οποίο πρέπει η λήψη βίντεο|


#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|400|BadRequest|
|401|Μη εξουσιοδοτημένο|
|404|Δεν βρέθηκε|
|500|Εσωτερικό σφάλμα διακομιστή|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|




### <a name="gets-information-about-a-particular-office365-video"></a>Λαμβάνει πληροφορίες σχετικά με μια συγκεκριμένη Office 365 βίντεο 
Λαμβάνει πληροφορίες σχετικά με μια συγκεκριμένη office365 βίντεο.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| Όνομα| Τύπος δεδομένων|Απαιτείται|Που βρίσκεται στο|Προεπιλεγμένη τιμή|Περιγραφή|
| ---|---|---|---|---|---|
|μισθωτή|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το όνομα του μισθωτή για τον κατάλογο του χρήστη είναι μέρος|
|channelId|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το αναγνωριστικό καναλιού|
|videoId|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το αναγνωριστικό του βίντεο|


#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|400|BadRequest|
|401|Μη εξουσιοδοτημένο|
|404|Δεν βρέθηκε|
|500|Εσωτερικό σφάλμα διακομιστή|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Λήψη διεύθυνσης url αναπαραγωγή των υπηρεσιών Azure Media Services δηλωτικό για βίντεο 
Λήψη διεύθυνσης url αναπαραγωγή των υπηρεσιών Azure Media Services δηλωτικό για ένα βίντεο.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| Όνομα| Τύπος δεδομένων|Απαιτείται|Που βρίσκεται στο|Προεπιλεγμένη τιμή|Περιγραφή|
| ---|---|---|---|---|---|
|μισθωτή|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το όνομα του μισθωτή για τον κατάλογο του χρήστη είναι μέρος|
|channelId|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το αναγνωριστικό καναλιού|
|videoId|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το αναγνωριστικό του βίντεο|
|streamingFormatType|συμβολοσειρά|Ναι|ερώτημα|Κανένας|Ροή μορφή τύπος. 1 - ομαλής ροής ή MPEG-ΠΑΎΛΑΣ. 0 - HLS ροής|


#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|400|BadRequest|
|401|Μη εξουσιοδοτημένο|
|404|Δεν βρέθηκε|
|500|Εσωτερικό σφάλμα διακομιστή|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>Λάβετε το διακριτικό φορέα για να αποκτήσετε πρόσβαση σε αποκρυπτογράφηση του βίντεο 
Λάβετε το διακριτικό φορέα για να αποκτήσετε πρόσβαση σε αποκρυπτογράφηση του βίντεο.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Όνομα| Τύπος δεδομένων|Απαιτείται|Που βρίσκεται στο|Προεπιλεγμένη τιμή|Περιγραφή|
| ---|---|---|---|---|---|
|μισθωτή|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το όνομα του μισθωτή για τον κατάλογο του χρήστη είναι μέρος|
|channelId|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το αναγνωριστικό καναλιού|
|videoId|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Το αναγνωριστικό του βίντεο|


#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|400|BadRequest|
|401|Μη εξουσιοδοτημένο|
|404|Δεν βρέθηκε|
|500|Εσωτερικό σφάλμα διακομιστή|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|


## <a name="object-definitions"></a>Ορισμούς αντικειμένων

#### <a name="channel-channel-class"></a>Κανάλι: Κλάση καναλιού

| Όνομα | Τύπος δεδομένων | Απαιτείται|
|---|---|---|
|Αναγνωριστικό|συμβολοσειρά|Όχι|
|Τίτλος|συμβολοσειρά|Όχι|
|Περιγραφή|συμβολοσειρά|Όχι|


#### <a name="video"></a>Βίντεο 

| Όνομα | Τύπος δεδομένων |Απαιτείται|
|---|---|---|
|Αναγνωριστικό|συμβολοσειρά|Όχι|
|Τίτλος|συμβολοσειρά|Όχι|
|Περιγραφή|συμβολοσειρά|Όχι|
|CreationDate|συμβολοσειρά|Όχι|
|Κάτοχος|συμβολοσειρά|Όχι|
|ThumbnailUrl|συμβολοσειρά|Όχι|
|VideoUrl|συμβολοσειρά|Όχι|
|VideoDuration|Ακέραιος αριθμός|Όχι|
|VideoProcessingStatus|Ακέραιος αριθμός|Όχι|
|ViewCount|Ακέραιος αριθμός|Όχι|


## <a name="next-steps"></a>Επόμενα βήματα
[Δημιουργία μιας εφαρμογής λογικής](../app-service-logic/app-service-logic-create-a-logic-app.md).