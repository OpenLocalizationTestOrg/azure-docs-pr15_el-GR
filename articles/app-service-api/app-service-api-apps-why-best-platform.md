<properties 
    pageTitle="Εισαγωγή εφαρμογές API | Microsoft Azure" 
    description="Μάθετε τον τρόπο εφαρμογής υπηρεσίας Azure σάς βοηθά να αναπτύσσουν, κεντρικός υπολογιστής, και κατανάλωση RESTful APIs." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>Επισκόπηση API εφαρμογές

API εφαρμογές στο Azure εφαρμογής υπηρεσίας προσφέρουν δυνατότητες που σας διευκολύνουν να αναπτύσσουν, κεντρικός υπολογιστής, και κατανάλωση APIs στο cloud και εσωτερικής εγκατάστασης. Με τις εφαρμογές API λαμβάνετε ασφαλείας βαθμολογίας για μεγάλες επιχειρήσεις, έλεγχο πρόσβασης σε απλό, υβριδική συνδεσιμότητας, αυτόματη δημιουργία SDK και απρόσκοπτη ενοποίηση με [Λογική εφαρμογών](../app-service-logic/app-service-logic-what-are-logic-apps.md).

[Azure εφαρμογής υπηρεσίας](../app-service/app-service-value-prop-what-is.md) είναι μια πλήρως διαχειριζόμενων πλατφόρμα για κινητές συσκευές, web και ενοποίηση σενάρια. API εφαρμογές είναι μόνο ένας από τους τέσσερις τύπους εφαρμογών που παρέχεται από [Azure εφαρμογής υπηρεσίας](../app-service/app-service-value-prop-what-is.md).

![Εφαρμογή τύπων στο Azure εφαρμογής υπηρεσίας](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Γιατί να χρησιμοποιήσετε API εφαρμογές;

Εδώ θα βρείτε ορισμένες βασικές δυνατότητες του API εφαρμογές:

- **Μεταφέρετε τις υπάρχουσες API ως-είναι** -δεν χρειάζεται να αλλάξετε οποιαδήποτε από τον κώδικα στο τις υπάρχουσες API για να επωφεληθείτε από τις εφαρμογές API--απλώς να αναπτύξετε τον κωδικό σε μια εφαρμογή API. Το API να χρησιμοποιήσετε οποιαδήποτε γλώσσα ή framework υποστηρίζεται από την υπηρεσία εφαρμογής, συμπεριλαμβανομένων των ASP.NET και C#, Java, PHP, Node.js και Python.

- **Εύκολη κατανάλωση** - ολοκληρωμένη υποστήριξη για [μετα-δεδομένα Swagger API](http://swagger.io/) κάνει σας APIs εύκολα ΑΝΑΛΩΣΙΜΩΝ από μια ποικιλία προγράμματα-πελάτες.  Αυτόματη δημιουργία κώδικα προγράμματος-πελάτη για το API σε διάφορες γλώσσες όπως C#, Java και Javascript. Να ρυθμίσετε εύκολα τις [CORS](app-service-api-cors-consume-javascript.md) χωρίς να αλλάξετε τον κωδικό. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εφαρμογών υπηρεσίας API μετα-δεδομένων για την παραγωγή εντοπισμού και κώδικα API](app-service-api-metadata.md) και [ανάλωση εφαρμογής API από JavaScript χρησιμοποιώντας CORS](app-service-api-cors-consume-javascript.md). 

- **Έλεγχος πρόσβασης απλό** - προστασία εφαρμογής API από την access χωρίς έλεγχο ταυτότητας χωρίς αλλαγές για να τον κωδικό. Υπηρεσίες ενσωματωμένο ελέγχου ταυτότητας ασφαλή APIs για πρόσβαση με άλλες υπηρεσίες ή με προγράμματα-πελάτες που αντιπροσωπεύει τους χρήστες. Υπηρεσίες παροχής ταυτότητας υποστηριζόμενες περιλαμβάνουν Azure Active Directory, Facebook, Twitter, Google και λογαριασμό Microsoft που διαθέτετε. Προγράμματα-πελάτες μπορούν να χρησιμοποιούν βιβλιοθήκη ελέγχου ταυτότητας Active Directory (ADAL) ή το SDK εφαρμογές του Mobile. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Έλεγχος ταυτότητας και εξουσιοδότησης για τις εφαρμογές API στο Azure εφαρμογής υπηρεσίας](app-service-api-authentication.md).

- **Ενοποίηση του visual Studio** - αποκλειστικός εργαλεία στο Visual Studio βελτιστοποιήσετε την εργασία με τη δημιουργία, την ανάπτυξη, κατανάλωση, τον εντοπισμό σφαλμάτων και τη Διαχείριση εφαρμογών API. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ανακοίνωση του SDK Azure 2.8.1 για .NET](/blog/announcing-azure-sdk-2-8-1-for-net/).

- **Ενοποίηση με τις εφαρμογές λογικής** - API εφαρμογές που δημιουργείτε μπορεί να χρησιμοποιηθεί από [Λογική εφαρμογών υπηρεσίας](../app-service-logic/app-service-logic-what-are-logic-apps.md).  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [χρήση το προσαρμοσμένο API φιλοξενούνται σε εφαρμογής υπηρεσίας με λογική εφαρμογές](../app-service-logic/app-service-logic-custom-hosted-api.md) και [νέο σχήμα έκδοση 2015 08-01-προεπισκόπηση](../app-service-logic/app-service-logic-schema-2015-08-01.md).

Επιπλέον, μια εφαρμογή API να επωφεληθείτε από δυνατότητες που παρέχονται από [Εφαρμογές Web](../app-service-web/app-service-web-overview.md) και [Εφαρμογές του Mobile](../app-service-mobile/app-service-mobile-value-prop.md). Ισχύει επίσης το αντίστροφο: Εάν χρησιμοποιείτε μια εφαρμογή web ή εφαρμογή για κινητές συσκευές για τη φιλοξενία του API, αυτό να επωφεληθείτε από εφαρμογές API δυνατότητες όπως Swagger μετα-δεδομένων για το πρόγραμμα-πελάτη δημιουργία κώδικα και CORS για πρόσβαση σε πρόγραμμα περιήγησης μεταξύ τομέων. Η μόνη διαφορά μεταξύ των τύπων τρεις εφαρμογής (API, web, κινητές συσκευές) είναι το όνομα και το εικονίδιο που χρησιμοποιούνται για αυτές στην πύλη του Azure.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Ποια είναι η διαφορά μεταξύ των εφαρμογών API και Azure API διαχείρισης;

API εφαρμογές και [Azure API διαχείρισης](../api-management/api-management-key-concepts.md) είναι συμπληρωματική υπηρεσίες:

* API διαχείρισης είναι σχετικά με τη Διαχείριση APIs. Τοποθέτηση μιας προσκηνίου API διαχείρισης στο API χρήση οθόνης και επιτάχυνσης, χειριστείτε εισόδου και εξόδου, συνάθροιση διάφορες APIs σε ένα τελικό σημείο και ούτω καθεξής. Τα API διαχείρισης μπορούν να φιλοξενηθούν σε οποιοδήποτε σημείο.
* API εφαρμογές είναι πληροφορίες για τη φιλοξενία APIs. Η υπηρεσία περιλαμβάνει δυνατότητες που διευκολύνουν την ανάπτυξη και καταναλώνει API, αλλά δεν μπορεί να κάνει τα είδη των παρακολούθησης, περιορισμού, χειρισμός ή η ενοποίηση που API διαχείρισης. Εάν δεν χρειάζεστε δυνατότητες API διαχείρισης, μπορείτε να φιλοξενήσετε APIs στις εφαρμογές API χωρίς τη χρήση API διαχείρισης.

Παρακάτω θα δείτε ένα διάγραμμα που απεικονίζει API διαχείρισης που χρησιμοποιούνται για API φιλοξενείται σε εφαρμογές API και σε κάποιο άλλο σημείο.

![Azure API διαχείρισης και εφαρμογές API](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Ορισμένες δυνατότητες της διαχείρισης API και εφαρμογές API έχουν παρόμοιες συναρτήσεις.  Για παράδειγμα, και τα δύο να αυτοματοποιήσετε CORS υποστήριξης. Όταν χρησιμοποιείτε τις υπηρεσίες δύο μαζί, μπορείτε να χρησιμοποιήσετε API διαχείρισης για CORS δεδομένου ότι λειτουργεί ως προσκήνιο στις εφαρμογές API. 

## <a name="getting-started"></a>Γρήγορα αποτελέσματα

Για να ξεκινήσετε με τις εφαρμογές API μέσω ανάπτυξης δείγμα κώδικα σε μία, δείτε το πρόγραμμα εκμάθησης για οποιαδήποτε framework που προτιμάτε:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Για να υποβάλετε ερωτήσεις σχετικά με τις εφαρμογές API, ξεκινήστε ένα νήμα στο [φόρουμ API εφαρμογές](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 