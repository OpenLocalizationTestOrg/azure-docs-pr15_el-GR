<properties
   pageTitle="Χρησιμοποιώντας τη γραμμή σύνδεσης του SQL σε εφαρμογές της λογικής | Microsoft Azure εφαρμογής υπηρεσίας"
   description="Πώς να δημιουργείτε και να ρυθμίσετε τις παραμέτρους της εφαρμογής σύνδεσης SQL ή API και να το χρησιμοποιήσετε σε μια εφαρμογή λογικής στο Azure εφαρμογής υπηρεσίας"
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
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>Γρήγορα αποτελέσματα με τη γραμμή σύνδεσης του Microsoft SQL και προσθήκη της σε εφαρμογή της λογικής
>[AZURE.NOTE] Αυτή η έκδοση του άρθρου ισχύει για λογική εφαρμογές 2014-12-01-προεπισκόπηση σχήματος έκδοση. Για την έκδοση σχήματος Azure SQL 2015-08-01-προεπισκόπηση, κάντε κλικ στην επιλογή [SQL Azure API](../connectors/connectors-create-api-sqlazure.md).

Σύνδεση με μια εσωτερική SQL Server ή μια βάση δεδομένων SQL Azure, για να δημιουργήσετε και να αλλάξετε τις πληροφορίες ή τα δεδομένα. Γραμμές σύνδεσης μπορεί να χρησιμοποιηθεί σε εφαρμογές της λογικής για να ανακτήσετε, διαδικασία, ή push δεδομένων ως μέρος μιας "ροής εργασίας". Όταν χρησιμοποιείτε τη γραμμή σύνδεσης SQL στη ροή εργασίας σας, μπορείτε να επιτύχετε μια ποικιλία σενάρια. Για παράδειγμα, μπορείτε να κάνετε:

- Εκθέτει μια ενότητα των δεδομένων που βρίσκεται στη βάση δεδομένων SQL μέσω web ή εφαρμογή κινητές συσκευές.
- Εισαγωγή δεδομένων σε έναν πίνακα βάσης δεδομένων SQL για το χώρο αποθήκευσης. Για παράδειγμα, μπορείτε να εισαγάγετε τις εγγραφές των υπαλλήλων, να ενημέρωση παραγγελιών πωλήσεων και ούτω καθεξής.
- Λήψη δεδομένων από SQL και χρησιμοποιήστε το σε μια επιχειρηματική διαδικασία. Για παράδειγμα, μπορείτε να λάβετε εγγραφές πελάτη και να τοποθετήσετε αυτές τις εγγραφές πελάτη στο SalesForce.

Μπορείτε να προσθέσετε τη γραμμή σύνδεσης του SQL για τα επαγγελματικά δεδομένα ροής εργασίας και διαδικασία ως μέρος αυτής της ροής εργασίας μέσα σε μια εφαρμογή λογικής. 

## <a name="triggers-and-actions"></a>Εναύσματα και ενέργειες
*Εναύσματα* είναι συμβάντα που πραγματοποιούνται. Για παράδειγμα, όταν ενημερώνεται μια σειρά ή όταν ένας νέος πελάτης προστίθεται. *Η ενέργεια* είναι το αποτέλεσμα του εναύσματος. Για παράδειγμα, όταν ενημερώνεται μια σειρά, στείλτε μια ειδοποίηση για τον πωλητή. Εναλλακτικά, όταν προστίθεται ένα νέο πελάτη, στείλτε ένα μήνυμα ηλεκτρονικού ταχυδρομείου υποδοχής για τον νέο πελάτη.

Η γραμμή σύνδεσης SQL μπορεί να χρησιμοποιηθεί ως ένα έναυσμα ή μια ενέργεια σε μια λογική εφαρμογής και υποστηρίζει δεδομένα σε μορφές JSON και XML. Για κάθε πίνακα που περιλαμβάνεται στο πακέτο σας ρυθμίσεις (περισσότερες πληροφορίες για που αργότερα σε αυτό το θέμα), δεν υπάρχει ένα σύνολο ενεργειών JSON και ένα σύνολο ενεργειών XML.

Η γραμμή σύνδεσης SQL περιλαμβάνει τα παρακάτω εναύσματα και ενέργειες που είναι διαθέσιμες:

Εναυσμάτων | Ενέργειες
--- | ---
Δεδομένα ψηφοφορίας | <ul><li>Εισαγωγή σε πίνακα</li><li>Ενημέρωση πίνακα</li><li>Επιλέξτε από πίνακα</li><li>Διαγραφή από πίνακα</li><li>Κλήση αποθηκευμένη διαδικασία</li></ul>

## <a name="create-the-sql-connector"></a>Δημιουργία σύνδεσης του SQL

Μια γραμμή σύνδεσης μπορεί να δημιουργηθεί μια εφαρμογή λογικής ή να δημιουργηθεί απευθείας από το Azure Marketplace. Για να δημιουργήσετε μια γραμμή σύνδεσης από το Marketplace:  

1. Στο το Azure startboard, επιλέξτε **Marketplace**.
2. Αναζήτηση για "SQL σύνδεσης", επιλέξτε το και επιλέξτε **Δημιουργία**.
3. Πληκτρολογήστε το όνομα της εφαρμογής υπηρεσίας Σχεδιασμός και άλλες ιδιότητες.
4. Εισαγάγετε τις παρακάτω ρυθμίσεις πακέτου:

    Όνομα | Απαιτείται |  Περιγραφή
--- | --- | ---
Το όνομα του διακομιστή | Ναι | Πληκτρολογήστε το όνομα του SQL Server. Για παράδειγμα, πληκτρολογήστε *της/των* ή *SQLserver.mydomain.com*.
Θύρα | Όχι | Η προεπιλογή είναι 1433.
Όνομα χρήστη | Ναι | Πληκτρολογήστε ένα όνομα χρήστη που μπορεί να συνδεθεί σε SQL Server. Συνδεθείτε σε έναν SQL Server εσωτερικής εγκατάστασης, εισαγάγετε τα διαπιστευτήριά τον έλεγχο ταυτότητας SQL.
Κωδικός πρόσβασης | Ναι | Πληκτρολογήστε τον κωδικό πρόσβασης για το όνομα χρήστη.
Όνομα βάσης δεδομένων | Ναι | Εισαγάγετε τη βάση δεδομένων που πρόκειται να συνδεθείτε. Για παράδειγμα, μπορείτε να εισαγάγετε *πελάτες* ή *dbo/παραγγελίες*.
Εσωτερικής εγκατάστασης | Ναι | Η προεπιλογή είναι False. Εισαγάγετε την τιμή False εάν τη σύνδεση σε μια βάση δεδομένων Azure SQL. Εισαγάγετε την τιμή True εάν τη σύνδεση σε έναν SQL Server εσωτερικής εγκατάστασης.
Συμβολοσειρά σύνδεσης Bus υπηρεσίας | Όχι | Εάν συνδέεστε στο εσωτερικής εγκατάστασης, πληκτρολογήστε τη συμβολοσειρά σύνδεσης μεταγωγής Bus υπηρεσίας.<br/><br/>[Χρήση της διαχείρισης σύνδεσης υβριδική](app-service-logic-hybrid-connection-manager.md)<br/>[Λειτουργία Bus τιμολόγησης](https://azure.microsoft.com/pricing/details/service-bus/)
Όνομα διακομιστή του συνεργάτη | Όχι | Εάν το πρωτεύον διακομιστή δεν είναι διαθέσιμη, μπορείτε να εισαγάγετε ένα διακομιστή του συνεργάτη ως εναλλακτική ή δημιουργίας αντιγράφων ασφαλείας διακομιστή.
Πίνακες | Όχι | Λίστα πίνακες της βάσης δεδομένων που μπορεί να ενημερωθεί από τη γραμμή σύνδεσης. Για παράδειγμα, πληκτρολογήστε *OrdersTable* ή *EmployeeTable*. Εάν οι πίνακες δεν εισάγονται, μπορεί να χρησιμοποιηθεί όλους τους πίνακες. Απαιτείται έγκυρη πίνακες ή/και αποθηκευμένες διαδικασίες για τη χρήση αυτή η γραμμή σύνδεσης ως ενέργεια.
Αποθηκευμένες διαδικασίες | Όχι | Εισαγάγετε μια υπάρχουσα αποθηκευμένη διαδικασία που μπορεί να ονομάζεται με τη γραμμή σύνδεσης. Για παράδειγμα, πληκτρολογήστε *sp_IsEmployeeEligible* ή *sp_CalculateOrderDiscount*. Απαιτείται έγκυρη πίνακες ή/και αποθηκευμένες διαδικασίες για τη χρήση αυτή η γραμμή σύνδεσης ως ενέργεια.
Διαθέσιμες ερωτήματος δεδομένων | Για την υποστήριξη εναυσμάτων | Πρόταση SQL για να προσδιορίσετε αν δεδομένα που είναι διαθέσιμα για σταθμοσκόπησης έναν πίνακα βάσης δεδομένων SQL Server. Αυτό θα πρέπει να επιστρέψετε μια αριθμητική τιμή που αντιπροσωπεύει τον αριθμό των γραμμών δεδομένων που είναι διαθέσιμες. Παράδειγμα: ΕΠΙΛΈΞΤΕ οι συναρτήσεις COUNT(*) από TABLE_NAME πρέπει.
Το ερώτημα δεδομένων ψηφοφορίας | Για την υποστήριξη εναυσμάτων | Την πρόταση SQL για να ψηφοφορία με τον πίνακα βάσης δεδομένων SQL Server. Μπορείτε να εισαγάγετε οποιονδήποτε αριθμό προτάσεις SQL, διαχωρισμένα με ελληνικό ερωτηματικό. Αυτή η πρόταση είναι εκτελεστεί ενημερώσετε και δεσμευμένου μόνο όταν τα δεδομένα με ασφάλεια είναι αποθηκευμένα σε εφαρμογή της λογικής. Παράδειγμα: ΕΠΙΛΈΞΤΕ *από TABLE_NAME πρέπει; ΔΙΑΓΡΑΦΉ από TABLE_NAME πρέπει. <br/>**Note**<br/>Πρέπει να δώσετε μια πρόταση ψηφοφορίας που αποτρέπει κατάσταση συνεχούς επανάληψης κατά τη διαγραφή, μετακίνηση ή ενημέρωση των επιλεγμένων δεδομένων για να βεβαιωθείτε ότι τα ίδια δεδομένα δεν είναι απομακρυσμένα ξανά.

5. Όταν ολοκληρωθεί η εγκατάσταση, τις ρυθμίσεις πακέτου είναι παρόμοιο με το εξής:  
![][1]  

6. Επιλέξτε **Δημιουργία**. 


## <a name="use-the-connector-as-a-trigger"></a>Χρησιμοποιήστε τη γραμμή σύνδεσης ως έναυσμα
Ας δούμε μια εφαρμογή απλή λογική που σταθμοσκοπεί δεδομένα από έναν πίνακα SQL, προσθέτει τα δεδομένα σε έναν άλλο πίνακα και ενημερώνει τα δεδομένα.

Για να χρησιμοποιήσετε τη γραμμή σύνδεσης SQL ως έναυσμα, εισαγάγετε τις τιμές **Δεδομένων διαθέσιμη ερωτήματος** και **Το ερώτημα δεδομένων ψηφοφορίας** . **Το ερώτημα δεδομένων διαθέσιμη** εκτελείται στο χρονοδιάγραμμα να εισαγάγετε και να καθορίζει εάν τα δεδομένα είναι διαθέσιμη. Επειδή αυτό το ερώτημα επιστρέφει μόνο μια ανυσματική αριθμό, μπορεί να απενεργοποιήσει και να βελτιστοποιηθεί για συχνές εκτέλεσης.

**Το ερώτημα δεδομένων ψηφοφορίας** εκτελείται μόνο όταν το ερώτημα δεδομένων διαθέσιμη υποδεικνύει ότι τα δεδομένα είναι διαθέσιμη. Αυτή η πρόταση εκτελεί μέσα σε μια συναλλαγή και είναι μόνο δεσμευμένου όταν τα δεδομένα που έχουν εξαχθεί κατάταξή αποθηκεύονται στη ροή εργασίας σας. Είναι σημαντικό για να αποφύγετε πολύ εκ νέου την εξαγωγή των ίδιων δεδομένων. Τη φύση συναλλαγών του εκτέλεσης αυτό μπορεί να χρησιμοποιηθεί για να διαγράψετε ή να ενημερώσετε τα δεδομένα για να βεβαιωθείτε ότι δεν έχουν συλλεχθεί, την επόμενη φορά που απευθύνεται το ερώτημα δεδομένων.

> [AZURE.NOTE] Το σχήμα που επιστρέφεται από αυτήν τη δήλωση προσδιορίζει τις διαθέσιμες ιδιότητες στο την γραμμή σύνδεσης. Πρέπει να ονομάζεται όλες τις στήλες.

#### <a name="data-available-query-example"></a>Παράδειγμα διαθέσιμη ερωτήματος δεδομένων

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>Παράδειγμα ερωτήματος δεδομένων ψηφοφορίας

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>Προσθέστε το έναυσμα
1. Όταν δημιουργείτε ή επεξεργάζεστε μια εφαρμογή λογικής, επιλέξτε τη γραμμή σύνδεσης SQL που δημιουργήσατε ως το έναυσμα. Αυτό εμφανίζει τα διαθέσιμα εναύσματα: **Ψηφοφορίας δεδομένων (JSON)** και **Ψηφοφορία δεδομένων (XML)**:  
![][5]

2. Επιλέξτε το έναυσμα **Ψηφοφορίας δεδομένων (JSON)** , εισαγάγετε τη συχνότητα και επιλέξτε το ✓:  
![][6]

3. Το έναυσμα εμφανίζεται πλέον όπως έχει ρυθμιστεί στην εφαρμογή λογικής. Το output(s) του εναύσματος εμφανίζονται και μπορούν να χρησιμοποιηθούν ως εισροές σε όλες τις επόμενες ενέργειες:  
![][7]

## <a name="use-the-connector-as-an-action"></a>Χρησιμοποιήστε τη γραμμή σύνδεσης με μια ενέργεια
Χρησιμοποιώντας το σενάριο εφαρμογή απλή λογική που σταθμοσκοπεί δεδομένα από έναν πίνακα SQL, προσθέτει τα δεδομένα σε έναν άλλο πίνακα και ενημερώνει τα δεδομένα.

Για να χρησιμοποιήσετε τη γραμμή σύνδεσης SQL ως ενέργεια, πληκτρολογήστε το όνομα του το πίνακες ή/και αποθηκευμένες διαδικασίες που πληκτρολογήσατε όταν δημιουργήσατε τη γραμμή σύνδεσης SQL:

1. Μετά την έναυσμα (ή επιλέξτε 'εκτέλεση αυτή η λογική με μη αυτόματο τρόπο'), προσθέστε τη γραμμή σύνδεσης SQL που δημιουργήθηκε από τη συλλογή. Επιλέξτε μία από τις ενέργειες εισαγωγή, όπως *Εισαγωγή σε TempEmployeeDetails (JSON)*:  
![][8]

2. Εισαγάγετε τις τιμές εισαγωγής από την εγγραφή για να εισαχθεί και κάντε κλικ στην εντολή το ✓:  
![][9]

3. Από τη συλλογή, επιλέξτε την ίδια γραμμή σύνδεσης του SQL που δημιουργήσατε. Ως μια ενέργεια, επιλέξτε την ενέργεια ενημέρωση στον ίδιο πίνακα, όπως *EmployeeDetails ενημερωμένη έκδοση*:  
![][11]

4. Πληκτρολογήστε τις τιμές εισαγωγής για την ενέργεια της ενημέρωσης και κάντε κλικ στην εντολή το ✓:  
![][12]

Μπορείτε να ελέγξετε την εφαρμογή λογικής, προσθέτοντας μια νέα εγγραφή στον πίνακα που είναι αυτή τη στιγμή.

## <a name="what-you-can-and-cannot-do"></a>Τι μπορείτε και τι δεν μπορείτε να κάνετε

Ερώτημα SQL | Υποστηρίζονται | Δεν υποστηρίζεται
--- | --- | ---
Όπου τον όρο FROM | <ul><li>Τελεστές: Και, ή, =, <>, <, < =, >, > = και ΌΠΩΣ</li><li>Πολλές συνθήκες sub μπορεί να συνδυαστεί με '(' και')'</li><li>Συμβολοσειρά λεκτικές σταθερές, ημερομηνίας/ώρας (εσωκλείεται σε μονά εισαγωγικά), αριθμών (πρέπει να περιέχει μόνο αριθμητικούς χαρακτήρες)</li><li>Θα πρέπει να αυστηρά είναι σε μορφή δυαδική παράσταση, όπως ((operand operator operand) ή/και (τελεστέος τελεστή τελεστέος)) *</li></ul> | <ul><li>Τελεστές: Μεταξύ "," σε</li><li>Όλες οι ενσωματωμένες συναρτήσεις, όπως ADD(), MAX() NOW(), POWER() και ούτω καθεξής</li><li>Μαθηματικοί τελεστές, όπως *, -, +, και ούτω καθεξής</li><li>Χρήση αλληλουχίες συμβολοσειρών +.</li><li>Όλες οι σύνδεσμοι</li><li>ΕΊΝΑΙ NULL και δεν ΕΊΝΑΙ Null</li><li>Τους αριθμούς με μη αριθμητικούς χαρακτήρες, όπως οι αριθμοί δεκαεξαδικό αριθμό</li></ul>
Πεδία (στο ερώτημα επιλογής) | <ul><li>Ονόματα στηλών έγκυρη που διαχωρίζονται με κόμματα. Δεν υπάρχει προθέματα όνομα πίνακα επιτρέπεται (η γραμμή σύνδεσης λειτουργεί σε έναν πίνακα κάθε φορά).</li><li>Ονόματα μπορεί να είναι διαφυγής με ' [' και ']'</li></ul> | <ul><li>Λέξεις-κλειδιά, όπως ΕΠΆΝΩ, DISTINCT και ούτω καθεξής</li><li>Εναλλακτικά, όπως οδός + Πόλη + Zip διεύθυνση AS</li><li>Όλα ενσωματωμένες συναρτήσεις, όπως ADD() MAX() NOW(), POWER() και ούτω καθεξής</li><li>Μαθηματικοί τελεστές, όπως *, -, +, και ούτω καθεξής</li><li>Χρήση αλληλουχίες συμβολοσειρών +</li></ul>

#### <a name="tips"></a>Συμβουλές

- Για προχωρημένους ερωτήματα, προτείνουμε να δημιουργείτε αποθηκευμένη διαδικασία και execute χρησιμοποιώντας την εκτέλεση αποθηκευμένη διαδικασία API.
- Όταν χρησιμοποιείτε το εσωτερικό ερωτημάτων, χρησιμοποιήστε τους μέσα σε αποθηκευμένες διαδικασίες.
- Για τη συμμετοχή σε πολλές συνθήκες, μπορείτε να χρησιμοποιήσετε το 'και' και 'OR' τελεστές.

## <a name="hybrid-configuration-optional"></a>Ρύθμιση παραμέτρων υβριδικού (προαιρετικά)

> [AZURE.NOTE] Αυτό το βήμα απαιτείται μόνο εάν χρησιμοποιείτε το SQL Server εσωτερικής εγκατάστασης πίσω από το τείχος προστασίας.

Εφαρμογή υπηρεσίας χρησιμοποιεί τη Διαχείριση ρύθμισης παραμέτρων υβριδικής για να συνδεθείτε με ασφάλεια στο σύστημα εσωτερικής εγκατάστασης. Εάν είστε σύνδεσης χρησιμοποιεί ένα SQL Server εσωτερικής εγκατάστασης, απαιτείται η διαχείριση σύνδεσης υβριδική.

> [AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure λογικής εφαρμογών πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε λογική εφαρμογής](https://tryappservice.azure.com/?appservice=logic), όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή της λογικής μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

Ανατρέξτε στο θέμα [χρήση της διαχείρισης σύνδεσης υβριδική](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Κάντε περισσότερα με την γραμμή σύνδεσης
Τώρα που δημιουργείται στη γραμμή σύνδεσης, μπορείτε να την προσθέσετε σε μια ροή εργασίας επιχειρήσεις χρήση λογικής εφαρμογής. Ανατρέξτε στο θέμα [Τι είναι οι εφαρμογές λογικής;](app-service-logic-what-are-logic-apps.md).

Προβάλετε την αναφορά Swagger REST API στο [γραμμών σύνδεσης και API εφαρμογές αναφοράς](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Μπορείτε επίσης να αναθεωρήσετε επιδόσεων στατιστικών στοιχείων και στοιχείο ελέγχου ασφαλείας στη γραμμή σύνδεσης. Ανατρέξτε στο θέμα [Διαχείριση και την παρακολούθηση τις ενσωματωμένες εφαρμογές API και τις γραμμές σύνδεσης](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png