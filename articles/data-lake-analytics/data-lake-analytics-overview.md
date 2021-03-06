<properties 
   pageTitle="Επισκόπηση της ανάλυσης λίμνης δεδομένων Microsoft Azure | Azure" 
   description="Ανάλυση λίμνης δεδομένων είναι μια υπηρεσία κατά τον υπολογισμό Azure Big Data που σας επιτρέπει να χρησιμοποιήσετε δεδομένα για να καθοδηγήσετε την επιχείρησή σας χρησιμοποιώντας τις πληροφορίες από τα δεδομένα σας στο cloud, ανεξάρτητα από το σημείο όπου είναι και ανεξάρτητα από το μέγεθός του. Ανάλυση λίμνης δεδομένων επιτρέπει αυτό στο απλούστερος, πιο μεταβλητού μεγέθους και πιο οικονομική τρόπο. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Επισκόπηση της ανάλυσης λίμνης δεδομένων Microsoft Azure

## <a name="what-is-azure-data-lake-analytics"></a>Τι είναι η ανάλυση λίμνης Azure δεδομένων;

Ανάλυση λίμνης Azure δεδομένων είναι μια νέα υπηρεσία, ενσωματωμένη για να διευκολύνετε την ανάλυση δεδομένων μεγάλο. Αυτή η υπηρεσία σάς επιτρέπει να μπορείτε να εστιάσετε στην εγγραφή, εκτελεί και τη διαχείριση των εργασιών και όχι λειτουργικό κατανέμεται υποδομή. Αντί για την ανάπτυξη, τη ρύθμιση των παραμέτρων και συντονισμού υλικού, μπορείτε να συντάξετε ερωτήματα για να μετατρέψετε τα δεδομένα σας και εξαγωγή πολύτιμο ιδέες. Η υπηρεσία analytics μπορεί να χειρίζεται έργα της κλίμακας οποιαδήποτε αμέσως ορίζοντας την κλήση σύνδεσης για τον όγκο power πρέπει απλώς. Μόνο πληρώνετε για την εργασία σας όταν εκτελείται καθιστώντας οικονομικός. Η υπηρεσία analytics υποστηρίζει Azure Active Directory, επιτρέποντάς σας να απλώς διαχείριση της πρόσβασης και τους ρόλους, ενσωματωμένο με το σύστημα ταυτοτήτων εσωτερικής εγκατάστασης. Περιλαμβάνει επίσης U-SQL, μια γλώσσα που ενοποιεί τα πλεονεκτήματα της SQL με την εκφραστικές ισχύ του κωδικού χρήστη. U SQL μεταβλητού μεγέθους κατανέμεται runtime σάς επιτρέπει να αποτελεσματική ανάλυση δεδομένων στο χώρο αποθήκευσης και στους διακομιστές SQL Azure, βάση δεδομένων SQL Azure και αποθήκη δεδομένων του SQL Azure.


## <a name="key-capabilities"></a>Βασικές δυνατότητες

- **Δυναμική κλιμάκωση** 

    Ανάλυση λίμνης δεδομένων είναι σχεδιασμένος από το μηδέν για cloud κλίμακα και επιδόσεων.  Δυναμικά διατάξεις πόρους και σας επιτρέπει να εκτελέσετε ανάλυση σε terabytes ή ακόμα και μεγέθους exabyte των δεδομένων. Όταν ολοκληρωθεί η εργασία, το ανέμων προς τα κάτω πόρους αυτόματα και πληρώνετε μόνο για το power επεξεργασίας που χρησιμοποιείται. Όπως μπορείτε να αυξήσετε ή να μειώσετε το μέγεθος των δεδομένων που είναι αποθηκευμένα ή την ποσότητα του υπολογισμού που χρησιμοποιείται, δεν χρειάζεται να επανεγγραφή κώδικα. Σας επιτρέπει να μπορείτε να εστιάσετε σε επιχειρηματικής λογικής μόνο και όχι από τον τρόπο επεξεργασία και αποθήκευση μεγάλων συνόλων δεδομένων. 

- **Ανάπτυξη πιο γρήγορα, εντοπισμός σφαλμάτων και βελτιστοποίηση αποδοτικότερα χρησιμοποιώντας οικεία εργαλεία**

    Ανάλυση λίμνης δεδομένων έχει πολλά επίπεδα ενοποίηση με το Visual Studio, ώστε να μπορείτε να χρησιμοποιήσετε οικεία εργαλεία για εκτέλεση, τον εντοπισμό σφαλμάτων και με ακρίβεια τον κωδικό. Απεικονίσεις τις εργασίες U-SQL σάς επιτρέπουν να δείτε τον τρόπο εκτέλεσης του κώδικα σε κλίμακα, έτσι ώστε να μπορείτε εύκολα να αναγνωρίσετε επιδόσεων σημεία κυκλοφοριακής συμφόρησης και βελτιστοποίηση κόστους. 

- **U-SQL: απλή και οικεία, πανίσχυρη και επεκτάσιμη**

    Ανάλυση δεδομένων λίμνης περιλαμβάνει U-SQL, μια γλώσσα ερωτημάτων που επεκτείνει την οικεία, απλή, δηλωτικό φύση του SQL με τη εκφραστικές δύναμη του C#. Η γλώσσα U-SQL δημιουργείται το ίδιο κατανέμεται χρόνο εκτέλεσης που προσφέρει τα συστήματα μεγάλο δεδομένων εντός της Microsoft. Εκατομμύρια προγραμματιστές SQL και του .NET τώρα να επεξεργαστείτε και να αναλύσετε όλα τα δεδομένα με τις δεξιότητες που διαθέτουν ήδη.

- **Απρόσκοπτη ενοποιείται με το επενδύσεις IT**

    Ανάλυση λίμνης δεδομένων να χρησιμοποιήσετε τις υπάρχουσες επενδύσεις IT για ταυτότητα, διαχείριση, ασφάλεια και αποθήκευση δεδομένων. Αυτό απλοποιεί διακυβέρνησης δεδομένων και διευκολύνει την επέκταση του τρέχοντος εφαρμογών δεδομένων. Ανάλυση λίμνης δεδομένων είναι συνδεδεμένο με την υπηρεσία καταλόγου Active Directory για τη Διαχείριση χρηστών και δικαιωμάτων και συνοδεύεται από ενσωματωμένη παρακολούθηση και έλεγχος.

- **Οικονομική και κόστους αποτελεσματική**

    Ανάλυση λίμνης δεδομένων είναι μια οικονομική λύση για την εκτέλεση φόρτους εργασίας μεγάλο δεδομένων. Πληρωμή με βάση ανά εργασία κατά την επεξεργασία δεδομένων. Δεν υπάρχει υλικό, άδειες χρήσης ή συμφωνίες συγκεκριμένης υπηρεσίας υποστήριξης απαιτούνται. Το σύστημα κλιμακώνει αυτόματα προς τα επάνω ή προς τα κάτω, όπως η εργασία ξεκινά και ολοκληρωθεί, γεγονός που σημαίνει ότι ποτέ πληρώνετε για περισσότερο από ό, τι χρειάζεστε. 

- **Λειτουργεί με όλα τα δεδομένα σας Azure**

    Ανάλυση δεδομένων λίμνης να εργαστείτε με έναν αριθμό των προελεύσεων δεδομένων Azure: χώρος αποθήκευσης αντικειμένων Blob του Azure, βάση δεδομένων Azure SQL και φυσικά Analytics λίμνης δεδομένων είναι ειδικά βελτιστοποιημένη για να εργαστείτε με το χώρο αποθήκευσης λίμνης δεδομένων Azure - παρέχει το υψηλότερο επίπεδο επιδόσεων, μετάδοσης και parallelization για εσάς φόρτους εργασίας μεγάλο δεδομένων.

## <a name="see-also"></a>Δείτε επίσης

- Γρήγορα αποτελέσματα
    - [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με την πύλη Azure](data-lake-analytics-get-started-portal.md)
    - [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με χρήση του PowerShell Azure](data-lake-analytics-get-started-powershell.md)
    - [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με Azure .NET SDK](data-lake-analytics-get-started-net-sdk.md)
    - [Ανάπτυξη δεσμών ενεργειών U-SQL χρησιμοποιώντας εργαλεία λίμνης δεδομένων για το Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
    - [Γρήγορα αποτελέσματα με το γλώσσας δεδομένων λίμνης ανάλυση U-SQL Azure](data-lake-analytics-u-sql-get-started.md)
    
- U-SQL & ανάπτυξης
    - [Γρήγορα αποτελέσματα με το γλώσσας δεδομένων λίμνης ανάλυση U-SQL Azure](data-lake-analytics-u-sql-get-started.md)
    - [Χρησιμοποιήστε συναρτήσεις παραθύρου U-SQL για τα έργα Azure δεδομένων λίμνης ανάλυσης](data-lake-analytics-use-window-functions.md)
    - [Ανάπτυξη U-SQL που ορίζονται από το χρήστη τελεστές για ανάλυση δεδομένων λίμνης εργασίες](data-lake-analytics-u-sql-develop-user-defined-operators.md)
    
- Διαχείριση
    - [Διαχείριση Azure ανάλυση λίμνης δεδομένων με την πύλη Azure](data-lake-analytics-manage-use-portal.md)
    - [Διαχείριση Azure ανάλυση λίμνης δεδομένων με χρήση του PowerShell Azure](data-lake-analytics-manage-use-powershell.md)
    - [Παρακολούθηση και αντιμετώπιση προβλημάτων του Azure δεδομένων λίμνης ανάλυσης εργασιών με πύλη Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
    - [Πρόσβαση σε αρχεία καταγραφής διαγνωστικών για ανάλυση λίμνης δεδομένων Azure](data-lake-analytics-diagnostic-logs.md)

- Πρόγραμμα εκμάθησης για να ολοκληρωμένες
    - [Χρήση αλληλεπιδραστικών προγράμματα εκμάθησης για ανάλυση λίμνης δεδομένων Azure](data-lake-analytics-use-interactive-tutorials.md)
    - [Ανάλυση αρχεία καταγραφής τοποθεσία Web χρησιμοποιώντας ανάλυση λίμνης δεδομένων Azure](data-lake-analytics-analyze-weblogs.md)

- Επιτρέψτε μας γνωρίζετε τη γνώμη σας
    - [Σχολιασμός μας λίστας εκκρεμοτήτων τεκμηρίωση](data-lake-analytics-documentation-backlog.md)
    - [Υποβάλετε μια αίτηση δυνατότητα](http://aka.ms/adlafeedback)
    - [Λήψη Βοήθειας στα φόρουμ του](http://aka.ms/adlaforums)


