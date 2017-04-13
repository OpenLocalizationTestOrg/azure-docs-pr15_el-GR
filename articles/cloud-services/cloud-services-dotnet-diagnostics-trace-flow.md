<properties
    pageTitle="Ανίχνευση της ροής σε μια εφαρμογή υπηρεσιών Cloud με το Azure Διαγνωστικά | Microsoft Azure"
    description="Προσθήκη ανίχνευση μηνυμάτων σε μια εφαρμογή του Azure για να βοηθήσει τον εντοπισμό σφαλμάτων, μέτρηση επιδόσεων, παρακολούθηση, ανάλυση της κίνησης και πολλά άλλα."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Ανίχνευση της ροής μιας εφαρμογής των υπηρεσιών Cloud με Διαγνωστικά του Azure

Η ανίχνευση είναι ένας τρόπος για να παρακολουθείτε την εκτέλεση της εφαρμογής σας ενώ εκτελείται. Μπορείτε να χρησιμοποιήσετε τις κατηγορίες [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)και [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) για να καταγράψετε πληροφορίες σχετικά με σφάλματα και εκτέλεση εφαρμογής σε αρχεία καταγραφής, αρχεία κειμένου ή άλλες συσκευές για μελλοντική ανάλυση. Για περισσότερες πληροφορίες σχετικά με την ανίχνευση, ανατρέξτε στο θέμα [Ανίχνευση και Instrumenting εφαρμογές](https://msdn.microsoft.com/library/zs6s4h68.aspx).


## <a name="use-trace-statements-and-trace-switches"></a>Χρήση προτάσεων ανίχνευση και ανίχνευση διακόπτες

Υλοποίηση της ανίχνευσης στην εφαρμογή σας υπηρεσίες Cloud, προσθέτοντας το [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) στη ρύθμιση παραμέτρων της εφαρμογής και να πραγματοποιήσετε κλήσεις System.Diagnostics.Trace ή System.Diagnostics.Debug στον κώδικα της εφαρμογής σας. Χρησιμοποιήστε το αρχείο ρύθμισης παραμέτρων *app.config* για εργαζόμενου ρόλους και τα *web.config* για ρόλους web. Όταν δημιουργείτε μια νέα υπηρεσία χρησιμοποιώντας ένα πρότυπο Visual Studio, Διαγνωστικά Azure προστίθεται αυτόματα στο έργο και το DiagnosticMonitorTraceListener προστίθεται στο αρχείο κατάλληλη ρύθμιση παραμέτρων για τους ρόλους που θέλετε να προσθέσετε.

Για πληροφορίες σχετικά με την τοποθέτηση δηλώσεις ανίχνευση, ανατρέξτε στο θέμα [ΔΙΑΔΙΚΑΣΙΕΣ: προσθήκη ανίχνευση προτάσεων για κώδικα της εφαρμογής](https://msdn.microsoft.com/library/zd83saa2.aspx).

Τοποθετώντας [Διακόπτες ανίχνευση](https://msdn.microsoft.com/library/3at424ac.aspx) στον κώδικά σας, μπορείτε να ελέγξετε εάν η ανίχνευση του πλήκτρου και πώς εκτεταμένη είναι. Αυτό σας επιτρέπει να παρακολουθείτε την κατάσταση της εφαρμογής σας σε ένα περιβάλλον παραγωγής. Αυτό είναι ιδιαίτερα σημαντικό σε μια επαγγελματική εφαρμογή που χρησιμοποιεί πολλά στοιχεία που εκτελούνται σε πολλούς υπολογιστές. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ΔΙΑΔΙΚΑΣΙΕΣ: ρύθμιση παραμέτρων διακόπτες ανίχνευση](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Ρύθμιση παραμέτρων λειτουργίας ακρόασης ανίχνευσης σε μια εφαρμογή του Azure

Ανίχνευση, ο εντοπισμός σφαλμάτων και TraceSource, απαιτούν να ρυθμίσετε "ακροατών" για να συλλέξετε και να καταγράψετε τα μηνύματα που αποστέλλονται. Ακροατών συλλογής, αποθήκευση και δρομολόγηση ανίχνευση μηνυμάτων. Καθοδηγούν τους την έξοδο ανίχνευση σε μια κατάλληλη προορισμού, όπως ένα αρχείο καταγραφής, το παράθυρο ή αρχείο κειμένου. Διαγνωστικά του Azure χρησιμοποιεί την κλάση [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) .

Πριν να ολοκληρώσετε την ακόλουθη διαδικασία, πρέπει να προετοιμάσετε το Azure εποπτεία διαγνωστικών. Για να το κάνετε αυτό, ανατρέξτε στο θέμα [Ενεργοποίηση Διαγνωστικά στο Microsoft Azure](cloud-services-dotnet-diagnostics.md).

Λάβετε υπόψη ότι εάν χρησιμοποιείτε τα πρότυπα που παρέχονται από Visual Studio, τη ρύθμιση παραμέτρων του προγράμματος ακρόασης προστίθεται αυτόματα για εσάς.


### <a name="add-a-trace-listener"></a>Προσθήκη μιας υπηρεσίας ακρόασης ανίχνευσης

1. Ανοίξτε το αρχείο web.config ή app.config για το ρόλο.
2. Προσθέστε τον ακόλουθο κώδικα στο αρχείο. Αλλάξτε το χαρακτηριστικό έκδοση για να χρησιμοποιήσετε τον αριθμό έκδοσης της συγκρότησης κάνετε αναφορά. Η έκδοση συγκρότησης δεν απαραίτητα αλλάζει με κάθε έκδοση Azure SDK, εκτός εάν υπάρχουν ενημερωμένες εκδόσεις σε αυτό.

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Βεβαιωθείτε ότι έχετε επιλέξει μια αναφορά έργου στο συγκρότησης Microsoft.WindowsAzure.Diagnostics. Ενημερώστε τον αριθμό έκδοσης στο παραπάνω xml ώστε να ταιριάζει με την έκδοση της αναφοράς συγκρότησης Microsoft.WindowsAzure.Diagnostics.

3. Αποθηκεύστε το αρχείο ρύθμισης παραμέτρων.

Για περισσότερες πληροφορίες σχετικά με την ακροατών, ανατρέξτε στο θέμα [Ανίχνευση ακροατών](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Αφού ολοκληρώσετε τα βήματα για να προσθέσετε το ακροατήριο, μπορείτε να προσθέσετε ανίχνευση προτάσεις για τον κωδικό.


### <a name="to-add-trace-statement-to-your-code"></a>Για να προσθέσετε δήλωση ανίχνευση σας κώδικα

1. Ανοίξτε ένα αρχείο προέλευσης για την εφαρμογή σας. Για παράδειγμα, το <RoleName>.cs αρχείο για το ρόλο web ή εργαζόμενου ρόλο.
2. Προσθέστε τα ακόλουθα χρησιμοποιώντας δήλωση, εάν δεν έχει προστεθεί ήδη:
    ```
        using System.Diagnostics;
    ```
3. Προσθήκη ανίχνευση δηλώσεις όπου θέλετε να καταγράψετε πληροφορίες σχετικά με την κατάσταση της εφαρμογής σας. Μπορείτε να χρησιμοποιήσετε διάφορες μεθόδους για να μορφοποιήσετε το αποτέλεσμα της πρότασης ανίχνευση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ΔΙΑΔΙΚΑΣΙΕΣ: προσθήκη ανίχνευση προτάσεων για κώδικα εφαρμογής](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Αποθηκεύστε το αρχείο προέλευσης.
