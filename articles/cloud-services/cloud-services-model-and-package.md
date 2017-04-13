<properties
    pageTitle="Τι είναι μια υπηρεσία Cloud μοντέλο και πακέτο | Microsoft Azure"
    description="Περιγράφει το μοντέλο υπηρεσία cloud (.csdef, .cscfg) και το πακέτο (.cspkg) στο Azure"
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

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Τι είναι το μοντέλο υπηρεσία στο Cloud και πώς να συσκευάσετε;
Μια υπηρεσία cloud δημιουργείται από τρία στοιχεία, η υπηρεσία definition _(.csdef)_, η υπηρεσία ρύθμισης παραμέτρων _(.cscfg)_και ένα πακέτο υπηρεσίας _(.cspkg)_. Τόσο η **ServiceDefinition.csdef** και **ServiceConfig.cscfg** αρχεία βασίζονται σε XML και περιγράφουν τη δομή της υπηρεσίας cloud και πώς έχει ρυθμιστεί, αποτελούν συνολικά ονομάζεται μοντέλο. Το **ServicePackage.cspkg** είναι ένα αρχείο zip που έχει δημιουργηθεί από το **ServiceDefinition.csdef** και μεταξύ άλλων, περιέχει όλες τις απαιτούμενες εξαρτήσεις βασίζεται σε δυαδικά. Azure δημιουργεί μια υπηρεσία cloud από το **ServicePackage.cspkg** και το **ServiceConfig.cscfg**.

Αφού στο Azure εκτελείται η υπηρεσία cloud, μπορείτε να ρυθμίσετε ξανά μέσω του αρχείου **ServiceConfig.cscfg** , αλλά δεν μπορείτε να τροποποιήσετε τον ορισμό.

## <a name="what-would-you-like-to-know-more-about"></a>Τι θα θέλατε να μάθω περισσότερα σχετικά με το;

* Θέλω να μάθω περισσότερα σχετικά με τα αρχεία [ServiceDefinition.csdef](#csdef) και [ServiceConfig.cscfg](#cscfg) .
* Να γνωρίζετε ήδη σχετικά με αυτό, να λαμβάνω [ορισμένα παραδείγματα](#next-steps) σχετικά με τι μπορώ να ρυθμίσετε τις παραμέτρους.
* Θέλω να δημιουργήσω το [ServicePackage.cspkg](#cspkg).
* Χρησιμοποιώ το Visual Studio και θέλω να...
    * [Δημιουργήστε μια νέα υπηρεσία στο cloud][vs_create]
    * [Ρυθμίστε ξανά τις παραμέτρους μια υπάρχουσα υπηρεσία cloud][vs_reconfigure]
    * [Ανάπτυξη ενός έργου υπηρεσία στο Cloud][vs_deploy]
    * [Σύνδεση απομακρυσμένης επιφάνειας εργασίας σε μια παρουσία της υπηρεσίας cloud][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
Το αρχείο **ServiceDefinition.csdef** καθορίζει τις ρυθμίσεις που χρησιμοποιούνται από Azure για να ρυθμίσετε μια υπηρεσία στο cloud. Το [Σχήμα ορισμού υπηρεσίας Azure (.csdef αρχείο)](https://msdn.microsoft.com/library/azure/ee758711.aspx) παρέχει το επιτρεπόμενο μορφοποίηση για ένα αρχείο ορισμού υπηρεσίας. Το παρακάτω παράδειγμα εμφανίζει τις ρυθμίσεις που μπορεί να οριστεί για το Web και εργαζόμενου ρόλους:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
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
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Μπορείτε να ανατρέξετε στην το [] [σχήματος ορισμού υπηρεσίας] για καλύτερη κατανόηση της το σχήμα XML που χρησιμοποιείται εδώ, ωστόσο, δείτε μια γρήγορη εξήγηση των ορισμένα από τα στοιχεία:

**Τοποθεσίες**  
Περιέχει τους ορισμούς για τοποθεσίες Web ή web εφαρμογές που φιλοξενούνται σε IIS7.

**InputEndpoints**  
Περιέχει τους ορισμούς για τα τελικά σημεία που χρησιμοποιούνται για την επικοινωνία με την υπηρεσία cloud.

**InternalEndpoints**  
Περιέχει τους ορισμούς για τα τελικά σημεία που χρησιμοποιούνται από το ρόλο παρουσίες για να επικοινωνούν μεταξύ τους.

**ConfigurationSettings**  
Περιέχει τους ορισμούς τη ρύθμιση για τις δυνατότητες των ένα συγκεκριμένο ρόλο.

**Πιστοποιητικά**  
Περιέχει τους ορισμούς για τα πιστοποιητικά που απαιτούνται για ένα ρόλο. Το προηγούμενο παράδειγμα κώδικα δείχνει ένα πιστοποιητικό που χρησιμοποιείται για τη ρύθμιση των παραμέτρων του Azure σύνδεση.

**LocalResources**  
Περιέχει τους ορισμούς για τους πόρους του τοπικού χώρου αποθήκευσης. Ένας πόρος τοπικού χώρου αποθήκευσης είναι ένας κατάλογος δεσμευμένο στο σύστημα αρχείων του εικονικές υπολογιστή στον οποίο εκτελείται μια παρουσία του ρόλου.

**Εισαγωγές**  
Περιέχει τους ορισμούς για λειτουργικές μονάδες που έχουν εισαχθεί. Το προηγούμενο παράδειγμα κώδικα δείχνει τις ενότητες για σύνδεση απομακρυσμένης επιφάνειας εργασίας και να συνδεθείτε Azure.

**Εκκίνησης**  
Περιέχει τις εργασίες που εκτελούνται όταν ξεκινά το ρόλο. Οι εργασίες έχουν οριστεί σε ένα εκτελέσιμο αρχείο ή. cmd..



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
Η ρύθμιση παραμέτρων για τις ρυθμίσεις για την υπηρεσία cloud προσδιορίζεται από τις τιμές στο αρχείο **ServiceConfiguration.cscfg** . Μπορείτε να καθορίσετε τον αριθμό των παρουσιών που θέλετε να αναπτύξετε για κάθε ρόλο σε αυτό το αρχείο. Οι τιμές για τις ρυθμίσεις παραμέτρων που έχετε ορίσει στο αρχείο ορισμού υπηρεσίας προστίθενται στο αρχείο παραμέτρων υπηρεσίας. Το thumbprints για όλα τα πιστοποιητικά διαχείρισης που σχετίζονται με την υπηρεσία cloud προστίθενται στο αρχείο. Το [Σχήμα της ομάδας παραμέτρων υπηρεσιών Azure (.cscfg αρχείο)](https://msdn.microsoft.com/library/azure/ee758710.aspx) παρέχει το επιτρεπόμενο μορφοποίηση για ένα αρχείο ρύθμισης παραμέτρων υπηρεσίας.

Το αρχείο ρύθμισης παραμέτρων υπηρεσίας δεν περιλαμβάνεται στη συσκευασία με την εφαρμογή, αλλά αποστέλλεται στο Azure ως ξεχωριστό αρχείο και χρησιμοποιείται για τη ρύθμιση παραμέτρων της υπηρεσίας cloud. Μπορείτε να αποστείλετε ένα νέο αρχείο ρύθμισης παραμέτρων υπηρεσίας χωρίς επανάληψη ανάπτυξης την υπηρεσία cloud. Οι τιμές παραμέτρων για την υπηρεσία cloud μπορεί να αλλάξει ενώ εκτελείται η υπηρεσία cloud. Το παρακάτω παράδειγμα εμφανίζει τις ρυθμίσεις παραμέτρων που μπορεί να οριστεί για το Web και εργαζόμενου ρόλους:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Μπορείτε να ανατρέξετε στην του [Σχήματος ρύθμισης παραμέτρων υπηρεσίας](https://msdn.microsoft.com/library/azure/ee758710.aspx) για να κατανοήσετε καλύτερα το σχήμα XML που χρησιμοποιείται εδώ, ωστόσο, ακολουθεί μια γρήγορη εξήγηση των στοιχείων:

**Παρουσίες**  
Ρυθμίζει τον αριθμό των παρουσίες του ρόλου. Για να αποτρέψετε την υπηρεσία cloud ενδεχομένως μη διαθέσιμες κατά τη διάρκεια αναβαθμίσεις, είναι συνιστάται να αναπτύξετε περισσότερες από μία παρουσία του ρόλους σας τοποθεσία web. Με αυτήν την ενέργεια, που τηρεί τις οδηγίες στο το [Azure τον υπολογισμό υπηρεσίας σύμβαση (SLA)](http://azure.microsoft.com/support/legal/sla/), που εγγυάται 99.95% εξωτερικών συνδεσιμότητας για ρόλους μέσω Internet, όταν δύο ή περισσότερες παρουσίες ρόλο αναπτύσσονται για μια υπηρεσία.

**ConfigurationSettings**  
Ρυθμίζει τις παραμέτρους των ρυθμίσεων για τις παρουσίες που εκτελούνται για ένα ρόλο. Το όνομα του `<Setting>` στοιχεία πρέπει να συμφωνεί με τη ρύθμιση ορισμών στο αρχείο ορισμού της υπηρεσίας.

**Πιστοποιητικά**  
Ρυθμίζει τα πιστοποιητικά που χρησιμοποιούνται από την υπηρεσία. Το προηγούμενο παράδειγμα κώδικα δείχνει πώς μπορείτε να ορίσετε το πιστοποιητικό για τη λειτουργική μονάδα απομακρυσμένης πρόσβασης. Η τιμή του χαρακτηριστικού *αποτύπωση* πρέπει να οριστούν για την αποτύπωση του πιστοποιητικού για να χρησιμοποιήσετε.

<p/>

 >[AZURE.NOTE] Την αποτύπωση για το πιστοποιητικό μπορούν να προστεθούν στο αρχείο ρύθμισης παραμέτρων, χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου ή την τιμή μπορούν να προστεθούν στην καρτέλα **πιστοποιητικά** από τη σελίδα **ιδιοτήτων** του ρόλου στο Visual Studio.



## <a name="defining-ports-for-role-instances"></a>Ορισμός θύρες για παρουσίες ρόλων
Azure επιτρέπει μόνο μία καταχώρηση σημείο σε ένα ρόλο web. Αυτό σημαίνει ότι γίνεται όλη την κυκλοφορία μέσω μία διεύθυνση IP. Μπορείτε να ρυθμίσετε τις τοποθεσίες Web για να μοιραστείτε μια θύρα ρυθμίζοντας την κεφαλίδα κεντρικού υπολογιστή για να κατευθύνουν την αίτηση στη σωστή θέση. Μπορείτε επίσης να ρυθμίσετε τις εφαρμογές σας για να ακούσετε γνωστές θύρες στη διεύθυνση IP.

Το παρακάτω παράδειγμα εμφανίζει τη ρύθμιση παραμέτρων για ένα ρόλο web με μια εφαρμογή web και την τοποθεσία Web. Στην τοποθεσία Web έχει ρυθμιστεί ως την προεπιλεγμένη θέση καταχώρησης στη θύρα 80 και τις εφαρμογές web έχουν ρυθμιστεί ώστε να λαμβάνει αιτήσεις από μια κεφαλίδα εναλλασσόμενες κεντρικού υπολογιστή που ονομάζεται "mail.mysite.cloudapp.net".

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Αλλαγή της διαμόρφωσης του ρόλου
Μπορείτε να ενημερώσετε τη ρύθμιση παραμέτρων για την υπηρεσία cloud ενώ εκτελείται στο Azure, χωρίς να χρειάζεται η υπηρεσία χωρίς σύνδεση. Για να αλλάξετε τις πληροφορίες ρύθμισης παραμέτρων, μπορείτε να αποστείλετε ένα νέο αρχείο ρύθμισης παραμέτρων, ή να επεξεργαστείτε το αρχείο ρύθμισης παραμέτρων στη θέση και να εφαρμόσετε σε υπηρεσία εκτελείται. Οι παρακάτω αλλαγές μπορούν να γίνουν με τη ρύθμιση παραμέτρων της υπηρεσίας:

- **Αλλαγή των τιμών των ρυθμίσεων παραμέτρων**  
Όταν μια ρύθμιση παραμέτρων αλλαγές στις ρυθμίσεις, μια παρουσία ρόλο να επιλέξετε να εφαρμόσετε την αλλαγή, ενώ η παρουσία είναι σε σύνδεση ή να Ανακύκλωσης ομαλά την παρουσία και να εφαρμόσετε την αλλαγή κατά την παρουσία είναι εκτός σύνδεσης.

- **Αλλαγή της τοπολογίας υπηρεσίας παρουσίες ρόλων**  
Οι αλλαγές τοπολογίας δεν επηρεάζουν τις εμφανίσεις που εκτελούνται, εκτός από το σημείο όπου καταργείται μια παρουσία. Όλες οι υπόλοιπες παρουσίες γενικά δεν πρέπει να τοποθέτησή; Ωστόσο, μπορείτε να επιλέξετε να Κάδος Ανακύκλωσης παρουσίες ρόλο απόκριση σε μια αλλαγή τοπολογίας.

- **Αλλάζοντας την αποτύπωση πιστοποιητικού**  
Μπορείτε να ενημερώσετε ένα πιστοποιητικό μόνο όταν μια παρουσία ρόλο είναι χωρίς σύνδεση. Εάν ένα πιστοποιητικό προστίθεται, διαγραφή, ή αλλάξει, ενώ μια παρουσία ρόλο είναι σε σύνδεση, Azure ομαλά μεταφέρει την παρουσία για εργασία χωρίς σύνδεση για να ενημερώσετε το πιστοποιητικό και να εμφανίσετε πάλι σε σύνδεση, αφού ολοκληρωθεί η αλλαγή.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Χειρισμός αλλαγές ρύθμισης παραμέτρων με τα συμβάντα χρόνου εκτέλεσης υπηρεσίας
Η [Βιβλιοθήκη χρόνου εκτέλεσης Azure](https://msdn.microsoft.com/library/azure/mt419365.aspx) περιλαμβάνει το χώρο ονομάτων [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) , η οποία παρέχει κλάσεις για αλληλεπίδραση με το Azure περιβάλλον από κώδικα που εκτελείται σε μια περίοδο λειτουργίας του ρόλου. Η κλάση [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) ορίζει τα παρακάτω συμβάντα που προκύπτουν πριν και μετά από μια αλλαγή ρύθμισης παραμέτρων:

- **[Αλλαγή](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) συμβάντος**  
Αυτό συμβαίνει πριν από την αλλαγή της ρύθμισης παραμέτρων έχει εφαρμοστεί σε μια καθορισμένη περίοδο λειτουργίας του ρόλου δίνει τη δυνατότητα να κρατήσετε πατημένο τις παρουσίες ρόλο εάν απαιτείται.
- **[Αλλαγμένες](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) συμβάντος**  
Συμβαίνει μετά την αλλαγή της ρύθμισης παραμέτρων έχει εφαρμοστεί σε μια καθορισμένη περίοδο λειτουργίας του ρόλου.

> [AZURE.NOTE] Επειδή το πιστοποιητικό αλλαγές έχουν πάντα τις παρουσίες του ρόλου χωρίς σύνδεση, αυτές δεν ενεργοποιούν τα συμβάντα RoleEnvironment.Changing ή RoleEnvironment.Changed.

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Για να αναπτύξετε μια εφαρμογή ως μια υπηρεσία στο cloud στο Azure, πρέπει να πρώτα να συσκευάσετε την εφαρμογή στην κατάλληλη μορφή. Μπορείτε να χρησιμοποιήσετε το εργαλείο γραμμής εντολών **CSPack** (έχει εγκατασταθεί με το [Azure SDK](https://azure.microsoft.com/downloads/)) για να δημιουργήσετε το αρχείο πακέτου ως εναλλακτική λύση για τη Visual Studio.

**CSPack** χρησιμοποιεί τα περιεχόμενα του αρχείου ορισμού υπηρεσίας και το αρχείο ρύθμισης παραμέτρων υπηρεσίας για να καθορίσετε τα περιεχόμενα του πακέτου. **CSPack** δημιουργεί ένα αρχείο πακέτου εφαρμογής (.cspkg) που μπορείτε να στείλετε στο Azure χρησιμοποιώντας την [πύλη του Azure](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Από προεπιλογή, το πακέτο ονομάζεται `[ServiceDefinitionFileName].cspkg`, αλλά μπορείτε να καθορίσετε ένα διαφορετικό όνομα, χρησιμοποιώντας το `/out` επιλογή **CSPack**.

**CSPack** βρίσκεται συνήθως στη  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
CSPack.exe (στα windows) είναι διαθέσιμη, εκτελώντας τη συντόμευση **Microsoft Azure εντολών** που έχει εγκατασταθεί με το SDK.  
>  
>Εκτελέστε το πρόγραμμα CSPack.exe μόνη για να δείτε τεκμηρίωσης σχετικά με όλες τις πιθανές διακόπτες και τις εντολές.

<p />

>[AZURE.TIP]
Εκτελέστε την υπηρεσία cloud τοπικά στο του **Microsoft Azure τον υπολογισμό προσομοίωσης**, χρησιμοποιήστε την επιλογή **/copyonly** αυτήν την επιλογή αντιγράφει την δυαδικών αρχείων για την εφαρμογή σε μια διάταξη καταλόγου από την οποία μπορούν να εκτελεστούν σε προσομοίωσης του υπολογισμού.

### <a name="example-command-to-package-a-cloud-service"></a>Παράδειγμα εντολής για να συσκευάσετε μια υπηρεσία στο cloud
Το παρακάτω παράδειγμα δημιουργεί ένα πακέτο εφαρμογών που περιέχει τις πληροφορίες για ένα ρόλο web. Η εντολή Καθορίζει το αρχείο ορισμού υπηρεσίας για να χρησιμοποιήσετε, τον κατάλογο όπου μπορεί να βρεθεί δυαδικών αρχείων, και το όνομα του αρχείου πακέτου.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

Εάν η εφαρμογή περιέχει ένα ρόλο web και ένα ρόλο εργαζόμενου, χρησιμοποιείται την ακόλουθη εντολή:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Όπου τις μεταβλητές ορίζονται ως εξής:

| Μεταβλητή                  | Τιμή |
| ------------------------- | ----- |
| \[Όνομα_καταλόγου\]         | Στον δευτερεύοντα κατάλογο κάτω από το ριζικό κατάλογο έργου που περιέχει το αρχείο .csdef του Azure έργου.|
| \[ServiceDefinition\]     | Το όνομα του αρχείου ορισμού υπηρεσίας. Από προεπιλογή, αυτό το αρχείο ονομάζεται ServiceDefinition.csdef.  |
| \[Όνομα_αρχείου_εξόδου\]        | Το όνομα για το αρχείο που δημιουργήθηκε το πακέτο. Συνήθως, αυτό έχει οριστεί στο όνομα της εφαρμογής. Εάν το όνομα του αρχείου δεν έχει καθοριστεί, το πακέτο εφαρμογών δημιουργείται ως \[ApplicationName\].cspkg.|
| \[Όνομα ρόλου\]              | Το όνομα του ρόλου όπως ορίζεται στο αρχείο ορισμού της υπηρεσίας.|
| \[RoleBinariesDirectory] | Η θέση των δυαδικών αρχείων για το ρόλο.|
| \[VirtualPath\]           | Η φυσική σε καταλόγους για κάθε εικονικού διαδρομής που έχει οριστεί στην ενότητα τοποθεσίες του ορισμού της υπηρεσίας.|
| \[PhysicalPath\]          | Η φυσική κατάλογοι των περιεχομένων για κάθε εικονική διαδρομή που ορίζονται από το στον κόμβο της τοποθεσίας του ορισμού της υπηρεσίας.|
| \[RoleAssemblyName\]      | Το όνομα του δυαδικού αρχείου για το ρόλο.| 


## <a name="next-steps"></a>Επόμενα βήματα

Να τη δημιουργία ενός πακέτου υπηρεσία cloud και θέλω να...

* [Ρύθμιση απομακρυσμένης επιφάνειας εργασίας για μια παρουσία της υπηρεσίας cloud][remotedesktop]
* [Ανάπτυξη ενός έργου υπηρεσία στο Cloud][deploy]

Χρησιμοποιώ το Visual Studio και θέλω να...

* [Δημιουργήστε μια νέα υπηρεσία στο cloud][vs_create]
* [Ρυθμίστε ξανά τις παραμέτρους μια υπάρχουσα υπηρεσία cloud][vs_reconfigure]
* [Ανάπτυξη ενός έργου υπηρεσία στο Cloud][vs_deploy]
* [Ρύθμιση απομακρυσμένης επιφάνειας εργασίας για μια παρουσία της υπηρεσίας cloud][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
