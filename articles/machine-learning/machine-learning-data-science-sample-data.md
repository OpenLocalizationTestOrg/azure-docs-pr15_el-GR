<properties 
    pageTitle="Δείγμα δεδομένων σε κοντέινερ αντικειμένων blob του Azure, SQL Server, και ομάδας πίνακες | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να εξερευνήσετε τα δεδομένα που είναι αποθηκευμένα σε διάφορες Azure enviromnents." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Δείγμα δεδομένων σε κοντέινερ αντικειμένων blob του Azure, SQL Server, και ομάδας πινάκων

Σε αυτό έγγραφο συνδέσεις σε θέματα που περιγράφει πώς μπορείτε να το δείγμα δεδομένων που είναι αποθηκευμένο σε μία από τρεις διαφορετικές Azure θέσεις:

- Γίνεται δοκιμή των **αντικειμένων blob του Azure κοντέινερ δεδομένων** κατά τη λήψη μέσω προγραμματισμού και, στη συνέχεια, η δειγματοληψία το με δείγμα Python κώδικα.
- **Δεδομένα του SQL Server** είναι δειγματοληψία χρησιμοποιώντας SQL και τη γλώσσα προγραμματισμού Python. 
- **Hive δεδομένα πίνακα** είναι δειγματοληψία χρήση ερωτημάτων ομάδας.

Το **μενού** παρακάτω συνδέσεις για να τα θέματα που περιγράφουν τον τρόπο δείγμα δεδομένων από κάθε ένα από αυτά τα περιβάλλοντα Azure χώρου αποθήκευσης. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Αυτή η εργασία δειγματοληψία είναι ένα βήμα για τη [Διαδικασία Science δεδομένων ομάδας (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="why-sample-data"></a>Γιατί δείγμα δεδομένων;

Εάν το σύνολο δεδομένων που σχεδιάζετε να αναλύσετε είναι μεγάλο, είναι συνήθως καλή ιδέα να τα δεδομένα για να μειώσετε το μέγεθος μικρότερο αλλά αντιπρόσωπος και πιο εύχρηστη δειγμάτων προς τα κάτω. Αυτό διευκολύνει την κατανόηση των δεδομένων, Εξερεύνηση και η δυνατότητα μηχανικής. Είναι ο ρόλος του για τη διαδικασία ανάλυσης Cortana για να ενεργοποιήσετε την γρήγορα τη δημιουργία πρωτοτύπων του λειτουργίες επεξεργασίας δεδομένων και μηχανικής εκμάθησης μοντέλων.



