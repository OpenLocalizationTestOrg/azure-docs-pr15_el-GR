<properties
   pageTitle="Χρησιμοποιώντας την επιλογή ακρόασης HTTP και σύνδεσης σε εφαρμογές της λογικής | Microsoft Azure εφαρμογής υπηρεσίας "
   description="Πώς να δημιουργείτε και να τις παραμέτρους του ακρόασης HTTP και HTTP ενέργεια γραμμής σύνδεσης ή API εφαρμογή και να το χρησιμοποιήσετε σε μια εφαρμογή λογικής στο Azure εφαρμογής υπηρεσίας"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/31/2016"
   ms.author="prkumar"/>


# <a name="get-started-with-the-http-listener-and-http-action-and-add-it-to-your-logic-app"></a>Γρήγορα αποτελέσματα με το ακρόασης HTTP και ενέργεια HTTP και προσθήκη της σε εφαρμογή της λογικής

> [AZURE.NOTE]Υποστήριξη για αυτή η γραμμή σύνδεσης θα σας που τελειώνει επειδή τη λειτουργικότητα τώρα περιλαμβάνονται από προεπιλογή ως η **μη αυτόματη έναυσμα** όταν δημιουργείτε νέες εφαρμογές λογικής.  Συνιστάται να κάνετε αναβάθμιση όλες τις εφαρμογές λογικής που χρησιμοποιούν αυτή η γραμμή σύνδεσης.  
> Αυτή η έκδοση του άρθρου ισχύει για λογική εφαρμογές 2014-12-01-προεπισκόπηση σχήματος έκδοση.

Σύνδεση απευθείας σε πόρους HTTP για να ακούσετε για αιτήσεις HTTP και ρύθμιση παραμέτρων αιτήσεων web HTTP. Υπάρχουν ορισμένα σενάρια όπου ίσως χρειαστεί να εργαστείτε απευθείας συνδέσεις HTTP, όπως:

1.  Για να αναπτύξετε μια λογική εφαρμογής που υποστηρίζει ένα web ή την κινητή χρηστών αλληλεπιδραστικό προσκηνίου.
2.  Για να λάβετε και επεξεργάζεται δεδομένα από μια υπηρεσία web που δεν έχει μια εκτός σύνδεσης πλαισίου.
3.  Για να εκτελέσετε ενέργειες που βρίσκονται ήδη εκτεθειμένης ως υπηρεσία web, αλλά δεν είναι διαθέσιμη ως εφαρμογής API.

Για αυτά τα σενάρια, υπάρχουν δύο επιλογές:

1. **Ακρόασης HTTP**: Αυτή η γραμμή σύνδεσης λειτουργεί ως ένα έναυσμα και παρακολουθεί για αιτήσεις HTTP σε ένα τελικό σημείο έχει ρυθμιστεί. Κατά τη λήψη μιας κλήσης σε το τελικό σημείο έχει ρυθμιστεί, ενεργοποιεί μια νέα παρουσία της ροής της και μεταβιβάζει τα δεδομένα που έχουν ληφθεί σε την αίτηση για τη ροή για επεξεργασία. Μπορεί επίσης να ρυθμιστεί για αυτόματη απάντηση στην πρόσκληση σε εισερχόμενα όταν η ροή περιλαμβάνει αποτελέσματα ή σάς επιτρέπουν να δημιουργήσετε μια απόκριση με βάση την εκτέλεση ροής και αποστολή απάντησης σε τον καλούντα.
2. **Η ενέργεια HTTP**: Αυτό σας επιτρέπει να ρυθμίσετε τις παραμέτρους μιας αίτησης web σε οποιαδήποτε διαθέσιμη το τελικό σημείο στο internet, πίσω λαμβάνει μια απάντηση και κάνει διαθέσιμο για πρόσθετες ενέργειες της ροής για την εκμετάλλευση.

Μπορεί να προκαλέσει την λογική εφαρμογών που βασίζονται σε μια ποικιλία προελεύσεων δεδομένων και των γραμμών σύνδεσης προσφορά για τη λήψη και επεξεργασίας δεδομένων ως μέρος της ροής. Μπορείτε να προσθέσετε τη γραμμή σύνδεσης HTTP για να τα επαγγελματικά δεδομένα ροής εργασίας και διαδικασία ως μέρος αυτής της ροής εργασίας μέσα σε μια εφαρμογή λογικής. 

## <a name="creating-an-http-listener-for-your-logic-app"></a>Δημιουργία μιας ακρόασης HTTP για την εφαρμογή σας λογικής
Μια γραμμή σύνδεσης μπορεί να δημιουργηθεί μια εφαρμογή λογικής ή να δημιουργηθεί απευθείας από το Azure Marketplace. Για να δημιουργήσετε μια γραμμή σύνδεσης από το Marketplace:  

1. Στο το Azure startboard, επιλέξτε **Marketplace**.
2. Αναζήτηση για "HTTP", επιλέξτε ακρόασης HTTP και επιλέξτε **Δημιουργία**.
3.  Ρυθμίστε τις παραμέτρους της ακρόασης HTTP ως εξής:  
![][1]

4.  Όταν ρυθμίζετε τις ρυθμίσεις πακέτου, θα δείτε την παρακάτω επιλογή αν το ακροατήριο θα πρέπει να απαντήσετε αυτόματα ή απαιτούν να στέλνει μια ρητή απάντηση. Ορίστε αυτή σε **False** για να στείλετε τη δική σας απάντηση:  
![][2]

5.  Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε.
6.  Όταν δημιουργηθεί η παρουσία εφαρμογής API, ανοίξτε τις ρυθμίσεις για να ρυθμίσετε τις παραμέτρους της ασφάλειας. Προς το παρόν, της ακρόασης HTTP υποστηρίζει βασικό έλεγχο ταυτότητας. Μπορείτε να το ρυθμίσετε χρησιμοποιώντας την επιλογή ασφαλείας κατά το άνοιγμα της ακρόασης HTTP:  
![][3]
  
    **Γνωστό θέμα** Ρυθμίσεις *η ασφάλεια Εμφάνιση "Κανένα" ως την προεπιλεγμένη τιμή, ωστόσο αυτό δεν έχει οριστεί. Πρέπει να αλλάξετε τη ρύθμιση για να Basic και επιστροφή σε κανένα πριν να αποθηκεύσετε την για να βεβαιωθείτε ότι το πρόγραμμα ακρόασης HTTP έχει ρυθμιστεί σωστά.*  

7. Τέλος, ορίστε τις ρυθμίσεις ασφαλείας της εφαρμογής API κοινό (ανώνυμη) για να επιτρέψετε σε εξωτερικά προγράμματα-πελάτες για να αποκτήσετε πρόσβαση το τελικό σημείο. Αυτή η ρύθμιση είναι διαθέσιμη στην περιοχή "όλες οι ρυθμίσεις > Ρυθμίσεις εφαρμογής" της εφαρμογής API ακρόασης HTTP:![][10]

Μόλις γίνει αυτό, μπορείτε πλέον να δημιουργήσετε μια εφαρμογή λογικής για να χρησιμοποιήσετε τη λειτουργία ακρόασης HTTP.

## <a name="using-the-http-listener-in-your-logic-app"></a>Χρήση της ακρόασης HTTP σε εφαρμογή της λογικής
Μετά τη δημιουργία της εφαρμογής σας API, μπορείτε να χρησιμοποιήσετε τώρα της ακρόασης HTTP ως έναυσμα για την εφαρμογή σας λογική. Για να το κάνετε αυτό, πρέπει να:

4.  Δημιουργήστε μια νέα εφαρμογή λογικής.
5.  Ανοίξτε το "Εναύσματα και ενέργειες" για να ανοίξετε το εργαλείο σχεδίασης εφαρμογές λογικής και ρύθμιση παραμέτρων ροής σας. Της ακρόασης HTTP παρατίθεται στη συλλογή. Επιλέξτε την.
6.  Τώρα, μπορείτε να ορίσετε τη μέθοδο HTTP και τη σχετική διεύθυνση URL στην οποία απαιτείται το ακροατήριο να ενεργοποιήσετε τη ροή:  
![][4]  
![][5]

7.  Για να δείτε την πλήρη URI, κάντε διπλό κλικ της ακρόασης HTTP για να προβάλετε τις ρυθμίσεις της ρύθμισης παραμέτρων και αντιγράψτε τη διεύθυνση URL για το "κεντρικός υπολογιστής" της εφαρμογής API:  
![][6]
8.  Τώρα, μπορείτε να χρησιμοποιήσετε τα δεδομένα που έχουν ληφθεί σε μια αίτηση HTTP σε άλλες ενέργειες της ροής ως εξής:  
![][7]  
![][8]
9.  Τέλος, για να στείλετε μια απάντηση, προσθέστε μια άλλη ακρόασης HTTP και επιλέξτε την ενέργεια αποστολή απόκρισης HTTP. Ορίστε το Αναγνωριστικό αίτηση για να το RequestID που λαμβάνονται από το ακροατήριο HTTP και συμπληρώσετε το σώμα απόκρισης και την κατάσταση HTTP που θέλετε να επιστρέψετε ξανά:  
![][9]

## <a name="using-the-http-action"></a>Χρησιμοποιώντας την ενέργεια HTTP
Η ενέργεια HTTP υποστηρίζεται εγγενώς από εφαρμογές λογικής και δεν απαιτεί μια εφαρμογή API θα δημιουργηθεί πρώτα να είναι σε θέση να το χρησιμοποιήσετε. Για να εισαγάγετε μια ενέργεια HTTP σε οποιοδήποτε σημείο στην εφαρμογή λογικής και επιλέξτε URI, κεφαλίδες και κυρίως κείμενο για την κλήση.
Η ενέργεια HTTP υποστηρίζει πολλές επιλογές για την ασφάλεια πλευρά του προγράμματος-πελάτη. Δείτε τις [Επιλογές ασφαλείας πλευρά του προγράμματος-πελάτη](../scheduler/scheduler-outbound-authentication.md).

Το αποτέλεσμα της ενέργειας HTTP είναι επικεφαλίδες και σώμα, τα οποία μπορούν να χρησιμοποιηθούν περαιτέρω μετά τη ροή παρόμοια με τον τρόπο καταναλώνεται αποτέλεσμα άλλων ενεργειών και των γραμμών σύνδεσης.

## <a name="do-more-with-your-connector"></a>Κάντε περισσότερα με την γραμμή σύνδεσης
Τώρα που δημιουργείται στη γραμμή σύνδεσης, μπορείτε να την προσθέσετε σε μια ροή εργασίας επιχειρήσεις χρήση λογικής εφαρμογής. Ανατρέξτε στο θέμα [Τι είναι οι εφαρμογές λογικής;](app-service-logic-what-are-logic-apps.md).

Προβάλετε την αναφορά Swagger REST API στο [γραμμών σύνδεσης και API εφαρμογές αναφοράς](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Μπορείτε επίσης να αναθεωρήσετε επιδόσεων στατιστικών στοιχείων και στοιχείο ελέγχου ασφαλείας στη γραμμή σύνδεσης. Ανατρέξτε στο θέμα [Διαχείριση και την παρακολούθηση τις ενσωματωμένες εφαρμογές API και τις γραμμές σύνδεσης](app-service-logic-monitor-your-connectors.md).

> [AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure λογικής εφαρμογών πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [App λογικής δοκιμάσετε](https://tryappservice.azure.com/?appservice=logic). Αμέσως, μπορείτε να δημιουργήσετε μια εφαρμογή της λογικής μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png
