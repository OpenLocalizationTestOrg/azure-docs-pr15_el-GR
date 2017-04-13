<properties
    pageTitle="Επισκόπηση του Azure Διαγνωστικά | Microsoft Azure"
    description="Χρησιμοποιήστε Διαγνωστικά του Azure για τον εντοπισμό σφαλμάτων, μέτρηση απόδοσης, παρακολούθηση, η ανάλυση της κίνησης στο cloud services, εικονικές μηχανές και ύφασμα υπηρεσίας"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Τι είναι τα Διαγνωστικά του Microsoft Azure


Διαγνωστικά του Azure είναι η δυνατότητα μέσα σε Azure που επιτρέπει τη συλλογή διαγνωστικών δεδομένων σε μια εφαρμογή ανεπτυγμένος. Μπορείτε να χρησιμοποιήσετε την επέκταση Διαγνωστικά από πολλές διαφορετικές προελεύσεις. Υποστηρίζονται αυτήν τη στιγμή είναι Azure Cloud υπηρεσίας Web και τους ρόλους εργαζόμενου, εικονικές μηχανές Windows Azure εκτελεί Microsoft Windows και ύφασμα υπηρεσίας. Άλλες υπηρεσίες του Azure έχουν τις δικές τους ξεχωριστή Διαγνωστικά.

## <a name="data-you-can-collect"></a>Μπορείτε να συλλέξετε δεδομένων

Διαγνωστικά του Azure μπορεί να συλλέξει τους ακόλουθους τύπους δεδομένων:

Αρχείο προέλευσης δεδομένων|Περιγραφή
---|---
Μετρητές επιδόσεων | Λειτουργικό σύστημα και το προσαρμοσμένο μετρητών
Αρχεία καταγραφής εφαρμογών     | Ανίχνευση μηνύματα που έχουν συνταχθεί από την εφαρμογή σας
Αρχεία καταγραφής συμβάντων των Windows   | Πληροφορίες που αποστέλλονται στο σύστημα καταγραφής συμβάντων των Windows
Προέλευση συμβάντος .NET    | Κωδικός γράφετε συμβάντα χρησιμοποιώντας την κλάση .NET [Προέλευση_συμβάντος](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx)
Αρχεία καταγραφής των υπηρεσιών IIS             | Πληροφορίες σχετικά με τις τοποθεσίες web των υπηρεσιών IIS
Δήλωση βάσει ETW   | Συμβάν ανίχνευσης για Windows συμβάντα που δημιουργούνται από κάθε διεργασίας
Ενδείξεις σφαλμάτων          | Πληροφορίες σχετικά με την κατάσταση της διαδικασίας σε περίπτωση ένα σφάλμα εφαρμογής
Αρχεία καταγραφής προσαρμοσμένου σφάλματος    | Αρχεία καταγραφής που δημιουργήθηκαν από την εφαρμογή ή υπηρεσία
Αρχεία καταγραφής διαγνωστικών υποδομή του Azure|Πληροφορίες σχετικά με τα Διαγνωστικά του εαυτού

Την επέκταση Azure Διαγνωστικά μπορεί να μεταφέρετε τα δεδομένα σε ένα λογαριασμό Azure χώρο αποθήκευσης ή να το στείλετε σε υπηρεσίες όπως [Ιδέες εφαρμογής](./application-insights/app-insights-cloudservices.md). Μπορείτε να χρησιμοποιήσετε τα δεδομένα για τον εντοπισμό σφαλμάτων και αντιμετώπιση προβλημάτων, μέτρηση απόδοσης, παρακολούθηση χρήσης πόρων, ανάλυση της κίνησης και χωρητικότητα σχεδιασμού και έλεγχος.


## <a name="versioning"></a>Διαχείριση εκδόσεων
Ανατρέξτε στο θέμα [Azure Διαγνωστικά ιστορικό εκδόσεων](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Επόμενα βήματα
Επιλέξτε ποια υπηρεσία που προσπαθείτε να συλλέξετε Διαγνωστικά στην και χρησιμοποιήστε τα ακόλουθα άρθρα για να ξεκινήσετε. Χρησιμοποιήστε τις συνδέσεις γενικά Azure Διαγνωστικά για αναφορά για συγκεκριμένες εργασίες.

## <a name="web-apps"></a>Εφαρμογές Web
Σημειώστε ότι οι εφαρμογές Web δεν χρησιμοποιούν Διαγνωστικά Azure. Εύρεση ισοδύναμες πληροφορίες στο [Web Apps](./app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Υπηρεσίες cloud χρησιμοποιώντας τα Διαγνωστικά του Azure
- Εάν χρησιμοποιείτε το Visual Studio, ανατρέξτε στο θέμα [Χρήση Visual Studio για την παρακολούθηση μιας εφαρμογής των υπηρεσιών Cloud](./vs-azure-tools-debug-cloud-services-virtual-machines.md) για να ξεκινήσετε. Διαφορετικά, ανατρέξτε στο θέμα
- [Πώς μπορείτε να παρακολουθείτε τις υπηρεσίες Cloud χρησιμοποιώντας τα Διαγνωστικά του Azure](./cloud-services/cloud-services-how-to-monitor.md)
- [Ρύθμιση του Azure Διαγνωστικά σε μια εφαρμογή υπηρεσιών Cloud](./cloud-services/cloud-services-dotnet-diagnostics.md)

Για πιο σύνθετες θέματα, ανατρέξτε στο θέμα

- [Χρησιμοποιώντας τα Διαγνωστικά του Azure με εφαρμογή ιδέες για τις υπηρεσίες Cloud](./application-insights/app-insights-cloudservices.md)
- [Ανίχνευση της ροής μιας εφαρμογής των υπηρεσιών Cloud με Διαγνωστικά του Azure](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [Χρήση του PowerShell για να ρυθμίσετε Διαγνωστικά σχετικά με τις υπηρεσίες Cloud](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>Εικονικές μηχανές χρησιμοποιώντας τα Διαγνωστικά του Azure
- Εάν χρησιμοποιείτε το Visual Studio, ανατρέξτε στο θέμα [Χρήση Visual Studio για ανίχνευση εικονικές μηχανές Windows Azure](./vs-azure-tools-debug-cloud-services-virtual-machines.md) για να ξεκινήσετε. Διαφορετικά, ανατρέξτε στο θέμα
- [Ρύθμιση του Azure Διαγνωστικά σε μια Azure εικονική μηχανή](./virtual-machines-dotnet-diagnostics.md)

Για πιο σύνθετες θέματα, ανατρέξτε στο θέμα

- [Χρήση του PowerShell για να ρυθμίσετε Διαγνωστικά σε εικονικές μηχανές Windows Azure](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [Δημιουργήστε έναν υπολογιστή Windows εικονικού παρακολούθηση και διαγνωστικά χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>Υπηρεσία ύφασμα χρησιμοποιώντας τα Διαγνωστικά του Azure
Γρήγορα αποτελέσματα με την [Παρακολούθηση μιας εφαρμογής υπηρεσίας ύφασμα](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Πολλά άλλα άρθρα Διαγνωστικά ύφασμα υπηρεσία είναι διαθέσιμες στο δέντρο περιήγησης στην αριστερή πλευρά, αφού μπορείτε να μεταβείτε σε αυτό το άρθρο.

## <a name="general-azure-diagnostics-articles"></a>Διαγνωστικά Azure γενικά άρθρα
- [Ρύθμιση παραμέτρων σχήματος Διαγνωστικά azure](https://msdn.microsoft.com/library/azure/mt634524.aspx) - μάθετε πώς μπορείτε να αλλάξετε το αρχείο σχήματος για να συγκεντρώσετε και να δρομολογήσετε Διαγνωστικά δεδομένων. Σημειώστε ότι μπορείτε επίσης να χρησιμοποιήσετε Visual Studio για να αλλάξετε το αρχείο σχήματος.
- [Πώς τα Διαγνωστικά Azure τα δεδομένα αποθηκεύονται στο χώρο αποθήκευσης Azure](./cloud-services/cloud-services-dotnet-diagnostics-storage.md) - γνωρίζετε τα ονόματα των πινάκων και των αντικειμένων blob όπου είναι γραμμένο τα διαγνωστικά δεδομένα.
- Μάθετε πώς να [χρησιμοποιείτε μετρητών επιδόσεων στο Azure Διαγνωστικά](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
- Μάθετε πώς να [Azure δρομολόγηση πληροφορίες διαγνωστικών για να ιδέες εφαρμογής](./azure-diagnostics-configure-applicationinsights.md)
- Εάν έχετε προβλήματα με Διαγνωστικά ξεκινώντας ή την εύρεση των δεδομένων σας στο χώρο αποθήκευσης Azure πινάκων, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων Διαγνωστικά Azure](./azure-diagnostics-troubleshooting.md)
