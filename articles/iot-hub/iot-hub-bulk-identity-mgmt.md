<properties
 pageTitle="Εισαγωγή/Εξαγωγή ταυτοτήτων συσκευή διανομέα IoT | Microsoft Azure"
 description="Έννοιες και .NET κωδικό τμήματα κώδικα για μαζική Διαχείριση ταυτοτήτων συσκευή IoT διανομέα"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>Μαζική διαχείρισης ταυτοτήτων συσκευή IoT διανομέα

Κάθε ενότητα IoT έχει ένα μητρώο ταυτότητα συσκευής που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε πόρων ανά συσκευής στην υπηρεσία, όπως μια ουρά που περιέχει εν πτήσει cloud-σε-συσκευή μηνύματα. Το μητρώο ταυτότητας συσκευή σάς επιτρέπει να ελέγχετε την πρόσβαση για τα τελικά σημεία άμεσα προσβάσιμη από τη συσκευή. Σε αυτό το άρθρο περιγράφει τον τρόπο εισαγωγής και εξαγωγής ταυτότητες συσκευή μαζικά προς και από ένα μητρώο ταυτότητας συσκευή.

Εισαγωγή και εξαγωγή λειτουργίες λάβει χώρα στο περιβάλλον των *εργασιών* που σας επιτρέπουν να εκτελεί λειτουργίες υπηρεσίας μαζικής σε σχέση με ένα διανομέα IoT.

Τάξη **RegistryManager** περιλαμβάνει τις **ExportDevicesAsync** και τις μεθόδους **ImportDevicesAsync** που χρησιμοποιούν το πλαίσιο **εργασίας** . Αυτές οι μέθοδοι σάς επιτρέπουν να εξαγωγή, εισαγωγή και συγχρονισμός το σύνολο της μια registry IoT διανομέα συσκευή.

## <a name="what-are-jobs"></a>Τι είναι οι εργασίες;

Συσκευή ταυτότητας registry λειτουργίες χρησιμοποιούν το σύστημα **εργασίας** κατά τη λειτουργία:

*  Έχει πιθανώς πολύ χρόνο εκτέλεσης σε σύγκριση με το τυπικό περιβάλλον εκτέλεσης λειτουργιών, ή
*  Επιστρέφει ένα μεγάλο όγκο δεδομένων για το χρήστη.

Σε αυτές τις περιπτώσεις, αντί για μία μόνο API κλήσης σε αναμονή ή εμποδίζουν το αποτέλεσμα της λειτουργίας, η λειτουργία δημιουργεί ασύγχρονα μια **εργασία** σε αυτόν το διανομέα IoT. Στη συνέχεια, η λειτουργία επιστρέφει αμέσως ένα αντικείμενο **JobProperties** .

Το παρακάτω C# τμήμα κώδικα δείχνει πώς μπορείτε να δημιουργήσετε μια εργασία Εξαγωγή:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

Στη συνέχεια, μπορείτε να χρησιμοποιήσετε την κλάση **RegistryManager** ερώτημα για την κατάσταση του **έργου** χρησιμοποιώντας το επιστρεφόμενο μετα-δεδομένων **JobProperties** .

Το παρακάτω C# τμήμα κώδικα δείχνει πώς μπορείτε να τις ψηφοφορίας κάθε πέντε δευτερόλεπτα για να δείτε εάν η εργασία έχει ολοκληρωθεί η εκτέλεση:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Εξαγωγή συσκευές

Χρησιμοποιήστε τη μέθοδο **ExportDevicesAsync** για να εξαγάγετε το σύνολο της ένα μητρώο IoT διανομέα συσκευή σε ένα κοντέινερ αντικειμένων blob [Αποθήκευσης Azure](https://azure.microsoft.com/documentation/services/storage/) χρησιμοποιώντας μια [Υπογραφή πρόσβασης σε κοινή χρήση](https://msdn.microsoft.com/library/ee395415.aspx).

Αυτή η μέθοδος σας επιτρέπει να δημιουργήσετε αξιόπιστη αντίγραφα ασφαλείας των πληροφοριών σας συσκευή σε ένα κοντέινερ αντικειμένων blob που μπορείτε να ελέγξετε.

Η μέθοδος **ExportDevicesAsync** απαιτεί δύο παραμέτρους:

*  Μια *συμβολοσειρά* που περιέχει ένα URI ενός κοντέινερ αντικειμένων blob. Σε αυτό το URI πρέπει να περιέχει ένα διακριτικό συσχετισμών Ασφαλείας που εκχωρεί δικαιώματα εγγραφής στο κοντέινερ. Η εργασία δημιουργεί ένα μπλοκ blob σε αυτό το κοντέινερ για να αποθηκεύσετε τη συσκευή αλληλουχίας εξαγωγή δεδομένων. Το διακριτικό συσχετισμών Ασφαλείας πρέπει να περιλαμβάνει τα παρακάτω δικαιώματα:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  *Boolean* που υποδεικνύει εάν θέλετε να εξαιρέσετε κλειδιά ελέγχου ταυτότητας από την εξαγωγή δεδομένων. Εάν **false**, έλεγχος ταυτότητας πλήκτρα περιλαμβάνονται στο εξαγωγή εξόδου Διαφορετικά, πλήκτρα εξάγονται ως **null**.

Το παρακάτω C# τμήμα κώδικα δείχνει πώς μπορείτε να ξεκινήσετε μια εργασία εξαγωγής που περιλαμβάνει πλήκτρα συσκευή ελέγχου ταυτότητας για την εξαγωγή δεδομένων και, στη συνέχεια, ψηφοφορία για ολοκλήρωση:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

Η εργασία αποθηκεύει τα αποτελέσματά του κοντέινερ παρεχόμενη blob ως blob μπλοκ με το όνομα **devices.txt**. Τα δεδομένα εξόδου αποτελείται από JSON σειριοποιηθεί συσκευή δεδομένα, με μία συσκευή ανά γραμμή.

Το παρακάτω παράδειγμα εμφανίζει τα δεδομένα εξόδου:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Εάν χρειάζεστε πρόσβαση σε αυτά τα δεδομένα στον κώδικα, μπορείτε εύκολα να αποσειριοποίηση αυτών των δεδομένων με την κλάση **ExportImportDevice** . Το παρακάτω C# τμήμα κώδικα δείχνει πώς μπορείτε να διαβάσετε πληροφορίες για τις συσκευές που έχει εξαχθεί προηγουμένως σε ένα μπλοκ blob:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  Μπορείτε επίσης να χρησιμοποιήσετε τη μέθοδο **GetDevicesAsync** της κλάσης **RegistryManager** για τη λήψη μια λίστα με τις συσκευές σας. Ωστόσο, αυτή η προσέγγιση έχει μια σκληρή καπέλο 1000 τον αριθμό των αντικειμένων συσκευής που επιστρέφονται. Η περίπτωση αναμενόμενο χρήσης για τη μέθοδο **GetDevicesAsync** είναι για σενάρια ανάπτυξης για να βοηθήσει τον εντοπισμό σφαλμάτων και δεν προτείνεται για την παραγωγή φόρτους εργασίας.

## <a name="import-devices"></a>Εισαγωγή συσκευές

Η μέθοδος **ImportDevicesAsync** στην τάξη **RegistryManager** σάς επιτρέπει να εκτελέσετε λειτουργίες μαζικής εισαγωγής και συγχρονισμού σε ένα μητρώο IoT διανομέα συσκευή. Όπως η μέθοδος **ExportDevicesAsync** , η μέθοδος **ImportDevicesAsync** χρησιμοποιεί το πλαίσιο **εργασίας** .

Να χειριστείτε χρησιμοποιώντας τη μέθοδο **ImportDevicesAsync** επειδή εκτός από την προμήθεια νέες συσκευές στο μητρώο την ταυτότητά σας συσκευή, μπορεί επίσης ενημέρωση και διαγραφή υπάρχουσες συσκευές.

> [AZURE.WARNING]  Δεν είναι δυνατή η αναίρεση μιας λειτουργίας εισαγωγής. Πάντα αντίγραφο ασφαλείας τα υπάρχοντα δεδομένα σας με τη μέθοδο **ExportDevicesAsync** στο άλλο κοντέινερ αντικειμένων blob πριν να κάνετε μαζικές αλλαγές στις μητρώο την ταυτότητά σας συσκευή.

Η μέθοδος **ImportDevicesAsync** λαμβάνει δύο παραμέτρους:

*  Μια *συμβολοσειρά* που περιέχει ένα URI ένα κοντέινερ [Azure χώρο αποθήκευσης](https://azure.microsoft.com/documentation/services/storage/) αντικειμένων blob για να χρησιμοποιήσετε ως *εισαγωγής* στο έργο. Σε αυτό το URI πρέπει να περιέχει ένα διακριτικό συσχετισμών Ασφαλείας που εκχωρεί πρόσβαση για ανάγνωση στο κοντέινερ. Αυτό το κοντέινερ πρέπει να περιέχει ένα αντικείμενο blob με το όνομα **devices.txt** που περιέχει τα δεδομένα σειριακή συσκευή για να εισαγάγετε στο μητρώο την ταυτότητά σας συσκευή. Η εισαγωγή δεδομένων πρέπει να περιέχει πληροφορίες συσκευής με την ίδια μορφή JSON που η εργασία **ExportImportDevice** χρησιμοποιεί όταν δημιουργεί ένα blob **devices.txt** . Το διακριτικό συσχετισμών Ασφαλείας πρέπει να περιλαμβάνει τα παρακάτω δικαιώματα:

    ```
    SharedAccessBlobPermissions.Read
    ```

*  Μια *συμβολοσειρά* που περιέχει ένα URI ένα κοντέινερ [Azure χώρο αποθήκευσης](https://azure.microsoft.com/documentation/services/storage/) αντικειμένων blob για να χρησιμοποιήσετε ως *αποτέλεσμα* από την εργασία. Η εργασία δημιουργεί ένα μπλοκ blob σε αυτό το κοντέινερ για την αποθήκευση πληροφοριών σφάλματος από την εισαγωγή ολοκληρωμένη **εργασία**. Το διακριτικό συσχετισμών Ασφαλείας πρέπει να περιλαμβάνει τα παρακάτω δικαιώματα:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  Τις δύο παραμέτρους μπορεί να οδηγεί στο ίδιο κοντέινερ αντικειμένων blob. Οι παράμετροι ξεχωριστή Ενεργοποίηση απλώς ελέγχετε περισσότερο τα δεδομένα σας με το κοντέινερ εξόδου απαιτεί επιπλέον δικαιώματα.

Το παρακάτω C# τμήμα κώδικα δείχνει πώς να ξεκινήσετε μια εργασία εισαγωγής:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Συμπεριφορά εισαγωγής

Μπορείτε να χρησιμοποιήσετε τη μέθοδο **ImportDevicesAsync** για να εκτελέσετε τις ακόλουθες λειτουργίες μαζικής στο μητρώο την ταυτότητά σας συσκευή:

-   Μαζική καταχώρηση των νέων συσκευών
-   Μαζική διαγραφές υπάρχουσες συσκευές
-   Μαζική αλλαγές κατάστασης (Ενεργοποίηση ή απενεργοποίηση συσκευές)
-   Μαζική ανάθεσης με τα νέα πλήκτρα ελέγχου ταυτότητας συσκευή
-   Αναδημιουργία μαζικής αυτόματα με τα πλήκτρα ελέγχου ταυτότητας συσκευή

Μπορείτε να εκτελέσετε οποιονδήποτε συνδυασμό από τις προηγούμενες λειτουργίες μέσα σε μια μεμονωμένη κλήση **ImportDevicesAsync** . Για παράδειγμα, καταχώρηση νέες συσκευές και διαγραφή ή ενημέρωση υπάρχουσες συσκευές την ίδια στιγμή. Όταν χρησιμοποιείται μαζί με τη μέθοδο **ExportDevicesAsync** , μπορείτε να μετεγκαταστήσετε πλήρως όλες τις συσκευές σας από μία ενότητα IoT σε κάποιον άλλο.

Χρησιμοποιήστε την ιδιότητα προαιρετικό **importMode** στα δεδομένα σειριοποίησης εισαγωγής για κάθε συσκευή για να ελέγξετε τη διαδικασία ανά-συσκευή εισαγωγής. Η ιδιότητα **importMode** περιλαμβάνει τις ακόλουθες επιλογές:

| importMode |  Περιγραφή |
| -------- | ----------- |
| **createOrUpdate** | Εάν δεν υπάρχει μια συσκευή με το καθορισμένο **αναγνωριστικό**, τότε που μόλις έχει καταχωρηθεί. <br/>Εάν η συσκευή υπάρχει ήδη, αντικαθίσταται υπάρχουσες πληροφορίες με τα δεδομένα εισόδου παρεχόμενη ανεξάρτητα από την τιμή **ETag** . |
| **Δημιουργία** | Εάν δεν υπάρχει μια συσκευή με το καθορισμένο **αναγνωριστικό**, τότε που μόλις έχει καταχωρηθεί. <br/>Εάν η συσκευή υπάρχει ήδη, σφάλμα εγγράφεται στο αρχείο καταγραφής. |
| **ενημέρωση** | Εάν υπάρχει ήδη μια συσκευή με το καθορισμένο **αναγνωριστικό**, υπάρχουσες πληροφορίες αντικαθίσταται με τα δεδομένα εισόδου που παρέχεται ανεξάρτητα από την τιμή **ETag** . <br/>Εάν η συσκευή δεν υπάρχει, σφάλμα εγγράφεται στο αρχείο καταγραφής. |
| **updateIfMatchETag** | Εάν υπάρχει ήδη μια συσκευή με το καθορισμένο **αναγνωριστικό**, υπάρχουσες πληροφορίες αντικαθίσταται με τα δεδομένα εισόδου που παρέχεται μόνο εάν υπάρχει μια αντιστοιχία **ETag** . <br/>Εάν η συσκευή δεν υπάρχει, σφάλμα εγγράφεται στο αρχείο καταγραφής. <br/>Εάν υπάρχει μια ασυμφωνία **ETag** , σφάλμα εγγράφεται στο αρχείο καταγραφής. |
| **createOrUpdateIfMatchETag** | Εάν δεν υπάρχει μια συσκευή με το καθορισμένο **αναγνωριστικό**, τότε που μόλις έχει καταχωρηθεί. <br/>Εάν η συσκευή υπάρχει ήδη, αντικαθίσταται υπάρχουσες πληροφορίες με τα δεδομένα εισόδου που παρέχεται μόνο εάν υπάρχει μια αντιστοιχία **ETag** . <br/>Εάν υπάρχει μια ασυμφωνία **ETag** , σφάλμα εγγράφεται στο αρχείο καταγραφής. |
| **Διαγραφή** | Εάν υπάρχει ήδη μια συσκευή με το καθορισμένο **αναγνωριστικό**, διαγράφεται ανεξάρτητα από την τιμή **ETag** . <br/>Εάν η συσκευή δεν υπάρχει, σφάλμα εγγράφεται στο αρχείο καταγραφής. |
| **deleteIfMatchETag** | Εάν υπάρχει ήδη μια συσκευή με το καθορισμένο **αναγνωριστικό**, διαγράφεται μόνο εάν υπάρχει μια αντιστοιχία **ETag** . Εάν η συσκευή δεν υπάρχει, σφάλμα εγγράφεται στο αρχείο καταγραφής. <br/>Εάν υπάρχει μια ασυμφωνία ETag, σφάλμα εγγράφεται στο αρχείο καταγραφής. |

> [AZURE.NOTE] Εάν τα δεδομένα σειριοποίησης δεν ρητά να ορίσετε μια σημαία **importMode** για μια συσκευή, από προεπιλογή **createOrUpdate** κατά τη λειτουργία εισαγωγής.

## <a name="import-devices-example--bulk-device-provisioning"></a>Εισαγωγή παράδειγμα συσκευές – μαζική προμήθεια συσκευή 

Το παρακάτω δείγμα κώδικα C# παρουσιάζει τον τρόπο για τη δημιουργία πολλών ταυτοτήτων συσκευή που:

- Συμπεριλάβετε κλειδιά ελέγχου ταυτότητας.
- Εγγραφή στοιχεία συσκευή σε ένα μπλοκ blob.
- Εισαγάγετε τις συσκευές στο μητρώο ταυτότητας συσκευή.

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Παράδειγμα συσκευές εισαγωγής – μαζικής διαγραφής

Το παρακάτω δείγμα κώδικα δείχνει πώς μπορείτε να διαγράψετε τις συσκευές που έχετε προσθέσει χρησιμοποιώντας το προηγούμενο παράδειγμα κώδικα:

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>Λήψη του κοντέινερ URI συσχετισμών Ασφαλείας


Το παρακάτω δείγμα κώδικα δείχνει πώς μπορείτε να δημιουργήσετε ένα [URI συσχετισμών Ασφαλείας](../storage/storage-dotnet-shared-access-signature-part-2.md) με ανάγνωση, εγγραφή και διαγραφή δικαιωμάτων για ένα κοντέινερ αντικειμένων blob:

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο μάθατε πώς να εκτελείτε μαζικές λειτουργίες σε σχέση με το μητρώο ταυτότητας συσκευή σε ένα διανομέα IoT. Ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα σχετικά με τη Διαχείριση Azure IoT διανομέα:

- [Χρήση μετρικά][lnk-metrics]
- [Λειτουργίες παρακολούθησης][lnk-monitor]

Για να εξερευνήσετε περαιτέρω τις δυνατότητες του IoT διανομέα, ανατρέξτε στα θέματα:

- [Οδηγός για προγραμματιστές][lnk-devguide]
- [Προσομοίωση μια συσκευή με το SDK IoT πύλης][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md