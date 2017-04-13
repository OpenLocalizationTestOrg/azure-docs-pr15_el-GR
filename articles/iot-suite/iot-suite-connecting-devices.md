<properties
   pageTitle="Συνδέστε μια συσκευή χρησιμοποιώντας C σε Windows | Microsoft Azure"
   description="Περιγράφει τον τρόπο για να συνδέσετε μια συσκευή για την οικογένεια προγραμμάτων IoT Azure προ-διαμορφωμένες απομακρυσμένο λύση παρακολούθησης με μια εφαρμογή που έχει συνταχθεί σε C που εκτελείται σε Windows."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Συνδέστε τη συσκευή με το απομακρυσμένο προδιαμορφωμένο λύση παρακολούθησης (Windows)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Δημιουργία μιας λύσης δείγμα C στα Windows

Ακολουθήστε τα παρακάτω βήματα που δείχνουν τον τρόπο χρήσης Visual Studio για να δημιουργήσετε μια εφαρμογή προγράμματος-πελάτη γραμμένο σε C που επικοινωνεί με τη λύση απομακρυσμένη παρακολούθηση προ-διαμορφωμένες.

Δημιουργία έργου starter στο Visual Studio 2015 και προσθέστε τα πακέτα NuGet διανομέα IoT συσκευή προγράμματος-πελάτη:

1. Στο Visual Studio 2015, δημιουργία εφαρμογής κονσόλας C χρησιμοποιώντας το πρότυπο Visual C++ **Εφαρμογή Win32 κονσόλας** . Το όνομα του έργου **RMDevice**.

2. Στη σελίδα **Ρυθμίσεις εφαρμογές** στον **Οδηγό εφαρμογή Win32**, βεβαιωθείτε ότι είναι επιλεγμένο **εφαρμογής κονσόλας** και καταργήστε την επιλογή **κεφαλίδα Precompiled** και **ελέγχει κύκλου ζωής ανάπτυξης ασφαλείας (SDL)**.

3. Στην **Εξερεύνηση λύσεων**, διαγράψτε τα αρχεία stdafx.h, targetver.h και stdafx.cpp.

4. Στην **Εξερεύνηση λύσεων**, μετονομάστε το αρχείο RMDevice.cpp σε RMDevice.c.

5. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο **RMDevice** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση NuGet πακέτων**. Κάντε κλικ στο κουμπί **Αναζήτηση**, στη συνέχεια, αναζητήστε και εγκαταστήστε τα ακόλουθα πακέτα NuGet στο έργο:

    - Microsoft.Azure.IoTHub.Serializer
    - Microsoft.Azure.IoTHub.IoTHubClient
    - Microsoft.Azure.IoTHub.HttpTransport

6. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο **RMDevice** και, στη συνέχεια, κάντε κλικ στην επιλογή **Ιδιότητες** για να ανοίξετε το παράθυρο διαλόγου **Σελίδες ιδιοτήτων** του έργου. Για λεπτομέρειες, ανατρέξτε στο θέμα [Ρύθμιση Visual C++ ιδιότητες του έργου][lnk-c-project-properties]. 

7. Κάντε κλικ στο φάκελο **πρόγραμμα σύνδεσης** και, στη συνέχεια, κάντε κλικ στη σελίδα ιδιοτήτων **εισαγωγής** .

8. Προσθήκη **crypt32.lib** στην ιδιότητα **Επιπλέον εξαρτήσεις** . Κάντε κλικ στο κουμπί **OK** και, στη συνέχεια, **OK** ξανά για να αποθηκεύσετε το έργο τιμές ιδιοτήτων.

## <a name="specify-the-behavior-of-the-iot-hub-device"></a>Καθορίστε τη συμπεριφορά της συσκευής IoT διανομέα

Οι βιβλιοθήκες προγράμματος-πελάτη IoT διανομέα Χρησιμοποιήστε ένα μοντέλο για να καθορίσετε τη μορφή των μηνυμάτων στη συσκευή σας στείλει IoT διανομέα και τις εντολές που λαμβάνει από διανομέα IoT.

1. Στο Visual Studio, ανοίξτε το αρχείο RMDevice.c. Αντικαταστήστε το υπάρχον `#include` δηλώσεις με τον ακόλουθο κώδικα:

    ```
    #include "iothubtransporthttp.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    ```

2. Προσθέστε το παρακάτω δηλώσεις μεταβλητών μετά το `#include` δηλώσεις. Αντικαταστήστε τις τιμές κράτησης θέσης [αναγνωριστικό συσκευής] και [κλειδί συσκευής] με τιμές για τη συσκευή σας από τον απομακρυσμένο πίνακα εργαλείων παρακολούθησης λύση. Χρησιμοποιήστε το όνομα κεντρικού υπολογιστή IoT διανομέα από τον πίνακα εργαλείων για να αντικαταστήσετε [όνομα IoTHub]. Για παράδειγμα, εάν σας Hostname διανομέα IoT είναι **contoso.azure devices.net**, αντικαταστήστε [όνομα IoTHub] με **contoso**:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```

3. Προσθέστε τον ακόλουθο κώδικα για να ορίσετε το μοντέλο που επιτρέπει τη συσκευή για να επικοινωνήσετε με το Κέντρο IoT. Αυτό το μοντέλο Καθορίζει ότι η συσκευή στέλνει θερμοκρασίας, εξωτερική θερμοκρασία, υγρασία και ένα αναγνωριστικό συσκευής ως τηλεμετρίας. Η συσκευή στέλνει επίσης μετα-δεδομένα σχετικά με τη συσκευή με IoT διανομέα, καθώς και μια λίστα των εντολών που υποστηρίζει η συσκευή. Αυτή η συσκευή ανταποκρίνεται στις εντολές **SetTemperature** και **SetHumidity**:

    ```
    // Define the Model
    BEGIN_NAMESPACE(Contoso);

    DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
    );

    DECLARE_STRUCT(DeviceProperties,
    ascii_char_ptr, DeviceID,
    _Bool, HubEnabledState
    );

    DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
    );

    END_NAMESPACE(Contoso);
    ```

## <a name="implement-the-behavior-of-the-device"></a>Υλοποίηση της συμπεριφοράς της συσκευής

Τώρα μπορείτε να προσθέσετε κώδικα που υλοποιεί τη συμπεριφορά που καθορίζονται στο μοντέλο.

1. Προσθέστε τις ακόλουθες λειτουργίες που εκτελούνται όταν η συσκευή λάβει τις εντολές **SetTemperature** και **SetHumidity** από διανομέα IoT:

    ```
    EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
    {
      (void)printf("Received temperature %d\r\n", temperature);
      thermostat->Temperature = temperature;
      return EXECUTE_COMMAND_SUCCESS;
    }

    EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
    {
      (void)printf("Received humidity %d\r\n", humidity);
      thermostat->Humidity = humidity;
      return EXECUTE_COMMAND_SUCCESS;
    }
    ```

2. Προσθέστε την ακόλουθη συνάρτηση που στέλνει ένα μήνυμα σε διανομέα IoT:

    ```
    static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
    {
      IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
      if (messageHandle == NULL)
      {
        printf("unable to create a new IoTHubMessage\r\n");
      }
      else
      {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, NULL, NULL) != IOTHUB_CLIENT_OK)
        {
          printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
          printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
      }
    free((void*)buffer);
    }
    ```

3. Προσθέστε την ακόλουθη συνάρτηση που άγκιστρα τη βιβλιοθήκη σειριοποίησης στο SDK:

    ```
    static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
      IOTHUBMESSAGE_DISPOSITION_RESULT result;
      const unsigned char* buffer;
      size_t size;
      if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
      {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
      }
      else
      {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
          printf("failed to malloc\r\n");
          result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
          memcpy(temp, buffer, size);
          temp[size] = '\0';
          EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
          result =
            (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
            (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
            IOTHUBMESSAGE_REJECTED;
          free(temp);
        }
      }
      return result;
    }
    ```

4. Προσθέστε την ακόλουθη συνάρτηση για να συνδεθείτε με IoT διανομέα, αποστολή και λήψη μηνυμάτων και αποσύνδεση από την ενότητα. Παρατηρήστε πώς η συσκευή αποστέλλει μεταδεδομένων σχετικά με μόνη της, συμπεριλαμβάνοντας τις εντολές που υποστηρίζει, με διανομέα IoT κατά τη σύνδεση. Αυτό μετα-δεδομένων επιτρέπει τη λύση για να ενημερώσετε την κατάσταση της συσκευής **εκτελείται** στον πίνακα εργαλείων:

    ```
    void remote_monitoring_run(void)
    {
      if (serializer_init(NULL) != SERIALIZER_OK)
      {
        printf("Failed on serializer_init\r\n");
      }
      else
      {
        IOTHUB_CLIENT_CONFIG config;
        IOTHUB_CLIENT_HANDLE iotHubClientHandle;

        config.deviceId = deviceId;
        config.deviceKey = deviceKey;
        config.iotHubName = hubName;
        config.iotHubSuffix = hubSuffix;
        config.protocol = HTTP_Protocol;
        config.deviceSasToken = NULL;
        config.protocolGatewayHostName = NULL;
        iotHubClientHandle = IoTHubClient_Create(&config);
        if (iotHubClientHandle == NULL)
        {
          (void)printf("Failed on IoTHubClient_CreateFromConnectionString\r\n");
        }
        else
        {
          Thermostat* thermostat = CREATE_MODEL_INSTANCE(Contoso, Thermostat);
          if (thermostat == NULL)
          {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
          }
          else
          {
            STRING_HANDLE commandsMetadata;

            if (IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, thermostat) != IOTHUB_CLIENT_OK)
            {
              printf("unable to IoTHubClient_SetMessageCallback\r\n");
            }
            else
            {

              /* send the device info upon startup so that the cloud app knows
              what commands are available and the fact that the device is up */
              thermostat->ObjectType = "DeviceInfo";
              thermostat->IsSimulatedDevice = false;
              thermostat->Version = "1.0";
              thermostat->DeviceProperties.HubEnabledState = true;
              thermostat->DeviceProperties.DeviceID = (char*)deviceId;

              commandsMetadata = STRING_new();
              if (commandsMetadata == NULL)
              {
                (void)printf("Failed on creating string for commands metadata\r\n");
              }
              else
              {
                /* Serialize the commands metadata as a JSON string before sending */
                if (SchemaSerializer_SerializeCommandMetadata(GET_MODEL_HANDLE(Contoso, Thermostat), commandsMetadata) != SCHEMA_SERIALIZER_OK)
                {
                  (void)printf("Failed serializing commands metadata\r\n");
                }
                else
                {
                  unsigned char* buffer;
                  size_t bufferSize;
                  thermostat->Commands = (char*)STRING_c_str(commandsMetadata);

                  /* Here is the actual send of the Device Info */
                  if (SERIALIZE(&buffer, &bufferSize, thermostat->ObjectType, thermostat->Version, thermostat->IsSimulatedDevice, thermostat->DeviceProperties, thermostat->Commands) != IOT_AGENT_OK)
                  {
                    (void)printf("Failed serializing\r\n");
                  }
                  else
                  {
                    sendMessage(iotHubClientHandle, buffer, bufferSize);
                  }

                }

                STRING_delete(commandsMetadata);
              }

              thermostat->Temperature = 50;
              thermostat->ExternalTemperature = 55;
              thermostat->Humidity = 50;
              thermostat->DeviceId = (char*)deviceId;

              while (1)
              {
                unsigned char*buffer;
                size_t bufferSize;

                (void)printf("Sending sensor value Temperature = %d, Humidity = %d\r\n", thermostat->Temperature, thermostat->Humidity);

                if (SERIALIZE(&buffer, &bufferSize, thermostat->DeviceId, thermostat->Temperature, thermostat->Humidity, thermostat->ExternalTemperature) != IOT_AGENT_OK)
                {
                  (void)printf("Failed sending sensor value\r\n");
                }
                else
                {
                  sendMessage(iotHubClientHandle, buffer, bufferSize);
                }

                ThreadAPI_Sleep(1000);
              }
            }

            DESTROY_MODEL_INSTANCE(thermostat);
          }
          IoTHubClient_Destroy(iotHubClientHandle);
        }
        serializer_deinit();
      }
    }
    ```
    
    Για αναφορά, ακολουθεί ένα δείγμα μηνύματος **DeviceInfo** που αποστέλλονται σε διανομέα IoT κατά την εκκίνηση:

    ```
    {
      "ObjectType":"DeviceInfo",
      "Version":"1.0",
      "IsSimulatedDevice":false,
      "DeviceProperties":
      {
        "DeviceID":"mydevice01", "HubEnabledState":true
      }, 
      "Commands":
      [
        {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
        { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
      ]
    }
    ```
    
    Για αναφορά, ακολουθεί ένα δείγμα μηνύματος **Τηλεμετρίας** που αποστέλλονται σε διανομέα IoT:

    ```
    {"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
    ```
    
    Για αναφορά, ακολουθεί ένα δείγμα **εντολής** που λάβατε από διανομέα IoT:
    
    ```
    {
      "Name":"SetHumidity",
      "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
      "CreatedTime":"2016-03-11T15:09:44.2231295Z",
      "Parameters":{"humidity":23}
    }
    ```

5. Αντικαταστήστε το **κύριο** συνάρτηση με το ακόλουθο κώδικα για να καλέσετε τη συνάρτηση **remote_monitoring_run** :

    ```
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

6. Κάντε κλικ στην επιλογή **Δημιουργία** και, στη συνέχεια, **Δημιουργία λύση** για να δημιουργήσετε την εφαρμογή συσκευή.

7. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο **RMDevice** , κάντε κλικ στην επιλογή **Εντοπισμός σφαλμάτων**και, στη συνέχεια, κάντε κλικ στην επιλογή **Έναρξη νέας παρουσίας** για να εκτελέσετε το δείγμα. Στην κονσόλα εμφανίζει τα μηνύματα, όπως η εφαρμογή στέλνει τηλεμετρίας δείγμα IoT διανομέα και λαμβάνει εντολές.

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx