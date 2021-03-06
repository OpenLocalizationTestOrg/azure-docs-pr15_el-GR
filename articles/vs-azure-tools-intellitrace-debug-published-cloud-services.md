<properties 
   pageTitle="Τον εντοπισμό σφαλμάτων σε μια υπηρεσία cloud δημοσιευμένο με IntelliTrace και του Visual Studio | Microsoft Azure"
   description="Τον εντοπισμό σφαλμάτων σε μια υπηρεσία cloud δημοσιευμένο με IntelliTrace και του Visual Studio"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-a-published-cloud-service-with-intellitrace-and-visual-studio"></a>Τον εντοπισμό σφαλμάτων σε μια υπηρεσία cloud δημοσιευμένο με IntelliTrace και του Visual Studio

##<a name="overview"></a>Επισκόπηση

Με IntelliTrace, μπορείτε να καταγράψετε εκτεταμένη πληροφορίες εντοπισμού σφαλμάτων για μια παρουσία ρόλο όταν εκτελείται στο Azure. Εάν χρειάζεστε για να βρείτε την αιτία του προβλήματος, μπορείτε να χρησιμοποιήσετε τα αρχεία καταγραφής IntelliTrace για να περιηγηθείτε στις τον κωδικό από το Visual Studio, όπως εάν εκτελείται στο Azure. Στην πραγματικότητα IntelliTrace εγγραφές βασικές εκτέλεση κώδικα και περιβάλλον δεδομένων κατά την εφαρμογή του Azure εκτελείται ως υπηρεσία στο cloud στο Azure και σας επιτρέπει να αναπαραγάγετε τα στοιχεία που έχουν καταγραφεί από το Visual Studio. Ως εναλλακτική λύση, μπορείτε να χρησιμοποιήσετε το απομακρυσμένο εντοπισμό σφαλμάτων για να επισυνάψετε απευθείας σε μια υπηρεσία cloud που εκτελείται στο Azure. Ανατρέξτε στο θέμα [Αντιμετώπιση σφαλμάτων υπηρεσίες Cloud](http://go.microsoft.com/fwlink/p/?LinkId=623041).

>[AZURE.IMPORTANT] IntelliTrace προορίζεται για σενάρια εντοπισμού σφαλμάτων μόνο και δεν πρέπει να χρησιμοποιούνται για μια ανάπτυξη παραγωγής.

>[AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε IntelliTrace εάν έχετε Visual Studio Enterprise εγκατεστημένο και το Azure εφαρμογής των προορισμών .NET Framework 4 ή νεότερη έκδοση. IntelliTrace συλλέγει πληροφορίες για ρόλους Azure σας. Τις εικονικές μηχανές για αυτούς τους ρόλους να εκτελείτε πάντα λειτουργικά συστήματα 64 bit.

## <a name="to-configure-an-azure-application-for-intellitrace"></a>Για να ρυθμίσετε μια εφαρμογή του Azure για IntelliTrace

Για να ενεργοποιήσετε IntelliTrace για μια εφαρμογή του Azure, πρέπει να δημιουργήσετε και να δημοσιεύσετε την εφαρμογή από ένα έργο Visual Studio Azure. Πρέπει να ρυθμίσετε IntelliTrace για την εφαρμογή Azure σας πριν να τη δημοσιεύσετε στο Azure. Εάν δημοσιεύσετε την εφαρμογή σας χωρίς ρύθμιση παραμέτρων IntelliTrace, αλλά, στη συνέχεια, αποφασίσετε ότι θέλετε να κάνετε αυτό, θα πρέπει να δημοσιεύσετε την εφαρμογή ξανά από το Visual Studio. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [δημοσίευση μια υπηρεσία στο Cloud χρησιμοποιώντας τα εργαλεία Azure](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Όταν είστε έτοιμοι να αναπτύξετε την εφαρμογή του Azure, βεβαιωθείτε ότι το προορισμών Δόμηση έργου έχουν ρυθμιστεί για τον **εντοπισμό σφαλμάτων**.

1. Άνοιγμα του μενού συντόμευσης για το έργο Azure στην Εξερεύνηση λύσεων και επιλέξτε **Δημοσίευση**.
 
    Εμφανίζεται ο οδηγός δημοσίευση εφαρμογής Azure.

1. Για να συλλέξετε IntelliTrace αρχεία καταγραφής για την εφαρμογή σας όταν δημοσιευτεί στο cloud, επιλέξτε το πλαίσιο ελέγχου **Ενεργοποίηση IntelliTrace** .

    >[AZURE.NOTE] Μπορείτε να ενεργοποιήσετε IntelliTrace ή δημιουργία προφίλ όταν δημοσιεύετε Azure εφαρμογή σας. Δεν μπορείτε να ενεργοποιήσετε και τα δύο.

1. Για να προσαρμόσετε τις βασικές παραμέτρους IntelliTrace, επιλέξτε την υπερ-σύνδεση **Ρυθμίσεις** .

    Εμφανίζεται το παράθυρο διαλόγου Ρυθμίσεις IntelliTrace, όπως φαίνεται στην παρακάτω εικόνα. Μπορείτε να καθορίσετε ποια συμβάντα στο αρχείο καταγραφής, εάν θέλετε να συλλέξετε πληροφορίες κλήσης, λειτουργικές μονάδες και τις διαδικασίες για τη συλλογή των αρχείων καταγραφής για και πόσο χώρο για να δεσμεύσετε για την εγγραφή. Για περισσότερες πληροφορίες σχετικά με το IntelliTrace, ανατρέξτε στο θέμα [Εντοπισμός σφαλμάτων με IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

Το αρχείο καταγραφής IntelliTrace είναι ένα αρχείο καταγραφής κυκλικής του μέγιστου μεγέθους που καθορίζεται στις ρυθμίσεις IntelliTrace (το προεπιλεγμένο μέγεθος είναι 250 MB). Σε ένα αρχείο στο σύστημα αρχείων των εικονική μηχανή της συλλογής των αρχείων καταγραφής IntelliTrace. Όταν κάνετε αίτηση για τα αρχεία καταγραφής, ένα στιγμιότυπο είναι που λαμβάνονται σε αυτό το σημείο στο χρόνο και λάβατε στον τοπικό υπολογιστή σας.

Μετά την εφαρμογή του Azure έχει δημοσιευθεί σε Azure, μπορείτε να καθορίσετε τη εάν έχει ενεργοποιηθεί η IntelliTrace από τον κόμβο τον υπολογισμό Azure στην Εξερεύνηση διακομιστή, όπως φαίνεται στην παρακάτω εικόνα:

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="downloading-intellitrace-logs-for-a-role-instance"></a>Λήψη αρχείων καταγραφής IntelliTrace για μια παρουσία ρόλων

Μπορείτε να κάνετε λήψη αρχείων καταγραφής IntelliTrace για μια παρουσία ρόλο από τον κόμβο **Υπηρεσίες Cloud** στην **Εξερεύνηση Server**. Αναπτύξτε τον κόμβο **Υπηρεσίες Cloud** μέχρι να εντοπίσετε την παρουσία που σας ενδιαφέρει, ανοίξτε το μενού συντόμευσης για αυτήν την παρουσία και επιλέξτε **Προβολή αρχείων καταγραφής IntelliTrace**. Γίνεται λήψη των αρχείων καταγραφής IntelliTrace σε ένα αρχείο σε έναν κατάλογο στον τοπικό σας υπολογιστή. Κάθε φορά που ζητάτε την IntelliTrace που συνδέεται, δημιουργείται ένα νέο στιγμιότυπο.

Όταν γίνεται λήψη των αρχείων καταγραφής, Visual Studio εμφανίζει την πρόοδο της λειτουργίας στο παράθυρο του αρχείου καταγραφής δραστηριότητας Azure. Όπως φαίνεται στην παρακάτω εικόνα, μπορείτε να αναπτύξετε το στοιχείο γραμμής για τη λειτουργία για να δείτε περισσότερες λεπτομέρειες.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Μπορείτε να συνεχίσετε να εργάζεστε στο Visual Studio, ενώ γίνεται λήψη των αρχείων καταγραφής IntelliTrace. Όταν το αρχείο καταγραφής έχει ολοκληρωθεί η λήψη, θα ανοίξει αυτόματα στο Visual Studio.

>[AZURE.NOTE] Τα αρχεία καταγραφής IntelliTrace ενδέχεται να περιέχουν εξαιρέσεις που πλαίσιο δημιουργεί και στη συνέχεια χειρίζεται. Κωδικός εσωτερικού framework δημιουργεί τις εξής εξαιρέσεις ως κανονική τμήμα έναρξης του ρόλου, ώστε να μπορείτε να αγνοήσετε τους.

## <a name="see-also"></a>Δείτε επίσης

[Υπηρεσίες Cloud εντοπισμού σφαλμάτων](https://msdn.microsoft.com/library/ee405479.aspx)

