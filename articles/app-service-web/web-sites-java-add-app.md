<properties 
    pageTitle="Προσθήκη μιας εφαρμογής Java για Azure εφαρμογής υπηρεσίας Web Apps" 
    description="Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να προσθέσετε μια σελίδα ή μια εφαρμογή για να την παρουσία του Azure υπηρεσία Web εφαρμογών που έχει ήδη ρυθμιστεί για να χρησιμοποιήσετε Java." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Προσθήκη μιας εφαρμογής Java για Azure εφαρμογής υπηρεσίας Web Apps

Αφού έχετε προετοιμασία την εφαρμογή web της Java στο [Azure εφαρμογής υπηρεσίας][] , όπως τεκμηριώνονται στη [Δημιουργία μια εφαρμογή web Java στο Azure εφαρμογής υπηρεσίας](web-sites-java-get-started.md), μπορείτε να αποστείλετε την εφαρμογή σας, τοποθετώντας το ΠΟΛΈΜΟΥ στο φάκελο **webapps** .

Η διαδρομή περιήγησης στο φάκελο **webapps** διαφέρει ανάλογα με τον τρόπο ρύθμισης σας παρουσία Web Apps.

- Εάν ρυθμίσετε την εφαρμογή web σας, χρησιμοποιώντας το Azure Marketplace, τη διαδρομή προς το φάκελο **webapps** έχει τη μορφή **d:\home\site\wwwroot\bin\application\_server\webapps**, όπου **εφαρμογή\_διακομιστή** είναι το όνομα του διακομιστή εφαρμογών σε ισχύ για την παρουσία σας Web Apps. 
- Εάν ρυθμίσετε την εφαρμογή web της με τη χρήση της ρύθμισης παραμέτρων Azure περιβάλλοντος εργασίας Χρήστη, τη διαδρομή προς το φάκελο **webapps** βρίσκεται στο τη φόρμα **d:\home\site\wwwroot\webapps**. 

Σημειώστε ότι μπορείτε να χρησιμοποιήσετε το στοιχείο ελέγχου προέλευσης για να αποστείλετε την εφαρμογή ή σελίδες web, συμπεριλαμβανομένων των [σεναρίων ενοποίησης συνεχής](app-service-continuous-deployment.md). FTP είναι επίσης μια επιλογή για την αποστολή σας εφαρμογή ή σελίδες web.

Σημείωση για τις εφαρμογές web Tomcat: Αφού έχετε αποστείλει το αρχείο ΠΟΛΈΜΟΥ στο φάκελο **webapps** , ο διακομιστής εφαρμογών Tomcat θα εντοπίσει ότι έχετε προσθέσει το και θα αυτόματα τη φόρτωση. Σημειώστε ότι, εάν αντιγράψετε τα αρχεία (εκτός από αρχεία ΠΟΛΈΜΟΥ) στον ΡΙΖΙΚΌ κατάλογο, το διακομιστή της εφαρμογής θα πρέπει να γίνει επανεκκίνηση πριν από αυτά τα αρχεία που χρησιμοποιούνται. Τη λειτουργικότητα autoload για τις εφαρμογές web Tomcat Java που εκτελείται σε Azure βασίζεται σε ένα νέο αρχείο ΠΟΛΈΜΟΥ Προσθήκη, ή νέα αρχεία ή καταλόγους προστίθενται στο φάκελο **webapps** . 

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές Java](/develop/java/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure εφαρμογής υπηρεσίας]: http://go.microsoft.com/fwlink/?LinkId=529714
