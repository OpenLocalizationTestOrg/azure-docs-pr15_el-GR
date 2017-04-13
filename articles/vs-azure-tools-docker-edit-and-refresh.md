<properties
   pageTitle="Εντοπισμός σφαλμάτων εφαρμογών σε ένα τοπικό κοντέινερ Docker | Microsoft Azure"
   description="Μάθετε πώς να τροποποιείτε μια εφαρμογή που εκτελείται σε ένα τοπικό κοντέινερ Docker, ανανεώστε το κοντέινερ μέσω επεξεργασίας και ανανέωση και ορίστε τον εντοπισμό σφαλμάτων σε σημεία διακοπής"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/22/2016"
   ms.author="mlearned" />

# <a name="debugging-apps-in-a-local-docker-container"></a>Εντοπισμός σφαλμάτων εφαρμογών σε ένα τοπικό Docker κοντέινερ

## <a name="overview"></a>Επισκόπηση
Τα εργαλεία του Visual Studio για Docker παρέχει μια συνεπή τρόπο για να αναπτύξετε μια και να επικυρώσετε την εφαρμογή σας τοπικά σε ένα κοντέινερ Linux Docker.
Δεν χρειάζεται να κάνετε επανεκκίνηση του κοντέινερ κάθε φορά που κάνετε μια αλλαγή κώδικα.
Σε αυτό το άρθρο θα δείχνουν πώς μπορείτε να χρησιμοποιήσετε τη δυνατότητα "Επεξεργασία και Ανανέωση" για την εκκίνηση μιας εφαρμογής Web ASP.NET βασικές τοπικής κοντέινερ Docker, κάντε τις απαραίτητες αλλαγές και, στη συνέχεια, ανανεώστε το πρόγραμμα περιήγησης για να δείτε αυτές τις αλλαγές.
Αυτό θα εμφανιστούν και πώς μπορείτε να ορίσετε σημεία διακοπής για τον εντοπισμό σφαλμάτων.

> [AZURE.NOTE] Υποστήριξη των Windows κοντέινερ θα σύντομα σε μελλοντική έκδοση

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Τα ακόλουθα εργαλεία πρέπει να εγκατασταθούν.

- [Ενημέρωση 2015 Visual Studio 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- Εγκαταστήστε την [Ενημέρωση 2015 Visual Studio 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [SDK του Microsoft ASP.NET πυρήνα 1.0](https://go.microsoft.com/fwlink/?LinkID=809122)

Για να εκτελέσετε κοντέινερ Docker τοπικά, χρειάζεστε ένα πρόγραμμα-πελάτη του τοπικού docker.
Μπορείτε να χρησιμοποιήσετε την τελική [Εργαλειοθήκη Docker](https://www.docker.com/products/overview#/docker_toolbox) που απαιτεί Hyper-V ώστε να απενεργοποιηθεί ή μπορείτε να χρησιμοποιήσετε το [Docker για την έκδοση Beta του Windows](https://beta.docker.com) το οποίο χρησιμοποιεί το Hyper-V και απαιτεί Windows 10.

Εάν χρησιμοποιείτε εργαλειοθήκη Docker, θα πρέπει να [ρυθμίσετε τις παραμέτρους του προγράμματος-πελάτη Docker](./vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. Δημιουργία εφαρμογής web

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Προσθέστε Docker υποστήριξης

[AZURE.INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]


## <a name="3-edit-your-code-and-refresh"></a>3. Επεξεργασία κώδικα και ανανέωσης

Για να επαναλάβετε γρήγορα τις αλλαγές, μπορείτε να εκκινήσετε την εφαρμογή σας μέσα σε ένα κοντέινερ, και να συνεχίσετε να κάνετε αλλαγές, τις προβάλλει όπως θα κάνατε με Express των υπηρεσιών IIS.

1. Ορίστε τη ρύθμιση παραμέτρων λύσης σε `Debug` και πατήστε το πλήκτρο ** &lt;CTRL + F5 >** για να δημιουργήσετε την εικόνα σας docker και εκτελέστε το τοπικά.

    Όταν η εικόνα κοντέινερ έχει δημιουργηθεί και εκτελείται σε ένα κοντέινερ Docker, Visual Studio θα ξεκινήσετε την εφαρμογή Web στο προεπιλεγμένο πρόγραμμα περιήγησης.
    Εάν χρησιμοποιείτε το πρόγραμμα περιήγησης Microsoft Edge ή αλλιώς έχουν σφάλματα, ανατρέξτε στην ενότητα [Αντιμετώπιση προβλημάτων](vs-azure-tools-docker-troubleshooting-docker-errors.md) .

1. Μεταβείτε στη σελίδα πληροφορίες, δηλαδή όπου θα κάνουμε για να κάνετε αλλαγές μας.

1. Επιστροφή στο Visual Studio και άνοιγμα `Views\Home\About.cshtml`.

1. Προσθέστε το παρακάτω περιεχόμενο HTML στο τέλος του αρχείου και αποθηκεύστε τις αλλαγές.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```

1.  Προβολή στο παράθυρο εξόδου, όταν ολοκληρωθεί η δημιουργία του .NET και να δείτε αυτές τις γραμμές, επιστρέψτε στο πρόγραμμα περιήγησης και ανανεώστε τη σελίδα πληροφορίες.

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.  Έχουν εφαρμοστεί τις αλλαγές σας!

## <a name="4-debug-with-breakpoints"></a>4. εντοπισμός σφαλμάτων με σημείο διακοπής

Συχνά, οι αλλαγές θα χρειαστεί περαιτέρω επιθεώρησης, αξιοποίηση τις δυνατότητες εντοπισμού σφαλμάτων του Visual Studio.

1.  Επιστροφή στο Visual Studio και άνοιγμα`Controllers\HomeController.cs`

1.  Αντικαταστήστε τα περιεχόμενα της μεθόδου About() με τα εξής:

    ```
    string message = "Your application description page from wthin a Container";
    ViewData["Message"] = message;
    ````

1.  Ορίστε ένα σημείο διακοπής στην αριστερή πλευρά της το `string message`... γραμμής.

1.  Πατήσετε ** &lt;F5 >** για να ξεκινήσετε τον εντοπισμό σφαλμάτων.

1.  Μεταβείτε στη σελίδα για να το πετύχετε το σημείο διακοπής.

1.  Μετάβαση στο Visual Studio για να προβάλετε το σημείο διακοπής και έλεγχος την τιμή του μηνύματος.

    ![][2]

##<a name="summary"></a>Σύνοψη

Με [2015 εργαλεία του Visual Studio για Docker](https://aka.ms/DockerToolsForVS), μπορείτε να λάβετε την παραγωγικότητα των τοπικά, εργασία με το ρεαλιστική απόδοση παραγωγής ανάπτυξη μέσα σε ένα κοντέινερ Docker.

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

[Αντιμετώπιση προβλημάτων του Visual Studio Docker ανάπτυξης](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Περισσότερες πληροφορίες σχετικά με Docker με το Visual Studio, τα Windows και Azure

- [Εργαλεία docker για το Visual Studio](http://aka.ms/dockertoolsforvs) - ανάπτυξη του κώδικα πυρήνα .NET σε ένα κοντέινερ
- [Εργαλεία docker για Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - Δημιουργήστε και αναπτύξτε docker κοντέινερ
- [Εργαλεία docker για κώδικα Visual Studio](http://aka.ms/dockertoolsforvscode) - υπηρεσίες γλωσσών για την επεξεργασία αρχείων docker, με περισσότερες e2e σενάρια σύντομα
- [Πληροφορίες κοντέινερ Windows](http://aka.ms/containers)- Windows Server και πληροφορίες διακομιστή νανομετρικής
- [Η υπηρεσία Azure κοντέινερ](https://azure.microsoft.com/services/container-service/) - [Azure κοντέινερ υπηρεσίας περιεχομένου](http://aka.ms/AzureContainerService)
-    Για περισσότερα παραδείγματα της εργασίας με Docker, ανατρέξτε στο θέμα [εργασία με Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) από τη σύνδεση [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [επίδειξη](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Για περισσότερες γρήγορες εκκινήσεις από την επίδειξη HealthClinic.biz, ανατρέξτε στο θέμα [Γρήγορες εκκινήσεις εργαλεία για προγραμματιστές Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Διάφορα εργαλεία Docker

[Ορισμένα εργαλεία εξαιρετική docker (ιστολογίου του Steve Lasker)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Καλή άρθρα

[Εισαγωγή στις Microservices από NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Παρουσιάσεις

- [Ο Steve Lasker: ΣΎΓΚΡΙΣΗ Live Λας Βέγκας 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [Εισαγωγή στις βασικές ASP.NET @ δημιουργία 2016 - όπου έχετε στο επίδειξης](https://channel9.msdn.com/Events/Build/2016/B810)
- [Ανάπτυξη εφαρμογών .NET στο κοντέινερ, 9 καναλιού](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
