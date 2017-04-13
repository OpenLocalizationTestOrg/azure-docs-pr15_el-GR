<properties
   pageTitle="Ύφασμα αντίστροφης μεσολάβησης υπηρεσίας | Microsoft Azure"
   description="Χρήση της υπηρεσίας ύφασμα αντίστροφης μεσολάβησης για επικοινωνία με το microservices από εντός και εκτός του συμπλέγματος"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/04/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-reverse-proxy"></a>Υπηρεσία ύφασμα αντίστροφης μεσολάβησης

Η υπηρεσία ύφασμα αντίστροφης μεσολάβησης είναι ένα ύφασμα ενσωματωμένα στο υπηρεσίας αντίστροφης μεσολάβησης που επιτρέπει την προσθήκη διεύθυνσης σε microservices στο σύμπλεγμα ύφασμα υπηρεσίας που εμφανίζουν τα τελικά σημεία HTTP.

## <a name="microservices-communication-model"></a>Μοντέλο Microservices επικοινωνίας

Microservices στην υπηρεσία ύφασμα συνήθως εκτελείται σε ένα υποσύνολο του Εικονική στο σύμπλεγμα και να μετακινήσετε από μία Εικονική σε ένα άλλο για διάφορους λόγους. Επομένως, τα τελικά σημεία για microservices να αλλάξετε δυναμικά. Το τυπικό μοτίβο για να επικοινωνείτε με το microservice είναι το παρακάτω, επίλυση βρόχο

1. Επίλυση η θέση της υπηρεσίας αρχικά μέσω της υπηρεσίας ονομασία.
2. Σύνδεση με την υπηρεσία.
3. Προσδιορίστε την αιτία αποτυχιών σύνδεσης και να επιλύσετε εκ νέου η θέση της υπηρεσίας όταν είναι απαραίτητο.

Αυτή η διαδικασία περιλαμβάνει γενικά αναδίπλωση βιβλιοθήκες επικοινωνίας πλευρά του προγράμματος-πελάτη σε "Επανάληψη" Επανάληψη που υλοποιεί τις πολιτικές υπηρεσιών ανάλυσης και προσπαθήστε ξανά.
Για περισσότερες πληροφορίες σχετικά με αυτό το θέμα, ανατρέξτε στο θέμα [Ενημέρωση σχετικά με τις υπηρεσίες](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-via-sf-reverse-proxy"></a>Επικοινωνία μέσω Ελ αντίστροφης μεσολάβησης
Υπηρεσία ύφασμα αντίστροφης μεσολάβησης εκτελείται σε όλους τους κόμβους του συμπλέγματος. Το εκτελεί τη διαδικασία επίλυσης ολόκληρη την υπηρεσία εκ μέρους ενός πελάτη και, στη συνέχεια, προωθεί την αίτηση προγράμματος-πελάτη. Επομένως, προγράμματα-πελάτες που εκτελούνται στο σύμπλεγμα απλώς να χρησιμοποιήσετε βιβλιοθήκες επικοινωνίας HTTP πλευρά του προγράμματος-πελάτη για να συνομιλήσετε με την υπηρεσία προορισμού μέσω του Ελ αντίστροφης μεσολάβησης εκτελείται τοπικά στον ίδιο κόμβο.

![Εσωτερική επικοινωνία][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>Επικοινωνούν μαζί Microservices από έξω από το σύμπλεγμα
Το μοντέλο εξωτερική επικοινωνία προεπιλογή για microservices είναι **επιλογή** όπου κάθε υπηρεσία από προεπιλογή δεν είναι δυνατή η πρόσβαση απευθείας από εξωτερικά προγράμματα-πελάτες. Η [Μονάδα εξισορρόπησης φόρτου Azure](../load-balancer/load-balancer-overview.md) είναι ένα όριο δικτύου μεταξύ microservices και εξωτερικά προγράμματα-πελάτες, που εκτελεί μετάφραση διευθύνσεων δικτύου και το προωθεί προσκλήσεις εξωτερικών στην εσωτερική **IP: Port** τελικά σημεία. Προκειμένου να κάνετε μια microservice τελικού σημείου άμεση πρόσβαση σε εξωτερικά προγράμματα-πελάτες, πρέπει πρώτα να ρυθμίσετε τη μονάδα εξισορρόπησης φόρτου Azure για κίνηση προς τα εμπρός κάθε θύρα που χρησιμοποιείται από την υπηρεσία του συμπλέγματος. Επιπλέον, οι περισσότερες microservices (esp. κατάστασης microservices) δεν live σε όλους τους κόμβους του συμπλέγματος και μπορούν να μετακινούν μεταξύ τους κόμβους στην ανακατεύθυνσης, ώστε να σε αυτές τις περιπτώσεις, την εξισορρόπηση φόρτου Azure αποτελεσματική δεν μπορεί να καθορίσει τον κόμβο προορισμού από το αντίγραφα βρίσκονται να προωθήσετε την κυκλοφορία που.

### <a name="reaching-microservices-via-the-sf-reverse-proxy-from-outside-the-cluster"></a>Επικοινωνούν μαζί Microservices μέσω Ελ αντίστροφης μεσολάβησης από έξω από το σύμπλεγμα

Αντί για τη ρύθμιση παραμέτρων της υπηρεσίας μεμονωμένες θύρες στο το azure εξισορρόπηση φόρτου, απλώς τη θύρα διακομιστή μεσολάβησης Αντιστροφή Ελ μπορεί να ρυθμιστεί στο το πρόγραμμα εξισορρόπησης φόρτου Azure. Αυτό σας επιτρέπει σε προγράμματα-πελάτες εκτός του συμπλέγματος για την επίτευξη υπηρεσίες μέσα στο σύμπλεγμα μέσω του αντίστροφης μεσολάβησης χωρίς μια επιπλέον ρυθμίσεις παραμέτρων.

![Εξωτερική επικοινωνία][0]

>[AZURE.WARNING] Ρύθμιση παραμέτρων της αντίστροφης μεσολάβησης θύρας στην τη μονάδα εξισορρόπησης φόρτου, κάνει όλες τις υπηρεσίες micro στο σύμπλεγμα που εμφανίζουν ένα τελικό σημείο http, για να addressible από έξω από το σύμπλεγμα.


## <a name="uri-format-for-addressing-services-via-the-reverse-proxy"></a>Μορφή URI για την προσθήκη διεύθυνσης σε υπηρεσίες μέσω του διακομιστή αντίστροφης μεσολάβησης

Το αντίστροφης μεσολάβησης χρησιμοποιεί μια συγκεκριμένη μορφή URI για να προσδιορίσετε ποια υπηρεσία διαμερίσματα θα προωθείται η εισερχόμενη αίτηση:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **οδηγεί:** Το αντίστροφης μεσολάβησης μπορεί να ρυθμιστεί για να αποδεχτείτε την κυκλοφορία HTTP ή HTTPS. Σε περίπτωση HTTPS κίνηση, SSL τερματισμού συμβαίνει σε το αντίστροφης μεσολάβησης. Οι αιτήσεις που έχουν προωθηθεί από το αντίστροφης μεσολάβησης με τις υπηρεσίες του συμπλέγματος είναι μέσω http.
 - **FQDN σύμπλεγμα | εσωτερική IP:** Για εξωτερικά προγράμματα-πελάτες, το αντίστροφης μεσολάβησης μπορεί να ρυθμιστεί ώστε να είναι δυνατή η πρόσβαση μέσω του τομέα συμπλέγματος (π.χ., mycluster.eastus.cloudapp.azure.com). Από προεπιλογή η αντίστροφης μεσολάβησης εκτελείται σε κάθε κόμβο, επομένως, για εσωτερική κυκλοφορία του μπορούν να σας καλούν σε τοπικού κεντρικού υπολογιστή ή σε οποιοδήποτε εσωτερικό κόμβο IP (π.χ., 10.0.0.1).
 - **Θύρας:** Η θύρα που έχει καθοριστεί για την αντίστροφης μεσολάβησης. Π.χ.: 19008.
 - **ServiceInstanceName:** Αυτό είναι το όνομα της παρουσίας πλήρως προσδιορισμένο ανεπτυγμένος υπηρεσίας της υπηρεσίας που προσπαθείτε να φτάσετε το "ύφασμα: /" συνδυασμού. Για παράδειγμα, για την επίτευξη υπηρεσίας *ύφασμα: / myapp/myservice/*, πρέπει να χρησιμοποιήσετε *myapp/myservice*.
 - **Διαδρομή επίθημα:** Αυτή είναι η πραγματική διαδρομή διεύθυνσης URL για την υπηρεσία που θέλετε να συνδεθείτε. Για παράδειγμα, *myapi/τιμές/Προσθήκη/3*
 - **PartitionKey:** Για μια υπηρεσία διαμερίσματα, αυτό είναι το κλειδί υπολογισμένη διαμερίσματα από τα διαμερίσματα που θέλετε να επικοινωνήσετε. Σημειώστε ότι αυτό είναι *δεν* τα διαμερίσματα Αναγνωριστικό GUID. Αυτή η παράμετρος δεν απαιτείται για υπηρεσίες χρησιμοποιώντας το συνδυασμό διαμερισμάτων singleton.
 - **PartitionKind:** Ο συνδυασμός διαμερίσματα υπηρεσίας. Αυτό μπορεί να είναι 'Int64Range' ή "Με όνομα". Αυτή η παράμετρος δεν απαιτείται για υπηρεσίες χρησιμοποιώντας το συνδυασμό διαμερισμάτων singleton.
 - **Χρονικό όριο:**  Καθορίζει το χρονικό όριο για την αίτηση http που δημιουργήθηκε από το διακομιστή αντίστροφης μεσολάβησης στην υπηρεσία εκ μέρους της αίτησης προγράμματος-πελάτη. Η προεπιλεγμένη τιμή για αυτό είναι 60 δευτερόλεπτα. Αυτή είναι μια προαιρετική παράμετρος.

### <a name="example-usage"></a>Παράδειγμα χρήσης

Ως παράδειγμα, ας ρίξουμε υπηρεσίας **ύφασμα: / MyApp/MyService** που ανοίγει μια ακρόασης HTTP στο την παρακάτω διεύθυνση URL:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Με τους ακόλουθους πόρους:

 - `/index.html`
 - `/api/users/<userId>`

Εάν η υπηρεσία χρησιμοποιεί του συνδυασμού διαμερισμάτων singleton, τις παραμέτρους συμβολοσειράς ερωτήματος *PartitionKey* και *PartitionKind* δεν είναι απαραίτητα και την υπηρεσία μπορούν να σας καλούν μέσω της πύλης ως:

 - Εξωτερική:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Εσωτερικά:`http://localhost:19008/MyApp/MyService`

Εάν η υπηρεσία χρησιμοποιεί του συνδυασμού διαμερισμάτων ενιαίο Int64, τις παραμέτρους συμβολοσειράς ερωτήματος *PartitionKey* και *PartitionKind* πρέπει να χρησιμοποιούνται για την επίτευξη μιας διαμερίσματα της υπηρεσίας:

 - Εξωτερική:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Εσωτερικά:`http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Για την επίτευξη τους πόρους που εκτίθεται από την υπηρεσία, απλώς τοποθετήστε τη διαδρομή πόρου μετά το όνομα της υπηρεσίας στη διεύθυνση URL:

 - Εξωτερική:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Εσωτερικά:`http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Η πύλη, στη συνέχεια, θα προωθεί αυτές τις αιτήσεις διεύθυνση URL της υπηρεσίας:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Ειδικό χειρισμό για κοινή χρήση θύρας υπηρεσιών

Η πύλη εφαρμογής προσπαθεί να επιλύσετε εκ νέου μια διεύθυνση υπηρεσιών και προσπαθήστε ξανά την αίτηση όταν δεν είναι δυνατή η πρόσβαση στην μια υπηρεσία. Αυτό είναι ένα από τα κύρια πλεονεκτήματα της πύλης, όπως κωδικός προγράμματος-πελάτη δεν χρειάζεται να υλοποιήσετε το δικό της υπηρεσίας ανάλυσης και να επιλύετε βρόχο.

Γενικά όταν μια υπηρεσία δεν είναι προσβάσιμος αυτό σημαίνει ότι η παρουσία της υπηρεσίας ρεπλίκα έχει μετακινηθεί ή σε διαφορετικό κόμβο ως μέρος του την κανονική διάρκεια του κύκλου ζωής. Όταν συμβαίνει αυτό, η πύλη ενδέχεται να εμφανιστεί ένα σφάλμα σύνδεσης δικτύου που υποδεικνύει ότι δεν είναι πλέον είναι ανοιχτό στη αρχικά επιλυθεί διεύθυνση τελικού σημείου.

Ωστόσο, αντίγραφα ή παρουσιών της υπηρεσίας, να κάνετε κοινή χρήση μια διεργασία κεντρικού υπολογιστή και επίσης μπορεί να αντιστοιχεί σε μια θύρα όταν φιλοξενούνται από ένα διακομιστή web που βασίζεται σε http.sys, όπως:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [WebListener πυρήνα ASP.NET](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

Σε αυτήν την περίπτωση είναι πιθανό ότι ο διακομιστής web είναι διαθέσιμη σε η διεργασία κεντρικού υπολογιστή και απάντηση σε προσκλήσεις αλλά η παρουσία της υπηρεσίας επιλυθεί ή ρεπλίκα δεν είναι πλέον διαθέσιμο στον κεντρικό υπολογιστή. Σε αυτήν την περίπτωση, η πύλη θα λάβουν μια απόκριση HTTP 404 από το διακομιστή web. Ως αποτέλεσμα, ένα HTTP 404 περιλαμβάνει δύο ξεχωριστές σημασία:

 1. Τη διεύθυνση της υπηρεσίας είναι σωστή, αλλά δεν υπάρχει ο πόρος που ζητήθηκε από το χρήστη.
 2. Τη διεύθυνση της υπηρεσίας είναι εσφαλμένη και ο πόρος που ζητήθηκε από το χρήστη ενδέχεται να υπάρχει στην πραγματικότητα σε διαφορετικό κόμβο.

Στην πρώτη περίπτωση, αυτή είναι μια κανονική HTTP 404, που θεωρείται ένα σφάλμα χρήστη. Ωστόσο, στη δεύτερη περίπτωση, ο χρήστης έχει ζητήσει έναν πόρο που υπάρχουν, αλλά η πύλη δεν ήταν δυνατό να εντοπίσετε και επειδή την ίδια την υπηρεσία έχει μετακινηθεί, οπότε της πύλης πρέπει να επιλύσετε εκ νέου τη διεύθυνση και προσπαθήστε ξανά.

Η πύλη πρέπει, επομένως, ένας τρόπος για να διακρίνετε ανάμεσα σε αυτές τις δύο περιπτώσεις. Για να κάνετε αυτό διάκριση, απαιτείται μια υπόδειξη από το διακομιστή.

 - Από προεπιλογή, η πύλη εφαρμογής προϋποθέτει πεζών-κεφαλαίων #2 και προσπαθεί να επιλύσετε εκ νέου και εκ νέου έκδοση της αίτησης.
 - Για να υποδείξετε πεζών-κεφαλαίων #1 για την πύλη εφαρμογής, η υπηρεσία θα πρέπει να επιστρέψει την ακόλουθη κεφαλίδα απόκρισης HTTP:

`X-ServiceFabric : ResourceNotFound`

Αυτή η κεφαλίδα απόκρισης HTTP υποδεικνύει μια κανονική κατάσταση HTTP 404 με την οποία ο πόρος που ζητήθηκε δεν υπάρχει, και η πύλη δεν θα επιχειρήσει να επιλύσετε εκ νέου τη διεύθυνση της υπηρεσίας.

## <a name="setup-and-configuration"></a>Εγκατάσταση και ρύθμιση παραμέτρων
Η υπηρεσία ύφασμα αντίστροφης μεσολάβησης μπορεί να ενεργοποιηθεί για το σύμπλεγμα μέσω του [προτύπου για τη διαχείριση πόρων Azure](./service-fabric-cluster-creation-via-arm.md).

Όταν έχετε ολοκληρώσει το πρότυπο για το σύμπλεγμα που θέλετε να αναπτύξετε (από τα δείγματα προτύπων ή, δημιουργώντας ένα πρότυπο manager προσαρμοσμένου πόρου) μπορεί να ενεργοποιηθεί η αντίστροφης μεσολάβησης στο πρότυπο από τα παρακάτω βήματα.

1. Ορίστε μια θύρα για το αντίστροφης μεσολάβησης στην [ενότητα τις παραμέτρους](../resource-group-authoring-templates.md) του προτύπου.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Καθορίστε τη θύρα για κάθε ένα από τα αντικείμενα nodetype στην [ενότητα Τύπος πόρου](../resource-group-authoring-templates.md) **συμπλέγματος**

    Για apiVersion του πριν από το ' 2016-09-01' στη θύρα προσδιορίζεται από την παράμετρο όνομα ***httpApplicationGatewayEndpointPort***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

    Για apiVersion του ή να μετά ' 2016-09-01' στη θύρα προσδιορίζεται από την παράμετρο όνομα ***reverseProxyEndpointPort***

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

3. Για να αντιμετωπίσετε το αντίστροφης μεσολάβησης από έξω από το azure σύμπλεγμα, το πρόγραμμα εγκατάστασης των **κανόνων εξισορρόπησης φόρτου azure** για τη θύρα που καθορίσατε στο βήμα 1.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. Για να ρυθμίσετε πιστοποιητικά SSL στη θύρα για το διακομιστή αντίστροφης μεσολάβησης, προσθέστε το πιστοποιητικό στην ιδιότητα httpApplicationGatewayCertificate στην [ενότητα Τύπος πόρου](../resource-group-authoring-templates.md) **συμπλέγματος**

    Για apiVersion του πριν από το ' 2016-09-01' το πιστοποιητικό έχει προσδιοριστεί από την παράμετρο όνομα ***httpApplicationGatewayCertificate***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```
    Για apiVersion του ή να μετά ' 2016-09-01' το πιστοποιητικό έχει προσδιοριστεί από την παράμετρο όνομα ***reverseProxyCertificate***
    
    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## <a name="next-steps"></a>Επόμενα βήματα
 - Δείτε ένα παράδειγμα HTTP επικοινωνίας μεταξύ των υπηρεσιών σε ένα [Δείγμα έργου σε GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Κλήσεις απομακρυσμένης διαδικασίας με αξιόπιστη Υπηρεσίες απομακρυσμένης πρόσβασης](service-fabric-reliable-services-communication-remoting.md)

 - [API που χρησιμοποιεί OWIN στις αξιόπιστες υπηρεσίες Web](service-fabric-reliable-services-communication-webapi.md)

 - [WCF επικοινωνίας με χρήση των υπηρεσιών αξιόπιστη](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png