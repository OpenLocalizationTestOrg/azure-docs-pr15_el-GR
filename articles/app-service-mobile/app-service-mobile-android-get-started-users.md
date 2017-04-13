<properties
    pageTitle="Προσθήκη ελέγχου ταυτότητας σε Android με εφαρμογές του Mobile | Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε εφαρμογές του Mobile Azure εφαρμογής υπηρεσίας για τον έλεγχο ταυτότητας τους χρήστες της εφαρμογής Android στις διάφορες υπηρεσίες παροχής ταυτότητας, συμπεριλαμβανομένων των Google, Facebook, Twitter και Microsoft."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>Προσθήκη ελέγχου ταυτότητας για την εφαρμογή σας Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Σύνοψη

Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να προσθέσετε τον έλεγχο ταυτότητας στο έργο γρήγορη έναρξη todolist Android με μια υπηρεσία παροχής ταυτότητας υποστηρίζονται. Αυτό το πρόγραμμα εκμάθησης είναι με βάση το [Γρήγορα αποτελέσματα με εφαρμογές του Mobile] πρόγραμμα εκμάθησης, που πρέπει να ολοκληρώσετε πρώτα.

##<a name="register"></a>Καταχώρηση της εφαρμογής για τον έλεγχο ταυτότητας και ρύθμιση παραμέτρων της εφαρμογής υπηρεσίας

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Περιορισμός δικαιωμάτων σε χρήστες με έλεγχο ταυτότητας

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ Στο Android Studio, ανοίξτε το έργο τις προβλεπόμενες Ολοκληρώσατε με το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με εφαρμογές του Mobile]. Από το μενού " **Εκτέλεση** " κάντε κλικ στην επιλογή **Εκτέλεση εφαρμογή** και να επαληθεύσετε ότι μια ανεπίλυτη εξαίρεση με κωδικό κατάστασης της 401 (εξουσιοδότηση) ενεργοποιείται μετά την εκκίνηση της εφαρμογής.

     Αυτή η εξαίρεση συμβαίνει επειδή η εφαρμογή προσπαθεί να αποκτήσει πρόσβαση υπόβαθρο ως χωρίς έλεγχο ταυτότητας χρήστη, αλλά ο πίνακας _TodoItem_ τώρα απαιτεί έλεγχο ταυτότητας.

Στη συνέχεια, μπορείτε να ενημερώσετε την εφαρμογή για τον έλεγχο ταυτότητας χρηστών πριν από την αίτηση πόρων από την εφαρμογή Mobile παρασκηνίου.

## <a name="add-authentication-to-the-app"></a>Προσθήκη ελέγχου ταυτότητας στην εφαρμογή

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>Κωδικοί ελέγχου ταυτότητας cache του προγράμματος-πελάτη

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα που ολοκληρωθεί αυτό το πρόγραμμα εκμάθησης βασικό έλεγχο ταυτότητας, μπορείτε να συνεχίσετε με ένα από τα παρακάτω προγράμματα εκμάθησης:

+ [Προσθήκη τις ειδοποιήσεις push σας Android εφαρμογή](app-service-mobile-android-get-started-push.md) Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους του παρασκηνίου εφαρμογή Mobile για την αποστολή ειδοποιήσεων push διανομείς ειδοποίηση Azure.

+ [Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή σας Android](app-service-mobile-android-get-started-offline-data.md) Μάθετε πώς μπορείτε να προσθέσετε την εφαρμογή σας χρησιμοποιώντας μια εφαρμογή Mobile παρασκηνίου υποστήριξη χωρίς σύνδεση. Συγχρονισμού χωρίς σύνδεση επιτρέπει στους τελικούς χρήστες να αλληλεπιδράσετε με μια εφαρμογή για κινητές συσκευές&mdash;προβολή, προσθήκη ή τροποποίηση δεδομένων&mdash;ακόμα και όταν δεν υπάρχει σύνδεση δικτύου.



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Γρήγορα αποτελέσματα με εφαρμογές του Mobile]: app-service-mobile-android-get-started.md
