<properties 
    pageTitle="Κάντε λήψη του Azure SDK για Java" 
    description="Μάθετε πώς μπορείτε να κάνετε λήψη του Azure SDK για Java, με το δείγμα κώδικα που παρέχεται για Maven έργα και βασικά βήματα εγκατάσταση για το Tookit Azure για Έκλειψη." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="multiple" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="download-the-azure-sdk-for-java"></a>Κάντε λήψη του Azure SDK για Java

Σε αυτό το άρθρο περιέχει οδηγίες για τη λήψη και εγκατάσταση των βιβλιοθηκών Azure για Java.

**Σημείωση:** Οι βιβλιοθήκες Azure για Java είναι διανέμονται σύμφωνα με την [άδεια χρήσης Apache, έκδοση 2.0][license].

## <a name="azure-libraries-for-java---manual-download"></a>Azure βιβλιοθηκών για Java - μη αυτόματη λήψη

Για να εγκαταστήσετε με μη αυτόματο τρόπο τις βιβλιοθήκες Azure για Java, κάντε κλικ στην επιλογή <http://go.microsoft.com/fwlink/?LinkId=690320> για να κάνετε λήψη σε ένα αρχείο ZIP που περιέχει όλες τις βιβλιοθήκες και όλων των εξαρτήσεων.

Αφού έχετε κάνει λήψη του αρχείου zip στον υπολογιστή σας, κάνετε εξαγωγή των περιεχομένων και χρησιμοποιήστε μία από τις παρακάτω επιλογές για να προσθέσετε τα αρχεία ΒΆΖΟ στο έργο σας:

* Εισαγάγετε τα αρχεία ΒΆΖΟ στο έργο σας Java με έκλειψη.

* Ρύθμιση παραμέτρων τη **Δόμηση διαδρομή** για το έργο σας Java Έκλειψη για να συμπεριλάβετε τη διαδρομή προς τα αρχεία ΒΆΖΟ.

Για λεπτομερείς πληροφορίες σχετικά με τη ρύθμιση του Δόμηση διαδρομές στο Έκλειψη, ανατρέξτε στο άρθρο [Διαδρομή Δόμηση Java] στην τοποθεσία Web του Έκλειψη.

**Σημείωση:** Δείτε το αρχείο license.txt και ThirdPartyNotices.txt αρχείο μέσα ZIP για άδεια χρήσης και άλλες πληροφορίες.

## <a name="azure-libraries-for-java---building-with-maven"></a>Βιβλιοθήκες του Azure για Java - δημιουργία με Maven

### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Βήμα 1 - ρύθμιση του έργου σας για να χρησιμοποιήσετε Maven για δημιουργία

Για να δημιουργήσετε έργα Maven στο Έκλειψη που χρησιμοποιούν τις βιβλιοθήκες Azure για Java, ακολουθώντας τις οδηγίες στο θέμα [Γρήγορα αποτελέσματα με το Azure βιβλιοθήκες διαχείρισης για Java] το[ maven-getting-started] το άρθρο. 

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Βήμα 2 - ρύθμιση παραμέτρων σας Maven με τις απαιτούμενες εξαρτήσεις

Μόλις το έργο σας έχει ρυθμιστεί για να χρησιμοποιήσετε Maven για δημιουργία, μπορείτε να προσθέσετε το τις απαιτούμενες εξαρτήσεις με το αρχείο pom.xml χρησιμοποιώντας τη σύνταξη, όπως το παρακάτω παράδειγμα. Σημειώστε ότι δεν χρειάζεται να προσθέσετε κάθε εξάρτηση που εμφανίζεται στο παρακάτω παράδειγμα, πρέπει να προσθέσετε τη συγκεκριμένη εξαρτήσεις που απαιτεί το έργο σας.

> [AZURE.NOTE] Μέσα σε κάθε `<version>` στοιχείο σε το παρακάτω παράδειγμα, αντικαταστήστε τα σύμβολα κράτησης θέσης "n.n.n" σε αυτό το παράδειγμα με αριθμούς έγκυρη έκδοση, οι οποίοι μπορεί να ληφθεί από το [Αρχείο φύλαξης βιβλιοθήκες Azure στο Maven].

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Κατά την εγκατάσταση του Κιτ εργαλείων Azure για Έκλειψη

Αυτή η ενότητα περιέχει βασικές οδηγίες για την εγκατάσταση του Κιτ εργαλείων Azure για Έκλειψη; Για λεπτομερείς οδηγίες, ανατρέξτε στο θέμα [κατά την εγκατάσταση του Κιτ εργαλείων Azure για Έκλειψη].

### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

1. Συστήματα operting Windows που παρατίθεται στο άρθρο [Τι νέο υπάρχει στο του Κιτ εργαλείων Azure για Έκλειψη] .
1. Macintosh ή Linux συστήματα operting που παρατίθεται στο άρθρο [Τι νέο υπάρχει στο του Κιτ εργαλείων Azure για Έκλειψη] .
1. Έκλειψη-IDE για τους προγραμματιστές ΕΕ Java, Indigo ή νεότερη έκδοση. Αυτό μπορεί να ληφθεί από <http://www.eclipse.org/downloads/>.

### <a name="basic-installation-steps"></a>Βασικά βήματα εγκατάστασης

1. Στο Έκλειψη, από το μενού **Βοήθεια** , επιλέξτε **Εγκατάσταση νέου λογισμικού**.
1. Πληκτρολογήστε τη θέση της τοποθεσίας <http://dl.microsoft.com/eclipse> και πατήστε το πλήκτρο **Enter**.
1. Επιλέξτε τα στοιχεία να έχει εγκατασταθεί και κάντε κλικ στο κουμπί **Τέλος**.

Το Κιτ εργαλείων Azure για Έκλειψη χρησιμοποιεί την πιο πρόσφατη έκδοση του Azure SDK. Αυτό μπορεί να ληφθεί χρησιμοποιώντας το πρόγραμμα εγκατάστασης πλατφόρμας Web (WebPI) στην <http://go.microsoft.com/fwlink/?LinkID=252838>. Ωστόσο, εάν δεν έχετε εγκαταστήσει, όταν δημιουργείτε πρώτη ανάπτυξη του Azure το έργο σας, το Κιτ εργαλείων Azure για Έκλειψη θα εγκαταστήσουν αυτόματα την κατάλληλη έκδοση του Azure SDK.

## <a name="see-also"></a>Δείτε επίσης

[Azure Κιτ εργαλείων για Έκλειψη]

[Κατά την εγκατάσταση του Κιτ εργαλείων Azure για Έκλειψη] 

[Δημιουργία εφαρμογής κόσμο Hello για Azure στο Έκλειψη]

Για περισσότερες πληροφορίες σχετικά με τη χρήση Azure με Java, ανατρέξτε στο [Κέντρο για προγραμματιστές του Azure Java].

<!-- URL List -->

[Κέντρο για προγραμματιστές του Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Αρχείο φύλαξης Azure βιβλιοθήκες στο Maven]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure Κιτ εργαλείων για Έκλειψη]: http://go.microsoft.com/fwlink/?LinkID=699529
[Δημιουργία εφαρμογής κόσμο Hello για Azure στο Έκλειψη]: http://go.microsoft.com/fwlink/?LinkID=699533
[Κατά την εγκατάσταση του Κιτ εργαλείων Azure για Έκλειψη]: http://go.microsoft.com/fwlink/?LinkId=699546
[Διαδρομή Δόμηση Java]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Τι νέο υπάρχει στο του Azure Κιτ εργαλείων για Έκλειψη]: http://go.microsoft.com/fwlink/?LinkId=690333
