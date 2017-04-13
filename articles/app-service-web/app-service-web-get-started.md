<properties 
    pageTitle="Ανάπτυξη εφαρμογή web της πρώτης Azure μετά από πέντε λεπτά | Microsoft Azure" 
    description="Μάθετε πόσο εύκολο είναι να εκτελείται εφαρμογών web στο εφαρμογής υπηρεσίας για την ανάπτυξη μιας εφαρμογής του δείγματος. Ξεκινήστε κάνοντας πραγματικό ανάπτυξης γρήγορα και να δείτε αμέσως αποτελέσματα." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-web-app-to-azure-in-five-minutes"></a>Ανάπτυξη εφαρμογή web της πρώτης Azure μετά από πέντε λεπτά

Αυτό το πρόγραμμα εκμάθησης σάς βοηθά να αναπτύξετε την εφαρμογή web της πρώτης σε [Azure εφαρμογής υπηρεσίας](../app-service/app-service-value-prop-what-is.md).
Μπορείτε να χρησιμοποιήσετε την εφαρμογή υπηρεσίας για να δημιουργήσετε εφαρμογές web της [εφαρμογής για κινητές συσκευές πίσω τελειώνει](/documentation/learning-paths/appservice-mobileapps/)και [εφαρμογές API](../app-service-api/app-service-api-apps-why-best-platform.md).

Θα πρέπει: 

- Δημιουργήστε μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας.
- Ανάπτυξη κώδικα δείγμα (Επιλέξτε μεταξύ των ASP.NET, PHP, Node.js, Java ή Python).
- Δείτε τον κώδικα που εκτελείται ζωντανή παραγωγή.
- Ενημερώστε την εφαρμογή web της τον ίδιο τρόπο όπως θα κάνατε [push δεσμεύεται Git](https://git-scm.com/docs/git-push).

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- [Git](http://www.git-scm.com/downloads).
- [Azure CLI](../xplat-cli-install.md).
- Ένας λογαριασμός Microsoft Azure. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να [εγγραφείτε για μια δωρεάν δοκιμαστική έκδοση](/pricing/free-trial/?WT.mc_id=A261C142F) ή να [ενεργοποιήσετε το Visual Studio πλεονεκτήματα συνδρομητών](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Μπορείτε να [Δοκιμάσετε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751) χωρίς λογαριασμό Azure. Δημιουργία εφαρμογής starter και αναπαραγωγή με αυτήν για έως και μία ώρα--απαιτείται πιστωτική κάρτα, χωρίς δεσμεύσεις.

## <a name="deploy-a-web-app"></a>Ανάπτυξη εφαρμογής web

Ας αναπτύξετε μια εφαρμογή web σε Azure εφαρμογής υπηρεσίας.

1. Ανοίξτε μια νέα γραμμή εντολών των Windows, το παράθυρο του PowerShell, Linux κελύφους ή terminal OS X. Εκτέλεση `git --version` και `azure --version` για να επαληθεύσετε ότι Git και το Azure CLI είναι εγκατεστημένα στον υπολογιστή σας.

    ![Δοκιμή της εγκατάστασης του CLI εργαλεία για την εφαρμογή web της πρώτης στο Azure](./media/app-service-web-get-started/1-test-tools.png)

    Εάν δεν έχετε εγκαταστήσει τα εργαλεία, ανατρέξτε στο θέμα [προαπαιτούμενα στοιχεία](#Prerequisites) για τις συνδέσεις λήψης.

3. Συνδεθείτε στο Azure ως εξής:

        azure login

    Ακολουθήστε το μήνυμα βοήθειας για να συνεχίσετε τη διαδικασία σύνδεσης.

    ![Συνδεθείτε στο Azure για να δημιουργήσετε την πρώτη εφαρμογή web](./media/app-service-web-get-started/3-azure-login.png)

4. Αλλάξτε το Azure CLI σε κατάσταση λειτουργίας ASM και, στη συνέχεια, ορίστε το χρήστη ανάπτυξης για εφαρμογής υπηρεσίας. Θα αναπτύξετε κώδικα, χρησιμοποιώντας τα διαπιστευτήρια αργότερα.

        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

1. Αλλαγή σε έναν κατάλογο εργασίας (`CD`) και κλωνοποίηση την εφαρμογή δείγμα ως εξής:

        git clone <github_sample_url>

    ![Δημιουργία διπλότυπου εφαρμογή δείγματος κώδικα για την πρώτη εφαρμογή web στο Azure](./media/app-service-web-get-started/2-clone-sample.png)

    Για * &lt;github_sample_url >*, χρησιμοποιήστε μία από τις ακόλουθες διευθύνσεις URL, ανάλογα με το πλαίσιο που θέλετε:

    - HTML + CSS + JS: [https://github.com/Azure-Samples/app-service-web-html-get-started.git](https://github.com/Azure-Samples/app-service-web-html-get-started.git)
    - ASP.NET: [https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git](https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git)
    - PHP (CodeIgniter): [https://github.com/Azure-Samples/app-service-web-php-get-started.git](https://github.com/Azure-Samples/app-service-web-php-get-started.git)
    - Node.js (Express): [https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git](https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git)
    - Java: [https://github.com/Azure-Samples/app-service-web-java-get-started.git](https://github.com/Azure-Samples/app-service-web-java-get-started.git)
    - Python (Django): [https://github.com/Azure-Samples/app-service-web-python-get-started.git](https://github.com/Azure-Samples/app-service-web-python-get-started.git)

2. Αλλαγή του αποθετηρίου από την εφαρμογή του δείγματος. Για παράδειγμα:

        cd app-service-web-html-get-started

4. Δημιουργία του πόρου εφαρμογή εφαρμογής υπηρεσίας στο Azure με το όνομα εφαρμογής μοναδικό και ο χρήστης ανάπτυξης που έχετε ρυθμίσει προηγουμένως. Όταν σας ζητηθεί, καθορίστε τον αριθμό της περιοχής που θέλετε.

        azure site create <app_name> --git --gitusername <username>

    ![Δημιουργία του Azure πόρου για την εφαρμογή web της πρώτης στο Azure](./media/app-service-web-get-started/4-create-site.png)

    Εφαρμογή σας έχει δημιουργηθεί στο Azure τώρα. Επίσης, του τρέχοντος καταλόγου είναι συνδεδεμένοι με τη νέα εφαρμογή εφαρμογής υπηρεσίας και προετοιμασία Git ως μια απομακρυσμένη Git.
    Μπορείτε να κάνετε αναζήτηση για τη διεύθυνση URL της εφαρμογής (http://&lt;app_name >. azurewebsites.net) για να δείτε τη σελίδα HTML εκπληκτικές προεπιλεγμένου, αλλά ας στην πραγματικότητα τον κωδικό εκεί Άμεση λήψη.

4. Αναπτύξτε το δείγμα κώδικα για την εφαρμογή του Azure όπως κάνατε push οποιονδήποτε κωδικό με Git. Όταν σας ζητηθεί, χρησιμοποιήστε τον κωδικό πρόσβασης που έχετε ρυθμίσει προηγουμένως.

        git push azure master

    ![Push κώδικα για την πρώτη εφαρμογή web της στο Azure](./media/app-service-web-get-started/5-push-code.png)

    Εάν χρησιμοποιείτε ένα από τα πλαίσια γλώσσα, θα βλέπετε διαφορετικό αποτέλεσμα. `git push`όχι μόνο τοποθετεί κώδικα στο Azure, αλλά ενεργοποιεί επίσης εργασιών ανάπτυξης στο μηχανισμό ανάπτυξης. Εάν έχετε οποιοδήποτε package.json (Node.js) ή requirements.txt (Python) αρχεία στο τη ριζική έργου (αποθετήριο) ή εάν έχετε ένα αρχείο packages.config στο έργο σας ASP.NET, η δέσμη ενεργειών ανάπτυξης επαναφέρει τα απαιτούμενα πακέτα για εσάς. Μπορείτε επίσης να [ενεργοποιήσετε την επέκταση σύνθεσης](web-sites-php-mysql-deploy-use-git.md#composer) για την αυτόματη επεξεργασία composer.json αρχείων στην εφαρμογή PHP.

Συγχαρητήρια, έχετε αναπτύξει την εφαρμογή σας για να Azure εφαρμογής υπηρεσίας.

## <a name="see-your-app-running-live"></a>Ανατρέξτε στο θέμα εφαρμογή σας εκτελείται live

Για να δείτε την εφαρμογή σας εκτελείται live στο Azure, εκτελέσετε αυτήν την εντολή από οποιονδήποτε κατάλογο στο αποθετήριο σας:

    azure site browse

## <a name="make-updates-to-your-app"></a>Κάνετε ενημερώσεις για την εφαρμογή σας

Τώρα, μπορείτε να χρησιμοποιήσετε Git για να προωθήσετε από το ριζικό κατάλογο έργου (αποθετήριο) οποιαδήποτε στιγμή για να κάνετε μια ενημέρωση στην ενεργή τοποθεσία. Να κάνετε τον ίδιο τρόπο όπως όταν αναπτυχθεί κώδικά σας την πρώτη φορά. Για παράδειγμα, κάθε φορά που θέλετε για να προωθήσετε μια νέα αλλαγή που που έχετε ελεγχθεί τοπικά, απλώς εκτελέστε τις ακόλουθες εντολές από το ριζικό κατάλογο του έργου (αποθετήριο):

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Επόμενα βήματα

Βρείτε την προτιμώμενη ανάπτυξης και βήματα για το πλαίσιο γλώσσα:

> [AZURE.SELECTOR]
- [.NET](web-sites-dotnet-get-started.md)
- [PHP](app-service-web-php-get-started.md)
- [Node.js](app-service-web-nodejs-get-started.md)
- [Python](web-sites-python-ptvs-django-mysql.md)
- [Java](web-sites-java-get-started.md)

Εναλλακτικά, κάντε περισσότερα με την πρώτη εφαρμογή web. Για παράδειγμα:

- Δοκιμάσετε [άλλους τρόπους για την ανάπτυξη του κώδικα για Azure](../app-service-web/web-sites-deploy.md). Για παράδειγμα, για να αναπτύξετε μία από τις αποθετήρια GitHub, απλώς επιλέξτε **GitHub** αντί για το **Τοπικό αρχείο φύλαξης Git** στις **Επιλογές ανάπτυξης**.
- Λήψη της εφαρμογής σας Azure για το επόμενο επίπεδο. Έλεγχος ταυτότητας τους χρήστες σας. Κλίμακα βασίστηκε ζήτηση. Ορίστε ορισμένες ειδοποιήσεις επιδόσεων. Με λίγα μόνο κλικ. Ανατρέξτε στο θέμα [Προσθήκη λειτουργικότητας σε πρώτη εφαρμογή web](app-service-web-get-started-2.md).

