<properties
    pageTitle="Σύνδεση με βάση δεδομένων SQL με χρήση PHP σε Windows | Microsoft Azure"
    description="Παρουσιάζει ένα πρόγραμμα PHP δείγμα που συνδέεται στη βάση δεδομένων SQL Azure από ένα πρόγραμμα-πελάτη των Windows και παρέχει συνδέσεις με τα στοιχεία λογισμικού που απαιτούνται από το πρόγραμμα-πελάτη."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-php-on-windows"></a>Σύνδεση με βάση δεδομένων SQL με χρήση PHP στα Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Αυτό το θέμα παρουσιάζει πώς μπορείτε να συνδεθείτε με βάση δεδομένων SQL Azure από μια εφαρμογή προγράμματος-πελάτη γραμμένο σε PHP που εκτελείται σε Windows.

## <a name="step-1--configure-development-environment"></a>Βήμα 1: Ρύθμιση παραμέτρων περιβάλλον ανάπτυξης

[Ρύθμιση παραμέτρων περιβάλλον ανάπτυξης για την ανάπτυξη PHP](https://msdn.microsoft.com/library/mt720663.aspx)

## <a name="step-2-create-a-sql-database"></a>Βήμα 2: Δημιουργήστε μια βάση δεδομένων SQL

Δείτε τη [σελίδα γρήγορων αποτελεσμάτων](sql-database-get-started.md) για να μάθετε πώς μπορείτε να δημιουργήσετε ένα δείγμα βάσης δεδομένων.  Είναι σημαντικό να ακολουθήσετε του οδηγού για τη δημιουργία ενός **προτύπου βάσης δεδομένων AdventureWorks**. Τα δείγματα που φαίνεται παρακάτω λειτουργούν μόνο με το **σχήμα AdventureWorks**.


## <a name="step-3-get-connection-details"></a>Βήμα 3: Λήψη λεπτομέρειες σύνδεσης

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]


## <a name="step-4-run-sample-code"></a>Βήμα 4: Εκτέλεση δείγμα κώδικα

* [Απόδειξη έννοια τη σύνδεση με χρήση PHP SQL](https://msdn.microsoft.com/library/mt720665.aspx)
* [Σύνδεση resiliently σε SQL με PHP](https://msdn.microsoft.com/library/mt720667.aspx)


## <a name="next-steps"></a>Επόμενα βήματα

* Δείτε την [Επισκόπηση ανάπτυξης βάσης δεδομένων SQL](sql-database-develop-overview.md)
* Περισσότερες πληροφορίες σχετικά με το [Πρόγραμμα οδήγησης PHP της Microsoft για τον SQL Server](https://msdn.microsoft.com/library/dn865013.aspx)
* Για περισσότερες πληροφορίες σχετικά με τις PHP εγκατάστασης και η χρήση, ανατρέξτε στο θέμα [Πρόσβαση σε βάσεις δεδομένων SQL Server με PHP](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx).

## <a name="additional-resources"></a>Πρόσθετοι πόροι 

* [Σχεδίαση μοτίβα για εφαρμογές ΑΔΑ πολλών μισθωτή με βάση δεδομένων SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Εξερεύνηση όλες τις [δυνατότητες της βάσης δεδομένων SQL](https://azure.microsoft.com/services/sql-database/)
