<properties
    pageTitle="Πώς μπορείτε να εργαστείτε με το διακομιστή υποστήριξης .NET SDK για τις εφαρμογές του Mobile | Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς μπορείτε να εργαστείτε με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure εφαρμογής υπηρεσίας."
    keywords="εφαρμογή υπηρεσίας, azure εφαρμογής υπηρεσίας, εφαρμογή για κινητές συσκευές, υπηρεσία κινητών τηλεφώνων, κλίμακα, με δυνατότητα κλιμάκωσης, ανάπτυξη, azure εφαρμογή ανάπτυξης εφαρμογής"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Εργασία με το διακομιστή παρασκηνίου .NET SDK για εφαρμογές του Mobile Azure

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε το διακομιστή παρασκηνίου .NET SDK σε πλήκτρο Mobile υπηρεσίας εφαρμογών Azure σενάρια. Στο SDK εφαρμογές του Mobile Azure σάς βοηθά να εργαστείτε με προγράμματα-πελάτες κινητού από την εφαρμογή σας ASP.NET.

>[AZURE.TIP] Το [.NET διακομιστή SDK για εφαρμογές του Mobile Azure] [ 2] είναι ανοιχτό προέλευσης σε GitHub. Το αρχείο φύλαξης περιέχει όλα πηγαίου κώδικα, όπως την οικογένεια ολόκληρο το διακομιστή SDK μονάδα δοκιμής και ορισμένα δείγματα έργα.

## <a name="reference-documentation"></a>Τεκμηρίωση αναφοράς

Η τεκμηρίωση αναφοράς για το διακομιστή SDK βρίσκεται εδώ: [Αναφορά .NET εφαρμογές του Mobile Azure][1].

## <a name="create-app"></a>Πώς μπορείτε να: Δημιουργήστε μια εφαρμογή Mobile .NET παρασκηνίου

Εάν ξεκινάτε ένα νέο έργο, μπορείτε να δημιουργήσετε μια εφαρμογή εφαρμογής υπηρεσίας χρησιμοποιώντας την [πύλη του Azure] ή Visual Studio. Μπορείτε να εκτελέσετε τοπικά της εφαρμογής υπηρεσίας εφαρμογών ή να δημοσιεύσετε το έργο σας βασίζεται στο cloud εφαρμογής υπηρεσίας εφαρμογής για κινητές συσκευές.  

Εάν προσθέτετε κινητή δυνατότητες σε ένα υπάρχον έργο, ανατρέξτε στην ενότητα [λήψη και προετοιμασία του SDK](#install-sdk) .

### <a name="create-a-net-backend-using-the-azure-portal"></a>Δημιουργήστε έναν υπολογιστή στο παρασκήνιο .NET με την πύλη Azure

Για τη δημιουργία μιας εφαρμογής υπηρεσίας κινητών παρασκηνίου, είτε ακολουθήστε το [πρόγραμμα εκμάθησης γρήγορη έναρξη] [ 3] ή ακολουθήστε τα παρακάτω βήματα:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Πίσω στο blade _Γρήγορα αποτελέσματα_ , στην περιοχή **Δημιουργία πίνακα API**, επιλέξτε **C#** ως **γλώσσα παρασκηνίου**. Κάντε κλικ στο κουμπί **λήψη**, εξαγάγετε τα αρχεία συμπιεσμένα έργου στον τοπικό υπολογιστή σας και να ανοίξετε τη λύση στο Visual Studio.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Δημιουργήστε έναν υπολογιστή στο παρασκήνιο .NET με χρήση του Visual Studio 2013 και του Visual Studio 2015

Εγκατάσταση του [SDK Azure για το .NET] [ 4] (έκδοση 2.9.0 ή νεότερη έκδοση) για να δημιουργήσετε ένα έργο εφαρμογές του Mobile Azure στο Visual Studio. Αφού έχετε εγκαταστήσει το SDK, δημιουργήστε μια εφαρμογή του ASP.NET ακολουθώντας τα παρακάτω βήματα:

1. Ανοίξτε το παράθυρο διαλόγου **Νέο έργο** (από *αρχείο* > **Δημιουργία** > **έργου...**).
2. Ανάπτυξη **προτύπων** > **Visual C#**και επιλέξτε **Web**.
3. Επιλέξτε **εφαρμογή ASP.NET Web**.
4. Συμπληρώστε το όνομα του έργου. Στη συνέχεια, κάντε κλικ στο **κουμπί OK**.
5. Στην περιοχή _ASP.NET 4.5.2 πρότυπα_, επιλέξτε **Εφαρμογή Mobile Azure**. Επιλέξτε **κεντρικού υπολογιστή στο cloud** για να δημιουργήσετε έναν φορητό υπολογιστή στο παρασκήνιο στο cloud στο οποίο μπορείτε να δημοσιεύσετε αυτό το έργο.
6. Κάντε κλικ στο **κουμπί OK**.

## <a name="install-sdk"></a>Πώς μπορείτε να: λήψη και προετοιμασία του SDK

Το SDK είναι διαθέσιμη στο [NuGet.org]. Αυτό το πακέτο περιλαμβάνει τη βασική λειτουργικότητα που απαιτείται για να ξεκινήσετε τη χρήση του SDK. Η προετοιμασία του SDK, πρέπει να εκτελέσετε ενέργειες στο αντικείμενο **HttpConfiguration** .

### <a name="install-the-sdk"></a>Εγκατάσταση του SDK

Για να εγκαταστήσετε το SDK, κάντε δεξί κλικ στο έργο διακομιστή στο Visual Studio, επιλέξτε **Διαχείριση πακέτων NuGet**, αναζητήστε το πακέτο [Microsoft.Azure.Mobile.Server] και κατόπιν κάντε κλικ στην επιλογή **εγκατάσταση**.

###<a name="server-project-setup"></a>Προετοιμασία του project server

Ένα έργο διακομιστή παρασκηνίου .NET έχει προετοιμαστεί παρόμοια με άλλα έργα ASP.NET, συμπεριλαμβάνοντας μια κλάση εκκίνησης OWIN. Βεβαιωθείτε ότι έχετε αναφέρεται το πακέτο NuGet `Microsoft.Owin.Host.SystemWeb`. Για να προσθέσετε αυτήν την κλάση στο Visual Studio, κάντε δεξί κλικ στο έργο σας διακομιστή και επιλέξτε **Προσθήκη** > 
**Νέο στοιχείο**και στη συνέχεια **Web** > **Γενικά** > **κλάση OWIN εκκίνησης**.  Δημιουργείται ένα εκπαιδευτικό με το ακόλουθο χαρακτηριστικό:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

Στο το `Configuration()` μέθοδο τάξη σας εκκίνησης OWIN, χρησιμοποιήστε ένα αντικείμενο **HttpConfiguration** για τη ρύθμιση παραμέτρων του περιβάλλοντος εφαρμογές του Mobile Azure.
Το παρακάτω παράδειγμα προετοιμάζει το project server με χωρίς πρόσθετες δυνατότητες:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Για να ενεργοποιήσετε τις μεμονωμένες δυνατότητες, πρέπει να καλέσετε μεθόδους επέκταση του αντικειμένου **MobileAppConfiguration** πριν από την κλήση **ApplyTo**. Για παράδειγμα, ο ακόλουθος κώδικας προσθέτει τις προεπιλεγμένες διαδρομές σε όλους τους ελεγκτές API που έχουν το χαρακτηριστικό `[MobileAppController]` κατά την προετοιμασία:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

Η γρήγορη έναρξη διακομιστή από την πύλη του Azure κλήσεις **UseDefaultConfiguration()**. Αυτό είναι ισοδύναμη με τις παρακάτω ρυθμίσεις:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

Οι μέθοδοι επέκταση χρησιμοποιούνται είναι:

* `AddMobileAppHomeController()`παρέχει προεπιλεγμένες εφαρμογές του Mobile Azure κεντρική σελίδα.
* `MapApiControllers()`παρέχει προσαρμοσμένες δυνατότητες API για ελεγκτές WebAPI από διακοσμημένου με το `[MobileAppController]` χαρακτηριστικό.
* `AddTables()`παρέχει μια αντιστοίχιση το `/tables` τελικών σημείων ελεγκτές πίνακα.
* `AddTablesWithEntityFramework()`είναι μια σύντομη-χέρι για την αντιστοίχιση του `/tables` τελικά σημεία χρησιμοποιώντας πλαίσιο οντότητα βάσει ελεγκτές.
* `AddPushNotifications()`παρέχει μια απλή μέθοδο εγγραφής συσκευές για διανομείς ειδοποίησης.
* `MapLegacyCrossDomainController()`παρέχει τυπική CORS κεφαλίδες για τοπική ανάπτυξη.

### <a name="sdk-extensions"></a>Επεκτάσεις SDK

Τα ακόλουθα πακέτα επέκτασης που βασίζεται σε NuGet παρέχει διάφορες δυνατότητες για φορητές συσκευές που μπορούν να χρησιμοποιηθούν από την εφαρμογή σας. Μπορείτε να ενεργοποιήσετε επεκτάσεις κατά την προετοιμασία χρησιμοποιώντας το αντικείμενο **MobileAppConfiguration** .

- [Microsoft.Azure.Mobile.Server.Quickstart] υποστηρίζει το βασικής ρύθμισης εφαρμογές του Mobile. Προσθήκη στη ρύθμιση παραμέτρων καλώντας τη μέθοδο επέκτασης **UseDefaultConfiguration** κατά την προετοιμασία. Αυτή η επέκταση περιλαμβάνει τα ακόλουθα επεκτάσεις: ειδοποιήσεις, έλεγχος ταυτότητας, οντότητα, πίνακες, μεταξύ τομέων και πακέτα κεντρική. Αυτό το πακέτο χρησιμοποιείται από το κινητό εφαρμογές γρήγορη έναρξη διαθέσιμο στην πύλη του Azure.

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) 
   υλοποιεί την προεπιλεγμένη *αυτό εφαρμογής για κινητές συσκευές είναι στη σελίδα* για τον ριζικό κατάλογο της τοποθεσίας web. Προσθέστε τη ρύθμιση παραμέτρων καλώντας τη μέθοδο επέκτασης   **AddMobileAppHomeController** .

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) 
   περιλαμβάνει κλάσεις για εργασία με δεδομένα και συνόλων προς τα επάνω της διοχέτευσης δεδομένων. Προσθέστε τη ρύθμιση παραμέτρων καλώντας τη μέθοδο επέκτασης **AddTables** .

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) 
   ενεργοποιεί το πλαίσιο οντότητα για πρόσβαση σε δεδομένα στη βάση δεδομένων SQL. Προσθέστε τη ρύθμιση παραμέτρων καλώντας τη μέθοδο επέκτασης **AddTablesWithEntityFramework** .

- [Microsoft.Azure.Mobile.Server.Authentication] ενεργοποιεί τον έλεγχο ταυτότητας και τα σύνολα προς τα επάνω το ενδιάμεσο OWIN που χρησιμοποιούνται για την επικύρωση διακριτικά. Προσθήκη στη ρύθμιση παραμέτρων, καλώντας την **AddAppServiceAuthentication**  
   και **IAppBuilder**. Μέθοδοι επέκταση **UseAppServiceAuthentication** .

- [Microsoft.Azure.Mobile.Server.Notifications] παρέχει τη δυνατότητα ειδοποιήσεις push και ορίζει ένα τελικό σημείο καταχώρησης push. Προσθέστε τη ρύθμιση παραμέτρων καλώντας τη μέθοδο επέκτασης **AddPushNotifications** .

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) 
   δημιουργεί έναν ελεγκτή που χρησιμοποιείται δεδομένων για προγράμματα περιήγησης web παλαιού τύπου από την εφαρμογή Mobile. Προσθέστε τη ρύθμιση παραμέτρων καλώντας τη μέθοδο επέκτασης   **MapLegacyCrossDomainController** .

- [Microsoft.Azure.Mobile.Server.Login] παρέχει τη μέθοδο AppServiceLoginHandler.CreateToken(), η οποία είναι μια στατική μέθοδος χρησιμοποιείται κατά τη διάρκεια σενάρια προσαρμοσμένου ελέγχου ταυτότητας.   

## <a name="publish-server-project"></a>Πώς μπορείτε να: δημοσιεύστε το έργο διακομιστή

Αυτή η ενότητα σας δείχνει πώς μπορείτε να δημοσιεύσετε το έργο σας .NET παρασκηνίου από το Visual Studio. Μπορείτε επίσης να αναπτύξετε το έργο παρασκηνίου χρησιμοποιώντας Git ή οποιαδήποτε από τις άλλες μεθόδους που αναφέρονται στην [τεκμηρίωση των υπηρεσιών εφαρμογής Azure ανάπτυξης](../app-service-web/web-sites-deploy.md).

1. Στο Visual Studio, εκ νέου δημιουργία του έργου για να επαναφέρετε τα πακέτα NuGet.

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο, κάντε κλικ στο κουμπί **Δημοσίευση**. Την πρώτη φορά που δημοσιεύετε, πρέπει να ορίσετε ένα προφίλ δημοσίευσης. Όταν έχετε ήδη ένα προφίλ που ορίζονται από το, μπορείτε να επιλέξτε το και κάντε κλικ στην επιλογή **Δημοσίευση**.

2. Εάν σας ζητηθεί να επιλέξετε μια δημοσίευση προορισμού, κάντε κλικ στην επιλογή **Microsoft Azure εφαρμογής υπηρεσίας** > **επόμενη**, στη συνέχεια, (εάν χρειάζεται) είσοδος με Azure τα διαπιστευτήριά σας. 
   Visual Studio στοιχεία λήψης και με ασφάλεια αποθηκεύει τις ρυθμίσεις απευθείας από το Azure δημοσίευσης.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)

3. Επιλέξτε τη **συνδρομή**σας, επιλέξτε **Τύπος πόρου** από την **Προβολή**, αναπτύξτε το στοιχείο **Εφαρμογή Mobile**, και κάντε κλικ στην επιλογή σας παρασκηνίου εφαρμογή Mobile και κατόπιν κάντε κλικ στο κουμπί **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)

4. Επιβεβαιώστε τις πληροφορίες προφίλ δημοσίευση και κάντε κλικ στο κουμπί **Δημοσίευση**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Όταν παρασκηνίου σας εφαρμογή Mobile δημοσίευσε με επιτυχία, μπορείτε να δείτε μια σελίδα υποδοχής που υποδεικνύει την επιτυχία.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

##<a name="define-table-controller"></a>Πώς μπορείτε να: Ορισμός ενός ελεγκτή πίνακα

Ορισμός ενός ελεγκτή πίνακα για να εμφανίσετε έναν πίνακα SQL για κινητές συσκευές.  Ρύθμιση των παραμέτρων ενός ελεγκτή πίνακα απαιτεί τρία βήματα:

1. Δημιουργία μιας κλάσης αντικειμένου μεταφορά δεδομένων (DTO).
2. Ρύθμιση παραμέτρων σε μια αναφορά πίνακα στην τάξη Mobile DbContext.
3. Δημιουργήστε έναν πίνακα ελεγκτή.

Μια βιβλιοθήκη δεδομένων μεταφορά αντικειμένου (DTO) είναι ένα απλό C# αντικείμενο που μεταβιβάζονται από `EntityData`.  Για παράδειγμα:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

Το DTO χρησιμοποιείται για να ορίσετε τον πίνακα μέσα στη βάση δεδομένων SQL.  Για να δημιουργήσετε την εγγραφή βάσης δεδομένων, να προσθέσετε μια `DbSet<>` ιδιότητα που θα το DbContext που χρησιμοποιείτε.  Στο project προεπιλεγμένο πρότυπο για τις εφαρμογές του Mobile Azure, που ονομάζεται τα DbContext `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Εάν έχετε το SDK Azure εγκατασταθεί, μπορείτε πλέον να δημιουργήσετε ένα πρότυπο ελεγκτή πίνακα ως εξής:

1. Κάντε δεξί κλικ στο φάκελο ελεγκτές και επιλέξτε **Προσθήκη** > **ελεγκτή...**.
2. Ενεργοποιήστε την επιλογή **Ελεγκτή πίνακα εφαρμογές του Mobile Azure** και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.
3. Στο παράθυρο διαλόγου **Προσθήκη ελεγκτή** :
    * Στην αναπτυσσόμενη λίστα **κλάση μοντέλου** , επιλέξτε το νέο DTO.
    * Στην αναπτυσσόμενη λίστα **DbContext** , επιλέξτε την κλάση DbContext υπηρεσία Mobile.
    * Το όνομα του ελεγκτή δημιουργείται για εσάς.
4. Κάντε κλικ στην επιλογή **Προσθήκη**.

Το project server γρήγορη έναρξη περιέχει ένα παράδειγμα για μια απλή **TodoItemController**.

### <a name="how-to-adjust-the-table-paging-size"></a>Πώς μπορείτε να: Προσαρμόστε το μέγεθος του πίνακα σελιδοποίησης

Από προεπιλογή, οι εφαρμογές του Mobile Azure επιστρέφει 50 εγγραφές ανά αίτηση.  Σελιδοποίησης εξασφαλίζει ότι ο υπολογιστής-πελάτης δεν συνδέουν προς τα επάνω τους νήμα περιβάλλοντος εργασίας Χρήστη ούτε ο διακομιστής για μεγάλο χρονικό διάστημα, εξασφαλίζοντας καλή εμπειρία χρήστη. Για να αλλάξετε το μέγεθος του πίνακα σελιδοποίησης, αύξηση στην πλευρά του διακομιστή "επιτρεπόμενο μέγεθος ερωτήματος" και τη σελίδα πλευρά του προγράμματος-πελάτη του μεγέθους στην πλευρά του διακομιστή "επιτρεπόμενο μέγεθος ερωτήματος" προσαρμόζεται χρησιμοποιώντας το `EnableQuery` χαρακτηριστικό:

    [EnableQuery(PageSize = 500)]

Βεβαιωθείτε ότι το PageSize είναι η ίδια ή μεγαλύτερο από το μέγεθος που ζητήθηκε από το πρόγραμμα-πελάτη.  Ανατρέξτε στο συγκεκριμένο υπολογιστή-πελάτη ΔΙΑΔΙΚΑΣΙΕΣ τεκμηρίωση για λεπτομέρειες σχετικά με την αλλαγή του μεγέθους σελίδας προγράμματος-πελάτη.

## <a name="how-to-define-a-custom-api-controller"></a>Πώς μπορείτε να: Ορισμός ένα προσαρμοσμένο ελεγκτή API

Το προσαρμοσμένο ελεγκτή API παρέχει τις πιο βασικές λειτουργίες για την εφαρμογή Mobile παρασκηνίου, εκθέσετε τελικού σημείου. Μπορείτε να καταχωρήσετε έναν ελεγκτή API mobile συγκεκριμένο χρησιμοποιώντας το χαρακτηριστικό [MobileAppController]. Το `MobileAppController` χαρακτηριστικό καταχωρεί τη διαδρομή, ρυθμίζει ο σειριοποιητής JSON εφαρμογές του Mobile και ενεργοποιεί [τον έλεγχο έκδοση προγράμματος-πελάτη](app-service-mobile-client-and-server-versioning.md).

1. Στο Visual Studio, κάντε δεξί κλικ στο φάκελο ελεγκτές, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη** > **ελεγκτή**, επιλέξτε **ελεγκτή 2 API Web&mdash;κενή** και κάντε κλικ στην επιλογή **Προσθήκη**.

2. Ορίστε ένα **Όνομα ελεγκτή**, όπως είναι οι `CustomController`, και κάντε κλικ στην επιλογή **Προσθήκη**.

3. Στο νέο αρχείο κλάσης ελεγκτή, προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση:

        using Microsoft.Azure.Mobile.Server.Config;

4. Εφαρμόστε το χαρακτηριστικό **[MobileAppController]** στον ορισμό κλάσης ελεγκτή API, όπως στο ακόλουθο παράδειγμα:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }

4. Στο αρχείο App_Start/Startup.MobileApp.cs, προσθέστε μια κλήση για να τη μέθοδο επέκτασης **MapApiControllers** , όπως στο ακόλουθο παράδειγμα:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Μπορείτε επίσης να χρησιμοποιήσετε το `UseDefaultConfiguration()` αντί για τη μέθοδο επέκτασης `MapApiControllers()`. Οποιονδήποτε ελεγκτή ο οποίος δεν έχει εφαρμοστεί **MobileAppControllerAttribute** εξακολουθεί να είναι δυνατή η πρόσβαση από προγράμματα-πελάτες, αλλά αυτό μπορεί να δεν είναι σωστά που καταναλώνεται από προγράμματα-πελάτες χρησιμοποιώντας οποιοδήποτε πρόγραμμα-πελάτη εφαρμογή Mobile SDK.

## <a name="how-to-work-with-authentication"></a>Πώς μπορείτε να: εργασία με έλεγχο ταυτότητας

Azure εφαρμογές του Mobile χρησιμοποιεί έλεγχο ταυτότητας εφαρμογής υπηρεσίας / εξουσιοδότησης για την ασφάλιση σας κινητής παρασκηνίου.  Αυτή η ενότητα σας δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες που σχετίζονται με τον έλεγχο ταυτότητας στο έργο σας .NET παρασκηνίου στο διακομιστή:

+ [Πώς μπορείτε να: Προσθήκη ελέγχου ταυτότητας σε ένα έργο διακομιστή](#add-auth)
+ [Πώς μπορείτε να: χρήση προσαρμοσμένου ελέγχου ταυτότητας για την εφαρμογή σας](#custom-auth)
+ [Πώς μπορείτε να: ανάκτηση ελεγχθεί η ταυτότητά πληροφοριών χρήστη](#user-info)
+ [Πώς μπορείτε να: περιορισμός πρόσβασης δεδομένα για τους εξουσιοδοτημένους χρήστες](#authorize)

### <a name="add-auth"></a>Πώς μπορείτε να: Προσθήκη ελέγχου ταυτότητας σε ένα έργο διακομιστή

Μπορείτε να προσθέσετε τον έλεγχο ταυτότητας στο έργο σας διακομιστή με επέκταση του αντικειμένου **MobileAppConfiguration** και ρύθμιση παραμέτρων ενδιάμεσο OWIN. Κατά την εγκατάσταση του πακέτου [Microsoft.Azure.Mobile.Server.Quickstart] και καλέστε τη μέθοδο επέκτασης **UseDefaultConfiguration** , μπορείτε να μεταβείτε στο βήμα 3.

1. Στο Visual Studio, εγκαταστήστε το πακέτο [Microsoft.Azure.Mobile.Server.Authentication] .

2. Στο αρχείο έργου Startup.cs, προσθέστε την ακόλουθη γραμμή του κώδικα στην αρχή της μεθόδου **ρύθμισης παραμέτρων** :

        app.UseAppServiceAuthentication(config);

    Αυτό το στοιχείο ενδιάμεσο OWIN επικυρώσει τα διακριτικά εκδοθεί από τη συσχετισμένη πύλη εφαρμογής υπηρεσίας.

3. Προσθήκη του `[Authorize]` χαρακτηριστικών σε οποιαδήποτε ελεγκτή ή τη μέθοδο που απαιτεί έλεγχο ταυτότητας. 

Για να μάθετε σχετικά με τον τρόπο για τον έλεγχο ταυτότητας προγράμματα-πελάτες για τις εφαρμογές του Mobile παρασκηνίου, ανατρέξτε στο θέμα [Προσθήκη ελέγχου ταυτότητας για την εφαρμογή σας](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Πώς μπορείτε να: χρήση προσαρμοσμένου ελέγχου ταυτότητας για την εφαρμογή σας

Εάν δεν θέλετε να χρησιμοποιήσετε μία από τις υπηρεσίες παροχής υπηρεσίας εφαρμογής ελέγχου ταυτότητας/εξουσιοδότησης, μπορείτε να υλοποιήσετε το δικό σας σύστημα login. Εγκαταστήστε το πακέτο [Microsoft.Azure.Mobile.Server.Login] για να σας βοηθήσει με έλεγχο ταυτότητας διακριτικού γενιάς.  Παρέχει το δικό σας κώδικα για την επικύρωση διαπιστευτήρια χρήστη. Για παράδειγμα, ενδέχεται να μπορείτε να ελέγξετε σε σχέση με αλατισμένα και διαγραμμισμένο τους κωδικούς πρόσβασης σε μια βάση δεδομένων. Στο παρακάτω παράδειγμα, το `isValidAssertion()` μέθοδο (ορίζεται σε κάποιο άλλο σημείο) είναι υπεύθυνος για αυτούς τους ελέγχους.

Ο έλεγχος ταυτότητας προσαρμοσμένο εκτίθεται, δημιουργώντας μια ApiController και εκθέσετε `register` και `login` ενέργειες. Ο υπολογιστής-πελάτης πρέπει να χρησιμοποιήσετε ένα προσαρμοσμένο περιβάλλον Εργασίας για τη συλλογή των πληροφοριών από το χρήστη.  Στη συνέχεια, υποβάλλεται τις πληροφορίες για το API με μια τυπική κλήση HTTP POST. Όταν ο διακομιστής επικυρώσει τη διεκδίκηση, ενός διακριτικού εκδίδεται χρησιμοποιώντας το `AppServiceLoginHandler.CreateToken()` μέθοδο.  Η χρήση **δεν θα πρέπει να** ApiController το `[MobileAppController]` χαρακτηριστικό. 

Παράδειγμα `login` ενέργεια:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

Στο προηγούμενο παράδειγμα, LoginResult και LoginResultUser είναι σειριοποιημένα αντικείμενα εκθέσετε απαιτούμενες ιδιότητες. Ο υπολογιστής-πελάτης αναμένει απαντήσεις login να επιστρέφονται ως αντικείμενα JSON της φόρμας:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

Το `AppServiceLoginHandler.CreateToken()` μέθοδος περιλαμβάνει ένα _ακροατήριο_ και μια παράμετρο _εκδότη_ . Και τα δύο από αυτές τις παραμέτρους ορίζονται για τη διεύθυνση URL από το ριζικό κατάλογο της εφαρμογής, χρησιμοποιώντας το σχήμα HTTPS. Ομοίως, θα πρέπει να ορίσετε _secretKey_ να είναι η τιμή της εφαρμογής σας υπογραφής αριθμού-κλειδιού. Να μην γίνει κατανομή το κλειδί υπογραφής σε ένα πρόγραμμα-πελάτη που μπορεί να χρησιμοποιηθεί για να μέντα πλήκτρα και μίμηση χρήστες. Μπορείτε να αποκτήσετε το κλειδί υπογραφής ενώ φιλοξενούνται σε εφαρμογής υπηρεσίας με την αναφορά του _τοποθεσία Web\_AUTH\_ΥΠΟΓΡΑΦΉ\_ΚΛΕΙΔΊ_ μεταβλητή περιβάλλοντος. Εάν είναι απαραίτητο, σε ένα τοπικό περιβάλλον εντοπισμού σφαλμάτων, ακολουθήστε τις οδηγίες στην ενότητα [τοπικό εντοπισμού με έλεγχο ταυτότητας](#local-debug) για να ανακτήσετε τον αριθμό-κλειδί και να αποθηκεύσετε ως μια ρύθμιση εφαρμογής.

Το διακριτικό έκδοσης ενδέχεται να περιλαμβάνουν άλλες απαιτήσεις και ημερομηνία λήξης.  Ελάχιστες, το διακριτικό έκδοσης πρέπει να συμπεριλάβετε ένα αίτημα θέμα (**sub**).

Μπορεί να υποστηρίξει το τυπικό πρόγραμμα-πελάτη `loginAsync()` μέθοδο από υπερφόρτωσης τη διαδρομή ελέγχου ταυτότητας.  Εάν ο υπολογιστής-πελάτης καλεί `client.loginAsync('custom');` για να συνδεθείτε στο, πρέπει να σας δρομολόγηση `/.auth/login/custom`.  Μπορείτε να ορίσετε τη διαδρομή για το προσαρμοσμένο έλεγχο ταυτότητας με χρήση ελεγκτή `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

>[AZURE.TIP] Χρήση του `loginAsync()` προσέγγιση εξασφαλίζει ότι το διακριτικό ελέγχου ταυτότητας είναι συνδεδεμένη με κάθε οι επόμενες κλήση στην υπηρεσία.

###<a name="user-info"></a>Πώς μπορείτε να: ανάκτηση ελεγχθεί η ταυτότητά πληροφοριών χρήστη

Όταν ένας χρήστης έχει γίνει έλεγχος ταυτότητας με εφαρμογή υπηρεσίας, μπορείτε να αποκτήσετε πρόσβαση το Αναγνωριστικό χρήστη που έχει αντιστοιχιστεί και άλλες πληροφορίες στον κώδικα παρασκηνίου .NET. Οι πληροφορίες χρήστη μπορεί να χρησιμοποιηθεί για τη λήψη αποφάσεων εξουσιοδότησης στο υπόβαθρο. Ο ακόλουθος κώδικας λαμβάνει το Αναγνωριστικό χρήστη που σχετίζεται με μια πρόσκληση σε:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

Το αναγνωριστικό ΑΣΦΑΛΕΊΑΣ προέρχεται από το Αναγνωριστικό χρήστη υπηρεσίας παροχής και είναι στατική για ένα συγκεκριμένο χρήστη και την υπηρεσία παροχής σύνδεσης.  Το αναγνωριστικό ΑΣΦΑΛΕΊΑΣ είναι null για κωδικοί ελέγχου ταυτότητας δεν είναι έγκυρη.

Εφαρμογή υπηρεσίας σάς επιτρέπει επίσης να ζητήσετε συγκεκριμένες αξιώσεων από την υπηρεσία παροχής σύνδεσης. Κάθε υπηρεσία παροχής ταυτότητας μπορούν να παρέχουν περισσότερες πληροφορίες χρησιμοποιώντας την υπηρεσία παροχής ταυτότητας SDK.  Για παράδειγμα, μπορείτε να χρησιμοποιήσετε το API Graph Facebook για τους φίλους σας πληροφορίες.  Μπορείτε να καθορίσετε αξιώσεων που ζητούνται στο την υπηρεσία παροχής blade στην πύλη του Azure. Ορισμένες αξιώσεων απαιτούν επιπλέον ρύθμιση παραμέτρων με την υπηρεσία παροχής ταυτότητας.

Ο ακόλουθος κώδικας καλεί τη μέθοδο επέκτασης **GetAppServiceIdentityAsync** για να λάβετε τα διαπιστευτήρια σύνδεσης, το οποίο περιλαμβάνει το διακριτικό πρόσβασης που χρειάζεται να κάνετε προσκλήσεις σε σχέση με το API Graph Facebook:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Να προσθέσετε ένα χρησιμοποιώντας δήλωση για `System.Security.Principal` για την παροχή της μεθόδου επέκταση **GetAppServiceIdentityAsync** .

### <a name="authorize"></a>Πώς μπορείτε να: περιορισμός πρόσβασης δεδομένα για τους εξουσιοδοτημένους χρήστες

Στην προηγούμενη ενότητα, θα σας εμφάνιζε πώς μπορείτε να ανακτήσετε το Αναγνωριστικό χρήστη του χρήστη με έλεγχο ταυτότητας. Μπορείτε να περιορίσετε την πρόσβαση σε δεδομένα και άλλοι πόροι που βασίζονται σε αυτή την τιμή. Για παράδειγμα, προσθέτοντας μια στήλη αναγνωριστικό χρήστη σε πίνακες και το φιλτράρισμα τα αποτελέσματα του ερωτήματος με το Αναγνωριστικό χρήστη είναι ένας απλός τρόπος για να περιορίσετε τα δεδομένα που επιστρέφονται μόνο σε εξουσιοδοτημένους χρήστες. Ο ακόλουθος κώδικας επιστρέφει γραμμές δεδομένων μόνο όταν το αναγνωριστικό ΑΣΦΑΛΕΊΑΣ συμφωνεί με την τιμή της στήλης αναγνωριστικό χρήστη του πίνακα TodoItem:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

Το `Query()` μέθοδος επιστρέφει μια `IQueryable` που μπορεί να χειριστεί LINQ να χειρίζεται το φιλτράρισμα.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Πώς μπορείτε να: Προσθήκη push ειδοποιήσεων για ένα έργο διακομιστή

Προσθέστε τις ειδοποιήσεις push στο έργο σας διακομιστή επέκταση του αντικειμένου **MobileAppConfiguration** και δημιουργώντας ένα πρόγραμμα-πελάτη ειδοποίηση διανομείς.

1. Στο Visual Studio, κάντε δεξί κλικ του project server και κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**, αναζητήστε `Microsoft.Azure.Mobile.Server.Notifications`, στη συνέχεια, κάντε κλικ στην επιλογή **εγκατάσταση**. 

2. Επαναλάβετε αυτό το βήμα για να εγκαταστήσετε το `Microsoft.Azure.NotificationHubs` πακέτο, η οποία περιλαμβάνει τη βιβλιοθήκη προγράμματος-πελάτη ειδοποίηση διανομείς.

3. Στο App_Start/Startup.MobileApp.cs, και προσθέστε μια κλήση προς τη μέθοδο επέκτασης **AddPushNotifications()** κατά την προετοιμασία:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

4. Προσθέστε τον ακόλουθο κώδικα που δημιουργεί ένα πρόγραμμα-πελάτη διανομείς ειδοποίηση:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Τώρα, μπορείτε να χρησιμοποιήσετε το πρόγραμμα-πελάτη διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων push σε καταχωρημένες συσκευές. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Προσθήκη ειδοποιήσεων push για την εφαρμογή σας](app-service-mobile-ios-get-started-push.md). Για να μάθετε περισσότερα σχετικά με την ειδοποίηση διανομείς, ανατρέξτε στο θέμα [Επισκόπηση διανομείς ειδοποίησης](../notification-hubs/notification-hubs-push-notification-overview.md).

##<a name="tags"></a>Πώς μπορείτε να: Ενεργοποίηση στοχευμένες push χρήση ετικετών

Ειδοποίηση διανομείς σάς επιτρέπει να αποστέλλονται ειδοποιήσεις στοχευμένες σε συγκεκριμένο καταχωρήσεων χρησιμοποιώντας τις ετικέτες. Πολλές ετικέτες δημιουργούνται αυτόματα:

* Το Αναγνωριστικό εγκατάστασης προσδιορίζει μια συγκεκριμένη συσκευή.
* Το αναγνωριστικό χρήστη με βάση το αναγνωριστικό ΑΣΦΑΛΕΊΑΣ με έλεγχο ταυτότητας προσδιορίζει έναν συγκεκριμένο χρήστη.

Το Αναγνωριστικό εγκατάστασης είναι δυνατή η πρόσβαση από την ιδιότητα **installationId** στην το **MobileServiceClient**.  Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε ένα Αναγνωριστικό εγκατάστασης για να προσθέσετε μια ετικέτα σε μια συγκεκριμένη εγκατάσταση σε διανομείς ειδοποίηση:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Όλες τις ετικέτες που παρέχονται από το πρόγραμμα-πελάτη κατά την εγγραφή της ειδοποίησης push αγνοούνται από υπολογιστή στο παρασκήνιο, κατά τη δημιουργία της εγκατάστασης. Για να ενεργοποιήσετε ένα πρόγραμμα-πελάτη για να προσθέσετε ετικέτες για την εγκατάσταση, πρέπει να δημιουργήσετε ένα προσαρμοσμένο API που προσθέτει ετικέτες χρησιμοποιώντας το προηγούμενο μοτίβο. 

Ανατρέξτε στο θέμα [ετικέτες ειδοποιήσεων push προστίθενται προγράμματος-πελάτη] [ 5] στο δείγμα εφαρμογών υπηρεσίας κινητών ολοκληρωμένη γρήγορης έναρξης για ένα παράδειγμα.

##<a name="push-user"></a>Πώς μπορείτε να: Αποστολή ειδοποιήσεων push για ένα χρήστη με έλεγχο ταυτότητας

Όταν ένα χρήστη με έλεγχο ταυτότητας καταχωρεί για τις ειδοποιήσεις push, μια ετικέτα Αναγνωριστικού χρήστη προστίθεται αυτόματα για την καταχώρηση. Με αυτήν την ετικέτα, μπορείτε να στείλετε τις ειδοποιήσεις push σε όλες τις συσκευές που έχουν καταχωρηθεί από αυτό το άτομο. Ο ακόλουθος κώδικας λαμβάνει το αναγνωριστικό ΑΣΦΑΛΕΊΑΣ του χρήστη που κάνει το αίτημα και στέλνει μια ειδοποίηση push προτύπου σε κάθε καταχώρηση της συσκευής για το συγκεκριμένο άτομο:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Κατά την εγγραφή για τις ειδοποιήσεις push από ένα πρόγραμμα-πελάτη με έλεγχο ταυτότητας, βεβαιωθείτε ότι ο έλεγχος ταυτότητας έχει ολοκληρωθεί πριν να επιχειρήσετε εγγραφής. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Push στους χρήστες] [ 6] στο δείγμα εφαρμογών υπηρεσίας κινητών ολοκληρωμένη γρήγορης έναρξης για .NET παρασκηνίου.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Πώς μπορείτε να: εντοπισμός σφαλμάτων και αντιμετώπιση προβλημάτων με το διακομιστή .NET SDK

Azure εφαρμογής υπηρεσίας παρέχει διάφορες εντοπισμού και τεχνικές για εφαρμογές ASP.NET αντιμετώπισης προβλημάτων:

- [Παρακολούθηση μιας εφαρμογής υπηρεσίας Azure](../app-service-web/web-sites-monitor.md)
- [Ενεργοποιήσετε την καταγραφή διαγνωστικών στο Azure εφαρμογής υπηρεσίας](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Αντιμετώπιση προβλημάτων μιας εφαρμογής υπηρεσίας Azure στο Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Καταγραφή

Μπορείτε να γράψετε σε αρχεία καταγραφής διαγνωστικών εφαρμογής υπηρεσίας, χρησιμοποιώντας την τυπική συγγραφής ανίχνευση ASP.NET. Πριν να γράφετε στα αρχεία καταγραφής, πρέπει να ενεργοποιήσετε τα Διαγνωστικά στο σας παρασκηνίου εφαρμογή Mobile.

Για να ενεργοποιήσετε Διαγνωστικά και εγγραφής για τα αρχεία καταγραφής:

1. Ακολουθήστε τα βήματα στο θέμα [πώς μπορείτε να ενεργοποιήσετε τα Διαγνωστικά](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).

2. Προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση στο αρχείο σας κώδικα:

        using System.Web.Http.Tracing;

3. Δημιουργήστε ένα πρόγραμμα ανίχνευσης εγγραφής για να γράψετε από υπολογιστή στο παρασκήνιο .NET στα αρχεία καταγραφής διαγνωστικών, ως εξής:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");

4. Αναδημοσίευση του project server, και αποκτήστε πρόσβαση στο υπόβαθρο εφαρμογή Mobile να εκτελέσει τη διαδρομή κώδικα με την καταγραφή.

5. Κάντε λήψη και την αξιολόγησή τα αρχεία καταγραφής, όπως περιγράφεται στο [Τρόπος: κάντε λήψη των αρχείων καταγραφής](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Τοπική εντοπισμού με έλεγχο ταυτότητας

Μπορείτε να εκτελέσετε την εφαρμογή σας τοπικά, για να εξετάσετε τις αλλαγές πριν από τη δημοσίευση τους στο cloud. Για περισσότερες εφαρμογές του Mobile Azure παρασκήνιο, πατήστε το πλήκτρο *F5* ενώ βρίσκεστε στο Visual Studio. Ωστόσο, υπάρχουν ορισμένα επιπλέον ζητήματα κατά τη χρήση του ελέγχου ταυτότητας.

Πρέπει να έχετε βασίζεται στο cloud εφαρμογής για κινητές συσκευές με εφαρμογή υπηρεσίας ελέγχου ταυτότητας/εξουσιοδότησης έχει ρυθμιστεί και το πρόγραμμα-πελάτης πρέπει να έχετε το τελικό σημείο cloud που έχουν καθοριστεί ως τον κεντρικό υπολογιστή του εναλλακτικού login. Ανατρέξτε στην τεκμηρίωση για την πλατφόρμα σας προγράμματος-πελάτη για τα συγκεκριμένα βήματα που απαιτούνται.

Βεβαιωθείτε ότι σας κινητή παρασκηνίου έχει εγκατασταθεί [Microsoft.Azure.Mobile.Server.Authentication] . Στη συνέχεια, στην τάξη εκκίνησης OWIN της εφαρμογής σας, προσθέστε τα εξής, μετά την `MobileAppConfiguration` έχει συσχετιστεί με το `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

Στο προηγούμενο παράδειγμα, θα πρέπει να ρυθμίζετε τις παραμέτρους των ρυθμίσεων της εφαρμογής _authAudience_ και _authIssuer_ εντός του αρχείου Web.config για κάθε είναι η διεύθυνση URL από το ριζικό κατάλογο της εφαρμογής, χρησιμοποιώντας το σχήμα HTTPS. Ομοίως, θα πρέπει να ορίσετε _authSigningKey_ να είναι η τιμή της εφαρμογής σας υπογραφής αριθμού-κλειδιού. Για να λάβετε τον αριθμό-κλειδί υπογραφής:

1. Περιήγηση σε εφαρμογή εντός της [πύλης Azure] 
2. Κάντε κλικ στην επιλογή **Εργαλεία**, **Kudu**, **Μετάβαση**.
3. Στην τοποθεσία του Kudu διαχείρισης, κάντε κλικ στην επιλογή **περιβάλλον**.
4. Εύρεση της τιμής για _τοποθεσία Web\_AUTH\_ΥΠΟΓΡΑΦΉ\_ΚΛΕΙΔΊ_. 

Χρησιμοποιήστε το πλήκτρο υπογραφής για την παράμετρο _authSigningKey_ στο αρχείο config τοπική εφαρμογή.  Τώρα διαθέτει σας παρασκηνίου κινητές συσκευές για να επικυρώσει τα διακριτικά όταν εκτελείται τοπικά, όπου ο υπολογιστής-πελάτης λαμβάνει το διακριτικό από το τελικό σημείο που βασίζεται στο cloud.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Πύλη του Azure]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

