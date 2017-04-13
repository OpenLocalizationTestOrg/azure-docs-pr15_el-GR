<properties 
    pageTitle="Κατάσταση περιόδου λειτουργίας με Azure Redis cache στο Azure εφαρμογής υπηρεσίας" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία Cache Azure για την υποστήριξη σε cache του ASP.NET περιόδου λειτουργίας κατάσταση." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Κατάσταση περιόδου λειτουργίας με Azure Redis cache στο Azure εφαρμογής υπηρεσίας


Αυτό το θέμα εξηγεί τον τρόπο χρήσης της υπηρεσίας Cache Redis Azure για κατάσταση περιόδου λειτουργίας.

Εάν η εφαρμογή web της ASP.NET χρησιμοποιεί κατάσταση περιόδου λειτουργίας, θα πρέπει να ρυθμίσετε τις παραμέτρους μιας υπηρεσίας παροχής κατάσταση εξωτερικών περιόδου λειτουργίας (της υπηρεσίας Cache Redis ή μια υπηρεσία παροχής κατάσταση λειτουργίας SQL Server). Εάν χρησιμοποιείτε κατάσταση περιόδου λειτουργίας και δεν χρησιμοποιείτε μια εξωτερική υπηρεσία παροχής, θα περιορίζεται σε μία παρουσία της εφαρμογής web. Η υπηρεσία Cache Redis είναι η πιο ασφαλής και απλούστερος για να ενεργοποιήσετε.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>Δημιουργία του Cache
Ακολουθήστε [αυτές τις οδηγίες](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) για να δημιουργήσετε το cache.

##<a id="configureproject"></a>Προσθήκη του πακέτου RedisSessionStateProvider NuGet σε εφαρμογή web
Εγκαταστήστε το NuGet `RedisSessionStateProvider` πακέτου.  Χρησιμοποιήστε την ακόλουθη εντολή για να εγκαταστήσετε από την Κονσόλα διαχείρισης πακέτου (**Εργαλεία** > **Διαχείρισης πακέτου NuGet** > **Κονσόλα διαχείρισης πακέτου**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
Για να εγκαταστήσετε από τα **Εργαλεία** > **Διαχείρισης πακέτου NuGet** > **Διαχείριση πακέτων NugGet για τη λύση**, αναζητήστε `RedisSessionStateProvider`.

Για περισσότερες πληροφορίες ανατρέξτε στο θέμα της [σελίδας NuGet RedisSessionStateProvider](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) και [Ρύθμιση παραμέτρων του προγράμματος-πελάτη cache](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

##<a id="configurewebconfig"></a>Τροποποίηση του αρχείου Web.Config
Εκτός από την πραγματοποίηση αναφορές συγκρότησης για Cache, το πακέτο NuGet προσθέτει στέλεχος καταχωρήσεις στο αρχείο *web.config* . 

1. Ανοίξτε το *αρχείο web.config* και βρείτε το το στοιχείο **sessionState** .

1. Πληκτρολογήστε τις τιμές για `host`, `accessKey`, `port` (η θύρα SSL πρέπει να είναι 6380), και ορίστε `SSL` να `true`. Μπορείτε να αποκτήσετε αυτές τις τιμές από την [Πύλη Azure](http://go.microsoft.com/fwlink/?LinkId=529715) blade για την παρουσία cache. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [σύνδεση στο cache](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Σημειώστε ότι η θύρα μη SSL είναι απενεργοποιημένη από προεπιλογή για νέες μνήμης cache. Για περισσότερες πληροφορίες σχετικά με την ενεργοποίηση της θύρας χωρίς SSL, ανατρέξτε στην ενότητα [Πρόσβαση θύρες](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) στο θέμα [Ρύθμιση παραμέτρων cache στο Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) . Το παρακάτω σημάνσεις: Εμφανίζει τις αλλαγές στο αρχείο *web.config* , ειδικά τις αλλαγές να *θύρα*, *κεντρικός υπολογιστής*, accessKey*, και *ssl *.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a>Χρησιμοποιήστε το αντικείμενο περιόδου λειτουργίας στον κώδικα
Το τελικό βήμα είναι να αρχίσετε να χρησιμοποιείτε το αντικείμενο περιόδου λειτουργίας στον κώδικα ASP.NET. Μπορείτε να προσθέσετε αντικείμενα στην κατάσταση περιόδου λειτουργίας, χρησιμοποιώντας τη μέθοδο **Session.Add** . Αυτή η μέθοδος χρησιμοποιεί ζεύγη κλειδιού-τιμής για την αποθήκευση στοιχείων στο cache κατάσταση περιόδου λειτουργίας.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Ο ακόλουθος κώδικας ανακτά αυτήν την τιμή από την κατάσταση λειτουργίας.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

Μπορείτε επίσης να χρησιμοποιήσετε το Cache Redis στο cache αντικειμένων στην εφαρμογή web. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εφαρμογή ταινίας MVC με Azure Redis Cache σε 15 λεπτά](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Για περισσότερες λεπτομέρειες σχετικά με τη χρήση περιόδου λειτουργίας ASP.NET, ανατρέξτε στο θέμα [Επισκόπηση κατάστασης περιόδου λειτουργίας ASP.NET][].

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751), όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή web μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

## <a name="whats-changed"></a>Τι έχει αλλάξει
* Για οδηγίες για την αλλαγή από τοποθεσίες Web App υπηρεσία ανατρέξτε στο θέμα: [Azure εφαρμογής υπηρεσίας και τον αντίκτυπο σχετικά με τις υπάρχουσες υπηρεσίες Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

  *Με [Rick Anderson](https://twitter.com/RickAndMSFT)*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [Επισκόπηση της περιόδου λειτουργίας ASP.NET]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 
