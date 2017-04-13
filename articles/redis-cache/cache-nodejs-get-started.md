<properties
    pageTitle="Πώς να χρησιμοποιείτε το Azure Redis Cache με Node.js | Microsoft Azure"
    description="Γρήγορα αποτελέσματα με το Cache Redis Azure χρησιμοποιώντας Node.js και node_redis."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Πώς να χρησιμοποιείτε το Azure Redis Cache με Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis Cache παρέχει πρόσβαση σε μια ασφαλή, αποκλειστικό Redis cache, γίνεται από τη Microsoft. Το cache είναι προσβάσιμα από οποιαδήποτε εφαρμογή μέσα σε Windows Azure.

Αυτό το θέμα δείχνει πώς μπορείτε να ξεκινήσετε με το Azure Redis Cache χρησιμοποιώντας Node.js. Για ένα άλλο παράδειγμα χρήσης Azure Redis Cache με Node.js, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής Node.js συνομιλείτε με Socket.IO σε μια τοποθεσία Web του Azure](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Εγκαταστήστε το [node_redis](https://github.com/mranney/node_redis):

    npm install redis

Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί [node_redis](https://github.com/mranney/node_redis). Για παραδείγματα με άλλα προγράμματα-πελάτες Node.js, ανατρέξτε στην τεκμηρίωση μεμονωμένα για τα προγράμματα-πελάτες Node.js που αναφέρονται στην [Node.js Redis προγράμματα-πελάτες](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Δημιουργήστε ένα cache Redis στο Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Ανακτήστε τα πλήκτρα πρόσβαση και όνομα κεντρικού υπολογιστή

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Σύνδεση με το cache με ασφάλεια μέσω SSL

Η πιο πρόσφατες εκδόσεις του [node_redis](https://github.com/mranney/node_redis) παρέχει υποστήριξη για τη σύνδεση στο Azure Redis Cache μέσω SSL. Το παρακάτω παράδειγμα εμφανίζει τον τρόπο σύνδεσης στο Azure Redis Cache χρησιμοποιώντας το τελικό σημείο SSL της 6380. Αντικατάσταση `<name>` με το όνομα του το cache και `<key>` με είτε το πρωτεύον ή δευτερεύον κλειδί ως που περιγράφονται στην προηγούμενη ενότητα [Ανάκτηση τα πλήκτρα πρόσβαση και όνομα κεντρικού υπολογιστή](#retrieve-the-host-name-and-access-keys) .

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Προσθήκη κάτι στο cache και να το ανακτήσετε

Το παρακάτω παράδειγμα δείχνει πώς να συνδεθείτε με μια παρουσία Azure Redis Cache, και να αποθηκεύετε και να ανακτήσετε ένα στοιχείο από το cache. Για περισσότερα παραδείγματα με τη χρήση Redis με το πρόγραμμα-πελάτη [node_redis](https://github.com/mranney/node_redis) , ανατρέξτε στο θέμα [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Αποτέλεσμα:

    OK
    value


## <a name="next-steps"></a>Επόμενα βήματα

- [Ενεργοποίηση Διαγνωστικά του cache](cache-how-to-monitor.md#enable-cache-diagnostics) , ώστε να μπορείτε να κάνετε [Παρακολούθηση](cache-how-to-monitor.md) της εύρυθμης λειτουργίας του το cache.
- Διαβάστε την επίσημη [Redis τεκμηρίωση](http://redis.io/documentation).



