<properties
    pageTitle="Εφαρμογή σε-εσωτερικής εγκατάστασης/cloud υβριδική (.NET) | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια εφαρμογή στην-εσωτερικής εγκατάστασης/cloud υβριδική .NET χρησιμοποιώντας τη μετάδοση Bus υπηρεσίας Azure."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="net-on-premisescloud-hybrid-application-using-azure-service-bus-relay"></a>Εφαρμογή στην-εσωτερικής εγκατάστασης/cloud υβριδική .NET χρησιμοποιώντας Azure Service Bus μετάδοση

## <a name="introduction"></a>Εισαγωγή

Σε αυτό το άρθρο περιγράφει τον τρόπο για να δημιουργήσετε μια εφαρμογή υβριδική cloud με το Microsoft Azure και του Visual Studio. Το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε χωρίς προηγούμενη εμπειρία με χρήση Azure. Λιγότερο από 30 λεπτά, θα έχετε μια εφαρμογή που χρησιμοποιεί πολλούς πόρους Azure προς τα επάνω και την εκτέλεση στο cloud.

Θα μάθετε:

-   Μάθετε πώς μπορείτε να δημιουργήσετε ή να προσαρμόσετε μια υπάρχουσα υπηρεσία web για την κατανάλωση από μια λύση web.
-   Πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία Azure Service Bus μετάδοση για την κοινή χρήση δεδομένων μεταξύ μια εφαρμογή του Azure και μια υπηρεσία web φιλοξενείται σε άλλη θέση.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Πώς η μετάδοση Bus υπηρεσίας βοηθούν στην υβριδική λύσεων

Επιχειρηματικές λύσεις συνήθως αποτελούνται από ένα συνδυασμό προσαρμοσμένου κώδικα να αντιμετωπίσουν τις νέες και μοναδικές επιχειρηματικές απαιτήσεις και τις υπάρχουσες λειτουργίες που παρέχονται από λύσεις και συστήματα που βρίσκονται ήδη στη θέση.

Λύση αρχιτέκτονες αρχίσουν να χρησιμοποιούν το στο cloud για ευκολότερη διαχείριση των απαιτήσεων κλίμακα και χαμηλότερο κόστος λειτουργίας. Σε αυτήν την περίπτωση, ότι τη βρίσκουν ότι επίτευξη υπάρχουσα υπηρεσία παγίων αυτές θα θέλατε να αξιοποιήσετε όπως τα μπλοκ δόμησης για λύσεις τους είναι εντός το εταιρικό τείχος προστασίας και έξοδος από το εύκολο για πρόσβαση από τη λύση cloud. Πολλές εσωτερικές υπηρεσίες δεν είναι ενσωματωμένη ή φιλοξενούνται με τον τρόπο που αυτές μπορεί να είναι εύκολα που εκτίθενται στο άκρο εταιρικό δίκτυο.

Η μετάδοση Bus υπηρεσίας έχει σχεδιαστεί για την υπόθεση χρήσης λήψης υπάρχουσες υπηρεσίες web Windows Communication Foundation (WCF) και πραγματοποίηση αυτών των υπηρεσιών ασφαλή πρόσβαση σε λύσεις που βρίσκονται έξω από την περίμετρο εταιρικό χωρίς ενοχλητικές αλλαγές στην υποδομή του εταιρικού δικτύου. Οι υπηρεσίες μεταγωγής Bus υπηρεσίας φιλοξενούνται ακόμη μέσα το υπάρχον περιβάλλον, αλλά μπορούν να αναθέτουν ακρόαση για εισερχόμενες περίοδοι λειτουργίας και των αιτήσεων το Bus υπηρεσία που φιλοξενείται στο cloud. Υπηρεσία Bus προστατεύει επίσης αυτών των υπηρεσιών από μη εξουσιοδοτημένη πρόσβαση με χρήση του ελέγχου ταυτότητας [Θέσει σε κοινή χρήση υπογραφής Access](../service-bus-messaging/service-bus-sas-overview.md) (συσχετισμών Ασφαλείας).

## <a name="solution-scenario"></a>Σενάριο λύσης

Σε αυτό το πρόγραμμα εκμάθησης, θα δημιουργήσετε μια τοποθεσία Web ASP.NET που σας επιτρέπει να δείτε μια λίστα με τα προϊόντα στη σελίδα απόθεμα του προϊόντος.

![][0]

Το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε πληροφορίες προϊόντος σε ένα υπάρχον σύστημα εσωτερικής εγκατάστασης και χρησιμοποιεί η μετάδοση Bus υπηρεσία για την επίτευξη σε αυτό το σύστημα. Αυτό είναι προσομοιωμένη από μια υπηρεσία web που εκτελείται σε μια εφαρμογή κονσόλας απλές και υποστηρίζεται από ένα σύνολο στη μνήμη προϊόντων. Θα μπορούν να εκτελέσουν αυτήν την εφαρμογή κονσόλας στον δικό σας υπολογιστή και ανάπτυξη του ρόλου web σε Azure. Με αυτόν τον τρόπο, θα δείτε πώς θα καλούν στην πραγματικότητα του ρόλου web εκτελείται στο το Azure κέντρο δεδομένων στον υπολογιστή σας, παρόλο που τον υπολογιστή σας σχεδόν σίγουρα θα βρίσκονται πίσω από τουλάχιστον ένα τείχος προστασίας και επιπέδου δίκτυο διεύθυνση μετάφραση (NAT).

Ακολουθεί ένα στιγμιότυπο οθόνης της αρχικής σελίδας της εφαρμογής web ολοκληρώθηκε.

![][1]

## <a name="set-up-the-development-environment"></a>Ρυθμίστε το περιβάλλον ανάπτυξης

Πριν να ξεκινήσετε Azure εφαρμογές προγραμματισμού, λάβετε τα εργαλεία και να ρυθμίσετε το περιβάλλον ανάπτυξης.

1.  Εγκατάσταση του SDK Azure για το .NET από τη σελίδα [γρήγορα εργαλεία και SDK][] .

2.  Κάντε κλικ στην επιλογή **εγκατάσταση του SDK** για την έκδοση του Visual Studio που χρησιμοποιείτε. Τα βήματα σε αυτό το πρόγραμμα εκμάθησης Χρησιμοποιήστε το Visual Studio 2015.

4.  Όταν σας ζητηθεί να εκτελέσετε ή να αποθηκεύσετε το πρόγραμμα εγκατάστασης, κάντε κλικ στην επιλογή **Εκτέλεση**.

5.  Στο **Πρόγραμμα εγκατάστασης πλατφόρμας Web**, κάντε κλικ στην επιλογή **εγκατάσταση** και συνεχίσετε με την εγκατάσταση.

6.  Μόλις ολοκληρωθεί η εγκατάσταση, θα έχετε όλα όσα χρειάζεται για να ξεκινήσετε την ανάπτυξη της εφαρμογής. Το SDK περιλαμβάνει εργαλεία που σας επιτρέπουν να αναπτύξετε εύκολα Azure εφαρμογές στο Visual Studio. Εάν δεν έχετε εγκατεστημένο το Visual Studio, το SDK εγκαθιστά το δωρεάν Visual Studio Express.

## <a name="create-a-namespace"></a>Δημιουργία ενός χώρου ονομάτων

Για να ξεκινήσετε να χρησιμοποιείτε τις δυνατότητες Bus υπηρεσίας στο Azure, πρέπει πρώτα να δημιουργήσετε ένα χώρο ονομάτων υπηρεσίας. Ένα χώρο ονομάτων παρέχει ένα κοντέινερ πεδίου για την προσθήκη διεύθυνσης σε πόρους Bus υπηρεσίας στην εφαρμογή σας.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Δημιουργήστε ένα διακομιστή εσωτερικής εγκατάστασης

Πρώτα, θα μπορείτε να δημιουργήσετε ένα σύστημα καταλόγου προϊόντος (mock) εσωτερικής εγκατάστασης. Θα είναι αρκετά απλή. Μπορείτε να το δείτε ως που αντιπροσωπεύει ένα σύστημα καταλόγου προϊόντος πραγματική εσωτερικής εγκατάστασης με μια επιφάνεια ολοκλήρωσης υπηρεσίας που Προσπαθούμε να ενοποιήσετε.

Αυτό το έργο είναι μια εφαρμογή κονσόλας Visual Studio, και χρησιμοποιεί το [πακέτο NuGet Bus Azure υπηρεσίας](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) για να συμπεριλάβετε τις βιβλιοθήκες Bus υπηρεσίας και τις ρυθμίσεις παραμέτρων.

### <a name="create-the-project"></a>Δημιουργία έργου

1.  Με δικαιώματα διαχειριστή, ξεκινήστε Microsoft Visual Studio. Για να ξεκινήσετε Visual Studio με δικαιώματα διαχειριστή, κάντε δεξί κλικ στο εικονίδιο του προγράμματος **Visual Studio** και, στη συνέχεια, κάντε κλικ στην επιλογή **Εκτέλεση ως διαχειριστής**.

2.  Στο Visual Studio, στο μενού **αρχείο** , κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **έργο**.

3.  Από τα **Εγκατεστημένα πρότυπα**, στην περιοχή **Visual C#**, κάντε κλικ στην επιλογή **Εφαρμογής κονσόλας**. Στο πλαίσιο **όνομα** , πληκτρολογήστε το όνομα **ProductsServer**:

    ![][11]

4.  Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο **ProductsServer** .

7.  Εάν έχετε ήδη εγκαταστήσει το NuGet διαχείρισης πακέτου για το Visual Studio, μεταβείτε στο επόμενο βήμα. Διαφορετικά, επισκεφθείτε [NuGet][] και κάντε κλικ στην επιλογή [Εγκατάσταση NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Ακολουθήστε τις οδηγίες για την εγκατάσταση της διαχείρισης πακέτου NuGet και, στη συνέχεια, ξεκινήστε πάλι το Visual Studio.

7.  Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **ProductsServer** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.

8.  Κάντε κλικ στην καρτέλα **Αναζήτηση** και, στη συνέχεια, αναζητήστε το `Microsoft Azure Service Bus`. Κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης.

    ![][13]

    Σημειώστε ότι τώρα αναφέρονται οι συγκροτήσεις απαιτείται πρόγραμμα-πελάτη.

9.  Προσθέστε μια νέα κλάση για το συμβόλαιο προϊόντος. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **ProductsServer** και κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **τάξης**.

10. Στο πλαίσιο **όνομα** , πληκτρολογήστε το όνομα **ProductsContract.cs**. Στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

11. Στο **ProductsContract.cs**, αντικαταστήστε τον ορισμό χώρο ονομάτων με τον ακόλουθο κώδικα, η οποία ορίζει τη σύμβαση για την υπηρεσία.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. Στο Program.cs, αντικαταστήστε τον ορισμό χώρο ονομάτων με τον ακόλουθο κώδικα, η οποία προσθέτει της υπηρεσίας προφίλ και τον κεντρικό υπολογιστή για αυτήν.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. Στην Εξερεύνηση λύσεων, κάντε διπλό κλικ στο αρχείο **App.config** για να το ανοίξετε στο πρόγραμμα επεξεργασίας Visual Studio. Στο κάτω μέρος του ** &lt;συστήματος. ServiceModel&gt; ** στοιχείο (αλλά εξακολουθείτε να μέσα &lt;συστήματος. ServiceModel&gt;), προσθέστε τον ακόλουθο κώδικα XML. Βεβαιωθείτε ότι για να αντικαταστήσετε *yourServiceNamespace* με το όνομα του χώρου ονομάτων και *yourKey* με τον αριθμό-κλειδί συσχετισμών Ασφαλείας που ανακτήσατε νωρίτερα από την πύλη:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. Βρίσκεται ακόμη στο App.config, στο το ** &lt;appSettings&gt; ** στοιχείο, αντικατάσταση τιμή συμβολοσειράς της σύνδεσης με τη συμβολοσειρά σύνδεσης που έχετε λάβει προηγουμένως από την πύλη. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Πατήστε το **Συνδυασμό πλήκτρων Ctrl + Shift + B** ή από το μενού " **Δημιουργία** ", κάντε κλικ στην επιλογή **Δημιουργία λύση** για να δημιουργήσετε την εφαρμογή και να επαληθεύσετε την ακρίβεια της εργασίας σας μέχρι στιγμής.

## <a name="create-an-aspnet-application"></a>Δημιουργία μιας εφαρμογής του ASP.NET

Σε αυτήν την ενότητα θα μπορείτε να δημιουργήσετε μια απλή εφαρμογή ASP.NET που εμφανίζει δεδομένα που έχουν ανακτηθεί από την υπηρεσία του προϊόντος.

### <a name="create-the-project"></a>Δημιουργία έργου

1.  Βεβαιωθείτε ότι εκτελείται το Visual Studio με δικαιώματα διαχειριστή.

2.  Στο Visual Studio, στο μενού **αρχείο** , κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **έργο**.

3.  Από **Εγκατεστημένα πρότυπα**, στην περιοχή **Visual C#**, κάντε κλικ στην επιλογή **Εφαρμογής Web ASP.NET**. Το όνομα του έργου **ProductsPortal**. Στη συνέχεια, κάντε κλικ στο **κουμπί OK**.

    ![][15]

4.  Από τη λίστα **Επιλέξτε ένα πρότυπο** , κάντε κλικ στην επιλογή **MVC**. 

6.  Επιλέξτε το πλαίσιο για τον **κεντρικό υπολογιστή στο cloud**.

    ![][16]

5. Κάντε κλικ στο κουμπί **Αλλαγή ελέγχου ταυτότητας** . Στο παράθυρο διαλόγου **Αλλαγή ελέγχου ταυτότητας** , κάντε κλικ στην επιλογή **Χωρίς έλεγχο ταυτότητας**και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**. Για αυτό το πρόγραμμα εκμάθησης, θα ανάπτυξη μια εφαρμογή που δεν χρειάζεται μια σύνδεση χρήστη.

    ![][18]

6.  Στην ενότητα **Microsoft Azure** του παραθύρου διαλόγου **Νέο έργο ASP.NET** , βεβαιωθείτε ότι είναι επιλεγμένο το πλαίσιο ελέγχου **κεντρικού υπολογιστή στο cloud** και ότι είναι επιλεγμένο το **Εφαρμογής υπηρεσίας** στην αναπτυσσόμενη λίστα.

    ![][19]

7. Κάντε κλικ στο **κουμπί OK**. 

8. Τώρα θα πρέπει να ορίσετε Azure πόρους για μια νέα εφαρμογή web. Ακολουθήστε όλα τα βήματα στην ενότητα [Ρύθμιση παραμέτρων Azure πόρους για μια νέα εφαρμογή web](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app). Στη συνέχεια, επιστρέψτε σε αυτό το πρόγραμμα εκμάθησης και προχωρήστε στο επόμενο βήμα.

5.  Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ σε **μοντέλα** και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**, στη συνέχεια, κάντε κλικ στην επιλογή **τάξης**. Στο πλαίσιο **όνομα** , πληκτρολογήστε το όνομα **Product.cs**. Στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

    ![][17]

### <a name="modify-the-web-application"></a>Τροποποίηση της εφαρμογής web

1.  Στο αρχείο Product.cs στο Visual Studio, αντικαταστήστε τον υπάρχοντα ορισμό χώρο ονομάτων με τον ακόλουθο κώδικα.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  Στην Εξερεύνηση λύσεων, αναπτύξτε το φάκελο **ελεγκτές** και, στη συνέχεια, κάντε διπλό κλικ στο αρχείο **HomeController.cs** για να το ανοίξετε στο Visual Studio.

3. Στο **HomeController.cs**, αντικαταστήστε τον υπάρχοντα ορισμό χώρο ονομάτων με τον ακόλουθο κώδικα.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  Στην Εξερεύνηση λύσεων, αναπτύξτε το φάκελο Views\Shared και, στη συνέχεια, κάντε διπλό κλικ για να το ανοίξετε στο πρόγραμμα επεξεργασίας Visual Studio **_Layout.cshtml** .

5.  Για να αλλάξετε όλες τις εμφανίσεις της **Εφαρμογής μου ASP.NET** στα **προϊόντα του LITWARE**.

6. Καταργήστε τις συνδέσεις ** **για οικιακή χρήση**, και **επαφή** **. Στο παρακάτω παράδειγμα, διαγράψτε την επισήμανση κώδικα.

    ![][41]

7.  Στην Εξερεύνηση λύσεων, αναπτύξτε το φάκελο Views\Home και, στη συνέχεια, κάντε διπλό κλικ για να το ανοίξετε στο πρόγραμμα επεξεργασίας Visual Studio **Index.cshtml** .
    Αντικαταστήστε όλα τα περιεχόμενα του αρχείου με τον ακόλουθο κώδικα.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Για να επιβεβαιώσετε την ακρίβεια της εργασίας σας μέχρι στιγμής, μπορείτε να πατήσετε το **Συνδυασμό πλήκτρων Ctrl + Shift + B** για να δημιουργήσετε το έργο.


### <a name="run-the-app-locally"></a>Εκτελέστε την εφαρμογή τοπικά

Εκτελέστε την εφαρμογή για να επαληθεύσετε ότι λειτουργεί.

1.  Βεβαιωθείτε ότι **ProductsPortal** είναι το ενεργό έργο. Κάντε δεξί κλικ στο όνομα του έργου στο Εξερεύνηση λύσεων και επιλέξτε **Ορισμός ως εκκίνησης έργου**.
2.  Στο Visual Studio, πατήστε το πλήκτρο F5.
3.  Θα πρέπει να εμφανίζεται η εφαρμογή σας, εκτελείται σε ένα πρόγραμμα περιήγησης.

    ![][21]

## <a name="put-the-pieces-together"></a>Τοποθέτηση τμήματα μαζί

Το επόμενο βήμα είναι να συνδέσετε τα προϊόντα διακομιστή εσωτερικής εγκατάστασης με την εφαρμογή του ASP.NET.

1.  Εάν δεν είναι ήδη ανοιχτή, στο Visual Studio ανοίξτε ξανά το έργο **ProductsPortal** που δημιουργήσατε στην ενότητα [Δημιουργία μιας εφαρμογής του ASP.NET](#create-an-aspnet-application) .

2.  Παρόμοιο με το βήμα στην ενότητα "Δημιουργία του διακομιστή εσωτερικής εγκατάστασης", προσθέστε το πακέτο NuGet για τις αναφορές του έργου. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **ProductsPortal** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.

3.  Αναζήτηση για "Υπηρεσία Bus" και επιλέξτε το στοιχείο **Microsoft Azure Service Bus** . Στη συνέχεια, ολοκληρώστε την εγκατάσταση και κλείστε το παράθυρο διαλόγου.

4.  Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **ProductsPortal** και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**, στη συνέχεια, **Υπάρχον στοιχείο**.

5.  Μεταβείτε στο αρχείο **ProductsContract.cs** από το έργο κονσόλας **ProductsServer** . Κάντε κλικ για να επισημάνετε ProductsContract.cs. Κάντε κλικ στο κάτω βέλος δίπλα στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη ως σύνδεσης**.

    ![][24]

6.  Τώρα, ανοίξτε το αρχείο **HomeController.cs** στο πρόγραμμα επεξεργασίας Visual Studio και αντικαταστήστε τον ορισμό χώρο ονομάτων με τον ακόλουθο κώδικα. Φροντίστε να αντικαταστήσετε *yourServiceNamespace* με το όνομα των χώρος ονομάτων υπηρεσίας και *yourKey* με τον αριθμό-κλειδί συσχετισμών Ασφαλείας. Αυτό θα ενεργοποιήσει το πρόγραμμα-πελάτη για να καλέσετε την υπηρεσία εσωτερικής εγκατάστασης, επιστρέφει το αποτέλεσμα της κλήσης.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ της λύσης **ProductsPortal** (Φροντίστε να κάνετε δεξί κλικ τη λύση, όχι το έργο). Κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **Υπάρχον έργο**.

8.  Μεταβείτε στο έργο **ProductsServer** και, στη συνέχεια, κάντε διπλό κλικ στο αρχείο **ProductsServer.csproj** λύσεων για να την προσθέσετε.

9.  Για να εμφανίσετε τα δεδομένα σε **ProductsPortal**, πρέπει να εκτελεί **ProductsServer** . Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ της λύσης **ProductsPortal** και κάντε κλικ στην επιλογή **Ιδιότητες**. Εμφανίζεται το παράθυρο διαλόγου **Ιδιοτήτων σελίδες** .

10. Στην αριστερή πλευρά, κάντε κλικ στην επιλογή **Project εκκίνησης**. Στη δεξιά πλευρά, κάντε κλικ στην επιλογή **πολλών έργων εκκίνησης**. Βεβαιωθείτε ότι **ProductsServer** και **ProductsPortal** εμφανίζονται, σε αυτήν τη σειρά, με **Έναρξη** Ορισμός με την ενέργεια για τα δύο.

      ![][25]

11. Ακόμη στο παράθυρο διαλόγου **Ιδιότητες** , κάντε κλικ στην επιλογή **Εξαρτήσεις μεταξύ έργων** στην αριστερή πλευρά.

12. Στη λίστα **έργα** , κάντε κλικ στην επιλογή **ProductsServer**. Βεβαιωθείτε ότι είναι **ProductsPortal** **δεν** είναι επιλεγμένο.

14. Στη λίστα **έργα** , κάντε κλικ στην επιλογή **ProductsPortal**. Βεβαιωθείτε ότι είναι επιλεγμένο το **ProductsServer** . 

    ![][26]

15. Κάντε κλικ στο **κουμπί OK** στο παράθυρο διαλόγου **Ιδιοτήτων σελίδες** .

## <a name="run-the-project-locally"></a>Εκτελέστε το έργο τοπικά

Για να ελέγξετε την εφαρμογή τοπικά, στο Visual Studio, πιέστε το πλήκτρο **F5**. Πρέπει να ξεκινά η πρώτη διακομιστή εσωτερικής εγκατάστασης (**ProductsServer**) και, στη συνέχεια, θα πρέπει να ξεκινήσει η εφαρμογή **ProductsPortal** σε ένα παράθυρο του προγράμματος περιήγησης. Αυτήν τη φορά, θα δείτε ότι το απόθεμα του προϊόντος λίστες δεδομένα που ανακτήθηκαν από το σύστημα προϊόντων υπηρεσίας εσωτερικής εγκατάστασης.

![][10]

Πατήστε " **Ανανέωση** " στη σελίδα **ProductsPortal** . Κάθε φορά που μπορείτε να ανανεώσετε τη σελίδα, θα δείτε την εφαρμογή διακομιστή, εμφανίζεται ένα μήνυμα όταν `GetProducts()` από **ProductsServer** ονομάζεται.

Κλείστε τις εφαρμογές και τις δύο πριν να προχωρήσετε στο επόμενο βήμα.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Αναπτύξτε το έργο ProductsPortal σε μια εφαρμογή Azure web

Το επόμενο βήμα είναι να μετατρέψετε το frontend **ProductsPortal** σε μια εφαρμογή Azure web. Πρώτα, αναπτύξτε το έργο **ProductsPortal** , ακολουθώντας όλα τα βήματα στην ενότητα [ανάπτυξη του έργου web για να το Azure web app](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app). Μετά την ολοκλήρωση ανάπτυξης, επιστρέψτε σε αυτό το πρόγραμμα εκμάθησης και προχωρήστε στο επόμενο βήμα.

> [AZURE.NOTE] Ενδέχεται να μπορείτε να δείτε ένα μήνυμα σφάλματος στο παράθυρο του προγράμματος περιήγησης, όταν το project web **ProductsPortal** εκκινείται αυτόματα μετά την ανάπτυξη. Αυτό είναι αναμενόμενο και προκύπτει επειδή η εφαρμογή **ProductsServer** δεν εκτελείται ακόμη.

Αντιγράψτε τη διεύθυνση URL της εφαρμογής web ανεπτυγμένος, χρειάζεστε τη διεύθυνση URL στο επόμενο βήμα. Μπορείτε επίσης να αποκτήσετε αυτήν τη διεύθυνση URL από το παράθυρο Azure εφαρμογής υπηρεσίας δραστηριότητας στο Visual Studio:

![][9] 

### <a name="set-productsportal-as-web-app"></a>Ορισμός ProductsPortal ως web app

Πριν από την εκτέλεση της εφαρμογής στο cloud, πρέπει να βεβαιωθείτε ότι **ProductsPortal** εκκινείται από μέσα σε Visual Studio ως μια εφαρμογή web.

1. Στο Visual Studio, κάντε δεξί κλικ στο έργο **ProjectsPortal** και, στη συνέχεια, κάντε κλικ στην επιλογή **Ιδιότητες**.

3. Στην αριστερή στήλη, κάντε κλικ στην επιλογή **Web**.

5. Στην ενότητα **Έναρξη ενέργειας** , κάντε κλικ στο κουμπί **Έναρξη διεύθυνση URL** και, στο πλαίσιο κειμένου, πληκτρολογήστε τη διεύθυνση URL για την εφαρμογή σας προηγουμένως ανεπτυγμένος web; Για παράδειγμα, `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. Από το μενού " **αρχείο** " στο Visual Studio, κάντε κλικ στην επιλογή **Αποθήκευση όλων**.

7. Από το μενού Δημιουργία στο Visual Studio, κάντε κλικ στην επιλογή **Εκ νέου δημιουργία λύσης**.

## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή

2.  Πατήστε το πλήκτρο F5 για να δημιουργήσετε και να εκτελέσετε την εφαρμογή. Πρέπει να ξεκινά η πρώτη διακομιστή εσωτερικής εγκατάστασης (η εφαρμογή κονσόλας **ProductsServer** ) και, στη συνέχεια, θα πρέπει να ξεκινήσει η εφαρμογή **ProductsPortal** σε ένα παράθυρο προγράμματος περιήγησης, όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης. Ειδοποίηση ξανά ότι το απόθεμα του προϊόντος λίστες δεδομένα που έχουν ανακτηθεί από την υπηρεσία προϊόντος εσωτερικής συστήματος και εμφανίζει ότι τα δεδομένα στο web app. Ελέγξτε τη διεύθυνση URL για να βεβαιωθείτε ότι **ProductsPortal** εκτελείται στο cloud, με Azure web app. 

    ![][1]

    > [AZURE.IMPORTANT] Η εφαρμογή κονσόλας **ProductsServer** πρέπει να εκτελείται και μπορούν να εξυπηρετήσει τα δεδομένα στην εφαρμογή **ProductsPortal** . Εάν το πρόγραμμα περιήγησης εμφανίζει ένα σφάλμα, περιμένετε μερικά δευτερόλεπτα περισσότερες για **ProductsServer** φόρτωση και εμφανίζεται το ακόλουθο μήνυμα. Στη συνέχεια, πατήστε το πλήκτρο **Ανανέωση** στο πρόγραμμα περιήγησης.

    ![][37]

3. Πίσω στο πρόγραμμα περιήγησης, πατήστε " **Ανανέωση** " στη σελίδα **ProductsPortal** . Κάθε φορά που μπορείτε να ανανεώσετε τη σελίδα, θα δείτε την εφαρμογή διακομιστή, εμφανίζεται ένα μήνυμα όταν `GetProducts()` από **ProductsServer** ονομάζεται.

    ![][38]

## <a name="next-steps"></a>Επόμενα βήματα  

Για να μάθετε περισσότερα σχετικά με την υπηρεσία Bus, ανατρέξτε στους ακόλουθους πόρους:  

* [Bus Azure υπηρεσίας][sbwacom]  
* [Πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Λήψη εργαλεία και SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

