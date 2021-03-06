<properties
    pageTitle="Βάση δεδομένων SQL ελαστικότητας χώρου συγκέντρωσης τιμής και απόδοσης"
    description="Πληροφορίες για τις τιμές ειδικά για χώρους συγκέντρωσης ελαστικότητας βάσης δεδομένων."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="05/27/2016"
    ms.author="srinia"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="elastic-database-pool-billing-and-pricing-information"></a>Χρέωση χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων και τις πληροφορίες τιμολόγησης

Τα σύνολα ελαστικότητας βάσης δεδομένων είναι χρεώνονται ανά τα εξής χαρακτηριστικά:

- Ένα χώρο συγκέντρωσης ελαστικότητας χρεώθηκε κατά τη δημιουργία του, ακόμα και όταν δεν υπάρχουν βάσεις δεδομένων στο χώρο συγκέντρωσης.
- Ένα χώρο συγκέντρωσης ελαστικότητας χρεώθηκε ανά ώρα. Αυτή είναι η ίδια μέτρησης συχνότητα όπως για επίπεδα επιδόσεων μία βάσεις δεδομένων.
- Εάν ένα χώρο συγκέντρωσης ελαστικότητας προσαρμόζεται ώστε να έναν νέο αριθμό eDTUs, στη συνέχεια, το χώρο συγκέντρωσης δεν χρεώνεται σύμφωνα με τον νέο αριθμό eDTUS μέχρι να ολοκληρωθεί η λειτουργία αλλαγής μεγέθους. Αυτό ακολουθεί το ίδιο μοτίβο ως αλλάζοντας το επίπεδο επιδόσεων μεμονωμένη βάσεων δεδομένων.


- Την τιμή ενός χώρου συγκέντρωσης ελαστικότητας βασίζεται στον αριθμό των eDTUs του χώρου συγκέντρωσης. Η τιμή ενός χώρου συγκέντρωσης ελαστικότητας είναι ανεξάρτητα από τον αριθμό και αξιοποίηση των ελαστικότητας βάσεων δεδομένων μέσα σε αυτό.
- Price υπολογίζεται από (αριθμός eDTUs χώρο συγκέντρωσης) x (τιμής μονάδας ανά eDTU).

Η τιμή eDTU μονάδας για ένα χώρο συγκέντρωσης ελαστικότητας είναι υψηλότερα από την τιμή μονάδας DTU για μια μεμονωμένη βάση δεδομένων στο ίδιο επίπεδο υπηρεσίας. Για λεπτομέρειες, ανατρέξτε στο θέμα [τις τιμές της βάσης δεδομένων SQL](https://azure.microsoft.com/pricing/details/sql-database/). 


Για να κατανοήσετε τις βαθμίδες eDTUs και της υπηρεσίας, ανατρέξτε στο θέμα [Επιλογές βάσης δεδομένων SQL και επιδόσεων](sql-database-service-tiers.md).

## <a name="next-steps"></a>Επόμενα βήματα

- [Δημιουργία ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool-create-portal.md)
- [Παρακολούθηση και διαχείριση του μεγέθους ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool-manage-portal.md)
- [Επιλογές βάσης δεδομένων SQL και επιδόσεων: Κατανόηση στοιχεία που είναι διαθέσιμα σε κάθε επίπεδο υπηρεσιών](sql-database-service-tiers.md)
- [Δέσμη ενεργειών του PowerShell για τον εντοπισμό βάσεις δεδομένων που είναι κατάλληλη για ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool-database-assessment-powershell.md)
