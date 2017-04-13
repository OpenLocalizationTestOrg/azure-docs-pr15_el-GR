<properties
   pageTitle="Εφαρμογές λογικής παραδείγματα και σενάρια | Microsoft Azure"
   description="Παραδείγματα κοινών λογικής εφαρμογές και για να μάθετε πώς μπορείτε να υλοποιήσετε συνηθισμένα σενάρια"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-examples-and-common-scenarios"></a>Παραδείγματα εφαρμογές λογικής και κοινά σενάρια

Αυτό το έγγραφο λεπτομέρειες συνηθισμένα σενάρια και παραδείγματα για να σας βοηθήσει να κατανοήσετε ορισμένους από τους τρόπους που να χρησιμοποιήσετε εφαρμογές λογική για την αυτοματοποίηση επιχειρηματικών διαδικασιών. 

## <a name="custom-triggers-and-actions"></a>Προσαρμοσμένη εναύσματα και ενέργειες

Υπάρχουν πολλοί τρόποι που μπορείτε να ενεργοποιήσετε μια εφαρμογή λογικής από άλλη εφαρμογή. Εδώ θα βρείτε μερικά συνηθισμένα παραδείγματα:

- [Δημιουργία ενός προσαρμοσμένου εναύσματος ή μιας ενέργειας](app-service-logic-create-api-app.md)
- [Μεγάλη διάρκεια εκτέλεσης ενέργειες](app-service-logic-create-api-app.md)
- [Έναυσμα αίτηση HTTP (ΔΗΜΟΣΊΕΥΣΗ)](app-service-logic-http-endpoint.md)
- [Webhook εναύσματα και ενέργειες](app-service-logic-create-api-app.md)
- [Σταθμοσκόπησης εναυσμάτων](app-service-logic-create-api-app.md)

### <a name="scenarios"></a>Σενάρια

- [Σύγχρονη απόκριση αίτησης](app-service-logic-http-endpoint.md)
- [Απόκριση αίτησης με SMS](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-and-logging"></a>Χειρισμός σφαλμάτων και καταγραφής

- [Εξαίρεση και χειρισμού σφαλμάτων](app-service-logic-exception-handling.md)
- [Ρύθμιση παραμέτρων ειδοποιήσεων Azure και τα Διαγνωστικά](app-service-logic-monitor-your-logic-apps.md)

### <a name="scenarios"></a>Σενάρια

- [Περίπτωσης χρήσης: Σφάλμα και χειρισμού εξαιρέσεων](app-service-logic-scenario-error-and-exception-handling.md)

## <a name="deploying-and-managing"></a>Για την ανάπτυξη και τη Διαχείριση

- [Δημιουργία μιας αυτοματοποιημένης ανάπτυξης](app-service-logic-create-deploy-template.md)
- [Δημιουργήστε και αναπτύξτε εφαρμογές λογικής του Visual Studio](app-service-logic-deploy-from-vs.md)
- [Παρακολούθηση λογικής εφαρμογών](app-service-logic-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations"></a>Τύποι περιεχομένου, τις μετατροπές και μετασχηματισμοί

Η λογική εφαρμογές [γλώσσα ορισμού ροής εργασίας](http://aka.ms/logicappsdocs) περιέχει πολλές συναρτήσεις για να σας επιτρέπει να μετατρέψετε και να εργαστείτε με διαφορετικούς τύπους περιεχομένου.  Επιπλέον ο μηχανισμός θα κάνετε όλων μπορεί να διατηρήσετε τύποι περιεχομένου ως ροών δεδομένων μέσω της ροής εργασίας.

- [Χειρισμός των τύπων περιεχομένου](app-service-logic-content-type.md) όπως η εφαρμογή/json, εφαρμογή/xml και απλό/κείμενο
- [Σύνταξη από κοινού ορισμών ροής εργασίας](app-service-logic-author-definitions.md)
- [Αναφορά γλώσσας ορισμού ροής εργασίας](http://aka.ms/logicappsdocs)

## <a name="batches-and-looping"></a>Οι δέσμες και επανάληψη

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Μέχρι την](app-service-logic-loops-and-scopes.md)

## <a name="integrating-with-azure-functions"></a>Ενοποίηση με το Azure συναρτήσεις

- [Ενοποίηση του Azure συναρτήσεις](app-service-logic-azure-functions.md)

### <a name="scenarios"></a>Σενάρια

- [Συνάρτηση Azure ως έναυσμα Bus υπηρεσίας](app-service-logic-scenario-function-sb-trigger.md)

## <a name="http-rest-and-soap"></a>HTTP, ΥΠΌΛΟΙΠΟ και SOAP

 - [Κλήση SOAP](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Θα σας θα συνεχίσετε να προσθέτετε παραδείγματα και σενάρια σε αυτό το έγγραφο. Χρησιμοποιήστε την παρακάτω ενότητα σχολίων για να μας ενημερώσετε ποιες παραδείγματα ή σενάρια που θέλετε να δείτε εδώ.