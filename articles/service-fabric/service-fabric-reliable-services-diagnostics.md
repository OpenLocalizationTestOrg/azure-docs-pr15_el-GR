<properties
   pageTitle="Διαγνωστικά κατάστασης αξιόπιστων υπηρεσιών | Microsoft Azure"
   description="Λειτουργίες διαγνωστικών για τις υπηρεσίες αξιόπιστη βάσει της κατάστασης"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="alanwar"/>

# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Λειτουργίες διαγνωστικών για τις υπηρεσίες αξιόπιστη βάσει της κατάστασης
Τάξη βάσει της κατάστασης αξιόπιστη StatefulServiceBase υπηρεσίες εκπέμπει [Προέλευση_συμβάντος](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) συμβάντα που μπορεί να χρησιμοποιηθεί για εντοπισμός σφαλμάτων της υπηρεσίας, δώστε τις ιδέες στο πώς λειτουργεί το περιβάλλον εκτέλεσης και Βοήθεια με την αντιμετώπιση προβλημάτων.

## <a name="eventsource-events"></a>Προέλευση_συμβάντος συμβάντα
Το όνομα του Προέλευση_συμβάντος για την τάξη βάσει της κατάστασης αξιόπιστη StatefulServiceBase υπηρεσίες είναι "-ServiceFabric-υπηρεσίες της Microsoft". Συμβάντα από αυτήν την προέλευση συμβάντος εμφανίζονται στο παράθυρο [Συμβάντα διαγνωστικού ελέγχου](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) όταν η υπηρεσία [διορθώνεται στο Visual Studio](service-fabric-debugging-your-application.md).

Παραδείγματα εργαλεία και τις τεχνολογίες που σας βοηθούν στη συλλογή ή/και Προβολή συμβάντων Προέλευση_συμβάντος είναι [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Τα Διαγνωστικά του Microsoft Azure](../cloud-services/cloud-services-dotnet-diagnostics.md)και στη [Βιβλιοθήκη Microsoft TraceEvent](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Συμβάντα

|Όνομα συμβάντος|Αναγνωριστικό συμβάντος|Επίπεδο|Περιγραφή του συμβάντος|
|----------|--------|-----|-----------------|
|StatefulRunAsyncInvocation|1|Ενημερωτικό|Που εκπέμπει κατά την εκκίνηση υπηρεσίας RunAsync εργασίας|
|StatefulRunAsyncCancellation|2|Ενημερωτικό|Που εκπέμπει όταν ακυρώθηκε υπηρεσία RunAsync εργασιών|
|StatefulRunAsyncCompletion|3|Ενημερωτικό|Που εκπέμπει όταν ολοκληρωθεί η εργασία RunAsync υπηρεσίας|
|StatefulRunAsyncSlowCancellation|4|Προειδοποίηση|Που εκπέμπει κατά την υπηρεσία RunAsync εργασίας διαρκεί πολύ χρόνο για να ολοκληρώσετε ακύρωσης|
|StatefulRunAsyncFailure|5|Σφάλμα|Που εκπέμπει κατά την υπηρεσία RunAsync εργασίας δημιουργεί μια εξαίρεση|

## <a name="interpret-events"></a>Ερμηνεία συμβάντα

Τα συμβάντα StatefulRunAsyncInvocation, StatefulRunAsyncCompletion και StatefulRunAsyncCancellation είναι χρήσιμο να εγγραφής της υπηρεσίας για να κατανοήσετε τον κύκλο ζωής των υπηρεσίας--, καθώς και την ώρα κατά αποτελέσματα, ακυρώθηκε ή ολοκληρώσει μια υπηρεσία. Αυτό μπορεί να είναι χρήσιμες κατά τον εντοπισμό σφαλμάτων ζητήματα υπηρεσίας ή Κατανόηση του κύκλου ζωής της υπηρεσίας.

Υπηρεσία συντάκτες, οι οποίοι θα πρέπει να παρακολουθείτε στενά συμβάντα StatefulRunAsyncSlowCancellation και StatefulRunAsyncFailure επειδή υποδεικνύουν προβλήματα με την υπηρεσία.

StatefulRunAsyncFailure αποστέλλεται κάθε φορά που η εργασία RunAsync() υπηρεσία δημιουργεί μια εξαίρεση. Συνήθως, μια εξαίρεση υποδεικνύει ένα σφάλμα ή σφάλμα στην υπηρεσία. Επιπλέον, η εξαίρεση έχει ως αποτέλεσμα την υπηρεσία αποτυχία, ώστε να μετακινηθεί σε διαφορετικό κόμβο. Αυτό μπορεί να είναι μια λειτουργία καταναλώνοντας και μπορεί να καθυστερήσει εισερχόμενες αιτήσεις ενώ μετακινείται στην υπηρεσία. Υπηρεσία συντάκτες θα πρέπει να προσδιορίσετε την αιτία της εξαίρεσης και, εάν είναι δυνατόν, μετριασμός του.

StatefulRunAsyncSlowCancellation αποστέλλεται κάθε φορά που μια αίτηση ακύρωσης για την εργασία RunAsync διαρκεί περισσότερο από τέσσερα δευτερόλεπτα. Όταν μια υπηρεσία διαρκεί πολύ χρόνο για να ολοκληρώσετε ακύρωσης, επηρεάζει τη δυνατότητα για την υπηρεσία για να γίνει επανεκκίνηση γρήγορα σε έναν άλλο κόμβο. Αυτό μπορεί να επηρεάσει τη συνολική διαθεσιμότητα της υπηρεσίας.
