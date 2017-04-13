<properties
   pageTitle="Δημιουργία ενσωματωμένες λύσεις με αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Εργαλεία και συνεργάτες με τις λύσεις που ενοποιούνται με το αποθήκη δεδομένων του SQL. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="leverage-other-services-with-sql-data-warehouse"></a>Αξιοποιήσετε άλλες υπηρεσίες με αποθήκη δεδομένων του SQL
Εκτός από τις βασικές λειτουργίες, αποθήκη δεδομένων του SQL επιτρέπει στους χρήστες να αξιοποιήσετε πολλές από τις άλλες υπηρεσίες στο Azure μαζί με αυτό.  Συγκεκριμένα, θα σας αυτήν τη στιγμή έχουν λάβει μέτρα για βάθος ενοποίηση με τα εξής:

+ Power BI
+ Εργοστασιακές Azure δεδομένων
+ Azure μηχανικής εκμάθησης
+ Ανάλυση Azure ροής

Εργαζόμαστε για τη σύνδεση με τις περισσότερες υπηρεσίες κατά μήκος του Azure περιβάλλον εμπορικής προσαρμογής.

##<a name="power-bi"></a>Power BI
Ενοποίηση του Power BI σάς επιτρέπει να αξιοποιήσετε στη δύναμη του υπολογισμού του αποθήκη δεδομένων του SQL με το δυναμικής αναφοράς και απεικόνιση του Power BI. Ενοποίηση του Power BI περιλαμβάνει τη συγκεκριμένη στιγμή:

+ **Απευθείας σύνδεση**: πιο σύνδεσης με λογική προώθηση προς τα κάτω σε σχέση με αποθήκη δεδομένων του SQL για προχωρημένους.  Αυτό παρέχει πιο γρήγορη ανάλυση σε μεγαλύτερη κλίμακα.
+ **Άνοιγμα στο Power BI**: το κουμπί 'Άνοιγμα στο Power BI' μεταφέρει πληροφορίες παρουσίας του Power BI, επιτρέποντάς για μια πιο ομαλή σύνδεση.

Ανατρέξτε στο θέμα [ενσωματώσουν το Power BI](./sql-data-warehouse-integrate-power-bi.md) ή στην [τεκμηρίωση του Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) για περισσότερες πληροφορίες.

##<a name="azure-data-factory"></a>Εργοστασιακές Azure δεδομένων
Azure εργοστασίου δεδομένων παρέχει στους χρήστες μια διαχειριζόμενη πλατφόρμα για να δημιουργήσετε σύνθετα αγωγούς εξαγάγετε φόρτωση.  Αποθήκη δεδομένων SQL ενοποίηση με το Azure εργοστασίου δεδομένων περιλαμβάνει τα εξής:

+ **Αποθηκευμένες διαδικασίες**: οργανώσετε την εκτέλεση αποθηκευμένων διαδικασιών σε αποθήκη δεδομένων του SQL.
+ **Αντιγραφή**: χρήση ADF για τη μετακίνηση δεδομένων σε αποθήκη δεδομένων του SQL.  Αυτή η λειτουργία να χρησιμοποιήσετε μηχανισμό κίνηση του ADF τυπική δεδομένων ή PolyBase στην περιοχή στο παρασκήνιο. 

Ανατρέξτε στο θέμα [ενσωματώσουν το Azure εργοστασίου δεδομένων](./sql-data-warehouse-integrate-azure-data-factory.md) ή την [τεκμηρίωση εργοστασίου δεδομένων Azure](https://azure.microsoft.com/documentation/services/data-factory/) για περισσότερες πληροφορίες.

##<a name="azure-machine-learning"></a>Azure μηχανικής εκμάθησης
Azure μηχανικής εκμάθησης είναι μια υπηρεσία πλήρως διαχειριζόμενων ανάλυσης που επιτρέπει στους χρήστες να δημιουργούν περίπλοκα μοντέλα αξιοποίηση ένα μεγάλο σύνολο εργαλείων πρόβλεψης.  Αποθήκη δεδομένων του SQL υποστηρίζεται ως προέλευσης και προορισμού για αυτά τα μοντέλα με τις ακόλουθες λειτουργίες:

+ **Ανάγνωση δεδομένων:** Μονάδα δίσκου, μοντέλα κλίμακα με T-SQL σε σχέση με αποθήκη δεδομένων του SQL.
+ **Εγγραφή δεδομένων:** Ολοκλήρωση αλλαγών από οποιοδήποτε μοντέλο Επιστροφή στην αποθήκη δεδομένων του SQL.

Ανατρέξτε στο θέμα [ενσωματώσουν το Azure μηχανικής εκμάθησης](./sql-data-warehouse-integrate-azure-machine-learning.md) ή την [τεκμηρίωση Azure μηχανικής εκμάθησης](https://azure.microsoft.com/services/machine-learning/) για περισσότερες πληροφορίες.

##<a name="azure-stream-analytics"></a>Ανάλυση Azure ροής
Azure ροή ανάλυσης είναι μια σύνθετη, πλήρως διαχειριζόμενων υποδομή για την επεξεργασία και κατανάλωση δεδομένα συμβάντων που δημιουργούνται από διανομέα συμβάν Azure.  Επιτρέπει την ενοποίηση με αποθήκη δεδομένων του SQL για ροή δεδομένων για αποτελεσματική επεξεργασία και αποθήκευση μαζί με σχεσιακών δεδομένων Ενεργοποίηση βαθύτερη, πιο σύνθετη ανάλυση.  

+ **Εργασία εξόδου:** Αποστολή εξόδου από ανάλυση ροή εργασιών απευθείας σε αποθήκη δεδομένων του SQL.

Ανατρέξτε στο θέμα [ενσωματώσουν ανάλυση ροή Azure](./sql-data-warehouse-integrate-azure-stream-analytics.md) ή την [ανάλυση ροή Azure τεκμηρίωση](https://azure.microsoft.com/documentation/services/stream-analytics/) για περισσότερες πληροφορίες.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
