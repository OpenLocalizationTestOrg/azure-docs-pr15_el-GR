<properties
   pageTitle="Ρύθμιση παραμέτρων του χρονικού ορίου αδράνειας TCP εξισορρόπησης φόρτου | Microsoft Azure"
   description="Ρύθμιση παραμέτρων του χρονικού ορίου αδράνειας TCP εξισορρόπησης φόρτου"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Ρύθμιση παραμέτρων TCP χρονικού ορίου αδράνειας για μονάδα εξισορρόπησης φόρτου Azure

Με την προεπιλεγμένη ρύθμιση παραμέτρων, Azure εξισορρόπηση φόρτου έχει μια ρύθμιση χρονικού ορίου αδράνειας 4 λεπτά. Εάν μια περίοδο αδράνειας είναι μεγαλύτερο από την τιμή χρονικού ορίου, δεν υπάρχει εγγύηση ότι η περίοδος λειτουργίας TCP ή HTTP διατηρείται μεταξύ του προγράμματος-πελάτη και την υπηρεσία cloud.

Όταν η σύνδεση είναι κλειστό, την εφαρμογή-πελάτη ενδέχεται να λάβετε το ακόλουθο μήνυμα σφάλματος: "τα υποκείμενα σύνδεση τερματίστηκε: μια σύνδεση που αναμενόταν να διατηρηθεί έχει κλείσει από το διακομιστή."

Μια κοινή πρακτική είναι να χρησιμοποιήσετε μια ενεργών TCP. Αυτή η πρακτική διατηρεί τη σύνδεση ενεργό για μεγαλύτερο χρονικό διάστημα. Για περισσότερες πληροφορίες, ανατρέξτε σε αυτά τα [παραδείγματα .NET](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Με ενεργοποιημένη ενεργών, αποστέλλονται τα πακέτα περιόδους αδράνειας κατά τη σύνδεση. Αυτά τα πακέτα ενεργών βεβαιωθείτε ότι το χρονικό όριο αδράνειας ποτέ όριο και διατηρείται η σύνδεση για μεγάλο χρονικό διάστημα.

Αυτή η ρύθμιση λειτουργεί για εισερχόμενες συνδέσεις μόνο. Για να αποτρέψετε την απώλεια της σύνδεσης, πρέπει να ρυθμίσετε τις παραμέτρους των ενεργών TCP με ένα χρονικό διάστημα μικρότερη από τη ρύθμιση του χρονικού ορίου αδράνειας ή να αυξήσετε την τιμή του χρονικού ορίου αδράνειας. Για την υποστήριξη τέτοια σενάρια, έχουμε προσθέσει υποστήριξης για μια δυνατότητα ρύθμισης χρονικού ορίου αδράνειας. Τώρα μπορείτε να τη ρυθμίσετε μια διάρκεια 4 έως 30 λεπτά.

TCP ενεργών λειτουργεί καλά για σενάρια όπου μπαταριών ζωής δεν είναι περιορισμού. Δεν συνιστάται για εφαρμογές για κινητές συσκευές. Χρησιμοποιώντας ένα ενεργών σε μια κινητή εφαρμογή TCP εξαντλούνται τη συσκευή μπαταριών γρηγορότερα.

![Χρονικό όριο TCP](./media/load-balancer-tcp-idle-timeout/image1.png)

Οι παρακάτω ενότητες περιγράφουν τον τρόπο για να αλλάξετε τις ρυθμίσεις χρονικού ορίου αδράνειας σε εικονικές μηχανές Windows και στο cloud services.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>Ρύθμιση παραμέτρων το χρονικό όριο TCP για δημόσια IP σας επιπέδου παρουσίας σε 15 λεπτά

    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes`είναι προαιρετικό. Εάν δεν έχει ρυθμιστεί, το προεπιλεγμένο χρονικό όριο είναι 4 λεπτά. Η περιοχή αποδεκτή χρονικού ορίου είναι 4 έως 30 λεπτά.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Ορίστε το χρονικό όριο αδράνειας κατά τη δημιουργία ενός ορίου Azure σε μια εικονική μηχανή

Για να αλλάξετε τη ρύθμιση χρονικού ορίου για ένα τελικό σημείο, χρησιμοποιήστε τα εξής:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

Για να ανακτήσετε της ρύθμισης παραμέτρων του χρονικού ορίου αδράνειας, χρησιμοποιήστε την ακόλουθη εντολή:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
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

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>Ορίστε το χρονικό όριο TCP σε ένα σύνολο εξισορρόπηση φόρτου τελικού σημείου

Εάν τελικά σημεία αποτελούν μέρος ενός συνόλου εξισορρόπηση φόρτου τελικό σημείο, το χρονικό όριο TCP πρέπει να οριστούν σε το σύνολο εξισορρόπηση φόρτου τελικού σημείου. Για παράδειγμα:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## <a name="change-timeout-settings-for-cloud-services"></a>Αλλαγή ρυθμίσεων χρονικού ορίου για τις υπηρεσίες cloud

Μπορείτε να χρησιμοποιήσετε το Azure SDK για να ενημερώσετε την υπηρεσία cloud. Μπορείτε να κάνετε ρυθμίσεις τελικό σημείο για τις υπηρεσίες cloud στο αρχείο .csdef. Ενημέρωση το χρονικό όριο TCP για ανάπτυξη από μια υπηρεσία cloud απαιτεί μια αναβάθμιση ανάπτυξης. Εξαίρεση είναι εάν το χρονικό όριο TCP καθορίζεται μόνο για μια δημόσια διεύθυνση IP. Δημόσια ρυθμίσεις IP που υπάρχουν στο αρχείο .cscfg και μπορείτε να ενημερώσετε τους μέσω update ανάπτυξης και της αναβάθμισης.

Οι αλλαγές .csdef για τις ρυθμίσεις του τελικού σημείου είναι:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

Οι αλλαγές .cscfg για τη ρύθμιση χρονικού ορίου σε δημόσια IP είναι:

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

## <a name="rest-api-example"></a>Παράδειγμα REST API

Μπορείτε να ρυθμίσετε το χρονικό όριο αδράνειας TCP, χρησιμοποιώντας το API διαχείρισης υπηρεσίας. Βεβαιωθείτε ότι το `x-ms-version` κεφαλίδα έχει οριστεί σε έκδοση `2014-06-01` ή νεότερη έκδοση. Ενημερώστε τη ρύθμιση παραμέτρων για το καθορισμένο εξισορρόπηση φόρτου εισαγωγής τελικά σημεία σε όλες οι εικονικές μηχανές σε μια ανάπτυξη του.

### <a name="request"></a>Αίτηση

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Απόκριση

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
        <LocalPort>local-port-number</LocalPort>
        <Port>external-port-number</Port>
        <LoadBalancerProbe>
          <Path>path-of-probe</Path>
          <Port>port-assigned-to-probe</Port>
          <Protocol>probe-protocol</Protocol>
          <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
          <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
        </LoadBalancerProbe>
        <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
        <Protocol>endpoint-protocol</Protocol>
        <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
        <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
        <EndpointACL>
          <Rules>
            <Rule>
              <Order>priority-of-the-rule</Order>
              <Action>permit-rule</Action>
              <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
              <Description>description-of-the-rule</Description>
            </Rule>
          </Rules>
        </EndpointACL>
      </InputEndpoint>
    </LoadBalancedEndpointList>

## <a name="next-steps"></a>Επόμενα βήματα

[Επισκόπηση εξισορρόπησης εσωτερικό φόρτωσης](load-balancer-internal-overview.md)

[Γρήγορα αποτελέσματα με τη ρύθμιση των παραμέτρων ενός εξισορρόπησης φόρτου μέσω Internet](load-balancer-get-started-internet-arm-ps.md)

[Ρύθμιση παραμέτρων μιας λειτουργίας διανομής εξισορρόπησης φόρτου](load-balancer-distribution-mode.md)
