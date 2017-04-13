<properties 
    pageTitle="Στο Web App χωρίς ρητή (Node.js) | Microsoft Azure" 
    description="Ένα πρόγραμμα εκμάθησης που βασίζεται στα το πρόγραμμα εκμάθησης υπηρεσία cloud, και παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε τη λειτουργική μονάδα Express." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>






# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Δημιουργία μιας εφαρμογής web Node.js χρησιμοποιώντας Express σε μια υπηρεσία Cloud Azure

Node.js περιλαμβάνει ένα ελάχιστο σύνολο λειτουργικότητας στο χρόνο εκτέλεσης πυρήνα.
Οι προγραμματιστές χρησιμοποιούν συχνά λειτουργικές μονάδες τρίτου κατασκευαστή για να παρέχουν πρόσθετες λειτουργίες κατά την ανάπτυξη μιας εφαρμογής Node.js. Σε αυτό το πρόγραμμα εκμάθησης που θα δημιουργήσετε μια νέα εφαρμογή χρησιμοποιώντας [Express][] λειτουργική μονάδα, που παρέχει ένα πλαίσιο MVC για τη δημιουργία εφαρμογών web Node.js.

Στιγμιότυπο οθόνης της εφαρμογής ολοκληρωμένη είναι κάτω από:

![Ένα πρόγραμμα περιήγησης web που εμφανίζει Καλώς ορίσατε στο Express στο Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##<a name="create-a-cloud-service-project"></a>Δημιουργία έργου υπηρεσίας Cloud

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε ένα νέο έργο υπηρεσιών cloud με το όνομα 'expressapp':

1. Από το **Μενού Έναρξη** ή η **Οθόνη έναρξης**, αναζήτηση για το **Windows PowerShell**. Τέλος, κάντε δεξί κλικ **Του Windows PowerShell** και επιλέξτε **Εκτέλεση ως διαχειριστής**.

    ![Εικονίδιο Azure PowerShell](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. Αλλαγή σε καταλόγους για να το **c:\\κόμβου** καταλόγου και, στη συνέχεια, καταχωρήστε τις παρακάτω εντολές για να δημιουργήσετε μια νέα λύση με το όνομα **expressapp** και ένα ρόλο web με το όνομα **WebRole1**:

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] Από προεπιλογή, η **Προσθήκη AzureNodeWebRole** χρησιμοποιεί μια παλαιότερη έκδοση του Node.js. Το παραπάνω **Σύνολο AzureServiceProjectRole** δήλωση καθοδηγεί Azure για να χρησιμοποιήσετε v0.10.21 κόμβου.  Σημείωση Οι παράμετροι διάκριση πεζών-κεφαλαίων.  Μπορείτε να επαληθεύσετε τη σωστή έκδοση του Node.js έχει επιλεγεί, επιλέγοντας την ιδιότητα **μηχανισμών** στο **WebRole1\package.json**.

##<a name="install-express"></a>Εγκατάσταση του Express

1. Εγκαταστήστε τη γεννήτρια Express με την έκδοση την ακόλουθη εντολή:

        PS C:\node\expressapp> npm install express-generator -g

    Το αποτέλεσμα της εντολής npm πρέπει να είναι παρόμοιο με το παρακάτω αποτέλεσμα. 

    ![Εμφανίζει το αποτέλεσμα του npm του Windows PowerShell express εντολής εγκατάστασης.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Αλλαγή σε καταλόγους στον κατάλογο **WebRole1** και χρησιμοποιήστε την εντολή express για να δημιουργήσετε μια νέα εφαρμογή:

        PS C:\node\expressapp\WebRole1> express

    Θα σας ζητηθεί να αντικαταστήσετε την παλαιότερη εφαρμογή. Πληκτρολογήστε **y** ή **Ναι** για να συνεχίσετε. Express θα δημιουργήσει το αρχείο app.js και μια δομή φακέλου για τη δημιουργία της εφαρμογής σας.

    ![Το αποτέλεσμα της εντολής express](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  Για να εγκαταστήσετε επιπλέον εξαρτήσεις ορίζονται στο αρχείο package.json, εισαγάγετε την ακόλουθη εντολή:

        PS C:\node\expressapp\WebRole1> npm install

    ![Το αποτέλεσμα του npm εντολής εγκατάστασης](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  Χρησιμοποιήστε την παρακάτω εντολή για να αντιγράψετε το αρχείο **Ανακύκλωσης/www** **server.js**. Αυτό είναι, ώστε να την υπηρεσία cloud να βρείτε το σημείο εισόδου για αυτήν την εφαρμογή.

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    Αφού ολοκληρωθεί η αυτή την εντολή, θα πρέπει να έχετε ένα αρχείο **server.js** στον κατάλογο WebRole1.

7.  Τροποποίηση του **server.js** για να καταργήσετε ένα από τα '.' χαρακτήρων από την ακόλουθη γραμμή.

        var app = require('../app');

    Μετά την πραγματοποίηση αυτής της τροποποίησης, η γραμμή θα πρέπει να εμφανίζεται ως εξής.

        var app = require('./app');

    Αυτή η αλλαγή απαιτείται δεδομένου ότι θα σας μετακινήσει το αρχείο (πρώην **Ανακύκλωσης/www**,) στον ίδιο κατάλογο με το αρχείο εφαρμογής που απαιτούνται. Μετά την πραγματοποίηση αυτής της αλλαγής, αποθηκεύστε το αρχείο **server.js** .

8.  Χρησιμοποιήστε την ακόλουθη εντολή για να εκτελέσετε την εφαρμογή σε το Azure προσομοίωσης:

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![Μια ιστοσελίδα που περιέχει Καλώς ορίσατε στο express.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>Τροποποίηση της προβολής

Τώρα μπορείτε να τροποποιήσετε την προβολή για να εμφανιστεί το μήνυμα "Καλώς ορίσατε στο Express στο Azure".

1.  Πληκτρολογήστε την παρακάτω εντολή για να ανοίξετε το αρχείο index.jade:

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![Τα περιεχόμενα του αρχείου index.jade.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Νεφρίτης είναι ο προεπιλεγμένος μηχανισμός προβολή που χρησιμοποιείται από εφαρμογές Express. Για περισσότερες πληροφορίες σχετικά με το μηχανισμό Jade προβολής, ανατρέξτε στο θέμα [http://jade-lang.com][].

2.  Τροποποιήστε την τελευταία γραμμή του κειμένου προσαρτώντας **στο Azure**.

    ![Το αρχείο index.jade, την τελευταία γραμμή διαβάζει: p Καλώς ορίσατε στο \#{τίτλος} στο Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  Αποθηκεύστε το αρχείο και κλείστε το Σημειωματάριο.

4.  Ανανεώστε το πρόγραμμα περιήγησης και θα δείτε τις αλλαγές σας.

    ![Ένα παράθυρο προγράμματος περιήγησης, η σελίδα περιέχει Καλώς ορίσατε στο Express στο Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Μετά τη δοκιμή της εφαρμογής, χρησιμοποιήστε το cmdlet **Διακοπή AzureEmulator** για να διακόψετε την προσομοίωσης.

##<a name="publishing-the-application-to-azure"></a>Δημοσίευση της εφαρμογής σε Azure

Στο παράθυρο Azure PowerShell, χρησιμοποιήστε το cmdlet **Δημοσίευση AzureServiceProject** για την ανάπτυξη της εφαρμογής σε μια υπηρεσία στο cloud

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Μόλις ολοκληρωθεί η λειτουργία ανάπτυξη, το πρόγραμμα περιήγησης θα ανοίξουν και να εμφανίσετε την ιστοσελίδα.

![Εμφάνιση της σελίδας Express ένα πρόγραμμα περιήγησης web. Η διεύθυνση URL υποδεικνύει τώρα φιλοξενούνται σε Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές του Node.js](/develop/nodejs/).

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://Jade-Lang.com]: http://jade-lang.com

 
