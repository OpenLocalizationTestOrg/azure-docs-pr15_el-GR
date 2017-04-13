<properties
    pageTitle="Python στο web app με Django | Microsoft Azure"
    description="Αυτό το πρόγραμμα εκμάθησης σάς μαθαίνει πώς μπορείτε να φιλοξενήσετε μια τοποθεσία Web Django βασίζονται στο Azure χρησιμοποιώντας μια εικονική μηχανή Windows Server 2012 R2 κέντρο δεδομένων με χρήση του μοντέλου κλασική ανάπτυξης."
    services="virtual-machines-windows"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>


<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>


# <a name="django-hello-world-web-application-on-a-windows-server-vm"></a>Εφαρμογή web Django Γεια σε μια Εικονική Windows Server

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac/Linux](virtual-machines-linux-python-django-web-app.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Αυτό το πρόγραμμα εκμάθησης περιγράφει πώς μπορείτε να φιλοξενήσετε μια τοποθεσία Web με βάση Django στο Microsoft Azure χρησιμοποιώντας μια εικονική μηχανή Windows Server. Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε χωρίς προηγούμενη εμπειρία με χρήση Azure. Αφού ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, θα έχετε μια εφαρμογή που βασίζεται σε Django προς τα επάνω και την εκτέλεση στο cloud.

Θα μάθετε πώς μπορείτε να:

* Ρυθμίστε μια εικονική μηχανή Azure κεντρικό υπολογιστή Django. Ενώ αυτό το πρόγραμμα εκμάθησης εξηγεί πώς μπορείτε να πραγματοποιήσετε αυτές τις στην περιοχή Windows Server, μπορεί επίσης να κάνετε το ίδιο με μια Εικονική φιλοξενείται στο Azure Linux.
* Δημιουργία νέας εφαρμογής Django από το Windows.

Ακολουθώντας αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να δημιουργήσετε μια απλή εφαρμογή Γεια στο web. Η εφαρμογή θα να φιλοξενούνται σε μια εικονική μηχανή Azure.

Στιγμιότυπο οθόνης της εφαρμογής ολοκληρωμένη εμφανίζεται Επόμενο.

![Εμφάνιση της σελίδας κόσμο hello στην Azure παράθυρο του προγράμματος περιήγησης][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Δημιουργία και ρύθμιση παραμέτρων ενός Azure εικονική μηχανή κεντρικό υπολογιστή Django

1. Ακολουθήστε τις οδηγίες δεδομένη [εδώ](virtual-machines-windows-classic-tutorial.md) για να δημιουργήσετε μια εικονική μηχανή Azure της κατανομής Windows Server 2012 R2 κέντρου δεδομένων.

1. Πείτε Azure για να κατευθύνουν θύρα 80 κυκλοφορία από το web στη θύρα 80 στον υπολογιστή εικονικές:
 - Μεταβείτε στον υπολογιστή σας που έχουν δημιουργηθεί πρόσφατα εικονικές στην πύλη του Azure κλασική και κάντε κλικ στην καρτέλα **τελικά ΣΗΜΕΊΑ** .
 - Κάντε κλικ στο κουμπί **ΠΡΟΣΘΉΚΗ** στο κάτω μέρος της οθόνης.
    ![Προσθήκη τελικού σημείου](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-addendpoint.png)

 - Άνοιγμα του το πρωτόκολλο **TCP** του **ΔΗΜΌΣΙΑ ΘΎΡΑ 80** ως **ΙΔΙΩΤΙΚΉ ΘΎΡΑ 80**.
![][port80]
1. Από την καρτέλα **πίνακα ΕΡΓΑΛΕΊΩΝ** , κάντε κλικ στην επιλογή **ΣΎΝΔΕΣΗ** με χρήση **Απομακρυσμένης επιφάνειας εργασίας** για να συνδεθείτε από απόσταση σε που έχουν δημιουργηθεί πρόσφατα Azure εικονική μηχανή της.  

**Σημαντική Σημείωση:** Όλες οι παρακάτω οδηγίες θεωρείται ότι έχετε συνδεθεί στο η εικονική μηχανή σωστά και εκδίδει εντολές εκεί και όχι τον τοπικό υπολογιστή.

## <a id="setup"> </a>Κατά την εγκατάσταση Python, Django, WFastCGI

**Σημείωση:** Για να κάνετε λήψη χρησιμοποιώντας τον Internet Explorer, ίσως χρειαστεί να ρυθμίσετε τις παραμέτρους του IE ESC (Έναρξη/διαχείρισης εργαλεία/διακομιστή Manager/τοπικού διακομιστή, στη συνέχεια, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων βελτιωμένης ασφάλειας IE**, απενεργοποιημένη).

1. Εγκαταστήστε την πιο πρόσφατη 2.7 Python ή 3.4 από [python.org][].
1. Εγκαταστήστε το wfastcgi και django πακέτα χρησιμοποιώντας pip.

    Για Python 2.7, χρησιμοποιήστε την ακόλουθη εντολή.

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    Για Python 3.4, χρησιμοποιήστε την ακόλουθη εντολή.

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="installing-iis-with-fastcgi"></a>Κατά την εγκατάσταση των υπηρεσιών IIS με FastCGI

1. Εγκατάσταση των υπηρεσιών IIS με την υποστήριξη FastCGI.  Αυτό μπορεί να διαρκέσει αρκετά λεπτά για την εκτέλεση.

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="creating-a-new-django-application"></a>Δημιουργία νέας εφαρμογής Django

1.  Από *C:\inetpub\wwwroot*, πληκτρολογήστε την παρακάτω εντολή για να δημιουργήσετε ένα νέο έργο Django:

    Για Python 2.7, χρησιμοποιήστε την ακόλουθη εντολή.

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    Για Python 3.4, χρησιμοποιήστε την ακόλουθη εντολή.

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![Το αποτέλεσμα της εντολής New-AzureService](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-cmd-new-azure-service.png)

1.  Η εντολή **django διαχείρισης** δημιουργεί μια βασική δομή για τοποθεσίες Web που βασίζονται σε Django:

  -   **helloworld\manage.PY** σάς βοηθά να για να ξεκινήσετε φιλοξενίας και να διακόψετε τη φιλοξενία της τοποθεσίας Web που βασίζονται σε Django
  -   **helloworld\helloworld\settings.PY** περιέχει Django ρυθμίσεις για την εφαρμογή σας.
  -   **helloworld\helloworld\urls.PY** περιέχει τον κωδικό αντιστοίχισης μεταξύ κάθε διεύθυνση url και την προβολή.

1.  Δημιουργήστε ένα νέο αρχείο με το όνομα **views.py** στον κατάλογο *C:\inetpub\wwwroot\helloworld\helloworld* . Αυτό θα περιέχει την προβολή που αποδίδει τη σελίδα "Χαίρετε". Ξεκινήστε το πρόγραμμα επεξεργασίας και πληκτρολογήστε τα εξής:

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Αντικαταστήστε τα περιεχόμενα του αρχείου urls.py με τα εξής.

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## <a name="configuring-iis"></a>Ρύθμιση παραμέτρων των υπηρεσιών IIS

1. Ξεκλείδωμα στην ενότητα προγράμματα χειρισμού σε την καθολική applicationhost.config.  Αυτό θα ενεργοποιήσει τη χρήση του προγράμματος χειρισμού python στο web.config σας.

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. Ενεργοποίηση WFastCGI.  Αυτό θα προσθέσετε μια εφαρμογή για το καθολικό applicationhost.config που αναφέρεται σε εκτελέσιμο μεταφραστή σας Python και τη δέσμη ενεργειών wfastcgi.py.

    Python 2.7:

        c:\python27\scripts\wfastcgi-enable

    Python 3.4:

        c:\python34\scripts\wfastcgi-enable

1. Δημιουργήστε ένα αρχείο web.config στο *C:\inetpub\wwwroot\helloworld*.  Η τιμή του το `scriptProcessor` χαρακτηριστικού πρέπει να συμφωνεί με την έξοδο από το προηγούμενο βήμα.  Δείτε τη σελίδα για [wfastcgi][] σε pypi για τις περισσότερες ρυθμίσεις wfastcgi ενεργοποίηση.

    Python 2.7:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    Python 3.4:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. Ενημερώστε τη θέση από την των υπηρεσιών IIS προεπιλεγμένη τοποθεσία Web ώστε να οδηγούν στο φάκελο django έργου.

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. Τέλος, φόρτωση της ιστοσελίδας στο πρόγραμμα περιήγησης.

![Εμφάνιση της σελίδας κόσμο hello στην Azure παράθυρο του προγράμματος περιήγησης][1]


## <a name="shutting-down-your-azure-virtual-machine"></a>Τερματισμός του Azure εικονική μηχανή

Όταν ολοκληρώσετε την εργασία με αυτό το πρόγραμμα εκμάθησης, τερματίστε ή/και κατάργηση σας που έχουν δημιουργηθεί πρόσφατα Azure εικονική μηχανή για να αποδεσμεύσετε πόρους για άλλα προγράμματα εκμάθησης και να αποφύγετε τις χρεώσεις Azure χρήση.

[1]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
