<properties
    pageTitle="Γρήγορα αποτελέσματα με δέσμευση Mobile Azure για εφαρμογές Web | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Mobile δέσμευση με ανάλυση και push ειδοποιήσεις για τις εφαρμογές Web."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Γρήγορα αποτελέσματα με δέσμευση Mobile Azure για εφαρμογές Web

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure Mobile δέσμευση για να κατανοήσετε την χρήση του Web App.

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ Visual Studio 2015 ή οποιοδήποτε άλλο πρόγραμμα επεξεργασίας της επιλογής σας
+ [Web SDK](http://aka.ms/P7b453) 

Αυτό το SDK Web βρίσκεται σε προεπισκόπηση και μόνο υποστηρίζει ανάλυση τη χρονική στιγμή και δεν υποστηρίζει ακόμη αποστολή ειδοποιήσεων push περιήγησης ή της εφαρμογής. 

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).

##<a name="setup-mobile-engagement-for-your-web-app"></a>Κινητές συσκευές δέσμευση ρύθμισης για την εφαρμογή Web

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Σύνδεση της εφαρμογής σας στον υπολογιστή στο παρασκήνιο Mobile δέσμευση

Αυτό το πρόγραμμα εκμάθησης παρουσιάζει μια "βασικές ενοποίησης," που είναι το ελάχιστο σύνολο που απαιτείται για τη συλλογή δεδομένων.

Θα δημιουργήσουμε μια εφαρμογή web βασικές με το Visual Studio για μια επίδειξη την ενοποίηση, αν και μπορείτε να ακολουθήσετε τα βήματα με οποιαδήποτε εφαρμογή web δημιουργήθηκε εκτός του Visual Studio επίσης. 

###<a name="create-a-new-web-app"></a>Δημιουργία νέας εφαρμογής Web

Ακολουθήστε τα παρακάτω βήματα θεωρείται ότι τη χρήση του Visual Studio 2015, αν και τα βήματα είναι παρόμοια σε παλαιότερες εκδόσεις του Visual Studio. 

1. Ξεκινήστε Visual Studio και στην **αρχική** οθόνη, επιλέξτε **Νέο έργο**.

2. Στο αναδυόμενο παράθυρο, επιλέξτε **Web** -> **Εφαρμογής Web ASP.Net**. Συμπληρώστε την εφαρμογή **όνομα**, **θέση** και το **όνομα της λύσης**, και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

3. Στο αναδυόμενο παράθυρο **Επιλέξτε ένα πρότυπο** , επιλέξτε **κενό** στην περιοχή **Πρότυπα διαίρεσης 4,5 ASP.Net** και κάντε κλικ στο κουμπί **OK**. 

Τώρα έχετε δημιουργήσει ένα νέο κενό έργο εφαρμογής Web στην οποία θα σας θα ενοποίηση του SDK Azure Mobile δέσμευση Web.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Σύνδεση της εφαρμογής σας στο κινητό δέσμευση παρασκηνίου

1. Δημιουργήστε ένα νέο φάκελο που ονομάζεται **javascript** στη λύση σας και προσθέστε το αρχείο Web SDK JS **azure engagement.js** σε αυτήν. 

2. Προσθέστε ένα νέο αρχείο που ονομάζεται **main.js** σε αυτόν το φάκελο με τον ακόλουθο κώδικα javascript. Βεβαιωθείτε ότι για να ενημερώσετε τη συμβολοσειρά σύνδεσης. Αυτό `azureEngagement` αντικείμενο θα χρησιμοποιηθεί για πρόσβαση στο Web SDK μεθόδους. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Visual Studio με τα αρχεία js][1]

##<a name="enable-real-time-monitoring"></a>Ενεργοποίηση παρακολούθησης σε πραγματικό χρόνο

Για να ξεκινήσετε την αποστολή δεδομένων και την εξασφάλιση ότι οι χρήστες είναι ενεργό, πρέπει να στείλετε τουλάχιστον μία δραστηριότητα στον υπολογιστή στο παρασκήνιο δέσμευση Mobile. Δραστηριότητα στο περιβάλλον της μια εφαρμογή web είναι μια ιστοσελίδα. 

1. Δημιουργία νέας σελίδας που ονομάζεται **home.html** στη λύση σας και ορίστε την ως την αρχική σελίδα για την εφαρμογή web σας. 
2. Συμπεριλάβετε τα δύο javascripts έχουμε προσθέσει νωρίτερα σε αυτήν τη σελίδα, προσθέτοντας την ακόλουθη μέσα στο σώμα tag. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. Ενημερώστε την ετικέτα body για την κλήση του EngagementAgent `startActivity` μέθοδος
        
        <body onload="engagement.agent.startActivity('Home')">

4. Δείτε τι σας **home.html** θα είναι όπως
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##<a name="connect-app-with-real-time-monitoring"></a>Σύνδεση εφαρμογής με παρακολούθηση σε πραγματικό χρόνο

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##<a name="extend-analytics"></a>Επέκταση ανάλυσης

Εδώ είναι προς το παρόν διαθέσιμες με SDK Web που μπορείτε να χρησιμοποιήσετε για ανάλυση όλες τις μεθόδους:

1. Δραστηριότητες/ιστοσελίδες:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Συμβάντα
        
        engagement.agent.sendEvent(name, extras);

3. Σφάλματα

        engagement.agent.sendError(name, extras);

4. Εργασίες

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

