<properties 
    pageTitle="Χρησιμοποιήστε SSH πλήκτρα με τα Windows για ΣΠΣ Linux | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε και να χρησιμοποιήσετε SSH πλήκτρα σε υπολογιστή με Windows για να συνδεθείτε με ένα εικονικό μηχάνημα Linux σε Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="rasquill"/>

# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Πώς να πλήκτρα χρήση SSH με παράθυρα στην Azure

> [AZURE.SELECTOR]
- [Windows](virtual-machines-linux-ssh-from-windows.md)
- [Linux/Mac](virtual-machines-linux-mac-create-ssh-keys.md)

Όταν συνδέεστε σε εικονικές μηχανές Linux (ΣΠΣ) στο Azure, θα πρέπει να μπορείτε να χρησιμοποιήσετε [κρυπτογράφηση δημόσιου κλειδιού](https://wikipedia.org/wiki/Public-key_cryptography) για να παρέχουν μια πιο ασφαλής τρόπος για να συνδεθείτε με το Εικονική Linux. Αυτή η διαδικασία περιλαμβάνει μια δημόσια και ιδιωτικά ανταλλαγή κλειδιών, χρησιμοποιώντας την εντολή ασφαλούς κελύφους (SSH) για τον έλεγχο ταυτότητας στον εαυτό σας και όχι ένα όνομα χρήστη και τον κωδικό πρόσβασης. Στους κωδικούς πρόσβασης γίνεται ευάλωτο σε βίαιες ενέργειες Ώθηση επιθέσεις, ειδικά σε ΣΠΣ μέσω Internet όπως διακομιστές web. Σε αυτό το άρθρο παρέχει μια επισκόπηση των πλήκτρων SSH και πώς να δημιουργείτε τα κατάλληλα κλειδιά σε έναν υπολογιστή Windows.


## <a name="overview-of-ssh-and-keys"></a>Επισκόπηση των SSH και κλειδιά

Ασφαλή μπορούν να συνδεθούν στην σας Εικονική Linux χρησιμοποιώντας δημόσια και ιδιωτικά κλειδιά:

- Το **δημόσιο κλειδί** τοποθετείται στο σας Εικονική Linux ή σε οποιαδήποτε άλλη υπηρεσία που θέλετε να χρησιμοποιήσετε με το δημόσιο κλειδί κρυπτογράφησης.
- Το **ιδιωτικό κλειδί** είναι τι κάνετε παρουσίαση για να σας Εικονική Linux κατά τη σύνδεση, για να επιβεβαιώσετε την ταυτότητά σας. Προστασία ιδιωτικό κλειδί. Μην κάνετε κοινή χρήση του.

Αυτά τα δημόσια και ιδιωτικά κλειδιά μπορεί να χρησιμοποιηθεί σε πολλές ΣΠΣ και τις υπηρεσίες. Δεν χρειάζεται ένα ζεύγος κλειδιών για κάθε Εικονική ή την υπηρεσία που θέλετε να έχετε πρόσβαση. Για μια πιο λεπτομερή επισκόπηση, ανατρέξτε στο θέμα [κρυπτογράφηση δημόσιου κλειδιού](https://wikipedia.org/wiki/Public-key_cryptography).

SSH είναι ένα πρωτόκολλο κρυπτογραφημένη σύνδεση που επιτρέπει ασφαλείς συνδέσεις σε μη ασφαλείς συνδέσεις. Είναι το προεπιλεγμένο πρωτόκολλο σύνδεσης για ΣΠΣ Linux φιλοξενείται στο Azure. Παρόλο που SSH ίδια παρέχει κρυπτογραφημένη σύνδεση, χρήση κωδικών πρόσβασης με SSH συνδέσεις εξακολουθούν να αφήνει το Εικονική ευάλωτο σε ισχύ βίαιες ενέργειες επιθέσεις ή υποθέσεις από τους κωδικούς πρόσβασης. Μια πιο ασφαλή, και προτιμώμενη, μέθοδο τη σύνδεση σε μια Εικονική χρησιμοποιώντας SSH είναι να χρησιμοποιήσετε αυτά τα δημόσια και ιδιωτικά κλειδιά, γνωστές και ως SSH πλήκτρα.

Εάν δεν θέλετε να χρησιμοποιήσετε τα πλήκτρα SSH, μπορείτε να εξακολουθείτε να συνδεθείτε σας ΣΠΣ Linux χρησιμοποιώντας έναν κωδικό πρόσβασης. Εάν σας Εικονική δεν εκτίθεται στο Internet, χρησιμοποιώντας τους κωδικούς πρόσβασης μπορεί να επαρκούν. Ωστόσο, εξακολουθείτε να χρειάζεστε για διαχείριση των κωδικών πρόσβασης για κάθε Εικονική Linux και τη συντήρηση πολιτικές κωδικού πρόσβασης σε καλή κατάσταση και, όπως ελάχιστο μήκος κωδικού πρόσβασης και την τακτικά ενημέρωσή τους. Χρήση κλειδιών SSH μειώνει την πολυπλοκότητα της διαχείρισης μεμονωμένα διαπιστευτήρια σε πολλές ΣΠΣ.


## <a name="windows-packages-and-ssh-clients"></a>Τα πακέτα των Windows και SSH προγράμματα-πελάτες

Μπορείτε να συνδεθείτε και να διαχειριστείτε ΣΠΣ Linux στο Azure χρησιμοποιώντας μια **ssh** πρόγραμμα-πελάτη. Υπολογιστές με Windows δεν έχουν συνήθως μια **ssh** εγκατεστημένο πρόγραμμα-πελάτη. Κοινές Windows SSH προγράμματα-πελάτες μπορείτε να το εγκαταστήσετε περιλαμβάνονται στα ακόλουθα πακέτα:

- [Git για Windows](https://git-for-windows.github.io/)
- [puTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Cygwin](https://cygwin.com/)

> [AZURE.NOTE] Την πιο πρόσφατη ενημέρωση των Windows 10 Επέτειος περιλαμβάνει πάρτι για Windows. Αυτή η δυνατότητα σάς επιτρέπει να εκτελέσετε το Windows υποσυστήματος για πρόσβαση και Linux βοηθητικά προγράμματα, όπως ένα πρόγραμμα-πελάτη SSH. Πάρτι για Windows είναι ακόμη υπό επεξεργασία, θεωρείται μια έκδοση Beta. Για περισσότερες πληροφορίες σχετικά με το πάρτι για Windows, ανατρέξτε στο θέμα [πάρτι σε Ubuntu στα Windows](https://msdn.microsoft.com/commandline/wsl/about).


## <a name="which-key-files-do-you-need-to-create"></a>Ποια αρχεία κλειδιών χρειάζεστε για να δημιουργήσετε;

Azure απαιτεί τουλάχιστον 2048 bit, **ssh rsa** μορφή δημόσια και ιδιωτικά κλειδιά. Εάν διαχειρίζεστε Azure πόρων με χρήση του μοντέλου ανάπτυξης κλασική, πρέπει επίσης να δημιουργήσετε ένα PEM (`.pem` αρχείου).

Εδώ θα βρείτε τα σενάρια ανάπτυξης και τους τύπους αρχείων που χρησιμοποιείτε σε κάθε:

1. πλήκτρα **SSH rsa** είναι απαραίτητα για οποιαδήποτε ανάπτυξη με την [πύλη του Azure](https://portal.azure.com)και τη διαχείριση πόρων αναπτύξεις χρησιμοποιώντας το [Azure CLI](../xplat-cli-install.md).
    - Τα πλήκτρα είναι συνήθως όλες οι περισσότεροι χρήστες χρειάζονται.
2. `.pem`Για να δημιουργήσετε ΣΠΣ με την [πύλη κλασική](https://manage.windowsazure.com)απαιτείται αρχείο. Τα πλήκτρα υποστηρίζονται επίσης στο αναπτύξεις κλασική που χρησιμοποιούν το [Azure CLI](../xplat-cli-install.md).
    - Πρέπει να δημιουργήσετε αυτές τις επιπλέον πλήκτρα και τα πιστοποιητικά, εάν διαχειρίζεστε τους πόρους που έχουν δημιουργηθεί με τη χρήση του μοντέλου ανάπτυξης κλασική.


## <a name="install-git-for-windows"></a>Εγκατάσταση Git για Windows

Στην προηγούμενη ενότητα αναφέρεται πολλές πακέτων που περιλαμβάνουν το `openssl` εργαλείο για Windows. Αυτό το εργαλείο είναι απαραίτητη για να δημιουργήσετε δημόσια και ιδιωτικά κλειδιά. Τα παρακάτω παραδείγματα με λεπτομέρειες τον τρόπο για να εγκαταστήσετε και να χρησιμοποιήσετε **Git για Windows**, αν και μπορείτε να επιλέξετε οποιαδήποτε πακέτο που προτιμάτε. **Git για Windows** παρέχει πρόσβαση σε ορισμένες επιπλέον λογισμικό ανοιχτού κώδικα ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) εργαλεία και βοηθητικά προγράμματα που μπορεί να είναι χρήσιμο όταν εργάζεστε με ΣΠΣ Linux.

1. Λήψη και εγκατάσταση του **Git για Windows** από την παρακάτω θέση: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).

2. Αποδεχτείτε τις προεπιλεγμένες επιλογές κατά τη διαδικασία εγκατάστασης, εκτός αν χρειάζεστε συγκεκριμένα για να τα αλλάξετε.

3. Εκτέλεση **Πάρτι Git** από το **μενού "Έναρξη"** > **Git** > **Git πάρτι**. Κονσόλα μοιάζει με το ακόλουθο παράδειγμα:

    ![Κέλυφος Git για Windows πάρτι](./media/virtual-machines-linux-ssh-from-windows/git-bash-window.png)


## <a name="create-a-private-key"></a>Δημιουργήστε ένα ιδιωτικό κλειδί

1. Στο παράθυρο του **Git πάρτι** , χρησιμοποιήστε `openssl.exe` για να δημιουργήσετε ένα ιδιωτικό κλειδί. Το ακόλουθο παράδειγμα δημιουργεί έναν αριθμό-κλειδί που ονομάζεται `myPrivateKey` και με το όνομα πιστοποιητικό `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    Το αποτέλεσμα μοιάζει με το ακόλουθο παράδειγμα:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

2. Απαντήστε τα μηνύματα προτροπής για όνομα χώρας, θέση, όνομα εταιρείας, κ.λπ.

3. Το νέο ιδιωτικό κλειδί και το πιστοποιητικό δημιουργούνται στον κατάλογό σας τρέχουσα εργασία. Για βέλτιστες πρακτικές ασφαλείας, θα πρέπει να ορίζετε τα δικαιώματα σε σας ιδιωτικό κλειδί, έτσι ώστε μόνο μπορείτε να αποκτήσετε πρόσβαση σε αυτήν:

    ```bash
    chmod 0600 myPrivateKey
    ```

4. Εάν χρειάζεστε για να διαχειριστείτε πόρους κλασική, μετατρέψτε το `myCert.pem` να `myCert.cer` (DER κωδικοποιημένο X509 πιστοποιητικό). Εκτέλεση αυτό το προαιρετικό βήμα μόνο εάν πρέπει να συγκεκριμένα διαχείριση παλαιότερων κλασική πόρων. 

    Μετατρέψτε το πιστοποιητικό χρησιμοποιώντας την ακόλουθη εντολή:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Δημιουργήστε ένα ιδιωτικό κλειδί για PuTTY

PuTTY είναι ένα κοινό πρόγραμμα-πελάτη SSH για Windows. Που είναι δωρεάν για να χρησιμοποιήσετε οποιονδήποτε υπολογιστή-πελάτη SSH που θέλετε. Για να χρησιμοποιήσετε PuTTY, πρέπει να δημιουργήσετε έναν τύπο επιπλέον του αριθμού-κλειδιού - μια PuTTY ιδιωτικό κλειδί (PPK). Εάν δεν θέλετε να χρησιμοποιήσετε PuTTY, παραλείψτε αυτήν την ενότητα.

Το παρακάτω παράδειγμα δημιουργεί αυτό επιπλέον ιδιωτικό κλειδί ειδικά για PuTTY για να χρησιμοποιήσετε:

1. Χρησιμοποιήστε **Git πάρτι** για να μετατρέψετε το ιδιωτικό κλειδί σας σε ένα ιδιωτικό κλειδί RSA που μπορεί να κατανοήσετε PuTTYgen. Το ακόλουθο παράδειγμα δημιουργεί έναν αριθμό-κλειδί που ονομάζεται `myPrivateKey_rsa` από το υπάρχον κλειδί με το όνομα `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Για βέλτιστες πρακτικές ασφαλείας, θα πρέπει να ορίζετε τα δικαιώματα σε σας ιδιωτικό κλειδί, έτσι ώστε μόνο μπορείτε να αποκτήσετε πρόσβαση σε αυτήν:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```

2. Κάντε λήψη και εκτελέστε PuTTYgen από την ακόλουθη θέση: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

3. Κάντε κλικ στο μενού: **αρχείο** > **φόρτωσης ένα ιδιωτικό κλειδί**

4. Εντοπίστε το ιδιωτικό κλειδί (`myPrivateKey_rsa` στο προηγούμενο παράδειγμα). Ο προεπιλεγμένος κατάλογος κατά την εκκίνηση του **Πάρτι Git** είναι `C:\Users\%username%`. Αλλαγή του αρχείου φίλτρου για να εμφανίσετε **όλα τα αρχεία (\*.\*)**:

    ![Φόρτωση το υπάρχον ιδιωτικό κλειδί στο PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/load-private-key.png)

5. Κάντε κλικ στην επιλογή **Άνοιγμα**. Ένα μήνυμα υποδεικνύει ότι το κλειδί έχει εισαχθεί με επιτυχία:

    ![Που έχουν εισαχθεί με επιτυχία το κλειδί για να PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/successfully-imported-key.png)

6. Κάντε κλικ στο **κουμπί OK** για να κλείσετε το μήνυμα.

7. Το δημόσιο κλειδί εμφανίζεται στο επάνω μέρος του παραθύρου **PuTTYgen** . Μπορείτε αντιγράψτε και επικολλήστε αυτή δημόσιο κλειδί στο Azure πύλη ή το πρότυπο διαχείρισης πόρων Azure όταν δημιουργείτε μια Εικονική Linux. Μπορείτε επίσης να επιλέξετε **Αποθήκευση δημόσιο κλειδί** για να αποθηκεύσετε ένα αντίγραφο στον υπολογιστή σας:

    ![Αποθήκευση PuTTY δημόσιο κλειδί αρχείο](./media/virtual-machines-linux-ssh-from-windows/save-public-key.png)

    Το παρακάτω παράδειγμα εμφανίζει πώς θα αντιγράψετε και επικολλήσετε δημόσιο κλειδί στην πύλη του Azure όταν δημιουργείτε μια Εικονική Linux. Το δημόσιο κλειδί συνήθως αποθηκεύεται στη συνέχεια `~/.ssh/authorized_keys` στη νέα σας Εικονική.

    ![Όταν δημιουργείτε μια Εικονική στην πύλη του Azure Χρησιμοποιήστε δημόσιο κλειδί](./media/virtual-machines-linux-ssh-from-windows/use-public-key-azure-portal.png)

7. Πίσω στο **PuTTYgen**, κάντε κλικ στην επιλογή **Αποθήκευση ιδιωτικό κλειδί**:

    ![Αποθήκευση αρχείου PuTTY ιδιωτικό κλειδί](./media/virtual-machines-linux-ssh-from-windows/save-ppk-file.png)

    > [AZURE.WARNING] Ένα μήνυμα που σας ρωτά εάν θέλετε να συνεχίσετε χωρίς να εισαγάγετε μια φράση πρόσβασης για τον αριθμό-κλειδί. Η φράση πρόσβασης είναι όπως έναν κωδικό πρόσβασης που έχουν επισυναφθεί σε σας ιδιωτικό κλειδί. Ακόμη και αν κάποιος για να αποκτήσετε το ιδιωτικό κλειδί σας, τους εξακολουθεί να δεν θα μπορούν να ελέγχουν την ταυτότητα χρησιμοποιώντας μόνο το κλειδί. Επίσης, χρειάζεστε τη φράση πρόσβασης. Χωρίς μια φράση πρόσβασης, εάν κάποιος λαμβάνει σας ιδιωτικό κλειδί, μπορούν να συνδέονται οποιαδήποτε Εικονική ή στην υπηρεσία που χρησιμοποιεί αυτό το πλήκτρο. Συνιστάται να δημιουργήσετε μια φράση πρόσβασης. Ωστόσο, εάν ξεχάσετε τη φράση πρόσβασης, δεν υπάρχει τρόπος να το ανακτήσετε.

    Εάν θέλετε να εισαγάγετε μια φράση πρόσβασης, κάντε κλικ στην επιλογή **όχι**, πληκτρολογήστε μια φράση πρόσβασης στο κύριο παράθυρο του PuTTYgen και, στη συνέχεια, κάντε ξανά κλικ στο κουμπί **Αποθήκευση ιδιωτικό κλειδί** . Διαφορετικά, κάντε κλικ στο κουμπί **Ναι** για να συνεχίσετε χωρίς να παραχωρήσετε το προαιρετικό φράση πρόσβασης.

8. Πληκτρολογήστε ένα όνομα και μια θέση για να αποθηκεύσετε το αρχείο σας PPK.


## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Χρήση Putty να SSH σε ένα μηχάνημα Linux

Και πάλι, PuTTY είναι ένα κοινό πρόγραμμα-πελάτη SSH για Windows. Που είναι δωρεάν για να χρησιμοποιήσετε οποιονδήποτε υπολογιστή-πελάτη SSH που θέλετε. Πώς μπορείτε να χρησιμοποιήσετε το ιδιωτικό κλειδί για τον έλεγχο ταυτότητας με το Azure Εικονική χρησιμοποιώντας SSH με λεπτομέρειες τα παρακάτω βήματα. Τα βήματα είναι παρόμοιος σε άλλα SSH κλειδιού προγράμματα-πελάτες όσον αφορά να χρειάζεται να φορτώσετε το ιδιωτικό κλειδί για τον έλεγχο ταυτότητας της σύνδεσης SSH.

1. Κάντε λήψη και εκτέλεση putty από την ακόλουθη θέση: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Συμπληρώστε το όνομα κεντρικού υπολογιστή ή τη διεύθυνση IP του σας Εικονική από την πύλη του Azure:

    ![Άνοιγμα νέας PuTTY σύνδεσης](./media/virtual-machines-linux-ssh-from-windows/putty-new-connection.png)

3. Προτού επιλέξετε **Άνοιγμα**, κάντε κλικ στην επιλογή **σύνδεση** > **SSH** > **Auth** καρτέλα. Αναζητήστε και επιλέξτε το ιδιωτικό κλειδί σας:

    ![Επιλέξτε το PuTTY ιδιωτικό κλειδί για τον έλεγχο ταυτότητας](./media/virtual-machines-linux-ssh-from-windows/putty-auth-dialog.png)

4. Κάντε κλικ στην επιλογή **Άνοιγμα** για να συνδεθείτε με την εικονική μηχανή
 

## <a name="next-steps"></a>Επόμενα βήματα
Μπορείτε επίσης να δημιουργήσετε τη δημόσια και ιδιωτικά κλειδιά [χρησιμοποιώντας OS X και Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Για περισσότερες πληροφορίες σχετικά με το πάρτι για Windows και τα πλεονεκτήματα της OSS εργαλεία άμεσα διαθέσιμα σε υπολογιστή με Windows, ανατρέξτε στο θέμα [πάρτι σε Ubuntu στα Windows](https://msdn.microsoft.com/commandline/wsl/about).

Εάν έχετε προβλήματα με τη χρήση SSH για να συνδεθείτε με το ΣΠΣ Linux, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων SSH συνδέσεις Εικονική μηχανή Azure Linux](virtual-machines-linux-troubleshoot-ssh-connection.md).