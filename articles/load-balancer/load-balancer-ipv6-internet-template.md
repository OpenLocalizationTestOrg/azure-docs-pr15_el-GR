<properties
    pageTitle="Ανάπτυξη μιας μέσω Internet εξισορρόπηση φόρτου λύσης με το IPv6 χρησιμοποιώντας ένα πρότυπο | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να αναπτύξετε υποστήριξη IPv6 για το πρόγραμμα εξισορρόπησης φόρτου Azure και εξισορρόπηση φόρτου ΣΠΣ."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="το IPv6, azure εξισορρόπηση φόρτου, διπλή στοίβα, δημόσια ip, εγγενούς ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Ανάπτυξη μιας μέσω Internet μονάδα εξισορρόπησης φόρτου της λύσης με το IPv6 χρησιμοποιώντας ένα πρότυπο

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Πρότυπο](./load-balancer-ipv6-internet-template.md)

Μια μονάδα εξισορρόπησης φόρτου Azure είναι μια μονάδα εξισορρόπησης φόρτου επιπέδου-4 (TCP, UDP). Η μονάδα εξισορρόπησης φόρτου παρέχει υψηλή διαθεσιμότητα από τη διανομή εισερχόμενη κίνηση μεταξύ των παρουσιών σε καλή κατάσταση υπηρεσίας στις υπηρεσίες cloud ή εικονικές μηχανές σε ένα σύνολο εξισορρόπησης φόρτου. Azure εξισορρόπηση φόρτου επίσης να εμφανίσετε αυτές τις υπηρεσίες σε πολλαπλές θύρες, πολλές διευθύνσεις IP ή και τα δύο.

## <a name="example-deployment-scenario"></a>Παράδειγμα ανάπτυξης σεναρίου

Το παρακάτω διάγραμμα παρουσιάζει το λύση εξισορρόπησης φόρτου αναπτύσσεται με χρήση του προτύπου παράδειγμα που περιγράφονται σε αυτό το άρθρο.

![Σενάριο εξισορρόπησης φόρτου](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

Σε αυτό το σενάριο θα δημιουργήσετε στους παρακάτω πόρους Azure:

- περιβάλλον εικονικού δικτύου για κάθε Εικονική με διευθύνσεις IPv4 και IPv6 στους οποίους έχουν ανατεθεί
- μια μονάδα εξισορρόπησης φόρτου μέσω Internet με ένα IPv4 και μια IPv6 δημόσια διεύθυνση IP
- δύο φόρτωση εξισορρόπησης κανόνες για να αντιστοιχίσετε τη δημόσια VIPs για τα τελικά σημεία ιδιωτική
- μια ρύθμιση διαθεσιμότητα που περιέχει τα δύο ΣΠΣ
- δύο εικονικές μηχανές (ΣΠΣ)

## <a name="deploying-the-template-using-the-azure-portal"></a>Για την ανάπτυξη του προτύπου με την πύλη Azure

Σε αυτό το άρθρο αναφέρεται σε ένα πρότυπο που δημοσιεύεται στη συλλογή [Προτύπων γρήγορη έναρξη Azure](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) . Μπορείτε να κάνετε λήψη του προτύπου από τη συλλογή ή εκκίνηση της ανάπτυξης στο Azure απευθείας από τη συλλογή. Σε αυτό το άρθρο προϋποθέτει ότι έχετε λάβει το πρότυπο στον τοπικό σας υπολογιστή.

1. Ανοίξτε την πύλη του Azure και πραγματοποιήστε είσοδο με ένα λογαριασμό που έχει δικαιώματα για τη δημιουργία ΣΠΣ και πόροι δικτύωσης μιας συνδρομής του Azure. Επίσης, εκτός και εάν χρησιμοποιείτε το υπάρχον πόρους, το λογαριασμό πρέπει δικαιωμάτων για να δημιουργήσετε μια ομάδα πόρων και ένα λογαριασμό του χώρου αποθήκευσης.

2. Κάντε κλικ στην επιλογή "+ Δημιουργία" από το μενού, στη συνέχεια, πληκτρολογήστε "πρότυπο" στο πλαίσιο αναζήτησης. Επιλέξτε "Ανάπτυξη του προτύπου" από τα αποτελέσματα αναζήτησης.

    ![λίβρες-ipv6-πύλη-βήμα 2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. Σε όλα blade, κάντε κλικ στο κουμπί "Ανάπτυξη του προτύπου".

    ![λίβρες-ipv6-πύλη-βήμα 3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Κάντε κλικ στην επιλογή "Δημιουργία".

    ![λίβρες-ipv6-πύλη-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Κάντε κλικ στην επιλογή "Επεξεργασία προτύπου". Διαγράψτε το υπάρχον περιεχόμενο και αντιγράψτε και επικολλήστε σε όλα τα περιεχόμενα του αρχείου του προτύπου (για να συμπεριλάβετε στην αρχή και το τέλος {}), στη συνέχεια, κάντε κλικ στην επιλογή "Αποθήκευση."

    > [AZURE.NOTE] Εάν χρησιμοποιείτε το Microsoft Internet Explorer, όταν κάνετε επικόλληση θα εμφανιστεί ένα παράθυρο διαλόγου που σας ζητά να επιτρέψετε την πρόσβαση στο Πρόχειρο των Windows. Κάντε κλικ στην επιλογή "Επιτρέπεται η πρόσβαση".

    ![λίβρες-ipv6-πύλη-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Κάντε κλικ στην επιλογή "Επεξεργασία παράμετροι". Στο το blade παραμέτρων, καθορίστε τις τιμές ανά τις οδηγίες στην ενότητα παράμετροι πρότυπο και, στη συνέχεια, κάντε κλικ στην επιλογή "Αποθήκευση" για να κλείσετε το blade παραμέτρους. Στο το blade ανάπτυξης προσαρμογή, επιλέξτε τη συνδρομή σας, μια υπάρχουσα ομάδα πόρων ή δημιουργήστε μία. Εάν θέλετε να δημιουργήσετε μια ομάδα πόρων, στη συνέχεια, επιλέξτε μια θέση για την ομάδα πόρων. Στη συνέχεια, κάντε κλικ στην επιλογή **νομική όρους**, στη συνέχεια, κάντε κλικ στην επιλογή **αγορά** για τους όρους νομική. Azure αρχίζει την ανάπτυξη τους πόρους. Χρειάζονται μερικά λεπτά για την ανάπτυξη όλους τους πόρους.

    ![λίβρες-ipv6-πύλη-ΒΗΜΑ 6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Για περισσότερες πληροφορίες σχετικά με αυτές τις παραμέτρους, ανατρέξτε στην ενότητα [παραμέτρους προτύπου και μεταβλητές](#template-parameters-and-variables) αργότερα σε αυτό το άρθρο.

7. Για να δείτε τους πόρους που δημιουργήθηκε από το πρότυπο, κάντε κλικ στο κουμπί Αναζήτηση, κάντε κύλιση προς τα κάτω στη λίστα μέχρι να δείτε "Ομάδες πόρων", στη συνέχεια, κάντε κλικ στην επιλογή.

    ![λίβρες-ipv6-πύλη-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Στην το blade ομάδες πόρων, κάντε κλικ στο όνομα της ομάδας πόρων που καθορίσατε στο βήμα 6. Μπορείτε να δείτε μια λίστα με όλους τους πόρους που είχαν αναπτυχθεί. Εάν όλα είναι σωστά, θα πρέπει να υπάρχει η ένδειξη "Ολοκληρώθηκε με" στην περιοχή "Τελευταία ανάπτυξης". Εάν όχι, βεβαιωθείτε ότι το λογαριασμό που χρησιμοποιείτε έχει δικαιώματα για να δημιουργήσετε οι πόροι είναι απαραίτητο.

    ![λίβρες-ipv6-πύλη-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [AZURE.NOTE] Εάν πραγματοποιήσετε αναζήτηση των ομάδων πόρων αμέσως μετά την ολοκλήρωση βήμα 6, "Τελευταία ανάπτυξης" θα εμφανίσει την κατάσταση "Ανάπτυξη" ενώ αναπτύσσονται τους πόρους.

9. Κάντε κλικ στην επιλογή "myIPv6PublicIP" στη λίστα των πόρων. Βλέπετε ότι έχει μια διεύθυνση IPv6 στην περιοχή διεύθυνση IP και ότι το όνομα DNS είναι η τιμή που καθορίσατε για την παράμετρο dnsNameforIPv6LbIP στο βήμα 6. Αυτός ο πόρος είναι το δημόσια IPv6 διευθύνσεων και κεντρικών υπολογιστών όνομα που είναι προσβάσιμος σε προγράμματα-πελάτες του Internet.

    ![λίβρες-ipv6-πύλη-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Επικυρώστε συνδεσιμότητας

Όταν το πρότυπο έχει αναπτυχθεί με επιτυχία, μπορείτε να επικυρώσετε τη σύνδεση, ακολουθώντας τις παρακάτω εργασίες:

1. Πραγματοποιήστε είσοδο στην πύλη του Azure και σύνδεση σε κάθε ένα από τα ΣΠΣ που δημιουργήθηκε από την ανάπτυξη προτύπου. Εάν έχετε αναπτύξει μια Εικονική διακομιστή των Windows, εκτελέστε ipconfig/όλων από μια γραμμή εντολών. Μπορείτε να δείτε ότι το ΣΠΣ έχουν διευθύνσεις IPv4 και IPv6. Εάν αναπτύξει ΣΠΣ Linux, πρέπει να ρυθμίσετε το λειτουργικό σύστημα Linux να λαμβάνετε δυναμικές διευθύνσεις IPv6 με τις οδηγίες που παρέχονται για τη διανομή Linux σας.
2. Από ένα πελάτη συνδέονται μέσω του IPv6 Internet, ξεκινήστε μια σύνδεση στη δημόσια διεύθυνση IPv6 από τη μονάδα εξισορρόπησης φόρτου. Για να επιβεβαιώσετε ότι η μονάδα εξισορρόπησης φόρτου εξισορρόπηση μεταξύ του δύο ΣΠΣ, ενδέχεται να μπορείτε να εγκαταστήσετε σε διακομιστή web, όπως το Microsoft Internet Information Services (IIS) σε κάθε του ΣΠΣ. Η προεπιλεγμένη ιστοσελίδα σε κάθε διακομιστή μπορεί να περιέχει το κείμενο "Server0" ή "Server1" για να προσδιορίσει μοναδικά. Στη συνέχεια, ανοίξτε ένα πρόγραμμα περιήγησης Internet σε ένα πρόγραμμα-πελάτη που συνδέονται μέσω του IPv6 Internet και αναζητήστε το όνομα κεντρικού υπολογιστή που έχει καθοριστεί για την παράμετρο dnsNameforIPv6LbIP από τη μονάδα εξισορρόπησης φόρτου για να επιβεβαιώσετε τη συνδεσιμότητα IPv6 λήξη-σε-end για κάθε Εικονική. Εάν βλέπετε μόνο την ιστοσελίδα από μία μόνο διακομιστή, ίσως χρειαστεί να κάνετε εκκαθάριση του cache του προγράμματος περιήγησης. Άνοιγμα πολλών περιόδους λειτουργίας περιήγησης ιδιωτικό. Θα πρέπει να βλέπετε μια απάντηση από κάθε διακομιστή.
3. Από ένα πρόγραμμα-συνδέονται μέσω του IPv4 Internet πελάτη, ξεκινήσει μια σύνδεση στη δημόσια διεύθυνση IPv4 από τη μονάδα εξισορρόπησης φόρτου. Για να επιβεβαιώσετε ότι η μονάδα εξισορρόπησης φόρτου είναι τα δύο ΣΠΣ εξισορρόπησης φόρτου, θα μπορούσατε να δοκιμάσετε με χρήση των υπηρεσιών IIS, όπως περιγράφεται στο βήμα 2.
4. Από κάθε Εικονική, ξεκινήστε μια εξερχόμενη σύνδεση σε μια συσκευή IPv6 ή IPv4 με σύνδεση στο Internet. Και στις δύο περιπτώσεις, η διεύθυνση IP προέλευσης ορατό από τη συσκευή προορισμού είναι στη δημόσια διεύθυνση IPv4 ή IPv6 από τη μονάδα εξισορρόπησης φόρτου.

>[AZURE.NOTE]
Πακέτα ICMP IPv4 και IPv6 έχει αποκλειστεί στο Azure δίκτυο. Ως αποτέλεσμα, τα εργαλεία ICMP όπως ping πάντα αποτύχει. Για να ελέγξετε τη σύνδεση, χρησιμοποιήστε μια εναλλακτική λύση TCP όπως TCPing ή το cmdlet του PowerShell δοκιμής-NetConnection. Σημειώστε ότι παραδείγματα των τιμών που μπορεί να δείτε τις διευθύνσεις IP που εμφανίζεται στο διάγραμμα. Επειδή οι διευθύνσεις IPv6 αντιστοιχίζονται δυναμικά, τις διευθύνσεις που λαμβάνετε θα διαφέρουν και ενδέχεται να διαφέρουν ανά περιοχή. Επίσης, είναι συνηθισμένο για τη δημόσια διεύθυνση IPv6 σε τη μονάδα εξισορρόπησης φόρτου για να ξεκινήσετε με μια διαφορετική πρόθεμα από τις διευθύνσεις IPv6 ιδιωτικό στο χώρο συγκέντρωσης παρασκηνίου.

## <a name="template-parameters-and-variables"></a>Παράμετροι προτύπων και μεταβλητές

Ένα πρότυπο από διαχειριστή πόρων Azure περιέχει πολλές μεταβλητές και οι παράμετροι που μπορείτε να προσαρμόσετε στις ανάγκες σας. Μεταβλητές που χρησιμοποιούνται για τις σταθερές τιμές που δεν θέλετε ένας χρήστης για να αλλάξετε. Παράμετροι χρησιμοποιούνται για τις τιμές που θέλετε ένας χρήστης να παράσχετε κατά την ανάπτυξη του προτύπου. Το πρότυπο παράδειγμα έχει ρυθμιστεί για το σενάριο που περιγράφονται σε αυτό το άρθρο. Μπορείτε να το προσαρμόσετε στις ανάγκες του περιβάλλοντός σας.

Το παράδειγμα προτύπου που χρησιμοποιείται σε αυτό το άρθρο περιλαμβάνει τα παρακάτω μεταβλητές και παραμέτρους:

| Η παράμετρος / μεταβλητής | Σημειώσεις |
|-----------|-------|
| adminUsername | Καθορίστε το όνομα του λογαριασμού διαχειριστή που χρησιμοποιείται για να εισέλθετε στο τις εικονικές μηχανές με. |
| adminPassword | Καθορίστε τον κωδικό πρόσβασης για το λογαριασμό διαχειριστή που χρησιμοποιείται για την είσοδο για να τις εικονικές μηχανές με. |
| dnsNameforIPv4LbIP | Καθορίστε το όνομα κεντρικού υπολογιστή DNS που θέλετε να αντιστοιχίσετε ως δημόσια όνομα από τη μονάδα εξισορρόπησης φόρτου. Αυτό το όνομα επιλύει το εξισορρόπηση φόρτου δημόσια διεύθυνση IPv4. Το όνομα πρέπει να είναι πεζά και ταιριάζει με το regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP | Καθορίστε το όνομα κεντρικού υπολογιστή DNS που θέλετε να αντιστοιχίσετε ως δημόσια όνομα από τη μονάδα εξισορρόπησης φόρτου. Αυτό το όνομα επιλύει το εξισορρόπηση φόρτου δημόσια διεύθυνση IPv6. Το όνομα πρέπει να είναι πεζά και ταιριάζει με το regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. Αυτό είναι το ίδιο όνομα με τη διεύθυνση IPv4. Όταν ένας υπολογιστής-πελάτης στέλνει ένα ερώτημα DNS για αυτό το όνομα που θα επιστρέψει Azure A and AAAA εγγραφών όταν το όνομα έχει θέσει σε κοινή χρήση. |
| vmNamePrefix | Καθορίστε το πρόθεμα ονόματος Εικονική. Το πρότυπο προσαρτά έναν αριθμό (0, 1, κ.λπ.) στο όνομα κατά τη δημιουργία του ΣΠΣ. |
| nicNamePrefix | Καθορίστε το πρόθεμα ονόματος περιβάλλοντος εργασίας δικτύου. Το πρότυπο προσαρτά έναν αριθμό (0, 1, κ.λπ.) στο όνομα όταν δημιουργούνται τις διασυνδέσεις δικτύου. |
| storageAccountName | Πληκτρολογήστε το όνομα του υπάρχοντος λογαριασμού χώρου αποθήκευσης ή να καθορίσετε το όνομα του ένα νέο να δημιουργηθεί από το πρότυπο. |
| availabilitySetName | Στη συνέχεια, εισαγάγετε όνομα από τη ρύθμιση για χρήση με το ΣΠΣ διαθεσιμότητα |
| addressPrefix | Το πρόθεμα διεύθυνση που χρησιμοποιείται για να ορίσετε μια περιοχή διευθύνσεων του εικονικού δικτύου |
| subnetName | Το όνομα του υποδικτύου στο που έχει δημιουργηθεί για το VNet |
| subnetPrefix | Το πρόθεμα διεύθυνση που χρησιμοποιείται για να ορίσετε μια περιοχή διευθύνσεων του υποδικτύου |
| vnetName | Καθορίστε το όνομα για το VNet που χρησιμοποιούνται από το ΣΠΣ. |
| ipv4PrivateIPAddressType | Η μέθοδος εκχώρησης που χρησιμοποιείται για την ιδιωτική διεύθυνση IP (στατική ή δυναμική) |
| ipv6PrivateIPAddressType | Η μέθοδος εκχώρησης που χρησιμοποιείται για την ιδιωτική διεύθυνση IP (δυναμική). Το IPv6 υποστηρίζει μόνο δυναμική εκχώρηση. |
| numberOfInstances | Ο αριθμός των παρουσιών εξισορρόπησης φόρτου αναπτυχθεί από το πρότυπο |
| ipv4PublicIPAddressName | Καθορίστε το όνομα DNS που θέλετε να χρησιμοποιήσετε για να επικοινωνήσετε με τη δημόσια διεύθυνση IPv4 τη μονάδα εξισορρόπησης φόρτου. |
| ipv4PublicIPAddressType | Η μέθοδος εκχώρησης που χρησιμοποιείται για τη δημόσια διεύθυνση IP (στατική ή δυναμική) |
| Ipv6PublicIPAddressName | Καθορίστε το όνομα DNS που θέλετε να χρησιμοποιήσετε για να επικοινωνήσετε με τη δημόσια διεύθυνση IPv6 τη μονάδα εξισορρόπησης φόρτου. |
| ipv6PublicIPAddressType | Η μέθοδος εκχώρησης που χρησιμοποιείται για τη δημόσια διεύθυνση IP (δυναμική). Το IPv6 υποστηρίζει μόνο δυναμική εκχώρηση. |
| lbName | Καθορίστε το όνομα του τη μονάδα εξισορρόπησης φόρτου. Αυτό το όνομα που εμφανίζεται στην πύλη ή χρησιμοποιείται κατά την αναφορά με μια εντολή CLI ή PowerShell. |

Οι υπόλοιπες μεταβλητές στο πρότυπο περιέχει που προέρχονται τιμές που έχουν εκχωρηθεί όταν Azure δημιουργεί τους πόρους. Μην αλλάξετε αυτές τις μεταβλητές.
