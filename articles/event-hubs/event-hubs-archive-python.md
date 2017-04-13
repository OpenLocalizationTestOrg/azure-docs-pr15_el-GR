<properties
    pageTitle="Azure αναλυτικές οδηγίες αρχειοθέτησης διανομείς συμβάν | Microsoft Azure"
    description="Δείγμα που χρησιμοποιεί το SDK Python Azure για μια επίδειξη με χρήση της δυνατότητας αρχειοθέτησης διανομείς συμβάν."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="event-hubs-archive-walkthrough-python"></a>Αναλυτικές οδηγίες αρχειοθέτησης διανομείς συμβάν: Python

Αρχειοθέτηση διανομείς συμβάν είναι μια νέα δυνατότητα του συμβάντος διανομείς που σας επιτρέπει να παρέχουν αυτόματα τα δεδομένα ροής στο κέντρο σας συμβάν σε ένα λογαριασμό χώρο αποθήκευσης Blob του Azure της επιλογής σας. Αυτό καθιστά εύκολη για να εκτελέσετε μαζική επεξεργασία στη ροή δεδομένα σε πραγματικό χρόνο. Αυτό το άρθρο περιγράφει τον τρόπο χρήσης του συμβάντος διανομείς αρχειοθέτηση με Python. Για περισσότερες πληροφορίες σχετικά με το συμβάν διανομείς αρχειοθέτησης, ανατρέξτε στο [άρθρο Επισκόπηση](event-hubs-archive-overview.md).

Αυτό το δείγμα χρησιμοποιεί το SDK Python Azure για μια επίδειξη χρησιμοποιώντας τη δυνατότητα αρχειοθέτησης. Το sender.py αποστέλλει προσομοιωμένη περιβαλλοντικό τηλεμετρίας διανομείς συμβάντος σε μορφή JSON. Ο διανομέας συμβάν έχει ρυθμιστεί ώστε να χρησιμοποιήσετε τη δυνατότητα "Αρχειοθέτηση" για να γράψετε αντικειμένων blob χώρου αποθήκευσης σε δέσμες αυτών των δεδομένων. Το archivereader.py, στη συνέχεια, διαβάζει τα παρακάτω αντικείμενα BLOB και δημιουργεί ένα αρχείο προσάρτησης ανά συσκευή και εγγράφει τα δεδομένα σε αρχεία .csv.

Τι θα πραγματοποιηθεί

1.  Δημιουργήστε ένα λογαριασμό χώρο αποθήκευσης Blob του Azure και ένα κοντέινερ αντικειμένων blob μέσα σε αυτό, με την πύλη Azure

2.  Δημιουργήστε ένα χώρο ονομάτων συμβάν διανομέα, με την πύλη Azure

3.  Δημιουργήστε ένα συμβάν διανομέα με τη δυνατότητα αρχειοθέτησης ενεργοποιημένη, με την πύλη Azure

4.  Αποστολή δεδομένων στο συμβάν διανομέα με μια δέσμη ενεργειών Python

5.  Διαβάστε τα αρχεία από το αρχείο αρχειοθέτησης και να τις επεξεργαστείτε με μια άλλη δέσμη ενεργειών Python

Προαπαιτούμενα στοιχεία

1.  Python 2.7.x

2.  Μια συνδρομή του Azure

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Δημιουργήστε ένα λογαριασμό αποθήκευσης Azure

1.  Συνδεθείτε στην [πύλη του Azure][].

2.  Στο αριστερό παράθυρο περιήγησης της πύλης, κάντε κλικ στην επιλογή Δημιουργία, στη συνέχεια, κάντε κλικ στην επιλογή δεδομένα + χώρος αποθήκευσης και, στη συνέχεια, κάντε κλικ στο λογαριασμό του χώρου αποθήκευσης.

3.  Συμπληρώστε τα πεδία του blade λογαριασμού χώρου αποθήκευσης και κάντε κλικ στην επιλογή **Δημιουργία**.

    ![][1]

4.  Όταν δείτε το μήνυμα **Αναπτύξεις ολοκληρώθηκε με επιτυχία** , κάντε κλικ στο κουμπί του νέου λογαριασμού χώρου αποθήκευσης και το blade **βασικά στοιχεία** , κάντε κλικ στην επιλογή **αντικείμενα BLOB**. Όταν ανοίξει το blade **Blob υπηρεσίας** , κάντε κλικ στο κουμπί **+ κοντέινερ** στο επάνω μέρος. Ονομάστε το κοντέινερ **αρχειοθέτησης**και, στη συνέχεια, κλείστε το blade **αντικειμένων Blob υπηρεσίας** .

5.  Κάντε κλικ στην επιλογή **πλήκτρα πρόσβασης** στο το αριστερό blade και αντιγράψτε το όνομα του λογαριασμού χώρου αποθήκευσης και την τιμή της **κλειδί1**. Αποθηκεύστε αυτές τις τιμές στο Σημειωματάριο ή σε κάποια άλλη θέση προσωρινό.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Δημιουργία δέσμης ενεργειών Python για να στείλετε τα συμβάντα σας διανομέα συμβάν

1.  Ανοίξτε τις αγαπημένες πρόγραμμα επεξεργασίας Python, όπως [Κώδικα Visual Studio][].

2.  Δημιουργήστε μια δέσμη ενεργειών που ονομάζεται **sender.py**. Αυτή η δέσμη ενεργειών θα στείλει 200 συμβάντα σας διανομέα συμβάν. Είναι απλή περιβαλλοντικό ενδείξεις αποστέλλονται στο JSON.

3.  Επικολλήστε τον ακόλουθο κώδικα στο sender.py:

    ```
    import uuid
    import datetime
    import random
    import json
    from azure.servicebus import ServiceBusService
    
    sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    for y in range(0,20):
        for dev in devices:
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading)
            sbs.send\_event('myhub', s)
        print y
    ```
4.  Ενημερώστε τον παραπάνω κώδικα για να χρησιμοποιήσετε το όνομα του χώρου ονομάτων και τιμές κλειδιού που έχετε λάβει όταν δημιουργήσατε το χώρο ονομάτων συμβάν διανομείς.

## <a name="create-a-python-script-to-read-your-archive-files"></a>Δημιουργία δέσμης ενεργειών Python για να διαβάσετε τα αρχεία σας αρχειοθέτησης

1.  Συμπληρώστε την blade και κάντε κλικ στην επιλογή **Δημιουργία**.

2.  Δημιουργήστε μια δέσμη ενεργειών που ονομάζεται **archivereader.py**. Αυτή η δέσμη ενεργειών θα διαβάσει τα αρχεία αρχειοθέτησης και να δημιουργήσετε ένα αρχείο ανά συσκευή για την εγγραφή των δεδομένων μόνο για τη συγκεκριμένη συσκευή.

3.  Επικολλήστε τον ακόλουθο κώδικα στο archivereader.py:

    ```
    import os
    import string
    import json
    import avro.schema
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    from azure.storage.blob import BlockBlobService
    
    def processBlob(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed\_json = json.loads(reading["Body"])
            if not 'id' in parsed\_json:
                return
            if not dict.has\_key(parsed\_json['id']):
            list = []
            dict[parsed\_json['id']] = list
        else:
            list = dict[parsed\_json['id']]
            list.append(parsed\_json)
        reader.close()
        for device in dict.keys():
            deviceFile = open(device + '.csv', "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\\n')

    def startProcessing(accountName, key, container):
        print 'Processor started using path: ' + os.getcwd()
        block\_blob\_service = BlockBlobService(account\_name=accountName, account\_key=key)
        generator = block\_blob\_service.list\_blobs(container)
        for blob in generator:
            if blob.properties.content\_length != 0:
                print('Downloaded a non empty blob: ' + blob.name)
                cleanName = string.replace(blob.name, '/', '\_')
                block\_blob\_service.get\_blob\_to\_path(container, blob.name, cleanName)
                processBlob(cleanName)
                os.remove(cleanName)
            block\_blob\_service.delete\_blob(container, blob.name)
    startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
    ```

4.  Φροντίστε να επικολλήσετε τις κατάλληλες τιμές για το όνομα του λογαριασμού χώρου αποθήκευσης και το κλειδί στην κλήση για `startProcessing`.

## <a name="run-the-scripts"></a>Εκτελέστε τις δέσμες ενεργειών

1.  Ανοίξτε μια γραμμή εντολών που έχει Python της διαδρομής και, στη συνέχεια, εκτελέστε αυτές οι εντολές για να εγκαταστήσετε το Python προαπαιτούμενα πακέτα:

    ```
    pip install azure-storage
    pip install azure-servicebus
    pip install avro
    ```
  
    Εάν έχετε μια παλαιότερη έκδοση του είτε αποθήκευσης azure ή azure που ίσως χρειαστεί να χρησιμοποιήσετε το **--αναβαθμίσετε** επιλογή

    Ίσως χρειαστεί επίσης να εκτελέσετε τα παρακάτω (δεν είναι απαραίτητο στα περισσότερα συστήματα):

    ```
    pip install cryptography
    ```

2.  Αλλαγή του καταλόγου σας σε όπου έχετε αποθηκεύσει sender.py και archivereader.py και εκτέλεση αυτής της εντολής:

    ```
    start python sender.py
    ```
    
    Ξεκινά μια νέα διαδικασία Python για να εκτελέσετε τον αποστολέα.

3. Τώρα, περιμένετε μερικά λεπτά για την αρχειοθέτηση για να εκτελέσετε. Στη συνέχεια, πληκτρολογήστε την παρακάτω εντολή στο το αρχικό παράθυρο εντολών:

    ```
    python archivereader.py
    ```

Αυτού του επεξεργαστή αρχειοθέτησης χρησιμοποιεί τον τοπικό κατάλογο για να κάνετε λήψη όλων των blob του από το λογαριασμό/κοντέινερ χώρου αποθήκευσης. Αυτό θα επεξεργαστεί οποιαδήποτε που δεν είναι κενά και να καταγράψετε τα αποτελέσματα ως αρχεία .csv στον τοπικό κατάλογο.

## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να μάθετε περισσότερα σχετικά με το συμβάν διανομείς, επισκεφθείτε τις παρακάτω συνδέσεις:

- [Επισκόπηση των συμβάντων διανομείς αρχειοθέτησης][]
- Μια ολοκληρωμένη [δείγμα εφαρμογής που χρησιμοποιεί το συμβάν διανομείς][].
- Το δείγμα [κλίμακα ανάληψη συμβάν επεξεργασίας με διανομείς συμβάν][] .
- [Επισκόπηση διανομείς συμβάντων][]
 

[Πύλη του Azure]: https://portal.azure.com/
[Επισκόπηση των διανομείς συμβάν αρχειοθέτηση]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/
[Κώδικα Visual Studio]: https://code.visualstudio.com/
[Επισκόπηση διανομείς συμβάντων]: event-hubs-overview.md
[δείγμα εφαρμογής που χρησιμοποιεί το συμβάν διανομείς]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Επεξεργασία συμβάντος με διανομείς συμβάν διαβάθμιση]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
