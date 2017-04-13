<properties 
    pageTitle="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή Web με εφαρμογή υπηρεσίας στην Linux | Microsoft Azure" 
    description="Web εφαρμογή δημιουργίας τη ροή εργασίας για εφαρμογή υπηρεσία ή Linux." 
    keywords="Azure εφαρμογής υπηρεσίας, εφαρμογή web, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="create-a-web-app-with-app-service-on-linux"></a>Δημιουργία εφαρμογής Web με εφαρμογή υπηρεσίας στην Linux

## <a name="using-the-management-portal-to-create-your-web-app"></a>Χρησιμοποιώντας την πύλη διαχείρισης για να δημιουργήσετε την εφαρμογή web
Μπορείτε να ξεκινήσετε τη δημιουργία του Web App σε Linux από την [πύλη διαχείρισης](https://portal.azure.com) , όπως φαίνεται στην παρακάτω εικόνα.

![][1]

Αφού επιλέξετε την παρακάτω επιλογή, θα εμφανιστούν τα blade δημιουργία όπως φαίνεται στην παρακάτω εικόνα. 

![][2]

-   Ονομάστε την εφαρμογή web.
-   Επιλέξτε μια υπάρχουσα ομάδα πόρων ή δημιουργήστε ένα νέο. (Ανατρέξτε στο θέμα περιοχών που είναι διαθέσιμες στην [ενότητα "περιορισμοί"](./app-service-linux-intro.md)).
-   Επιλέξτε ένα υπάρχον σχέδιο της εφαρμογής υπηρεσίας ή δημιουργήστε ένα νέο μία (ανατρέξτε στο θέμα εφαρμογή υπηρεσίας πρόγραμμα σημειώσεις στην [ενότητα "περιορισμοί"](./app-service-linux-intro.md)). 
-   Επιλέξτε το στοίβα της εφαρμογής που σκοπεύετε να χρησιμοποιήσετε. Θα λάβετε για να επιλέξετε ανάμεσα σε πολλές εκδόσεις Node.js και PHP. 

Όταν έχετε την εφαρμογή που δημιουργήσατε, μπορείτε να αλλάξετε το στοίβα της εφαρμογής από των ρυθμίσεων της εφαρμογής, όπως φαίνεται στην παρακάτω εικόνα.

![][3]

## <a name="deploying-your-web-app"></a>Για την ανάπτυξη της εφαρμογής web σας

Επιλογή του στοιχείου "Επιλογές ανάπτυξης" από την πύλη διαχείρισης σάς δίνει την επιλογή για να χρησιμοποιήσετε τοπική ένα αποθετήριο Git ή ένα αποθετήριο GitHub για να αναπτύξετε την εφαρμογή σας. Τις οδηγίες στη συνέχεια είναι παρόμοια σε μια εφαρμογή web μη Linux και μπορείτε να ακολουθήσετε τις οδηγίες στο θέμα μας [τοπικό Git ανάπτυξης](./app-service-deploy-local-git.md) ή μας άρθρο [συνεχούς ανάπτυξης](./app-service-continuous-deployment.md) για GitHub.

Μπορείτε επίσης να χρησιμοποιήσετε FTP για να αποστείλετε την εφαρμογή σας στην τοποθεσία σας. Μπορείτε να λάβετε το τελικό σημείο FTP για την εφαρμογή web από την ενότητα αρχεία καταγραφής διαγνωστικών, όπως φαίνεται στην παρακάτω εικόνα.

![][4]


## <a name="next-steps"></a>Επόμενα βήματα ##

* [Τι είναι η υπηρεσία εφαρμογής στην Linux;](./app-service-linux-intro.md)
* [Χρήση ΑΣ2 ρύθμισης παραμέτρων για Node.js στο Web Apps σε Linux](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
