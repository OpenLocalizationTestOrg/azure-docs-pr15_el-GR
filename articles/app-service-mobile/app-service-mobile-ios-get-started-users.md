<properties
    pageTitle="Προσθήκη ελέγχου ταυτότητας σε iOS με εφαρμογές του Mobile Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε εφαρμογές του Mobile Azure για τον έλεγχο ταυτότητας χρηστών από την εφαρμογή iOS στις διάφορες υπηρεσίες παροχής ταυτότητας, συμπεριλαμβανομένων των AAD, Google, Facebook, Twitter και Microsoft."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-ios-app"></a>Προσθήκη ελέγχου ταυτότητας για την εφαρμογή iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να προσθέσετε τον έλεγχο ταυτότητας στο έργο [iOS γρήγορης έναρξης] χρησιμοποιώντας μια υπηρεσία παροχής ταυτότητας υποστηρίζονται. Αυτό το πρόγραμμα εκμάθησης είναι με βάση το [iOS γρήγορης έναρξης] πρόγραμμα εκμάθησης, που πρέπει να ολοκληρώσετε πρώτα.

##<a name="register"></a>Καταχώρηση της εφαρμογής για τον έλεγχο ταυτότητας και ρύθμιση παραμέτρων της εφαρμογής υπηρεσίας

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Περιορισμός δικαιωμάτων σε χρήστες με έλεγχο ταυτότητας

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Στο Xcode, πατήστε **Εκτέλεση** για να ξεκινήσετε την εφαρμογή. Εξαίρεση στρογγυλοποιείται, επειδή η εφαρμογή προσπαθεί να αποκτήσει πρόσβαση υπόβαθρο ως χωρίς έλεγχο ταυτότητας χρήστη, αλλά _TodoItem_ πίνακα τώρα απαιτεί έλεγχο ταυτότητας.

##<a name="add-authentication"></a>Προσθήκη ελέγχου ταυτότητας σε εφαρμογή

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[iOS γρήγορης εκκίνησης]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
