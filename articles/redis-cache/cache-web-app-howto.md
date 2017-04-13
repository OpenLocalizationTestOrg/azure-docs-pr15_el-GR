<properties 
    pageTitle="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή Web με το Redis Cache | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια εφαρμογή Web με Redis Cache" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/11/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-a-web-app-with-redis-cache"></a>Πώς μπορείτε να δημιουργήσετε μια εφαρμογή Web με Redis Cache

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να δημιουργήσετε και να αναπτύξετε μια εφαρμογή web ASP.NET σε μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας με χρήση του Visual Studio 2015. Το δείγμα εφαρμογής εμφανίζει μια λίστα των ομάδων στατιστικών στοιχείων από μια βάση δεδομένων και εμφανίζει τους διάφορους τρόπους για να χρησιμοποιήσετε Azure Redis Cache για την αποθήκευση και ανάκτηση δεδομένων από το cache. Όταν ολοκληρωθεί το πρόγραμμα εκμάθησης, θα έχετε μια ενσωματωμένη εφαρμογή web που διαβάζει και καταγράφει μια βάση δεδομένων, βελτιστοποιημένη με Azure Redis Cache και φιλοξενείται στο Azure.

Θα μάθετε:

-   Μάθετε πώς μπορείτε να δημιουργήσετε μια εφαρμογή web ASP.NET MVC 5 στο Visual Studio.
-   Πώς μπορείτε να αποκτήσετε πρόσβαση σε δεδομένα από μια βάση δεδομένων χρησιμοποιώντας πλαίσιο οντότητα.
-   Πώς μπορείτε να βελτιώσετε τη μεταγωγή δεδομένων και μείωση φόρτου βάσης δεδομένων με την αποθήκευση και ανάκτηση δεδομένων με χρήση Azure Redis Cache.
-   Πώς μπορείτε να χρησιμοποιήσετε μια Redis ταξινομηθεί συνόλου για να ανακτήσετε την επάνω γραμμή 5 τις ομάδες.
-   Μάθετε πώς μπορείτε να παρέχετε το Azure πόρους για την εφαρμογή με τη χρήση προτύπου για τη διαχείριση πόρων.
-   Μάθετε πώς μπορείτε να δημοσιεύσετε την εφαρμογή για χρήση του Visual Studio Azure.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε το πρόγραμμα εκμάθησης, πρέπει να έχετε τις εξής προϋποθέσεις.

-   [Λογαριασμός Azure](#azure-account)
-   [Visual Studio 2015 με το Azure SDK για .NET](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Λογαριασμός Azure

Χρειάζεστε ένα λογαριασμό Azure για να ολοκληρώσετε το πρόγραμμα εκμάθησης. Μπορείς:

* [Ανοίξτε ένα λογαριασμό Azure δωρεάν](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Λαμβάνετε πιστώσεων που μπορούν να χρησιμοποιηθούν για να δοκιμάσετε την πληρωμή Azure υπηρεσίες. Ακόμα και μετά το πιστώσεων χρησιμοποιούνται προς τα επάνω, μπορείτε να διατηρήσετε το λογαριασμό και να χρησιμοποιήσετε δωρεάν Azure υπηρεσίες και τις δυνατότητες.
* [Ενεργοποίηση του Visual Studio συνδρομητών πλεονεκτήματα](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Συνδρομή σας στο MSDN παρέχει πιστώσεων κάθε μήνα που μπορείτε να χρησιμοποιήσετε για τις υπηρεσίες του Azure επί πληρωμή.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>Visual Studio 2015 με το Azure SDK για .NET

Το πρόγραμμα εκμάθησης είναι χειρόγραφων Visual Studio 2015 με το [Azure SDK για .NET](../dotnet-sdk.md) 2.8.2 ή νεότερη έκδοση. [Λήψη την πιο πρόσφατη SDK Azure για το Visual Studio 2015 εδώ](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio εγκαθίσταται αυτόματα με το SDK, εάν δεν το έχετε ήδη.

Εάν διαθέτετε το Visual Studio 2013, μπορείτε να [κάνετε λήψη την πιο πρόσφατη SDK Azure για το Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Ορισμένες οθόνες μπορεί να φαίνεται διαφορετικό από τις εικόνες που εμφανίζονται σε αυτό το πρόγραμμα εκμάθησης.

>[AZURE.NOTE] Ανάλογα με το πόσες από τις εξαρτήσεις SDK που έχετε ήδη στον υπολογιστή σας, η εγκατάσταση του SDK μπορεί να διαρκέσει μεγάλο χρονικό διάστημα, από μερικά λεπτά για να μισή ώρα ή περισσότερα.

## <a name="create-the-visual-studio-project"></a>Δημιουργήστε το έργο Visual Studio

1. Ανοίξτε το Visual Studio και κάντε κλικ στην επιλογή **αρχείο**, **Δημιουργία** **έργου**.

2. Αναπτύξτε τον κόμβο **Visual C#** στη λίστα **πρότυπα** , επιλέξτε **Cloud**και κάντε κλικ στην επιλογή **Εφαρμογή Web ASP.NET**. Βεβαιωθείτε ότι είναι επιλεγμένο το **.NET Framework 4.5.2** .  Πληκτρολογήστε **ContosoTeamStats** στο πλαίσιο κειμένου **όνομα** και κάντε κλικ στο **κουμπί OK**.
 
    ![Δημιουργία έργου][cache-create-project]

3. Επιλέξτε **MVC** ως τον τύπο του έργου. Καταργήστε την επιλογή από το πλαίσιο ελέγχου **κεντρικού υπολογιστή στο cloud** . Θα [προμήθεια Azure πόρων](#provision-the-azure-resources) και να [δημοσιεύσετε την εφαρμογή για να Azure](#publish-the-application-to-azure) επόμενα βήματα στην εκμάθηση. Για ένα παράδειγμα της προετοιμασίας μια εφαρμογή web της εφαρμογής υπηρεσίας από το Visual Studio αφήνοντας επιλεγμένο **κεντρικού υπολογιστή στο cloud** , ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Web Apps στο Azure εφαρμογής υπηρεσίας, χρησιμοποιώντας ASP.NET και του Visual Studio](../app-service-web/web-sites-dotnet-get-started.md).

    ![Επιλέξτε το πρότυπο έργου][cache-select-template]

4. Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο.

## <a name="create-the-aspnet-mvc-application"></a>Δημιουργία της εφαρμογής ASP.NET MVC

Σε αυτήν την ενότητα του προγράμματος εκμάθησης, θα μπορείτε να δημιουργήσετε τη βασική εφαρμογή που διαβάζει και εμφανίζει την ομάδα στατιστικών στοιχείων από μια βάση δεδομένων.

-   [Προσθήκη στο μοντέλο](#add-the-model)
-   [Προσθήκη του ελεγκτή](#add-the-controller)
-   [Ρύθμιση παραμέτρων των προβολών](#configure-the-views)

### <a name="add-the-model"></a>Προσθήκη στο μοντέλο

1. Κάντε δεξί κλικ σε **μοντέλα** σε **Εξερεύνηση λύσεων**και επιλέξτε **Προσθήκη**, **τάξης**. 

    ![Προσθήκη μοντέλου][cache-model-add-class]

2. Πληκτρολογήστε `Team` για την τάξη δώστε ένα όνομα και κάντε κλικ στην επιλογή **Προσθήκη**.

    ![Προσθήκη κλάση μοντέλου][cache-model-add-class-dialog]

3. Αντικαταστήστε το `using` δηλώσεις στο επάνω μέρος του `Team.cs` αρχείο με την εξής χρήση προτάσεων.


        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


4. Αντικαταστήστε τον ορισμό των το `Team` κλάσης με το παρακάτω τμήμα κώδικα που περιέχει μια ενημερωμένη `Team` κλάση ορισμού, καθώς και ορισμένα άλλα κλάσεις βοηθών Framework οντότητα. Για περισσότερες πληροφορίες σχετικά με την πρώτη προσέγγιση κώδικα για Framework οντότητα που χρησιμοποιείται σε αυτό το πρόγραμμα εκμάθησης, ανατρέξτε στο θέμα [κώδικα πρώτα σε μια νέα βάση δεδομένων](https://msdn.microsoft.com/data/jj193542).


        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }
        
            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();
        
                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }
        
        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }
        
            public DbSet<Team> Teams { get; set; }
        }
        
        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };
        
                Team.PlayGames(teams);
        
                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }
        
        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


2. Στην **Εξερεύνηση λύσεων**, κάντε διπλό κλικ στο **αρχείο web.config** για να το ανοίξετε.

    ![Web.config][cache-web-config]

3.  Η ακόλουθη συμβολοσειρά σύνδεσης για να προσθέσετε το `connectionStrings` ενότητας. Το όνομα της συμβολοσειράς σύνδεσης πρέπει να συμφωνεί με το όνομα της κλάσης περιβάλλοντος Framework οντότητα βάσης δεδομένων που είναι `TeamContext`.

        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />


    Μετά την προσθήκη, την `connectionStrings` ενότητα θα πρέπει να μοιάζει με το παρακάτω παράδειγμα.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"  providerName="System.Data.SqlClient" />
        </connectionStrings>

### <a name="add-the-controller"></a>Προσθήκη του ελεγκτή

1. Πατήστε το πλήκτρο **F6** για να δημιουργήσετε το έργο. 
2. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο φάκελο **ελεγκτές** και επιλέξτε **Προσθήκη**, **ελεγκτή**.

    ![Προσθήκη ελεγκτή][cache-add-controller]

3. Επιλέξτε **Ελεγκτή 5 MVC με προβολές, χρησιμοποιώντας πλαίσιο οντότητα**, και κάντε κλικ στην επιλογή **Προσθήκη**. Εάν λάβετε ένα σφάλμα μετά κάνοντας κλικ στην επιλογή **Προσθήκη**, βεβαιωθείτε ότι έχετε δημιουργήσει πρώτα το έργο.

    ![Προσθήκη ελεγκτή τάξης][cache-add-controller-class]

5. Επιλέξτε **την ομάδα (ContosoTeamStats.Models)** από την αναπτυσσόμενη λίστα **κλάση μοντέλου** . Επιλέξτε **TeamContext (ContosoTeamStats.Models)** από την αναπτυσσόμενη λίστα **κλάσης περιβάλλον δεδομένων** . Τύπος `TeamsController` στο πλαίσιο κειμένου όνομα **ελεγκτή** (Εάν αυτό δεν συμπληρώνεται αυτόματα). Κάντε κλικ στο κουμπί **Προσθήκη** για να δημιουργήσετε την κλάση ελεγκτή και να προσθέσετε τις προεπιλεγμένες προβολές.

    ![Ρύθμιση παραμέτρων του ελεγκτή][cache-configure-controller]

4. Στην **Εξερεύνηση λύσεων**, αναπτύξτε **Global.asax** και κάντε διπλό κλικ στο **Global.asax.cs** για να το ανοίξετε.

    ![Global.asax.CS][cache-global-asax]

5. Προσθέστε τις ακόλουθες δύο χρήση προτάσεων στο επάνω μέρος του αρχείου κάτω από την άλλη χρήση προτάσεων.


        using System.Data.Entity;
        using ContosoTeamStats.Models;


6. Προσθέστε την ακόλουθη γραμμή κώδικα στο τέλος του `Application_Start` μέθοδο.


        Database.SetInitializer<TeamContext>(new TeamInitializer());


7. Στην **Εξερεύνηση λύσεων**, αναπτύξτε το στοιχείο `App_Start` και κάντε διπλό κλικ `RouteConfig.cs`.

    ![RouteConfig.cs][cache-RouteConfig-cs]

8. Αντικατάσταση `controller = "Home"` τον παρακάτω κώδικα στο το `RegisterRoutes` μέθοδο με `controller = "Teams"` όπως φαίνεται στο παρακάτω παράδειγμα.


        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### <a name="configure-the-views"></a>Ρύθμιση παραμέτρων των προβολών

1. Στην **Εξερεύνηση λύσεων**, αναπτύξτε το φάκελο **προβολών** και, στη συνέχεια, στο φάκελο **κοινή χρήση** και κάντε διπλό κλικ στο **_Layout.cshtml**. 

    ![_Layout.cshtml][cache-layout-cshtml]

2. Αλλαγή των περιεχομένων του `title` στοιχείο και αντικατάσταση `My ASP.NET Application` με `Contoso Team Stats` όπως φαίνεται στο παρακάτω παράδειγμα.


        <title>@ViewBag.Title - Contoso Team Stats</title>


3. Στο το `body` ενότητας, ενημερώστε το πρώτο `Html.ActionLink` δήλωση και αντικατάσταση `Application name` με `Contoso Team Stats` και να αντικαταστήσετε `Home` με `Teams`.
    -   Πριν:`@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
    -   Μετά:`@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`

    ![Αλλαγές κώδικα][cache-layout-cshtml-code]

4. Πατήστε το **Συνδυασμό πλήκτρων Ctrl + F5** για να δημιουργήσετε και να εκτελέσετε την εφαρμογή. Αυτή η έκδοση της εφαρμογής διαβάζει τα αποτελέσματα απευθείας από τη βάση δεδομένων. Σημείωση η **Δημιουργία**, **Επεξεργασία**, **Λεπτομέρειες**και **Διαγραφή** ενέργειες που έχουν προστεθεί αυτόματα με την εφαρμογή από το scaffold **Ελεγκτή 5 MVC με προβολές, χρησιμοποιώντας πλαίσιο οντότητα** . Στην επόμενη ενότητα του προγράμματος εκμάθησης που θα προσθέσετε Redis Cache για να βελτιστοποιήσετε την πρόσβαση σε δεδομένα και να παρέχουν πρόσθετες δυνατότητες στην εφαρμογή.

![Εφαρμογή Starter][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>Ρύθμιση παραμέτρων της εφαρμογής για να χρησιμοποιήσετε Redis Cache

Σε αυτήν την ενότητα του προγράμματος εκμάθησης, θα μπορείτε να ρυθμίσετε τις παραμέτρους του δείγματος εφαρμογής για την αποθήκευση και ανάκτηση στατιστικά στοιχεία ομάδας Contoso από μια παρουσία Azure Redis Cache χρησιμοποιώντας το πρόγραμμα-πελάτη cache [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .

-   [Ρύθμιση παραμέτρων της εφαρμογής για να χρησιμοποιήσετε StackExchange.Redis](#configure-the-application-to-use-stackexchangeredis)
-   [Ενημερώστε την κλάση TeamsController ώστε να επιστρέφει αποτελέσματα από το cache ή της βάσης δεδομένων](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
-   [Ενημερώστε τις μεθόδους δημιουργία, επεξεργασία και διαγραφή για να εργαστείτε με το cache](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
-   [Ενημέρωση της προβολής ευρετηρίου τις ομάδες για να εργαστείτε με το cache](#update-the-teams-index-view-to-work-with-the-cache)


### <a name="configure-the-application-to-use-stackexchangeredis"></a>Ρύθμιση παραμέτρων της εφαρμογής για να χρησιμοποιήσετε StackExchange.Redis

1. Για να ρυθμίσετε μια εφαρμογή προγράμματος-πελάτη στο Visual Studio, χρησιμοποιώντας το πακέτο StackExchange.Redis NuGet, κάντε δεξί κλικ στο έργο στην **Εξερεύνηση λύσεων** και επιλέξτε **Διαχείριση πακέτων NuGet**. 

    ![Διαχείριση πακέτων NuGet][redis-cache-manage-nuget-menu]

2. Πληκτρολογήστε **StackExchange.Redis** στο πλαίσιο Αναζήτηση κειμένου, επιλέξτε την επιθυμητή έκδοση από τα αποτελέσματα και κάντε κλικ στην επιλογή **εγκατάσταση**.

    ![Το πακέτο StackExchange.Redis NuGet][redis-cache-stack-exchange-nuget]

    Το πακέτο NuGet στοιχεία λήψης και προσθέτει τις αναφορές συγκρότησης που απαιτείται για την εφαρμογή-πελάτη για πρόσβαση στο Azure Redis Cache με το πρόγραμμα-πελάτη cache StackExchange.Redis. Εάν προτιμάτε να χρησιμοποιήσετε μια έκδοση ισχυρό-με το όνομα της βιβλιοθήκης του προγράμματος-πελάτη **StackExchange.Redis** , επιλέξτε **StackExchange.Redis.StrongName**; Διαφορετικά, επιλέξτε **StackExchange.Redis**.

3. Στην **Εξερεύνηση λύσεων**, αναπτύξτε το φάκελο **ελεγκτές** και κάντε διπλό κλικ στο **TeamsController.cs** για να το ανοίξετε.

    ![Ελεγκτή τις ομάδες][cache-teamscontroller]

4. Προσθέστε τις ακόλουθες δύο χρήση προτάσεων για **TeamsController.cs**.

        using System.Configuration;
        using StackExchange.Redis;

5. Προσθέστε τις ακόλουθες δύο ιδιότητες για να το `TeamsController` τάξης.

        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
  
1. Δημιουργήστε ένα αρχείο στον υπολογιστή σας με το όνομα `WebAppPlusCacheAppSecrets.config` και να την τοποθετήσετε σε μια θέση στην οποία θα γίνει μεταβίβαση ελέγχου με τον πηγαίο κώδικα της εφαρμογής σας δείγμα, θα πρέπει να αποφασίσετε να μεταβιβάσετε τον έλεγχό του κάπου. Σε αυτό το παράδειγμα το `AppSettingsSecrets.config` βρίσκεται στο αρχείο `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.

    Επεξεργασία του `WebAppPlusCacheAppSecrets.config` αρχείων και προσθέστε το παρακάτω περιεχόμενο. Εάν εκτελείτε την εφαρμογή τοπικά αυτές οι πληροφορίες χρησιμοποιούνται για να συνδεθείτε στην περίοδο λειτουργίας του Azure Redis Cache. Αργότερα στο πρόγραμμα εκμάθησης που θα προμήθεια παρουσίας Azure Redis Cache και να ενημερώσετε το cache όνομα και τον κωδικό πρόσβασης. Εάν δεν σκοπεύετε να εκτελέσετε το δείγμα εφαρμογής τοπικά μπορείτε να παραλείψετε τη δημιουργία αυτού του αρχείου και τα επόμενα βήματα που αναφέρονται στο αρχείο, επειδή κατά την ανάπτυξη στη Azure η εφαρμογή ανακτά τις πληροφορίες σύνδεσης cache από τη ρύθμιση εφαρμογών για το Web App και όχι από αυτό το αρχείο. Εφόσον το `WebAppPlusCacheAppSecrets.config` δεν έχει αναπτυχθεί σε Azure με την εφαρμογή σας, δεν τη χρειάζεστε, εκτός εάν σκοπεύετε να εκτελέσετε την εφαρμογή τοπικά.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


2. Στην **Εξερεύνηση λύσεων**, κάντε διπλό κλικ στο **αρχείο web.config** για να το ανοίξετε.

    ![Web.config][cache-web-config]

3. Προσθέστε τα ακόλουθα `file` χαρακτηριστικών για να το `appSettings` στοιχείο. Εάν χρησιμοποιείτε ένα διαφορετικό όνομα αρχείου ή θέση, αντικαταστήστε τις τιμές για αυτές που εμφανίζονται στο παράδειγμα.
    -   Πριν:`<appSettings>`
    -   Μετά:` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`

    Το περιβάλλον εκτέλεσης ASP.NET συγχωνεύει τα περιεχόμενα του εξωτερικού αρχείου με τις σημάνσεις στο το `<appSettings>` στοιχείο. Το περιβάλλον εκτέλεσης παραβλέπει το χαρακτηριστικό αρχείου, αν το καθορισμένο αρχείο δεν είναι δυνατό να βρεθεί. Το απόρρητο (τη συμβολοσειρά σύνδεσης για να το cache) δεν περιλαμβάνονται ως τμήμα του κώδικα προέλευσης για την εφαρμογή. Κατά την ανάπτυξη της εφαρμογής σας web για να Azure, το `WebAppPlusCacheAppSecrests.config` αρχείων δεν θα να αναπτυχθούν (δηλαδή, τι θέλετε). Υπάρχουν πολλοί τρόποι για να καθορίσετε τα παρακάτω απόρρητο στο Azure και σε αυτό το πρόγραμμα εκμάθησης ρυθμίζονται αυτόματα για εσάς κατά την [προμήθεια του Azure πόρων](#provision-the-azure-resources) σε ένα πρόγραμμα εκμάθησης οι επόμενες βήμα. Για περισσότερες πληροφορίες σχετικά με την εργασία με απόρρητο στο Azure, ανατρέξτε στο θέμα [βέλτιστες πρακτικές για την ανάπτυξη κωδικών πρόσβασης και άλλα ευαίσθητα δεδομένα για να ASP.NET και Azure εφαρμογής υπηρεσίας](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).


### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>Ενημερώστε την κλάση TeamsController ώστε να επιστρέφει αποτελέσματα από το cache ή της βάσης δεδομένων

Σε αυτό το δείγμα, στατιστικά στοιχεία ομάδας μπορεί να ανακτηθεί από τη βάση δεδομένων ή από το cache. Στατιστικά στοιχεία ομάδας είναι αποθηκευμένα στο cache ως μια σειριακή `List<Team>`, και επίσης ως σύνολο ταξινομημένο χρησιμοποιούν τύπους δεδομένων Redis. Κατά την ανάκτηση στοιχείων από ένα ταξινομημένο σύνολο, μπορείτε να ανακτήσετε ορισμένες, όλα ή υποβολή ερωτήματος για ορισμένα στοιχεία. Σε αυτό το δείγμα θα ερώτημα συνόλου ταξινομημένο για τις κύριες 5 ομάδες στην κατάταξη κατά αριθμό wins.

>[AZURE.NOTE] Δεν είναι απαραίτητο να αποθηκεύσετε τα στατιστικά στοιχεία ομάδας σε πολλές μορφές στο cache για να χρησιμοποιήσετε Azure Redis Cache. Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί πολλά μορφές για μια επίδειξη ορισμένους από τους διαφορετικούς τρόπους και διαφορετικούς τύπους δεδομένων μπορείτε να χρησιμοποιήσετε για προσωρινή αποθήκευση των δεδομένων.



1. Προσθέστε τα ακόλουθα χρήση προτάσεων για το `TeamsController.cs` αρχείο στο επάνω μέρος με το άλλο χρήση προτάσεων.

        using System.Diagnostics;
        using Newtonsoft.Json;

2. Αντικαταστήστε την τρέχουσα `public ActionResult Index()` μέθοδο με την παρακάτω εφαρμογή.


        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;
        
            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;
        
                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;
        
                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }
        
            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();
        
            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;
        
                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;
        
                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;
        
                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }
        
            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();
        
            return View(teams);
        }


3. Προσθέστε τις ακόλουθες τρεις μεθόδους για την `TeamsController` κλάσης για την υλοποίηση το `playGames`, `clearCache`, και `rebuildDB` τύποι ενεργειών από τη δήλωση αλλαγής που προστέθηκαν στο προηγούμενο τμήμα κώδικα.

    Το `PlayGames` μέθοδο ενημερώνει τα στατιστικά στοιχεία ομάδας με προσομοίωση μια εποχή της αγώνων, αποθηκεύει τα αποτελέσματα με τη βάση δεδομένων και καταργεί το τώρα μη ενημερωμένων δεδομένων από το cache.


        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;
    
            Team.PlayGames(teams);
    
            db.SaveChanges();
    
            // Clear any cached results
            ClearCachedTeams();
        }


    Το `RebuildDB` μέθοδο reinitializes τη βάση δεδομένων με το προεπιλεγμένο σύνολο των ομάδων, δημιουργεί στατιστικά στοιχεία για τους και καταργεί το τώρα μη ενημερωμένων δεδομένων από το cache.
    
        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


    Το `ClearCachedTeams` μέθοδο καταργεί οποιαδήποτε στατιστικά στοιχεία στο cache ομάδας από το cache.

    
        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


4. Προσθέστε τις ακόλουθες τέσσερις μεθόδους για την `TeamsController` κλάση για να υλοποιήσετε το διάφοροι τρόποι ανακτώντας τα στατιστικά στοιχεία ομάδας από το cache και τη βάση δεδομένων. Κάθε μία από τις παρακάτω μεθόδους επιστρέφει ένα `List<Team>` που εμφανίζεται, στη συνέχεια, από την προβολή.

    Το `GetFromDB` μέθοδο διαβάζει τα στατιστικά στοιχεία ομάδας από τη βάση δεδομένων.

        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
    
            return results.ToList<Team>();
        }


    Το `GetFromList` μέθοδο διαβάζει τα στατιστικά στοιχεία ομάδας από το cache ως μια σειριακή `List<Team>`. Εάν υπάρχει μια αποτυχημένων cache, τα στατιστικά στοιχεία ομάδας είναι ανάγνωση από τη βάση δεδομένων και, στη συνέχεια, είναι αποθηκευμένα στο cache για την επόμενη φορά. Σε αυτό το δείγμα χρησιμοποιούμε τη σειριοποίησης JSON.NET σειριοποίηση τα αντικείμενα .NET προς και από το cache. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να εργαστείτε με το .NET αντικειμένων στο Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    Το `GetFromSortedSet` μέθοδο διαβάζει τα στατιστικά στοιχεία ομάδας από ένα σύνολο στο cache ταξινομημένο. Εάν υπάρχει μια αποτυχημένων cache, τα στατιστικά στοιχεία ομάδας είναι ανάγνωση από τη βάση δεδομένων και να είναι αποθηκευμένα στο cache ως ταξινομημένο σύνολο.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";
    
                // Read from DB
                teams = GetFromDB();
    
                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    Το `GetFromSortedSetTop5` μέθοδο διαβάζει τις ομάδες 5 επάνω από το σύνολο στο cache ταξινομημένο. Ξεκινά κατά τον έλεγχο της μνήμης cache για την ύπαρξη του `teamsSortedSet` κλειδί. Εάν αυτό το κλειδί δεν υπάρχει, η `GetFromSortedSet` ονομάζεται μέθοδο για να διαβάσετε τα στατιστικά στοιχεία ομάδας και να τα αποθηκεύσετε στο cache. Στη συνέχεια το σύνολο στο cache ταξινομημένο είναι ερώτημα με την επάνω γραμμή 5 τις ομάδες που επιστρέφονται.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>Ενημερώστε τις μεθόδους δημιουργία, επεξεργασία και διαγραφή για να εργαστείτε με το cache

Ο κώδικας ικριώματος που δημιουργήθηκε ως μέρος αυτού του δείγματος περιλαμβάνει μεθόδους για την προσθήκη, επεξεργασία και διαγραφή ομάδων. Οποιαδήποτε στιγμή μιας ομάδας προστίθεται, επεξεργασία, ή καταργηθεί, τα δεδομένα στο cache γίνονται μη ενημερωμένων. Σε αυτήν την ενότητα θα μπορείτε να τροποποιήσετε τις παρακάτω τρεις μεθόδους για να καταργήσετε τις ομάδες στο cache, έτσι ώστε το cache δεν θα δεν είναι συγχρονισμένο με τη βάση δεδομένων.

1. Αναζήτηση για να το `Create(Team team)` μέθοδο σε το `TeamsController` τάξης. Προσθήκη μιας κλήσης για να το `ClearCachedTeams` μέθοδο, όπως φαίνεται στο παρακάτω παράδειγμα.


        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
    
            return View(team);
        }


2. Αναζήτηση για να το `Edit(Team team)` μέθοδο σε το `TeamsController` τάξης. Προσθήκη μιας κλήσης για να το `ClearCachedTeams` μέθοδο, όπως φαίνεται στο παρακάτω παράδειγμα.


        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


3. Αναζήτηση για να το `DeleteConfirmed(int id)` μέθοδο σε το `TeamsController` τάξης. Προσθήκη μιας κλήσης για να το `ClearCachedTeams` μέθοδο, όπως φαίνεται στο παρακάτω παράδειγμα.


        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>Ενημέρωση της προβολής ευρετηρίου τις ομάδες για να εργαστείτε με το cache

1. Στην **Εξερεύνηση λύσεων**, αναπτύξτε το φάκελο **προβολές** , στη συνέχεια, στο φάκελο **τις ομάδες** , και κάντε διπλό κλικ στο **Index.cshtml**.

    ![Index.cshtml][cache-views-teams-index-cshtml]

2. Κοντά στο επάνω μέρος του αρχείου, αναζητήστε το ακόλουθο στοιχείο παραγράφου.

    ![Πίνακας ενεργειών][cache-teams-index-table]

    Αυτή είναι η σύνδεση για να δημιουργήσετε μια νέα ομάδα. Αντικαταστήστε το στοιχείο παραγράφου με τον παρακάτω πίνακα. Αυτός ο πίνακας έχει συνδέσεις ενεργειών για τη δημιουργία μιας νέας ομάδας, γίνεται αναπαραγωγή ενός νέου εποχή της αγώνων, εκκαθάριση του cache, ανακτώντας τις ομάδες από το cache σε διάφορες μορφές, ανακτώντας τις ομάδες από τη βάση δεδομένων και φτιάχνετε τη βάση δεδομένων με νέο δείγμα δεδομένων.


        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


3. Κάντε κύλιση στο κάτω μέρος του αρχείου **Index.cshtml** και προσθέστε τα ακόλουθα `tr` στοιχείο ώστε να είναι η τελευταία γραμμή του πίνακα τελευταία στο αρχείο.

        <tr><td colspan="5">@ViewBag.Msg</td></tr>

    Αυτή η γραμμή εμφανίζει την τιμή του `ViewBag.Msg` που περιέχει μια έκθεση προόδου για την τρέχουσα λειτουργία είναι ρυθμισμένο όταν κάνετε κλικ σε μία από τις συνδέσεις ενέργεια από το προηγούμενο βήμα.   

    ![Μήνυμα κατάστασης][cache-status-message]

4. Πατήστε το πλήκτρο **F6** για να δημιουργήσετε το έργο.

## <a name="provision-the-azure-resources"></a>Παροχή στους πόρους Azure

Για να φιλοξενήσετε την εφαρμογή στο Azure, πρέπει να πρώτα να παροχή των υπηρεσιών Azure που απαιτεί την εφαρμογή σας. Το δείγμα εφαρμογής σε αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί τις ακόλουθες υπηρεσίες Azure.

-   Azure Redis Cache
-   Εφαρμογή υπηρεσίας Web App
-   Βάση δεδομένων SQL

Για να αναπτύξετε αυτές τις υπηρεσίες σε μια ομάδα νέα ή υπάρχουσα πόρων της επιλογής σας, κάντε κλικ στην επιλογή το παρακάτω κουμπί **Ανάπτυξη να Azure** .

[! [Ανάπτυξη Azure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Αυτό το κουμπί **Ανάπτυξη να Azure** χρησιμοποιεί το πρότυπο [Γρήγορη έναρξη Azure](https://github.com/Azure/azure-quickstart-templates) [Δημιουργία μια εφαρμογή Web συν Redis Cache συν βάση δεδομένων SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) για την παροχή αυτών των υπηρεσιών και ορίστε τη συμβολοσειρά σύνδεσης για τη βάση δεδομένων SQL και τη ρύθμιση της εφαρμογής για τη συμβολοσειρά σύνδεσης Azure Redis Cache.

>[AZURE.NOTE] Εάν δεν έχετε λογαριασμό Azure, μπορείτε να [δημιουργήσετε έναν δωρεάν λογαριασμό Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) σε λίγα λεπτά.

Κάνοντας κλικ στο κουμπί **Ανάπτυξη να Azure** σας μεταφέρει στην πύλη του Azure και ξεκινά η διαδικασία δημιουργίας τους πόρους που περιγράφεται από το πρότυπο.

![Ανάπτυξη Azure][cache-deploy-to-azure-step-1]

1. Στην την **προσαρμοσμένη ανάπτυξης** blade, επιλέξτε τη συνδρομή Azure για να χρησιμοποιήσετε, και επιλέξτε μια υπάρχουσα ομάδα πόρων ή δημιουργήστε ένα νέο και καθορίστε τη θέση της ομάδας πόρων.
2. Στην το blade **παραμέτρων** , καθορίστε ένα όνομα λογαριασμού διαχειριστή (**ADMINISTRATORLOGIN** - δεν χρησιμοποιείτε το **διαχειριστή**), κωδικό πρόσβασης διαχειριστή (**ADMINISTRATORLOGINPASSWORD**) και το όνομα της βάσης δεδομένων (**όνομα βάσης ΔΕΔΟΜΈΝΩΝ**). Οι άλλες παράμετροι έχουν ρυθμιστεί για μια δωρεάν υπηρεσία της εφαρμογής φιλοξενίας σχέδιο και κάτω κόστος επιλογές για τη βάση δεδομένων SQL και Azure Redis Cache, που δεν παρέχονται με ένα δωρεάν επίπεδο.
3. Αλλάξετε οποιαδήποτε από τις άλλες ρυθμίσεις εάν θέλετε, ή διατηρήστε τις προεπιλεγμένες ρυθμίσεις και κάντε κλικ στο κουμπί **OK**.


![Ανάπτυξη Azure][cache-deploy-to-azure-step-2]

1. Κάντε κλικ στην επιλογή **Αναθεώρηση νομική όρους**.
2. Διαβάστε τους όρους σε το blade **αγοράς** και κάντε κλικ στην επιλογή **αγορά**.
3. Για να ξεκινήσετε την προμήθεια του τους πόρους, κάντε κλικ στην επιλογή **Δημιουργία** στην blade την **προσαρμοσμένη ανάπτυξης** .

Για να δείτε την πρόοδο της ανάπτυξής σας, κάντε κλικ στο εικονίδιο ειδοποίησης και κάντε κλικ στο κουμπί **ανάπτυξης αποτελέσματα**.

![Ανάπτυξη αποτελέσματα][cache-deployment-started]

Μπορείτε να προβάλετε την κατάσταση της ανάπτυξής σας σε το blade **Microsoft.Template** .

![Ανάπτυξη Azure][cache-deploy-to-azure-step-3]

Όταν ολοκληρωθεί η προμήθεια, μπορείτε να δημοσιεύσετε την εφαρμογή με το Azure από το Visual Studio.

>[AZURE.NOTE] Εμφανίζονται σφάλματα που μπορεί να προκύψουν κατά τη διάρκεια της διαδικασίας προετοιμασίας στην το blade **Microsoft.Template** . Συνήθη σφάλματα είναι πάρα πολλά διακομιστές SQL ή πάρα πολλά δωρεάν εφαρμογή υπηρεσία φιλοξενίας προγράμματος ανά συνδρομή. Επίλυση τυχόν σφάλματα και ξεκινήστε τη διαδικασία κάνοντας κλικ στην επιλογή **αναπτύξτε ξανά** στην το blade **Microsoft.Template** ή το κουμπί **Ανάπτυξη να Azure** σε αυτό το πρόγραμμα εκμάθησης.

## <a name="publish-the-application-to-azure"></a>Δημοσίευση της εφαρμογής σε Azure

Σε αυτό το βήμα του προγράμματος εκμάθησης, που θα δημοσιεύσετε την εφαρμογή για να Azure και εκτελέστε το στο cloud.

1. Κάντε δεξί κλικ στο έργο **ContosoTeamStats** στο Visual Studio και επιλέξτε **Δημοσίευση**.

    ![Δημοσίευση][cache-publish-app]

2. Κάντε κλικ στην επιλογή **Microsoft Azure εφαρμογής υπηρεσίας**.

    ![Δημοσίευση][cache-publish-to-app-service]

3. Επιλέξτε τη συνδρομή που χρησιμοποιείται κατά τη δημιουργία του Azure πόρους, αναπτύξτε την ομάδα των πόρων που περιέχει τους πόρους, επιλέξτε την επιθυμητή Web App και κάντε κλικ στο κουμπί **OK**. Εάν χρησιμοποιήσατε το κουμπί **Ανάπτυξη να Azure** το όνομα του Web App ξεκινά με **τοποθεσία Web** ακολουθείται από ορισμένες επιπλέον χαρακτήρες.

    ![Επιλέξτε Web App][cache-select-web-app]

4. Κάντε κλικ στην επιλογή **Επικύρωση σύνδεσης** για να επαληθεύσετε τις ρυθμίσεις σας και, στη συνέχεια, κάντε κλικ στο κουμπί **Δημοσίευση**.

    ![Δημοσίευση][cache-publish]

    Μετά από μερικά λεπτά θα ολοκληρώσει τη διαδικασία δημοσίευσης και θα ξεκινήσει ένα πρόγραμμα περιήγησης με το τρέχον δείγμα εφαρμογής. Εάν λάβετε ένα σφάλμα DNS κατά την επικύρωση ή δημοσίευσης και μόνο πρόσφατα ολοκλήρωση της διαδικασίας προετοιμασίας για τους πόρους Azure για την εφαρμογή, περιμένετε λίγο και προσπαθήστε ξανά.

    ![Προσθέσατε cache][cache-added-to-application]

Ο παρακάτω πίνακας περιγράφει κάθε σύνδεση ενέργεια από το δείγμα εφαρμογής.

| Ενέργεια                  | Περιγραφή                                                                                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Δημιουργία νέας              | Δημιουργήστε μια νέα ομάδα.                                                                                                                                               |
| Αναπαραγωγή Season             | Αναπαραγωγή μιας εποχή της αγώνων, ενημερώστε το όρισμα στατιστικές ομάδας και καταργήστε οποιαδήποτε παλιό ομάδας δεδομένων από το cache.                                                                          |
| Απαλοιφή Cache             | Καταργήστε την επιλογή από το όρισμα στατιστικές ομάδας από το cache.                                                                                                                             |
| Λίστα από το Cache         | Ανακτήστε τα στατιστικά στοιχεία ομάδας από το cache. Εάν υπάρχει μια αποτυχημένων cache, φόρτωση τα στατιστικά στοιχεία από τη βάση δεδομένων και την αποθήκευση στο cache για την επόμενη φορά.                                        |
| Ταξινομημένο σύνολο από το Cache   | Ανακτήστε τα στατιστικά στοιχεία ομάδας από το cache χρησιμοποιώντας ένα ταξινομημένο σύνολο. Εάν υπάρχει μια αποτυχημένων cache, φορτώστε το όρισμα στατιστικές από τη βάση δεδομένων και αποθήκευση στο cache χρησιμοποιώντας ένα ταξινομημένο σύνολο.  |
| Ομάδες 5 επάνω από το Cache  | Ανακτήστε την επάνω στις ομάδες 5 από το cache χρησιμοποιώντας ένα ταξινομημένο σύνολο. Εάν υπάρχει μια αποτυχημένων cache, φορτώστε το όρισμα στατιστικές από τη βάση δεδομένων και αποθήκευση στο cache χρησιμοποιώντας ένα ταξινομημένο σύνολο. |
| Φόρτωση από DB            | Ανακτήστε τα στατιστικά στοιχεία ομάδας από τη βάση δεδομένων.                                                                                                                       |
| Εκ νέου δημιουργία DB              | Εκ νέου δημιουργία της βάσης δεδομένων και να φορτώσετε ξανά με δείγμα δεδομένων ομάδας.                                                                                                        |
| Επεξεργασία / λεπτομέρειες / διαγραφή | Επεξεργασία ομάδας, προβολή λεπτομερειών για μια ομάδα, να διαγράψετε μια ομάδα.                                                                                                             |


Κάντε κλικ σε ορισμένες από τις ενέργειες και να πειραματιστείτε με την ανάκτηση των δεδομένων από διάφορες πηγές. Δεν τις διαφορές στις το χρόνο που χρειάζεται για να ολοκληρώσετε το διάφοροι τρόποι σχετικά με την ανάκτηση των δεδομένων από τη βάση δεδομένων και το cache.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>Διαγραφή των πόρων, όταν τελειώσετε με την εφαρμογή

Όταν τελειώσετε με την εφαρμογή του προγράμματος εκμάθησης δείγμα, μπορείτε να διαγράψετε το Azure τους πόρους που χρησιμοποιούνται για να εξοικονομήσετε πόροι κόστους και. Εάν μπορείτε να χρησιμοποιήσετε το κουμπί **Ανάπτυξη να Azure** στην ενότητα [προμήθεια Azure πόρων](#provision-the-azure-resources) και όλους τους πόρους σας περιέχονται στην ίδια ομάδα πόρων, μπορείτε να διαγράψετε τους μαζί με μια εργασία, διαγράφοντας την ομάδα των πόρων.

1. Πραγματοποιήστε είσοδο στην [πύλη του Azure](https://portal.azure.com) και κάντε κλικ στην επιλογή **ομάδες πόρων**.
2. Πληκτρολογήστε το όνομα της ομάδας σας πόρου στο πλαίσιο κειμένου **Φιλτράρισμα στοιχείων...** .
3. Κάντε κλικ στην επιλογή **...** δεξιά από την ομάδα πόρων.
4. Κάντε κλικ στην επιλογή **Διαγραφή**.

    ![Διαγραφή][cache-delete-resource-group]

5. Πληκτρολογήστε το όνομα της ομάδας σας πόρων και κάντε κλικ στην επιλογή **Διαγραφή**.

    ![Επιβεβαίωση διαγραφής][cache-delete-confirm]

Η ομάδα πόρων και όλα τα περιεχόμενα πόρων διαγράφονται μετά από μερικά λεπτά.

>[AZURE.IMPORTANT] Σημειώστε ότι η διαγραφή μιας ομάδας πόρων είναι αναστρέψιμη και ότι η ομάδα πόρων και όλους τους πόρους του διαγράφονται οριστικά. Βεβαιωθείτε ότι δεν διαγράψετε κατά λάθος ομάδας λάθος πόρων ή πόρους. Εάν έχετε δημιουργήσει τους πόρους για τη φιλοξενία αυτού του δείγματος μέσα σε μια υπάρχουσα ομάδα πόρων, μπορείτε να διαγράψετε μεμονωμένα κάθε πόρο από τους αντίστοιχους λεπίδες.

## <a name="run-the-sample-application-on-your-local-machine"></a>Εκτελέστε το δείγμα εφαρμογής στον τοπικό υπολογιστή σας

Για να εκτελέσετε την εφαρμογή τοπικά στον υπολογιστή σας, χρειάζεστε μια παρουσία Azure Redis Cache στην οποία θα προσωρινή αποθήκευση των δεδομένων σας. 

-   Εάν έχετε δημοσιεύσει την εφαρμογή με το Azure όπως περιγράφεται στην προηγούμενη ενότητα, μπορείτε να χρησιμοποιήσετε την παρουσία Azure Redis Cache που παρέχεται σε αυτό το βήμα.
-   Εάν έχετε μια άλλη υπάρχουσα παρουσία Azure Redis Cache, μπορείτε να χρησιμοποιήσετε που για την εκτέλεση αυτού του δείγματος τοπικά.
-   Εάν χρειάζεστε για να δημιουργήσετε μια παρουσία Azure Redis Cache, μπορείτε να ακολουθήσετε τα βήματα στο θέμα [Δημιουργία μιας cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Αφού έχετε επιλέξει ή δημιουργήσει το cache για να χρησιμοποιήσετε, μεταβείτε στο cache στην πύλη του Azure και να ανακτήσετε τα [πλήκτρα πρόσβασης](cache-configure.md#access-keys) για το cache και [όνομα κεντρικού υπολογιστή](cache-configure.md#properties) . Για οδηγίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων Redis ρυθμίσεις του cache](cache-configure.md#configure-redis-cache-settings).

1. Άνοιγμα του `WebAppPlusCacheAppSecrets.config` αρχείο που δημιουργήσατε κατά τη [Ρύθμιση παραμέτρων της εφαρμογής για να χρησιμοποιήσετε Redis Cache](#configure-the-application-to-use-redis-cache) βήμα αυτού του προγράμματος εκμάθησης χρησιμοποιώντας το πρόγραμμα επεξεργασίας της επιλογής σας.

2. Επεξεργασία του `value` χαρακτηριστικών και να αντικαταστήσετε `MyCache.redis.cache.windows.net` με το [όνομα κεντρικού υπολογιστή](cache-configure.md#properties) από το cache, και καθορίστε είτε το [πρωτεύον ή δευτερεύον κλειδί](cache-configure.md#access-keys) από το cache με τον κωδικό πρόσβασης.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


3. Πατήστε το **Συνδυασμό πλήκτρων Ctrl + F5** για να εκτελέσετε την εφαρμογή.

>[AZURE.NOTE] Σημειώστε ότι επειδή η εφαρμογή, συμπεριλαμβανομένης της βάσης δεδομένων, εκτελεί τοπικά και το Cache Redis φιλοξενείται στο Azure, το cache ενδέχεται να εμφανίζονται στην περιοχή εκτέλεσης της βάσης δεδομένων. Για καλύτερες επιδόσεις, την εφαρμογή υπολογιστή-πελάτη και την παρουσία Azure Redis Cache πρέπει να είναι στην ίδια θέση. 

## <a name="next-steps"></a>Επόμενα βήματα

-   Μάθετε περισσότερα για [Γρήγορα αποτελέσματα με το ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) στην τοποθεσία του [ASP.NET](http://asp.net/) .
-   Για περισσότερα παραδείγματα τη δημιουργία μιας εφαρμογής Web ASP.NET στην εφαρμογή υπηρεσίας, ανατρέξτε στο θέμα [Δημιουργία και ανάπτυξη της εφαρμογής web ASP.NET στο Azure εφαρμογής υπηρεσίας](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) από τη σύνδεση [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [επίδειξη](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
    -   Για περισσότερες γρήγορες εκκινήσεις από την επίδειξη HealthClinic.biz, ανατρέξτε στο θέμα [Γρήγορες εκκινήσεις εργαλεία για προγραμματιστές Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
-   Μάθετε περισσότερα σχετικά με την προσέγγιση [κώδικα πρώτα σε μια νέα βάση δεδομένων](https://msdn.microsoft.com/data/jj193542) να Framework οντότητα που χρησιμοποιείται σε αυτό το πρόγραμμα εκμάθησης.
-   Μάθετε περισσότερα σχετικά με τις [εφαρμογές web στο Azure εφαρμογής υπηρεσίας](../app-service-web/app-service-web-overview.md).
-   Μάθετε πώς μπορείτε να [οθόνη](cache-how-to-monitor.md) το cache στην πύλη του Azure.

-   Εξερευνήστε Azure Redis Cache κορυφαίες δυνατότητες
    -   [Τρόπος ρύθμισης των παραμέτρων διατήρησης για Premium Azure Redis μνήμης Cache](cache-how-to-premium-persistence.md)
    -   [Τρόπος ρύθμισης των παραμέτρων του συμπλέγματος για Premium Azure Redis μνήμης Cache](cache-how-to-premium-clustering.md)
    -   [Πώς μπορείτε να ρυθμίσετε τις παραμέτρους εικονικού δικτύου υποστήριξης για Premium Azure Redis μνήμης Cache](cache-how-to-premium-vnet.md)
    -   Ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις Cache Redis Azure](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) για περισσότερες λεπτομέρειες σχετικά με το μέγεθος, μετάδοσης και εύρους ζώνης με premium μνήμης cache.



<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

