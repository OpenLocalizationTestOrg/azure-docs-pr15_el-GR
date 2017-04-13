<properties
    pageTitle="Παράδειγμα MyDriving Azure IoT: γρήγορης εκκίνησης | Microsoft Azure"
    description="Γρήγορα αποτελέσματα με μια εφαρμογή που είναι μια ολοκληρωμένη επίδειξη του τρόπου αρχιτεκτονική ένα σύστημα IoT με χρήση του Windows Azure, συμπεριλαμβανομένης της ροής ανάλυσης, μηχανικής εκμάθησης και διανομείς συμβάν."
    services=""
    documentationCenter=".net"
    suite=""
    authors="harikmenon"
    manager="douge"/>

<tags
    ms.service="multiple"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="03/25/2016"
    ms.author="harikm"/>

# <a name="mydriving-iot-system-quick-start"></a>Σύστημα MyDriving IoT: γρήγορης εκκίνησης

MyDriving είναι ένα σύστημα που δείχνει τη σχεδίαση και την εφαρμογή μιας τυπικής λύσης [Internet του πράγματα](iot-suite-overview.md) (IoT) που συγκεντρώνει τηλεμετρίας από συσκευές, επεξεργάζεται αυτών των δεδομένων στο cloud και ισχύει μηχανικής εκμάθησης για την παροχή μια προσαρμόσιμη απάντηση. Την επίδειξη καταγράφει δεδομένα σχετικά με το αυτοκίνητο ταξίδια, χρησιμοποιώντας δεδομένα από το κινητό σας τηλέφωνο και έναν προσαρμογέα που συλλέγει πληροφορίες από το σύστημα ελέγχου του αυτοκινήτου. Χρησιμοποιεί αυτά τα δεδομένα για να σχολιάσετε τις οδηγίες στυλ σε σύγκριση με άλλους χρήστες.

Το πραγματικό σκοπός της MyDriving είναι για να ξεκινήσετε τη δημιουργία τη δική σας λύση IoT. Αλλά πριν από την οποία, ας ξεκινήσουμε που χρησιμοποιεί το MyDriving ίδια την εφαρμογή--ως μέλος της ομάδας χρήστη δοκιμής. Αυτό σας δίνει μια εμπειρία από την εφαρμογή και το σύστημα από πίσω ως καταναλωτή, πριν από την που το delve στο την αρχιτεκτονική. Αυτό επίσης σάς παρουσιάζει HockeyApp, εντυπωσιακές τρόπο από τη διαχείριση του άλφα και βήτα κατανομές από τις εφαρμογές σας για να ελέγξετε τους χρήστες.

## <a name="use-the-mobile-experience"></a>Χρησιμοποιήστε την εμπειρία με κινητή συσκευή

Εάν έχετε μια συσκευή Android, iOS ή Windows 10, μπορείτε να χρησιμοποιήσετε την εφαρμογή MyDriving.

### <a name="android-and-windows-10-mobile-installation"></a>Android και Windows 10 Mobile εγκατάστασης

Στη συσκευή σας:

1.  Να επιτρέπεται σε εφαρμογές ανάπτυξη:

    -   Android: Στις **Ρυθμίσεις** > **ασφαλείας**, επιτρέψετε στις εφαρμογές από **άγνωστες προελεύσεις**.

    -   Windows 10: Στις **Ρυθμίσεις** > **ενημερώσεις** > **Για τους προγραμματιστές**, ορίστε την **κατάσταση λειτουργίας προγραμματιστή**.

2.  Συμμετοχή σε ομάδα μας βήτα δοκιμής, η εγγραφή στο με ή είσοδος σε [HockeyApp](https://rink.hockeyapp.net). HockeyApp διευκολύνει να διανείμετε πρώιμη εκδόσεις της εφαρμογής για να ελέγξετε τους χρήστες.

    Εάν χρησιμοποιείτε Windows 10, χρησιμοποιήστε το πρόγραμμα περιήγησης Edge.

    Αν ο συμμετέχων Δόμηση 2016, πραγματοποιήστε είσοδο με που έχουν καταχωρηθεί για διάσκεψη, χρησιμοποιώντας ένα από τα κουμπιά της Microsoft το ίδιο ηλεκτρονικού ταχυδρομείου λογαριασμό Microsoft. Έχετε ήδη εγγραφεί με HockeyApp.

    ![HockeyApp στην οθόνη εισόδου](./media/iot-solution-get-started/image1.png)

3.  Κάντε λήψη και εγκαταστήστε την εφαρμογή από εδώ:

    -   [Android](http://rink.io/spMyDrivingAndroid)

    -   [Windows 10](http://rink.io/spMyDrivingUWP)

    Υπάρχουν δύο στοιχεία. Εγκαταστήστε το πιστοποιητικό στο **Αξιόπιστα άτομα**. Στη συνέχεια, εγκαταστήστε την εφαρμογή.

*Προβλήματα εκκίνηση της εφαρμογής σε Windows 10 Mobile;* Το τηλέφωνό σας μπορεί να είναι μια ενημέρωση ή δύο πίσω από. Βεβαιωθείτε ότι έχετε στη διάθεσή σας τις πιο πρόσφατες ενημερώσεις, ή εάν εγκαταστήσετε:

 - [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 

 - [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 

 - [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)


### <a name="ios-installation"></a>εγκατάσταση του iOS

Εάν παρακολούθηση Δόμηση 2016, κάντε λήψη της εφαρμογής ως μέλος της ομάδας δοκιμής στην HockeyApp:

1.  Στη συσκευή σας iOS, πραγματοποιήστε είσοδο στο [HockeyApp](https://rink.hockeyapp.net).
    Χρησιμοποιήστε μία από τις Microsoft εισόδου κουμπιά και πραγματοποιήστε είσοδο με το ίδιο Microsoft λογαριασμό ηλεκτρονικού ταχυδρομείου που καταχωρήσατε με τη διάσκεψη. (Μην χρησιμοποιείτε τα πεδία ηλεκτρονικού ταχυδρομείου και τον κωδικό πρόσβασης.)

    ![HockeyApp στην οθόνη εισόδου](./media/iot-solution-get-started/image1.png)

2.  Στον πίνακα εργαλείων HockeyApp, επιλέξτε MyDriving και κάντε λήψη του.

3.  Επιτρέπει την έκδοση beta από HockeyApp:

    μια. Μεταβείτε στις **Ρυθμίσεις** > **Γενικά** > **προφίλ και διαχείριση συσκευών.**

    β. Αξιόπιστο το πιστοποιητικό **GmbH σταδίου Bit** .

Εάν δεν μπορείτε να συμμετάσχετε σε Δόμηση 2016, μπορείτε να δημιουργήσετε και ανάπτυξη της εφαρμογής στον εαυτό σας:

1.   Κάντε λήψη του κώδικα [από GitHub].

2.   Δημιουργήστε και αναπτύξτε χρησιμοποιώντας [Xamarin].

Δείτε περισσότερες λεπτομέρειες του [Οδηγού αναφοράς MyDriving](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Λάβετε έναν προσαρμογέα OBD (προαιρετικά)

Αυτό είναι το τμήμα που το κάνει αυτό ένα πραγματικό σύστημα Internet του πράγματα! Μπορείτε να χρησιμοποιήσετε την εφαρμογή χωρίς ένα, αλλά είναι Διασκεδάστε περισσότερο με το πραγματικό πράγμα και δεν είναι ακριβό.

Ενσωματωμένα διαγνωστικά (OBD) είναι η δυνατότητα σας αυτοκινήτου που χρησιμοποιεί την αμερικανική για βελτιστοποίηση σας αυτοκινήτου και διάγνωση μονές θορύβων και λαμπτήρων προειδοποίησης. Εκτός εάν το αυτοκίνητο είναι ιδανική antiquity, θα βρείτε μια υποδοχή κάπου αλλού στο θάλαμο, συνήθως πίσω από ένα τα μέσα στην περιοχή του πίνακα εργαλείων. Με τη σωστή γραμμή σύνδεσης, μπορείτε να λάβετε μετρικά από το μηχανισμό απόδοσης και να κάνετε ορισμένες ρυθμίσεις. Μια γραμμή σύνδεσης OBD μπορεί να αγοραστεί δαπανηρές από τα συνήθη σημεία. Σύνδεση με χρήση Bluetooth ή Wi-Fi για μια εφαρμογή στο τηλέφωνό σας.

Σε αυτήν την περίπτωση, θα κάνουμε σύνδεσή σας αυτοκίνητο στο cloud. Η άμεση σύνδεση από το OBD είναι στο τηλέφωνό σας, αλλά λειτουργεί ως μια μετάδοση μας εφαρμογή. Τηλεμετρίας του αυτοκινήτου αποστέλλεται απευθείας την ενότητα MyDriving IoT, όπου γίνεται η επεξεργασία για να συνδεθείτε σας ταξίδια δρόμων και για την αξιολόγησή σας οδήγησης στυλ.

Για να συνδέσετε μια συσκευή OBD:

1.  Βεβαιωθείτε ότι το αυτοκίνητο διαθέτει μια υποδοχή OBD.

2.  Αποκτήστε έναν προσαρμογέα OBD:

    -   Εάν χρησιμοποιείτε το τηλέφωνο Android ή των Windows, χρειάζεστε έναν προσαρμογέα Bluetooth με δυνατότητα II OBD. Χρησιμοποιήσαμε [Προϊόντα BAFX 34t5 Bluetooth OBDII σάρωση εργαλείο].

    -   Εάν χρησιμοποιείτε το τηλέφωνο iOS, χρειάζεστε έναν προσαρμογέα OBD ενεργοποιημένη Wi-Fi. Χρησιμοποιήσαμε [ScanTool OBDLink MX Wi-Fi: OBD προσαρμογέα/Διαγνωστικά σαρωτή].

3.  Ακολουθήστε τις οδηγίες που παρέχονται με προσαρμογέα OBD, για να συνδέσετε με το τηλέφωνό σας. Έχετε υπόψη τα εξής:

    -   Έναν προσαρμογέα Bluetooth πρέπει να συνδυάζονται με το τηλέφωνο, στη σελίδα " **Ρυθμίσεις** ".

    -   Έναν προσαρμογέα Wi-Fi πρέπει να έχει μια διεύθυνση 192.168.xxx.xxx την περιοχή.

4.  Εάν έχετε πολλές αυτοκινήτων, μπορείτε να λάβετε μια ξεχωριστή προσαρμογέα για κάθε (έως τρία).

Εάν δεν έχετε έναν προσαρμογέα OBD, η εφαρμογή θα εξακολουθούν να στείλει θέση και της ταχύτητας δεδομένων από το τηλέφωνο δέκτη GPS παρασκηνίου και θα σας ρωτήσει εάν θέλετε να προσομοιώνουν μια OBD.

Μπορείτε να μάθετε περισσότερα σχετικά με τον τρόπο την εφαρμογή που χρησιμοποιεί δεδομένα από τον προσαρμογέα OBD και σχετικά με τις επιλογές για να δημιουργήσετε τη δική σας συσκευή OBD στην ενότητα 2.1, "IoT συσκευές," στον [Οδηγό αναφοράς MyDriving](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>Χρήση της εφαρμογής

Ξεκινήστε την εφαρμογή. Υπάρχει ένα αρχικό γρήγορης έναρξης για να σας καθοδηγήσει πώς λειτουργεί.

### <a name="track-your-trips"></a>Παρακολούθηση σας ταξίδια

Πατήστε το κουμπί εγγραφής (μεγάλο κόκκινο κύκλο στο κάτω μέρος της οθόνης) για να ξεκινήσετε ένα ταξίδι και πατήστε ξανά για να τερματίσετε.

![Εικόνα του κουμπιού "Εγγραφή" ταξίδι παρακολούθησης](./media/iot-solution-get-started/image2.png)

Κάθε φορά που ξεκινάτε ένα ταξίδι, εάν δεν υπάρχει καμία OBD συσκευή, θα σας ζητηθεί εάν θέλετε να χρησιμοποιήσετε το simulator.

Στο τέλος ενός ταξιδιού, πατήστε το κουμπί διακοπής και λαμβάνετε μια σύνοψη.

![Παράδειγμα ενός ταξιδιού σύνοψης](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Εξετάστε το ταξίδια

![Παράδειγμα της προηγούμενης ταξίδι](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Ελέγξτε το προφίλ σας

![Παράδειγμα ενός προφίλ οδήγησης στυλ](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Στείλτε μας τα σχόλιά σας δοκιμής

Επειδή που δημιουργήσαμε MyDriving για να jumpstart τις δικές σας συστήματα IoT, θέλουμε σίγουρα να σας ακούσουμε σχετικά με το πόσο καλά λειτουργεί. Επιτρέψτε μας γνωρίζετε εάν:

- Αντιμετωπίσετε δυσκολίες ή προκλήσεις.

- Υπάρχει ένα σημείο επέκτασης που θα κάνουν πιο κατάλληλη για το σενάριό σας.

- Μπορείτε να βρείτε μια πιο αποτελεσματικός τρόπος για να εκτελέσετε συγκεκριμένες ανάγκες.

- Έχετε τις άλλες προτάσεις για τη βελτίωση της MyDriving ή παρούσα τεκμηρίωση.

Μέσα σε MyDriving ίδια την εφαρμογή, μπορείτε να χρησιμοποιήσετε το ενσωματωμένο μηχανισμό HockeyApp σχολίων: στο iOS και Android, απλώς σας δώσει το τηλέφωνό σας ένα τη ή χρησιμοποιήστε την εντολή μενού **σχολίων** . Αυτό θα επισυνάψει αυτόματα ένα στιγμιότυπο οθόνης, ώστε να μας ξέρετε τι μιλάτε σχετικά με. Και αν υπάρχουν οποιαδήποτε unfortunate παρουσιάζει σφάλμα, HockeyApp συλλέγει τα αρχεία καταγραφής σφαλμάτων για να πείτε μας σχετικά με τους. Μπορείτε επίσης να δώσετε σχόλια μέσω της [πύλης HockeyApp].

Μπορείτε να αρχείων ένα [ζήτημα σε GitHub], ή στείλτε τα σχόλιά σας παρακάτω (en-μας edition).

Θα χαρούμε να ακοή από εσάς!

## <a name="next-steps"></a>Επόμενα βήματα

-   Εξερεύνηση του [MyDriving Οδηγός αναφοράς](http://aka.ms/mydrivingdocs) για να κατανοήσετε τον τρόπο μας έχετε έχει σχεδιαστεί και να δημιουργηθεί ολόκληρο το σύστημα MyDriving.

-   [Δημιουργία και ανάπτυξη ενός συστήματος δική σας](iot-solution-build-system.md) με τη χρήση μας δεσμών ενεργειών της διαχείρισης πόρων Azure. Ο [Οδηγός αναφοράς MyDriving](http://aka.ms/mydrivingdocs) επίσης σάς καθοδηγεί περιοχών όπου θα κάνετε τις περισσότερες προσαρμογές.

  [από το GitHub]: https://github.com/Azure-Samples/MyDriving
  [χρήση Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
  [BAFX προϊόντα 34t5 Bluetooth OBDII εργαλείο ανίχνευσης]: http://www.amazon.com/gp/product/B005NLQAHS
  [Σαρωτή προσαρμογέα/Diagnostic OBD ScanTool OBDLink MX Wi-Fi:]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
  [Πύλη HockeyApp]: https://rink.hockeyapp.org
  [πρόβλημα στην GitHub]: https://github.com/Azure-Samples/MyDriving/issues