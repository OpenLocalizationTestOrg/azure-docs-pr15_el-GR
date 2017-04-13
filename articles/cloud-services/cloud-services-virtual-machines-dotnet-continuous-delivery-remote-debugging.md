<properties
    pageTitle="Ενεργοποίηση απομακρυσμένου εντοπισμού σφαλμάτων με συνεχή παράδοσης | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ενεργοποιήσετε τον εντοπισμό σφαλμάτων απομακρυσμένο κατά τη χρήση συνεχής παράδοσης για ανάπτυξη Azure"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Ενεργοποίηση της απομακρυσμένης εντοπισμού κατά τη χρήση συνεχής παράδοσης για να δημοσιεύσετε στο Azure

Μπορείτε να ενεργοποιήσετε απομακρυσμένου εντοπισμού σφαλμάτων στο Azure, για τις υπηρεσίες cloud ή εικονικές μηχανές, όταν χρησιμοποιείτε [συνεχής παράδοσης](cloud-services-dotnet-continuous-delivery.md) για να δημοσιεύσετε στο Azure, ακολουθώντας τα παρακάτω βήματα.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Ενεργοποίηση της απομακρυσμένης τον εντοπισμό σφαλμάτων για τις υπηρεσίες cloud

1. Στον τον παράγοντα Δόμηση, ρυθμίστε το αρχικό περιβάλλον για Azure όπως περιγράφεται στη [Δημιουργία γραμμής εντολών για το Azure](http://msdn.microsoft.com/library/hh535755.aspx).
2. Επειδή το περιβάλλον εκτέλεσης απομακρυσμένου εντοπισμού σφαλμάτων (msvsmon.exe) είναι απαραίτητη για το πακέτο, εγκαταστήστε τα [Εργαλεία απομακρυσμένης Visual Studio 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (ή τα [Εργαλεία απομακρυσμένης για το Microsoft Visual Studio 2013 ενημερωμένη έκδοση 5](https://www.microsoft.com/en-us/download/details.aspx?id=48156) Εάν χρησιμοποιείτε το Visual Studio 2013). Ως εναλλακτική λύση, μπορείτε να αντιγράψετε τα δυαδικά αρχεία απομακρυσμένου εντοπισμού σφαλμάτων από ένα σύστημα που έχει εγκατεστημένο το Visual Studio.
3. Δημιουργία πιστοποιητικού, όπως περιγράφεται στην [Επισκόπηση των πιστοποιητικών για τις υπηρεσίες Cloud Azure](cloud-services-certs-create.md). Διατήρηση των .pfx και RDP αποτύπωση πιστοποιητικού και αποστείλετε το πιστοποιητικό για την υπηρεσία cloud προορισμού.
4. Χρησιμοποιήστε τις ακόλουθες επιλογές στη γραμμή εντολών MSBuild για να δημιουργήσετε και να συσκευάσετε με απομακρυσμένου εντοπισμού σφαλμάτων με δυνατότητα. (Αντικαταστήστε πραγματικές διαδρομές στα αρχεία σας σύστημα και το project για τα στοιχεία σε αγκύλες γωνίας.)

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath`είναι η διαδρομή προς το φάκελο που περιέχει msvsmon.exe στο τα Εργαλεία απομακρυσμένης για το Visual Studio.
    `RemoteDebuggerConnectorVersion`είναι η έκδοση Azure SDK στην υπηρεσία cloud. Θα πρέπει να ταιριάζει επίσης την έκδοση εγκατασταθεί με το Visual Studio.

5. Δημοσίευση στην υπηρεσία cloud προορισμού, χρησιμοποιώντας το αρχείο πακέτου και .cscfg που δημιουργούνται στο προηγούμενο βήμα.
6. Εισαγάγετε το πιστοποιητικό (αρχείο .pfx) στον υπολογιστή που διαθέτει Visual Studio με Azure SDK για το .NET εγκατεστημένο. Βεβαιωθείτε ότι για την εισαγωγή του `CurrentUser\My` χώρος αποθήκευσης πιστοποιητικού, διαφορετικά συνδέονται με το πρόγραμμα εντοπισμού σφαλμάτων στο Visual Studio θα αποτύχει.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Ενεργοποίηση του απομακρυσμένου εντοπισμού σφαλμάτων για εικονικές μηχανές

1. Δημιουργήστε μια εικονική μηχανή Azure. Ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή εκτελούν τον Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md) ή [Δημιουργία και διαχείριση Azure εικονικές μηχανές στο Visual Studio](../virtual-machines/virtual-machines-windows-classic-manage-visual-studio.md).
2. Στη [Azure κλασική σελίδα της πύλης](http://go.microsoft.com/fwlink/p/?LinkID=269851), προβάλετε την εικονική μηχανή πίνακα εργαλείων για να δείτε την εικονική μηχανή **RDP ΑΠΟΤΎΠΩΣΗ ΠΙΣΤΟΠΟΙΗΤΙΚΟΎ**. Αυτή η τιμή χρησιμοποιείται για την `ServerThumbprint` τιμή της ρύθμισης επέκτασης.
3. Δημιουργία πιστοποιητικού προγράμματος-πελάτη, όπως περιγράφεται στην [Επισκόπηση των πιστοποιητικών για τις υπηρεσίες Cloud Azure](cloud-services-certs-create.md) (διατήρηση των .pfx και RDP αποτύπωση πιστοποιητικού).
4. Εγκατάσταση του Azure Powershell (έκδοση 0.7.4 ή νεότερη έκδοση) όπως περιγράφεται στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).
5. Εκτελέστε την ακόλουθη δέσμη ενεργειών για να ενεργοποιήσετε την επέκταση RemoteDebug. Αντικαταστήστε τις διαδρομές και τα προσωπικά δεδομένα με το δικό σας, όπως το όνομα της συνδρομής, όνομα υπηρεσίας και αποτύπωση.

    >[AZURE.NOTE] Αυτή η δέσμη ενεργειών έχει ρυθμιστεί για Visual Studio 2015. Εάν χρησιμοποιείτε το Visual Studio 2013, τροποποιήστε το `$referenceName` και `$extensionName` αναθέσεις παρακάτω, για να χρησιμοποιήσετε `RemoteDebugVS2013` (αντί για `RemoteDebugVS2015`).

    <pre>
   Προσθήκη AzureAccount

    Επιλέξτε-AzureSubscription "Συνδρομή μου στο Microsoft"

    $vm = get-AzureVM - όνομα_υπηρεσίας "mytestvm1"-όνομα "mytestvm1"

    $endpoints = @( ,@{Name="RDConnVS2013"; PublicPort = 30400; PrivatePort = 30398} ,@{Name="RDFwdrVS2013"; PublicPort = 31400; PrivatePort = 31398})  

    foreach ($endpoint στο $endpoints) {Προσθήκη AzureEndpoint - Εικονική $vm-όνομα $endpoint. Δώστε ένα όνομα - πρωτόκολλο tcp - PublicPort $endpoint. PublicPort - Τοπική_θύρα $endpoint. PrivatePort}

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015" $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug" $extensionName = "RemoteDebugVS2015" $version = "1.*" $publicConfiguration = "<PublicConfig>true < /Connector.Enabled > < Connector.Enabled ><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Ορισμός AzureVMExtension `
    -ReferenceName $referenceName ` 
    -Publisher $publisher `
    -ExtensionName $extensionName ` 
    -έκδοση $version ' - PublicConfiguration $publicConfiguration

    foreach ($extension στο $vm. ΕΙΚΟΝΙΚΉ. ResourceExtensionReferences) {Εάν (($extension. Όνομα_αναφοράς - eq $referenceName) `
    -and ($extension.Publisher -eq $publisher) ` 
    - και ($extension. Δώστε ένα όνομα - eq $extensionName) '- και ($extension. Έκδοση - eq $version)) {$extension. ResourceExtensionParameterValues [0]. Πλήκτρο = 'config.txt' Αλλαγή}}

    $vm | Ενημέρωση AzureVM </pre>

6. Εισαγάγετε το πιστοποιητικό (.pfx) στον υπολογιστή που διαθέτει Visual Studio με Azure SDK για το .NET εγκατεστημένο.
