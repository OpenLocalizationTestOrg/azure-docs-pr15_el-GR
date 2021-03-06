<properties
   pageTitle="Εισαγωγή στην ενοποίηση καταγραφής Microsoft Azure | Microsoft Azure"
   description="Μάθετε περισσότερα σχετικά με την ενοποίηση του Azure καταγραφής, τις βασικές δυνατότητες και τον τρόπο που λειτουργεί."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="introduction-to-microsoft-azure-log-integration-preview"></a>Εισαγωγή στην ενοποίηση καταγραφής Microsoft Azure (έκδοση Preview)

Μάθετε περισσότερα σχετικά με την ενοποίηση του Azure καταγραφής, τις βασικές δυνατότητες και τον τρόπο που λειτουργεί.

## <a name="overview"></a>Επισκόπηση

Πλατφόρμα ως υπηρεσία (PaaS) και την υποδομή ως μια υπηρεσία (IaaS) που φιλοξενείται στο Azure δημιουργεί μεγάλο όγκο δεδομένων στα αρχεία καταγραφής ασφαλείας. Αυτά τα αρχεία καταγραφής περιέχουν σημαντικές πληροφορίες που μπορούν να παρέχουν πληροφοριών και τις ισχυρές ιδέες σε παραβιάσεις πολιτικής, εσωτερικές και εξωτερικές απειλές, εφαρμογή ρυθμιστικών διατάξεων θέματα και ανωμαλίες στο δίκτυο, κεντρικός υπολογιστής και δραστηριότητα του χρήστη.

Ενοποίηση του Azure καταγραφής σάς επιτρέπει να ενσωματώσετε ανεπεξέργαστα αρχεία καταγραφής από τους πόρους σας Azure σε συστήματα πληροφοριών ασφαλείας και διαχείρισης συμβάντων (SIEM) σας εσωτερικής εγκατάστασης. Ενοποίηση του Azure καταγραφής συλλέγει Διαγνωστικά Azure από το Windows *(WAD)* εικονικές μηχανές, καθώς και διαγνωστικά από λύσεις συνεργατών όπως ένα τείχος προστασίας των εφαρμογών Web (WAF). Αυτή η ενσωμάτωση παρέχει μια ενιαία πίνακα εργαλείων για όλους τους πόρους σας, εσωτερικής εγκατάστασης ή στο cloud, ώστε να μπορείτε να συγκεντρώσετε, συσχετισμός, την ανάλυση και ειδοποίηση για συμβάντα ασφαλείας.

![Ενοποίηση του Azure αρχείου καταγραφής][1]

## <a name="what-logs-can-i-integrate"></a>Ποια αρχεία καταγραφής να ενοποιήσετε;

Azure υπολογίζονται εκτεταμένη καταγραφής για κάθε υπηρεσία του Azure. Αυτά τα αρχεία καταγραφής κατηγοριοποιούνται σε δύο κύριοι τύποι:

- **Αρχεία καταγραφής ελέγχου/διαχείρισης**, που παρέχουν ορατότητα σε τις λειτουργίες Azure διαχείριση πόρων ΔΗΜΙΟΥΡΓΊΑ, ΕΝΗΜΈΡΩΣΗ και ΔΙΑΓΡΑΦΉ. Azure αρχεία καταγραφής ελέγχου είναι ένα παράδειγμα αυτού του τύπου του αρχείου καταγραφής.
- **Αρχεία καταγραφής επίπεδο δεδομένων**, που παρέχουν ορατότητα σε τα συμβάντα που προκύπτουν ως μέρος της χρήσης ενός πόρου Azure. Παραδείγματα αυτού του τύπου του αρχείου καταγραφής είναι το συμβάν Windows σύστημα, η ασφάλεια και αρχεία καταγραφής εφαρμογών σε μια εικονική μηχανή.

Ενοποίηση του Azure καταγραφής αυτήν τη στιγμή υποστηρίζει την ενσωμάτωση αρχείων καταγραφής ελέγχου Azure, εικονική μηχανή αρχεία καταγραφής και ειδοποιήσεις Κέντρο ασφάλειας Azure.

Εάν έχετε ερωτήσεις σχετικά με την ενοποίηση του αρχείου καταγραφής Azure, στείλτε ένα μήνυμα ηλεκτρονικού ταχυδρομείου για να [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το έγγραφο, που έχουν εισαχθεί στο θέμα ενσωμάτωση του Azure καταγραφής. Για να μάθετε περισσότερα σχετικά με την ενοποίηση του Azure καταγραφής και τους τύπους των αρχείων καταγραφής που υποστηρίζονται, ανατρέξτε στα παρακάτω:

- [Ενοποίηση καταγραφής του Microsoft Azure για Azure αρχεία καταγραφής (έκδοση Preview)](https://www.microsoft.com/download/details.aspx?id=53324) – το Κέντρο λήψης για λεπτομέρειες, απαιτήσεις συστήματος και οδηγίες εγκατάστασης ενοποίηση με το Azure καταγραφής.
- [Γρήγορα αποτελέσματα με το Azure καταγραφής ενοποίησης](security-azure-log-integration-get-started.md) - αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί σε εγκατάσταση του Azure καταγραφής ενοποίηση και ενσωμάτωση αρχείων καταγραφής από το Azure χώρου αποθήκευσης, αρχεία καταγραφής ελέγχου Azure και ειδοποιήσεων Κέντρο ασφάλειας.
- [Βήματα ρύθμισης παραμέτρων συνεργάτη](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – αυτή η καταχώρηση ιστολογίου σας δείχνει πώς να ρυθμίσετε τις παραμέτρους ενοποίησης Azure καταγραφής για να εργαστείτε με λύσεις συνεργατών Splunk HP ArcSight και IBM QRadar.
- [Azure καταγραφής ενοποίησης συχνά ζητηθεί ερωτήσεις](security-azure-log-integration-faq.md) - συνήθεις Ερωτήσεις για αυτό παρέχει απαντήσεις σε ερωτήσεις σχετικά με την ενοποίηση του Azure καταγραφής.
- [Κέντρο ασφάλειας ολοκλήρωση ειδοποιήσεις με Azure συνδεθείτε ενοποίησης](../security-center/security-center-integrating-alerts-with-log-integration.md) – αυτού του εγγράφου σας δείχνει πώς να συγχρονίσετε Κέντρο ασφάλειας ειδοποιήσεις, μαζί με τα συμβάντα ασφαλείας εικονικό μηχάνημα που συλλέγονται από Διαγνωστικά Azure και αρχεία καταγραφής ελέγχου Azure, με ανάλυση καταγραφής ή SIEM λύση.
- [Νέες δυνατότητες για Azure Διαγνωστικά και αρχεία καταγραφής ελέγχου Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – αυτή η καταχώρηση ιστολογίου που παρουσιάζει σε αρχεία καταγραφής ελέγχου Azure και άλλες δυνατότητες που σας βοηθούν να αποκτήσει ιδέες σε τις λειτουργίες Azure τους πόρους σας.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
