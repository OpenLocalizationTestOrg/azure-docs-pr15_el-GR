<properties
    pageTitle="Μετακίνηση βάσεις δεδομένων μεταξύ διακομιστών, μεταξύ συνδρομών και και έξοδος από το Azure."
    description="Γρήγορα βήματα για να αντιγράψετε, να μετακινήσετε και μετεγκατάσταση δεδομένων και βάσεων δεδομένων σε βάση δεδομένων SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Μετακίνηση βάσεις δεδομένων μεταξύ διακομιστών, μεταξύ συνδρομών και και έξοδος από Azure

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]
##<a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Για να μετακινήσετε μια βάση δεδομένων σε διαφορετικό διακομιστή στην ίδια συνδρομή
- Στην [Πύλη του Azure](https://portal.azure.com), κάντε κλικ στην επιλογή **βάσεις δεδομένων SQL**, επιλέξτε μια βάση δεδομένων από τη λίστα και, στη συνέχεια, κάντε κλικ στην επιλογή **Αντιγραφή**. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Αντιγραφή μιας βάσης δεδομένων Azure SQL](sql-database-copy.md) .

## <a name="to-move-a-database-between-subscriptions"></a>Για να μετακινήσετε μια βάση δεδομένων μεταξύ συνδρομών
- Στην [Πύλη του Azure](https://portal.azure.com), κάντε κλικ στην επιλογή **διακομιστές SQL** και, στη συνέχεια, επιλέξτε το διακομιστή που φιλοξενεί τη βάση δεδομένων από τη λίστα. Κάντε κλικ στην επιλογή **Μετακίνηση**και, στη συνέχεια, επιλέξτε τους πόρους για να μετακινήσετε και τη συνδρομή για να μετακινήσετε.

## <a name="to-migrate-a-sql-database-into-azure"></a>Να κάνετε μετεγκατάσταση σε μια βάση δεδομένων SQL στο Azure
- Καθορισμός συμβατότητας της βάσης δεδομένων και, στη συνέχεια, επιλέξτε τη μέθοδο δεξιά μετεγκατάστασης με βάση τις ανάγκες σας. Ακολουθήστε τις οδηγίες και τις επιλογές στη [μετεγκατάσταση μιας βάσης δεδομένων SQL Server](sql-database-cloud-migrate.md).

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>Για να δημιουργήσετε ένα αντίγραφο της βάσης δεδομένων για χρήση εκτός του Azure
- [Εξαγωγή σε αρχείο BACPAC.](sql-database-export.md)
