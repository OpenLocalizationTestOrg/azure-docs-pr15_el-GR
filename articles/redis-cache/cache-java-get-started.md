<properties
   pageTitle="Πώς να χρησιμοποιείτε το Azure Redis Cache με Java | Microsoft Azure"
    description="Γρήγορα αποτελέσματα με το Cache Redis Azure χρησιμοποιώντας Java"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-java"></a>Πώς να χρησιμοποιείτε το Azure Redis Cache με Java

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis Cache παρέχει πρόσβαση σε μια αποκλειστική Redis cache, γίνεται από τη Microsoft. Το cache είναι προσβάσιμα από οποιαδήποτε εφαρμογή μέσα σε Windows Azure.

Αυτό το θέμα δείχνει πώς μπορείτε να ξεκινήσετε με το Azure Redis Cache χρησιμοποιώντας Java.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

[Jedis](https://github.com/xetorthio/jedis) - Java προγράμματος-πελάτη για Redis

Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί Jedis, αλλά μπορείτε να χρησιμοποιήσετε οποιονδήποτε υπολογιστή-πελάτη Java που αναφέρονται στην [http://redis.io/clients](http://redis.io/clients).

## <a name="create-a-redis-cache-on-azure"></a>Δημιουργήστε ένα cache Redis στο Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Ανακτήστε τα πλήκτρα πρόσβαση και όνομα κεντρικού υπολογιστή

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Ενεργοποιήστε το τελικό σημείο χωρίς SSL

Ορισμένα προγράμματα-πελάτες Redis δεν υποστηρίζει SSL και, από προεπιλογή τη [θύρα μη SSL είναι απενεργοποιημένη για νέες παρουσίες Azure Redis Cache](cache-configure.md#access-ports). Προς το παρόν, το πρόγραμμα-πελάτη [Jedis](https://github.com/xetorthio/jedis) δεν υποστηρίζει SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## <a name="add-something-to-the-cache-and-retrieve-it"></a>Προσθήκη κάτι στο cache και να το ανακτήσετε

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Επόμενα βήματα

- [Ενεργοποίηση Διαγνωστικά του cache](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) , ώστε να μπορείτε να κάνετε [Παρακολούθηση](https://msdn.microsoft.com/library/azure/dn763945.aspx) της εύρυθμης λειτουργίας του το cache.
- Διαβάστε την επίσημη [Redis τεκμηρίωση](http://redis.io/documentation).

