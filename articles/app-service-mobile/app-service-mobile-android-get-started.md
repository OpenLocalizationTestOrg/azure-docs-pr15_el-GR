<properties
    pageTitle="Δημιουργία εφαρμογής Android στο Azure εφαρμογής υπηρεσίας εφαρμογές για κινητές συσκευές | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης για να ξεκινήσετε με τη χρήση της εφαρμογής για κινητές συσκευές Azure παρασκήνιο για Android ανάπτυξη"
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

#<a name="create-an-android-app"></a>Δημιουργία εφαρμογής Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να προσθέσετε μια υπηρεσία υποστήριξης που βασίζεται στο cloud για μια εφαρμογή για κινητές συσκευές Android, χρησιμοποιώντας μια εφαρμογή για κινητές συσκευές Azure παρασκηνίου.  Θα δημιουργήσετε έναν νέο υπολογιστή στο παρασκήνιο εφαρμογής για κινητές συσκευές και μια απλή _λίστα Todo_ εφαρμογή Android που αποθηκεύει δεδομένα εφαρμογής στο Azure.

Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλους άλλα Android προγράμματα εκμάθησης σχετικά με τη χρήση της δυνατότητας εφαρμογές του Mobile Azure εφαρμογής υπηρεσίας.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

* [Android εργαλεία για προγραμματιστές](https://developer.android.com/sdk/index.html), η οποία περιλαμβάνει το Android Studio ενσωματωμένο περιβάλλον ανάπτυξης και την πιο πρόσφατη πλατφόρμα Android.
* Azure Mobile Android SDK, το οποίο γίνεται αναφορά αυτόματα ως μέρος του έργου γρήγορη έναρξη κάνετε λήψη.
* Ένας [λογαριασμός Azure active](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Δημιουργήστε έναν νέο υπολογιστή στο παρασκήνιο Azure εφαρμογής για κινητές συσκευές

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Ρύθμιση παραμέτρων του project server

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-android-app"></a>Κάντε λήψη και εκτελέστε την εφαρμογή στο Android

[AZURE.INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
