<properties
    pageTitle="Ανάπτυξη του τοπικού Git να Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς μπορείτε να ενεργοποιήσετε την τοπική ανάπτυξη Git σε Azure εφαρμογής υπηρεσίας."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="local-git-deployment-to-azure-app-service"></a>Ανάπτυξη του τοπικού Git να Azure εφαρμογής υπηρεσίας

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να αναπτύξετε την εφαρμογή σας σε [Azure εφαρμογής υπηρεσίας] από ένα αποθετήριο Git στον τοπικό σας υπολογιστή. Εφαρμογή υπηρεσίας υποστηρίζει αυτή η προσέγγιση με την επιλογή ανάπτυξης **Τοπικό Git** στην [Πύλη του Azure].  
Πολλές από τις εντολές Git που περιγράφονται σε αυτό το άρθρο εκτελούνται αυτόματα κατά τη δημιουργία μιας εφαρμογής υπηρεσίας εφαρμογών χρησιμοποιώντας το [περιβάλλον γραμμής εντολών Azure] όπως περιγράφεται [εδώ](app-service-web-get-started.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει:

- Git. Μπορείτε να κάνετε λήψη του εγκατάστασης δυαδικό [εδώ](http://www.git-scm.com/downloads).  
- Βασικές γνώσεις Git.
- Ένας λογαριασμός Microsoft Azure. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να [εγγραφείτε για μια δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial) ή να [ενεργοποιήσετε το Visual Studio πλεονεκτήματα συνδρομητών](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751), όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.  

## <a name="Step1"></a>Βήμα 1: Δημιουργήστε ένα τοπικό αποθετήριο δεδομένων

Εκτελέστε τις ακόλουθες εργασίες για να δημιουργήσετε ένα νέο αρχείο φύλαξης Git.

1. Ξεκινήστε ένα εργαλείο γραμμής εντολών, όπως το **GitBash** (Windows) ή το **πάρτι** (κελύφους Unix). Σε λειτουργικό σύστημα OS X συστήματα μπορείτε να αποκτήσετε πρόσβαση γραμμή εντολών μέσω της εφαρμογής **Terminal** .

2. Μεταβείτε στον κατάλογο όπου να βρίσκεται το περιεχόμενο για την ανάπτυξη.

3. Χρησιμοποιήστε την παρακάτω εντολή για να προετοιμάσετε ένα νέο αρχείο φύλαξης Git:

        git init

## <a name="Step2"></a>Βήμα 2: Ολοκλήρωση το περιεχόμενό σας

Εφαρμογή υπηρεσίας υποστηρίζει εφαρμογές που δημιουργήθηκαν σε διάφορες γλώσσες προγραμματισμού. 

1. Εάν το αποθετήριο περιλαμβάνει ήδη περιεχομένου παράλειψη αυτό τοποθετήστε το δείκτη και μετακίνηση στο σημείο 2 παρακάτω. Εάν το αποθετήριο δεδομένων δεν περιέχει ήδη περιεχόμενο απλώς συμπληρώσετε το με ένα αρχείο στατική .html ως εξής: 

    - Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο με το όνομα **index.html** στον ριζικό κατάλογο του αποθετηρίου Git
    - Προσθέστε το ακόλουθο κείμενο, όπως το περιεχόμενο για το index.html αρχείων και να το αποθηκεύσετε: *Git Γεια σας!*
        
2. Από τη γραμμή εντολών, επαληθεύστε ότι είστε κάτω από το ριζικό κατάλογο της σας αποθετήριο Git. Στη συνέχεια, χρησιμοποιήστε την παρακάτω εντολή για να προσθέσετε αρχεία στο αποθετήριο σας:

        git add -A 

4. Στη συνέχεια, αποθηκεύστε τις αλλαγές στο αποθετήριο δεδομένων, χρησιμοποιώντας την ακόλουθη εντολή:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>Βήμα 3: Ενεργοποίηση του αποθετηρίου εφαρμογή εφαρμογής υπηρεσίας

Ακολουθήστε τα παρακάτω βήματα για να ενεργοποιήσετε ένα αποθετήριο Git για την εφαρμογή σας εφαρμογής υπηρεσίας.

1. Συνδεθείτε [πύλη του Azure].

2. Στο blade της εφαρμογής σας εφαρμογής υπηρεσίας, κάντε κλικ στην επιλογή **Ρυθμίσεις > προέλευση ανάπτυξης**. Κάντε κλικ στην **επιλογή προέλευση**, στη συνέχεια, κάντε κλικ στην επιλογή **Τοπικό Git αποθετήριο δεδομένων**και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.  

    ![Τοπική Git αποθετήριο δεδομένων](./media/app-service-deploy-local-git/local_git_selection.png)

3. Εάν αυτή είναι η ώρα πρώτη ρυθμίζετε ένα αποθετήριο στο Azure, πρέπει να δημιουργήσετε τα διαπιστευτήρια σύνδεσής για αυτήν. Θα χρησιμοποιήσετε τους για να συνδεθείτε με το Azure αποθετήριο και push αλλαγές από τον τοπικό χώρο αποθήκευσης Git. Από την εφαρμογή blade, κάντε κλικ στην επιλογή **Ρυθμίσεις > διαπιστευτήρια ανάπτυξης**, στη συνέχεια, ρύθμιση παραμέτρων σας ανάπτυξης όνομα χρήστη και τον κωδικό πρόσβασης. Όταν ολοκληρώσετε τη διαδικασία, κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Βήμα 4: Ανάπτυξη του έργου σας

Χρησιμοποιήστε τα ακόλουθα βήματα για να δημοσιεύσετε την εφαρμογή σας σε εφαρμογής υπηρεσίας με χρήση του τοπικού Git.

1. Στο blade της εφαρμογής σας στην πύλη του Azure, κάντε κλικ στην επιλογή **Ρυθμίσεις > Ιδιότητες** για τη διεύθυνση **Git URL**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **Διεύθυνση URL Git** είναι η απομακρυσμένη αναφορά για την ανάπτυξη σε από τον τοπικό χώρο αποθήκευσης. Θα χρησιμοποιήσετε αυτήν τη διεύθυνση URL στα παρακάτω βήματα.

2. Χρησιμοποιώντας τη γραμμή εντολών, επαληθεύστε ότι βρίσκεστε στη ρίζα της τον τοπικό αποθετήριο Git.

3. Χρήση `git remote` για να προσθέσετε την απομακρυσμένη αναφορά αναφέρεται στη **Διεύθυνση URL Git** από το βήμα 1. Η εντολή σας θα είναι παρόμοιο με το εξής:

        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] Η **Απομακρυσμένη** εντολή προσθέτει μια καθορισμένη αναφορά σε έναν απομακρυσμένο χώρο αποθήκευσης. Σε αυτό το παράδειγμα δημιουργεί μια αναφορά που ονομάζεται 'azure' για αποθετήριο δεδομένων της εφαρμογής σας web.

4. Push το περιεχόμενό σας στην υπηρεσία εφαρμογής χρησιμοποιώντας το νέο **azure** απομακρυσμένο που μόλις δημιουργήσατε.

        git push azure master

    Θα σας ζητηθεί για τον κωδικό πρόσβασης που δημιουργήσατε νωρίτερα, όταν κάνετε επαναφορά διαπιστευτηρίων ανάπτυξης στην πύλη του Azure. Πληκτρολογήστε τον κωδικό πρόσβασης (Σημειώστε ότι Gitbash δεν θα αναπαραγάγει αστερίσκους στην κονσόλα καθώς πληκτρολογείτε τον κωδικό πρόσβασής σας). 
       
5. Επιστροφή στην εφαρμογή σας στην πύλη του Azure. Μια καταχώρηση αρχείου καταγραφής από την πιο πρόσφατη push πρέπει να εμφανίζεται στο το blade **αναπτύξεις** . 

    ![](./media/app-service-deploy-local-git/deployment_history.png)

6. Κάντε κλικ στο κουμπί **Αναζήτηση** στο επάνω μέρος της εφαρμογής blade για να επαληθεύσετε το περιεχόμενο έχει αναπτυχθεί. 
    
## <a name="Step5"></a>Αντιμετώπιση προβλημάτων

Παρακάτω παρουσιάζονται σφάλματα ή προβλήματα συνηθισμένα κατά τη χρήση Git για να δημοσιεύσετε σε μια εφαρμογή της εφαρμογής υπηρεσίας στο Azure:

****

**Σύμπτωμα**: δεν είναι δυνατή η πρόσβαση '[siteURL]': Απέτυχε η σύνδεση στο [scmAddress]

**Αιτία**: αυτό το σφάλμα μπορεί να προκύψει εάν η εφαρμογή δεν είναι εγκατάσταση και λειτουργία.

**Ανάλυση**: Έναρξη της εφαρμογής στην πύλη του Azure. Ανάπτυξη Git δεν λειτουργεί, εκτός εάν η εφαρμογή εκτελείται. 


****

**Σύμπτωμα**: δεν ήταν δυνατή η επίλυση host 'hostname'

**Αιτία**: αυτό το σφάλμα μπορεί να προκύψει εάν τα στοιχεία της διεύθυνσης που εισάγονται κατά τη δημιουργία απομακρυσμένου 'azure' δεν είναι σωστές.

**Ανάλυση**: Χρησιμοποιήστε την `git remote -v` εντολή για να εμφανιστούν όλα τηλεχειριστήρια, μαζί με τη σχετική διεύθυνση URL. Βεβαιωθείτε ότι η διεύθυνση URL για το 'azure' απομακρυσμένο είναι σωστά. Εάν είναι απαραίτητο, καταργήστε και δημιουργήστε ξανά αυτό remote με τη σωστή διεύθυνση URL.

****

**Σύμπτωμα**: δεν υπάρχει αναφορών στο κοινό και δεν έχει καθοριστεί; κάνετε τίποτα. Ίσως θα πρέπει να καθορίσετε μια διακλάδωση όπως 'υπόδειγμα'.

**Αιτία**: αυτό το σφάλμα μπορεί να προκύψει εάν δεν καθορίσετε υποκατάστημα κατά την εκτέλεση μιας git push λειτουργίας και δεν έχετε ορίσει την τιμή push.default που χρησιμοποιείται από το Git.

**Ανάλυση**: εκτέλεση push ξανά τη λειτουργία, που καθορίζει την κύρια διακλάδωση. Για παράδειγμα:

    git push azure master

****

**Σύμπτωμα**: src refspec [branchname] δεν ανταποκρίνονται σε κανένα.

**Αιτία**: αυτό το σφάλμα μπορεί να προκύψει εάν προσπαθήσετε να προωθήσετε σε υποκατάστημα εκτός από το υπόδειγμα στην 'azure' απομακρυσμένο.

**Ανάλυση**: εκτέλεση push ξανά τη λειτουργία, που καθορίζει την κύρια διακλάδωση. Για παράδειγμα:

    git push azure master

****

**Σύμπτωμα**: σφάλμα - αλλαγές δεσμευμένου σε απομακρυσμένο χώρο αποθήκευσης, αλλά την εφαρμογή web της δεν ενημερώνονται.

**Αιτία**: αυτό το σφάλμα μπορεί να προκύψει εάν αναπτύσσετε μια εφαρμογή Node.js που περιέχει ένα αρχείο package.json που καθορίζει επιπλέον απαιτείται λειτουργικές μονάδες.

**Ανάλυση**: πρόσθετα μηνύματα που περιέχουν 'npm ERR!' θα πρέπει να είναι συνδεδεμένοι πριν από αυτό το σφάλμα και μπορούν να παρέχουν περισσότερες σχετικές πληροφορίες σχετικά με την αποτυχία. Ακολουθούν γνωστά αιτίες για αυτό το σφάλμα και το αντίστοιχο 'npm ERR!' μήνυμα:

* **Αρχείο ακατάλληλη package.json**: npm ERR! Δεν ήταν δυνατή η ανάγνωση εξαρτήσεις.

* **Εγγενής λειτουργική μονάδα που δεν διαθέτει μια δυαδική κατανομή για τα Windows**:

    * npm ERR! \`cmd "/ c" "κόμβο gyp αναδόμηση"\` απέτυχε με το 1

        OR

    * npm ERR! [modulename@version]προεγκαταστήσετε: \`κάνετε || gmake\`


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Τεκμηρίωση Git](http://git-scm.com/documentation)
* [Τεκμηρίωση Kudu έργου](https://github.com/projectkudu/kudu/wiki)
* [Συνεχής ανάπτυξη σε Azure εφαρμογής υπηρεσίας](app-service-continuous-deployment.md)
* [Τον τρόπο χρήσης του PowerShell για Azure](../powershell-install-configure.md)
* [Πώς μπορείτε να χρησιμοποιήσετε το περιβάλλον γραμμής εντολών Azure](../xplat-cli-install.md)

[Azure εφαρμογής υπηρεσίας]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Πύλη του Azure]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure περιβάλλον γραμμής εντολών]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
