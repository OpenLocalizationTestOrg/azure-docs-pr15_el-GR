<properties
pageTitle="Μάθετε πώς να χρησιμοποιείτε τη γραμμή σύνδεσης FTP σε εφαρμογές της λογικής | Microsoft Azure"
description="Δημιουργήστε εφαρμογές λογικής με Azure εφαρμογής υπηρεσίας. Συνδεθείτε στο διακομιστή FTP για να διαχειριστείτε τα αρχεία σας. Μπορείτε να εκτελέσετε διάφορες ενέργειες όπως η αποστολή ενημέρωση, λήψη και διαγραφή αρχείων σε διακομιστή FTP."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/22/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-ftp-connector"></a>Γρήγορα αποτελέσματα με τη γραμμή σύνδεσης FTP

Χρησιμοποιήστε τη γραμμή σύνδεσης FTP για να παρακολουθούν και να δημιουργήσετε αρχεία σε ένα διακομιστή FTP. 

Για να χρησιμοποιήσετε [οποιαδήποτε γραμμή σύνδεσης](./apis-list.md), πρέπει πρώτα να δημιουργήσετε μια εφαρμογή λογικής. Μπορείτε να ξεκινήσετε, [δημιουργώντας μια εφαρμογή της λογικής τώρα](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-ftp"></a>Σύνδεση με FTP

Πριν από την εφαρμογή της λογικής να αποκτήσετε πρόσβαση σε οποιαδήποτε υπηρεσία, πρέπει πρώτα να δημιουργήσετε μια *σύνδεση* στην υπηρεσία. [Σύνδεση](./connectors-overview.md) παρέχει σύνδεση ανάμεσα σε μια εφαρμογή λογικής και μια άλλη υπηρεσία.  

### <a name="create-a-connection-to-ftp"></a>Δημιουργία σύνδεσης με FTP

>[AZURE.INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]

## <a name="use-a-ftp-trigger"></a>Χρησιμοποιήστε ένα έναυσμα FTP

Ένα έναυσμα είναι ένα συμβάν που μπορούν να χρησιμοποιηθούν για να ξεκινήσετε τη ροή εργασίας που ορίζονται από το σε μια εφαρμογή για λογική. [Μάθετε περισσότερα σχετικά με τα εναύσματα](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.IMPORTANT]Η γραμμή σύνδεσης FTP απαιτεί ένα διακομιστή FTP που είναι προσβάσιμο από το Internet και έχει ρυθμιστεί ώστε να λειτουργεί με ΠΑΘΗΤΙΚΈΣ λειτουργία. Επίσης, η σύνδεση FTP είναι **δεν είναι συμβατό με την μη ρητή FTPS (FTP μέσω SSL)**. Η γραμμή σύνδεσης FTP υποστηρίζει μόνο ρητή FTPS (FTP μέσω SSL).  

Σε αυτό το παράδειγμα, που θα σας δείξουν πώς μπορείτε να χρησιμοποιήσετε το έναυσμα **FTP - όταν προστίθεται ένα αρχείο ή τροποποίηση** για να ξεκινήσετε μια ροή εργασίας εφαρμογή λογικής όταν προστίθεται ένα αρχείο ή τροποποιηθεί σε ένα διακομιστή FTP. Στο παράδειγμα για μεγάλες επιχειρήσεις, μπορείτε να χρησιμοποιήσετε αυτό το έναυσμα για την παρακολούθηση ενός φακέλου FTP για νέα αρχεία που αναπαριστούν παραγγελίες από πελάτες.  Στη συνέχεια, μπορείτε να χρησιμοποιήσετε μια ενέργεια σύνδεσης FTP όπως **λήψη περιεχομένου αρχείου** για να λάβετε τα περιεχόμενα της εντολής για περαιτέρω επεξεργασία και αποθήκευση στη βάση δεδομένων σας παραγγελίες.

1. Εισαγάγετε *ftp* στο πλαίσιο αναζήτησης σε τη σχεδίαση εφαρμογών λογικής και, στη συνέχεια, επιλέξτε το έναυσμα **FTP - όταν προστίθεται ένα αρχείο ή τροποποίησης**   
![Εικόνα έναυσμα FTP 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
Ανοίγει το στοιχείο ελέγχου **κατά την προσθήκη ή την τροποποίηση ενός αρχείου**  
![Εικόνα έναυσμα FTP 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
- Επιλέξτε το **...** που βρίσκεται στη δεξιά πλευρά του στοιχείου ελέγχου. Έτσι ανοίγει το στοιχείο ελέγχου επιλογής φακέλου  
![Εικόνα έναυσμα FTP 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
- Επιλέξτε το **>** (δεξιό βέλος) και αναζήτηση για να βρείτε το φάκελο που θέλετε να παρακολουθείτε για νέα ή τροποποιημένα αρχεία. Επιλέξτε το φάκελο και παρατηρήστε το φάκελο εμφανίζεται τώρα στο **φάκελο** στοιχείο ελέγχου.  
![Εικόνα έναυσμα FTP 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   


Σε αυτό το σημείο, την εφαρμογή λογικής σας έχει ρυθμιστεί με ένα έναυσμα που θα ξεκινήσει μια εκτέλεση των άλλων εναύσματα και ενέργειες της ροής εργασίας, όταν ένα αρχείο είναι τροποποίησης ή δημιουργηθεί σε συγκεκριμένο φάκελο FTP. 

>[AZURE.NOTE]Για μια εφαρμογή λογικής να λειτουργεί, πρέπει να περιέχει τουλάχιστον ένα έναυσμα και μια ενέργεια. Ακολουθήστε τα βήματα στην επόμενη ενότητα για να προσθέσετε μια ενέργεια.  



## <a name="use-a-ftp-action"></a>Χρησιμοποιήστε μια ενέργεια FTP

Μια ενέργεια είναι μια ενέργεια που εκτελείται από τη ροή εργασίας που ορίζονται από το σε μια εφαρμογή για λογική. [Μάθετε περισσότερα σχετικά με τις ενέργειες](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

Τώρα που έχετε προσθέσει ένα έναυσμα, ακολουθήστε τα παρακάτω βήματα για να προσθέσετε μια ενέργεια που θα σας βοηθήσουν τα περιεχόμενα του αρχείου νέα ή τροποποιημένα βρέθηκε από το έναυσμα.    

1. Επιλέξτε **+ νέο βήμα** για να προσθέσετε το την ενέργεια για να λάβετε τα περιεχόμενα του αρχείου στο διακομιστή FTP  
- Επιλέξτε τη σύνδεση **Προσθήκη μιας ενέργειας** .  
![Εικόνα ενέργεια FTP 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
- Εισαγάγετε *FTP* για να πραγματοποιήσετε αναζήτηση για όλες τις ενέργειες που σχετίζονται με FTP.
- Επιλέξτε την ενέργεια που εκτελείται όταν μια νέα ή τροποποιημένα το αρχείο βρίσκεται στο φάκελο FTP **FTP - λήψη περιεχομένου αρχείου** .      
![Εικόνα ενέργεια FTP 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
Ανοίγει το στοιχείο ελέγχου **γρήγορα το περιεχόμενο του αρχείου** . **Σημείωση**: θα σας ζητηθεί να εγκρίνετε την εφαρμογή σας λογικής για να αποκτήσετε πρόσβαση στο λογαριασμό διακομιστή FTP, εάν δεν έχετε κάνει ήδη.  
![Εικόνα ενέργεια FTP 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
- Επιλέξτε το στοιχείο ελέγχου του **αρχείου** (το κενό διάστημα που βρίσκεται κάτω από το **ΑΡΧΕΊΟ***). Εδώ, μπορείτε να χρησιμοποιήσετε οποιαδήποτε από τις διάφορες ιδιότητες από το αρχείο νέα ή τροποποιημένα βρεθεί στο διακομιστή FTP.  
- Ενεργοποιήστε την επιλογή **το περιεχόμενο του αρχείου** .  
![Εικόνα ενέργεια FTP 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
-  Το στοιχείο ελέγχου είναι ενημερωθεί, που υποδεικνύει ότι η ενέργεια **FTP - λήψη περιεχομένου αρχείου** θα εμφανιστεί το *αρχείο περιεχομένου* για το νέο ή έχουν τροποποιηθεί στο διακομιστή FTP.      
![Εικόνα ενέργεια FTP 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
- Αποθηκεύστε την εργασία σας και, στη συνέχεια, προσθέστε ένα αρχείο στο φάκελο FTP για να δοκιμάσετε τη ροή εργασίας.    

Σε αυτό το σημείο, η εφαρμογή λογικής έχει ρυθμιστεί με ένα έναυσμα για την παρακολούθηση ενός φακέλου σε ένα διακομιστή FTP και να ξεκινήσετε τη ροή εργασίας όταν βρει ένα νέο αρχείο ή ένα αρχείο που έχουν τροποποιηθεί στο διακομιστή FTP. 

Η εφαρμογή λογικής επίσης έχει ρυθμιστεί με μια ενέργεια για να λάβετε τα περιεχόμενα του αρχείου νέα ή έχουν τροποποιηθεί.

Τώρα, μπορείτε να προσθέσετε μια άλλη ενέργεια όπως η ενέργεια του [SQL Server - Εισαγωγή γραμμής](./connectors-create-api-sqlazure.md#insert-row) για να εισαγάγετε τα περιεχόμενα του αρχείου νέα ή τροποποιημένα σε έναν πίνακα βάσης δεδομένων SQL.  

## <a name="technical-details"></a>Τεχνικές λεπτομέρειες

Εδώ θα βρείτε τις λεπτομέρειες σχετικά με τα εναύσματα, ενέργειες και απαντήσεων που υποστηρίζει αυτήν τη σύνδεση:

## <a name="ftp-triggers"></a>FTP εναυσμάτων

FTP περιλαμβάνει τα παρακάτω trigger(s):  

|Έναυσμα | Περιγραφή|
|--- | ---|
|[Όταν προστίθεται ή τροποποίηση ενός αρχείου](connectors-create-api-ftp.md#when-a-file-is-added-or-modified)|Αυτή η λειτουργία εκκινεί μια ροή όταν προστίθεται ένα αρχείο ή τροποποίηση σε ένα φάκελο.|


## <a name="ftp-actions"></a>Ενέργειες FTP

FTP περιλαμβάνει τις ακόλουθες ενέργειες:


|Ενέργεια|Περιγραφή|
|--- | ---|
|[Λήψη αρχείων μετα-δεδομένων](connectors-create-api-ftp.md#get-file-metadata)|Αυτή η λειτουργία λαμβάνει τα μετα-δεδομένα για ένα αρχείο.|
|[Ενημέρωση αρχείων](connectors-create-api-ftp.md#update-file)|Αυτή η λειτουργία ενημερώνει ένα αρχείο.|
|[Διαγραφή αρχείου](connectors-create-api-ftp.md#delete-file)|Αυτή η λειτουργία διαγράφει ένα αρχείο.|
|[Λήψη αρχείων μετα-δεδομένων χρησιμοποιώντας διαδρομή](connectors-create-api-ftp.md#get-file-metadata-using-path)|Αυτή η λειτουργία λαμβάνει τα μετα-δεδομένα από ένα αρχείο χρησιμοποιώντας τη διαδρομή.|
|[Λάβετε το περιεχόμενο του αρχείου χρησιμοποιώντας διαδρομή](connectors-create-api-ftp.md#get-file-content-using-path)|Αυτή η λειτουργία λαμβάνει το περιεχόμενο χρησιμοποιώντας τη διαδρομή αρχείου.|
|[Λήψη περιεχομένου αρχείου](connectors-create-api-ftp.md#get-file-content)|Αυτή η λειτουργία λαμβάνει το περιεχόμενο του αρχείου.|
|[Δημιουργία αρχείου](connectors-create-api-ftp.md#create-file)|Αυτή η λειτουργία δημιουργεί ένα αρχείο.|
|[Αντιγραφή αρχείου](connectors-create-api-ftp.md#copy-file)|Αυτή η λειτουργία αντιγράφει ένα αρχείο σε ένα διακομιστή FTP.|
|[Λίστα αρχείων σε φάκελο](connectors-create-api-ftp.md#list-files-in-folder)|Αυτή η λειτουργία αποκτά τη λίστα των αρχείων και των υποφακέλων σε ένα φάκελο.|
|[Λίστα των αρχείων στον ριζικό φάκελο](connectors-create-api-ftp.md#list-files-in-root-folder)|Αυτή η λειτουργία αποκτά τη λίστα των αρχείων και των υποφακέλων στον ριζικό φάκελο.|
|[Εξαγωγή φακέλου](connectors-create-api-ftp.md#extract-folder)|Αυτή η λειτουργία εξάγει ένα αρχείο αρχειοθέτησης σε ένα φάκελο (παράδειγμα: .zip).|
### <a name="action-details"></a>Λεπτομέρειες ενέργειας

Εδώ θα βρείτε τις λεπτομέρειες για τις ενέργειες και εναύσματα για αυτή η γραμμή σύνδεσης, μαζί με τις απαντήσεις τους:



### <a name="get-file-metadata"></a>Λήψη αρχείων μετα-δεδομένων
Αυτή η λειτουργία λαμβάνει τα μετα-δεδομένα για ένα αρχείο. 


|Όνομα ιδιότητας| Εμφανιζόμενο όνομα|Περιγραφή|
| ---|---|---|
|αναγνωριστικό *|Αρχείο|Επιλέξτε ένα αρχείο|

Μια * υποδεικνύει ότι απαιτείται μια ιδιότητα

#### <a name="output-details"></a>Λεπτομέρειες εξόδου

BlobMetadata


| Όνομα ιδιότητας | Τύπος δεδομένων |
|---|---|---|
|Αναγνωριστικό|συμβολοσειρά|
|Όνομα|συμβολοσειρά|
|Εμφανιζόμενο όνομα|συμβολοσειρά|
|Διαδρομή|συμβολοσειρά|
|Τελευταία τροποποίηση|συμβολοσειρά|
|Μέγεθος|Ακέραιος αριθμός|
|MediaType|συμβολοσειρά|
|IsFolder|δυαδική τιμή|
|ETag|συμβολοσειρά|
|FileLocator|συμβολοσειρά|




### <a name="update-file"></a>Ενημέρωση αρχείων
Αυτή η λειτουργία ενημερώνει ένα αρχείο. 


|Όνομα ιδιότητας| Εμφανιζόμενο όνομα|Περιγραφή|
| ---|---|---|
|αναγνωριστικό *|Αρχείο|Επιλέξτε ένα αρχείο|
|σώμα *|Το περιεχόμενο του αρχείου|Περιεχόμενο του αρχείου|

Μια * υποδεικνύει ότι απαιτείται μια ιδιότητα

#### <a name="output-details"></a>Λεπτομέρειες εξόδου

BlobMetadata


| Όνομα ιδιότητας | Τύπος δεδομένων |
|---|---|---|
|Αναγνωριστικό|συμβολοσειρά|
|Όνομα|συμβολοσειρά|
|Εμφανιζόμενο όνομα|συμβολοσειρά|
|Διαδρομή|συμβολοσειρά|
|Τελευταία τροποποίηση|συμβολοσειρά|
|Μέγεθος|Ακέραιος αριθμός|
|MediaType|συμβολοσειρά|
|IsFolder|δυαδική τιμή|
|ETag|συμβολοσειρά|
|FileLocator|συμβολοσειρά|




### <a name="delete-file"></a>Διαγραφή αρχείου
Αυτή η λειτουργία διαγράφει ένα αρχείο. 


|Όνομα ιδιότητας| Εμφανιζόμενο όνομα|Περιγραφή|
| ---|---|---|
|αναγνωριστικό *|Αρχείο|Επιλέξτε ένα αρχείο|

Μια * υποδεικνύει ότι απαιτείται μια ιδιότητα




### <a name="get-file-metadata-using-path"></a>Λήψη αρχείων μετα-δεδομένων χρησιμοποιώντας διαδρομή
Αυτή η λειτουργία λαμβάνει τα μετα-δεδομένα από ένα αρχείο χρησιμοποιώντας τη διαδρομή. 


|Όνομα ιδιότητας| Εμφανιζόμενο όνομα|Περιγραφή|
| ---|---|---|
|διαδρομή *|Διαδρομή του αρχείου|Επιλέξτε ένα αρχείο|

Μια * υποδεικνύει ότι απαιτείται μια ιδιότητα

#### <a name="output-details"></a>Λεπτομέρειες εξόδου

BlobMetadata


| Όνομα ιδιότητας | Τύπος δεδομένων |
|---|---|---|
|Αναγνωριστικό|συμβολοσειρά|
|Όνομα|συμβολοσειρά|
|Εμφανιζόμενο όνομα|συμβολοσειρά|
|Διαδρομή|συμβολοσειρά|
|Τελευταία τροποποίηση|συμβολοσειρά|
|Μέγεθος|Ακέραιος αριθμός|
|MediaType|συμβολοσειρά|
|IsFolder|δυαδική τιμή|
|ETag|συμβολοσειρά|
|FileLocator|συμβολοσειρά|




### <a name="get-file-content-using-path"></a>Λάβετε το περιεχόμενο του αρχείου χρησιμοποιώντας διαδρομή
Αυτή η λειτουργία λαμβάνει το περιεχόμενο χρησιμοποιώντας τη διαδρομή αρχείου. 


|Όνομα ιδιότητας| Εμφανιζόμενο όνομα|Περιγραφή|
| ---|---|---|
|διαδρομή *|Διαδρομή του αρχείου|Επιλέξτε ένα αρχείο|

Μια * υποδεικνύει ότι απαιτείται μια ιδιότητα




### <a name="get-file-content"></a>Λήψη περιεχομένου αρχείου
Αυτή η λειτουργία λαμβάνει το περιεχόμενο του αρχείου. 


|Όνομα ιδιότητας| Εμφανιζόμενο όνομα|Περιγραφή|
| ---|---|---|
|αναγνωριστικό *|Αρχείο|Επιλέξτε ένα αρχείο|

Μια * υποδεικνύει ότι απαιτείται μια ιδιότητα




### <a name="create-file"></a>Δημιουργία αρχείου
Αυτή η λειτουργία δημιουργεί ένα αρχείο. 


|Όνομα ιδιότητας| Εμφανιζόμενο όνομα|Περιγραφή|
| ---|---|---|
|folderPath *|Η διαδρομή του φακέλου|Επιλέξτε ένα φάκελο|
|όνομα *|Όνομα αρχείου|Το όνομα του αρχείου|
|σώμα *|Το περιεχόμενο του αρχείου|Περιεχόμενο του αρχείου|

Μια * υποδεικνύει ότι απαιτείται μια ιδιότητα

#### <a name="output-details"></a>Λεπτομέρειες εξόδου

BlobMetadata


| Όνομα ιδιότητας | Τύπος δεδομένων |
|---|---|---|
|Αναγνωριστικό|συμβολοσειρά|
|Όνομα|συμβολοσειρά|
|Εμφανιζόμενο όνομα|συμβολοσειρά|
|Διαδρομή|συμβολοσειρά|
|Τελευταία τροποποίηση|συμβολοσειρά|
|Μέγεθος|Ακέραιος αριθμός|
|MediaType|συμβολοσειρά|
|IsFolder|δυαδική τιμή|
|ETag|συμβολοσειρά|
|FileLocator|συμβολοσειρά|




### <a name="copy-file"></a>Αντιγραφή αρχείου
Αυτή η λειτουργία αντιγράφει ένα αρχείο σε ένα διακομιστή FTP. 


|Όνομα ιδιότητας| Εμφανιζόμενο όνομα|Περιγραφή|
| ---|---|---|
|προέλευση *|Η διεύθυνση url προέλευσης|Διεύθυνση URL για το αρχείο προέλευσης|
|προορισμός *|Η διαδρομή του αρχείου προορισμού|Διαδρομή αρχείου προορισμού, συμπεριλαμβανομένων των όνομα αρχείου προορισμού|
|Αντικατάσταση|Αντικατάσταση;|Αντικαθιστά το αρχείο προορισμού, εάν οριστεί στην τιμή 'true'|

Μια * υποδεικνύει ότι απαιτείται μια ιδιότητα

#### <a name="output-details"></a>Λεπτομέρειες εξόδου

BlobMetadata


| Όνομα ιδιότητας | Τύπος δεδομένων |
|---|---|---|
|Αναγνωριστικό|συμβολοσειρά|
|Όνομα|συμβολοσειρά|
|Εμφανιζόμενο όνομα|συμβολοσειρά|
|Διαδρομή|συμβολοσειρά|
|Τελευταία τροποποίηση|συμβολοσειρά|
|Μέγεθος|Ακέραιος αριθμός|
|MediaType|συμβολοσειρά|
|IsFolder|δυαδική τιμή|
|ETag|συμβολοσειρά|
|FileLocator|συμβολοσειρά|




### <a name="when-a-file-is-added-or-modified"></a>Όταν προστίθεται ή τροποποίηση ενός αρχείου
Αυτή η λειτουργία εκκινεί μια ροή όταν προστίθεται ένα αρχείο ή τροποποίηση σε ένα φάκελο. 


|Όνομα ιδιότητας| Εμφανιζόμενο όνομα|Περιγραφή|
| ---|---|---|
|folderId *|Φάκελος|Επιλέξτε ένα φάκελο|

Μια * υποδεικνύει ότι απαιτείται μια ιδιότητα




### <a name="list-files-in-folder"></a>Λίστα αρχείων σε φάκελο
Αυτή η λειτουργία αποκτά τη λίστα των αρχείων και των υποφακέλων σε ένα φάκελο. 


|Όνομα ιδιότητας| Εμφανιζόμενο όνομα|Περιγραφή|
| ---|---|---|
|αναγνωριστικό *|Φάκελος|Επιλέξτε ένα φάκελο|

Μια * υποδεικνύει ότι απαιτείται μια ιδιότητα



#### <a name="output-details"></a>Λεπτομέρειες εξόδου

BlobMetadata


| Όνομα ιδιότητας | Τύπος δεδομένων |
|---|---|---|
|Αναγνωριστικό|συμβολοσειρά|
|Όνομα|συμβολοσειρά|
|Εμφανιζόμενο όνομα|συμβολοσειρά|
|Διαδρομή|συμβολοσειρά|
|Τελευταία τροποποίηση|συμβολοσειρά|
|Μέγεθος|Ακέραιος αριθμός|
|MediaType|συμβολοσειρά|
|IsFolder|δυαδική τιμή|
|ETag|συμβολοσειρά|
|FileLocator|συμβολοσειρά|




### <a name="list-files-in-root-folder"></a>Λίστα των αρχείων στον ριζικό φάκελο
Αυτή η λειτουργία αποκτά τη λίστα των αρχείων και των υποφακέλων στον ριζικό φάκελο. 


Δεν υπάρχουν παράμετροι για αυτήν την κλήση

#### <a name="output-details"></a>Λεπτομέρειες εξόδου

BlobMetadata


| Όνομα ιδιότητας | Τύπος δεδομένων |
|---|---|---|
|Αναγνωριστικό|συμβολοσειρά|
|Όνομα|συμβολοσειρά|
|Εμφανιζόμενο όνομα|συμβολοσειρά|
|Διαδρομή|συμβολοσειρά|
|Τελευταία τροποποίηση|συμβολοσειρά|
|Μέγεθος|Ακέραιος αριθμός|
|MediaType|συμβολοσειρά|
|IsFolder|δυαδική τιμή|
|ETag|συμβολοσειρά|
|FileLocator|συμβολοσειρά|




### <a name="extract-folder"></a>Εξαγωγή φακέλου
Αυτή η λειτουργία εξάγει ένα αρχείο αρχειοθέτησης σε ένα φάκελο (παράδειγμα: .zip). 


|Όνομα ιδιότητας| Εμφανιζόμενο όνομα|Περιγραφή|
| ---|---|---|
|προέλευση *|Διαδρομή αρχείου προέλευσης αρχειοθέτησης|Διαδρομή προς το αρχείο αρχειοθέτησης|
|προορισμός *|Διαδρομή του φακέλου προορισμού|Διαδρομή προς το φάκελο προορισμού|
|Αντικατάσταση|Αντικατάσταση;|Αντικαθιστά τα αρχεία προορισμού, εάν οριστεί στην τιμή 'true'|

Μια * υποδεικνύει ότι απαιτείται μια ιδιότητα



#### <a name="output-details"></a>Λεπτομέρειες εξόδου

BlobMetadata


| Όνομα ιδιότητας | Τύπος δεδομένων |
|---|---|---|
|Αναγνωριστικό|συμβολοσειρά|
|Όνομα|συμβολοσειρά|
|Εμφανιζόμενο όνομα|συμβολοσειρά|
|Διαδρομή|συμβολοσειρά|
|Τελευταία τροποποίηση|συμβολοσειρά|
|Μέγεθος|Ακέραιος αριθμός|
|MediaType|συμβολοσειρά|
|IsFolder|δυαδική τιμή|
|ETag|συμβολοσειρά|
|FileLocator|συμβολοσειρά|



## <a name="http-responses"></a>HTTP απαντήσεων

Ενεργειών και των παραπάνω εναύσματα μπορεί να επιστρέψει ένα ή περισσότερα από τους ακόλουθους κωδικούς κατάστασης HTTP: 

|Όνομα|Περιγραφή|
|---|---|
|200|Ok|
|202|Αποδοχή|
|400|Ακατάλληλη αίτηση|
|401|Μη εξουσιοδοτημένο|
|403|Δεν επιτρέπεται|
|404|Δεν βρέθηκε|
|500|Εσωτερικό σφάλμα διακομιστή. Άγνωστο σφάλμα.|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|







## <a name="next-steps"></a>Επόμενα βήματα
[Δημιουργία εφαρμογής λογικής](../app-service-logic/app-service-logic-create-a-logic-app.md)