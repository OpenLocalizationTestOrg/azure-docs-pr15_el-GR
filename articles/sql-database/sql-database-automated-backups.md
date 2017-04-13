<properties
   pageTitle="Δημιουργία αντιγράφων ασφαλείας της βάσης δεδομένων SQL - αυτόματη, παν πλεονάζοντα | Microsoft Azure" 
   description="Βάση δεδομένων SQL δημιουργεί ένα αντίγραφο ασφαλείας της τοπικής βάσης δεδομένων κάθε πέντε λεπτά να συνδεθεί αυτόματα και χρησιμοποιεί Azure πρόσβαση για ανάγνωση παν πλεονάζοντα χώρο αποθήκευσης (RA-Εξοπλισμό) για την παροχή παν πλεονασμού. "
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/20/2016"
   ms.author="carlrab;barbkess"/>

<!------------------
This topic is annotated with TEMPLATE guidelines for FEATURE TOPICS.


Metadata guidelines

pageTitle
    60 characters or less. Includes name of the feature - primary benefit. Not the same as H1. Its 60 characters or fewer including all characters between the quotes and the Microsoft Azure site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "SQL Database automatically creates a local database backup every few minutes and uses Azure read-access geo-redundant storage for geo-redundancy."
------------------>

<!----------------

TEMPLATE GUIDELINES for feature topics

The Feature Topic is a one-pager (ok, sometimes longer) that explains a capability of the product or service. It explains what the capability is and characteristics of the capability.  

It is a "learning" topic, not an action topic.

DO explain this:
    • Definition of the feature terminology.  i.e., What is a database backup?
    • Characteristics and capabilities of the feature. (How the feature works)
    • Common uses with links to overview topics that recommend when to use the feature.
    • Reference specifications (Limitations and Restrictions, Permissions, General Remarks, etc.)
    • Next Steps with links to related overviews, features, and tasks.

DON'T explain this:
    • How to steps for using the feature (Tasks)
    • How to solve business problems that incorporate the feature (Overviews)
------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What is in this topic?" Write the H1 heading in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  
    
    To help people understand this is a learning topic and not an action topic, start the title with "Learn about ... "

    Heading must use an industry standard term. If your feature is a proprietary name like "Elastic database pools", use a synonym. For example:    "Learn about elastic database pools for multi-tenant databases". In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="learn-about-sql-database-backups"></a>Μάθετε σχετικά με τη δημιουργία αντιγράφων ασφαλείας της βάσης δεδομένων SQL

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top key words that you are using throughout the article.The introduction should be brief and to the point of what the feature is, what it is used for, and what's in the article. 

    If the introduction is short enough, your article can pop to the top in Google Instant Answers.

    In this example:
    
 

Sentence #1 Explains what the article will cover, which is what the feature is or does. This is also the metadata description. 
    SQL Database automatically creates a local database backup every five minutes and uses Azure read-access geo-redundant storage (RA-GRS) to provide geo-redundancy. 

Sentence #2 Explains why I should care about this.  
    Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.

-------------------->

Βάση δεδομένων SQL αυτόματα δημιουργεί ένα αντίγραφο ασφαλείας της τοπικής βάσης δεδομένων κάθε λίγα λεπτά και χρησιμοποιεί Azure χώρο αποθήκευσης παν πλεονάζοντα πρόσβαση για ανάγνωση για παν πλεονασμού. Αντίγραφα ασφαλείας βάσης δεδομένων είναι ουσιαστικό μέρος της στρατηγικής οποιαδήποτε επιχειρήσεις συνέχειας και καταστροφή ανάκτησης, επειδή αυτές Προστατεύστε τα δεδομένα σας από καταστροφή έγιναν κατά λάθος ή διαγραφή. 

<!-- This image needs work, so not putting it in right now.

This diagram shows SQL Database running in the US East region. It creates a database backup every five minutes, which it stores locally to Azure Read Access Geo-redundant Storage (RA-GRS). Azure uses geo-replication to copy the database backups to a paired data center in the US West region.

![geo-restore](./media/sql-database-geo-restore/geo-restore-1.png)

-->

<!---------------
GUIDELINES for the first ## H2.

    The first ## describes what the feature encompasses and how it is used. It points to related task articles.
    
    For consistency, being the heading with "What is ... "
----------------->

## <a name="what-is-a-sql-database-backup"></a>Τι είναι ένα αντίγραφο ασφαλείας της βάσης δεδομένων SQL;  

<!-- 
    Explains what a SQL Database backup is and answers an important question that people want to know.
-->

Ένα αντίγραφο ασφαλείας της βάσης δεδομένων SQL περιλαμβάνει τόσο τοπική βάση δεδομένων και παν πλεονάζοντα αντιγράφων ασφαλείας. Αυτά τα αντίγραφα ασφαλείας δημιουργούνται αυτόματα και χωρίς πρόσθετη χρέωση. Δεν χρειάζεται να κάνετε τίποτα για τους συμβεί.

<!----------------- 
    Explains first component of the backup feature
------------------>

Για δημιουργία τοπικών αντιγράφων ασφαλείας, βάση δεδομένων SQL χρησιμοποιεί τεχνολογία SQL Server για να δημιουργήσετε αντίγραφα ασφαλείας [πλήρους](https://msdn.microsoft.com/library/ms186289.aspx) [διακριτική](https://msdn.microsoft.com/library/ms175526.aspx )και [αρχείο καταγραφής συναλλαγών](https://msdn.microsoft.com/library/ms191429.aspx) . Τα αντίγραφα ασφαλείας του αρχείου καταγραφής συναλλαγών συμβεί κάθε πέντε λεπτά, η οποία σας επιτρέπει να κάνετε επαναφορά σε δεδομένη χρονική στιγμή στον ίδιο διακομιστή που φιλοξενεί τη βάση δεδομένων. Όταν εκτελείτε επαναφορά μιας βάσης δεδομένων, η υπηρεσία στοιχεία ανάληψη ποιο πλήρους, η διαφορά και συναλλαγής καταγραφής δημιουργίας αντιγράφων ασφαλείας πρέπει να γίνει επαναφορά.

<!--------------- 
    Explicit list of what to do with a local backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Χρησιμοποιήστε ένα αντίγραφο ασφαλείας της τοπικής βάσης δεδομένων για να:

- Επαναφορά βάσης δεδομένων σε ένα σημείο-σε-time εντός της περιόδου διατήρησης. Με ένα αντίγραφο ασφαλείας της βάσης δεδομένων που μπορεί να επαναφέρετε μια βάση δεδομένων σε ένα σημείο-σε-time, επαναφέρετε μια διαγραμμένη βάση δεδομένων με το χρόνο που έχει διαγραφεί ή την επαναφορά μιας βάσης δεδομένων σε άλλη γεωγραφική περιοχή. Για να εκτελέσετε μια επαναφορά, ανατρέξτε στο θέμα [Επαναφορά βάσης δεδομένων από ένα αντίγραφο ασφαλείας της βάσης δεδομένων](sql-database-recovery-using-backups.md).

- Αντιγράψτε μια βάση δεδομένων SQL server στην περιοχή ίδιες ή σε διαφορετικές. Το αντίγραφο που είναι ενημερώσετε συμβατή με την τρέχουσα βάση δεδομένων SQL. Για να εκτελέσετε ένα αντίγραφο, ανατρέξτε στο θέμα [Αντιγραφή βάσης δεδομένων](sql-database-copy.md).

- Αρχειοθέτηση ένα αντίγραφο ασφαλείας της βάσης δεδομένων πέρα από την περίοδο διατήρησης δημιουργίας αντιγράφων ασφαλείας. Για να εκτελέσετε μια αρχειοθήκη, [μια βάση δεδομένων SQL για ένα BACPAC εξαγωγή](sql-database-export.md) αρχείου. Στη συνέχεια, μπορείτε να αρχειοθετήσετε το BACPAC με το χώρο αποθήκευσης μακροπρόθεσμες και να αποθηκεύσετε πέρα από το περίοδος διατήρησης. Εναλλακτικά, χρησιμοποιήστε το BACPAC για να μεταφέρετε ένα αντίγραφο της βάσης δεδομένων σας με τον SQL Server, είτε στην εσωτερική εγκατάσταση ή σε μια εικονική μηχανή Azure (Εικονική).

<!----------------- 
    Explains first component of the backup feature
------------------>

Για παν πλεονάζοντα αντίγραφα ασφαλείας, βάση δεδομένων SQL χρησιμοποιεί [χώρο αποθήκευσης Azure αναπαραγωγής](../storage/storage-redundancy.md). Βάση δεδομένων SQL αποθηκεύει αρχεία αντιγράφων ασφαλείας της τοπικής βάσης δεδομένων σε ένα λογαριασμό [Παν πλεονάζοντα πρόσβαση για ανάγνωση (RA-Εξοπλισμό) χώρου αποθήκευσης](../storage/storage-redundancy.md#read-access-geo-redundant-storage) . Azure αναπαράγει τα αρχεία αντιγράφων ασφαλείας σε ένα [Κέντρο δεδομένων ζεύγη](../best-practices-availability-paired-regions.md). 

<!--------------- 
    Explicit list of what to do with a geo-redundant backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Χρησιμοποιήστε παν πλεονάζοντα αντίγραφο ασφαλείας για να:

- Επαναφορά βάσης δεδομένων σε διαφορετική γεωγραφική περιοχή, σε περίπτωση που δεν μπορείτε να αποκτήσετε πρόσβαση το αντίγραφο ασφαλείας της βάσης δεδομένων από την περιοχή κύρια βάση δεδομένων σας. 

>[AZURE.NOTE] Azure χώρο αποθήκευσης, της όρο *αναπαραγωγής* αναφέρεται αντιγραφή αρχείων από μια θέση σε μια άλλη. Αναφέρεται διατήρηση σε πολλά δευτερεύοντα βάσεις δεδομένων που συγχρονίζονται με μια κύρια βάση δεδομένων του SQL *Αναπαραγωγή βάσης δεδομένων* . 

<!----------------
    The next ## H2's discuss key characteristics of how the feature works. The title is in conversational language and asks the question that will be answered.
------------------->
## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Πόσο χώρο αποθήκευσης αντιγράφων ασφαλείας περιλαμβάνεται χωρίς κόστος;

Βάση δεδομένων SQL παρέχει έως 200% του χώρου αποθήκευσής σας μέγιστο προμήθεια του φακέλου βάσης δεδομένων ως αποθήκευσης αντιγράφων ασφαλείας χωρίς πρόσθετο κόστος. Για παράδειγμα, εάν έχετε μια τυπική DB παρουσία με μέγεθος DB προμήθεια του φακέλου από 250 GB, έχετε 500 GB χώρου αποθήκευσης αντιγράφων ασφαλείας χωρίς πρόσθετη χρέωση. Εάν η βάση δεδομένων υπερβαίνει το παρεχόμενο αποθήκευσης αντιγράφων ασφαλείας, μπορείτε να επιλέξετε για να μειώσετε την περίοδο διατήρησης με επικοινωνία με την υποστήριξη του Azure. Μια άλλη επιλογή είναι να πληρώσετε για δημιουργίας αντιγράφων ασφαλείας επιπλέον χώρο αποθήκευσης που είναι χρεωθεί με την τυπική χρέωση πρόσβαση για ανάγνωση γεωγραφικά πλεονάζοντα χώρο αποθήκευσης (RA-Εξοπλισμό). 

## <a name="how-often-do-backups-happen"></a>Πόσο συχνά συμβεί αντίγραφα ασφαλείας;

Για τοπική βάση δεδομένων αντιγράφων ασφαλείας, πλήρους βάσης δεδομένων αντιγράφων ασφαλείας συμβεί εβδομαδιαία, συμβεί διακριτική βάσης δεδομένων αντιγράφων ασφαλείας ανά ώρα και αρχείο καταγραφής συναλλαγών αντίγραφα ασφαλείας συμβεί κάθε πέντε λεπτά. Το πρώτο πλήρες αντίγραφο ασφαλείας είναι προγραμματισμένη αμέσως μετά τη δημιουργία μιας βάσης δεδομένων. Συνήθως ότι ολοκληρώνεται εντός 30 λεπτών, αλλά αυτό μπορεί να διαρκέσει περισσότερο όταν η βάση δεδομένων είναι σημαντική μεγέθους. Για παράδειγμα, το αρχικό αντίγραφο ασφαλείας μπορεί να διαρκέσει περισσότερο σε Επαναφορά βάσης δεδομένων ή ένα αντίγραφο της βάσης δεδομένων. Μετά την πρώτη πλήρη αντίγραφα ασφαλείας, όλα τα αντίγραφα ασφαλείας περαιτέρω είναι προγραμματισμένη αυτόματα και ελέγχονται σιωπηρά στο παρασκήνιο. Τα ακριβή χρονισμό της πλήρους και [διακριτική](https://msdn.microsoft.com/library/ms175526.aspx) αντίγραφα ασφαλείας βάσης δεδομένων καθορίζεται ως το υπόλοιπα το συνολικό φόρτο εργασίας συστήματος. 

Για παν πλεονάζοντα αντίγραφα ασφαλείας, πλήρους και διακριτική αντίγραφα ασφαλείας αντιγράφονται σύμφωνα με το χρονοδιάγραμμα αναπαραγωγής αποθήκευσης Azure.

## <a name="how-long-do-you-keep-my-backups"></a>Πόσο χρόνο μπορείτε να διατηρήσετε μου αντίγραφα ασφαλείας;

Κάθε αντίγραφο ασφαλείας της βάσης δεδομένων SQL έχει μια περίοδος διατήρησης που βασίζεται στο [επίπεδο υπηρεσιών](sql-database-service-tiers.md) της βάσης δεδομένων. Η περίοδος διατήρησης για μια βάση δεδομένων σε του:

<!------------------

    Using a list so the information is easy to find when scanning.
------------------->

- Επίπεδο βασικές υπηρεσιών είναι επτά ημέρες.
- Επίπεδο τυπική υπηρεσιών είναι 35 ημέρες.
- Επίπεδο υπηρεσιών Premium είναι 35 ημέρες.


Εάν μπορείτε να υποβιβάσετε τη βάση δεδομένων από την τυπική ή Premium επίπεδα υπηρεσίας σε βασικό, τα αντίγραφα ασφαλείας αποθηκεύονται για επτά ημέρες. Όλα τα υπάρχοντα αντίγραφα ασφαλείας παλιότερα από επτά ημέρες δεν είναι πλέον διαθέσιμη. 

Αν κάνετε αναβάθμιση τη βάση δεδομένων από το επίπεδο βασικές υπηρεσιών σε τυπική ή Premium, βάση δεδομένων SQL διατηρεί τα υπάρχοντα αντίγραφα ασφαλείας μέχρι να είναι 35 ημερών. Διατηρεί νέα αντίγραφα ασφαλείας, καθώς προκύπτουν για 35 ημέρες.
 
Εάν διαγράψετε μια βάση δεδομένων, η βάση δεδομένων SQL διατηρεί τα αντίγραφα ασφαλείας με τον ίδιο τρόπο που θα είχε για μια ηλεκτρονική βάση δεδομένων. Για παράδειγμα, ας υποθέσουμε ότι μπορείτε να διαγράψετε μια βασική βάση δεδομένων που περιλαμβάνει μια περίοδος διατήρησης από επτά ημέρες. Για τις τρεις ημέρες περισσότερες αποθηκεύεται ένα αντίγραφο ασφαλείας που είναι τεσσάρων ημερών.

>[AZURE.IMPORTANT]
    Εάν διαγράψετε το Azure SQL διακομιστή που φιλοξενεί βάσεις δεδομένων SQL, όλες τις βάσεις δεδομένων που ανήκουν στον διακομιστή διαγράφονται επίσης και είναι δυνατή η ανάκτησή. Δεν μπορείτε να επαναφέρετε ένα διαγραμμένο διακομιστή.

<!-------------------
OPTIONAL section
## Best practices 
--------------------->

<!-------------------
OPTIONAL section
## General remarks
--------------------->

<!-------------------
OPTIONAL section
## Limitations and restrictions
--------------------->

<!-------------------
OPTIONAL section
## Metadata
--------------------->

<!-------------------
OPTIONAL section
## Performance
--------------------->

<!-------------------
OPTIONAL section
## Permissions
--------------------->

<!-------------------
OPTIONAL section
## Security
--------------------->

<!-------------------
GUIDELINES for Next Steps

    The last section is Next Steps. Give a next step that would be relevant to the customer after they have learned about the feature and the tasks associated with it.  Perhaps point them to one or two key scenarios that use this feature.

    You don't need to repeat links you have already given them.
--------------------->

## <a name="next-steps"></a>Επόμενα βήματα

Αντίγραφα ασφαλείας βάσης δεδομένων είναι ουσιαστικό μέρος της στρατηγικής οποιαδήποτε επιχειρήσεις συνέχειας και καταστροφή ανάκτησης, επειδή αυτές Προστατεύστε τα δεδομένα σας από καταστροφή έγιναν κατά λάθος ή διαγραφή. Για να δείτε πώς βάσης δεδομένων αντιγράφων ασφαλείας σε μιας ευρύτερης στρατηγικής, ανατρέξτε στο θέμα [Επισκόπηση συνέχειας επιχειρήσεις](sql-database-business-continuity.md).


