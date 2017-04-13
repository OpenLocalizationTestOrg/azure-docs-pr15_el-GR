<properties
    pageTitle="Σύνδεση με βάση δεδομένων SQL, χρησιμοποιώντας το Java με JDBC σε Windows | Microsoft Azure"
    description="Παρουσιάζει ένα δείγμα κώδικα Java που μπορείτε να χρησιμοποιήσετε για να συνδεθείτε με βάση δεδομένων SQL Azure. Το δείγμα χρησιμοποιεί JDBC και εκτελείται στον υπολογιστή-πελάτη υπολογιστή Windows."
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="lbosq"/>


# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>Σύνδεση με βάση δεδομένων SQL, χρησιμοποιώντας το Java με JDBC στα Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Αυτό το θέμα παρουσιάζει ένα δείγμα κώδικα Java που μπορείτε να χρησιμοποιήσετε για να συνδεθείτε με βάση δεδομένων SQL Azure. Το δείγμα Java βασίζεται σε το Java Development Kit (JDK) έκδοση 1.8. Το δείγμα συνδέεται με μια βάση δεδομένων SQL Azure χρησιμοποιώντας το πρόγραμμα οδήγησης JDBC.

## <a name="step-1--configure-development-environment"></a>Βήμα 1: Ρύθμιση παραμέτρων περιβάλλον ανάπτυξης

[Ρύθμιση παραμέτρων περιβάλλον ανάπτυξης για την ανάπτυξη Java](https://msdn.microsoft.com/library/mt720658.aspx)

## <a name="step-2-create-a-sql-database"></a>Βήμα 2: Δημιουργήστε μια βάση δεδομένων SQL

Δείτε τη [σελίδα γρήγορων αποτελεσμάτων](sql-database-get-started.md) για να μάθετε πώς μπορείτε να δημιουργήσετε μια βάση δεδομένων.  

## <a name="step-3-get-connection-string"></a>Βήμα 3: Εξαγωγή μιας συμβολοσειράς σύνδεσης

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Εάν χρησιμοποιείτε το πρόγραμμα οδήγησης JTDS JDBC και, στη συνέχεια, θα πρέπει να προσθέσετε "ssl = απαιτούν" στη διεύθυνση URL της σύνδεσης συμβολοσειράς και πρέπει να ορίσετε την παρακάτω επιλογή για την JVM "-Djsse.enableCBCProtection=false". Αυτή η επιλογή JVM απενεργοποιεί επιδιόρθωση για μια ευπάθεια ασφαλείας, οπότε βεβαιωθείτε ότι μπορείτε να κατανοήσετε τι κινδύνου εμπλέκεται πριν να ορίσετε αυτήν την επιλογή.

## <a name="step-4-run-sample-code"></a>Βήμα 4: Εκτέλεση δείγμα κώδικα

* [Απόδειξη έννοια τη σύνδεση σε SQL με χρήση Java](https://msdn.microsoft.com/library/mt720656.aspx)

## <a name="next-steps"></a>Επόμενα βήματα

* Επισκεφθείτε το [Κέντρο για προγραμματιστές Java](/develop/java/).
* Δείτε την [Επισκόπηση ανάπτυξης βάσης δεδομένων SQL](sql-database-develop-overview.md)
* Περισσότερες πληροφορίες σχετικά με το [Πρόγραμμα οδήγησης JDBC της Microsoft για τον SQL Server](https://msdn.microsoft.com/library/mt484311.aspx)

## <a name="additional-resources"></a>Πρόσθετοι πόροι 

* [Σχεδίαση μοτίβα για εφαρμογές ΑΔΑ πολλών μισθωτή με βάση δεδομένων SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Εξερεύνηση όλες τις [δυνατότητες της βάσης δεδομένων SQL](https://azure.microsoft.com/services/sql-database/)
