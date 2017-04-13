<properties
    pageTitle="Ρόλοι web και εργαζόμενου PHP | Microsoft Azure"
    description="Ένας οδηγός για τη δημιουργία ρόλων PHP web και εργαζόμενου σε μια υπηρεσία Azure cloud και τη ρύθμιση των παραμέτρων του χρόνου εκτέλεσης PHP."
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>Πώς μπορείτε να δημιουργήσετε ρόλους PHP web και εργασίας

## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός θα σας δείξουν πώς μπορείτε να δημιουργήσετε ρόλους PHP web ή εργασίας σε ένα περιβάλλον ανάπτυξης των Windows, επιλέξτε μια συγκεκριμένη έκδοση της PHP από τις διαθέσιμες "ενσωματωμένα" εκδόσεις, αλλάξτε τη ρύθμιση παραμέτρων PHP, Ενεργοποίηση επεκτάσεων και τέλος, ανάπτυξη Azure. Περιγράφει τον τρόπο ρύθμισης παραμέτρων ρόλου web ή εργασίας για να χρησιμοποιήσετε ένα περιβάλλον εκτέλεσης PHP (με προσαρμοσμένη ρύθμιση παραμέτρων και επεκτάσεις) που παρέχετε επίσης.

## <a name="what-are-php-web-and-worker-roles"></a>Τι είναι οι ρόλοι PHP web και εργασίας;

Azure παρέχει τρία τον υπολογισμό μοντέλα για την εκτέλεση εφαρμογών: Azure εφαρμογής υπηρεσίας, εικονικές μηχανές Windows Azure και τις υπηρεσίες Cloud Azure. Όλα τα τρία μοντέλα υποστηρίζει PHP. Υπηρεσίες cloud, που περιλαμβάνει το web και εργαζόμενου ρόλους, παρέχει *πλατφόρμα ως υπηρεσία (PaaS)*. Μέσα σε μια υπηρεσία cloud, ένα ρόλο web παρέχει ένα αποκλειστικό διακομιστή web Internet Information Services (IIS) για τις εφαρμογές web προσκηνίου του κεντρικού υπολογιστή. Ένα ρόλο εργαζόμενου να εκτελέσετε εργασίες ασύγχρονης, μεγάλη διάρκεια εκτέλεσης ή διαρκή ανεξάρτητα από την παρέμβαση του χρήστη ή εισαγωγή δεδομένων.

Για περισσότερες πληροφορίες σχετικά με αυτές τις επιλογές, ανατρέξτε στο θέμα [τον υπολογισμό φιλοξενίας επιλογές που παρέχονται από Azure](./cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>Κάντε λήψη του Azure SDK για PHP

Το [Azure SDK για PHP] αποτελείται από πολλά στοιχεία. Σε αυτό το άρθρο θα χρησιμοποιήσει δύο από αυτά: Azure PowerShell και το Azure emulators. Αυτά τα δύο στοιχεία μπορεί να εγκατασταθεί μέσω του προγράμματος εγκατάστασης πλατφόρμας Web της Microsoft. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](powershell-install-configure.md).

## <a name="create-a-cloud-services-project"></a>Δημιουργήστε ένα έργο υπηρεσιών Cloud

Το πρώτο βήμα για τη δημιουργία ρόλου PHP web ή εργασίας είναι να δημιουργήσετε ένα έργο υπηρεσιών Azure. ένα έργο υπηρεσιών Azure λειτουργεί ως ένα λογικό κοντέινερ για το web και εργαζόμενου τους ρόλους και περιέχει αρχεία [ορισμού υπηρεσίας (.csdef)] και [Ρύθμιση παραμέτρων της υπηρεσίας (.cscfg)] του έργου.

Για να δημιουργήσετε ένα νέο έργο υπηρεσιών Azure, εκτελέστε Azure PowerShell ως διαχειριστής και εκτελέστε την ακόλουθη εντολή:

    PS C:\>New-AzureServiceProject myProject

Αυτή η εντολή θα δημιουργήσετε έναν νέο κατάλογο (`myProject`) στο οποίο μπορείτε να προσθέσετε ρόλους web και εργασίας.

## <a name="add-php-web-or-worker-roles"></a>Προσθήκη PHP web ή εργαζόμενου ρόλους

Για να προσθέσετε ένα ρόλο web PHP ένα έργο, εκτελέστε την παρακάτω εντολή μέσα από το ριζικό κατάλογο του έργου:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Για ένα ρόλο εργαζόμενου, χρησιμοποιήστε αυτή την εντολή:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] Το `roleName` η παράμετρος είναι προαιρετική. Εάν παραλειφθεί, θα δημιουργηθεί αυτόματα το όνομα του ρόλου. Θα είναι το πρώτο ρόλο web δημιουργήθηκε `WebRole1`, θα είναι η δεύτερη `WebRole2`, και ούτω καθεξής. Θα είναι το πρώτο ρόλο εργαζόμενου δημιουργήθηκε `WorkerRole1`, θα είναι η δεύτερη `WorkerRole2`, και ούτω καθεξής.

## <a name="specify-the-built-in-php-version"></a>Καθορίστε την ενσωματωμένη έκδοση PHP

Όταν προσθέτετε ένα ρόλο PHP web ή εργασίας σε ένα έργο, τροποποιούνται αρχεία ρύθμισης παραμέτρων του έργου, ώστε να PHP θα εγκατασταθεί σε κάθε web ή εργαζόμενου παρουσία της εφαρμογής σας όταν έχει αναπτυχθεί το. Για να δείτε την έκδοση του PHP που θα εγκατασταθεί από προεπιλογή, εκτελέστε την ακόλουθη εντολή:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

Το αποτέλεσμα από την παραπάνω εντολή θα είναι παρόμοιο με αυτό που φαίνεται παρακάτω. Σε αυτό το παράδειγμα, το `IsDefault` σημαία έχει οριστεί στην `true` για PHP 5.3.17, που υποδεικνύει ότι θα είναι η προεπιλεγμένη έκδοση PHP εγκατεστημένο.

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

Μπορείτε να ορίσετε την έκδοση χρόνου εκτέλεσης PHP σε οποιαδήποτε από τις εκδόσεις PHP που παρατίθενται. Για παράδειγμα, για να ορίσετε την έκδοση PHP (για ένα ρόλο με το όνομα `roleName`) για να 5.4.0, χρησιμοποιήστε την ακόλουθη εντολή:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] Διαθέσιμες εκδόσεις PHP μπορεί να αλλάξει στο μέλλον.

## <a name="customize-the-built-in-php-runtime"></a>Προσαρμόστε το ενσωματωμένο περιβάλλον εκτέλεσης PHP

Έχετε πλήρη έλεγχο όσον αφορά τη ρύθμιση παραμέτρων του χρόνου εκτέλεσης PHP που εγκαθίσταται όταν ακολουθείτε τα βήματα που περιγράφονται παραπάνω, συμπεριλαμβανομένων των τροποποιήσεων της `php.ini` ρυθμίσεις και την ενεργοποίηση των επεκτάσεων.

Για να προσαρμόσετε το ενσωματωμένο περιβάλλον εκτέλεσης PHP, ακολουθήστε τα παρακάτω βήματα:

1. Προσθέσετε ένα νέο φάκελο που ονομάζεται `php`, για το `bin` καταλόγου του ρόλου σας web. Για ένα ρόλο εργαζόμενου, προσθέσετε ριζικό κατάλογο του ρόλου.
2. Στο το `php` φάκελο, δημιουργήστε έναν άλλο φάκελο που ονομάζεται `ext`. Τοποθέτηση οποιαδήποτε `.dll` αρχεία επέκτασης (π.χ., `php_mongo.dll`) που θέλετε να ενεργοποιήσετε την σε αυτόν το φάκελο.
3. Προσθήκη μιας `php.ini` αρχείων για να το `php` φακέλου. Ενεργοποιήστε τυχόν προσαρμοσμένες επεκτάσεις και ορίστε οποιαδήποτε PHP οδηγίες σε αυτό το αρχείο. Για παράδειγμα, εάν θέλατε να ενεργοποιήσετε `display_errors` σε και να ενεργοποιήσετε το `php_mongo.dll` επέκταση, τα περιεχόμενα του σας `php.ini` αρχείου θα είναι ως εξής:

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] Τυχόν ρυθμίσεις που δεν μπορείτε να ορίσετε ρητά σε το `php.ini` αρχείου που παρέχετε θα αυτόματα η ρύθμιση είναι οι προεπιλεγμένες τιμές. Ωστόσο, λάβετε υπόψη ότι μπορείτε να προσθέσετε μια ολοκληρωμένη `php.ini` αρχείου.

## <a name="use-your-own-php-runtime"></a>Χρησιμοποιήστε το δικό σας PHP χρόνου εκτέλεσης
Σε ορισμένες περιπτώσεις, αντί για επιλέγοντας μια ενσωματωμένη χρόνου εκτέλεσης PHP και να ρυθμίσετε τις παραμέτρους του όπως περιγράφεται παραπάνω, ενδέχεται να θέλετε να παρέχετε τις δικές σας runtime PHP. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε το ίδιο περιβάλλον εκτέλεσης PHP στο web ή εργαζόμενου ρόλου που χρησιμοποιείτε στο περιβάλλον ανάπτυξής σας. Αυτό διευκολύνει για να βεβαιωθείτε ότι η εφαρμογή δεν θα αλλάξει τη συμπεριφορά στο περιβάλλον παραγωγής σας.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Ρύθμιση παραμέτρων του ρόλου web για να χρησιμοποιήσετε τα δικά σας κατά το χρόνο εκτέλεσης PHP

Για να ρυθμίσετε ένα ρόλο web για να χρησιμοποιήσετε ένα περιβάλλον εκτέλεσης PHP που παρέχετε, ακολουθήστε τα παρακάτω βήματα:

1. Δημιουργία ενός έργου, η υπηρεσία του Azure και προσθέστε ένα ρόλο web PHP, όπως περιγράφεται προηγουμένως σε αυτό το θέμα.
2. Δημιουργία μιας `php` φακέλου σε το `bin` φακέλου που βρίσκεται στον ριζικό κατάλογο του ρόλου σας στο web και, στη συνέχεια, προσθέστε το περιβάλλον εκτέλεσης PHP (όλα δυαδικά δεδομένα, αρχεία ρύθμισης παραμέτρων, υποφακέλους, κ.λπ.) για να το `php` φακέλου.
3. (ΠΡΟΑΙΡΕΤΙΚΌ) Εάν τα [Προγράμματα οδήγησης της Microsoft για PHP για τον SQL Server]χρησιμοποιεί το περιβάλλον εκτέλεσης PHP[sqlsrv drivers], θα πρέπει να ρυθμίσετε τις παραμέτρους ο ρόλος σας web για να εγκαταστήσετε το [SQL Server Native 2012 προγράμματος-πελάτη] [ sql native client] όταν παρέχεται. Για να κάνετε αυτό, προσθέστε το [πρόγραμμα εγκατάστασης του sqlncli.msi x64] για να το `bin` φάκελος στον ριζικό κατάλογο του ρόλου σας στο web. Δέσμη ενεργειών εκκίνησης περιγράφεται στο επόμενο βήμα σιωπηρά θα εκτελείται το πρόγραμμα εγκατάστασης κατά την παροχή της υπηρεσίας του ρόλου. Εάν το περιβάλλον εκτέλεσης PHP δεν χρησιμοποιεί τα προγράμματα οδήγησης της Microsoft για PHP για τον SQL Server, μπορείτε να καταργήσετε την ακόλουθη γραμμή από τη δέσμη ενεργειών εμφανίζεται στο επόμενο βήμα:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Ορισμός μιας εργασίας εκκίνησης που ρυθμίζει τις παραμέτρους των [Υπηρεσιών Internet Information Services (IIS)] [ iis.net] για να χρησιμοποιήσετε το περιβάλλον εκτέλεσης PHP χειρισμού των αιτήσεων για `.php` σελίδες. Για να το κάνετε αυτό, ανοίξτε το `setup_web.cmd` αρχείου (στο το `bin` αρχείου με το ρόλο του web ριζικό κατάλογο) σε έναν επεξεργαστή κειμένου και αντικαταστήστε τα περιεχόμενά με την ακόλουθη δέσμη ενεργειών:

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. Προσθέστε τα αρχεία σας εφαρμογή ριζικό κατάλογο του ρόλου σας στο web. Αυτό θα είναι το διακομιστή web ριζικό κατάλογο.

6. Δημοσίευση εφαρμογής σας, όπως περιγράφεται στην παρακάτω ενότητα [δημοσίευση την εφαρμογή σας](#how-to-publish-your-application) .

> [AZURE.NOTE] Το `download.ps1` δέσμης ενεργειών (στο το `bin` φακέλου από το ρόλο του web ριζικό κατάλογο) μπορούν να διαγραφούν αφού ακολουθήσετε τα βήματα που περιγράφονται παραπάνω για τη χρήση τη δική σας runtime PHP.

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Ρύθμιση παραμέτρων του ρόλου εργασίας για να χρησιμοποιήσετε τα δικά σας κατά το χρόνο εκτέλεσης PHP

Για να ρυθμίσετε ένα ρόλο εργασίας για να χρησιμοποιήσετε ένα περιβάλλον εκτέλεσης PHP που παρέχετε, ακολουθήστε τα παρακάτω βήματα:

1. Δημιουργία ενός έργου, η υπηρεσία του Azure και προσθέστε ένα ρόλο εργαζόμενου PHP, όπως περιγράφεται προηγουμένως σε αυτό το θέμα.
2. Δημιουργία μιας `php` φάκελος στον ριζικό κατάλογο του ρόλου του εργαζόμενου, και, στη συνέχεια, προσθέστε το περιβάλλον εκτέλεσης PHP (όλα δυαδικά δεδομένα, αρχεία ρύθμισης παραμέτρων, υποφακέλους, κ.λπ.) για να το `php` φακέλου.
3. (ΠΡΟΑΙΡΕΤΙΚΌ) Εάν το περιβάλλον εκτέλεσης PHP χρησιμοποιεί [Προγράμματα οδήγησης της Microsoft για PHP για τον SQL Server][sqlsrv drivers], θα πρέπει να ρυθμίσετε τις παραμέτρους ο ρόλος σας εργασίας για να εγκαταστήσετε το [SQL Server Native 2012 προγράμματος-πελάτη] [ sql native client] όταν παρέχεται. Για να το κάνετε αυτό, προσθέστε το [πρόγραμμα εγκατάστασης του sqlncli.msi x64] το ρόλο του εργαζόμενου ριζικό κατάλογο. Δέσμη ενεργειών εκκίνησης περιγράφεται στο επόμενο βήμα σιωπηρά θα εκτελείται το πρόγραμμα εγκατάστασης κατά την παροχή της υπηρεσίας του ρόλου. Εάν το περιβάλλον εκτέλεσης PHP δεν χρησιμοποιεί τα προγράμματα οδήγησης της Microsoft για PHP για τον SQL Server, μπορείτε να καταργήσετε την ακόλουθη γραμμή από τη δέσμη ενεργειών εμφανίζεται στο επόμενο βήμα:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Ορίσετε μια εργασία εκκίνησης που προσθέτει το `php.exe` εκτελέσιμο το ρόλο του εργασίας μεταβλητή περιβάλλοντος PATH κατά την παροχή της υπηρεσίας του ρόλου. Για να το κάνετε αυτό, ανοίξτε το `setup_worker.cmd` το αρχείο (το ρόλο του εργαζόμενου ριζικό κατάλογο) σε έναν επεξεργαστή κειμένου και αντικαταστήστε τα περιεχόμενά με την ακόλουθη δέσμη ενεργειών:

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. Προσθέστε τα αρχεία σας εφαρμογή ρόλο εργαζόμενου ριζικό κατάλογο.

6. Δημοσίευση εφαρμογής σας, όπως περιγράφεται στην παρακάτω ενότητα [δημοσίευση την εφαρμογή σας](#how-to-publish-your-application) .

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Εκτέλεση της εφαρμογής σας στο το emulators υπολογισμού και χώρου αποθήκευσης

Το Azure emulators παρέχουν ένα τοπικό περιβάλλον στο οποίο μπορείτε να ελέγξετε την εφαρμογή του Azure πριν να την αναπτύξετε στο cloud. Υπάρχουν ορισμένες διαφορές μεταξύ του emulators και του Azure περιβάλλοντος. Για να κατανοήσετε καλύτερα αυτό, ανατρέξτε στο θέμα [χρήση του προσομοίωσης Azure χώρου αποθήκευσης για ανάπτυξη και δοκιμές](./storage/storage-use-emulator.md).

Σημειώστε ότι πρέπει να έχετε εγκατεστημένη τοπικά για να χρησιμοποιήσετε το προσομοίωσης υπολογισμού PHP. Το προσομοίωσης υπολογισμού θα χρησιμοποιήσει την τοπική PHP εγκατάσταση για να εκτελέσετε την εφαρμογή σας.

Για να εκτελέσετε το έργο σας σε το emulators, εκτελέστε την ακόλουθη εντολή από το ριζικό κατάλογο του έργου σας:

    PS C:\MyProject> Start-AzureEmulator

Θα δείτε εξόδου παρόμοιο με αυτό:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Μπορείτε να δείτε την εφαρμογή που εκτελείται στο το προσομοίωσης ανοίγοντας ένα πρόγραμμα περιήγησης web και περιήγηση σε την τοπική διεύθυνση που εμφανίζεται στο αποτέλεσμα (`http://127.0.0.1:81` στο παραπάνω παράδειγμα αποτέλεσμα).

Για να διακόψετε την emulators, εκτέλεση αυτής της εντολής:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Δημοσίευση εφαρμογής σας

Για να δημοσιεύσετε την εφαρμογή σας, πρέπει να πρώτα να εισαγάγετε τις ρυθμίσεις δημοσίευσης, χρησιμοποιώντας το cmdlet [AzurePublishSettingsFile εισαγωγής](https://msdn.microsoft.com/library/azure/dn790370.aspx) . Στη συνέχεια, μπορείτε να δημοσιεύσετε την εφαρμογή σας, χρησιμοποιώντας το cmdlet [AzureServiceProject δημοσίευση](https://msdn.microsoft.com/library/azure/dn495166.aspx) . Για πληροφορίες σχετικά με την είσοδο, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](powershell-install-configure.md).

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές PHP](/develop/php/).

[Azure SDK για PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[Ορισμός υπηρεσίας (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[ρύθμιση παραμέτρων της υπηρεσίας (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[πρόγραμμα εγκατάστασης του sqlncli.msi x64]: http://go.microsoft.com/fwlink/?LinkID=239648
