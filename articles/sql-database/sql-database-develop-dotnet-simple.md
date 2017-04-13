<properties
    pageTitle="Σύνδεση με βάση δεδομένων SQL με χρήση .NET (C#) | Microsoft Azure"
    description="Χρησιμοποιήστε το δείγμα κώδικα σε αυτό γρήγορης έναρξης για να δημιουργήσετε μια εφαρμογή σύγχρονο με C# και αντίγραφα ασφαλείας των ισχυρών σχεσιακή βάση δεδομένων στο cloud με βάση δεδομένων SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="tobbox"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="tobiast"/>

# <a name="connect-to-sql-database-by-using-net-c"></a>Σύνδεση με βάση δεδομένων SQL με χρήση .NET (C#)

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

## <a name="step-1--configure-development-environment"></a>Βήμα 1: Ρύθμιση παραμέτρων περιβάλλον ανάπτυξης

[Ρύθμιση παραμέτρων περιβάλλον ανάπτυξης για την ανάπτυξη ADO.NET](https://msdn.microsoft.com/library/mt718321.aspx)

## <a name="step-2-create-a-sql-database"></a>Βήμα 2: Δημιουργήστε μια βάση δεδομένων SQL

Δείτε τη [σελίδα γρήγορων αποτελεσμάτων](sql-database-get-started.md) για να μάθετε πώς μπορείτε να δημιουργήσετε ένα δείγμα βάσης δεδομένων.  Είναι σημαντικό να ακολουθήσετε του οδηγού για τη δημιουργία ενός **προτύπου βάσης δεδομένων AdventureWorks**. Τα δείγματα που φαίνεται παρακάτω λειτουργούν μόνο με το **σχήμα AdventureWorks**.  

## <a name="step-3--get-connection-string"></a>Βήμα 3: Εξαγωγή μιας συμβολοσειράς σύνδεσης

[AZURE.INCLUDE [sql-database-include-connection-string-dotnet-20-portalshots](../../includes/sql-database-include-connection-string-dotnet-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Βήμα 4: Εκτέλεση δείγμα κώδικα

* [Απόδειξη έννοια τη σύνδεση χρησιμοποιώντας το ADO.NET SQL](https://msdn.microsoft.com/library/mt718320.aspx)
* [Σύνδεση resiliently σε SQL με ADO.NET](https://msdn.microsoft.com/library/mt703195.aspx)

## <a name="next-steps"></a>Επόμενα βήματα

* [Δημιουργία εφαρμογής ASP.NET MVC με auth και SQL DB και ανάπτυξη σε Azure εφαρμογής υπηρεσίας]( ../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)
* Δείτε την [Επισκόπηση ανάπτυξης βάσης δεδομένων SQL](sql-database-develop-overview.md)
* Περισσότερες πληροφορίες σχετικά με το [Πρόγραμμα οδήγησης ADO.Net της Microsoft για τον SQL Server](https://msdn.microsoft.com/library/mt657768.aspx)

## <a name="additional-resources"></a>Πρόσθετοι πόροι 

* [Σχεδίαση μοτίβα για εφαρμογές ΑΔΑ πολλών μισθωτή με βάση δεδομένων SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Εξερεύνηση όλες τις [δυνατότητες της βάσης δεδομένων SQL](https://azure.microsoft.com/services/sql-database/)





