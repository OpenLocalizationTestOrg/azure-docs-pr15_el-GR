<properties 
pageTitle="Ενεργοποίηση σύνδεση απομακρυσμένης επιφάνειας εργασίας για ένα ρόλο στις υπηρεσίες Azure Cloud" 
description="Πώς μπορείτε να ρυθμίσετε τις παραμέτρους του azure cloud εφαρμογής υπηρεσίας για να επιτρέψετε συνδέσεις απομακρυσμένης επιφάνειας εργασίας" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Ενεργοποίηση σύνδεση απομακρυσμένης επιφάνειας εργασίας για ένα ρόλο στις υπηρεσίες Azure Cloud

>[AZURE.SELECTOR]
- [Azure κλασική πύλη](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Σύνδεση απομακρυσμένης επιφάνειας εργασίας σάς επιτρέπει να αποκτήσετε πρόσβαση στην επιφάνεια εργασίας του ρόλου εκτελείται στο Azure. Μπορείτε να χρησιμοποιήσετε μια σύνδεση απομακρυσμένης επιφάνειας εργασίας για αντιμετώπιση προβλημάτων και διάγνωση προβλημάτων με την εφαρμογή σας, ενώ εκτελείται. 

Μπορείτε να ενεργοποιήσετε μια σύνδεση απομακρυσμένης επιφάνειας εργασίας ο ρόλος σας κατά την ανάπτυξη, συμπεριλαμβάνοντας τις λειτουργικές μονάδες απομακρυσμένης επιφάνειας εργασίας στον ορισμό σας της υπηρεσίας ή μπορείτε να επιλέξετε την ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας έως την επέκταση απομακρυσμένης επιφάνειας εργασίας. Η προτιμώμενη προσέγγιση είναι να χρησιμοποιήσετε την επέκταση απομακρυσμένης επιφάνειας εργασίας, όπως μπορείτε να ενεργοποιήσετε απομακρυσμένης επιφάνειας εργασίας, ακόμα και μετά την εφαρμογή έχει αναπτυχθεί χωρίς να χρειάζεται να αναπτύξετε ξανά την εφαρμογή σας. 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Ρύθμιση παραμέτρων απομακρυσμένης επιφάνειας εργασίας από την πύλη κλασική του Azure
Πύλη του Azure κλασική χρησιμοποιεί η προσέγγιση επέκταση απομακρυσμένης επιφάνειας εργασίας, ώστε να μπορείτε να ενεργοποιήσετε απομακρυσμένης επιφάνειας εργασίας, ακόμα και μετά την ανάπτυξη της εφαρμογής. Η σελίδα **Ρύθμιση παραμέτρων** της υπηρεσίας cloud σάς επιτρέπει να ενεργοποίηση απομακρυσμένης επιφάνειας εργασίας, την αλλαγή του λογαριασμού τοπικού διαχειριστή που χρησιμοποιείται για σύνδεση με τις εικονικές μηχανές, το πιστοποιητικό που χρησιμοποιείται στον έλεγχο ταυτότητας και ορίστε την ημερομηνία λήξης. 


1. Κάντε κλικ στην επιλογή **Υπηρεσίες Cloud**, κάντε κλικ στο όνομα της υπηρεσίας cloud και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**.

2. Κάντε κλικ στην επιλογή **απομακρυσμένο**.
    
    ![Υπηρεσίες cloud της απομακρυσμένης](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] Όλες τις εμφανίσεις ρόλο θα γίνει επανεκκίνηση κατά την πρώτη ενεργοποίηση απομακρυσμένης επιφάνειας εργασίας και κάντε κλικ στο κουμπί OK (σημάδι ελέγχου). Για να αποτρέψετε την επανεκκίνηση, πρέπει να έχει εγκατασταθεί το πιστοποιητικό που χρησιμοποιείται για την κρυπτογράφηση του κωδικού πρόσβασης σχετικά με το ρόλο. Για να αποτρέψετε την επανεκκίνηση, [αποστείλετε ένα πιστοποιητικό για την υπηρεσία cloud](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service) και, στη συνέχεια, επιστρέψτε σε αυτό το παράθυρο διαλόγου.
    

3. Σε **ρόλους**, επιλέξτε το ρόλο που θέλετε να ενημερώσετε ή επιλέξτε **όλα** για όλους τους ρόλους.

4. Κάντε οποιαδήποτε από τις ακόλουθες αλλαγές:
    
    - Για να ενεργοποιήσετε απομακρυσμένης επιφάνειας εργασίας, επιλέξτε το πλαίσιο ελέγχου **Ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας** . Για να απενεργοποιήσετε τη σύνδεση απομακρυσμένης επιφάνειας εργασίας, καταργήστε την επιλογή από το πλαίσιο ελέγχου.
    
    - Δημιουργήστε ένα λογαριασμό για να χρησιμοποιήσετε σε συνδέσεις απομακρυσμένης επιφάνειας εργασίας για να τις παρουσίες ρόλο.
    
    - Ενημερώστε τον κωδικό πρόσβασης για τον υπάρχοντα λογαριασμό.
    
    - Επιλέξτε ένα πιστοποιητικό που έχουν αποσταλεί να χρησιμοποιήσετε για τον έλεγχο ταυτότητας (αποστολής το πιστοποιητικό που χρησιμοποιεί **Αποστολή** στη σελίδα **πιστοποιητικά** ) ή να δημιουργήσετε ένα νέο πιστοποιητικό. 
    
    - Αλλάξτε την ημερομηνία λήξης για τη ρύθμιση παραμέτρων απομακρυσμένης επιφάνειας εργασίας.

5. Όταν ολοκληρώσετε τις ενημερώσεις σας ρύθμισης παραμέτρων, κάντε κλικ στο **κουμπί OK** (σημάδι ελέγχου).


## <a name="remote-into-role-instances"></a>Remote σε παρουσίες ρόλων
Μόλις ενεργοποιηθεί η σύνδεση απομακρυσμένης επιφάνειας εργασίας σε τους ρόλους, μπορείτε να remote σε μια παρουσία ρόλο μέσω διάφορα εργαλεία.

Για να συνδεθείτε σε ένα ρόλο παρουσία από την πύλη του Azure κλασική:
    
  1.   Κάντε κλικ για να ανοίξετε τη σελίδα **παρουσίες** **παρουσίες** .
  2.   Επιλέξτε μια παρουσία ρόλο που έχει απομακρυσμένης επιφάνειας εργασίας που έχει ρυθμιστεί.
  3.   Κάντε κλικ στην επιλογή **σύνδεση**και ακολουθήστε τις οδηγίες για να ανοίξετε την επιφάνεια εργασίας. 
  4.   Κάντε κλικ στην επιλογή **Άνοιγμα** και, στη συνέχεια, **σύνδεση** για να ξεκινήσετε τη σύνδεση απομακρυσμένης επιφάνειας εργασίας. 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Χρησιμοποιήστε το Visual Studio σε απομακρυσμένη σε μια παρουσία ρόλων

Στο Visual Studio, Εξερεύνηση Server:

1. Αναπτύξτε το **Azure\\τις υπηρεσίες Cloud\\[όνομα υπηρεσίας cloud]** κόμβο.
2. Ανάπτυξη **οργάνωσης** ή **παραγωγής**.
3. Αναπτύξτε το ρόλο μεμονωμένα.
4. Κάντε δεξί κλικ σε μία από τις παρουσίες ρόλου, κάντε κλικ στην επιλογή **σύνδεση με χρήση... απομακρυσμένης επιφάνειας εργασίας**και, στη συνέχεια, πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης. 

![Εξερεύνηση διακομιστή απομακρυσμένης επιφάνειας εργασίας](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>Χρήση του PowerShell για να μεταβείτε στο αρχείο RDP
Μπορείτε να χρησιμοποιήσετε το cmdlet [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) για να ανακτήσετε το αρχείο RDP. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε το αρχείο RDP με σύνδεση απομακρυσμένης επιφάνειας εργασίας για να αποκτήσετε πρόσβαση στην υπηρεσία cloud.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Μέσω προγραμματισμού κάντε λήψη του αρχείου RDP έως την υπηρεσία διαχείρισης REST API
Μπορείτε να χρησιμοποιήσετε τη λειτουργία ΥΠΌΛΟΙΠΟ [Λήψη αρχείου RDP](https://msdn.microsoft.com/library/jj157183.aspx) για να κάνετε λήψη του αρχείου RDP. 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Να ρυθμίσετε τις παραμέτρους απομακρυσμένης επιφάνειας εργασίας στο αρχείο ορισμού υπηρεσίας

Αυτή η μέθοδος σάς επιτρέπει να ενεργοποίηση απομακρυσμένης επιφάνειας εργασίας για την εφαρμογή κατά την ανάπτυξη. Αυτή η προσέγγιση απαιτεί κρυπτογραφημένη τους κωδικούς πρόσβασης είναι αποθηκευμένα στη ρύθμιση παραμέτρων της υπηρεσίας του αρχείου και ενημέρωση σχετικά με τη ρύθμιση παραμέτρων απομακρυσμένης επιφάνειας εργασίας απαιτείται μια επανάληψη ανάπτυξης της εφαρμογής. Εάν θέλετε να αποφύγετε αυτά τα downsides θα πρέπει να χρησιμοποιήσετε την προσέγγιση απομακρυσμένης επιφάνειας εργασίας επέκταση βάσει που περιγράφονται παραπάνω.  

Μπορείτε να χρησιμοποιήσετε το Visual Studio για να [ενεργοποιήσετε μια σύνδεση απομακρυσμένης επιφάνειας εργασίας](../vs-azure-tools-remote-desktop-roles.md) με χρήση της προσέγγισης αρχείο ορισμού υπηρεσίας.  
Τα παρακάτω βήματα περιγράφουν τις αλλαγές που απαιτείται για τα αρχεία του μοντέλου service για την ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας. Visual Studio θα λαμβάνει αυτόματα αυτές οι αλλαγές κατά τη δημοσίευση.

### <a name="set-up-the-connection-in-the-service-model"></a>Ρύθμιση της σύνδεσης στο μοντέλο υπηρεσίας 
Χρησιμοποιήστε το στοιχείο **εισαγωγές** για την εισαγωγή της λειτουργικής μονάδας **απομακρυσμένης πρόσβασης** και τη λειτουργική μονάδα **RemoteForwarder** στο αρχείο [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) .

Το αρχείο ορισμού υπηρεσίας πρέπει να μοιάζει με το παρακάτω παράδειγμα με το `<Imports>` στοιχείο που προσθέσατε.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Το αρχείο [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) θα πρέπει να είναι παρόμοια με το ακόλουθο παράδειγμα, σημειώστε το `<ConfigurationSettings>` και `<Certificates>` στοιχεία. Το πιστοποιητικό που καθορίζεται πρέπει να [αποσταλεί στην υπηρεσία cloud](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Πρόσθετοι πόροι

[Πώς μπορείτε να ρυθμίσετε τις υπηρεσίες Cloud](cloud-services-how-to-configure.md)