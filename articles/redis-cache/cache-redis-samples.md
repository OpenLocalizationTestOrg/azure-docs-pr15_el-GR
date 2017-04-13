<properties 
    pageTitle="Azure δείγματα Redis Cache | Microsoft Azure" 
    description="Μάθετε πώς να χρησιμοποιείτε το Azure Redis Cache" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-samples"></a>Azure δείγματα Redis Cache 

Αυτό το θέμα παρέχει μια λίστα με δείγματα Azure Redis Cache, που καλύπτεται από σενάρια, όπως τη σύνδεση σε cache, ανάγνωσης και εγγραφής δεδομένων προς και από το cache και χρησιμοποιώντας τις υπηρεσίες παροχής ASP.NET Redis Cache. Ορισμένα από τα δείγματα είναι έργα με δυνατότητα λήψης, ενώ ορισμένες παρέχουν οδηγίες βήμα προς βήμα και να συμπεριλάβετε τμήματα κώδικα αλλά χωρίς σύνδεση με ένα έργο με δυνατότητα λήψης.

## <a name="hello-world-samples"></a>Δείγματα κόσμο Γεια σας

Τα δείγματα σε αυτήν την ενότητα εμφανίζονται τα βασικά στοιχεία για τη σύνδεση σε μια παρουσία Azure Redis Cache και ανάγνωση και εγγραφή δεδομένων στο cache χρησιμοποιώντας μια ποικιλία γλώσσες και Redis προγράμματα-πελάτες.

Το δείγμα [Χαίρετε](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) δείχνει πώς μπορείτε να εκτελέσετε διάφορες λειτουργίες cache χρησιμοποιώντας το πρόγραμμα-πελάτη .NET [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .

Αυτό το δείγμα δείχνει πώς μπορείτε να:

-   Χρησιμοποιήστε διάφορες επιλογές σύνδεσης
-   Ανάγνωση και εγγραφή αντικειμένων από το cache με σύγχρονη και ασύγχρονων λειτουργιών και
-   Χρησιμοποιήστε Redis MGET/MSET εντολές για την επιστροφή τιμών καθορισμένο πλήκτρων
-   Εκτέλεση λειτουργιών συναλλαγών Redis
-   Εργασία με λίστες Redis και ταξινομημένο σύνολα
-   Χώρος αποθήκευσης αντικειμένων .NET χρησιμοποιώντας JsonConvert serializers
-   Χρήση Redis σύνολα για την υλοποίηση εφαρμογή ετικετών
-   Εργασία με Redis συμπλέγματος

Για περισσότερες πληροφορίες, ανατρέξτε στην τεκμηρίωση [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) στο github και για περισσότερες σενάρια χρήσης ανατρέξτε στο θέμα τις δοκιμές [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) μονάδα.

[Πώς να χρησιμοποιείτε το Cache Redis Azure με Python](cache-python-get-started.md) δείχνει τον τρόπο γρήγορα αποτελέσματα με το Azure Redis Cache χρησιμοποιώντας Python και ο υπολογιστής-πελάτης [redis γραφή](https://github.com/andymccurdy/redis-py) .

[Εργασία με το .NET αντικειμένων στο cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) εμφανίζει ένας τρόπος για να σειριοποιεί αντικείμενα .NET, ώστε να μπορείτε να γράψετε να και να τα διαβάζετε από μια παρουσία Azure Redis Cache. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Χρήση Redis Cache ως μια κλίμακα ανάληψη βασική μονάδα για ASP.NET SignalR

Το δείγμα [Χρησιμοποιήστε Redis Cache ως κλίμακα ανάληψη βασική μονάδα για ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε Azure Redis Cache ως μια βασική μονάδα SignalR. Για περισσότερες πληροφορίες σχετικά με τη βασική μονάδα, ανατρέξτε στο θέμα [SignalR Scaleout με Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Redis δείγμα ερωτήματος πελάτη Cache

Αυτό το δείγμα παρουσιάζει συγκρίνει επιδόσεων μεταξύ πρόσβαση στα δεδομένα από ένα cache και την πρόσβαση σε δεδομένα από το χώρο αποθήκευσης διατήρησης. Αυτό το δείγμα περιλαμβάνει δύο έργα.

-   [Επίδειξη πώς Redis Cache να βελτιώσετε την απόδοση από την προσωρινή αποθήκευση δεδομένων](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [Σπόρων τη βάση δεδομένων και Cache για την επίδειξη](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>Κατάσταση περιόδου λειτουργίας ASP.NET και cache εμφάνισης σελίδας

Το δείγμα [Χρησιμοποιήστε Azure Redis Cache για την αποθήκευση ASP.NET SessionState και OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) δείχνει πώς να χρησιμοποιείτε Azure Redis Cache για να αποθηκεύσετε την περίοδο λειτουργίας ASP.NET και Cache εμφάνισης χρησιμοποιώντας τις υπηρεσίες παροχής SessionState και OutputCache για Redis.

## <a name="manage-azure-redis-cache-with-maml"></a>Διαχείριση Cache Azure Redis με MAML

Το δείγμα [Διαχείριση Cache Redis Azure χρησιμοποιώντας βιβλιοθήκες διαχείρισης Azure](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε βιβλιοθήκες διαχείρισης Azure για τη Διαχείριση - (δημιουργία / ενημέρωση / διαγραφή) το Cache. 

## <a name="custom-monitoring-sample"></a>Προσαρμοσμένη παρακολούθηση δείγματος

Το δείγμα [δεδομένων Access Redis παρακολούθηση Cache](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) παρουσιάζει πώς μπορείτε να αποκτήσετε πρόσβαση δεδομένων παρακολούθησης για το Cache Redis Azure εκτός της πύλης Azure.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Ένα στυλ Twitter κλωνοποίηση δημιουργηθεί με τη χρήση PHP και Redis

Το δείγμα [Retwis](https://github.com/SyntaxC4-MSFT/retwis) είναι η Redis Γεια. Είναι ένα ελάχιστο κλωνοποίηση κοινωνικό δίκτυο Twitter στυλ δημιουργηθεί με τη χρήση Redis και PHP χρησιμοποιώντας το πρόγραμμα-πελάτη [Predis](https://github.com/nrk/predis) . Τον κωδικό προέλευσης που έχει σχεδιαστεί για να είναι πολύ απλές και την ίδια στιγμή για να εμφανίσετε διαφορετικές Redis δομές δεδομένων.

## <a name="bandwidth-monitor"></a>Οθόνη εύρους ζώνης

Το δείγμα [εύρους ζώνης οθόνη](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) σάς επιτρέπει να παρακολουθείτε το εύρος ζώνης που χρησιμοποιείται στο πρόγραμμα-πελάτη. Για να μετρήσετε το εύρος ζώνης, εκτελέστε το δείγμα στον υπολογιστή-πελάτη cache, πραγματοποίηση κλήσεων στο cache και παρατηρήστε το εύρος ζώνης που αναφέρεται από το δείγμα οθόνη εύρους ζώνης.
