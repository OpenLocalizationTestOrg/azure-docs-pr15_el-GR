<properties 
    pageTitle="Python στο web app με Django στην Linux | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να φιλοξενήσετε μια εφαρμογή web βάσει Django στον Azure χρησιμοποιώντας μια εικονική μηχανή Linux." 
    services="virtual-machines-linux" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="huvalo"/>
    
# <a name="django-hello-world-web-application-on-a-linux-vm"></a>Εφαρμογή web Django Γεια σε μια Εικονική Linux

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac/Linux](virtual-machines-linux-python-django-web-app.md)

<br>

Αυτό το πρόγραμμα εκμάθησης περιγράφει πώς μπορείτε να φιλοξενήσετε μια τοποθεσία Web με βάση Django στο Microsoft Azure χρησιμοποιώντας μια εικονική μηχανή Linux. Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε χωρίς προηγούμενη εμπειρία με χρήση Azure. Κατά την ολοκλήρωση αυτού του οδηγού, θα έχετε μια εφαρμογή που βασίζεται σε Django προς τα επάνω και την εκτέλεση στο cloud.

Θα μάθετε πώς μπορείτε να:

* Μια εικονική μηχανή Azure κεντρικό υπολογιστή Django το πρόγραμμα εγκατάστασης. Ενώ αυτό το πρόγραμμα εκμάθησης, εξηγεί πώς μπορείτε να πραγματοποιήσετε αυτές τις στην περιοχή **Linux**μπορεί επίσης να κάνετε το ίδιο με μια Εικονική διακομιστή των Windows που φιλοξενούνται στο Azure. 
* Δημιουργία νέας εφαρμογής Django από Linux.

Ακολουθώντας αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να δημιουργήσετε μια απλή εφαρμογή Γεια στο web. Η εφαρμογή θα να φιλοξενούνται σε μια εικονική μηχανή Azure.

Στιγμιότυπο οθόνης της εφαρμογής ολοκληρωμένη είναι κάτω από:

![Εμφάνιση της σελίδας κόσμο hello στην Azure παράθυρο του προγράμματος περιήγησης](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Δημιουργία και ρύθμιση παραμέτρων ενός Azure εικονική μηχανή κεντρικό υπολογιστή Django

1. Ακολουθήστε τις οδηγίες δεδομένη [εδώ](virtual-machines-linux-quick-create-portal.md) για να δημιουργήσετε μια εικονική μηχανή Azure της κατανομής *Ubuntu διακομιστή 14.04 Αποτελεσμάτων* .  Εάν προτιμάτε, μπορείτε να επιλέξετε τον κωδικό πρόσβασης τον έλεγχο ταυτότητας αντί για το δημόσιο κλειδί SSH.

1. Επεξεργαστείτε την ομάδα ασφαλείας δικτύου για να επιτρέψετε την εισερχόμενη κυκλοφορία http στη θύρα 80 ακολουθώντας τις οδηγίες [εδώ](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

1. Από προεπιλογή, το νέο εικονική μηχανή δεν διαθέτει ένα πλήρως προσδιορισμένο όνομα τομέα (FQDN).  Μπορείτε να δημιουργήσετε μία ακολουθώντας τις οδηγίες [εδώ](virtual-machines-linux-portal-create-fqdn.md).  Αυτό το βήμα είναι προαιρετικό για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης.

## <a id="setup"> </a>Ρυθμίζοντας το περιβάλλον ανάπτυξης

**Σημείωση:** Εάν πρέπει να εγκαταστήσετε Python ή θέλετε να χρησιμοποιήσετε τις βιβλιοθήκες προγράμματος-πελάτη, ανατρέξτε στο θέμα ο [Οδηγός εγκατάστασης Python](../python-how-to-install.md).

Παρέχεται ήδη η Εικονική Linux Ubuntu με 2.7 Python προ-εγκατεστημένο, αλλά δεν έχει Apache ή django εγκατεστημένο.  Ακολουθήστε τα παρακάτω βήματα για να συνδεθείτε για να σας Εικονική και εγκαταστήστε Apache και django.

1.  Ξεκινήστε ένα νέο παράθυρο **Terminal** .
    
1.  Πληκτρολογήστε την παρακάτω εντολή για να συνδεθείτε με το Azure Εικονική.  Εάν δεν μπορείτε να δημιουργήσετε ένα FQDN, μπορείτε να συνδεθείτε χρησιμοποιώντας τη δημόσια διεύθυνση IP που εμφανίζεται σε εικονική μηχανή της σύνοψης στην πύλη του Azure κλασική.

        $ ssh yourusername@yourVmUrl

1.  Εισαγάγετε τις παρακάτω εντολές για να εγκαταστήσετε το django:

        $ sudo apt-get install python-setuptools python-pip
        $ sudo pip install django

1.  Πληκτρολογήστε την παρακάτω εντολή για να εγκαταστήσετε Apache με mod wsgi:

        $ sudo apt-get install apache2 libapache2-mod-wsgi


## <a name="creating-a-new-django-application"></a>Δημιουργία νέας εφαρμογής Django

1.  Ανοίξτε το παράθυρο **Terminal** που χρησιμοποιήσατε στην προηγούμενη ενότητα για να ssh σε Εικονική σας.
    
1.  Εισαγάγετε τις παρακάτω εντολές για να δημιουργήσετε ένα νέο έργο Django:

        $ cd /var/www
        $ sudo django-admin.py startproject helloworld

    Η δέσμη ενεργειών **django admin.py** δημιουργεί μια βασική δομή για τοποθεσίες Web που βασίζονται σε Django:
    -   **helloworld/Manage.PY** σάς βοηθά να ξεκινήσετε φιλοξενίας και να διακόψετε τη φιλοξενία της τοποθεσίας Web που βασίζονται σε Django
    -   **helloworld/helloworld/Settings.PY** περιέχει Django ρυθμίσεις για την εφαρμογή σας.
    -   **helloworld/helloworld/URLs.PY** περιέχει τον κωδικό αντιστοίχισης μεταξύ κάθε διεύθυνση url και την προβολή.

1.  Δημιουργήστε ένα νέο αρχείο με το όνομα **views.py** στον κατάλογο **/var/www/helloworld/helloworld** . Αυτό θα περιέχει την προβολή που αποδίδει τη σελίδα "Χαίρετε". Ξεκινήστε το πρόγραμμα επεξεργασίας και πληκτρολογήστε τα εξής:
        
        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Τώρα αντικαταστήστε τα περιεχόμενα του αρχείου **urls.py** με τα εξής:

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )


## <a name="setting-up-apache"></a>Ρύθμιση Apache

1.  Δημιουργήστε μια εικονική host Apache ρύθμισης παραμέτρων αρχείου **/etc/apache2/sites-available/helloworld.conf**. Ορίστε το περιεχόμενο με την ακόλουθη και αντικαταστήστε *yourVmName* με το πραγματικό όνομα του υπολογιστή που χρησιμοποιείτε (για παράδειγμα *pyubuntu*).

        <VirtualHost *:80>
        ServerName yourVmName
        </VirtualHost>
        WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
        WSGIPythonPath /var/www/helloworld

1.  Ενεργοποίηση της τοποθεσίας με την ακόλουθη εντολή:

        $ sudo a2ensite helloworld

1.  Επανεκκινήστε Apache με την ακόλουθη εντολή:

        $ sudo service apache2 reload

1.  Τέλος, φόρτωση της ιστοσελίδας στο πρόγραμμα περιήγησης:

    ![Ένα παράθυρο προγράμματος περιήγησης που εμφανίζει τη σελίδα κόσμο hello στο Azure](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)


## <a name="shutting-down-your-azure-virtual-machine"></a>Τερματισμός του Azure εικονική μηχανή

Όταν ολοκληρώσετε την εργασία με αυτό το πρόγραμμα εκμάθησης, τερματισμού ή/και κατάργηση σας που έχουν δημιουργηθεί πρόσφατα Azure εικονική μηχανή για να αποδεσμεύσετε πόρους για άλλα προγράμματα εκμάθησης και να αποφύγετε τις χρεώσεις Azure χρήση.
