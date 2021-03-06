<properties
    pageTitle="Λίστα με τις διαθέσιμες γραμμές σύνδεσης και εφαρμογές API | Microsoft Azure εφαρμογής υπηρεσίας"
    description="Διαβάστε σχετικά με τις γραμμές σύνδεσης και εφαρμογές API στο Azure εφαρμογής υπηρεσίας"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Λίστα με τις γραμμές σύνδεσης και εφαρμογές API για να χρησιμοποιήσετε στο εφαρμογές σας λογικής
>[AZURE.NOTE] Αυτή η έκδοση του άρθρου ισχύει για λογική εφαρμογές 2014-12-01-προεπισκόπηση σχήματος έκδοση. Για την έκδοση λογικής εφαρμογές γενικής διαθεσιμότητας (GA), ανατρέξτε στο θέμα [Νέα λίστα γραμμών σύνδεσης](../connectors/apis-list.md).

Μάθετε περισσότερα σχετικά με όλες τις διαθέσιμες γραμμών σύνδεσης και API εφαρμογές που δημιουργήθηκαν από τη Microsoft για να χρησιμοποιήσετε μέσα από τις εφαρμογές σας λογική.

Για πληροφορίες τιμολόγησης μια λίστα με τι περιλαμβάνεται με κάθε επίπεδο υπηρεσιών, ανατρέξτε στο θέμα [Τις τιμές του Azure εφαρμογής υπηρεσίας](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Για να ξεκινήσετε με τις εφαρμογές λογικής πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [App λογικής δοκιμάσετε](https://tryappservice.azure.com/?appservice=logic). Αμέσως, μπορείτε να δημιουργήσετε μια εφαρμογή της λογικής μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

## <a name="core-connectors"></a>Μέγεθος των κύριων γραμμών σύνδεσης
Ο παρακάτω πίνακας παραθέτει όλες τις διαθέσιμες γραμμών σύνδεσης και API εφαρμογές που έχουν δημιουργηθεί από τη Microsoft, που είναι διαθέσιμες ως πυρήνα γραμμές σύνδεσης:

Όνομα | Περιγραφή
--- | ---
[Ο μεταφραστής Bing](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Χρησιμοποιήστε Bing για μετάφραση κειμένου σε άλλη γλώσσα.
[HTTP](app-service-logic-connector-http.md) | Της ακρόασης HTTP ανοίγει ένα τελικό σημείο που λειτουργεί ως ένα διακομιστή HTTP και παρακολουθεί σε εισερχόμενες αιτήσεις HTTP ή HTTPS. Η ενέργεια HTTP δεν απαιτεί μια εφαρμογή API και υποστηρίζεται εγγενώς μέσα σε λογική εφαρμογών.
[Αδράνεια](app-service-logic-connector-slack.md) | Σύνδεση σε αδράνεια και δημοσιεύουν μηνύματα στα κανάλια αδράνειας.


## <a name="enterprise-integration-connectors"></a>Γραμμές σύνδεσης ενοποίησης για μεγάλες επιχειρήσεις
Ο παρακάτω πίνακας παραθέτει όλες τις διαθέσιμες γραμμών σύνδεσης και API εφαρμογές που δημιουργήθηκαν από τη Microsoft διαθέσιμη ως γραμμές σύνδεσης ενοποίησης για μεγάλες επιχειρήσεις:

Όνομα  | Περιγραφή
------------- | -------------
[Κανόνες BizTalk](app-service-logic-use-biztalk-rules.md) | Χρήση κανόνων BizTalk για να ορίσετε και να ελέγξετε της εταιρικής λογικής μέσα σε μια εταιρεία. Πολιτικές επιχειρήσεις μπορούν να ενημερωθούν χωρίς νέα μεταγλώττιση ή χωρίς επανάληψη ανάπτυξης τις σχετικές εφαρμογές.
[Πρόγραμμα εξαγωγής XPath BizTalk](app-service-logic-xpath-extract.md) | Αναζητά και εξάγει δεδομένα από το περιεχόμενο XML με βάση ένα XPath που επιλέγετε.
[Γραμμή σύνδεσης DB2](app-service-logic-connector-db2.md) | Συνδέεται σε ένα IBM DB2 βάση δεδομένων εσωτερικής εγκατάστασης και σε μια εικονική μηχανή Azure εκτελεί λειτουργικό σύστημα Windows. Να αντιστοιχίσετε λειτουργίες Web API και OData API Informix Structured Query Language εντολές. <br/><br/>Δεν υπάρχει εναύσματα. Ενέργειες περιλαμβάνουν πίνακα, επιλέξτε, εισαγωγή, ενημέρωση, διαγραφή και προσαρμοσμένης πρότασης<br/><br/>Αυτή η γραμμή σύνδεσης περιλαμβάνει επίσης το πρόγραμμα-πελάτη Microsoft για DRDA για να συνδεθείτε με ένα διακομιστή Informix σε ένα δίκτυο TCP/IP.
[Αρχείο](app-service-logic-connector-file.md) | Χρήση αυτή η γραμμή σύνδεσης, μπορείτε να συνδεθείτε με το σύστημα αρχείων στην εσωτερική εγκατάσταση ή δίκτυο και εργασίες ολοκλήρωσης διαφορετικό αρχείο, συμπεριλαμβανομένης της αποστολή, διαγραφή, λίστα αρχείων και πολλά άλλα.
[Informix](app-service-logic-connector-informix.md) | Συνδέεται σε μια βάση δεδομένων IBM Informix, εσωτερικής εγκατάστασης και σε ένα Azure εικονικές υπολογιστή που εκτελεί λειτουργικό σύστημα Windows. Να αντιστοιχίσετε λειτουργίες Web API και OData API Informix Structured Query Language εντολές.<br/><br/>Δεν υπάρχει εναύσματα. Ενέργειες περιλαμβάνουν πίνακα, επιλέξτε, εισαγωγή, ενημέρωση, διαγραφή και προσαρμοσμένης πρότασης.<br/><br/>Όταν χρησιμοποιείτε εσωτερικής εγκατάστασης, VPN ή Azure ExpressRoute μπορεί να χρησιμοποιηθεί. Αυτή η γραμμή σύνδεσης περιλαμβάνει επίσης ένα πρόγραμμα-πελάτη Microsoft για DRDA για να συνδεθείτε με ένα διακομιστή Informix σε ένα δίκτυο TCP/IP.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Συνδέεται με εσωτερικής SQL Server ή μια βάση δεδομένων SQL Azure. Μπορείτε να δημιουργήσετε, ενημέρωση, να γρήγορα και να διαγράψετε εγγραφές σε έναν πίνακα βάσης δεδομένων SQL.
MQ | Συνδέει IBM WebSphere MQ διακομιστή έκδοση 8, εσωτερικής εγκατάστασης και σε ένα Azure εικονικές υπολογιστή που εκτελεί λειτουργικό σύστημα Windows. Όταν χρησιμοποιείτε εσωτερικής εγκατάστασης, VPN ή Azure ExpressRoute μπορεί να χρησιμοποιηθεί. Η γραμμή σύνδεσης περιλαμβάνει επίσης το πρόγραμμα-πελάτη Microsoft για MQ.<br/><br/>Δεν υπάρχει εναύσματα. Δεν υπάρχουν ενέργειες.<br/><br/>**Σημείωση** Προς το παρόν δεν μπορούν να χρησιμοποιηθούν με τις εφαρμογές λογικής.

## <a name="connectors-as-triggers"></a>Γραμμές σύνδεσης ως εναύσματα
Πολλές συνδέσεις παρέχουν εναυσμάτων για τις εφαρμογές της λογικής. Αυτά τα εναύσματα είναι με δύο τύπους:

1. Ψηφοφορία εναύσματα: Αυτά τα εναύσματα ψηφοφορία με την υπηρεσία σας σε μια καθορισμένη συχνότητα ελέγχου για νέα δεδομένα. Όταν είναι διαθέσιμα νέα δεδομένα, εκτελείται μια νέα παρουσία της εφαρμογής σας λογικής με τα δεδομένα ως είσοδο. Για να αποτρέψετε που καταναλώνεται τα ίδια δεδομένα πολλές φορές, το έναυσμα μπορεί να εκκαθάρισης δεδομένων που έχει Διαβάστε και που του μεταβιβάστηκε η εφαρμογή λογικής. Παραδείγματα οι γραμμές σύνδεσης είναι αρχείο SQL και αποθήκευσης Azure.
2. Εναύσματα Push: Αυτά τα εναύσματα ακρόαση για δεδομένα σε ένα τελικό σημείο ή ενός συμβάντος για να προκύψει. Έναυσμα, στη συνέχεια, μια νέα παρουσία του μια εφαρμογή λογικής. Παραδείγματα οι γραμμές σύνδεσης είναι ακρόασης HTTP και Twitter.

## <a name="connectors-as-actions"></a>Γραμμές σύνδεσης ως ενέργειες
Γραμμές σύνδεσης μπορεί επίσης να χρησιμοποιηθεί ως ενεργειών μέσα σε εφαρμογή της λογικής. Οι ενέργειες είναι χρήσιμη για να αναζητήσετε δεδομένων μέσα στην εφαρμογή λογικής που μπορούν να χρησιμοποιηθούν, στη συνέχεια, κατά την εκτέλεση. Για παράδειγμα, ίσως χρειαστεί να αναζητήσετε δεδομένα από μια βάση δεδομένων SQL για πρόσθετες πληροφορίες σχετικά με έναν πελάτη κατά την επεξεργασία μιας παραγγελίας. Εναλλακτικά, ίσως χρειαστεί να γράψετε, να ενημερώσετε ή να διαγράψετε δεδομένα σε έναν προορισμό. Μπορείτε να το κάνετε χρησιμοποιώντας τις ενέργειες που παρέχονται από τις συνδέσεις. Ενέργειες αντιστοιχίστε λειτουργίες στις εφαρμογές API (όπως καθορίζεται από τα μετα-δεδομένα Swagger).

## <a name="create-your-own-connectors-and-api-apps"></a>Δημιουργήστε τις δικές σας γραμμές σύνδεσης και εφαρμογές API
[Γραμμές σύνδεσης και API εφαρμογές αναφοράς](http://aka.ms/appservicesconnectorreference)  
[Azure εναύσματα εφαρμογής API της εφαρμογής υπηρεσίας](../app-service-api/app-service-api-dotnet-triggers.md)  
[Αναφορά εφαρμογής λογικής](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>Περισσότερες πληροφορίες σχετικά με συνδέσεις και εφαρμογές API
[Τι είναι οι γραμμές σύνδεσης και BizTalk API εφαρμογές](app-service-logic-what-are-biztalk-api-apps.md)  
[Χρήση της διαχείρισης σύνδεσης υβριδική στο Azure εφαρμογής υπηρεσίας](app-service-logic-hybrid-connection-manager.md)  
[Διαχείριση και την παρακολούθηση τις ενσωματωμένες εφαρμογές API και τις γραμμές σύνδεσης](app-service-logic-monitor-your-connectors.md)
