<properties 
pageTitle="Επικοινωνίας για τους ρόλους στο Cloud Services | Microsoft Azure" 
description="Παρουσίες ρόλος στο Cloud Services μπορούν να έχουν τα τελικά σημεία (http, https, tcp, udp) καθορίσει για αυτές που επικοινωνία με το εξωτερικό ή μεταξύ άλλων περιόδων ρόλο." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="enable-communication-for-role-instances-in-azure"></a>Ενεργοποίηση επικοινωνίας για παρουσίες ρόλος στο azure

Ρόλοι υπηρεσία cloud επικοινωνία μέσω εσωτερικές και εξωτερικές συνδέσεις. Εξωτερικές συνδέσεις ονομάζονται **τελικά σημεία εισόδου** ενώ εσωτερικές συνδέσεις ονομάζονται **εσωτερικό τελικά σημεία**. Αυτό το θέμα περιγράφει πώς μπορείτε να τροποποιήσετε τον [ορισμό υπηρεσίας](cloud-services-model-and-package.md#csdef) για να δημιουργήσετε τελικά σημεία.


## <a name="input-endpoint"></a>Τελικό σημείο εισαγωγής
Το τελικό σημείο εισαγωγής χρησιμοποιείται όταν θέλετε να εμφανίσετε μια θύρα προς τα έξω. Μπορείτε να καθορίσετε τον τύπο πρωτοκόλλου και τη θύρα του τελικού σημείου που, στη συνέχεια, ισχύει για των εξωτερικών και εσωτερικών θυρών για το τελικό σημείο. Εάν θέλετε, μπορείτε να καθορίσετε μια διαφορετική εσωτερικής θύρα για το τελικό σημείο με το χαρακτηριστικό [Τοπική_θύρα](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) .

Το τελικό σημείο εισαγωγής να χρησιμοποιήσετε τα ακόλουθα πρωτόκολλα: **http, https, tcp και udp**.

Για να δημιουργήσετε ένα τελικό σημείο εισαγωγής, προσθέστε το θυγατρικό στοιχείο **InputEndpoint** στο στοιχείο **τελικά σημεία** του ρόλου μια web ή εργασίας.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Τελικό σημείο εισαγωγής παρουσίας
Τελικά σημεία εισόδου παρουσία είναι παρόμοια με εισαγωγής τελικά σημεία σάς επιτρέπει αλλά αντιστοίχιση συγκεκριμένες θύρες δημόσιας για κάθε παρουσία μεμονωμένα ρόλο χρησιμοποιώντας τη μονάδα εξισορρόπησης φόρτου θύρα προώθησης. Μπορείτε να καθορίσετε μια μεμονωμένη θύρα δημόσιας ή μια περιοχή θυρών.

Το τελικό σημείο εισαγωγής παρουσία να χρησιμοποιήσετε μόνο **tcp** ή **udp** ως το πρωτόκολλο.

Για να δημιουργήσετε ένα τελικό σημείο εισαγωγής παρουσία, προσθέστε το θυγατρικό στοιχείο **InstanceInputEndpoint** στο στοιχείο **τελικά σημεία** του ρόλου μια web ή εργασίας.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Εσωτερική τελικού σημείου
Εσωτερική τελικά σημεία είναι διαθέσιμοι για επικοινωνία με την παρουσία για να την παρουσία. Η θύρα είναι προαιρετική και εάν παραλείπεται, μια δυναμική θύρα έχει αντιστοιχιστεί στο τελικό σημείο. Μια περιοχή θύρας μπορεί να χρησιμοποιηθεί. Υπάρχει όριο πέντε εσωτερικό τελικά σημεία ανά ρόλο.

Το τελικό σημείο εσωτερικό να χρησιμοποιήσετε τα ακόλουθα πρωτόκολλα: **http, tcp και udp, οποιαδήποτε**.

Για να δημιουργήσετε ένα εσωτερικό τελικό σημείο εισαγωγής, προσθέστε το θυγατρικό στοιχείο **InternalEndpoint** στο στοιχείο **τελικά σημεία** του ρόλου μια web ή εργασίας.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Μπορείτε επίσης να χρησιμοποιήσετε μια περιοχή θύρας.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Ρόλοι εργαζόμενου έναντι ρόλους Web

Υπάρχει μια μικρή διαφορά με τα τελικά σημεία όταν εργάζεστε με εργασίας και τους ρόλους web. Ο ρόλος web πρέπει να έχετε τουλάχιστον ένα μεμονωμένο τελικό σημείο εισαγωγής με χρήση του πρωτοκόλλου **HTTP** .


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>Χρήση του SDK .NET πρόσβασης τελικού σημείου
Η βιβλιοθήκη διαχειριζόμενων Azure παρέχει μεθόδους για παρουσίες ρόλο για επικοινωνία κατά το χρόνο εκτέλεσης. Από κώδικα που εκτελείται μέσα σε μια παρουσία ρόλο, μπορείτε να ανακτήσετε πληροφορίες σχετικά με την ύπαρξη άλλες εμφανίσεις ρόλο και τα τελικά σημεία, καθώς και πληροφορίες σχετικά με την τρέχουσα παρουσία ρόλο.

> [AZURE.NOTE] Μπορείτε μόνο να ανακτήσετε πληροφορίες σχετικά με το ρόλο παρουσίες που εκτελούν στην υπηρεσία cloud και που ορίζουν τουλάχιστον μία εσωτερική τελικού σημείου. Δεν μπορείτε να αποκτήσετε δεδομένα σχετικά με το ρόλο παρουσιών που εκτελούνται σε διαφορετική υπηρεσία.

Μπορείτε να χρησιμοποιήσετε την ιδιότητα [παρουσίες](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) για την ανάκτηση των εμφανίσεων ένα ρόλο. Πρώτα να χρησιμοποιήσετε το [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) για να επιστρέψει μια αναφορά στην τρέχουσα παρουσία ρόλο και, στη συνέχεια, χρησιμοποιήστε την ιδιότητα [ρόλο](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) για να λάβετε μια αναφορά με το ρόλο ίδια.

Όταν συνδέεστε με μια παρουσία ρόλο μέσω προγραμματισμού μέσω το .NET SDK, είναι σχετικά εύκολο για να αποκτήσετε πρόσβαση στις πληροφορίες του τελικού σημείου. Για παράδειγμα, αφού έχετε ήδη συνδεθεί σε ένα συγκεκριμένο ρόλο περιβάλλον, μπορείτε να λάβετε τη θύρα από ένα συγκεκριμένο τελικό σημείο με αυτόν τον κώδικα:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

Η ιδιότητα **παρουσίες** επιστρέφει μια συλλογή **RoleInstance** αντικειμένων. Πάντα αυτή η συλλογή περιέχει την τρέχουσα παρουσία. Εάν ο ρόλος δεν έχει ορίσει εσωτερικού ορίου, η συλλογή περιλαμβάνει την τρέχουσα παρουσία, αλλά δεν υπάρχουν άλλες εμφανίσεις. Ο αριθμός των παρουσιών ρόλο στη συλλογή θα είναι πάντα 1 στην περίπτωση όπου δεν υπάρχει εσωτερικό τελικού σημείου ορίζεται για το ρόλο. Εάν ο ρόλος ορίζει εσωτερικό τελικού σημείου, τις παρουσίες εντοπίζονται κατά το χρόνο εκτέλεσης και ο αριθμός των παρουσιών της συλλογής θα αντιστοιχούν στον αριθμό των παρουσιών που έχουν καθοριστεί για το ρόλο στο αρχείο ρύθμισης παραμέτρων της υπηρεσίας.

> [AZURE.NOTE] Στη βιβλιοθήκη διαχειριζόμενων Azure δεν παρέχει ένα μέσο προσδιορισμού την εύρυθμη λειτουργία των παρουσιών άλλο ρόλο, αλλά μπορείτε να υλοποιήσετε οι εκτιμήσεις εύρυθμης λειτουργίας μόνοι σας εάν η υπηρεσία σας χρειάζεται αυτή η λειτουργία. Μπορείτε να χρησιμοποιήσετε [Τα Διαγνωστικά Azure](cloud-services-dotnet-diagnostics.md) για τη λήψη πληροφοριών σχετικά με τις παρουσίες ρόλο.

Για να καθορίσετε τον αριθμό θύρας για εσωτερική τελικού σημείου σε μια παρουσία ρόλο, μπορείτε να χρησιμοποιήσετε την ιδιότητα [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) για να λάβετε ένα αντικείμενο λεξικού που περιέχει τελικού σημείου ονόματα και τις αντίστοιχες διευθύνσεις IP και θύρες. Η ιδιότητα [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) επιστρέφει τη διεύθυνση IP και θύρα για ένα καθορισμένο τελικό σημείο. Η ιδιότητα **PublicIPEndpoint** επιστρέφει τη θύρα για ένα τελικό σημείο εξισορρόπησης φόρτου. Το τμήμα της διεύθυνσης IP της ιδιότητας **PublicIPEndpoint** δεν χρησιμοποιείται.

Ακολουθεί ένα παράδειγμα που επαναλαμβάνεται παρουσίες ρόλο.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Ακολουθεί ένα παράδειγμα ενός ρόλου εργαζόμενου που λαμβάνει το τελικό σημείο που εκτίθεται μέσω τον ορισμό υπηρεσίας και ξεκινά ακρόαση για τις συνδέσεις.

> [AZURE.WARNING] Αυτός ο κώδικας θα λειτουργεί μόνο για μια υπηρεσία ανεπτυγμένος. Όταν εκτελείται στο το Azure τον υπολογισμό προσομοίωσης, λαμβάνονται υπόψη στοιχεία ρύθμισης παραμέτρων υπηρεσίας που δημιουργήσετε τελικά σημεία απευθείας θύρα (στοιχεία**InstanceInputEndpoint** ).

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Κανόνες κίνηση του δικτύου για να ελέγξετε το ρόλο επικοινωνίας
Αφού προσδιορίσετε εσωτερικό τελικά σημεία, μπορείτε να προσθέσετε κανόνες κίνηση του δικτύου (με βάση τα τελικά σημεία που δημιουργήσατε) για να ελέγχετε τον τρόπο παρουσίες ρόλο μπορούν να επικοινωνούν μεταξύ τους. Το παρακάτω διάγραμμα παρουσιάζει ορισμένα κοινά σενάρια για τον έλεγχο της επικοινωνίας ρόλου:

![Σενάρια κανόνες κίνηση του δικτύου] (./media/cloud-services-enable-communication-role-instances/scenarios.png "Σενάρια κανόνες κίνηση του δικτύου")

Το παρακάτω παράδειγμα κώδικα εμφανίζει ορισμούς ρόλων για τους ρόλους στο προηγούμενο διαγράμματος. Κάθε ορισμό ρόλου περιλαμβάνει τουλάχιστον μία εσωτερική τελικού σημείου που ορίζονται από:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] Ο περιορισμός επικοινωνίας μεταξύ των ρόλων μπορεί να προκύψει με εσωτερική τελικά σημεία και των δύο σταθερών και εκχωρούνται αυτόματα θύρες.

Από προεπιλογή, μετά την εσωτερική τελικού σημείου ορίζεται, επικοινωνίας μπορεί να ρέει από οποιονδήποτε ρόλο στο εσωτερικό τελικό σημείο του ρόλου χωρίς περιορισμούς. Για να περιορίσετε επικοινωνίας, πρέπει να προσθέσετε ένα στοιχείο **NetworkTrafficRules** για το στοιχείο **ServiceDefinition** στο αρχείο ορισμού της υπηρεσίας.

### <a name="scenario-1"></a>Σενάριο 1
Να επιτρέπονται μόνο κίνηση του δικτύου από **WebRole1** **WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Σενάριο 2
Κίνηση του δικτύου από **WebRole1** επιτρέπει μόνο να **WorkerRole1** και **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Σενάριο 3
Κίνηση του δικτύου από **WebRole1** επιτρέπει μόνο να **WorkerRole1**και **WorkerRole1** σε **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Σενάριο 4
Κίνηση του δικτύου από **WebRole1** επιτρέπει μόνο να **WorkerRole1**, **WebRole1** για **WorkerRole2**και **WorkerRole1** σε **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Μπορείτε να βρείτε μια αναφορά σχήματος XML για τα στοιχεία που χρησιμοποιούνται πάνω από [εδώ](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Επόμενα βήματα
Διαβάστε περισσότερα σχετικά με την υπηρεσία Cloud [μοντέλο](cloud-services-model-and-package.md).