<properties
   pageTitle="Ρύθμιση παραμέτρων λειτουργίας διανομής εξισορρόπηση φόρτου | Microsoft Azure"
   description="Πώς μπορείτε να τη ρύθμιση παραμέτρων λειτουργίας διανομής εξισορρόπησης φόρτου Azure για την υποστήριξη συσχέτισης IP προέλευσης"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="configure-the-distribution-mode-for-load-balancer"></a>Ρύθμιση παραμέτρων της λειτουργίας διανομής για εξισορρόπηση φόρτου

## <a name="hash-based-distribution-mode"></a>Λειτουργία βάσει κατακερματισμός διανομής

Ο προεπιλεγμένος αλγόριθμος κατανομής είναι μιας πλειάδας 5 (IP, θύρα προέλευσης, προέλευσης IP προορισμού, θύρα προορισμού, πρωτόκολλο τύπο) κατακερματισμός για να αντιστοιχίσετε την κυκλοφορία διαθέσιμων διακομιστών. Παρέχει υποστεί μόνο μέσα σε μια περίοδο λειτουργίας μεταφοράς. Για να την ίδια παρουσία IP (DIP) κέντρο δεδομένων πίσω από το τελικό σημείο εξισορρόπησης φόρτου θα κατευθύνεται πακέτων στην ίδια περίοδο λειτουργίας. Όταν ο υπολογιστής-πελάτης ξεκινά μια νέα περίοδο λειτουργίας από το ίδιο IP προέλευσης, τη θύρα προέλευσης αλλάζει και έχει ως αποτέλεσμα την κίνηση για να μεταβείτε σε διαφορετικό endpoint DIP.

![Κατακερματισμός βάσει εξισορρόπηση φόρτου](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Σχήμα 1-5-πλειάδας διανομής

## <a name="source-ip-affinity-mode"></a>Λειτουργία συσχέτισης IP προέλευσης

Έχουμε διαφορετική λειτουργία διανομής που ονομάζεται συσχέτισης IP προέλευσης (γνωστό και ως περίοδο λειτουργίας συσχέτισης ή συσχέτισης IP υπολογιστή-πελάτη). Azure εξισορρόπηση φόρτου μπορεί να ρυθμιστεί για να χρησιμοποιήσετε μια 2-πλειάδας (IP προέλευσης, διεύθυνση IP προορισμού) ή 3-πλειάδας (πρωτόκολλο IP προέλευσης, διεύθυνση IP προορισμού,) για να αντιστοιχίσετε την κυκλοφορία τους διαθέσιμους διακομιστές. Με τη χρήση συσχέτισης IP προέλευσης, συνδέσεων που ξεκινά από τον ίδιο υπολογιστή-πελάτη Μετάβαση στο ίδιο τελικό σημείο DIP.

Το παρακάτω διάγραμμα παρουσιάζει μια ρύθμιση παραμέτρων 2-πλειάδας. Παρατηρήστε πώς 2-πλειάδας εκτελείται έως τη μονάδα εξισορρόπησης φόρτου στην εικονική μηχανή 1 (VM1) που δημιουργείται ένα αντίγραφο ασφαλείας, VM2 και VM3.

![συνάφεια περιόδου λειτουργίας](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Εικόνα 2-2-πλειάδας διανομής

Συνάφεια IP προέλευσης επιλύει μια ασυμβατότητα μεταξύ του Azure εξισορρόπηση φόρτου και πύλη απομακρυσμένης επιφάνειας εργασίας (RD). Τώρα μπορείτε να δημιουργήσετε ένα σύμπλεγμα πύλης απομακρυσμένης επιφάνειας Εργασίας σε μια υπηρεσία cloud μία.

Μια άλλη περίπτωσης χρήσης είναι αποστολή πολυμέσων όπου πραγματοποιείται η αποστολή δεδομένων μέσω UDP αλλά είναι δυνατό το επίπεδο στοιχείου ελέγχου μέσω TCP:

- Ένα πρόγραμμα-πελάτη πρώτα ξεκινά μια περίοδο λειτουργίας TCP στη δημόσια διεύθυνση εξισορρόπησης φόρτου, λαμβάνει κατευθύνεται σε μια συγκεκριμένη DIP, αυτό το κανάλι παραμείνει ενεργό για την παρακολούθηση της εύρυθμης λειτουργίας της σύνδεσης
- Μια νέα περίοδο λειτουργίας UDP από την ίδια ξεκινά υπολογιστή-πελάτη στο ίδιο τελικό σημείο δημόσια εξισορρόπησης φόρτου, την προοπτική εδώ είναι ότι αυτή η σύνδεση επίσης κατευθύνεται στο ίδιο τελικό σημείο DIP κατά την προηγούμενη σύνδεση TCP ώστε να αποστέλλετε πολυμέσων μπορεί να εκτελεστεί στο υψηλής απόδοσης διατηρώντας επίσης ένα στοιχείο ελέγχου κανάλι μέσω TCP.

>[AZURE.NOTE] Όταν αλλάζει η εξισορρόπηση φόρτου σύνολο (Κατάργηση του ή προσθέτοντας μια εικονική μηχανή), η κατανομή των αιτήσεων προγράμματος-πελάτη είναι recomputed. Δεν είναι δυνατό να εξαρτώνται νέες συνδέσεις από υπάρχοντα προγράμματα-πελάτες που τελειώνει προς τα επάνω στο ίδιο διακομιστή. Επιπλέον, με χρήση IP προέλευσης λειτουργία διανομής συσχέτισης μπορεί να προκαλέσει μια άνισες κατανομή της κυκλοφορίας. Προγράμματα-πελάτες λειτουργούν πίσω από διακομιστές μεσολάβησης ενδέχεται να θεωρούνται ως μία εφαρμογή μοναδικό προγράμματος-πελάτη.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Ρύθμιση των παραμέτρων ρυθμίσεων συσχέτισης IP προέλευσης για μονάδα εξισορρόπησης φόρτου

Για εικονικές μηχανές, μπορείτε να χρησιμοποιήσετε PowerShell για να αλλάξετε τις ρυθμίσεις χρονικών ορίων:

Προσθήκη Azure τελικού σημείου σε μια εικονική μηχανή και ορίστε λειτουργία διανομής εξισορρόπησης φόρτου

    Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM

LoadBalancerDistribution μπορεί να οριστεί σε sourceIP για εξισορρόπηση, sourceIPProtocol για εξισορρόπηση φόρτου 3-πλειάδας (πρωτόκολλο IP προέλευσης, διεύθυνση IP προορισμού,) ή καμία εάν θέλετε την προεπιλεγμένη συμπεριφορά της εξισορρόπηση φόρτου 5-πλειάδας φόρτωση 2-πλειάδας (IP προέλευσης, διεύθυνση IP προορισμού).

Χρησιμοποιήστε τα ακόλουθα για να ανακτήσετε μια παραμέτρων λειτουργίας τελικού σημείου φόρτωσης εξισορρόπησης διανομής:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15
    LoadBalancerDistribution : sourceIP

Εάν δεν υπάρχει το στοιχείο LoadBalancerDistribution τη μονάδα εξισορρόπησης φόρτου Azure χρησιμοποιεί ο προεπιλεγμένος αλγόριθμος 5-πλειάδας.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Ρύθμιση της λειτουργίας διανομής σε ένα σύνολο τελικού σημείου εξισορρόπησης φόρτου

Εάν τελικά σημεία αποτελούν μέρος ενός συνόλου τελικού σημείου εξισορρόπησης φόρτου, τη λειτουργία διανομής πρέπει να οριστούν σε το σύνολο τελικού σημείου εξισορρόπησης φόρτου:

    Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Ρύθμιση παραμέτρων της υπηρεσίας cloud για να αλλάξετε λειτουργία διανομής

Μπορείτε να αξιοποιήσετε στο SDK Azure για το 2,5 .NET (να κυκλοφορούν Νοέμβριο) για να ενημερώσετε την υπηρεσία Cloud. Ρυθμίσεις τελικό σημείο για τις υπηρεσίες Cloud γίνονται στο το .csdef. Για να ενημερώσετε τη λειτουργία διανομής εξισορρόπησης φόρτου για μια ανάπτυξη υπηρεσίες Cloud, απαιτείται μια αναβάθμιση ανάπτυξης.
Ακολουθεί ένα παράδειγμα της .csdef αλλαγές των ρυθμίσεων τελικού σημείου:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
      </Endpoints>
    </WorkerRole>
    <NetworkConfiguration>
      <VirtualNetworkSite name="VNet"/>
      <AddressAssignments>
    <InstanceAddress roleName="VMRolePersisted">
      <PublicIPs>
        <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
      </PublicIPs>
    </InstanceAddress>
      </AddressAssignments>
    </NetworkConfiguration>

## <a name="api-example"></a>Παράδειγμα API

Μπορείτε να ρυθμίσετε την κατανομή εξισορρόπησης φόρτου χρησιμοποιώντας το API διαχείρισης υπηρεσίας. Βεβαιωθείτε ότι έχετε προσθέσει το `x-ms-version` κεφαλίδα έχει οριστεί σε έκδοση `2014-09-01` ή νεότερη έκδοση.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>Ενημερώστε τη ρύθμιση παραμέτρων για το καθορισμένο σύνολο εξισορρόπηση φόρτου σε ανάπτυξη

#### <a name="request-example"></a>Παράδειγμα αίτηση

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet    x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

Η τιμή του LoadBalancerDistribution μπορεί να είναι sourceIP για 2-πλειάδας συσχέτισης, sourceIPProtocol για 3-πλειάδας συσχέτισης ή καμία (για χωρίς συνάφεια. δηλαδή 5-πλειάδας)

#### <a name="response"></a>Απόκριση

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Επόμενα βήματα

[Επισκόπηση εξισορρόπησης εσωτερικό φόρτωσης](load-balancer-internal-overview.md)

[Γρήγορα αποτελέσματα με τη ρύθμιση των παραμέτρων Internet αντικριστές εξισορρόπηση φόρτου](load-balancer-get-started-internet-arm-ps.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)
