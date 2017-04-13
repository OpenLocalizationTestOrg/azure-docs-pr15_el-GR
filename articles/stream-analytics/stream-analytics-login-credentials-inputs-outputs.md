<properties 
    pageTitle="Ροή Analytics: Περιστροφή καταγραφής τα διαπιστευτήρια για εισροές και εκροές | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να ενημερώσετε τα διαπιστευτήρια για ανάλυση ροή εισροές και εκροές."
    keywords="τα διαπιστευτήρια σύνδεσής"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Περιστροφή διαπιστευτηρίων για εισροές και εκροές στη ροή εργασιών αναλυτικών στοιχείων

##<a name="abstract"></a>Απόσπασμα
Ανάλυση Azure ροή σήμερα δεν σας επιτρέπει αντικαθιστώντας τα διαπιστευτήρια σε μια εισόδου/εξόδου, ενώ εκτελείται η εργασία.

Κατά την ανάλυση ροή Azure υποστηρίζει συνέχιση μιας εργασίας από την τελευταία Έξοδος, θέλουμε να κάνετε κοινή χρήση ολόκληρη τη διαδικασία για την ελαχιστοποίηση του υστέρηση μεταξύ τη διακοπή και εκκίνηση της εργασίας και περιστροφή τα διαπιστευτήρια σύνδεσης.

##<a name="part-1---prepare-the-new-set-of-credentials"></a>Μέρος 1 - Προετοιμάστε το νέο σύνολο διαπιστευτηρίων:
Αυτό το τμήμα είναι που ισχύουν για το παρακάτω εισόδων/εξόδους:

* Χώρος αποθήκευσης αντικειμένων blob
* Διανομείς συμβάντος
* Βάση δεδομένων SQL
* Χώρος αποθήκευσης πινάκων

Για άλλες εισόδων/εξόδους, συνεχίστε με το μέρος 2.

###<a name="blob-storagetable-storage"></a>Χώρος αποθήκευσης αντικειμένων blob αποθήκευσης/πίνακα
1.  Μεταβείτε στο την επέκταση χώρου αποθήκευσης στην πύλη διαχείρισης Azure:  
![graphic1][graphic1]
2.  Εντοπίστε το χώρο αποθήκευσης που χρησιμοποιούνται από την εργασία σας και μεταβείτε σε αυτήν:  
![graphic2][graphic2]
3.  Κάντε κλικ στην εντολή Διαχείριση πλήκτρα πρόσβασης:  
![graphic3][graphic3]
4.  Μεταξύ του πρωτεύοντος κλειδιού Access και το δευτερεύον κλειδί πρόσβασης, **Επιλέξτε αυτήν που δεν χρησιμοποιείται από την εργασία σας**.
5.  Επισκέψεων αναδημιουργία:  
![graphic4][graphic4]
6.  Αντιγράψτε τον αριθμό-κλειδί που δημιουργήθηκε πρόσφατα:  
![graphic5][graphic5]
7.  Συνεχίστε να μέρος 2.

###<a name="event-hubs"></a>Διανομείς συμβάντος
1.  Μεταβείτε στο θέμα επέκταση της υπηρεσίας Bus στην πύλη διαχείρισης Azure:  
![graphic6][graphic6]
2.  Εντοπίστε το Namespace Bus υπηρεσίας που χρησιμοποιούνται από την εργασία σας και μεταβείτε σε αυτήν:  
![graphic7][graphic7]
3.  Εάν την εργασία σας χρησιμοποιεί μια πολιτική κοινόχρηστη πρόσβαση στην το Namespace Bus υπηρεσίας, μεταβείτε στο βήμα 6  
4.  Μεταβείτε στην καρτέλα συμβάν διανομείς:  
![graphic8][graphic8]
5.  Εντοπίστε την ενότητα συμβάντων που χρησιμοποιούνται από την εργασία σας και μεταβείτε σε αυτήν:  
![graphic9][graphic9]
6.  Μεταβείτε στην καρτέλα ρύθμιση παραμέτρων:  
![graphic10][graphic10]
7.  Στον την αναπτυσσόμενη λίστα Όνομα πολιτικής, εντοπίστε την πολιτική κοινόχρηστο πρόσβασης που χρησιμοποιούνται από την εργασία σας:  
![graphic11][graphic11]
8.  Μεταξύ του πρωτεύοντος κλειδιού και το δευτερεύον κλειδί, **Επιλέξτε αυτήν που δεν χρησιμοποιείται από την εργασία σας**.  
9.  Επισκέψεων αναδημιουργία:  
![graphic12][graphic12]
10. Αντιγράψτε τον αριθμό-κλειδί που δημιουργήθηκε πρόσφατα:  
![graphic13][graphic13]
11. Συνεχίστε να μέρος 2.  

###<a name="sql-database"></a>Βάση δεδομένων SQL

>[AZURE.NOTE] Σημείωση: θα πρέπει να συνδεθείτε με την υπηρεσία βάσης δεδομένων SQL. Πρόκειται να δείχνουν πώς μπορείτε να το κάνετε αυτό με την εμπειρία διαχείρισης στην πύλη διαχείρισης Azure, αλλά μπορείτε να επιλέξετε να χρησιμοποιήσετε καθώς και ορισμένα εργαλείο πλευρά του προγράμματος-πελάτη όπως SQL Server Management Studio.

1.  Μεταβείτε στο την επέκταση βάσεις δεδομένων SQL στην πύλη διαχείρισης Azure:  
![graphic14][graphic14]
2.  Εντοπίστε τη βάση δεδομένων SQL που χρησιμοποιούνται από την εργασία και **κάντε κλικ στο διακομιστή** σύνδεσης στην ίδια γραμμή:  
![graphic15][graphic15]
3.  Κάντε κλικ στην εντολή Διαχείριση:  
![graphic16][graphic16]
4.  Τύπος υπόδειγμα βάσης δεδομένων:  
![graphic17][graphic17]
5.  Πληκτρολογήστε το όνομα χρήστη, τον κωδικό πρόσβασης, και κάντε κλικ στο αρχείο καταγραφής σε:  
![graphic18][graphic18]
6.  Κάντε κλικ στην επιλογή νέο ερώτημα:  
![graphic19][graphic19]
7.  Πληκτρολογήστε το ακόλουθο ερώτημα, αντικαθιστώντας < login_name > με το όνομα χρήστη και αντικατάστασης <enterStrongPasswordHere> με τον νέο κωδικό πρόσβασης:  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  Κάντε κλικ στην επιλογή εκτέλεση:  
![graphic20][graphic20]
9.  Επιστρέψτε στο βήμα 2 και αυτό χρόνου, κάντε κλικ στη βάση δεδομένων:  
![graphic21][graphic21]
10. Κάντε κλικ στην εντολή Διαχείριση:  
![graphic22][graphic22]
11. Πληκτρολογήστε το όνομα χρήστη, τον κωδικό πρόσβασης, και κάντε κλικ στην επιλογή σύνδεσης:  
![graphic23][graphic23]
12. Κάντε κλικ στην επιλογή νέο ερώτημα:  
![graphic24][graphic24]
13. Πληκτρολογήστε το ακόλουθο ερώτημα, αντικαθιστώντας < όνομα_χρήστη > με ένα όνομα με την οποία θέλετε να προσδιορίσετε αυτή τη σύνδεση στο περιβάλλον αυτής της βάσης δεδομένων (μπορείτε να παρέχετε την ίδια τιμή που έχετε δώσει για < login_name >, για παράδειγμα) και αντικαθιστώντας < login_name > με το νέο όνομα χρήστη:  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. Κάντε κλικ στην επιλογή εκτέλεση:  
![graphic25][graphic25]
15. Τώρα θα πρέπει να παρέχετε το νέο χρήστη με το ίδιο τους ρόλους και τα δικαιώματα που είχαν του αρχικού χρήστη.
16. Συνεχίστε να μέρος 2.

##<a name="part-2-stopping-the-stream-analytics-job"></a>Μέρος 2: Διακοπή της ροής εργασίας ανάλυσης
1.  Μεταβείτε στο την επέκταση ροή ανάλυσης στην πύλη διαχείρισης Azure:  
![graphic26][graphic26]
2.  Εντοπίστε την εργασία σας και μεταβείτε σε αυτήν:  
![graphic27][graphic27]
3.  Μεταβείτε στην καρτέλα εισόδων ή στην καρτέλα εξόδους βάση εάν που περιστρέφοντας τα διαπιστευτήρια σε μια εισαγωγής ή στο αποτέλεσμα.  
![graphic28][graphic28]
4.  Κάντε κλικ στην εντολή Διακοπή και επιβεβαιώστε το έργο έχει διακοπεί:  
![graphic29][graphic29] περιμένετε για το έργο για να διακόψετε.
5.  Εντοπίστε το εισόδου/εξόδου που θέλετε να περιστρέψετε διαπιστευτήρια στη και να μεταβείτε σε αυτήν:  
![graphic30][graphic30]
6.  Προχωρήστε στο τμήμα 3.

##<a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Μέρος 3: Επεξεργασία τα διαπιστευτήρια στην εργασία ανάλυσης ροής

###<a name="blob-storagetable-storage"></a>Χώρος αποθήκευσης αντικειμένων blob αποθήκευσης/πίνακα
1.  Βρείτε το πεδίο κλειδί λογαριασμού χώρου αποθήκευσης και επικολλήστε τον αριθμό-κλειδί που δημιουργήθηκε πρόσφατα σε αυτήν:  
![graphic31][graphic31]
2.  Κάντε κλικ στην εντολή Αποθήκευση και επιβεβαιώστε την αποθήκευση των αλλαγών σας:  
![graphic32][graphic32]
3.  Δοκιμή σύνδεσης θα ξεκινήσει αυτόματα όταν αποθηκεύσετε τις αλλαγές σας, βεβαιωθείτε ότι αυτό σημαίνει ότι έχει περάσει με επιτυχία.
4.  Προχωρήστε στο βήμα 4.

###<a name="event-hubs"></a>Διανομείς συμβάντος
1.  Βρείτε το πεδίο κλειδί πολιτικής διανομέα συμβάντων και επικολλήστε τον αριθμό-κλειδί που δημιουργήθηκε πρόσφατα σε αυτήν:  
![graphic33][graphic33]
2.  Κάντε κλικ στην εντολή Αποθήκευση και επιβεβαιώστε την αποθήκευση των αλλαγών σας:  
![graphic34][graphic34]
3.  Δοκιμή σύνδεσης θα ξεκινήσει αυτόματα όταν αποθηκεύσετε τις αλλαγές σας, βεβαιωθείτε ότι έχουν περάσει με επιτυχία.
4.  Προχωρήστε στο βήμα 4.

###<a name="power-bi"></a>Power BI
1.  Κάντε κλικ στην επιλογή ανανέωση άδειας:  
* ![graphic35][graphic35]
* Θα λάβετε την επιβεβαίωση παρακάτω:  
* ![graphic36][graphic36]
2.  Κάντε κλικ στην εντολή Αποθήκευση και επιβεβαιώστε την αποθήκευση των αλλαγών σας:  
![graphic37][graphic37]
3.  Δοκιμή σύνδεσης θα ξεκινήσει αυτόματα όταν αποθηκεύσετε τις αλλαγές σας, βεβαιωθείτε ότι έχει με επιτυχία περάσει.
4.  Προχωρήστε στο βήμα 4.

###<a name="sql-database"></a>Βάση δεδομένων SQL
1.  Βρείτε τα πεδία όνομα χρήστη και τον κωδικό πρόσβασης και επικολλήστε το σύνολο διαπιστευτηρίων που έχουν δημιουργηθεί πρόσφατα στο τους:  
![graphic38][graphic38]
2.  Κάντε κλικ στην εντολή Αποθήκευση και επιβεβαιώστε την αποθήκευση των αλλαγών σας:  
![graphic39][graphic39]
3.  Δοκιμή σύνδεσης θα ξεκινήσει αυτόματα όταν αποθηκεύσετε τις αλλαγές σας, βεβαιωθείτε ότι έχουν περάσει με επιτυχία.  
4.  Προχωρήστε στο βήμα 4.

##<a name="part-4-starting-your-job-from-last-stopped-time"></a>Μέρος 4: Έναρξη εργασίας από την τελευταία φορά που σταμάτησε
1.  Περιηγηθείτε μακριά από το εισόδου/εξόδου:  
![graphic40][graphic40]
2.  Κάντε κλικ στην εντολή έναρξης:  
![graphic41][graphic41]
3.  Επιλέξτε Διακοπή της ώρας της τελευταίας και κάντε κλικ στο κουμπί OK:  
 ![graphic42][graphic42]
4.  Προχωρήστε στο βήμα 5.  

##<a name="part-5-removing-the-old-set-of-credentials"></a>Μέρος 5: Κατάργηση το παλιό σύνολο διαπιστευτηρίων
Αυτό το τμήμα είναι που ισχύουν για το παρακάτω εισόδων/εξόδους:
* Χώρος αποθήκευσης αντικειμένων blob
* Διανομείς συμβάντος
* Βάση δεδομένων SQL
* Χώρος αποθήκευσης πινάκων

###<a name="blob-storagetable-storage"></a>Χώρος αποθήκευσης αντικειμένων blob αποθήκευσης/πίνακα
Επαναλάβετε μέρος 1 για το πλήκτρο πρόσβασης που χρησιμοποιήθηκε κατά την εργασία σας για να ανανεώσετε το πλήκτρο πρόσβασης που δεν χρησιμοποιούνται πλέον.

###<a name="event-hubs"></a>Διανομείς συμβάντος
Επαναλάβετε μέρος 1 για τον αριθμό-κλειδί που χρησιμοποιήθηκε κατά την εργασία σας για να ανανεώσετε το κλειδί που δεν χρησιμοποιείται πλέον.

###<a name="sql-database"></a>Βάση δεδομένων SQL
1.  Επιστρέψτε στο παράθυρο "ερώτημα" από το τμήμα 1 βήμα 7 και πληκτρολογήστε το ακόλουθο ερώτημα, αντικαθιστώντας < previous_login_name > με το όνομα χρήστη που χρησιμοποιήθηκε κατά την εργασία σας:  
`DROP LOGIN <previous_login_name>`  
2.  Κάντε κλικ στην επιλογή εκτέλεση:  
    ![graphic43][graphic43]  

Θα πρέπει να λάβετε την επιβεβαίωση παρακάτω: 

    Command(s) completed successfully.

## <a name="get-help"></a>Λήψη Βοήθειας
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ ανάλυση ροή Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 
