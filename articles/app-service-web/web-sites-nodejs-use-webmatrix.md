<properties 
    pageTitle="Δημιουργήστε και αναπτύξτε μια εφαρμογή web Node.js στον Azure χρησιμοποιώντας WebMatrix" 
    description="Ένα πρόγραμμα εκμάθησης που σας μαθαίνει πώς να χρησιμοποιείτε το WebMatrix για την ανάπτυξη μιας εφαρμογής Node.js και να αναπτύξετε για Azure εφαρμογής υπηρεσίας Web Apps." 
    services="app-service\web" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>Δημιουργήστε και αναπτύξτε μια εφαρμογή web Node.js στον Azure χρησιμοποιώντας WebMatrix

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε WebMatrix για την ανάπτυξη μιας εφαρμογής Node.js και να αναπτύξετε για [Azure εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps. WebMatrix είναι ένα εργαλείο ανάπτυξης δωρεάν web από τη Microsoft που περιλαμβάνει όλα όσα χρειάζεστε για την ανάπτυξη εφαρμογών τοποθεσίας Web ή web. WebMatrix περιλαμβάνει πολλές δυνατότητες που να είναι εύκολο για χρήση Node.js όπως ολοκλήρωσης κώδικα, προ-δομημένα πρότυπα και υποστήριξη προγράμματος επεξεργασίας για Νεφρίτης, ΛΙΓΌΤΕΡΟ και CoffeeScript. Μάθετε περισσότερα σχετικά με το [WebMatrix](https://www.microsoft.com/web/webmatrix/).

Κατά την ολοκλήρωση αυτού του οδηγού, θα έχετε μια εφαρμογή web Node.js εκτελείται στο Azure εφαρμογής υπηρεσίας.
 
Στιγμιότυπο οθόνης της εφαρμογής ολοκληρωμένη είναι κάτω από:

![Τοποθεσία Web Azure κόμβου][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751), όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή web μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

## <a name="sign-into-azure"></a>Πραγματοποιήστε είσοδο στο Azure

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας.

1. Εκκίνηση WebMatrix
2. Εάν αυτή είναι η πρώτη φορά που χρησιμοποιήσατε WebMatrix, θα σας ζητηθεί να συνδεθείτε στο Azure.  Διαφορετικά, μπορείτε να κάντε κλικ στο κουμπί **Sign In** και επιλέξτε **Προσθήκη λογαριασμού**.  Επιλέξτε για να **εισέλθετε** χρησιμοποιώντας το λογαριασμό της Microsoft.

    ![Προσθήκη λογαριασμού][addaccount]

3. Εάν έχετε εγγραφεί για ένα λογαριασμό Azure, ενδέχεται να μπορείτε να συνδεθείτε χρησιμοποιώντας το λογαριασμό της Microsoft:

    ![Πραγματοποιήστε είσοδο στο Azure][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>Δημιουργία τοποθεσίας με χρήση του προτύπου ενσωματωμένη για Azure

1. Στην οθόνη έναρξης, κάντε κλικ στο κουμπί **Δημιουργία** και επιλέξτε **Συλλογή προτύπων** για να δημιουργήσετε μια νέα τοποθεσία από τη συλλογή προτύπων:

    ![Νέα τοποθεσία από τη συλλογή προτύπων][sitefromtemplate]

2. Στο παράθυρο διαλόγου **τοποθεσίας από πρότυπο** , επιλέξτε **κόμβο** και, στη συνέχεια, επιλέξτε **Την τοποθεσία Express**. Τέλος, κάντε κλικ στο κουμπί **Επόμενο**. Εάν λείπει οποιοδήποτε τις προϋποθέσεις για το πρότυπο **Τοποθεσίας Express** , θα σας ζητηθεί να τις εγκαταστήσετε.

    ![Επιλέξτε το πρότυπο express][webmatrix-templates]

3. Εάν έχετε εισέλθει στο Azure, τώρα έχετε την επιλογή για να δημιουργήσετε μια εφαρμογή web της εφαρμογής υπηρεσίας για τοπική τοποθεσία σας.  Επιλέξτε ένα μοναδικό όνομα και επιλέξτε το κέντρο δεδομένων όπου θέλετε να δημιουργηθεί την εφαρμογή web της εφαρμογής υπηρεσίας: 

    ![Δημιουργία τοποθεσίας σε Azure][nodesitefromtemplateazure]
    
4. Αφού WebMatrix ολοκληρωθεί η δημιουργία της τοπικής τοποθεσίας και τη δημιουργία της εφαρμογής υπηρεσίας web app, εμφανίζεται το IDE WebMatrix.

    ![webmatrix ide][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>Δημοσιεύστε την εφαρμογή με το Azure

1. Στο WebMatrix, κάντε κλικ στην επιλογή " **Δημοσίευση** " από την κορδέλα **για οικιακή χρήση** για να εμφανίσετε το παράθυρο διαλόγου **Δημοσίευση προεπισκόπησης** για την τοποθεσία.

    ![δημοσίευση preview][webmatrix-node-publishpreview]

2. Κάντε κλικ στο κουμπί **συνέχεια**. Όταν ολοκληρωθεί η δημοσίευση, τη διεύθυνση URL για την εφαρμογή υπηρεσίας web app εμφανίζεται στο κάτω μέρος του IDE WebMatrix

    ![δημοσίευση ολοκλήρωσης][webmatrix-publish-complete]

3. Κάντε κλικ στη σύνδεση για να ανοίξετε την εφαρμογή υπηρεσίας web app στο πρόγραμμα περιήγησης.

    ![Express εφαρμογής web][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>Τροποποιήστε και δημοσιεύστε ξανά την εφαρμογή σας

Μπορείτε εύκολα να τροποποιήσετε και να δημοσιεύσετε ξανά την εφαρμογή σας. Εδώ, θα μπορείτε να κάνετε μια απλή αλλαγή στην επικεφαλίδα στο αρχείο **index.jade** και δημοσιεύστε ξανά την εφαρμογή.

1. Στο WebMatrix, επιλέξτε **αρχεία**και, στη συνέχεια, αναπτύξτε το φάκελο **προβολές** . Ανοίξτε το αρχείο **index.jade** κάνοντας διπλό κλικ επάνω του.

    ![index.jade προβολή webmatrix][webmatrix-modify-index]

2. Αλλάξτε τη γραμμή της παραγράφου με το εξής:

        p Welcome to #{title} with WebMatrix on Azure!

3. Αποθηκεύστε τις αλλαγές σας και, στη συνέχεια, κάντε κλικ στο εικονίδιο "Δημοσίευση". Τέλος, κάντε κλικ στην επιλογή " **συνέχεια** " στο παράθυρο διαλόγου **Δημοσίευση προεπισκόπηση** και περιμένετε να να δημοσιευτεί η ενημέρωση.

    ![δημοσίευση preview][webmatrix-republish]

4. Όταν ολοκληρωθεί η δημοσίευση, χρησιμοποιήστε τη σύνδεση που επιστρέφονται όταν ολοκληρώνεται η διαδικασία δημοσίευσης για να δείτε το ενημερωμένο εφαρμογής υπηρεσίας web app.

    ![Εφαρμογή web Azure κόμβου][webmatrix-node-completed]

##<a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με τις εκδόσεις του Node.js που παρέχονται με το Azure και πώς μπορείτε να καθορίσετε την έκδοση που θα χρησιμοποιηθεί με την εφαρμογή σας, ανατρέξτε στο θέμα [που καθορίζει μια έκδοση Node.js σε μια εφαρμογή του Azure](../nodejs-specify-node-version-azure-apps.md).

Εάν αντιμετωπίσετε προβλήματα με την εφαρμογή σας αφού έχει αναπτυχθεί σε Azure, ανατρέξτε στο θέμα [πώς μπορείτε να εντοπίσετε σφάλματα σε μια εφαρμογή web Node.js στο Azure εφαρμογής υπηρεσίας](web-sites-nodejs-debug.md) για πληροφορίες σχετικά με τη διάγνωση του προβλήματος.

## <a name="whats-changed"></a>Τι έχει αλλάξει
* Για οδηγίες για την αλλαγή από τοποθεσίες Web App υπηρεσία ανατρέξτε στο θέμα: [Azure εφαρμογής υπηρεσίας και τον αντίκτυπο σχετικά με τις υπάρχουσες υπηρεσίες Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 