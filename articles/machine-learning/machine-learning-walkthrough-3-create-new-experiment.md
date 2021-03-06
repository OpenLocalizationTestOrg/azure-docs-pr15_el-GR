<properties
    pageTitle="Βήμα 3: Δημιουργία ενός νέου έρευνας μηχανικής εκμάθησης | Microsoft Azure"
    description="Βήμα 3 από την ανάπτυξη λύσης πρόβλεψης αναλυτικές οδηγίες: Δημιουργήστε μια νέα έρευνα εκπαίδευση στο Azure μηχανικής εκμάθησης Studio."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Αναλυτικές οδηγίες βήμα 3: Δημιουργία ενός νέου έρευνας Azure μηχανικής εκμάθησης

Αυτό είναι το τρίτο βήμα το αναλυτικές οδηγίες, [ανάπτυξη μιας λύσης πρόβλεψης ανάλυσης στο Azure μηχανικής εκμάθησης](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Δημιουργία χώρου εργασίας μηχανικής εκμάθησης](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Αποστολή υπάρχοντα δεδομένα](machine-learning-walkthrough-2-upload-data.md)
3.  **Δημιουργήστε μια νέα έρευνας**
4.  [Εκπαίδευση και αξιολόγηση των μοντέλων](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Ανάπτυξη της υπηρεσίας web](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Πρόσβαση στην υπηρεσία web](machine-learning-walkthrough-6-access-web-service.md)

----------

Σε αυτές τις οδηγίες στο επόμενο βήμα είναι να δημιουργήσετε μια έρευνα στο μηχανικής εκμάθησης Studio που χρησιμοποιεί το σύνολο δεδομένων σας που έχουν αποσταλεί.  

1.  Στο Studio, κάντε κλικ στο κουμπί **+ ΔΗΜΙΟΥΡΓΊΑ** στο κάτω μέρος του παραθύρου.
2.  Επιλέξτε **ΈΡΕΥΝΑΣ**και, στη συνέχεια, επιλέξτε "Κενή έρευνας". Επιλέξτε το προεπιλεγμένο όνομα έρευνας στο επάνω μέρος του καμβά και μετονομάστε το σε χαρακτηριστικό

    > [AZURE.TIP] Είναι καλό να συμπληρώσετε **Σύνοψη** και μια **Περιγραφή** για την έρευνα, στο παράθυρο **Ιδιότητες** . Αυτές οι ιδιότητες σας δώσει τη δυνατότητα να εγγράφου την έρευνα, έτσι ώστε όλα τα άτομα που εξετάζει αυτό αργότερα θα είναι κατανοητή από την επίτευξη των στόχων και τη μεθοδολογία.

3.  Από την παλέτα λειτουργικής μονάδας στην αριστερή πλευρά της έρευνας του καμβά, αναπτύξτε το στοιχείο **Αποθήκευση συνόλων δεδομένων**.
4.  Εύρεση του συνόλου δεδομένων που δημιουργήσατε στην περιοχή **My συνόλων δεδομένων** και μεταφέρετέ τη στη τον καμβά. Μπορείτε επίσης να βρείτε το σύνολο δεδομένων, πληκτρολογώντας το όνομα στο πλαίσιο **αναζήτησης** επάνω από την παλέτα.  

##<a name="prepare-the-data"></a>Προετοιμασία των δεδομένων
Μπορείτε να προβάλετε των πρώτων 100 γραμμών των δεδομένων και ορισμένα στατιστικά στοιχεία για το σύνολο δεδομένων ολόκληρη τη θύρα εξόδου το σύνολο δεδομένων (το μικρό κύκλο στο κάτω μέρος) και επιλέγοντας **αναπαράσταση**.  

Επειδή το αρχείο δεδομένων δεν συνοδεύεται από επικεφαλίδες στηλών, Studio έδωσε γενικής χρήσης επικεφαλίδες (Στ1, Στηλ2, *κ.λπ.*). Καλή οι επικεφαλίδες δεν είναι απαραίτητες για τη δημιουργία ενός μοντέλου, αλλά κάνουν ευκολότερη την εργασία με τα δεδομένα στο την έρευνα. Επίσης, όταν τελικά θα σας να δημοσιεύσετε αυτό το μοντέλο σε μια υπηρεσία web, τις επικεφαλίδες θα σας βοηθήσει προσδιορισμό των στηλών στο χρήστη της υπηρεσίας.  

Να προσθέσουμε επικεφαλίδες στηλών, χρησιμοποιώντας την [Επεξεργασία μετα-δεδομένων] [ edit-metadata] λειτουργική μονάδα.
Μπορείτε να χρησιμοποιήσετε τα [Μετα-δεδομένα επεξεργασία] [ edit-metadata] λειτουργικής μονάδας για να αλλάξετε μετα-δεδομένα που σχετίζονται με ένα σύνολο δεδομένων. Σε αυτήν την περίπτωση, μπορεί να παρέχει περισσότερα ονόματα φιλικό για επικεφαλίδες στηλών. 

Για να χρησιμοποιήσετε [Επεξεργασία μετα-δεδομένων][edit-metadata], μπορείτε πρώτα να καθορίσετε ποιες στήλες για να τροποποιήσετε (σε αυτήν την περίπτωση, όλες τις.) Στη συνέχεια, καθορίστε τις ενέργειες που πρέπει να εκτελεστούν σε αυτές τις στήλες (σε αυτήν την περίπτωση, η αλλαγή επικεφαλίδες στήλης.)

1.  Από την παλέτα λειτουργικής μονάδας, πληκτρολογήστε "μετα-δεδομένων" στο πλαίσιο **αναζήτησης** . Θα δείτε [Επεξεργασία μετα-δεδομένων] [ edit-metadata] εμφανίζονται στη λίστα λειτουργική μονάδα.
2.  Κάντε κλικ και σύρετε τα [Μετα-δεδομένα επεξεργασία] [ edit-metadata] λειτουργικής μονάδας σε τον καμβά και αποθέστε το κάτω από το σύνολο δεδομένων που έχουμε προσθέσει προηγουμένως.
3.  Σύνδεση του συνόλου δεδομένων για να τα [Επεξεργαστείτε μετα-δεδομένων][edit-metadata]: κάντε κλικ στη θύρα εξόδου το σύνολο δεδομένων (το μικρό κύκλο στο κάτω μέρος του συνόλου δεδομένων.) Στη συνέχεια, σύρετε τη θύρα εισόδου [Επεξεργασία] μετα-δεδομένων[ edit-metadata] (το μικρό κύκλο στο επάνω μέρος της λειτουργικής μονάδας), στη συνέχεια, αφήστε το κουμπί του ποντικιού. Το σύνολο δεδομένων και λειτουργική μονάδα παραμένουν συνδεδεμένοι ακόμα και αν μετακίνηση είτε μέσα στον καμβά.

    Η έρευνα πρέπει τώρα να μοιάζει κάπως έτσι:  

    ![Προσθήκη Επεξεργασία μετα-δεδομένων][2]
    
    Το κόκκινο θαυμαστικό δηλώνει ότι θα σας δεν έχετε ρυθμίσει τις ιδιότητες για αυτήν τη λειτουργική μονάδα ακόμη. Θα κάνουμε αυτό Επόμενο.
    
    > [AZURE.TIP] Μπορείτε να προσθέσετε ένα σχόλιο σε μια λειτουργική μονάδα κάνοντας διπλό κλικ στη λειτουργική μονάδα και εισαγωγή κειμένου. Αυτό μπορεί να σας βοηθήσει να δείτε με μια ματιά τι κάνει τη λειτουργική μονάδα στο σας έρευνας. Σε αυτήν την περίπτωση, κάντε διπλό κλικ σε [Επεξεργασία μετα-δεδομένων] [ edit-metadata] λειτουργική μονάδα και πληκτρολογήστε το σχόλιο "Προσθήκη επικεφαλίδες στηλών". Κάντε κλικ οπουδήποτε αλλού στον καμβά για να κλείσετε το πλαίσιο κειμένου. Για να εμφανίσετε το σχόλιο, κάντε κλικ στο βέλος προς τα κάτω στη λειτουργική μονάδα.

4.  Επιλέξτε [Επεξεργασία μετα-δεδομένων][edit-metadata], στη συνέχεια, στο παράθυρο **ιδιοτήτων** στα δεξιά του καμβά, κάντε κλικ στην επιλογή **Εκκίνηση επιλογής στήλης**.
5.  Στο παράθυρο διαλόγου **Επιλογή στηλών** , επιλέξτε όλες τις γραμμές στις **Διαθέσιμες στήλες** και κάντε κλικ στην επιλογή > για να μετακινήσετε τις **Επιλεγμένες στήλες**.
Το παράθυρο διαλόγου θα πρέπει να μοιάζει κάπως έτσι: ![επιλογής στήλης με όλες τις στήλες επιλεγμένο][4]
7.  Επιλέξτε το σημάδι επιλογής **OK** .
8.  Πίσω στο παράθυρο **Ιδιότητες** , αναζητήστε την παράμετρο **νέα ονόματα στηλών** . Σε αυτό το πεδίο, εισαγάγετε μια λίστα με τα ονόματα για τις 21 στήλες του συνόλου δεδομένων, διαχωρισμένες με κόμμα και με τη σειρά στηλών. Μπορείτε να αποκτήσετε τα ονόματα στηλών από την τεκμηρίωση του συνόλου δεδομένων στην τοποθεσία Web του UCI ή για τη διευκόλυνσή σας, μπορείτε να αντιγράψετε και να επικολλήσετε την παρακάτω λίστα:  

          Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    Το παράθυρο Ιδιότητες θα μοιάζει κάπως έτσι:

    ![Ιδιότητες για επεξεργασία μετα-δεδομένων][1]

> [AZURE.TIP] Εάν θέλετε να επαληθεύσετε τις επικεφαλίδες στηλών, εκτελέστε την έρευνα (κάντε κλικ στην επιλογή **ΕΚΤΈΛΕΣΗ** κάτω από τον καμβά έρευνας). Όταν ολοκληρωθεί η εκτέλεση (ένα πράσινο σημάδι ελέγχου θα εμφανίζεται στην [Επεξεργασία μετα-δεδομένων][edit-metadata]), κάντε κλικ στη θύρα εξόδου την [Επεξεργασία μετα-δεδομένων] [ edit-metadata] λειτουργική μονάδα και επιλέξτε **αναπαράσταση**. Μπορείτε να προβάλετε το αποτέλεσμα της οποιαδήποτε λειτουργική μονάδα με τον ίδιο τρόπο για να δείτε την πρόοδο των δεδομένων μέσω του έρευνας.

##<a name="create-training-and-test-datasets"></a>Δημιουργία εκπαιδευτικού και να ελέγξετε συνόλων δεδομένων

Είναι το επόμενο βήμα της την έρευνα για να δημιουργήσετε ξεχωριστές συνόλων δεδομένων που θα χρησιμοποιήσουμε για εκπαίδευση και δοκιμές μας μοντέλο.

Για να το κάνετε αυτό, χρησιμοποιούμε τα [Δεδομένα διαίρεσης] [ split] λειτουργική μονάδα.  

1.  Βρείτε τα [Δεδομένα διαίρεσης] [ split] λειτουργική μονάδα, σύρετέ το στη τον καμβά και συνδέστε τη με τα τελευταία [Επεξεργασία μετα-δεδομένων] [ edit-metadata] λειτουργική μονάδα.
2.  Από προεπιλογή, ο λόγος διαίρεσης είναι 0,5 και έχει οριστεί η παράμετρος **Randomized διαίρεση** . Αυτό σημαίνει ότι τυχαία μισό από τα δεδομένα εξόδου μέσω μίας θύρας τα [Δεδομένα διαίρεσης] [ split] λειτουργική μονάδα και ήμισυ μέσα από το άλλο. Μπορείτε να προσαρμόσετε αυτά, καθώς και την παράμετρο **τυχαία σπόρων** , για να αλλάξετε τη διαίρεση μεταξύ εκπαίδευση και έλεγχο των δεδομένων. Για αυτό το παράδειγμα, θα σας θα παραμένουν ως-είναι.
    
    > [AZURE.TIP] Η ιδιότητα **κλάσμα των γραμμών του ορίσματος του πρώτου συνόλου δεδομένων εξόδου** προσδιορίζει τον όγκο των δεδομένων είναι εξόδου μέσω της θύρας αριστερό εξόδου. Για παράδειγμα, εάν η αναλογία με 0,7, στη συνέχεια, 70% των δεδομένων είναι εξόδου μέσω της θύρας αριστερή και 30% μέσω της θύρας δεξιά.  
    
3. Κάντε διπλό κλικ τα [Δεδομένα διαίρεσης] [ split] λειτουργική μονάδα και πληκτρολογήστε το σχόλιο, "εκπαίδευση/δοκιμές δεδομένων διαίρεση 50%". 

Μπορούμε να χρησιμοποιήσουμε το εξόδους τα [Δεδομένα διαίρεσης] [ split] λειτουργική μονάδα ωστόσο θα σας αρέσει, αλλά επιλέξτε ας για να χρησιμοποιήσετε το αριστερό αποτέλεσμα όπως εκπαίδευση δεδομένων και το δικαίωμα εξόδου ως δοκιμών δεδομένων.  

Όπως αναφέρθηκε στην τοποθεσία Web του UCI, το κόστος των misclassifying πιστωτικό υψηλό κίνδυνο ως χαμηλής είναι πέντε φορές μεγαλύτερο από το κόστος των misclassifying χαμηλής πιστωτικό κίνδυνο ως υψηλής. Με λογαριασμού για αυτό, θα σας να δημιουργήσετε ένα νέο σύνολο δεδομένων που απεικονίζει αυτή η συνάρτηση κόστους. Στο νέο dataset, κάθε παράδειγμα υψηλό κίνδυνο αναπαράγεται πέντε φορές, ενώ δεν αναπαράγεται κάθε παράδειγμα χαμηλού κινδύνου.   

Μπορούμε να κάνουμε αυτό αναπαραγωγής χρησιμοποιώντας κωδικό R:  

1.  Βρείτε και να σύρετε την [Εκτέλεση δέσμης ενεργειών R] [ execute-r-script] λειτουργικής μονάδας στην έρευνα του καμβά και συνδέστε τη θύρα αριστερό εξόδου τα [Δεδομένα διαίρεσης] [ split] λειτουργικής μονάδας στην αρχική εισόδου θύρα ("Dataset1") από την [Εκτέλεση ενεργειών R] [ execute-r-script] λειτουργική μονάδα.
2. Κάντε διπλό κλικ την [Εκτέλεση δέσμης ενεργειών R] [ execute-r-script] λειτουργική μονάδα και πληκτρολογήστε το σχόλιο, "Ορισμός προσαρμογή κόστους".
2.  Στο παράθυρο **Ιδιότητες** , διαγράψτε το προεπιλεγμένο κείμενο στην παράμετρο **Δέσμης ενεργειών R** και πληκτρολογήστε αυτήν τη δέσμη ενεργειών:

          dataset1 <- maml.mapInputPort(1)
          data.set<-dataset1[dataset1[,21]==1,]
          pos<-dataset1[dataset1[,21]==2,]
          for (i in 1:5) data.set<-rbind(data.set,pos)
          maml.mapOutputPort("data.set")


Πρέπει να το κάνετε αυτό ίδια λειτουργία αναπαραγωγής για κάθε εξόδου τα [Δεδομένα διαίρεσης] [ split] λειτουργικής μονάδας, έτσι ώστε τα δεδομένα εκπαίδευσης και δοκιμών έχουν το ίδιο προσαρμογή κόστους.

1.  Κάντε δεξί κλικ την [Εκτέλεση δέσμης ενεργειών R] [ execute-r-script] λειτουργική μονάδα και επιλέξτε **Αντιγραφή**.
2.  Κάντε δεξί κλικ στον καμβά έρευνας και επιλέξτε **Επικόλληση**.
3.  Σύνδεση στην αρχική εισόδου θύρα από αυτό [Εκτέλεση δέσμης ενεργειών R] [ execute-r-script] λειτουργική μονάδα προς τα δεξιά εξόδου θύρας τα [Δεδομένα διαίρεσης] [ split] λειτουργική μονάδα. 
4.  Στο κάτω μέρος του καμβά, κάντε κλικ στην επιλογή **Εκτέλεση**. 

> [AZURE.TIP] Το αντίγραφο της λειτουργικής μονάδας εκτέλεση δέσμης ενεργειών R περιέχει την ίδια δέσμη ενεργειών ως το αρχικό λειτουργική μονάδα. Όταν κάνετε αντιγραφή και επικόλληση λειτουργικής μονάδας στον καμβά, το αντίγραφο διατηρεί όλες τις ιδιότητες του αρχικού.  

Έρευνα μας τώρα είναι κάπως έτσι:

![Προσθήκη λειτουργικής μονάδας διαίρεσης και δεσμών ενεργειών R][3]

Για περισσότερες πληροφορίες σχετικά με τη χρήση R δέσμες ενεργειών σε δοκιμές σας, ανατρέξτε στο θέμα [επέκταση σας πειραματιστείτε με R](machine-learning-extend-your-experiment-with-r.md).

**Επόμενο: [τρένο και την αξιολόγησή τα μοντέλα](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
