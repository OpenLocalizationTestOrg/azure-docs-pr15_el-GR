<properties
   pageTitle="Λειτουργία ύφασμα και ανάπτυξη κοντέινερ | Microsoft Azure"
   description="Υπηρεσία ύφασμα και τη χρήση του κοντέινερ για την ανάπτυξη εφαρμογών microservice. Σε αυτό το άρθρο περιγράφει τις δυνατότητες που παρέχει το ύφασμα υπηρεσίας για κοντέινερ και πώς μπορείτε να αναπτύξετε μια εικόνα κοντέινερ των Windows σε ένα σύμπλεγμα"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Προεπισκόπηση: Ανάπτυξη ενός κοντέινερ Windows ύφασμα υπηρεσίας

> [AZURE.SELECTOR]
- [Ανάπτυξη των Windows κοντέινερ](service-fabric-deploy-container.md)
- [Ανάπτυξη Docker κοντέινερ](service-fabric-deploy-container-linux.md)

Σε αυτό το άρθρο σάς καθοδηγεί σε κατασκευή περιεχόμενα υπηρεσίες στο κοντέινερ των Windows. 

>[AZURE.NOTE] Αυτή η δυνατότητα είναι σε προεπισκόπηση για Linux και δεν είναι διαθέσιμη αυτήν τη στιγμή για χρήση με το Windows Server 2016. Αυτό θα είναι διαθέσιμη στο preview για Windows Server 2016 στην επόμενη έκδοση της υπηρεσίας ύφασμα και υποστηρίζεται στην έκδοση οι επόμενες μετά από αυτό.

Υπηρεσία ύφασμα έχει πολλές δυνατότητες κοντέινερ που σας βοηθούν να με τη δημιουργία εφαρμογών που αποτελούνται από microservices που είναι περιεχόμενα. Αυτά τα πεδία ονομάζονται περιεχόμενα υπηρεσίες. 

Συμπεριλάβετε τις δυνατότητες;

- Ανάπτυξη ειδώλου κοντέινερ και την ενεργοποίηση
- Πόρος διακυβέρνησης
- Έλεγχος ταυτότητας αποθετήριο δεδομένων
- Θύρα κοντέινερ για αντιστοίχιση θύρας κεντρικού υπολογιστή
- Κοντέινερ-κοντέινερ εντοπισμού και επικοινωνία
- Δυνατότητα να ρυθμίσετε τις παραμέτρους και να ρυθμίσετε μεταβλητές περιβάλλοντος

Σας επιτρέπει να δούμε κάθε μία από τις δυνατότητες με τη σειρά όταν συσκευασία περιεχόμενα υπηρεσίας που θα συμπεριληφθούν στην εφαρμογή σας.

## <a name="packaging-a-windows-container"></a>Συσκευασία ένα κοντέινερ των Windows

Όταν συσκευασία ένα κοντέινερ, μπορείτε να επιλέξετε είτε να χρησιμοποιήσετε ένα πρότυπο έργου Visual Studio ή να [δημιουργήσετε με μη αυτόματο τρόπο το πακέτο εφαρμογών](#manually). Χρήση του Visual Studio, τη δομή του πακέτου εφαρμογής και δήλωσης αρχεία δημιουργούνται από τον "Οδηγό νέου έργου" για εσάς (αυτό σύντομα στην επόμενη έκδοση).

## <a name="using-visual-studio-to-package-an-existing-container-image"></a>Χρήση του Visual Studio για να συσκευάσετε μια υπάρχουσα εικόνα κοντέινερ

>[AZURE.NOTE] Σε μια μελλοντική έκδοση του Visual Studio tooling SDK, θα μπορείτε να προσθέσετε ένα κοντέινερ σε μια εφαρμογή με παρόμοιο τρόπο που μπορείτε να προσθέσετε εκτελέσιμο επισκέπτης σήμερα. Ανατρέξτε στο θέμα [Ανάπτυξη επισκέπτης εκτελέσιμο σε ύφασμα υπηρεσίας](service-fabric-deploy-existing-app.md) . Αυτήν τη στιγμή που πρέπει να κάνετε μη αυτόματη συσκευασία, όπως περιγράφεται παρακάτω.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-a-container"></a>Συσκευασία με μη αυτόματο τρόπο και την ανάπτυξη ενός κοντέινερ
Η διαδικασία με μη αυτόματο τρόπο συσκευασία περιεχόμενα υπηρεσίας βασίζεται σε τα παρακάτω βήματα:

1. Δημοσίευση του κοντέινερ χώρου αποθήκευσης στο.
2. Δημιουργία δομής του πακέτου καταλόγου.
3. Επεξεργαστείτε το αρχείο δήλωσης υπηρεσίας.
4. Επεξεργαστείτε το αρχείο δηλώσεων εφαρμογής.

## <a name="container-image-deployment-and-activation"></a>Ανάπτυξη ειδώλου κοντέινερ και την ενεργοποίηση.
Στην υπηρεσία ύφασμα [μοντέλο εφαρμογών](service-fabric-application-model.md), ένα κοντέινερ αντιπροσωπεύει μια υπηρεσία παροχής φιλοξενίας εφαρμογής στην οποία τοποθετούνται πολλά αντίγραφα υπηρεσίας. Για να αναπτύξετε και να ενεργοποιήσετε ένα κοντέινερ, τοποθετήστε το όνομα της εικόνας κοντέινερ σε μια `ContainerHost` στοιχείο στο δηλωτικό υπηρεσίας.

Στο δηλωτικό υπηρεσίας Προσθήκη ενός `ContainerHost` για την καταχώρηση σημείο και ορίστε την `ImageName` να είναι το όνομα του αποθετηρίου κοντέινερ και εικόνα. Το παρακάτω μερική δηλωτικό παρουσιάζει ένα παράδειγμα για την ανάπτυξη του κοντέινερ που ονομάζεται *myimage:v1* από ένα αποθετήριο που ονομάζεται *myrepo*

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Μπορείτε να παράσχετε εντολές εισαγωγής στην εικόνα κοντέινερ, καθορίζοντας την προαιρετική `Commands` στοιχείο με κόμμα Διαχ.: το σύνολο των εντολών για την εκτέλεση μέσα στο κοντέινερ. 

## <a name="resource-governance"></a>Πόρος διακυβέρνησης
Διαχείριση πόρων είναι μια δυνατότητα του κοντέινερ και περιορίζει τους πόρους που μπορούν να χρησιμοποιούν το κοντέινερ στον κεντρικό υπολογιστή. Το `ResourceGovernancePolicy`, που καθορίζεται στο δηλωτικό εφαρμογής, παρέχει τη δυνατότητα να δηλώσετε όρια πόρων για ένα πακέτο κωδικός υπηρεσίας. Όρια πόρων μπορεί να οριστεί για;

- Μνήμη
- MemorySwap
- CpuShares (σχετική CPU βάρος)
- MemoryReservationInMB  
- BlkioWeight (σχετική BlockIO βάρος). 

>[AZURE.NOTE] Σε μια μελλοντική έκδοση, υποστήριξη για να καθορίσετε συγκεκριμένες μπλοκ εισόδου/ΕΞΌΔΟΥ όρια όπως IOP, BPS και οι άλλοι χρήστες θα είναι δυνατή η ανάγνωση/εγγραφή.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Έλεγχος ταυτότητας αποθετήριο δεδομένων
Για να κάνετε λήψη ενός κοντέινερ που ίσως χρειαστεί να παρέχετε τα διαπιστευτήρια σύνδεσής του αποθετηρίου κοντέινερ. Τα διαπιστευτήρια σύνδεσης, που καθορίζεται στο δηλωτικό *εφαρμογής* που χρησιμοποιούνται για να καθορίσετε τις πληροφορίες σύνδεσης ή SSH αριθμό-κλειδί, για τη λήψη της εικόνας κοντέινερ από το χώρο αποθήκευσης εικόνα.  Το παρακάτω παράδειγμα εμφανίζει ένα λογαριασμό που ονομάζεται *TestUser* μαζί με τον κωδικό πρόσβασης σε απλό κείμενο. Αυτό **δεν** συνιστάται.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Ο κωδικός πρόσβασης μπορεί να και θα πρέπει να είναι κρυπτογραφημένη χρησιμοποιώντας ένα πιστοποιητικό που έχει αναπτυχθεί σε υπολογιστή.

Το παρακάτω παράδειγμα εμφανίζει ένα λογαριασμό που ονομάζεται *TestUser* με τον κωδικό πρόσβασης κρυπτογραφημένη χρησιμοποιώντας ένα πιστοποιητικό που ονομάζεται *MyCert*. Μπορείτε να χρησιμοποιήσετε το `Invoke-ServiceFabricEncryptText` εντολή Powershell για να δημιουργήσετε το κείμενο μυστικού κρυπτογράφησης για τον κωδικό πρόσβασης. Ανατρέξτε στο άρθρο [Διαχείριση απορρήτου σε εφαρμογές της υπηρεσίας ύφασμα](service-fabric-application-secret-management.md) για λεπτομέρειες σχετικά με τον τρόπο. Το ιδιωτικό κλειδί του πιστοποιητικού για να αποκρυπτογραφήσετε τον κωδικό πρόσβασης, πρέπει να αναπτυχθούν στον τοπικό υπολογιστή σε μια μέθοδο εκτός εύρους (στο Azure αυτό είναι μέσω ARM). Στη συνέχεια, όταν ύφασμα υπηρεσίας αναπτύσσει το πακέτο υπηρεσίας στον υπολογιστή, είναι μπορεί να αποκρυπτογραφήσει το μυστικό και μαζί με το όνομα του λογαριασμού, τον έλεγχο ταυτότητας με το αρχείο φύλαξης κοντέινερ με αυτά τα διαπιστευτήρια.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>Θύρα κοντέινερ για αντιστοίχιση θύρας κεντρικού υπολογιστή
Μπορείτε να ρυθμίσετε μια θύρα κεντρικού υπολογιστή που χρησιμοποιούνται για την επικοινωνία με το κοντέινερ, καθορίζοντας μια `PortBinding` στη δήλωση εφαρμογής. Η σύνδεση θύρας χαρτών θύρας στην οποία η υπηρεσία ακρόαση σε μέσα στο κοντέινερ σε μια θύρα στον κεντρικό υπολογιστή.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>Κοντέινερ-κοντέινερ εντοπισμού και επικοινωνία
Χρησιμοποιώντας το `PortBinding` πολιτικής, μπορείτε να αντιστοιχίσετε μια θύρα κοντέινερ για μια `Endpoint` στο δηλωτικό υπηρεσίας όπως φαίνεται στο παρακάτω παράδειγμα. Το τελικό σημείο `Endpoint1` να καθορίσετε μια σταθερή θύρα, για παράδειγμα θύρα 80 ή καθορίστε τη θύρα δεν υπάρχει καθόλου, οπότε επιλέγεται μια τυχαία θύρα από περιοχή θύρα εφαρμογής των συμπλεγμάτων για εσάς.

Για κοντέινερ επισκέπτη, καθορίζοντας μια `Endpoint` όπως αυτό στο δηλωτικό υπηρεσίας δίνει τη δυνατότητα ύφασμα υπηρεσίας για να δημοσιεύσετε αυτόματα αυτό το τελικό σημείο για την υπηρεσία ονομάτων, ώστε να μπορούν να εντοπίζουν αυτό το κοντέινερ χρησιμοποιώντας τα ερωτήματα επίλυση υπηρεσίας ΥΠΌΛΟΙΠΑ άλλες υπηρεσίες που εκτελούνται στο σύμπλεγμα. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Κατά την εγγραφή σας με την υπηρεσία ονομάτων, μπορείτε να κάνετε εύκολα κοντέινερ-κοντέινερ επικοινωνίας στον κώδικα εντός του κοντέινερ χρησιμοποιώντας το [αντίστροφης μεσολάβησης](service-fabric-reverseproxy.md). Μόνο που πρέπει να κάνετε είναι να παρέχουν τη θύρα ακρόασης αντίστροφης μεσολάβησης http και το όνομα των υπηρεσιών που θέλετε να επικοινωνήσετε με ορίζοντας τους ως μεταβλητές περιβάλλοντος. Ανατρέξτε στην επόμενη ενότητα σχετικά με τον τρόπο για να το κάνετε αυτό.  

## <a name="configure-and-set-environment-variables"></a>Ρύθμιση παραμέτρων και ορίστε μεταβλητές περιβάλλοντος
Μεταβλητές περιβάλλοντος μπορεί να είναι ένας καθορισμένος εχθρός κάθε πακέτο κώδικα στην υπηρεσία δήλωσης για τις υπηρεσίες και τις δύο αναπτυχθεί σε κοντέινερ ή ως εκτελέσιμα διεργασίες/επισκέπτη. Αυτές τις τιμές μεταβλητών περιβάλλοντος είναι δυνατό να παρακαμφθούν συγκεκριμένα στο δηλωτικό εφαρμογής ή που καθορίζεται κατά την ανάπτυξη ως οι παράμετροι της εφαρμογής.

Το παρακάτω δηλωτικό υπηρεσίας XML τμήματος κώδικα εμφανίζει ένα παράδειγμα του τρόπου για να καθορίσετε μεταβλητές περιβάλλοντος για ένα πακέτο κώδικα. 

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>

Μπορείτε να παρακάμψετε αυτές τις μεταβλητές περιβάλλοντος σε επίπεδο εφαρμογής δήλωσης:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

Στο παραπάνω παράδειγμα, θα σας έχετε καθορίσει μια ρητή τιμή για το `HttpGateway` μεταβλητή περιβάλλοντος (19000) κατά την τιμή για `BackendServiceName` η παράμετρος οριστεί μέσω του `[BackendSvc]` παραμέτρων εφαρμογής. Αυτό σας επιτρέπει να καθορίσετε την τιμή για `BackendServiceName`τιμή κατά τη διάρκεια του για την ανάπτυξη της εφαρμογής και δεν έχουν μια σταθερή τιμή στο δηλωτικό. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Ολοκλήρωση παραδείγματα για την εφαρμογή και δηλωτικό υπηρεσίας
Ακολουθεί μια δήλωση εφαρμογή παράδειγμα που εμφανίζει τις δυνατότητες του κοντέινερ.


    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>


Ακολουθεί ένα παράδειγμα υπηρεσίας δηλωτικό (που καθορίζεται στο προηγούμενο δηλωτικό εφαρμογής) που εμφανίζει τις δυνατότητες του κοντέινερ

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>
