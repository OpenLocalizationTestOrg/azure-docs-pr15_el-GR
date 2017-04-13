<properties
    pageTitle="Σύνδεση με βάση δεδομένων SQL με χρήση φωνητικής γραφής | Microsoft Azure"
    description="Δώστε ένα δείγμα φωνητικής γραφής κώδικα που μπορείτε να εκτελέσετε για να συνδεθείτε με βάση δεδομένων SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="ajlam"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="andrela"/>


# <a name="connect-to-sql-database-by-using-ruby"></a>Σύνδεση με βάση δεδομένων SQL με χρήση φωνητικής γραφής 

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

Αυτό το θέμα δείχνει πώς μπορείτε να συνδεθείτε και υποβολή ερωτήματος σε μια βάση δεδομένων SQL Azure με χρήση φωνητικής γραφής. Μπορείτε να εκτελέσετε αυτό το δείγμα από το Windows, Ubuntu Linux ή Mac πλατφόρμες.

## <a name="step-1-configure-development-environment"></a>Βήμα 1: Ρύθμιση παραμέτρων περιβάλλον ανάπτυξης

[Προαπαιτούμενα στοιχεία για τη χρήση του προγράμματος οδήγησης φωνητικής γραφής TinyTDS για SQL Server](https://msdn.microsoft.com/library/mt711041.aspx)

## <a name="step-2-create-a-sql-database"></a>Βήμα 2: Δημιουργήστε μια βάση δεδομένων SQL

Δείτε τη [σελίδα γρήγορων αποτελεσμάτων](sql-database-get-started.md) για να μάθετε πώς μπορείτε να δημιουργήσετε ένα δείγμα βάσης δεδομένων.  Είναι σημαντικό να ακολουθήσετε του οδηγού για τη δημιουργία ενός **προτύπου βάσης δεδομένων AdventureWorks**. Τα δείγματα που φαίνεται παρακάτω λειτουργούν μόνο με το **σχήμα AdventureWorks**.

## <a name="step-3-get-connection-details"></a>Βήμα 3: Λήψη λεπτομέρειες σύνδεσης

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Βήμα 4: Εκτέλεση δείγμα κώδικα

[Απόδειξη έννοια τη σύνδεση σε SQL με χρήση φωνητικής γραφής](http://msdn.microsoft.com/library/mt715797.aspx)

## <a name="next-steps"></a>Επόμενα βήματα

* Δείτε την [Επισκόπηση ανάπτυξης βάσης δεδομένων SQL](sql-database-develop-overview.md)
* Περισσότερες πληροφορίες σχετικά με το [Πρόγραμμα οδήγησης φωνητικής γραφής της Microsoft για τον SQL Server](https://msdn.microsoft.com/library/mt691981.aspx)

## <a name="additional-resources"></a>Πρόσθετοι πόροι 

* [Σχεδίαση μοτίβα για εφαρμογές ΑΔΑ πολλών μισθωτή με βάση δεδομένων SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Εξερεύνηση όλες τις [δυνατότητες της βάσης δεδομένων SQL](https://azure.microsoft.com/services/sql-database/)
