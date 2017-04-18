<properties 
    pageTitle="Χρήση του PowerShell για να δημιουργήσετε μια Εικονική με ένα διακομιστή αναφοράς εγγενή λειτουργία | Microsoft Azure"
    description="Αυτό το θέμα περιγράφει και σας καθοδηγεί το ανάπτυξης και ρύθμισης παραμέτρων διακομιστή αναφορών εγγενή λειτουργία των υπηρεσιών αναφοράς του SQL Server σε ένα Azure εικονική μηχανή. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management"/>
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>Χρήση του PowerShell για να δημιουργήσετε μια Εικονική Azure με ένα διακομιστή αναφοράς εγγενή λειτουργία

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Αυτό το θέμα περιγράφει και σας καθοδηγεί το ανάπτυξης και ρύθμισης παραμέτρων διακομιστή αναφορών εγγενή λειτουργία των υπηρεσιών αναφοράς του SQL Server σε ένα Azure εικονική μηχανή. Τα βήματα σε αυτό το έγγραφο χρησιμοποιήσετε ένα συνδυασμό των μη αυτόματα βήματα για να δημιουργήσετε την εικονική μηχανή και μια δέσμη ενεργειών του Windows PowerShell για τη ρύθμιση παραμέτρων υπηρεσιών αναφοράς στην η Εικονική. Η δέσμη ενεργειών ρύθμισης παραμέτρων περιλαμβάνει το άνοιγμα μιας θύρας τείχους προστασίας για HTTP ή HTTPs.

>[AZURE.NOTE] Εάν δεν χρειάζεστε **HTTPS** στο διακομιστή αναφοράς, **παραλείψετε το βήμα 2**.
>
>Αφού δημιουργήσετε την εικονική Μηχανή στο βήμα 1, μεταβείτε στην ενότητα Χρήση δέσμης ενεργειών για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς και HTTP. Αφού εκτελέσετε τη δέσμη ενεργειών, το διακομιστή αναφορών είναι έτοιμη για χρήση.

## <a name="prerequisites-and-assumptions"></a>Προαπαιτούμενα και υποθέσεις

- **Azure συνδρομή**: Επαληθεύστε τον αριθμό πυρήνων που είναι διαθέσιμες στη συνδρομή σας Azure. Εάν δημιουργήσετε το προτεινόμενο μέγεθος Εικονική **A3**, χρειάζεστε διαθέσιμα πυρήνων **4** . Εάν χρησιμοποιείτε μια Εικονική μέγεθος του **κελιού A2**, χρειάζεστε διαθέσιμα πυρήνων **2** .
    
    - Για να επαληθεύσετε το όριο πυρήνα της συνδρομής σας, στην κλασική Azure πύλη, κάντε κλικ στην επιλογή ΡΥΘΜΊΣΕΙΣ στο αριστερό τμήμα του παραθύρου και, στη συνέχεια, κάντε κλικ στην επιλογή ΧΡΉΣΗ στο επάνω μενού.
    
    - Για να αυξήσετε το μέγεθος των κύριων ορίου, επικοινωνήστε με την [Υποστήριξη Azure](https://azure.microsoft.com/support/options/). Για πληροφορίες μέγεθος Εικονική, δείτε [Μεγέθη εικονική μηχανή για Azure](virtual-machines-linux-sizes.md).

- **Δέσμες ενεργειών για το Windows PowerShell**: το θέμα προϋποθέτει ότι έχετε κάποια βασικά εμπειρία του Windows PowerShell. Για περισσότερες πληροφορίες σχετικά με τη χρήση του Windows PowerShell, ανατρέξτε στα παρακάτω:

    - [Εκκίνηση του Windows PowerShell σε Windows Server](https://technet.microsoft.com/library/hh847814.aspx)
    
    - [Γρήγορα αποτελέσματα με το Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Βήμα 1: Προετοιμασία μια εικονική μηχανή Azure

1. Μεταβείτε στην πύλη του Azure κλασική.

1. Στο αριστερό παράθυρο, κάντε κλικ στην επιλογή **εικονικές μηχανές** .

    ![Microsoft azure εικονικές μηχανές](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)

1. Κάντε κλικ στην επιλογή **νέα**.

    ![κουμπί "Δημιουργία"](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)

1. Κάντε κλικ στην επιλογή **από τη συλλογή**.

    ![Νέα εικονική από συλλογή](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)

1. Κάντε κλικ στην επιλογή **SQL Server 2014 RTM Standard – Windows Server 2012 R2** και, στη συνέχεια, κάντε κλικ στο βέλος για να συνεχίσετε.

    ![Επόμενο](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

    Εάν χρειάζεστε τα δεδομένα αναφοράς υπηρεσιών βάσει δυνατότητα συνδρομές, επιλέξτε **SQL Server 2014 RTM επιχείρηση – Windows Server 2012 R2**. Για περισσότερες πληροφορίες σχετικά με εκδόσεις του SQL Server και υποστήριξη δυνατοτήτων, ανατρέξτε στο θέμα [Δυνατότητες που υποστηρίζονται από το εκδόσεις του SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).

1. Στη σελίδα **Ρύθμιση παραμέτρων εικονική μηχανή** , επεξεργαστείτε τα ακόλουθα πεδία:
                                    
    - Εάν υπάρχουν περισσότερες από μία **ΗΜΕΡΟΜΗΝΊΑ ΈΚΔΟΣΗΣ ΔΗΜΟΣΊΕΥΣΗΣ**, επιλέξτε την πιο πρόσφατη έκδοση.
    
    - **Εικονική μηχανή όνομα**: το όνομα του υπολογιστή χρησιμοποιείται επίσης στην επόμενη σελίδα ρύθμισης παραμέτρων ως το προεπιλεγμένο όνομα DNS υπηρεσία Cloud. Το όνομα DNS πρέπει να είναι μοναδικό κατά μήκος της υπηρεσίας Azure. Εξετάστε το ενδεχόμενο τη ρύθμιση των παραμέτρων του Εικονική με ένα όνομα υπολογιστή που περιγράφει τι χρησιμεύει η Εικονική. Για παράδειγμα ssrsnativecloud.
    
    - **Επίπεδο**: τυπική
    
    - **Μέγεθος: A3** είναι το προτεινόμενο μέγεθος Εικονική για φόρτους εργασίας του SQL Server. Εάν μια Εικονική χρησιμοποιείται μόνο με ένα διακομιστή αναφοράς, μια Εικονική μέγεθος του κελιού A2 είναι αρκετό, εκτός εάν ο διακομιστής αναφοράς αντιμετωπίζει ένα μεγάλο φόρτο εργασίας. Για Εικονική πληροφορίες για τις τιμές, ανατρέξτε στο θέμα [Εικονικές μηχανές τις πληροφορίες τιμολόγησης](https://azure.microsoft.com/pricing/details/virtual-machines/).
    
    - **Νέο όνομα χρήστη**: το όνομα που παρέχετε δημιουργείται ως διαχειριστής στον η Εικονική.
    
    - **Νέο κωδικό πρόσβασης** και **Επιβεβαιώστε**. Αυτός ο κωδικός πρόσβασης χρησιμοποιείται για το νέο λογαριασμό διαχειριστή και συνιστάται να χρησιμοποιείτε έναν ισχυρό κωδικό πρόσβασης.
    
    - Κάντε κλικ στο κουμπί **Επόμενο**. ![Επόμενο](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Στην επόμενη σελίδα, επεξεργαστείτε τα ακόλουθα πεδία:

    - **Υπηρεσία cloud**: Επιλέξτε **Δημιουργία μια νέα υπηρεσία στο Cloud**.
    
    - **Όνομα DNS υπηρεσία cloud**: αυτό είναι το δημόσιο όνομα DNS της υπηρεσίας Cloud που είναι συσχετισμένο με την εικονική Μηχανή. Το προεπιλεγμένο όνομα είναι το όνομα που πληκτρολογήσατε στο για το όνομα Εικονική. Εάν στα επόμενα βήματα του θέματος, μπορείτε να δημιουργήσετε ένα αξιόπιστο πιστοποιητικό SSL και, στη συνέχεια, χρησιμοποιείται το όνομα του DNS για την τιμή από την "**κάτοχος**" του πιστοποιητικού.
    
    - **Περιοχή/συνάφειας/εικονική ομάδα δικτύου**: Επιλέξτε την περιοχή πλησιέστερη στους τελικούς χρήστες σας.
    
    - **Το λογαριασμό χώρου αποθήκευσης**: χρήση ενός λογαριασμού χώρου αποθήκευσης που δημιουργούνται αυτόματα.
    
    - **Ορισμός διαθεσιμότητας**: καμία.
    
    - **Τα τελικά ΣΗΜΕΊΑ** Διατήρηση τα τελικά σημεία **Απομακρυσμένης επιφάνειας εργασίας** και **του PowerShell** και στη συνέχεια, προσθέστε ένα HTTP ή HTTPS το τελικό σημείο, ανάλογα με το περιβάλλον.

        - **HTTP**: οι προεπιλεγμένες θύρες δημόσια και ιδιωτικά είναι **80**. Σημειώστε ότι αν χρησιμοποιείτε μια ιδιωτική θύρα εκτός από 80, τροποποίηση **$HTTPport = 80** στη δέσμη ενεργειών http.

        - **HTTPS**: **443**είναι οι προεπιλεγμένες θύρες δημόσια και ιδιωτικά. Ασφάλεια βέλτιστη πρακτική είναι να αλλάξετε τη θύρα ιδιωτικό και ρύθμιση παραμέτρων του τείχους προστασίας και διακομιστή αναφοράς για να χρησιμοποιήσετε τη θύρα ιδιωτικό. Για περισσότερες πληροφορίες σχετικά με τα τελικά σημεία, δείτε [πώς μπορείτε να ορίσετε επικοινωνίας με μια εικονική μηχανή](virtual-machines-windows-classic-setup-endpoints.md). Σημειώστε ότι αν χρησιμοποιείτε μια θύρα εκτός από 443, αλλάξτε την παράμετρο **$HTTPsport = 443** στη δέσμη ενεργειών HTTPS.
    
    - Κάντε κλικ στο κουμπί Επόμενο. ![Επόμενο](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Στην τελευταία σελίδα του οδηγού, διατηρήστε την προεπιλεγμένη **εγκαταστήσετε τον παράγοντα Εικονική** επιλεγμένο. Τα βήματα σε αυτό το θέμα, δεν χρησιμοποιούν τον παράγοντα Εικονική αλλά εάν σκοπεύετε να διατηρήσετε Εικονική αυτό, η Εικονική παράγοντας και επεκτάσεις θα σας επιτρέψει να εμπλουτισμός αυτός Εκατοστών.  Για περισσότερες πληροφορίες σχετικά με τον παράγοντα Εικονική, ανατρέξτε στο θέμα [παράγοντας Εικονική και επεκτάσεις-μέρος 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Ένα από το ad επεκτάσεις εγκατεστημένο προεπιλεγμένο εκτελείται είναι η επέκταση "BGINFO" που εμφανίζεται στην επιφάνεια εργασίας Εικονική, πληροφορίες συστήματος, όπως εσωτερικές IP και χώρο στη μονάδα δίσκου δωρεάν.

1. Κάντε κλικ στην επιλογή ολοκλήρωση. ![Ok](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)

1. Την **κατάσταση** της η Εικονική εμφανίζει ως **Έναρξη (Provisioning)** κατά τη διαδικασία διάταξη και, στη συνέχεια, εμφανίζει ως **εκτελείται** όταν η Εικονική προμήθεια του φακέλου και είστε έτοιμοι να χρησιμοποιήσετε.

## <a name="step-2-create-a-server-certificate"></a>Βήμα 2: Δημιουργήστε ένα πιστοποιητικό διακομιστή

>[AZURE.NOTE] Εάν δεν χρειάζεστε HTTPS στο διακομιστή αναφοράς, μπορείτε να **παραλείψετε το βήμα 2** και μεταβείτε στην ενότητα **Χρήση δέσμης ενεργειών για να ρυθμίσετε το διακομιστή αναφοράς και HTTP**. Χρησιμοποιήστε τη δέσμη ενεργειών HTTP για να ρυθμίσετε γρήγορα το διακομιστή αναφορών και θα είναι έτοιμη για χρήση στο διακομιστή αναφορών.

Για να χρησιμοποιήσετε HTTPS η Εικονική, χρειάζεστε ένα αξιόπιστο πιστοποιητικό SSL. Ανάλογα με το σενάριο, μπορείτε να χρησιμοποιήσετε μία από τις ακόλουθες δύο μεθόδους:

- Ένα έγκυρο πιστοποιητικό SSL εκδίδεται από μια αρχή έκδοσης πιστοποιητικών (CA) και αξιόπιστες από τη Microsoft. Τα πιστοποιητικά αρχή έκδοσης Πιστοποιητικών ρίζας που απαιτούνται για να διανεμηθούν μέσω πρόγραμμα πιστοποιητικών ρίζας της Microsoft. Για περισσότερες πληροφορίες σχετικά με αυτό το πρόγραμμα, ανατρέξτε στο θέμα [των Windows και Windows Phone 8 SSL πρόγραμμα πιστοποιητικών ρίζας (CA μέλος)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) και [Εισαγωγή στο πρόγραμμα πιστοποιητικών ρίζας της Microsoft](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).

- Ένα αυτο-υπογεγραμμένο πιστοποιητικό. Δεν συνιστάται η πιστοποιητικά αυτόματης υπογραφής για περιβάλλοντα παραγωγής.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Για να χρησιμοποιήσετε ένα πιστοποιητικό που δημιουργήθηκε από μια αξιόπιστη αρχή έκδοσης πιστοποιητικών (CA)

1. **Αίτηση για ένα πιστοποιητικό διακομιστή για την τοποθεσία Web από μια αρχή έκδοσης πιστοποιητικών**. 

    Μπορείτε να χρησιμοποιήσετε τον Οδηγό πιστοποιητικού διακομιστή Web, είτε για τη δημιουργία ενός αρχείου αίτησης πιστοποιητικού (Certreq.txt) που στέλνετε σε μια αρχή έκδοσης πιστοποιητικών ή για να δημιουργήσετε μια αίτηση για μια αρχή έκδοσης πιστοποιητικών. Για παράδειγμα, υπηρεσίες πιστοποιητικών της Microsoft στο Windows Server 2012. Ανάλογα με το επίπεδο εξασφάλισης αναγνώρισης που παρέχεται από το πιστοποιητικό διακομιστή, είναι αρκετές ημέρες για πολλούς μήνες για την αρχή έκδοσης πιστοποιητικών για να εγκρίνετε την αίτησή σας και να σας στείλει ένα αρχείο πιστοποιητικού. 

    Για περισσότερες πληροφορίες σχετικά με την αίτηση ένα πιστοποιητικά διακομιστή, ανατρέξτε στα παρακάτω: 
    
    - Χρησιμοποιήστε το [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
    
    - Εργαλεία ασφάλειας για τη διαχείριση των Windows Server 2012.

    [Εργαλεία ασφαλείας για τη διαχείριση του Windows Server 2012](https://technet.microsoft.com/library/jj730960.aspx)

    >[AZURE.NOTE] Το πεδίο **που εκδίδονται σε** του αξιόπιστου πιστοποιητικού SSL πρέπει να είναι το ίδιο με το **ΌΝΟΜΑ DNS υπηρεσία Cloud** που χρησιμοποιείται για τη νέα Εικονική.

1. **Εγκαταστήστε το πιστοποιητικό διακομιστή στο διακομιστή Web**. Σε αυτήν την περίπτωση που ο διακομιστής Web είναι η Εικονική που φιλοξενεί το διακομιστή αναφορών και την τοποθεσία Web δημιουργείται στα επόμενα βήματα κατά τη ρύθμιση παραμέτρων υπηρεσιών αναφοράς. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση του πιστοποιητικού διακομιστή στο διακομιστή Web με χρήση του συμπληρωματικού προγράμματος πιστοποιητικού MMC, ανατρέξτε στο θέμα [Εγκατάσταση πιστοποιητικού διακομιστή](https://technet.microsoft.com/library/cc740068).
    
    Εάν θέλετε να χρησιμοποιήσετε τη δέσμη ενεργειών που περιλαμβάνεται με αυτό το θέμα, για να ρυθμίσετε το διακομιστή αναφορών, η τιμή του τα πιστοποιητικά **αποτύπωση** απαιτείται ως παράμετρο της δέσμης ενεργειών. Ανατρέξτε στην επόμενη ενότητα για λεπτομέρειες σχετικά με τον τρόπο για να αποκτήσετε την αποτύπωση του πιστοποιητικού.

1. Εκχωρήστε το πιστοποιητικό διακομιστή στο διακομιστή αναφορών. Η ανάθεση έχει ολοκληρωθεί στην επόμενη ενότητα όταν ρυθμίζετε τις παραμέτρους του διακομιστή αναφοράς.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Για να χρησιμοποιήσετε τις εικονικές μηχανές αυτο-υπογεγραμμένο πιστοποιητικό

Ένα αυτο-υπογεγραμμένο πιστοποιητικό που δημιουργήθηκε σε η Εικονική όταν παρέχεται η Εικονική. Το πιστοποιητικό έχει το ίδιο όνομα με το όνομα Εικονική DNS. Για να αποφύγετε σφάλματα πιστοποιητικού, απαιτείται ότι το πιστοποιητικό είναι αξιόπιστο το Εικονική ίδια και έπειτα από όλους τους χρήστες της τοποθεσίας.

1. Για να θεωρήσετε αξιόπιστη αρχή έκδοσης Πιστοποιητικών ρίζας του πιστοποιητικού στην την τοπική εικονική Μηχανή, προσθέστε το πιστοποιητικό στις **Αξιόπιστες αρχές έκδοσης πιστοποιητικών ρίζας**. Ακολουθεί μια σύνοψη των τα βήματα που απαιτούνται. Για λεπτομερείς οδηγίες σχετικά με τον τρόπο να θεωρήσετε αξιόπιστη την αρχή έκδοσης Πιστοποιητικών, ανατρέξτε στο θέμα [Εγκατάσταση πιστοποιητικού διακομιστή](https://technet.microsoft.com/library/cc740068).

    1. Από την πύλη του Azure κλασική, επιλέξτε την εικονική Μηχανή και κάντε κλικ στην επιλογή σύνδεση. Ανάλογα με τη ρύθμιση παραμέτρων του προγράμματος περιήγησης, ίσως σας ζητηθεί να αποθηκεύσετε ένα αρχείο .rdp για τη σύνδεση με την εικονική Μηχανή.
    
        ![σύνδεση με azure εικονική μηχανή](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Χρησιμοποιήστε το όνομα χρήστη Εικονική, όνομα χρήστη και τον κωδικό πρόσβασης που έχετε ρυθμίσει τις παραμέτρους όταν δημιουργήσατε την εικονική Μηχανή. 
    
        Για παράδειγμα, στην παρακάτω εικόνα, το όνομα του Εικονική είναι **ssrsnativecloud** και το όνομα χρήστη είναι **testuser**.
        
        ![όνομα εικονική inlcudes σύνδεσης](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
    
    1. Εκτελέστε mmc.exe. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος: Προβολή πιστοποιητικών με το συμπληρωματικό](https://msdn.microsoft.com/library/ms788967.aspx).
    
    1. Στο μενού **αρχείο** κονσόλας εφαρμογής, προσθήκη του συμπληρωματικού προγράμματος **πιστοποιητικών** , επιλέξτε **Το λογαριασμό υπολογιστή** όταν σας ζητηθεί και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
    
    1. Επιλέξτε **Τοπικό υπολογιστή** για τη διαχείριση και, στη συνέχεια, κάντε κλικ στο κουμπί **Τέλος**.
    
    1. Κάντε κλικ στο κουμπί **Ok** και, στη συνέχεια, αναπτύξτε το **πιστοποιητικά - προσωπικών** κόμβους και, στη συνέχεια, κάντε κλικ στην επιλογή **πιστοποιητικά**. Το πιστοποιητικό παίρνει το όνομα DNS του η Εικονική και τελειώνει με **cloudapp.net**. Κάντε δεξί κλικ στο όνομα πιστοποιητικό και κάντε κλικ στην επιλογή **Αντιγραφή**.
    
    1. Αναπτύξτε τον κόμβο **Αξιόπιστες αρχές έκδοσης πιστοποιητικών ρίζας** και, στη συνέχεια, κάντε δεξί κλικ **πιστοποιητικά** και, στη συνέχεια, κάντε κλικ στην επιλογή **Επικόλληση**.
    
    1. Για να επικυρώσετε, κάντε διπλό κλικ στο όνομα του πιστοποιητικού στην περιοχή **Αξιόπιστες αρχές έκδοσης πιστοποιητικών ρίζας** και βεβαιωθείτε ότι δεν υπάρχουν σφάλματα και δείτε πιστοποιητικού. Εάν θέλετε να χρησιμοποιήσετε τη δέσμη ενεργειών HTTPS περιλαμβάνεται με αυτό το θέμα, για να ρυθμίσετε το διακομιστή αναφορών, η τιμή του τα πιστοποιητικά **αποτύπωση** απαιτείται ως παράμετρο της δέσμης ενεργειών. **Για να λάβετε την τιμή αποτύπωση**, ολοκληρώστε τα ακόλουθα. Υπάρχει επίσης ένα δείγμα του PowerShell για να ανακτήσετε την αποτύπωση στην ενότητα [Χρήση δέσμης ενεργειών για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς και HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).
        
        1. Κάντε διπλό κλικ στο όνομα του πιστοποιητικού, για παράδειγμα ssrsnativecloud.cloudapp.net.
        
        1. Κάντε κλικ στην καρτέλα " **Λεπτομέρειες** ".
        
        1. Κάντε κλικ στην επιλογή **αποτύπωση**. Η τιμή του την αποτύπωση εμφανίζεται στο πεδίο λεπτομερειών, για παράδειγμα a6 08 3c df f9 0b f7 e3 7c 25 Επεξεργασία a4 2f fb 9c 2c ac 91 7e επεξεργασία.
        
        1. Αντιγράψτε την αποτύπωση και αποθηκεύστε την τιμή για αργότερα ή επεξεργασία της δέσμης ενεργειών τώρα.
        
        1. (*) Πριν να εκτελέσετε τη δέσμη ενεργειών, καταργήστε τα διαστήματα ανάμεσα στα τα ζεύγη τιμών. Για παράδειγμα, την αποτύπωση σημειωθεί πριν από την τώρα θα ήταν a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.
        
        1. Εκχωρήστε το πιστοποιητικό διακομιστή στο διακομιστή αναφορών. Η ανάθεση έχει ολοκληρωθεί στην επόμενη ενότητα όταν ρυθμίζετε τις παραμέτρους του διακομιστή αναφοράς.

Εάν χρησιμοποιείτε ένα αυτο-υπογεγραμμένο πιστοποιητικό SSL, το όνομα του πιστοποιητικού συμφωνεί ήδη με το όνομα κεντρικού υπολογιστή του την εικονική Μηχανή. Επομένως, το DNS του υπολογιστή έχει ήδη καταχωρηθεί καθολικά και είναι δυνατή η πρόσβαση από οποιοδήποτε πρόγραμμα-πελάτη.

## <a name="step-3-configure-the-report-server"></a>Βήμα 3: Ρύθμιση παραμέτρων του διακομιστή αναφοράς

Αυτή η ενότητα σάς καθοδηγεί σε ρύθμιση παραμέτρων του Εικονική ως ένα διακομιστή αναφοράς εγγενή λειτουργία των υπηρεσιών αναφοράς. Μπορείτε να χρησιμοποιήσετε μία από τις παρακάτω μεθόδους για να ρυθμίσετε το διακομιστή αναφορών:

- Χρησιμοποιήστε τη δέσμη ενεργειών για να ρυθμίσετε το διακομιστή αναφορών

- Χρησιμοποιήστε τη Διαχείριση ρύθμισης παραμέτρων για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς.

Για πιο λεπτομερείς οδηγίες, ανατρέξτε στην ενότητα [σύνδεση με την εικονική μηχανή και να ξεκινήσετε τη Διαχείριση ομάδας παραμέτρων υπηρεσιών αναφοράς](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Ελέγχου ταυτότητας Σημείωση:** Έλεγχος ταυτότητας των Windows είναι της μεθόδου ελέγχου ταυτότητας συνιστάται και είναι το προεπιλεγμένο έλεγχο ταυτότητας υπηρεσιών αναφοράς. Μόνο οι χρήστες που έχουν ρυθμιστεί στο η Εικονική να αποκτήσετε πρόσβαση σε υπηρεσίες αναφοράς και που έχουν εκχωρηθεί σε ρόλους των υπηρεσιών αναφοράς.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Χρήση δέσμης ενεργειών για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς και HTTP

Για να χρησιμοποιήσετε τη δέσμη ενεργειών του Windows PowerShell για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς, ολοκληρώστε τα ακόλουθα βήματα. Η ρύθμιση παραμέτρων περιλαμβάνει HTTP, δεν HTTPS:

1. Από την πύλη του Azure κλασική, επιλέξτε την εικονική Μηχανή και κάντε κλικ στην επιλογή σύνδεση. Ανάλογα με τη ρύθμιση παραμέτρων του προγράμματος περιήγησης, ίσως σας ζητηθεί να αποθηκεύσετε ένα αρχείο .rdp για τη σύνδεση με την εικονική Μηχανή.

    ![σύνδεση με azure εικονική μηχανή](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Χρησιμοποιήστε το όνομα χρήστη Εικονική, όνομα χρήστη και τον κωδικό πρόσβασης που έχετε ρυθμίσει τις παραμέτρους όταν δημιουργήσατε την εικονική Μηχανή. 

    Για παράδειγμα, στην παρακάτω εικόνα, το όνομα του Εικονική είναι **ssrsnativecloud** και το όνομα χρήστη είναι **testuser**.
    
    ![όνομα εικονική inlcudes σύνδεσης](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Στην την εικονική Μηχανή, ανοίξτε το **Windows PowerShell ISE** με δικαιώματα διαχειριστή. Το PowerShell ISE είναι εγκατεστημένο από προεπιλογή σε Windows server 2012. Συνιστάται να χρησιμοποιείτε το ISE αντί για ένα τυπικό παράθυρο του Windows PowerShell, έτσι ώστε να μπορείτε να επικολλήσετε τη δέσμη ενεργειών σε το ISE, τροποποιήστε τη δέσμη ενεργειών, και, στη συνέχεια, εκτελέστε τη δέσμη ενεργειών.

1. Στο Windows PowerShell ISE, κάντε κλικ στο μενού **Προβολή** και, στη συνέχεια, κάντε κλικ στην επιλογή **Εμφάνιση παραθύρου δέσμης ενεργειών**.

1. Αντιγράψτε την ακόλουθη δέσμη ενεργειών και επικολλήστε τη δέσμη ενεργειών στο τμήμα παραθύρου δέσμης ενεργειών των Windows PowerShell ISE.

        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
        
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
        
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Report Server Configuration Steps
        
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
           
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
        
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Εάν έχετε δημιουργήσει την εικονική Μηχανή με θύρα HTTP εκτός από 80, να τροποποιήσετε την παράμετρο $HTTPport = 80.

1. Η δέσμη ενεργειών έχει ρυθμιστεί για τις υπηρεσίες αναφοράς. Εάν θέλετε να εκτελέσετε τη δέσμη ενεργειών για τις υπηρεσίες αναφοράς, τροποποιήστε το τμήμα έκδοση τη διαδρομή προς το χώρο ονομάτων για να V11 ":", στην κατάσταση Get-WmiObject.

1. Εκτελέστε τη δέσμη ενεργειών.

**Επικύρωση**: για να επαληθεύσετε ότι λειτουργεί η λειτουργικότητα διακομιστή βασικής αναφοράς, ανατρέξτε στην ενότητα [Επαλήθευση της ρύθμισης παραμέτρων](#verify-the-configuration) αργότερα σε αυτό το θέμα.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Χρήση δέσμης ενεργειών για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς και HTTPS

Για να χρησιμοποιήσετε το Windows PowerShell για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς, ολοκληρώστε τα ακόλουθα βήματα. Οι ρυθμίσεις περιλαμβάνουν HTTPS, δεν HTTP.

1. Από την πύλη του Azure κλασική, επιλέξτε την εικονική Μηχανή και κάντε κλικ στην επιλογή σύνδεση. Ανάλογα με τη ρύθμιση παραμέτρων του προγράμματος περιήγησης, ίσως σας ζητηθεί να αποθηκεύσετε ένα αρχείο .rdp για τη σύνδεση με την εικονική Μηχανή.

    ![σύνδεση με azure εικονική μηχανή](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Χρησιμοποιήστε το όνομα χρήστη Εικονική, όνομα χρήστη και τον κωδικό πρόσβασης που έχετε ρυθμίσει τις παραμέτρους όταν δημιουργήσατε την εικονική Μηχανή. 

    Για παράδειγμα, στην παρακάτω εικόνα, το όνομα του Εικονική είναι **ssrsnativecloud** και το όνομα χρήστη είναι **testuser**.

    ![όνομα εικονική inlcudes σύνδεσης](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Στην την εικονική Μηχανή, ανοίξτε το **Windows PowerShell ISE** με δικαιώματα διαχειριστή. Το PowerShell ISE είναι εγκατεστημένο από προεπιλογή σε Windows server 2012. Συνιστάται να χρησιμοποιείτε το ISE αντί για ένα τυπικό παράθυρο του Windows PowerShell, έτσι ώστε να μπορείτε να επικολλήσετε τη δέσμη ενεργειών σε το ISE, τροποποιήστε τη δέσμη ενεργειών, και, στη συνέχεια, εκτελέστε τη δέσμη ενεργειών.

1. Για να ενεργοποιήσετε την εκτέλεση δεσμών ενεργειών, εκτελέστε την ακόλουθη εντολή του Windows PowerShell:

        Set-ExecutionPolicy RemoteSigned

    Στη συνέχεια, μπορείτε να εκτελέσετε τα παρακάτω για να επιβεβαιώσετε την πολιτική:

        Get-ExecutionPolicy

1. Στο **Windows PowerShell ISE**, κάντε κλικ στο μενού **Προβολή** και, στη συνέχεια, κάντε κλικ στην επιλογή **Εμφάνιση παραθύρου δέσμης ενεργειών**.

1. Αντιγράψτε την ακόλουθη δέσμη ενεργειών και επικολλήστε το στο παράθυρο δέσμης ενεργειών των Windows PowerShell ISE.

        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
        
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
        
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Reporting Services Report Server Configuration Steps
        
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
        
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
            
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## 3. Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
        
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
        
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Τροποποιήστε την παράμετρο **$certificatehash** στη δέσμη ενεργειών:

    - Αυτή είναι μια **απαιτούμενη** παράμετρος. Εάν δεν έχετε αποθηκεύσει την τιμή πιστοποιητικού από τα προηγούμενα βήματα, χρησιμοποιήστε μία από τις παρακάτω μεθόδους για να αντιγράψετε την τιμή Κατακερματισμός πιστοποιητικού από την αποτύπωση πιστοποιητικά.:

        Στην την εικονική Μηχανή, ανοίξτε το Windows PowerShell ISE και εκτελέστε την ακόλουθη εντολή:

            dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer

        Το αποτέλεσμα θα είναι παρόμοιο με το εξής. Εάν η δέσμη ενεργειών επιστρέφει μια κενή γραμμή, η Εικονική δεν διαθέτει ένα πιστοποιητικό που έχει ρυθμιστεί για παράδειγμα, ανατρέξτε στην ενότητα [για να χρησιμοποιήσετε το πιστοποιητικό Self-signed εικονικές μηχανές](#to-use-the-virtual-machines-self-signed-certificate).
    
    OR
    
    - Στην η Εικονική εκτέλεση mmc.exe και στη συνέχεια, προσθέστε το συμπληρωματικό πρόγραμμα **πιστοποιητικά** .
    
    - Κάτω από τον κόμβο **Αξιόπιστες αρχές έκδοσης πιστοποιητικών ρίζας** , κάντε διπλό κλικ στο όνομά σας πιστοποιητικό. Εάν χρησιμοποιείτε το πιστοποιητικό αυτόματης υπογραφής από την εικονική Μηχανή, το πιστοποιητικό παίρνει το όνομα DNS του η Εικονική και τελειώνει με **cloudapp.net**.
    
    - Κάντε κλικ στην καρτέλα " **Λεπτομέρειες** ".
    
    - Κάντε κλικ στην επιλογή **αποτύπωση**. Η τιμή του την αποτύπωση εμφανίζεται στο πεδίο λεπτομερειών, για παράδειγμα af 11 60 b6 4 β 28 8 d 89 0a 82 12 λλ 6β a9 c3 66 4f 31 90 48
    
    - **Πριν να εκτελέσετε τη δέσμη ενεργειών**, καταργήστε τα διαστήματα ανάμεσα στα τα ζεύγη τιμών. Για παράδειγμα af1160b64b288d890a8212ff6ba9c3664f319048

1. Τροποποιήστε την παράμετρο **$httpsport** : 

    - Εάν χρησιμοποιούσατε θύρα 443 για το τελικό σημείο HTTPS, στη συνέχεια, δεν χρειάζεται να ενημερώσετε αυτή η παράμετρος στη δέσμη ενεργειών. Διαφορετικά χρησιμοποιήστε την τιμή θύρας που επιλέξατε όταν ρυθμίσατε το τελικό σημείο ιδιωτικό HTTPS στο η Εικονική.

1. Τροποποιήστε την παράμετρο **$DNSName** : 

    - Η δέσμη ενεργειών έχει ρυθμιστεί για ένα πιστοποιητικό μπαλαντέρ $DNSName = "+". Εάν το κάνετε δεν θέλετε να ρυθμίσετε τις παραμέτρους για μια σύνδεση πιστοποιητικού μπαλαντέρ, να σχολιάσετε $DNSName ="+"και να ενεργοποιήσετε την ακόλουθη γραμμή, η αναφορά πλήρους $DNSNAme, ## $DNSName="$server.cloudapp.net".

        Αλλάξτε την τιμή $DNSName, εάν δεν θέλετε να χρησιμοποιήσετε την εικονική μηχανή όνομα DNS για υπηρεσίες αναφοράς. Εάν χρησιμοποιήσετε την παράμετρο, το πιστοποιητικό πρέπει επίσης να χρησιμοποιήσετε αυτό το όνομα και καταχώρηση του ονόματος καθολικά σε ένα διακομιστή DNS.

1. Η δέσμη ενεργειών έχει ρυθμιστεί για τις υπηρεσίες αναφοράς. Εάν θέλετε να εκτελέσετε τη δέσμη ενεργειών για τις υπηρεσίες αναφοράς, τροποποιήστε το τμήμα έκδοση τη διαδρομή προς το χώρο ονομάτων για να V11 ":", στην κατάσταση Get-WmiObject.

1. Εκτελέστε τη δέσμη ενεργειών.

**Επικύρωση**: για να επαληθεύσετε ότι λειτουργεί η λειτουργικότητα διακομιστή βασικής αναφοράς, ανατρέξτε στην ενότητα [Επαλήθευση της ρύθμισης παραμέτρων](#verify-the-connection) αργότερα σε αυτό το θέμα. Για να επαληθεύσετε το πιστοποιητικό σύνδεση Ανοίξτε μια γραμμή εντολών με δικαιώματα διαχειριστή και, στη συνέχεια, εκτελέστε την ακόλουθη εντολή:

    netsh http show sslcert

Το αποτέλεσμα θα περιλαμβάνουν τα εξής:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Χρήση της διαχείρισης παραμέτρων για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς

Εάν δεν θέλετε να εκτελέσετε τη δέσμη ενεργειών του PowerShell για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς, ακολουθήστε τα βήματα σε αυτήν την ενότητα για να χρησιμοποιήσετε τη Διαχείριση ομάδας παραμέτρων εγγενή λειτουργία των υπηρεσιών αναφοράς για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς.

1. Από την πύλη του Azure κλασική, επιλέξτε την εικονική Μηχανή και κάντε κλικ στην επιλογή σύνδεση. Χρησιμοποιήστε το όνομα χρήστη και τον κωδικό πρόσβασης που έχετε ρυθμίσει τις παραμέτρους όταν δημιουργήσατε την εικονική Μηχανή.

    ![σύνδεση με azure εικονική μηχανή](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)

1. Εκτέλεση του Windows update και εγκαταστήστε τις ενημερώσεις για την εικονική Μηχανή. Εάν απαιτείται επανεκκίνηση του η Εικονική, επανεκκινήστε το Εικονική και συνδεθείτε ξανά με την εικονική Μηχανή από την πύλη του Azure κλασική.

1. Από το μενού Έναρξη η Εικονική, πληκτρολογήστε **Υπηρεσιών αναφοράς** και άνοιγμα **Αναφοράς υπηρεσιών Configuration Manager**.

1. Αφήστε τις προεπιλεγμένες τιμές για το **Όνομα του διακομιστή** και **Παρουσία διακομιστή αναφοράς**. Κάντε κλικ στην επιλογή **σύνδεση**.

1. Στο αριστερό παράθυρο, κάντε κλικ στην επιλογή **Διεύθυνση URL της υπηρεσίας Web**.

1. Από προεπιλογή, RS έχει ρυθμιστεί για θύρας HTTP 80 με IP "Ανάθεση όλων". Για να προσθέσετε HTTPS:

    1. Στο **Πιστοποιητικό SSL**: Επιλέξτε το πιστοποιητικό που θέλετε να χρησιμοποιήσετε, για παράδειγμα [όνομα Εικονική]. cloudapp.net. Εάν δεν υπάρχουν πιστοποιητικά είναι στη λίστα, ανατρέξτε στην ενότητα **βήμα 2: Δημιουργήστε ένα πιστοποιητικό διακομιστή** για πληροφορίες σχετικά με τον τρόπο εγκατάστασης και αξιόπιστο το πιστοποιητικό στην η Εικονική.
    
    1. Στην περιοχή **Θύρα SSL**: Επιλέξτε 443. Εάν έχετε ρυθμίσει το τελικό σημείο ιδιωτικό HTTPS σε η Εικονική με μια διαφορετική θύρα ιδιωτικό, χρησιμοποιήστε αυτήν την τιμή εδώ.
    
    1. Κάντε κλικ στην επιλογή **εφαρμογή** και περιμένετε να ολοκληρωθεί η λειτουργία.

1. Στο αριστερό παράθυρο, κάντε κλικ στην επιλογή **βάση δεδομένων**.

    1. Κάντε κλικ στην επιλογή **Αλλαγή βάση**e.
    
    1. Κάντε κλικ στην επιλογή **Δημιουργία νέας βάσης δεδομένων διακομιστή αναφοράς** και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
    
    1. Κρατήστε το προεπιλεγμένο **Όνομα διακομιστή**: ως η Εικονική δώστε ένα όνομα και αφήστε το προεπιλεγμένο **Τύπο ελέγχου ταυτότητας** ως **Τρέχοντα χρήστη** – **Ενσωματωμένη ασφάλεια**. Κάντε κλικ στο κουμπί **Επόμενο**.
    
    1. Αφήστε το προεπιλεγμένο **Όνομα βάσης δεδομένων** ως **ReportServer** και κάντε κλικ στο κουμπί **Επόμενο**.
    
    1. Αφήστε τις προεπιλεγμένες **Τύπος ελέγχου ταυτότητας** με **Διαπιστευτήρια υπηρεσίας** και κάντε κλικ στο κουμπί **Επόμενο**.
    
    1. Κάντε κλικ στο κουμπί **Επόμενο** στη σελίδα **Σύνοψη** .
    
    1. Όταν ολοκληρωθεί η ρύθμιση παραμέτρων, κάντε κλικ στο κουμπί **Τέλος**.

1. Στο αριστερό παράθυρο, κάντε κλικ στην επιλογή **Διαχείριση αναφορών διεύθυνση URL**. Αφήστε τις προεπιλεγμένες **Εικονικό κατάλογο** ως **αναφορών** και κάντε κλικ στο κουμπί **εφαρμογή**.

1. Κάντε κλικ στην επιλογή **Έξοδος** για να κλείσετε τη Διαχείριση ομάδας παραμέτρων υπηρεσιών αναφοράς.

## <a name="step-4-open-windows-firewall-port"></a>Βήμα 4: Άνοιγμα Windows τείχος προστασίας θύρα

>[AZURE.NOTE] Αν χρησιμοποιήσατε μία από τις δέσμες ενεργειών για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς, μπορείτε να παραλείψετε αυτή την ενότητα. Η δέσμη ενεργειών συμπεριλαμβάνεται ένα βήμα για να ανοίξετε τη θύρα τείχος προστασίας. Η προεπιλογή έχει θύρα 80 για το HTTP και τη θύρα 443 για το HTTPS.

Για να συνδεθείτε απομακρυσμένη διαχείριση αναφορών ή το διακομιστή αναφορών στον υπολογιστή εικονικές, ένα τελικό σημείο TCP απαιτείται για την εικονική Μηχανή. Απαιτείται για να ανοίξετε την ίδια θύρα στο τείχος προστασίας του Εικονική. Το τελικό σημείο δημιουργήθηκε όταν παρέχεται η Εικονική.

Αυτή η ενότητα παρέχει βασικές πληροφορίες σχετικά με τον τρόπο για να ανοίξετε τη θύρα τείχος προστασίας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων του τείχους προστασίας για πρόσβαση στο διακομιστή αναφοράς](https://technet.microsoft.com/library/bb934283.aspx)

>[AZURE.NOTE] Εάν χρησιμοποιήσατε τη δέσμη ενεργειών για τη ρύθμιση παραμέτρων του διακομιστή αναφοράς, μπορείτε να παραλείψετε αυτή την ενότητα. Η δέσμη ενεργειών συμπεριλαμβάνεται ένα βήμα για να ανοίξετε τη θύρα τείχος προστασίας.

Εάν έχετε ρυθμίσει μια ιδιωτική θύρα για HTTPS εκτός από 443, τροποποιήστε την ακόλουθη δέσμη ενεργειών σωστά. Για να ανοίξετε τη θύρα **443** του τείχους προστασίας των Windows, ολοκληρώστε τα εξής:

1. Ανοίξτε ένα παράθυρο του Windows PowerShell με δικαιώματα διαχειριστή.

1. Εάν χρησιμοποιήσατε μια θύρα εκτός από 443 όταν ρυθμίσατε το τελικό σημείο HTTPS στο η Εικονική, ενημερώστε τη θύρα στην παρακάτω εντολή και, στη συνέχεια, εκτελέστε την εντολή:

        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443

1. Κατά την ολοκλήρωση της εντολής, **Ok** εμφανίζεται στη γραμμή εντολών.

Για να επαληθεύσετε ότι έχει ανοίξει τη θύρα, ανοίξτε ένα παράθυρο του Windows PowerShell και εκτελέστε την ακόλουθη εντολή:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Επαληθεύστε τις ρυθμίσεις παραμέτρων

Για να επαληθεύσετε ότι τώρα λειτουργεί τη λειτουργικότητα διακομιστή βασικής αναφοράς, ανοίξτε το πρόγραμμα περιήγησης με δικαιώματα διαχειριστή και, στη συνέχεια, αναζητήστε την παρακάτω αναφορά διακομιστή ad Διαχείριση αναφορών διευθύνσεις URL:

- Στην την εικονική Μηχανή, μεταβείτε στη διεύθυνση URL διακομιστή αναφοράς:

        http://localhost/reportserver

- Στην την εικονική Μηχανή, μεταβείτε στη διεύθυνση URL διαχείρισης αναφοράς:

        http://localhost/Reports

- Από τον τοπικό υπολογιστή σας, αναζητήστε την **Απομακρυσμένη** αναφορά Manager για την εικονική Μηχανή. Ενημερώστε το όνομα DNS στο παρακάτω παράδειγμα ανάλογα με την περίπτωση. Όταν σας ζητηθεί κωδικός πρόσβασης, χρησιμοποιήστε τα διαπιστευτήρια διαχειριστή που δημιουργήσατε κατά την εικονική Μηχανή παρέχεται. Το όνομα χρήστη είναι στο [τομέας]\[όνομα χρήστη] μορφή, όπου ο τομέας είναι το όνομα υπολογιστή Εικονική, για παράδειγμα ssrsnativecloud\testuser. Εάν δεν χρησιμοποιείτε HTTP**S**, καταργήστε το **s** στη διεύθυνση URL. Ανατρέξτε στην επόμενη ενότητα για πληροφορίες σχετικά με τη δημιουργία επιπλέον χρήστες στην Εικονική.

        https://ssrsnativecloud.cloudapp.net/Reports

- Από τον τοπικό υπολογιστή, μεταβείτε στη διεύθυνση URL απομακρυσμένης αναφοράς διακομιστή. Ενημερώστε το όνομα DNS στο παρακάτω παράδειγμα ανάλογα με την περίπτωση. Εάν δεν χρησιμοποιείτε HTTPS, καταργήστε το s στη διεύθυνση URL.

        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Δημιουργία χρηστών και η αντιστοίχιση ρόλων

Μετά τη ρύθμιση των παραμέτρων και την επαλήθευση στο διακομιστή αναφορών, μια κοινή εργασία διαχείρισης είναι να δημιουργήσετε έναν ή περισσότερους χρήστες και να εκχωρήσετε στους χρήστες σε ρόλους υπηρεσιών αναφοράς. Για περισσότερες πληροφορίες, ανατρέξτε στα παρακάτω:

- [Δημιουργήστε ένα τοπικό λογαριασμό χρήστη](https://technet.microsoft.com/library/cc770642.aspx)

- [Εκχώρηση χρήστη πρόσβαση σε ένα διακομιστή αναφοράς (Διαχείριση αναφορών)](https://msdn.microsoft.com/library/ms156034.aspx))

- [Δημιουργία και διαχείριση εκχώρησης ρόλου](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Για να δημιουργήσετε και να δημοσιεύσετε αναφορές του Azure εικονική μηχανή

Ο παρακάτω πίνακας συνοψίζει ορισμένες από τις επιλογές που είναι διαθέσιμες για να δημοσιεύσετε τις αναφορές από έναν υπολογιστή εσωτερικής εγκατάστασης στο διακομιστή αναφορών που φιλοξενούνται σε Azure εικονική μηχανή της Microsoft:

- **Δέσμη ενεργειών RS.exe**: RS.exe χρήση δέσμης ενεργειών για την αντιγραφή στοιχείων αναφοράς από και υπάρχοντος διακομιστή αναφοράς για το Microsoft Azure εικονική μηχανή. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα "Εγγενή λειτουργία σε εγγενή λειτουργία – Microsoft Azure εικονική μηχανή" στο [δείγμα υπηρεσιών αναφοράς rs.exe δέσμη ενεργειών για μετεγκατάσταση περιεχομένου ανάμεσα σε διακομιστές αναφορών](https://msdn.microsoft.com/library/dn531017.aspx).

- **Εργαλείο δόμησης αναφορών**: Η εικονική μηχανή περιλαμβάνει κάντε κλικ στην επιλογή-μία φορά την έκδοση του Microsoft SQL Server Report Builder. Για να ξεκινήσετε αναφοράς Δόμηση την πρώτη φορά στην η εικονική μηχανή:

    1. Ξεκινήστε το πρόγραμμα περιήγησης με δικαιώματα διαχειριστή.
    
    1. Μεταβείτε στη Διαχείριση αναφορών στον υπολογιστή εικονικές και κάντε κλικ στο **Εργαλείο δόμησης αναφορών** στην κορδέλα.

    Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εγκατάσταση, κατάργηση της εγκατάστασης, και υποστήριξης, εργαλείο δόμησης αναφορών](https://technet.microsoft.com/library/dd207038.aspx).

- **SQL Server Data Tools: Εικονική**: Εάν έχετε δημιουργήσει την εικονική Μηχανή με SQL Server 2012, στη συνέχεια, SQL Server Data Tools είναι εγκατεστημένο στον υπολογιστή εικονικές και μπορεί να χρησιμοποιηθεί για να δημιουργήσετε **Αναφορά διακομιστή έργα** και αναφορές σχετικά με την εικονική μηχανή. SQL Server Data Tools να δημοσιεύσετε τις αναφορές στο διακομιστή αναφορών στον υπολογιστή εικονική.
    
    Εάν έχετε δημιουργήσει την εικονική Μηχανή με τον SQL server 2014, μπορείτε να εγκαταστήσετε το SQL Server δεδομένων εργαλεία - BI για το visual Studio. Για περισσότερες πληροφορίες, ανατρέξτε στα παρακάτω:

    - [Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)

    - [Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)

    - [Εργαλεία δεδομένων του SQL Server και SQL Server επιχειρηματικής ευφυΐας (SSDT-BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)

- **SQL Server Data Tools: απομακρυσμένης**: στον τοπικό σας υπολογιστή, δημιουργήσετε ένα έργο υπηρεσιών αναφοράς του SQL Server Data Tools που περιέχει αναφορές των υπηρεσιών αναφοράς. Ρυθμίστε τις παραμέτρους του έργου για να συνδεθείτε με τη διεύθυνση URL της υπηρεσίας web.

    ![ιδιότητες του έργου ssdt SSRS έργου](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)

- **Χρήση δέσμης ενεργειών**: χρήση δέσμης ενεργειών για την αντιγραφή περιεχομένου διακομιστή αναφοράς. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [rs.exe υπηρεσιών αναφοράς του δείγματος δέσμης ενεργειών για μετεγκατάσταση περιεχομένου ανάμεσα σε διακομιστές αναφορών](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Ελαχιστοποίηση κόστος, εάν δεν χρησιμοποιείτε την εικονική Μηχανή

>[AZURE.NOTE] Για να ελαχιστοποιήσετε τις χρεώσεις για τις εικονικές μηχανές Windows Azure όταν δεν είναι σε χρήση, τερματίστε την εικονική Μηχανή από την πύλη του Azure κλασική. Εάν χρησιμοποιείτε τις επιλογές τροφοδοσίας Windows μέσα σε μια Εικονική για να τερματίσετε την εικονική Μηχανή, που θα χρεωθείτε εξακολουθεί να της ίδιας ποσότητας για την εικονική Μηχανή. Για μείωση της επιβάρυνσης, πρέπει να τερματίσετε την εικονική Μηχανή στην πύλη του Azure κλασική. Εάν δεν χρειάζεστε πλέον την εικονική Μηχανή, μην ξεχάσετε να διαγράψετε την εικονική Μηχανή και τα αρχεία που σχετίζονται .vhd για αποφυγή χρεώσεων λόγω χώρου αποθήκευσης. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα Συνήθεις Ερωτήσεις [εικονικές μηχανές τις τιμές](https://azure.microsoft.com/pricing/details/virtual-machines/)των λεπτομερειών.

## <a name="more-information"></a>Περισσότερες πληροφορίες

### <a name="resources"></a>Πόροι

- Για παρόμοιο περιεχόμενο που σχετίζονται με μια ανάπτυξη μεμονωμένου διακομιστή επιχειρηματικής ευφυΐας του SQL Server και του SharePoint 2013, ανατρέξτε στο θέμα [Χρήση του Windows PowerShell για να δημιουργήσετε μια Azure Εικονική με SQL Server BI και το SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).

- Για παρόμοιο περιεχόμενο που σχετίζονται με μια ανάπτυξη πολλών διακομιστών επιχειρηματικής ευφυΐας του SQL Server και του SharePoint 2013, ανατρέξτε στο θέμα [Ανάπτυξη του SQL Server επιχειρηματικής ευφυΐας σε εικονικές μηχανές Windows Azure](https://msdn.microsoft.com/library/dn321998.aspx).

- Για γενικές πληροφορίες που σχετίζονται με υλοποιήσεις του SQL Server επιχειρηματικής ευφυΐας σε εικονικές μηχανές Windows Azure, ανατρέξτε στο θέμα [SQL Server επιχειρηματικής ευφυΐας σε εικονικές μηχανές Windows Azure](virtual-machines-windows-classic-ps-sql-bi.md).

- Για περισσότερες πληροφορίες σχετικά με το κόστος του Azure τον υπολογισμό χρεώσεις, ανατρέξτε στο θέμα καρτέλα εικονικές μηχανές του [Azure Αριθμομηχανής τις πληροφορίες τιμολόγησης](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Περιεχόμενο Κοινότητας

- Για οδηγίες βήμα προς βήμα σχετικά με τον τρόπο δημιουργίας μιας εγγενούς υπηρεσιών αναφοράς διακομιστή αναφοράς λειτουργία χωρίς τη χρήση δέσμης ενεργειών, ανατρέξτε στο θέμα [Φιλοξενίας SQL υπηρεσία αναφοράς στην Azure εικονική μηχανή](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Συνδέσεις με άλλους πόρους για τον SQL Server στο ΣΠΣ Azure

[SQL Server Azure εικονικές μηχανές Επισκόπηση](virtual-machines-windows-sql-server-iaas-overview.md)
