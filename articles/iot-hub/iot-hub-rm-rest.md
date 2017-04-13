<properties
    pageTitle="Δημιουργήστε ένα διανομέα IoT χρησιμοποιώντας το REST API | Microsoft Azure"
    description="Παρακολουθήστε αυτήν την εκμάθηση για να ξεκινήσετε να χρησιμοποιείτε το REST API για να δημιουργήσετε ένα διανομέα IoT."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-create-an-iot-hub-using-a-c-program-and-the-rest-api"></a>Πρόγραμμα εκμάθησης: Δημιουργία ενός διανομέα IoT, χρησιμοποιώντας ένα πρόγραμμα C# και το REST API

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Εισαγωγή

Μπορείτε να χρησιμοποιήσετε την υπηρεσία [παροχής πόρων διανομέα IoT REST API] [ lnk-rest-api] για να δημιουργήσετε και να διαχειριστείτε διανομείς Azure IoT μέσω προγραμματισμού. Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία παροχής πόρων διανομέα IoT REST API για να δημιουργήσετε ένα διανομέα IoT από ένα πρόγραμμα C#.

> [AZURE.NOTE] Azure έχει δύο διαφορετικές ανάπτυξης μοντέλα για τη δημιουργία και εργασία με πόρους: [Διαχείριση πόρων Azure και κλασική](../resource-manager-deployment-model.md).  Σε αυτό το άρθρο καλύπτει χρησιμοποιώντας το μοντέλο ανάπτυξης Azure διαχείριση πόρων.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

- Microsoft Visual Studio 2015.
- Λογαριασμού Azure active. <br/>Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] ή νεότερη έκδοση.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Προετοιμασία το έργο Visual Studio

1. Στο Visual Studio, δημιουργήστε ένα έργο Visual C# Windows χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Το όνομα του έργου **CreateIoTHubREST**.

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο σας και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.

3. Στο Nuget πακέτο Manager, επιλέξτε **περιλαμβάνουν προέκδοση**και αναζητήστε το **Microsoft.Azure.Management.ResourceManager**. Κάντε κλικ στην επιλογή **εγκατάσταση**, **Αναθεώρηση** αλλαγών, κάντε κλικ στο κουμπί **OK**και κατόπιν κάντε κλικ στην επιλογή **να αποδοχή** για να αποδεχτείτε τις άδειες χρήσης.

4. Στο Nuget διαχείρισης πακέτου, πραγματοποιήστε αναζήτηση για **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Κάντε κλικ στην επιλογή **εγκατάσταση**, **Αναθεώρηση** αλλαγών, κάντε κλικ στο κουμπί **OK**και, στη συνέχεια, κάντε κλικ στην επιλογή **να αποδοχή** για να αποδεχτείτε την άδεια χρήσης.

6. Στο Program.cs, αντικαταστήστε το υπάρχον δηλώσεις **χρησιμοποιώντας** με τα εξής:

    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
    
7. Στο Program.cs, προσθέστε τις ακόλουθες μεταβλητές στατική αντικαθιστώντας τις τιμές του πλαισίου κράτησης θέσης. Κάνατε μια Σημείωση **αναγνωριστικά εφαρμογής**, **SubscriptionId**, **TenantId**και **τον κωδικό πρόσβασης** σε αυτό το εκπαιδευτικό μάθημα. **Όνομα ομάδας πόρων** είναι το όνομα της ομάδας πόρων που χρησιμοποιείτε όταν δημιουργείτε την ενότητα IoT, μπορεί να είναι μια ήδη υπάρχουσα ομάδα πόρων ή ένα νέο. **Ενότητα IoT όνομα** είναι το όνομα του διανομέα IoT που δημιουργείτε, όπως **MyIoTHub** (αυτό το όνομα πρέπει να είναι μοναδικό καθολικό, έτσι θα πρέπει να περιλαμβάνει το όνομα ή τα αρχικά). **Ανάπτυξη όνομα** είναι ένα όνομα για την ανάπτυξη, όπως **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-rest-api-to-create-an-iot-hub"></a>Χρησιμοποιήστε το REST API για να δημιουργήσετε ένα διανομέα IoT

Χρησιμοποιήστε το [IoT διανομέα REST API] [ lnk-rest-api] για να δημιουργήσετε ένα διανομέα IoT στο σας ομάδα πόρων. Μπορείτε επίσης να χρησιμοποιήσετε το REST API για να κάνετε αλλαγές σε ένα υπάρχον διανομέα IoT.

1. Προσθέστε την ακόλουθη μέθοδο Program.cs:
    
    ```
    static void CreateIoTHub(string token)
    {
        
    }
    ```

2. Προσθέστε τον ακόλουθο κώδικα στη μέθοδο **CreateIoTHub** για να δημιουργήσετε ένα αντικείμενο **HttpClient** με το διακριτικό ελέγχου ταυτότητας των κεφαλίδων:

    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Προσθέστε τον ακόλουθο κώδικα στη μέθοδο **CreateIoTHub** για να περιγράψετε την ενότητα IoT για να δημιουργήσετε και να δημιουργήσετε μια αναπαράσταση JSON (για την τρέχουσα λίστα θέσεων που υποστηρίζουν διανομέα IoT δουν [Την κατάσταση Azure][lnk-status]):

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. Προσθέστε τον ακόλουθο κώδικα για τη μέθοδο **CreateIoTHub** για να υποβάλετε αίτηση για τα ΥΠΌΛΟΙΠΑ σε Azure, ελέγξτε την απάντηση και ανάκτηση της διεύθυνσης URL που μπορείτε να χρησιμοποιήσετε για την παρακολούθηση της κατάστασης της εργασίας ανάπτυξης:

    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
      
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
    
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. Προσθέστε τον ακόλουθο κώδικα στο τέλος της μεθόδου **CreateIoTHub** να χρησιμοποιήσετε τη διεύθυνση **asyncStatusUri** ανακτήσατε στο προηγούμενο βήμα για να περιμένετε για την ανάπτυξη για να ολοκληρώσετε:

    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Προσθέστε τον ακόλουθο κώδικα στο τέλος της μεθόδου **CreateIoTHub** για να ανακτήσετε τα πλήκτρα του διανομέα IoT που δημιουργήσατε και να τις εκτυπώνετε στην κονσόλα:

    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2015-08-15-preview", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
    
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```
    
## <a name="complete-and-run-the-application"></a>Ολοκλήρωση και εκτελέστε την εφαρμογή

Μπορείτε τώρα να ολοκληρώσετε την εφαρμογή, καλώντας τη μέθοδο **CreateIoTHub** πριν δημιουργήσετε και εκτελέστε το.

1. Προσθέστε τον ακόλουθο κώδικα στο τέλος της μεθόδου **κύριες** :

    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
    
2. Κάντε κλικ στην επιλογή **Δημιουργία** και, στη συνέχεια, να **δημιουργήσετε λύση**. Διορθώστε τυχόν σφάλματα.

3. Κάντε κλικ στην επιλογή **Εντοπισμός σφαλμάτων** και, στη συνέχεια, **Ξεκινήστε τον εντοπισμό σφαλμάτων** για να εκτελέσετε την εφαρμογή. Ενδέχεται να χρειαστούν αρκετά λεπτά για την ανάπτυξη για να εκτελέσετε.

4. Μπορείτε να επαληθεύσετε ότι η εφαρμογή σας προσθέσει ο νέος διανομέας IoT με την επίσκεψή σας στην [πύλη του Azure] [ lnk-azure-portal] και προβολή της λίστας των πόρων ή χρησιμοποιώντας το cmdlet **Get-AzureRmResource** PowerShell.

> [AZURE.NOTE] Αυτή η εφαρμογή παράδειγμα προσθέτει ένα διανομέα IoT τυπική S1 για την οποία θα γίνει χρέωση. Όταν ολοκληρώσετε, μπορείτε να διαγράψετε την ενότητα IoT μέσω της [πύλης Azure] [ lnk-azure-portal] ή χρησιμοποιώντας το cmdlet του PowerShell **Κατάργηση AzureRmResource** όταν είστε έτοιμοι.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα έχετε αναπτύξει ένα διανομέα IoT χρησιμοποιώντας το REST API, ίσως θελήσετε να εξερευνήσετε περαιτέρω:

- Διαβάστε σχετικά με τις δυνατότητες της [υπηρεσίας παροχής πόρων διανομέα IoT REST API][lnk-rest-api].
- Διαβάστε την [Επισκόπηση της διαχείρισης πόρων Azure] [ lnk-azure-rm-overview] για να μάθετε περισσότερα σχετικά με τις δυνατότητες της διαχείρισης πόρων Azure.

Για να μάθετε περισσότερα σχετικά με την ανάπτυξη IoT διανομέα, ανατρέξτε στα παρακάτω:

- [Εισαγωγή στις C SDK][lnk-c-sdk]
- [Ο διανομέας IoT SDK][lnk-sdks]

Για να εξερευνήσετε περαιτέρω τις δυνατότητες του IoT διανομέα, ανατρέξτε στα θέματα:

- [Προσομοίωση μια συσκευή με το SDK IoT πύλης][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
