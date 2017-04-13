<properties
    pageTitle="Πώς να χρησιμοποιείτε το Azure Redis Cache με Python | Microsoft Azure"
    description="Γρήγορα αποτελέσματα με το Cache Redis Azure χρησιμοποιώντας Python"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-python"></a>Πώς να χρησιμοποιείτε το Azure Redis Cache με Python

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Αυτό το θέμα δείχνει πώς μπορείτε να ξεκινήσετε με το Azure Redis Cache χρησιμοποιώντας Python.


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Εγκαταστήστε το [redis γραφή](https://github.com/andymccurdy/redis-py).


## <a name="create-a-redis-cache-on-azure"></a>Δημιουργήστε ένα cache Redis στο Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Ανακτήστε τα πλήκτρα πρόσβαση και όνομα κεντρικού υπολογιστή

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Ενεργοποιήστε το τελικό σημείο χωρίς SSL

Ορισμένα προγράμματα-πελάτες Redis δεν υποστηρίζει SSL και, από προεπιλογή τη [θύρα μη SSL είναι απενεργοποιημένη για νέες παρουσίες Azure Redis Cache](cache-configure.md#access-ports). Προς το παρόν, το πρόγραμμα-πελάτη [redis γραφή](https://github.com/andymccurdy/redis-py) δεν υποστηρίζει SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Προσθήκη κάτι στο cache και να το ανακτήσετε


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Αντικατάσταση `<name>` με το όνομα του cache και `key` με τον αριθμό-κλειδί πρόσβασης.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
