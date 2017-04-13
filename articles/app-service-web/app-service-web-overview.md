<properties
    pageTitle="Επισκόπηση εφαρμογές Web | Microsoft Azure"
    description="Μάθετε πώς Azure εφαρμογής υπηρεσίας σάς βοηθά να αναπτύξετε και εφαρμογές web κεντρικού υπολογιστή"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Επισκόπηση εφαρμογών Web

*Εφαρμογή υπηρεσίας Web Apps* είναι μια πλατφόρμα πλήρως διαχειριζόμενων υπολογισμού που είναι βελτιστοποιημένη για τη φιλοξενία εφαρμογών web και τοποθεσίες Web που διαθέτετε. Αυτήν την προσφορά [πλατφόρμα ως-a-υπηρεσίας](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) της Microsoft Azure σάς επιτρέπει να που εστιάσετε της επιχειρηματικής λογικής σας ενώ Azure αναλαμβάνει της υποδομής του για να εκτελέσετε και να περιορίσετε το μέγεθος των εφαρμογών σας.

5 λεπτών βίντεο που ακολουθεί παρουσιάζει Azure εφαρμογής υπηρεσίας Web Apps.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>Τι είναι μια εφαρμογή web στην υπηρεσία εφαρμογή;

Στην εφαρμογή υπηρεσίας, μια *εφαρμογή web* είναι οι πόροι υπολογισμού που παρέχει το Azure για τη φιλοξενία μιας τοποθεσίας Web ή μιας εφαρμογής web.  

Οι πόροι υπολογισμού μπορεί να είναι σε κοινόχρηστη ή αποκλειστική εικονικές μηχανές (ΣΠΣ), ανάλογα με τη σειρά τιμολόγησης που επιλέγετε. Κώδικα της εφαρμογής σας εκτελείται σε ένα διαχειριζόμενο Εικονική που απομόνωσης από τους άλλους πελάτες.

Ο κώδικας μπορεί να είναι σε οποιαδήποτε γλώσσα ή framework που υποστηρίζεται από το [Azure εφαρμογής υπηρεσίας](../app-service/app-service-value-prop-what-is.md), όπως το ASP.NET, Node.js, Java, PHP ή Python. Μπορείτε επίσης να εκτελέσετε τις δέσμες ενεργειών που χρησιμοποιούν [PowerShell και άλλες γλώσσες δεσμών ενεργειών](web-sites-create-web-jobs.md#acceptablefiles) σε μια εφαρμογή web.

Για παραδείγματα σεναρίων τυπική εφαρμογή που μπορείτε να χρησιμοποιήσετε εφαρμογών για το Web, ανατρέξτε στο θέμα [σενάρια εφαρμογής Web](https://azure.microsoft.com/documentation/scenarios/web-app/) και στην ενότητα **σενάρια και συστάσεις** του [Azure εφαρμογής υπηρεσίας, σύγκρισης εικονικές μηχανές, ύφασμα υπηρεσίας, και τις υπηρεσίες Cloud](choose-web-site-cloud-service-vm.md#scenarios).

## <a name="why-use-web-apps"></a>Γιατί να χρησιμοποιήσετε εφαρμογές Web;

Εδώ θα βρείτε ορισμένες βασικές δυνατότητες της εφαρμογής υπηρεσίας που ισχύουν για εφαρμογές Web:

- **Πολλές γλώσσες και πλαίσια** - εφαρμογής υπηρεσίας έχει κορυφαία υποστήριξη για ASP.NET, Node.js, Java, PHP και Python. Μπορείτε επίσης να εκτελέσετε [PowerShell και άλλες δέσμες ενεργειών ή εκτελέσιμα αρχεία](../app-service-web/web-sites-create-web-jobs.md) στην εφαρμογή υπηρεσίας ΣΠΣ.

- **Βελτιστοποίηση DevOps** - ρύθμιση [συνεχής ενοποίησης και ανάπτυξη](../app-service-web/app-service-continuous-deployment.md) με το Visual Studio Team Services, GitHub ή BitBucket. Προβιβασμός ενημερώσεις με [δοκιμή και ενδιάμεσου περιβάλλοντα](../app-service-web/web-sites-staged-publishing.md). Εκτέλεση [A / B δοκιμές](../app-service-web/app-service-web-test-in-production-get-start.md). Διαχειριστείτε τις εφαρμογές σας στην εφαρμογή υπηρεσίας με τη χρήση [Του PowerShell Azure](../powershell-install-configure.md) ή το [περιβάλλον γραμμής εντολών πλατφόρμες (CLI)](../xplat-cli-install.md).

- **Καθολικό πρότυπο κλιμάκωση με υψηλή διαθεσιμότητα** - κλίμακας [προς τα επάνω](../app-service-web/web-sites-scale.md) ή προς τα [έξω](../monitoring-and-diagnostics/insights-how-to-scale.md) αυτόματα ή με μη αυτόματο τρόπο. Φιλοξενεί τις εφαρμογές σας σε οποιοδήποτε σημείο της υποδομής καθολικού κέντρο δεδομένων της Microsoft και της εφαρμογής υπηρεσίας [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) υπόσχεται υψηλής διαθεσιμότητας.

- **Συνδέσεις σε πλατφόρμες ΑΔΑ και τα δεδομένα εσωτερικής εγκατάστασης** - επιλέξτε από περισσότερα από 50 [γραμμές σύνδεσης](../connectors/apis-list.md) για συστήματα για μεγάλες επιχειρήσεις (όπως SAP, το Siebel και Oracle), ΑΔΑ υπηρεσίες (όπως Salesforce και Office 365) και τις υπηρεσίες internet (όπως Facebook και Twitter). Access εσωτερικής δεδομένων με χρήση [Υβριδική συνδέσεις](../biztalk-services/integration-hybrid-connection-overview.md) και [Azure εικονικού δίκτυα](../app-service-web/web-sites-integrate-with-vnet.md).

- **Ασφάλεια και συμμόρφωση** - εφαρμογής υπηρεσίας είναι [ISO, SOC, και PCI συμβατή](https://www.microsoft.com/TrustCenter/).

- **Πρότυπα εφαρμογών** - επιλέξτε από μια μεγάλη λίστα εφαρμογή προτύπων από το [Azure Marketplace](https://azure.microsoft.com/marketplace/) που σας επιτρέπουν να χρησιμοποιήσετε έναν οδηγό για να εγκαταστήσετε το λογισμικό δημοφιλείς ανοιχτού κώδικα όπως WordPress, Joomla και Drupal.

- **Ενοποίηση του visual Studio** - αποκλειστικός εργαλεία στο Visual Studio βελτιστοποιήσετε την εργασία τη δημιουργία, την ανάπτυξη και τον εντοπισμό σφαλμάτων.

Επιπλέον, μια εφαρμογή web να επωφεληθείτε από δυνατότητες που παρέχεται από [Εφαρμογές API](../app-service-api/app-service-api-apps-why-best-platform.md) (όπως CORS υποστήριξης) και τις [Εφαρμογές του Mobile](../app-service-mobile/app-service-mobile-value-prop.md) (όπως τις ειδοποιήσεις push). Για περισσότερες πληροφορίες σχετικά με τους τύπους εφαρμογής στην εφαρμογή υπηρεσίας, ανατρέξτε στο θέμα [Επισκόπηση Azure εφαρμογής υπηρεσίας](../app-service/app-service-value-prop-what-is.md).

Εκτός από το Web Apps στο εφαρμογής υπηρεσίας, Azure προσφέρει άλλες υπηρεσίες που μπορούν να χρησιμοποιηθούν για τη φιλοξενία εφαρμογών web και τοποθεσίες Web που διαθέτετε. Για περισσότερα σενάρια, οι εφαρμογές Web είναι η καλύτερη επιλογή.  Για την αρχιτεκτονική microservice, εξετάστε το ενδεχόμενο να [Ύφασμα υπηρεσίας](https://azure.microsoft.com/documentation/services/service-fabric)και, εάν χρειάζεστε περισσότερο έλεγχο του ΣΠΣ που εκτελεί τον κώδικα στην, εξετάστε το ενδεχόμενο [εικονικές μηχανές Windows Azure](https://azure.microsoft.com/documentation/services/virtual-machines/). Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να επιλέξετε μεταξύ αυτών των υπηρεσιών Azure, ανατρέξτε στο θέμα [Azure εφαρμογής υπηρεσίας, εικονικές μηχανές, ύφασμα υπηρεσίας, και τις υπηρεσίες Cloud σύγκρισης](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Γρήγορα αποτελέσματα

Για να ξεκινήσετε από την ανάπτυξη δείγμα κώδικα σε μια νέα εφαρμογή web στην εφαρμογή υπηρεσίας, ακολουθήστε το πρόγραμμα εκμάθησης [Ανάπτυξη την πρώτη εφαρμογή web Azure σε 5 λεπτά](app-service-web-get-started.md) . Θα χρειαστείτε έναν δωρεάν λογαριασμό Azure.

Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751), όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή web μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.
