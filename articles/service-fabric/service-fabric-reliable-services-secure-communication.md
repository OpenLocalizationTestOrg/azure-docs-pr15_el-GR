<properties
   pageTitle="Ασφαλής επικοινωνία για τις υπηρεσίες στο ύφασμα υπηρεσίας Βοήθειας | Microsoft Azure"
   description="Επισκόπηση του τρόπου για την ασφαλή επικοινωνία για αξιόπιστη τις υπηρεσίες που λειτουργούν σε ένα σύμπλεγμα ύφασμα υπηρεσίας Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="suchiagicha"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/06/2016"
   ms.author="suchiagicha"/>

# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Βοήθεια για ασφαλή επικοινωνία για τις υπηρεσίες στο ύφασμα υπηρεσίας Azure

Η ασφάλεια είναι μία από τις πιο σημαντικές πτυχές της επικοινωνίας. Το πλαίσιο εφαρμογή υπηρεσιών αξιόπιστη παρέχει ορισμένα στοίβες προ-δομημένες επικοινωνίας και εργαλεία που μπορείτε να χρησιμοποιήσετε για τη βελτίωση της ασφάλειας. Σε αυτό το άρθρο θα μιλήσετε σχετικά με το πώς μπορείτε να βελτιώσετε την ασφάλεια όταν χρησιμοποιείτε το Windows Communication Foundation (WCF) στοίβας επικοινωνίας και υπηρεσίας απομακρυσμένης πρόσβασης.

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Ασφάλιση μιας υπηρεσίας όταν χρησιμοποιείτε υπηρεσίας απομακρυσμένης πρόσβασης

Θα χρησιμοποιούμε ένα υπάρχον [παράδειγμα](service-fabric-reliable-services-communication-remoting.md) που εξηγεί τον τρόπο ρύθμισης απομακρυσμένης πρόσβασης για τις υπηρεσίες αξιόπιστη. Για να ασφαλίσετε μια υπηρεσία όταν χρησιμοποιείτε υπηρεσίας απομακρυσμένης πρόσβασης, ακολουθήστε τα παρακάτω βήματα:

1. Δημιουργήστε ένα περιβάλλον εργασίας, `IHelloWorldStateful`, που ορίζει τις μεθόδους που θα είναι διαθέσιμος για μια κλήση απομακρυσμένης διαδικασίας την υπηρεσία. Θα χρησιμοποιήσει την υπηρεσία `FabricTransportServiceRemotingListener`, που έχει δηλωθεί στο το `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` χώρο ονομάτων. Αυτή είναι μια `ICommunicationListener` υλοποίηση, η οποία παρέχει δυνατότητες απομακρυσμένης πρόσβασης.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```

2. Προσθήκη ρυθμίσεις ακρόασης και διαπιστευτηρίων ασφαλείας.

    Βεβαιωθείτε ότι έχει εγκατασταθεί το πιστοποιητικό που θέλετε να χρησιμοποιήσετε για να ασφαλίσετε επικοινωνία σας υπηρεσία σε όλους τους κόμβους του συμπλέγματος. Υπάρχουν δύο τρόποι που μπορείτε να παρέχετε ρυθμίσεις ακρόασης και διαπιστευτηρίων ασφαλείας:

    1. Δώστε τους την απευθείας στον κώδικα υπηρεσίας:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            FabricTransportListenerSettings listenerSettings = new FabricTransportListenerSettings
            {
                MaxMessageSize = 10000000,
                SecurityCredentials = GetSecurityCredentials()
            };
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
            };
        }

        private static SecurityCredentials GetSecurityCredentials()
        {
            // Provide certificate details.
            var x509Credentials = new X509Credentials
            {
                FindType = X509FindType.FindByThumbprint,
                FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
                StoreLocation = StoreLocation.LocalMachine,
                StoreName = "My",
                ProtectionLevel = ProtectionLevel.EncryptAndSign
            };
            x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
            return x509Credentials;
        }
        ```
    2. Δώστε τους, χρησιμοποιώντας ένα [πακέτο ρύθμισης παραμέτρων](service-fabric-application-model.md):

        Προσθήκη μιας `TransportSettings` ενότητα στο αρχείο settings.xml.

        ```xml
        <!--Section name should always end with "TransportSettings".-->
        <!--Here we are using a prefix "HelloWorldStateful".-->
        <Section Name="HelloWorldStatefulTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509" />
            <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
            <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
            <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
            <Parameter Name="CertificateStoreName" Value="My" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
            <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
        </Section>
        ```

        Σε αυτήν την περίπτωση, η `CreateServiceReplicaListeners` μέθοδο θα μοιάζει κάπως έτσι:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(
                        context,this,FabricTransportListenerSettings.LoadFrom("HelloWorldStateful")))
            };
        }
        ```

         Εάν προσθέσετε μια `TransportSettings` ενότητα στο αρχείο settings.xml χωρίς οποιαδήποτε πρόθεμα, `FabricTransportListenerSettings` θα φορτώσετε όλες τις ρυθμίσεις από αυτήν την ενότητα από προεπιλογή.

         ```xml
         <!--"TransportSettings" section without any prefix.-->
         <Section Name="TransportSettings">
             ...
         </Section>
         ```
         Σε αυτήν την περίπτωση, η `CreateServiceReplicaListeners` μέθοδο θα μοιάζει κάπως έτσι:

         ```csharp
         protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
         {
             return new[]
             {
                 return new[]{
                         new ServiceReplicaListener(
                             (context) => new FabricTransportServiceRemotingListener(context,this))};
             };
         }
         ```

3. Κατά την κλήση μεθόδων σε μια υπηρεσία ασφαλούς με τη χρήση της απομακρυσμένης πρόσβασης στοίβας, αντί να χρησιμοποιήσετε το `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` κλάση για να δημιουργήσετε ένα διακομιστή μεσολάβησης υπηρεσίας, χρησιμοποιήστε `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Μεταβίβαση στην `FabricTransportSettings`, που περιέχει `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");

    FabricTransportSettings transportSettings = new FabricTransportSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Εάν ο κωδικός προγράμματος-πελάτη εκτελείται ως μέρος μιας υπηρεσίας, μπορείτε να φορτώσετε `FabricTransportSettings` από το αρχείο settings.xml. Δημιουργήστε μια ενότητα TransportSettings που είναι παρόμοια με την υπηρεσία κώδικα, όπως φαίνεται παραπάνω. Κάνετε τις ακόλουθες αλλαγές στον κώδικα του προγράμματος-πελάτη:

    ```csharp

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportSettings.LoadFrom("TransportSettingsPrefix")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Εάν ο υπολογιστής-πελάτης δεν εκτελείται ως μέρος μιας υπηρεσίας, μπορείτε να δημιουργήσετε ένα αρχείο client_name.settings.xml στην ίδια θέση όπου βρίσκεται το client_name.exe. Στη συνέχεια, δημιουργήστε μια ενότητα TransportSettings σε αυτό το αρχείο.

    Παρόμοια με την υπηρεσία, εάν προσθέσετε μια `TransportSettings` ενότητας χωρίς οποιαδήποτε πρόθεμα στο πρόγραμμα-πελάτη settings.xml/client_name.settings.xml, `FabricTransportSettings` θα φορτώσετε όλες τις ρυθμίσεις από αυτήν την ενότητα από προεπιλογή.

    Σε αυτή την περίπτωση, την προηγούμενη κώδικα ακόμη περισσότερο απλοποιημένα:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a>Ασφάλιση μιας υπηρεσίας όταν χρησιμοποιείτε μια στοίβα βάσει WCF επικοινωνίας

Θα χρησιμοποιούμε ένα υπάρχον [παράδειγμα](service-fabric-reliable-services-communication-wcf.md) που εξηγεί πώς μπορείτε να ρυθμίσετε μια στοίβα βάσει WCF επικοινωνίας για τις υπηρεσίες αξιόπιστη. Για να ασφαλίσετε μια υπηρεσία όταν χρησιμοποιείτε μια στοίβα επικοινωνία με βάση το WCF, ακολουθήστε τα παρακάτω βήματα:

1. Για την υπηρεσία, πρέπει να ασφαλίσετε την παρακολούθηση της επικοινωνίας WCF (`WcfCommunicationListener`) που δημιουργείτε. Για να το κάνετε αυτό, τροποποιήστε το `CreateServiceReplicaListeners` μέθοδο.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```

2. Στο πρόγραμμα-πελάτη, την `WcfCommunicationClient` κλάσης που δημιουργήθηκε στο προηγούμενο [παράδειγμα](service-fabric-reliable-services-communication-wcf.md) παραμένει αμετάβλητη. Αλλά πρέπει να παρακάμψετε τη `CreateClientAsync` μέθοδο `WcfCommunicationClientFactory`:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Χρήση `SecureWcfCommunicationClientFactory` για να δημιουργήσετε ένα πρόγραμμα-πελάτη επικοινωνίας WCF (`WcfCommunicationClient`). Χρησιμοποιήστε το πρόγραμμα-πελάτη για να καλέσετε μεθόδους της υπηρεσίας.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a>Επόμενα βήματα

* [API με OWIN στις αξιόπιστες υπηρεσίες Web](service-fabric-reliable-services-communication-webapi.md)
