<properties
   pageTitle="Κατανόηση της εφαρμογής υπηρεσίας ύφασμα και πολιτικών ασφάλειας υπηρεσίας | Microsoft Azure"
   description="Μάθετε πώς να εκτελέσετε μια εφαρμογή υπηρεσίας ύφασμα στην περιοχή σύστημα και λογαριασμούς τοπικής ασφαλείας, όπως το σημείο SetupEntry όπου μια εφαρμογή πρέπει να εκτελέσετε κάποια ενέργεια Προνομιούχους πριν να ξεκινήσει"
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
   ms.date="09/22/2016"
   ms.author="mfussell"/>

# <a name="configure-security-policies-for-your-application"></a>Ρύθμιση παραμέτρων πολιτικών ασφάλειας για την εφαρμογή σας
Azure ύφασμα υπηρεσία παρέχει τη δυνατότητα για την ασφάλιση εφαρμογές που εκτελούνται στο σύμπλεγμα στην περιοχή διαφορετικούς λογαριασμούς χρηστών. Υπηρεσία ύφασμα διασφαλίζει επίσης τους πόρους που χρησιμοποιούνται από τις εφαρμογές τη στιγμή της ανάπτυξης στην περιοχή ο λογαριασμός χρήστη όπως αρχεία, καταλόγους και πιστοποιητικά. Με αυτόν τον τρόπο εφαρμογές που εκτελούνται, ακόμη και σε ένα κοινόχρηστο περιβάλλον φιλοξενούμενο, ασφαλή από το ένα το άλλο. 

Από προεπιλογή, οι εφαρμογές υπηρεσίας ύφασμα εκτελούνται κάτω από το λογαριασμό στον οποίο εκτελείται η διαδικασία Fabric.exe. Υπηρεσία ύφασμα παρέχει επίσης τη δυνατότητα να εκτελείτε εφαρμογές στην περιοχή ένα τοπικό λογαριασμό χρήστη ή το λογαριασμό τοπικό σύστημα, που καθορίζεται μέσα στη δήλωση της εφαρμογής. Τοπικό σύστημα που υποστηρίζεται τύπους λογαριασμών για είναι **LocalUser**, **NetworkService**, **LocalService**και **τοπικού συστήματος**.

 Κατά την εκτέλεση ύφασμα υπηρεσίας σε Windows Server στο κέντρο δεδομένων σας χρησιμοποιώντας το πρόγραμμα εγκατάστασης του μεμονωμένου, μπορείτε να χρησιμοποιήσετε λογαριασμούς τομέα Active Directory (AD).

Ομάδες χρηστών είναι δυνατό να οριστούν και να δημιουργήθηκε έτσι ώστε να μπορείτε να προσθέσετε έναν ή περισσότερους χρήστες σε κάθε ομάδα να γίνεται μαζί. Αυτό είναι χρήσιμο όταν υπάρχουν πολλοί χρήστες για διαφορετική υπηρεσία σημεία εισόδου και χρειάζεται να έχετε ορισμένα κοινά δικαιώματα που είναι διαθέσιμες σε επίπεδο ομάδας.

## <a name="configure-the-policy-for-service-setupentrypoint"></a>Ρύθμιση παραμέτρων της πολιτικής για την υπηρεσία SetupEntryPoint

Όπως περιγράφεται στο [μοντέλο εφαρμογής](service-fabric-application-model.md) το **SetupEntryPoint** είναι ένα σημείο εισόδου Προνομιούχους που εκτελείται με τα ίδια διαπιστευτήρια ως υπηρεσία ύφασμα (συνήθως το λογαριασμό *NetworkService* ) πριν από οποιοδήποτε άλλο σημείο εισόδου. Το εκτελέσιμο αρχείο που καθορίζεται από **EntryPoint** είναι συνήθως ο κεντρικός υπολογιστής υπηρεσίας μεγάλη διάρκεια εκτέλεσης, ώστε να αντιμετωπίζετε ένα σημείο καταχώρησης ξεχωριστή εγκατάστασης αποφεύγεται χρειάζεται να εκτελέσετε τον κεντρικό υπολογιστή υπηρεσίας εκτελέσιμο με υψηλή δικαιώματα για το εκτεταμένο χρονικά διαστήματα. Το εκτελέσιμο αρχείο που καθορίζεται από **EntryPoint** εκτελείται μετά την έξοδο **SetupEntryPoint** με επιτυχία. Η διαδικασία που προκύπτουν είναι παρακολουθούνται και την επανεκκίνηση, ξεκινώντας ξανά με **SetupEntryPoint**, αν ποτέ καταγγελία ή παρουσιάζει σφάλμα.

Ακολουθεί ένα παράδειγμα δήλωσης απλό υπηρεσίας που δείχνει το SetupEntryPoint και το κύριο EntryPoint για την υπηρεσία.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-using-a-local-account"></a>Ρύθμιση παραμέτρων της πολιτικής χρησιμοποιώντας ένα λογαριασμό του τοπικού

Αφού ρυθμίσετε την υπηρεσία για να έχετε ένα σημείο καταχώρησης εγκατάστασης, μπορείτε να αλλάξετε τα δικαιώματα ασφαλείας που εκτελείται στο δηλωτικό εφαρμογής. Το ακόλουθο παράδειγμα δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους της υπηρεσίας για να εκτελέσετε στην περιοχή δικαιώματα λογαριασμού διαχειριστή του χρήστη.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Πρώτα, δημιουργήστε μια ενότητα **αρχές** με ένα όνομα χρήστη, όπως SetupAdminUser. Αυτό υποδεικνύει ότι ο χρήστης είναι μέλος της ομάδας διαχειριστών συστήματος.

Στη συνέχεια, στην περιοχή **ServiceManifestImport** , ρυθμίστε τις παραμέτρους μιας πολιτικής για να εφαρμόσετε σε αυτό το κεφάλαιο **SetupEntryPoint**. Αυτό σας ενημερώνει ύφασμα υπηρεσία ότι όταν εκτελείται το αρχείο **MySetup.bat** , θα πρέπει να είναι RunAs με δικαιώματα διαχειριστή. Δεδομένου ότι έχετε *δεν* εφαρμόζεται μια πολιτική για το κύριο σημείο εισόδου, ο κώδικας στο **MyServiceHost.exe** εκτελείται στο πλαίσιο του συστήματος λογαριασμό **NetworkService** . Πρόκειται για τον προεπιλεγμένο λογαριασμό που εκτελούνται ως όλα τα σημεία καταχώρησης υπηρεσίας.

Προσθέστε τώρα το αρχείο MySetup.bat στο έργο Visual Studio για να ελέγξετε τα δικαιώματα διαχειριστή. Στο Visual Studio, κάντε δεξί κλικ στο έργο υπηρεσιών και προσθέστε ένα νέο αρχείο που ονομάζεται MySetup.bat.

Στη συνέχεια, βεβαιωθείτε ότι το αρχείο MySetup.bat περιλαμβάνεται στο πακέτο υπηρεσίας. Από προεπιλογή, δεν είναι. Επιλέξτε το αρχείο, κάντε δεξί κλικ για να λάβετε το μενού περιβάλλοντος και επιλέξτε **Ιδιότητες**. Στο παράθυρο διαλόγου Ιδιότητες, βεβαιωθείτε ότι **η αντιγραφή στον κατάλογο εξόδου** έχει οριστεί για να **αντιγράψετε εάν νεότερη έκδοση**. Δείτε το παρακάτω στιγμιότυπο οθόνης.

![Visual Studio CopyToOutput για το αρχείο δέσμης SetupEntryPoint][image1]

Τώρα, ανοίξτε το αρχείο MySetup.bat και προσθέστε τις παρακάτω εντολές:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Στη συνέχεια, δημιουργία και ανάπτυξη της λύσης σε ένα σύμπλεγμα τοπικής ανάπτυξης.  Αφού έχει ξεκινήσει η υπηρεσία, όπως φαίνεται στην Εξερεύνηση ύφασμα υπηρεσίας, μπορείτε να δείτε ότι το MySetup.bat ολοκληρώθηκε με επιτυχία με μια δύο τρόπους. Ανοίξτε μια γραμμή εντολών του PowerShell και πληκτρολογήστε:

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Στη συνέχεια, σημειώστε το όνομα του κόμβου όπου η υπηρεσία αναπτύχθηκε και αποτελέσματα στην Εξερεύνηση ύφασμα υπηρεσίας, για παράδειγμα, 2 κόμβο. Στη συνέχεια, μεταβείτε στο φάκελο εργασίας παρουσία της εφαρμογής για να βρείτε το αρχείο out.txt που εμφανίζει την τιμή της **TestVariable**. Για παράδειγμα, εάν αυτή η υπηρεσία έχει αναπτυχθεί σε κόμβο 2, στη συνέχεια, μπορείτε να μεταβείτε σε αυτήν τη διαδρομή για το **MyApplicationType**:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

###  <a name="configure-the-policy-using-local-system-accounts"></a>Ρύθμιση παραμέτρων της πολιτικής χρησιμοποιώντας τους λογαριασμούς τοπικό σύστημα
Συχνά είναι προτιμότερο να εκτελέσετε τη δέσμη ενεργειών εκκίνησης χρησιμοποιώντας ένα λογαριασμό τοπικό σύστημα αντί για ένα λογαριασμό διαχειριστές όπως φαίνεται πριν από. Εκτέλεση του RunAs πολιτικής ως διαχειριστές συνήθως δεν λειτουργεί καλά εφόσον μηχανές έχουν πρόσβαση ελέγχου χρήστη (UAC) ενεργοποιημένη από προεπιλογή. Σε αυτές τις περιπτώσεις, **η σύσταση είναι για να εκτελέσετε το SetupEntryPoint ως τοπικό σύστημα αντί για τοπικός χρήστης προστέθηκε σε ομάδα διαχειριστών**. Το παρακάτω παράδειγμα εμφανίζει τη ρύθμιση του SetupEntryPoint για να εκτελέσετε ως τοπικό σύστημα.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

##  <a name="launch-powershell-commands-from-a-setupentrypoint"></a>Εκκίνηση από μια SetupEntryPoint τις εντολές του PowerShell
Για να εκτελέσετε το PowerShell από το σημείο **SetupEntryPoint** , μπορείτε να εκτελέσετε **PowerShell.exe** σε ένα αρχείο δέσμης που οδηγεί σε ένα αρχείο του PowerShell. Πρώτα, προσθέστε ένα αρχείο του PowerShell στο έργο υπηρεσία, όπως **MySetup.ps1**. Μην ξεχάσετε να ορίσετε την ιδιότητα *Αντιγραφή εάν νεότερη* , έτσι ώστε το αρχείο περιλαμβάνεται επίσης στο πακέτο υπηρεσίας. Το παρακάτω παράδειγμα εμφανίζει ένα δείγμα αρχείου δέσμης για την εκκίνηση ενός αρχείου του PowerShell που ονομάζεται MySetup.ps1, η οποία ορίζει έναν μεταβλητή περιβάλλοντος που ονομάζεται **TestVariable**.


MySetup.bat εκκίνηση αρχείο PowerShell.

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

Στο αρχείο PowerShell, προσθέστε τα ακόλουθα για να ορίσετε μια μεταβλητή περιβάλλοντος συστήματος:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

**Σημείωση:** Από προεπιλογή όταν εκτελείται το αρχείο δέσμης πραγματοποιεί αναζήτηση στο φάκελο της εφαρμογής που ονομάζεται **εργασία** για τα αρχεία. Σε αυτήν την περίπτωση, κατά την εκτέλεση του MySetup.bat θέλουμε αυτό για να βρείτε το MySetup.ps1 στον ίδιο φάκελο, που βρίσκεται στο φάκελο **του πακέτου κώδικα** της εφαρμογής. Για να αλλάξετε αυτόν το φάκελο, ορίστε στο φάκελο εργασίας, όπως φαίνεται στα παρακάτω.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="using-console-redirection-for-local-debugging"></a>Χρησιμοποιώντας την ανακατεύθυνση κονσόλας για τοπική τον εντοπισμό σφαλμάτων
Μερικές φορές είναι χρήσιμο για να δείτε το αποτέλεσμα κονσόλας από την εκτέλεση μιας δέσμης ενεργειών για σκοπούς εντοπισμού σφαλμάτων. Για να το κάνετε αυτό, μπορείτε να ορίσετε μια πολιτική ανακατεύθυνσης κονσόλας, η οποία εγγράφει το αποτέλεσμα σε ένα αρχείο. Το αποτέλεσμα του αρχείου είναι γραμμένο στο φάκελο της εφαρμογής που ονομάζεται **καταγραφής** στον κόμβο όπου η εφαρμογή αναπτύσσεται και εκτέλεση (ανατρέξτε στο θέμα πού μπορώ να βρω αυτό στο προηγούμενο παράδειγμα).

**Σημείωση: ποτέ** χρησιμοποιούν την πολιτική ανακατεύθυνσης κονσόλας σε μια εφαρμογή αναπτυχθεί στο παραγωγής επειδή αυτό μπορεί να επηρεάσει την εφαρμογή ανακατεύθυνσης. **ΜΌΝΟ** Χρησιμοποιήστε αυτήν την επιλογή για σκοπούς εντοπισμού σφαλμάτων και τοπικής ανάπτυξης.  

Το παρακάτω παράδειγμα εμφανίζει τη ρύθμιση της ανακατεύθυνσης κονσόλας με τιμή FileRetentionCount.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Εάν αλλάξετε τώρα το αρχείο MySetup.ps1 για να γράψετε μια εντολή **Echo** , αυτό θα εγγραφής στο αρχείο εξόδου για σκοπούς εντοπισμού σφαλμάτων.

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Αφού έχετε εντοπισμός σφαλμάτων δέσμης ενεργειών σας, καταργήστε αμέσως αυτή την πολιτική ανακατεύθυνσης κονσόλας**

## <a name="configure-policy-for-service-code-packages"></a>Ρύθμιση παραμέτρων της πολιτικής για τα πακέτα κωδικός υπηρεσίας 
Στα προηγούμενα βήματα, είδατε πώς μπορείτε να εφαρμόσετε πολιτική RunAs σε SetupEntryPoint. Ας ρίξουμε μια ματιά λίγο βαθύτερη σε πώς μπορείτε να δημιουργήσετε διαφορετικές αρχές, οι οποίες μπορούν να εφαρμοστούν ως πολιτικές της υπηρεσίας.

### <a name="create-local-user-groups"></a>Δημιουργία τοπικού χρήστη ομάδων
Ομάδες χρηστών μπορούν να που ορίζονται από το και να δημιουργήσει που επιτρέπουν έναν ή περισσότερους χρήστες θα προστεθούν σε μια ομάδα. Αυτό είναι ιδιαίτερα χρήσιμο εάν υπάρχουν πολλοί χρήστες για διαφορετική υπηρεσία σημεία εισόδου και χρειάζεται να έχετε ορισμένα κοινά δικαιώματα που είναι διαθέσιμες σε επίπεδο ομάδας. Το παρακάτω παράδειγμα εμφανίζει μιας τοπικής ομάδας που ονομάζεται **LocalAdminGroup** με δικαιώματα διαχειριστή. Δύο χρήστες, Customer1 και Customer2, πραγματοποιούνται τα μέλη αυτής της τοπικής ομάδας.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Δημιουργία τοπικού χρηστών
Μπορείτε να δημιουργήσετε ένα τοπικό χρήστη που μπορούν να χρησιμοποιηθούν για την ασφάλιση μιας υπηρεσίας μέσα από την εφαρμογή. Όταν ένας τύπος λογαριασμού **LocalUser** καθορίζεται στην ενότητα αρχές της τη δήλωση της εφαρμογής, υπηρεσία ύφασμα δημιουργεί τοπικούς λογαριασμούς χρηστών σε υπολογιστές όπου έχει αναπτυχθεί της εφαρμογής. Από προεπιλογή, οι λογαριασμοί δεν έχουν τα ίδια ονόματα με αυτά που καθορίζεται στο δηλωτικό εφαρμογής (για παράδειγμα, Customer3 στο ακόλουθο δείγμα). Αντί για αυτό, δημιουργούνται δυναμικά και έχουν τυχαία κωδικούς πρόσβασης.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>Εκχώρηση πολιτικών τα πακέτα κωδικός υπηρεσίας
Στην ενότητα **RunAsPolicy** για μια **ServiceManifestImport** Καθορίζει το λογαριασμό από την ενότητα αρχές που πρέπει να χρησιμοποιούνται για να εκτελέσετε ένα πακέτο κώδικα. Επίσης, συσχετίζει πακέτων κώδικα από την υπηρεσία δηλωτικό με τους λογαριασμούς χρηστών στην ενότητα αρχές. Μπορείτε να καθορίσετε αυτό για το πρόγραμμα εγκατάστασης ή κύρια σημεία ή μπορείτε να καθορίσετε `All` για να το εφαρμόσετε και τα δύο. Το παρακάτω παράδειγμα εμφανίζει διαφορετικές πολιτικές που έχουν εφαρμοστεί:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Εάν δεν έχει καθοριστεί **EntryPointType** , η προεπιλογή έχει οριστεί σε EntryPointType = "Κύριες". Καθορισμός **SetupEntryPoint** είναι ιδιαίτερα χρήσιμη όταν θέλετε να εκτελέσετε ορισμένες λειτουργία του προγράμματος εγκατάστασης υψηλή δικαίωμα κάτω από ένα λογαριασμό συστήματος. Να εκτελέσετε τον κώδικα πραγματική υπηρεσίας κάτω από ένα λογαριασμό κάτω δικαίωμα.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Εφαρμογή μιας προεπιλεγμένης πολιτικής σε όλα τα πακέτα κωδικός υπηρεσίας
Στην ενότητα **DefaultRunAsPolicy** χρησιμοποιείται για να καθορίσετε έναν προεπιλεγμένο λογαριασμό χρήστη για όλα τα πακέτα κώδικα που δεν διαθέτουν ένα συγκεκριμένο **RunAsPolicy** που ορίζονται από το. Εάν χρειάζεστε περισσότερες από τα πακέτα κώδικα που καθορίζεται στο δηλωτικό υπηρεσίας που χρησιμοποιούνται από μια εφαρμογή του ώστε να εκτελείται με τον ίδιο χρήστη, η εφαρμογή απλώς να ορίσετε μια προεπιλεγμένη πολιτική RunAs με αυτόν το λογαριασμό χρήστη. Το παρακάτω παράδειγμα καθορίζει ότι εάν ένα πακέτο κώδικα δεν διαθέτει ένα **RunAsPolicy** που καθορίζονται, το πακέτο του κώδικα πρέπει να εκτελέσετε στην περιοχή **MyDefaultAccount** που καθορίζονται στην ενότητα αρχές.

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="using-an-active-directory-domain-group-or-user"></a>Με μια ομάδα τομέα της υπηρεσίας καταλόγου Active Directory ή χρήστη
Για την υπηρεσία ύφασμα εγκατασταθεί σε Windows Server με το πρόγραμμα εγκατάστασης του μεμονωμένου, μπορεί να εκτελείται η υπηρεσία τα διαπιστευτήρια για μια Active Directory (AD) λογαριασμό χρήστη ή ομάδας. Σημείωση: Αυτή είναι υπηρεσίας καταλόγου Active Directory εσωτερικής εντός του τομέα σας και δεν είναι με Azure Active Directory (AAD). Με τη χρήση ενός τομέα χρήστη ή μια ομάδα, μπορείτε, στη συνέχεια, να αποκτήσετε πρόσβαση άλλοι πόροι του τομέα (για παράδειγμα, κοινόχρηστα στοιχεία αρχείων) που έχουν εκχωρηθεί δικαιώματα.

Το παρακάτω παράδειγμα εμφανίζει έναν χρήστη AD που ονομάζεται *TestUser* με τον τομέα τον κωδικό πρόσβασής τους κρυπτογραφημένη χρησιμοποιώντας ένα πιστοποιητικό που ονομάζεται *MyCert*. Μπορείτε να χρησιμοποιήσετε το `Invoke-ServiceFabricEncryptText` εντολή Powershell για να δημιουργήσετε το μυστικό κρυπτογράφηση κείμενο. Ανατρέξτε στο άρθρο [Διαχείριση απορρήτου σε εφαρμογές της υπηρεσίας ύφασμα](service-fabric-application-secret-management.md) για λεπτομέρειες σχετικά με τον τρόπο. Το ιδιωτικό κλειδί του πιστοποιητικού για να αποκρυπτογραφήσετε τον κωδικό πρόσβασης, πρέπει να αναπτυχθούν στον τοπικό υπολογιστή σε μια μέθοδο εκτός εύρους (στο Azure αυτό είναι μέσω της διαχείρισης πόρων). Στη συνέχεια, όταν ύφασμα υπηρεσίας αναπτύσσει το πακέτο υπηρεσίας στον υπολογιστή, είναι μπορεί να αποκρυπτογραφήσει το μυστικό και μαζί με το όνομα χρήστη, τον έλεγχο ταυτότητας με AD ώστε να εκτελείται με αυτά τα διαπιστευτήρια.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-securityaccesspolicy-for-http-and-https-endpoints"></a>Εκχώρηση SecurityAccessPolicy για τα τελικά σημεία HTTP και HTTPS
Εάν μπορείτε να εφαρμόσετε μια πολιτική RunAs σε μια υπηρεσία και υπηρεσία δηλωτικό δηλώνει πόρους τελικού σημείου με το πρωτόκολλο HTTP, πρέπει να καθορίσετε μια **SecurityAccessPolicy** για να βεβαιωθείτε ότι οι θύρες εκχωρηθεί σε αυτά τα τελικά σημεία είναι σωστά που αναφέρονται για το λογαριασμό χρήστη RunAs που εκτελείται η υπηρεσία ελέγχου πρόσβασης. Διαφορετικά, **http.sys** δεν έχει πρόσβαση στην υπηρεσία και λάβετε αποτυχίες με τις κλήσεις από το πρόγραμμα-πελάτη. Το ακόλουθο παράδειγμα αφορά το λογαριασμό Customer3 ένα τελικό σημείο που ονομάζεται **ServiceEndpointName**, παρέχοντας την πλήρη δικαιώματα πρόσβασης.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

Για το τελικό σημείο HTTPS, έχετε επίσης για να υποδείξετε ότι το όνομα του πιστοποιητικού για να επιστρέψετε στο πρόγραμμα-πελάτη. Μπορείτε να το κάνετε με τη χρήση **EndpointBindingPolicy**, με το πιστοποιητικό που ορίζεται σε μια ενότητα πιστοποιητικά στο δηλωτικό εφαρμογής.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>Ένα παράδειγμα δήλωσης ολοκλήρωσης εφαρμογής
Το παρακάτω δήλωση εφαρμογής εμφανίζει πολλές από τις διαφορετικές ρυθμίσεις:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Επόμενα βήματα

* [Κατανόηση του μοντέλου εφαρμογής](service-fabric-application-model.md)
* [Καθορίστε πόρων σε μια υπηρεσία δηλωτικό](service-fabric-service-manifest-resources.md)
* [Ανάπτυξη μιας εφαρμογής](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
