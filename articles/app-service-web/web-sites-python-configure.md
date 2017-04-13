<properties 
    pageTitle="Ρύθμιση παραμέτρων Python με Azure εφαρμογής υπηρεσίας Web Apps" 
    description="Αυτό το πρόγραμμα εκμάθησης περιγράφει τις επιλογές για τη σύνταξη και τη ρύθμιση των παραμέτρων σε διακομιστή Web βασική εφαρμογή συμβατή με Python πύλης περιβάλλοντος εργασίας (WSGI) σε Azure εφαρμογής υπηρεσίας Web Apps." 
    services="app-service" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/26/2016" 
    ms.author="huvalo"/>




# <a name="configuring-python-with-azure-app-service-web-apps"></a>Ρύθμιση παραμέτρων Python με Azure εφαρμογής υπηρεσίας Web Apps

Αυτό το πρόγραμμα εκμάθησης περιγράφει τις επιλογές για τη σύνταξη και τη ρύθμιση των παραμέτρων μια βασική εφαρμογή Web διακομιστή πύλης περιβάλλοντος εργασίας (WSGI) συμβατή με Python στην [Azure εφαρμογής υπηρεσίας Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Περιγράφει πρόσθετες δυνατότητες ανάπτυξης Git, όπως το εικονικό περιβάλλον και εγκατάσταση του πακέτου με χρήση requirements.txt.


## <a name="bottle-django-or-flask"></a>Μπουκάλι, Django ή φιάλη;

Το Azure Marketplace περιέχει πρότυπα για τα πλαίσια μπουκάλι, Django και φιάλη. Εάν σχεδιάζετε πρώτη εφαρμογή web της στο Azure εφαρμογής υπηρεσίας ή δεν είστε εξοικειωμένοι με Git, συνιστάται να ακολουθήσετε ένα από αυτά τα προγράμματα εκμάθησης, η οποία περιλαμβάνει οδηγίες βήμα προς βήμα για τη δημιουργία μιας εφαρμογής εργασίας από τη συλλογή χρησιμοποιώντας Git ανάπτυξης από το Windows ή Mac:

- [Δημιουργία εφαρμογών web με μπουκάλι](web-sites-python-create-deploy-bottle-app.md)
- [Δημιουργία εφαρμογών web με Django](web-sites-python-create-deploy-django-app.md)
- [Δημιουργία εφαρμογών web με φιάλη](web-sites-python-create-deploy-flask-app.md)


## <a name="web-app-creation-on-azure-portal"></a>Δημιουργία εφαρμογής Web στην πύλη του Azure

Αυτό το πρόγραμμα εκμάθησης προϋποθέτει υπάρχουσα συνδρομή στο Azure και πρόσβαση στην πύλη του Azure.

Εάν δεν έχετε μια υπάρχουσα εφαρμογή web, μπορείτε να δημιουργήσετε μία από την [Πύλη Azure](https://portal.azure.com).  Κάντε κλικ στο ΝΈΟ κουμπί στην επάνω αριστερή γωνία και, στη συνέχεια, κάντε κλικ στην επιλογή **Web + Mobile** > **εφαρμογής Web**.

## <a name="git-publishing"></a>Δημοσίευση Git

Ρυθμίστε τις παραμέτρους της δημοσίευσης Git για την εφαρμογή web που έχουν δημιουργηθεί πρόσφατα, ακολουθώντας τις οδηγίες στην [Τοπική ανάπτυξη Git σε Azure εφαρμογής υπηρεσίας](app-service-deploy-local-git.md). Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί Git για δημιουργία, διαχείριση και δημοσίευση μας εφαρμογής web Python Azure εφαρμογής υπηρεσίας.

Μόλις ρυθμιστεί Git δημοσίευσης, ένα αποθετήριο Git θα δημιουργηθεί και θα που σχετίζεται με την εφαρμογή web. Διεύθυνση URL του αποθετηρίου θα εμφανιστούν και στο εξής μπορούν να χρησιμοποιηθούν για να προωθήσετε δεδομένων από το περιβάλλον ανάπτυξης τοπικά στο cloud. Για να δημοσιεύσετε εφαρμογές μέσω Git, βεβαιωθείτε ότι το πρόγραμμα-πελάτη και Git είναι επίσης εγκατεστημένη και χρησιμοποιήστε τις οδηγίες που παρέχονται για να προωθήσετε το περιεχόμενό σας web app σε Azure εφαρμογής υπηρεσίας.


## <a name="application-overview"></a>Επισκόπηση της εφαρμογής

Στις επόμενες ενότητες, δημιουργούνται τα ακόλουθα αρχεία. Θα πρέπει να τοποθετηθεί στη ρίζα του αποθετηρίου Git.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>Πρόγραμμα χειρισμού WSGI

WSGI είναι ένα πρότυπο Python που περιγράφεται από τον ορισμό ένα περιβάλλον εργασίας μεταξύ του διακομιστή web και Python [ΠΟΠ 3333](http://www.python.org/dev/peps/pep-3333/) . Παρέχει μια τυποποιημένη περιβάλλον εργασίας για τη σύνταξη διάφορες εφαρμογές web και πλαίσια χρησιμοποιώντας Python. Δημοφιλή πλαίσια web Python σήμερα χρησιμοποιούν WSGI. Azure εφαρμογής υπηρεσίας Web Apps παρέχει που υποστήριξης για τα πλαίσια; Επιπλέον, έμπειρους χρήστες να ακόμα και σύνταξη τις δικές τους με την προϋπόθεση ότι το προσαρμοσμένο πρόγραμμα χειρισμού ακολουθεί τις οδηγίες προδιαγραφή WSGI.

Ακολουθεί ένα παράδειγμα ενός `app.py` που ορίζει ένα προσαρμοσμένο πρόγραμμα χειρισμού:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Μπορείτε να εκτελέσετε αυτήν την εφαρμογή τοπικά με `python app.py`, στη συνέχεια, μεταβείτε `http://localhost:5555` στο πρόγραμμα περιήγησης web.


## <a name="virtual-environment"></a>Εικονικό περιβάλλον

Παρόλο που το παραπάνω παράδειγμα εφαρμογή δεν απαιτεί των εξωτερικών πακέτων, είναι πιθανό ότι η εφαρμογή σας θα απαιτεί ορισμένες.

Για βοήθεια στη Διαχείριση εξωτερικών πακέτου εξαρτήσεις, ανάπτυξη Azure Git υποστηρίζει τη δημιουργία εικονικών περιβάλλοντα.

Όταν το Azure εντοπίσει ένα requirements.txt στον ριζικό κατάλογο του αποθετηρίου, δημιουργεί αυτόματα ένα εικονικό περιβάλλον με το όνομα `env`. Αυτό συμβαίνει μόνο στην πρώτη ανάπτυξη ή κατά τη διάρκεια κάθε ανάπτυξης μετά το επιλεγμένο Python χρόνου εκτέλεσης έχει αλλάξει.

Πιθανότατα θα θέλετε να δημιουργήσετε ένα εικονικό περιβάλλον τοπικά για την ανάπτυξη, αλλά δεν περιλαμβάνονται στο αποθετήριο σας Git.


## <a name="package-management"></a>Πακέτο διαχείρισης

Πακέτα που αναγράφονται στην requirements.txt θα εγκατασταθεί αυτόματα στο εικονικό περιβάλλον χρησιμοποιώντας pip. Αυτό συμβαίνει με κάθε ανάπτυξη, αλλά pip θα παραλείψει εγκατάστασης, εάν είναι ήδη εγκατεστημένο ένα πακέτο.

Παράδειγμα `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Έκδοση Python

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Παράδειγμα `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config

Θα χρειαστεί να δημιουργήσετε ένα αρχείο web.config για να καθορίσετε πώς ο διακομιστής θα πρέπει να χειριστείτε αιτήσεις.

Λάβετε υπόψη ότι, εάν έχετε ένα αρχείο web.x.y.config στο αποθετήριο δεδομένων σας, όπου x.y ταιριάζει με το επιλεγμένο περιβάλλον εκτέλεσης Python, στη συνέχεια, Azure αντιγράφει αυτόματα το κατάλληλο αρχείο ως web.config.

Τα παρακάτω παραδείγματα web.config εξαρτώνται από μια δέσμη ενεργειών διακομιστή μεσολάβησης εικονικό περιβάλλον, η οποία περιγράφεται στην επόμενη ενότητα.  Λειτουργούν με το πρόγραμμα χειρισμού WSGI που χρησιμοποιείται στο παράδειγμα `app.py` παραπάνω.

Παράδειγμα `web.config` για Python 2.7:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Παράδειγμα `web.config` για Python 3.4:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Στατικά αρχεία θα γίνεται ο χειρισμός από το διακομιστή web απευθείας, χωρίς να μεταβείτε μέσω Python κώδικα, για βελτιωμένη απόδοση.

Στα παραπάνω παραδείγματα, τη θέση των στατική αρχείων στο δίσκο θα πρέπει να συμφωνεί με τη θέση στη διεύθυνση URL. Αυτό σημαίνει ότι μια αίτηση για `http://pythonapp.azurewebsites.net/static/site.css` θα χρησιμοποιηθεί το αρχείο στο δίσκο στο `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`είναι όπου μπορείτε να ορίσετε το πρόγραμμα χειρισμού WSGI. Στα παραπάνω παραδείγματα, που έχει `app.wsgi_app` επειδή το πρόγραμμα χειρισμού είναι μια συνάρτηση που ονομάζεται `wsgi_app` στο `app.py` στον ριζικό φάκελο.

`PYTHONPATH`μπορεί να προσαρμοστεί, αλλά εάν εγκαταστήσετε όλες τις εξαρτήσεις το εικονικό περιβάλλον, καθορίζοντας τις στη requirements.txt, δεν θα πρέπει πρέπει να την αλλάξετε.


## <a name="virtual-environment-proxy"></a>Εικονικό περιβάλλον διακομιστή μεσολάβησης

Χρησιμοποιείται η ακόλουθη δέσμη ενεργειών για να ανακτήσετε τη λαβή WSGI, να ενεργοποιήσετε το εικονικό περιβάλλον και αρχείου καταγραφής σφαλμάτων. Έχει σχεδιαστεί ώστε να είναι γενικής χρήσης και χωρίς τροποποιήσεις που χρησιμοποιήθηκαν.

Περιεχόμενα του `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n');

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')
    
        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)
    
        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()
    
        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))
    
        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []
    
        site.main()
    
        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Προσαρμογή Git ανάπτυξης

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]


## <a name="troubleshooting---package-installation"></a>Αντιμετώπιση προβλημάτων - η εγκατάσταση του πακέτου

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Αντιμετώπιση προβλημάτων - εικονικό περιβάλλον

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές Python](/develop/python/).

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751), όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή web μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

## <a name="whats-changed"></a>Τι έχει αλλάξει
* Για οδηγίες για την αλλαγή από τοποθεσίες Web App υπηρεσία ανατρέξτε στο θέμα: [Azure εφαρμογής υπηρεσίας και τον αντίκτυπο σχετικά με τις υπάρχουσες υπηρεσίες Azure](http://go.microsoft.com/fwlink/?LinkId=529714)





 
