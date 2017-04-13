<properties 
pageTitle="Σύννεφο ρόλο υπηρεσιών config XPath σκονάκι | Microsoft Azure" 
description="Τις διάφορες ρυθμίσεις XPath μπορείτε να χρησιμοποιήσετε στο αρχείο config ρόλο υπηρεσία cloud για να εκθέσετε ρυθμίσεις ως μια μεταβλητή περιβάλλοντος." 
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
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Έκθεση ρυθμίσεις παραμέτρων ρόλο ως μια μεταβλητή περιβάλλοντος με XPath

Στο αρχείο ορισμού υπηρεσίας web ρόλο ή εργαζόμενου υπηρεσία cloud, να μπορείτε να εμφανίσετε τιμές παραμέτρων χρόνου εκτέλεσης ως μεταβλητές περιβάλλοντος. Υποστηρίζονται οι ακόλουθες τιμές XPath (που αντιστοιχούν στις τιμές API).

Αυτές οι τιμές XPath είναι επίσης διαθέσιμες μέσω της βιβλιοθήκης [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) . 

## <a name="app-running-in-emulator"></a>Εφαρμογή εκτελείται στο προσομοίωσης

Υποδεικνύει ότι η εφαρμογή εκτελείται σε το προσομοίωσης.

| Τύπος  | Παράδειγμα |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| Κωδικός  | VAR x = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>Αναγνωριστικό ανάπτυξης

Ανακτά το Αναγνωριστικό ανάπτυξης για την παρουσία.

| Τύπος  | Παράδειγμα |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@id" |
| Κωδικός  | VAR deploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>Αναγνωριστικό ρόλου 

Ανακτά το τρέχον Αναγνωριστικό ρόλο για την παρουσία.

| Τύπος  | Παράδειγμα |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| Κωδικός  | αναγνωριστικό VAR = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>Ενημέρωση τομέα

Ανακτά τον τομέα ενημέρωση της παρουσίας.

| Τύπος  | Παράδειγμα |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Κωδικός  | VAR ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>Σφάλμα τομέα

Ανακτά τον τομέα σφαλμάτων της παρουσίας.

| Τύπος  | Παράδειγμα |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Κωδικός  | VAR fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>Όνομα ρόλου

Ανακτά το όνομα του ρόλου των παρουσιών.

| Τύπος  | Παράδειγμα |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Κωδικός  | VAR rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>Ρύθμιση παραμέτρων

Ανακτά την τιμή της ρύθμισης παραμέτρων καθορισμένο.

| Τύπος  | Παράδειγμα |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Κωδικός  | ρύθμιση VAR = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>Διαδρομή τοπικού χώρου αποθήκευσης

Ανακτά τη διαδρομή του τοπικού χώρου αποθήκευσης για την παρουσία.

| Τύπος  | Παράδειγμα |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Κωδικός  | VAR localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |


## <a name="local-storage-size"></a>Μέγεθος του τοπικού χώρου αποθήκευσης

Ανακτά το μέγεθος του τοπικού χώρου αποθήκευσης για την παρουσία.

| Τύπος  | Παράδειγμα |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Κωδικός  | VAR localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Πρωτόκολλο τελικού σημείου 

Ανακτά το πρωτόκολλο τελικό σημείο για την παρουσία.

| Τύπος  | Παράδειγμα |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Κωδικός  | VAR prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Πρωτόκολλο. |

## <a name="endpoint-ip"></a>Τελικό σημείο IP

Λαμβάνει το καθορισμένο τελικό σημείο διεύθυνση IP.

| Τύπος | Παράδειγμα |
| ----- | ---- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Κωδικός  | διεύθυνση VAR = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Θύρα τελικού σημείου 

Ανακτά τη θύρα τελικό σημείο για την παρουσία.

| Τύπος  | Παράδειγμα |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Κωδικός  | θύρα VAR = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port; |





## <a name="example"></a>Παράδειγμα

Ακολουθεί ένα παράδειγμα ενός ρόλου εργαζόμενου που δημιουργεί μια εργασία εκκίνησης με μια μεταβλητή περιβάλλοντος με το όνομα `TestIsEmulated` ρύθμιση για το [ @emulated xpath τιμή](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με το αρχείο [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) .

Δημιουργήστε ένα πακέτο [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) .

Ενεργοποίηση της [απομακρυσμένης επιφάνειας εργασίας](cloud-services-role-enable-remote-desktop.md) για ένα ρόλο.
