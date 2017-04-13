<properties
   pageTitle="Πώς μπορείτε να διαχειριστείτε τις ρυθμίσεις παραμέτρων υπηρεσιών και προφίλ | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να εργαστείτε με αρχεία ρύθμισης παραμέτρων υπηρεσίας ρυθμίσεις παραμέτρων και προφίλ | που αποθηκεύουν ρυθμίσεις για τα περιβάλλοντα ανάπτυξης και ρυθμίσεις για τις υπηρεσίες cloud της δημοσίευσης."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-manage-service-configurations-and-profiles"></a>Πώς μπορείτε να διαχειριστείτε τις ρυθμίσεις παραμέτρων υπηρεσιών και προφίλ

## <a name="overview"></a>Επισκόπηση

Όταν δημοσιεύετε μια υπηρεσία cloud, Visual Studio αποθηκεύουν ρυθμίσεις παραμέτρων σε δύο είδη των αρχείων ρύθμισης παραμέτρων: υπηρεσίας ρυθμίσεις παραμέτρων και προφίλ. Ρυθμίσεις παραμέτρων υπηρεσίας (αρχεία .cscfg) αποθηκεύουν ρυθμίσεις για τα περιβάλλοντα ανάπτυξης για μια υπηρεσία Azure cloud. Azure χρησιμοποιεί αυτά τα αρχεία ρύθμισης παραμέτρων κατά τη διαχείριση των υπηρεσιών cloud σας. Από την άλλη πλευρά, ρυθμίσεις για τις υπηρεσίες cloud δημοσίευσης αποθήκευσης προφίλ (.azurePubxml αρχεία). Αυτές οι ρυθμίσεις είναι μια εγγραφή από την επιλογή όταν χρησιμοποιείτε τον Οδηγό δημοσίευσης και χρησιμοποιούνται τοπικά από το Visual Studio. Αυτό το θέμα εξηγεί τον τρόπο εργασίας με τους δύο τύπους αρχείων ρύθμισης παραμέτρων.

## <a name="service-configurations"></a>Ρυθμίσεις παραμέτρων υπηρεσίας

Μπορείτε να δημιουργήσετε πολλές ρυθμίσεις παραμέτρων υπηρεσίας για να χρησιμοποιήσετε για κάθε μία από τις περιβάλλοντα ανάπτυξης. Για παράδειγμα, μπορείτε να δημιουργήσετε μια ρύθμιση παραμέτρων της υπηρεσίας για το τοπικό περιβάλλον που χρησιμοποιείτε για να εκτελέσετε και να δοκιμάσετε μια εφαρμογή του Azure και μια άλλη ρύθμιση παραμέτρων της υπηρεσίας για το περιβάλλον παραγωγής.

Μπορείτε να προσθέσετε, διαγραφή, να μετονομάσετε και να τροποποιείτε αυτές τις ρυθμίσεις παραμέτρων υπηρεσίας σύμφωνα με τις απαιτήσεις σας. Μπορείτε να διαχειριστείτε αυτές τις ρυθμίσεις παραμέτρων υπηρεσίας από το Visual Studio, όπως φαίνεται στην παρακάτω εικόνα.

![Διαχείριση ρυθμίσεων παραμέτρων υπηρεσίας](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Μπορείτε επίσης να ανοίξετε το παράθυρο διαλόγου **Διαχείριση ρυθμίσεις παραμέτρων** από τις σελίδες ιδιοτήτων του ρόλου. Για να ανοίξετε τις ιδιότητες για ένα ρόλο στο έργο σας Azure, ανοίξτε το μενού συντόμευσης για τον συγκεκριμένο ρόλο και, στη συνέχεια, επιλέξτε **Ιδιότητες**. Στην καρτέλα **Ρυθμίσεις** , αναπτύξτε τη λίστα **Ρύθμιση παραμέτρων της υπηρεσίας** και, στη συνέχεια, επιλέξτε **Διαχείριση** για να ανοίξετε το παράθυρο διαλόγου **Διαχείριση ρυθμίσεων παραμέτρων** .

### <a name="to-add-a-service-configuration"></a>Για να προσθέσετε μια ρύθμιση παραμέτρων της υπηρεσίας

1. Στην Εξερεύνηση λύσεων άνοιγμα του μενού συντόμευσης για το έργο Azure και, στη συνέχεια, επιλέξτε **Διαχείριση ρυθμίσεων παραμέτρων**.

    Εμφανίζεται το παράθυρο διαλόγου **Διαχείριση ρυθμίσεις παραμέτρων υπηρεσίας** .

1. Για να προσθέσετε μια ρύθμιση παραμέτρων της υπηρεσίας, πρέπει να δημιουργήσετε ένα αντίγραφο της υπάρχουσας ρύθμισης παραμέτρων. Για να το κάνετε αυτό, επιλέξτε τη ρύθμιση παραμέτρων που θέλετε να αντιγράψετε από τη λίστα όνομα και, στη συνέχεια, επιλέξτε **Δημιουργία αντιγράφου**.

1. (Προαιρετικό) Για να δώσετε τις παραμέτρους της υπηρεσίας ένα διαφορετικό όνομα, επιλέξτε τη νέα ρύθμιση παραμέτρων της υπηρεσίας από τη λίστα όνομα και, στη συνέχεια, επιλέξτε **Μετονομασία**. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε το όνομα που θέλετε να χρησιμοποιήσετε για αυτήν τη ρύθμιση παραμέτρων υπηρεσιών και, στη συνέχεια, επιλέξτε **OK**.

    Μια νέα υπηρεσία αρχείο ρύθμισης παραμέτρων που ονομάζεται ServiceConfiguration. [Νέο όνομα] .cscfg προστίθεται στο Azure έργο στην Εξερεύνηση λύσεων.


### <a name="to-delete-a-service-configuration"></a>Για να διαγράψετε μια ρύθμιση παραμέτρων της υπηρεσίας

1. Στην Εξερεύνηση λύσεων, ανοίξτε το μενού συντόμευσης για το έργο Azure και, στη συνέχεια, επιλέξτε **Διαχείριση ρυθμίσεων παραμέτρων**.

    Εμφανίζεται το παράθυρο διαλόγου **Διαχείριση ρυθμίσεις παραμέτρων υπηρεσίας** .

1. Για να διαγράψετε μια ρύθμιση παραμέτρων της υπηρεσίας, επιλέξτε τη ρύθμιση παραμέτρων που θέλετε να διαγράψετε από τη λίστα **όνομα** και, στη συνέχεια, επιλέξτε **Κατάργηση**. Εμφανίζεται ένα παράθυρο διαλόγου για να επιβεβαιώσετε ότι θέλετε να διαγράψετε αυτήν τη ρύθμιση παραμέτρων.

1. Επιλέξτε **Διαγραφή**.

     Το αρχείο ρύθμισης παραμέτρων υπηρεσίας καταργείται από το Azure έργο στην Εξερεύνηση λύσεων.


### <a name="to-rename-a-service-configuration"></a>Για να μετονομάσετε μια ρύθμιση παραμέτρων της υπηρεσίας

1. Στην Εξερεύνηση λύσεων, ανοίξτε το μενού συντόμευσης για το έργο Azure και, στη συνέχεια, επιλέξτε **Διαχείριση ρυθμίσεων παραμέτρων**.

    Εμφανίζεται το παράθυρο διαλόγου **Διαχείριση ρυθμίσεις παραμέτρων υπηρεσίας** .

1. Για να μετονομάσετε μια ρύθμιση παραμέτρων της υπηρεσίας, επιλέξτε τη νέα ρύθμιση παραμέτρων της υπηρεσίας από τη λίστα **όνομα** και, στη συνέχεια, επιλέξτε **Μετονομασία**. Στο πλαίσιο κειμένου **όνομα** , πληκτρολογήστε το όνομα που θέλετε να χρησιμοποιήσετε αυτήν τη ρύθμιση παραμέτρων υπηρεσιών και, στη συνέχεια, επιλέξτε **OK**.

    Έχει αλλάξει το όνομα του αρχείου ρύθμισης παραμέτρων υπηρεσίας του Azure έργου στην Εξερεύνηση λύσεων.

### <a name="to-change-a-service-configuration"></a>Για να αλλάξετε μια ρύθμιση παραμέτρων της υπηρεσίας

- Εάν θέλετε να αλλάξετε μια ρύθμιση παραμέτρων της υπηρεσίας, ανοίξτε το μενού συντόμευσης για το συγκεκριμένο ρόλο που θέλετε να αλλάξετε το Azure έργου και, στη συνέχεια, επιλέξτε την εντολή **Ιδιότητες**. Ανατρέξτε στο θέμα [ΔΙΑΔΙΚΑΣΙΕΣ: ρύθμιση παραμέτρων τους ρόλους για μια υπηρεσία Cloud Azure με το Visual Studio](https://msdn.microsoft.com/library/azure/hh369931.aspx) για περισσότερες πληροφορίες.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Κάντε τους συνδυασμούς διαφορετική ρύθμιση χρησιμοποιώντας προφίλ

Χρησιμοποιώντας ένα προφίλ, μπορείτε να συμπληρώσετε αυτόματα στον **"Οδηγό δημοσίευσης"** με διαφορετικούς συνδυασμούς των ρυθμίσεων για διαφορετικούς λόγους. Για παράδειγμα, μπορείτε να έχετε ένα προφίλ για τον εντοπισμό σφαλμάτων και δημιουργεί ένα άλλο για την έκδοση. Σε αυτή την περίπτωση, το προφίλ σας **Εντοπισμός σφαλμάτων** να έχουν **IntelliTrace** με δυνατότητα και τη ρύθμιση παραμέτρων **Εντοπισμός σφαλμάτων** επιλεγμένο και το προφίλ σας **κυκλοφορίας** να έχουν **IntelliTrace** απενεργοποιημένη και τη ρύθμιση παραμέτρων **έκδοσης** επιλεγμένο. Μπορείτε επίσης να χρησιμοποιήσετε διαφορετικό προφίλ για να αναπτύξετε μια υπηρεσία χρησιμοποιώντας ένα λογαριασμό διαφορετικό χώρο αποθήκευσης.

Κατά την εκτέλεση του οδηγού για πρώτη φορά, δημιουργείται ένα προεπιλεγμένο προφίλ. Visual Studio αποθηκεύει το προφίλ σε ένα αρχείο που έχει μια επέκταση .azurePubXml, η οποία προστίθεται στο έργο σας Azure κάτω από το φάκελο του **προφίλ** . Εάν καθορίσετε με μη αυτόματο τρόπο διαφορετικές επιλογές κατά την εκτέλεση του Οδηγού αργότερα, ενημερώνει αυτόματα το αρχείο. Πριν να εκτελέσετε την ακόλουθη διαδικασία, πρέπει να έχετε ήδη δημοσιεύσει την υπηρεσία cloud τουλάχιστον μία φορά.

### <a name="to-add-a-profile"></a>Για να προσθέσετε ένα προφίλ

1. Άνοιγμα του μενού συντόμευσης για το έργο σας Azure και, στη συνέχεια, επιλέξτε **Δημοσίευση**.

1. Δίπλα στη λίστα **προορισμού προφίλ** , επιλέξτε το κουμπί **Αποθήκευση προφίλ** , όπως φαίνεται στο παρακάτω εικόνα. Αυτό δημιουργεί ένα προφίλ για εσάς.

    ![Δημιουργήστε ένα νέο προφίλ](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)

1. Αφού δημιουργηθεί το προφίλ, επιλέξτε **<... Διαχείριση >** στη λίστα **προορισμού προφίλ** .

    Εμφανίζεται το παράθυρο διαλόγου **Διαχείριση προφίλ** , όπως φαίνεται στο παρακάτω εικόνα.

    ![Διαχείριση προφίλ παραθύρου διαλόγου](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)

1. Στη λίστα **όνομα** , επιλέξτε ένα προφίλ και, στη συνέχεια, επιλέξτε **Δημιουργία αντιγράφου**.

1. Επιλέξτε το κουμπί " **Κλείσιμο** ".

    Το νέο προφίλ εμφανίζεται στη λίστα προορισμού προφίλ.

1. Στη λίστα **προορισμού προφίλ** , επιλέξτε το προφίλ που μόλις δημιουργήσατε. Ρυθμίσεις του οδηγού δημοσίευση συμπληρώνονται με τις επιλογές από το προφίλ που επιλέξατε.

1. Επιλέξτε τα κουμπιά **Προηγούμενο** και **Επόμενο** για να εμφανίσετε κάθε σελίδα του οδηγού δημοσίευση και, στη συνέχεια, να προσαρμόσετε τις ρυθμίσεις για αυτό το προφίλ. Για πληροφορίες, ανατρέξτε στο θέμα [Οδηγός δημοσίευση εφαρμογών Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Αφού ολοκληρώσετε την προσαρμογή των ρυθμίσεων, επιλέξτε **Επόμενο** για να επιστρέψετε στη σελίδα ρυθμίσεων. Στο προφίλ που αποθηκεύεται κατά τη δημοσίευση της υπηρεσίας, χρησιμοποιώντας αυτές τις ρυθμίσεις ή εάν επιλέξετε **Αποθήκευση** δίπλα στη λίστα των προφίλ.

### <a name="to-rename-or-delete-a-profile"></a>Για να μετονομάσετε ή να διαγράψετε ένα προφίλ

1. Άνοιγμα του μενού συντόμευσης για το έργο σας Azure και, στη συνέχεια, επιλέξτε **Δημοσίευση**.

1. Στη λίστα **προορισμού προφίλ** , επιλέξτε **Διαχείριση**.

1. Στο παράθυρο διαλόγου **Διαχείριση προφίλ** , επιλέξτε το προφίλ που θέλετε να διαγράψετε και, στη συνέχεια, επιλέξτε **Κατάργηση**.

1. Στο παράθυρο διαλόγου επιβεβαίωσης που εμφανίζεται, επιλέξτε **OK**.

1. Επιλέξτε **Κλείσιμο**.

### <a name="to-change-a-profile"></a>Για να αλλάξετε ένα προφίλ

1. Άνοιγμα του μενού συντόμευσης για το έργο σας Azure και, στη συνέχεια, επιλέξτε **Δημοσίευση**.

1. Στη λίστα **προορισμού προφίλ** , επιλέξτε το προφίλ που θέλετε να αλλάξετε.

1. Επιλέξτε τα κουμπιά **Προηγούμενο** και **Επόμενο** για να εμφανίσετε κάθε σελίδα του οδηγού δημοσίευση και, στη συνέχεια, αλλάξτε τις ρυθμίσεις που θέλετε. Για πληροφορίες, ανατρέξτε στο θέμα [Οδηγός δημοσίευση εφαρμογών Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Αφού ολοκληρώσετε την αλλαγή των ρυθμίσεων, επιλέξτε **Επόμενο** για να επιστρέψετε στη σελίδα **ρυθμίσεων** .

1. (Προαιρετικό) επιλέξτε **Δημοσίευση** για να δημοσιεύσετε την υπηρεσία cloud, χρησιμοποιώντας τις νέες ρυθμίσεις. Εάν δεν θέλετε να δημοσιεύσετε την υπηρεσία cloud αυτήν τη στιγμή και κλείσετε τον Οδηγό δημοσίευσης, Visual Studio σας ρωτά εάν θέλετε να αποθηκεύσετε τις αλλαγές στο προφίλ.

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με τη ρύθμιση των παραμέτρων άλλα τμήματα του έργου σας Azure από το Visual Studio, ανατρέξτε στο θέμα [ρύθμιση των παραμέτρων ενός έργου Azure](http://go.microsoft.com/fwlink/p/?LinkID=623075)