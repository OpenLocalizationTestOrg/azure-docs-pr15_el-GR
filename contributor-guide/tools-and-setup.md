<properties
pageTitle="Εγκατάσταση και ρύθμιση του εργαλεία για σύνταξη από κοινού με GitHub"
description="Εργαλεία και βήματα για να οριστεί για σύνταξη από κοινού Azure περιεχομένου στο GitHub."
services="contributor-guide"
documentationCenter=""
authors="tysonn"  
manager="carolz" />

<tags
ms.service="contributor-guide"
 ms.devlang=""
 ms.topic="article"
  ms.tgt_pltfrm=""
  ms.workload=""
  ms.date="01/19/2015"
  ms.author="tysonn" />

#<a name="install-and-set-up-tools-for-authoring-in-github"></a>Εγκατάσταση και ρύθμιση του εργαλεία για σύνταξη από κοινού με GitHub

Ακολουθήστε τα βήματα σε αυτό το άρθρο για να ρυθμίσετε εργαλεία για τη συνεισφορά στην τεκμηρίωση του Azure τεχνική. Οι συνεργάτες περιστασιακοί και περιστασιακές πιθανώς να χρησιμοποιήσετε το GitHub περιβάλλον εργασίας Χρήστη που περιγράφονται στο βήμα 2.

Εάν είστε εξοικειωμένοι με τις Git, ενδέχεται να θέλετε να αναθεωρήσετε ορολογία Git: [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary). Επιπλέον, αυτό το νήμα StackOverflow περιέχει ένα γλωσσάρι που θα αντιμετωπίσετε σε βήματα αυτού του συνόλου όρων Git: [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## <a name="contents"></a>Περιεχόμενα

- [Δημιουργία λογαριασμού GitHub και ρύθμιση του προφίλ σας](#create-a-github-account-and-set-up-your-profile)
- [Εγγραφείτε για Disqus](#sign-up-for-disqus)
- [Καθορίστε εάν χρειάζεστε πραγματικά να ακολουθήσετε τα υπόλοιπα βήματα](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [Δικαιώματα στο GitHub](#permissions-in-github)
- [Εγκατάσταση Git για Windows](#install-git-for-windows)
- [Ενεργοποίηση του ελέγχου ταυτότητας δύο παραγόντων](#enable-two-factor-authentication)
- [Εγκαταστήστε ένα πρόγραμμα επεξεργασίας markdown](#install-a-markdown-editor)
- [Ρύθμιση παραμέτρων του Atom](#configure-atom)
- [Το αποθετήριο διαχωρίζονται και αντιγραφή του στον υπολογιστή σας](#fork-the-repository-and-copy-it-to-your-computer)
- [Ρυθμίστε το όνομα χρήστη και ηλεκτρονικό ταχυδρομείο τοπικά](#configure-your-user-name-and-email-locally)
- [Επόμενα βήματα](#next-steps)

## <a name="create-a-github-account-and-set-up-your-profile"></a>Δημιουργία λογαριασμού GitHub και ρύθμιση του προφίλ σας

Για να συνεισφέρετε στο Azure τεχνική περιεχόμενο, θα χρειαστείτε έναν λογαριασμό [GitHub](http://www.github.com) .

Εάν είστε συνεργάτης της Microsoft, πρέπει να ρυθμίσετε το λογαριασμό GitHub, ώστε να με σαφήνεια που προσδιορίζονται ως ένας υπάλληλος της Microsoft. Ρύθμιση του προφίλ σας ως εξής:

- **Εικόνα του προφίλ**: μια εικόνα από εσάς (απαιτείται)
- **Όνομα**: το πρώτο και το τελευταίο όνομα (υποχρεωτικό)
- **Μήνυμα ηλεκτρονικού ταχυδρομείου**: στη διεύθυνση ηλεκτρονικού ταχυδρομείου της Microsoft (προαιρετικά)
- **Εταιρεία**: Microsoft Corporation (απαιτείται)
- **Θέση**: λίστα θέση σας (προαιρετικό)

Το προφίλ σας θα πρέπει να μοιάζει με αυτό το προφίλ:

<p align="center">
 ![Παράδειγμα GitHub προφίλ](./media/tools-and-setup/githubprofile.png)

## <a name="sign-up-for-disqus"></a>Εγγραφείτε για Disqus

Κάθε δημοσιευμένου άρθρου Azure τεχνική έχει μια ροή σχόλιο που παρέχεται από την υπηρεσία Disqus.

 ![Λογότυπο discus](./media/tools-and-setup/discus.png)

Εάν είστε ένας υπάλληλος της Microsoft και, εάν είστε ο συντάκτης του ή ένα συνεργάτη για ένα άρθρο, πρέπει να εγγραφείτε για Disqus, ώστε να μπορείτε να συμμετάσχετε στη ροή σχόλιο για το άρθρο.

1. Εγγραφείτε για ένα λογαριασμό στο [http://www.disqus.com/](http://www.disqus.com/)
2. Συμπληρώστε το προφίλ σας ως εξής:

 - **Πλήρες όνομα**: το πλήρες όνομά σας όπως εμφανίζεται στο σας καταχώρηση βιβλίου διευθύνσεων της Microsoft, καθώς και τις πληροφορίες σε αγκύλες, το οποίο είναι το ψευδώνυμο συν @MSFT. Μορφή: *όνομα επώνυμο [alias@MSFT] *
 - **Θέση**: θέση σας
 - **Σύντομο βιογραφικού**: τον τίτλο

## <a name="determine-whether-you-really-need-to-follow-the-rest-of-these-steps"></a>Καθορίστε εάν χρειάζεστε πραγματικά να ακολουθήσετε τα υπόλοιπα βήματα

Όχι, ίσως χρειαστεί να ακολουθήσετε όλα τα βήματα σε αυτό το άρθρο. Εξαρτάται από το είδος συνεισφορά περιεχομένου που θέλετε ή πρέπει να κάνετε.

###<a name="submit-a-text-only-change-to-an-existing-article"></a>Υποβάλετε μια αλλαγή μόνο κείμενο σε ένα υπάρχον άρθρο

Εάν μόνο πρέπει ή θέλετε να κάνετε ενημερώσεις που περιέχουν κείμενο σε ένα υπάρχον άρθρο, μάλλον δεν χρειάζεται να ακολουθήσετε τα υπόλοιπα βήματα. Μπορείτε να χρησιμοποιήσετε το πρόγραμμα επεξεργασίας του GitHub markdown που βασίζεται στο web για να υποβάλετε τις αλλαγές σας. Απλώς κάντε κλικ στη σύνδεση GitHub στο άρθρο που θέλετε να τροποποιήσετε:

 ![Παράδειγμα GitHub προφίλ](./media/tools-and-setup/contributetogit.png)

 Στη συνέχεια, πατήστε το εικονίδιο επεξεργασίας στην έκδοση GitHub αυτού του άρθρου

 ![Παράδειγμα GitHub προφίλ](./media/tools-and-setup/editicon.PNG)

 Που ανοίγει το πρόγραμμα επεξεργασίας εύκολο για χρήση web που σας διευκολύνει να υποβάλετε τις αλλαγές. Δεν χρειάζεται να ακολουθήσετε τα υπόλοιπα βήματα σε αυτό το άρθρο.

###<a name="all-other-changes"></a>Όλες οι άλλες αλλαγές
Περιβάλλον εργασίας Χρήστη του GitHub υποστηρίζει τη δημιουργία νέων αρχείων και μεταφορά και απόθεση εικόνων. Ωστόσο, όταν εργάζεστε με το περιβάλλον εργασίας Χρήστη, τη Διαχείριση διακλαδώσεις μπορεί να προκαλεί σύγχυση, ώστε να συνήθως συνιστάται να εγκαταστήσετε τα εργαλεία και μάθετε τις εντολές για τη δημιουργία και διαχείριση άρθρα. Εάν θέλετε να χρησιμοποιήσετε το περιβάλλον εργασίας Χρήστη, ανατρέξτε στα θέματα:

- [Δημιουργία αρχείων σε Github](https://github.com/blog/1327-creating-files-on-github)
- [Αποστολή αρχείων σε αποθετήρια σας](https://github.com/blog/2105-upload-files-to-your-repositories)

Για τα ακόλουθα ταξινομήσεις της εργασίας, συνιστάται ιδιαίτερα να εγκαταστήσετε και μάθετε πώς μπορείτε να χρησιμοποιήσετε τα εργαλεία:

 - Πραγματοποίηση σημαντικών αλλαγών για ένα άρθρο
 - Δημιουργία και δημοσίευση νέου άρθρου
 - Προσθήκη νέων εικόνων ή η ενημέρωση εικόνες
 - Ενημέρωση ένα άρθρο σε μια περίοδο ημέρες χωρίς δημοσίευσης αλλαγές κάθε από τις ημέρες
 - Δημιουργία περιεχομένου για μια έκδοση που έχει για να μεταβείτε μια συγκεκριμένη ημέρα σε ένα συγκεκριμένο χρονικό διάστημα

##<a name="permissions-in-github"></a>Δικαιώματα στο GitHub

Οποιοσδήποτε με ένα λογαριασμό GitHub μπορούν να συμβάλλουν Azure τεχνικό περιεχόμενο μέσω μας δημόσια αποθετήριο δεδομένων στο [https://github.com/Azure/azure-content](https://github.com/Azure/azure-content). Δεν υπάρχουν ειδικές δικαιώματα που απαιτούνται.

Εάν είστε μια μμ Microsoft ή writer ποιος εργάζεται στο Azure περιεχομένου, πρέπει να εργάζεστε σε ιδιωτική μας περιεχομένου αποθετήριο, azure-περιεχομένου-; τιμή;. Επισκεφθείτε [https://repos.opensource.microsoft.com](https://repos.opensource.microsoft.com ) για να ζητήσετε από τα δικαιώματα ανάγνωσης που θα σας επιτρέψει να κάνετε εισφορών μέσω του ιδιωτικού repo - είσοδος σε GitHub χρησιμοποιώντας το κουμπί > κάντε κλικ στην επιλογή Azure > κάντε κλικ στην επιλογή **συμμετοχή σε μια ομάδα** ή να **συμμετάσχετε σε μια άλλη ομάδα**, και, στη συνέχεια, αναζητήστε και συμμετοχή σε ομάδα **azure περιεχόμενο ανάγνωσης** .

## <a name="install-git-for-windows"></a>Εγκατάσταση Git για Windows

Εγκατάσταση του Git για Windows από [http://git-scm.com/download/win](http://git-scm.com/download/win). Αυτό το στοιχείο λήψης εγκαθιστά το σύστημα Git έκδοσης στοιχείου ελέγχου και εγκαθιστά Git πάρτι, την εφαρμογή της γραμμής εντολών που θα χρησιμοποιήσετε για να αλληλεπιδράσετε με την τοπική αποθετήριο Git.

Μπορείτε να αποδεχτείτε τις προεπιλεγμένες ρυθμίσεις; Εάν θέλετε τις εντολές για να είναι διαθέσιμη μέσα σε γραμμή εντολών των Windows, ενεργοποιήστε την επιλογή που επιτρέπει την.

<p align="center">
 ![Παράδειγμα GitHub προφίλ](./media/tools-and-setup/gitbashinstall.png)

(Σημείωση: αυτή δεν είναι ίδια με "Github για Windows". "Github για τα Windows" είναι ένα διαφορετικό εργαλείο βάσει Γραφικών που λειτουργούν επίσης εάν θέλετε να διαβάστε περισσότερα για τον εαυτό σας. [https://windows.github.com/](https://windows.github.com/))

## <a name="enable-two-factor-authentication"></a>Ενεργοποίηση του ελέγχου ταυτότητας δύο παραγόντων

Πρέπει να ενεργοποιήσετε έλεγχος ταυτότητας δύο παραγόντων (2FA) στο λογαριασμό σας στο GitHub Εάν εργάζεστε στο ιδιωτικό αποθετήριο περιεχομένου. Απαιτείται στο αποθετήριο ιδιωτικό.

Για να ενεργοποιήσετε αυτό, ακολουθήστε τις οδηγίες στα δύο παρακάτω GitHub θέματα της Βοήθειας:

- [Πληροφορίες για τον έλεγχο ταυτότητας δύο παραγόντων](https://help.github.com/articles/about-two-factor-authentication/)
- [Δημιουργία ενός διακριτικού πρόσβασης για χρήση της γραμμής εντολών](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

Όταν δημιουργείτε το διακριτικό, επιλέξτε τα πεδία που είναι διαθέσιμα στο περιβάλλον εργασίας Χρήστη του δημιουργίας ([λεπτομέρειες σχετικά με κάθε εμβέλεια](https://developer.github.com/v3/oauth/#scopes))

Μετά την ενεργοποίηση 2FA, πρέπει να εισαγάγετε το διακριτικό πρόσβασης αντί για τον κωδικό πρόσβασής σας GitHub στη γραμμή εντολών, όταν προσπαθείτε να αποκτήσετε πρόσβαση σε ένα αποθετήριο GitHub από τη γραμμή εντολών. Το διακριτικό πρόσβασης δεν είναι ο κωδικός ελέγχου ταυτότητας που λαμβάνετε σε ένα μήνυμα κειμένου κατά τη ρύθμιση του 2FA. Πρόκειται για μια μεγάλη συμβολοσειρά που μοιάζει κάπως έτσι: fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee. Μερικά σημειώσεις σχετικά με αυτό:

- Όταν δημιουργείτε το διακριτικό πρόσβασης, αποθηκεύστε το σε ένα αρχείο κειμένου ώστε να είναι εύκολα προσβάσιμο όταν τα χρειάζεστε.

- Αργότερα, όταν θέλετε να επικολλήσετε το διακριτικό, γνωρίζετε υπάρχουν δύο τρόποι για να επικολλήσετε στη γραμμή εντολών:

 - Κάντε κλικ στο εικονίδιο στην επάνω αριστερή γωνία του παραθύρου γραμμής εντολών > Επεξεργασία > Επικόλληση.
 - Κάντε δεξί κλικ στο εικονίδιο στην επάνω αριστερή γωνία του παραθύρου και κάντε κλικ στην επιλογή Ιδιότητες > Επιλογές > κατάσταση λειτουργίας. Αυτό ρυθμίζει τις παραμέτρους της γραμμής εντολών, ώστε να μπορείτε να επικολλήσετε κάνοντας δεξί κλικ στο παράθυρο γραμμής εντολών.

## <a name="install-a-markdown-editor"></a>Εγκαταστήστε ένα πρόγραμμα επεξεργασίας markdown

Θα σας συντάκτης περιεχομένου χρησιμοποιώντας σημειογραφία απλό "markdown" στα αρχεία, προτιμάτε από μιγαδικού "σήμανση" (HTML, XML, κ.λπ.). Επομένως, θα χρειαστεί να εγκαταστήσετε ένα πρόγραμμα επεξεργασίας markdown.

- **Atom**: πιο εμάς χρήση του GitHub Atom markdown επεξεργασίας: [http://atom.io](http://atom.io). Δεν απαιτεί μια άδεια χρήσης για επαγγελματική χρήση. Έχει τον ορθογραφικό έλεγχο.

- **Το Σημειωματάριο**: μπορείτε να χρησιμοποιήσετε το Σημειωματάριο για μια πολύ πιο απλές επιλογή.

- **Prose**: αυτό είναι ένα πρόγραμμα επεξεργασίας markdown ελαφριές, κομψές, ηλεκτρονική και Άνοιγμα προέλευσης που παρέχει μια προεπισκόπηση. Επισκεφθείτε [http://prose.io](http://prose.io) και εγκρίνετε Prose στο αποθετήριο σας.

- **[Κώδικα visual Studio](https://www.visualstudio.com/products/code-vs.aspx)** - της Microsoft καταχώρηση σε αυτόν το χώρο.

## <a name="configure-atom"></a>Ρύθμιση παραμέτρων του Atom

Εάν χρησιμοποιείτε το Atom, θα χρειαστεί να ρυθμίσετε μερικά πράγματα.

- Atom προεπιλογών με τη χρήση του 2 κενά διαστήματα για τις καρτέλες, αλλά Markdown αναμένει 4 κενά διαστήματα. Εάν αφήσετε την προεπιλεγμένη δύο, το άρθρο σας θα καταπληκτική εμφάνιση στην τοπική προεπισκόπηση, αλλά όχι όταν το εισάγεται στο Azure. Επομένως, ρύθμιση παραμέτρων Atom να χρησιμοποιήσετε και 4 διαστήματα - μπορείτε να βρείτε αυτήν τη ρύθμιση στην περιοχή αρχείο > Ρυθμίσεις > ρυθμίσεις του προγράμματος επεξεργασίας > καρτέλα μήκος.
- Πιθανό ότι θα επίσης να θέλετε να ενεργοποιήσετε την αναδίπλωση απαλές σε αυτήν την ενότητα, που εκτελεί την ίδια ως "Αναδίπλωση κειμένου" στο Σημειωματάριο.
- Για να ενεργοποιήσετε την προεπισκόπηση markdown, κάντε κλικ στην επιλογή πακέτων > Προεπισκόπηση Markdown > εναλλαγή προεπισκόπησης. Μπορείτε να χρησιμοποιήσετε το συνδυασμό πλήκτρων Ctrl-Shift-M για να κάνετε εναλλαγή της προεπισκόπησης προβολή HTML.

## <a name="fork-the-repository-and-copy-it-to-your-computer"></a>Το αποθετήριο διαχωρίζονται και αντιγραφή του στον υπολογιστή σας

1. Δημιουργία μιας διακλάδωσης του αποθετηρίου στο GitHub - μεταβείτε στην επάνω δεξιά πλευρά της σελίδας και κάντε κλικ στο κουμπί διακλάδωση. Εάν σας ζητηθεί, επιλέξτε το λογαριασμό σας με τη θέση όπου θα πρέπει να δημιουργηθεί η διακλάδωση. Αυτό δημιουργεί ένα αντίγραφο του αποθετηρίου μέσα στο λογαριασμό σας Git διανομέα. Σε γενικές γραμμές, τεχνική συντάκτες και διαχειριστές πρόγραμμα πρέπει να διακλάδωσης azure-περιεχομένου-τιμή, η ιδιωτική repo. Οι συνεργάτες Κοινότητας πρέπει να διακλάδωσης azure περιεχομένου, τη δημόσια repo. Αρκεί να διαχωρίζονται μία φορά. μετά την πρώτη ρύθμιση, εάν θέλετε να αντιγράψετε το διακλάδωσης σε άλλον υπολογιστή, μόνο πρέπει να εκτελέσετε τις εντολές που ακολουθούν σε αυτήν την ενότητα για να αντιγράψετε το repo στον υπολογιστή σας.  Εάν επιλέξετε να δημιουργήσετε διακλαδώσεις δύο αποθετήρια, θα πρέπει να δημιουργήσετε μια διακλάδωσης για κάθε αποθετήριο δεδομένων.

2. Αντιγράψτε το προσωπικό Access διακριτικού που λάβατε από [https://github.com/settings/tokens](https://github.com/settings/tokens). Μπορείτε να αποδεχτείτε τα προεπιλεγμένα δικαιώματα για το διακριτικό.  Αποθηκεύστε το προσωπικό διακριτικό πρόσβασης σε ένα αρχείο κειμένου για μελλοντική χρήση.

3. Στη συνέχεια, αντιγράψτε το αρχείο φύλαξης με τον υπολογιστή σας με τα διαπιστευτήριά σας ενσωματωμένο στη συμβολοσειρά εντολής.  Για να το κάνετε αυτό, ανοίξτε το πάρτι Git και εκτελέστε το ως διαχειριστής. Στη γραμμή εντολών, πληκτρολογήστε την ακόλουθη εντολή.  Αυτή η εντολή δημιουργεί έναν κατάλογο azure-content(-pr) στον υπολογιστή σας.  Εάν χρησιμοποιείτε την προεπιλεγμένη θέση, θα είναι στο c:\users<your Windows user name>\azure-content(-pr).

Δημόσια repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

Ιδιωτική repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

Για παράδειγμα, αυτή η εντολή κλωνοποίηση μπορεί να μοιάζει κάπως έτσι:

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## <a name="set-remote-repository-connection-and-configure-credentials"></a>Ρύθμιση σύνδεσης απομακρυσμένο χώρο αποθήκευσης και ρύθμιση παραμέτρων διαπιστευτηρίων

Δημιουργήστε μια αναφορά του αποθετηρίου ριζικό εισάγοντας αυτές οι εντολές. Αυτό ρυθμίζει συνδέσεις στο αποθετήριο δεδομένων στο GitHub, έτσι ώστε να μπορείτε να λάβετε τις πιο πρόσφατες αλλαγές στον τοπικό υπολογιστή σας και να προωθήσετε ξανά τις αλλαγές σας σε GitHub. Αυτή η εντολή ρυθμίζει επίσης το διακριτικό τοπικά ώστε δεν χρειάζεται να εισαγάγετε το όνομα και τον κωδικό πρόσβασης κάθε φορά που προσπαθείτε να αποκτήσετε πρόσβαση την επόμενη repo και σας διακλάδωσης σε GitHub.

Δημόσια repo:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

Ιδιωτική repo:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

Αυτό συνήθως διαρκεί αρκετή ώρα. Αφού κάνετε αυτό, θα χρειαστεί να διαχωρίζονται ξανά ή να εισαγάγετε ξανά τα διαπιστευτήριά σας. Θα έχετε μόνο για να αντιγράψετε το διακλαδώσεις σε έναν τοπικό υπολογιστή ξανά, εάν ρυθμίζετε τα εργαλεία σε άλλον υπολογιστή.


## <a name="configure-your-user-name-and-email-locally"></a>Ρυθμίστε το όνομα χρήστη και ηλεκτρονικό ταχυδρομείο τοπικά

Για να εξασφαλίσετε που παρατίθενται σωστά ως ένα συνεργάτη, πρέπει να ρυθμίσετε το όνομα χρήστη και ηλεκτρονικό ταχυδρομείο τοπικά στο Git.

1. Ξεκινήστε πάρτι Git και μετάβαση σε περιεχόμενο azure ή azure-περιεχομένου-τιμή:

   ````
   cd azure-content
   ````

 ή

   ````
   cd azure-content-pr
   ````

2. Ρυθμίστε το όνομα χρήστη, ώστε να ταιριάζει με το όνομά σας όπως ρυθμίζετε στο προφίλ σας GitHub:

    ````
    git config --global user.name "John Doe"
    ````
3. Ρύθμιση παραμέτρων ηλεκτρονικού ταχυδρομείου σας, ώστε να ταιριάζει με το κύριο μήνυμα ηλεκτρονικού ταχυδρομείου που καθορίζεται από το στο προφίλ σας GitHub; Εάν είστε έναν υπάλληλο MSFT, θα πρέπει να μπορεί να είναι διεύθυνση ηλεκτρονικού ταχυδρομείου σας MSFT:

    ````
    git config --global user.email "alias@example.com"
    ````
4. Τύπος `git config -l` και εξετάστε τις τοπικές ρυθμίσεις για να βεβαιωθείτε ότι το όνομα χρήστη και ηλεκτρονικό ταχυδρομείο στη ρύθμιση παραμέτρων είναι σωστές.

##<a name="next-steps"></a>Επόμενα βήματα

- Κατανόηση του τύπου περιεχομένου που ανήκει σε την τεχνική repo περιεχομένου και γνωρίζετε τι δεν ανήκουν. Δείτε τις [οδηγίες περιεχομένου καναλιού](./content-channel-guidance.md)!
- Ακολουθήστε [τα παρακάτω βήματα για να δημιουργήσετε ή να τροποποιήσετε ένα άρθρο και, στη συνέχεια, να την υποβάλετε για τη δημοσίευση](./git-commands-for-master.md).
- Αντιγράψτε [το πρότυπο markdown](../markdown templates/markdown-template-for-new-articles.md) ως βάση για ένα νέο άρθρο.
- Χρησιμοποιήστε [αυτήν τη λίστα ελέγχου για να επιβεβαιώσετε την αίτησή σας ελκυστική θα ικανοποιεί τα κριτήρια ποιότητας](./contributor-guide-pr-criteria.md) για τη συγχώνευση.


###<a name="contributors-guide-navigation"></a>Οι συνεργάτες Οδηγός περιήγησης

- [Το άρθρο Επισκόπηση](./../README.md)
- [Ευρετήριο άρθρων με οδηγίες](./contributor-guide-index.md)



<!--Anchors-->
[Use a customer-friendly voice]: #use-a-customer-friendly-voice
[Consider localization and machine translation]: #consider-localization-and-machine-translation
[other style and voice issues to watch for]: #other-style-and-voice-issues-to-watch-for


[Create a GitHub account and set up your profile]: #create-a-github-account-and-set-up-your-profile
[Determine whether you really need to follow the rest of these steps]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[Permissions in GitHub]: #permissions-in-github
[Install Git for Windows]: #install-git-for-windows
[Enable two-factor authentication]: #enable-two-factor-authentication
[Install a markdown editor]: #install-a-markdown-editor
[Fork the repository and copy it to your computer]: #fork-the-repository-and-copy-it-to-your-computer
[Install git-credential-winstore]: #install-git-credential-winstore
[Sign up for Disqus]: #sign-up-for-disqus
[Configure your user name and email locally]: #configure-your-user-name-and-email-locally
[Next steps]: #next-steps
