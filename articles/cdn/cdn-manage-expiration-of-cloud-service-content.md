<properties
 pageTitle="Πώς μπορείτε να διαχειριστείτε λήξης των υπηρεσιών Azure Web Apps/Cloud Services, ASP.NET και των υπηρεσιών IIS περιεχομένου στο Azure CDN | Microsoft Azure"
 description="Περιγράφει τον τρόπο διαχείρισης της λήξης περιεχομένου υπηρεσία cloud στο Azure CDN"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Πώς μπορείτε να διαχειριστείτε λήξης των υπηρεσιών Azure Web Apps/Cloud Services, ASP.NET ή των υπηρεσιών IIS περιεχομένου στο Azure CDN

> [AZURE.SELECTOR]
- [Υπηρεσίες εφαρμογών/Cloud Azure Web, ASP.NET ή των υπηρεσιών IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure χώρο αποθήκευσης αντικειμένων blob υπηρεσίας](cdn-manage-expiration-of-blob-content.md)

Αρχεία από οποιονδήποτε διακομιστή web, δυνατότητα δημόσιας πρόσβασης origin μπορεί να είναι προσωρινά στο Azure CDN μέχρι να περάσει το time to live (TTL).  Η τιμή TTL προσδιορίζεται από την [κεφαλίδα *Cache-Control* ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) στην απάντηση HTTP από το διακομιστή προέλευσης.  Σε αυτό το άρθρο περιγράφει τον τρόπο ρύθμισης `Cache-Control` κεφαλίδων για εφαρμογές Web Azure, τις υπηρεσίες Cloud Azure, εφαρμογές ASP.NET και τοποθεσίες Internet Information Services, τα οποία έχουν ρυθμιστεί οι παράμετροι ομοίως.

>[AZURE.TIP] Μπορείτε να επιλέξετε να ορίσετε χωρίς TTL σε ένα αρχείο.  Σε αυτήν την περίπτωση, Azure CDN εφαρμόζει αυτόματα έναν προεπιλεγμένο TTL από επτά ημέρες.
>
>Για περισσότερες πληροφορίες σχετικά με τη λειτουργία Azure CDN ταχύτητα πρόσβαση σε αρχεία και άλλους πόρους, ανατρέξτε στο θέμα η [Επισκόπηση CDN Azure](./cdn-overview.md).

## <a name="setting-cache-control-headers-in-configuration"></a>Ρύθμιση κεφαλίδες Cache-Control στη ρύθμιση παραμέτρων

Για στατικό περιεχόμενο, όπως εικόνες και φύλλα στυλ, μπορείτε να ελέγξετε τη συχνότητα ενημέρωσης, τροποποιώντας τα αρχεία **applicationHost.config** ή **web.config** για την εφαρμογή web.  Το στοιχείο **system.webServer\staticContent\clientCache** στο αρχείο παραμέτρων θα οριστεί η `Cache-Control` κεφαλίδα για το περιεχόμενό σας. Για **web.config**, τις ρυθμίσεις παραμέτρων θα επηρεάσει όλα τα στοιχεία στο φάκελο και όλους τους υποφακέλους, εκτός και εάν παρακαμφθούν στο επίπεδο υποφάκελο.  Για παράδειγμα, μπορείτε να ορίσετε μια προεπιλεγμένη time to live στη ρίζα να έχουν όλοι στατικό περιεχόμενο στο cache για 3 ημέρες, αλλά έχετε έναν υποφάκελο που έχει περισσότερα μεταβλητής περιεχομένου με ρύθμιση cache 6 ώρες.  Για **applicationHost.config**, όλες τις εφαρμογές στην τοποθεσία, θα επηρεαστεί, αλλά μπορείτε να παρακάμψετε στα αρχεία **web.config** στις εφαρμογές.

Το παρακάτω εμφανίζει XML και παράδειγμα ρύθμιση **clientCache** για να καθορίσετε μια μέγιστη ηλικία 3 ημερών:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Καθορισμός **UseMaxAge** προσθέτει μια `Cache-Control: max-age=<nnn>` κεφαλίδα στην απάντηση με βάση την τιμή που καθορίζεται στο χαρακτηριστικό **CacheControlMaxAge** . Είναι η μορφή για το χρονικό διάστημα για το χαρακτηριστικό **cacheControlMaxAge** είναι <days>. <hours>:<min>:<sec>. Για περισσότερες πληροφορίες σχετικά με τον κόμβο **clientCache** , ανατρέξτε στο θέμα [Cache του προγράμματος-πελάτη <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Ρύθμιση κεφαλίδες Cache στοιχείων ελέγχου στον κώδικα

Για τις εφαρμογές ASP.NET, μπορείτε να ορίσετε το CDN σε cache συμπεριφορά μέσω προγραμματισμού, ορίζοντας την ιδιότητα **HttpResponse.Cache** . Για περισσότερες πληροφορίες σχετικά με την ιδιότητα **HttpResponse.Cache** , ανατρέξτε στο θέμα [HttpResponse.Cache ιδιότητα](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) και [HttpCachePolicy τάξης](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Εάν θέλετε να μέσω προγραμματισμού cache εφαρμογή περιεχομένου στο ASP.NET, βεβαιωθείτε ότι το περιεχόμενο έχει σημανθεί ως δυνατότητα αποθήκευσης στο cache, ορίζοντας HttpCacheability σε *δημόσια*. Επίσης, βεβαιωθείτε ότι έχει οριστεί ένα επικύρωσης cache. Η εφαρμογή επικύρωσης cache μπορεί να είναι μια τελευταία τροποποίηση χρονικής σήμανσης Ορισμός καλώντας SetLastModified ή μια τιμή etag Ορισμός καλώντας SetETag. Προαιρετικά, μπορείτε επίσης να καθορίσετε μια ώρα λήξης cache καλώντας SetExpires ή να χρησιμοποιείτε την προεπιλεγμένη λύσεις cache που περιγράφονται προηγουμένως σε αυτό το έγγραφο.  

Για παράδειγμα, για να cache περιεχομένου για μία ώρα, προσθέστε τα εξής:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Επόμενα βήματα

- [Διαβάστε λεπτομέρειες σχετικά με το στοιχείο **clientCache**](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [Διαβάστε την τεκμηρίωση για την ιδιότητα **HttpResponse.Cache**](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [Διαβάστε την τεκμηρίωση για την **Κλάση HttpCachePolicy**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  
