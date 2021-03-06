<properties 
    pageTitle="Αποστολή εξατομικευμένης ειδοποίησης με δέσμευση Mobile Azure" 
    description="Πώς μπορείτε να στείλετε εξατομικευμένης ειδοποιήσεις, συμπεριλαμβάνοντας τις πληροφορίες προφίλ χρήστη σε τις ειδοποιήσεις όπως τα ονόματά τους"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="personalize-notifications-by-including-user-name"></a>Προσαρμογή ειδοποιήσεων, συμπεριλαμβάνοντας το όνομα χρήστη

Στην έρευνά σας για να κάνετε πιο ελκυστικό ειδοποιήσεις στους χρήστες της εφαρμογής σας, πρέπει να λάβετε υπόψη εξατομίκευση τους. Ένα ισχυρό προσέγγιση αποτελείται από επιλεκτικής χρησιμοποιώντας τα ονόματα χρηστών εφαρμογή διεύθυνση τις ειδοποιήσεις για να τα κάνετε πιο προσωπικό. Προειδοποίηση εδώ - που θα πρέπει να προσεγγίσετε προσθέτοντας ονόματα χρηστών για τις ειδοποιήσεις προσεκτικά επειδή εάν έχετε υπερβολική χρήση αυτήν τη στρατηγική, στη συνέχεια, αυτό θα μπορούσε να προέρχονται κατά μήκος ως απειλητικός για ορισμένες χρήστες της εφαρμογής. Πρέπει επίσης να διασφαλίζετε ότι επιτρέποντάς ο χρήστης επιλέξετε για την παροχή αυτών των προσωπικών στοιχείων για εσάς με πλήρη συγκατάθεση εφαρμογή για κινητές συσκευές πριν να ξεκινήσετε να χρησιμοποιείτε αυτό. 

Τεχνικά, με το Azure δέσμευση Mobile, μπορείτε να επιτύχετε εξατομίκευση τις ειδοποιήσεις, ακολουθώντας τα παρακάτω βήματα με την οποία θα χρησιμοποιήσουμε το σενάριο συμπερίληψη από το όνομα χρήστη στο τις ειδοποιήσεις. Θα χρησιμοποιήσετε την έννοια της εφαρμογής πληροφορίες ή ετικέτες οι τιμές των οποίων θα μπορούσε να είτε να διαβιβαστεί από το SDK ενσωματωμένο στην εφαρμογή Mobile ή μέσω APIs. Στη συνέχεια, θα μπορούσαν να χρησιμοποιηθούν αυτών των εφαρμογών-Infos ή ετικέτες:

1. για στόχευση ειδοποιήσεις σε συγκεκριμένους χρήστες με βάση τις τιμές από τις πληροφορίες εφαρμογής ή 
2. ως σύμβολα κράτησης θέσης στις κοινοποιήσεις που θα αντικατασταθεί με τις τιμές του συσκευή/χρήστη κατά την αποστολή ειδοποιήσεων σε αυτήν τη συσκευή. 

> [AZURE.IMPORTANT] Σημειώστε ότι η ταχύτητα αποστολής ειδοποιήσεων θα βλέπουν μείωση λόγω αυτήν τη διαδικασία επιπλέον αντικατάστασης πληροφορίες app τιμών με τις ειδοποιήσεις κάθε. 

##<a name="register-app-info-in-the-mobile-engagement-portal"></a>REGISTER-πληροφορίες App στην πύλη του κινητού δέσμευση

1) Ξεκινήστε με την εγγραφή σας πληροφορίες App ή ετικέτες στην πύλη του Azure. Μεταβείτε στις **Ρυθμίσεις** -> **ετικέτα (πληροφορίες App)** για αυτό.  

![][1]  

2) Κάντε κλικ στη **νέα ετικέτα (πληροφορίες app)** και δώστε το όνομα ως *όνομα_χρήστη* και τον τύπο ως *συμβολοσειρά* και κάντε κλικ στην επιλογή **Υποβολή**. 

![][2]

3) Τέλος εμφανίζεται αυτό πληροφορίες app καταχωρηθεί όπως το εξής:

![][3]

##<a name="send-app-info-from-the-client-sdk"></a>Αποστολή εφαρμογής-πληροφοριών από το πρόγραμμα-πελάτη SDK

Εδώ χρησιμοποιούμε Windows καθολική εφαρμογή παράδειγμα αλλά ισοδύναμο μεθόδους υπάρχει επίσης για τα άλλα SDK. 

Υπό την προϋπόθεση ότι έχετε μια μέθοδο όπου λάβετε τις πληροφορίες του προφίλ από το χρήστη, όπως τα ονόματά τους πιθανώς μετά την πραγματοποίηση ελέγχου ταυτότητας τους εφαρμογή για κινητές συσκευές, θα ονομάσετε `SendAppInfo` μέθοδο εδώ και συμπληρώστε την τιμή του το `user_name` πληροφορίες app που καταχωρήσατε προηγουμένως στο υπόβαθρο υπηρεσίας δέσμευση Mobile. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##<a name="send-personalized-notifications"></a>Αποστολή εξατομικευμένης ειδοποιήσεων

Τώρα είστε έτοιμοι για την αποστολή ειδοποιήσεων χρησιμοποιώντας αυτό **όνομα_χρήστη**. 

1) Μεταβείτε στην πύλη δέσμευση Mobile στην καρτέλα **επίτευξη** για να δημιουργήσετε μια ειδοποίηση και μπορείτε να χρησιμοποιήσετε αυτήν τη θέση αντικειμένου με την εξής μορφή σε οποιοδήποτε σημείο της ειδοποίησης τίτλου ή στο σώμα. 

![][4]  

> [AZURE.NOTE] Όλοι οι χρήστες για την οποία τις πληροφορίες εφαρμογής όνομα_χρήστη δεν έχει ρυθμιστεί, δεν θα σας βοηθήσουν οποιαδήποτε ειδοποίηση. Εάν εκτελείτε την ειδοποίηση εκστρατεία σε λειτουργία δοκιμής και εάν δεν έχετε οριστεί, στη συνέχεια, θα στείλουμε πληροφορίες app '; ' χαρακτήρα για να αντικαταστήσετε το σύμβολο κράτησης θέσης. 

2) Όταν δέσμευση Mobile θα επιλέξετε μια συσκευή για να στείλετε αυτήν την ειδοποίηση, στη συνέχεια, θα δείτε αυτό πληροφορίες app και αντικαταστήστε την τιμή στο πλαίσιο κράτησης θέσης.  
Για παράδειγμα, εάν έχετε μπορούμε να εγκαταστήσουμε `str = "Scott"` για ένα χρήστη από τη συσκευή εγγραφής θα σας βοηθήσουν σχετίζονται με τις πληροφορίες εφαρμογής της **όνομα_χρήστη = SCOTT** για αυτόν το χρήστη και αυτός ο χρήστης θα δείτε μια εκτός ειδοποιήσεων push εφαρμογής με την εξής μορφή. 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png

