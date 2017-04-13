<properties
   pageTitle="Καθορισμός τελικά σημεία υπηρεσίας ύφασμα υπηρεσίας | Microsoft Azure"
   description="Πώς μπορείτε να περιγράψετε ορίου πόρων σε μια υπηρεσία δηλωτικό, συμπεριλαμβανομένων των πώς μπορείτε να ρυθμίσετε τα τελικά σημεία HTTPS"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="specify-resources-in-a-service-manifest"></a>Καθορίστε πόρων σε μια υπηρεσία δηλωτικό

## <a name="overview"></a>Επισκόπηση

Το δηλωτικό υπηρεσία επιτρέπει στους πόρους που χρησιμοποιούνται από την υπηρεσία για να έχουν δηλωθεί/αλλάξει χωρίς να αλλάξετε τον μεταγλωττισμένο κώδικα. Azure ύφασμα υπηρεσία υποστηρίζει τη ρύθμιση παραμέτρων ορίου πόρων για την υπηρεσία. Η πρόσβαση στους πόρους που έχουν καθοριστεί στο δηλωτικό υπηρεσία μπορεί να ελέγχεται μέσω του SecurityGroup στο δηλωτικό εφαρμογής. Η δήλωση πόρων επιτρέπει σε αυτούς τους πόρους που θα αλλάξει κατά το χρόνο ανάπτυξης, γεγονός που σημαίνει ότι η υπηρεσία δεν χρειάζεται να εισάγουν νέα μηχανισμό ρύθμισης παραμέτρων. Ο ορισμός σχήματος για το αρχείο ServiceManifest.xml έχει εγκατασταθεί με την υπηρεσία ύφασμα SDK και εργαλεία για να *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

## <a name="endpoints"></a>Τα τελικά σημεία

Όταν ένας πόρος τελικού σημείου ορίζεται στο δηλωτικό υπηρεσίας, υπηρεσία ύφασμα αντιστοιχίζει θύρες από την περιοχή θύρα δεσμευμένη εφαρμογής, όταν δεν έχει καθοριστεί ρητά θύρα. Για παράδειγμα, εξετάστε το τελικό σημείο *ServiceEndpoint1* που καθορίζεται στο τμήμα δήλωσης που παρέχονται μετά από αυτήν την παράγραφο. Επιπλέον, υπηρεσίες επίσης να ζητήσετε μια συγκεκριμένη θύρα σε έναν πόρο. Υπηρεσία αντίγραφα εκτελείται σε διαφορετικό σύμπλεγμα κόμβους μπορούν να αντιστοιχιστούν διαφορετικούς αριθμούς θύρας, ενώ αντίγραφα των μια υπηρεσία που εκτελείται στον ίδιο κόμβο κοινή χρήση της θύρας. Η υπηρεσία αντίγραφα, στη συνέχεια, να χρησιμοποιήσετε αυτές τις θύρες ανάλογα με τις ανάγκες για την αναπαραγωγή και ακρόαση για αιτήσεις προγράμματος-πελάτη.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Ανατρέξτε στην [Ρύθμιση παραμέτρων βάσει της κατάστασης αξιόπιστων υπηρεσιών](service-fabric-reliable-services-configuration.md) για να διαβάσετε περισσότερα σχετικά με την αναφορά σε τελικά σημεία από τις ρυθμίσεις πακέτου config αρχείων (settings.xml).

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Παράδειγμα: Καθορισμός ενός ορίου HTTP για την υπηρεσία σας

Το παρακάτω δηλωτικό υπηρεσία ορίζει έναν πόρο τελικού σημείου TCP και δύο πόρους τελικού σημείου HTTP στο το &lt;πόρους&gt; στοιχείο.

Τα τελικά σημεία HTTP είναι αυτόματα ACL θα από ύφασμα υπηρεσίας.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Παράδειγμα: Καθορισμός HTTPS τελικού σημείου της υπηρεσίας

Το πρωτόκολλο HTTPS παρέχει έλεγχο ταυτότητας διακομιστή και χρησιμοποιείται επίσης για την κρυπτογράφηση επικοινωνίας υπολογιστή-πελάτη διακομιστή. Για να ενεργοποιήσετε HTTPS την υπηρεσία ύφασμα υπηρεσίας, καθορίστε το πρωτόκολλο στο το *πόροι -> τελικά σημεία -> τελικού σημείου* ενότητα το δηλωτικό υπηρεσία, όπως φαίνεται παραπάνω για το τελικό σημείο *ServiceEndpoint3*.

>[AZURE.NOTE] Μια υπηρεσία πρωτοκόλλου δεν μπορεί να αλλάξει κατά την αναβάθμιση εφαρμογής χωρίς που αποτελούν ένα πρόσφατες αλλάζει.


Ακολουθεί ένα παράδειγμα ApplicationManifest που χρειάζεστε για να ορίσετε για HTTPS. Πρέπει να δοθεί την αποτύπωση για το πιστοποιητικό σας. Η EndpointRef είναι μια αναφορά σε EndpointResource στο ServiceManifest, για την οποία μπορείτε να ορίσετε το πρωτόκολλο HTTPS. Μπορείτε να προσθέσετε περισσότερες από μία EndpointCertificate.  

```
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```
