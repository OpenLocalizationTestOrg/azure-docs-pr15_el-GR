<properties
   pageTitle="Κλιμάκωση υπηρεσίας web | Microsoft Azure"
   description="Μάθετε πώς να κλιμακωθεί μια υπηρεσία web με αύξηση της ταυτόχρονης εκτέλεσης και προσθήκη νέου τελικά σημεία."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="Azure μηχανικής εκμάθησης, υπηρεσίες web, operationalization, κλίμακας, τελικό σημείο, ταυτόχρονης εκτέλεσης"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/05/2016"
   ms.author="neerajkh"/>

# <a name="scaling-a-web-service"></a>Κλιμάκωση μια υπηρεσία Web

>[AZURE.NOTE] Αυτό το θέμα περιγράφει τεχνικές που εφαρμόζονται σε μια υπηρεσία Web εκμάθησης κλασική υπολογιστή. 

Από προεπιλογή, κάθε δημοσιευμένη υπηρεσία Web έχει ρυθμιστεί ώστε να υποστηρίζει 20 ταυτόχρονες αιτήσεις και μπορεί να είναι όσο το 200 ταυτόχρονες αιτήσεις. Ενώ η πύλη του Azure κλασική παρέχει ένας τρόπος για να ορίσετε αυτήν την τιμή, Azure μηχανικής εκμάθησης βελτιστοποιεί αυτόματα τη ρύθμιση για την παροχή της υπηρεσίας web τις καλύτερες επιδόσεις και την τιμή πύλης παραβλέπεται. 

Εάν σχεδιάζετε να καλέσετε το API με υψηλότερη φόρτωση από μια τιμή Max ταυτόχρονες κλήσεις 200 θα υποστηρίζει, πρέπει να δημιουργήσετε πολλές τελικά σημεία στην ίδια υπηρεσία Web. Στη συνέχεια, να διανείμετε τυχαία σας φόρτωση σε όλες τις.

## <a name="add-new-endpoints-for-same-web-service"></a>Προσθήκη νέου τελικά σημεία για την ίδια υπηρεσία web

Η κλιμάκωση της υπηρεσίας Web είναι μια κοινή εργασία. Ορισμένοι λόγοι για να περιορίσετε το μέγεθος των είναι να υποστηρίζει περισσότερα από 200 ταυτόχρονες αιτήσεις, αύξηση Διαθεσιμότητα έως πολλών τελικά σημεία ή παρέχουν ξεχωριστή τελικά σημεία για την υπηρεσία web. Μπορείτε να αυξήσετε την κλίμακα με την προσθήκη επιπλέον τελικά σημεία για την ίδια υπηρεσία Web μέσω του [Azure κλασική πύλη](https://manage.windowsazure.com/) ή την πύλη [Υπηρεσίας Web Azure μηχανικής εκμάθησης](https://services.azureml.net/) .

Για περισσότερες πληροφορίες σχετικά με την προσθήκη νέου τελικά σημεία, ανατρέξτε στο θέμα [Δημιουργία τελικά σημεία](machine-learning-create-endpoint.md).

Έχετε υπόψη ότι με μια μέτρηση υψηλή ταυτόχρονης εκτέλεσης μπορεί να είναι επιβλαβείς εάν που καλείτε δεν το API με αντίστοιχα υψηλή χρέωσης. Ίσως δείτε σποραδική χρονικών ορίων ή/και αιχμές στο το λανθάνων χρόνος Εάν τοποθετήσετε μια σχετικά χαμηλές φόρτωση σε API έχει ρυθμιστεί για υψηλό φόρτο.

Τα API σύγχρονη χρησιμοποιούνται συνήθως στις περιπτώσεις όπου θέλετε να εγκαταστήσετε ένα μικρό λανθάνοντα. Λανθάνων χρόνος εδώ υποδηλώνει την ώρα που χρειάζεται για το API για να ολοκληρώσετε μια αίτηση και δεν λογαριασμού για τυχόν καθυστερήσεων του δικτύου. Ας υποθέσουμε ότι έχετε ένα API με μια λανθάνων χρόνος 50-ms. Για την εκμετάλλευση πλήρως τη διαθέσιμη χωρητικότητα με το επίπεδο επιτάχυνσης υψηλής και Max ταυτόχρονες κλήσεις = 20, πρέπει να καλέσετε αυτό το API 20 * 1000 / 50 = 400 φορές ανά δευτερόλεπτο. Επέκταση αυτό περαιτέρω, ένα μέγιστο ταυτόχρονες κλήσεις 200 σάς επιτρέπει να καλέσετε το API 4000 φορές ανά δευτερόλεπτο, υπό την προϋπόθεση ότι ο λανθάνων χρόνος 50-ms.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
