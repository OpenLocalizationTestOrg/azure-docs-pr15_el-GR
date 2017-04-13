<properties 
    pageTitle="Πώς να χρησιμοποιείτε το Cache Azure Redis | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να βελτιώσετε την απόδοση των εφαρμογών σας Azure με Azure Redis Cache" 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache"></a>Πώς να χρησιμοποιείτε το Cache Azure Redis

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Αυτός ο οδηγός δείχνει πώς μπορείτε να ξεκινήσετε τη χρήση του **Azure Redis Cache**. Microsoft Azure Redis Cache βασίζεται σε δημοφιλείς Άνοιγμα προέλευσης Redis Cache. Το παρέχει πρόσβαση σε μια ασφαλή, αποκλειστικό Redis cache, γίνεται από τη Microsoft. Ένα cache που δημιουργούνται με χρήση Azure Redis Cache είναι προσβάσιμα από οποιαδήποτε εφαρμογή μέσα σε Windows Azure.

Microsoft Azure Redis Cache είναι διαθέσιμη σε τα παρακάτω επίπεδα:

-   **Βασική** – μεμονωμένο κόμβο. Πολλά μεγέθη έως 53 GB.
-   **Τυπική** – κόμβου δύο κύρια/αντιγράφου. Πολλά μεγέθη έως 53 GB. 99,9 SLA %.
-   **Premium** – κόμβου δύο κύρια/ρεπλίκα με έως και 10 shards. Πολλά μεγέθη από 6 GB για 530 GB (επικοινωνήστε μαζί μας για περισσότερες πληροφορίες). Όλες οι δυνατότητες τυπική σειρά και περισσότερες συμπεριλαμβανομένου υποστήριξη για [Redis σύμπλεγμα](cache-how-to-premium-clustering.md) [Redis διατήρησης](cache-how-to-premium-persistence.md)και [Azure εικονικού δικτύου](cache-how-to-premium-vnet.md). 99,9 SLA %.

Κάθε επίπεδο διαφέρει όσον αφορά τις δυνατότητες και τις πληροφορίες τιμολόγησης. Για πληροφορίες σχετικά με τις τιμές, ανατρέξτε στο θέμα [Cache τις τιμές λεπτομερειών][].

Αυτός ο οδηγός σας δείχνει πώς μπορείτε να χρησιμοποιήσετε το πρόγραμμα-πελάτη [StackExchange.Redis][] χρησιμοποιώντας C\# κώδικα. Τα σενάρια καλύπτεται περιλαμβάνουν **τη δημιουργία και τη ρύθμιση παραμέτρων cache**, **τη ρύθμιση παραμέτρων cache προγράμματα-πελάτες**και **Προσθήκη και κατάργηση αντικειμένων από το cache**. Για περισσότερες πληροφορίες σχετικά με τη χρήση μνήμης Cache Redis Azure, ανατρέξτε στην ενότητα [Επόμενα βήματα][] . Για ένα βήμα προς βήμα εκμάθηση της δόμησης ενός MVC ASP.NET web app με Redis Cache, δείτε [πώς μπορείτε να δημιουργήσετε μια εφαρμογή Web με Redis Cache](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## <a name="get-started-with-azure-redis-cache"></a>Γρήγορα αποτελέσματα με το Azure Redis Cache

Γρήγορα αποτελέσματα με το Azure Redis Cache είναι εύκολη. Για να ξεκινήσετε, μπορείτε να προμηθεύσουν και ρύθμιση παραμέτρων cache. Στη συνέχεια, μπορείτε να ρυθμίσετε τα προγράμματα-πελάτες του cache, ώστε να μπορούν να έχουν πρόσβαση το cache. Όταν έχουν ρυθμιστεί οι παράμετροι των υπολογιστών-πελατών του cache, μπορείτε να ξεκινήσετε την εργασία με αυτά.

-   [Δημιουργία του cache][]
-   [Ρύθμιση παραμέτρων των υπολογιστών-πελατών του cache][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Δημιουργήστε ένα cache

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Για να αποκτήσετε πρόσβαση το cache μετά από αυτό δημιουργείται

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων του cache, δείτε [πώς μπορείτε να ρυθμίσετε τις παραμέτρους Azure Redis Cache](cache-configure.md).

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>Ρύθμιση παραμέτρων των υπολογιστών-πελατών του cache

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Μόλις το έργο προγράμματος-πελάτη σας έχει ρυθμιστεί για προσωρινή αποθήκευση, μπορείτε να χρησιμοποιήσετε τις τεχνικές που περιγράφονται στις ενότητες που ακολουθούν για την εργασία με το cache.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Εργασία με μνήμης cache

Τα βήματα σε αυτήν την ενότητα περιγράφουν πώς να εκτελείτε συνήθεις εργασίες με το Cache.

-   [Σύνδεση με το cache][]
-   [Προσθήκη και ανάκτησης αντικειμένων από το cache][]
-   [Εργασία με .NET αντικειμένων στο cache](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## <a name="connect-to-the-cache"></a>Σύνδεση με το cache

Για να εργαστείτε μέσω προγραμματισμού με ένα cache, χρειάζεστε μια αναφορά στο cache. Προσθέστε τα ακόλουθα στο επάνω μέρος οποιουδήποτε αρχείου από το οποίο θέλετε να χρησιμοποιήσετε το πρόγραμμα-πελάτη StackExchange.Redis για να αποκτήσετε πρόσβαση σε μια Azure Redis Cache.

    using StackExchange.Redis;

>[AZURE.NOTE] Το πρόγραμμα-πελάτη StackExchange.Redis απαιτεί .NET Framework 4 ή νεότερη έκδοση.

Η σύνδεση με το Cache Redis Azure γίνεται από το `ConnectionMultiplexer` τάξης. Αυτή η κλάση έχει σχεδιαστεί ώστε να είναι κοινόχρηστη και να τα χρησιμοποιήσετε πάλι σε ολόκληρη την εφαρμογή-πελάτη και δεν χρειάζεται να δημιουργηθούν σε βάση ανά λειτουργία. 

Για να συνδεθείτε με μια Cache Redis Azure και να επιστραφεί μια παρουσία ενός συνδεδεμένου `ConnectionMultiplexer`, καλέστε την στατική `Connect` μέθοδο και Πέρασμα στο cache τελικού σημείου και αριθμού-κλειδιού, όπως το παρακάτω παράδειγμα. Χρησιμοποιήστε το πλήκτρο που δημιουργούνται από την πύλη Azure ως η παράμετρος τον κωδικό πρόσβασης.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Προειδοποίηση: Ποτέ χώρο αποθήκευσης διαπιστευτηρίων στο πηγαίου κώδικα. Για να διατηρήσετε αυτό το δείγμα απλή, να που εμφανίζει τους στον πηγαίο κώδικα. Ανατρέξτε στο θέμα [Πώς συμβολοσειρές εφαρμογής και σύνδεση συμβολοσειρές εργασίας][] για πληροφορίες σχετικά με την αποθήκευση των διαπιστευτηρίων.

Εάν δεν θέλετε να χρησιμοποιήσετε το SSL, είτε ορίστε `ssl=false` ή να παραλείψετε το `ssl` παραμέτρου.

>[AZURE.NOTE] Η θύρα μη SSL είναι απενεργοποιημένη από προεπιλογή για νέες μνήμης cache. Για οδηγίες σχετικά με την ενεργοποίηση της θύρας χωρίς SSL, ανατρέξτε στο θέμα τις [Θύρες Access](cache-configure.md#access-ports)...

Μία προσέγγιση για την κοινή χρήση ενός `ConnectionMultiplexer` παρουσία στην εφαρμογή σας είναι να έχετε μια στατική ιδιότητα που επιστρέφει ένα συνδεδεμένο παρουσία, παρόμοια με το παρακάτω παράδειγμα. Αυτό παρέχει νήματος ασφαλή τρόπο για να προετοιμάσετε ένα μόνο συνδεδεμένοι `ConnectionMultiplexer` παρουσία. Σε αυτά τα παραδείγματα `abortConnect` έχει οριστεί σε false, γεγονός που σημαίνει ότι η κλήση θα επιτύχει ακόμα και αν δεν δημιουργείται μια σύνδεση στο cache Redis Azure. Ένα βασικό χαρακτηριστικό της `ConnectionMultiplexer` είναι ότι θα επαναφέρει συνδεσιμότητας αυτόματα στο cache μία φορά το πρόβλημα δικτύου ή άλλες αιτίες επιλύονται.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Για περισσότερες πληροφορίες σχετικά με τις επιλογές ρύθμισης παραμέτρων σύνδεσης για προχωρημένους, ανατρέξτε στο θέμα [μοντέλο ρύθμισης παραμέτρων StackExchange.Redis][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Όταν δημιουργηθεί η σύνδεση, να επιστρέφει αναφορά στη βάση δεδομένων του cache redis καλώντας την `ConnectionMultiplexer.GetDatabase` μέθοδο. Το αντικείμενο που επιστρέφονται από το `GetDatabase` μέθοδος είναι ένα αντικείμενο ελαφριά διαβίβασης και δεν χρειάζεται να αποθηκευτεί.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Τώρα που γνωρίζετε πώς να συνδεθείτε σε μια περίοδο λειτουργίας Azure Redis Cache και επιστρέφει μια αναφορά στη βάση δεδομένων του cache, ας ρίξουμε μια ματιά στην εργασία με το cache.

<a name="add-object"></a>
## <a name="add-and-retrieve-objects-from-the-cache"></a>Προσθήκη και ανάκτησης αντικειμένων από το cache

Στοιχεία μπορεί να είναι αποθηκευμένα σε και να ανακτηθούν από ένα cache χρησιμοποιώντας το `StringSet` και `StringGet` μεθόδους.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis αποθηκεύει περισσότερα δεδομένα ως συμβολοσειρές Redis, αλλά αυτές τις συμβολοσειρές μπορεί να περιέχει πολλούς τύπους δεδομένων, συμπεριλαμβανομένων των αλληλουχίας δυαδικά δεδομένα, τα οποία μπορούν να χρησιμοποιηθούν κατά την αποθήκευση .NET αντικειμένων στο cache.

Κατά την κλήση `StringGet`, εάν υπάρχει στο αντικείμενο, επιστρέφεται, και αν δεν έχει, `null` επιστρέφεται. Σε αυτήν την περίπτωση μπορείτε να ανακτήσετε την τιμή από την προέλευση δεδομένων που θέλετε και να αποθηκεύσετε στο cache για μετέπειτα χρήση. Αυτό είναι γνωστό ως μοτίβο παύσης cache.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Για να καθορίσετε τη λήξη ενός στοιχείου στο cache, χρησιμοποιήστε το `TimeSpan` παράμετρος της `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Εργασία με .NET αντικειμένων στο cache

Azure Redis Cache να cache αντικειμένων .NET, καθώς και προκαταρκτικούς τύπους δεδομένων, αλλά πριν από ένα αντικείμενο .NET μπορούν να αποθηκευτούν προσωρινά πρέπει να σειριοποιηθεί. Αυτό είναι ευθύνη από τον προγραμματιστή της εφαρμογής και σας δίνει την ευελιξία για προγραμματιστές κατά την επιλογή των ο σειριοποιητής.

Ένα απλό τρόπο να σειριοποιεί αντικείμενα που είναι να χρησιμοποιήσετε το `JsonConvert` μεθόδων σειριοποίησης στο [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) και σειριοποίηση προς και από JSON. Το παρακάτω παράδειγμα εμφανίζει μια λήψη και χρήση του συνόλου μια `Employee` παρουσία του αντικειμένου.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα σχετικά με το Azure Redis Cache.

-   Δείτε τις υπηρεσίες παροχής ASP.NET για Azure Redis Cache.
    -   [Υπηρεσία παροχής κατάσταση περιόδου λειτουργίας Redis Azure](cache-aspnet-session-state-provider.md)
    -   [Υπηρεσία παροχής Cache εξόδου ASP.NET Cache Redis Azure](cache-aspnet-output-cache-provider.md)
-   [Ενεργοποίηση Διαγνωστικά του cache](cache-how-to-monitor.md#enable-cache-diagnostics) , ώστε να μπορείτε να κάνετε [Παρακολούθηση](cache-how-to-monitor.md) της εύρυθμης λειτουργίας του το cache. Μπορείτε να προβάλετε τα μετρικά στην πύλη του Azure και μπορείτε επίσης να [κάνετε λήψη και να αναθεωρήσετε](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) τις χρησιμοποιώντας τα εργαλεία της επιλογής σας.
-   Δείτε την [τεκμηρίωση του προγράμματος-πελάτη cache StackExchange.Redis][].
    -   Azure Redis Cache είναι δυνατή η πρόσβαση από πολλά προγράμματα-πελάτες του Redis και ανάπτυξη γλώσσες. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [http://redis.io/clients][].
-   Azure Redis Cache μπορεί επίσης να χρησιμοποιηθεί με εργαλεία όπως Redsmin και Redis διαχείρισης επιφάνειας εργασίας και υπηρεσιών τρίτων.
    -   Για περισσότερες πληροφορίες σχετικά με την Redsmin, δείτε [πώς μπορείτε να ανακτήσετε μια συμβολοσειρά σύνδεσης Azure Redis και να το χρησιμοποιήσετε με Redsmin][].
    -   Πρόσβαση και έλεγχος των δεδομένων σας στο Cache Redis Azure με μια Γραφικών με χρήση [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).
-   Ανατρέξτε στην τεκμηρίωση [redis][] και Διαβάστε σχετικά με τις [redis τους τύπους δεδομένων][] και [μια εισαγωγή δεκαπέντε λεπτά Redis τύπους δεδομένων][].



<!-- INTRA-TOPIC LINKS -->
[Επόμενα βήματα]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Δημιουργία του cache]: #create-cache
[Configure the cache]: #enable-caching
[Ρύθμιση παραμέτρων των υπολογιστών-πελατών του cache]: #NuGet
[Working with Caches]: #working-with-caches
[Σύνδεση με το cache]: #connect-to-cache
[Προσθήκη και ανάκτησης αντικειμένων από το cache]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.IO/Clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Πώς μπορείτε να ανακτήσετε μια συμβολοσειρά σύνδεσης Azure Redis και να το χρησιμοποιήσετε με Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[Μοντέλο ρύθμισης παραμέτρων StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Πληροφορίες για την τιμολόγηση cache]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[Τεκμηρίωση προγράμματος-πελάτη StackExchange.Redis cache]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis τύποι δεδομένων]: http://redis.io/topics/data-types
[μια εισαγωγή δεκαπέντε λεπτά Redis τύπους δεδομένων]: http://redis.io/topics/data-types-intro

[Πώς λειτουργεί η εφαρμογή συμβολοσειρές και συμβολοσειρές σύνδεσης]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


