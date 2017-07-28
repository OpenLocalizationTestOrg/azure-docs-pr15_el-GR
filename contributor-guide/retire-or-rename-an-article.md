# <a name="steps-to-follow-when-you-retire-or-change-the-name-of-an-acom-technical-article"></a>Βήματα για να παρακολουθήσετε όταν αποσύρετε ή αλλάξτε το όνομα ενός άρθρου τεχνική ACOM

Αυτές οι οδηγίες είναι για τις ΜΜΕ που παρατίθενται ως συντάκτης ενός άρθρου που πρέπει να είναι απόσυρση από την ενότητα τεχνική τεκμηρίωση του azure.microsoft.com. Τα βήματα ισχύουν επίσης εάν ένα αρχείο έχει μετονομαστεί.

Εάν είστε μέλος της κοινότητάς μας Azure και πιστεύετε ότι θα πρέπει να είναι απόσυρση ένα άρθρο για οποιονδήποτε λόγο, στείλτε τα σχόλιά στη ροή σχόλιο Disqus για το άρθρο για να επιτρέψετε ο συντάκτης γνωρίζετε υπάρχει κάποιο πρόβλημα με το άρθρο.

Οι συντάκτες ΜΜΕ πρέπει να ακολουθήσετε αρκετά βήματα που πρέπει να αποσύρετε ομαλά περιεχόμενο, ώστε οι χρήστες της τοποθεσίας Web δεν έχουν μια εσφαλμένη εμπειρία όταν θα σας αποσύρετε περιεχόμενο από την τοποθεσία. Διαγραφή στο άρθρο ή αλλάζοντας το όνομά της, θα πρέπει να το τελευταίο πράγμα που συμβαίνει!

## <a name="step-1-set-the-article-to-no-indexno-follow-and-republish-it-recommended"></a>Βήμα 1: Ορίστε στο άρθρο στα χωρίς ευρετήριο/χωρίς παρακολούθηση και να το δημοσιεύσετε ξανά (συνιστάται)

Το πρώτο πράγμα που πρέπει να κάνετε είναι να αναδημοσιεύετε στο άρθρο ως χωρίς ευρετήριο/χωρίς παρακολούθηση μερικές εβδομάδες πριν διαγράψετε στην πραγματικότητα. Αυτό θεωρείται η βέλτιστη πρακτική εξάσκηση "προ-εργασία" για την ακύρωση περιεχομένου. Αυτή η ενέργεια καταργεί τα στο άρθρο από τα ευρετήρια μηχανισμού αναζήτησης, ώστε τα άτομα δεν θα βρείτε στο άρθρο στην αναζήτηση. [Ανατρέξτε στο θέμα εσωτερική wiki για λεπτομέρειες.](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)

## <a name="step-2-manage-inbound-links-required"></a>Βήμα 2: Διαχείριση εισερχομένων συνδέσεων (απαιτείται)

Για να καθορίσετε εάν θα υπάρχουν εισερχόμενες συνδέσεις δεν ανήκουν στη Microsoft για το περιεχόμενό σας. Συχνά, ιστολόγια, φόρουμ και άλλο περιεχόμενο στο web οδηγεί σε άρθρα. Συχνά, μπορείτε να εργαστείτε με κάτοχοι ιστολογίου για να αλλάξετε αυτές τις συνδέσεις και μπορείτε να καταργήσετε ή να ενημερώνονται οι συνδέσεις από το φόρουμ δημοσιεύσεις. Εργαλεία ανάλυσης Web μπορούν να σας εάν θα υπάρχουν συνδέσεις εισερχομένων μεγάλη κίνηση ίσως χρειαστεί να διαχειριστείτε με αυτόν τον τρόπο.

## <a name="step-3-remove-all-crosslinks-to-the-article-from-the-technical-content-repository-required"></a>Βήμα 3: Κατάργηση όλων crosslinks στο άρθρο από την τεχνική αποθετήριο περιεχομένου (απαιτείται)

Δεν βασίζονται σχετικά με την ανακατεύθυνση για να χειριστείτε crosslinks από άλλα άρθρα. Ενημέρωση ή κατάργηση στο σταυρό αναφέρεται στο άρθρο Διαγραφή ή μετονομασία, συμπεριλαμβανομένων των στα άρθρα που ανήκουν σε άλλα άτομα.

1. Βεβαιωθείτε ότι εργάζεστε σε μια τοπική ενημερωμένο διακλάδωση – εκτέλεση `git pull upstream master` (ή στην κατάλληλη παραλλαγή για αυτήν την εντολή.

2.  Εξετάστε το φάκελο azure-περιεχομένου-τιμή/άρθρα και στο φάκελο azure-περιεχόμενο-τιμή/περιλαμβάνει για οποιαδήποτε τα άρθρα και περιλαμβάνει αυτήν τη σύνδεση για το άρθρο που θέλετε να αποσύρετε και καταργήσετε την crosslinks ή αντικαταστήστε τα με μια κατάλληλη νέα crosslinks. Μπορείτε να χρησιμοποιήσετε μια αναζήτηση και αντικατάσταση για να βρείτε το crosslinks, εάν έχετε εγκαταστήσει το βοηθητικό πρόγραμμα. Εάν δεν το χρησιμοποιείτε, μπορείτε να χρησιμοποιήσετε του Windows PowerShell δωρεάν! Παρακάτω περιγράφεται ο τρόπος χρήσης του PowerShell για να βρείτε το crosslinks:

 μια. Εκκινήστε το Windows PowerShell.

 β. Στη γραμμή εντολών του PowerShell, αλλάξτε στο φάκελο azure-περιεχομένου-pr\articles:

 `cd azure-content-pr\articles`

 c. Πληκτρολογήστε αυτήν την εντολή, που θα εμφανιστούν όλα τα αρχεία που περιέχει μια αναφορά στο άρθρο που θέλετε να διαγράψετε:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Εάν προτιμάτε να στείλετε τη λίστα των ονομάτων αρχείων σε ένα αρχείο κειμένου (σε αυτό psoutput.txt πεζών-κεφαλαίων, Επώνυμο), μπορείτε να:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Προσθήκη και ολοκλήρωση όλων των αλλαγών σας, push τους για να σας διακλάδωσης και δημιουργήστε μια αίτηση ελκυστική για να μετακινήσετε τις αλλαγές σας από το διακλάδωσης το πρωτότυπο υποκατάστημα του κύριου αποθετηρίου.

## <a name="step-4-update-the-fwlink-tool-required"></a>Βήμα 4: Ενημέρωση του εργαλείου σύνδεση (απαιτείται)

Επιλέξτε το εργαλείο σύνδεση για οποιαδήποτε FWLinks που μπορεί να οδηγούν στο άρθρο. Τοποθετήστε το δείκτη οποιαδήποτε FWLinks αντικατάστασης περιεχόμενο; Εάν δεν βρίσκεστε στον το ψευδώνυμο που ανήκει η σύνδεση, συμμετάσχετε σε αυτή. Εάν οι κάτοχοι δεν ενημερώσετε τη σύνδεση, το αρχείο δελτίου με MSCOM να έχουν τη σύνδεση που άλλαξε. Περισσότερες πληροφορίες - [εσωτερικό wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-5-remove-all-crosslinks-to-the-article-from-other-pages-on-azuremicrosoftcom-and-create-a-redirect-for-the-retired-page-if-appropriate-required"></a>Βήμα 5: Κατάργηση όλων crosslinks στο άρθρο από άλλες σελίδες στην azure.microsoft.com και δημιουργήστε μια ανακατεύθυνση για τη σελίδα αποσυρθεί, εάν κατάλληλο (απαιτείται)

Θα πρέπει να εργαστείτε με το άτομο που διατηρεί και ενημερώνει τη σελίδα προορισμού τεκμηρίωση για την υπηρεσία για αυτό το τμήμα. Εάν δεν γνωρίζετε ποιος είναι αυτό το άτομο, επικοινωνήστε με το συνεργάτη περιεχομένου ομάδας. Το άτομο που διατηρεί και ενημερώνει τη σελίδα προορισμού του εγγράφου θα πρέπει να κάνετε δύο πράγματα:

1. Στο Visual Studio, εξετάστε το **ολόκληρο** ACOM λύση web για παραπομπές στο αρχείο για να αποσύρετε. Κατάργηση του παραπομπές ή αντικαταστήστε τα με μια ενημερωμένη αναφοράς. Θα χρειαστεί να καταργήσετε τις συνδέσεις HTML, καθώς και τις συμβολοσειρές σχετικό πόρος για τις συνδέσεις HTML. Περισσότερες πληροφορίες - ανατρέξτε στο θέμα [εσωτερική wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. Εάν υπάρχει ένα άρθρο αντικατάστασης, δημιουργήστε μια ανακατεύθυνση. Περισσότερες πληροφορίες - ανατρέξτε στο θέμα [εσωτερική wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx).

3. Ελέγξτε τις αλλαγές στο χώρο αποθήκευσης.

## <a name="step-6-retire-the-article"></a>Βήμα 6: Αποσύρετε στο άρθρο

Αφού ολοκληρώσετε τα βήματα εκ των προτέρων και αυτές οι αλλαγές είναι ζωντανά, μπορείτε να διαγράψετε το άρθρο από το χώρο αποθήκευσης. 

**Σημαντικό:** Όταν διαγράφετε αρχεία, πρέπει να χρησιμοποιήσετε το `git add --all` εντολή.

## <a name="step-7-remove-links-from-msdn-required"></a>Βήμα 7: Κατάργηση συνδέσεων από το MSDN (απαιτείται)

Ελέγξτε το περιεχόμενο εργαλείο Ερωτήσεις για κατεστραμμένες συνδέσεις στο θέμα ακύρωσης ή που έχουν μετονομαστεί και κατάργηση/επιδιόρθωση τις συνδέσεις σε όλα τα θέματα MSDN που επηρεάζονται.

## <a name="step-8-remove-cached-pages-from-search-engines-only-if-absolutely-necessary"></a>Βήμα 8: Κατάργηση σελίδων στο cache από τους μηχανισμούς αναζήτησης (μόνο εάν είναι απολύτως απαραίτητο)

Κάντε αυτό ΜΌΝΟ εάν το περιεχόμενο που πρέπει να καταργηθούν γρήγορα λόγω ζητημάτων νομικές ή σοβαρές πελατών. Ανά βέλτιστες πρακτικές από το Google, διαγραφές σελίδα Κανονική προτεραιότητα πρέπει απλώς να διαχειρίζεται διεργασίες μηχανισμού αναζήτησης φυσική. Μεταβείτε σε αυτές τις σελίδες web για να καταργήσετε στο cache ιστοσελίδες από τους μηχανισμούς αναζήτησης:

[Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Συνδέσεις Οδηγός των συνεργατών

- [Το άρθρο Επισκόπηση](./../README.md)
- [Ευρετήριο άρθρων με οδηγίες](./contributor-guide-index.md)