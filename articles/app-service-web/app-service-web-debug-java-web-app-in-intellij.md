<properties 
    pageTitle="Εντοπισμός σφαλμάτων σε μια εφαρμογή Web Java στην Azure στο IntelliJ | Microsoft Azure" 
    description="Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε το Κιτ εργαλείων Azure για IntelliJ για τον εντοπισμό σφαλμάτων σε μια εφαρμογή Web Java εκτελείται σε Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>Εντοπισμός σφαλμάτων σε μια εφαρμογή Web Java στην Azure στο IntelliJ

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να εντοπίσετε σφάλματα σε μια εφαρμογή Web Java εκτελείται σε Azure χρησιμοποιώντας το [Κιτ εργαλείων Azure για IntelliJ]. Λόγους απλούστευσης, θα μπορείτε να χρησιμοποιήσετε ένα βασικό παράδειγμα σελίδας διακομιστή Java (JSP) για αυτό το πρόγραμμα εκμάθησης, αλλά τα βήματα θα είναι παρόμοια για ένα servlet Java όταν εκτελείτε εντοπισμού σφαλμάτων στο Azure.

Όταν ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, η εφαρμογή σας θα είναι παρόμοιο με ακολουθεί όταν που εκτελούν εντοπισμό σφαλμάτων σε IntelliJ:

![][01]
 
## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Μια Java προγραμματιστής Kit (JDK), v 1,8 ή νεότερη έκδοση.
* IntelliJ ΙΔΈΑ Ultimate Edition. Αυτό μπορεί να ληφθεί από <https://www.jetbrains.com/idea/download/index.html>.
* Μια κατανομή διακομιστή web που βασίζονται σε Java ή διακομιστή εφαρμογών, όπως Apache Tomcat ή Jetty.
* Azure συνδρομή, που μπορεί να αποκτήσει από <https://azure.microsoft.com/en-us/free/> ή <http://azure.microsoft.com/pricing/purchase-options/>.
* Το Azure Κιτ εργαλείων για IntelliJ. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κατά την εγκατάσταση του Κιτ εργαλείων Azure για IntelliJ].
* Δυναμική έργου Web δημιουργούνται και αναπτυχθεί σε Azure εφαρμογής υπηρεσίας. Για παράδειγμα ανατρέξτε στο θέμα [Δημιουργία μιας Hello World Web App για το Azure στο IntelliJ].

## <a name="to-debug-a-java-web-app-on-azure"></a>Για τον εντοπισμό σφαλμάτων σε μια εφαρμογή Web Java στην Azure

Για να ολοκληρώσετε αυτά τα βήματα σε αυτήν την ενότητα, μπορείτε να χρησιμοποιήσετε ένα υπάρχον έργο Web δυναμικής που έχετε ήδη αναπτύξει ως μια εφαρμογή Web Java στην Azure, κάνετε λήψη ενός [Δείγματος δυναμικής Project Web] και ακολουθήστε τα βήματα στο θέμα [Δημιουργία μιας Hello World Web App για το Azure στο IntelliJ] για την ανάπτυξη του σε Azure. 

1. Ανοίξτε το έργο Java Web App που θα αναπτυχθεί σε Azure στο IntelliJ.

1. Κάντε κλικ στο μενού **Εκτέλεση** και, στη συνέχεια, κάντε κλικ στην επιλογή **Επεξεργασία ρυθμίσεις παραμέτρων...**.

    ![][02]

1. Όταν ανοίγει το παράθυρο διαλόγου **Ρυθμίσεις παραμέτρων εκτέλεση/εντοπισμού σφαλμάτων** : 

    1. Επιλέξτε **Azure Web App**.
    1. Κάντε κλικ στην εντολή **+** για να προσθέσετε μια νέα ρύθμιση παραμέτρων.
    1. Δώστε ένα **όνομα** για τη ρύθμιση παραμέτρων.
    1. Αποδεχτείτε τις υπόλοιπες προεπιλεγμένες τιμές που έχουν προτείνονται από το Azure Κιτ εργαλείων και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

        ![][03]

1. Επιλέξτε τη ρύθμιση παραμέτρων εντοπισμού σφαλμάτων Azure Web App που μόλις δημιουργήσατε στο επάνω δεξιό μέρος του μενού και κάντε κλικ στο **εντοπισμού σφαλμάτων**

    ![][04]

1. Όταν σας ζητηθεί να **Ενεργοποίηση απομακρυσμένου εντοπισμού σφαλμάτων στην εφαρμογή Web remote τώρα;**, κάντε κλικ στο κουμπί **OK**.

1. Όταν σας ζητηθεί αυτήν **την εφαρμογή web είναι τώρα έτοιμο για απομακρυσμένο εντοπισμό σφαλμάτων**, κάντε κλικ στο **κουμπί OK**.

    ![][05]

1. Επιλέξτε τη ρύθμιση παραμέτρων εντοπισμού σφαλμάτων Azure Web App που μόλις δημιουργήσατε στο επάνω δεξιό μέρος του μενού και, στη συνέχεια, κάντε κλικ στην εντολή **Εντοπισμός σφαλμάτων**.

1. Μια γραμμή εντολών των Windows ή κελύφους Unix θα ανοίξουν και προετοιμασία απαραίτητες σύνδεσης για τον εντοπισμό σφαλμάτων; πρέπει να περιμένετε έως ότου η σύνδεση σε απομακρυσμένη εφαρμογή Java Web είναι επιτυχής, πριν να συνεχίσετε. Εάν χρησιμοποιείτε Windows, θα μοιάζει με την ακόλουθη εικόνα:

    ![][06]

1. Εισαγάγετε ένα σημείο διακοπής στη σελίδα σας JSP και, στη συνέχεια, ανοίξτε τη διεύθυνση URL για την εφαρμογή Web της Java σε ένα πρόγραμμα περιήγησης:

    1. Άνοιγμα του **Azure Explorer** στο IntelliJ.
    1. Περιήγηση στο **Web Apps** και την εφαρμογή Web Java που θέλετε για τον εντοπισμό σφαλμάτων.
    1. Κάντε δεξί κλικ στην εφαρμογή Web και κάντε κλικ στην επιλογή **Άνοιγμα σε πρόγραμμα περιήγησης**.
    1. Τώρα θα εισαγάγετε IntelliJ σε κατάσταση εντοπισμού σφαλμάτων.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με τη χρήση Azure με Java, ανατρέξτε στο [Κέντρο για προγραμματιστές του Azure Java].

Για πρόσθετες πληροφορίες σχετικά με τη δημιουργία εφαρμογών Web Azure, ανατρέξτε στο θέμα η [Επισκόπηση εφαρμογές Web].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure Κιτ εργαλείων για IntelliJ]: ../azure-toolkit-for-intellij.md
[Κατά την εγκατάσταση του Κιτ εργαλείων Azure για IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Δημιουργία εφαρμογής Web διεθνείς Hello για Azure στο IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Δείγμα έργου δυναμικού περιεχομένου Web]: http://go.microsoft.com/fwlink/?LinkId=817337

[Κέντρο για προγραμματιστές του Azure Java]: https://azure.microsoft.com/develop/java/
[Επισκόπηση εφαρμογών Web]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
