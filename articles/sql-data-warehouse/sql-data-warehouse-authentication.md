<properties
   pageTitle="Έλεγχος ταυτότητας αποθήκη δεδομένων Azure SQL | Microsoft Azure"
   description="Azure Active Directory (AAD) και στον SQL Server ελέγχου ταυτότητας για την αποθήκη δεδομένων του SQL Azure."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/24/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# <a name="authentication-to-azure-sql-data-warehouse"></a>Έλεγχος ταυτότητας αποθήκη δεδομένων Azure SQL

> [AZURE.SELECTOR]
- [Επισκόπηση ασφαλείας](sql-data-warehouse-overview-manage-security.md)
- [Έλεγχος ταυτότητας](sql-data-warehouse-authentication.md)
- [Κρυπτογράφηση (πύλη)](sql-data-warehouse-encryption-tde.md)
- [Κρυπτογράφηση (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Για να συνδεθείτε αποθήκη δεδομένων του SQL, πρέπει να εισάγετε διαπιστευτηρίων ασφαλείας για λόγους ελέγχου ταυτότητας. Κατά τη δημιουργία μιας σύνδεσης, ορισμένες ρυθμίσεις σύνδεσης έχουν ρυθμιστεί ως μέρος του καθορισμού περίοδο λειτουργίας σας στο ερώτημα.  

Για περισσότερες πληροφορίες σχετικά με την ασφάλεια και πώς μπορείτε να ενεργοποιήσετε τις συνδέσεις με την αποθήκη δεδομένων σας, ανατρέξτε στο θέμα [ασφαλούς μια βάση δεδομένων στην αποθήκη δεδομένων του SQL][].

## <a name="sql-authentication"></a>Έλεγχος ταυτότητας του SQL
Για να συνδεθείτε αποθήκη δεδομένων του SQL, πρέπει να δώσετε τις παρακάτω πληροφορίες:

- Πλήρως προσδιορισμένο όνομα διακομιστή
- Καθορίστε τον έλεγχο ταυτότητας SQL
- Όνομα χρήστη
- Κωδικός πρόσβασης
- Προεπιλεγμένη βάση δεδομένων (προαιρετικό)

Από προεπιλογή τη σύνδεσή σας συνδέεται με την *κύρια* βάση δεδομένων και όχι στη βάση δεδομένων του χρήστη. Για να συνδεθείτε στη βάση δεδομένων του χρήστη, μπορείτε να επιλέξετε να κάνετε ένα από δύο πράγματα:

- Καθορισμός της προεπιλεγμένης βάσης δεδομένων κατά την εγγραφή σας διακομιστή με την Εξερεύνηση SQL Server αντικειμένου στο SSDT, SSMS, ή στη συμβολοσειρά σύνδεσης εφαρμογής. Για παράδειγμα, συμπεριλάβετε την παράμετρο InitialCatalog για μια σύνδεση ODBC.
- Επισημάνετε τη βάση δεδομένων χρήστη πριν από τη δημιουργία μιας περιόδου λειτουργίας στο SSDT.

> [AZURE.NOTE] Η πρόταση Transact-SQL **Βάση_δεδομένων ΧΡΉΣΗ,** δεν υποστηρίζεται για την αλλαγή της βάσης δεδομένων για μια σύνδεση. Για τη σύνδεση σε αποθήκη δεδομένων του SQL με SSDT οδηγίες, ανατρέξτε στο άρθρο [ερωτήματος με το Visual Studio][] .

## <a name="azure-active-directory-aad-authentication"></a>Έλεγχος ταυτότητας Azure Active Directory (AAD)

[Azure Active Directory] [ What is Azure Active Directory] ελέγχου ταυτότητας είναι ένας μηχανισμός σύνδεσης σε αποθήκη δεδομένων του Microsoft Azure SQL, χρησιμοποιώντας τις ταυτότητες σε Azure Active Directory (Azure AD). Με έλεγχο ταυτότητας Azure Active Directory, μπορείτε να διαχειριστείτε κεντρικά τις ταυτότητες χρηστών βάσεων δεδομένων και άλλες υπηρεσίες της Microsoft σε μια κεντρική θέση. Κεντρική διαχείριση Αναγνωριστικό παρέχει μια ενιαία θέση για τη Διαχείριση χρηστών αποθήκη δεδομένων του SQL και απλοποιεί τη Διαχείριση δικαιωμάτων. 

### <a name="benefits"></a>Πλεονεκτήματα

Azure Active Directory πλεονεκτήματα είναι:

- Παρέχει μια εναλλακτική λύση για έλεγχο ταυτότητας του SQL Server.
- Βοηθά στην διακόψτε τη διάδοση ταυτοτήτων χρήστη στους διακομιστές της βάσης δεδομένων.
- Επιτρέπει την περιστροφή τον κωδικό πρόσβασης σε ένα μόνο σημείο
- Διαχείριση δικαιωμάτων της βάσης δεδομένων με χρήση εξωτερικών ομάδες (AAD).
- Αποκλείει την αποθήκευση κωδικών πρόσβασης, ενεργοποιώντας ενσωματωμένο έλεγχο ταυτότητας των Windows και άλλες μορφές του ελέγχου ταυτότητας που υποστηρίζονται από το Azure Active Directory.
- Χρησιμοποιεί περιείχε χρήστες βάσεων δεδομένων για τον έλεγχο ταυτότητας ταυτότητες στο επίπεδο της βάσης δεδομένων.
- Υποστηρίζει έλεγχο ταυτότητας που βασίζεται σε διακριτικό για εφαρμογές τη σύνδεση σε αποθήκη δεδομένων του SQL.
- Υποστηρίζει έλεγχο ταυτότητας πολλών παραγόντων μέσω του Active Directory καθολικής ελέγχου ταυτότητας για SQL Server Management Studio. Για μια περιγραφή του ελέγχου ταυτότητας πολλαπλών παραγόντων, ανατρέξτε στο θέμα [SSMS υποστήριξης για το Azure AD MFA με βάση δεδομένων SQL και αποθήκη δεδομένων του SQL](../sql-database/sql-database-ssms-mfa-authentication.md).

> [AZURE.NOTE] Azure Active Directory είναι ακόμα σχετικά νέα και έχει κάποιους περιορισμούς. Για να βεβαιωθείτε ότι το Azure Active Directory είναι κατάλληλες για το περιβάλλον σας, ανατρέξτε στο θέμα [Azure AD δυνατότητες και περιορισμοί][], συγκεκριμένα τα επιπλέον ζητήματα.

### <a name="configuration-steps"></a>Βήματα ρύθμισης παραμέτρων

Ακολουθήστε τα παρακάτω βήματα για τη ρύθμιση παραμέτρων ελέγχου ταυτότητας Azure Active Directory.

1. Δημιουργία και τη συμπλήρωση μιας Azure Active Directory
2. Προαιρετικό: Συσχετίσετε ή να αλλάξετε την υπηρεσία καταλόγου active directory που σχετίζεται με τη συνδρομή σας Azure
3. Δημιουργία διαχειριστή Azure Active Directory για την αποθήκη δεδομένων του SQL Azure.
4. Ρύθμιση παραμέτρων υπολογιστές-πελάτες σας
5. Δημιουργία χρηστών περιεχόμενα βάσης δεδομένων στη βάση δεδομένων σας έχει αντιστοιχιστεί σε Azure AD ταυτότητες
6. Σύνδεση με την αποθήκη δεδομένων σας με χρήση Azure AD ταυτότητες

Αυτήν τη στιγμή χρήστες Azure Active Directory δεν εμφανίζονται στην Εξερεύνηση αντικείμενο SSDT. Ως λύση, προβάλετε τους χρήστες της [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
  
### <a name="find-the-details"></a>Βρείτε τις λεπτομέρειες
- Ολοκληρώστε τα λεπτομερή βήματα. Λεπτομερείς οδηγίες για τη ρύθμιση και χρήση του ελέγχου ταυτότητας Azure Active Directory είναι σχεδόν ίδια και για βάση δεδομένων SQL Azure και αποθήκη δεδομένων του SQL Azure. Ακολουθήστε τα λεπτομερή βήματα στο θέμα [σύνδεση με βάση δεδομένων SQL ή δεδομένα αποθήκης από χρησιμοποιώντας Azure Active Directory έλεγχος ταυτότητας του SQL](../sql-database/sql-database-aad-authentication.md).
- Δημιουργία ρόλων προσαρμοσμένη βάση δεδομένων και να προσθέσετε χρήστες σε ρόλους. Στη συνέχεια, εκχώρηση λεπτομερούς δικαιωμάτων για τους ρόλους. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με δικαιώματα μηχανισμός βάσεων δεδομένων](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Επόμενα βήματα

Για να ξεκινήσετε την υποβολή ερωτημάτων σας αποθήκη δεδομένων με το Visual Studio και άλλες εφαρμογές, ανατρέξτε στο θέμα [ερωτήματος με το Visual Studio][].

<!-- Article references -->
[Ασφάλιση μιας βάσης δεδομένων στην αποθήκη δεδομένων του SQL]: ./sql-data-warehouse-overview-manage-security.md
[Ερώτημα με το Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD δυνατότητες και περιορισμοί]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations