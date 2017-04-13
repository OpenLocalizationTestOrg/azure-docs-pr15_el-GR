<properties
    pageTitle="Υπηρεσία παροχής Cache εξόδου ASP.NET cache"
    description="Μάθετε πώς μπορείτε να cache εξόδου σελίδα ASP.NET χρησιμοποιώντας Azure Redis Cache"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>Υπηρεσία παροχής Cache εξόδου ASP.NET για Azure Redis Cache

Η υπηρεσία παροχής εξόδου Cache Redis είναι ένας μηχανισμός αποθήκευσης εκτός της διαδικασίας για τα δεδομένα εξόδου cache. Αυτά τα δεδομένα είναι ειδικά για την πλήρη αποκρίσεις HTTP (σελίδα cache εμφάνισης σελίδας). Η υπηρεσία παροχής που συνδέεται στη νέα εξόδου cache παροχής επεκτασιμότητα του σημείου που παρουσιάστηκε στο ASP.NET 4.

Για να χρησιμοποιήσετε την υπηρεσία παροχής εξόδου Cache Redis, πρώτα να ρυθμίσετε το cache και, στη συνέχεια, ρύθμιση παραμέτρων εφαρμογής ASP.NET χρησιμοποιώντας το πακέτο Redis NuGet παροχής Cache εξόδου. Αυτό το θέμα παρέχει οδηγίες σχετικά με τη ρύθμιση της εφαρμογής σας για να χρησιμοποιήσετε την υπηρεσία παροχής εξόδου Cache Redis. Για περισσότερες πληροφορίες σχετικά με τη δημιουργία και τη ρύθμιση παραμέτρων Azure Redis Cache παρουσίας, ανατρέξτε στο θέμα [Δημιουργία μιας cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>Αποθήκευση εξόδου σελίδα ASP.NET στο cache

Για να ρυθμίσετε μια εφαρμογή προγράμματος-πελάτη στο Visual Studio, χρησιμοποιώντας το πακέτο Redis NuGet παροχής Cache εξόδου, κάντε δεξί κλικ στο έργο στην **Εξερεύνηση λύσεων** και επιλέξτε **Διαχείριση πακέτων NuGet**.

![Azure Redis Cache διαχείριση πακέτων NuGet](./media/cache-aspnet-output-cache-provider/redis-cache-manage-nuget-menu.png)

Πληκτρολογήστε **RedisOutputCacheProvider** στο πλαίσιο κειμένου "Αναζήτηση", επιλέξτε την από τα αποτελέσματα και κάντε κλικ στην επιλογή **εγκατάσταση**.

![Υπηρεσία παροχής Cache εξόδου Cache Redis Azure](./media/cache-aspnet-output-cache-provider/redis-cache-page-output-provider.png)

Το πακέτο Redis NuGet παροχής Cache εξόδου διαθέτει μια εξάρτηση από το πακέτο StackExchange.Redis.StrongName. Εάν το πακέτο StackExchange.Redis.StrongName δεν υπάρχει στο έργο σας θα εγκατασταθούν. Σημειώστε ότι εκτός από το πακέτο StackExchange.Redis.StrongName ισχυρό ονομάζεται εκεί είναι επίσης την έκδοση μη ισχυρό-με όνομα StackExchange.Redis. Εάν το έργο σας χρησιμοποιεί την έκδοση StackExchange.Redis μη ισχυρό-με όνομα που πρέπει να καταργήσετε την εγκατάστασή του, πριν ή μετά την εγκατάσταση του πακέτου Redis NuGet παροχής Cache εξόδου, διαφορετικά που θα λάβετε ονομασία διενέξεων στο έργο σας. Για περισσότερες πληροφορίες σχετικά με αυτά τα πακέτα, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων .NET προγράμματα-πελάτες cache](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Το πακέτο NuGet στοιχεία λήψης και προσθέτει τις απαιτούμενες συναρμολόγησης αναφορές και την παρακάτω ενότητα στο αρχείο σας web.config που περιέχει τις απαιτούμενες ρυθμίσεις για την εφαρμογή σας ASP.NET για να χρησιμοποιήσετε την υπηρεσία παροχής εξόδου Cache Redis.

    <caching>
      <outputCachedefault Provider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

Στην ενότητα σχόλια παρέχει ένα παράδειγμα του χαρακτηριστικά και τις ρυθμίσεις δείγμα για κάθε χαρακτηριστικό.

Ρύθμιση παραμέτρων τα χαρακτηριστικά με τις τιμές από το cache blade στην πύλη του Microsoft Azure και ρυθμίσετε τις άλλες τιμές που θέλετε. Για οδηγίες σχετικά με την πρόσβαση σε ιδιότητες του cache σας, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων Redis ρυθμίσεις του cache](cache-configure.md#configure-redis-cache-settings).

-   **κεντρικός υπολογιστής** -Καθορίστε το τελικό σημείο cache.
-   **θύρα** – χρήση θύρα σας χωρίς SSL ή το SSL θύρα, ανάλογα με τις ρυθμίσεις ssl.
-   **accessKey** -Χρησιμοποιήστε το πρωτεύον ή δευτερεύον κλειδί για το cache.
-   **ssl** – true εάν θέλετε να ασφαλούς επικοινωνιών cache/προγράμματος-πελάτη με ssl; Διαφορετικά false. Φροντίστε να καθορίσετε τη σωστή θύρα.
    -   Η θύρα μη SSL είναι απενεργοποιημένη από προεπιλογή για νέες μνήμης cache. Καθορίστε την τιμή true για αυτήν τη ρύθμιση για να χρησιμοποιήσετε τη θύρα SSL. Για περισσότερες πληροφορίες σχετικά με την ενεργοποίηση της θύρας χωρίς SSL, ανατρέξτε στην ενότητα [Πρόσβαση θύρες](cache-configure.md#access-ports) στο θέμα [Ρύθμιση παραμέτρων cache](cache-configure.md) .
-   **databaseId** – καθοριστεί της βάσης δεδομένων για να χρησιμοποιήσετε για τα δεδομένα εξόδου cache. Εάν δεν καθορίζεται, χρησιμοποιείται η προεπιλεγμένη τιμή 0.
-   **applicationName** – πλήκτρα είναι αποθηκευμένα σε redis ως <AppName>_<SessionId>_Data. Αυτή η δυνατότητα επιτρέπει πολλές εφαρμογές να μοιράζονται το ίδιο κλειδί. Αυτή η παράμετρος είναι προαιρετική και εάν δεν παρέχεται αυτό χρησιμοποιείται μια προεπιλεγμένη τιμή.
-   **connectionTimeoutInMilliseconds** – αυτή η ρύθμιση σάς επιτρέπει να αντικαταστήσετε τη ρύθμιση connectTimeout στο πρόγραμμα-πελάτη StackExchange.Redis. Εάν δεν καθορίζεται, χρησιμοποιείται η προεπιλεγμένη ρύθμιση connectTimeout 5000. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [μοντέλο ρύθμισης παραμέτρων StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** – αυτή η ρύθμιση σάς επιτρέπει να αντικαταστήσετε τη ρύθμιση syncTimeout στο πρόγραμμα-πελάτη StackExchange.Redis. Εάν δεν καθορίζεται, χρησιμοποιείται η προεπιλεγμένη ρύθμιση syncTimeout 1000. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [μοντέλο ρύθμισης παραμέτρων StackExchange.Redis](http://go.microsoft.com/fwlink/?LinkId=398705).

Προσθέστε μια οδηγία OutputCache σε κάθε σελίδα για την οποία θέλετε να αποθηκεύσουν προσωρινά την έξοδο.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

Σε αυτό το παράδειγμα τα δεδομένα στο cache σελίδα παραμένει στο cache για 60 δευτερόλεπτα, και μια διαφορετική έκδοση της σελίδας είναι cache για κάθε συνδυασμός παραμέτρων. Για περισσότερες πληροφορίες σχετικά με την οδηγία OutputCache, ανατρέξτε στο θέμα [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).

Όταν εκτελούνται αυτά τα βήματα, η εφαρμογή σας έχει ρυθμιστεί ώστε να χρησιμοποιήσετε την υπηρεσία παροχής εξόδου Cache Redis.

## <a name="next-steps"></a>Επόμενα βήματα

Δείτε την υπηρεσία [Παροχής κατάσταση περιόδου λειτουργίας ASP.NET για Azure Redis Cache](cache-aspnet-session-state-provider.md).
