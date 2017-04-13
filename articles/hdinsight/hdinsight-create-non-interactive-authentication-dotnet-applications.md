<properties
    pageTitle="Δημιουργία μη αλληλεπιδραστική ελέγχου ταυτότητας .NET HDInsight applciations | Microsoft Azure"
    description="Μάθετε πώς να δημιουργείτε μη αλληλεπιδραστική ελέγχου ταυτότητας εφαρμογές .NET HDInsight."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Δημιουργία μη αλληλεπιδραστική ελέγχου ταυτότητας εφαρμογών .NET HDInsight

Μπορείτε να εκτελέσετε την εφαρμογή .NET Azure HDInsight με ταυτότητα της εφαρμογής (μη αλληλεπιδραστική) είτε με την ταυτότητα του χρήστη συνδεδεμένοι στο της εφαρμογής (αλληλεπιδραστικά). Για ένα δείγμα την αλληλεπιδραστική εφαρμογή, ανατρέξτε στο θέμα [Υποβολή Hive/γουρούνι/Sqoop εργασίες χρησιμοποιώντας HDInsight .NET SDK](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). Αυτό το άρθρο παρουσιάζει τον τρόπο δημιουργίας μη αλληλεπιδραστική ελέγχου ταυτότητας .NET εφαρμογής για να συνδεθείτε με το Azure HDInsight και υποβάλετε μια εργασία της ομάδας.

Από την εφαρμογή σας .NET, θα πρέπει:

- το Αναγνωριστικό μισθωτή Azure συνδρομή
- το Αναγνωριστικό καταλόγου Azure εφαρμογής προγράμματος-πελάτη
- το μυστικό κλειδί εφαρμογής καταλόγου Azure.  

Η κύρια διαδικασία περιλαμβάνει τα παρακάτω βήματα:

2. Δημιουργία μιας εφαρμογής καταλόγου Azure.
2. Εκχώρηση ρόλων με την εφαρμογή AD.
3. Αναπτύξτε την εφαρμογή-πελάτη.


##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- Σύμπλεγμα HDInsight. Μπορείτε να δημιουργήσετε μία ακολουθώντας τις οδηγίες που υπάρχουν στις το [πρόγραμμα εκμάθησης για γρήγορα αποτελέσματα](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). 




## <a name="create-azure-directory-application"></a>Δημιουργία εφαρμογής καταλόγου Azure 
Όταν δημιουργείτε μια εφαρμογή της υπηρεσίας καταλόγου Active Directory, στην πραγματικότητα δημιουργεί την εφαρμογή και μιας υπηρεσίας κεφαλαίου. Μπορείτε να εκτελέσετε την εφαρμογή στην περιοχή την ταυτότητα της εφαρμογής.

Προς το παρόν, πρέπει να χρησιμοποιήσετε το Azure κλασική πύλη για να δημιουργήσετε μια νέα εφαρμογή υπηρεσίας καταλόγου Active Directory. Αυτή η δυνατότητα θα προστεθεί στην πύλη του Azure σε νεότερη έκδοση. Μπορείτε επίσης να εκτελέσετε τα παρακάτω βήματα μέσω του Azure PowerShell ή Azure CLI. Για περισσότερες πληροφορίες σχετικά με τη χρήση του PowerShell ή CLI με την υπηρεσία του κεφαλαίου, ανατρέξτε στο θέμα [Έλεγχος ταυτότητας υπηρεσίας κεφαλαίου με τη διαχείριση πόρων Azure](../resource-group-authenticate-service-principal.md).

**Για να δημιουργήσετε μια εφαρμογή του καταλόγου Azure**

1.  Πραγματοποιήστε είσοδο στο [Azure κλασική πύλη]( https://manage.windowsazure.com/).
2.  Επιλέξτε την **Υπηρεσία καταλόγου Active Directory** από το αριστερό παράθυρο.

    ![Azure κλασική πύλης υπηρεσίας καταλόγου active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  Επιλέξτε τον κατάλογο που θέλετε να χρησιμοποιήσετε για τη δημιουργία της νέας εφαρμογής. Πρέπει να το υπάρχον.
4.  Κάντε κλικ στην επιλογή **εφαρμογές** από το επάνω μέρος για να εμφανίσετε τις υπάρχουσες εφαρμογές.
5.  Κάντε κλικ στην επιλογή **Προσθήκη** από το κάτω μέρος για να προσθέσετε μια νέα εφαρμογή.
6.  Πληκτρολογήστε **το όνομα**, επιλέξτε **την εφαρμογή Web ή/και το API Web**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![νέα εφαρμογή καταλόγου azure active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  Πληκτρολογήστε **εισόδου στη διεύθυνση URL** και **URI Αναγνωριστικό εφαρμογής**. Για **Διεύθυνση URL ΕΙΣΌΔΟΥ Ενεργο**, δώστε το URI σε μια τοποθεσία web που περιγράφει την εφαρμογή σας. Η ύπαρξη της τοποθεσίας web δεν είναι επικύρωση. Για URI Αναγνωριστικό Εφαρμογής, δώστε το URI που προσδιορίζει την εφαρμογή σας. Και, στη συνέχεια, κάντε κλικ στην επιλογή **ολοκληρώθηκε**.
Χρειάζονται μερικά λεπτά για να δημιουργήσετε την εφαρμογή.  Όταν δημιουργηθεί η εφαρμογή, την πύλη δείχνει τη γρήγορη Glace σελίδα της νέας εφαρμογής. Μην κλείσετε την πύλη. 

    ![νέες ιδιότητες εφαρμογής υπηρεσίας καταλόγου azure active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**Για να λάβετε το πρόγραμμα-πελάτη εφαρμογής Αναγνωριστικό και το μυστικό κλειδί**

1.  Από τη σελίδα εφαρμογής AD που έχουν δημιουργηθεί πρόσφατα, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων** από το επάνω μενού.
2.  Δημιουργήστε ένα αντίγραφο του **Αναγνωριστικό υπολογιστή-πελάτη**. Θα το χρειαστείτε στην εφαρμογή .NET.
3.  Στην περιοχή **πλήκτρα**, κάντε κλικ στην επιλογή **Επιλέξτε διάρκεια** αναπτυσσόμενη λίστα και επιλέξτε **έτος 1** ή **2 για το έτος**. Η τιμή του κλειδιού δεν θα εμφανίζεται μέχρι να αποθηκεύσετε τη ρύθμιση παραμέτρων.
4.  Κάντε κλικ στην επιλογή " **Αποθήκευση** " στο κάτω μέρος της σελίδας. Όταν εμφανιστεί το μυστικό κλειδί, δημιουργήστε ένα αντίγραφο του αριθμού-κλειδιού. Θα το χρειαστείτε στην εφαρμογή .NET.

##<a name="assign-ad-application-to-role"></a>Εκχώρηση εφαρμογή AD ρόλο

Πρέπει να αντιστοιχίσετε την εφαρμογή σε ένα [ρόλο](../active-directory/role-based-access-built-in-roles.md) για να εκχωρήσετε δικαιώματα για την εκτέλεση ενεργειών σε αυτό. Μπορείτε να ορίσετε το εύρος στο επίπεδο τη συνδρομή, ομάδα πόρων ή πόρου. Τα δικαιώματα μεταβιβάζονται σε χαμηλότερα επίπεδα εμβέλειας (για παράδειγμα, προσθέτοντας μια εφαρμογή ρόλο αναγνώστη για μια ομάδα πόρων σημαίνει να μπορεί να διαβάσει την ομάδα των πόρων και πόροι περιέχει). Σε αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να ορίσετε το εύρος στο επίπεδο ομάδας πόρων.  Επειδή το Azure κλασική πύλη δεν υποστηρίζει ομάδες πόρων, αυτό το τμήμα έχει εκτελεστεί από την πύλη του Azure. 

**Για να προσθέσετε το ρόλο του κατόχου της εφαρμογής AD**

1.  Είσοδος στην [πύλη του Azure](https://portal.azure.com).
2.  Από το αριστερό παράθυρο, κάντε κλικ στην επιλογή " **Ομάδα πόρων** ".
3.  Κάντε κλικ στην ομάδα πόρων που περιέχει το HDInsight σύμπλεγμα όπου έχετε το ερώτημα θα εκτελεστεί Hive παρακάτω σε αυτό το πρόγραμμα εκμάθησης. Εάν υπάρχουν πάρα πολλές ομάδες πόρων, μπορείτε να χρησιμοποιήσετε το φίλτρο.
4.  Κάντε κλικ στην επιλογή **πρόσβαση** από το σύμπλεγμα blade.

    ![εικονίδιο cloud και thunderbolt = γρήγορη έναρξη](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  Κάντε κλικ στην επιλογή **Προσθήκη** από το blade **χρήστες** .
6.  Ακολουθήστε τις οδηγίες για να προσθέσετε το ρόλο του **κατόχου** της εφαρμογής AD που δημιουργήσατε στην τελευταία διαδικασία. Όταν ολοκληρωθεί με επιτυχία, θα βλέπετε την εφαρμογή που αναφέρονται σε το blade χρήστες με το ρόλο του κατόχου.


##<a name="develop-hdinsight-client-application"></a>Ανάπτυξη εφαρμογής HDInsight προγράμματος-πελάτη

Δημιουργία μιας εφαρμογής κονσόλας C# .net ακολουθώντας τις οδηγίες που υπάρχουν στις [εργασίες υποβολή Hadoop στο HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). Στη συνέχεια, αντικαταστήστε τη μέθοδο GetTokenCloudCredentials με τα εξής:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

Για να ανακτήσετε το Αναγνωριστικό μισθωτή μέσω του PowerShell:

    Get-AzureRmSubscription

Εναλλακτικά, Azure CLI:

    azure account show --json

      
## <a name="see-also"></a>Δείτε επίσης

- [Υποβολή Hadoop εργασιών στο HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Δημιουργία εφαρμογής υπηρεσίας καταλόγου Active Directory και υπηρεσία κεφάλαιο με πύλη](../resource-group-create-service-principal-portal.md)
- [Ο έλεγχος ταυτότητας υπηρεσίας κεφάλαιο με τη διαχείριση πόρων Azure](../resource-group-authenticate-service-principal.md)
- [Έλεγχος πρόσβασης βάσει ρόλων Azure](../active-directory/role-based-access-control-configure.md)
