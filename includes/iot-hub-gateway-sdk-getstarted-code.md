## <a name="typical-output"></a>Η τυπική έξοδος

Ακολουθεί ένα παράδειγμα της εξόδου που εγγράφονται στο αρχείο καταγραφής από το δείγμα Γεια. Έχουν προστεθεί χαρακτήρες νέας γραμμής και καρτέλα για αναγνωσιμότητα:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Τμήματα κώδικα

Αυτή η ενότητα περιγράφει ορισμένα βασικά τμήματα του κώδικα στο δείγμα Γεια.

### <a name="gateway-creation"></a>Δημιουργία πύλης

Ο προγραμματιστής πρέπει να συντάξετε τη *διαδικασία πύλη*. Αυτό το πρόγραμμα δημιουργεί την εσωτερική υποδομή (broker), φορτώνει τις λειτουργικές μονάδες και ρυθμίζει όλα τα στοιχεία για να λειτουργήσουν σωστά. Το SDK παρέχει τη συνάρτηση **Gateway_Create_From_JSON** για να μπορέσετε να γίνεται εκκίνηση μιας πύλης από ένα αρχείο JSON. Για να χρησιμοποιήσετε τη συνάρτηση **Gateway_Create_From_JSON** , πρέπει να μεταβιβάσετε τη διαδρομή σε ένα αρχείο JSON που καθορίζει τις λειτουργικές μονάδες για να φορτώσετε. 

Μπορείτε να βρείτε τον κωδικό για τη διαδικασία της πύλης του δείγματος Γεια σε το [main.c] [ lnk-main-c] αρχείο. Για αναγνωσιμότητα, το παρακάτω τμήμα κώδικα δείχνει μια συντομευμένη εκδοχή του κώδικα διαδικασίας πύλη. Αυτό το πρόγραμμα δημιουργεί μια πύλη και, στη συνέχεια, περιμένει το χρήστη να πατήσει το πλήκτρο **ENTER** πριν από τη δημιουργία προς τα κάτω στην πύλη. 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

Το αρχείο ρυθμίσεων JSON περιέχει μια λίστα των λειτουργικών μονάδων φόρτωσης. Κάθε λειτουργική μονάδα, πρέπει να καθορίσετε α:

- **module_name**: ένα μοναδικό όνομα για τη λειτουργική μονάδα.
- **module_path**: η διαδρομή προς τη βιβλιοθήκη που περιέχει τη λειτουργική μονάδα. Για το Linux είναι ένα αρχείο .so, στα Windows, αυτό είναι ένα αρχείο .dll.
- **args**: οι πληροφορίες ρύθμισης παραμέτρων που χρειάζεται τη λειτουργική μονάδα.

Το αρχείο JSON περιέχει επίσης τις συνδέσεις μεταξύ των λειτουργικών μονάδων που θα μεταβιβαστούν broker. Μια σύνδεση έχει δύο ιδιότητες:
- **προέλευση**: ένα όνομα λειτουργικής μονάδας από την `modules` ενότητα, ή "\*".
- **δέκτης**: ένα όνομα λειτουργικής μονάδας από την `modules` ενότητα

Κάθε σύνδεση ορίζει μια διαδρομή του μηνύματος και την κατεύθυνση. Μηνύματα από τη λειτουργική μονάδα `source` θα παραδοθούν στη λειτουργική μονάδα `sink`. Το `source` μπορεί να οριστεί ως "\*", που δηλώνει ότι τα μηνύματα από οποιαδήποτε λειτουργική μονάδα θα ληφθεί από `sink`.

Το ακόλουθο δείγμα εμφανίζει το αρχείο ρυθμίσεων JSON που χρησιμοποιείται για τη ρύθμιση του δείγματος Γεια σε Linux. Κάθε μήνυμα που παράγονται από λειτουργική μονάδα `hello_world` θα καταναλωθεί από λειτουργική μονάδα `logger`. Αν μια λειτουργική μονάδα απαιτεί όρισμα εξαρτάται από το σχεδιασμό της λειτουργικής μονάδας. Σε αυτό το παράδειγμα, η λειτουργική μονάδα καταγραφής δέχεται ένα όρισμα που είναι η διαδρομή προς το αρχείο εξόδου και τη λειτουργική μονάδα Γεια δεν δέχεται ορίσματα:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Hello World λειτουργική μονάδα μήνυμα δημοσίευσης

Μπορείτε να βρείτε τον κωδικό που χρησιμοποιείται από τη λειτουργική μονάδα "hello world" για να δημοσιεύσετε μηνύματα σε το ['hello_world.c'] [ lnk-helloworld-c] αρχείο. Το παρακάτω τμήμα κώδικα δείχνει μια τροποποιημένη έκδοση με πρόσθετα σχόλια και κάποιο σφάλμα χειρισμού κώδικα για αναγνωσιμότητα:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Επεξεργασία μηνυμάτων του Hello World λειτουργική μονάδα

Η λειτουργική μονάδα Γεια ποτέ πρέπει να επεξεργαστούν τα μηνύματα που θα δημοσιεύσετε το broker άλλες λειτουργικές μονάδες. Με αυτόν τον τρόπο εφαρμογής της επιστροφής κλήσης μήνυμα στη λειτουργική μονάδα Γεια μια συνάρτηση μη λειτουργική.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Μήνυμα λειτουργική μονάδα καταγραφής δημοσίευσης και της επεξεργασίας

Η λειτουργική μονάδα καταγραφής λαμβάνει μηνύματα από το μεσίτη και τις γράφει σε ένα αρχείο. Δημοσιεύει ποτέ τα μηνύματα. Επομένως, ο κωδικός της λειτουργικής μονάδας καταγραφής ποτέ καλεί τη συνάρτηση **Broker_Publish** .

Η συνάρτηση **Logger_Recieve** στο το [logger.c] [ lnk-logger-c] αρχείο είναι η επιστροφή κλήσης που καλεί το broker για την παράδοση μηνυμάτων στη λειτουργική μονάδα καταγραφής. Το παρακάτω τμήμα κώδικα δείχνει μια τροποποιημένη έκδοση με πρόσθετα σχόλια και κάποιο σφάλμα χειρισμού κώδικα για αναγνωσιμότητα:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε σχετικά με τη χρήση του SDK πύλη IoT, ανατρέξτε στα παρακάτω θέματα:

- [Πύλη IoT SDK – αποστολή μηνυμάτων συσκευή-σύννεφο με μια συσκευή προσομοίωσης χρησιμοποιεί Linux][lnk-gateway-simulated].
- [Πύλη IoT Azure SDK] [ lnk-gateway-sdk] σε GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md