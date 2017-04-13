<properties
   pageTitle="Συνδέστε μια συσκευή χρησιμοποιώντας C σε Linux | Microsoft Azure"
   description="Περιγράφει τον τρόπο για να συνδέσετε μια συσκευή για να την οικογένεια προγραμμάτων IoT Azure προ-διαμορφωμένες απομακρυσμένο λύση παρακολούθησης με μια εφαρμογή που έχει συνταχθεί σε C που εκτελείται σε Linux."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Συνδέστε τη συσκευή με το απομακρυσμένο προδιαμορφωμένο λύση παρακολούθησης (Linux)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Δημιουργία και εκτέλεση ενός πελάτη δείγμα C Linux

Οι παρακάτω διαδικασίες σας δείχνουν πώς μπορείτε να δημιουργήσετε μια εφαρμογή προγράμματος-πελάτη, γραμμένο σε C και να δημιουργηθεί και να εκτελέσετε σε Ubuntu Linux, που επικοινωνεί με το απομακρυσμένο προδιαμορφωμένο λύση παρακολούθησης. Για να ολοκληρώσετε αυτά τα βήματα, χρειάζεστε μια συσκευή στην οποία εκτελείται Ubuntu έκδοση 15.04 ή 15.10. Πριν να συνεχίσετε, εγκαταστήστε τα προαπαιτούμενα πακέτα στη συσκευή σας Ubuntu χρησιμοποιώντας την ακόλουθη εντολή:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a>Εγκατάσταση του προγράμματος-πελάτη βιβλιοθήκες στη συσκευή σας

Οι βιβλιοθήκες διανομέα IoT Azure προγράμματος-πελάτη είναι διαθέσιμες ως ένα πακέτο που μπορείτε να εγκαταστήσετε το στη συσκευή σας Ubuntu χρησιμοποιώντας την εντολή **get κατάλληλη** . Ολοκληρώστε τα ακόλουθα βήματα για να εγκαταστήσετε το πακέτο που περιέχει τα διανομέα IoT προγράμματος-πελάτη βιβλιοθήκη και κεφαλίδα αρχεία στον υπολογιστή σας Ubuntu:

1. Προσθήκη του αποθετηρίου AzureIoT στον υπολογιστή:

    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

2. Εγκατάσταση του πακέτου azure-iot-sdk-c-ανάπτυξης

    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="add-code-to-specify-the-behavior-of-the-device"></a>Προσθήκη κώδικα για να καθορίσετε τη συμπεριφορά της συσκευής

Στον υπολογιστή σας Ubuntu, δημιουργήστε ένα φάκελο που ονομάζεται **απομακρυσμένης\_παρακολούθηση**. Στο το **απομακρυσμένης\_παρακολούθηση** τα τέσσερα αρχεία **main.c**, Δημιουργία φακέλου **απομακρυσμένης\_monitoring.c**, **απομακρυσμένης\_monitoring.h**, και **CMakeLists.txt**.

Οι βιβλιοθήκες διανομέα IoT σειριοποιητής προγράμματος-πελάτη Χρησιμοποιήστε ένα μοντέλο για να καθορίσετε τη μορφή των μηνυμάτων στη συσκευή σας στείλει IoT διανομέα και τις εντολές που λαμβάνει από διανομέα IoT.

1. Σε ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το **απομακρυσμένης\_monitoring.c** αρχείου. Προσθέστε τα ακόλουθα `#include` προτάσεις:

    ```
    #include "iothubtransportamqp.h"
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
    static const char* hubName = "IoTHub Name]";
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

### <a name="add-code-to-implement-the-behavior-of-the-device"></a>Προσθήκη κώδικα για την υλοποίηση της συμπεριφοράς της συσκευής

Προσθέστε τις συναρτήσεις για την εκτέλεση όταν η συσκευή λάβει μια εντολή από την ενότητα, καθώς και τον κωδικό για να στείλετε προσομοιωμένη τηλεμετρίας για την ενότητα.

1. Προσθέστε τις ακόλουθες λειτουργίες που εκτελούνται όταν η συσκευή λάβει τις εντολές **SetTemperature** και **SetHumidity** καθορίζονται στο μοντέλο:

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
      if (platform_init() != 0)
      {
        printf("Failed to initialize the platform.\r\n");
      }
      else
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
          config.deviceSasToken = NULL;
          config.iotHubName = hubName;
          config.iotHubSuffix = hubSuffix;
          config.protocol = AMQP_Protocol;
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
        platform_deinit();
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

### <a name="add-code-to-invoke-the-remotemonitoringrun-function"></a>Προσθήκη κώδικα για να καλέσετε τη συνάρτηση remote_monitoring_run

Σε ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο **remote_monitoring.h** . Προσθέστε τον ακόλουθο κώδικα:

```
void remote_monitoring_run(void);
```

Σε ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο **main.c** . Προσθέστε τον ακόλουθο κώδικα:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="use-cmake-to-build-the-client-application"></a>Χρησιμοποιήστε CMake για να δημιουργήσετε την εφαρμογή-πελάτη

Ακολουθήστε τα παρακάτω βήματα περιγράφουν τον τρόπο χρήσης του *CMake* για να δημιουργήσετε την εφαρμογή-πελάτη.

1. Σε ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο **CMakeLists.txt** στο φάκελο **remote_monitoring** .

2. Προσθέστε τις παρακάτω οδηγίες για να ορίσετε τον τρόπο για να δημιουργήσετε την εφαρμογή-πελάτη:

    ```
    cmake_minimum_required(VERSION 2.8.11)

    set(AZUREIOT_INC_FOLDER ".." "/usr/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_amqp_transport
        aziotsharedutil
        uamqp
        pthread
        curl
        ssl
        crypto
    )
    ```

3. Στο φάκελο **remote_monitoring** , δημιουργήστε ένα φάκελο για να αποθηκεύσετε τα αρχεία που *κάνετε* που παράγει το CMake και, στη συνέχεια, εκτελέστε τις εντολές **cmake** και να **κάνετε** ως εξής:

    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

4. Εκτελέστε την εφαρμογή υπολογιστή-πελάτη και αποστολή τηλεμετρίας σε διανομέα IoT:

    ```
    ./sample_app
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

