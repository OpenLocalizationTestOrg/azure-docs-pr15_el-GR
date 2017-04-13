<properties 
    pageTitle="Πώς μπορείτε να δημιουργήσετε μια βάση δεδομένων σε DocumentDB | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια βάση δεδομένων με την πύλη ηλεκτρονικής υπηρεσίας για DocumentDB Azure, εξαιρετική γρήγορη, παγκόσμια κλίμακα NoSQL βάση δεδομένων σας." 
    keywords="πώς μπορείτε να δημιουργήσετε μια βάση δεδομένων" 
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="mimig"/>

# <a name="how-to-create-a-database-for-documentdb-using-the-azure-portal"></a>Πώς μπορείτε να δημιουργήσετε μια βάση δεδομένων για DocumentDB με την πύλη Azure

Για να χρησιμοποιήσετε το Microsoft Azure DocumentDB, πρέπει να έχετε ένα [λογαριασμό DocumentDB](documentdb-create-account.md), μια βάση δεδομένων, μια συλλογή και έγγραφα. Στην πύλη του Azure, DocumentDB βάσεις δεδομένων τώρα δημιουργούνται την ίδια στιγμή όπως δημιουργείται μια συλλογή. 

Για να δημιουργήσετε μια βάση δεδομένων DocumentDB και συλλογής στην πύλη, δείτε [πώς μπορείτε να δημιουργήσετε μια συλλογή DocumentDB με την πύλη Azure](documentdb-create-collection.md).

## <a name="other-ways-to-create-a-documentdb-database"></a>Άλλοι τρόποι για να δημιουργήσετε μια βάση δεδομένων DocumentDB

Βάσεις δεδομένων δεν χρειάζεται να δημιουργηθούν με την πύλη, μπορείτε επίσης να δημιουργήσετε τους με τη χρήση του [SDK DocumentDB](documentdb-sdk-dotnet.md) ή το [REST API](https://msdn.microsoft.com/library/mt489072.aspx). Για πληροφορίες σχετικά με την εργασία με βάσεις δεδομένων, χρησιμοποιώντας το SDK .NET, δείτε [παραδείγματα βάσης δεδομένων .NET](documentdb-dotnet-samples.md#database-examples). Για πληροφορίες σχετικά με την εργασία με βάσεις δεδομένων, χρησιμοποιώντας το SDK Node.js, δείτε [παραδείγματα Node.js βάσης δεδομένων](documentdb-nodejs-samples.md#database-examples). 

## <a name="next-steps"></a>Επόμενα βήματα

Μετά τη δημιουργία της βάσης δεδομένων και τη συλλογή, μπορείτε να [προσθέσετε έγγραφα JSON](documentdb-view-json-document-explorer.md) , χρησιμοποιώντας την Εξερεύνηση εγγράφων στην πύλη, [Εισαγωγή εγγράφων](documentdb-import-data.md) στη συλλογή, χρησιμοποιώντας το εργαλείο μετεγκατάστασης DocumentDB δεδομένων, ή χρησιμοποιήστε ένα από τα [DocumentDB SDK](documentdb-sdk-dotnet.md) για να εκτελέσετε λειτουργίες CRUD. DocumentDB έχει .NET, Java, Python, Node.js και JavaScript API SDK. Για δείγματα κώδικα .NET που δείχνει πώς να δημιουργήσετε, κατάργηση, ενημέρωση και διαγραφή εγγράφων, ανατρέξτε στο θέμα [.NET παραδείγματα εγγράφου](documentdb-dotnet-samples.md#document-examples). Για πληροφορίες σχετικά με την εργασία με έγγραφα, χρησιμοποιώντας το SDK Node.js, δείτε [παραδείγματα Node.js εγγράφου](documentdb-nodejs-samples.md#document-examples). 

Αφού έχετε έγγραφα σε μια συλλογή, μπορείτε να χρησιμοποιήσετε [DocumentDB SQL](documentdb-sql-query.md) για την [Εκτέλεση ερωτημάτων](documentdb-sql-query.md#executing-sql-queries) σε σχέση με τα έγγραφά σας, χρησιμοποιώντας την [Εξερεύνηση ερωτήματος](documentdb-query-collections-query-explorer.md) στην πύλη, το [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)ή ένα από τα [SDK](documentdb-sdk-dotnet.md). 
