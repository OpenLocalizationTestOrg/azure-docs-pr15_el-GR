<properties
    pageTitle="Azure εφαρμογής υπηρεσίας web app ρύθμισης παραμέτρων και επεκτάσεις για προχωρημένους"
    description="Χρησιμοποιήστε δηλώσεων Transformation(XDT) έγγραφο XML για να μετατρέψετε το αρχείο ApplicationHost.config σε εφαρμογή web της εφαρμογής υπηρεσίας Azure και για να προσθέσετε ιδιωτικό επεκτάσεις για να ενεργοποιήσετε τη Διαχείριση προσαρμοσμένες ενέργειες."
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Azure εφαρμογής υπηρεσίας web app ρύθμισης παραμέτρων και επεκτάσεις για προχωρημένους

Με τη χρήση δηλώσεων [Μετασχηματισμού έγγραφο XML](http://msdn.microsoft.com/library/dd465326.aspx) (XDT), μπορείτε να μετατρέψετε το αρχείο [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) στην εφαρμογή web στο Azure εφαρμογής υπηρεσίας. Μπορείτε επίσης να χρησιμοποιήσετε δηλώσεων XDT για να προσθέσετε ιδιωτικό επεκτάσεις για να ενεργοποιήσετε την προσαρμοσμένη web app διαχείρισης ενέργειες. Σε αυτό το άρθρο περιλαμβάνει μια επέκταση εφαρμογών web PHP Manager δείγμα που επιτρέπει τη διαχείριση των ρυθμίσεων PHP μέσω περιβάλλον web.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a id="transform"></a>Ρύθμιση παραμέτρων για προχωρημένους μέσω ApplicationHost.config
Την πλατφόρμα εφαρμογής υπηρεσίας παρέχει ευελιξία και έλεγχος για ρύθμιση παραμέτρων της εφαρμογής web. Παρόλο που το τυπικό αρχείο ρύθμισης παραμέτρων των υπηρεσιών IIS ApplicationHost.config δεν είναι διαθέσιμο για επεξεργασία απευθείας στην εφαρμογή υπηρεσίας, την πλατφόρμα υποστηρίζει δηλωτικό ApplicationHost.config μετασχηματισμός μοντέλου που βασίζεται σε μετασχηματισμό έγγραφο XML (XDT).

Για να αξιοποιήσετε αυτήν τη λειτουργικότητα μετασχηματισμός, μπορείτε να δημιουργήσετε ένα αρχείο ApplicationHost.xdt με XDT περιεχόμενο και τοποθέτηση κάτω από τη ριζική τοποθεσία (d:\home\site) στην [Κονσόλα Kudu](https://github.com/projectkudu/kudu/wiki/Kudu-console). Ίσως χρειαστεί να κάνετε επανεκκίνηση της εφαρμογής Web για τις αλλαγές να τεθούν σε ισχύ.

Το παρακάτω δείγμα applicationHost.xdt δείχνει πώς μπορείτε να προσθέσετε μια νέα μεταβλητή προσαρμοσμένου περιβάλλοντος για μια εφαρμογή web που χρησιμοποιεί PHP 5.4.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


Ένα αρχείο καταγραφής κατάστασης μετασχηματισμός και λεπτομέρειες που είναι διαθέσιμη από το ριζικό κατάλογο FTP στην περιοχή LogFiles\Transform.

Για πρόσθετα δείγματα, ανατρέξτε στο θέμα [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Σημείωση**<br />
Στοιχεία από τη λίστα των λειτουργικών μονάδων στην περιοχή `system.webServer` δεν είναι δυνατό να καταργηθούν ή να παραγγελθούν ξανά, αλλά είναι δυνατές οι προσθήκες στη λίστα.


##<a id="extend"></a>Επέκταση της εφαρμογής web σας

###<a id="overview"></a>Επισκόπηση των επεκτάσεων ιδιωτικό web app

Εφαρμογή υπηρεσίας υποστηρίζει επεκτάσεις εφαρμογής web ως σημείο επεκτασιμότητα του για διαχειριστικές εργασίες. Στην πραγματικότητα, ορισμένες δυνατότητες πλατφόρμα εφαρμογής υπηρεσίας υλοποιούνται ως προ-εγκατεστημένες επεκτάσεις. Ενώ οι επεκτάσεις προ-εγκατεστημένες πλατφόρμα δεν μπορεί να τροποποιηθεί, μπορείτε να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους ιδιωτικό επεκτάσεις για τη δική σας web app. Αυτή η λειτουργία επίσης βασίζεται σε δηλώσεις XDT. Τα βασικά βήματα για να δημιουργήσετε μια επέκταση εφαρμογών web ιδιωτικό είναι οι εξής:

1. Επέκταση εφαρμογών **περιεχομένου**Web: δημιουργία οποιαδήποτε εφαρμογή web που υποστηρίζονται από το εφαρμογής υπηρεσίας
2. Στο Web app επέκταση **δήλωσης**: δημιουργία ενός αρχείου ApplicationHost.xdt
3. Επέκταση εφαρμογής **ανάπτυξης**στο Web: τοποθετήσετε περιεχόμενο στο φάκελο SiteExtensions στην περιοχή`root`

Εσωτερικές συνδέσεις για την εφαρμογή web πρέπει να παραπέμπει σε μια διαδρομή σε σχέση με τη διαδρομή της εφαρμογής που καθορίζεται στο αρχείο ApplicationHost.xdt. Οποιαδήποτε αλλαγή στο αρχείο ApplicationHost.xdt απαιτεί μια Ανακύκλωσης εφαρμογής web.

**Σημείωση**: πρόσθετες πληροφορίες για αυτά τα βασικά στοιχεία είναι διαθέσιμη από την [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Ένα αναλυτικό παράδειγμα περιλαμβάνεται που δείχνει τα βήματα για τη δημιουργία και ενεργοποιώντας μια επέκταση εφαρμογών web ιδιωτικό. Ο κωδικός προέλευσης για τη Διαχείριση PHP παράδειγμα που ακολουθεί μπορούν να ληφθούν από [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

###<a id="SiteSample"></a>Παράδειγμα επέκταση εφαρμογών Web: Διαχείριση PHP

Διαχείριση PHP είναι μια επέκταση εφαρμογών web που επιτρέπει στους διαχειριστές εφαρμογής για να προβάλετε και να ρυθμίσουν τις παραμέτρους PHP χρησιμοποιώντας ένα περιβάλλον web αντί να χρειάζεται να τροποποιήσετε αρχεία .ini PHP απευθείας εύκολα να web. Κοινά αρχεία ρύθμισης παραμέτρων για PHP, συμπεριλάβετε το αρχείο php.ini που βρίσκεται στην περιοχή αρχεία προγράμματος και η. user.ini αρχείο που βρίσκεται στον ριζικό φάκελο της εφαρμογής web. Επειδή το αρχείο php.ini δεν είναι απευθείας με δυνατότητα επεξεργασίας στην πλατφόρμα εφαρμογής υπηρεσίας, χρησιμοποιεί την επέκταση PHP Manager η. user.ini αρχείο για να εφαρμοστούν οι αλλαγές.

####<a id="PHPwebapp"></a>Η εφαρμογή web PHP Manager

Ακολουθεί η αρχική σελίδα της ανάπτυξης PHP Manager:

![TransformSitePHPUI][TransformSitePHPUI]

Όπως μπορείτε να δείτε μια επέκταση εφαρμογών web είναι ακριβώς όπως μια εφαρμογή web κανονική, αλλά με ένα πρόσθετο αρχείο ApplicationHost.xdt τοποθετείται στον ριζικό φάκελο του web app (περισσότερες λεπτομέρειες σχετικά με το αρχείο ApplicationHost.xdt είναι διαθέσιμα στην επόμενη ενότητα αυτού του άρθρου).

Την επέκταση PHP Manager που δημιουργήθηκε με χρήση του προτύπου Visual Studio ASP.NET MVC 4 εφαρμογής Web. Η ακόλουθη προβολή από την Εξερεύνηση λύσεων εμφανίζει τη δομή της επέκτασης PHP Manager.

![TransformSiteSolEx][TransformSiteSolEx]

Η λογική μοναδικός ειδικός που απαιτείται για το αρχείο εισόδου/εξόδου είναι για να υποδείξετε πού βρίσκεται ο κατάλογος wwwroot της εφαρμογής web. Όπως δείχνει το παρακάτω παράδειγμα κώδικα, περιβάλλον μεταβλητής "ΚΕΝΤΡΙΚΉ" υποδηλώνει διαδρομή ρίζας του web app και τη διαδρομή wwwroot μπορεί να κατασκευαστεί προσαρτώντας "site\wwwroot":

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


Αφού έχετε τη διαδρομή του καταλόγου, μπορείτε να χρησιμοποιήσετε λειτουργίες εισόδου/εξόδου κανονικό αρχείο για ανάγνωση και εγγραφή σε αρχεία.

Ένα σημείο συμβουλή με επεκτάσεις εφαρμογής web αφορά τη διαχείριση των εσωτερικές συνδέσεις.  Εάν έχετε οποιεσδήποτε συνδέσεις στα αρχεία σας HTML που παρέχουν απόλυτες διαδρομές σε εσωτερικές συνδέσεις στην εφαρμογή web της, πρέπει να βεβαιωθείτε ότι είναι τοποθετηθούν μπροστά από αυτές τις συνδέσεις με το όνομά σας επέκταση ως τη ριζική. Αυτό είναι απαραίτητο, επειδή ο ριζικός κατάλογος για την επέκταση είναι τώρα "/`[your-extension-name]`/" αντί να γίνεται απλώς "/", επομένως, κάθε εσωτερική συνδέσεις πρέπει να ενημερωθούν αντίστοιχα. Για παράδειγμα, ας υποθέσουμε ότι σας κώδικα περιλαμβάνει μια σύνδεση με το εξής:

`"<a href="/Home/Settings">PHP Settings</a>"`

Όταν η σύνδεση είναι μέρος μια επέκταση εφαρμογών web, πρέπει να είναι η σύνδεση με την εξής μορφή:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Μπορείτε να επιλύσετε αυτήν την απαίτηση, είτε χρησιμοποιώντας μόνο σχετικές διαδρομές μέσα από την εφαρμογή web ή στην περίπτωση εφαρμογές ASP.NET, χρησιμοποιώντας το `@Html.ActionLink` μέθοδο η οποία δημιουργεί τις κατάλληλες συνδέσεις για εσάς.

####<a id="XDT"></a>Το αρχείο applicationHost.xdt

Τον κωδικό για την επέκταση εφαρμογών web μεταβαίνει στην περιοχή %HOME%\SiteExtensions\[-επέκταση-το όνομά σας]. Θα ονομάζουμε αυτή την επέκταση ρίζα.  

Για να καταχωρήσετε την επέκταση εφαρμογών web με το αρχείο applicationHost.config, πρέπει να τοποθετήσετε ένα αρχείο που ονομάζεται ApplicationHost.xdt στη ρίζα επέκτασης. Το περιεχόμενο του αρχείου ApplicationHost.xdt πρέπει να είναι ως εξής:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

Το όνομα που επιλέγετε ως το όνομα του επέκταση θα πρέπει να έχουν το ίδιο όνομα με τον ριζικό φάκελο επέκταση.

Αυτό έχει ως αποτέλεσμα της προσθήκης μιας νέας διαδρομή της εφαρμογής για να το `system.applicationHost` λίστα τοποθεσιών στην περιοχή της τοποθεσίας SCM. Η τοποθεσία SCM είναι μια τοποθεσία διαχείρισης τελικού σημείου με καθορισμένη πρόσβαση διαπιστευτήρια. Έχει τη διεύθυνση URL `https://[your-site-name].scm.azurewebsites.net`.  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a id="deploy"></a>Ανάπτυξη επέκταση εφαρμογών Web

Για να εγκαταστήσετε την επέκταση εφαρμογών web, μπορείτε να χρησιμοποιήσετε FTP για να αντιγράψετε όλα τα αρχεία της εφαρμογής σας web για να το `\SiteExtensions\[your-extension-name]` φάκελο της εφαρμογής web στην οποία θέλετε να εγκαταστήσετε την επέκταση.  Φροντίστε να αντιγράψει το αρχείο ApplicationHost.xdt σε αυτήν τη θέση καθώς και. Επανεκκινήστε την εφαρμογή σας web για να ενεργοποιήσετε την επέκταση.

Θα πρέπει να μπορείτε να δείτε την επέκταση εφαρμογών web στο:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Σημειώστε ότι η διεύθυνση URL μοιάζει με τη διεύθυνση URL για την εφαρμογή web σας, με τη διαφορά ότι χρησιμοποιεί HTTPS και περιέχει ".scm".

Είναι δυνατή η απενεργοποίηση όλων των επεκτάσεων (δεν προ-εγκατεστημένες) ιδιωτικό για την εφαρμογή web της κατά τη διάρκεια της ανάπτυξης και έρευνες, προσθέτοντας μια εφαρμογή ρυθμίσεων με το κλειδί `WEBSITE_PRIVATE_EXTENSIONS` και η τιμή `0`.

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751), όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή web μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

## <a name="whats-changed"></a>Τι έχει αλλάξει
* Για οδηγίες για την αλλαγή από τοποθεσίες Web App υπηρεσία ανατρέξτε στο θέμα: [Azure εφαρμογής υπηρεσίας και τον αντίκτυπο σχετικά με τις υπάρχουσες υπηρεσίες Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
