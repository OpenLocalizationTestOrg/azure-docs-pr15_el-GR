<properties
 pageTitle="Οδηγός για προγραμματιστές - SDK διανομέα IoT | Microsoft Azure"
 description="Azure IoT το Κέντρο για προγραμματιστές Οδηγός - πληροφορίες και συνδέσεις για τα διάφορα SDK συσκευή και υπηρεσιών Azure IoT διανομέα."
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016"
 ms.author="dobett"/>

# <a name="iot-hub-sdks"></a>Ο διανομέας IoT SDK

## <a name="iot-hub-device-sdks"></a>Συσκευή διανομέα IoT SDK

Η συσκευή Microsoft Azure IoT SDK περιέχει κώδικα που διευκολύνει τις συσκευές δόμησης και τις εφαρμογές που συνδεθείτε και πραγματοποιείται από το Κέντρο IoT Azure υπηρεσίες.

Το παρακάτω συσκευή IoT SDK είναι διαθέσιμα για λήψη από το GitHub:

- [Azure IoT συσκευή SDK για C] [ lnk-c-device-sdk] γραμμένο σε ANSI C (C99) για δυνατότητα μεταφοράς και γενικά πλατφόρμα συμβατότητας.
- [Azure IoT συσκευή SDK για .NET][lnk-dotnet-device-sdk]
- [Azure IoT συσκευή SDK για Java][lnk-java-device-sdk]
- [Azure IoT συσκευή SDK για Node.js][lnk-node-device-sdk]
- [Microsoft Azure IoT συσκευή SDK για Python 2.7][lnk-python-device-sdk]

> [AZURE.NOTE] Δείτε τα αρχεία readme των αποθετηρίων GitHub για πληροφορίες σχετικά με τη χρήση γλώσσας και οι διαχειριστές συγκεκριμένης πλατφόρμας πακέτου εγκατάστασης δυαδικών αρχείων και εξαρτήσεις στον υπολογιστή σας στην ανάπτυξη.

## <a name="os-platforms-and-hardware-compatibility"></a>Πλατφόρμες λειτουργικού Συστήματος και υλικό συμβατότητας

Για περισσότερες πληροφορίες σχετικά με τη συμβατότητα SDK με συγκεκριμένες συσκευές, ανατρέξτε στο θέμα το [Azure πιστοποιηθεί για τον κατάλογο συσκευή IoT][lnk-certified].

## <a name="iot-hub-service-sdks"></a>Ο διανομέας IoT υπηρεσίας SDK

Το Microsoft Azure IoT SDK υπηρεσίας περιέχει κώδικα που διευκολύνει τη δημιουργία εφαρμογών που αλληλεπιδρούν απευθείας με IoT διανομέα για τη Διαχείριση συσκευών και ασφάλεια.

Η ακόλουθη υπηρεσία IoT SDK είναι διαθέσιμα για λήψη από το GitHub:

- [Azure IoT υπηρεσία SDK για .NET][lnk-dotnet-service-sdk]
- [Azure IoT υπηρεσία SDK για Node.js][lnk-node-service-sdk]
- [Azure IoT υπηρεσίας SDK για Java][lnk-java-service-sdk]

> [AZURE.NOTE] Δείτε τα αρχεία readme των αποθετηρίων GitHub για πληροφορίες σχετικά με τη χρήση γλώσσας και οι διαχειριστές συγκεκριμένης πλατφόρμας πακέτου εγκατάστασης δυαδικών αρχείων και εξαρτήσεις στον υπολογιστή σας στην ανάπτυξη.

## <a name="azure-iot-gateway-sdk"></a>SDK Azure IoT πύλης

Αυτό το SDK πύλης IoT Azure περιλαμβάνει την υποδομή και λειτουργικές μονάδες για να δημιουργήσετε IoT πύλης λύσεις. Μπορείτε να επεκτείνετε το SDK για να δημιουργήσετε πύλες προσαρμοσμένες σε οποιαδήποτε τέλος-σεναρίου.

Μπορείτε να κάνετε λήψη του [Azure IoT πύλης SDK] [ lnk-gateway-sdk] από GitHub.

## <a name="online-api-reference-documentation"></a>Ηλεκτρονική τεκμηρίωση αναφοράς API

Ακολουθεί μια λίστα με συνδέσεις σε ηλεκτρονική τεκμηρίωση αναφοράς API για συσκευή Azure IoT, την υπηρεσία και βιβλιοθήκες πύλης:

- [Internet πράγματα (IoT) .NET][lnk-dotnet-ref]
- [Ο διανομέας IoT ΥΠΌΛΟΙΠΟ][lnk-rest-ref]
- [Azure IoT συσκευή SDK για C][lnk-c-ref]
- [Azure IoT συσκευή SDK για Java][lnk-java-ref]
- [Azure IoT υπηρεσίας SDK για Java][lnk-java-service-ref]
- [Azure IoT συσκευή SDK για Node.js][lnk-node-ref]
- [Azure IoT υπηρεσία SDK για Node.js][lnk-node-service-ref]
- [Azure IoT πύλης SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Επόμενα βήματα

Άλλα θέματα αναφορά σε αυτόν τον οδηγό για προγραμματιστές διανομέα IoT περιλαμβάνουν τα εξής:

- [Τα τελικά σημεία IoT διανομέα][lnk-devguide-endpoints]
- [Γλώσσα ερωτημάτων για twins, μεθόδους και εργασίες][lnk-devguide-query]
- [Τα όρια και περιορισμού][lnk-devguide-quotas]
- [Υποστήριξη MQTT διανομέα IoT][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md