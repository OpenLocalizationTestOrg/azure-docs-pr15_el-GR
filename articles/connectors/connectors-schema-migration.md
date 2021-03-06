<properties
    pageTitle="Με τη μετεγκατάσταση λογικής εφαρμογών σε σχήμα έκδοση 2015-08-01-προεπισκόπηση | Microsoft Azure εφαρμογής υπηρεσίας"
    description="Μπορείτε εύκολα να μετεγκαταστήσετε τις εφαρμογές σας λογικής στην πιο πρόσφατη έκδοση του σχήματος. Απλώς ακολουθήστε τα παρακάτω βήματα."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Με τη μετεγκατάσταση λογικής εφαρμογών για το σχήμα έκδοση 2015-08-01-preview

Για να μετακινήσετε τις υπάρχουσες εφαρμογές λογικής στο νέο σχήμα, κάντε τα εξής:  
1. Ανοίξτε την εφαρμογή λογικής στην πύλη του Azure  
2. Κάντε κλικ στην επιλογή ενημέρωση σχήματος:

 ![Εικονίδιο API][step1]   
Στη σελίδα Update σχήμα εμφανίζει και παρέχει μια σύνδεση σε ένα έγγραφο που παρέχουν λεπτομέρειες σχετικά με τις βελτιώσεις στο νέο σχήμα: ![εικονίδιο API][step2]

>[AZURE.NOTE] Όταν επιλέγετε **Σχήματος Update**, θα σας αυτόματα εκτελέστε τα βήματα μετεγκατάστασης και δώστε το αποτέλεσμα του κώδικα για εσάς. Μπορείτε να το χρησιμοποιήσετε για να ενημερώσετε τον ορισμό, ωστόσο, βεβαιωθείτε ότι ακολουθείτε καλή coding πρακτικές όπως αυτές που περιγράφονται στην παρακάτω ενότητα **βέλτιστες πρακτικές** .

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Βέλτιστες πρακτικές κατά τη μετεγκατάσταση των εφαρμογών σας λογικής στην πιο πρόσφατη έκδοση του σχήματος:  

- Αντιγραφή της μετεγκαταστάθηκαν δέσμης ενεργειών σε μια νέα εφαρμογή λογικής - δεν θα αντικαταστήσετε το παλιό μία μέχρι να έχετε ολοκληρώσει τον έλεγχο και να επιβεβαιώσει την εφαρμογή μετεγκατάστασης λειτουργεί όπως αναμένεται.
- Δοκιμάστε το λογικής εφαρμογή **πριν από την** τοποθέτηση στο παραγωγής
- Μετά την ολοκλήρωση της μετεγκατάστασης, ξεκινήστε ενημέρωση τις εφαρμογές σας λογικής για να χρησιμοποιήσετε τα [διαχειριζόμενα APIs](./apis-list.md) όπου είναι δυνατό. Για παράδειγμα, μπορείτε να ξεκινήσετε να χρησιμοποιείτε v2 Dropbox, whereever χρησιμοποιείτε DropBox v1.


## <a name="whats-next"></a>Τι ακολουθεί
-  [Μάθετε πώς μπορείτε να μετεγκαταστήσετε με μη αυτόματο τρόπο τις εφαρμογές σας λογικής](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






