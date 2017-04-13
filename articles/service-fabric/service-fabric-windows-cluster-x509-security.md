<properties
   pageTitle="Συνδεθείτε με μια ασφαλή ιδιωτική σύμπλεγμα | Microsoft Azure"
   description="Σε αυτό το άρθρο περιγράφει τον τρόπο για την ασφάλιση επικοινωνίας μέσα στη μεμονωμένη ή ιδιωτικό σύμπλεγμα καθώς και μεταξύ των υπολογιστών-πελατών και του συμπλέγματος."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Ασφαλούς ένα σύμπλεγμα μεμονωμένη στα Windows χρησιμοποιώντας πιστοποιητικά X.509

Σε αυτό το άρθρο περιγράφει τον τρόπο για την ασφαλή επικοινωνία μεταξύ τους διάφορους κόμβους του συμπλέγματος Windows μεμονωμένη, καθώς και τον τρόπο για τον έλεγχο ταυτότητας πελάτες τη σύνδεση σε αυτό το σύμπλεγμα, χρησιμοποιώντας X.509 πιστοποιητικά. Αυτό εξασφαλίζει ότι μόνο σε εξουσιοδοτημένους χρήστες να αποκτούν πρόσβαση στο σύμπλεγμα, την ανάπτυξη εφαρμογών και να εκτελέσετε εργασίες διαχείρισης.  Πιστοποιητικό ασφαλείας θα πρέπει να είναι ενεργοποιημένη στο σύμπλεγμα όταν δημιουργείται το σύμπλεγμα.  

Για περισσότερες πληροφορίες σχετικά με την ασφάλεια σύμπλεγμα όπως κόμβου προς κόμβο ασφαλείας, ασφάλεια κόμβος προγράμματος-πελάτη και έλεγχος πρόσβασης βάσει ρόλων, ανατρέξτε στο θέμα [σενάρια ασφαλείας συμπλέγματος](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Πιστοποιητικών που θα χρειαστείτε;

Για να ξεκινήσετε με, [κάντε λήψη του πακέτου σύμπλεγμα μεμονωμένη](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) σε έναν από τους κόμβους του συμπλέγματος. Λήψη του πακέτου, θα βρείτε ένα αρχείο **ClusterConfig.X509.MultiMachine.json** . Ανοίξτε το αρχείο και εξετάστε την ενότητα για την **ασφάλεια** στην ενότητα **Ιδιότητες** :

    "security": {
        "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        "CertificateInformation": {
            "ClusterCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
            "HttpApplicationGatewayCertificate":{
                "Thumbprint": "[Thumbprint]",
                "X509StoreName": "My"
            }
        }
    }

Αυτή η ενότητα περιγράφει τα πιστοποιητικά που χρειάζεστε για την ασφάλεια των μεμονωμένη συμπλέγματος των Windows. Για να ενεργοποιήσετε την ασφάλεια που βασίζεται σε πιστοποιητικό, ορίστε τις τιμές των **ClusterCredentialType** και **ServerCredentialType** σε *X509*.

>[AZURE.NOTE] Μια [αποτύπωση](https://en.wikipedia.org/wiki/Public_key_fingerprint) είναι η κύρια ταυτότητα ενός πιστοποιητικού. Διαβάστε [πώς μπορείτε να ανακτήσετε αποτύπωση του πιστοποιητικού](https://msdn.microsoft.com/library/ms734695.aspx) για να βρείτε την αποτύπωση από τα πιστοποιητικά που δημιουργείτε.

Ο παρακάτω πίνακας παραθέτει τα πιστοποιητικά που θα χρειαστείτε σε τις ρυθμίσεις του συμπλέγματος σας:

|**Ρύθμιση CertificateInformation**|**Περιγραφή**|
|-----------------------|--------------------------|
|ClusterCertificate|Αυτό το πιστοποιητικό απαιτείται για την ασφαλή επικοινωνία μεταξύ τους κόμβους σε ένα σύμπλεγμα. Μπορείτε να χρησιμοποιήσετε δύο διαφορετικά πιστοποιητικά, ένα πρωτεύον και δευτερεύον για αναβάθμιση. Ορίστε την αποτύπωση του πρωτεύοντος πιστοποιητικού στην ενότητα **αποτύπωση** και από τη δευτερεύουσα στο τις μεταβλητές **ThumbprintSecondary** .|
|ServerCertificate|Αυτό το πιστοποιητικό παρουσιάζονται στον υπολογιστή-πελάτη όταν προσπαθεί να συνδεθείτε με αυτό το σύμπλεγμα. Για τη διευκόλυνσή σας, μπορείτε να επιλέξετε να χρησιμοποιήσετε το ίδιο πιστοποιητικό για *ClusterCertificate* και *ServerCertificate*. Μπορείτε να χρησιμοποιήσετε δύο πιστοποιητικά διαφορετικό διακομιστή, μια κύρια και μια δευτερεύουσα για αναβάθμιση. Ορίστε την αποτύπωση του πρωτεύοντος πιστοποιητικού στην ενότητα **αποτύπωση** και από τη δευτερεύουσα στο τις μεταβλητές **ThumbprintSecondary** . |
|ClientCertificateThumbprints|Αυτό είναι ένα σύνολο πιστοποιητικά που θέλετε να εγκαταστήσετε το στους υπολογιστές-πελάτες με έλεγχο ταυτότητας. Μπορείτε να έχετε έναν αριθμό εγκατεστημένο στους υπολογιστές που θέλετε να επιτρέψετε την πρόσβαση στο σύμπλεγμα πιστοποιητικών διαφορετικό πρόγραμμα-πελάτη. Ορίστε την αποτύπωση κάθε πιστοποιητικού στη μεταβλητή **CertificateThumbprint** . Εάν ορίσετε το **IsAdmin** στην *τιμή true*, στη συνέχεια, το πρόγραμμα-πελάτη με αυτό το πιστοποιητικό εγκατεστημένο το να το κάνετε διαχειριστή δραστηριότητες διαχείρισης στο σύμπλεγμα. Εάν το **IsAdmin** είναι *false*, το πρόγραμμα-πελάτη με αυτό το πιστοποιητικό να εκτελέσετε μόνο τις ενέργειες που επιτρέπονται για δικαιώματα πρόσβασης του χρήστη, συνήθως μόνο για ανάγνωση. Για περισσότερες πληροφορίες σχετικά με τους ρόλους, διαβάστε [(RBAC) ο έλεγχος πρόσβασης βάσει ρόλων](service-fabric-cluster-security.md/#role-based-access-control-rbac)  |
|ClientCertificateCommonNames|Ορίστε το κοινό όνομα του πρώτου πιστοποιητικού προγράμματος-πελάτη για το **CertificateCommonName**. Το **CertificateIssuerThumbprint** είναι την αποτύπωση για τον εκδότη αυτού του πιστοποιητικού. Διαβάστε [εργασία με τα πιστοποιητικά](https://msdn.microsoft.com/library/ms731899.aspx) για να μάθετε περισσότερα σχετικά με τις κοινές ονόματα και τον εκδότη.|
|HttpApplicationGatewayCertificate|Αυτό είναι ένα προαιρετικό πιστοποιητικό που μπορεί να καθορίσει εάν που θέλετε να διασφαλίσετε την πύλη εφαρμογής Http. Βεβαιωθείτε ότι έχει οριστεί reverseProxyEndpointPort στο nodeTypes Εάν χρησιμοποιείτε αυτό το πιστοποιητικό.|

Ακολουθεί η ρύθμιση παραμέτρων του συμπλέγματος παράδειγμα όπου έχει παραχωρηθεί τα πιστοποιητικά σύμπλεγμα διακομιστή και προγράμματος-πελάτη.

 ```
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
      "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
      "iPAddress": "10.7.0.6",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpoint": "19001",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="aquire-the-x509-certificates"></a>Λήψη X.509 πιστοποιητικών
Για να ασφαλίσετε επικοινωνίας μέσα στο σύμπλεγμα, θα πρέπει πρώτα να αποκτήσετε πιστοποιητικά X.509 για το σύμπλεγμα κόμβους. Επιπλέον, για να περιορίσετε τη σύνδεση με αυτό το σύμπλεγμα σε εξουσιοδοτημένους μηχανές/χρήστες, θα πρέπει να αποκτήσετε και να εγκαταστήσετε τα πιστοποιητικά για τα προγράμματα-πελάτες.

Για συμπλεγμάτων που εκτελούνται παραγωγής φόρτους εργασίας, θα πρέπει να χρησιμοποιήσετε μια [Αρχή έκδοσης πιστοποιητικών (CA)](https://en.wikipedia.org/wiki/Certificate_authority) υπογεγραμμένο πιστοποιητικό X.509 για την ασφάλιση του συμπλέγματος. Για λεπτομέρειες σχετικά με την απόκτηση αυτά τα πιστοποιητικά, ανατρέξτε στο θέμα [Τρόπος: προμηθευτείτε ένα πιστοποιητικό](http://msdn.microsoft.com/library/aa702761.aspx).

Για συμπλεγμάτων που χρησιμοποιείτε για σκοπούς δοκιμής, μπορείτε να επιλέξετε να χρησιμοποιήσετε ένα αυτο-υπογεγραμμένο πιστοποιητικό.

## <a name="optional-create-a-self-signed-certificate"></a>Προαιρετικά: Δημιουργήστε ένα πιστοποιητικό αυτόματης υπογραφής
Ένας τρόπος για να δημιουργήσετε ένα αυτο-υπογεγραμμένου πιστοποιητικού που μπορείτε να προστατεύσετε τα σωστά είναι να χρησιμοποιήσετε τη δέσμη ενεργειών *CertSetup.ps1* στο φάκελο SDK ύφασμα υπηρεσίας στον κατάλογο *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*. Επεξεργαστείτε αυτό το αρχείο και χρησιμοποιήστε αυτήν την επιλογή για να δημιουργήσετε ένα πιστοποιητικό με ένα κατάλληλο όνομα.

Τώρα μπορείτε να εξαγάγετε το πιστοποιητικό σε ένα αρχείο PFX με προστατευμένο κωδικό πρόσβασης. Πρώτα πρέπει να λάβετε την αποτύπωση του πιστοποιητικού. Εκτελέστε την εφαρμογή certmgr.exe. Μεταβείτε στο φάκελο **Τοπικό Computer\Personal** και βρείτε το πιστοποιητικό που μόλις δημιουργήσατε. Κάντε διπλό κλικ το πιστοποιητικό για να ανοίξει το μήνυμα, επιλέξτε την καρτέλα " *Λεπτομέρειες* " και κάντε κύλιση προς τα κάτω, στο πεδίο *αποτύπωση* . Αντιγράψτε την τιμή αποτύπωση σε την εντολή PowerShell παρακάτω, καταργώντας τα κενά διαστήματα.  Αλλάξτε την τιμή *$pswd* σε έναν κωδικό πρόσβασης καταλληλότητα ασφαλούς να προστατεύσετε και να εκτελέσετε το PowerShell:

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

Για να δείτε τις λεπτομέρειες του πιστοποιητικού εγκατεστημένο στον υπολογιστή, μπορείτε να εκτελέσετε την ακόλουθη εντολή PowerShell:

```
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Εναλλακτικά, εάν έχετε μια συνδρομή του Azure, ακολουθήστε την ενότητα [Προσθήκη πιστοποιητικών θάλαμο αριθμού-κλειδιού](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>Εγκαταστήστε τα πιστοποιητικά
Όταν έχετε πιστοποιητικά, μπορείτε να τα εγκαταστήσετε σε τους κόμβους συμπλέγματος. Οι κόμβοι σας πρέπει να έχουν την πιο πρόσφατη Windows PowerShell 3.x εγκατασταθεί σε αυτά. Θα πρέπει να επαναλάβετε αυτά τα βήματα σε κάθε κόμβο, για το σύμπλεγμα και Server πιστοποιητικά και οποιαδήποτε δευτερεύοντα πιστοποιητικά.

1. Αντιγράψτε το αρχείο .pfx ή τα αρχεία στον κόμβο.
2. Ανοίξτε ένα παράθυρο του PowerShell ως διαχειριστής και εισαγάγετε τις παρακάτω εντολές. Αντικαταστήστε το *$pswd* με τον κωδικό πρόσβασης που χρησιμοποιήσατε για να δημιουργήσετε αυτό το πιστοποιητικό. Αντικαταστήστε το *$PfxFilePath* με την πλήρη διαδρομή της το .pfx αντιγραφεί σε αυτόν τον κόμβο.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Στη συνέχεια, πρέπει να ορίσετε τον έλεγχο πρόσβασης σε αυτό το πιστοποιητικό, ώστε να διαδικασίας ύφασμα υπηρεσίας, η οποία εκτελείται στο λογαριασμό υπηρεσίας δικτύου, να το χρησιμοποιήσετε, εκτελώντας την ακόλουθη δέσμη ενεργειών. Δώστε την αποτύπωση του πιστοποιητικού και "ΥΠΗΡΕΣΊΑ ΔΙΚΤΎΟΥ" για το λογαριασμό της υπηρεσίας. Μπορείτε να ελέγξετε ότι οι ACL στο πιστοποιητικό είναι σωστές, χρησιμοποιώντας το εργαλείο certmgr.exe και να βλέπετε τα ιδιωτικά κλειδιά διαχείριση στη του πιστοποιητικού.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Repeat the steps above for each server certificate. You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.

## Create the secure cluster
After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster. Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster. For example, your command might look like the following:

```
.\CreateServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab - AcceptEULA $true
```

Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it. For example:

```
Σύνδεση ServiceFabricCluster - ConnectionEndpoint 10.7.0.4:19000 - KeepAliveIntervalInSec 10 - X509Credential - ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 - FindType FindByThumbprint - FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 - StoreLocation CurrentUser - StoreName μου
```

If you are logged on to one of the machines in the cluster, since this already has the certificate installed locally you can simply run the Powershell command to connect to the cluster and show a list of nodes:

```
Σύνδεση ServiceFabricCluster Get-ServiceFabricNode
```
To remove the cluster call the following command:

```
.\RemoveServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab
```
